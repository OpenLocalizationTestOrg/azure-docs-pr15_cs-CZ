<properties
   pageTitle="Správce prostředků šablony návodu | Microsoft Azure"
   description="Návod krok za krokem šablony správce zdrojů zřizování Základní architektura Azure IaaS."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="navalev"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/04/2016"
   ms.author="navale;tomfitz"/>
   
# <a name="resource-manager-template-walkthrough"></a>Správce prostředků šablony návod

Jednou z první otázky při vytváření šablony je "jak spustit?". Můžete začít z prázdné šablony, sledovat základní struktura popsaná v [článku vytváření šablony](resource-group-authoring-templates.md#template-format)a přidat zdroje a příslušné parametry a proměnných. Dobrý alternativní bude začněte tím, že přejdete do [Galerie rychlý úvod](https://github.com/Azure/azure-quickstart-templates) a vypadat podobně jako scénářích té, kterou chcete vytvořit. Můžete sloučit několik šablon nebo upravte existující podle určitých nefunguje. 

Podívejme se na běžné infrastruktury:

* Dva virtuálních počítačích, které používají stejný účet úložiště, ve stejnou sadu dostupnost a ve stejné podsítě virtuální sítě.
* Jednu NIC a OM IP adresu každého virtuálního počítače.
* Vyrovnávání zatížení při pravidlo pro port 80 Vyrovnávání zatížení

![Architektura](./media/resource-group-overview/arm_arch.png)

Toto téma vás provede jednotlivými kroky vytvoření šablony správce prostředků pro dané infrastruktury. Konečný šablonu, kterou vytvoříte je založené na šabloně rychlý úvod s názvem [2 VMs ve schůzce služby Vyrovnávání zatížení a pravidla pro vyrovnávání zatížení](https://azure.microsoft.com/documentation/templates/201-2-vms-loadbalancer-lbrules/).

Ale, jestli je hodně vytvořit v celém dokumentu, nejprve tak se Pojďme vytvořit účet úložiště a nasazení. Až budete mít standardní vytvoření účtu úložiště, přidáte další zdroje a znovu nasazení šablony dokončete infrastrukturu.

>[AZURE.NOTE] Libovolný typ editor můžete použít při vytváření šablony. Visual Studio poskytuje nástroje, které zjednodušení vývoje šablony, ale nepotřebujete Visual Studio tento kurz. Návod k použití aplikace Visual Studio vytvořit Web App a databáze SQL nasazení najdete v článku [Vytvoření a nasazení Azure zdroje skupiny pomocí aplikace Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md). 

## <a name="create-the-resource-manager-template"></a>Vytvoření šablony správce prostředků

Šablona je JSON soubor, který definuje všechny zdroje, které budete nasazovat. Také umožňuje definovat parametry určených při nasazení proměnných, které vytvořen z jiných hodnot a výrazy a výstupy z nasazení. 

Začněme nejjednodušší šablony:

```json
    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {  },
      "variables": {  },
      "resources": [  ],
      "outputs": {  }
    }
 ```

Uložení souboru jako **azuredeploy.json** (Všimněte si, že šablona může mít libovolnou jméno, které chcete jenom tato musí být soubor json).

## <a name="create-a-storage-account"></a>Vytvoření účtu úložiště
V části **zdroje** přidejte objekt, který definuje účtu úložiště, jak je ukázáno v následujícím příkladu. 

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[parameters('storageAccountName')]",
    "apiVersion": "2015-06-15",
    "location": "[resourceGroup().location]",
    "properties": {
      "accountType": "Standard_LRS"
    }
  }
]
```

Budete možná vás zajímá, odkud pocházejí z těchto vlastností a hodnot. Vlastnosti **Typ**, **název**, **apiVersion**a **umístění** jsou standardní prvky, které jsou dostupné pro všechny typy zdrojů. Se dozvíte o společných prvků na [zdroje](resource-group-authoring-templates.md#resources). **název** nastavena na hodnotu parametru, který předáváte ve při nasazení a **umístění** jako umístění použité podle skupiny zdrojů. Podíváme na tom, jak určit **Typ** a **apiVersion** v následujících částech.

V části **Vlastnosti** obsahuje všechny vlastnosti, které jsou jedinečné pro typ určitého zdroje. Hodnoty, které zadáte v této části, přesně shodovat se operace umístění v rozhraní REST API pro vytváření tento typ zdroje. Při vytváření účtu úložiště, je nutné zadat **Typ účtu**. Všimněte si v [Rozhraní REST API pro vytvoření účtu úložiště](https://msdn.microsoft.com/library/azure/mt163564.aspx) části Vlastnosti ZBÝVAJÍCÍ práce obsahuje také vlastnosti **Typ účtu** , a jsou popsány povolené hodnoty. V tomto příkladu typ účtu nastavená na **Standard_LRS**, ale můžete zadat jinou hodnotu nebo povolení uživatelům předat typ účtu jako parametr.

Teď Pojďme Přejít zpátky do oddílu **Parametry** a najdete v článku jak definovat název účtu úložiště. Další informace o použití parametrů v [Parametry](resource-group-authoring-templates.md#parameters). 

```json
"parameters" : {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Storage Account Name"
      }
    }
}
```
Sem jste definovali parametru typu řetězec, který bude obsahovat název účtu úložiště. Při nasazení šablony se zadána hodnota pro tento parametr.

## <a name="deploying-the-template"></a>Nasazení šablony
Máme úplné šablony pro vytvoření nového účtu úložiště. Jako odvolat, šablona se uloží do **azuredeploy.json** soubor:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters" : {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Storage Account Name"
      }
    }
  },  
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[parameters('storageAccountName')]",
      "apiVersion": "2015-06-15",
      "location": "[resourceGroup().location]",
      "properties": {
        "accountType": "Standard_LRS"
      }
    }
  ]
}
```

Způsoby několik nasazení šablony, jak je vidět v následujícím [článku nasazení zdroje](resource-group-template-deploy.md). Nasazení šablony pomocí prostředí PowerShell Azure, můžete:

```powershell
# create a new resource group
New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West Europe"

# deploy the template to the resource group
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile azuredeploy.json
```

Nebo nasadit šablony pomocí rozhraní příkazového řádku Azure, můžete:

```
azure group create -n ExampleResourceGroup -l "West Europe"

azure group deployment create -f azuredeploy.json -g ExampleResourceGroup -n ExampleDeployment
```

Měli byste být hrdí majitele účtu úložiště!

Další kroky budou přidávat všechny materiály potřebné k nasazení architektura popsané na začátku tohoto kurzu. Přidejte tyto materiály ve stejném šablonu, kterou jste pracovali.

## <a name="availability-set"></a>Nastavení dostupnosti
Po definici účtu úložiště přidáte dostupnosti nastavovat pro virtuálních počítačích. V tomto případě nejsou žádné další vlastnosti potřeby tak její definice je velmi jednoduché. V případě, že chcete definovat aktualizace domény počet a poruch počet hodnoty domény, najdete v článku [Rozhraní REST API pro vytváření dostupnost nastavení](https://msdn.microsoft.com/library/azure/mt163607.aspx) pro části úplné vlastnosti.

```json
{
   "type": "Microsoft.Compute/availabilitySets",
   "name": "[variables('availabilitySetName')]",
   "apiVersion": "2015-06-15",
   "location": "[resourceGroup().location]",
   "properties": {}
}
```

Všimněte si, že **název** je nastavený na hodnotu do proměnné. Pro tuto šablonu název sady dostupnost potřebné v několika různých míst. Definování daná hodnota jednou a použitím na několika místech snadněji spravovat vaši šablonu.

Hodnota, kterou zadáte pro **Typ** obsahuje zprostředkovatele prostředků a typ zdroje. Dostupnost sady poskytovatele zdroje je **Microsoft.Compute** a typ zdroje je **availabilitySets**. Získání seznamu poskytovatelů dostupné zdroje spuštěním následujícího příkazu Powershellu:

```powershell
    Get-AzureRmResourceProvider -ListAvailable
```

Nebo pokud používáte Azure rozhraní příkazového řádku, můžete spusťte tento příkaz:
```
    azure provider list
```
Vzhledem k tomu, že v tomto tématu vytváříte s účty úložiště, virtuálních počítačích a virtuální sítě, budou fungovat s:

- Microsoft.Storage
- Microsoft.Compute
- Microsoft.Network

Zobrazíte typy prostředků pro určitého zprostředkovatele, spusťte tento příkaz Powershellu:

```powershell
    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute).ResourceTypes
```

Nebo pro rozhraní příkazového řádku Azure, bude tento příkaz vrátí dostupných typů formátu JSON a si ji uložit do souboru.

```
    azure provider show Microsoft.Compute --json > c:\temp.json
```

Měli byste vidět **availabilitySets** jako jeden z typů v rámci **Microsoft.Compute**. Úplný název typu je **Microsoft.Compute/availabilitySets**. Můžete určit, název typu zdroje pro některý ze zdrojů je šablony.

## <a name="public-ip"></a>Veřejné IP
Definování veřejnou IP adresu. Akci podívejte se na [Rozhraní REST API pro veřejnou IP adres](https://msdn.microsoft.com/library/azure/mt163590.aspx) pro vlastnosti, které chcete nastavit.

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[parameters('publicIPAddressName')]",
  "location": "[resourceGroup().location]",
  "properties": {
    "publicIPAllocationMethod": "Dynamic",
    "dnsSettings": {
      "domainNameLabel": "[parameters('dnsNameforLBIP')]"
    }
  }
}
```

Metoda rozdělení je nastavena na **dynamické** , ale můžete ho nastavit na hodnotu, budete potřebovat nebo nastavit tak, aby přijmout hodnotu parametru. Povolili jste uživatelům šablony pro předávání v hodnotě popisku název domény.

Teď Pojďme se podívat na tom, jak určit **apiVersion**. Zadanou hodnotu jednoduše shoduje s verzí rozhraní REST API, který chcete použít při vytváření zdroje. Ano můžete si prohlédnout dokumentace rozhraní REST API pro tento typ zdroje. Nebo spuštěním příkazu Powershellu pro určitý typ.

```powershell
    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Network).ResourceTypes | Where-Object ResourceTypeName -eq publicIPAddresses).ApiVersions
