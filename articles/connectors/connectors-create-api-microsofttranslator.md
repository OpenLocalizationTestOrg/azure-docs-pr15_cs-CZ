<properties
    pageTitle="Přidání Microsoft Translator v aplikacích pro použití logických operátorů | Microsoft Azure"
    description="Základní informace o konektoru Microsoft Translator s parametry rozhraní REST API"
    services=""
    suite=""
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

# <a name="get-started-with-the-microsoft-translator-connector"></a>Začínáme s Microsoft Translator spojnice
Připojení k Microsoft Translator přeložení textu, rozpoznávat jazyk a další. Microsoft Translator můžete: 

- Vytvoření vaše obchodní toku podle data, která se zobrazí z Microsoft Translator. 
- Umožňuje přeložení textu, zjišťování jazykem a další akce. Tyto akce se odpověď a zpřístupněte výstup pro jiné akce. Například po vytvoření nového souboru v Dropboxu můžete přeložení textu v souboru na jiný jazyk pomocí Microsoft Translator.

Operace v aplikacích pro použití logických operátorů přidáte v tématu [Vytvoření logiky aplikace](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Aktivace a akce
Microsoft Translator zahrnuje následující akce. Existuje žádné aktivačních událostí.

Aktivace | Akce
--- | ---
Žádná | <ul><li>Rozpoznání jazyka</li><li>Převod textu na řeč</li><li>Překládání textu</li><li>Získat jazyky</li><li>Získat řeči jazyky</li></ul>

Všechny spojnice podporují dat ve formátu JSON a XML.


## <a name="create-a-connection-to-microsoft-translator"></a>Vytvoření připojení k Microsoft Translator

>[AZURE.INCLUDE [Steps to create a connection to Microsoft Translator](../../includes/connectors-create-api-microsofttranslator.md)]


## <a name="swagger-rest-api-reference"></a>Odkaz swagger rozhraní REST API
Platí pro verze: 1.0.

### <a name="detect-language"></a>Rozpoznání jazyka    
Rozpozná jazyk zdroj zadaných text.  
```GET: /Detect```

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|dotaz|řetězec|Ano|dotaz|žádná |Text se poznají jejichž jazyk|

#### <a name="response"></a>Odpověď
|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


### <a name="text-to-speech"></a>Převod textu na řeč    
Převede daného textového řeči jako zvuku toku ve formátu vlnovka.  
```GET: /Speak```

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|dotaz|řetězec|Ano|dotaz|žádná |Text převáděný|
|jazyk|řetězec|Ano|dotaz|žádná |Kód jazyka generovat řeči (Příklad: "cs-cz)|

#### <a name="response"></a>Odpověď
|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


### <a name="translate-text"></a>Překládání textu    
Převede text na určenému jazyku pomocí Microsoft Translator.  
```GET: /Translate```

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|dotaz|řetězec|Ano|dotaz|žádná |Chcete-li přeložit textu|
|languageTo|řetězec|Ano|dotaz| žádná|Kód jazyka cílové (Příklad: "fr")|
|languageFrom|řetězec|Ne|dotaz|žádná |Zdrojového jazyka; Pokud není zadán, Microsoft Translator pokusí se automatické rozpoznání. (Příklad: en)|
|kategorie|řetězec|Ne|dotaz|Obecné |Překlad kategorie (výchozí: Obecné")|

#### <a name="response"></a>Odpověď
|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


### <a name="get-languages"></a>Získat jazyky    
Vyhledá všechny jazyky, které podporuje Microsoft Translator.  
```GET: /TranslatableLanguages```

Neexistují žádné parametry pro tento hovor. 

#### <a name="response"></a>Odpověď
|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|


### <a name="get-speech-languages"></a>Získat řeči jazyky    
Načte umožňující řeči souhrnnou jazyky.  
```GET: /SpeakLanguages``` 

Neexistují žádné parametry pro tento hovor.

#### <a name="response"></a>Odpověď
|Jméno|Popis|
|---|---|
|200|Ok|
|Výchozí|Operace se nezdařila.|

## <a name="object-definitions"></a>Definice objektů

#### <a name="language-language-model-for-microsoft-translator-translatable-languages"></a>Jazyk: model jazyka Microsoft Translator nepřeložitelná jazyků se zápisem

|Název vlastnosti | Datový typ | Povinné|
|---|---|---|
|Kód|řetězec|Ne|
|Jméno|řetězec|Ne|


## <a name="next-steps"></a>Další kroky

[Vytvoření logiky aplikace](../app-service-logic/app-service-logic-create-a-logic-app.md).

Přejděte zpět do [seznamu rozhraní API](apis-list.md).


<!--References-->
[5]: https://datamarket.azure.com/developer/applications/
[6]: ./media/connectors-create-api-microsofttranslator/register-your-application.png
