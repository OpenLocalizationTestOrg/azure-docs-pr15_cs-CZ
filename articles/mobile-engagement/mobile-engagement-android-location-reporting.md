<properties
    pageTitle="Vytváření sestav pro Android SDK Azure mobilní zapojení umístění"
    description="Popisuje, jak nakonfigurovat umístění vytváření sestav pro Android SDK zapojení Azure Mobile"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

# <a name="location-reporting-for-azure-mobile-engagement-android-sdk"></a>Vytváření sestav pro Android SDK Azure mobilní zapojení umístění

> [AZURE.SELECTOR]
- [Android](mobile-engagement-android-integrate-engagement.md)

Toto téma popisuje, jak provádět umístění oznámení pro aplikace Android.

## <a name="prerequisites"></a>Zjistit předpoklady pro

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="location-reporting"></a>Vytváření sestav umístění

Pokud budete potřebovat umístění uvedená, budete muset přidat několik řádků konfigurace (mezi `<application>` a `</application>` značky).

### <a name="lazy-area-location-reporting"></a>Vytváření sestav umístění pustí oblast

Umístění pustí oblasti vykazování umožňuje vytváření sestav země, Kraj a místo přidružený k zařízení. Tento typ umístění vykazování používá pouze síťová umístění (na základě ID buňky nebo přes Wi-Fi). Oblasti zařízení je uveden nejvíce jednou za relace. Nepoužívali GPS a tedy tento typ sestavy umístění má zhoršeným vliv na battery.

Nahlášeného oblastí slouží k výpočtu zeměpisné statistických údajů o uživatelích, relace, událostí a chyb. Můžete taky používají jako kritéria v Reach kampaní.

Povolení umístění pustí oblasti vytváření sestav pomocí nástroje Konfigurace výše zmíněné následujícím způsobem:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Budete potřebovat k určení umístění oprávnění. Tento kód používá ``COARSE`` oprávnění:

    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Pokud aplikace vyžaduje, můžete použít ``ACCESS_FINE_LOCATION`` místo.

### <a name="real-time-location-reporting"></a>Vytváření sestav v reálném čase umístění

Vytváření sestav v reálném čase umístění umožňuje vytváření sestav šířky a délky přidružený k zařízení. Ve výchozím nastavení používá tento typ umístění vykazování pouze síťová umístění na základě ID buňky nebo přes Wi-Fi. Vykazování působí pouze při spuštění aplikace v popředí (třeba během relace).

Data v reálném umístění jsou *není* používá pro výpočet statistiky. Jejich pouze slouží k použití v reálném čase geo oddělení \<Reach cílové skupiny geofencing\> kritéria v Reach kampaní.

Povolit vykazování v reálném čase umístění, Přidání spojnice kódu kde nastavení zapojení připojovací řetězec v Spouštěč aktivity. Výsledek vypadá takto:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

        You also need to specify a location permission. This code uses ``COARSE`` permission:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

        If your app requires it, you can use ``ACCESS_FINE_LOCATION`` instead.

#### <a name="gps-based-reporting"></a>Na základě GPS vytváření sestav

Ve výchozím nastavení vytváření sestav v reálném čase umístění používá pouze síťové umístění. Chcete-li povolit použití na základě GPS umístění, které jsou úplně přesnější, pomocí objektu konfigurace:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Budete taky muset přidat následující oprávnění pokud chybějící:

    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a>Vytváření sestav pozadí

Ve výchozím nastavení vytváření sestav v reálném čase umístění je aktivní pouze při spuštění aplikace v popředí (třeba během relace). Povolíte také vykazování pozadí, použijte tento objekt konfigurace:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [AZURE.NOTE] Při spuštění aplikace na pozadí, jsou uvedena pouze síťové umístění, i když jste povolili GPS.

Pokud uživatel restartuje svého zařízení, se zastaví sestavy umístění pozadí. Chcete-li znovu spustit automaticky při spuštění, přidejte tento kód.

    <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
           android:exported="false">
        <intent-filter>
            <action android:name="android.intent.action.BOOT_COMPLETED" />
        </intent-filter>
    </receiver>

Budete taky muset přidat následující oprávnění pokud chybějící:

    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

## <a name="android-m-permissions"></a>Android M oprávnění

Začnete s Androidem M, určitá oprávnění je spravováno za běhu potřeby a schválení uživatele.

Pokud obrázku Android rozhraní API úroveň 23 runtime oprávnění jsou vypnou a zůstanou vypnuté ve výchozím nastavení pro nové aplikace instalace. V opačném případě zapnuté ve výchozím nastavení.

Je můžete povolit nebo zakázat oprávnění v nabídce Nastavení zařízení. Pokud vypnete oprávnění z systémové nabídky ukončuje procesů aplikace, která je chování systému a nemá žádný vliv na možnost pro příjem nabízených pozadí na pozadí.

Kontext umístění Mobile zapojení vykazování oprávnění, které vyžadují schválení za běhu způsoby:

- `ACCESS_COARSE_LOCATION`
- `ACCESS_FINE_LOCATION`

Požádat o oprávnění od uživatele pomocí dialogového okna standardní systému. Pokud schválí uživatel zjistit ``EngagementAgent`` přijmout tuto změnu v úvahu v reálném čase. Změna zpracování jinak při příštím spuštění aplikace.

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
         * Request location permission, but this doesn't explain why it is needed to the user.
         * The standard Android documentation explains with more details how to display a rationale activity to explain the user why the permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of the same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence the request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }
