<properties
    pageTitle="Účet řízení zdrojů s dávku správy .NET | Microsoft Azure"
    description="Vytvoření, odstranění a upravte Azure dávku účtu zdroje s knihovnou správy .NET dávku."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="10/19/2016"
    ms.author="marsma"/>

# <a name="manage-azure-batch-accounts-and-quotas-with-batch-management-net"></a>Správa účtů dávku Azure a kvóty dávku správy .NET

> [AZURE.SELECTOR]
- [Azure portálu](batch-account-create-portal.md)
- [Správa dávku .NET](batch-management-dotnet.md)

Můžete snížit údržbu nároky v aplikacích dávku Azure pomocí [Dávku správy .NET] [ api_mgmt_net] knihovny tak, aby automatizovat vytváření účtů dávku, odstranění, správy klíčů a zjišťování kvóty.

- **Vytvořit a odstranění dávky účtů** všechny oblasti. Pokud jako nezávisle prodejci například zadáte do služby pro klienty v nichž každý přiřazen samostatný účet dávku fakturační účely, můžete přidat účet vytváření a odstraňování možnosti na portál zákazníka.
- **Načtení a klíče regenerovat účtu** programově pro všechny účty dávku. Pomáhá plnit zásady zabezpečení vynucující pravidelných při přechodu myší nebo vypršení platnosti účtu klíče. Když budete mít několik dávku účtů v různých oblastech Azure, automatizaci tento proces přechodu zvyšuje efektivity vašeho řešení.
- **Zaškrtněte políčko účtu kvót** a opět převzít nepřesných zkušební verzi a chyby odlišené účty, které dávku mít jaká omezení. Zaškrtnutím účtu kvóty před spuštěním úlohy vytváření fondů nebo přidáním výpočetním uzlů, můžete upravit včasným where nebo když tyto výpočet zdroje vytvářejí. Můžete určit účty, které vyžadují kvóty zvyšuje před přiřazení dalšího zdroje v těchto účtů.
- **Kombinování funkce jiné služby Azure** pro plné řízení setkat i v případě – pomocí dávku správy .NET [Azure Active Directory][aad_about]a [Správce prostředků Azure] [ resman_overview] společně ve stejné aplikaci. Pomocí těchto funkcí a jejich rozhraní API zajistíte frictionless ověřování setkat i v případě, možnost vytváření a odstraňování skupin zdrojů a funkcí, které jsou popsané výše pro řešení pro správu začátku do konce.

> [AZURE.NOTE] Během Tento článek se zaměřuje na programové Správa účtů dávku, klíče a kvót, můžete nastavit spoustu tyto činnosti pomocí [Azure portál][azure_portal]. Další informace najdete v tématu [Vytvoření účet Azure dávku na portálu Azure](batch-account-create-portal.md) a [kvót a limity pro službu Azure dávku](batch-quota-limit.md).

## <a name="create-and-delete-batch-accounts"></a>Vytváření a odstraňování dávku účty

Jak je uvedeno, primární funkcí rozhraní API správy dávku reprodukujte k vytváření a odstraňování dávku účty v Azure oblasti. Použijte [BatchManagementClient.Account.CreateAsync] [ net_create] a [DeleteAsync][net_delete], nebo jejich synchronní protějšky.

Následující fragment kódu vytvoří účet získá nově vytvořený účet služby dávku a potom jej odstraní. V tomto fragment kódu a jiné v tomto článku `batchManagementClient` je plně spuštění instancí [BatchManagementClient][net_mgmt_client].

```csharp
// Create a new Batch account
await batchManagementClient.Account.CreateAsync("MyResourceGroup",
    "mynewaccount",
    new BatchAccountCreateParameters() { Location = "West US" });

// Get the new account from the Batch service
AccountResource account = await batchManagementClient.Account.GetAsync(
    "MyResourceGroup",
    "mynewaccount");

// Delete the account
await batchManagementClient.Account.DeleteAsync("MyResourceGroup", account.Name);
```

