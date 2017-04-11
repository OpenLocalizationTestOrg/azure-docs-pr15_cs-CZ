<properties
    pageTitle="Použití zařízeních s Windows 10 ve vaší firmě | Microsoft Azure"
    description="Poskytuje snímek funkce pro uživatele a IT kontrastní různými způsoby zařízení lze zřízení a použít v podniku ve Windows 10."
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
    ms.date="10/17/2016"
    ms.author="femila"/>

# <a name="using-windows-10-devices-in-your-workplace"></a>Použití zařízeních s Windows 10 ve vaší firmě

Platí pro: počítače s logem Windows 10

Windows 10 nabízí tři modely v organizacích, které umožňují uživatelům přístup k práce zdroje v zabezpečené a pohodlný způsob.

- **Připojit se ke Azure Active Directory** (Azure AD spojením) pro zaměstnance, kteří mají přístup zdroje, jako jsou Office365 především v cloudu. Azure AD spojení je práce samoobslužné vytváření prezentaci, která je nového ve Windows 10.
- **Připojení k doméně**, v organizacích, které mají investic do místní aplikací a zdroje. Připojení k doméně nabízí Vylepšený ve Windows 10 při připojení k Azure AD.
- **Nové zjednodušené BYOD prostředí**pro uživatele, které chcete přidat nový účet pracovní nebo školní do Windows a snadno přistupovat k prostředkům na zařízeních s osobní.

V následující tabulce jsou uvedeny snímek funkce pro uživatele a správce IT kontrastní různými způsoby zařízení lze zřízení a použít v podniku ve Windows 10:

|                                                                                                                                                                 | Připojení k doméně     | Azure AD spojení | Osobní zařízení |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|---------------|-----------------|
| Windows zařízení pro sprá pracovní nebo školní účty.                                                                                                                      | Ano             | Ano           | Ne              |
| Uživatel jednotného přihlašování (SSO) do Office 365 a Azure AD aplikací. Jednotné přihlašování je možnost přihlašovat jenom jednou a získat organizační informace. | Ano             | Ano           | Ano             |
| Uživatel jednotné přihlašování k aplikacím Kerberos/NTLM.                                                                                                                                  | Ano             | Omezené       | Ano, přes VPN         |
| Směrový autorizace a vhodné pro sprá pracovní nebo školní účty s Microsoft Passport a Ahoj Windows.                                                                   | Ano             | Ano           | Ano             |
| Přístup k enterprise pro Windows Store pomocí pracovního nebo školního účtu (ne účet Microsoft).                                                                                    | Ano             | Ano           | Ano             |
| Vyhovujících zápisu Enterprise cestovní uživatelských nastavení na zařízeních s pracovní nebo školní účty.                                                                                 | Ano             | Ano           | Ano             |
| Možnost omezit přístup k organizační aplikací na zařízení, která jsou kompatibilní s zásady organizace zařízení.                                                      | Ano             | Ano           | Ano             |
| Uživatel samoobslužné vytváření zařízení pro "práci odkudkoliv."                                                                                                | Ne              | Ano           | Ano             |
| Možnost spravovat zařízení.                                                                                                                                       | Ano, prostřednictvím zásad skupiny/SCCM | Ano           | Ano             |

## <a name="use-work-owned-devices-with-azure-ad-join-and-domain-join-in-windows-10"></a>Použití vlastní práce zařízení s Azure AD spojení a domény připojení ve Windows 10

Windows 10 vám přináší dva způsoby práce vlastnictví zařízení pro přístup k práci zdroje:

- Azure AD spojení
- Připojení k doméně

 Jak může být platné možnosti podle potřeby a požadavky organizace. V některých případech může organizace využívat povolení obě metody nasazení.

## <a name="when-to-use-azure-active-directory-join"></a>Kdy použít připojení Azure Active Directory

Azure AD spojení je nového pracovního samoobslužné vytváření prostředí ve Windows 10.  Je určený jenom pro zaměstnanců, kteří mají přístup práce zdroje jako je třeba Office 365 primárně v cloudu. Je lehké způsob, jak konfigurovat počítačů, tablety a telefony v podniku. Zařízení se spravuje přes Správa mobilních zařízení pomocí konzistentní ovládacích prvků na platformách Windows.

