<properties
    pageTitle="Vytvoření události rozbočovače názvů s centrální událostí a povolení archivu pomocí šablony správce prostředků Azure | Microsoft Azure"
    description="Vytvoření události rozbočovače názvů s centrální událostí a povolení archivu pomocí šablony správce prostředků Azure"
    services="event-hubs"
    documentationCenter=".net"
    authors="ShubhaVijayasarathy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="09/14/2016"
    ms.author="ShubhaVijayasarathy"/>

# <a name="create-an-event-hubs-namespace-with-event-hub-and-enable-archive-using-an-azure-resource-manager-template"></a>Vytvoření události rozbočovače názvů s centrální událostí a povolení archivu pomocí šablony správce prostředků Azure

Tento článek ukazuje, jak použít šablonu správce prostředků Azure, vytvoří událost rozbočovače názvů s rozbočovači události, který umožňuje archiv na vaše Centrum události. Se naučíte, jak definovat prostředků, které používají a jak definovat parametry, které jsou uvedené provedení nasazení. Můžete pomocí této šablony vlastního nasazení nebo přizpůsobit tak, aby vyhovovala vašim požadavkům

Další informace o vytváření šablon najdete v tématu [vytváření správce prostředků Azure šablony][].

Další informace o praktické cvičení a vzorky konvence Azure materiály najdete v článku [Konvence pojmenování Azure zdroje][].

Dokončení šablony najdete v [Centrální událostí a povolit archivu šablony][] na GitHub.

>[AZURE.NOTE]
>Nejnovější šablony, navštěvujte blog do Galerie [Šablon Azure rychlý úvod][] a vyhledejte rozbočovače události.

## <a name="what-you-deploy"></a>Co nasadíte?

Pomocí této šablony nasazení události rozbočovače obor názvů s rozbočovači událostí a povolte archivu.

[Událost rozbočovače](../event-hubs/event-hubs-what-is-event-hubs.md) je události služba slouží k poskytování událostí a telemetrie průniku Azure ve velkém měřítku obrovské, zhoršeným latence a vysoký spolehlivost pro zpracování. Archiv rozbočovače události vám umožní automaticky předávat streamování data v události rozbočovače k úložišti objektů Blob Azure podle svého výběru v daném čase nebo intervalu velikost e.

Automatické spuštění nasazení, klikněte na toto tlačítko:

