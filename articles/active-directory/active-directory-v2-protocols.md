<properties
    pageTitle="Protokoly verze 2.0 Azure AD | Microsoft Azure"
    description="Příručka k protokolům nepodporuje koncový bod Azure AD verze 2.0."
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

# <a name="v20-protocols---oauth-20--openid-connect"></a>Připojení protokoly - OAuth 2.0 & OpenID verze 2.0

Koncový bod verze 2.0 můžete použít Azure AD pro identitu jako-service s standardních protokolů, OpenID připojení a OAuth 2.0.  Během služba není kompatibilní se standardy, může být pravopisné rozdíly mezi libovolné dva implementace těchto protokolů.  Zde uvedené informace budou užitečné, pokud se rozhodnete zadejte kód přímo odesláním a zpracování požadavků HTTP nebo použití 3rd strana otevřete Knihovna zdrojů, nikoli pomocí jednoho z našich knihoven otevřít zdroj.
<!-- TODO: Need link to libraries above -->

> [AZURE.NOTE]
    Některé scénáře Azure Active Directory a funkce jsou podporovány koncový bod verze 2.0.  Pokud chcete zjistit, zda by měly používat koncový bod verze 2.0, přečtěte si informace o [omezení verze 2.0](active-directory-v2-limitations.md).

## <a name="the-basics"></a>Základní informace
Skoro všech OAuth a OpenID připojení toků existují čtyři strany zahrnuté v exchange:

![Role OAuth 2.0](../media/active-directory-v2-flows/protocols_roles.png)

- **Povolení Server** je koncový bod verze 2.0.  Odpovídá pro zajištění identita uživatele, poskytnutí odvolání přístupu k prostředkům a vydávání tokeny.  Je označovaná taky jako zprostředkovatele identit – všechno, co dělat s informací o jeho, jejich přístup a vztahy důvěryhodnosti mezi strany toku bezpečně zpracovává.
- **Vlastníka zdroje** je obvykle koncový uživatel.  Je stran, který vlastní data a je možnost povolit jinými výrobci pro přístup k této dat nebo zdroje.
- **OAuth klienta** je aplikace, označenými jeho ID aplikace.  Je obvykle straně koncový uživatel pracuje s a žádosti tokeny ze serveru se tak mohli ověřovat.  Klient musí udělit oprávnění pro přístup k prostředku vlastníka zdroje.
- Kde jsou uložená zdroje nebo data je **Prostředků serveru** .  Vztah důvěryhodnosti serveru se tak mohli ověřovat bezpečně ověřování a povolte OAuth klienta a používá nosný access_tokens zajistit, že můžete mít udělený přístup k prostředku.


## <a name="app-registration"></a>Registrace aplikace
Všechny aplikace, která používá koncový bod verze 2.0 muset registrovaná u [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) před můžete komunikovat pomocí OAuth nebo OpenID připojení.  Proces registrace aplikace shromažďovat a přiřadit několika hodnot do aplikace:

- Číslo **Id aplikace** , které jednoznačně identifikuje aplikace
- **Identifikátor URI balíčku** něhož chcete směrovat odpovědi zpátky do aplikace nebo **Přesměrovat URI**
- Několik scénář specifické hodnotám.

Další informace, zjistěte, jak si [zaregistrovat aplikace](active-directory-v2-app-registration.md).

## <a name="endpoints"></a>Koncové body
Po registraci, aplikace informuje uživatele o s Azure AD tak, že žádosti o verze 2.0 koncového bodu:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token
```

Kde `{tenant}` může trvat jednu ze čtyř různých hodnot:

| Hodnota | Popis |
| ----------------------- | ------------------------------- |
| `common` | Umožňuje uživatelům s osobní účty Microsoft a pracovní/školní účty služby Azure Active Directory a přihlaste se do aplikace. |
| `organizations` | Umožňuje jenom uživatelé s oprávněními pracovní/školní z Azure Active Directory a přihlaste se do aplikace. |
| `consumers` | Umožňuje jenom uživatelé s osobní účty (MSA) a přihlaste se do aplikace Microsoft. |
| `8eaef023-2b34-4da1-9baa-8bc8c9d6a490`nebo`contoso.onmicrosoft.com` | Umožňuje jenom uživatelé s oprávněními pracovní/školní z určitého klienta služby Azure Active Directory a přihlaste se do aplikace.  Popisný domény název klienta Azure AD nebo identifikátor guid klienta mohou sloužit.  |

Další informace o tom, jak pracovat s těmito koncové body vyberte jeden z následujících typů určité aplikaci.

## <a name="tokens"></a>Tokeny
Verze 2.0 provádění OAuth 2.0 a připojte OpenID využívat rozsáhlé nosný tokenů, včetně nosný tokeny tvaru JWTs. Nosný token je tokenu zabezpečení lightweight, který zajišťuje přístup "nosný" chráněné zdroji. V tomto smyslu "nosný" je osobě, která Visio svádět k zahrnutí tokenu. Když na oslavu nejdřív ověřuje s Azure AD pro příjem nosný token požadované kroky nebudou přijata zabezpečit token pro předávání a úložiště, můžete zachyceny a použity před nechtěným skupinou. Během pár tokenů zabezpečení mají předdefinované mechanismus zabránit neoprávněnými osobami je používat, nosný tokeny nemusí tento postup a musí být přepravovány v zabezpečeného kanálu například transport layer zabezpečení (HTTPS). Pokud nosný token jsou přenášena v vymazat, člověka v prostředním útok lze škodlivým skupinou získat tokenu a použijte ji k neoprávněnému přístupu k chráněného zdroj. Platí stejné zásady zabezpečení při ukládání nebo ukládání do mezipaměti nosný tokeny pro pozdější použití. Vždy zajistěte, aby aplikace přenáší a ukládá nosný tokenů zabezpečení způsobem. Další otázky bezpečnosti pro na nosný tokenů najdete v článku [RFC 6750 oddílu 5](http://tools.ietf.org/html/rfc6750).

Další podrobnosti o různých typů tokeny používané v koncový bod verze 2.0 je k dispozici v [tokenu reference koncového bodu verze 2.0](active-directory-v2-tokens.md).

## <a name="protocols"></a>Protokoly

Pokud budete chtít zobrazit některé žádosti o příklad, Začínáme s některým z pod výukové programy pro.  Každý z nich odpovídá scénáři konkrétní ověřování.  Pokud potřebujete pomoc při určení, které je správné tok za vás, podívejte se na [typy aplikace, které je možné vytvářet se verze 2.0](active-directory-v2-flows.md).

- [Vytvoření Mobile, přičemž nativní aplikaci OAuth 2.0](active-directory-v2-protocols-oauth-code.md)
- [Vytvoření webové aplikace s otevřít ID připojení](active-directory-v2-protocols-oidc.md)
- [Vytvoření jedné stránky aplikací implicitní toku OAuth 2.0](active-directory-v2-protocols-implicit.md)
- [Procesy daemon sestavení nebo serveru straně procesů pomocí přihlašovacích údajů klienta OAuth 2.0 tok](active-directory-v2-protocols-oauth-client-creds.md)
- Tokeny vstoupit rozhraní API webových s OAuth 2.0 dál jménem volnosti tok (už brzo)

<!-- - Get tokens using a username & password with the OAuth 2.0 Resource Owner Password Credentials Flow (coming soon) --> 
