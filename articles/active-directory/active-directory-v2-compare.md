<properties
    pageTitle="Koncový bod verze 2.0 Azure AD | Microsoft Azure"
    description="Srovnání původní Azure AD a koncové body verze 2.0."
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
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="whats-different-about-the-v20-endpoint"></a>Jaký je rozdíl o koncový bod verze 2.0?

Pokud znáte Azure Active Directory nebo integrovali aplikace s Azure AD v minulosti, může být některé rozdíly koncového bodu verze 2.0, který by očekávat.  Tento dokument volá se na tyto rozdíly pro vaše principy.

> [AZURE.NOTE]
    Některé scénáře Azure Active Directory a funkce jsou podporovány koncový bod verze 2.0.  Pokud chcete zjistit, zda by měly používat koncový bod verze 2.0, přečtěte si informace o [omezení verze 2.0](active-directory-v2-limitations.md).


## <a name="microsoft-accounts-and-azure-ad-accounts"></a>Účty Microsoft a Azure AD účty
koncový bod verze 2.0 umožňuje vývojářům psát aplikace, které podporují přihlášení z účtů Microsoft Accounts a Azure AD pomocí jednoho auth koncového bodu.  To vám dává možnost zápisu aplikace úplně účtu bez ohledu na; může být ignorant typu účtu, který uživatel přihlásí se.  Samozřejmě *můžete* informovat aplikace typu použitý v relaci konkrétní účet, ale nemusíte.

