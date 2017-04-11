<properties
    pageTitle="Vytvoření rozbočovači IoT pomocí rozhraní příkazového řádku Azure | Microsoft Azure"
    description="Postupujte podle tohoto článku Vytvoření rozbočovači IoT pomocí rozhraní Azure příkazového řádku."
    services="iot-hub"
    documentationCenter=".net"
    authors="BeatriceOltean"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="multiple"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/21/2016"
     ms.author="boltean"/>

# <a name="create-an-iot-hub-using-azure-cli"></a>Vytvoření rozbočovači IoT pomocí rozhraní příkazového řádku Azure

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Úvod

Rozhraní příkazového řádku Azure slouží k vytváření a správa Azure IoT rozbočovače programově. Tento článek popisuje, jak vytvořit rozbočovači IoT pomocí rozhraní příkazového řádku Azure.

Tento kurz je potřeba k provedení těchto věcí:

- Účet Azure active. Pokud nemáte účet, můžete vytvořit [bezplatný účet] [ lnk-free-trial] v jenom pár minut.
- [Azure rozhraní příkazového řádku 0.10.4] [ lnk-CLI-install] nebo novější. Pokud už máte rozhraní příkazového řádku Azure aktuální verzi příkazového řádku pomocí následujícího příkazu můžete ověřit:
```
    azure --version
```

> [AZURE.NOTE] Azure obsahuje dva různé nasazení modely pro vytváření grafů a práci s prostředky: [Správce prostředků Azure a klasické](../resource-manager-deployment-model.md). Rozhraní příkazového řádku Azure musí být v režimu správce prostředků Azure:
```
    azure config mode arm
```

## <a name="set-your-azure-account-and-subscription"></a>Nastavit účet Azure a předplatného 

1. Při přihlášení příkazovém řádku zadejte tento příkaz
```
    azure login
```
Umožňuje navrhovaných webový prohlížeč a kód ověření.

2. Pokud máte víc předplatných Azure, bude připojení k Azure udělit přístup k Všechna předplatná Azure přidružené přihlašovacích údajů. Můžete zobrazit Azure předplatná, který je ve výchozím pomocí příkazu
```
    azure account list 
```

Chcete-li nastavit předplatné kontext, pod kterým chcete spustit zbytek použijte příkazy

```
    azure account set <subscription name>
```

3. Pokud nemáte skupina zdroje můžete vytvořit jednu pojmenované **exampleResourceGroup** 
```
    azure group create -n exampleResourceGroup -l westus
```

> [AZURE.TIP] V článku [použití Azure rozhraní příkazového řádku pro správu Azure materiály a zdroje skupin] [ lnk-CLI-arm] poskytuje další informace o používání rozhraní příkazového řádku Azure ke správě Azure prostředků. 


## <a name="create-an-iot-hub"></a>Vytvoření rozbočovači IoT

Požadované parametry:

```
 azure iothub create -g <resource-group> -n <name> -l <location> -s <sku-name> -u <units>  
    - <resourceGroup> The resource group name (case insensitive alphanumeric, underscore and hyphen, 1-64 length)
    - <name> (The name of the IoT hub to be created. The format is case insensitive alphanumeric, underscore and hyphen, 3-50 length )
    - <location> (The location (azure region/datacenter) where the IoT hub will be provisioned.
    - <sku-name> (The name of the sku, one of: [F1, S1, S2, S3] etc. For the latest full list refer to the pricing page for IoT Hub.
    - <units> (The number of provisioned units. Range : F1 [1-1] : S1, S2 [1-200] : S3 [1-10]. IoT Hub units are based on your total message count and the number of devices you want to connect.)
```
Zobrazíte všechny parametry dostupné pro vytvoření můžete příkaz Pomozte příkazový řádek
```
    azure iothub create -h 
```
Rychlý příklad:

 Vytvoření rozbočovači IoT s názvem **exampleIoTHubName** ve skupině zdroje **exampleResourceGroup** , jednoduše spusťte tento příkaz
```
    azure iothub create -g exampleResourceGroup -n exampleIoTHubName -l westus -k s1 -u 1
```

> [AZURE.NOTE] Příkaz rozhraní příkazového řádku Azure vytvoří S1 standardní IoT rozbočovači pro kterou se vám účtovat poplatky. Můžete odstranit pomocí následující příkaz IoT centrální **exampleIoTHubName** 
```
    azure iothub delete -g exampleResourceGroup -n exampleIoTHubName
```


## <a name="next-steps"></a>Další kroky
Další informace o vytváření pro centrální IoT, najdete v těchto článcích:
- [Rozbočovač IoT SDK][lnk-sdks]

Prozkoumat další možnosti IoT centrální, najdete tady:

- [Správa IoT centrální pomocí portálu Azure][lnk-portal]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-CLI-install]: ../xplat-cli-install.md
[lnk-rest-api]: https://msdn.microsoft.com/library/mt589014.aspx
[lnk-CLI-arm]: ../xplat-cli-azure-resource-manager.md

[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-portal]: iot-hub-create-through-portal.md 
