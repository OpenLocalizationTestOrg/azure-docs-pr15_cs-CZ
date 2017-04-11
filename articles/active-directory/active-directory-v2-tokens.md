<properties
    pageTitle="Azure AD verze 2.0 tokenu odkaz | Microsoft Azure"
    description="Typy tokeny a deklarací které koncový bod verze 2.0"
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

# <a name="v20-token-reference"></a>odkaz tokenu verze 2.0

Koncový bod verze 2.0 posílá několik typů tokenů zabezpečení ve zpracování každý [tok ověřování](active-directory-v2-flows.md). Tento dokument popisuje formát, vlastnosti zabezpečení a obsah každého typu token.

> [AZURE.NOTE]
    Některé scénáře Azure Active Directory a funkce jsou podporovány koncový bod verze 2.0.  Pokud chcete zjistit, zda by měly používat koncový bod verze 2.0, přečtěte si informace o [omezení verze 2.0](active-directory-v2-limitations.md).

## <a name="types-of-tokens"></a>Typy tokenů

Koncový bod verze 2.0 podporuje [protokol se tak mohli ověřovat OAuth 2.0](active-directory-v2-protocols.md), které využívá access_tokens a refresh_tokens.  Podporuje také ověřování a přihlašovací prostřednictvím [OpenID připojení](active-directory-v2-protocols.md#openid-connect-sign-in-flow), které představuje třetí typ token, id_token.  Každý z těchto tokenů představuje "nosný token".

Nosný token je tokenu zabezpečení lightweight, který zajišťuje přístup "nosný" chráněné zdroji. V tomto smyslu "nosný" je osobě, která Visio svádět k zahrnutí tokenu. Když na oslavu nejdřív ověřuje s Azure AD pro příjem nosnému tokenu zabezpečení token pro předávání a úložiště nebudou přijata požadované kroky, můžete být zachyceny a použity před nechtěným osobou. Během pár tokenů zabezpečení mají předdefinované mechanismus zabránit neoprávněnými osobami je používat, nosný tokeny nemusí tento postup a musí být přepravovány v zabezpečeného kanálu například transport layer security (HTTPS). Pokud nosný token jsou přenášena v vymazat, člověka v prostředním útok lze škodlivým skupinou získat tokenu a použijte ji k neoprávněnému přístupu k chráněného zdroj. Platí stejné zásady zabezpečení při ukládání nebo ukládání do mezipaměti nosný tokeny pro pozdější použití. Vždy zajistěte, aby aplikace přenáší a ukládá nosný tokenů zabezpečení způsobem. Další otázky bezpečnosti pro na nosný tokenů najdete v článku [RFC 6750 oddílu 5](http://tools.ietf.org/html/rfc6750).

V mnoha tokenů vydán koncový bod verze 2.0 implementovaná formátu Json Web tokeny nebo JWTs.  JWT je kompaktní, XLL s podporou URL prostředky pro přenos informací mezi dvěma stranami.  Informace obsažené v JWTs se označují jako "deklarací" nebo výrazy informací o nosný a předmět tokenu.  Nároky v JWTs jsou objekty JSON kódování a serializován pro přenos.  Protože JWTs vydaný koncový bod verze 2.0 podepsán, ale ne zašifrovaných, můžete snadno zkontrolovat obsah JWT pro účely ladění. Další informace o JWTs může označovat [JWT specifikace](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

## <a name="idtokens"></a>Id_tokens

Id_tokens jsou formuláře přihlašovací tokenu zabezpečení jsme narazili načítající aplikace při ověřování pomocí [OpenID spojení](active-directory-v2-protocols.md#openid-connect-sign-in-flow).  Jsou zobrazeny jako [JWTs](#types-of-tokens)a obsahují deklarace, které používáte pro přihlášení uživatele k aplikaci.  Deklarace můžete použít v id_token jak najdete v článku přizpůsobení - běžně používají se k zobrazení informací o účtu nebo rozhodování ovládacího prvku v aplikaci pro access.  Koncový bod verze 2.0 problémy pouze jednoho typu id_token, který obsahuje sadu deklarované bez ohledu na typ uživatele, který má přihlášení.  Je to znamená, že formát a obsah id_tokens bude na to samé platí pro oba osobní uživatelé Account Microsoft a na pracovní nebo školní účet.

Id_tokens jsou přihlášení, ale ne zašifrovaných v současné době.  Přijaté aplikace id_token, musí být [ověřit podpis](#validating-tokens) prokázat tokenu pravost a ověřte několik nároky v tokenu abyste prokázali, že jeho platnosti.  Nároky ověřit aplikace lišit podle toho, scénář požadavky, ale existují některé [běžné ověření nároku](#validating-tokens) , který aplikace musíte udělat v každé firem.

Podrobnosti o nároky v id_tokens jsou uvedeny níže, i id_token vzorku.  Všimněte si, že nároky v id_tokens nejsou vráceny v určitém pořadí.  Kromě toho nové deklarované může dojít k tomu id_tokens kdykoli včas: aplikace neměli konce zavedení nové deklarované.  Následující seznam obsahuje deklarace, které aplikace může problémy se spolehlivým interpretovat při psaní tohoto textu.  V případě potřeby i další podrobnosti najdete v [specifikace OpenID připojení](http://openid.net/specs/openid-connect-core-1_0.html).

#### <a name="sample-idtoken"></a>Ukázka id_token

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiI2NzMxZGU3Ni0xNGE2LTQ5YWUtOTdiYy02ZWJhNjkxNDM5MWUiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vYjk0MTk4MTgtMDlhZi00OWMyLWIwYzMtNjUzYWRjMWYzNzZlL3YyLjAiLCJpYXQiOjE0NTIyODUzMzEsIm5iZiI6MTQ1MjI4NTMzMSwiZXhwIjoxNDUyMjg5MjMxLCJuYW1lIjoiQmFiZSBSdXRoIiwibm9uY2UiOiIxMjM0NSIsIm9pZCI6ImExZGJkZGU4LWU0ZjktNDU3MS1hZDkzLTMwNTllMzc1MGQyMyIsInByZWZlcnJlZF91c2VybmFtZSI6InRoZWdyZWF0YmFtYmlub0BueXkub25taWNyb3NvZnQuY29tIiwic3ViIjoiTUY0Zi1nZ1dNRWppMTJLeW5KVU5RWnBoYVVUdkxjUXVnNWpkRjJubDAxUSIsInRpZCI6ImI5NDE5ODE4LTA5YWYtNDljMi1iMGMzLTY1M2FkYzFmMzc2ZSIsInZlciI6IjIuMCJ9.p_rYdrtJ1oCmgDBggNHB9O38KTnLCMGbMDODdirdmZbmJcTHiZDdtTc-hguu3krhbtOsoYM2HJeZM3Wsbp_YcfSKDY--X_NobMNsxbT7bqZHxDnA2jTMyrmt5v2EKUnEeVtSiJXyO3JWUq9R0dO-m4o9_8jGP6zHtR62zLaotTBYHmgeKpZgTFB9WtUq8DVdyMn_HSvQEfz-LWqckbcTwM_9RNKoGRVk38KChVJo4z5LkksYRarDo8QgQ7xEKmYmPvRr_I7gvM2bmlZQds2OeqWLB1NSNbFZqyFOCgYn3bAQ-nEQSKwBaA36jYGPOVG2r2Qv1uKcpSOxzxaQybzYpQ
```

> [AZURE.TIP] Praktická část zkuste podrobná nároky v ukázkové id_token vložením do [calebb.net](https://calebb.net).

#### <a name="claims-in-idtokens"></a>Nároky v id_tokens
| Jméno | Deklarace | Příklad hodnoty | Popis |
| ----------------------- | ------------------------------- | ------------ | --------------------------------- |
| Cílové skupiny | `aud` | `6731de76-14a6-49ae-97bc-6eba6914391e` | Určuje určeného příjemce tokenu.  V id_tokens cílové skupiny je Id aplikace vaše aplikace přiřazená vaši aplikaci distribuovali portálu registrace aplikace.  Aplikace by měl ověřte tuto hodnotu a odmítnout tokenu, pokud ho neodpovídá. |
| Vydavatel | `iss` | `https://login.microsoftonline.com/b9419818-09af-49c2-b0c3-653adc1f376e/v2.0 ` | Určuje Služba tokenů zabezpečení (STS), který vytváří a vrátí tokenu, jakož i klienta Azure AD ověření uživatele.  Aplikace by měl ověření nároku Vystavitel zajistit, že tokenu pochází z verze 2.0 koncového bodu.  Používejte také část guid deklaraci omezení sadu klienti, které jsou povoleny a přihlaste se do aplikace.  Identifikátor guid, který označuje uživatel je uživatel spotř od Microsoftu účet je `9188040d-6c67-4c5b-b112-36a304b66dad`. |
| Vydané | `iat` | `1452285331` | Čas, kdy byl vydán tokenu, který v čase epocha. |
| Čas vypršení platnosti | `exp` | `1452289231` | Čas, pro niž tokenu ztratí, reprezentované epocha času.  Aplikace používejte toto tvrzení ověřte platnost tokenu životnost.  |
| Nejdříve | `nbf` | `1452285331` |  Čas, pro niž tokenu platnosti, reprezentované epocha času. Je obvykle stejné jako vydávání čas.  Aplikace používejte toto tvrzení ověřte platnost tokenu životnost.  |
| Verze | `ver` | `2.0` | Verze id_token definované Azure AD.  Verze 2.0 koncového bodu, bude hodnota `2.0`. |
| Id klienta | `tid` | `b9419818-09af-49c2-b0c3-653adc1f376e` | Identifikátor guid představující Azure AD klienta, kterou je uživatel z.  Pro pracovní a školní účty budou identifikátor guid neměnný klienta ID organizace, která patří uživatele.  Pro osobní účty, bude hodnota `9188040d-6c67-4c5b-b112-36a304b66dad`.  `profile` Obor je nutný pro příjem této deklarace. |
| Kód Hash | `c_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Kód hash je součástí id_tokens pouze v případě, že id_token vystaven spolu s kódem se tak mohli ověřovat OAuth 2.0.  Mohou sloužit k ověření pravosti se tak mohli ověřovat kódu.  Najdete v článku [připojení OpenID specifikace](http://openid.net/specs/openid-connect-core-1_0.html) podrobné informace o provádění ověřování. |
| Access tokenu Hash | `at_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Tokenu hash přístup je součástí id_tokens pouze v případě, že id_token vystaven vedle přístupový token OAuth 2.0.  Mohou sloužit k ověření pravosti přístupový token.  Najdete v článku [připojení OpenID specifikace](http://openid.net/specs/openid-connect-core-1_0.html) podrobné informace o provádění ověřování. |
| Nonce | `nonce` | `12345` | Náhodně vygenerovaný identifikátor je strategie pro zmírnění tokenu opakované útoky.  Aplikaci můžete vyplnit nonce žádost se tak mohli ověřovat pomocí `nonce` dotazu parametr.  Hodnota zadaná v žádosti o bude vypuštění v id_token `nonce` deklarace smíte bez jakýchkoli úprav.  Díky aplikaci můžete ověřit hodnotu s hodnotou zadaný v pozvánce na schůzku, která přidruží dané id_token relace v aplikaci.  Aplikace by měl provést ověřování během procesu ověření id_token. |
| Jméno | `name` | `Babe Ruth` | Deklarace názvu obsahuje lidské čitelné hodnotu, která určuje předmět tokenu. Tato hodnota není musí být jedinečný, je proměnlivých a je určen pouze pro účely zobrazení.  `profile` Obor je nutný pro příjem této deklarace. |
| E-mailu | `email` | `thegreatbambino@nyy.onmicrosoft.com` | Primární e-mailovou adresu přidruženou k uživatelský účet, pokud existuje.  Jeho hodnota je proměnlivých a pro daného uživatele v průběhu času mění.  `email` Obor je nutný pro příjem této deklarace. |
| Upřednostňovaný uživatelské jméno | `preferred_username` | `thegreatbambino@nyy.onmicrosoft.com` | Primární uživatelské jméno, který se používá pro uživatele systému koncový bod verze 2.0.  Může to být e-mailovou adresu, telefonní číslo nebo obecný uživatelské jméno bez zadaného formátu.  Jeho hodnota je proměnlivých a pro daného uživatele v průběhu času mění.  `profile` Obor je nutný pro příjem této deklarace. |
| Předmět | `sub` | `MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q` | Hlavního uživatele o tom, které tokenu uplatňuje informace, například uživatelem aplikace. Tato hodnota je neměnný a nelze znovu přiřadit nebo znovu použít, takže ho lze použít ke kontrole autorizace bezpečně, například kdy se používá tokenu pro přístup k prostředku. Vzhledem k tomu předmět vždy účastní Azure AD problémech tokeny, doporučujeme používat tuto hodnotu v systému se tak mohli ověřovat univerzální. |
| Objektu | `oid` | `a1dbdde8-e4f9-4571-ad93-3059e3750d23` | Objekt, který Id pracovní nebo školní účet v systému Azure AD.  Toto tvrzení nebude vystaven pro osobní účty Microsoft.  `profile` Obor je nutný pro příjem této deklarace. |


## <a name="access-tokens"></a>Tokeny přístupu

Tokeny přístupu vydaný koncový bod verze 2.0 jsou pouze spotřební tak, že Microsoft Services v daném okamžiku.  Vaše aplikace nemusí provádět ověření nebo kontroly přístup tokenů z některého z aktuálně podporované scénáře.  Tokeny přístupu můžete považovat za plně neprůhledné - jsou pouze řetězce, které aplikace Microsoft předat v žádosti o HTTP.

V blízké budoucnosti koncový bod verze 2.0 zavede možnost aplikace přijme tokeny přístupu od jiných klientech.  V té době tyto informace aktualizují údaje, včetně aplikací je potřeba tokenu ověření přístup a jiné podobné úkoly.

Při žádosti přístupový token z verze 2.0 koncového bodu koncový bod verze 2.0 také vrátí některé metadata přístupový token spotřebu vaše aplikace.  Tyto informace obsahuje i čas ukončení přístupový token a oborů, pro které platí.  Díky aplikaci provádět inteligentní ukládání do mezipaměti přístup tokenů aniž byste museli analyzovat otevřít přístupový token samotné.

## <a name="refresh-tokens"></a>Aktualizace tokenů

Aktualizace tokeny jsou tokenů zabezpečení, které aplikace můžete získat nový přístup tokeny v toku OAuth 2.0.  Je možné aplikaci pro dosažení dlouhodobé přístupu k prostředkům jménem uživatele bez nutnosti interakce uživatelem.

Tokeny aktualizace jsou více zdrojů.  To znamená, že aktualizace token přijaté v průběhu tokenu žádost o jeden zdroj můžete uplatnit přístup tokeny úplně jinému zdroji.

Abyste mohli přijímat aktualizace do tokenu odpověď, aplikace musí žádost o & k ní mít udělený `offline_acesss` obor.   Další informace o `offline_access` rozsah, podívejte se na [souhlas & obory článků zde](active-directory-v2-scopes.md).

Aktualizace tokeny jsou a vždycky, úplně neprůhledné do aplikace.  Budou vydává koncový bod verze 2.0 Azure AD a můžete jenom být zkontrolovat a interpretovat koncový bod verze 2.0.  Jsou dlouhodobé, ale aplikace by neměly být došlo k zápisu očekávat, že bude trvat token aktualizovat všechny dobu.  Tokeny aktualizace může být neplatné kdykoli různých důvodů.  Jediný způsob, jakým aplikace vědět, pokud platí token aktualizace je pokus o uplatnění tím, že žádost o tokenu koncový bod verze 2.0.

Po uplatnění token aktualizace pro nové přístupový token (a pokud byl udělen aplikace `offline_access` obor), zobrazí nový token aktualizace tokenu odpověď.  Uložte tokenu nově vydané aktualizace nahrazení tu, kterou jste použili v pozvánce.  To bude zaručit, že aktualizace tokeny platné po dobu.

## <a name="validating-tokens"></a>Ověřování tokenů

V tomto okamžiku pouze tokenu ověření, které aplikace by mělo být potřeba provést ověřování id_tokens.  Chcete ověřit id_token, měli aplikace ověřit podpis id_token a nároky v id_token.

<!-- TODO: Link -->
Připravili jsme pro knihovny a ukázek kódu, které ukazují, jak snadno zpracovávání tokenu ověření - pod informace je jednoduše k dispozici pro uživatele, kteří chtějí pochopit základní proces.  Máte k dispozici taky několik 3 stran otevřít zdroj knihovny umožňující JWT ověření: existuje aspoň jednou z možností skoro všech platformu a jazyk oddálit tam.

#### <a name="validating-the-signature"></a>Ověření podpisu
JWT obsahuje tři segmenty, které jsou odděleny `.` znak.  První segment se označuje jako **záhlaví**druhý **textu**a třetí jako **podpis**.  Segment podpisu lze ověřit pravost id_token tak, aby může být důvěryhodný aplikace.

Id_Tokens jsou přihlášenými pomocí standardní asymetrické šifrování algoritmů odvětví, například RSA 256. Záhlaví id_token obsahuje informace o metodě klíč a šifrování slouží k podpisu tokenu:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

`alg` Deklarace označuje algoritmu, který byl použit k podepsání tokenu, při `kid` deklarace označuje určité veřejným klíčem, který byl použit k podepsání tokenu.

V libovolném bodě v čase může koncový bod verze 2.0 podepsat id_token pomocí jedné z množiny klíčových dvojice veřejných a soukromých.  Koncový bod verze 2.0 otáčí možné množiny klíčů v pravidelných intervalech, takže aplikace by se měly zapisovat zpracovávání tyto klíčové změny automaticky.  Frekvenci rozumné zkontrolovat aktualizace veřejné klíče používané verze 2.0 koncový bod se asi 24 hodin.

Můžete získat podpisový klíčové údaje nutné ověřit podpis pomocí c:\ dokument metadat OpenID připojení:

```
https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration
```

> [AZURE.TIP] Vyzkoušejte tuto adresu URL v prohlížeči.

Tento dokument metadat je objekt JSON obsahující několik užitečných použitelné informací, jako je místo různých koncové body potřebných pro ověřování OpenID připojení.  

Obsahuje taky `jwks_uri`, který dává umístění sady veřejné klíče slouží k podpisu tokeny.  Dokument JSON c:\ `jwks_uri` obsahuje všechny informace o veřejných klíči používaných v této konkrétní časovém okamžiku.  Pomocí aplikace `kid` převzetí v záhlaví JWT vyberte, které veřejným klíčem v tomto dokumentu byla použita k podepsat konkrétní token.  Pak můžete provádět potvrzení podpisu správné veřejným klíčem a určené algoritmus.

Ověřování podpisu je mimo rozsah tento dokument – nejsou k dispozici pro pomůže vám to udělat v případě potřeby mnoha otevřít zdroj knihovny.

#### <a name="validating-the-claims"></a>Ověření deklarace
Pokud aplikace obdrží id_token při přihlašování uživatelů, má taky provést několik kontroly proti deklarace v id_token.  Zahrnout, ale nejsou omezené na:

- Deklarace **cílovou skupinu** – můžete ověřit, že id_token byl má být zadán do aplikace.
- **Nejdříve** a **Dobu platnosti** deklarace – můžete ověřit, že id_token nevypršela.
- Deklarace **Vystavitel** – můžete ověřit, že token byl skutečně vystaven do aplikace koncový bod verze 2.0.
- **Nonce** – jako omezení rizik útok tokenu přehrání.
- a mnohem víc...

Úplný seznam ověření nároku, které by měl provést aplikace v příručce [specifikace OpenID připojení](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation).

Podrobnosti o očekávané hodnoty pro tyto požadavky jsou uvedeny výše v [oddílu id_token](#id_tokens).


## <a name="token-lifetimes"></a>Tokenu životnost

Následující tokenu životnost jsou podle čistě pro vaše Principy můžou pomoct ve vývoji a ladění aplikace.  Vaše aplikace by neměly být došlo k zápisu očekávat některou z těchto životnost nemění – můžete a se změní v libovolném bodě v čase.

| Tokenu | Životnost | Popis |
| ----------------------- | ------------------------------- | ------------ |
| Id_Tokens (pracovní nebo školní účty) | 1 hodinu | Id_Tokens jsou obvykle platné po za hodinu.  Web appu můžete použít tento stejné životnost pro zachování vlastní relace s uživatelem (doporučeno) nebo zvolte životnost úplně různých relace.  Pokud potřebujete-li získat nové id_token aplikace, jednoduše potřebuje nové přihlašovací požádat o verze 2.0 povolte koncového bodu.  Pokud má uživatel relace platné prohlížeče s koncovým verze 2.0, nemusí být nutné znovu zadejte své přihlašovací údaje. |
| Id_Tokens (osobní účty) | 24 hodin | Id_Tokens pro osobní účty jsou obvykle platné po dobu 24 hodin.  Web appu můžete použít tento stejné životnost pro zachování vlastní relace s uživatelem (doporučeno) nebo zvolte životnost úplně různých relace.  Pokud potřebujete-li získat nové id_token aplikace, jednoduše potřebuje nové přihlašovací požádat o verze 2.0 povolte koncového bodu.  Pokud má uživatel relace platné prohlížeče s koncovým verze 2.0, nemusí být nutné znovu zadejte své přihlašovací údaje. |
| Tokeny přístupu (pracovní nebo školní účty) | 1 hodinu | Podle tokenu odpovědi jako součást tokenu metadata. |
| Tokeny přístupu (osobní účty) | 1 hodinu | Podle tokenu odpovědi jako součást tokenu metadata.  Access_tokens Vystavitel jménem osobní účty může nakonfigurován pro různé životnost, ale hodinu nastane, obvykle. |
| Aktualizace tokeny (pracovní nebo školní účet) | Až 14 dní | Token jednoho aktualizace platí pro maximálně 14 dní.  Aktualizace token však může stanou neplatnými kdykoli pro libovolný počet důvodů, tak, aby aplikace nadále vyzkoušejte a použijte token aktualizace, dokud se nezdaří, nebo aplikace ho nahradí nový token aktualizace.  Token aktualizovat také stanou neplatnými, pokud už uběhlo 90 dní od uživatel zadal své přihlašovací údaje. |
| Aktualizace tokeny (osobní účty) | Až 1 rok | Token jednoho aktualizace platí pro než jeden rok.  Aktualizace token však může stanou neplatnými kdykoli pro libovolný počet důvodů, tak, aby aplikace nadále si můžete vyzkoušet a používat token aktualizace, dokud se nezdaří. |
| Povolení kódy (pracovní nebo školní účty) | 10 minut | Povolení kódy jsou purposefully krátkodobý a by měl být okamžitě uplatnili access_tokens a refresh_tokens při příjmu. |
| Povolení kódy (osobní účty) | 5 minut | Povolení kódy jsou purposefully krátkodobý a by měl být okamžitě uplatnili access_tokens a refresh_tokens při příjmu.  Jednorázové použití jsou také se tak mohli ověřovat kódy Vystavitel jménem osobní účty. |
