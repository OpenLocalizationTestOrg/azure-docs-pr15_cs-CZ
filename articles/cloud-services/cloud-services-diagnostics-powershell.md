<properties
    pageTitle="Povolení Diagnostika v Azure Cloud Services pomocí prostředí PowerShell | Microsoft Azure"
    description="Zjistěte, jak povolit diagnostiky služby cloudu pomocí prostředí PowerShell"
    services="cloud-services"
    documentationCenter=".net"
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>


# <a name="enable-diagnostics-in-azure-cloud-services-using-powershell"></a>Povolení Diagnostika v Azure Cloud Services pomocí prostředí PowerShell

Můžete shromáždit data diagnostiky jako protokoly aplikace čítače atd do cloudové služby Azure diagnostiky s příponou. Tento článek popisuje, jak povolit koncovku Azure Diagnostika pro do cloudové služby pomocí Powershellu.  Zjistěte, [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md) pro požadavcích pro tento článek.

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>Povolení rozšíření Diagnostika v rámci nasazení do cloudové služby

Tento přístup dobré pro typ nepřetržitý integrace scénářů, kde můžete povoleno rozšíření diagnostiky jako součást nasazení cloudovou službu. Při vytváření nového nasazení cloudové služby povolíte koncovku diagnostiky předáním parametr *ExtensionConfiguration* rutině [AzureDeployment nový](https://msdn.microsoft.com/library/azure/mt589089.aspx) . *ExtensionConfiguration* parametr maticových konfigurace diagnostických nástrojů, které lze vytvářet pomocí rutinu [New-AzureServiceDiagnosticsExtensionConfig](https://msdn.microsoft.com/library/azure/mt589168.aspx) .

Následující příklad ukazuje, jak můžete povolit diagnostiky do cloudové služby WebRole a WorkerRole každý s konfigurace různých diagnostiky.

    $service_name = "MyService"
    $service_package = "CloudService.cspkg"
    $service_config = "ServiceConfiguration.Cloud.cscfg"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)

Pokud soubor konfigurace diagnostiky určuje prvek StorageAccount s název účtu úložiště, pak rutinu New-AzureServiceDiagnosticsExtensionConfig bude automaticky používat úložiště. Tento postup vyžaduje účet úložiště musí být v rámci stejného předplatného jako Cloudovou službu nasazení.

Z Azure SDK 2.6 dále publikovat konfigurační soubory s příponou generovaných MSBuild cílový výstup bude obsahovat název účtu úložiště podle diagnostiky konfigurační řetězec zadané v souboru konfigurace služby (.cscfg). Skript dole ukazuje, jak analyzovat konfigurační soubory s příponou z cílový výstup publikovat a konfigurace rozšíření diagnostických nástrojů pro každou roli při nasazení cloudovou službu.

    $service_name = "MyService"
    $service_package = "C:\build\output\CloudService.cspkg"
    $service_config = "C:\build\output\ServiceConfiguration.Cloud.cscfg"

    #Find the Extensions path based on service configuration file
    $extensionsSearchPath = Join-Path -Path (Split-Path -Parent $service_config) -ChildPath "Extensions"

    $diagnosticsExtensions = Get-ChildItem -Path $extensionsSearchPath -Filter "PaaSDiagnostics.*.PubConfig.xml"
    $diagnosticsConfigurations = @()
    foreach ($extPath in $diagnosticsExtensions)
    {
    #Find the RoleName based on file naming convention PaaSDiagnostics.<RoleName>.PubConfig.xml
    $roleName = ""
    $roles = $extPath -split ".",0,"simplematch"
    if ($roles -is [system.array] -and $roles.Length -gt 1)
        {
        $roleName = $roles[1]
        $x = 2
        while ($x -le $roles.Length)
            {
               if ($roles[$x] -ne "PubConfig")
                {
                    $roleName = $roleName + "." + $roles[$x]
                }
                else
                {
                    break
                }
                $x++
            }
        $fullExtPath = Join-Path -path $extensionsSearchPath -ChildPath $extPath
        $diagnosticsconfig = New-AzureServiceDiagnosticsExtensionConfig -Role $roleName -DiagnosticsConfigurationPath $fullExtPath
        $diagnosticsConfigurations += $diagnosticsconfig
        }
    }
    New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration $diagnosticsConfigurations

