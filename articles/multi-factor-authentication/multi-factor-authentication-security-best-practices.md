<properties 
    pageTitle="Zabezpečení osvědčené postupy pro používání Azure MFA"
    description="Tento dokument obsahuje doporučené postupy kolem Azure MFA pomocí Azure účty"
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="security-best-practices-for-using-azure-multi-factor-authentication-with-azure-ad-accounts"></a>Zabezpečení osvědčené postupy pro používání Azure Vícefaktorové ověřování s Azure AD účty

Když přijde na vylepšení procesu ověření, vícefaktorové ověřování je upřednostňované volba pro většinu organizací jsou. Azure Multi-Factor Authentication (MFA) umožňuje společnostem požadavkům jejich zabezpečení a dodržování předpisů zároveň jednoduché přihlášení pro své uživatele. Tento článek se zabývá některé osvědčené postupy, které byste měli zvážit při plánování přijetí Azure MFA.

## <a name="best-practices-for-azure-multi-factor-authentication-in-the-cloud"></a>Doporučené postupy pro Azure Vícefaktorové ověřování v cloudu
Abyste všem uživatelům poskytnout vícefaktorové ověřování a využít výhod rozšířené funkce, které nabízí Azure Vícefaktorové ověřování, bude třeba povolit Azure Vícefaktorové ověřování na všechny vaše uživatele.  Je to provést pomocí jedné z následujících akcí:

- Azure AD Premium nebo sadu Enterprise mobilita
- Vícenásobné Auth poskytovatele

### <a name="azure-ad-premiumenterprise-mobility-suite"></a>Nastavení mobilních zařízení sadu Azure AD Premium/Enterprise

![EMS](./media/multi-factor-authentication-security-best-practices/ems.png)

Cílem prvního kroku doporučené pro přijetí MFA Azure v cloudu pomocí Azure AD Premium nebo sadu mobilita organizace je jim přiřazovat licence uživatelů.  Azure Vícefaktorové ověřování je součástí těchto sad a vaše organizace jako takové nevyžaduje nic další rozšířit funkce vícefaktorové ověřování pro všechny uživatele.

Při nastavování Vícefaktorové ověřování vzít v úvahu následující:

- Aby se vytvořil poskytovatel Auth Multi-Factor nepotřebujete.  Azure AD Premium a sadu mobilita organizace je součástí Azure Vícefaktorové ověřování.  Pokud vytvoříte poskytovatele Auth můžete získat dvojité vám účtovat poplatky.
- Pokud synchronizujete prostředí služby Active Directory místního adresáře služby Azure AD, je Azure AD Connect pouze povinné. Pokud používáte jenom Azure AD adresář, který se nesynchronizuje s instance místní služby Active Directory, nemusí Azure AD Connect.


### <a name="multi-factor-auth-provider"></a>Vícenásobné Auth poskytovatele

![Vícenásobné Auth poskytovatele](./media/multi-factor-authentication-security-best-practices/authprovider.png)

Pokud máte Azure AD Premium nebo sadu mobilita Enterprise cílem prvního kroku doporučené pro přijetí MFA Azure v cloudu je aby se vytvořil poskytovatel Auth MFA. I když MFA je k dispozici ve výchozím nastavení pro globální správce, kteří mají Azure Active Directory, při nasazení MFA pro vaši organizaci, budete muset rozšířit funkci vícefaktorové ověřování všem uživatelům a chcete je potřeba poskytovatele Auth Multi-Factor.
Když vyberete poskytovatele Auth, musíte si vyberte adresář a zvažte následující skutečnosti:

- Adresář služby Azure AD aby se vytvořil poskytovatel Auth Multi-Factor nepotřebujete.
- Je třeba poskytovatele Auth Multi-Factor přidružit adresáře služby Azure AD Pokud chcete rozšířit na všechny vaše uživatele vícefaktorové ověřování a/nebo má globální správci mohli využívat funkce, jako jsou portálu Správa vlastních pozdravy a sestavy.
- DirSync nebo AAD Sync jsou pouze požadavek, když synchronizujete prostředí služby Active Directory místního adresáře služby Azure AD. Pokud používáte jenom Azure AD adresář, který se nesynchronizuje s instance místní služby Active Directory, nepotřebujete DirSync nebo AAD Sync.
- Pokud máte Azure AD Premium nebo sadu mobilita Enterprise, nepotřebujete vytvořil poskytovatel Multi-Factor Auth. Potřebujete přiřadit licence uživatele a potom můžete začít zapnutí MFA pro uživatele.
- Zkontrolujte, že volba správné použití modelu pro svoji firmu (za ověření nebo povolené uživatele), po výběru modelu použití ji nelze změnit.

### <a name="user-account"></a>Uživatelský účet
Při aktivaci MFA pro uživatele, mohou být uživatelské účty jedním ze tří států core: zakázané, povoleno nebo nevynucují.
Pomocí níže uvedených pokynů zajistit, že používáte požadovanou nejvhodnější možnost pro nasazení:

