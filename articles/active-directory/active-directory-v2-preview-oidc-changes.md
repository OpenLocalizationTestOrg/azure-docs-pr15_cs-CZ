<properties
    pageTitle="Změní na koncový bod verze 2.0 Azure AD | Microsoft Azure"
    description="Popis změny provedené na veřejné náhled protokoly aplikace model verze 2.0."
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

# <a name="important-updates-to-the-v20-authentication-protocols"></a>Důležité aktualizace protokoly ověřování verze 2.0
Vývojáři pozornost! Přes příští dva týdny jsme budete provádět několik aktualizace pro naše protokoly ověření verze 2.0, které může znamenat nejnovější změny pro všechny aplikace, které jste napsali období naše náhled.  

## <a name="who-does-this-affect"></a>Kdo to mít vliv?
Aplikace, které jste napsali používat verze 2.0 sblížen koncový bod ověřování

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
```

Další informace o koncový bod verze 2.0 najdete [tady](active-directory-appmodel-v2-overview.md).

Pokud máte vytvořenou aplikaci pomocí kódování přímo do protokolu verze 2.0 koncový bod verze 2.0, použití libovolného příkazu pro naše OpenID připojit nebo OAuth middlewares web nebo použití jiných 3 knihoven stran pro ověřování, můžete máme ještě počítat otestovat projektů a proveďte změny v případě potřeby.

## <a name="who-doesnt-this-affect"></a>Kdo to neovlivní?
Žádných aplikací, které jste napsali proti koncový bod výrobní Azure AD ověřování

```
https://login.microsoftonline.com/common/oauth2/authorize
```

Tento protokol se nastavuje pomocí dlaždice a nebude nastaly všechny změny.

Kromě toho pokud vaše aplikace **pouze** používá naše ADAL knihovny pro ověřování, nemusíte provádět žádné změny.  ADAL má chráněná aplikace ze změny.  

## <a name="what-are-the-changes"></a>Jaké jsou změny?
### <a name="removing-the-x5t-value-from-jwt-headers"></a>Odebrání záhlaví JWT hodnotu x5t
Koncový bod verze 2.0 používá JWT tokeny rozsáhle, které obsahují parametry záhlaví s relevantní metadata tokenu.  Pokud dekódovat záhlaví jednoho z našich aktuální JWTs by nějak najít:

```
{ 
    "type": "JWT",
    "alg": "RS256",
    "x5t": "MnC_VZcATfM5pOYiJHMba9goEKY",
    "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

Kde vlastnosti "x5t" a "dítě" popsána veřejným klíčem, který má být použit k ověřit podpis tokenu jako načtené z koncového bodu metadat OpenID připojení.

Změna, kterou jsme zpřístupnili, tady je odeberte vlastnost "x5t".  Možná bude dál používat stejné mechanismy ověřuje tokeny, ale měli spolehnout pouze vlastnost "dítě" k načtení správné veřejný klíč, jak je uvedeno v protokolu OpenID připojení. 

> [AZURE.IMPORTANT] **Práce: Ujistěte se, aplikace nezávisí na existenci hodnotu x5t.**

### <a name="removing-profileinfo"></a>Odebrání profile_info
Dříve koncový bod verze 2.0 má byla vrácení objekt ve formátu Base 64 zakódovaný JSON v tokenu odpovědi s názvem `profile_info`.  Při požadování přístupový token tak, že pošlete žádost o z verze 2.0 koncového bodu:

```
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Odpověď na vypadat následujícím JSON objektu:
```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

`profile_info` Hodnoty obsažené informace o uživateli, který přihlášení k aplikaci - jejich zobrazované jméno, křestního jména, příjmení, e-mailovou adresu, identifikátor a tak dál.  Především `profile_info` byla použita pro tokenu ukládání do mezipaměti a zobrazit jako referenci.

Teď můžeme odstraňujete `profile_info` hodnoty – ale Nebojte, jsme pořád poskytuje tyto informace pro vývojáře mírně jinam.  Namísto `profile_info`, koncový bod verze 2.0 nyní vrátí `id_token` v každé tokenu odpovědi:

```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

Může dekódovat a analyzovat id_token k načtení stejné informace, které jste dostali od profile_info.  Id_token je JSON Web tokenu (JWT), s obsahem podle OpenID připojení.  Kód pro provedete tak by měl být velmi podobné – stačí extrahování druhého segmentu (obsah) id_token a base64 dekódovat pro přístup k objektu JSON uvnitř.

Přes příští dva týdny by měl kódu aplikace načítat informace o uživateli z některého `id_token` nebo `profile_info`; podle toho, co je k dispozici.  Tak při provádění změn, aplikace Bezproblémová zpracovat přechodu z `profile_info` k `id_token` nepřetržitě.

> [AZURE.IMPORTANT] **Práce: Ujistěte se, aplikace nezávisí na existenci `profile_info` hodnotu.**

### <a name="removing-idtokenexpiresin"></a>Odebrání id_token_expires_in
Podobně jako `profile_info`, jsme také odstraňujete `id_token_expires_in` parametr z odpovědí.  Koncový bod verze 2.0 dříve, vrátí hodnotu `id_token_expires_in` spolu s každá odpověď id_token, například v odpověď udělit oprávnění:

```
https://myapp.com?id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...&id_token_expires_in=3599...
```

Nebo v odpovědi na token:

```
{ 
    "token_type": "Bearer",
    "id_token_expires_in": 3599,
    "scope": "openid",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

`id_token_expires_in` Hodnotu naznačují počet sekund, než id_token zůstane platná pro.  Teď můžeme odebrání `id_token_expires_in` úplně hodnoty.  Místo toho můžete použít standardní připojení OpenID `nbf` a `exp` tvrdí, zkontrolujte správnost id_token.  [Verze 2.0 tokenu odkaz](active-directory-v2-tokens.md) Další informace najdete na webu tyto požadavky.

> [AZURE.IMPORTANT] **Práce: Ujistěte se, aplikace nezávisí na existenci `id_token_expires_in` hodnotu.**


### <a name="changing-the-claims-returned-by-scopeopenid"></a>Změna deklarací vrácený rozsah = openid
Tato změna se nejvýznamnějším – ve skutečnosti, bude to mít vliv téměř všechny aplikace, která používá koncový bod verze 2.0.  Řada aplikací odešlou pomocí verze 2.0 koncového bodu `openid` rozsah, třeba:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid offline_access https://outlook.office.com/mail.read
```

Dnes, kdy uživatel uděluje souhlas `openid` rozsah, aplikace obdrží velkému množství informací o uživateli ve výsledném id_token.  Tyto požadavky může obsahovat jeho jméno, upřednostňované uživatelské jméno, e-mailovou adresu, ID objektu a další.

Tuto aktualizaci jsme měníte informace, které `openid` obor dává přístup aplikace, k lepší comform s specifikaci OpenID připojení.  `openid` Obor bude pouze povolit aplikaci podepsat uživatele systému a dostávat identifikátor specifické pro aplikaci pro uživatele systému `sub` nárok id_token.  Nároky v id_token pouze `openid` obor udělena bude neobsahuje žádné určitelné osobní údaje.  Příklad id_token deklarace jsou:

```
{ 
    "aud": "580e250c-8f26-49d0-bee8-1c078add1609",
    "iss": "https://login.microsoftonline.com/b9410318-09af-49c2-b0c3-653adc1f376e/v2.0 ",
    "iat": 1449520283,
    "nbf": 1449520283,
    "exp": 1449524183,
    "nonce": "12345",
    "sub": "MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q",
    "tid": "b9410318-09af-49c2-b0c3-653adc1f376e",
    "ver": "2.0",
}
```

Pokud chcete získat identifikovatelné osobní údaje o uživateli v aplikaci, aplikace muset požádat o další oprávnění od uživatele.  Budeme se úvodní informace o podporu pro dva nové obory z specifikace OpenID připojení – `email` a `profile` obory –, které vám umožní dělat tak.

- `email` Obor je velmi jednoduché – umožňuje aplikace access uživatele primární e-mailovou adresu prostřednictvím `email` převzetí v id_token.  Všimněte si, že `email` deklarace nebude vždy účastní id_tokens – bude pouze však započítávány Pokud je k dispozici v profilu uživatele.
- `profile` Obor dává aplikace přístup k další základní informace o uživateli – jejich jména, upřednostňovaný uživatelské jméno, ID objektu a tak dále.

Umožňuje provádět kódu aplikace způsobem minimální vyzrazení – požádejte uživatele, sady jenom informací, že aplikace vyžaduje, aby jeho práci.  Pokud chcete pokračovat začíná úplné sady uživatelské informace, které aktuálně přijímá aplikace, byste měli zahrnout všechny tři obory v žádostech se tak mohli ověřovat:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid profile email offline_access https://outlook.office.com/mail.read
```

Aplikaci můžete začít odesílat `email` a `profile` obory okamžitě a koncový bod verze 2.0 přijetí těchto dvou oborů a začněte vyžaduje oprávnění uživatelů podle potřeby.  Ale změnit při interpretaci `openid` obor se projeví po pár týdnů.

> [AZURE.IMPORTANT] **Práce: Přidat `profile` a `email` obory, pokud vaše aplikace vyžaduje informace o uživateli.**  Všimněte si, že ADAL bude obsahovat obě tato oprávnění v žádosti o ve výchozím nastavení. 

### <a name="removing-the-issuer-trailing-slash"></a>Odebrání koncových lomítko vydavatel.
Dříve Vystavitel hodnotu, která se zobrazí v tokenů z verze 2.0 koncového bodu pořídili pomocí formuláře

```
https://login.microsoftonline.com/{some-guid}/v2.0/
```

Identifikátor guid místo, kam bylo tenantId Azure AD klienta, který vystavil tokenu.  Tyto změny změní hodnotu vydavatel

```
https://login.microsoftonline.com/{some-guid}/v2.0 
```

v obou tokeny a zjišťování dokumentů OpenID připojení.

> [AZURE.IMPORTANT] **Práce: Ujistěte se, aplikace přijímá Vystavitel hodnotu s i bez koncových lomítko během Vystavitel ověření.**

## <a name="why-change"></a>Proč změnit?
Primární pracovat zavedení tyto změny je kompatibilní s standardní specifikaci OpenID připojení.  Tím, že je OpenID připojení požadavkům, jsme ať minimalizovat rozdíly mezi integrace s služby identit a další služby identit v odvětví.  Chceme umožňuje vývojářům pomocí knihoven ověřování své oblíbené otevřít zdroj aniž by bylo nutné změnit knihovny tak, aby zahrnoval Microsoft rozdíly.

## <a name="what-can-you-do"></a>Co se dá dělat?
K dnešnímu dni můžete začít vytvářet všechny změny jsme je popsali výše.  Měli byste okamžitě:

1.  **Odeberte všechny služby závislé na `x5t` parametr záhlaví.**
2.  **Řádně zpracovat přechodu z `profile_info` k `id_token` v tokenu odpovědi.**
3.  **Odeberte všechny služby závislé na `id_token_expires_in` parametr odpověď.**
3.  **Přidejte `profile` a `email` obory do aplikace, pokud aplikace potřebuje základní informace o uživatelích.**
4.  **Přijměte Vystavitel hodnoty v tokeny s i bez koncových lomítko.**

Náš [si přečtěte následující dokumentaci protokol verze 2.0](active-directory-v2-protocols.md) již byly aktualizovány podle těchto změn, může ho použít jako odkaz v pomoci aktualizovat kódu.

Pokud máte další otázky v rozsahu změn, přejděte prosím neváhejte kontaktujte nás na Twitter na @AzureAD.

## <a name="how-often-will-protocol-changes-occur"></a>Jak často se protokol dochází ke změnám?
Není jsme stanovit, že všechny další přerušení se změní na protokoly pro ověřování.  Tyto změny do jedné verze jsme jsou navazují úmyslně tak, že nebudete muset absolvovat tento typ proveďte znovu kdykoli brzy bude k dispozici.  Samozřejmě budeme dál přidávat funkce ke službě ověřování zapnutým verze 2.0, který můžete využít, ale tyto změny by měl být doplňková a ne konec existující kód.

Nakonec byste rádi vyslovte Děkujeme za období náhled vyzkoušet věci.  Další informace a prostředí naše jednotlivá byly velmi užitečné v případě doposud a jsme ať že budete dál sdílet nápady a názorů.

Veselý kódování!

Dělení identit
