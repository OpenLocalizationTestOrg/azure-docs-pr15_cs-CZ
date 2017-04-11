<properties 
    pageTitle="Přesunutí dat do/z Oracle pomocí Data Factory | Microsoft Azure" 
    description="Další informace o přesunutí dat z databáze Oracle, který je místní pomocí Azure Data Factory." 
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
    ms.date="09/20/2016" 
    ms.author="jingwang"/>

# <a name="move-data-tofrom-on-premises-oracle-using-azure-data-factory"></a>Přesuňte data z místního Oracle pomocí Azure Data Factory 

Tento článek shrnuje, jak můžete data factory kopírovat aktivity přesuňte data z Oracle z nebo do jiného datového uložit. Tento článek je založena na článek [aktivity přesun dat](data-factory-data-movement-activities.md) , který uvádí obecné základní informace o přesun dat s kopírovat aktivitu a podporovaný datový úložiště kombinace.

## <a name="installation"></a>Instalace 
Pro službu Azure Data Factory mohli připojit k místní databázi Oracle musíte nainstalovat tyto prvky: 

- Brána pro správu dat na stejném počítači, který je hostitelem databáze nebo na samostatném počítač chcete-li předejít soutěží pro zdroje s databází. Brána pro správu dat je agent klienta, který propojuje místních zdrojů dat ke cloudovým službám pro účely zabezpečené a spravovaných. Přečtěte si téma [Přesun dat mezi místním prostředím a cloudu](data-factory-move-data-between-onprem-and-cloud.md) článek podrobnosti o bráně pro správu dat. 
- Zprostředkovatel dat Oracle pro .NET Tato součást je součástí [Oracle dat aplikace Access součásti pro Windows](http://www.oracle.com/technetwork/topics/dotnet/downloads/). Instalace odpovídající verze (32 nebo 64bitový) na hostitelském počítači nainstalovanou brány. [12.1 .NET zprostředkovatele dat Oracle](http://docs.oracle.com/database/121/ODPNT/InstallSystemRequirements.htm#ODPNT149) můžete získat přístup k databázi Oracle 10 g verze 2 nebo novější.

    Pokud se rozhodnete "XCopy instalace", postupujte podle kroků v readme.htm. Doporučujeme že zvolit instalační služby UI (není XCopy jednu). 
 
    Po instalaci na poskytovatele restartujte službu hostitelská brány správy dat v počítači pomocí služby aplet (nebo) Správce konfigurace brány pro správu dat.  

> [AZURE.NOTE] Naleznete v tématu [Poradce při potížích s brány problémy](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) tipy pro řešení potíží s připojením/brána související s jejich stavem. 

## <a name="copy-data-wizard"></a>Kopírování dat Průvodce
Nejjednodušší způsob, jak vytvořit kanál, který slouží ke kopírování dat z/k databázi Oracle se ukládají data podporované jímky je použití Průvodce datovým kopírovat. V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) rychlé informace o vytváření kanálů pomocí Průvodce datovým kopírovat. 

