<properties
    pageTitle="Úvodní příručka: počítače výukové Text analýzy API | Microsoft Azure"
    description="Azure počítače výukové Text analýzy – úvodní příručka"
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

# <a name="getting-started-with-the-text-analytics-apis-to-detect-sentiment-key-phrases-topics-and-language"></a>Začínáme s rozhraní API analýzy Text zjišťování myšlenkou klíčových frází, témata a jazyk

<a name="HOLTop"></a>

Tento dokument popisuje, jak na integrovaný služby nebo aplikace pro použití rozhraní [API analýzy Text](//go.microsoft.com/fwlink/?LinkID=759711).
Tato rozhraní API můžete použít ke zjištění myšlenkou klíčových frází, témata a jazyk z textu. [Kliknutím sem zobrazíte interaktivní ukázka prostředí.](//go.microsoft.com/fwlink/?LinkID=759712)

Získáte [definice rozhraní API](//go.microsoft.com/fwlink/?LinkID=759346) pro technické si přečtěte následující dokumentaci pro rozhraní API.

Tato příručka je určená pro verze 2 rozhraní API. Podrobné informace o verzi 1 rozhraní API, [podívejte se na tento dokument](../machine-learning/machine-learning-apps-text-analytics.md).

Na konci tohoto kurzu budete moct programově zjistit:

- **Myšlenkou** – je text kladnou nebo zápornou?

- **Klíč věty** – co jsou lidé projednávat ve znalostní bázi jednoho?

- **Témata** – co jsou lidé projednávat napříč celou řadu článků?

- **Jazyky** – kterých jazycích je text napsaný v?

Všimněte si, že toto rozhraní API poplatky 1 transakce dokumentem předaným. Jako příklad Pokud žádáte o myšlenkou 1000 dokumentů v hovoru jednoho 1000 transakce odečte.



<a name="Overview"></a>
## <a name="general-overview"></a>Obecné základní informace ##

Tento dokument je podrobného průvodce. Naším cílem je pro vás provede jednotlivými kroky potřebné školení modelu a tak, aby ukazovaly na zdroje, které vám umožní umístění v výroby. Tento cvičení bude trvat asi 30 minut.

K těmto úkolům budete potřebovat editor a volání RESTful koncové body v jazyce podle výběru.

Pusťme se do práce.

## <a name="task-1---signing-up-for-the-text-analytics-apis"></a>Úkol 1 - podpisový pro rozhraní API analýzy textu ####

V tomto úkolu bude registraci service analýz text.

1. Přejděte do části **Kognitivní služby** [Azure portál](//go.microsoft.com/fwlink/?LinkId=761108) a zajistěte, aby **Že text analýzy** je zúžený na typ rozhraní API.

1. Vyberte požadovaný plán. Můžete vybrat **bezplatné osy na 5 000 transakce/měsíc**. Je bezplatná plánu, se nezapočítávají pro použití služby. Musíte se přihlásit k předplatnému Azure. 

1. Provedení dalších polí a vytvořte svůj účet.

1. Po registraci analýzy Text, najděte **Rozhraní API klíče**. Zkopírujte primární klíč, budete potřebovat při používání rozhraní API služeb.


## <a name="task-2---detect-sentiment-key-phrases-and-languages"></a>Úkol 2: zjišťování myšlenkou, klíčových frází a jazyky ####

Není těžké si zjišťování myšlenkou, klíčových frází a jazyky v textu. Bude programově dostanete stejné výsledky jako [Ukázka prostředí](//go.microsoft.com/fwlink/?LinkID=759712) vrátí.

>[AZURE.TIP] Pro analýzu myšlenkou doporučujeme rozdělení textu do věty. To obecně vede k vyšší přesnosti v myšlenkou předpovědí.

Všimněte si, že jazyků podporovaných následujícím způsobem:

| Funkce | Podporované kódy |
|:-----|:----|
| Myšlenkou | `en`(V angličtině), `es` (Španělsko), `fr` (francouzština) `pt` (Brazílie) |
| Klíčové věty | `en`(V angličtině), `es` (Španělsko), `de` (němčina) `ja` (japonština) |


1. Bude třeba nastavit záhlaví následujícím způsobem. Všimněte si, že JSON aktuálně jenom přípustném vstupní formát pro rozhraní API. XML nepodporuje.

        Ocp-Apim-Subscription-Key: <your API key>
        Content-Type: application/json
        Accept: application/json

1. Pak formátujte vstupní řádky ve formátu JSON. Myšlenkou klíčových frází a jazyk formát je stejný. Poznámka: každého ID musí být v jedinečný Identifikátor vrácené funkcí systému. Maximální velikosti doručovaných jeden dokument, který lze odeslat je 10KB a celkový maximální velikosti doručovaných odeslané vstupní je 1MB. Víc než 1 000 dokumenty podat jednu hovor. Omezení rychlosti existuje sazbou 100 hovory za minutu - proto doporučujeme odeslat velké množství dokumentů v jediné volání. Jazyk je volitelný parametr, který by měl být zadán Pokud analýza neanglickém text. Příklad vstupní je uveden níže, kde volitelný parametr `language` myšlenkou klávesy nebo analysis slovní spojení extrahování je součástí:

        {
            "documents": [
                {
                    "language": "en",
                    "id": "1",
                    "text": "First document"
                },
                ...
                {
                    "language": "en",
                    "id": "100",
                    "text": "Final document"
                }
            ]
        }

1. Zavolání **příspěvek** v systému s vstupní myšlenkou, klíčových frází a jazyk. Adresy URL bude vypadat takto:

        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/sentiment
        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases
        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/languages

1. Volání vrátí JSON naformátovaná odpovědí s ID a ke zjištění vlastnosti. Příklad výstup myšlenkou je uveden níže (s podrobnosti o chybě vyloučené). V případě myšlenkou bude vrácena hodnota mezi 0 a 1 pro každý dokument:

        // Sentiment response
        {
            "documents": [
                {
                    "id": "1",
                    "score": "0.934"
                },
                ...
                {
                    "id": "100",
                    "score": "0.002"
                },
            ]
        }

        // Key phrases response
        {
            "documents": [
                {
                    "id": "1",
                    "keyPhrases": ["key phrase 1", ..., "key phrase n"]
                },
                ...
                {
                    "id": "100",
                    "keyPhrases": ["key phrase 1", ..., "key phrase n"]
                },
            ]
        }

        // Languages response
        {
            "documents": [
                {
                    "id": "1",
                    "detectedLanguages": [
                        {
                            "name": "English",
                            "iso6391Name": "en",
                            "score": "1"
                        }
                    ]
                },
                ...
                {
                    "id": "100",
                    "detectedLanguages": [
                        {
                            "name": "French",
                            "iso6391Name": "fr",
                            "score": "0.985"
                        }
                    ]
                }
            ]
        }


## <a name="task-3---detect-topics-in-a-corpus-of-text"></a>Úkol 3 – zjišťování témata v souhrnu textu ####

To je nově vydané rozhraní API, které vrátí horní zjištěnou témata seznam odeslaný text záznamů. Téma přiřazen klíčové slovní spojení, které může být jeden nebo více související slova. Rozhraní API je navržený pro práci i pro krátké, lidské napsaný text, například recenze a názory uživatelů.

Toto rozhraní API vyžaduje **aspoň 100 text záznamů** odeslání, ale slouží ke zjištění témata přes stovky k tisícům záznamů. Všechny záznamy neanglickém nebo záznamy s nejméně 3 slova budou ztraceny a tedy nesmí být přiděleno témata. Vyhledání téma maximální velikosti doručovaných jeden dokument, který lze odeslat je 30KB a celkový maximální velikosti doručovaných odeslané vstupní je 30MB. Téma rozpoznávání je sazba omezený na 5 odeslání každých 5 minut.

Existují dva další **volitelná** vstupních parametrů, které pomáhají zlepšit kvalitu výsledky:

- **Vypnout slova.**  Tato slova a jejich zavřít formuláře (například množné číslo) bude vyloučit z celého tématu zjišťování kanálu. Používejte pro běžné slova (pro například "problém", "Chyba" a "uživatel" může být vhodné možnosti pro zákazníka stížností software). Každý řetězec by měl být jedno slovo.
- **Ukončení věty** – těchto frází bude vyloučené ze seznamu vrácené témat. Slouží k vyloučit obecná témata, které se nemají zobrazit v seznamu výsledků. Například "Microsoft" a "Azure" bude vhodné možnosti témat chcete vyloučit. Řetězce může obsahovat více slov.

Tímto postupem zjistit témata v textu.

1. Formátujte vstupní ve formátu JSON. Tentokrát můžete definovat zastavit slova a zastavit fráze.

        {
            "documents": [
                {
                    "id": "1",
                    "text": "First document"
                },
                ...
                {
                    "id": "100",
                    "text": "Final document"
                }
            ],
            "stopWords": [
                "issue", "error", "user"
            ],
            "stopPhrases": [
                "Microsoft", "Azure"
            ]
        }

1. Použití stejné záhlaví podle úkolu 2, zkontrolujte **příspěvek** volání koncový bod témata:

        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/topics

1. Tento postup vrátí `operation-location` jako záhlaví v odpovědi, jejichž hodnota je adresa URL dotazu výsledné témat:

        'operation-location': 'https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>'

1. Dotaz vráceného `operation-location` pravidelně s žádostí o **získat** . Jednou za minutu se doporučuje.

        GET https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>

1. Koncový bod vrátí odpověď včetně `{"status": "notstarted"}` před zpracování `{"status": "running"}` při zpracování a `{"status": "succeeded"}` s výkonem po dokončení. Pak můžete využívat výstupu, která bude v tomto formátu (Poznámka: podrobnosti jako chyba formát a kalendářních dat byly vyloučeny v tomto příkladu):

        {
            "status": "succeeded",
            "operationProcessingResult": {
                "topics": [
                    {
                        "id": "8b89dd7e-de2b-4a48-94c0-8e7844265196"
                        "score": "5"
                        "keyPhrase": "first topic name"
                    },
                    ...
                    {
                        "id": "359ed9cb-f793-4168-9cde-cd63d24e0d6d"
                        "score": "3"
                        "keyPhrase": "final topic name"
                    }
                ],
                "topicAssignments": [
                    {
                        "topicId": "8b89dd7e-de2b-4a48-94c0-8e7844265196",
                        "documentId": "1",
                        "distance": "0.354"
                    },
                    ...
                    {
                        "topicId": "359ed9cb-f793-4168-9cde-cd63d24e0d6d",
                        "documentId": "55",
                        "distance": "0.758"
                    },            
                ]
            }
        }

Všimněte si, že odpověď na úspěšné témat z `operations` koncový bod bude mít schématu následující:

    {
            "topics" : [{
                "id" : "string",
                "score" : "number",
                "keyPhrase" : "string"
            }],
            "topicAssignments" : [{
                "documentId" : "string",
                "topicId" : "string",
                "distance" : "number"
            }],
            "errors" : [{
                "id" : "string",
                "message" : "string"
            }]
        }

Vysvětlení pro každou část tato odpověď se následujícím způsobem:

**témata nápovědy**

| Klíč | Popis |
|:-----|:----|
| ID | Jedinečný identifikátor pro každé téma. |
| skóre | Počet dokumentů přiřazené k tématu. |
| keyPhrase | Summarizing slovo nebo slovní spojení v tématu. |

**topicAssignments**

| Klíč | Popis |
|:-----|:----|
| documentId | Identifikátor pro dokument. Rovná ID zahrnuty ve vstupním. |
| topicId | ID téma, které přiřadila dokumentu. |
| distance (vzdálenost) | Členství v dokumentu téma skóre mezi 0 a 1. Dole v vzdálenost skóre, tím silnější je členství v tématu. |

**chyby**

| Klíč | Popis |
|:-----|:----|
| ID | Jedinečný identifikátor vstupní dokumentu chybu odkazuje. |
| zpráva | Chybová zpráva. |

## <a name="next-steps"></a>Další kroky ##

Blahopřejeme! Teď jste dokončili pomocí analýzy text s daty. Teď můžete chtít podívat pomocí nástroje, například [Power BI](//powerbi.microsoft.com) pro vizualizaci dat, stejně jako automatizace přehledy vám v reálném čase zobrazení textová data.

Použití funkce Text analýzy, například myšlenkou, jako součást moduly bot najdete [Duševní Bot](http://docs.botframework.com/en-us/bot-intelligence/language/#example-emotional-bot) příklad portálu Bot Framework.
