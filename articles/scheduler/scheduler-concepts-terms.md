<properties
 pageTitle="Plánovač koncepty, termíny a entity | Microsoft Azure"
 description="Azure koncepty Plánovač terminologie a hierarchie entit, včetně úlohy a úlohy kolekcí.  Zobrazí úplný příklad plánované práce."
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="get-started-article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="scheduler-concepts-terminology--entity-hierarchy"></a>Plánovač koncepty, terminologie + entity hierarchie

## <a name="scheduler-entity-hierarchy"></a>Plánovač entity hierarchie

Následující tabulka popisuje hlavní zdroje zveřejněným ani ho nepoužíval rozhraním API Plánovač:

|Zdroje | Popis |
|---|---|
|**Kolekce projektu**|Shromažďování úlohy obsahuje skupinu úlohy a udržuje nastavení kvót a omezením, které jsou sdílené úlohami v rámci kolekce. Kolekce úlohy se vytvořil vlastník předplatného a skupin úlohy společně podle hranice využití nebo aplikací. Je omezen na jednom regionu. Umožňuje také vynucení kvóty omezit používání všechny úlohy v této kolekci. Kvóty zahrnout MaxJobs a MaxRecurrence.|
|**Úlohy**|Úlohy definuje jeden akce opakující se jednoduchou nebo složitou strategie pro spuštění. Akce může obsahovat HTTP, fronty úložiště, bus frontě nebo žádost o službu bus tématu.|
|**Historie úlohy**|Historie úlohy představuje podrobnosti o spuštění úlohy. Obsahuje úspěšné a selhání, jakož i cokoli v podrobnostech odpověď.|

## <a name="scheduler-entity-management"></a>Správa entity Plánovač

Na vysoké úrovni Plánovač a rozhraní API pro správu služby vystavit tyto operace o zdrojích:

|Funkce|Popis a identifikátor URI adresu|
|---|---|
|**Správa úloh kolekce**|ZÍSKAT, umístění a odstraňte podpory k vytváření a upravování kolekcí úlohy a úlohy v něm obsažené. Úlohy kolekce je kontejner pro úlohy a mapy kvót a sdílené nastavení. Příklady kvóty popsáno dále, jsou maximální počet úlohy a nejmenší intervalu opakování. <p>UMÍSTĚNÍ a odstranění:`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</p><p>ZÍSKÁNÍ:`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</p>
|**Správa úloh**|ZÍSKAT, umístění, publikovat, oprava a odstraňte podpory k vytváření a upravování úlohy. Všechny úlohy musí patřit ke kolekci úlohy, které už existuje, tedy žádné implicitní vytvoření. <p>`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}`</p>|
|**Správa úloh historie**|ZÍSKÁNÍ podpory pro načítání 60 dní historie provádění úloh, například úlohy uplynulého času a výsledky spuštění úlohy. Přidá podporu parametru řetězce dotazu pro filtrování podle státu a stav. <P>`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}/history`</p>|

## <a name="job-types"></a>Typy úloh

Existuje několik typů úloh: úlohy HTTP (včetně HTTPS úloh, které podporují SSL), úlohy ve frontě úložiště, úlohy ve frontě bus služby a služby bus téma úlohy. Nastavit informace HTTP úlohy jsou ideální, pokud máte koncový bod existující pracovní zátěž nebo služby. Úlohy ve frontě úložiště můžete použít k odesílání zpráv do fronty úložiště, abyste tyto úlohy jsou ideální pro úloh, které používají fronty úložiště. Podobně služby bus úlohy jsou ideální pro úloh, které používají fronty bus služby a témata.

## <a name="the-job-entity-in-detail"></a>Entita "práce" podrobně

Základní úrovni plánované práce obsahuje několik částí:

- Akci provádět úlohy časovače se aktivuje, když  

- (Volitelné) Čas spuštění úlohy  

- (Volitelné) Kdy a jak často se má opakovat projektu  

- (Volitelné) Akci, kterou chcete Pokud primární akce se nezdaří  

Naplánované úlohy interně, obsahuje také systém-za předpokladu, že data třeba příští naplánovaný čas spuštění.

