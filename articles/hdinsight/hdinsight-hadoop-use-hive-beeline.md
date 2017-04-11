<properties
   pageTitle="Použití Beeline pro práci s podregistru na HDInsight (Hadoop) | Microsoft Azure"
   description="Naučte se používat SSH připojit k obrázku Hadoop v HDInsight a interaktivně odesílání podregistru dotazů pomocí Beeline. Beeline je nástroj pro práci s HiveServer2 přes JDBC."
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
   ms.date="10/10/2016"
   ms.author="larryfr"/>

#<a name="use-hive-with-hadoop-in-hdinsight-with-beeline"></a>Použití podregistru s Hadoop v Hdinsightu pomocí Beeline

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

V tomto článku se dozvíte, jak použít zabezpečené prostředí (SSH) se připojit k obrázku na základě Linux HDInsight a interaktivně odesílání podregistru dotazů nástrojem [Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) příkazového řádku.

> [AZURE.NOTE] Beeline používá pro připojení k podregistru JDBC. Další informace o použití JDBC s podregistru najdete v článku [připojení k podregistru na Azure Hdinsightu pomocí ovladače podregistru JDBC](hdinsight-connect-hive-jdbc-driver.md).

##<a id="prereq"></a>Zjistit předpoklady pro

Kroky v tomto článku, budete potřebovat:

* Základě Linux Hadoop clusteru HDInsight.

