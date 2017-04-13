<properties
    pageTitle="Vytvoření pravidla služby Bus se tak mohli ověřovat pomocí šablony správce prostředků Azure | Microsoft Azure"
    description="Vytvoření služby Bus ověřovací pravidlo pro obor názvů a fronty pomocí šablony správce prostředků Azure"
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

# <a name="create-a-service-bus-authorization-rule-for-namespace-and-queue-using-an-azure-resource-manager-template"></a>Vytvoření služby Bus ověřovací pravidlo pro obor názvů a fronty pomocí šablony pro správce prostředků Azure

Tento článek ukazuje, jak používat šablonu správce prostředků Azure, která vytvoří [ověřovací pravidlo](service-bus-authentication-and-authorization.md#shared-access-signature-authentication) pro službu Bus názvů a fronty. Naučíte se, jak definovat prostředků, které používají a jak definovat parametry, které jsou uvedené provedení nasazení. Pomocí této šablony vlastního nasazení nebo přizpůsobit tak, aby vyhovovala vašim požadavkům.

Další informace o vytváření šablon najdete v článku [vytváření správce prostředků Azure šablony][].

Dokončení šablonu vyberete najdete v článku [šablony pravidla ověřování služby Bus][] na GitHub.

>[AZURE.NOTE] Následující správce prostředků Azure šablony jsou k dispozici ke stažení a nasazení.
>
>-    [Vytvoření události rozbočovače názvů ke skupině centrální událostí a příjemce](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)
>-    [Vytvoření obor služby Bus frontě](service-bus-resource-manager-namespace-queue.md)
>-    [Vytvoření služby Bus obor názvů s téma a předplatného](service-bus-resource-manager-namespace-topic.md)
>-    [Vytvoření obor Bus služby](service-bus-resource-manager-namespace.md)
>
>Nejnovější šablony, navštěvujte blog do Galerie [Šablon Azure rychlý úvod][] a vyhledejte "Služby Bus."

## <a name="what-will-you-deploy"></a>Co se nasadit?

Pomocí této šablony můžete nasadit služby Bus ověřovací pravidlo pro zasílání zpráv entity (v tomto případě fronty) a názvů.

Tato šablona používá ověřování [Sdílené přidružení (zabezpečení aplikace Access podpis)](service-bus-sas-overview.md) . Přidružení zabezpečení aplikacím ověřování služby Bus pomocí přístupové klávesy nakonfigurovaný na obor nebo zasílání zpráv entity (fronty nebo tématu), ke které jsou přidružené určitého práva. Tento klíč pak můžete použít ke generování token přidružení zabezpečení, klienty zase můžete ověřit Bus služby.

Automatické spuštění nasazení, klikněte na toto tlačítko:

[![Nasazení Azure](./media/service-bus-resource-manager-namespace-auth-rule/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F301-servicebus-create-authrule-namespace-and-queue%2Fazuredeploy.json)

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

### <a name="namespaceauthorizationrulename"></a>namespaceAuthorizationRuleName 

Název ověřovací pravidlo pro obor.

```
"namespaceAuthorizationRuleName ": {
"type": "string"
}
```

### <a name="servicebusqueuename"></a>serviceBusQueueName

Název fronty v oboru Bus služby.

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

Vytvoří služby Bus obor názvů typu **zasílání zpráv**a služby Bus ověřovací pravidlo pro obor názvů a entity.

```
"resources": [
        {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusNamespaceName')]",
            "type": "Microsoft.ServiceBus/namespaces",
            "location": "[variables('location')]",
            "kind": "Messaging",
            "sku": {
                "name": "StandardSku",
                "tier": "Standard"
            },
            "resources": [
                {
                    "apiVersion": "[variables('sbVersion')]",
                    "name": "[parameters('serviceBusQueueName')]",
                    "type": "Queues",
                    "dependsOn": [
                        "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
                    ],
                    "properties": {
                        "path": "[parameters('serviceBusQueueName')]"
                    },
                    "resources": [
                        {
                            "apiVersion": "[variables('sbVersion')]",
                            "name": "[parameters('queueAuthorizationRuleName')]",
                            "type": "authorizationRules",
                            "dependsOn": [
                                "[parameters('serviceBusQueueName')]"
                            ],
                            "properties": {
                                "Rights": ["Listen"]
                            }
                        }
                    ]
                }
            ]
        }, {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[variables('namespaceAuthRuleName')]",
            "type": "Microsoft.ServiceBus/namespaces/authorizationRules",
            "dependsOn": ["[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"],
            "location": "[resourceGroup().location]",
            "properties": {
                "Rights": ["Send"]
            }
        }
    ]
```

## <a name="commands-to-run-deployment"></a>Příkazy pro spuštění nasazení

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>Prostředí PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure rozhraní příkazového řádku

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="next-steps"></a>Další kroky

Teď, když jste vytvořili a nasazené zdrojů pomocí Správce prostředků Azure, přečtěte si, jak spravovat tyto materiály pomocí těchto článků:

- [Správa služby Bus pomocí prostředí PowerShell](service-bus-powershell-how-to-provision.md)
- [Přidávání a používání zdrojů Bus služby v Průzkumníkovi Bus služby](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)
- [Služba Bus a tak mohli ověřovat](service-bus-authentication-and-authorization.md)

  [Vytváření šablon správce prostředků Azure]: ../resource-group-authoring-templates.md
  [Rychlý úvod Azure šablony]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
  [Šablony pravidel auth Bus služby]: https://github.com/Azure/azure-quickstart-templates/blob/master/301-servicebus-create-authrule-namespace-and-queue/
