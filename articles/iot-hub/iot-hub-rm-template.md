<properties
    pageTitle="Vytvoření rozbočovači IoT pomocí ARM šablony a C# | Microsoft Azure"
    description="Postupujte podle kurzu, který Začínáme s používáním šablon správce prostředků Azure vytvořit rozbočovači IoT s C# programu."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/16/2016"
     ms.author="dobett"/>

# <a name="create-an-iot-hub-using-a-c-program-with-an-azure-resource-manager-template"></a>Vytvoření rozbočovači IoT programem C# šablonou správce prostředků Azure

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Úvod

Správce prostředků Azure slouží k vytváření a správa Azure IoT rozbočovače programově. Tento kurz se dozvíte, jak používat šablonu správce prostředků Azure k vytváření rozbočovači IoT času z programu C#.

> [AZURE.NOTE] Azure obsahuje dva různé nasazení modely pro vytváření grafů a práci s prostředky: [Správce prostředků Azure a klasické](../resource-manager-deployment-model.md).  Tento článek se věnuje pomocí Správce prostředků Azure nasazení modelu.

Tento kurz, je potřeba k provedení těchto věcí:

- Microsoft Visual Studio 2015.
- Účet Azure active. <br/>Pokud nemáte účet, můžete vytvořit [bezplatný účet] [ lnk-free-trial] v jenom pár minut.
- [Účet Azure úložiště] [ lnk-storage-account] kde můžete ukládat soubory šablon správce prostředků Azure.
- [Microsoft Azure PowerShell 1.0] [ lnk-powershell-install] nebo novější.

