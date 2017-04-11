<properties
    pageTitle="Připojení k protokolu analýzy Azure virtuálních počítačích | Microsoft Azure"
    description="Doporučené postupy shromážděny protokoly a metriky pro Windows a Linux virtuálních počítačích spuštěné v Azure, je instalace OM Azure protokolu analýzy příponu. Instalace technologie pro analýzu protokolu rozšíření virtuálního počítače na Azure VMs můžete používat Azure portál nebo Powershellu."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="richrund"/>

# <a name="connect-azure-virtual-machines-to-log-analytics"></a>Připojení k protokolu analýzy Azure virtuálních počítačích

U počítačů s Windows a Linux pro shromáždění protokolů a metriky doporučujeme instalací agenta protokolu analýzy.

Nejjednodušší způsob, jak nainstalovat agenta protokolu analýzy Azure virtuálních počítačích je prostřednictvím protokolu analýzy OM rozšíření.  S příponou zjednodušuje proces instalace a automaticky nakonfiguruje agent odeslání dat do pracovního prostoru protokolu analýzy, který určíte. Agent také k automatické aktualizaci, zajistit, že máte nejnovější funkce a opravy.

Pro virtuálních počítačích Windows povolíte rozšíření *Microsoft sledování agenta* virtuálního počítače.
Pro Linux virtuálních počítačích povolíte koncovku virtuálního počítače *OMS Agent pro Linux* .

Další informace o [rozšíření Azure virtuálního počítače](../virtual-machines/virtual-machines-windows-extensions-features.md) a [Linux agenta] (... / virtual-machines/virtual-machines-linux-agent-user-guide.md).

Při použití na základě agent kolekce dat protokolu musíte nakonfigurovat [zdroje dat v protokolu analýzy](log-analytics-data-sources.md) určete protokoly a metriky, která chcete shromažďovat.

>[AZURE.IMPORTANT] Pokud konfigurace protokolu analýzy indexovat protokolu data pomocí [Azure Diagnostika](log-analytics-azure-storage.md)a konfigurace agenta získat informace o protokolech stejný, klikněte byly shromážděny protokoly dvakrát. Vám bude účtovaná za obou zdrojích dat. Pokud máte agent nainstalována, by měl shromažďovat data o protokolu pomocí samotný agent: není konfigurace protokolu analýzy sběr protokolu dat z Azure diagnostiky.

Existují tři jednoduchých způsobů, jak povolit rozšíření protokolu analýzy virtuálního počítače:

+ Pomocí portálu Azure
+ Pomocí prostředí PowerShell Azure
+ Pomocí šablony pro správce prostředků Azure

## <a name="enable-the-vm-extension-in-the-azure-portal"></a>Povolení rozšíření OM na portálu Azure

Můžete nainstalovat agent pro analýzu protokolu a připojte Azure virtuální počítač, který se spustí [Azure portálu](https://portal.azure.com).

### <a name="to-install-the-log-analytics-agent-and-connect-the-virtual-machine-to-a-log-analytics-workspace"></a>Nainstalovat agenta protokolu technologie pro analýzu a připojit virtuálního počítače do pracovního prostoru protokolu analýzy

1.  Přihlaste se k [portálu Azure](http://portal.azure.com).
2.  Vyberte **Procházet** na levé straně portálu a přejděte do **Protokolu analýzy (OMS)** a vyberte ho.
3.  V seznamu pracovních prostorů protokolu analýzy vyberte ten, který chcete použít s OM Azure.  
    ![Pracovní prostory OMS](./media/log-analytics-azure-vm-extension/oms-connect-azure-01.png)
4.  V části **Správa analýzy protokolu**vyberte **virtuálních počítačích**.  
    ![Virtuálních počítačích](./media/log-analytics-azure-vm-extension/oms-connect-azure-02.png)
5.  V seznamu **virtuálních počítačích**vyberte virtuálního počítače, na kterém chcete nainstalovat agent. **Stav připojení OMS** OM značí, že je **připojené**.  
    ![OM nejste připojení](./media/log-analytics-azure-vm-extension/oms-connect-azure-03.png)
6.  V části podrobností virtuálního počítače vyberte **Připojit**. Agent je automaticky nainstalovali a nakonfigurovali pro pracovní prostor protokolu analýzy. Tento proces trvá několik minut, během této doby stav připojení OMS je *připojení...*  
    ![Připojení OM](./media/log-analytics-azure-vm-extension/oms-connect-azure-04.png)
7.  Po instalaci a připojte agent stav **připojení OMS** aktualizován a zobrazí **Tento pracovní prostor**.  
    ![Připojení](./media/log-analytics-azure-vm-extension/oms-connect-azure-05.png)


## <a name="enable-the-vm-extension-using-powershell"></a>Povolte OM rozšíření pomocí prostředí PowerShell

Existují různé příkazy pro Azure klasické virtuálních počítačích a správce prostředků virtuálních počítačích. Tady jsou příklady klasické a virtuálních počítačích správce prostředků.

Klasický virtuálních počítačích použijte následující příklad Powershellu:

```
Add-AzureAccount

$workspaceId = "enter workspace ID here"
$workspaceKey = "enter workspace key here"
$hostedService = "enter hosted service here"

$vm = Get-AzureVM –ServiceName $hostedService

# For Windows VM uncomment the following line
# Set-AzureVMExtension -VM $vm -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionName 'MicrosoftMonitoringAgent' -Version '1.*' -PublicConfiguration "{'workspaceId': '$workspaceId'}" -PrivateConfiguration "{'workspaceKey': '$workspaceKey' }" | Update-AzureVM -Verbose

# For Linux VM uncomment the following line
# Set-AzureVMExtension -VM $vm -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionName 'OmsAgentForLinux' -Version '1.*' -PublicConfiguration "{'workspaceId': '$workspaceId'}" -PrivateConfiguration "{'workspaceKey': '$workspaceKey' }" | Update-AzureVM -Verbose
```

Správce prostředků virtuálních počítačích použijte následující příklad Powershellu:

```
Login-AzureRMAccount
Select-AzureSubscription -SubscriptionId "**"

$workspaceName = "your workspace name"
$VMresourcegroup = "**"
$VMresourcename = "**"

$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq $workspaceName})

if ($workspace.Name -ne $workspaceName)
{
    Write-Error "Unable to find OMS Workspace $workspaceName. Do you need to run Select-AzureRMSubscription?"
}

$workspaceId = $workspace.CustomerId
$workspaceKey = (Get-AzureRmOperationalInsightsWorkspaceSharedKeys -ResourceGroupName $workspace.ResourceGroupName -Name $workspace.Name).PrimarySharedKey

$vm = Get-AzureRmVM -ResourceGroupName $VMresourcegroup -Name $VMresourcename
$location = $vm.Location

# For Windows VM uncomment the following line
# Set-AzureRmVMExtension -ResourceGroupName $VMresourcegroup -VMName $VMresourcename -Name 'MicrosoftMonitoringAgent' -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionType 'MicrosoftMonitoringAgent' -TypeHandlerVersion '1.0' -Location $location -SettingString "{'workspaceId': '$workspaceId'}" -ProtectedSettingString "{'workspaceKey': '$workspaceKey'}"

# For Linux VM uncomment the following line
# Set-AzureRmVMExtension -ResourceGroupName $VMresourcegroup -VMName $VMresourcename -Name 'OmsAgentForLinux' -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionType 'OmsAgentForLinux' -TypeHandlerVersion '1.0' -Location $location -SettingString "{'workspaceId': '$workspaceId'}" -ProtectedSettingString "{'workspaceKey': '$workspaceKey'}"


```
Při konfiguraci virtuálního počítače pomocí prostředí PowerShell budete muset zadat **ID pracovní prostor** a **Primární klíč**. Id a klíč najdete na stránce **Nastavení** domény v portálu OMS nebo pomocí prostředí PowerShell uvedeno v předchozím příkladu.

![Pracovní prostor ID a primárního klíče](./media/log-analytics-azure-vm-extension/oms-analyze-azure-sources.png)

## <a name="deploy-the-vm-extension-using-a-template"></a>Nasazení rozšíření OM použít šablonu?

Pomocí Správce prostředků Azure můžete vytvořit jednoduchý šablony (ve formátu JSON), který definuje nasazení a konfiguraci aplikace. Tato šablona se označuje jako šablonu správce prostředků a poskytuje deklarativně definovat nasazení. Pomocí šablony můžete opakovaně nasazení aplikace v celém životním cyklu aplikace a důvěřovat, že zdroje jsou právě nasazeny do konzistentního stavu.

S využitím agenta protokolu analýzy jako součást šablony správce prostředků, zajistíte, že každé virtuálního počítače je předkonfigurovaná a vykazování do pracovního prostoru protokolu analýzy.

Další informace o šablonách správce prostředků najdete v článku [vytváření správce prostředků Azure šablony](../resource-group-authoring-templates.md).

Následuje příklad, který se používá pro nasazení virtuálního počítače se systémem Windows s příponou sledování programu Microsoft Agent nainstalované šablony správce prostředků. Tato šablona je šablony typické virtuálního počítače následující položky:

+ Parametry workspaceId a workspaceName
+ Skupinový rámeček rozšíření Microsoft.EnterpriseCloud.Monitoring zdroj
+ Chcete-li vyhledat workspaceId a workspaceSharedKey výstupy


```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "adminUsername": {
      "type": "string",
      "metadata": {
        "description": "Username for the Virtual Machine."
      }
    },
    "adminPassword": {
      "type": "securestring",
      "metadata": {
        "description": "Password for the Virtual Machine."
      }
    },
    "dnsLabelPrefix": {
       "type": "string",
       "metadata": {
          "description": "DNS Label for the Public IP. Must be lowercase. It should match with the following regular expression: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$ or it will raise an error."
       }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "OMS workspace ID"
      }
    },
    "workspaceName": {
      "type": "string",
      "metadata": {
         "description": "OMD workspace name"
      }
    },
    "windowsOSVersion": {
      "type": "string",
      "defaultValue": "2012-R2-Datacenter",
      "allowedValues": [
        "2008-R2-SP1",
        "2012-Datacenter",
        "2012-R2-Datacenter",
        "Windows-Server-Technical-Preview"
      ],
      "metadata": {
        "description": "The Windows version for the VM. This will pick a fully patched image of this given Windows version. Allowed values: 2008-R2-SP1, 2012-Datacenter, 2012-R2-Datacenter, Windows-Server-Technical-Preview."
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]",
    "apiVersion": "2015-06-15",
    "imagePublisher": "MicrosoftWindowsServer",
    "imageOffer": "WindowsServer",
    "OSDiskName": "osdiskforwindowssimple",
    "nicName": "myVMNic",
    "addressPrefix": "10.0.0.0/16",
    "subnetName": "Subnet",
    "subnetPrefix": "10.0.0.0/24",
    "storageAccountType": "Standard_LRS",
    "publicIPAddressName": "myPublicIP",
    "publicIPAddressType": "Dynamic",
    "vmStorageAccountContainerName": "vhds",
    "vmName": "MyWindowsVM",
    "vmSize": "Standard_DS1",
    "virtualNetworkName": "MyVNET",
    "resourceId": "[resourceGroup().id]",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "[variables('apiVersion')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "accountType": "[variables('storageAccountType')]"
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIPAddressName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
        "dnsSettings": {
          "domainNameLabel": "[parameters('dnsLabelPrefix')]"
        }
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/virtualNetworks",
      "name": "[variables('virtualNetworkName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "addressSpace": {
          "addressPrefixes": [
            "[variables('addressPrefix')]"
          ]
        },
        "subnets": [
          {
            "name": "[variables('subnetName')]",
            "properties": {
              "addressPrefix": "[variables('subnetPrefix')]"
            }
          }
        ]
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[variables('nicName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
      ],
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]"
              },
              "subnet": {
                "id": "[variables('subnetRef')]"
              }
            }
          }
        ]
      }
    },
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[variables('vmName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
      ],
      "properties": {
        "hardwareProfile": {
          "vmSize": "[variables('vmSize')]"
        },
        "osProfile": {
          "computername": "[variables('vmName')]",
          "adminUsername": "[parameters('adminUsername')]",
          "adminPassword": "[parameters('adminPassword')]"
        },
        "storageProfile": {
          "imageReference": {
            "publisher": "[variables('imagePublisher')]",
            "offer": "[variables('imageOffer')]",
            "sku": "[parameters('windowsOSVersion')]",
            "version": "latest"
          },
          "osDisk": {
            "name": "osdisk",
            "vhd": {
              "uri": "[concat('http://',variables('storageAccountName'),'.blob.core.windows.net/',variables('vmStorageAccountContainerName'),'/',variables('OSDiskName'),'.vhd')]"
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
        },
        "diagnosticsProfile": {
          "bootDiagnostics": {
             "enabled": "true",
             "storageUri": "[concat('http://',variables('storageAccountName'),'.blob.core.windows.net')]"
          }
        }
      },
      "resources": [
        {
          "type": "extensions",
          "name": "Microsoft.EnterpriseCloud.Monitoring",
          "apiVersion": "[variables('apiVersion')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
          ],
          "properties": {
            "publisher": "Microsoft.EnterpriseCloud.Monitoring",
            "type": "MicrosoftMonitoringAgent",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
              "workspaceId": "[parameters('workspaceId')]"
            },
            "protectedSettings": {
              "workspaceKey": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-03-20').primarySharedKey]"
            }
          }
        }
      ]
    }
  ],
  "outputs": {
      "sharedKeyOutput": {
         "value": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName')), '2015-03-20').primarySharedKey]",
         "type": "string"
      },
      "workspaceIdOutput": {
         "value": "[reference(concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName')), '2015-03-20').customerId]",
        "type" : "string"
      }
  }
}
```

Nasazení šablony pomocí následujícího příkazu Powershellu:

```
New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath
```

## <a name="troubleshooting-windows-virtual-machines"></a>Poradce při potížích virtuálních počítačích Windows

Pokud rozšíření *Microsoft sledování agenta* OM agent není instalaci nebo po nahlášení můžete provádět následující kroky k řešení problému.

1. Zkontrolujte, zda je nainstalovaný agenta Azure OM a jejím zprovozněním správně pomocí kroků v [článku Znalostní 2965986](https://support.microsoft.com/kb/2965986#mt1).
  + Můžete taky zkontrolovat soubor protokolu agent OM`C:\WindowsAzure\logs\WaAppAgent.log`
  + Pokud protokol neexistuje, není nainstalovaný agenta OM.
    - [Instalace Agent OM Azure na klasické VMs](../virtual-machines/virtual-machines-windows-classic-agents-and-extensions.md)
2. Potvrďte, že úkol prezenční signál rozšíření Microsoft Agent sledování běží pomocí následujících kroků:
  + Přihlaste se k virtuálního počítače
  + Otevřete Plánovač úloh a najděte `update_azureoperationalinsight_agent_heartbeat` úkolu
  + Potvrďte úkol je povolený a běží každé jednu minutu
  + Vrácení se změnami prezenční signál souboru protokolu`C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\heartbeat.log`
3. Kontrola souborů protokolu rozšíření Angličtině sledování Agent v`C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent`
3. Zajistěte, aby že virtuální počítač mohlo by umožnit spuštění skriptů Powershellu
4. Zajistěte, aby se nezměnily oprávnění C:\Windows\temp
5. Zobrazení stavu programu Microsoft Agent sledování tak, že zadáte následující okno zvýšenými oprávněními Powershellu v počítači virtuální`  (New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`
6. Kontrola souborů protokolu instalačního programu Microsoft Agent sledování v`C:\Windows\System32\config\systemprofile\AppData\Local\SCOM\Logs`

Další informace najdete v příručce [Poradce při potížích s doplňky systému Windows](../virtual-machines/virtual-machines-windows-extensions-troubleshoot.md).

## <a name="troubleshooting-linux-virtual-machines"></a>Poradce při potížích s Linux virtuálních počítačích

Pokud není rozšíření agenta OM *OMS Agent Linux* instalaci nebo vytváření sestav můžete provádět následující kroky k řešení problému.

1. Pokud je stav rozšíření *Neznámý* zkontrolujte, zda je nainstalovaný agenta Azure OM a fungovat správně podle revizí soubor protokolu agent OM`/var/log/waagent.log`
  + Pokud protokol neexistuje, není nainstalovaný agenta OM.
  - [Agent Azure OM nainstalovat Linux VMs](../virtual-machines/virtual-machines-linux-agent-user-guide.md)
2. Pro ostatní chybná stavy zkontrolujte OMS Agent pro rozšíření Linux OM protokoly soubory do `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log` a`/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log`
3. Pokud je stav rozšíření správný, ale není odesílaných data zkontrolujte OMS Agent pro Linux protokoly v`/var/opt/microsoft/omsagent/log/omsagent.log`

Další informace najdete v příručce [Poradce při potížích s rozšíření Linux](../virtual-machines/virtual-machines-linux-extensions-troubleshoot.md).


## <a name="next-steps"></a>Další kroky

+ Konfigurace [zdroje dat v protokolu analýzy](log-analytics-data-sources.md) určete protokoly a metriky shromažďovat.
+ Shromažďování dat z virtuální stroje [Přidat analýzy protokolu řešení z Galerie řešení](log-analytics-add-solutions.md).
+ [Sběr dat pomocí Azure Diagnostika](log-analytics-azure-storage.md) pro ostatní zdroje, které jsou spuštěné v Azure.

U počítačů, které nejsou v Azure můžete nainstalovat agenta protokolu analýzy pomocí metody, které jsou popsané v těchto článcích:

+ [Připojení k analýzy protokolu počítače se systémem Windows](log-analytics-windows-agents.md)
+ [Připojení k protokolu analýzy Linux počítačů](log-analytics-linux-agents.md)
