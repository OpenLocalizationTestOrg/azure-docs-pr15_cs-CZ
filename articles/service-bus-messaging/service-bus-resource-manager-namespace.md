<properties
    pageTitle="Vytvoření obor služby Bus pomocí šablony správce prostředků | Microsoft Azure"
    description="Správce prostředků Azure šablonu použít k vytvoření obor Bus služby"
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
    ms.date="10/04/2016"
    ms.author="sethm;shvija"/>

# <a name="create-a-service-bus-namespace-using-an-azure-resource-manager-template"></a>Vytvoření obor služby Bus pomocí šablony správce prostředků Azure

Tento článek popisuje, jak používat šablonu správce prostředků Azure, která vytvoří služby Bus obor názvů typu **zprávy** s jinou SKU standardní/Basic. V článku také definuje parametry určených pro spuštění nasazení. Pomocí této šablony vlastního nasazení nebo přizpůsobit tak, aby vyhovovala vašim požadavkům.

Další informace o vytváření šablon najdete v článku [vytváření správce prostředků Azure šablony][].

Dokončení šablonu vyberete najdete v článku [služby Bus názvů šablony][] na GitHub.

>[AZURE.NOTE] Následující správce prostředků Azure šablony jsou k dispozici ke stažení a nasazení. 
>
>-    [Vytvoření události rozbočovače názvů ke skupině centrální událostí a příjemce](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)
>-    [Vytvoření obor služby Bus frontě](service-bus-resource-manager-namespace-queue.md)
>-    [Vytvoření služby Bus obor názvů s téma a předplatného](service-bus-resource-manager-namespace-topic.md)
>-    [Vytvoření obor služby Bus s fronty a ověření pravidla](service-bus-resource-manager-namespace-auth-rule.md)
>
>Nejnovější šablony, navštěvujte blog do Galerie [Šablon Azure rychlý úvod][] a vyhledejte Bus služby.

## <a name="what-will-you-deploy"></a>Co se nasadit?

Pomocí této šablony nasadíte obor Bus služby [základní, standardní nebo Premium](https://azure.microsoft.com/pricing/details/service-bus/) SKU.

Automatické spuštění nasazení, klikněte na toto tlačítko:

[![Nasazení Azure](./media/service-bus-resource-manager-namespace/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-servicebus-create-namespace%2Fazuredeploy.json)

## <a name="parameters"></a>Parametry

Pomocí Správce prostředků Azure definovat parametry pro hodnoty, které chcete zadat při nasazení šablony. Šablona obsahuje oddílu s názvem `Parameters` , která obsahuje všechny hodnoty parametrů. Je třeba definovat parametr hodnoty, které se liší podle projektu, který nasazujete nebo podle prostředí, které nasazujete. Parametry pro hodnoty, které bude vždy, zůstane stejná není definovat. Každá hodnota parametru slouží v šabloně k definování zdroje, které používají.

Tato šablona určuje následujících parametrů.

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

### <a name="servicebussku"></a>serviceBusSKU

Název služby Bus [SKU](https://azure.microsoft.com/pricing/details/service-bus/) vytvořit.

```
"serviceBusSku": { 
    "type": "string", 
    "allowedValues": [ 
        "Basic", 
        "Standard",
        "Premium" 
    ], 
    "defaultValue": "Standard", 
    "metadata": { 
        "description": "The messaging tier for service Bus namespace" 
    } 

```

Šablona definuje hodnoty, které jsou povoleny pro tento parametr (základní, standardní nebo Premium) a přiřadí výchozí hodnotu (standardní), pokud je zadána žádná hodnota.

Další informace o služby Bus ceny najdete v článku [ceny Bus služby a fakturaci][].

### <a name="servicebusapiversion"></a>serviceBusApiVersion

Rozhraní API služeb Bus verze šablony.

```
"serviceBusApiVersion": { 
       "type": "string", 
       "defaultValue": "2015-08-01", 
       "metadata": { 
           "description": "Service Bus ApiVersion used by the template" 
       } 
```

## <a name="resources-to-deploy"></a>Materiály pro nasazení

### <a name="service-bus-namespace"></a>Obor názvů Bus služby

Vytvoří služby Bus obor názvů typu **zasílání zpráv**.

```
"resources": [
    {
        "apiVersion": "[parameters('serviceBusApiVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
            "tier": "Standard"
        },
        "properties": {
        }
    }
]
```

## <a name="commands-to-run-deployment"></a>Příkazy pro spuštění nasazení

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>Prostředí PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

### <a name="azure-cli"></a>Azure rozhraní příkazového řádku

```
azure config mode arm

azure group deployment create <my-resource-group> <my-deployment-name> --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

## <a name="next-steps"></a>Další kroky

Teď, když jste vytvořili a nasazené zdrojů pomocí Správce prostředků Azure, přečtěte si, jak spravovat tyto materiály o těchto článků:

- [Správa služby Bus pomocí prostředí PowerShell](service-bus-powershell-how-to-provision.md)
- [Přidávání a používání zdrojů Bus služby v Průzkumníkovi Bus služby](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)

  [Vytváření šablon správce prostředků Azure]: ../resource-group-authoring-templates.md
  [Šablona služby Bus obor názvů]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-servicebus-create-namespace/
  [Rychlý úvod Azure šablony]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Služba Bus ceny a fakturace]: https://azure.microsoft.com/documentation/articles/service-bus-pricing-billing/
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
