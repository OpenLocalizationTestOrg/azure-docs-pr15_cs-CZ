<properties
   pageTitle="Analyzovat a proces JSON dokumenty s podregistru v HDInsight | Microsoft Azure"
   description="Naučte se používat JSON dokumentů a jejich analýzu použití podregistru v HDInsight."
   services="hdinsight"
   documentationCenter=""
   authors="rashimg"
   manager="mwinkle"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="06/22/2015"
   ms.author="rashimg"/>


# <a name="process-and-analyze-json-documents-using-hive-in-hdinsight"></a>Zpracování a analyzovat JSON dokumentů pomocí podregistru v HDInsight

Informace o procesu a analyzovat soubory JSON pomocí podregistru v HDInsight. Následující JSON dokument se použije v tomto kurzu

    {
        "StudentId": "trgfg-5454-fdfdg-4346",
        "Grade": 7,
        "StudentDetails": [
            {
                "FirstName": "Peggy",
                "LastName": "Williams",
                "YearJoined": 2012
            }
        ],
        "StudentClassCollection": [
            {
                "ClassId": "89084343",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "High",
                "Score": 93,
                "PerformedActivity": false
            },
            {
                "ClassId": "78547522",
                "ClassParticipation": "NotSatisfied",
                "ClassParticipationRank": "None",
                "Score": 74,
                "PerformedActivity": false
            },
            {
                "ClassId": "78675563",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "Low",
                "Score": 83,
                "PerformedActivity": true
            }
        ]
    }

Soubor můžete najít v wasbs://processjson@hditutorialdata.blob.core.windows.net/. Další informace o pomocí úložiště objektů Blob Azure Hdinsightu najdete v článku [úložiště objektů Blob Azure použití HDFS kompatibilní s Hadoop v HDInsight](hdinsight-hadoop-use-blob-storage.md). Soubor můžete zkopírovat do výchozího kontejneru svůj cluster požadovaná.

V tomto kurzu použijete konzole podregistru.  Otevření konzole podregistru, najdete v článku [Použití podregistru s Hadoop na Hdinsightu pomocí vzdálené plochy](hdinsight-hadoop-use-hive-remote-desktop.md).

##<a name="flatten-json-documents"></a>Sloučit JSON dokumentů

Metody uvedené v následující části vyžadují JSON dokumentu v jediném řádku. Tak třeba sloučit dokument JSON řetězec. Pokud už je sloučí dokumentu JSON, můžete tento krok přeskočit a přejít rovnou k následující části JSON analýza dat.

    DROP TABLE IF EXISTS StudentsRaw;
    CREATE EXTERNAL TABLE StudentsRaw (textcol string) STORED AS TEXTFILE LOCATION "wasb://processjson@hditutorialdata.blob.core.windows.net/";

    DROP TABLE IF EXISTS StudentsOneLine;
    CREATE EXTERNAL TABLE StudentsOneLine
    (
      json_body string
    )
    STORED AS TEXTFILE LOCATION '/json/students';

    INSERT OVERWRITE TABLE StudentsOneLine
    SELECT CONCAT_WS(' ',COLLECT_LIST(textcol)) AS singlelineJSON
          FROM (SELECT INPUT__FILE__NAME,BLOCK__OFFSET__INSIDE__FILE, textcol FROM StudentsRaw DISTRIBUTE BY INPUT__FILE__NAME SORT BY BLOCK__OFFSET__INSIDE__FILE) x
          GROUP BY INPUT__FILE__NAME;

    SELECT * FROM StudentsOneLine

Jako nezpracovaná JSON se soubor nachází na **wasbs://processjson@hditutorialdata.blob.core.windows.net/**. Tabulku podregistru *StudentsRaw* bodů nezpracovanými zrušením sloučený JSON dokumentu.

Tabulku podregistru *StudentsOneLine* bude uchovávat data v systému souborů výchozí HDInsight cestě */json/studentů /* .

Příkaz INSERT naplnění tabulky StudentOneLine s údaji sloučený JSON.

Příkaz SELECT pouze vrátit 1 řádek.

Tady je výstup příkazu SELECT:

![Sloučení dokumentu JSON.][image-hdi-hivejson-flatten]

##<a name="analyze-json-documents-in-hive"></a>Analýza dokumentů JSON v podregistru

Podregistru nabízí tři různé mechanismy spouštění dotazů na JSON dokumentů:

- Použijte\_JSON\_UDF objektu (uživatele definovaná funkce)
- použití JSON_TUPLE UDF
- Použití vlastní SerDe
- psaní, že jste vlastníkem UDF pomocí Python nebo jiných jazyků. Viz [Tento článek] [ hdinsight-python] o spuštění kódu Python s podregistru.

### <a name="use-the-getjsonobject-udf"></a>Použijte\_JSON_OBJECT UDF
Podregistru nabízí předdefinované UDF říká [získání json objektu](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object) které lze provádět JSON dotazování za běhu. Tento způsob se zadávají dva argumenty – název tabulky a metoda název, který obsahuje sloučené JSON dokumentu a JSON pole, které je potřeba analyzovat. Podívejme se na příklad v tématu Princip tento UDF.

Získání křestního jména a příjmení všech studentů

    SELECT
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.FirstName'),
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.LastName')
    FROM StudentsOneLine;

Tady je výstup při spuštění dotazu v okně.

![get_json_object UDF][image-hdi-hivejson-getjsonobject]

Existuje několik omezení UDF get-json_object.

- Protože jednotlivá pole v dotazu vyžaduje znovu analyzování dotazu, ovlivňuje výkon.
- ZÍSKÁNÍ\_JSON_OBJECT() vrátí řetězec číselné pole. Pokud chcete převést to maticový podregistru, budete muset umožňuje nahradit hranatých závorek regulární výrazy ' ["a"] "a také volat rozdělit zobrazíte pole.


Z tohoto důvodu wikiwebu podregistru doporučuje používat json_tuple.  

### <a name="use-the-jsontuple-udf"></a>Použití JSON_TUPLE UDF

Jiné UDF poskytovanou podregistru nazývá [json_tuple](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-json_tuple) , která provádí půjde vám ještě snadněji [get_ json _object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object). Tento způsob zabírá sadu klíče a řetězec formátu JSON a vrátí řazené kolekce členů hodnot pomocí funkcí. Následující dotaz vrátí id studenta a třída z dokumentu JSON:

    SELECT q1.StudentId, q1.Grade
      FROM StudentsOneLine jt
      LATERAL VIEW JSON_TUPLE(jt.json_body, 'StudentId', 'Grade') q1
        AS StudentId, Grade;

Výstup tento skript v konzole podregistru:

![json_tuple UDF][image-hdi-hivejson-jsontuple]

JSON\_n-TICI syntaxí [boční zobrazení](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) v podregistru, které umožňuje json\_n-tici vytvořit virtuální tabulku použitím funkce UDT pro každý řádek původní tabulky.  Komplexní JSONs být moc nepraktické kvůli opakované použití boční zobrazení. Kromě toho JSON_TUPLE nemůže zpracovat vnořené JSONs.


###<a name="use-custom-serde"></a>Použití vlastní SerDe

SerDe je nejlepší volbou pro analýzu vnořené JSON dokumenty, umožňuje definice schématu JSON a použití schématu analyzovat dokumenty. V tomto kurzu se používáte některou z více Oblíbené SerDe, která byla vytvořena tak, že [rcongiu](https://github.com/rcongiu).

**Pokud chcete používat vlastní SerDe:**

1. Nainstalujte si [Java jihovýchodní Development Kit 7u55 JDK 1.7.0_55](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html#jdk-7u55-oth-JPR). Zvolte verze Windows X64 JDK, pokud se chystáte používat nasazení systému Windows HDInsight

    >[AZURE.WARNING] JDK 1.8 nefunguje u tohoto SerDe.

    Po dokončení instalace přidáte novou proměnnou uživatele:

    1. Na obrazovce Windows otevřete **zobrazení rozšířené nastavení systému** .
    2. Klikněte na tlačítko **proměnné**.  
    3. Přidáte že novou **JAVA_HOME** proměnnou přejdete **C:\Program Files\Java\jdk1.7.0_55** nebo tam, kde je vaše JDK nainstalovaný.

    ![Nastavení konfigurace správné hodnoty pro JDK][image-hdi-hivejson-jdk]

2. Instalace [Maven 3.3.1](http://mirror.olnevhost.net/pub/apache/maven/maven-3/3.3.1/binaries/apache-maven-3.3.1-bin.zip)

    Přidat do složky Koš path tak, že přejdete na ovládací prvek Panel--> Upravit systémové proměnné pro váš účet Environment proměnné. Následující snímek obrazovky se dozvíte, jak se to dělá.

    ![Nastavení Maven][image-hdi-hivejson-maven]

3. Klonovat projektu z [Podregistru JSON SerDe](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) github webu. Můžete to uděláte tak, že kliknete na tlačítko "Stáhnout Zip", jak je znázorněno níže snímek.

    ![Kopírování projektu][image-hdi-hivejson-serde]

4: Přejít do složky, kde jste stáhli tento balíčky a typ "mvn". To měli vytvořit potřebné sklenice soubory, které pak zkopírovat do clusteru.

5: přejděte na cílovou složku kořenové složce, které jste stáhli balíček. Nahrání souboru json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar vedoucí uzel clusteru. Můžu obvykle ji ve složce binární podregistru: C:\apps\dist\hive-0.13.0.2.1.11.0-2316\bin nebo podobným.

6: v řádku podregistru, zadejte "Přidat sklenice /path/to/json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar". Protože se v mém případě sklenice ve složce C:\apps\dist\hive-0.13.x\bin, můžete přímo přidám sklenice s názvem jak je ukázáno v následujícím příkladu:

    add jar json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar;

    ![Adding JAR to your project][image-hdi-hivejson-addjar]

Teď jste připraveni SerDe umožňuje spouštění dotazů proti JSON dokumentu.

Následující příkaz Vytvořit tabulku se definované schéma

    DROP TABLE json_table;
    CREATE EXTERNAL TABLE json_table (
      StudentId string,
      Grade int,
      StudentDetails array<struct<
          FirstName:string,
          LastName:string,
          YearJoined:int
          >
      >,
      StudentClassCollection array<struct<
          ClassId:string,
          ClassParticipation:string,
          ClassParticipationRank:string,
          Score:int,
          PerformedActivity:boolean
          >
      >
    ) ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe'
    LOCATION '/json/students';

Chcete-li zobrazit křestního jména a příjmení student

    SELECT StudentDetails.FirstName, StudentDetails.LastName FROM json_table;

Tady je výsledkem z konzoly podregistru.

![Dotaz SerDe 1][image-hdi-hivejson-serde_query1]

Chcete-li vypočítat součet skóre JSON dokumentu

    SELECT SUM(scores)
    FROM json_table jt
      lateral view explode(jt.StudentClassCollection.Score) collection as scores;

Nahoře query používá [boční zobrazení rozbalit](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) UDF rozbalte pole skóre tak, aby mohli sečtou.

Tady je výstup z konzoly podregistru.

![SerDe dotazu 2][image-hdi-hivejson-serde_query2]

Najít, které má daný student dosáhne více než 80 bodů předměty vyberte  
      Jt. StudentClassCollection.ClassId z json_table jt boční zobrazení Rozbalit (jt. Kolekce StudentClassCollection.Score) jako skóre kde skóre > 80;

Nahoře query vrátí matici podregistru na rozdíl od get\_json\_objekt, který vrátí hodnotu typu string.

![Dotaz SerDe 3][image-hdi-hivejson-serde_query3]

Pokud chcete skil poškozený JSON, a jak je uvedeno v této SerDe [stránky wikiwebu](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) dosáhnete, zadáním následující kód:  

    ALTER TABLE json_table SET SERDEPROPERTIES ( "ignore.malformed.json" = "true");




##<a name="summary"></a>Souhrn
Na závěr typ JSON operátor v podregistru, které zvolíte, závisí na vaší situaci. Pokud máte jednoduchý dokument JSON a jedno pole vyhledat v – stačí se rozhodnete sdělit nám get podregistru UDF\_json\_objektu. Pokud máte víc než jednu klávesy se šipkami vyhledat můžete json_tuple. Pokud máte otevřený vnořený dokument, měli byste použít JSON SerDe.

Další články související najdete v tématu

- [Pomocí podregistru a HiveQL Hadoop v HDInsight analyzovat ukázkový soubor log4j Apache](hdinsight-use-hive.md)
- [Analýza dat zpoždění letů pomocí podregistru v HDInsight](hdinsight-analyze-flight-delay-data.md)
- [Analýza dat Twitter pomocí podregistru v HDInsight](hdinsight-analyze-twitter-data.md)
- [Spuštění úlohy Hadoop pomocí DocumentDB a HDInsight](../documentdb/documentdb-run-hadoop-with-hdinsight.md)

[hdinsight-python]: hdinsight-python.md

[image-hdi-hivejson-flatten]: ./media/hdinsight-using-json-in-hive/flatten.png
[image-hdi-hivejson-getjsonobject]: ./media/hdinsight-using-json-in-hive/getjsonobject.png
[image-hdi-hivejson-jsontuple]: ./media/hdinsight-using-json-in-hive/jsontuple.png
[image-hdi-hivejson-jdk]: ./media/hdinsight-using-json-in-hive/jdk.png
[image-hdi-hivejson-maven]: ./media/hdinsight-using-json-in-hive/maven.png
[image-hdi-hivejson-serde]: ./media/hdinsight-using-json-in-hive/serde.png
[image-hdi-hivejson-addjar]: ./media/hdinsight-using-json-in-hive/addjar.png
[image-hdi-hivejson-serde_query1]: ./media/hdinsight-using-json-in-hive/serde_query1.png
[image-hdi-hivejson-serde_query2]: ./media/hdinsight-using-json-in-hive/serde_query2.png
[image-hdi-hivejson-serde_query3]: ./media/hdinsight-using-json-in-hive/serde_query3.png
[image-hdi-hivejson-serde_result]: ./media/hdinsight-using-json-in-hive/serde_result.png
