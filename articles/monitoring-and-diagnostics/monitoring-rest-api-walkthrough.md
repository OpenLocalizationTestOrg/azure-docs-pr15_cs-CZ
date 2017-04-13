<properties
    pageTitle="Azure sledování REST API návodu | Microsoft Azure"
    description="Jak ověřit požadavky na a používat Azure sledování rozhraní REST API."
    authors="mcollier, rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="mcollier"/>

# <a name="azure-monitoring-rest-api-walkthrough"></a>Sledování REST API návodu Azure
V tomto článku se dozvíte, jak provádět ověření tak, aby váš kód použít [Microsoft Azure Monitor REST API odkaz](https://msdn.microsoft.com/library/azure/dn931943.aspx).         

Rozhraní API monitoru Azure umožňuje programově získat dostupné výchozí metrických definice (typ metriky například časového využití procesoru, požadavky, atd.), granularity a metrických hodnoty. Jakmile načíst, můžete data uložena v samostatný datový úložiště například databáze SQL Azure, DocumentDB nebo jezera dat Azure. Odtud můžete provést další analýzu podle potřeby.

Kromě práce s různými metrických datových bodů, jak ukazuje tento článek rozhraní API monitoru umožňuje seznamu pravidel výstrah, zobrazení aktivity protokolů a řadu dalších objektů. Úplný seznam dostupných operací najdete v článku [Microsoft Azure Monitor REST API odkaz](https://msdn.microsoft.com/library/azure/dn931943.aspx).

## <a name="authenticating-azure-monitor-requests"></a>Ověřování Azure Monitor požadavky

Cílem prvního kroku je k ověření žádost.

Všechny úkoly spouštět oproti rozhraní API monitoru Azure pomocí ověřování modelu správce prostředků Azure. Proto musí být všechny žádosti o ověřeny službou Azure Active Directory (Azure AD). Jedním ze způsobů ověření klientské aplikace je vytvořit Azure AD jistinu služby a získat token ověřování (JWT). Následující ukázkový skript ukazuje vytváření služby Azure AD hlavní prostřednictvím Powershellu. Podrobnější walk-through najdete v dokumentaci k [použití Azure PowerShell vytvoření služby základní k přístupu k prostředkům](../resource-group-authenticate-service-principal.md#authenticate-service-principal-with-password—powershell). Také je možné [vytvořit pravidlo služby prostřednictvím portálu Azure](../resource-group-create-service-principal-portal.md).

```PowerShell
$subscriptionId = "{azure-subscription-id}"
$resourceGroupName = "{resource-group-name}"
$location = "centralus"

# Authenticate to a specific Azure subscription.
Login-AzureRmAccount -SubscriptionId $subscriptionId

# Password for the service principal
$pwd = "{service-principal-password}"

# Create a new Azure AD application
$azureAdApplication = New-AzureRmADApplication `
                        -DisplayName "My Azure Monitor" `
                        -HomePage "https://localhost/azure-monitor" `
                        -IdentifierUris "https://localhost/azure-monitor" `
                        -Password $pwd

# Create a new service principal associated with the designated application
New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

# Assign Reader role to the newly created service principal
New-AzureRmRoleAssignment -RoleDefinitionName Reader `
                          -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

```

K vytvoření dotazu rozhraní API monitoru Azure, používejte klientské aplikaci jistinu dříve vytvořené služby ověření. Následující příklad skript Powershellu zobrazuje jedním ze způsobů, které vám pomůžou ověřování JWT tokenu pomocí [Active Directory Authentication Library](../active-directory/active-directory-authentication-libraries.md) (ADAL). Azure Monitor REST API předána jako součást parametr HTTP se tak mohli ověřovat v žádosti o JWT token.

```PowerShell
$azureAdApplication = Get-AzureRmADApplication -IdentifierUri "https://localhost/azure-monitor"

$subscription = Get-AzureRmSubscription -SubscriptionId $subscriptionId

$clientId = $azureAdApplication.ApplicationId.Guid
$tenantId = $subscription.TenantId
$authUrl = "https://login.windows.net/${tenantId}"

$AuthContext = [Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext]$authUrl
$cred = New-Object -TypeName Microsoft.IdentityModel.Clients.ActiveDirectory.ClientCredential -ArgumentList ($clientId, $pwd)
 
$result = $AuthContext.AcquireToken("https://management.core.windows.net/", $cred)

# Build an array of HTTP header values 
$authHeader = @{
'Content-Type'='application/json'
'Accept'='application/json'
'Authorization'=$result.CreateAuthorizationHeader()
}
```

Po dokončení kroku nastavení ověřování dotazů můžete pak spouštět oproti Azure Monitor REST API. Existují dva užitečné dotazy:

1. Seznam metrických definice zdroje

2. Načtení metrických hodnot


## <a name="retrieve-metric-definitions"></a>Načtení metrických definice
>[AZURE.NOTE] Načíst metrické definice pomocí rozhraní Azure Monitor REST API, "2016 03 01" jako použijte verze rozhraní API.

```PowerShell
$apiVersion = "2016-03-01"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metricDefinitions?api-version=${apiVersion}"

Invoke-RestMethod -Uri $request `
                  -Headers $authHeader `
                  -Method Get `
                  -Verbose
```
Použití logických operátorů aplikace Azure metrických definice by vypadat následující snímek obrazovky:

![ALT "JSON zobrazení metrických definice odpovědi".](./media/monitoring-rest-api-walkthrough/available_metric_definitions_logic_app_json_response_clean.png)

Další informace najdete v dokumentaci [seznam metrické definice zdroje v Azure Monitor REST API](https://msdn.microsoft.com/library/azure/mt743621.aspx) .

## <a name="retrieve-metric-values"></a>Načtení metrické hodnot
Jakmile je známo, k dispozici metrických definice, bude získat souvisejících metrických hodnot. Použijte název míru "hodnotu" (ne "localizedValue") pro filtrování požadavky (například načíst metrických datových bodů "CpuTime" a "Žádosti"). Pokud byla vyplněná žádné filtry, bude vrácena míru výchozí.

>[AZURE.NOTE] Načtení metrických hodnot pomocí rozhraní Azure monitoru REST API, můžete "2016 06 01" jako verze rozhraní API.

**Metoda**: GET

**Žádost o URI**: https://management.azure.com/subscriptions/_{id předplatného}_/resourceGroups/_{název zdroje skupiny}_/providers/_{zdroje názvů poskytovatele}_/_{Typ zdroje}_/verze rozhraní api & /providers/microsoft.insights/metrics?$filter=_{název zdroje}__{Filtr}_=_{apiVersion}_

Například k načtení RunsSucceeded metrických datových bodů pro dané období a zrn čas nebo 1 hodinu, žádost by vypadal takto:

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'RunsSucceeded') and aggregationType eq 'Total' and startTime eq 2016-09-23 and endTime eq 2016-09-24 and timeGrain eq duration'PT1H'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

Výsledek vypadat podobně jako příklad následující snímek obrazovky:

![ALT "JSON odpověď zobrazující průměrnou doba odezvy metrické hodnotu"](./media/monitoring-rest-api-walkthrough/available_metrics_logic_app_json_response.png)

K načtení více bodů dat nebo agregace, přidejte metrických definice názvy a agregace typy filtr, jak je vidět v následujícím příkladu:

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'ActionsCompleted' or name.value eq 'RunsSucceeded') and (aggregationType eq 'Total' or aggregationType eq 'Average') and startTime eq 2016-09-23T13:30:00Z and endTime eq 2016-09-23T14:30:00Z and timeGrain eq duration'PT1M'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

### <a name="use-armclient"></a>Použití ARMClient
Alternativy pomocí Powershellu (viz výše), je použít [ARMClient](https://github.com/projectkudu/ARMClient) na počítači se systémem Windows. ARMClient pracuje s Azure AD ověřování (a výsledná token JWT) automaticky. Následující kroky popisují využívání ARMClient načítání metrických dat:

1. Nainstalujte [Chocolatey](https://chocolatey.org/) a [ARMClient](https://github.com/projectkudu/ARMClient).

2. V okně terminálu zadejte _armclient.exe přihlášení_. Výzva k přihlášení k Azure.

3. Typ _armclient GET [your_resource_id]/providers/microsoft.insights/metricdefinitions?api-version=2016-03-01_

4. Typ _armclient GET [your_resource_id]/providers/microsoft.insights/metrics?api-version=2016-06-01_


![ALT "Použití ARMClient pro práci s Azure sledování rozhraní REST API"](./media/monitoring-rest-api-walkthrough/armclient_metricdefinitions.png)


## <a name="retrieve-the-resource-id"></a>Načtení číslo ID zdroje
Pomocí rozhraní REST API skutečně pomůže pochopit dostupné metrické definice, granularity a souvisejících hodnot. Tyto informace jsou užitečné při použití [Knihovnou správy Azure](https://msdn.microsoft.com/library/azure/mt417623.aspx).

Předchozí kód číslo ID zdroje můžete po úplnou cestu k požadované Azure zdroje. Například k vytvoření dotazu s webovou aplikaci Azure, bude číslo ID zdroje:

*/subscriptions/{Subscription-ID}/resourceGroups/{Resource-Group-Name}/providers/Microsoft.Web/Sites/{Site-Name}/*

Následující seznam obsahuje několik příkladů formátů ID zdroje pro různé Azure zdroje:

* **Rozbočovač IoT** - /subscriptions/_{id předplatného}_/resourceGroups/_{název zdroje skupiny}_/providers/Microsoft.Devices/IotHubs/_{iot centrální název}_

* **Pružná fondu SQL** - /subscriptions/_{id předplatného}_/resourceGroups/_{název zdroje skupiny}_/providers/Microsoft.Sql/servers/ {fondu-} /elasticpools/_databáze__{název sql fondu}_

* **Databáze SQL (v12)** - /subscriptions/_{id předplatného}_/resourceGroups/_{název zdroje skupiny}_/providers/Microsoft.Sql/servers/_{název serveru}_/databases/_{název databáze}_

* **Služba Bus** - /subscriptions/_{id předplatného}_/resourceGroups/_{název zdroje skupiny}_/providers/Microsoft.ServiceBus/_{názvů}_/_{servicebus název}_

* **Sady měřítko OM** - /subscriptions/_{id předplatného}_/resourceGroups/_{název zdroje skupiny}_/providers/Microsoft.Compute/virtualMachineScaleSets/_{OM název}_

* **VMs** - /subscriptions/_{id předplatného}_/resourceGroups/_{název zdroje skupiny}_/providers/Microsoft.Compute/virtualMachines/_{OM název}_

* **Událost rozbočovače** - /subscriptions/_{id předplatného}_/resourceGroups/_{název zdroje skupiny}_/providers/Microsoft.EventHub/namespaces/_{eventhub obor názvů}_


Existuje přístupy k načtení pole číslo ID zdroje, včetně použití Azure zdroje Průzkumníku zobrazení požadovaným prostředkem Azure portálu a pomocí prostředí PowerShell nebo Azure rozhraní příkazového řádku.

### <a name="azure-resource-explorer"></a>Průzkumník Azure zdroje
Číslo ID zdroje pro požadovaný zdroj najdete jedním ze způsobů užitečné je používat nástroj [Explorer Azure zdroje](https://resources.azure.com) . Přejděte do požadované zdroje a podívejte se na ID zobrazené jako následující snímek obrazovky:

![ALT "Azure zdroje Průzkumníka"](./media/monitoring-rest-api-walkthrough/azure_resource_explorer.png)

### <a name="azure-portal"></a>Azure portálu
Číslo ID zdroje lze získat také z portálu Microsoft Azure. Postup, přejděte do požadované zdroje a potom vyberte vlastnosti. Číslo ID zdroje se zobrazí v zásuvné vlastnosti popsané v následující snímek obrazovky:

![ALT "pole číslo ID zdroje v zásuvné vlastnosti na portálu Azure"](./media/monitoring-rest-api-walkthrough/resourceid_azure_portal.png)

### <a name="azure-powershell"></a>Azure Powershellu
Číslo ID zdroje můžete načíst pomocí rutin prostředí PowerShell Azure stejně. Například získat číslo ID zdroje pro webovou aplikaci Azure, spouštět rutiny Get-AzureRmWebApp jako následující snímek obrazovky:

![ALT "pole číslo ID zdroje získaných prostřednictvím Powershellu"](./media\monitoring-rest-api-walkthrough\resourceid_powershell.png)

### <a name="azure-cli"></a>Azure rozhraní příkazového řádku
K načtení pole číslo ID zdroje pomocí rozhraní příkazového řádku Azure, provést příkaz azure web appu zobrazit určující "– json" možnost, jak je vidět na následující snímek obrazovky:

![ALT "pole číslo ID zdroje získaných prostřednictvím Powershellu"](./media\monitoring-rest-api-walkthrough\resourceid_azurecli.png)

## <a name="retrieve-activity-log-data"></a>Načtení dat protokolu aktivita
Kromě práci s metrické definice a souvisejících hodnot je také možné získat další zajímavé přehledy související s Azure zdroje. Jako příklad je možné zadat dotaz na data [protokolu činnosti](https://msdn.microsoft.com/library/azure/dn931934.aspx) . Následující příklad ukazuje, jak pomocí rozhraní REST API dat protokolu aktivit dotazu v určitém rozsahu Azure Monitor Azure předplatného:

```PowerShell
$apiVersion = "2014-04-01"
$filter = "eventTimestamp ge '2016-09-23' and eventTimestamp le '2016-09-24'and eventChannels eq 'Admin, Operation'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/providers/microsoft.insights/eventtypes/management/values?api-version=${apiVersion}&`$filter=${filter}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

## <a name="next-steps"></a>Další kroky
* Projděte si [základní informace o sledování](monitoring-overview.md).
* Zobrazení [podporované metriky s Azure Monitor](monitoring-supported-metrics.md).
* Prohlédněte si [Microsoft Azure Monitor REST API odkaz](https://msdn.microsoft.com/library/azure/dn931943.aspx).
* Prohlédněte si [Knihovna Azure správy](https://msdn.microsoft.com/library/azure/mt417623.aspx).