* SSH klienta. S klientem SSH by se Linux, Unix a Mac OS. Uživatelé Windows stáhněte klientů, jako je [nátěrové](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

##<a id="ssh"></a>Spojení s SSH

Připojení k plně kvalifikovaný název domény (FQDN) svého clusteru HDInsight pomocí příkazu SSH. Úplný název domény budou název, který jste zadali obrázku, klikněte **. azurehdinsight.net**. Například následující by připojit k obrázku s názvem **myhdinsight**:

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Pokud jste zadali certifikát klíč pro ověřování SSH** při vytvoření obrázku HDInsight budete muset zadat umístění privátním klíčem na klientské systémy:

    ssh admin@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

**Pokud jste zadali heslo pro ověřování SSH** při vytvoření obrázku HDInsight, musíte k zadání hesla po zobrazení výzvy.

Další informace o použití SSH s Hdinsightu najdete v článku [Použití SSH s Hadoop Linux založené na HDInsight z Linux, OS X a Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-based-clients"></a>Nátěrové (klientů serveru s Windows)

Windows neposkytuje předdefinované SSH klienta. Doporučujeme používat **nátěrové**, které si můžete stáhnout z [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Další informace o použití nátěrové najdete v tématu [Použití SSH s Hadoop Linux založené na HDInsight z Windows ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="beeline"></a>Použití příkazu Beeline

1. Po připojení, použijte následující zahájíte Beeline:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin

    To spusťte Beeline klienta a připojte k JDBC url. Tady `localhost` slouží vzhledem k tomu HiveServer2 běží na obou hlavy uzlů v clusteru, a můžeme provozu Beeline přímo v primární headnode.
    
    Po dokončení příkazu budou přicházet na `jdbc:hive2://localhost:10001/>` dotaz.

3. Příkazy beeline obvykle začínají `!` znaků, například `!help` zobrazí nápovědu. Ale na `!` často nemusí provádět. Například `help` budou fungovat i.

    Pokud zobrazit nápovědu, zjistíte `!sql`, které se používají k provedení HiveQL příkazy. Však HiveQL tak často používá, že je možné vynechat, předchozí `!sql`. Následující dva příkazy mají přesně stejné výsledky; zobrazení momentálně dostupné prostřednictvím podregistru tabulky:
    
        !sql show tables;
        show tables;
    
    Nové clusteru, by měl být uveden jenom jednu tabulku: __hivesampletable__.

4. Použijte následující zobrazíte schéma hivesampletable:

        describe hivesampletable;
        
    Vrátí následující informace:
    
        +-----------------------+------------+----------+--+
        |       col_name        | data_type  | comment  |
        +-----------------------+------------+----------+--+
        | clientid              | string     |          |
        | querytime             | string     |          |
        | market                | string     |          |
        | deviceplatform        | string     |          |
        | devicemake            | string     |          |
        | devicemodel           | string     |          |
        | state                 | string     |          |
        | country               | string     |          |
        | querydwelltime        | double     |          |
        | sessionid             | bigint     |          |
        | sessionpagevieworder  | bigint     |          |
        +-----------------------+------------+----------+--+

    Sloupce se zobrazí v tabulce. Během provedeme může některé dotazy týkající se tato data, místo toho Vytvořme úplně novou tabulku a ukazuje, jak k načtení dat do podregistru a použít schéma.
    
5. Zadejte následující příkazy k vytvoření nové tabulky s názvem **log4jLogs** pomocí ukázková data součástí HDInsight obrázku:

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Tyto příkazy provádět následující akce:

    * **PŘETAŽENÍ tabulky** – odstraní tabulku a datovém souboru, v případě, že v tabulce už existuje.
    * **Vytvořit externí tabulka** – vytvoří nová tabulka "externí" v podregistru. Externí tabulky pouze obsahují definice tabulky v podregistru. Data ještě zbývá do původního umístění.
    * **Formát řádku** - říká podregistru formátování data. V tomto případě polí v jednotlivých protokolu oddělená mezerou.
    * **ULOŽENÁ jako textový soubor umístění** - říká podregistru, kde jsou data uložená (příklad/datového adresáře) a že je uložená jako text.
    * **Vyberte** – slouží k výběru počet řádků místo, kam sloupec **t4** obsahuje hodnotu **[Chyba]**. Jsou tři řádky, které obsahují hodnotu to by vrátit hodnotu **3** .
    * **INPUT__FILE__NAME jako "%.log"** - říká, která by měla pouze vrácených data ze souborů, které končí na podregistru. protokolu. Za normálních okolností by stačí jenom data se na stejné schéma do stejné složky při dotazování v podregistru, ale tento soubor protokolu příkladu je uložen pomocí dalších formátů data.

    > [AZURE.NOTE] Externí tabulky by měl použít, když je očekávaná podkladová data, která chcete aktualizovat externího zdroje, jako je proces nahrávání automatické dat nebo jinou MapReduce operací, ale stále chcete podregistru dotazů použít nejnovější data.
    >
    > Uvolnění externí tabulce znamená **neodstraňujte data, pouze definici tabulky** .
    
    Výstup tohoto příkazu by měl být stejný následujícím způsobem:
    
        INFO  : Tez session hasn't been created yet. Opening session
        INFO  :
        
        INFO  : Status: Running (Executing on YARN cluster with App id application_1443698635933_0001)
        
        INFO  : Map 1: -/-      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0(+1)/1
        INFO  : Map 1: 1/1      Reducer 2: 1/1
        +----------+--------+--+
        |   sev    | count  |
        +----------+--------+--+
        | [ERROR]  | 3      |
        +----------+--------+--+
        1 row selected (47.351 seconds)

4. Beeline opustíte použít `!quit`.

##<a id="file"></a>Spusťte soubor HiveQL

Beeline lze také spustit soubor, který obsahuje HiveQL příkazy. Pomocí následujících kroků pro vytvoření souboru a pak ho pomocí Beeline spusťte.

1. Umožňuje vytvořit nový soubor s názvem __query.hql__tento příkaz:

        nano query.hql
        
2. Jakmile se otevře se editor, použijte následující jako obsah souboru. Tento dotaz vytvoří novou "interní" tabulku s názvem **errorLogs**:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Tyto příkazy provádět následující akce:

    * **Vytvoření tabulky IF NOT EXISTS** - vytvoří tabulku, pokud už neexistuje. Protože nepoužívá **externí** klíčové slovo, toto je vnitřní tabulku, která je uložena v datový sklad podregistru a úplně spravuje podregistru.
    * **ULOŽENÝ jako ORC** - jsou uložená data ve formátu optimalizované řádku sloupcovém (ORC). Jedná se o formát vysoce Optimalizovaná a efektivně pro ukládání dat podregistru.
    * PŘEPSAT **vložení... Vyberte** – slouží k výběru řádků z tabulky **log4jLogs** , které obsahují **[Chyba]**a vloží data do tabulky **errorLogs** .
    
    > [AZURE.NOTE] Na rozdíl od externích tabulek přetažením vnitřní tabulku odstraníte podkladová data.
    
3. Uložte soubor stisknutím kláves __Ctrl__+ ___X__a potom zadejte __Y__a konečně __Enter__.

4. Ke spuštění soubor pomocí Beeline použijte následující. __HOSTNAME__ nahraďte názvem získaný dříve hlavního uzlu a __heslo__ s heslo k účtu správce:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -i query.hql

    > [AZURE.NOTE] `-i` Parametr spuštění Beeline spustí příkazy v souboru query.hql a zůstane v Beeline na `jdbc:hive2://localhost:10001/>` dotaz. Můžete taky spustit souboru pomocí `-f` parametr, který po zpracování soubor se vrátíte do flám.

5. Pokud chcete ověřit, že byla vytvořená tabulka **errorLogs** , použijte následující příkaz k vrácení všech řádků z **errorLogs**:

        SELECT * from errorLogs;

    Tři řádky dat má být vrácena, obsahující všechny **[Chyba]** ve sloupci t4:
    
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | errorlogs.t1  | errorlogs.t2  | errorlogs.t3  | errorlogs.t4  | errorlogs.t5  | errorlogs.t6  | errorlogs.t7  |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | 2012-02-03    | 18:35:34      | SampleClass0  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 18:55:54      | SampleClass1  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 19:25:27      | SampleClass4  | [ERROR]       | incorrect     | id            |               |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        3 rows selected (1.538 seconds)

## <a name="more-about-beeline-connectivity"></a>Další informace o připojení Beeline

Kroky v tomto dokumentu použijte `localhost` připojení k HiveServer2 spuštěna headnode obrázku. Zatímco můžete také použít název hostitele nebo plně kvalifikovaný název domény headnode můžou být vyžaduje další kroky procesu (kroků a dozvíte název hostitele nebo plně kvalifikovaný název domény). Použití `localhost` stačí při použití Beeline z headnode.

Pokud máte postranní uzel v svůj cluster s Beeline nainstalovaný, musíte použijte hodnotu název hostitele nebo plně kvalifikovaný název domény headnode na připojení.

Pokud máte nainstalovaný v klientském počítači mimo svůj cluster Beeline, můžete se připojit pomocí následujícího příkazu. __NÁZEV_CLUSTERU__ nahraďte názvem svůj cluster HDInsight. Nahraďte __heslo__ heslo k účtu správce (HTTP přihlášení).

    beeline -u 'jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;ssl=true?hive.server2.transport.mode=http;hive.server2.thrift.http.path=hive2' -n admin -p PASSWORD

Všimněte si, že parametry identifikátor URI se liší od při spuštění přímo headnode nebo ze strany uzlech clusteru. Důvodem je připojení k clusteru z Internetu používá veřejné brány, který přesměrovává přenosy přes port 443. Také několik dalších služeb jsou k dispozici prostřednictvím veřejné brány na port 443, tak, aby se liší od při připojování přímo identifikátor URI. Při připojení z Internetu relace musí ověřovat také zadáním hesla.

##<a id="summary"></a><a id="nextsteps"></a>Další kroky

Jakmile uvidíte, příkaz Beeline vám bude radit snadný způsob, jak interaktivně spouštění dotazů podregistru clusteru HDInsight.

Obecné informace o podregistru v HDInsight:

* [Použití podregistru s Hadoop na HDInsight](hdinsight-use-hive.md)

Informace o jiných způsobech můžete pracovat s Hadoop na HDInsight:

* [Použití Prasátko s Hadoop na HDInsight](hdinsight-use-pig.md)

* [Použití MapReduce s Hadoop na HDInsight](hdinsight-use-mapreduce.md)

Pokud používáte Tez se podregistru, přečtěte si článek tyto dokumenty pro ladění informace:

* [Použití rozhraní Tez na serveru s Windows HDInsight](hdinsight-debug-tez-ui.md)

* [Použití zobrazení Ambari Tez na základě Linux HDInsight](hdinsight-debug-ambari-tez-view.md)

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md

[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

