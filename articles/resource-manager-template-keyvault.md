<properties
   pageTitle="Správce prostředků šablony pro klíčové trezoru | Microsoft Azure"
   description="Zobrazuje schématu správce prostředků pro nasazení klíčové trezorů pomocí šablony."
   services="azure-resource-manager,key-vault"
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
   ms.date="06/23/2016"
   ms.author="tomfitz"/>

# <a name="key-vault-template-schema"></a>Schéma klíčové trezoru šablony

Vytvoří klíčové trezoru.

## <a name="schema-format"></a>Formát schématu

Vytvoření klíčových trezoru, přidejte následující schéma oddílu zdroje šablony.

    {
        "type": "Microsoft.KeyVault/vaults",
        "apiVersion": "2015-06-01",
        "name": string,
        "location": string,
        "properties": {
            "enabledForDeployment": bool,
            "enabledForTemplateDeployment": bool,
            "enabledForVolumeEncryption": bool,
            "tenantId": string,
            "accessPolicies": [
                {
                    "tenantId": string,
                    "objectId": string,
                    "permissions": {
                        "keys": [ keys permissions ],
                        "secrets": [ secrets permissions ]
                    }
                }
            ],
            "sku": {
                "name": enum,
                "family": "A"
            }
        },
        "resources": [
             child resources
        ]
    }

## <a name="values"></a>Hodnoty

Následující tabulka popisuje hodnoty, které je potřeba nastavit ve schématu.