Například pokud aplikace volá [Microsoft Graph](https://graph.microsoft.io), některé další funkce a data budou dostupné uživatelům enterprise, například jejich Sharepointových webů nebo dat adresáře.  Pro mnoho akce, například pro [čtení uživatele pošty](https://graph.microsoft.io/docs/api-reference/v1.0/resources/message), kód lze však zapisovat stejně pro účty Accounts Microsoft a Azure AD.  

Integrace aplikace s Microsoft Accounts a Azure AD je teď jeden jednoduchý proces.  Jednu sadu koncové body, jedné knihovně a registrace jedné aplikace můžete použít k získání přístupu k příjemce a enterprise světě.  Další informace o koncový bod verze 2.0, podívejte se [na přehled](active-directory-appmodel-v2-overview.md).


## <a name="new-app-registration-portal"></a>Nový portál registrace aplikace
koncový bod verze 2.0 můžete registrované pouze v novém umístění: [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Toto je portál, kde můžete získat Id aplikace přizpůsobení vzhledu aplikace na přihlašovací stránce a další.  Potřebujete získat přístup na portál stačí účtu Microsoft technologii – osobní nebo pracovní/školní účet.  

Budeme dál přidávat další a další funkce tento portál registrace aplikace určitou dobu.  Záměr je, že tento portál bude nové místo, kde můžete přejít ke správě nic a všechno, co máte dělat s aplikací Microsoft.


## <a name="one-app-id-for-all-platforms"></a>Jedné aplikaci Id pro všechny platformy
V původním služby Azure Active Directory pravděpodobně jste si zaregistrovali několik různých aplikací pro jeden projekt.  Byly muset používat samostatnou aplikaci registrace pro nativní zařízení a webových aplikací web apps:

![Registrace staré aplikace uživatelského rozhraní](../media/active-directory-v2-flows/old_app_registration.PNG)

Například pokud jste sestavili web a aplikace pro iOS, bylo nutné zaregistrovat samostatně, pomocí dvou různých ID aplikace.  Pokud jste měli web a back-end webového rozhraní api, pravděpodobně jste si zaregistrovali každý jako samostatnou aplikaci v Azure AD.  Pokud jste měli aplikace pro iOS a Android aplikaci, taky možná jste si zaregistrovali dva různé aplikace.  

<!-- You may have even registered different apps for each of your build environments - one for dev, one for test, and one for production. -->

Teď budete potřebovat stačí registrace jedné aplikace a jeden Id aplikace pro každou z vašich projektů.  Můžete přidat několik "platformách" pro jednotlivé projekty a poskytují příslušná data pro každou platformu, které můžete přidat.  Samozřejmě můžete vytvořit tolik aplikace jako stejně jako v závislosti na vašim požadavkům, ale pro většině případů by měl být nutné jedinou Id aplikace.

<!-- You can also label a particular platform as "production-ready" when it is ready to be published to the outside world, and use that same Application Id safely in your development environments. -->

Náš cílem je, že to bude vést k více zjednodušená správa aplikací a vývojové prostředí a vytvořit více konsolidované zobrazení jednoho projektu, který může být pracujete.


## <a name="scopes-not-resources"></a>Obory, ne prostředky
Ve službě původní Azure AD aplikace chovat jako **zdroje**nebo od příjemce tokenů.  Zdroje můžete definovat několik **obory** nebo **oAuth2Permissions** rozumí, povolení klientské aplikace požádat o tokeny tomuto zdroji pro určitou skupinu obory.  Zvažte rozhraní API Azure AD graf jako příklad zdroje:

- Identifikátor URI Resource, nebo `AppID URI`:`https://graph.windows.net/`
- Obory, nebo `OAuth2Permissions`: `Directory.Read`, `Directory.Write`atd.  

Všechny informace o platí pro koncový bod verze 2.0.  Aplikaci můžete pořád chovat jako zdroje, definování oborů a označenými identifikátor URI.  Klient aplikace můžete pořád požádat o přístup k těmto obory.  Však byl změněný způsob, ve kterém klient požaduje tato oprávnění.  V minulosti OAuth 2.0 povolte žádosti o Azure AD může mít vypadalo:

```
GET https://login.microsoftonline.com/common/oauth2/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&resource=https%3A%2F%2Fgraph.windows.net%2F
...
```

Pokud parametr **zdroje** uvedeno, které zdroje aplikaci Klient požaduje se tak mohli ověřovat pro.  Azure AD vypočítanou informace o oprávněních požadovaných podle aplikace založené na statický konfigurace na portálu Azure a Vystavitel tokeny příslušným způsobem.  Teď stejnou 2.0 OAuth povolte žádosti o vypadá:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

kde parametr **obor** označuje, které zdroje a oprávnění aplikace žádá o povolení. Požadované zdrojů nachází pořád velmi v žádosti o - je jednoduše zahrnuta v žádném z hodnoty parametru obor.  Použití parametru oboru tímto způsobem umožňuje verze 2.0 koncový bod, který vyhovovat více specifikaci OAuth 2.0 a zarovnán lépe běžné odvětví postupy.  Umožňuje také aplikace provádět [Přírůstková souhlas](#incremental-and-dynamic-consent), který je popsaný v další části.

## <a name="incremental-and-dynamic-consent"></a>Přírůstková a dynamické souhlas
Registraci aplikace v obecně dostupných Azure AD služby, třeba určit jejich požadovaná OAuth 2.0 oprávnění v portálu Azure času vytvoření aplikace:

![Registrace oprávnění uživatelského rozhraní](../media/active-directory-v2-flows/app_reg_permissions.PNG)

Oprávnění aplikace povinné byly nakonfigurované **staticky**.  Povolené konfigurace aplikace existovat v portálu Azure a hodní a jednoduše k dispozici kód, nabízí několik problémů pro vývojáře:

- Aplikace měli znát všechna oprávnění, která by někdy potřebovat při tvorbě aplikace.  Přidání oprávnění v čase byl náročné.
- Aplikace měli znát všechny zdroje, které byste měli dříve přístup předem.  Bylo obtížné vytvářet aplikace, které může zpřístupnit libovolný počet zdrojů.
- Aplikace dosáhl požádat o všechna oprávnění, která by někdy potřebovat při jeho první přihlášení.  V některých případech to vedly k velmi dlouhý seznam oprávnění, která se nedoporučuje koncových uživatelů z schválení v aplikaci access při počáteční přihlášení.

S koncový bod verze 2.0 můžete určit oprávnění aplikace potřebuje **dynamicky**za běhu během pravidelné použití aplikace.  Postup, můžete zadat obory aplikace, musí v libovolném bodě v čase zahrnutím je v `scope` parametr žádostí o povolení:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

Výše uvedené žádosti o oprávnění pro aplikaci zobrazíte data adresářové Azure AD uživatele, stejně jako zápis dat do jejich adresáře.  Pokud uživatel souhlasí s oprávnění v minulosti pro konkrétní aplikaci, jednoduše zadejte své přihlašovací údaje a přihlášení k aplikaci.  Pokud uživatel nemá souhlas některou z těchto oprávnění, koncový bod verze 2.0 požádejte uživatele, pro souhlas s tato oprávnění.  Další informace si můžete přečíst o [oprávněních, souhlas a obory](active-directory-v2-scopes.md).

Povolení aplikace žádosti o oprávnění dynamicky prostřednictvím `scope` parametr umožňuje úplnou kontrolu nad vašeho uživatelského prostředí.  Pokud chcete, můžete frontload souhlas zaznamenat a požádejte o všechna oprávnění v jedné žádost o počáteční ověření.  Nebo pokud aplikace vyžaduje velkého počtu oprávnění, můžete shromažďovat oprávnění uživatele postupně při pokusu o použití některé funkce aplikace určitou dobu.

## <a name="well-known-scopes"></a>Známé oborů

#### <a name="offline-access"></a>Offline přístup
koncový bod verze 2.0 může vyžadovat použití nové známý oprávnění k aplikacím - `offline_access` obor.  Všechny aplikace muset požádat o toto oprávnění, pokud budou muset přístupu k prostředkům jménem uživatele po dobu delší dobu, i když uživatel nemusí používat aktivně aplikace.  `offline_access` Obor se zobrazí uživateli v dialogových oknech souhlas jako "Získat přístup k datům v režimu offline", která bude třeba souhlasit s.  Požadování `offline_access` oprávnění vám umožní webovou aplikaci pro příjem OAuth 2.0 refresh_tokens z verze 2.0 koncového bodu.  Refresh_tokens jsou dlouhodobé a můžete vyměňovat pro nové OAuth 2.0 access_tokens po delší dobu přístupu.  

Pokud aplikace nepožaduje `offline_access` rozsah, nebudou refresh_tokens.  To znamená, že když uplatnění authorization_code v [toku kód se tak mohli ověřovat OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow)pouze získáte zpět access_token z `/token` koncového bodu.  Tento access_token platí krátký časový úsek (obvykle hodinu), ale nakonec vyprší.  V této okamžiku aplikace muset přesměrování uživatele zpět `/authorize` koncový bod k načtení nové authorization_code.  Během tohoto přesměrování uživatel může nebo nemusí muset zadávat přihlašovací údaje znovu nebo znovu souhlas oprávnění, v závislosti na domovské stránky týmového.

Další informace o OAuth 2.0, refresh_tokens a access_tokens, podívejte se na [odkaz protokol verze 2.0](active-directory-v2-protocols.md).

#### <a name="openid-profile--email"></a>OpenID, profilu a e-mailu

V původním služby Azure Active Directory umožní základní připojení OpenID přihlašovací tok velkému množství informací o uživateli ve výsledném id_token.  Nároky v id_token mohou obsahovat uživatele název, upřednostňované uživatelské jméno, e-mailovou adresu, ID objektu a další.

Jsme jsou teď omezení informace, které `openid` obor dává přístupu aplikace.  Obor "openid" bude pouze povolit aplikaci podepsat uživatele systému a přijímání identifikátor specifické pro aplikaci pro uživatele.  Pokud chcete získat identifikovatelné osobní údaje o uživateli v aplikaci, aplikace muset požádat o další oprávnění od uživatele.  Budeme představení dva nové obory – `email` a `profile` obory –, které vám umožní dělat tak.

`email` Obor je velmi jednoduché – umožňuje aplikace access uživatele primární e-mailovou adresu prostřednictvím `email` převzetí v id_token.  `profile` Oboru dává aplikace access pro všechny další základní informace o uživateli – jejich jména, upřednostňované uživatelské jméno, ID objektu a tak dále.

Umožňuje provádět kódu aplikace způsobem minimální zveřejnění – můžete jenom požádejte uživatele, sady informací, že aplikace vyžaduje, aby jeho práci.  Další informace o těchto oborů odkazují [odkaz obor verze 2.0](active-directory-v2-scopes.md). 

## <a name="token-claims"></a>Deklarace tokenů

Nároky v tokeny vydán koncový bod verze 2.0 nebude shodovat s tokeny vydán obecně dostupných koncové body Azure AD - aplikací migrací do nové služby neměli se předpokládá deklaraci konkrétní bude existovat v id_tokens nebo access_tokens.   Tokeny vydán koncový bod verze 2.0 jsou kompatibilní s specifikace OAuth 2.0 a OpenID připojení, ale může podle různých sémantiku než obvykle dostupná služba Azure AD.

Další informace o konkrétní deklarací vyzařovaného do tokeny verze 2.0 najdete v tématu [odkaz tokenu verze 2.0](active-directory-v2-tokens.md).

## <a name="limitations"></a>Omezení
Existuje několik omezení nějaká při použití čárky verze 2.0.  Získáte [verze 2.0 omezení dokumentu](active-directory-v2-limitations.md) zobrazíte kterékoli z těchto omezení použití u určitého nefunguje.
