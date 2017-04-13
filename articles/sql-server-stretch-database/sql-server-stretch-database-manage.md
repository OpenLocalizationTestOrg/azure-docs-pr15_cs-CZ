<properties
    pageTitle="Správa a řešení potíží s roztáhnout databáze | Microsoft Azure"
    description="Naučte se spravovat a řešení potíží s roztáhnout databáze."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="douglasl"/>

# <a name="manage-and-troubleshoot-stretch-database"></a>Správa a řešení potíží s roztáhnout databáze

Správa a odstraňování problémů s databáze roztáhnout, použijte nástroje a metod popsaných v tomto tématu.

## <a name="manage-local-data"></a>Správa místních dat

### <a name="LocalInfo"></a>Získat informace o místní databází a tabulek povolených pro databázi roztáhnout
Otevřete zobrazení katalogu **sys.databases** a **sys.tables** zobrazíte informace o roztáhnout\-povolené databáze systému SQL Server a tabulek. Další informace najdete v tématu [sys.databases (Transact-SQL)](https://msdn.microsoft.com/library/ms178534.aspx) a [sys.tables jazyce Transact-SQL ()](https://msdn.microsoft.com/library/ms187406.aspx).

K zjištění dostupného prostoru roztáhnout\-povolené tabulky používá na serveru SQL Server, spusťte následující příkaz.

 ```tsql
USE <Stretch-enabled database name>;
GO
EXEC sp_spaceused '<Stretch-enabled table name>', 'true', 'LOCAL_ONLY';
GO
 ```
## <a name="manage-data-migration"></a>Správa migraci dat

### <a name="check-the-filter-function-applied-to-a-table"></a>Kontrola funkci Filtr použitý na tabulku
Otevřete zobrazení katalogu **sys.remote\_dat\_archivu\_tabulek** a zkontrolujte hodnotu **Filtr\_predikát** sloupec k určení funkci, která roztáhnout databázi používá vyberte řádky, které chcete migrovat. Pokud je argument hodnota null, je oprávněni k poštovním celou tabulku. Další informace najdete v tématu [sys.remote_data_archive_tables (Transact-SQL)](https://msdn.microsoft.com/library/dn935003.aspx) a [Vyberte řádky pomocí funkce filter migrovat](sql-server-stretch-database-predicate-function.md).

### <a name="Migration"></a>Kontrola stavu migraci dat
Vyberte úkoly **| Roztáhnout | Sledování** pro databáze systému SQL Server Management Studio sledování migraci dat v nástroji Sledování roztáhnout databáze. Další informace najdete v tématu [sledování a odstraňování problémů s migrací dat (roztáhnout databáze)](sql-server-stretch-database-monitor.md).

Nebo otevřete zobrazení dynamická správa **sys.dm\_db\_rda\_migrace\_stav** zobrazíte, kolik listy a řádků dat migraci.

### <a name="Firewall"></a>Poradce při potížích s migrací dat
Poradce při potížích s návrhy, najdete v článku [sledování a odstraňování problémů s migrací dat (roztáhnout databáze)](sql-server-stretch-database-monitor.md).

## <a name="manage-remote-data"></a>Správa Vzdálená data

### <a name="RemoteInfo"></a>Získat informace o vzdálená databáze a tabulek použitých roztáhnout databáze
Otevření zobrazení katalogu **sys.remote\_dat\_archivu\_databází** a **sys.remote\_dat\_archivu\_tabulky** zobrazíte informace o vzdálená databáze a tabulky, ve kterých je uložen migrovaná data. Další informace najdete v tématu [sys.remote_data_archive_databases (Transact-SQL)](https://msdn.microsoft.com/library/dn934995.aspx) a [sys.remote_data_archive_tables jazyce Transact-SQL ()](https://msdn.microsoft.com/library/dn935003.aspx).

K zjištění dostupného prostoru tabulku roztáhnout s podporou používá v Azure, spusťte následující příkaz.

 ```tsql
USE <Stretch-enabled database name>;
GO
EXEC sp_spaceused '<Stretch-enabled table name>', 'true', 'REMOTE_ONLY';
GO
 ```

### <a name="delete-migrated-data"></a>Odstranění migrovaných dat  
Pokud chcete odstranit data, která již byla nemigruje do Azure, postupujte podle kroků popsaných v [sys.sp_rda_reconcile_batch](https://msdn.microsoft.com/library/mt707768.aspx).

## <a name="manage-table-schema"></a>Správa schématu tabulky

### <a name="dont-change-the-schema-of-the-remote-table"></a>Neměníte schématu vzdálené tabulky
Neměňte schématu vzdálené Azure tabulky, který máte přidružený k tabulce SQL serveru nakonfigurovány roztáhnout databáze. Zejména Neupravujte název nebo datový typ sloupce. Funkce databáze roztáhnout používání různých předpoklady o schématu vzdálené tabulky vzhledem k schématu tabulce SQL serveru. Pokud změna schématu Vzdálená databáze roztáhnout přestane fungovat pro tabulku se změnami.

### <a name="reconcile-table-columns"></a>Sloučení sloupců v tabulce  
Pokud jste odstranili omylem sloupců z vzdálené tabulky, spusťte **sp_rda_reconcile_columns** můžete přidat sloupce do vzdálené tabulky, která jsou v roztáhnout\-povolené tabulce SQL serveru, ale ne v tabulce vzdálené. Další informace najdete v tématu [sys.sp_rda_reconcile_columns](https://msdn.microsoft.com/library/mt707765.aspx).  

  > [!IMPORTANT] Když **sp_rda_reconcile_columns** znovu vytvoří sloupce, které jste omylem odstranili ze vzdáleného tabulky, neobnoví data, která byla dříve ve sloupci odstraněné.

**sp_rda_reconcile_columns** nedojde k odstranění sloupců z vzdálené tabulky, která jsou v tabulce vzdálené, ale ne v roztáhnout\-povolené tabulce SQL serveru. Pokud jsou sloupce v tabulce vzdálené Azure, které již v roztáhnout\-povolené SQL Server tabulky, tyto další sloupce nezabrání roztáhnout databáze provozní běžným způsobem. Další sloupce můžete volitelně odebrat ručně.  

## <a name="manage-performance-and-costs"></a>Správa výkon a náklady  

### <a name="troubleshoot-query-performance"></a>Řešení potíží s výkonem dotazu
Dotazy, které obsahují roztáhnout\-povolené tabulky očekává provádět pomaleji, než fungovaly před tabulky službu povolenou pro roztáhnout. Pokud výkonu dotazu snižuje výrazně, zkontrolujte následující možných problémech.

-   Je váš server Azure jinou zeměpisnou oblast než SQL Server? Konfigurace serveru Azure byli ve stejném zeměpisnou oblast jako SQL Server zmenšit latence sítě.

-   Stav sítě může mít sníženou kvalitu. Požádejte správce sítě informace o poslední problémy nebo výpadků.

### <a name="increase-azure-performance-level-for-resource-intensive-operations-such-as-indexing"></a>Zvýšení úrovně Azure výkonu prostředku\-náročné operací, jako je indexování
Při vytváření, znovu vytvořit nebo změnit uspořádání indexu u velkých tabulky nakonfigurovaný pro roztáhnout databáze a předpokládáte těžké dotazování migrované dat v Azure během této doby, zvažte zvýšit výkon úroveň odpovídající Vzdálená databáze Azure po celou dobu operace. Další informace o úrovních výkonu a ceny najdete v článku [SQL Server roztáhnout databáze cena za](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

### <a name="you-cant-pause-the-sql-server-stretch-database-service-on-azure"></a>Nelze pozastavit službu roztáhnout databáze systému SQL Server na Azure  
 Ujistěte se, vyberte příslušnou výkonu a ceny úroveň. Pokud zvýšit úroveň výkonu dočasně pro zdroj\-náročné operace v ní obnovit předchozí úrovni po dokončení operace. Další informace o úrovních výkonu a ceny najdete v článku [SQL Server roztáhnout databáze cena za](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).  

## <a name="change-the-scope-of-queries"></a>Změnit obor dotazu  
 Dotazy týkající se roztáhnout\-povolené tabulky vrátit místní a Vzdálená data ve výchozím nastavení. Můžete změnit rozsah dotazů pro všechny dotazy tak, že všichni uživatelé, nebo pouze pro jeden dotaz správcem.  

### <a name="change-the-scope-of-queries-for-all-queries-by-all-users"></a>Změnit obor dotazu pro všechny dotazy tak, že všichni uživatelé  
 Pokud chcete změnit rozsah všechny dotazy tak, že všichni uživatelé, spusťte uložená procedura **sys.sp_rda_set_query_mode**. Můžete omezit obor dotazu místních dat, zakázat všechny dotazy nebo obnovit výchozí nastavení. Další informace najdete v tématu [sys.sp_rda_set_query_mode](https://msdn.microsoft.com/library/mt703715.aspx).  

### <a name="queryHints"></a>Změna oboru dotazů jednoho dotazu správcem  
 Chcete-li změnit obor dotazu jednu členem db_owner role, přidejte * *s \( REMOTE_DATA_ARCHIVE_OVERRIDE = *hodnotu* \)** dotazu nápovědu k příkazu SELECT. Tip REMOTE_DATA_ARCHIVE_OVERRIDE dotazu může mít následující hodnoty.  
 -   **LOCAL_ONLY**. Dotaz místní data.  

 -   **REMOTE_ONLY**. Dotaz Vzdálená data.  

 -   **STAGE_ONLY**. Dotaz pouze data v tabulce místo, kam databáze roztáhnout fází řádky vhodný k migraci jenom a migrované řádků pro určité období po migraci. Tento dotaz tip je jediný způsob k vytvoření dotazu pracovní tabulky.  

Například následující dotaz vrátí místní výsledky.  

 ```tsql  
 USE <Stretch-enabled database name>;
 GO
 SELECT * FROM <Stretch_enabled table name> WITH (REMOTE_DATA_ARCHIVE_OVERRIDE = LOCAL_ONLY) WHERE ... ;
 GO
```  

## <a name="adminHints"></a>Aktualizace pro správce a odstraní  
 Ve výchozím nastavení nelze aktualizovat nebo odstranit řádky, které jsou vhodné k migraci nebo řádky, které již byly přesunuty, v roztáhnout\-povolené tabulky. Pokud máte problém vyřešit, členem db_owner role spouštět operace aktualizace nebo odstranění přidáním * *s \( REMOTE_DATA_ARCHIVE_OVERRIDE = *hodnotu* \)** dotazu nápovědu k příkazu. Tip REMOTE_DATA_ARCHIVE_OVERRIDE dotazu může mít následující hodnoty.  
 -   **LOCAL_ONLY**. Aktualizace nebo odstranění místní data.  

 -   **REMOTE_ONLY**. Aktualizace nebo odstranění Vzdálená data.  

 -   **STAGE_ONLY**. Aktualizace nebo odstranění pouze data v tabulce místo, kam databáze roztáhnout fází řádky vhodný k migraci jenom a migrované řádků pro určité období po migraci.  

## <a name="see-also"></a>Viz taky

[Sledování roztáhnout databáze](sql-server-stretch-database-monitor.md)

[Zálohování databáze roztáhnout s podporou](sql-server-stretch-database-backup.md)

[Obnovování databází roztáhnout s podporou](sql-server-stretch-database-restore.md)
