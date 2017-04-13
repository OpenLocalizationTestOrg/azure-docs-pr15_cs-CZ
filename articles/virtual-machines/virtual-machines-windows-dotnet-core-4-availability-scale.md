<properties
   pageTitle="Dostupnost a velkém měřítku v Azure šablony správce prostředků | Microsoft Azure"
   description="Kurz DotNet Core Azure virtuálního počítače"
   services="virtual-machines-windows"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/21/2016"
   ms.author="nepeters"/>

# <a name="availability-and-scale-in-azure-resource-manager-templates"></a>Dostupnost a velkém měřítku v správce prostředků Azure šablony

Dostupnost a měřítka v nápovědě k provozu a možnost zahájit služba. Pokud aplikace, musí být 99,9 % čas, musí mít architektura, která umožňuje více souběžné výpočetním zdrojů. Například namísto jeden web konfigurace s do vyšší úrovně dostupnost zahrnuje víc instancí stejné síti s technologií před jejich vyrovnávání. V této konfiguraci jednou instancí aplikace možné na údržbu, zatímco zbývající nadále pracovat. Měřítko na druhou stranu odkazuje na možnost aplikace a bude předávat služba. Se zatížením vyrovnané použití, přidávání a odebírání instance z fondu umožňuje aplikaci zobrazit podle služba.