Následující kód obsahuje úplný příklad plánované práce. Informace jsou k dispozici v následujících částech.

    {
        "startTime": "2012-08-04T00:00Z",               // optional
        "action":
        {
            "type": "http",
            "retryPolicy": { "retryType":"none" },
            "request":
            {
                "uri": "http://contoso.com/foo",        // required
                "method": "PUT",                        // required
                "body": "Posting from a timer",         // optional
                "headers":                              // optional

                {
                    "Content-Type": "application/json"
                },
            },
           "errorAction":
           {
               "type": "http",
               "request":
               {
                   "uri": "http://contoso.com/notifyError",
                   "method": "POST",
               },
           },
        },
        "recurrence":                                   // optional
        {
            "frequency": "week",                        // can be "year" "month" "day" "week" "minute"
            "interval": 1,                              // optional, how often to fire (default to 1)
            "schedule":                                 // optional (advanced scheduling specifics)
            {
                "weekDays": ["monday", "wednesday", "friday"],
                "hours": [10, 22]
            },
            "count": 10,                                 // optional (default to recur infinitely)
            "endTime": "2012-11-04",                     // optional (default to recur infinitely)
        },
        "state": "disabled",                           // enabled or disabled
        "status":                                       // controlled by Scheduler service
        {
            "lastExecutionTime": "2007-03-01T13:00:00Z",
            "nextExecutionTime": "2007-03-01T14:00:00Z ",
            "executionCount": 3,
                                                "failureCount": 0,
                                                "faultedCount": 0
        },
    }

Jak je vidět v ukázkové naplánované úlohy výše, definici úlohy obsahuje několik částí:

- Spuštění ("čas spuštění")  

- Akce ("akce"), který obsahuje chyby akce ("errorAction")

- Opakování ("opakování")  

- Stav ("krajů")  

- Stav ("status")  

- Opakovat zásad ("retryPolicy")  

Podívejme se podrobněji, každý z těchto podrobně:

## <a name="starttime"></a>čas spuštění

