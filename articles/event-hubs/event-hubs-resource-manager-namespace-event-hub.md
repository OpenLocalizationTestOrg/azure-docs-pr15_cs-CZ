<properties
    pageTitle="Vytvoření události rozbočovače názvů s centrální událostí a spotř skupiny pomocí šablony pro správce prostředků Azure | Microsoft Azure"
    description="Vytvoření události rozbočovače názvů centrální událostí a spotř skupinu pomocí Správce prostředků Azure šablony a"
    services="event-hubs"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="08/31/2016"
    ms.author="sethm;shvija"/>

# <a name="create-an-event-hubs-namespace-with-event-hub-and-consumer-group-using-an-azure-resource-manager-template"></a>Vytvoření události rozbočovače názvů s centrální událostí a spotř skupiny pomocí šablony pro správce prostředků Azure

Tento článek ukazuje, jak použít šablonu správce prostředků Azure, která vytvoří událost rozbočovače názvů s rozbočovači událostí a skupinu příjemce. Naučíte se, jak definovat prostředků, které používají a jak definovat parametry, které jsou uvedené provedení nasazení. Můžete pomocí této šablony vlastního nasazení nebo přizpůsobit tak, aby vyhovovala vašim požadavkům

Další informace o vytváření šablon najdete v článku [vytváření správce prostředků Azure šablony][].

Dokončení šablony najdete v [Centrální událostí a šablona skupiny spotř][] na GitHub.

>[AZURE.NOTE]
>Nejnovější šablony, navštěvujte blog do Galerie [Šablon Azure rychlý úvod][] a vyhledejte rozbočovače události.

## <a name="what-will-you-deploy"></a>Co se nasadit?

Pomocí této šablony nasadíte události rozbočovače obor názvů s rozbočovači událostí a skupinu příjemce.

[Událost rozbočovače](../event-hubs/event-hubs-what-is-event-hubs.md) je události služba slouží k poskytování událostí a telemetrie průniku Azure ve velkém měřítku obrovské, zhoršeným latence a vysoký spolehlivost pro zpracování.

Automatické spuštění nasazení, klikněte na toto tlačítko:

[![Nasazení Azure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

## <a name="parameters"></a>Parametry

Pomocí Správce prostředků Azure definovat parametry pro hodnoty, které chcete zadat nasazené na šablonu. Šablona obsahuje oddílu s názvem `Parameters` , která obsahuje všechny hodnoty parametrů. Je třeba definovat parametr hodnoty, které se liší podle projektu, který nasazujete nebo podle prostředí, které nasazujete. Parametry pro hodnoty, které bude vždy, zůstane stejná není definovat. Každá hodnota parametru slouží v šabloně k definování zdroje, které používají.

Šablona definuje následujících parametrů.

### <a name="eventhubnamespacename"></a>eventHubNamespaceName

Název oboru rozbočovače události chcete vytvořit.

```
"eventHubNamespaceName": {
"type": "string"
}
```

### <a name="eventhubname"></a>eventHubName

Název události centrální vytvořené v případě rozbočovače názvů.

```
"eventHubName": {
"type": "string"
}
```

### <a name="eventhubconsumergroupname"></a>eventHubConsumerGroupName

Název skupiny spotř vytvořené pro centrální události.

```
"eventHubConsumerGroupName": {
"type": "string"
}
```

### <a name="apiversion"></a>apiVersion

Rozhraní API verze šablony.

```
"apiVersion": {
"type": "string"
}
```

## <a name="resources-to-deploy"></a>Materiály pro nasazení

Vytvoří obor názvů typu **EventHubs**s rozbočovači událostí a skupinu příjemce.

```
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('namespaceName')]",
         "type":"Microsoft.EventHub/Namespaces",
         "location":"[variables('location')]",
         "sku":{  
            "name":"Standard",
            "tier":"Standard"
         },
         "resources":[  
            {  
               "apiVersion":"[variables('ehVersion')]",
               "name":"[parameters('eventHubName')]",
               "type":"EventHubs",
               "dependsOn":[  
                  "[concat('Microsoft.EventHub/namespaces/', parameters('namespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]"
               },
               "resources":[  
                  {  
                     "apiVersion":"[variables('ehVersion')]",
                     "name":"[parameters('consumerGroupName')]",
                     "type":"ConsumerGroups",
                     "dependsOn":[  
                        "[parameters('eventHubName')]"
                     ],
                     "properties":{  

                     }
                  }
               ]
            }
         ]
      }
   ],
```

## <a name="commands-to-run-deployment"></a>Příkazy pro spuštění nasazení

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>Prostředí PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json
```

## <a name="azure-cli"></a>Azure rozhraní příkazového řádku

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json][]
```

[Vytváření šablon správce prostředků Azure]: ../resource-group-authoring-templates.md
[Rychlý úvod Azure šablony]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Šablona události centrální a spotř skupiny]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/
