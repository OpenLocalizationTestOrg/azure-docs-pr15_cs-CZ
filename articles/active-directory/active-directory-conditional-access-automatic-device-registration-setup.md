<properties
    pageTitle="Nastavení automatické registrace Windows doméně zařízení s Azure Active Directory | Microsoft Azure"
    description="Nastavení zařízení s Windows doméně k registraci automaticky a tiše s Azure Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="markvi"/>



# <a name="set-up-automatic-registration-of-windows-domain-joined-devices-with-azure-active-directory"></a>Nastavení automatické registrace Windows doméně zařízení s Azure Active Directory

Pokud chcete použít [podmíněné přístupu na základě zařízení Azure Active Directory](active-directory-conditional-access.md), musí být počítače doméně Windows registrován u Azure Active Directory (Azure AD). V tomto článku se dozvíte, co je potřeba nastavit registrace zařízení doméně Windows s Azure AD ve vaší organizaci.

Použití podmíněného přístupu v Azure AD poskytuje tyto výhody:

-   Možnosti rozšířeného jednotné přihlašování (SSO) v aplikacích Azure AD pomocí pracovní nebo školní účty
-   Pole organizace cestovní nastavení na zařízeních
-   Přístup k webu Windows Store pro firmy
-   Silnější ověřování a pohodlný přihlášení k aplikaci Ahoj systému Windows

> [AZURE.NOTE] Aktualizace Windows 10 listopad nabízí některé rozšířené uživatelského prostředí v Azure AD, ale aktualizace Windows 10 výročí plně podporuje podmíněné přístupu na základě zařízení. Další informace o podmíněném přístupu najdete v článku [podmíněné přístupu na základě zařízení Azure Active Directory](active-directory-conditional-access.md). Další informace o zařízení Windows 10 ve pracovišti a jak uživatel zaregistruje zařízení s Windows 10 s Azure AD najdete v tématu [Windows 10 Enterprise: pomocí zařízení pro práci](active-directory-azureadjoin-windows10-devices-overview.md).

Můžete zaregistrovat některé starší verze systému Windows, včetně tyto verze:

-   Windows 8.1
-   Windows 7

Pokud používáte Windows serveru jako plochy, můžete zaregistrovat tyto platformách:

- Windows Server 2016
- Windows serveru 2012 R2
- Windows Server 2012
- Windows Server 2008 R2


## <a name="prerequisites"></a>Zjistit předpoklady pro

Hlavní požadavku na automatické registrace doméně zařízení pomocí Azure AD má mít aktuální verzi Azure Active Directory se připojit (Azure AD Connect).

Podle toho, jak nasazena Azure AD Connect a jestli jste použili k express nebo vlastní instalace nebo upgrade na místě následující požadavky mohou nakonfigurovány automaticky:

-   **Spojovací bod služby v místní služby Active Directory**. Pro vyhledání informací klienta Azure AD pomocí počítače, který si zaregistrovat Azure AD.

-   **Active Directory Federation Services (AD FS) vydávání transformace pravidla**. Ověření počítače při registraci (platí pro federované konfigurace).

Není-li některé zařízení ve vaší organizací doméně zařízeních s Windows 10, zkontrolujte, jestli že dělat tyto věci:

- Nastavit zásady v Azure AD tak, aby uživatelé můžou zaregistrovat zařízení
- Nastavení integrované ověřování systému Windows (IWA) jako platná alternativa vícefaktorové ověřování ve službě AD FS



## <a name="set-a-service-connection-point-for-discovery-of-the-azure-ad-tenant"></a>Nastavení spojovací bod služby pro zjišťování Azure AD klienta

Objekt spojovací bod služby musí existovat v kontextu názvové struktury domény které připojen počítačů. Spojovací bod služby obsahuje informace o zjišťování o tenantu Azure AD kde počítače registrovat. V konfiguraci služby Active Directory více struktury musí existovat spojovací bod služby ve všech strukturami, které mají doméně počítačů.

Nastavte spojovací bod služby v těchto umístěních ve službě Active Directory:

    CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,[Configuration Naming Context]

> [AZURE.NOTE] Pro struktuře s služby Active Directory domain Název *example.com*konfigurace kontext názvů je CN = konfigurace, Datacentrum = příkladu Datacentrum = cz.

Můžete vyhledat klienta objektu a zjištění hodnoty Azure AD pomocí následující skriptu prostředí Windows PowerShell. (Nahraďte kontextu názvů konfigurace v příkladu naming kontext konfigurace.)

    $scp = New-Object System.DirectoryServices.DirectoryEntry;

    $scp.Path = "LDAP://CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=example,DC=com";

    $scp.Keywords;

