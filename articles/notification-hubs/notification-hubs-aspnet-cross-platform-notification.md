<properties
    pageTitle="Odeslat oznámení a platformy uživatelům s rozbočovače oznámení (ASP.NET)"
    description="Informace o použití šablon rozbočovače oznámení odeslat, v jednom požadavku, bez ohledu na platformu oznámení, že zaměřuje všech platformách."
    services="notification-hubs"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/03/2016" 
    ms.author="yuaxu"/>

# <a name="send-cross-platform-notifications-to-users-with-notification-hubs"></a>Uživatelé, kteří mají rozbočovače oznámení o neodeslání oznámení a platformy


V předchozím kurzu [upozornění uživatelé, kteří mají oznámení rozbočovače]se naučíte nabízená oznámení na všech zařízeních registrovaných konkrétního ověřeného uživatele. V tomto kurzu byly více požadavků potřebné k odeslání oznámení pro každou podporované klientské platformu. Oznámení o rozbočovače podporuje šablony, které umožňují určit, jak chtít dostávat oznámení o konkrétní zařízení. Tato funkce zjednodušuje odesílání oznámení a platformy. Toto téma ukazuje, jak umožní využít výhod šablon, které chcete odeslat, v jednom požadavku, bez ohledu na platformu oznámení, že zaměřuje všech platformách. Podrobnější informace o šablonách naleznete v tématu [Přehled rozbočovače oznámení Azure][Templates].

> [AZURE.NOTE] Oznámení o rozbočovače umožňuje zařízení k registraci více šablon stejnou značku. V tomto případě příchozí zprávy zaměření na tuto značku za následek více oznámení o doručených v zařízení, jedno pro každé šablony. Umožňuje zobrazit stejná zpráva v několika vizuálních upozornění, například jako Odznáček i jako oznámením aplikace pro Windows Store.

Proveďte následující postup o neodeslání oznámení a platformy pomocí šablon:

1. V Průzkumníku řešení ve Visual Studiu rozbalte složku **řadiče** a potom otevřete soubor RegisterController.cs.

2. Vyhledejte bloku kódu způsobu **příspěvku** , který vytvoří nový nahrazení registraci `switch` obsahu s kódem takto:

        switch (deviceUpdate.Platform)
        {
            case "mpns":
                var toastTemplate = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                    "<wp:Notification xmlns:wp=\"WPNotification\">" +
                       "<wp:Toast>" +
                            "<wp:Text1>$(message)</wp:Text1>" +
                       "</wp:Toast> " +
                    "</wp:Notification>";
                registration = new MpnsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "wns":
                toastTemplate = @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(message)</text></binding></visual></toast>";
                registration = new WindowsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "apns":
                var alertTemplate = "{\"aps\":{\"alert\":\"$(message)\"}}";
                registration = new AppleTemplateRegistrationDescription(deviceUpdate.Handle, alertTemplate);
                break;
            case "gcm":
                var messageTemplate = "{\"data\":{\"message\":\"$(message)\"}}";
                registration = new GcmTemplateRegistrationDescription(deviceUpdate.Handle, messageTemplate);
                break;
            default:
                throw new HttpResponseException(HttpStatusCode.BadRequest);
        }

    Tento kód volá metodu specifické pro platformu pro registraci šablony místo nativní registrace. Existující registrace nemusí upravit, protože šablony registrace jsou odvozeny od nativní registrace.

3. V **oznámení o** řadiče nahraďte metodu **sendNotification** tento kód:

        public async Task<HttpResponseMessage> Post()
        {
            var user = HttpContext.Current.User.Identity.Name;
            var userTag = "username:" + user;

            var notification = new Dictionary<string, string> { { "message", "Hello, " + user } };
            await Notifications.Instance.Hub.SendTemplateNotificationAsync(notification, userTag);

            return Request.CreateResponse(HttpStatusCode.OK);
        }

    Tento kód pošle oznámení všechny platformy ve stejnou dobu a aniž by bylo nutné zadat nativní data. Oznámení o rozbočovače vytvoří a poskytuje správné datové každým zařízením s hodnotou ujednaných _značku_ podle registrovaných šablon.

4. Znova Publikujte projekt back-end WebApi.

5. Znova spusťte aplikaci klienta a ověřte, že se mu registrace.

6. (Volitelné) Nasazení aplikace klienta do druhého zařízení a potom spusťte aplikaci.

    Všimněte si, že v každém zařízení se zobrazí oznámení.

## <a name="next-steps"></a>Další kroky

Teď, když jste dokončili tohoto kurzu, zjistíte něco víc o rozbočovače oznámení a šablon v těchto tématech:

+ **[Odeslání novinky pomocí rozbočovače oznámení]** <br/>Ukazuje jiné scénáře pro používání šablon

+  **[Přehled rozbočovače Azure oznámení][Templates]**<br/>Podrobnější informace o šablonách obsahuje téma přehledu.


<!-- Anchors. -->

<!-- Images. -->




<!-- URLs. -->
[Push to users ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Push to users Mobile Services]: /manage/services/notification-hubs/notify-users/
[Visual Studio 2012 Express for Windows 8]: http://go.microsoft.com/fwlink/?LinkId=257546

[Odeslání novinky pomocí rozbočovače oznámení]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[Azure Notification Hubs]: http://go.microsoft.com/fwlink/p/?LinkId=314257
[Informujte uživatele s rozbočovače oznámení]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Templates]: http://go.microsoft.com/fwlink/p/?LinkId=317339
[Notification Hub How to for Windows Store]: http://msdn.microsoft.com/library/windowsazure/jj927172.aspx
