<properties
    pageTitle="Zachycení Linux OM chcete použít jako šablonu | Microsoft Azure"
    description="Zjistěte, jak zachytit a generalize obrázek na základě Linux Azure virtuálního počítače (OM) vytvořená pomocí Správce prostředků Azure nasazení modelu."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="iainfou"/>


# <a name="capture-a-linux-virtual-machine-running-on-azure"></a>Zachycení Linux virtuální počítač se systémem Azure

Postupujte podle kroků v tomto článku generalize a zachytit virtuálního počítače Azure Linux (OM) v modelu nasazení Správce prostředků. Když generalize OM odebrat osobní účet údaje a připravit OM má být použit jako obrázek. Můžete pak můžete udělat snímek generalized virtuální pevný disk (virtuální pevný disk) pro operační systém, VHD disků připojená data a [Správce prostředků šablony](../azure-resource-manager/resource-group-overview.md) nové OM nasazení. 

Pokud chcete vytvořit VMs pomocí obrázku, nastavení sítě materiály pro každý nový OM a nasazení z zachycené obrázky virtuální pevný disk pomocí šablony (soubor JavaScript Object Notation nebo JSON,). Tímto způsobem můžete replikovat OM s jeho aktuálním konfiguraci softwaru, podobně jako použití obrázků v Azure Marketplace.

>[AZURE.TIP]Pokud chcete vytvořit kopii existující OM Linux s stavu specializované pro zálohování nebo ladění, najdete v článku [Vytvoření kopie Linux virtuálního počítače se systémem Azure](virtual-machines-linux-copy-vm.md). A pokud chcete nahrát virtuálního pevného disku Linux z místního OM, přečtěte si článek [Nahrát a vytvořte OM Linux z vlastní bitové](virtual-machines-linux-upload-vhd.md).  

## <a name="before-you-begin"></a>Než začnete

Ujistěte se, že budete splňovat následující požadavky:

* **Vytvoření v modelu nasazení Správce prostředků OM azure** – Pokud jste nevytvořili Linux OM, můžete používat [portál](virtual-machines-linux-quick-create-portal.md), [Azure rozhraní příkazového řádku](virtual-machines-linux-quick-create-cli.md)nebo [Správce prostředků šablony](virtual-machines-linux-cli-deploy-templates.md). 

    Konfigurace OM podle potřeby. Například [Přidat disků data](virtual-machines-linux-add-disk.md), používat aktualizace a instalovat aplikace. 
* **Azure rozhraní příkazového řádku** - nainstalovat [Azure rozhraní příkazového řádku](../xplat-cli-install.md) v místním počítači.

## <a name="step-1-remove-the-azure-linux-agent"></a>Krok 1: Odebrání agenta Azure Linux

Nejdřív příkaz **waagent** s parametrem **identitách** na OM Linux. Příkaz odstraní soubory a data tak, aby OM jste připraveni na proces generalizace. Podrobnosti najdete v tématu [Průvodce uživatelského agenta Linux Azure](virtual-machines-linux-agent-user-guide.md).

1. Připojení k vaší Linux OM pomocí SSH klienta.

2. V okně SSH zadejte tento příkaz:

    ```
    sudo waagent -deprovision+user
    ```
    >[AZURE.NOTE] Pouze spusťte tento příkaz OM, kterou chcete zachytit jako obrázek. Nezaručuje se, že obrázek zaškrtnuté všechny citlivých informací nebo je vhodné pro redistribuci.

3. Zadejte **y** pokračovat. Můžete přidat **– vynucení** parametr nechcete-li tento potvrzení krok.

4. Po dokončení příkazu zadejte **Ukončit**. Tento krok zavře SSH klienta.

    
## <a name="step-2-capture-the-vm"></a>Krok 2: Poznačte OM

Pomocí rozhraní příkazového řádku Azure generalize a zachytit OM. V následujících příkladech nahraďte názvy parametrů příklad vlastní hodnoty. Názvy parametrů příklad zahrnout **myResourceGroup** **myVnet**a **myVM**.

5. Z místního počítače otevřete Azure rozhraní příkazového řádku a [přihlášení k předplatnému Azure](../xplat-cli-connect.md). 

6. Zkontrolujte, jestli že jste v režimu správce prostředků.

    ```
    azure config mode arm
    ```

