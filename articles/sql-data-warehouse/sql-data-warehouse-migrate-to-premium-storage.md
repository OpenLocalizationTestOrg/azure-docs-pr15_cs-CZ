<properties
   pageTitle="Migrace stávajícího datový sklad SQL Azure k základnímu úložišti premium | Microsoft Azure"
   description="Pokyny pro migraci stávajícího SQL datový sklad k základnímu úložišti premium"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="happynicolle"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/24/2016"
   ms.author="nicw;barbkess;sonyama"/>

# <a name="migration-to-premium-storage-details"></a>Přechod na podrobnosti úložiště Premium
SQL datový sklad naposledy zavádí [Premium úložiště pro větší předvídatelnost výkonu][].  Teď připraveni migrovat stávající Data sklady aktuálně na standardní úložiště k základnímu úložišti Premium.  Přečtěte si další informace o způsob a četnost automatické migrace provést a jak vlastním migrovat, pokud chcete určit, kdy dojde k prostoje.

Pokud máte víc než jeden datový sklad, můžete [Naplánovat automatické migrace][] dole určit, kdy bude taky migrovat.

## <a name="determine-storage-type"></a>Určení typu úložiště
Pokud jste vytvořili DW před datem dole, už používáte standardní úložiště.  Každý datový sklad standardní úložný prostor, který je vyměřené poplatky za jeho automatické migrace má upozornění, že v horní části datový sklad zásuvné [Portál Azure][] , že "*nadcházející upgrade k základnímu úložišti premium budou vyžadovat výpadku.  Přečtěte si další ->*. "

| **Oblast**          | **DW vytvořené před tímto datem**   |
| :------------------ | :-------------------------------- |
| Austrálie východ      | Úložiště Premium dosud není k dispozici |
| Austrálie jihovýchodní | 5 srpen 2016                    |
| Brazílie jih        | 5 srpen 2016                    |
| Centrální Kanada      | 25 květen 2016                      |
| Kanada východ         | 26 květen 2016                      |
| Centrální USA          | 26 květen 2016                      |
| Čína východ          | Úložiště Premium dosud není k dispozici |
| Severní Číně         | Úložiště Premium dosud není k dispozici |
| Východní Asie           | 25 května 2016                      |
| Východní USA             | 26 května 2016                      |
| Východní US2            | 27 květen 2016                      |
| Indie centrální       | 27 květen 2016                      |
| Indie jih         | 26 květen 2016                      |
| Indie západní          | Úložiště Premium dosud není k dispozici |
| Japonsko východ          | 5 srpen 2016                    |
| Japonsko západní          | Úložiště Premium dosud není k dispozici |
| Severní centrální USA    | Úložiště Premium dosud není k dispozici |
| Severní Evropě        | 5 srpen 2016                    |
| Jižní centrální USA    | 27 květen 2016                      |
| Jihovýchodní Asie      | 24 května 2016                      |
| Západní Evropě         | 25 květen 2016                      |
| Západní centrální USA     | 26 srpen 2016                   |
| Západ USA             | 26 květen 2016                      |
| Západní US2            | 26 srpen 2016                   |

## <a name="automatic-migration-details"></a>Podrobnosti o automatických migrace
Ve výchozím nastavení jsme bude migrovat databázi můžete během 18: 00 a 6: 00 ve vaší oblasti místní čas průběhu [plánu Automatická migrace][] níže.  Během migrace budou nelze použít existující datový sklad.  Jsme odhad, že migrace bude trvat zhruba hodinu za TB úložiště na datový sklad.  Jsme také zajistí, že nejsou Účtovaná během libovolnou část Automatická migrace.

> [AZURE.NOTE] Nebudete moct používat existující datový sklad během migrace.  Po dokončení migrace se budou Data Warehouse návrat do online režimu.

Detaily níže jsou uvedeny kroky, aby Microsoft trvá vaším jménem k dokončení migrace a nevyžaduje všechny účast na druhé straně.  V tomto příkladu si představte, že existující DW na standardní úložiště je aktuálně s názvem "MyDW."

1.  Microsoft přejmenuje "MyDW" k "MyDW_DO_NOT_USE_ [časové razítko]"
2.  Microsoft pozastaví "MyDW_DO_NOT_USE_ [časové razítko]."  Během této doby, se považuje zálohy.  Pokud nám dojde k potížím během tohoto procesu, může se zobrazit více pozastavit a životopisů.
3.  Microsoft vytvoří nový DW s názvem "MyDW" Premium úložný prostor ze zálohy pořízené v kroku 2.  "MyDW" nezobrazí až po dokončení obnovení.
4.  Po dokončení obnovení "MyDW" vrátí stejný DWUs a pozastaveného nebo aktivního stavu, ve kterém byl před migrací.
5.  Po dokončení migrace Microsoft odstraní "MyDW_DO_NOT_USE_ [časové razítko]"
    
