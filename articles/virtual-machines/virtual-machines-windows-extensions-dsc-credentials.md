<properties
   pageTitle="Předání pověření Azure pomocí DSC | Microsoft Azure"
   description="Základní informace o bezpečně předávání přihlašovací údaje k Azure virtuálních počítačích pomocí prostředí PowerShell žádoucí konfigurace stavu"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="zjalexander"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"
   keywords=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="09/15/2016"
   ms.author="zachal"/>

# <a name="passing-credentials-to-the-azure-dsc-extension-handler"></a>Předání pověření rutiny rozšíření Azure DSC #

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Tento článek popisuje koncovku žádoucí konfigurace stavu pro Azure. Základní informace o rutiny rozšíření DSC najdete na [Úvod do rozšíření rutiny konfigurace stavu požadované Azure](virtual-machines-windows-extensions-dsc-overview.md). 


## <a name="passing-in-credentials"></a>Předávání v přihlašovací údaje
Jako součást procesu konfigurace můžete třeba nastavit uživatelských účtů přístup ke službám nebo instalace programu v kontextu uživatele. Provádět tyto kroky, budete muset zadat pověření. 

DSC umožňuje parametry konfigurace, ve kterých jsou předaným konfiguraci a bezpečně uložené v souborech MOF přihlašovací údaje. Obslužná rutina rozšíření Azure zjednodušuje Správa pověření zadáním Automatická správa certifikátů. 

Zkuste tento DSC konfigurace skript, který vytvoří místní uživatelský účet pomocí zadaného hesla:

*user_configuration.ps1*

```
configuration Main
{
    param(
        [Parameter(Mandatory=$true)]
        [ValidateNotNullorEmpty()]
        [PSCredential]
        $Credential
    )    
    Node localhost {       
        User LocalUserAccount
        {
            Username = $Credential.UserName
            Password = $Credential
            Disabled = $false
            Ensure = "Present"
            FullName = "Local User Account"
            Description = "Local User Account"
            PasswordNeverExpires = $true
        } 
    }  
} 
```

Je důležité *uzel localhost* jako součást zahrnout konfigurace. Pokud tento příkaz chybí, podle těchto kroků nepracuje podle obslužné rozšíření konkrétně vyhledá příkazu localhost uzel. Je také třeba zahrnout typecast *[PsCredential]*podle určitého typu spustí koncovku šifrování pověření. 

Publikujte tento skript k úložišti objektů blob:

`Publish-AzureVMDscConfiguration -ConfigurationPath .\user_configuration.ps1`

Nastavení rozšíření Azure DSC a zadat pověření:

```
$configurationName = "Main"
$configurationArguments = @{ Credential = Get-Credential }
$configurationArchive = "user_configuration.ps1.zip"
$vm = Get-AzureVM "example-1"
 
$vm = Set-AzureVMDSCExtension -VM $vm -ConfigurationArchive $configurationArchive 
-ConfigurationName $configurationName -ConfigurationArgument @configurationArguments
 
$vm | Update-AzureVM
```
## <a name="how-credentials-are-secured"></a>Jak zabezpečené přihlašovací údaje
Spuštění tento kód zobrazí výzvu k pověření. Jakmile není uvedený, uloží se v paměti stručně. Když je publikován s `Set-AzureVmDscExtension` rutina, ho přenášené HTTPS angličtině, kde Azure ukládá zašifrovaných na disku, pomocí místní OM certifikát. Potom je ale stručně dešifrovat v paměti a znovu zašifrovaných předat DSC.

Toto chování se liší od [používání zabezpečené konfigurace bez rozšíření rutiny](https://msdn.microsoft.com/powershell/dsc/securemof). Azure prostředí vám bude radit způsobem předávat data konfigurace bezpečně prostřednictvím certifikáty. Pokud chcete použít rutinu rozšíření DSC, je potřeba zadat $CertificatePath nebo $CertificateID / $Thumbprint položky v ConfigurationData.


## <a name="next-steps"></a>Další kroky ##

Další informace o rutiny rozšíření Azure DSC najdete v článku [Úvod do rozšíření rutiny konfigurace stavu požadované Azure](virtual-machines-windows-extensions-dsc-overview.md). 

Prozkoumejte [Správce prostředků Azure šablony pro rozšíření DSC](virtual-machines-windows-extensions-dsc-template.md).

Další informace o prostředí PowerShell DSC, [Navštěvujte blog o Centru si přečtěte následující dokumentaci Powershellu](https://msdn.microsoft.com/powershell/dsc/overview). 

Najít další funkce můžete spravovat pomocí prostředí PowerShell DSC, [vyhledejte v galerii Powershellu](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) pro další DSC zdroje.
