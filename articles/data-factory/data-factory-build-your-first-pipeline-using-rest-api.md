<properties
    pageTitle="Vytvoření první dat factory (REST) | Microsoft Azure"
    description="V tomto kurzu vytvoříte ukázková Azure Data Factory kanálem k odesílání zpráv pomocí rozhraní REST API dat Factory."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"
/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/16/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-first-azure-data-factory-using-data-factory-rest-api"></a>Kurz: Vytvoření výrobce první Azure dat pomocí datového Factory REST API
> [AZURE.SELECTOR]
- [Přehled a požadavky](data-factory-build-your-first-pipeline.md)
- [Azure portálu](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [Prostředí PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Správce prostředků šablony](data-factory-build-your-first-pipeline-using-arm.md)
- [ROZHRANÍ REST API](data-factory-build-your-first-pipeline-using-rest-api.md)

V tomto článku použijete dat Factory REST API k vytvoření prvního factory Azure data.

## <a name="prerequisites"></a>Zjistit předpoklady pro
- Prostudujte si článek [Přehled](data-factory-build-your-first-pipeline.md) a postupujte podle pokynů **předpoklad** .
- Nainstalujte [otáčení](https://curl.haxx.se/dlwiz/) ve vašem počítači. Použijte nástroj OTOČENÍ s ZBÝVAJÍCÍ příkazy k vytvoření data factory. 
- Postupujte podle pokynů v [tomto článku](../resource-group-create-service-principal-portal.md) se: 
    1. Vytvoření webové aplikace s názvem **ADFGetStartedApp** v Azure Active Directory.
    2. Pokud potřebujete **ID klienta** a **tajné klíče**. 
    3. Pokud potřebujete **ID klienta**. 
    4. Přiřadíte **ADFGetStartedApp** aplikace role **Přispěvatele Factory Data** .  
- Nainstalujte [Azure Powershellu](../powershell-install-configure.md).  
- Spuštění **prostředí PowerShell** a zadejte následující příkaz. Nechte otevřené Azure PowerShell až do konce tomto kurzu. Pokud zavřete a znovu otevřete, budete muset znovu spustit příkazy.
    1. Spusťte **Přihlášení AzureRmAccount** a zadejte uživatelské jméno a heslo, které používáte pro přihlášení k portálu Azure.  
    2. Spusťte **Get-AzureRmSubscription** zobrazíte všechna předplatná pro tento účet.
    3. Spuštění **Get AzureRmSubscription - SubscriptionName NameOfAzureSubscription | Nastavení AzureRmContext** vyberte předplatné, ke kterým chcete pracovat s. Nahraďte název předplatného Azure **NameOfAzureSubscription** . 
3. Vytvoření skupiny Azure zdroje s názvem **ADFTutorialResourceGroup** spuštěním následujícího příkazu v Powershellu:  

        New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

    Několik kroků v tomto kurzu se předpokládá, že používáte skupina zdroje s názvem ADFTutorialResourceGroup. Pokud používáte jiné skupině zdrojů, budete muset nesou název této skupiny zdrojů místo ADFTutorialResourceGroup v tomto kurzu.

## <a name="create-json-definitions"></a>Vytvoření JSON definice
Vytvořte následující JSON soubory ve složce, které je umístěn curl.exe. 

### <a name="datafactoryjson"></a>DataFactory.JSON 
> [AZURE.IMPORTANT] Název musí být jedinečné, tak můžete chtít předponu nebo příponu ADFCopyTutorialDF snažíme usnadnit jeho jedinečný název. 

    {  
        "name": "FirstDataFactoryREST",  
        "location": "WestUS"
    }  

### <a name="azurestoragelinkedservicejson"></a>azurestoragelinkedservice.JSON
> [AZURE.IMPORTANT] Nahraďte **název účtu** a **accountkey** název a klíč účtu Azure úložiště. Se dozvíte, jak získat přístupová klávesa úložiště, najdete v článku [zobrazení, kopírovat a přístupových kláves z verze regenerovat úložiště](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).

    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        }
    }


### <a name="hdinsightondemandlinkedservicejson"></a>hdinsightondemandlinkedservice.JSON

    {
        "name": "HDInsightOnDemandLinkedService",
        "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
                "version": "3.2",
                "clusterSize": 1,
                "timeToLive": "00:30:00",
                "linkedServiceName": "AzureStorageLinkedService"
            }
        }
    }

Následující tabulka obsahuje popis vlastností JSON používá úryvek:

| Vlastnost | Popis |
| :------- | :---------- |
| Verze | Určuje, že verze HDInsight vytvořen jako 3,2. | 
| ClusterSize | Velikost obrázku HDInsight. | 
| TimeToLive | Určuje, že prodlevy pro HDInsight clusteru, než se odstraní. |
| linkedServiceName | Určuje úložiště účet, který se používá k ukládání protokolů vytvořených pomocí HDInsight |

Mějte na paměti následující skutečnosti: 

- Data Factory vytvoří HDInsight clusteru **serveru s Windows** s nad JSON. Může taky ji máte vytvoření HDInsight clusteru **na základě Linux** . Podrobnosti najdete v části [Propojené služby na vyžádání HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . 
- Místo použití clusteru HDInsight na vyžádání můžete použít **HDInsight cluster** . Podrobnosti najdete v části [Propojené služby HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) .
- HDInsight clusteru vytvoří **výchozí kontejner** v úložišti objektů blob, které jste zadali v JSON (**linkedServiceName**). HDInsight nedojde k odstranění tento kontejner při odstranění clusteru. Toto chování se o záměr. Se službou HDInsight propojený na vyžádání HDInsight clusteru je vytvořen pokaždé, když řez zpracování není existující live obrázku (**timeToLive**) a odstraní se po dokončení zpracování.

    Jak se zpracovávají další výseče, vidíte mnoho kontejnery v úložišti objektů blob Azure. Pokud pro řešení problémů s projekty je nepotřebujete, můžete odstranit snížit náklady úložiště. Názvy těchto kontejnerů platí vzorek: "adf**yourdatafactoryname**-**linkedservicename**- datetimestamp". Odstranění kontejnery v úložišti objektů blob Azure pomocí nástrojů, jako jsou [Průzkumníka úložiště](http://storageexplorer.com/) .

Podrobnosti najdete v části [Propojené služby na vyžádání HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . 

### <a name="inputdatasetjson"></a>inputdataset.JSON

    {
        "name": "AzureBlobInput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "input.log",
                "folderPath": "adfgetstarted/inputdata",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Month",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }


Ve formátu JSON definuje datovou sadu s názvem **AzureBlobInput**, která představuje zadávání dat pro aktivita v kanálu. Kromě toho určuje, že vstupních dat se nachází v kontejneru objektů blob s názvem **adfgetstarted** a složku s názvem **inputdata**.

Následující tabulka obsahuje popis vlastností JSON používán fragment:

| Vlastnost | Popis |
| :------- | :---------- |
| Typ | Vlastnost typ je nastavena na AzureBlob, protože jsou data uložená v úložišti objektů blob Azure. |  
| linkedServiceName | odkazuje na StorageLinkedService jste dříve vytvořili. |
| Název souboru | Tato vlastnost je nepovinný krok. Vynecháme-li tuto vlastnost, jsou vybrány všechny soubory ze cesta_ke_složce. V tomto případě pouze input.log zpracování. |
| Typ | Soubory protokolu jsou v textovém formátu, takže používáme formát textu. | 
| columnDelimiter | sloupce v seznamu soubory protokolu jsou odděleny znak čárky () |
| četnost/interval | je počet_plateb měsíce a intervalu 1, což znamená, že budou vstupní výsečí měsíční dostupné. | 
| externí | Tato vlastnost je nastavena na hodnotu true, pokud není zadávání dat generované službou Data Factory. | 

### <a name="outputdatasetjson"></a>outputdataset.JSON

    {
        "name": "AzureBlobOutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "adfgetstarted/partitioneddata",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Month",
                "interval": 1
            }
        }
    }

Ve formátu JSON definuje datovou sadu s názvem **AzureBlobOutput**, která představuje výstupní data pro aktivita v kanálu. Kromě toho určuje, že výsledky jsou uloženy v kontejneru objektů blob s názvem **adfgetstarted** a složku s názvem **partitioneddata**. V části **dostupnost** Určuje, že datovou sadu výstup vyrobeno měsíčně.

### <a name="pipelinejson"></a>pipeline.JSON
> [AZURE.IMPORTANT] Nahraďte název účtu úložiště Azure **storageaccountname** . 


    {
        "name": "MyFirstPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [{
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                    "scriptLinkedService": "AzureStorageLinkedService",
                    "defines": {
                        "inputtable": "wasb://adfgetstarted@<stroageaccountname>.blob.core.windows.net/inputdata",
                        "partitionedtable": "wasb://adfgetstarted@<stroageaccountname>t.blob.core.windows.net/partitioneddata"
                    }
                },
                "inputs": [{
                    "name": "AzureBlobInput"
                }],
                "outputs": [{
                    "name": "AzureBlobOutput"
                }],
                "policy": {
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Month",
                    "interval": 1
                },
                "name": "RunSampleHiveActivity",
                "linkedServiceName": "HDInsightOnDemandLinkedService"
            }],
            "start": "2016-07-10T00:00:00Z",
            "end": "2016-07-11T00:00:00Z",
            "isPaused": false
        }
    }

