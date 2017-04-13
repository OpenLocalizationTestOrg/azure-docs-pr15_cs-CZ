<properties
    pageTitle="Použití oznámení rozbočovače k odeslání Novinky (Windows Universal)"
    description="Použití Azure oznámení rozbočovače se značkami v registrace k odeslání novinky univerzální aplikace Windows."
    services="notification-hubs"
    documentationCenter="windows"
    authors="ysxu"
    manager="erikre"
    editor=""/>


<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-breaking-news"></a>Odeslání novinky pomocí rozbočovače oznámení


[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]


##<a name="overview"></a>Základní informace

V tomto tématu se dozvíte, jak používat Azure oznámení rozbočovače vysílání nejnovějších příspěvků oznámení pro Windows Store nebo aplikace Windows Phone 8.1 (bez Silverlight). Pokud směrujete Windows Phone 8.1 Silverlight, získáte verze [Windows Phone](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md) . Až budete hotovi, bude moct zaregistrujte si nejnovější příspěvky kategorií, které vás zajímají a zobrazí jenom nabízených oznámení pro těchto kategorií. Tento scénář je běžné vzor pro mnoho aplikace, kde oznámení musí být odeslány do skupiny uživatelů, které jste dříve deklarované zájem je například RSS reader, který je aplikace pro ventilátory hudby a tak dál. 

Vysílání scénáře jsou povoleny s využitím jednu nebo více _značek_ při vytváření registrace v centru oznámení. Pokud značku posílají oznámení o, všechna zařízení, které jste si zaregistrovali značku dostávat oznámení. Protože značek jsou jednoduše řetězce, nemají zřízení předem. Další informace o značky podívejte se do [směrování rozbočovače oznámení a výrazy značku](notification-hubs-tags-segment-push-message.md).

##<a name="prerequisites"></a>Zjistit předpoklady pro

Toto téma je založena na aplikaci vytvořené v části [Začínáme s oznámení rozbočovače][get-started]. Před zahájením tohoto kurzu, třeba již jste dokončili [začít pracovat s oznámení rozbočovače][get-started].

##<a name="add-category-selection-to-the-app"></a>Výběr kategorie přidejte do aplikace

Cílem prvního kroku je přidání prvků uživatelského rozhraní existující hlavní stránky, které umožňují uživatelům vybrat kategorie k registraci. Kategorie vybrané uživatelem jsou uložené na zařízení. Po spuštění aplikace, vytvoří se registrace zařízení jako značky ve rozbočovače oznámení s vybrané kategorie.

1. Otevření souboru projektu MainPage.xaml a potom zkopírujte následující kód v elementu **mřížky** :

        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition/>
                <ColumnDefinition/>
            </Grid.ColumnDefinitions>
            <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="1" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="2" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="3" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="1" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="2" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="3" Grid.Column="1" HorizontalAlignment="Center"/>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="4" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click"/>
        </Grid>


2. Klikněte pravým tlačítkem myši **sdílené** projektu a přidat novou třídu s názvem **oznámení**, přidejte modifikátorem **veřejné** definici třídy a pak přidejte následující příkazy **pomocí** nový soubor kód:

        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.Storage;
        using System.Threading.Tasks;

3. Zkopírujte následující kód do nové třídy **oznámení** :

        private NotificationHub hub;

        public Notifications(string hubName, string listenConnectionString)
        {
            hub = new NotificationHub(hubName, listenConnectionString);
        }

        public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            return await SubscribeToCategories(categories);
        }

        public IEnumerable<string> RetrieveCategories()
        {
            var categories = (string) ApplicationData.Current.LocalSettings.Values["categories"];
            return categories != null ? categories.Split(','): new string[0];
        }

        public async Task<Registration> SubscribeToCategories(IEnumerable<string> categories = null)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            if (categories == null)
            {
                categories = RetrieveCategories();
            }

            // Using a template registration to support notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.

            const string templateBodyWNS = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "simpleWNSTemplateExample",
                    categories);
        }

    Této třídy používá místní úložiště pro ukládání kategorií příspěvků, který má toto zařízení dostávat. Všimněte si, že místo volání metody *RegisterNativeAsync* jsme volat *RegisterTemplateAsync* zaregistrovat k kategorie pomocí šablony registrace. 
    
    Také nabízíme název šablony ("simpleWNSTemplateExample"), protože jsme možná budete chtít zaregistrovat více šablon (například jedno pro oznámení informačního) a jedno pro dlaždice a potřebujeme pojmenování provést aktualizaci nebo jejich odstranění.

    Poznámka: Pokud zařízení registruje více šablon stejnou značku, příchozí zprávy zacílení, značky bude mít za následek více oznámení doručení zařízení (jedno pro každé šablony). Toto chování je užitečná, když bude stejná zpráva logické má za následek více vizuálních upozornění, například ukazující Odznáček i oznámením do aplikace pro Windows Store.

    Další informace o šablonách naleznete v tématu [šablony](notification-hubs-templates-cross-platform-push-messages.md).




4. V souboru projektu App.xaml.cs přidejte následující vlastnosti třídy **aplikace** :

        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");

    Tato vlastnost se používá k vytváření a získat přístup k instanci **oznámení** .

    Ve výše uvedeném kódu nahradit `<hub name>` a `<connection string with listen access>` zástupných symbolů s vaším jménem centrální oznámení a připojovací řetězec pro *DefaultListenSharedAccessSignature* , který jste získali dříve.

    > [AZURE.NOTE] Protože přihlašovací údaje, které jsou rozdělení s klientské aplikace nejsou obecně zabezpečené, by měl distribuovat klávesu poslech přístup pomocí protokolu pouze s aplikací klienta. Příjem umožňuje přístup k aplikaci zaregistrovat k oznámení, ale existující registrace nelze upravit, a nemůže odeslána oznámení následujícím. Plný přístup používá se služby Zabezpečené back-end pro posílání oznámení a změna existující registrace.

