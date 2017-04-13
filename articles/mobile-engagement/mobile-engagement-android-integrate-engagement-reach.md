<properties
    pageTitle="Integrace Azure mobilní zapojení Android SDK"
    description="Nejnovější aktualizace a postupy pro Android SDK pro zapojení Mobile Azure"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

#<a name="how-to-integrate-engagement-reach-on-android"></a>Jak integrovat zapojení Reach na Androidu

> [AZURE.IMPORTANT] Postup integrace popsaná v dialogu jak chcete integrovat zapojení na Android dokumentu před provedením této příručce.

##<a name="standard-integration"></a>Standardní integrace

SDK dosáhla vyžaduje **Android podpory knihovny (v4)**.

Chcete-li přidat knihovny do projektu v **zatmění** nejrychleji `Right click on your project -> Android Tools -> Add Support Library...`.

Pokud nepoužíváte zatmění, si můžete přečíst pokyny [v tomto poli].

Zkopírujte souborů prostředků Reach z SDK v projektu:

-   Kopírování souborů `res/layout` složku Doručená s SDK do `res/layout` složky aplikace.
-   Kopírování souborů `res/drawable` složku Doručená s SDK do `res/drawable` složky aplikace.

Úprava vaší `AndroidManifest.xml` souboru:

-   Přidejte následující část (mezi `<application>` a `</application>` značky):

            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/html" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity" android:theme="@android:style/Theme.Dialog" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>
            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver" android:exported="false">
              <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
              </intent-filter>
            </receiver>
            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachDownloadReceiver">
              <intent-filter>
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
              </intent-filter>
            </receiver>

-   Potřebujete toto oprávnění k přehrání upozornění systému, které jste neklikli při spuštění (v opačném případě bude nacházet na disku, ale nezobrazí se už, potřebujete skutečně zahrnují).

            <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

-   Určení ikony umožňují oznámení (obě v aplikaci a systém z nich) kopírujete a upravujete v následující části (mezi `<application>` a `</application>` značky):

            <meta-data android:name="engagement:reach:notification:icon" android:value="<name_of_icon_WITHOUT_file_extension_and_WITHOUT_'@drawable/'>" />

> [AZURE.IMPORTANT] Tento oddíl je **povinný** , pokud nebudete chtít pomocí upozornění systému při vytváření Reach kampaní. Android zabrání oznámení systém bez ikony zobrazovaly. Takže vynecháme-li v této části, koncovým uživatelům nebude moct přijímat.

