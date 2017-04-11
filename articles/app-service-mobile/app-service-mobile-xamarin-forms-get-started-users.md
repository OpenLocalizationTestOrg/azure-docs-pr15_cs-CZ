<properties
    pageTitle="Začínáme s ověřování pro mobilní aplikace v aplikaci Xamarin.Forms | Microsoft Azure"
    description="Informace o používání mobilní aplikace pro ověřování uživatelů Xamarin formulářů aplikace pomocí různých Zprostředkovatelé identit jiní, včetně AAD Google, Facebooku, Twitteru a Microsoft."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-xamarinforms-app"></a>Přidání ověřování aplikace Xamarin.Forms

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

##<a name="overview"></a>Základní informace

V tomto tématu se dozvíte, jak pro ověřování uživatelů aplikace služby mobilní aplikaci z vaší klientské aplikace. V tomto kurzu přidáte ověřování Xamarin.Forms rychlý úvod projektu pomocí zprostředkovatelem identit podporovaný aplikaci služby. Až se úspěšně ověřené a povolené mobilní aplikaci, zobrazí se hodnotu ID uživatele a bude mít přístup k datům s omezeným přístupem tabulky.

##<a name="prerequisites"></a>Zjistit předpoklady pro

Nejlepších výsledků pomocí tohoto kurzu doporučujeme nejdřív dokončení kurz [Vytvoření Xamarin.Forms aplikace](app-service-mobile-xamarin-forms-get-started.md) . Po dokončení tohoto kurzu, budete mít Xamarin.Forms projektu, který je v aplikaci seznam úkolů více.

Pokud nepoužíváte project serveru stažený úvodní, je nutné přidat balíček rozšíření ověřování do projektu. Další informace o serveru rozšíření balíčků najdete v tématu [práce s back-end serveru .NET SDK Azure mobilních aplikací](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

##<a name="register-your-app-for-authentication-and-configure-app-services"></a>Registrace aplikace pro ověřování a nakonfigurovat aplikaci služby

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="restrict-permissions-to-authenticated-users"></a>Omezení oprávnění pro ověřeného uživatele

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]


##<a name="add-authentication-to-the-portable-class-library"></a>Přidání ověřování ke knihovně přenosných tříd

