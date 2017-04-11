<properties 
   pageTitle="Správa Azure dat jezera technologie pro analýzu použití Azure .NET SDK | Azure" 
   description="Naučte se spravovat jezera analýzy dat projektů, zdrojů dat, uživatelé. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/23/2016"
   ms.author="jgao"/>

# <a name="manage-azure-data-lake-analytics-using-azure-net-sdk"></a>Správa Azure dat jezera technologie pro analýzu použití Azure .NET SDK

[AZURE.INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Naučte se spravovat účty jezera analýzy dat Azure, zdrojů dat, uživatelů a úlohy pomocí Azure .NET SDK. Správa témat pomocí dalších nástrojů zobrazíte kliknutím na nahoře vyberte kartu.

**Zjistit předpoklady pro**

Před zahájením tohoto kurzu, musíte mít takto:

- **Azure předplatného**. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).


<!-- ################################ -->
<!-- ################################ -->


## <a name="connect-to-azure-data-lake-analytics"></a>Připojení k analýzy jezera Azure dat

Budete potřebovat následující balíčky Nuget:

    Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
    Install-Package Microsoft.Azure.Common 
    Install-Package Microsoft.Azure.Management.ResourceManager -Pre
    Install-Package Microsoft.Azure.Management.DataLake.Analytics -Pre


Následující příklad kódu ukazuje, jak se připojit k Azure a seznam stávajících účtů jezera analýzy dat v rámci předplatného Azure.

    using System;
    using System.Collections.Generic;
    using System.Threading;

    using Microsoft.Rest;
    using Microsoft.Rest.Azure.Authentication;

    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.DataLake.Store;
    using Microsoft.Azure.Management.DataLake.Analytics;
    using Microsoft.Azure.Management.DataLake.Analytics.Models;

    namespace ConsoleAcplication1
    {
        class Program
        {

            private const string SUBSCRIPTIONID = "<Enter Your Azure Subscription ID>";
            private const string CLIENTID = "1950a258-227b-4e31-a9cf-717495945fc2";
            private const string DOMAINNAME = "common"; // Replace this string with the user's Azure Active Directory tenant ID or domain name, if needed.

            private static DataLakeAnalyticsAccountManagementClient _adlaClient;

            private static void Main(string[] args)
            {

                var creds = AuthenticateAzure(DOMAINNAME, CLIENTID);

                _adlaClient = new DataLakeAnalyticsAccountManagementClient(creds);
                _adlaClient.SubscriptionId = SUBSCRIPTIONID;

                var adlaAccounts = ListADLAAccounts();

                Console.WriteLine("You have %i Data Lake Analytics account(s).", adlaAccounts.Count);
                for (int i = 0; i < adlaAccounts.Count; i ++)
                {
                    Console.WriteLine(adlaAccounts[i].Name);
                }

                System.Console.WriteLine("Press ENTER to continue");
                System.Console.ReadLine();
            }

            public static ServiceClientCredentials AuthenticateAzure(
            string domainName,
            string nativeClientAppCLIENTID)
            {
                // User login via interactive popup
                SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
                // Use the client ID of an existing AAD "Native Client" application.
                var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(nativeClientAppCLIENTID, new Uri("urn:ietf:wg:oauth:2.0:oob"));
                return UserTokenProvider.LoginWithPromptAsync(domainName, activeDirectoryClientSettings).Result;
            }

            public static List<DataLakeAnalyticsAccount> ListADLAAccounts()
            {
                var response = _adlaClient.Account.List();
                var accounts = new List<DataLakeAnalyticsAccount>(response);

                while (response.NextPageLink != null)
                {
                    response = _adlaClient.Account.ListNext(response.NextPageLink);
                    accounts.AddRange(response);
                }

                return accounts;
            }
        }
    }


## <a name="manage-accounts"></a>Správa účtů

Před spuštěním všechny úlohy jezera analýzy dat, musíte mít účet analýzy dat jezera. Na rozdíl od Azure HDInsight není zaplatit účet analýzy když neběží projektu.  Pouze zaplatit čas, kdy běží projektu.  Další informace najdete v tématu [Přehled technologie pro analýzu dat jezera Azure](data-lake-analytics-overview.md).  

###<a name="create-accounts"></a>Vytvoření účtů

Řízení zdrojů Azure skupiny a účet úložiště jezera dat musí mít před spuštěním v následujícím příkladu.

Následující kód ukazuje, jak vytvořit skupinu zdrojů:

    public static async Task<ResourceGroup> CreateResourceGroupAsync(
        ServiceClientCredentials credential,
        string groupName,
        string subscriptionId,
        string location)
    {

        Console.WriteLine("Creating the resource group...");
        var resourceManagementClient = new ResourceManagementClient(credential)
        { SubscriptionId = subscriptionId };
        var resourceGroup = new ResourceGroup { Location = location };
        return await resourceManagementClient.ResourceGroups.CreateOrUpdateAsync(groupName, resourceGroup);
    }

Následující kód ukazuje, jak vytvořit účet úložiště jezera dat:

    var adlsParameters = new DataLakeStoreAccount(location: _location);
    _adlsClient.Account.Create(_resourceGroupName, _adlsAccountName, adlsParameters);

Následující kód ukazuje, jak vytvořit účet jezera analýzy dat:

    var defaultAdlsAccount = new List<DataLakeStoreAccountInfo> { new DataLakeStoreAccountInfo(adlsAccountName, new DataLakeStoreAccountInfoProperties()) };
    var adlaProperties = new DataLakeAnalyticsAccountProperties(defaultDataLakeStoreAccount: adlsAccountName, dataLakeStoreAccounts: defaultAdlsAccount);
    var adlaParameters = new DataLakeAnalyticsAccount(properties: adlaProperties, location: location);
    var adlaAccount = _adlaClient.Account.Create(resourceGroupName, adlaAccountName, adlaParameters);

###<a name="list-accounts"></a>Seznam účtů

Najdete v článku [připojení k Azure dat jezera analýzy](#connect_to_azure_data_lake_analytics).

###<a name="find-an-account"></a>Vyhledání účet

Po nastavení objektu seznam účtů jezera analýzy dat, můžete najít účet následující:

    Predicate<DataLakeAnalyticsAccount> accountFinder = (DataLakeAnalyticsAccount a) => { return a.Name == adlaAccountName; };
    var myAdlaAccount = adlaAccounts.Find(accountFinder);

###<a name="delete-data-lake-analytics-accounts"></a>Odstranění účtů jezera analýzy dat

Následující fragment kódu odstraní účet jezera analýzy dat:

    _adlaClient.Account.Delete(resourceGroupName, adlaAccountName);

<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-account-data-sources"></a>Správa zdrojů dat účtu

V následujících zdrojích dat v současné době podporuje jezera analýzy dat:

- [Ukládání jezera Azure dat](../data-lake-store/data-lake-store-overview.md)
- [Azure úložiště](../storage/storage-introduction.md)

Při vytváření účtu analýzy, je třeba určit účet Azure úložný prostor jezera výchozí účet úložiště. Výchozí úložiště jezera dat účet se používá k ukládání protokolů auditování metadata a úlohy projektu. Po vytvoření účtu analýzy, můžete přidat další úložný prostor jezera účty a/nebo účet Azure úložiště. 

### <a name="find-the-default-data-lake-store-account"></a>Vyhledání výchozí účet jezera úložiště dat

Přečtěte si část vyhledání účet v tomto článku k nalezení dat jezera analýzy účtu. Potom následujícím způsobem:

    string adlaDefaultDataLakeStoreAccountName = myAccount.Properties.DefaultDataLakeStoreAccount;


## <a name="use-azure-resource-manager-groups"></a>Používání skupin správce prostředků Azure

Aplikace se obvykle vytvářejí součástí mnoho, třeba do webových aplikací databáze, databázový server, úložiště a 3 stran služby. Azure správce prostředků umožňuje práce se zdroji v aplikaci jako skupinu, označovaný taky jako skupina zdroje Azure. Můžete nasadit, aktualizace, sledovat nebo odstranit všechny zdroje pro aplikaci v operaci jediné, koordinovaný. Použití šablony pro nasazení a této šablony můžete pracovat na jiném prostředí například testování pracovní a výroby. Fakturace můžete vysvětlit pro vaši organizaci, zobrazením nákladů úrovní pro celou skupinu. Další informace najdete v tématu [Přehled Správce prostředků Azure](../azure-resource-manager/resource-group-overview.md). 

Služba jezera analýzy dat může obsahovat tyto prvky:

- Účet technologie pro analýzu dat jezera Azure
- Požadované výchozí účet pro ukládání jezera dat Azure
- Účty jezera dat Azure další úložiště
- Další úložiště Azure účty

Můžete vytvořit tyto komponenty v jedné skupině řízení zdrojů a jejich snadněji spravovat.

![Účet Azure dat jezera technologie pro analýzu a úložiště](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-arm-structure.png)

Účet jezera analýzy dat a účty závislá úložiště musí být umístěny ve stejné Azure datovém centru.
Řízení zdrojů skupina však mohou být umístěny v různých datovém centru.  

##<a name="see-also"></a>Viz taky 

- [Přehled analýzy dat jezera Microsoft Azure](data-lake-analytics-overview.md)
- [Začínáme s jezera analýzy dat Azure portálu](data-lake-analytics-get-started-portal.md)
- [Správa Azure dat jezera technologie pro analýzu Azure portálu](data-lake-analytics-manage-use-portal.md)
- [Sledování a odstraňování případných problémů jezera analýzy dat Azure úlohy Azure portálu](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

