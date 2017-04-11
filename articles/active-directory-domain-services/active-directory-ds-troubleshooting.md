<properties
    pageTitle="Služby Azure Active Directory Domain Services: Průvodce pro řešení | Microsoft Azure"
    description="Příručka pro řešení potíží pro Azure AD Domain Services"
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
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="maheshu"/>

# <a name="azure-ad-domain-services---troubleshooting-guide"></a>Azure AD Domain Services – Průvodce pro řešení
Tento článek obsahuje pokyny k odstranění potíží problémů, ke kterým může dojít při nastavení nebo Správa služeb Domain Azure Active Directory (AD).


## <a name="you-cannot-enable-azure-ad-domain-services-for-your-azure-ad-directory"></a>Není možné povolit Azure AD Domain služby Azure AD adresáře
Tato část umožňuje odstraňování chyb při pokusu o povolení služby Azure AD Domain pro adresáře a selže nebo se přepnete zpátky na "Zakázáno".

Vyberte Poradce při potížích pokynů, které odpovídají chybová zpráva, ke kterým dojít.

|**Chybová zpráva**|**Rozlišení**|
|---|:---|
|*Název contoso100.com se už používá v této síti. Zadejte název, který se nepoužívá.*|[Konflikt názvů domén v virtuální sítě](active-directory-ds-troubleshooting.md#domain-name-conflict)
|*V tomto klientovi Azure AD nelze povolit Domain Services. Služba nemá dostatečná oprávnění k aplikaci s názvem "Synchronizace služby Azure AD domény". Odstranění aplikace s názvem "Synchronizace služby Azure AD domény" a zkuste se dají povolit služby domén pro vašeho klienta Azure AD.*|[Domain Services nemá odpovídající oprávnění aplikace synchronizaci služby Azure AD domény](active-directory-ds-troubleshooting.md#inadequate-permissions)|
|*V tomto klientovi Azure AD nelze povolit Domain Services. Domain Services aplikace ve vašem klientovi Azure AD nemá požadovaná oprávnění k povolení Domain Services. Odstranění aplikace s d87dcbc6-a371-462e-88e3-28ad15ec4e64 identifikátor aplikace a zkuste se dají povolit služby domén pro vašeho klienta Azure AD.*|[Aplikaci Domain Services není správně nakonfigurovaný ve vašem klientovi](active-directory-ds-troubleshooting.md#invalid-configuration)|
|*V tomto klientovi Azure AD nelze povolit Domain Services. Aplikace Microsoft Azure AD je vypnutá, ve vašem klientovi Azure AD. Povolit aplikaci 00000002-0000-0000-c000-000000000000 identifikátor aplikace a pak se pokusíte povolení služby domén pro vašeho klienta Azure AD.*|[Aplikace Microsoft Graph je vypnutá, ve vašem klientovi Azure AD](active-directory-ds-troubleshooting.md#microsoft-graph-disabled)|


### <a name="domain-name-conflict"></a>Konflikt názvů domén
**Chybová zpráva:**

*Název contoso100.com se už používá v této síti. Zadejte název, který se nepoužívá.*

**Remediation:**

Ujistěte se, že nemusíte stávající domény se stejným názvem domény k dispozici tento virtuální síti se systémem. Například se předpokládá, že máte doménu s názvem "contoso.com" už k dispozici na vybraný virtuální sítě. Později pokusíte povolit doménu spravovaný Azure AD Domain Services se stejným názvem domény (to znamená "contoso.com") na tento virtuální sítě. Při používání narazíte na chybu při pokusu o povolení Azure AD Domain Services.

Tato chyba je kvůli konflikty jmen pro název domény, které virtuální síti se systémem. V takovém případě musí použijte jiný název pro nastavení domény spravovaných Azure AD Domain Services. Případně můžete zrušit zřízení stávající domény a potom přejděte k povolení Azure AD Domain Services.


### <a name="inadequate-permissions"></a>Dostatečná oprávnění
**Chybová zpráva:**

*V tomto klientovi Azure AD nelze povolit Domain Services. Služba nemá dostatečná oprávnění k aplikaci s názvem "Synchronizace služby Azure AD domény". Odstranění aplikace s názvem "Synchronizace služby Azure AD domény" a zkuste se dají povolit služby domén pro vašeho klienta Azure AD.*

**Remediation:**

Zkontrolujte, zda je aplikace s názvem "Azure AD Domain služby synchronizace" v adresáři Azure AD. Pokud tuto aplikaci existuje, odstraňte ji a znovu povolit Azure AD Domain Services.

Proveďte následující kroky a kontrola stavu aplikace ji odstranit, pokud existuje aplikace:

  1. Přejděte na **portál Azure klasické** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).
  2. Vyberte uzel **Služby Active Directory** v levém podokně.
  3. Vyberte Azure AD klienta (adresář), pro kterou chcete povolit Azure AD Domain Services.
  4. Přejděte na kartu **aplikace** .
  5. V rozevíracím seznamu vyberte možnost **aplikací vlastní naší společnosti** .
  6. Vyhledat aplikace s názvem **synchronizace služby Azure AD domény**. Pokud aplikace existuje, přejděte na odstranit.
  7. Po odstranění aplikace zkuste znovu povolit Azure AD Domain Services.


### <a name="invalid-configuration"></a>Neplatná konfigurace
**Chybová zpráva:**

*V tomto klientovi Azure AD nelze povolit Domain Services. Domain Services aplikace ve vašem klientovi Azure AD nemá požadovaná oprávnění k povolení Domain Services. Odstranění aplikace s d87dcbc6-a371-462e-88e3-28ad15ec4e64 identifikátor aplikace a zkuste se dají povolit služby domén pro vašeho klienta Azure AD.*

**Remediation:**

Zkontrolujte, jestli máte v adresáři vašeho Azure AD aplikace s názvem "AzureActiveDirectoryDomainControllerServices" (s aplikací identifikátor d87dcbc6-a371-462e-88e3-28ad15ec4e64). Pokud existuje tuto aplikaci, musíte ji odstranit a pak znovu povolit Azure AD Domain Services.

Použijte tento skript Powershellu najít aplikace a odstraňte ji.

> [AZURE.NOTE] Tento skript používá rutiny **Prostředí PowerShell Azure AD verze 2** . Úplný seznam všech dostupných rutin a ke stažení modulu naleznete v [dokumentaci AzureAD Powershellu](https://msdn.microsoft.com/library/azure/mt757189.aspx).

```
$InformationPreference = "Continue"
$WarningPreference = "Continue"

$aadDsSp = Get-AzureADServicePrincipal -Filter "AppId eq 'd87dcbc6-a371-462e-88e3-28ad15ec4e64'" -ErrorAction Ignore
if ($aadDsSp -ne $null)
{
    Write-Information "Found Azure AD Domain Services application. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $aadDsSp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services application."
}

$identifierUri = "https://sync.aaddc.activedirectory.windowsazure.com"
$appFilter = "IdentifierUris eq '" + $identifierUri + "'"
$app = Get-AzureADApplication -Filter $appFilter
if ($app -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync application. Deleting it ..."
    Remove-AzureADApplication -ObjectId $app.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync application."
}

$spFilter = "ServicePrincipalNames eq '" + $identifierUri + "'"
$sp = Get-AzureADServicePrincipal -Filter $spFilter
if ($sp -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync service principal. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $sp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync service principal."
}
```
<br>

### <a name="microsoft-graph-disabled"></a>Aplikace Microsoft Graph zakázaný
**Chybová zpráva:**

V tomto klientovi Azure AD nelze povolit Domain Services. Aplikace Microsoft Azure AD je vypnutá, ve vašem klientovi Azure AD. Povolit aplikaci 00000002-0000-0000-c000-000000000000 identifikátor aplikace a pak se pokusíte povolení služby domén pro vašeho klienta Azure AD.

**Remediation:**

Zkontrolujte, pokud jste zakázali aplikaci s identifikátorem 00000002-0000-0000-c000-000000000000. Tato aplikace je aplikace Microsoft Azure AD a poskytuje rozhraní API grafu přístup k vašemu tenantovi Azure AD. Azure AD Domain Services musí tuto aplikaci aktivní synchronizovat vašeho klienta Azure AD pro vaši doménu spravovaný.

Tuto chybu můžete vyřešit, povolení této aplikace a pak se pokusíte povolení služby domén pro vašeho klienta Azure AD.


## <a name="users-are-unable-to-sign-in-to-the-azure-ad-domain-services-managed-domain"></a>Uživatelé nemůžou přihlásit spravovaných domain Azure AD Domain Services
-Li jeden nebo víc uživatelů ve vašem klientovi Azure AD nelze se přihlásit do nově vytvořený spravované domény, proveďte následující kroky:

- **Přihlašovací formátu UPN:** Zkuste se přihlásit pomocí formátu Přípony (třeba 'joeuser@contoso.com') místo formátu SAMAccountName (CONTOSO\joeuser). SAMAccountName může jejíž předpona UPN je příliš dlouhá nebo je shodný s jiným uživatelem ve spravovaných doméně automaticky vygenerují pro uživatele. Formát UPN je musí být jedinečný v rámci Azure AD klienta.

> [AZURE.NOTE] Doporučujeme používat formát UPN se přihlásit k spravovaných domain Azure AD Domain Services.

- Ujistěte se, že máte [povolená synchronizace hesel](active-directory-ds-getting-started-password-sync.md) podle kroků uvedených v příručce Začínáme.

- **Externí účty:** Zajistěte, aby byl problémového uživatelský účet není externí účet Azure AD klientovi. Externí účty příklady účtů Microsoft (například 'joe@live.com') nebo uživatelských účtů z externího Azure AD adresář. Protože Azure AD Domain Services nemá přihlašovací údaje pro tyto uživatelských účtů, tito uživatelé nemůžete přihlásit ke spravované domény.

- **Synchronizované účty:** Pokud se synchronizují problémového uživatelských účtů z místního adresáře, zkontrolujte následující skutečnosti:
    - Můžete nasadit nebo aktualizují na [nejnovější doporučené verzi Azure AD Connect](active-directory-ds-getting-started-password-sync.md#install-or-update-azure-ad-connect).

    - Jste nakonfigurovali Azure AD Connect provádět [úplné synchronizaci](active-directory-ds-getting-started-password-sync.md).

    - V závislosti na velikosti adresáře může chvíli trvat, než uživatelské účty a pověření hash byl dostupný pod Azure AD Domain Services. Zajistěte, aby že odložíte dost dlouho opakování ověřování (v závislosti na velikosti adresáře - několik hodin za den nebo dvě pro velké adresáře).

    - Pokud problém přetrvává po ověření předchozích kroků, zkuste restartovat službu Microsoft Azure AD Sync. Na počítači synchronizační spustit příkazový řádek a následující příkazy:
      1. čistý zastavit "Microsoft Azure AD Sync"
      2. čistý start "Microsoft Azure AD Sync"

- **Účty jen cloudu**: Pokud problémového uživatelský účet je jen cloudu uživatelský účet, ujistěte se, že uživatel změnil heslo po povolení služby Azure AD domény. Tento krok způsobí, že pověření hash požadovaný pro služby Azure AD domény generovat.


## <a name="users-removed-from-your-azure-ad-tenant-are-not-removed-from-your-managed-domain"></a>Uživatelé odebrali Azure AD klienta nebudou odebrány z spravované domény
Azure AD chrání před nechtěným odstraněním objektů uživatelů. Po odstranění uživatelského účtu z vašeho klienta Azure AD odpovídající objektu uživatele přesune do koše. Při odstranění synchronizována na vaši doménu spravovaný, způsobí odpovídající uživatelský účet označit zakázané. Tato funkce umožňuje obnovit nebo později obnovit uživatelský účet.

Úplně odebrat uživatelský účet spravované domény, trvale odstraňte uživatele z Azure AD klienta. Získáte pomocí rutiny prostředí PowerShell odebrat-MsolUser s "-RemoveFromRecycleBin" možnost, jak je uvedeno v tomto [článku MSDN o](https://msdn.microsoft.com/library/azure/dn194132.aspx).


## <a name="contact-us"></a>Kontaktujte nás
Obraťte se na tým technické Azure Active Directory Domain Services na [sdílení názoru nebo o podporu] (aktivní adresáře – pošta – kontakt – us.md).
