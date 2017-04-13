<properties
    pageTitle="Informace o obrázky virtuálních počítačích Windows | Microsoft Azure"
    description="Další informace o způsobu použití obrázků s virtuálních počítačích Windows v Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/21/2016"
    ms.author="cynthn"/>

# <a name="about-images-for-windows-virtual-machines"></a>Informace o obrázky virtuálních počítačích Windows

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [virtual-machines-common-classic-about-images](../../includes/virtual-machines-common-classic-about-images.md)]



## <a name="working-with-images"></a>Práce s obrázky

Modul Azure Powershellu slouží ke správě obrázky k dispozici u předplatného Azure. Taky můžete použít Azure klasické portálu pro některé úkoly obrázek, ale příkazového řádku nabídne vám další možnosti.


-   **Zobrazí všechny obrázky**:`Get-AzureVMImage`vrátí seznam všech obrázků v aktuálního předplatného k dispozici: obrázků jakož i osoby poskytovaných Azure nebo partnery. To znamená, že se může zobrazit s rozsáhlým seznamem. Další příklady ukazují, jak získat kratší seznam.
-   **Získat obrázek rodiny**:`Get-AzureVMImage | select ImageFamily` obdrží seznam obrázek rodiny zobrazením řetězce **ImageFamily** vlastnost.
-   **Zobrazí všechny obrázky v určité skupině**:`Get-AzureVMImage | Where-Object {$_.ImageFamily -eq $family}`
-   **Vyhledání obrázků OM**: `Get-AzureVMImage | where {(gm –InputObject $_ -Name DataDiskConfigurations) -ne $null} | Select -Property Label, ImageName` to funguje filtrováním DataDiskConfiguration vlastnost, která platí jenom pro OM obrázky. V tomto příkladu filtruje výstup pouze popisek a název obrázku.
-   **Uložení obrázku generalized**:`Save-AzureVMImage –ServiceName "myServiceName" –Name "MyVMtoCapture" –OSState "Generalized" –ImageName "MyVmImage" –ImageLabel "This is my generalized image"`
-   **Uložení obrázku specializované**:`Save-AzureVMImage –ServiceName "mySvc2" –Name "MyVMToCapture2" –ImageName "myFirstVMImageSP" –OSState "Specialized" -Verbose`
>[Azure.Tip] Parametr OSState je povinný, pokud chcete vytvořit OM obrázek, který obsahuje data disků a disk operačního systému. Pokud nepoužíváte parametr, vytvoří rutinu obrázek s operačním systémem. Hodnota parametru označuje, zda obrázek generalized nebo specializovaný, podle zda disk operačního systému připravený k opakovanému použití.
-   **Odstranění obrázku**:`Remove-AzureVMImage –ImageName "MyOldVmImage"`


## <a name="next-steps"></a>Další kroky

Můžete také [vytvořit počítače se systémem Windows na klasické portálu](virtual-machines-windows-classic-tutorial.md)