`$scp.Keywords` Výstup zobrazuje informace klienta Azure AD:

    azureADName:microsoft.com

    azureADId:72f988bf-86f1-41af-91ab-2d7cd011db47

Pokud spojovací bod služby neexistuje, vytvořte ji spuštěním tento skript Powershellu na server Azure AD Connect:

    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;


> [AZURE.NOTE]Když spustíte `$aadAdminCred = Get-Credential`, použijte formát *user@example.com* pro uživatelské jméno v dialogovém okně **Načíst přihlašovacích údajů** .  
> Když spustíte rutinu inicializace ADSyncDomainJoinedComputerSync, nahraďte webu [*název účtu spojnice*] účtu domény, který se používá v spojnice účet služby Active Directory.  
> Rutiny používá modul Active Directory PowerShell, který využívá webové služby Active Directory v řadiče domény. Web služby Active Directory Services podporují řadiče domény v systému Windows Server 2008 R2 a novějších verzích. U řadiče domény v systému Windows Server 2008 a starších verzích vytvoření spojovací bod služby pomocí rozhraní API System.DirectoryServices pomocí prostředí PowerShell a přiřaďte hodnoty klíčových slov.

## <a name="create-ad-fs-rules-for-instant-device-registration-in-federated-organizations"></a>Vytváření pravidel AD FS pro registraci rychlé zařízení ve federovaných organizacích

Ve federovaných Azure AD konfigurace zařízení závisí na AD FS (nebo na místní federaci server) pro ověření Azure AD. Potom registraci proti Azure Active Directory zařízení registrace služba Azure AD zařízení registrace.

> [AZURE.NOTE] V konfiguraci nefederované hash heslo uživatele se synchronizují Azure AD a Windows 10 a Windows Server 2016 doméně počítačích ověřování služby Azure AD zařízení registrace. Uživatel se ověří pomocí přihlašovacích údajů, který data zapisuje uživatele do svých účtů místního počítače a který je přenos Azure AD pomocí Azure AD Connect. Pro Windows 10 a Windows Server 2016 počítače v nefederované konfiguraci máte možnosti pro nastavení na základě zařízení certifikační autorita ve vaší organizaci. Další informace najdete v tématu stáhnout Instalační služby systému Windows balíčků oddíl pro Windows 10 počítače v tomto článku.

U počítačů s Windows 10 a Windows Server 2016 Azure AD Connect přidružuje objekt zařízení v Azure AD objekt účtu místního počítače. Následující deklarací musí existovat při ověřování pro službu Azure AD zařízení registrace dokončete registraci a vytvářet objekt zařízení:

- http://schemas.microsoft.com/ws/2012/01/AccountType obsahuje hodnotu DJ, který identifikuje hlavní ověřovacích dat jako počítač doméně.
- http://schemas.microsoft.com/identity/Claims/onpremobjectguid obsahuje hodnotu atributu **objectGUID** účtu místního počítače.
- http://schemas.microsoft.com/ws/2008/06/identity/Claims/primarysid obsahuje počítače primární zabezpečení ID, které odpovídá hodnota atributu **objectSid** účtu místního počítače.
- http://schemas.microsoft.com/ws/2008/06/identity/Claims/issuerid obsahuje hodnotu, která Azure AD používá k zabezpečení token nebyl vystaven ze služby AD FS a od místní zabezpečení tokenu Service (Služba tokenů zabezpečení). Co je důležité ve více doménové konfiguraci služby Active Directory. V této konfiguraci počítače může být připojen k strukturu než ten, který se připojí k Azure AD na AD FS nebo místní služba tokenů zabezpečení. Služba AD FS použijte hodnotu z pole http://<*název domény*>/služby AD FS/služby/zabezpečení /, kde <*název domény*> ověřený váš název domény je v Azure AD.

Vytváření pravidel ručně, ve službě AD FS použijte tento skript Powershellu v relaci, který je připojen k serveru. Nahradí první řádek se svým názvem domény ověřený organizace v Azure AD.

> [AZURE.NOTE] Pokud nepoužíváte pro místní federačního serveru AD FS, postupujte podle pokynů vašeho dodavatele vytvořit pravidla, která problému tyto požadavky.

    $validatedDomain = "example.com"      # Replace example.com with your organization's validated domain name in Azure AD

    $rule1 = '@RuleName = "Issue object GUID"

        c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&

        c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]

        => issue(store = "Active Directory", types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), query = ";objectguid;{0}", param = c2.Value);'

    $rule2 = '@RuleName = "Issue account type for domain-joined computers"

      c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]

      => issue(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "DJ");'

