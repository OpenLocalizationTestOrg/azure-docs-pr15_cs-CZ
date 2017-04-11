

<properties
    pageTitle="Povolení OAuth verze 2.0 Azure AD kódu toku | Microsoft Azure"
    description="Vytváření webových aplikací pomocí Azure AD provádění protokolu OAuth 2.0."
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
    ms.date="08/08/2016"
    ms.author="dastrock"/>

# <a name="v20-protocols---oauth-20-authorization-code-flow"></a>Protokoly verze 2.0 – OAuth 2.0 se tak mohli ověřovat kód toku

Udělení OAuth 2.0 se tak mohli ověřovat kód lze použít v aplikacích, které jsou instalovány na zařízení k získání přístupu k chráněným prostředkům, například webového rozhraní API.  Pomocí verze 2.0 modelu aplikace provádění OAuth 2.0, můžete přidat, přihlaste se a rozhraní API přístup ke svým aplikacím přenosných a stolních.  Tato příručka je nezávislý na jazyk a popisuje, jak k odesílání a přijímání zpráv HTTP bez použití libovolného příkazu pro naše knihoven otevřít zdroj.

<!-- TODO: Need link to libraries -->

> [AZURE.NOTE]
    Některé scénáře Azure Active Directory a funkce jsou podporovány koncový bod verze 2.0.  Pokud chcete zjistit, zda by měly používat koncový bod verze 2.0, přečtěte si informace o [omezení verze 2.0](active-directory-v2-limitations.md).

