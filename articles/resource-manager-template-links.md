<properties
   pageTitle="Správce prostředků šablony propojení zdroje | Microsoft Azure"
   description="Zobrazuje schématu správce prostředků pro nasazení propojení mezi související materiály pomocí šablony."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/05/2016"
   ms.author="tomfitz"/>

# <a name="resource-links-template-schema"></a>Schéma šablony odkazy zdroje

Vytvoří odkazu mezi dva zdroje. Odkaz bude použit pro zdroj jmenoval zdroje zdroje. Druhá zdroje v poli odkaz se nazývá cílový prostředek.

## <a name="schema-format"></a>Formát schématu

Pokud chcete vytvořit odkaz, přidejte následující schéma oddílu zdroje šablony.
    
    {
        "type": enum,
        "apiVersion": "2015-01-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "targetId": string,
            "notes": string
        }
    }



## <a name="values"></a>Hodnoty

Následující tabulka popisuje hodnoty, které je potřeba nastavit ve schématu.

| Jméno | Hodnota |
| ---- | ---- |
| Typ | Výčet<br />Povinné<br />**{názvů} / {zadejte} / poskytovatelů/odkazy**<br /><br />Typ zdroje k vytvoření. {Oboru} a {typ} hodnoty se vztahují na poskytovatele obor názvů a zdroje typ zdroje zdroje. |
| apiVersion | Výčet<br />Povinné<br />**2015 01 01**<br /><br />Verze rozhraní API pro použití při vytváření zdroje. |  
| Jméno | Řetězec<br />Povinné<br />**{resouce}/Microsoft.Resources/{linkname}**<br /> až 64 znaků a nesmí obsahovat <>; % &,?, nebo libovolný řídicí znaky.<br /><br />Hodnota, která určuje název zdroje prostředku i název odkaz. |
| dependsOn | Pole<br />Volitelné<br />Seznam oddělený čárkami zdroje názvy nebo identifikátory jedinečné zdroje.<br /><br />Kolekce prostředky, které závisí na tom tento odkaz. Pokud prostředky, které jsou propojení jsou umístěné ve stejné šablony, zahrnout: Tento element zajistit že nasazením nejprve tyto názvy zdrojů. | 
| Vlastnosti | Objekt<br />Povinné<br />[vlastnosti objektu](#properties)<br /><br />Objekt, který identifikuje zdroje chcete vytvořit odkaz a poznámky k propojení. |  

<a id="properties" />
### <a name="properties-object"></a>vlastnosti objektu

| Jméno | Hodnota |
| ------- | ---- |
| targetId | Řetězec<br />Povinné<br />**{pole číslo id zdroje}**<br /><br />Identifikátor cílové zdroje, pokud chcete vytvořit odkaz. |
| Poznámky | Řetězec<br />Volitelné<br />512 znaků<br /><br />Popis zámek. |


## <a name="how-to-use-the-link-resource"></a>Používání zdrojů odkaz

Použití odkazu mezi dva zdroje zdroje obsahujících závislost pokračuje po nasazení. Aplikace může například připojení k databázi v jiné skupině zdrojů. Tato závislost můžete definovat vytvořením odkazu z aplikace do databáze. Odkazy umožňují dokumentu vztah mezi dva zdroje. Vy nebo někdo jiný ve vaší organizaci můžete později, dotaz Zdroj odkazy a zjistíte, jak zdroje označené jako pracuje s další zdroje informací.

Na stejném předplatném musí patřit všech propojených zdrojů. Jednotlivé zdroje můžou být propojené s 50 další zdroje informací. Pokud některý z propojených zdrojů jsou odstranili nebo přesunuli, musí vlastník odkaz vyčistit zbývající odkaz.

Pracovat s odkazy až ZBÝVAJÍCÍ, najdete v článku [Propojených zdrojů](https://msdn.microsoft.com/library/azure/mt238499.aspx).

Pomocí následujícího příkazu Powershellu Azure zobrazíte všechny odkazy ve vašem předplatném. Můžete zadat další parametry chcete omezit výsledky.

    Get-AzureRmResource -ResourceType Microsoft.Resources/links -isCollection -ResourceGroupName <YourResourceGroupName>

## <a name="examples"></a>Příklady

Následující příklad se týká jen pro čtení zámek do webových aplikací.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "hostingPlanName": {
                "type": "string"
            }
        },
        "variables": {
            "siteName": "[concat('site',uniqueString(resourceGroup().id))]"
        },
        "resources": [
            {
                "apiVersion": "2015-08-01",
                "type": "Microsoft.Web/serverfarms",
                "name": "[parameters('hostingPlanName')]",
                "location": "[resourceGroup().location]",
                "sku": {
                    "tier": "Free",
                    "name": "f1",
                    "capacity": 0
                },
                "properties": {
                    "numberOfWorkers": 1
                }
            },
            {
                "apiVersion": "2015-08-01",
                "name": "[variables('siteName')]",
                "type": "Microsoft.Web/sites",
                "location": "[resourceGroup().location]",
                "dependsOn": [ "[parameters('hostingPlanName')]" ],
                "properties": {
                    "serverFarmId": "[parameters('hostingPlanName')]"
                }
            },
            {
                "type": "Microsoft.Web/sites/providers/links",
                "apiVersion": "2015-01-01",
                "name": "[concat(variables('siteName'),'/Microsoft.Resources/SiteToStorage')]",
                "dependsOn": [ "[variables('siteName')]" ],
                "properties": {
                    "targetId": "[resourceId('Microsoft.Storage/storageAccounts','storagecontoso')]",
                    "notes": "This web site uses the storage account to store user information."
                }
            }
        ],
        "outputs": {}
    }

## <a name="quickstart-templates"></a>Rychlý úvod šablony

Následující šablony rychlý úvod nasazení zdroje s odkazem.

- [Upozornit do fronty aplikaci pro použití logických operátorů](https://azure.microsoft.com/documentation/templates/201-alert-to-queue-with-logic-app)
- [Upozornění na časová rezerva aplikaci pro použití logických operátorů](https://azure.microsoft.com/documentation/templates/201-alert-to-slack-with-logic-app)
- [Vytvořit aplikaci pro rozhraní API s existující brány](https://azure.microsoft.com/documentation/templates/201-api-app-gateway-existing)
- [Vytvořit aplikaci pro rozhraní API s novou bránu](https://azure.microsoft.com/documentation/templates/201-api-app-gateway-new)
- [Vytvoření aplikace logiky plus rozhraní API aplikace použít šablonu?](https://azure.microsoft.com/documentation/templates/201-logic-app-api-app-create)
- [Použití logických operátorů aplikace, která odešle textové zprávy při upozornění](https://azure.microsoft.com/documentation/templates/201-alert-to-text-message-with-logic-app)


## <a name="next-steps"></a>Další kroky

- Informace o struktuře šablony najdete v tématu [vytváření správce prostředků Azure šablony](resource-group-authoring-templates.md).
