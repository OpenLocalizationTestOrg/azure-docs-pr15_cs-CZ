<properties 
    pageTitle="Psaní aplikace, které využívají služby Bus fronty | Microsoft Azure"
    description="Jak psát jednoduché fronty aplikace založené na využívající služby Bus."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/03/2016"
    ms.author="sethm" />

# <a name="create-applications-that-use-service-bus-queues"></a>Vytvoření aplikace, které využívají služby Bus fronty

Toto téma popisuje služby Bus fronty a ukazuje, jak psát jednoduché fronty aplikace založené na využívající služby Bus.

Zvažte situace ze světa maloobchodní, ve kterém je nutné data o prodeji z jednotlivých terminály Point-of-Sale (POS) směrovat k systému správy zásob, který používá data zjistit, kdy papíru má doplnit. Toto řešení používá pro komunikaci mezi terminály a systému správy zásob zasílání zpráv služby Bus jak je znázorněno na následujícím obrázku:

![Služba Bus fronty obrázek 1](./media/service-bus-create-queues/IC657161.gif)

Každý terminál POS sestavy její data o prodeji tak, že zprávy **DataCollectionQueue**. Tyto zprávy zůstanou v této fronty, dokud načtením systémem správy zásob. Tento způsob je často označují *asynchronní zasílání zpráv*, protože terminál POS není potřeba počkat, odpovědi ze systému správy zásob pokračovat ve zpracování.

## <a name="why-queuing"></a>Proč řízení fronty?

Před podíváme na kód, který je potřeba nastavit tuto aplikaci, zvažte výhody používání fronty v tomto scénáři takže není nutné terminály POS mluvit přímo (synchronní) do systému správy zásob.

### <a name="temporal-decoupling"></a>Časový oddělení

S zpráv asynchronní výrobci a které se zobrazují koncovým nemusí být online ve stejnou dobu. Zasílání zpráv infrastruktury problémy se spolehlivým ukládá zprávy, dokud náročný strana je připravená k jejich odběru. To znamená, že komponent distribuované aplikace může být odpojený, buď dobrovolně; například pro údržbu nebo z důvodu zhroucení součásti bez ovlivnění celého systému. Kromě toho náročný aplikace mohou pouze musí být online určité době dne. Například v tomto scénáři maloobchodní systému řízení zásob pouze pravděpodobně nutné přechodu do online po konec pracovního dne.

### <a name="load-leveling"></a>Načtení vyrovnání

V mnoha aplikacích zatížení systému se liší v čase, že zpracování dobu potřebnou pro každý jednotky práce je obvykle konstanty. Intermediating výrobci zprávy a uživatelé s fronty znamená, že náročný aplikace (pracovní) pouze musí zřízení služby průměr zatížení spíše než zatížení ve špičce. Název hloubkové fronty zvětšovat a smluv jako příchozí načíst se liší. Tím se uloží přímo peníze ohledně množství infrastruktury muset služby načítání aplikace.

![Služba Bus fronty obrázek 2](./media/service-bus-create-queues/IC657162.gif)

### <a name="load-balancing"></a>Vyrovnávání zatížení

Jak zvyšuje načíst další pracovních postupů můžete zní ve frontě pracovního. Každá zpráva zpracován jenom jeden pracovní procesy. Kromě toho tento na základě vyžádané Vyrovnávání zatížení, umožňuje pro optimální spotřebu počítačů pracovníka i v případě, že pracovní počítačích lišit pokud jde o zpracování power budou získávat zprávy vlastní maximální sazbou. Tento způsob je často označují konkurenční vzorek příjemce.

![Služba Bus fronty obrázek 3](./media/service-bus-create-queues/IC657163.gif)

### <a name="loose-coupling"></a>Volné spojování

Použití až středně pokročilé mezi výrobci zprávy a které se zobrazují koncovým pro zpracování zpráv poskytuje vnitřní volné spojení mezi součásti. Protože výrobci a které se zobrazují koncovým nejsou navzájem ví, je možné příjemce upgradovat bez nutnosti žádný vliv na výrobce. Kromě toho můžete vyvíjet topologii zpráv beze změny existující koncové body. Probereme tato více při budeme publikovat nebo přihlášení k odběru.

## <a name="show-me-the-code"></a>Zobrazit kód

V následující části ukazuje, jak používat službu Bus vytvářet této aplikace.

### <a name="sign-up-for-an-azure-account"></a>Zaregistrujte se účet Azure

Musíte mít účet Azure abyste mohli začít pracovat s Bus služby. Pokud už nemáte jeden, můžou zaregistrovat ke bezplatného účtu [tady](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A85619ABF).

### <a name="create-a-namespace"></a>Vytvořit obor názvů

Až budete mít předplatné, můžete [vytvořit nový obor názvů](service-bus-create-namespace-portal.md). Každý názvů se chová jako oboru kontejner pro sadu služby Bus entity. Pojmenujte svůj nový obor názvů jedinečné všech účtů služeb Bus. 

### <a name="install-the-nuget-package"></a>Instalace balíček NuGet

Použít oboru Bus služeb, musí odkazovat aplikace služby Bus sestavení konkrétně Microsoft.ServiceBus.dll. Můžete najít sestavení jako součást Microsoft Azure SDK a stahování je k dispozici v [Azure SDK stáhnout stránky](https://azure.microsoft.com/downloads/). [Služba Bus NuGet balíčku](https://www.nuget.org/packages/WindowsAzure.ServiceBus) je však nejjednodušší a chcete nakonfigurovat aplikaci se všemi závislostí služby Bus si rozhraní API Bus služby.

### <a name="create-the-queue"></a>Vytváření fronty

Správa pro službu Bus zpráv entity (témata fronty a publikovat a přihlášení k odběru) provádějí pomocí [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) předmětu. Služba Bus používá model na základě zabezpečení [Sdílené přidružení (zabezpečení aplikace Access podpis)](service-bus-sas-overview.md) . [Poskytovatel TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) třídy představuje Poskytovatel tokenu zabezpečení pomocí metody předdefinované factory vrácení některých známých tokenu poskytovatelů. Způsob [CreateSharedAccessSignatureTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.createsharedaccesssignaturetokenprovider.aspx) budeme používat k ukládání přihlašovacích údajů přidružení zabezpečení. [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) instance je pak vytvořen s základní adresu oboru Bus služby a tokenu poskytovatele.

Třídy [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) nabízí způsobů vytváření, vytvořit výčet a odstraňování zpráv entity. Kód, který je zobrazena zde ukazuje, jak je [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) instance vytvořili a použít k vytvoření fronty **DataCollectionQueue** .

```
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", 
                "test-blog", string.Empty);
string name = "RootManageSharedAccessKey";
string key = "abcdefghijklmopqrstuvwxyz";
 
TokenProvider tokenProvider = 
    TokenProvider.CreateSharedAccessSignatureTokenProvider(name, key);
NamespaceManager namespaceManager = 
    new NamespaceManager(uri, tokenProvider);
namespaceManager.CreateQueue("DataCollectionQueue");
```

Všimněte si, že jsou přetížení metody [CreateQueue](https://msdn.microsoft.com/library/azure/hh322663.aspx) podporující nástroj vlastnosti fronty vyladit. Je třeba nastavit výchozí time to live (TTL) hodnotu pro zprávy odeslané do fronty.

### <a name="send-messages-to-the-queue"></a>Odeslání zprávy do fronty

Pro spuštění operace u služby Bus entit; například odesílat a přijímat zprávy, aplikace nutné nejprve vytvořit [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) objekt. Podobně jako třídy [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) instance je tvořen základní adresu obor názvů služby a tokenu poskytovatele.

```
 BrokeredMessage bm = new BrokeredMessage(salesData);
 bm.Label = "SalesReport";
 bm.Properties["StoreName"] = "Redmond";
 bm.Properties["MachineID"] = "POS_1";
```

Zpráva pošle a od služby Bus přijata fronty jsou instancemi tříd [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) . Tato třída obsahuje sadu standardní vlastnosti (například [Popisek](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) a [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)), slovník, který se používá k ukládání vlastnosti aplikace a textu data libovolného aplikací. Aplikaci můžete nastavit subjekt předáním jakéhokoliv serializovatelný objektu (v následujícím příkladu předá v **SalesData** objekt, který představuje data o prodeji z terminál POS), který umožňuje [DataContractSerializer](https://msdn.microsoft.com/library/azure/system.runtime.serialization.datacontractserializer.aspx) Serializujte objekt. Můžete taky můžete zadat objekt [proudu](https://msdn.microsoft.com/library/azure/system.io.stream.aspx) .

Nejjednodušší způsob, jak odesílat zprávy do dané fronty v našem případě **DataCollectionQueue**je použití [CreateMessageSender](https://msdn.microsoft.com/library/azure/hh322659.aspx) vytvořit objekt [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx) přímo z [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) instance.

```
MessageSender sender = factory.CreateMessageSender("DataCollectionQueue");
sender.Send(bm);
```

### <a name="receiving-messages-from-the-queue"></a>Přijímání zpráv ve frontě

Aby se zprávy ve frontě, můžete [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) objekt, který můžete vytvářet přímo na [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) pomocí [CreateMessageReceiver](https://msdn.microsoft.com/library/azure/hh322642.aspx). Příjemci zprávy, můžete pracovat v dvěma různými způsoby: **ReceiveAndDelete** a **PeekLock**. [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) nastavenou vytvoření příjemce zprávy, jako parametr [CreateMessageReceiver](https://msdn.microsoft.com/library/azure/hh322642.aspx) volání.


Použití režimu **ReceiveAndDelete** příjem při operaci jeden snímek; To znamená když služby Bus obdrží žádost, ji označí zprávu jako právě spotřebované a vrátí do aplikace. Režim **ReceiveAndDelete** je nejjednodušší modelu a je nejvhodnější pro scénáře, které aplikace nevadí vám není zpracování zprávy kdyby selhání problémy. Pokud chcete porozumět tomu, zvažte scénáři, ve kterém příjemci problémy žádost přijmout a potom dojde k chybě před zpracování. Protože služby Bus označit zprávu jako spotřebované, po restartování aplikace a spuštění jinými zprávy znovu ho bude uniknout zprávu, která byla spotřebované množství před k chybě.

V režimu **PeekLock** bude příjem dvoufázový operaci, která vám umožní podporují aplikace, které nelze tolerovat chybějící zprávy. Když služby Bus obdrží žádost, zjistí další zprávu do být spotřebované množství, ji nechcete, aby ostatní uživatelé jeho přijetí zamkne a vrátí hodnotu aplikace. Po aplikace dokončení zpracování zprávy (nebo problémy se spolehlivým ukládá pro budoucí zpracování), provede druhé fáze procesu přijímání tak, že zavoláte [Dokončeno](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) na přijaté zprávy. Služba Bus uvidí [dokončení](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) volání, označí zprávu jako právě spotřebované množství.

Jsou dva další výsledky. Nejdřív nemůže-li žádost zpracuje zprávu z nějakého důvodu, ho můžete volat [opustit](https://msdn.microsoft.com/library/azure/hh181837.aspx) na přijaté zprávy (místo [Dokončeno](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx)). To způsobí, že služba Bus odemkněte zprávu a jeho Příprava získaná znovu stejným klientem nebo jiné dokončení klientem. Za druhé existuje vypršení časového limitu přidružené zámek a pokud aplikace se nedají zpracovat zprávu před zámek vypršení časového limitu (například pokud způsobí zhroucení aplikace) a pak služby Bus bude odemknout zprávu a jeho Příprava získaná znovu (v podstatě provedení operace [opustit](https://msdn.microsoft.com/library/azure/hh181837.aspx) ve výchozím nastavení).

Všimněte si, že pokud aplikace padá po zpracování zprávy, ale před [Dokončeno](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) byl vydán žádost, zprávy se být znovu dodány aplikaci po restartování. To je často označují *Aspoň jednou* zpracování. To znamená, že každá zpráva budou zpracovány aspoň jednou, ale v některých případech může být znovu dodány stejná zpráva. Pokud scénáře nelze tolerovat duplicitní zpracování, je potřeba další logiku v aplikaci duplicity. Toho můžete dosáhnout na základě vlastnosti [MessageId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) zprávy. Přes neúspěšných pokusech o doručení konstantní hodnota této vlastnosti. Tím se nazývá *Jednou* zpracování.

Kód, který je zobrazena zde přijímá a zpracovává zprávy pomocí **PeekLock** režimu, což je výchozí explicitně Pokud žádná hodnota [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) .

```
MessageReceiver receiver = factory.CreateMessageReceiver("DataCollectionQueue");
BrokeredMessage receivedMessage = receiver.Receive();
try
{
    ProcessMessage(receivedMessage);
    receivedMessage.Complete();
}
catch (Exception e)
{
    receivedMessage.Abandon();
}
```

### <a name="use-the-queue-client"></a>Použití klienta fronty

Předchozích příkladech v tomto oddílu vytvořit [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx) a [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) objekty přímo z [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) k odesílání a přijímání zpráv ve frontě, respektive. Alternativní přístup, je použít [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) předmětu, který podporuje odesílání a příjem operace kromě rozšířených funkce, například relace.

```
QueueClient queueClient = factory.CreateQueueClient("DataCollectionQueue");
queueClient.Send(bm);
            
BrokeredMessage message = queueClient.Receive();

try
{
    ProcessMessage(message);
    message.Complete();
}
catch (Exception e)
{
    message.Abandon();
} 
```

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili základy fronty, najdete v článku [vytvoření aplikací používajících služby Bus témata a předplatná](service-bus-create-topics-subscriptions.md) pokračujte této diskuse pomocí funkce publikování a přihlášení k odběru služby Bus témat a předplatná.