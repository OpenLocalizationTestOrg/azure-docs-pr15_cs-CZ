<properties
    pageTitle="Zkopírujte aktivity výkon a ladění Průvodce | Microsoft Azure"
    description="Informace o klíčových faktory, které ovlivňují výkon přesun dat v Azure Data Factory při použití kopírovat aktivity."
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
    ms.date="10/25/2016"
    ms.author="jingwang"/>


# <a name="copy-activity-performance-and-tuning-guide"></a>Zkopírujte aktivity výkon a ladění Průvodce
Azure dat Factory kopírovat aktivity poskytuje prvotřídní zabezpečené spolehlivý a výkonné datové načítání řešení. Umožňuje desítky kopii TB dat každý den s formátovaným různých cloudu a místní úložiště dat. Výkon při načítání dat svěží rychlé je klíčová k zajistit, aby se můžete zaměřit na problém "velký data" core: vytváření řešení pokročilé technologie pro analýzu a zprovoznění hloubkové přehledy ze všech dat.

Azure k dispozici sadu podnikové dat úložiště a data warehouse řešení a kopírovat aktivity nabízí vysoce optimalizované dat načítání, který je snadné ke konfiguraci a nastavit možnosti. S jenom jednu kopii aktivitou můžete dosáhnout:

- Načtení dat do **Azure SQL datový sklad** v **1.2 s technologií**
- Načtení dat do **úložiště objektů Blob Azure** v **1.0 s technologií**
- Načtení dat do **Úložiště jezera dat Azure** při **1.0 s technologií**


Tento článek popisuje:

