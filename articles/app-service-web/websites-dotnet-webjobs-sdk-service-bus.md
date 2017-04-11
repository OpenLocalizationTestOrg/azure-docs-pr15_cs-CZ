<properties 
    pageTitle="Jak Bus služby Azure pomocí WebJobs SDK" 
    description="Naučte se používat Bus služby Azure fronty a témata s WebJobs SDK." 
    services="app-service\web, service-bus" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="06/01/2016" 
    ms.author="tdykstra"/>

# <a name="how-to-use-azure-service-bus-with-the-webjobs-sdk"></a>Jak používat Bus služby Azure s WebJobs SDK

## <a name="overview"></a>Základní informace

Tato příručka obsahuje C# ukázek kódu, které ukazují, jak při doručení zprávy Bus služby Azure spustí proces. Ukázky kódu používat verzi [WebJobs SDK](websites-dotnet-webjobs-sdk.md) 1.x.

Průvodce předpokládá, že víte, [jak vytvořit projekt WebJob ve Visual Studiu s připojovací řetězec, které odkazují ke svému účtu úložiště](websites-dotnet-webjobs-sdk-get-started.md).

Fragmenty kódu zobrazí jenom funkce, není kód, který vytvoří `JobHost` objekt jako v následujícím příkladu:

```
public class Program
{
   public static void Main()
   {
      JobHostConfiguration config = new JobHostConfiguration();
      config.UseServiceBus();
      JobHost host = new JobHost(config);
      host.RunAndBlock();
   }
}
```

[Dokončení příkladu Bus služby](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Program.cs) je v úložišti azure webjobs sdk odběrů na GitHub.com.

## <a id="prerequisites"></a>Zjistit předpoklady pro

Pro práci s služby Bus budete muset nainstalovat balíček [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/) NuGet kromě WebJobs SDK balíčků. 

Je taky potřeba nejdřív nastavit připojovací řetězec AzureWebJobsServiceBus kromě řetězce připojení úložiště.  Můžete provést `connectionStrings` oddíl konfiguračního souboru, jak je vidět v následujícím příkladu:

        <connectionStrings>
            <add name="AzureWebJobsDashboard" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsServiceBus" connectionString="Endpoint=sb://[yourServiceNamespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[yourKey]"/>
        </connectionStrings>