-   Pokud vytvoříte kampaní s pomocí celkového upozornění systému, budete muset přidat následující oprávnění (po `</application>` značka) Pokud chybějící:

            <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
            <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

  -   Na Android M a pokud vaše aplikace zaměřuje Android rozhraní API úrovně 23 nebo vyšší ``WRITE_EXTERNAL_STORAGE`` oprávnění vyžaduje schválení uživatele. Přečtěte si [Tento oddíl](mobile-engagement-android-integrate-engagement.md#android-m-permissions).

-   Systém oznámení můžete taky určit kampaně Reach Pokud zařízení by měl vyzvánění a/nebo vibrace. Pro bude platit, musíte se deklarované následující oprávnění (po `</application>` značka):

            <uses-permission android:name="android.permission.VIBRATE" />

    Bez toto oprávnění Android zabrání oznámení systém před zobrazí, pokud jste zaškrtli vyzvánět nebo možnost vibrate ve Správci dosáhla kampaní.

-   Pokud vytváříte aplikace pomocí **ProGuard** a mít chyby týkající se systémem Android podpory knihovny nebo využití sklenice, přidejte následující řádky do svého `proguard.cfg` souboru:

            -dontwarn android.**
            -keep class android.support.v4.** { *; }

## <a name="native-push"></a>Nativní připínáčku

Teď jste nakonfigurovali Reach modul, budete muset konfigurace nativní nabízených aby mohli přijímat kampaní v zařízení.

Na Androidu podporujeme dvě služby:

  - Google Play zařízení: použití [Zasílání zpráv Cloud Google] podle pokynů průvodce [jak integrovat GCM pomocí Průvodce zapojení](mobile-engagement-android-gcm-integrate.md) .
  - Zařízení Amazon: použití [Zasílání zpráv zařízení Amazon] podle pokynů průvodce [jak integrovat ADM pomocí Průvodce Engagement](mobile-engagement-android-adm-integrate.md) .

Pokud chcete směrovat zařízení Amazon a Google Play, možných mít všechno uvnitř 1 AndroidManifest.xml/APK rozvoje. Ale při odesílání do Amazon, jsou-li najít kód GCM může odmítnout aplikace.

Měli byste použít více APKs v takovém případě.

**Aplikace je nyní připravena k přijímání a zobrazit reach kampaně!**

##<a name="how-to-handle-data-push"></a>Postup v případě nabízených dat

### <a name="integration"></a>Integrace

Pokud chcete aplikaci moct přijímat Reach dat posune, musíte vytvořit podřízené třídu `com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver` a odkaz na `AndroidManifest.xml` souboru (mezi `<application>` a/nebo `</application>` značky):

            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>

Pak můžete změnit `onDataPushStringReceived` a `onDataPushBase64Received` zpětná. Tady je příklad:

            public class MyDataPushReceiver extends EngagementReachDataPushReceiver
            {
              @Override
              protected Boolean onDataPushStringReceived(Context context, String category, String body)
              {
                Log.d("tmp", "String data push message received: " + body);
                return true;
              }

              @Override
              protected Boolean onDataPushBase64Received(Context context, String category, byte[] decodedBody, String encodedBody)
              {
                Log.d("tmp", "Base64 data push message received: " + encodedBody);
                // Do something useful with decodedBody like updating an image view
                return true;
              }
            }

### <a name="category"></a>Kategorie

Parametr kategorie je nepovinný při vytváření kampaně nabízená dat a umožňuje že k filtrování dat posune. To je užitečné, pokud ještě několika vysílání příjemce zpracování různé typy dat posune, nebo pokud chcete posunout různých druhů z `Base64` dat a chcete určit jejich typ před jejich analýzy.

### <a name="callbacks-return-parameter"></a>Vrácení parametr zpětná účastníků

Tady je několik pokynů, které správně zpracovávat zpáteční parametr `onDataPushStringReceived` a `onDataPushBase64Received`:

-   Vysílání sluchátko, měly vrátit `null` v zpětné Pokud neví postup v případě dat připínáčku. Chcete-li zjistit, jestli vaše vysílání sluchátko zacházet nabízených dat nebo ne používejte kategorii.
-   Jednu vysílání sluchátko, měly vrátit `true` v zpětné Pokud umožňovala přijímání vašich dat připínáčku.
-   Jednu vysílání sluchátko, měly vrátit `false` v zpětné pokud rozpozná nabízených dat, ale odstraní z jakéhokoliv důvodu. Například vrátit `false` při přijaté dat je neplatný.
-   Pokud jeden vysílání sluchátko vrátí `true` zatímco jiné jednu vrátí `false` pro stejný push dat chování je definován, nikdy, měli byste udělat.

Je vráceným typem slouží jenom pro statistiku Reach:

-   `Replied`je zvýšen, pokud je nějaká vysílání příjemce vrácené buď `true` nebo `false`.
-   `Actioned`je zvýšen jenom v případě, že jeden vysílání příjemci vrácena `true`.

##<a name="how-to-customize-campaigns"></a>Jak přizpůsobit kampaně

Přizpůsobení kampaní, můžete změnit rozložení podle SDK kontaktovat.

Měli byste zachovat všechny identifikátory použité v rozložení a zachovat typy zobrazení, které používají identifikátoru, zejména u textu a obrázků zobrazení. Některá zobrazení jenom slouží k zobrazení nebo skrytí oblasti tak jejich typ může změnit. Pokud chcete změnit typ zobrazení v zadané rozložení zkontrolujte zdrojového kódu.

### <a name="notifications"></a>Oznámení

Existují dva typy oznámení: systém a v aplikaci oznámení, které používají jiné rozložení soubory.

#### <a name="system-notifications"></a>Upozornění systému

Chcete-li dialogové okno Upravit oznamování systémové potřebných pro použití **kategorií**. Můžete přejít na [kategorie](#categories).

#### <a name="in-app-notifications"></a>Oznámení v aplikaci

Oznámení v aplikaci je ve výchozím zobrazení, ve kterém se dynamicky přidá do uživatelského rozhraní aktuální činnosti díky metodu Android `addContentView()`. Je místo toho možnost překrytím oznámení. Oznámení o překrytí jsou skvělé k rychlé integrace, protože nevyžadují můžete upravit jakékoli rozložení v aplikaci.

Chcete-li změnit vzhled překrytí oznámení, můžete jednoduše změnit soubor `engagement_notification_area.xml` vašim potřebám.

> [AZURE.NOTE] Soubor `engagement_notification_overlay.xml` je ten, který slouží k vytváření oznámení překrytí obsahuje soubor `engagement_notification_area.xml`. Můžete taky přizpůsobit tak, aby odpovídal vašim potřebám (například umístění oznamovací oblasti v rámci překrytí).

##### <a name="include-notification-layout-as-part-of-an-activity-layout"></a>Zahrnout oznámení rozložení jako součást aktivity rozložení

Překrytí jsou skvělé k rychlé integrace, ale můžete být nevhodné nebo vedlejší efekty v speciálních případů. Systém překryvném můžete přizpůsobit na úrovni aktivity, což usnadňuje zabránit vedlejší efekty za speciální.

Můžete se rozhodnout, zahrnout naše oznámení rozložení existující rozložení díky příkazu Android **Zahrnout** . Následuje příklad změněné `ListActivity` rozložení obsahující jenom `ListView`.

**Před integrací můžete zapojit:**

            <?xml version="1.0" encoding="utf-8"?>
            <ListView
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@android:id/list"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent" />

**Po integrace můžete zapojit:**

            <?xml version="1.0" encoding="utf-8"?>
            <LinearLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:orientation="vertical"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <ListView
                android:id="@android:id/list"
                android:layout_width="fill_parent"
                android:layout_height="fill_parent"
                android:layout_weight="1" />

              <include layout="@layout/engagement_notification_area" />

            </LinearLayout>

V tomto příkladu teď nově přidaná kontejneru nadřazené od původní rozložení použití zobrazení seznamu jako element nejvyšší úrovně. Také přidali jsme `android:layout_weight="1"` mohli přidat zobrazení pod zobrazení seznamu nakonfigurována `android:layout_height="fill_parent"`.

Zapojení dosáhla SDK automaticky zjistí, že rozložení oznámení je součástí tuto aktivitu a nepřidá překrytí pro tuto aktivitu.

> [AZURE.TIP] Pokud používáte ListActivity v aplikaci, viditelné překrytí Reach zabrání reagovat na už kliknutí položek v zobrazení seznamu. Toto je známý problém. Informace k alternativním řešením tohoto problému doporučujeme vložit rozložení oznámení v rozložení vlastní seznam aktivity podobně jako v předchozím příkladu.

##### <a name="disabling-application-notification-per-activity"></a>Zakázání aplikace oznámení podle aktivity

Pokud nechcete, aby překrytí které budou přidány do vaší aktivity a pokud rozložení oznámení nechcete zahrnout do vlastního rozložení, můžete zakázat překrytí pro tuto aktivitu v `AndroidManifest.xml` přidáním `meta-data` oddíl jako v následujícím příkladu:

            <activity android:name="SplashScreenActivity">
              <meta-data android:name="engagement:notification:overlay" android:value="false"/>
            </activity>

#### <a name="categories"></a>Kategorie

Při úpravě zadané rozložení změníte vzhled vám oznámení. Kategorie umožňují definovat různé vzhledy cílových (případně chování) pro oznámení. Kategorie lze zadat při vytváření Reach kampaní. Mějte na paměti, že kategorie také umožňují přizpůsobit oznámení a hlasování, které jsou popsané dál v tomto dokumentu.

Zaregistrovat rutinu kategorie pro vám oznámení, potřebujete přidat hovoru při inicializaci aplikace.

> [AZURE.IMPORTANT] Přečtěte si upozornění o atributu android: proces \<android sdk zapojení proces\> v dialogu jak chcete integrovat zapojení na Android téma pokračovat.

Následující příklad předpokládá potvrzeno předchozí upozornění a použijte dílčí třídu `EngagementApplication`:

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), "myCategory");
              }
            }