Tok kód se tak mohli ověřovat OAuth 2.0 je popsaná v v [části 4.1 specifikace OAuth 2.0](http://tools.ietf.org/html/rfc6749).  Slouží k provádění a tak mohli ověřovat ve většině aplikace typů, včetně [webových aplikací web apps](active-directory-v2-flows.md#web-apps) a [nativně nainstalované aplikace](active-directory-v2-flows.md#mobile-and-native-apps).  Umožňuje aplikace bezpečně získat access_tokens, které lze použít pro přístup k prostředky, které jsou správně zapojené pomocí verze 2.0 koncový bod.  

## <a name="protocol-diagram"></a>Diagram Protocol (protokol)
Na vysoké úrovni celého ověřování tok nativní/mobilní aplikace trochu vypadá nějak takto:

![OAuth Auth kód toku](../media/active-directory-v2-flows/convergence_scenarios_native.png)

## <a name="request-an-authorization-code"></a>Žádost o povolení kód
Tok kód se tak mohli ověřovat začíná klienta odkazující uživatele, aby `/authorize` koncového bodu.  V této žádosti klienta označuje oprávnění, která potřebuje získat od uživatele:

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&state=12345
```

> [AZURE.TIP] Po kliknutí na odkaz k provedení tohoto požadavku. Po přihlášení prohlížeči přesměrovaní na `https://localhost/myapp/` s `code` do adresního řádku.
    <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=code&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&response_mode=query&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&state=12345" target="_blank">https://Login.microsoftonline.com/common/oauth2/v2.0/authorize...</a>

| Parametr | | Popis |
| ----------------------- | ------------------------------- | --------------- |
| klienta | povinné | `{tenant}` Hodnoty v parametru path žádosti o lze určit, kdo může přihlásit k aplikaci.  Jsou povolené hodnoty `common`, `organizations`, `consumers`a klient identifikátory.  Další podrobnosti najdete v článku [základní informace o protokolu](active-directory-v2-protocols.md#endpoints). |
| client_id | povinné | Id aplikace portál registrace ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) přiřazené aplikace. |
| response_type | povinné | Musí obsahovat `code` toku kód ověření. |
| redirect_uri | Doporučené | Redirect_uri aplikace, kde můžete ověřování odpovědi odešlete nebo přijmete tak, že aplikace.  To se musí přesně shodovat jednu redirect_uris zapsaný na portálu kromě musí být překódován jako adresa url.  Pro nativní a mobilní aplikace, byste měli použít výchozí hodnoty `https://login.microsoftonline.com/common/oauth2/nativeclient`. |
| rozsah | povinné | Seznam [oborů](active-directory-v2-scopes.md) , které má uživatel souhlas s oddělený mezerami.  |
| response_mode | Doporučené | Určuje metodu, která má být použit k odeslání výsledné tokenu zpět do aplikace.  Může být `query` nebo `form_post`.  |
| Stav | Doporučené | Hodnota součástí žádosti o, bude vrácena také v tokenu odpovědi.  Může být řetězec veškerý obsah, který chcete.  Náhodně generovaným jedinečnou hodnotu se obvykle používá pro [Zabránění útoky padělání žádost webů](http://tools.ietf.org/html/rfc6749#section-10.12).  Stav je také použít ke kódování informace o stavu uživatele v aplikaci před požadavek na ověření došlo k chybě, například stránky nebo zobrazení, které byly na. |
| dotaz | volitelné | Označuje typ interakce s uživatelem, který je potřebný.  Platný pouze hodnoty v současné době jsou "přihlášení", "žádný" a "souhlas.  `prompt=login`Vynutí uživateli zadejte své přihlašovací údaje na žádost, negace jednotného přihlašování na.  `prompt=none`je opakem - budete mít jistotu, že uživatel nezobrazují se libovolný interaktivní zprávou jakékoliv.  Pokud žádost nelze dokončit tiše prostřednictvím jednotného přihlašování na, koncový bod verze 2.0 vrátí chybu.  `prompt=consent`Spustí OAuth dialogovém souhlas, když uživatel přihlásí, s dotazem, uživatelům udělit oprávnění k aplikaci. |
| login_hint | volitelné | Slouží k předem zadání pole uživatelské jméno nebo e-mailovou adresu obrazovky přihlašovací stránky pro uživatele, pokud znáte svoje uživatelské jméno předem.  Často aplikace použijete tento parametr během nové ověřování, máte už extrahovaných uživatelské jméno z předchozí přihlášení pomocí `preferred_username` deklarace. |
| domain_hint | volitelné | Může se jednat o `consumers` nebo `organizations`.  Pokud, přeskočí proces zjišťování e mailové tento uživatel prochází na stránce verze 2.0 přihlásit, vést k mírně více optimalizovaného uživatelského prostředí.  Často aplikace se nepoužívá tento parametr během nové ověřování extrahování `tid` z předchozí přihlášení.  Pokud `tid` převzetí hodnotu `9188040d-6c67-4c5b-b112-36a304b66dad`, byste měli použít `domain_hint=consumers`.  V ostatních případech použít `domain_hint=organizations`. |

V tomto okamžiku že uživatel bude požádán, aby zadejte své přihlašovací údaje a dokončit ověření.  Koncový bod verze 2.0 také zajistí, že uživatel souhlasí oprávnění vyznačené `scope` dotazu parametr.  Pokud některý z těchto oprávnění nedala souhlas uživatele, bude požádejte uživatele, aby souhlas s potřebná oprávnění.  Podrobnosti o [zde uvedené oprávnění, souhlas a více klienta aplikace](active-directory-v2-scopes.md).

Jakmile je uživatel ověří a uděluje souhlas, koncový bod verze 2.0 vrátí odpověď aplikace v určené `redirect_uri`, metodou podle `response_mode` parametr.

#### <a name="successful-response"></a>Úspěšné odpověď
Úspěšné odpovědí pomocí `response_mode=query` vypadá podobně jako:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=12345
```

| Parametr | Popis |
| ----------------------- | ------------------------------- |
| kód | Authorization_code požadovanou aplikaci. Aplikaci můžete použít kód se tak mohli ověřovat požádat o přístupový token pro cílové zdrojů.  Authorization_codes jsou velmi zkrátí, obvykle vyprší jejich platnost po asi 10 minut. |
| Stav | Je-li parametr stát zahrnuta v pozvánce na schůzku, by se měly stejnou hodnotu v odpovědi. Aplikace by měl ověřte, jestli jsou stejné hodnoty stavu v žádostí a odpovědí. |

#### <a name="error-response"></a>Chyba odpověď
Chyba odpovědi může posílat taky `redirect_uri` tak aplikaci můžete řádně podporovat zpracování:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parametr | Popis |
| ----------------------- | ------------------------------- |
| Chyba | Řetězec kódu chyby, který slouží ke klasifikaci typy chyb probíhajících a mohou sloužit k reagovat na chyby. |
| error_description | Konkrétní chybové zprávy, které vám můžou pomoct vývojář příčinu kořenové chybu ověřování.  |

#### <a name="error-codes-for-authorization-endpoint-errors"></a>Kódy chyb se tak mohli ověřovat koncový bod chyby

Následující tabulka popisuje jednotlivé kódy chyb, které může být vrácen v `error` parametr odpovědi na chyby.

| Kód chyby | Popis | Akce klienta |
|------------|-------------|---------------|
| invalid_request | Protocol (protokol) došlo k chybě, například chybějící potřeba parametr. | Řešení a opětovné odeslání žádosti. Toto je vývoj chyba je zpravidla zachyceny počáteční testování.|
| unauthorized_client | Klientská aplikace není povoleno s žádostí o povolení kód. | Obvykle k tomu dojde, když klientské aplikace není registrovaný v Azure AD nebo nepřidá uživatele Azure AD klienta. Aplikaci můžete výzva k zadání s pokyny k instalaci aplikace a pak ho přidejte do Azure AD. |
| ACCESS_DENIED | Odepřen souhlas vlastníka zdroje | Klientská aplikace může uživatel, který nelze přejít, pokud uživatel souhlasí upozornění. |
| unsupported_response_type | Povolení server nepodporuje typ odpovědi v pozvánce. | Řešení a opětovné odeslání žádosti. Toto je vývoj chyba je zpravidla zachyceny počáteční testování.|
|server_error | Došlo k neočekávané chybě. | Opakování žádosti. Tyto chyby mohou výsledkem dočasné podmínky. Klientská aplikace může vysvětlit uživatele, že je zpožděných odpovědi z důvodu dočasné chyby. |
| temporarily_unavailable | Server je dočasně přeplněné zpracování požadavku. | Opakování žádosti. Klientská aplikace může vysvětlit uživatele, že je zpožděných odpovědi z důvodu dočasné podmínky. |
| invalid_resource |Cílový prostředek je neplatný, protože není k dispozici, nemůžete ho najít Azure AD nebo není správně nakonfigurované.| To znamená, že zdroje, pokud existuje, není nakonfigurovaný v klientovi. Aplikaci můžete výzva k zadání s pokyny k instalaci aplikace a pak ho přidejte do Azure AD. |

## <a name="request-an-access-token"></a>Žádost o přístupový token
Teď, když jste získali authorization_code a máte přidělená oprávnění uživatelem, můžete uplatnit `code` pro `access_token` požadované zdroji tak, že `POST` požádat o `/token` koncového bodu:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=authorization_code
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

> [AZURE.TIP] Zkuste spuštěním tohoto požadavku v pošťák! (Nezapomeňte nahradit `code`)     [ ![Se spouštějí v pošťák](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)

| Parametr | | Popis |
| ----------------------- | ------------------------------- | --------------------- |
| klienta | povinné | `{tenant}` Hodnoty v parametru path žádosti o lze určit, kdo může přihlásit k aplikaci.  Jsou povolené hodnoty `common`, `organizations`, `consumers`a klient identifikátory.  Další podrobnosti najdete v článku [základní informace o protokolu](active-directory-v2-protocols.md#endpoints). |
| client_id | povinné | Id aplikace portál registrace ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) přiřazené aplikace. |
| grant_type | povinné | Musí být `authorization_code` toku kód ověření. |
| rozsah | povinné | Seznam oborů oddělený mezerami.  Obory požadované v tomto nohy musí být odpovídá nebo podmnožinu obory požadované první nohy.  Pokud obory podle tohoto požadavku zahrnují více servery zdrojů, koncový bod verze 2.0 vrátí token zdroje podle prvního oboru.  Podrobnější vysvětlení obory odkazy na [oprávnění, souhlas a obory](active-directory-v2-scopes.md).  |
| kód | povinné | Authorization_code, který jste získali v první nohy toku.   |
| redirect_uri | povinné | Stejné redirect_uri hodnota, která byla použita k získání authorization_code. |
| client_secret | požadovaný pro webové aplikace | Tajná aplikace, který jste vytvořili v portálu registrace aplikace aplikace.  Není vhodné používat nativní aplikaci, protože client_secrets nelze uložit problémy se spolehlivým na zařízeních.  Je potřeba pro web apps a webové rozhraní API, které mají možnost bezpečně ukládat client_secret na straně serveru. |

#### <a name="successful-response"></a>Úspěšné odpověď
Odpověď na úspěšné tokenu bude vypadat jako:

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parametr | Popis |
| ----------------------- | ------------------------------- |
| access_token | Požadovaná přístupový token. Aplikaci můžete použít tento token ověření zabezpečeným prostředku, například webového rozhraní API. |
| token_type | Určuje typ tokenu hodnotu. Pouze typ, který podporuje Azure AD je nosný  |
| expires_in | Jak dlouho přístupový token je platný (v sekundách). |
| rozsah | Obory, které access_token platí pro. |
| refresh_token |  Aktualizace token OAuth 2.0. Aplikaci můžete použít tento token získat další přístup tokeny po vypršení platnosti aktuálního přístupový token.  Refresh_tokens jsou dlouhodobé a mohou sloužit k uchovávat přístupu k prostředkům dlouhou dobu.  Podrobněji podívejte se do [verze 2.0 tokenu odkaz](active-directory-v2-tokens.md).  |
| id_token | Nepodepsané JSON Web tokenu (JWT). Base64Url se dají aplikace dekódovat segmentů tento token na žádost o informace o uživateli přihlášenému. Aplikaci můžete mezipaměti hodnoty a zobrazovat je, ale neměli závisí na nich se tak mohli ověřovat nebo hranice zabezpečení.  Další informace o id_tokens najdete v článku [tokenu reference verze 2.0 koncového bodu](active-directory-v2-tokens.md). |

#### <a name="error-response"></a>Chyba odpověď
Chyba odpovědi bude vypadat jako:

```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Parametr | Popis |
| ----------------------- | ------------------------------- |
| Chyba | Řetězec kódu chyby, který slouží ke klasifikaci typy chyb probíhajících a mohou sloužit k reagovat na chyby. |
| error_description | Konkrétní chybové zprávy, které vám můžou pomoct vývojář příčinu kořenové chybu ověřování.  |
| error_codes | Seznam kódů specifickou chybu služba tokenů zabezpečení, které mohou usnadnit diagnostických nástrojů.  |
| časové razítko | Čas, kdy došlo k chybě. |
| trace_id | Jedinečný identifikátor pro požadavek, který mohou usnadnit diagnostických nástrojů.  |
| correlation_id | Jedinečný identifikátor pro požadavek, který mohou usnadnit diagnostiky přes součásti. |

#### <a name="error-codes-for-token-endpoint-errors"></a>Kódy chyb chyb tokenu koncový bod

| Kód chyby | Popis | Akce klienta |
|------------|-------------|---------------|
| invalid_request | Protocol (protokol) došlo k chybě, například chybějící potřeba parametr. | Řešení a opětovné odeslání žádosti |
| invalid_grant | Kód se tak mohli ověřovat neplatný nebo vypršela. | Vyzkoušejte nový požadavek `/authorize` koncový bod |
| unauthorized_client | Chcete použít tento typ grant (udělit) se tak mohli ověřovat nemá oprávnění ověřeného klienta. | Obvykle k tomu dojde, když klientské aplikace není registrovaný v Azure AD nebo nepřidá uživatele Azure AD klienta. Aplikaci můžete výzva k zadání s pokyny k instalaci aplikace a pak ho přidejte do Azure AD. |
| invalid_client | Ověření klienta se nezdařila. | Pověření klienta nejsou platné. Pokud chcete opravit, správce aplikace aktualizuje pověření. |
| unsupported_grant_type | Povolení server nepodporuje typ grant (udělit) se tak mohli ověřovat. | Změna typu grant (udělit) v pozvánce. Tento typ chyby by měla proběhnout pouze během vývoje a zjišťování počáteční testování. |
| invalid_resource | Cílový prostředek je neplatný, protože není k dispozici, nemůžete ho najít Azure AD nebo není správně nakonfigurované. | To znamená, že zdroje, pokud existuje, není nakonfigurovaný v klientovi. Aplikaci můžete výzva k zadání s pokyny k instalaci aplikace a pak ho přidejte do Azure AD. |
| interaction_required | Pokud chcete žádost vyžaduje interakci uživatele. Příklad krok další ověření požaduje sledování. | Opakování žádosti o s stejné zdroje. |
| temporarily_unavailable | Server je dočasně přeplněné zpracování požadavku. | Opakování žádosti. Klientská aplikace může vysvětlit uživatele, že je zpožděných odpovědi z důvodu dočasné podmínky.|

## <a name="use-the-access-token"></a>Použití přístupový token
Teď jste získali úspěšně `access_token`, aktivujete tokenu v žádosti o rozhraní API webových včetně ji v `Authorization` záhlaví:

> [AZURE.TIP] Provedení tohoto požadavku v pošťák! (Nahradit `Authorization` záhlaví první)     [ ![Se spouštějí v pošťák](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-access-token"></a>Aktualizace přístupový token
Access_tokens jsou krátké životnost a musí aktualizujete je po vypršení jejich platnosti pokračujte přístupu k prostředkům.  Můžete to udělat tak, že druhý odeslání `POST` požádat o `/token` koncový bod, tentokrát poskytující `refresh_token` místo `code`:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=refresh_token
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

> [AZURE.TIP] Zkuste spuštěním tohoto požadavku v pošťák! (Nezapomeňte nahradit `refresh_token`)     [ ![Se spouštějí v pošťák](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)

| Parametr | | Popis |
| ----------------------- | ------------------------------- | -------- |
| klienta | povinné | `{tenant}` Hodnoty v parametru path žádosti o lze určit, kdo může přihlásit k aplikaci.  Jsou povolené hodnoty `common`, `organizations`, `consumers`a klient identifikátory.  Další podrobnosti najdete v článku [Základy protokol](active-directory-v2-protocols.md#endpoints). |
| client_id | povinné | Id aplikace portál registrace ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) přiřazené aplikace. |
| grant_type | povinné | Musí být `refresh_token` pro tento nohy směrování kód ověření. |
| rozsah | povinné | Seznam oborů oddělený mezerami.  Obory požadované v tomto nohy musí být odpovídá nebo podmnožinu obory požadované původní žádost o nohy authorization_code.  Pokud obory podle tohoto požadavku zahrnují více servery zdrojů, koncový bod verze 2.0 vrátí token zdroje podle prvního oboru.  Podrobnější vysvětlení obory odkazy na [oprávnění, souhlas a obory](active-directory-v2-scopes.md).  |
| refresh_token | povinné | Refresh_token, který jste získali v druhém nohy toku.   |
| redirect_uri | povinné | Stejné redirect_uri hodnota, která byla použita k získání authorization_code. |
| client_secret | požadovaný pro webové aplikace | Tajná aplikace, který jste vytvořili v portálu registrace aplikace aplikace.  Není vhodné používat nativní aplikaci, protože client_secrets nelze uložit problémy se spolehlivým na zařízeních.  Je potřeba pro web apps a webové rozhraní API, které mají možnost bezpečně ukládat client_secret na straně serveru. |

#### <a name="successful-response"></a>Úspěšné odpověď
Odpověď na úspěšné tokenu bude vypadat jako:

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parametr | Popis |
| ----------------------- | ------------------------------- |
| access_token | Požadovaná přístupový token. Aplikaci můžete použít tento token ověření zabezpečené prostředku, například webového rozhraní API. |
| token_type | Určuje typ tokenu hodnotu. Pouze typ, který podporuje Azure AD je nosný  |
| expires_in | Jak dlouho přístupový token je platný (v sekundách). |
| rozsah | Obory, které access_token platí pro. |
| refresh_token |  Nový token OAuth 2.0 aktualizace. Měli byste nahradit staré token aktualizace tento nově získaná aktualizace token zajistit že aktualizace tokeny platné po dobu.  |
| id_token | Nepodepsané JSON Web tokenu (JWT). Base64Url se dají aplikace dekódovat segmentů tento token na žádost o informace o uživateli přihlášenému. Aplikaci můžete mezipaměti hodnoty a zobrazovat je, ale neměli závisí na nich se tak mohli ověřovat nebo hranice zabezpečení.  Další informace o id_tokens najdete v článku [tokenu reference verze 2.0 koncového bodu](active-directory-v2-tokens.md). |

#### <a name="error-response"></a>Chyba odpověď
```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Parametr | Popis |
| ----------------------- | ------------------------------- |
| Chyba | Řetězec kódu chyby, který slouží ke klasifikaci typy chyb probíhajících a mohou sloužit k reagovat na chyby. |
| error_description | Konkrétní chybové zprávy, které vám můžou pomoct vývojář příčinu kořenové chybu ověřování.  |
| error_codes | Seznam kódů specifickou chybu služba tokenů zabezpečení, které mohou usnadnit diagnostických nástrojů.  |
| časové razítko | Čas, kdy došlo k chybě. |
| trace_id | Jedinečný identifikátor pro požadavek, který může pomoci při diagnostických nástrojů.  |
| correlation_id | Jedinečný identifikátor pro požadavek, který mohou usnadnit diagnostiky přes součásti. |

Popis kódy chyb a akce doporučené klienta najdete v článku [kódy chyb chyb tokenu koncového bodu](#error-codes-for-token-endpoint-errors).