> [AZURE.NOTE] Během migrace se nepřenesou tato nastavení:
> 
>   -  Sestavy auditování na úrovni databáze je potřeba jej znovu povolit.
>   -  Pravidla brány firewall na úrovni **databáze** muset být přidáno.  Nebudou mít vliv na pravidla brány firewall na úrovni **serveru** .

### <a name="automatic-migration-schedule"></a>Naplánování automatické migrace
Automatické migrací nastat z 18: 00 – 6: 00 (místní čas na oblasti) při následující naplánování výpadku.

| **Oblast**          | **Odhadovaná počáteční datum**     | **Pole Předpokládaná koncové datum**       |
| :------------------ | :--------------------------- | :--------------------------- |
| Austrálie východ      | Ještě není určeno           | Ještě není určeno           |
| Austrálie jihovýchodní | 10 srpen 2016              | 24 srpen 2016              |
| Brazílie jih        | 10 srpen 2016              | 24 srpen 2016              |
| Centrální Kanada      | 23 června 2016                | 1. července 2016                 |
| Kanada východ         | 23 června 2016                | 1. července 2016                 |
| Centrální USA          | 23 června 2016                | 4 červenec 2016                 |
| Čína východ          | Ještě není určeno           | Ještě není určeno           |
| Severní Číně         | Ještě není určeno           | Ještě není určeno           |
| Východní Asie           | 23 června 2016                | 1. července 2016                 |
| Východní USA             | 23 června 2016                | 11 červenec 2016                |
| Východní US2            | 23 června 2016                | 8 červenec 2016                 |
| Indie centrální       | 23 června 2016                | 1. července 2016                 |
| Indie jih         | 23 června 2016                | 1. července 2016                 |
| Indie západní          | Ještě není určeno           | Ještě není určeno           |
| Japonsko východ          | 10 srpen 2016              | 24 srpen 2016              |
| Japonsko západní          | Ještě není určeno           | Ještě není určeno           |
| Severní centrální USA    | Ještě není určeno           | Ještě není určeno           |
| Severní Evropě        | 10 srpen 2016              | 31. srpna 2016              |
| Jižní centrální USA    | 23 června 2016                | 2 červenec 2016                 |
| Jihovýchodní Asie      | 23 června 2016                | 1. července 2016                 |
| Západní Evropě         | 23 června 2016                | 8 červenec 2016                 |
| Západní centrální USA     | 14 srpen 2016              | 31. srpna 2016              |
| Západ USA             | 23 června 2016                | 7 červenec 2016                 |
| Západní US2            | 14 srpen 2016              | 31. srpna 2016              |

## <a name="self-migration-to-premium-storage"></a>Vlastní migrace k základnímu úložišti Premium
Pokud chcete určit, kdy dojde k vaší výpadek služeb, můžete použít následující kroky migrace existující datový sklad standardní úložný prostor úložiště Premium.  Pokud zvolíte možnost vlastní migrovat, musíte nejdřív udělat vlastní migrace před zahájením automatické migrace v dané oblasti zamezení Automatická migrace příčinou konfliktu (odkaz na [plánu Automatická migrace][]).

### <a name="self-migration-instructions"></a>Pokyny k vlastním migrace
Pokud chcete určit vaší prostoje, můžete automatické nejdřív migrujete datový sklad pomocí zálohování a obnovení.  Očekává se, že obnovení část migrace trvat zhruba hodinu za TB úložiště na DW.  Pokud budete chtít použít stejný název po migraci, postupujte podle pokynů k [přejmenování v průběhu migrace][]. 

1.  [Pozastavit][] vaše DW, která trvá automatického zálohování
2.  [Obnovení][] od vaší poslední snímek
3.  Odstranění existující DW standardní úložný prostor. **Pokud neuděláte tento krok, který strhne příslušná obou DWs.**

> [AZURE.NOTE] Během migrace se nepřenesou tato nastavení:
> 
>   -  Sestavy auditování na úrovni databáze je potřeba jej znovu povolit.
>   -  Pravidla brány firewall na úrovni **databáze** muset být přidáno.  Nebudou mít vliv na pravidla brány firewall na úrovni **serveru** .

#### <a name="optional-steps-to-rename-during-migration"></a>Volitelné: Postup přejmenování v průběhu migrace 
Dvě databáze na stejném logické serveru nemůžou mít stejný název. SQL datový sklad nyní podporuje možnost Přejmenovat DW.

