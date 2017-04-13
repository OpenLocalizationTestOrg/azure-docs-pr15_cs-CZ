<properties
    pageTitle="Nastaví automatické měřítko Windows virtuálního počítače měřítko | Microsoft Azure"
    description="Nastavení neobsahovaly text sady Windows virtuálního počítače měřítko pomocí prostředí PowerShell Azure"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="automatically-scale-machines-in-a-virtual-machine-scale-set"></a>Automaticky nastavit měřítko počítačích v sadě měřítko virtuálního počítače

Virtuální počítač měřítko sady usnadňují nasazením a správou identickými virtuálních počítačích jako sady. Sady měřítko poskytují vrstvě vysoce scalable a přizpůsobitelné výpočetním mnoha aplikací a podporují obrázky platformu Windows, Linux platformy obrázky, vlastní obrázky a rozšíření. Další informace o sadách měřítko najdete v článku [Nastaví měřítko virtuálního počítače](virtual-machine-scale-sets-overview.md).

Tento kurz se dozvíte, jak vytvořit sadu měřítko virtuálních počítačích Windows a automaticky měřítko strojů v sadě. Vytvoření měřítko nastavení a nastavení proměnné velikosti – vytvoření šablony pro správce prostředků Azure a nasazení pomocí prostředí PowerShell Azure. Další informace o šablonách najdete v článku [vytváření správce prostředků Azure šablony](../resource-group-authoring-templates.md). Další informace o automatické změny velikosti sad měřítko, najdete v článku [Automatické měřítko a nastaví měřítko virtuálního počítače](virtual-machine-scale-sets-autoscale-overview.md).

V tomto článku nasadíte následující zdroje a přípon souborů:

- Microsoft.Storage/storageAccounts
- Microsoft.Network/virtualNetworks
- Microsoft.Network/publicIPAddresses
- Microsoft.Network/loadBalancers
- Microsoft.Network/networkInterfaces
- Microsoft.Compute/virtualMachines
- Microsoft.Compute/virtualMachineScaleSets
- Microsoft.Insights.VMDiagnosticsSettings
- Microsoft.Insights/autoscaleSettings

Další informace o zdrojích správce prostředků najdete v článku [Výpočet Azure sítě a poskytovatelů úložiště v části správce prostředků Azure](../virtual-machines/virtual-machines-windows-compare-deployment-models.md).

## <a name="step-1-install-azure-powershell"></a>Krok 1: Instalace modulu Azure PowerShell

Zjistěte, [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md) informace o instalaci nejnovější verzi Azure Powershellu, vyberte předplatné a přihlášení k Azure.

## <a name="step-2-create-a-resource-group-and-a-storage-account"></a>Krok 2: Vytvoření skupiny zdrojů a účet úložiště