Mobilní aplikace bude použit metodu rozšíření [LoginAsync] [MobileServiceClient] k přihlášení uživatele pomocí ověřování aplikace služby. V tomto příkladu tok ověřování serveru Správa přístupových práv, která se zobrazí na poskytovatele přihlašovací rozhraní v aplikaci. Další informace najdete v tématu [ověřování serveru Správa přístupových práv](app-service-mobile-dotnet-how-to-use-client-library.md#serverflow). Poskytnout lepší uživatele v aplikaci výrobní, zvažte místo pomocí [klienta Správa přístupových práv ověřování](app-service-mobile-dotnet-how-to-use-client-library.md#clientflow). 

Ověření sebou máte projekt Xamarin.Forms, definujete rozhraní **IAuthenticate** v přenosném knihovna tříd aplikace. Můžete taky aktualizovat uživatelské rozhraní podle přenosném knihovna tříd přidejte tlačítko **přihlásit** , které kliknutí na tlačítko spustit ověřování. Po úspěšném ověření načtení dat z back-end mobilní aplikace.

Je nutné implementovat rozhraní **IAuthenticate** pro každou platformu nepodporuje aplikace.


1. Ve Visual Studiu nebo Xamarin Studio otevřít App.cs z projektu s **Portable** v názvu, který je přenosný knihovna tříd projektu, přidejte následující `using` údajů:

        using System.Threading.Tasks;

2. V App.cs, přidejte následující text `IAuthenticate` rozhraní definice těsně před `App` definici třídy.

        public interface IAuthenticate
        {
            Task<bool> Authenticate();
        }

3. Přidejte následující statické členy třídy **aplikace** inicializace rozhraní s konkrétní implementaci platformu.

        public static IAuthenticate Authenticator { get; private set; }

        public static void Init(IAuthenticate authenticator)
        {
            Authenticator = authenticator;
        }

4. Otevřete TodoList.xaml z Portable knihovna tříd projektu, přidejte následující **tlačítko** prvek v rozložení elementu *buttonsPanel* po existující tlačítko: 

        <Button x:Name="loginButton" Text="Sign-in" MinimumHeightRequest="30" 
            Clicked="loginButton_Clicked"/>

    Kliknutím na toto tlačítko spustí serveru Správa přístupových práv ověřování pomocí mobilní aplikace backendovou.

5. Otevřete TodoList.xaml.cs z přenosném knihovna tříd projektu a potom na Přidat na následující pole `TodoList` třídy:

        // Track whether the user has authenticated. 
        bool authenticated = false;


6. Nahraďte metodu **OnAppearing** tento kód:

        protected override async void OnAppearing()
        {
            base.OnAppearing();
    
            // Refresh items only when authenticated.
            if (authenticated == true)
            {
                // Set syncItems to true in order to synchronize the data 
                // on startup when running in offline mode.
                await RefreshItems(true, syncItems: false);

                // Hide the Sign-in button.
                this.loginButton.IsVisible = false;
            }
        }

    Zajistíte tím, že data jenom aktualizaci ze služby po ověření uživatele.

7. Přidejte následující rutinu pro událost **Clicked** třídy **seznamu úkolů** :

        async void loginButton_Clicked(object sender, EventArgs e)
        {
            if (App.Authenticator != null)
                authenticated = await App.Authenticator.Authenticate();

            // Set syncItems to true to synchronize the data on startup when offline is enabled.
            if (authenticated == true)
                await RefreshItems(true, syncItems: false);
        }

8. Uložte provedené změny a znovu vytvořit projekt přenosný knihovna tříd ověření bez chyb.


##<a name="add-authentication-to-the-android-app"></a>Přidání ověřování aplikace v Androidu

Tato část popisuje implementace rozhraní **IAuthenticate** v projektu aplikace Androidu. Tuto část přeskočte, pokud nejsou podporovány zařízení s Androidem.

1. Ve Visual Studiu nebo Xamarin Studio klikněte pravým tlačítkem **droid** projekt, pak **nastavit jako spouštěný projekt**.

2. Stisknutím klávesy F5 projekt ladění a pak zkontrolujte, že je po spuštění aplikace aktivována neošetřené výjimce s stavový kód 401 (Neautorizováno). K tomu dojde, protože se access na back-end oprávněnými pouze na uživatele s omezeným přístupem.

3. Otevřete MainActivity.cs Android projektu a přidejte následující `using` příkazy:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;

4. Aktualizace třídy **MainActivity** implementovat rozhraní **IAuthenticate** následujícím způsobem:

        public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity, IAuthenticate


5. Třídy **MainActivity** můžete aktualizujte přidáním pole **MobileServiceUser** a metodu **Ověřit** , který je potřeba rozhraní **IAuthenticate** následujícím způsobem:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(this,
                    MobileServiceAuthenticationProvider.Facebook);
                if (user != null)
                {
                    message = string.Format("you are now signed-in as {0}.",
                        user.UserId);
                    success = true;
                }
            }
            catch (Exception ex)
            {
                message = ex.Message;
            }

            // Display the success or failure message.
            AlertDialog.Builder builder = new AlertDialog.Builder(this);
            builder.SetMessage(message);
            builder.SetTitle("Sign-in result");
            builder.Create().Show();

            return success;
        }


    Pokud používáte zprostředkovatelem identit než Facebook, zvolte jinou hodnotu pro [MobileServiceAuthenticationProvider].

6. Přidejte následující kód metody **OnCreate** třídy **MainActivity** před voláním `LoadApplication()`:

        // Initialize the authenticator before loading the app.
        App.Init((IAuthenticate)this);

    Zajistíte tím, že je ověřovacích dat inicializován před zatížení aplikace.

7. Znovu vytvořit aplikaci, ho spusťte a přihlaste se pomocí zprostředkovatele ověřování zvolili a ověřte, jestli že je možné získat přístup k datům jako ověřený uživatel.

##<a name="add-authentication-to-the-ios-app"></a>Přidání ověřování aplikace IOS

Tato část popisuje implementaci rozhraní **IAuthenticate** v aplikaci project iOS. Tuto část přeskočte, pokud nejsou podporovány zařízení s iOS.

1. Ve Visual Studiu nebo Xamarin Studio klikněte pravým tlačítkem myši **iOS** projektu, pak **nastavit jako po spuštění projektu**.

2. Stisknutím klávesy F5 projekt ladění a pak zkontrolujte, že neošetřené výjimce s stavový kód 401 (Neautorizováno) mocninu po spuštění aplikace. K tomu dojde, protože se access na back-end oprávněnými pouze na uživatele s omezeným přístupem.

4. Otevřete AppDelegate.cs v iOS projektu a přidejte následující `using` příkazy:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;

4. Aktualizace třídy **AppDelegate** implementovat rozhraní **IAuthenticate** následujícím způsobem:

        public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate, IAuthenticate

5. Třídy **AppDelegate** můžete aktualizujte přidáním pole **MobileServiceUser** a metodu **Ověřit** , který je potřeba rozhraní **IAuthenticate** následujícím způsobem:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(UIApplication.SharedApplication.KeyWindow.RootViewController,
                        MobileServiceAuthenticationProvider.Facebook);
                    if (user != null)
                    {
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                        success = true;                        
                    }
                }        
            }
            catch (Exception ex)
            {
               message = ex.Message;
            }

            // Display the success or failure message.
            UIAlertView avAlert = new UIAlertView("Sign-in result", message, null, "OK", null);
            avAlert.Show();         

            return success;
        }

    Pokud používáte zprostředkovatelem identit než Facebook, zvolte jinou hodnotu pro [MobileServiceAuthenticationProvider].

6. Přidejte následující řádek kód metodu **FinishedLaunching** před voláním `LoadApplication()`: 

        App.Init(this);

    Zajistíte tím, že je ověřovacích dat inicializován před načtením aplikace.

7. Znovu vytvořit aplikaci, ho spusťte a přihlaste se pomocí zprostředkovatele ověřování zvolili a ověřte, jestli že je možné získat přístup k datům jako ověřený uživatel.


##<a name="add-authentication-to-windows-app-projects"></a>Přidání ověřování Windows aplikace projekty

Tato část popisuje implementace rozhraní **IAuthenticate** ve Windows 8.1 a Windows Phone 8.1 aplikace projekty. Stejný postup platí pro projekty univerzální platformu systému Windows (UWP). Tuto část přeskočte, pokud nejsou podporovány zařízení s Windows.

1. Ve Visual Studiu klikněte pravým tlačítkem myši **WinApp** nebo **WinPhone81** projektu, pak **nastavit jako po spuštění projektu**.

2. Stisknutím klávesy F5 projekt ladění a pak zkontrolujte, že je po spuštění aplikace aktivována neošetřené výjimce s stavový kód 401 (Neautorizováno). K tomu dojde, protože se access na back-end oprávněnými pouze na uživatele s omezeným přístupem.

3. Otevřete MainPage.xaml.cs pro projekt aplikace Windows a přidejte následující `using` příkazy:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.UI.Popups;
        using <your_Portable_Class_Library_namespace>;

    Nahrazení `<your_Portable_Class_Library_namespace>` s obor názvů pro váš Přenosná knihovna tříd.

4. Aktualizace třídy **MainPage** implementovat rozhraní **IAuthenticate** následujícím způsobem:

        public sealed partial class MainPage : IAuthenticate


