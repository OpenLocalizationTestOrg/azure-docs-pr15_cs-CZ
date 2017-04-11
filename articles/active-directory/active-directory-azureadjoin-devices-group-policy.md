<properties
    pageTitle="Připojte zařízení doméně Azure AD pro Windows 10 dochází | Microsoft Azure"
    description="Vysvětluje, jak mohou správci konfigurovat zásad skupiny povolit zařízení, která mají být domény připojen k síti organizace."
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

# <a name="connect-domain-joined-devices-to-azure-ad-for-windows-10-experiences"></a>Připojte zařízení doméně Azure AD pro Windows 10 prostředí

Připojení k doméně je že tradiční způsob organizace připojili zařízení pro pracovní zátěž: za poslední 15 roky a další. Povolil uživatelům přihlásit na jejich zařízení pomocí jejich systému Windows Server služby Active Directory (Active Directory) pracovní nebo školní účty a povolené IT plně Správa těchto zařízení. Organizace obvykle spolehnout pro zpracování obrázků metod k poskytování zařízení uživatelům a obecně pomocí konfigurace správce (nástroj SCCM System Center) nebo zásadu skupin je spravovat.

Připojení k doméně ve Windows 10 poskytuje následující výhody po připojení zařízení pro službu Azure Active Directory (Azure AD):

- Jednotné přihlašování (SSO) podívejte se na materiály Azure AD odkudkoli
- Přístup k enterprise pro Windows Store pomocí pracovní nebo školní účty (bez účtu Microsoft povinné)
- Kompatibilní s Enterprise cestovní uživatelských nastavení na zařízeních s použitím pracovní nebo školní účty (bez účtu Microsoft povinné)
- Silné ověřování a vhodné pro sprá pracovní nebo školní účty s Microsoft Passport a Ahoj systému Windows
- Možnost omezit přístup jenom pro zařízení, které splňují nastavení zásad skupiny organizační zařízení

## <a name="prerequisites"></a>Zjistit předpoklady pro

Připojení k doméně pokračuje být užitečné. Však k získání výhody Azure AD SSO, nastavení s pracovní nebo školní účty a accessem roamingu na pro Windows Store s pracovní nebo školní účty, budete potřebovat následující:

- Azure AD předplatného
- Azure AD Connect rozšířit Azure AD místního adresáře
- Zásady, která obsahuje nastavení pro připojení k Azure AD doméně zařízení
- Windows 10 sestavení (build 10551 nebo novější) pro zařízení

Povolení Microsoft Passport pro čísle do zaměstnání, Windows Dobrý den, budete taky potřebovat takto:

