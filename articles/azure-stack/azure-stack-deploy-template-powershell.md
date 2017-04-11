<properties
    pageTitle="Nasazení šablony pomocí prostředí PowerShell ve vrstvě Azure | Microsoft Azure"
    description="Naučte se nasadit virtuálního počítače pomocí Správce prostředků šablony a Powershellu."
    services="azure-stack"
    documentationCenter=""
    authors="heathl17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="helaw"/>

# <a name="deploy-templates-in-azure-stack-using-powershell"></a>Nasazení šablon ve vrstvě Azure pomocí prostředí PowerShell

Použití Powershellu ke nasazení šablon správce prostředků Azure Koncepce zásobníku Azure.  Správce prostředků šablony nasazení a zřízení všechny zdroje pro aplikaci v operaci jediné, koordinovaný.

## <a name="run-azurerm-powershell-cmdlets"></a>Spouštět rutiny prostředí AzureRM PowerShell

V tomto příkladu spustíte skript pro nasazení virtuálního počítače do zásobníku Koncepce Azure pomocí šablony správce prostředků.  Než budete pokračovat, zajistěte, aby že jste [nainstalovali a nakonfigurovali prostředí PowerShell](azure-stack-connect-powershell.md)  

Virtuální pevný disk použít v této šabloně příkladu je výchozí obrázek marketplace (WindowsServer-2012 – R2-Datacentra).

1.  Přejděte na <http://aka.ms/AzureStackGitHub>, vyhledejte šablonu **101 jednoduché windows OM** a uložte jej v následujícím umístění: c:\\šablony\\azuredeploy-101jednoduché – windows – vm.json.

2.  V prostředí PowerShell spusťte tento skript nasazení. Nahraďte *uživatelské jméno* a *heslo* svoje uživatelské jméno a heslo. Na pozdější použití zvýšte hodnota parametru *$myNum* nechcete přepsat nasazení.

    ```PowerShell
        # Set Deployment Variables
        $myNum = "001" #Modify this per deployment
        $RGName = "myRG$myNum"
        $myLocation = "local"

        # Create Resource Group for Template Deployment
        New-AzureRmResourceGroup -Name $RGName -Location $myLocation

        # Deploy Simple IaaS Template
        New-AzureRmResourceGroupDeployment `
            -Name myDeployment$myNum `
            -ResourceGroupName $RGName `
            -TemplateFile c:\templates\azuredeploy-101-simple-windows-vm.json `
            -NewStorageAccountName mystorage$myNum `
            -DnsNameForPublicIP mydns$myNum `
            -AdminUsername <username> `
            -AdminPassword ("<password>" | ConvertTo-SecureString -AsPlainText -Force) `
            -VmName myVM$myNum `
            -WindowsOSVersion 2012-R2-Datacenter
    ```

3.  Otevřete portál zásobníku Azure, klikněte na tlačítko **Procházet**, klikněte na **virtuálních počítačích**a vyhledejte nové virtuálního počítače (*myDeployment001*).

## <a name="video-example-hybrid-virtual-machine-deployment"></a>Videa příklad: hybridní nasazení virtuálního počítače

[AZURE.VIDEO microsoft-azure-stack-tp1-poc-hybrid-vm-deployment]

## <a name="next-steps"></a>Další kroky

[Nasazení šablon aplikace Visual Studio](azure-stack-deploy-template-visual-studio.md)
