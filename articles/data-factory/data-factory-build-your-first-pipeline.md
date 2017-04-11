<properties
    pageTitle="Kurz Factory dat: první datového kanálu | Microsoft Azure"
    description="Tento kurz Azure Data Factory ukazuje, jak můžete vytvořit a naplánovat si data factory zpracovávající data pomocí skriptu podregistru Hadoop clusteru."
    services="data-factory"
    keywords="Azure dat factory kurzu hadoop clusteru hadoop podregistru"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-first-pipeline-to-process-data-using-hadoop-cluster"></a>Kurz: Vytvoření první pipeline na zpracování dat pomocí Hadoop obrázku 
> [AZURE.SELECTOR]
- [Přehled a požadavky](data-factory-build-your-first-pipeline.md)
- [Azure portálu](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [Prostředí PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Správce prostředků šablony](data-factory-build-your-first-pipeline-using-arm.md)
- [ROZHRANÍ REST API](data-factory-build-your-first-pipeline-using-rest-api.md)

V tomto kurzu vytvoříte výrobce první Azure dat pomocí datového kanálu zpracovávající data spuštěním podregistru skriptu clusteru Azure HDInsight (Hadoop). 

> [AZURE.NOTE] Tento článek neposkytuje přehled Azure Data Factory. Přehled služby najdete v článku [Úvod k Azure Data Factory](data-factory-introduction.md). V tématu [cesta výuky Data Factory](https://azure.microsoft.com/documentation/learning-paths/data-factory/) doporučený postup procházení náš obsahu se naučit používat Data Factory.

## <a name="whats-covered-in-this-tutorial"></a>Co je součástí v tomto kurzu? 
**Azure Data Factory** umožňuje vytváření dat **pohybu** a úkoly **zpracování** dat jako založených na datech pracovní postupy (nazývané také datové kanály). Naučíte se vytvářet první aktivity kanálem k odesílání zpráv s zpracování dat (nebo transformace) data. Tuto aktivitu používá clusteru HDInsight Hadoop transformace a analyzovat protokoly web vzorku.  

V tomto kurzu proveďte následující kroky:

1.  Vytvoření **data factory**. Factory dat může obsahovat jeden nebo víc dat potrubí tato data přesunout a proces. 
2.  Vytvoření **propojené služby**. Vytvoříte propojené služby propojení úložiště dat nebo službu výpočetním data factory. Úložiště dat jako je třeba úložiště Azure obsahuje vstupní a výstupní data aktivity v kanálu. Výpočetní služby, třeba HDInsight Hadoop clusteru procesů a transformací data.    
3.  Vytvoření vstupní a výstupní **datové sady**. Zadávání datovou sadu představuje vstupní aktivitu v kanálu a datovou sadu výstup představuje výstup aktivity.
3.  Vytvoření **kanálu**. Potrubí můžete mít minimálně jednu aktivity (příklady: Kopírovat aktivity, HDInsight podregistru). V tomto příkladu HDInsight podregistru aktivity, které se spouští skript podregistru clusteru HDInsight Hadoop. Skript nejprve vytvoří tabulku odkazující na data protokolu nezpracovanými web uložených v úložišti objektů blob Azure a oddíly původní data podle roku a měsíce.

    Existují dva typy činností nepodporuje Azure Data Factory. Jsou: [data pohyb aktivity](data-factory-data-movement-activities.md) a [aktivity transformace dat](data-factory-data-transformation-activities.md). Existuje jenom jednu datovou pohyb aktivity, kopírovat aktivity. V tomto kurzu nepoužíváte aktivity kopírovat. Návod k použití aktivity kopírovat, najdete v článku [kurz: kopírování dat z Azure objektů blob Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). HDInsight podregistru činnost, kterou používáte v tomto kurzu je jednou z činností transformace dat nepodporuje Data Factory.  
 
Tady je **zobrazení diagramu** závodu ukázkových dat, který vytvoříte v tomto kurzu. 

![Zobrazení diagramu v kurzu Data Factory](./media/data-factory-build-your-first-pipeline/data-factory-tutorial-diagram-view.png)

V tomto kurzu obsahuje **inputdata** složek kontejneru objektů blob Azure **adfgetstarted** jednoho souboru s názvem input.log. Tento soubor protokolu má položky z tři měsíce: leden, Únorový a dne 2016. Tady je ukázka řádků pro každý měsíc ve vstupním souboru. 

    2016-01-01,02:01:09,SAMPLEWEBSITE,GET,/blogposts/mvc4/step2.png,X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,53175,871 
    2016-02-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
    2016-03-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871

Při zpracování souboru kanálem s HDInsight podregistru aktivitou aktivity spustí skript podregistru clusteru HDInsight, že oddíly zadané data podle roku a měsíce. Skript vytvoří tři výstup složky, které obsahují soubor s položky z každý měsíc.  

    adfgetstarted/partitioneddata/year=2016/month=1/000000_0
    adfgetstarted/partitioneddata/year=2016/month=2/000000_0
    adfgetstarted/partitioneddata/year=2016/month=3/000000_0

Z výběru řádků nahoře, první z nich (plus pár 2016 01 01) je došlo k zápisu souboru 000000_0 v měsíci = 1 složky. Podobně druhý je došlo k zápisu soubor v měsíci = 2 složky a třetí jednu je došlo k zápisu soubor v měsíci = 3 složky.  


## <a name="pre-requisites"></a>Předpoklady
Před zahájením tohoto kurzu, musíte mít následující požadavky:

1.  **Azure předplatné** – Pokud nemáte předplatné Azure, můžete vytvořit bezplatnou zkušební účet v jenom pár minut. V tématu [Bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/) na tom, jak můžete získat bezplatnou zkušební účet.

2.  **Azure úložiště** – použití účtu Azure úložiště pro ukládání data v tomto kurzu. Pokud nemáte účet Azure úložiště, podívejte se na článek [Vytvoření účtu úložiště](../storage/storage-create-storage-account.md#create-a-storage-account) . Po vytvoření účtu úložiště, poznamenejte **název účtu** a **přístupové klávesy**. V tématu [zobrazení, kopírovat a přístupových kláves z verze regenerovat úložiště](../storage/storage-create-storage-account.md#view-and-copy-storage-access-keys). 

### <a name="upload-files-to-azure-storage-for-the-tutorial"></a>Nahrání souborů na Azure úložiště pro kurz
Než s kurzem začnete, budete muset Příprava účtu úložiště Azure s ukázkové soubory kurzu.

1. Nahrání souboru dotazu podregistru (HQL) do složky **skript** kontejneru objektů blob **adfgetstarted** .
2. Zadávání soubor nahrajte **inputdata** složek kontejneru objektů blob **adfgetstarted** . 

#### <a name="create-hql-script-file"></a>Vytvořte soubor skriptu HQL 

1. Spuštění **programu Poznámkový blok** a vložte následující HQL skriptu. Tento skript podregistru vytvoří dvě tabulky: **WebLogsRaw** a **WebLogsPartitioned**. V nabídce klikněte na **soubor** a vyberte příkaz **Uložit jako**. Přejděte do složky **C:\adfgetstarted** na pevném disku. Vyberte * *všechny soubory (*.*) **pro** Uložit jako zadejte** pole. Zadejte **partitionweblogs.hql** pro **název souboru**. Potvrďte, že **kódování** pole v dolní části dialogového okna je nastavena na **ANSI**. Pokud ne, nastavte **ANSI **.  

        set hive.exec.dynamic.partition.mode=nonstrict;
        
        DROP TABLE IF EXISTS WebLogsRaw; 
        CREATE TABLE WebLogsRaw (
          date  date,
          time  string,
          ssitename string,
          csmethod  string,
          csuristem  string,
          csuriquery string,
          sport int,
          susername string,
          cipcsUserAgent string,
          csCookie string,
          csReferer string,
          cshost  string,
          scstatus  int,
          scsubstatus  int,
          scwin32status  int,
          scbytes int,
          csbytes int,
          timetaken int
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        LINES TERMINATED BY '\n' 
        tblproperties ("skip.header.line.count"="2");
        
        LOAD DATA INPATH '${hiveconf:inputtable}' OVERWRITE INTO TABLE WebLogsRaw;
        
        DROP TABLE IF EXISTS WebLogsPartitioned ; 
        create external table WebLogsPartitioned (  
          date  date,
          time  string,
          ssitename string,
          csmethod  string,
          csuristem  string,
          csuriquery string,
          sport int,
          susername string,
          cipcsUserAgent string,
          csCookie string,
          csReferer string,
          cshost  string,
          scstatus  int,
          scsubstatus  int,
          scwin32status  int,
          scbytes int,
          csbytes int,
          timetaken int
        )
        partitioned by ( year int, month int)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
        STORED AS TEXTFILE 
        LOCATION '${hiveconf:partitionedtable}';
        
        INSERT INTO TABLE WebLogsPartitioned  PARTITION( year , month) 
        SELECT
          date,
          time,
          ssitename,
          csmethod,
          csuristem,
          csuriquery,
          sport,
          susername,
          cipcsUserAgent,
          csCookie,
          csReferer,
          cshost,
          scstatus,
          scsubstatus,
          scwin32status,
          scbytes,
          csbytes,
          timetaken,
          year(date),
          month(date)
        FROM WebLogsRaw

Za běhu podregistru aktivity v kanálu Data Factory předává hodnoty **inputtable** a parametrech **partitionedtable** uvedeno v následující ukázce:  

        "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
        "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"

Název účtu Azure úložiště je **storageaccountname** .
 
#### <a name="create-a-sample-input-file"></a>Vytvoření souboru ukázková vstupní
Poznámkový blok, vytvořte do souboru nazvaného **input.log** **c:\adfgetstarted** následující obsahu: 

    #Software: Microsoft Internet Information Services 8.0
    #Fields: date time s-sitename cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step4.png X-ARR-LOG-ID=4bea5b3d-8ac9-46c9-9b8c-ec3e9500cbea 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 72177 871 47
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step5.png X-ARR-LOG-ID=9b0c14b1-434d-495a-9b0d-46775194257b 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 37931 871 32
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step6.png X-ARR-LOG-ID=f99cff81-ec38-4a13-b2fe-21b10adca996 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 27146 871 47
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step1.png X-ARR-LOG-ID=af94d3b4-8e05-4871-82c4-638f866d4e83 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 66259 871 140
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47

#### <a name="upload-input-file-and-hql-file-to-your-azure-blob-storage"></a>Nahrání souboru zadávání a HQL soubor k úložišti objektů Blob Azure

Tato část obsahuje pokyny k použití **AzCopy** nástroj Kopírovat input.log a partitionweblogs.hql soubory k úložišti objektů Blob Azure. Můžete použít libovolný nástroj podle svého výběru (například: [Microsoft Azure úložiště Explorer](http://storageexplorer.com/), [CloudXPlorer softwarem ClumsyLeaf](http://clumsyleaf.com/products/cloudxplorer) k provedení tohoto úkolu.   
     
1. Stáhněte si [nejnovější verzi **AzCopy**](http://aka.ms/downloadazcopy)nebo [nejnovější verze preview](http://aka.ms/downloadazcopypr). Článek [použití AzCopy](../storage/storage-use-azcopy.md) najdete v tématu Další informace o používání nástroje.
2. Přejděte do složky c:\adfgetstarted a spusťte tento příkaz: 

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\AzCopy" /Source:. /Dest:https://<storageaccountname>.blob.core.windows.net/adfgetstarted/inputdata /DestKey:<storageaccesskey>  /Pattern:input.log

    Tento příkaz odešlou **input.log** soubor účtu úložiště (**adfgetstarted** kontejner a **inputdata** složky). Nahraďte **storageaccountname** s název účtu úložiště a **storageaccesskey** přístupová klávesa úložiště.

    > [AZURE.NOTE] Tento příkaz vytvoří kontejner **adfgetstarted** v úložišti objektů Blob Azure a zkopíruje **input.log** soubor z místního disku ke složce **inputdata** v kontejneru. 
    
3. Po úspěšném nahrání souboru se zobrazí podobá následující z AzCopy výstup.
    
        Finished 1 of total 1 file(s).
        [2015/12/16 23:07:33] Transfer summary:
        -----------------
        Total files transferred: 1
        Transfer successfully:   1
        Transfer skipped:        0
        Transfer failed:         0
        Elapsed time:            00.00:00:01
5. Spusťte tento příkaz nahrát soubor **partitionweblogs.hql** **skript** složek kontejneru **adfgetstarted** . Tady je příkaz: 
    
        AzCopy /Source:. /Dest:https://<storageaccountname>.blob.core.windows.net/adfgetstarted/script /DestKey:<storageaccesskey>  /Pattern:partitionweblogs.hql

Jste dokončili požadavky. Můžete vytvořit factory dat pomocí jedné z následujících způsobů. Klikněte na jednu z karty začátku nebo na následující odkazy provádět kurzu. 

- [Azure portálu](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [Prostředí PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Správce prostředků šablony](data-factory-build-your-first-pipeline-using-arm.md)
- [ROZHRANÍ REST API](data-factory-build-your-first-pipeline-using-rest-api.md)