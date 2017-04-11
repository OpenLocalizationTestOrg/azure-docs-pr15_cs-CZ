<properties
    pageTitle="Začínáme s ověřováním mobilních aplikací ve Xamarin Android"
    description="Informace o používání mobilní aplikace pro ověřování uživatelů aplikace Xamarin Android celou řadu Zprostředkovatelé identit jiní, včetně AAD Google, Facebooku, Twitteru a Microsoft."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-xamarinandroid-app"></a>Přidání ověřování aplikace Xamarin.Android

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

V tomto tématu se dozvíte, jak pro ověřování uživatelů v mobilní aplikaci z vaší klientské aplikace. V tomto kurzu přidáte ověřování rychlý úvod projektu pomocí poskytovatele identit, který je podporován aplikací Mobile Azure. Po je úspěšně ověřené a oprávnění v mobilní aplikaci, zobrazí se hodnotu ID uživatele.

Tento kurz se podle rychlý úvod mobilní aplikaci. Taky nejdřív budete muset kurz [Vytvoření Xamarin.Android aplikace]. Pokud nepoužíváte project serveru stažený úvodní, je nutné přidat balíček rozšíření ověřování do projektu. Další informace o serveru rozšíření balíčků najdete v tématu [práce s back-end serveru .NET SDK Azure mobilních aplikací](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

##<a name="register"></a>Registrace aplikace pro ověřování a nakonfigurovat aplikaci služby

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Omezení oprávnění pro ověřeného uživatele

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Ve Visual Studiu nebo Xamarin Studio spusťte na zařízení nebo emulátoru projektu klienta. Ověřte, že je po spuštění aplikace aktivována neošetřené výjimce s stavový kód 401 (Neautorizováno). K tomu dojde, protože aplikace pokusí o přístup k mobilní aplikaci backendovou jako neověřené uživatele. V tabulce *TodoItem* teď vyžaduje ověření.

Pak aktualizujete klientské aplikace na žádost o zdroje v mobilní aplikaci back-end s ověřeného uživatele.

##<a name="add-authentication"></a>Přidání ověřování do aplikace

Aplikace se aktualizuje mají uživatelé klepněte na tlačítko **přihlásit** a ověření před zobrazí data.

1. Přidejte následující kód třídy **TodoActivity** :

        // Define a authenticated user.
        private MobileServiceUser user;
        private async Task<bool> Authenticate()
        {
                var success = false;
                try
                {
                    // Sign in with Facebook login using a server-managed flow.
                    user = await client.LoginAsync(this,
                        MobileServiceAuthenticationProvider.Facebook);
                    CreateAndShowDialog(string.Format("you are now logged in - {0}",
                        user.UserId), "Logged in!");

                    success = true;
                }
                catch (Exception ex)
                {
                    CreateAndShowDialog(ex, "Authentication failed");
                }
                return success;
        }

        [Java.Interop.Export()]
        public async void LoginUser(View view)
        {
            // Load data only after authentication succeeds.
            if (await Authenticate())
            {
                //Hide the button after authentication succeeds.
                FindViewById<Button>(Resource.Id.buttonLoginUser).Visibility = ViewStates.Gone;

                // Load the data.
                OnRefreshItemsSelected();
            }
        }

    Tím vytvoříte nové metody ověřování uživatele a metoda rutinu pro nové tlačítko **přihlásit** . Uživatel v kód příklad ověřen pomocí přihlášení Facebook. Dialogovém okně slouží k zobrazení ID uživatele po ověření.

    > [AZURE.NOTE] Pokud používáte zprostředkovatelem identit než Facebook, změňte hodnotu předán **LoginAsync** nad na jeden z těchto věcí: _MicrosoftAccount_, _Twitter_, _Google_nebo _WindowsAzureActiveDirectory_.

3. Metoda **OnCreate** odstranit nebo komentář a rezervovat na následující řádek kódu:

        OnRefreshItemsSelected ();

4. V souboru Activity_To_Do.axml přidejte následující definici tlačítko *LoginUser* před existující tlačítko *AddItem* :

        <Button
            android:id="@+id/buttonLoginUser"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="LoginUser"
            android:text="@string/login_button_text" />

5. Přidejte následující prvek k souboru Strings.xml zdroje:

        <string name="login_button_text">Sign in</string>

6. Ve Visual Studiu nebo Xamarin Studio projekt klienta spustit na zařízení nebo emulátor a přihlaste se pomocí svého poskytovatele zvolené identity.

    Po úspěšném přihlášení k lyncu se změnami, aplikace se zobrazí přihlašovací ID a seznamu položek úkol a uděláte aktualizace k datům.


<!-- URLs. -->
[Vytvoření aplikace Xamarin.Android]: app-service-mobile-xamarin-android-get-started.md