1. **Vytvoření skupiny zdrojů** – všechny zdroje musí být nasazené na skupina zdroje. Umožňuje vytvořit skupinu zdroje s názvem **vmsstestrg1** [AzureRmResourceGroup nový](https://msdn.microsoft.com/library/mt603739.aspx) .

2. **Vytvoření účtu úložiště** – tento účet úložiště je, kde je šablona uložená. Umožňuje vytvořit účet úložiště s názvem **vmsstestsa** [AzureRmStorageAccount nový](https://msdn.microsoft.com/library/mt607148.aspx) .

## <a name="step-3-create-the-template"></a>Krok 3: Vytvoření šablony
Správce prostředků Azure šablona umožňuje nasazením a správou Azure zdroje společně s použitím JSON popis zdroje a parametrů přidružené nasazení.

1. V seznamu Oblíbené editoru vytvořte soubor C:\VMSSTemplate.json a přidejte počáteční strukturu JSON podporuje šablony.

        {
          "$schema":"http://schema.management.azure.com/schemas/2014-04-01-preview/VM.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
          },
          "variables": {
          },
          "resources": [
          ]
        }

2. Parametry vždy nejsou povinná, ale poskytují způsob, jak vstupní hodnoty při zavedení šabloně. Přidejte tyto parametry v části nadřazený prvek parametrů, které jste přidali do šablony.

        "vmName": { "type": "string" },
        "vmSSName": { "type": "string" },
        "instanceCount": { "type": "string" },
        "adminUsername": { "type": "string" },
        "adminPassword": { "type": "securestring" },
        "resourcePrefix": { "type": "string" }

    - Nastavte název samostatné virtuální počítač, který se používá k přístupu počítačích v měřítku.
    - Název účtu úložiště, kde je šablona uložená.
    - Počet virtuálních počítačích původně vytvořit v sadě měřítko.
    - Jméno a heslo účtu správce na virtuálních počítačích.
    - Nastavte název předpony pro zdroje, které jsou vytvořené pro podporu měřítka.

3. Proměnné lze do šablony zadejte hodnoty, které se mohou často měnit nebo hodnoty, které je potřeba vytvořit z kombinací hodnoty parametrů. Přidejte tyto proměnných ve skupinovém rámečku proměnné nadřazený prvek, který jste přidali do šablony.

        "dnsName1": "[concat(parameters('resourcePrefix'),'dn1')]",
        "dnsName2": "[concat(parameters('resourcePrefix'),'dn2')]",
        "publicIP1": "[concat(parameters('resourcePrefix'),'ip1')]",
        "publicIP2": "[concat(parameters('resourcePrefix'),'ip2')]",
        "loadBalancerName": "[concat(parameters('resourcePrefix'),'lb1')]",
        "virtualNetworkName": "[concat(parameters('resourcePrefix'),'vn1')]",
        "nicName": "[concat(parameters('resourcePrefix'),'nc1')]",
        "lbID": "[resourceId('Microsoft.Network/loadBalancers',variables('loadBalancerName'))]",
        "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/loadBalancerFrontEnd')]",
        "storageAccountSuffix": [ "a", "g", "m", "s", "y" ],
        "diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'a')]",
        "accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/','Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
          "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
        "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
        "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
        "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
        "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"

  - Názvy DNS, které využívají rozhraní sítě.
    - IP adresy názvů a předpon virtuální sítě a podsítí.
    - Jména a identifikátory virtuální sítě načíst vyrovnávání a síťových rozhraní.
    - Názvy účtů úložiště pro účty přidružené počítačích v měřítku nastavení.
    - Nastavení pro rozšíření diagnostických nástrojů, které máte nainstalované na virtuálních počítačích. Další informace o koncovku Diagnostika v tématu [Vytvoření Windows virtuální stroje s sledování a diagnostice pomocí šablony Azure správce prostředků](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md).

4. Přidání zdroje účtu úložiště v části nadřazený prvek zdroje, které jste přidali do šablony. Tato šablona používá smyčku vytvořit doporučené pět účtů úložiště disků operačního systému a dat diagnostiky uloženými. Toto nastavení účtů podporují až 100 virtuálních počítačích v sadě měřítko, což je aktuální maximum. Každý účet úložiště je použito písmeno pro indikaci, která byla definována v proměnné společně s předponou poskytnete v parametrech šablony.

        {
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[concat(parameters('resourcePrefix'), variables('storageAccountSuffix')[copyIndex()])]",
          "apiVersion": "2015-06-15",
          "copy": {
            "name": "storageLoop",
            "count": 5
          },
          "location": "[resourceGroup().location]",
          "properties": { "accountType": "Standard_LRS" }
        },

5. Přidání zdroje virtuální sítě. Další informace najdete v tématu [Zprostředkovatele zdroje](../virtual-network/resource-groups-networking.md).

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "[variables('virtualNetworkName')]",
          "location": "[resourceGroup().location]",
          "properties": {
            "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
            "subnets": [
              {
                "name": "subnet1",
                "properties": { "addressPrefix": "10.0.0.0/24" }
              }
            ]
          }
        },

