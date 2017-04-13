<properties
pageTitle="Indexování objektů BLOB CSV s indexování objektů blob Azure hledání | Microsoft Azure"
description="Zjistěte, jak chcete indexovat CSV objektů BLOB Azure hledání"
services="search"
documentationCenter=""
authors="chaosrealm"
manager="pablocas"
editor="" />

<tags
ms.service="search"
ms.devlang="rest-api"
ms.workload="search" ms.topic="article"  
ms.tgt_pltfrm="na"
ms.date="07/12/2016"
ms.author="eugenesh" />

# <a name="indexing-csv-blobs-with-azure-search-blob-indexer"></a>Indexování objektů BLOB CSV s indexování objektů blob Azure hledání 

Ve výchozím nastavení [indexování objektů blob Azure hledání](search-howto-indexing-azure-blob-storage.md) rozdělí s oddělovači objektů BLOB text jako jeden blok textu. Však s objekty BLOB obsahující data ve formátu CSV, často chcete zacházet s čárami v objektů blob jako samostatný dokument. Například mít takto oddělený textový: 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

Možná budete chtít analyzovat na 2 dokumenty, každý obsahující "identifikátor", "datePublished" a "značky" polí.

V tomto článku se dozvíte, jak analyzovat objektů BLOB CSV s indexování objektů blob Azure vyhledávání. 

> [AZURE.IMPORTANT] Tato funkce momentálně v náhledu. Je k dispozici pouze v rozhraní REST API pomocí verze **2015 02 28 náhled**. Mějte na paměti, náhled rozhraní API jsou určené pro testování a vyhodnocení a neměla používat ve provozním prostředí. 

## <a name="setting-up-csv-indexing"></a>Nastavení indexování CSV

Index objektů BLOB CSV, vytvoření nebo aktualizace definici indexování s `delimitedText` analýza režimu:  

    {
      "name" : "my-csv-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "firstLineContainsHeaders" : true } }
    }

Podrobné informace o rozhraní API indexování vytvořit najdete v článku [Vytvoření indexování](search-api-indexers-2015-02-28-preview.md#create-indexer).

`firstLineContainsHeaders`Označuje, že první řádek (Neprázdné) každý objektů blob obsahuje záhlaví.
Pokud objektů BLOB neobsahují linku počáteční záhlaví, záhlaví by měl být zadán v konfigurace indexování: 

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } } 

V současnosti je podporována pouze kódování UTF-8. Navíc pouze hodnoty `','` znak je podporované jako oddělovač. Pokud potřebujete podporu pro další kódování nebo oddělovače, dejte nám prosím vědět na [webu UserVoice](https://feedback.azure.com/forums/263029-azure-search).

> [AZURE.IMPORTANT] Při použití oddělený textový analýza režimu Azure hledání předpokládá, po které všechny objekty BLOB ve zdroji dat bude CSV. Pokud potřebujete podporu kombinaci CSV a -CSV objektů BLOB ve stejné zdroje dat, dejte nám prosím vědět na [webu UserVoice](https://feedback.azure.com/forums/263029-azure-search).

## <a name="request-examples"></a>Příklady požadavek

Vložení to všechny společně příkladů dokončení data. 

Zdroje dat: 

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<my storage connection string>" },
        "container" : { "name" : "my-container", "query" : "<optional, my-folder>" }
    }   

Indexování:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-csv-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } }
    }

## <a name="help-us-make-azure-search-better"></a>Pomozte nám vylepšit Azure hledání

Pokud máte poslat žádost o funkci nebo nápady pro vylepšení, prosím kontaktujte nás na [webu UserVoice](https://feedback.azure.com/forums/263029-azure-search/).