Následující příklad uvádí definice JSON vzorku, které slouží k vytvoření potrubí pomocí [Azure portál](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [Azure Powershellu](data-factory-copy-activity-tutorial-using-powershell.md). Kopírování dat z/k databázi Oracle z úložiště objektů Blob Azure znázorňují. Data můžete však zkopírována do jednotlivých propadů uvedená [tady](data-factory-data-movement-activities.md#supported-data-stores) pomocí kopírování aktivity v Azure Data Factory.   

## <a name="sample-copy-data-from-oracle-to-azure-blob"></a>Ukázka: Zkopírování dat z Oracle objektů Blob Azure
Tento příklad ukazuje, jak ke kopírování dat z databáze Oracle místní k úložišti objektů Blob Azure. Data lze však zkopírovaný **přímo** k některým propadů uvedená [tady](data-factory-data-movement-activities.md#supported-data-stores) pomocí kopírování aktivity v Azure Data Factory.  
 
Vzorku obsahuje následující factory entit dat:

1.  Propojené služba typu [OnPremisesOracle](data-factory-onprem-oracle-connector.md#oracle-linked-service-properties).
2.  Propojené služba typu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Zadávání [datovou sadu](data-factory-create-datasets.md) typu [OracleTable](data-factory-onprem-oracle-connector.md#oracle-dataset-type-properties). 
4.  Objekt výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
5.  [Kanálem k odesílání zpráv](data-factory-create-pipelines.md) s kopírovat aktivity na počítači se používá [OracleSource](data-factory-onprem-oracle-connector.md#oracle-copy-activity-type-properties) jako zdroj a [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) jako jímky.

Vzorku zkopíruje data z tabulky databáze Oracle místní objektů blob každou hodinu. Další informace o různých vlastnosti použité ve vzorku najdete v článku si přečtěte následující dokumentaci v částech sledovat vzorky.

**Oracle propojené služby:**

    {
      "name": "OnPremisesOracleLinkedService",
      "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
          "ConnectionString": "data source=<data source>;User Id=<User Id>;Password=<Password>;",
          "gatewayName": "<gateway name>"
        }
      }
    }

**Úložiště objektů Blob Azure propojené služby:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
      }
    }

**Oracle vstupní datovou sadu:**

Vzorku předpokládá nevytvoříte tabulky "Tabulka" Oracle a obsahuje sloupec s názvem "timestampcolumn" pro dat časové řady. 

Nastavení "externí": "true" informuje o tom, službu Data Factory, datové externí stránku factory dat a není vytvořené pomocí aktivity v data factory.

    {
        "name": "OracleInput",
        "properties": {
            "type": "OracleTable",
            "linkedServiceName": "OnPremisesOracleLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
               "external": true,
            "availability": {
                "offset": "01:00:00",
                "interval": "1",
                "anchorDateTime": "2014-02-27T12:00:00",
                "frequency": "Hour"
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


**Objektů Blob Azure výstup datovou sadu:**

Zápisu dat do nových objektů blob každou hodinu (četnost: hodiny, intervalu: 1). Složky cesty a názvu souboru pro objekt blob jsou vyhodnoceny dynamicky podle počáteční čas dané řezu, který zpracovávání. Cestu ke složce používá rok, měsíc, den a hodin díly počáteční čas.
    
    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
          "partitionedBy": [
            {
              "name": "Year",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "yyyy"
              }
            },
            {
              "name": "Month",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "MM"
              }
            },
            {
              "name": "Day",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "dd"
              }
            },
            {
              "name": "Hour",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "HH"
              }
            }
          ],
          "format": {
            "type": "TextFormat",
            "columnDelimiter": "\t",
            "rowDelimiter": "\n"
          }
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }


**Příležitostí s kopírovat aktivity:**

Kanálu obsahuje kopírovat aktivity, který je nakonfigurovaný na používání datové sady vstupní a výstupní a je naplánováno spuštění každou hodinu. V kanálu JSON definice typ **zdroje** je nastavena na **OracleSource** a **jímky** typ je nastavený na **BlobSink**.  Dotaz SQL zobrazený zadanými pomocí vlastnosti **oracleReaderQuery** slouží k výběru data v poslední hodinu zkopírovat.

    
    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
          {
            "name": "OracletoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [
              {
                "name": " OracleInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "OracleSource",
                "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
              },
              "sink": {
                "type": "BlobSink"
              }
            },
           "scheduler": {
              "frequency": "Hour",
              "interval": 1
            },
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
         ]
       }
    }


Potřebujete upravit řetězce dotazu podle konfiguraci kalendářních dat v databázi Oracle. Pokud se zobrazí tato chybová zpráva: 

    Message=Operation failed in Oracle Database with the following error: 'ORA-01861: literal does not match format string'.,Source=,''Type=Oracle.DataAccess.Client.OracleException,Message=ORA-01861: literal does not match format string,Source=Oracle Data Provider for .NET,'.

