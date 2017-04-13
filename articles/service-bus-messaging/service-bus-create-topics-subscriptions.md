<properties 
    pageTitle="Vytvoření aplikace, které využívají služby Bus témata a předplatná | Microsoft Azure"
    description="Úvod do publikování-přihlášení k odběru funkce nabízené služby Bus témata a předplatná."
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
    ms.date="10/04/2016"
    ms.author="sethm" />

# <a name="create-applications-that-use-service-bus-topics-and-subscriptions"></a>Vytvoření aplikace, které využívají služby Bus témata a předplatného

Azure Bus služby podporuje sadu technologií middleware cloudové, orientovaného zprávy včetně řízení fronty spolehlivé zprávy a trvalé vydávání/odebírání zasílání zpráv. Tento článek je založena na údaje v [vytvořit aplikacemi, které používají fronty Bus služby](service-bus-create-queues.md) a nabízí úvod do publikování a přihlášení k odběru funkce nabízené služby Bus témata.

## <a name="evolving-retail-scenario"></a>Dlouhodobým scénář maloobchod

Tento článek pokračuje maloobchodní scénář použitý v [aplikacích vytvořit používajících fronty Bus služby](service-bus-create-queues.md). Odvolání tato prodejní data z jednotlivých čárky prodej (POS) terminály musí být směrovány systému správy zásob, kterou používá tato data k určení po doplnit burzovního. Každý terminál POS sestav její data o prodeji odesláním zprávy frontě **DataCollectionQueue** kde zůstávají, dokud jsou doručeny v systému správy zásob, jak je znázorněno zde:

![Služba Bus 1](./media/service-bus-create-topics-subscriptions/IC657161.gif)

Vyvíjet Tento scénář je přidán nový požadavek systému: vlastníka úložiště chce mít možnost monitorovat, jak funguje obchodu v reálném čase.

Při řešení tohoto požadavku, systému třeba "klepněte na" data o prodeji proudu. Pořád chceme každé zprávy odeslané terminály POS nechat zasílat systému řízení zásob jako před, ale chceme jiné kopie každé zprávy, který používáme k prezentaci zobrazení řídicího panelu vlastníkem úložiště přihlašovacích údajů.

V každé situaci, jako je tady, ve které požadujete každou zprávu spotřebované víc stranami, můžete použít službu Bus *témata*. Témata poskytují publikovat nebo přihlášení k odběru vzorek ve kterém je k dispozici jeden nebo víc předplatných registrovaný u tématu každé publikované zprávy. Naopak s fronty každé zprávy obdrží jeden příjemce.

Jsou odeslány zprávy na téma stejným způsobem, jako jsou odeslány do fronty. Však zprávy neobdrží z tématu přímo. jsou doručeny z předplatného. Si můžete představit předplatného na téma jako virtuální fronty načítající kopie zpráv, které jsou odeslány Toto téma. Příjem zpráv od předplatné stejným způsobem jako jsou doručeny z fronty.

Přejdete zpět na maloobchodní scénáře, fronty nahrazuje téma a předplatné přibude, které můžete použít systémovou součást správy zásob. Jestli chcete systém se teď zobrazuje následujícím způsobem:

![Služba Bus 2](./media/service-bus-create-topics-subscriptions/IC657165.gif)

Konfigurace tady provede stejně jako v předchozí na základě fronty návrhu. To znamená zprávy poslané na téma směrovány do **zásob** předplatné, ze kterého je spotřebovává **Systému řízení zásob** .

Kvůli podpoře řídicí panel pro správu, vytvoříme druhý předplatné v tématu jak je znázorněno zde:

![Služba Bus 3](./media/service-bus-create-topics-subscriptions/IC657166.gif)

V této konfiguraci je k dispozici **řídicích panelů** a **zásob** předplatná každé zprávy z terminály POS.

## <a name="show-me-the-code"></a>Zobrazit kód

