<properties
    pageTitle="Přidání nabízená oznámení pro aplikace Androidu Azure mobilních aplikacích"
    description="Naučte se používat Azure mobilní aplikace odeslat nabízených oznámení pro aplikace pro Android."
    services="app-service\mobile"
    documentationCenter="android"
    manager="erikre"
    editor=""
    authors="ysxu"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-android-app"></a>Přidání nabízených oznámení pro aplikace pro Android

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Základní informace
V tomto kurzu přidáte nabízená oznámení [Android úvodní] projektu tak, aby nabízená oznámení odeslaný do zařízení pokaždé, když je vložen záznamu.

Pokud nepoužíváte project serveru stažený rychlý start, musíte balíček rozšíření nabízená oznámení. Další informace najdete v tématu [Práce se serverem back-end .NET SDK Azure mobilních aplikací](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Budete potřebovat:

* IDE v závislosti na back-end projektu:

    * [Android Studio](https://developer.android.com/sdk/index.html) Pokud tato aplikace má back-end Node.js.

    * [Visual Studio komunity 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) nebo novější, pokud tato aplikace má back-end .net.

* Zařízení s androidem 2.3 nebo novější, revize Google úložiště 27 a novějších verzích a Google Play Services 9.0.2 nebo vyšší pro zasílání zpráv cloudu Firebase.

* Dokončení [Android rychlý start].

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a>Vytvoření projektu, který podporuje zasílání zpráv cloudu Firebase

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-notification-hub"></a>Konfigurace rozbočovači oznámení

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Konfigurace Azure odeslat nabízená oznámení

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a name="enable-push-notifications-for-the-server-project"></a>Povolení nabízených oznámení pro server project

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-configure-push-google](../../includes/app-service-mobile-dotnet-backend-configure-push-google.md)]

## <a name="add-push-notifications-to-your-app"></a>Přidání nabízených oznámení pro aplikace

V této části můžete aktualizovat aplikace Android klienta pro zpracování nabízená oznámení.

### <a name="verify-android-sdk-version"></a>Ověření verze Android SDK

[AZURE.INCLUDE [app-service-mobile-verify-android-sdk-version](../../includes/app-service-mobile-verify-android-sdk-version.md)]

Dalším krokem je nainstalujte Google Play services. Zasílání zpráv Cloud Google má některé minimální rozhraní API úrovně požadavky pro vývoj a testování, které musí odpovídat vlastnost **minSdkVersion** v manifestu.

Pokud testujete s starších zařízení, použijte [Nastavení nahoru Google Play Services SDK] lze určit, jak vysoko můžete nastavte u této hodnoty a správně nastavená.

### <a name="add-google-play-services-to-the-project"></a>Přidejte Google Play Services do projektu

[AZURE.INCLUDE [Add Play Services](../../includes/app-service-mobile-add-google-play-services.md)]

### <a name="add-code"></a>Přidání kódu

[AZURE.INCLUDE [app-service-mobile-android-getting-started-with-push](../../includes/app-service-mobile-android-getting-started-with-push.md)]

## <a name="test-the-app-against-the-published-mobile-service"></a>Testování aplikaci s publikované mobilní služby

Aplikaci můžete otestovat přímo připojení telefonu s Androidem pomocí kabelu USB, nebo pomocí virtuální zařízení v emulátor.

## <a name="more"></a>Další

<!-- URLs -->
[Android rychlým startem]: app-service-mobile-android-get-started.md

[Nastavení služby Google Play SDK]:https://developers.google.com/android/guides/setup
