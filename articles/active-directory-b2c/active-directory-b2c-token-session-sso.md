<properties
    pageTitle="Azure Active Directory B2C: Token relace a jednotné přihlašování | Microsoft Azure"
    description="Token, relace a přihlašování jednotné v Azure Active Directory B2C"
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

# <a name="azure-active-directory-b2c-token-session-and-single-sign-on-configuration"></a>Azure Active Directory B2C: Token relace a jednotné přihlašování

Tato funkce umožňuje jemně odstupňovaná kontrolu v [základna za zásad](active-directory-b2c-reference-policies.md), a od:
 
1. Životnost tokenů zabezpečení, které B2C Azure Active Directory (Azure AD).
2. Životnost spravuje Azure AD B2C webové aplikace relace.
3. Jednotné přihlašování (SSO) chování napříč několika aplikace a zásady klienta B2C.

Tato funkce slouží ve vašem klientovi B2C následujícím způsobem:

1. Postupujte podle těchto kroků [přejděte na zásuvné funkce B2C](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) na portálu Azure.
2. Klikněte na **Sign in Zásady**. *Poznámka: můžete tuto funkci na libovolný typ zásad nejen na* *Zásady přihlašovací***.
3. Otevřete zásadu kliknutím na něj. Například klikněte na **B2C_1_SiIn**.
4. Klikněte na **Upravit** v horní části zásuvné.
5. Klikněte na **Token, relace a konfigurace přihlašování**.
6. Proveďte požadované změny. Informace o dostupných vlastností v následujících částech.
7. Klikněte na **OK**.
8. Nahoře zásuvné klikněte na **Uložit** .

![Snímek obrazovky s token, relace a konfigurace přihlašování](./media/active-directory-b2c-token-session-sso/token-session-sso.png)

## <a name="token-lifetimes-configuration"></a>Konfigurace tokenu životnost

Azure AD B2C podporuje [protokol se tak mohli ověřovat OAuth 2.0](active-directory-b2c-reference-protocols.md) pro povolení zabezpečeného přístupu k chráněným prostředkům. Provádět toto odborné pomoci posílá Azure AD B2C různých [tokenů zabezpečení](active-directory-b2c-reference-tokens.md). Jedná se o vlastnosti, které můžete použít ke správě Životnost tokenů zabezpečení, které Azure AD B2C:

- **Přístup a ID tokenu životnost (v minutách)**: životnost nosný token OAuth 2.0 používá k získání přístupu k chráněného zdroj. Azure AD B2C problémy pouze ID tokeny v současné době. Tato hodnota by vyrovnat přístupové tokeny také při přičteme podpory pro ně.
   - Výchozí = 60 minut.
   - Minimální (včetně) = 5 minut.
   - Maximální (včetně) = 1440 minut.
- **Životnost tokenů aktualizace (dní)**: maximální časové období, před který token aktualizace lze získat nové access nebo ID token (a volitelně nové aktualizace token, pokud byl udělen aplikace `offline_access` rozsah).
   - Výchozí = 14 dní.
   - Minimální (včetně) = 1 den.
   - Maximální (včetně) = 90 dní.
- **Aktualizace token klouzavé okno životnost (dní)**: po tohoto časového období má uživatel bude muset znovu ověřit, bez ohledu platnosti nejnovější aktualizace token získali aplikací. Ho může být k dispozici pouze pokud přepínačem nastavena na **Bounded**. Musí být větší než nebo rovno **Životnost tokenů aktualizace (dní)** hodnotu. Pokud přepínač je nastavena na **Unbounded**, nemůže zajistit určité hodnoty.
   - Výchozí = 90 dní.
   - Minimální (včetně) = 1 den.
   - Maximální (včetně) = 365 dní.

Toto jsou několik případy použití umožňující pomocí tyto vlastnosti:

- Umožňuje povolte uživateli dodržovat předpisy podepsané v mobilní aplikaci donekonečna udržovat, dokud se u něho neustále aktivní v aplikaci. Můžete to udělat pomocí nastavení **aktualizace Životnost tokenů posuvná okna (dní)** přepněte do **Unbounded** svoje přihlašovací zásady.
- Zahájit vaše odvětví zabezpečení a dodržování předpisů požadavky nastavením tokenu životnost odpovídající přístup.

## <a name="session-configuration"></a>Konfigurace relací

Azure AD B2C podporuje [ověřování protokolem OpenID připojení](active-directory-b2c-reference-oidc.md) pro povolení zabezpečené přihlášení do webových aplikací. Jedná se o vlastnosti, které můžete použít ke správě webových aplikací relací:

- **V prohlížeči životnost relace (v minutách)**: životnost cookie relace Azure AD B2C uložené v prohlížeči uživatele po úspěšném ověření.
   - Výchozí = 1440 minut.
   - Minimální (včetně) = 15 minut.
   - Maximální (včetně) = 1440 minut.
- **Časový limit relace webové aplikace**: Pokud tento přepínač je nastavený na **absolutní**, uživatel bude muset znovu ověřit za časové období nastavil uplyne **v prohlížeči životnost relace (v minutách)** . Pokud tento přepínač je nastavený **kolejová** (výchozí nastavení), zůstane uživatel přihlášený, dokud uživatel neustále aktivní ve webové aplikaci.

Toto jsou několik případy použití umožňující pomocí tyto vlastnosti:

- Požadavkům vaše odvětví zabezpečení a dodržování předpisů nastavením příslušné webové aplikace relace životnost.
- Vynucení nové ověřování po určité době nastavovat čas při interakci uživatele s vysokou úroveň zabezpečení část webové aplikace. 

## <a name="single-sign-on-sso-configuration"></a>Konfigurace jednotného přihlašování (SSO)

Pokud máte více aplikací a zásady klienta B2C, můžete spravovat interakcí uživatele v nich pomocí vlastnosti **jednotné přihlašování** . Nastavte vlastnost na jednu z následujících možností:

- **Klienta**: Toto je ve výchozím nastavení. Toto nastavení připojíte více aplikací a zásady klienta B2C sdílet stejnou relace uživatele. Například po uživatel přihlásí k aplikaci, nákupní Contoso, kterými váš kolega můžete taky Bezproblémová Přihlaste se do jiného jedné farmacie Contoso při přístupu.
- **Aplikace**: umožňuje spravovat relace uživatele výhradně pro aplikaci nezávisle na ostatních aplikací. Například pokud chcete uživateli se přihlásit k Contoso farmacie (plus pár stejné přihlašovací údaje), i když se u něho se už přihlásili k nákupu Contoso, jiné aplikace na stejném B2C klienta. 
- **Zásady**: umožňuje spravovat relace uživatele výhradně pro zásady nezávislé aplikací s ním pracovat. Například pokud uživatel již přihlášeni a dokončení kroku více faktor ověřování (MFA), kterými váš kolega může být zadán přístup k částem vyšší zabezpečení více aplikací, dokud relace se zásada stejným nevyprší.
- **Zakázané**: To přinutí uživateli projít celý uživatele cesty na každé spuštění zásadu. Například to vám umožní více uživatelů se přihlásit do aplikace (ve scénáři sdílené plochy), dokonce i při jednoho uživatele zůstane přihlášený po celou dobu.
