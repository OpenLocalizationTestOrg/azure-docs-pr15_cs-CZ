<properties 
    pageTitle="SQL Server uložené procedury aktivity" 
    description="Zjistěte, jak můžete na SQL Server uložené procedury činnost vyvolat proceduru uloženou v databázi SQL Azure nebo Azure SQL datový sklad od Data Factory kanálu." 
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
    ms.topic="article" 
    ms.date="09/30/2016" 
    ms.author="spelluru"/>

# <a name="sql-server-stored-procedure-activity"></a>SQL Server uložené procedury aktivity
> [AZURE.SELECTOR]
[Podregistru](data-factory-hive-activity.md)  
[Prasátko](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Přenos Hadoop](data-factory-hadoop-streaming-activity.md)
[Počítače výukové](data-factory-azure-ml-batch-execution-activity.md) 
[Uložená procedura](data-factory-stored-proc-activity.md)
[Analýzy jezera dat U-SQL](data-factory-usql-activity.md)
[.NET vlastní](data-factory-use-custom-activities.md)

SQL Server uložená procedura aktivity v Data Factory [kanálem k odesílání zpráv](data-factory-create-pipelines.md) můžete vyvolat proceduru uloženou v některém z následujících úložištích dat: 


- Databáze Azure SQL 
- Datový sklad Azure SQL  
- Databáze systému SQL Server ve vašem podniku nebo OM Azure. Budete potřebovat k instalaci Brána pro správu dat na stejném počítači, který je hostitelem databáze nebo samostatné počítače, chcete-li předejít soutěží pro zdroje s databází. Brána pro správu dat je software, který se připojuje místního zdroje dat zdrojů a dat hosed v Azure VMs ke cloudovým službám zabezpečené a spravovaných způsobem. Přečtěte si téma [Přesun dat mezi místním prostředím a cloudu](data-factory-move-data-between-onprem-and-cloud.md) článek podrobnosti o bráně pro správu dat. 

Tento článek je založena na článek [aktivity transformace dat](data-factory-data-transformation-activities.md) , který představuje obecné základní informace o transformaci dat a aktivity podporované transformace.

## <a name="walkthrough"></a>Návod

### <a name="sample-table-and-stored-procedure"></a>Ukázková tabulka a uložená procedura
1. Vytvořte následující **tabulky** v databázi SQL Azure pomocí SQL Server Management Studio nebo jiný nástroj budete spokojeni. Ve sloupci datetimestamp jsou datum a čas, kdy je generováno odpovídající ID. 

        CREATE TABLE dbo.sampletable
        (
            Id uniqueidentifier,
            datetimestamp nvarchar(127)
        )
        GO

        CREATE CLUSTERED INDEX ClusteredID ON dbo.sampletable(Id);
        GO

    Sloupec datetimestamp je datum a čas, kdy je generováno odpovídající ID ID je jedinečný identifikovat.
    ![Ukázková data](./media/data-factory-stored-proc-activity/sample-data.png)

    > [AZURE.NOTE] Tento příklad používá databáze SQL Azure, ale funguje stejným způsobem technologie Azure SQL datový sklad a databáze systému SQL Server. 
2. Vytvořte následující **uložené procedury** , vloží data v **sampletable**.

        CREATE PROCEDURE sp_sample @DateTime nvarchar(127)
        AS
        
        BEGIN
            INSERT INTO [sampletable]
            VALUES (newid(), @DateTime)
        END

    > [AZURE.IMPORTANT] **Název** a **velikost písmen** parametru (DateTime v tomto příkladu) musí odpovídat parametr zadaný v kanálem k odesílání zpráv a aktivity JSON. Při definici uložená procedura zajistit, aby **@** , se použije jako předpony pro parametr.
    