> [AZURE.NOTE] Pouze první tři pravidla použijte, pokud je vaše prostředí místní jedné stromové struktury. Pokud jsou počítače v jiné struktuře než jedna synchronizace s Azure AD nebo pokud používáte alternativní názvy z nich v konfiguraci synchronizace, musí obsahovat také zbývajících pravidel.

    $rule3 = '@RuleName = "Pass through primary SID"

      c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&

      c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]

      => issue(claim = c2);'

    $rule4 = '@RuleName = "Issue AccountType with the value User when it’s not a computer account"

      NOT EXISTS([Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "DJ"])

      => add(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "User");'

    $rule5 = '@RuleName = "Capture UPN when AccountType is User and issue the IssuerID"

      c1:[Type == "http://schemas.xmlsoap.org/claims/UPN"] &&

      c2:[Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "User"]

      => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c1.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));'

    $rule6 = '@RuleName = "Update issuer for DJ computer auth"

      c1:[Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "DJ"]

      => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = "http://'+$validatedDomain+'/adfs/services/trust/");'

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3 + $rule4 + $rule5 + $rule6

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString

> [AZURE.NOTE] Windows 10 a Windows Server 2016 doméně počítačích ověřovat pomocí IWA aktivní WS zabezpečení koncový bod, který je hostitelem služby AD FS. Ujistěte se, že koncový bod je aktivní. Pokud používáte proxy serveru webové aplikace, ujistěte se, že tento koncový bod je publikován proxy server. Koncový bod je služby AD FS/služby/zabezpečení/13/windowstransport. Zkontrolujte, zda je aktivní v konzole pro správu služby AD FS nedokážete **služby** > **koncové body**. Pokud nepoužíváte pro místní federačního serveru AD FS, postupujte podle pokynů vašeho dodavatele a ujistěte se, že odpovídající koncový bod je aktivní.



## <a name="set-up-ad-fs-for-authentication-of-device-registration"></a>Nastavení služby AD FS pro ověření registrace zařízení

Ujistěte se, že IWA je nastavena jako platná alternativa vícefaktorové ověřování pro registraci zařízení ve službě AD FS. K tomuto účelu musíte mít vydávání transformace pravidlo, které prochází metody ověřování.

1. V konzole pro správu služby AD FS, přejděte na **AD FS** > **Vztahy důvěryhodnosti** > **Může stran považuje za důvěryhodnou**.

2. Klikněte pravým tlačítkem na Microsoft Office 365 Identity platformě předávající stran zabezpečení objekt a pak vyberte **Upravit pravidla deklarace**.

3.  Na kartě **Transformace pravidel pro vydávání** vyberte **Přidat pravidlo**.

4.  V seznamu šablony **deklarace pravidla** zaškrtněte políčko **Odeslat deklarací pomocí vlastní pravidla**.

5.  Vyberte **Další**.

6.  Do pole **název pravidla deklarace** zadání **Auth metoda deklarace pravidla**.

7.  V dialogovém okně **pravidla deklarace** zadejte tento příkaz:

    `c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"]
    => issue(claim = c);`.

