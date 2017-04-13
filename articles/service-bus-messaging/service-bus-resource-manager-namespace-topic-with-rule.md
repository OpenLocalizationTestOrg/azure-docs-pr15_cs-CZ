<properties
    pageTitle="Vytvoření obor služby Bus s tématu předplatného a pravidel pomocí šablony správce prostředků Azure | Microsoft Azure"
    description="Vytvoření služby Bus obor názvů s téma, předplatné a pravidla pomocí šablony správce prostředků Azure"
    services="service-bus"
    documentationCenter=".net"
    authors="ShubhaVijayasarathy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="ShubhaVijayasarathy"/>

# <a name="create-a-service-bus-namespace-with-topic-subscription-and-rule-using-an-azure-resource-manager-template"></a>Vytvoření služby Bus obor názvů s téma, předplatné a pravidlo pomocí šablony pro správce prostředků Azure

Tento článek ukazuje, jak používat šablonu správce prostředků Azure, která vytvoří obor služby Bus téma, předplatné a pravidlo (filtr). Se naučíte, jak definovat prostředků, které používají a jak definovat parametry, které jsou uvedené provedení nasazení. Můžete pomocí této šablony vlastního nasazení nebo přizpůsobit tak, aby vyhovovala vašim požadavkům

Další informace o vytváření šablon najdete v tématu [vytváření správce prostředků Azure šablony][].

Další informace o praktické cvičení a vzorky Azure zdroje konvence najdete v tématu [Zásady vytváření názvů zdrojů Azure][].

Dokončení šablonu vyberete najdete v článku šabloně [služby Bus obor názvů s téma, předplatného a pravidel][] .

>[AZURE.NOTE] Následující správce prostředků Azure šablony jsou k dispozici ke stažení a nasazení.
>
>-    [Vytvoření obor služby Bus s fronty a ověření pravidla](service-bus-resource-manager-namespace-auth-rule.md)
>-    [Vytvoření obor služby Bus frontě](service-bus-resource-manager-namespace-queue.md)
>-    [Vytvoření obor Bus služby](service-bus-resource-manager-namespace.md)
>-    [Vytvoření služby Bus obor názvů s téma a předplatného](service-bus-resource-manager-namespace-topic.md)
>
>Nejnovější šablony, navštěvujte blog do Galerie [Šablon Azure rychlý úvod][] a vyhledejte Bus služby.

## <a name="what-will-you-deploy"></a>Co se nasadit?

Pomocí této šablony můžete nasadit služby Bus obor názvů s téma, předplatné a pravidlo (filtr).

[Služba Bus témata a předplatná](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) poskytují-n forma komunikace ve vzorci *publikovat nebo přihlášení k odběru* . Při použití témata a předplatná komponent distribuované aplikace není přímo vzájemně komunikovat, místo toho si vyměňovat zprávy přes tematickým, která funguje jako zprostředkovatele. Předplatné na téma podobá virtuální fronta, která přijímá kopie zpráv, které byly odeslány tématu. Filtru u předplatného umožňuje lze určit, které zprávy poslané na téma by se měly v rámci předplatného konkrétním tématu.

## <a name="what-are-rules-filters"></a>Jaká pravidla (filtry)?

V mnoha případech musí být zprávy, které mají zvláštní znaky zpracovány různými způsoby. Aby, můžete nakonfigurovat předplatná pro vyhledání zpráv, které mají požadované vlastnosti a pak proveďte některé změny těchto vlastností. Během služby Bus předplatná zobrazit všechny zprávy na téma, můžete kopírovat podmnožinu tyto zprávy pouze do fronty virtuální předplatného. To lze provést pomocí filtrů předplatného. Další informace o rules(filters) najdete v tématu [frontách Bus služby, témata a předplatná][].

Automatické spuštění nasazení, klikněte na toto tlačítko:

[![Nasazení Azure](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-subscription-rule%2Fazuredeploy.json)

## <a name="parameters"></a>Parametry

S Azure správce prostředků, měli byste definovat parametry pro hodnoty, které chcete zadat při nasazení šablony. Šablona obsahuje oddílu s názvem `Parameters` , která obsahuje všechny hodnoty parametrů. Je třeba definovat parametr hodnoty, které se liší podle projektu, který nasazujete nebo podle prostředí, které nasazujete. Není definovat parametry pro hodnoty, které vždy zůstávají stejné. Každá hodnota parametru slouží v šabloně k definování zdroje, které používají.

Šablona definuje následujících parametrů:

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
### <a name="servicebusrulename"></a>serviceBusRuleName

Název rule(filter) vytvořené v oboru Bus služby.

```
   "serviceBusRuleName": {
   "type": "string",
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

Vytvoří služby Bus obor názvů typu **zprávy**s téma a předplatné a pravidla.

```
 "resources": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "sku": {
            "name": "Standard",
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
                "path": "[parameters('serviceBusTopicName')]"
            },
            "resources": [{
                "apiVersion": "[variables('sbVersion')]",
                "name": "[parameters('serviceBusSubscriptionName')]",
                "type": "Subscriptions",
                "dependsOn": [
                    "[parameters('serviceBusTopicName')]"
                ],
                "properties": {},
                "resources": [{
                    "apiVersion": "[variables('sbVersion')]",
                    "name": "[parameters('serviceBusRuleName')]",
                    "type": "Rules",
                    "dependsOn": [
                        "[parameters('serviceBusSubscriptionName')]"
                    ],
                    "properties": {
                        "filter": {
                            "sqlExpression": "StoreName = 'Store1'"
                        },
                        "action": {
                            "sqlExpression": "set FilterTag = 'true'"
                        }
                    }
                }]
            }]
        }]
    }]
```

## <a name="commands-to-run-deployment"></a>Příkazy pro spuštění nasazení

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>Prostředí PowerShell

```
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure rozhraní příkazového řádku

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="next-steps"></a>Další kroky

Teď, když jste vytvořili a nasazené zdrojů pomocí Správce prostředků Azure, přečtěte si, jak spravovat tyto materiály pomocí těchto článků:

- [Správa Bus služby Azure pomocí automatizace Azure](service-bus-automation-manage.md)
- [Správa služby Bus pomocí prostředí PowerShell](service-bus-powershell-how-to-provision.md)
- [Přidávání a používání zdrojů Bus služby v Průzkumníkovi Bus služby](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)


  [Vytváření šablon správce prostředků Azure]: ../resource-group-authoring-templates.md
  [Rychlý úvod Azure šablony]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
  [Konvence pojmenování Azure zdroje]: https://azure.microsoft.com/en-us/documentation/articles/guidance-naming-conventions/
  [Obor názvů služby Bus s téma, předplatné a pravidla]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-subscription-rule/
  [Služba Bus fronty, témata a předplatné]:service-bus-queues-topics-subscriptions.md
  
