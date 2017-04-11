<properties
    pageTitle="B2C Azure Active Directory: Přehled | Microsoft Azure"
    description="Vývoj spotř webových aplikací s Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-sign-up-and-sign-in-consumers-in-your-applications"></a>Azure Active Directory B2C: Registrace a přihlaste se uživatelé v aplikacích

Azure Active Directory B2C je řešení správy identit komplexní cloudu pro váš web vystavený příjemce a mobilní aplikace. Je velmi dostupné globální služba, která rozšiřuje stovky miliony identit příjemce. Založená na podnikové platformy, Azure Active Directory B2C uchovává aplikace, máte ve firmě a uživatele chráněné.

V minulosti vývojáře aplikací, kteří chtěli zaregistrovat a přihlaste se do svých aplikací spotřebitele by vytvořilo vlastní kód. A má mít používají místní databáze nebo systémů pro ukládání uživatelská jména a hesla. Azure Active Directory B2C umožňuje vývojářům lepší integrace Správa identit příjemce do svých aplikací s pomocí zabezpečené a standardy platformu a celá řada extensible zásady. Při použití Azure Active Directory B2C vaše uživatelé můžou zaregistrovat ke aplikace pomocí své existující účty sociálních (Facebook Google, Amazon, Linkedinu) nebo vytváření nových přihlašovacích údajů (e-mailovou adresu a heslo, nebo uživatelské jméno a heslo); Název druhém "místní účty."

## <a name="get-started"></a>Začínáme

Vytvářet aplikace, která přijímá spotř registrace a přihlášení, kterou budete první potřeba při registraci aplikace Azure Active Directory B2C klient. Pokud potřebujete vlastního klienta pomocí kroků uvedených v tématu [Vytvoření Azure AD B2C klienta](active-directory-b2c-get-started.md).

Aplikace proti služby Azure Active Directory B2C můžete vytvořit výběrem k odesílání zpráv protokol přímo pomocí verze [OAuth 2.0](active-directory-b2c-reference-protocols.md#oauth2-authorization-code-flow) nebo [Otevřené připojení ID](active-directory-b2c-reference-protocols.md#openid-connect-sign-in-flow), nebo pomocí naše knihoven, a to postará za vás. Vyberte svoji Oblíbené platformu v následující tabulce a můžete začít.

[AZURE.INCLUDE [active-directory-b2c-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]

## <a name="whats-new"></a>Co je nového

Přečtěte si zpět možná často se naučit používat budoucí změny Azure Active Directory B2C. Budeme se také tweet o nějaké aktualizace pomocí @AzureAD.

- Informace o naše [framework extensible zásad](active-directory-b2c-reference-policies.md) a o typech zásad, které můžete vytvářet a používat v aplikacích.
- Vytvořte si záložku pro upozornění na problémy se službou menší, aktualizace, stav a mitigations našeho [blogu služby](https://blogs.msdn.microsoft.com/azureadb2c/) . Pokračujte ve sledování [řídicí panel stavu Azure](https://azure.microsoft.com/status/) i.
- Aktuální [omezení služeb, omezení a omezení](active-directory-b2c-limitations.md).
- Nakonec [příkladu](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-b2c) použití Azure AD B2C & základní ASP.NET.

## <a name="how-to-articles"></a>Postupy

Naučte se používat specifických funkcí Azure Active Directory B2C:

- Konfigurace [služby Facebook](active-directory-b2c-setup-fb-app.md), [Google +](active-directory-b2c-setup-goog-app.md), [účtu Microsoft](active-directory-b2c-setup-msa-app.md), [Amazon](active-directory-b2c-setup-amzn-app.md)a [Linkedinu](active-directory-b2c-setup-li-app.md) účtů pro použití v spotř webových aplikacích.
- [Použití vlastní atributy shromažďování informací o uživatele](active-directory-b2c-reference-custom-attr.md).
- [Povolení Azure Vícefaktorové ověřování spotř webových aplikacích](active-directory-b2c-reference-mfa.md).
- [Nastavení samoobslužné resetování hesla pro uživatele](active-directory-b2c-reference-sspr.md).
- [Přizpůsobit vzhled a chování registrace přihlášení a dalších spotř webových stránek](active-directory-b2c-reference-ui-customization.md) , které jsou doručeny Azure Active Directory B2C.
- [Použití Azure Active Directory grafu rozhraní API programově vytvořit, číst, aktualizovat, a odstraňovat spotřebitele](active-directory-b2c-devquickstarts-graph-dotnet.md) Azure Active Directory B2C klientovi.

## <a name="next-steps"></a>Další kroky

Tyto odkazy mohou být užitečné pro prohlížení služby na hloubkové ose:

- Přečtěte si [informace o cenách Azure Active Directory B2C](https://azure.microsoft.com/pricing/details/active-directory-b2c/).
- Nápovědu k přetečení zásobníku pomocí [azure active directory](http://stackoverflow.com/questions/tagged/azure-active-directory) nebo [adal](http://stackoverflow.com/questions/tagged/adal) značky.
- Sdělte nám své myšlenky pomocí [Uživatele hlasovou](https://feedback.azure.com/forums/169401-azure-active-directory/)– chceme se jim! Použijte frázi "AzureADB2C:" v názvu svůj příspěvek, abychom ho našli.
- Prohlédněte si [Azure AD B2C protokol odkaz](active-directory-b2c-reference-protocols.md).
- Prohlédněte si [Azure AD B2C tokenu odkaz](active-directory-b2c-reference-tokens.md).
- Přečtěte si [FAQ B2C Azure Active Directory](active-directory-b2c-faqs.md).
- [Soubor podporují požadavky pro Azure Active Directory B2C](active-directory-b2c-support.md).

## <a name="get-security-updates-for-our-products"></a>Aktualizace zabezpečení pro naše produkty

Budeme rádi, když chcete-li získat oznámení o Pokud zabezpečení incidentem dojde k [této](https://technet.microsoft.com/security/dd252948) stránce a přihlášení k odběru poradní výstrah zabezpečení.