V tomto příkladu si představte, že existující DW na standardní úložiště je aktuálně s názvem "MyDW."

1.  Přejmenování "MyDW" pomocí příkazu Vlastnosti databáze, který následuje na něco jako "MyDW_BeforeMigration."  Tento příkaz ukončuje všechny existující transakce a musí mít v hlavní databázi úspěšné.
```
ALTER DATABASE CurrentDatabasename MODIFY NAME = NewDatabaseName;
```
2.  [Pozastavit][] "MyDW_BeforeMigration", která trvá automatického zálohování
3.  [Obnovení][] od vaší poslední snímek nové databáze s názvem používaný k (ex: "MyDW")
4.  Odstranění "MyDW_BeforeMigration".  **Pokud neuděláte tento krok, který strhne příslušná obou DWs.**

> [AZURE.NOTE] Během migrace se nepřenesou tato nastavení:
> 
>   -  Sestavy auditování na úrovni databáze je potřeba jej znovu povolit.
>   -  Pravidla brány firewall na úrovni **databáze** muset být přidáno.  Nebudou mít vliv na pravidla brány firewall na úrovni **serveru** .

## <a name="next-steps"></a>Další kroky
Změna k základnímu úložišti Premium jsme mít taky zvyšuje počet databázových objektů blob souborů v podkladovém architektura datový sklad.  Maximalizace výkonu výhody tuto změnu, doporučujeme vám skupinový Columnstore indexů pomocí následující skript znovu vytvořit.  Skript dole funguje tak, že vynucení některé z existujících dat do další objekty BLOB.  Pokud žádná akce data se mají přirozený znovu distribuovat časem při načítání dalších dat do datový sklad tabulek.

**Předpoklady:**

1.  Datový sklad by měla běžet s 1 000 DWUs nebo novější (viz [Měřítko výpočetního výkonu][])
2.  Uživatel skriptu by měl být v [mediumrc role][] nebo vyšší
    1.  Přidání uživatele do této role, proveďte následující: 
        1.  ````EXEC sp_addrolemember 'xlargerc', 'MyUser'````

````sql
-------------------------------------------------------------------------------
-- Step 1: Create Table to control Index Rebuild
-- Run as user in mediumrc or higher
--------------------------------------------------------------------------------
create table sql_statements
WITH (distribution = round_robin)
as select 
    'alter index all on ' + s.name + '.' + t.NAME + ' rebuild;' as statement,
    row_number() over (order by s.name, t.name) as sequence
from 
    sys.schemas s
    inner join sys.tables t
        on s.schema_id = t.schema_id
where
    is_external = 0
;
go
 
--------------------------------------------------------------------------------
-- Step 2: Execute Index Rebuilds.  If script fails, the below can be rerun to restart where last left off
-- Run as user in mediumrc or higher
--------------------------------------------------------------------------------

declare @nbr_statements int = (select count(*) from sql_statements)
declare @i int = 1
while(@i <= @nbr_statements)
begin
      declare @statement nvarchar(1000)= (select statement from sql_statements where sequence = @i)
      print cast(getdate() as nvarchar(1000)) + ' Executing... ' + @statement
      exec (@statement)
      delete from sql_statements where sequence = @i
      set @i += 1
end;
go
-------------------------------------------------------------------------------
-- Step 3: Cleanup Table Created in Step 1
--------------------------------------------------------------------------------
drop table sql_statements;
go
````

Pokud dojde k potížím s datový sklad, [požadavek podpory můžete vytvořit][] a odkaz "Migrace do úložiště Premium" jako možných příčin.

<!--Image references-->

<!--Article references-->
[Naplánování automatické migrace]: #automatic-migration-schedule
[self-migration to Premium Storage]: #self-migration-to-premium-storage
[Vytvoření požadavek podpory můžete]: sql-data-warehouse-get-started-create-support-ticket.md
[Azure paired region]: best-practices-availability-paired-regions.md
[main documentation site]: services/sql-data-warehouse.md
[Pozastavit]: sql-data-warehouse-manage-compute-portal.md/#pause-compute
[Obnovení]: sql-data-warehouse-restore-database-portal.md
[přejmenování v průběhu migrace]: #optional-steps-to-rename-during-migration
[Měřítko výpočetního výkonu]: sql-data-warehouse-manage-compute-portal/#scale-compute-power
[mediumrc role]: sql-data-warehouse-develop-concurrency/#workload-management

<!--MSDN references-->


<!--Other Web references-->
[Premium úložiště pro větší předvídatelnost výkonu]: https://azure.microsoft.com/en-us/blog/azure-sql-data-warehouse-introduces-premium-storage-for-greater-performance/
[Azure portálu]: https://portal.azure.com
