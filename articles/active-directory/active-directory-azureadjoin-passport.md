<properties
    pageTitle="Ověření identity bez hesla prostřednictvím Microsoft Passport | Microsoft Azure"
    description="Přehled Microsoft Passport a dalších informací o nasazení Microsoft Passport."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="authenticating-identities-without-passwords-through-microsoft-passport"></a>Ověření identity bez hesla prostřednictvím Microsoft Passport

Aktuální metody ověřování pomocí hesel samotný nejsou dostatečné a zabezpečit uživatelů. Uživatelé znovu použít a zapomenete heslo. U hesel se breachable, phishable chybám praskliny a uhodnutelných. Taky získat těžké si zapamatovat a chybám útokům jako "[předat algoritmus hash](https://technet.microsoft.com/dn785092.aspx)".

## <a name="about-microsoft-passport"></a>O Microsoft Passport
Microsoft Passport je soukromá/veřejný klíč nebo ověřování na základě certifikát přístup pro organizace a spotřebitele, která přesahuje hesla. Tento způsob ověřování závisí na pár klíčů přihlašovací údaje, které můžete nahradit hesla a jsou proti narušením, krádeže a útoky phishing.

 Microsoft Passport umožňuje uživateli ověření účtu Microsoft, účtem služby Active Directory pro Windows Server, účet Microsoft Azure Active Directory (Azure AD) nebo služby společnosti Microsoft, který podporuje ověřování rychlé IDentity Online (FIDO). Po počáteční dvoustupňového ověření během Microsoft Passport zápisu Microsoft Passport nastavenou na zařízení uživatele a uživatel zadá gesto, který může být Windows Dobrý den nebo PIN kódu. Uživatel zadá gesto ověřte identitu. Windows pomocí Microsoft Passport ověření uživatele a pomůže jim přístup ke chráněné zdrojů a služeb.

Privátním klíčem je k dispozici pouze prostřednictvím "gesto uživatele" jako PIN kódu, biometrika nebo vzdáleného zařízení jako čipové karty, který uživatel používá se přihlásit do zařízení. Tyto informace se propojí certifikát nebo asymetrické pár klíče. Privátním klíčem je hardware potvrzené má-li zařízení čipové důvěryhodné Platform Module (TPM). Soukromý klíč nikdy ponechá zařízení.

Veřejný klíč máte zaregistrované v rámci služby Azure Active Directory a Windows Server Active Directory (pro místní). Zprostředkovatelé identit jiní (IDPs) ověření uživatele mapováním veřejným klíčem uživatele na privátním klíčem a zadejte přihlašovací údaje pomocí hesla jeden čas (OTP), PhoneFactor nebo mechanismus různých oznámení.

## <a name="why-enterprises-should-adopt-microsoft-passport"></a>Proč podniky přijmout Microsoft Passport

Povolením Microsoft Passport podniky můžete jejich zdroje ještě lépe zabezpečit tak, že:

* Nastavení Microsoft Passport s možností upřednostňované hardwaru. To znamená, že klíče vygeneruje na TPM 1.2 nebo TPM 2.0-li k dispozici. Když TPM není k dispozici, software generování klíče.

* Definování složitostí i délky PIN kódu, a zda je povoleno Ahoj použití ve vaší organizaci.

* Konfigurace Microsoft Passport podporují čipové karty profesionálové scénáře pomocí založené na certifikátu zabezpečení.

## <a name="how-microsoft-passport-works"></a>Jak funguje Microsoft Passport
1. Klíčů na hardware TPM nebo softwaru. Mnoha zařízeních mít předdefinované Čipová TPM, který zabezpečuje hardware integrací klíče do zařízení. TPM 1.2 nebo TPM 2.0 vygeneruje klíčů nebo certifikáty, které jsou vytvořené z generovaného klíče.

2. TPM osvědčuje klávesy vazbou hardwaru.

3. Gesto jednoho odemknout odemknou zařízení. Tato gesta umožňuje přístup k více zdrojů-li zařízení doméně nebo Azure AD připojen.

## <a name="how-the-microsoft-passport-lifecycle-works"></a>Jak funguje Microsoft Passport cyklus

![Microsoft Passport cyklus](./media/active-directory-azureadjoin/active-directory-azureadjoin-microsoft-passport.png)

Předchozí diagram znázorňuje pár klíčů soukromá/veřejná a ověření zprostředkovatelem identit. Každý z těchto kroků se podrobně tady:

1. Uživatel ukáže identitu prostřednictvím více předdefinované kontroly pravopisu a gramatiky metod (gesta, fyzické čipové karty, vícefaktorové ověřování) a odesílá tyto informace do Identity poskytovatele (IDP) jako Azure Active Directory nebo místní služby Active Directory.

2. Zařízení vytvoří klíč, osvědčuje klávesu, trvá veřejné části tohoto klíče, připojí s příkazy stanice, přihlášení a odešle IDP zaregistrovat klávesu.

4. Hned IDP zaregistruje veřejné část klíče, IDP přijal se pokusí zařízení, aby se přihlaste pomocí soukromé část klíče.

5. IDP pak ověří záležitostí a token ověřování, který umožňuje uživatele a zařízení přístup chráněné prostředky. IDPs můžete napsat platformy, aplikací nebo použití podpory prohlížeče (přes JavaScript/Webcrypto rozhraní API) umožňuje vytvářet a používat Microsoft Passport přihlašovací údaje pro své uživatele.

## <a name="the-deployment-requirements-for-microsoft-passport"></a>Požadavky na nasazení Microsoft Passport
### <a name="at-the-enterprise-level"></a>Na úrovni podniku

* Organizace má Azure předplatné.

### <a name="at-the-user-level"></a>Na úrovni uživatele

* Počítači uživatele systémem Windows 10 Professional nebo Enterprise.

Podrobné pokyny najdete v článku [Povolení Microsoft Passport pro práci v organizaci](active-directory-azureadjoin-passport-deployment.md).


## <a name="additional-information"></a>Další informace

* [Windows 10 Enterprise: způsoby použití zařízení pro práci](active-directory-azureadjoin-windows10-devices-overview.md)
* [Rozšíření možností cloudu na zařízeních s Windows 10 až Azure Active Directory připojit se ke](active-directory-azureadjoin-user-upgrade.md)
* [Další informace o použití scénáře pro Azure AD připojení](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Připojte zařízení doméně Azure AD pro Windows 10 prostředí](active-directory-azureadjoin-devices-group-policy.md)
* [Nastavení připojení Azure AD](active-directory-azureadjoin-setup.md)
