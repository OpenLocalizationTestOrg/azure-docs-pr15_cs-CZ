<properties
    pageTitle="Přidání ověřování IOS s aplikací Mobile Azure"
    description="Naučte se používat pro ověřování uživatelů aplikace pro iOS celou řadu Zprostředkovatelé identit jiní, včetně AAD Google, Facebooku, Twitteru a Microsoft Azure mobilní aplikace."
    services="app-service\mobile"
    documentationCenter="ios"
    authors="ysxu"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="add-authentication-to-your-ios-app"></a>Přidání ověřování aplikace pro iOS

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

V tomto kurzu přidáte ověřování [iOS rychlé zahájení] projektu pomocí zprostředkovatele identit podporované. Tento kurz se podle kurzu [iOS Snadné spuštění] , které musíte nejdřív udělat.

##<a name="register"></a>Registrace aplikace pro ověřování a konfigurace aplikace služby

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Omezení oprávnění pro ověřeného uživatele

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

V Xcode stiskněte klávesu **Spustit** spusťte aplikaci. Výjimky bude zvýšit, protože aplikace pokusí o přístup k back-end jako neověřené uživatele, ale _TodoItem_ tabulka teď vyžaduje ověření.

##<a name="add-authentication"></a>Přidání ověřování aplikace

[AZURE.INCLUDE [app-service-mobile-ios-authenticate-app](../../includes/app-service-mobile-ios-authenticate-app.md)]


<!-- URLs. -->

[Představení aplikace iOS]: app-service-mobile-ios-get-started.md

[Azure portal]: https://portal.azure.com
