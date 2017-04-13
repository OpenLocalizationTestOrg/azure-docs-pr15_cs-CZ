<properties
    pageTitle="Vytvořit kopii vaší OM Linux Azure | Microsoft Azure"
    description="Naučte se vytvářet kopii virtuálního počítače Azure Linux v modelu nasazení Správce prostředků"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="cynthn"/>

# <a name="create-a-copy-of-a-linux-virtual-machine-running-on-azure"></a>Vytvoření kopie Linux virtuální počítač se systémem Azure


Tento článek popisuje, jak vytvořit kopii Azure virtuálního počítače (OM) systémem Linux pomocí nasazení modelu správce prostředků. Nejprve zkopírovat operačního systému a disků dat do kontejneru nový a pak nastavení sítě zdroje a vytvoření nového virtuálního zařízení.

Můžete také [ukládat a vytvářet OM z vlastní disku obrázku](virtual-machines-linux-upload-vhd.md).


## <a name="before-you-begin"></a>Než začnete

Zajistěte, než začnete kroky splnit tyto požadavky:

- Máte [Azure rozhraní příkazového řádku] (... / xplat rozhraní příkazového řádku install.md) stáhnout a nainstalovat do počítače. 

- Taky musíte některé informace o vaší stávající Linux OM Azure:

| Informace o zdroji OM | Kde ho získat |
|------------|-----------------|
| Název OM | `azure vm list` |
| Název pole Skupina zdroje | `azure vm list` |
| Umístění | `azure vm list` |
| Název účtu úložiště | `azure storage account list -g <resourceGroup>` |
| Název kontejneru | `azure storage container list -a <sourcestorageaccountname>` |
| Název zdrojového souboru OM virtuální pevný disk | `azure storage blob list --container <containerName>` |



- Budete potřebovat provést několik možností o nové OM:   <br> -Název kontejneru   <br> Název - OM   <br> Velikost - OM   <br> Název - vNet   <br> -Název podsítě   <br> Název - IP   <br> NIC – název
    

## <a name="login-and-set-your-subscription"></a>Přihlášení a nastavení vašeho předplatného

1. Přihlaste se ke rozhraní příkazového řádku.
        
        azure login

2. Zkontrolujte, jestli že jste v režimu správce prostředků.
    
        azure config mode arm

3. Nastavte správné předplatného. Můžete použít účet azure seznamu zobrazíte všechny vaše předplatné.

        azure account set <SubscriptionId>



## <a name="stop-the-vm"></a>Ukončení OM 

Ukončení a zrušit zdroji OM. Můžete azure OM seznamu zobrazíte seznam všech VMs předplatného a jejich zdroje názvy skupin.
    
        azure vm stop <ResourceGroup> <VmName>
        azure vm deallocate <ResourceGroup> <VmName>




## <a name="copy-the-vhd"></a>Zkopírujte virtuálního pevného disku


Zkopírujte virtuálního pevného disku z úložiště zdroj k určení pomocí `azure storage blob copy start`. V tomto příkladu budeme zkopírujte virtuálního pevného disku do jednoho účtu úložiště, ale jiný kontejner.

Zkopírovat virtuální pevný disk na jiný kontejner ve stejném účtu úložiště, zadejte:

        azure storage blob copy start https://<sourceStorageAccountName>.blob.core.windows.net:8080/<sourceContainerName>/<SourceVHDFileName.vhd> <newcontainerName>
        

## <a name="set-up-the-virtual-network-for-your-new-vm"></a>Nastavte virtuální síť pro nové OM

Nastavte si virtuální sítě a NIC nového OM. 

    azure network vnet create <ResourceGroupName> <VnetName> -l <Location>

    azure network vnet subnet create -a <address.prefix.in.CIDR/format> <ResourceGroupName> <VnetName> <SubnetName>

    azure network public-ip create <ResourceGroupName> <IpName> -l <yourLocation>

    azure network nic create <ResourceGroupName> <NicName> -k <SubnetName> -m <VnetName> -p <IpName> -l <Location>


## <a name="create-the-new-vm"></a>Vytvoření nového OM 

Teď můžete vytvářet virtuálního počítače z nahraný virtuální disk [pomocí šablony správce zdrojů](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) nebo prostřednictvím rozhraní příkazového řádku zadáním identifikátor URI zkopírovaný disku zadáním:

```bash
azure vm create -n <newVMName> -l "<location>" -g <resourceGroup> -f <newNicName> -z "<vmSize>" -d https://<storageAccountName>.blob.core.windows.net/<containerName/<fileName.vhd> -y Linux
```



## <a name="next-steps"></a>Další kroky

Naučte se používat rozhraní příkazového řádku Azure ke správě nové virtuálního počítače, najdete v článku [Příkazy Azure rozhraní příkazového řádku pro správce prostředků Azure](azure-cli-arm-commands.md).
