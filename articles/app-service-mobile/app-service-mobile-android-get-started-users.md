<properties
    pageTitle="Přidání ověřování na Androidu s aplikací Mobile | Azure aplikace služby"
    description="Informace o používání mobilní aplikace v aplikaci služby Azure k ověření uživatele aplikace pro Android celou řadu Zprostředkovatelé identit jiní, včetně Google, Facebooku, Twitteru a Microsoft."
    services="app-service\mobile"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="add-authentication-to-your-android-app"></a>Přidání ověřování aplikace pro Android

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a>Souhrn

V tomto kurzu přidáte ověřování do seznamu úkolů projektu rychlý úvod na Android pomocí zprostředkovatele identit podporované. Tento kurz se podle kurz [Seznámení s aplikací Mobile] , které musíte nejdřív udělat.

##<a name="register"></a>Registrace aplikace pro ověřování a konfigurace aplikace služby

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Omezení oprávnění pro ověřeného uživatele

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

+ V Android Studio otevřete projektu plánovaný, které jste dokončili s kurz [Seznámení s aplikací Mobile]. V nabídce **Spustit** klikněte na **Spustit aplikaci** a ověřte, že je po spuštění aplikace aktivována neošetřené výjimce s stavový kód 401 (Neautorizováno).

     Tato výjimka se stane, protože aplikace pokusí o přístup k back-end jako neověřené uživatele, ale v tabulce _TodoItem_ teď vyžaduje ověření.

Pak aktualizujte aplikaci k ověření uživatele před žádosti o zdrojů z back-end mobilní aplikaci.

## <a name="add-authentication-to-the-app"></a>Přidání ověřování do aplikace

[AZURE.INCLUDE [mobile-android-authenticate-app](../../includes/mobile-android-authenticate-app.md)]

## <a name="cache-tokens"></a>Tokeny ověřování mezipaměti na straně klienta

[AZURE.INCLUDE [mobile-android-authenticate-app-with-token](../../includes/mobile-android-authenticate-app-with-token.md)]

##<a name="next-steps"></a>Další kroky

Teď, když jste dokončili tento kurz základní ověřování, vezměte v úvahu pokračováním k jedné z následujících výukové programy pro:

+ [Přidat nabízených oznámení pro aplikace pro Android](app-service-mobile-android-get-started-push.md) Zjistěte, jak nakonfigurovat back-end vaše mobilní aplikaci pro použití Azure oznámení rozbočovače k odeslání nabízená oznámení.

+ [Povolení offline synchronizace pro aplikace pro Android](app-service-mobile-android-get-started-offline-data.md) Naučte se přidávat aplikace pomocí mobilní aplikace back-end podpora offline režimu. Offline synchronizace koncoví uživatelé chcete provést interakci s mobilní aplikaci&mdash;zobrazení, přidání nebo úprava dat&mdash;i v případě žádné síťové připojení.



<!-- Anchors. -->
[Register your app for authentication and configure Mobile Services]: #register
[Restrict table permissions to authenticated users]: #permissions
[Add authentication to the app]: #add-authentication
[Store authentication tokens on the client]: #cache-tokens
[Refresh expired tokens]: #refresh-tokens
[Next Steps]:#next-steps


<!-- URLs. -->
[Začínáme s aplikací Mobile]: app-service-mobile-android-get-started.md
