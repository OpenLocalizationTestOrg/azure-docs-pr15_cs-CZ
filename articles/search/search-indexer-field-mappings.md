<properties
pageTitle="Mapování polí v indexování hledání Azure"
description="Konfigurace mapování polí indexování hledání Azure kontrolujte rozdíly ve názvy polí a formátovaná data"
services="search"
documentationCenter=""
authors="chaosrealm"
manager="pablocas"
editor="" />

<tags
ms.service="search"
ms.devlang="rest-api"
ms.workload="search" 
ms.topic="article"  
ms.tgt_pltfrm="na"
ms.date="10/17/2016"
ms.author="eugenesh" />

# <a name="field-mappings-in-azure-search-indexers"></a>Mapování polí v indexování hledání Azure

Při použití indexování hledání Azure, občas najdete sami v situacích, kde zadávání dat úplně neshoduje s schématu cílové index. V těchto případech můžete **mapování polí** transformace dat na požadovaný obrazec. 

Některé situacích, kdy mapování polí jsou užitečné:
 
- Zdroj dat obsahuje pole `_id`, ale hledání Azure neumožňuje počínaje podtržítko názvy polí. Mapování polí umožňuje "název" pole. 
- Chcete-li vyplnit více polí v indexu se stejnými daty zdroj dat, například vzhledem k tomu, které chcete použít jiný analyzátory příslušných polích. Mapování polí umožňují "rozvětvené" datové pole zdroj.
- Budete potřebovat k ve formátu Base 64 zakódovat nebo dekódovat vaše data. Mapování polí podporuje několik **funkcí mapování**, včetně funkcí ve formátu Base 64 kódování a dekódování.   


> [AZURE.IMPORTANT] Funkci mapování pole je v současné době v náhledu. Je k dispozici pouze v rozhraní REST API pomocí verze **2015 02 28 náhledu**. Mějte na paměti, náhled rozhraní API jsou určené pro testování a vyhodnocení a neměla používat ve provozním prostředí.

## <a name="setting-up-field-mappings"></a>Nastavení mapování polí