- [Výkon odkazy](#performance-reference) podporované zdroje a jímky data uloží při plánování projektu;
- Funkce, které může pomoct výkon kopie v různých scénářích, včetně [paralelní kopírovat](#parallel-copy) [cloudu dat pohyb jednotky](#cloud-data-movement-units)a [připravené kopírovat](#staged-copy);
- [Pokyny pro optimalizaci výkonu](#performance-tuning-steps) o tom, jak optimalizovat výkon a klíčové faktory, které můžou mít vliv na výkon kopírovat.

> [AZURE.NOTE] Pokud znáte není kopírovat aktivity obecně, najdete v článku [přesunutí dat pomocí kopírování aktivity](data-factory-data-movement-activities.md) před čtení tohoto článku.

## <a name="performance-reference"></a>Výkonu

![Matice výkonu](./media/data-factory-copy-activity-performance/CopyPerfRef.png)

> [AZURE.NOTE] Vyšší výkon můžete dosáhnout využívání více pohyb jednotek dat (DMUs) než výchozí maximální DMUs, což je 8 cloudu do cloudu kopírovat aktivity spustit. Například s 100 DMUs můžete zkopírovat data z objektů Blob Azure do úložiště jezera dat Azure sazbou 1 GB za sekundu. Naleznete v části [cloudu dat pohyb jednotky](#cloud-data-movement-units) podrobné informace o této funkci. Obraťte se na [podporu Azure](https://azure.microsoft.com/support/) požádat o další DMUs.

Ukazatel je potřeba pamatovat:

- Výkon nevypočítávají pomocí následujícího vzorce: [velikost dat přečíst ze zdroje] / [Kopírovat aktivita spustit doba trvání].
- Referenční čísla výkonu v tabulce byly měřit [TPC H](http://www.tpc.org/tpch/) uvedenou množinu dat v jedné kopie aktivitě spustit.
- Zkopírovat mezi cloudové úložiště dat, nastavte pro porovnání **cloudDataMovementUnits** 1 a 4 (nebo 8). není zadaný **parallelCopies** . V části [paralelní kopírovat](#parallel-copy) podrobné informace o těchto funkcích.
- V Azure dat ukládají do něj zdrojovou a jímky jsou ve stejné oblasti Azure.
- Pro hybridní nasazení (místní cloudu nebo cloudu pro místní) přesun dat jednoho brány byl spuštěn na počítač, který byl nezávislá na místním úložišti. Konfigurace je uvedený v následující tabulce. Při práci na brány se systémem jedné aktivity, spotřebované množství operace kopírování pouze malou část procesoru počítače test, paměti nebo šířka pásma.
    <table>
    <tr>
        <td>PROCESOR</td>
        <td>32 jádra 2,20 GHz Intel Xeon E5-2660 v2</td>
    </tr>
    <tr>
        <td>Paměti</td>
        <td>128 GB</td>
    </tr>
    <tr>
        <td>Sítě</td>
        <td>Rozhraní Internetu: 10 GB/s; rozhraní intranet: 40 s technologií</td>
    </tr>
    </table>

## <a name="parallel-copy"></a>Paralelní kopie
Můžete číst data ze zdroje nebo zápisu dat cíl **paralelně v rámci aktivitu kopírovat spustit**. Tato funkce zlepšuje výkon operace Kopírovat a snižuje časové náročnosti Pokud chcete přesunout data.

Toto nastavení se liší od vlastnost **souběžné** v definici aktivity. Vlastnost **souběžné** zjistí počet **že souběžné aktivity kopírovat spustí** zpracování dat z různých aktivity windows (1: 00 do 2: 00, 2 dopoledne 3 dopoledne, 3 dopoledne 4 dop a tak dál). Tato možnost je užitečné při provádění historických načíst. Funkce paralelní kopírovat platí **jedné aktivity spustit**.

Podívejme se na scénáři výběru. V následujícím příkladu více výsečí v minulosti nutné zpracovat. Data Factory běží instance kopírovat aktivity (aktivity spustit) pro každé řez:

- Výseč z okna první činnosti (1: 00 do 2: 00) == > aktivity spustit 1
- Výseč z okna druhý aktivity (2: 00 do 3: 00) == > aktivity spusťte 2
- Výseč z okna druhý aktivity (3: 00 do 4: 00) == > aktivity spustit 3

Atd.

V tomto příkladu při **souběžné** hodnota je nastavena na hodnotu 2 **aktivity spusťte 1** a **2 Spusťte aktivity** kopírování dat z dvou aktivity windows **souběžně** zvýšit výkon pohyb data. Však pokud najednou několik souborů spojená se aktivity spustit 1, datové služby pohybu slouží ke kopírování soubory ze zdroje do cílového souboru jeden po druhém.

### <a name="parallelcopies"></a>parallelCopies
Vlastnost **parallelCopies** označíte paralelismus, které chcete kopírovat aktivity používat. Tato vlastnost si můžete představit jako maximální počet podprocesy Kopírovat činnost, které můžete číst ze zdroje nebo zapisovat do vašeho úložiště dat jímky souběžně.

Pro každé spuštění aktivitě kopírovat Data Factory určuje počet kopií paralelní použít ke zkopírování dat ze zdroje dat ukládat a data cílového úložiště. Výchozí počet kopií paralelní, které používá závisí na typu zdroje a jímky, který používáte.  

Zdroj a jímky |   Počet kopií paralelní výchozí určena služby
------------- | -------------------------------------------------
Přesouvat data mezi založené na souboru ukládá (úložiště objektů Blob; Úložiště jezera dat; Amazon S3; systém souborů místní; místní HDFS) | 1 až 32. Závisí na velikosti souborů a počet jejích cloudu dat pohyb jednotky (DMUs) slouží k přesouvat data mezi dvěma cloudové úložiště dat nebo fyzickou konfiguraci počítače brány umožňuje kopii hybridní (kopírovat data z úložiště místní data).
Kopírování dat z **libovolné zdrojová data obsahují úložiště tabulek Azure** | 4
Všechny ostatní zdroje a jímky dvojice | 1

Obvykle výchozí chování by vám měl dát nejlepší výkon. Však řídit načíst do počítačů, které hostují datům ukládá nebo pro optimalizaci výkonu kopírovat, si můžete změnit výchozí hodnota a zadejte hodnotu pro vlastnost **parallelCopies** . Hodnota musí být 1 až 32 (obě včetně). Za běhu pro dosažení nejlepších výsledků, kopírovat aktivita používá hodnota, která je větší nebo rovnu hodnotě nastavené.

    "activities":[  
        {
            "name": "Sample copy activity",
            "description": "",
            "type": "Copy",
            "inputs": [{ "name": "InputDataset" }],
            "outputs": [{ "name": "OutputDataset" }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                },
                "sink": {
                    "type": "AzureDataLakeStoreSink"
                },
                "parallelCopies": 8
            }
        }
    ]

Ukazatel je potřeba pamatovat:

- Přesouvat data mezi založené na souboru ukládá paralelismus nastane na úrovni souboru. Neexistuje žádný bloků v rámci jednoho souboru. Skutečný počet kopií parallel data pohyb služba používá pro kopírování za běhu není větší než počet souborů, které máte. -Li kopírovat chování **mergeFile**, kopírovat aktivity nejde využívat paralelismus.
- Při zadání hodnoty pro vlastnost **parallelCopies** zvažte zvýšení zatížení na datových úložišť zdrojový a jímky a k bráně pro Pokud hybridní kopii. K tomu dojde, zejména pokud máte víc aktivity nebo souběžné výskytů stejnou činnost, která spustí hledání stejném úložišti. Pokud zjistíte, že úložiště dat nebo brány je mnoha načíst snížíte hodnotu **parallelCopies** snížit načíst.
- Při kopírování dat z úložiště, které nejsou založených na úložiště, které jsou založeny na souboru datové služby pohyb ignoruje vlastnost **parallelCopies** . I v případě paralelismus není v tomto případě použije.

> [AZURE.NOTE] Použít funkci **parallelCopies** po výběru kopii hybridní je nutné použít Brána pro správu dat verze 1.11 nebo novější.

### <a name="cloud-data-movement-units"></a>Shluk dat pohyb jednotky
**Shluk datové pohyb jednotky (DMU)** je míra, která představuje power (kombinací vytížení procesoru, paměti a přidělení zdrojů síť) jednu jednotku v Data Factory. DMU lze použít v cloudu do cloudu kopírování, ale ne v hybridním kopii.

Ve výchozím nastavení používá Data Factory jednoho cloudu DMU provádět jedné kopie aktivity spustit. Chcete-li změnit výchozí, zadejte hodnotu pro vlastnost **cloudDataMovementUnits** následujícím způsobem. Informace o úrovni zvýšení výkonu, která se může zobrazit při konfiguraci více jednotek pro konkrétní Kopírovat zdroj a jímky najdete v tématu [výkonu](#performance-reference).

    "activities":[  
        {
            "name": "Sample copy activity",
            "description": "",
            "type": "Copy",
            "inputs": [{ "name": "InputDataset" }],
            "outputs": [{ "name": "OutputDataset" }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                },
                "sink": {
                    "type": "AzureDataLakeStoreSink"
                },
                "cloudDataMovementUnits": 4
            }
        }
    ]

**Povolené hodnoty** pro vlastnost **cloudDataMovementUnits** jsou 1 (výchozí), 2, 4 a 8. **Skutečný počet cloudu DMUs** používající zkopírování za běhu je rovna nebo menší než hodnota nakonfigurováno v závislosti na typu dat. 

> [AZURE.NOTE] Pokud potřebujete další cloudu DMUs pro vyšší výkon, kontaktujte [podporu Azure](https://azure.microsoft.com/support/). Nastavení 8 a nad aktuálně funguje pouze v případě, že zkopírujete více souborů z úložiště objektů Blob k úložišti objektů Blob, úložiště jezera dat nebo databáze SQL Azure, a velikost souboru je větší než nebo rovno 16 MB samostatně.

Lepší používat tyto dvě vlastnosti a zlepšit výkon pohyb vaše data, najdete v článku [ukázkové případy použití](#case-study-use-parallel-copy). Nemusíte konfigurace **parallelCopies** umožní využít výhod výchozí chování. Pokud konfigurace a **parallelCopies** je příliš malý, nemusí plně využít více cloudu DMUs.  

Je **důležité** si zapamatovat, že vám bude účtovaná na základě celkový čas operace kopírování. Pokud Kopírovat projekt slouží k hodinu pořídit jednu cloudu jednotky a teď trvá 15 minut s čtyři cloudu jednotky, celkové faktury se nezmění téměř. Například použijete čtyři cloudu jednotky. První jednotka cloudu tráví 10 minut, druhý, 10 minut, třetí jeden 5 minut a je čtvrtý 5 minut všechny v jedné kopie aktivitě spustit. Vám bude účtovaná za doby celkové kopie (přesun dat), což je 10 + 10 + 5 + 5 = 30 minut. Používání **parallelCopies** nemá vliv na fakturace.

## <a name="staged-copy"></a>Fázovanou kopie
Při kopírování dat z úložiště zdroje dat do úložiště jímky dat, můžete použít úložiště objektů Blob jako dočasné pracovní úložiště. Pracovní je užitečné v těchto případech:

1.  **Chcete-li jedí data z různých ukládají data do SQL datový sklad prostřednictvím PolyBase**. SQL datový sklad používá PolyBase jako mechanismus vysoký výkon načítání velkého množství dat v SQL datový sklad. Však zdrojová data musí být v úložišti objektů Blob a musí splňovat další kritéria. Po načtení dat z úložiště dat než úložiště objektů Blob můžete aktivovat data kopírování prostřednictvím dočasné pracovní úložiště objektů Blob. V takovém případě Data Factory provede transformace požadovaná data zajistit, že vyhovuje požadavkům PolyBase. Potom použije PolyBase k načtení dat do SQL datový sklad. Další podrobnosti najdete v tématu [Použití PolyBase k načtení dat do datový sklad SQL Azure](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse).
2.  **Někdy trvá, než si ji přesouvat data hybridní (to znamená, zkopírujte mezi místních dat úložiště přihlašovacích údajů a dat cloudu úložiště) přes pomalé síťové připojení**. Aby se zvýšil výkon, Komprimovat můžete data místní tak, aby ho zkracuje chcete přesunout data k úložišti pracovní dat v cloudu. Můžete pak rozbalte data v úložišti pracovní než načtení do cílového úložiště.
3.  **Nechcete otevřete porty než port 80 a 443 v bráně firewall, protože podnikových zásad IT**. Třeba při kopírování dat z místního datový úložiště jímky databáze SQL Azure nebo jímky datový sklad SQL Azure, musíte aktivovat odchozí komunikaci TCP na port 1433 brána Windows firewall a podnikovou bránu firewall. V tomto scénáři využívat brány první zkopírovat údaje k instanci pracovní úložiště objektů Blob přes HTTP nebo HTTPS na port 443. Potom data načítáte do SQL databáze nebo SQL datový sklad z pracovní úložiště objektů Blob. V tomto tok není potřeba povolit port 1433.

### <a name="how-staged-copy-works"></a>Jak fázované kopírování funguje
Při aktivaci funkce pracovní nejdřív data zkopírována z obchodu zdroje dat k úložišti pracovní dat (přenést vlastní). Pak data zkopírována z pracovní úložiště dat k úložišti jímky. Data Factory automaticky spravuje tok dvoufázový za vás. Data Factory taky vyčistí dočasná data z pracovní úložiště přesun dat dokončení.

V případě kopírovat cloud (zdroj a jímky data, která ukládá se v cloudu) není použit brány. Data Factory služby provádí operace Kopírovat.

![Připravené kopie: scénář cloudu](media/data-factory-copy-activity-performance/staged-copy-cloud-scenario.png)

V případě kopírovat hybridní (zdrojem místní a jímky je v cloudu), brány slouží k přesunutí dat z úložiště zdroj dat pracovní datový úložiště. Datové Factory služby slouží k přesunutí dat z obchodu pracovní dat k úložišti jímky. Kopírování dat z cloudové úložiště dat do úložiště místní data prostřednictvím pracovní je podporované taky obrácené toku.

![Připravené kopie: hybridní scénář](media/data-factory-copy-activity-performance/staged-copy-hybrid-scenario.png)

Při aktivaci přesun dat pomocí pracovní úložiště, můžete určit, zda chcete data, která chcete komprimovat teprve pak přejděte data z úložiště zdroje dat k úložišti dočasné nebo pracovní data a potom dekomprimoványt před přesouvání dat mezi jako dočasné nebo přípravu úložiště jímky úložiště.

V současné době nejde přesouvat data mezi dvěma místní úložiště dat pomocí pracovní úložiště. Tato možnost je k dispozici brzy očekávat.

### <a name="configuration"></a>Konfigurace
Nakonfigurujte nastavení **enableStaging** v kopírovat aktivity můžete určit, zda chcete data, která mají být připravené v úložišti objektů Blob než načtení do cílového úložiště. Pokud nastavíte **enableStaging** logickou hodnotu PRAVDA, zadejte další vlastnosti uvedené v následující tabulce. Pokud nemáte jeden, bude potřeba vytvořit úložišti Azure nebo úložiště sdílené aplikace access propojené podpis služby pro pracovní.

Vlastnost | Popis | Výchozí hodnota | Povinné
--------- | ----------- | ------------ | --------
**enableStaging** | Určete, zda chcete zkopírovat data prostřednictvím průběžného přípravu úložiště přihlašovacích údajů. | NEPRAVDA | Ne
**linkedServiceName** | Zadejte název [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) nebo [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) propojené služby, které odkazuje na instanci úložiště, který můžete použít jako dočasné pracovní úložiště. <br/><br/> Úložiště nelze použít s podpisem sdílený přístup k načtení dat do SQL datový sklad prostřednictvím PolyBase. Můžete ji ve všech ostatních případech. | NENÍ K DISPOZICI | Ano, pokud **enableStaging** je nastavena na TRUE
**Cesta** | Zadejte cestu úložiště objektů Blob, která bude obsahovat fázované data. Pokud nezadáte cesty, vytvoří službu kontejneru k ukládání dočasná data. <br/><br/> Zadejte cestu pouze v případě použití úložiště s podpisem sdílený přístup vyžadují dočasné dat je v určitém umístění. | NENÍ K DISPOZICI | Ne
**enableCompression** | Určuje, zda mají být data komprimovány před zkopírována do cíle. Toto nastavení snižuje objemu dat převáděny. | NEPRAVDA | Ne

Tady je ukázka definice kopírovat aktivity s vlastnostmi, které jsou popsané v předchozí tabulce:

    "activities":[  
    {
        "name": "Sample copy activity",
        "type": "Copy",
        "inputs": [{ "name": "OnpremisesSQLServerInput" }],
        "outputs": [{ "name": "AzureSQLDBOutput" }],
        "typeProperties": {
            "source": {
                "type": "SqlSource",
            },
            "sink": {
                "type": "SqlSink"
            },
            "enableStaging": true,
            "stagingSettings": {
                "linkedServiceName": "MyStagingBlob",
                "path": "stagingcontainer/path",
                "enableCompression": true
            }
        }
    }
    ]


### <a name="billing-impact"></a>Dopad fakturace
Bude účtovaná podle dva kroky: kopírování dobu trvání a zkopírujte typu. 

- Při použití přípravu při kopírování cloud (kopírování dat z cloudové úložiště dat do jiné cloudové úložiště) vám bude účtovaná [Součet kopírovat dobu kroků 1 a 2] x [cloudu kopírovat Jednotková cena].
- Při použití přípravu při kopírování hybridní (kopírování dat z úložiště místních dat do cloudové úložiště dat) vám bude účtovaná za [hybridní kopírovat trvání] x [hybridní kopírovat Jednotková cena] + [cloudu kopírovat trvání] x [cloudu kopírovat Jednotková cena].


## <a name="performance-tuning-steps"></a>Postup pro optimalizaci výkonu
Doporučujeme, že provedete tyto kroky pro optimalizaci výkonu služby Data Factory s kopírovat aktivity:

1.  **Vytvoření směrný plán**. Během fáze vývoje testovat vaše profilace pomocí Kopírovat aktivity proti výběru zástupce data. Chcete-li omezit množství dat, se kterými pracujete s můžete použít Data Factory [rozdělení na řezy modelu](data-factory-scheduling-and-execution.md#time-series-datasets-and-data-slices) .

    Shromáždit času spuštění a vlastnosti výkon při používání **sledování a Správa aplikací**. Na domovské stránce Data Factory zvolte **Monitor a spravovat** . Ve stromovém zobrazení vyberte **datovou sadu výstupu**. V seznamu **Aktivity Windows** zvolte Kopírovat aktivity spustit. Doba trvání kopírovat aktivity a velikost dat, které se zkopírují uvádí seznam **Aktivity Windows** . Výkon je uvedená v **Průzkumníku okno aktivity**. Další informace o aplikaci najdete v tématu [Monitor a správa Azure Data Factory potrubí při používání sledování a Správa aplikací](data-factory-monitor-manage-app.md).

    ![Spustit podrobnosti o aktivitách](./media/data-factory-copy-activity-performance/mmapp-activity-run-details.png)

    Dále v tomto článku můžete porovnat výkon a konfigurace nefunguje k kopírovat aktivity [výkonu](#performance-reference) z našich testů.
2. **Diagnostikování a optimalizovat výkon**. Pokud výkonu, které můžete sledovat nevyhovuje vašemu očekávání, musíte identifikovat kritické. Optimalizujte výkon odebrat nebo snížit efekt problémových míst. Úplný popis Diagnostika výkonu je nad rámec v tomto článku, ale tady je několik běžných tipů:
    - Funkce výkonu:
        - [Paralelní kopie](#parallel-copy)
        - [Shluk dat pohyb jednotky](#cloud-data-movement-units)
        - [Fázovanou kopie](#staged-copy)   
    - [Zdroje](#considerations-for-the-source)
    - [Jímky](#considerations-for-the-sink)
    - [Serializace a rekonstrukce](#considerations-for-serialization-and-deserialization)
    - [Komprese](#considerations-for-compression)
    - [Mapování sloupce](#considerations-for-column-mapping)
    - [Brána pro správu dat](#considerations-for-data-management-gateway)
    - [Další informace](#other-considerations)

3. **Rozbaleno: Konfigurace celou sadu dat.** Až budete spokojeni s výsledky spuštění a výkonu, můžete rozbalit definice a kanálem k odesílání zpráv aktivního období pokrýval celý množiny dat.

## <a name="considerations-for-the-source"></a>Důležité informace o zdroji
### <a name="general"></a>Obecné
Ujistěte se, že není tak, že ostatní pracovního vytížení spuštěné nebo u ní mnoha úložišti podkladová data. 

Úložiště dat Microsoft v tématu [sledování a optimalizace témata](#performance-reference) , které jsou specifické pro datový úložiště a pomůže pochopit dat ukládání pracovních charakteristik minimalizovat doby odezvy a maximalizace výkon.

Pokud zkopírujete data z úložiště objektů Blob SQL datový sklad, zvažte použití **PolyBase** zvýšit výkon. Další informace najdete v článku [Použití PolyBase k načtení dat do Azure SQL datový sklad](data-factory-azure-sql-data-warehouse-connector.md###use-polybase-to-load-data-into-azure-sql-data-warehouse) .


### <a name="file-based-data-stores"></a>Úložiště na základě souboru dat
*(Včetně úložiště objektů Blob, úložiště jezera dat, Amazon S3, místní systémy souborů a místní HDFS)*

- **Průměrná velikost souboru a počet souborů**: Kopírovat aktivity přepojí datového souboru po druhém. Stejné množství dat, které se mají přesunout celkový výkon je se malá písmena, pokud data obsahují větší množství souborů menší než několik velkých souborů z důvodu zavádění fáze pro každý soubor. Proto pokud je to možné, zkombinovat malé soubory většími soubory k získání vyšší výkon.
- **Formát souboru a komprese**: Další způsoby, jak zlepšit výkonu, naleznete v částech [aspektech serializace a rekonstrukce](#considerations-for-serialization-and-deserialization) a [Co byste měli zvážit pro kompresi](#considerations-for-compression) .
- **Místní systém souborů** scénáři, ve které se **Brána pro správu dat** vyžaduje, naleznete v části [Co byste měli zvážit dat brány pro správu](#considerations-for-data-management-gateway) .

### <a name="relational-data-stores"></a>Úložiště relačních dat
*(Včetně SQL databázi. SQL datový sklad; Amazon Redshift; Databáze SQL serveru. a Oracle databáze MySQL DB2, Teradata, Sybase a PostgreSQL, atd.)*

- **Typu dat**: schématu tabulky vliv na výkon kopírovat. Velikost velkých řádku dává vyšší výkon než velikost malé řádku zkopírujte stejné množství dat. Důvodem je, aby databázi můžete mnohem efektivněji vyvolat méně dávkách obsahujících data, která obsahují méně řádků.
- **Dotazu nebo uložené procedury**: optimalizace logickou dotazu nebo uložené procedury zadané ve zdroji kopírovat aktivity načítání dat mnohem efektivněji.
- Na **místním relační databáze**, třeba SQL Server nebo Oracle, které vyžadují použití **Brána pro správu dat**, naleznete v části [Důležité informace týkající se Brána pro správu dat](#considerations-on-data-management-gateway) .

## <a name="considerations-for-the-sink"></a>Co zvážit při jímce

### <a name="general"></a>Obecné
Ujistěte se, že není tak, že ostatní pracovního vytížení spuštěné nebo u ní mnoha úložišti podkladová data. 

Úložiště dat Microsoft naleznete [sledování a optimalizace témata](#performance-reference) , které jsou specifické pro datový úložiště. Tato témata vám ukazatele výkonu úložiště a jak minimalizovat doby odezvy a maximalizovat výkon můžou pomoct.

Pokud zkopírujete data z **úložiště objektů Blob** **SQL datový sklad**, zvažte použití **PolyBase** zvýšit výkon. Další informace najdete v článku [Použití PolyBase k načtení dat do Azure SQL datový sklad](data-factory-azure-sql-data-warehouse-connector.md###use-polybase-to-load-data-into-azure-sql-data-warehouse) .


### <a name="file-based-data-stores"></a>Úložiště na základě souboru dat
*(Včetně úložiště objektů Blob, úložiště jezera dat, Amazon S3, místní systémy souborů a místní HDFS)*

- **Kopírovat chování**: kopírování dat z jiné souborové úložiště kopírovat aktivity má tři možnosti prostřednictvím **copyBehavior** vlastnosti. Zachová hierarchie, sloučí hierarchie nebo sloučí soubory. Zachování nebo sloučení hierarchie má malý nebo žádný nároky na výkon, ale sloučení souborů způsobí, že nároky na výkon zvětšíte.
- **Formát souboru a komprese**: naleznete v částech [aspektech serializace a rekonstrukce](#considerations-for-serialization-and-deserialization) a [Co byste měli zvážit komprese](#considerations-for-compression) další způsoby, aby se zvýšil výkon.
- **Úložiště objektů BLOB**: v současné době podporuje úložiště objektů Blob blokovat pouze objektů blob pro přenos optimalizované dat a výkon.
- **Místní systémy souborů** scénáře, které vyžadují použití **Brána pro správu dat**naleznete v části [Co byste měli zvážit dat brány pro správu](#considerations-for-data-management-gateway) .

### <a name="relational-data-stores"></a>Úložiště relačních dat
*(Včetně SQL databázi SQL datový sklad, databáze systému SQL Server a databáze Oracle)*

- **Zkopírujte chování**: v závislosti na vlastnosti jste nastavili pro **sqlSink**aktivity kopírovat data zapisuje cílové databázi různými způsoby.
    - Ve výchozím nastavení připojení dat pohyb služba používá rozhraní API kopírovat hromadné vložení dat v režimu, který obsahuje nejlepší možný výkon.
    - Pokud konfigurace proceduru uloženou v jímce databázi platí jeden řádek dat současně ne jako hromadné načítání. Výkon vynechává podstatně. Je-li množiny dat velká, případně zvažte možnost používat vlastnost **sqlWriterCleanupScript** .
    - Konfigurace vlastností **sqlWriterCleanupScript** pro jednotlivé kopie aktivity spustit službu spustí skript a potom vložte data pomocí rozhraní API kopírovat hromadné. Například přepsat celou tabulku s nejnovějšími daty, můžete určit skript, který nejdřív odstranit všechny záznamy před hromadného načtení nová data ze zdroje.
- **Velikost vzorku a dávku dat**:
    - Schématu tabulky vliv na výkon kopírovat. Zkopírovat stejné množství dat, velikost velkých řádku umožňuje vyšší výkon než velikost malé řádku, protože databázi efektivněji potvrdit méně dávkách obsahujících data.
    - Kopírovat aktivity vloží data v řadě listy. Počet řádků v listu můžete nastavit pomocí vlastnosti **writeBatchSize** . Pokud vaše data obsahují malé řádky, můžete nastavit vlastnost **writeBatchSize** s hodnotou vyšší využívat nižší dávku režijních a vyšší výkon. Pokud řádku dat je velký, dávejte při zvětšení **writeBatchSize**. Hodnotu Vysoká může vést k selhání kopírovat způsobená přetížení databáze.
- **Místní relační databáze** jako SQL serveru nebo Oracle, které vyžaduje použití **Brána pro správu dat**, naleznete v části [Co byste měli zvážit dat brány pro správu](#considerations-for-data-management-gateway) .


### <a name="nosql-stores"></a>NoSQL úložiště
*(I úložiště tabulek Azure DocumentDB)*

- **Úložiště tabulek**:
    - **Oddíl**: zápis dat do prokládaném oddíly výrazně snižuje výkon. Zdrojová data řadit klíč oddílu tak, aby data je vložen efektivní do jeden oddíl po druhém nebo upravte logickou k zápisu dat na jeden oddíl.
- Pro **DocumentDB**:
    - **Velikost dávky**: vlastnost **writeBatchSize** nastaví počet paralelní žádosti o službu DocumentDB pro vytváření dokumentů. Lepší výkon můžete očekávat, pokud zvětšíte **writeBatchSize** , protože DocumentDB se odešle žádost o další paralelní. Však pozor omezení při psaní DocumentDB (chyba se zpráva "Požadavku sazba velké"). Různé faktory mohou způsobit omezení velikosti dokumentů, včetně počtu podmínek v dokumentech a cílové kolekce indexování zásad. K dosažení vyšší výkon kopírovat, zvažte použití kolekci lepší, například S3.

## <a name="considerations-for-serialization-and-deserialization"></a>Co byste měli zvážit serializace a rekonstrukce
Serializace a rekonstrukce může dojít při zadávání uvedenou množinu dat nebo výstup uvedenou množinu dat je soubor. V současné době podporuje kopírovat aktivity Avro a Text (například CSV a TSV) formátů data.

**Zkopírujte chování**:

-   Kopírování souborů mezi dat na základě souboru ukládá:
    - Když vstupní a výstupní množinách dat. obou mají stejné nebo žádné nastavení formátu souboru, službu přesun dat provede binární Kopírovat bez serializace nebo deserializace. Zobrazí vyšší výkon ve srovnání s scénáře, ve kterém se liší od sebe nastavení zdroje a jímky formát souboru.
    - Při zadávání a výstup sady dat obě jsou ve formátu textu a pouze kódování typ se liší, datové služby pohyb pouze znamená kódování převod. Neprovádí všechny serializace a rekonstrukce, který způsobí, že některé výkonu srovnatelného nároky na binární kopii.
    - Při vstupní a výstupní sady dat obou máte jiných formátech souboru nebo různé konfigurace jako oddělovače, datové služby pohyb deserializuje zdrojových dat můžete vysílat datovými proudy, transformace a potom serializuje do výstupní formát, které jste uvedli. Tuto operaci výsledkem mnohem víc významné výkon nároky ve srovnání s ostatních případech.
- Při kopírování souborů z úložiště dat, která není založená na souborech (například z úložiště souborové relační Store) požaduje serializace nebo deserializace kroku. Tento krok za následek režijních výkonu.

**Formát souboru**: formátu zvolíte může mít vliv na výkon kopírovat. Avro je například kompaktní binárním formátu, který ukládá metadat s daty. Rozsáhlá podpora má ekosystému Hadoop pro zpracování a dotazování. Však Avro je větší serializace a rekonstrukce, jehož výsledkem je nižší výkon kopie ve srovnání s textovém formátu. Rozhodování formátu souborů v celém Tok zpracování komplexně. Začínají u co formulář data je uložený ve zdroji dat ukládají nebo extrahovat ze externí systémy; Nejlepším formátem úložiště, analytické zpracování a dotazování; a v jakém formátu je možné exportovat data do marts dat pro vytváření sestav a vizualizace nástroje. Někdy formát souboru, který je nepřesnými pro čtení a zápis může být výkon Dobrá volba, pokud po zvažte procesu celkové analytické.

## <a name="considerations-for-compression"></a>Co zvážit při komprese
Je-li množiny dat vstupní a výstupní soubor, můžete nastavit kopírovat aktivity k provedení komprese nebo dekomprese jako zápisu dat do cíle. Při volbě komprese udělat kompromis mezi vstupní/výstupu a procesoru. Komprese navíc ve výpočetním zdroje dat náklady. Ale naopak snižuje síť vstupu a výstupu a úložiště. V závislosti na datech může se zobrazit zesílení v celkový výkon kopírovat.

**Kodek**: Kopírovat aktivity podporuje gzip, bzip2 a typy komprese Deflate. Azure HDInsight může používat všechny tři typy pro zpracování. Každý kodek komprese přináší následující výhody. Například bzip2 má nejnižší výkon kopie, ale získáte nejlepší možný výkon dotazu podregistru s bzip2, protože je můžete rozdělit pro zpracování. GZIP je možnost nejčastěji Rovnováha a pracovní postup slouží nejčastěji. Zvolte správný kodek, který se nejvíc hodí nefunguje začátku do konce.

**Úroveň**: můžete vybrat ze dvou možností pro každou kodek kompresi: nejrychleji komprimované a optimálně mu tuhle zkomprimovanou. Nejrychleji mu tuhle zkomprimovanou možnost zkomprimuje data co nejdříve, i když není optimálně komprimaci výsledné souboru. Možnost optimálně mu tuhle zkomprimovanou stráví delší dobu na komprese a výsledkem minimální množství dat. Můžete otestovat obou možnostech zobrazíte, která poskytuje lepší výkon v váš případ.

**Pozornost A**: zkopírovat velkého množství dat mezi místním úložišti a cloudem, zvažte použití úložiště objektů blob dočasné pomocí komprese. Použití dočasné úložiště je užitečný šířky pásma, jakou vaší podnikové sítě a služby Azure omezující faktor, a chcete vstupní uvedenou množinu dat i uvedenou množinu dat výstupu v nekomprimované formě. Konkrétně můžete přerušíte aktivitu jednu kopii do dvou kopírovat aktivity. První činnosti kopírovat zkopíruje ze zdroje pomocného nebo pracovní blob mu tuhle zkomprimovanou formuláře. Druhé činnosti kopírovat zkopíruje zkomprimovaná data z pracovní a potom dekomprimuje během zapíše do jímce.

## <a name="considerations-for-column-mapping"></a>Co zvážit při mapování sloupců
Nastavte vlastnost **columnMappings** činnosti kopie na mapu všechny nebo podmnožiny vstupní sloupců tak, aby výstupní sloupce. Po službu pohyb dat načte data ze zdroje, je nutné provést mapování sloupce v data před zapíše data do jímce. Tato další zpracování snižuje výkon kopírovat.

Pokud zdrojová data obsahují je jako dotazovatelné, například, je-li relační úložiště jako SQL databáze nebo SQL Server, nebo pokud je úložištěm NoSQL jako úložiště tabulek nebo DocumentDB, zvažte vložení filtrování sloupce a změna uspořádání logiky do vlastnosti **dotazu** místo použití mapování sloupců. Tímto způsobem promítání probíhá během službu pohyb dat načte data z úložiště zdroj dat, kde je mnohem efektivnější.

## <a name="considerations-for-data-management-gateway"></a>Důležité informace týkající se Brána pro správu dat
Doporučení pro nastavení brány najdete v článku [co zvážit při použití Brána pro správu dat](data-factory-move-data-between-onprem-and-cloud.md#Considerations-for-using-Data-Management-Gateway).

**Prostředí počítači brány**: doporučujeme použít vyhrazené počítače na hostitele Brána pro správu dat. Pomocí nástrojů, jako je PerfMon zkoumat procesoru, paměti a šířky pásma použití během operace kopírování v počítači brány. Přejděte na výkonnější počítač Pokud procesoru a paměti, šířka pásma změní kritické.

**Spustí souběžné kopírovat aktivity**: jedna instance Brána pro správu dat si můžete vytisknout více se spustí kopírovat aktivity ve stejnou dobu nebo současně. Maximální počet souběžné úloh počítán podle konfigurace hardwaru, ve počítače brány. Další kopii úlohy jsou ve frontě až do doby odebrání brána nebo do jiného projektu vyprší její časový limit. Chcete-li předejít konflikty prostředků v počítači brány, můžete fáze plánu kopírovat aktivity omezit počet kopií úlohy ve frontě po jednom nebo zvažte možnost rozdělení načíst do více počítačů brány.


## <a name="other-considerations"></a>Další informace
Je-li velikost dat, která chcete zkopírovat velký, můžete upravit obchodní logiky další oddíl dat pomocí řezů mechanismus v Data Factory. Naplánujte kopírovat aktivity ke spuštění častěji zmenšit velikost dat pro jednotlivé kopie aktivity spustit.

Buďte opatrní počet datových sad a kopírovat aktivity nevyžadují Data Factory spojnice tak, aby stejném úložišti ve stejnou dobu. Mnoho úloh souběžné kopírovat může omezení úložiště dat a vést výkon kopírovat úlohy interní opakování a v některých případech spuštění chyby.

## <a name="sample-scenario-copy-from-an-on-premises-sql-server-to-blob-storage"></a>Ukázkový scénář: Kopírovat z místního SQL serveru k úložišti objektů Blob
**Scénář**: potrubí je integrovaná zkopírujte data z místního SQL serveru k úložišti objektů Blob ve formátu CSV. Urychlit Kopírovat projekt, jsou soubory CSV by měl komprimován do formátu bzip2.

**Test a analýza**: výkon kopírovat aktivity je menší než 2 MB / je mnohem nižší než srovnávacích výkonu.

**Analýza výkonu a ladění**: řešení problémů s výkonem, Pojďme se podívat na způsobu zpracování a přesunout data.

1.  **Číst data**: brány otevře připojení k serveru SQL Server a odešle dotaz. SQL Server odpoví tak, že datového proudu brány prostřednictvím podnikové sítě.
2.  **Data serializovatelnou a komprimovat**: brány jsou toku dat ve formátu CSV a zkomprimuje data, která chcete bzip2 proudu.
3.  **Zápis dat**: Brána odešle proudu bzip2 k úložišti objektů Blob přes Internet.

Jak je vidět, která má být data zpracování a přesouvat způsobem, datových proudů sekvenční: SQL Server > LAN > brány > WAN > úložiště objektů Blob. **Celkový výkon jsou závislé na minimální výkon přes kanálu**.

![Toku dat](./media/data-factory-copy-activity-performance/case-study-pic-1.png)

Nejméně jeden z následujících skutečností může způsobit kritický výkon:

-   **Zdroje**: SQL Server má zhoršeným výkon kvůli vysoké zatížení.
-   **Brána pro správu dat**:
    -   **LAN**: brány je vzdáleno počítače serveru SQL Server a je pomalé připojení.
    -   **Brána**: brány dosáhl jeho omezení zatížení dělat následující operace:
        -   **Serializace**: serializaci toku dat ve formátu CSV obsahuje pomalé výkon.
        -   **Komprese**: jste se rozhodli je kodek pomalé komprese (například bzip2, což je 2,8 MB / s základní i7).
    -   **WAN**: nedostatku šířku pásma mezi podnikovou síť a služby Azure (například T1 = 1,544 kB/s; T2 = 6,312 kb/s).
-   **Dřez**: úložiště objektů Blob má zhoršeným výkon. (Tento scénář je pravděpodobné, protože jeho SLA zaručuje aspoň 60 MB/s.)

V tomto případě komprese bzip2 dat může zpomalovat za celou kanálu. Přechod na je kodek komprese gzip může usnadnění tento kritické.


## <a name="sample-scenarios-use-parallel-copy"></a>Ukázka scénáře: použití paralelní kopie  

**Scénář I:** Zkopírujte 1 000 1 MB soubory ze systému souborů místní k úložišti objektů Blob.

**Analýza a ladění výkonu**: příklad, pokud jste nainstalovali brány na počítač čtyřstrannou základní Data Factory používá 16 paralelní kopie přesunutí souborů ze systému souborů k úložišti objektů Blob současně. Paralelní spuštění by měl mít za následek vysoký výkon. Můžete také explicitně zadat počet kopií paralelní. Když zkopírujete mnoho malé soubory, paralelní kopií výrazně pomáhají výkon mnohem efektivněji uskutečňovat pomocí zdroje.

![Scénář 1](./media/data-factory-copy-activity-performance/scenario-1.png)

**Scénář II**: zkopírovat 20 objektů BLOB 500 MB z úložiště objektů Blob dat jezera úložiště technologie pro analýzu a ladění výkonu.

**Analýza a ladění výkonu**: V tomto scénáři Data Factory slouží ke kopírování dat z úložiště objektů Blob úložišti jezera dat pomocí jedné kopie (**parallelCopies** ve nastavena na hodnotu 1) a jednotky pohyb jednoduchým cloudu dat. Výkon, které můžete sledovat budou zavřít, popsány v [části referenční výkon](#performance-reference).   

![Scénář 2](./media/data-factory-copy-activity-performance/scenario-2.png)

**Scénář III**: jednotlivých souborů velikost je větší než desítky MB a celkový objem je velká.

**Analýza a výkonu zapnutí**: zvětšení **parallelCopies** nemá za následek lepší výkon kopírovat z důvodu omezení prostředků DMU jednoduchým cloudu. Místo toho je nutné zadat další cloudu DMUs získat další materiály k přesouvat data. Není zadejte hodnotu pro vlastnost **parallelCopies** . Data Factory pracuje paralelismus za vás. V tomto případě Pokud nastavíte **cloudDataMovementUnits** 4, výkon z asi čtyřikrát opakuje.

![Scénář 3](./media/data-factory-copy-activity-performance/scenario-3.png)

## <a name="reference"></a>Odkaz
Tady je sledování výkonu a optimalizace odkazy pro některé z úložiště podporované dat:

- Azure úložiště (včetně úložiště objektů Blob úložiště tabulek): [úložišti Azure škálovatelnost cílů](../storage/storage-scalability-targets.md) a [úložišti Azure výkon a škálovatelnost kontrolního seznamu](../storage//storage-performance-checklist.md)
- Databáze SQL Azure: Máte tyto možnosti [Sledování výkonu](../sql-database/sql-database-service-tiers.md#monitoring-performance) a zaškrtněte políčko procento databáze transakce jednotkové (DTU)
- Azure SQL datový sklad: Jeho funkce měří se v dat skladové jednotky (DWUs); v tématu [Správa výpočet power v Azure SQL datový sklad (přehled)](../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md)
- Azure DocumentDB: [úrovně výkonu při DocumentDB](../documentdb/documentdb-performance-levels.md)
- Místní SQL Server: [sledování a ladění výkonu](https://msdn.microsoft.com/library/ms189081.aspx)
- Místní souborový server: [ladění výkonu pro servery soubor](https://msdn.microsoft.com/library/dn567661.aspx)
