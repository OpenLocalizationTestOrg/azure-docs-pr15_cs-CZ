<properties 
    pageTitle="Windows Phone přehled Silverlight SDK" 
    description="Základní informace o SDK Windows Phone Silverlight pro Azure mobilní zasunutí"                     
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede"
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-phone-silverlight-sdk-overview-for-azure-mobile-engagement"></a>Windows Phone přehled Silverlight SDK pro Azure mobilní zasunutí

Chcete-li získat podrobnosti o tom, jak integrovat Azure Mobile zapojení v aplikaci Windows Phone Silverlight, začněte tady. Pokud byste chtěli vyzkoušejte si to poprvé, ujistěte se, musíte udělat naše [kurz 15 minut](mobile-engagement-windows-phone-get-started.md).

Kliknutím na tlačítko Zobrazit [Obsahu SDK](mobile-engagement-windows-phone-sdk-content.md)

##<a name="integration-procedures"></a>Integrace postupy

1. Začněte zde: [jak integrovat Mobile zapojení do aplikace pro Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)

2. Oznámení: [Jak integrovat Reach (oznámení) v aplikaci Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement-reach.md)

3. Označení plán implementaci: [používání Upřesnit Mobile zapojení označování rozhraní API v aplikaci Windows Phone Silverlight](mobile-engagement-windows-phone-use-engagement-api.md)

##<a name="release-notes"></a>Poznámky k verzi

###<a name="330-04192016"></a>3.3.0 (04/19/2016)
Část *MicrosoftAzure.MobileEngagement* nuget balíček **v3.4.0**

-   Které SDK protokoly konzoly dodat "TestLogLevel" rozhraní API pro povolit nebo zakázat/filtr.

Starší verze najdete v článku [dokončení poznámky](mobile-engagement-windows-phone-release-notes.md)

##<a name="upgrade-procedures"></a>Upgrade postupy

Pokud jste už máte integrovaný starší verzí systému naše SDK aplikace, je nutné zvážit následující skutečnosti při upgradu SDK.

Možná bude potřeba postup několik Pokud zmeškané více verzích SDK. V tématu dokončení [Upgradu postupy](mobile-engagement-windows-phone-upgrade-procedure.md). Pokud například můžete migrovat z 0.10.1 do 0.11.0 budete muset nejdřív použijte postup "z 0.9.0 0.10.1" pak "z 0.10.1 0.11.0" postup.

###<a name="from-200-to-330"></a>Z 2.0.0 3.3.0

####<a name="test-logs"></a>Testování protokoly

Protokoly konzoly vytvořené pomocí SDK můžete by se povolené/vypnutí/filtrování. Chcete-li přizpůsobit, aktualizujte vlastnost `EngagementAgent.Instance.TestLogEnabled` na jednu hodnotu poskytuje společnost `EngagementTestLogLevel` výčet, například:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="upgrade-from-older-versions"></a>Upgrade ze starších verzí

V tématu [Upgrade postupy](mobile-engagement-windows-phone-upgrade-procedure.md)
 
