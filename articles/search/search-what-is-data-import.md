<properties
    pageTitle="Nahrajte dat Azure hledání | Microsoft Azure | Vyhledávací služby serveru hostovanou cloudu"
    description="Informace o odeslání dat do indexu vyhledávání Azure."
    services="search"
    documentationCenter=""
    authors="ashmaka"
    manager="jhubbard"
    editor=""
    tags=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="upload-data-to-azure-search"></a>Odeslání dat do hledání Azure
> [AZURE.SELECTOR]
- [Základní informace](search-what-is-data-import.md)
- [.NET](search-import-data-dotnet.md)
- [ZBÝVAJÍCÍ](search-import-data-rest-api.md)


## <a name="data-upload-models-in-azure-search"></a>Data ukládat modely v Azure hledání
Existují dva způsoby k naplnění Azure vyhledávacím odkazu s daty. První možnost je ručně předání dat do indexu vyhledávání Azure [Rozhraní REST API](search-import-data-rest-api.md) nebo [.NET SDK](search-import-data-dotnet.md). Druhou možností je tak, aby Azure vyhledávacím odkazu [přejděte podporovaný zdroj dat](search-indexer-overview.md) a zpřístupněte Azure hledání automaticky začlenit do vyhledávací služba vaše data.

Tato příručka pouze převezme pokyny týkající se použití modelu nabízených odeslání dat (což je podporována pouze v [Rozhraní REST API](search-import-data-rest-api.md) a [.NET SDK](search-import-data-dotnet.md)), ale pořád se můžou dozvědět víc o modelem vložit.

### <a name="push-data-to-an-index"></a>Zápis dat do indexu

Tento přístup odkazuje programově odesílání dat Azure hledání být k dispozici pro vyhledávání. Pro aplikace s požadavky na velmi nízký latence (například pokud potřebujete vyhledávání operací se nesynchronizují s databázemi dynamických zásob), model nabízených je vaše jediná možnost.

Můžete [Rozhraní REST API](https://msdn.microsoft.com/library/azure/dn798930.aspx) nebo [.NET SDK](search-import-data-dotnet.md) zápis dat do indexu. Je aktuálně nepodporuje nástroj pro vložení dat prostřednictvím portálu.

Tento přístup je flexibilnější než vyžádané model, protože můžete odesílat dokumenty jednotlivě nebo hromadně (až 1000 dávku nebo 16 MB, ať limit je v ní dřív). Model nabízených umožňuje nahrajte dokumenty do hledání Azure bez ohledu na to, kde jsou vaše data.

### <a name="pull-data-into-an-index"></a>Začlenit do indexu data

Model vyžádané procházení zdroje dat podporované a data se automaticky uloží do indexu vyhledávání Azure je za vás. Pomocí sledování změn a odstranění existující dokumenty kromě rozpoznávání nových dokumentů, odeberte indexování potřeba aktivně Správa dat v indexu.

V Azure vyhledávání tato funkce implementovaná prostřednictvím *indexování*, aktuálně dostupné [úložiště objektů Blob (verze preview)](search-howto-indexing-azure-blob-storage.md), [DocumentDB](http://aka.ms/documentdb-search-indexer) [databáze Azure SQL a SQL Server na Azure VMs](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md).

Funkce indexování se zobrazí na [Portál Azure](search-import-data-portal.md) stejně jako [Rozhraní REST API](https://msdn.microsoft.com/library/azure/dn946891.aspx).
