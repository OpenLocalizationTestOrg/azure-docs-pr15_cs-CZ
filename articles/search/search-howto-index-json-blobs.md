<properties
pageTitle="Indexování objektů BLOB JSON s indexování objektů blob Azure hledání"
description="Indexování objektů BLOB JSON s indexování objektů blob Azure hledání"
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
ms.date="07/26/2016"
ms.author="eugenesh" />

# <a name="indexing-json-blobs-with-azure-search-blob-indexer"></a>Indexování objektů BLOB JSON s indexování objektů blob Azure hledání 

Tento článek popisuje postup konfigurace indexování objektů blob Azure hledání extrahovat strukturovaných obsah z objektů BLOB, které obsahují JSON.

## <a name="scenarios"></a>Scénáře

Ve výchozím nastavení [indexování objektů blob Azure hledání](search-howto-indexing-azure-blob-storage.md) rozdělí objektů BLOB JSON jako jeden blok textu. Často chcete zachovat strukturu JSON dokumentů. Například mít JSON dokumentu 

    { 
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13" 
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

můžete chtít analyzovat na Azure Prohledat dokument s "text", "datePublished" a "značky" polí.

Můžete taky, pokud vaše objektů BLOB obsahují **pole objektů JSON**, je vhodné jednotlivé elementy pole osvobozením od samostatné Azure Prohledat dokument. Například mít objektů blob s Tento JSON:  

    [
        { "id" : "1", "text" : "example 1" },
        { "id" : "2", "text" : "example 2" },
        { "id" : "3", "text" : "example 3" }
    ]

můžete naplnit Azure vyhledávacím odkazu s 3 samostatné dokumenty, oba objekty mají pole "id" a "text". 

> [AZURE.IMPORTANT] Tato funkce momentálně v náhledu. Je k dispozici pouze v rozhraní REST API pomocí verze **2015 02 28 náhledu**. Mějte na paměti, náhled rozhraní API jsou určené pro testování a vyhodnocení a neměla používat ve provozním prostředí. 

## <a name="setting-up-json-indexing"></a>Nastavení formátu JSON indexování

Index objektů BLOB JSON, nastavte `parsingMode` konfigurace parametr `json` (chcete indexovat každý objektů blob jako jeden dokument) nebo `jsonArray` (Pokud vaše objektů BLOB obsahují JSON matice): 

    {
      "name" : "my-json-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "json" | "jsonArray" } }
    }

V případě potřeby vyberte vlastnosti dokumentu JSON zdroj použitá k naplnění cílové vyhledávacím odkazu pomocí **mapování polí** .  Tím se píše níže. 

> [AZURE.IMPORTANT] Pokud používáte `json` nebo `jsonArray` analýze režimu, Azure hledání se předpokládá, že všechny objekty BLOB ve zdroji dat bude JSON. Pokud potřebujete podporu kombinaci JSON a jiných JSON objektů BLOB ve stejné zdroje dat, dejte nám prosím vědět na [webu UserVoice](https://feedback.azure.com/forums/263029-azure-search).

## <a name="using-field-mappings-to-build-search-documents"></a>Použití mapování polí k vytvoření Hledat dokumenty 

V současné době Azure hledání nelze indexovat libovolného JSON dokumentů přímo, protože podporuje pouze základní datové typy, řetězec matice a GeoJSON čárky. Však můžete **mapování polí** vyberte část dokumentu JSON a "zvedněte" je do nejvyšší úrovně pole Prohledat dokument. Informace o základních funkcích mapování polí najdete v tématu [mapování polí indexování hledání Azure most rozdíly mezi zdrojů dat a hledat indexy](search-indexer-field-mappings.md).

Vraceli podobu dokumentu JSON příkladu: 

    { 
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13" 
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Řekněme, že máte indexu vyhledávání pomocí následujících polí: `text` typu Edm.String, `date` typu Edm.DateTimeOffset, a `tags` typu Collection(Edm.String). Požadovaný tvar mapovat vaší JSON, použijte následující mapování polí: 

    "fieldMappings" : [ 
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
    ]

Pole názvy zdrojů v části mapování byla vyplněná soustavě [JSON ukazatel](http://tools.ietf.org/html/rfc6901) . Začít s lomítko odkázat na kořenovém dokumentu JSON a potom procházejte požadované vlastnosti (úrovni libovolného vnoření) pomocí dopředu oddělené lomítko cesty. 

Můžete taky odkazovat na jednotlivé pole prvky pomocí index od nuly. Například vyberte první prvek "značky" pole z výše uvedeném příkladu, můžete mapování polí takto:

    { "sourceFieldName" : "/article/tags/0", "targetFieldName" : "firstTag" }

> [AZURE.NOTE] Pokud pole název zdroje v parametru path mapování pole odkazuje na vlastnost, která ve formátu JSON neexistuje, mapování vynechán bez chybu. Důvodem je, aby podporujeme dokumenty s jiné schéma (což je běžné případu použití). Protože neexistuje žádná ověření, potřebujete starat Chcete-li předejít překlepy ve specifikaci mapování polí. 

Pokud vaše dokumenty JSON obsahovat pouze jednoduché nejvyšší úrovně vlastnosti, nemusí mapování polí vůbec potřebujete. Například pokud vaše JSON vypadá takto, nejvyšší úrovně vlastnosti "text", "datePublished" a "značky" přímo namapuje do příslušných polí do indexu vyhledávání: 
 
    { 
       "text" : "A hopefully useful article explaining how to parse JSON blobs",
       "datePublished" : "2016-04-13" 
       "tags" : [ "search", "storage", "howto" ]    
    }

## <a name="indexing-nested-json-arrays"></a>Indexování vnořených JSON polí

Co dělat, když chcete indexovat maticových JSON objektů, ale dané pole je vnořené někde dokumentu? Můžete vybrat vlastností, které obsahuje pole pomocí `documentRoot` konfigurace vlastností. Pokud například vaše objektů BLOB vypadat takto: 

    { 
        "level1" : {
            "level2" : [
                { "id" : "1", "text" : "Use the documentRoot property" }, 
                { "id" : "2", "text" : "to pluck the array you want to index" },
                { "id" : "3", "text" : "even if it's nested inside the document" }  
            ]
        }
    } 

Pomocí této konfigurace indexování pole obsažená ve vlastnosti "level2": 

    {
        "name" : "my-json-array-indexer",
        ... other indexer properties
        "parameters" : { "configuration" : { "parsingMode" : "jsonArray", "documentRoot" : "/level1/level2" } }
    }


## <a name="request-examples"></a>Příklady požadavek

Vložení to všechny společně příkladů dokončení datové části. 

Zdroje dat: 

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<my storage connection string>" },
        "container" : { "name" : "my-container", "query" : "optional, my-folder" }
    }   

Indexování:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "useJsonParser" : true } }, 
      "fieldMappings" : [ 
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
      ]
    }

## <a name="help-us-make-azure-search-better"></a>Pomozte nám vylepšit Azure hledání

Pokud máte poslat žádost o funkci nebo nápady pro vylepšení, prosím kontaktujte nás na [webu UserVoice](https://feedback.azure.com/forums/263029-azure-search/).