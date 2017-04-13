<properties
    pageTitle="Oznámení o rozbočovače novinky výuková – Android"
    description="Naučte se používat rozbočovače oznámení Bus služby Azure o neodeslání oznámení nejnovějších příspěvků zařízení s Androidem."
    services="notification-hubs"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>


# <a name="use-notification-hubs-to-send-breaking-news"></a>Odeslání novinky pomocí rozbočovače oznámení

[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

##<a name="overview"></a>Základní informace

V tomto tématu se dozvíte, jak používat Azure oznámení rozbočovače vysílání oznámení nejnovější příspěvky do aplikace pro Android. Až budete hotovi, bude moct zaregistrujte si nejnovější příspěvky kategorií, které vás zajímají a zobrazí jenom nabízených oznámení pro těchto kategorií. Tento scénář je běžné vzor pro mnoho aplikace, kde oznámení musí být odeslány do skupiny uživatelů, které jste dříve deklarované zájem o nich, například programu pro čtení RSS, aplikace pro hudbu ventilátory atd.

Vysílání scénáře jsou povoleny s využitím jednu nebo více _značek_ při vytváření registrace v centru oznámení. Když jsou odeslána oznámení následujícím uživatelům značku, všechna zařízení, které jste si zaregistrovali značku dostanou oznámení. Protože značek jsou jednoduše řetězce, nemají zřízení předem. Další informace o značky podívejte se do [směrování rozbočovače oznámení a značky výrazech](notification-hubs-tags-segment-push-message.md).


##<a name="prerequisites"></a>Zjistit předpoklady pro

Toto téma je založena na aplikaci vytvořené v části [Začínáme s oznámení rozbočovače][get-started]. Před zahájením tohoto kurzu, třeba již jste dokončili [začít pracovat s oznámení rozbočovače][get-started].

##<a name="add-category-selection-to-the-app"></a>Výběr kategorie přidejte do aplikace

Cílem prvního kroku je přidání prvků uživatelského rozhraní k existující hlavní činnosti, které umožňují uživatelům vybrat kategorie k registraci. Kategorie vybrané uživatelem jsou uložené na zařízení. Po spuštění aplikace, vytvoří se registrace zařízení jako značky ve rozbočovače oznámení s vybrané kategorie.

1. Otevřete soubor res/layout/activity_main.xml a nahradit obsahu pomocí následujících akcí:

        <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:tools="http://schemas.android.com/tools"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:paddingBottom="@dimen/activity_vertical_margin"
            android:paddingLeft="@dimen/activity_horizontal_margin"
            android:paddingRight="@dimen/activity_horizontal_margin"
            android:paddingTop="@dimen/activity_vertical_margin"
            tools:context="com.example.breakingnews.MainActivity"
            android:orientation="vertical">

                <CheckBox
                    android:id="@+id/worldBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_world" />
                <CheckBox
                    android:id="@+id/politicsBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_politics" />
                <CheckBox
                    android:id="@+id/businessBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_business" />
                <CheckBox
                    android:id="@+id/technologyBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_technology" />
                <CheckBox
                    android:id="@+id/scienceBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_science" />
                <CheckBox
                    android:id="@+id/sportsBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_sports" />
                <Button
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:onClick="subscribe"
                    android:text="@string/button_subscribe" />
        </LinearLayout>

2. Otevřete soubor res/values/strings.xml a přidejte následující řádky:

        <string name="button_subscribe">Subscribe</string>
        <string name="label_world">World</string>
        <string name="label_politics">Politics</string>
        <string name="label_business">Business</string>
        <string name="label_technology">Technology</string>
        <string name="label_science">Science</string>
        <string name="label_sports">Sports</string>

    Grafické rozložení main_activity.xml by měl vypadat takto:

    ![][A1]

3. Nyní vytvořte třídu **oznámení** ve stejném balíčku jako svojí třídě **MainActivity** .

        import java.util.HashSet;
        import java.util.Set;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.os.AsyncTask;
        import android.util.Log;
        import android.widget.Toast;

        import com.google.android.gms.gcm.GoogleCloudMessaging;
        import com.microsoft.windowsazure.messaging.NotificationHub;

        public class Notifications {
            private static final String PREFS_NAME = "BreakingNewsCategories";
            private GoogleCloudMessaging gcm;
            private NotificationHub hub;
            private Context context;
            private String senderId;

            public Notifications(Context context, String senderId, String hubName, 
                                    String listenConnectionString) {
                this.context = context;
                this.senderId = senderId;
        
                gcm = GoogleCloudMessaging.getInstance(context);
                hub = new NotificationHub(hubName, listenConnectionString, context);
            }

            public void storeCategoriesAndSubscribe(Set<String> categories)
            {
                SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
                settings.edit().putStringSet("categories", categories).commit();
                subscribeToCategories(categories);
            }

            public Set<String> retrieveCategories() {
                SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
                return settings.getStringSet("categories", new HashSet<String>());
            }

            public void subscribeToCategories(final Set<String> categories) {
                new AsyncTask<Object, Object, Object>() {
                    @Override
                    protected Object doInBackground(Object... params) {
                        try {
                            String regid = gcm.register(senderId);
        
                            String templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";
        
                            hub.registerTemplate(regid,"simpleGCMTemplate", templateBodyGCM, 
                                categories.toArray(new String[categories.size()]));
                        } catch (Exception e) {
                            Log.e("MainActivity", "Failed to register - " + e.getMessage());
                            return e;
                        }
                        return null;
                    }
        
                    protected void onPostExecute(Object result) {
                        String message = "Subscribed for categories: "
                                + categories.toString();
                        Toast.makeText(context, message,
                                Toast.LENGTH_LONG).show();
                    }
                }.execute(null, null, null);
            }

        }

    Této třídy používá místní úložiště pro ukládání kategorií příspěvků, který má toto zařízení dostávat. Obsahuje také metody zaregistrovat k těchto kategorií.


4. Ve svojí třídě **MainActivity** odeberte pole private **NotificationHub** a **GoogleCloudMessaging**a přidejte pole pro **oznámení**:

        // private GoogleCloudMessaging gcm;
        // private NotificationHub hub;
        private Notifications notifications;

5. V metodu **onCreate** odeberte inicializace pole **rozbočovači** a metodu **registerWithNotificationHubs** . Potom přidejte následující řádky, které Inicializace instance třídy **oznámení** . 


        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            MyHandler.mainActivity = this;
    
            NotificationsManager.handleNotifications(this, SENDER_ID,
                    MyHandler.class);
    
            notifications = new Notifications(this, SENDER_ID, HubName, HubListenConnectionString);
    
            notifications.subscribeToCategories(notifications.retrieveCategories());
        }

    `HubName`a `HubListenConnectionString` nastavte už se `<hub name>` a `<connection string with listen access>` zástupné symboly s vaším jménem centrální oznámení a připojovací řetězec pro *DefaultListenSharedAccessSignature* , který jste získali dříve.

    > [AZURE.NOTE] Protože přihlašovací údaje, které jsou rozdělení s klientské aplikace nejsou obecně zabezpečené, by měl distribuovat klávesu poslech přístup pomocí protokolu pouze s aplikací klienta. Příjem umožňuje přístup k aplikaci zaregistrovat k oznámení, ale existující registrace nelze upravit, a nemůže odeslána oznámení následujícím. Plný přístup používá se služby Zabezpečené back-end pro posílání oznámení a změna existující registrace.


