<properties
    pageTitle="Upřesnit konfiguraci pro zapojení univerzální aplikace Windows SDK"
    description="Upřesnění možností konfigurace pro zapojení Azure Mobile s univerzální aplikace Windows"                    
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-configuration-for-windows-universal-apps-engagement-sdk"></a>Upřesnit konfiguraci pro zapojení univerzální aplikace Windows SDK

> [AZURE.SELECTOR]
- [Univerzální Windows](mobile-engagement-windows-store-advanced-configuration.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-advanced-configuration.md)

Tento postup popisuje, jak nakonfigurovat možnosti konfigurace Azure Mobile zapojení Android aplikací.

## <a name="prerequisites"></a>Zjistit předpoklady pro

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

##<a name="advanced-configuration"></a>Upřesnit

### <a name="disable-automatic-crash-reporting"></a>Zákaz automatické pád vykazování

Můžete zakázat automatické pád vytváření sestav funkce Engagement. Potom když dojde k neošetřené výjimce, zapojení se nic nestane.

> [AZURE.WARNING] Pokud jste tuto funkci zakázat, pak dojde k neošetřené ukončení ve své aplikaci zapojení není odeslat že pád **a** nezavře úlohy a relace.

Vypnout automatické pád vytváření sestav, přizpůsobení konfiguraci podle toho, tak, jak ho deklarované:

#### <a name="from-engagementconfigurationxml-file"></a>Z `EngagementConfiguration.xml` souboru

Nastavit sestavy pád `false` mezi `<reportCrash>` a `</reportCrash>` značky.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Z `EngagementConfiguration` objekt za běhu

Nastavení sestavy pád NEPRAVDA pomocí EngagementConfiguration objektu.

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="disable-real-time-reporting"></a>Vypnutí oznámení v reálném čase

Ve výchozím nastavení protokoly sestavy využití služeb v reálném čase. Pokud aplikace sestav protokolů často, je lepší vyrovnávací paměť protokoly a vykazování v celém dokumentu v pravidelných časových intervalech. Je místo toho možnost "Shlukový režim".

K tomu, zavolejte metodu:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Argument je hodnota v **milisekundách**. Kdykoli budete chtít znova aktivovat v reálném čase protokolování, zavolejte metodu bez zadání parametru, nebo hodnotu 0.

Režim požadavků mírně zvyšuje životnost battery ale má vliv na monitoru můžete zapojit: všechny relace a úlohy doba trvání zaokrouhleny požadavků prahové hodnoty (tedy relace a úlohy kratší než mezní hodnota požadavků nemusí být viditelné). Doporučujeme používat prahové požadavků než 30000 (30s). Uložené protokoly se omezuje 300 položek. Pokud odesílání je příliš dlouhá, může dojít ke ztrátě některých protokoly.

> [AZURE.WARNING] Mezní hodnota požadavků se nedají konfigurovat období menší než jedna sekunda. Pokud uděláte, SDK ukazuje trasování s chybou a automaticky obnoví výchozí hodnotu nula sekund. To spustí SDK vykazování protokolech v reálném čase.

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview
