<properties 
    pageTitle="DocumentDB měřítko a testování výkonu | Microsoft Azure" 
    description="Zjistěte, jak provádět měřítko a výkonu testováním pomocí Azure DocumentDB"
    keywords="testování výkonu"
    services="documentdb" 
    authors="arramac" 
    manager="jhubbard" 
    editor="" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="arramac"/>

# <a name="performance-and-scale-testing-with-azure-documentdb"></a>Výkon a měřítko testováním pomocí Azure DocumentDB
Testování měřítko výkonu a je klíčové krokem při vývoj aplikací. Pro mnoho aplikací osy databáze obsahuje významný vliv na celkový výkon a škálovatelnost a tedy důležitou součástí testování výkonu. [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) je purpose-built pružná měřítko a předvídatelná výkon a tedy Skvělá volba pro aplikace, které potřebují osy výkonné databáze. 

Tento článek je odkaz pro vývojáře implementaci výkonu testování sad pro jejich pracovního vytížení DocumentDB nebo vyhodnocení DocumentDB výkonné aplikace scénářích. Se zaměřuje na testování izolace výkonu databáze, ale také doporučené postupy pro výrobní aplikace.

Po přečtení v tomto článku, budou moct odpovězte na následující otázky:   

- Kde najdu své ukázkové .NET klientské aplikaci pro testování výkonu z Azure DocumentDB? 
- Jak dosáhnout vysoký výkon úrovně s Azure DocumentDB z klientské aplikace

Začít s kódem, stáhněte si prosím projektu z [DocumentDB výkonu testování vzorku](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark). 

> [AZURE.NOTE] Cílem tuto aplikaci je ukazují doporučené postupy pro extrahování lepší výkon mimo DocumentDB s malým počtem poštovních klientských počítačích. Volání nebyl prokázat Špička objemu službu, kterou lze přizpůsobit limitlessly.

Pokud hledáte konfigurace na straně klienta možnosti zvýšení výkonu DocumentDB naleznete v tématu [Tipy pro DocumentDB výkon](documentdb-performance-tips.md).

## <a name="run-the-performance-testing-application"></a>Spusťte testování aplikace výkonu
Nejrychlejším způsobem, jak začít je kompilaci a spuštění ukázku .NET níže popsaných v následujících krocích. Můžete taky zkontrolujte zdrojového kódu a implementaci podobné konfigurací klientské aplikace.

**Krok 1:** Stažení projektu z [DocumentDB výkonu testování vzorku](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)nebo rozvětvené Github úložiště.

**Krok 2:** Změnit nastavení pro EndpointUrl AuthorizationKey, CollectionThroughput a DocumentTemplate (volitelné) do App.config.

