<properties
    pageTitle="Přehled protokolu .NET Azure AD | Microsoft Azure"
    description="Tento článek popisuje, jak povolit přístup k webovým aplikacím a webového rozhraní API ve vašem klientovi pomocí služby Azure Active Directory a OpenID připojení pomocí protokolu HTTP zpráv."
    services="active-directory"
    documentationCenter=".net"
    authors="priyamohanram"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="priyamo"/>


# <a name="authorize-access-to-web-applications-using-openid-connect-and-azure-active-directory"></a>Povolit přístup k webové aplikace pomocí OpenID připojení a Azure Active Directory

[Připojení OpenID](http://openid.net/specs/openid-connect-core-1_0.html) je jednoduchý identity vrstva této technologii postavená protokol OAuth 2.0. OAuth 2.0 definuje mechanismy získat a používat **tokeny Accessu** pro přístup k chráněné prostředky, ale ne definují standardní metod pro informování identity. Připojení OpenID implementuje ověřování jako protažení proces se tak mohli ověřovat OAuth 2.0 poskytuje informace o koncového uživatele ve formě `id_token` , ověří identita uživatele a také poskytuje profil základní informace o uživateli.

Připojení OpenID je naše doporučení, pokud vytváříte webovou aplikaci, která je hostitelem server a k nim získat přístup prostřednictvím prohlížeče.

## <a name="authentication-flow-using-openid-connect"></a>Tok ověřování pomocí OpenID spojení

Základní vývojový přihlašovací obsahuje následující kroky: každý z nich se píše níže.

![OpenId připojení tok ověřování](media/active-directory-protocols-openid-connect-code/active-directory-oauth-code-flow-web-app.png)


## <a name="send-the-sign-in-request"></a>Odeslání žádosti o přihlášení

Potřebujete-li ověření uživatele webové aplikace, musí přesměrování uživatele `/authorize` koncový bod. Tento požadavek je podobný první nohy z části [OAuth 2.0 se tak mohli ověřovat kód toku](active-directory-protocols-oauth-code.md), s několika důležité rozdíly:

- Žádost musí obsahovat obor `openid` v `scope` parametr.
- `response_type` Parametr musí obsahovat `id_token`.
- Žádost musí obsahovat `nonce` parametr.

Žádost o vzorek tak by vypadal takto:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=form_post
&scope=openid
&state=12345
&nonce=7362CAEA-9CA5-4B43-9BA3-34D7C303EBA7
```

| Parametr | | Popis |
| ----------------------- | ------------------------------- | --------------- |
| klienta | povinné | `{tenant}` Hodnoty v parametru path žádosti o lze určit, kdo může přihlásit k aplikaci.  Povolené hodnoty jsou klienta, například `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` nebo `contoso.onmicrosoft.com` nebo `common` tokeny nezávislým klienta |
| client_id | povinné | Id aplikace přiřazené k aplikaci při registraci s Azure AD. Můžete najít v portálu klasické Azure. Klepněte na **Služby Active Directory**, klikněte na adresář, klepněte na aplikace a potom klikněte na **Konfigurovat** |
| response_type | povinné | Musí obsahovat `id_token` pro připojení OpenID přihlášení.  Může taky zahrnovat další response_types jako `code`. |
| rozsah | povinné | Seznam oborů oddělený mezerami.  Pro OpenID připojit, musí obsahovat obor `openid`, které se převádí na oprávnění "Přihlásit" v souhlasu uživatelského rozhraní.  Jiné obory mohou také obsahovat v tomto žádosti o žádosti o souhlas. |
| Nonce | povinné | Hodnoty zahrnuté v pozvánce na schůzku, generovaný aplikaci, která bude obsahovat výsledné `id_token` jako deklaraci.  Aplikaci můžete ověřit pak tuto hodnotu na zmírnění tokenu opakované útoky.  Hodnota je obvykle náhodného jedinečného řetězce nebo GUID použitý k identifikaci origin žádost.  |
| redirect_uri | Doporučené | Redirect_uri aplikace, kde můžete ověřování odpovědi odešlete nebo přijmete tak, že aplikace.  To se musí přesně shodovat jednu redirect_uris zapsaný na portálu kromě musí být překódován jako adresa url. |
| response_mode | Doporučené | Určuje metodu, která má být použit k odeslání výsledné authorization_code zpátky do aplikace.  Podporované hodnoty jsou `form_post` pro *HTTP formuláře* nebo `fragment` pro *část adresy URL*.  U webových aplikací doporučujeme používat `response_mode=form_post` k zajištění nejbezpečnější přenosu tokeny aplikaci.  
| Stav | Doporučené | Hodnota součástí požadavek, který bude vrácena také v tokenu odpovědi.  Může být řetězec veškerý obsah, který chcete.  Náhodně generovaným jedinečnou hodnotu se obvykle používá pro [Zabránění útoky padělání žádost webů](http://tools.ietf.org/html/rfc6749#section-10.12).  Stav je také použít ke kódování informace o stavu uživatele v aplikaci před požadavek na ověření došlo k chybě, například stránky nebo zobrazení, které byly na. |
| dotaz | volitelné | Označuje typ interakce s uživatelem, který je potřebný.  Platný pouze hodnoty v současné době jsou "přihlášení", "žádný" a "souhlas.  `prompt=login`Vynutí uživateli zadejte své přihlašovací údaje na žádost, negace jednotného přihlašování na.  `prompt=none`je opakem - budete mít jistotu, že uživatel nezobrazují se libovolný interaktivní zprávou jakékoliv.  Pokud žádost nelze dokončit tiše prostřednictvím jednotného přihlašování na, koncový bod vrátí chybu.  `prompt=consent`Spustí OAuth dialogovém souhlas, když uživatel přihlásí, s dotazem, uživatelům udělit oprávnění k aplikaci. |
| login_hint | volitelné | Slouží k předem zadání pole uživatelské jméno nebo e-mailovou adresu obrazovky přihlašovací stránky pro uživatele, pokud znáte svoje uživatelské jméno předem.  Často aplikace použijete tento parametr během nové ověřování, máte už extrahovaných uživatelské jméno z předchozí přihlášení pomocí `preferred_username` deklarace. |


V tomto okamžiku že uživatel bude požádán, aby zadejte své přihlašovací údaje a dokončit ověření.

### <a name="sample-response"></a>Ukázka odpověď

Ukázka odpověď po ověření uživatele se může vypadat takto:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&state=12345
```

| Parametr | Popis |
| ----------------------- | ------------------------------- |
| id_token | `id_token` Požadovanou aplikaci. Můžete použít `id_token` ověřit identitu uživatele a zahájení relace s uživatelem.  |
| Stav | Hodnota součástí požadavek, který bude taky vrácena jako token odpověď. Náhodně generovaným jedinečnou hodnotu se obvykle používá pro [Zabránění útoky padělání žádost webů](http://tools.ietf.org/html/rfc6749#section-10.12).  Stav je také použít ke kódování informace o stavu uživatele v aplikaci před požadavek na ověření došlo k chybě, například stránky nebo zobrazení, které byly na. |

### <a name="error-response"></a>Chyba odpověď
Chyba odpovědi může posílat taky `redirect_uri` tak aplikaci můžete řádně podporovat zpracování:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parametr | Popis |
| ----------------------- | ------------------------------- |
| Chyba | Řetězec kódu chyby, který slouží ke klasifikaci typy chyb probíhajících a mohou sloužit k reagovat na chyby. |
| error_description | Konkrétní chybové zprávy, které vám můžou pomoct vývojář příčinu kořenové chybu ověřování.  |

#### <a name="error-codes-for-authorization-endpoint-errors"></a>Kódy chyb se tak mohli ověřovat koncový bod chyby

Následující tabulka popisuje jednotlivé kódy chyb, které může být vrácen v `error` parametr odpovědi na chyby.

| Kód chyby | Popis | Akce klienta |
|------------|-------------|---------------|
| invalid_request | Protokol chyb, například chybějící potřeba parametr. | Řešení a opětovné odeslání žádosti. Toto je vývoj chyba je zpravidla zachyceny počáteční testování.|
| unauthorized_client | Klientská aplikace není povoleno s žádostí o povolení kód. | Obvykle k tomu dojde, když klientské aplikace není registrovaný v Azure AD nebo nepřidá uživatele Azure AD klienta. Aplikaci můžete výzva k zadání s pokyny k instalaci aplikace a pak ho přidejte do Azure AD. |
| ACCESS_DENIED | Odepřen souhlas vlastníka zdroje | Klientská aplikace může uživatel, který nelze přejít, pokud uživatel souhlasí upozornění. |
| unsupported_response_type | Povolení server nepodporuje typ odpovědi v pozvánce. | Řešení a opětovné odeslání žádosti. Toto je vývoj chyba je zpravidla zachyceny počáteční testování.|
|server_error | Došlo k neočekávané chybě. | Opakování žádosti. Tyto chyby mohou výsledkem dočasné podmínky. Klientská aplikace může vysvětlit uživatele, že je zpožděných odpovědi z důvodu dočasné chyby. |
| temporarily_unavailable | Server je dočasně přeplněné zpracování požadavku. | Opakování žádosti. Klientská aplikace může vysvětlit uživatele, že je zpožděných odpovědi z důvodu dočasné podmínky. |
| invalid_resource |Cílový prostředek je neplatný, protože není k dispozici, nemůžete ho najít Azure AD nebo není správně nakonfigurované.| To znamená, že zdroje, pokud existuje, není nakonfigurovaný v klientovi. Aplikaci můžete výzva k zadání s pokyny k instalaci aplikace a pak ho přidejte do Azure AD. |

## <a name="validate-the-idtoken"></a>Ověření id_token

Jenom příjem `id_token` nestačí k ověření uživatele. je nutné ověřit podpis a ověřte nároky v `id_token` za požadavky vaše aplikace. Koncový bod Azure AD používá JSON Web tokeny (JWTs) a veřejné klíčové kryptografický podepsat tokeny a ověřte platnost.

Pak můžete ověřit `id_token` v klientovi kód, ale běžné si je odeslat `id_token` do back-end serveru a ověření tam. Jakmile jste ověřit podpis u `id_token`, existuje několik deklarací budete vyzváni k ověření.

Můžete taky další stížnosti podle toho, nefunguje ověření. Některé běžné ověření patří:

- Zajištění uživatele nebo organizace přihlásila k odběru aplikace.
- Zajištění uživatel má správná autorizace a oprávnění
- Zajištění sílu ověřování došlo k chybě, například vícefaktorové ověřování.

Po úplně ověření `id_token`, můžete zahájit relaci s uživatelem a používat nároky v `id_token` získat informace o uživateli v aplikaci. Tyto informace lze použít pro zobrazení záznamy, povolení, atd. Další informace o tokenu typů a pohledávky přečtěte si prosím [podporovaných tokenů a typy deklarace](active-directory-token-and-claims.md).

## <a name="send-a-sign-out-request"></a>Odhlásit se žádost o odeslání

Pokud se chcete odhlásit uživatele z aplikace, nestačí vymažte soubory cookie vaše aplikace nebo jinak ukončení relace s uživatelem.  Taky musíte nastavit přesměrování uživateli `end_session_endpoint` pro Odhlásit se.  Pokud se vám nepovede k tomu nevyzve, budou moct znovu ověřit aplikace bez zadání své přihlašovací údaje znovu, protože budou muset platné přihlašování relaci s koncovým Azure AD uživatele.

Můžete jednoduše přesměrovat uživateli `end_session_endpoint` uvedené v dokumentu OpenID připojení metadat:

```
GET https://login.microsoftonline.com/common/oauth2/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F

```

| Parametr | | Popis |
| ----------------------- | ------------------------------- | ------------ |
| post_logout_redirect_uri | Doporučené | Adresy URL, které má uživatel přesměrovaní na po úspěšném odhlášení.  Pokud ale nezahrnutý uživatele zobrazí zprávu obecný.  |

## <a name="token-acquisition"></a>Pořízení tokenu

Mnoho webových aplikací web apps muset nejen podepsat uživatele systému, ale taky přístup k webové službě jménem uživatele pomocí OAuth. Tento scénář sloučí OpenID připojení pro ověřování uživatelů při souběžné získáním `authorization_code` , které lze použít k získání `access_tokens` použití plovoucích kód se tak mohli ověřovat OAuth.

## <a name="get-access-tokens"></a>Získat přístup tokeny

Získat přístup tokenů, musíte změnit žádost přihlašovací shora:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e      // Your registered Application Id
&response_type=id_token+code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F       // Your registered Redirect Uri, url encoded
&response_mode=form_post                              // form_post', or 'fragment'
&scope=openid
&resource=https%3A%2F%2Fservice.contoso.com%2F                                   
&state=12345                                         // Any value, provided by your app
&nonce=678910                                        // Any value, provided by your app
```

Včetně rozsahů oprávnění v žádosti o a použitím `response_type=code+id_token`, `authorize` koncový bod zajistí, že uživatel souhlasí oprávnění vyznačené `scope` dotazu parametr a vraťte se tak mohli ověřovat kód na exchange pro přístupový token aplikace.

### <a name="successful-response"></a>Úspěšné odpověď

Úspěšné odpovědí pomocí `response_mode=form_post` vypadá podobně jako:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&state=12345
```

| Parametr | Popis |
| ----------------------- | ------------------------------- |
| id_token | `id_token` Požadovanou aplikaci. Můžete použít `id_token` ověřit identitu uživatele a zahájení relace s uživatelem. |
| kód | Authorization_code požadovanou aplikaci. Aplikaci můžete použít kód se tak mohli ověřovat požádat o přístupový token pro cílové zdrojů. Authorization_codes jsou velmi zkrátí, obvykle vyprší jejich platnost po asi 10 minut. |
| Stav | Je-li parametr stát zahrnuta v pozvánce na schůzku, by se měly stejnou hodnotu v odpovědi. Aplikace by měl ověřte, jestli jsou stejné hodnoty stavu v žádostí a odpovědí. |

### <a name="error-response"></a>Chyba odpověď

Chyba odpovědi může posílat taky `redirect_uri` tak aplikaci můžete řádně podporovat zpracování:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parametr | Popis |
| ----------------------- | ------------------------------- |
| Chyba | Řetězec kódu chyby, který slouží ke klasifikaci typy chyb probíhajících a mohou sloužit k reagovat na chyby. |
| error_description | Konkrétní chybové zprávy, které vám můžou pomoct vývojář příčinu kořenové chybu ověřování.  |

Popis kódy chyb a jejich doporučená klienta akce najdete v článku [kódy chyb se tak mohli ověřovat koncový bod chyby](#error-codes-for-authorization-endpoint-errors).

Jakmile jste získali povolení `code` a `id_token`, přihlaste se uživatele a získat tokeny přístup k nim jejich jménem.  Pokud se chcete odhlásit uživatele systému, je třeba ověřit `id_token` přesně tak, jak jsme je popsali výše. Získat přístup tokenů, postupujte podle kroků popsaných v naší [si přečtěte následující dokumentaci protokol OAuth](active-directory-protocols-oauth-code.md#Use-the-Authorization-Code-to-Request-an-Access-Token).
