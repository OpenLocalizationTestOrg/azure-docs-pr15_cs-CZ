<properties
    pageTitle="Vytvoření rozbočovači IoT pomocí rozhraní REST API | Microsoft Azure"
    description="Postupujte podle kurzu, který začít používat rozhraní REST API vytvořit rozbočovači IoT."
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

# <a name="tutorial-create-an-iot-hub-using-a-c-program-and-the-rest-api"></a>Kurz: Vytváření rozbočovači IoT pomocí programu C# a rozhraní REST API

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Úvod

Používáte [rozbočovač IoT zdroje poskytovatele rozhraní REST API] [ lnk-rest-api] můžete vytvořit a spravovat Azure IoT rozbočovače programově. Tento kurz se dozvíte, jak pro použití poskytovatele zdroje IoT centrální rozhraní REST API pro vytvoření rozbočovači IoT času z programu C#.

> [AZURE.NOTE] Azure obsahuje dva různé nasazení modely pro vytváření grafů a práci s prostředky: [Správce prostředků Azure a klasické](../resource-manager-deployment-model.md).  Tento článek se věnuje pomocí Správce prostředků Azure nasazení modelu.

Tento kurz, je potřeba k provedení těchto věcí:

- Microsoft Visual Studio 2015.
- Účet Azure active. <br/>Pokud nemáte účet, můžete vytvořit [bezplatný účet] [ lnk-free-trial] v jenom pár minut.
- [Microsoft Azure PowerShell 1.0] [ lnk-powershell-install] nebo novější.