Tento dokument obsahuje podrobnosti konfiguraci nasazení ukázkové hudbu úložiště dostupnosti a měřítko. Závislosti a jedinečné konfigurace se zvýrazní. Která zaručuje nejlepší možnosti, předem nasazení instancí řešení Azure předplatného a práce spolu s šabloně správce prostředků Azure. Dokončení šablony najdete tady – [Nasazení hudbu úložiště přihlašovacích údajů v systému Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="availability-set"></a>Nastavení dostupnosti

Dostupnost nastavit logicky zahrnuje Azure virtuálních počítačích fyzické hosts a dalších infrastruktury součástí například napájecí a fyzických sociální sítě. Dostupnost sady zajistit, že během údržby, selhání zařízení nebo jiné dolů čas, ne všechny virtuálních počítačích, budou provedeny. Dostupnost nastavit přidáním šablonu aplikace Správce prostředků Azure pomocí aplikace Visual Studio Průvodce přidáním nového zdroje nebo vložení platné JSON do šablony.

Chcete-li zobrazit ukázku JSON v šabloně správce prostředků – [Nastavte dostupnost](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L368)na tento odkaz.


```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/availabilitySets",
  "name": "[variables('availabilitySetName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "availability-set"
  },
  "properties": {
  }
}
```

Dostupnost nastavit deklarovány jako vlastnost Zdroj virtuálního počítače. 

Chcete-li zobrazit ukázku JSON v šabloně správce prostředků – [Nastavte dostupnost přidružení virtuálního počítače](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L302)na tento odkaz.

```none
"properties": {
  "availabilitySet": {
    "id": "[resourceId('Microsoft.Compute/availabilitySets', variables('availabilitySetName'))]"
  }
```
Dostupnost nastavte, jak je vidět z portálu Microsoft Azure. Tady jsou podrobné každé virtuálního počítače a podrobnosti o konfiguraci.

![Nastavení dostupnosti](./media/virtual-machines-windows-dotnet-core/ase-win.png)

Podrobnější informace o dostupnosti sady najdete v článku [Správa dostupnosti virtuálních počítačích](./virtual-machines-windows-manage-availability.md). 

## <a name="network-load-balancer"></a>Vyrovnávání zatížení sítě

Vzhledem k tomu sady dostupnosti aplikace odolnost proti chybám, Vyrovnávání zatížení zpřístupní mnoho instancí aplikace na adrese jedné sítě. Více instancí aplikace může být umístěny na mnoha virtuálních počítačích, každý z nich připojené k vyrovnávání zatížení. Při přístupu k aplikaci Vyrovnávání zatížení směruje příchozí žádosti mezi připojených členy. Vyrovnávání zatížení může být přidán pomocí aplikace Visual Studio Průvodce přidáním nového zdroje nebo vložením správně formátované JSON zdrojů do šablony správce prostředků Azure.

Přejděte na tento odkaz zobrazíte JSON vzorku v šabloně správce prostředků – [Vyrovnávání zatížení sítě](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L198).

```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/loadBalancers",
  "name": "[variables('loadBalancerName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "load-balancer"
  },
  ........<truncated>
}
```

Protože ukázková aplikace je vystaven na Internetu s veřejnou IP adresu, je přidružený Vyrovnávání zatížení tuto adresu. 

Chcete-li zobrazit ukázku JSON v šabloně správce prostředků – [přidružení Vyrovnávání zatížení sítě s veřejnou IP adresu](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L211)na tento odkaz.

```none
"frontendIPConfigurations": [
  {
    "properties": {
      "publicIPAddress": {
        "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicIpAddressName'))]"
      }
    },
    "name": "LoadBalancerFrontend"
  }
]
```

Z portálu Microsoft Azure zobrazuje přehled Vyrovnávání zatížení sítě přidružení veřejnou IP adresu.

![Vyrovnávání zatížení sítě](./media/virtual-machines-windows-dotnet-core/nlb-win.png)

## <a name="load-balancer-rule"></a>Pravidlo Vyrovnávání zatížení

Pokud chcete použít při vyrovnávání zatížení, konfigurovaná pravidla, kterými se řídí jak přenosy rovnoměrně mezi zamýšleného zdroje. Nechejte aplikaci Store hudbu ukázkové přenosy přijde na portu 80 veřejnou IP adresu a rozvržena port 80 všechny virtuálních počítačích. 

Chcete-li zobrazit ukázku JSON v šabloně správce prostředků – [Pravidla Vyrovnávání zatížení](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L226)na tento odkaz.


```none
"loadBalancingRules": [
  {
    "name": "[variables('loadBalencerRule')]",
    "properties": {
      "frontendIPConfiguration": {
        "id": "[concat(resourceId('Microsoft.Network/loadBalancers', variables('loadBalancerName')), '/frontendIPConfigurations/LoadBalancerFrontend')]"
      },
      "backendAddressPool": {
        "id": "[variables('lbPoolID')]"
      },
      "protocol": "Tcp",
      "frontendPort": 80,
      "backendPort": 80,
      "enableFloatingIP": false,
      "idleTimeoutInMinutes": 5,
      "probe": {
        "id": "[variables('lbProbeID')]"
      }
    }
  }
]
```

Zobrazení pravidlo Vyrovnávání zatížení sítě z portálu.

![Pravidlo Vyrovnávání zatížení sítě](./media/virtual-machines-windows-dotnet-core/lbrule-win.png)

## <a name="load-balancer-probe"></a>Zkušební Vyrovnávání zatížení

Vyrovnávání zatížení taky musí sledovat dostupnost virtuálního počítače tak, aby se žádostí o podávané množství jenom pro systémy. Sledování probíhá podle konstantní zjišťování předdefinovaných port. Nasazení hudbu úložiště nakonfigurovaný tak, aby probe port 80 na všechny však započítávány virtuálních počítačích. 

Chcete-li zobrazit ukázku JSON v šabloně správce prostředků – [Probe Vyrovnávání zatížení](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L247)na tento odkaz.


```none
"probes": [
  {
    "properties": {
      "protocol": "Tcp",
      "port": 80,
      "intervalInSeconds": 15,
      "numberOfProbes": 2
    },
    "name": "lbprobe"
  }
]
```

Zkušební Vyrovnávání zatížení vidět z portálu Microsoft Azure.

![Zkušební Vyrovnávání zatížení sítě](./media/virtual-machines-windows-dotnet-core/lbprobe-win.png)

## <a name="inbound-nat-rules"></a>Příchozí pravidla překladu síťových adres

Při použití služby Vyrovnávání zatížení, třeba pravidla zavedením, zpřístupnění není zatížení rovnováha virtuální počítače. Například když vytvoříte připojení ke vzdálené ploše s virtuální počítače, tento přenos by neměly být vyrovnávání zatížení, raději by měl konfigurovat předem určených cesty. Předem určených cesty se konfigurují příchozí pravidla překladu síťových adres zdroje. Pomocí tohoto prostředku, příchozí komunikaci možné namapovat na jednotlivé virtuálních počítačích. 

Nechejte aplikaci Store hudbu port počínaje 5000 namapované na port 3389 na každém počítači virtuální RDP přístup pomocí protokolu. `copyindex()` Funkce se používá k zvyšte příchozí port tak, aby se druhý počítač virtuální přijme příchozí port 5001 třetí 5002 a tak dále.

Chcete-li zobrazit ukázku JSON v šabloně správce prostředků – [Příchozí pravidla překladu síťových adres](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L260)na tento odkaz. 

```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/loadBalancers/inboundNatRules",
  "name": "[concat(variables('loadBalancerName'), '/', 'RDP-VM', copyIndex())]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "load-balancer-nat-rule"
  },
  "copy": {
    "name": "lbNatLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "dependsOn": [
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
  ],
  "properties": {
    "frontendIPConfiguration": {
      "id": "[variables('ipConfigID')]"
    },
    "protocol": "tcp",
    "frontendPort": "[copyIndex(5000)]",
    "backendPort": 3389,
    "enableFloatingIP": false
  }
}
```

Příklad příchozí pravidla překladu síťových adres zobrazené v portálu Azure. Pravidlo překladu síťových adres RDP se vytvoří pro každé virtuálního počítače v nasazení.

![Příchozí pravidla překladu síťových adres](./media/virtual-machines-windows-dotnet-core/natrule-win.png)

Podrobnější informace o vyrovnávání zatížení sítě Azure najdete v článku [služby Azure infrastruktury služby Vyrovnávání zatížení](./virtual-machines-windows-load-balance.md).

## <a name="deploy-multiple-vms"></a>Nasazení více VMs

Nakonec nastavte dostupnost nebo Vyrovnávání zatížení efektivně pracovat, více virtuálních počítačích podporují. Více VMs můžete nasazených pomocí funkce Kopírovat správce prostředků Azure šablony. Pomocí funkce Kopírovat, není nutné zadávat definovat omezený počet virtuálních počítačích, raději tuto hodnotu se dá dynamicky poskytovat v době nasazení. Funkce Kopírovat využívá opakujícího vytvořit a úchyty pro nasazení správný počet virtuálních počítačích a přidružené prostředky.

Hudbu úložiště Ukázková šablona parametru definovaný při počet instancí. Toto číslo je použít v celém šabloně při vytváření virtuálních počítačích a související materiály.

```none
"numberOfInstances": {
  "type": "int",
  "minValue": 1,
  "defaultValue": 2,
  "metadata": {
    "description": "Number of VM instances to be created behind load balancer."
  }
},
```

Na prostředek virtuální počítač smyčka kopie je uveden název a počet jejích instance parametr slouží k určení počtu výsledné kopií.

Chcete-li zobrazit ukázku JSON v šabloně správce prostředků – [Funkce kopie virtuálního počítače](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L290)na tento odkaz. 


```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/virtualMachines",
  "name": "[concat(variables('vmName'),copyindex())]",
  "location": "[resourceGroup().location]",
  "copy": {
    "name": "virtualMachineLoop",
    "count": "[parameters('numberOfInstances')]"
  }
```

Aktuální iterací funkci Kopírovat můžete přistupovat pomocí `copyIndex()` (funkce). Hodnota funkce index kopírovat mohou sloužit k název virtuálních počítačích a dalších zdrojů. Pokud jsou umístěné dvě instance virtuálního počítače, například potřebují různé názvy. `copyIndex()` Funkce mohou sloužit jako součást název počítače virtuální vytvořit jedinečný název. Příklad `copyindex()` funkce se používá účelům pojmenování může vypadat jinak prostředek virtuální počítač. Tady je tvořen název počítače `vmName` parametr a `copyIndex()` (funkce). 

Chcete-li zobrazit ukázku JSON v šabloně správce prostředků – [Funkce Index kopie](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L309)na tento odkaz. 


```none
"osProfile": {
  "computerName": "[concat(variables('vmName'),copyindex())]",
  "adminUsername": "[parameters('adminUsername')]",
  "adminPassword": "[parameters('adminPassword')]"
}
```

`copyIndex` Se používá funkce několikrát Ukázková šablona hudbu úložiště. Zdroje a použití funkce `copyIndex` obsahuje něco specifické pro jeden výskyt virtuální počítač takové a rozhraní sítě, pravidla Vyrovnávání zatížení a některé závisí na tom, funkce. 

Další informace o funkci Kopírovat najdete v článku [Vytvoření více instancí zdrojů v Azure správce](../resource-group-create-multiple.md).

## <a name="next-step"></a>Další krok

<hr>

[Krok 4 – nasazení aplikace šablonami správce prostředků Azure](./virtual-machines-windows-dotnet-core-5-app-deployment.md)