7. Vypnutí OM, který jste už poskytování zrušeno pomocí následujícího příkazu:

    ```
    azure vm deallocate -g MyResourceGroup -n myVM
    ```

8. Generalize OM pomocí následujícího příkazu:

    ```
    azure vm generalize -g MyResourceGroup -n myVM
    ```

9. Teď příkaz **zachytit azure OM** , která zachytí OM. V následujícím příkladu VHD obrázek ukládány s názvy začínající **MyVHDNamePrefix**a možnost **-t** Určuje cestu k šabloně **MyTemplate.json**. 

    ```
    azure vm capture MyResourceGroup MyResourceGroup MyVHDNamePrefix -t MyTemplate.json
    ```

    >[AZURE.IMPORTANT]Ve výchozím nastavení ve stejném úložišti účtu, který používá původní OM získat vytvoří v souborech obrázků virtuální pevný disk. Použijte *stejný účet úložiště* pro ukládání VHD pro všechny nové VMs vytvořit z obrázku. 

6. Nalezení zachycený obrázek, otevřete šablonu JSON v textovém editoru. V **storageProfile**najděte **uri** **Obrázek** umístěné v kontejneru **systému** . Příklad je podobný URI bitové s operačním systémem`https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`

## <a name="step-3-create-a-vm-from-the-captured-image"></a>Krok 3: Vytvoření virtuálního počítače z zachycený obrázek
Teď pomocí obrázku se šablonou vytvářet OM Linux. Tento postup předvedení použití rozhraní příkazového řádku Azure a šablonu JSON souborů, které se nezaznamenávají vytvoření OM v nové virtuální sítě.

### <a name="create-network-resources"></a>Vytvořit prostředky sítě

Chcete-li použít šablonu, musíte nejdřív nastavit virtuální sítě a NIC pro nové OM. Doporučujeme že vytvořit skupinu zdrojů pro tyto materiály na místě, kde je uložena OM obrázek. Spuštění příkazů podobně jako následující zaokrouhlením názvy zdrojů a odpovídající Azure umístění ("centralus" v příkazech):

    azure group create MyResourceGroup1 -l "centralus"

    azure network vnet create MyResourceGroup1 myVnet -l "centralus"

    azure network vnet subnet create MyResourceGroup1 myVnet mySubnet

    azure network public-ip create MyResourceGroup1 myIP -l "centralus"

    azure network nic create MyResourceGroup1 myNIC -k mySubnet -m myVnet -p myIP -l "centralus"

### <a name="get-the-id-of-the-nic"></a>Získání Id NIC

Abyste mohli nasadit OM z obrázku pomocí JSON jste si uložili během snímku, potřebujete Id síťovou Získání spuštěním následujícího příkazu:

    azure network nic show MyResourceGroup1 myNIC

**Id** do výstupu je podobný`/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic`



### <a name="create-a-vm"></a>Vytvoření virtuálního počítače
Spusťte následující příkaz a vytvořte svůj OM z zachycený obrázek OM. Použijte parametr **– f** a zadejte cestu k souboru JSON šablony, které jste uložili.

    azure group deployment create MyResourceGroup1 MyDeployment -f MyTemplate.json

Příkaz navíc zobrazí se výzva Zadejte nový název OM, správce uživatelské jméno a heslo a Id NIC jste dříve vytvořili.

    info:    Executing command group deployment create
    info:    Supply values for the following parameters
    vmName: myNewVM
    adminUserName: myAdminuser
    adminPassword: ********
    networkInterfaceId: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resource Groups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic

Následující příklad ukazuje, co vidíte na úspěšné nasazení:

    + Inicializace konfigurace šablon a parametrů
    + Vytváření nasazení informace: vytvoření šablony nasazení xxxxxxx
    + Čeká se na nasazení dokončete dat: DeploymentName: MyDeployment dat: ResourceGroupName: MyResourceGroup1 dat: ProvisioningState: úspěšně dat: časové razítko: xxxxxxx dat: režim: Přírůstková data: typ hodnoty název


    data:    ------------------  ------------  -------------------------------------

    data: vmName řetězec myNewVM


    data: vmSize Standard_D1 řetězec


    data: adminUserName řetězec myAdminuser


    data: adminPassword Nedefinováno SecureString


    data: networkInterfaceId řetězec /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic informace: nasazení skupinu vytvořit, příkaz OK

### <a name="verify-the-deployment"></a>Ověření nasazení