V fragment JSON vytváření kanálů, což se skládá z jedné aktivity, která používá podregistru k datům proces clusteru HDInsight.

Soubor skriptu podregistru **partitionweblogs.hql**uložený v okně účet Azure úložiště (definované scriptLinkedService s názvem **StorageLinkedService**) a ve složce **skript** v kontejneru **adfgetstarted**.

Tato část **definuje** určuje runtime nastavení, které předávají skriptu podregistru jako podregistru konfigurace hodnoty (například ${hiveconf: inputtable}, ${hiveconf:partitionedtable}).

**Začátek** a **Konec** vlastnosti kanálu určí aktivního kanálu.

Činnosti JSON zadáte, že skript podregistru běží na výpočetním nastavil **linkedServiceName** – **HDInsightOnDemandLinkedService**.

> [AZURE.NOTE] Další informace o vlastnostech JSON použili v předchozím příkladu najdete v článku [Podrobný rozbor kanálů](data-factory-create-pipelines.md#anatomy-of-a-pipeline) . 

## <a name="set-global-variables"></a>Nastavení globální proměnné

V prostředí PowerShell Azure provést po nahradit hodnoty vlastní následující příkazy:

> [AZURE.IMPORTANT] Podle pokynů na získání ID klienta, tajná klienta, najdete v článku [požadavky](#prerequisites) části klient ID a ID předplatného.   

    $client_id = "<client ID of application in AAD>"
    $client_secret = "<client key of application in AAD>"
    $tenant = "<Azure tenant ID>";
    $subscription_id="<Azure subscription ID>";

    $rg = "ADFTutorialResourceGroup"
    $adf = "FirstDataFactoryREST"



## <a name="authenticate-with-aad"></a>Ověření pomocí AAD

    $cmd = { .\curl.exe -X POST https://login.microsoftonline.com/$tenant/oauth2/token  -F grant_type=client_credentials  -F resource=https://management.core.windows.net/ -F client_id=$client_id -F client_secret=$client_secret };
    $responseToken = Invoke-Command -scriptblock $cmd;
    $accessToken = (ConvertFrom-Json $responseToken).access_token;
    
    (ConvertFrom-Json $responseToken) 



## <a name="create-data-factory"></a>Vytvoření factory dat

V tomto kroku vytvoříte Azure Data Factory s názvem **FirstDataFactoryREST**. Factory dat můžete mít minimálně jednu potrubí. Potrubí může obsahovat jedno nebo více aktivitami ho. Příklad kopírování aktivitu ke kopírování dat ze zdroje do cílového úložiště dat a aktivitu HDInsight podregistru spustit podregistru skript k transformaci dat. Spusťte následující příkazy k vytvoření factory dat: 

1. Příkaz přiřadíte proměnné s názvem **cmd**. 

    Potvrďte, že název factory dat určíte tady (ADFCopyTutorialDF) shoduje s názvem podle **datafactory.json**. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@datafactory.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/FirstDataFactoryREST?api-version=2015-10-01};
2. Spusťte příkaz pomocí **Příkazu vyvolat**.

        $results = Invoke-Command -scriptblock $cmd;
3. Zobrazte výsledky. Pokud úspěšné vytvoření factory dat zobrazí JSON factory dat v seznamu **výsledků**; v opačném se zobrazí chybová zpráva.  

        Write-Host $results

Mějte na paměti následující skutečnosti:
 
- Název Azure Data Factory musí být jedinečné. Pokud se zobrazí chyba ve výsledcích: **název factory dat "FirstDataFactoryREST" není k dispozici**, proveďte následující kroky:  
    1. Změna názvu (například yournameFirstDataFactoryREST) do souboru **datafactory.json** . Pojmenování pravidla pro Data Factory artefakty naleznete v tématu [Data Factory - pojmenování pravidla](data-factory-naming-rules.md) .
    2. Příkaz v prvním kde proměnnou **$cmd** přiřazena hodnota nahraďte FirstDataFactoryREST novým názvem a příkaz spustit. 
    3. Spusťte dalších dvou příkazy pro vyvolání rozhraní REST API vytvářet factory dat a Tisk výsledků operaci. 
- Pokud chcete vytvořit instance Data Factory, budete muset správci skupiny přispěvatelů/Azure předplatného
- Název factory dat může být registrován jako název DNS v budoucnu a tedy se stanou veřejně viditelnými.
- Pokud se zobrazí chyba: "**Toto předplatné není registrovaný používat obor názvů Microsoft.DataFactory**", proveďte jednu z následujících akcí a zkuste to znovu publikovat: 

    - V prostředí PowerShell Azure, spusťte tento příkaz k registraci poskytovatele Factory dat: 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        Spuštěním následujícího příkazu potvrdit, že Data Factory registrované poskytovatele: 
    
            Get-AzureRmResourceProvider
    - Přihlášení pomocí Azure předplatného na [portálu Azure](https://portal.azure.com) a přejděte na zásuvné Data Factory (nebo) vytvářet factory dat na portálu Azure. Tato akce automaticky zaregistruje poskytovatele za vás.

Před vytvořením kanálů, je potřeba nejdřív vytvořit několik Data Factory entity. Nejdřív vytvořte propojené služby propojení dat ukládají/vypočte do vašeho úložiště, definují vstupní a výstupní datové sady představující data v úložišti propojená data. 

## <a name="create-linked-services"></a>Vytvoření propojené služby 
V tomto kroku propojíte váš účet Azure úložiště a clusteru Azure HDInsight na vyžádání data factory. Účet Azure úložiště obsahuje vstupní a výstupní data profilace v tomto příkladu. Služba HDInsight propojené slouží ke spuštění podregistru skript podle aktivity kanálu v tomto příkladu. 

### <a name="create-azure-storage-linked-service"></a>Vytvoření propojené Azure úložiště služby
V tomto kroku propojíte účtu úložiště Azure data factory. Pomocí tohoto kurzu použít stejný účet Azure úložiště pro ukládání vstupní a výstupní data a soubor skriptu HQL.

1. Příkaz přiřadíte proměnné s názvem **cmd**. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@azurestoragelinkedservice.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureStorageLinkedService?api-version=2015-10-01};
2. Spusťte příkaz pomocí **Příkazu vyvolat**.
 
        $results = Invoke-Command -scriptblock $cmd;
3. Zobrazte výsledky. Pokud úspěšné vytvoření propojených služby zobrazí JSON pro službu propojené **výsledky**; v opačném se zobrazí chybová zpráva.
  
        Write-Host $results

### <a name="create-azure-hdinsight-linked-service"></a>Vytvoření služby Azure HDInsight propojené
V tomto kroku které vytváříte odkazy clusteru HDInsight na vyžádání data factory. HDInsight clusteru je automaticky vytvořen za běhu a bude odstraněný poté se to dělá zpracování a nečinné pro zadaný časový úsek. Místo použití clusteru HDInsight na vyžádání můžete použít vlastní clusteru HDInsight. Další informace najdete v článku [Výpočet propojené služby](data-factory-compute-linked-services.md) .  

1. Příkaz přiřadíte proměnné s názvem **cmd**.
 
        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@hdinsightondemandlinkedservice.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/hdinsightondemandlinkedservice?api-version=2015-10-01};
2. Spusťte příkaz pomocí **Příkazu vyvolat**.

        $results = Invoke-Command -scriptblock $cmd;
3. Zobrazte výsledky. Pokud úspěšné vytvoření propojených služby zobrazí JSON pro službu propojené **výsledky**; v opačném se zobrazí chybová zpráva.  

        Write-Host $results

## <a name="create-datasets"></a>Vytvoření datové sady
V tomto kroku vytvoříte datové sady představují vstupní a výstupní data pro zpracování podregistru. Tyto datové sady v nápovědě k **StorageLinkedService** jste vytvořili dříve v tomto kurzu. Bodů propojené služby Azure úložiště účtu a datové sady zadejte kontejneru, složku, název souboru v úložišti obsahující vstupní a výstupní data.   

### <a name="create-input-dataset"></a>Vytvoření sady zadávání dat
V tomto kroku vytvoříte vstupní datovou sadu pro zadávání dat uložených v úložišti objektů Blob Azure.

1. Příkaz přiřadíte proměnné s názvem **cmd**. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@inputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobInput?api-version=2015-10-01};
2. Spusťte příkaz pomocí **Příkazu vyvolat**.

        $results = Invoke-Command -scriptblock $cmd;
