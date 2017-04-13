<properties
    pageTitle="Použití oznámení rozbočovače k odeslání Novinky (Windows Phone)"
    description="Použijte Azure oznámení rozbočovače značku v registrace k odeslání Novinky v aplikaci Windows Phone."
    services="notification-hubs"
    documentationCenter="windows"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-breaking-news"></a>Odeslání novinky pomocí rozbočovače oznámení

[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]


##<a name="overview"></a>Základní informace

V tomto tématu se dozvíte, jak používat Azure oznámení rozbočovače vysílání nejnovějších příspěvků oznámení v aplikaci Windows Phone 8.0/8.1 Silverlight. Pokud směrujete pro Windows Store nebo aplikace Windows Phone 8.1, získáte na [Windows univerzální](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md) verzi. Až budete hotovi, bude moct zaregistrujte si nejnovější příspěvky kategorií, které vás zajímají a zobrazí jenom nabízených oznámení pro těchto kategorií. Tento scénář je běžné vzor pro mnoho aplikace, kde oznámení musí být odeslány do skupiny uživatelů, které jste dříve deklarované zájem o nich, například programu pro čtení RSS, aplikace pro hudbu ventilátory atd.

Vysílání scénáře jsou povoleny s využitím jednu nebo více _značek_ při vytváření registrace v centru oznámení. Pokud značku posílají oznámení o, všechna zařízení, které jste si zaregistrovali značku dostávat oznámení. Protože značek jsou jednoduše řetězce, nemají zřízení předem. Další informace o značky podívejte se do [směrování rozbočovače oznámení a výrazy značku](notification-hubs-tags-segment-push-message.md).

##<a name="prerequisites"></a>Zjistit předpoklady pro

Toto téma je založena na aplikaci, kterou jste vytvořili v [začít pracovat s rozbočovače oznámení]. Před zahájením tohoto kurzu, třeba již jste dokončili [začít pracovat s rozbočovače oznámení].

##<a name="add-category-selection-to-the-app"></a>Výběr kategorie přidejte do aplikace

Cílem prvního kroku je přidání prvků uživatelského rozhraní existující hlavní stránky, které umožňují uživatelům vybrat kategorie k registraci. Kategorie vybrané uživatelem jsou uložené na zařízení. Po spuštění aplikace, vytvoří se registrace zařízení jako značky ve rozbočovače oznámení s vybrané kategorie.

1. Otevření souboru projektu MainPage.xaml a potom nahraďte **mřížky** prvky s názvem `TitlePanel` a `ContentPanel` s kódem takto:

        <StackPanel x:Name="TitlePanel" Grid.Row="0" Margin="12,17,0,28">
            <TextBlock Text="Breaking News" Style="{StaticResource PhoneTextNormalStyle}" Margin="12,0"/>
            <TextBlock Text="Categories" Margin="9,-7,0,0" Style="{StaticResource PhoneTextTitle1Style}"/>
        </StackPanel>

        <Grid Name="ContentPanel" Grid.Row="1" Margin="12,0,12,0">
            <Grid.RowDefinitions>
                <RowDefinition Height="auto"/>
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition />
                <ColumnDefinition />
            </Grid.ColumnDefinitions>
            <CheckBox Name="WorldCheckBox" Grid.Row="0" Grid.Column="0">World</CheckBox>
            <CheckBox Name="PoliticsCheckBox" Grid.Row="1" Grid.Column="0">Politics</CheckBox>
            <CheckBox Name="BusinessCheckBox" Grid.Row="2" Grid.Column="0">Business</CheckBox>
            <CheckBox Name="TechnologyCheckBox" Grid.Row="0" Grid.Column="1">Technology</CheckBox>
            <CheckBox Name="ScienceCheckBox" Grid.Row="1" Grid.Column="1">Science</CheckBox>
            <CheckBox Name="SportsCheckBox" Grid.Row="2" Grid.Column="1">Sports</CheckBox>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="3" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
        </Grid>

2. V aplikaci project vytvořte novou třídu s názvem **oznámení**, přidejte modifikátorem **veřejné** definici třídy a pak přidejte následující příkazy **pomocí** nový soubor kód:

        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;
        using System.IO.IsolatedStorage;
        using System.Windows;

