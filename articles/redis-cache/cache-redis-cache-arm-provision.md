<properties 
    pageTitle="Zřízení Redis mezipaměti | Microsoft Azure" 
    description="Pomocí Správce prostředků Azure šablony pro nasazení mezipaměti Redis Azure." 
    services="app-service" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="web" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/27/2016" 
    ms.author="sdanie"/>

# <a name="create-a-redis-cache-using-a-template"></a>Vytvořit Redis mezipaměť použít šablonu?

V tomto tématu se naučíte, jak vytvořit šablonu správce prostředků Azure, která nasadí mezipaměti Redis Azure. Mezipaměť lze s existujícím účtem úložiště zachovat diagnostiky data. Můžete taky informace o definování prostředků, které používají a jak definovat parametry, které jsou uvedené provedení nasazení. Pomocí této šablony vlastního nasazení nebo přizpůsobit tak, aby vyhovovala vašim požadavkům.

V současné době diagnostiky nastavení jsou sdílené pro všechny mezipaměti ve stejné oblasti pro předplatné. Aktualizace jeden mezipaměti v oblasti ovlivní všechny mezipaměti v oblasti.

Další informace o vytváření šablon najdete v tématu [Vytváření šablony Azure správce prostředků](../resource-group-authoring-templates.md).

Dokončení šablonu vyberete najdete v článku [Redis mezipaměť šablony](https://github.com/Azure/azure-quickstart-templates/blob/master/101-redis-cache/azuredeploy.json).

>[AZURE.NOTE] Jsou dostupné šablony správce prostředků pro nové [osy Premium](cache-premium-tier-intro.md) . 
>
>-    [Vytvořit mezipaměť Redis Premium s clusterů](https://azure.microsoft.com/documentation/templates/201-redis-premium-cluster-diagnostics/)
>-    [Vytvoření Premium Redis mezipaměti s trvání dat](https://azure.microsoft.com/documentation/templates/201-redis-premium-persistence/)
>-    [Vytvoření Premium Redis mezipaměti s VNet a volitelné clusterů](https://azure.microsoft.com/documentation/templates/201-redis-premium-vnet-cluster-diagnostics/)
>
>Vyhledat nejnovější šablon najdete v článku [Azure rychlý úvod šablony](https://azure.microsoft.com/documentation/templates/) a vyhledejte `Redis Cache`.

## <a name="what-you-will-deploy"></a>Co budete nasazovat

V této šabloně budete nasazovat Azure Redis mezipaměti, která používá existujícího účtu úložiště pro diagnostiku data.

Automatické spuštění nasazení, klikněte na toto tlačítko:

[![Nasazení Azure](./media/cache-redis-cache-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-redis-cache%2Fazuredeploy.json)

## <a name="parameters"></a>Parametry

Pomocí Správce prostředků Azure definovat parametry pro hodnoty, které chcete zadat při nasazení šablony. Šablona obsahuje oddílu s názvem parametrů obsahující všechny hodnoty parametrů.
Je třeba definovat parametr hodnoty, které se liší podle projektu, který nasazujete nebo podle prostředí, které nasazujete. Není definovat parametry pro hodnoty, které vždy zůstávají stejné. Každá hodnota parametru slouží v šabloně k definování zdroje, které používají. 


[AZURE.INCLUDE [app-service-web-deploy-redis-parameters](../../includes/cache-deploy-parameters.md)]

### <a name="rediscachelocation"></a>redisCacheLocation

Umístění Redis mezipaměti. Dosažení nejlepších výsledků dosáhnete na stejném místě aplikace pro použití se mezipaměti.

    "redisCacheLocation": {
      "type": "string"
    }

### <a name="existingdiagnosticsstorageaccountname"></a>existingDiagnosticsStorageAccountName

Název existujícího účtu úložiště pro účely diagnostiky. 

    "existingDiagnosticsStorageAccountName": {
      "type": "string"
    }

### <a name="enablenonsslport"></a>enableNonSslPort

Logická hodnota, která označuje, zda chcete povolit přístup prostřednictvím portů není SSL.

    "enableNonSslPort": {
      "type": "bool"
    }

### <a name="diagnosticsstatus"></a>diagnosticsStatus

Hodnota, která označuje, zda je povolen diagnostických nástrojů. Použití zapnuto nebo vypnuto.

    "diagnosticsStatus": {
      "type": "string",
      "defaultValue": "ON",
      "allowedValues": [
            "ON",
            "OFF"
        ]
    }
    
## <a name="resources-to-deploy"></a>Materiály pro nasazení

### <a name="redis-cache"></a>Redis mezipaměti

Vytvoří Azure Redis mezipaměti.

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('redisCacheName')]",
      "type": "Microsoft.Cache/Redis",
      "location": "[parameters('redisCacheLocation')]",
      "properties": {
        "enableNonSslPort": "[parameters('enableNonSslPort')]",
        "sku": {
          "capacity": "[parameters('redisCacheCapacity')]",
          "family": "[parameters('redisCacheFamily')]",
          "name": "[parameters('redisCacheSKU')]"
        }
      },
      "resources": [
        {
          "apiVersion": "2015-07-01",
          "type": "Microsoft.Cache/redis/providers/diagnosticsettings",
          "name": "[concat(parameters('redisCacheName'), '/Microsoft.Insights/service')]",
          "location": "[parameters('redisCacheLocation')]",
          "dependsOn": [
            "[concat('Microsoft.Cache/Redis/', parameters('redisCacheName'))]"
          ],
          "properties": {
            "status": "[parameters('diagnosticsStatus')]",
            "storageAccountName": "[parameters('existingDiagnosticsStorageAccountName')]"
          }
        }
      ]
    }



## <a name="commands-to-run-deployment"></a>Příkazy pro spuštění nasazení

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)] 

### <a name="powershell"></a>Prostředí PowerShell

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup -redisCacheName ExampleCache -redisCacheLocation "West US"

### <a name="azure-cli"></a>Azure rozhraní příkazového řádku

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -g ExampleDeployGroup


