<properties
    pageTitle="Směrování a značky výrazů"
    description="Toto téma vysvětluje výrazy směrování a značky pro rozbočovače Azure oznámení."
    services="notification-hubs"
    documentationCenter=".net"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="routing-and-tag-expressions"></a>Směrování a značky výrazech

##<a name="overview"></a>Základní informace

Značka výrazů vám umožní cílové specifické sady zařízení nebo konkrétněji registrace při odesílání nabízená oznámení prostřednictvím rozbočovače oznámení.


## <a name="targeting-specific-registrations"></a>Zaměření na konkrétní registrace

Je možné jenom cílové konkrétní oznámení registrace je chcete přidružit k značky, pak zacílit tyto značky. Jak je popsáno v [Části Správa registrace](notification-hubs-push-notification-registration-management.md)pro příjem nabízených oznámení aplikace musí zaregistrovat zařízení úchytu na rozbočovači oznámení. Po vytvoření registrace na rozbočovači oznámení back-end aplikací můžete poslat nabízená oznámení ho.
Back-end aplikace můžete vybrat záznamy, které chcete cíl konkrétní oznámení následujícím způsobem:

1. **Vysílání**: všechny registrace v centru oznámení upozornění.
2. **Značka**: všechny registrace, které obsahují značku zadaný upozornění.
3. **Výraz označení**: všechny registrace, jehož nastavení značky odpovídají zadaný výraz upozornění.

## <a name="tags"></a>Značky

Značky může být jakýkoli řetězec až o 120 znaků obsahující alfanumerický a následující jiné než alfanumerické znaky: "_" ‘@’, "#", ".",":", "-". Následující příklad ukazuje aplikace ze které se zobrazí zprávu oznámení o konkrétní hudbu skupiny. V tomto scénáři je jednoduchý způsob, jak směrování oznámení popisek registrace s značky, které představují různé pásma, jako na následujícím obrázku.

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags.png)

Na tomto obrázku zprávu příznakem **Beatles** dosáhne pouze tabletu, ve kterém registrovaný u značku **Beatles**.

Další informace o vytváření registrace značek naleznete v tématu [Správa registrace](notification-hubs-push-notification-registration-management.md).

Oznámení můžete poslat značky pomocí metody odeslat oznámení `Microsoft.Azure.NotificationHubs.NotificationHubClient` třídy v [Microsoft Azure oznámení rozbočovače](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) SDK. Můžete taky použít Node.js nebo rozhraní nabízená oznámení REST API.  Tady je příklad pomocí SDK.


    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You requested a Beatles notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Beatles");

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You requested a Wailers notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Wailers");




Značky nemusí být předem zřizování a můžete odkázat na více aplikací specifické koncepty. Uživatelé aplikace tento příklad můžete například komentovat pruhy a nechcete dostat žádnou toasts, nejen pro poznámky na své oblíbené pruhy, ale taky pro všechny komentáře od jejich přátel, bez ohledu na pruh, ve kterém jsou komentáře. Následující obrázek ukazuje příklad situaci:



![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags2.png)

Na tomto obrázku Alice je zájem o zavedli Beatles a Jan je zájem o zavedli Wailers. Jan je taky zajímat Karel na komentáře, a Karel v Wailers máte zájem o. Po odeslání oznámení pro jeho Karel komentování Beatles Alice a Jan ji přijmout.

Můžete kódovat více pochybnosti v značky (například "band_Beatles" nebo "follows_Charlie"), jsou značky jednoduché řetězce a nikoli vlastnosti s hodnotami. Registrace spárována jenom na informace o stavu nebo nepřítomnosti určité značky.

Úplné podrobný kurz o tom, jak používat značky pro odesílání úrok skupinám najdete v článku [Novinkách](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md).


## <a name="using-tags-to-target-users"></a>Pomocí značek cílových uživatelů

Další způsob, jak používat značky je k identifikaci všech zařízeních určitému uživateli. Registrace můžete příznakem se značkou, která obsahuje id uživatele, jako na následujícím obrázku:


![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags3.png)

Na tomto obrázku uid:Alice zprávu příznakem dosáhne všechny označené uid:Alice registrace; proto všechna Alice zařízení.


##<a name="tag-expressions"></a>Značka výrazů

Jsou případy, kdy má oznámení cílovou sadu registrace popsanou není jednu značku, ale logický výraz značek.

Zvažte sportovní aplikaci, která odešle připomenutí pro všechny uživatele v Boston her mezi červená Sox a Cardinals. Registruje-li v klientské aplikaci značek o zájem týmy a umístění, by měl oznámení směrovat všichni účastníci Boston, který je zájem o Sox červená nebo Cardinals. Tato podmínka může být vyjádřena pomocí následující logický výraz:

    (follows_RedSox || follows_Cardinals) && location_Boston


![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags4.png)

Výrazy značku mohou obsahovat všechny logické operátory, jako například AND (& &), nebo (|) a ne (!). Mohou také obsahovat závorek. Značka výrazy jsou omezené na 20 značky, pokud obsahují pouze ORs; v opačném případě jsou omezeny na 6 značky.

Tady je příklad pro zasílání oznámení pomocí výrazů značky pomocí SDK.


    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    String userTag = "(location_Boston && !follows_Cardinals)"; 

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You want info on the Red Socks</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You want info on the Red Socks</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);
