<properties
    pageTitle="Integrace s Windows univerzální SDK"
    description="Univerzální integrace s Windows pro SDK pro Azure mobilní zasunutí"                                     
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

#<a name="windows-universal-sdk-integration-for-azure-mobile-engagement"></a>Integrace univerzální SDK Windows Azure mobilní zapojení

Tento dokument popisuje všechny integraci a konfigurace možnosti k dispozici v Azure Mobile zapojení Windows univerzální SDK.

## <a name="prerequisites"></a>Zjistit předpoklady pro

Před zahájením tohoto kurzu, musíte nejdřív dokončení naše [kurz 15 minut](mobile-engagement-windows-store-dotnet-get-started.md).

## <a name="advanced-features"></a>Pokročilé funkce

### <a name="reporting-features"></a>Funkce pro vytváření sestav
Můžete přidat tyto funkce:

1. [Rozšířené možnosti vytváření sestav](mobile-engagement-windows-store-advanced-reporting.md)

2. [Upřesnit možnosti](mobile-engagement-windows-store-advanced-configuration.md)

### <a name="notifications"></a>Oznámení

[Jak integrovat Reach (oznámení) v systému Windows univerzální aplikace](mobile-engagement-windows-store-integrate-engagement-reach.md)

### <a name="tag-plan-implementation"></a>Implementace plán značku:

[Použití rozšířeného Mobile zapojení značení rozhraní API systému Windows univerzální aplikace](mobile-engagement-windows-store-use-engagement-api.md)

##<a name="release-notes"></a>Poznámky k verzi

###<a name="340-04192016"></a>3.4.0 (04/19/2016)

-   Kontaktujte překryvném vylepšení.
-   Které SDK protokoly konzoly dodat "TestLogLevel" rozhraní API pro povolit nebo zakázat/filtr.
-   Spusťte pevné oznámení v aktivity zacílení nezobrazí přihlášená k první činnosti.

Starší verze najdete v článku [dokončení poznámky](mobile-engagement-windows-store-release-notes.md)

##<a name="upgrade-procedures"></a>Upgrade postupy

Pokud jste už integrovali starší verzí systému zapojení do aplikace, je nutné zvážit následující skutečnosti při upgradu SDK.

Pokud zmeškané více verzích SDK pravděpodobně několik postup. V tématu dokončení [Upgradu postupy](mobile-engagement-windows-store-upgrade-procedure.md). Pokud například můžete migrovat z 0.10.1 do 0.11.0 budete muset nejdřív použijte postup "z 0.9.0 0.10.1" pak "z 0.10.1 0.11.0" postup.

###<a name="from-330-to-340"></a>Z 3.3.0 3.4.0

####<a name="test-logs"></a>Testování protokoly

Protokoly konzoly vytvořené pomocí SDK můžete by se povolené/vypnutí/filtrování. Chcete-li přizpůsobit, aktualizujte vlastnost `EngagementAgent.Instance.TestLogEnabled` na jednu z dostupných z hodnot `EngagementTestLogLevel` výčet, například:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

####<a name="resources"></a>Zdroje informací

Překryvné zobrazení Reach jsme vylepšili. Je součástí SDK NuGet balíčku zdrojů.

Při upgradu na novou verzi sady SDK, můžete zvolit, jestli chcete zachovat stávající soubory ve složce překryvném prostředků nebo ne:

* Předchozí překryvném pracuje za vás, zda jsou integrace `WebView` prvky ručně, pak můžete se rozhodnout, aby vaše ukončení soubory, bude dál pracovat.
* Provést aktualizaci na nové překryvném nahradit celý `overlay` složku, ze svých prostředcích novým z balíčku SDK (UWP aplikace: po upgradu, dostanete se nová složka překryvném z uživatelského profilu %\\.nuget\packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).

> [AZURE.WARNING] Pomocí nové překryvném přepíše veškeré vlastní nastavení na předchozí verze.

### <a name="upgrade-from-older-versions"></a>Upgrade ze starších verzí

V tématu [Upgrade postupy](mobile-engagement-windows-store-upgrade-procedure.md)
