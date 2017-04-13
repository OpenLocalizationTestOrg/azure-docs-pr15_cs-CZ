<properties
   pageTitle="Shromáždění protokolů pomocí diagnostických nástrojů Azure | Microsoft Azure"
   description="Tento článek popisuje, jak nastavit Azure diagnostiky shromáždit protokoly ze struktury služby clusteru spuštěné v Azure."
   services="service-fabric"
   documentationCenter=".net"
   authors="ms-toddabel"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/28/2016"
   ms.author="toddabel"/>


# <a name="collect-logs-by-using-azure-diagnostics"></a>Shromáždění protokolů pomocí diagnostických nástrojů Azure

> [AZURE.SELECTOR]
- [Windows](service-fabric-diagnostics-how-to-setup-wad.md)
- [Linux](service-fabric-diagnostics-how-to-setup-lad.md)

Pokud používáte struktury služby Azure clusteru, je vhodné shromáždit protokoly ze všech uzlů v centrálním umístění. S protokoly v centrálním umístění vám pomůže analyzovat a řešení problémů v svůj cluster nebo problémy v aplikacích a službách spuštěné v tomto obrázku.

Jedním ze způsobů nahrávat a shromáždění protokolů, je použít rozšíření Azure diagnostických nástrojů, které odešlou protokoly úložišti Azure. Protokoly popsané není to přímo v úložišti. Ale je možné použít externí proces události z úložiště a, že je umístíte v produktu, například [Protokolu analýzy](../log-analytics/log-analytics-service-fabric.md), [Pružná vyhledávání](service-fabric-diagnostic-how-to-use-elasticsearch.md)nebo jiné řešení analýza protokolu.

## <a name="prerequisites"></a>Zjistit předpoklady pro
Mezi ně použijete k provedení některých operace v tomto dokumentu:

