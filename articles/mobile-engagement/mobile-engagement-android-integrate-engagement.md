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

#<a name="how-to-integrate-engagement-on-android"></a>Jak integrovat zapojení na Androidu

> [AZURE.SELECTOR]
- [Univerzální systému Windows](mobile-engagement-windows-store-integrate-engagement.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-integrate-engagement.md)

Tento postup popisuje nejjednodušší způsob, jak aktivovat společnosti zapojení technologie pro analýzu a sledování funkce v aplikaci Android.

> [AZURE.IMPORTANT] Minimální úroveň Android rozhraní API SDK musí být 10 nebo novější (Android 2.3.3 nebo vyšší).

Tyto kroky jsou tak, abyste aktivuje sestav protokolů potřebné pro výpočet všech statistických údajů týkajících se uživatelů, relací, aktivity, dojde k chybě a Technicals. Sestava protokoly potřebné pro výpočet dalších statistiky jako události, chyb a úlohy musí udělat ručně pomocí rozhraní API zapojení (protože tyto statistiky jsou aplikace závislé informace [o použití Upřesnit Mobile zapojení přiřazování značek k rozhraní API v Androidu](mobile-engagement-android-use-engagement-api.md) .

##<a name="embed-the-engagement-sdk-and-service-into-your-android-project"></a>Vložení zapojení SDK a služeb do Androidem projektu

Stahování Android SDK [tady](https://aka.ms/vq9mfn) Get `mobile-engagement-VERSION.jar` a vložit je do `libs` složky Android projektu (vytvořte složku knihoven, pokud ho dosud neexistuje).

> [AZURE.IMPORTANT]
> Vytvoření balíčku aplikace s ProGuard potřebnosti zachovat některé třídy. Můžete použít následující fragment konfigurace:
>
>
            -keep public class * extends android.os.IInterface
            -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
            <methods>;
            }

Zadejte připojovací řetězec zapojení tak, že zavoláte podle pokynů v aktivitě pro spuštění:

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

Připojovací řetězec pro aplikaci se zobrazí na portál Azure.

-   Pokud chybí, přidejte následující Android oprávnění (před `<application>` značka):

            <uses-permission android:name="android.permission.INTERNET"/>
            <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>

-   Přidejte následující část (mezi `<application>` a `</application>` značky):

            <service
              android:name="com.microsoft.azure.engagement.service.EngagementService"
              android:exported="false"
              android:label="<Your application name>Service"
              android:process=":Engagement"/>

-   Změna `<Your application name>` tak, že název aplikace.

> [AZURE.TIP] `android:label` Atribut umožňuje vybrat název služby zapojení vizitku koncovým uživatelům na obrazovce "Službami" Telefon. Doporučuje se nastavit tento atribut `"<Your application name>Service"` (například `"AcmeFunGameService"`).

Určení `android:process` atribut zaručuje, že službu zapojení se spustí ve vlastním procesu (aplikaci zapojení stejným způsobem jako aplikace, aby hlavní/uživatelské rozhraní vlákna potenciálně pomaleji,).

> [AZURE.NOTE] Jakýkoli kód, umístěte do `Application.onCreate()` a ostatní aplikace zpětná spustí pro všechny aplikace procesy, včetně službu Engagement. Může mít nežádoucí straně efektů (jako nepotřebné paměti přidělení a vláknech zapojení procesu, duplicitní vysílání příjemce nebo služby).

Pokud je přepsat `Application.onCreate()`, doporučujeme přidat následující fragment kódu na začátku vaše `Application.onCreate()` funkce:

             public void onCreate()
             {
               if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
                 return;

               ... Your code...
             }

Umí totéž `Application.onTerminate()`, `Application.onLowMemory()` a `Application.onConfigurationChanged(...)`.

Můžete také rozšířit `EngagementApplication` místo rozšíření `Application`: zpětné `Application.onCreate()` provede kontrolu obrázku a volání `Application.onApplicationProcessCreate()` jen pokud aktuální proces není zapojení službě hostingu, stejná pravidla požádat o další zpětná.

##<a name="basic-reporting"></a>Základní vytváření sestav

### <a name="recommended-method-overload-your-activity-classes"></a>Doporučená metoda: přetížení vaše `Activity` třídy

Pro aktivaci sestavy všechny požadované zapojení pro výpočet uživatelů, relací, aktivity, dojde k chybě a technických statistických protokoly, stačí musíte všechny vaše `*Activity` dílčí třídy dědí odpovídající `Engagement*Activity` třídy (například pokud starší verze aktivitách rozšiřuje `ListActivity`, přepnutí přesahuje `EngagementListActivity`).

**Bez můžete zapojit:**

            package com.company.myapp;

            import android.app.Activity;
            import android.os.Bundle;

            public class MyApp extends Activity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

**S můžete zapojit:**

            package com.company.myapp;

            import com.microsoft.azure.engagement.activity.EngagementActivity;
            import android.os.Bundle;

            public class MyApp extends EngagementActivity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

> [AZURE.IMPORTANT] Při použití `EngagementListActivity` nebo `EngagementExpandableListActivity`, ujistěte se, že některé volat `requestWindowFeature(...);` je volání před voláním `super.onCreate(...);`, jinak dojde k chybě.

Můžete najít těchto tříd v `src` složky a můžete zkopírovat do projektu. Třídy jsou také **JavaDoc**.

### <a name="alternate-method-call-startactivity-and-endactivity-manually"></a>Alternativní metody: volání `startActivity()` a `endActivity()` ručně

Pokud nemůžete ani nebudete chtít přetížení vaše `Activity` třídy, můžete místo toho počátečního a koncového vašich aktivit najdete tak, že zavoláte `EngagementAgent`společnosti metody přímo.

> [AZURE.IMPORTANT] Android SDK nikdy volání `endActivity()` metody, i když zavřený (na Androidu, jsou ve skutečnosti nikdy ukončit aplikace). Je tedy *DŮRAZNĚ* doporučujeme volání `startActivity()` metoda v `onResume` zpětné ze *všech* vašich aktivit a `endActivity()` metoda v `onPause()` zpětné ze *všech* vašich aktivit najdete. Toto je jediný způsob, jak Ujistěte se, že nebude prozrazený relace. Pokud prozrazený relaci službu zapojení (protože službu zůstane připojen, dokud relaci čeká na vyřízení) nikdy odpojit back-end Engagement.

Tady je příklad:

            public class MyActivity extends Some3rdPartyActivity
            {
              @Override
              protected void onResume()
              {
                super.onResume();
                String activityNameOnEngagement = EngagementAgentUtils.buildEngagementActivityName(getClass()); // Uses short class name and removes "Activity" at the end.
                EngagementAgent.getInstance(this).startActivity(this, activityNameOnEngagement, null);
              }

              @Override
              protected void onPause()
              {
                super.onPause();
                EngagementAgent.getInstance(this).endActivity();
              }
            }

V tomto příkladu velmi podobné `EngagementActivity` předmětu a jeho variant, podle jehož zdrojového kódu `src` složky.

##<a name="test"></a>Test

Teď zkontrolujte, zda vaše integrace spuštěním mobilní aplikace v emulátoru nebo zařízení a ověřit, že registruje relace na kartě Monitor.

V následujících částech jsou volitelné.

##<a name="location-reporting"></a>Vytváření sestav umístění

Pokud budete potřebovat umístění vykázat, budete muset přidat několik řádků konfigurace (mezi `<application>` a `</application>` značky).

### <a name="lazy-area-location-reporting"></a>Vytváření sestav umístění pustí oblast

Vytváření sestav umístění pustí oblasti umožňuje vykazování zemi, oblasti a místo přidružené k zařízení. Tento typ umístění vykazování používá pouze síťová umístění (na základě ID buňku nebo přes Wi-Fi). Oblasti zařízení je uveden nejvíce jednou za relace. GPS nepoužívali, a tento typ sestavy umístění dostane velmi málo (pozor, abyste uveden bez) dopad na battery.

Nahlášeného oblasti slouží k výpočtu zeměpisnou statistických údajů o uživatelích, relace, událostí a chyb. Můžete taky používají jako kritéria v Reach kampaní.

Povolit vykazování umístění pustí oblasti, můžete to udělat pomocí nástroje Konfigurace výše zmíněné následujícím způsobem:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Budete taky muset přidat následující oprávnění pokud chybějící:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Nebo můžete dál používat ``ACCESS_FINE_LOCATION`` použijete už ho v aplikaci.

### <a name="real-time-location-reporting"></a>Vytváření sestav umístění reálném čase

Vytváření sestav umístění reálném čase umožňuje vykazování šířky a délky přidružené k zařízení. Ve výchozím nastavení tento typ umístění vykazování pouze používá síťová umístění (na základě ID buňku nebo přes Wi-Fi) a vykazování je jenom aktivní při spuštění aplikace v popředí (tedy během relace).

Umístění reálném čase jsou *není* používá pro výpočet statistiky. Jejich pouze účel umožňující použití geo oddělení reálném čase \<Reach cílové skupiny geofencing\> kritéria v Reach kampaní.

Povolit vykazování umístění reálném čase, můžete to udělat pomocí nástroje Konfigurace výše zmíněné následujícím způsobem:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Budete taky muset přidat následující oprávnění pokud chybějící:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Nebo můžete dál používat ``ACCESS_FINE_LOCATION`` použijete už ho v aplikaci.

#### <a name="gps-based-reporting"></a>Na základě GPS oznámení

Ve výchozím nastavení hlášení umístění reálném čase používá pouze na základě síťových umístěních. Chcete-li povolit použití GPS na základě umístění, (které jsou úplně přesnější), použijte objekt konfigurace:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Budete taky muset přidat následující oprávnění pokud chybějící:

            <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a>Vytváření sestav pozadí

Ve výchozím nastavení hlášení umístění reálném čase je aktivní pouze při spuštění aplikace v popředí (tedy během relace). Povolíte také vykazování pozadí, použijte objekt konfigurace:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [AZURE.NOTE] Při spuštění aplikace na pozadí, jen síť na základě umístění vznikly, i když jste povolili GPS.

Sestava umístění pozadí se zastaví, pokud uživatel restartuje své zařízení, můžete si přidat následující postup je znovu spustit automaticky při spuštění:

            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>

Budete taky muset přidat následující oprávnění pokud chybějící:

            <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

### <a name="android-m-permissions"></a>Android M oprávnění

Začnete s Androidem M, některé oprávnění jsou spravovány za běhu a vyžaduje schválení uživatele.

Oprávnění runtime vypnou se ve výchozím nastavení pro nové aplikace instalace pokud obrázku Android rozhraní API úroveň 23. V opačném případě ho bude se ve výchozím nastavení zapnutá.

Uživatele můžete povolit nebo zakázat oprávnění v nabídce Nastavení zařízení. Pokud vypnete oprávnění z systémovou nabídku ukončuje procesy na pozadí aplikace, to je chování systému a nemá žádný vliv na možnost pro příjem nabízených pozadí.

Kontext Mobile zapojení oprávnění, které vyžadují schválení za běhu způsoby:

- `ACCESS_COARSE_LOCATION`
- `ACCESS_FINE_LOCATION`
- `WRITE_EXTERNAL_STORAGE`(pouze v případě zacílení Android rozhraní API úroveň 23 pro tuto položku)

Externí úložiště slouží jenom pro Reach celkového funkce. Pokud zjistíte, než toto oprávnění je vyloučit, můžete ho odebrat Pokud jste použili jenom pro mobilní zapojení ale na druhou stranu zakázání funkce celkového rámce.

Místo funkce vyžádat oprávnění pro uživatele pomocí dialogového okna standardní systému. Pokud schválí uživatele, budete muset zadat ``EngagementAgent`` přijmout tuto změnu v úvahu v reálném čase (jinak změny budou zpracovány při příštím spuštění aplikace).

Tady je ukázka kódu můžete pomocí v aktivitu aplikace žádosti o oprávnění a dál výsledek, pokud pozitivní ``EngagementAgent``:

    @Override
    public void onCreate(Bundle savedInstanceState)
    {
      /* Other code... */

      /* Request permissions */
      requestPermissions();
    }

    @TargetApi(Build.VERSION_CODES.M)
    private void requestPermissions()
    {
      /* Avoid crashing if not on Android M */
      if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M)
      {
        /*
         * Request location permission, but this won't explain why it is needed to the user.
         * The standard Android documentation explains with more details how to display a rationale activity to explain the user why the permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of the same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

        /* Only if you want to keep features using external storage */
        if (checkSelfPermission(android.Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.WRITE_EXTERNAL_STORAGE }, 1);
      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence the request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }

##<a name="advanced-reporting"></a>Rozšířené možnosti vytváření sestav

Volitelně, pokud chcete do sestavy aplikace zvláštní události, chyby a úlohy, potřebujete rozhraní API zapojení pomocí metody `EngagementAgent` předmětu. Objekt této třídy může být retreived tak, že zavoláte `EngagementAgent.getInstance()` statické metody.

Rozhraní API zapojení umožňuje používat všechny pokročilé možnosti, které je zapojení a podrobné v dialogu jak používat rozhraní API zapojení na Android (stejně jako v technickou dokumentaci `EngagementAgent` třídy).

##<a name="advanced-configuration-in-androidmanifestxml"></a>Upřesnit konfiguraci (v AndroidManifest.xml)

### <a name="wake-locks"></a>Aktivovat zámků webů

Pokud chcete mít jistotu, že statistiky jsou odeslány v reálném čase při používání Wi-Fi nebo když obrazovku, přidejte následující volitelná oprávnění:

            <uses-permission android:name="android.permission.WAKE_LOCK"/>

### <a name="crash-report"></a>Zpráva o selhání

Pokud chcete zakázat pád sestav, přidejte tento (mezi `<application>` a `</application>` značky):

            <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a>Mezní hodnota požadavků

Ve výchozím nastavení protokoly sestavy využití služeb v reálném čase. Pokud aplikace sestav protokolů často, je lepší vyrovnávací paměť protokoly a vykazování v celém dokumentu na pravidelných základní časových (říká se "požadavků režimu"). Postup přidání to (mezi `<application>` a `</application>` značky):

            <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

Režim požadavků mírně zvětšit života battery, ale bude mít vliv na monitoru zapojení: všechny relace a úlohy doba trvání bude zaokrouhleno na požadavků prahové hodnoty (tedy relace a úlohy kratší než mezní hodnota požadavků nemusí být vidět). Doporučujeme používat mezní požadavků než 30000 (30s).

### <a name="session-timeout"></a>Časový limit relace

Ve výchozím nastavení je relaci ukončené 10s po skončení jeho poslední aktivity, (které obvykle dojde, stisknutím klávesy pro domácnosti nebo pro zpět, nastavením telefonu nečinný nebo přechod do jiné aplikace). Toto je chcete-li předejít rozdělení relace každý čas ukončení uživatele a vraťte se do aplikace velmi rychle (který můžete objevil Jestliže mu vystopovat obrázku, zkontrolujte, zda oznámení atd.). Je vhodné upravit tento parametr. Postup přidání to (mezi `<application>` a `</application>` značky):

            <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

##<a name="disable-log-reporting"></a>Zakázání protokol

### <a name="using-a-method-call"></a>Pomocí metody hovoru

Pokud budete potřebovat zapojení zastavit odesílání protokoly, můžete volat:

            EngagementAgent.getInstance(context).setEnabled(false);

Volání je trvalý: použije souboru sdíleného předvolby.

Pokud zapojení je aktivní, při volání na tuto funkci, může trvat jednu minutu pro zastavení služby. Však ho nespustí službu vůbec při příštím spuštění aplikace.

Povolit vykazování znovu tak, že zavoláte stejnou funkci pomocí protokolu `true`.

### <a name="integration-in-your-own-preferenceactivity"></a>Integrace v vlastní`PreferenceActivity`

Místo volání tuto funkci, můžete také integrovat toto nastavení používají přímo ve vašem stávajícím `PreferenceActivity`.

Konfigurace zapojení používat předvolby soubor (požadované režimu) v `AndroidManifest.xml` soubor s `application meta-data`:

-   `engagement:agent:settings:name` Klíče slouží k definování název souboru s sdílených předvolby.
-   `engagement:agent:settings:mode` Klíče slouží k definování režimu souboru sdíleného předvoleb, byste měli použít stejné režimu ve vaší `PreferenceActivity`. Režim musí předané jako čísla: Pokud používáte kombinací konstantní příznaků v kódu, zkontrolujte celkové hodnoty.

Zapojení vždy použít `engagement:key` boolean klíč souboru aplikace na předvolby pro správu toto nastavení.

Následující příklad `AndroidManifest.xml` zobrazí výchozí hodnoty:

            <application>
                [...]
                <meta-data
                  android:name="engagement:agent:settings:name"
                  android:value="engagement.agent" />
                <meta-data
                  android:name="engagement:agent:settings:mode"
                  android:value="0" />

Pak můžete přidat `CheckBoxPreference` předvoleb rozložení jako je následující:

            <CheckBoxPreference
              android:key="engagement:enabled"
              android:defaultValue="true"
              android:title="Use Engagement"
              android:summaryOn="Engagement is enabled."
              android:summaryOff="Engagement is disabled." />

<!-- URLs. -->
[Device API]: http://go.microsoft.com/?linkid=9876094
