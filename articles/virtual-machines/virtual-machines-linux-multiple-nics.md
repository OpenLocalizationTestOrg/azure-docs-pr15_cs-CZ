<properties
   pageTitle="Vytvoření Linux OM s více nic | Microsoft Azure"
   description="Naučte se vytvářet Linux OM s více nic připojená pomocí šablon Azure rozhraní příkazového řádku nebo správce prostředků."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="creating-a-linux-vm-with-multiple-nics"></a>Vytváření Linux OM s více nic
Vytvoření virtuálního počítače (OM) v Azure obsahující více virtuální sítě rozhraní (NIC) připojené k němu. Běžné situace bude mít různých podsítí pro připojení front-end a back-end nebo sítě snaží o sledování nebo záložní řešení. Tento článek obsahuje rychlý příkazy k vytvoření virtuálního počítače s více nic připojené k němu. Podrobné informace, včetně jak vytvořit víc nic ve vlastních flám skriptů, přečtěte si další informace o [nasazení více NIC VMs](../virtual-network/virtual-network-deploy-multinic-arm-cli.md). Různé [formáty OM](virtual-machines-linux-sizes.md) podporují různý počet nic, tak velikost vaší OM příslušným způsobem.

>[AZURE.WARNING] Při vytváření virtuálního počítače – nic nelze přidat do existující OM, je nutné připojit víc nic. Můžete [vytvořit OM založený na původní virtuální discích](virtual-machines-linux-copy-vm.md) a vytvořit více nic nasadit OM.

## <a name="quick-commands"></a>Rychlé příkazy
Ujistěte se, jestli máte [Rozhraní příkazového řádku Azure](../xplat-cli-install.md) přihlášení a správce prostředků v režimu:

```bash
azure config mode arm
```

V následujících příkladech nahraďte názvy parametrů příklad vlastní hodnoty. Názvy parametrů příklad zahrnuté `myResourceGroup`, `mystorageaccount`, a `myVM`.

Vytvoření skupiny zdrojů. Následující příklad vytvoří skupinu zdroje s názvem `myResourceGroup` v `WestUS` umístění:

```bash
azure group create myResourceGroup -l WestUS
```

Vytvoření účtu úložiště pro uložení vaší VMs. Následující příklad vytvoří úložiště účet s názvem `mystorageaccount`:

```bash
azure storage account create mystorageaccount -g myResourceGroup \
    -l WestUS --kind Storage --sku-name PLRS
```

Vytvořte virtuální síťové připojení vaší VMs. Následující příklad vytvoří virtuální sítě s názvem `myVnet` předponou adresu z `192.168.0.0/16`:

```bash
azure network vnet create -g myResourceGroup -l WestUS \
    -n myVnet -a 192.168.0.0/16
```

Vytvoření dvou virtuální podsítí – jeden pro front-end komunikaci a jedno pro přenos back-end. Následující příklad vytvoří dva podsítí s názvem `mySubnetFrontEnd` a `mySubnetBackEnd`:

```bash
azure network vnet subnet create -g myResourceGroup -e myVnet \
    -n mySubnetFrontEnd -a 192.168.1.0/24
azure network vnet subnet create -g myResourceGroup -e myVnet \
    -n mySubnetBackEnd -a 192.168.2.0/24
```

Vytvořte dva nic připojení jeden NIC front-end podsítě a jeden NIC podsítě back-end. Následující příklad vytvoří dva nic s názvem `myNic1` a `myNic2`a připojuje k vaší podsítě:

```bash
azure network nic create -g myResourceGroup -l WestUS \
    -n myNic1 -m myVnet -k mySubnetFrontEnd
azure network nic create -g myResourceGroup -l WestUS \
    -n myNic2 -m myVnet -k mySubnetBackEnd
```

Nakonec vytvořte OM, dvě nic dříve vytvořené připojení. Následující příklad vytvoří OM s názvem `myVM`:

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location WestUS \
    --os-type linux \
    --nic-names myNic1,myNic2 \
    --vm-size Standard_DS2_v2 \
    --storage-account-name mystorageaccount \
    --image-urn UbuntuLTS \
    --admin-username ops \
    --ssh-publickey-file ~/.ssh/id_rsa.pub
