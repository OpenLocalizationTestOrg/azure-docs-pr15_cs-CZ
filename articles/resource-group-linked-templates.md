<properties
   pageTitle="Propojené šablony pomocí Správce prostředků | Microsoft Azure"
   description="Popisuje, jak používat propojené šablony v šabloně aplikace Správce prostředků Azure k vytváření řešení moduly šablony. Ukazuje, jak můžete předávat hodnoty parametrů, zadat souboru parametr a dynamicky vytvořený adresy URL."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/02/2016"
   ms.author="tomfitz"/>

# <a name="using-linked-templates-with-azure-resource-manager"></a>Správce propojené šablony s Azure zdroje

Z v rámci jedné správce prostředků Azure šablony můžete propojit jinou šablonu, která umožňuje rozložit nasazení do sady určené, šablony pro konkrétní účel. Stejně jako u decomposing aplikace do několika třídy kódu, rozložené výhody z hlediska testování opakované použití a čitelnosti.  

Lze předat parametry ze šablony hlavní propojené šablony a tyto parametry můžete přímo zmapovat parametry nebo proměnné zveřejněné příslušným volání šablony. Propojené šablony můžete také předat výstupní proměnná šablonu zdroj povolení exchange mezi dvěma stranami dat mezi šablonami.

## <a name="linking-to-a-template"></a>Propojení do šablony

Vytvořit propojení mezi dvěma šablony přidáním nasazení prostředků v šabloně hlavní odkazující na šabloně propojené. Nastavte vlastnost **templateLink** na URI propojené šablony. Je můžete zadat hodnoty parametrů propojené šablony zadáním hodnoty přímo do vaší šablony nebo propojení se souborem parametr. Vlastnost **Parametry** Chcete-li zadat hodnotu parametru přímo v následujícím příkladu.

    "resources": [ 
      { 
         "apiVersion": "2015-01-01", 
         "name": "linkedTemplate", 
         "type": "Microsoft.Resources/deployments", 
         "properties": { 
           "mode": "incremental", 
           "templateLink": {
              "uri": "https://www.contoso.com/AzureTemplates/newStorageAccount.json",
              "contentVersion": "1.0.0.0"
           }, 
           "parameters": { 
              "StorageAccountName":{"value": "[parameters('StorageAccountName')]"} 
           } 
         } 
      } 
    ] 

Služba Správce prostředků musíte mít přístup k šabloně propojené. Nelze zadat místní soubor nebo soubor, který je dostupný pouze na místní síti propojené šablony. Můžete jenom poskytnout URI hodnotu, která obsahuje **http** nebo **https**. Jednou z možností je umístění propojeného šablony v účtu úložiště a použijte identifikátor URI pro danou položku tak jak je vidět v následujícím příkladu.

    "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0",
    }

I když propojené šablony musí být externě k dispozici, nemusí být přístupné veřejnosti. Šablony můžete přidat k soukromé úložiště účtu, který bude přístupný pro jenom vlastník účtu úložiště. Pak vytvoříte sdílené přístupový token podpisu (přidružení zabezpečení) k povolení přístupu ke během nasazení. Identifikátor URI propojené šablony přidáte token přidružení zabezpečení. Postup pro nastavení šablony v účtu úložiště a generování token přidružení zabezpečení najdete v tématech [nasazení zdroje se šablonami správce prostředků a Azure PowerShell](resource-group-template-deploy.md) nebo [nasadit zdroje se šablonami správce prostředků a Azure rozhraní příkazového řádku](resource-group-template-deploy-cli.md). 

Následující příklad ukazuje šablonu nadřazené propojující jinou šablonu. Propojené šablony pracuje s tokem přidružení zabezpečení, které je v jako parametr.

    "parameters": {
        "sasToken": { "type": "securestring" }
    },
    "resources": [
        {
            "apiVersion": "2015-01-01",
            "name": "linkedTemplate",
            "type": "Microsoft.Resources/deployments",
            "properties": {
              "mode": "incremental",
              "templateLink": {
                "uri": "[concat('https://storagecontosotemplates.blob.core.windows.net/templates/helloworld.json', parameters('sasToken'))]",
                "contentVersion": "1.0.0.0"
              }
            }
        }
    ],