6. Přidání veřejné IP adresu zdroji, které využívají vyrovnávání zatížení a rozhraní sítě.

        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/publicIPAddresses",
          "name": "[variables('publicIP1')]",
          "location": "[resourceGroup().location]",
          "properties": {
            "publicIPAllocationMethod": "Dynamic",
            "dnsSettings": {
              "domainNameLabel": "[variables('dnsName1')]"
            }
          }
        },
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/publicIPAddresses",
          "name": "[variables('publicIP2')]",
          "location": "[resourceGroup().location]",
          "properties": {
            "publicIPAllocationMethod": "Dynamic",
            "dnsSettings": {
              "domainNameLabel": "[variables('dnsName2')]"
            }
          }
        },

7. Přidáte zdroj Vyrovnávání zatížení, který používá sadu měřítko. Další informace najdete v tématu [Podpory Azure správce prostředků pro vyrovnávání zatížení](../load-balancer/load-balancer-arm.md).

        {
          "apiVersion": "2015-06-15",
          "name": "[variables('loadBalancerName')]",
          "type": "Microsoft.Network/loadBalancers",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
          ],
          "properties": {
            "frontendIPConfigurations": [
              {
                "name": "loadBalancerFrontEnd",
                "properties": {
                  "publicIPAddress": {
                    "id": "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
                  }
                }
              }
            ],
            "backendAddressPools": [ { "name": "bepool1" } ],
            "inboundNatPools": [
              {
                "name": "natpool1",
                "properties": {
                  "frontendIPConfiguration": {
                    "id": "[variables('frontEndIPConfigID')]"
                  },
                  "protocol": "tcp",
                  "frontendPortRangeStart": 50000,
                  "frontendPortRangeEnd": 50500,
                  "backendPort": 3389
                }
              }
            ]
          }
        },

8. Přidání síťového rozhraní prostředku, který se používá pro samostatné virtuální počítač. Protože počítačích v sadě měřítko nejsou přístupné přes veřejnou IP adresu, samostatné virtuálního počítače se vytvoří ve stejné síti virtuální vzdálený přístup počítačích.

        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[variables('nicName')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP2'))]",
            "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
          ],
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "privateIPAllocationMethod": "Dynamic",
                  "publicIPAddress": {
                    "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicIP2'))]"
                  },
                  "subnet": {
                    "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
                  }
                }
              }
            ]
          }
        },

9. Přidání samostatné virtuálního počítače ve stejné síti jako sada měřítko.

        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Compute/virtualMachines",
          "name": "[parameters('vmName')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "storageLoop",
            "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
          ],
          "properties": {
            "hardwareProfile": { "vmSize": "Standard_A1" },
            "osProfile": {
              "computername": "[parameters('vmName')]",
              "adminUsername": "[parameters('adminUsername')]",
              "adminPassword": "[parameters('adminPassword')]"
            },
            "storageProfile": {
              "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "2012-R2-Datacenter",
                "version": "latest"
              },
              "osDisk": {
                "name": "[concat(parameters('resourcePrefix'), 'os1')]",
                "vhd": {
                  "uri":  "[concat('https://',parameters('resourcePrefix'),'a.blob.core.windows.net/vhds/',parameters('resourcePrefix'),'os1.vhd')]"
                },
                "caching": "ReadWrite",
                "createOption": "FromImage"        
              }
            },
            "networkProfile": {
              "networkInterfaces": [
                {
                  "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
                }
              ]
            }
          }
        },

10. Přidejte, že měřítko virtuálního počítače nastavený zdroje a určit, že přípona diagnostických nástrojů, které máte nainstalované na všechny virtuálních počítačích v sadě měřítko. Většina nastavení pro tento zdroj se podobají s tímto zdrojem virtuálního počítače. Hlavní rozdíly jsou kapacita prvek, který určuje počet virtuálních počítačích v měřítku sadu a upgradePolicy, která určuje, jak jsou aktualizace provedené virtuálních počítačích. Dokud všechny účty úložiště vytvořené není vytvořit sadu měřítko uvedeno elementem dependsOn.

            {
              "type": "Microsoft.Compute/virtualMachineScaleSets",
              "apiVersion": "2016-03-30",
              "name": "[parameters('vmSSName')]",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "storageLoop",
                "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
                "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
              ],
              "sku": {
                "name": "Standard_A1",
                "tier": "Standard",
                "capacity": "[parameters('instanceCount')]"
              },
              "properties": {
                "upgradePolicy": {
                  "mode": "Manual"
                },
                "virtualMachineProfile": {
                  "storageProfile": {
                    "osDisk": {
                      "vhdContainers": [
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[0], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[1], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[2], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[3], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[4], '.blob.core.windows.net/vhds')]"
                      ],
                      "name": "vmssosdisk",
                      "caching": "ReadOnly",
                      "createOption": "FromImage"
                    },
                    "imageReference": {
                      "publisher": "MicrosoftWindowsServer",
                      "offer": "WindowsServer",
                      "sku": "2012-R2-Datacenter",
                      "version": "latest"
                    }
                  },
                  "osProfile": {
                    "computerNamePrefix": "[parameters('vmSSName')]",
                    "adminUsername": "[parameters('adminUsername')]",
                    "adminPassword": "[parameters('adminPassword')]"
                  },
                  "networkProfile": {
                    "networkInterfaceConfigurations": [
                      {
                        "name": "networkconfig1",
                        "properties": {
                          "primary": "true",
                          "ipConfigurations": [
                            {
                              "name": "ip1",
                              "properties": {
                                "subnet": {
                                  "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
                                },
                                "loadBalancerBackendAddressPools": [
                                  {
                                    "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/backendAddressPools/bepool1')]"
                                  }
                                ],
                                "loadBalancerInboundNatPools": [
                                  {
                                    "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/inboundNatPools/natpool1')]"
                                  }
                                ]
                              }
                            }
                          ]
                        }
                      }
                    ]
                  },
                  "extensionProfile": {
                    "extensions": [
                      {
                        "name": "Microsoft.Insights.VMDiagnosticsSettings",
                        "properties": {
                          "publisher": "Microsoft.Azure.Diagnostics",
                          "type": "IaaSDiagnostics",
                          "typeHandlerVersion": "1.5",
                          "autoUpgradeMinorVersion": true,
                          "settings": {
                            "xmlCfg": "[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
                            "storageAccount": "[variables('diagnosticsStorageAccountName')]"
                          },
                          "protectedSettings": {
                            "storageAccountName": "[variables('diagnosticsStorageAccountName')]",
                            "storageAccountKey": "[listkeys(variables('accountid'), '2015-06-15').key1]",
                            "storageAccountEndPoint": "https://core.windows.net"
                          }
                        }
                      }
                    ]
                  }
                }
              }
            },

11. Přidání autoscaleSettings prostředek, který definuje, jak nastavit měřítko upraví podle využití procesoru v počítačích v sadě měřítko.

            {
              "type": "Microsoft.Insights/autoscaleSettings",
              "apiVersion": "2015-04-01",
              "name": "[concat(parameters('resourcePrefix'),'as1')]",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "[concat('Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
              ],
              "properties": {
                "enabled": true,
                "name": "[concat(parameters('resourcePrefix'),'as1')]",
                "profiles": [
                  {
                    "name": "Profile1",
                    "capacity": {
                      "minimum": "1",
                      "maximum": "10",
                      "default": "1"
                    },
                    "rules": [
                      {
                        "metricTrigger": {
                          "metricName": "\\Processor(_Total)\\% Processor Time",
                          "metricNamespace": "",
                          "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]",
                          "timeGrain": "PT1M",
                          "statistic": "Average",
                          "timeWindow": "PT5M",
                          "timeAggregation": "Average",
                          "operator": "GreaterThan",
                          "threshold": 50.0
                        },
                        "scaleAction": {
                          "direction": "Increase",
                          "type": "ChangeCount",
                          "value": "1",
                          "cooldown": "PT5M"
                        }
                      }
                    ]
                  }
                ],
                "targetResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
              }
            }

    Pro účely tohoto návodu jsou důležité tyto hodnoty:

    - **metricName** – tato hodnota je stejná jako čítače, která byla definována wadperfcounter proměnné. Používáte tuto proměnnou, shromažďuje koncovku diagnostiky **procesor(_Total)\% času procesoru** čítač.
    - **metricResourceUri** – tato hodnota je identifikátor zdroje s nastavením měřítko virtuálního počítače.
    - **timeGrain** – tato hodnota je metriky, které byly shromážděny. V této šabloně je nastavena na minutu.
    - **statistický** – tato hodnota určuje, jak kombinovat metriky tak, aby zahrnoval automatické změny velikosti akce. Možné hodnoty jsou: průměr, Min a Max. V této šablony se shromažďují průměr celkové využití procesoru virtuálních počítačích.
    - **hodnota timeWindow** – tato hodnota je oblast čas, ve kterém se shromažďují instance data. Musí být mezi 5 minut a 12 hodin.
    - **Agregace času** – tato hodnota určuje, jak by měl kombinovat data, která se shromažďují určitou dobu. Výchozí hodnota je průměr. Možné hodnoty jsou: průměr, Minimum, Maximum, poslední, součet, počet.
    - **operátor** – tato hodnota je operátor, který se používá k porovnání data metriky a prahové hodnoty. Možné hodnoty jsou: rovná se NotEquals GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.
    - **mezní hodnoty** – tato hodnota je hodnota, která spustí akci měřítko. V této šabloně počítačích přidají měřítko nastavení, když je průměrná využití procesoru mezi počítačích v sadě víc než 50 %.
    - **směr** – tato hodnota Určuje akci, která je provedena při dosažení mezní hodnota. Možné hodnoty jsou zvýšení nebo snížení. V této šablony se zvyšuje počet virtuálních počítačích v sadě měřítko, když je mezní hodnota je delší než 50 % definovaný časového intervalu.
    - **Typ** – tato hodnota je typ akci, která by měla proběhnout a musí být nastavený na ChangeCount.
    - **hodnoty** – tato hodnota je počet virtuálních počítačích, které jsou přidat i odebrat ze sady měřítko. Tato hodnota musí být 1 nebo větší. Výchozí hodnota 1. V této šabloně počtu počítačů v měřítku nastavil zvýšení 1 při splnění je mezní hodnota.
    - **cooldown** – tato hodnota je dobu čekat od poslední změny měřítka akce před další akce. Tato hodnota musí být mezi jednu minutu a jeden týden.

12. Uložte soubor šablony.    

## <a name="step-4-upload-the-template-to-storage"></a>Krok 4: Nahrání šablony do úložiště

Šablony můžete nahrát, dokud znáte název a primární klíč účtu úložiště, který jste vytvořili v kroku 1.

1.  V okně Microsoft Azure PowerShell nastavte proměnnou určující název účtu úložiště, který jste vytvořili v kroku 1.

            $storageAccountName = "vmstestsa"

2.  Nastavte proměnnou, která určuje primární klíč účtu úložiště.

            $storageAccountKey = "<primary-account-key>"

    Tento klíč se připojíte klepnutím na ikonu klíče při prohlížení zdroje účtu úložiště na portálu Azure.

3.  Vytvoření úložiště účtu kontextový objekt, který se používá k ověření operace s účtem úložiště.

            $ctx = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

4.  Vytvořte kontejner pro uložení šablony.

            $containerName = "templates"
            New-AzureStorageContainer -Name $containerName -Context $ctx  -Permission Blob

