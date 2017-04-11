<properties
    pageTitle="Zjistěte, co je podregistru a jak používat HiveQL | Microsoft Azure"
    description="Přečtěte si o Apache podregistru a můžete ho Hadoop v HDInsight. Výběr způsobu spuštění podregistru úlohy a analýza ukázkový soubor log4j Apache pomocí HiveQL."
    keywords="hiveql, co je podregistru"
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
    ms.date="09/19/2016"
    ms.author="larryfr"/>

# <a name="use-hive-and-hiveql-with-hadoop-in-hdinsight-to-analyze-a-sample-apache-log4j-file"></a>Pomocí podregistru a HiveQL Hadoop v HDInsight analyzovat ukázkový soubor log4j Apache

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]


V tomto kurzu se informace o použití Apache podregistru v Hadoop na HDInsight a zvolte k tomu podregistru práce. Budete taky pročíst HiveQL a jak analyzovat ukázkový soubor log4j Apache.

##<a id="why"></a>Co je podregistru a proč používat?
[Apache podregistru](http://hive.apache.org/) je data warehouse systému Hadoop, což umožňuje souhrnu dat dotazování a analýza dat pomocí HiveQL (což je dotazovací jazyk podobný SQL). Podregistru mohou sloužit k interaktivně procházet data nebo k vytvoření opakovaně použitelného dávkové zpracování úloh.

Podregistru umožňuje struktury projektu na převážně Nestrukturovaná data. Po definování struktury slouží k vytvoření dotazu tato data bez znalosti Java nebo MapReduce podregistru. **HiveQL** (dotazovací jazyk podregistru) umožňuje psát dotazy s příkazy, které jsou podobné T-SQL.

Podregistru rozumí jak pracovat s strukturovaná a částečně strukturovaná data, třeba soubory ve formátu RTF kde jsou pole odděleny zvláštní znaky. Podregistru taky podporuje vlastní **serializer/deserializers (SerDe)** pro složitější nebo nepravidelně strukturovaná data. Další informace najdete v tématu [jak používat vlastní SerDe JSON s HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight.aspx).

## <a name="user-defined-functions-udf"></a>Funkce definované uživatelem (UDF)

Podregistru lze také rozšířit pomocí **funkcí definovaných uživatelem (UDF)**. Uživatelem definovanou FUNKCI umožňuje implementace funkcí nebo použití logických operátorů, který není modelovat snadno HiveQL. Příklad použití funkce definované uživatelem s podregistru najdete v těchto článcích:

* [Použití jazyka Java uživatele definované funkce se podregistru](hdinsight-hadoop-hive-java-udf.md)

* [Prostřednictvím Python podregistru Prasátko v HDInsight](hdinsight-python.md)

* [Pomocí C# podregistru a Prasátko v HDInsight](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

* [Jak přidat vlastní podregistru uživatelem definovanou FUNKCI HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

* [Příklad vlastního podregistru UDF převést formátů data a času na časové razítko podregistru](https://github.com/Azure-Samples/hdinsight-java-hive-udf)

## <a name="hive-internal-tables-vs-external-tables"></a>Podregistru interní tabulek a externích tabulek

Existuje několik věcí, které byste měli vědět o tabulku podregistru interní a externí tabulka:

- Příkaz **CREATE TABLE** vytvoří vnitřní tabulku. Datový soubor nacházet v kontejneru výchozí.
- Příkaz **CREATE TABLE** přesune datový soubor /hive/sklad/<TableName> složky.
- Příkaz **Vytvořit externí tabulka** vytvoří externí tabulku. Datový soubor mohou být umístěny mimo výchozí kontejner.
- Příkaz **CREATE TABLE externích** nepřesune datového souboru.
- Příkaz **Vytvořit externí tabulka** neumožňuje všech složek v umístění. Toto je důvod, proč kurzu vytvoří kopii souboru sample.log.

Další informace najdete v tématu [HDInsight: podregistru interní a externí tabulky Úvod][cindygross-hive-tables].


##<a id="data"></a>Informace o ukázkových dat souboru log4j Apache

Tento příklad používá *log4j* ukázkový soubor, který je uložen v **/example/data/sample.log** kontejner úložiště objektů blob. Každý protokolu souboru se skládá z řádku pole, která obsahuje `[LOG LEVEL]` pole zobrazíte typ a závažnosti, například:

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

Úroveň protokolování v předchozím příkladu, je chyba.

> [AZURE.NOTE] Můžete taky vytvořit soubor log4j pomocí nástroje [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) protokolování a pak nahrajte soubor do kontejneru objektů blob. Postupujte podle pokynů uvedených v tématu [Nahrání dat HDInsight](hdinsight-upload-data.md) . Další informace o použití úložiště objektů Blob Azure s Hdinsightu najdete v článku [Použití úložiště objektů Blob Azure s HDInsight](hdinsight-hadoop-use-blob-storage.md).

Ukázková data se ukládají v úložišti objektů Blob Azure HDInsight používá jako výchozí systém souborů. HDInsight přístup k souborům uloženým v objektů BLOB pomocí předponou **wasb** . Například pro přístup k souboru sample.log, použijete tuto syntaxi:

    wasbs:///example/data/sample.log

Úložiště objektů Blob Azure je výchozí úložiště pro HDInsight, a proto dostanete soubor pomocí **/example/data/sample.log** z HiveQL.

> [AZURE.NOTE] Syntaxe, **wasbs: / / /**, se používá pro přístup k souborům uloženým v kontejneru výchozí úložiště pro svůj cluster HDInsight. Pokud jste zadali účty další úložiště až zřízení svůj cluster a chcete získat přístup k souborům uloženým v těchto účtů, lze přístup k datům zadáním kontejneru a jeho název účtu adresu, například **wasbs://mycontainer@mystorage.blob.core.windows.net/example/data/sample.log**.

##<a id="job"></a>Ukázka úlohy: Project sloupců do s oddělovači dat

Následující příkazy HiveQL bude projekt sloupců do s oddělovači daty uloženými v **wasbs: / / / Příklad/dat** adresáře:

    set hive.execution.engine=tez;
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

V předchozím příkladu výkazy HiveQL provádět následující akce:

* __Nastavení hive.execution.engine=tez;__: Nastaví modul spuštění používat Tez. Použití Tez namísto MapReduce můžete poskytnout zvýšení výkonu dotazu. Další informace o Tez naleznete v části [Použití Apache Tez pro zvýšení výkonu](#usetez) .

    > [AZURE.NOTE] Tento příkaz je pouze nutné v případě pomocí serveru s Windows HDInsight clusteru; Tez je výchozí modul spuštění na základě Linux HDInsight.

* **PŘETAŽENÍ tabulky**: Odstraní tabulku a datového souboru, pokud již existuje v tabulce.
* **Vytvořit externí tabulka**: vytvoří nová tabulka **externí** v podregistru. Externí tabulky pouze ukládat definice tabulky v podregistru; data ještě zbývá v původním umístění a v původním formátu.
* **Formát řádku**: říká podregistru formátování data. V tomto případě polí v jednotlivých protokolu oddělená mezerou.
* **ULOŽENÝ jako textový soubor umístění**: říká podregistru, kde jsou data uložená (příklad/datového adresáře) a je uložená jako text. Data lze v souboru nebo rozšířit mezi více souborů v rámci adresáře.
* **Vyberte**: slouží k výběru počet řádků místo, kam sloupec **t4** obsahuje hodnotu **[Chyba]**. Protože jsou tři řádky, které obsahují hodnotu to by vrátit hodnotu **3** .
* **INPUT__FILE__NAME jako "%.log"** - říká, která by měla pouze vrácených data ze souborů, které končí na podregistru. protokolu. Omezuje hledání sample.log soubor, který obsahuje data, zůstane vracet dat z jiných příkladu datové soubory, které se neshodují schéma, které byla definována.

> [AZURE.NOTE] Externí tabulky by měl být vyvolají očekáváte podkladová data mají být aktualizovány pomocí externího zdroje, například proces nahrávání automatické dat nebo jinou MapReduce operací a chcete, aby podregistru dotazů použít nejnovější data.
>
> Uvolnění externí tabulce znamená **nelze** odstranit data, pouze slouží k odstranění definice tabulky.

Po vytvoření externí tabulka, následující příkazy slouží k vytvoření **interního** tabulky.

    set hive.execution.engine=tez;
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs
    SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';

Tyto příkazy provádět následující akce:

* **Vytvoření tabulky IF NOT EXISTS**: vytvoří tabulku, pokud už neexistuje. Protože se nepoužívá **externí** klíčové slovo, toto je vnitřní tabulku, která je uložena v datový sklad podregistru a úplně spravuje podregistru.
* **ULOŽENÝ jako ORC**: jsou uložená data ve formátu optimalizované řádku sloupcovém (ORC). Jedná se o formát vysoce Optimalizovaná a efektivně pro ukládání dat podregistru.
* PŘEPSAT **vložení... Vyberte**: vybere řádků z tabulky **log4jLogs** , který obsahuje **[Chyba]**a vloží data do tabulky **errorLogs** .

> [AZURE.NOTE] Na rozdíl od externích tabulek přetažením vnitřní tabulku odstraněny také podkladová data.

##<a id="usetez"></a>Použití Apache Tez pro zvýšení výkonu

[Apache Tez](http://tez.apache.org) je rámec, který umožňuje náročné aplikace dat, například podregistru, spusťte mnohem efektivněji ve velkém měřítku. V nejnovější verzi HDInsight podregistru podporuje spuštěna Tez. Tez aktivované ve výchozím nastavení clusterů na základě Linux HDInsight.

> [AZURE.NOTE] Tez vypnutá aktuálně ve výchozím nastavení serveru s Windows HDInsight clusterů a musí být povolený. Umožní využít výhod Tez podregistru dotazu nastavte následující hodnotu:
>
> ```set hive.execution.engine=tez;```
>
>To lze odeslat na základě za dotazu tak, že zaškrtnete na začátku tohoto dotazu. Můžete také nastavit tuto variantu na ve výchozím nastavení clusteru nastavením hodnotu konfigurace při vytváření clusteru. Další informace najdete v [Zřizování clusterů HDInsight](hdinsight-provision-clusters.md).

[Podregistru na dokumentech návrh Tez](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez) obsahují počet podrobnosti o implementaci výběru a ladění konfigurace.

Ladění úlohy spustili pomocí Tez, HDInsight k dispozici následující webu uživatelského rozhraní, které umožňují zobrazit podrobnosti o Tez úlohy:

* [Použití rozhraní Tez na serveru s Windows HDInsight](hdinsight-debug-tez-ui.md)

* [Použití zobrazení Ambari Tez na základě Linux HDInsight](hdinsight-debug-ambari-tez-view.md)

##<a id="run"></a>Výběr způsobu spuštění úlohy HiveQL

HDInsight by umožnit spuštění úlohy HiveQL celou řadou způsobů. Rozhodnout, jaký způsob je pro vás nejlepší, použijte následující tabulku a pak kliknutím na odkaz informace.

| **Pomocí tohoto příkazu** Pokud chcete...                                                     | .. .an **interaktivní** prostředí | ... **dávkové** zpracování | .. .with **clusteru operační systém** | .. .from tohoto **klienta operační systém** |
|:--------------------------------------------------------------------------------|:---------------------------:|:-----------------------:|:------------------------------------------|:-----------------------------------------|
| [Zobrazení podregistru](hdinsight-hadoop-use-hive-ambari-view.md) | ✔ | ✔ | Linux | Všechny (využitím prohlížeče) |
| [Příkaz beeline (z relaci SSH)](hdinsight-hadoop-use-hive-beeline.md)                                         |              ✔              |            ✔            | Linux                                     | Linux, Unix, systému Mac OS X nebo systému Windows        |
| [Příkaz podregistru (z relaci SSH)](hdinsight-hadoop-use-hive-ssh.md)                                         |              ✔              |            ✔            | Linux                                     | Linux, Unix, systému Mac OS X nebo systému Windows        |
| [Otočení](hdinsight-hadoop-use-hive-curl.md)                                       |           &nbsp;            |            ✔            | Linux nebo Windows                          | Linux, Unix, systému Mac OS X nebo systému Windows        |
| [Konzoly dotazu](hdinsight-hadoop-use-hive-query-console.md)                     |           &nbsp;            |            ✔            | Windows                                   | Všechny (využitím prohlížeče)                            |
| [HDInsight nástroje for Visual Studio](hdinsight-hadoop-use-hive-visual-studio.md) |           &nbsp;            |            ✔            | Linux nebo Windows                          | Windows                                  |
| [Prostředí Windows PowerShell](hdinsight-hadoop-use-hive-powershell.md)                   |           &nbsp;            |            ✔            | Linux nebo Windows                          | Windows                                  |
| [Vzdálená plocha](hdinsight-hadoop-use-hive-remote-desktop.md)                   |              ✔              |            ✔            | Windows                                   | Windows                                  |

## <a name="running-hive-jobs-on-azure-hdinsight-using-on-premises-sql-server-integration-services"></a>Spuštěna podregistru úlohy Azure Hdinsightu pomocí místní SQL Server Integration Services

Integrace služby SSIS (SQL Server) můžete taky spustit podregistru úlohu. Azure Feature Pack pro SSIS obsahuje následující součásti, které spolupracují s podregistru úlohy na HDInsight.


- [Azure HDInsight podregistru úkolu][hivetask]
- [Azure předplatné Connection Manager][connectionmanager]


Další informace o Azure Feature Pack pro SSIS [tady][ssispack].


##<a id="nextsteps"></a>Další kroky

Teď, když jste se naučili novinky podregistru a jak ho použít s Hadoop v HDInsight, použijte následující odkazy, které můžete prozkoumat další způsoby, jak pracovat s Azure HDInsight.


- [Odeslání dat do HDInsight][hdinsight-upload-data]
- [Použití Prasátko s HDInsight][hdinsight-use-pig]
- [Použití Sqoop s HDInsight](hdinsight-use-sqoop.md)
- [Použití Oozie s HDInsight](hdinsight-use-oozie.md)
- [Použití MapReduce úlohy s HDInsight][hdinsight-use-mapreduce]

[check]: ./media/hdinsight-use-hive/hdi.checkmark.png

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/
[hivetask]: http://msdn.microsoft.com/library/mt146771(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx

[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md


[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-get-started.md

[Powershell-install-configure]: ../powershell-install-configure.md
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png


[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx
