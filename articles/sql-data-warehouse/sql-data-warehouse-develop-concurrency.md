<properties
   pageTitle="Souběžné a pracovní zátěž správy v SQL datový sklad | Microsoft Azure"
   description="Princip správy souběžné a pracovní zátěž v Azure SQL datový sklad k vývoji řešení."
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
   ms.date="09/27/2016"
   ms.author="sonyama;barbkess;jrj"/>

# <a name="concurrency-and-workload-management-in-sql-data-warehouse"></a>Souběžné a pracovní zátěž správy v SQL datový sklad

K dosažení předvídatelná výkonu ve velkém měřítku, Microsoft Azure SQL datový sklad pomáhá ovládáte souběžné úrovně a přidělení zdroje jako stanovení priority procesoru a paměti. Tento článek vás seznámí s koncepty souběžné a pracovní zátěž Správa vysvětlující, jak obě tyto funkce byly provedeny a jak je můžete nastavit v datový sklad. Správa pracovního vytížení SQL datový sklad usnadňuje podporu více uživatelů prostředí. Není určená pro více klienta úloh.

## <a name="concurrency-limits"></a>Souběžné omezení

SQL datový sklad umožňuje až 1 024 souběžné připojení. Všechna 1 024 připojení můžete odeslat souběžně dotazů. Optimalizovat výkon, ale může SQL datový sklad fronty některé dotazy zajistit, aby každý dotaz obdrží grant minimální paměti. Řízení fronty probíhá čas spuštění dotazu. Pomocí fronty dotazů při souběžné limitu, SQL datový sklad lze zlepšit celkový výkon zajistit, aby aktivní dotazů získání přístupu k zásadně potřebnou paměť zdroje.  

Limity souběžné se řídí dvě koncepce: *souběžné dotazů* a *souběžné sloty*. Pro dotaz: Pokud chcete provést třeba spouštět v limit souběžné dotazu a přidělení úsek souběžné.

