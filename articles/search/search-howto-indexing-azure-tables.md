<properties
pageTitle="Indexování úložiště tabulek Azure pomocí Azure hledání"
description="Zjistěte, jak chcete indexovat data uložená v tabulkách Azure pomocí funkce hledání Azure"
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
ms.date="08/16/2016"
ms.author="eugenesh" />

# <a name="indexing-azure-table-storage-with-azure-search"></a>Indexování úložiště tabulek Azure pomocí Azure hledání

Tento článek ukazuje, jak pomocí funkce Hledat Azure index dat uložených v úložišti tabulek Azure. Rychlé a bezproblémové díky nové tabulky indexování hledání Azure tento proces. 

> [AZURE.IMPORTANT] Tato funkce je v současné době v náhledu. Je k dispozici pouze v rozhraní REST API pomocí verze **2015 02 28 náhled** a verze 2.0 – náhled .NET SDK. Mějte na paměti, náhled rozhraní API jsou určené pro testování a vyhodnocení a neměla používat ve provozním prostředí.

## <a name="setting-up-azure-table-indexing"></a>Nastavení indexování tabulek Azure

Nastavení a konfigurace indexování tabulek Azure, můžete rozhraní REST API pro hledání Azure můžete vytvořit a spravovat **indexování** a **zdrojů dat** podle popisu v [operace indexování](https://msdn.microsoft.com/library/azure/dn946891.aspx). Můžete taky použít [verze 2.0 – náhled](https://msdn.microsoft.com/library/mt761536%28v=azure.103%29.aspx) .NET SDK. Podpora v budoucnu indexování tabulky se přidá k portálu Azure.

Zdroj dat určuje, která data indexovat přihlašovací údaje pro přístup k datům a zásady podporující nástroj Azure vyhledávání a pokusí se efektivní určení změn v řádcích dat (nový, upravit nebo odstranit řádky).

Indexování načte data ze zdroje dat a načte do indexu vyhledávání cílové.

Nastavení indexování tabulky:

1. Vytvoření zdroje dat
    - Nastavit `type` parametr`azuretable`
    - Předání v připojovacím řetězci účtu úložiště jako `credentials.connectionString` parametrů
    - Zadejte název tabulky pomocí `container.name` parametrů
    - Můžete taky zadat dotaz pomocí `container.query` parametr. Kdykoli je to možné, použijte filtr na PartitionKey pro dosažení nejlepších výsledků; Další dotazu způsobí prohledání celé tabulky, což může způsobit snížení výkonu pro velké tabulky.
2. Vytvoření indexu vyhledávání s schéma, které odpovídá sloupců v tabulce, které chcete indexovat. 
3. Vytvoříte připojení zdroje dat do indexu vyhledávání indexování.

### <a name="create-data-source"></a>Vytvoření zdroje dat

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "table-datasource",
        "type" : "azuretable",
        "credentials" : { "connectionString" : "<my storage connection string>" },
        "container" : { "name" : "my-table", "query" : "PartitionKey eq '123'" }
    }   

Další informace o rozhraní API vytvořit zdroj dat najdete v tématu [Vytvoření zdroje dat](search-api-indexers-2015-02-28-preview.md#create-data-source).

### <a name="create-index"></a>Vytvoření indexu 

    POST https://[service name].search.windows.net/indexes?api-version=2015-02-28
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-target-index",
        "fields": [
            { "name": "key", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "SomeColumnInMyTable", "type": "Edm.String", "searchable": true }
        ]
    }

Další informace o rozhraní API vytvořit Index najdete v článku [Vytvoření indexu](https://msdn.microsoft.com/library/dn798941.aspx)

### <a name="create-indexer"></a>Vytvoření rejstříku 

Nakonec vytvořte indexování odkazující na zdroj dat a cílové index. Příklad:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "table-indexer",
      "dataSourceName" : "table-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }

Podrobné informace o rozhraní API indexování vytvořit najdete v článku [Vytvoření indexování](search-api-indexers-2015-02-28-preview.md#create-indexer).

To je vše není nutná - spokojení indexování!

## <a name="dealing-with-different-field-names"></a>Práce s názvy různých polí

Často názvů polí v existující index se lišit od názvy vlastností v tabulce. **Mapování polí** můžete použít pro mapování názvů tyto vlastnosti z tabulky názvy polí ve vyhledávacím odkazu. Další informace o mapování polí najdete v tématu [mapování polí indexování hledání Azure most rozdíly mezi zdrojů dat a hledat indexy](search-indexer-field-mappings.md).

## <a name="handling-document-keys"></a>Zpracování dokumentu klíče

Do pole hledání Azure klíč dokumentu jednoznačně identifikuje dokumentu. Každý indexu vyhledávání může mít právě jeden klíčové pole typu `Edm.String`. Pole s klíčem je potřebný pro každý dokument, který se přidá do indexu (ve skutečnosti je pouze požadovaná pole).

Protože tabulky řádky mají složeného klíče, Azure hledání vygeneruje syntetické pole s názvem `Key` , která je tvořen oddílu spolu s řádku hodnoty klíče. Pokud řádek PartitionKey je například `PK1` a RowKey `RK1`, pak `Key` hodnotu pole bude `PK1RK1`. 

> [AZURE.NOTE] `Key` Hodnota může obsahovat znaky, které jsou neplatné v dokumentu klávesy, třeba typ čáry. Neplatné znaky můžete řešit pomocí `base64Encode` [funkce mapování polí](search-indexer-field-mappings.md#base64EncodeFunction). Pokud to uděláte, nezapomeňte také použít ve formátu Base 64 XLL s podporou URL kódování při předáním klíčů dokumentu v rozhraní API volá například vyhledávání.

## <a name="incremental-indexing-and-deletion-detection"></a>Přírůstková indexování a odstranění duplicit
 
Když nastavíte indexování tabulky spuštění podle plánu, ho Reindexuje pouze nový nebo aktualizovaný řádků podle na řádku `Timestamp` hodnotu. Nemáte k určení zjišťování zásad změnit – přírůstková je povoleno indexování za vás automaticky. 

Vyznačení, že některé dokumenty potřeba ji nejdřív odebrat z indexu pomocí měkké odstranit strategie – místo odstranění řádku, můžete přidat vlastnost označující, že je odstranit a nastavit zásady zjišťování měkké odstranění ve zdroji dat. Zásady ukázáno v následujícím příkladu se Zvažte například, že je odstranění řádku, pokud je vlastnost `IsDeleted` s hodnotou `"true"`: 

    PUT https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]
    
    {
        "name" : "my-table-datasource",
        "type" : "azuretable",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "table name", "query" : "query" },
        "dataDeletionDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy", "softDeleteColumnName" : "IsDeleted", "softDeleteMarkerValue" : "true" }
    }   


## <a name="help-us-make-azure-search-better"></a>Pomozte nám vylepšit Azure hledání

Pokud máte poslat žádost o funkci nebo nápady pro vylepšení, prosím kontaktujte nás na [webu UserVoice](https://feedback.azure.com/forums/263029-azure-search/).