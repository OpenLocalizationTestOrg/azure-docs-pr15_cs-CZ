
1. Otevření souboru sdíleného projektu MainPage.cs a třídy MainPage přidat následující fragment kódu:
    
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

3. Vymazání komentáře nebo odstranění volání metody **RefreshTodoItems** v existující přepsání metody **OnNavigatedTo** .

    Tento údaj zabrání načtení před ověření uživatele. Pak přidá tlačítko **přihlásit** k aplikaci, která aktivuje ověřování.

4. Přidejte následující fragment kódu třídy MainPage:

        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login the user and then load data from the mobile app.
            if (await AuthenticateAsync())
            {
                // Hide the login button and load items from the mobile app.
                ButtonLogin.Visibility = Windows.UI.Xaml.Visibility.Collapsed;
                //await InitLocalStoreAsync(); //offline sync support.
                await RefreshTodoItems();
            }
        }
        
5. V aplikaci project pro Windows Store otevřete soubor projektu MainPage.xaml a přidejte následující prvek **tlačítko** těsně před prvek, který definuje tlačítka **Uložit** :

        <Button Name="ButtonLogin" Click="ButtonLogin_Click" 
                        Visibility="Visible">Sign in</Button>

6. V aplikaci project Windows Phone Storu aplikaci přidejte následující prvek **tlačítko** v **ContentPanel**po prvku **textové pole** :

        <Button Grid.Row ="1" Grid.Column="1" Name="ButtonLogin" Click="ButtonLogin_Click" 
            Margin="10, 0, 0, 0" Visibility="Visible">Sign in</Button>

8. Otevřete sdílený soubor App.xaml.cs projektu a přidat následující kód:

        protected override void OnActivated(IActivatedEventArgs args)
        {
            // Windows Phone 8.1 requires you to handle the respose from the WebAuthenticationBroker.
            #if WINDOWS_PHONE_APP
            if (args.Kind == ActivationKind.WebAuthenticationBrokerContinuation)
            {
                // Completes the sign-in process started by LoginAsync.
                // Change 'MobileService' to the name of your MobileServiceClient instance. 
                App.MobileService.LoginComplete(args as WebAuthenticationBrokerContinuationEventArgs);
            }
            #endif

            base.OnActivated(args);
        }

    Pokud metodu **OnActivated** už existuje, přidejte `#if...#endif` blok kódu.

9. Stisknutím klávesy F5 spuštění aplikace pro Windows Store, klikněte na tlačítko **přihlásit** a přihlaste se k aplikaci u poskytovatele zvolené identity. 

    Po úspěšném přihlášení k lyncu se změnami, aplikace by měla běžet bez chyb a by měla dotazu vaše back-end a aktualizace dat.

10. Klikněte pravým tlačítkem na Windows Phone Storu aplikaci project, klikněte na **nastavit jako projekt při spuštění**a pak opakujte předchozí krok a ověřte, zda úložišti Windows Phone aplikace spuštěné jsou správně.  

 