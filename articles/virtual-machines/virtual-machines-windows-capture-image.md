<properties
    pageTitle="Zachycení obrázku OM z generalized OM Azure | Microsoft Azure"
    description="Zjistěte, jak můžete udělat snímek OM z generalized OM Azure vytvoření v modelu nasazení Správce prostředků"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="cynthn"/>

# <a name="how-to-capture-a-vm-image-from-a-generalized-azure-vm"></a>Jak zachytit OM z generalized OM Azure


V tomto článku se dozvíte, jak pomocí Powershellu Azure vytvořit obrázek generalized OM Azure. Vytvoření jiného OM můžete obrázek. Obrázek obsahuje disku OS a disků dat, které jsou připojené k virtuální počítač. Obrázek neobsahuje zdroje virtuální sítě, budete muset nastavit tyto materiály při vytváření nové OM. 


## <a name="prerequisites"></a>Zjistit předpoklady pro

- Potřebujete už [generalized OM](virtual-machines-windows-generalize-vhd.md). Proces generalizace virtuálního počítače odeberete všechny osobní informace o účtu, mimo jiné a připraví počítač má být použit jako obrázek.

- Musíte mít verzi Azure PowerShell 1.0.x nebo novější. Už jste nenainstalovali Powershellu, najdete v tématu [instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md) postup instalace.


## <a name="log-in-to-azure-powershell"></a>Přihlaste se k prostředí PowerShell Azure

1. Otevřete Azure PowerShell a přihlaste se k účtu Azure.

    ```powershell
    Login-AzureRmAccount
    ```

    Automaticky otevírané okno otevře k zadání přihlašovacích údajů účet Azure.

2. Získejte předplatné ID pro předplatné k dispozici.

    ```powershell
    Get-AzureRmSubscription
    ```

3. Nastavit správné předplatného pomocí ID předplatného.

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="deallocate-the-vm-and-set-the-state-to-generalized"></a>Zrušit OM a nastavit stav na generalized       

1. Zrušit OM zdroje.

    ```powershell
    Stop-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName>
    ```

    *Stav* OM Azure portálu změní z **Zastaveno** **Zastaveno (uvolnit)**.

2. Nastavte stav virtuálního počítače na **Generalized**. 

    ```powershell
    Set-AzureRmVm -ResourceGroupName <resourceGroup> -Name <vmName> -Generalized
    ```

3. Kontrola stavu OM. V části OM **OSState/generalized** by měla být **DisplayStatus** nastavena na **OM generalized**.  

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName> -Status
    $vm.Statuses
    ```

## <a name="create-the-image"></a>Vytvoření obrázku 

1. Zkopírujte obrázek virtuálního počítače do kontejneru cílového úložiště pomocí tohoto příkazu. Obrázek se vytvoří ve stejném účtu úložiště jako původní virtuálního počítače. `-Path` Parametr uloží kopii JSON šablony místně. `-DestinationContainerName` Parametr je název, který chcete podržte obrázků kontejneru. Pokud kontejneru neexistuje, vytvoří se za vás.

    ```powershell
    Save-AzureRmVMImage -ResourceGroupName <resourceGroupName> -Name <vmName> `
        -DestinationContainerName <destinationContainerName> -VHDNamePrefix <templateNamePrefix> `
        -Path <C:\local\Filepath\Filename.json>
    ```

    Získat adresu URL obrázku ze souboru šablony JSON. Přejděte na **zdroje** > **storageProfile** > **osDisk** > **Obrázek** > **uri** část pro úplnou cestu obrázek. Adresa URL obrázku vypadá: `https://<storageAccountName>.blob.core.windows.net/system/Microsoft.Compute/Images/<imagesContainer>/<templatePrefix-osDisk>.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`.
    
    Můžete taky ověřit URI na portálu. Obrázek se kopírují do kontejneru s názvem **systém** ve vašem účtu úložiště. 


## <a name="next-steps"></a>Další kroky

- Teď můžete [vytvořit OM z obrázku](virtual-machines-windows-create-vm-generalized.md).