8. Na federačním serveru zadejte tento příkaz Powershellu:

    `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

<*RPObjectName*> je předávající stran objekt název objektu Azure AD předávající stran zabezpečení. Tento objekt obvykle jmenuje *Identity platformu Microsoft Office 365*.



## <a name="deployment-and-rollout"></a>Nasazení a zavedení

Když doméně počítačů nesplňuje požadavky, jsou připravené k registraci Azure AD.

Aktualizace výročí Windows 10 a Windows Server 2016 doméně počítačích automaticky zaregistrovat Azure AD při příštím restartování zařízení nebo pokud se uživatel přihlásí k systému Windows. Nových počítačů, které jsou připojené k doméně zaregistrovat Azure AD restartování zařízení po operace připojení domény.

> [AZURE.NOTE] Windows 10 doméně počítačů automaticky zaregistrujte Azure AD jenom v případě, že je nastavený na objekt zásad skupiny zavedení.

Použití objektu zásad skupiny pro řízení zavádění automatické registrace Windows 10 a Windows Server 2016 doméně počítačů. Rozbalíte Automatická registrace doméně počítačích Windows 10, můžete nasadit balíček Instalační služby systému Windows u počítačů, které vyberete.

> [AZURE.NOTE] Zásad skupiny pro řízení zavádění taky spustí registrace doméně počítačích Windows 8.1. Zásady můžete použít pro registraci Windows 8.1 doméně počítačů. Nebo pokud máte kombinaci verzí systému Windows, včetně verzí Windows 7 nebo Windows Server, můžete zaregistrovat Windows 10 a Windows Server 2016 počítače pomocí balíček Instalační služby systému Windows.

### <a name="create-a-group-policy-object-to-control-the-rollout-of-automatic-registration"></a>Vytvoření objektu zásad skupiny pro řízení zavádění automatické registrace

Pro řízení zavádění Automatická registrace doméně počítačích s Azure AD nástroje můžete nasazovat zásad skupiny **zaregistrovat doméně počítače jako zařízení** do počítačů, které chcete zaregistrovat. Například nasazením zásady s organizační jednotkou nebo skupiny zabezpečení.

Nastavení zásad, postupujte takto:

1. Otevřete správce serveru a pak přejděte na **Nástroje** > **Správa zásad skupiny**.

2. Přejděte na uzel domény, který odpovídá domény místo, kam chcete aktivovat automatického registrace Windows 10 nebo Windows Server 2016 počítačů.

3. Klikněte pravým tlačítkem myši **Objekty zásad skupiny**a pak vyberte **Nový**.

4. Zadejte název objektu zásad skupiny. Například *Automatické registrace Azure AD*. Klikněte na **OK**.

4. Klikněte pravým tlačítkem na nový objekt zásad skupiny a pak vyberte **Upravit**.

5. Přejděte na **počítač konfigurace** > **zásady** > **šablony pro správu** > **součásti systému Windows** > **registrace zařízení**. Klikněte pravým tlačítkem **Register domény připojen počítače jako zařízení**a pak vyberte **Upravit**.

    > [AZURE.NOTE] Přejmenování tuto šablonu zásad skupiny se ze starších verzí konzolu Správa zásad skupiny. Pokud používáte dřívější verzi konzole, přejděte na **Konfigurace počítače** > **zásady** > **Šablony pro správu** > **Součásti systému Windows** > **Pracovišti spojení** > **automaticky pracovišti připojení ke klientské počítače**.

6. Vyberte **Povolit**a pak vyberte **použít**.

7. Klikněte na **OK**.

8. Odkaz na objekt zásad skupiny do umístění podle svého výběru. Například můžete propojit s konkrétní organizační jednotkou. Můžete také propojit ho na konkrétní skupiny zabezpečení počítačů, které automaticky zaregistrovat Azure AD. Nastavení této zásady pro všechny doméně Windows 10 a Windows Server 2016 počítače ve vaší organizaci, odkaz na doménu objekt zásad skupiny.

### <a name="download-windows-installer-packages-for-non-windows-10-computers"></a>Stažení balíčků Instalační služby systému Windows pro Windows 10 počítače  

Zaregistrovat doméně počítače se systémem Windows 8.1, Windows 7, Windows Server 2012 R2, Windows Server 2012 nebo Windows Server 2008 R2, stáhněte si a nainstalujte tyto Windows Installer (MSI) souborů:

- [x64](http://download.microsoft.com/download/C/A/7/CA79FAE2-8C18-4A8C-A4C0-5854E449ADB8/Workplace_x64.msi)
- [x86](http://download.microsoft.com/download/C/A/7/CA79FAE2-8C18-4A8C-A4C0-5854E449ADB8/Workplace_x86.msi)

Nasazení balíčku pomocí systému distribuci softwaru jako centrum Správce konfigurace pro System. Balíček podporuje možnosti standardní pasivní instalace s parametrem *Tichý* . Správce konfigurace pro System Center 2016 nabízí další výhody ze starších verzí, jako je možnost sledování dokončených registrace. Další informace najdete v tématu [System Center 2016](https://www.microsoft.com/en-us/cloud-platform/system-center).

Instalační program vytvoří naplánovaný úkol v systému, který běží v kontextu uživatele. Úkol se aktivuje, pokud se uživatel přihlásí k systému Windows. Úkol tiše zaregistruje zařízení s Azure AD pomocí přihlašovací údaje uživatele po ověření prostřednictvím IWA. Pokud chcete zobrazit naplánovaný úkol, přejděte na **Microsoft** > **Pracovišti připojení ke**schůzce a pak se vraťte do knihovny Plánovač úloh.



## <a name="next-steps"></a>Další kroky

- [Azure Active Directory podmíněného přístupu](active-directory-conditional-access.md)
