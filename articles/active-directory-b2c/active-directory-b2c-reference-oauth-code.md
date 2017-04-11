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

# <a name="azure-active-directory-b2c-oauth-20-authorization-code-flow"></a>Azure Active Directory B2C: OAuth 2.0 se tak mohli ověřovat kód toku

Udělení OAuth 2.0 se tak mohli ověřovat kód lze použít v aplikacích, které jsou instalovány na zařízení k získání přístupu k chráněné zdroje, jako je třeba webového rozhraní API. Pomocí služby Azure Active Directory (Azure AD) B2C provádění OAuth 2.0 můžete přidat úkoly správy identit registrace, přihlásit a dalších ke svým aplikacím přenosných a stolních. Tato příručka je závislý na jazyce. Popisuje způsob odesílání a přijímání zpráv HTTP bez použití libovolného příkazu pro naše knihoven otevřít zdroj.

<!-- TODO: Need link to libraries -->

Tok kód se tak mohli ověřovat OAuth 2.0 je popsaná v [části 4.1 specifikace OAuth 2.0](http://tools.ietf.org/html/rfc6749). Můžete ji použijete k provedení a tak mohli ověřovat ve většině aplikace typů, včetně [webových aplikací web apps](active-directory-b2c-apps.md#web-apps) a [nativně nainstalované aplikace](active-directory-b2c-apps.md#mobile-and-native-apps). Umožňuje aplikace bezpečně získat **access_tokens**, které lze použít pro přístup k prostředky, které jsou správně zapojené [se tak mohli ověřovat serveru](active-directory-b2c-reference-protocols.md#the-basics).

Tento průvodce se zaměřuje na konkrétní charakter OAuth 2.0 se tak mohli ověřovat kód toku –**veřejných klientů**. Veřejné klient je klientské aplikace, který není důvěryhodný bezpečně k zachování integrity heslo. Jedná se o mobilních aplikací, aplikace klasické pracovní plochy a poměrně mnohem libovolné aplikaci, která běží na zařízení a musí získat access_tokens. Pokud chcete přidat Správa identit do webové aplikace pomocí Azure AD B2C, používejte [OpenID připojení](active-directory-b2c-reference-oidc.md) , nikoli OAuth 2.0.

Azure AD B2C rozšiřuje standardní OAuth 2.0 přecházel více než jednoduchý a tak mohli ověřovat. Uvádí [**zásad parametr**](active-directory-b2c-reference-policies.md), který vám umožní použít OAuth 2.0 uživatelského prostředí přidáte aplikace, jako například registrace, přihlásit a Správa profilů. Tady ukážeme, jak používat OAuth 2.0 a zásady každý z těchto prostředí provádět v aplikacích nativní. Také ukážeme, jak získat access_tokens pro přístup k webu rozhraní API.

Požadavky HTTP příkladu níže použije našeho adresáře B2C vzorku, **fabrikamb2c.onmicrosoft.com**i naše ukázková aplikace a zásady. Které můžete zkusit požadavky se sami s využitím těchto hodnot zadarmo nebo je můžete nahradit svým vlastním.
Zjistěte, jak [získat vlastní B2C adresáře, aplikace a zásady](#use-your-own-b2c-directory).

## <a name="1-get-an-authorization-code"></a>1. získat kód se tak mohli ověřovat
Tok kód se tak mohli ověřovat začíná klienta odkazující uživatele, aby `/authorize` koncového bodu. Sem zadejte tu část interaktivní toku, kdy uživatel skutečně provede akci. V této žádosti klienta označuje oprávnění, která potřebuje získat od uživatele v `scope` parametry a zásady provést v `p` parametr. Tři příklady jsou uvedeny níže (plus pár konce řádků pro snazší čitelnost), každý pomocí různých zásad.

#### <a name="use-a-sign-in-policy"></a>Použití zásad přihlášení

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_in
```

#### <a name="use-a-sign-up-policy"></a>Pomocí zápisu zásad

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_up
```

#### <a name="use-an-edit-profile-policy"></a>Použití zásad upravit profil

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_edit_profile
```

| Parametr | Povinné? | Popis |
| ----------------------- | ------------------------------- | ----------------------- |
| client_id | Povinné | ID aplikace přiřazené [Azure portál](https://portal.azure.com) aplikace. |
| response_type | Povinné | Typ odpovědi musí obsahovat `code` toku kód ověření. |
| redirect_uri | Povinné | Redirect_uri aplikace, kde můžete ověřování odpovědi odešlete nebo přijmete tak, že aplikace. Se musí přesně shodovat jednu redirect_uris, kterou jste registrovali na portálu s tím rozdílem, že musí být překódován jako adresa URL. |
| rozsah | Povinné | Seznam oborů oddělený mezerami. Hodnota jeden obor označuje Azure AD obou oprávnění, které jsou požadovány. Použití ID klienta jako obor označuje, že aplikace vyžaduje **tokenu přístupu** , které lze použít vlastní služby nebo webového rozhraní API, představované stejné ID klienta.  `offline_access` Obor označuje, že aplikace, musí **refresh_token** dlouhodobé přístupu pro zdroje.  Můžete taky použít `openid` obor požádat o **id_token** z Azure AD B2C. |
| response_mode | Doporučené | Metoda použitý k odeslání výsledné authorization_code aplikace. Může být jeden z "dotazu", "form_post" nebo "neúplné".
| Stav | Doporučené | Hodnota součástí požadavek, který bude vrácena také v tokenu odpovědi. Může být řetězec veškerý obsah, který chcete. Náhodně generovaným jedinečnou hodnotu se obvykle používá pro zabránění útoky padělání žádost webů. Stav je také použít ke kódování informace o stavu uživatele v aplikaci před požadavek na ověření došlo k chybě, například stránky, které byly na nebo této zásadě prováděný. |
| p | Povinné | Zásady, který bude spuštěn. Název zásady vytvořenou v adresáři B2C je. Hodnotu zásad název musí začínat "b2c\_1\_". Další informace o zásady v [rámci Extensible zásad](active-directory-b2c-reference-policies.md). |
| dotaz | Volitelné | Typ interakce s uživatelem, který je potřebný. Platný pouze v současné době hodnotu "přihlášení", který vynucuje uživateli zadejte své přihlašovací údaje na žádosti. Jednotné přihlašování neprojeví. |

V tomto okamžiku uživateli se zobrazí výzva k dokončení pracovního postupu na zásadu. To může zahrnovat uživatele při zadávání svoje uživatelské jméno a heslo, přihlášení pomocí sociální identity registruje k adresáři nebo jiné číslo kroků podle toho, jak je definován zásadu.

Po dokončení zásady uživatele Azure AD vrátí odpověď aplikace v určené `redirect_uri`, metodou podle `response_mode` parametr. Odpověď na bude přesně stejné pro každou z výše uvedených případech nezávislé zásad, které byl spuštěn.

Úspěšné odpověď, která používá `response_mode=query` vypadá podobně jako:

```
GET urn:ietf:wg:oauth:2.0:oob?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...        // the authorization_code, truncated
&state=arbitrary_data_you_can_receive_in_the_response                // the value provided in the request
```

| Parametr | Popis |
| ----------------------- | ------------------------------- |
| kód | Authorization_code požadovanou aplikaci. Aplikaci můžete použít kód se tak mohli ověřovat požádat o access_token pro cílové zdrojů. Authorization_codes jsou velmi zkrátí. Obvykle vyprší jejich platnost po asi 10 minut. |
| Stav | Zobrazit celou popisu v předchozí tabulce. Je-li parametr stát zahrnuta v pozvánce na schůzku, by se měly stejnou hodnotu v odpovědi. Aplikace by měl ověřte, jestli jsou stejné hodnoty stavu v žádosti a odpověď. |

Chyba odpovědi může posílat taky `redirect_uri` tak, aby aplikace můžete řádně podporovat zpracování:

```
GET urn:ietf:wg:oauth:2.0:oob?
error=access_denied
&error_description=The+user+has+cancelled+entering+self-asserted+information
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parametr | Popis |
| ----------------------- | ------------------------------- |
| Chyba | Řetězec kódu chyby, který slouží ke klasifikaci typy chyb probíhajících a mohou sloužit k reagovat na chyby. |
| error_description | Konkrétní chybovou zprávu, které vám můžou pomoct vývojář k identifikaci příčina chyby ověření. |
| Stav | Zobrazit úplný popis z první tabulky v této části. Je-li parametr stát zahrnuta v pozvánce na schůzku, by se měly stejnou hodnotu v odpovědi. Aplikace by měl ověřte, jestli jsou stejné hodnoty stavu v žádosti a odpověď. |


## <a name="2-get-a-token"></a>2. získání tokenu
Teď jste získali authorization_code, můžete uplatnit `code` token požadovanou zdroji tak, že `POST` požádat o `/token` koncový bod. V Azure AD B2C pouze prostředek, který můžete požádat o token pro je vaše aplikace vlastní back-end webového rozhraní API. Konvence, která se používá žádosti o token sami sobě, je použít ID klienta vaše aplikace jako obor:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob

```

| Parametr | Povinné? | Popis |
| ----------------------- | ------------------------------- | --------------------- |
| p | Povinné | Zásad, která byla použita k získání kódu se tak mohli ověřovat. Jiné zásady nelze použít v této pozvánce. Všimněte si přidat tento parametr s *řetězci dotazu*, není v těle příspěvek. |
| client_id | Povinné | ID aplikace přiřazené [Azure portál](https://portal.azure.com) aplikace. |
| grant_type | Povinné | Typ grant (udělit), která musí být `authorization_code` toku kód ověření. |
| rozsah | Reccommended | Seznam oborů oddělený mezerami. Hodnota jeden obor označuje Azure AD obou oprávnění, které jsou požadovány. Použití ID klienta jako obor označuje, že aplikace vyžaduje **tokenu přístupu** , které lze použít vlastní služby nebo webového rozhraní API, představované stejné ID klienta.  `offline_access` Obor označuje, že aplikace, musí **refresh_token** dlouhodobé přístupu pro zdroje.  Můžete taky použít `openid` obor požádat o **id_token** z Azure AD B2C. |
| kód | Povinné | Authorization_code, který jste získali v první nohy toku. |
| redirect_uri | Povinné | Redirect_uri aplikace, kterou jste dostali authorization_code. |

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
| not_before | Čas, pro niž je tokenu považovány za platné v čase epocha. |
| token_type | Tokenu typ hodnoty. Pouze typ, který podporuje Azure AD je nosný. |
| access_token | Podepsané JSON Web tokenu (JWT) tokenu požadovanému. |
| rozsah | Oborů, které tokenu platí pro, která se dá použít pro ukládání do mezipaměti tokeny pro pozdější použití. |
| expires_in | Časový úsek, která platí tokenu (v sekundách). |
| refresh_token | Refresh_token OAuth 2.0. Aplikaci můžete použít tento token získat další tokeny po vypršení platnosti aktuálního tokenu. Refresh_tokens je dlouhodobě a mohou sloužit k uchovávat přístupu k prostředkům dlouhou dobu. Další podrobnosti najdete [B2C tokenu odkaz](active-directory-b2c-reference-tokens.md). |

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
| error_description | Konkrétní chybovou zprávu, které vám můžou pomoct vývojář k identifikaci příčina chyby ověření. |

## <a name="3-use-the-token"></a>3. pomocí tokenu
Teď jste získali úspěšně `access_token`, můžete tokenu v žádosti o vaší back-end webu rozhraní API s využitím v `Authorization` záhlaví:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="4-refresh-the-token"></a>4. aktualizace tokenu
Tokeny přístupu a Id jsou zkrátí. Je třeba aktualizovat po vypršení jejich platnosti pokračovat, nebudete mít přístup k zdroje. Můžete to udělat tak, že druhý odeslání `POST` požádat o `/token` koncový bod. Poskytovat tuto chvíli `refresh_token` místo `code`:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob
```

| Parametr | Povinné? | Popis |
| ----------------------- | ------------------------------- | -------- |
| p | Povinné | Zásady, která byla použita k získání původní refresh_token. Jiné zásady nelze použít v této pozvánce. Všimněte si přidat tento parametr s *řetězci dotazu*, není v těle příspěvek. |
| client_id | Doporučené | ID aplikace přiřazené [Azure portál](https://portal.azure.com) aplikace. |
| grant_type | Povinné | Typ grant (udělit), která musí být `refresh_token` pro tento nohy směrování kód ověření. |
| rozsah | Doporučené | Seznam oborů oddělený mezerami. Hodnota jeden obor označuje Azure AD obou oprávnění, které jsou požadovány. Použití ID klienta jako obor označuje, že aplikace vyžaduje **tokenu přístupu** , které lze použít vlastní služby nebo webového rozhraní API, představované stejné ID klienta.  `offline_access` Obor označuje, že aplikace, musí **refresh_token** dlouhodobé přístupu pro zdroje.  Můžete taky použít `openid` obor požádat o **id_token** z Azure AD B2C. |
| redirect_uri | Volitelné | Redirect_uri aplikace, kterou jste dostali authorization_code. |
| refresh_token | Povinné | Původní refresh_token, který jste získali v druhém nohy toku. |

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
| not_before | Čas, pro niž je tokenu považovány za platné v čase epocha. |
| token_type | Tokenu typ hodnoty. Pouze typ, který podporuje Azure AD je nosný. |
| access_token | Podepsané JSON Web tokenu (JWT) tokenu požadovanému. |
| rozsah | Oborů, které tokenu platí pro, která se dá použít pro ukládání do mezipaměti tokeny pro pozdější použití. |
| expires_in | Časový úsek, která platí tokenu (v sekundách). |
| refresh_token | Refresh_token OAuth 2.0. Aplikaci můžete použít tento token získat další tokeny po vypršení platnosti aktuálního tokenu. Refresh_tokens je dlouhodobě a mohou sloužit k uchovávat přístupu k prostředkům dlouhou dobu. Další podrobnosti najdete [B2C tokenu odkaz](active-directory-b2c-reference-tokens.md). |

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

## <a name="use-your-own-b2c-directory"></a>Použití adresáře B2C

Pokud chcete, můžete vyzkoušet tyto požadavky pro sebe, musíte nejdřív provádět tyto tři kroky a nahradit příklad hodnoty nad vlastního:

- [Vytvoření B2C adresář](active-directory-b2c-get-started.md) a použití názvu vašeho adresáře v požadavcích.
- [Vytvoření aplikace](active-directory-b2c-app-registration.md) pro získání ID aplikace a redirect_uri. Můžete zahrnout **native client –** aplikace.
- [Vytvoření pravidla](active-directory-b2c-reference-policies.md) získat zásad názvy.
