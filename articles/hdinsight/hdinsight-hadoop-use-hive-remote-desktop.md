<properties
   pageTitle="Použití podregistru Hadoop a Vzdálená plocha v HDInsight | Microsoft Azure"
   description="Zjistěte, jak se připojit k obrázku Hadoop v HDInsight pomocí vzdálené plochy a spusťte podregistru dotazů pomocí rozhraní podregistru příkazového řádku."
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
   ms.date="09/06/2016"
   ms.author="larryfr"/>

# <a name="use-hive-with-hadoop-on-hdinsight-with-remote-desktop"></a>Použití podregistru s Hadoop na Hdinsightu pomocí vzdálené plochy

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

V tomto článku se dozvíte, jak se připojit k obrázku HDInsight pomocí vzdálené plochy a spusťte podregistru dotazů pomocí podregistru rozhraní příkazového řádku (rozhraní příkazového řádku).

> [AZURE.NOTE] Tento dokument neposkytuje podrobný popis čemu HiveQL příkazy, které se používají v příkladech. Informace o HiveQL, který je v tomto příkladě použita najdete v tématu [Použití podregistru s Hadoop na HDInsight](hdinsight-use-hive.md).

##<a id="prereq"></a>Zjistit předpoklady pro

Kroky v tomto článku, budete potřebovat:

* Shluk serveru s Windows HDInsight (Hadoop na HDInsight)

* Klientského počítače se systémem Windows 10, okno 8 nebo Windows 7

##<a id="connect"></a>Připojit se ke vzdálené ploše

Povolit připojení ke vzdálené ploše pro HDInsight clusteru, potom je připojíte k němu postupujte podle pokynů na [připojit k clusterů HDInsight pomocí RDP](hdinsight-administer-use-management-portal.md#rdp).

##<a id="hive"></a>Použití příkazu podregistru

Po připojení na plochu HDInsight clusteru, použijte pro práci s podregistru podle těchto kroků:

1. Z počítače HDInsight začněte **Hadoop příkazového řádku**.

2. Zadejte tento příkaz Spustit podregistru rozhraní příkazového řádku:

        %hive_home%\bin\hive

    Po spuštění rozhraní příkazového řádku, zobrazí se výzva podregistru rozhraní příkazového řádku: `hive>`.

3. Pomocí rozhraní příkazového řádku zadejte následující příkazy k vytvoření nové tabulky s názvem **log4jLogs** používání ukázkových dat:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Tyto příkazy provádět následující akce:

    * **PŘETAŽENÍ tabulky**: Odstraní tabulku a datového souboru, pokud již existuje v tabulce.

    * **Vytvořit externí tabulka**: vytvoří nová tabulka "externí" v podregistru. Externí tabulky obsahují pouze definici tabulky v podregistru (data ještě zbývá v původním umístění).

        > [AZURE.NOTE] Externí tabulky bude použito při očekáváte podkladová data mají být aktualizovány externí zdroj (například proces nahrávání automatizovaných data) nebo jinou MapReduce operací, ale chcete, aby podregistru dotazů použít nejnovější data.
        >
        > Uvolnění externí tabulce znamená **neodstraňujte data, pouze definici tabulky** .

    * **Formát řádku**: říká podregistru formátování data. V tomto případě polí v jednotlivých protokolu oddělená mezerou.

    * **ULOŽENÝ jako textový soubor umístění**: říká podregistru, kde jsou data uložená (příklad/datového adresáře) a je uložená jako text.

    * **Vyberte**: slouží k výběru počet řádků místo, kam sloupec **t4** obsahuje hodnotu **[Chyba]**. Protože jsou tři řádky, které obsahují hodnotu to by vrátit hodnotu **3** .

    * **INPUT__FILE__NAME jako "%.log"** - říká, která by měla pouze vrácených data ze souborů, které končí na podregistru. protokolu. Omezuje hledání sample.log soubor, který obsahuje data, zůstane vracet dat z jiných příkladu datové soubory, které se neshodují schéma, které byla definována.


4. Umožňuje vytvořit novou "interní" tabulku s názvem **errorLogs**následující příkazy:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Tyto příkazy provádět následující akce:

    * **Vytvoření tabulky IF NOT EXISTS**: vytvoří tabulku, pokud už neexistuje. Protože se nepoužívá **externí** klíčové slovo, toto je vnitřní tabulku, která je uložena v datový sklad podregistru a úplně spravuje podregistru.

        > [AZURE.NOTE] Na rozdíl od **externích** tabulek přetažením vnitřní tabulku odstraněny také podkladová data.

    * **ULOŽENÝ jako ORC**: jsou uložená data ve sloupcovém formátu (ORC) optimalizované řádku. Jedná se o formát vysoce Optimalizovaná a efektivně pro ukládání dat podregistru.

    * PŘEPSAT **vložení... Vyberte**: vybere řádků z tabulky **log4jLogs** , které obsahují **[Chyba]**, vloží data do tabulky **errorLogs** .

    Abyste ověřili, že k tabulce **errorLogs** byly uloženy pouze řádky, které ve sloupci t4 obsahují **[Chyba]** , použijte následující příkaz k vrácení všech řádků z **errorLogs**:

        SELECT * from errorLogs;

    Tři řádky dat má být vrácena, obsahující všechny **[Chyba]** ve sloupci t4.

##<a id="summary"></a>Souhrn

Jak vidíte, příkaz podregistru poskytuje snadný způsob, jak interaktivně spouštění dotazů podregistru HDInsight clusteru, sledování stavu úlohy a načíst výstupu.

##<a id="nextsteps"></a>Další kroky

Obecné informace o podregistru v HDInsight:

* [Použití podregistru s Hadoop na HDInsight](hdinsight-use-hive.md)

Informace o jiných způsobech můžete pracovat s Hadoop na HDInsight:

* [Použití Prasátko s Hadoop na HDInsight](hdinsight-use-pig.md)

* [Použití MapReduce s Hadoop na HDInsight](hdinsight-use-mapreduce.md)

Pokud používáte Tez se podregistru, přečtěte si článek tyto dokumenty pro ladění informace:

* [Použití rozhraní Tez na serveru s Windows HDInsight](hdinsight-debug-tez-ui.md)

* [Použití zobrazení Ambari Tez na základě Linux HDInsight](hdinsight-debug-ambari-tez-view.md)

[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

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





[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[Powershell-install-configure]: ../powershell-install-configure.md
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