Ukázka projekt, který obsahuje řetězec nastavení připojení služby Bus v konfiguračního souboru najdete v článku [Příklad Bus služby](https://github.com/Azure/azure-webjobs-sdk-samples/tree/master/BasicSamples/ServiceBus). 

Jsou řetězce připojení můžete nastavit taky v prostředí Azure runtime potom přepíše nastavení App.config při spuštění WebJob v Azure; Další informace najdete v tématu [Začínáme s WebJobs SDK](websites-dotnet-webjobs-sdk-get-started.md#configure-the-web-app-to-use-your-azure-sql-database-and-storage-account).

## <a id="trigger"></a>Jak aktivovat funkci při příjmu zprávy fronty Bus služby

Psaní funkci, která volá WebJobs SDK po přijetí frontě zprávu, můžete `ServiceBusTrigger` atribut. Konstruktoru atributu má parametr, který určuje název fronty tak, aby hlasování.

### <a name="how-servicebustrigger-works"></a>Jak funguje ServiceBusTrigger

V SDK obdrží zprávu v `PeekLock` režimu a volání `Complete` na zprávy v případě funkci dokončení úspěšně nebo volání `Abandon` selže funkci. Pokud funkce trvá déle, než `PeekLock` automaticky obnoven časový limit, zámek.

Služba Bus znamená vlastní nemůže ovládat nebo nakonfigurovaný tak, že WebJobs SDK poškozená fronty zpracování. 

### <a name="string-queue-message"></a>Řetězec fronty zprávy

Následující příklad kódu přečte fronty zprávy, která obsahuje řetězec a zapíše řetězec do řídicího panelu WebJobs SDK.

        public static void ProcessQueueMessage([ServiceBusTrigger("inputqueue")] string message, 
            TextWriter logger)
        {
            logger.WriteLine(message);
        }

**Poznámka:** Pokud vytváříte fronty zpráv v aplikaci, která nepoužívá WebJobs SDK, zkontrolujte, chcete-li nastavit [BrokeredMessage.ContentType](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx) "text/prostý".

### <a name="poco-queue-message"></a>POCO fronty zprávy

V SDK bude automaticky rekonstruovat fronty zprávu, která obsahuje JSON pro POCO [(obyčejný původní objekt CLR](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) typu. Následující příklad kódu přečte fronty zprávu, která obsahuje `BlobInformation` objekt, který má `BlobName` vlastnosti:

        public static void WriteLogPOCO([ServiceBusTrigger("inputqueue")] BlobInformation blobInfo,
            TextWriter logger)
        {
            logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
        }

Ukázky znázorňující, jak používat vlastnosti POCO pro práci s objekty BLOB a tabulky ve stejné funkci najdete v článku [úložiště fronty verzi tohoto článku](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#pocoblobs).

Pokud kód, který vytvoří zprávu fronty nepoužívá WebJobs SDK, použijte kód podobně jako v následujícím příkladu:

        var client = QueueClient.CreateFromConnectionString(ConfigurationManager.ConnectionStrings["AzureWebJobsServiceBus"].ConnectionString, "blobadded");
        BlobInformation blobInformation = new BlobInformation () ;
        var message = new BrokeredMessage(blobInformation);
        client.Send(message);

### <a name="types-servicebustrigger-works-with"></a>Typy ServiceBusTrigger spolupracuje

Kromě `string` a POCO typů, můžete použít `ServiceBusTrigger` atribut s polem bajt nebo `BrokeredMessage` objektu.

## <a id="create"></a>Jak vytvořit služby Bus zpráv

Psát funkci, která vytvoří novou zprávu použití fronty `ServiceBus` atribut a do pole Název fronty předat konstruktoru atributu. 


### <a name="create-a-single-queue-message-in-a-non-async-function"></a>Vytvoření zprávy jedné fronty ve funkci není asynchronní

Následující příklad kódu používá výstupní parametr k vytvoření nové zprávy ve frontě s názvem "outputqueue" s stejný obsah jako zobrazí se zpráva ve frontě s názvem "inputqueue".

        public static void CreateQueueMessage(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] out string outputQueueMessage)
        {
            outputQueueMessage = queueMessage;
        }

Parametr výstup pro vytvoření zprávy jedné fronty může být v kterémkoliv z následujících typů:

* `string`
* `byte[]`
* `BrokeredMessage`
* Serializovatelný POCO typ, který určíte. Automaticky serializováni jako JSON.

Typ parametrů POCO fronta zprávu vždy vznikne po ukončení funkce; Pokud má parametr hodnotu null, vytvoří v SDK fronty zprávu, která vrátí hodnotu null při zprávy přijaté a převést ze sériového tvaru. Pro jiné typy, pokud má parametr hodnotu null žádné fronty zpráva je vytvořena.

### <a name="create-multiple-queue-messages-or-in-async-functions"></a>Vytvoření více zpráv nebo v asynchronních funkcí

Chcete-li vytvořit více zpráv, použijte `ServiceBus` atributem `ICollector<T>` nebo `IAsyncCollector<T>`, jak je ukázáno v následujícím příkladu:

        public static void CreateQueueMessages(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

Každá zpráva fronty vznikne ihned po `Add` s názvem metody.

## <a id="topics"></a>Jak pracovat s služby Bus témata

Psaní funkci, která volá SDK po přijetí zprávy služby Bus témat, můžete `ServiceBusTrigger` atributem konstruktor, který má název tématu a název předplatného, jak je vidět v následujícím příkladu:

        public static void WriteLog([ServiceBusTrigger("outputtopic","subscription1")] string message,
            TextWriter logger)
        {
            logger.WriteLine("Topic message: " + message);
        }

Vytvoření zprávy témat, můžete `ServiceBus` atribut s názvem téma stejným způsobem jako ho použít s názvem fronty.

## <a name="features-added-in-release-11"></a>Funkcí přidaných do verze 1.1

Následující funkce byly přidány verze 1.1:

* Povolit hloubkové přizpůsobení zpracování prostřednictvím zprávy `ServiceBusConfiguration.MessagingProvider`.
* `MessagingProvider`podporuje přizpůsobení Bus služby `MessagingFactory` a `NamespaceManager`.
* A `MessageProcessor` strategie vzorek vám umožní určit procesor na fronty nebo tématu.
* Ve výchozím nastavení je podporována souběžné zpracování zprávy. 
* Snadno přizpůsobení `OnMessageOptions` prostřednictvím `ServiceBusConfiguration.MessageOptions`.
* Povolit [AccessRights](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Functions.cs#L71) být zadán v `ServiceBusTriggerAttribute` / `ServiceBusAttribute` (pro scénáře, kdy nemáte Správa přístupových práv). 

## <a id="queues"></a>Související témata článek s postupy paměťových fronty

Informace o WebJobs SDK scénáře nejsou specifické pro službu Bus najdete v tématu [Použití úložiště Azure fronty s WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

Témata v tomto článku patřit následující úkoly:

* Funkce asynchronní
* Více instancí
* Vypnutí
* Použít atributy WebJobs SDK v těle funkce
* Nastavení připojení řetězce SDK v kódu
* Nastavení hodnot pro WebJobs SDK konstruktor parametrů v kódu
* Aktivace funkce ručně
* Zápisu protokolů

## <a id="nextsteps"></a>Další kroky

Tato příručka dodala ukázek kódu, které ukazují, jak řešit obvyklé scénáře pro práci s Bus služby Azure. Další informace o používání Azure WebJobs a WebJobs SDK tématech [Azure WebJobs doporučené](http://go.microsoft.com/fwlink/?linkid=390226).
 
