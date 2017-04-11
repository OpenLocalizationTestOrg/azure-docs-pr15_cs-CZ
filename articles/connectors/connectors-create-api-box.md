<properties
    pageTitle="Přidání spojnice pole ke svým aplikacím použití logických operátorů | Microsoft Azure"
    description="Základní informace o konektoru pole s parametry rozhraní REST API"
    services=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="multiple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="08/18/2016"
   ms.author="mandia"/>

# <a name="get-started-with-the-box-connector"></a>Začínáme s konektoru pole
Připojení k pole a vytvořte souborů odstranit a další. 

>[AZURE.NOTE] Tuto verzi článku platí pro verze schématu 2015 08 01 náhled logiky aplikace.

S polem máte tyto možnosti:

- Vytvoření vaše obchodní toku podle data, která se zobrazí v rozevíracím seznamu. 
- Pomocí aktivačních událostí vytvořila nebo aktualizovala souboru.
- Pomocí akce, které zkopírujte soubor, odstraňte soubor a další. Tyto akce se odpověď a zpřístupněte výstup pro jiné akce. Třeba při změně souboru na pole, můžete tento soubor a e-mailem s použitím Office 365.

Operace v aplikacích logiku přidáte v tématu [Vytvoření logiku aplikace](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Aktivace a akce
Pole obsahuje následující aktivační události a akce.

| Aktivace | Akce|
| --- | --- |
|<ul><li>Vytvoření souboru</li><li>Úpravy souboru</li></ul> | <ul><li>Vytvoření souboru</li><li>Vytvoření souboru</li><li>Zkopíroval soubor</li><li>Odstranění souboru</li><li>Extrahování archiv složky</li><li>Získejte soubor obsahu pomocí id</li><li>Získání obsahu soubor pomocí cesty</li><li>Získejte soubor metadat pomocí id</li><li>Získat metadata soubor pomocí cesty</li><li>Aktualizace souboru</li><li>Úpravy souboru</li></ul>

Všechny spojnice podporují dat ve formátu JSON a XML.

## <a name="create-a-connection-to-box"></a>Vytvoření připojení k pole
Tato spojnice přidáte ke svým aplikacím použití logických operátorů, musí povolit aplikace logiky pro připojení k svůj seznam.

>[AZURE.INCLUDE [Steps to create a connection to box](../../includes/connectors-create-api-box.md)]

Po vytvoření připojení zadejte vlastností pole. Tyto vlastnosti popisuje **rozhraní REST API odkazy** v tomto tématu.

>[AZURE.TIP] Stejné pole připojení můžete použít v jiných aplikacích pro použití logických operátorů.

## <a name="swagger-rest-api-reference"></a>Odkaz swagger rozhraní REST API
Platí pro verze: 1.0.

### <a name="create-file"></a>Vytvoření souboru
Odešle soubor do pole.  
```POST: /datasets/default/files```

| Jméno|Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|cesta_ke_složce|řetězec|Ano|dotaz|Žádná |Nahrání souboru do pole složka|
|Jméno|řetězec|Ano|dotaz|Žádná |Název souboru pro vytvoření v poli|
|textu|String(Binary) |Ano|textu|Žádná |Obsah souboru na pole|

#### <a name="response"></a>Odpověď
|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


### <a name="when-a-file-is-created"></a>Vytvoření souboru
Tok spustí, když se vytvoří nový soubor ve složce pole.  
```GET: /datasets/default/triggers/onnewfile```

| Jméno|Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|ID složky|řetězec|Ano|dotaz|Žádná |Jedinečný identifikátor složky do pole|

#### <a name="response"></a>Odpověď
|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


### <a name="copy-file"></a>Zkopíroval soubor
Slouží ke kopírování souboru do pole.  
```POST: /datasets/default/copyFile```

| Jméno|Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|zdroje|řetězec|Ano|dotaz|Žádná |Adresa URL pro zdrojový soubor|
|cíl|řetězec|Ano|dotaz| Žádná|Určení cesta k souboru v poli, včetně cílový název souboru|
|Přepsat|Logická hodnota|Ne|dotaz| Žádná|Pokud přepíše cílového souboru nastavena na hodnotu "true"|

#### <a name="response"></a>Odpověď
|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


### <a name="delete-file"></a>Odstranění souboru
Odstranění souboru v rozevíracím seznamu.  
```DELETE: /datasets/default/files/{id}```


| Jméno|Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|ID|řetězec|Ano|Cesta|Žádná |Jedinečný identifikátor odstranit v rozevíracím seznamu souborů|

#### <a name="response"></a>Odpověď
|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


### <a name="extract-archive-to-folder"></a>Extrahování archiv složky
Extrahuje souboru archivu do jiné složky v poli (Příklad: .zip).  
```POST: /datasets/default/extractFolderV2```

| Jméno|Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|zdroje|řetězec|Ano|dotaz| |Cesta k souboru archivu|
|cíl|řetězec|Ano|dotaz| |Cesta v poli umožňující extrahování obsahu archivu|
|Přepsat|Logická hodnota|Ne|dotaz| |Přepíše cílové soubory, pokud nastavena na hodnotu "true"|

#### <a name="response"></a>Odpověď
|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


### <a name="get-file-content-using-id"></a>Získejte soubor obsahu pomocí id
Vyhledá obsah souboru v poli pomocí id.  
```GET: /datasets/default/files/{id}/content```

| Jméno|Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|ID|řetězec|Ano|Cesta|Žádná |Jedinečný identifikátor souboru v poli|

#### <a name="response"></a>Odpověď
|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


### <a name="get-file-content-using-path"></a>Získání obsahu soubor pomocí cesty
Vyhledá obsah souboru v rozevíracím seznamu pomocí cesty.  
```GET: /datasets/default/GetFileContentByPath```

| Jméno|Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|Cesta|řetězec|Ano|dotaz|Žádná |Jedinečný cestu k souboru v poli|

#### <a name="response"></a>Odpověď
|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


### <a name="get-file-metadata-using-id"></a>Získejte soubor metadat pomocí id
Načte soubor metadata v rozevíracím seznamu pomocí id souboru.  
```GET: /datasets/default/files/{id}```

| Jméno|Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|ID|řetězec|Ano|Cesta| Žádná|Jedinečný identifikátor souboru v poli|

#### <a name="response"></a>Odpověď
|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


### <a name="get-file-metadata-using-path"></a>Získat metadata soubor pomocí cesty
Načte soubor metadata v rozevíracím seznamu pomocí cesty.  
```GET: /datasets/default/GetFileByPath```

| Jméno|Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|Cesta|řetězec|Ano|dotaz|Žádná |Jedinečný cestu k souboru v poli|

#### <a name="response"></a>Odpověď
|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


### <a name="update-file"></a>Aktualizace souboru
Aktualizace souboru do pole.  
```PUT: /datasets/default/files/{id}```

| Jméno|Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|ID|řetězec|Ano|Cesta| Žádná|Jedinečný identifikátor souboru aktualizovat pole|
|textu|String(Binary) |Ano|textu|Žádná |Obsah souboru na Aktualizovat pole|

#### <a name="response"></a>Odpověď
|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


### <a name="when-a-file-is-modified"></a>Úpravy souboru
Tok spustí, když je změněn soubor ve složce pole.  
```GET: /datasets/default/triggers/onupdatedfile```

| Jméno|Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|ID složky|řetězec|Ano|dotaz|Žádná |Jedinečný identifikátor složky do pole|

#### <a name="response"></a>Odpověď
|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


## <a name="object-definitions"></a>Definice objektů

#### <a name="datasetsmetadata"></a>DataSetsMetadata

|Název vlastnosti | Datový typ | Povinné|
|---|---|---|
|tabulkové|Nedefinováno|Ne|
|objektů BLOB|Nedefinováno|Ne|

#### <a name="tabulardatasetsmetadata"></a>TabularDataSetsMetadata

|Název vlastnosti | Datový typ |Povinné|
|---|---|---|
|zdroje|řetězec|Ne|
|displayName|řetězec|Ne|
|urlEncoding|řetězec|Ne|
|tableDisplayName|řetězec|Ne|
|tablePluralName|řetězec|Ne|

#### <a name="blobdatasetsmetadata"></a>BlobDataSetsMetadata

|Název vlastnosti | Datový typ |Povinné|
|---|---|---|
|zdroje|řetězec|Ne|
|displayName|řetězec|Ne|
|urlEncoding|řetězec|Ne|

#### <a name="blobmetadata"></a>BlobMetadata

|Název vlastnosti | Datový typ |Povinné|
|---|---|---|
|ID|řetězec|Ne|
|Jméno|řetězec|Ne|
|DisplayName|řetězec|Ne|
|Cesta|řetězec|Ne|
|Změněno|řetězec|Ne|
|Velikost|celé číslo|Ne|
|MediaType|řetězec|Ne|
|IsFolder|Logická hodnota|Ne|
|ETag|řetězec|Ne|
|FileLocator|řetězec|Ne|

## <a name="next-steps"></a>Další kroky

[Vytvoření logiky aplikace](../app-service-logic/app-service-logic-create-a-logic-app.md).