### <a name="create-a-data-factory"></a>Vytvoření factory dat  
4. Přihlaste se k [portálu Azure](https://portal.azure.com/). 
5. Klikněte na tlačítko **Nový** v nabídce nalevo klikněte **Intelligence + technologie pro analýzu**a klikněte na **Data Factory**.
    
    ![Nová data factory](media/data-factory-stored-proc-activity/new-data-factory.png)   
4.  **Nová data factory** zásuvné vyplňte **SProcDF** název. Azure Data Factory názvy jsou **jedinečné**. Je třeba název factory dat s názvem vaší povolit úspěšné vytvoření továrny předpony.

    ![Nová data factory](media/data-factory-stored-proc-activity/new-data-factory-blade.png)      
3.  Vyberte **Azure předplatného**. 
4.  **Pole Skupina zdroje**proveďte jednu z následujících kroků: 
    1.  Klikněte na **vytvořit nový** a zadejte název skupiny zdrojů.
    2.  Klikněte na **použít existující** a vyberte existující skupiny zdrojů.  
5.  Vyberte **umístění** pro data factory.
6.  Vyberte **Připnout na řídicí panel** tak, aby bylo vidět factory dat na řídicím panelu při příštím přihlášení. 
6.  Klikněte na **vytvořit** na zásuvné **Nová data factory** .
6.  Zobrazí data factory vzniku v **řídicím panelu** portálu Azure. Po úspěšném vytvoření factory dat, uvidíte stránce factory dat, který zobrazuje obsah data factory.
    ![Data Factory domovskou stránku](media/data-factory-stored-proc-activity/data-factory-home-page.png)

### <a name="create-an-azure-sql-linked-service"></a>Vytvoření služby propojené SQL Azure  
Po vytvoření factory dat, vytvořit služby propojené SQL Azure, který propojuje databázi SQL Azure data factory. Tato databáze obsahuje sampletable tabulky a sp_sample uložené procedury.

7.  Klikněte na **Autor a nasazení** na zásuvné **Data Factory** pro **SProcDF** ke spuštění editoru Factory Data.
2.  Klikněte na **Nová data uložit** na panelu s příkazy a zvolte **Databáze SQL Azure**. Měli byste vidět JSON skriptu pro vytváření služeb SQL Azure propojené v editoru. 

    ![Nové úložiště dat](media/data-factory-stored-proc-activity/new-data-store.png)
4. V části skript JSON proveďte následující změny: 
    1. Nahrazení ** &lt;webu název serveru&gt; ** s název serveru databáze SQL Azure.
    2. Nahrazení ** &lt;název databáze&gt; ** s databází, ve které jste vytvořili v tabulce a uložené procedury.
    3. Nahrazení ** &lt; username@servername ** s uživatelský účet, který má přístup k databázi.
    4. Nahrazení ** &lt;heslo&gt; ** pomocí hesla k uživatelskému účtu. 

    ![Nové úložiště dat](media/data-factory-stored-proc-activity/azure-sql-linked-service.png)
5. Na panelu s příkazy pro nasazení propojené služby klikněte na **nasazení** . Potvrďte, že se AzureSqlLinkedService ve stromovém zobrazení na levé straně. 

    ![stromové zobrazení propojené službou](media/data-factory-stored-proc-activity/tree-view.png)

### <a name="create-an-output-dataset"></a>Vytvoření datovou sadu výstup
6. Klikněte na **... Další** na panelu nástrojů klikněte na **novou sadu**a klikněte na **Azure SQL**. **Nové datové sady** na panelu s příkazy a vyberte **Azure SQL**.

    ![stromové zobrazení propojené službou](media/data-factory-stored-proc-activity/new-dataset.png)
7. Zkopírujte tento JSON skript v editoru JSON.

        {               
            "name": "sprocsampleout",
            "properties": {
                "type": "AzureSqlTable",
                "linkedServiceName": "AzureSqlLinkedService",
                "typeProperties": {
                    "tableName": "sampletable"
                },
                "availability": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }
        }
7. Na panelu s příkazy pro nasazení datovou sadu, klikněte na **nasazení** . Potvrďte, že se datovou sadu ve stromovém zobrazení. 

    ![stromové zobrazení propojené služby](media/data-factory-stored-proc-activity/tree-view-2.png)

### <a name="create-a-pipeline-with-sqlserverstoredprocedure-activity"></a>Vytvořte příležitost s SqlServerStoredProcedure aktivity
Teď aktivitu SqlServerStoredProcedure vytvoření kanálu.
 
9. Klikněte na **... Další** příkaz pruhové a klikněte na **Nový kanál**. 
9. Zkopírujte následující úryvek JSON. Nastavit, aby **sp_sample** **storedProcedureName** . Název a velikosti písmen parametru **DateTime** musí shodovat s názvem a velikost písmen parametru v definici uložená procedura.  

        {
            "name": "SprocActivitySamplePipeline",
            "properties": {
                "activities": [
                    {
                        "type": "SqlServerStoredProcedure",
                        "typeProperties": {
                            "storedProcedureName": "sp_sample",
                            "storedProcedureParameters": {
                                "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                            }
                        },
                        "outputs": [
                            {
                                "name": "sprocsampleout"
                            }
                        ],
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        },
                        "name": "SprocActivitySample"
                    }
                ],
                "start": "2016-08-02T00:00:00Z",
                "end": "2016-08-02T05:00:00Z",
                "isPaused": false
            }
        }

    Pokud potřebujete předáte null k zadání parametru, pomocí následující syntaxe: "Parametr1": null (malá písmena). 
9. Na panelu nástrojů pro nasazení kanálu klikněte na **nasazení** .  

### <a name="monitor-the-pipeline"></a>Sledování kanálu

6. Klikněte na **X** zavřete Editor Factory dat listy a přejděte zpátky na zásuvné Data Factory a klikněte na **Diagram**.

    ![dlaždice diagramu](media/data-factory-stored-proc-activity/data-factory-diagram-tile.png)
7. V **Zobrazení diagramu**najdete v článku Přehled kanály a datové sady použita v tomto kurzu. 

    ![dlaždice diagramu](media/data-factory-stored-proc-activity/data-factory-diagram-view.png)
8. V zobrazení diagramu poklikejte na datovou sadu **sprocsampleout**. Zobrazí výsečí připravena. Je třeba pět výsečí vzhledem k tomu, že je vyrobeno řez pro každou hodinu mezi čas zahájení a od ve formátu JSON koncového času.

    ![dlaždice diagramu](media/data-factory-stored-proc-activity/data-factory-slices.png) 
10. Po řez **připravena** spuštění, * *Vyberte* z sampletable** dotazu v databázi Azure SQL k ověření, že vložení dat v tabulce uloženou procedurou.

    ![Výstupní data](./media/data-factory-stored-proc-activity/output.png)

    Podrobné informace o sledování Azure Data Factory potrubí naleznete v tématu [Sledování kanálu](data-factory-monitor-manage-pipelines.md) .  

> [AZURE.NOTE] V tomto příkladu SprocActivitySample má bez vstupů. Pokud chcete zřetězení tuto aktivitu s aktivity před (to znamená předchozí processing), výstupy nadřazeného aktivity mohou sloužit jako vstupů v této aktivitě. V takovém případě tuto aktivitu nespustí, dokud dokončení nadřazeného aktivity a výstupy nadřazeného aktivity nabízí (připravení stav). Vstupní hodnoty nelze použít přímo jako parametr aktivity uložená procedura

## <a name="json-format"></a>Formátu JSON
    {
        "name": "SQLSPROCActivity",
        "description": "description", 
        "type": "SqlServerStoredProcedure",
        "inputs":  [ { "name": "inputtable"  } ],
        "outputs":  [ { "name": "outputtable" } ],
        "typeProperties":
        {
            "storedProcedureName": "<name of the stored procedure>",
            "storedProcedureParameters":  
            {
                "param1": "param1Value"
                …
            }
        }
    }

## <a name="json-properties"></a>Vlastnosti JSON

Vlastnost | Popis | Povinné
-------- | ----------- | --------
Jméno | Název aktivity | Ano
Popis | Text s popisem aktivitu k čemu slouží | Ne
Typ | SqlServerStoredProcedure | Ano
vstupů | Volitelné. Pokud nezadáte vstupní datovou sadu, musí být součástí (stavu "Připraveno") pro uložené procedury aktivitu na spustit. Zadávání datovou sadu nelze spotřebované v uložená procedura jako parametr. Pouze umožňuje zkontrolovat závislost před spuštěním uložená procedura aktivity. | Ne
výstup | Je nutné zadat výstup datovou sadu uložená procedura aktivity. Výstup datovou sadu Určuje **plán** pro uložené procedury aktivitu (každou hodinu, týdně, měsíčně, atd.). <br/><br/>Výstup datovou sadu musíte použít **propojené služby** , které odkazuje na databázi SQL Azure nebo datový sklad SQL Azure nebo databázi SQL serveru, ve kterém chcete uložená procedura ke spuštění. <br/><br/>Výstup datovou sadu mohou sloužit jako způsob, jak předat výsledek uložené procedury pro pozdější zpracování pomocí jiného aktivitu ([řetězení aktivity](data-factory-scheduling-and-execution.md#chaining-activities)) v kanálu. Však Factory dat nejsou automaticky výstup uložené procedury k zápisu tento datovou sadu. Je uložená procedura, který data zapisuje do tabulky SQL odkazující na datovou sadu výstupu. <br/><br/>V některých případech může být datovou sadu výstup, **formální datovou sadu**, který se používá pouze k určení plán pro spuštění uložené procedury aktivity. | Ano
storedProcedureName | Zadejte název uložené procedury v databázi Azure SQL nebo Azure SQL datový sklad, který je znázorněn propojené službu, kterou používá výstupní tabulce. | Ano
storedProcedureParameters | Zadejte hodnoty pro uložené procedury parametry. Pokud potřebujete předat null k zadání parametru, pomocí následující syntaxe: "Parametr1": hodnoty null (všechna malá písmena). Viz následující příklad informace o používání této vlastnosti.| Ne

## <a name="passing-a-static-value"></a>Předávání statická hodnota 
Teď Pojďme můžete do ní přidat další sloupec s názvem "Scénář" v tabulce obsahující statická hodnota s názvem "Dokumentu vzorku".

![Ukázková data 2](./media/data-factory-stored-proc-activity/sample-data-2.png)

    CREATE PROCEDURE sp_sample @DateTime nvarchar(127) , @Scenario nvarchar(127)
    
    AS
    
    BEGIN
        INSERT INTO [sampletable]
        VALUES (newid(), @DateTime, @Scenario)
    END

Teď předáte parametr scénář a hodnotu z uložené procedury aktivity. V části typeProperties v předchozím příkladu vypadá jako následující fragment kódu:

    "typeProperties":
    {
        "storedProcedureName": "sp_sample",
        "storedProcedureParameters": 
        {
            "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)",
            "Scenario": "Document sample"
        }
    }