`MyNotifier` Objekt je provádění rutiny kategorie oznámení. Je buď implementace `EngagementNotifier` rozhraní nebo třídy sub provádění výchozí: `EngagementDefaultNotifier`.

Všimněte si, že stejný oznamovatel zpracovat několik kategorií se můžete zaregistrovat nějak takto:

            reachAgent.registerNotifier(new MyNotifier(this), "myCategory", "myAnotherCategory");

Nahrazení výchozí kategorie implementace zaregistrujete k vaší implementací podobně jako v následujícím příkladu:

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), Intent.CATEGORY_DEFAULT); // "android.intent.category.DEFAULT"
              }
            }

Kategorie aktuální používán obslužná rutina předané jako parametr ve většině metody můžete přepsat v `EngagementDefaultNotifier`.

Předáním buď jako `String` parametr nebo nepřímo `EngagementReachContent` objekt, který má `getCategory()` metody.

Většina proces vytváření oznámení můžete změnit předefinováním metody na `EngagementDefaultNotifier`, pro pokročilejší úpravy neváhejte podívejte se na technickou dokumentaci a na další zdrojového kódu.

##### <a name="in-app-notifications"></a>Oznámení v aplikaci

Pokud chcete použít alternativní rozložení pro určité kategorie, můžete to implementace jako v následujícím příkladu:

            public class MyNotifier extends EngagementDefaultNotifier
            {
              public MyNotifier(Context context)
              {
                super(context);
              }

              @Override
              protected int getOverlayLayoutId(String category)
              {
                return R.layout.my_notification_overlay;
              }


              @Override
              public Integer getOverlayViewId(String category)
              {
                return R.id.my_notification_overlay;
              }

              @Override
              public Integer getInAppAreaId(String category)
              {
                return R.id.my_notification_area;
              }
            }

**Příklad `my_notification_overlay.xml` :**

            <?xml version="1.0" encoding="utf-8"?>
            <RelativeLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/my_notification_overlay"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <include layout="@layout/my_notification_area" />

            </RelativeLayout>

Jak můžete vidět, identifikátor překryvném zobrazení se liší od standardní. Je důležité, že každé rozložení určenou překrytí jedinečný identifikátor.

**Příklad `my_notification_area.xml` :**

            <?xml version="1.0" encoding="utf-8"?>
            <merge
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <RelativeLayout
                android:id="@+id/my_notification_area"
                android:layout_width="fill_parent"
                android:layout_height="64dp"
                android:layout_alignParentTop="true"
                android:background="#B000">

                <LinearLayout
                  android:orientation="horizontal"
                  android:layout_width="fill_parent"
                  android:layout_height="fill_parent"
                  android:gravity="center_vertical">

                  <ImageView
                    android:id="@+id/engagement_notification_icon"
                    android:layout_width="48dp"
                    android:layout_height="48dp" />

                  <LinearLayout
                    android:id="@+id/engagement_notification_text"
                    android:orientation="vertical"
                    android:layout_width="fill_parent"
                    android:layout_height="fill_parent"
                    android:layout_weight="1"
                    android:gravity="center_vertical">

                    <TextView
                      android:id="@+id/engagement_notification_title"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:singleLine="true"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Medium" />

                    <TextView
                      android:id="@+id/engagement_notification_message"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:maxLines="2"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Small" />

                  </LinearLayout>

                  <ImageView
                    android:id="@+id/engagement_notification_image"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:adjustViewBounds="true" />

                  <ImageButton
                    android:id="@+id/engagement_notification_close_area"
                    android:visibility="invisible"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:src="@android:drawable/btn_dialog"
                    android:background="#0F00" />

                </LinearLayout>

                <ImageButton
                  android:id="@+id/engagement_notification_close"
                  android:layout_width="wrap_content"
                  android:layout_height="fill_parent"
                  android:layout_alignParentRight="true"
                  android:src="@android:drawable/btn_dialog"
                  android:background="#0F00" />

              </RelativeLayout>

            </merge>

