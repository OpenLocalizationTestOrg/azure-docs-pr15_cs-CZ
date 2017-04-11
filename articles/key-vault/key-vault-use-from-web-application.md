<properties
    pageTitle="Použití Azure klíčové trezoru z webové aplikace | Microsoft Azure"
    description="Pomocí tohoto kurzu vám pomohou naučit používat Azure klíč trezoru z webové aplikace."
    services="key-vault"
    documentationCenter=""
    authors="adhurwit"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="adhurwit"/>

# <a name="use-azure-key-vault-from-a-web-application"></a>Použití Azure klíčové trezoru z webové aplikace #

## <a name="introduction"></a>Úvod  
Pomocí tohoto kurzu vám pomohou naučit používat Azure klíč trezoru z webové aplikace v Azure. Ho vás provede procesem přístupu k tajná z trezoru klíč Azure, takže můžete použít ve webové aplikaci.

**Předpokládaná doba dokončete:** 15 minut


Základní informace o trezoru klíč Azure, najdete v článku [Co je Azure klíč trezoru?](key-vault-whatis.md)

## <a name="prerequisites"></a>Zjistit předpoklady pro

Tento kurz musí mít takto:

- Identifikátor URI tajná trezoru klíč Azure
- ID klienta a tajná klienta pro webovou aplikaci registrovaný u Azure Active Directory, která má přístup k trezoru klíč
- Webová aplikace. Budeme se být zobrazena kroky pro aplikaci ASP.NET MVC nasazenou v Azure jako Web App.

> [AZURE.NOTE]  Je důležité, že jste dokončili postupů uvedených v tématu [Začínáme s Azure klíč trezoru](key-vault-get-started.md) pro účely tohoto návodu tak, aby měli URI tajná ID klienta a tajná klienta pro webovou aplikaci.

Webové aplikace, která budou trezoru klíč je ten, který je zaregistrovaná v Azure Active Directory a dostal přístup k trezoru klíče. Pokud to není v případě, přejděte zpátky na webu Register aplikace v kurzu Začínáme a zopakujte kroky uvedené.

Tento kurz je určený pro vývojáře webů, které seznámení se základy vytváření webových aplikací na Azure. Další informace o Azure Web Apps najdete v tématu [Přehled Web Apps](../app-service-web/app-service-web-overview.md).



## <a id="packages"></a>Přidání Nuget balíčků ##
Existují dva balíčků, které webové aplikace musí mít nainstalovaný.

- Active Directory Authentication Library - obsahuje způsoby práce s Azure Active Directory a správě identita uživatele
- Azure knihovna trezoru klíč – obsahuje metody pro komunikaci s trezoru klíč Azure


Obě tyto balíčky možné nainstalovat pomocí Správce balíčků konzoly pomocí příkazu instalační balíček.

    // this is currently the latest stable version of ADAL
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

    Install-Package Microsoft.Azure.KeyVault


## <a id="webconfig"></a>Úprava Web.Config ##
Existují tři nastavení aplikace, které je potřeba přidat nastavení(Web.config)) následujícím způsobem.

    <!-- ClientId and ClientSecret refer to the web application registration with Azure Active Directory -->
    <add key="ClientId" value="clientid" />
    <add key="ClientSecret" value="clientsecret" />

    <!-- SecretUri is the URI for the secret in Azure Key Vault -->
    <add key="SecretUri" value="secreturi" />


Pokud jste neprojdou hostovat aplikace jako webovou aplikaci Azure měli správných hodnot ClientId tajná klienta a identifikátor URI tajná přidat do web.config. V opačném případě ponechte tyto formální hodnoty, protože jsme přidání správných hodnot v portálu Azure pro další úroveň zabezpečení.


## <a id="gettoken"></a>Přidání způsob, jak získat přístupový Token ##
Chcete-li pomocí rozhraní API trezoru klíč potřebujete přístupový token. Klient trezoru klíč zpracovává volání rozhraní API trezoru klíče, ale budete muset zadat pomocí funkce, která vrací přístupový token.  

Následuje kódu a získání přístupový token ze služby Azure Active Directory. Tento kód můžete kamkoliv v aplikaci. Můžu chtěli přidat Utils nebo EncryptionHelper předmětu.  

    //add these using statements
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System.Threading.Tasks;
    using System.Web.Configuration;

    //this is an optional property to hold the secret after it is retrieved
    public static string EncryptSecret { get; set; }

    //the method that will be provided to the KeyVaultClient
    public static async Task<string> GetToken(string authority, string resource, string scope)
    {
        var authContext = new AuthenticationContext(authority);
        ClientCredential clientCred = new ClientCredential(WebConfigurationManager.AppSettings["ClientId"],
                    WebConfigurationManager.AppSettings["ClientSecret"]);
        AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

        if (result == null)
            throw new InvalidOperationException("Failed to obtain the JWT token");

        return result.AccessToken;
    }

