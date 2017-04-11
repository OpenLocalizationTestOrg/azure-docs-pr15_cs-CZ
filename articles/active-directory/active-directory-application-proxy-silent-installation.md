<properties
    pageTitle="Postup tiše instalace konektoru Proxy aplikací Azure AD | Microsoft Azure"
    description="Popisuje, jak provádět používaného Azure AD aplikace Proxy spojnice k poskytnutí zabezpečeného vzdáleného přístupu ke svým aplikacím místní."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>

# <a name="how-to-silently-install-the-azure-ad-application-proxy-connector"></a>Postup tiše instalace konektoru Proxy aplikací Azure AD

Chcete mít možnost odeslat skriptu instalace Windows servery nebo servery systému Windows, které nemají uživatelského rozhraní povolena. Toto téma vysvětluje, jak vytvořit skript prostředí Windows PowerShell, který umožňuje bezobslužného instalace instalovat a registrovat váš Azure AD aplikace Proxy spojnice.

## <a name="enabling-access"></a>Povolení přístupu
Proxy aplikace funguje díky instalaci slim služba Windows Server s názvem konektoru uvnitř vaší síti. Konektoru aplikace proxy serveru pro práci se musí být registrovaný u adresáři Azure AD pomocí globální správce a hesla. Obvykle to zadává se během instalace konektoru v zobrazeném dialogovém. Můžete taky používat Windows PowerShell pro vytvoření objektu přihlašovací údaje k zadání registračních údajů, nebo můžete vytvořit vlastní token a použijte ji k zadání registračních údajů.

## <a name="step-1--install-the-connector-without-registration"></a>Krok 1: Instalace konektoru bez registrace


Nainstalujte souborů MSI spojnice bez registrace spojnice následujícím způsobem:


1. Otevřete příkazový řádek.
2. Spusťte tento příkaz, ve kterém/q znamená, že tichý instalace – instalace nezobrazí výzvu potvrďte licenční smlouvy s koncovým uživatelem.

        AADApplicationProxyConnectorInstaller.exe REGISTERCONNECTOR="false" /q

## <a name="step-2-register-the-connector-with-azure-active-directory"></a>Krok 2: Registrovat Connector službou Azure Active Directory
Můžete to provést pomocí jedné z těchto způsobů:


- Registrovat Connector pomocí přihlašovacích údajů objektu prostředí Windows PowerShell
- Registrace konektoru pomocí tokenu vytvořený v režimu offline

### <a name="register-the-connector-using-a-windows-powershell-credential-object"></a>Registrovat Connector pomocí přihlašovacích údajů objektu prostředí Windows PowerShell


1. Vytvoření objektu přihlašovací údaje Windows PowerShell spuštěním následujícího, kde "<username>"a"<password>" by měla být nahrazena uživatelské jméno a heslo pro adresáři:

        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $User, $SecurePassword

2. Přejděte na **C:\Program Files\Microsoft AAD aplikace Proxy spojnici** a spustit skript prostřednictvím Powershellu přihlašovací údaje, který jste vytvořili, kde $cred je název prostředí PowerShell pověření objekt, který jste vytvořili:

        RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Credentials -Usercredentials $cred


### <a name="register-the-connector-using-a-token-created-offline"></a>Registrace konektoru pomocí tokenu vytvořený v režimu offline

1. Vytvoření token offline pomocí AuthenticationContext třídy pomocí hodnot v fragment kódu:


        using System;
        using System.Diagnostics;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

        class Program
        {
        #region constants
        /// <summary>
        /// The AAD authentication endpoint uri
        /// </summary>
        static readonly Uri AadAuthenticationEndpoint = new Uri("https://login.windows.net/common/oauth2/token?api-version=1.0");

        /// <summary>
        /// The application ID of the connector in AAD
        /// </summary>
        static readonly string ConnectorAppId = "55747057-9b5d-4bd4-b387-abf52a8bd489";

        /// <summary>
        /// The reply address of the connector application in AAD
        /// </summary>
        static readonly Uri ConnectorRedirectAddress = new Uri("urn:ietf:wg:oauth:2.0:oob");

        /// <summary>
        /// The AppIdUri of the registration service in AAD
        /// </summary>
        static readonly Uri RegistrationServiceAppIdUri = new Uri("https://proxy.cloudwebappproxy.net/registerapp");

        #endregion

        #region private members
        private string token;
        private string tenantID;
        #endregion

        public void GetAuthenticationToken()
        {
            AuthenticationContext authContext = new AuthenticationContext(AadAuthenticationEndpoint.AbsoluteUri);

            AuthenticationResult authResult = authContext.AcquireToken(RegistrationServiceAppIdUri.AbsoluteUri,
                ConnectorAppId,
                ConnectorRedirectAddress,
                PromptBehavior.Always);

            if (authResult == null || string.IsNullOrEmpty(authResult.AccessToken) || string.IsNullOrEmpty(authResult.TenantId))
            {
                Trace.TraceError("Authentication result, token or tenant id returned are null");
                throw new InvalidOperationException("Authentication result, token or tenant id returned are null");
            }

            token = authResult.AccessToken;
            tenantID = authResult.TenantId;
        }





2. Až budete mít token vytvořit SecureString pomocí tokenu: <br>
`$SecureToken = $Token | ConvertTo-SecureString -AsPlainText -Force`
3. Spusťte tento příkaz prostředí Windows PowerShell, kde SecureToken je název, který jste vytvořili nad token a tenantID je GUID vašeho klienta: <br>
`RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Token -Token $SecureToken -TenantId <tenant GUID>`



## <a name="see-also"></a>Viz taky

- [Povolení aplikace Proxy služby Azure Active Directory](active-directory-application-proxy-enable.md)
- [Publikování aplikací pomocí vlastního názvu domény](active-directory-application-proxy-custom-domains.md)
- [Povolit jednotného přihlašování](active-directory-application-proxy-sso-using-kcd.md)
- [Řešení potíží s aplikací Proxy](active-directory-application-proxy-troubleshoot.md)

Nejnovější příspěvky a aktualizace najdete v článku [aplikace Proxy blogu](http://blogs.technet.com/b/applicationproxyblog/)
