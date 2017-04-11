<properties
   pageTitle="Příručka Azure Active Directory vývojář | Microsoft Azure"
   description="Tento článek obsahuje komplexní příručku k prostředkům orientovaného vývojář služby Azure Active Directory."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/24/2016"
   ms.author="mbaldwin"/>


# <a name="azure-active-directory-developers-guide"></a>Příručka Azure Active Directory vývojář

## <a name="overview"></a>Základní informace
Jako Správa identit jako platformu služby (IDMaaS) Azure Active Directory (AD) obsahuje vývojáři efektivní způsob správy identit integrovat do svých aplikací. V následujících článcích poskytují přehled o implementaci a klíčové funkce Azure AD. Doporučujeme číst postupně nebo přejít na [Začínáme](#getting-started) Pokud budete chtít dostanete.


1. [Výhody Azure AD integrace](./develop/active-directory-how-to-integrate.md): zjistit, proč integrace se službou Azure AD nabízí nejlepším řešením pro zabezpečené přihlášení a se tak mohli ověřovat.

1. [Scénáře ověřování Azure AD](active-directory-authentication-scenarios.md): Využijte výhod zjednodušené ověřování v Azure AD poskytnout přihlašování do aplikace.

1. [Integrace aplikace s Azure AD](active-directory-integrating-applications.md): Přečtěte si, jak přidat, aktualizovat a odebrání aplikace z Azure AD a informace o konfiguraci pokyny pro integrované aplikace.

1. [Rozhraní API Azure AD grafu](active-directory-graph-api.md): použití rozhraní API Azure AD grafu programově přístup k Azure AD pomocí rozhraní REST API koncové body. Rozhraní API Azure AD graf je také k dispozici prostřednictvím [Aplikace Microsoft Graph](https://graph.microsoft.io/). Microsoft Graph jsou k dispozici jednotné rozhraní API, který umožňuje přístup k několika cloudové službě Microsoftu rozhraní API, až jeden koncový bod rozhraní REST API a pomocí jednoho přístupový token.

1. [Azure AD ověřování knihoven](active-directory-authentication-libraries.md): snadno ověřování uživatelů pomocí knihoven ověřování Azure AD pro .NET, JavaScript, a získat přístup tokeny cíle-C, Android a další.


## <a name="getting-started"></a>Začínáme

Tyto kurzy jsou přizpůsobené oprávněnými a můžete začít vývoj s Azure Active Directory. Jako předpoklad musíte [získat tenanta služby Azure Active Directory](active-directory-howto-tenant.md).

### <a name="mobile-and-pc-application-quick-start-guides"></a>Rychlé úvodní příručky k počítače a mobilní aplikace

|[![iOS](./media/active-directory-developers-guide/ios.png)](active-directory-devquickstarts-ios.md)|[![Android](./media/active-directory-developers-guide/android.png)](active-directory-devquickstarts-android.md)|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-dotnet.md)|[![Univerzální systému Windows](./media/active-directory-developers-guide/windows.png)](active-directory-devquickstarts-windowsstore.md)|[![Xamarin](./media/active-directory-developers-guide/xamarin.png)](active-directory-devquickstarts-xamarin.md)|[![Cordova](./media/active-directory-developers-guide/cordova.png)](active-directory-devquickstarts-cordova.md)|[![OAuth 2.0](./media/active-directory-developers-guide/oauth-2.png)](active-directory-protocols-oauth-code.md)
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|[iOS](active-directory-devquickstarts-ios.md)|[Android](active-directory-devquickstarts-android.md)|[.NET](active-directory-devquickstarts-dotnet.md)|[Univerzální systému Windows](active-directory-devquickstarts-windowsstore.md)|[Xamarin](active-directory-devquickstarts-xamarin.md)|[Cordova](active-directory-devquickstarts-cordova.md)|[Integrovat OAuth 2.0](active-directory-protocols-oauth-code.md)|

### <a name="web-application-quick-start-guides"></a>Rychlé úvodní příručky k webové aplikace

|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapp-dotnet.md)|[![Java](./media/active-directory-developers-guide/java.png)](active-directory-devquickstarts-webapp-java.md)|[![AngularJS](./media/active-directory-developers-guide/angularjs.png)](active-directory-devquickstarts-angular.md)|[![JavaScript](./media/active-directory-developers-guide/javascript.png)](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi)|[![Node.js](./media/active-directory-developers-guide/nodejs.png)](active-directory-devquickstarts-openidconnect-nodejs.md) | [![Připojení OpenID](./media/active-directory-developers-guide/openid-connect.png)](active-directory-protocols-openid-connect-code.md)
|:--:|:--:|:--:|:--:|:--:|:--:|
|[.NET](active-directory-devquickstarts-webapp-dotnet.md)|[Java](active-directory-devquickstarts-webapp-java.md)|[AngularJS](active-directory-devquickstarts-angular.md)|[JavaScript](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi)|[Node.js](active-directory-devquickstarts-openidconnect-nodejs.md)|[Integrovat OpenID připojení](active-directory-protocols-openid-connect-code.md)|

### <a name="web-api-quick-start-guides"></a>Průvodce rychlým startem webového rozhraní API

|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapi-dotnet.md)|[![Node.js](./media/active-directory-developers-guide/nodejs.png)](active-directory-devquickstarts-webapi-nodejs.md)
|:--:|:--:|
|[.NET](active-directory-devquickstarts-webapi-dotnet.md)|[Node.js](active-directory-devquickstarts-webapi-nodejs.md)

### <a name="querying-the-directory-quickstart-guide"></a>Dotaz na Průvodce rychlým startem adresáře

| [![.NET](./media/active-directory-developers-guide/graph.png)](active-directory-graph-api-quickstart.md)|
|:--:|
|[Rozhraní API graf](active-directory-graph-api-quickstart.md)|

## <a name="how-tos"></a>Postupy

Tyto články popisují provádění konkrétních úkolů pomocí služby Azure Active Directory:

- [Získání Azure AD klienta](active-directory-howto-tenant.md)
- [Přihlaste se všichni uživatelé Azure AD pomocí vzorku více klienta aplikace](active-directory-devhowto-multi-tenant-overview.md)
- Povolení více aplikací jednotné přihlašování pomocí ADAL, na [Android](active-directory-sso-android.md) a na zařízení s [iOS](active-directory-sso-ios.md)
- [Zkontrolujte aplikace AppSource certifikované pro Azure AD](active-directory-devhowto-appsource-certified.md)
- [Seznam aplikace v galerii Azure AD aplikace](active-directory-app-gallery-listing.md)
- [Odeslání webové aplikace pro Office 365 na řídicím panelu prodejce](https://msdn.microsoft.com/office/office365/howto/submit-web-apps-seller-dashboard)
- [Princip manifest aplikace služby Azure Active Directory](active-directory-application-manifest.md)
- [Princip brandingu pokyny pro pořízení tlačítka přihlášení a aplikace v klientské aplikaci](active-directory-branding-guidelines.md)
- [Náhled: Jak vytvářet aplikace, které uživatelé pomocí jak osobní a pracovní nebo školní účty](active-directory-appmodel-v2-overview.md)
- [Náhled: Jak vytvářet aplikace, které si zaregistrovali a přihlaste se uživatelé](../active-directory-b2c/active-directory-b2c-overview.md)
- [Náhled: Konfigurace tokenu životnost v Azure AD](active-directory-configurable-token-lifetimes.md) pomocí Powershellu. Najdete v článku [operace zásady](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) a [zásady entity](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#policy-entity) podrobnosti o konfiguraci prostřednictvím rozhraní API Azure AD grafu.

## <a name="reference"></a>Odkaz

Tyto články základem foundation ZBÝVAJÍCÍ a ověřování knihovny rozhraní API protokoly, chyby, ukázek kódu a koncové body.  

###  <a name="support"></a>Podpora
- [Otázky značkami](http://stackoverflow.com/questions/tagged/azure-active-directory): vyhledání Azure Active Directory řešení přetečení zásobníku vyhledáním značky [azure active directory](http://stackoverflow.com/questions/tagged/azure-active-directory) a [adal](http://stackoverflow.com/questions/tagged/adal).
- Přejděte na téma [Glosář vývojář Azure AD](active-directory-dev-glossary.md) pro definice některých z nejčastěji používané podmínky vztahující se k vývoji aplikací a integrace.

### <a name="code"></a>Kód

- [Knihovny zdrojů otevřít Azure Active Directory](http://github.com/AzureAD): je nejjednodušším způsobem vyhledání zdroje knihovny pomocí náš [seznam knihovna](active-directory-authentication-libraries.md).

- [Ukázky Azure Active Directory](https://github.com/azure-samples?query=active-directory): nejjednodušší přejděte na seznam ukázky je pomocí [Rejstřík ukázek kódu](active-directory-code-samples.md).

- [Active Directory Authentication knihovny (ADAL) pro .NET](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet) - si přečtěte následující dokumentaci odkaz je dostupný u [nejnovější hlavní verze](https://docs.microsoft.com/active-directory/adal/microsoft.identitymodel.clients.activedirectory) a [předchozí hlavní verze](https://docs.microsoft.com/active-directory/adal/v2/microsoft.identitymodel.clients.activedirectory).

### <a name="graph-api"></a>Rozhraní API graf

- [Rozhraní API diagram reference](https://msdn.microsoft.com/library/azure/hh974476.aspx): Přehled ZBÝVAJÍCÍ pro Azure Active Directory grafu rozhraní API. [Zobrazit interaktivní referenční zkušenosti rozhraní API grafu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

- [Rozhraní API grafu oprávnění obory](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes): obory oprávnění OAuth 2.0, které se používají k řízení přístupu, který má aplikace dat adresáře na klientovi.

### <a name="authentication-and-authorization-protocols"></a>A mohli ověřovat protokoly

- [Podepsání klíče při přechodu myší v Azure AD](active-directory-signing-key-rollover.md): Další informace o Azure AD podepisování cadence přechodu klíče a k aktualizaci klíč pro obvyklé scénáře aplikace.

- [OAuth 2.0 Protocol (protokol): udělení kód se tak mohli ověřovat pomocí](active-directory-protocols-oauth-code.md): protokol OAuth 2.0 se tak mohli ověřovat kód grant (udělit), můžete povolit přístup k webovým aplikacím a klient webového rozhraní API v Azure Active Directory.

- [OAuth 2.0 Protocol (protokol): Principy udělení implicitní](active-directory-dev-understanding-oauth2-implicit-grant.md): Další informace o udělení implicitní autorizace a jestli je nejlepší pro aplikaci.

- [OAuth 2.0 Protocol (protokol): Služba pověření klientů pomocí služby volání](active-directory-protocols-oauth-service-to-service.md): pověření klienta 2.0 OAuth grant (udělit) umožňuje webové služby (důvěrné klienta) k použití vlastní přihlašovací údaje k ověření při volání jiné webové služby, místo zosobnění uživatele. V tomto scénáři klienta je obvykle vícevrstvé webové služby, démon služby nebo Web.

- [Protokol OpenID připojení 1.0: přihlášení a ověřování](active-directory-protocols-openid-connect-code.md): protokol OpenID připojení 1.0 rozšiřuje OAuth 2.0 pro použití ověřovací protokol. Klientská aplikace můžete přijímat id_token spravovat proces přihlášení nebo rozšířit tok se tak mohli ověřovat kódu pro příjem e id_token a povolení kód.

- [Odkaz protokol SAML 2.0](active-directory-saml-protocol-reference.md): protokol SAML 2.0 aplikacím svým uživatelům poskytovat jednoho prostředí přihlašování.

- [WS Federation 1.2 Protocol (protokol)](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html): podporuje Azure Active Directory WS Federation 1.2 podle specifikaci webové služby federace verze 1.2. Další informace o dokument metadat federace najdete v článku [Federace Metadata](active-directory-federation-metadata.md).

- [Podporované typy token a deklarace](active-directory-token-and-claims.md): můžete použít tato příručka, pochopit a posoudit v tokeny SAML 2.0 a JSON Web tokeny (JWT).

## <a name="videos"></a>Videa

### <a name="build"></a>Tvůrce dotazů

Tyto přehled prezentace ve vývoji aplikací pomocí služby Azure Active Directory funkcí reproduktory, kteří pracují přímo v technickým týmu. Prezentace vysvětlovat základních témata, včetně IDMaaS ověřování, federaci identity a jednotné přihlašování.

- [Identit: Stav Union a budoucí směru](https://azure.microsoft.com/documentation/videos/build-2016-microsoft-identity-state-of-the-union-and-future-direction/)
- [Azure Active Directory: Správa identit jako služba pro moderní aplikace](https://azure.microsoft.com/documentation/videos/build-2015-azure-active-directory-identity-management-as-a-service-for-modern-applications/)
- [Můžete vyvíjet moderní webové aplikace pomocí služby Azure Active Directory](https://azure.microsoft.com/documentation/videos/build-2015-develop-modern-web-applications-with-azure-active-directory/)
- [Můžete vyvíjet moderní nativní aplikace pomocí služby Azure Active Directory](https://azure.microsoft.com/documentation/videos/build-2015-develop-modern-native-applications-with-azure-active-directory/)

### <a name="azure-friday"></a>Azure pátek
[Azure pátek](https://azure.microsoft.com/documentation/videos/azure-friday/) je opakované pátek 1:1 řada videí, která se snaží o jejich přenesením je krátké (10 – 15 minut) dotazuje s odborníky na různých Azure témata.  Použijte funkci Filtr služby na stránce zobrazit všechna videa Azure Active Directory.

- [Azure Identity 101](https://azure.microsoft.com/documentation/videos/azure-identity-basics/)
- [Azure identit 102](https://azure.microsoft.com/documentation/videos/azure-identity-creating-active-directory/)
- [Azure identit 103](https://azure.microsoft.com/documentation/videos/azure-identity-application-to-authenticate/)

## <a name="social"></a>Sociální

- [Active Directory týmového blogu](http://blogs.technet.com/b/ad/): nejnovější vývoj ve světě Azure Active Directory.

- [Azure Active Directory graf týmového blogu](http://blogs.msdn.com/b/aadgraphteam): Azure Active Directory informace, které jsou specifické pro rozhraní API graf.

- [Identita cloudu](http://www.cloudidentity.net): názory na Správa identit jako službu z hlavní Azure Active Directory odp.  

- [Azure Active Directory na Twitter](https://twitter.com/azuread): oznámení služby Azure Active Directory v 140 znaků.

## <a name="windows-server-on-premises-development"></a>Vývojové místního systému Windows Server
Použití vývoj systému Windows Server a Active Directory Federation Services služby AD FS (), najdete v článku:

- [AD FS scénáře pro vývojáře](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/ad-fs-scenarios-for-developers): Přehled součástí služby AD FS a jak to funguje, se v detailech na scénáře podporovaného ověřování/se tak mohli ověřovat.
- [Služba AD FS návody](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/ad-fs-development): seznam walk-through článků, které obsahují podrobné pokyny o implementaci toků související ověřování/se tak mohli ověřovat.