6. Potom přidejte následující importy a `subscribe` metodu ke zpracování na tlačítko Přihlásit odběr klikněte na událost:
        
        import android.widget.CheckBox;
        import java.util.HashSet;
        import java.util.Set;

        public void subscribe(View sender) {
            final Set<String> categories = new HashSet<String>();

            CheckBox world = (CheckBox) findViewById(R.id.worldBox);
            if (world.isChecked())
                categories.add("world");
            CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
            if (politics.isChecked())
                categories.add("politics");
            CheckBox business = (CheckBox) findViewById(R.id.businessBox);
            if (business.isChecked())
                categories.add("business");
            CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
            if (technology.isChecked())
                categories.add("technology");
            CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
            if (science.isChecked())
                categories.add("science");
            CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
            if (sports.isChecked())
                categories.add("sports");

            notifications.storeCategoriesAndSubscribe(categories);
        }

    Tento způsob vytvoří seznam kategorií a používá třídu **oznámení** ukládat seznamu v místní úložiště a zaregistrujte příslušné značky v centrální oznámení. Při změně kategorie znovu pomocí nových kategorií vytvoří registrace.

Aplikace je teď možné ukládat sadu kategorií do místní úložiště v zařízení a zaregistrovat v centru oznámení pokaždé, když uživatel změní výběru kategorie.

