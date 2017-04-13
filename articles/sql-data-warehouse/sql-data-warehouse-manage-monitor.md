<properties
   pageTitle="Sledovat vaše pracovní zátěž pomocí DMVs | Microsoft Azure"
   description="Zjistěte, jak sledovat vaše pracovní zátěž pomocí DMVs."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/08/2016"
   ms.author="sonyama;barbkess"/>

# <a name="monitor-your-workload-using-dmvs"></a>Sledovat vaše pracovní zátěž pomocí DMVs

Tento článek popisuje, jak dynamická správa zobrazení (DMVs) umožňuje sledovat svého pracovního vytížení a zjistěte spuštění dotazu v SQL Azure datový sklad.

## <a name="permissions"></a>Oprávnění

K vytvoření dotazu DMVs v tomto článku, musíte stavu zobrazení databáze nebo ovládacího PRVKU oprávnění. Udělení stav databáze zobrazení je obvykle upřednostňované oprávnění je to mnohem víc restriktivní.

```sql
GRANT VIEW DATABASE STATE TO myuser;
```

## <a name="monitor-connections"></a>Sledování připojení

Všechny přihlášení k SQL datový sklad přihlášeni k [sys.dm_pdw_exec_sessions][].  Tento DMV obsahuje poslední 10 000 přihlášení.  Session_id je primární klíč a postupně přiřazen každé nové přihlášení.

```sql
-- Other Active Connections
SELECT * FROM sys.dm_pdw_exec_sessions where status <> 'Closed' and session_id <> session_id();
```

## <a name="monitor-query-execution"></a>Spuštění dotazu monitoru

Všechny dotazy na SQL datový sklad přihlášeni k [sys.dm_pdw_exec_requests][].  Tento DMV obsahuje poslední 10 000 dotazy spouštět.  Request_id jednoznačně identifikuje každý dotaz a je primární klíč pro tento DMV.  Request_id přiřazena postupně pro každý nový dotaz a předponou QID, Today ID dotazu.  Dotazování tento DMV pro dané session_id zobrazují všechny dotazy pro dané přihlášení.

>[AZURE.NOTE] Uložené procedury použít více požádat o ID.  Žádost o ID, mají přiřazenou v pořadí. 

Tady jsou kroky k prošetřit plány spuštění dotazu a času u určitého dotazu.

### <a name="step-1-identify-the-query-you-wish-to-investigate"></a>Krok 1: Určení dotaz, který chcete prozkoumat

```sql
-- Monitor active queries
SELECT * 
FROM sys.dm_pdw_exec_requests 
WHERE status not in ('Completed','Failed','Cancelled')
  AND session_id <> session_id()
ORDER BY submit_time DESC;

-- Find top 10 queries longest running queries
SELECT TOP 10 * 
FROM sys.dm_pdw_exec_requests 
ORDER BY total_elapsed_time DESC;

-- Find a query with the Label 'My Query'
-- Use brackets when querying the label column, as it it a key word
SELECT  *
FROM    sys.dm_pdw_exec_requests
WHERE   [label] = 'My Query';
```

Ve výsledcích předchozí dotazu **poznámku ID požádat o** dotaz, který chcete prozkoumat.

Dotazy ve stavu **Suspended** jsou právě ve frontě z důvodu omezení souběžné. Tyto dotazy se zobrazí taky v dotazu čeká sys.dm_pdw_waits s typem UserConcurrencyResourceType. Další informace o limitech souběžné naleznete v tématu [Správa souběžné a pracovní zátěž][] . Dotazy můžete taky počkejte další důvody jako objekt zámky.  Pokud váš dotaz je čeká se na zdroj, najdete v článku [Investigating dotazů čeká se na materiály][] další dolů v tomto článku.

Pro zjednodušení vyhledávacího dotazu v tabulce sys.dm_pdw_exec_requests, umožňuje [Popisek][] přiřadit komentáře do dotazu, který může být vyhledané v zobrazení sys.dm_pdw_exec_requests.

```sql
-- Query with Label
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query')
;
```

### <a name="step-2-investigate-the-query-plan"></a>Krok 2: Prozkoumejte plán dotazů

Načtení dotazu distribuované SQL (DSQL) plán z [sys.dm_pdw_request_steps][]pomocí ID požádat o.

```sql
-- Find the distributed query plan steps for a specific query.
-- Replace request_id with value from Step 1.

SELECT * FROM sys.dm_pdw_request_steps
WHERE request_id = 'QID####'
ORDER BY step_index;
```

Když plán DSQL trvá déle než obvykle, příčinou může být složité plán s několika krocích DSQL nebo jenom postupně po jednotlivých krocích trvá moc dlouho.  Pokud je plán několika krocích s několika přesunout operací, můžete zkusit optimalizovat tabulky distribuce zmenšit přesun dat. [Distribuce tabulky][] článek vysvětluje, proč dat je přesunout vyřešit dotazu a vysvětluje několik strategie pro distribuci minimalizovat přesun dat.

Chcete prozkoumat další podrobnosti o krokovat sloupci *operation_type* krok dotazu dlouho probíhajících a poznamenejte si **Krok indexu**:

