<properties
   pageTitle="Přístup a zabezpečení v Azure šablony správce prostředků | Microsoft Azure" 
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

# <a name="access-and-security-in-azure-resource-manager-templates"></a>Přístup a zabezpečení v správce prostředků Azure šablony

Aplikace umístěné v Azure pravděpodobně potřeba přístup prostřednictvím Internetu nebo VPN / Express směrování připojení s Azure. S Ukázka aplikace hudbu úložiště je na webu k dispozici na Internetu s veřejnou IP adresu. S přístupem zřídit být zabezpečená připojení k aplikaci a přístupu k prostředkům virtuálního počítače sami. Tento přístup zabezpečení je součástí skupiny zabezpečení sítě. 

Tento dokument obsahuje podrobnosti jak zabezpečené aplikace hudbu úložiště přihlašovacích údajů v šabloně správce prostředků Azure vzorku. Závislosti a jedinečné konfigurace se zvýrazní. Která zaručuje nejlepší možnosti, předem nasazení instanci řešení Azure předplatného a práce spolu s šabloně správce prostředků Azure. Dokončení šablony najdete tady – [Nasazení úložiště hudby na systémem Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).


## <a name="public-ip-address"></a>Veřejnou IP adresu

Pokud chcete, aby veřejné přístup k prostředku Azure, mohou sloužit veřejné prostředek IP adresy. Veřejnou IP adresu je možné konfigurovat pomocí statické nebo dynamické IP adresu. Pokud se používá dynamická adresa a zastaven a uvolnit virtuální počítač, adresy se odebere. Když v počítači spuštěná znovu, může přiřazen různých veřejnou IP adresu. IP adresy zabránit změnám ze strany, vyhrazené IP adresu lze. 

Veřejnou IP adresu lze přidat do šablony aplikace Správce prostředků Azure pomocí aplikace Visual Studio Průvodce přidáním nového zdroje nebo vložením platné JSON do šablony. 

Chcete-li zobrazit ukázku JSON v šabloně správce prostředků – [Veřejnou IP adresu](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L121)na tento odkaz.


```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[variables('publicipaddressName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "public-ip-front"
  },
  "properties": {
    "publicIPAllocationMethod": "Dynamic",
    "dnsSettings": {
      "domainNameLabel": "[parameters('publicipaddressDnsName')]"
    }
  }
}
```

Veřejnou IP adresu můžou být přidružené virtuální síťový adaptér nebo Vyrovnávání zatížení. V tomto příkladu vzhledem k tomu, že váš web hudbu úložiště je rozloženy do několika virtuálních počítačích, na veřejnou IP adresu přiřazen Vyrovnávání zatížení.

Chcete-li zobrazit ukázku JSON v šabloně správce prostředků – [veřejnou IP adresu přidružení Vyrovnávání zatížení](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L208)na tento odkaz.

```none
"frontendIPConfigurations": [
  {
    "properties": {
      "publicIPAddress": {
        "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicipaddressName'))]"
      }
    },
    "name": "LoadBalancerFrontend"
  }
]
```

Veřejnou IP adresu příručky z portálu Microsoft Azure. Všimněte si, že na veřejnou IP adresu přidruženou k vyrovnávání zatížení a ne virtuálního počítače. Vyrovnávání zatížení sítě jsou uvedené v další dokument řady.

![Veřejnou IP adresu](./media/virtual-machines-linux-dotnet-core/pubip.png)

Další informace na veřejnou IP adresy Azure najdete v tématu [IP adresy v Azure](../virtual-network/virtual-network-ip-addresses-overview-arm.md).

## <a name="network-security-group"></a>Skupina zabezpečení sítě

Jakmile prokázáno, přístupu k prostředkům Azure, by měl být omezený přístup. Pro Azure virtuálních počítačích zabezpečený přístup dosáhnete pomocí síťové skupiny zabezpečení. S Ukázka aplikace hudbu úložiště všechny přístup k virtuálního počítače je omezený s výjimkou přes http přístup pomocí protokolu porty 80 a port 22 SSH přístup pomocí protokolu. Skupiny zabezpečení sítě můžete přidat do šablony aplikace Správce prostředků Azure pomocí aplikace Visual Studio Průvodce přidáním nového zdroje nebo vložením platné JSON do šablony.

Chcete-li zobrazit ukázku JSON v šabloně správce prostředků – [Skupiny zabezpečení v síti](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L68)na tento odkaz.

```none
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Network/networkSecurityGroups",
  "name": "[variables('nsgfront')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "nsg-front"
  },
  "properties": {
    "securityRules": [
      {
        "name": "http",
        "properties": {
          "description": "http endpoint",
          "protocol": "Tcp",
          "sourcePortRange": "*",
          "destinationPortRange": "80",
          "sourceAddressPrefix": "*",
          "destinationAddressPrefix": "*",
          "access": "Allow",
          "priority": 100,
          "direction": "Inbound"
        }
      },
      ........<truncated> 
    ]
  }
}
```

V tomto příkladu je skupina zabezpečení sítě spojit s objekt podsítě deklarované ve zdroji virtuální sítě. 

Přejděte na tento odkaz zobrazíte JSON vzorku v šabloně správce prostředků – [skupiny zabezpečení sítě přidružení virtuální sítě](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L158).


```none
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
```

Takto vypadá síťovou skupinu zabezpečení z portálu Microsoft Azure. Všimněte si, že NSG můžete propojit s rozhraním podsítě a / nebo sítě. V tomto případě je přidružený podsítě NSG. V této konfiguraci příchozí pravidla platí pro všechny virtuálních počítačích připojené k podsítě.

![Skupina zabezpečení sítě](./media/virtual-machines-linux-dotnet-core/nsg.png)

Podrobnější informace o skupiny zabezpečení sítě v tématu [Co je skupinu zabezpečení sítě]( https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/).

## <a name="next-step"></a>Další krok

<hr>

[Krok 3 – dostupnost a velkém měřítku v Azure správce prostředků šablony](./virtual-machines-linux-dotnet-core-4-availability-scale.md)
