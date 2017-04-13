<properties
    pageTitle="Integrace Android SDK pro Azure mobilní zasunutí"
    description="Popisuje, jak integrovat Azure Mobile zapojení SDK v aplikacích pro Android"
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

# <a name="android-sdk-integration-for-azure-mobile-engagement"></a>Integrace Android SDK pro Azure mobilní zasunutí

> [AZURE.SELECTOR]
- [Univerzální Windows](mobile-engagement-windows-store-sdk-overview.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-sdk-overview.md)
- [iOS](mobile-engagement-ios-sdk-overview.md)
- [Android](mobile-engagement-android-sdk-overview.md)

Tento dokument popisuje všechny integrace a konfigurace možností pro Android SDK zapojení Azure Mobile.

## <a name="prerequisites"></a>Zjistit předpoklady pro

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="advanced-features"></a>Pokročilé funkce

### <a name="reporting-features"></a>Funkce pro vytváření sestav

Můžete přidat tyto funkce:

1. [Rozšířené možnosti vytváření sestav](mobile-engagement-android-advanced-reporting.md)
2. [Zasílání zpráv o chybách možností umístění](mobile-engagement-android-location-reporting.md)
3. [Rozšířené možnosti konfigurace](mobile-engagement-android-advanced-configuration.md)

### <a name="notifications"></a>Oznámení:
[Jak integrovat Reach (oznámení) do aplikace pro Android](mobile-engagement-android-integrate-engagement-reach.md)

1. Zasílání zpráv Cloud Google (GCM): [jak integrovat GCM s mobilním zapojení](mobile-engagement-android-gcm-integrate.md)

2. Amazon zařízení zpráv (ADM): [jak integrovat ADM s mobilním zapojení](mobile-engagement-android-adm-integrate.md)

### <a name="tag-plan-implementation"></a>Implementace plán značku:
[Použití rozšířeného Mobile zapojení značení rozhraní API aplikace pro Android](mobile-engagement-android-use-engagement-api.md)

## <a name="release-notes"></a>Poznámky k verzi

### <a name="423-08102016"></a>4.2.3 (08/10/2016)

 - Žádná další lock Wi-Fi.
 - Oprava zablokování při volání getDeviceId před inicializace (Chyba zavedený 4.2.0).

Všechny verze najdete v článku [dokončení poznámky](mobile-engagement-android-release-notes.md).

## <a name="upgrade-procedures"></a>Upgrade postupy

Pokud jste už máte integrovaný starší verzí systému naše SDK aplikace, použijte [Upgrade postupy](mobile-engagement-android-upgrade-procedure.md).
