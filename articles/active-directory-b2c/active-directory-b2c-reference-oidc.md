<properties
    pageTitle="Azure Active Directory B2C | Microsoft Azure"
    description="Vytváření webových aplikací pomocí služby Azure Active Directory provádění ověřování protokolem OpenID připojení."
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

# <a name="azure-active-directory-b2c-web-sign-in-with-openid-connect"></a>Azure Active Directory B2C: Web přihlášení k aplikaci OpenID připojení

Připojení OpenID je protokol ověřování postavená OAuth 2.0, mohou sloužit k bezpečnému přihlásit uživatelé do webových aplikací.  Pomocí služby Azure Active Directory (Azure AD) B2C provádění OpenID připojení můžete externí registrace, přihlásit a další služby Správa identit dojde k ve webových aplikacích Azure AD. Tento průvodce vám ukáže, jak k tomu nevyzve způsobem nezávislým jazyk. To bude popisují, jak k odesílání a přijímání zpráv HTTP bez použití libovolného příkazu pro naše knihoven otevřít zdroj.

[Připojení OpenID](http://openid.net/specs/openid-connect-core-1_0.html) rozšiřuje protokol *se tak mohli ověřovat* OAuth 2.0 pro použití *ověřovací* protokol. Umožňuje provádět jednotné přihlašování pomocí OAuth. Zavádí koncept `id_token`, což je tokenu zabezpečení, který umožňuje klientovi ověření identity uživatele a získat základní informace o profilu o uživateli.

Protože rozšiřuje OAuth 2.0, zároveň povolíte aplikace bezpečně získat **access_tokens**. Můžete použít access_tokens přístupu k prostředkům, které jsou správně zapojené [serveru se tak mohli ověřovat](active-directory-b2c-reference-protocols.md#the-basics). Pokud vytváříte webovou aplikaci, která je hostitelem server a k nim získat přístup prostřednictvím prohlížeče doporučujeme OpenID připojení. Pokud chcete přidat Správa identit na mobilu nebo desktopové aplikace pomocí Azure AD B2C, používejte [OAuth 2.0](active-directory-b2c-reference-oauth-code.md) nikoli OpenID připojení.

Azure AD B2C rozšiřuje standardní protokol OpenID připojit a udělat víc než jednoduchý a tak mohli ověřovat. Uvádí [**zásad parametr**](active-directory-b2c-reference-policies.md), který umožňuje OpenID připojit pomocí přidejte uživatelské prostředí aplikace – například registrace, přihlásit a Správa profilů. Tady Ukážeme vám jak používat OpenID připojení a zásady implementovat každé z těchto prostředí ve webových aplikacích. Také Ukážeme vám jak získat access_tokens pro přístup k webu rozhraní API.

Požadavky HTTP příkladu níže použije našeho adresáře B2C vzorku, **fabrikamb2c.onmicrosoft.com**i naše ukázkové aplikace **https://aadb2cplayground.azurewebsites.net** a zásady. Které můžete vyzkoušet žádosti o sobě s využitím těchto hodnot zadarmo nebo je můžete nahradit svým vlastním.
Zjistěte, jak [získat vlastní B2C klienta, aplikace a zásady](#use-your-own-b2c-directory).

## <a name="send-authentication-requests"></a>Odeslání žádosti o ověřování
Potřebujete-li ověření uživatele a spouštět zásadu webovou aplikaci, můžete odkázat uživateli `/authorize` koncového bodu. Toto je interaktivní část tok, pokud uživatel skutečně provede akci, v závislosti zásadách.

V této žádosti klienta označuje oprávnění, která potřebuje získat od uživatele v `scope` parametry a zásady provést v `p` parametr. Tři příklady jsou uvedeny níže (plus pár konce řádků pro snazší čitelnost), každý pomocí různých zásad. Chcete-li zjistěte, jak funguje každý požadavek, zkuste vložením žádost do prohlížeče a spuštěním.

#### <a name="use-a-sign-in-policy"></a>Použití zásad přihlášení

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_in
```

#### <a name="use-a-sign-up-policy"></a>Pomocí zápisu zásad

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_up
```

#### <a name="use-an-edit-profile-policy"></a>Použití zásad upravit profil

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_edit_profile
```

| Parametr | Povinné? | Popis |
| ----------------------- | ------------------------------- | ----------------------- |
| client_id | Povinné | ID aplikace přiřazené [Azure portál](https://portal.azure.com/) aplikace. |
| response_type | Povinné | Typ odpovědi musí obsahovat `id_token` pro OpenID připojení. Pokud svoji webovou aplikaci potřebovat tokeny pro volání webového rozhraní API, můžete použít `code+id_token`, jak můžeme udělat i v tomto poli. |
| redirect_uri | Doporučené | Redirect_uri aplikace, kde můžete ověřování odpovědi odešlete nebo přijmete tak, že aplikace. Se musí přesně shodovat jednu redirect_uris, kterou jste registrovali na portálu s tím rozdílem, že musí být překódován jako adresa URL. |
| rozsah | Povinné | Seznam oborů oddělený mezerami. Jeden obor hodnota označuje Azure AD obou oprávnění, které jsou požadovány. `openid` Rozsah Určuje oprávnění k přihlášení uživatele a získání dat o uživateli ve formě **id_tokens** (Další věci se chystají k tomu dále v tomto článku). `offline_access` Obor vynechán webových aplikací web apps. Znamená to, že aplikace, musí **refresh_token** dlouhodobé přístupu pro zdroje. |
| response_mode | Doporučené | Metoda použitý k odeslání výsledné authorization_code aplikace. Může být jeden z "dotazu", "form_post" nebo "neúplné".  "form_post" se doporučuje nejlepší cenného papíru. |
| Stav | Doporučené | Hodnota součástí požadavek, který bude vrácena také v tokenu odpovědi. Může být řetězec veškerý obsah, který chcete. Náhodně generovaným jedinečnou hodnotu se obvykle používá pro zabránění útoky padělání žádost webů. Stav je také použít ke kódování informace o stavu uživatele v aplikaci před požadavek na ověření došlo k chybě, například stránky, které byly na. |
| Nonce | Povinné | Hodnota součástí žádosti o (generované aplikaci), budou zahrnuty do výsledné id_token jako deklaraci. Aplikaci můžete ověřit pak tuto hodnotu na zmírnění tokenu opakované útoky. Hodnota je obvykle náhodného jedinečné řetězcem, který slouží k identifikaci origin žádost. |
| p | Povinné | Zásady, který bude spuštěn. Název zásady, která se vytvoří ve vašem klientovi B2C je. Hodnotu zásad název musí začínat "b2c\_1\_". Další informace o zásady v [rámci Extensible zásad](active-directory-b2c-reference-policies.md). |
| dotaz | Volitelné | Typ interakce s uživatelem, který je potřebný. Platný pouze v současné době hodnotu "přihlášení", který vynucuje uživateli zadejte své přihlašovací údaje na žádosti. Jednotné přihlašování neprojeví. |

V tomto okamžiku uživateli se zobrazí výzva k dokončení pracovního postupu na zásadu. To může zahrnovat uživatele při zadávání svoje uživatelské jméno a heslo, přihlášení pomocí sociální identity registruje k adresáři nebo jiné číslo kroků podle toho, jak je definován zásadu.

Po dokončení zásady uživatele Azure AD vrátí odpověď aplikace na určené `redirect_uri`, pomocí metody uvedenou v `response_mode` parametr. Odpověď na bude přesně stejné pro každou z výše uvedených případech nezávislé zásad, které byl spuštěn.

Úspěšné odpovědí pomocí `response_mode=fragment` výsledek podobný jako:

```
GET https://aadb2cplayground.azurewebsites.net/#
id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parametr | Popis |
| ----------------------- | ------------------------------- |
| id_token | Id_token požadovanou aplikaci. Id_token slouží k ověření identity uživatele a zahájení relace s uživatelem. Další informace o id_tokens a jejich obsah jsou součástí [Azure AD B2C tokenu odkaz](active-directory-b2c-reference-tokens.md). |
| kód | Authorization_code požadovanou aplikaci, pokud jste použili `response_type=code+id_token`. Aplikaci můžete použít kód se tak mohli ověřovat požádat o access_token pro cílové zdrojů. Authorization_codes jsou velmi zkrátí. Obvykle vyprší jejich platnost po asi 10 minut. |
| Stav | Je-li parametr stát zahrnuta v pozvánce na schůzku, by se měly stejnou hodnotu v odpovědi. Aplikace by měl ověřte, jestli jsou stejné hodnoty stavu v žádostí a odpovědí. |

Chyba odpovědi můžete posílat taky `redirect_uri` tak, aby aplikace můžete řádně podporovat zpracování:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parametr | Popis |
| ----------------------- | ------------------------------- |
| Chyba | Řetězec kódu chyby, který slouží ke klasifikaci typy chyb probíhajících a mohou sloužit k reagovat na chyby. |
| error_description | Konkrétní chybové zprávy, které vám můžou pomoct vývojář příčinu kořenové chybu ověřování. |
| Stav | Zobrazit celou popisu v předchozí tabulce. Je-li parametr stát zahrnuta v pozvánce na schůzku, by se měly stejnou hodnotu v odpovědi. Aplikace by měl ověřte, jestli jsou stejné hodnoty stavu v žádostí a odpovědí. |


## <a name="validate-the-idtoken"></a>Ověření id_token
Jenom příjem id_token není dost k ověření uživatele – je nutné ověřit podpis id_token a ověřte nároky v tokenu podle požadavky vaše aplikace. Azure AD B2C používá [JSON Web tokeny (JWTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) a veřejné klíčové kryptografický podepsat tokeny a ověřte platnost.

Existuje mnoho otevřít zdroj knihoven, které jsou k dispozici pro ověřování JWTs, v závislosti na svůj jazyk předvoleb. Doporučujeme prozkoumání tyto možnosti, spíše než implementace logiky ověření. Zde uvedené informace budou užitečné při řešení problémů s správně používání těchto knihoven.

Azure AD B2C má OpenID připojení metadat koncový bod, který umožňuje načítat informace o Azure AD B2C při spuštění aplikace. Tyto informace obsahuje koncové body, obsah tokenů a token podepisování klíče. Je dokument JSON metadat pro každého zásadu ve vašem klientovi B2C. Například dokument metadat pro `b2c_1_sign_in` zásad v `fabrikamb2c.onmicrosoft.com` je umístěn v:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

Některé vlastnosti dokumentu konfigurace je `jwks_uri`, jejichž hodnota bude stejné zásady:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`.

V objednejte pro zjištění zásadu, které byly podepisování id_token (a where k načtení metadata ze), máte dvě možnosti. Nejprve je součástí na její název `acr` převzetí v id_token. Další informace o tom, jak analyzovat deklarací z id_token najdete v článku [Azure AD B2C tokenu odkaz](active-directory-b2c-reference-tokens.md). Další možností je kódovat zásad hodnoty `state` parametr při vydání žádosti a dekódovat a určit, které zásady byl použit. Dokonale platí obě metody.

Poté, co jste získali dokument metadat z koncového bodu metadat OpenID připojení, můžete použít veřejný zkratku RSA 256, (které se nacházejí na tomto koncovém bodu) ověřit podpis u id_token. Doména může obsahovat více zkratky uvedené v této koncového bodu v libovolném bodě v čase, označených `kid`. Záhlaví id_token obsahuje také `kid` převzetí, což znamená, které klávesy byl použit k podepsání id_token. Viz [Azure AD B2C tokenu odkaz](active-directory-b2c-reference-tokens.md) Další informace, včetně oddílů na [ověření tokeny](active-directory-b2c-reference-tokens.md#validating-tokens) a [Důležité informace o přihlášení přechodu klíče](active-directory-b2c-reference-tokens.md#validating-tokens).
<!--TODO: Improve the information on this-->

Po ověření podpis id_token existuje několik deklarace, které je třeba ověřit, například:

- Je třeba ověřit `nonce` požadovat, aby útokům tokenu přehrání. Jeho hodnota musí být uvedené v žádosti o přihlášení.
- Je třeba ověřit `aud` požadovat, aby zkontrolovat, zda id_token byl vydán aplikace. ID aplikace aplikace je třeba jeho hodnotu.
- Je třeba ověřit `iat` a `exp` tvrdí, ujistěte se, že id_token nevypršela.

Existuje několik další ověření, které by měl provést, a píše v [OpenID připojení základní specifikace](http://openid.net/specs/openid-connect-core-1_0.html).  Můžete taky další deklarací, podle toho, nefunguje ověření. Některé běžné ověření patří:

- Zajištění, aby uživatele nebo organizace přihlásila k odběru aplikace.
- Zajištění, jestli má uživatel správnou autorizace a oprávnění.
- Zajistit, že sílu ověřování došlo k chybě, například Azure Vícefaktorové ověřování.

Další informace o nároky v id_token najdete v článku [Azure AD B2C tokenu odkaz](active-directory-b2c-reference-tokens.md).

Po úplně ověření id_token můžete zahájit relaci s uživatelem a použít deklarace v id_token získat informace o uživateli v aplikaci. Tyto informace lze použít pro zobrazení, záznamů, povolení a tak dál.

## <a name="get-a-token"></a>Získání tokenu
Pokud všeho, co potřeb webové aplikace dělat spustit zásad, můžete přejít další několik oddílů. Následujícími částmi platí pouze pro webové aplikace, které potřebujete zdůraznit ověření volání webového rozhraní API a také chráněn Azure AD B2C.

Uplatnění authorization_code, který jste získali (pomocí `response_type=code+id_token`) pro token požadované zdroji tak, že `POST` požádat o `/token` koncového bodu. Je v současné době, pouze prostředek, který můžete požádat o token vaše aplikace vlastní back-end webového rozhraní API. Systém názvů žádosti o token sami sobě, je použít ID klienta vaše aplikace jako obor:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>

```

| Parametr | Povinné? | Popis |
| ----------------------- | ------------------------------- | --------------------- |
| p | Povinné | Zásad, která byla použita k získání kódu se tak mohli ověřovat. Jiné zásady nelze použít v této žádosti. Všimněte si přidat tento parametr s **řetězci dotazu**, není v těle příspěvku. |
| client_id | Povinné | ID aplikace přiřazené [Azure portál](https://portal.azure.com/) aplikace. |
| grant_type | Povinné | Typ grant (udělit), která musí být `authorization_code` toku kód ověření. |
| rozsah | Doporučené | Seznam oborů oddělený mezerami. Jeden obor hodnota označuje Azure AD obou oprávnění, které jsou požadovány. `openid` Rozsah Určuje oprávnění k přihlášení uživatele a získání dat o uživateli ve formě **id_tokens**. Můžete použít k získání tokeny webu back-end aplikace vlastní rozhraní API, který je znázorněn stejné ID aplikace jako klient. `offline_access` Obor označuje, že aplikace, musí **refresh_token** dlouhodobé přístupu pro zdroje. |
| kód | Povinné | Authorization_code, který jste získali v první nohy toku. |
| redirect_uri | Povinné | Redirect_uri aplikace, kterou jste dostali authorization_code. |
| client_secret | Povinné | Tajná aplikace, které vygenerovalo [Azure portálu](https://portal.azure.com/). Tajná tato aplikace je artefaktem důležité zabezpečení. Měli byste ukládat ho bezpečně na server. Jste měli také starat otočit tohoto klienta tajná v pravidelných intervalech. |

Odpověď na úspěšné tokenu bude vypadat jako:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Parametr | Popis |
| ----------------------- | ------------------------------- |
| not_before | Čas, pro niž tokenu je považován za platné v čase epocha. |
| token_type | Tokenu typ hodnoty. Pouze typ, který podporuje Azure AD je nosný. |
| access_token | Podepsané JWT tokenu požadovanými. |
| rozsah | Oborů, které tokenu platí pro, která se dá použít pro ukládání do mezipaměti tokeny pro pozdější použití. |
| expires_in | Časový úsek, která access_token platí (v sekundách). |
| refresh_token | Refresh_token OAuth 2.0. Aplikaci můžete použít tento token získat další tokeny po vypršení platnosti aktuálního tokenu.  Refresh_tokens je dlouhodobě a mohou sloužit k uchovávat přístupu k prostředkům dlouhou dobu. Další podrobnosti najdete [B2C tokenu odkaz](active-directory-b2c-reference-tokens.md). Poznámku, třeba jste použili obor `offline_access` v autorizace a tokenu požadavky získali refresh_token. |

Chyba odpovědi bude vypadat jako:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parametr | Popis |
| ----------------------- | ------------------------------- |
| Chyba | Řetězec kódu chyby, který slouží ke klasifikaci typy chyb probíhajících a mohou sloužit k reagovat na chyby. |
| error_description | Konkrétní chybové zprávy, které vám můžou pomoct vývojář příčinu kořenové chybu ověřování. |

## <a name="use-the-token"></a>Použití tokenu
Teď jste získali úspěšně `access_token`, můžete tokenu v žádosti o vaší back-end webu rozhraní API s využitím v `Authorization` záhlaví:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-token"></a>Aktualizace tokenu
Id_tokens jsou zkrátí. Je třeba aktualizovat po vypršení jejich platnosti pokračovat, nebudete mít přístup k zdroje. Můžete to udělat tak, že druhý odeslání `POST` požádat o `/token` koncového bodu. Poskytovat tuto chvíli `refresh_token` místo `code`:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=openid offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>
```

| Parametr | Povinné | Popis |
| ----------------------- | ------------------------------- | -------- |
| p | Povinné | Zásad, která byla použita k získání původní refresh_token. Jiné zásady nelze použít v této žádosti. Všimněte si přidat tento parametr s **řetězci dotazu**, není v těle příspěvku. |
| client_id | Povinné | ID aplikace přiřazené [Azure portál](https://portal.azure.com/) aplikace. |
| grant_type | Povinné | Typ grant (udělit), která musí být `refresh_token` pro tento nohy směrování kód ověření. |
| rozsah | Doporučené | Seznam oborů oddělený mezerami. Jeden obor hodnota označuje Azure AD obou oprávnění, které jsou požadovány. `openid` Rozsah Určuje oprávnění k přihlášení uživatele a získání dat o uživateli ve formě **id_tokens**. Můžete použít k získání tokeny webu back-end aplikace vlastní rozhraní API, který je znázorněn stejné ID aplikace jako klient. `offline_access` Obor označuje, že aplikace, musí **refresh_token** dlouhodobé přístupu pro zdroje. |
| redirect_uri | Doporučené | Redirect_uri aplikace, kterou jste dostali authorization_code. |
| refresh_token | Povinné | Původní refresh_token, který jste získali v druhém nohy toku. Poznámku, třeba jste použili obor `offline_access` v autorizace a tokenu požadavky získali refresh_token. |
| client_secret | Povinné | Tajná aplikace, které vygenerovalo [Azure portálu](https://portal.azure.com/). Tajná tato aplikace je artefaktem důležité zabezpečení. Měli byste ukládat ho bezpečně na server. Jste měli také starat otočit tohoto klienta tajná v pravidelných intervalech. |

Odpověď na úspěšné tokenu bude vypadat jako:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Parametr | Popis |
| ----------------------- | ------------------------------- |
| not_before | Čas, pro niž tokenu je považován za platné v čase epocha. |
| token_type | Tokenu typ hodnoty. Pouze typ, který podporuje Azure AD je nosný. |
| access_token | Podepsané JWT tokenu požadovanými. |
| rozsah | Oborů, které tokenu platí pro, která se dá použít pro ukládání do mezipaměti tokeny pro pozdější použití. |
| expires_in | Časový úsek, která access_token platí (v sekundách). |
| refresh_token | Refresh_token OAuth 2.0. Aplikaci můžete použít tento token získat další tokeny po vypršení platnosti aktuálního tokenu.  Refresh_tokens je dlouhodobě a mohou sloužit k uchovávat přístupu k prostředkům dlouhou dobu. Další podrobnosti najdete [B2C tokenu odkaz](active-directory-b2c-reference-tokens.md). |

Chyba odpovědi bude vypadat jako:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parametr | Popis |
| ----------------------- | ------------------------------- |
| Chyba | Řetězec kódu chyby, který slouží ke klasifikaci typy chyb probíhajících a mohou sloužit k reagovat na chyby. |
| error_description | Konkrétní chybové zprávy, které vám můžou pomoct vývojář příčinu kořenové chybu ověřování. |


## <a name="send-a-sign-out-request"></a>Pošlete žádost o odhlašovací

Když budete chtít uživatele z aplikace odhlásit, není dost vymažte soubory cookie vaše aplikace nebo jinak ukončení relace s uživatelem. Uživatel musí také přesměrovat Azure AD na Odhlásit se. Pokud neuděláte tak, může uživatel moct opakovaně ověřit nad aplikace bez své přihlašovací údaje znovu zadat. Je to proto, mají uživatelé platné přihlašování relaci s Azure AD.

Můžete jednoduše přesměrovat uživateli `end_session_endpoint` , který je uvedený ve stejném OpenID připojení metadat dokumentu popisu v předchozí části "Ověřit id_token":

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/logout?
p=b2c_1_sign_in
&post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| Parametr | Povinné? | Popis |
| ----------------------- | ------------------------------- | ------------ |
| p | Povinné | Pravidlo, které chcete použít k přihlášení uživatele mimo aplikaci. |
| post_logout_redirect_uri | Doporučené | Adresu URL, kterou by měl přesměrován uživatele po úspěšném odhlašovací. Pokud není zahrnut, uživatel zobrazí obecný zprávy tak, že Azure AD B2C.  |

> [AZURE.NOTE]
    Při odkazující uživatele, aby `end_session_endpoint` smažete některé uživatele jeden přihlašování stát s Azure AD B2C, nebudete přihlášení uživatele mimo relace poskytovatele (IDP) sociální identita uživatele. Pokud uživatel vybere stejné IDP během pozdější přihlášení, budou bude mít k novému ověření, bez zadání své přihlašovací údaje. Pokud se chcete odhlásit z aplikace B2C chce uživatel, ho neznamená, že se chcete odhlásit se od svůj Facebookový účet úplně. Však v případě místních účtů relace uživatele bude ukončena správně.

## <a name="use-your-own-b2c-tenant"></a>Použití vlastní B2C klienta

Pokud chcete, můžete vyzkoušet tyto požadavky pro sebe, musíte nejdřív provádět tyto tři kroky a nahradit hodnoty příklad nad vlastní:

- [Vytvořit B2C klienta](active-directory-b2c-get-started.md)a použití názvu vašeho klienta do žádosti.
- [Vytvoření aplikace](active-directory-b2c-app-registration.md) pro získání ID aplikace a redirect_uri. Chcete zahrnout **webové aplikace nebo webové rozhraní api** ve své aplikaci a volitelně vytvořit **tajná aplikace**.
- [Vytvoření pravidla](active-directory-b2c-reference-policies.md) získat zásad názvy.
