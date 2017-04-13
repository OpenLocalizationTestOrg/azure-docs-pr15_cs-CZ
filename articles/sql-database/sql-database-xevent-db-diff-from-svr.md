<properties
    pageTitle="Rozšířené událostí v databázi SQL | Microsoft Azure"
    description="Popisuje v databázi SQL Azure rozšířené události (XEvents) a jak relace událostí trochu lišit relace událostí v Microsoft SQL Server."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor=""
    tags=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="genemi"/>


# <a name="extended-events-in-sql-database"></a>Rozšířené událostí v databázi SQL

[AZURE.INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Toto téma vysvětluje, jak je mírně odlišnou provádění rozšířené událostí v databázi SQL Azure ve srovnání s rozšířenou událostí v Microsoft SQL Server.


- V12 databáze SQL získané funkci Rozšířené události v druhé polovině kalendář 2015.
- SQL Server byla rozšířené události od 2008.
- Sada funkcí Rozšířené událostí v databázi SQL je robustní podmnožinu funkce na serveru SQL Server.


*XEvents* neformální přezdívek, někdy používaný k "rozšířené události" Probíhá blogy a jiných neformální umístění.


> [AZURE.NOTE] K říjen 2015 aktivaci funkce relace rozšířené událostí v databázi SQL Azure na úrovni náhled. Datum obecný dostupnosti (GA) ještě není nastavená.
>
> Stránka Azure [Aktualizací služeb](https://azure.microsoft.com/updates/?service=sql-database) má příspěvky po větších změn GA oznámení.


Další informace o rozšířené události, pro databáze SQL Azure a Microsoft SQL Server, jsou k dispozici na:

- [Představení aplikace: Rozšířené události na serveru SQL Server](http://msdn.microsoft.com/library/mt733217.aspx)
- [Rozšířené události](http://msdn.microsoft.com/library/bb630282.aspx)


## <a name="prerequisites"></a>Zjistit předpoklady pro


Toto téma předpokládá, že už máte některé znalost:


- [Databáze SQL azure služby](https://azure.microsoft.com/services/sql-database/).


- [Rozšířený událostí](http://msdn.microsoft.com/library/bb630282.aspx) v Microsoft SQL Server.
 - Hromadné naše dokumentace o událostech rozšířené platí pro SQL Server a databázi SQL.


Při výběru souboru událost jako [cílové](#AzureXEventsTargets)je užitečné v těchto předchozí vystavení následující položky:


- [Azure služba úložiště](https://azure.microsoft.com/services/storage/)


- Prostředí PowerShell
 - [Pomocí prostředí PowerShell Azure s úložištěm Azure](../storage/storage-powershell-guide-full.md) - obsahuje podrobné informace o prostředí PowerShell a služba Azure úložiště.


## <a name="code-samples"></a>Ukázky


Související témata obsahují dvě ukázek kódu:


- [Vyzvání kódu cílové vyrovnávací paměť rozšířené událostí v databázi SQL](sql-database-xevent-code-ring-buffer.md)
 - Krátký jednoduchého skriptu jazyce Transact-SQL.
 - Jsme zdůraznit v ukázkové téma kód, že po dokončení s cílovou vyrovnávací, by měl uvolníte jeho zdroje spuštěním alter myší `ALTER EVENT SESSION ... ON DATABASE DROP TARGET ...;` údajů. Později můžete přidat další instanci vyrovnávací tak, že `ALTER EVENT SESSION ... ON DATABASE ADD TARGET ...`.


- [Kód cílové soubor události rozšířené událostí v databázi SQL](sql-database-xevent-code-event-file.md)
 - Fáze 1 je prostředí PowerShell vytvořit kontejner Azure úložiště.
 - Fáze 2 je jazyce Transact-SQL, který používá kontejneru Azure úložiště.


## <a name="transact-sql-differences"></a>Rozdíly Transact-SQL


- Po provedení příkazu [Vytvořit událost relace](http://msdn.microsoft.com/library/bb677289.aspx) na serveru SQL Server pomocí klauzule **Na serveru** . Ale na SQL databáze je místo toho použít klauzuli **Dál databáze** .


- Klauzule **Dál databáze** týká rovněž [PŘETÁHNOUT události cvičení](http://msdn.microsoft.com/library/bb630257.aspx) jazyce Transact-SQL příkazy a [Změnit událost relace](http://msdn.microsoft.com/library/bb630368.aspx) .


- Doporučený postup je zahrnout události cvičení možnost **STARTUP_STATE = zapnuto** v příkazech **RELACI měnit událostí** nebo **Vytvoření události relace** .
 - Hodnota **= zapnuto** podporuje automatické restartování po konfigurace logické databáze z důvodu přepojení.


## <a name="new-catalog-views"></a>Nová zobrazení katalogu


Funkce rozšířený události podporuje několik [katalogu zobrazení](http://msdn.microsoft.com/library/ms174365.aspx). Zobrazení katalogu informovat o *metadata nebo definice* relace uživatel vytvořil událostí v aktuální databázi. Zobrazení nevkládejte informace o výskyty relace aktivní událostí.


| Název<br/>zobrazení katalogu | Popis |
| :-- | :-- |
| **Sys.database_event_session_actions** | Vrátí řádek pro každou akci na každou událost relace událostí. |
| **Sys.database_event_session_events** | Vrátí řádek pro každou událost v relaci události. |
| **Sys.database_event_session_fields** | Vrátí řádku pro každý možnost přizpůsobit sloupec, který byl výslovně nastavené na událostí a cílů. |
| **Sys.database_event_session_targets** | Vrátí řádek pro každou cíl události relaci události. |
| **Sys.database_event_sessions** | Vrátí řádek pro každou událost relaci z databáze SQL databáze. |


V Microsoft SQL Server, podobně jako zobrazení katalogu mají názvy, které obsahují *Server\_ * namísto *.database\_*. Název vzorku je jako **sys.server_event_%**.


## <a name="new-dynamic-management-views-dmvshttpmsdnmicrosoftcomlibraryms188754aspx"></a>Nové Správa dynamických zobrazení [(DMVs)](http://msdn.microsoft.com/library/ms188754.aspx)


Databáze SQL Azure má [Správa dynamických zobrazení (DMVs)](http://msdn.microsoft.com/library/bb677293.aspx) , které podporují rozšířené události. DMVs informovat o *aktivní* relace událostí.


| Název DMV | Popis |
| :-- | :-- |
| **Sys.dm_xe_database_session_event_actions** | Vrátí informace o události cvičení akce. |
| **Sys.dm_xe_database_session_events** | Vrátí informace o zvláštních událostech relace. |
| **Sys.dm_xe_database_session_object_columns** | Zobrazí hodnoty konfigurace pro objekty, které jsou vázaný na relace. |
| **Sys.dm_xe_database_session_targets** | Vrátí informace o cílů relace. |
| **Sys.dm_xe_database_sessions** | Vrátí řádek pro každou relaci událost, kterou má obor vymezený do aktuální databáze. |


V Microsoft SQL Server, podobně jako katalog zobrazení jsou pojmenované bez * \_databáze* část názvu, například:


- **sys.dm_xe_sessions**místo názvu<br/>**sys.dm_xe_database_sessions**.


### <a name="dmvs-common-to-both"></a>DMVs běžné současně


Rozšířené události existují další DMVs, která jsou společná pro databázi SQL Azure nebo Microsoft SQL Server:


- **Sys.dm_xe_map_values**
- **Sys.dm_xe_object_columns**
- **Sys.dm_xe_objects**
- **Sys.dm_xe_packages**



 <a name="sqlfindseventsactionstargets" id="sqlfindseventsactionstargets"></a>

## <a name="find-the-available-extended-events-actions-and-targets"></a>Vyhledání dostupných rozšířených události, akce a cílů


Můžete spustit jednoduchý SQL **Vyberte** získat seznam dostupných událostí, akcí a cílové.


```
SELECT
        o.object_type,
        p.name         AS [package_name],
        o.name         AS [db_object_name],
        o.description  AS [db_obj_description]
    FROM
                   sys.dm_xe_objects  AS o
        INNER JOIN sys.dm_xe_packages AS p  ON p.guid = o.package_guid
    WHERE
        o.object_type in
            (
            'action',  'event',  'target'
            )
    ORDER BY
        o.object_type,
        p.name,
        o.name;
```



<a name="AzureXEventsTargets" id="AzureXEventsTargets"></a>

&nbsp;

## <a name="targets-for-your-sql-database-event-sessions"></a>Cíle pro relace událostí databáze SQL


Tady jsou cílů, které můžete zachytit výsledkem relací událostí v databázi SQL:


- [Cíl vyrovnávací](http://msdn.microsoft.com/library/ff878182.aspx) - stručně obsahuje data události v paměti.
- [Událost čítač cílové](http://msdn.microsoft.com/library/ff878025.aspx) - spočítá všechny události probíhajících během relace rozšířené události.
- [Cílový soubor události](http://msdn.microsoft.com/library/ff878115.aspx) - zápisy dokončení rezerv na kontejner Azure úložiště.


[Událost trasování pro Windows (trasování událostí pro Windows)](http://msdn.microsoft.com/library/ms751538.aspx) rozhraní API není k dispozici pro rozšířený události v databázi SQL.


## <a name="restrictions"></a>Omezení


Existuje několik rozdílů související se zabezpečením befitting cloudu prostředí databáze SQL:


- Rozšířené události jsou založené na modelu izolace jednoho klienta. Relaci událostí v jedné databázi nelze získat přístup k dat nebo události z jiné databáze.

- Nelze zadejte příkaz **Vytvořit událost relace** v kontextu **hlavní** databáze.


## <a name="permission-model"></a>Model oprávnění


Musíte mít oprávnění **ovládací prvek** databáze na příkaz **Vytvořit událost relace** . Vlastník databáze (dbo) oprávnění **řízení** .


### <a name="storage-container-authorizations"></a>Povolení kontejneru úložiště


Token přidružení zabezpečení, které můžete generovat pro kontejner Azure úložiště zadejte **rwl** u oprávnění. Hodnota **rwl** poskytuje následující oprávnění:


- Pro čtení
- Zápis
- Seznam


## <a name="performance-considerations"></a>Důležité informace o výkonu


Existují scénáře, kde můžete náročné využívání rozšířené události nahromadit aktivní paměti, než je správný pro celého systému. Databáze SQL Azure systém proto dynamicky nastaví a upraví kvůli omezením množství aktivní paměti, které mohou být shrnuty relaci události. Spousta faktorů přejděte do dynamického výpočtu.


Pokud se zobrazí chybová zpráva, že nevynucují paměti maximální, jsou některé korekční akce, kterými můžete:


- Spusťte méně relací souběžné události.


- Prostřednictvím svého **vytvořit** a **změnit** příkazy pro událost relace, snižte počet paměti určíte v **MAX\_paměti** klauzule.


### <a name="network-latency"></a>Latence sítě


Cílový **Soubor události** můžete zaznamenat zpoždění síti a k chybám při zachování dat Azure úložiště objektů BLOB. Další události do SQL databáze může zpoždění, které čekají na síťové komunikace dokončete. Toto zpoždění snížit vaše pracovní zátěž.

- Zmírnění rizik tento výkonu, vyhněte se nastavení možnost **EVENT_RETENTION_MODE** **NO_EVENT_LOSS** v události definice relace.


## <a name="related-links"></a>Související odkazy


- [Použití Azure prostředí PowerShell s Azure úložiště](../storage/storage-powershell-guide-full.md).
- [Rutiny pro Azure úložiště](http://msdn.microsoft.com/library/dn806401.aspx)


- [Pomocí prostředí PowerShell Azure s úložištěm Azure](../storage/storage-powershell-guide-full.md) - obsahuje podrobné informace o prostředí PowerShell a služba Azure úložiště.
- [Použití úložiště objektů Blob z .NET](../storage/storage-dotnet-how-to-use-blobs.md)


- [VYTVOŘENÍ přihlašovacích údajů (Transact-SQL)](http://msdn.microsoft.com/library/ms189522.aspx)
- [Vytvoření relace událostí (Transact-SQL)](http://msdn.microsoft.com/library/bb677289.aspx)


- [Jonathan Kehayias příspěvky rozšířené událostí v Microsoft SQL Server](http://www.sqlskills.com/blogs/jonathan/category/extended-events/)


Další témata kód vzorku rozšířený události jsou k dispozici následující odkazy. Však je třeba zkontrolovat pravidelně kterémkoliv vzorku zobrazíte, zda vzorku zaměřuje Microsoft SQL Server a databázi SQL Azure. Pak můžete se rozhodnout, jestli jsou menší změny potřebné ke spuštění vzorku.


<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find the Objects That Have the Most Locks Taken on Them](http://msdn.microsoft.com/library/bb630355.aspx)
-->