Můžete přidat mapování polí při vytváření nové indexování pomocí [Vytvoření indexování](search-api-indexers-2015-02-28-preview.md#create-indexer) rozhraní API. Mapování polí na indexování indexování pomocí [Aktualizace rejstříku](search-api-indexers-2015-02-28-preview.md#update-indexer) rozhraní API aplikace můžete spravovat. 

Mapování polí se skládá ze 3 částí: 

1. A `sourceFieldName`, která představuje pole ve zdroji dat. Tato vlastnost je povinný. 

2. Volitelný `targetFieldName`, která představuje pole ve vyhledávacím odkazu. Pokud není zadáno, se stejným názvem jako zdroje dat se používá. 

3. Volitelný `mappingFunction`, které transformace dat pomocí jedné z několika předdefinované funkce. Úplný seznam funkcí je [menší než](#mappingFunctions).

Mapování polí se přidají do `fieldMappings` pole na definici indexování. 

Tady je například jak pokryly rozdíly v názvech polí: 

```JSON

PUT https://[service name].search.windows.net/indexers/myindexer?api-version=[api-version]
Content-Type: application/json
api-key: [admin key]
{
    "dataSourceName" : "mydatasource",
    "targetIndexName" : "myindex",
    "fieldMappings" : [ { "sourceFieldName" : "_id", "targetFieldName" : "id" } ] 
} 
```

Indexování může obsahovat více mapování polí. Například, tady je jak je můžete "rozvětvené" pole:

```JSON

"fieldMappings" : [ 
    { "sourceFieldName" : "text", "targetFieldName" : "textStandardEnglishAnalyzer" },
    { "sourceFieldName" : "text", "targetFieldName" : "textSoundexAnalyzer" }, 
] 
```

> [AZURE.NOTE] Azure hledání používá porovnávání jmen pole a funkce v mapování polí. Tato možnost je užitečná (nemusíte správného všechny velikost písmen), ale znamená to, že zdroje dat nebo index nesmí obsahovat pole, která se liší pouze v případě.  

<a name="mappingFunctions"></a>
## <a name="field-mapping-functions"></a>Funkce mapování polí

Nyní jsou podporované tyto funkce: 

- [base64Encode](#base64EncodeFunction)
- [base64Decode](#base64DecodeFunction)
- [extractTokenAtPosition](#extractTokenAtPositionFunction)
- [jsonArrayToStringCollection](#jsonArrayToStringCollectionFunction)

<a name="base64EncodeFunction"></a>
### <a name="base64encode"></a>base64Encode 

Provede *XLL s podporou URL* ve formátu Base 64 kódování vstupního řetězce. Předpokládá, že zadání kódování UTF-8. 

#### <a name="sample-use-case"></a>Ukázka případu použití 

Pouze znaky XLL s podporou adresy URL mohou zobrazovat klíče dokumentu Azure hledání (protože zákazníci musíte mít adresy dokumentu, například pomocí rozhraní API vyhledávacího). Pokud data obsahují URL nebezpečnými znaky a chcete ho použít k naplnění klíčové pole ve vyhledávacím odkazu, použijte tuto funkci.   

#### <a name="example"></a>Příklad 

```JSON

"fieldMappings" : [ 
  { 
    "sourceFieldName" : "Path", 
    "targetFieldName" : "UrlSafePath",
    "mappingFunction" : { "name" : "base64Encode" } 
  }] 
```

<a name="base64DecodeFunction"></a>
### <a name="base64decode"></a>base64Decode

Provede ve formátu Base 64 dekódování vstupního řetězce. Zadání se dosadí *XLL s podporou URL* kódováním Base 64 řetězec. 

#### <a name="sample-use-case"></a>Ukázka případu použití 

Kulatý vlastní metadata hodnoty musí být kódováním ASCII. Můžete ve formátu Base 64 kódování představují libovolného řetězce Unicode v objektů blob vlastní metadata. Ale aby hledání smysl, můžete tuto funkci zase zakódovaný data "běžná" řetězce při naplňovat vyhledávacím odkazu.  

#### <a name="example"></a>Příklad 

```JSON

"fieldMappings" : [ 
  { 
    "sourceFieldName" : "Base64EncodedMetadata", 
    "targetFieldName" : "SearchableMetadata",
    "mappingFunction" : { "name" : "base64Decode" } 
  }] 
```

<a name="extractTokenAtPositionFunction"></a>
### <a name="extracttokenatposition"></a>extractTokenAtPosition

Rozdělí textové pole pomocí určeného oddělovače a vybere tokenu určené pozice výsledné rozdělení.

Například, pokud je vstupní `Jane Doe`, `delimiter` je `" "`(mezera) a `position` je 0, bude výsledek `Jane`; Pokud `position` 1, bude výsledek `Doe`. Pokud pozici odkazuje na token, který neexistuje, bude vrácena chyba.

#### <a name="sample-use-case"></a>Ukázka případu použití 

Zdroj dat obsahuje `PersonName` pole který chcete indexovat jako dva samostatné `FirstName` a `LastName` pole. Tato funkce umožňuje rozdělení vstupní pomocí mezeru jako oddělovač.

#### <a name="parameters"></a>Parametry

- `delimiter`: řetězci na znaky s provádějte rozdělení vstupní řetězce jako oddělovač.
- `position`: objekt celé číslo od nuly polohu tokenu vybrat po rozdělení vstupní řetězec.    

#### <a name="example"></a>Příklad

```JSON 

"fieldMappings" : [ 
  { 
    "sourceFieldName" : "PersonName", 
    "targetFieldName" : "FirstName",
    "mappingFunction" : { "name" : "extractTokenAtPosition", "parameters" : { "delimiter" : " ", "position" : 0 } } 
  }, 
  { 
    "sourceFieldName" : "PersonName", 
    "targetFieldName" : "LastName",
    "mappingFunction" : { "name" : "extractTokenAtPosition", "parameters" : { "delimiter" : " ", "position" : 1 } } 
  }] 
```

<a name="jsonArrayToStringCollectionFunction"></a>
### <a name="jsonarraytostringcollection"></a>jsonArrayToStringCollection

Transformace řetězec formátu JSON pole řetězců na pole, které mohou sloužit k naplnění `Collection(Edm.String)` pole v indexu. 

Řekněme, že vstupní řetězec je `["red", "white", "blue"]`, potom cílové pole typu `Collection(Edm.String)` bude naplněný tři hodnotami `red`, `white` a `blue`. Pro vstupní hodnoty, které nelze analyzovat jako JSON řetězec matice bude vrácena chyba. 

#### <a name="sample-use-case"></a>Ukázka případu použití

Databáze SQL Azure nemá předdefinované datový typ, které mají přirozený mapují na `Collection(Edm.String)` polí v Azure vyhledávání. K naplnění řetězec kolekci polí, naformátujte zdrojová data jako řetězců JSON a použití této funkce. 

#### <a name="example"></a>Příklad 

```JSON

"fieldMappings" : [ 
  { "sourceFieldName" : "tags", "mappingFunction" : { "name" : "jsonArrayToStringCollection" } }
] 
```

## <a name="help-us-make-azure-search-better"></a>Pomozte nám vylepšit Azure hledání

Pokud máte poslat žádost o funkci nebo nápady pro vylepšení, prosím kontaktujte nás na [webu UserVoice](https://feedback.azure.com/forums/263029-azure-search/).