<properties 
    pageTitle="Načtení TB data do SQL datový sklad | Microsoft Azure" 
    description="Ukazuje, jak 1 TB dat lze načíst do datový sklad Azure SQL Azure Data Factory ve skupinovém rámečku 15 minut" 
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="jingwang"/>

# <a name="load-1-tb-into-azure-sql-data-warehouse-under-15-minutes-with-azure-data-factory-copy-wizard"></a>Načtení 1 TB do datový sklad Azure SQL Azure Data Factory [Průvodce kopírováním] v části 15 minut
[Azure SQL datový sklad](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) je cloudové, škálování databáze může zpracování rozsáhlé objemy dat, relační i -relační.  Nakonec na architektura datových paralelní zpracování (MPP) SQL datový sklad je optimalizována pro enterprise datový sklad úloh.  Cloud pružnost nabízí flexibilní velikost úložiště a výpočet nezávisle na sobě.

Začínáme s Azure SQL datový sklad je teď jednodušší než kdykoliv předtím **Azure Data Factory**.  Azure Data Factory je služba integrace plně spravovaných cloudové dat, která mohou sloužit k naplnění SQL datový sklad daty z existující systému a tím vám hodně času při vyhodnocení SQL datový sklad a vytváření řešení analýzy sobě.  Tady jsou hlavních výhod načítání dat do datový sklad SQL Azure pomocí Azure Data Factory:

- **Snadno nastavit**: krok 5 intuitivní průvodce se žádné povinné skriptování.
- **Podpora úložiště formátováním dat**: předdefinované podpora celá řada místním prostředím a data cloudového úložiště.
- **Zabezpečení a požadavkům**: data jsou přenášena HTTPS nebo ExpressRoute a stavu globální služby zajišťuje datům nikdy ponechá zeměpisné okraj
- **Bezkonkurenční výkonu pomocí PolyBase** – pomocí Polybase způsobem nejefektivnější přesunutí dat do datový sklad SQL Azure. Použití funkce pracovní objektů blob, můžete dosáhnout velkém zatížení rychlosti ze všech typů dat ukládají kromě úložiště objektů Blob Azure, který Polybase podporuje ve výchozím nastavení.

Tento článek popisuje, jak používat Průvodce kopírováním Factory dat k načtení dat 1 TB z úložiště objektů Blob Azure do Azure SQL datový sklad ve skupinovém rámečku 15 minut na víc než 1.2 s technologií výkon.

Tento článek obsahuje podrobné pokyny k přesunutí dat do datový sklad SQL Azure pomocí Průvodce kopírováním. 

> [AZURE.NOTE] Viz článek [přesunout data mezi libovolnými datový sklad SQL Azure pomocí Azure Data Factory](data-factory-azure-sql-data-warehouse-connector.md) obecné informace o možnostech Data Factory v přesunutí dat do/z Azure SQL datový sklad. 
> 
> Také je možné vytvářet kanály portálu Azure, Visual Studiu Powershellu, atd. V tématu [kurz: kopírování dat z objektů Blob Azure k databázi SQL Azure](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) stručný návod, podrobné pokyny pro použití kopírovat aktivity v Azure Data Factory.  

