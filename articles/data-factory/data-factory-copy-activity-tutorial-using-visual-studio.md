<properties 
    pageTitle="Kurz: Vytváření kanálů se kopie aktivity pomocí aplikace Visual Studio | Microsoft Azure" 
    description="V tomto kurzu vytvoříte Azure Data Factory profilace aktivitu kopírovat pomocí aplikace Visual Studio." 
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
    ms.topic="get-started-article" 
    ms.date="10/17/2016" 
    ms.author="spelluru"/>

# <a name="tutorial-create-a-pipeline-with-copy-activity-using-visual-studio"></a>Kurz: Vytváření kanálů se kopie aktivity pomocí aplikace Visual Studio
> [AZURE.SELECTOR]
- [Přehled a požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Kopírování Průvodce](data-factory-copy-data-wizard-tutorial.md)
- [Azure portálu](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [Prostředí PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Azure správce prostředků šablony](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [ROZHRANÍ REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [ROZHRANÍ API .NET](data-factory-copy-activity-tutorial-using-dotnet-api.md)


Tento kurz se dozvíte, jak vytvořit a sledovat factory Azure dat pomocí aplikace Visual Studio. Kanálu v factory dat slouží ke kopírování dat z úložiště objektů Blob Azure k databázi SQL Azure aktivitu na Kopírovat.

Tady je postup, které provádíte jako součást tohoto kurzu:

1. Vytvořte dvě propojené služby: **AzureStorageLinkedService1** a **AzureSqlinkedService1**. 

    Odkazy AzureStorageLinkedService1 Azure úložiště a AzureSqlLinkedService1 propojuje databáze Azure SQL factory dat: **ADFTutorialDataFactoryVS**. Zadávání dat profilace jsou uložená v kontejneru objektů blob v úložišti objektů blob Azure a výstupní data se uloží do tabulky v databázi Azure SQL. Proto přidejte tyto dvě datová ukládá jako propojené služby data factory.
2. Vytvoření dvou datových sad: **InputDataset** a **OutputDataset**, které představují vstupní a výstupní data uložená v úložišti data. 

    U InputDataset zadáte kontejneru objektů blob obsahující objektů blob se zdrojovými daty. U OutputDataset zadáte v tabulce SQL, která jsou uložená data výstupu. Můžete taky určit jiné vlastnosti například strukturu, dostupnost a zásady.
3. Vytvoření kanálu s názvem **ADFTutorialPipeline** v ADFTutorialDataFactoryVS. 

    Kanálu má **Kopírovat aktivity** , které se kopie pro zadávání dat z Azure objektů blob Azure SQL tabulky výstup. Aktivity kopírovat provede přesun dat v Azure Data Factory. Aktivity používá technologii globálně dostupná služba, která můžete přesouvat data mezi různých úložiště dat zabezpečené spolehlivý a scalable způsobem. Článek [Aktivity přesun dat](data-factory-data-movement-activities.md) najdete v článku podrobné informace o aktivitě kopírovat. 
4. Vytvoření factory dat s názvem **VSTutorialFactory**. Nasazení factory dat a všechny Data Factory entity (propojené služeb, tabulky a potrubí).    

## <a name="prerequisites"></a>Zjistit předpoklady pro

1. Prostudujte si článek [Přehled](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) a postupujte podle pokynů **předpoklad** . 
2. Abyste mohli publikovat Data Factory entity Azure Data Factory musí být **Správce Azure předplatného** .  
3. Musí mít na počítači nainstalovaný následující: 
    - Visual Studio 2013 nebo Visual Studio 2015
    - Stáhněte si Azure SDK Visual Studio 2013 nebo Visual Studio 2015. Přejděte na [Stránku Stáhnout Azure](https://azure.microsoft.com/downloads/) a klikněte na **a 2013** nebo **2015 a** v části **.NET** .
    - Stáhněte si nejnovější modul plug-in Azure Data Factory for Visual Studio: [2013 a](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) nebo [a 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Modul plug-in je také možné aktualizovat provedením následujících kroků: V nabídce klikněte na **Nástroje** -> **rozšíření a aktualizace** -> **Online** -> **Galerie Visual Studio** -> **Microsoft Azure datové Factory nástroje for Visual Studio** -> **Aktualizovat**.

## <a name="create-visual-studio-project"></a>Vytvoření projektu Visual Studio 
1. Spuštění **aplikace Visual Studio 2013**. Klikněte na **soubor**, přejděte na **Nový**a klikněte na **projekt**. Zobrazí dialogové okno **Nový projekt** .  
2. V dialogovém okně **Nový projekt** vyberte šablonu **DataFactory** a klikněte na **Prázdný projekt Factory Data**. Pokud nevidíte šabloně DataFactory, zavřete Visual Studiu, instalace Azure SDK for Visual Studio 2013 a otevřete Visual Studio.  

    ![Dialogové okno Nový projekt](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-project-dialog.png)

3. Zadejte **název** projektu, **umístění**a název **řešení**a klikněte na **OK**.

    ![Průzkumník řešení](./media/data-factory-copy-activity-tutorial-using-visual-studio/solution-explorer.png) 

## <a name="create-linked-services"></a>Vytvoření propojené služby
Propojené služby propojení úložiště dat nebo výpočet služby Azure dat factory. Najdete v článku [podporované datové ukládá](data-factory-data-movement-activities.md##supported-data-stores-and-formats) pro všechny zdroje a propadů nepodporuje aktivity kopírovat. Najdete v článku [Výpočet propojené služby](data-factory-compute-linked-services.md) pro seznam služby výpočetním podporované Data Factory. V tomto kurzu nepoužíváte jiné výpočetní služby. 

V tomto kroku vytvoříte dvě propojené služby: **AzureStorageLinkedService1** a **AzureSqlLinkedService1**. AzureStorageLinkedService1 propojené odkazy služby účet Azure úložiště a AzureSqlLinkedService propojí databáze Azure SQL factory dat: **ADFTutorialDataFactory**. 

### <a name="create-the-azure-storage-linked-service"></a>Vytvoření službu Azure úložiště propojené

4. Klikněte pravým tlačítkem myši **Propojené služby** v okně Průzkumník, přejděte na **Přidat**a klikněte na **Nová položka**.      
5. V dialogovém okně **Přidat novou položku** ze seznamu vyberte **Propojené služba Azure úložiště** a klikněte na **Přidat**. 

    ![Nové propojené služby](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-linked-service-dialog.png)
 
3. Nahrazení `<accountname>` a `<accountkey>`* s názvem účet Azure úložiště a klíče. 

    ![Propojené Azure úložiště služby](./media/data-factory-copy-activity-tutorial-using-visual-studio/azure-storage-linked-service.png)

4. Uložte soubor **AzureStorageLinkedService1.json** .

> Další informace o vlastnostech JSON najdete v článku [přesunutí dat z/objektů Blob Azure](data-factory-azure-blob-connector.md#azure-storage-linked-service) .

### <a name="create-the-azure-sql-linked-service"></a>Vytvoření službu propojené SQL Azure

5. Klikněte pravým tlačítkem myši na uzel **Propojené služby** v **Okně Průzkumník** znovu, přejděte na **Přidat**a klikněte na **Nová položka**. 
6. Tentokrát, vyberte **Propojené služby SQL Azure**a klikněte na **Přidat**. 
7. V **souboru AzureSqlLinkedService1.json**nahraďte `<servername>`, `<databasename>`, `<username@servername>`, a `<password>` s názvy Azure SQL serveru, databázi, uživatelský účet, a heslo.    
8.  Uložte soubor **AzureSqlLinkedService1.json** . 

> [AZURE.NOTE]
> Další informace o vlastnostech JSON najdete v článku [přesunutí dat z/k databázi SQL Azure](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) .

## <a name="create-datasets"></a>Vytvoření datové sady
V předchozím kroku, vytvořeny propojené služeb **AzureStorageLinkedService1** a **AzureSqlLinkedService1** propojit účet Azure úložiště a databáze Azure SQL factory dat: **ADFTutorialDataFactory**. V tomto kroku definujete dvě datové sady – **InputDataset** a **OutputDataset** – představující vstupní a výstupní data uložená v úložišti dat odkazuje AzureStorageLinkedService1 a AzureSqlLinkedService1. U InputDataset zadáte kontejneru objektů blob obsahující objektů blob se zdrojovými daty. U OutputDataset zadáte v tabulce SQL, která jsou uložená data výstupu.

### <a name="create-input-dataset"></a>Vytvoření sady zadávání dat
V tomto kroku vytvoříte datovou sadu s názvem **InputDataset** odkazující na kontejner objektů blob Azure úložiště představované službu **AzureStorageLinkedService1** propojené. Tabulky je obdélníkový datovou sadu a typ pouze datovou sadu teď podporuje. 

9. Klikněte pravým tlačítkem myši **tabulky** v **Okně Průzkumník**, přejděte na **Přidat**a klikněte na **Nová položka**.
10. V dialogovém okně **Přidat novou položku** vyberte **Objektů Blob Azure**a klikněte na **Přidat**.   
10. Nahrazení textu JSON tímto textem a **AzureBlobLocation1.json** soubor uložte. 

        {
          "name": "InputDataset",
          "properties": {
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
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService1",
            "typeProperties": {
              "folderPath": "adftutorial/",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "external": true,
            "availability": {
              "frequency": "Hour",
              "interval": 1
            }
          }
        }

     Mějte na paměti následující skutečnosti: 
    
    - datovou sadu **Typ** je nastavený na **AzureBlob**.
    - **linkedServiceName** nastavenou **AzureStorageLinkedService**. Tuto službu propojené jste vytvořili v kroku 2.
    - **cesta_ke_složce** nastavenou kontejneru **adftutorial** . Můžete také zadat název objektů blob ve složce pomocí vlastnosti **název souboru** . Protože nejsou zadáním názvu objektů blob, data ze všech objektů BLOB v kontejneru považovány za vstupní data.  
    - Formát **Typ** je nastavený na **Formát textu**
    - Zobrazí se dvě pole v textovém souboru – **jméno** a **Příjmení** – odděleni znak čárky (**columnDelimiter**) 
    - **Dostupnost** je nastavený na **každou hodinu** (**četnosti** je nastavený na **hodiny** a **interval** je nastavena na hodnotu **1**). Proto Data Factory vyhledá zadávání dat každou hodinu v kořenové složce kontejneru objektů blob (**adftutorial**) jste zadali. 
    
    Pokud nezadáte **název souboru** pro **zadávání** datovou sadu, všechny soubory nebo objekty BLOB ve složce vstupní (**cesta_ke_složce**) jsou považovány za zadávání. Pokud zadáte název souboru v ve formátu JSON, jenom určité soubor/objektů blob je považován za asn vstupní.
 
    Pokud nezadáte **název souboru** pro **výstupní tabulky**, soubory v **cesta_ke_složce** jsou uvedeny v v tomto formátu: Data. &lt;Guid\&gt;. TXT (Příklad: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).

    Pokud chcete nastavit **cesta_ke_složce** a **název souboru** dynamicky na základě **SliceStart** času, použijte vlastnost **partitionedBy** . V následujícím příkladu cesta_ke_složce používá rok, měsíc a den z SliceStart (počáteční čas dané výseč zpracovávání) a název souboru používá hodinu od SliceStart. Například, pokud je adresami vytvořené řez 2016-09-20T08:00:00, název_složky nastavena na wikidatagateway/wikisampledataout/2016/09/20 a název souboru je nastavený na 08.csv. 

            "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
            "fileName": "{Hour}.csv",
            "partitionedBy": 
            [
                { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
                { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
                { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
                { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 

> [AZURE.NOTE]
> Další informace o vlastnostech JSON najdete v článku [přesunutí dat z/objektů Blob Azure](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) .

### <a name="create-output-dataset"></a>Vytvoření datovou sadu výstup
V tomto kroku vytvoříte výstup datovou sadu s názvem **OutputDataset**. Tento datovou sadu odkazy na tabulky SQL databáze Azure SQL představované **AzureSqlLinkedService1**. 

11. Klikněte pravým tlačítkem na **tabulky** v **Okně Průzkumník** znovu, přejděte na **Přidat**a klikněte na **Nová položka**.
12. V dialogovém okně **Přidat novou položku** vyberte **Azure SQL**a klikněte na **Přidat**. 
13. Nahraďte JSON text následující JSON a **AzureSqlTableLocation1.json** soubor uložte.

        {
          "name": "OutputDataset",
          "properties": {
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
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService1",
            "typeProperties": {
              "tableName": "emp"
            },
            "availability": {
              "frequency": "Hour",
              "interval": 1
            }
          }
        }

     Mějte na paměti následující skutečnosti: 
    
    - datovou sadu **Typ** je nastavený na **AzureSQLTable**.
    - **linkedServiceName** nastavenou **AzureSqlLinkedService** (jste vytvořili tuto službu propojené v kroku 2).
    - **název tabulky** je nastavena na **emp**.
    - Existují tři sloupce – **ID**, **jméno**a **Příjmení** – v tabulce emp v databázi. ID je sloupec identity, takže budete muset zadat jenom **jméno** a **Příjmení** tady.
    - **Dostupnost** nastavenou **hodinové** (**frekvence** nastavte na **hodiny** a **interval** nastavený na hodnotu **1**).  Data Factory generuje tato služba výseč dat výstupu každou hodinu v tabulce **emp** v databázi Azure SQL.

> [AZURE.NOTE]
> Další informace o vlastnostech JSON najdete v článku [přesunutí dat z/k databázi SQL Azure](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) .

## <a name="create-pipeline"></a>Vytvoření kanálu 
Vytvoření propojené služby vstupní a výstupní a tabulky, pokud. Dále vytvořte potrubí s **Kopírovat aktivity** ke kopírování dat z Azure objektů blob k databázi Azure SQL. 


1. Klikněte pravým tlačítkem myši **potrubí** v **Průzkumníku řešení**, přejděte na **Přidat**a klikněte na **Nová položka**.  
15. Vyberte **Kopírovat Data kanálem k odesílání zpráv** v dialogovém okně **Přidat novou položku** a klikněte na **Přidat**. 
16. Nahraďte ve formátu JSON následující JSON a **CopyActivity1.json** soubor uložte.
            
        {
          "name": "ADFTutorialPipeline",
          "properties": {
            "description": "Copy data from a blob to Azure SQL table",
            "activities": [
              {
                "name": "CopyFromBlobToSQL",
                "type": "Copy",
                "inputs": [
                  {
                    "name": "InputDataset"
                  }
                ],
                "outputs": [
                  {
                    "name": "OutputDataset"
                  }
                ],
                "typeProperties": {
                  "source": {
                    "type": "BlobSource"
                  },
                  "sink": {
                    "type": "SqlSink",
                    "writeBatchSize": 10000,
                    "writeBatchTimeout": "60:00:00"
                  }
                },
                "Policy": {
                  "concurrency": 1,
                  "executionPriorityOrder": "NewestFirst",
                  "style": "StartOfInterval",
                  "retry": 0,
                  "timeout": "01:00:00"
                }
              }
            ],
            "start": "2015-07-12T00:00:00Z",
            "end": "2015-07-13T00:00:00Z",
            "isPaused": false
          }
        }

    Mějte na paměti následující skutečnosti:

    - V části aktivity je pouze jeden aktivity, jehož **Typ** je nastavený na **Kopírovat**.
    - Vstupní aktivity je nastavený na **InputDataset** a výstup aktivity je nastavený na **OutputDataset**.
    - V části **typeProperties** **BlobSource** zadaný jako typ zdroje a **SqlSink** není zadán psaní jímky.

    Nahraďte hodnotu vlastnost **zahájení** aktuální den a **end** hodnotu pomocí následujícího dne. Můžete zadat jenom část data a přeskočit časovou část datum čas. Například "2016-02-03", která je ekvivalentní "2016 – 02-03T00:00:00Z"
    
    Jak spustit a koncového data a času musí být ve [formátu ISO](http://en.wikipedia.org/wiki/ISO_8601). Příklad: 2016-10 – 14T16:32:41Z. Čas **ukončení** vynechán, ale doporučujeme používat v tomto kurzu. 
    
    Pokud nezadáte hodnoty pro vlastnost **Ukončit** , se vypočítá jako "**zahájení + 48 hodin**". Jako hodnotu pro vlastnost **Ukončit** spustíte kanálu donekonečna udržovat zadejte **9999-09-09** .
    
    V předchozím příkladu je 24 výsečí dat každý výseč pochází každou hodinu.

## <a name="publishdeploy-data-factory-entities"></a>Publikování a nasazení Entity Data Factory
V tomto kroku publikovat Data Factory entity (propojené služby datové sady a kanálem k odesílání zpráv) jste dříve vytvořili. Můžete také zadat název nové factory dat vytvořit pro uložení tyto entity.  

18. Klikněte pravým tlačítkem na projektu v Průzkumníku řešení a klikněte na **Publikovat**. 
19. Pokud se zobrazí dialogové okno **přihlásit ke svému účtu Microsoft** , zadejte svoje přihlašovací údaje pro účet, který má Azure předplatné a klikněte na **přihlásit**.
20. Měli byste vidět dialogovým oknem takto:

    ![Dialogové okno Publikovat](./media/data-factory-copy-activity-tutorial-using-visual-studio/publish.png)
21. Na stránce Konfigurovat dat factory proveďte následující kroky: 
    1. Vyberte možnost **Vytvořit nový Data Factory** .
    2. Zadejte **název** **VSTutorialFactory** .  
    
        > [AZURE.IMPORTANT]  
        > Název factory Azure data musí být jedinečné. Pokud se zobrazí chyba související názvem factory dat při publikování, změňte název factory data (například yournameVSTutorialFactory) a zkuste znovu publikovat. Pojmenování pravidla pro Data Factory artefakty naleznete v tématu [Data Factory - pojmenování pravidla](data-factory-naming-rules.md) .     
    3 vyberte předplatné Azure pole **předplatného** .
     
        > [AZURE.IMPORTANT]Pokud nevidíte žádné předplatné, ujistěte se, že jste přihlášeni pomocí účtu, který je správce nebo správce co předplatného.  
    4. Vyberte **pole Skupina zdroje** dat factory vytvořit. 5. Vyberte **oblast** dat factory. V rozevíracím seznamu se zobrazí jenom oblasti podporované službu Data Factory.
6. Klikněte na tlačítko **Další** přejděte na stránku **Publikování položek** .
    
        ![Konfigurace stránku factory dat.](media/data-factory-copy-activity-tutorial-using-visual-studio/configure-data-factory-page.png)   
23. Na stránce **Publikování položek** zajistit, aby všechny zdroje dat entity nedošlo k výběru a klikněte na tlačítko **Další** přejděte na stránku **souhrnu** .
    
    ![Publikovat stránku položek](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-items-page.png)     
24. Zkontrolujte souhrnné a klikněte na **Další** zahájení procesu nasazení a zobrazit **Stav nasazení**.

    ![Publikovat stránku souhrnu](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-summary-page.png)
25. Na stránce **Stav nasazení** byste měli vidět stav procesu nasazení. Po dokončení nasazení, klikněte na Dokončit. 
    ![Stránka stav nasazení](media/data-factory-copy-activity-tutorial-using-visual-studio/deployment-status.png) Poznámka: následující skutečnosti: 

- Pokud se zobrazí chyba: "**Toto předplatné není registrovaný používat obor názvů Microsoft.DataFactory**", proveďte jednu z následujících akcí a zkuste to znovu publikovat: 

    - V prostředí PowerShell Azure, spusťte tento příkaz k registraci poskytovatele Data Factory. 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        Spuštěním následujícího příkazu potvrdit, že Data Factory registrované poskytovatele. 
    
            Get-AzureRmResourceProvider
    - Přihlášení pomocí Azure předplatného na [portálu Azure](https://portal.azure.com) a přejděte na zásuvné Data Factory (nebo) vytvářet factory dat na portálu Azure. Tato akce automaticky zaregistruje poskytovatele za vás.
-   Název factory dat může být registrován jako název DNS v budoucnu a tedy viditelná, bude veřejně.

> [AZURE.IMPORTANT] Pokud chcete vytvořit instance Data Factory, musíte být správce/co správcem Azure předplatného

## <a name="summary"></a>Souhrn
V tomto kurzu jste vytvořili factory Azure dat ke kopírování dat z Azure objektů blob k databázi Azure SQL. Visual Studio slouží k vytváření factory dat, propojené služby, datových sad a potrubí. Tady jsou hlavních kroků, které jste provedli v tomto kurzu:  

1.  Vytvoření Azure **data factory**.
2.  Vytvoření **propojené služby**:
    1. Služba **Azure úložiště** propojené propojit váš účet Azure úložiště obsahující vstupní data.    
    2. Služba **Azure SQL** propojené k propojení databáze Azure SQL, která obsahuje výstupní data. 
3.  Vytvoření **datové sady**, které popisují vstupních dat a výstupní data pro kanály.
4.  Vytvoření **kanálem k odesílání zpráv** s **Kopírovat aktivity** s **BlobSource** jako zdroj a **SqlSink** jako jímky. 


## <a name="use-server-explorer-to-view-data-factories"></a>Zobrazení dat továrny pomocí Průzkumníka serveru

1. Ve **Visual Studiu**v nabídce klikněte na **zobrazení** a klikněte na položku **Průzkumník serveru**.
2. V okně Průzkumníka Server rozbalte **Azure** a rozbalte položku **Data Factory**. Pokud se zobrazí, **Přihlaste se k aplikaci Visual Studio**, zadejte **účtu** spojeného s předplatným Azure a klikněte na **pokračovat**. Zadejte **heslo**a klikněte na **přihlásit**. Získat informace o všech továrny Azure dat ve vašem předplatném Visual Studio se snaží. Zobrazit stav této operace v okně **Data seznamu úkolů Factory** .
    ![Průzkumník serveru](./media/data-factory-copy-activity-tutorial-using-visual-studio/server-explorer.png)
3. Klikněte pravým tlačítkem myši na factory dat a vyberte Exportovat Data Factory nového projektu vytvořit projekt aplikace Visual Studio založené na existující factory data.
    ![Export dat factory SESTAVTE projekt](./media/data-factory-copy-activity-tutorial-using-visual-studio/export-data-factory-menu.png)  

## <a name="update-data-factory-tools-for-visual-studio"></a>Aktualizovat Data Factory nástroje for Visual Studio
Aktualizovat Azure Data Factory nástroje for Visual Studio, proveďte následující kroky:

1. V nabídce klikněte na **Nástroje** a vyberte **rozšíření a aktualizace**. 
2. Vyberte **aktualizace** v levém podokně a potom vyberte **Galerii Visual Studio**.
4. Vyberte **Nástroje Azure Data Factory for Visual Studio** a klikněte na **Aktualizovat**. Pokud nevidíte tuto položku, máte už nejnovější verzi nástroje. 

Pokyny, jak používat portál Azure sledování kanálem k odesílání zpráv a datové sady, že které jste vytvořili v tomto kurzu najdete v článku [datové sady Monitor a profilace](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline) .

## <a name="see-also"></a>Viz taky
| Téma | Popis |
| :---- | :---- |
| [Činnost pohyb dat](data-factory-data-movement-activities.md) | Tento článek obsahuje podrobné informace o aktivity kopie použité v tomto kurzu. |
| [Plánování a provádění](data-factory-scheduling-and-execution.md) | Tento článek vysvětluje plánování a provádění aspekty model aplikace Azure Data Factory. |
| [Potrubí](data-factory-create-pipelines.md) | Tento článek vám pomůže porozumět kanály a aktivity v Azure Data Factory |
| [Datové sady](data-factory-create-datasets.md) | Tento článek vám pomůže pochopit datové sady v Azure Data Factory.
| [Sledování a správa potrubí pomocí sledování aplikace](data-factory-monitor-manage-app.md) | Tento článek popisuje, jak sledovat, Správa a ladění potrubí pomocí příkazu sledování a Správa aplikací. 