budete muset změnit dotaz, jak je vidět v následujícím příkladu (použití funkce to_date):

    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= to_date(\\'{0:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\')  AND timestampcolumn < to_date(\\'{1:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\') ', WindowStart, WindowEnd)"

## <a name="sample-copy-data-from-azure-blob-to-oracle"></a>Ukázka: Zkopírování dat z objektů Blob Azure Oracle
Tento příklad ukazuje, jak ke kopírování dat z úložiště objektů Blob Azure k databázi Oracle místní. Data lze však zkopírovaný **přímo** z libovolného zdroje uvedená [tady](data-factory-data-movement-activities.md#supported-data-stores) pomocí kopírování aktivity v Azure Data Factory.  
 
Vzorku obsahuje následující factory entit dat:

1.  Propojené služba typu [OnPremisesOracle](data-factory-onprem-oracle-connector.md#oracle-linked-service-properties).
2.  Propojené služba typu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Zadávání [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Objekt výstup [datovou sadu](data-factory-create-datasets.md) typu [OracleTable](data-factory-onprem-oracle-connector.md#oracle-dataset-type-properties). 
5.  [Kanálem k odesílání zpráv](data-factory-create-pipelines.md) s kopírovat aktivity na počítači se používá [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) jako zdroj [OracleSink](data-factory-onprem-oracle-connector.md#oracle-copy-activity-type-properties) jako jímky.

Vzorku slouží ke kopírování dat z objektů blob do tabulky v databázi Oracle místní každou hodinu. Další informace o různých vlastnosti použité ve vzorku najdete v článku si přečtěte následující dokumentaci v částech sledovat vzorky.

**Oracle propojené služby:**

    {
      "name": "OnPremisesOracleLinkedService",
      "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
          "ConnectionString": "data source=<data source>;User Id=<User Id>;Password=<Password>;",
          "gatewayName": "<gateway name>"
        }
      }
    }

**Úložiště objektů Blob Azure propojené služby:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
      }
    }

**Azure vstupní datovou sadu objektů Blob**

Data je vybraných kontakty z nových objektů blob každou hodinu (četnost: hodiny, intervalu: 1). Složky cesty a názvu souboru pro objekt blob jsou vyhodnoceny dynamicky podle počáteční čas dané řezu, který zpracovávání. Cestu ke složce používá rok, měsíc a den část čas zahájení a název souboru hodinu část počáteční čas. "externí": "true" nastavení informuje službu Data Factory, že v této tabulce externí stránku factory dat a není vytvořené pomocí aktivity v data factory.

    {
      "name": "AzureBlobInput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
          "fileName": "{Hour}.csv",
          "partitionedBy": [
            {
              "name": "Year",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "yyyy"
              }
            },
            {
              "name": "Month",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "MM"
              }
            },
            {
              "name": "Day",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "dd"
              }
            },
            {
              "name": "Hour",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "HH"
              }
            }
          ],
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": "\n"
          }
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

**Sady dat Oracle výstup:**

Vzorku předpokládá, že jste vytvořili tabulky "Tabulka" v Oracle. Vytvoření tabulky v Oracle mají stejný počet sloupců podle očekávání soubor CSV objektů Blob má být. Nové řádky jsou přidané do tabulky každou hodinu.

    {
        "name": "OracleOutput",
        "properties": {
            "type": "OracleTable",
            "linkedServiceName": "OnPremisesOracleLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": "1"
            }
        }
    }


**Příležitostí s kopírovat aktivity:**

Kanálu obsahuje kopírovat aktivity, který je nakonfigurovaný na používání datové sady vstupní a výstupní a je naplánováno spuštění každou hodinu. V kanálu JSON definice typ **zdroje** je nastavena na **BlobSource** a **jímky** typ je nastavený na **OracleSink**.  

    
    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
          {
            "name": "AzureBlobtoOracle",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureBlobInput"
              }
            ],
            "outputs": [
              {
                "name": "OracleOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "OracleSink"
              }
            },
           "scheduler": {
              "frequency": "Hour",
              "interval": 1
            },
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
          ]
       }
    }