Visual Studio Online používá podobný přístup pro automatické deploymnts Cloudovým službám s příponou diagnostických nástrojů. Dokončení příkladu naleznete v tématu [Publikování AzureCloudDeployment.ps1](https://github.com/Microsoft/vso-agent-tasks/blob/master/Tasks/AzureCloudPowerShellDeployment/Publish-AzureCloudDeployment.ps1) .

Pokud žádná StorageAccount zadaná v konfiguraci diagnostických nástrojů, musíte v parametru StorageAccountName předat rutiny. Pokud zadaný parametr StorageAccountName potom rutinu bude vždy používat účet úložiště, který je určen v parametru a ne té, která je zadaný v souboru konfigurace diagnostiky.

Pokud účet úložiště Diagnostika v jiné předplatné z cloudové služby, musíte explicitně v parametrech StorageAccountName a StorageAccountKey předat rutiny. Při účet úložiště diagnostiky je ve stejném předplatném rutinu můžete automaticky dotazu a nastavit hodnoty klíče při aktivaci koncovku Diagnostika není potřeba parametr StorageAccountKey. Však-li účet úložiště Diagnostika v jiné předplatné, pak rutinu nebudete moct automaticky získat klávesu a budete muset explicitně zadat klíč prostřednictvím parametr StorageAccountKey.

    $webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
    $workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key


## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>Povolení rozšíření diagnostiky na existující cloudové služby

Povolení nebo aktualizovat konfigurace diagnostiky na do cloudové služby, na kterém běží už můžete použít rutinu [Set-AzureServiceDiagnosticsExtension](https://msdn.microsoft.com/library/azure/mt589140.aspx) .


    $service_name = "MyService"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    Set-AzureServiceDiagnosticsExtension -DiagnosticsConfiguration @($webrole_diagconfig,$workerrole_diagconfig) -ServiceName $service_name


## <a name="get-current-diagnostics-extension-configuration"></a>Získání aktuální konfigurace rozšíření diagnostiky
Získání aktuální konfigurace diagnostiky ke cloudové službě získáte pomocí rutiny [Get-AzureServiceDiagnosticsExtension](https://msdn.microsoft.com/library/azure/mt589204.aspx) .

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"

## <a name="remove-diagnostics-extension"></a>Odstranění diagnostických nástrojů rozšíření
Vypnout diagnostiku do cloudové služby můžete použít rutinu [AzureServiceDiagnosticsExtension odebrat](https://msdn.microsoft.com/library/azure/mt589183.aspx) .

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"

Pokud jste povolili koncovku diagnostických nástrojů *Sady AzureServiceDiagnosticsExtension* nebo *Nový AzureServiceDiagnosticsExtensionConfig* bez parametru *Role* můžete odebrat rozšíření pomocí *Odebrat AzureServiceDiagnosticsExtension* bez parametru *Role* . Pokud parametr *Role* byla použita při aktivaci rozšíření potom ho musí také při odebrání rozšíření.

Odebrání koncovku Diagnostika z každé jednotlivé role:

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"


## <a name="next-steps"></a>Další kroky

- Další informace o použití Azure diagnostiky a jinými postupy pro řešení potíží najdete v článku [Povolení Diagnostika v Azure cloudovými službami a virtuálních počítačích](cloud-services-dotnet-diagnostics.md).
- [Schéma konfigurace diagnostiky](https://msdn.microsoft.com/library/azure/dn782207.aspx) vysvětluje různé možnosti konfigurace xml pro diagnostiku rozšíření.
- Zjistěte, jak povolit koncovku diagnostických nástrojů pro virtuálních počítačích, najdete v tématu [Vytvoření Windows virtuální stroje s sledování a diagnostice pomocí šablony správce prostředků Azure](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)  
