<properties
    pageTitle="Azure Active Directory B2C | Microsoft Azure"
    description="Typy tokeny vydané v Azure Active Directory B2C."
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
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>


# <a name="azure-ad-b2c-token-reference"></a>Azure AD B2C: Odkaz Token

Azure Active Directory (Azure AD) B2C posílá několik typů tokenů zabezpečení během zpracování každý [tok ověřování](active-directory-b2c-apps.md). Tento dokument popisuje formát, vlastnosti zabezpečení a obsah každého typu token.

## <a name="types-of-tokens"></a>Typy tokenů

Azure AD B2C podporuje [protokol se tak mohli ověřovat OAuth 2.0](active-directory-b2c-reference-protocols.md), které využívá tokeny přístupu a tokeny aktualizace. Taky podporuje ověřování a přihlašovací prostřednictvím [OpenID připojení](active-directory-b2c-reference-protocols.md), které představuje třetí typ token: ID token. Každý z těchto tokenů představuje nosný token.

Nosný token je tokenu zabezpečení lightweight, který zajišťuje přístup "nosný" chráněné zdroji. Nosný je osobě, která Visio svádět k zahrnutí token. Azure AD Nejdřív musíte před dostávat nosný token ověřit strana. Ale pokud požadované kroky se dostali do zabezpečené token pro předávání a úložiště, můžete zachyceny a použity před nechtěným skupinou. Některé tokenů zabezpečení mají předdefinované mechanismus zabránit neoprávněnými osobami je používat, ale nosný tokeny nemusí tento postup. Musí být transportovány zabezpečené kanálu, například transport layer security (HTTPS).

Pokud nosný token jsou přenášena mimo zabezpečený kanál, škodlivý stran umožňuje útok člověka v – uprostřed získat tokenu a použijte ji k získání neoprávněnému přístupu k chráněného zdroj. Platí stejné zásady zabezpečení při nosný tokeny jsou uložené v mezipaměti pro pozdější použití. Vždy zajistěte, aby aplikace přenáší a ukládá nosný tokenů zabezpečení způsobem.