## <a name="oracle-linked-service-properties"></a>Vlastnosti Oracle propojené služby

Následující tabulka obsahuje popis prvků JSON specifické pro službu Oracle propojené. 

Vlastnost | Popis | Povinné
-------- | ----------- | --------
Typ | Vlastnost typu musí být nastavena na: **OnPremisesOracle** | Ano
connectionString | Zadejte informace potřebné k připojení k databázi Oracle instanci vlastnost connectionString. | Ano 
názevbrány | Název brány, který se používá pro připojení k místní server Oracle | Ano

Další informace o nastavení přihlašovacích údajů pro zdroj dat Oracle místní najdete v článku [Nastavení přihlašovacích údajů a zabezpečení](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .
## <a name="oracle-dataset-type-properties"></a>Vlastnosti typu datovou sadu Oracle

Úplný seznam oddíly a vlastnosti jsou k dispozici pro definování datové sady naleznete v článku [Vytvoření datové sady](data-factory-create-datasets.md) . Například strukturu, dostupnost a zásady datovou sadu JSON jsou podobné pro všechny typy datovou sadu (Oracle, Azure objektů blob Azure table, atd.).
 
V části typeProperties je jiné u jednotlivých typů datovou sadu a poskytuje informace o umístění dat v úložišti. V části typeProperties pro datovou sadu typ OracleTable obsahuje následující vlastnosti:

Vlastnost | Popis | Povinné
-------- | ----------- | --------
Název tabulky | Název tabulky v databázi Oracle odkazující propojené služby. | Ne (Pokud není zadán **oracleReaderQuery** **OracleSource** )

## <a name="oracle-copy-activity-type-properties"></a>Oracle kopírovat aktivity typ vlastnosti

Úplný seznam oddíly a vlastnosti jsou k dispozici pro definování aktivity naleznete v článku [Vytvoření kanály](data-factory-create-pipelines.md) . Vlastnosti jako je název, popis, vstupní a výstupní tabulky a zásad jsou dostupné pro všechny typy aktivit. 

> [AZURE.NOTE]
Aktivity kopírovat pouze jeden vstup, která bude a jedinou výstup.

Vlastnosti dostupné v části typeProperties aktivity na druhou stranu se liší podle jednotlivé typy aktivit. Kopírovat aktivity budou lišit podle toho, typy zdrojů a propadů.

### <a name="oraclesource"></a>OracleSource
Kopírovat činnost Pokud je zdroj typu **OracleSource** následující vlastnosti jsou k dispozici **typeProperties** oddílu:

Vlastnost | Popis |Povolené hodnoty | Povinné
-------- | ----------- | ------------- | --------
oracleReaderQuery | Vlastní dotaz umožňuje číst data. | Řetězec SQL dotazu. Příklad: vyberte *z tabulka <br/> <br/>Pokud není zadán, který se spustí příkaz SQL: Vyberte* z tabulka | Ne (Pokud není zadán **tabulky** **datovou sadu** )

### <a name="oraclesink"></a>OracleSink
**OracleSink** podporuje následující vlastnosti:

Vlastnost | Popis | Povolené hodnoty | Povinné
-------- | ----------- | -------------- | --------
writeBatchTimeout | Počkejte, než času pro vložení dávku dokončete časový limit. | časového rozpětí<br/><br/> Příklad: 00:30:00 (30 minut). | Ne
writeBatchSize | Vloží data do tabulky SQL vyrovnávací paměť dosažení writeBatchSize.   | Celé číslo (počet řádků)| Ne (výchozí: 10000)  
sqlWriterCleanupScript | Zadejte dotaz kopírovat aktivity provést tak, aby se data konkrétní výseče vyčistí. | Příkaz dotazu. | Ne
sliceIdentifierColumnName | Zadejte název sloupce kopírovat aktivity vyplnit identifikátor automatické generování řez, který se používá vyčistit data konkrétní výseče při znovu spustit. | Sloupec název sloupce s datovým typem binary(32). | Ne


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-oracle"></a>Typ mapování pro Oracle

Jak je uvedeno v aktivity [aktivity přesun dat](data-factory-data-movement-activities.md) článek kopírování provádí automatické typ převody z typy zdrojů do jímky typů s následující postup krok 2:

1. Převod typy nativní zdrojů na typ .NET
2. Převod typu .NET na nativní jímky typ

Při přesunutí dat z Oracle, slouží následující mapování z typ dat Oracle pro .NET typ a naopak.

Typ dat Oracle | .NET framework datový typ
---------------- | ------------------------
BFILE | Byte]
OBJEKTŮ BLOB | Byte]
ZNAK | Řetězec
DATOVÝ TYP CLOB | Řetězec
DATUM | Data a času
PLOVOUCÍ | Decimal
CELÉ ČÍSLO | Decimal
INTERVAL ROK, MĚSÍC | Int32
INTERVAL DEN SEKUNDU | Časového rozpětí
DLOUHÉ | Řetězec
DLOUHÉ NEZPRACOVANÝMI | Byte]
NCHAR | Řetězec
NCLOB | Řetězec
ČÍSLO | Decimal
NVARCHAR2 | Řetězec
JAKO NEZPRACOVANÁ | Byte]
ID ŘÁDKU | Řetězec
ČASOVÉ RAZÍTKO | Data a času
ČASOVÉ RAZÍTKO S MÍSTNÍ ČASOVÉ PÁSMO | Data a času
ČASOVÉ RAZÍTKO S ČASOVÉ PÁSMO | Data a času
CELÉ ČÍSLO BEZ ZNAMÉNKA | Číslo
VARCHAR2 | Řetězec
XML | Řetězec

