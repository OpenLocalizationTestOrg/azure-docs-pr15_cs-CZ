<properties
    pageTitle="Azure AD verze 2.0 obory, oprávnění a souhlas | Microsoft Azure"
    description="Popis se tak mohli ověřovat v Azure AD verze 2.0 koncový bod, včetně obory, oprávnění a souhlas."
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

# <a name="scopes-permissions--consent-in-the-v20-endpoint"></a>Obory, oprávnění a souhlasu koncový bod verze 2.0

Aplikace, které integrace s Azure AD podle určité oprávnění modelu, který umožňuje určit, jak můžete aplikaci pro přístup ke svým datům.  Byl aktualizován verze 2.0 provádění tento model se tak mohli ověřovat, změna, musíte interakce aplikace s Azure AD.  Toto téma popisuje základních koncepcí tento model se tak mohli ověřovat, včetně obory, oprávnění a souhlas.

> [AZURE.NOTE]
    Některé scénáře Azure Active Directory a funkce jsou podporovány koncový bod verze 2.0.  Pokud chcete zjistit, zda by měly používat koncový bod verze 2.0, přečtěte si informace o [omezení verze 2.0](active-directory-v2-limitations.md).

## <a name="scopes--permissions"></a>Obory a oprávnění

Azure AD používá protokol se tak mohli ověřovat [OAuth 2.0](active-directory-v2-protocols.md) , který představuje metodu umožňující 3 aplikace jiných výrobců pro přístup k prostředkům web hostovaný jménem uživatele.  Web hostovaný zdroje, které lze integrovat s Azure AD bude mít identifikátoru zdroje nebo **URI ID aplikace**.  Například společnosti Microsoft web hostovaný zdroje patří:

- Office 365 jednotné poštovní rozhraní API:`https://outlook.office.com`
- Rozhraní API Azure AD grafu:`https://graph.windows.net`
- Aplikace Microsoft Graph:`https://graph.microsoft.com`

Platí totéž i pro 3 stran zdroje, které obsahuje integrovaný s Azure AD.  Některé z těchto zdrojů můžete taky definují množinu oprávnění, která mohou sloužit k rozdělení funkce, které zdroje na menší bloky.  Jako příklad má aplikace Microsoft Graph definované několik oprávnění:

