<properties
    pageTitle="Použití služby Bus témata s .NET | Microsoft Azure"
    description="Naučte se používat službu Bus témata a předplatná s .NET v Azure. Ukázky jsou určena .NET aplikace."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="09/16/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Používání služby Bus témata a předplatného

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Tento článek popisuje, jak používat službu Bus témata a předplatná. Vzorky napsané v jazyce C# a pomocí rozhraní API .NET. Scénáře nichž se uplatní zahrnují vytváření témata a předplatná, vytváření filtrů předplatné posílání zpráv do tématu, přijímat zprávy z předplatného a odstranění témata a předplatná. Další informace o témata a předplatná naleznete v části [Další kroky](#next-steps) .

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="configure-the-application-to-use-service-bus"></a>Nakonfigurovat aplikaci pro použití služby Bus

Při vytváření aplikace, která používá Bus služby, musíte přidat odkaz na službu Bus sestavení a zahrnout odpovídající obory názvů. Nejjednodušší způsob je stáhnout balíček pro příslušný [NuGet](https://www.nuget.org) .

## <a name="get-the-service-bus-nuget-package"></a>Získání balíček NuGet Bus služby

[Služba Bus NuGet balíčku](https://www.nuget.org/packages/WindowsAzure.ServiceBus) je nejjednodušší a chcete nakonfigurovat aplikaci se všechny potřebné závislostmi služby Bus si rozhraní API Bus služby. K instalaci balíčku služby Bus NuGet v projektu, postupujte takto:

1.  V Průzkumníku klikněte pravým tlačítkem na **odkazy**a potom klikněte na **Spravovat balíčků NuGet**.
2.  Vyhledejte "Služby Bus" a vyberte položku **Bus služby Microsoft Azure** . Klikněte na **nainstalovat** a dokončete instalaci a potom zavřete dialogové okno takto:

    ![][7]

Teď jste připraveni vytvořit kód pro službu Bus.

## <a name="create-a-service-bus-connection-string"></a>Vytvoření připojovacího řetězce Bus služby

Služby Bus používá připojovací řetězec pro ukládání koncové body a přihlašovací údaje. V souboru konfigurace, spíše než pevně zadány ho můžete dát připojovací řetězec:

- Při používání služby Azure, doporučujeme uložit připojovací řetězec v systému služby Azure konfigurace (soubory .csdef a .cscfg).
- Když používáte Azure weby nebo Azure virtuálních počítačích, doporučujeme uložit připojovací řetězec v systému .NET konfigurace (například nastavení(Web.config))).

V obou případech je možné získat pomocí připojení řetězce `CloudConfigurationManager.GetSetting` způsob, jak je uvedeno dále v tomto článku.

### <a name="configure-your-connection-string"></a>Konfigurace připojovacího řetězce

Konfigurace mechanismus služby umožňuje dynamicky měnit nastavení z [Azure portál][] bez znovu nasazení aplikace. Například, přidejte `Setting` popisek, který se soubor definice (**.csdef**) služby, jak je vidět v následujícím příkladu.

```
<ServiceDefinition name="Azure1">
...
    <WebRole name="MyRole" vmsize="Small">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString" />
        </ConfigurationSettings>
    </WebRole>
...
</ServiceDefinition>
```

Pak zadejte hodnoty v souboru konfigurace (.cscfg) služby.

```
<ServiceConfiguration serviceName="Azure1">
...
    <Role name="MyRole">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString"
                     value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
        </ConfigurationSettings>
    </Role>
...
</ServiceConfiguration>
```

Použijte název klíče přidružení sdílené podpis aplikace Access (zabezpečení) a klíčové hodnoty načtené z portálu výše popsaným způsobem.

### <a name="configure-your-connection-string-when-using-azure-websites-or-azure-virtual-machines"></a>Konfigurace připojovací řetězec při použití Azure weby nebo virtuálních počítačích Azure

Když používáte weby nebo virtuálních počítačích, doporučujeme použít konfigurace systému .NET (například Web.config). Obsahují řetězce připojení pomocí `<appSettings>` prvek.

```
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
    </appSettings>
</configuration>
```

Použijte název přidružení zabezpečení a hodnoty klíče, které jste načtená z kontingenčního seznamu [Azure portál][]výše popsaným způsobem.

## <a name="create-a-topic"></a>Vytvoření tematického

Můžete provádět operace správy služby Bus témata a předplatných pomocí [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) předmětu. Tato třída poskytuje metody vytvořit, zobrazit výčet a odstraňovat témata.

Následující příklad vytvoří `NamespaceManager` objektu pomocí Azure `CloudConfigurationManager` třídy s připojovací řetězec obsahující základní adresu obor Bus služby a odpovídající přidružení zabezpečení pověření s oprávněním ke správě ho. Tento připojovací řetězec je takto:

```
Endpoint=sb://<yourNamespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<yourKey>
```

Použijte následující příklad zadaných nastavení v předchozí části.

```
// Create the topic if it does not exist already.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic("TestTopic");
}
```

Existují přetížení [CreateTopic](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.createtopic.aspx) metody, které vám umožní nastavení vlastností v tématu; například nastavení výchozího time to live (TTL) hodnota, která má být použity na zprávy poslané na téma. Tato nastavení nejsou použity prostřednictvím [TopicDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.aspx) předmětu. Následující příklad ukazuje, jak vytvořit téma s názvem **TestTopic** s maximální velikosti doručovaných 5 GB a zprávu výchozí TTL 1 minuty.

```
// Configure Topic Settings.
TopicDescription td = new TopicDescription("TestTopic");
td.MaxSizeInMegabytes = 5120;
td.DefaultMessageTimeToLive = new TimeSpan(0, 1, 0);

// Create a new Topic with custom settings.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic(td);
}
```

> [AZURE.NOTE] Můžete metodu [TopicExists](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.topicexists.aspx) objektům [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) zkontrolujte, zda téma se zadaným názvem už existuje v rámci obor.

## <a name="create-a-subscription"></a>Vytvoření předplatného

Můžete taky vytvořit téma předplatného pomocí [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) předmětu. Předplatné se s názvem a může obsahovat volitelné filtr, který omezuje sadu zpráv do fronty virtuální předplatného.

> [AZURE.IMPORTANT] Aby zpráv do dostali předplatné musíte vytvořit tuto předplatné před odesláním všechny zprávy na téma. Pokud jsou žádné předplatné na téma, tématu ponechává tyto zprávy.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Vytvoření předplatné s filtrem výchozí (MatchAll)

V případě bez filtru při vytvoření nové předplatné filtr **MatchAll** je výchozí filtr, který se používá. Při použití filtru **MatchAll** všech zpráv publikovaných na téma jsou umístěny ve frontě virtuální předplatného. Následující příklad vytvoří předplatné s názvem "AllMessages" a používá jako výchozí **MatchAll** filtr.

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.SubscriptionExists("TestTopic", "AllMessages"))
{
    namespaceManager.CreateSubscription("TestTopic", "AllMessages");
}
```

### <a name="create-subscriptions-with-filters"></a>Vytvoření předplatného s filtry

Můžete taky nastavit filtry, které umožňují určit, které zprávy poslané na téma by se měly objevit během určitého tématu předplatné.

Flexibilní typ filtru nepodporuje pro předplatná je třída [SqlFilter][] , které podmnožinu SQL92. Filtry SQL pracují s vlastnostmi zprávy, které jsou publikované na téma. Další informace o výrazech, které lze použít s filtrem SQL najdete v článku [SqlFilter.SqlExpression][] syntaxe.

Následující příklad vytvoří předplatné s názvem **HighMessages** [SqlFilter][] objekt, který pouze vybere zprávy, které mají vlastních vlastností **MessageNumber** větší než 3.

```
// Create a "HighMessages" filtered subscription.
SqlFilter highMessagesFilter =
   new SqlFilter("MessageNumber > 3");

