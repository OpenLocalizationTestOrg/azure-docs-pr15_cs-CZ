<properties
    pageTitle="Rozhraní API výukové stroje: Text analýzy | Microsoft Azure"
    description="Rozhraní API společnosti Microsoft počítače výukové Text analýzy můžete použít k analýze nestrukturovaná textu pro myšlenkou analýzy, extrahování klíčové frázi, rozpoznávání jazyka a tématu zjišťování."
    services="machine-learning"
    documentationCenter=""
    authors="onewth"
    manager="jhubbard"
    editor="cgronlun"/> 

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="onewth"/>


# <a name="machine-learning-apis-text-analytics-for-sentiment-key-phrase-extraction-language-detection-and-topic-detection"></a>Rozhraní API výukové počítače: Extrahování frázi, rozpoznávání jazyka a tématu zjišťování klíčových Text analýzy myšlenkou,

>[AZURE.NOTE] Tato příručka je určená pro verze 1 rozhraní API. Verze 2, [**podívejte se na tento dokument**](../cognitive-services/cognitive-services-text-analytics-quick-start.md). Verze 2 je teď upřednostňované verzi tohoto rozhraní API.

## <a name="overview"></a>Základní informace

Rozhraní API analýzy Text je sada analytics pro zpracování textu [webové služby](https://datamarket.azure.com/dataset/amla/text-analytics) vytvořené pomocí výukové počítače Azure. Rozhraní API mohou sloužit k analýze nestrukturovaná text úloh, jako je myšlenkou analýzy, extrahování klíčové frázi, rozpoznávání jazyka a tématu zjišťování. Žádná data školení je potřeba k použití tohoto rozhraní API: jenom přenést textová data. Toto rozhraní API používá Upřesnit přirozeného jazyka zpracování postupy při nejlepší v předmětu předpovědí.

Zobrazí se text analýzy v akci naše [ukázku webu](https://text-analytics-demo.azurewebsites.net/), kde je také k dispozici [vzorky](https://text-analytics-demo.azurewebsites.net/Home/SampleCode) o tom, jak implementovat analýzy text v jazyce C# a Python.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)] 

---

## <a name="sentiment-analysis"></a>Myšlenkou analýzy

Rozhraní API vrátí číselného skóre mezi 0 a 1. Skóre zavřít 1 označuje kladné myšlenkou, zatímco skóre zavřít 0 označuje záporné myšlenkou. Myšlenkou skóre generováno technik klasifikace. Zadávání funkcí, které třídění zahrnout n g, funkce generované část řeči značky a vložené části aplikace word. V současné době angličtina je jediný podporovaný jazyk.
 
## <a name="key-phrase-extraction"></a>Extrahování klíčové frázi

Rozhraní API vrátí seznam řetězců označující klíčové body projevu zadávání textu. Použijeme technik ze sady nástrojů pro sofistikované zpracování přirozeného jazyka Microsoft Office. V současné době angličtina je jediný podporovaný jazyk.

## <a name="language-detection"></a>Rozpoznání jazyka

Rozhraní API vrátí zjištěné jazyka a číselného skóre mezi 0 a 1. Skóre zavřít 1 označuje 100 % jistoty platí identifikované jazyk. Celkem 120 jazyky podporují.

## <a name="topic-detection"></a>Téma zjišťování

To je nově vydané rozhraní API, které vrátí horní zjištěnou témata seznam odeslaný text záznamů. Téma přiřazen klíčové slovní spojení, které může být jeden nebo více související slova. Toto rozhraní API vyžaduje aspoň 100 text záznamy, které chcete odeslat, ale slouží ke zjištění témata přes stovky k tisícům záznamů. Všimněte si, že toto rozhraní API poplatky 1 transakce za text záznam odeslaný. Rozhraní API je navržený pro práci i pro krátké, lidské napsaný text, například recenze a názory uživatelů.

---

## <a name="api-definition"></a>Rozhraní API definice

### <a name="headers"></a>Záhlaví

Ujistěte se, zahrnout správné záhlaví v žádosti o, která by měla být následujícím způsobem:

    Authorization: Basic <creds>
    Accept: application/json
               
    Where <creds> = ConvertToBase64(“AccountKey:” + yourActualAccountKey);  

Svůj klíč účtu ze svého účtu můžete najít v [Azure Data Market](https://datamarket.azure.com/account/keys). Všimněte si, že právě pouze JSON přijetím odvolat vstupní a výstupní formátů. XML nepodporuje.

---

## <a name="single-response-apis"></a>Rozhraní API jednoho odpověď

### <a name="getsentiment"></a>GetSentiment

**ADRESA URL** 

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment

**Příklad požadavek**

Během hovoru pod jsme který požaduje myšlenkou analysis slovní spojení "Ahoj světe":

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment?Text=hello+world

To vrátí odpověď následujícím způsobem:

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
        "Score":1.0
    }

---

### <a name="getkeyphrases"></a>GetKeyPhrases

**ADRESA URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases

**Příklad požadavek**

V rámci hovoru jsme požadavek na že klíčových frází součástí text "Byl nádhernou hotelu dodržovat předpisy, s vnitřní jedinečný a popisný zaměstnance":

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases?
    Text=It+was+a+wonderful+hotel+to+stay+at,+with+unique+decor+and+friendly+staff

To vrátí odpověď následujícím způsobem:

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
      "KeyPhrases":[
        "wonderful hotel",
        "unique decor",
        "friendly staff"
      ]
    }
 
---

### <a name="getlanguage"></a>GetLanguage

**ADRESA URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguage

**Příklad požadavek**

Dole hovor GET jsme jsou požadující myšlenkou klíčové vět text *Vítáme*

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguages?
    Text=Hello+World

To vrátí odpověď následujícím způsobem:

    {
      "UnknownLanguage": false,
      "DetectedLanguages": [{
        "Name": "English",
        "Iso6391Name": "en",
        "Score": 1.0
      }]
    }

**Volitelné parametry**

`NumberOfLanguagesToDetect`parametr je volitelný. Výchozí hodnota je 1.

---

## <a name="batch-apis"></a>Rozhraní API dávku

Služba Text Analytics umožňuje provádět myšlenkou a extrakci klíč frázi se použije v režimu dávku. Všimněte si, že každý záznam skóre počty jedné transakce. Jako příklad Pokud žádáte o myšlenkou 1000 záznamy v jedné volání 1000 transakce odečte.

Všimněte si, že ID zadané do systému ID vrácené systému. Webová služba není zkontrolujte, že tyto ID jedinečný. Odpovídá volajícího ověřit jedinečnost. 


### <a name="getsentimentbatch"></a>GetSentimentBatch

**ADRESA URL** 

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch

**Příklad požadavek**

Během hovoru příspěvek pod jsme jsou požadující chráněny věty "Ahoj světe", "Foo Vítáme" a "Moje Vítáme" v textu žádosti o:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch 

Žádost o textu:

    {"Inputs":
    [
        {"Id":"1","Text":"hello world"},
        {"Id":"2","Text":"hello foo world"},
        {"Id":"3","Text":"hello my world"},
    ]}

V níže odpovědi se zobrazí seznam skóre související s textem ID:

    {
      "odata.metadata":"<url>", 
      "SentimentBatch":
      [
        {"Score":0.9549767,"Id":"1"},
        {"Score":0.7767222,"Id":"2"},
        {"Score":0.8988889,"Id":"3"}
      ],  
      "Errors":[]
    }


---

### <a name="getkeyphrasesbatch"></a>GetKeyPhrasesBatch

**ADRESA URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

**Příklad požadavek**

V tomto příkladu jsme vyžaduje pro seznam chráněny klíčové vět v těchto údajů: 

* "Byl nádhernou hotelu dodržovat předpisy, s vnitřní jedinečný a popisný zaměstnance"
* "Byl skvělých konference sestavení s velmi zajímavé rozhovory"
* "Přenos z trapných let, vynaloženy tři hodiny přejdete na letišti"

Tohoto požadavku jako příspěvku volání koncového bodu:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

Žádost o textu:

    {"Inputs":
    [
        {"Id":"1","Text":"It was a wonderful hotel to stay at, with unique decor and friendly staff"},
        {"Id":"2","Text":"It was an amazing build conference, with very interesting talks"},
        {"Id":"3","Text":"The traffic was terrible, I spent three hours going to the airport"}
    ]}

V níže odpovědi se zobrazí seznam klíčových frází související s textem ID:

    { "odata.metadata":"<url>",
        "KeyPhrasesBatch":
        [
           {"KeyPhrases":["unique decor","friendly staff","wonderful hotel"],"Id":"1"},
           {"KeyPhrases":["amazing build conference","interesting talks"],"Id":"2"},
           {"KeyPhrases":["hours","traffic","airport"],"Id":"3" }
        ],
        "Errors":[]
    }

---

### GetLanguageBatch

In the POST call below, we are requesting language detection for two text inputs:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguageBatch

Request body:

    {
      "Inputs": [
        {"Text": "hello world", "Id": "1"},
        {"Text": "c'est la vie", "Id": "2"}
      ]
    }

This returns the following response, where English is detected in the first input and French in the second input:

    {
       "LanguageBatch": [{
         "Id": "1",
         "DetectedLanguages": [{
            "Name": "Angličtina",
            "Iso6391Name": "en",
            "Score": 1.0
         }],
         "UnknownLanguage": false
       },
       {
         "Id": "2",
         "DetectedLanguages": [{
            "Name": "Francouzština",
            "Iso6391Name": "fr",
            "Score": 1.0
         }],
         "UnknownLanguage": false
       }],
       "Errors": []
    }

---

## <a name="topic-detection-apis"></a>Rozhraní API tématu zjišťování

To je nově vydané rozhraní API, které vrátí horní zjištěnou témata seznam odeslaný text záznamů. Téma přiřazen klíčové slovní spojení, které může být jeden nebo více související slova. Všimněte si, že toto rozhraní API poplatky 1 transakce za text záznam odeslaný.

Toto rozhraní API vyžaduje aspoň 100 text záznamy, které chcete odeslat, ale slouží ke zjištění témata přes stovky k tisícům záznamů.


### <a name="topics--submit-job"></a>Témata – odeslání projektu

**ADRESA URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection

**Příklad požadavek**


Dole hovor příspěvek jsme jsou žádosti o tématech sady 100 články, kde se zobrazí křestní jméno a příjmení vstupní články a dvě StopPhrases jsou však započítávány.

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection HTTP/1.1

Žádost o textu:

    {"Inputs":[
        {"Id":"1","Text":"I loved the food at this restaurant"},
        ...,
        {"Id":"100","Text":"I hated the decor"}
    ],
    "StopPhrases":[
        "restaurant", “visitor"
    ]}

V následujících odpovědi se zobrazí JobId odeslané úlohy:

    {
        "odata.metadata":"<url>",
        "JobId":"<JobId>"
    }

Seznam jedno slovo nebo více frází aplikace word, která by neměly být vrácena jako témata. Lze použít k filtrování velmi obecná témata. Například v datové sadě o hotelu recenze "hotelu" a "hostel" může být smysluplné zastavit fráze.  

### <a name="topics--poll-for-job-results"></a>Výsledky hlasování úlohy témata:

**ADRESA URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult

**Příklad požadavek**

Předáte JobId vrácených z kroku odeslat úlohy načítání výsledků. Doporučujeme volání tento koncový bod každou minutu do stavu = "Hotovo" v odpovědi. Kolem 10 minut projektu bude trvat dokončení či delší u projektů s mnoha tisíce záznamů.

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult?JobId=<JobId>


Během zpracování, odpověď bude mít takto:

    {
        "odata.metadata":"<url>",
        "Status":"Running",
        "TopicInfo":[],
        "TopicAssignment":[],
        "Errors":[]
    }


Rozhraní API vrátí výstup ve formátu JSON ve formátu následující:

    {
        "odata.metadata":"<url>",
        "Status":"Finished",
        "TopicInfo":[
        {
            "TopicId":"ed00480e-f0a0-41b3-8fe4-07c1593f4afd",
            "Score":8.0,
            "KeyPhrase":"food"
        },
        ...
        {
            "TopicId":"a5ca3f1a-fdb1-4f02-8f1b-89f2f626d692",
            "Score":6.0,
            "KeyPhrase":"decor"
            }
        ],
        "TopicAssignment":[
        {
            "Id":"1",
            "TopicId":"ed00480e-f0a0-41b3-8fe4-07c1593f4afd",
            "Distance":0.7809
        },
        ...
        {
            "Id":"100",
            "TopicId":"a5ca3f1a-fdb1-4f02-8f1b-89f2f626d692",
            "Distance":0.8034
        }
        ],
        "Errors":[]


Vlastnosti pro každou část odpověď jsou následujícím způsobem:

**Vlastnosti TopicInfo**

| Klíč | Popis |
|:-----|:----|
| TopicId | Jedinečný identifikátor pro každé téma. |
| Skóre | Počet záznamů přiřazené k tématu. |
| KeyPhrase | Summarizing slovo nebo slovní spojení v tématu. Může být 1 nebo víc slov. |

**Vlastnosti TopicAssignment**

| Klíč | Popis |
|:-----|:----|
| ID | Identifikátor záznamu. Rovná ID zahrnuty ve vstupním. |
| TopicId | ID téma, které přiřadila záznam. |
| Distance (vzdálenost) | CONFIDENCE že záznam patří do tématu. Vzdálenost blíže k nule označuje vyšší spolehlivosti. |
