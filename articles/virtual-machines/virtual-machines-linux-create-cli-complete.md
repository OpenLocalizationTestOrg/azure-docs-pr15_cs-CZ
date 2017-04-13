
<properties
   pageTitle="Vytvoření dokončení Linux prostředí pomocí rozhraní příkazového řádku Azure | Microsoft Azure"
   description="Vytvoření úložiště, Linux OM, virtuální sítě a podsítě, Vyrovnávání zatížení, NIC, veřejnou IP a skupiny zabezpečení sítě, všechny od základu nahoru pomocí rozhraní příkazového řádku Azure."
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="iainfoulds"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/24/2016"
   ms.author="iainfou"/>

# <a name="create-a-complete-linux-environment-by-using-the-azure-cli"></a>Vytvoření prostředí dokončení Linux pomocí rozhraní příkazového řádku Azure

V tomto článku jsme vytvořit jednoduchý síť s Vyrovnávání zatížení a dvojice VMs, které jsou užitečné pro vývoj a jednoduché výpočetních. Projdeme procesu příkazu command, dokud nedosáhnete dva pracovní, zabezpečené VMs Linux ke kterým se můžete připojit z kdekoli na Internetu. Pak můžete posunout složitější sítí a prostředí.

Tím můžete informace o hierarchií závislostí, aby vám nabídne nasazení modelu správce prostředků a o kolik power jej obsahuje. Až uvidíte, jak je systém sestavují, můžete znovu sestavit ho rychleji pomocí [Správce prostředků Azure šablon](../resource-group-authoring-templates.md). Po se dozvíte, jak součásti prostředí spolupracují, vytváření šablon k automatizaci je také stane snadnější.

Prostředí obsahuje:

- Dva VMs uvnitř sady dostupnosti.
- Vyrovnávání zatížení pomocí pravidel Vyrovnávání zatížení na portu 80.
- Pravidla skupiny (NSG) zabezpečení chránit vaše OM z nežádoucích přenosy v síti.

![Přehled základní prostředí](./media/virtual-machines-linux-create-cli-complete/environment_overview.png)

