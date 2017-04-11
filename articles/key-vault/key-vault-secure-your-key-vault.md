<properties
    pageTitle="Zabezpečené trezoru klíčové | Microsoft Azure"
    description="Správa přístupových oprávnění pro klíčové trezoru správy trezorů a klíče a tajemství. A mohli ověřovat modelu doplňku pro klíčové trezoru a jak zajistit trezoru klíče"
    services="key-vault"
    documentationCenter=""
    authors="amitbapat"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/07/2016"
    ms.author="ambapat"/>


# <a name="secure-your-key-vault"></a>Zabezpečené trezoru klíče

Azure trezoru klíč je do cloudové služby, které zabezpečuje šifrovacího klíče a tajemství (například certifikáty, připojovací řetězec, hesel) získáte cloudu. Vzhledem k tomu tato data se rozlišují malá písmena a business kritické, chcete-li zabezpečený přístup k klíčové trezorů tak, aby pouze oprávnění aplikace a uživatelům přístup k klíčové trezoru. Tento článek obsahuje přehled modelu klíčové trezoru přístupu, vysvětluje a tak mohli ověřovat a popisuje, jak zajistit přístup k klíčové trezoru aplikace cloudu příklad.

## <a name="overview"></a>Základní informace

Přístup k klíčové trezoru řídí dvě samostatná rozhraní: Správa rovině a data rovině. Pro oba plochy správné a tak mohli ověřovat požaduje před můžete získat přístup do klíčové trezoru, volající (uživatele nebo aplikaci). Ověřování zavádí identifikaci volajícího, zatímco se tak mohli ověřovat k určení jaké operací, volající bude moct dělat.

Ověřování správy rovině a rovině dat pomocí služby Azure Active Directory. Povolení správy rovině se používá pro řízení přístupu na základě rolí (RBAC) zatímco rovině dat používá zásady přístupu klíčové trezoru.

Tady je stručný přehled témat:

[Ověření pomocí služby Azure Active Directory](#authentication-using-azure-active-direcrory) – Tato část vysvětluje, jak volající ověří službou Azure Active Directory pro přístup k klíčové trezoru prostřednictvím správy rovině a data rovině. 

[Správa rovině a rovině dat](#management-plane-and-data-plane) - správy rovině a rovině dat jsou dva plochy Accessu používat pro přístup k trezoru klíčové. Každý rovině aplikace access podporuje konkrétní operace. Tato část popisuje koncové body přístup, operace podporované a přístup k řízení metodou tak, že každé plochy. 

[Řízení přístupu rovině](#management-plane-access-control) – v této části, které se podíváme na povolení přístupu k řízení plochy operace pomocí řízení přístupu na základě rolí.

[Data rovině řízení přístupu](#data-plane-access-control) – Tato část popisuje, jak používat zásady přístupu klíčové trezoru řízení přístupu k datům rovině.

[Příklad](#example) : v tomto příkladu popisuje, jak nastavit řízení přístupu k trezoru klíčů umožníte tři různé týmy (zabezpečení týmu vývojáři/operátory a auditory) k provádění konkrétních úkolů pro vývoj, Správa a sledování aplikace v Azure.


## <a name="authentication-using-azure-active-directory"></a>Ověření pomocí služby Azure Active Directory

Při vytváření klíčové trezoru v Azure předplatného se automaticky přiřazovat k tenantovi Azure Active Directory předplatného. Všechny volající (uživatelů a aplikací) musí být registrován v tohoto klienta pro přístup k trezoru klíče. Aplikace nebo uživatel musí ověřit službou Azure Active Directory pro přístup k klíčové trezoru. Platí to pro obě správy rovině a rovině přístup k datům. V obou případech dostanete aplikace klíčové trezoru dvěma způsoby:

-  **uživatele + aplikace access** – obvykle jde o aplikace, které přístup k klíčové trezoru jménem přihlášený uživatel. Azure Powershellu, Azure portál příkladů tento typ přístupu. Existují dva způsoby, jak udělit přístup uživatelům: jedním ze způsobů, je udělit přístup uživatelům, aby se můžou dostat k klíčové trezoru z libovolné aplikace a jiným způsobem je chcete udělit přístup uživatelů k klíčové trezoru jenom při použití určitou aplikaci (jako takzvaný složeného identity). 
-  **pouze aplikace access** – k aplikacím, které spustit démon služeb, pozadí úlohy atd. Identitu aplikace je povolený přístup klíčové trezoru.

V obou typů aplikace aplikace ověří službou Azure Active Directory použití libovolného příkazu pro [podporované metody ověřování](../active-directory/active-directory-authentication-scenarios.md) a získá token. Použité metodě ověřování závisí na typu aplikace. Potom aplikaci používá tento token a odešle žádost o rozhraní REST API klíčové trezoru. V případě přístupu ke správě rovině požadavky na směrovány koncový bod správce prostředků Azure. Při přístupu k dat plochy, aplikací rozhovory přímo ke klíči trezoru koncového bodu. Další informace najdete na webu [toku celé ověřování](../active-directory/active-directory-protocols-oauth-code.md). 

Název zdroje, u kterého aplikace požádá o token se liší podle toho, jestli aplikaci přístupu ke řízení plochy nebo dat rovině. Proto název zdroje je buď správy rovině nebo dat rovině koncový bod popsané v tabulce v další části, v závislosti na Azure prostředí.

Umístění jedné jednoho mechanismus při ověřování správy a data rovině přináší vlastní výhody:

- Organizace centrálně řízení přístupu k všechny klíčové trezory ve své organizaci
- Pokud uživatel opustí, budou okamžitě ztratíte přístup k všechny klíčové trezory v organizaci
- Organizace můžete přizpůsobit ověřovat pomocí možností v Azure Active Directory (například povolení vícefaktorové ověřování pro zvýšené zabezpečení)

## <a name="management-plane-and-data-plane"></a>Správa rovině a rovině dat

Azure trezoru klíč je Azure služba dostupné prostřednictvím Správce prostředků Azure nasazení modelu. Při vytváření klíčové trezoru dostanete virtuální kontejner, do kterého můžete vytvořit jiné objekty, například klíče, tajemství a certifikáty. Mohli dostat trezoru klíčové pomocí správy rovině a rovině data k provedení určité operací. Správa rovině rozhraní slouží ke správě trezoru klíčové samotné, například k vytváření, odstraňování, aktualizace klíčové trezoru atributy a nastavení zásady přístupu pro data rovině. Data rovině rozhraní slouží k přidat, odstranit, upravit a pomocí kláves, tajemství a certifikátů uložených v trezoru klíče.

Rozhraní správy rovině a data rovině se otevírají pomocí různých koncové body (viz tabulka). Druhý sloupec v tabulce popisuje názvy DNS pro tyto koncové body v různých prostředích Azure. Třetí sloupec popisuje operace, které mohou vykonávat z každé plochy přístup. Každé plochy Accessu má i svůj vlastní mechanismus řízení přístupu: řízení přístupu rovině je nastavena pomocí řízení přístupu Azure Resource Manager Role-Based (RBAC), zatímco pro řízení přístupu rovině dat je nastaven pomocí zásady přístupu klíčové trezoru.

| Rovině aplikace Access | Koncové body přístup | Operace | Mechanismus řízení přístupu|
|--------------|------------------|--------------------|--------|
| Správa plochy|**Globální:**<br> Management.Azure.com:443<br><br> **Azure Čína:**<br> Management.chinacloudapi.CN:443<br><br> **Azure vládní organizace:**<br> Management.usgovcloudapi.NET:443<br><br> **Azure Německo:**<br> Management.microsoftazure.de:443 | Vytvoření, čtení nebo aktualizace nebo odstranění klíčové trezoru <br> Nastavit zásady přístupu pro klíčové trezoru<br>Nastavení značky pro klíčové trezoru | Řízení přístupu na základě rolí správce Azure zdroje (RBAC) |
| Rovině dat | **Globální:**<br> &lt;Název trezoru&gt;. vault.azure.net:443<br><br> **Azure Čína:**<br> &lt;Název trezoru&gt;. vault.azure.cn:443<br><br> **Azure vládní organizace:**<br> &lt;Název trezoru&gt;. vault.usgovcloudapi.net:443<br><br> **Azure Německo:**<br> &lt;Název trezoru&gt;. vault.microsoftazure.de:443 | Pro klíče: Dešifrování, zašifrovat, UnwrapKey, WrapKey, ověřte, odhlásit, získat, seznam, aktualizace, vytvoření, Import, odstranit, zálohování, obnovení<br><br> Pro tajemství: získání, seznamu, nastavení, odstranit | Zásady přístupu klíčové trezoru|

Ovládací prvky správy rovině a data rovině přístup pracovat nezávisle na sobě. Pokud chcete udělit přístup aplikace použít v klíčové trezoru, stačí udělit zásady přístupu klíčové trezoru oprávnění pro přístup k datům rovině a bez přístupu ke správě rovině je potřeba k této aplikaci. A přístup k rovině dat je nutné a naopak, pokud chcete uživatelům moct číst vlastnosti trezoru a značky, ale nemáte přístup k klíčů, tajemství nebo certifikáty, můžete udělit tohoto uživatele, "read" připojení přes RBAC.

## <a name="management-plane-access-control"></a>Řízení přístupu plochy

Správa rovině se skládá z operacích, které ovlivňují klíčové trezoru samotné. Můžete například vytvořit nebo odstranit klíčové trezoru. Seznam trezorů může vstoupit předplatné. Můžete načíst klíčové trezoru vlastnosti (například SKU, značky) a nastavit klíčové trezoru zásady přístupu, kterými se řídí uživatelů a aplikací, které mají přístup k klíče a tajemství klíčové trezoru. Řízení přístupu rovině používá RBAC. Zobrazit úplný seznam klíčových trezoru operacích, které lze provést pomocí řízení plochy v tabulce v předchozí části. 

### <a name="role-based-access-control-rbac"></a>Řízení přístupu na základě rolí (RBAC)
Každý Azure předplatné má Azure Active Directory. Uživatelé, skupiny a aplikacemi od tohoto adresáře můžete k ní mít udělený přístup k přidávání a používání zdrojů v Azure předplatné, které používají správce prostředků Azure nasazení modelu. Tento typ řízení přístupu je uvedená jako řízení přístupu na základě rolí (RBAC). Ke správě tento přístup, můžete [Azure portál](https://portal.azure.com/), [rozhraní příkazového řádku Azure nástroje](../xplat-cli-install.md), [prostředí PowerShell](../powershell-install-configure.md)nebo [Azure správce prostředků REST API](https://msdn.microsoft.com/library/azure/dn906885.aspx).

Správce prostředků Azure modelu vytvoříte trezoru klíčové v zdroje skupiny a řízení přístupu k rovině správy klíčů trezoru pomocí služby Azure Active Directory. Můžete například udělit uživatele nebo skupiny možnost spravovat klíčové trezorů v určité skupiny zdrojů.

Přiřazením příslušné role RBAC můžete udělit přístup uživatelů, skupin a aplikací na určitý obor. Například udělujete přístup uživatelům spravovat klíčové trezorů by přiřadit roli předdefinované klíčové trezoru skupiny přispěvatelů pro tohoto uživatele na určitý obor. Obor v tomto případě bude předplatné, skupina zdroje nebo jenom určité klíčové trezoru. Role přiřazené na úrovni předplatné platí pro všechny skupiny zdrojů a zdroje v rámci tohoto předplatného. Role přiřazené na úrovni zdroje skupiny platí pro všechny zdroje v dané skupině zdroje. Role přiřazené k určitému zdroji platí jenom pro daný zdroj. Existuje několik předdefinovaných rolí (viz [RBAC: předdefinované role](../active-directory/role-based-access-built-in-roles.md)), a Jestliže předdefinované role vašim potřebám nevyhovuje můžete definovat vlastní role.

>[AZURE.IMPORTANT] Všimněte si, že pokud uživatel má přispěvatelů udělit oprávnění (RBAC) rovině správy klíčů trezoru, se dají še sebe přístup k rovině data tak, že nastavení klíčové trezoru zásady přístupu, které řídí přístup k rovině data. Proto doporučujeme pevně určit, kdo má přístup "Přispěvatelů" k klíčové trezorů zajistit pouze oprávněnými osoby můžete zpřístupnit a spravovat klíčové trezorů, klíče, tajemství a certifikáty.

## <a name="data-plane-access-control"></a>Řízení přístupu rovině dat

Rovině klíčové trezoru dat se skládá z operacích, které ovlivňují objektů v klíčové trezoru, například klíče, tajemství a certifikáty.  Jedná se o klíč, jako například vytvořená operace, import, aktualizace, seznamu, zálohování a obnovení klíče cryptographic operací, jako je třeba podepsat, ověřte, šifrování, dešifrování, zalomit a Zrušit zalomení a nastavit značky a další atributy klíče. Podobně tajemství, který obsahuje, získáte nastavení seznamu, odstranit.

Přístup k datům rovině uděleno nastavením zásady přístupu pro klíčové trezoru. Uživateli, skupině nebo aplikaci vyžaduje oprávnění Přispěvatel (RBAC) pro správu rovině pro klíčové trezoru mohli nastavit zásady přístupu pro tohoto klíče trezoru. Uživateli, skupině nebo aplikace můžete k ní mít udělený přístup k provedení určité operací pro klíčů nebo tajemství klíčové trezoru. klíčové trezoru podporu až 16 položky zásady přístupu klíčové trezoru. Vytvoření skupiny zabezpečení Azure Active Directory a přidání uživatelů do skupiny, které chcete udělit přístup k datům rovině několika uživatelů do klíčové trezoru.

### <a name="key-vault-access-policies"></a>klíčové trezoru zásady přístupu

zásady přístupu klíčové trezoru udělení oprávnění ke klíče, tajemství a certifikáty samostatně. Můžete například udělit přístup uživatelů k pouze klíče, ale bez oprávnění pro tajemství. Oprávnění pro přístup k klíče nebo tajemství nebo certifikáty jsou však na úrovni trezoru. Jinými slovy zásady přístupu klíčové trezoru podporuje oprávnění na úrovni objektů. [Azure portál](https://portal.azure.com/), [rozhraní příkazového řádku Azure nástroje](../xplat-cli-install.md), [prostředí PowerShell](../powershell-install-configure.md)nebo [klíčové trezoru REST API správy](https://msdn.microsoft.com/library/azure/mt620024.aspx) slouží k nastavení zásady přístupu pro klíčové trezoru.

>[AZURE.IMPORTANT] Všimněte si, že zásady přístupu klíčové trezoru použít na úrovni trezoru. Například při uživatele udělení oprávnění k vytváření a odstraňování klíče mohla v můžete dělat tyto operace na všechny klíče tohoto klíče trezoru.

## <a name="example"></a>Příklad

Řekněme, že vyvíjíte aplikace, která používá certifikát SSL, Azure úložiště pro ukládání dat a používá RSA 2048bitový klíč pro operace podepsat. Řekněme, že tato aplikace spuštěna virtuálního počítače (nebo nastavte měřítko OM). Použijte klíčové trezoru uchovávat všechny tajemství aplikace a umožňuje ukládat zavádění certifikát, který používá aplikaci ověření s Azure Active Directory klíčové trezoru.

Ano tady je souhrn všech klíče a tajemství uložený v klíčové trezoru.

- **Certifikát SSL** - použitý pro protokol SSL
- **Klíč úložiště** - používaný k získání přístupu k účtu úložiště
- **RSA 2048bitový klíč** – použitý pro operace znak
- **Zavádění certifikát** – slouží k ověření Azure Active Directory, abyste získali přístup k klíčové trezoru načíst klávesu úložiště a pomocí klávesy RSA podpisu.

Teď Pojďme zahájit lidmi, kteří jsou Správa nasazení a auditování této aplikace. V tomto příkladu budeme používat tři role.

- **Zabezpečení týmu** – Toto jsou obvykle zaměstnance IT z office CSO (hlavní bezpečnostní úředník) nebo rovnocenný, zodpovědný za správné bezpečně uchovával tajemství například certifikáty SSL, RSA klíče sloužící k podpisu, připojovací řetězec, pro databáze úložiště účtu klíče.
- **Vývojáři/operátory** - Toto jsou osoby, kteří vyvíjet této aplikace a potom ho nasadit v Azure. Obvykle nejsou součástí týmových zabezpečení a tedy neměli mít přístup citlivá data, například certifikáty SSL, RSA klíče, ale aplikaci, kterou budou nasazení měli přístup k těmto.
- **Auditory** – je to obvykle jinou skupinu lidí, samostatný od vývojáře a obecné zaměstnance IT. Zodpovídají je zkontrolujte správné použití a údržba certifikáty, klíče atd a dodržování standardů zabezpečení data. 

Existuje jednu další roli, který je mimo rozsah této aplikace, ale důležité zde uváděného a, který bude předplatné (nebo pole Skupina zdroje) správce. Správce předplatné nastaví počáteční přístupová oprávnění pro tým zabezpečení. Tady jsme se předpokládá, že předplatné správce udělil přístup týmu zabezpečení skupiny zdrojů ve které všechny prostředky potřebné pro bydliště této aplikace.

Teď si ukážeme, jaké akce provádí jednotlivým rolím v rámci této aplikace.

- **Zabezpečení týmu**
    - Vytvoření klíčových trezorů
    - Zapnutí klávesy trezoru protokolování
    - Přidání klíče/tajemství
    - Vytvořit záložní kopii klíčů havárie obnovení
    - Nastavit zásady přístupu klíčové trezoru udělit oprávnění uživatelů a aplikací k provádění operací, konkrétní
    - Pravidelně vrátit klíče/tajemství
- **Vývojáři/operátory**
    - Získat odkazy na bootstrap a certifikáty SSL (thumbprints), úložiště (tajná URI) spolu s přihlášením key (klíč URI) od týmu služeb zabezpečení
    - Vytvoření a nasazení aplikace, která programově přistupuje klíče a tajemství
- **Auditory**
    - Zobrazení protokolů použití potvrďte správné použití klávesy/tajná a dodržování standardy zabezpečení dat.

Teď si ukážeme, jaká oprávnění k přístupu k klíčové trezoru jsou vyžadovány každou roli (a aplikace) k provedení přiřazených úkolů. 

| Role uživatele    | Oprávnění ke správě plochy | Oprávnění k rovině dat. |
|--------------|------------------------------|------------------------|
|Zabezpečení týmu|klíčové trezoru přispěvatelů|Klíče: zálohovat, vytvoření, odstranění, zjištění import, seznam, obnovení <br> Tajemství: všechny|
|Vývojáři/operátor| klíčové trezoru nasazení oprávnění tak, aby VMs jejich nasazením můžete načíst tajemství z trezoru klíče | Žádná |
|Auditory| Žádná | Klíče: seznam<br>Tajemství: seznam|
|Aplikace| Žádná | Klíče: symbol<br>Tajemství: get |

>[AZURE.NOTE] Auditory potřebujete oprávnění pro klíče a tajemství zobrazovat, můžete zkontrolovat atributy pro klíče a tajemství, které nejsou vyzařovaného v protokolech, například značky, aktivaci a datum vypršení platnosti.

Kromě oprávnění k klíčové trezoru všechny tři role taky potřebovat přístup k jiným zdrojům. Například, abyste mohli nasadit VMs (nebo webových aplikací atd.) Vývojáři/operátory taky "Přispěvatelů" k potřebují mít přístup jenom určité typy zdrojů. Auditory potřebují mít přístup pro čtení k účtu úložiště, kde jsou uložené protokoly klíčové trezoru.

Protože fokus v tomto článku je zabezpečení přístupu k trezoru klíčů, jenom ilustrují odpovídající části vztahující se na toto téma jsme Přeskočit podrobnosti týkající se nasazení certifikátů, přístup k klíče a tajemství programově atd. Tyto informace jsou už nichž se uplatní kdekoli jinde. Nasazení certifikátů uložené v klíčové trezoru VMs jsou obsaženy v [blogu](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/)a je [ukázkový kód](https://www.microsoft.com/download/details.aspx?id=45343) k dispozici, který ukazuje, jak používat zavádění certifikát ověření Azure AD abyste získali přístup k klíčové trezoru.

Většina přístupová oprávnění můžete k ní mít udělený Azure portálu, ale pokud chcete udělit oprávnění podrobného budete muset pomocí prostředí PowerShell Azure (nebo rozhraní příkazového řádku Azure) dosáhnout požadované výsledky. 

Předpokládejme následující zlomky Powershellu:

- Skupiny zabezpečení, které představují tři role, a to Contoso zabezpečení týmu Contoso aplikace Devops Contoso aplikace auditory vytvořil správce služby Azure Active Directory. Správce také přidal uživatelů do skupin, do kterých patří.

- **ContosoAppRG** je skupina zdroje, kde jsou uloženy všechny zdroje. **contosologstorage** je, kde jsou uložené protokoly. 

- Klíčové trezoru **ContosoKeyVault** a úložiště účet pro klíčové trezoru protokoly **contosologstorage** musí být ve stejném umístění Azure


Nejdřív správce předplatné přiřadí "základní trezoru přispěvatelů" a "správce přístup uživatelů" role týmu zabezpečení. Díky zabezpečení týmu spravovat přístup k jiným zdrojům a spravovat klíčové trezorů ve skupině prostředků ContosoAppRG.

```
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "key vault Contributor" -ResourceGroupName ContosoAppRG
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "User Access Administrator" -ResourceGroupName ContosoAppRG
```

Tento skript ukazuje, jak týmu zabezpečení můžete vytvořit klíčové trezoru nastavení protokolování i u ostatních rolí a aplikaci nastavit přístupová oprávnění. 

```
# Create key vault and enable logging
$sa = Get-AzureRmStorageAccount -ResourceGroup ContosoAppRG -Name contosologstorage
$kv = New-AzureRmKeyVault -VaultName ContosoKeyVault -ResourceGroup ContosoAppRG -SKU premium -Location 'westus' -EnabledForDeployment
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

# Data plane permissions for Security team
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -PermissionToKeys backup,create,delete,get,import,list,restore -PermissionToSecrets all

# Management plane permissions for Dev/ops
# Create a new role from an existing role
$devopsrole = Get-AzureRmRoleDefinition -Name "Virtual Machine Contributor"
$devopsrole.Id = $null
$devopsrole.Name = "Contoso App Devops"
$devopsrole.Description = "Can deploy VMs that need secrets from key vault"
$devlopsrole.AssignableScopes = @("/subscriptions/<SUBSCRIPTION-GUID>")

# Add permission for dev/ops so they can deploy VMs that have secrets deployed from key vaults
$devopsrole.Actions.Add("Microsoft.KeyVault/vaults/deploy/action")
New-AzureRmRoleDefinition -Role $role

# Assign this newly defined role to Dev ops security group
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Devops')[0].Id -RoleDefinitionName "Contoso App Devops" -ResourceGroupName ContosoAppRG

# Data plane permissions for Auditors
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Auditors')[0].Id -PermissionToKeys list -PermissionToSecrets list
```

Vlastní rolí definované, je jenom přiřadit k předplatnému tam, kde je skupina zdroje ContosoAppRG vytvořili. Pokud stejné vlastní role se použije pro jiné projekty v jiných předplatných, jeho obor mít víc předplatných přidali.

Přiřazení vlastní role pro vývojáře a operátory pro oprávnění "nasazení/akce" má obor vymezený na skupina zdroje. Tímto způsobem pouze VMs vytvořené ve skupině prostředků "ContosoAppRG" pošle tajemství (certifikátu SSL a zavádění certifikátu). VMs, členem týmu vývojáře/ops vytvořené v jiné skupiny prostředků nebude moct připojit tyto tajemství a to i vědom tajné URI.

Tento příklad znázorňuje jednoduchý scénář. Scénáře skutečných může být složitější a budete muset nastavit oprávnění pro trezoru klíčové podle vašich potřeb. Předpokládejme například, v našem příkladu jsme tento bezpečnostní tým poskytne klávesu spolu s tajné odkazy (URI a thumbprints), které týmu vývojáři/operátory nemusí odkazovat v aplikacích. Proto jsou není potřeba udělit vývojáři/operátory všechny datové rovině. Nezapomeňte, že v tomto příkladě se zaměřuje na zabezpečení trezoru klíče. Podobně jako by měl být zvážit příliš zabezpečení [vaší VMs](https://azure.microsoft.com/services/virtual-machines/security/), [účty úložiště](../storage/storage-security-guide.md) a další Azure zdroje.

>[AZURE.NOTE] Poznámka: Tento příklad ukazuje, jak klíčové trezoru access nebude možné v výroby. Vývojáři si měli svoje předplatné nebo resourcegroup kde mají oprávnění k úplnému ke správě svých trezorů, VMs a úložiště účtu kde vyvíjejí aplikace.


## <a name="resources"></a>Zdroje informací

-   [Řízení přístupu na základě rolí Azure Active Directory](../active-directory/role-based-access-control-configure.md)

    Tento článek vysvětluje řízení přístupu na základě rolí Azure Active Directory a jak to funguje.

-   [RBAC: Integrované role](../active-directory/role-based-access-built-in-roles.md)

    Tento článek obsahuje podrobnosti všechny předdefinované role v RBAC k dispozici.

-   [Principy správce prostředků nasazení a klasické nasazení](../resource-manager-deployment-model.md)

    Tento článek vysvětluje správce prostředků nasazení a modelů klasické nasazení a vysvětluje výhody používání skupin Správce zdrojů a zdrojů

-    [Správa řízení přístupu na základě rolí pomocí prostředí PowerShell Azure](../active-directory/role-based-access-control-manage-access-powershell.md)

     Tento článek vysvětluje, jak spravovat řízení přístupu na základě rolí pomocí prostředí PowerShell Azure

-   [Správa řízení přístupu na základě rolí pomocí rozhraní REST API](../active-directory/role-based-access-control-manage-access-rest.md)

    Tento článek ukazuje, jak používat rozhraní REST API pro správu RBAC.

-   [Řízení přístupu na základě rolí pro službu Microsoft Azure z Ignite](https://channel9.msdn.com/events/Ignite/2015/BRK2707)

    Toto je odkaz na video na kanál 9 z konference Ignite 2015 MS. V této relaci, budou si o přístup k řízení a možnosti vytváření sestav v Azure a prozkoumání doporučené postupy kolem zabezpečení přístupu k předplatným Azure pomocí služby Azure Active Directory.

-   [Povolit přístup k webové aplikace pomocí OAuth 2.0 a Azure Active Directory](../active-directory/active-directory-protocols-oauth-code.md)

    Tento článek popisuje dokončení OAuth 2.0 tok ověřování s Azure Active Directory.

-   [klíčové trezoru správy REST API](https://msdn.microsoft.com/library/azure/mt620024.aspx)

    Tento dokument je odkaz REST API ke správě trezoru klíčové programově, včetně nastavení zásady přístupu klíčové trezoru.

-   [klíčové trezoru rozhraní REST API](https://msdn.microsoft.com/library/azure/dn903609.aspx)

    Odkaz na klíčové trezoru si přečtěte následující dokumentaci rozhraní REST API odkaz.

-   [Řízení přístupu klíče](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_KeyAccessControl)

    Odkaz na dokumentaci ovládací prvek klíč přístup.

-   [Řízení přístupu tajné](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_SecretAccessControl)

    Odkaz na dokumentaci ovládací prvek klíč přístup.

-   [Nastavení](https://msdn.microsoft.com/library/mt603625.aspx) a [Odebrání](https://msdn.microsoft.com/library/mt619427.aspx) klíčové trezoru zásady přístupu pomocí prostředí PowerShell

    Odkazy na referenční si přečtěte následující dokumentaci pro rutiny prostředí PowerShell pro správu klíčů trezoru přístupu.

## <a name="next-steps"></a>Další kroky

Výukové dosáhnout toho, aby Začínáme pro správce najdete v článku [Začínáme s Azure klíčové trezoru](key-vault-get-started.md).

Další informace o použití protokolování pro klíčové trezoru najdete v článku [protokolování Azure klíčové trezoru](key-vault-logging.md).

Další informace o použití klávesy a tajemství s Azure klíčové trezoru najdete v článku [o klíče a tajemství](https://msdn.microsoft.com/library/azure/dn903623.aspx).

Pokud máte dotazy k klíčové trezoru, navštěvujte blog o [Azure klíčové trezoru fóra](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)