> [AZURE.NOTE] 
> Nejjednodušší způsob, jak ověřit aplikace Azure AD pomocí ID klienta a tajná klienta je. A používat ho ve webové aplikaci umožňuje oddělení úkolů a větší kontrolu nad správy klíčů. Ale závisí na umístění tajná klienta v nastaveních konfigurace, které pro některé může být jako riziková jako uvedení tajná, který chcete zamknout v konfiguraci. Informace o tom, jak pomocí ID klienta a certifikát místo ID klienta a tajná klienta ověřování aplikace Azure AD najdete níže.



## <a id="appstart"></a>Načtení tajná na spuštění aplikace ##
Teď potřebujeme kódu volat rozhraní API trezoru klíče a načíst tajná. Následující kód můžete umístit kamkoliv, dokud je označená jako předtím, než budete muset používat. Jste přesunuli jste tento kód událost spustit aplikaci Global.asax tak, aby se spustí jednou při spuštění a zpřístupní tajná pro aplikace.

    //add these using statements
    using Microsoft.Azure.KeyVault;
    using System.Web.Configuration;

    // I put my GetToken method in a Utils class. Change for wherever you placed your method.
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

    var sec = kv.GetSecretAsync(WebConfigurationManager.AppSettings["SecretUri"]).Result.Value;

    //I put a variable in a Utils class to hold the secret for general  application use.
    Utils.EncryptSecret = sec;



## <a id="portalsettings"></a>Přidání nastavení aplikace v portálu Azure (volitelné) ##
Pokud jste ještě webovou aplikaci Azure teď přidáním skutečné hodnoty pro AppSettings na portálu Azure. Tímto způsobem, správných hodnot nebudou v web.config ale chráněné prostřednictvím portálu, kde máte možnosti řízení oddělených přístupů. Tyto hodnoty nahradí pro hodnoty, které jste zadali v souboru web.config. Ujistěte se, že názvy jsou stejné.

![Nastavení aplikace v portálu Azure][1]


## <a name="authenticate-with-a-certificate-instead-of-a-client-secret"></a>Ověřování pomocí certifikátu místo tajná klienta
Další způsob, jak ověřit aplikace Azure AD je ID klienta a certifikát místo použije ID klienta a tajná klienta. Tady jsou kroky pro použití certifikát ve webové aplikaci pro Azure:

1. Získání nebo vytvoření certifikátu
2. Certifikát přidružit Azure AD aplikace
3. Přidání kódu pro webovou aplikaci pomocí certifikátu
4. Přidání certifikátu do webové aplikace


**Získání nebo vytvoření certifikátu** Pro naše potřeby provedeme testovacího certifikátu. Tady je několik dostupných příkazů, které můžete použít na příkazovém řádku vývojář vytvořit certifikát. Změňte adresář na místo, kam chcete soubor certifikátu vytvořit.

    makecert -sv mykey.pvk -n "cn=KVWebApp" KVWebApp.cer -b 07/31/2015 -e 07/31/2016 -r
    pvk2pfx -pvk mykey.pvk -spc KVWebApp.cer -pfx KVWebApp.pfx -po test123

Poznamenejte si konečným datem a heslo pro .pfx (v tomto příkladu: 07/31/2016 a test123). Budete potřebovat je dole.

