<properties
    pageTitle="Vytvoření Windows virtuální počítače s sledování a diagnostice pomocí šablony správce prostředků Azure | Microsoft Azure"
    description="Vytvoření nového virtuálního počítače Windows s příponou Azure diagnostiky pomocí šablony správce Azure zdroje."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="sbtron"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="saurabh"/>

# <a name="create-a-windows-virtual-machine-with-monitoring-and-diagnostics-using-azure-resource-manager-template"></a>Vytvoření Windows virtuální počítače s monitorování a diagnostice pomocí šablony správce prostředků Azure

Rozšíření diagnostiky Azure poskytuje sledování a možnosti Diagnostika v systému Windows na základě Azure virtuálního počítače. Je-li povolit tyto funkce počítače virtuální, včetně přípony jako součást šabloně správce azure zdrojů. Další informace o včetně libovolnou linku jako součást šablony virtuálního počítače najdete v článku [Vytváření Azure správce prostředků šablony s OM](virtual-machines-windows-extensions-authoring-templates.md) . Tento článek popisuje, jak přidat koncovku Azure diagnostiky do šablony windows virtuálního počítače.  
  

## <a name="add-the-azure-diagnostics-extension-to-the-vm-resource-definition"></a>Přidání rozšíření Azure Diagnostika pro definici OM zdroje 

Chcete-li povolit koncovku Diagnostika v systému Windows virtuálního počítače budete muset přidat koncovku jako OM prostředků v šabloně správce zdrojů.

Pro jednoduché správce prostředků základě virtuálního počítače přidání konfigurace rozšíření pole *zdrojů* pro virtuálního počítače: 

    "resources": [
                {
                    "name": "Microsoft.Insights.VMDiagnosticsSettings",
                    "type": "extensions",
                    "location": "[resourceGroup().location]",
                    "apiVersion": "2015-06-15",
                    "dependsOn": [
                        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
                    ],
                    "tags": {
                        "displayName": "AzureDiagnostics"
                    },
                    "properties": {
                        "publisher": "Microsoft.Azure.Diagnostics",
                        "type": "IaaSDiagnostics",
                        "typeHandlerVersion": "1.5",
                        "autoUpgradeMinorVersion": true,
                        "settings": {
                            "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), variables('vmName'), variables('wadcfgxend')))]",
                            "storageAccount": "[parameters('existingdiagnosticsStorageAccountName')]"
                        },
                        "protectedSettings": {
                            "storageAccountName": "[parameters('existingdiagnosticsStorageAccountName')]",
                            "storageAccountKey": "[listkeys(variables('accountid'), '2015-05-01-preview').key1]",
                            "storageAccountEndPoint": "https://core.windows.net"
                        }
                    }
                }
            ]


Jiné běžné názvů je přidání konfigurace rozšíření v kořenovém uzlu zdroje šablony místo definování zdroje uzlu virtuálního počítače. Tento způsob máte explicitně zadat hierarchické vztahy mezi rozšíření a virtuálního počítače s hodnotami *název* a *Typ* . Příklad: 
  
    "name": "[concat(variables('vmName'),'Microsoft.Insights.VMDiagnosticsSettings')]",
    "type": "Microsoft.Compute/virtualMachines/extensions",

Rozšíření přidružen vždy virtuálního počítače, můžete buď přímo přímo definování zdroje uzlu virtuálního počítače nebo definovat na úrovni base a pomocí hierarchické konvence přidružit virtuální počítač.

Pro virtuální počítač měřítko sady konfigurace rozšíření zadané ve vlastnosti *extensionProfile* *VirtualMachineProfile*.
   
Vlastnosti *aplikace publisher* s hodnotou **Microsoft.Azure.Diagnostics** a *Typ* s hodnotou **IaaSDiagnostics** bylo možné jednoznačně identifikovat koncovku Azure diagnostiky.

Hodnota vlastnosti *název* rozhraní mohou sloužit k rozšíření ve skupině prostředků. Nastavení konkrétně na **Microsoft.Insights.VMDiagnosticsSettings** umožní snadno identifikovat tak, že Azure klasické portálu portálu zajistit, aby sledování grafy zobrazí správně v portálu Azure klasické.

*TypeHandlerVersion* Určuje verzi příponu, kterou chcete použít. Nastavení *autoUpgradeMinorVersion* podverze na **hodnotu true** zajistí bude nejnovější podverze rozšíření, které je k dispozici. Důrazně doporučujeme nastavit vždy *autoUpgradeMinorVersion* jako vždy **true** tak, že si vždy můžete použít nejnovější rozšíření dostupné diagnostiky s novými funkcemi a opravy chyb. 

