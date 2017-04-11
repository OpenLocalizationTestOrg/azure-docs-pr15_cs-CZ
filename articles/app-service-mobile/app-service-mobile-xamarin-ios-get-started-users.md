<properties
    pageTitle="Začínáme s ověřování pro mobilní aplikace v Xamarin iOS"
    description="Informace o používání mobilní aplikace pro ověřování uživatelů aplikace pro iOS Xamarin celou řadu Zprostředkovatelé identit jiní, včetně AAD Google, Facebooku, Twitteru a Microsoft."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-xamarinios-app"></a>Přidání ověřování aplikace Xamarin.iOS

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

V tomto tématu se dozvíte, jak pro ověřování uživatelů aplikace služby mobilní aplikaci z vaší klientské aplikace. V tomto kurzu přidáte ověřování Xamarin.iOS rychlý úvod projektu pomocí zprostředkovatelem identit podporovaný aplikaci služby. Až se úspěšně ověřené a povolené mobilní aplikaci, zobrazí se hodnotu ID uživatele a bude mít přístup k datům s omezeným přístupem tabulky.

Nejdřív musíte nejdřív udělat kurz [Vytvoření Xamarin.iOS aplikace]. Pokud nepoužíváte project serveru stažený úvodní, je nutné přidat balíček rozšíření ověřování do projektu. Další informace o serveru rozšíření balíčků najdete v tématu [práce s back-end serveru .NET SDK Azure mobilních aplikací](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

##<a name="register-your-app-for-authentication-and-configure-app-services"></a>Registrace aplikace pro ověřování a nakonfigurovat aplikaci služby

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="restrict-permissions-to-authenticated-users"></a>Omezení oprávnění pro ověřeného uživatele

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

&nbsp;&nbsp;4.v Visual Studio nebo Xamarin Studio spusťte na zařízení nebo emulátoru projektu klienta. Ověřte, že je po spuštění aplikace aktivována neošetřené výjimce s stavový kód 401 (Neautorizováno). Selhání zaznamenané v konzole ladění. Takže ve Visual Studiu, měli byste vidět Chyba instalace v okně výstupu.

&nbsp;&nbsp;Tato chyba neoprávněným způsobeno aplikace pokusí o přístup k mobilní aplikaci backendovou jako neověřené uživatele. V tabulce *TodoItem* teď vyžaduje ověření.

Pak aktualizujete klientské aplikace na žádost o zdroje v mobilní aplikaci back-end s ověřeného uživatele.

##<a name="add-authentication-to-the-app"></a>Přidání ověřování do aplikace

V této části upravíte aplikace zobrazí přihlašovací obrazovka před zobrazením data. Po spuštění aplikace nebudete připojení k aplikaci služby a nezobrazí žádná data. Po prvním uživatel provede gesto aktualizace, zobrazí se přihlašovací obrazovka; Po úspěšném přihlášení se zobrazí seznam položek úkol.

1. V aplikaci project klienta, otevřete soubor **QSTodoService.cs** a přidejte následující pomocí příkazu a `MobileServiceUser` s přístupové třídy QSTodoService:

    ```
        using UIKit;
    ```

        // Logged in user
        private MobileServiceUser user;
        public MobileServiceUser User { get { return user; } }

2. Přidání nového způsobu s názvem **Ověřit** **QSTodoService** s definicí následující:


        public async Task Authenticate(UIViewController view)
        {
            try
            {
                user = await client.LoginAsync(view, MobileServiceAuthenticationProvider.Facebook);
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine (@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
            }
        }

    >[AZURE.NOTE] Pokud používáte zprostředkovatelem identit než Facebooku, změňte hodnotu předán **LoginAsync** nad na jeden z těchto věcí: _MicrosoftAccount_, _Twitter_, _Google_nebo _WindowsAzureActiveDirectory_.

3. Otevřete **QSTodoListViewController.cs**. Úprava definice metody **ViewDidLoad** odebrání volání **RefreshAsync()** těsně před koncem:

        public override async void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            todoService = QSTodoService.DefaultService;
           await todoService.InitializeStoreAsync ();

           RefreshControl.ValueChanged += async (sender, e) => {
                await RefreshAsync ();
           }

            // Comment out the call to RefreshAsync
            // await RefreshAsync ();
        }


4. Upravte metodu **RefreshAsync** k ověření, pokud je vlastnost **uživatele** null. V horní části definice metody přidáte následující kód:

        // start of RefreshAsync method
        if (todoService.User == null) {
            await QSTodoService.DefaultService.Authenticate (this);
            if (todoService.User == null) {
                Console.WriteLine ("couldn't login!!");
                return;
            }
        }
        // rest of RefreshAsync method

5. Ve Visual Studiu nebo Xamarin Studio připojení k hostitele sestavit Xamarin na počítači Mac, spusťte projekt klienta zaměření zařízení nebo emulátoru. Zkontrolujte, že aplikace objevil žádná data.

    Proveďte aktualizaci gesto roztažením dolů seznam položek, které bude mít za následek přihlašovací obrazovky zobrazit. Po úspěšném jste zadali platných přihlašovacích údajů, aplikace se zobrazí seznam položek úkol a uděláte aktualizace k datům.


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Vytvoření aplikace Xamarin.iOS]: app-service-mobile-xamarin-ios-get-started.md
