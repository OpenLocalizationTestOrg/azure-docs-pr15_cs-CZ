<properties
    pageTitle="Přidání nabízená oznámení pro iOS aplikace s aplikací Mobile Azure"
    description="Naučte se používat Azure mobilní aplikace odeslat nabízených oznámení pro aplikace pro iOS."
    services="app-service\mobile"
    documentationCenter="ios"
    manager="yochayk"
    editor=""
    authors="ysxu"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="yuaxu"/>


# <a name="add-push-notifications-to-your-ios-app"></a>Přidání služby nabízená oznámení pro iOS aplikace

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Základní informace
V tomto kurzu přidáte nabízená oznámení [iOS Snadné spuštění] projektu tak, aby nabízená oznámení odeslaný do zařízení pokaždé, když je vložen záznamu.

Pokud nepoužíváte project serveru stažený úvodní, budete potřebovat balíček nabízená oznámení rozšíření. Další informace najdete v článku [práce s back-end serveru .NET SDK Azure mobilních aplikací](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

Domovské stránce [iOS simulator nepodporuje nabízená oznámení](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html). Potřebujete zařízení fyzicky iOS a [členství v Apple vývojář programu](https://developer.apple.com/programs/ios/).

##<a name="configure-hub"></a>Konfigurace oznámení centrální

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a id="register"></a>Registrace aplikace nabízených oznámení

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Konfigurace Azure odeslat nabízená oznámení

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

##<a id="update-server"></a>Aktualizace back-end odeslat nabízená oznámení

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-configure-push-apns](../../includes/app-service-mobile-dotnet-backend-configure-push-apns.md)]

## <a id="add-push"></a>Přidání nabízených oznámení pro aplikace

[AZURE.INCLUDE [app-service-mobile-add-push-notifications-to-ios-app.md](../../includes/app-service-mobile-add-push-notifications-to-ios-app.md)]

## <a id="test"></a>Test nabízená oznámení

[AZURE.INCLUDE [Test Push Notifications in App](../../includes/test-push-notifications-in-app.md)]

##<a id="more"></a>Další

* Šablony pružnost posune různé platformy a lokalizované posune pošlete. [Jak používat iOS klienta v knihovně Azure mobilní aplikace](app-service-mobile-ios-how-to-use-client-library.md#templates) se dozvíte, jak si zaregistrovat šablony.

<!-- Anchors.  -->

<!-- Images. -->

<!-- URLs. -->
[Představení aplikace iOS]: app-service-mobile-ios-get-started.md
