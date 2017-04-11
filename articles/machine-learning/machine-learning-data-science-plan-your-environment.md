<properties
    pageTitle="Jak určit scénáře a plánování rozšířené zpracování analýzy dat | Microsoft Azure"
    description="Vytvoření plánu pro pokročilé technologie pro analýzu v úvahu několik klíčových dotazů."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="bradsev" />


# <a name="how-to-identify-scenarios-and-plan-for-advanced-analytics-data-processing"></a>Jak určit plán pro zpracování pokročilé technologie pro analýzu dat a scénáře

K čemu zdroje měli byste naplánovat pracovat při nastavování prostředí dělat pokročilé technologie pro analýzu zpracování na datovou sadu? Tento článek navrhne řadu otázky, které vám pomůže identifikovat úkoly a zdroje relevantních nefunguje. Pořadí hlavních kroků pro prediktivní analýzy je uvedené v [Co je proces pro výzkum dat týmu (TDSP)?](data-science-process-overview.md). Každý z těchto kroků budou vyžadovat určitého zdroje pro úkoly týkající se určitého nefunguje. Klíčové otázky k identifikaci nefunguje týkají dat doprava, vlastnosti a kvalitu nástroje a jazyky, které chcete udělat analýzu a datové sady.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="logistic-questions-data-locations-and-movement"></a>Logistickou dotazy: umístění dat a pohybu
Logistickou otázky se týkají umístění **zdroje dat**, **směrovat cíle** v Azure a požadavky pro přesunutí dat, včetně plánu, velikosti a zdroje. Data může být nutné během analýzy přesouvat několikrát. Běžné situace je přesunout místních dat do některé formuláře úložiště na Azure a potom do počítače výukové Studio.

1. **Co je zdroj dat?** Je místní nebo v cloudu? Příklad:
    - Data jsou veřejně dostupný na adrese HTTP.
    - Data uložena v místní/síťové umístění souboru.
    - Data jsou v databázi SQL serveru.
    - Data uložená v kontejneru Azure úložiště

2. **Co je Azure cíl?** Kde musí přístup pro zpracování nebo modelování? Příklad:
    - Úložiště objektů Blob Azure
    - Databáze SQL Azure
    - SQL Server na Azure OM
    - HDInsight (Hadoop na Azure) nebo podregistru tabulek
    - Výukové Azure počítače
    - Připojit Azure virtuální pevných discích.

3. **Jak budete přesunout data?** V následujících tématech jsou zobrazené postupy a prostředky k dispozici pro jedí nebo načtení dat do spoustu různých úložiště a zpracování prostředí.

    -  [Načtení dat do úložiště prostředí pro analýzy](machine-learning-data-science-ingest-data.md)
    -  [Import dat školení do Azure počítače výukové Studio z různých zdrojů dat](machine-learning-data-science-import-data.md).

4. **Data musím přesunuty v pravidelných intervalech nebo změněny v průběhu migrace?** Zvažte použití Azure dat Factory (ADF) data potřebujete-li k poštovním průběžně, zejména pokud hybridní scénář, který má přístup k oběma místních a cloudové zdroje se jedná o nebo když data je zpracováván jako transakce nebo musí změnit nebo máte obchodní logiky do něj přidali v průběhu migrací. Další informace najdete v tématu [přesunutí dat ze serveru SQL místní na SQL Azure pomocí Azure Data Factory](machine-learning-data-science-move-sql-azure-adf.md)

5. **Kolik dat se mají přesunout do Azure?** Datové sady, které jsou obrovské nesmí překročit kapacitou určitých prostředí. Příklad v tématu diskuse limity velikosti pro počítač výukové Studio v další části. V takovém případě ukázkových dat lze během analýzy. Podrobné informace o tom, jak dolů ukázkové datové sady v různých prostředích Azure najdete v tématu [Ukázková data v procesu týmu dat pro výzkum](machine-learning-data-science-sample-data.md).


## <a name="data-characteristics-questions-type-format-and-size"></a>Otázky vlastnosti data: typ, formát a velikost
Tyto otázky mají klíčový plánování úložišti a zpracování prostředí, přičemž každý z nich jsou vhodné k různým typům dat a každý z nich má určitá omezení.

