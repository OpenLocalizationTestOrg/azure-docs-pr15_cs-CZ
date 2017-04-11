<properties 
    pageTitle="Vozidla telemetrie analýzy řešení playbook | Microsoft Azure" 
    description="Použití funkcí Cortana Intelligence získat v reálném čase a prediktivní Další informace o stavu vozidla a řízení zvyky." 
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
    ms.date="09/12/2016" 
    ms.author="bradsev" />


# <a name="vehicle-telemetry-analytics-solution-playbook"></a>Vozidla telemetrie analýzy řešení playbook

Této **nabídce** odkazy na kapitoly v tomto playbook. 

[AZURE.INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

## <a name="overview"></a>Základní informace
Super počítače byly přesunuté mimo testovacím prostředí a jsou teď parkování v naší garáží! Tyto špičkové automobily obsahovat velkého počtu senzory jim umožňuje sledovat a monitorovat miliony událostí, každý druhý. Očekáváme, že tak, že 2020, nejčastěji tyto automobilů bude mít bylo připojení k Internetu. Představte si klepnutí do této množství dat za účelem zadání nejlepší v předmětu zabezpečení, spolehlivost a řízení! Microsoft připíše to sní skutečnosti s Cortana měřítka.

Cortana Intelligence společnosti Microsoft jsou plně spravovaných velký data a pokročilé technologie pro analýzu sadu, který umožňuje transformace dat do inteligentní akce. Chceme představí šablona Cortana Intelligence vozidla Telemetrie analýzy řešení. Toto řešení ukazuje, jak dealerům auta, výrobců automobilů a pojištění společnosti můžete použít funkce Cortana Intelligence získat v reálném čase a zvyky prediktivní Další informace o stavu vozidla a řízení. 

Řešení je jako [lambda architektura vzorek](https://en.wikipedia.org/wiki/Lambda_architecture) zobrazující potenciální platformy Cortana Intelligence pro úplné implementovaná v reálném čase a dávkové zpracování. Řešení: 

- poskytuje simulator telematika vozidla
- využití rozbočovače událostí pro ingesting miliony simulovaný vozidla telemetrie událostí do Azure 
- Použití analýzy toku získat v reálném čase Další informace o stavu vozidla
-  potrvají data do dlouhodobé úložiště pro bohatší dávku analýzy. 
- využívá výukové počítače ke zjištění odchylky v v reálném čase a dávkové zpracování získat prediktivní přehledy.
- využití HDInsight k transformaci dat ve velkém měřítku a Data Factory zpracovávání průběhu, plánování, Správa zdrojů a sledování kanálu dávkové zpracování 
- poskytuje tato řešení bohaté řídicí panel pro data v reálném čase a prediktivní analýzy vizualizací pomocí Power BI

## <a name="architecture"></a>Architektura

![](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)
*Obrázek 1 – architektury řešení analýzy Telemetrie vozidla*

Toto řešení obsahuje následující **komponenty Cortana Intelligence** a hodnotí jejich konce integrace


- **Událost rozbočovače** ingesting miliony vozidla telemetrie událostí do Azure.
- **Toku Analytics** pro získání v reálném čase přehledy na vozidla zdraví a trvá tato data do dlouhodobé úložiště pro bohatší dávku analýzy.
- **Výukové počítače** ke zjištění odchylky v reálném čase a dávkové zpracování získat prediktivní přehledy.
- **HDInsight** využít k transformaci dat ve velkém měřítku
- **Data Factory** zpracovává průběhu, plánování, Správa zdrojů a sledování kanálu dávkové zpracování.
- **Power BI** vám bude radit toto řešení bohaté řídicí panel pro data v reálném čase a prediktivní analýzy vizualizace.

Toto řešení přistupuje dvou různých **zdrojů dat**: 

- **Simulovaného vozidla signály a diagnostice**: simulator telematika vozidla posílá diagnostické informace a signály, které odpovídají stavu vozidla a řízení vzorek v daném okamžiku v čase. 
- **Katalog vozidla**: datovou sadu reference obsahující VIN modelu mapování.
