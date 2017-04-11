<properties
    pageTitle="Azure Active Directory B2C: Omezení a omezení | Microsoft Azure"
    description="Seznam omezení a omezení s Azure Active Directory B2C"
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
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-limitations-and-restrictions"></a>Azure Active Directory B2C: Omezení a omezení

Existuje několik funkcí a funkcí B2C Azure Active Directory (Azure AD), které zatím nejsou podporované. Tyto známé problémy a omezení v mnoha bude odesláno přecházející do dalšího období, ale je třeba si uvědomit z nich Pokud vytváříte spotř webových aplikací používajících Azure AD B2C.

## <a name="issues-during-the-creation-of-azure-ad-b2c-tenants"></a>Problémy při vytváření Azure AD B2C klientů

Pokud dojde k potížím při [vytváření Azure AD B2C klienta](active-directory-b2c-get-started.md), naleznete v tématu [Vytvoření Azure AD klienta nebo Azure AD B2C klienta – problémů a řešení](active-directory-b2c-support-create-directory.md) pokyny.

Všimněte si, že jsou známé problémy při odstranění stávajícímu klientovi B2C a opětovné vytvoření se stejným názvem domény. Budete muset vytvoření klienta B2C s jiným názvem domény.

## <a name="note-about-b2c-tenant-quotas"></a>Všimněte si o kvótách B2C klienta

Ve výchozím nastavení se počet uživatelů v klientovi B2C omezí na více než 50 000 uživatelů. Pokud potřebujete vysokoškolskou úroveň kvóty B2C klienta, by měl kontaktujte podporu.

## <a name="branding-issues-on-verification-email"></a>Problémy brandingu na ověření e-mailech

Výchozí ověřovací e-mail obsahuje Microsoft branding. Jsme odeberete ho v budoucnu. Nyní můžete ho odebrat pomocí [funkce brandingu společnosti](../active-directory/active-directory-add-company-branding.md).

## <a name="restrictions-on-applications"></a>Omezení aplikace

Následující typy žádostí o nepodporuje aktuálně Azure AD B2C. Popis podporované typy aplikací najdete v příručce [Azure AD B2C: typy aplikací](active-directory-b2c-apps.md).

### <a name="single-page-applications-javascript"></a>Jedna stránka aplikace (JavaScript)

Mnoho moderní aplikací mít jedné stránce aplikace (zabezpečené ověřování HESLA) front-end vzniku především JavaScript a často používá zabezpečené ověřování HESLA framework například AngularJS Ember.js, Durandal, atd. Tento vývojový zatím není k dispozici v Azure AD B2C.

### <a name="daemons--server-side-applications"></a>Procesy daemon / serverové aplikace

