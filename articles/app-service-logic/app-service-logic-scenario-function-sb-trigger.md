<properties
   pageTitle="Scénář aplikace použití logických operátorů: Vytvoření aktivační události pro Bus služby Azure funkce | Microsoft Azure"
   description="Funkce Azure slouží k vytváření služby Bus aktivační události pro aplikaci použití logických operátorů"
   services="logic-apps,functions"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/23/2016"
   ms.author="jehollan"/>

# <a name="logic-app-scenario-create-an-azure-service-bus-trigger-by-using-azure-functions"></a>Scénář aplikace použití logických operátorů: Vytvoření aktivační události pro Bus služby Azure pomocí funkcí Azure

Funkce Azure slouží k vytvoření Spouštěč aplikací použití logických operátorů, když potřebujete nasazení dlouho probíhajících posluchače nebo úkolů. Můžete například vytvořit funkci, která bude sledovat fronty a okamžitě spustit aplikaci logiky jako aktivační událost nabízené.

## <a name="build-the-logic-app"></a>Vytvářet aplikace logiku

V tomto příkladu máte funkci spuštěna aplikace každý použití logických operátorů, kterou je potřeba aktivován. Nejprve vytvořte v aplikaci použití logických operátorů, který obsahuje aktivační události pro HTTP žádost o. Funkce hovorů tohoto koncového bodu pokaždé, když přijetí frontě zprávy.  

1. Vytvoření nové aplikace logiky; Vyberte **Ruční – Pokud HTTP požádat o přijetí** aktivační událost.  
   Pokud chcete můžete určit JSON schématu pomocí zprávou fronty pomocí nástroje, například [jsonschema.net](http://jsonschema.net). Vložte schématu aktivační událost. Díky návrháře pochopit obrazce data a další snadno pokračujících vlastnosti prostřednictvím pracovního postupu.
1. Přidání dalších kroků, které chcete provést po přijetí frontě zprávy. Například odešlete e-mailu přes Office 365.  
1. Uložení aplikace logiky pro generování adresy URL zpětné aktivační události pro tuto aplikaci logiky. Adresa URL se zobrazí na kartě aktivační událost.

![Zpětné adresa URL se zobrazí na kartě aktivační událost][1]

## <a name="build-the-function"></a>Vytvoření funkci

Dál je potřeba vytvořit funkci, která bude sloužit jako aktivační událost a poslouchat fronty.

1. Na [portálu Azure funkce](https://functions.azure.com/signin)vyberte **Nové funkce**a vyberte šablonu **ServiceBusQueueTrigger - C#** .

    ![Azure funkce portálu][2]

2. Konfigurace připojení pro fronty služby Bus (který budou používat SDK Bus služby Azure `OnMessageReceive()` posluchače).
3. Napište jednoduché funkce pro volání koncový bod aplikace logiky (dříve vytvořili) pomocí fronty zprávu jako aktivační událost. Tady je kompletní příklad funkce. V příkladu se pomocí `application/json` zprávy typu obsahu, ale můžete to v případě potřeby změňte.

   ```
   using System;
   using System.Threading.Tasks;
   using System.Net.Http;
   using System.Text;

   private static string logicAppUri = @"https://prod-05.westus.logic.azure.com:443/.........";

   public static void Run(string myQueueItem, TraceWriter log)
   {

       log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
       using (var client = new HttpClient())
       {
           var response = client.PostAsync(logicAppUri, new StringContent(myQueueItem, Encoding.UTF8, "application/json")).Result;
       }
   }
   ```

Otestovat, můžete přidáte zprávu fronty pomocí nástroje podobného [Explorer Bus služby](https://github.com/paolosalvatori/ServiceBusExplorer). V tématu aplikaci logiku aktivováno hned po funkci, zobrazí se zpráva.

<!-- Image References -->
[1]: ./media/app-service-logic-scenario-function-sb-trigger/manualTrigger.PNG
[2]: ./media/app-service-logic-scenario-function-sb-trigger/newQueueTriggerFunction.PNG