namespaceManager.CreateSubscription("TestTopic",
   "HighMessages",
   highMessagesFilter);
```

Následující příklad vytvoří podobně předplatné s názvem **LowMessages** s [SqlFilter][] pouze vybere zprávy, které mají **MessageNumber** vlastnost menším větší nebo rovnu na 3.

```
// Create a "LowMessages" filtered subscription.
SqlFilter lowMessagesFilter =
   new SqlFilter("MessageNumber <= 3");

namespaceManager.CreateSubscription("TestTopic",
   "LowMessages",
   lowMessagesFilter);
```

Teď když zprávu se neodesílají `TestTopic`, vždy Doručená do příjemce, nemáte předplatné předplatné téma **AllMessages** a selektivním doručila ji do příjemce, nemáte předplatné **HighMessages** a **LowMessages** téma předplatná (podle toho, obsah zprávy).

## <a name="send-messages-to-a-topic"></a>Odeslání zprávy na téma

Zaslání zprávy na téma Bus služby aplikace vytvoří objekt [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) pomocí připojovacího řetězce.

Následující kód ukazuje, jak vytvořit objekt [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) **TestTopic** tématu vytvořili dříve pomocí [`CreateFromConnectionString`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.createfromconnectionstring.aspx) rozhraní API volání.

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

TopicClient Client =
    TopicClient.CreateFromConnectionString(connectionString, "TestTopic");

Client.Send(new BrokeredMessage());
```

