<properties
   pageTitle="Nasazení několika instancích zdrojů | Microsoft Azure"
   description="Pomocí kopírování a matic v šabloně aplikace Správce prostředků Azure zapracovávat ji tisknutím při nasazení prostředků."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="wpickett"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/30/2016"
   ms.author="tomfitz"/>

# <a name="create-multiple-instances-of-resources-in-azure-resource-manager"></a>Vytvoření více instancí zdrojů v správce prostředků Azure

V tomto tématu se dozvíte, jak iterace v šabloně správce prostředků Azure k vytvoření několika instancí zdroje.

## <a name="copy-copyindex-and-length"></a>kopírování, copyIndex a délky

V rámci příslušný zdroj k vytvoření tisknutím můžete definovat **Kopírovat** objekt, který určuje počet zapracovávat ji. Vytvoření kopie trvá v tomto formátu:

    "copy": { 
        "name": "websitescopy", 
        "count": "[parameters('count')]" 
    } 

Aktuální hodnota iterace pomocí funkce **copyIndex()** dostanete tak, jak vidíte dole ve funkci propojit.

    [concat('examplecopy-', copyIndex())]

Při vytváření více zdrojů v matici hodnot, můžete zadat počet funkce **Délka** . Matice zadaná jako parametr funkce délka.

    "copy": {
        "name": "websitescopy",
        "count": "[length(parameters('siteNames'))]"
    }

## <a name="use-index-value-in-name"></a>Použijte index hodnotu do pole název

Můžete použít výtisku operace vytvoření opakovaně prostředek jedinečně nazvanou podle rostoucí po krocích index. Můžete například přidat jedinečné číslo na konec každé název zdroje, které je nasazené. Nasazení tři webů s názvem:

- examplecopy 0
- examplecopy-1
- examplecopy-2.

Šablona:

    "parameters": { 
      "count": { 
        "type": "int", 
        "defaultValue": 3 
      } 
    }, 
    "resources": [ 
      { 
          "name": "[concat('examplecopy-', copyIndex())]", 
          "type": "Microsoft.Web/sites", 
          "location": "East US", 
          "apiVersion": "2015-08-01",
          "copy": { 
             "name": "websitescopy", 
             "count": "[parameters('count')]" 
          }, 
          "properties": {
              "serverFarmId": "hostingPlanName"
          }
      } 
    ]

## <a name="offset-index-value"></a>Posun hodnota indexu

Zjistíte v předchozím příkladu, hodnota indexu jdoucí od nuly 2. Pro odsazení hodnotu index, můžete předáte hodnoty ve funkci **copyIndex()** například **copyIndex(1)**. Element kopírovat pořád podle počtu iterací provádět, ale hodnota copyIndex posun podle určité hodnoty. Ano pomocí šablony stejné jako v předchozím příkladu, ale určující **copyIndex(1)** by nasazení tři webů s názvem:

- examplecopy-1
- examplecopy-2
- examplecopy-3

## <a name="use-copy-with-array"></a>Použít kopírování s polem
   
Zkopírování je užitečné při práci s maticemi vzhledem k tomu můžete iteraci každý prvek v poli. Nasazení tři webů s názvem:

- examplecopy Contoso
- examplecopy Fabrikam
- examplecopy Coho

Šablona:

    "parameters": { 
      "org": { 
         "type": "array", 
             "defaultValue": [ 
             "Contoso", 
             "Fabrikam", 
             "Coho" 
          ] 
      }
    }, 
    "resources": [ 
      { 
          "name": "[concat('examplecopy-', parameters('org')[copyIndex()])]", 
          "type": "Microsoft.Web/sites", 
          "location": "East US", 
          "apiVersion": "2015-08-01",
          "copy": { 
             "name": "websitescopy", 
             "count": "[length(parameters('org'))]" 
          }, 
          "properties": {
              "serverFarmId": "hostingPlanName"
          } 
      } 
    ]

Samozřejmě nastavte počet kopií na jinou hodnotu než délku pole. Můžete například vytvořit pole s více hodnotami a potom předejte hodnoty parametru, která určuje, kolik prvků pole pro nasazení. V takovém případě nastavíte počet kopií, jak je vidět v prvním příkladu. 

## <a name="depending-on-resources-in-a-loop"></a>V závislosti na prostředcích ve smyčce

Můžete určit, že zdroje zavést až po jinému zdroji pomocí **dependsOn** element. Pokud potřebujete nasazení zdroj, který závisí na kolekci zdrojů ve smyčce, můžete zadat název smyčka kopie v elementu **dependsOn** . Následující příklad ukazuje, jak nasazení 3 účty úložiště před nasazením virtuální počítač. Úplné definice virtuálního počítače se nezobrazí. Všimněte si, že element kopie má **název** nastavena na **storagecopy** a **storagecopy**také nastavit element **dependsOn** pro virtuálních počítačích.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "resources": [
            {
                "apiVersion": "2015-06-15",
                "type": "Microsoft.Storage/storageAccounts",
                "name": "[concat('storage', uniqueString(resourceGroup().id), copyIndex())]",
                "location": "[resourceGroup().location]",
                "properties": {
                    "accountType": "Standard_LRS"
                 },
                "copy": { 
                    "name": "storagecopy", 
                    "count": 3 
                }
            },
           {
               "apiVersion": "2015-06-15", 
               "type": "Microsoft.Compute/virtualMachines", 
               "name": "[concat('VM', uniqueString(resourceGroup().id))]",  
               "dependsOn": ["storagecopy"],
               ...
           }
        ],
        "outputs": {}
    }