3. Zobrazte výsledky. Pokud úspěšné vytvoření datové zobrazí JSON pro datovou sadu **výsledků**; v opačném se zobrazí chybová zpráva.
  
        Write-Host $results
### <a name="create-output-dataset"></a>Vytvoření datovou sadu výstup
V tomto kroku vytvoříte datovou sadu výstup představující výstup dat uložených v úložišti objektů Blob Azure.

1. Příkaz přiřadíte proměnné s názvem **cmd**.
 
        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@outputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobOutput?api-version=2015-10-01};
2. Spusťte příkaz pomocí **Příkazu vyvolat**.

        $results = Invoke-Command -scriptblock $cmd;
3. Zobrazte výsledky. Pokud úspěšné vytvoření datové zobrazí JSON pro datovou sadu **výsledků**; v opačném se zobrazí chybová zpráva.
  
        Write-Host $results 

## <a name="create-pipeline"></a>Vytvoření kanálu
V tomto kroku vytvoříte první profilace aktivitu **HDInsightHive** . Zadávání výseč neexistuje každý měsíc (četnost: měsíc, intervalu: 1) výstup výseč pochází měsíčně, a vlastnost Plánovač aktivity také nastavit na měsíční. Nastavení pro datovou sadu výstup a Plánovač aktivity musí odpovídat. Datovou sadu výstup je v současné době co jednotky s plánem, třeba vytvoříte datovou sadu výstup i v případě aktivity nevytvoří jakýkoli výstup. Pokud aktivity bude jakýkoli vstup, můžete přejít vytváření vstupních datovou sadu.  