V článku [vytvoření aplikací používajících služby Bus fronty](service-bus-create-queues.md) popisuje, jak zaregistrovat účet Azure a vytvořte obor názvů služby. Použít obor Bus služeb, musí odkazovat aplikace služby Bus sestavení konkrétně Microsoft.ServiceBus.dll. Nejjednodušší způsob, jak odkazovat služby Bus závislosti je nainstalovat Service Bus [Nuget balíčku](https://www.nuget.org/packages/WindowsAzure.ServiceBus/). Můžete zjistit taky sestavení jako součást Azure SDK. Stahování je k dispozici v [Azure SDK stáhnout stránky](https://azure.microsoft.com/downloads/).

### <a name="create-the-topic-and-subscriptions"></a>Vytvoření téma a předplatného

Správa pro službu Bus zpráv entity (témata fronty a publikovat a přihlášení k odběru) provádějí pomocí [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) předmětu. Příslušná oprávnění jsou potřebná k vytvoření instanci [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) pro určitý obor. Služba Bus používá model na základě zabezpečení [Sdílené přidružení (zabezpečení aplikace Access podpis)](service-bus-sas-overview.md) . [Poskytovatel TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) třídy představuje Poskytovatel tokenu zabezpečení pomocí metody předdefinované factory vrácení některých známých tokenu poskytovatelů. Způsob [CreateSharedAccessSignatureTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.createsharedaccesssignaturetokenprovider.aspx) budeme používat k ukládání přihlašovacích údajů přidružení zabezpečení. [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) instance je pak vytvořen s základní adresu oboru Bus služby a tokenu poskytovatele.

Třídy [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) nabízí způsobů vytváření, vytvořit výčet a odstraňování zpráv entity. Kód, který je zobrazena zde ukazuje, jak je [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) instance vytvořili a použít k vytvoření téma **DataCollectionTopic** .

```
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", "test-blog", string.Empty);
string name = "RootManageSharedAccessKey";
string key = "abcdefghijklmopqrstuvwxyz";
     
TokenProvider tokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(name, key);
NamespaceManager namespaceManager = new NamespaceManager(uri, tokenProvider);
 
namespaceManager.CreateTopic("DataCollectionTopic");
```

Všimněte si, že jsou přetížení [CreateTopic](https://msdn.microsoft.com/library/azure/hh293080.aspx) metody, které vám umožní nastavení vlastností v tématu. Je třeba nastavit výchozí time to live (TTL) hodnotu pro zprávy poslané na téma. Dále přidejte **zásob** a **řídicích panelů** předplatná.

```
namespaceManager.CreateSubscription("DataCollectionTopic", "Inventory");
namespaceManager.CreateSubscription("DataCollectionTopic", "Dashboard");
```

### <a name="send-messages-to-the-topic"></a>Odeslání zprávy s tématem

Pro spuštění operace u služby Bus entit; například odesílat a přijímat zprávy, aplikace nutné nejprve vytvořit [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) objekt. Podobně jako třídy [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) instance je tvořen základní adresu obor názvů služby a tokenu poskytovatele.

```
MessagingFactory factory = MessagingFactory.Create(uri, tokenProvider);
```

Zpráva pošle a dostali ze služby Bus témata, jsou instancemi tříd [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) . Tato třída obsahuje sadu standardní vlastnosti (například [Popisek](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) a [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)), slovník, který se používá k ukládání vlastnosti aplikace a textu data libovolného aplikací. Aplikaci můžete nastavit subjekt předáním jakéhokoliv serializovatelný objektu (v následujícím příkladu předá v **SalesData** objekt, který představuje data o prodeji z terminál POS), který umožňuje [DataContractSerializer](https://msdn.microsoft.com/library/azure/system.runtime.serialization.datacontractserializer.aspx) Serializujte objekt. Můžete taky můžete zadat objekt [proudu](https://msdn.microsoft.com/library/azure/system.io.stream.aspx) .

```
BrokeredMessage bm = new BrokeredMessage(salesData);
bm.Label = "SalesReport";
bm.Properties["StoreName"] = "Redmond";
bm.Properties["MachineID"] = "POS_1";
```

Nejjednodušší způsob odeslání zprávy s tématem je použití [CreateMessageSender](https://msdn.microsoft.com/library/azure/hh322659.aspx) vytvořit objekt [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx) přímo z [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) instance.

```
MessageSender sender = factory.CreateMessageSender("DataCollectionTopic");
sender.Send(bm);
```

### <a name="receive-messages-from-a-subscription"></a>Přijímání zpráv z předplatného

Podobné fronty, aby se zprávy z předplatného můžete [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) objekt, který můžete vytvářet přímo na [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) pomocí [CreateMessageReceiver](https://msdn.microsoft.com/library/azure/hh322642.aspx). Můžete použít jednu z těchto dvou různých přijímání režimy (**ReceiveAndDelete** a **PeekLock**), jak je uvedeno v [aplikacích vytvořit používajících fronty Bus služby](service-bus-create-queues.md).

Všimněte si, že při vytváření [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) předplatná parametr *entityPath* formuláře `topicPath/subscriptions/subscriptionName`. Proto [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) pro předplatné **zásob** témat **DataCollectionTopic** vytvoříte *entityPath* musíte nastavit `DataCollectionTopic/subscriptions/Inventory`. Kód vypadat takto:

```
MessageReceiver receiver = factory.CreateMessageReceiver("DataCollectionTopic/subscriptions/Inventory");
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

## <a name="subscription-filters"></a>Filtry předplatného

Zatím v tomto scénáři všechny zprávy na téma jsou k dispozici všechna předplatná registrovaných. Klíčové výraz tady je "k dispozici." Během služby Bus předplatná zobrazit všechny zprávy na téma, můžete zkopírovat jen podmnožinu tyto zprávy do fronty virtuální předplatného. Tím se provádí pomocí předplatného *filtry*. Když vytvoříte předplatné, můžete zadat výraz filtru v podobě predikát SQL92 styl, který provozuje přes vlastnosti zpráv, dialogové okno Vlastnosti systému (například [Popisek](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx)) a vlastnosti aplikací, jako je **položku StoreName** v předchozím příkladu.

Vyvíjejí scénáře pro znázornění, které budou přidány do náš maloobchodní scénář je druhý úložiště. Data o prodeji ze všech terminály POS z obou ukládá přesto na směrovány systému správy centralizované zásob, ale úložiště přihlašovacích údajů správce pomocí nástroje pro řídicí panel pouze zájem o plnění tohoto úložiště. Můžete předplatné filtrování docílit. Všimněte si, že pokud terminály POS publikovat zprávy, budou vlastnost **položku StoreName** aplikace ve zprávě. Zadaných dvě úložiště, třeba **Redmond** a **Olomouc**terminály POS Redmond obsahují razítka, který jejich data o prodeji zprávy s **položku StoreName** rovno **Redmond**, že terminály POS Olomouc úložiště přihlašovacích údajů používat **položku StoreName** rovno **Olomouc**. Úložiště přihlašovacích údajů správce úložiště Redmond pouze k zobrazení dat z jeho terminály POS chce. Jestli chcete systém vypadat takto:

![Služba Bus4](./media/service-bus-create-topics-subscriptions/IC657167.gif)

Chcete-li nastavení této směrování, vytvořte předplatné **řídicího panelu** takto:

```
SqlFilter dashboardFilter = new SqlFilter("StoreName = 'Redmond'");
namespaceManager.CreateSubscription("DataCollectionTopic", "Dashboard", dashboardFilter);
```

K tomuto předplatnému filtru budou zkopírovány pouze zprávy, které obsahují **položku StoreName** sadu vlastností přiřadit **Redmond** virtuální fronty pro předplatné **řídicího panelu** . Je mnohem víc předplatné filtrování, ale. Aplikace může obsahovat více pravidel filtr na jedno předplatné kromě možnosti podle předává virtuální fronty přihlášení k odběru změnit vlastnosti zprávy.

## <a name="summary"></a>Souhrn

Všechny důvody pro použití řízení fronty podle [vytváření aplikací používajících služby Bus fronty](service-bus-create-queues.md) také použít na témata, konkrétně:

- Časový oddělení – zprávy výrobci a které se zobrazují koncovým nemusí být online ve stejnou dobu.

- Načtení vyrovnání – píků zatížení vyhlazují tak, že v tématu Povolení náročný aplikací pro zřízení průměr zatížení místo ve špičce načíst.

- Vyrovnávání zatížení – podobně jako fronty, můžete mít víc konkurenční spotřebitele listening jednoho předplatného s každá zpráva předána pouze jeden spotřebitele, čímž Vyrovnávání zatížení.

- Volné spojování – vyvíjet zpráv sítě beze změny existující koncové body; například předplatná přidáním nebo změnou filtrů k tématu povolit pro nové uživatele.

## <a name="next-steps"></a>Další kroky

Informace o používání dotazů v případě maloobchodní POS najdete v článku [vytváření aplikací používajících fronty Bus služby](service-bus-create-queues.md) .