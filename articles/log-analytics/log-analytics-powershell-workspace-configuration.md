<properties
    pageTitle="Použití Powershellu ke vytváření a konfigurace pracovní prostor protokolu analýzy | Microsoft Azure"
    description="Protokolování údajů o použití analýzy na serverech ve vaší místní nebo cloudové infrastruktury. Shromáždit data počítače z Azure úložiště až generovaná diagnostických nástrojů Azure."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="powershell"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="richrund"/>

# <a name="manage-log-analytics-using-powershell"></a>Správa protokolu analýzy pomocí prostředí PowerShell

Můžete použít [rutiny prostředí PowerShell analýzy protokolu] (https://msdn.microsoft.com/library/mt188224(v=azure.300\).aspx) dělat různé funkce v protokolu analýzy z příkazového řádku nebo jako součást skriptu.  Příklady úkoly, které lze provést pomocí prostředí PowerShell:

+ Vytvoření pracovního prostoru aplikace Groove
+ Přidání nebo odebrání řešení
+ Import a export uložená hledání
+ Vytvoření skupiny pro počítače
+ Povolení kolekce protokoly služby IIS z počítače s Windows agent nainstalovaný
+ Shromáždit výkonnosti z počítače Linux a Windows
+ Shromažďovat události z syslog Linux počítačů 
+ Shromažďovat události z protokoly událostí systému Windows
+ Shromáždění protokolů vlastní události
+ Přidání agent analýzy protokolu Azure virtuálního počítače
+ Konfigurace protokolu analýzy indexovat data shromažďované pomocí Azure diagnostiky


Tento článek obsahuje dva ukázek kódu, které ilustrují některé funkce, které můžete provádět Powershellu.  Můžete vytvořit odkaz [reference pro rutiny prostředí PowerShell analýzy protokolu] (https://msdn.microsoft.com/library/mt188224(v=azure.300\).aspx) u jiných funkcí.

> [AZURE.NOTE] Protokol analýzy byla dříve nazývanou provozní přehledy, a proto je název použitý ve rutiny.

## <a name="prerequisites"></a>Zjistit předpoklady pro

Pomocí prostředí PowerShell pracovního prostoru protokolu analýzy, musíte mít:

+ Předplatné Azure a 
+ Azure protokolu analýzy pracovního prostoru propojené s Azure předplatné.

Pokud jste vytvořili pracovního prostoru OMS, ale ho není zatím propojena s Azure předplatného můžete vytvořit odkaz:

+ Na portálu Azure
+ Na portálu OMS nebo 
+ Pomocí rutiny Get-AzureRmOperationalInsightsLinkTargets a nový AzureRmOperationalInsightsWorkspace.


## <a name="create-and-configure-a-log-analytics-workspace"></a>Vytváření a konfigurace pracovní prostor protokolu analýzy

V následujícím příkladu skript ukazuje postup:

1.  Vytvoření pracovního prostoru aplikace Groove
2.  Seznam dostupných řešení
3.  Přidání řešení do pracovního prostoru
4.  Hledání importem uložili
5.  Export uložené hledání
6.  Vytvoření skupiny pro počítače
7.  Povolení kolekce protokoly služby IIS z počítače s Windows agent nainstalovaný
8.  Shromáždit logické čítače výkonu z počítače Linux (% používá Inodes; Bezplatné megabajtů; % Použít místo; Přenosy disku/s; Čtení z disku/s; Zápisy na disk/s)
9.  Shromáždit syslog události z Linux počítačů
10. Shromáždit chyb a upozornění události z protokolu událostí z počítače se systémem Windows
11. Shromáždit dostupné MB paměti čítače z počítače se systémem Windows
12. Shromáždit vlastní protokol 


```

$ResourceGroup = "oms-example"
$WorkspaceName = "log-analytics-" + (Get-Random -Maximum 99999) # workspace names need to be unique - Get-Random helps with this for the example code
$Location = "westeurope"

# List of solutions to enable
$Solutions = "Security", "Updates", "SQLAssessment"

# Saved Searches to import
$ExportedSearches = @"
[
    {
        "Category":  "My Saved Searches",
        "DisplayName":  "WAD Events (All)",
        "Query":  "Type=Event SourceSystem:AzureStorage ",
        "Version":  1
    },
    {        
        "Category":  "My Saved Searches",
        "DisplayName":  "Current Disk Queue Length",
        "Query":  "Type=Perf ObjectName=LogicalDisk InstanceName=\"C:\" CounterName=\"Current Disk Queue Length\"",
        "Version":  1
    }
]
"@ | ConvertFrom-Json

# Custom Log to collect
$CustomLog = @"
{
    "customLogName": "sampleCustomLog1", 
    "description": "Example custom log datasource", 
    "inputs": [
        { 
            "location": { 
            "fileSystemLocations": { 
                "windowsFileTypeLogPaths": [ "e:\\iis5\\*.log" ], 
                "linuxFileTypeLogPaths": [ "/var/logs" ] 
                } 
            }, 
        "recordDelimiter": { 
            "regexDelimiter": { 
                "pattern": "\\n", 
                "matchIndex": 0, 
                "matchIndexSpecified": true, 
                "numberedGroup": null 
                } 
            } 
        }
    ], 
    "extractions": [
        { 
            "extractionName": "TimeGenerated", 
            "extractionType": "DateTime", 
            "extractionProperties": { 
                "dateTimeExtraction": { 
                    "regex": null, 
                    "joinStringRegex": null 
                    } 
                } 
            }
        ] 
    }
"@

# Create the resource group if needed
try {
    Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop
} catch {
    New-AzureRmResourceGroup -Name $ResourceGroup -Location $Location
}

# Create the workspace
New-AzureRmOperationalInsightsWorkspace -Location $Location -Name $WorkspaceName -Sku Standard -ResourceGroupName $ResourceGroup

# List all solutions and their installation status
Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Add solutions
foreach ($solution in $Solutions) {
    Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -IntelligencePackName $solution -Enabled $true
}

#List enabled solutions
(Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Where({($_.enabled -eq $true)})

# Import Saved Searches
foreach ($search in $ExportedSearches) {
    $id = $search.Category + "|" + $search.DisplayName
    New-AzureRmOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId $id -DisplayName $search.DisplayName -Category $search.Category -Query $search.Query -Version $search.Version
}

# Export Saved Searches
(Get-AzureRmOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Value.Properties | ConvertTo-Json 

# Create Computer Group
New-AzureRmOperationalInsightsComputerGroup -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId "My Web Servers" -DisplayName "Web Servers" -Category "My Saved Searches" -Query "Computer=""web*"" | distinct Computer" -Version 1

# Enable IIS Log Collection using agent
Enable-AzureRmOperationalInsightsIISLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Perf
New-AzureRmOperationalInsightsLinuxPerformanceObjectDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Logical Disk" -InstanceName "*"  -CounterNames @("% Used Inodes", "Free Megabytes", "% Used Space", "Disk Transfers/sec", "Disk Reads/sec", "Disk Reads/sec", "Disk Writes/sec") -IntervalSeconds 20  -Name "Example Linux Disk Performance Counters"
Enable-AzureRmOperationalInsightsLinuxCustomLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Syslog
New-AzureRmOperationalInsightsLinuxSyslogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -Facility "kern" -CollectEmergency -CollectAlert -CollectCritical -CollectError -CollectWarning -Name "Example kernal syslog collection"
Enable-AzureRmOperationalInsightsLinuxSyslogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Windows Event
New-AzureRmOperationalInsightsWindowsEventDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -EventLogName "Application" -CollectErrors -CollectWarnings -Name "Example Application Event Log"

# Windows Perf
New-AzureRmOperationalInsightsWindowsPerformanceCounterDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Memory" -InstanceName "*" -CounterName "Available MBytes" -IntervalSeconds 20 -Name "Example Windows Performance Counter"

# Custom Logs
New-AzureRmOperationalInsightsCustomLogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -CustomLogRawJson "$CustomLog" -Name "Example Custom Log Collection"

```

## <a name="configuring-log-analytics-to-index-azure-diagnostics"></a>Konfigurace protokolu analýzy indexovat Azure diagnostiky 

Agentless sledování Azure prostředků, zdrojů musí mít Azure diagnostiky povolené a nakonfigurovali zapisovat do účtu úložiště. Technologie pro analýzu protokolu je možné konfigurovat pak pokud chcete shromáždit protokoly z účtu úložiště. Zdroje informací, které musíte udělat předcházející konfigurace obsahuje:

+ Klasický cloudovým službám (role pro web a pracovní)
+ Service struktury clusterů
+ Skupiny zabezpečení sítě
+ Základní trezorů a 
+ Aplikace brány

Prostředí PowerShell můžete taky používá ke konfiguraci pracovního prostoru protokolu analýzy v Azure předplatných shromáždit protokoly z různých Azure předplatného.

Následující příklad ukazuje postup:

1.  Seznam stávajících účtů úložiště a umístění, která protokolu analýzy budou indexovat data z
2.  Vytvoření konfigurace číst z účtu úložiště
3.  Aktualizace nově vytvořený konfigurace indexovat data z dalších umístění
4.  Odstranění nově vytvořený konfigurace

```
# validTables = "WADWindowsEventLogsTable", "LinuxsyslogVer2v0", "WADServiceFabric*EventTable", "WADETWEventTable" 
$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq "your workspace name"})

# Update these two lines with the storage account resource ID and the storage account key for the storage account you want to Log Analytics to  
$storageId = "/subscriptions/ec11ca60-1234-491e-5678-0ea07feae25c/resourceGroups/demo/providers/Microsoft.Storage/storageAccounts/wadv2storage"
$key = "abcd=="

# List existing insights
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name

# Create a new insight
New-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -StorageAccountResourceId $storageId -StorageAccountKey $key -Tables @("WADWindowsEventLogsTable") -Containers @("wad-iis-logfiles")

# Update existing insight
Set-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -Tables @("WADWindowsEventLogsTable", "WADETWEventTable") -Containers @("wad-iis-logfiles", "insights-logs-networksecuritygroupevent/resourceId=/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO")

# Remove the insight
Remove-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" 

```

## <a name="next-steps"></a>Další kroky

- [Rutiny prostředí PowerShell analýzy protokolu revize](http://msdn.microsoft.com/library/mt188224.aspx) Další informace o použití Powershellu pro konfiguraci protokolu analýzy.

