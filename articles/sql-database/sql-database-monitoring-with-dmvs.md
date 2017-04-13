<properties
   pageTitle="Sledování databáze Azure SQL pomocí Správa dynamických zobrazení | Microsoft Azure"
   description="Informace o zjišťování a Diagnostika běžné problémy s výkonem pomocí Správa dynamických zobrazení sledování databázi Microsoft Azure SQL."
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/20/2016"
   ms.author="carlrab"/>

# <a name="monitoring-azure-sql-database-using-dynamic-management-views"></a>Sledování databáze SQL Azure pomocí Správa dynamických zobrazení

Databázi Microsoft Azure SQL umožňuje podmnožinu Správa dynamických zobrazení pro diagnostiku problémy s výkonem, které můžou být způsobená blokovaných nebo dlouho probíhajících dotazů, kritické zdroje body, plány špatné dotazů a tak dále. Toto téma obsahuje informace o tom, jak zjistit běžné problémy s výkonem pomocí Správa dynamických zobrazení.

Databáze SQL částečně podporuje tři kategorie Správa dynamických zobrazení:

- Správa dynamických vztahující se k databázi zobrazení.
- Zobrazení souvisejících s spuštění dynamické správy.
- Zobrazení souvisejících s transakce dynamické správy.

Podrobné informace o zobrazeních dynamické správy najdete v článku [Dynamická správa zobrazení a funkcí (Transact-SQL)](https://msdn.microsoft.com/library/ms188754.aspx) v SQL Server knihy Online.

## <a name="permissions"></a>Oprávnění

V SQL databázi dotazování Správa dynamických zobrazení vyžaduje oprávnění **Stav databáze zobrazení** . Oprávnění k **Zobrazení stav databáze** vrátí informace o všech objektů v aktuální databázi.
Pokud chcete udělit oprávnění **Stav databáze zobrazit** konkrétní databázi uživateli, spusťte následující dotaz:

```GRANT VIEW DATABASE STATE TO database_user; ```

Správa dynamických zobrazení v instance serveru SQL Server, místní vrátí informace o stavu serveru. V SQL databázi vracejí informace týkající se jenom v aktuální databázi logické.

## <a name="calculating-database-size"></a>Výpočet velikost databáze

Následující dotaz vrátí velikost databáze (v megabajtech):

```
-- Calculates the size of the database.
SELECT SUM(reserved_page_count)*8.0/1024
FROM sys.dm_db_partition_stats;
GO
```

Následující dotaz vrátí velikost jednotlivé objekty (MB) v databázi:

```
-- Calculates the size of individual database objects.
SELECT sys.objects.name, SUM(reserved_page_count) * 8.0 / 1024
FROM sys.dm_db_partition_stats, sys.objects
WHERE sys.dm_db_partition_stats.object_id = sys.objects.object_id
GROUP BY sys.objects.name;
GO
```

## <a name="monitoring-connections"></a>Sledování připojení

Získat informace o připojení k serveru konkrétní databáze SQL Azure a podrobnosti každé připojení můžete použít zobrazení [sys.dm_exec_connections](https://msdn.microsoft.com/library/ms181509.aspx) . Kromě toho [sys.dm_exec_sessions](https://msdn.microsoft.com/library/ms176013.aspx) zobrazení je užitečný načtení informací o všech aktivních uživatelů připojení a interní úkoly.
Následující dotaz načte informace o aktuální připojení:

```
SELECT
    c.session_id, c.net_transport, c.encrypt_option,
    c.auth_scheme, s.host_name, s.program_name,
    s.client_interface_name, s.login_name, s.nt_domain,
    s.nt_user_name, s.original_login_name, c.connect_time,
    s.login_time
FROM sys.dm_exec_connections AS c
JOIN sys.dm_exec_sessions AS s
    ON c.session_id = s.session_id
WHERE c.session_id = @@SPID;
```

> [AZURE.NOTE] Při provádění **sys.dm_exec_requests** a **sys.dm_exec_sessions zobrazení**, pokud máte oprávnění **Stav databáze zobrazení** v databázi, uvidíte všechny provádění relací v databázi. v ostatních případech zobrazí pouze pro aktuální relaci.

## <a name="monitoring-query-performance"></a>Sledování výkonu dotazu

Pomalé nebo dlouho spouštění dotazů můžete používat významné systémové prostředky. V této části demonstruje použití Správa dynamických zobrazení zjišťování několik běžných problémů výkonu dotazu. Odkaz na starší, ale pořád užitečné pro odstraňování potíží, pak je toto článek [Odstraňování potíží s výkonem systému SQL Server 2008](http://download.microsoft.com/download/D/B/D/DBDE7972-1EB9-470A-BA18-58849DB3EB3B/TShootPerfProbs2008.docx) na Microsoft TechNet.

### <a name="finding-top-n-queries"></a>Hledání horních N dotazů

Následující příklad vrátí informace o nejčastější dotazy pět jde řadit podle Průměrná doba využití procesoru. V tomto příkladu sloučí dotazy podle jejich hash dotazu tak, aby logicky ekvivalentní dotazy jsou seskupená podle jejich spotřebu kumulativní zdroje.

```
SELECT TOP 5 query_stats.query_hash AS "Query Hash",
    SUM(query_stats.total_worker_time) / SUM(query_stats.execution_count) AS "Avg CPU Time",
    MIN(query_stats.statement_text) AS "Statement Text"
FROM
    (SELECT QS.*,
    SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
    ((CASE statement_end_offset
        WHEN -1 THEN DATALENGTH(ST.text)
        ELSE QS.statement_end_offset END
            - QS.statement_start_offset)/2) + 1) AS statement_text
     FROM sys.dm_exec_query_stats AS QS
     CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
GROUP BY query_stats.query_hash
ORDER BY 2 DESC;
```

### <a name="monitoring-blocked-queries"></a>Sledování blokovaných dotazů

Pomalé nebo dlouho probíhajících dotazů můžete přispět k využití prostředků nadbytečné a být důsledek blokovaných dotazů. Příčiny blokování může být návrh špatné aplikace, plány chybný dotaz, chybějící užitečné indexy atd. Zobrazení sys.dm_tran_locks slouží k získání informací o uzamčení ukončíte v databázi SQL Azure. Například kód, přečtěte si téma [sys.dm_tran_locks jazyce Transact-SQL ()](https://msdn.microsoft.com/library/ms190345.aspx) v SQL Server knihy Online.

### <a name="monitoring-query-plans"></a>Sledování plány dotazů

Plán neefektivní dotazu může taky zvýšit procesoru spotřebu. Zobrazení [sys.dm_exec_query_stats](https://msdn.microsoft.com/library/ms189741.aspx) zjistit, dotazu, který používá nejčastěji kumulativní procesoru v následujícím příkladu.

```
SELECT
    highest_cpu_queries.plan_handle,
    highest_cpu_queries.total_worker_time,
    q.dbid,
    q.objectid,
    q.number,
    q.encrypted,
    q.[text]
FROM
    (SELECT TOP 50
        qs.plan_handle,
        qs.total_worker_time
    FROM
        sys.dm_exec_query_stats qs
    ORDER BY qs.total_worker_time desc) AS highest_cpu_queries
    CROSS APPLY sys.dm_exec_sql_text(plan_handle) AS q
ORDER BY highest_cpu_queries.total_worker_time DESC;
```

## <a name="see-also"></a>Viz taky

[Úvod k SQL databázi](sql-database-technical-overview.md)
