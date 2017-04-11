<properties
   pageTitle="Pomocí prostředí podregistru v HDInsight (Hadoop) | Microsoft Azure"
   description="Naučte se používat prostředí podregistru s obrázku na základě Linux HDInsight. Se dozvíte, jak se připojit k obrázku HDInsight pomocí SSh a pak pomocí prostředí podregistru můžete interaktivně spouštění dotazů."
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
   ms.date="10/04/2016"
   ms.author="larryfr"/>

# <a name="use-hive-with-hadoop-in-hdinsight-with-ssh"></a>Použití podregistru s Hadoop v Hdinsightu pomocí SSH

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

V tomto článku se dozvíte, jak použít zabezpečené prostředí (SSH) pro připojení k Hadoop Azure HDInsight clusteru a interaktivně odesílání podregistru dotazů pomocí rozhraní příkazového řádku podregistru (rozhraní příkazového řádku).

> [AZURE.IMPORTANT] Při provádění příkazu podregistru je k dispozici na základě Linux HDInsight clusterů, zvažte použití Beeline. Beeline je novějšího klienta pro práci s podregistru a je součástí svůj cluster HDInsight. Další informace o použití to najdete v článku [Použití podregistru s Hadoop v Hdinsightu pomocí Beeline](hdinsight-hadoop-use-hive-beeline.md).

##<a id="prereq"></a>Zjistit předpoklady pro

Kroky v tomto článku, budete potřebovat:

* Základě Linux Hadoop clusteru HDInsight.

* SSH klienta. S klientem SSH by se Linux, Unix a Mac OS. Uživatelé Windows stáhněte klientů, jako je [nátěrové](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

##<a id="ssh"></a>Spojení s SSH

Připojení k plně kvalifikovaný název domény (FQDN) svého clusteru HDInsight pomocí příkazu SSH. Úplný název domény budou název, který jste zadali obrázku, klikněte **. azurehdinsight.net**. Například následující by připojit k obrázku s názvem **myhdinsight**:

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Pokud jste zadali certifikát klíč pro ověřování SSH** při vytvoření obrázku HDInsight budete muset zadat umístění privátním klíčem na klientské systémy:

    ssh -i ~/mykey.key admin@myhdinsight-ssh.azurehdinsight.net

**Pokud jste zadali heslo pro ověřování SSH** při vytvoření obrázku HDInsight, musíte k zadání hesla po zobrazení výzvy.

Další informace o použití SSH s Hdinsightu najdete v článku [Použití SSH s Hadoop Linux založené na HDInsight z Linux, OS X a Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-based-clients"></a>Nátěrové (klientů serveru s Windows)

Windows neposkytuje předdefinované SSH klienta. Doporučujeme používat **nátěrové**, které si můžete stáhnout z [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Další informace o použití nátěrové najdete v tématu [Použití SSH s Hadoop Linux založené na HDInsight z Windows ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="hive"></a>Použití příkazu podregistru

2. Po připojení, začněte tím, že pomocí následujícího příkazu podregistru rozhraní příkazového řádku:

        hive

3. Pomocí rozhraní příkazového řádku zadejte následující příkazy k vytvoření nové tabulky s názvem **log4jLogs** pomocí ukázkových dat:

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
    * **INPUT__FILE__NAME jako "%.log"** - říká, která by měla pouze vrácených data ze souborů, které končí na podregistru. protokolu. Omezuje hledání sample.log soubor, který obsahuje data, zůstane vracet dat z jiných příkladu datové soubory, které se neshodují schéma, které byla definována.

    > [AZURE.NOTE] Externí tabulky by měl použít, když je očekávaná podkladová data, která chcete aktualizovat externího zdroje, jako je proces nahrávání automatické dat nebo jinou MapReduce operací, ale stále chcete podregistru dotazů použít nejnovější data.
    >
    > Uvolnění externí tabulce znamená **neodstraňujte data, pouze definici tabulky** .

4. Umožňuje vytvořit novou "interní" tabulku s názvem **errorLogs**následující příkazy:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Tyto příkazy provádět následující akce:

    * **Vytvoření tabulky IF NOT EXISTS** - vytvoří tabulku, pokud už neexistuje. Protože nepoužívá **externí** klíčové slovo, toto je vnitřní tabulku, která je uložena v datový sklad podregistru a úplně spravuje podregistru.
    * **ULOŽENÝ jako ORC** - jsou uložená data ve formátu optimalizované řádku sloupcovém (ORC). Jedná se o formát vysoce Optimalizovaná a efektivně pro ukládání dat podregistru.
    * PŘEPSAT **vložení... Vyberte** – slouží k výběru řádků z tabulky **log4jLogs** , které obsahují **[Chyba]**a vloží data do tabulky **errorLogs** .

    Pokud chcete ověřit, že pouze řádky obsahující **[Chyba]** ve sloupci t4 byly uloženy k tabulce **errorLogs** , použijte následující příkaz k vrácení všech řádků z **errorLogs**:

        SELECT * from errorLogs;

    Tři řádky dat má být vrácena, obsahující všechny **[Chyba]** ve sloupci t4.

    > [AZURE.NOTE] Na rozdíl od externích tabulek přetažením vnitřní tabulku odstraníte podkladová data.

##<a id="summary"></a>Souhrn

Jakmile uvidíte, příkaz podregistru vám bude radit snadný způsob, jak interaktivně spouštění dotazů podregistru HDInsight clusteru, sledování stavu úlohy a načíst výstupu.

##<a id="nextsteps"></a>Další kroky

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


[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png

