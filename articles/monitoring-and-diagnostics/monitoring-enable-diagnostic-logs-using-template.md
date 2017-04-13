<properties
    pageTitle="Automaticky povolit diagnostické nastavení pomocí šablony správce prostředků | Microsoft Azure"
    description="Naučte se pomocí Správce prostředků šablony můžete vytvořit diagnostické nastavení, které vám umožní vysílat diagnostických protokolů k rozbočovače události nebo ukládány v účtu úložiště."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="johnkem"/>

# <a name="automatically-enable-diagnostic-settings-at-resource-creation-using-a-resource-manager-template"></a>Automaticky povolte diagnostické nastavení při vytvoření zdrojů pomocí šablony správce prostředků
V tomto článku ukážeme, jak můžete s [Správce prostředků Azure šablony](../resource-group-authoring-templates.md) ke konfiguraci diagnostiky zdroje při vytvoření. Umožňuje automaticky spouštět datových proudů protokoly pro diagnostiku a metriky k události rozbočovače, aby je archivoval v účtu úložiště nebo odesláním protokolu analýzy při vytváření zdroje.

Metody pro povolení protokoly pro diagnostiku pomocí šablony správce prostředků závisí na typu zdroje.

- **Bez výpočetním** zdrojů (například skupiny zabezpečení sítě, použití logických operátorů aplikace automatizaci) pomocí [diagnostických nastavení popisované v tomto článku](./monitoring-overview-of-diagnostic-logs.md#diagnostic-settings).
- **Výpočet** (WAD/LAD založené) zdrojů pomocí [konfiguračního souboru WAD/LAD popisované v tomto článku](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md).

V tomto článku jsme popisují, jak nakonfigurovat diagnostiky oběma způsoby.

Základní kroky, které jsou následujícím způsobem:

1. Vytvoření šablony jako JSON soubor, který popisuje, jak vytvořit zdroj a povolení diagnostických nástrojů.
2. [Nasazení šablony jakýmkoli způsobem nasazení](../resource-group-template-deploy.md).

Tady vidíte nabízíme příkladem JSON soubor šablony, které bude nutné generovat pro není výpočetním a výpočetním zdroje.

## <a name="non-compute-resource-template"></a>Šablona non-výpočetním zdroje
Není výpočetním zdrojů budete potřebovat provést dva kroky:

1. Přidáte parametry objektů blob parametry pro název účtu úložiště, ID pravidla bus služby nebo ID pracovního prostoru OMS protokolu analýzy (povolení archivace protokoly pro diagnostiku v účtu úložiště, datových proudů protokolů, které chcete rozbočovače událost nebo odeslání protokolů do protokolu analýzy).

    ```json
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "serviceBusRuleId": {
      "type": "string",
      "metadata": {
        "description": "Service Bus Rule Id for the Service Bus Namespace in which the Event Hub should be created or streamed to."
      }
    },
    "workspaceId":{
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for the Log Analytics workspace to which logs will be sent."
      }
    }
    ```
2. V poli zdroje zdroje, pro kterou chcete povolit protokoly pro diagnostiku přidání zdroje typu `[resource namespace]/providers/diagnosticSettings`.

    ```json
    "resources": [
      {
        "type": "providers/diagnosticSettings",
        "name": "Microsoft.Insights/service",
        "dependsOn": [
          "[/*resource Id for which Diagnostic Logs will be enabled>*/]"
        ],
        "apiVersion": "2015-07-01",
        "properties": {
          "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
          "serviceBusRuleId": "[parameters('serviceBusRuleId')]",
          "workspaceId": "[parameters('workspaceId')]",
          "logs": [ 
            {
              "category": "/* log category name */",
              "enabled": true,
              "retentionPolicy": {
                "days": 0,
                "enabled": false
              }
            }
          ]
        }
      }
    ]
    ```

Vlastnosti objektů blob diagnostiky nastavení sleduje [formátu popisované v tomto článku](https://msdn.microsoft.com/library/azure/dn931931.aspx).

Tady je příklad úplné vytvoří skupinu zabezpečení sítě a také bude zapnuta streaming rozbočovače událostí a úložiště v účtu úložiště.

```json

{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "nsgName": {
      "type": "string",
      "metadata": {
        "description": "Name of the NSG that will be created."
      }
    },
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "serviceBusRuleId": {
      "type": "string",
      "metadata": {
        "description": "Service Bus Rule Id for the Service Bus Namespace in which the Event Hub should be created or streamed to."
      }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for the Log Analytics workspace to which logs will be sent."
      }
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Network/networkSecurityGroups",
      "name": "[parameters('nsgName')]",
      "apiVersion": "2016-03-30",
      "location": "westus",
      "properties": {
        "securityRules": []
      },
      "resources": [
        {
          "type": "providers/diagnosticSettings",
          "name": "Microsoft.Insights/service",
          "dependsOn": [
            "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('nsgName'))]"
          ],
          "apiVersion": "2015-07-01",
          "properties": {
            "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
            "serviceBusRuleId": "[parameters('serviceBusRuleId')]",
            "workspaceId": "[parameters('workspaceId')]",
            "logs": [
              {
                "category": "NetworkSecurityGroupEvent",
                "enabled": true,
                "retentionPolicy": {
                  "days": 0,
                  "enabled": false
                }
              },
              {
                "category": "NetworkSecurityGroupRuleCounter",
                "enabled": true,
                "retentionPolicy": {
                  "days": 0,
                  "enabled": false
                }
              }
            ]
          }
        }
      ],
      "dependsOn": []
    }
  ]
}

```

## <a name="compute-resource-template"></a>Výpočet zdroje šablony
Povolit diagnostiku výpočetních zdroje, například virtuálního počítače nebo služby struktury clusteru, musíte:

1. Přidejte příponu Azure diagnostiky definici OM zdroje.
2. Zadejte rozbočovači úložiště účtu a/nebo zvláštní události jako parametr.
3. Přidání obsahu souboru WADCfg XML do vlastnosti XMLCfg úniku všechny znaky XML správně.

> [AZURE.WARNING] Tento poslední krok může být záludné správného. Příklad, který schéma konfigurace diagnostiky rozdělí proměnné, které jsou uvést a správně naformátovaný [najdete v tomto článku](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md#diagnostics-configuration-variables) .

Celého procesu, včetně výběry, je popsáno [v tomto dokumentu](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md).


## <a name="next-steps"></a>Další kroky
- [Přečtěte si něco víc o protokoly pro diagnostiku Azure](./monitoring-overview-of-diagnostic-logs.md)
- [Azure diagnostické protokoly událostí rozbočovače můžete vysílat datovými proudy](./monitoring-stream-diagnostic-logs-to-event-hubs.md)