Zprávy zasílané do služby Bus témata jsou instancemi tříd [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) . [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) objekty mají sadu standardní vlastnosti (například [štítky](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) a [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)) slovník, který se používá k ukládání vlastní vlastnosti specifické pro aplikaci a textu data libovolného aplikací. Aplikaci můžete nastavit textu zprávy předáním serializovatelný objekt konstruktoru [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) objektu a odpovídající **DataContractSerializer** je pak použít ke Serializujte objekt. Můžete taky můžete zadat **System.IO.Stream** .

Následující příklad ukazuje, jak pět zkušební zprávy odešlete objekt [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) **TestTopic** získáte v předchozím příkladu. Všimněte si, že hodnotu [MessageNumber](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.properties.aspx) každé zprávy se liší v závislosti na opakování smyčky (Tato možnost určuje které předplatná obdrží).

```
for (int i=0; i<5; i++)
{
  // Create message, passing a string message for the body.
  BrokeredMessage message = new BrokeredMessage("Test message " + i);

  // Set additional custom app-specific property.
  message.Properties["MessageNumber"] = i;

  // Send message to the topic.
  Client.Send(message);
}
```

Služba Bus témata podpory pro maximální velikost 256 KB [Standardní osy](service-bus-premium-messaging.md) a 1 MB [Premium osy](service-bus-premium-messaging.md). Záhlaví obsahuje vlastností standardních a vlastních aplikací, může mít maximální velikosti doručovaných 64 KB. Tam bez omezení počtu zpráv v tématu je ale zakončení na celkové velikosti zprávy uskutečňuje podle tématu. Toto téma velikost je definována na údaje o času vytvoření, s horní mez 5 GB. Pokud je povoleno oddílů, horní mez je vyšší. Další informace najdete v tématu [Partitioned messaging entity](service-bus-partitioning.md).

## <a name="how-to-receive-messages-from-a-subscription"></a>Jak přijímat zprávy z předplatného

Doporučené postupy pro příjem zpráv z předplatného, je použít objekt [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) . [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) objektů, můžete pracovat v dvěma různými způsoby: [ *ReceiveAndDelete* a *PeekLock*](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx).

Použití režimu **ReceiveAndDelete** přijímání při operaci jeden snímek; To znamená když služby Bus obdrží žádost o přečtení zprávy v předplatné, ji označí zprávu jako právě spotřebované a vrátí do aplikace. Režim **ReceiveAndDelete** je nejjednodušší modelu a je nejvhodnější pro scénáře, které aplikace nevadí vám není zpracování zprávy v případě se nepovede. Pokud chcete porozumět tomu, zvažte scénáři, ve kterém příjemci problémy žádost přijmout a potom dojde k chybě před zpracování. Protože služby Bus označil zprávu jako spotřebované, když se aplikace restartování a jinými zprávy znovu, nebude zahájen, ho bude zmeškané zprávu, která byla spotřebované množství před k chybě.

V režimu **PeekLock** (což je výchozí režim) proces přijímání optimalizuje dvoufázový operaci, která umožní podpory aplikace, které nelze tolerovat chybějící zprávy. Když služby Bus obdrží žádost, zjistí další zprávu do být spotřebované množství, ji nechcete, aby ostatní uživatelé jeho přijetí zamkne a vrátí hodnotu aplikace. Po aplikace dokončení zpracování zprávy (nebo problémy se spolehlivým ukládá pro budoucí zpracování), provede druhé fáze procesu přijímání tak, že zavoláte [Dokončeno](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) na přijaté zprávy. Po služby Bus uvidí **dokončení** volání, označí zprávu jako právě spotřebované a potom ji odebere z předplatného.

Následující příklad ukazuje, jak zprávy stavu a zpracovány pomocí výchozí režim **PeekLock** . Chcete-li zadat jinou hodnotu [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) , můžete provádět jiné přetížení [CreateFromConnectionString](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.createfromconnectionstring.aspx). V tomto příkladu zpětné [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.onmessage.aspx) na zpracování zpráv doručenými **HighMessages** předplatné.

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

SubscriptionClient Client =
    SubscriptionClient.CreateFromConnectionString
            (connectionString, "TestTopic", "HighMessages");

