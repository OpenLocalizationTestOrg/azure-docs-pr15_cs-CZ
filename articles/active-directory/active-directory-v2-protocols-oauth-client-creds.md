
<properties
    pageTitle="Azure AD verze 2.0 OAuth klienta pověření toku | Microsoft Azure"
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
    ms.date="09/26/2016"
    ms.author="dastrock"/>

# <a name="v20-protocols---oauth-20-client-credentials-flow"></a>Protokoly verze 2.0 – pověření 2.0 klienta OAuth tok

[Udělit pověření klienta OAuth 2.0](http://tools.ietf.org/html/rfc6749#section-4.4), někdy označovaný taky jako "s rameny dvou OAuth", mohou sloužit k přístupu k prostředkům web hostovaný pomocí identity aplikace.  Obvykle se používá pro serverů interakcí, které musí běhu na pozadí bez okamžité precense koncovým uživatelem.  Tyto aplikace jsou někdy označovány jako **procesy daemon** nebo **účtů služeb**.

> [AZURE.NOTE]
    Některé scénáře Azure Active Directory a funkce jsou podporovány koncový bod verze 2.0.  Pokud chcete zjistit, zda by měly používat koncový bod verze 2.0, přečtěte si informace o [omezení verze 2.0](active-directory-v2-limitations.md).

V více typické tři "s rameny tři OAuth," klientské aplikace je oprávnění pro přístup k prostředku jménem určitému uživateli.  Oprávnění je **Delegovat** uživateli aplikace obvykle během [souhlas](active-directory-v2-scopes.md) .  Však v toku pověření klienta oprávnění jsou udělena **přímo** do této aplikace.  Když aplikaci představuje token zdroji, vynutí zdroje samotnou aplikaci má oprávnění k provedení určité akce – Ne některé má uživatel, který se tak mohli ověřovat.

## <a name="protocol-diagram"></a>Diagram Protocol (protokol)
Tok pověření celého klienta vypadá přibližně takto: všechny kroky se píše níže.

![Tok pověření klienta](../media/active-directory-v2-flows/convergence_scenarios_client_creds.png)

## <a name="get-direct-authorization"></a>Přímé ověření 
Aplikace obvykle přijímá přímé autorizaci pro přístup k prostředku dvěma způsoby: pomocí seznamu řízení přístupu k prostředku nebo prostřednictvím přiřazení oprávnění aplikace v Azure AD.  Existuje několik způsobů, které zdroje můžete povolit klientů a jednotlivých prostředků serveru může zvolte metodu, která je nejsmysluplnější pro jeho používání.  Tyto dva způsoby jsou nejčastěji používané v Azure AD a reccommended pro zařízení a zdroje informací, které chcete provést tok pověření klienta.

### <a name="access-control-lists"></a>Seznamy řízení přístupu
Zprostředkovatele daný zdroj může vynutit kontrolu autorizace založené na seznamu ID aplikace, že zná a povolí některé určitou úroveň přístupu.  Přijaté zdroje token z verze 2.0 koncového bodu, může dekódovat tokenu nebo extrahovat klienta ID aplikace z `appid` a `iss` deklarované.  Pak můžete porovnat, že aplikace proti některé seznam řízení přístupu (ACL) zachová.  Rozlišení a způsob jejich seznamu přístupových práv se může lišit výrazně ze zdroje na zdroje.

Běžné případu použití těchto seznamů ACL je testování závodníky pro webovou aplikaci nebo webového rozhraní api.  Webu rozhraní api mohou jenom poskytnout podmnožinu jeho oprávnění k úplnému různých klientů.  Chcete-li spustit testů konce na rozhraní api, Testovací klient vytvořený, ale získá tokenů z verze 2.0 koncového bodu a odešle rozhraní API.  Rozhraní api, můžete ACL ID aplikace testovací klient pro plný přístup k rozhraní api celý funkce.  Všimněte si, že pokud máte tento seznam na službu, měli byste být potřeba nejen ověřit volajícího `appid`, ale taky ověřit, zda `iss` tokenu je důvěryhodná stejně.

Tento typ ověřování je běžné procesy daemon a účtů služeb, které potřebují dostat k datům vlastněná spotř uživatele s osobní účty Microsoft.  Data vlastněná organizacím se doporučuje získat potřebné ověření prostřednictvím perimssions aplikace.

### <a name="application-permissions"></a>Oprávnění aplikace
Nepoužívejte ACL rozhraní API vystavit sadu **oprávnění aplikace** poskytovaná aplikace.  Aplikace oprávnění, která aplikace správce organizace a lze použít pouze pro přístup k datům vlastněná organizace a jeho zaměstnanci.  Například Microsoft Graph poskytuje několik oprávnění aplikace:

- Čtení pošty v všechny poštovní schránky
- Čtení a zápis pošta v všechny poštovní schránky
- Odeslání e-mailu jako všichni uživatelé
- Čtení dat adresáře
- [+ Další](https://graph.microsoft.io)

Pokud chcete získat tato oprávnění v aplikaci, můžete proveďte následující kroky.

#### <a name="request-the-permissions-in-the-app-registration-portal"></a>Žádosti o oprávnění v portálu registrace aplikace

- Pokud jste to ještě neudělali, přejděte do aplikace v [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)nebo [Vytvoření aplikace pro](active-directory-v2-app-registration.md) .  Bude nutné ověřit, že aplikace vytvořil alespoň jeden tajná aplikace.
- Vyhledejte část **Přímé oprávnění aplikace** a přidejte oprávnění, která vyžaduje aplikaci.
- Nejdřív musíte mít **Uložit** registrace aplikace

#### <a name="recommended-sign-the-user-into-your-app"></a>Doporučené: uživatel přihlásit k aplikaci aplikace

Obvykle při vytváření aplikace, které používá oprávnění aplikace, aplikace bude nutné stránky nebo zobrazení, které umožňuje správu ke schválení oprávnění aplikace.  Na této stránce může být součástí aplikace na registrace toku, část nastavení v aplikaci nebo vyhrazené toku "připojit".  V mnoha případech dává smysl aplikace zobrazovat "" zobrazení pouze po připojit uživatele má přihlášení pomocí pracovního nebo školního účtu Microsoft.

Po přihlášení uživatele k aplikaci umožňuje určit organziation, ke kterému patří uživatele před s žádostí o schválení oprávnění aplikace.  Když není nezbytných ho můžete vám pomůže s vytvářením intuitivnější prostředí pro organizační uživatele.  Pokud se chcete odhlásit uživatele systému, postupujte podle naše [verze 2.0 protokol výukové programy pro](active-directory-v2-protocols.md).

#### <a name="request-the-permissions-from-a-directory-admin"></a>Požádat oprávnění správce adresáře

Jakmile budete připraveni k žádosti o oprávnění od společnosti správce, můžete je přesměrovávat uživatele na verze 2.0 **Správce souhlas koncového bodu**.

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/adminconsent?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&state=12345
&redirect_uri=http://localhost/myapp/permissions
```

```
// Pro Tip: Try pasting the below request in a browser!
```

```
https://login.microsoftonline.com/common/adminconsent?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&state=12345&redirect_uri=http://localhost/myapp/permissions
```

| Parametr | | Popis |
| ----------------------- | ------------------------------- | --------------- |
| klienta | povinné | Adresář klienta, který chcete požádat o souhlas.  Lze zadat ve formátu popisný název nebo guid.  Pokud nechcete vědět, které uživatel patří klienta a chcete, aby je Přihlaste se pomocí libovolné klienta, použijte k `common`. |
| client_id | povinné | Id aplikace portál registrace ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) přiřazené aplikace. |
| redirect_uri | povinné | Redirect_uri místo, kam chcete odpověď k odeslání aplikace pro zpracování.  To se musí přesně shodovat jednu redirect_uris zapsaný na portálu kromě musí být překódován jako adresa url a může mít segmenty další cesty. |
| Stav | Doporučené | Hodnota součástí žádosti o, bude vrácena také v tokenu odpovědi.  Může být řetězec veškerý obsah, který chcete.  Stav slouží ke kódování informace o stavu uživatele v aplikaci před požadavek na ověření došlo k chybě, například stránky nebo zobrazení, které byly na. |

V tomto okamžiku Azure AD bude jejímu vynucení, že jenom správce klienta se přihlásit k dokončení zadání.  Správce bude požádán, aby všechna oprávnění přímé aplikace, které požadujete pro vaši aplikaci distribuovali portálu registrace schválení. 

##### <a name="successful-response"></a>Úspěšné odpověď
Pokud správce schválí oprávnění pro aplikaci, budou úspěšné odpověď:

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Parametr | Popis |
| ----------------------- | ------------------------------- | --------------- |
| klienta | Adresář klienta, který příslušná oprávnění, která byla požadována ve formátu guid aplikace. |
| Stav | Hodnota součástí požadavek, který bude vrácena také v tokenu odpovědi.  Může být řetězec veškerý obsah, který chcete.  Stav slouží ke kódování informace o stavu uživatele v aplikaci před požadavek na ověření došlo k chybě, například stránky nebo zobrazení, které byly na. |
| admin_consent | Nastaví se `True`. |


##### <a name="error-response"></a>Chyba odpověď
Pokud správce není schválit oprávnění pro aplikaci, budou nezdařeném uložení odpověď:

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Parametr | Popis |
| ----------------------- | ------------------------------- | --------------- |
| Chyba | Řetězec kódu chyby, který slouží ke klasifikaci typy chyb probíhajících a mohou sloužit k reagovat na chyby. |
| error_description | Konkrétní chybové zprávy, které vám můžou pomoct vývojář příčinu kořenové chybu.  |

Jednou z aplikace zřizování koncový bod byla přijata odpověď úspěšné, aplikace získal oprávnění přímé aplikace, která byla požadována.  Teď můžete přesunout na vyžádání token pro požadovaný zdroj.

## <a name="get-a-token"></a>Získání tokenu
Jakmile jste získali potřebná oprávnění pro aplikaci, můžete přejít se získáním tokeny přístup k rozhraní API.  Získat token pomocí klienta udělit přihlašovací údaje, pošlete žádost příspěvek `/token` verze 2.0 koncového bodu:

```
POST /common/oauth2/v2.0/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=535fb089-9ff3-47b6-9bfb-4f1264799865&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_secret=qWgdYAmab0YSkuL1qKv5bPX&grant_type=client_credentials
```

```
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d 'client_id=535fb089-9ff3-47b6-9bfb-4f1264799865&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_secret=qWgdYAmab0YSkuL1qKv5bPX&grant_type=client_credentials' 'https://login.microsoftonline.com/common/oauth2/v2.0/token'
```

| Parametr | | Popis |
| ----------------------- | ------------------------------- | --------------- |
| client_id | povinné | Id aplikace portál registrace ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) přiřazené aplikace. |
| rozsah | povinné | Předaná hodnota `scope` parametr v této pozvánce by měl být identifikátor zdrojů (aplikace Identifikátor URI) požadované zdroje označeny `.default` přípona.  Tak například Microsoft Graph zadaných hodnota musí být `https://graph.microsoft.com/.default`.  Tato hodnota informuje koncový bod verze 2.0, že všechny přímé aplikace oprávnění, které jste nakonfigurovali aplikace, ho k vydávání token pro pravidla týkající se požadované zdroje. |
| client_secret | povinné | Tajná aplikace, které vygenerovalo na portálu registrace aplikace. |
| grant_type | povinné | Musí být `client_credentials`. | 

#### <a name="successful-response"></a>Úspěšné odpověď
Odpověď na úspěšné bude trvat formuláře:

```
{
  "token_type": "Bearer",
  "expires_in": 3599,
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBP..."
}
```

| Parametr | Popis |
| ----------------------- | ------------------------------- |
| access_token | Požadovaná přístupový token. Aplikaci můžete použít tento token ověření zabezpečeným prostředku, například webového rozhraní API. |
| token_type | Určuje typ tokenu hodnotu. Pouze typ, který je Azure AD podporuje `Bearer`.  |
| expires_in | Jak dlouho přístupový token je platný (v sekundách). |

#### <a name="error-response"></a>Chyba odpověď
Odpověď s chybou bude trvat formuláře:

```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/.default is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
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

## <a name="use-a-token"></a>Použití token
Teď jste získali token, můžete tento token tak, abyste požadavků zdroje.  Když skončí platnost tokenu, jednoduše opakujte žádost `/token` koncový bod získat svěže přístupový token.

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

```
// Pro Tip: Try the below command out! (but replace the token with your own)
```

```
curl -X GET -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q" 'https://graph.microsoft.com/v1.0/me/messages'
```

## <a name="code-sample"></a>Ukázka kódu
Příklad aplikace implementuje, která client_credentials udělit pomocí Správce souhlas koncový bod zobrazíte v nápovědě k našimi [Ukázka kódu démon verze 2.0](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).
