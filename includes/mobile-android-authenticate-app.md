
1. V **Project Explorer** Android Studio otevřete soubor ToDoActivity.java a přidejte následující příkazy pro import.

        import java.util.concurrent.ExecutionException;
        import java.util.concurrent.atomic.AtomicBoolean;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;

        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceAuthenticationProvider;
        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceUser;

2. Přidejte následující metodu třídy **ToDoActivity** : 
    
        private void authenticate() {
            // Login using the Google provider.
            
            ListenableFuture<MobileServiceUser> mLogin = mClient.login(MobileServiceAuthenticationProvider.Google);
    
            Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                @Override
                public void onFailure(Throwable exc) {
                    createAndShowDialog((Exception) exc, "Error");
                }           
                @Override
                public void onSuccess(MobileServiceUser user) {
                    createAndShowDialog(String.format(
                            "You are now logged in - %1$2s",
                            user.getUserId()), "Success");
                    createTable();  
                }
            });     
        }


    Tím vytvoříte nový způsob zpracování procesu ověření. Uživatel je ověřen pomocí Google přihlášení. Dialogové okno se zobrazí zobrazující ID ověřeného uživatele. Nemůže pokračovat bez kladné ověření.

    > [AZURE.NOTE] Pokud používáte zprostředkovatelem identit než Google, změňte hodnotu předán metodu **login** nad na jeden z těchto věcí: _MicrosoftAccount_, _Facebook_, _Twitter_nebo _windowsazureactivedirectory_.

3. V metodě **onCreate** přidat následující kód za kód, který vytvoří `MobileServiceClient` objektu.

        authenticate();

    Volání spustí proces ověření.

4. Přesunutí zbývající kód po `authenticate();` v metodu **onCreate** nová metoda **createTable** , která vypadá takto:

        private void createTable() {
    
            // Get the table instance to use.
            mToDoTable = mClient.getTable(ToDoItem.class);
    
            mTextNewToDo = (EditText) findViewById(R.id.textNewToDo);
    
            // Create an adapter to bind the items with the view.
            mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
            ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
            listViewToDo.setAdapter(mAdapter);
    
            // Load the items from Azure.
            refreshItemsFromTable();
        }

9. V nabídce **Spustit** klepnutím na tlačítko **Spustit aplikaci** spusťte aplikaci a přihlaste se pomocí svého poskytovatele zvolené identity. 

    Po úspěšném přihlášení k lyncu se změnami, aplikace by měla běžet bez chyb a by měla dotazu službu back-end a aktualizace dat.