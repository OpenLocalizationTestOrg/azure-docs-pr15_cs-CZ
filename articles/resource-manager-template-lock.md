<properties
   pageTitle="Správce prostředků šablony pro zdroje zámek | Microsoft Azure"
   description="Zobrazuje schématu správce prostředků pro nasazení uzamčení prostředků pomocí šablony."
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
   ms.date="10/03/2016"
   ms.author="tomfitz"/>

# <a name="resource-locks-template-schema"></a>Schéma šablony zdroje zámků webů

Vytvoří zámek na zdroj a jeho podřízených zdroje.

## <a name="schema-format"></a>Formát schématu

Pokud chcete vytvořit zámek, přidejte následující schéma do oddílu zdroje šablony.
    
    {
        "type": enum,
        "apiVersion": "2015-01-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "level": enum,
            "notes": string
        }
    }



## <a name="values"></a>Hodnoty

Následující tabulka popisuje hodnoty, které je potřeba nastavit ve schématu.

| Jméno | Povinné | Popis |
| ---- | -------- | ----------- |
| Typ | Ano | Typ zdroje k vytvoření.<br /><br />Pro zdroje:<br />**{názvů} / {zadejte} / poskytovatelů/zámků webů**<br /><br/>Pro skupiny zdrojů:<br />**Microsoft.Authorization/locks** |
| apiVersion | Ano | Verze rozhraní API pro použití při vytváření zdroje.<br /><br />Použití:<br />**2015 01 01**<br /><br /> |
| Jméno | Ano | Hodnota, která určuje zdroje, chcete-li uzamknout a název zámek. Můžou obsahovat až 64 znaků a nesmí obsahovat <>; % &,?, nebo libovolný řídicí znaky.<br /><br />Pro zdroje:<br />**{resource}/Microsoft.Authorization/{lockname}**<br /><br />Pro skupiny zdrojů:<br />**{lockname}** |
| dependsOn | Ne | Seznam oddělený čárkami zdroje názvy nebo identifikátory jedinečné zdroje.<br /><br />Kolekce prostředky, které tato zamknout závisí na. Zdroje, které jsou zamknutí po nasazení ve stejné šabloně, zahrnout tohoto názvu zdroje: Tento element zajistit, že nejdřív nasazení zdroje. | 
| Vlastnosti | Ano | Objekt, který identifikuje typ lock a poznámky k zámek.<br /><br />Zobrazit [Vlastnosti objektu](#properties-object). |  

### <a name="properties-object"></a>vlastnosti objektu

| Jméno | Povinné | Popis |
| ---- | -------- | ----------- |
| úroveň   | Ano | Typ lock vyrovnat obor.<br /><br />**CannotDelete** - uživatelů můžete upravit zdroje, ale ne.<br />**Jen pro čtení** - uživatelé mohou číst ze zdroje, ale nemůžete ho odstranit nebo provádět žádné akce. |
| Poznámky   | Ne | Popis zámek. Může být 512 znaků. |


## <a name="how-to-use-the-lock-resource"></a>Použití zámku zdroje

Přidání tohoto prostředku do šablony nechcete, aby určité akce zdroje. Uzamčení platí pro všechny uživatele a skupiny.

Vytvářet a odstraňovat zámky správy, musíte mít přístup k **Microsoft.Authorization/** * nebo * *Microsoft.Authorization/locks/* ** Akce. Předdefinované rolí pouze **vlastník** a **uživatel přístup správce ** udělena tato akce. Informace o řízení přístupu na základě rolí v tématu [Řízení přístupu na základě rolí Azure](./active-directory/role-based-access-control-configure.md).

Uzamčení se použije pro daný zdroj a podřízené zdroje.

Můžete odebrat zámek s příkazu Powershellu **AzureRmResourceLock odebrat** nebo [Odstranit operace](https://msdn.microsoft.com/library/azure/mt204562.aspx) rozhraní REST API.

## <a name="examples"></a>Příklady

Následující příklad se týká zámek nelze odstranit web appu.

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
                "name": "[variables('siteName')]",
                "type": "Microsoft.Web/sites",
                "location": "[resourceGroup().location]",
                "properties": {
                    "serverFarmId": "[parameters('hostingPlanName')]"
                },
            },
            {
                "type": "Microsoft.Web/sites/providers/locks",
                "apiVersion": "2015-01-01",
                "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
                "dependsOn": [ "[variables('siteName')]" ],
                "properties":
                {
                    "level": "CannotDelete",
                    "notes": "my notes"
                }
             }
        ],
        "outputs": {}
    }

Následující příklad se týká zámek nelze odstranit skupina zdroje.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.Authorization/locks",
                "apiVersion": "2015-01-01",
                "name": "MyGroupLock",
                "properties":
                {
                    "level": "CannotDelete",
                    "notes": "my notes"
                }
            }
        ],
        "outputs": {}
    }

## <a name="next-steps"></a>Další kroky

- Informace o struktuře šablony najdete v tématu [vytváření správce prostředků Azure šablony](resource-group-authoring-templates.md).
- Další informace o uzamčení tématech [Zámek pomocí Správce prostředků Azure](resource-group-lock-resources.md).
