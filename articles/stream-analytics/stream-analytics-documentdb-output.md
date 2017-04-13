<properties
    pageTitle="Výstup JSON pro analýzy toku | Microsoft Azure"
    description="Zjistěte, jak toku analýzy Azure DocumentDB obrázku pro JSON výstup pro archivaci dat nebo Nízká latence dotazů Nestrukturovaná data JSON."
    keywords="Výstup JSON"
    documentationCenter=""
    services="stream-analytics,documentdb"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="target-azure-documentdb-for-json-output-from-stream-analytics"></a>Cíl Azure DocumentDB pro JSON výstup toku analýzy

Technologie pro analýzu toku můžete zacílit [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) pro JSON výstup, povolení archivace a nízkou latencí dotazy na data na Nestrukturovaná data JSON. Zjistěte, jak nejlépe implementovat Tato integrace.

Uživatelům, kteří jsou DocumentDB ještě neznáte podívejte se [na DocumentDB cesta výuky](https://azure.microsoft.com/documentation/learning-paths/documentdb/) začít pracovat.

## <a name="basics-of-documentdb-as-an-output-target"></a>Základní informace o DocumentDB jako cílový výstup
Výstup Azure DocumentDB v toku analýzy, který umožňuje zapisovat vašeho proudu zpracování výsledky jako výstup JSON do kolekce DocumentDB. Technologie pro analýzu toku nevytvoří kolekce v databázi, místo toho bylo potřeba vytvářet předem. Je to tak, aby fakturační náklady DocumentDB kolekce průhledné pro vás a tak, že můžete optimalizovat výkon, konzistenci a kapacita vaše kolekce přímo pomocí rozhraní [API DocumentDB](https://msdn.microsoft.com/library/azure/dn781481.aspx). Doporučujeme použít jednu databázi DocumentDB na streamování projekt logicky oddělit vaše kolekce streamování projektu.

Některé z možností kolekce DocumentDB jsou podrobně uvedeny níže.

## <a name="tune-consistency-availability-and-latency"></a>Ladění konzistence dostupnost a latence

Odpovídá vašim požadavkům aplikace DocumentDB vám umožní graf doladit databáze a kolekcí a zkontrolujte střídání mezi konzistence, dostupnost a latence. Podle toho, jaké úrovně čtení konzistence vašim potřebám scénář proti čtení a zápis latence, že vyberte úroveň konzistence ve vašem účtu databáze. Ve výchozím nastavení DocumentDB umožňuje synchronní indexování na každé operace CRUD pro vaši kolekci. Toto je další užitečné možností pro řízení výkonu zápisu/přečtené v DocumentDB. Další informace o tomto tématu přečtěte si článek [Změna úrovně konzistence databáze a dotazů](../documentdb/documentdb-consistency-levels.md) .

## <a name="choose-a-performance-level"></a>Vyberte úroveň výkonu

Kolekce DocumentDB se dají vytvořit na úrovni 3 různých výkon (S1, S2 nebo S3) určující výkon k dispozici pro CRUDs ní. Kromě toho je tak, že indexování/konzistence úrovně v kolekci dopad na výkon. Naleznete [v tomto článku](../documentdb/documentdb-performance-levels.md) Principy úrovní tyto výkonu podrobně.

## <a name="upserts-from-stream-analytics"></a>Upserts z toku analýzy

Integrace analýzy toku s DocumentDB umožňuje vložit nebo aktualizovat záznamy do kolekci DocumentDB na základě zadaného sloupce ID dokumentu. Tím se taky nazývá *Upsert*.

Technologie pro analýzu toku využívá optimistické Upsert přístupu k, kde aktualizací jenom dělají při vložení selže kvůli konflikt ID dokumentu. Tuto aktualizaci provádí toku analýzy jako opravu, takže umožňuje částečné aktualizace dokumentu, tedy přidání nové vlastnosti nebo nahrazení existující vlastnost je provedena postupně. Všimněte si, že není sloučené změny mezi hodnotami ve vlastnosti pole v vaší JSON dokumentu za následek celé pole začíná přepsat, tedy pole.

## <a name="data-partitioning-in-documentdb"></a>Data oddílů v DocumentDB

Kolekce DocumentDB umožňují oddílů dat na základě vzorce dotazu a výkonu potřebám vaší aplikace. Jednotlivé kolekce může obsahovat až 10GB dat (maximálně) a současné době nejde žádným způsobem škálování (nebo přetečení) kolekce. Rozšiřování, toku analýzy můžete zapisovat do víc kolekcí dané předponou (viz podrobnostmi používání níže). Technologie pro analýzu toku používá konzistentní [Hash oddíl překládání](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.partitioning.hashpartitionresolver.aspx) strategie podle uživatelem zadaná PartitionKey sloupec oddíl jeho výstup záznamy. Počet kolekcí s předponou dané při spuštění streamování úlohy slouží jako výstup počet oddílů, ke kterému úlohy zapíše paralelně (DocumentDB kolekcí = výstup oddíly). Pro jeden S3 kolekce s pustí indexování tím pouze vloží, o 0.4 MB/s zápisu výkon můžete očekávat. Použití více kolekcí můžete povolit dosažení vyšší výkon a lepší výkon.

Pokud chcete zvýšit počet oddílů v budoucnu, budete muset ukončení práce, oddíly data z existující kolekce do nové kolekce a restartujte úlohy toku analýzy. Další informace o používání PartitionResolver a znovu rozdělení spolu s ukázkový kód, zahrne do příspěvku zpracování. Článek [oddílů v DocumentDB](../articles/documentdb-partition-data.md#developing-a-partitioned-application) také poskytuje podrobné informace o to.

## <a name="documentdb-settings-for-json-output"></a>Nastavení DocumentDB JSON výstup

Vytvoření DocumentDB jako výstup v toku analýzy vygeneruje výzva informace, jak je vidět níže. Tato část obsahuje vysvětlení definici vlastnosti.

![documentdb toku analýzy výstup obrazovky](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output.png)  

-   **Výstup Alias** – alias odkazoval tento výstup dotazu ASA  
-   **Název účtu** – název nebo identifikátor URI účtu DocumentDB koncového bodu.  
-   **Klíč účtu** – sdílené přístupová klávesa DocumentDB účtu.  
-   **Databáze** – DocumentDB název databáze.  
-   **Vzor názvu kolekce** – vzorek název kolekce Collections se nemusí používat. Formát názvu kolekce lze vytvořit pomocí tokenu volitelné {oddíl}, kde oddíly začít od 0. Následují platné vstupy vzorku:  
   1\) kolekce – musí existovat jedna kolekce s názvem "Kolekce".  
   2\) kolekce {oddíl} – například kolekce musí existovat – "MyCollection0", "MyCollection1", "MyCollection2" a tak dál.  
-   **Klíč oddílu** – název pole v událostech výstup slouží k určení klíč pro rozdělení výstup v kolekcích. K výstupu jednoho kolekce libovolného sloupce libovolného výstup lze použít například ID oddílu.  
-   **ID dokumentu** – volitelné. Název pole v událostech výstup slouží k určení toho, kterou vložit nebo aktualizace jsou operace založené primární klíč.  
