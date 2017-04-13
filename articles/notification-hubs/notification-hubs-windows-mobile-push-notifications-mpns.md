<properties
    pageTitle="Odesílání nabízená oznámení s Azure oznámení rozbočovače na Windows Phone | Microsoft Azure"
    description="V tomto kurzu se naučíte, jak pomocí Azure oznámení rozbočovače nabízených oznámení pro Windows Phone 8 nebo Windows Phone 8.1 Silverlight aplikaci."
    services="notification-hubs"
    documentationCenter="windows"
    keywords="nabízená oznámení, nabízená oznámení pro windows phone připínáčku"
    authors="ysxu"
    manager="erikre"
    editor="erikre"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-with-azure-notification-hubs-on-windows-phone"></a>Odesílání nabízená oznámení s Azure oznámení rozbočovače na Windows Phone

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Základní informace

> [AZURE.NOTE] Tento kurz, musíte mít účet Azure active. Pokud nemáte účet, můžete vytvořit bezplatný účet zkušební v jenom pár minut. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-phone-get-started%2F).

Tento kurz se dozvíte, jak používat Azure oznámení rozbočovače odeslat nabízených oznámení pro Windows Phone 8 nebo Windows Phone 8.1 Silverlight aplikaci. Pokud směrujete Windows Phone 8.1 (bez Silverlight), potom nápovědě k [Windows univerzální](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) verze.
V tomto kurzu vytvoříte z prázdné aplikace Windows Phone 8, která přijímá nabízená oznámení pomocí Microsoft nabízená oznámení služby (MPNS). Až budete hotovi, budete moct používat rozbočovače oznámení vysílání nabízená oznámení na zařízeních s aplikací.

> [AZURE.NOTE] V oznámení o rozbočovače Windows Phone SDK nepodporuje Windows nabízená oznámení služby (WNS) pomocí aplikace Windows Phone 8.1 Silverlight. Pomocí aplikace Windows Phone 8.1 Silverlight WNS (místo MPNS), postupujte podle [Oznámení rozbočovače – kurz Silverlight Windows Phone], který využívá rozhraní REST API.

Kurz ukazuje jednoduchý scénář vysílání při používání rozbočovače oznámení.

##<a name="prerequisites"></a>Zjistit předpoklady pro

Tento kurz vyžaduje:

+ [Visual Studio 2012 Express pro Windows Phone], nebo jeho novější verzí.

Dokončení tohoto kurzu je požadována pro všechny ostatní oznámení rozbočovače výukové programy pro Windows Phone 8 aplikace.

##<a name="create-your-notification-hub"></a>Vytvoření rozbočovače oznámení

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li><p>Klikněte na oddíl <b>Oznámení služby</b> (v rámci <i>Nastavení</i>), klepněte na <b>Windows Phone (MPNS)</b> a potom zaškrtněte políčko <b>Povolit neověřených nabízených</b> .</p>
</li>
</ol>

&emsp;&emsp;![Azure portálu - povolit unathenticated nabízená oznámení](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)

Vaše centrum je teď vytvořili a odeslání neověřených oznámení pro Windows Phone.