Pokud chcete vytvořit vlastní prostředí, potřebujete nejnovější [Azure rozhraní příkazového řádku](../xplat-cli-install.md) v režimu správce prostředků (`azure config mode arm`). Budete potřebovat JSON analýza nástroj. Tento příklad používá [jq](https://stedolan.github.io/jq/).

## <a name="quick-commands"></a>Rychlé příkazy
Pokud potřebujete rychle provést úkol tyto údaje oddíl základu příkazy k nahrání OM na Azure. Podrobnější informace a kontext pro každý krok najdete ve zbývající části dokumentu, počáteční [tady](#detailed-walkthrough).

Ujistěte se, jestli máte [Rozhraní příkazového řádku Azure](../xplat-cli-install.md) přihlášení a správce prostředků v režimu:

```bash
azure config mode arm
```

V následujících příkladech nahraďte názvy parametrů příklad vlastní hodnoty. Názvy parametrů příklad zahrnout `myResourceGroup`, `mystorageaccount`, a `myVM`.

Vytvoření skupiny zdrojů. Následující příklad vytvoří skupinu zdroje s názvem `myResourceGroup` v `westeurope` umístění:

```bash
azure group create -n myResourceGroup -l westeurope
```

Skupina zdroje ověřte pomocí analyzátoru formátu JSON:

```bash
azure group show myResourceGroup --json | jq '.'
```

Vytvoření účtu úložiště. Následující příklad vytvoří úložiště účet s názvem `mystorageaccount` (název účtu úložiště musí být jedinečné, takže zadejte vlastní jedinečný název):

```bash
azure storage account create -g myResourceGroup -l westeurope \
  --kind Storage --sku-name GRS mystorageaccount
```

Ověření účtu úložiště pomocí analyzátoru formátu JSON:

```bash
azure storage account show -g myResourceGroup mystorageaccount --json | jq '.'
```

Vytvořte virtuální sítě. Následující příklad vytvoří virtuální sítě s názvem `myVnet`:

```bash
azure network vnet create -g myResourceGroup -l westeurope\
  -n myVnet -a 192.168.0.0/16
```

Vytvoření podsítě. Následující příklad vytvoří podsítě s názvem `mySubnet`"

```bash
azure network vnet subnet create -g myResourceGroup \
  -e myVnet -n mySubnet -a 192.168.1.0/24
```

Virtuální sítě a ověřte podsítě pomocí analyzátoru formátu JSON:


```bash
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

Vytvoření veřejné IP. Následující příklad vytvoří veřejnou IP s názvem `myPublicIP` s názvem DNS `mypublicdns` (název DNS musí být jedinečné, takže zadejte jedinečný název vlastní):

```bash
azure network public-ip create -g myResourceGroup -l westeurope \
  -n myPublicIP  -d mypublicdns -a static -i 4
```

Vytvoření Vyrovnávání zatížení. Následující příklad vytvoří Vyrovnávání zatížení s názvem `myLoadBalancer`:

```bash
azure network lb create -g myResourceGroup -l westeurope -n myLoadBalancer
```

Vytvoření fondu front-end IP pro vyrovnávání zatížení a přidružení veřejnou IP. Následující příklad vytvoří fondu front-end IP s názvem `mySubnetPool`:

```bash
azure network lb frontend-ip create -g myResourceGroup -l myLoadBalancer \
  -i myPublicIP -n myFrontEndPool 
```

Vytvoření fondu IP back-end pro vyrovnávání zatížení. Následující příklad vytvoří fond IP back-end s názvem `myBackEndPool`:

```bash
azure network lb address-pool create -g myResourceGroup -l myLoadBalancer \
  -n myBackEndPool
```

Vytvořte příchozí síťové SSH adresu překladu pravidla pro vyrovnávání zatížení. Následující příklad vytvoří dvě pravidla Vyrovnávání zatížení, `myLoadBalancerRuleSSH1` a `myLoadBalancerRuleSSH2`:

```bash
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH1 -p tcp -f 4222 -b 22
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH2 -p tcp -f 4223 -b 22
```

Vytvoření webu příchozí pravidla překladu síťových adres pro vyrovnávání zatížení. Následující příklad vytvoří pravidlo Vyrovnávání zatížení s názvem`myLoadBalancerRuleWeb`

```bash
azure network lb rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleWeb -p tcp -f 80 -b 80 \
  -t myFrontEndPool -o myBackEndPool
```

Vytvořte zkušební stavu Vyrovnávání zatížení. Následující příklad vytvoří TCP zkušební s názvem `myHealthProbe`:

```bash
azure network lb probe create -g myResourceGroup -l myLoadBalancer \
  -n myHealthProbe -p "tcp" -i 15 -c 4
```

Ověření pomocí analyzátoru formátu JSON Vyrovnávání zatížení, fondy IP a překladu síťových adres pravidla:

```bash
azure network lb show -g myResourceGroup -n myLoadBalancer --json | jq '.'
```

Vytvoření první síťové karty (NIC). Nahrazení `#####-###-###` oddíly pomocí vlastní ID Azure předplatného. Vaše předplatné ID je uvedeno v výstup `jq` při zkoumání zdroje vytváříte. Můžete taky zobrazit ID předplatného s `azure account list`. 

Následující příklad vytvoří NIC s názvem `myNic1`:

```bash
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic1 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
```

Vytvoření druhého NIC. Následující příklad vytvoří NIC s názvem `myNic2`:

```bash
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic2 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
```

Ověřte dvou nic pomocí analyzátoru formátu JSON:

```bash
azure network nic show myResourceGroup myNic1 --json | jq '.'
azure network nic show myResourceGroup myNic2 --json | jq '.'
```

Vytvoření skupiny zabezpečení sítě. Následující příklad vytvoří skupinu zabezpečení sítě s názvem `myNetworkSecurityGroup`:

```bash
azure network nsg create -g myResourceGroup -l westeurope \
  -n myNetworkSecurityGroup
```

Přidání dvou příchozí pravidla pro skupinu zabezpečení sítě. Následující příklad vytvoří dvě pravidla `myNetworkSecurityGroupRuleSSH` a `myNetworkSecurityGroupRuleHTTP`:

```bash
azure network nsg rule create -p tcp -r inbound -y 1000 -u 22 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleSSH
azure network nsg rule create -p tcp -r inbound -y 1001 -u 80 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleHTTP
```

Skupina zabezpečení sítě a ověřte příchozí pravidla pomocí analyzátoru formátu JSON:

```bash
azure network nsg show -g myResourceGroup -n myNetworkSecurityGroup --json | jq '.'
```

Skupina zabezpečení sítě svázat dvou nic:

```bash
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic1
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic2
```

Vytvoření sady dostupnosti. Následující příklad vytvoří dostupné nastavit pojmenované `myAvailabilitySet`:

```bash
azure availset create -g myResourceGroup -l westeurope -n myAvailabilitySet
```

Vytvoření první OM Linux. Následující příklad vytvoří OM s názvem `myVM1`:

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM1 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic1 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username ops
```

Vytvoření druhého OM Linux. Následující příklad vytvoří OM s názvem `myVM2`:

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM2 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic2 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username ops
```

Pomocí analyzátoru formátu JSON ověřte, že všechny položky, které tvořily původní:

```bash
azure vm show -g myResourceGroup -n myVM1 --json | jq '.'
azure vm show -g myResourceGroup -n myVM2 --json | jq '.'
```

Nové prostředí exportujte do šablony a rychle znovu vytvořte nové instance:

```bash
azure group export myResourceGroup
```

## <a name="detailed-walkthrough"></a>Podrobné informace
Popisují podrobné kroky popisují, co každého příkazu probíhá při vytváření mimo prostředí. Koncepce jsou užitečné, když vytvoříte vlastní vlastní prostředí pro vývoje nebo výroby.

Ujistěte se, jestli máte [Rozhraní příkazového řádku Azure](../xplat-cli-install.md) přihlášení a správce prostředků v režimu:

```bash
azure config mode arm
```

V následujících příkladech nahraďte názvy parametrů příklad vlastní hodnoty. Názvy parametrů příklad zahrnout `myResourceGroup`, `mystorageaccount`, a `myVM`.

## <a name="create-resource-groups-and-choose-deployment-locations"></a>Vytvoření skupiny zdrojů a zvolte umístění nasazení

Skupiny Azure prostředků jsou logické nasazení entity, které obsahují informace o konfiguraci a metadata povolit logické Správa nasazení zdroje. Následující příklad vytvoří skupinu zdroje s názvem `myResourceGroup` v `westeurope` umístění:

```bash
azure group create --name myResourceGroup --location westeurope
```

Výstup:

```bash                        
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
data:    Id:                  /subscriptions/guid/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westeurope
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

## <a name="create-a-storage-account"></a>Vytvoření účtu úložiště

Účty úložiště potřebujete pro disků OM a pro všechny další data disků, které chcete přidat. Vytvoření úložiště účtů téměř ihned po vytvoření skupiny zdrojů.

Tady máme použít `azure storage account create` příkazu a typ podpory úložiště se má předávání umístění účtu Skupina zdroje ovládacích prvků. Následující příklad vytvoří úložiště účet s názvem `mystorageaccount`:

```bash
azure storage account create \  
  --location westeurope \
  --resource-group myResourceGroup \
  --kind Storage --sku-name GRS \
  mystorageaccount
```

Výstup:

```bash
info:    Executing command storage account create
+ Creating storage account
info:    storage account create command OK
```

Prověřit naší pole Skupina zdroje pomocí `azure group show` příkazu, použijeme nástroj [jq](https://stedolan.github.io/jq/) spolu s `--json` možnost Azure rozhraní příkazového řádku. (Slouží **jsawk** nebo jazyk knihovny chcete analyzovat ve formátu JSON.)

```bash
azure group show myResourceGroup --json | jq '.'
```

Výstup:

```bash
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

Prozkoumat účtu úložiště pomocí rozhraní příkazového řádku, musíte nejdřív nastavit účet názvy a klíče. Nahraďte název, který můžete zvolit název účtu úložiště v následujícím příkladu:

```
AZURE_STORAGE_CONNECTION_STRING="$(azure storage account connectionstring show mystorageaccount --resource-group myResourceGroup --json | jq -r '.string')"
```

Můžete zobrazit informace o ukládání snadno:

```bash
azure storage container list
```

Výstup:

```bash
info:    Executing command storage container list
+ Getting storage containers
data:    Name  Public-Access  Last-Modified
data:    ----  -------------  -----------------------------
data:    vhds  Off            Sun, 27 Sep 2015 19:03:54 GMT
info:    storage container list command OK
```

## <a name="create-a-virtual-network-and-subnet"></a>Vytvořit virtuální sítě a podsítě

Další budete muset vytvořit virtuální spuštěné v Azure a podsítě, ve kterém můžete vytvořit svůj VMs sítě. Následující příklad vytvoří virtuální sítě s názvem `myVnet` s `192.168.0.0/16` předponu adresy:

```bash
azure network vnet create --resource-group myResourceGroup --location westeurope \
  --name myVnet --address-prefixes 192.168.0.0/16 
```

Výstup:

```bash
info:    Executing command network vnet create
+ Looking up virtual network "myVnet"
+ Creating virtual network "myVnet"
+ Loading virtual network state
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet
data:    Name                            : myVnet
data:    Type                            : Microsoft.Network/virtualNetworks
data:    Location                        : westeurope
data:    ProvisioningState               : Succeeded
data:    Address prefixes:
data:      192.168.0.0/16
info:    network vnet create command OK
```

Znovu použijeme možnost – json `azure group show` a **jq** zobrazíte, jak vytváříme naše zdroje. Nyní je k dispozici `storageAccounts` zdroje a `virtualNetworks` zdroje.  

```bash
azure group show myResourceGroup --json | jq '.'
```

Výstup:

```bash
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
      "name": "myVnet",
      "type": "virtualNetworks",
      "location": "westeurope",
      "tags": null
    },
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

Teď Pojďme vytvořit podsítě v `myVnet` virtuální sítě, do kterého jsou nasazeny VMs. Používáme `azure network vnet subnet create` příkazu spolu s jste už vytvořili zdroje: `myResourceGroup` pole Skupina zdroje a `myVnet` virtuální sítě. V následujícím příkladu jsme přidat podsítě s názvem `mySubnet` s předponou adres podsítí `192.168.1.0/24`:

```bash
azure network vnet subnet create --resource-group myResourceGroup \
  --vnet-name myVnet --name mySubnet --address-prefix 192.168.1.0/24
```

Výstup:

```bash
info:    Executing command network vnet subnet create
+ Looking up the subnet "mySubnet"
+ Creating subnet "mySubnet"
+ Looking up the subnet "mySubnet"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:    Type                            : Microsoft.Network/virtualNetworks/subnets
data:    ProvisioningState               : Succeeded
data:    Name                            : mySubnet
data:    Address prefix                  : 192.168.1.0/24
data:
info:    network vnet subnet create command OK
```

Protože podsítě je logicky uvnitř virtuální sítě, podíváme informace podsítě mírně odlišnou příkazem. Příkaz používáme je `azure network vnet show`, ale nemůžeme pokračovat zkoumat výstupu JSON pomocí **jq**.

```bash
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

Výstup:

```bash
{
  "subnets": [
    {
      "ipConfigurations": [],
      "addressPrefix": "192.168.1.0/24",
      "provisioningState": "Succeeded",
      "name": "mySubnet",
      "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
    }
  ],
  "tags": {},
  "addressSpace": {
    "addressPrefixes": [
      "192.168.0.0/16"
    ]
  },
  "dhcpOptions": {
    "dnsServers": []
  },
  "provisioningState": "Succeeded",
  "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
  "name": "myVnet",
  "location": "westeurope"
}
```

## <a name="create-a-public-ip-address-pip"></a>Vytvořit veřejnou IP adresu (PIP)

Teď Pojďme vytvořit veřejnou IP adresy (PIP), kterou jsme přiřadit Vyrovnávání zatížení. Umožňuje připojit k vaší VMs z Internetu pomocí `azure network public-ip create` příkaz. A proto výchozí adresu je dynamické vytvoříme pojmenované položky DNS v doméně **cloudapp.azure.com** pomocí `--domain-name-label` možnost. Následující příklad vytvoří veřejnou IP s názvem `myPublicIP` s názvem DNS `mypublicdns`. Jako název DNS musí být jedinečné, zadejte vlastní jedinečný název DNS:

```bash
azure network public-ip create --resource-group myResourceGroup \
  --location westeurope --name myPublicIP --domain-name-label mypublicdns
```

Výstup:

```bash
info:    Executing command network public-ip create
+ Looking up the public ip "myPublicIP"
+ Creating public ip address "myPublicIP"
+ Looking up the public ip "myPublicIP"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
data:    Name                            : myPublicIP
data:    Type                            : Microsoft.Network/publicIPAddresses
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Allocation method               : Dynamic
data:    Idle timeout                    : 4
data:    Domain name label               : mypublicdns
data:    FQDN                            : mypublicdns.westeurope.cloudapp.azure.com
info:    network public-ip create command OK
```

Na veřejnou IP adresu je také nejvyšší úrovně zdroje, abyste ho viděli `azure group show`.

```bash
azure group show myResourceGroup --json | jq '.'
```

Výstup:

```bash
{
"tags": {},
"id": "/subscriptions/guid/resourceGroups/myResourceGroup",
"name": "myResourceGroup",
"provisioningState": "Succeeded",
"location": "westeurope",
"properties": {
    "provisioningState": "Succeeded"
},
"resources": [
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
    "name": "myPublicIP",
    "type": "publicIPAddresses",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
    "name": "myVnet",
    "type": "virtualNetworks",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
    "name": "mystorageaccount",
    "type": "storageAccounts",
    "location": "westeurope",
    "tags": null
    }
],
"permissions": [
    {
    "actions": [
        "*"
    ],
    "notActions": []
    }
]
}
```

Prozkoumejte zdroje podrobnosti, včetně plně kvalifikovaný název domény (FQDN) subdomény, pomocí kompletní `azure network public-ip show` příkaz. Veřejné zdroje IP adresu přidělené logicky, ale konkrétní adresu nebyla dosud nebyly přiřazeny. Získá IP adresu, budete potřebovat při vyrovnávání zatížení, který jste ještě nevytvořili.

```bash
azure network public-ip show myResourceGroup myPublicIP --json | jq '.'
```

Výstup:

```bash
{
"tags": {},
"publicIpAllocationMethod": "Dynamic",
"dnsSettings": {
    "domainNameLabel": "mypublicdns",
    "fqdn": "mypublicdns.westeurope.cloudapp.azure.com"
},
"idleTimeoutInMinutes": 4,
"provisioningState": "Succeeded",
"etag": "W/\"c63154b3-1130-49b9-a887-877d74d5ebc5\"",
"id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
"name": "myPublicIP",
"location": "westeurope"
}
```

## <a name="create-a-load-balancer-and-ip-pools"></a>Vytvoření Vyrovnávání zatížení a fondy IP
Při vytváření Vyrovnávání zatížení umožňuje distribuovat přenosy napříč několika VMs. Také poskytuje redundance aplikaci tím, že spouštění více virtuálních, které odpovídají koncových uživatelů v případě údržbu nebo vysoké zatížení. Následující příklad vytvoří Vyrovnávání zatížení s názvem `myLoadBalancer`:

```bash
azure network lb create --resource-group myResourceGroup --location westeurope \
  --name myLoadBalancer
```

Výstup:

```bash
info:    Executing command network lb create
+ Looking up the load balancer "myLoadBalancer"
+ Creating load balancer "myLoadBalancer"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer
data:    Name                            : myLoadBalancer
data:    Type                            : Microsoft.Network/loadBalancers
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
info:    network lb create command OK
```

Náš Vyrovnávání zatížení je poměrně prázdná, tak se Pojďme vytvořit některé fondy IP. Chcete vytvořit dvěma fondy IP pro naše Vyrovnávání zatížení – jeden pro front-end a druhý pro back-end. Fondu front-end IP je veřejně viditelné. Je taky umístění, do kterého jsme přiřadit PIP, kterou jsme vytvořili dříve. Potom používáme fondu back-end jako místo naše VMs připojit. Tímto způsobem přenos můžete postupují Vyrovnávání zatížení k VMs.

Nejdřív Vytvořme naše fondu front-end IP. Následující příklad vytvoří fondu front-end s názvem `myFrontEndPool`:

```bash
azure network lb frontend-ip create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --public-ip-name myPublicIP \
  --name myFrontEndPool 
```

Výstup:

```bash
info:    Executing command network lb frontend-ip create
+ Looking up the load balancer "myLoadBalancer"
+ Looking up the public ip "myPublicIP"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myFrontEndPool
data:    Provisioning state              : Succeeded
data:    Private IP allocation method    : Dynamic
data:    Public IP address id            : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
info:    network lb mySubnet-ip create command OK
```

Všimněte si, jak jsme použili `--public-ip-name` přepnout na předat `myPublicIP` , kterou jsme vytvořili dříve. Přiřazení veřejnou IP adresu pro vyrovnávání zatížení umožňuje kontaktovat vaše VMs všude na Internetu.

Pak vytvoření naše druhý fondu IP tentokrát přenosů naše back-end. Následující příklad vytvoří fond back-end s názvem `myBackEndPool`:

```bash
azure network lb address-pool create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myBackEndPool
```

Výstup:

```bash
info:    Executing command network lb address-pool create
+ Looking up the load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myBackEndPool
data:    Provisioning state              : Succeeded
info:    network lb address-pool create command OK
```

Je vidět, jak naši Vyrovnávání zatížení vedou zobrazením s `azure network lb show` a zkoumání výstupu JSON:

```bash
azure network lb show myResourceGroup myLoadBalancer --json | jq '.'
```

Výstup:

```bash
{
  "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [],
  "probes": []
}
```

## <a name="create-load-balancer-nat-rules"></a>Vytváření pravidel překladu síťových adres Vyrovnávání zatížení
Získat provoz v naší Vyrovnávání zatížení, potřebujeme vytvořit síť adresu překladu pravidla, které určují příchozí nebo odchozí akce. Můžete zadat protokol použít a potom mapování externí porty na vnitřní porty podle potřeby. Naše prostředí Vytvořme některá pravidla, které umožňují SSH prostřednictvím naše Vyrovnávání zatížení k naše VMs. Porty TCP 4222 a 4223 jsme nastavit odkázat TCP port 22 na naše VMs (které vytvoříme později). Následující příklad vytvoří pravidla s názvem `myLoadBalancerRuleSSH1` TCP port 4222 přiřadit port 22:

```bash
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH1 \
  --protocol tcp --frontend-port 4222 --backend-port 22
```

Výstup:

```bash
info:    Executing command network lb inbound-nat-rule create
+ Looking up the load balancer "myLoadBalancer"
warn:    Using default enable floating ip: false
warn:    Using default idle timeout: 4
warn:    Using default mySubnet IP configuration "myFrontEndPool"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleSSH1
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 4222
data:    Backend port                    : 22
data:    Enable floating IP              : false
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
info:    network lb inbound-nat-rule create command OK
```

Opakujte postup pro druhé pravidlo překladu síťových adres pro SSH. Následující příklad vytvoří pravidla s názvem `myLoadBalancerRuleSSH2` TCP port 4223 přiřadit port 22:

```bash
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH2 --protocol tcp \
  --frontend-port 4223 --backend-port 22
```

Pojďme také pokračovat a vytvořit pravidlo překladu síťových adres pro port TCP 80 přenosů web zapojení pravidla nahoru naše fondy IP. Pokud nám připojit pravidlo fondu IP místo zachycení pravidlo naše VMs jednotlivě, jsme přidat nebo odebrat VMs z fondu IP. Vyrovnávání zatížení automaticky přizpůsobí tok přenosy. Následující příklad vytvoří pravidla s názvem `myLoadBalancerRuleWeb` TCP port 80 přiřadit port 80:

```bash
azure network lb rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleWeb --protocol tcp \
  --frontend-port 80 --backend-port 80 --frontend-ip-name myFrontEndPool \
  --backend-address-pool-name myBackEndPool
```

Výstup:

```bash
info:    Executing command network lb rule create
+ Looking up the load balancer "myLoadBalancer"
warn:    Using default idle timeout: 4
warn:    Using default enable floating ip: false
warn:    Using default load distribution: Default
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleWeb
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 80
data:    Backend port                    : 80
data:    Enable floating IP              : false
data:    Load distribution               : Default
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
data:    Backend address pool id         : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
info:    network lb rule create command OK
```

## <a name="create-a-load-balancer-health-probe"></a>Vytvořte zkušební stavu Vyrovnávání zatížení

Zdravotní probe pravidelně kontroly, které jsou za naše Vyrovnávání zatížení zkontrolujte, jestli máte provozní a reagování na žádosti o definované VMs. V opačném případě jste odebrali operace zajistit, aby uživatelé nejsou byli přesměrováni do nich. Můžete definovat vlastní kontroly stavu zkušební spolu s intervalů a časových limitů hodnoty. Další informace o stavu sond najdete v tématu [sond Vyrovnávání zatížení](../load-balancer/load-balancer-custom-probe-overview.md). Následující příklad vytvoří TCP stavu zkoumat pojmenované `myHealthProbe`:

```bash
azure network lb probe create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myHealthProbe --protocol "tcp" \
  --interval 15 --count 4
```

Výstup:

```bash
info:    Executing command network lb probe create
warn:    Using default probe port: 80
+ Looking up the load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myHealthProbe
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    Port                            : 80
data:    Interval in seconds             : 15
data:    Number of probes                : 4
info:    network lb probe create command OK
```

Tady jsme zadané intervalu 15 sekund pro naše kontroly stavu. Jsme můžete zmeškat nejvýše čtyři sond (minutu) před Vyrovnávání zatížení byly použity už funguje hostiteli.

## <a name="verify-the-load-balancer"></a>Ověření Vyrovnávání zatížení
Konfigurace služby Vyrovnávání zatížení teď probíhá. Tady je postup, jako když jste:

1. Nejdřív jste vytvořili při vyrovnávání zatížení.
2. Potom jste vytvořili fondu front-end IP a přiřazenou veřejnou IP.
3. Vytvoření fondu back-end IP, který VMs se můžete připojit k další.
4. Potom jste vytvořili překladu síťových adres pravidla, které umožňují SSH VMs pro správu, spolu s pravidlo, které umožňuje port TCP 80 pro naše web app.
5. Nakonec přidaný stavu zkušební pravidelné hledání VMs. Tento stav zkušební zaručuje, že si uživatelé pokusí o přístup OM, je už funguje nebo použití obsahu.

Pojďme si rozebrat Vyrovnávání zatížení vypadá nyní:

```bash
azure network lb show --resource-group myResourceGroup \
  --name myLoadBalancer --json | jq '.'
```

Výstup:

```bash
{
  "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH1",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4222,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    },
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH2",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4223,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    }
  ],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "inboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        },
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleWeb",
      "provisioningState": "Succeeded",
      "enableFloatingIP": false,
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "backendAddressPool": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
      },
      "protocol": "Tcp",
      "loadDistribution": "Default",
      "mySubnetPort": 80,
      "backendPort": 80,
      "idleTimeoutInMinutes": 4
    }
  ],
  "probes": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myHealthProbe",
      "provisioningState": "Succeeded",
      "numberOfProbes": 4,
      "intervalInSeconds": 15,
      "port": 80,
      "protocol": "Tcp",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/probes/myHealthProbe"
    }
  ]
}
```

## <a name="create-an-nic-to-use-with-the-linux-vm"></a>Vytvoření NIC pomocí služby Linux OM

Nic jsou programově k dispozici, protože můžete použít pravidla pro jejich použití. Mohou také obsahovat více než jedno. V následujícím `azure network nic create` příkaz připojit NIC do fondu zatížení back-end IP a přidružit překladu síťových adres pravidlo pro povolení SSH komunikace.
 
Nahrazení `#####-###-###` oddíly pomocí vlastní ID Azure předplatného. Vaše předplatné ID je uvedeno v výstup `jq` při zkoumání zdroje vytváříte. Můžete taky zobrazit ID předplatného s `azure account list`. 

Následující příklad vytvoří NIC s názvem `myNic1`:

```bash
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic1 \
  --lb-address-pool-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
```

Výstup:

```bash
info:    Executing command network nic create
+ Looking up the subnet "mySubnet"
+ Looking up the network interface "myNic1"
+ Creating network interface "myNic1"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1
data:    Name                            : myNic1
data:    Type                            : Microsoft.Network/networkInterfaces
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Enable IP forwarding            : false
data:    IP configurations:
data:      Name                          : Nic-IP-config
data:      Provisioning state            : Succeeded
data:      Private IP address            : 192.168.1.4
data:      Private IP allocation method  : Dynamic
data:      Subnet                        : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:      Load balancer backend address pools:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
data:      Load balancer inbound NAT rules:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
data:
info:    network nic create command OK
```

Zobrazit podrobnosti porovnáním zdroje přímo. Zkoumat zdroje pomocí `azure network nic show` příkaz:

```bash
azure network nic show myResourceGroup myNic1 --json | jq '.'
```

Výstup:

```bash
{
  "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
  "provisioningState": "Succeeded",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1",
  "name": "myNic1",
  "type": "Microsoft.Network/networkInterfaces",
  "location": "westeurope",
  "ipConfigurations": [
    {
      "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1/ipConfigurations/Nic-IP-config",
      "loadBalancerBackendAddressPools": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
        }
      ],
      "loadBalancerInboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        }
      ],
      "privateIPAddress": "192.168.1.4",
      "privateIPAllocationMethod": "Dynamic",
      "subnet": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
      },
      "provisioningState": "Succeeded",
      "name": "Nic-IP-config"
    }
  ],
  "dnsSettings": {
    "appliedDnsServers": [],
    "dnsServers": []
  },
  "enableIPForwarding": false,
  "resourceGuid": "a20258b8-6361-45f6-b1b4-27ffed28798c"
}
```

Teď můžeme vytvořit druhý NIC zapojení do fondu IP naše back-end znovu. Toto pravidlo překladu síťových adres čas druhý umožňuje SSH přenosy. Následující příklad vytvoří NIC s názvem `myNic2`:

```bash
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic2 \
  --lb-address-pool-ids  /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2
```

## <a name="create-a-network-security-group-and-rules"></a>Vytvoření skupiny zabezpečení sítě a pravidla

Teď můžeme vytvořit skupinu zabezpečení sítě a příchozí pravidla, která bude řídit přístup k rozhraní NIC. Skupiny zabezpečení sítě lze použít k NIC nebo podsítě. Definujte pravidla pro řízení toku přenosy a odhlášení z vaší VMs. Následující příklad vytvoří skupinu zabezpečení sítě s názvem `myNetworkSecurityGroup`:

```bash
azure network nsg create --resource-group myResourceGroup --location westeurope \
  --name myNetworkSecurityGroup
```

Přidejte do něj příchozí pravidla pro NSG umožňuje příchozí připojení port 22 (kvůli podpoře SSH). Následující příklad vytvoří pravidla s názvem `myNetworkSecurityGroupRuleSSH` umožňuje TCP port 22:

```bash
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1000 --destination-port-range 22 --access allow \
  --name myNetworkSecurityGroupRuleSSH
```

Teď Pojďme přidání příchozí pravidla pro NSG umožňuje příchozí připojení na portu 80 (pro podporu webu přenosu). Následující příklad vytvoří pravidla s názvem `myNetworkSecurityGroupRuleHTTP` umožňuje TCP port 80:

```bash
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1001 --destination-port-range 80 --access allow \
  --name myNetworkSecurityGroupRuleHTTP
```

> [AZURE.NOTE] Příchozí pravidla je filtr pro příchozí připojení. V tomto příkladu jsme svázat NSG virtuální NIC VMs, což znamená, že jakékoliv žádosti o s portem 22 projde NIC na naše OM. Toto pravidlo pro příchozí je o připojení k síti a o koncový bod, který není co by bylo o v klasické nasazení. Otevření portu, ponechte `--source-port-range` nastavena na "\*" (nebo rovná hodnotě) přijmout příchozí žádosti o **všechny** žádosti o port. Porty jsou obvykle dynamické.

## <a name="bind-to-the-nic"></a>Svázat s NIC

Svázat NSG nic. Potřebujeme spojit naše nic se naší sítě skupiny zabezpečení. Spusťte oba příkazy připojit z našich nic:

```bash
azure network nic set --resource-group myResourceGroup --name myNic1 \
  --network-security-group-name myNetworkSecurityGroup
```

```bash
azure network nic set --resource-group myResourceGroup --name myNic2 \
  --network-security-group-name myNetworkSecurityGroup
```

## <a name="create-an-availability-set"></a>Vytvoření sady dostupnosti
Dostupnost nastaví šíření nápovědy svého VMs přes poruch domén a upgrade domén. Vytvoření dostupné pro váš VMs. Následující příklad vytvoří dostupné nastavit pojmenované `myAvailabilitySet`:

```bash
azure availset create --resource-group myResourceGroup --location westeurope
  --name myAvailabilitySet
```

Poruchy domény nadefinovat seskupování virtuálních počítačích majících společné zdroje a síťové vypínač. Ve výchozím nastavení jsou oddělené virtuálních počítačích, které jsou nakonfigurováno v sadě dostupnost přes až tři poruch domény. Cílem je, že hardwaru problém na jednu z těchto domén poruch nemá vliv na každé OM, na kterém běží aplikace. Azure automaticky rozdělí VMs domény poruch při umístění v sadě dostupnosti.

Upgrade domény označují skupiny virtuálních počítačích a základní fyzické hardware, který můžete restartování ve stejnou dobu. Pořadí, ve kterém jsou restartování upgradu domény nemusí být sekvenční plánované údržbě, ale jenom jeden upgrade po restartování najednou. Znovu Azure automaticky rozdělí vaší VMs upgradu domén při umístění v kolekci webů pro dostupnosti.

Další informace o [správě dostupnost VMs](./virtual-machines-linux-manage-availability.md).

## <a name="create-the-linux-vms"></a>Vytvoření Linux VMs

Jste si vytvořili úložiště a sítě prostředky, které podporují přístupné pro Internet VMs. Teď Pojďme vytvořit můžou být VMs a zabezpečit SSH klíčem, který nemá hesla. V tomto případě chceme vytvořit systémem Ubuntu OM podle posledních l. Pomocí vyhledat tyto informace obrázek `azure vm image list`, jak je uvedeno v [hledání obrázků OM Azure](virtual-machines-linux-cli-ps-findimage.md).

Jsme vybraný obrázek pomocí příkazu `azure vm image list westeurope canonical | grep LTS`. V tomto případě používáme `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`. V posledním poli nám předat `latest` , aby v budoucnu jsme vždycky nejnovější Tvůrce dotazů. (Řetězec používáme `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`).

Dalším krokem je známé všem uživatelům, kteří už vytvořila ssh veřejné a privátní klíče rsa spárování na Linux nebo Mac s použitím **ssh-keygen-t rsa -b 2048**. Pokud nemáte žádné dvojice klíčů certifikát vaší `~/.ssh` adresáře, můžete vytvořit je:

- Automaticky pomocí `azure vm create --generate-ssh-keys` možnost.
- Ručně pomocí [pokynů a vytvořte sami](virtual-machines-linux-mac-create-ssh-keys.md).

Můžete taky můžete použít `--admin-password` metody ověřování připojení SSH po vytvoření OM. Tento způsob je obvykle méně bezpečné.

Vytvoříme OM tak, že všechny naše materiály a informace, spolu s `azure vm create` příkaz:

```bash
azure vm create \
  --resource-group myResourceGroup \
  --name myVM1 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic1 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username ops
```

Výstup:

```bash
info:    Executing command vm create
+ Looking up the VM "myVM1"
info:    Verifying the public key SSH file: /home/ahmet/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_DS1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account mystorageaccount
+ Looking up the availability set "myAvailabilitySet"
info:    Found an Availability set "myAvailabilitySet"
+ Looking up the NIC "myNic1"
info:    Found an existing NIC "myNic1"
info:    Found an IP configuration with virtual network subnet id "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet" in the NIC "myNic1"
info:    This is an NIC without publicIP configured
info:    The storage URI 'https://mystorageaccount.blob.core.windows.net/' will be used for boot diagnostics settings, and it can be overwritten by the parameter input of '--boot-diagnostics-storage-uri'.
info:    vm create command OK
```

Se můžete připojit k vaší OM okamžitě pomocí výchozí SSH klíče. Ujistěte se, zadejte příslušný port od jsme jste procházející Vyrovnávání zatížení. (Pro naše první OM jsme nastavit pravidlo překladu síťových adres přesměrovat port 4222 naše v angličtině):

```bash
 ssh ops@mypublicdns.westeurope.cloudapp.azure.com -p 4222
```

Výstup:

```bash
The authenticity of host '[mypublicdns.westeurope.cloudapp.azure.com]:4222 ([xx.xx.xx.xx]:4222)' can't be established.
ECDSA key fingerprint is 94:2d:d0:ce:6b:fb:7f:ad:5b:3c:78:93:75:82:12:f9.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '[mypublicdns.westeurope.cloudapp.azure.com]:4222,[xx.xx.xx.xx]:4222' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.1 LTS (GNU/Linux 4.4.0-34-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

ops@myVM1:~$
```

Pojďte dále a vytvořte druhý OM stejným způsobem:

```bash
azure vm create \
  --resource-group myResourceGroup \
  --name myVM2 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic2 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username ops
```

A teď můžete `azure vm show myResourceGroup myVM1` příkaz podívat, co jste vytvořili. V tomto okamžiku při spouštění vaší systémem Ubuntu virtuálních za vyrovnávání zatížení v Azure, který můžete přihlášení do pouze pomocí pár klíčů SSH (protože hesla jsou zakázaná). Můžete nainstalovat nginx nebo httpd, nasadit web appu a v tématu přenos tok přes Vyrovnávání zatížení současně VMs.

```bash
azure vm show --resource-group myResourceGroup --name myVM1
```

Výstup:

```bash
info:    Executing command vm show
+ Looking up the VM "TestVM1"
+ Looking up the NIC "myNic1"
data:    Id                              :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM1
data:    ProvisioningState               :Succeeded
data:    Name                            :myVM1
data:    Location                        :westeurope
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :16.04.0-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clib45a8b650f4428a1-os-1471973896525
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://mystorageaccount.blob.core.windows.net/vhds/clib45a8b650f4428a1-os-1471973896525.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM1
data:      User Name                     :ops
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-24-D4-AA
data:          Provisioning State        :Succeeded
data:          Name                      :LmyNic1
data:          Location                  :westeurope
data:
data:    AvailabilitySet:
data:      Id                            :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/availabilitySets/myAvailabilitySet
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://mystorageaccount.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm show command OK

```


## <a name="export-the-environment-as-a-template"></a>Export prostředí jako šablony
Teď, když jste vytvořili, toto prostředí, co dělat, když chcete vytvořit další vývojové prostředí stejné parametry nebo provozním prostředí, která odpovídá to? Správce prostředků používá JSON šablony, které definují všechny parametry ve vašem prostředí. Můžete vytvořit, celý prostředí odkazování na této šabloně JSON. Můžete [ručně vytvořit JSON šablon](../resource-group-authoring-templates.md) nebo exportovat existující prostředí chcete vytvořit šablonu JSON za vás:

```bash
azure group export --name myResourceGroup
```

Tento příkaz vytvoří `myResourceGroup.json` souboru v aktuální pracovní adresář. Při vytváření prostředí pomocí této šablony se zobrazí výzva pro všechny názvů zdrojů, včetně názvu pro vyrovnávání zatížení, síťových rozhraní nebo VMs. Tyto názvy v souboru šablony můžete naplnit přidáním `-p` nebo `--includeParameterDefaultValue` parametr `azure group export` příkaz uvedená výše. Úprava šablony JSON určit názvy zdrojů nebo [Vytvoření souboru parameters.json](../resource-group-authoring-templates.md#parameters) , která určuje názvy zdrojů.

Vytvoření prostředí z vlastní šablony:

```bash
azure group deployment create --resource-group myNewResourceGroup \
  --template-file myResourceGroup.json
```

Můžete chtít přečtěte si [víc o tom, jak implementovat ze šablony](../resource-group-template-deploy-cli.md). Informace o tom, jak postupně aktualizovat prostředí, pomocí souboru parametrů a přistupovat k šablonám z jednoho úložiště.

## <a name="next-steps"></a>Další kroky

Teď jste připraveni začít pracovat s několika síťové součásti a VMs. Vytvoření aplikace pomocí základní součásti zavádí Tady můžete použít tato ukázková prostředí.
