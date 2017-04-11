<properties
    pageTitle="Analýza Azure protokoly pro diagnostiku pomocí protokolu analýzy | Microsoft Azure"
    description="Protokol analýzy může číst protokoly z Azure služeb, které psaní Azure diagnostické protokoly, aby objektů blob úložiště ve formátu JSON."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>


# <a name="analyze-azure-diagnostic-logs-using-log-analytics"></a>Analýza Azure protokoly pro diagnostiku pomocí protokolu analýzy

Protokol analýzy můžete shromáždit protokoly pro následující Azure služby, které k úložišti objektů blob ve formátu JSON zápisu [Azure diagnostických protokolů](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) :

+ Automatizace (verze Preview)
+ Klíčové trezoru (verze Preview)
+ Aplikace brány (verze Preview)
+ Skupina zabezpečení síti (verze Preview)

V následujících částech vás provede jednotlivými pomocí Powershellu:

+ Konfigurace protokolu analýzy získat informace o protokolech z úložiště pro jednotlivé zdroje  
+ Povolení protokolu analýzy řešení pro službu Azure

Před protokolu analýzy můžete shromáždit data pro tyto materiály, musí být povolený Azure diagnostiky. Použití `Set-AzureRmDiagnosticSetting` rutina povolit protokolování.

Naleznete v následujících článcích Další informace o tom, jak povolit diagnostické protokolování:

+ [Klíčové trezoru](../key-vault/key-vault-logging.md)
+ [Aplikace brány](../application-gateway/application-gateway-diagnostics.md)
+ [Skupina zabezpečení sítě](../virtual-network/virtual-network-nsg-manage-log.md)

Tento si přečtěte následující dokumentaci obsahuje taky informace na:

+ Poradce při potížích s shromažďování dat
+ Zastavení shromažďování dat

## <a name="configure-log-analytics-to-collect-azure-diagnostic-logs"></a>Konfigurace protokolu analýzy shromažďovat Azure protokoly pro diagnostiku

Shromáždění protokolů u těchto služeb a povolení řešení vizualizovat protokoly se provádí pomocí skriptů Powershellu.

V příkladu níže umožňuje přihlášení všechny podporované zdroje

```
# update to be the storage account that logs will be written to. Storage account must be in the same region as the resource to monitor
# format is similar to "/subscriptions/ec11ca60-ab12-345e-678d-0ea07bbae25c/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount"
$storageAccountId = ""

$supportedResourceTypes = ("Microsoft.Automation/AutomationAccounts", "Microsoft.KeyVault/Vaults", "Microsoft.Network/NetworkSecurityGroups", "Microsoft.Network/ApplicationGateways")

# update location to match your storage account location
$resources = Get-AzureRmResource | where { $_.ResourceType -in $supportedResourceTypes -and $_.Location -eq "westus" }

foreach ($resource in $resources) {
    Set-AzureRmDiagnosticSetting -ResourceId $resource.ResourceId -StorageAccountId $storageAccountId -Enabled $true -RetentionEnabled $true -RetentionInDays 1
}
```


Pro některé zdroje je možné provést předcházející konfigurace z portálu Azure pomocí postupu v [protokoly pro diagnostiku přehled Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-diagnostic-logs).

## <a name="configure-log-analytics-to-collect-azure-diagnostic-logs"></a>Konfigurace protokolu analýzy shromažďovat Azure protokoly pro diagnostiku

Uvádíme moduly skript Powershellu, které exportuje dvou rutiny kvůli usnadnění protokolu technologie pro analýzu konfigurace:

1. `Add-AzureDiagnosticsToLogAnalyticsUI`Zobrazí výzvu k zadání vstupních hodnot a je možné nastavit jednoduché konfigurace
2. `Add-AzureDiagnosticsToLogAnalytics`Přenese materiály ke sledování předávat na vstupu a pak nastaví protokolu analýzy  

### <a name="pre-requisites"></a>Předpoklady

1. Azure Powershellu s verzí 1.0.8 nebo novější rutin provozní přehledy.
  - [Instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md)
  - Ověřte svoji verzi rutiny:`Import-Module AzureRM.OperationalInsights -MinimumVersion 1.0.8 `