> [AZURE.NOTE] Tento kurz vyžaduje MPNS v neověřených režimu. Režim neověřených MPNS přijat s omezeními zobrazit dotaz upozornění, které můžete poslat jednotlivé kanálu. Oznámení o rozbočovače podporuje [MPNS ověření režimu](http://msdn.microsoft.com/library/windowsphone/develop/ff941099.aspx) umožňuje odeslat certifikát.

##<a name="connecting-your-app-to-the-notification-hub"></a>Připojení aplikace k centru oznámení

1. Ve Visual Studiu vytvořte novou aplikaci Windows Phone 8.

    ![Aplikace Visual Studio – nový projekt – Windows Phone][13]

    Ve Visual Studiu 2013 aktualizace 2 nebo novější místo toho vytvoření aplikace Windows Phone Silverlight.

    ![Visual Studio – nový projekt - Blank aplikací – Windows Phone Silverlight][11]

2. Ve Visual Studiu klikněte pravým tlačítkem myši na řešení a potom klikněte na **Spravovat balíčků NuGet**.

    Zobrazí se dialogové okno **Spravovat balíčků NuGet** .

3. Vyhledejte `WindowsAzure.Messaging.Managed` klikněte na tlačítko **nainstalovat**a potom klikněte na přijmout podmínky použití.

    ![Visual Studio - NuGet balíčku správce][20]

    To ke stažení, instalace a přidá odkaz na knihovnu Azure zpráv pro systém Windows pomocí <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">WindowsAzure.Messaging.Managed NuGet balíčku</a>.

4. Otevřete soubor App.xaml.cs a přidejte následující `using` příkazy:

        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;

5. Přidejte následující kód v horní části **Application_Launching** metodu App.xaml.cs:

        var channel = HttpNotificationChannel.Find("MyPushChannel");
        if (channel == null)
        {
            channel = new HttpNotificationChannel("MyPushChannel");
            channel.Open();
            channel.BindToShellToast();
        }

        channel.ChannelUriUpdated += new EventHandler<NotificationChannelUriEventArgs>(async (o, args) =>
        {
            var hub = new NotificationHub("<hub name>", "<connection string>");
            var result = await hub.RegisterNativeAsync(args.ChannelUri.ToString());

            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
            {
                MessageBox.Show("Registration :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
            });
        });

    >[AZURE.NOTE] Hodnota **MyPushChannel** je index, který se používá k vyhledání existujících kanálu v kolekci [HttpNotificationChannel](https://msdn.microsoft.com/library/windows/apps/microsoft.phone.notification.httpnotificationchannel.aspx) . Pokud není jeden tam, vytvořte novou položku s tímto názvem.
    
    Nezapomeňte vložit název rozbočovače a připojovací řetězec s názvem **DefaultListenSharedAccessSignature** , který jste získali v předchozí části.
    Tento kód načte kanálu URI aplikace z MPNS a pak tento kanál URI zaregistruje s rozbočovače oznámení. Je taky zaručuje, že kanál, který URI registraci v centrální oznámení při každém spuštění aplikace.

    >[AZURE.NOTE]Tento kurz pošle oznámením zařízení. Při odeslání oznámení dlaždice musí místo toho zavolat metodu **BindToShellTile** na kanál. K podpoře obou oznámením a dlaždice oznámení, zavolejte na **BindToShellTile** a **BindToShellToast**.

6. V Průzkumníku rozbalte **Vlastnosti**, otevřete `WMAppManifest.xml` soubor, klikněte na kartu **Možnosti** a ujistěte se, že je zaškrtnuta možnost **ID_CAP_PUSH_NOTIFICATION** .

    ![Visual Studio – Windows Phone funkcí aplikace][14]

    Zajistíte tím, že aplikace můžete příjem nabízených oznámení. Bez něho všechny odeslání nabízená oznámení v aplikaci nezdaří.

7. Stiskněte `F5` klávesy spustit aplikaci.

    V seznamu aplikací se zobrazí zpráva registrace.

8. Ukončete aplikaci.  

   >[AZURE.NOTE] Pro příjem nabízených oznámením, nesmí být aplikace spuštěna v popředí.

##<a name="send-push-notifications-from-your-backend"></a>Odeslání nabízená oznámení z vaší back-end

Odeslání nabízená oznámení pomocí rozbočovače oznámení ze všech back-end prostřednictvím veřejné <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">rozhraní REST</a>. V tomto kurzu odesíláte nabízená oznámení pomocí aplikace konzoly .NET. 

Příklad posílání nabízená oznámení z ASP.NET WebAPI back-end, která je integrována rozbočovače oznámení najdete v článku [Azure oznámení rozbočovače oznámení s uživateli back-end .NET](./notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md).  

Příklad toho, jak chcete poslat nabízená oznámení pomocí rozhraní [REST API](https://msdn.microsoft.com/library/azure/dn223264.aspx), podívejte se na [použití rozbočovače oznámení z Java](./notification-hubs-java-push-notification-tutorial.md) a [jak používat rozbočovače oznámení z PHP](./notification-hubs-php-push-notification-tutorial.md).

1. Klikněte pravým tlačítkem myši na řešení, vyberte **Přidat** a **Nový projekt...**a potom v části **Visual Basic**, klikněte na **Windows** a **Aplikace konzoly**a klikněte na **OK**.

    ![Aplikace Visual Studio – nový projekt - konzoly][6]

    Tím přidáte nové vizuální C# aplikace konzoly řešení. Můžete taky provést v samostatném řešení.

4. Klikněte na **Nástroje**, klikněte na **Správce balíčku knihovny**a klikněte na **Správce balíčků konzoly**.

    Zobrazí se konzole balíčku správce.

5.  V okně **Správce balíčků konzoly** nastavit **výchozí projektu** nový projekt aplikace konzoly a v okně konzoly spusťte následující příkaz:

        Install-Package Microsoft.Azure.NotificationHubs

    Přidá odkaz na SDK rozbočovače oznámení Azure pomocí <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification rozbočovače NuGet balíčku</a>.

6. Otevřít `Program.cs` soubor a přidejte následující `using` údajů:

        using Microsoft.Azure.NotificationHubs;

6. V `Program` třídy, přidejte následujícím způsobem:

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient
                .CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            string toast = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                "<wp:Notification xmlns:wp=\"WPNotification\">" +
                   "<wp:Toast>" +
                        "<wp:Text1>Hello from a .NET App!</wp:Text1>" +
                   "</wp:Toast> " +
                "</wp:Notification>";
            await hub.SendMpnsNativeNotificationAsync(toast);
        }

    Ujistěte se, pokud chcete nahradit `<hub name>` zástupný symbol s názvem centrální oznámení, která se zobrazí na portálu. Zástupný symbol řetězec připojení taky nahraďte připojovací řetězec s názvem **DefaultFullSharedAccessSignature** , který jste získali v části "Konfigurovat rozbočovače oznámení."

    >[AZURE.NOTE]Ujistěte se, používají připojovací řetězec s **úplným** přístupem, ne **Poslech** přístup. Poslech přístupový řetězec nemá oprávnění k odesílání nabízená oznámení.

4. Přidejte následující řádek ve vaší `Main` metodu:

         SendNotificationAsync();
         Console.ReadLine();

5. Svůj Windows Phone emulátoru systém a aplikace zavřené, nastavte projekt aplikace konzoly jako výchozí při spuštění projekt a potom stiskněte `F5` klávesy spustit aplikaci.

    Zobrazí se oznámení nabízená oznámení. Klepnutím nápis oznámením načte aplikace.

Všechny možné datové části najdete v tématech [oznámením katalogu] a [dlaždice katalogu] na webu MSDN.

##<a name="next-steps"></a>Další kroky

V tomto příkladu jednoduché vysílat nabízená oznámení na všech svých zařízeních Windows Phone 8. 

S cílem zacílit určité uživatele, podívejte se do kurz [Používání oznámení rozbočovače nabízených oznámení pro uživatele] . 

Pokud budete chtít segmentech uživatelů tak, že skupin úrok, si můžete přečíst [Rozbočovače použití oznámení o odeslání novinky]. 

Další informace o používání upozornění rozbočovače [oznámení rozbočovače]pokyny.



<!-- Images. -->
[6]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png
[7]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal.png
[8]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal2.png
[9]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal.png
[10]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal2.png
[11]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-silverlight-app.png
[12]: ./media/notification-hubs-windows-phone-get-started/notification-hub-connection-strings.png

[13]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-app.png
[14]: ./media/notification-hubs-windows-phone-get-started/mobile-app-enable-push-wp8.png
[15]: ./media/notification-hubs-windows-phone-get-started/notification-hub-pushauth.png
[20]: ./media/notification-hubs-windows-phone-get-started/notification-hub-windows-universal-app-install-package.png
[213]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png





<!-- URLs. -->
[Visual Studio 2012 Express pro Windows Phone]: https://go.microsoft.com/fwLink/p/?LinkID=268374
[Pokyny pro rozbočovače oznámení]: http://msdn.microsoft.com/library/jj927170.aspx
[MPNS authenticated mode]: http://msdn.microsoft.com/library/windowsphone/develop/ff941099(v=vs.105).aspx
[Pomocí oznámení rozbočovače nabízených oznámení pro uživatele]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Odeslání novinky pomocí rozbočovače oznámení]: notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md
[informační katalogu]: http://msdn.microsoft.com/library/windowsphone/develop/jj662938(v=vs.105).aspx
[dlaždice katalogu]: http://msdn.microsoft.com/library/windowsphone/develop/hh202948(v=vs.105).aspx
[Oznámení o rozbočovače – kurz Silverlight Windows Phone]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSLPhoneApp

