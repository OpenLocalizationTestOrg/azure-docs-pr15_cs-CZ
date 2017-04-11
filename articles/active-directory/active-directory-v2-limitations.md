<properties
    pageTitle="Omezení verze 2.0 koncového bodu a omezení | Microsoft Azure"
    description="Seznam omezení a omezení s koncovým verze 2.0 Azure AD."
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

# <a name="should-i-use-the-v20-endpoint"></a>Mám používat koncový bod verze 2.0?

Při vytváření aplikace, které integrace se službou Azure Active Directory, musíte se rozhodnout, jestli protokoly koncového bodu a ověření verze 2.0 vašim potřebám.  Původní koncový bod Azure AD pořád plně podporované a v některých ohledem další funkce ve formátu RTF než verze 2.0.  Však verze 2.0 koncový bod [přináší spoustu značných výhod](active-directory-v2-compare.md) pro vývojáře, které může přesvědčit použít nový model programování.

V tomto okamžiku naší při použití koncového bodu verze 2.0 doporučujeme následujícím způsobem:

- Pokud chcete podporu osobní účtů Microsoft v aplikaci, používejte koncový bod verze 2.0.  Ale zkontrolujte, že chápete omezení uvedené v tomto článku, zejména těch, které se týkají speciálně pro práci a školní účty.
- Pokud vaše aplikace pouze vyžaduje podpůrné práce a školních účtů, byste měli použít [Původní koncové body Azure AD](active-directory-developers-guide.md).

V průběhu času koncový bod verze 2.0 rozrůstat eliminuje omezení uvedený tady, takže byste potřebovali pouze kdykoliv použít koncový bod verze 2.0.  Tento článek je určený mezitím pomoct zjistit, jestli je článek pro vás koncový bod verze 2.0.  Budeme dál aktualizovat v tomto článku v čase obsahovat aktuální stav koncový bod verze 2.0, takže sledujte zpátky na přehodnocovat vašim požadavkům s možnostmi verze 2.0.

Pokud máte existující aplikace s Azure AD, která nepoužívá koncový bod verze 2.0, je potřeba vytvořit od začátku.  V budoucnu budeme poskytovat způsob, jak můžete povolit existující aplikace Azure AD pomocí verze 2.0 koncového bodu.

## <a name="restrictions-on-apps"></a>Omezení aplikace
Následující typy aplikací aktuálně nepodporuje koncový bod verze 2.0.  Popis podporované typy aplikací naleznete [v tomto článku](active-directory-v2-flows.md).

