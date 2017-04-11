<properties
   pageTitle="Práce s identitami založených na deklaraci identity v aplikacích víceklientské | Microsoft Azure"
   description="Způsob použití deklarace pro ověření Vystavitel a ověření"
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/23/2016"
   ms.author="mwasson"/>

# <a name="working-with-claims-based-identities-in-multitenant-applications"></a>Práce s na základě deklarací identit víceklientské aplikace

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Tento článek je [součástí řady]. Je také úplné [Ukázková aplikace] , který doprovází řady.

## <a name="claims-in-azure-ad"></a>Nároky v Azure AD

Pokud se uživatel přihlásí, odešle Azure AD token ID, který obsahuje sadu žádostí o uživateli. Deklaraci je jednoduše druh informací vyjádřený pár klíč/hodnota. Například `email` = `bob@contoso.com`.  Nároky mít Vystavitel &mdash; v tomto případě Azure AD &mdash; tedy entita, která ověřuje uživatele a vytvoří deklarace. Deklarace zabezpečení, protože důvěřujete Vystavitel. (Naopak, pokud nedůvěřovat Vystavitel nedůvěřovat deklarace!)

Na vysoké úrovni:

1.  Uživatel se ověří.
2.  IDP odešle sadu deklarací.
3.  Aplikaci normalizuje nebo argumentech deklarací (volitelné).
4.  Aplikaci rozhodovat se tak mohli ověřovat pomocí deklarace.

V OpenID připojit nastavení deklarace, které se zobrazí řídí tak, že [oboru parametr] požadavek na ověření. Problémy se však Azure AD omezené sadu deklarací prostřednictvím OpenID připojení; v tématu [podporované tokenu a typy deklarace]. Pokud budete potřebovat další informace o uživateli, musíte použít rozhraní API Azure AD grafu.

Tady jsou některé deklarací z AAD, které aplikace může obvykle záleží na:

Typ ID tokenu deklarace |    Popis
-----------------------|--------------
oblast | Kdo token byl vydán pro. To bude ID aplikace klienta. Obecně neměli byste potřebovat starat o toto tvrzení protože middleware automaticky ověří. Příklad:`"91464657-d17a-4327-91f3-2ed99386406f"`
skupiny   | Seznam skupin AAD kterých je uživatel členem. Příklad:`["93e8f556-8661-4955-87b6-890bc043c30f", "fc781505-18ef-4a31-a7d5-7d931d7b857e"]`
iss  | [Vydavatel] OIDC token. Příklad:`https://sts.windows.net/b9bd2162-77ac-4fb2-8254-5c36e9c0a9c4/`
Jméno    | Zobrazované jméno uživatele. Příklad:`"Alice A."`
ID objektu | Identifikátor objektu pro uživatele systému AAD. Tato hodnota je neměnný a jednorázových identifikátor uživatele. Použijte tato hodnota, ne e-mail, jako jedinečný identifikátor pro uživatele. můžete změnit e-mailové adresy. Použití rozhraní API Azure AD graf v aplikaci ID objektu je tato hodnota slouží k informace o profilu dotazu. Příklad:`"59f9d2dc-995a-4ddf-915e-b3bb314a7fa4"`
role   | Seznam aplikací role pro uživatele. Příklad:`["SurveyCreator"]`
TID | ID klienta. Tato hodnota je jedinečný identifikátor klienta v Azure AD. Příklad:`"b9bd2162-77ac-4fb2-8254-5c36e9c0a9c4"`
unique_name | Lidské čitelné zobrazované jméno uživatele. Příklad:`"alice@contoso.com"`
UPN | Hlavní název uživatele. Příklad:`"alice@contoso.com"`

V této tabulce jsou uvedeny typy deklarace přenesené do ID token. V ASP.NET Core 1.0 middleware OpenID připojení u některých typů deklarace při převádí naplní kolekci deklarací hlavní uživateli:

-   ID objektu >`http://schemas.microsoft.com/identity/claims/objectidentifier`
-   TID >`http://schemas.microsoft.com/identity/claims/tenantid`
-   unique_name >`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`
-   přípony >`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn`

## <a name="claims-transformations"></a>Nároky transformace

Během tok ověřování můžete chtít změnit deklarace, které se zobrazí v IDP. V ASP.NET Core 1.0 můžete provádět deklarací transformace uvnitř **AuthenticationValidated** událost z middleware OpenID připojení. (Viz [události ověřování].)

Nároky, které přidáte během **AuthenticationValidated** jsou uložené v cookie ověřování relace. Budou nechcete dostat posunuto Azure AD.

Tady je pár příkladů deklarací transformaci:

-   **Normalizace nároky**nebo vytváření deklarací konzistentní napříč uživatelů. To platí, zejména pokud z více IDPs, které může použít typy různých deklarací podobné informace se zobrazují deklarované.
Například Azure AD odesílá "" deklarace, který obsahuje e-mailu uživatelů. Další IDPs může poslat deklaraci "e-mailu". Následující kód převede "deklarace" deklaraci "e-mailu":

    ```csharp
    var email = principal.FindFirst(ClaimTypes.Upn)?.Value;
    if (!string.IsNullOrWhiteSpace(email))
    {
      identity.AddClaim(new Claim(ClaimTypes.Email, email));
    }
    ```

