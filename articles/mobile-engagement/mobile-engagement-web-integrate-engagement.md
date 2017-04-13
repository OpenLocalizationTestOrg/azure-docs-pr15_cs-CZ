<properties
    pageTitle="Azure integrace Mobile zapojení Web SDK | Microsoft Azure"
    description="Nejnovější aktualizace a postupy v Azure Mobile zapojení Web SDK"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="web"
    ms.devlang="js"
    ms.topic="article"
    ms.date="02/29/2016"
    ms.author="piyushjo" />

#<a name="integrate-azure-mobile-engagement-in-a-web-application"></a>Integrace Azure Mobile zapojení ve webové aplikaci

> [AZURE.SELECTOR]
- [Univerzální systému Windows](mobile-engagement-windows-store-integrate-engagement.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-integrate-engagement.md)

Nejjednodušší způsob, jak aktivovat technologie pro analýzu a sledování funkcí v Azure Mobile zapojení ve webové aplikaci popisují postupy v tomto článku.

Postupujte podle pokynů k aktivaci, které jsou potřebné pro výpočet všech statistických údajů o uživatelé relace, aktivity, dojde k chybě a technicals sestavy protokolu. Pro statistiku závislé na aplikaci, třeba události, chyby a úlohy musíte aktivovat sestavy protokolu ručně pomocí rozhraní API Azure Mobile Engagement. Další informace Naučte [se používat rozšířené Mobile zapojení přiřazování značek k rozhraní API ve webové aplikaci.](mobile-engagement-web-use-engagement-api.md)

## <a name="introduction"></a>Úvod

[Stažení webu Azure mobilní zapojení SDK](http://aka.ms/P7b453).
Web SDK zapojení Mobile vyřízeny jako jeden soubor JavaScript, azure engagement.js, které je potřeba zahrnout na každé stránce webu nebo webové aplikace.

> [AZURE.IMPORTANT] Dříve než spustíte tento skript, je třeba spustit fragment kód nebo skript, který napíšete konfigurace zapojení mobilní aplikace.

## <a name="browser-compatibility"></a>Kompatibilita s prohlížečem

SDK Web zapojení mobilní používá nativní JSON kódování a dekódování kromě žádosti AJAX doménami (může na specifikaci W3C CORS). Je kompatibilní se službou těchto prohlížečů:

* Microsoft Edge 12 +
* Internet Explorer 10 +
* Firefox 3.5 +
* Chrome 4 +
* Safari 6 +
* Opera 12 +

## <a name="configure-mobile-engagement"></a>Konfigurace mobilního zapojení

Vytvořit skript, který vytvoří globální `azureEngagement` JavaScript object jako v následujícím příkladu. Vzhledem k tomu webu může násobky stránky, v tomto příkladě se předpokládá, tento skript je součástí každé stránky. V tomto příkladu je JavaScript object s názvem `azure-engagement-conf.js`.

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
    };

`connectionString` Hodnota aplikace se zobrazí na portálu Azure.

> [AZURE.NOTE] `appVersionName`a `appVersionCode` jsou volitelné. Doporučujeme ale nakonfigurovat je, aby mohl analýzy zpracovávat informace o verzi.

## <a name="include-mobile-engagement-scripts-in-your-pages"></a>Zahrnout skripty Mobile zapojení do stránky
Přidání mobilní zapojení skripty ke stránkám v jednom z těchto způsobů:

    <head>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </head>

Nebo takto:

    <body>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </body>

## <a name="alias"></a>Alias

Po načtení skript Mobile zapojení Web SDK vytvoří **zapojení** alias pro přístup k rozhraní API SDK. Stanovit konfigurační SDK nelze použít tento alias. Alias slouží jako odkaz na tento si přečtěte následující dokumentaci.

Všimněte si, že pokud výchozí alias v konfliktu s prací jiného globální proměnná ze stránky, můžete upravit ho v konfiguraci takto před načtením SDK Web zapojení Mobile:

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
      alias:'anotherAlias'
    };

## <a name="basic-reporting"></a>Základní vytváření sestav

Základní vytváření sestav v mobilní zapojení zahrnuje Statistika úrovně relace, například statistických údajů o uživatelích, relace, aktivity a dojde k chybě.

### <a name="session-tracking"></a>Sledování relací

Relaci můžete zapojit Mobile je rozdělen posloupnost aktivity, označených název.

Klasický webu doporučujeme deklarovat jinou aktivitu na každé stránce webu. Pro web nebo web aplikace které aktuální stránky nikdy změní můžete sledovat aktivity na menší měřítko jako v rámci stránky.

V obou případech start nebo změnit aktuální činnosti uživatelů, zavolejte `engagement.agent.startActivity` (funkce). Příklad:

    <body onload="yourOnload()">

    <!-- -->

    yourOnload = function() {
      [...]
      engagement.agent.startActivity('welcome');
    };

Zapojení Mobile serveru automaticky ukončí otevřít relaci do tří minut po zavření aplikace na stránce.

Můžete taky můžete ukončit relaci ručně tak, že zavoláte `engagement.agent.endActivity`. Tím se nastaví aktuální činnosti uživatelů na "Nečinnosti."  Relace skončit později 10 sekund, pokud novou volání `engagement.agent.startActivity` obnoví relace.

V dialogovém okně objekt globální zapojení můžete nakonfigurovat zpoždění 10 sekund následujícím způsobem:

    engagement.sessionTimeout = 2000; // 2 seconds
    // or
    engagement.sessionTimeout = 0; // end the session as soon as endActivity is called

> [AZURE.NOTE] Nelze použít `engagement.agent.endActivity` v `onunload` zpětné protože nemůžete v této fázi AJAX volání.

## <a name="advanced-reporting"></a>Rozšířené možnosti vytváření sestav

Pokud chcete Pokud budete chtít zprávu specifické pro aplikaci události, chyby a úlohy, musíte použít rozhraní API zapojení Mobile. Přístup k rozhraní API zapojení Mobile prostřednictvím `engagement.agent` objektu.

Přístup ke všem rozšířených možností v mobilní zapojení v rozhraní API zapojení Mobile. Rozhraní API podrobné v článku znalostní báze [jak používat rozšířené Mobile zapojení přiřazování značek k rozhraní API ve webové aplikaci](mobile-engagement-web-use-engagement-api.md).

## <a name="customize-the-urls-used-for-ajax-calls"></a>Přizpůsobení adresy URL používané pro hovory AJAX

Můžete přizpůsobit adresy URL, které SDK Web zapojení mobilní používá. Znovu URL protokolu (SDK koncový bod pro protokolování) je možné přepsat konfigurace takto:

    window.azureEngagement = {
      ...
      urls: {
        ...        
        getLoggerUrl: function() {
        return 'someProxy/log';
        }
      }
    };

Pokud vaše adresa URL funkce vrátí řetězec začínající `/`, `//`, `http://`, nebo `https://`, není použit výchozí schéma. Ve výchozím nastavení `https://` schéma se používá pro tyto adresy URL. Pokud chcete upravit výchozí schéma, přepište konfiguraci, třeba takto:

    window.azureEngagement = {
      ...
      urls: {
        ...      
        scheme: '//'
      }
    };
