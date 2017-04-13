<properties
    pageTitle="Vytvoření služby Bus obor názvů s fronty pomocí šablony pro správce prostředků Azure | Microsoft Azure"
    description="Vytváření názvů Bus služby a fronty pomocí šablony správce prostředků Azure"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="10/14/2016"
    ms.author="sethm;shvija"/>

# <a name="create-a-service-bus-namespace-and-a-queue-using-an-azure-resource-manager-template"></a>Vytváření názvů Bus služby a fronty pomocí šablony pro správce prostředků Azure

Tento článek ukazuje, jak používat šablonu správce prostředků Azure, která vytvoří obor Bus služby a fronty. Naučíte se, jak definovat prostředků, které používají a jak definovat parametry, které jsou uvedené provedení nasazení. Pomocí této šablony vlastního nasazení nebo přizpůsobit tak, aby vyhovovala vašim požadavkům.

Další informace o vytváření šablon najdete v článku [vytváření správce prostředků Azure šablony][].

Dokončení šablonu vyberete najdete v článku [služby Bus názvů fronty šablony][] na GitHub.

>[AZURE.NOTE] Následující správce prostředků Azure šablony jsou k dispozici ke stažení a nasazení.
>
>-    [Vytvoření obor služby Bus s fronty a ověření pravidla](service-bus-resource-manager-namespace-auth-rule.md)
>-    [Vytvoření služby Bus obor názvů s téma a předplatného](service-bus-resource-manager-namespace-topic.md)
>-    [Vytvoření obor Bus služby](service-bus-resource-manager-namespace.md)
>-    [Vytvoření události rozbočovače názvů ke skupině centrální událostí a příjemce](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)
>
>Nejnovější šablony, navštěvujte blog do Galerie [Šablon Azure rychlý úvod][] a vyhledejte "Služby Bus."

## <a name="what-will-you-deploy"></a>Co se nasadit?

Pomocí této šablony můžete nasadit služby Bus obor názvů s fronty.

[Služba Bus fronty](service-bus-queues-topics-subscriptions.md#queues) nabízejí první v první mimo FIFO doručení zprávy pro jednoho nebo více konkurenční spotřebitele.

Automatické spuštění nasazení, klikněte na toto tlačítko:

[![Nasazení Azure](./media/service-bus-resource-manager-namespace-queue/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-queue%2Fazuredeploy.json)

## <a name="parameters"></a>Parametry

Pomocí Správce prostředků Azure definovat parametry pro hodnoty, které chcete zadat při nasazení šablony. Šablona obsahuje oddílu s názvem `Parameters` , která obsahuje všechny hodnoty parametrů. Je třeba definovat parametr hodnoty, které se liší podle projektu, který nasazujete nebo podle prostředí, které nasazujete. Parametry pro hodnoty, které bude vždy, zůstane stejná není definovat. Každá hodnota parametru slouží v šabloně k definování zdroje, které používají.

Šablona definuje následujících parametrů.

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName

Název oboru Bus služby chcete vytvořit.

```
"serviceBusNamespaceName": {
"type": "string",
"metadata": { 
    "description": "Name of the Service Bus namespace" 
    }
}
```

### <a name="servicebusqueuename"></a>serviceBusQueueName

Název fronty vytvořené v oboru Bus služby.

```
"serviceBusQueueName": {
"type": "string"
}
```

### <a name="servicebusapiversion"></a>serviceBusApiVersion

Rozhraní API služeb Bus verze šablony.

```
"serviceBusApiVersion": {
"type": "string"
}
```

## <a name="resources-to-deploy"></a>Materiály pro nasazení

Vytvoří služby Bus obor názvů typu **zprávy**s fronty.

```
"resources ": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
            "tier": "Standard"
        },
        "resources": [{
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusQueueName')]",
            "type": "Queues",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusQueueName')]",
            }
        }]
    }]
```

## <a name="commands-to-run-deployment"></a>Příkazy pro spuštění nasazení

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>Prostředí PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-queue/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure rozhraní příkazového řádku

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-queue/azuredeploy.json>
```

## <a name="next-steps"></a>Další kroky

Teď, když jste vytvořili a nasazené zdrojů pomocí Správce prostředků Azure, přečtěte si, jak spravovat tyto materiály pomocí těchto článků:

- [Správa služby Bus pomocí prostředí PowerShell](service-bus-powershell-how-to-provision.md)
- [Přidávání a používání zdrojů Bus služby v Průzkumníkovi Bus služby](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)


  [Vytváření šablon správce prostředků Azure]: ../resource-group-authoring-templates.md
  [Služba Bus názvů fronty šablony]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/
  [Rychlý úvod Azure šablony]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Learn more about Service Bus queues]: service-bus-queues-topics-subscriptions.md
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
