<properties
    pageTitle="Vytvoření obor služby Bus s téma a předplatného pomocí šablony pro správce prostředků Azure | Microsoft Azure"
    description="Vytvoření obor služby Bus s téma a předplatného pomocí šablony správce prostředků Azure"
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

# <a name="create-a-service-bus-namespace-with-topic-and-subscription-using-an-azure-resource-manager-template"></a>Vytvoření obor služby Bus s téma a předplatného pomocí šablony pro správce prostředků Azure

Tento článek ukazuje, jak používat šablonu správce prostředků Azure, která vytvoří obor služby Bus s téma a předplatné. Naučíte se, jak definovat prostředků, které používají a jak definovat parametry, které jsou uvedené provedení nasazení. Můžete pomocí této šablony vlastního nasazení nebo přizpůsobit tak, aby vyhovovala vašim požadavkům

Další informace o vytváření šablon najdete v článku [vytváření správce prostředků Azure šablony][].

Dokončení šablonu vyberete najdete v článku šabloně [služby Bus obor názvů s téma a předplatné][] .

>[AZURE.NOTE] Následující správce prostředků Azure šablony jsou k dispozici ke stažení a nasazení.
>
>-    [Vytvoření obor služby Bus s fronty a ověření pravidla](service-bus-resource-manager-namespace-auth-rule.md)
>-    [Vytvoření obor služby Bus frontě](service-bus-resource-manager-namespace-queue.md)
>-    [Vytvoření obor Bus služby](service-bus-resource-manager-namespace.md)
>-    [Vytvoření události rozbočovače názvů ke skupině centrální událostí a příjemce](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)
>
>Nejnovější šablony, navštěvujte blog do Galerie [Šablon Azure rychlý úvod][] a vyhledejte "Služby Bus."

## <a name="what-will-you-deploy"></a>Co se nasadit?

Pomocí této šablony můžete nasadit služby Bus obor názvů s téma a předplatné.

[Služba Bus témata a předplatná](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) poskytují-n forma komunikace ve vzorci *publikovat nebo přihlášení k odběru* .

Automatické spuštění nasazení, klikněte na toto tlačítko:

[![Nasazení Azure](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-and-subscription%2Fazuredeploy.json)

## <a name="parameters"></a>Parametry

Pomocí Správce prostředků Azure definovat parametry pro hodnoty, které chcete zadat při nasazení šablony. Šablona obsahuje oddílu s názvem `Parameters` , která obsahuje všechny hodnoty parametrů. Je třeba definovat parametr hodnoty, které se liší podle projektu, který nasazujete nebo podle prostředí, které nasazujete. Parametry pro hodnoty, které bude vždy, zůstane stejná není definovat. Každá hodnota parametru slouží v šabloně k definování zdroje, které používají.

Šablona definuje následujících parametrů.

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName

Název oboru Bus služby chcete vytvořit.

```
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="servicebustopicname"></a>serviceBusTopicName

Název tématu vytvořené v oboru Bus služby.

```
"serviceBusTopicName": {
"type": "string"
}
```

### <a name="servicebussubscriptionname"></a>serviceBusSubscriptionName

Název předplatného vytvořené v oboru Bus služby.

```
"serviceBusSubscriptionName": {
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

Vytvoří služby Bus obor názvů typu **zprávy**s téma a předplatné.

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
            "name": "[parameters('serviceBusTopicName')]",
            "type": "Topics",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusTopicName')]",
            },
            "resources": [{
                "apiVersion": "[variables('sbVersion')]",
                "name": "[parameters('serviceBusSubscriptionName')]",
                "type": "Subscriptions",
                "dependsOn": [
                    "[parameters('serviceBusTopicName')]"
                ],
                "properties": {}
            }]
        }]
    }]
```

## <a name="commands-to-run-deployment"></a>Příkazy pro spuštění nasazení

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>Prostředí PowerShell

```
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-and-subscription/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure rozhraní příkazového řádku

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-and-subscription/azuredeploy.json>
```

## <a name="next-steps"></a>Další kroky

Teď, když jste vytvořili a nasazené zdrojů pomocí Správce prostředků Azure, přečtěte si, jak spravovat tyto materiály pomocí těchto článků:

- [Správa služby Bus pomocí prostředí PowerShell](service-bus-powershell-how-to-provision.md)
- [Přidávání a používání zdrojů Bus služby v Průzkumníkovi Bus služby](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)


  [Vytváření šablon správce prostředků Azure]: ../resource-group-authoring-templates.md
  [Rychlý úvod Azure šablony]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
  [Obor názvů služby Bus s téma a předplatného]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-and-subscription/
