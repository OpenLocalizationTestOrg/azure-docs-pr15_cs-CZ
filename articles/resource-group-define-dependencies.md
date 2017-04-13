<properties
   pageTitle="Závislosti v správce prostředků šablony | Microsoft Azure"
   description="Popisuje, jak nastavit jeden zdroj jako závisí na jiný zdroj během nasazení zajistit, že zdroje jsou nasazeny ve správném pořadí."
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
   ms.date="09/12/2016"
   ms.author="tomfitz"/>

# <a name="defining-dependencies-in-azure-resource-manager-templates"></a>Definování závislosti v správce prostředků Azure šablony

Pro daný zdroj může být další prostředky, které musí existovat před nasazením zdroje. SQL server, například musí existovat před pokusem o nasazení databázi SQL. Definování této relaci označením jeden zdroj jako závislé na jiných zdrojů. Obvykle definujete závislost elementem **dependsOn** , ale můžete ho můžete definovat pomocí funkce **odkaz** . 

Správce prostředků vyhodnocena jako závislostí mezi zdroje a nasadí závislá pořadí. Když prostředků nezávisí na sobě, správce prostředků je nasadí souběžně.

## <a name="dependson"></a>dependsOn

V rámci vaší šablony dependsOn element umožňuje definovat jeden zdroj jako závisí na jeden nebo více zdrojů. Jeho hodnota je seznam oddělený čárkami názvy zdrojů. 

Následující příklad ukazuje sadu měřítko virtuálního počítače, která závisí na Vyrovnávání zatížení, virtuální sítě a smyčku vytvoří více účtů úložiště. Tyto materiály nejsou zobrazeny v následujícím příkladu, ale budou muset existují jinde v šabloně.

    {
      "type": "Microsoft.Compute/virtualMachineScaleSets",
      "name": "[variables('namingInfix')]",
      "location": "[variables('location')]",
      "apiVersion": "2016-03-30",
      "tags": {
        "displayName": "VMScaleSet"
      },
      "dependsOn": [
        "storageLoop",
        "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
      ],
      ...
    }

Pokud chcete definovat závislost mezi zdroji a zdroje informací, které jsou vytvořené pomocí smyčku kopírovat, nastavte dependsOn element název opakovat. Příklad najdete v článku [vytvoření několika instancích zdrojů v Azure správce](resource-group-create-multiple.md).

Když jste sklon nesmí být používat dependsOn namapujte vztahy mezi zdrojů, je důležité pochopit, proč děláte ho vzhledem k tomu může ovlivnit výkon nasazení. Například do dokumentu, jak propojených zdrojů, dependsOn není správný přístup. Nelze zadat dotaz prostředků, které byly definované v elementu dependsOn po nasazení. Pomocí dependsOn se mohou mít vliv na dobu nasazení protože správce prostředků není nasadit v paralelní dva zdroje, které mají závislost. Do dokumentu vztahy mezi zdroji, místo toho použít [propojení zdroje](resource-group-link-resources.md).

## <a name="child-resources"></a>Podřízené zdroje

Vlastnost materiály vám umožní určit podřízené prostředky, které se vztahují k zdroje definovaného. Podřízené zdrojů lze pouze definovaný pěti úrovní vnoření. Je důležité mít na paměti, že není vytvořena implicitním závislost mezi prostředků nadřazené a podřízené zdroje. V případě potřeby zdroje podřízené nasazeny po zdroje nadřazené musí výslovně neuvedete, které závislost vlastnost dependsOn. 

Jednotlivé zdroje nadřazené přijímá jenom určité typy zdrojů jako podřízené zdroje. Typy přípustném prostředků jsou uvedeny v [šabloně schématu](https://github.com/Azure/azure-resource-manager-schemas) nadřazené zdroje. Název podřízené pole Typ zdroje obsahuje název nadřazeného typu zdroje, jako jsou oba zdroje podřízené **Microsoft.Web/sites** **Microsoft.Web/sites/config** a **Microsoft.Web/sites/extensions** .

Následující příklad ukazuje SQL server a databázi SQL. Přestože je databáze serveru nadpis na podřízené úrovni, Všimněte si, že je definován explicitní závislost mezi databáze a SQL serveru SQL server.

    "resources": [
      {
        "name": "[variables('sqlserverName')]",
        "type": "Microsoft.Sql/servers",
        "location": "[resourceGroup().location]",
        "tags": {
          "displayName": "SqlServer"
        },
        "apiVersion": "2014-04-01-preview",
        "properties": {
          "administratorLogin": "[parameters('administratorLogin')]",
          "administratorLoginPassword": "[parameters('administratorLoginPassword')]"
        },
        "resources": [
          {
            "name": "[parameters('databaseName')]",
            "type": "databases",
            "location": "[resourceGroup().location]",
            "tags": {
              "displayName": "Database"
            },
            "apiVersion": "2014-04-01-preview",
            "dependsOn": [
              "[variables('sqlserverName')]"
            ],
            "properties": {
              "edition": "[parameters('edition')]",
              "collation": "[parameters('collation')]",
              "maxSizeBytes": "[parameters('maxSizeBytes')]",
              "requestedServiceObjectiveName": "[parameters('requestedServiceObjectiveName')]"
            }
          }
        ]
      }
    ]


## <a name="reference-function"></a>odkaz (funkce)

[Funkce odkaz](resource-group-template-functions.md#reference) umožňuje výraz odvodit jeho hodnotu z jiný název JSON a dvojice nebo runtime zdrojů. Odkaz výrazů implicitně deklarovat, že jeden zdroj závisí na jiný. 

    reference('resourceName').propertyPath

: Tento element nebo dependsOn element můžete zadávat závislosti, ale nemusíte používat pro stejný zdroj závislá. Kdykoli je to možné, umožňuje implicitní odkaz nedávejte omylem nepotřebných závislost.

Další informace najdete v tématu [funkce odkaz](resource-group-template-functions.md#reference).

## <a name="next-steps"></a>Další kroky

- Další informace o vytváření správce prostředků Azure šablon najdete v tématu [vytváření šablony](resource-group-authoring-templates.md). 
- Seznam dostupných funkcí v šabloně najdete v tématu [funkce šablony](resource-group-template-functions.md).

