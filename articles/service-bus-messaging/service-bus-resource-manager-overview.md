<properties
    pageTitle="Vytvoření Bus služby zdrojů pomocí šablony správce prostředků Azure | Microsoft Azure"
    description="Když použijete šablonu správce prostředků Azure automatizovat vytváření poštovních Bus služby zdrojů"
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
    ms.author="sethm"/>

# <a name="create-service-bus-resources-using-azure-resource-manager-templates"></a>Vytvoření Bus služby zdrojů pomocí šablony správce prostředků Azure

Tento článek popisuje, jak vytvářet a publikovat Bus služby a události rozbočovače zdrojů pomocí Správce prostředků Azure šablony prostředí PowerShell a poskytovatele služeb Bus zdroje.

Azure šablony správce zdrojů můžete definovat zdrojů nasazení řešení a zadejte parametry a proměnných, které vám umožní vstupní hodnoty na jiném prostředí. Šablona se skládá z JSON a výrazy, které můžete použít k vytvoření hodnoty pro nasazení. Podrobné informace o vytváření správce prostředků Azure šablony a popis formátu šablony najdete v článku [vytváření správce prostředků Azure šablony](../resource-group-authoring-templates.md). 

>[AZURE.NOTE] Příklady v tomto článku ukazují, jak správce prostředků Azure použít k vytváření názvů Bus služby a zpráv entity (fronty). Další příklady šablony Navštěvujte blog o do [Galerie šablon Azure rychlý úvod][] a vyhledejte "Služby Bus."

## <a name="service-bus-and-event-hubs-resource-manager-templates"></a>Šablony Bus služby a správce prostředků rozbočovače události

Tyto služby Bus a správce prostředků Azure rozbočovače události šablony jsou k dispozici ke stažení a instalaci. Klepněte na následující odkazy podrobnosti o každém z nich, s odkazy na šablony na GitHub: 

- [Vytvoření obor Bus služby](service-bus-resource-manager-namespace.md)
- [Vytvoření obor služby Bus frontě](service-bus-resource-manager-namespace-queue.md)
- [Vytvoření služby Bus obor názvů s téma a předplatného](service-bus-resource-manager-namespace-topic.md)
- [Vytvoření obor služby Bus s fronty a ověření pravidla](service-bus-resource-manager-namespace-auth-rule.md)
- [Vytvoření události rozbočovače názvů ke skupině centrální událostí a příjemce](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)

## <a name="deploy-with-powershell"></a>Nasazení pomocí prostředí PowerShell