Ověřte, najdete v článku **input.log** soubor ve složce **adfgetstarted/inputdata** v úložišti objektů blob Azure a spusťte tento příkaz nasazení profilace. Vzhledem k tomu čas **zahájení** a **ukončení** se nastavují v minulosti a **isPaused** je nastavena na hodnotu NEPRAVDA, kanálu (aktivita v kanálu) se spustí hned po nasazení. 

1. Příkaz přiřadíte proměnné s názvem **cmd**.
 
        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@pipeline.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datapipelines/MyFirstPipeline?api-version=2015-10-01};
2. Spusťte příkaz pomocí **Příkazu vyvolat**.

        $results = Invoke-Command -scriptblock $cmd;
3. Zobrazte výsledky. Pokud úspěšné vytvoření datové zobrazí JSON pro datovou sadu **výsledků**; v opačném se zobrazí chybová zpráva.  

        Write-Host $results
5. Gratulujeme, úspěšně jste vytvořili svůj první kanálem k odesílání zpráv pomocí prostředí PowerShell Azure!

## <a name="monitor-pipeline"></a>Sledování kanálem k odesílání zpráv
V tomto kroku pomocí rozhraní REST API dat Factory sledování výsečí adresami vytvořené pomocí kanálu.

    $ds ="AzureBlobOutput"

    $cmd = {.\curl.exe -X GET -H "Authorization: Bearer $accessToken" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/$ds/slices?start=1970-01-01T00%3a00%3a00.0000000Z"&"end=2016-08-12T00%3a00%3a00.0000000Z"&"api-version=2015-10-01};

    $results2 = Invoke-Command -scriptblock $cmd;

    IF ((ConvertFrom-Json $results2).value -ne $NULL) {
        ConvertFrom-Json $results2 | Select-Object -Expand value | Format-Table
    } else {
            (convertFrom-Json $results2).RemoteException
    }


