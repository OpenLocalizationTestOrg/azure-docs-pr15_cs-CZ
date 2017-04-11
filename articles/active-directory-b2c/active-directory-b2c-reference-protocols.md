<properties
    pageTitle="Azure Active Directory B2C | Microsoft Azure"
    description="Postup vytvoření aplikace přímo pomocí protokolů podporovaných Azure Active Directory B2C."
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

# <a name="azure-ad-b2c-authentication-protocols"></a>Azure AD B2C: Protokoly ověřování

Azure Active Directory (Azure AD) B2C poskytuje identitu jako služba pro aplikace podporou dvou standardních protokolů: OpenID připojení a OAuth 2.0. Služba není kompatibilní se standardy, ale jakékoli dva implementace tyto protokoly může mít pravopisné rozdíly.  Jestliže zadejte kód přímo a po odeslání zpracování žádostí o HTTP, ne pomocí knihovny otevřít zdroj, budou informace v této příručce užitečný. Doporučujeme, abyste předtím, než se ponoříte hlouběji do podrobnosti každé určitého protokolu číst tuto stránku. Ale pokud znáte už Azure AD B2C, můžete přejít přímo na [referenční příručky protokol](#protocols).

<!-- TODO: Need link to libraries above -->

## <a name="the-basics"></a>Základní informace
Všechny aplikace, která používá Azure AD B2C potřebný k registraci v adresáři B2C [Azure portálu](https://portal.azure.com). Proces registrace aplikace shromažďuje a přiřadí několika hodnot do aplikace:

- **ID aplikace** , které jednoznačně identifikuje aplikace.
- **Přesměrovat URI** nebo **balíčku identifikátor** něhož chcete směrovat odpovědi zpátky do aplikace.
- Několik scénář specifické hodnotám. Další informace zjistěte, [jak si zaregistrovat aplikace](active-directory-b2c-app-registration.md).

Po registraci aplikace ho informuje uživatele o s Azure AD tak, že žádosti o verze 2.0 koncového bodu:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Skoro všech OAuth a připojte OpenID toků čtyři strany zahrnuje exchange:

![Role OAuth 2.0](./media/active-directory-b2c-reference-protocols/protocols_roles.png)

- **Povolení server** je koncový bod Azure AD verze 2.0. Bezpečné zpracovává cokoli související informace o uživatelích s přístupem k. Zpracuje také vztahy důvěryhodnosti mezi stranami v toku. Je zodpovědný za ověření identity uživatele, poskytnutí odvolání přístupu k prostředkům a vydání tokeny. Je označovaná taky jako poskytovatele identity.
- **Vlastníka zdroje** je obvykle koncového uživatele. Je strany, která data vlastní a nemá možnost povolit jinými výrobci pro přístup k této dat nebo zdroje.
- Aplikace je **OAuth klienta** . Je určen jeho aplikace ID. Je obvykle straně koncoví uživatelé mohli komunikovat s. Také požádá tokeny ze serveru se tak mohli ověřovat. Vlastníka zdroje musí udělit oprávnění klienta pro přístup k prostředku.
- Kde jsou uložená zdroje nebo data je **prostředků serveru** . Důvěryhodný server se tak mohli ověřovat bezpečně ověřování a povolte OAuth klienta. Taky používá nosný přístup tokeny zajistit, že můžete mít udělený přístup k prostředku.

## <a name="policies"></a>Zásady
Pravděpodobně Azure AD B2C zásad jsou nejdůležitější funkce služby. Azure AD B2C rozšiřuje standardní protokoly OAuth 2.0 a připojte OpenID zavedením zásady. Umožňují Azure AD B2C provádět mnohem víc než jednoduchý a tak mohli ověřovat. Zásady plně popisují spotř identity prostředí, včetně registrace, přihlásit a úpravy profilu. Zásady možné definovat ve správy uživatelského rozhraní. Mohou být provedeny pomocí speciálních dotazu parametr v požadavky ověřování HTTP. Zásady nejsou standardních funkcí OAuth 2.0 a OpenID připojit, proto byste měli učinit čas jejich. Další informace najdete v tématu [Azure AD B2C zásad referenční příručka](active-directory-b2c-reference-policies.md).

## <a name="tokens"></a>Tokeny
Azure AD B2C provádění OAuth 2.0 a připojte OpenID využívá rozsáhlé nosný tokenů, včetně nosný tokenů, které jsou zobrazeny jako JSON web tokeny (JWTs). Nosný token je tokenu zabezpečení lightweight, který zajišťuje přístup "nosný" chráněné zdroji. Nosný je osobě, která Visio svádět k zahrnutí tokenu. Azure AD Nejdřív musíte před dostávat nosný token ověřit strana. Ale pokud požadované kroky se dostali do zabezpečené token pro předávání a úložiště, můžete zachyceny a použity neúmyslné skupinou.

Některé tokenů zabezpečení mají předdefinované mechanismus, který brání neoprávněnými osobami je používat, ale nemáte nosný tokeny tento postup. Musí být transportovány zabezpečené kanálu, například transport layer security (HTTPS). Pokud nosný token jsou přenášena mimo zabezpečený kanál, škodlivý stran umožňuje útok člověka v – uprostřed získat tokenu a použijte ji k získání neoprávněnému přístupu k chráněného zdroj. Platí stejné zásady zabezpečení při nosný tokeny jsou uložené v mezipaměti pro pozdější použití. Vždy zajistěte, aby aplikace přenáší a ukládá nosný tokenů zabezpečení způsobem.

Důležité informace o dalších nosnému tokenu zabezpečení najdete v článku [RFC 6750 oddílu 5](http://tools.ietf.org/html/rfc6750).

Další informace o různých typů tokeny používané v Azure AD B2C jsou dostupné v [Azure AD tokenu odkaz](active-directory-b2c-reference-tokens.md).

## <a name="protocols"></a>Protokoly

Až budete chtít zkontrolovat některé požadavky příkladu, můžete začít s některým z následujících kurzy. Každý z nich odpovídá scénáři konkrétní ověřování. Pokud potřebujete pomoc při určování kterých tok je pro vás nejlepší, přečtěte si [že typy aplikací, je možné vytvářet pomocí Azure AD B2C](active-directory-b2c-apps.md).

- [Vytváření aplikací mobilní a nativní pomocí OAuth 2.0](active-directory-b2c-reference-oauth-code.md)
- [Vytvoření webové aplikace pomocí OpenID připojení](active-directory-b2c-reference-oidc.md)
