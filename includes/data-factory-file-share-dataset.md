## <a name="fileshare-dataset-type-properties"></a>Vlastnosti typu datovou sadu sdílené položce

Úplný seznam oddíly a vlastnosti jsou k dispozici pro definování datové sady naleznete v článku [Vytvoření datové sady](../articles/data-factory/data-factory-create-datasets.md) . Například strukturu, dostupnost a zásady datovou sadu JSON jsou podobné pro všechny typy datovou sadu. 

V části **typeProperties** se liší u jednotlivých typů datovou sadu. Poskytuje informace, které jsou specifické pro typ datovou sadu. V části typeProperties datovou sadu typu datovou sadu **sdílené položce** obsahuje následující vlastnosti:

Vlastnost | Popis | Povinné
-------- | ----------- | --------
cesta_ke_složce | Sub cestu ke složce. Používejte řídicí znak "\" pro speciální znaky v řetězci. Příklady najdete v článku [ukázkové propojené definice služby a datovou sadu](#sample-linked-service-and-dataset-definitions) .<br/><br/>Tato vlastnost můžete kombinovat s **partitionBy** mít složku cesty založené na výseč začátek/konec data a času. | Ano
Název souboru | Zadejte název souboru **cesta_ke_složce** Pokud tabulka by měla odkázat na konkrétní soubor ve složce. Pokud nezadáte každá hodnota parametru tato vlastnost tabulky odkazuje na všechny soubory ve složce.<br/><br/>Při zadání hodnoty název souboru není pro datovou sadu výstup název souboru vygenerovaných by měl být následující v tomto formátu: <br/><br/>Data. <Guid>txt (Příklad: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt | Ne
partitionedBy | partitionedBy mohou sloužit k určení dynamické cesta_ke_složce název dat časové řady. Například cesta_ke_složce s parametry pro každou hodinu data. | Ne
Formát | Následující typy formátování podporované: **Formát textu**, **AvroFormat**, **JsonFormat**a **OrcFormat**. Nastavte vlastnosti **Typ** v části formát na jednu z těchto hodnot. Naleznete v částech [Určující formát textu](#specifying-textformat), [Zadání AvroFormat](#specifying-avroformat), [Zadání JsonFormat](#specifying-jsonformat)a [Určení OrcFormat](#specifying-orcformat) podrobnosti. Pokud chcete zkopírovat soubory jako-je mezi souborové úložištích (binární kopírujte), můžete přejít v části formát v obou definice vstupní a výstupní datovou sadu. | Ne
fileFilter | Určení filtru můžete vybrat podmnožinu soubory v cesta_ke_složce, nikoli všechny soubory.<br/><br/>Povolené hodnoty jsou: `*` (více znaků) a `?` (znak).<br/><br/>Příklady 1:`"fileFilter": "*.log"`<br/>Příklad 2:`"fileFilter": 2014-1-?.txt"`<br/><br/> fileFilter platí pro zadávání datovou sadu sdílené položce. Tato vlastnost není podporována s HDFS.  | Ne
| komprese | Určete typ a stupeň komprese pro data. Jsou podporované typy: **GZip** **Deflate**a **BZip2** a podporované úrovně jsou: **Optimal** a **nejrychlejší**. Nastavení komprese nejsou v současné době podporované pro **AvroFormat** nebo **OrcFormat**daty. Další informace najdete v části [podporu komprese](#compression-support) .  | Ne |
| useBinaryTransfer | Určení zda použít režim binární přenosu. Vrátí hodnotu true pro binární režimu a NEPRAVDA ASCII. Výchozí hodnota: PRAVDA. Tato vlastnost lze použít pouze v případě přidružené propojené služby typ má typu: Server_ftp. | Ne | 
 

> [AZURE.NOTE] Název souboru a fileFilter nejdou současně.

### <a name="using-partionedby-property"></a>Pomocí partionedBy vlastnosti

Jak je uvedeno v předchozí části, můžete určit dynamické cesta_ke_složce, název souboru pro dat časové řady s partitionedBy. Lze provést pomocí Data Factory maker a systém proměnnou SliceStart, SliceEnd, které označují logické časové období pro dané výseč. 

Další informace o datových sad časové řady, plánování a výseče, najdete v článku [Vytvoření datové sady](../articles/data-factory/data-factory-create-datasets.md) [plánování a provádění](../articles/data-factory/data-factory-scheduling-and-execution.md)a [Vytváření potrubí](../articles/data-factory/data-factory-create-pipelines.md) články. 

#### <a name="sample-1"></a>Příklad 1:

    "folderPath": "wikidatagateway/wikisampledataout/{Slice}",
    "partitionedBy": 
    [
        { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
    ],

V tomto příkladu {výseč} nahrazuje s hodnotou Data Factory systém proměnné SliceStart ve formátu (YYYYMMDDHH). SliceStart odkazuje na začátek výseče. Cesta_ke_složce se liší jednotlivých výsečí. Příklad: wikidatagateway/wikisampledataout/2014100103 nebo wikidatagateway/wikisampledataout/2014100104.

#### <a name="sample-2"></a>Příklad 2:

    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy": 
     [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
    ],

V tomto příkladu rok, měsíc, den a čas SliceStart extrahování do samostatných proměnných, které se používají vlastnostmi cesta_ke_složce a název souboru.
