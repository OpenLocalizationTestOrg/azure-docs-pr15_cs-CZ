<properties 
    pageTitle="Odesílání přizpůsobených oznámení s zapojení Mobile Azure" 
    description="Jak odeslat přizpůsobených oznámení s využitím informace profilů uživatelů v oznámení jako názvů"        
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="all" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="personalize-notifications-by-including-user-name"></a>Vložením oznámení uživatelského jména

Ve vaší společnosti quest aby oznámení poutavost uživatelům aplikace měli byste zvážit přizpůsobení je. Jedním ze způsobů výkonné zahrnuje selektivně používání aplikace uživatelská jména při řešení oznámení pro jejich více osobní. Word upozornění tady – měli přístup přidávání uživatelská jména k oznámení pečlivě vzhledem k tomu, že pokud nadměrné použití této strategie potom ho může dojít následujícím strach postiženým uživatelům aplikace. Měli byste taky zajistit, že uživatele vyjádření výslovného souhlasu poskytnout tyto osobních údajů pro vás úplné souhlas v mobilní aplikaci předtím, než začnete pomocí tohoto příkazu jsou nechat. 

Doslova s Azure Mobile zapojení se dají dělat přizpůsobení oznámení pomocí následujícího postupu ve kterých použijeme scénář včetně uživatelské jméno v oznámení. Použijete koncepci informace App nebo značky jehož hodnoty může být buď předaných SDK integrované v mobilní aplikaci nebo prostřednictvím rozhraní API. Tyto informace o aplikaci nebo značky se pak může:

1. pro cílovou oznámení na určité uživatele na základě hodnot informace App nebo 
2. jako zástupné symboly v oznámení, které bude nahradily hodnotami specifické pro zařízení/uživatele při odesílání oznámení o toto zařízení. 

> [AZURE.IMPORTANT] Všimněte si, že rychlosti odesílání oznámení uvidí snížení z důvodu tento Pobočková nahrazení hodnot informace app každý oznámení. 

##<a name="register-app-info-in-the-mobile-engagement-portal"></a>Registrace aplikace informace v portálu mobilní zapojení

1) Začínáte s registrace aplikace informace nebo značky na portálu Azure. Přejděte na **Nastavení** -> **Tag (informace App)** pro tuto.  

![][1]  

2) Klikněte na **Nový tag (informace app)** a zadejte název jako *uživatelské_jméno* a typ jako *řetězec* a klikněte na **Odeslat**. 

![][2]

3) Nakonec zobrazí tyto aplikace – informace o registrované takto:

![][3]

##<a name="send-app-info-from-the-client-sdk"></a>Odesílat informace o aplikaci z klienta SDK

Tady používáme Windows univerzální aplikace příkladu, ale odpovídající metody existují pro naše SDK taky. 

Za předpokladu, že máte metodu v mobilní aplikaci, kde můžete získat informace o profilu od uživatele jako jejich jména pravděpodobně po ověření je, bude zavoláte `SendAppInfo` metoda tady a naplnění přínosu `user_name` informace aplikace, kterou jste registrovali dříve do back-end služby zapojení Mobile. 

    Dictionary<object, object> appInfo = new Dictionary<object, object>();
    appInfo.Add("user_name", str);
    EngagementAgent.Instance.SendAppInfo(appInfo); 

##<a name="send-personalized-notifications"></a>Odesílání přizpůsobených oznámení

Teď jsou všechna nastavení o neodeslání oznámení pomocí tohoto **uživatelské_jméno**. 

1) Přejděte na portál zapojení Mobile na kartě **dosáhla** vytvořit oznámení a použijete tento zástupný symbol v následujícím formátu kdekoli v názvu oznámení nebo textu. 

![][4]  

> [AZURE.NOTE] Všichni uživatelé, u kterých není nastavená informace app uživatelské_jméno, nebude získat oznámení. Pokud dostanete oznámení kampaně v režimu test a pokud nemáte aplikaci – informace o nastavení a pak pošleme "?" znak nahrazení zástupného symbolu. 

2) Kdy Mobile zapojení vybereme zařízení k odeslání oznámení a pak se podívejte se na informace o této aplikaci a nahraďte hodnotu v zástupném symbolu.  
Například, pokud jsme nastavili `str = "Scott"` pro uživatele než zařízení pošle registrace přidružené k aplikaci informace o **uživatelské_jméno = JIŘÍ** pro tohoto uživatele a tomuto uživateli se zobrazí v nepřítomnosti aplikace nabízená oznámení v v tomto formátu. 

![][5]  

<!-- Images. -->
[1]: ./media/mobile-engagement-send-personalized-notifications/app-info.png
[2]: ./media/mobile-engagement-send-personalized-notifications/create-app-info.png
[3]: ./media/mobile-engagement-send-personalized-notifications/app-info-user-name.png
[4]: ./media/mobile-engagement-send-personalized-notifications/personal-notification.png
[5]: ./media/mobile-engagement-send-personalized-notifications/notification.png

