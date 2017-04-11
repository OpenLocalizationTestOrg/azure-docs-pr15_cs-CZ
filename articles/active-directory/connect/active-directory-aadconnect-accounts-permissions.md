<properties
   pageTitle="Azure AD Connect: Účty a oprávnění | Microsoft Azure"
   description="Toto téma popisuje účty používá a vytvořili a informace o oprávněních požadovaných."
   services="active-directory"
   documentationCenter=""
   authors="billmath"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"  
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="10/04/2016"
   ms.author="billmath"/>


# <a name="azure-ad-connect-accounts-and-permissions"></a>Azure AD Connect: Účty a oprávnění
Průvodce instalací Azure AD Connect vám přináší dva různé cesty:

- Ve skupinovém rámečku Nastavení Express Průvodce vyžaduje další oprávnění tak, aby ho můžete nastavit konfiguraci snadno, aniž by bylo potřeba vytvářet uživatele nebo konfiguraci oprávnění samostatně.

- V dialogovém okně Vlastní nastavení Průvodce nabízí další možnosti a možnosti, ale existují určité situace, ve kterých je třeba zajistit, že nemáte správná oprávnění sami.

## <a name="related-documentation"></a>Související si přečtěte následující dokumentaci
Pokud jste si v dokumentaci [Integrace místních identit s Azure Active Directory](../active-directory-aadconnect.md), následující tabulka obsahuje odkazy na související témata.