```
Které vrátí následující hodnoty:

    2015-06-15
    2015-05-01-preview
    2014-12-01-preview

Rozhraní API verze s Azure rozhraní příkazového řádku zobrazíte příkaz stejné **azure poskytovatele zobrazit** zobrazené dříve.

Při vytváření nové šablony, vyberte nejnovější verze rozhraní API.

## <a name="virtual-network-and-subnet"></a>Virtuální sítě a podsítě
Vytvořte virtuální sítě s jednou podsítí. Podívejte se na [Rozhraní REST API pro virtuální sítě](https://msdn.microsoft.com/library/azure/mt163661.aspx) pro všechny vlastnosti chcete nastavit.

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Network/virtualNetworks",
   "name": "[parameters('vnetName')]",
   "location": "[resourceGroup().location]",
   "properties": {
     "addressSpace": {
       "addressPrefixes": [
         "10.0.0.0/16"
       ]
     },
     "subnets": [
       {
         "name": "[variables('subnetName')]",
         "properties": {
           "addressPrefix": "10.0.0.0/24"
         }
       }
     ]
   }
}
```

## <a name="load-balancer"></a>Vyrovnávání zatížení
Teď vytvoříte externí vystaveného Vyrovnávání zatížení. Protože tento Vyrovnávání zatížení používá veřejnou IP adresu, třeba deklarovat závislost na veřejnou IP adresu v části **dependsOn** . To znamená, že vyrovnávání zatížení nebude nenasadí nedokončili veřejnou IP adresu má nasazení. Bez definování tuto závislost, zobrazí se chyba protože správce prostředků pokusí prostředky paralelně nasadit a pokusí se nastavte Vyrovnávání zatížení na veřejnou IP adresu, který ještě neexistuje. 