##### <a name="standalone-web-apis"></a>Rozhraní API samostatného Web
Pomocí verze 2.0 koncový bod máte možnost [vytvořit rozhraní API webových, která je zabezpečená pomocí OAuth 2.0](active-directory-v2-flows.md#web-apis).  Však tohoto rozhraní API webových pouze budou moct přijímat tokenů z aplikace, který sdílí stejné ID aplikace.  Vytváření webového rozhraní API, který je přístupný z klienta se jiné Id aplikace není podporovaná.  Tohoto klienta nebude moct požádat o nebo získejte oprávnění k webu rozhraní API.

Vytvoření webového rozhraní API, která přijímá tokenů z klienta se stejným Id aplikace najdete ukázky rozhraní API webových verze 2.0 koncového bodu v [Příručce Začínáme](active-directory-appmodel-v2-overview.md#getting-started).

##### <a name="web-api-on-behalf-of-flow"></a>Webového rozhraní API na jménem Of toku
Mnoho architektury zahrnují rozhraní API webových, kterou je potřeba zavolat další podřízené rozhraní API webových, obě zajištěná koncový bod verze 2.0.  Tento postup je běžný nativní klienti, které mají rozhraní API webových back-end systému, který volá online službu Microsoftu nebo jiného vlastní integrované webového rozhraní API, který podporuje Azure AD.

Tento scénář můžete podporována formátu grant OAuth 2.0 Jwt nosný pověření, jinak jmenoval On-Behalf-Of tok.  Tok zapnuto-jménem z však není aktuálně podporován koncového bodu verze 2.0.  Jak funguje tento toku v obecně dostupných Azure AD služeb, podívejte se [na-jménem-z příkladu na GitHub](https://github.com/AzureADSamples/WebAPI-OnBehalfOf-DotNet).

## <a name="restrictions-on-app-registrations"></a>Omezení registrace aplikace
Všechny aplikace, které chcete integrovat s koncovým verze 2.0 v tomto okamžiku v okamžiku, musíte vytvořit novou registraci aplikace v [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Existující Azure AD Account Microsoft aplikace nebude kompatibilní s koncovým verze 2.0, ani bude aplikace registrované všechny portálu kromě novém portálu registrace aplikace.  Plánujeme možnost, jak povolit stávajících aplikací pro použití jako z verze 2.0 aplikace. V současné době Ačkoli, se nezobrazí žádná cestu migrace pro aplikace koncový bod verze 2.0.

Podobně aplikace registraci v novém portálu registrace aplikace nebude fungovat proti původní koncový bod Azure AD ověřování.  Ale můžete aplikace vytvořené v portálu registrace aplikace úspěšně integrace s koncovým ověření účtu Microsoft, `https://login.live.com`.

Kromě toho registrace aplikace vytvořené v [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) mají následující upozornění:

- **Domovské stránce** , označovaná taky jako **Adresa URL přihlašování** není podporována.  Bez domovskou stránku tyto aplikace nezobrazí v panelu Moje aplikace Office.
- Pouze dvě tajemství aplikace jsou povolených pro jednoho aplikace Id v současné době.
- Registrace aplikace můžete jenom zobrazit a spravovat pomocí účtu, karta Vývojář v jedné.  Nejde sdílet mezi více vývojářů.
- Existuje několik omezení formátu redirect_uri povolené.  Následující části Podrobnosti.

## <a name="restrictions-on-redirect-uris"></a>Omezení URI přesměrování
Aplikace, které jsou registrované v novém portálu registrace aplikace jsou aktuálně omezeny omezenou sadu hodnot redirect_uri.  Redirect_uri pro webové aplikace a služby vždy začíná schématu `https`, a všechny hodnoty redirect_uri sdílejí jednu doménu DNS.  Například není možné k registraci do webových aplikací, který má redirect_uris:

`https://login-east.contoso.com`  
`https://login-west.contoso.com`

Registračního systému porovnává celé název DNS existující redirect_uri s názvem DNS redirect_uri, kterou chcete přidat. Žádost a přidejte jméno DNS se nezdaří splnění některá z následujících podmínek:  

- Pokud celý název DNS nové redirect_uri neodpovídá DNS název existující redirect_uri
- nejsou-li celý název DNS nové redirect_uri poddomény existující redirect_uri

Pokud například aplikaci právě redirect_uri:

`https://login.contoso.com`

Je možné přidat:

`https://login.contoso.com/new`

které přesně shoduje s názvem DNS nebo:

`https://new.login.contoso.com`

tedy subdoménu DNS z login.contoso.com.  Pokud chcete mít aplikace, která obsahuje přihlášení east.contoso.com a přihlášení west.contoso.com jako redirect_uris a potom musíte přidat následující redirect_uris v pořadí:

`https://contoso.com`  
`https://login-east.contoso.com`  
`https://login-west.contoso.com`  

Poslední dvě můžete přidat, protože jsou subdomén první redirect_uri contoso.com. Toto omezení se v nadcházející verzi odeberou.

Zjistěte, jak si zaregistrovat aplikace v novém portálu registrace aplikace, najdete pod odkazy [v tomto článku](active-directory-v2-app-registration.md).

## <a name="restrictions-on-services--apis"></a>Omezení služby a rozhraní API
Koncový bod verze 2.0 aktuálně podporuje přihlášení k libovolné aplikaci registraci v novém portálu registrace aplikace, za předpokladu, že spadá do seznamu toků [podporovaného ověřování](active-directory-v2-flows.md).  Však tyto aplikace pouze budou moct získat tokeny aplikace access OAuth 2.0 pro velmi omezené množství zdrojů.  Koncový bod verze 2.0 vydá pouze access_tokens pro:

- Aplikaci požadovanou tokenu.  Aplikace můžou získávat access_token pro sebe, pokud logické aplikace se skládá z několika různých složek nebo vrstvy.  Pokud chcete zobrazit tento scénář v akci, podívejte se na naše výukové programy [Začínáme](active-directory-appmodel-v2-overview.md#getting-started) .
- Outlook pošta, kalendář a kontakty ZBÝVAJÍCÍ rozhraní API, které se nacházejí v https://outlook.office.com.  Jak psát aplikace, která má přístup k těchto rozhraní API, najdete pod odkazy na tyto výukové programy [Začínáme Office](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2) .
- Rozhraní API aplikace Microsoft Graph.  Informace o aplikaci Microsoft Graph a všechna data, která je k dispozici, navštěvujte blog o [https://graph.microsoft.io](https://graph.microsoft.io).

V současné době podporuje dalšími službami.  Další služby Microsoft Online se přidají v budoucnu a podpora pro vlastní vytvořená rozhraní API webových a služeb.

## <a name="restrictions-on-libraries--sdks"></a>Omezení knihovny a SDK
Podpora knihovny koncového bodu verze 2.0 je velmi omezené v současné době.  Pokud chcete použít koncový bod verze 2.0 v praxi, máte následující možnosti:

- Pokud vytváříte webovou aplikaci, bezpečně můžete naší obecně dostupných middleware serverovou k provádění přihlásit a token ověření.  Jedná se o middleware OWIN otevřít ID připojit se pro naše NodeJS Passport plug-in a ASP.NET.  Ukázky pomocí našich middleware jsou k dispozici v naší [Začínáme](active-directory-appmodel-v2-overview.md#getting-started) oddíl.
- Pro jiné platformy a pro nativní a mobilní aplikace můžete také integrovat s koncovým verze 2.0 přímo odesílání a přijímání zpráv protokol v kódu aplikace.  Verze 2.0 OpenID připojení a OAuth protokoly [výslovně popsány](active-directory-v2-protocols.md) můžete provést tyto integrace.
- Nakonec můžete otevřít zdroj otevřete připojení ID a OAuth knihoven integrace s koncový bod verze 2.0.  Protokol verze 2.0 by měla být kompatibilní s více knihovnami protokol otevřít zdroj bez nejdůležitějších změn.  Těchto knihoven dostupnost na jazyk, tak i platformu a weby [Otevřít připojení ID](http://openid.net/connect/) a [OAuth 2.0](http://oauth.net/2/) spravovat seznam oblíbených implementace. Další informace a seznam knihoven klienta otevřít zdroj a ukázky, které jste otestovali s koncovým verze 2.0 najdete v článku [verze 2.0 a ověřování knihoven Azure Active Directory (AD)](active-directory-v2-libraries.md) .

Také vydala počáteční Náhled [Microsoft Authentication Library (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) pro .NET pouze.  Měli byste být Vítá vás vyzkoušet si tuto knihovnu aplikace .NET klienta a serveru, ale jako náhled knihovnu, kterou je nedojde k GA kvalitní podporovat.

## <a name="restrictions-on-protocols"></a>Omezení protokoly
Koncový bod verze 2.0 podporuje pouze otevřené připojení ID & OAuth 2.0.  Ale ne všech funkcí a možností každý protokol byly zahrnuty do koncový bod verze 2.0, včetně:

- Webová část Seznam připojit OpenID `end_session_endpoint`, což umožňuje aplikaci ukončit relaci s koncovým verze 2.0.
- id_tokens vydán koncový bod verze 2.0 obsahovat pouze párový identifikátor pro uživatele.  To znamená, že dva různé aplikace dostanou různých ID pro jednoho uživatele.  Všimněte si, že pomocí dotazu aplikace Microsoft Graph `/me` koncový bod, který bude moct získání correlatable ID pro uživatele, které můžete používat ve všech aplikacích.
- neobsahují id_tokens vydán koncový bod verze 2.0 `email` převzetí pro uživatele v současné době i v případě získání oprávnění od uživatele zobrazíte jeho e-mailu.
- Informace o uživateli připojení OpenID koncový bod. Informace o uživateli koncového bodu není implementovaná na koncový bod verze 2.0 v současné době.  Všechna uživatelská data profilu potenciálně obdržíte na tomto koncovém bodu je však k dispozici z aplikace Microsoft Graph `/me` koncového bodu.
- Role a deklarace skupiny.  V současné době koncový bod verze 2.0 vydávající role nebo skupinu nároky v id_tokens nepodporuje.

Pochopíte lépe obor protokol funkce podporované v koncový bod verze 2.0, přečtěte si prostřednictvím naše [OpenID připojení a OAuth 2.0 protokol Reference](active-directory-v2-protocols.md).

## <a name="restrictions-for-work--school-accounts"></a>Omezení pro pracovní a školní účty
Máte specifické pro Microsoft organizace/podnikoví uživatelé, které zatím nepodporuje koncový bod verze 2.0 několik funkcí.

##### <a name="device-based-conditional-access-native-and-mobile-apps-and-the-microsoft-graph"></a>Podmíněného přístupu na základě zařízení, nativními a mobilní aplikace a aplikace Microsoft Graph
Koncový bod verze 2.0 ještě podporuje ověřování zařízení pro mobilní a nativní aplikacemi, třeba nativní aplikace systémem iOS nebo Android.  To může blokovat nativní aplikaci z volání aplikace Microsoft Graph některých organizacích.  Ověřování zařízení se vyžaduje správce službou zásadu podmíněné přístupu na základě zařízení na aplikace.  Koncový bod verze 2.0 pravděpodobně scénáře pro podmíněné přístupu na základě zařízení je Správce nastavení zásad na zdroj v aplikaci Microsoft Graph, například rozhraní API aplikace Outlook.  Pokud správce nastaví tuto zásadu a nativní aplikace požaduje token do aplikace Microsoft Graph, žádost nakonec nezdaří, protože ověřování zařízení není zatím podporované.  Webové aplikace, které vyžadují tokeny do aplikace Microsoft Graph jsou však podporovány, pokud jsou nakonfigurované zásady založené na zařízení.  Na webu aplikace scénář zařízení ověřování uživatele webového prohlížeče.

Vývojář pravděpodobně máte žádný ovládací prvek nad při zásady se nastavují v aplikaci Microsoft Graph zdroje, nebo dokonce vědět, když se to děje.  Pokud vytváříte aplikace pro pracovní a školní uživatele, byste měli použít [Původní koncový bod Azure AD](active-directory-developers-guide.md) dokud koncový bod verze 2.0 podporuje ověřování zařízení.  Další informace o podmíněné přístupu na základě zařízení podívejte se na [Tento článek](active-directory-conditional-access.md#device-based-conditional-access).

##### <a name="windows-integrated-authentication-for-federated-tenants"></a>Ověřování systému Windows pro federované klientů
Pokud jste vypotřebovali ADAL (a tedy původní koncový bod Azure AD) v aplikacích systému Windows, pravděpodobně v režimu výhod takzvanou udělení SAML vyhodnocení výrazu.  Tento grant umožňuje uživatelům federované Azure AD klienti tiše ověření s jejich instancí služby Active Directory v místní bez nutnosti zadávat přihlašovací údaje.  Udělení výraz SAML aktuálně nepodporuje na koncový bod verze 2.0.
