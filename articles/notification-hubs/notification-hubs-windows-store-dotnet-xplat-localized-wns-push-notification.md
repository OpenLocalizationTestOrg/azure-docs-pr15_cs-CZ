<properties
    pageTitle="Oznámení o rozbočovače lokalizované kurz nejnovějších příspěvků"
    description="Naučte se používat rozbočovače Azure oznámení o neodeslání oznámení příspěvky lokalizované jazycích."
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

# <a name="use-notification-hubs-to-send-localized-breaking-news"></a>Odeslání lokalizované novinky pomocí rozbočovače oznámení

> [AZURE.SELECTOR]
- [Pro Windows Store C#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
- [iOS](notification-hubs-ios-xplat-localized-apns-push-notification.md)

##<a name="overview"></a>Základní informace

V tomto tématu se dozvíte, jak používat funkci **šablony** Azure oznámení rozbočovače vysílání jazycích příspěvky upozornění, která byla lokalizované jazyk a zařízení. V tomto kurzu spuštění se aplikace pro Windows Store vytvořené v [Rozbočovače použití oznámení o odeslání novinky]. Až budete hotovi, budete moct zaregistrujte kategorií, které vás zajímají, zadejte jazyk, ve kterém upozornění a přijímat jen nabízených oznámení pro vybrané kategorie v příslušném jazyce.


Tvoří dvě části situaci:

- aplikace pro Windows Store umožňuje klientského zařízení nastavit jazyk a přihlášení k odběru kategorií příspěvků různých jazycích.

- back-end vysílání oznámení pomocí **značek** a **šablon** feautres rozbočovače oznámení Azure.



##<a name="prerequisites"></a>Zjistit předpoklady pro

Musíte již jste dokončili kurz [Používání rozbočovače oznámení o odeslání novinky] a mít kód k dispozici, protože tento kurz vychází přímo z tento kód.

Budete potřebovat Visual Studio 2012 nebo novější.


##<a name="template-concepts"></a>Principy šablony

V [Oznámení rozbočovače použít k odeslání novinky] integrované aplikace, která používá **značky** odběry oznámení pro různé nové kategorie.
Mnoho aplikace však obrázku více trzích a vyžadovat lokalizace. To znamená, že obsah oznámení sami muset lokalizované a doručila ji do správnou sadu zařízení.
V tomto tématu ukážeme, jak můžete pomocí funkce **šablony** rozbočovače oznámení umožňuje snadno poskytovat lokalizované nejnovějších příspěvků oznámení.

Poznámka: jedním ze způsobů pošle lokalizované upozornění, že je vytvořit více verzí každou značku. Například pro podporu angličtinu, francouzštinu a Mandarínština, by potřebujeme třemi jiné značky světě informace: "world_en", "world_fr" a "world_ch". Potom nám odešlete lokalizované verzi novinky světa každý z těchto značky. V tomto tématu používáme šablony šíření značky a požadavku odeslání více zpráv.

Na vysoké úrovni jsou šablony způsob, jak určit, jak mají určitému zařízení dostávat oznámení. Šablona určuje formát přesné datové odkazem vlastnosti, které jsou součástí zprávy aplikace back-end. V našem případě pošleme bez ohledu na národním prostředí zprávu obsahující všechny podporované jazyky:

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Potom jsme zajistí, že u šablony, které odkazuje na správnou vlastnost zaregistrovat zařízení. Například aplikace pro Windows Store, který chce zpráva jednoduché oznámením zaregistruje pro následující šablonu odpovídající značkami:

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">$(News_English)</text>
        </binding>
      </visual>
    </toast>



Šablony jsou velmi výkonných funkcí můžou dozvědět víc o v článku naší [šablony](notification-hubs-templates-cross-platform-push-messages.md) . 


##<a name="the-app-user-interface"></a>Uživatelském rozhraní aplikace

Teď můžeme změní aplikaci novinkách, kterou jste vytvořili v tématu [Použití oznámení rozbočovače odeslat novinky] odeslat lokalizované novinky pomocí šablon.

V aplikaci pro Windows Store:

Změna MainPage.xaml zahrnout pole se seznamem národní prostředí:

    <Grid Margin="120, 58, 120, 80"  
            Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition />
            <ColumnDefinition />
        </Grid.ColumnDefinitions>
        <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top"/>
        <ComboBox Name="Locale" HorizontalAlignment="Left" VerticalAlignment="Center" Width="200" Grid.Row="1" Grid.Column="0">
            <x:String>English</x:String>
            <x:String>French</x:String>
            <x:String>Mandarin</x:String>
        </ComboBox>
        <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="2" Grid.Column="0"/>
        <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="3" Grid.Column="0"/>
        <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="4" Grid.Column="0"/>
        <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="2" Grid.Column="1"/>
        <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="3" Grid.Column="1"/>
        <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="4" Grid.Column="1"/>
        <Button Content="Subscribe" HorizontalAlignment="Center" Grid.Row="5" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
    </Grid>

##<a name="building-the-windows-store-client-app"></a>Vytváření klientské aplikace pro Windows Store

1. Ve svojí třídě oznámení o přidání parametru národní prostředí *StoreCategoriesAndSubscribe* a *SubscribeToCateories* metody.

        public async Task<Registration> StoreCategoriesAndSubscribe(string locale, IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            ApplicationData.Current.LocalSettings.Values["locale"] = locale;
            return await SubscribeToCategories(categories);
        }

        public async Task<Registration> SubscribeToCategories(string locale, IEnumerable<string> categories = null)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            if (categories == null)
            {
                categories = RetrieveCategories();
            }

            // Using a template registration. This makes supporting notifications across other platforms much easier.
            // Using the localized tags based on locale selected.
            string templateBodyWNS = String.Format("<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(News_{0})</text></binding></visual></toast>", locale);

            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "localizedWNSTemplateExample", categories);
        }

    Všimněte si, že místo volání metody *RegisterNativeAsync* jsme volat *RegisterTemplateAsync*: můžeme registrace formátu konkrétní oznámení šabloně závisí na národním prostředí. Také nabízíme název šablony ("localizedWNSTemplateExample"), protože jsme možná budete chtít zaregistrovat více šablon (například jedno pro oznámení informačního) a jedno pro dlaždice a potřebujeme pojmenování provést aktualizaci nebo jejich odstranění.

    Poznámka: Pokud zařízení registruje více šablon stejnou značku, příchozí zprávy zacílení, značky bude mít za následek více oznámení doručení zařízení (jedno pro každé šablony). Toto chování je užitečná, když bude stejná zpráva logické má za následek více vizuálních upozornění, například ukazující Odznáček i oznámením do aplikace pro Windows Store.

