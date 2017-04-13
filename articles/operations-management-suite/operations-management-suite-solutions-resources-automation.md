<properties
   pageTitle="Automatizace zdrojů v řešení OMS | Microsoft Azure"
   description="Řešení v OMS obvykle obsahují runbooks v Azure automatizaci k automatizaci procesů například shromáždění a zpracování monitorování dat.  Tento článek popisuje, jak zahrnout runbooks a související materiály řešení."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/19/2016"
   ms.author="bwren" />

# <a name="automation-resources-in-oms-solutions-preview"></a>Automatizace zdrojů v řešení OMS (verze Preview)

>[AZURE.NOTE]Toto je předběžná verze dokumentace pro vytváření řešení pro správu v OMS, které jsou v současnosti ve verzi preview. Libovolné schéma píše níže se mohou změnit.   

[Řešení pro správu v OMS](operations-management-suite-solutions.md) obvykle obsahují runbooks v Azure automatizaci k automatizaci procesů například shromáždění a zpracování monitorování dat.  Kromě runbooks automatizaci účty obsahuje majetek, jako je proměnných a plánů, které podporují runbooks použité v řešení.  Tento článek popisuje, jak zahrnout runbooks a související materiály řešení.

>[AZURE.NOTE]Příklady v tomto článku použít parametry a proměnných, které jsou potřeba nebo společné řešení pro správu a popsány v [řešení pro správu vytvoření v sadě správy operace (OMS)](operations-management-suite-solutions-creating.md) 


## <a name="prerequisites"></a>Zjistit předpoklady pro
V tomto článku se předpokládá, že jste již vědět, jak [vytvářet řešení pro správu](operations-management-suite-solutions-creating.md) a struktury soubor řešení.

