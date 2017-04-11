<properties
pageTitle="OneDrive pro firmy | Microsoft Azure"
description="Vytváření aplikací pro použití logických operátorů službou Azure aplikace. Připojení k Onedrivu pro firmy ke správě souborů. Můžete dělat různé akce například nahrát, aktualizovat, získat a odstraňovat soubory."
services="logic-apps"   
documentationCenter=".net,nodejs,java"  
authors="msftman"   
manager="erikre"    
editor=""
tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-onedrive-for-business-connector"></a>Začínáme s Onedrivem pro firmy spojnice

Připojení k Onedrivu pro firmy ke správě souborů. Můžete dělat různé akce například nahrát, aktualizovat, získat a odstraňovat soubory.

>[AZURE.NOTE] Tuto verzi článku platí pro verze schématu 2015 08 01 náhled logiky aplikace. 

Můžete začít s vytvářením logiky aplikaci najdete v tématu [Vytvoření logiky aplikace](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Aktivace a akce

OneDrive pro firmy spojnice mohou sloužit jako akce; má které aktivační. Všechny spojnice podporují dat ve formátu JSON a XML. 

 OneDrive pro firmy spojnice obsahuje následující akce nebo které aktivační k dispozici:

### <a name="onedrive-for-business-actions"></a>OneDrive pro firmy akce
Můžete provést tyto akce:

|Akce|Popis|
|--- | ---|
|[GetFileMetadata](connectors-create-api-onedriveforbusiness.md#getfilemetadata)|Načte metadat souboru na Onedrivu pro firmy pomocí id|
|[UpdateFile](connectors-create-api-onedriveforbusiness.md#updatefile)|Aktualizace souboru na Onedrivu pro firmy|
|[DeleteFile](connectors-create-api-onedriveforbusiness.md#deletefile)|Odstraní soubor z Onedrivu pro firmy|
|[GetFileMetadataByPath](connectors-create-api-onedriveforbusiness.md#getfilemetadatabypath)|Načte metadat souboru na Onedrivu pro firmy pomocí cesty|
|[GetFileContentByPath](connectors-create-api-onedriveforbusiness.md#getfilecontentbypath)|Vyhledá obsah souboru na Onedrivu pro firmy pomocí cesty|
|[GetFileContent](connectors-create-api-onedriveforbusiness.md#getfilecontent)|Vyhledá obsah souboru na Onedrivu pro firmy pomocí id|
|[CreateFile](connectors-create-api-onedriveforbusiness.md#createfile)|Odeslání souboru na OneDrive pro firmy|
|[CopyFile](connectors-create-api-onedriveforbusiness.md#copyfile)|Slouží ke kopírování souboru na OneDrive pro firmy|
|[ListFolder](connectors-create-api-onedriveforbusiness.md#listfolder)|Seznam souborů v Onedrivu pro firmy složku|
|[ListRootFolder](connectors-create-api-onedriveforbusiness.md#listrootfolder)|Seznam souborů v Onedrivu pro firmy kořenové složky|
|[ExtractFolderV2](connectors-create-api-onedriveforbusiness.md#extractfolderv2)|Extrahuje složku na OneDrive pro firmy|
### <a name="onedrive-for-business-triggers"></a>Spustí Onedrivu pro firmy
Můžete poslouchat tyto události:

|Aktivační událost | Popis|
|--- | ---|
|Vytvoření souboru|Spustí tok při vytvoření nového souboru v složky Onedrivu pro firmy|
|Úpravy souboru|Spustí tok při úpravě souboru na Onedrivu pro firmy složky|


## <a name="create-a-connection-to-onedrive-for-business"></a>Vytvoření připojení k Onedrivu pro firmy
Vytváření aplikací pro použití logických operátorů s Onedrivem pro firmy, můžete musíte nejdřív vytvořit **připojení** a pak zadejte podrobnosti pro následující vlastnosti: 

|Vlastnost| Povinné|Popis|
| ---|---|---|
|Token|Ano|Zadání Onedrivu pro firmy přihlašovací údaje|
Po vytvoření připojení můžete k provedení akce a poslouchat aktivace popisované v tomto článku. 

>[AZURE.INCLUDE [Steps to create a connection to OneDrive for Business](../../includes/connectors-create-api-onedriveforbusiness.md)]

>[AZURE.TIP] Toto připojení můžete použít v jiných aplikacích pro použití logických operátorů.

## <a name="reference-for-onedrive-for-business"></a>Odkaz pro OneDrive pro firmy
Platí pro verze: 1.0

## <a name="getfilemetadata"></a>GetFileMetadata
Získejte soubor metadat pomocí id: načte metadat souboru na Onedrivu pro firmy pomocí id 

```GET: /datasets/default/files/{id}``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|ID|řetězec|Ano|Cesta|žádná|Zadejte soubor|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


## <a name="updatefile"></a>UpdateFile
Aktualizace souboru: aktualizuje souboru na Onedrivu pro firmy 

```PUT: /datasets/default/files/{id}``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|ID|řetězec|Ano|Cesta|žádná|Zadejte soubor aktualizovat|
|textu| |Ano|textu|žádná|Obsah souboru k aktualizaci na Onedrivu pro firmy|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


## <a name="deletefile"></a>DeleteFile
Odstraňte soubor: Odstraní soubor z Onedrivu pro firmy 

```DELETE: /datasets/default/files/{id}``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|ID|řetězec|Ano|Cesta|žádná|Zadejte soubor odstranit|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


## <a name="getfilemetadatabypath"></a>GetFileMetadataByPath
Získat metadata soubor pomocí cesty: načte metadat souboru na Onedrivu pro firmy pomocí cesty 

```GET: /datasets/default/GetFileByPath``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|Cesta|řetězec|Ano|dotaz|žádná|Jedinečný cestu k souboru na Onedrivu pro firmy|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


## <a name="getfilecontentbypath"></a>GetFileContentByPath
Získání obsahu soubor pomocí cesty: načte obsah souboru na Onedrivu pro firmy pomocí cesty 

```GET: /datasets/default/GetFileContentByPath``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|Cesta|řetězec|Ano|dotaz|žádná|Jedinečný cestu k souboru na Onedrivu pro firmy|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


## <a name="getfilecontent"></a>GetFileContent
Získejte soubor obsahu pomocí id: načte obsah souboru na Onedrivu pro firmy pomocí id 

```GET: /datasets/default/files/{id}/content``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|ID|řetězec|Ano|Cesta|žádná|Zadejte soubor|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


## <a name="createfile"></a>CreateFile
Vytvoření souboru: odeslání souboru na OneDrive pro firmy 

```POST: /datasets/default/files``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|cesta_ke_složce|řetězec|Ano|dotaz|žádná|Složka pro nahrání souboru na OneDrive pro firmy|
|Jméno|řetězec|Ano|dotaz|žádná|Název souboru pro vytvoření na Onedrivu pro firmy|
|textu| |Ano|textu|žádná|Obsah souboru na OneDrive pro firmy|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


## <a name="copyfile"></a>CopyFile
Kopírování souboru: slouží ke kopírování souboru na OneDrive pro firmy 

```POST: /datasets/default/copyFile``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|zdroje|řetězec|Ano|dotaz|žádná|Adresa URL pro zdrojový soubor|
|cíl|řetězec|Ano|dotaz|žádná|Určení cesta k souboru na Onedrivu pro firmy, včetně cílový název souboru|
|Přepsat|Logická hodnota|Ne|dotaz|NEPRAVDA|Pokud přepíše cílového souboru nastavena na hodnotu "true"|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


## <a name="onnewfile"></a>OnNewFile
Vytvoření souboru: aktivuje tok při vytvoření nového souboru v složky Onedrivu pro firmy 

```GET: /datasets/default/triggers/onnewfile``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|ID složky|řetězec|Ano|dotaz|žádná|Zadejte do složky|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


## <a name="onupdatedfile"></a>OnUpdatedFile
Úpravy souboru: aktivuje tok při úpravě souboru na Onedrivu pro firmy složky 

```GET: /datasets/default/triggers/onupdatedfile``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|ID složky|řetězec|Ano|dotaz|žádná|Zadejte do složky|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


## <a name="listfolder"></a>ListFolder
Seznam souborů ve složce: seznam souborů v Onedrivu pro firmy složku 

```GET: /datasets/default/folders/{id}``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|ID|řetězec|Ano|Cesta|žádná|Složky|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


## <a name="listrootfolder"></a>ListRootFolder
Seznam kořenové složce: seznam souborů v Onedrivu pro firmy kořenové složky 

```GET: /datasets/default/folders``` 

Neexistují žádné parametry pro tento hovor
#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


## <a name="extractfolderv2"></a>ExtractFolderV2
Extrahování složky: Extrahuje složku na OneDrive pro firmy 

```POST: /datasets/default/extractFolderV2``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|zdroje|řetězec|Ano|dotaz|žádná|Cesta k souboru archivu|
|cíl|řetězec|Ano|dotaz|žádná|Cesta v Onedrivu pro firmy umožňující extrahování obsahu archivu|
|Přepsat|Logická hodnota|Ne|dotaz|NEPRAVDA|Přepíše cílové soubory, pokud nastavena na hodnotu "true"|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


## <a name="object-definitions"></a>Definice objektů 

### <a name="datasetsmetadata"></a>DataSetsMetadata


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|tabulkové|Nedefinováno|Ne |
|objektů BLOB|Nedefinováno|Ne |



### <a name="tabulardatasetsmetadata"></a>TabularDataSetsMetadata


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|zdroje|řetězec|Ne |
|displayName|řetězec|Ne |
|urlEncoding|řetězec|Ne |
|tableDisplayName|řetězec|Ne |
|tablePluralName|řetězec|Ne |



### <a name="blobdatasetsmetadata"></a>BlobDataSetsMetadata


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|zdroje|řetězec|Ne |
|displayName|řetězec|Ne |
|urlEncoding|řetězec|Ne |



### <a name="blobmetadata"></a>BlobMetadata


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|ID|řetězec|Ne |
|Jméno|řetězec|Ne |
|DisplayName|řetězec|Ne |
|Cesta|řetězec|Ne |
|Změněno|řetězec|Ne |
|Velikost|celé číslo|Ne |
|MediaType|řetězec|Ne |
|IsFolder|Logická hodnota|Ne |
|ETag|řetězec|Ne |
|FileLocator|řetězec|Ne |



### <a name="object"></a>Objekt


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|


## <a name="next-steps"></a>Další kroky
[Vytvoření aplikace logiky](../app-service-logic/app-service-logic-create-a-logic-app.md)