2. Přidání podle pokynů k načtení uložené národní prostředí:

        public string RetrieveLocale()
        {
            var locale = (string) ApplicationData.Current.LocalSettings.Values["locale"];
            return locale != null ? locale : "English";
        }

3. Ve vaší MainPage.xaml.cs vaše tlačítko Aktualizovat kliknutím na tlačítko rutiny načtení aktuální hodnoty pole se seznamem národní prostředí a poskytování volání třídu oznámení, viz:

        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var locale = (string)Locale.SelectedItem;

            var categories = new HashSet<string>();
            if (WorldToggle.IsOn) categories.Add("World");
            if (PoliticsToggle.IsOn) categories.Add("Politics");
            if (BusinessToggle.IsOn) categories.Add("Business");
            if (TechnologyToggle.IsOn) categories.Add("Technology");
            if (ScienceToggle.IsOn) categories.Add("Science");
            if (SportsToggle.IsOn) categories.Add("Sports");

            var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(locale,
                 categories);

            var dialog = new MessageDialog("Locale: " + locale + " Subscribed to: " + 
                string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }


4. Nakonec v souboru App.xaml.cs, zkontrolujte, že aktualizace vaší `InitNotificationsAsync` způsob, jak získat národní prostředí a používá při přihlášení k odběru:

        private async void InitNotificationsAsync()
        {
            var result = await notifications.SubscribeToCategories(notifications.RetrieveLocale());

            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }


##<a name="send-localized-notifications-from-your-back-end"></a>Odeslání lokalizované oznámení z back-end

[AZURE.INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]






<!-- Anchors. -->
[Template concepts]: #concepts
[The app user interface]: #ui
[Building the Windows Store client app]: #building-client
[Send notifications from your back-end]: #send
[Next Steps]:#next-steps

<!-- Images. -->

<!-- URLs. -->
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Notify users with Notification Hubs: Mobile Services]: /manage/services/notification-hubs/notify-users
[Odeslání novinky pomocí rozbočovače oznámení]: /manage/services/notification-hubs/breaking-news-dotnet

[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-dotnet
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-dotnet
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-dotnet
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-app-users-dotnet
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-dotnet
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