## <a name="samples"></a>Vzorky
Ukázky šablon správce prostředků pro automatizaci zdroje dostali ze [šablony rychlý úvod do GitHub](https://github.com/azureautomation/automation-packs/tree/master/101-sample-automation-resource-templates).

## <a name="automation-account"></a>Automatizace účtu
Všechny zdroje v Azure automatizaci jsou obsaženy v [automatizaci účtu](../automation/automation-security-overview.md#automation-account-overview).  Podle popisu v [pracovním prostoru OMS a automatizaci účtu](operations-management-suite-solutions-creating.md#oms-workspace-and-automation-account) automatizaci účet není součástí řešení pro správu, ale musí existovat před instalací řešení.  Pokud není k dispozici, se nepovede nainstalovat řešení.

Název účtem automatizaci je název každého automatizaci zdroje.  Důvodem je v řešení s parametrem **název účtu** jako v následujícím příkladu postupu runbook zdroje.
    
    "name": "[concat(parameters('accountName'), '/MyRunbook'))]"


## <a name="runbooks"></a>Runbooks
[Automatizace Azure postupu runbook](../automation/automation-runbook-types.md) zdroje mít typ **Microsoft.Automation/automationAccounts/runbooks** a mají vlastnosti v následující tabulce.

| Vlastnost | Popis |
|:--|:--|
| runbookType | Určuje typy postupu runbook. <br><br> Skript - skript Powershellu <br>Prostředí PowerShell - prostředí PowerShell pracovního postupu <br> GraphPowerShell - postupu runbook skript Powershellu grafické <br> GraphPowerShellWorkflow - grafický Powershellu pracovního postupu runbook   |
| logProgress | Určuje, zda má být [Průběh záznamy](../automation/automation-runbook-output-and-messages.md) generováno pro postupu runbook. |
| logVerbose  | Určuje, zda má být [podrobného záznamy](../automation/automation-runbook-output-and-messages.md) generováno pro postupu runbook. |
| Popis | Volitelný popis postupu runbook. |
| publishContentLink | Určuje obsah postupu runbook. <br><br>identifikátor URI - Uri obsahu postupu runbook.  To bude .ps1 souboru pro PowerShell a skript runbooks a soubor exportovaného grafický postupu runbook pro graf postupu runbook.  <br> verze: verze postupu runbook pro vlastní sledování. |

Příkladu postupu runbook zdroje je menší než.  V tomto případě použije postupu runbook z [Galerie Powershellu](https://www.powershellgallery.com).

    "name": "[concat(parameters('accountName'), '/Start-AzureV2VMs'))]",
    "type": "Microsoft.Automation/automationAccounts/runbooks",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "location": "[parameters('regionId')]",
    "dependsOn": [
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('startVmsRunbookName'))]"
    ],
    "tags": { },
    "properties": {
        "runbookType": "PowerShell",
        "logProgress": "true",
        "logVerbose": "true",
        "description": "",
        "publishContentLink": {
            "uri": "https://www.powershellgallery.com/api/v2/items/psscript/package/Update-ModulesInAutomationToLatestVersion/1.0/Start-AzureV2VMs.ps1",
            "version": "1.0"
        }
    }

### <a name="starting-a-runbook"></a>Spuštění postupu runbook
V řešení pro správu začít postupu runbook dvěma způsoby.

- Spuštění postupu runbook při řešení je nainstalováno, vytvořte [úlohy zdroje](#automation-jobs) podle níže uvedeného popisu.
- Začít postupu runbook při plánování vytvoření [plánu zdroje](#schedules) podle níže uvedeného popisu. 


## <a name="automation-jobs"></a>Automatizace úloh
Abyste mohli začít postupu runbook řešení pro správu je nainstalovaná, vytvořte **pracovní** zdroj.  Zdroje projektu s typem **Microsoft.Automation/automationAccounts/jobs** a mít vlastnosti v následující tabulce.

| Vlastnost | Popis |
|:--|:--|
| postupu runbook    | Entita jedno **jméno** s názvem postupu runbook.  |
| Parametry | Entita pro každý parametr vyžadované postupu runbook. |


Projekt obsahuje název postupu runbook a všechny hodnoty parametrů nechat zasílat postupu runbook.  Úlohy musíte, [závisí na](operations-management-suite-solutions-creating.md#resources) postupu runbook, které se spouští od postupu runbook musí být vytvořeny před projektu.  V dalších úlohách pro runbooks, která by měla být dokončen před stávající lze také vytvořit závislosti.

Název zdroje úlohy musí obsahovat identifikátor GUID, což je obvykle přidělil parametru.  Další informace o GUID parametrů v [vytváření řešení v sadě správy operace (OMS)](operations-management-suite-solutions-creating.md#parameters).  

Následuje příklad zdroj projektu, který se spustí postupu runbook řešení pro správu je nainstalovaná.  Jeden další runbooks musí být dokončen před tím se spustí, tak, aby měl závislosti u projektů pro tohoto postupu runbook.  Podrobné informace o projektu jsou k dispozici prostřednictvím parametry a proměnných spíše než přímo zadané.

    {
        "name": "[concat(parameters('accountName'), '/', parameters('startVmsRunbookGuid'))]",
        "type": "Microsoft.Automation/automationAccounts/jobs",
        "apiVersion": "[variables('AutomationApiVersion')]",
        "location": "[parameters('regionId')]",
        "dependsOn": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('startVmsRunbookName'))]",
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/jobs/', parameters('initialPrepRunbookGuid'))]"
        ],
        "tags": { },
        "properties": {
            "runbook": {
                "name": "[variables('startVmsRunbookName')]"
            },
            "parameters": {
                "AzureConnectionAssetName": "[variables('AzureConnectionAssetName')]",
                "ResourceGroupName": "[variables('ResourceGroupName')]"
            }
        }
    }


## <a name="certificates"></a>Certifikáty
[Automatizace Azure certifikáty](../automation/automation-certificates.md) mít typ **Microsoft.Automation/automationAccounts/certificates** a mít vlastnosti v následující tabulce.

| Vlastnost | Popis |
|:--|:--|
| base64Value   | Základní 64 hodnotu certifikátu. |
| Miniatura    | Miniatura certifikátu. |

Příklad certifikát zdroje je menší než.

    "name": "[concat(parameters('accountName'), '/MyCertificate')]",
    "type": "certificates",
    "apiVersion": "2015-01-01-preview",
    "location": "[parameters('regionId')]",
    "tags": {},
    "dependsOn": [
    ],
    "properties": {
        "base64Value": "MIIC1jCCAA8gAwIAVgIYJY4wXCXH/YAHMtALh7qEFzANAgkqhkiG5w0AAQUFGDAUMRIwEBYDVQQDEwlsA3NhAGevc3QqHhcNMTZwNjI5MHQxMjAsWhcNOjEwNjI5NKWwMDAwWjAURIwEAYDVQQBEwlsA2NhAGhvc3QwggEiMA0GCSqGSIA3DQEAAQUAA4IADwAwggEKAoIAAQDIyzv2A0RUg1/AAryI9W1DGAHAqqGdlFfTkUSDfv+hEZTAwKv0p8daqY6GroT8Du7ctQmrxJsy8JxIpDWxUaWwXtvv1kR9eG9Vs5dw8gqhjtOwgXvkOcFdKdQwA82PkcXoHlo+NlAiiPPgmHSELGvcL1uOgl3v+UFiiD1ro4qYqR0ITNhSlq5v2QJIPnka8FshFyPHhVtjtKfQkc9G/xDePW8dHwAhfi8VYRmVMmJAEOLCAJzRjnsgAfznP8CZ/QUczPF8LuTZ/WA/RaK1/Arj6VAo1VwHFY4AZXAolz7xs2sTuHplVO7FL8X58UvF7nlxq48W1Vu0l8oDi2HjvAgMAAAGjJDAiMAsGA1UdDwREAwIEsDATAgNVHSUEDDAKAggrAgEFNQcDATANAgkqhkiG9w0AAQUFAAOCAQEAk8ak2A5Ug4Iay2v0uXAk95qdAthJQN5qIVA13Qay8p4MG/S5+aXOVz4uMXGt18QjGds1A7Q8KDV4Slnwn95sVgA5EP7akvoGXhgAp8Dm90sac3+aSG4fo1V7Y/FYgAgpEy4C/5mKFD1ATeyyhy3PmF0+ZQRJ7aLDPAXioh98LrzMZr1ijzlAAKfJxzwZhpJamAwjZCYqiNZ54r4C4wA4QgX9sVfQKd5e/gQnUM8gTQIjQ8G2973jqxaVNw9lZnVKW3C8/QyLit20pNoqX2qQedwsqg3WCUcPRUUqZ4NpQeHL/AvKIrt158zAfU903yElAEm2Zr3oOUR4WfYQ==",
        "thumbprint": "F485CBE5569F7A5019CB68D7G6D987AC85124B4C"
    }

## <a name="credentials"></a>Přihlašovací údaje
[Automatizace Azure pověření](../automation/automation-credentials.md) mít typ **Microsoft.Automation/automationAccounts/credentials** a mít vlastnosti v následující tabulce.

| Vlastnost | Popis |
|:--|:--|
| uživatelské jméno   | Uživatelské jméno pro přihlašovací údaje. |
| heslo   | Heslo pro přihlašovací údaje. |

Příklad přihlašovacích údajů zdroje je menší než.

    "name": "[concat(parameters('accountName'), '/', variables('credentialName'))]",
    "type": "Microsoft.Automation/automationAccounts/runbooks/credentials",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "location": "[parameters('regionId')]",
    "tags": { },
    "dependsOn": [
    ],
    "properties": {
        "userName": "User01",
        "password": "password"
    }


## <a name="schedules"></a>Plány
[Automatizace Azure plány](../automation/automation-schedules.md) mít typ **Microsoft.Automation/automationAccounts/schedules** a mít vlastnosti v následující tabulce.

| Vlastnost | Popis |
|:--|:--|
| Popis | Volitelný popis k plánu. |
| čas spuštění   | Určuje čas spuštění plánu jako objekt data a času. Řetězec lze zadat, pokud lze převést na platný datový typ DateTime. |
| isEnabled   | Určuje, zda je povolen plánu. |
| interval    | Typ intervalu k plánu.<br><br>den<br>Hodina |
| četnosti   | Četnost plánu by měl aktivováno počet hodin nebo dnů. |

Příklad plán zdrojů je menší než.

    "name": "[concat(parameters('accountName'), '/', variables('variableName'))]",
    "type": "microsoft.automation/automationAccounts/schedules",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "tags": { },
    "dependsOn": [
    ],
    "properties": {
        "description": "Schedule for StartByResourceGroup runbook",
        "startTime": "10/30/2016 12:00:00",
        "isEnabled": "true",
        "interval": "day",
        "frequency": "1"
    }


## <a name="variables"></a>Proměnné
[Automatizace Azure proměnné](../automation/automation-variables.md) typu **Microsoft.Automation/automationAccounts/variables** a mít vlastnosti v následující tabulce.

| Vlastnost | Popis |
|:--|:--|
| Popis | Volitelný popis proměnné. |
| isEncrypted | Určuje, zda má být zašifrován proměnné. |
| Typ        | Datový typ proměnné. |
| Hodnota       | Hodnota proměnné. |

Příklad proměnné zdroje je menší než.

    "name": "[concat(parameters('accountName'), '/', variables('StartTargetResourceGroupsAssetName')) ]",
    "type": "microsoft.automation/automationAccounts/variables",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "tags": { },
    "dependsOn": [
    ],
    "properties": {
        "description": "Description for the variable.",
        "isEncrypted": "true",
        "type": "string",
        "value": "'This is a string value.'"
    }


## <a name="modules"></a>Moduly
Řešení pro správu nemusí definovat [globální moduly](../automation/automation-integration-modules.md) použít tak, že vaše runbooks, protože vždy budou k dispozici.  Potřebujete zahrnout zdroje pro ostatní moduly používané vaší runbooks a postupu runbook by měl závisí na modulu zdroje zajistit, že už je vytvořená před postupu runbook.

[Moduly integrace](../automation/automation-integration-modules.md) s typem **Microsoft.Automation/automationAccounts/modules** a mají vlastnosti v následující tabulce.

| Vlastnost | Popis |
|:--|:--|
| contentLink | Určuje obsah modulu. <br><br>identifikátor URI - Uri obsahu postupu runbook.  To bude .ps1 souboru pro PowerShell a skript runbooks a soubor exportovaného grafický postupu runbook pro graf postupu runbook.  <br> verze: verze postupu runbook pro vlastní sledování. |

Příklad modul zdroje je menší než.

    {       
        "name": "[concat(parameters('accountName'), '/', variables('OMSIngestionModuleName'))]",
        "type": "Microsoft.Automation/automationAccounts/modules",
        "apiVersion": "[variables('AutomationApiVersion')]",
        "dependsOn": [
        ],
        "properties": {
            "contentLink": {
                "uri": "https://devopsgallerystorage.blob.core.windows.net/packages/omsingestionapi.1.3.0.nupkg"
            }
        }
    }

### <a name="updating-modules"></a>Aktualizace moduly
Pokud aktualizujete řešení pro správu, které obsahuje postupu runbook používající plán a novou verzi řešení má nový modul používaný tohoto postupu runbook, může pomocí postupu runbook starou verzi modulu.  By měl obsahuje následující runbooks ve vašem řešení a vytvořit úlohu spustit před další runbooks.  Zajistíte tak, že všechny moduly aktualizovány jako povinné před načtením runbooks.

- [Aktualizace ModulesinAutomationToLatestVersion](https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/DisplayScript) zajistí, že všechny moduly používaný runbooks ve vašem řešení nejnovější verzi.  
- [ReRegisterAutomationSchedule-MS-Správa](https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/DisplayScript) se zaregistrovat všechny zdroje plánu zajistit, aby runbooks automaticky připojenými s volbou nejnovější moduly.


Následuje ukázka povinných elementů řešení, které podporují modul aktualizace.
    
    "parameters": {
        "ModuleImportGuid": {
            "type": "string",
            "metadata": {
                "control": "guid"
            }
        },
        "ReRegisterRunbookGuid": {
            "type": "string",
            "metadata": {
                "control": "guid"
            }
        }
    },

    "variables": {
        "ModuleImportRunbookName": "Update-ModulesInAutomationToLatestVersion",
        "ModuleImportRunbookUri": "https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/Content/Update-ModulesInAutomationToLatestVersion.ps1",
        "ModuleImportRunbookDescription": "Imports modules and also updates all Azure dependent modules that are in the same Automation Account.",
        "ReRegisterSchedulesRunbookName": "Update-ModulesInAutomationToLatestVersion",
        "ReRegisterSchedulesRunbookUri": "https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/Content/ReRegisterAutomationSchedule-MS-Mgmt.ps1",
        "ReRegisterSchedulesRunbookDescription": "Reregisters schedules to ensure that they use latest modules."
    },

    "resources": [
        {
            "name": "[concat(parameters('accountName'), '/', variables('ModuleImportRunbookName'))]",
            "type": "Microsoft.Automation/automationAccounts/runbooks",
            "apiVersion": "[variables('AutomationApiVersion')]",
            "dependsOn": [
            ],
            "location": "[parameters('regionId')]",
            "tags": { },
            "properties": {
                "runbookType": "PowerShell",
                "logProgress": "true",
                "logVerbose": "true",
                "description": "[variables('ModuleImportRunbookDescription')]",
                "publishContentLink": {
                    "uri": "[variables('ModuleImportRunbookUri')]",
                    "version": "1.0.0.0"
                }
            }
        },
        {
            "name": "[concat(parameters('accountName'), '/', parameters('ModuleImportGuid'))]",
            "type": "Microsoft.Automation/automationAccounts/jobs",
            "apiVersion": "[variables('AutomationApiVersion')]",
            "location": "[parameters('regionId')]",
            "dependsOn": [
                "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('ModuleImportRunbookName'))]"
            ],
            "tags": { },
            "properties": {
                "runbook": {
                    "name": "[variables('ModuleImportRunbookName')]"
                },
                "parameters": {
                    "ResourceGroupName": "[resourceGroup().name]",
                    "AutomationAccountName": "[parameters('accountName')]",
                    "NewModuleName": "AzureRM.Insights"
                }
            }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('ReRegisterSchedulesRunbookName'))]",
          "type": "Microsoft.Automation/automationAccounts/runbooks",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "dependsOn": [
          ],
          "location": "[parameters('regionId')]",
          "tags": { },
          "properties": {
            "runbookType": "PowerShell",
            "logProgress": "true",
            "logVerbose": "true",
            "description": "[variables('ReRegisterSchedulesRunbookDescription')]",
            "publishContentLink": {
              "uri": "[variables('ReRegisterSchedulesRunbookUri')]",
              "version": "1.0.0.0"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', parameters('reRegisterSchedulesGuid'))]",
          "type": "Microsoft.Automation/automationAccounts/jobs",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "dependsOn": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('ReRegisterSchedulesRunbookName'))]"
          ],
          "tags": { },
          "properties": {
            "runbook": {
              "name": "[variables('ReRegisterSchedulesRunbookName')]"
            },
            "parameters": {
              "targetSubscriptionId": "[subscription().subscriptionId]",
              "resourcegroup": "[resourceGroup().name]",
              "automationaccount": "[parameters('accountName')]"
            }
        }
    ]


## <a name="next-steps"></a>Další kroky

- [Přidat zobrazení do vašeho řešení](operations-management-suite-solutions-resources-views.md) vizualizace dat shromážděných.