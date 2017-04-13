<properties
   pageTitle="Vyplňování stavu konfigurace Azure přehled | Microsoft Azure"
   description="Přehled použití rutiny rozšíření Microsoft Azure pro PowerShell žádoucí konfigurace stavu. Včetně požadavky, architekturu, rutiny."
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

# <a name="introduction-to-the-azure-desired-state-configuration-extension-handler"></a>Úvod do rozšíření rutiny konfigurace stavu požadované Azure #

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Agent OM Azure a související rozšíření jsou součástí služby Microsoft Azure infrastruktury. Rozšíření OM jsou softwarových součástí, které rozšířit použitelnost funkce OM a zjednodušení různých operacích správy OM. Například koncovku VMAccess mohou sloužit k resetování hesla správce nebo vlastní skript linky lze použít k provedení skriptu na OM.

Tento článek uvádí koncovku prostředí PowerShell žádoucí stavu konfigurace (DSC) pro Azure VMs jako součást SDK prostředí PowerShell Azure. Nové rutiny pro správu můžete nahrávat a konfigurace prostředí PowerShell DSC dnem Azure OM povolené s příponou DSC Powershellu. DSC prostředí PowerShell rozšíření volání do Powershellu DSC přijmout přijaté konfigurace DSC v OM. Tato funkce je také k dispozici prostřednictvím portálu Azure.

## <a name="prerequisites"></a>Zjistit předpoklady pro ##
**Místního počítače** Pokud chcete provést interakci s příponou Azure OM, budete muset používat portál Azure nebo SDK prostředí PowerShell Azure. 

**Agent hosta** Azure OM, které budou nakonfigurovány konfigurací DSC musí být operačního systému, který podporuje Windows (Management Framework WMF) 4.0 a 5.0. Úplný seznam podporovaných verzí operačního systému najdete na [Historie verzí DSC rozšíření](https://blogs.msdn.microsoft.com/powershell/2014/11/20/release-history-for-the-azure-dsc-extension/).

## <a name="terms-and-concepts"></a>Podmínky a koncepty ##
Tato příručka předpokládá znalost následující pojmy:

Konfigurace – konfigurace dokumentu DSC. 

Uzel - cílové hodnotě DSC konfigurace. V tomto dokumentu "uzel" vždy odkazuje OM Azure.

Konfigurace Data – .psd1 soubor obsahující klimatizace data pro konfiguraci

## <a name="architectural-overview"></a>Přehled ##

Rozšíření Azure DSC pomocí framework Azure OM Agent poskytovat, přijmout a informování o DSC konfigurace spuštěna Azure VMs. Rozšíření DSC předpokládá, že soubor ZIP obsahující alespoň konfigurace dokumentu a nastavení parametrů pomocí prostředí PowerShell SDK Azure nebo pomocí portálu Azure.

Při volání rozšíření poprvé spustí proces instalace. Tento proces instalace verze aplikace Management Framework WMF (Windows) pomocí následujícího postupu:

1. Když je OS OM Azure Windows serveru 2016, žádná akce. Windows serveru 2016 už je nejnovější verze prostředí PowerShell nainstalovaný.
2. Pokud `wmfVersion` vlastnost není zadán, pokud není kompatibilní s operačním systémem OM je už nainstalovaná této verze WMF.
3. Pokud ne `wmfVersion` vlastnost není zadán, máte nainstalovanou nejnovější příslušných verzi WMF.

Instalace WMF vyžaduje restartování. Po restartování počítače, rozšíření ke stažení souboru ZIP podle `modulesUrl` vlastnost. Pokud je toto umístění v úložišti objektů blob Azure, token přidružení zabezpečení může být zadán v `sasToken` vlastnost pro přístup k souboru. Po stažení a rozbalili ZIP funkci konfigurace podle `configurationFunction` je spuštěn pro vytvoření souboru MOF. Rozšíření spustí `Start-DscConfiguration -Force` na generované MOF. Rozšíření zaznamenává výstup a zapisuje zpět stav kanálu Azure. DSC LCM od této chvíle, zpracuje sledování a oprav jako normálně. 

## <a name="powershell-cmdlets"></a>Rutiny prostředí PowerShell ##

Rutiny prostředí PowerShell můžete používat se ARM a ASM balíček, publikovat a sledovat DSC rozšíření nasazení. Tyto rutiny uvedené jsou ASM moduly, ale jde nahradit "Azure" s "AzureRm" použití ARM modelu. Například `Publish-AzureVMDscConfiguration` používá ASM, kde `Publish-AzureRmVMDscConfiguration` používá ARM. 

`Publish-AzureVMDscConfiguration`Přenese v souboru konfigurace, vyhledává závislé DSC prostředky a vytvoří soubor ZIP obsahujícího konfigurace a DSC prostředky potřebné k přijmout konfiguraci. Můžete taky vytvořit balíček místně pomocí `-ConfigurationArchivePath` parametr. V opačném publikuje souboru ZIP k úložišti objektů blob Azure a zabezpečené s tokem přidružení zabezpečení.

ZIP soubor vytvořený tuto rutinu má skript konfiguraci .ps1 na kořenovou složku pro archivaci. Zdroje mají složce modul umístí do složky archivu. 

`Set-AzureVMDscExtension`Vloží nastavení potřeby tak, že přípona prostředí PowerShell DSC do objekt konfigurace OM, které lze použít k Azure OM s `Update-AzureVM`.

`Get-AzureVMDscExtension`Obnoví stav rozšíření DSC konkrétní OM. 

`Get-AzureVMDscExtensionStatus`Obnoví stav konfigurace DSC použity rozšíření rutinou DSC. Tato akce lze provést na jeden OM nebo okruhem VMs.

`Remove-AzureVMDscExtension`Odebere rutinu rozšíření od dané virtuálního počítače. Tato rutina slouží **Odebrat konfigurace,** odinstalujte WMF nebo změnit použité nastavení počítače virtuální. Odebere jenom rutinu rozšíření. 

**Důležité rozdíly v ASM a ARM rutiny**

- Rutiny pro ARM jsou synchronní. Rutiny pro ASM jsou asynchronní.
- ResourceGroupName VMName, ArchiveStorageAccountName, verze a umístění jsou všechny nové požadované parametry.
- ArchiveResourceGroupName je nový volitelné parametr ARM. Tento parametr můžete určit, pokud účtu úložiště patří do skupiny různých zdrojů než tam, kde je vytvořili virtuální počítač.
- ConfigurationArchive nazývá ArchiveBlobName v ARM
- Název_kontejneru nazývá ArchiveContainerName v ARM
- StorageEndpointSuffix nazývá ArchiveStorageEndpointSuffix v ARM
- Přepínačem AutoUpdate přidala ARM povolení automatické aktualizace rozšíření rutiny na nejnovější verzi jako a není k dispozici. Uvědomte si tento parametr se může způsobit, že se restartuje na OM vydána nová verze WMF. 


## <a name="azure-portal-functionality"></a>Azure portálu funkce ##
Přejděte na klasické OM. V části Nastavení -> Obecné klikněte na "Rozšíření". Nové podokno se vytvoří. Klikněte na "Přidat" a vyberte DSC Powershellu.

Na portálu musí vstupní.
**Konfigurace moduly nebo skript**: Toto pole není povinné. Vyžaduje .ps1 soubor obsahující konfigurační skript nebo soubor ZIP se skriptem konfigurace .ps1 v kořenovém a všechny závislá zdroje v modulu podsložek ZIP. Lze vytvořit pomocí `Publish-AzureVMDscConfiguration -ConfigurationArchivePath` rutina součástí SDK prostředí PowerShell Azure. Soubor ZIP uloženy v úložišti objektů blob uživatele zajištěná token přidružení zabezpečení. 

**Konfigurační soubor PSD1 dat**: Toto pole není povinné. Pokud konfigurace vyžaduje konfiguraci datového souboru v .psd1, toto pole využít vybrat a ho nahrajte do úložiště objektů blob uživatele, kde je zabezpečená tak, že token přidružení zabezpečení. 
 
**Module-Qualified název konfigurace**: .ps1 soubory můžete mít několik funkcí konfigurace. Zadejte název skript .ps1 konfiguraci a za ním uveďte "\' a název funkce konfigurace. Například pokud skript .ps1 má název "configuration.ps1" a konfigurace je "IisInstall", který byste zadali:`configuration.ps1\IisInstall`

**Konfigurace argumentů**: Pokud funkce Konfigurace trvá argumenty, zadejte je zde ve formátu `argumentName1=value1,argumentName2=value2`. Poznámka: Tento formát je v jiném formátu než způsob konfigurace argumenty přijaté pomocí rutin prostředí PowerShell nebo správce prostředků šablony. 

## <a name="getting-started"></a>Začínáme ##

Rozšíření Azure DSC zabírá v dokumentech konfigurace DSC a představuje na Azure VMs. Jednoduchý příklad konfigurace sleduje. Uložte místně jako "IisInstall.ps1":

```powershell
configuration IISInstall 
{ 
    node "localhost"
    { 
        WindowsFeature IIS 
        { 
            Ensure = "Present" 
            Name = "Web-Server"                       
        } 
    } 
}
```

Podle těchto kroků skript IisInstall.ps1 umístit na zadaný OM, spouštění konfiguraci a zpět hlášení o stavu.
 
```powershell
#Azure PowerShell cmdlets are required
Import-Module Azure

#Use an existing Azure Virtual Machine, 'DscDemo1'
$demoVM = Get-AzureVM DscDemo1

#Publish the configuration script into user storage.
Publish-AzureVMDscConfiguration -ConfigurationPath ".\IisInstall.ps1" -StorageContext $storageContext -Verbose -Force

#Set the VM to run the DSC configuration
Set-AzureVMDscExtension -VM $demoVM -ConfigurationArchive "demo.ps1.zip" -StorageContext $storageContext -ConfigurationName "runScript" -Verbose

#Update the configuration of an Azure Virtual Machine
$demoVM | Update-AzureVM -Verbose

#check on status
Get-AzureVMDscExtensionStatus -VM $demovm -Verbose
```

## <a name="logging"></a>Zapnout protokolování ##

Protokoly jsou umístěná v:

C:\WindowsAzure\Logs\Plugins\Microsoft.PowerShell.DSC\[číslo verze]

## <a name="next-steps"></a>Další kroky ##

Další informace o prostředí PowerShell DSC, [Navštěvujte blog o Centru si přečtěte následující dokumentaci Powershellu](https://msdn.microsoft.com/powershell/dsc/overview). 

Prozkoumejte [Správce prostředků Azure šablony pro rozšíření DSC](virtual-machines-windows-extensions-dsc-template.md
). 

Najít další funkce můžete spravovat pomocí prostředí PowerShell DSC, [vyhledejte v galerii Powershellu](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) pro další DSC zdroje.

Podrobnosti o předávání citlivé parametry do konfigurace najdete v tématu [Spravovat pověření bezpečně s obslužné DSC rozšíření](virtual-machines-windows-extensions-dsc-credentials.md).
