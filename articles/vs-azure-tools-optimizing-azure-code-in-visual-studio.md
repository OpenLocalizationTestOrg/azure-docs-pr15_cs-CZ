<properties
   pageTitle="Optimalizace Azure kódu ve Visual Studiu | Microsoft Azure"
   description="Přečtěte si o tom, jak Azure kódu nástroje optimalizace ve Visual Studiu pomohli kódu robustní a lépe provést."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="optimizing-your-azure-code"></a>Optimalizace kódu Azure

Při plánování aplikace, které používají Microsoft Azure existují některé kódování postupy, pomocí kterých můžete by se tím vyhnout potížím s škálovatelnost aplikace, chování a výkonu v prostředí cloudu. Poskytuje společnost Microsoft Azure kód analytický nástroj, který rozpozná a identifikuje některé z těchto problémů běžně narazili a vám pomůže vyřešit. Stáhněte si nástroj ve Visual Studiu prostřednictvím NuGet.

## <a name="azure-code-analysis-rules"></a>Azure pravidla analýza kódu

Azure kód analytického nástroje používá následující pravidla automaticky označení Azure kódu při nalezení známé problémy ovlivňující ochranu výkonu. Zjištěny problémy se zobrazí jako upozornění nebo chyby kompilátoru. Opravy kódu návrhy řešení upozornění nebo Chyba často poskytované nebo prostřednictvím žárovky ikony.

## <a name="avoid-using-default-in-process-session-state-mode"></a>Eliminování možnosti použití výchozí režim stavu relace (na obrázku)

### <a name="id"></a>ID

AP0000

### <a name="description"></a>Popis

Pokud používáte výchozí režim stavu relace (na obrázku) získáte cloudu, může dojít ke ztrátě stavu relace.

Zkontrolujte sdílejte nápady a názory na [názory analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Důvod, proč

Ve výchozím nastavení je režim stavu relace podle nastavení(Web.config)) v procesu. Také pokud žádná položka zadaná v souboru konfigurace režimu stavu relace ve výchozím nastavení v procesu. Režim v procesu ukládá stav relace v paměti na webový server. Když se nerestartuje instance nebo nové instance se používá pro vyrovnávání zatížení nebo selháním, se neuloží stav relace uložené v paměti na webovém serveru. Tato situace zabraňuje aplikaci před scalable v cloudu.

