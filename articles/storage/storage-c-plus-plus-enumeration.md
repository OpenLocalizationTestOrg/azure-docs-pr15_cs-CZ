<properties
    pageTitle="Seznam zdrojů Azure úložiště s knihovnou Microsoft Azure úložiště klienta pro C++ | Microsoft Azure"
    description="Naučte se používat výpisu rozhraní API knihovnou Microsoft Azure úložiště klienta pro C++ udělat výčet kontejnery objektů BLOB, fronty, tabulky a entity."
    documentationCenter=".net"
    services="storage"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>
<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>

# <a name="list-azure-storage-resources-in-c"></a>Seznam zdrojů Azure úložiště v C++

Seznam operace mají klíčový mnoho scénářů vývoje s úložištěm Azure. Tento článek popisuje co nejefektivněji umožňuje zobrazit výčet objektů v úložišti Azure pomocí seznamu rozhraní API pro C++ uvedený v knihovně Microsoft Azure úložiště klienta.

>[AZURE.NOTE] Tato příručka cílovou knihovnu Azure úložiště klienta pro verzi C++ 2.x, což je k dispozici prostřednictvím [NuGet](http://www.nuget.org/packages/wastorage) nebo [GitHub](https://github.com/Azure/azure-storage-cpp).

Knihovně úložiště klienta nabízí řadu způsobů seznamu či dotazu objektů v úložišti Azure. Tento článek se zaměřuje na tyto scénáře:

-   Kontejnery seznam na účtu
-   Seznam objektů BLOB v kontejneru nebo virtuální objektů blob adresáře
-   Seznam fronty na účtu
-   Seznam tabulek na účtu
-   Entity dotazu v tabulce

Každá z těchto metod se zobrazí při použití různých přetížení různých scénářích.

## <a name="asynchronous-versus-synchronous"></a>Asynchronní versus synchronní

Protože nakonec knihovně úložiště klienta pro C++ nad [C++ ZBÝVAJÍCÍ knihovny](https://github.com/Microsoft/cpprestsdk)podporujeme podstatě asynchronní operace pomocí [pplx::task](http://microsoft.github.io/cpprestsdk/classpplx_1_1task.html). Příklad:

    pplx::task<list_blob_item_segment> list_blobs_segmented_async(continuation_token& token) const;

Obrázek operace zalomit odpovídající asynchronní operace:

    list_blob_item_segment list_blobs_segmented(const continuation_token& token) const
    {
        return list_blobs_segmented_async(token).get();
    }

Pokud pracujete s několika využívaný aplikací nebo služeb, doporučujeme použít asynchronní rozhraní API přímo místo abyste vytvářeli podproces volání synchronizace rozhraní API, která výrazně ovlivňuje výkon.

## <a name="segmented-listing"></a>Segmentovaný seznam

Měřítka cloudového úložiště vyžaduje segmentovaný seznam. Můžete třeba mít přes objektů BLOB miliónů v kontejneru objektů blob Azure nebo přes miliard entity v tabulce Azure. Nejsou teoretický čísla, ale případy použití skutečných zákazníků.

Je proto nepraktické seznam všech objektů jedinou odpověď. Místo toho můžete vytvořit seznam objektů pomocí stránkování. Má každá výpisu rozhraní API *segmentována* přetížení.

Odpověď na operace segmentovaný seznam obsahuje:

-   <i>_segment</i>, která obsahuje sadu výsledky vrácené jedné volání k rozhraní API seznam.
-   *continuation_token*, které se předávají funkcím další volání, abyste mohli získávat na další stránku s výsledky. Když nejsou žádné další výsledky vrátit, token pokračování je null.

Pro typické zavolat na seznam všechny objekty BLOB v kontejneru může vypadat například následující fragment kódu. Kód je k dispozici v naší [Ukázky](https://github.com/Azure/azure-storage-cpp/blob/master/Microsoft.WindowsAzure.Storage/samples/BlobsGettingStarted/Application.cpp):

    // List blobs in the blob container
    azure::storage::continuation_token token;
    do
    {
        azure::storage::list_blob_item_segment segment = container.list_blobs_segmented(token);
        for (auto it = segment.results().cbegin(); it != segment.results().cend(); ++it)
    {
        if (it->is_blob())
        {
            process_blob(it->as_blob());
        }
        else
        {
            process_diretory(it->as_directory());
        }
    }

        token = segment.continuation_token();
    }
    while (!token.empty());

Všimněte si, že počet výsledky na stránce můžete ho řídil parametr *max_results* v přetížení každý rozhraní API, například:

    list_blob_item_segment list_blobs_segmented(const utility::string_t& prefix, bool use_flat_blob_listing,
        blob_listing_details::values includes, int max_results, const continuation_token& token,
        const blob_request_options& options, operation_context context)

Pokud nezadáte parametr *max_results* , vrátí se výchozí maximální hodnota až 5 000 výsledků v jedné stránky.

Navíc nezapomeňte, že dotaz na úložiště tabulek Azure může vrátit žádné záznamy nebo méně záznamů než hodnota parametru *max_results* , který jste zadali, i když token pokračování není prázdný. Jeden důvod může být dotaz nelze dokončit pět sekund. Dokud token pokračování není prázdné, pokračovat dotaz a kód neměli předpokládá velikost segmentu výsledků.

Doporučené kódování vzor pro většinu scénářů je rozdělena záznam, který obsahuje explicitní průběhu výpis nebo dotazy a jak reagovat na každého žádosti o službu. Zejména pro C++ aplikací nebo služeb mohou pomoci nižší úrovně ochrany průběh výpis paměti ovládací prvek a výkonu.

## <a name="greedy-listing"></a>Hladový seznam

Předchozích verzí aplikace v knihovně úložiště klienta C++ (verze 0.5.0 náhled a starší) zahrnuté-segmentována výpis rozhraní API pro tabulky a fronty jako v následujícím příkladu:

    std::vector<cloud_table> list_tables(const utility::string_t& prefix) const;
    std::vector<table_entity> execute_query(const table_query& query) const;
    std::vector<cloud_queue> list_queues() const;

Jako obálky Segmentovaný rozhraní API byly implementovaná těchto postupů. Pro každou odpověď Segmentovaný výpis kód přidaným výsledky k vektor a vráceny všechny výsledky po celou kontejnery byly zkontrolovány.

Tento postup může pracovat, když na úložiště klienta nebo tabulka obsahuje malým počtem poštovních objekty. Však se zvýšením počet objektů, paměti požadovaného může zvýšit bez omezení, protože všechny výsledky remained v paměti. Jeden seznam operace může trvat velmi dlouho, po kterou volající měli žádné informace o jejím zpracování.

Tyto hladový výpis rozhraní API v SDK nejsou v C# Java a JavaScript Node.js prostředí. Chcete-li předejít problémům použití těchto hladový rozhraní API, jsme je odebraný ve verzi 0.6.0 náhled.

Pokud váš kód volá tyto hladový rozhraní API:

    std::vector<azure::storage::table_entity> entities = table.execute_query(query);
    for (auto it = entities.cbegin(); it != entities.cend(); ++it)
    {
        process_entity(*it);
    }

Potom měli upravit kód použít výpisu Segmentovaný rozhraní API:

    azure::storage::continuation_token token;
    do
    {
        azure::storage::table_query_segment segment = table.execute_query_segmented(query, token);
        for (auto it = segment.results().cbegin(); it != segment.results().cend(); ++it)
        {
            process_entity(*it);
        }

        token = segment.continuation_token();
    } while (!token.empty());

Zadáním parametru *max_results* segmentu můžete zůstatek mezi čísly žádosti a spotřebu paměti plnit důležité informace o výkonu aplikace.

Kromě toho, pokud používáte Segmentovaný výpis rozhraní API, ale data uložit do místního kolekce ve stylu "hladový", také důrazně doporučujeme Refaktorovat kódu pro zpracování ukládání dat v kolekci místní pečlivě ve velkém měřítku.

## <a name="lazy-listing"></a>Ďábelské seznam

Sice hladový výpis mocninu potenciálních problémů, je vhodné, pokud nejsou žádná příliš mnoho objektů v kontejneru.

Pokud používáte také C# nebo Oracle Java SDK, měli byste vědět s vyčíslitelné programovací model, který nabízí ďábelské stylu seznamu, kde data u některých odsazení pouze načtení v případě potřeby. V C++ šabloně založený na iterace také poskytuje podobný přístup.

Typické ďábelské seznam rozhraní API, pomocí **list_blobs** jako příklad vypadat takto:

    list_blob_item_iterator list_blobs() const;

Typické kódu, který používá vzorek pustí výpis může vypadat takto:

    // List blobs in the blob container
    azure::storage::list_blob_item_iterator end_of_results;
    for (auto it = container.list_blobs(); it != end_of_results; ++it)
    {
        if (it->is_blob())
        {
            process_blob(it->as_blob());
        }
        else
        {
            process_directory(it->as_directory());
        }
    }

Všimněte si, že pustí seznam je k dispozici pouze v synchronní režimu.

Porovnání s hladový seznam, ďábelské výpis načte data pouze v případě potřeby. Na pozadí je načte data z Azure úložiště pouze v případě, že další iterace přejde do další segment. Proto spotřebu paměti řízeno s velikostí ohraničenou a operace je rychlý.

Ďábelské výpis rozhraní API jsou součástí v knihovně úložiště klienta pro C++ verze 2.2.0.

## <a name="conclusion"></a>Uzavření

V tomto článku jsme popisované různých přetížení záznam rozhraní API pro jednotlivé objekty v knihovně úložiště klienta C++. Shrnutí:

-   Rozhraní API asynchronní důrazně doporučujeme v části více vláken scénáře.
-   Segmentovaný seznam se doporučuje většina scénářích.
-   Pustí seznam je k dispozici v knihovně jako pohodlný obálky v synchronní scénáře.
-   Hladový seznam se nedoporučuje a byla odebrána z knihovny.

##<a name="next-steps"></a>Další kroky

Další informace o úložišti Azure a knihovny klienta pro C++ naleznete v následujících zdrojích.

-   [Použití úložiště objektů Blob z C++](storage-c-plus-plus-how-to-use-blobs.md)
-   [Použití úložiště tabulek z C++](storage-c-plus-plus-how-to-use-tables.md)
-   [Použití úložiště fronty z C++](storage-c-plus-plus-how-to-use-queues.md)
-   [Knihovna klienta Azure úložiště si přečtěte následující dokumentaci C++ API](http://azure.github.io/azure-storage-cpp/)
-   [Blog týmu Azure úložiště](http://blogs.msdn.com/b/windowsazurestorage/)
-   [Si přečtěte následující dokumentaci Azure úložiště](https://azure.microsoft.com/documentation/services/storage/)
