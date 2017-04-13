<properties
    pageTitle="Začínáme s Azure Mobile zapojení jednotky iOS nasazení"
    description="Zjistěte, jak pomocí služby Azure Mobile zapojení technologie pro analýzu a nabízená oznámení pro aplikace jednotky nasazení na zařízení s iOS."
    services="mobile-engagement"
    documentationCenter="unity"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-unity-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-unity-ios-deployment"></a>Začínáme s Azure Mobile zapojení jednotky iOS nasazení

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

V tomto tématu se dozvíte, jak používat Azure Mobile zapojení pochopit použití aplikace a jak odeslat nabízených oznámení pro segmentována uživatelé aplikace jednotky při instalaci na zařízení s iOS.
Tento kurz používá klasické jednotky vrátit míč kurz jako výchozí bod. Postupujte podle kroků uvedených v tomto [kurzu](mobile-engagement-unity-roll-a-ball.md) před pokračováním integrace zapojení Mobile, kterou jsme prezentovat v tomto kurzu níže. 

Tento kurz vyžaduje:

+ [Editor jednotky](http://unity3d.com/get-unity)
+ [Mobilní zapojení jednotky SDK](https://aka.ms/azmeunitysdk)
+ XCode Editor

> [AZURE.NOTE] Tento kurz, musíte mít účet Azure active. Pokud nemáte účet, můžete vytvořit bezplatný účet zkušební v jenom pár minut. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-ios-get-started).

##<a id="setup-azme"></a>Nastavit mobilní zapojení pro aplikace pro iOS

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

1. Otevřete soubor skriptu **EngagementConfiguration** z SDK složky a aktualizace **IOS\_připojení\_řetězec** pomocí připojovacího řetězce jste získali dříve z portálu Microsoft Azure.  

    ![][73]

2. Uložte soubor. 

###<a name="configure-the-app-for-basic-tracking"></a>Konfigurace aplikace pro základní sledování

1. Otevřete skript **PlayerController** připojené k objektu přehrávače pro úpravy. 

2. Přidejte následující pomocí příkazu:

        using Microsoft.Azure.Engagement.Unity;

3. Přidání podle následujících pokynů `Start()` metody
    
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

###<a name="deploy-and-run-the-app"></a>Nasazení a spuštění aplikace

1. Připojte zařízení s iOS do počítače. 

2. Otevřete **Soubor -> Vytvořit nastavení** 

    ![][40]

3. Vyberte **iOS** a potom klikněte na **Přepínač platformy**

    ![][41]

    ![][42]

4. Klikněte na **Nastavení** a identifikátor platné příruček. 

    ![][53]

5. Nakonec klikněte na **Vytvořit a spustit**

    ![][54]

6. Můžete být vyzváni k zadání názvu složky pro ukládání balíček iOS. 

    ![][43]

7. Všechno, co v případě pořádku, bude kompilovaný projektu a je vhodné otevřít ho v aplikaci XCode. 

8. Ujistěte se, **sbalení identifikátor** správnost v projektu.  

    ![][75]

10. Spusťte aplikaci v XCode tak, aby zařízení připojeného po nasazení balíčku a jednotky hra byste měli vidět na telefonu? 

##<a id="monitor"></a>Připojení aplikace s sledování v reálném čase

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Zapnout nabízená oznámení a zasílání zpráv v aplikaci

Zapojení Mobile umožňuje pracovat s jinými uživateli a dosáhnout s nabízená oznámení a aplikace pro zasílání zpráv v rámci kampaní. Tento modul nazývá REACH na portálu Mobile Engagement.
Nemusíte udělat žádnou další konfiguraci v aplikaci pro příjem oznámení ve formě a už je spuštěný instalace.

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- Images. -->
[40]: ./media/mobile-engagement-unity-ios-get-started/40.png
[41]: ./media/mobile-engagement-unity-ios-get-started/41.png
[42]: ./media/mobile-engagement-unity-ios-get-started/42.png
[43]: ./media/mobile-engagement-unity-ios-get-started/43.png
[53]: ./media/mobile-engagement-unity-ios-get-started/53.png
[54]: ./media/mobile-engagement-unity-ios-get-started/54.png
[70]: ./media/mobile-engagement-unity-ios-get-started/70.png
[71]: ./media/mobile-engagement-unity-ios-get-started/71.png
[72]: ./media/mobile-engagement-unity-ios-get-started/72.png
[73]: ./media/mobile-engagement-unity-ios-get-started/73.png
[74]: ./media/mobile-engagement-unity-ios-get-started/74.png
[75]: ./media/mobile-engagement-unity-ios-get-started/75.png