2. Protokolování diagnostiky nakonfigurovaný pro Azure zdroj, který chcete sledovat. Použití `Set-AzureRmDiagnosticSetting` nebo v nápovědě k [Použití protokolu analýzy sběr dat z Azure úložiště účtů](log-analytics-azure-storage.md) pro povolení diagnostických nástrojů.
3. Pracovní prostor [Protokolu analýzy](https://portal.azure.com/#create/Microsoft.LogAnalyticsOMS)  
4. Modul AzureDiagnosticsAndLogAnalytics Powershellu
  - Stahování modulu [AzureDiagnosticsAndLogAnalytics](https://www.powershellgallery.com/packages/AzureDiagnosticsAndLogAnalytics/) z Galerie prostředí PowerShell

### <a name="option-1-run-the-interactive-configuration-scripts"></a>Možnost 1: Spouštěly skripty interaktivní konfigurace

Otevřete PowerShell a spuštění:

```
# Connect to Azure
Login-AzureRmAccount
# If you have diagnostics logs being written to classic storage you will also need to run
Add-AzureAccount

# Import the module
Install-Module -Name AzureDiagnosticsAndLogAnalytics

# Run the UI configuration script
Add-AzureDiagnosticsToLogAnalyticsUI

```

Jsou zobrazeny seznamu dostupné a zadaných výzva k jednotlivé možnosti.
Budete požádáni o vyberte požadované nastavení pro každou z těchto věcí:

+ Typy zdrojů (služby Azure) získat protokoly z
+ Instance zdroje, které chcete shromáždit protokoly z
+ Pracovní prostor protokolu analýzy shromažďování dat

Po spuštění tento skript, měli byste vidět záznamů v protokolu analýzy asi 30 minut po nová diagnostiky data k základnímu úložišti. Pokud záznamy není k dispozici po tuto dobu v nápovědě k řešení potíží v části.

### <a name="option-2-build-a-list-of-resources-and-pass-them-to-the-configuration-cmdlet"></a>Možnost 2: Vytvoření seznamu zdrojů a předávat je rutině konfigurace

Můžete vytvořit seznam zdrojů, které máte Azure diagnostiky povolené a potom předejte zdroje rutině konfigurace.

Zobrazí se další informace o rutině spuštěním `Get-Help Add-AzureDiagnosticsToLogAnalytics`.


Jak najít další informace na OMS [Rutiny prostředí PowerShell analýzy protokolu](https://msdn.microsoft.com/library/mt188224.aspx)

>[AZURE.NOTE] Pokud jsou v různých předplatných Azure zdroje a pracovní prostor, mezi nimi přepněte v případě potřeby pomocí`Select-AzureRmSubscription -SubscriptionId <Subscription the resource is in>`

```
# Connect to Azure
Login-AzureRmAccount
# If you have diagnostics logs being written to classic storage you will also need to run
Add-AzureAccount

# Import the module
Install-Module -Name AzureDiagnosticsAndLogAnalytics

# Update the values below with the name of the resource that has Azure diagnostics enabled and the name of your Log Analytics workspace
$resourceName = "***example-resource***"
$workspaceName = "***log-analytics-workspace***"

# Find the resource to monitor:
$resource = Find-AzureRmResource -ResourceType "Microsoft.KeyVault/Vaults" -ResourceNameContains $resourceName

# Find the Log Analytics workspace to configure, for example:
$workspace = Find-AzureRmResource -ResourceType "Microsoft.OperationalInsights/workspaces" -ResourceNameContains $workspaceName

# Perform configuration
Add-AzureDiagnosticsToLogAnalytics $resource $workspace

# Enable the Log Analytics solution
Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -intelligencepackname KeyVault -Enabled $true

```
Po spuštění tento skript, měli byste vidět záznamů v protokolu analýzy asi 30 minut po nová diagnostiky data k základnímu úložišti. Pokud záznamy není k dispozici po tuto dobu v nápovědě k řešení potíží v části.  

>[AZURE.NOTE] Konfigurace není vidět v portálu Azure. Můžete ověřit pomocí konfigurace `Get-AzureRmOperationalInsightsStorageInsight` rutiny.  


## <a name="stopping-log-analytics-from-collecting-azure-diagnostic-logs"></a>Zastavení protokolu analýzy z shromažďujete Azure protokoly pro diagnostiku

Můžete odstranit protokolu technologie pro analýzu konfigurace zdroje `Remove-AzureRmOperationalInsightsStorageInsight` rutiny.

## <a name="troubleshooting-configuration-for-azure-diagnostic-logs"></a>Řešení potíží s konfigurací Azure protokoly pro diagnostiku

*Problém*

Tato chyba se zobrazí při konfiguraci zdroje, které se přihlašujete k základnímu úložišti klasické:

```
Select-AzureSubscription : The subscription id 7691b0d1-e786-4757-857c-7360e61896c3 doesn't exist.

Parameter name: id
```

*Rozlišení*

Přihlaste se k rozhraní API pro klasické nasazení modelu pomocí`Add-AzureAccount`

## <a name="troubleshooting-data-collection-for-azure-diagnostic-logs"></a>Poradce při potížích s shromažďování dat pro Azure protokoly pro diagnostiku

Pokud nevidíte dat Azure zdroj v protokolu analýzy, můžete použít následující kroky:

+ Ověření účtu úložiště toku dat
+ Ověřte, zda že je povolen protokolu analýzy řešení pro službu
+ Ověřte, zda protokolu analýzy nakonfigurován pro čtení z úložiště
+ Aktualizace klíč účtu úložiště

### <a name="verify-data-is-flowing-to-the-storage-account"></a>Ověřte, jestli je k tomuto účtu úložiště toku dat

Pokud je k tomuto účtu úložiště s nástroj, který umožňuje prohlížení Azure úložiště (například Visual Studio) nebo pomocí prostředí PowerShell toku dat, můžete zkontrolovat.

K vyhledání účtu úložiště zdroje nakonfigurovaný tak, aby přihlásit pomocí následující Powershellu:

`Find-AzureRmResource -ResourceType "Microsoft.KeyVault/Vaults" | select ResourceId | Get-AzureRmDiagnosticSetting `

Kontejner úložiště používaný Azure diagnostiky má název ve formátu:  

`insights-logs-<Category>`

Platí pro [Používání rutin úložiště ke kontrole kontejneru poslední dat](../storage/storage-powershell-guide-full.md) přečíst si víc o prohlížení obsahu účtu úložiště.

### <a name="verify-the-log-analytics-solution-for-the-service-is-enabled"></a>Ověřte, zda že je povolen protokolu analýzy řešení pro službu

Pokud používáte `Add-AzureDiagnosticsToLogAnalyticsUI`, správný protokolu analýzy řešení je automaticky zapnutá za vás.

Zkontrolujte, zda je povolena řešení, spusťte následující Powershellu:

`Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $logAnalyticsResourceGroup -WorkspaceName $logAnalyticsWorkspaceName`

Pokud není povolený řešení, můžete povolit pomocí následující Powershellu:

`Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $logAnalyticsResourceGroup -WorkspaceName $logAnalyticsWorkspaceName -IntelligencePackName $solution -Enabled $true`

Při hledání název řešení povolit pro každý typ zdroje, použijte následující Powershellu (tuto proměnnou neexistuje naimportování modulu):

`$MonitorableResourcesToOMSSolutions`

### <a name="verify-that-log-analytics-is-configured-to-read-from-storage"></a>Ověřte, zda protokolu analýzy nakonfigurován pro čtení z úložiště

Pokud chcete přidat další zdroje informací Azure, potřebujete diagnostiky protokolování pro ně povolení a konfigurace protokolu Analytics pro ně.
Zkontrolovat, které zdroje a úložiště účty protokolu analýzy nakonfigurovaný tak, aby shromáždit protokoly pro použití následující Powershellu:

```
# Find the Workspace ResourceGroup and Name
$logAnalyticsWorkspace = Get-AzureRmOperationalInsightsWorkspace

#Get the configuration for all resources:
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $logAnalyticsWorkspace.ResourceGroupName -WorkspaceName $logAnalyticsWorkspace.Name

#Get the just the resources configured:
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $logAnalyticsWorkspace.ResourceGroupName -WorkspaceName $logAnalyticsWorkspace.Name | select Containers
```

### <a name="update-the-storage-key"></a>Aktualizace klávesu úložiště

Pokud změníte klíč účtu úložiště, protokolu technologie pro analýzu konfigurace také musí mají být aktualizovány pomocí nového klíče.
Aktualizace protokolu technologie pro analýzu konfigurace s novým klíčem pomocí následující Powershellu:

`Set-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $logAnalyticsWorkspace.ResourceGroupName -WorkspaceName $logAnalyticsWorkspace.Name –Name <Storage Insight Name> -StorageAccountKey $newKey `

Jak najdu název Přehled úložiště, můžete `Get-AzureRmOperationalInsightsStorageInsight` rutina, jak je vidět v předchozích příkladech.

## <a name="next-steps"></a>Další kroky

- [Úložiště objektů blob použití služby IIS a úložiště tabulek pro události](log-analytics-azure-storage-iis-table.md) čtení protokoly pro Azure služeb této zápisu diagnostiky úložiště tabulek nebo došlo k úložišti objektů blob zápisu protokolů IIS.
- [Povolení řešení](log-analytics-add-solutions.md) poskytují přehled o data.
- [Použití vyhledávací dotazy](log-analytics-log-searches.md) k analýze dat.
