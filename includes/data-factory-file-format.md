## <a name="specifying-formats"></a>Určení formátů

Azure Data Factory podporuje následující typy formátů: 

- [Formátování textu](#specifying-textformat)
- [Formátu JSON](#specifying-jsonformat)
- [Formát Avro](#specifying-avroformat)
- [Formát ORC](#specifying-orcformat)
- [Formát parquet](#specifying-parquetformat)

### <a name="specifying-textformat"></a>Určení formát textu

Pokud formát je nastavena na **Formát textu**, můžete určit následující **volitelná** vlastnosti v části **Formát** .

| Vlastnost | Popis | Povolené hodnoty | Povinné |
| -------- | ----------- | -------- | -------- | 
| columnDelimiter | Znak, který samostatných sloupců v souboru. | Pouze jeden znak jsou povolené. Výchozí hodnota je čárka (,). <br/><br/>Znak sady Unicode v nápovědě k [Znaků kódu Unicode](https://en.wikipedia.org/wiki/List_of_Unicode_characters) získat odpovídající kód. Můžete třeba použít méně častých netisknutelné znak, který není pravděpodobně existují ve vašich datech: zadejte "\u0001" představující spustit z nadpis (prohlášení o stavu). | Ne |
| rowDelimiter | Znak použitý k oddělení řádků v souboru. | Pouze jeden znak jsou povolené. Výchozí hodnota je některý z následujících hodnot na pro čtení: ["\r\n", "\r", "\n"] a "\r\n" na zápisu. | Ne |
| escapeChar | Speciální znak, který uniknout oddělovače sloupce v obsahu vstupní soubor. <br/><br/>Nelze zadat escapeChar a quoteChar pro tabulku. | Pouze jeden znak jsou povolené. Výchozí hodnota. <br/><br/>Příklad: Pokud máte čárkou (",") jako sloupec oddělovač, ale chcete, aby se znak čárky v textu (Příklad: "Ahoj, světe"), můžete definovat "$" jako řídicí znak a použijte řetězec "Ahoj$, world" ve zdroji. | Ne | 
| quoteChar | Znak, který nabídka hodnotu řetězce. Oddělovačů sloupců a řádků do znaky uvozovek by pokládány jako součást hodnotu řetězce. Tato vlastnost je k dispozici vstupní a výstupní datové sady.<br/><br/>Nelze zadat escapeChar a quoteChar pro tabulku. | Pouze jeden znak jsou povolené. Výchozí hodnota. <br/><br/>Například, pokud máte čárkou (",") jako sloupec oddělovač, ale chcete, aby se znak čárky v textu (Příklad: < Ahoj, prezentace >), můžete definovat "(dvojité uvozovky) jako znak a použití řetězec"Ahoj, světe"ve zdroji. | Ne |
| nullValue | Jeden nebo víc znaků představující hodnotu null. | Jeden nebo víc znaků. Výchozí hodnoty jsou "\N" a "NULL" pro čtení a "\N" na zápisu. | Ne |
| encodingName | Zadejte název kódování. | Platný název kódování. v tématu [Encoding.EncodingName vlastnosti](https://msdn.microsoft.com/library/system.text.encoding.aspx). Příklad: 1 windows 250 nebo shift_jis. Výchozí hodnota je UTF-8. | Ne | 
| firstRowAsHeader | Určuje, zda k zamyšlení první řádek jako záhlaví. Pro zadávání datovou sadu načte Data Factory první řádek jako záhlaví. Datovou sadu výstupní Data Factory zapíše data z první řádek jako záhlaví. <br/><br/>Ukázka scénáře naleznete v tématu [scénáře pro používání **firstRowAsHeader** a **skipLineCount** ](#scenarios-for-using-firstrowasheader-and-skiplinecount) . | PRAVDA<br/>NEPRAVDA (výchozí) | Ne |
| skipLineCount | Označuje počet řádků, které chcete přejít při čtení data ze souborů s zadávání. Pokud nejsou zadány skipLineCount a firstRowAsHeader, řádky jsou nejdřív vynechány a potom informace v záhlaví číst e-maily na vstupní soubor. <br/><br/>Ukázka scénáře naleznete v tématu [scénáře pro používání firstRowAsHeader a skipLineCount](#scenarios-for-using-firstrowasheader-and-skiplinecount) . | Celé číslo | Ne | 
| treatEmptyAsNull | Určuje, jestli se považovat null nebo prázdná hodnota null při čtení dat ze souboru vstupní. | PRAVDA (výchozí)<br/>NEPRAVDA | Ne |  

#### <a name="textformat-example"></a>Příklad formát textu
Následující příklad ukazuje některé vlastnosti formát pro formát textu.

    "typeProperties":
    {
        "folderPath": "mycontainer/myfolder",
        "fileName": "myblobname"
        "format":
        {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": ";",
            "quoteChar": "\"",
            "NullValue": "NaN"
            "firstRowAsHeader": true,
            "skipLineCount": 0,
            "treatEmptyAsNull": true
        }
    },

Použití escapeChar místo quoteChar, nahraďte řádku quoteChar s následující escapeChar:

    "escapeChar": "$",



### <a name="scenarios-for-using-firstrowasheader-and-skiplinecount"></a>Scénáře pro používání firstRowAsHeader a skipLineCount

- Kopírujete zdroj jiných souborů do textového souboru a chcete přidat řádek záhlaví, který obsahuje schématu metadata (třeba: schématu SQL). Zadejte **firstRowAsHeader** jako PRAVDA v datové sadě výstup pro tento scénář. 
- Kopírování z textového souboru, který obsahuje řádek záhlaví na jímka než soubor a chcete přetáhnout tento řádek. Zadejte **firstRowAsHeader** jako PRAVDA v zadávání datovou sadu.
- Kopírování z textového souboru a chcete přeskočit na začátku několik řádky, které obsahují žádné informace dat nebo záhlaví. Zadejte **skipLineCount** pro určení počtu řádků, které mají být vynechány. Obsahuje-li zbytek souboru řádek záhlaví, můžete taky určit **firstRowAsHeader**. Pokud nejsou zadány **skipLineCount** a **firstRowAsHeader** , řádky jsou nejdřív vynechány a potom číst informace v záhlaví z vstupní soubor

### <a name="specifying-avroformat"></a>Určení AvroFormat
Pokud je nastaveno formátu AvroFormat, nepotřebujete určete libovolné vlastnosti v části formát v části typeProperties. Příklad:

    "format":
    {
        "type": "AvroFormat",
    }

Pokud chcete použít formát Avro v tabulku podregistru, můžete vytvořit odkaz [Apache podregistru kurz](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe).

Mějte na paměti následující skutečnosti:  

- Nejsou podporovány [složitými datovými typy](http://avro.apache.org/docs/current/spec.html#schema_complex) (zaznamenává výčty matice, mapy, sjednocení a podařilo odstranit)

### <a name="specifying-jsonformat"></a>Určení JsonFormat

Pokud formát je nastavena na **JsonFormat**, můžete určit následující **volitelná** vlastnosti v části **Formát** .

| Vlastnost | Popis | Povinné |
| -------- | ----------- | -------- |
| filePattern | Označení obchodních dat uložených v jednotlivých souborů JSON. Povolené hodnoty jsou: **setOfObjects** a **arrayOfObjects**. **Výchozí** hodnota je: **setOfObjects**. Sledovat oddíly podrobné informace o těchto vzorků.| Ne |
| encodingName | Zadejte název kódování. Seznam platné názvy kódování najdete v tématu: [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx) vlastnost. Příklad: 1 windows 250 nebo shift_jis. **Výchozí** hodnota je: **UTF-8**. | Ne | 
| nestingSeparator | Znak, který se používá k oddělení počet úrovní vnoření. Výchozí hodnota je "." (tečka). | Ne | 


#### <a name="setofobjects-file-pattern"></a>vzor setOfObjects souboru

Každý soubor obsahuje jeden objekt nebo řádek středníkem/spojeny více objektů. Pokud zvolíte tuto možnost v sadě dat výstupu Kopírovat činnost produkuje jeden soubor JSON s všech objektů na řádek (řádek oddělený středníkem).

**jeden objekt** 

    {
        "time": "2015-04-29T07:12:20.9100000Z",
        "callingimsi": "466920403025604",
        "callingnum1": "678948008",
        "callingnum2": "567834760",
        "switch1": "China",
        "switch2": "Germany"
    }

**řádek středníkem JSON** 

    {"time":"2015-04-29T07:12:20.9100000Z","callingimsi":"466920403025604","callingnum1":"678948008","callingnum2":"567834760","switch1":"China","switch2":"Germany"}
    {"time":"2015-04-29T07:13:21.0220000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789037573","switch1":"US","switch2":"UK"}
    {"time":"2015-04-29T07:13:21.4370000Z","callingimsi":"466923101048691","callingnum1":"678901578","callingnum2":"345626404","switch1":"Germany","switch2":"UK"}
    {"time":"2015-04-29T07:13:22.0960000Z","callingimsi":"466922202613463","callingnum1":"789037573","callingnum2":"789044691","switch1":"UK","switch2":"Australia"}
    {"time":"2015-04-29T07:13:22.0960000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789044691","switch1":"US","switch2":"Australia"}

**zřetězený JSON**

    {
        "time": "2015-04-29T07:12:20.9100000Z",
        "callingimsi": "466920403025604",
        "callingnum1": "678948008",
        "callingnum2": "567834760",
        "switch1": "China",
        "switch2": "Germany"
    }
    {
        "time": "2015-04-29T07:13:21.0220000Z",
        "callingimsi": "466922202613463",
        "callingnum1": "123436380",
        "callingnum2": "789037573",
        "switch1": "US",
        "switch2": "UK"
    }
    {
        "time": "2015-04-29T07:13:21.4370000Z",
        "callingimsi": "466923101048691",
        "callingnum1": "678901578",
        "callingnum2": "345626404",
        "switch1": "Germany",
        "switch2": "UK"
    }


#### <a name="arrayofobjects-file-pattern"></a>arrayOfObjects souborů vzorku. 

Každý soubor obsahuje pole objektů. 

    [
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        },
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:22.0960000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "789037573",
            "callingnum2": "789044691",
            "switch1": "UK",
            "switch2": "Australia"
        },
        {
            "time": "2015-04-29T07:13:22.0960000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789044691",
            "switch1": "US",
            "switch2": "Australia"
        },
        {
            "time": "2015-04-29T07:13:24.2120000Z",
            "callingimsi": "466922201102759",
            "callingnum1": "345698602",
            "callingnum2": "789097900",
            "switch1": "UK",
            "switch2": "Australia"
        },
        {
            "time": "2015-04-29T07:13:25.6320000Z",
            "callingimsi": "466923300236137",
            "callingnum1": "567850552",
            "callingnum2": "789086133",
            "switch1": "China",
            "switch2": "Germany"
        }
    ]

### <a name="jsonformat-example"></a>Příklad JsonFormat

Pokud máte otevřený soubor JSON následující obsahu:  

    {
        "Id": 1,
        "Name": {
            "First": "John",
            "Last": "Doe"
        },
        "Tags": ["Data Factory”, "Azure"]
    }

a chcete ho zkopírujte do tabulky Azure SQL ve formátu následující: 

ID  | Name.First | Name.Middle | Name.Last | Značky
--- | ---------- | ----------- | --------- | ----
1 | Petr | Null | Karásek | ["Data Factory", "Azure"]

Zadávání datovou sadu typu JsonFormat je definována následujícím způsobem: (částečné definice se jenom příslušné části)

    "properties": {
        "structure": [
            {"name": "Id", "type": "Int"},
            {"name": "Name.First", "type": "String"},
            {"name": "Name.Middle", "type": "String"},
            {"name": "Name.Last", "type": "String"},
            {"name": "Tags", "type": "string"}
        ],
        "typeProperties":
        {
            "folderPath": "mycontainer/myfolder",
            "format":
            {
                "type": "JsonFormat",
                "filePattern": "setOfObjects",
                "encodingName": "UTF-8",
                "nestingSeparator": "."
            }
        }
    }

Pokud není definované strukturu, kopírovat aktivity sloučí struktuře ve výchozím nastavení a zkopírujte každé věc. 

#### <a name="supported-json-structure"></a>Podporované strukturu JSON
Mějte na paměti následující skutečnosti: 

- Každý objekt s kolekcí párů název/hodnota je namapované na jeden řádek dat ve formátu tabulky. Objekty může být vnořeno do a můžete určit, jak chcete sloučit struktuře v datovou sadu vnoření oddělovače (.) ve výchozím nastavení. Podívejte se na [Příklad JsonFormat](#jsonformat-example) předcházejícího oddílu příklad.  
- Pokud struktuře není definované v datové sadě dat Factory, kopírovat aktivity zjistí schéma z první objekt a sloučit celého objektu. 
- Pokud vstupní JSON maticových, převede aktivity kopírovat celé pole hodnotu řetězec. Je možné vynechat pomocí [mapování sloupců nebo filtrování](#column-mapping-with-translator-rules).
- Pokud jsou názvy duplicitních na stejné úrovni, kopírovat aktivity, vybere poslední.
- Vlastnost názvy rozlišují malá a velká písmena. Dvě vlastnosti se stejným názvem, ale různých střeva je považováno za dvě samostatná vlastnosti. 

### <a name="specifying-orcformat"></a>Určení OrcFormat
Pokud je nastaveno formátu OrcFormat, nepotřebujete určete libovolné vlastnosti v části formát v části typeProperties. Příklad:

    "format":
    {
        "type": "OrcFormat"
    }

> [AZURE.IMPORTANT] Pokud nejsou kopírování souborů ORC **jako-je** mezi místním prostředím a cloudové úložiště dat, musíte si nainstalovat JRE 8 (prostředí Java Runtime) v počítači brány. 64bitová verze brány vyžaduje JRE 64bitovou a 32bitovou verzi brány vyžaduje JRE 32bitová verze. Obě verze [zde](http://go.microsoft.com/fwlink/?LinkId=808605)můžete najít. Vyberte příslušný.

Mějte na paměti následující skutečnosti:

-   Komplexní data, které typy nejsou podporované (struktura MAPU, seznamu, unie)
-   Soubor ORC má tři [Možnosti související s kompresi](http://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/): žádné ZLIB, SNAPPY. Data Factory podporuje načítání dat ze souboru ORC v některém z těchto mu tuhle zkomprimovanou formátů. Použije komprese správný kodek, je v metadatech číst data. Však při psaní do souboru ORC Data Factory zvolí ZLIB, což je výchozí hodnota pro ORC. V současné době žádným způsobem přepsat toto chování. 

### <a name="specifying-parquetformat"></a>Určení ParquetFormat
Pokud je nastaveno formátu ParquetFormat, nepotřebujete určete libovolné vlastnosti v části formát v části typeProperties. Příklad:

    "format":
    {
        "type": "ParquetFormat"
    }

> [AZURE.IMPORTANT] Pokud nejsou kopírování souborů Parquet **jako-je** mezi místním prostředím a cloudové úložiště dat, musíte si nainstalovat JRE 8 (prostředí Java Runtime) v počítači brány. 64bitová verze brány vyžaduje JRE 64bitovou a 32bitovou verzi brány vyžaduje JRE 32bitová verze. Obě verze [zde](http://go.microsoft.com/fwlink/?LinkId=808605)můžete najít. Vyberte příslušný.

Mějte na paměti následující skutečnosti:

-   Komplexní data, které typy nejsou podporované (mapa, seznam)
-   Parquet soubor neobsahuje následující možnosti komprese související: žádný, SNAPPY GZIP a LZO. Data Factory podporuje načítání dat ze souboru ORC v některém z těchto mu tuhle zkomprimovanou formátů. Použije kodek komprese metadata zobrazíte data. Však při psaní do souboru Parquet Data Factory zvolí SNAPPY, což je výchozí hodnota Parquet formátu. V současné době žádným způsobem přepsat toto chování. 
