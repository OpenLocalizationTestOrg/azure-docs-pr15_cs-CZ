<properties
    pageTitle="Přesuňte data z místních SQL Server SQL Azure pomocí Azure Data Factory | Azure"
    description="Nastavte si ADF kanálu, který vytvoří dva činnosti migrace dat, které dohromady přesuňte data z denně mezi databázemi místních i cloudových."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="bradsev" />


# <a name="move-data-from-an-on-premise-sql-server-to-sql-azure-with-azure-data-factory"></a>Přesunutí dat z místních SQL serveru k SQL Azure s Azure Data Factory

Toto téma ukazuje, jak chcete přesunout data z databáze místních SQL serveru k databázi SQL Azure pomocí úložiště objektů Blob Azure pomocí Factory dat Azure (ADF).

Následující **nabídky** odkazy na témata, která popisují, jak jedí data do cílové prostředí, kde můžete data uložená a zpracování během procesu týmu dat pro výzkum.

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]


## <a name="intro"></a>Úvod: Co je ADF a kdy ji bude použito k migraci dat?

Azure Data Factory je služba integrace plně spravovaná cloudové data, která orchestrates a automaticky pohybu a transformace dat. Klíčové koncept v modelu ADF je kanálem k odesílání zpráv. Potrubí je logické seskupení činností, z nichž každá definuje akce provádět dat obsažených v datové sady. Propojené služby slouží k definování informace potřebné pro Data Factory připojení ke zdroji dat.

S ADF můžete existující služby pro zpracování dat složený do datové kanály, které jsou vysoce k dispozici a spravovaný v cloudu. Tyto datové kanály můžou být naplánované jedí Příprava, transformace, analyzovat a publikování dat a ADF spravuje a orchestrates komplexních dat a zpracování závislosti. Řešení můžete rychle integrované a používaný v cloudu, připojení rostoucí počet místních i cloudových dat zdrojů.

Zvažte použití ADF:

- data potřebujete-li k neustále poštovním ve scénáři hybridní, který má přístup k místním i cloudové zdroje 
- Pokud data je zpracováván jako transakce nebo musí změnit nebo máte obchodní logiky do něj přidali při migrací. 

