 <properties
   pageTitle="Odkaz Azure AD Token | Microsoft Azure"
   description="Příručka pro členy tý principy a hodnocení nároky v SAML 2.0 a JSON Web tokeny (JWT) tokeny vydané tak, že Azure Active Directory (AAD)"
   documentationCenter="na"
   authors="bryanla"
   services="active-directory"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/06/2016"
   ms.author="mbaldwin"/>

# <a name="azure-ad-token-reference"></a>Azure AD tokenu odkaz

Azure Active Directory (Azure AD) posílá několik typů tokenů zabezpečení ve zpracování každý tok ověřování. Tento dokument popisuje formát, vlastnosti zabezpečení a obsah každého typu token.

## <a name="types-of-tokens"></a>Typy tokenů

Azure AD podporuje [protokol se tak mohli ověřovat OAuth 2.0](active-directory-protocols-oauth-code.md), které využívá access_tokens a refresh_tokens.  Podporuje také ověřování a přihlašovací prostřednictvím [OpenID připojení](active-directory-protocols-openid-connect-code.md), které představuje třetí typ token, id_token.  Každý z těchto tokenů představuje "nosný token".

Nosný token je tokenu zabezpečení lightweight, který zajišťuje přístup "nosný" chráněné zdroji. V tomto smyslu "nosný" je osobě, která Visio svádět k zahrnutí tokenu. Když se při přijímání nosný token vyžaduje ověření s Azure AD, je třeba učinit zajistit token, kvůli kterým zachycení před nechtěným skupinou. Protože nosný tokeny nemají předdefinované mechanismus zabráníte neoprávněnými osobami je používat, musí být transportovány prostřednictvím zabezpečeného kanálu například transport layer security (HTTPS). Pokud nosný token jsou přenášena v vymazat, člověka v prostředním útok mohou sloužit k získání tokenu a získat neoprávněnému přístupu na chráněného zdroj. Platí stejné zásady zabezpečení při ukládání nebo ukládání do mezipaměti nosný tokeny pro pozdější použití. Vždy zajistěte, aby aplikace přenáší a ukládá nosný tokenů zabezpečení způsobem. Další otázky bezpečnosti pro na nosný tokenů najdete v článku [RFC 6750 oddílu 5](http://tools.ietf.org/html/rfc6750).

V mnoha tokenů vydán Azure AD implementovaná formátu JSON Web tokeny nebo JWTs.  JWT je kompaktní, XLL s podporou URL prostředky pro přenos informací mezi dvěma stranami.  Informace obsažené v JWTs se označují jako "deklarací" nebo výrazy informací o nosný a předmět tokenu.  Nároky v JWTs jsou objekty JSON kódování a serializován pro přenos.  Protože JWTs vydán Azure AD podepsán, ale ne zašifrovaných, můžete snadno zkontrolovat obsah JWT pro účely ladění.  Nejsou k dispozici pro to, například [jwt.calebb.net](http://jwt.calebb.net)několik nástrojů. Další informace o JWTs může označovat [JWT specifikace](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

## <a name="idtokens"></a>Id_tokens

Id_tokens jsou formuláře přihlašovací tokenu zabezpečení jsme narazili načítající aplikace při ověřování pomocí [OpenID spojení](active-directory-protocols-openid-connect-code.md).  Jsou zobrazeny jako [JWTs](#types-of-tokens)a obsahují deklarace, které používáte pro přihlášení uživatele k aplikaci.  Deklarace můžete použít v id_token jak najdete v článku přizpůsobení - běžně používají se k zobrazení informací o účtu nebo rozhodování ovládacího prvku v aplikaci pro access.

Id_tokens jsou přihlášení, ale ne zašifrovaných v současné době.  Přijaté aplikace id_token, musí být [ověřit podpis](#validating-tokens) prokázat tokenu pravost a ověřte několik nároky v tokenu abyste prokázali, že jeho platnosti.  Nároky ověřit aplikace lišit podle toho, scénář požadavky, ale existují některé [běžné ověření nároku](#validating-tokens) , který aplikace musíte udělat v každé firem.

Na id_tokens deklarací, jakož i ukázka id_token naleznete v části následující informace.  Všimněte si, že nároky v id_tokens nejsou vráceny v určitém pořadí.  Kromě toho nové deklarované může dojít k tomu id_tokens kdykoli včas: aplikace neměli konce zavedení nové deklarované.  Následující seznam obsahuje deklarace, které aplikace může problémy se spolehlivým interpretovat při psaní tohoto textu.  V případě potřeby i další podrobnosti najdete v [specifikace OpenID připojení](http://openid.net/specs/openid-connect-core-1_0.html).

#### <a name="sample-idtoken"></a>Ukázka id_token

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ.
```

> [AZURE.TIP] Praktická část zkuste podrobná nároky v ukázkové id_token vložením do [calebb.net](http://jwt.calebb.net).

#### <a name="claims-in-idtokens"></a>Nároky v id_tokens

| Deklarace JWT | Jméno | Popis |
|-----------|------|-------------|
| `appid`| ID aplikace | Určuje aplikace, která používá tokenu pro přístup k prostředku. Aplikaci můžete fungují jako samotné nebo jejím jménem uživatele. ID aplikace obvykle představuje objekt aplikace, ale může také představovat služby objekt v Azure AD. <br><br> **Příklad JWT hodnotu**: <br> `"appid":"15CB020F-3984-482A-864D-1D92265E8268"` |
| `aud`| Cílové skupiny | Určeného příjemce tokenu. Aplikace, která přijímá tokenu musí zkontrolujte, že hodnota cílové skupiny je správné odmítnout všechny tokeny určené pro různé cílové skupiny. <br><br> **Příklad SAML hodnotu**: <br> `<AudienceRestriction>`<br>`<Audience>`<br>`https://contoso.com`<br>`</Audience>`<br>`</AudienceRestriction>` <br><br> **Příklad JWT hodnotu**: <br> `"aud":"https://contoso.com"` |
| `appidacr`| Odkaz třídy kontextu ověřování aplikace | Určuje, jak ověření klienta. Veřejné klienta hodnotu 0. Použijete-li ID klienta a tajná klienta, je hodnota 1. <br><br> **Příklad JWT hodnotu**: <br> `"appidacr": "0"`|
| `acr`| Ověřování kontextu tříd | Označuje, jak ověření předmět, namísto klienta v deklaraci odkaz třídy kontextu ověřování aplikace. Hodnota "0" označuje, že ověřování koncových uživatelů nesplňuje požadavky ISO/IEC 29115. <br><br> **Příklad JWT hodnotu**: <br> `"acr": "0"`|
| | Ověřování rychlých zpráv | Záznamů datum a čas, kdy došlo k ověření. <br><br> **Příklad SAML hodnotu**: <br> `<AuthnStatement AuthnInstant="2011-12-29T05:35:22.000Z">` |
| `amr`| Metody ověřování | Určuje, jak předmět tokenu ověření. <br><br> **Příklad SAML hodnotu**: <br> `<AuthnContextClassRef>`<br>`http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod/password`<br>`</AuthnContextClassRef>` <br><br> **Příklad JWT hodnotu**:`“amr”: ["pwd"]` |
| `given_name`| Křestní jméno | Poskytuje první nebo "" jméno uživatele, jak je to nastavené v objektu uživatele Azure AD. <br><br> **Příklad SAML hodnotu**: <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname”>`<br>`<AttributeValue>Frank<AttributeValue>` <br><br> **Příklad JWT hodnotu**: <br> `"given_name": "Frank"` |
| `groups`| Skupiny | Poskytuje ID objektů, které představují členství ve skupinách předmětu. Tyto hodnoty jsou jedinečné (viz téma ID objektu) a bezpečné slouží ke správě přístup, jako je třeba vynucení autorizaci pro přístup k prostředku. Skupiny součástí skupiny deklarace nastavené na základě jednotlivé aplikace prostřednictvím vlastnosti "groupMembershipClaims" manifestu aplikace. Hodnota null budou vyloučit všechny skupiny, hodnotu "SecurityGroup" bude obsahovat pouze členství ve skupině zabezpečení Active Directory a hodnotu "Všechny" bude obsahovat skupin zabezpečení a Office 365 distribuční seznamy. <br><br> **Příklad SAML hodnotu**: <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">`<br>`<AttributeValue>07dd8a60-bf6d-4e17-8844-230b77145381</AttributeValue>` <br><br> **Příklad JWT hodnotu**: <br> `“groups”: ["0e129f5b-6b0a-4944-982d-f776045632af", … ]` |
| `idp` | Zprostředkovatele identit | Zprostředkovatele identit, které ověřeny předmět tokenu záznamů. Tato hodnota je shodné s hodnotou deklaraci Vystavitel není uživatelský účet v různých klienta než Vystavitel. <br><br> **Příklad SAML hodnotu**: <br> `<Attribute Name=” http://schemas.microsoft.com/identity/claims/identityprovider”>`<br>`<AttributeValue>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/<AttributeValue>` <br><br> **Příklad JWT hodnotu**: <br> `"idp":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `iat` | IssuedAt | Ukládá čas, kdy byl vydán tokenu. Často používá k změřit tokenu aktuálnost. <br><br> **Příklad SAML hodnotu**: <br> `<Assertion ID="_d5ec7a9b-8d8f-4b44-8c94-9812612142be" IssueInstant="2014-01-06T20:20:23.085Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">` <br><br> **Příklad JWT hodnotu**: <br> `"iat": 1390234181` |
| `iss` | Vydavatel | Určuje Služba tokenů zabezpečení (STS), který vytváří a vrátí tokenu. Tokeny Azure AD vrátí Vystavitel je sts.windows.net. Identifikátor GUID v hodnota deklarace vydavatel je ID klienta Azure AD adresáře. ID klienta je identifikátor neměnný a spolehlivé adresáře. <br><br> **Příklad SAML hodnotu**: <br> `<Issuer>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/</Issuer>` <br><br> **Příklad JWT hodnotu**: <br>  `"iss":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `family_name` | Příjmení | Jak je definováno v objektu uživatele Azure AD obsahuje příjmení jméno, příjmení nebo příjmení uživatele. <br><br> **Příklad SAML hodnotu**: <br> `<Attribute Name=” http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname”>`<br>`<AttributeValue>Miller<AttributeValue>` <br><br> **Příklad JWT hodnotu**: <br> `"family_name": "Miller"` |
| `unique_name`| Jméno | Poskytuje lidské čitelné hodnotu, která určuje předmět tokenu. Tato hodnota není musí být jedinečný v rámci klienta a je určen pouze pro účely zobrazení. <br><br> **Příklad SAML hodnotu**: <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name”>`<br>`<AttributeValue>frankm@contoso.com<AttributeValue>` <br><br> **Příklad JWT hodnotu**: <br> `"unique_name": "frankm@contoso.com"` |
| `oid` | ID objektu | Obsahuje jedinečný identifikátor objektu v Azure AD. Tato hodnota je neměnný a nelze přiřazen nebo znovu použít. Identifikaci objektu v dotazech Azure AD pomocí ID objektu. <br><br> **Příklad SAML hodnotu**: <br> `<Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">`<br>`<AttributeValue>528b2ac2-aa9c-45e1-88d4-959b53bc7dd0<AttributeValue>` <br><br> **Příklad JWT hodnotu**: <br> `"oid":"528b2ac2-aa9c-45e1-88d4-959b53bc7dd0"` |
| `roles` | Role | Představuje všechny role aplikace, které byly udělil přímo i nepřímo pomocí členství ve skupinách a mohou sloužit k jejímu vynucení řízení přístupu na základě rolí. Role aplikací se definují na základě jednotlivé aplikace, až `appRoles` vlastnosti manifestu aplikace. `value` Vlastnost každou roli aplikace je hodnota, která se zobrazí v role deklarace. <br><br> **Příklad SAML hodnotu**: <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/role">`<br>`<AttributeValue>Admin</AttributeValue>` <br><br> **Příklad JWT hodnotu**: <br> `“roles”: ["Admin", … ]` |
| `scp` | Rozsah | Označuje zosobnění oprávněním ke klientské aplikaci. Výchozí oprávnění je `user_impersonation`. Vlastník zabezpečené zdroje v Azure AD evidovat další hodnoty. <br><br> **Příklad JWT hodnotu**: <br> `"scp": "user_impersonation"`|
| `sub` |Předmět| Určuje hlavního uživatele o tom, které tokenu uplatňuje informace, například uživatelem aplikace. Tato hodnota je neměnný a nelze znovu přiřadit nebo znovu použít, tak jej lze použít ke kontrole autorizace bezpečné. Vzhledem k tomu předmět vždy účastní Azure AD problémech tokeny, doporučujeme používat tuto hodnotu v systému se tak mohli ověřovat univerzální. <br> `SubjectConfirmation`není deklaraci. Popisuje, jak ověřit předmět tokenu. `Bearer`Označuje, že je potvrzena předmětu vlastní tokenu. <br><br> **Příklad SAML hodnotu**: <br> `<Subject>`<br>`<NameID>S40rgb3XjhFTv6EQTETkEzcgVmToHKRkZUIsJlmLdVc</NameID>`<br>`<SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />`<br>`</Subject>` <br><br> **Příklad JWT hodnotu**: <br> `"sub":"92d0312b-26b9-4887-a338-7b00fb3c5eab"`|
| `tid` | ID klienta | Neměnný, jednorázových identifikátor, který identifikuje klienta adresář, který vystavil tokenu. Tato hodnota umožňuje přístup k adresář klienta konkrétního zdroje v aplikaci více klienta. Například můžete tuto hodnotu k identifikaci klienta telefonuje k rozhraní API grafu. <br><br> **Příklad SAML hodnotu**: <br> `<Attribute Name=”http://schemas.microsoft.com/identity/claims/tenantid”>`<br>`<AttributeValue>cbb1a5ac-f33b-45fa-9bf5-f37db0fed422<AttributeValue>` <br><br> **Příklad JWT hodnotu**: <br> `"tid":"cbb1a5ac-f33b-45fa-9bf5-f37db0fed422"`|
| `nbf`, `exp`|Životnost tokenu | Definuje časový interval, ve kterém je token platný. Služba, která ověří tokenu by měl zkontrolujte, že aktuální datum v rámci Životnost tokenů, ještě měli odmítnout tokenu. Služba by mohly umožnit posílání až pět minut mimo rozsah Životnost tokenů kontrolujte rozdíly v čas ("čas zkosení") mezi Azure AD a službách. <br><br> **Příklad SAML hodnotu**: <br> `<Conditions`<br>`NotBefore="2013-03-18T21:32:51.261Z"`<br>`NotOnOrAfter="2013-03-18T22:32:51.261Z"`<br>`>` <br><br> **Příklad JWT hodnotu**: <br> `"nbf":1363289634, "exp":1363293234` |
| `upn`| Hlavní jméno uživatele | Ukládá uživatelské jméno uživatele jistiny.<br><br> **Příklad JWT hodnotu**: <br> `"upn": frankm@contoso.com`|
| `ver`| Verze | Ukládá číslo verze tokenu. <br><br> **Příklad JWT hodnotu**: <br> `"ver": "1.0"`|

## <a name="access-tokens"></a>Tokeny přístupu

Tokeny přístupu jsou pouze spotřební tak, že Microsoft Services v daném okamžiku.  Vaše aplikace nemusí provádět ověření nebo kontroly přístup tokenů z některého z aktuálně podporované scénáře.  Tokeny přístupu můžete považovat za plně neprůhledné - jsou pouze řetězce, které aplikace Microsoft předat v žádosti o HTTP.

Při žádosti přístupový token Azure AD také vrátí některé metadata přístupový token spotřebu vaše aplikace.  Tyto informace obsahuje i čas ukončení přístupový token a oborů, pro které platí.  Díky aplikaci provádět inteligentní ukládání do mezipaměti přístup tokenů aniž byste museli analyzovat otevřít přístupový token samotné.

## <a name="refresh-tokens"></a>Aktualizace tokenů

Aktualizace tokeny jsou tokenů zabezpečení, které aplikace můžete získat nový tokeny přístupu v toku OAuth 2.0.  Je možné aplikaci pro dosažení dlouhodobé přístupu k prostředkům jménem uživatele bez nutnosti interakce uživatelem.

Tokeny aktualizace jsou více zdrojů, což znamená, že může být přijaté v průběhu tokenu žádost o jeden zdroj, ale uplatnili přístup tokeny úplně jinému zdroji. Chcete-li více zdrojů, nastavte `resource` parametrů v žádosti o cílené zdroji.

Tokeny aktualizace jsou úplně neprůhledné do aplikace. Jsou dlouhodobé, ale aplikace by neměly být došlo k zápisu očekávat, že bude trvat token aktualizovat všechny dobu.  Tokeny aktualizace může být neplatné kdykoli různých důvodů.  Jediný způsob, jakým aplikace vědět, pokud platí token aktualizace je pokus o uplatnění tím, že žádost o tokenu Azure AD tokenu koncový bod.

Po uplatnění token aktualizace pro nové přístupový token se zobrazí nový token aktualizace tokenu odpověď.  Uložte tokenu nově vydané aktualizace nahrazení tu, kterou jste použili v pozvánce.  To bude zaručit, že aktualizace tokeny platné po dobu.

## <a name="validating-tokens"></a>Ověřování tokenů

V tomto okamžiku pouze tokenu ověřování, které klientské aplikace by mělo být potřeba provést ověřování id_tokens.  Chcete ověřit id_token, měli aplikace ověřit podpis id_token a nároky v id_token.

Připravili jsme pro knihovny a ukázek kódu, které ukazují, jak snadno zpracovat tokenu ověření, pokud chcete porozumět procesu podkladového.  Existují také několik knihoven třetích stran otevřít zdroj k dispozici pro ověření JWT, aspoň jednou z možností pro každý platformy a jazyk. Další informace o knihovnách Azure AD ověřování a ukázek kódu najdete v článku [Azure AD ověřování knihovny](active-directory-authentication-libraries.md).

#### <a name="validating-the-signature"></a>Ověření podpisu

JWT obsahuje tři segmenty, které jsou odděleny `.` znak.  První segment se označuje jako **záhlaví**druhý **textu**a třetí jako **podpis**.  Segment podpisu lze ověřit pravost id_token tak, aby může být důvěryhodný aplikace.

Id_Tokens jsou přihlášenými pomocí standardní asymetrické šifrování algoritmů odvětví, například RSA 256. Záhlaví id_token obsahuje informace o metodě klíč a šifrování slouží k podpisu tokenu:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "x5t": "kriMPdmBvx68skT8-mPAB3BseeA"
}
```

`alg` Deklarace označuje algoritmu, který byl použit k podepsání tokenu, při `x5t` deklarace označuje určité veřejným klíčem, který byl použit k podepsání tokenu.

V libovolném bodě v čase Azure AD podepsat id_token pomocí jedné z množiny klíčových dvojice veřejných soukromých. Azure AD otočí možné množiny klíčů v pravidelných intervalech, aby aplikace by se měly zapisovat zpracovávání tyto klíčové změny automaticky.  Frekvenci rozumné zkontrolovat aktualizace veřejné zkratek používaných v Azure AD je každých 24 hodin.

Můžete získat podpisový klíčové údaje nutné ověřit podpis pomocí c:\ dokument metadat OpenID připojení:

```
https://login.microsoftonline.com/common/.well-known/openid-configuration
```

> [AZURE.TIP] Vyzkoušejte tuto adresu URL v prohlížeči.

Tento dokument metadat je objekt JSON obsahující několik užitečných použitelné informací, jako je místo různých koncové body potřebných pro ověřování OpenID připojení.  

Obsahuje taky `jwks_uri`, který dává umístění sady veřejné klíče slouží k podpisu tokeny.  Dokument JSON c:\ `jwks_uri` obsahuje všechny informace o veřejných klíči používaných v této konkrétní časovém okamžiku.  Pomocí aplikace `kid` převzetí v záhlaví JWT vyberte, které veřejným klíčem v tomto dokumentu byla použita k podepsat konkrétní token.  Pak můžete provádět potvrzení podpisu správné veřejným klíčem a určené algoritmus.

Ověřování podpisu je mimo rozsah tento dokument – nejsou k dispozici pro pomůže vám to udělat v případě potřeby mnoha otevřít zdroj knihovny.

#### <a name="validating-the-claims"></a>Ověření deklarace

Pokud aplikace obdrží id_token při přihlašování uživatelů, má taky provést několik kontroly proti deklarace v id_token.  Zahrnout, ale nejsou omezené na:

  - Deklarace **cílovou skupinu** – můžete ověřit, že id_token byl má být zadán do aplikace.
  - **Nejdříve** a **Dobu platnosti** deklarace – můžete ověřit, že id_token nevypršela.
  - Deklarace **Vystavitel** – můžete ověřit, že token byl skutečně vystaven do aplikace Azure AD.
  - **Nonce** - zmírnit tokenu zopakování útoku.
  - a mnohem víc...

Úplný seznam ověření nároku, které by měl provést aplikace v příručce [specifikace OpenID připojení](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation).

Podrobnosti o očekávané hodnoty pro tyto požadavky najdete v předchozí části [oddílu id_token](#id-tokens) .

## <a name="sample-tokens"></a>Ukázka tokenů

Tato část zobrazuje příklady SAML a JWT klíčovými slovy, která vrací Azure AD. Tyto příklady umožňují najdete v článku nároky v souvislosti.
SAML Token

Jedná se o ukázku typické token SAML.

    <?xml version="1.0" encoding="UTF-8"?>
    <t:RequestSecurityTokenResponse xmlns:t="http://schemas.xmlsoap.org/ws/2005/02/trust">
      <t:Lifetime>
        <wsu:Created xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T05:15:47.060Z</wsu:Created>
        <wsu:Expires xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T06:15:47.060Z</wsu:Expires>
      </t:Lifetime>
      <wsp:AppliesTo xmlns:wsp="http://schemas.xmlsoap.org/ws/2004/09/policy">
        <EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
          <Address>https://contoso.onmicrosoft.com/MyWebApp</Address>
        </EndpointReference>
      </wsp:AppliesTo>
      <t:RequestedSecurityToken>
        <Assertion xmlns="urn:oasis:names:tc:SAML:2.0:assertion" ID="_3ef08993-846b-41de-99df-b7f3ff77671b" IssueInstant="2014-12-24T05:20:47.060Z" Version="2.0">
          <Issuer>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</Issuer>
          <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
            <ds:SignedInfo>
              <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
              <ds:SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
              <ds:Reference URI="#_3ef08993-846b-41de-99df-b7f3ff77671b">
                <ds:Transforms>
                  <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                  <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
                </ds:Transforms>
                <ds:DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256" />
                <ds:DigestValue>cV1J580U1pD24hEyGuAxrbtgROVyghCqI32UkER/nDY=</ds:DigestValue>
              </ds:Reference>
            </ds:SignedInfo>
            <ds:SignatureValue>j+zPf6mti8Rq4Kyw2NU2nnu0pbJU1z5bR/zDaKaO7FCTdmjUzAvIVfF8pspVR6CbzcYM3HOAmLhuWmBkAAk6qQUBmKsw+XlmF/pB/ivJFdgZSLrtlBs1P/WBV3t04x6fRW4FcIDzh8KhctzJZfS5wGCfYw95er7WJxJi0nU41d7j5HRDidBoXgP755jQu2ZER7wOYZr6ff+ha+/Aj3UMw+8ZtC+WCJC3yyENHDAnp2RfgdElJal68enn668fk8pBDjKDGzbNBO6qBgFPaBT65YvE/tkEmrUxdWkmUKv3y7JWzUYNMD9oUlut93UTyTAIGOs5fvP9ZfK2vNeMVJW7Xg==</ds:SignatureValue>
            <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
              <X509Data>
                <X509Certificate>MIIDPjCCAabcAwIBAgIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTQwMTAxMDcwMDAwWhcNMTYwMTAxMDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAkSCWg6q9iYxvJE2NIhSyOiKvqoWCO2GFipgH0sTSAs5FalHQosk9ZNTztX0ywS/AHsBeQPqYygfYVJL6/EgzVuwRk5txr9e3n1uml94fLyq/AXbwo9yAduf4dCHTP8CWR1dnDR+Qnz/4PYlWVEuuHHONOw/blbfdMjhY+C/BYM2E3pRxbohBb3x//CfueV7ddz2LYiH3wjz0QS/7kjPiNCsXcNyKQEOTkbHFi3mu0u13SQwNddhcynd/GTgWN8A+6SN1r4hzpjFKFLbZnBt77ACSiYx+IHK4Mp+NaVEi5wQtSsjQtI++XsokxRDqYLwus1I1SihgbV/STTg5enufuwIDAQABo2IwYDBeBgNVHQEEVzBVgBDLebM6bK3BjWGqIBrBNFeNoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAA4IBAQCJ4JApryF77EKC4zF5bUaBLQHQ1PNtA1uMDbdNVGKCmSp8M65b8h0NwlIjGGGy/unK8P6jWFdm5IlZ0YPTOgzcRZguXDPj7ajyvlVEQ2K2ICvTYiRQqrOhEhZMSSZsTKXFVwNfW6ADDkN3bvVOVbtpty+nBY5UqnI7xbcoHLZ4wYD251uj5+lo13YLnsVrmQ16NCBYq2nQFNPuNJw6t3XUbwBHXpF46aLT1/eGf/7Xx6iy8yPJX4DyrpFTutDz882RWofGEO5t4Cw+zZg70dJ/hH/ODYRMorfXEW+8uKmXMKmX2wyxMKvfiPbTy5LmAU8Jvjs2tLg4rOBcXWLAIarZ</X509Certificate>
              </X509Data>
            </KeyInfo>
          </ds:Signature>
          <Subject>
            <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent">m_H3naDei2LNxUmEcWd0BZlNi_jVET1pMLR6iQSuYmo</NameID>
            <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />
          </Subject>
          <Conditions NotBefore="2014-12-24T05:15:47.060Z" NotOnOrAfter="2014-12-24T06:15:47.060Z">
            <AudienceRestriction>
              <Audience>https://contoso.onmicrosoft.com/MyWebApp</Audience>
            </AudienceRestriction>
          </Conditions>
          <AttributeStatement>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
              <AttributeValue>a1addde8-e4f9-4571-ad93-3059e3750d23</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/tenantid">
              <AttributeValue>b9411234-09af-49c2-b0c3-653adc1f376e</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
              <AttributeValue>sample.admin@contoso.onmicrosoft.com</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname">
              <AttributeValue>Admin</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname">
              <AttributeValue>Sample</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">
              <AttributeValue>5581e43f-6096-41d4-8ffa-04e560bab39d</AttributeValue>
              <AttributeValue>07dd8a89-bf6d-4e81-8844-230b77145381</AttributeValue>
              <AttributeValue>0e129f4g-6b0a-4944-982d-f776000632af</AttributeValue>
              <AttributeValue>3ee07328-52ef-4739-a89b-109708c22fb5</AttributeValue>
              <AttributeValue>329k14b3-1851-4b94-947f-9a4dacb595f4</AttributeValue>
              <AttributeValue>6e32c650-9b0a-4491-b429-6c60d2ca9a42</AttributeValue>
              <AttributeValue>f3a169a7-9a58-4e8f-9d47-b70029v07424</AttributeValue>
              <AttributeValue>8e2c86b2-b1ad-476d-9574-544d155aa6ff</AttributeValue>
              <AttributeValue>1bf80264-ff24-4866-b22c-6212e5b9a847</AttributeValue>
              <AttributeValue>4075f9c3-072d-4c32-b542-03e6bc678f3e</AttributeValue>
              <AttributeValue>76f80527-f2cd-46f4-8c52-8jvd8bc749b1</AttributeValue>
              <AttributeValue>0ba31460-44d0-42b5-b90c-47b3fcc48e35</AttributeValue>
              <AttributeValue>edd41703-8652-4948-94a7-2d917bba7667</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/identityprovider">
              <AttributeValue>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</AttributeValue>
            </Attribute>
          </AttributeStatement>
          <AuthnStatement AuthnInstant="2014-12-23T18:51:11.000Z">
            <AuthnContext>
              <AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
            </AuthnContext>
          </AuthnStatement>
        </Assertion>
      </t:RequestedSecurityToken>
      <t:RequestedAttachedReference>
        <SecurityTokenReference xmlns="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedAttachedReference>
      <t:RequestedUnattachedReference>
        <SecurityTokenReference xmlns="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedUnattachedReference>
      <t:TokenType>http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0</t:TokenType>
      <t:RequestType>http://schemas.xmlsoap.org/ws/2005/02/trust/Issue</t:RequestType>
      <t:KeyType>http://schemas.xmlsoap.org/ws/2005/05/identity/NoProofKey</t:KeyType>
    </t:RequestSecurityTokenResponse>

### <a name="jwt-token---user-impersonation"></a>JWT Token - zosobnění uživatele

Toto je ukázka typické JSON web tokenu (JWT) použitý v toku se tak mohli ověřovat kód grant (udělit).
Kromě deklarací tokenu zahrnuje číslo verze **ver** a **appidacr**, ověřování kontextu tříd, což znamená, jak ověření klienta. Veřejné klienta hodnotu 0. Pokud jste použili kód klienta nebo tajná klienta, je hodnota 1.

    {
     typ: "JWT",
     alg: "RS256",
     x5t: "kriMPdmBvx68skT8-mPAB3BseeA"
    }.
    {
     aud: "https://contoso.onmicrosoft.com/scratchservice",
     iss: "https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/",
     iat: 1416968588,
     nbf: 1416968588,
     exp: 1416972488,
     ver: "1.0",
     tid: "b9411234-09af-49c2-b0c3-653adc1f376e",
     amr: [
      "pwd"
     ],
     roles: [
      "Admin"
     ],
     oid: "6526e123-0ff9-4fec-ae64-a8d5a77cf287",
     upn: "sample.user@contoso.onmicrosoft.com",
     unique_name: "sample.user@contoso.onmicrosoft.com",
     sub: "yf8C5e_VRkR1egGxJSDt5_olDFay6L5ilBA81hZhQEI",
     family_name: "User",
     given_name: "Sample",
     groups: [
      "0e129f6b-6b0a-4944-982d-f776000632af",
      "323b13b3-1851-4b94-947f-9a4dacb595f4",
      "6e32c250-9b0a-4491-b429-6c60d2ca9a42",
      "f3a161a7-9a58-4e8f-9d47-b70022a07424",
      "8d4c81b2-b1ad-476d-9574-544d155aa6ff",
      "1bf80164-ff24-4866-b19c-6212e5b9a847",
      "76f80127-f2cd-46f4-8c52-8edd8bc749b1",
      "0ba27160-44d0-42b5-b90c-47b3fcc48e35"
     ],
     appid: "b075ddef-0efa-123b-997b-de1337c29185",
     appidacr: "1",
     scp: "user_impersonation",
     acr: "1"
    }.

## <a name="related-content"></a>Související obsah
- Další informace o správě zásad Životnost tokenů prostřednictvím rozhraní API Azure AD grafu naleznete v Azure AD grafu [operace zásady](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) a [zásady entity](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#policy-entity).
- Další informace a příklady ke správě zásad pomocí rutin prostředí PowerShell, včetně výběry najdete v článku [Konfigurovatelné tokenu životnost v Azure AD](active-directory-configurable-token-lifetimes.md). 
