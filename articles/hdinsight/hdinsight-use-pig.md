<properties
   pageTitle="Použití Hadoop Prasátko v HDInsight | Microsoft Azure"
   description="Naučte se používat Prasátko s Hadoop na HDInsight."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/14/2016"
   ms.author="larryfr"/>

# <a name="use-pig-with-hadoop-on-hdinsight"></a>Použití Prasátko s Hadoop na HDInsight

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

[Prasátko Apache](http://pig.apache.org/) je platforma k vytváření aplikací pro Hadoop pomocí postupu jazyka jmenoval *Prasátko latinky*. Prasátko je alternativy Java pro vytváření *MapReduce* řešení a je součástí Azure HDInsight.

V tomto článku se dozvíte, jak můžete Prasátko s HDInsight.

##<a id="why"></a>Proč používat Prasátko?

Pomocí pouze mapy a zmenšení funkce mezi úkoly zpracování dat pomocí MapReduce v Hadoop je implementace logiky zpracování. Zpracování složitých často máte zpracování rozdělí více MapReduce operací, které jsou zřetězené můžete dosáhnout požadované výsledky.

Místo vynucení můžete použít jenom mapování a zmenšení funkce Prasátko umožňuje definovat zpracování jako řadu transformace, které prochází data k vytvoření požadovaného výstupního.

Jazyk Prasátko latinky umožňuje popisují toku dat z nezpracovanými zadání vstupních hodnot pomocí jednoho nebo více transformace k vytváření požadované výstupu. Prasátko latinky programy podle tohoto obecného vzorku:

- **Načtení**: Číst data, která chcete pracovat ze systému souborů
- **Transformace**: manipulaci s nimi data
- **Výpis nebo storu**: výstupní data na obrazovku nebo je uložíte pro zpracování

Latinka Prasátko taky podporuje funkcí definovaných uživatelem (UDF), které umožňuje vyvolat externí součásti implementující logiku, která je těžké modelu v Prasátko (latinka).

Další informace o Prasátko latinka najdete v článku [Prasátko latinka odkaz ruční 1](http://pig.apache.org/docs/r0.7.0/piglatin_ref1.html) a [2 Ruční Prasátko latinka odkaz](http://pig.apache.org/docs/r0.7.0/piglatin_ref2.html).

Příklad použití funkce definované uživatelem s Prasátko naleznete v tématu tyto dokumenty:

* [Použití DataFu s Prasátko v HDInsight](hdinsight-hadoop-use-pig-datafu-udf.md) - DataFu je sada vedené Apache užitečné funkce definované uživatelem

* [Použití Python s Prasátko a podregistru v HDInsight](hdinsight-python.md)

* [Pomocí C# podregistru a Prasátko v HDInsight](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

##<a id="data"></a>Informace o ukázkových dat

Tento příklad používá *log4j* ukázkový soubor, který je uložen v **/example/data/sample.log** kontejner úložiště objektů blob. Každý protokolu souboru se skládá z řádku pole, která obsahuje `[LOG LEVEL]` pole zobrazíte typ a závažnosti, například:

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

Úroveň protokolování v předchozím příkladu, je chyba.

> [AZURE.NOTE] Můžete taky vytvořit soubor log4j pomocí nástroje [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) protokolování a pak nahrajte soubor do svého objektů blob. Postupujte podle pokynů uvedených v tématu [Nahrání dat HDInsight](hdinsight-upload-data.md) . Další informace o použití v Azure úložiště objektů BLOB s Hdinsightu najdete v článku [Použití úložiště objektů Blob Azure s HDInsight](hdinsight-hadoop-use-blob-storage.md).

Ukázková data se ukládají v úložišti objektů Blob Azure HDInsight používá jako výchozí systém souborů Hadoop clusterů. HDInsight přístup k souborům uloženým v objektů BLOB pomocí předponou **wasb** . Například pro přístup k souboru sample.log, použijete tuto syntaxi:

    wasbs:///example/data/sample.log

Protože WASB je výchozí úložiště pro HDInsight, dostanete soubor pomocí **/example/data/sample.log** z Prasátko (latinka).

> [AZURE.NOTE] Syntaxe, **wasbs: / / /**, umožňuje přístup k souborům uloženým v kontejneru výchozí úložiště pro svůj cluster HDInsight. Pokud jste zadali účty další úložiště až zřízení svůj cluster a chcete získat přístup k souborům uloženým v těchto účtů, lze přístup k datům zadáním kontejneru a jeho název účtu adresu, třeba: **wasbs://mycontainer@mystorage.blob.core.windows.net/example/data/sample.log**.


##<a id="job"></a>Informace o projektu Ukázka

Následující úlohy Prasátko latinka načte **sample.log** soubor z výchozí úložiště pro svůj cluster HDInsight. Pak provede řadu transformace, jejichž výsledkem je počet kolikrát došlo k každou úroveň protokolování jednotkou vstupních dat.. Výsledky jsou dumpingové do STDOUT.

    LOGS = LOAD 'wasbs:///example/data/sample.log';
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
    RESULT = order FREQUENCIES by COUNT desc;
    DUMP RESULT;

Následující obrázek znázorňuje přehled každé transformace slouží k datům.

![Grafické znázornění transformací][image-hdi-pig-data-transformation]

##<a id="run"></a>Spuštění úlohy Prasátko (latinka)

HDInsight poběží Prasátko latinka úlohy pomocí různých způsobů. Rozhodnout, jaký způsob je pro vás nejlepší, použijte následující tabulku a pak kliknutím na odkaz informace.

| **Pomocí tohoto příkazu** , pokud chcete...                                   | .. .an **interaktivní** prostředí | ... **dávkové** zpracování | .. .with **clusteru operační systém** | .. .from tohoto **klienta operační systém** |
|:--------------------------------------------------------------|:---------------------------:|:-----------------------:|:------------------------------------------|:-----------------------------------------|
| [SSH](hdinsight-hadoop-use-pig-ssh.md)                        |              ✔              |            ✔            | Linux                                     | Linux, Unix, systému Mac OS X nebo systému Windows        |
| [Otočení](hdinsight-hadoop-use-pig-curl.md)                      |           &nbsp;            |            ✔            | Linux nebo Windows                          | Linux, Unix, systému Mac OS X nebo systému Windows        |
| [SDK .NET pro Hadoop](hdinsight-hadoop-use-pig-dotnet-sdk.md) |           &nbsp;            |            ✔            | Linux nebo Windows                          | Windows (prozatím)                        |
| [Prostředí Windows PowerShell](hdinsight-hadoop-use-pig-powershell.md)  |           &nbsp;            |            ✔            | Linux nebo Windows                          | Windows                                  |
| [Vzdálená plocha](hdinsight-hadoop-use-pig-remote-desktop.md)  |              ✔              |            ✔            | Windows                                   | Windows                                  |


## <a name="running-pig-jobs-on-azure-hdinsight-using-on-premises-sql-server-integration-services"></a>Spuštěna Prasátko úlohy Azure Hdinsightu pomocí místní SQL Server Integration Services

Integrace služby SSIS (SQL Server) můžete taky úlohu Prasátko. Azure Feature Pack pro SSIS obsahuje následující součásti, které spolupracují s Prasátko úlohy na HDInsight.


- [Azure HDInsight Prasátko úkolu][pigtask]
- [Azure předplatné Connection Manager][connectionmanager]


Další informace o Azure Feature Pack pro SSIS [tady][ssispack].


##<a id="nextsteps"></a>Další kroky

Teď, když jste se naučili použití Prasátko s HDInsight, použijte následující odkazy, které můžete prozkoumat další způsoby, jak pracovat s Azure HDInsight.

* [Odeslání dat do HDInsight][hdinsight-upload-data]
* [Použití podregistru s HDInsight][hdinsight-use-hive]
* [Použití Sqoop s HDInsight](hdinsight-use-sqoop.md)
* [Použití Oozie s HDInsight](hdinsight-use-oozie.md)
* [Použití MapReduce úlohy s HDInsight][hdinsight-use-mapreduce]

[check]: ./media/hdinsight-use-pig/hdi.checkmark.png

[apachepig-home]: http://pig.apache.org/
[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html
[curl]: http://curl.haxx.se/
[pigtask]: http://msdn.microsoft.com/library/mt146781(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx

[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: ../hdinsight-get-started.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md#mapreduce-sdk

[Powershell-install-configure]: ../powershell-install-configure.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx

[image-hdi-log4j-sample]: ./media/hdinsight-use-pig/HDI.wholesamplefile.png
[image-hdi-pig-data-transformation]: ./media/hdinsight-use-pig/HDI.DataTransformation.gif
[image-hdi-pig-powershell]: ./media/hdinsight-use-pig/hdi.pig.powershell.png
[image-hdi-pig-architecture]: ./media/hdinsight-use-pig/HDI.Pig.Architecture.png