I když je předaná tokenu jako bezpečné řetězec, URI propojené šabloně, včetně tokenu přidružení zabezpečení přihlášení operace nasazení pro danou skupinu prostředků. Pokud chcete omezit zobrazení, nastavte vypršení platnosti pro token.

## <a name="linking-to-a-parameter-file"></a>Propojení se souborem parametrů

V dalším příkladu vlastnost **parametersLink** propojit se souborem parametr.

    "resources": [ 
      { 
         "apiVersion": "2015-01-01", 
         "name": "linkedTemplate", 
         "type": "Microsoft.Resources/deployments", 
         "properties": { 
           "mode": "incremental", 
           "templateLink": {
              "uri":"https://www.contoso.com/AzureTemplates/newStorageAccount.json",
              "contentVersion":"1.0.0.0"
           }, 
           "parametersLink": { 
              "uri":"https://www.contoso.com/AzureTemplates/parameters.json",
              "contentVersion":"1.0.0.0"
           } 
         } 
      } 
    ] 

Hodnotu URI souboru propojené parametru nesmí být místní soubor a musí obsahovat **http** nebo **https**. Parametr soubor lze také omezený přístup pomocí token přidružení zabezpečení.

## <a name="using-variables-to-link-templates"></a>Pokud chcete vytvořit odkaz šablony pomocí proměnných

V předchozích příkladech ukázal pevně hodnot URL šablony odkazy. Tento přístup ve vašem mohlo fungovat pro šablonu jednoduché, ale nefunguje dobře při práci s velké sadu šablon pro moduly. Místo toho můžete vytvořit statická proměnná obsahujícího základní adresu URL pro hlavní šablony a dynamicky vytvořte adresy URL pro propojené šablony z této základní adresu URL. Výhodou tento přístup je mohli snadno přesouvat a rozvětvené šabloně vzhledem k tomu potřebujete změnit statická proměnná v šabloně hlavní. Hlavní šablona předává správné URI v celém rozložený šablony.

Následující příklad ukazuje, jak používat základní adresu URL k vytvoření dvou adresy URL pro propojené šablony (**sharedTemplateUrl** a **vmTemplate**). 

    "variables": {
        "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
        "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
        "tshirtSizeSmall": {
            "vmSize": "Standard_A1",
            "diskSize": 1023,
            "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
            "vmCount": 2,
            "slaveCount": 1,
            "storage": {
                "name": "[parameters('storageAccountNamePrefix')]",
                "count": 1,
                "pool": "db",
                "map": [0,0],
                "jumpbox": 0
            }
        }
    }

Můžete taky použít [deployment()](resource-group-template-functions.md#deployment) zobrazíte základní adresu URL pro aktuální šablony a použít k získání adresy URL pro jiných šablon ve stejném umístění. Tento přístup je užitečné, pokud vaše šablona změny umístění (třeba kvůli versioning) nebo chcete zabránit pevně kódování adresy URL v souboru šablony. 

    "variables": {
        "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"
    }

## <a name="conditionally-linking-to-templates"></a>Podmíněně propojení na šablony

Můžete propojit různé šablony předáním hodnoty parametru, který slouží k vytvoření URI propojené šablony. Tento postup funguje situacích, kdy je třeba zadat při nasazení, který propojené šablona používat. Můžete třeba určit jedné šablony můžete u existujícího účtu úložiště a jinou šablonu použít k vytvoření nového účtu úložiště.

Následující příklad ukazuje parametr pro název účtu úložiště a parametr můžete určit, zda je účet úložiště nové nebo existující.

    "parameters": {
        "storageAccountName": {
            "type": "String"
        },
        "newOrExisting": {
            "type": "String",
            "allowedValues": [
                "new",
                "existing"
            ]
        }
    },

Proměnné šablony identifikátor URI, který obsahuje hodnotu parametru nové nebo existující vytvoříte.

    "variables": {
        "templatelink": "[concat('https://raw.githubusercontent.com/exampleuser/templates/master/',parameters('newOrExisting'),'StorageAccount.json')]"
    },

Zadejte tuto hodnotu proměnné zdroje nasazení.

    "resources": [
        {
            "apiVersion": "2015-01-01",
            "name": "linkedTemplate",
            "type": "Microsoft.Resources/deployments",
            "properties": {
                "mode": "incremental",
                "templateLink": {
                    "uri": "[variables('templatelink')]",
                    "contentVersion": "1.0.0.0"
                },
                "parameters": {
                    "StorageAccountName": {
                        "value": "[parameters('storageAccountName')]"
                    }
                }
            }
        }
    ],

Identifikátor URI vyřeší do šablony s názvem **existingStorageAccount.json** nebo **newStorageAccount.json**. Vytvoření šablon pro tyto URI.

Následující příklad ukazuje **existingStorageAccount.json** šablonu.

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "storageAccountName": {
          "type": "String"
        }
      },
      "variables": {},
      "resources": [],
      "outputs": {
        "storageAccountInfo": {
          "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')),providers('Microsoft.Storage', 'storageAccounts').apiVersions[0])]",
          "type" : "object"
        }
      }
    }