5. Třídy **MainPage** můžete aktualizujte přidáním pole **MobileServiceUser** a metodu **Ověřit** , který je potřeba rozhraní **IAuthenticate** následujícím způsobem:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            string message = string.Empty;
            var success = false;

            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                    if (user != null)
                    {
                        success = true;
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                    }
                }
                
            }
            catch (Exception ex)
            {
                message = string.Format("Authentication Failed: {0}", ex.Message);
            }

            // Display the success or failure message.
            await new MessageDialog(message, "Sign-in result").ShowAsync();

            return success;
        }


    Pokud používáte zprostředkovatelem identit než Facebook, zvolte jinou hodnotu pro [MobileServiceAuthenticationProvider].

6. Přidejte následující řádek kódu v konstruktoru předmětu **MainPage** před voláním `LoadApplication()`:

        // Initialize the authenticator before loading the app.
        <your_Portable_Class_Library_namespace>.App.Init(this);
 
    Nahrazení `<your_Portable_Class_Library_namespace>` s obor názvů pro knihovny přenosném tříd.  
    Pokud je to WinApp projektu, můžete přejít dolů kroku 8. Dalším krokem platí jenom pro WinPhone81 projektu, kdy je potřeba dokončit zpětné přihlášení.

7. (Volitelné) V aplikaci project **WinPhone81** , otevřete App.xaml.cs a přidejte následující `using` příkazy:

        using Microsoft.WindowsAzure.MobileServices;
        using <your_Portable_Class_Library_namespace>;

    Nahrazení `<your_Portable_Class_Library_namespace>` s obor názvů pro knihovny přenosném tříd.

8.  Přidejte následující přepsání metody **OnActivated** třídy **aplikace** :

        protected override void OnActivated(IActivatedEventArgs args)
        {
            base.OnActivated(args);

            // We just need to handle activation that occurs after web authentication. 
            if (args.Kind == ActivationKind.WebAuthenticationBrokerContinuation)
            {
                // Get the client and call the LoginComplete method to complete authentication.
                var client = TodoItemManager.DefaultManager.CurrentClient as MobileServiceClient;
                client.LoginComplete(args as WebAuthenticationBrokerContinuationEventArgs);
            }
        }

    Při přepsání metody už existuje, jednoduše přidáte kód podmíněné z výše uvedený fragment kódu.

7. Znovu vytvořit aplikaci, ho spusťte a přihlaste se pomocí zprostředkovatele ověřování zvolili a ověřte, jestli že je možné získat přístup k datům jako ověřený uživatel.

##<a name="next-steps"></a>Další kroky

Teď, když jste dokončili tento kurz základní ověřování, vezměte v úvahu pokračováním k jedné z následujících výukové programy pro:

+ [Přidání nabízených oznámení pro aplikace](app-service-mobile-xamarin-forms-get-started-push.md)  
  Naučte se přidávat nabízená oznámení podpora k aplikaci a konfigurace back-end vaše mobilní aplikaci pro použití Azure oznámení rozbočovače k odeslání nabízená oznámení.

+ [Povolení offline synchronizace aplikace](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Naučte se přidávat aplikace pomocí mobilní aplikace backendovou podpora offline režimu. Offline synchronizace umožňuje koncovým uživatelům chcete provést interakci s mobilní aplikaci&mdash;zobrazení, přidání nebo úprava dat&mdash;i v případě žádné síťové připojení.

<!-- Images. -->

<!-- URLs. -->
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333
[LoginAsync]: https://msdn.microsoft.com/library/azure/dn268341(v=azure.10).aspx
[MobileServiceClient]: https://msdn.microsoft.com/library/azure/JJ553674(v=azure.10).aspx
[MobileServiceAuthenticationProvider]: https://msdn.microsoft.com/library/azure/jj730936(v=azure.10).aspx

