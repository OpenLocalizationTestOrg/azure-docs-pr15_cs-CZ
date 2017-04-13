<properties
   pageTitle="Nasazení výpočet zdroje s Azure šablony správce prostředků | Microsoft Azure"
   description="Kurz DotNet Core Azure virtuálního počítače"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/21/2016"
   ms.author="nepeters"/>

# <a name="application-architecture-with-azure-resource-manager-templates"></a>Architektura aplikací pomocí šablon správce prostředků Azure

Při vytváření nasazení Správce prostředků Azure, výpočetních požadavků muset namapovat na Azure zdrojů a služeb. Pokud aplikace obsahuje několik koncové body http, databáze a data ukládání do mezipaměti služby, Azure prostředky, které hostují jednotlivých součástí musí být rationalized. Například ukázková hudbu úložiště aplikace obsahuje webovou aplikaci, která je hostovaný ve počítače virtuální a databázi SQL, který je hostovaný v databázi Azure SQL. 

Tento dokument obsahuje podrobnosti konfiguraci zdroje výpočetním hudbu úložiště v šabloně správce prostředků Azure vzorku. Závislosti a jedinečné konfigurace se zvýrazní. Která zaručuje nejlepší možnosti, předem nasazení instanci řešení Azure předplatného a práce spolu s šabloně správce prostředků Azure. Dokončení šablony najdete tady – [Nasazení úložiště hudby na systémem Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

## <a name="virtual-machine"></a>Virtuální počítač

Aplikace hudbu úložiště přihlašovacích údajů obsahuje webovou aplikaci místo, kam můžete prohlížet a nakupovat hudbu zákazníky. I když jsou několika Azure služeb, které můžete hostovat webových aplikací, například virtuální počítač používá. Pomocí šablony hudbu úložiště ukázkové nasazený virtuálního počítače, na webový server instalaci a hudbu úložiště webu nainstalovali a nakonfigurovali. Za účelem Tento článek je podrobné pouze nasazení virtuálního počítače. Konfigurace nastavení webového serveru a aplikace je podrobné ve znalostní bázi novější.

Virtuální počítač lze přidat do šablony jiný plán pomocí průvodce Visual Studio, přidání nového prostředku nebo vložením platné JSON do nasazení šablony. Když nasadíte virtuálního počítače, jsou taky potřebné několik související materiály. Pokud chcete vytvořit šablonu pomocí aplikace Visual Studio, jsou vytvoří tyto materiály. Pokud ručně stavba šabloně, tyto materiály muset vložená a nakonfigurovali.

Chcete-li zobrazit ukázku JSON v šabloně správce prostředků – [JSON virtuálního počítače](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L295)na tento odkaz.

```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/virtualMachines",
  "name": "[concat(variables('vmName'),copyindex())]",
  "location": "[resourceGroup().location]",
  "copy": {
    "name": "virtualMachineLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "tags": {
    "displayName": "virtual-machine"
  },
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('vhdStorageName'))]",
    "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]",
    "nicLoop"
  ],
  "properties": {
    "availabilitySet": {
      "id": "[resourceId('Microsoft.Compute/availabilitySets', variables('availabilitySetName'))]"
    },
      ........<truncated>  
    }
```

Po zavedení, uvidí vlastnosti virtuálního počítače na portálu Azure.

![Virtuální počítač](./media/virtual-machines-linux-dotnet-core/vm.png)

## <a name="storage-account"></a>Úložiště účtu

Účty úložiště nabízí mnoho možností ukládání a možnosti. V kontextu Azure virtuálních počítačích obsahuje účet úložiště virtuální pevných discích virtuálního počítače a jakékoli další data disků. Ukázka hudbu úložiště obsahuje jednoho účtu úložiště pro uložení virtuálního pevného disku virtuální počítače v nasazení. 

Chcete-li zobrazit ukázku JSON v šabloně správce prostředků – [Účtu úložiště](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L109)na tento odkaz.


```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[variables('vhdStorageName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "storage-account"
  },
  "properties": {
    "accountType": "[variables('vhdStorageType')]"
  }
}
```

Účet úložiště je spojit s virtuálního počítače uvnitř deklaraci šablony správce prostředků virtuálního počítače. 

Tento odkaz zobrazíte JSON vzorku v šabloně správce prostředků – [přidružení virtuální počítač a úložiště účtu](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L341).

```none
"osDisk": {
  "name": "osdisk",
  "vhd": {
    "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/',variables('vhdStorageName')), '2015-06-15').primaryEndpoints.blob,'vhds/osdisk', copyindex(), '.vhd')]"
  },
  "caching": "ReadWrite",
  "createOption": "FromImage"
}
```

Po nasazení účtu úložiště uvidí na portálu Azure.

![Úložiště účtu](./media/virtual-machines-linux-dotnet-core/storacct.png)

Kliknutím do kontejneru úložiště objektů blob účtu virtuálního pevného disku souboru pro každý počítač virtuální nasazených pomocí šablony uvidí.

![Virtuální pevných discích](./media/virtual-machines-linux-dotnet-core/vhd.png)

Další informace o úložišti Azure dokumentaci k [Úložišti Azure](https://azure.microsoft.com/documentation/services/storage/).

## <a name="virtual-network"></a>Virtuální sítě

Pokud virtuální počítač vyžaduje vnitřní síti například možnost komunikovat s jinými virtuálních počítačích a Azure zdrojů, požaduje virtuální sítě Azure.  Virtuální sítě není zpřístupnit virtuálního počítače přes internet. Připojení k veřejné službě vyžaduje veřejné IP adresu, která později naleznete v této řadě.

Chcete-li zobrazit ukázku JSON v šabloně správce prostředků – [virtuální sítě a podsítí](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L136)na tento odkaz.

```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/virtualNetworks",
  "name": "[variables('virtualNetworkName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Network/networkSecurityGroups/', variables('networkSecurityGroup'))]"
  ],
  "tags": {
    "displayName": "virtual-network"
  },
  "properties": {
    "addressSpace": {
      "addressPrefixes": [
        "10.0.0.0/24"
      ]
    },
    "subnets": [
      {
        "name": "[variables('subnetName')]",
        "properties": {
          "addressPrefix": "10.0.0.0/24",
          "networkSecurityGroup": {
            "id": "[resourceId('Microsoft.Network/networkSecurityGroups', variables('networkSecurityGroup'))]"
          }
        }
      }
    ]
  }
}
```

Z portálu Azure virtuální sítě vypadá jako na následujícím obrázku. Všimněte si, že všechny virtuálních počítačích nasazených pomocí šablony jsou připojené k virtuální sítě.

![Virtuální sítě](./media/virtual-machines-linux-dotnet-core/vnet.png)

## <a name="network-interface"></a>Rozhraní sítě

 Rozhraní sítě připojí virtuálního počítače k síti virtuální konkrétně k podsítě definovaném v virtuální sítě. 
 
 Chcete-li zobrazit ukázku JSON v šabloně správce prostředků – [Síťové](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L166)na tento odkaz.
 
```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/networkInterfaces",
  "name": "[concat(variables('networkInterfaceNamePrefix'), copyindex())]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "network-interface"
  },
  "copy": {
    "name": "nicLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "dependsOn": [
    "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
    "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIpAddressName'))]",
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'), '/inboundNatRules/', 'SSH-VM', copyIndex())]"
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
              "id": "[variables('lbPoolID')]"
            }
          ],
          "loadBalancerInboundNatRules": [
            {
              "id": "[concat(variables('lbID'),'/inboundNatRules/SSH-VM', copyIndex())]"
            }
          ]
        }
      }
    ]
  }
}
```

Jednotlivé zdroje virtuální počítač zahrnuje profil sítě. Rozhraní sítě je přidružený virtuálního počítače v tomto profilu.  

Chcete-li zobrazit ukázku JSON v šabloně správce prostředků – [Profil sítě virtuálního počítače](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L350)na tento odkaz.


```none
"networkProfile": {
  "networkInterfaces": [
    {
      "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('networkInterfaceNamePrefix'), copyindex()))]"
    }
  ]
}
```

Z portálu Azure rozhraní sítě vypadá jako na následujícím obrázku. Vnitřní IP adresa a přidružení virtuálního počítače se vejde na zdroje rozhraní sítě.

![Rozhraní sítě](./media/virtual-machines-linux-dotnet-core/nic.png)

Další informace o Azure virtuálních sítí najdete v článku [si přečtěte následující dokumentaci Azure virtuální sítě](https://azure.microsoft.com/documentation/services/virtual-network/).

## <a name="azure-sql-database"></a>Databáze Azure SQL

Kromě virtuálního počítače hostingu webu úložiště hudbu databáze SQL Azure používaný hostovat databáze hudbu úložiště. Výhodou používání databáze SQL Azure tady je, že nepožaduje druhý množiny virtuálních počítačích a měřítko a dostupnosti je zabudované do služby.

Databáze Azure SQL můžete přidat pomocí aplikace Visual Studio přidat nový prostředek průvodce, nebo vložením platné JSON do šablony. SQL Server zdroje obsahuje uživatelské jméno a heslo, které je přidělit oprávnění správce na instanci systému SQL. Také je přidání zdroje brány firewall SQL. Ve výchozím nastavení aplikace umístěné v Azure se spojit s instanci systému SQL. Pokud chcete povolit externí aplikaci takové SQL Server Management studio se připojit k instanci systému SQL, bránu firewall vyžaduje konfiguraci. Výchozí konfigurace za účelem ukázku hudbu úložiště je v pořádku. 

Chcete-li zobrazit ukázku JSON v šabloně správce prostředků – [Databáze SQL Azure](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L401)na tento odkaz.


```none
{
  "apiVersion": "2014-04-01-preview",
  "type": "Microsoft.Sql/servers",
  "name": "[variables('musicStoreSqlName')]",
  "location": "[resourceGroup().location]",

  "dependsOn": [],
  "tags": {
    "displayName": "sql-music-store"
  },
  "properties": {
    "administratorLogin": "[parameters('adminUsername')]",
    "administratorLoginPassword": "[parameters('sqlAdminPassword')]"
  },
  "resources": [
    {
      "apiVersion": "2014-04-01-preview",
      "type": "firewallrules",
      "name": "firewall-allow-azure",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Sql/servers/', variables('musicStoreSqlName'))]"
      ],
      "properties": {
        "startIpAddress": "0.0.0.0",
        "endIpAddress": "0.0.0.0"
      }
    }
  ]
}
```

Zobrazení SQL server a databázi MusicStore zobrazené v portálu Azure.

![SQL Server](./media/virtual-machines-linux-dotnet-core/sql.png)

Další informace o nasazení databáze SQL Azure najdete v tématu [Dokumentace databáze SQL Azure](https://azure.microsoft.com/documentation/services/sql-database/).

## <a name="next-step"></a>Další krok

<hr>

[Krok 2: přístup a zabezpečení v Azure správce prostředků šablony](./virtual-machines-linux-dotnet-core-3-access-security.md)