> [AZURE.NOTE] Před zřizujete kolekci s vysoký výkon, získáte [Ceny stránky](https://azure.microsoft.com/pricing/details/documentdb/) k odhadu nákladů na kolekci. Ukládání DocumentDB faktury a výkon nezávisle na hodinu, takže můžete ušetřit náklady odstraněním nebo snížení výkon kolekcí DocumentDB po testování.

**Krok 3:** Kompilace a spusťte aplikaci konzoly z příkazového řádku. Měli byste vidět výstup takto:

    Summary:
    ---------------------------------------------------------------------
    Endpoint: https://docdb-scale-demo.documents.azure.com:443/
    Collection : db.testdata at 50000 request units per second
    Document Template*: Player.json
    Degree of parallelism*: 500
    ---------------------------------------------------------------------

    DocumentDBBenchmark starting...
    Creating database db
    Creating collection testdata
    Creating metric collection metrics
    Retrying after sleeping for 00:03:34.1720000
    Starting Inserts with 500 tasks
    Inserted 661 docs @ 656 writes/s, 6860 RU/s (18B max monthly 1KB reads)
    Inserted 6505 docs @ 2668 writes/s, 27962 RU/s (72B max monthly 1KB reads)
    Inserted 11756 docs @ 3240 writes/s, 33957 RU/s (88B max monthly 1KB reads)
    Inserted 17076 docs @ 3590 writes/s, 37627 RU/s (98B max monthly 1KB reads)
    Inserted 22106 docs @ 3748 writes/s, 39281 RU/s (102B max monthly 1KB reads)
    Inserted 28430 docs @ 3902 writes/s, 40897 RU/s (106B max monthly 1KB reads)
    Inserted 33492 docs @ 3928 writes/s, 41168 RU/s (107B max monthly 1KB reads)
    Inserted 38392 docs @ 3963 writes/s, 41528 RU/s (108B max monthly 1KB reads)
    Inserted 43371 docs @ 4012 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 48477 docs @ 4035 writes/s, 42282 RU/s (110B max monthly 1KB reads)
    Inserted 53845 docs @ 4088 writes/s, 42845 RU/s (111B max monthly 1KB reads)
    Inserted 59267 docs @ 4138 writes/s, 43364 RU/s (112B max monthly 1KB reads)
    Inserted 64703 docs @ 4197 writes/s, 43981 RU/s (114B max monthly 1KB reads)
    Inserted 70428 docs @ 4216 writes/s, 44181 RU/s (115B max monthly 1KB reads)
    Inserted 75868 docs @ 4247 writes/s, 44505 RU/s (115B max monthly 1KB reads)
    Inserted 81571 docs @ 4280 writes/s, 44852 RU/s (116B max monthly 1KB reads)
    Inserted 86271 docs @ 4273 writes/s, 44783 RU/s (116B max monthly 1KB reads)
    Inserted 91993 docs @ 4299 writes/s, 45056 RU/s (117B max monthly 1KB reads)
    Inserted 97469 docs @ 4292 writes/s, 44984 RU/s (117B max monthly 1KB reads)
    Inserted 99736 docs @ 4192 writes/s, 43930 RU/s (114B max monthly 1KB reads)
    Inserted 99997 docs @ 4013 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 100000 docs @ 3846 writes/s, 40304 RU/s (104B max monthly 1KB reads)

    Summary:
    ---------------------------------------------------------------------
    Inserted 100000 docs @ 3834 writes/s, 40180 RU/s (104B max monthly 1KB reads)
    ---------------------------------------------------------------------
    DocumentDBBenchmark completed successfully.


**Kroku 4 (v případě potřeby):** Výkon vykázaného (RU/ne) z nástroje se musí shodovat nebo vyšší než zřizování výkon kolekce. V opačném případě zvýšení DegreeOfParallelism po malých částech mohou vám pomoci zodpovědět limit. Pokud náhorních výkon z aplikace pro klienta plošinách spuštění víc instancí aplikace ve stejném nebo jiném počítačích se vám pomoci zodpovědět limit zřizování přes různé instancí. Pokud potřebujete pomoct s tímto krokem, napište e-mailu pro askdocdb@microsoft.com nebo jej vyplnil požadavek podpory můžete.

Až budete mít aplikaci spuštěná, můžete zkusit jiné [zásady indexování](documentdb-indexing-policies.md) a [konzistence úrovně](documentdb-consistency-levels.md) pochopit jejich dopad na výkon a latence. Můžete taky zkontrolujte zdrojového kódu a implementaci podobné konfigurace a vlastní testování sad produkční aplikace.

## <a name="next-steps"></a>Další kroky
V tomto článku jsme dívali jak je možné provést výkonu a měřítko testováním pomocí DocumentDB pomocí aplikace konzoly .NET. Získáte další informace o práci s DocumentDB na odkazy dole.

* [Testování vzorku DocumentDB výkonu](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)
* [Možnosti konfigurace klienta pro zvýšení výkonu DocumentDB](documentdb-performance-tips.md)
* [Serverový oddílů v DocumentDB](documentdb-partition-data.md)
* [Kolekce DocumentDB a výkonu úrovně](documentdb-performance-levels.md)
* [Na webu MSDN si přečtěte následující dokumentaci DocumentDB .NET SDK](https://msdn.microsoft.com/library/azure/dn948556.aspx)
* [DocumentDB .NET vzorky](https://github.com/Azure/azure-documentdb-net)
* [Blog DocumentDB na výkonu](https://azure.microsoft.com/blog/2015/01/20/performance-tips-for-azure-documentdb-part-1-2/)
