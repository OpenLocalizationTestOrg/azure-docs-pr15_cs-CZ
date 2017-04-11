<properties 
    pageTitle="Začínáme s ověřovací certifikát založený na iOS | Microsoft Azure" 
    description="Zjistěte, jak nakonfigurovat ověřování na základě certifikátů v řešení zařízení s iOS" 
    services="active-directory" 
    authors="MarkusVi"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="10/21/2016" 
    ms.author="markvi" />



# <a name="get-started-with-certificate-based-authentication-on-ios---public-preview"></a>Začínáme s ověřováním certifikát založený na iOS - Public Preview

> [AZURE.SELECTOR]
- [iOS](active-directory-certificate-based-authentication-ios.md)
- [Android](active-directory-certificate-based-authentication-android.md)


V tomto tématu se dozvíte, jak konfigurovat a používat certifikátů na základě ověřování (CBA) na zařízení s iOS pro uživatele klienti v plánech Office 365 Enterprise, Business a vzdělávací organizace. 

CBA umožňuje podpisům službou Azure Active Directory s certifikátem klienta na zařízení s Androidem nebo iOS při připojení účtu Exchange online na: 

- Mobilní aplikace Office jako je Microsoft Outlook a Microsoft Word   
- Klientů Exchange ActiveSync (EAS) 

Konfigurace tuto funkci tak nemusí zadejte uživatelské jméno a heslo kombinaci do určité pošty a aplikací systému Microsoft Office na mobilním zařízení. 
 

## <a name="supported-scenarios-and-requirements"></a>Podporované scénáře a požadavky  



### <a name="general-requirements"></a>Obecné požadavky 


Pro všechny scénáře v tomto tématu jsou požadovány následující úkoly:  

- Přístup k authority(s) certifikát o vystavení klienta certifikáty.  