Vytvoříte také fond back-end adresu, několik příchozí pravidla překladu síťových adres RDP do VMs a pravidlo Vyrovnávání zatížení s tcp zkušební na portu 80 v této definici zdroje. Rezervace [Rozhraní REST API pro vyrovnávání zatížení](https://msdn.microsoft.com/library/azure/mt163574.aspx) pro všechny vlastnosti.

```json
{
   "apiVersion": "2015-06-15",
   "name": "[parameters('lbName')]",
   "type": "Microsoft.Network/loadBalancers",
   "location": "[resourceGroup().location]",
   "dependsOn": [
     "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]"
   ],
   "properties": {
     "frontendIPConfigurations": [
       {
         "name": "LoadBalancerFrontEnd",
         "properties": {
           "publicIPAddress": {
             "id": "[variables('publicIPAddressID')]"
           }
         }
       }
     ],
     "backendAddressPools": [
       {
         "name": "BackendPool1"
       }
     ],
     "inboundNatRules": [
       {
         "name": "RDP-VM0",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "protocol": "tcp",
           "frontendPort": 50001,
           "backendPort": 3389,
           "enableFloatingIP": false
         }
       },
       {
         "name": "RDP-VM1",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "protocol": "tcp",
           "frontendPort": 50002,
           "backendPort": 3389,
           "enableFloatingIP": false
         }
       }
     ],
     "loadBalancingRules": [
       {
         "name": "LBRule",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "backendAddressPool": {
             "id": "[variables('lbPoolID')]"
           },
           "protocol": "tcp",
           "frontendPort": 80,
           "backendPort": 80,
           "enableFloatingIP": false,
           "idleTimeoutInMinutes": 5,
           "probe": {
             "id": "[variables('lbProbeID')]"
           }
         }
       }
     ],
     "probes": [
       {
         "name": "tcpProbe",
         "properties": {
           "protocol": "tcp",
           "port": 80,
           "intervalInSeconds": 5,
           "numberOfProbes": 2
         }
       }
     ]
   }
}
```

## <a name="network-interface"></a>Rozhraní sítě
Vytvoříte 2 síťová rozhraní, jedno pro každé OM. Namísto zahrnout duplicitní položky pro síťová rozhraní, můžete pomocí [copyIndex() funkce](resource-group-create-multiple.md) iteraci smyčka zkopírovat (jako takzvaný nicLoop) a vytvoření číselné síťová rozhraní podle `numberOfInstances` proměnné. Rozhraní sítě závisí na vytváření virtuálních sítí a vyrovnávání zatížení. Podsítě podle vytváření virtuálních sítí a id Vyrovnávání zatížení používá ke konfiguraci fondu adres Vyrovnávání zatížení a příchozí pravidla překladu síťových adres.
Podívejte se na [Rozhraní REST API pro síťová rozhraní](https://msdn.microsoft.com/library/azure/mt163668.aspx) pro všechny vlastnosti.

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Network/networkInterfaces",
   "name": "[concat(parameters('nicNamePrefix'), copyindex())]",
   "location": "[resourceGroup().location]",
   "copy": {
     "name": "nicLoop",
     "count": "[variables('numberOfInstances')]"
   },
   "dependsOn": [
     "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]",
     "[concat('Microsoft.Network/loadBalancers/', parameters('lbName'))]"
   ],
   "properties": {
     "ipConfigurations": [
       {
         "name": "ipconfig1",
         "properties": {
           "privateIPAllocationMethod": "Dynamic",
           "subnet": {
             "id": "[variables('subnetRef')]"
           },
           "loadBalancerBackendAddressPools": [
             {
               "id": "[concat(variables('lbID'), '/backendAddressPools/BackendPool1')]"
             }
           ],
           "loadBalancerInboundNatRules": [
             {
               "id": "[concat(variables('lbID'),'/inboundNatRules/RDP-VM', copyindex())]"
             }
           ]
         }
       }
     ]
   }
}
```

## <a name="virtual-machine"></a>Virtuální počítač
Vytvoříte 2 virtuálních počítačích funkcí copyIndex(), stejně jako při vytváření [rozhraní sítě](#network-interface).
Vytvoření OM závisí na účtu úložiště sítě rozhraní a dostupnosti sad. Tento OM se vytvoří z marketplace obrázku, jak je definováno v `storageProfile` vlastnost - `imageReference` slouží k definování obrázek Publisheru, nabídky, sku a verze. Nakonec je diagnostiky profil nakonfigurován pro povolení diagnostických nástrojů pro OM. 

Najít odpovídající vlastnosti obrázku marketplace, postupujte podle článků [Vyberte Linux virtuálního počítače obrázky](./virtual-machines/virtual-machines-linux-cli-ps-findimage.md) nebo [obrázky virtuálního počítače Windows](./virtual-machines/virtual-machines-windows-cli-ps-findimage.md) .

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Compute/virtualMachines",
   "name": "[concat(parameters('vmNamePrefix'), copyindex())]",
   "copy": {
     "name": "virtualMachineLoop",
     "count": "[variables('numberOfInstances')]"
   },
   "location": "[resourceGroup().location]",
   "dependsOn": [
     "[concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName'))]",
     "[concat('Microsoft.Network/networkInterfaces/', parameters('nicNamePrefix'), copyindex())]",
     "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]"
   ],
   "properties": {
     "availabilitySet": {
       "id": "[resourceId('Microsoft.Compute/availabilitySets',variables('availabilitySetName'))]"
     },
     "hardwareProfile": {
       "vmSize": "[parameters('vmSize')]"
     },
     "osProfile": {
       "computerName": "[concat(parameters('vmNamePrefix'), copyIndex())]",
       "adminUsername": "[parameters('adminUsername')]",
       "adminPassword": "[parameters('adminPassword')]"
     },
     "storageProfile": {
       "imageReference": {
         "publisher": "[parameters('imagePublisher')]",
         "offer": "[parameters('imageOffer')]",
         "sku": "[parameters('imageSKU')]",
         "version": "latest"
       },
       "osDisk": {
         "name": "osdisk",
         "vhd": {
           "uri": "[concat('http://',parameters('storageAccountName'),'.blob.core.windows.net/vhds/','osdisk', copyindex(), '.vhd')]"
         },
         "caching": "ReadWrite",
         "createOption": "FromImage"
       }
     },
     "networkProfile": {
       "networkInterfaces": [
         {
           "id": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'),copyindex()))]"
         }
       ]
     },
     "diagnosticsProfile": {
       "bootDiagnostics": {
          "enabled": "true",
          "storageUri": "[concat('http://',parameters('storageAccountName'),'.blob.core.windows.net')]"
       }
     }
}
```

>[AZURE.NOTE] Pro obrázky s zveřejněné **dodavatelů 3**, budete muset zadat jinou vlastnost s názvem `plan`. Příklad najdete v [tuto šablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/checkpoint-single-nic) z Galerie rychlý úvod. 

Dokončení definování zdroje pro vaši šablonu.

## <a name="parameters"></a>Parametry

V části Parametry definujte hodnoty, které lze zadat při nasazení šablony. Pouze definice parametrů pro hodnoty, které by podle vás by se měla měnit během nasazení. Zadání výchozí hodnoty pro parametr, který se používá, pokud jeden není k dispozici při nasazení. Můžete taky definujete povolené hodnoty viz parametru **imageSKU** .

```json
"parameters": {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of storage account"
      }
    },
    "adminUsername": {
      "type": "string",
      "metadata": {
        "description": "Admin username"
      }
    },
    "adminPassword": {
      "type": "securestring",
      "metadata": {
        "description": "Admin password"
      }
    },
    "dnsNameforLBIP": {
      "type": "string",
      "metadata": {
        "description": "DNS for Load Balancer IP"
      }
    },
    "vmNamePrefix": {
      "type": "string",
      "defaultValue": "myVM",
      "metadata": {
        "description": "Prefix to use for VM names"
      }
    },
    "imagePublisher": {
      "type": "string",
      "defaultValue": "MicrosoftWindowsServer",
      "metadata": {
        "description": "Image Publisher"
      }
    },
    "imageOffer": {
      "type": "string",
      "defaultValue": "WindowsServer",
      "metadata": {
        "description": "Image Offer"
      }
    },
    "imageSKU": {
      "type": "string",
      "defaultValue": "2012-R2-Datacenter",
      "allowedValues": [
        "2008-R2-SP1",
        "2012-Datacenter",
        "2012-R2-Datacenter"
      ],
      "metadata": {
        "description": "Image SKU"
      }
    },
    "lbName": {
      "type": "string",
      "defaultValue": "myLB",
      "metadata": {
        "description": "Load Balancer name"
      }
    },
    "nicNamePrefix": {
      "type": "string",
      "defaultValue": "nic",
      "metadata": {
        "description": "Network Interface name prefix"
      }
    },
    "publicIPAddressName": {
      "type": "string",
      "defaultValue": "myPublicIP",
      "metadata": {
        "description": "Public IP Name"
      }
    },
    "vnetName": {
      "type": "string",
      "defaultValue": "myVNET",
      "metadata": {
        "description": "VNET name"
      }
    },
    "vmSize": {
      "type": "string",
      "defaultValue": "Standard_D1",
      "metadata": {
        "description": "Size of the VM"
      }
    }
  }
```

## <a name="variables"></a>Proměnné

V části proměnných můžete definovat hodnoty, které se používají víc než jedno místo v šabloně nebo hodnoty, které jsou vytvořen z jiných výrazy nebo proměnných. Proměnné se často používají zjednodušit syntaxe vaši šablonu.

```json
"variables": {
    "availabilitySetName": "myAvSet",
    "subnetName": "Subnet-1",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('vnetName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables ('subnetName'))]",
    "publicIPAddressID": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]",
    "numberOfInstances": 2,
    "lbID": "[resourceId('Microsoft.Network/loadBalancers',parameters('lbName'))]",
    "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/LoadBalancerFrontEnd')]",
    "lbPoolID": "[concat(variables('lbID'),'/backendAddressPools/BackendPool1')]",
    "lbProbeID": "[concat(variables('lbID'),'/probes/tcpProbe')]"
  }
```

Jste dokončili šabloně! Je možné porovnávat šablony proti úplné šablony v [Galerii rychlý úvod](https://github.com/Azure/azure-quickstart-templates) v části [2 VMs s služba Vyrovnávání zatížení a načíst vyrovnávání pravidla šablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules). Šablona se může lišit mírně podle pomocí různých verzí čísel. 

Šablona znovu nasadíte pomocí stejného příkazy, které jste použili při nasazení účtu úložiště. Odstranění účtu úložiště než znovu nasadíte protože správce prostředků přeskočí znovu vytváření prostředky, které už existuje a nezměnily nepotřebujete.

## <a name="next-steps"></a>Další kroky

- [Azure správce prostředků šablony Visualizer (ARMViz)](http://armviz.io/#/) je skvělým nástrojem vizualizovat ARM šablony, jak bude může se jednat o pochopit jen z čtení json souboru je příliš velký.
- Další informace o struktuře šablony, najdete v článku [vytváření správce prostředků Azure šablony](resource-group-authoring-templates.md).
- Další informace o nasazení šablony, najdete v článku [nasazení skupina zdroje šablonou správce prostředků Azure](resource-group-template-deploy.md)