**Použití Azure AD připojit se pro některé z těchto důvodů**:

- Chcete-li povolit Samoobslužná zřizování zařízení pro pracovníky na cestách.
- Můžete uživatelům poskytnout vlastní práce mobilní zařízení, jako třeba tablety a telefony.
- Chcete-li zařídit několika uživatelům Azure AD ne ve službě Active Directory, například sezónních pracovníků, dodavatelé nebo studenty.
- Chcete-li poskytnout spojování funkce zaměstnancům v vzdálené pobočky omezené místního infrastruktury.
- Nemáte na adresářová služba Active Directory.

V některých organizacích použije Azure AD připojit jako primární způsob, jak nasadit vlastní pracovní zařízení, zejména jako migrace většina nebo všechny své prostředky do cloudu. Hybridní organizace s služby Active Directory a Azure AD můžete také rozhodnout pro nasazení jeden metody odpisu nebo jiné v závislosti na uživatele nebo oddělení.

Oblasti škole a vysokých škol, například můžou spravovat bloky pro pedagogy ve službě Active Directory a studentů v Azure AD. Některé společnosti možná budete chtít spravovat vzdálené kanceláře nebo prodejů oddělení v Azure AD. Azure AD spojení a domény spojení metody lze použít v rámci organizace hybridní. Azure AD spojení může být skvělý doplněk spojení domény pro nasazení zařízení v pracovním prostředí.

**Pokud obvykle přístup k zdroje organizace, spočívá ve využití cloudu, vaše organizace může moct užít dobrý pocit další výhody Pokud**:

- Závislosti u infrastruktura místních identit, můžete odebrat.
- Nasazení modelu zařízení, můžete zjednodušit začíná od obrázků řešení tím, že Samoobslužná konfigurace.
- Správa mobilních zařízení můžete použít ke správě všech svých zařízeních pro jiné platformy.

Další informace o připojení Azure AD najdete v článku [rozšíření cloudové možnosti na zařízeních s Windows 10 až Azure Active Directory připojit se](active-directory-azureadjoin-overview.md).

## <a name="when-to-use-domain-join-or-keep-using-it"></a>Kdy použít domény spojení (nebo zachovat s ním pracovat)

Za poslední 15 roky provozu mnoho organizací používají doménu spojení připojení zařízení práce. Umožňuje uživatelům Přihlaste se k jejich zařízení se svým pracovním služby Active Directory nebo školní účty. Připojení k doméně umožňuje, aby IT centrálně a plně Správa těchto zařízení. Organizace obvykle spolehnout pro zpracování obrázků metod k poskytování zařízení a často System Center konfigurace správce (SCCM) nebo zásad skupiny (obecná) je spravovat.

**Vaše organizace používejte domény spojení (nebo zachovat s ním pracovat) některého z těchto důvodů**:

- Máte aplikace typu Win32 nasazených na těchto zařízeních, které používají NTLM/Kerberos.
- Požadujete zásad skupiny nebo SCCM/DCM zařízení můžete spravovat.
- Chcete používat pro zpracování obrázků řešení pro nastavení zařízení pro vaše zaměstnance.

**Připojení k doméně ve Windows 10 také poskytuje následující výhody při připojení k Azure AD**:

- Směrový vazbou zařízení ověřování a vhodné pro sprá pracovní nebo školní účty s Microsoft Passport a Ahoj Windows.
- Přístup k enterprise pro Windows Store pro zařízení, které používají pracovní nebo školní účty (bez účtu Microsoft povinné).
- Kompatibilní s Enterprise cestovní uživatelských nastavení na zařízeních, které používají pracovní nebo školní účty (bez účtu Microsoft povinné).
- Možnost omezit přístup k organizační aplikací na zařízení, která jsou kompatibilní s zásady organizace zařízení.

Další informace o připojení Azure AD najdete v článku [rozšíření cloudové možnosti na zařízeních s Windows 10 až Azure Active Directory připojit se](active-directory-azureadjoin-overview.md).

