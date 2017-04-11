<properties
    pageTitle="Přehled protokolu .NET Azure AD | Microsoft Azure"
    description="Tento článek popisuje, jak povolit přístup k webovým aplikacím a webového rozhraní API ve vašem klientovi pomocí služby Azure Active Directory a OAuth 2.0 pomocí protokolu HTTP zpráv."
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


# <a name="authorize-access-to-web-applications-using-oauth-20-and-azure-active-directory"></a>Povolit přístup k webové aplikace pomocí OAuth 2.0 a Azure Active Directory

Azure Active Directory (Azure AD) použití OAuth 2.0 umožňující povolit přístup k webovým aplikacím a webového rozhraní API ve vašem klientovi Azure AD. Tato příručka je nezávislý jazyk a popisuje, jak k odesílání a přijímání zpráv HTTP bez použití libovolného příkazu pro naše knihoven otevřít zdroj.

Tok kód se tak mohli ověřovat OAuth 2.0 je popsaná v [části 4.1 specifikace OAuth 2.0](https://tools.ietf.org/html/rfc6749#section-4.1) . Slouží k provádění a tak mohli ověřovat ve většině aplikace typů, včetně webových aplikací web apps a nativně nainstalované aplikace.

[AZURE.INCLUDE [active-directory-protocols-getting-started](../../includes/active-directory-protocols-getting-started.md)]


## <a name="oauth-20-authorization-flow"></a>Tok se tak mohli ověřovat OAuth 2.0

Na vysoké úrovni tok celý povolení aplikace trochu vypadá nějak takto:

![Tok OAuth ověřovací kód](media/active-directory-protocols-oauth-code/active-directory-oauth-code-flow-native-app.png)


## <a name="request-an-authorization-code"></a>Žádost o povolení kód

Tok kód se tak mohli ověřovat začíná klienta odkazující uživatele, aby `/authorize` koncového bodu. V této žádosti klienta označuje oprávnění, která potřebuje získat od uživatele. Na stránce vaše aplikace klasické portálu Azure, na tlačítko **Zobrazit koncové body** v dolní okraj můžete získat koncové body OAuth 2.0.

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&resource=https%3A%2F%2Fservice.contoso.com%2F
&state=12345
```

| Parametr | | Popis |
| ----------------------- | ------------------------------- | --------------- |
| klienta | povinné | `{tenant}` Hodnoty v parametru path žádosti o lze určit, kdo může přihlásit k aplikaci.  Povolené hodnoty jsou klienta, například `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` nebo `contoso.onmicrosoft.com` nebo `common` tokeny nezávislé na klienta |
| client_id | povinné | Id aplikace přiřazené k aplikaci při registraci s Azure AD. Můžete najít v portálu pro správu Azure. Klepněte na **Služby Active Directory**, klikněte na adresář, klepněte na aplikace a potom klikněte na **Konfigurovat** |
| response_type | povinné | Musí obsahovat `code` toku kód ověření. |
| redirect_uri | Doporučené | Redirect_uri aplikace, kde můžete ověřování odpovědi odešlete nebo přijmete tak, že aplikace.  To se musí přesně shodovat jednu redirect_uris zapsaný na portálu kromě musí být překódován jako adresa url.  Pro nativní a mobilní aplikace, byste měli použít výchozí hodnoty `urn:ietf:wg:oauth:2.0:oob`. |
| response_mode | Doporučené | Určuje metodu, která má být použit k odeslání výsledné tokenu zpět do aplikace.  Může být `query` nebo `form_post`.  |
| Stav | Doporučené | Hodnota součástí požadavek, který bude vrácena také v tokenu odpovědi. Náhodně generovaným jedinečnou hodnotu se obvykle používá pro [zabránění útoků padělání žádost webů](http://tools.ietf.org/html/rfc6749#section-10.12).  Stav je také použít ke kódování informace o stavu uživatele v aplikaci před požadavek na ověření došlo k chybě, například stránky nebo zobrazení, které byly na. |
| zdroje | volitelné | Identifikátor URI aplikace ID webového rozhraní API (zabezpečené zdroje). Identifikátor URI ID aplikace webového rozhraní API, na portálu Správa Azure najdete klikněte **Služby Active Directory**, klikněte na adresář, klikněte na aplikace a potom klikněte na **Konfigurovat**. |
| dotaz | volitelné |  Označuje typ interakce s uživatelem, který je potřebný.<p> Platné hodnoty jsou: <p> *přihlášení*: uživatel má vyzváni k ověření znovu. <p> *svolení*: souhlasu uživatele byl udělen, ale je třeba aktualizovat. Uživatel by se výzva k souhlas. <p> *admin_consent*: Správce by měl vyzváni k souhlasu jménem všem uživatelům v organizaci |
| login_hint | volitelné | Slouží k předem zadání pole uživatelské jméno nebo e-mailovou adresu obrazovky přihlašovací stránky pro uživatele, pokud znáte svoje uživatelské jméno předem.  Často aplikace použijete tento parametr během nové ověřování, máte už extrahovaných uživatelské jméno z předchozí přihlášení pomocí `preferred_username` deklarace. |
| domain_hint | volitelné | Poskytuje informace o klienta nebo domény, který uživatel by měl použít k přihlášení. Domain_hint hodnotu registrované domény pro klienta. Pokud klienta je federované do místního adresáře, přesměruje AAD federačního serveru zadaný klienta. |

> [AZURE.NOTE] Pokud je uživatel součástí organizace, správce organizace můžete souhlas odmítnutí jménem uživatele nebo povolení uživatelského souhlas. Uživateli se zobrazí možnost souhlas pouze v případě, že ji povolí správce.

V tomto okamžiku uživatel bude požádán, zadejte své přihlašovací údaje a souhlas s oprávnění vyznačené `scope` dotazu parametr. Jakmile je uživatel ověří a uděluje souhlas, Azure AD odeslána zpráva aplikace na `redirect_uri` adresu v pozvánce.

### <a name="successful-response"></a>Úspěšné odpověď

Odpověď na úspěšné může vypadat takto:

```
GET  HTTP/1.1 302 Found
Location: http://localhost/myapp/?code= AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA&session_state=7B29111D-C220-4263-99AB-6F6E135D75EF&state=D79E5777-702E-4260-9A62-37F75FF22CCE
```

| Parametr | Popis |
| -----------------------| --------------- |
| admin_consent | Hodnota je PRAVDA, pokud správce souhlas výzva žádost o souhlas.|
| kód | Povolení kód požadovanou aplikaci. Aplikaci můžete použít kód se tak mohli ověřovat požádat o přístupový token pro cílové zdrojů. |
| session_state | Jedinečná hodnota, která určuje relaci aktuálního uživatele. Tato hodnota je GUID, ale měly považovat za neprůhledné hodnotu, které je bez zkoumání. |
| Stav | Je-li parametr stát zahrnuta v pozvánce na schůzku, by se měly stejnou hodnotu v odpovědi. Je vhodné pro aplikaci ověřte hodnoty stavu v žádostí a odpovědí identickými před použitím odpověď. Díky ke zjištění [útoků webů požádat o padělání (CSRF)](https://tools.ietf.org/html/rfc6749#section-10.12) proti klienta.  

### <a name="error-response"></a>Chyba odpověď

Chyba odpovědi může posílat taky `redirect_uri` tak, aby aplikace můžete provede řádně podporovat.

```
GET http://localhost:12345/?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parametr | Popis |
|-----------|-------------|
| Chyba | Kód chybové hodnoty v části 5,2 [OAuth 2.0 se tak mohli ověřovat Framework](http://tools.ietf.org/html/rfc6749)definované. Následující tabulka popisuje kódy chyb, které vrátí Azure AD. |
| error_description | Podrobnější popis chyby do schránky. Tato zpráva není určená k být popisný koncových uživatelů. |
| Stav | Hodnoty stav je náhodně generovaným-opakovaně použít hodnotu, která je Odeslaná pošta v žádosti a vrácená v odpovědi na útokům webů žádost padělání (CSRF). |

#### <a name="error-codes-for-authorization-endpoint-errors"></a>Kódy chyb se tak mohli ověřovat koncový bod chyby

Následující tabulka popisuje jednotlivé kódy chyb, které může být vrácen v `error` parametr odpovědi na chyby.

| Kód chyby | Popis | Akce klienta |
|------------|-------------|---------------|
| invalid_request | Protocol (protokol) došlo k chybě, například chybějící potřeba parametr. | Řešení a opětovné odeslání žádosti. Toto je vývoj chyba je zpravidla zachyceny během počáteční testování.|
| unauthorized_client | Klientská aplikace není povoleno s žádostí o povolení kód. | Obvykle k tomu dojde, když klientské aplikace není registrovaný v Azure AD nebo nepřidá uživatele Azure AD klienta. Aplikaci můžete výzva k zadání s pokyny k instalaci aplikace a pak ho přidejte do Azure AD. |
| ACCESS_DENIED | Odepřen souhlas vlastníka zdroje | Klientská aplikace může uživatel, který nelze přejít, pokud uživatel souhlasí upozornění. |
| unsupported_response_type | Povolení server nepodporuje typ odpovědi v pozvánce. | Řešení a opětovné odeslání žádosti. Toto je vývoj chyba je zpravidla zachyceny během počáteční testování.|
|server_error | Došlo k neočekávané chybě. | Opakování žádosti. Tyto chyby mohou výsledkem dočasné podmínky. Klientská aplikace může vysvětlit uživatele, že je zpožděných odpovědi z důvodu dočasné chyby. |
| temporarily_unavailable | Server je dočasně přeplněné zpracování požadavku. | Opakování žádosti. Klientská aplikace může vysvětlit uživatele, že je zpožděných odpovědi z důvodu dočasné podmínky. |
| invalid_resource |Cílový prostředek je neplatný, protože není k dispozici, nemůžete ho najít Azure AD nebo není správně nakonfigurované.| To znamená, že zdroje, pokud existuje, není nakonfigurovaný v klientovi. Aplikaci můžete výzva k zadání s pokyny k instalaci aplikace a pak ho přidejte do Azure AD. |

## <a name="use-the-authorization-code-to-request-an-access-token"></a>Použití kódu se tak mohli ověřovat požádat o přístupový token

Teď, když jste získali kódem se tak mohli ověřovat a máte přidělená oprávnění uživatelem, můžete uplatnit kód přístupový token požadované zdroji odesláním žádosti o s příspěvek `/token` koncového bodu:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded
grant_type=authorization_code
&client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA
&redirect_uri=https%3A%2F%2Flocalhost%2Fmyapp%2F
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=p@ssw0rd

//NOTE: client_secret only required for web apps
```

| Parametr | | Popis |
| ----------------------- | ------------------------------- | --------------------- |
| klienta | povinné |  `{tenant}` Hodnoty v parametru path žádosti o lze určit, kdo může přihlásit k aplikaci.  Povolené hodnoty jsou klienta, například `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` nebo `contoso.onmicrosoft.com` nebo `common` tokeny nezávislé na klienta |
| client_id | povinné | Id aplikace přiřazené k aplikaci při registraci s Azure AD. Můžete najít v portálu klasické Azure. Klepněte na **Služby Active Directory**, klikněte na adresář, klepněte na aplikace a potom klikněte na **Konfigurovat** |
| grant_type | povinné | Musí být `authorization_code` toku kód ověření. |
| kód | povinné | `authorization_code` , Který jste získali v předchozí části   |
| redirect_uri | povinné | Stejný `redirect_uri` hodnotu, která byla použita k získání `authorization_code`. |
| client_secret | požadovaný pro webové aplikace | Tajná aplikace, který jste vytvořili v portálu registrace aplikace aplikace.  Není vhodné používat nativní aplikaci, protože client_secrets nelze uložit problémy se spolehlivým na zařízeních.  Je potřebný pro web apps a webové rozhraní API, která mít možnost Uložit `client_secret` bezpečně na straně serveru. |
| zdroje | povinná, pokud podle se tak mohli ověřovat žádost o kódu, jinak se ještě volitelné | Identifikátor URI aplikace ID webového rozhraní API (zabezpečené zdroje).
Identifikátor URI ID aplikace na portálu Správa Azure najdete klikněte **Služby Active Directory**, klikněte na adresář, klikněte na aplikace a potom klikněte na **Konfigurovat**.

### <a name="successful-response"></a>Úspěšné odpověď

Azure AD vrátí přístupový token po úspěšném odpověď. Minimalizovat sítě hovory z klientské aplikace a jeho přidružený latence, by měl klientské aplikaci mezipaměti tokeny přístupu pro zadané v odpovědi OAuth 2.0 životnost tokenu. Určení Životnost tokenů, můžete buď `expires_in` nebo `expires_on` hodnoty parametrů.

Pokud vrátí webového rozhraní API zdroje `invalid_token` kód chyby to může znamenat, zda je zdroj určila vypršela platnost tokenu. Pokud čas hodiny klienta a zdroje jsou uvedené různé (nazývané také "skew čas"), zvažte zdroje tokenu vypršela před tokenu zaškrtnuté z mezipaměti klienta. V takovém případě zrušte tokenu z mezipaměti, i když je pořád v rámci životnosti vypočítané.

Odpověď na úspěšné může vypadat takto:

```
{
  "access_token": " eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1388444763",
  "resource": "https://service.contoso.com/",
  "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA",
  "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
"id_token": " eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ.”
}

```

| Parametr | Popis |
| ----------------------- | ------------------------------- |
| access_token | Požadovaná přístupový token. Aplikaci můžete použít tento token ověření zabezpečené prostředku, například webového rozhraní API. |
| token_type | Určuje typ tokenu hodnotu. Pouze typ, který podporuje Azure AD je nosný. Další informace o nosný tokeny najdete v tématu [Framework OAuth2.0 se tak mohli ověřovat: nosný Token použití (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt)  |
| expires_in | Jak dlouho přístupový token je platný (v sekundách). |
| expires_on | Čas, kdy vyprší platnost přístupový token. Datum představuje počet sekund, než z 1970-01-01T0:0:0Z UTC až do doby vypršení platnosti. Tato hodnota slouží k určení dobu trvání mezipaměti tokeny. |
| zdroje | Identifikátor URI aplikace ID webového rozhraní API (zabezpečené zdroje).|
| rozsah | Zosobnění oprávněním ke klientské aplikaci. Výchozí oprávnění je `user_impersonation`. Vlastník zabezpečené zdroje v Azure AD evidovat další hodnoty.|
| refresh_token |  Aktualizace token OAuth 2.0. Aplikaci můžete použít tento token získat další přístup tokeny po vypršení platnosti aktuálního přístupový token.  Aktualizace tokeny jsou dlouhodobé a mohou sloužit k uchovávat přístupu k prostředkům dlouhou dobu. |
| id_token | Nepodepsané JSON Web tokenu (JWT). Base64Url se dají aplikace dekódovat segmentů tento token na žádost o informace o uživateli přihlášenému. Aplikaci můžete mezipaměti hodnoty a zobrazovat je, ale neměli závisí na nich se tak mohli ověřovat nebo hranice zabezpečení. |

### <a name="jwt-token-claims"></a>Deklarace tokenů JWT
Tokenu JWT hodnoty `id_token` parametru lze dekódovat do následujících deklarací:

```
{
 "typ": "JWT",
 "alg": "none"
}.
{
 "aud": "2d4d11a2-f814-46a7-890a-274a72a7309e",
 "iss": "https://sts.windows.net/7fe81447-da57-4385-becb-6de57f21477e/",
 "iat": 1388440863,
 "nbf": 1388440863,
 "exp": 1388444763,
 "ver": "1.0",
 "tid": "7fe81447-da57-4385-becb-6de57f21477e",
 "oid": "68389ae2-62fa-4b18-91fe-53dd109d74f5",
 "upn": "frank@contoso.com",
 "unique_name": "frank@contoso.com",
 "sub": "JWvYdCWPhhlpS1Zsf7yYUxShUwtUm5yzPmw_-jX3fHY",
 "family_name": "Miller",
 "given_name": "Frank"
}.
```

`id_token` Parametr zahrnuje následující typy deklarací. Další informace o JSON web tokenů najdete v článku [specifikace JWT IETF koncept](http://go.microsoft.com/fwlink/?LinkId=392344). Další informace o tokenu typů a pohledávky přečtěte si prosím [podporovaných tokenů a typy deklarací](active-directory-token-and-claims.md)

| Typ deklarace | Popis |
|------------|-------------|
| oblast | Cílové skupině představované tokenu. Vystavení tokenu ke klientské aplikaci cílové skupiny je `client_id` klienta.
| Exp | Čas vypršení platnosti. Čas, kdy vyprší platnost tokenu. Token platit aktuální datum a čas musí být menší než nebo rovno `exp` hodnotu. Čas představuje počet sekund, než od 1. ledna 1970 (1970-01-01T0:0:0Z) UTC až do doby byl vydán tokenu. |
| family_name | Uživatele příjmení. Aplikaci můžete zobrazit tuto hodnotu. |
| given_name | Křestní jméno uživatele. Aplikaci můžete zobrazit tuto hodnotu. |
| IAT | Vydaná v čase. Čas, kdy byl vydán JWT. Čas představuje počet sekund, než od 1. ledna 1970 (1970-01-01T0:0:0Z) UTC až do doby byl vydán tokenu. |
| iss | Určuje vydavatele tokenu |
| NBF | Nejdříve čas. Čas, kdy tokenu platnosti. Token platit aktuální datum a čas musí být větší než nebo rovná hodnotě Nbf. Čas představuje počet sekund, než od 1. ledna 1970 (1970-01-01T0:0:0Z) UTC až do doby byl vydán tokenu. |
| ID objektu | Identifikátor objektu (ID) objektu uživatele služby Azure AD. |
| Sub | Identifikátor URI tokenu předmět. Toto je trvalý a neměnný identifikátor pro uživatele, který popisuje tokenu. Použijte tuto hodnotu v logických ukládání do mezipaměti. |
| TID | Identifikátor URI klienta (ID) Azure AD klienta, který vystavil tokenu. |
| unique_name | Jedinečný identifikátor, která mohou být zobrazena uživateli. Je to obvykle hlavní jméno uživatele (UPN). |
| UPN | Hlavní uživatelské jméno uživatele. |
| verze | Verze. Verze token JWT, obvykle 1.0. |

### <a name="error-response"></a>Chyba odpověď

Chyby koncový bod vydání tokenu jsou kódy chyb HTTP, protože klient volá koncový bod vydání tokenu přímo. Kromě stavový kód HTTP koncový bod vydání tokenu Azure AD také vrátí JSON dokumentu s objekty, které popisují chyby do schránky.

Ukázka chyby odpověď může vypadat takto:

```
{
  "error": "invalid_grant",
  "error_description": "AADSTS70002: Error validating credentials. AADSTS70008: The provided authorization code or refresh token is expired. Send a new interactive authorization request for this user and resource.\r\nTrace ID: 3939d04c-d7ba-42bf-9cb7-1e5854cdce9e\r\nCorrelation ID: a8125194-2dc8-4078-90ba-7b6592a7f231\r\nTimestamp: 2016-04-11 18:00:12Z",
  "error_codes": [
    70002,
    70008
  ],
  "timestamp": "2016-04-11 18:00:12Z",
  "trace_id": "3939d04c-d7ba-42bf-9cb7-1e5854cdce9e",
  "correlation_id": "a8125194-2dc8-4078-90ba-7b6592a7f231"
}
```
| Parametr | Popis |
| ----------------------- | ------------------------------- |
| Chyba | Řetězec kódu chyby, který slouží ke klasifikaci typy chyb probíhajících a mohou sloužit k reagovat na chyby. |
| error_description | Konkrétní chybové zprávy, které vám můžou pomoct vývojář příčinu kořenové chybu ověřování.  |
| error_codes | Seznam kódů specifickou chybu služba tokenů zabezpečení, které mohou usnadnit diagnostických nástrojů. |
| časové razítko | Čas, kdy došlo k chybě. |
| trace_id | Jedinečný identifikátor pro požadavek, který mohou usnadnit diagnostických nástrojů.  |
| correlation_id | Jedinečný identifikátor pro požadavek, který mohou usnadnit diagnostiky přes součásti.|

#### <a name="http-status-codes"></a>Nastavit informace HTTP stavů

Následující tabulka uvádí kódy HTTP stavu, které vrátí vydání tokenu koncového bodu. V některých případech kód chyby stačí popisují odpověď, ale v případě chyby, bude potřeba analyzovat průvodním JSON a zkontrolujte jeho kód chyby.

| Kód HTTP | Popis |
|-----------|-------------|
| 400       | Výchozí HTTP kód. Použít ve většině případů který je obvykle kvůli chybný požadavek. Řešení a opětovné odeslání žádosti. |
| 401       | Ověření se nezdařilo. Například žádost chybí parametr client_secret.|
| 403       | Ověření se nezdařilo. Například uživatel nemá oprávnění pro přístup k prostředku. |
| 500       | Ve službách došlo k interní chybě. Opakování žádosti. |

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

## <a name="use-the-access-token-to-access-the-resource"></a>Použití přístupový token pro přístup k prostředku

Teď jste získali úspěšně `access_token`, aktivujete tokenu v žádosti o webového rozhraní API, včetně v `Authorization` záhlaví. Specifikace [RFC 6750](http://www.rfc-editor.org/rfc/rfc6750.txt) vysvětluje, jak používat nosný tokeny v žádosti o HTTP pro přístup ke chráněné prostředky.

### <a name="sample-request"></a>Ukázkový požadavek

```
GET /data HTTP/1.1
Host: service.contoso.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ
```

### <a name="error-response"></a>Chyba odpověď

Zabezpečené prostředky, které implementace RFC 6750 problém HTTP stavů. Pokud žádost nezahrnuje přihlašovacími údaji nebo chybí tokenu, odpověď obsahuje `WWW-Authenticate` záhlaví. Pokud žádost o selže, prostředků serveru odpoví HTTP stavový kód a kód chyby.

Následující obrázek je příkladem neúspěšný odpověď při žádosti klienta nezahrnuje nosný token:

```
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Bearer authorization_uri="https://login.window.net/contoso.com/oauth2/authorize",  error="invalid_token",  error_description="The access token is missing.",
```

#### <a name="error-parameters"></a>Parametry chybové zprávy

| Parametr | Popis |
|-----------|-------------|
| authorization_uri | Identifikátor URI (fyzické koncový bod) se tak mohli ověřovat serveru. Tato hodnota slouží také jako vyhledávací klíč z koncového bodu zjišťování získat další informace o serveru. <p><p> Klient musí ověřit, zda se tak mohli ověřovat serveru důvěryhodné. Když zdroje je chráněn Azure AD, stačí k ověření, že adresa URL začíná https://login.windows.net nebo jiný název hostitele, který podporuje Azure AD. Prostředek specifické pro klienta, měly vrátit vždy specifické pro klienta povolení URI. |
| Chyba | Kód chybové hodnoty v části 5,2 [OAuth 2.0 se tak mohli ověřovat Framework](http://tools.ietf.org/html/rfc6749)definované.|
| error_description | Podrobnější popis chyby do schránky. Tato zpráva není určená k být popisný koncových uživatelů.|
| ID_prostředku | Vrátí jedinečný identifikátor zdroje. Klientské aplikaci můžete použít tento identifikátor jako hodnota `resource` parametr požádá token zdroje. <p><p> Je velmi důležité pro klientské aplikace k ověření tuto hodnotu, jinak škodlivým služby pak můžou způsobit útok **zvýšení úrovně oprávnění** <p><p> Pro zabránění útok doporučujeme ověřit, zda `resource_id` souhlasí základ webového rozhraní API URL přistupuje. Pokud https://service.contoso.com/data pracuje, například `resource_id` může být htttps://service.contoso.com/. Klientská aplikace musí odmítnout `resource_id` , který nemá na začátku základní adresu URL nejedná spolehlivé alternativní způsob, jak ověřit id. |

#### <a name="bearer-scheme-error-codes"></a>Kódy chyb schéma nosný

Specifikace RFC 6750 definuje následujících chyb pro zdroje, které používají pomocí ověřování WWW záhlaví a nosný schéma v odpovědi.

| Nastavit informace HTTP stavový kód | Kód chyby | Popis | Akce klienta |
|------------------|------------|-------------|---------------|
| 400 | invalid_request | Pokud chcete žádost není ve správném formátu. Příklad by mohlo chybí parametr nebo dvakrát pomocí stejný parametr. | Oprava chyby a opakujte žádost. Tento typ chyby by měla proběhnout pouze během vývoje a zjišťování při počáteční testování. |
| 401 | invalid_token   | Přístupový token je chybí, neplatný nebo byl odvolán. Hodnota parametru error_description poskytuje další podrobnosti. |  Požádat o nové token serveru se tak mohli ověřovat. Pokud se nezdaří, nový token došlo k neočekávané chybě. Odešlete chybovou zprávu na uživatele a opakovat po náhodné zpoždění. |
| 403 | insufficient_scope | Přístupový token neobsahuje zosobnění o oprávněních požadovaných pro přístup k prostředku. | Odešlete novou žádost o povolení koncový bod se tak mohli ověřovat. Pokud odpověď obsahuje obor parametr, použijte hodnotou oboru v žádosti o zdroji. |
| 403 | insufficient_access | Předmět tokenu nemá oprávnění požadovaných pro přístup k prostředku. | Výzva k zadání použijte jiný účet nebo požádejte o oprávnění pro daný zdroj. |

## <a name="refreshing-the-access-tokens"></a>Aktualizace tokeny přístupu

Tokeny přístupu jsou krátkodobý a potřeba aktualizovat po vypršení jejich platnosti pokračujte přístupu k prostředkům. Můžete aktualizovat `access_token` odesláním jiného `POST` požádat o `/token` koncový bod, ale tento poskytující čas `refresh_token` místo `code`.

Aktualizace tokeny nemají zadaný životnost. Životnost tokeny aktualizace jsou obvykle relativně dlouhých. Však v některých případech aktualizace tokeny vypršení platnosti, odvolání nebo chybí dostatečná oprávnění pro požadovanou akci. Aplikace potřebuje očekávat a obsloužení chyb vrácené koncový bod vydání tokenu správně.

Když dostanete odpověď s chybou tokenu aktualizace zrušit aktuální token aktualizace a požádat o nový kód se tak mohli ověřovat nebo tokenu přístupu. Zejména při použití aktualizace token v toku se tak mohli ověřovat kód udělit Pokud dostanete odpověď s `interaction_required` nebo `invalid_grant` kódy chyb zahodit token aktualizace a požádat o nový kód ověření.

Ukázka požádat o koncový bod **specifické pro klienta** (můžete také **běžné** koncový bod) získat novou přístup tokenu pomocí, které token aktualizace vypadá nějak takto:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&grant_type=refresh_token
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```
| Parametr | Popis |
|-----------|-------------|
| access_token | Nové přístupový token, která byla požadovaná.|
| expires_in   | Životnost tokenu v sekundách. Typické hodnota je 3600 (jedna hodina). |
| expires_on   | Datum a čas, kdy vyprší platnost tokenu. Datum představuje počet sekund, než z 1970-01-01T0:0:0Z UTC až do doby vypršení platnosti. |
| refresh_token | Nové refresh_token OAuth 2.0 něhož požádat o nové tokeny přístup, když skončí platnost v této odpovědi. |
| zdroje     | Určuje zabezpečené prostředek, který může být přístupový token provede aplikace access. |
| rozsah        | Zosobnění oprávněním nativní klientské aplikaci. Výchozí oprávnění je **user_impersonation**. Vlastník cílový prostředek zaregistrovat alternativní hodnoty v Azure AD. |
| token_type   | Typ tokenu. Podporované hodnota je pouze **nosný**. |

### <a name="successful-response"></a>Úspěšné odpověď

Odpověď na úspěšné tokenu bude vypadat jako:

```
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1460404526",
  "resource": "https://service.contoso.com/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "refresh_token": "AwABAAAAv YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl PM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfmVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA"
}
```

### <a name="error-response"></a>Chyba odpověď

Ukázka chyby odpověď může vypadat takto:

```
{
  "error": "invalid_resource",
  "error_description": "AADSTS50001: The application named https://foo.microsoft.com/mail.read was not found in the tenant named 295e01fc-0c56-4ac3-ac57-5d0ed568f872.  This can happen if the application has not been installed by the administrator of the tenant or consented to by any user in the tenant.  You might have sent your authentication request to the wrong tenant.\r\nTrace ID: ef1f89f6-a14f-49de-9868-61bd4072f0a9\r\nCorrelation ID: b6908274-2c58-4e91-aea9-1f6b9c99347c\r\nTimestamp: 2016-04-11 18:59:01Z",
  "error_codes": [
    50001
  ],
  "timestamp": "2016-04-11 18:59:01Z",
  "trace_id": "ef1f89f6-a14f-49de-9868-61bd4072f0a9",
  "correlation_id": "b6908274-2c58-4e91-aea9-1f6b9c99347c"
}
```

| Parametr | Popis |
| ----------------------- | ------------------------------- |
| Chyba | Řetězec kódu chyby, který slouží ke klasifikaci typy chyb probíhajících a mohou sloužit k reagovat na chyby. |
| error_description | Konkrétní chybové zprávy, které vám můžou pomoct vývojář příčinu kořenové chybu ověřování.  |
| error_codes | Seznam kódů specifickou chybu služba tokenů zabezpečení, které mohou usnadnit diagnostických nástrojů. |
| časové razítko | Čas, kdy došlo k chybě. |
| trace_id | Jedinečný identifikátor pro požadavek, který mohou usnadnit diagnostických nástrojů.  |
| correlation_id | Jedinečný identifikátor pro požadavek, který mohou usnadnit diagnostiky přes součásti.|

Popis kódy chyb a akce doporučené klienta najdete v článku [kódy chyb chyb tokenu koncového bodu](#error-codes-for-token-endpoint-errors).
