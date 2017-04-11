<properties 
    pageTitle="Přesunutí dat – Brána pro správu dat | Microsoft Azure"
    description="Nastavení brány dat přesun dat mezi místním prostředím a cloudu. Přesunutí dat pomocí Brána pro správu dat v Azure Data Factory." 
    keywords="Brána dat, integrace dat, přesunutí dat, pověření brány"
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="jingwang"/>

# <a name="move-data-between-on-premises-sources-and-the-cloud-with-data-management-gateway"></a>Přesouvání dat mezi zdrojů na pracovišti a cloud se Brána pro správu dat
Tento článek obsahuje přehled integrace dat mezi místní datový úložiště a cloudu dat pomocí Data Factory. Vytvoří na článek [Aktivity přesun dat](data-factory-data-movement-activities.md) a dalších dat factory základní koncepty článků: [datové sady](data-factory-create-datasets.md) a [potrubí](data-factory-create-pipelines.md). 

## <a name="data-management-gateway"></a>Brána pro správu dat
Brána pro správu dat je nutné nainstalovat na počítači místní Povolit přesunutí dat z úložiště místní data. Brány můžete nainstalovat na stejném počítači jako úložiště dat nebo do jiného počítače, dokud brány se můžete připojit k úložišti. 

> [AZURE.IMPORTANT] Viz článek [Brána pro správu dat](data-factory-data-management-gateway.md) podrobnosti o bráně pro správu dat.   

Následující návodu se dozvíte, jak vytvořit factory dat s kanálu, který slouží k přesunutí dat z databáze **SQL serveru** místní k úložišti objektů blob Azure. Jako součást návodu nainstalujte a nakonfigurujte bránu pro správu dat ve vašem počítači. 

## <a name="walkthrough-copy-on-premises-data-to-cloud"></a>Návod: zkopírování místních dat do cloudu
  
## <a name="create-data-factory"></a>Vytvoření factory dat
V tomto kroku použijete Azure portálu pro vytvoření instance Azure Data Factory s názvem **ADFTutorialOnPremDF**. 