[![Nasazení Azure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-eventhubs-create-namespace-and-enable-archive%2Fazuredeploy.json)

## <a name="parameters"></a>Parametry

Pomocí Správce prostředků Azure definovat parametry pro hodnoty, které chcete zadat při nasazení šablony. Šablona obsahuje oddílu s názvem `Parameters` , která obsahuje všechny hodnoty parametrů. Je třeba definovat parametr hodnoty, které se liší podle projektu, který nasazujete nebo podle prostředí, které nasazujete. Není definovat parametry pro hodnoty, které vždy zůstávají stejné. Každá hodnota parametru slouží v šabloně k definování zdroje, které používají.

Šablona definuje následujících parametrů.

### <a name="eventhubnamespacename"></a>eventHubNamespaceName

Název oboru rozbočovače události chcete vytvořit.

```
"eventHubNamespaceName":{  
     "type":"string",
     "metadata":{  
         "description":"Name of the EventHub namespace"
      }
}
```

### <a name="eventhubname"></a>eventHubName

Název události centrální vytvořené v případě rozbočovače názvů.

```
"eventHubName":{  
    "type":"string",
    "metadata":{  
        "description":"Name of the Event Hub"
    }
}
```

### <a name="messageretentionindays"></a>messageRetentionInDays

Počet dnů chcete zprávy jsou zachovány v centrální události. 

```
"messageRetentionInDays":{
    "type":"int",
    "defaultValue": 1,
    "minValue":"1",
    "maxValue":"7",
    "metadata":{
       "description":"How long to retain the data in Event Hub"
     }
 }
```

### <a name="partitioncount"></a>partitionCount

Počet oddílů, které chcete v centrální události.

```
"partitionCount":{
    "type":"int",
    "defaultValue":2,
    "minValue":2,
    "maxValue":32,
    "metadata":{
        "description":"Number of partitions chosen"
    }
 }
```

### <a name="archiveenabled"></a>archiveEnabled

Povolte archiv na vaše Centrum události.

```
"archiveEnabled":{
    "type":"string",
    "defaultValue":"true",
    "allowedValues": [
    "false",
    "true"],
    "metadata":{
        "description":"Enable or disable the Archive for your Event Hub"
    }
 }
```
### <a name="archiveencodingformat"></a>archiveEncodingFormat

Formát kódování zadáte serializovat Data události.

```
"archiveEncodingFormat":{
    "type":"string",
    "defaultValue":"Avro",
    "allowedValues":[
    "Avro"],
    "metadata":{
        "description":"The encoding format Archive serializes the EventData"
    }
}
```

### <a name="archivetime"></a>archiveTime

Časový interval, pro niž archivu spustí archivace dat v úložišti objektů blob Azure.

```
"archiveTime":{
    "type":"int",
    "defaultValue":300,
    "minValue":60,
    "maxValue":900,
    "metadata":{
         "description":"the time window in seconds for the archival"
    }
}
```

### <a name="archivesize"></a>archiveSize

Interval velikost niž archivu spustí archivace dat v úložišti objektů blob Azure.

```
"archiveSize":{
    "type":"int",
    "defaultValue":314572800,
    "minValue":10485760,
    "maxValue":524288000,
    "metadata":{
        "description":"the size window in bytes for archival"
    }
}
```

### <a name="destinationstorageaccountresourceid"></a>destinationStorageAccountResourceId

Archiv vyžaduje zdroje účtu úložiště id povolit archivace k úložišti požadované Azure.

```
 "destinationStorageAccountResourceId":{
    "type":"string",
    "metadata":{
        "description":"Your existing storage Account resource id where you want the blobs be archived"
    }
 }
```

### <a name="blobcontainername"></a>blobContainerName

Archivovat kontejneru objektů blob místo, kam chcete Data události.

```
 "blobContainerName":{
    "type":"string",
    "metadata":{
        "description":"Your existing storage Container that you want the blobs archived in"
    }
}
```


### <a name="apiversion"></a>apiVersion

Rozhraní API verze šablony.

```
 "apiVersion":{  
    "type":"string",
    "defaultValue":"2015-08-01",
    "metadata":{  
        "description":"ApiVersion used by the template"
    }
 }
```

## <a name="resources-to-deploy"></a>Materiály pro nasazení

Vytvoří obor názvů typu **EventHubs**s rozbočovači událostí a umožňuje archivu.

```
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('eventHubNamespaceName')]",
         "type":"Microsoft.EventHub/Namespaces",
         "location":"[variables('location')]",
         "sku":{  
            "name":"Standard",
            "tier":"Standard"
         },
         "resources":[  
            {  
               "apiVersion":"[variables('ehVersion')]",
               "name":"[parameters('eventHubName')]",
               "type":"EventHubs",
               "dependsOn":[  
                  "[concat('Microsoft.EventHub/namespaces/', parameters('eventHubNamespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]",
                  "MessageRetentionInDays":"[parameters('messageRetentionInDays')]",
                  "PartitionCount":"[parameters('partitionCount')]",
                  "ArchiveDescription":{
                        "enabled":"[parameters('archiveEnabled')]",
                        "encoding":"[parameters('archiveEncodingFormat')]",
                        "intervalInSeconds":"[parameters('archiveTime')]",
                        "sizeLimitInBytes":"[parameters('archiveSize')]",
                        "destination":{
                            "name":"EventHubArchive.AzureBlockBlob",
                            "properties":{
                                "StorageAccountResourceId":"[parameters('destinationStorageAccountResourceId')]",
                                "BlobContainer":"[parameters('blobContainerName')]"
                            }
                        } 
                  }
                  
               }
               
            }
         ]
      }
   ]
```

## <a name="commands-to-run-deployment"></a>Příkazy pro spuštění nasazení

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>Prostředí PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-archive/azuredeploy.json
```

## <a name="azure-cli"></a>Azure rozhraní příkazového řádku

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-archive/azuredeploy.json][]
```

[Vytváření šablon správce prostředků Azure]: ../resource-group-authoring-templates.md
[Rychlý úvod Azure šablony]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Event Hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-eventhubs-create-namespace-and-enable-archive/
[Konvence pojmenování Azure zdroje]: https://azure.microsoft.com/en-us/documentation/articles/guidance-naming-conventions/
[Centrální událostí a povolit archivu šablony]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-archive