* [Azure diagnostiky](../cloud-services/cloud-services-dotnet-diagnostics.md) (související ke Cloudovým službám Azure, ale mám užitečných informací a příklady)
* [Azure správce prostředků](../azure-resource-manager/resource-group-overview.md)
* [Azure Powershellu](../powershell-install-configure.md)
* [Azure správce klienta](https://github.com/projectkudu/ARMClient)
* [Azure správce prostředků šablony](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)


## <a name="log-sources-that-you-might-want-to-collect"></a>Protokol zdroje, které můžete chtít shromáždit
- **Služba struktury protokolů**: vyzařováno platformu na standardní události trasování pro Windows (trasování událostí pro Windows) a ZDROJ_UDÁLOSTI kanály. Protokoly může být jeden z několika typů:
  - Funkční události: protokoly pro operace, které provádí platformu struktury služby. Jako příklad lze uvést vytváření aplikací a služeb, změny stavu uzel a informace o upgradu.
  - [Spolehlivé účastníky programování událostí modelu](service-fabric-reliable-actors-diagnostics.md)
  - [Spolehlivé služby programování událostí modelu](service-fabric-reliable-services-diagnostics.md)
- **Události aplikace**: události vyzařováno kód vaše služby a napsána s použitím pomocníka třídy ZDROJ_UDÁLOSTI podle šablony aplikace Visual Studio. Další informace o tom, jak uložit protokoly z aplikace najdete v tématu [Monitor a Diagnostika služby v nastavení vývoj místního počítače](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).


## <a name="deploy-the-diagnostics-extension"></a>Nasazení rozšíření diagnostických nástrojů
Cílem prvního kroku v shromáždění protokolů je pro nasazení rozšíření diagnostiky jednotlivých hodnot VMs clusteru struktury služby. Rozšíření diagnostiky shromažďují protokoly na každý OM a odešle úložiště účtu, který určíte. Postup trochu závisí na tom, jestli používáte Azure portál nebo správce prostředků Azure. Postup také lišit v závislosti na tom, jestli nasazení je součástí vytváření clusteru nebo je určený pro obrázku, které už existuje. Podívejme se na tyto kroky pro každý scénář.

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-through-the-portal"></a>Nasazení rozšíření diagnostiky jako součást vytváření clusteru prostřednictvím portálu
Do kterých se nasadí koncovku diagnostiky VMs clusteru jako součást vytváření clusteru, použijte panel nastavení diagnostických nástrojů vidět na následujícím obrázku. Povolení spolehlivé objekty actor nebo spolehlivé služby kolekce události, ověřte, zda diagnostiky **na** (výchozí nastavení). Po vytvoření clusteru nelze změnit nastavení tak, že na portálu.

![Azure nastavení diagnostických nástrojů v portálu pro vytvoření obrázku](./media/service-fabric-diagnostics-how-to-setup-wad/portal-cluster-creation-diagnostics-setting.png)

Podpora *vyžaduje* Azure podpory týmu protokoly vyřešit požadavky na podporu, které jste vytvořili. Tyto protokoly se odebírají v reálném čase a jsou uložené v jednom z úložiště účtů vytvořených ve skupině prostředků. Nastavení diagnostických nástrojů konfigurace událostí, které úrovni aplikace. Tyto události zahrnují [Spolehlivé účastníky](service-fabric-reliable-actors-diagnostics.md) , události [Spolehlivé služeb](service-fabric-reliable-services-diagnostics.md) a některé úrovni systému služby struktury událostech uložené v úložišti Azure.

Produkty [Pružná hledání](service-fabric-diagnostic-how-to-use-elasticsearch.md) ATP vlastního obrázku můžete získat události z účtu úložiště. Současné době nejde žádným způsobem nějak filtrovat nebo upravovat události, které jsou odeslány do tabulky. Pokud nemáte zavést proces, jak odebrat události z tabulky, zůstanou v tabulce naroste.

Když vytváříte clusteru pomocí portálu, doporučujeme stáhnout šablonu *před kliknutím na * *OK*** vytvoření clusteru. Informace najdete v příručce [Nastavení služby struktury obrázku s použitím správce prostředků Azure šablony](service-fabric-cluster-creation-via-arm.md). Šablonu, kterou chcete změnit později, budete potřebovat, protože nemůžete udělat pár změn pomocí portálu.

Pomocí následujících kroků můžete exportovat šablony z portálu. Tyto šablony však může být obtížné použít, protože mohou mít prázdných hodnot, které nebyly nalezeny požadované informace.

1. Otevřete skupina zdroje.
2. Vyberte **Nastavení** pro zobrazení panelu nastavení.
3. Vyberte **nasazení** pro zobrazení panelu Historie nasazení.
4. Vyberte nasazení a zobrazte podrobnosti nasazení.
5. Vyberte **Export šablony** pro zobrazení panelu šablony.
6. Zaškrtněte políčko **Uložit soubor** k exportu souboru ZIP obsahujícího šablony, parametry a prostředí PowerShell soubory.

Po exportu soubory, které potřebujete udělat změny. Upravte soubor parameters.json a odeberte prvek **adminPassword** . Výzva k zadání hesla dojde k při spuštění skript pro nasazení. Pokud používáte skript pro nasazení, bude pravděpodobně nutné opravit hodnoty null parametrů.

Použití staženou šablonu aktualizovat konfigurace:

1. Extrahování obsahu do složky na místním počítači.
2. Změňte obsah tak, aby odrážely nové konfigurace.
3. Spusťte prostředí PowerShell a přejděte do složky, které jste extrahovali obsah.
4. Spusťte **deploy.ps1** a vyplňte ID předplatného, na název skupiny zdrojů (použijte stejný název aktualizace konfigurace) a nasazení jedinečný název.


### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-by-using-azure-resource-manager"></a>Nasazení rozšíření diagnostiky jako součást vytváření clusteru pomocí Správce prostředků Azure
Vytvoření clusteru pomocí Správce prostředků, potřebujete přidat konfigurace diagnostiky JSON k šabloně správce prostředků úplné clusteru před vytvořením clusteru. Ukázková šablona správce prostředků clusteru pět OM poskytneme konfigurace diagnostiky přidané v rámci naší vzorků šablony správce prostředků. Můžete ho vidíte na tomto místě v galerii Azure ukázky: [clusteru pět uzel s ukázkovými správce prostředků diagnostiky šablony](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype-wad).

Pokud chcete zobrazit nastavení diagnostických nástrojů v šabloně správce prostředků, otevřete soubor azuredeploy.json a vyhledejte **IaaSDiagnostics**. Clusteru pomocí této šablony vytvoříte klikněte na tlačítko **instalovat na Azure** jsou dostupné na předchozí odkaz.

Můžete taky můžete stáhnout ukázkový správce prostředků, proveďte změny a clusteru pomocí změněné šablony můžete vytvořit pomocí `New-AzureRmResourceGroupDeployment` příkazu v okně Azure Powershellu. Následující kód parametrů, které předáváte v na příkaz zobrazit. Podrobné informace o tom, jak nasazení skupiny zdrojů pomocí prostředí PowerShell najdete v článku [nasazení skupina zdroje šablonou správce prostředků Azure](../resource-group-template-deploy.md).

```powershell

New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -Name $deploymentName -TemplateFile $pathToARMConfigJsonFile -TemplateParameterFile $pathToParameterFile –Verbose
```

### <a name="deploy-the-diagnostics-extension-to-an-existing-cluster"></a>Nasazení rozšíření diagnostiky do existujícího clusteru
Pokud máte existující clusteru, který nemá diagnostiky používaný nebo pokud chcete upravit stávající konfigurace, můžete přidat nebo aktualizovat. Úprava šablony správce prostředků, který slouží k vytvoření existující clusteru nebo stažení šablony z portálu jak bylo popsáno dříve. Upravte soubor template.json provedením úkoly podle těchto pokynů.

Přidejte nový zdroj úložiště k šabloně do oddílu zdroje.

```json
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[parameters('applicationDiagnosticsStorageAccountName')]",
  "location": "[parameters('computeLocation')]",
  "properties": {
    "accountType": "[parameters('applicationDiagnosticsStorageAccountType')]"
  },
  "tags": {
    "resourceType": "Service Fabric",
    "clusterName": "[parameters('clusterName')]"
  }
},
```

 Dále přidejte do oddílu parametry hned za definici účtu úložiště mezi `supportLogStorageAccountName` a `vmNodeType0Name`. Nahrazení zástupného textu, *sem zadejte název účtu úložiště* název účtu úložiště.

```json
    "applicationDiagnosticsStorageAccountType": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "Replication option for the application diagnostics storage account"
      }
    },
    "applicationDiagnosticsStorageAccountName": {
      "type": "string",
      "defaultValue": "storage account name goes here",
      "metadata": {
        "description": "Name for the storage account that contains application diagnostics data from the cluster"
      }
    },
```
Aktualizujte `VirtualMachineProfile` template.json souboru můžete přidat následující kód v rámci určené rozšíření. Ujistěte se, že na začátku nebo na konec, podle toho, kde je vložen přidat čárku.

```json
{
    "name": "[concat(parameters('vmNodeType0Name'),'_Microsoft.Insights.VMDiagnosticsSettings')]",
    "properties": {
        "type": "IaaSDiagnostics",
        "autoUpgradeMinorVersion": true,
        "protectedSettings": {
        "storageAccountName": "[parameters('applicationDiagnosticsStorageAccountName')]",
        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName')),'2015-05-01-preview').key1]",
        "storageAccountEndPoint": "https://core.windows.net/"
        },
        "publisher": "Microsoft.Azure.Diagnostics",
        "settings": {
        "WadCfg": {
            "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": "50000",
            "EtwProviders": {
                "EtwEventSourceProviderConfiguration": [
                {
                    "provider": "Microsoft-ServiceFabric-Actors",
                    "scheduledTransferKeywordFilter": "1",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableActorEventTable"
                    }
                },
                {
                    "provider": "Microsoft-ServiceFabric-Services",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableServiceEventTable"
                    }
                }
                ],
                "EtwManifestProviderConfiguration": [
                {
                    "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                    "scheduledTransferLogLevelFilter": "Information",
                    "scheduledTransferKeywordFilter": "4611686018427387904",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricSystemEventTable"
                    }
                }
                ]
            }
            }
        },
        "StorageAccount": "[parameters('applicationDiagnosticsStorageAccountName')]"
        },
        "typeHandlerVersion": "1.5"
    }
}
```

Po úpravě souboru template.json jak je popsáno, publikujte šablony správce prostředků. Pokud šablona exportován, spuštění souboru deploy.ps1 znovu publikuje uzamkl šabloně. Po nasazení, zajistěte, aby byl **ProvisioningState** **proběhla úspěšně**.


## <a name="update-diagnostics-to-collect-and-upload-logs-from-new-eventsource-channels"></a>Aktualizace Diagnostika a shromáždit protokoly z nového ZDROJ_UDÁLOSTI kanálů
Aktualizovat diagnostiky shromáždit protokoly od nové ZDROJ_UDÁLOSTI kanály, které představují novou aplikaci, která se chystáte nasazení, proveďte stejným způsobem jako v [předchozí části](#deploywadarm) pro nastavení diagnostických nástrojů pro existující obrázku.

Aktualizovat `EtwEventSourceProviderConfiguration` oddíl v souboru template.json k přidání položek pro nové kanály ZDROJ_UDÁLOSTI před použitím konfigurace aktualizace pomocí `New-AzureRmResourceGroupDeployment` PowerShell command. Zdroj události je definována jako část kódu v souboru generované Visual Studio ServiceEventSource.cs.

Pokud zdroj události s názvem Moje ZDROJ_UDÁLOSTI, například přidáte následující kód umístit události z osobního ZDROJ_UDÁLOSTI do v tabulce s názvem MyDestinationTableName.

```json
        {
            "provider": "My-Eventsource",
            "scheduledTransferPeriod": "PT5M",
            "DefaultEvents": {
            "eventDestination": "MyDestinationTableName"
            }
        }
```

Získat informace o výkonnosti nebo protokoly událostí, upravte pomocí příklady uvedené v tématu [Vytvoření Windows virtuálního počítače s sledování a diagnostice pomocí šablony pro správce prostředků Azure](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)správce prostředků šablonu. Pak publikujte šablony správce prostředků.

## <a name="next-steps"></a>Další kroky
Podrobnější k jakým událostem by měla hledáte při řešení problémů s, najdete v tématu diagnostických událostí vyzařovaného [Spolehlivé účastníky](service-fabric-reliable-actors-diagnostics.md) a [Spolehlivé služeb](service-fabric-reliable-services-diagnostics.md).


## <a name="related-articles"></a>Související články
* [Zjistěte, jak získat informace o výkonnosti nebo protokoly pomocí diagnostických nástrojů rozšíření](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)
* [Řešení služby struktury v protokolu analýzy](../log-analytics/log-analytics-service-fabric.md)
