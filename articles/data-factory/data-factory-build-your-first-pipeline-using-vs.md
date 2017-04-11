<properties
    pageTitle="Vytvoření první dat factory (Visual Studio) | Microsoft Azure"
    description="V tomto kurzu vytvoříte ukázková Azure Data Factory kanálem k odesílání zpráv pomocí aplikace Visual Studio."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article" 
    ms.date="10/17/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-azure-first-data-factory-using-microsoft-visual-studio"></a>Kurz: Sestavení Azure první dat factory pomocí Microsoft Visual Studio
> [AZURE.SELECTOR]
- [Přehled a požadavky](data-factory-build-your-first-pipeline.md)
- [Azure portálu](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [Prostředí PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Správce prostředků šablony](data-factory-build-your-first-pipeline-using-arm.md)
- [ROZHRANÍ REST API](data-factory-build-your-first-pipeline-using-rest-api.md)

V tomto článku použijete Microsoft Visual Studio k vytvoření prvního factory Azure data.

## <a name="prerequisites"></a>Zjistit předpoklady pro
1. Prostudujte si článek [Přehled](data-factory-build-your-first-pipeline.md) a postupujte podle pokynů **předpoklad** .
2. Je možné publikovat entit Factory dat z aplikace Visual Studio Azure Data Factory musí být **Správce Azure předplatného** .
3. Musí mít na počítači nainstalovaný následující: 
    - Visual Studio 2013 nebo Visual Studio 2015
    - Stáhněte si Azure SDK Visual Studio 2013 nebo Visual Studio 2015. Přejděte na [Stránku Stáhnout Azure](https://azure.microsoft.com/downloads/) a klikněte na **a 2013** nebo **2015 a** v části **.NET** .
    - Stáhněte si nejnovější modul plug-in Azure Data Factory for Visual Studio: [2013 a](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) nebo [a 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Modul plug-in je také možné aktualizovat podle následujících pokynů: V nabídce klikněte na **Nástroje** -> **přípony a aktualizace** -> **Online** -> **Galerie Visual Studio** -> **Microsoft Azure datové Factory nástroje for Visual Studio** -> **Aktualizovat**. 
 
Teď Pojďme vytvořit factory Azure dat pomocí aplikace Visual Studio. 


## <a name="create-visual-studio-project"></a>Vytvoření projektu Visual Studio 
1. Spuštění **aplikace Visual Studio 2013** nebo **Visual Studio 2015**. Klikněte na **soubor**, přejděte na **Nový**a klikněte na **projekt**. Zobrazí dialogové okno **Nový projekt** .  
2. V dialogovém okně **Nový projekt** vyberte šablonu **DataFactory** a klikněte na **Prázdný projekt Factory Data**.   

    ![Dialogové okno Nový projekt](./media/data-factory-build-your-first-pipeline-using-vs/new-project-dialog.png)

3. Zadejte **název** projektu, **umístění**a název **řešení**a klikněte na **OK**.

    ![Průzkumník řešení](./media/data-factory-build-your-first-pipeline-using-vs/solution-explorer.png)

## <a name="create-linked-services"></a>Vytvoření propojené služby
Factory dat můžete mít minimálně jednu potrubí. Potrubí může mít jeden nebo více aktivity v nich. Například kopírovat aktivitu ke kopírování dat ze zdroje do cílového úložiště dat a aktivitu HDInsight podregistru spustit podregistru skript k transformaci vstupní data. Najdete v článku [podporované datové ukládá](data-factory-data-movement-activities.md##supported-data-stores-and-formats) pro všechny zdroje a propadů nepodporuje aktivitu kopírovat. Najdete v článku [Výpočet propojené služby](data-factory-compute-linked-services.md) pro seznam služby výpočetním podporované Data Factory. 

V tomto kroku propojíte váš účet Azure úložiště a clusteru Azure HDInsight na vyžádání data factory. Účet Azure úložiště obsahuje vstupní a výstupní data profilace v tomto příkladu. Služba HDInsight propojené slouží ke spuštění podregistru skript podle aktivity kanálu v tomto příkladu. Určit data, která úložiště/výpočetním služeb se používají nefunguje a propojit tyto služby factory dat vytvořením propojené služby.  

Při publikování dat Factory řešení zadejte název a nastavení factory dat později.

#### <a name="create-azure-storage-linked-service"></a>Vytvoření propojené Azure úložiště služby
V tomto kroku propojíte účtu úložiště Azure data factory. Pro účely tohoto návodu použít stejný účet Azure úložiště pro ukládání vstupní a výstupní data a soubor skriptu HQL. 

4. Klikněte pravým tlačítkem myši **Propojené služby** v okně Průzkumník, přejděte na **Přidat**a klikněte na **Nová položka**.      
5. V dialogovém okně **Přidat novou položku** ze seznamu vyberte **Propojené služba Azure úložiště** a klikněte na **Přidat**. 
3. Nahraďte **název účtu** a **accountkey** název účtu Azure úložiště a klíče. Se dozvíte, jak získat přístupová klávesa úložiště, přečtěte si téma [zobrazení, kopírovat a regenerovat úložiště přístupových kláves](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)

    ![Propojené Azure úložiště služby](./media/data-factory-build-your-first-pipeline-using-vs/azure-storage-linked-service.png)

4. Uložte soubor **AzureStorageLinkedService1.json** .

#### <a name="create-azure-hdinsight-linked-service"></a>Vytvoření služby Azure HDInsight propojené
V tomto kroku které vytváříte odkazy clusteru HDInsight na vyžádání data factory. HDInsight clusteru je automaticky vytvořen za běhu a bude odstraněný poté se to dělá zpracování a nečinné pro zadaný časový úsek. Místo použití clusteru HDInsight na vyžádání můžete použít HDInsight cluster. Další informace najdete v článku [Výpočet propojené služby](data-factory-compute-linked-services.md) . 

1. V **Okně Průzkumník řešení**klikněte pravým tlačítkem myši **Propojené služby**, přejděte na **Přidat**a klikněte na **Nová položka**.
2. Vyberte **HDInsight služba propojené služba**a klikněte na **Přidat**. 
3. Nahraďte **JSON** takto:

        {
          "name": "HDInsightOnDemandLinkedService",
          "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
              "version": "3.2",
              "clusterSize": 1,
              "timeToLive": "00:30:00",
              "linkedServiceName": "AzureStorageLinkedService1"
            }
          }
        }
    
    Následující tabulka obsahuje popis vlastností JSON používá úryvek:
    
    Vlastnost | Popis
    -------- | -----------
    Verze | Určuje, že verze HDInsight vytvořen jako 3,2. 
    ClusterSize | Určuje velikost obrázku HDInsight. 
    TimeToLive | Určuje, že prodlevy pro HDInsight clusteru, než se odstraní.
    linkedServiceName | Určuje úložiště účet, který se používá k ukládání protokolů vytvořených pomocí HDInsight

    Poznámka: 
    
    - Data Factory vytvoří HDInsight clusteru **serveru s Windows** s předchozím JSON. Může taky ji máte vytvoření HDInsight clusteru **na základě Linux** . Podrobnosti najdete v části [Propojené služby na vyžádání HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . 
    - Místo použití clusteru HDInsight na vyžádání můžete použít **HDInsight cluster** . Podrobnosti najdete v části [Propojené služby HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) .
    - HDInsight clusteru vytvoří **výchozí kontejner** v úložišti objektů blob, které jste zadali v JSON (**linkedServiceName**). HDInsight nedojde k odstranění tento kontejner při odstranění clusteru. Toto chování se o záměr. Se službou HDInsight propojený na vyžádání se vytvoří clusteru HDInsight pokaždé, když řez zpracování nejedná existujícího živá clusteru (**timeToLive**). Po dokončení zpracování se automaticky odstraní clusteru.
    
        Jak se zpracovávají další výseče, vidíte mnoho kontejnery v úložišti objektů blob Azure. Pokud pro řešení problémů s projekty je nepotřebujete, můžete odstranit snížit náklady úložiště. Názvy těchto kontejnerů platí vzorek: "adf**yourdatafactoryname**-**linkedservicename**- datetimestamp". Odstranění kontejnery v úložišti objektů blob Azure pomocí nástrojů, jako jsou [Průzkumníka úložiště](http://storageexplorer.com/) .

    Podrobnosti najdete v části [Propojené služby na vyžádání HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . 
4. Uložte soubor **HDInsightOnDemandLinkedService1.json** .

## <a name="create-datasets"></a>Vytvoření datové sady
V tomto kroku vytvoříte datové sady představují vstupní a výstupní data pro zpracování podregistru. Tyto datové sady v nápovědě k **AzureStorageLinkedService1** jste vytvořili dříve v tomto kurzu. Bodů propojené služby Azure úložiště účtu a datové sady zadejte kontejneru, složku, název souboru v úložišti obsahující vstupní a výstupní data.   

#### <a name="create-input-dataset"></a>Vytvoření sady zadávání dat

1. V **Okně Průzkumník řešení**klikněte pravým tlačítkem na **tabulky**, přejděte na **Přidat**a klikněte na **Nová položka**. 
2. V seznamu vyberte **Objektů Blob Azure** , změňte název souboru na **InputDataSet.json**a klikněte na **Přidat**.
3. Nahraďte **JSON** v editoru takto: 

    V fragment JSON vytváříte datovou sadu s názvem **AzureBlobInput** představující zadávání dat pro aktivita v kanálu. Kromě toho určíte, že vstupních dat se nachází v kontejneru objektů blob s názvem **adfgetstarted** a složku s názvem **inputdata**
        
        {
            "name": "AzureBlobInput",
            "properties": {
                "type": "AzureBlob",
                "linkedServiceName": "AzureStorageLinkedService1",
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

    Následující tabulka obsahuje popis vlastností JSON používá úryvek:

  	| Vlastnost | Popis |
  	| :------- | :---------- |
  	| Typ | Vlastnost typ je nastavena na AzureBlob, protože jsou data uložená v úložišti objektů blob Azure. |  
  	| linkedServiceName | odkazuje na AzureStorageLinkedService1 jste dříve vytvořili. |
  	| Název souboru | Tato vlastnost je nepovinný krok. Vynecháme-li tuto vlastnost, jsou vybrány všechny soubory ze cesta_ke_složce. V tomto případě pouze input.log zpracování. |
  	| Typ | Soubory protokolu jsou v textovém formátu, takže používáme formát textu. | 
  	| columnDelimiter | sloupce v seznamu soubory protokolu jsou odděleny čárka () |
  	| četnost/interval | je počet_plateb měsíce a intervalu 1, což znamená, že budou vstupní výsečí měsíční dostupné. | 
  	| externí | Tato vlastnost je nastavena na hodnotu true, pokud není zadávání dat generované službou Data Factory. | 
      
    
3. Uložte soubor **InputDataset.json** . 

 
#### <a name="create-output-dataset"></a>Vytvoření datovou sadu výstup
Dále vytvořte datovou sadu výstup představující výstup dat uložených v úložišti objektů Blob Azure. 

1. V **Okně Průzkumník řešení**klikněte pravým tlačítkem na **tabulky**, přejděte na **Přidat**a klikněte na **Nová položka**. 
2. V seznamu vyberte **Objektů Blob Azure** , změňte název souboru na **OutputDataset.json**a klikněte na **Přidat**. 
3. Nahraďte **JSON** v editoru takto: 

    V fragment JSON vytváříte datovou sadu s názvem **AzureBlobOutput**a specifikována struktura data, která je vytvořené pomocí skriptu podregistru. Kromě toho určíte, že výsledky jsou uloženy v kontejneru objektů blob s názvem **adfgetstarted** a složku s názvem **partitioneddata**. V části **dostupnost** Určuje, že datovou sadu výstup vyrobeno měsíčně.
    
        {
          "name": "AzureBlobOutput",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService1",
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

    **Vytváření vstupních datovou sadu** v části popisy těchto vlastností. Vlastnost externí nejsou nastavené na datovou sadu výstup jako datové vytvořené pomocí služeb Data Factory.

4. Uložte soubor **OutputDataset.json** .


### <a name="create-pipeline"></a>Vytvoření kanálu
V tomto kroku vytvoříte aktivitu **HDInsightHive** vaše první kanálem k odesílání zpráv. Zadávání výseč neexistuje každý měsíc (četnost: měsíc, intervalu: 1) výstup výseč pochází měsíčně, a vlastnost Plánovač aktivity také nastavit na měsíční. Nastavení pro datovou sadu výstup a Plánovač aktivitu musí odpovídat. Datovou sadu výstup je v současné době co jednotky plánu, musíte vytvoříte datovou sadu výstup i v případě aktivity nevytvoří jakýkoli výstup. Pokud aktivity bude jakýkoli vstup, můžete přejít vytváření vstupních datovou sadu. Na konci této části jsou vysvětleny vlastnosti použité v následujících JSON.

1. V **Okně Průzkumník řešení**, klikněte pravým tlačítkem myši **kanály**, přejděte na **Přidat**a klikněte na **Nová položka.** 
2. V seznamu vyberte **Podregistru transformace kanálu** a klikněte na **Přidat**. 
3. Nahraďte **JSON** následující fragment kódu.

    > [AZURE.IMPORTANT] Nahraďte **storageaccountname** název účtu úložiště.

        {
            "name": "MyFirstPipeline",
            "properties": {
                "description": "My first Azure Data Factory pipeline",
                "activities": [
                    {
                        "type": "HDInsightHive",
                        "typeProperties": {
                            "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                            "scriptLinkedService": "AzureStorageLinkedService1",
                            "defines": {
                                "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                                "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                            }
                        },
                        "inputs": [
                            {
                                "name": "AzureBlobInput"
                            }
                        ],
                        "outputs": [
                            {
                                "name": "AzureBlobOutput"
                            }
                        ],
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
                    }
                ],
                "start": "2016-04-01T00:00:00Z",
                "end": "2016-04-02T00:00:00Z",
                "isPaused": false
            }
        }

    V fragment JSON vytváření kanálů, což se skládá z jedné aktivity, která používá podregistru procesu dat clusteru HDInsight.
    
    V fragment JSON vytváření kanálů, což se skládá z jedné aktivity, která používá podregistru procesu dat clusteru HDInsight.
    
    Soubor skriptu podregistru **partitionweblogs.hql**uložený v okně účet Azure úložiště (definované scriptLinkedService s názvem **AzureStorageLinkedService1**) a ve složce **skript** v kontejneru **adfgetstarted**.

    V části **definuje** slouží k určení runtime nastavení, které předávají skriptu podregistru jako podregistru konfigurace hodnoty (například ${hiveconf: inputtable}, ${hiveconf:partitionedtable}).

    **Začátek** a **Konec** vlastnosti kanálu určí aktivního kanálu.

    Činnosti JSON zadáte, že skript podregistru běží na výpočetním nastavil **linkedServiceName** – **HDInsightOnDemandLinkedService**.

    > [AZURE.NOTE] Další informace o vlastnostech JSON použitých v tomto příkladu najdete v článku [Podrobný rozbor kanálů](data-factory-create-pipelines.md#anatomy-of-a-pipeline) . 
3. Uložte soubor **HiveActivity1.json** .

### <a name="add-partitionweblogshql-and-inputlog-as-a-dependency"></a>Přidání partitionweblogs.hql a input.log jako závislostí 

1. Klikněte pravým tlačítkem myši **závislosti** v okně **Průzkumník řešení** , přejděte na **Přidat**a klikněte na **Existující položku**.  
2. Přejděte na **C:\ADFGettingStarted** a vyberte **partitionweblogs.hql**, **input.log** soubory a klikněte na **Přidat**. Jste vytvořili tyto dva soubory jako součást požadavky z [Přehledu kurzu](data-factory-build-your-first-pipeline.md).

Při publikování řešení v dalším kroku soubor **partitionweblogs.hql** nahrát do složky skriptů v kontejneru objektů blob **adfgetstarted** .   

### <a name="publishdeploy-data-factory-entities"></a>Publikování a nasazení Entity Data Factory

18. Klikněte pravým tlačítkem na projektu v Průzkumníku řešení a klikněte na **Publikovat**. 
19. Pokud se zobrazí dialogové okno **přihlásit ke svému účtu Microsoft** , zadejte svoje přihlašovací údaje pro účet, který má Azure předplatné a klikněte na **přihlásit**.
20. Měli byste vidět dialogovým oknem takto:

    ![Dialogové okno Publikovat](./media/data-factory-build-your-first-pipeline-using-vs/publish.png)

21. Na stránce Konfigurovat dat factory postupujte takto: 
    1. Vyberte možnost **Vytvořit nový Data Factory** .
    2. Zadejte jedinečný **název** pro data factory. Příklad: **FirstDataFactoryUsingVS09152016**. Název musí být jedinečné.  
    
    
        > [AZURE.IMPORTANT] Pokud se zobrazí chyba **dat factory název "FirstDataFactoryUsingVS" není k dispozici** při publikování, změňte název (například yournameFirstDataFactoryUsingVS). Pojmenování pravidla pro Data Factory artefakty naleznete v tématu [Data Factory - pojmenování pravidla](data-factory-naming-rules.md) .
3. Vyberte správné předplatné pro pole **předplatného** .
     
     
        > [AZURE.IMPORTANT] Pokud nevidíte žádné předplatné, ujistěte se, že jste přihlášeni pomocí účtu, který je správce nebo správce ko předplatného.  
        
    4. Vyberte **pole Skupina zdroje** dat factory vytvořit. 
    5. Vyberte **oblast** dat factory. 
    6. Klikněte na tlačítko **Další** přejděte na stránku **Publikování položek** . (Stisknutím klávesy **TAB** přesunout z pole název, pokud je zakázané na tlačítko **Další** .) 
23. Na stránce **Publikování položek** zajistit, aby všechny zdroje dat entity nedošlo k výběru a klikněte na tlačítko **Další** přejděte na stránku **souhrnu** .     
24. Zkontrolujte souhrnné a klikněte na **Další** zahájení procesu nasazení a zobrazit **Stav nasazení**.
25. Na stránce **Stav nasazení** byste měli vidět stav procesu nasazení. Po dokončení nasazení, klikněte na Dokončit. 

 
Důležitých aspektů, je potřeba pamatovat: 

- Pokud se zobrazí chyba: "**Toto předplatné není registrovaný používat obor názvů Microsoft.DataFactory**", proveďte jednu z následujících akcí a zkuste to znovu publikovat: 

    - V prostředí PowerShell Azure, spusťte tento příkaz k registraci poskytovatele Data Factory. 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        Můžete spusťte tento příkaz potvrdit, že Data Factory registrované poskytovatele. 
    
            Get-AzureRmResourceProvider
    - Přihlášení pomocí Azure předplatné v [Azure portál](https://portal.azure.com) a přejděte na zásuvné Data Factory (nebo) vytvářet factory dat na portálu Azure. Tato akce automaticky zaregistruje poskytovatele za vás.
-   Název factory dat může být registrován jako název DNS v budoucnu a tedy viditelná, bude veřejně.
-   Pokud chcete vytvořit instance Data Factory, musíte být správce nebo správce co Azure předplatného

 
## <a name="monitor-pipeline"></a>Sledování kanálem k odesílání zpráv

### <a name="monitor-pipeline-using-diagram-view"></a>Sledování kanálu v zobrazení diagramu
6. Přihlaste se k [portálu Azure](https://portal.azure.com/), postupujte takto:
    1. Klikněte na **Další služby** a klikněte na **Data továrny**.
        ![Procházet zdroje dat](./media/data-factory-build-your-first-pipeline-using-vs/browse-datafactories.png) 
    2. Vyberte název factory dat (třeba: **FirstDataFactoryUsingVS09152016**) ze seznamu továrny data. 
        ![Vyberte data factory](./media/data-factory-build-your-first-pipeline-using-vs/select-first-data-factory.png)
7. Na domovské stránce výrobce dat klikněte na **Diagram**.
  
    ![Dlaždice diagramu](./media/data-factory-build-your-first-pipeline-using-vs/diagram-tile.png)
7. V zobrazení diagramu najdete v článku Přehled kanály a datové sady použita v tomto kurzu.
    
    ![Zobrazení diagramu](./media/data-factory-build-your-first-pipeline-using-vs/diagram-view-2.png) 
8. Pokud chcete zobrazit všechny aktivity v kanálu, klikněte pravým tlačítkem myši v diagramu kanálem k odesílání zpráv a klikněte na Otevřít kanálem k odesílání zpráv. 

    ![Nabídka Otevřít kanálem k odesílání zpráv](./media/data-factory-build-your-first-pipeline-using-vs/open-pipeline-menu.png)
9. Potvrďte, že se HDInsightHive aktivity v kanálu. 
  
    ![Zobrazení otevřených kanálem k odesílání zpráv](./media/data-factory-build-your-first-pipeline-using-vs/open-pipeline-view.png)

    Pokud chcete přejít zpátky na předchozí zobrazení, klikněte na **Data factory** v nabídce navigace s popisem cesty v horní. 
10. V **Zobrazení diagramu**poklikejte na datovou sadu **AzureBlobInput**. Zkontrolujte, zda řez **připravena** . Může trvat několik minut, než výseč neprojeví připravena. Pokud ho nedojde z toho důvodu po čekáte někdy, přečtěte si téma Pokud máte vstupní soubor (input.log) umístí do pravého kontejneru (adfgetstarted) a složek (inputdata).

    ![Zadávání výseč připravena](./media/data-factory-build-your-first-pipeline-using-vs/input-slice-ready.png)
11. Klikněte na **X** zavřete **AzureBlobInput** zásuvné. 
12. V **Zobrazení diagramu**poklikejte na datovou sadu **AzureBlobOutput**. Můžete vidět, že řez, které aktuálně zpracovávání.

    ![Datové sady](./media/data-factory-build-your-first-pipeline-using-vs/dataset-blade.png)
9. Po dokončení zpracování uvidíte řez **připravena** .

    > [AZURE.IMPORTANT] Vytvoření obrázku HDInsight na vyžádání zabere někdy (přibližně 20 minut). Proto očekávat potrubí zpracování trvat **zhruba 30 minut** výseče.  

    ![Datové sady](./media/data-factory-build-your-first-pipeline-using-vs/dataset-slice-ready.png) 
    
10. Pokud řez je **připravena** , zkontrolujte **partitioneddata** složku v kontejneru **adfgetstarted** v úložišti objektů blob pro výstupní data.  
 
    ![výstupní data](./media/data-factory-build-your-first-pipeline-using-vs/three-ouptut-files.png)
11. Klepněte na výseč zobrazíte její podrobnosti v zásuvné **Výseč** .

    ![Podrobnosti výseč dat](./media/data-factory-build-your-first-pipeline-using-vs/data-slice-details.png)  
12. Klikněte na aktivitu spustit v **seznamu se spustí aktivity** zobrazit podrobné informace o aktivitu spustit (podregistru aktivita v našem případě) v okně **aktivity spustit podrobnosti** .   
    ![Spustit podrobnosti o aktivitách](./media/data-factory-build-your-first-pipeline-using-vs/activity-window-blade.png)  
    
    Ze souboru protokolu uvidíte podregistru dotaz, který byl spuštěn a informací o stavu. Tyto protokoly jsou užitečné při odstraňování všech problémů.  
 

Pokyny pro sledování kanálem k odesílání zpráv a datových sad pomocí portálu Azure, že které jste vytvořili v tomto kurzu najdete v článku [kanálem k odesílání zpráv a sledování datové sady](data-factory-monitor-manage-pipelines.md) .

### <a name="monitor-pipeline-using-monitor--manage-app"></a>Sledování kanálem k odesílání zpráv pomocí sledování a Správa aplikací
Můžete taky použít Monitor a spravovat aplikace pro sledování vaší potrubí. Podrobné informace o použití této aplikace najdete v tématu [sledování a správa Azure Data Factory potrubí pomocí sledování a Správa aplikací](data-factory-monitor-manage-app.md).

1. Klikněte na položku sledování a správa dlaždice.

    ![Sledování a správa vedle sebe](./media/data-factory-build-your-first-pipeline-using-vs/monitor-and-manage-tile.png) 
2. Mají v tématu sledování a spravovat aplikace. Změna **Počáteční čas** a **koncový čas** podle start (04 01 2016 12:00 dop.) a koncový čas (04 02 2016 12:00 dop.) ze svého kanálu a klikněte na **použít**.

    ![Sledování a Správa aplikací](./media/data-factory-build-your-first-pipeline-using-vs/monitor-and-manage-app.png) 
3. Okno aplikace aktivity vyberte v seznamu aktivity Windows zobrazíte její podrobnosti. 
    ![Podrobnosti o aktivitě okna](./media/data-factory-build-your-first-pipeline-using-vs/activity-window-details.png)


> [AZURE.IMPORTANT] Vstupní soubor se odstraní, jakmile řez úspěšně zpracovat. Proto pokud chcete znovu spustit řez nebo znovu provést kurzu, vstupní soubor (input.log) na nahrajte inputdata složek kontejneru adfgetstarted.
 

## <a name="use-server-explorer-to-view-data-factories"></a>Zobrazení dat továrny pomocí Průzkumníka serveru

1. Ve **Visual Studiu**v nabídce klikněte na **zobrazení** a klikněte na položku **Průzkumník serveru**.
2. V okně Průzkumníka Server rozbalte **Azure** a rozbalte položku **Data Factory**. Pokud se zobrazí, **Přihlaste se k aplikaci Visual Studio**, zadejte **účtu** spojeného s předplatným Azure a klikněte na **pokračovat**. Zadejte **heslo**a klikněte na **přihlásit**. Chcete-li získat informace o všech továrny Azure dat ve vašem předplatném se snaží Visual Studio. Zobrazit stav této operace v okně **Data seznamu úkolů Factory** .

    ![Průzkumník serveru](./media/data-factory-build-your-first-pipeline-using-vs/server-explorer.png)
3. Klikněte pravým tlačítkem myši factory dat a vyberte **Exportovat Data Factory nového projektu** vytvořit projekt aplikace Visual Studio založené na existující factory data.

    ![Export dat factory](./media/data-factory-build-your-first-pipeline-using-vs/export-data-factory-menu.png)

## <a name="update-data-factory-tools-for-visual-studio"></a>Aktualizovat Data Factory nástroje for Visual Studio

Aktualizovat Azure Data Factory nástroje for Visual Studio, postupujte takto:

1. V nabídce klikněte na **Nástroje** a vyberte **přípony a aktualizace**.
2. Vyberte **aktualizace** v levém podokně a potom vyberte **Galerii Visual Studio**.
3. Vyberte **Nástroje Azure Data Factory for Visual Studio** a klikněte na **Aktualizovat**. Tento údaj se nezobrazuje, už máte nejnovější verzi nástroje. 

## <a name="use-configuration-files"></a>Používání souborů konfigurace
Konfigurace soubory ve Visual Studiu můžete použít pro nastavení vlastností pro propojené služby/tabulek a potrubí jinak pro každé prostředí. 

Zvažte následující definice JSON služby Azure úložiště propojené. Chcete-li zadat **connectionString** s různé hodnoty název účtu a accountkey podle prostředí (odchylka/Test/výroba), do kterého nasazujete Data Factory entity. Toto chování můžete dosáhnout pomocí samostatných konfiguračního souboru pro každé prostředí. 

    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "description": "",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        }
    } 

### <a name="add-a-configuration-file"></a>Přidání konfiguračního souboru
Přidání konfiguračního souboru pro každé prostředí provedením následujících kroků:   

1. Klikněte pravým tlačítkem Data Factory projekt ve Visual Studiu řešení, přejděte na **Přidat**a klikněte na **Nová položka**.
2. **Konfigurace** vyberte ze seznamu instalovaných šablon na levé straně vyberte **Konfiguračního souboru**, zadejte **název** pro konfiguračního souboru a klikněte na **Přidat**.

    ![Přidání konfiguračního souboru](./media/data-factory-build-your-first-pipeline-using-vs/add-config-file.png)
3. Přidejte parametry konfigurace a jejich hodnot v následujícím formátu.

        {
            "$schema": "http://datafactories.schema.management.azure.com/vsschemas/V1/Microsoft.DataFactory.Config.json",
            "AzureStorageLinkedService1": [
                {
                    "name": "$.properties.typeProperties.connectionString",
                    "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
                }
            ],
            "AzureSqlLinkedService1": [
                {
                    "name": "$.properties.typeProperties.connectionString",
                    "value":  "Server=tcp:spsqlserver.database.windows.net,1433;Database=spsqldb;User ID=spelluru;Password=Sowmya123;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
                }
            ]
        }

    V tomto příkladu konfiguruje vlastnosti connectionString služby Azure úložiště propojený a služby propojené SQL Azure. Všimněte si, že je syntaxe určující název [JsonPath](http://goessner.net/articles/JsonPath/).   

    Pokud JSON má vlastnost, která obsahuje matici hodnot, jak to znázorňuje následující kód:  

        "structure": [
            {
                "name": "FirstName",
                "type": "String"
            },
            {
                "name": "LastName",
                "type": "String"
            }
        ],
    
    Konfigurace vlastností, jak je vidět v následujícím konfiguračního souboru (použití od nuly indexování): 
        
        {
            "name": "$.properties.structure[0].name",
            "value": "FirstName"
        }
        {
            "name": "$.properties.structure[0].type",
            "value": "String"
        }
        {
            "name": "$.properties.structure[1].name",
            "value": "LastName"
        }
        {
            "name": "$.properties.structure[1].type",
            "value": "String"
        }

### <a name="property-names-with-spaces"></a>Názvy vlastností s mezerami
Pokud vlastnost název obsahuje mezery, použijte hranatých závorek, jak je vidět v následujícím příkladu (název databázového serveru): 

     {
         "name": "$.properties.activities[1].typeProperties.webServiceParameters.['Database server name']",
         "value": "MyAsqlServer.database.windows.net"
     }


### <a name="deploy-solution-using-a-configuration"></a>Nasazení řešení pomocí konfigurace
Při publikování Azure Data Factory entity v a můžete určit konfiguraci, která chcete použít pro operaci publikování. 

Pokud chcete publikovat entity v Azure Data Factory projektu pomocí konfiguračního souboru:   

1. Klikněte pravým tlačítkem myši Data Factory projektu a klikněte na **Publikovat** zobrazíte dialogové okno **Publikovat položky** . 
2. Vyberte stávající factory dat nebo zadejte hodnoty pro vytváření factory dat na stránce **Konfigurovat dat factory** a klikněte na tlačítko **Další**.   
3. Na stránce **Publikování položky** : Zobrazí se seznam rozevírací seznam s konfigurací dostupných pro pole **Vyberte konfigurace nasazení** .

    ![Vyberte soubor konfigurace](./media/data-factory-build-your-first-pipeline-using-vs/select-config-file.png)

4. Vyberte **konfigurační soubor** , který chcete použít a klikněte na tlačítko **Další**. 
5. Potvrďte, že jméno JSON souboru na stránce **Souhrn** zobrazeno a klikněte na tlačítko **Další**. 
6. Po dokončení operace nasazení, klikněte na **Dokončit** . 

Při nasazení, hodnoty z konfiguračního souboru slouží k nastavení hodnot v seznamu soubory JSON Data Factory entit před nasazením entity služby Azure Data Factory.   

## <a name="summary"></a>Souhrn 
V tomto kurzu jste vytvořili factory Azure dat k datům proces spuštěním podregistru skriptu clusteru hadoop HDInsight. Editor Factory dat na portálu Azure umožňuje proveďte následující kroky:  

1.  Vytvoření Azure **data factory**.
2.  Vytvoření dvě **propojené služby**:
    1.  Služba **Úložiště azure** propojené propojit obsahující vstupní a výstupní soubory do továrny datový úložiště objektů blob Azure
    2.  Na vyžádání propojené služby **Azure HDInsight** propojíte clusteru HDInsight Hadoop na vyžádání data factory. Azure Data Factory vytvoří HDInsight Hadoop clusteru těsně za běhu zpracování zadávání dat a předložit výstupní data. 
3.  Vytvoří dva **datové sady**, které popisují vstupní a výstupní data pro HDInsight podregistru aktivity v kanálu. 
4.  Vytvořená **kanálem k odesílání zpráv** pomocí aktivitu **Podregistru HDInsight** .  


## <a name="next-steps"></a>Další kroky
V tomto článku vytvořili potrubí aktivitu transformace (HDInsight aktivita), které se spouští skript podregistru clusteru HDInsight na vyžádání. Informace o použití aktivitu na Kopírovat zkopírujte data z objektů Blob Azure Azure SQL, najdete v článku [kurz: kopírování dat z Azure objektů blob Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
  
## <a name="see-also"></a>Viz taky
| Téma | Popis |
| :---- | :---- |
| [Činnost transformace dat](data-factory-data-transformation-activities.md) | Tento článek obsahuje seznam aktivit transformace dat (třeba jste použili v tomto kurzu transformace HDInsight podregistru) nepodporuje Azure Data Factory. | 
| [Plánování a provádění](data-factory-scheduling-and-execution.md) | Tento článek vysvětluje plánování a provádění aspekty model aplikace Azure Data Factory. |
| [Potrubí](data-factory-create-pipelines.md) | Tento článek vám pomůže porozumět kanály a aktivity v Azure Data Factory a jak je lze používat k vytvoření začátku do konce postupů založených na datech pro scénář nebo business. |
| [Datové sady](data-factory-create-datasets.md) | Tento článek vám pomůže pochopit datové sady v Azure Data Factory.
| [Sledování a správa potrubí pomocí sledování aplikace](data-factory-monitor-manage-app.md) | Tento článek popisuje, jak sledovat, Správa a ladění potrubí pomocí příkazu sledování a Správa aplikací. 