- Souběžné dotazy jsou dotazů spouštěním ve stejnou dobu. SQL datový sklad podporuje až 32 souběžné dotazy na větších DWU.
- Souběžné sloty se přiřazují podle DWU. Každý 100 DWU obsahuje 4 sloty souběžné. Například DW100 přidělí 4 sloty souběžné a DW1000 přidělí 40. Každý dotaz spotřebovává jeden nebo více souběžné sloty, závisí na [zdroje třídy](#resource-classes) dotazu. Dotazy spuštění ve třídě zdroje smallrc používání jednoho souběžné úsek. Dotazy spuštění ve třídě vyšší zdroje používat další souběžné oblasti.

Následující tabulka popisuje limity pro souběžné dotazů a souběžné sloty v různých velikostech DWU.

### <a name="concurrency-limits"></a>Souběžné omezení

|  DWU   | Max souběžné dotazů  | Souběžné sloty přidělit |
| :----  | :---------------------: | :-------------------------: |
| DW100  |            4            |                4            |
| DW200  |            8            |                8            |
| DW300  |           12            |               12            |
| DW400  |           16            |               16            |
| DW500  |           20            |               20            |
| DW600  |           24            |               24            |
| DW1000 |           32            |               40            |
| DW1200 |           32            |               48            |
| DW1500 |           32            |               60            |
| DW2000 |           32            |               80            |
| DW3000 |           32            |              120            |
| DW6000 |           32            |              240            |

Pokud jeden z těchto mezní hodnoty došlo ke splnění, nové dotazy ve frontě a provedeny jednotlivých první dovnitř, ven.  Dokončení dotazy a počet dotazů a sloty spadá pod limity, dojde k uvolnění ve frontě dotazů. 

> [AZURE.NOTE]  *Vyberte* dotazů spouštěním výhradně na Správa dynamických zobrazení (DMVs) nebo katalogu zobrazení nejsou řídit podle libovolného limity souběžné. Můžete sledovat systému bez ohledu na počet dotazů spouštěním v něm.

## <a name="resource-classes"></a>Zdroje třídy

Zdroje třídy nápovědy ovládáte přidělování paměti a cykly CPU věnovat dotazu. Čtyři třídy zdroje můžete přiřadit uživatele ve formě *role databáze*. Čtyři zdroje třídy jsou **smallrc**, **mediumrc**, **largerc**a **xlargerc**. Uživatelé v smallrc jsou uvedeny menší velikost paměti a mohli využívat všech vyšší souběžné. Naopak uživatelé přiřazená xlargerc jsou uvedeny velké množství paměti, a proto méně jejich dotazy mohlo by umožnit spuštění současně.

Ve výchozím nastavení každý uživatel členem malé zdroje třídy smallrc. Postup `sp_addrolemember` slouží ke zvýšení třídě zdroje a `sp_droprolemember` slouží ke zmenšení třídy prostředků. Třídy prostředků loaduser largerc Zvětšete například tento příkaz:

```sql
EXEC sp_addrolemember 'largerc', 'loaduser'
```

Vhodné je trvale přiřazení uživatelů do třídy prostředku namísto změny svoje třídy zdroje. Například zatížení skupinový columnstore tabulkami vytvořit kvalitu indexy po přidělení víc paměti. Zajištění zatížení přístup do vyšší paměti, vytvořit uživatelský účet speciálně pro načítání dat a trvale přidělte tohoto uživatele do vyšší třídy prostředků.

Existuje několik typů dotazy, které se nevztahují větší přidělování paměti. Systém ignorovat jejich předmětu přidělení zdrojů a vždy spustit tyto dotazy ve třídě malé zdroje místo. Pokud tyto dotazy vždy spustíte ve třídě malé zdroje mohou spouštět při souběžné sloty jsou stlačení a nebudete používat další oblasti než potřeby. Další informace najdete v tématu [výjimky třídy zdroje](#query-exceptions-to-concurrency-limits) .

Několik další podrobnosti o třídy zdroje:

- *Změnit role* oprávnění je potřeba změnit třídy prostředků uživatele.  
- Přestože do jedné nebo více vyšší tříd zdrojů můžete přidat uživatele, bude trvat uživatelé atributy nejvyšší zdroje předmětu, ke kterému jsou přiřazené. Pokud uživatel mediumrc a largerc vyšší třídy prostředků (largerc) je třída zdroje, která bude použito.  
- Nemůžete změnit zdroj třídy správce systému.

Podrobných příkladů najdete v článku [Změna uživatele zdroje třídy příklad](#changing-user-resource-class-example).

## <a name="memory-allocation"></a>Přidělování paměti

Existuje výhody a nevýhody ke zvýšení třídy prostředků uživatele. Zvětšení třída zdroje pro uživatele se přístup jejich dotazy k další paměti, což znamená, že dotazů prováděny rychleji.  Vyšší třídy zdroje však také snižte počet souběžné dotazy, které mohlo by umožnit spuštění. Toto je poměr přidělování velké množství paměti do jednoho dotazu nebo povolení dalších dotazů, které potřebovat přidělování paměti, jak souběžně spustit. Pokud jeden uživatel je možnost Vysoká přidělování paměti dotazu, jiní uživatelé nebudou mít přístup k této stejné paměti ke spuštění dotazu.

V následující tabulce mapy paměť určená pro každé rozdělení podle předmětu DWU a zdroje.

### <a name="memory-allocations-per-distribution-mb"></a>Přidělování paměti za rozdělení (MB)

|  DWU   | smallrc | mediumrc | largerc | xlargerc |
| :----- | :-----: | :------: | :-----: | :------: |
| DW100  |   100   |    100   |    200  |    400   |
| DW200  |   100   |    200   |    400  |    800   |
| DW300  |   100   |    200   |    400  |    800   |
| DW400  |   100   |    400   |    800  |  1,600   |
| DW500  |   100   |    400   |    800  |  1,600   |
| DW600  |   100   |    400   |    800  |  1,600   |
| DW1000 |   100   |    800   |  1,600  |  3,200   |
| DW1200 |   100   |    800   |  1,600  |  3,200   |
| DW1500 |   100   |    800   |  1,600  |  3,200   |
| DW2000 |   100   |  1,600   |  3,200  |  6,400   |
| DW3000 |   100   |  1,600   |  3,200  |  6,400   |
| DW6000 |   100   |  3,200   |  6,400  |  12,800  |

Z předchozí tabulce uvidíte dotazu spuštěna DW2000 ve třídě zdroje xlargerc byste měli mít přístup k 6 400 MB paměti v rámci každé 60 distribuované databází.  V SQL datový sklad jsou 60 rozdělení. Proto abyste spočítali přidělování paměti celkové dotazu ve třídě daného zdroje, výše uvedených hodnot by měl vynásobí 60.

### <a name="memory-allocations-system-wide-gb"></a>Paměť rozdělení celého systému (GB)

|  DWU   | smallrc | mediumrc | largerc | xlargerc |
| :----- | :-----: | :------: | :-----: | :------: |
| DW100  |    6    |    6     |    12   |    23    |
| DW200  |    6    |    12    |    23   |    47    |
| DW300  |    6    |    12    |    23   |    47    |
| DW400  |    6    |    23    |    47   |    94    |
| DW500  |    6    |    23    |    47   |    94    |
| DW600  |    6    |    23    |    47   |    94    |
| DW1000 |    6    |    47    |    94   |   188    |
| DW1200 |    6    |    47    |    94   |   188    |
| DW1500 |    6    |    47    |    94   |   188    |
| DW2000 |    6    |    94    |   188   |   375    |
| DW3000 |    6    |    94    |   188   |   375    |
| DW6000 |    6    |   188    |   375   |   750    |

Z této tabulky přidělování paměti v celém systému, zobrazí se, že dotazu spuštěna DW2000 ve třídě xlargerc zdrojů přiřazen celkem 375 GB paměti (6 400 MB * 60 distribuce / 1 024 převést na GB) přes celý datový sklad SQL.

## <a name="concurrency-slot-consumption"></a>Souběžné úsek spotřebu

SQL datový sklad uděluje více paměti pro dotazy spuštění vyšších tříd zdroje. Paměť je pevné zdroje.  Proto víc paměti přidělený na dotaz méně souběžné dotazů můžete spustit. V následující tabulce opětovně uvádí, že všechny předchozí koncepty v jednom zobrazení, který ukazuje počet souběžné sloty k dispozici prostřednictvím DWU a sloty využívané jednotlivé třídy zdroje.

### <a name="allocation-and-consumption-of-concurrency-slots"></a>Pole přidělení a spotřeby souběžné sloty

|  DWU   | Maximální souběžné dotazů  | Souběžné sloty přidělit | Sloty používaný smallrc |  Sloty používaný mediumrc |  Sloty používaný largerc |  Sloty používaný xlargerc |
| :----  | :---------------------: | :-------------------------: | :-----: | :------: | :-----: | :------: |
| DW100  |            4            |                4            |    1    |     1    |    2    |    4     |
| DW200  |            8            |                8            |    1    |     2    |    4    |    8     |
| DW300  |           12            |               12            |    1    |     2    |    4    |    8     |
| DW400  |           16            |               16            |    1    |     4    |    8    |   16     |
| DW500  |           20            |               20            |    1    |     4    |    8    |   16     |
| DW600  |           24            |               24            |    1    |     4    |    8    |   16     |
| DW1000 |           32            |               40            |    1    |     8    |   16    |   32     |
| DW1200 |           32            |               48            |    1    |     8    |   16    |   32     |
| DW1500 |           32            |               60            |    1    |     8    |   16    |   32     |
| DW2000 |           32            |               80            |    1    |    16    |   32    |   64     |
| DW3000 |           32            |              120            |    1    |    16    |   32    |   64     |
| DW6000 |           32            |              240            |    1    |    32    |   64    |  128     |


Z této tabulky zobrazí se se systémem SQL datový sklad jako DW1000 přidělí maximálně 32 souběžné dotazů a celkových 40 sloty souběžné. Pokud všichni uživatelé jsou spuštěné v smallrc, by 32 souběžné dotazů udělat, protože všechny dotazy, byste měli používat 1 souběžné úsek. Pokud všichni uživatelé na DW1000 spustili v mediumrc obou dotazů by přidělený 800 MB na rozdělení pro přidělování paměti celkové 47 GB na dotaz a souběžné omezeny na 5 uživatelů (40 sloty souběžné / 8 paticím uživatele mediumrc).

## <a name="query-importance"></a>Význam dotazu

SQL datový sklad používá zdroje třídy pomocí pracovního vytížení skupin. Existuje celkem osm pracovní zátěž skupin, kterými se řídí chování třídy zdrojů mezi různými velikostmi DWU. Pro všechny DWU SQL datový sklad používá pouze čtyři osm pracovní zátěž skupin. Tohle dává smysl vzhledem k tomu, že každá skupina pracovní zátěž má přiřazenou do jedné ze čtyř zdroje tříd: smallrc, mediumrc, largerc, nebo xlargerc. Význam Princip skupin pracovní zátěž je, že některé z těchto vytížení skupin nastavený na vyšší *důležitost*. Význam se používá pro procesor plánování. Dotazy pořádání ve vysoké důležitosti pošle třikrát více cykly CPU než s střední důležitost. Proto souběžné úsek mapování taky určit, procesoru priority (priorita). Když dotazu spotřebovává 16 či více sloty, spustí jako Vysoká důležitost.

Následující tabulka zobrazuje důležitost mapování pro každou skupinu zátěží na projektu.

### <a name="workload-group-mappings-to-concurrency-slots-and-importance"></a>Mapování skupin pracovní zátěž souběžné sloty a důležitost

| Pracovní zátěž skupiny | Souběžné úsek mapování | MB / rozdělení. | Mapování důležitost |
| :-------------- | :----------------------: | :---------------: | :----------------- |
| SloDWGroupC00   |            1             |         100       |       Střední       |
| SloDWGroupC01   |            2             |         200       |       Střední       |
| SloDWGroupC02   |            4             |         400       |       Střední       |
| SloDWGroupC03   |            8             |         800       |       Střední       |
| SloDWGroupC04   |           16             |       1,600       |       Vysoký         |
| SloDWGroupC05   |           32             |       3,200       |       Vysoký         |
| SloDWGroupC06   |           64             |       6,400       |       Vysoký         |
| SloDWGroupC07   |          128             |      12,800       |       Vysoký         |

**Rozdělení a spotřeby souběžné sloty** grafu uvidíte, že DW500 používá 1, 4, 8 nebo 16 souběžné sloty pro smallrc, mediumrc, largerc a xlargerc, respektive. Můžete vyhledat jsou tyto hodnoty v předchozím grafu zobrazíte důležitost pro jednotlivé třídy zdroje.

### <a name="dw500-mapping-of-resource-classes-to-importance"></a>Mapování DW500 třídy zdroje na důležitost

| Třídy prostředků | Pracovní zátěž skupiny | Souběžné sloty používá | MB / rozdělení. | Význam |
| :------------- | :------------- | :--------------------: | :---------------: | :--------- |
| smallrc        | SloDWGroupC00  |           1            |         100       |   Střední   |
| mediumrc       | SloDWGroupC02  |           4            |         400       |   Střední   |
| largerc        | SloDWGroupC03  |           8            |         800       |   Střední   |
| xlargerc       | SloDWGroupC04  |          16            |        1,600      |   Vysoký     |


Můžete použít následující dotaz DMV, podívejte se na rozdílech mezi přidělování paměti zdroje podrobně z pohledu správce prostředků k dosažení nebo analýza využití aktivní a historické skupin pracovní zátěž při řešení potíží.

```sql
WITH rg
AS
(   SELECT  
     pn.name                        AS node_name
    ,pn.[type]                      AS node_type
    ,pn.pdw_node_id                 AS node_id
    ,rp.name                        AS pool_name
    ,rp.max_memory_kb*1.0/1024              AS pool_max_mem_MB
    ,wg.name                        AS group_name
    ,wg.importance                  AS group_importance
    ,wg.request_max_memory_grant_percent        AS group_request_max_memory_grant_pcnt
    ,wg.max_dop                     AS group_max_dop
    ,wg.effective_max_dop               AS group_effective_max_dop
    ,wg.total_request_count             AS group_total_request_count
    ,wg.total_queued_request_count          AS group_total_queued_request_count
    ,wg.active_request_count                AS group_active_request_count
    ,wg.queued_request_count                AS group_queued_request_count
    FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
    JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    
            ON  wg.pdw_node_id  = rp.pdw_node_id
            AND wg.pool_id      = rp.pool_id
    JOIN    sys.dm_pdw_nodes pn
            ON  wg.pdw_node_id  = pn.pdw_node_id
    WHERE   wg.name like 'SloDWGroup%'
        AND     rp.name = 'SloDWPool'
)
SELECT  pool_name
,       pool_max_mem_MB
,       group_name
,       group_importance
,       (pool_max_mem_MB/100)*group_request_max_memory_grant_pcnt AS max_memory_grant_MB
,       node_name
,       node_type
,       group_total_request_count
,       group_total_queued_request_count
,       group_active_request_count
,       group_queued_request_count
FROM    rg
ORDER BY
    node_name
,   group_request_max_memory_grant_pcnt
,   group_importance
;
```

## <a name="queries-that-honor-concurrency-limits"></a>Dotazy, které přijmout souběžné omezení

Většina dotazů se řídí třídy zdroje. Tyto dotazy musí vešel do obou souběžné dotazu a souběžné úsek prahové hodnoty. Aby se vyloučila dotazu z modelu úsek souběžné nemůžete si vybrat uživatele.

K zopakovat, dodržovat následující příkazy třídy zdroje:

- VYBERTE VLOŽIT
- AKTUALIZACE
- ODSTRANĚNÍ
- Výběr (je-li dotaz na uživatelské tabulky)
- PŘÍKAZ ALTER OPĚTOVNÉHO VYTVOŘENÍ INDEXU
- PŘÍKAZ ALTER INDEX PŘEUSPOŘÁDAT
- PŘÍKAZ ALTER OPĚTOVNÉHO VYTVOŘENÍ TABULKY
- VYTVOŘENÍ INDEXU
- VYTVOŘENÍ INDEXU SKUPINOVÝ COLUMNSTORE
- VYTVOŘENÍ TABULKY JAKO VYBERTE (CTAS)
- Načítání dat
- Operace s daty pohyb provedeny tak, že Data pohyb služby správy

## <a name="query-exceptions-to-concurrency-limits"></a>Dotaz výjimky z omezení souběžné

Některé dotazy respektovat třídy prostředků přiřazenou uživatele. Tyto výjimky z omezení souběžné jsou určené po paměťové prostředky potřebné pro konkrétní příkaz málo, často vzhledem k tomu, že je příkaz operace metadat. Cílem následujícími výjimkami je vyhnout větší přidělování paměti dotazy, které je nikdy nebudete potřebovat. V těchto případech výchozí malé zdroje třídy (smallrc) používat bez ohledu na skutečnou třídy se uživateli přiřadí. Například `CREATE LOGIN` vždy poběží v smallrc. Materiály potřebné ke splnění operaci jsou velmi nízký, takže ho smysl zahrňte do modelu úsek souběžné dotaz.  Tyto dotazy není omezena taky omezení počtu souběžné 32 uživatelů, neomezený počet tyto dotazy mohlo by umožnit spuštění k limit relace 1 024 relací.

Následující příkazy respektovat třídy zdroje:

- Vytvoření nebo PŘETÁHNOUT tabulky
- ALTER TABLE... PŘEPNOUT, rozdělení nebo sloučit oddíl
- PŘÍKAZ ALTER INDEX ZAKÁZAT
- UVOLNĚNÍ INDEXU
- Vytvoření, aktualizace a PŘETÁHNOUT statistiky
- VYMAZAT DATA TABULKY
- PŘÍKAZ ALTER SE TAK MOHLI OVĚŘOVAT
- VYTVOŘENÍ PŘIHLÁŠENÍ
- Vytvoření, změna nebo DROP USER
- Vytvoření, změna nebo PŘETÁHNOUT postup
- Vytvoření nebo PŘETÁHNOUT zobrazení
- VLOŽENÍ HODNOT
- Vyberte zobrazení systému a DMVs
- VYSVĚTLIT
- DBCC

<!--
Removed as these two are not confirmed / supported under SQLDW
- CREATE REMOTE TABLE AS SELECT
- CREATE EXTERNAL TABLE AS SELECT
- REDISTRIBUTE
-->

## <a name="change-a-user-resource-class-example"></a>Změna příklad třídy zdroje uživatele

1. **Vytvořit přihlášení:** Otevřete připojení k databázi **předlohy** na serveru SQL, který je hostitelem vaší databáze SQL datový sklad a následující příkazy.

    ```sql
    CREATE LOGIN newperson WITH PASSWORD = 'mypassword';
    CREATE USER newperson for LOGIN newperson;
    ```

    > [AZURE.NOTE] Je dobré vytvořit uživatele v hlavní databázi uživatelům Azure SQL datový sklad. Vytvoření uživatele předlohy umožňuje uživateli přihlásit pomocí nástroje jako SSMS bez zadání názvu databáze.  Umožňuje také používat Průzkumník objektů zobrazíte všechny databáze na serveru SQL server.  Podrobné informace o vytváření a Správa uživatelů najdete v článku [zabezpečení databáze systému SQL datový sklad][].

2. **Uživatele vytvořit datový sklad SQL:** Otevřete připojení k databázi **SQL datový sklad** a spusťte následující příkaz.

    ```sql
    CREATE USER newperson FOR LOGIN newperson;
    ```

3. **Udělit oprávnění:** Následující příklad povolí `CONTROL` k databázi **SQL datový sklad** . `CONTROL`v databázi úroveň odpovídá db_owner na serveru SQL Server.

    ```sql
    GRANT CONTROL ON DATABASE::MySQLDW to newperson;
    ```

4. **Zvětšit třídy zdroje:** Přidání uživatele do vyšší rolí Správa pracovního vytížení pomocí následujícího dotazu.

    ```sql
    EXEC sp_addrolemember 'largerc', 'newperson'
    ```

5. **Zmenšit třídy zdroje:** Odebrání uživatele z role, Správa pracovního vytížení použijte následující dotaz.

    ```sql
    EXEC sp_droprolemember 'largerc', 'newperson';
    ```

    > [AZURE.NOTE] Je možné odebrat uživatele z smallrc.

## <a name="queued-query-detection-and-other-dmvs"></a>Zjišťování ve frontě dotaz a další DMVs

Můžete použít `sys.dm_pdw_exec_requests` DMV k identifikaci dotazy, které čekají ve frontě souběžné. Dotazy čeká se na souběžné úsek bude mít stav **pozastaveno**.

```sql
SELECT   r.[request_id]              AS Request_ID
        ,r.[status]              AS Request_Status
        ,r.[submit_time]             AS Request_SubmitTime
        ,r.[start_time]              AS Request_StartTime
        ,DATEDIFF(ms,[submit_time],[start_time]) AS Request_InitiateDuration_ms
        ,r.resource_class                         AS Request_resource_class
FROM    sys.dm_pdw_exec_requests r;
```

Pracovní zátěž Správa rolí je možné zobrazit s `sys.database_principals`.

```sql
SELECT  ro.[name]           AS [db_role_name]
FROM    sys.database_principals ro
WHERE   ro.[type_desc]      = 'DATABASE_ROLE'
AND     ro.[is_fixed_role]  = 0;
```

Následující dotaz ukazuje, jaké role přiřazená každému uživateli.

```sql
SELECT   r.name AS role_principal_name
        ,m.name AS member_principal_name
FROM    sys.database_role_members rm
JOIN    sys.database_principals AS r            ON rm.role_principal_id     = r.principal_id
JOIN    sys.database_principals AS m            ON rm.member_principal_id   = m.principal_id
WHERE   r.name IN ('mediumrc','largerc', 'xlargerc');
```

SQL datový sklad obsahuje následující čekat typy:

- **LocalQueriesConcurrencyResourceType**: dotazy, které nejsou v rámci úsek souběžné sednout. DMV dotazů a systémem funguje jako `SELECT @@VERSION` příkladů místní dotazů.
- **UserConcurrencyResourceType**: dotazy, které sednout v rámci úsek souběžné. Dotazů tabulky koncových uživatelů představují příklady, které byste měli použít tento typ zdroje.
- **DmsConcurrencyResourceType**: čeká výsledkem pohyb operace s daty.
- **BackupConcurrencyResourceType**: Toto čekání označuje, že databáze je zálohování. Maximální hodnota pro tento typ zdroje je 1. Vyžádání více zálohy ve stejnou dobu na ostatní fronty.

`sys.dm_pdw_waits` DMV mohou sloužit k prostředcích, které čeká žádost.

```sql
SELECT  w.[wait_id]
,       w.[session_id]
,       w.[type]            AS Wait_type
,       w.[object_type]
,       w.[object_name]
,       w.[request_id]
,       w.[request_time]
,       w.[acquire_time]
,       w.[state]
,       w.[priority]
,   SESSION_ID()            AS Current_session
,   s.[status]          AS Session_status
,   s.[login_name]
,   s.[query_count]
,   s.[client_id]
,   s.[sql_spid]
,   r.[command]         AS Request_command
,   r.[label]
,   r.[status]          AS Request_status
,   r.[submit_time]
,   r.[start_time]
,   r.[end_compile_time]
,   r.[end_time]
,   DATEDIFF(ms,r.[submit_time],r.[start_time])     AS Request_queue_time_ms
,   DATEDIFF(ms,r.[start_time],r.[end_compile_time])    AS Request_compile_time_ms
,   DATEDIFF(ms,r.[end_compile_time],r.[end_time])      AS Request_execution_time_ms
,   r.[total_elapsed_time]
FROM    sys.dm_pdw_waits w
JOIN    sys.dm_pdw_exec_sessions s  ON w.[session_id] = s.[session_id]
JOIN    sys.dm_pdw_exec_requests r  ON w.[request_id] = r.[request_id]
WHERE   w.[session_id] <> SESSION_ID();
```

`sys.dm_pdw_resource_waits` DMV zobrazuje pouze čeká zdroje využívané daný dotaz. Prodleva zdroje opatření jenom čas, čeká se na materiály poskytované namísto signálu prodleva, což je časové náročnosti podkladového serverů SQL Server naplánování dotazu na procesoru.

```sql
SELECT  [session_id]
,       [type]
,       [object_type]
,       [object_name]
,       [request_id]
,       [request_time]
,       [acquire_time]
,       DATEDIFF(ms,[request_time],[acquire_time])  AS acquire_duration_ms
,       [concurrency_slots_used]                    AS concurrency_slots_reserved
,       [resource_class]
,       [wait_id]                                   AS queue_position
FROM    sys.dm_pdw_resource_waits
WHERE   [session_id] <> SESSION_ID();
```

`sys.dm_pdw_wait_stats` DMV se dá použít pro historické trendu analýze čekat.

```sql
SELECT  w.[pdw_node_id]
,       w.[wait_name]
,       w.[max_wait_time]
,       w.[request_count]
,       w.[signal_time]
,       w.[completed_count]
,       w.[wait_time]
FROM    sys.dm_pdw_wait_stats w;
```

## <a name="next-steps"></a>Další kroky

Další informace o správě uživatelů databáze a zabezpečení najdete v článku [zabezpečení databáze systému SQL datový sklad][]. Další informace o tom, jak větší zdroje třídy zlepšit kvalitu index skupinový columnstore najdete v tématu [Rebuilding indexy zlepšit kvalitu segment].

<!--Image references-->

<!--Article references-->
[Zabezpečení databáze aplikace SQL datový sklad]: ./sql-data-warehouse-overview-manage-security.md
[Opětovné vytvoření indexy zlepšit kvalitu segmentu]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Zabezpečení databáze aplikace SQL datový sklad]: ./sql-data-warehouse-overview-manage-security.md

<!--MSDN references-->
[Managing Databases and Logins in Azure SQL Database]:https://msdn.microsoft.com/library/azure/ee336235.aspx

<!--Other Web references-->