- Certifikáty authority(s) musí být nakonfigurované v Azure Active Directory. Podrobný postup, jak dokončete konfiguraci v části [Začínáme](#getting-started) můžete najít.  

- Kořenová autorita certifikát a všechny intermediate certifikačních autoritách musí být nakonfigurované v Azure Active Directory.  

- Každý certifikační autorita musí mít seznam odvolaných certifikátů (CRL), na které můžete odkazuje prostřednictvím internetové adresy URL.  

- Klientský certifikát musí být vydán pro ověření klienta.  


- U klientů protokolu Exchange ActiveSync klienta certifikát musí mít směrovatelné e-mailová adresa uživatele v Exchange online v hlavní název nebo hodnotu z pole Název RFC822 předmět alternativní název pole. Azure Active Directory mapuje hodnotu RFC822 atribut Proxy adresou do adresáře.  



### <a name="office-mobile-applications-support"></a>Podpora mobilních aplikací Office 

| Aplikace                      | Podpora      |
| ---                       | ---          |
| Aplikace Word nebo Excel nebo PowerPoint | ![Kontrola][1]  |
| Aplikace OneNote                   | ![Kontrola][1]  |
| OneDrive                  | ![Kontrola][1]  |
| Aplikace Outlook                   | ![Kontrola][1]  |
| Yammer                    | ![Kontrola][1]  |
| Skype pro firmy        | Již brzy  |


### <a name="requirements"></a>Požadavky  

Musí být verze zařízení s operačním systémem iOS 9 a vyšší 

Musí být nakonfigurované federačního serveru.  

Azure ověřovacích dat je nutný pro aplikace Office na iOS.  

Azure Active Directory, aby klientský certifikát odvolat musí mít token služby AD FS následující deklarací:  

  - `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>`  
(Pořadové číslo certifikát klienta) 

  - `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>`  
(Řetězec pro Vystavitel certifikátu klienta) 

Azure Active Directory přidá tyto požadavky na aktualizace token, pokud jsou k dispozici v token služby AD FS (nebo jiný token SAML). Při aktualizaci token je potřeba ověřit, tyto informace slouží k Kontrola odvolání. 

Osvědčený měli byste aktualizovat služby AD FS chybových stránek s tímto:

- Požadavky pro instalaci ověřovacích dat Azure na iOS

- Pokyny, jak získat certifikát uživatele. 

Další informace najdete v tématu [přizpůsobení AD FS přihlásit stránky](https://technet.microsoft.com/library/dn280950.aspx).  



### <a name="exchange-activesync-clients-support"></a>Podpora klientů Exchange ActiveSync 


V iOS 9 nebo novější je podporované poštovním klientovi nativní iOS. Pokud chcete určit, pokud tato funkce je podporována, všechny ostatní aplikace Exchange ActiveSync získáte vývojář aplikace.  



## <a name="getting-started"></a>Začínáme 


Nejdřív musíte nakonfigurovat certifikačních autoritách v Azure Active Directory. Pro každý certifikační autorita odešlete takto: 

- Veřejné část certifikátu ve formátu *.cer* 

- Internetové adresy URL ve žijete seznamy odvolaných certifikátů (CRL)
 

Níže je schématu certifikační autorita: 

    class TrustedCAsForPasswordlessAuth 
    { 
       CertificateAuthorityInformation[] certificateAuthorities;    
    } 

    class CertificateAuthorityInformation 

    { 
        CertAuthorityType authorityType; 
        X509Certificate trustedCertificate; 
        string crlDistributionPoint; 
        string deltaCrlDistributionPoint; 
        string trustedIssuer; 
        string trustedIssuerSKI; 
    }                

    enum CertAuthorityType 
    { 
        RootAuthority = 0, 
        IntermediateAuthority = 1 
    } 


Pokud chcete odeslat informace, můžete použít modul Azure AD pomocí prostředí Windows PowerShell.  
Tady jsou příklady pro přidání, odebrání nebo změna certifikační autorita. 



### <a name="configuring-your-azure-ad-tenant-for-certificate-based-authentication"></a>Konfigurace vašeho klienta Azure AD pro certifikát ověřování na základě 

1. Spusťte Windows PowerShell s oprávněními správce. 

2. Nainstalujte modul Azure AD. Musíte si nainstalovat verzi [1.1.143.0](http://www.powershellgallery.com/packages/AzureADPreview/1.1.143.0) nebo vyšší.  

        Install-Module -Name AzureADPreview –RequiredVersion 1.1.143.0 

3. Připojení ke tenantovi cíl: 

        Connect-AzureAD 

### <a name="adding-a-new-certificate-authority"></a>Přidání nového certifikační autorita

1. Nastavení jednotlivých vlastností certifikační autorita a přiřadit ji někomu Azure Active Directory: 

        $cert=Get-Content -Encoding byte "[LOCATION OF THE CER FILE]" 
        $new_ca=New-Object -TypeName Microsoft.Open.AzureAD.Model.CertificateAuthorityInformation 
        $new_ca.AuthorityType=0 
        $new_ca.TrustedCertificate=$cert 
        New-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $new_ca 

5. Získání certifikačních autoritách: 

        Get-AzureADTrustedCertificateAuthority 


### <a name="retrieving-the-list-certificate-authorities"></a>Načtení seznamu certifikačních autoritách

Načtení certifikačních autoritách obsažené v Azure Active Directory pro vašeho klienta: 

        Get-AzureADTrustedCertificateAuthority 


### <a name="removing-a-certificate-authority"></a>Odebrání certifikační autorita

1.  Načtení certifikačních autoritách: 

        $c=Get-AzureADTrustedCertificateAuthority 


2. Odebrání certifikát pro certifikační autorita: 

        Remove-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[2] 


### <a name="modfiying-a-certificate-authority"></a>Modfiying certifikační autorita 

1.  Načtení certifikačních autoritách: 

        $c=Get-AzureADTrustedCertificateAuthority 


2. Upravte vlastnosti na certifikační autorita: 

        $c[0].AuthorityType=1 

3. Nastavte **certifikační autorita**: 

        Set-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[0] 




## <a name="testing-office-mobile-applications"></a>Testování mobilních aplikací Office  

Chcete-li otestovat ověřování certifikátů na vaše mobilní aplikace Office: 

1.  Na vašem zařízení test instalace mobilní aplikace Office (například OneDrive) z App Storu.

2.  Ověřte, že byla zajištěna uživatelský certifikát do zařízení test. 

3.  Spusťte aplikaci. 

4.  Zadejte své uživatelské jméno a potom vyberte uživatele certifikát, který chcete použít. 

Abyste měli se úspěšně přihlásit. 





## <a name="testing-exchange-activesync-client-applications"></a>Testování Exchange ActiveSync klientskými aplikacemi

Přístup k serveru Exchange ActiveSync pomocí ověřování na základě certifikátů, musí být profil aplikace EAS obsahující certifikát klienta zpřístupňuje aplikacím. Zásady EAS profil musí obsahovat následující informace:

- Uživatel certifikát, který chcete použít pro ověřování 

- Koncový bod EAS musí odpovídat outlook.office365.com (Tato funkce je podporována jenom v prostředí Exchange online více klienta)

Profil aplikace EAS můžete nakonfigurované a klepnutím na zařízení pomocí využití MDM, třeba Intune nebo ručně umístěním certifikát ve svém profilu EAS zařízení.  

### <a name="testing-eas-client-applications-on-ios"></a>Testování EAS klientské aplikace v iOS 

Chcete-li otestovat ověřování certifikátu s nativní e-mailové aplikace v iOS 9 nebo nad: 

1.  Konfigurace EAS profil, který splňuje požadavky na výše. 

2.  Profil nainstalovat na zařízení iOS (buď pomocí MDM, třeba Intune nebo Apple Konfigurátor aplikace)

3.  Po nainstalování správně profilu, otevřete nativní e-mailová aplikace a ověřte, že je synchronizace pošty



## <a name="revocation"></a>Odvolání

Přejděte do centra certifikát klienta, Azure Active Directory načte seznam odvolaných certifikátů (CRL) z adresy URL uložit jako součást informací o certifikační autorita a uloží jej. Publikování poslední časového razítka (**Datum účinnosti** vlastnost) v seznamu CRL slouží k zajištění seznam odvolaných certifikátů pořád platná. Seznam odvolaných certifikátů odkazuje pravidelně odvolat přístup k certifikáty, které jsou součástí seznamu.

Pokud více rychlých odvolání požaduje (například pokud uživatel ztratí zařízení), můžete ukončena jejich platnost token ověření uživatele. Platnost token se tak mohli ověřovat **StsRefreshTokenValidFrom** poli nastavte u této konkrétní uživatelský pomocí Windows Powershellu. Musíte aktualizovat pole **StsRefreshTokenValidFrom** pro každého uživatele, který chcete odvolat přístup.
 
Zajistit, že odvolání potrvají, musíte nastavit **Datum účinnosti** seznam odvolaných certifikátů k určitému datu po hodnotu nastavil **StsRefreshTokenValidFrom** a zajistit, aby certifikát předmětné je seznam odvolaných certifikátů.
 
Následující kroky popisují proces aktualizace a zrušili platnost token se tak mohli ověřovat pomocí nastavení pole **StsRefreshTokenValidFrom** . 

1. Připojte se ke službě MSOL přihlašovacích údajů správce: 

        $msolcred = get-credential 
        connect-msolservice -credential $msolcred 

1.  Načtení aktuální StsRefreshTokensValidFrom hodnota pro uživatele: 

        $user = Get-MsolUser -UserPrincipalName test@yourdomain.com` 
        $user.StsRefreshTokensValidFrom 


1.  Konfigurace novou hodnotu StsRefreshTokensValidFrom pro uživatele, který je rovno aktuální časové razítko: 

        Set-MsolUser -UserPrincipalName test@yourdomain.com -StsRefreshTokensValidFrom ("03/05/2016")


Datum, které můžete nastavit musí být v budoucnosti. Nejsou-li datum v budoucnosti, není nastavena vlastnost **StsRefreshTokensValidFrom** . Pokud je datum v budoucnosti, **StsRefreshTokensValidFrom** nastavenou na aktuální čas (ne datum označený příkaz Set-MsolUser). 



<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-ios/ic195031.png