<properties
    pageTitle="Začínáme s Azure Mobile zapojení pro Web Apps | Microsoft Azure"
    description="Zjistěte, jak můžete zapojit Mobile Azure pomocí služby technologie pro analýzu a nabízených oznámení pro Web Apps."
    services="mobile-engagement"
    documentationCenter="Mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="js"
    ms.topic="hero-article"
    ms.date="06/01/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-web-apps"></a>Začínáme s Azure Mobile zapojení pro Web Apps

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Toto téma ukazuje, jak používat Azure Mobile zapojení pochopit používání v prohlížeči.

Tento kurz vyžaduje:

+ Visual Studio 2015 nebo jiném editoru podle svého výběru
+ [Web SDK](http://aka.ms/P7b453) 

Tento Web SDK je v náhledu pouze podporuje analýzy v okamžiku, kdy a, které nejsou podporovány odesílání prohlížeči nebo v aplikaci nabízená oznámení ještě. 

> [AZURE.NOTE] Tento kurz, musíte mít účet Azure active. Pokud nemáte účet, můžete vytvořit bezplatný účet zkušební v jenom pár minut. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-web-app-get-started).

##<a name="setup-mobile-engagement-for-your-web-app"></a>Nastavení mobilní zapojení pro webovou aplikaci

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Připojení k back-end zapojení mobilní aplikace

Tento kurz nabízí "základní integrace," což je sadu minimální potřebná k shromáždit data.

Pomocí aplikace Visual Studio prokázat integrace, když použijete můžete tento postup u jakékoli aplikace web vytvořen mimo Visual Studio také vytvoříme základní web appu. 

###<a name="create-a-new-web-app"></a>Vytvořit novou webovou aplikaci

Následující kroky předpokládají použití Visual Studio 2015 kdyby kroky jsou podobné v dřívějších verzích aplikace Visual Studio. 

1. Spusťte aplikaci Visual Studio a na **úvodní** obrazovce vyberte **Nový projekt**.

2. V místní nabídce vyberte **Web** -> **ASP.Net webové aplikace**. Vyplňte aplikaci **název**, **umístění** a **název řešení**a potom klikněte na **OK**.

3. V místní nabídce **Vyberte šablonu** vyberte **prázdné** části **ASP.Net 4.5 šablony** a klikněte na **OK**. 

Teď jste vytvořili nový prázdný projekt v prohlížeči kam jsme začlenit Azure Mobile Engagement Web SDK.

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Připojení aplikace k mobilní zapojení back-end

1. Vytvoření nové složky s názvem **javascript** v řešení a přidejte soubor Web SDK JS **azure engagement.js** do ní. 

2. Přidání nového souboru s názvem **main.js** v této složce javascript s následující kód. Zkontrolujte, že aktualizace připojovací řetězec. To `azureEngagement` objekt se použijí k přístupu SDK webovou postupů. 

        var azureEngagement = {
            debug: true,
            connectionString: 'xxxxx'
        };

    ![Visual Studio se soubory js][1]

##<a name="enable-real-time-monitoring"></a>Povolit sledování v reálném čase

Abyste mohli začít odesílání dat a zajistit, aby uživatelé jsou aktivní, musí pošlete alespoň jeden aktivity back-end Mobile Engagement. Aktivita v kontextu web appu je na webovou stránku. 

1. Vytvořit novou stránku s názvem **home.html** ve vašem řešení a jeho nastavení jako výchozí stránky pro web app. 
2. Obsahovat dva JavaScript, které jsme přidali dříve na této stránce přidáním následující do textu značku. 

        <script type="text/javascript" src="javascript/main.js"></script>
        <script type="text/javascript" src="javascript/azure-engagement.js"></script>

3. Aktualizace značky body volání na EngagementAgent `startActivity` metody
        
        <body onload="engagement.agent.startActivity('Home')">

4. Tady je **home.html** bude vypadat takto
        
        <html>
        <head>
            ...
        </head>
        <body onload="engagement.agent.startActivity('Home')">
            <script type="text/javascript" src="javascript/main.js"></script>
            <script type="text/javascript" src="javascript/azure-engagement.js"></script>
        </body>
        </html>

##<a name="connect-app-with-real-time-monitoring"></a>Připojení aplikace s sledování v reálném čase

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

![][2]

##<a name="extend-analytics"></a>Rozšíření analýzy

Tady jsou všechny metody aktuálně dostupné SDK webu, které můžete použít pro analýzy:

1. Aktivity nebo webové stránky:

        engagement.agent.startActivity(name);
        engagement.agent.endActivity();

2. Události
        
        engagement.agent.sendEvent(name, extras);

3. Chyby

        engagement.agent.sendError(name, extras);

4. Úlohy

        engagement.agent.startJob(name);
        engagement.agent.endJob(name);

<!-- Images. -->
[1]: ./media/mobile-engagement-web-app-get-started/visual-studio-solution-js.png
[2]: ./media/mobile-engagement-web-app-get-started/session.png

