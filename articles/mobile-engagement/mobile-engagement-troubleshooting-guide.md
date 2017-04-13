<properties 
   pageTitle="Poradce při potížích s vodítka Azure mobilní zapojení" 
   description="Příručka pro řešení potíží pro Azure mobilní zasunutí" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="08/19/2016"
   ms.author="piyushjo"/>

# <a name="azure-mobile-engagement---troubleshooting-guide"></a>Azure mobilní zapojení – Průvodce pro řešení

## <a name="introduction"></a>Úvod
Následující Poradce při potížích Průvodce vám pomůže porozumět tomu, že příčin některé často příručky problémů a bude umožňují Poradce při potížích s vlastními. 

## <a name="general"></a>Obecné

Obecně měli byste vždy zajistit takto:

1. Ujistěte se, že máte pryč všemi všechny kroky pro integraci podle popisu v naší [výukové programy Začínáme](mobile-engagement-windows-store-dotnet-get-started.md)
2. Používáte nejnovější verzi platformu SDK. 
3. Test na aktuální zařízení a emulátoru protože jsou specifické pro emulátoru jenom některé problémy. 
4. Nejsou zasažení všechny limity/omezením z mobilních zapojení které jsou popsány [tady](../azure-subscription-service-limits.md)
5. Pokud jste není možné se připojit k zapojení Mobile service back-end nebo vidět dat nejsou nepřetržitě načíst, potom zajistit, aby byly kontrolou žádné probíhající servisním incidentem [tady](https://azure.microsoft.com/status/)

## <a name="monitor-issues"></a>"Sledování problémů

### <a name="i-am-not-seeing-my-device-showing-up-on-the-monitor-tab"></a>Se nezobrazují zařízení neprojevují na kartě Monitor
Zobrazuje kartu monitor zařízení připojených k svoji platformu Mobile zapojení v reálném čase. Pokud jsou ladění na emulace a zařízení, byste měli vidět tady aspoň jednu relaci. Pokud má distribuované aplikace, uvidíte obrys aktivní relace odrážet zařízení, které jsou spojeny s informacemi o platformě v reálném čase. 

Pokud nevidíte zařízení na kartě Monitor je pravděpodobně problém integrace SDK. Některé běžné kroky k řešení je takto:

1. Ujistěte se, že používáte správné připojovací řetězec v mobilní aplikaci a je z části SDK klíče a ne v části rozhraní API klíče. Připojovací řetězec připojí mobilní aplikace instanci aplikace zapojení Mobile, ve kterém uvidíte zařízení na kartě Monitor. 
2. Pro platformu Windows už – Pokud přepíše stránku `OnNavigatedTo` metoda, ujistěte se, že volání `base.OnNavigatedTo(e)`.
3. Pokud jsou integrace Mobile zapojení do existující mobilní aplikace a pak můžete zajistit, že nepřijdete všechny kroky vyhledáním kroky rozšířené integrace [tady](mobile-engagement-windows-store-integrate-engagement.md)
4. Zkontrolujte, zda že odesíláte alespoň jeden obrazovky/aktivity přepsáním na stránku s EngagementActivity podle toho, kterou pracujete, jak je uvedeno v [Začínáme výukové programy pro](mobile-engagement-windows-store-dotnet-get-started.md)platformu.

### <a name="i-am-seeing-the-monitor-tab-showing-a-session-even-when-i-have-disconnected-or-closed-my-app-emulator"></a>Zobrazují se mi kartě Monitor zobrazující relaci i když mám odpojení nebo zavření aplikace Moje / emulátoru. 
Pokud jste připojení k platformu v tomto okamžiku pouze jeden a používáte emulátoru otevřete aplikaci je to pravděpodobně kvůli emulátoru quirks. Obecně je třeba zajistit, že se vraťte na úvodní obrazovce v PDC relaci aplikace úspěšně odpojit. Navíc na platformu Windows už během ladění s Visual Studio můžete může být nutné ověřit, že ve Visual Studiu přejdete na řádku nabídek **Životního cyklu události** a klikněte na **Pozastavit** skutečně ukončíte relace. Další informace v tématu [kurz Windows](mobile-engagement-windows-store-dotnet-get-started.md) . 

## <a name="analytics-issues"></a>"Analýzy" problémy

### <a name="i-am-not-seeing-any-data-refreshed-data-on-analytics-tab"></a>Nezobrazují žádná data / aktualizaci dat na kartě analýza 
Přepočítá se technologie pro analýzu dat v pravidelných intervalech a může trvat až 24 hodin tato aktualizace. Tato data není reálném čase a zobrazí se že aktualizovat v rámci tohoto 24 hodin časové období.
Ujistěte se, ale, že odesíláte alespoň jednu obrazovku nebo aktivity back-end platformy tak, že některý z přepsání alespoň jednu stránku s `EngagementActivity` nebo volání `SendActivity` explcitly. 

### <a name="i-am-seeing-incorrectly-captured-datetime-for-a-device-on-the-analytics-tab"></a>Zobrazují se mi nesprávně zachycený data a času pro zařízení na kartě analýza
Časové období analýzy je na základě data z nastavení zařízení uživatelů. Takže zajistěte zařízení správně nastavená datum. 

## <a name="segment-issues"></a>"Segmentu" problémy

### <a name="i-created-a-segment-and-it-is-showing-up-as-greyed-out-or-not-showing-any-data"></a>Vytvoření segmentu a je neprojevují zašedlé nebo není zobrazena všechna data
Vytvoření segmentu není v reálném čase v okamžiku. Se vypočítá ve stejnou dobu jako agregované technologie pro analýzu dat a může trvat až 24 hodin. Měli byste zkontrolovat zpět později, ale mezitím měli byste taky zajistit, že vaše mobilní aplikace jsou skutečně odesílání dat na základě kterého jsou tvořící segmenty. Například Pokud události vyslovte příkaz "foo" nebyla odeslána pomocí mobilního zařízení a pak by pak nebyl žádná data segmentu segmentu vytvořená pomocí EventName = foo jako kritérium. Také byste měli zkontrolovat, že SDK integrace zajistit mobilní aplikace odesílá data správně. 

## <a name="reach-or-push-notifications-issues"></a>Problémy s "Dosáhl" nebo nabízená oznámení

### <a name="my-push-messages-are-not-being-delivered"></a>Nejsou doručována zprávách připínáčku 

1. Pokuste se odeslat oznámení na zařízení test nejdřív zajistit všechny komponenty – mobilní aplikace, SDK a službách správně připojení a moct předvádění nabízená oznámení. 
2. Vždy odeslat nejjednodušší "mimo aplikaci oznámením" nejdřív kampaně, který není naplánován a ani má libovolné kritérium cílové skupiny zadané. To je znovu prokážete, že oznámení připojení funguje zařízení správně. 
3. Pokud máte potíže v doručování oznámení v aplikaci také je dobré prvním krokem zkusit nejdřív odesílání oznámení mimo aplikaci. 
4. Ujistěte se, že "nativní nabízená" správně nakonfigurovaný pro mobilní aplikaci. V závislosti na platformu ho buď zahrnuje klávesy (Android, Windows) nebo certifikáty (iOS). Zobrazit [uživatelské rozhraní – nastavení](mobile-engagement-user-interface-settings.md)
5. Mimo aplikaci, kterou oznámení může taky budou blokovány uživatelem přes mobilní operační systém tak zajistěte, aby že to neplatí pro. 
6. Ujistěte se, že nenastavujete *Ignorovat cílové skupiny nabízených se pošle uživatelů prostřednictvím rozhraní API* možnost v části **kampaně** kampaně Reach vzhledem k tomu, že zajistíte, že nabízená oznámení může odeslána pouze prostřednictvím rozhraní API. 
7. Ujistěte se, že testujete kampaně nabízených pomocí obou zařízení připojeného prostřednictvím Wi-Fi a telefonní sítě operátor eliminuje síťové připojení jako zdroj možné problémy.
8. Zajistěte, aby byl systémové datum a čas v zařízení/PDC správné protože nejsou synchronizovány zařízení také rušit možnost nabízená oznámení služby pro doručování oznámení. 

Další specifický pro platformu Poradce při potížích s pokynů níže:

1. **iOS** 

    - Ujistěte se, že certifikáty jsou platné a zbývající pro iOS nabízená oznámení. 
    - Ujistěte se, že správně konfigurujete *výrobní* certifikát v aplikaci Mobile Engagement. 
    - Ujistěte se, že testujete na *zařízení real, fyzické.* IOS simulator se nedají zpracovat nabízených zprávy.
    - Ověřte, že identifikátor příruček správně nakonfigurované v mobilní aplikaci. Postupujte podle pokynů [v tomto poli](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)
    - Při testování, použijte "Ad Hoc" rozdělení ve vašem mobilním zřizovací profilu. Nebudete moct přijímat oznámení, pokud aplikace kompilaci pomocí "Ladění"

2. **Android**

    - Ujistěte se, že jste zadali správné číslo projektu v mobilní aplikaci AndroidManifest.xml souboru, který následuje znak \n. 
    
            <meta-data android:name="engagement:gcm:sender" android:value="************\n" />
        
    - Zajistit, aby nebyly nalezeny nebo chybně nakonfigurované všechna oprávnění v souboru Android Manifest 
    - Zajistěte, aby byl číslo projektu, který přidáváte do aplikace klienta z jednoho účtu místo, kam jste získali klávesu GCM serveru. Všechny neshodu mezi těmito dvěma zabrání vaší posune ven.. 
    - Pokud dostáváte upozornění systému, ale ne v aplikaci a revize [zadat ikona pro část oznámení](mobile-engagement-android-get-started.md) jako pravděpodobně nejsou správné ikony určující v souboru Android Manifest. 
    - Odesílání oznámení BigPicture a nakonec zajistit, že pokud máte externí obrázek servery pak budou muset mít možnost podpory HTTP "GET" a "Záhlaví".

3. **Windows**
    
    - Ujistěte se, že jste aplikaci přidružené platná aplikace pro Windows Store. Ve Visual Studiu - budete muset klikněte pravým tlačítkem myši projektu a vyberte možnost "Spojit aplikace s úložiště přihlašovacích údajů" a vyberte aplikaci, kterou jste vytvořili ve Windows Storu. Tato aplikace pro Windows Store se musí shodovat jednu z místo, kam jste získali nativní nabízených přihlašovací údaje pro nastavení portálu Mobile Engagement.
    - Pokud se vám zobrazuje mimo aplikaci nabízená oznámení, ale ne aplikace oznámení s `EngagementOverlay` integrace potom zajistit, aby je kořenový prvek mřížky na stránce. EngagementOverlay použije první prvek "Tabulky" najde v souboru xaml přidat dvěma web zobrazí na stránce. Pokud chcete zjistit, kde bude nastavení zobrazení web, můžete definovat síť s názvem "EngagementGrid", jako se to ale budete muset zkontrolujte, zda je dostatečně výška a šířka pro dva další webové zobrazení, které se zobrazí oznámení a následující oznámení jako oznámení ve aplikace:
        
            <Grid x:Name="EngagementGrid"></Grid>

### <a name="i-created-a-push-notificationannouncement-campaign-and-even-after-it-sent-me-the-notification-it-is-showing-as-active-what-does-it-mean"></a>Jsem vytvořil(a) nabízená oznámení a oznámení/kampaně a i když je mi odeslat oznámení, je zobrazen jako "Aktivní". Co znamená? 
**Kampaně** , který jste vytvořili v mobilní zapojení se nazývá, protože nová zařízení připojíte k platformy mobilních zapojení je dlouhodobé význam nabízená oznámení, budou automaticky odeslány oznámení, které jste nakonfigurovali, dokud nevyhovují kritéria, které jste nastavili v kampaně. Toto není jeden nastavení snímek jednoho oznamování. Budete muset ručně klikněte na tlačítko **Dokončit** ukončíte kampaně tak, aby ho další neodesílá oznámení. 

### <a name="i-created-a-push-campaign-and-i-am-receiving-notifications-successfully-however-whenever-i-open-up-the-app-i-get-the-same-notification-even-when-i-had-actioned-it-before"></a>Jsem vytvořil(a) nabízených kampaní a zobrazuje oznámení úspěšně však každé otevření instalaci aplikace, můžu získat stejné oznámení i v případě, že došlo k actioned před? 
Toto je pravděpodobně dojde při zkoušení a používáte emulátorů nebo jenom některé otestujte framework jako TestFlight. Co se děje, tady je, že v každé aplikaci spustit instanci se získáním nové DeviceID a odesláním na našeho back-end, který způsobuje platformu zapojení Mobile s ním zacházet jako nové zařízení a posílání oznámení. 

## <a name="getting-support"></a>Získání podpory

Pokud nemůžete tento problém vyřešit sami pak můžete provést tyto akce:

1. Vyhledejte problém v existující podprocesů na [fórum MSDN](https://social.msdn.microsoft.com/Forums/windows/en-US/home?forum=azuremobileengagement) a StackOverflow fóra a pokud nejsou pak zeptejte se tam. 
2. Pokud se vám najít funkci chybějící potom přidat/hlas pro požadavek na naše [UserVoice fórum](https://feedback.azure.com/forums/285737-mobile-engagement/)
3. Pokud máte Microsoft podporují otevřít podpory incidentu zadáním tyto údaje: 
    - ID předplatného Azure
    - Platformu (například iOS, Android atd.)
    - ID aplikace
    - ID kampaně (pro nabízená oznámení problémy)
    - ID zařízení
    - Zapojení SDK mobilní verze (například v2.1.0 Android SDK)
    - Podrobnosti o chybě se přesně chybová zpráva a scénáři
