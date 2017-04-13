<properties
    pageTitle="Začínáme s Azure mobilní zapojení pro Cordova/Phonegap"
    description="Zjistěte, jak pomocí služby Azure Mobile zapojení technologie pro analýzu a nabízená oznámení pro aplikace Cordova/Phonegap."
    services="mobile-engagement"
    documentationCenter="Mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-phonegap"
    ms.devlang="js"
    ms.topic="hero-article" 
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-cordovaphonegap"></a>Začínáme s Azure mobilní zapojení pro Cordova/Phonegap

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Toto téma ukazuje, jak používat Azure Mobile zapojení pomůže porozumět použití aplikace a odešlete Segmentovaný uživatelů pro mobilní aplikaci vyvinuté s Cordova nabízená oznámení.

V tomto kurzu budeme vytvoříte z prázdné aplikace Cordova počítači Mac a integrace SDK zapojení Mobile. Shromažďuje základní technologie pro analýzu dat a přijímání nabízená oznámení pomocí Apple Push upozornění systému (APNS) pro iOS a zpráv Cloud Google (GCM) pro Android. Jsme nasadí s iOS nebo Android zařízení pro účely testování. 

> [AZURE.NOTE] Tento kurz, musíte mít účet Azure active. Pokud nemáte účet, můžete vytvořit bezplatný účet zkušební v jenom pár minut. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-cordova-get-started).

Tento kurz vyžaduje:

