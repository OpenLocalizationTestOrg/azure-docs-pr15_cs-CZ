<properties
    pageTitle="Přidání ověřování aplikace univerzální platformu systému Windows (UWP) | Azure mobilní aplikace"
    description="Naučte se používat Azure aplikace služby mobilní aplikace pro ověřování uživatelů pomocí různých Zprostředkovatelé identit jiní, včetně aplikace univerzální platformu systému Windows (UWP): AAD Google, Facebooku, Twitteru a Microsoft."
    services="app-service\mobile"
    documentationCenter="windows"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-windows-app"></a>Přidání ověřování aplikace pro Windows

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

V tomto tématu se dozvíte, jak přidat cloudové ověřování do mobilní aplikace. V tomto kurzu přidáte ověřování projektu rychlý úvod univerzální platformu systému Windows (UWP) mobilních aplikací pomocí poskytovatele identit podporovaný aplikaci služby Azure. Po je úspěšně ověřené a povolené backendovou mobilní aplikaci, zobrazí se hodnotu ID uživatele.

Tento kurz se podle rychlý úvod mobilní aplikace. Nejdřív musíte nejdřív udělat kurz [Seznámení s aplikací Mobile](app-service-mobile-windows-store-dotnet-get-started.md).

##<a name="register"></a>Registrace aplikace pro ověřování a konfigurace aplikace služby

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Omezení oprávnění pro ověřeného uživatele

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Teď můžete ověřit, že byl zakázán anonymní přístup k vaší back-end. S aplikací project UWP nastavit jako po spuštění projektu, nasazení a spustit aplikaci, Ověřte, že je po spuštění aplikace aktivována neošetřené výjimce s stavový kód 401 (Neautorizováno). K tomu dojde, protože aplikace pokusí o přístup k vašemu mobilní aplikace kódu jako neověřené uživatele, ale tabulce *TodoItem* teď vyžaduje ověření.

Dále se aktualizovat aplikaci k ověření uživatele před žádosti o zdrojů z aplikace služby.

##<a name="add-authentication"></a>Přidání ověřování do aplikace

1. V UWP aplikace project soubor MainPage.cs a třídy MainPage přidat následující fragment kódu:
    
        // Define a member variable for storing the signed-in user. 
        private MobileServiceUser user;

        // Define a method that performs the authentication process
        // using a Facebook sign-in. 
        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;
            try
            {
                // Change 'MobileService' to the name of your MobileServiceClient instance.
                // Sign-in using Facebook authentication.
                user = await App.MobileService
                    .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                message =
                    string.Format("You are now signed in - {0}", user.UserId);

                success = true;
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }

            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
            return success;
        }

    Tento kód ověřuje uživatele s přihlášením Facebook. Pokud používáte zprostředkovatelem identit než Facebook, změňte hodnotu **MobileServiceAuthenticationProvider** nad na hodnotu pro vašeho poskytovatele.

3. Vymazání komentáře nebo odstranění volání **ButtonRefresh_Click** metodu (nebo **InitLocalStoreAsync** ) v existující přepsání metody **OnNavigatedTo** . Tento údaj zabrání načtení před ověření uživatele. Pak přidá tlačítko **přihlásit** k aplikaci, která aktivuje ověřování.

4. Přidejte následující fragment kódu třídy MainPage:

        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login the user and then load data from the mobile app.
            if (await AuthenticateAsync())
            {
                // Switch the buttons and load items from the mobile app.
                ButtonLogin.Visibility = Visibility.Collapsed;
                ButtonSave.Visibility = Visibility.Visible;
                //await InitLocalStoreAsync(); //offline sync support.
                await RefreshTodoItems();
            }
        }
        
5. Otevření souboru projektu MainPage.xaml, vyhledejte prvek, který definuje tlačítka **Uložit** a nahradit je tento kód:

        <Button Name="ButtonSave" Visibility="Collapsed" Margin="0,8,8,0" 
                Click="ButtonSave_Click">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Add"/>
                <TextBlock Margin="5">Save</TextBlock>
            </StackPanel>
        </Button>
        <Button Name="ButtonLogin" Visibility="Visible" Margin="0,8,8,0" 
                Click="ButtonLogin_Click" TabIndex="0">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Permissions"/>
                <TextBlock Margin="5">Sign in</TextBlock> 
            </StackPanel>
        </Button>

9. Stisknutím klávesy F5 spustit aplikaci, klikněte na tlačítko **přihlásit** a přihlaste se k aplikaci u poskytovatele zvolené identity. Jakmile je vaše přihlašovací úspěšné, aplikace spustí bez chyb a budete moci dotazu vaše back-end a provést aktualizace dat.


##<a name="tokens"></a>Uložení ověřovací token na straně klienta

V předchozím příkladu ukázal standardní přihlásit, který vyžaduje klienta kontaktovat zprostředkovatele identit a služba aplikace při každém spuštění aplikace. Nejen je tato metoda, můžete spustit do použití týká problémy má mnoho zákazníků pokusu o spuštění aplikace ve stejnou dobu. Lepší přístup je mezipaměti tokenu se tak mohli ověřovat vrácené funkcí aplikace služby a pokusíte použít první před použitím základě poskytovatele přihlášení.

>[AZURE.NOTE]Můžete mezipaměti token webová aplikace služby bez ohledu na to, jestli používáte klienta Správa přístupových práv nebo služby Správa přístupových práv ověřování. Tento kurz používá ověřování služby Správa přístupových práv.

[AZURE.INCLUDE [mobile-windows-universal-dotnet-authenticate-app-with-token](../../includes/mobile-windows-universal-dotnet-authenticate-app-with-token.md)]

##<a name="next-steps"></a>Další kroky

Teď, když jste dokončili tento kurz základní ověřování, vezměte v úvahu pokračováním k jedné z následujících výukové programy pro:

+ [Přidání nabízená oznámení pro aplikace](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  Naučte se přidávat nabízená oznámení podpora k aplikaci a konfigurace back-end vaše mobilní aplikaci pro použití Azure oznámení rozbočovače k odeslání nabízená oznámení.

+ [Povolení offline synchronizace aplikace](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Naučte se přidávat aplikace pomocí mobilní aplikace backendovou podpora offline režimu. Offline synchronizace umožňuje koncovým uživatelům chcete provést interakci s mobilní aplikaci&mdash;zobrazení, přidání nebo úprava dat&mdash;i v případě žádné síťové připojení.


<!-- URLs. -->
[Get started with your mobile app]: app-service-mobile-windows-store-dotnet-get-started.md