Další informace o vytváření testovacího certifikátu najdete v tématu [jak: vytvořit svůj vlastní testování certifikát](https://msdn.microsoft.com/library/ff699202.aspx)


**Přidružení certifikátu pomocí aplikace Azure AD** Teď, když máte certifikát, budete muset přidružit Azure AD aplikace. Ale to portálu pro správu Azure teď nepodporuje. Místo toho máte pomocí Powershellu. Tady jsou příkazy, které potřebujete k spuštění:

    $x509 = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2

    PS C:\> $x509.Import("C:\data\KVWebApp.cer")

    PS C:\> $credValue = [System.Convert]::ToBase64String($x509.GetRawCertData())

    PS C:\> $now = [System.DateTime]::Now

    # this is where the end date from the cert above is used
    PS C:\> $yearfromnow = [System.DateTime]::Parse("2016-07-31")

    PS C:\> $adapp = New-AzureRmADApplication -DisplayName "KVWebApp" -HomePage "http://kvwebapp" -IdentifierUris "http://kvwebapp" -KeyValue $credValue -KeyType "AsymmetricX509Cert" -KeyUsage "Verify" -StartDate $now -EndDate $yearfromnow

    PS C:\> $sp = New-AzureRmADServicePrincipal -ApplicationId $adapp.ApplicationId

    PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName 'contosokv' -ServicePrincipalName $sp.ServicePrincipalName -PermissionsToSecrets all -ResourceGroupName 'contosorg'

    # get the thumbprint to use in your app settings
    PS C:\>$x509.Thumbprint

Po spuštění tyto příkazy uvidíte aplikace v Azure AD. Pokud nevidíte aplikaci na první, vyhledejte "Aplikací vlastní naší společnosti" místo "Využití aplikací naší společnosti".

Další informace o Azure AD aplikace objekty a ServicePrincipal, najdete v článku [aplikace a služby jistinu objekty](../active-directory/active-directory-application-objects.md)



**Přidat kód pro webovou aplikaci pomocí certifikátu** Nyní přidáme kód do webové aplikace přístup certifikát a použijte ji k ověření.

Nejprve je kód pro přístup k certifikátu.

    public static class CertificateHelper
    {
        public static X509Certificate2 FindCertificateByThumbprint(string findValue)
        {
            X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
            try
            {
                store.Open(OpenFlags.ReadOnly);
                X509Certificate2Collection col = store.Certificates.Find(X509FindType.FindByThumbprint,
                    findValue, false); // Don't validate certs, since the test root isn't installed.
                if (col == null || col.Count == 0)
                    return null;
                return col[0];
            }
            finally
            {
                store.Close();
            }
        }
    }


Všimněte si, že StoreLocation CurrentUser místo LocalMachine. A že jsme jsou poskytnutí false metody najít protože používáme testovací certifikát.


Dál je kód, který používá CertificateHelper a vytváří ClientAssertionCertificate, která je potřebná k ověření.

    public static ClientAssertionCertificate AssertionCert { get; set; }

    public static void GetCert()
    {
        var clientAssertionCertPfx = CertificateHelper.FindCertificateByThumbprint(WebConfigurationManager.AppSettings["thumbprint"]);
        AssertionCert = new ClientAssertionCertificate(WebConfigurationManager.AppSettings["clientid"], clientAssertionCertPfx);
    }


Tady je nový kód získat přístupový token. Nahrazuje metodu GetToken. Můžu jste zadali je jiný název pro usnadnění.

    public static async Task<string> GetAccessToken(string authority, string resource, string scope)
    {
        var context = new AuthenticationContext(authority, TokenCache.DefaultShared);
        var result = await context.AcquireTokenAsync(resource, AssertionCert);
        return result.AccessToken;
    }

Do třídy Utils Moje aplikace project Web App pro snadnější použití jste umístili všechny tento kód

Poslední změnu kód je v metody Application_Start. Nejdřív potřeba volat metodu GetCert() načíst ClientAssertionCertificate. A potom Změníme jsme zadat při vytváření nové KeyVaultClient metodu zpětného volání. Poznámka: Toto nahrazuje kód, který jsme měli nad.

    Utils.GetCert();
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetAccessToken));


**Přidání certifikátu do webové aplikace pomocí portálu Azure** Přidání certifikátu do webové aplikace je jednoduchých krocích. Nejdřív přejděte na portál Azure a přejděte na Web Appu. Na zásuvné nastavení pro webovou aplikaci klikněte na položku "vlastní domény a SSL". V zobrazeném můžete zásuvné budou moct nahrát certifikát, který jste vytvořili nad KVWebApp.pfx, ujistěte se, že si vzpomenout na heslo pfx.

![Přidání certifikátu do webových aplikací na portálu Azure][2]


Poslední věcí, které musíte udělat, je přidání nastavení aplikace do webové aplikace obsahující název webu\_zatížení\_certifikáty a hodnotu *. Zajistíte, že jsou všechny certifikáty načíst. Pokud byste chtěli načíst pouze certifikáty, které jste nahráli, můžete zadat hodnoty oddělené seznam jejich thumbprints.

Další informace o tom, že certifikát Web Appu najdete v tématu [Použití certifikátů v aplikacích weby Azure](https://azure.microsoft.com/blog/2014/10/27/using-certificates-in-azure-websites-applications/)


**Přidání certifikátu do klíč trezoru jako tajná** Místo odesílání certifikátu přímo ve službě v prohlížeči, můžete uložit v klíč trezoru jako tajná a nasazení odtud. Toto je proces se dvěma kroky, které je uvedené v následující příspěvku na blogu, [Nasazení Azure webové aplikace prostřednictvím certifikát trezoru klíč](https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/)



## <a id="next"></a>Další kroky ##


Programování odkazy, najdete v článku Principy [Azure klíč trezoru C# klientského rozhraní API](https://msdn.microsoft.com/library/azure/dn903628.aspx).


<!--Image references-->
[1]: ./media/key-vault-use-from-web-application/PortalAppSettings.png
[2]: ./media/key-vault-use-from-web-application/PortalAddCertificate.png
