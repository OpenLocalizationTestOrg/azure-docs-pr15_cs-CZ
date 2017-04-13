<properties
    pageTitle="Odesílání nabízená oznámení s Azure oznámení rozbočovače a Node.js"
    description="Naučte se používat rozbočovače oznámení o odeslání nabízená oznámení z aplikace Node.js."
    keywords="nabízená oznámení, nabízených notifications,node.js nabízených nabízených ios"
    services="notification-hubs"
    documentationCenter="nodejs"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-with-azure-notification-hubs-and-nodejs"></a>Odesílání nabízená oznámení s Azure oznámení rozbočovače a Node.js
[AZURE.INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

##<a name="overview"></a>Základní informace

> [AZURE.IMPORTANT] Tento kurz, musíte mít účet Azure active. Pokud nemáte účet, můžete vytvořit bezplatný účet zkušební v jenom pár minut. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-nodejs-how-to-use-notification-hubs).

Tento průvodce vám ukáže, jak chcete odeslat nabízená oznámení pomocí Azure oznámení rozbočovače přímo z aplikace Node.js. 

Scénáře nichž se uplatní zasílání nabízených oznámení pro aplikace na následujících platformách:

* Android
* iOS
* Windows Phone
* Univerzální Windows platformy 

Další informace o oznámení rozbočovače naleznete v části [Další kroky](#next) .

##<a name="what-are-notification-hubs"></a>Co jsou oznámení rozbočovače?

Azure rozbočovače oznámení poskytovat infrastrukturu – použití více platformy, scalable odesílání nabízená oznámení na mobilní zařízení. Podrobnosti o infrastruktury služby najdete v tématu stránce [Rozbočovače oznámení Azure](http://msdn.microsoft.com/library/windowsazure/jj927170.aspx) .

##<a name="create-a-nodejs-application"></a>Vytvoření aplikace Node.js

Cílem prvního kroku v tomto kurzu je vytvoření nové prázdné Node.js aplikace. Pokyny týkající se vytváření aplikace Node.js najdete v tématu [Vytvoření a nasazení aplikace Node.js k webu Azure][nodejswebsite], [Node.js cloudové služby] [ Node.js Cloud Service] pomocí prostředí Windows PowerShell nebo [Web s WebMatrix].

##<a name="configure-your-application-to-use-notification-hubs"></a>Konfigurace aplikace pomocí rozbočovače oznámení

Použít rozbočovače oznámení Azure, musíte stáhnout a používat Node.js [azure balíčku](https://www.npmjs.com/package/azure), který obsahuje několik předdefinovaných pomocné knihovny, které komunikovat s ZBÝVAJÍCÍ služby nabízená oznámení.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Použití uzel balíčku správce (NPM) získání balíčku

1.  Pomocí příkazového řádku rozhraní, jako je **prostředí PowerShell** (Windows), **terminálu** (Mac) nebo **flám** (Linux) a přejděte do složky, které jste vytvořili aplikace prázdné.

2.  Do příkazového řádku zadejte **npm instalace azure sb** .

3.  Můžete ručně spusťte **ls** nebo **dir** příkaz ověřte, že **uzel\_moduly** složky byl vytvořen. V této složce najděte balíček **azure** , která obsahuje knihovnu potřebujete získat přístup k centru oznámení.

>[AZURE.NOTE] Je další informace o instalaci NPM oficiální [NPM blogu](http://blog.npmjs.org/post/85484771375/how-to-install-npm). 

### <a name="import-the-module"></a>Import modulu

V textovém editoru, přidejte následující text do horní části souboru **server.js** aplikace:

    var azure = require('azure');

### <a name="setup-an-azure-notification-hub-connection"></a>Nastavení připojení k centrální oznámení Azure

Objekt **NotificationHubService** umožňuje pracovat s rozbočovače oznámení. Následující kód vytvoří objekt **NotificationHubService** nofication rozbočovače s názvem **hubname**. Přidejte ho do horní části souboru **server.js** po provedení příkazu importovat modul azure:

    var notificationHubService = azure.createNotificationHubService('hubname','connectionstring');

Hodnotu **connectionstring** připojení můžete získat z [Portálu Azure] provedením následujících kroků:

1. V levém navigačním podokně klikněte na **Procházet**.

2. Vyberte **Rozbočovače oznámení**a najděte centru, které chcete použít pro vzorku. Můžete vytvořit odkaz na [Windows Store Začínáme kurz](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) Pokud potřebujete pomoc s vytvořením nového centrální oznámení.

3. Vyberte **Nastavení**.

4. Klikněte na **zásady přístupu**. Zobrazí se obě sdílené a úplný přístup připojovací řetězec.

![Azure portál - rozbočovače oznámení](./media/notification-hubs-nodejs-how-to-use-notification-hubs/notification-hubs-portal.png)

> [AZURE.NOTE] Můžete také zadat připojovací řetězec pomocí rutiny **Get-AzureSbNamespace** poskytnuté [Azure PowerShell](../powershell-install-configure.md) nebo na příkaz **Zobrazit azure sb názvů** [rozhraní Azure příkazového řádku (Azure rozhraní příkazového řádku)](../xplat-cli-install.md).

##<a name="general-architecture"></a>Obecné architektura

Objekt **NotificationHubService** poskytuje následující instance objektů k odesílání nabízených oznámení pro určitá zařízení a aplikace:

* **Android** – použití **GcmService** objekt, který je dostupný na **notificationHubService.gcm**
* **iOS** : pomocí objektu **ApnsService** , která je dostupná na **notificationHubService.apns**
* **Windows Phone** – použití **MpnsService** objekt, který je dostupný na **notificationHubService.mpns**
* **Univerzální platformu Windows už** - použití **WnsService** objekt, který je dostupný na **notificationHubService.wns**

### <a name="how-to-send-push-notifications-to-android-applications"></a>Postup: odesílání nabízená oznámení na Android aplikace

Objekt **GcmService** představuje způsob **odeslání** použitý k odeslání nabízená oznámení na Android aplikace. Způsob **odeslání** přijme následujících parametrů:

* **Značky** - identifikátor značku. Pokud chybí značka klientům al odešle oznámení.
* **Datové** - JSON nebo datové nezpracovanými řetězec zprávy.
* **Zpětné** – funkce zpětného volání.

Další informace o formátu data naleznete v části **datové** [Implementace serverového GCM](http://developer.android.com/google/gcm/server.html#payload) dokumentu.

Následující kód používá instanci **GcmService** zveřejněné příslušným **NotificationHubService** odešlete všem registrovaným klientům nabízená oznámení.

    var payload = {
      data: {
        message: 'Hello!'
      }
    };
    notificationHubService.gcm.send(null, payload, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-ios-applications"></a>Postup: odesílání nabízených oznámení pro aplikace iOS

Stejný jako s Androidem aplikacemi jsme je popsali výše, objekt **ApnsService** představuje způsob **odeslání** použitý k odeslání nabízená oznámení pro aplikace iOS. Způsob **odeslání** přijme následujících parametrů:

* **Značky** - identifikátor značku. Pokud chybí značka odešle oznámení pro všechny klienty.
* **Datové** - JSON zprávy nebo řetězec data.
* **Zpětné** – funkce zpětného volání.

Další informace na formát datové naleznete v části **Oznámení datové** [místní a nabízená oznámení Programming Guide](http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html) dokumentu.

Následující kód používá instanci **ApnsService** zveřejněné příslušným **NotificationHubService** odešlete všechny klienty výstrahy zabezpečení:

    var payload={
        alert: 'Hello!'
      };
    notificationHubService.apns.send(null, payload, function(error){
      if(!error){
        // notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-windows-phone-applications"></a>Postup: odesílání nabízených oznámení pro aplikace Windows Phone

Objekt **MpnsService** představuje způsob **odeslání** použitý k odeslání nabízených oznámení pro Windows Phone aplikace. Způsob **odeslání** přijme následujících parametrů:

* **Značky** - identifikátor značku. Pokud chybí značka odešle oznámení pro všechny klienty.
* **Datové** - datová část XML zprávy.
* **Cílový_název**  -  `toast` informačního oznámení. `token`oznámení dlaždice.
* **NotificationClass** - priority oznámení. V části **Prvky záhlaví HTTP** dokumentu [nabízená oznámení ze serveru](http://msdn.microsoft.com/library/hh221551.aspx) pro platné hodnoty.
* **Možnosti** – volitelné žádost o záhlaví.
* **Zpětné** – funkce zpětného volání.

Seznam možností platné **Cílový_název**, **NotificationClass** a záhlaví podívejte se na stránce [nabízená oznámení ze serveru](http://msdn.microsoft.com/library/hh221551.aspx) .

Následující ukázkový kód používá instance **MpnsService** zveřejněné příslušným **NotificationHubService** k odeslání oznámení nabízená oznámení:

    var payload = '<?xml version="1.0" encoding="utf-8"?><wp:Notification xmlns:wp="WPNotification"><wp:Toast><wp:Text1>string</wp:Text1><wp:Text2>string</wp:Text2></wp:Toast></wp:Notification>';
    notificationHubService.mpns.send(null, payload, 'toast', 22, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-universal-windows-platform-uwp-applications"></a>Postup: odesílání nabízená oznámení aplikací univerzální platformu systému Windows (UWP)

Objekt **WnsService** představuje způsob **odeslání** použitý k odeslání nabízená oznámení aplikací univerzální platformu Windows už.  Způsob **odeslání** přijme následujících parametrů:

* **Značky** - identifikátor značku. Pokud chybí značka oznámení odesláno všem registrovaných klientům.
* **Datové** - datová část XML zprávy.
* **Typ** – typ oznámení.
* **Možnosti** – volitelné žádost o záhlaví.
* **Zpětné** – funkce zpětného volání.

Seznam platné typy a žádost o záhlaví najdete v článku [nabízená oznámení služby žádostí a odpovědí záhlaví](http://msdn.microsoft.com/library/windows/apps/hh465435.aspx).

Následující kód používá instanci **WnsService** zveřejněné příslušným **NotificationHubService** odešlete UWP aplikace informačního nabízená oznámení:

    var payload = '<toast><visual><binding template="ToastText01"><text id="1">Hello!</text></binding></visual></toast>';
    notificationHubService.wns.send(null, payload , 'wns/toast', function(error){
      if(!error){
        // notification sent
      }
    });

## <a name="next-steps"></a>Další kroky

Výše uvedené ukázkové fragmenty umožňují snadno vytvářet infrastruktury služby předvádění nabízená oznámení širokou škálu zařízení. Teď, když jste se naučili základy používání rozbočovače oznámení s node.js, tyto odkazy vedou na další informace o tom, jak můžete rozšířit tyto další možnosti.

-   [Azure oznámení rozbočovače](https://msdn.microsoft.com/library/azure/jj927170.aspx)naleznete v tématu MSDN odkazu.
-   Navštivte [Azure SDK uzel] úložiště na GitHub Další příklady a informace o implementaci.

  [Azure SDK uzel]: https://github.com/WindowsAzure/azure-sdk-for-node
  [Next Steps]: #nextsteps
  [What are Service Bus Topics and Subscriptions?]: #what-are-service-bus-topics
  [Create a Service Namespace]: #create-a-service-namespace
  [Obtain the Default Management Credentials for the Namespace]: #obtain-default-credentials
  [Create a Node.js Application]: #Create_a_Nodejs_Application
  [Configure Your Application to Use Service Bus]: #Configure_Your_Application_to_Use_Service_Bus
  [How to: Create a Topic]: #How_to_Create_a_Topic
  [How to: Create Subscriptions]: #How_to_Create_Subscriptions
  [How to: Send Messages to a Topic]: #How_to_Send_Messages_to_a_Topic
  [How to: Receive Messages from a Subscription]: #How_to_Receive_Messages_from_a_Subscription
  [How to: Handle Application Crashes and Unreadable Messages]: #How_to_Handle_Application_Crashes_and_Unreadable_Messages
  [How to: Delete Topics and Subscriptions]: #How_to_Delete_Topics_and_Subscriptions
  [1]: #Next_Steps
  [Topic Concepts]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-topics-01.png
  [Azure Classic Portal]: http://manage.windowsazure.com
  [image]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-03.png
  [2]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-04.png
  [3]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-05.png
  [4]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-06.png
  [5]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-07.png
  [SqlFilter.SqlExpression]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [Azure Service Bus Notification Hubs]: http://msdn.microsoft.com/library/windowsazure/jj927170.aspx
  [SqlFilter]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.aspx
  [Web s WebMatrix]: /develop/nodejs/tutorials/web-site-with-webmatrix/
  [Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Previous Management Portal]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/previous-portal.png
  [nodejswebsite]: /develop/nodejs/tutorials/create-a-website-(mac)/
  [Node.js Cloud Service with Storage]: /develop/nodejs/tutorials/web-app-with-storage/
  [Node.js Web Application with Storage]: /develop/nodejs/tutorials/web-site-with-storage/
  [Azure portálu]: https://portal.azure.com
