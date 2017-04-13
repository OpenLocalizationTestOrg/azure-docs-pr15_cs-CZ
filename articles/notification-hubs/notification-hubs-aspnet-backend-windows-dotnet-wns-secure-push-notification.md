<properties
    pageTitle="Nabízená oznámení Azure rozbočovače zabezpečené"
    description="Informace o odeslání zabezpečené nabízená oznámení v Azure. Ukázky napsané v jazyce C# pomocí rozhraní API .NET."
    documentationCenter="windows"
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs" 
    ms.workload="mobile"
    ms.tgt_pltfrm="windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-secure-push"></a>Nabízená oznámení Azure rozbočovače zabezpečené

> [AZURE.SELECTOR]
- [Univerzální systému Windows](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
- [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
- [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)


##<a name="overview"></a>Základní informace

Podpora nabízených oznámení v Microsoft Azure umožňuje přístup k snadno použitelné s více platformami, diagramů s měřítky mimo nabízených infrastruktury, což výrazně zjednodušuje provádění nabízených oznámení pro aplikace příjemce a enterprise pro mobilní platformy.

Z důvodu zákonné nebo omezení zabezpečení někdy aplikace může být vhodné zahrnout něco upozornění, které nelze předávají infrastruktury standardní nabízená oznámení. Tento kurz popisuje, jak dosáhnout stejné prostředí tak, že citlivé informace pomocí funkce zabezpečené ověřené připojení mezi klientského zařízení a back-end aplikace.

Na vysoké úrovni, řízení toku, je takto:

1. Aplikace back-end:
    - Úložiště zabezpečené datové v back-end databázi.
    - ID toto oznámení odešle zařízení (se neodesílají žádné informace zabezpečené).
2. Aplikaci v zařízení pro příjem oznámení:
    - Zařízení kontaktů back-end požaduje zabezpečené datové.
    - Aplikaci můžete zobrazit datové jako upozornění na zařízení.

Je důležité mít na paměti, že v předchozím tok (a v tomto kurzu) jsme se předpokládá, že zařízení ukládá token ověřování místní úložiště, po přihlášení. Úplně bezproblémovou práci, zajistíte tak jako zařízení můžete načíst pomocí tohoto tokenu zabezpečení datové na upozornění. Pokud aplikace neukládá tokeny ověřování na zařízení nebo můžete platnost těchto tokenů, aplikaci zařízení po přijetí oznámení by měl zobrazit obecné oznámení dotázání uživatele k spuštění aplikace. Aplikace potom ověřuje uživatele a zobrazí datové oznámení.

Tento kurz zabezpečené nabízených ukazuje, jak bezpečně odeslání nabízená oznámení. Kurz je založena na kurz [Upozornit uživatele](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) tak, aby měli dokončení kroků v tomto kurzu nejdřív.

> [AZURE.NOTE] Tento kurz se předpokládá, že jste vytvořili a nakonfigurovali rozbočovače oznámení podle popisu v [Příručce Začínáme s rozbočovače oznámení (pro Windows Store)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).
Všimněte si také, že Windows Phone 8.1 vyžaduje přihlašovací údaje Windows (ne Windows Phone) a že úlohy na pozadí nefungují na Windows Phone 8.0 nebo Silverlight 8.1. V aplikacích pro Windows Store, můžete dostávat upozornění prostřednictvím úlohy na pozadí jenom v případě, že je povolená zamykací obrazovka aplikace (klepnutím na zaškrtávací políčko v Appmanifest).

[AZURE.INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-windows-phone-project"></a>Upravit projekt Windows Phone

1. V projectu **NotifyUserWindowsPhone** přidejte následující kód App.xaml.cs zaregistrovat push úloh pozadí. Přidat následující kód na konci `OnLaunched()` metodu:

        RegisterBackgroundTask();

2. Stále nacházejí v App.xaml.cs, přidejte následující kód bezprostředně za `OnLaunched()` metodu:

        private async void RegisterBackgroundTask()
        {
            if (!Windows.ApplicationModel.Background.BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name == "PushBackgroundTask"))
            {
                var result = await BackgroundExecutionManager.RequestAccessAsync();
                var builder = new BackgroundTaskBuilder();

                builder.Name = "PushBackgroundTask";
                builder.TaskEntryPoint = typeof(PushBackgroundComponent.PushBackgroundTask).FullName;
                builder.SetTrigger(new Windows.ApplicationModel.Background.PushNotificationTrigger());
                BackgroundTaskRegistration task = builder.Register();
            }
        }

3. Přidejte následující `using` příkazy v horní části souboru App.xaml.cs:

        using Windows.Networking.PushNotifications;
        using Windows.ApplicationModel.Background;

4. V nabídce **soubor** ve Visual Studiu klikněte na **Uložit vše**.

## <a name="create-the-push-background-component"></a>Vytvoření komponentu nabízených pozadí

Dalším krokem je vytvoření komponentu nabízených pozadí.

1. V Průzkumníku klikněte pravým tlačítkem myši na nejvyšší uzel má toto řešení (**SecurePush řešení** v tomto případě), klikněte na tlačítko **Přidat**a potom klikněte na **Nový projekt**.

2. Rozšíření **Úložiště přihlašovacích údajů aplikace**, a pak klikněte na **Windows Phone aplikace**a potom na **Modul Runtime součásti systému Windows (Windows Phone)**. Název projektu **PushBackgroundComponent**a potom klikněte na **OK** vytvořte projekt.

    ![][12]

3. V okně Průzkumník projektu **PushBackgroundComponent (Windows Phone 8.1)** , klikněte pravým tlačítkem myši a pak klikněte na **Přidat**a potom na **předmětu**. Zadejte název nové třídy **PushBackgroundTask.cs**. Klikněte na **Přidat** do generovat předmětu.

4. Nahradit celý obsah definici obor názvů **PushBackgroundComponent** následující kód nahrazení zástupného symbolu `{back-end endpoint}` s koncovým back-end získali při nasazování back-end:

        public sealed class Notification
            {
                public int Id { get; set; }
                public string Payload { get; set; }
                public bool Read { get; set; }
            }

            public sealed class PushBackgroundTask : IBackgroundTask
            {
                private string GET_URL = "{back-end endpoint}/api/notifications/";

                async void IBackgroundTask.Run(IBackgroundTaskInstance taskInstance)
                {
                    // Store the content received from the notification so it can be retrieved from the UI.
                    RawNotification raw = (RawNotification)taskInstance.TriggerDetails;
                    var notificationId = raw.Content;

                    // retrieve content
                    BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
                    var httpClient = new HttpClient();
                    var settings = ApplicationData.Current.LocalSettings.Values;
                    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);

                    var notificationString = await httpClient.GetStringAsync(GET_URL + notificationId);

                    var notification = JsonConvert.DeserializeObject<Notification>(notificationString);

                    ShowToast(notification);

                    deferral.Complete();
                }

                private void ShowToast(Notification notification)
                {
                    ToastTemplateType toastTemplate = ToastTemplateType.ToastText01;
                    XmlDocument toastXml = ToastNotificationManager.GetTemplateContent(toastTemplate);
                    XmlNodeList toastTextElements = toastXml.GetElementsByTagName("text");
                    toastTextElements[0].AppendChild(toastXml.CreateTextNode(notification.Payload));
                    ToastNotification toast = new ToastNotification(toastXml);
                    ToastNotificationManager.CreateToastNotifier().Show(toast);
                }
            }

5. V okně Průzkumník projektu **PushBackgroundComponent (Windows Phone 8.1)** pravým tlačítkem klikněte na **Spravovat balíčků NuGet**.

6. Na levé straně klikněte na tlačítko **Online**.

7. Do pole **Hledat** zadejte **Http klienta**.

8. V seznamu výsledků klikněte na **Knihoven klienta Microsoft HTTP**a potom klikněte na **nainstalovat**. Dokončete instalaci.

9. Zpět do NuGet **vyhledávacího** pole zadejte **Json.net**. Nainstalujte balíček **Json.NET** a potom zavřete okno Správce balíčků NuGet.

10. Přidejte následující `using` příkazy v horní části souboru **PushBackgroundTask.cs** :

        using Windows.ApplicationModel.Background;
        using Windows.Networking.PushNotifications;
        using System.Net.Http;
        using Windows.Storage;
        using System.Net.Http.Headers;
        using Newtonsoft.Json;
        using Windows.UI.Notifications;
        using Windows.Data.Xml.Dom;

11. V Průzkumníku v projektu **NotifyUserWindowsPhone (Windows Phone 8.1)** klikněte pravým tlačítkem na **odkazy**a potom klikněte na **Přidat odkaz**. V dialogovém okně Správce odkaz zaškrtněte políčko vedle **PushBackgroundComponent**a potom klikněte na **OK**.

12. V Průzkumníku poklikejte **Package.appxmanifest** v aplikaci project **NotifyUserWindowsPhone (Windows Phone 8.1)** . V části **oznámení**nastavena na hodnotu **Ano** **Oznámením může** .

    ![][3]

13. Pořád v **Package.appxmanifest**klikněte na nabídku **deklarace** horní. V rozevíracím seznamu **Dostupná deklarace** klikněte na **Úlohy na pozadí**a potom klikněte na **Přidat**.

14. V **Package.appxmanifest**ve skupinovém rámečku **Vlastnosti**zaškrtněte políčko **nabízená oznámení**.

15. Do **Package.appxmanifest**v části **Nastavení aplikace**zadejte **PushBackgroundComponent.PushBackgroundTask** v poli **Vstupní bod** .

    ![][13]

16. V nabídce **soubor** klikněte na **Uložit vše**.

## <a name="run-the-application"></a>Spusťte aplikaci

Pokud chcete spustit aplikaci, postupujte takto:

1. Ve Visual Studiu spusťte aplikaci rozhraní API webových **AppBackend** . Zobrazí se webová stránka ASP.NET.

2. Ve Visual Studiu spusťte aplikaci Windows Phone **NotifyUserWindowsPhone (Windows Phone 8.1)** . Windows Phone emulátoru spustí a automaticky načte aplikace.

3. V aplikaci **NotifyUserWindowsPhone** uživatelského rozhraní zadejte uživatelské jméno a heslo. Tyto může být jakýkoli řetězec, ale musí mít stejnou hodnotu.

4. V aplikaci **NotifyUserWindowsPhone** uživatelského rozhraní klikněte na **přihlásit a zaregistrovat**. Klepněte na tlačítko **Odeslat připínáčku**.

[3]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push3.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push13.png