Teď SSH virtuálního počítače, který jste vytvořili pro ověření nasazení a začněte používat novou OM. Připojení přes SSH, zjistěte IP adresu OM jste vytvořili pomocí tohoto příkazu:

    azure network public-ip show MyResourceGroup1 myIP

Na veřejnou IP adresu je uvedený v výstup příkazu. Ve výchozím nastavení se připojíte ke OM Linux tak, že SSH na port 22.

## <a name="create-additional-vms"></a>Vytvořit další VMs
Umožňuje zachycený obrázek a šablon nasazení další VMs pomocí kroků v předchozí části. Další možnosti pro vytváření VMs z obrázku obsahovat pomocí šablony rychlý úvod a spuštění příkazu **azure OM vytvořit** .

### <a name="use-the-captured-template"></a>Použití zachycený šablony

Použití zachycený obrázek a šablony, postupujte takto (podrobně v předchozí části):

* Zajistěte, aby byl obrázek OM ve stejném úložišti účtu, který je hostitelem vaší OM virtuální pevný disk.
* Zkopírujte soubor JSON šablony a zadejte jedinečný název disku OS nové OM virtuálního pevného disku (nebo VHD). Například v **storageProfile**, klikněte v části **virtuální pevný disk**v **uri**, zadejte jedinečný název **osDisk** souboru podobně jako`https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`
* Vytvoření NIC ve stejném nebo jiném virtuální sítě.
* Pomocí změněný JSON soubor šablony můžete vytvořte nasazení ve skupině prostředků, ve kterém můžete nastavit virtuální sítě.

### <a name="use-a-quickstart-template"></a>Použít šablonu rychlý úvod

Pokud budete potřebovat sítě nastavení automaticky při vytváření virtuálního počítače z obrázku, můžete určit prostředků v šabloně. Například najdete v článku [101om z uživatele – obrázek šablony](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) z GitHub. Tato šablona vytvoří virtuálního počítače z vlastní obrázek a potřebné virtuální sítě, veřejnou IP adresu a NIC zdroje. Informace o použití šablony v portálu Azure zjistěte, [jak vytvořit virtuální počítač z vlastní obrázek použít šablonu? správce prostředků](http://codeisahighway.com/how-to-create-a-virtual-machine-from-a-custom-image-using-an-arm-template/).

### <a name="use-the-azure-vm-create-command"></a>Příkaz azure OM create slouží

Obvykle je nejjednodušší pomocí Správce prostředků šablony můžete vytvořit virtuálního počítače z obrázku. Však můžete vytvořit OM _imperativně_ pomocí příkazu **vytvořit azure OM** **-Q** (**– Obrázek urn**) parametr. Pokud tímto způsobem také předáte parametr **d** (**--os vhd disk**) k určení umístění souboru VHD OS pro nové OM. Tento soubor musí být v kontejneru VHD účtu úložiště uložení souboru s obrázkem virtuální pevný disk. Příkaz zkopíruje virtuálního pevného disku pro nové OM automaticky do kontejneru **VHD** .

Před spuštěním **azure OM vytvořit** s obrázkem, proveďte následující kroky:

1.  Vytvoření skupiny prostředků nebo určit existující skupiny prostředků pro nasazení.

2.  Vytvoření veřejné prostředek IP adresy a NIC zdroje pro nové OM. Postup vytvoření virtuální sítě, veřejnou IP adresu a NIC pomocí rozhraní příkazového řádku najdete v tématech dříve v tomto článku. (**Vytvoření azure OM** můžete taky vytvořit NIC, ale potřebujete předat další parametry virtuální sítě a podsítě.)


Spusťte příkaz, který URI předává nový soubor s operačním systémem virtuálního pevného disku a existující obrázek. V tomto příkladu je vytvořen velikostí Standard_A1 OM v oblasti východoasijských USA.

    azure vm create MyResourceGroup1 myNewVM eastus Linux -d "https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix.vhd" -Q "https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd" -z Standard_A1 -u myAdminname -p myPassword -f myNIC

Další příkaz Možnosti spustit `azure help vm create`.

## <a name="next-steps"></a>Další kroky

Ke správě VMs s rozhraní příkazového řádku, najdete v tématu úkoly v [nasazení a správu virtuálních počítačích pomocí Správce prostředků Azure šablony a rozhraní příkazového řádku Azure](virtual-machines-linux-cli-deploy-templates.md).