// Configure the callback options.
OnMessageOptions options = new OnMessageOptions();
options.AutoComplete = false;
options.AutoRenewTimeout = TimeSpan.FromMinutes(1);

Client.OnMessage((message) =>
{
    try
    {
        // Process message from subscription.
        Console.WriteLine("\n**High Messages**");
        Console.WriteLine("Body: " + message.GetBody<string>());
        Console.WriteLine("MessageID: " + message.MessageId);
        Console.WriteLine("Message Number: " +
            message.Properties["MessageNumber"]);

        // Remove message from subscription.
        message.Complete();
    }
    catch (Exception)
    {
        // Indicates a problem, unlock message in subscription.
        message.Abandon();
    }
}, options);
```

V tomto příkladu nakonfiguruje zpětné [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.onmessage.aspx) pomocí objektu [OnMessageOptions](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.aspx) . [Funkce Automatické dokončování](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autocomplete.aspx) nastavena na **hodnotu false** povolte ruční ovládání ze kdy volat [Dokončeno](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) na přijaté zprávy. [AutoRenewTimeout](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autorenewtimeout.aspx) je nastavený na 1 minuta, což způsobuje klienta potřeba počkat, až jednu minutu před ukončením funkci automatického obnovení a klient chce nového volání kontrola zpráv. Tato hodnota vlastnosti snižuje počet opakování klienta volá daně, které nejsou vzkazy z.

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Postup v případě havárie aplikací a čitelný zprávy

Služba Bus poskytuje funkce vám řádně obnovení z chyb v aplikaci nebo potíže zpracování zprávy. Pokud je přijímání aplikace nelze ji zpracovat z nějakého důvodu, ho metodu [opustit](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.abandon.aspx) zavolat na přijaté zprávy (namísto metody [Dokončeno](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) ). To způsobí, že služba Bus odemknout zpráv v rámci předplatného a jeho Příprava získaná znovu stejné náročný aplikací nebo jinou náročný aplikací.

Je také vypršení časového limitu spojený se zprávou uzamčené v rámci předplatného a neúspěšné žádost zpracuje ji před lock vypršení časového limitu (například pokud způsobí zhroucení aplikace), pak služby Bus odemčena zprávy automaticky a díky kterému je dostupný získaná znovu.

V případě, že aplikace padá po zpracování zprávy, ale před zadáním požadavku [Dokončeno](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) , zprávy se být znovu dodány aplikaci po restartování. Toto je někdy označovány jako *aspoň jednou zpracování*; To znamená každou zprávu zpracovat aspoň jednou, ale v některých případech může být znovu dodány stejnou zprávu. Pokud scénáře nelze tolerovat duplicitní zpracování, pak vývojáři bychom přidat další použití logických operátorů k aplikaci, která bude řešit duplicitní zprávu doručení. To je často dosáhnout pomocí vlastnosti [MessageId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) zprávy, která konstantní přes pokusy o doručení.

## <a name="delete-topics-and-subscriptions"></a>Odstranění témata a předplatného

Následující příklad ukazuje, jak odstranit téma **TestTopic** z oboru **HowToSample** služby.

```
// Delete Topic.
namespaceManager.DeleteTopic("TestTopic");
```

Odstranění tématu odstraněny také žádné předplatné, které jsou registrované s tématem. Předplatné můžete odstraní taky nezávisle na sobě. Následující kód ukazuje, jak odstranit předplatné s názvem **HighMessages** z téma **TestTopic** .

```
namespaceManager.DeleteSubscription("TestTopic", "HighMessages");
```

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili základy služby Bus témata a předplatná, tyto odkazy vedou na další informace.

-   [Fronty, témata a předplatné][].
-   [Ukázka tématu filtry][]
-   Přehled rozhraní API pro [SqlFilter][].
-   Vytvoření funkční aplikaci, která umožňuje volání a přijímání zpráv do a z fronty Bus služby: [Služba Bus brokered zpráv kurz .NET][].
-   Služba Bus ukázky: Stáhněte si z [Azure vzorky][] nebo najdete v článku [Přehled](service-bus-samples.md).

  [Azure portálu]: https://portal.azure.com

  [7]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/getting-started-multi-tier-13.png

  [Fronty, témata a předplatné]: service-bus-queues-topics-subscriptions.md
  [Ukázka tématu filtry]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx
  [SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [Kurz .NET pro zasílání zpráv služby Bus brokered]: service-bus-brokered-tutorial-dotnet.md
  [Azure vzorky]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