[AZURE.INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>Příprava projektu aplikace Visual Studio

1. Ve Visual Studiu vytvořte Visual Basic Windows projektu v šabloně **Aplikace konzoly** projektu. Zadejte název projektu **CreateIoTHubREST**.

2. V Průzkumníku klikněte pravým tlačítkem myši na projektu a potom klikněte na **Spravovat balíčky NuGet**.

3. Ve Správci balíčku Nuget zaškrtněte **Zahrnout zkušební**a vyhledejte **Microsoft.Azure.Management.ResourceManager**. Klikněte na tlačítko **instalovat**, **Revidovat změny** kliknutím na tlačítko **OK**a potom na **přijímám** přijmout licencí.

4. Ve Správci balíčku Nuget Hledat **Microsoft.IdentityModel.Clients.ActiveDirectory**.  Klikněte na tlačítko **instalovat**, **Revidovat změny** kliknutím na tlačítko **OK**a potom na **přijímám** přijměte licenční.

6. V Program.cs nahraďte stávající příkazy **pomocí** následujících akcí:

    ```
    using System;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Text;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Newtonsoft.Json;
    using Microsoft.Rest;
    using System.Linq;
    using System.Threading;
    ```
    
7. V Program.cs přidejte následující statické proměnné nahrazení hodnot zástupný symbol. Poznámka: **ApplicationId**, **SubscriptionId**, **TenantId**a **heslo** volání dříve v tomto kurzu. **Název skupiny zdroje** je název Skupina zdroje, které používáte při vytvoření centra IoT, může být stávajících skupina zdroje nebo novou. **Rozbočovač IoT název** je název centru IoT vytvoříte, například **MyIoTHub** (Tento název musí být jedinečné, takže by měl obsahovat vaše jméno nebo iniciály). **Nasazení název** je název pro nasazení, například **Deployment_01**.

    ```
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId}";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";
    
    static string rgName = "{Resource group name}";
    static string iotHubName = "{IoT Hub name including your initials}";
    ```

[AZURE.INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## <a name="use-the-rest-api-to-create-an-iot-hub"></a>Vytvoření rozbočovači IoT pomocí rozhraní REST API

Použití [Rozhraní REST API IoT centrální] [ lnk-rest-api] vytvořit rozbočovači IoT ve skupině prostředků. Můžete taky rozhraní REST API ke změnám existující centrální IoT.

1. Přidejte Program.cs následujícím způsobem:
    
    ```
    static void CreateIoTHub(string token)
    {
        
    }
    ```

2. Přidejte následující kód metody **CreateIoTHub** **HttpClient** objekt vytvořte tokenu ověřování v záhlaví:

    ```
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
    ```

3. Přidejte následující kód metody **CreateIoTHub** popisuje Centrum IoT vytvářet a generovat formátu JSON (aktuální seznam míst, které podporují IoT centrální najdete v článku [Azure stav][lnk-status]):

    ```
    var description = new
    {
      name = iotHubName,
      location = "East US",
      sku = new
      {
        name = "S1",
        tier = "Standard",
        capacity = 1
      }
    };
    
    var json = JsonConvert.SerializeObject(description, Formatting.Indented);
    ```

4. Přidejte následující kód metody **CreateIoTHub** ZBÝVAJÍCÍ žádost o Azure, kontrolu odpověď a získat adresu URL slouží ke sledování stavu úkolu nasazení:

    ```
    var content = new StringContent(JsonConvert.SerializeObject(description), Encoding.UTF8, "application/json");
    var requestUri = string.Format("https://management.azure.com/subscriptions/{0}/resourcegroups/{1}/providers/Microsoft.devices/IotHubs/{2}?api-version=2016-02-03", subscriptionId, rgName, iotHubName);
    var result = client.PutAsync(requestUri, content).Result;
      
    if (!result.IsSuccessStatusCode)
    {
      Console.WriteLine("Failed {0}", result.Content.ReadAsStringAsync().Result);
      return;
    }
    
    var asyncStatusUri = result.Headers.GetValues("Azure-AsyncOperation").First();
    ```

5. Přidejte následující kód za účelem metodu **CreateIoTHub** použijete adresu **asyncStatusUri** načíst v předchozím kroku až nasazení dokončete:

    ```
    string body;
    do
    {
      Thread.Sleep(10000);
      HttpResponseMessage deploymentstatus = client.GetAsync(asyncStatusUri).Result;
      body = deploymentstatus.Content.ReadAsStringAsync().Result;
    } while (body == "{\"status\":\"Running\"}");
    ```

6. Přidejte následující kód za účelem metodu **CreateIoTHub** k načtení klíčů centru IoT vytvoření a tisk ke konzole:

    ```
    var listKeysUri = string.Format("https://management.azure.com/subscriptions/{0}/resourceGroups/{1}/providers/Microsoft.Devices/IotHubs/{2}/IoTHubKeys/listkeys?api-version=2015-08-15-preview", subscriptionId, rgName, iotHubName);
    var keysresults = client.PostAsync(listKeysUri, null).Result;
    
    Console.WriteLine("Keys: {0}", keysresults.Content.ReadAsStringAsync().Result);
    ```
    
## <a name="complete-and-run-the-application"></a>Dokončení a spustit aplikaci

Aplikaci teď dokončení volání metody **CreateIoTHub** před vytvořit a spustit.

1. Přidejte následující kód za účelem metodu **hlavní** :

    ```
    CreateIoTHub(token.AccessToken);
    Console.ReadLine();
    ```
    
2. Klikněte na **vytvořit** a pak **vytvoření řešení**. Oprava chyby.

3. Klikněte na **ladění** a potom **Spustit ladění** spustit aplikaci. Může trvat několik minut pro nasazení na spustit.

4. Můžete ověřit, že aplikace přidal nový rozbočovač IoT návštěva [Azure portál] [ lnk-azure-portal] a zobrazení Seznam zdrojů nebo můžete použít rutinu Powershellu **Get-AzureRmResource** .

> [AZURE.NOTE] Tento příklad aplikace přidá S1 standardní IoT rozbočovači pro kterou se vám účtovat poplatky. Až to uděláte, můžete odstranit centru IoT přes [Azure portál] [ lnk-azure-portal] nebo pomocí rutiny prostředí PowerShell **Odebrat AzureRmResource** po dokončení.

## <a name="next-steps"></a>Další kroky

Teď jste nasadili rozbočovači IoT pomocí rozhraní REST API, může chcete prozkoumat další:

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

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