[AZURE.INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>Příprava projektu aplikace Visual Studio

1. Ve Visual Studiu vytvořte Visual Basic Windows projektu v šabloně **Aplikace konzoly** projektu. Zadejte název projektu **CreateIoTHub**.

2. V Průzkumníku klikněte pravým tlačítkem myši na projektu a potom klikněte na **Spravovat balíčky NuGet**.

3. Ve Správci balíčku Nuget zaškrtněte **Zahrnout zkušební**a vyhledejte **Microsoft.Azure.Management.ResourceManager**. Klikněte na tlačítko **instalovat**, **Revidovat změny** kliknutím na tlačítko **OK**a potom na **přijímám** přijmout licencí.

4. Ve Správci balíčku Nuget Hledat **Microsoft.IdentityModel.Clients.ActiveDirectory**.  Klikněte na tlačítko **instalovat**, **Revidovat změny** kliknutím na tlačítko **OK**a potom na **přijímám** přijměte licenční.

5. V Program.cs nahraďte stávající příkazy **pomocí** následujících akcí:

    ```
    using System;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```
    
6. V Program.cs přidejte následující statické proměnné nahrazení hodnot zástupný symbol. Poznámka: **ApplicationId**, **SubscriptionId**, **TenantId**a **heslo** volání dříve v tomto kurzu. **Název účtu úložiště your Azure** je název účtu úložiště Azure, kam ukládat soubory šablon správce prostředků Azure. **Název skupiny zdroje** je název Skupina zdroje, které používáte při vytvoření centra IoT, může být stávajících skupina zdroje nebo novou. **Nasazení název** je název pro nasazení, například **Deployment_01**.

    ```
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId}";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";
    static string storageAddress = "https://{Your storage account name}.blob.core.windows.net";
    static string rgName = "{Resource group name}";
    static string deploymentName = "{Deployment name}";
    ```

[AZURE.INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## <a name="submit-an-azure-resource-manager-template-to-create-an-iot-hub"></a>Odeslat správce prostředků Azure šablonu k vytvoření rozbočovači IoT

Pomocí souboru šablony a parametr JSON vytvořit rozbočovači IoT ve skupině prostředků. Taky můžete šablonu správce prostředků Azure ke změnám existující centrální IoT.

1. V Průzkumníku klikněte pravým tlačítkem myši na projektu, klikněte na tlačítko **Přidat**a potom klikněte na **Nová položka**. Přidání souboru JSON s názvem **template.json** do projektu.

2. Nahraďte obsah **template.json** definici následující zdroje přidat standardní centrální IoT k oblasti **Východoasijských USA** (aktuální seznam oblastí, které podporují IoT centrální najdete v článku [Azure stav][lnk-status]):

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

3. V Průzkumníku klikněte pravým tlačítkem myši na projektu, klikněte na tlačítko **Přidat**a potom klikněte na **Nová položka**. Přidání souboru JSON s názvem **parameters.json** do projektu.

4. Nahrazení obsahu **parameters.json** s následujícími informacemi parametr, která nastavuje jako název nové centrum IoT **{Iniciály} mynewiothub**. Nezapomeňte, že název centrální IoT musí být jedinečné, měl by obsahovat vaše jméno nebo iniciály):

    ```
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": { "value": "mynewiothub" }
      }
    }
    ```

5. V **Průzkumníku serveru**připojit k předplatnému Azure a ve vašem úložišti Azure účtu vytvořit kontejner s názvem **šablony**. Na panelu **Vlastnosti** nastavení oprávnění **veřejné přístup pro čtení** pro kontejner **šablon** **objektů Blob**.

6. V **Průzkumníku serveru**klikněte pravým tlačítkem myši na kontejner **šablony** a klikněte na **Zobrazení objektů Blob kontejner**. Klikněte na tlačítko **Nahrát objektů Blob** , vyberte dva soubory, **parameters.json** a **templates.json**a klepněte na tlačítko **Otevřít** JSON soubory nahrajete do kontejneru **šablony** . Adresy URL obsahující JSON data objektů BLOB jsou:

    ```
    https://{Your storage account name}.blob.core.windows.net/templates/parameters.json
    https://{Your storage account name}.blob.core.windows.net/templates/template.json
    ```

7. Přidejte Program.cs následujícím způsobem:
    
    ```
    static void CreateIoTHub(ResourceManagementClient client)
    {
        
    }
    ```

5. Přidejte následující kód metody **CreateIoTHub** odesílat soubory šablony a parametr správce prostředků Azure:

    ```
    var createResponse = client.Deployments.CreateOrUpdate(
        rgName,
        deploymentName,
        new Deployment()
        {
          Properties = new DeploymentProperties
          {
            Mode = DeploymentMode.Incremental,
            TemplateLink = new TemplateLink
            {
              Uri = storageAddress + "/templates/template.json"
            },
            ParametersLink = new ParametersLink
            {
              Uri = storageAddress + "/templates/parameters.json"
            }
          }
        });
    ```

6. Přidejte následující kód metody **CreateIoTHub** zobrazující stav a klíče pro nové centrum IoT:

    ```
    string state = createResponse.Properties.ProvisioningState;
    Console.WriteLine("Deployment state: {0}", state);

    if (state != "Succeeded")
    {
      Console.WriteLine("Failed to create iothub");
    }
    Console.WriteLine(createResponse.Properties.Outputs);
    ```

## <a name="complete-and-run-the-application"></a>Dokončení a spustit aplikaci

Aplikaci teď dokončení volání metody **CreateIoTHub** před vytvořit a spustit.

1. Přidejte následující kód za účelem metodu **hlavní** :

    ```
    CreateIoTHub(client);
    Console.ReadLine();
    ```
    
2. Klikněte na **vytvořit** a pak **vytvoření řešení**. Oprava chyby.

3. Klikněte na **ladění** a potom **Spustit ladění** spustit aplikaci. Může trvat několik minut pro nasazení na spustit.

4. Můžete ověřit, že aplikace přidal nový rozbočovač IoT návštěva [Azure portál] [ lnk-azure-portal] a zobrazení Seznam zdrojů nebo můžete použít rutinu Powershellu **Get-AzureRmResource** .

> [AZURE.NOTE] Tento příklad aplikace přidá S1 standardní IoT rozbočovači pro kterou se vám účtovat poplatky. Můžete odstranit centru IoT přes [Azure portál] [ lnk-azure-portal] nebo pomocí rutiny prostředí PowerShell **Odebrat AzureRmResource** po dokončení.

## <a name="next-steps"></a>Další kroky

Teď nasadili rozbočovači IoT šablony pro správce prostředků Azure pomocí programu C# může chcete prozkoumat další:

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
[lnk-storage-account]: ../storage/storage-create-storage-account.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