Další důležité informace o zabezpečení na nosný tokenů najdete v článku [RFC 6750 oddílu 5](http://tools.ietf.org/html/rfc6750).

V mnoha klíčovými Azure AD B2C problémy implementovaná jako JSON web tokeny (JWTs). JWT je kompaktní, XLL s podporou URL prostředkem přenosu informací mezi dvěma stranami. JWTs obsahují informace jmenoval deklarované. Jedná se o výrazy informace o nosný a předmět tokenu. Nároky v JWTs jsou JSON objekty, které jsou kódování a serializována pro přenos. Protože JWTs vydán Azure AD B2C jsou podepsán, ale ne zašifrovaných, můžete snadno zkontrolovat obsah JWT ladění ho. K dispozici několik nástrojů, který lze provést, včetně [calebb.net](http://calebb.net). Další informace o JWTs najdete v příručce [JWT specifikace](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

### <a name="id-tokens"></a>Tokeny ID

ID token je forma tokenu zabezpečení jsme narazili načítající aplikace z Azure AD B2C `authorize` a `token` koncové body. Tokeny ID jsou zobrazeny jako [JWTs](#types-of-tokens)a obsahují deklarace, které můžete použít k identifikaci uživatelů v aplikaci. Když jsou získány ID tokeny `authorize` koncový bod, jsou často používá pro přihlášení uživatele k webovým aplikacím. Když jsou získány tokeny ID `token` koncový bod, se můžou posílat v žádosti o HTTP během komunikace mezi dvěma komponent stejné aplikace nebo služby. Deklarace můžete použít v ID token podle potřeby. Se běžně používají k zobrazení informací o účtu nebo rozhodovat ovládacího prvku v aplikaci pro access.  

Tokeny ID přihlášeni, ale nejsou šifrované v současné době. Pokud aplikace nebo rozhraní API obdrží ID token, musí být [ověřit podpis](#token-validation) prokázat platná token. Aplikace nebo rozhraní API rovněž nutné ověřit, několik nároky v tokenu aby bylo prokázáno, že je platný. Podle toho, že splníte požadavky scénáře deklarací ověřit aplikace se může lišit, ale aplikace musíte udělat některé [běžné ověření nároku](#token-validation) v každé firem.

#### <a name="sample-id-token"></a>Ukázka ID tokenu
```
// Line breaks for display purposes only

eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6IklkVG9rZW5TaWduaW5nS2V5Q29udGFpbmVyIn0.
eyJleHAiOjE0NDIzNjAwMzQsIm5iZiI6MTQ0MjM1NjQzNCwidmVyIjoiMS4wIiwiaXNzIjoiaHR0cHM6Ly9s
b2dpbi5taWNyb3NvZnRvbmxpbmUuY29tLzc3NTUyN2ZmLTlhMzctNDMwNy04YjNkLWNjMzExZjU4ZDkyNS92
Mi4wLyIsImFjciI6ImIyY18xX3NpZ25faW5fc3RvY2siLCJzdWIiOiJOb3Qgc3VwcG9ydGVkIGN1cnJlbnRs
eS4gVXNlIG9pZCBjbGFpbS4iLCJhdWQiOiI5MGMwZmU2My1iY2YyLTQ0ZDUtOGZiNy1iOGJiYzBiMjlkYzYi
LCJpYXQiOjE0NDIzNTY0MzQsImF1dGhfdGltZSI6MTQ0MjM1NjQzNCwiaWRwIjoiZmFjZWJvb2suY29tIn0.
h-uiKcrT882pSKUtWCpj-_3b3vPs3bOWsESAhPMrL-iIIacKc6_uZrWxaWvIYkLra5czBcGKWrYwrAC8ZvQe
DJWZ50WXQrZYODEW1OUwzaD_I1f1HE0c2uvaWdGXBpDEVdsD3ExKaFlKGjFR2V7F-fPThkVDdKmkUDQX3bVc
yyj2V2nlCQ9jd7aGnokTPfLfpOjuIrTsAdPcGpe5hfSEuwYDmqOJjGs9Jp1f-eSNEiCDQOaTBSvr479L5ptP
XWeQZyX2SypN05Rjr05bjZh3j70ZUimiocfJzjibeoDCaQTz907yAg91WYuFOrQxb-5BaUoR7K-O7vxr2M-_
CQhoFA

```

### <a name="access-tokens"></a>Tokeny přístupu

Přístupový token je také formou tokenu zabezpečení jsme narazili načítající aplikace z Azure AD B2C `authorize` a `token` koncové body. Tokeny přístupu představují taky jako [JWTs](#types-of-tokens)a obsahují deklarace, které můžete použít k identifikaci uživatelů ve vaší webové služby a rozhraní API.

Tokeny přístupu přihlášeni, ale nejsou v současné době - zašifrované a velmi podobné id tokeny.  Tokeny přístupu bude použito k poskytnutí přístupu k webovým službám a rozhraní API a k identifikaci a ověření uživatele v těchto službách.  Však není poskytují libovolný výraz se tak mohli ověřovat v těchto služeb.  To znamená, která `scp` deklaraci identity v Accessu tokeny omezit nebo jinak představují poskytovaná předmět tokenu přístup.

Pokud váš rozhraní API obdrží přístupový token, musí být [ověřit podpis](#token-validation) prokázat platná tokenu. Vaše rozhraní API musí ověřit také několik nároky v tokenu aby bylo prokázáno, že je platný. Podle toho, že splníte požadavky scénáře deklarací ověřit aplikace se může lišit, ale aplikace musíte udělat některé [běžné ověření nároku](#token-validation) v každé firem.

### <a name="claims-in-id--access-tokens"></a>Nároky v ID a tokeny přístupu

Při použití Azure AD B2C máte jemně odstupňovaná publikum nemůže ovládat obsah svého tokeny. [Zásady](active-directory-b2c-reference-policies.md) odeslání určité sady uživatelská data v deklarace, které aplikace požaduje pro její operace je možné konfigurovat. Tyto požadavky mohou obsahovat standardní vlastnosti například uživatele `displayName` a `emailAddress`. Mohou také obsahovat [vlastní atributy](active-directory-b2c-reference-custom-attr.md) , které můžete definovat v adresáři B2C. Každý ID a přístup token, která se zobrazí bude obsahovat určitou skupinu související se zabezpečením deklarované. Aplikace můžete použít tyto požadavky zabezpečené ověřování uživatelů a požadavky.

Všimněte si, že nejsou v určitém pořadí vráceny nároky v ID tokeny. Kromě toho můžete nové nároky zavedená v ID tokeny kdykoli. Aplikaci byste neměli přerušit zavedení nové nároky. Tady jsou nároky, které plánujete ve ID a přístup tokeny vydán Azure AD B2C existují. Další stížnosti určí zásady. Praktická část zkuste podrobná nároky v ukázkové ID tokenu vložením do [calebb.net](http://calebb.net). Další podrobnosti najdete ve [specifikaci OpenID připojení](http://openid.net/specs/openid-connect-core-1_0.html).

| Jméno | Deklarace | Příklad hodnoty | Popis |
| ----------------------- | ------------------------------- | ------------ | --------------------------------- |
| Cílové skupiny | `aud` | `90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6` | Deklaraci cílové skupiny identifikuje určeného příjemce tokenu. Azure AD B2C cílové skupiny je ID aplikace vaše aplikace přiřazeny vaši aplikaci distribuovali portálu registrace aplikace. Aplikace by měl ověřte tuto hodnotu a odmítnout tokenu, pokud ho neodpovídá. |
| Vydavatel | `iss` | `https://login.microsoftonline.com/775527ff-9a37-4307-8b3d-cc311f58d925/v2.0/` | Toto tvrzení identifikuje Služba tokenů zabezpečení (STS), který vytváří a vrátí tokenu. Označuje také Azure AD adresář ověření uživatele. Aplikace by měl ověření nároku Vystavitel zajistit, že tokenu pochází z verze 2.0 koncového bodu. |
| Vydané | `iat` | `1438535543` | Toto tvrzení je údaj o čase niž byl vydán tokenu, který je v čase epocha. |
| Čas vypršení platnosti | `exp` | `1438539443` | Vypršení platnosti čase tvrzení je údaj o čase niž tokenu ztratí, který v epocha čas. Aplikace používejte toto tvrzení ověřte platnost tokenu životnost.  |
| Nejdříve | `nbf` | `1438535543` | Toto tvrzení je čas, pro niž tokenu stane platné, zastoupeného epocha času. Je to obvykle stejný jako okamžiku, kdy byl vydán tokenu. Aplikace používejte toto tvrzení ověřte platnost tokenu životnost.  |
| Verze | `ver` | `1.0` | Toto je verze ID token definované Azure AD. |
| Kód hash | `c_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Kód hash je součástí ID token pouze vystavení tokenu je společně s kódem se tak mohli ověřovat OAuth 2.0. Kód hash lze ověřit pravost kódem se tak mohli ověřovat. V tématu [připojení OpenID specifikace](http://openid.net/specs/openid-connect-core-1_0.html) podrobné informace o tom, jak provádět ověřování. |
| Access tokenu hash | `at_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Algoritmus hash tokenu přístup je součástí ID token pouze vystavení tokenu je společně s přístupový token OAuth 2.0. Algoritmus hash tokenu access lze ověřit pravost přístupový token. V tématu [připojení OpenID specifikace](http://openid.net/specs/openid-connect-core-1_0.html) podrobné informace o tom, jak provádět ověřování. |
| Nonce | `nonce` | `12345` | Náhodně vygenerovaný identifikátor je strategii slouží k zmírnění tokenu opakované útoky. Aplikaci můžete vyplnit nonce žádost se tak mohli ověřovat pomocí `nonce` dotazu parametr. Hodnota zadaná v žádosti o bude vypuštění jehož `nonce` převzetí ID token. Díky aplikaci můžete ověřit hodnotu s hodnotou zadaný v pozvánce na schůzku, která přidruží token ID dané relace v aplikaci. Aplikace by měl provést ověřování během procesu tokenu ověření ID. |
| Předmět | `sub` | `Not supported currently. Use oid claim.` | To je o tom, které token uplatňuje informace, například uživatelem aplikace. Tato hodnota je neměnný a nelze přiřazen nebo znovu použít. Slouží ke kontrole autorizace bezpečně, například kdy se používá tokenu pro přístup k prostředku. Deklarace předmět není zatím implementovaná v Azure AD B2C. Je třeba nakonfigurovat zásady zahrnout ID objektu `oid` převzetí a jeho hodnotu umožňuje určit uživatele, spíše než používat předmět náhradu se tak mohli ověřovat. |
| Ověřování kontextu tříd | `acr` | `b2c_1_sign_in` | Toto je název zásady, která byla použita k získání tokenu ID.  |
| Doba ověřování | `auth_time` | `1438535543` | Toto tvrzení je údaj o čase niž poslední zadané přihlašovací údaje uživatele, který je v čase epocha. |


### <a name="refresh-tokens"></a>Aktualizace tokenů

Aktualizace tokeny jsou tokenů zabezpečení, které aplikace umožňuje získat nové ID tokeny ani otevírat tokeny v toku OAuth 2.0. Poskytují aplikace dlouhodobé přístupu k prostředkům jménem uživatele bez nutnosti interakce s tyto uživatele.

Přijímat aktualizace tokenu v odpovědi na token aplikace musí žádost o `offline_acesss` obor. Další informace o `offline_access` rozsah, podívejte se na [odkaz protokol Azure AD B2C](active-directory-b2c-reference-protocols.md).

Aktualizace tokeny jsou a vždycky, úplně neprůhledné do aplikace. Vydává Azure AD a mohou být kontrolovány a interpretovat pouze Azure AD. Jsou dlouhodobé, ale aplikace se neměli měly zapisovat s tím token aktualizace bude trvat za určité období počet minut. Tokeny aktualizace může být neplatné kdykoliv různých důvodů. Jediný způsob, jakým aplikace vědět, pokud platí token aktualizace je pokus o uplatnění provedením žádost o tokenu Azure AD.

Po uplatnění token aktualizace nový token (a pokud přidělená aplikace `offline_access` obor), zobrazí nový token aktualizace tokenu odpověď. Měli byste uložit token nově vydané aktualizace. Měli byste nahradit token aktualizace, které jste dříve použili v žádosti o. Tato funkce pomáhá zaručit, že aktualizace tokeny platné po dobu.

## <a name="token-validation"></a>Tokenu ověření

Ověřit token, měli byste aplikace podpis a nároky tokenu.

Mnoho otevřít zdroj knihoven jsou k dispozici pro ověřování JWTs, v závislosti na preferovaný jazyk. Doporučujeme, aby můžete prozkoumat tyto možnosti namísto implementace logiky ověření. Informace v této příručce můžete informace o použití těchto knihoven správně.

### <a name="validate-the-signature"></a>Ověření podpisu
JWT obsahuje tři segmenty, oddělené `.` znak. První segment je **záhlaví**, druhý je **text**, a třetí **podpis**. Segmentu podpisu lze ověřit pravost tokenu tak, aby může být důvěryhodný aplikace.

Azure AD B2C tokeny přihlášeni pomocí standardní asymetrické šifrování algoritmy, například RSA 256. Záhlaví tokenu obsahuje informace o metodě klíč a šifrování slouží k podpisu tokenu:

```
{
        "typ": "JWT",
        "alg": "RS256",
        "kid": "GvnPApfWMdLRi8PDmisFn7bprKg"
}
```

`alg` Deklarace označuje algoritmu, který byl použit k podepsání tokenu. `kid` Deklarace označuje určité veřejným klíčem, který byl použit k podepsání tokenu.

Kdykoli může Azure AD pomocí jedné z množiny klíčových dvojice veřejných soukromých podepsat token. Azure AD otočí možné množiny klíče pravidelně, tak, aby aplikace by se měly zapisovat automaticky zpracovávání změnám klíče. Je vhodnější počet_plateb zkontrolovat aktualizace veřejné zkratek používaných v Azure AD každých 24 hodin.

Azure AD B2C má koncový bod metadat OpenID připojení. Díky aplikace načítat informace o Azure AD B2C za běhu. Tyto informace obsahuje koncové body, obsah tokenů a token podepisování klíče. Adresář B2C obsahuje dokument JSON metadat pro každého zásadu. Například dokument metadat pro `b2c_1_sign_in` zásad v `fabrikamb2c.onmicrosoft.com` je umístěn v:

```
https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in
```

`fabrikamb2c.onmicrosoft.com`adresář B2C slouží k ověření uživatele a `b2c_1_sign_in` jsou zásady použitý k získání tokenu. Chcete-li zjistit zásadu, které jste použili k podepsat token (a kam se obrátit vzdáleně metadata), máte dvě možnosti. Nejprve je součástí na její název `acr` převzetí v tokenu. Analyzovat deklarací mimo textu JWT o základu-64 dekódování textu a deserializace JSON řetězec, který výsledků. `acr` Deklarace bude název zásady, která byla použita o vystavení tokenu.  Další možností je kódovat zásad hodnoty `state` parametr při vydávání žádost a pak dekódovat a zjistit zásadu, které jste použili. Platí obě metody.

Dokument metadat je JSON objekt, který obsahuje několik užitečných použitelné informací. Jedná se o umístění koncové body muset ověřování OpenID připojení. Budou se týkají taky `jwks_uri`, který obsahuje místo množiny veřejných klíčů, která se používají k podpisu tokeny. Umístění není uvedený tady, že je nejlepší vzdáleně umístění dynamicky pomocí metadat dokumentu a analýze se `jwks_uri`:

```
https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in
```

JSON dokumentu uloženého na této adrese URL obsahuje všechny veřejné důležitých informací v používat v určitém okamžiku. Pomocí aplikace `kid` převzetí v záhlaví JWT vyberte veřejný klíč v JSON dokumentu, který se používá k podpisu konkrétní token. Pomocí správné veřejným klíčem a algoritmu určené ho pak můžete provádět ověření podpisu.

Popis toho, jak provádět ověření podpisu je mimo rozsah tohoto dokumentu. Mnoho knihoven otevřít zdroj jsou dostupné vám s tím pokud ho potřebujete.

### <a name="validate-the-claims"></a>Ověření deklarace
Když aplikace nebo rozhraní API obdrží ID token, má taky provést několik kontroly proti deklarace tokenu ID. Tyto zahrnout, ale nejsou omezené na:

- Deklarace **cílovou skupinu** : ověříte, že byl ID token určené věnovat aplikace.
- Nároky **nejdříve** a **dobu platnosti** : tyto ověřte, že tokenu ID nevypršela.
- Deklarace **Vystavitel** : ověříte, že token byl vydán do aplikace Azure AD.
- **Nonce**: Toto je strategie pro omezení rizik útok tokenu přehrání.

Úplný seznam ověření, které by měl provést aplikace získáte [specifikace OpenID připojení](https://openid.net). Podrobnosti o očekávané hodnoty pro tyto požadavky najdete v předchozí [tokenu oddíl](#types-of-tokens).  

## <a name="token-lifetimes"></a>Tokenu životnost

Následující tokenu životnost jsou k dispozici pro další své znalosti. Jejich můžete vyvíjet a ladění aplikace. Všimněte si, že vaše aplikace by neměly být došlo k zápisu očekávat některou z těchto životnost nezmění. Mohou a se změní.  Další informace o přizpůsobení tokenu životnost v Azure AD B2C [tady](active-directory-b2c-token-session-sso.md).

| Tokenu | Životnost | Popis |
| ----------------------- | ------------------------------- | ------------ |
| Tokeny ID | Hodina | Tokeny ID jsou obvykle platné po za hodinu. Webovou aplikaci pomocí tohoto životnost můžete udržovat vlastní relace s uživateli (doporučeno). Můžete taky zvolit jiné relaci životnost. Pokud potřebujete-li získat nové ID token aplikace, jednoduše musí tak, aby Azure AD novou žádost o přihlášení. Pokud má uživatel relace platné prohlížeče s Azure AD, nemusí být nutné uživatele zadejte přihlašovací údaje znovu. |
| Aktualizace tokenů | Až 14 dní | Token jednoho aktualizace platí pro maximálně 14 dní. Však token aktualizace může zneplatnění kdykoli pro libovolný počet důvodů. Aplikace nadále pokusíte použít token aktualizace, dokud se nezdaří žádost, nebo aplikace nahradí token aktualizace novou.  Aktualizace token taky můžete stanou neplatnými Pokud 90 dnů uplynulo od posledního zadání přihlašovacích údajů. |
| Povolení kódy | Pět minut | Povolení kódy jsou úmyslně krátkodobý. Budou by měl uplatnit okamžitě tokeny přístupu, ID tokeny nebo aktualizace tokeny při příjmu. |