Stav relace ASP.NET podporuje několik různých možností ukládání pro data stavu relace: InProc StateServer, SQL Server, vlastní a možnost Vypnuto. Se doporučuje používat vlastní režim hostitele data pro externí úložiště stavu relace, například [poskytovatele Azure relace stavu Redis](http://go.microsoft.com/fwlink/?LinkId=401521).

### <a name="solution"></a>Řešení

Jeden doporučené řešení je k ukládání stavu relace na služby spravovaných mezipaměti. Naučte se používat [poskytovatele Azure relace stavu Redis](http://go.microsoft.com/fwlink/?LinkId=401521) k ukládání stavu relace. Stav relace lze také uložit na jiných místech a zkontrolujte, že aplikace scalable v cloudu. Další informace o alternativním přečtěte řešení [Režimy stavu relace](https://msdn.microsoft.com/library/ms178586).

## <a name="run-method-should-not-be-async"></a>Spuštění metoda by neměly být asynchronní

### <a name="id"></a>ID

AP1000

### <a name="description"></a>Popis

Vytvořte asynchronních metod (například [očekávat](https://msdn.microsoft.com/library/hh156528.aspx)) mimo metody [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) a potom metody asynchronní volat z [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx). Deklarace metody [[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) jako asynchronní způsobí, že roli pracovníka zadat smyčku restartování počítače.

Zkontrolujte sdílejte nápady a názory na [názory analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Důvod, proč

Volání asynchronní metod uvnitř metody [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) způsobí, že modul runtime služby cloudu odpadkového roli kolegy. Po spuštění pracovního role všechny spuštění programu probíhá uvnitř metody [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) . Ukončení metody [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) způsobí, že roli pracovníka restartovat. Když role runtime pracovní narazí asynchronní metody, odešle všechny operace za metodu asynchronní a vrátí. To způsobí, že roli pracovníka z metody [[[[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ukončíte a restartujete. V dalším opakování spuštění roli pracovní narazí metodu asynchronní znovu a znovu a příčinou roli pracovníka do koše znovu taky.

### <a name="solution"></a>Řešení

Umístěte všechny operace asynchronní mimo metody [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) . Volejte metodu refactored asynchronní z uvnitř metody [[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) například .wait RunAsync (). Azure kód analytického nástroje můžete problém vyřešit.

Následující fragment kódu ukazuje oprava kódu pro tento problém:

```
public override void Run()
{
    RunAsync().Wait();
}

public async Task RunAsync()
{
    //Asynchronous operations code logic
    // This is a sample worker implementation. Replace with your logic.

    Trace.TraceInformation("WorkerRole1 entry point called");

    HttpClient client = new HttpClient();

    Task<string> urlString = client.GetStringAsync("http://msdn.microsoft.com");

    while (true)
    {
        Thread.Sleep(10000);
        Trace.TraceInformation("Working");

        string stream = await urlString;
    }

}
```

## <a name="use-service-bus-shared-access-signature-authentication"></a>Použít podpis přístup sdílené Bus služby ověřování

### <a name="id"></a>ID

AP2000

### <a name="description"></a>Popis

Použití sdílené přístup podpisu (přidružení) ověřování. Služby Řízení přístupu (ACS) je zastaralý pro ověření bus služby.

Zkontrolujte sdílejte nápady a názory na [názory analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Důvod, proč

Zvýšit úroveň zabezpečení Azure Active Directory je nahradit ACS ověřování ověřování přidružení zabezpečení. Informace o plánu přechodu najdete v článku [Azure Active Directory je budoucnost ACS](http://blogs.technet.com/b/ad/archive/2013/06/22/azure-active-directory-is-the-future-of-acs.aspx) .

### <a name="solution"></a>Řešení

Pomocí ověřování přidružení zabezpečení v aplikací. Následující příklad ukazuje, jak používat existující token přidružení zabezpečení pro přístup ke obor názvů bus služby či entitu.

```
MessagingFactory listenMF = MessagingFactory.Create(endpoints, new StaticSASTokenProvider(subscriptionToken));
SubscriptionClient sc = listenMF.CreateSubscriptionClient(topicPath, subscriptionName);
BrokeredMessage receivedMessage = sc.Receive();
```

Následujících tématech Další informace.

- Přehled najdete v článku [Sdílené ověřování přístupu k podpisu Bus služby](https://msdn.microsoft.com/library/dn170477.aspx)

- [Použití sdílené ověřování podpisu s Bus služby](https://msdn.microsoft.com/library/dn205161.aspx)

- Ukázkový projekt v tématu [používání sdílených přístup podpisu (přidružení zabezpečení) ověřování u předplatných Bus služby](http://code.msdn.microsoft.com/windowsazure/Using-Shared-Access-e605b37c)

## <a name="consider-using-onmessage-method-to-avoid-receive-loop"></a>Zvažte metodou OnMessage vyhnout "přijímání smyčka"

### <a name="id"></a>ID

AP2002

### <a name="description"></a>Popis

Chcete-li předejít přejdete do "přijímání smyčka", **OnMessage** metoda je lepší řešení pro příjem zpráv než metodu **přijmout** volání. Ale pokud je nutné použít způsob **přijímání** a zadáte serveru jiného než výchozího doba, ujistěte se, že prodleva serveru víc než jednu minutu.

Zkontrolujte sdílejte nápady a názory na [názory analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Důvod, proč

Při volání **OnMessage**, spustí klienta pumpu interní zpráv, které neustále zjišťuje fronty nebo předplatného. Tato zpráva čerpadlo obsahuje nekonečné smyčce obsahujícího volání přijímat zprávy. Pokud chcete hovor vyprší její časový limit, odešle nového volání. Časový limit, je určený hodnotou ve vlastnosti [OperationTimeout](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx) [MessagingFactory](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactory.aspx), který používá.

Výhodou používání **OnMessage** ve srovnání s **přijímání** je, aby uživatelé neměli ručně hlasování pro zprávy, zpracování výjimek, zpracování více zpráv paralelně a dokončete zprávy.

Pokud voláte **přijímání** bez použití použita výchozí hodnota, ujistěte se, že je hodnota *ServerWaitTime* víc než jednu minutu. Nastavení *ServerWaitTime* na víc než jednu minutu zabrání serveru vypršel časový limit před úplně doručení zprávy.

### <a name="solution"></a>Řešení

Najdete následující příklady kódu pro doporučené použití. Další informace najdete v tématu [QueueClient.OnMessage metody (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.onmessage.aspx)a [QueueClient.Receive (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.receive.aspx).

Aby se zvýšil výkon Azure infrastrukturu zasílání zpráv, najdete v článku vzorek návrh [Asynchronní základy zasílání zpráv](https://msdn.microsoft.com/library/dn589781.aspx).

Následující obrázek je příkladem použití **OnMessage** přijímat zprávy.

```
void ReceiveMessages()
{
    // Initialize message pump options.
    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = true; // Indicates if the message-pump should call complete on messages after the callback has completed processing.
    options.MaxConcurrentCalls = 1; // Indicates the maximum number of concurrent calls to the callback the pump should initiate.
    options.ExceptionReceived += LogErrors; // Enables you to get notified of any errors encountered by the message pump.

    // Start receiving messages.
    QueueClient client = QueueClient.Create("myQueue");
    client.OnMessage((receivedMessage) => // Initiates the message pump and callback is invoked for each message that is recieved, calling close on the client will stop the pump.
    {
        // Process the message.
    }, options);
    Console.WriteLine("Press any key to exit.");
    Console.ReadKey();
```

Následující obrázek je příkladem **přijímání** pomocí prodleva výchozí server.

```
string connectionString =  
CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

QueueClient Client =  
    QueueClient.CreateFromConnectionString(connectionString, "TestQueue");

while (true)  
{   
   BrokeredMessage message = Client.Receive();
   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
```

Následující obrázek je příkladem **přijímání** pomocí jiného než výchozího serveru prodleva.

```
while (true)  
{   
   BrokeredMessage message = Client.Receive(new TimeSpan(0,1,0));

   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
}
```
## <a name="consider-using-asynchronous-service-bus-methods"></a>Zvažte použití asynchronních metod Bus služby

### <a name="id"></a>ID

AP2003

### <a name="description"></a>Popis

Způsobem asynchronní Bus služby pro zvýšení výkonu brokered zpráv.

Informujte je o nápady a názory na [Azure Code Analysis názory](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Důvod, proč

Použití asynchronní metody umožňuje souběžné program aplikace, protože provedení každého volání není blokovat hlavní podproces. Při používání služeb Bus zpráv metody, provedení operace (odeslat, přijmout, odstranit, apod) dlouho. Tentokrát obsahuje zpracování operace službou služby Bus kromě latence vaší žádosti a odpověď. Pokud chcete zvýšit počet operací podle času, musí současně provést operace. Další informace získáte [Doporučené postupy pro výkon vylepšení pomocí služby Bus Brokered zasílání zpráv](https://msdn.microsoft.com/library/azure/hh528527.aspx).

### <a name="solution"></a>Řešení

Informace o tom, jak použijte metodu doporučené asynchronní najdete v článku [QueueClient třídy (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.aspx) .

Aby se zvýšil výkon Azure infrastrukturu zasílání zpráv, najdete v článku vzorek návrh [Asynchronní základy zasílání zpráv](https://msdn.microsoft.com/library/dn589781.aspx).

## <a name="consider-partitioning-service-bus-queues-and-topics"></a>Zvažte rozdělení služby Bus fronty a témata

### <a name="id"></a>ID

AP2004

### <a name="description"></a>Popis

Oddíl služby Bus fronty a témata nápovědy pro lepší výkon služby Bus zpráv.

Zkontrolujte sdílejte nápady a názory na [názory analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Důvod, proč

Rozdělení služby Bus fronty a témata zvýšíte výkon výkon a přihlašovacích údajů dostupnost, protože celkový výkon rozdělený fronty nebo tématu už není omezena výkon zprostředkovatele podstatu sdělení nebo zpráv úložiště. Kromě toho dočasné výpadku zpráv úložiště nemá zkontrolujte rozdělený fronty nebo tématu není k dispozici. Další informace najdete v tématu [Rozdělení zpráv entity](https://msdn.microsoft.com/library/azure/dn520246.aspx).

### <a name="solution"></a>Řešení

Následující fragment kódu znázorňuje oddíl zpráv entity.

```
// Create partitioned topic.
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

Další informace najdete v tématu [oddíly služby Bus fronty a témata | Microsoft Azure Blog](https://azure.microsoft.com/blog/2013/10/29/partitioned-service-bus-queues-and-topics/) a najdete v tématu [Microsoft Azure Bus oddíly frontě](https://code.msdn.microsoft.com/windowsazure/Service-Bus-Partitioned-7dfd3f1f) vzorku.

## <a name="do-not-set-sharedaccessstarttime"></a>Nejsou nastavené SharedAccessStartTime

### <a name="id"></a>ID

AP3001

### <a name="description"></a>Popis

Byste neměli používat SharedAccessStartTimeset aktuální čas okamžitě začít sdílet přístupu. Stačí nastavovaná vlastnost, pokud chcete začít sdílet přístupu později.

Zkontrolujte sdílejte nápady a názory na [názory analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Důvod, proč

Synchronizace hodin způsobí, že lehké časový rozdíl mezi datacentrech. Třeba byste logicky by podle vás nastavení času spuštění úložiště přidružení zabezpečení zásady jako aktuální čas pomocí DateTime.Now nebo podobnou metodou způsobí zásadu přidružení zabezpečení se projeví okamžitě. Lehké čas rozdíly mezi datacentrech však může způsobit potíže s tímto od některých případech datacentra může být trochu pozdější než počáteční čas, zatímco jiné před ho. V důsledku toho můžete rychle (nebo dokonce okamžitě) vypršení platnosti zásad přidružení zabezpečení Pokud životnost zásady nastavená kratší.

Další podpisem sdílené přístup Azure úložný prostor, najdete v článku [Úvod do tabulky přidružení zabezpečení (sdílené přístup podpis), přidružení fronty zabezpečení a aktualizace přidružení objektů Blob zabezpečení – Blogy MSDN Microsoft Azure úložiště týmového blogu – Domovská stránka webu](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).

### <a name="solution"></a>Řešení

Odebrání příkazu, který nastavuje počáteční čas dané sdílené přístupu. Azure kód analytický nástroj poskytuje opravu pro tento problém. Další informace o správě zabezpečení najdete v článku vzorek návrh [Osobní služby klíč vzorku](https://msdn.microsoft.com/library/dn568102.aspx).

Následující fragment kódu ukazuje oprava kódu pro tento problém.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

## <a name="shared-access-policy-expiry-time-must-be-more-than-five-minutes"></a>Sdílené zásady přístupu čas vypršení musí být delší než pět minut

### <a name="id"></a>ID

AP3002

### <a name="description"></a>Popis

Může být velmi podobným způsobem jako pět minut rozdíl v hodiny mezi datacentrech na různých místech kvůli podmínky jmenoval "hodiny zkosení." Nechcete, aby přidružení zabezpečení token zásad vypršení platnosti dříve, než plánované, nastavte hodnotu time vypršení platnosti být delší než pět minut.

Zkontrolujte sdílejte nápady a názory na [názory analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Důvod, proč

Datacentrech na různých místech ve světě synchronizovat signál hodiny. Protože má čas hodiny signál projít na různých místech, může být čas odchylka datacentrech na různých místech zeměpisné sice všechno, co která synchronizovat. Časový rozdíl může ovlivnit Shared Access zásad počáteční čas a vypršení platnosti interval. Proto abyste měli jistotu, že zásady přístupu sdílené se projeví okamžitě, není zadat počáteční čas. Kromě toho Ujistěte se, že doby vypršení platnosti víc než 5 minut nechcete, aby nejdříve možné časový limit.

Další informace o používání sdílené podpis přístup Azure úložný prostor najdete v článku [Úvod do tabulky přidružení zabezpečení (sdílené přístup podpis), přidružení fronty zabezpečení a aktualizace přidružení objektů Blob zabezpečení – Blogy MSDN Microsoft Azure úložiště týmového blogu – Domovská stránka webu](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).

### <a name="solution"></a>Řešení

Další informace o správě zabezpečení najdete v tématu návrh vzorek [Osobní služby klíč vzorku](https://msdn.microsoft.com/library/dn568102.aspx).

Následující obrázek je příkladem bez zadání čas začátku zásad sdílené přístup.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

Následující obrázek je příkladem určující čas začátku zásady přístupu sdílené s dobou trvání zásad vypršení platnosti větší než pět minut.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
  SharedAccessStartTime = new DateTime(2014,1,20),   
 SharedAccessExpiryTime = new DateTime(2014, 1, 21),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

Další informace najdete v tématu [Vytvoření a používání podpisu sdílené aplikace Access](https://msdn.microsoft.com/library/azure/jj721951.aspx).

## <a name="use-cloudconfigurationmanager"></a>Použití CloudConfigurationManager

### <a name="id"></a>ID

AP4000

### <a name="description"></a>Popis

Použití třídy [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager(v=vs.110).aspx) pro projekty například webu Azure a Azure mobilních služeb nebude Představte runtime problémy. Osvědčený ale je vhodné použít cloudu[ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager(v=vs.110).aspx) jako jednotné způsob správy konfigurace pro všechny aplikace Azure cloudu.

Zkontrolujte sdílejte nápady a názory na [názory analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Důvod, proč

CloudConfigurationManager přečte konfiguračního souboru třeba prostředí aplikace.

[CloudConfigurationManager](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx)

### <a name="solution"></a>Řešení

Refaktorovat kódu pro použití [CloudConfigurationManager předmětu](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx). Oprava kódu pro tento problém poskytuje společnost Azure kód analytický nástroj.

Následující fragment kódu ukazuje oprava kódu pro tento problém. Nahrazení

`var settings = ConfigurationManager.AppSettings["mySettings"];`

s

`var settings = CloudConfigurationManager.GetSetting("mySettings");`

Tady je příklad toho, jak ukládat nastavení konfigurace v souboru App.config nebo Web.config. Přidáte nastavení do části appSettings konfiguračního souboru. Následující obrázek je nastavení(Web.config)) v předchozím příkladu kódu.

```
<appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="mySettings" value="[put_your_setting_here]"/>
  </appSettings>  
```

## <a name="avoid-using-hard-coded-connection-strings"></a>Eliminování možnosti použití pevně připojovací řetězec

### <a name="id"></a>ID

AP4001

### <a name="description"></a>Popis

Pokud byste potřebovali aktualizovat později použijete pevně připojovací řetězec, budete muset provést změny zdrojového kódu a zkompilujte. Ale pokud obsahují připojovací řetězec v souboru konfigurace změníte je později jednoduše aktualizací konfiguračního souboru.

Zkontrolujte sdílejte nápady a názory na [názory analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Důvod, proč

Pevné kódování připojovací řetězec totiž chybná praktická část představuje potíže při připojovací řetězec potřebujete rychle změnit. Kromě toho pokud projekt musí být vrácený se změnami do ovládacího prvku zdroje, pevně připojovací řetězec Představte zabezpečením od řetězce je možné zobrazit ve zdrojovém kódu.

### <a name="solution"></a>Řešení

Obsahují řetězce připojení v konfiguraci soubory nebo Azure prostředí.

- Pro samostatné aplikace umožňuje app.config uložit nastavení řetězec připojení.

- U hostující na serveru IIS webových aplikací umožňuje web.config ukládat připojovací řetězec.

- Aplikace technologie ASP.NET vNext umožňuje configuration.json ukládání připojovací řetězec.

Informace o používání souborů konfigurace web.config ATP app.config najdete v tématu [Pokyny ke konfiguraci webové ASP.NET](https://msdn.microsoft.com/library/vstudio/ff400235(v=vs.100).aspx). Další informace o způsobu Azure proměnné pracovního prostředí, najdete v článku [Azure weby: jak řetězce aplikace a připojení řetězce fungovat](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). Další informace o ukládání připojovací řetězec do ovládacího prvku zdroje najdete v článku [nevkládejte citlivé informace, například připojení řetězce v souborech, které jsou uložené v úložišti zdroje kódu](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control).

## <a name="use-diagnostics-configuration-file"></a>Použití diagnostiky konfiguračního souboru

### <a name="id"></a>ID

AP5000

### <a name="description"></a>Popis

Místo a nakonfigurujte nastavení diagnostických nástrojů v kódu jako Microsoft.WindowsAzure.Diagnostics programové rozhraní API, měli byste nakonfigurovat nastavení diagnostických nástrojů v souboru diagnostics.wadcfg. (Nebo diagnostics.wadcfgx použijete Azure SDK 2,5). Tímto způsobem, můžete změnit nastavení diagnostických nástrojů aniž byste museli překompilovat kódu.

Zkontrolujte sdílejte nápady a názory na [názory analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Důvod, proč

Před Azure SDK 2,5 (které využívá Azure diagnostiky 1.3), Azure diagnostiky (WAD) lze nastavit pomocí několika způsoby: pomocí naléhavé kódu, deklarativní konfiguraci nebo výchozí konfigurace ho přidáte na objektů blob konfigurace v úložišti. Konfigurace diagnostiky upřednostňovaný způsob je však pomocí konfiguračního souboru XML (diagnostics.wadcfg nebo diagnositcs.wadcfgx pro SDK 2,5 a novější) v aplikaci project. V tomto přístupu diagnostics.wadcfg soubor úplně Určuje konfiguraci a lze aktualizovat a znovu nasadit kdykoli. Míchání pomocí konfiguračního souboru diagnostics.wadcfg s programové metody konfigurace nastavení pomocí tříd [DiagnosticMonitor](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.diagnosticmonitor.aspx)nebo [RoleInstanceDiagnosticManager](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.management.roleinstancediagnosticmanager.aspx)může způsobit zmatky. Další informace najdete v článku [Inicializace nebo konfigurace diagnostiky Azure změnit](https://msdn.microsoft.com/library/azure/hh411537.aspx) .

Začínající WAD 1.3 (zahrnutý v sadě Azure SDK 2,5), je už není možné použít kód pro nastavení diagnostických nástrojů. V důsledku toho můžete jenom poskytnout konfiguraci při použití nebo aktualizaci koncovku diagnostických nástrojů.

### <a name="solution"></a>Řešení

Pomocí návrháře diagnostiky konfigurace přesunutí diagnostiky nastavení diagnostických nástrojů konfiguračního souboru (diagnositcs.wadcfg nebo diagnositcs.wadcfgx pro SDK 2,5 a novější). Také se doporučuje instalace [Azure SDK 2,5](http://go.microsoft.com/fwlink/?LinkId=513188) a použijte nejnovější funkce diagnostických nástrojů.

1. V místní nabídce role, kterou chcete konfigurovat vyberte vlastnosti a klikněte na kartu konfigurace.

1. Ujistěte se, že je zaškrtnuté políčko **Povolit Diagnostika** v části **Diagnostics** .

1. Klikněte na tlačítko **Konfigurace** .

  ![Přístup k možnost povolit diagnostických nástrojů](./media/vs-azure-tools-optimizing-azure-code-in-visual-studio/IC796660.png)

  Další informace najdete v tématu [Konfigurace diagnostických nástrojů pro Azure cloudovými službami a virtuálních počítačích](vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) .


## <a name="avoid-declaring-dbcontext-objects-as-static"></a>Vyhněte se deklarování jako statické objekty DbContext

### <a name="id"></a>ID

AP6000

### <a name="description"></a>Popis

Uložte paměti, snažte se deklarování jako statické objekty DBContext.

Zkontrolujte sdílejte nápady a názory na [názory analýza kódu Azure](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Důvod, proč

Objekty DBContext podržte výsledků dotazu v každé volání. Statické objekty DBContext nejsou uvolnit, dokud je doména aplikace uvolněním. Proto statického DBContext objektu můžete využívat velké množství paměti.

### <a name="solution"></a>Řešení

Deklarovat DBContext jako lokální proměnné nebo pole není statického instance, použijte pro daný úkol a nechat ji prodávat po použití.

Následující příklad třídy řadiče MVC ukazuje, jak pomocí DBContext objektu.

```
public class BlogsController : Controller
    {
        //BloggingContext is a subclass to DbContext        
        private BloggingContext db = new BloggingContext();
        // GET: Blogs
        public ActionResult Index()
        {
            //business logics…
            return View();
        }
        protected override void Dispose(bool disposing)
        {
            if (disposing)
            {
                db.Dispose();
            }
            base.Dispose(disposing);
        }
    }
```

## <a name="next-steps"></a>Další kroky

Další informace o optimzing a řešení problémů s Azure aplikace, najdete v článku [Poradce při potížích s web app v aplikaci služby Azure pomocí aplikace Visual Studio](./app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).