5.  Odeslání souboru šablony do nového kontejneru.

            $blobName = "VMSSTemplate.json"
            $fileName = "C:\" + $BlobName
            Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob  $blobName -Context $ctx

## <a name="step-5-deploy-the-template"></a>Krok 5: Nasazení šablony

Teď jste vytvořili šabloně, se můžete pustit nasazení zdroje. Pomocí následujícího příkazu ke spuštění procesu:

    New-AzureRmResourceGroupDeployment -Name "vmsstestdp1" -ResourceGroupName "vmsstestrg1" -TemplateUri "https://vmsstestsa.blob.core.windows.net/templates/VMSSTemplate.json"

Po stisknutí klávesy enter, zobrazí se výzva k zadání hodnot pro proměnné, které jste přiřadili. Zadejte tyto hodnoty:

    vmName: vmsstestvm1
      vmSSName: vmsstest1
      instanceCount: 5
      adminUserName: vmadmin1
      adminPassword: VMpass1
      resourcePrefix: vmsstest

Trvá asi 15 minut pro všechny zdroje úspěšně nasazení.

>[AZURE.NOTE] Můžete taky možnost na portálu pro nasazení zdroje. Použijte tento odkaz: "https://portal.azure.com/#create/Microsoft.Template/uri/<link to VM Scale Set JSON template>"

## <a name="step-6-monitor-resources"></a>Krok 6: Sledování zdroje

Můžete získat některé informace o sadách měřítko virtuálního počítače pomocí těchto postupů:

 - Portál Azure – aktuálně dostanete omezené množství informací o na portálu.
 - [Azure zdroje Explorer](https://resources.azure.com/) - tento nástroj nejvhodnějšího pro prohlížení aktuální stav sadě měřítko. Postupujte podle tato cesta a byste měli vidět zobrazení instance množiny měřítko, kterou jste vytvořili:

        subscriptions > {your subscription} > resourceGroups > vmsstestrg1 > providers > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtualMachines

 - Azure PowerShell – pomocí tohoto příkazu určité informace:

        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

        Or

        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceView

 - Připojení k samostatné virtuální počítač úplně stejně, jako kdybyste druhý počítač a potom vzdáleně se dostanete virtuálních počítačích v měřítku nastavit sledování jednotlivé procesy.

>[AZURE.NOTE] Dokončení rozhraní REST API pro získání informací o sadách měřítka najdete v [Nastaví měřítko virtuálního počítače](https://msdn.microsoft.com/library/mt589023.aspx)

## <a name="step-7-remove-the-resources"></a>Krok 7: Odebrání zdroje

Protože vám bude účtovaná za zdrojů použitých v Azure, je vždy vhodné odstranit prostředky, které už nepotřebujete. Není potřeba odstranit jednotlivé zdroje zvlášť ze skupiny zdrojů. Skupina zdroje můžete odstranit a všechny zdroje jsou automaticky odstraní.

    Remove-AzureRmResourceGroup -Name vmsstestrg1

Pokud chcete zachovat skupina zdroje, můžete odstranit měřítko nastavte jenom.

    Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name"

## <a name="next-steps"></a>Další kroky

- Správa nastavení měřítko, že jste právě vytvořili podle pokynů v [Správa virtuálních počítačích v sadě měřítko virtuálního počítače](virtual-machine-scale-sets-windows-manage.md).
- Další informace o svislé měřítko kontrolou [svislé automatické měřítko sadami měřítko virtuálního počítače](virtual-machine-scale-sets-vertical-scale-reprovision.md)
- Vyhledání příklady Azure sledovat sledování funkcí v [Azure Monitor PowerShell rychlý start vzorky](../monitoring-and-diagnostics/insights-powershell-samples.md)
- Další informace o funkcích oznámení v [použít automatické měřítko akce, které chcete odeslat e-mailu a webhook oznámení v nástroji Sledování Azure](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)
- Zjistěte, jak chcete [protokoly auditování použití odeslat e-mailu a webhook oznámení v nástroji Sledování Azure](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)
