<properties
   pageTitle="Správce prostředků šablony pro tajná klíčové trezoru | Microsoft Azure"
   description="Zobrazuje schématu správce prostředků pro nasazení klíčové trezoru tajemství pomocí šablony."
   services="azure-resource-manager,key-vault"
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
   ms.date="06/23/2016"
   ms.author="tomfitz"/>

# <a name="key-vault-secret-template-schema"></a>Schéma klíčové trezoru tajné šablony

Vytvoří tajná uloženými v klíčové trezoru. Tento typ zdroje je často používaný jako prostředek podřízené [klíčové trezoru](resource-manager-template-keyvault.md).

## <a name="schema-format"></a>Formát schématu

Vytvoření klíčových trezoru tajná do šablony přidáte následující schéma. Tajná je to možné definovat jako prostředek podřízené z klíčové trezoru nebo nejvyšší úrovně zdroje. Při lze definovat ho jako prostředek podřízené klíčové trezoru nasazenou v stejnou šablonu. Je třeba definovat tajná jako prostředek nejvyšší úrovně, pokud klíčové trezoru není nasazenou v stejnou šablonu nebo pokud je potřeba vytvořit více tajemství ve smyčce na typ zdroje. 

    {
        "type": enum,
        "apiVersion": "2015-06-01",
        "name": string,
        "properties": {
            "value": string
        },
        "dependsOn": [ array values ]
    }

## <a name="values"></a>Hodnoty

Následující tabulka popisuje hodnoty, které je potřeba nastavit ve schématu.

| Jméno | Hodnota |
| ---- | ---- | 
| Typ | Výčet<br />Povinné<br />**tajemství** (nasazené jako prostředek podřízené klíčové trezoru) nebo<br /> **Microsoft.KeyVault/vaults/secrets** (nasazené nejvyšší úrovně zdroje)<br /><br />Typ zdroje k vytvoření. |
| apiVersion | Výčet<br />Povinné<br />**2015 06 01** nebo **2014 12 19 náhledu**<br /><br />Verze rozhraní API pro použití při vytváření zdroje. | 
| Jméno | Řetězec<br />Povinné<br />Pro jedno slovo nasazené jako prostředek podřízené klíčové trezoru nebo ve formátu **{název klíče trezoru} / {tajná název}** nasazené nejvyšší úrovně zdroje, které budou přidány do existující klíčové trezoru.<br /><br />Název tajná vytvořit. |
| Vlastnosti | Objekt<br />Povinné<br />[vlastnosti objektu](#properties)<br /><br />Objekt, který určuje hodnotu tajná vytvořit. |
| dependsOn | Pole<br />Volitelné<br />Seznam oddělený čárkami zdroje názvy nebo identifikátory jedinečné zdroje.<br /><br />Kolekce prostředky, které závisí na tom tento odkaz. Klíčové trezoru tajná po nasazení ve stejné šabloně, uveďte název klíčové trezoru v tomto elementu zajistit, že nejdřív nasazení. |

<a id="properties" />
### <a name="properties-object"></a>vlastnosti objektu

| Jméno | Hodnota |
| ---- | ---- | 
| Hodnota | Řetězec<br />Povinné<br /><br />Řeknu hodnota pro uložení klíčové trezoru. Při předávání v hodnotě pro tuto vlastnost parametr typ **securestring**.  |

    
## <a name="examples"></a>Příklady

První příklad nasadí tajná jako prostředek podřízené klíčové trezoru.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "keyVaultName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the vault"
                }
            },
            "tenantId": {
                "type": "string",
                "metadata": {
                   "description": "Tenant ID for the subscription and use assigned access to the vault. Available from the Get-AzureRmSubscription PowerShell cmdlet"
                }
            },
            "objectId": {
                "type": "string",
                "metadata": {
                    "description": "Object ID of the AAD user or service principal that will have access to the vault. Available from the Get-AzureRmADUser or the Get-AzureRmADServicePrincipal cmdlets"
                }
            },
            "keysPermissions": {
                "type": "array",
                "defaultValue": [ "all" ],
                "metadata": {
                    "description": "Permissions to grant user to keys in the vault. Valid values are: all, create, import, update, get, list, delete, backup, restore, encrypt, decrypt, wrapkey, unwrapkey, sign, and verify."
                }
            },
            "secretsPermissions": {
                "type": "array",
                "defaultValue": [ "all" ],
                "metadata": {
                    "description": "Permissions to grant user to secrets in the vault. Valid values are: all, get, set, list, and delete."
                }
            },
            "vaultSku": {
                "type": "string",
                "defaultValue": "Standard",
                "allowedValues": [
                    "Standard",
                    "Premium"
                ],
                "metadata": {
                    "description": "SKU for the vault"
                }
            },
            "enabledForDeployment": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for VM or Service Fabric deployment"
                }
            },
            "enabledForTemplateDeployment": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for ARM template deployment"
                }
            },
            "enableVaultForVolumeEncryption": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for volume encryption"
                }
            },
            "secretName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the secret to store in the vault"
                }
            },
            "secretValue": {
                "type": "securestring",
                "metadata": {
                    "description": "Value of the secret to store in the vault"
                }
            }
        },
        "resources": [
        {
            "type": "Microsoft.KeyVault/vaults",
            "name": "[parameters('keyVaultName')]",
            "apiVersion": "2015-06-01",
            "location": "[resourceGroup().location]",
            "tags": {
                "displayName": "KeyVault"
            },
            "properties": {
                "enabledForDeployment": "[parameters('enabledForDeployment')]",
                "enabledForTemplateDeployment": "[parameters('enabledForTemplateDeployment')]",
                "enabledForVolumeEncryption": "[parameters('enableVaultForVolumeEncryption')]",
                "tenantId": "[parameters('tenantId')]",
                "accessPolicies": [
                {
                    "tenantId": "[parameters('tenantId')]",
                    "objectId": "[parameters('objectId')]",
                    "permissions": {
                        "keys": "[parameters('keysPermissions')]",
                        "secrets": "[parameters('secretsPermissions')]"
                    }
                }],
                "sku": {
                    "name": "[parameters('vaultSku')]",
                    "family": "A"
                }
            },
            "resources": [
            {
                "type": "secrets",
                "name": "[parameters('secretName')]",
                "apiVersion": "2015-06-01",
                "properties": {
                    "value": "[parameters('secretValue')]"
                },
                "dependsOn": [
                    "[concat('Microsoft.KeyVault/vaults/', parameters('keyVaultName'))]"
                ]
            }]
        }]
    }

Druhý příklad nasadí tajná jako nejvyšší úrovně prostředek, který je uložený v existující klíčové trezoru.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "keyVaultName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the existing vault"
                }
            },
            "secretName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the secret to store in the vault"
                }
            },
            "secretValue": {
                "type": "securestring",
                "metadata": {
                    "description": "Value of the secret to store in the vault"
                }
            }
        },
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.KeyVault/vaults/secrets",
                "apiVersion": "2015-06-01",
                "name": "[concat(parameters('keyVaultName'), '/', parameters('secretName'))]",
                "properties": {
                    "value": "[parameters('secretValue')]"
                }
            }
        ],
        "outputs": {}
    }


## <a name="next-steps"></a>Další kroky

- Obecné informace o klíčových trezorů najdete v článku [Začínáme s Azure klíč trezoru](./key-vault/key-vault-get-started.md).
- Příklad odkazování na klíčové trezoru tajná při nasazování šablon najdete v článku [předání zabezpečené hodnot během nasazení](resource-manager-keyvault-parameter.md).