- Pokračujte krokem 3a pro **operace SQL**: OnOperation, RemoteOperation, ReturnOperation.
- Pokračujte krokem 3b pro **Přesun dat operace**: ShuffleMoveOperation BroadcastMoveOperation, TrimMoveOperation, PartitionMoveOperation, MoveOperation, CopyOperation.

### <a name="step-3a-investigate-sql-on-the-distributed-databases"></a>KROK 3a: prozkoumejte SQL na distribuované databází

Umožňuje požádat o ID a Index krok načtení podrobností ze [sys.dm_pdw_sql_requests][], který obsahuje informace spuštění kroku dotazu ve všech distribuované databází.

```sql
-- Find the distribution run times for a SQL step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_sql_requests
WHERE request_id = 'QID####' AND step_index = 2;
```

Při spuštění krok dotazu [DBCC PDW_SHOWEXECUTIONPLAN][] mohou sloužit k načítání odhadovaná plán SQL Server z mezipaměti plán SQL Server kroku spuštěných pro konkrétní rozdělení.

```sql
-- Find the SQL Server execution plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(1, 78);
```

### <a name="step-3b-investigate-data-movement-on-the-distributed-databases"></a>Krok 3b: prozkoumejte přesun dat na distribuované databází

Získat informace o kroku pohyb data spuštěných pro každé rozdělení z [sys.dm_pdw_dms_workers][]pomocí ID požádat o a Index kroku.

```sql
-- Find the information about all the workers completing a Data Movement Step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_dms_workers
WHERE request_id = 'QID####' AND step_index = 2;
```

- Zaškrtněte políčko ve sloupci *total_elapsed_time* zobrazíte, když se konkrétní rozdělení trvá výrazně delší než ostatní pro přesun dat.
- K distribuci dlouho probíhajících zaškrtněte políčko ve sloupci *rows_processed* zobrazíte, pokud je počet řádků, která je přesouvána z této rozdělení podstatně vyšší než ostatní. Pokud ano, může to znamenat skew podkladová data.

Pokud dotaz je spuštěn, [DBCC PDW_SHOWEXECUTIONPLAN][] mohou sloužit k načítání odhadovaná plán SQL Server z mezipaměti plán SQL Server aktuálně spuštěných kroku SQL v rámci konkrétní rozdělení.

```sql
-- Find the SQL Server estimated plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(55, 238);
```

<a name="waiting"></a>
## <a name="monitor-waiting-queries"></a>Sledování čekání dotazů

Pokud zjistíte, že váš dotaz není díky průběh vzhledem k tomu, čeká se na zdroj, tady je dotaz, který se zobrazuje všechny zdroje že čeká dotazu.

```sql
-- Find queries 
-- Replace request_id with value from Step 1.

SELECT waits.session_id,
      waits.request_id,  
      requests.command,
      requests.status,
      requests.start_time,  
      waits.type,
      waits.state,
      waits.object_type,
      waits.object_name
FROM   sys.dm_pdw_waits waits
   JOIN  sys.dm_pdw_exec_requests requests
   ON waits.request_id=requests.request_id
WHERE waits.request_id = 'QID####'
ORDER BY waits.object_name, waits.object_type, waits.state;
```

Pokud dotaz aktivně čeká na zdroje z jiného dotazu, stav budou **AcquireResources**.  Pokud dotaz obsahuje všechny požadované prostředky, stav budou **udělit**.

## <a name="next-steps"></a>Další kroky
Další informace o DMVs naleznete v tématu [zobrazení systému][] .
Další informace o doporučených postupech najdete [SQL datový sklad doporučené postupy][]

<!--Image references-->

<!--Article references-->
[Manage overview]: ./sql-data-warehouse-overview-manage.md
[Doporučené postupy SQL datový sklad]: ./sql-data-warehouse-best-practices.md
[Systémová zobrazení]: ./sql-data-warehouse-reference-tsql-system-views.md
[Distribuce tabulky]: ./sql-data-warehouse-tables-distribute.md
[Správa souběžné a zátěží na projektu]: ./sql-data-warehouse-develop-concurrency.md
[Zkoumání dotazů čeká se na zdroje]: ./sql-data-warehouse-manage-monitor.md#waiting

<!--MSDN references-->
[Sys.dm_pdw_dms_workers]: http://msdn.microsoft.com/library/mt203878.aspx
[Sys.dm_pdw_exec_requests]: http://msdn.microsoft.com/library/mt203887.aspx
[Sys.dm_pdw_exec_sessions]: http://msdn.microsoft.com/library/mt203883.aspx
[Sys.dm_pdw_request_steps]: http://msdn.microsoft.com/library/mt203913.aspx
[Sys.dm_pdw_sql_requests]: http://msdn.microsoft.com/library/mt203889.aspx
[DBCC PDW_SHOWEXECUTIONPLAN]: http://msdn.microsoft.com/library/mt204017.aspx
[DBCC PDW_SHOWSPACEUSED]: http://msdn.microsoft.com/library/mt204028.aspx
[POPISEK]: https://msdn.microsoft.com/library/ms190322.aspx
