
1. V zobrazení řešení (nebo v **Okně Průzkumník** ve Visual Studiu) klikněte pravým tlačítkem na složku **součásti** , klikněte na **Získat víc součástí...**, vyhledejte komponentu **Google Cloud zasílání zpráv** a přidejte do projektu.

2. Otevření souboru projektu ToDoActivity.cs a přidejte následující pomocí příkazu třídy:

        using Gcm.Client;

3. Ve třídě **ToDoActivity** přidejte tyto typy nového kódu: 

        // Create a new instance field for this activity.
        static ToDoActivity instance = new ToDoActivity();

        // Return the current activity instance.
        public static ToDoActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }
        // Return the Mobile Services client.
        public MobileServiceClient CurrentClient
        {
            get
            {
                return client;
            }
        }

    Umožňuje přístup k instanci mobilních klientů z procesu nabízených rutiny služby.

4.  Přidejte následující kód metody **OnCreate** po vytvoření **MobileServiceClient** :

        // Set the current instance of TodoActivity.
        instance = this;

        // Make sure the GCM client is set up correctly.
        GcmClient.CheckDevice(this);
        GcmClient.CheckManifest(this);

        // Register the app for push notifications.
        GcmClient.Register(this, ToDoBroadcastReceiver.senderIDs);

**ToDoActivity** je nyní připravena pro přidání nabízená oznámení.