- Až o stavu uživatele na zakázáno, tento uživatel nepoužívá vícefaktorové ověřování. Toto je ve výchozím stavu.
- Až o stavu uživatele povoleno, znamená to, že uživatel zapnuté, ale nebyla dokončení procesu registrace. Jim výzva k dokončení procesu na další přihlásit. Toto nastavení nemá vliv jiné prohlížeče. Všechny aplikace bude dál fungovat, dokud po dokončení procesu registrace.
- Až o stavu uživatele vynucený, znamená to, že uživatel může nebo nemusí dokončili registrace. Pokud jste dokončili procesu registrace používají vícefaktorové ověřování. V opačném uživatele se výzva k dokončení procesu registrace na další přihlásit. Toto nastavení vliv na jiné prohlížeče. Tato aplikace nebude fungovat vytváří a používá aplikaci hesla.
- Šablona uživatele oznámení naleznete v článku [Začínáme s Azure Vícefaktorové ověřování v cloudu](multi-factor-authentication-get-started-cloud.md) odeslat e-mailu uživatelům týkající se MFA přijetí.

### <a name="supportability"></a>Možnosti podpory

Vzhledem k tomu, že většina uživatelů zvyklí používat jenom hesla k ověření, je důležité, že vaše společnost přenést plánování informovanosti všem uživatelům týkající se tohoto procesu. Tento plánování informovanosti můžete snížit pravděpodobnost, že uživatelé budou volání technickou podporu pro menší problémům souvisejícím s MFA.
Jsou tu ale i některé scénáře, kde je nezbytné dočasně zakázání MFA. Pomocí níže uvedených pokynů pochopit, jak provádět tyto scénáře:

- Zkontrolujte, že pracovníci technické podpory vyškolení úchyt scénáře, kde na mobilní aplikace nebo telefon nechodí oznámení nebo telefonní hovor a proto, který uživatel používá nelze se přihlásit. Umožňují jednorázový přemostění možnost umožníte uživatelům ověření jednou vynecháním"" vícefaktorové ověřování. Přemostění je dočasný a vyprší po určitý počet sekund.
- V případě potřeby můžete využít funkci důvěryhodné IP adresy v Azure MFA. Tato funkce umožňuje správcům spravované nebo federované klienta možnost obejít vícefaktorové ověřování pro uživatele, kteří jsou přihlášení z místní intranet společnosti. Funkce jsou dostupné pro Azure AD klienti, které mají Azure AD Premium, Enterprise mobilita Suite nebo Azure Vícefaktorové ověřování licencí.


## <a name="best-practices-for-azure-multi-factor-authentication-on-premises"></a>Doporučené postupy pro Azure Vícefaktorové ověřování místní
Pokud vaše společnost rozhodli využíval vlastní infrastrukturu pro MFA, je nutné zajistit Azure Multi-Factor ověřování serveru místního nasazení. Součásti MFA serveru se zobrazí v diagramu pod:

![Zprostředkovatel Auth Multi-Factor](./media/multi-factor-authentication-security-best-practices/server.png)
*Nenainstalováno ve výchozím nastavení ** nainstalovanou Nepovoleno ve výchozím nastavení


Azure Multi-Factor ověřování serveru lze zajistit cloudu a místní zdroje, které jsou používána v Azure AD účty.  Ale může pouze provést pomocí federace.  To znamená musíte mít AD FS a ji federovaní vašeho klienta Azure AD.
Při nastavení serveru Multi-Factor Authentication vzít v úvahu následující:

- Pokud jste zabezpečení zdrojů Azure AD pomocí Active Directory Federation Services, pak se provádí 1st faktorem ověřování místní pomocí služby AD FS a 2 faktorem je provedena místního tak, že ctít zásady deklaraci.
- Není povinné Server Azure Multi-Factor Authentication nainstalované na server služby AD FS federace však adaptér Multi-Factor Authentication služby AD FS musí být nainstalovaná Windows Server 2012 R2 spuštěná služba AD FS. Můžete nainstalovat server na jiný počítač, dokud ho na podporovanou verzi a nainstalujte adaptér AD FS nezávisle na federačního serveru AD FS. Přečtěte si článek o postupu pod na instalaci adaptér samostatně.
- Průvodce instalací Vícefaktorové ověřování AD FS adaptér vytvoří skupinu zabezpečení s názvem PhoneFactor Admins ve službě Active Directory a přidá účet služby AD FS federace služby do této skupiny. Je vhodné je řadiče domény ověřit, že skupiny správců PhoneFactor skutečně vytvořili a že účet služby AD FS členové této skupiny. V případě potřeby přidáte účet služby AD FS ručně do skupiny správců PhoneFactor řadiče domény.

### <a name="user-portal"></a>Portál uživatele
Tento portál běží web na serveru (internetové), který umožňuje samoobslužné funkce a nabízí celou řadu a možnosti správy uživatelů. Pomocí níže uvedených pokynů pro nastavení této součásti:

- Je požadována IIS 6 nebo novější
- ASP.NET v2.0.507207 musí být nainstalovaná a registrován
- Tento server může být nasazené v síti obvod.



### <a name="app-passwords"></a>Aplikace hesla
Pokud je vaše organizace Federovaná jednotné přihlašování s Azure AD a kterou se chystáte používat Azure MFA, pak mějte na paměti z těchto kroků při používání aplikace hesel (Nezapomeňte, že to platí jenom pro federované (SSO) slouží):

- Heslo aplikace ověřený Azure AD a tedy obchází federace. Federace se používá pouze při nastavování aplikace heslo.
- Federované (SSO) budou uloženy hesla uživatele v organizační id. Pokud uživatel opustí společnosti, má tyto informace na plovoucí dlaždice na organizační id pomocí DirSync v reálném čase. Zakázání nebo odstranění účtu může trvat až 3 hodin synchronizovat, zpoždění zakázání nebo odstranění aplikace hesla v Azure AD.
- Místní nastavení řízení přístupu klienta nejsou dodržet aplikace heslem
- Místní ověřování protokolování / auditování možnost je k dispozici pro aplikaci heslo
- Další education koncových uživatelů je nutný pro klienta Microsoft Lync 2013.
- Některé rozšířené architektonických návrhů může vyžadovat kombinací organizační uživatelského jména a hesla aplikace při použití vícefaktorové ověřování s klienty, podle toho, kde jsou ověření. Pro klienty, které ověřování infrastrukturu místní byste použili organizační uživatelské jméno a heslo. Pro klienty, které ověřování Azure AD byste použili heslo aplikace.
- Ve výchozím nastavení nelze uživatelé vytvářet aplikace hesla, pokud vaše společnost vyžaduje, aby nebo v případě potřeby umožnit uživatelům vytvářet aplikace heslo v některých případech zajistit, že je vybraná možnost Povolit uživatelům vytvářet aplikace hesla se přihlásit do aplikace jiné prohlížeče.

## <a name="additional-considerations"></a>Další informace
Pomocí následujícího seznamu porozumět tomu, že některé další informace a osvědčené postupy pro jednotlivé součásti, která bude místní nasazení:

Metoda|Popis
:------------- | :------------- |
[Služba Active Directory Federation](multi-factor-authentication-get-started-adfs.md)|Informace o nastavení Azure Vícefaktorové ověřování se službou AD FS.
[RADIUS Authenticaton](multi-factor-authentication-get-started-server-radius.md)|  Informace o nastavení a konfiguraci Azure MFA Server s RADIUS.
[Ověření Internetové informační služby](multi-factor-authentication-get-started-server-iis.md)|Informace o nastavení a konfiguraci Azure MFA Server pomocí služby IIS.
[Authenticaton systému Windows](multi-factor-authentication-get-started-server-windows.md)|  Informace o nastavení a konfigurace Azure MFA Server pomocí ověřování Windows.
[Ověření protokolu LDAP](multi-factor-authentication-get-started-server-ldap.md)|Informace o nastavení a konfiguraci Azure MFA Server s ověřováním LDAP.
[Vzdálené plochy brány a Azure Multi-Factor ověřování serveru pomocí RADIUS](multi-factor-authentication-get-started-server-rdg.md)|  Informace o nastavení a konfigurace Azure MFA Server pomocí vzdálené plochy brány pomocí RADIUS.
[Synchronizace se službou Active Directory pro Windows Server](multi-factor-authentication-get-started-server-dirint.md)|Informace o nastavení a konfigurace synchronizace služby Active Directory a Azure MFA Server.
[Nasazení služby Azure Vícefaktorové ověřování serveru mobilní webové aplikace](multi-factor-authentication-get-started-server-webservice.md)|Informace o nastavení a konfiguraci webová služba Azure MFA serveru.
[Konfigurace rozšířené VPN s Azure Vícefaktorové ověřování](multi-factor-authentication-advanced-vpn-configurations.md)|Informace o konfiguraci Cisco ASA Citrix Netscaler a zabezpečené VPN Juniper/Pulse spotřebiče pomocí LDAP nebo RADIUS.


## <a name="additional-resources"></a>Další zdroje informací
I když tento článek popisuje některé osvědčené postupy pro Azure MFA, existují další zdroje, které můžete taky použít při plánování MFA nasazení. Následující seznam obsahuje některé klíčové články, které vám mohou pomoci během tohoto procesu:

- [Sestavy v Azure Vícefaktorové ověřování](multi-factor-authentication-manage-reports.md)
- [Nastavení prostředí pro Azure Vícefaktorové ověřování](multi-factor-authentication-end-user-first-time.md)
- [Nejčastější dotazy týkající se Azure Vícefaktorové ověřování](multi-factor-authentication-faq.md)