- Přečtěte si kalendář jiného uživatele
- Zapisovat do kalendář jiného uživatele
- Posílání pošty jako uživatel
- [+ Další](https://graph.microsoft.io)

Když definujete tato oprávnění, můžete mít zdroje jemně odstupňovaná publikum nemůže ovládat její data a jak se zobrazí ve vnějším světě.  3 aplikace jiných výrobců můžete pak žádost o oprávněních s koncovým uživatelem – a koncový uživatel musí schválit oprávnění, než může aplikace fungovat nim jejich jménem.  Podle bloků zdroje funkce do menší sad oprávnění, můžete 3 stran aplikace vytvořené požádat o pouze konkrétní oprávnění, které potřebují k provedení jejich povinnosti.  Také umožňuje koncovým uživatelům vědět, co přesně aplikace se používání související data jsou větší jistotu, že aplikace nepracuje s lidi se zlými úmysly.

V Azure AD a OAuth, tato oprávnění se označují jako **obory**.  Může taky uvidíte je uvedená jako **oAuth2Permissions**.  Obor představuje v Azure AD hodnotu řetězce.  Pokračovat v příkladu Microsoft Graph obor pro každé oprávnění hodnotu:

- Přečtěte si kalendář jiného uživatele:`Calendar.Read`
- Napište do kalendáře uživatele:`Mail.ReadWrite`
- Odešlete poštu jako uživatele:`Mail.Send`

Aplikaci můžete požádat o oprávněních zadáním obory v žádosti o koncový bod verze 2.0 podle níže uvedeného popisu.

## <a name="openid-connect-scopes"></a>Připojení OpenId oborů

Verze 2.0 provádění OpenID připojení obsahuje několik dobře definovaný obory, které se nevztahují konkrétního zdroje - `openid`, `email`, `profile`, a `offline_access`.

#### <a name="openid"></a>OpenId

Pokud aplikace provádí přihlášení pomocí [OpenID spojení](active-directory-v2-protocols.md#openid-connect-sign-in-flow), musí požádat `openid` obor.  `openid` Obor se zobrazí na obrazovce pracovní účet souhlas jako oprávnění "Přihlásit" a osobní obrazovce souhlas účet Microsoft jako oprávnění "Zobrazit svůj profil a připojte k aplikace a služby pomocí účtu Microsoft".  Toto oprávnění umožňuje aplikaci nedostáváte jedinečný identifikátor pro uživatele v podobě `sub` deklarace.  Také poskytuje aplikace access koncový bod informace o uživateli.  `openid` Obor lze také na token koncovém bodu verze 2.0 získat id_tokens, které lze použít k zabezpečení HTTP volání mezi jednotlivých součástí aplikace.

#### <a name="email"></a>E-mailu

`email` Rozsahu může být součástí podél `openid` rozsah a všechna další.  Poskytuje aplikace access uživatele primární e-mailovou adresu ve formě `email` deklarace.  `email` Deklarace bude pouze součástí tokeny při přidružené k uživatelskému účtu, který je vždy v případě e-mailovou adresu.  Používáte-li `email` rozsah, aplikace máme ještě počítat zpracovávání případ, ve kterém `email` deklaraci identity v tokenu neexistuje.

#### <a name="profile"></a>Profil

`profile` Rozsahu může být součástí podél `openid` rozsah a všechna další.  Poskytuje aplikace přístup k velkému množství informací o uživateli.  Zahrnutí, ale není omezena na uživatele křestní jméno, příjmení, upřednostňované uživatelské jméno, ID objektu a tak dál.  Úplný seznam deklarací profilu v id_tokens k dispozici pro daného uživatele podívejte se do [verze 2.0 tokenu odkaz](active-directory-v2-tokens.md).

#### <a name="offlineaccess"></a>Offline_access

[ `offline_access` Obor](http://openid.net/specs/openid-connect-core-1_0.html#OfflineAccess) umožňuje aplikace při přístupu k prostředkům jménem uživatele delší dobu.  Na obrazovce pracovní účet souhlas tento obor se zobrazí jako oprávnění "Přístup ke svým datům kdykoli".  Osobní Microsoft účtu souhlas obrazovce se zobrazí jako oprávnění "Informace o přístup kdykoliv".  Pokud schválí uživatele `offline_access` rozsah, aplikace budou povoleny přijímat aktualizace tokenů z verze 2.0 tokenu koncového bodu.  Aktualizace tokeny jsou dlouhodobé a povolit aplikaci získat nové tokeny přístupu jako platnost starší z nich.

Pokud aplikace nepožaduje `offline_access` rozsah, nebudou refresh_tokens.  To znamená, že když uplatnění authorization_code v [toku kód se tak mohli ověřovat OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow)pouze získáte zpět access_token z `/token` koncového bodu.  Tento access_token platí krátký časový úsek (obvykle hodinu), ale nakonec vyprší.  V této okamžiku aplikace muset přesměrování uživatele zpět `/authorize` koncový bod k načtení nové authorization_code.  Během tohoto přesměrování uživatel může nebo nemusí muset zadávat přihlašovací údaje znovu nebo znovu souhlas oprávnění, v závislosti na domovské stránky týmového.

Další informace o tom, jak získat a použijte aktualizace tokenů, podívejte se na [odkaz protokol verze 2.0](active-directory-v2-protocols.md).


## <a name="requesting-individual-user-consent"></a>Požadování souhlasu jednotlivé uživatele

V [OpenID připojit nebo OAuth 2.0](active-directory-v2-protocols.md) se tak mohli ověřovat žádost o konverzaci, aplikace můžete požádat o oprávnění potřebná pomocí `scope` dotazu parametr.  Například pokud se uživatel přihlásí do aplikace, aplikace by pošlete žádost o například následující (plus pár konce řádků pro snazší čitelnost):

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=
https%3A%2F%2Fgraph.microsoft.com%2Fcalendar.read%20
https%3A%2F%2Fgraph.microsoft.com%2Fmail.send
&state=12345
```

`scope` Parametr je seznam oddělený mezerami oborů, které žádosti o aplikaci.  Každý jednotlivé obor je označen přidávání hodnotou oboru zdroje identifikátoru (aplikace Identifikátor URI).  Výše uvedený požadavek označuje, že aplikace vyžaduje oprávnění ke čtení kalendář jiného uživatele a odesílání pošty jako uživatel.

Poté, co uživatel zadá své přihlašovací údaje, koncový bod verze 2.0 kontroloval odpovídající záznam **svolení uživatele**.  Pokud uživatel nedala souhlas se požadovaná oprávnění v minulosti, bude koncový bod verze 2.0 požádejte uživatele, aby požadované oprávnění.  

![Snímek obrazovky souhlas pracovní účet](../media/active-directory-v2-flows/work_account_consent.png)

Když uživatel schválí oprávnění, budou souhlas ho máte nastavený tak, aby uživatel nemá znovu souhlas na pozdější přihlášení.

## <a name="requesting-consent-for-an-entire-tenant"></a>Vyžadování souhlasu pro celého klienta

Když se organizace nákup licence a předplatné aplikace, chtějí plně zřízení pro jejich zaměstnance.  V rámci tohoto procesu company Administrators udělit souhlas pro tuto aplikaci k následným akcím jménem zaměstnance.  Udělením souhlas pro celého klienta nebude zaměstnanců organizace zaznamenat obrazovce souhlas pro tuto aplikaci.

Zažádat o souhlas všem uživatelům klienta, můžete pomocí aplikace **správu souhlas koncový bod**, píše níže.

## <a name="admin-restricted-scopes"></a>Omezená Správa oborů

Určitá oprávnění maximum oprávnění v aplikaci Microsoft ekosystému můžete označit jako **Správce omezení**.  Příklady těchto obory:

- Čtení organizaion adresáře dat:`Directory.Read`
- Zápis dat do adresáře organizace:`Directory.ReadWrite`
- Čtení skupiny zabezpečení v adresáři organizace:`Groups.Read.All`

Během spotř uživatel může udělit aplikaci přístup k těmto údajům, organizační uživatelé jsou znemožnit udělení přístupu k stejnou sadu dat citlivé společnosti.  Pokud aplikace požádá o přístup na jeden z těchto oprávnění z organizační uživatele, zobrazí chybová zpráva, že pocházejí autorizaci pro souhlas s oprávnění vaše aplikace.

Pokud aplikace vyžaduje přístup k těchto omezení správce oborů určené pro organizace, měli byste je požadovat přímo z company Administrators taky používá **správu souhlas koncový bod**, píše níže.

Pokud správce poskytuje tato oprávnění prostřednictvím koncový bod správce souhlas, souhlas udělena všem uživatelům klienta, jak jsme je popsali výše.

## <a name="using-the-admin-consent-endpoint"></a>Použití koncový bod souhlas správce

Provedením těchto kroků, budou moct shromažďovat oprávnění pro všechny uživatele v dané klienta, včetně správy omezení obory aplikace.  Ukázka kódu implementující následující kroky desribed zobrazíte v nápovědě k [správce s omezeným přístupem obory vzorku](https://github.com/Azure-Samples/active-directory-dotnet-admin-restricted-scopes-v2).

#### <a name="request-the-permissions-in-the-app-registration-portal"></a>Žádosti o oprávnění v portálu registrace aplikace

- Pokud jste to ještě neudělali, přejděte do aplikace v [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)nebo [Vytvoření aplikace pro](active-directory-v2-app-registration.md) .
- Vyhledejte část **Microsoft Graph oprávnění** a přidejte oprávnění, která vyžaduje aplikaci.
- Nejdřív musíte mít **Uložit** registrace aplikace

#### <a name="recommended-sign-the-user-into-your-app"></a>Doporučené: uživatel přihlásit k aplikaci aplikace

Obvykle při vytváření aplikace, která používá správce souhlas koncový bod, aplikace bude nutné stránky nebo zobrazení, které umožňuje správu ke schválení oprávnění aplikace.  Na této stránce může být součástí aplikace na registrace toku, část nastavení v aplikaci nebo vyhrazené toku "připojit".  V mnoha případech dává smysl aplikace zobrazovat "" zobrazení pouze po připojit uživatele má přihlášení pomocí pracovního nebo školního účtu Microsoft.

Po přihlášení uživatele k aplikaci umožňuje určit organziation, ke kterému patří před s žádostí o schválení potřebná oprávnění správce.  Když není nezbytných ho můžete vám pomůže s vytvářením intuitivnější prostředí pro organizační uživatele.  Pokud se chcete odhlásit uživatele systému, postupujte podle naše [verze 2.0 protokol výukové programy pro](active-directory-v2-protocols.md).

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
| klienta | povinné | Adresář klienta, který chcete požádat o souhlas.  Lze zadat ve formátu popisný název nebo guid. |
| client_id | povinné | Id aplikace portál registrace ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) přiřazené aplikace. |
| redirect_uri | povinné | Redirect_uri místo, kam chcete odpověď k odeslání aplikace pro zpracování.  To se musí přesně shodovat jednu redirect_uris zapsaný na portálu. |
| Stav | Doporučené | Hodnota součástí požadavek, který bude vrácena také v tokenu odpovědi.  Může být řetězec veškerý obsah, který chcete.  Stav slouží ke kódování informace o stavu uživatele v aplikaci před požadavek na ověření došlo k chybě, například stránky nebo zobrazení, které byly na. |

V tomto okamžiku Azure AD bude jejímu vynucení, že jenom správce klienta se přihlásit k dokončení zadání.  Správce bude požádán, aby všechna oprávnění, které požadujete pro vaši aplikaci distribuovali portálu registrace schválení. 

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
Pokud správce není schválit oprávnění pro aplikaci, budou selhalo odpověď:

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Parametr | Popis |
| ----------------------- | ------------------------------- | --------------- |
| Chyba | Řetězec kódu chyby, který slouží ke klasifikaci typy chyb probíhajících a mohou sloužit k reagovat na chyby. |
| error_description | Konkrétní chybové zprávy, které vám můžou pomoct vývojář příčinu kořenové chybu.  |

Po přijetí úspěšné odpovědi z koncového bodu souhlas správce aplikace získal oprávnění, která byla požadována.  Teď můžete přesunout na vyžádání token pro požadovaný zdroj podle níže uvedeného popisu.

## <a name="using-permissions"></a>Pomocí oprávnění

Poté, co uživatel souhlasí oprávnění pro aplikaci, aplikace můžete získat přístup tokenů, které představují vaše aplikace oprávnění pro přístup k prostředku v některých kapacity.  Dané přístupový token lze použít pouze pro jeden resorce, ale zakódovaný uvnitř budou každé poskytovaná aplikace pro daný zdroj.  Získat přístupový token aplikace můžete požádat verze 2.0 tokenu koncového bodu:

```
POST common/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/json

{
    "grant_type": "authorization_code",
    "client_id": "6731de76-14a6-49ae-97bc-6eba6914391e",
    "scope": "https://outlook.office.com/mail.read https://outlook.office.com/mail.send",
    "code": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq..."
    "redirect_uri": "https://localhost/myapp",
    "client_secret": "zc53fwe80980293klaj9823"  // NOTE: Only required for web apps
}
```

Výsledný přístupový token lze do HTTP požadavků zdroje: ho problémy se spolehlivým označí tomuto zdroji, že má aplikace správné oprávnění k provedení daného úkolu.  

Další podrobnosti o protokolu OAuth 2.0 a jak získat přístup tokenů najdete v článku [verze 2.0 koncový bod protokol odkaz](active-directory-v2-protocols.md).
