
V předchozím příkladu ukázal standardní přihlásit, který vyžaduje klienta kontaktovat zprostředkovatele identit a back-end služby Azure při každém spuštění aplikace. Nejen je tato metoda, že můžete narazíte na problémy související s použití by měl množství zákazníků pokusu o spuštění aplikace ve stejnou dobu. Lepší přístup je do mezipaměti se tak mohli ověřovat token vrácené službou Azure a pokusíte použít první před použitím základě poskytovatele přihlášení. 

>[AZURE.NOTE]Můžete mezipaměti token vydané back-end Azure služby bez ohledu na to, jestli používáte klienta Správa přístupových práv nebo služby Správa přístupových práv ověřování. Tento kurz používá ověřování služby Správa přístupových práv.


1. Otevřete soubor ToDoActivity.java a přidejte následující příkazy import:

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;

2. Přidání následujícím členům `ToDoActivity` předmětu.

        public static final String SHAREDPREFFILE = "temp"; 
        public static final String USERIDPREF = "uid";  
        public static final String TOKENPREF = "tkn";   


3. V souboru ToDoActivity.java přidat následující definice `cacheUserToken` metody.
 
        private void cacheUserToken(MobileServiceUser user)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            Editor editor = prefs.edit();
            editor.putString(USERIDPREF, user.getUserId());
            editor.putString(TOKENPREF, user.getAuthenticationToken());
            editor.commit();
        }   
  
    Tento způsob ukládá id uživatele a token předvoleb soubor, který je označený soukromé. Aplikace access do mezipaměti to měli Zamknout a dalších aplikací na zařízení nemají přístup do tokenu, protože předvolby je v izolovaném prostoru aplikace. Pokud někdo získá přístup k zařízení, je však možné, že mohou získat přístup k mezipaměti tokenů jiným způsobem. 

    >[AZURE.NOTE]Můžete dál uzamknutím token s šifrování Pokud tokenu přístup k vašim datům se považuje za vysoce citlivé a někdo mohou získat přístup k zařízení. Úplně bezpečné řešení je však nad rámec tohoto kurzu a v závislosti na vašim požadavkům na zabezpečení.


4. V souboru ToDoActivity.java přidat následující definice `loadUserTokenCache` metody.

        private boolean loadUserTokenCache(MobileServiceClient client)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            String userId = prefs.getString(USERIDPREF, null); 
            if (userId == null)
                return false;
            String token = prefs.getString(TOKENPREF, null); 
            if (token == null)
                return false;
                
            MobileServiceUser user = new MobileServiceUser(userId);
            user.setAuthenticationToken(token);
            client.setCurrentUser(user);
                
            return true;
        }



5. V souboru *ToDoActivity.java* nahraďte `authenticate` metodu pomocí následujícího postupu využívající tokenu mezipaměti. Pokud chcete používat účet než Google změňte poskytovatele přihlášení.

        private void authenticate() {
            // We first try to load a token cache if one exists.
            if (loadUserTokenCache(mClient))
            {
                createTable();
            }
            // If we failed to load a token cache, login and create a token cache
            else
            {
                // Login using the Google provider.    
                ListenableFuture<MobileServiceUser> mLogin = mClient.login(MobileServiceAuthenticationProvider.Google);
        
                Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                    @Override
                    public void onFailure(Throwable exc) {
                        createAndShowDialog("You must log in. Login Required", "Error");
                    }           
                    @Override
                    public void onSuccess(MobileServiceUser user) {
                        createAndShowDialog(String.format(
                                "You are now logged in - %1$2s",
                                user.getUserId()), "Success");
                        cacheUserToken(mClient.getCurrentUser());
                        createTable();  
                    }
                });
            }
        }

6. Vytvořte aplikaci a otestujte ověřování pomocí platného účtu. Spusťte aspoň dvakrát. Při prvním spuštění mělo by se zobrazit výzva k přihlášení a vytvoření tokenu mezipaměti. Poté každé spuštění pokus o načtení tokenu mezipaměti pro ověřování a nesmí být nutné k přihlášení.