1.  Přihlaste se k [portálu Azure](https://portal.azure.com). 
2.  Klikněte na **+ Nový**, klikněte na **Intelligence + technologie pro analýzu**a klikněte na **Data Factory**.

    ![Nové -> DataFactory](./media/data-factory-move-data-between-onprem-and-cloud/NewDataFactoryMenu.png)  
2. **Nová data factory** zásuvné vyplňte **ADFTutorialOnPremDF** název.

    ![Přidání Startboard](./media/data-factory-move-data-between-onprem-and-cloud/OnPremNewDataFactoryAddToStartboard.png)

    > [AZURE.IMPORTANT] 
    > Název factory Azure data musí být jedinečné. Pokud se zobrazí chyba: **název factory dat "ADFTutorialOnPremDF" není k dispozici**, změňte název factory data (například yournameADFTutorialOnPremDF) a zkuste znovu vytvořit. Použijte tento název místo ADFTutorialOnPremDF při provedením zbývajících kroků v tomto kurzu.
    > 
    > Název factory dat může být registrován jako název **DNS** v budoucnu a tedy viditelná, bude veřejně.
3. Vyberte místo, kam chcete data factory vytvořit **Azure předplatného** . 
4.  Vyberte existující **pole Skupina zdroje** nebo vytvořte novou skupinu zdroje. Kurz, vytvořte zdroje skupinu nazvanou: **ADFTutorialResourceGroup**. 
5.  Klikněte na **vytvořit** na zásuvné **Nová data factory** .

    > [AZURE.IMPORTANT] Pokud chcete vytvořit instance Data Factory, musí být členem role [Přispěvatele Factory dat](../active-directory/role-based-access-built-in-roles.md/#data-factory-contributor) na úrovni skupiny předplatné nebo zdroje. 
11. Po dokončení vytváření uvidíte zásuvné **Data Factory** , jak je znázorněno na následujícím obrázku:

    ![Data Factory domovskou stránku](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDataFactoryHomePage.png)

## <a name="create-gateway"></a>Vytvoření brány
5. V zásuvné **Data Factory** , klikněte na **Autor a nasazení** dlaždici a spusťte **Editor** pro data factory.

    ![Vytváření a nasazení vedle sebe](./media/data-factory-move-data-between-onprem-and-cloud/author-deploy-tile.png) 
6.  V editoru Factory dat klikněte na **... Další** na panelu nástrojů a potom klikněte na **Nová brána data**. Můžete taky kliknete pravým tlačítkem **Bran dat** ve stromovém zobrazení a klikněte na **Nová brána data**. 

    ![Nová brána dat na panelu nástrojů](./media/data-factory-move-data-between-onprem-and-cloud/NewDataGateway.png)
2. V zásuvné **vytvořit** **adftutorialgateway** zadejte **název**a klikněte na **OK**.    

    ![Vytvoření brány zásuvné](./media/data-factory-move-data-between-onprem-and-cloud/OnPremCreateGatewayBlade.png)
3. V zásuvné **Konfigurovat** klikněte na **instalovat přímo na tomto počítači**. Tato akce stáhne instalační balíček brány, instalace, nakonfiguruje a registruje brány na počítači.  

    > [AZURE.NOTE] 
    > Pomocí aplikace Internet Explorer nebo Microsoft ClickOnce kompatibilní webového prohlížeče.
    > 
    > Pokud používáte Chrome, přejděte do [Chrome webového úložiště](https://chrome.google.com/webstore/)vyhledávání pomocí klíčových slov "ClickOnce", zvolte jednu z rozšíření ClickOnce a nainstalujte ji. 
    >  
    > Stejně postupujte i Firefox (nainstalovat doplněk). Klikněte na tlačítko **Otevřít nabídku** na panelu nástrojů (**tři vodorovných čar** v pravém horním rohu), klikněte na **Doplňky**, vyhledávání pomocí klíčových slov "ClickOnce", zvolte jednu z ClickOnce přípony a nainstalujte ji.    

    ![Brána – konfigurace zásuvné](./media/data-factory-move-data-between-onprem-and-cloud/OnPremGatewayConfigureBlade.png)

    Tímto způsobem, je nejjednodušší (jedním kliknutím) stáhnout, nainstalovat, nakonfigurovat a zaregistrovat bránu v jednom kroku jednoho. Uvidíte, že je v počítači nainstalována aplikace **Správce konfigurace brány pro správu dat Microsoft** . Spustitelný **ConfigManager.exe** můžete zjistit taky ve složce: **C:\Program Files\Microsoft dat správu Gateway\2.0\Shared**.

    Můžete taky stažení a instalace brány ručně pomocí odkazů v tomto zásuvné a zaregistrovat pomocí klíč zobrazený v textovém poli **Nový klíč** .
    
    Viz článek [Brána pro správu dat](data-factory-data-management-gateway.md) pro všechny podrobnosti o brány.

    >[AZURE.NOTE] Musíte být správcem v místním počítači instalace a konfigurace brány pro správu dat úspěšně. Místní skupiny Windows **Uživatelé Brána pro správu dat** můžete přidat další uživatele. Členové této skupiny můžete použít nástroj Správce konfigurace brány pro správu dat konfigurace brány. 

5. Počkejte několik minut a počkejte, až se zobrazí následující zpráva oznámení:

    ![Instalace brány proběhla úspěšně.](./media/data-factory-move-data-between-onprem-and-cloud/gateway-install-success.png) 
6. Spusťte **Správce konfigurace brány pro správu dat** aplikace ve vašem počítači. V okně **hledání** zadejte **Brána pro správu dat** pro přístup k tento nástroj. Spustitelný **ConfigManager.exe** můžete zjistit taky ve složce: **C:\Program Files\Microsoft dat správu Gateway\2.0\Shared** 

    ![Správce konfigurace brány](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDMGConfigurationManager.png)
6. Potvrďte, že se `adftutorialgateway is connected to the cloud service` zprávy. Stavový řádek dolní zobrazí **připojit ke službě cloud** spolu s **zelenou značku zaškrtnutí**.

    Na kartě **Domů** můžete taky udělat tyto operace: 
    - **Registraci** brány klíčem z portálu Microsoft Azure pomocí tlačítka přihlásit. 
    - **Ukončení** služba hostitele brány pro správu dat spuštěná na počítači brány. 
    - **Plán aktualizace** instalaci v daném čase dne. 
    - Zobrazení, pokud byla brány **poslední aktualizace**.
    - Zadejte čas niž aktualizaci brány je možné nainstalovat. 

8. Přejděte na kartu **Nastavení** . Certifikát uvedený v části **certifikát** slouží k šifrování/dešifrování přihlašovací údaje pro místní úložiště dat, které zadáte na portálu. (volitelné) Klikněte na **změnit** a použijte vlastní certifikát. Ve výchozím nastavení používá bránu certifikát, který je automaticky generované službou Data Factory.

    ![Konfigurace certifikátu brány](./media/data-factory-move-data-between-onprem-and-cloud/gateway-certificate.png)

    Můžete taky udělat následující akce na kartě **Nastavení** : 
    - Zobrazení nebo exportujte certifikát používán brány.
    - Změna koncový bod HTTPS brána.    
    - Nastavení HTTP proxy serveru pro použití v brány.   
9. (volitelné) Přejděte na kartu **diagnostiky** , pokud chcete povolit podrobné protokolování, které slouží k řešení jakýchkoli problémů s bránou zaškrtněte možnost **Povolit protokolování podrobností** . Informace zaznamenané do protokolu najdete v **Prohlížeč událostí** v části **protokoly aplikací a služeb** -> **Brána pro správu dat** uzel. 

    ![Karta Diagnostika](./media/data-factory-move-data-between-onprem-and-cloud/diagnostics-tab.png)

    Na kartě **diagnostiky** můžete taky provádět následující akce: 
    
    - Pomocí oddílu **Testovat připojení** ke zdroji dat místní pomocí brány.
    - Klikněte na **Zobrazení protokolů** zobrazíte protokol Brána pro správu dat v okně Prohlížeč událostí. 
    - Klikněte na **Odeslat protokoly** k nahrání souboru zip s protokoly posledních sedmi dnů Microsoftu kvůli usnadnění vaší problémů. 
10. Na kartě **Diagnostika** v části **Test připojení** vyberte **SQL Server** pro druh úložiště dat, zadejte název databázového serveru, název databáze, zadejte typ ověřování, zadejte uživatelské jméno a heslo a klikněte na možnost **Test** k ověření, zda brány můžete připojit k databázi. 
11. Přepněte do webového prohlížeče a **Azure portál**, na zásuvné **Konfigurovat** a potom na zásuvné **Nová brána dat** , klikněte na tlačítko **OK** .
6. Měli byste vidět **adftutorialgateway** v části **Bran dat** ve stromovém zobrazení na levé straně.  Pokud kliknete, byste měli vidět přidružené JSON. 
    

## <a name="create-linked-services"></a>Vytvoření propojené služby 
V tomto kroku vytvoříte dvě propojené služby: **AzureStorageLinkedService** a **SqlServerLinkedService**. **SqlServerLinkedService** propojí databáze SQL serveru místní a odkazy propojené služby **AzureStorageLinkedService** objektů blob Azure ukládat data factory. Vytvoření kanálu dál v tomto návodu, který slouží ke kopírování dat z místní databázi SQL serveru k úložišti objektů blob Azure. 

#### <a name="add-a-linked-service-to-an-on-premises-sql-server-database"></a>Přidání propojeného služby k databázi serveru SQL Server místní
1.  V **Editoru Factory dat**na panelu nástrojů klikněte na **Uložit nová data** a vyberte **SQL serveru**. 

    ![Nové služby SQL Server propojené](./media/data-factory-move-data-between-onprem-and-cloud/NewSQLServer.png) 
3.  V **editoru JSON** na pravé straně proveďte následující kroky: 
    1. Pro **názevbrány**zadejte **adftutorialgateway**. 
    2. V **connectionString**proveďte následující kroky: 
        1. **Webu název serveru**zadejte název serveru, který je hostitelem databáze SQL serveru.
        2. **Název databáze**zadejte název databáze.
        3. Na panelu nástrojů na tlačítko **šifrování** . Stažení a spustí aplikace přihlašovací údaje správce.
        
            ![Přihlašovací údaje správce aplikace](./media/data-factory-move-data-between-onprem-and-cloud/credentials-manager-application.png)
        5. V dialogovém okně **Nastavení přihlašovacích údajů** zadejte typ ověřování, uživatelské jméno a heslo a klikněte na **OK**. Pokud se připojení podaří, šifrované přihlašovací údaje uložené v ve formátu JSON a zavření dialogového okna. 
        6. Zavřete kartu prázdné prohlížeče, který spustí dialogového okna, pokud není automaticky zavřený a vrátit se na kartu pomocí portálu Azure. 
  
            Na počítači brány tyto údaje jsou **šifrovány** pomocí certifikát, který vlastní službu Data Factory. Pokud chcete použít certifikát, který souvisí s Brána pro správu dat místo toho, najdete v článku [Nastavení přihlašovacích údajů bezpečné](#set-credentials-and-security).    
    1.  Na panelu s příkazy pro nasazení služby SQL Server propojené klikněte na **nasazení** . Měli byste vidět službu propojené ve stromovém zobrazení. 
        
        ![Služba SQL Server propojené ve stromovém zobrazení](./media/data-factory-move-data-between-onprem-and-cloud/sql-linked-service-in-tree-view.png)  

#### <a name="add-a-linked-service-for-an-azure-storage-account"></a>Přidání propojeného služby pro účet Azure úložiště
 
1. V **Editoru Factory dat**klikněte na **Nová data uložit** na panelu příkazů a klikněte na **Azure úložiště**.
2. Zadejte název účtu úložiště Azure **název účtu**.
3. Zadejte klíč pro váš účet Azure úložiště **klíč účtu**.
4. Klikněte na **Deploy** nasazení **AzureStorageLinkedService**.
   
 
## <a name="create-datasets"></a>Vytvoření datové sady
V tomto kroku vytvoříte vstupní a výstupní datové sady, které představují vstupní a výstupní data pro kopírování (databáze SQL serveru místní = > úložišti objektů blob Azure). Před vytvořením datové sady, postupujte následujícím způsobem (podrobný postup následuje seznam):

- Vytvořte v tabulce s názvem **emp** v databázi SQL serveru jste přidali jako propojené služba factory dat a vložení několika položek vzorku do tabulky.
- Vytvoření kontejneru objektů blob s názvem **adftutorial** v účtu úložiště objektů blob Azure, kterou jste přidali jako propojené služba data factory.

### <a name="prepare-on-premises-sql-server-for-the-tutorial"></a>Příprava na kurz místního serveru SQL

1. V databázi, kterou jste zadali pro systém SQL Server místní propojené služby (**SqlServerLinkedService**), použijte tento skript SQL k vytvoření **emp** tabulky v databázi.

        CREATE TABLE dbo.emp
        (
            ID int IDENTITY(1,1) NOT NULL, 
            FirstName varchar(50),
            LastName varchar(50),
            CONSTRAINT PK_emp PRIMARY KEY (ID)
        )
        GO 
2. Vložení ukázkami do tabulky: 

        INSERT INTO emp VALUES ('John', 'Doe')
        INSERT INTO emp VALUES ('Jane', 'Doe')

### <a name="create-input-dataset"></a>Vytvoření sady zadávání dat

1. V **Editoru Factory dat**které můžete klikněte ** Další**, klikněte na **nové datové sady** na panelu příkazů a klikněte na **Tabulka serveru SQL Server**. 
2.  Nahraďte JSON v pravém podokně následující text:
        
            {       
                "name": "EmpOnPremSQLTable",
                "properties": {
                    "type": "SqlServerTable",
                    "linkedServiceName": "SqlServerLinkedService",
                    "typeProperties": {
                        "tableName": "emp"
                    },
                    "external": true,
                    "availability": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "policy": {
                        "externalData": {
                            "retryInterval": "00:01:00",
                            "retryTimeout": "00:10:00",
                            "maximumRetry": 3
                        }
                    }
                }
            }    

    Mějte na paměti následující skutečnosti: 

    - **Typ** je nastavený na **SqlServerTable**.
    - **název tabulky** je nastavena na **emp**.
    - **linkedServiceName** nastavenou **SqlServerLinkedService** (jste vytvořili tuto službu propojené dříve v tomto návodu.).
    - Pro zadávání sadu generovaný není jiný kanálem k odesílání zpráv v Azure Data Factory musíte nastavit **externí** **logickou hodnotu PRAVDA**. Označuje, že je vyrobeno vstupních dat pro službu Azure Data Factory. Volitelně můžete zadat všechny zásady externích dat pomocí elementu **externalData** v části **zásady** .    

    Další informace o vlastnostech JSON najdete v článku [přesunutí dat z SQL serveru](data-factory-sqlserver-connector.md) .
2. Na panelu s příkazy pro nasazení datovou sadu, klikněte na **nasazení** .  


### <a name="create-output-dataset"></a>Vytvoření datovou sadu výstup

1.  V **Editoru Factory dat**na panelu s příkazy klikněte na **novou sadu** a klikněte na **úložiště objektů Blob Azure**.
2.  Nahraďte JSON v pravém podokně následující text: 

            {
                "name": "OutputBlobTable",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "AzureStorageLinkedService",
                    "typeProperties": {
                        "folderPath": "adftutorial/outfromonpremdf",
                        "format": {
                            "type": "TextFormat",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Hour",
                        "interval": 1
                    }
                }
            }
  
    Mějte na paměti následující skutečnosti: 
    
    - **Typ** je nastavený na **AzureBlob**.
    - **linkedServiceName** nastavenou **AzureStorageLinkedService** (jste vytvořili tuto službu propojené v kroku 2).
    - **cesta_ke_složce** nastavenou **adftutorial/outfromonpremdf** kde outfromonpremdf je složka v kontejneru adftutorial. Vytvoření kontejneru **adftutorial** : Pokud ještě neexistuje. 
    - **Dostupnost** nastavenou **hodinové** (**frekvence** nastavte na **hodiny** a **interval** nastavený na hodnotu **1**).  Data Factory generuje tato služba řez dat výstupu každou hodinu v tabulce **emp** v databázi SQL Azure. 

    Pokud nezadáte **název souboru** pro **výstupní tabulky**, soubory v **cesta_ke_složce** jsou uvedeny v v tomto formátu: Data. <Guid>txt (například:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).

    Pokud chcete nastavit **cesta_ke_složce** a **název souboru** dynamicky na základě **SliceStart** času, použijte vlastnost partitionedBy. V následujícím příkladu cesta_ke_složce používá rok, měsíc a den z SliceStart (počáteční čas dané výseč zpracovávání) a název souboru používá hodinu od SliceStart. Například, pokud je adresami vytvořené řez 2014-10 – 20T08:00:00, název_složky nastavena na wikidatagateway/wikisampledataout/2014/10/20 a název souboru je nastavený na 08.csv. 

        "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
        "fileName": "{Hour}.csv",
        "partitionedBy": 
        [
            { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
            { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
            { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
            { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
        ],

 
    Další informace o vlastnostech JSON najdete v článku [přesunutí dat z úložiště objektů Blob Azure](data-factory-azure-blob-connector.md) .
2.  Na panelu s příkazy pro nasazení datovou sadu, klikněte na **nasazení** . Potvrďte, že se obě datové sady ve stromovém zobrazení.  

## <a name="create-pipeline"></a>Vytvoření kanálu
V tomto kroku vytvoříte pomocí jedné **Kopie aktivity** , který používá **EmpOnPremSQLTable** předávat na vstupu **kanálem k odesílání zpráv** a **OutputBlobTable** jako výstup.

1.  V editoru Factory dat klikněte na **... Další**a klikněte na **Nový kanál**. 
2.  Nahraďte JSON v pravém podokně následující text: 
    
            {
                "name": "ADFTutorialPipelineOnPrem",
                "properties": {
                "description": "This pipeline has one Copy activity that copies data from an on-prem SQL to Azure blob",
                "activities": [
                {
                    "name": "CopyFromSQLtoBlob",
                    "description": "Copy data from on-prem SQL server to blob",
                    "type": "Copy",
                    "inputs": [
                    {
                        "name": "EmpOnPremSQLTable"
                    }
                    ],
                    "outputs": [
                    {
                        "name": "OutputBlobTable"
                      }
                    ],
                    "typeProperties": {
                      "source": {
                        "type": "SqlSource",
                        "sqlReaderQuery": "select * from emp"
                      },
                      "sink": {
                        "type": "BlobSink"
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
                "start": "2016-07-05T00:00:00Z",
                "end": "2016-07-06T00:00:00Z",
                "isPaused": false
              }
            }

    > [AZURE.IMPORTANT]
    > Nahraďte hodnotu vlastnost **zahájení** aktuální den a **end** hodnotu pomocí následujícího dne.

    Mějte na paměti následující skutečnosti:
 
    - V části aktivity je pouze aktivity, jehož **Typ** je nastavený na **Kopírovat**.
    - **Vstupní** aktivity je nastavený na **EmpOnPremSQLTable** a **výstup** aktivity je nastavený na **OutputBlobTable**.
    - V části **typeProperties** **SqlSource** zadaný jako **Typ zdroje** a **BlobSink **zadaný jako **Typ jímky**.
    - Dotaz SQL zobrazený `select * from emp` je určený pro vlastnost **sqlReaderQuery** **SqlSource**.

     Jak spustit a koncového data a času musí být ve [formátu ISO](http://en.wikipedia.org/wiki/ISO_8601). Příklad: 2014-10 – 14T16:32:41Z. Čas **ukončení** vynechán, ale doporučujeme používat v tomto kurzu. 
    
    Pokud nezadáte hodnoty pro vlastnost **Ukončit** , se vypočítá jako "**zahájení + 48 hodin**". Jako hodnotu pro vlastnost **Ukončit** spustíte kanálu donekonečna udržovat zadejte **9/9/9999** . 
    
    Definování doba trvání, ve kterém jsou zpracovány výsečí data podle **dostupnosti** vlastnosti, které byly definované pro každý sady dat Azure Data Factory.
    
    V tomto příkladu je 24 výsečí dat každý výseč pochází každou hodinu.     
2. Na panelu s příkazy pro nasazení datovou sadu klikněte na **Deploy** (obdélníkový datovou sadu je tabulka). Potvrďte, že se kanálu objeví ve stromovém zobrazení uzlu **potrubí** .  
5. Teď klikněte na **X** dvakrát zavřete listy vrátit k zásuvné **Data Factory** pro **ADFTutorialOnPremDF**.

**Blahopřejeme!** Úspěšně jste vytvořili Azure dat factory, propojené služby, datových sad a potrubí a naplánované kanálu.

#### <a name="view-the-data-factory-in-a-diagram-view"></a>Zobrazení factory dat v zobrazení diagramu 
1. V **Azure portál**klikněte na dlaždici **diagramu** na domovskou stránku factory dat **ADFTutorialOnPremDF** . :

    ![Diagram odkaz](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramLink.png)

2. Měli byste vidět diagram vidíte na následujícím obrázku:

    ![Zobrazení diagramu](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramView.png)

    Přiblížit, oddálit, můžete změnit velikost na 100 %, Lupa na přizpůsobit automaticky umístěte kanály a datové sady a zobrazit informace o souvisejících zdrojích (zvýrazní směrem nahoru a dolů položek z vybraných položek).  Poklepejte na objekt (datovou sadu vstupní a výstupní nebo profilace) můžete zobrazit vlastnosti. 

## <a name="monitor-pipeline"></a>Sledování kanálem k odesílání zpráv
V tomto kroku používat portál Azure ke sledování, co se děje v Azure dat factory. Můžete taky pomocí rutin prostředí PowerShell sledování datové sady a potrubí. Podrobnosti o sledování najdete v tématu [sledování a správa potrubí](data-factory-monitor-manage-pipelines.md).

5. V diagramu poklikejte **EmpOnPremSQLTable**.  

    ![EmpOnPremSQLTable výsečí](./media/data-factory-move-data-between-onprem-and-cloud/OnPremSQLTableSlicesBlade.png)

6. Všimněte si, že všechna data řezy nahoru jsou **připravena** protože doby trvání kanálem k odesílání zpráv (počáteční čas pro konec) v minulosti. Je taky protože vložíte data v databázi SQL serveru a tam je vždy. Potvrďte, že žádný výsečí objeví v části **problém výsečí** v dolní. Všechny výseče zobrazíte kliknutím na **Zobrazit další** v dolní části seznamu výsečí. 
7. Pak v **datové sady** zásuvné, klikněte na **OutputBlobTable**.

    ![OputputBlobTable výsečí](./media/data-factory-move-data-between-onprem-and-cloud/OutputBlobTableSlicesBlade.png)
9. Klikněte na jakoukoli výseč dat ze seznamu a byste měli vidět zásuvné **Výseč** . Vidíte aktivity dlouhodobě spuštěná výseče. Zobrazí pouze jeden aktivity obvykle spustit.  

    ![Zásuvné výseč dat](./media/data-factory-move-data-between-onprem-and-cloud/DataSlice.png)

    Jestliže řez nejsou **připravena** , uvidíte nadřazeného řezy, které nejsou připravená a blokují aktuální řez spuštění v seznamu **, které nejste připravení nadřazeného výsečí** .

10. Klikněte na tlačítko **Spustit aktivity** ze seznamu v dolní části zjistit, jestli **aktivity podrobnosti**.

    ![Zásuvné spustit podrobnosti o aktivitách](./media/data-factory-move-data-between-onprem-and-cloud/ActivityRunDetailsBlade.png)

    Uvidí informace, například výkon, dobu trvání a Brána pro přenos dat použít. 
11. Klikněte na **X** zavřete všechny listy dokud 
12. Návrat k domácí zásuvné pro **ADFTutorialOnPremDF**.
14. (volitelné) Klikněte na tlačítko **kanály**klikněte **ADFTutorialOnPremDF**a k podrobnostem vstupních tabulek (**spotřebováno**) nebo výstup datových sad (**z**).
15. Pomocí nástrojů, jako jsou pomocí nástrojů například [Úložiště Průzkumníka](http://storageexplorer.com/) ověřit, že objektů blob/souboru je pro každou hodinu.

    ![Průzkumník Azure úložiště](./media/data-factory-move-data-between-onprem-and-cloud/OnPremAzureStorageExplorer.png)

## <a name="next-steps"></a>Další kroky

- Viz článek [Brána pro správu dat](data-factory-data-management-gateway.md) pro všechny podrobnosti o bráně pro správu dat.
- V tématu [kopírování dat z objektů Blob Azure SQL Azure](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) získat informace o tom, jak kopírovat činnost slouží k přesunutí dat z úložiště zdroje dat do úložiště jímky dat. 
