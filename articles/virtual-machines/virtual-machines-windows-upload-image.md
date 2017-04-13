<properties
    pageTitle="Nahrání Windows virtuálního pevného disku pro správce prostředků | Microsoft Azure"
    description="Zjistěte, jak nahrát Windows virtuální počítač virtuální pevný disk z místního Azure pomocí nasazení modelu správce prostředků. Můžete nahrát virtuálního pevného disku z buď generalized nebo specializovaný OM."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="cynthn"/>

# <a name="upload-a-windows-vhd-from-an-on-premises-vm-to-azure"></a>Nahrání Windows virtuální z OM místní pevný disk na Azure 


V tomto článku se dozvíte, jak vytvořit a nahrát Windows virtuální pevný disk virtuální (pevný disk) pro použití při vytváření OM Azure. Můžete nahrát virtuálního pevného disku z generalized OM nebo specializovaný OM. 

Další informace o disků a VHD v Azure najdete v článku [o disků a VHD virtuálních počítačích](virtual-machines-linux-about-disks-vhds.md).


## <a name="prepare-the-vm"></a>Příprava OM 

Nahrajte generalized a specializované VHD na Azure. Každý typ vyžaduje připravit OM před spuštěním.

- **Virtuální pevný disk generalized** - generalized virtuálního pevného disku byla všech informací vaší osobní účet odebrat pomocí Sysprep. Pokud chcete použít virtuální pevný disk jako obrázek k vytvoření nové VMs z, postupujte následujícím způsobem:
    - [Příprava Windows virtuální pevný disk na Azure](virtual-machines-windows-prepare-for-upload-vhd-image.md). 
    - [Generalizace virtuálního počítače pomocí Sysprep](virtual-machines-windows-generalize-vhd.md). 

- **Speciální virtuální pevný disk** - specializované virtuálního pevného disku udržuje uživatelských účtů, aplikací a dalších dat stavu ze svého původního OM. Pokud chcete použít virtuální pevný disk jako-je chcete vytvořit nové OM, zajistit, aby v následujících krocích. 
    - [Příprava Windows virtuální pevný disk na Azure](virtual-machines-windows-prepare-for-upload-vhd-image.md). **Co nedělat** generalize OM pomocí Sysprep.
    - Odebrání nástroje virtualizace hosta a agentů, kteří instalují na OM (tedy VMware nástroje).
    - Zkontrolujte, zda že OM nakonfigurovaný na jeho IP adresa a nastavení DNS prostřednictvím DHCP. Zajistíte tak, že server získá IP adresu v rámci VNet při spuštění systému. 

## <a name="log-in-to-azure"></a>Přihlaste se k Azure

Pokud ještě nemáte prostředí PowerShell verze 1.4 nebo nad nainstalované, přečtěte si [Postup instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md).

1. Otevřete Azure PowerShell a přihlaste se k účtu Azure. Automaticky otevírané okno otevře k zadání přihlašovacích údajů účet Azure.

    ```powershell
    Login-AzureRmAccount
    ```


2. Získejte předplatné ID pro předplatné k dispozici.

    ```powershell
    Get-AzureRmSubscription
    ```

3. Nastavit správné předplatného pomocí ID předplatného. Nahrazení `<subscriptionID>` s ID správné předplatného.

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="get-the-storage-account"></a>Zřízení účtu úložiště

Je třeba úložiště účtu v Azure k ukládání nahraný OM obrázek. Můžete použít existující účet úložiště nebo vytvořte nový účet. 

Zobrazit volného účty, zadejte:

```powershell
Get-AzureRmStorageAccount
```

Pokud chcete použít existující účet úložiště, přejděte k části [Nahrát obrázek OM](#upload-the-vm-vhd-to-your-storage-account) .

Pokud je potřeba vytvořit účet úložiště, postupujte takto:

1. Je třeba název Skupina zdroje, kde by měl účtu úložiště vytvořen. Pokud chcete zjistit, všechny skupiny zdrojů, které jsou ve vašem předplatném, zadejte:

    ```powershell
    Get-AzureRmResourceGroup
    ```

Pokud chcete vytvořit skupina zdroje s názvem **myResourceGroup** v oblastí **Západní USA** , zadejte:

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. Vytvoření účtu úložiště s názvem **mystorageaccount** v této skupině zdrojů pomocí rutinu [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) :

    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" -SkuName "Standard_LRS" -Kind "Storage"
    ```
            
    Platné hodnoty – SkuName jsou:

    - **Standard_LRS** - místně nadbytečné úložiště. 
    - **Standard_ZRS** - nadbytečné úložiště zóny.
    - **Standard_GRS** - Geo nadbytečné úložiště. 
    - **Standard_RAGRS** - nadbytečné úložiště geo přístup pro čtení. 
    - **Premium_LRS** - Premium místně nadbytečné úložiště. 



## <a name="upload-the-vhd-to-your-storage-account"></a>Odeslání virtuálního pevného disku ke svému účtu úložiště

Nahrání obrázku do kontejneru ve vašem účtu úložiště získáte pomocí rutiny [AzureRmVhd přidat](https://msdn.microsoft.com/library/mt603554.aspx) . V tomto příkladu odešle soubor **myVHD.vhd** z `"C:\Users\Public\Documents\Virtual hard disks\"` úložiště účet s názvem **mystorageaccount** ve skupině **myResourceGroup** prostředků. Soubor se umístit do kontejneru s názvem **mycontainer** a nový název souboru bude **myUploadedVHD.vhd**.

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


Pokud je úspěšná, dostanete odpověď, která vypadá nějak takto:

```
  C:\> Add-AzureRmVhd -ResourceGroupName myResourceGroup -Destination https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
  MD5 hash is being calculated for the file C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd.
  MD5 hash calculation is completed.
  Elapsed time for the operation: 00:03:35
  Creating new page blob of size 53687091712...
  Elapsed time for upload: 01:12:49

  LocalFilePath           DestinationUri
  -------------           --------------
  C:\Users\Public\Doc...  https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd
```

V závislosti na vaše síťové připojení a velikost souboru virtuálního pevného disku tento příkaz může chvíli trvat k dokončení


## <a name="next-steps"></a>Další kroky

- [Vytvoření virtuálního počítače v Azure z generalized virtuální pevný disk](virtual-machines-windows-create-vm-generalized.md)
- [Vytvoření OM v Azure z specializované virtuálního pevného disku](virtual-machines-windows-create-vm-specialized.md) připojením jako s operačním systémem disku při vytváření nové OM.