```

## <a name="creating-multiple-nics-using-azure-cli"></a>Vytvoření více nic pomocí rozhraní příkazového řádku Azure
Pokud jste dříve vytvořili OM pomocí rozhraní příkazového řádku Azure, třeba znát rychlé příkazy. Proces je podobně se dají vytvořit NIC jeden nebo více nic. Přečtěte si další informace o [nasazení více nic pomocí rozhraní příkazového řádku Azure](../virtual-network/virtual-network-deploy-multinic-arm-cli.md), včetně skriptování proces opakování prostřednictvím vytvořit všechny nic.

Následující příklad vytvoří dva nic s názvem `myNic1` a `myNic2`, s jeden NIC připojení ke každé podsítě:

```bash
azure network nic create --resource-group myResourceGroup --location WestUS \
    -n myNic1 --subnet-vnet-name myVnet --subnet-name mySubnetFrontEnd
azure network nic create --resource-group myResourceGroup --location WestUS \
    -n myNic2 --subnet-vnet-name myVnet --subnet-name mySubnetBackEnd
```

Obvykle také vytvoříte [Skupiny zabezpečení sítě](../virtual-network/virtual-networks-nsg.md) nebo [Služba Vyrovnávání zatížení](../load-balancer/load-balancer-overview.md) umožňuje spravovat a distribuovat přenosy přes svůj VMs. Znovu příkazy jsou stejné při práci s několika nic. Následující příklad vytvoří skupinu zabezpečení sítě s názvem `myNetworkSecurityGroup`:

```bash
azure network nsg create --resource-group myResourceGroup --location WestUS \
    --name myNetworkSecurityGroup
```

Svázat pomocí skupiny zabezpečení v síti vaší nic `azure network nic set`. Následující příklad vytvoří vazbu `myNic1` a `myNic2` s `myNetworkSecurityGroup`:

```bashazure 
azure network nic set --resource-group myResourceGroup --name myNic1 \
    --network-security-group-name myNetworkSecurityGroup
azure network nic set --resource-group myResourceGroup --name myNic2 \
    --network-security-group-name myNetworkSecurityGroup
```

Při vytváření OM, teď zadat více nic. Místo použití `--nic-name` zajistit jednoho NIC místo použijte `--nic-names` a zadejte hodnoty oddělené seznam nic. Budete potřebovat starat po výběru OM velikost. Existuje limity pro celkový počet nic přidané do virtuálního počítače. Další informace o [velikosti OM Linux](virtual-machines-linux-sizes.md). Následující příklad ukazuje, jak zadat víc nic a potom velikost OM, která podporuje pomocí několika nic (`Standard_DS2_v2`):

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location WestUS \
    --os-type linux \
    --nic-names myNic1,myNic2 \
    --vm-size Standard_DS2_v2 \
    --storage-account-name mystorageaccount \
    --image-urn UbuntuLTS \
    --admin-username ops \
    --ssh-publickey-file ~/.ssh/id_rsa.pub
```

## <a name="creating-multiple-nics-using-resource-manager-templates"></a>Vytvoření více nic pomocí Správce prostředků šablon
Azure správce prostředků šablony pomocí deklarativní soubory JSON prostředí definovat. Přečtěte si [informace o nástroji Azure zdroje správce](../azure-resource-manager/resource-group-overview.md). Správce prostředků šablony poskytují způsob, jak vytvořit víc instancí zdroj během nasazení, jako jsou vytváření více nic. Chcete-li zadat počet instancí vytvořit použít *kopírování* :

```bash
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

Další informace o [vytváření více instancí *zkopírováním*](../resource-group-create-multiple.md). 

Můžete taky použít `copyIndex()` přidání čísla do název zdroje, který umožňuje vytvářet `myNic1`, `myNic2`atd. Na následujícím obrázku je příklad přidávání hodnota indexu:

```bash
"name": "[concat('myNic', copyIndex())]", 
```

Přečtěte si kompletní příklad [vytváření více nic pomocí šablon správce prostředků](../virtual-network/virtual-network-deploy-multinic-arm-template.md).

## <a name="next-steps"></a>Další kroky
Zkontrolujte, že při pokusu o vytvoření virtuálního počítače s více nic kontrola [velikostí OM Linux](virtual-machines-linux-sizes.md) . Věnujte pozornost maximální počet NIC podporuje každý OM velikost. 

Myslete na to, že se nedají přidat další nic existující OM, musíte vytvořit všechny nic při nasazení OM. Dávejte při plánování nasazení a ujistěte se, že máte všechny požadované připojení k síti od začátku.