##<a name="register-for-notifications"></a>Registrace k oznámení

Tento postup zaregistrujte v centru oznámení při spuštění pomocí kategorií, které byly uložené v místním úložišti.

> [AZURE.NOTE] Vzhledem k tomu můžete kdykoli změnit atributy registrationId přiřazené tak, že zpráv Cloud Google (GCM), byste měli zaregistrovat oznámení často chcete-li předejít oznámení o selhání. V tomto příkladu zaregistruje pro oznámení o každém spuštění aplikace. K aplikacím, které často se mají spustit více než jednou za den, můžete pravděpodobně přeskočit registrace Chcete-li zachovat šířky pásma, pokud od předchozího registrace uplynul menší než jeden den.


1. Na konci metodu **onCreate** ve třídě **MainActivity** přidáte následující kód:

        notifications.subscribeToCategories(notifications.retrieveCategories());

    Zajistíte tím, že při každém spuštění aplikace načte kategorií z místního úložiště a žádosti registeration těchto kategorií. 

2. Změňte `onStart()` metodu `MainActivity` třídy následujícím způsobem:

    @Overridepole Chráněná void onStart() {super.onStart();      isVisible = PRAVDA a naopak.

        Set<String> categories = notifications.retrieveCategories();

        CheckBox world = (CheckBox) findViewById(R.id.worldBox);
        world.setChecked(categories.contains("world"));
        CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
        politics.setChecked(categories.contains("politics"));
        CheckBox business = (CheckBox) findViewById(R.id.businessBox);
        business.setChecked(categories.contains("business"));
        CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
        technology.setChecked(categories.contains("technology"));
        CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
        science.setChecked(categories.contains("science"));
        CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
        sports.setChecked(categories.contains("sports"));
    }

    Toto tlačítko aktualizujete hlavní činnosti založené na stav dříve uložené kategorie.

Aplikace je teď dokončení a mohou být uloženy sadu kategorií v místní úložiště zařízení používá k registraci centru oznámení pokaždé, když uživatel změní výběru kategorie. Dále jsme definovat back-end, které můžou odesílat upozornění na kategorii této aplikace.

##<a name="sending-tagged-notifications"></a>Odeslání příznakem oznámení

[AZURE.INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

##<a name="run-the-app-and-generate-notifications"></a>Spusťte aplikaci a generovat oznámení

1. V Android Studio vytvořit aplikaci a začněte na zařízení nebo emulátoru.

    Poznámka: aby aplikaci uživatelského rozhraní k dispozici sadu přepíná, který umožňuje zvolte kategorií, které se přihlásit k odběru.

2. Povolení jeden nebo více přepíná kategorií a potom klikněte na **Přihlásit se k odběru**.

    Aplikace převede vybrané kategorie značky a požádá rozbočovači oznámení o nové registrace zařízení pro vybrané značky. Registrovaná kategorie jsou vráceny a zobrazeny v informačního oznámení.

4. Odeslání oznámení o nové spuštěním konzoly .NET aplikace.  Můžete taky můžete odeslat příznakem šablony oznámení pomocí karty ladění rozbočovače oznámení na [Klasické portál Azure].

    Oznámení pro vybrané kategorie se zobrazí jako informačního oznámení.

##<a name="next-steps"></a>Další kroky

V tomto kurzu budeme naučili vysílání novinky podle kategorie. Zkuste jednu z následujících výukové programy, které zvýrazňují další rozšířené scénáře rozbočovače oznámení o dokončení:

+ [Použití oznámení rozbočovače vysílání lokalizované novinky]

    Zjistěte, jak rozbalte aplikaci nejnovějších příspěvků povolit odesílání lokalizované upozornění.





<!-- Images. -->
[A1]: ./media/notification-hubs-aspnet-backend-android-breaking-news/android-breaking-news1.PNG

<!-- URLs.-->
[get-started]: notification-hubs-android-push-notification-google-gcm-get-started.md
[Použití oznámení rozbočovače vysílání lokalizované novinky]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Azure klasické portálu]: https://manage.windowsazure.com
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
