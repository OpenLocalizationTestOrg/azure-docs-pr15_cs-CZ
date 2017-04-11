<properties
   pageTitle="Podregistru dotazu pomocí nástrojů Hadoop for Visual Studio | Microsoft Azure"
   description="Naučte se používat podregistru s Hadoop v HDInsight pomocí nástrojů Visual Studio Hadoop."
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

#<a name="run-hive-queries-using-the-hdinsight-tools-for-visual-studio"></a>Spouštění dotazů podregistru pomocí nástroje HDInsight for Visual Studio

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

V tomto článku se dozvíte, jak můžou odeslat podregistru dotazy k clusteru HDInsight pomocí nástroje HDInsight for Visual Studio.

> [AZURE.NOTE] Tento dokument neposkytuje podrobný popis čemu HiveQL příslušným použité v příkladech. Informace o HiveQL, který je v tomto příkladě použita najdete v tématu [Použití podregistru s Hadoop na HDInsight](hdinsight-use-hive.md).

##<a id="prereq"></a>Zjistit předpoklady pro

Kroky v tomto článku, budete potřebovat.

* Clusteru služby Azure HDInsight (Hadoop na HDInsight) (Linux nebo serveru s Windows)

* Visual Studio (jedno z následujících verzí):

    Visual Studio 2013 komunity/Professional/Premium/Ultimate s [Aktualizovat 4](https://www.microsoft.com/download/details.aspx?id=44921)

    Visual Studio 2015 (komunity a Enterprise)

- HDInsight nástroje pro Visual Studia. Informace o instalaci a konfiguraci nástroje najdete v článku [Začínáme s používáním aplikace Visual Studio Hadoop nástroje pro HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md) .

##<a id="run"></a>Spouštění dotazů podregistru pomocí nástroje HDInsight for Visual Studio

1. Otevřete **Aplikaci Visual Studio** a vyberte **Nový** > **projektu** > **HDInsight** > **Podregistru aplikace**. Zadejte název pro tento projekt.

2. Otevřete soubor **Script.hql** , který se vytvoří pomocí tohoto projektu a vložte následující příkazy HiveQL:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Tyto příkazy provádět následující akce:

    * **PŘETAŽENÍ tabulky**: Odstraní tabulku a datového souboru, pokud již existuje v tabulce.
    * **Vytvořit externí tabulka**: vytvoří nová tabulka "externí" v podregistru. Externí tabulky pouze obsahují definice tabulky v podregistru (data ještě zbývá v původním umístění).

        > [AZURE.NOTE] Externí tabulky má používat, když je očekávaná podkladová data mají být aktualizovány externí zdroj (například proces nahrávání automatické data) nebo jinou MapReduce operací, ale chcete, aby podregistru dotazů použít nejnovější data.
        >
        > Uvolnění externí tabulce znamená **neodstraňujte data, pouze definici tabulky** .

    * **Formát řádku**: říká podregistru formátování data. V tomto případě polí v jednotlivých protokolu oddělená mezerou.
    * **ULOŽENÝ jako textový soubor umístění**: říká podregistru, kde jsou data uložená (příklad/datového adresáře) a je uložená jako text.
    * **Vyberte**: Vyberte počet řádků místo, kam sloupec **t4** obsahují hodnotu **[Chyba]**. Protože jsou tři řádky, které obsahují hodnotu to by vrátit hodnotu **3** .
    * **INPUT__FILE__NAME jako "%.log"** - říká, která by měla pouze vrácených data ze souborů, které končí na podregistru. protokolu. Omezuje hledání sample.log soubor, který obsahuje data, zůstane vracet dat z jiných příkladu datové soubory, které se neshodují schéma, které byla definována.

3. Na panelu nástrojů vyberte **HDInsight obrázku** , který chcete použít pro tento dotaz a pak vyberte **Odeslat WebHCat** příslušným spustili podregistru úlohy pomocí WebHCat. Taky můžete odeslat úlohy pomocí tlačítka __spustit prostřednictvím HiveServer2__ Pokud HiveServer2 je k dispozici ve verzi obrázku. **Podregistru souhrn projektu** se zobrazí informace o spuštěná úloha. Pomocí odkazu na **aktualizace** aktualizovat informace o projektu, dokud **Stavem** změní na **Dokončit**.

4. Zobrazit výstup této úlohy pomocí odkazu na **Výstup projektu** . By měl zobrazit `[ERROR] 3`, což je hodnota vrácená příkazem SELECT.

5. Můžete taky spustit podregistru dotazy bez vytvoření projektu. Pomocí **Průzkumníka serveru**, rozbalte **Azure** > **HDInsight**, klikněte pravým tlačítkem na serveru HDInsight a pak vyberte **vytvořit dotaz podregistru**.

6. V dokumentu **temp.hql** , která se zobrazí přidejte následující příkazy HiveQL:

        set hive.execution.engine=tez;
        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Tyto příkazy provádět následující akce:

    * **Vytvoření tabulky IF NOT EXISTS**: vytvoří tabulku, pokud už neexistuje. Protože nepoužívá **externí** klíčové slovo, toto je vnitřní tabulku, která je uložena v datový sklad podregistru a úplně spravuje podregistru.

        > [AZURE.NOTE] Na rozdíl od **externích** tabulek umístěním vnitřní tabulku odstraněny také podkladová data.

    * **ULOŽENÝ jako ORC**: jsou uložená data ve sloupcovém formátu (ORC) optimalizované řádku. Jedná se o formát vysoce Optimalizovaná a efektivně pro ukládání dat podregistru.
    * PŘEPSAT **vložení... Vyberte**: vybere řádků z tabulky **log4jLogs** , které obsahují **[Chyba]**, vloží data do tabulky **errorLogs** .

7. Na panelu nástrojů vyberte **Odeslat** úlohu. Pomocí **Stavu úloh** zjistíte, že úloha byla úspěšně dokončena.

8. Ověřte, zda úkoly dokončeny a vytvoření nové tabulky, použijte **Průzkumník serveru** a rozbalte **Azure** > **HDInsight** > HDInsight obrázku > **Podregistru databází** > a **výchozí**. Měli byste vidět **errorLogs** tabulky a **log4jLogs** .

##<a id="summary"></a>Souhrn

Jak je vidět nástroje HDInsight for Visual Studio poskytují snadný způsob, jak ke spouštění dotazů podregistru HDInsight clusteru, sledování stavu úlohy a načíst výstupu.

##<a id="nextsteps"></a>Další kroky

Obecné informace o podregistru v HDInsight:

* [Použití podregistru s Hadoop na HDInsight](hdinsight-use-hive.md)

Informace o jiných způsobech můžete pracovat s Hadoop na HDInsight:

* [Použití Prasátko s Hadoop na HDInsight](hdinsight-use-pig.md)

* [Použití MapReduce s Hadoop na HDInsight](hdinsight-use-mapreduce.md)

Další informace o nástrojích HDInsight for Visual Studio:

* [Začínáme s HDInsight nástroje for Visual Studio](../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md)


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



[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png
