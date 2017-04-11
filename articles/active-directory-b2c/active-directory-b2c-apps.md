<properties
    pageTitle="Azure AD B2C | Microsoft Azure"
    description="Typy aplikací, je možné vytvářet v Azure Active Directory B2C."
    services="active-directory-b2c"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-active-directory-b2c-types-of-applications"></a>Azure Active Directory B2C: Typy aplikací

Azure Active Directory (Azure AD) B2C podporuje ověřování pro řadu architektury moderní aplikace. Všechny uvedené vycházejí standardních protokolů [OAuth 2.0](active-directory-b2c-reference-protocols.md) nebo [OpenID připojení](active-directory-b2c-reference-protocols.md). Tento dokument stručně popisuje typy aplikací, které můžete vytvářet, nezávisle na jazyk nebo platformu dáváte přednost. Ho taky vám pomůže pochopit uceleném scénáře před [začít vytvářet aplikace](active-directory-b2c-overview.md#getting-started).

## <a name="the-basics"></a>Základní informace
Všechny aplikace, která používá Azure AD B2C musí být registrován v [adresáři B2C](active-directory-b2c-get-started.md) prostřednictvím [Portálu Azure](https://portal.azure.com/). Proces registrace aplikace shromažďuje a přiřadí několika hodnot do aplikace:

- **ID aplikace** , které jednoznačně identifikuje aplikace.
- **Přesměrovat URI** , něhož chcete směrovat odpovědi zpátky do aplikace.
- Další scénář specifické hodnoty. Další informace, zjistěte, jak si [zaregistrovat aplikace](active-directory-b2c-app-registration.md).

Po registraci aplikace ho informuje uživatele o s Azure AD tak, že požadavky na koncový bod Azure AD verze 2.0:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Každý požadavek odesílaný velkému Azure AD B2C Určuje **zásad**. Zásady řídí chování Azure AD. K vytvoření přizpůsobit sadu uživatelského prostředí můžete taky použít tyto koncové body. Běžné zásady patří registrace, přihlásit a profilu upravit zásady. Pokud znáte není zásad, přečtěte si o Azure AD B2C [framework extensible zásad](active-directory-b2c-reference-policies.md) až potom pokračujte.

Interakce každé aplikace s koncovým verze 2.0 spočívá v podobné uceleném vzorek:

1. Uživatele koncového bodu verze 2.0 provést [zásad](active-directory-b2c-reference-policies.md)směrovat aplikace.
2. Uživatel dokončí zásad podle definici zásad.
4. Aplikace obdrží určitý druh tokenu zabezpečení z verze 2.0 koncového bodu.
5. Aplikace používá tokenu zabezpečení pro přístup k chráněné informace nebo chráněného zdroj.
6. Server prostředků ověří tokenu zabezpečení pro ověření, že můžete mít udělený přístup.
7. Aplikaci aktualizuje pravidelně tokenu zabezpečení.

<!-- TODO: Need a page for libraries to link to -->
Tento postup se může lišit mírně v závislosti na typu aplikaci, kterou vytváříte. Otevřít zdroj knihovny můžete adresa podrobnosti.

## <a name="web-apps"></a>Webové aplikace
U webové aplikace (včetně .NET PHP, Java, skutečné, Python a Node.js), které jsou hostované na serveru a k nim získat přístup prostřednictvím prohlížeče Azure AD B2C podporuje [OpenID připojení](active-directory-b2c-reference-protocols.md) pro všechny uživatelského prostředí. Jedná se o přihlášení, registrace a Správa profilů. V Azure AD B2C provádění OpenID připojení se spustí webovou aplikaci tyto uživatelského prostředí odesláním žádosti o ověřování Azure AD. Žádost vyhodnotí `id_token`. Tento tokenu zabezpečení představuje identita uživatele. Obsahuje taky informace o uživateli ve formuláři deklarací:

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

Další informace o typech tokeny a deklarací do aplikace v [B2C token odkazovat](active-directory-b2c-reference-tokens.md).

Ve web appu se zadávají každé spuštění [zásad](active-directory-b2c-reference-policies.md) uceleném takto:

![Obrázek plaveckých drah Web App](./media/active-directory-b2c-apps/webapp.png)

Ověření `id_token` pomocí veřejný klíč podpisu, který je dostali z Azure AD stačí k ověření identity uživatele. Nastaví také cookie relace, který slouží k identifikaci uživatele na požadavky na pozdější stránku.

Pokud chcete zobrazit tento scénář v akci, použijte jeden z web app přihlašovací ukázky v naší [že Začínáme oddíl](active-directory-b2c-overview.md#getting-started).

Kromě usnadnění jednoduché přihlásit do webových aplikací serveru může taky potřebují dostat k back-end webové služby. V tomto případě web appu můžete provádět mírně odlišnou [OpenID připojení toku](active-directory-b2c-reference-oidc.md) a získat tokeny pomocí kódů se tak mohli ověřovat a aktualizujte tokeny. Tento scénář je znázorněno na následujícím [rozhraní API webové části](#web-apis).

<!--, and in our [WebApp-WebAPI Getting started topic](active-directory-b2c-devquickstarts-web-api-dotnet.md).-->

## <a name="web-apis"></a>Rozhraní API webových
Azure AD B2C umožňuje zabezpečená webových služeb, jako je vaše aplikace RESTful webového rozhraní API. Webového rozhraní API umožňuje OAuth 2.0 ověřování příchozí žádosti HTTP používání tokenů zabezpečení související data. Volající webového rozhraní API připojí token v záhlaví se tak mohli ověřovat žádost HTTP:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

Webového rozhraní API můžete používat tokenu k ověření identity volající rozhraní API a k získání informací o volajícím z deklarace, které jsou zakódovaný v tokenu. Další informace o typech tokeny a deklarací do aplikace v [Azure AD B2C token odkazovat](active-directory-b2c-reference-tokens.md).

> [AZURE.NOTE]
    Azure AD B2C v současné době podporuje pouze webového rozhraní API, která jsou přístupné pro svoje vlastní známý klienty. Dokončení aplikace může zahrnovat aplikace pro iOS, aplikace pro Android a back-end webového rozhraní API. Tato architektura je plně podporován. Povolení partnera klientem, například jiné aplikaci iOS, pracovat na stejném webu, který není aktuálně podporován rozhraní API. Všechny prvky dokončení aplikace musí vzájemně sdílet ID jedné aplikace.

Webového rozhraní API dostávat tokenů z mnoha typů klientů, včetně webových aplikací web apps plochu a mobilní aplikace, jedné stránky aplikací, procesy serverovou Daemon a jiných webového rozhraní API. Tady je příklad dokončení směrování do webových aplikací, který volá webového rozhraní API:

![Webové aplikace webového rozhraní API plaveckých drah obrázku](./media/active-directory-b2c-apps/webapi.png)

Další informace o kódech se tak mohli ověřovat, aktualizace tokeny a postup získání tokenů, přečtěte si o [protokolu OAuth 2.0](active-directory-b2c-reference-oauth-code.md).

Informace o zabezpečení webového rozhraní API pomocí Azure AD B2C, podívejte se na webu výukové programy pro rozhraní API v naší [Začínáme oddíl](active-directory-b2c-overview.md#getting-started).

## <a name="mobile-and-native-apps"></a>Mobile, přičemž nativní aplikace
Aplikace, které jsou nainstalované v zařízení, třeba přenosných a stolních aplikace, často potřebují dostat k služby back-end nebo webového rozhraní API jménem uživatele. Prostředí management přizpůsobený identity můžete přiřadit nativní aplikace a bezpečné volání back-end služby Azure AD B2C a [OAuth 2.0 se tak mohli ověřovat kód toku](active-directory-b2c-reference-oauth-code.md).  

V tomto toku aplikace provede [zásady](active-directory-b2c-reference-policies.md) volání a přijímání `authorization_code` z Azure AD po zadání zásadu. `authorization_code` Představuje oprávnění v aplikaci volat back-end služby jménem uživatele, kterému je aktuálně přihlášení. Pak můžete vyměňovat aplikace `authorization_code` na pozadí pro `id_token` a `refresh_token`.  Aplikaci můžete použít `id_token` ověření back-end webového rozhraní API v žádosti o HTTP. Můžete taky použít `refresh_token` získat novou `id_token` vypršení platnosti starší verzí.

> [AZURE.NOTE]
    Azure AD B2C v současné době podporuje pouze klíčovými slovy, která se používají pro přístup k back-end webové služby aplikace vlastní. Dokončení aplikace může zahrnovat aplikace pro iOS, aplikace pro Android a back-end webového rozhraní API. Tato architektura je plně podporován. Povolení aplikace pro iOS pro přístup k webu partnera rozhraní API pomocí tokeny aplikace access OAuth 2.0 není aktuálně podporován. Všechny prvky dokončení aplikace musí vzájemně sdílet ID jedné aplikace.

![Obrázek nativní aplikaci plavecké dráhy](./media/active-directory-b2c-apps/native.png)

## <a name="current-limitations"></a>Aktuální omezení
Azure AD B2C aktuálně nepodporuje tyto typy aplikací, ale jsou roadmapy. Další omezení a omezení související s Azure AD B2C jsou popsány v [omezení a omezení](active-directory-b2c-limitations.md).

### <a name="single-page-apps-javascript"></a>Jedna stránka aplikace (JavaScript)
Mnoho moderní aplikace mít jednostránkové aplikace front-end napsaný primárně v JavaScriptu. Často používají rámec například AngularJS, Ember.js nebo Durandal. Obecně dostupných služby Azure AD podporuje tyto aplikace pomocí implicitní tok OAuth 2.0. Však tento toku ještě není k dispozici v Azure AD B2C.

### <a name="daemonsserver-side-apps"></a>Procesy daemon/serverovou aplikace
Aplikace, které obsahují dlouhodobě spuštěných procesů nebo vzorců bez informací o stavu uživatele taky potřebují pro přístup k zabezpečené zdroje, jako jsou webového rozhraní API. Tyto aplikace můžete ověřit a získejte tokeny pomocí identity v aplikaci (spíše než delegované identita uživatele) pomocí klienta OAuth 2.0 toku přihlašovací údaje.

Tento vývojový se aktuálně nepodporuje Azure AD B2C. Tyto aplikace dostane tokeny až po interaktivní uživatel toku došlo k chybě.

### <a name="web-api-chains-on-behalf-of-flow"></a>Webového rozhraní API zřetězí (toku na jménem of)
Mnoho architektury zahrnout webového rozhraní API, kterou je potřeba zavolat další podřízené webového rozhraní API, kde i zabezpečená tak, že Azure AD B2C. Tento postup je běžný v původní klienti, které mají rozhraní API webových back-end. To pak zavolá online služby společnosti Microsoft, třeba rozhraní API Azure AD grafu.

Tento scénář zřetězené webového rozhraní API můžete pomocí přihlašovacích údajů grant nosný OAuth 2.0 JWT, nazývaný také toku na jménem of podporované.  Však toku na jménem of není součástí aktuálně Azure AD B2C.
