<properties
    pageTitle="Implicitní toku Azure AD verze 2.0 | Microsoft Azure"
    description="Vytváření webových aplikací pomocí Azure AD verze 2.0 provádění implicitní tok jedné stránky aplikací."
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

# <a name="v20-protocols---spas-using-the-implicit-flow"></a>verze 2.0 protokoly - SPA pomocí implicitní toku
S koncový bod verze 2.0 můžete podepisovat uživatele do jedné stránky aplikací s osobní a pracovní/školní účty od Microsoftu.  Jedné stránky a dalších aplikací JavaScript, spusťte především v prohlížeči nominální několika zajímavými přijal pokusí, pokud jde o ověření:

- Vlastnosti zabezpečení tyto aplikace jsou významně neliší od tradiční serverové webových aplikací.
- Mnoho se tak mohli ověřovat servery a zprostředkovatelé identit jiní nepodporují CORS žádosti.
- Mimo aplikaci se stanou zejména invazivní uživatelské prostředí přesměrování celou stránku prohlížeče.

Pro tyto aplikace (popřemýšlet: AngularJS, Ember.js, React.js, atd) Azure AD podporuje tok OAuth 2.0 implicitní grant (udělit).  Implicitní tok je popsaná v [Specifikace 2.0 OAuth](http://tools.ietf.org/html/rfc6749#section-4.2).  Primární výhoda je možnost aplikace na vaši tokenů z Azure AD provádění back-end serveru exchange přihlašovacích údajů.  Díky aplikaci a přihlaste se uživatele, spravovat relace a získat tokeny k jiným webovým rozhraní API všechno v klientovi kód v JavaScriptu.  Existuje několik důležité informace o zabezpečení vzít v úvahu při použití implicitní tok - konkrétně kolem [klienta](http://tools.ietf.org/html/rfc6749#section-10.3) a [zosobnění uživatele](http://tools.ietf.org/html/rfc6749#section-10.3).

Pokud chcete použít implicitní tok a Azure AD přidat ověřování do aplikace JavaScript, doporučujeme že použít naše otevřít JavaScript úloh Knihovna zdrojů [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js).  Několik AngularJS kurzy jsou dostupné [tady](active-directory-appmodel-v2-overview.md#getting-started) vám pomůžou začít.  

Ale pokud chcete raději není používá knihovnu v aplikaci jedné stránky a posílání zpráv protokol sami, postupujte podle následujících obecných krocích.

> [AZURE.NOTE]
    Některé scénáře Azure Active Directory a funkce jsou podporovány koncový bod verze 2.0.  Pokud chcete zjistit, zda by měly používat koncový bod verze 2.0, přečtěte si informace o [omezení verze 2.0](active-directory-v2-limitations.md).
    
## <a name="protocol-diagram"></a>Diagram Protocol (protokol)
Celý implicitní přihlásit toku vypadá přibližně takto: všechny kroky se píše níže.

![OpenId připojení plavecké dráhy](../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

## <a name="send-the-sign-in-request"></a>Odeslání žádosti o přihlášení

Se původně přihlásit do aplikace pro uživatele, můžete odeslat žádostí o povolení [Připojení OpenID](active-directory-v2-protocols-oidc.md) a dostávat `id_token` z verze 2.0 koncového bodu:

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token+token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=openid%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&response_mode=fragment
&state=12345
&nonce=678910
```

> [AZURE.TIP] Po kliknutí na odkaz k provedení tohoto požadavku. Po přihlášení prohlížeči přesměrovaní na `https://localhost/myapp/` s `id_token` do adresního řádku.
    <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token+token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=openid%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment&state=12345&nonce=678910" target="_blank">https://Login.microsoftonline.com/common/oauth2/v2.0/authorize...</a>

| Parametr | | Popis |
| ----------------------- | ------------------------------- | --------------- |
| klienta | povinné | `{tenant}` Hodnoty v parametru path žádosti o lze určit, kdo může přihlásit k aplikaci.  Jsou povolené hodnoty `common`, `organizations`, `consumers`a klient identifikátory.  Další podrobnosti najdete v článku [Základy protokol](active-directory-v2-protocols.md#endpoints). |
| client_id | povinné | Id aplikace portál registrace ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) přiřazené aplikace. |
| response_type | povinné | Musí obsahovat `id_token` pro připojení OpenID přihlášení.  Mohou také obsahovat response_type `token`. Použití `token` tady vám umožní aplikace okamžitě přijme přístupový token od koncového bodu povolení bez nutnosti provádět žádost o druhé koncový bod udělit oprávnění.  Pokud používáte `token` response_type, `scope` parametr musí obsahovat obor určující, které zdroje o vystavení tokenu pro. |
| redirect_uri | Doporučené | Redirect_uri aplikace, kde můžete ověřování odpovědi odešlete nebo přijmete tak, že aplikace.  To se musí přesně shodovat jednu redirect_uris zapsaný na portálu kromě musí být překódován jako adresa url. |
| rozsah | povinné | Seznam oborů oddělený mezerami.  Pro OpenID připojit, musí obsahovat obor `openid`, které se převádí na oprávnění "Přihlásit" v souhlasu uživatelského rozhraní.  Volitelně můžete taky chtít zahrnout `email` nebo `profile` [obory](active-directory-v2-scopes.md) pro získání přístupu k datům další uživatele.  Jiné obory mohou také obsahovat v tomto žádosti o žádosti o souhlas s různým zdrojům.  |
| response_mode | Doporučené | Určuje metodu, která má být použit k odeslání výsledné tokenu zpět do aplikace.  By měl být `fragment` pro implicitní tok.  |
| Stav | Doporučené | Hodnota součástí požadavek, který bude vrácena také v tokenu odpovědi.  Může být řetězec veškerý obsah, který chcete.  Náhodně generovaným jedinečnou hodnotu se obvykle používá pro [Zabránění útoky padělání webů žádost](http://tools.ietf.org/html/rfc6749#section-10.12).  Stav je také použít ke kódování informace o stavu uživatele v aplikaci před požadavek na ověření došlo k chybě, například stránky nebo zobrazení, které byly na. |
| Nonce | povinné | Hodnota zahrnuté v pozvánce na schůzku, generovaných aplikaci, která bude obsahovat výsledné id_token jako deklaraci.  Aplikaci můžete ověřit pak tuto hodnotu na zmírnění tokenu opakované útoky.  Hodnota je obvykle náhodného jedinečné řetězcem, který slouží k identifikaci origin žádost.  |
| dotaz | volitelné | Označuje typ interakce s uživatelem, který je potřebný.  Platný pouze hodnoty v současné době jsou "přihlášení", "žádný" a "souhlas.  `prompt=login`Vynutí uživateli zadejte své přihlašovací údaje na žádost, negace jednotného přihlašování na.  `prompt=none`je opakem - budete mít jistotu, že uživatel nezobrazují se libovolný interaktivní zprávou jakékoliv.  Pokud žádost nelze dokončit tiše prostřednictvím jednotného přihlašování na, koncový bod verze 2.0 vrátí chybu.  `prompt=consent`Spustí OAuth dialogovém souhlas, když uživatel přihlásí, s dotazem, uživatelům udělit oprávnění k aplikaci. |
| login_hint | volitelné | Slouží k předem zadání pole uživatelské jméno nebo e-mailovou adresu obrazovky přihlašovací stránky pro uživatele, pokud znáte svoje uživatelské jméno předem.  Často aplikace použijete tento parametr během nové ověřování, máte už extrahovaných uživatelské jméno z předchozí přihlášení pomocí `preferred_username` deklarace. |
| domain_hint | volitelné | Může se jednat o `consumers` nebo `organizations`.  Pokud, přeskočí proces zjišťování e mailové tento uživatel prochází na stránce verze 2.0 přihlásit, vést k mírně více optimalizovaného uživatelského prostředí.  Často aplikace se nepoužívá tento parametr během nové ověřování extrahování `tid` požadovat id_token.  Pokud `tid` převzetí hodnotu `9188040d-6c67-4c5b-b112-36a304b66dad`, byste měli použít `domain_hint=consumers`.  V ostatních případech použít `domain_hint=organizations`. |

V tomto okamžiku že uživatel bude požádán, aby zadejte své přihlašovací údaje a dokončit ověření.  Koncový bod verze 2.0 také zajistí, že uživatel souhlasí oprávnění vyznačené `scope` dotazu parametr.  Pokud některý z těchto oprávnění nedala souhlas uživatele, bude požádejte uživatele, aby souhlas s potřebná oprávnění.  Podrobnosti o [zde uvedené oprávnění, souhlas a více klienta aplikace](active-directory-v2-scopes.md).

Jakmile je uživatel ověří a uděluje souhlas, koncový bod verze 2.0 vrátí odpověď aplikace v určené `redirect_uri`, metodou podle `response_mode` parametr.

#### <a name="successful-response"></a>Úspěšné odpověď

Úspěšné odpovědí pomocí `response_mode=fragment` a `response_type=id_token+token` vypadá jako následující s konce řádků pro lepší čitelnosti:

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&token_type=Bearer
&expires_in=3599
&scope=https%3a%2f%2fgraph.microsoft.com%2fmail.read 
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
```

| Parametr | Popis |
| ----------------------- | ------------------------------- |
| access_token | Když však započítávány `response_type` obsahuje `token`. Přístupový token požadovanou aplikaci, v tomto případě pro aplikace Microsoft Graph.  Přístupový token by neměly být dekódovat nebo jinak zkontrolovat, můžete se považuje za neprůhledné řetězec. |
| token_type | Když však započítávány `response_type` obsahuje `token`.  Vždycky `Bearer`. |
| expires_in | Když však započítávány `response_type` obsahuje `token`.  Označuje počet sekund tokenu je platný pro účely mezipaměti. |
| rozsah | Když však započítávány `response_type` obsahuje `token`.  Označuje scope(s), u kterého access_token bude platit. |
| id_token | Id_token požadovanou aplikaci. Id_token slouží k ověření identity uživatele a zahájení relace s uživatelem.  Další informace o id_tokens a jejich obsah je součástí [tokenu reference verze 2.0 koncového bodu](active-directory-v2-tokens.md).  |
| Stav | Je-li parametr stát zahrnuta v pozvánce na schůzku, by se měly stejnou hodnotu v odpovědi. Aplikace by měl ověřte, jestli jsou stejné hodnoty stavu v žádostí a odpovědí. |


#### <a name="error-response"></a>Chyba odpověď
Chyba odpovědi může posílat taky `redirect_uri` tak aplikaci můžete řádně podporovat zpracování:

```
GET https://localhost/myapp/#
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parametr | Popis |
| ----------------------- | ------------------------------- |
| Chyba | Řetězec kódu chyby, který slouží ke klasifikaci typy chyb probíhajících a mohou sloužit k reagovat na chyby. |
| error_description | Konkrétní chybové zprávy, které vám můžou pomoct vývojář příčinu kořenové chybu ověřování.  |

## <a name="validate-the-idtoken"></a>Ověření id_token
Jenom příjem id_token nestačí k ověření uživatele. je nutné ověřit podpis id_token a ověření nároky v tokenu za požadavky vaše aplikace.  Koncový bod verze 2.0 používá [JSON Web tokeny (JWTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) a veřejné klíčové kryptografický podepsat tokeny a ověřte platnost.

Pak můžete ověřit `id_token` v klientovi kód, ale běžné si je odeslat `id_token` do back-end serveru a ověření tam.  Po ověření podpis id_token existuje několik deklarací, které budete vyzváni k ověření.  V tématu [verze 2.0 tokenu odkaz](active-directory-v2-tokens.md) Další informace, včetně [Ověřování tokeny](active-directory-v2-tokens.md#validating-tokens) a [Důležité informace o přihlašování klíče při přechodu myší](active-directory-v2-tokens.md#validating-tokens).  Doporučujeme tokeny využívání knihovně pro analýzu a ověření – alespoň jedno pro není k dispozici většinu jazyků a platformách.
<!--TODO: Improve the information on this-->

Můžete taky další deklarací podle toho, nefunguje ověření.  Některé běžné ověření patří:

- Zajištění uživatele nebo organizace přihlásila k odběru aplikace.
- Zajištění uživatel má správná autorizace a oprávnění
- Zajištění sílu ověřování došlo k chybě, například vícefaktorové ověřování.

Další informace o nároky v id_token najdete v článku [tokenu reference verze 2.0 koncového bodu](active-directory-v2-tokens.md).

Jakmile úplně ověření id_token můžete zahájit relaci s uživatelem a použít deklarace v id_token získat informace o uživateli v aplikaci.  Tyto informace lze použít pro zobrazení záznamy, povolení, atd.

## <a name="get-access-tokens"></a>Získat přístup tokeny

Teď, když uživatel jste přihlášení k aplikaci jednu stránku, můžete získat přístup tokeny pro volající webového rozhraní API zajištěná Azure AD, jako je [Microsoft Graph](https://graph.microsoft.io).  I když už přijatých tokenu pomocí `token` response_type, můžete tento způsob získání tokeny na další materiály aniž byste museli přesměrovat uživatele, aby se znovu přihlaste.

V normálním toku OpenID připojit/OAuth by to uděláte tak, že požadavek verze 2.0 `/token` koncového bodu.  Koncový bod verze 2.0 však nepodporuje CORS požadavky, takže volání AJAX a získat aktualizace tokeny je mimo otázku.  Místo toho můžete implicitní tok v skryté iframe zobrazíte nové tokeny pro ostatní webového rozhraní API: 

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment
&state=12345&nonce=678910
&prompt=none
&domain_hint=organizations
&login_hint=myuser@mycompany.com
```

> [AZURE.TIP] Zkuste kopírování a vkládání pod požadavek na na záložce prohlížeče! (Nezapomeňte nahradit `domain_hint` a `login_hint` správné hodnoty pro vaše uživatele)

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment&state=12345&nonce=678910&prompt=none&domain_hint={{consumers-or-organizations}}&login_hint={{your-username}}
```

| Parametr | | Popis |
| ----------------------- | ------------------------------- | --------------- |
| klienta | povinné | `{tenant}` Hodnoty v parametru path žádosti o lze určit, kdo může přihlásit k aplikaci.  Jsou povolené hodnoty `common`, `organizations`, `consumers`a klient identifikátory.  Další podrobnosti najdete v článku [Základy protokol](active-directory-v2-protocols.md#endpoints). |
| client_id | povinné | Id aplikace portál registrace ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) přiřazené aplikace. |
| response_type | povinné | Musí obsahovat `id_token` pro připojení OpenID přihlášení.  Může taky zahrnovat další response_types jako `code`. |
| redirect_uri | Doporučené | Redirect_uri aplikace, kde můžete ověřování odpovědi odešlete nebo přijmete tak, že aplikace.  To se musí přesně shodovat jednu redirect_uris zapsaný na portálu kromě musí být překódován jako adresa url. |
| rozsah | povinné | Seznam oborů oddělený mezerami.  Dosáhnout toho, aby tokeny zahrnout všechny [obory](active-directory-v2-scopes.md) , kterou požadujete pro zdroj úrok.  |
| response_mode | Doporučené | Určuje metodu, která má být použit k odeslání výsledné tokenu zpět do aplikace.  Může se jednat o `query`, `form_post`, nebo `fragment`.  |
| Stav | Doporučené | Hodnota součástí požadavek, který bude vrácena také v tokenu odpovědi.  Může být řetězec veškerý obsah, který chcete.  Náhodně generovaným jedinečnou hodnotu se obvykle používá pro zabránění útoky padělání žádost webů.  Stav je také použít ke kódování informace o stavu uživatele v aplikaci před požadavek na ověření došlo k chybě, například stránky nebo zobrazení, které byly na. |
| Nonce | povinné | Hodnota zahrnuté v pozvánce na schůzku, generovaných aplikaci, která bude obsahovat výsledné id_token jako deklaraci.  Aplikaci můžete ověřit pak tuto hodnotu na zmírnění tokenu opakované útoky.  Hodnota je obvykle náhodného jedinečné řetězcem, který slouží k identifikaci origin žádost.  |
| dotaz | povinné | Aktualizace a zprovoznění tokeny v skryté iframe, měli byste používat `prompt=none` ověřit, že do rámce iframe není reagovat na stránce verze 2.0 přihlásit a vrátí okamžitě. |
| login_hint | povinné | Aktualizace a zprovoznění tokeny v skryté iframe, je třeba zahrnout uživatelské jméno uživatele v této nápovědy k rozlišení mezi více relací, který může mít uživatel v daném okamžiku v čase. Extrahování uživatelské jméno z předchozí přihlášení pomocí `preferred_username` deklarace. |
| domain_hint | povinné | Může se jednat o `consumers` nebo `organizations`.  Pro aktualizaci a zprovoznění tokeny v skryté iframe, musí obsahovat domain_hint v pozvánce.  By měl extrahovat `tid` požadovat id_token předchozí přihlášení k určení hodnot, které chcete použít.  Pokud `tid` převzetí hodnotu `9188040d-6c67-4c5b-b112-36a304b66dad`, byste měli použít `domain_hint=consumers`.  V ostatních případech použít `domain_hint=organizations`. |

Poděkování `prompt=none` parametr, tato žádost o bude buď úspěšné nebo selhání okamžitě a vraťte se do aplikace.  Odešle úspěšné odpověď do aplikace v určené `redirect_uri`, metodou podle `response_mode` parametr.

#### <a name="successful-response"></a>Úspěšné odpověď
Úspěšné odpovědí pomocí `response_mode=fragment` vypadá podobně jako:

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
&token_type=Bearer
&expires_in=3599
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read
```

| Parametr | Popis |
| ----------------------- | ------------------------------- |
| access_token | Token požadovanou aplikaci. |
| token_type | Vždycky `Bearer`. |
| Stav | Je-li parametr stát zahrnuta v pozvánce na schůzku, by se měly stejnou hodnotu v odpovědi. Aplikace by měl ověřte, jestli jsou stejné hodnoty stavu v žádostí a odpovědí. |
| expires_in | Jak dlouho přístupový token je platný (v sekundách). |
| rozsah | Obory, které přístupový token platí pro. |

#### <a name="error-response"></a>Chyba odpověď
Chyba odpovědi může posílat taky `redirect_uri` tak aplikaci můžete provede řádně podporovat.  U `prompt=none`, bude očekávané Chyba:

```
GET https://localhost/myapp/#
error=user_authentication_required
&error_description=the+request+could+not+be+completed+silently
```

| Parametr | Popis |
| ----------------------- | ------------------------------- |
| Chyba | Řetězec kódu chyby, který slouží ke klasifikaci typy chyb probíhajících a mohou sloužit k reagovat na chyby. |
| error_description | Konkrétní chybové zprávy, které vám můžou pomoct vývojář příčinu kořenové chybu ověřování.  |

Pokud se zobrazí tato chyba v žádosti o iframe, uživatel musí interaktivně Přihlaste se znovu načíst nový token.  Můžete případě v jakékoli způsob, jak bude výstižně popisovat aplikace.

## <a name="refreshing-tokens"></a>Aktualizace tokenů

Obě `id_token`s a `access_token`s vyprší po krátký časový úsek, aby aplikace musí být připraveni tyto aktualizace tokeny pravidelně.  K aktualizaci obou typů token, můžete provádět zadání skryté iframe z horní pomocí `prompt=none` parametr řízení chování Azure AD.  Pokud nechcete dostat žádnou nový `id_token`, je nutné použít `response_type=id_token` a `scope=openid`, jakož i `nonce` parametr.


## <a name="send-a-sign-out-request"></a>Odhlásit se žádost o odeslání

OpenIdConnect `end_session_endpoint` není aktuálně podporován koncový bod verze 2.0. To znamená, že aplikace nelze pošlete žádost o koncový bod verze 2.0 ukončit relaci uživatele a vymažte soubory cookie nastavil koncový bod verze 2.0.
Se pokud chcete odhlásit uživatele mimo, aplikace můžete jednoduše ukončení vlastní relace s uživatelem a nechte uživatele relace se verze 2.0 koncového bodu v tact.  Při příštím při pokusu o přihlášení, uvidí stránku "Zvolte účet" s jejich aktivně přihlášený účty uvedené.
Na této stránce můžete uživatele zvolte se chcete odhlásit z libovolného účtu, ukončení relace s koncovým verze 2.0.

<!--

When you wish to sign the user out of the  app, it is not sufficient to clear your app's cookies or otherwise end the session with the user.  You must also redirect the user to the v2.0 endpoint for sign out.  If you fail to do so, the user will be able to re-authenticate to your app without entering their credentials again, because they will have a valid single sign-on session with the v2.0 endpoint.

You can simply redirect the user to the `end_session_endpoint` listed in the OpenID Connect metadata document:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
```

| Parameter | | Description |
| ----------------------- | ------------------------------- | ------------ |
| post_logout_redirect_uri | recommended | The URL which the user should be redirected to after successful logout.  If not included, the user will be shown a generic message by the v2.0 endpoint.  |

-->