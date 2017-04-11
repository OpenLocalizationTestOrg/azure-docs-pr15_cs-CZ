<properties
    pageTitle="Azure AD Domain Services: Povolení synchronizace hesel | Microsoft Azure"
    description="Začínáme s Azure Active Directory Domain Services"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/20/2016"
    ms.author="maheshu"/>

# <a name="enable-password-synchronization-to-azure-ad-domain-services"></a>Povolení synchronizace hesel Azure AD Domain Services
V předchozím úkoly povolené služby Azure AD domén pro vašeho klienta Azure AD. Je další úkol k povolení synchronizace hesel Azure AD Domain Services. Po nastavení synchronizace pověření si mohli uživatelé přihlašovat spravovaných doménu pomocí své přihlašovací údaje společnosti.

Kroky se různých podle toho, jestli vaše organizace má jenom cloudu Azure AD klient nebo je nastavený na synchronizovat s svého místního adresáře pomocí Azure AD Connect.

<br>

> [AZURE.SELECTOR]
- [Jen cloudu Azure AD klienta](active-directory-ds-getting-started-password-sync.md)
- [Synchronizovat Azure AD klienta](active-directory-ds-getting-started-password-sync-synced-tenant.md)

<br>


## <a name="task-5-enable-password-synchronization-to-aad-domain-services-for-a-synced-azure-ad-tenant"></a>Krok 5: Povolení synchronizace hesel AAD Domain Services pro synchronizované Azure AD klienta
A synchronizovat Azure AD klienta nastavená synchronizovat síť se službou místního adresáře vaší organizace pomocí Azure AD Connect. Azure AD Connect nesynchronizuje NTLM a Kerberos pověření hash Azure AD ve výchozím nastavení. Použití služby Azure AD domény, musíte nakonfigurovat Azure AD Connect synchronizovat pověření hash potřebných pro ověřování NTLM a Kerberos. Podle těchto kroků povolit synchronizaci hash požadovaných pověření ke tenantovi Azure AD.


### <a name="install-or-update-azure-ad-connect"></a>Instalace nebo aktualizovat Azure AD Connect
Nainstalujte nejnovější doporučené verzi Azure AD Connect na počítači připojeném do domény. Pokud máte existující instanci Azure AD Connect nastavením, musíte aktualizujte nejnovější verzi Azure AD Connect. Chcete-li předejít známých problémů a chyb, které mohou již opravené, zajistěte, aby že vždy používat nejnovější verzi Azure AD Connect.

**[Připojení ke stažení Azure AD](http://www.microsoft.com/download/details.aspx?id=47594)**

Doporučené verze: **1.1.281.0** - publikovaných na 7 září 2016.

  > [AZURE.WARNING] MUSÍTE nainstalovat nejnovější doporučené verzi Azure AD Connect povolte pověření starší heslo (povinné pro ověřování protokolem Kerberos a NTLM) synchronizovat do vašeho tenanta Azure AD. Tato funkce není k dispozici v předchozích verzích Azure AD Connect nebo starší verze nástroj DirSync.

Pokyny k instalaci Azure AD Connect jsou k dispozici v následujícím článku: [Začínáme s aplikacemi Azure AD Connect](../active-directory/active-directory-aadconnect.md)


### <a name="enable-synchronization-of-ntlm-and-kerberos-credential-hashes-to-azure-ad"></a>Povolení synchronizace NTLM a Kerberos hash pověření Azure AD
Na každé AD doménové vynutit úplné synchronizaci, provést tento skript Powershellu a povolte hash všechny místní uživatele pověření synchronizovat do vašeho tenanta Azure AD. Tento skript umožňuje potřebných pro NTLM/Kerberos ověřit synchronizovat do vašeho tenanta Azure AD hash přihlašovacích údajů.

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"  
$azureadConnector = "<CASE SENSITIVE AZURE AD CONNECTOR NAME>"  
Import-Module adsync  
$c = Get-ADSyncConnector -Name $adConnector  
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1  
$c.GlobalParameters.Remove($p.Name)  
$c.GlobalParameters.Add($p)  
$c = Add-ADSyncConnector -Connector $c  
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $false   
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $true  
```

V závislosti na velikosti adresáře (počet uživatelů, skupin atd), synchronizace pověření hash Azure AD dlouho. Hesla bude použitelné na spravované domain Azure AD Domain Services brzy po hash pověření synchronizovali Azure AD.


<br>

## <a name="related-content"></a>Související obsah

- [Povolení synchronizace hesel AAD Domain Services pro jen cloudu Azure AD adresáře](active-directory-ds-getting-started-password-sync.md)

- [Spravovat spravovaná domain Azure AD Domain Services](active-directory-ds-admin-guide-administer-domain.md)

- [Spojení Windows virtuálního počítače a spravovaných domain Azure AD Domain Services](active-directory-ds-admin-guide-join-windows-vm.md)

- [Spojení červené klobouk Enterprise Linux virtuálního počítače a spravovaných domain Azure AD Domain Services](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