*Nastavení* prvek obsahuje konfigurace vlastností pro rozšíření, které můžete nastavit a číst zpět z linky (někdy označovány jako veřejné konfigurace). Vlastnost *xmlcfg* obsahuje xml na základě konfigurace protokolování diagnostiky výkonnosti shromážděné agentem diagnostiky atd. Další informace o schéma xml samotné najdete v článku [Schéma konfigurace diagnostiky](https://msdn.microsoft.com/library/azure/dn782207.aspx) . Ve formátu Base 64 kódovat nastavte hodnotu pro *xmlcfg*běžné si je ukládat konfigurace skutečné xml jako proměnná v šabloně správce prostředků Azure a poté zřetězí. Naleznete v části [proměnné konfigurace diagnostiky](#diagnostics-configuration-variables) bližší informace o tom, jak uložit xml do proměnné. Vlastnost *storageAccount* Určuje název účtu úložiště, ke kterému bude převeden diagnostiky dat. 
 
Vlastnosti v *protectedSettings* (někdy označovány jako soukromé konfigurace) lze nastavit, ale nemůže přečíst zpět po nastavení. Pouze pro zápis přírodní *protectedSettings* díky kterému je užitečné pro ukládání tajemství jako klíč účtu úložiště místo, kam se měly zapisovat diagnostiky data.    

## <a name="specifying-diagnostics-storage-account-as-parameters"></a>Zadání účtu úložiště diagnostiky jako parametry 

Fragment diagnostiky rozšíření json výše předpokládá dvěma parametry *existingdiagnosticsStorageAccountName* a *existingdiagnosticsStorageResourceGroup* určete účet úložiště diagnostiky místo, kam se ukládají data diagnostiky. Zadání účtu úložiště diagnostiky jako parametr umožňuje snadno změnit účtu úložiště Diagnostika v různých prostředích například je vhodné používat účet úložiště různých Diagnostika pro testování a jiný název pro nasazení výroby.  

        "existingdiagnosticsStorageAccountName": {
            "type": "string",
            "metadata": {
        "description": "The name of an existing storage account to which diagnostics data will be transfered."
            }        
        },
        "existingdiagnosticsStorageResourceGroup": {
            "type": "string",
            "metadata": {
        "description": "The resource group for the storage account specified in existingdiagnosticsStorageAccountName"
            }
        }

Je vhodné pokusit se o to zadejte účet úložiště Diagnostika v jiné skupině zdrojů než skupina zdroje pro virtuální počítač. Skupina zdroje můžete považovat za jednotku nasazení s vlastním životnost, virtuálního počítače můžete nasazeném a znovu nasadit jako nové konfigurace aktualizace jsou určené pro ho ale budete chtít pokračovat ukládání dat diagnostiky ve stejném úložišti účtu přes tyto nasazení virtuálního počítače. Bez účtu úložiště v různých zdrojů umožňuje účtu úložiště přijmout data z různých virtuálního počítače nasazení usnadňuje Poradce při potížích s mezi různými verzemi.

>[AZURE.NOTE] Je-li vytvořit šablonu virtuálního počítače windows z aplikace Visual Studio výchozí účet úložiště může nastavit použít stejný účet úložiště, kde je uložená virtuálního počítače virtuální pevný disk. Toto je zjednodušit počáteční instalace OM. Měli znova faktor šablonu, kterou chcete použít jiný úložiště účet, který může být předaný jako parametr. 

## <a name="diagnostics-configuration-variables"></a>Proměnné konfigurace diagnostických nástrojů
 
Fragment diagnostiky rozšíření json výše definuje proměnnou *accountid* zjednodušit začíná klíč účtu úložiště pro ukládání diagnostických nástrojů:   
    
    "accountid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/',parameters('existingdiagnosticsStorageResourceGroup'), '/providers/','Microsoft.Storage/storageAccounts/', parameters('existingdiagnosticsStorageAccountName'))]"


Vlastnost *xmlcfg* pro rozšíření diagnostiky je definována pomocí více proměnných, které jsou spojeny společně. Hodnoty tyto proměnné jsou ve formátu xml, je nutné správně při nastavování proměnné json.

V následujícím textu najdete xml konfigurace diagnostických nástrojů, který shromáždí standardní systém úrovně výkonnosti spolu s některé protokoly událostí systému windows a infrastruktury protokolování diagnostiky. Bylo uvést a správně naformátovaný tak, aby konfigurace můžete vložit přímo do části proměnné šablony. V tématu [Schéma konfigurace diagnostiky](https://msdn.microsoft.com/library/azure/dn782207.aspx) víc lidí čitelné příklad xml konfiguraci.
    
        "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
        "wadperfcounters1": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Privileged Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU privileged time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% User Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU user time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor Information(_Total)\\Processor Frequency\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"CPU frequency\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\System\\Processes\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Processes\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Threads\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Handle Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Handles\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\% Committed Bytes In Use\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Memory usage\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Available Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory available\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Committed Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory committed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Commit Limit\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory commit limit\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active time\" locale=\"en-us\"/></PerformanceCounterConfiguration>",
        "wadperfcounters2": "<PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Read Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active read time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Write Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active write time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Transfers/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Reads/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk read operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Writes/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk write operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Read Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk read speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Write Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk write speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\LogicalDisk(_Total)\\% Free Space\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk free space (percentage)\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
        "wadcfgxstart": "[concat(variables('wadlogs'), variables('wadperfcounters1'), variables('wadperfcounters2'), '<Metrics resourceId=\"')]",
        "wadmetricsresourceid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name , '/providers/', 'Microsoft.Compute/virtualMachines/')]",
        "wadcfgxend": "\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>"

Uzel xml definice metriky ve výše uvedené konfigurace, je důležité konfigurace prvek jak definuje jak výkonnosti dříve podle jazyka xml v *PerformanceCounter* uzel agregované a budou uloženy. 

> [AZURE.IMPORTANT] Tyto metriky jednotka sledování grafy a upozornění na portálu Azure.  **Metriky** uzel s *resourceID* a **MetricAggregation** musí být součástí diagnostiky konfiguraci vašeho OM, pokud chcete zobrazit údaje o sledování OM Azure portálu. 

Následující obrázek je příkladem xml metrikou definic: 

        <Metrics resourceId="/subscriptions/subscription().subscriptionId/resourceGroups/resourceGroup().name/providers/Microsoft.Compute/virtualMachines/vmName">
            <MetricAggregation scheduledTransferPeriod="PT1H"/>
            <MetricAggregation scheduledTransferPeriod="PT1M"/>
        </Metrics>

Atribut *resourceID* jednoznačně identifikuje virtuálního počítače v předplatného. Zkontrolujte, že funkce subscription() a resourceGroup() tak, aby šablony automaticky aktualizuje jsou tyto hodnoty na základě předplatného a pole Skupina zdroje, které nasazujete.

Pokud vytváříte více virtuálních počítačích ve smyčce budete muset naplnění hodnotu *resourceID* s funkcí copyIndex() správně od sebe jednotlivé jednotlivé OM. Hodnotu *xmlCfg* lze aktualizovat na podporu to takto:  

    "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), concat(parameters('vmNamePrefix'), copyindex()), variables('wadcfgxend')))]", 

Hodnota MetricAggregation *PT1H* a *PT1M* znamenat agregaci za minutu a agregace myši za hodinu.

## <a name="wadmetrics-tables-in-storage"></a>WADMetrics tabulkami v úložiště

Nastavením metriky vygeneruje tabulky ve vašem účtu úložiště diagnostických nástrojů s následující konvence:

- **WADMetrics** : standardní předpony pro všechny WADMetrics tabulkami
- **PT1H** nebo **PT1M** : označuje, že tabulka obsahuje víc než 1 hodinu nebo minutu agregace dat
- **P10D** : označuje tabulka bude obsahovat data pro 10 dnů ze kdy tabulce spuštěna shromažďování dat
- **V2S** : řetězcová konstanta
- **RRRRMMDD** : datum, pro niž tabulce shromažďování dat

Příklad: *WADMetricsPT1HP10DV2S20151108* bude obsahovat data metriky agregované přes hodinu 10 dnů začíná 11 listopadu 2015    

Jednotlivými WADMetrics tabulkami bude obsahovat následující sloupce:

- **PartitionKey**: partitionkey je vytvořen na základě hodnoty *resourceID* identifikují OM zdroje. pro např: 002Fsubscriptions:<subscriptionID>: 002FresourceGroups:002F<ResourceGroupName>: 002Fproviders:002FMicrosoft:002ECompute:002FvirtualMachines:002F<vmName>  
- **RowKey** : sleduje formátu <Descending time tick>:<Performance Counter Name>. Sestupné výpočet popisky čas je maximální době značky mínus čas začátku období placení agregace. Například Pokud období ukázkové začít pracovat s 10 listopadu 2015 a 00:00Hrs UTC pak výpočtu bude: DateTime.MaxValue.Ticks - (nové DateTime(2015,11,10,0,0,0,DateTimeKind.Utc). Značky). Paměť Bajty k dispozici výkon čítač klávesu řádek bude vypadat podobně jako: 2519551871999999999__:005CMemory:005CAvailable:0020 bajtů
- **Název_čítače** : je název tohoto čítače. To odpovídá *counterSpecifier* podle konfigurace xml.
- **Maximální** : maximální hodnota čítače agregace období.
- **Minimální** : minimální hodnota čítače agregace období.
- **Celková** : součet všech hodnot čítače výkonu vykázaného za období agregace.
- **Počet** : Celkový počet hodnot vykázaného za čítač výkonu.
- **Průměrná** : průměr (celkem/počet) přínosu čítače agregace období.


## <a name="next-steps"></a>Další kroky

- Celá Ukázková šablona Windows virtuálního počítače s příponou diagnostiky najdete v článku [201 OM monitorování diagnostiky rozšíření](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-monitoring-diagnostics-extension)   
- Nasazení šablony správce zdrojů pomocí [Prostředí PowerShell Azure](virtual-machines-windows-ps-manage.md) nebo [Azure příkazového řádku](virtual-machines-linux-cli-deploy-templates.md)
- Další informace o [vytváření šablon správce prostředků Azure](../resource-group-authoring-templates.md)