| Jméno | Hodnota |
| ---- | ---- | 
| Typ | Výčet<br />Povinné<br />**Microsoft.KeyVault/vaults**<br /><br />Typ zdroje k vytvoření. |
| apiVersion | Výčet<br />Povinné<br />**2015 06 01** nebo **2014 12 19 náhledu**<br /><br />Verze rozhraní API pro použití při vytváření zdroje. | 
| Jméno | Řetězec<br />Povinné<br />Název, který je v celé Azure jedinečná.<br /><br />Název klíčové trezoru vytvořit. Zvažte použití funkce [uniqueString](resource-group-template-functions.md#uniquestring) s stejný systém názvů vytvořit jedinečný název, jak je vidět v následujícím příkladu. |
| umístění | Řetězec<br />Povinné<br />Platné oblast klíčové trezorů. K určení platné oblastí, najdete v článku [podporované oblastí](resource-manager-supported-services.md#supported-regions).<br /><br />Oblast hostovat klíčové trezoru. |
| Vlastnosti | Objekt<br />Povinné<br />[vlastnosti objektu](#properties)<br /><br />Objekt, který určuje typ klíčové trezoru vytvořit. |
| zdroje informací | Pole<br />Volitelné<br />Povoleny hodnoty: [klíč trezoru tajné zdroje](resource-manager-template-keyvault-secret.md)<br /><br />Zdroje podřízené klíčové trezoru. |

<a id="properties" />
### <a name="properties-object"></a>vlastnosti objektu

| Jméno | Hodnota |
| ---- | ---- | 
| enabledForDeployment | Logická hodnota<br />Volitelné<br />**true** nebo **false**<br /><br />Určuje, zda je povoleno trezoru virtuálního počítače nebo služby struktury nasazení. |
| enabledForTemplateDeployment | Logická hodnota<br />Volitelné<br />**true** nebo **false**<br /><br />Určuje, zda je povoleno trezoru pro použití v nasazení šablony správce prostředků. Další informace najdete v tématu [předat zabezpečené hodnoty při zavedení](resource-manager-keyvault-parameter.md) |
| enabledForVolumeEncryption | Logická hodnota<br />Volitelné<br />**true** nebo **false**<br /><br />Určuje, zda je povoleno trezoru hlasitost šifrování. |
| tenantId | Řetězec<br />Povinné<br />**Globálně jedinečný identifikátor**<br /><br />Identifikátor klienta pro předplatné. Můžete ho vyvolat pomocí rutiny Powershellu [Get-AzureRmSubscription](https://msdn.microsoft.com/library/azure/mt619284.aspx) nebo **Zobrazit účet azure** příkaz Azure rozhraní příkazového řádku. |
| accessPolicies | Pole<br />Povinné<br />[accessPolicies objektu](#accesspolicies)<br /><br />Pole až 16 objektů, které určují oprávnění pro uživatele nebo jistinu služby. |
| SKU | Objekt<br />Povinné<br />[objekt SKU](#sku)<br /><br />SKU pro klíčové trezoru. |

<a id="accesspolicies" />
### <a name="propertiesaccesspolicies-object"></a>properties.accessPolicies objektu

| Jméno | Hodnota |
| ---- | ---- | 
| tenantId | Řetězec<br />Povinné<br />**Globálně jedinečný identifikátor**<br /><br />Identifikátor klienta obsahující **objektu** v této zásady přístupu klienta služby Azure Active Directory |
| Objektu | Řetězec<br />Povinné<br />**Globálně jedinečný identifikátor**<br /><br />Identifikátor objektu Azure Active Directory uživatele nebo jistinu služba, která bude mít přístup k trezoru. Získání hodnoty [Get-AzureRmADUser](https://msdn.microsoft.com/library/azure/mt679001.aspx) nebo rutinách Powershellu [Get-AzureRmADServicePrincipal](https://msdn.microsoft.com/library/azure/mt678992.aspx) nebo příkazy rozhraní příkazového řádku Azure **azure ad uživatele** nebo **azure ad sp** . |
| oprávnění | Objekt<br />Povinné<br />[oprávnění objektů](#permissions)<br /><br />Oprávnění udělená trezoru objekt služby Active Directory. |

<a id="permissions" />
### <a name="propertiesaccesspoliciespermissions-object"></a>properties.accessPolicies.permissions objektu

| Jméno | Hodnota |
| ---- | ---- | 
| klíče | Pole<br />Povinné<br />**všechny**, **záložní**, **vytvořit**, **dešifrování**, **Odstranit**, **šifrování**, **získat**, **import**, **seznamu**, **Obnovit**, **Odhlásit**, **unwrapkey**, **Aktualizovat**, **Ověřte**, **wrapkey**<br /><br />Oprávnění udělena klíčů v trezoru tento objekt služby Active Directory. Tato hodnota musí být zadán jako matice z jedné nebo několika hodnotám povoleno. |
| tajemství | Pole<br />Povinné<br />**všechny**, **Odstranění**, **zjištění** **seznam**, **Nastavení**<br /><br />Oprávnění udělená tajemství v trezoru tento objekt služby Active Directory. Tato hodnota musí být zadán jako matice z jedné nebo několika hodnotám povoleno. |

<a id="sku" />
### <a name="propertiessku-object"></a>Properties.SKU objektu

| Jméno | Hodnota |
| ---- | ---- | 
| Jméno | Výčet<br />Povinné<br />**Standardní**nebo **premium** <br /><br />Vrstvy služeb KeyVault používat.  Standardní podporuje tajemství a software chráněn klíče.  Premium přidá podporu pro chráněné heslem HSM klíče. |
| řady | Výčet<br />Povinné<br />**A** <br /><br />Řady sku používat. |
 
    
## <a name="examples"></a>Příklady

Následující příklad nasadí klíčové trezoru a tajná.

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

## <a name="quickstart-templates"></a>Rychlý úvod šablony

Následující šablonu rychlý úvod nasadí klíčové trezoru.

- [Vytvoření klíčových trezoru](https://azure.microsoft.com/documentation/templates/101-key-vault-create/)


## <a name="next-steps"></a>Další kroky

- Obecné informace o klíčových trezorů najdete v článku [Začínáme s Azure klíč trezoru](./key-vault/key-vault-get-started.md).
- Příklad odkazování na klíčové trezoru tajná při nasazování šablon najdete v článku [předání zabezpečené hodnot během nasazení](resource-manager-keyvault-parameter.md).