1. **Jaké typy dat?** Příklad:
    - Číselné
    - Kategorií
    - Řetězce
    - Binární

2. **Data formátování?** Příklad:
    - Oddělené čárkami (CSV) nebo oddělenými tabulátory (TSV) textových souborů
    - Komprimovaná nebo nekomprimované
    - Objekty BLOB Azure
    - Hadoop podregistru tabulek
    - Tabulky SQL serveru

2. **Jak velké jsou vaše data?**
    - Small: menší než 2GB
    - Střední: Větší než 2GB a menší než 10
    - Velké: Větší než 10GB

Prostředí Azure počítače výukové Studio přebírají například:

- Seznam formátů data a typy podporované v Azure počítače výukové studiu [dat formáty a datové typy podporované](machine-learning-data-science-import-data.md#data-formats-and-data-types-supported) v části.
- Informace o omezení dat pro Azure počítače výukové Studio najdete v tématu **jak velké uvedenou množinu dat lze pro svůj moduly?** část [Import a export dat pro výukové počítače](machine-learning-faq.md#machine-learning-studio-questions)

Informace o omezeních jiných Azure služby používané v procesu analýzy najdete v článku [Azure předplatné a omezení služby, kvót a omezení](../azure-subscription-service-limits.md).

## <a name="data-quality-questions-exploration-and-pre-processing"></a>Otázky kvality dat: průzkum a předzpracování

1. **Co byste vědět o datům?** Zkoumání dat, když potřebujete získat vysvětlení informací na jeho základní vlastnosti. Co vzorky nebo trendů předměty, co je odlehlé hodnoty obsahuje nebo chybí počet hodnot. Tento krok je důležité při určení rozsahu předzpracování potřeby zpracovávající hypotézy, které by mohly navrhnout funkci nejvhodnější nebo zadejte analýzy a zpracovávající plány pro shromažďování dat další. Analytický nástroj Popisná statistika výpočtu a zakreslení vizualizace způsoby užitečné pro kontrolu data. Podrobné informace o tom, jak zkoumat datovou sadu v různých prostředích Azure najdete v tématu [zkoumání dat v procesu týmu dat pro výzkum](machine-learning-data-science-explore-data.md).

2. **Vyžaduje data předzpracování nebo čištění?**
Předzpracování a čištění dat jsou důležité úkoly, které obvykle se musí provést před datovou sadu lze efektivně výukové počítače. Neformátovaná data je často vysokou úroveň šumu a nedůvěryhodných a by mohla chybět hodnoty. Pomocí těchto dat pro modelování můžete výsledkům zavádějící. Popis najdete v článku [výukové úkoly Příprava dat pro lepší počítač](machine-learning-data-science-prepare-data.md).

## <a name="tools-and-languages-questions"></a>Jazyky a nástroje otázky
Existuje spoustu možností podle toho, jaké jazyky a vývojové prostředí nebo nástroje potřebujete nebo jsou nejčastěji conformable pomocí.

1. **Jaké jazyky chcete použít pro analýzu?**  
    - R
    - Python
    - SQL

2. **Jaké nástroje mám použít k analýze dat?**
    - [Microsoft Azure Powershell](powershell-install-configure.md) - jazykem skript sloužící ke správě Azure zdrojů v jazyce skriptu.
    - [Azure počítače výukové Studio](machine-learning-what-is-ml-studio/)
    - [Otočení analýzy](http://www.revolutionanalytics.com/revolution-r-open)
    - [RStudio](http://www.rstudio.com)
    - [Python Tools for Visual Studio](http://microsoft.github.io/PTVS/)
    - [Anaconda](https://www.continuum.io/why-anaconda)
    - [Poznámkové bloky Jupyter](http://jupyter.org/)
    - [Microsoft Power BI](http://powerbi.microsoft.com)


## <a name="identify-your-advanced-analytics-scenario"></a>Určení pokročilé technologie pro analýzu nefunguje
Jakmile máte zodpovězené otázky v předchozí části, budete chtít zjistit, jaký scénář Nejlepší vešel váš případ. Scénáře výběru jsou zobrazené v [scénáře pro pokročilé technologie pro analýzu Azure počítač přečíst](machine-learning-data-science-plan-sample-scenarios.md).