- Přidání **nárok na výchozí hodnoty** pro deklarací, které nejsou k dispozici &mdash; například přiřazení uživatele do výchozí role. V některých případech to můžete zjednodušit logiky se tak mohli ověřovat.
- Přidáte **vlastní deklarace typy** specifické pro aplikaci informace o uživateli. Například může obsahují některé informace o uživateli v databázi. Můžete přidat vlastní deklarace se tyto informace do lístek ověření. Deklaraci je uložen v soubor cookie, takže potřebujete získat z databáze na začátku každé relace přihlášení. Na druhé straně taky chcete zabránit vytváření příliš velký soubory cookie, takže byste měli myslet poměr velikosti souborů cookie a vyhledávání v databázi.   

Po dokončení tok ověřování deklarace nabízí `HttpContext.User`. V tomto okamžiku je by měly považovat za kolekce jen pro čtení &mdash; například používat k přijímání rozhodnutí se tak mohli ověřovat.

## <a name="issuer-validation"></a>Ověření vydavatel
V OpenID připojit identifikuje deklaraci vystavitel ("iss") IDP, která vystavila ID token. Část tok ověřování OIDC je ověřte, zda vydavatel deklarace odpovídá skutečné vydavatel. To zpracovává OIDC middleware.

V Azure AD Vystavitel hodnotu jedinečné jednoho klienta AD (`https://sts.windows.net/<tenantID>`). Aplikace proto, měli byste udělat další zaškrtnutí, aby zkontrolovala, jestli že vystavitel představuje klienta umožňující se přihlásit k aplikaci.

Aplikace pomocí jednoho klienta jednoduše zjistíte, že je vystavitel vlastního klienta. Ve skutečnosti OIDC middleware dělá automaticky ve výchozím nastavení. V aplikaci více klienta budete muset povolit více vydavatelů odpovídající různých klientů. Tady je obecný postup použít:

-   V části Možnosti middleware OIDC nastavte **ValidateIssuer** NEPRAVDA. Tato možnost vypne automatické zaškrtnutí.
-   Pokud zaregistruje ke klientovi uchovávat klienta a Vystavitel v uživatele DB.
-   Pokaždé, když uživatel přihlásí, vyhledejte Vystavitel v databázi. Pokud není nalezen vystavitel, znamená to, nebyla registraci tomuto klientovi. Můžete je přesměrovávat je na stránce registrace.
-  Se může taky heslem seznam zakázaných serverů určitých klientů; pro zákazníky, které jste se vlastně zaplatit předplatné.

Podrobnější informace najdete v článku [registrace a klient rychlého připojení víceklientské aplikaci][signup].

## <a name="using-claims-for-authorization"></a>Použití žádostí o povolení

S deklarací identity uživatele už monolitický entity. Uživatel může mít například e-mailové adresy, telefonního čísla, narozeniny, pohlaví, atd. Možná IDP uživatele jsou uloženy všechny tyto informace. Ale když ověřuje uživatele, zobrazí se obvykle podmnožinu jako deklarované. V tomto modelu identita uživatele je jednoduše sada škod. Při rozhodování se tak mohli ověřovat o uživateli, bude vypadat pro konkrétní sady deklarované. Jinými slovy, otázku "Uživatele X akci můžou provádět Y" nakonec se stane "Znamená X mají nároku uživatele Z".

Tady jsou některé základní vzorce pro kontrolu deklarované.

-  Chcete-li zkontrolovat, jestli má uživatel konkrétní deklaraci identity s určitou hodnotu:

    ```csharp
    if (User.HasClaim(ClaimTypes.Role, "Admin")) { ... }
    ```
    Tento kód zkontroluje, jestli má uživatel Role deklarace s hodnotou "Správce". Zpracuje správně případ, kde má uživatel žádné Role deklarace nebo více Role deklarace.

    Třídy **ClaimTypes** definuje konstanty pro typy běžně používaných deklarace. Však může použít libovolnou hodnotu řetězce pro typ deklarace.

-   Zobrazíte jednu hodnotu pro typ deklarace, když je očekávaná existovat maximálně jednu hodnotu:
    ```csharp
     string email = User.FindFirst(ClaimTypes.Email)?.Value;
    ```
-   Chcete-li získat všechny hodnoty pro typ deklarace:

    ```csharp
     IEnumerable<Claim> groups = User.FindAll("groups");
    ```

Další informace najdete v tématu [na základě rolí a na základě zdroje se tak mohli ověřovat víceklientské aplikace][authorization].

## <a name="next-steps"></a>Další kroky

- Přečtěte si další článek v této řadě: [registrace a klient rychlého připojení víceklientské aplikaci][signup]
- Další informace o [povolení na základě deklarované identity] v dokumentaci ASP.NET Core 1.0

<!-- Links -->
[součástí řady]: guidance-multitenant-identity.md
[obor parametrů]: http://nat.sakimura.org/2012/01/26/scopes-and-claims-in-openid-connect/
[Podporované Token a typy deklarací]: ../active-directory/active-directory-token-and-claims.md
[Vydavatel]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken
[Ověřování události]: guidance-multitenant-identity-authenticate.md#authentication-events
[signup]: guidance-multitenant-identity-signup.md
[Ověřování na základě deklarací]: https://docs.asp.net/en/latest/security/authorization/claims.html
[Ukázková aplikace]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
[authorization]: guidance-multitenant-identity-authorize.md
