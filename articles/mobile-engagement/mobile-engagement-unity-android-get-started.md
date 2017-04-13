<properties
    pageTitle="Začínáme s zapojení Azure Mobile pro Android jednotky nasazení"
    description="Zjistěte, jak pomocí služby Azure Mobile zapojení technologie pro analýzu a nabízená oznámení pro aplikace jednotky nasazení na zařízení s iOS."
    services="mobile-engagement"
    documentationCenter="unity"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-unity-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-unity-android-deployment"></a>Začínáme s zapojení Azure Mobile pro Android jednotky nasazení

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

V tomto tématu se dozvíte, jak používat Azure Mobile zapojení pochopit použití aplikace a jak odeslat nabízených oznámení pro segmentována uživatelé aplikace jednotky při instalaci na zařízení s Androidem.
Tento kurz používá klasické jednotky vrátit míč kurz jako výchozí bod. Postupujte podle kroků uvedených v tomto [kurzu](mobile-engagement-unity-roll-a-ball.md) před pokračováním integrace zapojení Mobile, kterou jsme prezentovat v tomto kurzu níže. 

Tento kurz vyžaduje:

+ [Editor jednotky](http://unity3d.com/get-unity)
+ [Mobilní zapojení jednotky SDK](https://aka.ms/azmeunitysdk)
+ Google Android SDK

> [AZURE.NOTE] Tento kurz, musíte mít účet Azure active. Pokud nemáte účet, můžete vytvořit bezplatný účet zkušební v jenom pár minut. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-android-get-started).

##<a id="setup-azme"></a>Nastavení mobilních zapojení pro aplikace pro Android

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Připojení k back-end zapojení mobilní aplikace

###<a name="import-the-unity-package"></a>Import balíček jednotky

1. Stáhnout [balíček Mobile zapojení jednotky](https://aka.ms/azmeunitysdk) a si ji uložit do místního počítače. 

2. Přejděte na **prostředky -> Importovat balíček -> vlastní balíčku** a vyberte balíček jste si stáhli z předchozího kroku. 

    ![][70] 

3. Ujistěte se, budou vybrány všechny soubory a klikněte na tlačítko **importovat** . 

    ![][71] 

4. Po úspěšném Import uvidíte importované SDK soubory do projektu.  

    ![][72] 

###<a name="update-the-engagementconfiguration"></a>Aktualizace EngagementConfiguration

1. Otevřete soubor skriptu **EngagementConfiguration** z SDK složky a aktualizace **ANDROID\_připojení\_řetězec** pomocí připojovacího řetězce jste získali dříve z portálu Microsoft Azure.  

    ![][73]

2. Uložte soubor 

3. Spuštění **Soubor -> zapojení -> Generovat Android Manifest**. Toto je modul plug-in přidal SDK zapojení Mobile a kliknutí na tlačítko se automaticky aktualizuje nastavení projektu. 

    ![][74]

> [AZURE.IMPORTANT] Zkontrolujte, že spustit pokaždé, když aktualizujete **EngagementConfiguration** souboru jinak vaše změny se projeví v seznamu aplikací. 

###<a name="configure-the-app-for-basic-tracking"></a>Konfigurace aplikace pro základní sledování

1. Otevřete skript **PlayerController** připojené k objektu přehrávače pro úpravy. 

2. Přidejte následující pomocí příkazu:

        using Microsoft.Azure.Engagement.Unity;

3. Přidání podle následujících pokynů `Start()` metody
    
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

###<a name="deploy-and-run-the-app"></a>Nasazení a spuštění aplikace
Ujistěte se, že máte v počítači nainstalována před pokusem o nasazení aplikace od tohoto jednotky na zařízení s Androidem SDK. 

1. Připojte zařízení s Androidem do počítače. 

2. Otevřete **Soubor -> Vytvořit nastavení** 

    ![][40]

3. Vyberte **Android** a potom klikněte na **Přepínač platformy**

    ![][51]

    ![][52]

4. Klikněte na **Nastavení** a identifikátor platné příruček. 

    ![][53]

5. Nakonec klikněte na **Vytvořit a spustit**

    ![][54]

6. Můžete být vyzváni k zadání názvu složky pro ukládání balíček Android. 

7. Všechno, co v případě pořádku, balíček má být nasazené na zařízení připojeného a jednotky hra byste měli vidět na telefonu? 

##<a id="monitor"></a>Připojení aplikace s sledování v reálném čase

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Zapnout nabízená oznámení a zasílání zpráv v aplikaci

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

###<a name="update-the-engagementconfiguration"></a>Aktualizace EngagementConfiguration

1. Otevřete soubor skriptu **EngagementConfiguration** z SDK složky a aktualizace **ANDROID\_GOOGLE\_číslo** **Číslo projektu Google** jste získali dříve z portálu pro vývojáře Cloud Google. Toto je řetězec hodnotu proto se ujistěte, uzavřete ho do dvojitých uvozovkách. 

    ![][75]

2. Uložte soubor. 

3. Spuštění **Soubor -> zapojení -> Generovat Android Manifest**. Toto je modul plug-in přidal SDK zapojení Mobile a kliknutí na tlačítko se automaticky aktualizuje nastavení projektu. 

    ![][74]

###<a name="configure-the-app-to-receive-notifications"></a>Konfigurace aplikace pro příjem oznámení ve formě

1. Otevřete skript **PlayerController** připojené k objektu přehrávače pro úpravy. 

2. Přidání podle následujících pokynů `Start()` metody

        EngagementReachAgent.Initialize();

3. Teď, když se aktualizuje aplikace, nasazení a spustit aplikaci na zařízení podle pokynů uvedených níže. 

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[40]: ./media/mobile-engagement-unity-android-get-started/40.png
[70]: ./media/mobile-engagement-unity-android-get-started/70.png
[71]: ./media/mobile-engagement-unity-android-get-started/71.png
[72]: ./media/mobile-engagement-unity-android-get-started/72.png
[73]: ./media/mobile-engagement-unity-android-get-started/73.png
[74]: ./media/mobile-engagement-unity-android-get-started/74.png
[75]: ./media/mobile-engagement-unity-android-get-started/75.png
[51]: ./media/mobile-engagement-unity-android-get-started/51.png
[52]: ./media/mobile-engagement-unity-android-get-started/52.png
[53]: ./media/mobile-engagement-unity-android-get-started/53.png
[54]: ./media/mobile-engagement-unity-android-get-started/54.png