## <a name="looping-on-a-nested-resource"></a>Opakování vnořené zdroje

Kopírovat smyčka nelze použít vnořené zdroje. Pokud je potřeba vytvořit několika instancích zdroje, které definujete obvykle jako vnořené do jiného zdroje, musíte místo toho vytvořit zdroj jako prostředek nejvyšší úrovně a definování vztahu s tímto zdrojem nadřazený **Typ** a **název** vlastnosti.

Předpokládejme například, že definujete obvykle datovou sadu jako vnořené zdroj v rámci výroby Data.

    "parameters": {
        "dataFactoryName": {
            "type": "string"
         },
         "dataSetName": {
            "type": "string"
         }
    },
    "resources": [
    {
        "type": "Microsoft.DataFactory/datafactories",
        "name": "[parameters('dataFactoryName')]",
        ...
        "resources": [
        {
            "type": "datasets",
            "name": "[parameters('dataSetName')]",
            "dependsOn": [
                "[parameters('dataFactoryName')]"
            ],
            ...
        }
    }]
    
Pokud chcete vytvořit víc instancí datové sady, musíte změníte šablonu, jak je ukázáno v následujícím příkladu. Zadejte plně kvalifikovaný oznámení a název obsahuje název factory data.

    "parameters": {
        "dataFactoryName": {
            "type": "string"
         },
         "dataSetName": {
            "type": "array"
         }
    },
    "resources": [
    {
        "type": "Microsoft.DataFactory/datafactories",
        "name": "[parameters('dataFactoryName')]",
        ...
    },
    {
        "type": "Microsoft.DataFactory/datafactories/datasets",
        "name": "[concat(parameters('dataFactoryName'), '/', parameters('dataSetName')[copyIndex()])]",
        "dependsOn": [
            "[parameters('dataFactoryName')]"
        ],
        "copy": { 
            "name": "datasetcopy", 
            "count": "[length(parameters('dataSetName'))]" 
        } 
        ...
    }]

## <a name="create-multiple-instances-when-copy-wont-work"></a>Vytvoření více instancí při kopírování nefunguje

**Kopírovat** můžete použít jenom typy zdrojů, nikoli vlastnosti v rámci typu zdroje. To může způsobit problémy za vás, když budete chtít vytvořit více instancí něco, co je součástí zdroje. Běžné situace je vytvořit více disků dat pro virtuální počítač. **Kopírovat** nelze použít s discích dat, protože **dataDisks** je vlastnosti virtuálního počítače, nikoli vlastní typ zdroje. Místo toho vytvoříte matici s množstvím dat discích jako je třeba a předejte skutečný počet discích dat k vytvoření. V definici virtuálního počítače použijte funkci **trvat** z pole získat počet prvků, které vás skutečně důležité.

Kompletní příklad tento způsob je zobrazit v okně [vytvořit OM s vybranou dynamických dat disků](https://azure.microsoft.com/documentation/templates/201-vm-dynamic-data-disks-selection/) šablonu.

Příslušných částí nasazení šablony jsou uvedeny níže. Hodně šablona byla odebrána ke zvýraznění části týkající se dynamicky vytváření počet disků data. Všimněte si parametr **numDataDisks** , který umožňuje předat číslo disk a vytvořit. 

```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    ...
    "numDataDisks": {
      "type": "int",
      "maxValue": 64,
      "metadata": {
        "description": "This parameter allows you to select the number of disks you want"
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'dynamicdisk')]",
    "sizeOfDataDisksInGB": 100,
    "diskCaching": "ReadWrite",
    "diskArray": [
      {
        "name": "datadisk1",
        "lun": 0,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk1.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk2",
        "lun": 1,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk2.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk3",
        "lun": 2,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk3.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk4",
        "lun": 3,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk4.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      ...
      {
        "name": "datadisk63",
        "lun": 62,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk63.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk64",
        "lun": 63,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk64.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      }
    ]
  },
  "resources": [
    ...
    {
      "type": "Microsoft.Compute/virtualMachines",
      "properties": {
        ...
        "storageProfile": {
          ...
          "dataDisks": "[take(variables('diskArray'),parameters('numDataDisks'))]"
        },
        ...
      }
      ...
    }
  ]
}
```


## <a name="next-steps"></a>Další kroky
- Pokud chcete další informace o v částech šablony, najdete v článku [Vytváření šablon Azure správce prostředků](./resource-group-authoring-templates.md).
- Některé funkce, které můžete použít v šabloně najdete v článku [Funkce šablony Azure správce prostředků](./resource-group-template-functions.md).
- Naučte se nasadit šablony, najdete v článku [nasazení aplikace šablonou Azure správce prostředků](resource-group-template-deploy.md).