> [AZURE.NOTE] Aplikace, které využívají knihovnu dávku správy .NET a svou třídu BatchManagementClient vyžaduje **Správce služeb** nebo **coadministrator** přístup k předplatnému, který vlastní dávku účet, který chcete spravovat. Další informace najdete v části [Azure Active Directory](#azure-active-directory) a [AccountManagement] [ acct_mgmt_sample] ukázka kódu.

## <a name="retrieve-and-regenerate-account-keys"></a>Načtení a obnovovat účtu klíče

Získat klíče primárních a sekundárních účtu z libovolného účtu dávku v rámci předplatného pomocí [ListKeysAsync][net_list_keys]. Tyto klíče můžete obnovit pomocí [RegenerateKeyAsync][net_regenerate_keys].

```csharp
// Get and print the primary and secondary keys
BatchAccountListKeyResult accountKeys =
    await batchManagementClient.Account.ListKeysAsync(
        "MyResourceGroup",
        "mybatchaccount");
Console.WriteLine("Primary key:   {0}", accountKeys.Primary);
Console.WriteLine("Secondary key: {0}", accountKeys.Secondary);

// Regenerate the primary key
BatchAccountRegenerateKeyResponse newKeys =
    await batchManagementClient.Account.RegenerateKeyAsync(
        "MyResourceGroup",
        "mybatchaccount",
        new BatchAccountRegenerateKeyParameters() {
            KeyName = AccountKeyType.Primary
            });
```

> [AZURE.TIP] Vytvoření pracovního postupu optimalizovaného připojení pro správu aplikace. Nejprve získat klíč účtu pro účet dávku chcete provádět správu přes [ListKeysAsync][net_list_keys]. Potom provádějte tento klíč inicializace knihovnu dávku .NET [BatchSharedKeyCredentials] [ net_sharedkeycred] předmětu, které se používá při inicializace [BatchClient][net_batch_client].

## <a name="check-azure-subscription-and-batch-account-quotas"></a>Kontrola Azure předplatné a dávku účtu kvót

Azure předplatná a jednotlivých Azure služeb, jako se dávkové všechny mají výchozí kvóty, které omezit počet určitých entit obsahují. Výchozí kvóty pro Azure předplatné najdete v článku [Azure předplatné a omezení služby, kvót a omezení](./../azure-subscription-service-limits.md). Výchozí kvóty službu dávku najdete v článku [kvót a limity pro službu Azure dávku](batch-quota-limit.md). Pomocí knihovnu dávku správy .NET se změnami jde kvót aplikace. Umožňuje rozhodovat přidělení před přidání účtů nebo výpočet zdrojů, jako jsou fondů a výpočet uzlů.

### <a name="check-an-azure-subscription-for-batch-account-quotas"></a>Kontrola předplatné Azure dávku účtu kvót

Před vytvořením účet dávku v oblasti, můžete zkontrolovat, že předplatné Azure zobrazíte, zda je možné přidat účet v dané oblasti.

V následujícím fragment kódu používáme nejdřív [BatchManagementClient.Account.ListAsync] [ net_mgmt_listaccounts] získat skupinu všechny účty dávku, které jsou v rámci předplatného. Po získání jsme tuto kolekci určení ke kolika účtům se v oblasti cílové. Potom používáme [BatchManagementClient.Subscriptions] [ net_mgmt_subscriptions] získat kvóty dávku účtu a zjistit, kolik účty (pokud existuje) lze vytvořit v dané oblasti.

```csharp
// Get a collection of all Batch accounts within the subscription
BatchAccountListResponse listResponse =
        await batchManagementClient.Account.ListAsync(new AccountListParameters());
IList<AccountResource> accounts = listResponse.Accounts;
Console.WriteLine("Total number of Batch accounts under subscription id {0}:  {1}",
    creds.SubscriptionId,
    accounts.Count);

// Get a count of all accounts within the target region
string region = "westus";
int accountsInRegion = accounts.Count(o => o.Location == region);

// Get the account quota for the specified region
SubscriptionQuotasGetResponse quotaResponse = await batchManagementClient.Subscriptions.GetSubscriptionQuotasAsync(region);
Console.WriteLine("Account quota for {0} region: {1}", region, quotaResponse.AccountQuota);

// Determine how many accounts can be created in the target region
Console.WriteLine("Accounts in {0}: {1}", region, accountsInRegion);
Console.WriteLine("You can create {0} accounts in the {1} region.", quotaResponse.AccountQuota - accountsInRegion, region);
```

Ve výše uvedená fragment `creds` instancí [TokenCloudCredentials][azure_tokencreds]. Příklad vytvoření tento objekt najdete v tématu [AccountManagement] [ acct_mgmt_sample] ukázka kódu na GitHub.

### <a name="check-a-batch-account-for-compute-resource-quotas"></a>Kontrola účet dávka pro kvóty pro využití prostředků

Před zvýšením výpočetním prostředky v řešení dávku, můžete zkontrolovat, že zajistit prostředky, které chcete přiřadit nesmí překročit kvóty účtu. V následujícím fragment kódu jsme vytisknout informací o kvót pro dávku účet s názvem `mybatchaccount`. Vlastní aplikaci můžete tyto informace zjistit, zda účtu můžete dělat další zdroje informací vytvořit.

```csharp
// First obtain the Batch account
BatchAccountGetResponse getResponse =
    await batchManagementClient.Account.GetAsync("MyResourceGroup", "mybatchaccount");
AccountResource account = getResponse.Resource;

// Now print the compute resource quotas for the account
Console.WriteLine("Core quota: {0}", account.Properties.CoreQuota);
Console.WriteLine("Pool quota: {0}", account.Properties.PoolQuota);
Console.WriteLine("Active job and job schedule quota: {0}", account.Properties.ActiveJobAndJobScheduleQuota);
```

> [AZURE.IMPORTANT] I když jsou výchozí kvóty pro Azure předplatná a služby, spoustu tyto limity lze zvýšit zadáním požadavku na [Azure portál][azure_portal]. Například najdete v článku [kvót a limity pro službu Azure dávku](batch-quota-limit.md) pokyny týkající se zvýšením kvóty účtu dávku.

## <a name="batch-management-net-azure-ad-and-resource-manager"></a>Dávkové správy .NET, Azure AD a správce zdrojů

Při práci s knihovnou správy .NET dávku obvykle používáte rovněž [Azure Active Directory] [ aad_about] (Azure AD) a [Správce prostředků Azure][resman_overview]. Ukázkový projekt popsána níže používá Azure Active Directory a správce prostředků, zatímco ukazuje rozhraní API dávku správy .NET.

### <a name="azure-active-directory"></a>Azure Active Directory

Azure sama používá Azure AD pro ověřování svých zákazníků, správci služeb a organizační uživatelů. V rámci správy .NET dávku použijete Azure AD ověření správci předplatné a dalších správci. Díky knihovnou správy dotazu službu dávku a provedení operace popisované v tomto článku.

V aplikaci project ukázkové popsána níže Azure [Active Directory Authentication Library] [ aad_adal] (ADAL) slouží k požádání uživatele o své přihlašovací údaje společnosti Microsoft. Když služby správce nebo coadministrator jsou pověření, aplikace můžete dotazu Azure seznam předplatných – a můžete vytvářet a odstraňovat skupiny zdrojů a dávku účty.

### <a name="resource-manager"></a>Správce prostředků

Při vytváření účty dávka s knihovnou správy .NET dávku obvykle vytvoříte je v rámci [pole Skupina zdroje][resman_overview]. Skupina zdroje můžete vytvořit programově pomocí [ResourceManagementClient] [ resman_client] třídy v [Správce prostředků .NET] [ resman_api] knihovny. Nebo můžete přidat účet do existující skupiny zdroje, který jste vytvořili dříve pomocí [Azure portál][azure_portal].

## <a name="sample-project-on-github"></a>Ukázkový projekt na GitHub

Zobrazit dávku správy .NET v akci, najdete v tématech [AccountManagment] [ acct_mgmt_sample] ukázkový projekt na GitHub. Tato aplikace konzoly zobrazuje vytvoření a použití te000126961 [BatchManagementClient] [ net_mgmt_client] a [ResourceManagementClient][resman_client]. Také demonstruje použití Azure [Active Directory Authentication Library] [ aad_adal] (ADAL), které je potřeba obou klienty.

Pokud chcete spustit aplikaci vzorku, nejdřív Zaregistrujte ho s Azure AD pomocí portálu Azure. Postupujte podle kroků uvedených v části [Přidání aplikace](../active-directory/active-directory-integrating-applications.md#adding-an-application) v [aplikacích integrace se službou Azure Active Directory] [ aad_integrate] zaregistrovat ukázková aplikace v rámci vašeho účtu uživatele výchozí adresář. Je potřeba vybrat **Nativní klientské aplikace** pro typ aplikace a můžete použít libovolný platný identifikátor URI (například `http://myaccountmanagementsample`) pro **Přesměrování URI**– nemusí být skutečné koncového bodu.

Po přidání aplikace se delegovat **Správu služby Azure přístupu jako organizace** oprávnění k *Rozhraní API systému Windows Azure služby Správa* aplikací v nastavení aplikace na portálu:

![Oprávnění aplikace Azure portálu][2]

> [AZURE.TIP] **Rozhraní API správy služby Windows Azure** není zobrazena ve skupinovém rámečku *oprávnění pro ostatní aplikace*, klikněte na **Přidat aplikaci**, vyberte **Rozhraní API správy služby Windows Azure**klikněte na tlačítko se zatržítkem. Potom delegujte oprávnění výše uvedené.

Po přidání aplikace jak jsme je popsali výše, aktualizujte `Program.cs` v [AccountManagment] [ acct_mgmt_sample] vzorku projektu se vaše aplikace přesměrovat URI a ID klienta. Na kartě **Konfigurovat** aplikace můžete najít tyto hodnoty:

![Konfigurace aplikace Azure portálu][3]

[AccountManagment] [ acct_mgmt_sample] ukázková aplikace ukazuje následující operace:

1. Získání tokenu zabezpečení z Azure AD pomocí [ADAL][aad_adal]. Pokud již není přihlášení uživatele, se zobrazí výzva k zadání přihlašovacích údajů jejich Azure.
2. Pomocí tokenu zabezpečení získané Azure AD vytvořit [SubscriptionClient] [ resman_subclient] dotazu Azure seznam předplatných přidružené k účtu. Díky výběru předplatného-li více najdou.
3. Vytvoření přidružený k předplatnému vybraného objektu přihlašovací údaje.
4. Vytvoření [ResourceManagementClient] [ resman_client] pomocí pověření.
5. Použití [ResourceManagementClient] [ resman_client] vytvořit skupinu zdroje.
6. Použití [BatchManagementClient] [ net_mgmt_client] provádět několik dávku účtu operace:
  - Vytvoření účtu dávku do nové skupiny prostředků.
  - Pokud potřebujete nově vytvořený účet služby dávku.
  - Tisk klávesy účtu pro nový účet.
  - Obnovit nové primární klíč účtu.
  - Tisk kvóta informace o účtu.
  - Tisk informací o kvót pro předplatné.
  - Vytiskněte všechny účty v rámci předplatného.
  - Nově vytvořený účet odstraňte.
7. Odstranění skupiny zdrojů.

Před odstraněním nově vytvořený dávku účet a zdroje skupině, můžete zkontrolovat obě [Azure portál][azure_portal]:

![Azure portál zobrazující pole Skupina zdroje a dávku účtu][1]
<br />
*Azure portálu zobrazení nové skupiny prostředků a dávku účtu*

[aad_about]: ../active-directory/active-directory-whatis.md "Co je Azure Active Directory?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Ověřování scénáře Azure AD"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Integrace aplikace s Azure Active Directory"
[acct_mgmt_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/AccountManagement
[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_mgmt_net]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[azure_portal]: http://portal.azure.com
[azure_storage]: https://azure.microsoft.com/services/storage/
[azure_tokencreds]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.tokencloudcredentials.aspx
[batch_explorer_project]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_list_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.listkeysasync.aspx
[net_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.createasync.aspx
[net_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.deleteasync.aspx
[net_regenerate_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.regeneratekeyasync.aspx
[net_sharedkeycred]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.auth.batchsharedkeycredentials.aspx
[net_mgmt_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.aspx
[net_mgmt_subscriptions]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.subscriptions.aspx
[net_mgmt_listaccounts]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.iaccountoperations.listasync.aspx
[resman_api]: https://msdn.microsoft.com/library/azure/mt418626.aspx
[resman_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.resourcemanagementclient.aspxs
[resman_subclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.subscriptions.subscriptionclient.aspx
[resman_overview]: ../azure-resource-manager/resource-group-overview.md

[1]: ./media/batch-management-dotnet/portal-01.png
[2]: ./media/batch-management-dotnet/portal-02.png
[3]: ./media/batch-management-dotnet/portal-03.png