3. Zkopírujte následující kód do nové třídy **oznámení** :

        private NotificationHub hub;

        // Registration task to complete registration in the ChannelUriUpdated event handler
        private TaskCompletionSource<Registration> registrationTask;

        public Notifications(string hubName, string listenConnectionString)
        {
            hub = new NotificationHub(hubName, listenConnectionString);
        }

        public IEnumerable<string> RetrieveCategories()
        {
            var categories = (string)IsolatedStorageSettings.ApplicationSettings["categories"];
            return categories != null ? categories.Split(',') : new string[0];
        }

        public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
        {
            var categoriesAsString = string.Join(",", categories);
            var settings = IsolatedStorageSettings.ApplicationSettings;
            if (!settings.Contains("categories"))
            {
                settings.Add("categories", categoriesAsString);
            }
            else
            {
                settings["categories"] = categoriesAsString;
            }
            settings.Save();

            return await SubscribeToCategories();
        }

        public async Task<Registration> SubscribeToCategories()
        {
            registrationTask = new TaskCompletionSource<Registration>();

            var channel = HttpNotificationChannel.Find("MyPushChannel");

            if (channel == null)
            {
                channel = new HttpNotificationChannel("MyPushChannel");
                channel.Open();
                channel.BindToShellToast();
                channel.ChannelUriUpdated += channel_ChannelUriUpdated;

                // This is optional, used to receive notifications while the app is running.
                channel.ShellToastNotificationReceived += channel_ShellToastNotificationReceived;
            }

            // If channel.ChannelUri is not null, we will complete the registrationTask here.  
            // If it is null, the registrationTask will be completed in the ChannelUriUpdated event handler.
            if (channel.ChannelUri != null)
            {
                await RegisterTemplate(channel.ChannelUri);
            }
            
            return await registrationTask.Task;
        }

        async void channel_ChannelUriUpdated(object sender, NotificationChannelUriEventArgs e)
        {
            await RegisterTemplate(e.ChannelUri);
        }

        async Task<Registration> RegisterTemplate(Uri channelUri)
        {
            // Using a template registration to support notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.

            const string templateBodyMPNS = "<wp:Notification xmlns:wp=\"WPNotification\">" +
                                                "<wp:Toast>" +
                                                    "<wp:Text1>$(messageParam)</wp:Text1>" +
                                                "</wp:Toast>" +
                                            "</wp:Notification>";

            // The stored categories tags are passed with the template registration.

            registrationTask.SetResult(await hub.RegisterTemplateAsync(channelUri.ToString(), 
                templateBodyMPNS, "simpleMPNSTemplateExample", this.RetrieveCategories()));

            return await registrationTask.Task;
        }

        // This is optional. It is used to receive notifications while the app is running.
        void channel_ShellToastNotificationReceived(object sender, NotificationEventArgs e)
        {
            StringBuilder message = new StringBuilder();
            string relativeUri = string.Empty;

            message.AppendFormat("Received Toast {0}:\n", DateTime.Now.ToShortTimeString());

            // Parse out the information that was part of the message.
            foreach (string key in e.Collection.Keys)
            {
                message.AppendFormat("{0}: {1}\n", key, e.Collection[key]);

                if (string.Compare(
                    key,
                    "wp:Param",
                    System.Globalization.CultureInfo.InvariantCulture,
                    System.Globalization.CompareOptions.IgnoreCase) == 0)
                {
                    relativeUri = e.Collection[key];
                }
            }

            // Display a dialog of all the fields in the toast.
            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() => 
            { 
                MessageBox.Show(message.ToString()); 
            });
        }


    Tato třída používá k ukládání kategorií příspěvků, o kterou toto zařízení dostávat izolace úložiště. Obsahuje také metody zaregistrovat k těchto kategorií pomocí zápisu oznámení [šablony](notification-hubs-templates-cross-platform-push-messages.md) .


4. V souboru projektu App.xaml.cs přidejte následující vlastnosti třídy **aplikace** . Nahrazení `<hub name>` a `<connection string with listen access>` zástupné symboly s vaším jménem centrální oznámení a připojovací řetězec pro *DefaultListenSharedAccessSignature* , který jste získali dříve.

        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");

    > [AZURE.NOTE] Protože přihlašovací údaje, které jsou rozdělení s klientské aplikace nejsou obecně zabezpečené, by měl distribuovat klávesu poslech přístup pomocí protokolu pouze s aplikací klienta. Příjem umožňuje přístup k aplikaci zaregistrovat k oznámení, ale existující registrace nelze upravit, a nemůže odeslána oznámení následujícím. Plný přístup používá se služby Zabezpečené back-end pro posílání oznámení a změna existující registrace.

5. Ve vaší MainPage.xaml.cs přidejte následující řádek:

        using Windows.UI.Popups;

