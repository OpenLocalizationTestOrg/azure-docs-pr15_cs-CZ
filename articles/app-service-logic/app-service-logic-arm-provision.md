<properties 
    pageTitle="Vytvoření aplikace logiky pomocí Správce prostředků Azure šablon v aplikaci služby Azure | Microsoft Azure" 
    description="Použití šablony správce prostředků Azure k nasazení aplikace prázdné logiky pro definování pracovních postupů." 
    services="logic-apps" 
    documentationCenter="" 
    authors="MSFTMan" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/25/2016" 
    ms.author="deonhe"/>

# <a name="create-a-logic-app-using-a-template"></a>Vytvoření aplikace logiky použít šablonu?

Použití Správce prostředků Azure šablony k vytvoření aplikace pro použití logických operátorů prázdné něhož chcete definovat pracovní postupy. Můžete definovat prostředků, které jsou nasazeném a jak definovat parametry určených při spuštění nasazení. Pomocí této šablony vlastního nasazení nebo přizpůsobit tak, aby vyhovovala vašim požadavkům.

Podrobné informace o vlastnostech aplikace použití logických operátorů najdete v článku [Použití logických operátorů rozhraní API správy pracovního postupu aplikace](https://msdn.microsoft.com/library/azure/mt643788.aspx). 

Příklady samotnou definici najdete v článku [definice Autor logiky aplikace](app-service-logic-author-definitions.md). 

Další informace o vytváření šablon najdete v tématu [Vytváření šablony Azure správce prostředků](../resource-group-authoring-templates.md).

Dokončení šablony najdete v článku [aplikace logiky šablony](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json).

## <a name="what-you-will-deploy"></a>Co budete nasazovat

Pomocí této šablony nasazení aplikace od použití logických operátorů.

Automatické spuštění nasazení, vyberte toto tlačítko:  

[![Nasazení Azure](media/app-service-logic-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)

## <a name="parameters"></a>Parametry

[AZURE.INCLUDE [app-service-logic-deploy-parameters](../../includes/app-service-logic-deploy-parameters.md)]

### <a name="testuri"></a>testUri

     "testUri": {
        "type": "string",
        "defaultValue": "http://azure.microsoft.com/en-us/status/feed/"
      }
    
## <a name="resources-to-deploy"></a>Materiály pro nasazení

### <a name="logic-app"></a>Použití logických operátorů aplikace

Vytvoří aplikaci logiky.

Šablony používá hodnoty parametru pro použití logických operátorů název aplikace. Nastaví umístění aplikace logiky do stejného umístění jako skupina zdroje. 

Tato konkrétní definice spustí jednou za hodinu a pomocí příkazu ping místo určené parametrem **testUri** . 

    {
      "type": "Microsoft.Logic/workflows",
      "apiVersion": "2016-06-01",
      "name": "[parameters('logicAppName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "displayName": "LogicApp"
      },
      "properties": {
        "definition": {
          "$schema": "http://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "testURI": {
              "type": "string",
              "defaultValue": "[parameters('testUri')]"
            }
          },
          "triggers": {
            "recurrence": {
              "type": "recurrence",
              "recurrence": {
                "frequency": "Hour",
                "interval": 1
              }
            }
          },
          "actions": {
            "http": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('testUri')"
              },
              "runAfter": {}
            }
          },
          "outputs": {}
        },
        "parameters": {}
      }
    }


## <a name="commands-to-run-deployment"></a>Příkazy pro spuštění nasazení

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>Prostředí PowerShell

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure rozhraní příkazového řádku

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -g ExampleDeployGroup


 
