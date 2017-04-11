<properties
    pageTitle="Online přes instalační program aplikace přehledy v Azure | Microsoft Azure"
    description="Automatizace konfigurace Azure diagnostiky kanálu interpretace aplikace."
    services="application-insights"
    documentationCenter=".net"
    authors="sbtron"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="11/17/2015"
    ms.author="awills"/>

# <a name="using-powershell-to-set-up-application-insights-for-an-azure-web-app"></a>Nastavení aplikace přehledy pro Azure webovou aplikaci pomocí prostředí PowerShell

[Microsoft Azure](https://azure.com) může být [nakonfigurované tak, aby odeslat Azure diagnostiky](app-insights-azure-diagnostics.md) interpretace [aplikace Visual Studio](app-insights-overview.md). Diagnostika týkající se Azure cloudovými službami a Azure VMs. Se doplní telemetrie, ze které budete odesílat v aplikaci pomocí SDK přehledy aplikace. V rámci automatizace proces vytváření nových zdrojů v Azure můžete nakonfigurovat diagnostiky pomocí Powershellu.

## <a name="azure-template"></a>Azure šablony

Pokud je webové aplikace Azure a vytvoříte zdrojů pomocí šablony správce prostředků Azure, můžete nakonfigurovat aplikace přehledy přidáním uzel zdroje:

    {
      resources: [
        /* Create Application Insights resource */
        {
          "apiVersion": "2015-05-01",
          "type": "microsoft.insights/components",
          "name": "nameOfAIAppResource",
          "location": "centralus",
          "kind": "web",
          "properties": { "ApplicationId": "nameOfAIAppResource" },
          "dependsOn": [
            "[concat('Microsoft.Web/sites/', myWebAppName)]"
          ]
        }
       ]
     } 

* `nameOfAIAppResource`– Název zdroje přehledy aplikace
* `myWebAppName`– id web appu


## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>Povolení rozšíření Diagnostika v rámci nasazení do cloudové služby

`New-AzureDeployment` Rutina má parametr `ExtensionConfiguration`, která trvá maticových Diagnostika konfigurace. Tyto dá vytvořit `New-AzureServiceDiagnosticsExtensionConfig` rutiny. Příklad:

```ps

    $service_package = "CloudService.cspkg"
    $service_config = "ServiceConfiguration.Cloud.cscfg"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $primary_storagekey = (Get-AzureStorageKey `
     -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext `
       -StorageAccountName $diagnostics_storagename `
       -StorageAccountKey $primary_storagekey

    $webrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WebRole" -Storage_context $storageContext `
      -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WorkerRole" `
      -StorageContext $storage_context `
      -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    New-AzureDeployment `
      -ServiceName $service_name `
      -Slot Production `
      -Package $service_package `
      -Configuration $service_config `
      -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)

``` 

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>Povolení rozšíření diagnostiky na existující cloudové služby

Stávající služba, pomocí `Set-AzureServiceDiagnosticsExtension`.

```ps
 
    $service_name = "MyService"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"
    $primary_storagekey = (Get-AzureStorageKey `
         -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext `
        -StorageAccountName $diagnostics_storagename `
        -StorageAccountKey $primary_storagekey

    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $webrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WebRole" 
    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $workerrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WorkerRole"
```

## <a name="get-current-diagnostics-extension-configuration"></a>Získání aktuální konfigurace rozšíření diagnostiky

```ps

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```


## <a name="remove-diagnostics-extension"></a>Odstranění diagnostických nástrojů rozšíření

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

Pokud jste povolili koncovku diagnostiky některým `Set-AzureServiceDiagnosticsExtension` nebo `New-AzureServiceDiagnosticsExtensionConfig` bez parametru Role můžete odebrat pomocí rozšíření `Remove-AzureServiceDiagnosticsExtension` bez parametru Role. Pokud parametr Role byla použita při aktivaci rozšíření potom ho musí také při odebrání rozšíření.

Odebrání koncovku Diagnostika z každé jednotlivé role:

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```


## <a name="see-also"></a>Viz taky

* [Sledování Azure Cloud Services aplikace s přehledy aplikace](app-insights-cloudservices.md)
* [Odeslání Azure diagnostiky interpretace aplikace](app-insights-azure-diagnostics.md)
* [Automatizace Konfigurace upozornění](app-insights-powershell-alerts.md)