5. Ve vaší MainPage.xaml.cs přidejte následující řádek:

        using Windows.UI.Popups;

6. V souboru projektu MainPage.xaml.cs přidáte následujícím způsobem:

        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var categories = new HashSet<string>();
            if (WorldToggle.IsOn) categories.Add("World");
            if (PoliticsToggle.IsOn) categories.Add("Politics");
            if (BusinessToggle.IsOn) categories.Add("Business");
            if (TechnologyToggle.IsOn) categories.Add("Technology");
            if (ScienceToggle.IsOn) categories.Add("Science");
            if (SportsToggle.IsOn) categories.Add("Sports");

            var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);

            var dialog = new MessageDialog("Subscribed to: " + string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }

    Tento způsob vytvoří seznam kategorií a používá třídu **oznámení** ukládat seznamu v místní úložiště a zaregistrujte příslušné značky v centrální oznámení. Při změně kategorie znovu pomocí nových kategorií vytvoří registrace.

Aplikace je teď možné ukládat sadu kategorií do místní úložiště v zařízení a zaregistrovat v centru oznámení pokaždé, když uživatel změní výběru kategorie.

##<a name="register-for-notifications"></a>Registrace k oznámení

Tento postup zaregistrujte v centru oznámení při spuštění pomocí kategorií, které byly uložené v místním úložišti.

> [AZURE.NOTE] Protože kanálu URI přiřazené tak, že oznámení služby systému Windows (WNS) můžete kdykoli změnit, byste měli zaregistrovat oznámení často chcete-li předejít oznámení o selhání. V tomto příkladu zaregistruje pro oznámení o každém spuštění aplikace. K aplikacím, které často se mají spustit více než jednou za den, můžete pravděpodobně přeskočit registrace Chcete-li zachovat šířky pásma, pokud od předchozího registrace uplynul menší než jeden den.

1. Otevřete soubor App.xaml.cs a aktualizovat metodu **InitNotificationsAsync** používat `notifications` třídy pro přihlášení k odběru podle kategorie.

        // *** Remove or comment out these lines *** 
        //var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
        //var hub = new NotificationHub("your hub name", "your listen connection string");
        //var result = await hub.RegisterNativeAsync(channel.Uri);
    
        var result = await notifications.SubscribeToCategories();

    Zajistíte tím, že při každém spuštění aplikace načte kategorií z místního úložiště a žádosti registeration těchto kategorií. Metoda **InitNotificationsAsync** byl vytvořen jako součást [začít pracovat s oznámení rozbočovače] [ get-started] kurz.

3. V souboru projektu MainPage.xaml.cs přidáte následující kód metody *OnNavigatedTo* :

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var categories = ((App)Application.Current).notifications.RetrieveCategories();

            if (categories.Contains("World")) WorldToggle.IsOn = true;
            if (categories.Contains("Politics")) PoliticsToggle.IsOn = true;
            if (categories.Contains("Business")) BusinessToggle.IsOn = true;
            if (categories.Contains("Technology")) TechnologyToggle.IsOn = true;
            if (categories.Contains("Science")) ScienceToggle.IsOn = true;
            if (categories.Contains("Sports")) SportsToggle.IsOn = true;
        }

    Toto tlačítko aktualizujete hlavní stránku založenou na stav dříve uložené kategorie.

Aplikace je teď dokončení a mohou být uloženy sadu kategorií v místní úložiště zařízení používá k registraci centru oznámení pokaždé, když uživatel změní výběru kategorie. Dále jsme definovat back-end, které můžou odesílat upozornění na kategorii této aplikace.

##<a name="sending-tagged-notifications"></a>Odeslání příznakem oznámení

[AZURE.INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

##<a name="run-the-app-and-generate-notifications"></a>Spusťte aplikaci a generovat oznámení

1. Ve Visual Studiu stisknutím klávesy F5 kompilaci a spusťte aplikaci.

    ![][1]

    Poznámka: aby aplikaci uživatelského rozhraní k dispozici sadu přepíná, který umožňuje zvolte kategorií, které se přihlásit k odběru.

2. Povolení jeden nebo více přepíná kategorií a potom klikněte na **Přihlásit se k odběru**.

    Aplikace převede vybrané kategorie značky a žádosti nové registrace zařízení pro vybrané značky z centra oznámení. Registrovaná kategorie jsou získány a zobrazeny v dialogovém okně.

    ![][19]

4. Odeslání oznámení o nové z back-end v jednom z těchto způsobů:

    + **Konzoly aplikace:** spusťte aplikaci konzoly.

    + **Java/PHP:** spustit aplikaci/skript.

    Oznámení pro vybrané kategorie se zobrazí jako informačního oznámení.

    ![][14]

##<a name="next-steps"></a>Další kroky

V tomto kurzu budeme naučili vysílání novinky podle kategorie. Zkuste jednu z následujících výukové programy, které zvýrazňují další rozšířené scénáře rozbočovače oznámení o dokončení:

+ [Použití oznámení rozbočovače vysílání lokalizované novinky]

    Zjistěte, jak rozbalte aplikaci nejnovějších příspěvků povolit odesílání lokalizované upozornění.



<!-- Anchors. -->
[Add category selection to the app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run the app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-breakingnews-win1.png

[14]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-toast-2.png


[19]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-reg-2.png

<!-- URLs.-->
[get-started]: /manage/services/notification-hubs/getting-started-windows-dotnet/
[Použití oznámení rozbočovače vysílání lokalizované novinky]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