6. V souboru projektu MainPage.xaml.cs přidáte následujícím způsobem:

        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
          var categories = new HashSet<string>();
          if (WorldCheckBox.IsChecked == true) categories.Add("World");
          if (PoliticsCheckBox.IsChecked == true) categories.Add("Politics");
          if (BusinessCheckBox.IsChecked == true) categories.Add("Business");
          if (TechnologyCheckBox.IsChecked == true) categories.Add("Technology");
          if (ScienceCheckBox.IsChecked == true) categories.Add("Science");
          if (SportsCheckBox.IsChecked == true) categories.Add("Sports");
    
          var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);
    
          MessageBox.Show("Subscribed to: " + string.Join(",", categories) + " on registration id : " +
             result.RegistrationId);
        }

    Tento způsob vytvoří seznam kategorií a používá třídu **oznámení** ukládat seznamu v místní úložiště a zaregistrujte příslušné značky v centrální oznámení. Při změně kategorie znovu pomocí nových kategorií vytvoří registrace.

Aplikace je teď možné ukládat sadu kategorií do místní úložiště v zařízení a zaregistrovat v centru oznámení pokaždé, když uživatel změní výběru kategorie.

##<a name="register-for-notifications"></a>Registrace k oznámení

Tento postup zaregistrujte v centru oznámení při spuštění pomocí kategorií, které byly uložené v místním úložišti.

> [AZURE.NOTE] Protože kanálu URI přiřazené tak, že Microsoft nabízená oznámení služby (MPNS) můžete kdykoli změnit, byste měli zaregistrovat oznámení často chcete-li předejít oznámení o selhání. V tomto příkladu zaregistruje pro oznámení o každém spuštění aplikace. K aplikacím, které často se mají spustit více než jednou za den, můžete pravděpodobně přeskočit registrace Chcete-li zachovat šířky pásma, pokud od předchozího registrace uplynul menší než jeden den.


1. Otevřete soubor App.xaml.cs a přidání modifikátorem **asynchronní** metodě **Application_Launching** a nahraďte registračního kódu oznámení o rozbočovače, které jste přidali do [začít pracovat s rozbočovače oznámení] s kódem následující:

        private async void Application_Launching(object sender, LaunchingEventArgs e)
        {
            var result = await notifications.SubscribeToCategories();

            if (result != null)
                System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
                {
                    MessageBox.Show("Registration Id :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
                });
        }

    Zajistíte tím, že při každém spuštění aplikace načte kategorií z místního úložiště a žádosti registrace pro těchto kategorií.

2. V souboru projektu MainPage.xaml.cs přidáte následující kód, který používá způsob **OnNavigatedTo** :

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var categories = ((App)Application.Current).notifications.RetrieveCategories();

            if (categories.Contains("World")) WorldCheckBox.IsChecked = true;
            if (categories.Contains("Politics")) PoliticsCheckBox.IsChecked = true;
            if (categories.Contains("Business")) BusinessCheckBox.IsChecked = true;
            if (categories.Contains("Technology")) TechnologyCheckBox.IsChecked = true;
            if (categories.Contains("Science")) ScienceCheckBox.IsChecked = true;
            if (categories.Contains("Sports")) SportsCheckBox.IsChecked = true;
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

    ![][2]

3. Po přijetí potvrzení, že vaše kategorie byly předplatné dokončení, spusťte aplikaci konzoly o neodeslání oznámení pro jednotlivé kategorie. Ověřte, zda že pouze dostanou oznámení pro kategorií, které jste přihlášení k odběru.

    ![][3]

Jste dokončili v tomto tématu.

<!--##Next steps

In this tutorial we learned how to broadcast breaking news by category. Consider completing one of the following tutorials that highlight other advanced Notification Hubs scenarios:

+ [Use Notification Hubs to broadcast localized breaking news]

    Learn how to expand the breaking news app to enable sending localized notifications.

+ [Notify users with Notification Hubs]

    Learn how to push notifications to specific authenticated users. This is a good solution for sending notifications only to specific users.
-->

<!-- Anchors. -->
[Add category selection to the app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run the app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-breakingnews.png
[2]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-registration.png
[3]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-toast.png



<!-- URLs.-->
[Začínáme s rozbočovače oznámení]: /manage/services/notification-hubs/get-started-notification-hubs-wp8/
[Use Notification Hubs to broadcast localized breaking news]: ../breakingnews-localized-wp8.md
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users/
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Phone]: ??