## <a name="troubleshooting-tips"></a>Tipy pro odstraňování potíží

**Problém:** Zobrazit tato **chybová zpráva**: Kopírovat aktivity došlo ke splnění při použití neplatných parametrů: UnknownParameterName, podrobné zprávy: Nelze najít požadované .net Framework Data Provider. To nemusí být nainstalovaný".  

**Možných příčin:**

1. Zprostředkovatel dat .NET Framework pro Oracle nebyl nainstalovaný.
2. Zprostředkovatel dat .NET Framework pro Oracle byl nainstalovaný .NET Framework 2.0 a nebyla nalezena ve složkách .NET Framework 4.0. 

**Rozlišení/alternativní řešení:**

1. Pokud jste nenainstalovali zprostředkovatel .NET pro Oracle, [nainstalujte ji](http://www.oracle.com/technetwork/topics/dotnet/downloads/) a opakovat scénáře. 
2. Pokud se zobrazí chybová zpráva i po instalaci na poskytovatele, proveďte následující kroky: 
    1. Otevřete počítače konfigurace .NET 2.0 ze složky: <system disk>: \Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG\machine.config.
    2. Hledání pro **Zprostředkovatele dat Oracle pro .NET**a soubory byste měli najít položku uvedeno v následující ukázkové unwn v následujících tématech přehrajte zrušením zaškrtnutíder **system.data** -> **DbProviderFactories**:
            “<add name="Oracle Data Provider for .NET" invariant="Oracle.DataAccess.Client" description="Oracle Data Provider for .NET" type="Oracle.DataAccess.Client.OracleClientFactory, Oracle.DataAccess, Version=2.112.3.0, Culture=neutral, PublicKeyToken=89b483f429c47342" />”
2.  Zkopírujte tuto položku do souboru machine.config následující složky 4.0: <system disk>: \Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config a změňte na verzi 4.xxx.x.x.
3.  Instalace "< cesta instalace ODP.NET > \11.2.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll" do globální mezipaměti sestavení (GAC) spuštěním `gacutil /i [provider path]`.



[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]


## <a name="performance-and-tuning"></a>Výkon a optimalizace  
V tématu [kopírování aktivity Performance optimalizace Průvodce](data-factory-copy-activity-performance.md) se naučit používat klíčové faktory, které dopad na výkon přesun dat (Kopírovat aktivita) v Azure Data Factory a různé způsoby, jak optimalizovat jeho.