> [AZURE.IMPORTANT] 
> Vytvoření obrázku HDInsight na vyžádání zabere někdy (přibližně 20 minut). Proto očekávat potrubí zpracování trvat **zhruba 30 minut** výseče.  

Spuštění příkazu vyvolat a dalším se zobrazila výsečí v **připravena** nebo stavu **Neúspěšný** . Pokud řez je připravena, zkontrolujte **partitioneddata** složku v kontejneru **adfgetstarted** v úložišti objektů blob pro výstupní data.  Vytvoření obrázku HDInsight na vyžádání zabere určitou dobu.

![výstupní data](./media/data-factory-build-your-first-pipeline-using-rest-api/three-ouptut-files.png)

> [AZURE.IMPORTANT] Vstupní soubor se odstraní, jakmile řez úspěšně zpracovat. Proto pokud chcete znovu spustit řez nebo znovu provést kurzu, vstupní soubor (input.log) na nahrajte inputdata složek kontejneru adfgetstarted.

Můžete taky Azure portál ke sledování výseče a při odstraňování všech problémů. Zobrazit podrobnosti [sledovat potrubí Azure portálu](data-factory-build-your-first-pipeline-using-editor.md##monitor-pipeline) .  

## <a name="summary"></a>Souhrn 
V tomto kurzu jste vytvořili factory Azure dat k datům proces spuštěním podregistru skriptu clusteru hadoop HDInsight. Editor Factory dat na portálu Azure umožňuje proveďte následující kroky:  

1.  Vytvoření Azure **data factory**.
2.  Vytvoření dvě **propojené služby**:
    1.  Služba **Úložiště azure** propojené propojit úložišti objektů blob Azure obsahující vstupní a výstupní soubory do továrny dat
    2.  Na vyžádání propojené služby **Azure HDInsight** propojíte clusteru HDInsight Hadoop na vyžádání data factory. Azure Data Factory vytvoří HDInsight Hadoop clusteru těsně za běhu zpracování zadávání dat a předložit výstupní data. 
3.  Vytvoří dva **datové sady**, které popisují vstupní a výstupní data pro HDInsight podregistru aktivity v kanálu. 
4.  Vytvoření **profilace** aktivitu **Podregistru HDInsight** . 

## <a name="next-steps"></a>Další kroky
V tomto článku vytvořili jste profilace aktivitu transformace (HDInsight aktivita), které se spouští skript podregistru clusteru Azure HDInsight na vyžádání. Informace o použití aktivitu na Kopírovat zkopírujte data z objektů Blob Azure Azure SQL, najdete v článku [kurz: zkopírování dat z objektů Blob Azure Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

## <a name="see-also"></a>Viz taky
| Téma | Popis |
| :---- | :---- |
| [Data Factory REST API odkaz](https://msdn.microsoft.com/library/azure/dn906738.aspx) |  Přečtěte si článek komplexní si přečtěte následující dokumentaci na Data Factory rutiny |
| [Činnost transformace dat](data-factory-data-transformation-activities.md) | Tento článek obsahuje seznam aktivit transformace dat (třeba jste použili v tomto kurzu transformace HDInsight podregistru) nepodporuje Azure Data Factory. |
| [Plánování a provádění](data-factory-scheduling-and-execution.md) | Tento článek vysvětluje plánování a provádění aspekty model aplikace Azure Data Factory. |
| [Potrubí](data-factory-create-pipelines.md) | Tento článek vám pomůže porozumět kanály a aktivity v Azure Data Factory a jak je lze používat k vytvoření začátku do konce postupů založených na datech pro scénář nebo business. |
| [Datové sady](data-factory-create-datasets.md) | Tento článek vám pomůže pochopit datové sady v Azure Data Factory.
| [Sledování a správa potrubí pomocí Azure portálu listy](data-factory-monitor-manage-pipelines.md) | Tento článek popisuje, jak sledovat, Správa a ladění vaší potrubí pomocí Azure portálu listy. |
| [Sledování a správa potrubí pomocí sledování aplikace](data-factory-monitor-manage-app.md) | Tento článek popisuje, jak sledovat, Správa a ladění potrubí pomocí příkazu sledování a Správa aplikací. 

