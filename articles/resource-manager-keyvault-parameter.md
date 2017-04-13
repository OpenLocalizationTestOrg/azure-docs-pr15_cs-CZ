<properties
   pageTitle="Tajná klíč trezoru šablonou správce prostředků | Microsoft Azure"
   description="Ukazuje, jak předat tajná klíčové trezoru jako parametr během nasazení."
   services="azure-resource-manager,key-vault"
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
   ms.date="06/23/2016"
   ms.author="tomfitz"/>

# <a name="pass-secure-values-during-deployment"></a>Předání zabezpečené hodnoty při zavedení

Když budete potřebovat k předání zabezpečené hodnoty (například hesla) jako parametr během nasazení, můžete uložit hodnotu jako tajná v [Azure klíč trezoru](./key-vault/key-vault-whatis.md) a odkazovat hodnotu v jiných šablon správce prostředků. Zahrnuté jen odkaz tajná šablony tajná nikdy vystaven, takže není potřeba ručně zadat hodnotu pro tajná pokaždé, když nasadíte zdroje. Můžete určit, kteří uživatelé a objektů služby přístup k tajná.  

## <a name="deploy-a-key-vault-and-secret"></a>Nasazení klíčové trezoru a tajná

Pokud chcete vytvořit klíčové trezoru odkazující z jiných šablon správce prostředků, musí nastavíte **enabledForTemplateDeployment** vlastnost na **hodnotu true**a musí udělit přístup uživatelů nebo jistinu služby, který spustí nasazení, který odkazuje tajná.

Další informace o nasazení klíčové trezoru a tajná, najdete v článku [klíč trezoru schématu](resource-manager-template-keyvault.md) a [klíč trezoru tajné schéma](resource-manager-template-keyvault-secret.md).

## <a name="reference-a-secret-with-static-id"></a>Vytvořte odkaz tajná s statická id

Odkaz tajná z parametry souboru, který splňuje hodnoty do šablony aplikace. Odkaz tajná předáním zdroje identifikátor klíčové trezoru a název tajná. V tomto příkladu tajná klíčové trezoru, musí existovat a používáte statická hodnota je pole číslo id zdroje.

    "parameters": {
      "adminPassword": {
        "reference": {
          "keyVault": {
            "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
          }, 
          "secretName": "sqlAdminPassword" 
        } 
      }
    }

Soubor celý parametr mohla vypadat takto:

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "sqlsvrAdminLogin": {
          "value": ""
        },
        "sqlsvrAdminLoginPassword": {
          "reference": {
            "keyVault": {
              "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
            },
            "secretName": "adminPassword"
          }
        }
      }
    }

Parametr, který přijme tajná by měl být **securestring**. Následující příklad ukazuje příslušných částí šablonu, která nasadí SQL serveru, který vyžaduje heslo správce.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "sqlsvrAdminLogin": {
                "type": "string",
                "minLength": 4
            },
            "sqlsvrAdminLoginPassword": {
                "type": "securestring"
            },
            ...
        },
        "resources": [
        {
            "name": "[variables('sqlsvrName')]",
            "type": "Microsoft.Sql/servers",
            "location": "[resourceGroup().location]",
            "apiVersion": "2014-04-01-preview",
            "properties": {
                "administratorLogin": "[parameters('sqlsvrAdminLogin')]",
                "administratorLoginPassword": "[parameters('sqlsvrAdminLoginPassword')]"
            },
            ...
        }],
        "variables": {
            "sqlsvrName": "[concat('sqlsvr', uniqueString(resourceGroup().id))]"
        },
        "outputs": { }
    }

## <a name="reference-a-secret-with-dynamic-id"></a>Vytvořte odkaz tajná s dynamické id

V předchozí části ukázal, jak předat statické zdroje id pro tajná klíčové trezoru. Však v některých případech se budete muset odkaz se liší podle aktuální nasazení klíčové trezoru tajná. V takovém případě nejde pevného kódu číslo id zdroje v souboru parametrů. Bohužel nelze generovat dynamicky číslo id zdroje v souboru parametrů protože šablony výrazů nejsou povoleny v souboru parametrů.

Dynamicky generovat číslo id zdroje pro klíčové trezoru tajná, je nutné přesunout prostředek, který potřebuje tajná do vnořené šablony. V šabloně předlohy přidat vnořené šablonu a přidat parametr, který obsahuje id dynamicky generované zdroje.

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "vaultName": {
          "type": "string"
        },
        "secretName": {
          "type": "string"
        }
      },
      "resources": [
        {
          "apiVersion": "2015-01-01",
          "name": "nestedTemplate",
          "type": "Microsoft.Resources/deployments",
          "properties": {
            "mode": "incremental",
            "templateLink": {
              "uri": "https://www.contoso.com/AzureTemplates/newVM.json",
              "contentVersion": "1.0.0.0"
            },
            "parameters": {
              "adminPassword": {
                "reference": {
                  "keyVault": {
                    "id": "[concat(resourceGroup().id, '/providers/Microsoft.KeyVault/vaults/', parameters('vaultName'))]"
                  },
                  "secretName": "[parameters('secretName')]"
                }
              }
            }
          }
        }
      ],
      "outputs": {}
    }


## <a name="next-steps"></a>Další kroky

- Obecné informace o klíčových trezorů najdete v článku [Začínáme s Azure klíč trezoru](./key-vault/key-vault-get-started.md).
- Informace o používání klíčových trezoru s virtuálního počítače najdete v tématu [otázky bezpečnosti pro správce prostředků Azure](best-practices-resource-manager-security.md).
- Dokončení příklady odkazování na klíčové tajemství najdete v článku [Příklady klíč trezoru](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).