ADF umožňuje plánování a sledování úlohy pomocí jednoduché skripty JSON Správa pohybu dat v pravidelných intervalech. ADF má i další funkce, například podpora složité operace. Další informace o ADF najdete v dokumentaci v [Azure dat Factory (ADF)](https://azure.microsoft.com/services/data-factory/).


## <a name="scenario"></a>Scénáře

Nastavení ADF kanálem k odesílání zpráv, které vytvoří dvě aktivity migraci dat. Společně se data přesunout denně mezi databáze SQL místních a databáze SQL Azure v cloudu. Jsou dva aktivity:

* kopírování dat z databáze SQL serveru místní k úložišti objektů Blob Azure účtu.
* kopírování dat z účtu úložiště objektů Blob Azure k databázi SQL Azure.

>[AZURE.NOTE] Kroky zobrazené tady byly upraveny z podrobnější kurzu poskytnutých týmem ADF: odkazů do příslušných částí Toto téma [Přesun dat mezi zdrojů na pracovišti a cloud se Brána pro správu dat](../data-factory/data-factory-move-data-between-onprem-and-cloud.md) jsou k dispozici v případě potřeby.


## <a name="prereqs"></a>Zjistit předpoklady pro
Tento kurz se předpokládá, že máte:

* **Azure předplatného**. Pokud nemáte předplatné, můžete zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).
* **Účet Azure úložiště**. Používáte účet Azure úložiště pro ukládání data v tomto kurzu. Pokud nemáte účet Azure úložiště, podívejte se na článek [Vytvoření účtu úložiště](storage-create-storage-account.md#create-a-storage-account) . Po vytvoření účtu úložiště, musíte získat klíč účtu použitých při přístupu k úložišti. V tématu [zobrazení, kopírovat a přístupových kláves z verze regenerovat úložiště](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
* Přístup k **databázi Azure SQL**. Pokud musíte nastavit databáze SQL Azure, tpoic [Otevřenu databázi Microsoft Azure SQL](../sql-database/sql-database-get-started.md) obsahuje informace o tom, jak vytvořit nové instance databáze SQL Azure.
* Nainstalovanou a nakonfigurovanou **Azure PowerShell** místně. Pokyny najdete v tématu [instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md).

> [AZURE.NOTE] Tento postup používá [Azure portálu](https://portal.azure.com/).


##<a name="upload-data"></a>Odeslání dat do místní SQL serveru

Budeme používat [datovou sadu NYC Taxi](http://chriswhong.com/open-data/foil_nyc_taxi/) k předvedli migračního procesu. Datovou sadu NYC Taxi neexistuje, jak je uvedeno v tomto příspěvku na Azure objektů blob úložiště [NYC Taxi Data](http://www.andresmh.com/nyctaxitrips/). Data má dva soubory, trip_data.csv soubor, který obsahuje údaje ze služební cesty, a trip_far.csv soubor, který obsahuje podrobnosti o tarif zaplacený při každé ze služební cesty. Výběr a popis tyto soubory jsou uvedeny v [NYC Taxi cest datovou sadu popis](machine-learning-data-science-process-sql-walkthrough.md#dataset).


Můžete upravit postup pro nastavení vlastních datech, které tady najdete nebo postupujte podle pokynů pomocí NYC Taxi datovou sadu. K nahrání datovou sadu NYC Taxi do vaší místní databáze SQL serveru, podle pokynů uvedených v [Hromadné Import dat do databáze systému SQL Server](machine-learning-data-science-process-sql-walkthrough.md#dbload). Tyto pokyny jsou určené pro systém SQL Server na Azure virtuálního počítače, ale postup pro odeslání do místní SQL Server je stejný.


##<a name="create-adf"></a>Vytvoření Azure Data Factory

Pokyny k vytvoření nové Factory dat Azure nebo skupina zdroje [Azure portál](https://portal.azure.com/) jsou uvedeny [vytvořit Azure Data Factory](../data-factory/data-factory-build-your-first-pipeline-using-editor.md#step-1-creating-the-data-factory). Název nové instance ADF *adfdsp* a název skupiny vytvořili zdroje *adfdsprg*.


## <a name="install-and-configure-up-the-data-management-gateway"></a>Nainstalujte a nakonfigurujte nahoru Brána pro správu dat

Povolit kanály v Azure dat factory pro práci s místním SQL serveru, musíte přidat jako propojené služby do továrny data. Chcete-li vytvořit propojený služby pro systém SQL Server na pracovišti, postupujte takto:

- Stáhněte a nainstalujte Microsoft Brána pro správu dat na místním počítači. 
- Konfigurovat službu propojené pro místní zdroj dat použít brány. 

Brána pro správu dat jsou a deserializuje zdroje a jímky dat v počítači, kde je hostovaný.

Pokyny k nastavení a informace o bráně pro správu dat najdete v tématu [přesouvání dat mezi zdrojů na pracovišti a cloud se Brána pro správu dat](../data-factory/data-factory-move-data-between-onprem-and-cloud.md)


## <a name="adflinkedservices"></a>Vytvoření propojené služby připojení ke zdroji dat

Propojené služby definuje informace potřebné pro Azure Data Factory pro připojení ke zdroji dat. Podrobný postup pro vytvoření propojených služby je k dispozici v [vytváření propojených služeb](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#step-2-create-linked-services).

V tomto scénáři, u kterého jsou propojené služby potřebné máme tři zdroje.

1. [Propojené služby pro místní systém SQL Server](#adf-linked-service-onprem-sql)
2. [Propojené služby pro úložišti objektů Blob Azure](#adf-linked-service-blob-store)
3. [Propojené služby pro databáze Azure SQL](#adf-linked-service-azure-sql)


###<a name="adf-linked-service-onprem-sql"></a>Propojené služby pro místní databázi SQL serveru

Vytvoření propojené služby pro systém SQL Server na pracovišti:

- Klikněte na **Úložiště dat** v ADF cílovou stránku na klasické portálu Azure 
- Vyberte **SQL** a zadejte přihlašovací údaje *uživatelské jméno* a *heslo* pro místní systém SQL Server. Budete muset zadat název serveru jako **úplný název instance webu název serveru zpětné lomítko (servername\instancename)**. Název propojené služby *adfonpremsql*.

###<a name="adf-linked-service-blob-store"></a>Propojené služby pro objektů Blob

Vytvoření propojené služba úložiště objektů Blob Azure účtu:

- Klikněte na **Úložiště dat** v ADF cílovou stránku na klasické portálu Azure
- Vyberte **Účet Azure úložiště** 
- Zadejte název účtu úložiště objektů Blob Azure spolu s kontejner. Název propojené služby *adfds*.

###<a name="adf-linked-service-azure-sql"></a>Propojené služby pro databáze Azure SQL

Vytvoření propojené služby pro databázi SQL Azure:

- Klikněte na **Úložiště dat** v ADF cílovou stránku na klasické portálu Azure
- Vyberte **Azure SQL** a zadejte přihlašovací údaje *uživatelské jméno* a *heslo* pro databázi SQL Azure. *Uživatelské jméno* musí být zadán jako *user@servername*.   


##<a name="adf-tables"></a>Definování tabulek a jejich vytváření můžete určit, jak získat přístup ke datové sady

Vytvoření tabulky, které určují strukturu, umístění a dostupnosti datové sady s tohoto skriptu. Soubory JSON se používají k definování tabulek. Další informace o struktuře tyto soubory najdete v tématu [datové sady](../data-factory/data-factory-create-datasets.md).

> [AZURE.NOTE]  Můžete spustit `Add-AzureAccount` rutina před spuštěním rutinu [New-AzureDataFactoryTable](https://msdn.microsoft.com/library/azure/dn835096.aspx) ověřte, že je zaškrtnuté vpravo Azure předplatné pro spuštění příkazu. Dokumentace tuto rutinu najdete v článku [Přidání AzureAccount](https://msdn.microsoft.com/library/azure/dn790372.aspx).

Definice na základě JSON v tabulkách používat tyto názvy:

* *nyctaxi_data* je v poli **název tabulky** v místním SQL serveru
* **jméno container** v úložišti objektů Blob Azure účet je *název_kontejneru*  

Tři definice tabulky jsou potřebné pro tento ADF kanálem k odesílání zpráv:

1. [Místní tabulka serveru SQL](#adf-table-onprem-sql)
2. [Kulatý tabulky](#adf-table-blob-store)
3. [SQL Azure Table](#adf-table-azure-sql)

> [AZURE.NOTE]  Postup pomocí prostředí PowerShell Azure definovat a vytvořit ADF aktivity. Ale tyto úkoly lze provést také pomocí portálu Azure. Podrobnosti najdete v tématu [Vytvoření vstupní a výstupní datové sady](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#step-3-create-input-and-output-datasets).

###<a name="adf-table-onprem-sql"></a>Místní tabulka serveru SQL

Definice tabulky pro systém SQL Server na místní není zadán do údajem soubor JSON:

        {
            "name": "OnPremSQLTable",
            "properties":
            {
                "location":
                {
                "type": "OnPremisesSqlServerTableLocation",
                "tableName": "nyctaxi_data",
                "linkedServiceName": "adfonpremsql"
                },
                "availability":
                {
                "frequency": "Day",
                "interval": 1,   
                "waitOnExternal":
                {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
                }

                }
            }
        }

Názvy sloupců vydané není tady. Můžete zvolit podřízeného na názvy sloupců s využitím je zde (pro kontrolu informace v tématu [si přečtěte následující dokumentaci ADF](../data-factory/data-factory-data-movement-activities.md ) .

Zkopírovat definici JSON tabulky do souboru s názvem souboru *onpremtabledef.json* a uložte známé umístění (tady dosadí *C:\temp\onpremtabledef.json*). Vytvoření tabulky v ADF pomocí následující rutiny prostředí PowerShell Azure:

    New-AzureDataFactoryTable -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp –File C:\temp\onpremtabledef.json


###<a name="adf-table-blob-store"></a>Kulatý tabulky
Definice tabulky pro umístění výstupu objektů blob je v následujících (mapy požití dat z místních objektů blob Azure):

        {
            "name": "OutputBlobTable",
            "properties":
            {
                "location":
                {
                "type": "AzureBlobLocation",
                "folderPath": "containername",
                "format":
                {
                "type": "TextFormat",
                "columnDelimiter": "\t"
                },
                "linkedServiceName": "adfds"
                },
                "availability":
                {
                "frequency": "Day",
                "interval": 1
                }
            }
        }

Zkopírovat definici JSON tabulky do souboru s názvem souboru *bloboutputtabledef.json* a uložte známé umístění (tady dosadí *C:\temp\bloboutputtabledef.json*). Vytvoření tabulky v ADF pomocí následující rutiny prostředí PowerShell Azure:

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\bloboutputtabledef.json  

###<a name="adf-table-azure-sq"></a>SQL Azure Table
Definice tabulky databáze SQL Azure výstup v následujícím (toto schéma mapy dat pocházejících z objektů blob):

    {
        "name": "OutputSQLAzureTable",
        "properties":
        {
            "structure":
            [
                { "name": "column1", type": "String"},
                { "name": "column2", type": "String"}                
            ],
            "location":
            {
                "type": "AzureSqlTableLocation",
                "tableName": "your_db_name",
                "linkedServiceName": "adfdssqlazure_linked_servicename"
            },
            "availability":
            {
                "frequency": "Day",
                "interval": 1            
            }
        }
    }

Zkopírovat definici JSON tabulky do souboru s názvem souboru *AzureSqlTable.json* a uložte známé umístění (tady dosadí *C:\temp\AzureSqlTable.json*). Vytvoření tabulky v ADF pomocí následující rutiny prostředí PowerShell Azure:

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\AzureSqlTable.json  


##<a name="adf-pipeline"></a>Definování a vytvoření kanálu

Určení činnosti, které patří kanálu a vytvoření kanálu pomocí tohoto skriptu. Soubor JSON slouží k definování vlastnosti kanálem k odesílání zpráv.

* Skript předpokládá, že **příležitostí název** *AMLDSProcessPipeline*.
* Navíc nezapomeňte, že nastavení četnost kanálu provádět denně a používat výchozí čas spuštění úlohy (UTC 12 dop.).

> [AZURE.NOTE]Následující postupy pomocí prostředí PowerShell Azure definovat a vytvoření kanálu ADF. Ale tento úkol lze provést také pomocí portálu Azure. Podrobnosti najdete v tématu [Vytvoření a spuštění kanálů](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#step-4-create-and-run-a-pipeline).

Použití tabulky definice dříve, definici kanálem k odesílání zpráv pro ADF zadán následujícím způsobem:

        {
            "name": "AMLDSProcessPipeline",
            "properties":
            {
                "description" : "This pipeline has one Copy activity that copies data from an on-premise SQL to Azure blob",
                 "activities":
                [
                    {
                        "name": "CopyFromSQLtoBlob",
                        "description": "Copy data from on-premise SQL server to blob",     
                        "type": "CopyActivity",
                        "inputs": [ {"name": "OnPremSQLTable"} ],
                        "outputs": [ {"name": "OutputBlobTable"} ],
                        "transformation":
                        {
                            "source":
                            {                               
                                "type": "SqlSource",
                                "sqlReaderQuery": "select * from nyctaxi_data"
                            },
                            "sink":
                            {
                                "type": "BlobSink"
                            }   
                        },
                        "Policy":
                        {
                            "concurrency": 3,
                            "executionPriorityOrder": "NewestFirst",
                            "style": "StartOfInterval",
                            "retry": 0,
                            "timeout": "01:00:00"
                        }       

                     },

                    {
                        "name": "CopyFromBlobtoSQLAzure",
                        "description": "Push data to Sql Azure",        
                        "type": "CopyActivity",
                        "inputs": [ {"name": "OutputBlobTable"} ],
                        "outputs": [ {"name": "OutputSQLAzureTable"} ],
                        "transformation":
                        {
                            "source":
                            {                               
                                "type": "BlobSource"
                            },
                            "sink":
                            {
                                "type": "SqlSink",
                                "WriteBatchTimeout": "00:5:00",             
                            }           
                        },
                        "Policy":
                        {
                            "concurrency": 3,
                            "executionPriorityOrder": "NewestFirst",
                            "style": "StartOfInterval",
                            "retry": 2,
                            "timeout": "02:00:00"
                        }
                     }
                ]
            }
        }

Zkopírujte tato JSON definice kanálu do souboru s názvem souboru *pipelinedef.json* a uložte známé umístění (tady dosadí *C:\temp\pipelinedef.json*). Vytvoření kanálu v ADF pomocí následující rutiny prostředí PowerShell Azure:

    New-AzureDataFactoryPipeline  -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\pipelinedef.json

Potvrďte, že se kanálem k odesílání zpráv na ADF klasické portálu Azure zobrazit jako následující (po klepnutí na tlačítko diagram)

![](media/machine-learning-data-science-move-sql-azure-adf/DJP1kji.png)


##<a name="adf-pipeline-start"></a>Zahájení kanálu
Kanálu lze spustit teď pomocí následujícího příkazu:

    Set-AzureDataFactoryPipelineActivePeriod -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp -StartDateTime startdateZ –EndDateTime enddateZ –Name AMLDSProcessPipeline

Hodnoty parametrů *startdate* a *enddate* muset nahrazuje skutečná kalendářní data, mezi kterými se má kanálu spustit.

Jakmile kanálu spustí, by měl být k dispozici dat zobrazí v kontejneru vybraná objektů blob jeden soubor denně.

Poznámka: můžeme nebyly využít funkce poskytované ADF datového kanálu postupně. Další informace o tom, jak se to a další možnosti poskytovanou ADF najdete v [dokumentaci ADF](https://azure.microsoft.com/services/data-factory/).
