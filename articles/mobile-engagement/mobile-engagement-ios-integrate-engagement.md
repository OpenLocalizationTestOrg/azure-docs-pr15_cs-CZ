<properties
    pageTitle="Azure Mobile zapojení iOS SDK integrace | Microsoft Azure"
    description="Nejnovější aktualizace a postupy pro iOS SDK pro zapojení Mobile Azure"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="piyushjo" />

#<a name="how-to-integrate-engagement-on-ios"></a>Jak integrovat zapojení na iOS

> [AZURE.SELECTOR]
- [Univerzální systému Windows](mobile-engagement-windows-store-integrate-engagement.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-integrate-engagement.md)

Tento postup popisuje nejjednodušší způsob, jak aktivovat společnosti zapojení technologie pro analýzu a sledování funkce v aplikaci iOS.

Zapojení SDK vyžaduje iOS6 + a Xcode 8: nasazení cílové aplikace musí být alespoň iOS 6.

> [AZURE.NOTE]
> Pokud opravdu závisí na XCode 7 mohli byste použít [iOS v3.2.4 Engagement SDK](https://aka.ms/r6oouh). Je známý problém v modulu Reach této předchozí verze při spuštění na zařízení s iOS 10 [Integrace modul reach](mobile-engagement-ios-integrate-engagement-reach.md) podrobné informace najdete v článku. Pokud jste se rozhodli použít SDK v3.2.4 a pak stačí Přejít `UserNotifications.framework` importovat v dalším kroku.

Tyto kroky jsou tak, abyste aktivovat sestav protokolů potřebné pro výpočet všech statistických údajů týkajících se uživatelů, relací, aktivity, dojde k chybě a Technicals. Sestava protokoly potřebné pro výpočet dalších statistiky jako události, chyby a úlohy musí udělat ručně pomocí rozhraní API zapojení (postup [pomocí rozšířeného Mobile zapojení přiřazování značek k rozhraní API aplikace v iOS aplikace](mobile-engagement-ios-use-engagement-api.md) od tyto statistiky jsou závisí na aplikaci.

##<a name="embed-the-engagement-sdk-into-your-ios-project"></a>Vložení SDK zapojení do projektu iOS

- Stahování iOS SDK [tady](http://aka.ms/qk2rnj).

- Přidání SDK zapojení do projektu iOS: v Xcode, klikněte pravým tlačítkem myši na projektu a vyberte **"Souborů, které chcete přidat..."** a zvolte `EngagementSDK` složky.

- Zapojení vyžaduje další předlohy pro práci: v aplikaci project explorer otevřete podokno projektu a vyberte správné cílové. Potom otevřete kartu **"Sestavení fáze"** a v nabídce **"binární s knihovnami"** přidejte tyto rámce:

    -   `UserNotifications.framework`– nastavení na odkaz jako`Optional`
    -   `AdSupport.framework`– nastavení na odkaz jako`Optional`
    -   `SystemConfiguration.framework`
    -   `CoreTelephony.framework`
    -   `CFNetwork.framework`
    -   `CoreLocation.framework`
    -   `libxml2.dylib`

> [AZURE.NOTE] Rozhraní AdSupport je možné odebrat. Zapojení musí tento rámec shromažďovat IDFA. Však může zakázat IDFA kolekce \<ios sdk zapojení idfa\> dodržovat nové zásady společnosti Apple týkající se tohoto ID.

##<a name="initialize-the-engagement-sdk"></a>Inicializace zapojení SDK

Potřebujete změnit delegáta aplikace:

-   V horní části souboru implementaci importujte agenta můžete zapojit:

        [...]
        #import "EngagementAgent.h"

-   Inicializace zapojení do metodu "**applicationDidFinishLaunching:**"nebo"**aplikace: didFinishLaunchingWithOptions:**":

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
          [...]
          [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
          [...]
        }

##<a name="basic-reporting"></a>Základní vytváření sestav

### <a name="recommended-method-overload-your-uiviewcontroller-classes"></a>Doporučená metoda: přetížení vaše `UIViewController` třídy

Pro aktivaci sestavy všechny požadované zapojení pro výpočet uživatelů, relací, aktivity, dojde k chybě a technických statistických protokoly, můžete jednoduše vytvoříte všechny vaše `UIViewController` dílčí třídy dědí `EngagementViewController` třídy (stejné pravidlo pro `UITableViewController`  - \> `EngagementTableViewController`).

**Bez můžete zapojit:**

    #import <UIKit/UIKit.h>

    @interface Tab1ViewController : UIViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

**S můžete zapojit:**

    #import <UIKit/UIKit.h>
    #import "EngagementViewController.h"

    @interface Tab1ViewController : EngagementViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

### <a name="alternate-method-call-startactivity-manually"></a>Alternativní metody: volání `startActivity()` ručně

Když nemůžete nebo nechcete přetížení vaše `UIViewController` třídy, můžete místo toho začít vašich aktivit najdete tak, že zavoláte `EngagementAgent`společnosti metody přímo.

> [AZURE.IMPORTANT] IOS SDK automaticky volá `endActivity()` metody, když máte zavřený aplikace. Je tedy *DŮRAZNĚ* doporučujeme volání `startActivity` metoda kdykoli aktivity uživatele změnit a *nikdy* volání `endActivity` metody, protože volání tato metoda vynutí aktuální relaci ukončit.

##<a name="location-reporting"></a>Vytváření sestav umístění

Apple podmínky poskytování služeb není dovoleno aplikací pro použití sledování za účelem statistiky pouze umístění. Proto doporučujeme umístění sestavy Povolit jenom v případě, že aplikace taky používat sledování z jiného důvodu umístění.

Začnete s iOS 8, je nutné zadat popis použití služby umístění aplikace nastavením řetězec klávesy [NSLocationWhenInUseUsageDescription] nebo [NSLocationAlwaysUsageDescription] v souboru Info.plist vaše aplikace. Pokud budete chtít umístění sestavy na pozadí s zapojení, přidejte klíč NSLocationAlwaysUsageDescription. Ve všech ostatních případech přidejte klíč NSLocationWhenInUseUsageDescription.

### <a name="lazy-area-location-reporting"></a>Vytváření sestav umístění pustí oblast

Vytváření sestav umístění pustí oblasti umožňuje vykazování zemi, oblasti a místo přidružené k zařízení. Tento typ umístění vykazování používá pouze síťová umístění (na základě ID buňky nebo přes Wi-Fi). Oblasti zařízení je uveden nejvíce jednou za relace. GPS nepoužívali, a tento typ sestavy umístění dostane velmi málo (pozor, abyste uveden bez) dopad na battery.

Nahlášeného oblasti slouží k výpočtu zeměpisné statistických údajů o uživatelích, relace, událostí a chyb. Můžete taky používají jako kritéria v Reach kampaní. Oblasti poslední známé vykázaného za zařízení můžete načíst díky rozhraní [API zařízení].

Má povolit nahlašování umístění pustí oblast, přidejte za inicializace agenta zapojení následující řádek:

    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
    {
      [...]
      [[EngagementAgent shared] setLazyAreaLocationReport:YES];
      [...]
    }

### <a name="real-time-location-reporting"></a>Vytváření sestav umístění reálném čase

Vytváření sestav umístění reálném čase umožňuje vykazování šířky a délky přidružené k zařízení. Ve výchozím nastavení tento typ umístění vykazování pouze používá síťová umístění (na základě ID buňku nebo přes Wi-Fi) a vykazování je jenom aktivní při spuštění aplikace v popředí (tedy během relace).

Umístění reálném čase jsou *není* používá pro výpočet statistiky. Jejich pouze účel umožňující použití geo oddělení reálném čase \<Reach cílové skupiny geofencing\> kritéria v Reach kampaní.

Povolit vykazování času na reálná umístění projektu, přidejte za inicializace agenta zapojení následující řádek:

    [[EngagementAgent shared] setRealtimeLocationReport:YES];

#### <a name="gps-based-reporting"></a>Na základě GPS vytváření sestav

Ve výchozím nastavení hlášení umístění reálném čase používá pouze na základě síťových umístěních. Pokud chcete povolit používání GPS na základě umístění, (které jsou úplně přesnější), přidejte:

    [[EngagementAgent shared] setFineRealtimeLocationReport:YES];

#### <a name="background-reporting"></a>Vytváření sestav pozadí

Ve výchozím nastavení hlášení umístění reálném čase je aktivní pouze při spuštění aplikace v popředí (tedy během relace). Povolíte také vykazování pozadí, přidejte:

    [[EngagementAgent shared] setBackgroundRealtimeLocationReport:YES withLaunchOptions:launchOptions];

> [AZURE.NOTE] Při spuštění aplikace na pozadí, jen síť na základě umístění vznikly, i když jste povolili GPS.

Provedení této funkce zavolá [startMonitoringSignificantLocationChanges] při aplikace přejde do pozadí. Mějte na paměti, že ji bude automaticky znovu spusťte aplikace do pozadí Pokud obdržíte událost nastavit jako nové umístění.

##<a name="advanced-reporting"></a>Rozšířené možnosti vytváření sestav

Volitelně, pokud chcete do sestavy aplikace zvláštní události, chyby a úlohy, potřebujete rozhraní API zapojení pomocí metody `EngagementAgent` předmětu. Objekt této třídy lze získat tak, že zavoláte `[EngagementAgent shared]` statické metody.

Rozhraní API zapojení umožňuje používat všechny společnosti zapojení pokročilých funkcí a podrobné v dialogu jak používat rozhraní API zapojení IOS (stejně jako v technickou dokumentaci `EngagementAgent` třídy).

##<a name="disable-idfa-collection"></a>Zakázání IDFA kolekce

Ve výchozím nastavení použije zapojení [IDFA] identifikují uživatele. Ale pokud nepoužíváte reklamní jinde v seznamu aplikací, může odmítnuto revize procesem App Storu. Kolekce IDFA můžete zakázat přidáním preprocesor makro `ENGAGEMENT_DISABLE_IDFA` v souboru pch (nebo v `Build Settings` aplikace). Zajistíte tím, že je žádné odkazy na `ASIdentifierManager`, `advertisingIdentifier` nebo `isAdvertisingTrackingEnabled` v dialogu sestavení aplikace.

Integrace **prefix.pch** souboru:

    #define ENGAGEMENT_DISABLE_IDFA
    ...

Můžete ověřit, že kolekci IDFA správně zakázána v aplikaci kontrolou protokoly test Engagement. V tématu Integrace testovací\<ios-sdk zapojení test-idfa\> si přečtěte následující dokumentaci pro další informace.

##<a name="disable-log-reporting"></a>Zakázání protokol

### <a name="using-a-method-call"></a>Pomocí metody hovoru

Pokud budete potřebovat zapojení zastavit odesílání protokoly, můžete volat:

    [[EngagementAgent shared] setEnabled:NO];

Volání je trvalý: použije `NSUserDefaults` pro uložení údajů.

Povolit vykazování znovu tak, že zavoláte stejnou funkci pomocí protokolu `YES`.

### <a name="integration-in-your-settings-bundle"></a>Integrace v nastavení skupiny

Místo volání tuto funkci, můžete také integrovat toto nastavení používají přímo ve vašem stávajícím `Settings.bundle` soubor. Řetězec `engagement_agent_enabled` je nutné použít jako čímž předvoleb identifikátor musí být přidružené k přepnout přepínač (`PSToggleSwitchSpecifier`).

Následující příklad `Settings.bundle` ukazuje, jak implementovat ho:

    <dict>
        <key>PreferenceSpecifiers</key>
        <array>
            <dict>
                <key>DefaultValue</key>
                <true/>
                <key>Key</key>
                <string>engagement_agent_enabled</string>
                <key>Title</key>
                <string>Log reporting enabled</string>
                <key>Type</key>
                <string>PSToggleSwitchSpecifier</string>
            </dict>
        </array>
        <key>StringsTable</key>
        <string>Root</string>
    </dict>

<!-- URLs. -->
[Zařízení rozhraní API]: http://go.microsoft.com/?linkid=9876094
[NSLocationWhenInUseUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26
[NSLocationAlwaysUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18
[startMonitoringSignificantLocationChanges]:http://developer.apple.com/library/IOs/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html#//apple_ref/occ/instm/CLLocationManager/startMonitoringSignificantLocationChanges
[IDFA]:https://developer.apple.com/library/ios/documentation/AdSupport/Reference/ASIdentifierManager_Ref/ASIdentifierManager.html#//apple_ref/occ/instp/ASIdentifierManager/advertisingIdentifier