+ XCode, které si můžete nainstalovat z App Store pro Mac. (pro nasazení na iOS)
+ [Android SDK & emulátoru](http://developer.android.com/sdk/installing/index.html) (pro nasazení na Android)
+ Nabízená oznámení certifikát (p12), který je můžete získat od Centrum pro vývojáře Apple APNS
+ Číslo GCM projektu, kterou můžete získat z konzoly Developer Google pro GCM
+ [Modul plug-in Cordova mobilní zapojení](https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-engagement)

> [AZURE.NOTE] Zdrojový kód a soubor ReadMe pro najdete modul plug-in Cordova na [Github](https://github.com/Azure/azure-mobile-engagement-cordova)

##<a id="setup-azme"></a>Nastavení mobilních zapojení Cordova aplikace

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Připojení k back-end zapojení mobilní aplikace

Tento kurz nabízí "základní integrace" tedy s minimálně nastavte požadované shromažďování dat a odeslání nabízená oznámení. 

Vytvoříme základní aplikace s Cordova prokázat integrace:

###<a name="create-a-new-cordova-project"></a>Vytvoření nového projektu Cordova

1. Spusťte okně *terminálu* ve vašem počítači Mac a zadejte následující, která vytvoří nový projekt Cordova z výchozí šablonu. Ujistěte se, že publikování profil, který se nasadit pomocí aplikace pro iOS používá "com.mycompany.myapp" jako ID aplikace. 

        $ cordova create azme-cordova com.mycompany.myapp
        $ cd azme-cordova

2. Spuštění podle následujících pokynů nakonfigurujte projektu pro **iOS** a spusťte v iOS Simulator:

        $ cordova platform add ios 
        $ cordova run ios

3. Provedení podle následujících pokynů nakonfigurujte projektu pro **Android** a spusťte v Android emulátoru. Ověření, zda nastavení Android emulátoru SDK jeho cílové jako rozhraní API Google (Google Inc.) s procesoru / ABI jako ARM rozhraní API Google.  

        $ cordova platform add android
        $ cordova run android

4.  Přidání modulu plug-in Cordova konzoly. 

        $ cordova plugin add cordova-plugin-console 

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Připojení aplikace k mobilní zapojení back-end

1. Nainstalujte modul plug-in Azure Mobile zapojení Cordova zároveň hodnotách proměnných nakonfigurovat modul plug-in:

        cordova plugin add cordova-plugin-ms-azure-mobile-engagement    
            --variable AZME_IOS_CONNECTION_STRING=<iOS Connection String> 
            --variable AZME_IOS_REACH_ICON=... (icon name WITH extension) 
            --variable AZME_ANDROID_CONNECTION_STRING=<Android Connection String> 
            --variable AZME_ANDROID_REACH_ICON=... (icon name WITHOUT extension)       
            --variable AZME_ANDROID_GOOGLE_PROJECT_NUMBER=... (From your Google Cloud console for sending push notifications) 
            --variable AZME_ACTION_URL =... (URL scheme which triggers the app for deep linking)
            --variable AZME_ENABLE_NATIVE_LOG=true|false
            --variable AZME_ENABLE_PLUGIN_LOG=true|false

*Android dosáhla ikona* : musí být název zdroje, bez jakékoli rozšíření ani drawable předponu (ex: mynotificationicon), a na ikonu soubor musí být zkopírovány do android projektu (platformách/android/rozlišení/drawable)

*iOS dosáhla ikona* : musí být název zdroje s přípona (ex: mynotificationicon.png), a na ikonu soubor musí být přidán do projektu iOS s XCode (pomocí nabídky přidat soubory)

##<a id="monitor"></a>Povolení sledování v reálném čase

1. V aplikaci project Cordova - upravte **www/js/index.js** přidat přijetí hovoru na mobilní telefon zapojení do nového aktivitu jednou *deviceReady* událost deklarovat.

         onDeviceReady: function() {
                Engagement.startActivity("myPage",{});
            }

2. Spusťte aplikaci:
        
    - **Pro iOS**
    
        V `Terminal` okno spuštění aplikace v nové instance Simulator provedením následujících akcí:

            cordova run ios

    - **Pro Android**
        
        V `Terminal` okno spuštění aplikace v nové instance emulátoru provedením následujících akcí:

            cordova run android

3. Zobrazí se v protokolech konzoly následující kroky:

        [Engagement] Agent: Session started
        [Engagement] Agent: Activity 'myPage' started
        [Engagement] Connection: Established
        [Engagement] Connection: Sent: appInfo
        [Engagement] Connection: Sent: startSession
        [Engagement] Connection: Sent: activity name='myPage'

##<a id="monitor"></a>Připojení aplikace s sledování v reálném čase

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Povolení služby nabízená oznámení a zasílání zpráv v aplikaci

Zapojení Mobile umožňuje pracovat s jinými uživateli pomocí služby nabízená oznámení a aplikace pro zasílání zpráv v rámci kampaní. Tento modul nazývá REACH na portálu Mobile Engagement.
V následujících částech se instalační program aplikace k jejich odběru.

###<a name="configure-push-credentials-for-mobile-engagement"></a>Nastavení nabízených přihlašovací údaje pro zapojení Mobile

Pokud chcete, aby Mobile zapojení odeslat nabízená oznámení vaším jménem, budete muset udělit přístup k počítači Apple iOS certifikát nebo GCM serveru rozhraní API klíč. 
    
1. Přejděte na portál zapojení Mobile. Ujistěte se, že jste v aplikaci jsme používáte pro tento projekt a potom klikněte na tlačítko **Engage** dole:
    
    ![][1]
    
2. Na stránce nastavení se má v portálu Engagement. Z viditelné klikněte v části **Nativní nabízená** :
    
    ![][2]

3. Konfigurace iOS certifikát/GCM serveru rozhraní API klíč

    **[iOS]**

    na. Vyberte svůj P12, ho nahrajte a zadejte svoje heslo:
    
    ![][3]

    **[Android]**

    na. Klikněte na ikonu úpravy před **Klíč rozhraní API** v části Nastavení GCM a v místní, který ukazuje, vložte klávesu GCM serveru a klikněte na tlačítko **OK**. 
        
    ![][4]

###<a name="enable-push-notifications-in-the-cordova-app"></a>Povolení nabízená oznámení v aplikaci Cordova

Úprava **www/js/index.js** přidáte hovor na mobilní telefon zapojení do žádosti o nabízená oznámení a deklarovat obslužné rutiny:

     onDeviceReady: function() {
           Engagement.initializeReach(  
                // on OpenUrl  
                function(_url) {   
                alert(_url);   
                });  
            Engagement.startActivity("myPage",{});  
        }

###<a name="run-the-app"></a>Spusťte aplikaci

**[iOS]**

1. Použijeme XCode sestavovat a nasazovat aplikaci v zařízení vyzkoušet nabízená oznámení, protože iOS umožňuje pouze nabízená oznámení na aktuální zařízení. Přejděte do umístění, kde je vytvořen Cordova projektu a přejděte do umístění **...\platforms\ios** . Otevřete soubor nativní .xcodeproj XCode. 
    
2. Vytvořte a nasaďte aplikaci Cordova zařízení iOS pomocí účtu, který má na zřizovací profilu obsahující certifikát, že který jste právě nahráli portálu zapojení Mobile a Id aplikace které shody tu jste zadali při vytváření aplikace Cordova. Můžete se podívat *příruček identifikátor* vaší **zdroje\*-info.plist** souboru v XCode podle. 

3. Zobrazí se v rozevírací nabídce standardní iOS na vašem zařízení oznamující, že aplikace žádosti o oprávnění k odeslání oznámení. Udělte oprávnění. 

**[Android]**

Můžete jednoduše použijete emulátor ke spuštění aplikace Androidu jako GCM oznámení jsou podporovány na Android emulátoru. 

    cordova run android

##<a id="send"></a>Odeslání oznámení pro aplikace

Nyní vytvoříme jednoduchý kampaň nabízená oznámení, která odešle push aplikace spuštěné v zařízení:

1. Přejděte na kartu **dosáhla** v portálu zapojení Mobile

2. Klikněte na vytvořit kampaň nabízená **Oznámení narození**

    ![][6]

3. Zadejte vstupní hodnoty pro vytvoření kampaně **[Android]**
    
    - Zadejte **název** pro kampaň. 
    - Vyberte **Typ dodávky** jako *oznámení systému* *jednoduché*
    - Vyberte **čas doručení** jako *"libovolného Time"*
    - Zadejte **název** pro vaše oznámení, které budou prvního řádku nabízených.
    - Zadejte **zprávu** pro vaše oznámení, které bude sloužit jako text zprávy. 

    ![][11]

4. Zadejte vstupní hodnoty pro vytvoření kampaně **[iOS]**

    - Zadejte **název** pro kampaň. 
    - Vyberte **čas doručení** jako *"mimo aplikaci jenom"*
    - Zadejte **název** pro vaše oznámení, které budou prvního řádku nabízených.
    - Zadejte **zprávu** pro vaše oznámení, které bude sloužit jako text zprávy. 
 
    ![][12]

5. Posuňte se dolů a v části obsah vyberte **pouze oznámení**

    ![][8]

6. [Volitelné] Můžete taky poskytnout adresa URL akce. Ujistěte se, že používá schéma adresy URL k dispozici při konfiguraci modulu plug-in **AZME\_PŘESMĚROVAT\_URL** proměnné například *myapp://test*.  

7. Dokončení možné nastavení základní kampaní. Teď znovu přejděte dolů a klikněte na tlačítko **vytvořit** a uložit kampaně.

8. Nakonec **Aktivovat** kampaň
    
    ![][10]

9. Teď byste měli vidět nabízená oznámení na zařízení nebo emulátoru jako součást této kampaně. 

##<a id="next-steps"></a>Další kroky
[Základní informace o všech metod dostupných s Cordova Mobile zapojení SDK](https://github.com/Azure/azure-mobile-engagement-cordova)

<!-- Images. -->

[1]: ./media/mobile-engagement-cordova-get-started/engage-button.png
[2]: ./media/mobile-engagement-cordova-get-started/engagement-portal.png
[3]: ./media/mobile-engagement-cordova-get-started/native-push-settings.png
[4]: ./media/mobile-engagement-cordova-get-started/api-key.png
[6]: ./media/mobile-engagement-cordova-get-started/new-announcement.png
[8]: ./media/mobile-engagement-cordova-get-started/campaign-content.png
[10]: ./media/mobile-engagement-cordova-get-started/campaign-activate.png
[11]: ./media/mobile-engagement-cordova-get-started/campaign-first-params-android.png
[12]: ./media/mobile-engagement-cordova-get-started/campaign-first-params-ios.png