Aplikace, které obsahují dlouhodobě spuštěných procesů nebo vzorců bez informací o stavu uživatele taky potřebují způsob, jak získat přístup k zabezpečené zdroje, jako je třeba rozhraní API webových. Tyto aplikace můžete ověřit a získat tokeny pomocí identitu aplikace (spíše než delegované identita uživatele) v [toku pověření klienta OAuth 2.0](active-directory-b2c-reference-protocols.md#oauth2-client-credentials-grant-flow). Tento vývojový ještě není k dispozici v Azure AD B2C, abyste prozatím aplikací mohli tokeny až po došlo k interaktivní spotř přihlašovací toku.

### <a name="standalone-web-apis"></a>Rozhraní API samostatného Web

V Azure AD B2C máte možnost [vytvořit rozhraní API webových, která je zabezpečená pomocí tokeny OAuth 2.0](active-directory-b2c-apps.md#web-apis). Však této rozhraní API webových pouze budou moct přijímat tokenů z klienta, který sdílí stejné aplikace ID. Vytváření webového rozhraní API, které je přístupný z několika různých klientů není podporovaná.

### <a name="web-api-chains-on-behalf-of"></a>Webového rozhraní API Chains (řetězy) (na-jménem-Of)

Mnoho architektury zahrnout rozhraní API webových, kterou je potřeba zavolat další podřízené rozhraní API webových, obě zajištěná Azure AD B2C. Tento postup je běžný nativní klienti, které mají rozhraní API webových back-end, která volá online služby společnosti Microsoft, třeba rozhraní API Azure AD grafu.

Tento scénář zřetězené rozhraní API webových můžete podporovaná pomocí udělení OAuth 2.0 Jwt nosný pověření, jinak jmenoval toku dál-jménem z. Však toku dál-jménem z není součástí aktuálně Azure AD B2C.

## <a name="restriction-on-libraries-and-sdks"></a>Omezení knihovny a SDK

Sada Microsoft podporované knihoven, které fungují Azure AD B2C je velmi omezený v současné době. Máme pro .NET na základě webové aplikace a služby, jakož i NodeJS webové aplikace a služby podpory.  Máme také knihovně náhled .NET klienta jmenoval MSAL, kterou budete moct použít s Azure AD B2C ve Windows a dalších aplikací .NET.

Knihovna podporují všechny jazyky nebo platformy, včetně iOS a Android aktuálně nemáme.  Pokud chcete vytvořit různé platformy než výše uvedených, doporučujeme používat SDK otevřít zdroj, odkazující na naši stránku věnovanou [OAuth 2.0 a funkce pro odkazy protokol připojení OpenID](active-directory-b2c-reference-protocols.md) podle potřeby.  Azure AD B2C používá OAuth & OpenID připojení, které vám umožní používá obecný knihovnu OAuth nebo OpenID připojení pro integraci.

Náš iOS a Android úvodní výuka pomocí otevřít zdroj knihoven, které jsme otestovali kompatibility s Azure AD B2C.  Všechny naše rychlého kurzy jsou dostupné v části naše [Začínáme](active-directory-b2c-overview.md#getting-started) .

## <a name="restriction-on-protocols"></a>Omezení protokoly

Azure AD B2C podporuje připojení OpenID a OAuth 2.0. Ale ne všech funkcí a možností každý protokol provedena. Lépe porozumět tomu obor funkčnosti podporovaný protokol v Azure AD B2C, prostudujte si naše [OpenID připojení a funkce pro odkazy protokol OAuth 2.0](active-directory-b2c-reference-protocols.md). Podpora protokolu SAML a WS Fed není k dispozici.

## <a name="restriction-on-tokens"></a>Omezení tokenů

V mnoha tokenů vydán Azure AD B2C implementovaná formátu JSON Web tokeny nebo JWTs. Ne všechny informace obsažené v JWTs (označované jako "deklarací") je však úplně, jako by měla být nebo není k dispozici. Příklady "sub" a "preferred_username" deklarované.  Jako hodnoty, formát nebo smyslu deklarací změn v průběhu času pro existující zásady tokeny zůstane nemá vliv: můžete spolehnout na jejich hodnoty v aplikacích výroby.  Jak změnit hodnoty, můžeme vám umožní možnost konfigurace tyto změny u každé vaše zásady.  Pochopíte lépe tokeny vyzařovaného aktuálně službou Azure AD B2C, přečtěte si prostřednictvím naše [tokenu odkaz](active-directory-b2c-reference-tokens.md).

## <a name="restriction-on-nested-groups"></a>Omezení vnořené skupiny

Členství ve skupinách vnořené nejsou podporované v Azure AD B2C klienti. Plánujeme není přidáte tuto možnost.

## <a name="restriction-on-differential-query-feature-on-azure-ad-graph-api"></a>Omezení funkce rozdílné dotazu na rozhraní API Azure AD grafu

[Funkce rozdílné dotazu na rozhraní API Azure AD grafu](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-differential-query) není podporována v Azure AD B2C klienti. Toto je na naše dlouhodobé Přehled.

## <a name="issues-with-user-management-on-the-azure-classic-portal"></a>Problémy s Správa uživatelů na portálu Azure klasické

Funkce B2C jsou k dispozici na portálu Azure. Portál Azure klasické však může použít pro přístup k dalších funkcí klienta, včetně správy uživatelů. Momentálně je několik známých problémů s Správa uživatelů (karta **Uživatelé** ) na portálu Azure klasické:

- Pro uživatele místního účtu (například příjemce kdo zaregistruje s e-mailovou adresu a heslo, nebo uživatelské jméno a heslo) pole **Uživatelské jméno** neodpovídá přihlašovací identifikátor (e-mailovou adresu nebo uživatelské jméno), který byl používaný během registrace. Důvodem je pole zobrazené na portálu Azure klasické je skutečně uživatele hlavní název (UPN), které se nepoužívá v B2C scénáře. Identifikátor přihlašovací místního účtu zobrazíte najdete objektu uživatele v [Průzkumníkovi grafu](https://graphexplorer.cloudapp.net/). Zjistíte stejný problém s uživatelem sociální účtu (například příjemce kdo zaregistruje s Facebook, Google + atd.), ale v takovém případě se žádný identifikátor přihlášení k hovoru z.

    ![Místní účet – UPN](./media/active-directory-b2c-limitations/limitations-user-mgmt.png)

- Pro uživatele místního účtu se dozvíte, jak není možné upravovat žádné z polí a uložte změny na kartě **profil** .

## <a name="issues-with-admin-initiated-password-reset-on-the-azure-classic-portal"></a>Problémy s inicializovaný správce na portálu Azure klasické pro vytvoření nového hesla

Pokud resetování hesla pro místní spotř účet založený na portálu Azure klasické (příkaz **Resetovat heslo** na kartě **uživatele** ), které spotř nebude moct změnit heslo na další přihlásit, pokud vyčerpat přihlášení nebo přihlášení zásady a nebude možné odhlášení z aplikace. Jako alternativu pomocí rozhraní [API Azure AD grafu](active-directory-b2c-devquickstarts-graph-dotnet.md) resetování hesla příjemce (bez vypršení platnosti hesla) nebo vyčerpat přihlásit zásad místo přihlášení nebo přihlášení zásad.

## <a name="issues-with-creating-a-custom-attribute"></a>Problémy s vytvářením vlastních atributů

[Vlastní atribut přidali na portálu Azure](active-directory-b2c-reference-custom-attr.md) nevytvoří okamžitě ve vašem klientovi B2C. Budete muset použít vlastní atribut v alespoň jedno z zásady pro ně získat vytvořené ve vašem klientovi B2C a budou dostupné prostřednictvím rozhraní API grafu.

## <a name="issues-with-verifying-a-domain-on-the-azure-classic-portal"></a>Problémy s ověřením domény na portálu Azure klasické

Aktuálně nemůže ověřit doménu úspěšně na [Azure klasické portálu](https://manage.windowsazure.com/).

## <a name="issues-with-sign-in-with-mfa-policy-on-safari-browsers"></a>Problémy s přihlášením zásadám MFA v prohlížečích Safari

Požadavky na selhalo přihlašovací zásady (MFA zapnutá): někdy v prohlížečích Safari s chybami HTTP 400 (Chybný požadavek). Toto je termín splnění: limity velikosti souborů cookie zhoršeným Safari na. Existuje několik řešení tohoto problému:

- Použijte "registrace nebo přihlašovací zásady" místo "zásad přihlašovací".
- Snižte počet **deklarace aplikace** požadovány v svoje zásady.