- Veřejné klíčové infrastruktury pro vydávání certifikáty uživatele.
- Centrum pro správce konfigurace pro System verze 1509 Technical Preview. Další informace najdete v tématu [Microsoft System Center konfigurace správce Technical Preview](https://technet.microsoft.com/library/dn965439.aspx#BKMK_TP3Update) a [Blog týmu Centrum Správce konfigurace systému](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx). To je potřeba k nasazení uživatelských certifikátů založené na Microsoft Passport klíči.

Jako alternativu k požadovaného nasazení KLÍČŮ můžete udělat toto:

- Máte několik řadiče domény s Windows serveru 2016 Active Directory Domain Services.

Povolit podmíněné přístup, můžete vytvořit nastavení zásad skupiny, které umožňují přístup k doméně zařízení s žádné další nasazení. Ke správě řízení přístupu založeny na dodržování předpisů zařízení, budete potřebovat:

- Správce konfigurace pro System Center verzi 1509 Technical Preview Passport scénáře

## <a name="deployment-instructions"></a>Pokyny pro nasazení



### <a name="step-1-deploy-azure-active-directory-connect"></a>Krok 1: Nasazení služby Azure Active Directory připojení

Azure AD Connect vám umožní zřízení počítačů místní jako objekty zařízení v cloudu. Abyste mohli nasadit Azure AD Connect, v nápovědě k "Instalace Azure AD Connect" v článku znalostní báze [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md#install-azure-ad-connect).

 - Pokud jste postupovali podle typu [Vlastní instalace pro Azure AD Connect](./connect/active-directory-aadconnect-get-started-custom.md) (ne Expresní instalace), potom postup **vytvoření připojení služby přejděte ve službě Active Directory místní**, dál v tomto kroku.
 - Pokud máte federované konfigurace s Azure AD před instalací Azure AD Connect (například, pokud jste nasadili Active Directory Federation Services (AD FS) před), potom postupujte **pravidla deklarace konfigurovat službu AD FS** , dál v tomto kroku.

#### <a name="create-a-service-connection-point-in-on-premises-active-directory"></a>Vytvoření spojovací bod služby v místní služby Active Directory

Doméně zařízení bude používat spojovací bod služby Seznamte se s Azure AD klienta informace v době automatické registrace registrace službou Azure zařízení.

Na server Azure AD Connect spusťte následující příkazy Powershellu:

    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;


Při spuštění rutinu $aadAdminCred = Get-přihlašovacích údajů, použijte formát *user@example.com* pro uživatelské jméno přihlašovací údaje zadané začalo automaticky otevírané okno Get-pověření.

Při spuštění rutinu inicializace ADSyncDomainJoinedComputerSync..., nahraďte webu [*název účtu spojnice*] doménovým účtem, který slouží jako spojnice účet služby Active Directory.

#### <a name="configure-ad-fs-claim-rules"></a>Konfigurace služby AD FS deklarace pravidel
Konfigurace pravidel deklarace AD FS umožňuje okamžité registrace počítače se službou Azure zařízení registrace povolením počítačům ověřovat pomocí protokolu Kerberos/NTLM prostřednictvím služby AD FS. Bez tento krok počítačů pošle Azure AD zpožděné způsobem (vyměřené poplatky za jeho doby synchronizace Azure AD Connect).

>[AZURE.NOTE]
Pokud nemáte službu AD FS jako federačního serveru místní, postupujte podle pokynů dodavatele k vytváření pravidel deklarace.

Na server služby AD FS (nebo na relaci připojení na server služby AD FS) spusťte následující příkazy Powershellu:

      <#----------------------------------------------------------------------
     |   Modify the Azure AD Relying Party to include the claims needed
     |   for DomainJoin++. The rules include:
     |   -ObjectGuid
     |   -AccountType
     |   -ObjectSid
     +---------------------------------------------------------------------#>

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules

    $rule1 = '@RuleName = "Issue object GUID"
          c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&
          c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(store = "Active Directory", types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), query = ";objectguid;{0}", param = c2.Value);'

    $rule2 = '@RuleName = "Issue account type for domain joined computers"
          c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "DJ");'

    $rule3 = '@RuleName = "Pass through primary SID"
          c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&
          c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(claim = c2);'

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString

>[AZURE.NOTE]
Windows 10 počítačů bude ověřovat pomocí integrovaného ověřování systému Windows aktivní koncový bod WS zabezpečení hostované pomocí služby AD FS. Ujistěte se, že je povolená tento koncový bod. Pokud používáte Web ověřování Proxy, taky zajistit, aby zveřejnění tento koncový bod proxy server. Lze provést kontrolou služby AD FS/služby/zabezpečení/13/windowstransport. Má zobrazit jako povolená v konzole pro správu služby AD FS v rámci **služby** > **koncové body**.


### <a name="step-2-configure-automatic-device-registration-via-group-policy-in-active-directory"></a>Krok 2: Nakonfigurujte automatické zařízení registrace prostřednictvím zásad skupiny ve službě Active Directory

Můžete zásad skupiny ve službě Active Directory konfigurovat zařízení Windows 10 doméně automaticky zaregistrovat s Azure AD.

> [AZURE.NOTE]
> Nejnovější pokyny k nastavení registrace automatické zařízení najdete v článku, [jak nastavit automatické registrace Windows domény připojen zařízení s Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).
>
> Přejmenování tuto šablonu zásad skupiny se ve Windows 10. Pokud používáte nástroj zásad skupiny z počítače s Windows 10, zásady se zobrazí jako: <br>
> **Registrace domény připojen počítače jako zařízení**<br>
> Zásada se v následujícím umístění:<br>
> ***Registrace součásti/zařízení Konfigurace/zásady/správní šablony/Windows počítače***


## <a name="additional-information"></a>Další informace
* [Windows 10 Enterprise: způsoby použití zařízení pro práci](active-directory-azureadjoin-windows10-devices-overview.md)
* [Rozšíření možností cloudu na zařízeních s Windows 10 až Azure Active Directory připojit se ke](active-directory-azureadjoin-user-upgrade.md)
* [Další informace o použití scénáře pro Azure AD připojení](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Připojte zařízení doméně Azure AD pro Windows 10 prostředí](active-directory-azureadjoin-devices-group-policy.md)
* [Nastavení připojení Azure AD](active-directory-azureadjoin-setup.md)