Jak můžete vidět, se liší od standardní identifikátor oznamovací oblasti zobrazení. Je důležité, že každé rozložení používá jedinečný identifikátor pro oznamovací oblasti.

Tento jednoduchý příklad kategorie zajišťuje aplikace (nebo v aplikaci) oznámení zobrazené v horní části obrazovky. Jsme nezměnili standardní identifikátory použité v oznamovací oblasti samotné.

Pokud chcete změnit, budete muset změnit `EngagementDefaultNotifier.prepareInAppArea` metody. Doporučujeme podívejte se na technickou dokumentaci a na zdrojový kód `EngagementNotifier` a `EngagementDefaultNotifier` Pokud chcete vlastní úroveň.

##### <a name="system-notifications"></a>Upozornění systému

Prodloužením `EngagementDefaultNotifier`, je možné přepsat `onNotificationPrepared` změnit oznámení, že byl připraven implementací výchozí.

Příklad:

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content)
              throws RuntimeException
            {
              if ("ongoing".equals(content.getCategory()))
                notification.flags |= Notification.FLAG_ONGOING_EVENT;
              return true;
            }

V tomto příkladu je systém oznámení pro obsah je zobrazena jako událost probíhající při použití "probíhající" kategorie.

Pokud budete chtít vytvořit `Notification` objektu od začátku, můžete se vrátit `false` způsobem a volání `notify` sami na `NotificationManager`. V takovém případě je důležité, abyste `contentIntent`, `deleteIntent` a identifikátor oznámení používaný `EngagementReachReceiver`.

Tady je správné příklady těchto implementaci:

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content) throws RuntimeException
            {
              /* Required fields */
              NotificationCompat.Builder builder = new NotificationCompat.Builder(mContext)
                .setSmallIcon(notification.icon)              // icon is mandatory
                .setContentIntent(notification.contentIntent) // keep content intent
                .setDeleteIntent(notification.deleteIntent);  // keep delete intent

              /* Your customization */
              // builder.set...

              /* Dismiss option can be managed only after build */
              Notification myNotification = builder.build();
              if (!content.isNotificationCloseable())
                myNotification.flags |= Notification.FLAG_NO_CLEAR;

              /* Notify here instead of super class */
              NotificationManager manager = (NotificationManager) mContext.getSystemService(Context.NOTIFICATION_SERVICE);
              manager.notify(getNotificationId(content), myNotification); // notice the call to get the right identifier

              /* Return false, we notify ourselves */
              return false;
            }

##### <a name="notification-only-announcements"></a>Oznámení o pouze oznámení

Správa klikněte na informační pouze oznámení můžete přizpůsobit přepsáním `EngagementDefaultNotifier.onNotifAnnouncementIntentPrepared` pro úpravy připravený `Intent`. Používání této metody umožňuje snadno ladění příznaků.

Pokud například chcete přidat `SINGLE_TOP` příznak:

            @Override
            protected Intent onNotifAnnouncementIntentPrepared(EngagementNotifAnnouncement notifAnnouncement,
              Intent intent)
            {
              intent.addFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP);
              return intent;
            }