## <a name="enable-joining-of-personally-owned-devices-for-work-or-school"></a>Povolit připojení osobně vlastní zařízení pro práci a studium

K podpoře BYOD v organizaci, Windows 10 umožňuje uživatele k přidání pracovního nebo školního účtu na počítači, tabletu nebo telefonu. Poté, co uživatel přidá pracovního nebo školního účtu, je zařízení registrovaný u Azure AD a volitelně zaregistrované v systému správy mobilních zařízení, která má nakonfigurovanou jejich organizace. V adresáři se zobrazí tato zařízení jako "Registrovaná" porovnání Azure AD připojen. IT administraors můžete použít jiné zásady založené na tyto informace poskytující světlejší dotykového ovládání v osobně vlastní zařízení než na zařízeních s vlastní práce podle potřeby.

Uživatele můžete přidat pracovní nebo školní účet do své osobní zařízení velmi snadno. Je to při přístupu k práci aplikace poprvé nebo můžou dělat ho ručně pomocí nabídky nastavení. Tento účet poskytne jednotné přihlašování organizační zdroje.

Další informace o připojení Azure AD najdete v článku [připojení zařízení Azure AD pro Windows 10 doméně dojde](active-directory-azureadjoin-devices-group-policy.md).

## <a name="enable-domain-join-or-azure-ad-join"></a>Povolit připojení k doméně nebo Azure AD spojení

Všechny výše popsaných metod (připojení k doméně, Azure AD spojení a přidat pracovní nebo školní účet) mít vstupní body v uživatelských nastaveních Windows 10. Však všechny vyžadují správce IT pro povolení funkce infrastruktury před zkušenosti budou fungovat.

## <a name="requirements-for-deploying-azure-ad-join"></a>Požadavky pro nasazení Azure AD připojení

Nasazení připojení Azure AD pro nastavených uživatelů budete potřebovat:

