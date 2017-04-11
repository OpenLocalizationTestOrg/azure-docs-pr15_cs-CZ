<properties
    pageTitle="Přehled koncový bod verze 2.0 | Microsoft Azure"
    description="Úvod do vytváření aplikací klasické Account Microsoft a služby Azure Active Directory přihlášení."
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
    ms.date="09/27/2016"
    ms.author="dastrock"/>

# <a name="sign-in-microsoft-account--azure-ad-users-in-a-single-app"></a>Přihlašovací & Account Microsoft Azure AD uživatelů v jedné aplikaci

V minulosti byl integrovat ve dvou samostatných systémech povinné vývojář aplikace, který chtěli podporují účtů Microsoft a služby Azure Active Directory.  Zavedli jsme teď novou verzi rozhraní API ověřování, který umožňuje přihlašování uživatelů pomocí obou typů účtů v systému Azure AD.  Tento zapnutým ověřování systému se označuje jako **koncový bod verze 2.0**.  S koncovým verze 2.0 jeden Jednoduchá integrace vám umožní dosažení cílové skupiny, který přesahuje miliony uživatele s osobní a pracovní/školní účty.

Aplikace, které používají koncový bod verze 2.0 můžete taky používat REST API z [Aplikace Microsoft Graph](https://graph.microsoft.io) a [Office 365](https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2) pomocí některý typ účtu.

<!-- For a quick introduction to the v2.0 endpoint, please view the [Getting Started with Microsoft Identities: Enterprise Grade Sign In For Your Apps](https://azure.microsoft.com/documentation/videos/build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps/) video. -->

## <a name="getting-started"></a>Začínáme
[AZURE.VIDEO build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps]

Vyberte svoji Oblíbené platformu tento seznam můžete vytvořit aplikaci pro použití naše otevřít zdroj knihoven a rámce.  Můžete taky můžete naše OAuth 2.0 & OpenID připojení protokol si přečtěte následující dokumentaci odesílat a přijímat zprávy protokol přímo bez použití auth knihovny.

<!-- TODO: Finalize this table  -->
[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-v2-quickstart-table.md)]

## <a name="whats-new"></a>Co je nového
Informací vysvětlujících tady bude užitečný porozumět tomu, co je a co není možné s koncový bod verze 2.0.

- Další informace o [typech aplikace, které je možné vytvářet s koncovým verze 2.0](active-directory-v2-flows.md).
- Porozumíte [omezení, omezení a omezení](active-directory-v2-limitations.md) s koncovým verze 2.0.
- Přidali jsme naposledy podpora pro [Správce omezení obory](active-directory-v2-scopes.md) a [OAuth2 klienta udělit přihlašovací údaje](active-directory-v2-protocols-oauth-client-creds.md).  Vyzkoušejte je!

## <a name="reference"></a>Odkaz
Tyto odkazy mohou být užitečné pro prohlížení platformu podrobně:

- Sestavení 2016: [Začínáme s Microsoft identitami: Enterprise testu přihlásit pro aplikace](https://azure.microsoft.com/documentation/videos/build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps/)
- Nápovědu k přetečení zásobníku služby [azure active directory](http://stackoverflow.com/questions/tagged/azure-active-directory) nebo [adal](http://stackoverflow.com/questions/tagged/adal) značky.
- [Odkaz protokol verze 2.0](active-directory-v2-protocols.md)
- [Odkaz tokenu verze 2.0](active-directory-v2-tokens.md)
- [Reference knihovny verze 2.0](active-directory-v2-libraries.md)
- [Obory a souhlasu koncový bod verze 2.0](active-directory-v2-scopes.md)
- [Registrace aplikace Microsoft portálu](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)
- [Office 365 REST API Reference](https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2)
- [Aplikace Microsoft Graph](https://graph.microsoft.io)