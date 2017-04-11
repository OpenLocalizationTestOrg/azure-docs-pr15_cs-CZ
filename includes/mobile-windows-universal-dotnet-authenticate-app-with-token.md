
1. V souboru MainPage.xaml.cs projektu přidejte následující příkazy **pomocí** :

        using System.Linq;      
        using Windows.Security.Credentials;

2. Nahraďte metodu **AuthenticateAsync** tento kód:

        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;

            // This sample uses the Facebook provider.
            var provider = MobileServiceAuthenticationProvider.Facebook;

            // Use the PasswordVault to securely store and access credentials.
            PasswordVault vault = new PasswordVault();
            PasswordCredential credential = null;

            try
            {
                // Try to get an existing credential from the vault.
                credential = vault.FindAllByResource(provider.ToString()).FirstOrDefault();
            }
            catch (Exception)
            {
                // When there is no matching resource an error occurs, which we ignore.
            }

            if (credential != null)
            {
                // Create a user from the stored credentials.
                user = new MobileServiceUser(credential.UserName);
                credential.RetrievePassword();
                user.MobileServiceAuthenticationToken = credential.Password;

                // Set the user from the stored credentials.
                App.MobileService.CurrentUser = user;

                // Consider adding a check to determine if the token is 
                // expired, as shown in this post: http://aka.ms/jww5vp.

                success = true;
                message = string.Format("Cached credentials for user - {0}", user.UserId);
            }
            else
            {
                try
                {
                    // Login with the identity provider.
                    user = await App.MobileService
                        .LoginAsync(provider);

                    // Create and store the user credentials.
                    credential = new PasswordCredential(provider.ToString(),
                        user.UserId, user.MobileServiceAuthenticationToken);
                    vault.Add(credential);

                    success = true;
                    message = string.Format("You are now logged in - {0}", user.UserId);
                }
                catch (MobileServiceInvalidOperationException)
                {
                    message = "You must log in. Login Required";
                }
            }
            
            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();

            return success;
        }

    V této verzi **AuthenticateAsync**aplikaci pokusí se použít přihlašovacím údajům uloženým v **PasswordVault** pro přístup ke službě. Běžná přihlašovací proběhne také po žádné uložené přihlašovací údaje.

    >[AZURE.NOTE]Režim cached token může vypršelo a vypršení platnosti tokenu může dojít po ověření se při použití aplikace. Zjistěte, jak zjistit, pokud vypršela platnost token, najdete v článku [zjišťovat tokeny vypršela platnost ověřování](http://aka.ms/jww5vp). Řešení zpracování chyb se tak mohli ověřovat související s vypršení jejich platnosti tokenů najdete v tématu [ukládání do mezipaměti a zpracování vypršela platnost tokeny služby Azure Mobile spravovaných SDK](http://blogs.msdn.com/b/carlosfigueira/archive/2014/03/13/caching-and-handling-expired-tokens-in-azure-mobile-services-managed-sdk.aspx)příspěvek. 

3. Restartujte aplikaci dvakrát.

    Všimněte si, že při prvním spuštění, přihlaste se pomocí zprostředkovatele požaduje znovu. Však na druhém restartování se používají v mezipaměti přihlašovací údaje a přihlašovací obejít. 