Téma |  
--------- | ---------
Instalace pomocí nastavení Express | [Expresní instalace Azure AD Connect](active-directory-aadconnect-get-started-express.md)
Instalace pomocí vlastní nastavení | [Vlastní instalace Azure AD Connect](active-directory-aadconnect-get-started-custom.md)
Upgrade z DirSync | [Upgrade z synchronizační nástroj služby Azure AD (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md)


## <a name="express-settings-installation"></a>Expresní nastavení instalace
Ve skupinovém rámečku Nastavení Express Průvodce instalací zeptá přihlašovacích údajů správce organizace AD DS tak, aby se požadovaná oprávnění pro Azure AD Connect možné konfigurovat na adresářová služba Active Directory. Pokud upgradujete z DirSync, AD DS podnikový správce přihlašovacích údajů se používají k resetování hesla pro účet používaný DirSync. Budete potřebovat Azure AD globální správce přihlašovacích údajů.

Stránka Průvodce  | Shromažďované přihlašovací údaje | Informace o oprávněních požadovaných| Použít pro
------------- | ------------- |------------- |-------------
NENÍ K DISPOZICI|Uživatel spouštějící Průvodce instalací| Správce místního serveru| <li>Vytvoří místního účtu, který slouží jako [účet služby sync engine](#azure-ad-connect-sync-service-account).
Připojení k Azure AD| Přihlašovací údaje k directory Azure AD | Role globálního správce v Azure AD | <li>Povolení synchronizace v adresáři Azure AD.</li>  <li>Vytvoření [účtu Azure AD](#azure-ad-service-account) používanou pro operace synchronizace probíhajících v Azure AD.</li>
Připojení k službě AD DS | Místní přihlašovací údaje k Active Directory | Člen skupiny Správci Enterprise (EA) ve službě Active Directory| <li>Vytvoří [účtu](#active-directory-account) ve službě Active Directory a udělit oprávnění k němu. Vytvoření účtu používá se pro čtení a zápis informace při synchronizaci adresářů.</li>

### <a name="enterprise-admin-credentials"></a>Přihlašovací údaje správce organizace
Těchto přihlašovacích údajů se používají pouze v průběhu instalace a používají po dokončení instalace. Je správce organizace a ne Domain Admin, aby zkontrolovala, jestli že ve všech doménách lze nastavit oprávnění ve službě Active Directory.

### <a name="global-admin-credentials"></a>Globální správce přihlašovacích údajů
Těchto přihlašovacích údajů se používají pouze v průběhu instalace a nepoužívaná po dokončení instalace. Slouží k vytvoření [účtu Azure AD](#azure-ad-service-account) pro synchronizaci změny Azure AD. Účet zároveň povolíte synchronizace jako funkce v Azure AD.

### <a name="permissions-for-the-created-ad-ds-account-for-express-settings"></a>Oprávnění pro služby AD DS vytvořený účet pro expresní nastavení
[Účet](#active-directory-account) vytvořený pro čtení a zápis do služby AD DS oprávnění následující při vytvoření Expresní nastavení:

Oprávnění | Použít pro
---- | ----
<li>Replikace změn adresáře</li><li>Všechny změny v replikovat adresářích | Synchronizace hesel
Pro čtení i zápis všech vlastností uživatele | Import a Exchange hybridní
Pro čtení i zápis všechny iNetOrgPerson vlastnosti | Import a Exchange hybridní
Seskupení všech vlastností pro čtení i zápis | Import a Exchange hybridní
Obraťte se na pro čtení i zápis všech vlastností | Import a Exchange hybridní
Resetování hesla | Příprava povolení zpětným heslo

## <a name="custom-settings-installation"></a>Vlastní nastavení instalace
Pokud chcete použít vlastní nastavení, je třeba vytvořit účet používá pro připojení ke službě Active Directory před instalací. Tento účet musí udělit oprávnění najdete v [vytvořit účet služby AD DS](#create-the-ad-ds-account).

Stránka Průvodce  | Shromažďované přihlašovací údaje | Informace o oprávněních požadovaných| Použít pro
------------- | ------------- |------------- |-------------
NENÍ K DISPOZICI | Uživatel spouštějící Průvodce instalací|<li>Správce místního serveru</li><li>Pokud používáte úplné SQL serveru, uživatel musí být systém správce v jazyce SQL</li>| Ve výchozím nastavení vytvoří místního účtu, který slouží jako [účet služby sync engine](#azure-ad-connect-sync-service-account). Účet pouze při vznikne správce není zadán určitého účtu.
Nainstalujte synchronizační služba služby, možnost účet služby | AD nebo pověření místního uživatelského účtu | Uživatele, pomocí Průvodce instalací mít příslušná oprávnění | Pokud správce určuje účet, tento účet se používá jako účet služby pro službu synchronizace.
Připojení k Azure AD | Přihlašovací údaje k directory Azure AD| Role globálního správce v Azure AD| <li>Povolení synchronizace v adresáři Azure AD.</li>  <li>Vytvoření [účtu Azure AD](#azure-ad-service-account) používanou pro operace synchronizace probíhajících v Azure AD.</li>
Připojení vaší adresáře | Místní služby Active Directory pro ni přihlašovací údaje jednotlivé domény, který je spojený s Azure AD | Oprávnění závisí na funkcí, které můžete povolit a najdete v [účtu vytvořit služby AD DS](#create-the-ad-ds-account) |Tento účet se používá pro čtení a zápis informace při synchronizaci adresářů.
Servery AD FS | Pro každý server v seznamu Průvodce shromažďuje přihlašovací údaje při přihlašovací údaje uživatele Průvodce nestačí připojení | Správce domény | Instalace a konfigurace role server služby AD FS.
Webové aplikace proxy servery |Pro každý server v seznamu Průvodce shromáždí přihlašovací údaje při přihlašovací údaje uživatele Průvodce nestačí připojení | Místní správce v cílovém počítači | Instalace a konfigurace role serveru WAP.
Přihlašovací údaje zabezpečení proxy serveru |Federace služby zabezpečení pověření (přihlašovací údaje proxy serveru používá k zápisu certifikátu zabezpečení pomocí FS |Účet domény, který slouží jako místní správce server služby AD FS | Počáteční zápisu FS WAP důvěryhodnosti certifikátu.
AD FS účet služby stránky, "Použít možnost účtu uživatele domény" | Přihlašovací údaje AD uživatelského účtu | Uživatel domény | Uživatelský účet AD jsou k dispozici u přihlašovacích údajů se použije jako přihlašovací účet služby AD FS.

### <a name="create-the-ad-ds-account"></a>Vytvoření účtu služby AD DS
Při instalaci Azure AD Connect účet, který je zadat na stránce **Připojit vaše adresáře** se musí nacházet ve službě Active Directory a vyžadovaly oprávnění udělena. Průvodce instalací není ověřte že oprávnění a všech problémů, jsou pouze zjištěné při synchronizaci.

Oprávnění, která nastavíte, závisí na volitelné funkce povolíte. Pokud máte víc domén, musí být oprávnění udělena pro všechny domény ve struktuře. Pokud nepovolíte některou z těchto funkcí výchozí **Domény uživatelská** oprávnění jsou dostatečné.

Funkce | Oprávnění
------ | ------
Synchronizace hesel | <li>Replikace změn adresáře</li>  <li>Všechny změny v replikovat adresářích
Hybridní nasazení Exchange | Oprávnění k zápisu atributů uvedených v [Exchange hybridní zpětným](../active-directory-aadconnectsync-attributes-synchronized.md#exchange-hybrid-writeback) uživatelé, skupiny a kontakty.
Heslo zpětného zápisu | Oprávnění k zápisu atributů uvedených v [Začínáme s Správa hesel](../active-directory-passwords-getting-started.md#step-4-set-up-the-appropriate-active-directory-permissions) pro uživatele.
Zpětného zápisu zařízení | Oprávnění udělena s skript Powershellu podle popisu v [zpětného zápisu zařízení](../active-directory-aadconnect-feature-device-writeback.md).
Skupina zpětného zápisu | Číst, vytvářet, aktualizovat a odstraňovat seskupení objektů v OU místo, kam mají být umístěny distribuci skupiny.

## <a name="upgrade"></a>Upgrade
Pokud upgradujete z jedné verze Azure AD Connect novou verzi, musíte následující oprávnění:

Hlavní | Informace o oprávněních požadovaných | Použít pro
---- | ---- | ----
Uživatel spouštějící Průvodce instalací | Správce místního serveru | Aktualizujte binární soubory.
Uživatel spouštějící Průvodce instalací | Členem ADSyncAdmins | Proveďte změny synchronizovat pravidla a další konfiguraci.
Uživatel spouštějící Průvodce instalací | Pokud používáte úplné SQL serveru: DBO (nebo podobné) databáze sync engine | Měnit databáze úrovni, třeba aktualizace tabulek s nové sloupce.

## <a name="more-about-the-created-accounts"></a>Další informace o vytvořené účty

### <a name="active-directory-account"></a>Účet Active Directory
Pokud používáte Expresní nastavení, účet vytvořené ve službě Active Directory, který se používá k synchronizaci. Vytvořený účet je umístěn v kořenové domény v kontejneru uživatelů a má její jméno s předponou **MSOL_**. Účet se vytvoří pomocí dlouho složité heslo, které nevyprší. Pokud máte ve vaší doméně zásady hesel, proveďte zkontrolujte dlouhé a složitá hesla bude mít možnost pro tento účet.

![AD účtu](./media/active-directory-aadconnect-accounts-permissions/adsyncserviceaccount.png)

### <a name="azure-ad-connect-sync-service-accounts"></a>Účty služby Azure AD Connect synchronizace
Místní služba účtu se vytvoří pomocí Průvodce instalací (Pokud nezadáte účet, který chcete použít v dialogovém okně Vlastní nastavení). Je účet s předponou **AAD_** a kterých se používá pro službu skutečné synchronizace jako. Pokud nainstalujete Azure AD Connect řadiče domény, vytvoří se účet v doméně. Pokud používáte vzdálený server systém SQL server nebo pokud používáte proxy server, který vyžaduje ověření, účet služby **AAD_** musí být umístěné v doméně.

![Účet služby synchronizace](./media/active-directory-aadconnect-accounts-permissions/syncserviceaccount.png)

Účet se vytvoří pomocí dlouho složité heslo, které nevyprší.

Tento účet se používá k ukládání hesel pro ostatní účty zabezpečené způsobem. Tyto účty hesla jsou uloženy šifrované databáze. Tajná klíč šifrování šifrování pomocí Windows Data Protection API (DPAPI) jsou chráněny privátních klíčů pro šifrování klíče. Neměli resetování hesla u účtu služby od Windows se pak destroy šifrovacího klíče z bezpečnostních důvodů.

Pokud používáte úplné SQL serveru, je účet služby DBO vytvořenou databázi pro sync engine. Službu nebude fungovat s jiná oprávnění. Přihlášení SQL je také vytvořit.

Účtu je také udělit oprávnění k souborům, klíče registru a další objekty související s modul synchronizace.

### <a name="azure-ad-service-account"></a>Účet služby Azure AD
Účet v Azure AD se vytvoří pomocí služby synchronizace. Tento účet lze identifikovat podle zobrazovaného jména.

![AD účtu](./media/active-directory-aadconnect-accounts-permissions/aadsyncserviceaccount.png)

Název serveru, který účet se používá na můžete označené v druhé části uživatelské jméno. Na obrázku je název serveru FABRIKAMCON. Pokud máte pracovní servery, má každý server vlastní účet. Je v Azure AD maximálně 10 účty služby synchronizace.

Účet služby se vytvoří pomocí dlouho složité heslo, které nevyprší. Uděleno zvláštní roli **Účty synchronizace adresáře** , který má jenom oprávnění provádět úlohy synchronizace adresářů. Tato zvláštní předdefinované role nelze udělit mimo Průvodce Azure AD Connect a portálu Azure ukazuje tento účet s rolí **uživatele**.

![Účtu AD rolí](./media/active-directory-aadconnect-accounts-permissions/aadsyncserviceaccountrole.png)

## <a name="next-steps"></a>Další kroky

Další informace o [Integrace místních identit s Azure Active Directory](../active-directory-aadconnect.md).
