<properties
    pageTitle="Typy koncový bod verze 2.0 | Microsoft Azure"
    description="Typy aplikací a scénáře nepodporuje koncový bod Azure AD verze 2.0."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="dastrock"/>

# <a name="types-of-apps-for-the-v20-endpoint"></a>Typy aplikací koncového bodu verze 2.0
Koncový bod verze 2.0 podporuje ověřování pro řadu architektury moderní aplikace, které jsou založeny na standardních protokolů oborové [OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow) a/nebo [OpenID připojení](active-directory-v2-protocols.md#openid-connect-sign-in-flow).  Tento dokument stručně popisuje typy aplikace, které můžete vytvářet, nezávisle na jazyk nebo platformu dáváte přednost.  Průvodce vám pomůže pochopit nejvyšší úrovně scénáře před [pustit rovnou do kódu](active-directory-appmodel-v2-overview.md#getting-started).

> [AZURE.NOTE]
    Některé scénáře Azure Active Directory a funkce jsou podporovány koncový bod verze 2.0.  Pokud chcete zjistit, zda by měly používat koncový bod verze 2.0, přečtěte si informace o [omezení verze 2.0](active-directory-v2-limitations.md).

## <a name="the-basics"></a>Základní informace
Všechny aplikace, která používá koncový bod verze 2.0 muset registrovaná u [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Proces registrace aplikace shromažďovat a přiřadit několika hodnot do aplikace:

- Číslo **Id aplikace** , které jednoznačně identifikuje aplikace
- **Přesměrovat URI** něhož chcete směrovat odpovědi zpátky do aplikace
- Několik scénář specifické hodnotám.  Další podrobnosti, zjistěte, jak si [zaregistrovat aplikace](active-directory-v2-app-registration.md).

Po registraci, aplikace informuje uživatele o s Azure AD tak, že požadavky na koncový bod verze 2.0 Azure Active Directory.  Připravili jsme pro rámce otevřít zdroj a knihoven, které starat o podrobnosti tyto požadavky, nebo můžete implementovat logiku ověřování sami tím, že vytvoří požadavky na tyto koncové body:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```
<!-- TODO: Need a page for libraries to link to -->

## <a name="web-apps"></a>Webové aplikace
Pro webové aplikace (.NET, PHP, Java, skutečné, Python, uzel a další), které jsou dostupné prostřednictvím prohlížeče, můžete dělat uživatel přihlášení pomocí [OpenID spojení](active-directory-v2-protocols.md#openid-connect-sign-in-flow).  V OpenID připojení dostane web appu `id_token`, tokenu zabezpečení, který ověří identita uživatele a poskytuje informace o uživateli ve formuláři deklarací:

```
// Partial raw id_token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded id_token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

Se dozvíte o všechny typy tokeny a deklarací do aplikace pro [referenci tokenu verze 2.0](active-directory-v2-tokens.md).

Ve webových aplikacích pro server jen tok ověřování přihlašovací nejvyšší úrovně takto:

![Obrázek plaveckých drah Web App](../media/active-directory-v2-flows/convergence_scenarios_webapp.png)

Ověření id_token pomocí veřejných podpisový dostali z verze 2.0 koncového bodu stačí k zajištění identita uživatele a nastavit cookie relace, který slouží k identifikaci uživatele na požadavky na pozdější stránku.

Tento scénář v akci zobrazíte Vyzkoušejte jednu web app přihlašovací ukázky v části naše [Začínáme](active-directory-appmodel-v2-overview.md#getting-started) .

Kromě jednoduchých přihlášení do webových aplikací serveru může taky potřebují dostat k některé další webové služby, třeba rozhraní REST API.  V tomto případě web appu serveru můžete zapojit v kombinovaných OpenID připojení a OAuth 2.0 toku, pomocí [kódu se tak mohli ověřovat 2.0 OAuth toku](active-directory-v2-protocols.md#oauth2-authorization-code-flow). Tento scénář se vztahuje pod v naší [téma Začínáme WebAPI web Appu](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md).

## <a name="web-apis"></a>Rozhraní API webových
Koncový bod verze 2.0 umožňuje zabezpečené webovým službám stejně, jako je vaše aplikace RESTful rozhraní API webových.  Místo id_tokens a relace soubory cookie pomocí rozhraní API webových OAuth 2.0 access_tokens zabezpečené svých dat a ověřování příchozí žádosti.  Volající rozhraní API webových připojí access_token v záhlaví se tak mohli ověřovat žádost HTTP:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

Rozhraní API webových můžete používat access_token ověřit identitu volajícího rozhraní API a z deklarace, které jsou zakódovaný v access_token vyčíst informace o volajícím.  Se dozvíte o všechny typy tokeny a deklarací do aplikace pro [referenci tokenu verze 2.0](active-directory-v2-tokens.md).

Rozhraní API webových můžete uživatelům poskytnout možnost zvolit v/odhlásit určité funkce nebo dat vystavením oprávnění, jinak jmenoval [obory](active-directory-v2-scopes.md).  Uživatel musí aplikace volající získat oprávnění k obor souhlas s obor během toku.  Koncový bod verze 2.0 budou starat o výzvou o souhlas a záznamu oprávnění ve všech access_tokens, které rozhraní API webových přijímá.  Všechny rozhraní API webových potřebuje starat o to je ověření access_tokens, které obdrží při každém volání a provádění kredibility příslušná oprávnění.

Rozhraní API webových dostávat access_tokens ze všech typů aplikací, včetně web apps serveru, plochu a mobilní aplikace, jedné stránky aplikací, procesy daemon straně serveru a i další rozhraní API webových.  Nejvyšší úrovně tok ověřování rozhraní api webových vypadá takto:

![Obrázek webového rozhraní API plavecké dráhy](../media/active-directory-v2-flows/convergence_scenarios_webapi.png)

Další informace o authorization_codes refresh_tokens a podrobný postup získání access_tokens, přečtěte si o [protokolu OAuth 2.0](active-directory-v2-protocols-oauth-code.md).

Informace o zabezpečeného webového rozhraní api s OAuth2 access_tokens, podívejte se na webového rozhraní api ukázky v naší [Začínáme oddíl](active-directory-appmodel-v2-overview.md#getting-started).


## <a name="mobile-and-native-apps"></a>Mobile, přičemž nativní aplikace
Aplikace, které jsou instalovány na zařízení, třeba přenosných a stolních aplikace, často potřebují dostat k služby back-end nebo webového rozhraní API ukládají data a používat různé funkce jménem uživatele.  Tyto aplikace můžete přidat přihlašovací a povolení ke službě back-end pomocí [kódu se tak mohli ověřovat 2.0 OAuth toku](active-directory-v2-protocols-oauth-code.md).  

V tomto toku aplikace dostává authorization_code z verze 2.0 koncový bod při přihlašování uživatelů, které představuje oprávnění v aplikaci volat back-end služby jménem aktuálně přihlášený uživatel.  Aplikaci můžete vyměňovat pak authoriztion_code na pozadí pro access_token OAuth 2.0 a refresh_token.  Aplikaci můžete access_token ověřit webového rozhraní API v žádosti o HTTP a využití refresh_token nové access_tokens po vypršení platnosti starší z nich.

![Obrázek nativní aplikaci plavecké dráhy](../media/active-directory-v2-flows/convergence_scenarios_native.png)

## <a name="single-page-apps-javascript"></a>Jedna stránka aplikace (javascript)
Mnoho moderní aplikace mít jedné stránky aplikace (SPA) front-end primárně písemné v JavaScriptu a často používáte rámců například AngularJS Ember.js, Durandal, atd.  Koncový bod verze 2.0 Azure AD podporuje tyto aplikace použití [OAuth 2.0 implicitní tok](active-directory-v2-protocols-implicit.md).

V tomto toku aplikaci dostává tokenů z verze 2.0 povolte koncový bod přímo, bez provádění všech serverů výměny back-end.  Díky tomu, že všechny logiku ověřování a zpracování relace se dali úplně v klientovi javascript bez provedení přesměrování na stránku navíc.

![Obrázek implicitní toku plavecké dráhy](../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

Zobrazíte situaci, v akci, vyzkoušejte jeden z jedné stránky aplikace ukázky v naší části [Začínáme](active-directory-appmodel-v2-overview.md#getting-started) .

### <a name="daemonsserver-side-apps"></a>Procesy daemon server straně aplikace
Aplikace, které obsahují dlouho spuštěné procesy nebo vzorců bez informací o stavu uživatele taky potřebují pro přístup k zabezpečené zdroje, jako je třeba rozhraní API webových.  Tyto aplikace můžete ověřit a najděte tokenů identity v aplikaci (spíše než delegovaného identita uživatele) pomocí klienta OAuth 2.0 toku přihlašovací údaje.

V tomto toku aplikace obdrží tokeny Přímá práce s `/token` koncového bodu:

![Obrázek démon aplikace plavecké dráhy](../media/active-directory-v2-flows/convergence_scenarios_daemon.png)

Vytvářet aplikace démon, najdete v článku documeenation pověření klienta v části naše [Začínáme](active-directory-appmodel-v2-overview.md#getting-started) nebo v nápovědě k [této aplikace ukázkové .NET](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).

## <a name="current-limitations"></a>Aktuální omezení
Tyto typy aplikací aktuálně nepodporuje se koncový bod verze 2.0, ale jsou na plán.  Další omezení a omezení koncového bodu verze 2.0 jsou popsané v [článku omezení verze 2.0](active-directory-v2-limitations.md).

### <a name="chained-web-apis-on-behalf-of"></a>Zřetězené webového rozhraní API (na jménem)
Mnoho architektury zahrnout rozhraní API webových, kterou je potřeba zavolat jiné podřízený Web rozhraní API, obě zajištěná koncový bod verze 2.0.  Tento postup je běžný v původní klienti, které mají rozhraní API webových backendovou, která volá Microsoft Online služby, jako je Office 365 nebo rozhraní API grafu.

Tento scénář zřetězené rozhraní API webových můžete podporována formátu udělení OAuth 2.0 Jwt nosný pověření, jinak jmenoval [On-Behalf-Of tok](active-directory-v2-protocols.md#oauth2-on-behalf-of-flow).  Však toku dál-jménem z není součástí aktuálně koncový bod verze 2.0.  Jak funguje tento toku v obecně dostupných Azure AD služeb, podívejte se [na-jménem-z příkladu na GitHub](https://github.com/AzureADSamples/WebAPI-OnBehalfOf-DotNet).
