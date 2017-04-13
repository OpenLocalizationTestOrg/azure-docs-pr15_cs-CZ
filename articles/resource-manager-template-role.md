<properties
   pageTitle="Správce prostředků šablony pro přiřazení rolí | Microsoft Azure"
   description="Zobrazuje schématu správce prostředků pro nasazení přiřazování rolí pomocí šablony."
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

# <a name="role-assignments-template-schema"></a>Schéma šablony přiřazení rolí

Přiřadí uživateli, skupině nebo služby jistinu na zadaný obor role.

## <a name="resource-format"></a>Zdrojů – formát

Pokud chcete vytvořit přiřazování rolí, přidejte následující schéma do oddílu zdroje šablony.
    
    {
        "type": string,
        "apiVersion": "2015-07-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "roleDefinitionId": string,
            "principalId": string,
            "scope": string
        }
    }

## <a name="values"></a>Hodnoty

Následující tabulka popisuje hodnoty, které je potřeba nastavit ve schématu.

| Jméno | Povinné | Popis |
| ---- | -------- | ----------- |
| Typ | Ano    | Typ zdroje k vytvoření.<br /><br /> Pro pole Skupina zdroje:<br />**Microsoft.Authorization/roleAssignments**<br /><br />Zdroje:<br />**{obor názvů zprostředkovatele} / {typ zdroje} / poskytovatelů/roleAssignments** |
| apiVersion |Ano | Verze rozhraní API pro použití při vytváření zdroje.<br /><br /> Použití **2015 07 01**. | 
| Jméno | Ano | Globálně jedinečný identifikátor pro nové přiřazování rolí. |
| dependsOn | Ne | Hodnoty oddělené pole zdroj názvy nebo identifikátory jedinečné zdroje.<br /><br />Kolekce prostředky, které tato přiřazování rolí závisí na. Pokud přiřazení role, omezené zdroji a zdroje nasazenou v stejnou šablonu, zahrnout tohoto názvu zdroje: Tento element zajistit, že nejdřív nasazení zdroje. |
| Vlastnosti | Ano | Vlastnosti objektu, který identifikuje definice role, hlavní a rozsahu. |

### <a name="properties-object"></a>vlastnosti objektu

| Jméno | Povinné | Popis |
| ---- | -------- | ----------- |
| roleDefinitionId | Ano |  Identifikátor existující definice role pro použití v přiřazování rolí.<br /><br /> Použijte v tomto formátu:<br /> **/ subscriptions/{subscription-id}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}** |
| principalId | Ano | Globálně jedinečný identifikátor existující jistinu. Tato hodnota mapy ID uvnitř adresář a mohou odkazovat na uživatele, služby jistinu nebo skupiny zabezpečení. |
| rozsah | Ne | Rozsah niž platí pro toto přiřazení role.<br /><br />U skupin zdrojů pomocí:<br />**/resourceGroups/ /Subscriptions/ {id předplatného} {název zdroje skupiny}**  <br /><br />U zdroje můžete pomocí:<br />**/ subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{provider-namespace}/{resource-type}/{resource-name}** |  |


## <a name="how-to-use-the-role-assignment-resource"></a>Používání zdrojů přiřazení rolí

Přiřazení role do šablony přidat budete muset přidat uživatele, skupinu nebo služby základní k roli během nasazení. Přiřazení rolí se dědí od vyšší úrovně rozsah, takže pokud jste už přidali objekt zabezpečení na úrovni předplatné role, není potřeba přiřazení Skupina zdroje nebo zdroje.

Existuje více identifikátor hodnot, které je třeba zadat při práci s přiřazování rolí. Můžete získat hodnoty pomocí prostředí PowerShell nebo Azure rozhraní příkazového řádku.

### <a name="powershell"></a>Prostředí PowerShell

Název přiřazování rolí vyžaduje globálně jedinečný identifikátor. Můžete generovat nové identifikátor pro **název** :

    $name = [System.Guid]::NewGuid().toString()

Je možné získat identifikátor definici role:

    $roledef = (Get-AzureRmRoleDefinition -Name Reader).id

Načtení identifikátor pro jistinu s některým z následujících příkazů.

Pro skupinu název **auditory**:

    $principal = (Get-AzureRmADGroup -SearchString Auditors).id

Pro uživatele s názvem **exampleperson**:

    $principal = (Get-AzureRmADUser -SearchString exampleperson).id