Následující postup popisuje, jak pomocí Powershellu nasazení šablonu správce prostředků Azure, která vytvoří obor **Standardní** služba Bus osy a fronty v rámci tohoto názvů. V tomto příkladu je založené na šabloně [vytvořit obor služby Bus frontě](https://github.com/Azure/azure-quickstart-templates/tree/master/201-servicebus-create-queue) . Přibližná pracovní postup je následující:

1. Nainstalujte prostředí PowerShell.
2. Vytvoření šablony a (volitelně) souboru parametrů.
2. V prostředí PowerShell Přihlaste se k účtu Azure.
3. Vytvoření nové skupiny prostředků Pokud neexistuje.
4. Otestujte nasazení.
5. Pokud chcete, nastavte režimu nasazení.
6. Nasazení šablony.

Podrobné informace o nasazení šablon správce prostředků Azure tématech [nasazení se šablonami správce prostředků Azure][].

### <a name="install-powershell"></a>Instalace prostředí PowerShell

Nainstalujte prostředí PowerShell Azure podle pokynů uvedených v [článku Jak nainstalovat a nakonfigurovat Azure Powershellu](../powershell-install-configure.md).

### <a name="create-a-template"></a>Vytvoření šablony

Zkopírovat nebo zkopírovat [201 servicebus – vytvoření fronty](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.json) z GitHub:

```
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "type": "string",
            "metadata": {
                "description": "Name of the Service Bus namespace"
            }
        },
        "serviceBusQueueName": {
            "type": "string",
            "metadata": {
                "description": "Name of the Queue"
            }
        },
        "serviceBusApiVersion": {
            "type": "string",
            "defaultValue": "2015-08-01",
            "metadata": {
                "description": "Service Bus ApiVersion used by the template"
            }
        }
    },
    "variables": {
        "location": "[resourceGroup().location]",
        "sbVersion": "[parameters('serviceBusApiVersion')]",
        "defaultSASKeyName": "RootManageSharedAccessKey",
        "authRuleResourceId": "[resourceId('Microsoft.ServiceBus/namespaces/authorizationRules', parameters('serviceBusNamespaceName'), variables('defaultSASKeyName'))]"
    },
    "resources": [{
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
                "path": "[parameters('serviceBusQueueName')]"
            }
        }]
    }],
    "outputs": {
        "NamespaceConnectionString": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryConnectionString]"
        },
        "SharedAccessPolicyPrimaryKey": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryKey]"
        }
    }
}
```

### <a name="create-a-parameters-file-optional"></a>Vytvoření souboru parametrů (volitelné)

Při použití volitelné parametry souboru, zkopírujte soubor [201 servicebus – vytvoření fronty](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.parameters.json) . Nahraďte hodnotu `serviceBusNamespaceName` s názvem oboru Bus služby chcete vytvořit v tomto nasazení a nahraďte hodnotu `serviceBusQueueName` s názvem fronty, které chcete vytvořit. 

```
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "value": "<myNamespaceName>"
        },
        "serviceBusQueueName": {
            "value": "<myQueueName>"
        },
        "serviceBusApiVersion": {
            "value": "2015-08-01"
        }
    }
}
```

Další informace najdete v tématu [soubor parametrů](../resource-group-template-deploy.md#parameter-file) .

### <a name="log-in-to-azure-and-set-the-azure-subscription"></a>Přihlaste se k Azure a nastavte Azure předplatného

Na příkazovém řádku prostředí PowerShell spusťte tento příkaz:

```
Login-AzureRmAccount
```

Zobrazí se výzva k přihlášení k účtu Azure. Po přihlášení, spusťte tento příkaz zobrazíte předplatné k dispozici.

```
Get-AzureRMSubscription
```

Tento příkaz vrátí seznam dostupných Azure předplatného. Zvolte předplatné pro aktuální relaci spuštěním následujícího příkazu. Nahrazení `<YourSubscriptionId>` s identifikátor GUID Azure předplatné, které chcete použít.

```
Set-AzureRmContext -SubscriptionID <YourSubscriptionId>
```

### <a name="set-the-resource-group"></a>Nastavení skupiny zdrojů

Pokud nemáte existující skupiny zdrojů, vytvoření nové skupiny prostředků pomocí příkazu **Nový AzureRmResourceGroup** . Zadejte název pole Skupina zdroje a umístění, do kterého chcete použít. Příklad:

```
New-AzureRmResourceGroup -Name MyDemoRG -Location "West US"
```

Pokud je úspěšná, se zobrazí souhrn nové skupiny prostředků.

```
ResourceGroupName : MyDemoRG
Location          : westus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/<GUID>/resourceGroups/MyDemoRG
```

### <a name="test-the-deployment"></a>Testování nasazení

Ověření nasazení spuštěním `Test-AzureRmResourceGroupDeployment` rutiny. Při testování nasazení, zadejte parametry přesně stejně jako při provádění nasazení.

```
Test-AzureRmResourceGroupDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

### <a name="create-the-deployment"></a>Vytvoření nasazení

Pokud chcete vytvořit nové nasazení, spusťte `New-AzureRmResourceGroupDeployment` příkaz a zadejte potřebné parametry po zobrazení výzvy. Parametry obsahovat název pro nasazení, název skupiny zdrojů a na cestu nebo adresu URL pro soubor šablony. Pokud není zadán parametr **režimu** , bude použita výchozí hodnota **Přírůstková** . Další informace najdete v tématu [Přírůstková a dokončení nasazení](../resource-group-template-deploy.md#incremental-and-complete-deployments).

Tento příkaz zobrazí výzvu k tři parametry v okně Powershellu:

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

Chcete-li soubor parametry místo toho, použijte následující příkaz.

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -TemplateParameterFile <path to parameters file>\azuredeploy.parameters.json
```

Můžete také vložené parametry při spuštění rutinu nasazení. Příkaz vypadá takto:

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -parameterName "parameterValue"
```

[Dokončení](../resource-group-template-deploy.md#incremental-and-complete-deployments) nasazení spustíte nastavte parametr **režimu** **Dokončeno**:

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -Mode Complete -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json 
```

### <a name="verify-the-deployment"></a>Ověření nasazení

Pokud zdroje jsou umístěné úspěšně, přehled nasazení se zobrazí v okně Powershellu:

```
DeploymentName    : MyDemoDeployment
ResourceGroupName : MyDemoRG
ProvisioningState : Succeeded
Timestamp         : 4/19/2016 10:38:30 PM
Mode              : Incremental
TemplateLink      :
Parameters        :
                    Name             Type                       Value
                    ===============  =========================  ==========
                    serviceBusNamespaceName  String             <namespaceName>
                    serviceBusQueueName  String                 <queueName>
                    serviceBusApiVersion  String                2015-08-01

```

## <a name="next-steps"></a>Další kroky

Teď vidíte základní pracovní postup a příkazy pro nasazení šablony pro správce prostředků Azure. Podrobnější informace získáte následující odkazy:

- [Azure Přehled Správce zdrojů][]
- [Nasazení zdroje se šablonami správce prostředků Azure][]
- [Vytváření šablon](../resource-group-authoring-templates.md)


[Azure Přehled Správce zdrojů]: ../resource-group-overview.md
[Nasazení zdroje se šablonami správce prostředků Azure]: ../resource-group-template-deploy.md
[Azure Galerie šablon rychlý úvod]: https://azure.microsoft.com/documentation/templates/?term=service+bus