Následující příklad ukazuje **newStorageAccount.json** šablony. Všimněte si, že jako existující úložiště je šablona účtu objekt účtu úložiště vrácena v výstupy. Hlavní šablony spolupracuje buď propojené šablony.

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "storageAccountName": {
          "type": "string"
        }
      },
      "resources": [
        {
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[parameters('StorageAccountName')]",
          "apiVersion": "2016-01-01",
          "location": "[resourceGroup().location]",
          "sku": {
            "name": "Standard_LRS"
          },
          "kind": "Storage",
          "properties": {
          }
        }
      ],
      "outputs": {
        "storageAccountInfo": {
          "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('StorageAccountName')),providers('Microsoft.Storage', 'storageAccounts').apiVersions[0])]",
          "type" : "object"
        }
      }
    }

## <a name="complete-example"></a>Dokončení příkladu

Následující příklad šablony zobrazení zjednodušené uspořádání propojené šablon ke znázornění několik koncepty v tomto článku. Předpokládá, že šablon přidané do kontejneru stejný účet úložiště s veřejným přístupem vypnou a zůstanou vypnuté. Propojené šablony splňuje hodnotu zpátky do hlavního šablony v části **výstup** .

Soubor **parent.json** obsahuje:

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "containerSasToken": { "type": "string" }
      },
      "resources": [
        {
          "apiVersion": "2015-01-01",
          "name": "linkedTemplate",
          "type": "Microsoft.Resources/deployments",
          "properties": {
            "mode": "incremental",
            "templateLink": {
              "uri": "[concat(uri(deployment().properties.templateLink.uri, 'helloworld.json'), parameters('containerSasToken'))]",
              "contentVersion": "1.0.0.0"
            }
          }
        }
      ],
      "outputs": {
        "result": {
          "type": "object",
          "value": "[reference('linkedTemplate').outputs.result]"
        }
      }
    }

Soubor **helloworld.json** obsahuje:

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {},
      "variables": {},
      "resources": [],
      "outputs": {
        "result": {
            "value": "Hello World",
            "type" : "string"
        }
      }
    }
    
V prostředí PowerShell získat token kontejneru a nasazení šablon s:

    Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates
    $token = New-AzureStorageContainerSASToken -Name templates -Permission r -ExpiryTime (Get-Date).AddMinutes(30.0)
    New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri ("https://storagecontosotemplates.blob.core.windows.net/templates/parent.json" + $token) -containerSasToken $token

V Azure rozhraní příkazového řádku získat token kontejneru a nasazení šablon s následující kód. Při použití šablony identifikátor URI, který obsahuje token přidružení zabezpečení v současné době nutné zadat název pro nasazení.  

    expiretime=$(date -I'minutes' --date "+30 minutes")  
    azure storage container sas create --container templates --permissions r --expiry $expiretime --json | jq ".sas" -r
    azure group deployment create -g ExampleGroup --template-uri "https://storagecontosotemplates.blob.core.windows.net/templates/parent.json?{token}" -n tokendeploy  

Zobrazí se výzva k zadání token přidružení zabezpečení jako parametr. Budete muset před token s **?**.

## <a name="next-steps"></a>Další kroky
- Další informace o definování nasazení pořadí svých prostředcích, najdete v článku [definují závislosti v správce prostředků Azure šablony](resource-group-define-dependencies.md)
- Zjistěte, jak definovat jeden zdroj ale vytvářet mnoha instance, najdete v článku [vytvoření několika instancích zdrojů v správce prostředků Azure](resource-group-create-multiple.md)
