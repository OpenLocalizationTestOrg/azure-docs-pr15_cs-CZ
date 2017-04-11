<properties
    pageTitle="Upgrade na 2 verzi technologie pro analýzu Text rozhraní API | Microsoft Azure"
    description="Azure počítače výukové Text analýzy - upgradovat na verze 2"
    services="cognitive-services"
    documentationCenter=""
    authors="onewth"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="onewth"/>

# <a name="upgrading-to-version-2-of-the-text-analytics-api"></a>Upgrade na 2 verzi technologie pro analýzu Text rozhraní API #

Tato příručka vás provede proces upgrade kód z [první verzi rozhraní API](../machine-learning/machine-learning-apps-text-analytics.md) pro použití na druhém verzi. 

Pokud nepoužili rozhraní API a chcete získat další informace, můžete **[Další informace o rozhraní API](//go.microsoft.com/fwlink/?LinkID=759711)** nebo kterou **[sledujete – úvodní příručka](//go.microsoft.com/fwlink/?LinkID=760860)**. Technické informace v příručce **[Rozhraní API definice](//go.microsoft.com/fwlink/?LinkID=759346)**.

### <a name="part-1-get-a-new-key"></a>Část 1. Získat nový klíč ###

Nejdřív se musíte získat nový klíč rozhraní API z **Azure portálu**:

1. Přejděte ke službě Text analýzy pomocí [Galerie Intelligence Cortana](//gallery.cortanaintelligence.com/MachineLearningAPI/Text-Analytics-2). Tady je také k dispozici odkazy na příklady si přečtěte následující dokumentaci a kód.

1. Klikněte na **přihlásit**. Tento odkaz přejdete na portál správy Azure místo, kam se můžou zaregistrovat ke službě.

1. Vyberte požadovaný plán. Můžete vybrat **bezplatné osy na 5 000 transakce/měsíc**. Je bezplatná plánu, se nezapočítávají pro použití služby. Musíte se přihlásit k předplatnému Azure. 

1. Po registraci Text analýzy, budete mít k dispozici **Rozhraní API klíče**. Zkopírujte tento klíč, musíte ho při používání rozhraní API služeb.

### <a name="part-2-update-the-headers"></a>Část 2. Aktualizovat záhlaví ###

Aktualizace odeslané záhlaví hodnoty, jak je ukázáno v následujícím příkladu. Všimněte si, že je už kódovaný klíč účtu.

**Verze 1**

    Authorization: Basic base64encode(<your Data Market account key>)
    Accept: application/json

**Verze 2**

    Content-Type: application/json
    Accept: application/json
    Ocp-Apim-Subscription-Key: <your Azure Portal account key>


### <a name="part-3-update-the-base-url"></a>Část 3. Aktualizujte základní adresu URL ###

**Verze 1**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/

**Verze 2**

    https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/

### <a name="part-4a-update-the-formats-for-sentiment-key-phrases-and-languages"></a>Část 4a. Aktualizovat formát myšlenkou, klíčových frází a jazyky ###

#### <a name="endpoints"></a>Koncové body ####

ZÍSKÁNÍ koncové body mít teď byly změněny, tak všechny vstupní by měl odeslaný jako žádost o příspěvek. Aktualizuje koncové body a ty, které vidíte na obrázku.

| |Koncový bod jedné verze 1|Koncový bod dávku verze 1|Koncový bod verze 2|
|---|---|---|---|
|Typ volání|ZÍSKÁNÍ|PŘÍSPĚVEK|PŘÍSPĚVEK|
|Myšlenkou|```GetSentiment```|```GetSentimentBatch```|```sentiment```|
|Klíčové věty|```GetKeyPhrases```|```GetKeyPhrasesBatch```|```keyPhrases```|
|Jazyky|```GetLanguage```|```GetLanguageBatch```|```languages```|

#### <a name="input-formats"></a>Vstupní formáty ####

Poznámka: Tento formát pouze příspěvek teď přijme, tak, aby měli přeformátovat jakýkoli vstup, který dříve používal koncové body jeden dokument příslušným způsobem. Vstupní data nejsou malá a velká písmena.

**Verze 1 (dávku)**

    {
      "Inputs": [
        {
          "Id": "string",
          "Text": "string"
        }
      ]
    }

**Verze 2**

    {
      "documents": [
        {
          "id": "string",
          "text": "string"
        }
      ]
    }

#### <a name="output-from-sentiment"></a>Výstup myšlenkou ####

**Verze 1**

    {
      "SentimentBatch":[{
        "Id":"string",
        "Score":"double"
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Verze 2**

    {
      "documents":[{
        "id":"string",
        "score":"double"
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }

#### <a name="output-from-key-phrases"></a>Výstup klíčových frází ####

**Verze 1**

    {
      "KeyPhrasesBatch":[{
        "Id":"string",
        "KeyPhrases":["string"]
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Verze 2**

    {
      "documents":[{
        "id":"string",
        "keyPhrases":["string"]
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }

#### <a name="output-from-languages"></a>Výstup jazyky ####


**Verze 1**

    {
      "LanguageBatch":[{
        "id":"string",
        "detectedLanguages": [{
          "Score":"double"
          "Name":"string",
          "Iso6391Name":"string"
        }]
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Verze 2**

    {
      "documents":[{
        "id":"string",
        "detectedLanguages": [{
          "score":"double"
          "name":"string",
          "iso6391Name":"string"
        }]
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }


### <a name="part-4b-update-the-formats-for-topics"></a>Část 4b. Aktualizace formáty témat ###

#### <a name="endpoints"></a>Koncové body ####

| |Koncový bod verze 1 | Koncový bod verze 2|
|---|---|---|
|Odeslání tématu zjišťování (publikovat)|```StartTopicDetection```|```topics```|
|Vzdálené použití téma výsledků (GET)|```GetTopicDetectionResult?JobId=<jobId>```|```operations/<operationId>```|

#### <a name="input-formats"></a>Vstupní formáty ####

**Verze 1**

    {
      "StopWords": [
        "string"
      ],
      "StopPhrases": [
        "string"
      ], 
      "Inputs": [
        {
          "Id": "string",
          "Text": "string"
        }
      ]
    }

**Verze 2**

    {
      "stopWords": [
        "string"
      ],
      "stopPhrases": [
        "string"
      ],
      "documents": [
        {
          "id": "string",
          "text": "string"
        }
      ]
    }

#### <a name="submission-results"></a>Odeslání výsledků ####

**Verze 1 (publikovat)**

Dříve po dokončení projektu obdržíte následující JSON výstupu, kde by jobId připojena k adresy URL vzdálené použití výstupu.

    {
        "odata.metadata":"<url>",
        "JobId":"<JobId>"
    }

**Verze 2 (publikovat)**

Odpověď na schůzku zahrne hodnotu záhlaví takto, kde `operation-location` slouží jako koncový bod dotazování výsledků:

    'operation-location': 'https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>'

#### <a name="operation-results"></a>Výsledky operace ####

**Verze 1 (GET)**

    {
      "TopicInfo" : [{
        "TopicId" : "string"
        "Score" : "double"
        "KeyPhrase" : "string"
      }],
      "TopicAssignment" : [{
        "Id" : "string",
        "TopicId" : "string",
        "Distance" : "double"
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Verze 2 (GET)**

Jako dříve bude vrácena **pravidelně hlasování výstup** (navrhované období je každou minutu) do výstupu. 

Jakmile rozhraní API témata má dokončen, čtení stav `succeeded` bude vrácena. Potom to bude obsahovat výstup ve formátu ukázáno v následujícím příkladu:

    {
        "status": "succeeded",
        "createdDateTime": "string",
        "operationType": "topics",
        "processingResult": {
            "topics" : [{
            "id" : "string"
            "score" : "double"
            "keyPhrase" : "string"
          }],
          "topicAssignments" : [{
            "topicId" : "string",
            "documentId" : "string",
            "distance" : "double"
          }],
          "errors" : [{
              "id":"string",
              "message":"string"
          }]
        }
    }

### <a name="part-5-test-it"></a>Část 5. Vyzkoušejte! ###

Teď byste měli být můžete začít! Testování kódu pomocí malého vzorového zajistit, že můžete úspěšně zpracovávat vaše data.