Pro službu jistinu s názvem **exampleapp**:

    $principal = (Get-AzureRmADServicePrincipal -SearchString exampleapp).id 

### <a name="azure-cli"></a>Azure rozhraní příkazového řádku

Je možné získat identifikátor definici role:

    azure role show Reader --json | jq .[].Id -r

Načtení identifikátor pro jistinu s některým z následujících příkazů.

Pro skupinu název **auditory**:

    azure ad group show --search Auditors --json | jq .[].objectId -r

Pro uživatele s názvem **exampleperson**:

    azure ad user show --search exampleperson --json | jq .[].objectId -r

Pro službu jistinu s názvem **exampleapp**:

    azure ad sp show --search exampleapp --json | jq .[].objectId -r

## <a name="examples"></a>Příklady

Následující šablonu obdrží identifikátor pro roli a identifikátor pro uživatele, skupinu nebo jistinu služby. Slouží k přiřazování rolí na úrovni skupiny zdrojů.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "roleDefinitionId": {
                "type": "string"
            },
            "roleAssignmentId": {
                "type": "string"
            },
            "principalId": {
                "type": "string"
            }
        },
        "resources": [
            {
              "type": "Microsoft.Authorization/roleAssignments",
              "apiVersion": "2015-07-01",
              "name": "[parameters('roleAssignmentId')]",
              "properties":
              {
                "roleDefinitionId": "[concat(subscription().id, '/providers/Microsoft.Authorization/roleDefinitions/', parameters('roleDefinitionId'))]",
                "principalId": "[parameters('principalId')]",
                "scope": "[concat(subscription().id, '/resourceGroups/', resourceGroup().name)]"
              }
            }
        ],
        "outputs": {}
    }



Další šablony vytvoří účet úložiště a přiřadí roli Čtenář účtu úložiště. Identifikátory dvěma skupinami a roli Čtenář neobsahuje v šabloně zjednodušit nasazení. Jsou tyto hodnoty může načítá při nasazení prostřednictvím skriptu a předaný jako parametry.

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "roleName": {
          "type": "string"
        },
        "groupToAssign": {
          "type": "string",
          "allowedValues": [
            "Auditors",
            "Limited"
          ]
        }
      },
      "variables": {
        "readerRole": "[concat('/subscriptions/',subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7')]",
        "storageName": "[concat('storage', uniqueString(resourceGroup().id))]",
        "scope": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Storage/storageAccounts/', variables('storageName'))]",
        "Auditors": "1c272299-9729-462a-8d52-7efe5ece0c5c",
        "Limited": "7c7250f0-7952-441c-99ce-40de5e3e30b5",
        "fullRoleName": "[concat(variables('storageName'), '/Microsoft.Authorization/', parameters('roleName'))]"
      },
      "resources": [
        {
          "apiVersion": "2016-01-01",
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[variables('storageName')]",
          "location": "[resourceGroup().location]",
          "sku": {
            "name": "Standard_LRS"
          },
          "kind": "Storage",
          "tags": {
            "displayName": "MyStorageAccount"
          },
          "properties": {}
        },
        {
          "type": "Microsoft.Storage/storageAccounts/providers/roleAssignments",
          "apiVersion": "2015-07-01",
          "name": "[variables('fullRoleName')]",
          "dependsOn": [
            "[variables('storageName')]"
          ],
          "properties": {
            "roleDefinitionId": "[variables('readerRole')]",
            "principalId": "[variables(parameters('groupToAssign'))]"
          }
        }
      ]
    }

## <a name="quickstart-templates"></a>Rychlý úvod šablony

Následující šablony zobrazení Používání zdrojů přiřazení role:

- [Předdefinované roli přiřadit pole Skupina zdroje](https://azure.microsoft.com/documentation/templates/101-rbac-builtinrole-resourcegroup)
- [Předdefinované roli přiřadit existující OM](https://azure.microsoft.com/documentation/templates/101-rbac-builtinrole-virtualmachine)
- [Předdefinované roli přiřadit více existující VMs](https://azure.microsoft.com/documentation/templates/201-rbac-builtinrole-multipleVMs)

## <a name="next-steps"></a>Další kroky

- Informace o struktuře šablony najdete v tématu [vytváření správce prostředků Azure šablony](resource-group-authoring-templates.md).
- Další informace o řízení přístupu na základě rolí v tématu [Řízení přístupu na základě rolí Azure Active Directory](./active-directory/role-based-access-control-configure.md).