"Čas spuštění" je čas zahájení a umožňuje volajícím určete časové pásmo posun na drátěný ve [formátu ISO 8601 formátu](http://en.wikipedia.org/wiki/ISO_8601).

## <a name="action-and-erroraction"></a>Akce a errorAction

"Akce" akci vyvolat na všechny výskyty a popisuje typu vyvolání služby. Tato akce je, co se bude provádět na zadané plán. Plánovač podporuje HTTP, fronty úložiště, téma bus služby a služby bus fronty akce.

Akce v předchozím příkladu je HTTP akce. Tady je příklad úložiště fronty akce:

    {
            "type": "storageQueue",
            "queueMessage":
            {
                "storageAccount": "myStorageAccount",  // required
                "queueName": "myqueue",                // required
                "sasToken": "TOKEN",                   // required
                "message":                             // required
                    "My message body",
            },
    }

Níže je příklad akce téma bus služby.

  "akce": {"typ": "serviceBusTopic", "serviceBusTopicMessage": {"topicPath": "t1"  
      "názvů": "mySBNamespace", "hodnotu transportType": "netMessaging" / / může být netMessaging nebo AMQP "ověřování": {"sasKeyName": "QPolicy", "typ": "sharedAccessKey"}, "zprávou": "Některé zprávou", "brokeredMessageProperties": {}, "customMessageProperties": {"název_aplikace": "FromScheduler"}},}

Tady je příklad akce fronty bus služby:


  "akce": {"serviceBusQueueMessage": {"název_fronty": "q1"  
      "názvů": "mySBNamespace", "hodnotu transportType": "netMessaging" / / může být netMessaging nebo AMQP "ověřování": {  
        "sasKeyName": "QPolicy", "typ": "sharedAccessKey"}, "zprávou": "Některé zprávy"  
      "brokeredMessageProperties": {}, "customMessageProperties": {"název_aplikace": "FromScheduler"}}, "typ": "serviceBusQueue"}

"errorAction" je rutinu chyb, akce vyvolat selhání primární akci. Použijte tuto proměnnou zavolat koncového zpracování chyb nebo odeslat oznámení uživatele. Tím se dá použít pro dosažení sekundární koncového bodu v případě, že hlavní není k dispozici (například v případě havárie na webu na koncový bod) nebo se dá použít pro upozornění zpracování chyb ve vzorcích koncového bodu. Stejně jako primární akci akce chyby může být jednoduchého nebo složeného použití logických operátorů založené na další akce. Naučte se vytvářet token přidružení zabezpečení, najdete pod odkazy [vytváření a používání podpisu sdílené aplikace Access](https://msdn.microsoft.com/library/azure/jj721951.aspx).

## <a name="recurrence"></a>opakování

Opakování obsahuje několik částí:

- Četnost: Nějaká minuty, hodiny, den, týden, měsíc, rok  

- Intervalu: Interval na dané frekvenci opakování  

- Předepsaném plánu: Zadejte minut doby, pracovních dnů, měsíců a monthdays opakování  

- Počet: Určení počtu výskytů  

- Čas ukončení: žádné úlohy provede po určený čas ukončení  

Pokud je nějaký objekt opakované podle definici JSON je zápisy z pravidelných projektu. Pokud nejsou zadány počet a čas_ukončení, který bude proveden první pravidlo dokončení dodržet.

## <a name="state"></a>Stav

Stav úlohy je jedním ze čtyř hodnot: povoleno, zakázané, Dokončeno nebo došlo k chybě. Můžete DÁT nebo oprava úlohy aktualizace stavu povolené nebo zakázané. Pokud úloha byla dokončena nebo došlo k chybě, tedy konečný stav, který nelze aktualizovat (když úlohy se dá pořád odstranit). Příklad vlastnost state vypadá takto:


        "state": "disabled", // enabled, disabled, completed, or faulted
Dokončení a chybovém úlohy odstraňují po 60 dní.

## <a name="status"></a>Stav

Po spuštění Plánovač úloh vrátí informace o aktuálním stavu úlohy. Tento objekt není nastavit uživatelem – je nastavený systém. Však je součástí objektu úlohy (spíše než samostatné propojených zdrojů) tak, aby jeden snadno získat stav úlohy.

Stav úlohy obsahuje čas předchozí spuštění (pokud existuje) a počet spuštění úlohy čas další plánované spuštění (pro probíhající práce).

## <a name="retrypolicy"></a>retryPolicy

Pokud Plánovač úloh nezdaří, je možné stanovit zásady opakování a zjistit, zda a jak je možné akce. To je určený podle objekt **retryType** – to je nastavena na **žádný** Pokud nejsou žádné zásady opakovat uvedené výše. Pokud zásadu opakovat, nastavte ji na **pevnou** .

Nastavení zásad opakovat, může být zadán dva další nastavení: interval opakování (**retryInterval**) a počet opakování (**retryCount**).

Interval opakování zadaným **retryInterval** objekt je časový interval mezi nimi. Výchozí hodnota je 30 sekund, jeho minimální hodnota, která dokáže nahradit je 15 sekund a jeho maximální hodnota je 18 měsíců. Úlohy v kolekcích bezplatné úlohy mají minimální, která dokáže nahradit hodnoty 1 hodinu.  Je definován ve formátu formátu ISO 8601. Podobně je hodnota argumentu číslo opakování zadaným **retryCount** objektu. je počet opakování pokus o opakování. Použita výchozí hodnota je 4 a jeho maximální hodnota je 20\. obě **retryInterval** a **retryCount** jsou volitelné. Jsou uvedeny výchozí hodnoty, pokud je nastaveno **retryType** **pevných** a žádné hodnoty byla vyplněná explicitně.

## <a name="see-also"></a>Viz taky

 [Co je Plánovač?](scheduler-intro.md)

 [Začínáme s používáním Plánovač na portálu Azure](scheduler-get-started-portal.md)

 [Plány a fakturace v Azure Plánovač](scheduler-plans-billing.md)

 [Jak vytvářet složité plány a Upřesnit opakování s Azure Plánovač](scheduler-advanced-complexity.md)

 [Azure odkaz Plánovač REST API](https://msdn.microsoft.com/library/mt629143)

 [Azure odkaz rutiny prostředí PowerShell Plánovač](scheduler-powershell-reference.md)

 [Azure vysoké dostupnosti Plánovač a spolehlivosti](scheduler-high-availability-reliability.md)

 [Azure Plánovač omezení, výchozí hodnoty a kódy chyb](scheduler-limits-defaults-errors.md)

 [Azure ověřování odchozích připojení Plánovač](scheduler-outbound-authentication.md)