Uživatelům starších zapojení dejte pozor, aby upozornění systému bez akce adresy URL teď spustí aplikaci pokud byl pozadí, abyste tuto metodu můžete volat s oznámení bez adresa URL akce. Měli byste zvážit, přizpůsobování záměr.

Můžete taky implementovat `EngagementNotifier.executeNotifAnnouncementAction` úplně od začátku.

##### <a name="notification-life-cycle"></a>Oznámení o životním cyklu

Pokud chcete použít výchozí kategorie, některé metody životního cyklu nazývají na `EngagementReachInteractiveContent` objektu do sestavy Statistika a aktualizovat stav kampaně:

-   Při zobrazení v aplikaci nebo umístění na stavovém řádku na upozornění `displayNotification` způsob je místo toho možnost (který sestavy Statistika) tak, že `EngagementReachAgent` Pokud `handleNotification` vrátí `true`.
-   Je-li zrušit oznámení `exitNotification` názvem metody statistický hlášení a další kampaní lze teď zpracovávat.
-   Po kliknutí na oznámení `actionNotification` je s názvem, vykázaného statistický a spuštění přidružené záměr.

Pokud vaše implementace `EngagementNotifier` obchází výchozí chování, budete muset volat těchto postupů životního cyklu za vás. Následující příklady ilustrují někdy, kde je výchozí chování obejít:

-   Není rozšíření `EngagementDefaultNotifier`, například implementovaná kategorie zpracování od začátku.
-   Pro upozornění systému overrode `onNotificationPrepared` a změnili `contentIntent` nebo `deleteIntent` v `Notification` objektu.
-   Oznámení ve aplikace overrode `prepareInAppArea`, je potřeba namapovat aspoň `actionNotification` jednu z vaší U.I ovládacích prvků.

> [AZURE.NOTE] Pokud `handleNotification` vyvolá se odstraní výjimky obsahu a `dropContent` se nazývá. To je uveden ve Statistika a další kampaní lze teď zpracovávat.

### <a name="announcements-and-polls"></a>Oznámení a hlasování

#### <a name="layouts"></a>Rozložení

Můžete změnit `engagement_text_announcement.xml`, `engagement_web_announcement.xml` a `engagement_poll.xml` souborů, které chcete přizpůsobit oznámení textu, web oznámení a hlasování.

Tyto soubory můžete sdílet dva běžné rozložení pro oblasti nadpisu a oblasti tlačítka. Rozložení pro název je `engagement_content_title.xml` a používá eponymous drawable soubor jako pozadí. Rozložení pro tlačítka akce a konec je `engagement_button_bar.xml` a používá eponymous drawable soubor jako pozadí.

V hlasování, otázka rozložení a jejich možnosti jsou dynamicky nahuštěny pomocí několikrát `engagement_question.xml` soubor rozložení pro otázky a `engagement_choice.xml` soubor možnosti.

#### <a name="categories"></a>Kategorie

##### <a name="alternate-layouts"></a>Alternativní rozložení

Třeba oznámení kampaně kategorie lze mít alternativní rozložení pro oznámení a hlasování.

Například vytvořit kategorie pro oznámení text, můžete rozšířit `EngagementTextAnnouncementActivity` a odkazovat `AndroidManifest.xml` souboru:

            <activity android:name="com.your_company.MyCustomTextAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>

Všimněte si, že kategorie ve záměru filtrování se používají k vytvoření rozdíl s výchozí aktivita oznámení.

SDK dosáhla se systémem záměru řešení správné aktivitou pro určité kategorie a se přejde zpět na výchozí kategorie Pokud selže rozlišení.

Pak budete muset implementace `MyCustomTextAnnouncementActivity`, pokud chcete jenom změnit rozložení (při zachování stejné identifikátory zobrazení), je třeba jen definování třídy podobně jako v následujícím příkladu:

            public class MyCustomTextAnnouncementActivity extends EngagementTextAnnouncementActivity
            {
              @Override
              protected String getLayoutName()
              {
                return "my_text_announcement";  // tell super class to use R.layout.my_text_announcement
              }
            }

Pokud chcete nahradit výchozí kategorie text oznámení, jednoduše nahradit `android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"` vaší implementací.

Podobným způsobem můžete přizpůsobit web oznámení a hlasování.

Pro web oznámení můžete rozšířit `EngagementWebAnnouncementActivity` a deklarovat vaše aktivita v `AndroidManifest.xml` jako v následujícím příkladu:

            <activity android:name="com.your_company.MyCustomWebAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/html" />    <!-- only difference with text announcements in the intent is the data mime type -->
              </intent-filter>
            </activity>

Pro hlasování můžete rozšířit `EngagementPollActivity` a deklarovat vaší v `AndroidManifest.xml` jako v následujícím příkladu:

            <activity android:name="com.your_company.MyCustomPollActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="my_category" />
              </intent-filter>
            </activity>

##### <a name="implementation-from-scratch"></a>Implementace od začátku

Kategorie pro vašich aktivit najdete oznámení (a hlasování) můžete používat bez rozšíření jednu z `Engagement*Activity` třídy poskytovanou SDK kontaktovat. To je užitečné například pokud chcete definovat rozložení, která nepoužívá stejném zobrazení jako standardní rozložení.

Jako Upřesnit oznámení přizpůsobit, se doporučuje se podívat, zdrojového kódu standardní implementace.

Tady jsou některé věci, které mějte na paměti: Reach spustí aktivitu s konkrétní záměr (odpovídá záměru filtr) plus další parametr, což je identifikátor obsahu.

Načtení obsahu objektu, které obsahují pole, která jste zadali při vytváření kampaně na webu je můžete takto:

            public class MyCustomTextAnnouncement extends EngagementActivity
            {
              private EngagementAnnouncement mContent;

              @Override
              protected void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);

                /* Get content */
                mContent = EngagementReachAgent.getInstance(this).getContent(getIntent());
                if (mContent == null)
                {
                  /* If problem with content, exit */
                  finish();
                  return;
                }

                setContentView(R.layout.my_text_announcement);

                /* Configure views by querying fields on mContent */
                // ...
              }
            }

Statistiky, měli byste nahlásit obsah zobrazená v `onResume` události:

            @Override
            protected void onResume()
            {
             /* Mark the content displayed */
             mContent.displayContent(this);
             super.onResume();
            }

Potom, nezapomeňte zavolejte `actionContent(this)` nebo `exitContent(this)` obsahu objektu před aktivity přejde do pozadí.

Pokud nemůžete volat buď `actionContent` nebo `exitContent`, Statistika neodešle (tedy bez technologie pro analýzu na kampaně) a důležitější další kampaní upozorněni, dokud se nerestartuje procesu aplikace.

Orientace nebo jiné změny konfigurace může být kód záludné a zjistit, zda aktivity přejde do pozadí nebo Ne, standardní implementace zajišťuje, obsah ohlášení neopustí Pokud uživatel opustí aktivity (buď stisknutím kombinace kláves `HOME` nebo `BACK`), ale ne, pokud se změní orientace.

Tady je zajímavé součástí provedení:

            @Override
            protected void onUserLeaveHint()
            {
              finish();
            }

            @Override
            protected void onPause()
            {
              if (isFinishing() && mContent != null)
              {
                /*
                 * Exit content on exit, this is has no effect if another process method has already been
                 * called so we don't have to check anything here.
                 */
                mContent.exitContent(this);
              }
              super.onPause();
            }

Jak je vidět, pokud jste volali už někdy `actionContent(this)` dokončení aktivity, a pak `exitContent(this)` lze bezpečně volat bez nutnosti žádný vliv.

[Tady]:http://developer.android.com/tools/extras/support-library.html#Downloading
[Cloud Google zasílání zpráv]:http://developer.android.com/guide/google/gcm/index.html
[Zasílání zpráv zařízení Amazon]:https://developer.amazon.com/sdk/adm.html
