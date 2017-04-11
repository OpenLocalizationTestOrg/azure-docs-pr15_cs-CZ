<properties
    pageTitle="Vytvoření rozbočovači IoT pomocí Správce prostředků Azure šablony a prostředí PowerShell | Microsoft Azure"
    description="Postupujte podle kurzu, který Začínáme s používáním šablon správce prostředků Azure vytvořit rozbočovači IoT s Powershellu."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="multiple"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/07/2016"
     ms.author="dobett"/>

# <a name="create-an-iot-hub-using-powershell"></a>Vytvoření rozbočovači IoT pomocí prostředí PowerShell

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Úvod

Správce prostředků Azure slouží k vytváření a správa Azure IoT rozbočovače programově. Tento kurz se dozvíte, jak použít šablonu správce prostředků Azure k vytvoření rozbočovači IoT pomocí prostředí PowerShell.

> [AZURE.NOTE] Azure obsahuje dva různé nasazení modely pro vytváření grafů a práci s prostředky: [Správce prostředků Azure a klasické](../resource-manager-deployment-model.md).  Tento článek se věnuje pomocí Správce prostředků Azure nasazení modelu.

Tento kurz je potřeba k provedení těchto věcí:

- Účet Azure active. <br/>Pokud nemáte účet, můžete vytvořit [bezplatný účet] [ lnk-free-trial] v jenom pár minut.
- [Microsoft Azure PowerShell 1.0] [ lnk-powershell-install] nebo novější.

> [AZURE.TIP] Článek [Pomocí prostředí PowerShell Azure pomocí Správce prostředků Azure] [ lnk-powershell-arm] poskytuje další informace o tom, jak pomocí skriptů Powershellu a správce prostředků Azure šablony vytvořit Azure zdroje. 

## <a name="connect-to-your-azure-subscription"></a>Připojení k předplatnému Azure

Na příkazovém řádku prostředí PowerShell zadejte tento příkaz se přihlásit k předplatnému Azure:

```
Login-AzureRmAccount
```

Můžete zjistit, kde můžete nasadit rozbočovači IoT a aktuálně podporovaných verzí rozhraní API následující příkazy:

```
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).Locations
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).ApiVersions
```

Vytvoření skupiny prostředků má být vaše Centrum IoT pomocí následujícího příkazu v jednom z podporovaných umístění pro centrální IoT. Tento příklad vytvoří skupinu zdroje s názvem **MyIoTRG1**:

```
New-AzureRmResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="submit-an-azure-resource-manager-template-to-create-an-iot-hub"></a>Odeslat správce prostředků Azure šablonu k vytvoření rozbočovači IoT

Pomocí šablony JSON vytvořit rozbočovači IoT ve skupině prostředků. Taky můžete šablonu správce prostředků Azure ke změnám existující centrální IoT.

1. Umožňuje vytvořit šablonu správce prostředků Azure s názvem **template.json** s definicí následující zdroje k vytvoření nové standardní centrální IoT textovém editoru. V tomto příkladu přidá centru IoT v oblasti **Východoasijských USA** , vytvoří dva spotř skupiny (**cg1** a **cg2**) na koncový bod kompatibilní s prohlížečem události centrální a používá verze **2016 02 03** rozhraní API. Tato šablona také očekává předat v názvu centrální IoT jako parametr s názvem **hubName**. Aktuální seznam míst, které podporují IoT centrální najdete v článku [Azure stav][lnk-status].

    ```
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": {
          "type": "string"
        }
      },
      "resources": [
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs",
        "name": "[parameters('hubName')]",
        "location": "East US",
        "sku": {
          "name": "S1",
          "tier": "Standard",
          "capacity": 1
        },
        "properties": {
          "location": "East US"
        }
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg1')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg2')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      }
      ],
      "outputs": {
        "hubKeys": {
          "value": "[listKeys(resourceId('Microsoft.Devices/IotHubs', parameters('hubName')), '2016-02-03')]",
          "type": "object"
        }
      }
    }
    ```

2. Uložte soubor šablony správce prostředků Azure na místním počítači. V tomto příkladu předpokládá, že ho uložit ve složce s názvem **c:\templates**.

3. Spusťte tento příkaz nasaďte novou centrální IoT předejte název rozbočovače IoT jako parametr. V tomto příkladu je v poli Název centru IoT **abcmyiothub** (Všimněte si, že tento název musí být jedinečné, takže by měl obsahovat vaše jméno nebo iniciály):

    ```
    New-AzureRmResourceGroupDeployment -ResourceGroupName MyIoTRG1 -TemplateFile C:\templates\template.json -hubName abcmyiothub
    ```

4. Výstup zobrazí klávesy rozbočovače IoT, který jste vytvořili.

5. Můžete ověřit, že aplikace přidal nový rozbočovač IoT návštěva [Azure portál] [ lnk-azure-portal] a zobrazení Seznam zdrojů nebo můžete použít rutinu Powershellu **Get-AzureRmResource** .

> [AZURE.NOTE] Tento příklad aplikace přidá S1 standardní IoT rozbočovači pro kterou se vám účtovat poplatky. Můžete odstranit centru IoT přes [Azure portál] [ lnk-azure-portal] nebo pomocí rutiny prostředí PowerShell **Odebrat AzureRmResource** po dokončení.

## <a name="next-steps"></a>Další kroky

Teď jste nasadili rozbočovači IoT šablony pro správce prostředků Azure pomocí prostředí PowerShell, může chcete prozkoumat další:

- Přečtěte si o možnostech [IoT Centrum zdrojů poskytovatele rozhraní REST API][lnk-rest-api].
- Přečtěte si [Přehled Správce prostředků Azure] [ lnk-azure-rm-overview] Další informace o možnostech správce prostředků Azure.

Další informace o vytváření pro centrální IoT, najdete v těchto článcích:

- [Úvod k C SDK][lnk-c-sdk]
- [Rozbočovač IoT SDK][lnk-sdks]

Prozkoumat další možnosti IoT centrální, najdete tady:

- [Napodobení zařízení s SDK IoT brány][lnk-gateway]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-powershell-install]: ../powershell-install-configure.md
[lnk-rest-api]: https://msdn.microsoft.com/library/mt589014.aspx
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md
[lnk-powershell-arm]: ../powershell-azure-resource-manager.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
