<properties
    pageTitle="Odesílání zabezpečené nabízená oznámení s rozbočovače Azure oznámení"
    description="Zjistěte, jak posílání zabezpečené nabízená oznámení pro aplikace pro Android z Azure. Ukázky napsané v Java a C#."
    documentationCenter="android"
    keywords="nabízená oznámení, nabízená oznámení nabízená zprávy, android nabízená oznámení"
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

#<a name="sending-secure-push-notifications-with-azure-notification-hubs"></a>Odesílání zabezpečené nabízená oznámení s rozbočovače Azure oznámení

> [AZURE.SELECTOR]
- [Univerzální systému Windows](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
- [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
- [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)

##<a name="overview"></a>Základní informace

> [AZURE.IMPORTANT] Tento kurz, musíte mít účet Azure active. Pokud nemáte účet, můžete vytvořit bezplatný účet zkušební v jenom pár minut. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).

Podpora nabízených oznámení v Microsoft Azure umožňuje přístup k-použití s více platformami, diagramů s měřítky mimo nabízená zprávy infrastruktury, což výrazně zjednodušuje provádění nabízených oznámení pro aplikace příjemce a enterprise pro mobilní platformy.

Z důvodu zákonné nebo omezení zabezpečení někdy aplikace může být vhodné zahrnout něco upozornění, které nelze předávají infrastruktury standardní nabízená oznámení. Tento kurz popisuje, jak dosáhnout stejné prostředí tak, že citlivé informace pomocí funkce zabezpečené ověřené připojení mezi klientského zařízení s Androidem a back-end aplikace.

Na vysoké úrovni, řízení toku, je takto:

1. Aplikace back-end:
    - Úložiště zabezpečené datové v back-end databázi.
    - ID toto oznámení odešle zařízení s Androidem (se neodesílají žádné informace zabezpečené).
2. Aplikaci v zařízení pro příjem oznámení:
    - Zařízení s Androidem kontaktů back-end požaduje zabezpečené náklad.
    - Aplikaci můžete zobrazit datové jako upozornění na zařízení.

Je důležité mít na paměti, že v předchozím tok (a v tomto kurzu) jsme se předpokládá, že zařízení ukládá token ověřování místní úložiště, po přihlášení. Úplně bezproblémovou práci, zajistíte tak jako zařízení můžete načíst pomocí tohoto tokenu zabezpečení datové na upozornění. Pokud aplikace neukládá tokeny ověřování na zařízení nebo můžete platnost těchto tokenů, aplikaci zařízení po přijetí nabízená oznámení by měl zobrazit obecné oznámení dotázání uživatele k spuštění aplikace. Aplikace potom ověřuje uživatele a zobrazí datové oznámení.

Tento kurz ukazuje, jak poslat zabezpečené nabízená oznámení. To je založena na kurz [Upozornit uživatele](notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md) tak, aby měli dokončení kroků v tomto kurzu nejdřív Pokud jste tak již neučinili.

> [AZURE.NOTE] Tento kurz se předpokládá, že jste vytvořili a nakonfigurovali oznámení rozbočovači podle popisu v [Příručce Začínáme s rozbočovače oznámení (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md).

[AZURE.INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-android-project"></a>Úprava Android projektu

Teď jste změnili vaše aplikace serverové odeslat jenom nabízená oznámení *id* , budete muset změnit Android aplikace zpracovat oznámení a zavoláte zpět vaše serverové k načtení zabezpečené zprávy zobrazovat.
K tomuto účelu máte Ujistěte se, že aplikace pro Android ví, jak a dokončit ověření s back-end Jakmile obdrží nabízená oznámení.

Teď jsme změní tok *přihlášení* za účelem uložení hodnotu ověřování záhlaví v předvolbách sdílené aplikace. Podobnou stránce mechanismy mohou sloužit k ukládání všechny token ověřování (například OAuth tokeny), který aplikace muset používat bez nutnosti přihlašovací údaje uživatele.

1. Do aplikace Androidu projektu přidejte následující konstanty v horní části třídy **MainActivity** :

        public static final String NOTIFY_USERS_PROPERTIES = "NotifyUsersProperties";
        public static final String AUTHORIZATION_HEADER_PROPERTY = "AuthorizationHeader";

2. Pořád ve třídě **MainActivity** aktualizovat `getAuthorizationHeader()` metoda má být tento kód:

        private String getAuthorizationHeader() throws UnsupportedEncodingException {
            EditText username = (EditText) findViewById(R.id.usernameText);
            EditText password = (EditText) findViewById(R.id.passwordText);
            String basicAuthHeader = username.getText().toString()+":"+password.getText().toString();
            basicAuthHeader = Base64.encodeToString(basicAuthHeader.getBytes("UTF-8"), Base64.NO_WRAP);

            SharedPreferences sp = getSharedPreferences(NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            sp.edit().putString(AUTHORIZATION_HEADER_PROPERTY, basicAuthHeader).commit();

            return basicAuthHeader;
        }

3. Přidejte následující `import` příkazy v horní části souboru **MainActivity** :

        import android.content.SharedPreferences;

Nyní změníme rutinu, která se nazývá při doručení.

4. V **MyHandler** třídy změnit `OnReceive()` metoda má být:

        public void onReceive(Context context, Bundle bundle) {
            ctx = context;
            String secureMessageId = bundle.getString("secureId");
            retrieveNotification(secureMessageId);
        }

5. Přidejte `retrieveNotification()` metoda nahrazení zástupného symbolu `{back-end endpoint}` s koncovým back-end získali při nasazení back-end:

        private void retrieveNotification(final String secureMessageId) {
            SharedPreferences sp = ctx.getSharedPreferences(MainActivity.NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            final String authorizationHeader = sp.getString(MainActivity.AUTHORIZATION_HEADER_PROPERTY, null);

            new AsyncTask<Object, Object, Object>() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {
                        HttpUriRequest request = new HttpGet("{back-end endpoint}/api/notifications/"+secureMessageId);
                        request.addHeader("Authorization", "Basic "+authorizationHeader);
                        HttpResponse response = new DefaultHttpClient().execute(request);
                        if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                            Log.e("MainActivity", "Error retrieving secure notification" + response.getStatusLine().getStatusCode());
                            throw new RuntimeException("Error retrieving secure notification");
                        }
                        String secureNotificationJSON = EntityUtils.toString(response.getEntity());
                        JSONObject secureNotification = new JSONObject(secureNotificationJSON);
                        sendNotification(secureNotification.getString("Payload"));
                    } catch (Exception e) {
                        Log.e("MainActivity", "Failed to retrieve secure notification - " + e.getMessage());
                        return e;
                    }
                    return null;
                }
            }.execute(null, null, null);
        }


Tento způsob hovorů vaše aplikace serverové načíst obsah oznámení pomocí přihlašovacích údajů uložených v předvolbách sdílené a zobrazí jako normální upozornění. Oznámení vypadá uživateli aplikace stejně jako jakékoli jiné nabízená oznámení.

Všimněte si, že je vhodnější případů chybějící vlastnosti záhlaví ověřování nebo zamítnutí zpracovávat back-end. Specifické zpracování takovýchto případech závisí převážně na cílovém uživatelské prostředí. Jednou z možností se zobrazí oznámení s obecný výzva pro uživatele k ověření k načtení skutečné oznámení.

## <a name="run-the-application"></a>Spusťte aplikaci

Pokud chcete spustit aplikaci, postupujte takto:

1. Zkontrolujte, že **AppBackend** nasazenou v Azure. Pokud používáte Visual Studiu, spusťte aplikaci rozhraní API webových **AppBackend** . Zobrazí se webová stránka ASP.NET.

2. V zatmění spusťte aplikaci na fyzické zařízení s Androidem nebo emulátor.

3. Aplikaci Android uživatelského rozhraní zadejte uživatelské jméno a heslo. Tyto může být jakýkoli řetězec, ale musí mít stejnou hodnotu.

4. Aplikaci Android uživatelského rozhraní klikněte na **přihlásit**. Klepněte na tlačítko **Odeslat připínáčku**.