- Předplatné Azure AD.
- Předplatné Azure AD Premium například mobilního zařízení Správa automatického zápisu, pokud potřebujete další možnosti.
- Správa mobilních zařízení – například Microsoft Intune předplatné, Správa mobilních zařízení pro Office 365, nebo dodavatelé správy mobilních zařízení partnera integrovaných s Azure AD. (Najdete v [části Nejčastější dotazy](#frequently-asked-questions) na konci tohoto článku Další informace).

Pokud vaše střediska hybridní, doporučujeme nasazení Azure AD Connect rozšířit místního adresáře Azure AD.

## <a name="requirements-for-using-domain-join-with-azure-ad"></a>Požadavky na používání domény spojení s Azure AD

Připojení k doméně bude dál fungovat i vždy má. Však výhody Azure AD byste potřebovali takto:

- Předplatné Azure AD.
- Nasazení Azure AD Connect rozšířit místního adresáře Azure AD.
- Pravidlo, které umožňuje doméně zařízení podmíněné přístup k Azure AD.
- Pravidlo, které umožňuje přístup k zařízení "doméně", pokud chcete omezit přístup u některých zařízení.
- Centrum pro správce konfigurace pro System verze 1509 pro Technical Preview, jak povolit pravidla pro vyžadování zařízení kompatibilní s. (Viz TechNetu si přečtěte následující dokumentaci a příspěvek blogu).

Další informace o připojení k doméně ve Windows 10 najdete v článku [dojde připojit doméně zařízení Azure AD pro Windows 10](active-directory-azureadjoin-devices-group-policy.md)

## <a name="requirements-for-using-byod-and-add-a-work-or-school-account"></a>Požadavky pro použití BYOD a "Přidání pracovního nebo školního účtu"

Chcete-li povolit "přenést vlastní zařízení" (BYOD) s pracovní nebo školní účty, budete potřebovat následující:

- Předplatné Azure AD.
- Předplatné Azure AD Premium například mobilního zařízení Správa automatického zápisu, pokud potřebujete další možnosti.

## <a name="requirements-for-using-microsoft-passport"></a>Požadavky pro práci s Microsoft Passport

Povolit Microsoft Passport, budete potřebovat:

- Veřejné klíčové infrastruktury podporu ověřování na základě certifikátu využívající Microsoft Passport.
- Předplatné Intune podporu ověřování na základě certifikátu využívající Microsoft Passport pro Azure AD připojení a pracovní nebo školní účty.
- Správce konfigurace pro System Center verze 1509 Technical Preview (podívejte se na TechNetu si přečtěte následující dokumentaci a příspěvek blogu) pro podporu ověřování na základě certifikát, který používá Microsoft Passport pro připojení k doméně.
- Zásady pro povolení Microsoft Passport v organizaci.

Jako alternativu k použití infrastruktury můžete povolit na základě klíč Microsoft Passport následujícím způsobem:

- Nasazení systému Windows Server 2016 (bez nutnosti domény nebo doménové funkční úrovně; několik řadiče domény k předávání redundance, který bude postačovat každý web služby Active Directory) řadiče domény "Výrobní náhled 1".
- Nastavení zásad Microsoft Passport povolit v organizaci.

Další informace o Microsoft Passport a Windows Ahoj ve Windows 10 najdete v článku < link-to-MS-Passport-and-Windows-Hello-document >.

## <a name="frequently-asked-questions"></a>Nejčastější dotazy
### <a name="which-partner-mobile-device-management-products-integrate-with-azure-ad"></a>Které produkty správy mobilních zařízení partnera integrace s Azure AD?

Následující produkty dodavatele integrace s Azure AD pro jednotné přihlášení a podmíněného přístupu ve Windows 10:

- AirWatch tak, že VMware
- Citrix Xenmobile
- Správce Lightspeed Mobile
- SOTI místní Správa mobilních zařízení
- MobileIron

### <a name="what-about-workplace-join-in-windows-10"></a>Na co dávat pozor na pracovišti připojení ve Windows 10?
K povolení BYOD pracovišti spojení používal ve Windows 8.1. Ve Windows 10 aktivované BYOD prostřednictvím "Přidejte pracovní nebo školní účet", jak je popsáno dříve v tomto dokumentu. Organizacích, které nechcete integrovat Azure AD jejich správa mobilních zařízení, můžete uživatelům zápisu zařízení na stránce pro správu ručně pomocí **Nastavení** > **účty** > **práce přístup**.


### <a name="can-users-connect-their-microsoft-account-to-their-domain-account-in-windows-10"></a>Můžete uživatelům připojit svým účtem Microsoft jejich doménovým účtem ve Windows 10?
Není ve Windows 10. Ve Windows 8.1 uživatelé doméně zařízení může "připojit" svým účtem Microsoft (třeba Hotmail, Live, Outlook, Xbox atd.) k jejich doménovým účtem povolit určitých prostředí jako jednotné přihlašování k živé služby, použití Windows Store a cestovní uživatelských nastavení na zařízeních. Ve Windows 10 byla vyřadit "připojit" funkce účtu Microsoft. Uživatele můžete přidat jeden nebo více účtů Microsoft jako dalších účtů povolit jednotného přihlašování pro spotř služeb, jako je Windows Storu. Důvodem je v **dialogovém okně Nastavení** > **účty** > **účtu**.

Uživatele, kteří upgradu z Windows 8.1 doméně zařízení a kteří měli svým účtem Microsoft připojení, bude mít automaticky přidají do seznamu dalších účtů, které používají jejich připojeného účtu Microsoft.


## <a name="additional-information"></a>Další informace
* [Windows 10 Enterprise: způsoby použití zařízení pro práci](active-directory-azureadjoin-windows10-devices-overview.md)
* [Rozšíření možností cloudu na zařízeních s Windows 10 až Azure Active Directory připojit se ke](active-directory-azureadjoin-user-upgrade.md)
* [Další informace o použití scénáře pro Azure AD připojení](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Připojte zařízení doméně Azure AD pro Windows 10 prostředí](active-directory-azureadjoin-devices-group-policy.md)
* [Nastavení připojení Azure AD](active-directory-azureadjoin-setup.md)