## <a name="prerequisites"></a>Zjistit předpoklady pro
- Úložiště objektů Blob Azure: Tento experiment používá úložiště objektů Blob Azure (GRS) pro ukládání TPC H testování datovou sadu.  Pokud nemáte účet Azure úložiště, přečtěte si, [jak vytvořit účet úložiště](../storage/storage-create-storage-account.md#create-a-storage-account).
- Data [TPC H](http://www.tpc.org/tpch/) : budeme používat TPC H jako testování datovou sadu.  Aby je dostala, je potřeba použít `dbgen` ze sady nástrojů TPC H, který umožňuje generovat datové sady.  Můžete buď stáhnout zdrojového kódu pro `dbgen` nástrojích [TPC](http://www.tpc.org/tpc_documents_current_versions/current_specifications.asp) a zpracováním sami nebo kompilovaný binární stahování [GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TPCHTools).  Spuštění dbgen.exe s následující příkazy Generovat plochému souboru 1 TB pro `lineitem` tabulky rozložení přes 10 souborů:
    - `Dbgen -s 1000 -S **1** -C 10 -T L -v`
    - `Dbgen -s 1000 -S **2** -C 10 -T L -v`
    - …
    - `Dbgen -s 1000 -S **10** -C 10 -T L -v` 

    Teď zkopírujte vygenerovaných objektů Blob Azure.  V nápovědě k [přesunutí dat z systému souborů místní pomocí Azure Data Factory a](data-factory-onprem-file-system-connector.md) jak to udělat pomocí ADF kopie.   
- Azure SQL datový sklad: Tento experiment načte data do Azure SQL datový sklad vytvořená pomocí 6000 DWUs

    Podrobné pokyny k vytvoření databáze SQL datový sklad v nápovědě k [Vytvoření datový sklad SQL Azure](../sql-data-warehouse/sql-data-warehouse-get-started-provision/) .  Aby optimálního výkonu možné zatížení do SQL datový sklad pomocí Polybase jsme vyberte maximální počet jednotek sklad dat (DWUs) povolených v nastavení výkonu, což je 6000 DWUs.

    > [AZURE.NOTE] 
    > Při načítání z objektů Blob Azure, data výkon při načítání jsou přímo poměrné počty DWUs konfigurace na datový sklad SQL:
    > 
    > Načtení 1 TB do 1 000 DWU SQL datový sklad trvá 87min (~ 200MBps výkon) načítání 1 TB do 2 000 DWU SQL datový sklad trvá 46min (~ 380MBps výkon) načítání 1 TB do 6 000 DWU SQL datový sklad trvá 14min (~1.2GBps výkon) 

    Pokud chcete vytvořit datový sklad SQL s 6000 DWUs, posuňte jezdec výkonu úplně vpravo:

    ![Posuvník výkonu](media/data-factory-load-sql-data-warehouse/performance-slider.png)

    Pro existující databázi, která nejsou nakonfigurováni 6000 DWUs můžete změnit jeho velikost tak Azure portálu.  Přejděte k databázi Azure portálu a na panelu **Přehled** vidět na následujícím obrázku je tlačítko **Měřítko** :

    ![Tlačítko měřítko](media/data-factory-load-sql-data-warehouse/scale-button.png)    

    Klikněte na tlačítko **Měřítko** chcete otevřít panel následující posunutím posuvníku maximální hodnotu a klikněte na tlačítko **Uložit** .

    ![Dialog měřítko](media/data-factory-load-sql-data-warehouse/scale-dialog.png)
    
    Tento experiment načte data do datový sklad SQL Azure pomocí `xlargerc` třídy prostředků.

    K dosažení nejlepších možné výkon, je nutné provést pomocí SQL datový sklad uživateli z kopie `xlargerc` třídy prostředků.  Zjistěte, jak to udělat tak, že následující [změnit příklad třídy zdroje uživatele](../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md#change-a-user-resource-class-example).  

- Vytvoření cíle schéma v databázi SQL Azure datový sklad spuštěním následujícího příkazu DDL:

        CREATE TABLE [dbo].[lineitem]
        (
            [L_ORDERKEY] [bigint] NOT NULL,
            [L_PARTKEY] [bigint] NOT NULL,
            [L_SUPPKEY] [bigint] NOT NULL,
            [L_LINENUMBER] [int] NOT NULL,
            [L_QUANTITY] [decimal](15, 2) NULL,
            [L_EXTENDEDPRICE] [decimal](15, 2) NULL,
            [L_DISCOUNT] [decimal](15, 2) NULL,
            [L_TAX] [decimal](15, 2) NULL,
            [L_RETURNFLAG] [char](1) NULL,
            [L_LINESTATUS] [char](1) NULL,
            [L_SHIPDATE] [date] NULL,
            [L_COMMITDATE] [date] NULL,
            [L_RECEIPTDATE] [date] NULL,
            [L_SHIPINSTRUCT] [char](25) NULL,
            [L_SHIPMODE] [char](10) NULL,
            [L_COMMENT] [varchar](44) NULL
        )
        WITH
        (
            DISTRIBUTION = ROUND_ROBIN,
            CLUSTERED COLUMNSTORE INDEX
        )

Základní kroky dokončení jsme jste připraveni konfigurace aktivity kopírovat pomocí Průvodce kopírováním.

## <a name="launch-copy-wizard"></a>Spuštění Průvodce kopírováním

1.  Přihlaste se k [portálu Azure](https://portal.azure.com).
2.  Klikněte na **+ Nový** z levého horního rohu, klikněte na **Intelligence + technologie pro analýzu**a klikněte na **Data Factory**. 
6. V **nové factory dat** zásuvné:
    1. Zadejte **název** **LoadIntoSQLDWDataFactory** .
        Název factory Azure data musí být jedinečné. Pokud se zobrazí chyba: **název factory dat "LoadIntoSQLDWDataFactory" není k dispozici**, změňte název factory data (například yournameLoadIntoSQLDWDataFactory) a zkuste znovu vytvořit. Pojmenování pravidla pro Data Factory artefakty naleznete v tématu [Data Factory - pojmenování pravidla](data-factory-naming-rules.md) .  
     
    2. Vyberte Azure **předplatného**.
    3. Pole Skupina zdroje proveďte jeden z následujících kroků: 
        1. Vyberte možnost **používání existujících** vyberte existující skupiny zdrojů.
        2. Vyberte **vytvořit nový** a zadejte název skupiny zdrojů.
    3. Vyberte **umístění** pro data factory.
    4. Zaškrtněte políčko **Připnout do řídicího panelu** v dolní části zásuvné.  
    5. Klikněte na **vytvořit**.
10. Po dokončení vytváření se zobrazí zásuvné **Data Factory** , jak je znázorněno na následujícím obrázku:

    ![Data factory domovskou stránku](media/data-factory-load-sql-data-warehouse/data-factory-home-page-copy-data.png)
11. Na domovské stránce Data Factory klikněte na dlaždici **kopírování dat** a spuštění **Průvodce kopírováním**. 

    > [AZURE.NOTE] Pokud se zobrazí, že webového prohlížeče se zarazí na "Autorizace...", **Block cookies třetích stran a data webu** nastavení zakázat nebo zrušte zaškrtnutí políčka (nebo) v jednoduchosti je povolený vytvořit výjimku pro **login.microsoftonline.com** a pak to zkuste znovu spuštění průvodce.


## <a name="step-1-configure-data-loading-schedule"></a>Krok 1: Konfigurace plán pro načítání dat
Cílem prvního kroku je třeba nakonfigurovat načítání plánu dat.  

Na stránce **Vlastnosti** :
1. Zadejte **Název úkolu** **CopyFromBlobToAzureSqlDataWarehouse**
2. Možnost **jednou spustit** .   
3. Klikněte na tlačítko **Další**.  

![Zkopírujte Wizard – stránka Vlastnosti](media/data-factory-load-sql-data-warehouse/copy-wizard-properties-page.png)

## <a name="step-2-configure-source"></a>Krok 2: Nakonfigurujte zdroje
Tato část popisuje postup pro nastavení zdroje: objektů Blob Azure obsahující řádku 1 TB TPC-H položky soubory.

**Úložiště objektů Blob Azure** vyberte, jak ukládat data a klikněte na tlačítko **Další**.

![Zkopírujte Wizard – stránka vyberte zdroj](media/data-factory-load-sql-data-warehouse/select-source-connection.png)

Vyplňte informace o připojení k účtu úložiště objektů Blob Azure a klikněte na tlačítko **Další**.

![Průvodce kopírováním – informace o připojení zdroje](media/data-factory-load-sql-data-warehouse/source-connection-info.png)

Vyberte **složku** , která obsahuje soubory TPC H řádkové položky a klikněte na tlačítko **Další**.

![Kopírování průvodce – vyberte Vstupní složka](media/data-factory-load-sql-data-warehouse/select-input-folder.png)

Po kliknutí na tlačítko **Další**, jsou automaticky rozpoznat nastavení formátu souboru.  Zkontrolujte, musí být tento sloupec oddělovačem "|"místo čárky výchozí",".  Po mít náhled dat, klikněte na tlačítko **Další** .

![Zkopírujte Wizard – nastavení formátu souboru](media/data-factory-load-sql-data-warehouse/file-format-settings.png)

## <a name="step-3-configure-destination"></a>Krok 3: Konfigurace cíl
V této části se dozvíte, jak nakonfigurovat cíle: `lineitem` tabulky v databázi SQL Azure datový sklad.

Zvolte **Datový sklad SQL Azure** jako cílové úložiště a klikněte na tlačítko **Další**.

![Průvodce kopírováním – vyberte cílovou úložiště dat](media/data-factory-load-sql-data-warehouse/select-destination-data-store.png)

Vyplňte informace o připojení pro datový sklad SQL Azure.  Ujistěte se, vyberte uživatele, který je členem role `xlargerc` (viz oddíl **požadavky na** podrobné pokyny) a klikněte na tlačítko **Další**. 

![Průvodce kopírováním – informace o připojení cíl](media/data-factory-load-sql-data-warehouse/destination-connection-info.png)

Vyberte cílovou tabulku a klikněte na tlačítko **Další**.

![Zkopírujte Wizard – stránka mapování tabulky](media/data-factory-load-sql-data-warehouse/table-mapping-page.png)

Přijměte výchozí nastavení pro mapování sloupců a klikněte na tlačítko **Další**.

![Zkopírujte Wizard – stránka mapování schématu](media/data-factory-load-sql-data-warehouse/schema-mapping.png)


## <a name="step-4-performance-settings"></a>Krok 4: Nastavení výkonu

Ve výchozím nastavení je zaškrtnuté políčko **Povolit polybase** .  Klikněte na tlačítko **Další**.

![Zkopírujte Wizard – stránka mapování schématu](media/data-factory-load-sql-data-warehouse/performance-settings-page.png)

## <a name="step-5-deploy-and-monitor-load-results"></a>Krok 5: Nasazení a sledovat načítání výsledků
Klikněte na tlačítko **Dokončit** nasazení. 

![Průvodce kopírováním - stránce se souhrnem](media/data-factory-load-sql-data-warehouse/summary-page.png)

Po dokončení nasazení na tlačítko `Click here to monitor copy pipeline` sledování výtisku spustit průběh.

Vyberte Kopírovat kanálem k odesílání zpráv, který jste vytvořili v seznamu **Aktivity Windows** .

![Průvodce kopírováním - stránce se souhrnem](media/data-factory-load-sql-data-warehouse/select-pipeline-monitor-manage-app.png)

Můžete zobrazit kopii spustit podrobnosti v **Aktivity okno Průzkumníka** na pravém panelu, včetně dat hlasitost číst data ze zdroje a zapisovat do cíle, dobu trvání a průměrná výkon spustit.

Jak je vidět na následující obrazovce snímek pořídili pomocí kopírování 1 TB z úložiště objektů Blob Azure do SQL datový sklad 14 minut efektivně dosažení 1.22 výkon s technologií!

![Průvodce kopírováním - úspěchu dialogového okna](media/data-factory-load-sql-data-warehouse/succeeded-info.png)

## <a name="best-practices"></a>Doporučené postupy
Tady je několik doporučené postupy pro spuštění databáze SQL Azure datový sklad:

- Použití větší třídy zdroje při načítání do SESKUPENÝ COLUMNSTORE INDEX.
- Efektivnější spojení zvažte použití hash distribuční podle vyberte sloupec místo výchozí zaokrouhlit Petra distribuční.
- Rychlejší načítání rychlosti zvažte použití haldy pro přechodná data.
- Po dokončení načítání Azure SQL datový sklad vytvořte statistiky.

Další informace najdete v článku [Doporučené postupy pro datový sklad SQL Azure](../sql-data-warehouse/sql-data-warehouse-best-practices.md) . 

## <a name="next-steps"></a>Další kroky
- [Průvodce kopírováním Factory dat](data-factory-copy-wizard.md) – Tento článek obsahuje podrobné informace o Průvodci kopírovat. 
- [Kopírovat aktivity výkon a ladění Průvodce](data-factory-copy-activity-performance.md) – Tento článek obsahuje odkaz měření výkonu a ladění průvodce.

