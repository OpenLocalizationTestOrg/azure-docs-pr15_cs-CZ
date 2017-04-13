<properties
    pageTitle="Vytvoření kopie specializované OM v Azure | Microsoft Azure"
    description="Naučte se vytvořit kopii specializované OM Windows spuštěné v Azure, v modelu nasazení Správce prostředků."
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
    
    
    
# <a name="create-a-copy-of-a-specialized-windows-vm-running-in-azure"></a>Vytvoření kopie specializované OM Windows spuštěné v Azure 

Tento článek ukazuje, jak můžete vytvořit kopii virtuálního pevného disku z specializované OM Windows, na kterém běží v Azure pomocí nástroje AZCopy. Pak můžete kopírovat virtuálního pevného disku k vytvoření nové OM. 

- Pokud chcete zkopírovat generalized OM, najdete v tématu [Vytvoření OM obrázek z existující generalized OM Azure](virtual-machines-windows-capture-image.md).

- Pokud chcete nahrát virtuálního pevného disku z místního OM, jako jsou vytvořené pomocí Hyper-V, najdete v tématu [nahrání virtuálního pevného disku Windows z místního OM na Azure](virtual-machines-windows-upload-image.md).


## <a name="before-you-begin"></a>Než začnete

Ujistěte se, že jste:

- Máte informace o **účty zdrojového a cílového úložiště**. Pro tento zdroj OM budete muset názvy účtu a kontejneru úložiště. Obvykle se jméno container budou **VHD**. Taky musíte mít účet cílového úložiště. Pokud už nemáte, můžete vytvořit jednu buď na portálu (**Další služby** > úložiště účty > Přidat) nebo pomocí rutinu [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) . 

- Máte Azure [PowerShell 1.0](../powershell-install-configure.md) (nebo novější) nainstalovaný.

- Stažení a instalaci [AzCopy nástroj](../storage/storage-use-azcopy.md). 


## <a name="deallocate-the-vm"></a>Zrušit OM

Zrušit OM, které uvolní virtuální pevný disk zkopírovala. 

- **Portál**: klikněte na **virtuálních počítačích** > **myVM** > Zastavit
- **Prostředí PowerShell**: `Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM` zruší přidělení OM s názvem **myVM** zdrojů skupina **myResourceGroup**.

**Stav** OM Azure portálu změní z **Zastaveno** **Zastaveno (uvolnit)**.


## <a name="get-the-storage-account-urls"></a>Získání adresy URL účtu úložiště

Je třeba adresy URL účty zdrojového a cílového úložiště. Adresy URL vypadá: `https://<storageaccount>.blob.core.windows.net/<containerName>/`. Pokud znáte název účtu a kontejneru úložiště, můžete jednoduše nahradit informace v hranatých závorkách vytvořit svoji adresu URL. 

Azure portál nebo Azure Powershell slouží k získání adresy URL:

- **Portál**: klikněte na **Další služby** > **úložiště účty**  >  <storage account> **objektů BLOB** a zdrojový soubor virtuálního pevného disku je pravděpodobně v kontejneru **VHD** . Klikněte na **Vlastnosti** kontejneru a zkopírujte tak text s označením **adresu URL**. Musíte mít adresy URL zdrojová i cílová kontejnery. 

- **Prostředí PowerShell**: `Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"` získá informace pro OM s názvem **myVM** skupina zdroje **myResourceGroup**. Ve výsledcích hledejte v části **úložiště profilu** **Uri virtuální pevný disk**. První část identifikátor Uri je adresa URL kontejneru a poslední část je název OS virtuální pevný disk OM.

## <a name="get-the-storage-access-keys"></a>Získání úložiště přístupových kláves

Vyhledejte přístupových kláves zdrojová i cílová úložiště účty. Další informace o přístupových kláves z verze najdete v tématu [o Azure úložiště účty](../storage/storage-create-storage-account.md).

- **Portál**: klikněte na **Další služby** > **úložiště účty**  >  <storage account> **Všechna nastavení** > **přístupových kláves z verze**. Zkopírujte klávesu textem je označená jako **klíč1**.
- **Prostředí PowerShell**: `Get-AzureRmStorageAccountKey -Name mystorageaccount -ResourceGroupName myResourceGroup` ní může vstoupit úložiště klíč účtu úložiště **mystorageaccount** skupina zdroje **myResourceGroup**. Zkopírujte klávesu textem je označená jako **klíč1**.


## <a name="copy-the-vhd"></a>Zkopírujte virtuálního pevného disku 

Můžete zkopírovat soubory mezi účty úložiště pomocí AzCopy. Cíl kontejneru Pokud zadaný kontejner neexistuje, bude vytvořena za vás. 

Pokud chcete použít AzCopy, otevřete okno příkazového řádku v místním počítači a přejděte do složky, kde je nainstalovaný AzCopy. Bude vypadat podobně jako *C:\Program Files (x86) \Microsoft SDKs\Azure\AzCopy*. 

Zkopírujte všechny soubory v kontejneru, použijte přepínač **/S** . Lze použít ke kopírování OS virtuálního pevného disku a všech dat disků Pokud jsou ve stejném kontejneru. Tento příklad ukazuje, jak zkopírujte všechny soubory v kontejneru **mysourcecontainer** v účtu úložiště **mysourcestorageaccount** do kontejneru **mydestinationcontainer** účtu úložiště **mydestinationstorageaccount** . Nahraďte názvy účtů úložiště a kontejnery vlastní. Nahrazení `<sourceStorageAccountKey1>` a `<destinationStorageAccountKey1>` s vlastní klíče.

```
    AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
        /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
        /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> /S
```

Pokud chcete zkopírovat konkrétní virtuální pevný disk v kontejneru s více soubory, můžete také zadat název souboru přepínačem /Pattern. V tomto příkladu se zkopírují pouze soubor s názvem **myFileName.vhd** .

```
    AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
        /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
        /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> `
        /Pattern:myFileName.vhd
```


Po skončení, dostanete zprávu, která vypadá podobně jako:

```
  Finished 2 of total 2 file(s).
  [2016/10/07 17:37:41] Transfer summary:
  -----------------
  Total files transferred: 2
  Transfer successfully:   2
  Transfer skipped:        0
  Transfer failed:         0
  Elapsed time:            00.00:13:07
```

## <a name="troubleshooting"></a>Řešení potíží

- Pokud používáte AZCopy, pokud se zobrazí chyba "Server se nepodařilo ověřit žádost. Ujistěte se, že hodnota se tak mohli ověřovat záhlaví je vytvořen správně včetně podpis. " a používáte klíč 2 nebo klávesu sekundárním úložiště, zkuste použít klávesu primární nebo 1st úložiště.


## <a name="next-steps"></a>Další kroky

- Vytvoření nové OM připojením [kopii virtuální pevný disk angličtině jako disk s operačním systémem](virtual-machines-windows-create-vm-specialized.md).












