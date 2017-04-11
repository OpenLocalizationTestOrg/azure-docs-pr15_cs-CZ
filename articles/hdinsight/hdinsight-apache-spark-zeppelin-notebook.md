<properties 
    pageTitle="Použití Zeppelin poznámkových bloků s Spark obrázku na HDInsight Linux | Microsoft Azure" 
    description="Podrobný návod, jak používat Zeppelin poznámkových bloků pomocí Spark clusterů v HDInsight Linux." 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/05/2016" 
    ms.author="nitinme"/>


# <a name="use-zeppelin-notebooks-with-apache-spark-cluster-on-hdinsight-linux"></a>Pomocí obrázku Apache Spark na HDInsight Linux Zeppelin poznámkových bloků

HDInsight Spark clusterů zahrnout Zeppelin poznámkové bloky, které můžete použít k spuštění Spark úloh. V tomto článku se naučíte, jak používat Poznámkový blok Zeppelin clusteru HDInsight.


**Požadavky:**

* Předplatné Azure. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* Apache Spark obrázku. Pokyny najdete v tématu [Vytvoření Spark Apache clusterů Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="launch-a-zeppelin-notebook"></a>Spuštění poznámkového bloku Zeppelin

1. Na zásuvné Spark obrázku klikněte na **Řídicí panel obrázku**a klikněte na **Poznámkový blok Zeppelin**. Pokud se zobrazí výzva, zadejte přihlašovací údaje Správce clusteru.

    > [AZURE.NOTE] Může taky dostanete Zeppelin Poznámkový blok pro svůj cluster otevřením následující adresu URL v prohlížeči. __NÁZEV_CLUSTERU__ nahraďte názvem vašeho obrázku:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/zeppelin`

2. Vytvoření nového poznámkového bloku. V podokně záhlaví klikněte na **Poznámkový blok**a potom klikněte na **Vytvořit novou poznámku**.

    ![Vytvoření nového poznámkového bloku Zeppelin] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.createnewnote.png "Vytvoření nového poznámkového bloku Zeppelin")

    Zadejte název poznámkového bloku a klikněte na **Vytvořit poznámku**.

3. Taky musíte že záhlaví Poznámkový blok se zobrazí stav připojení. Je označen Zelená tečka v pravém horním rohu.

    ![Stav Zeppelin poznámkového bloku] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.newnote.connected.png "Stav Zeppelin poznámkového bloku")

4. Načtení ukázkových dat do dočasné tabulky. Při vytváření Spark obrázku v HDInsight Ukázka datového souboru, **hvac.csv**zkopírována přidružené úložiště účtu v části **\HdiSamples\SensorSampleData\hvac**.

    Do prázdných odstavců, která se vytvoří ve výchozím nastavení do nového poznámkového bloku vložte následující fragment kódu.

        %livy.spark
        //The above magic instructs Zeppelin to use the Livy Scala interpreter

        // Create an RDD using the default Spark context, sc
        val hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
        
        // Define a schema
        case class Hvac(date: String, time: String, targettemp: Integer, actualtemp: Integer, buildingID: String)
        
        // Map the values in the .csv file to the schema
        val hvac = hvacText.map(s => s.split(",")).filter(s => s(0) != "Date").map(
            s => Hvac(s(0), 
                    s(1),
                    s(2).toInt,
                    s(3).toInt,
                    s(6)
            )
        ).toDF()
        
        // Register as a temporary table called "hvac"
        hvac.registerTempTable("hvac")
        
    Když stisknete **SHIFT + ENTER** nebo klikněte na tlačítko **Přehrát** odstavce spustit fragment. Stav v pravém dolním rohu odstavce by měl probíhají od PŘIPRAVENÝCH čekající na spuštění dokončeno. Výstup se zobrazí v dolní části odstavcem. Snímek obrazovky vypadá takto:

    ![Vytvořit dočasné tabulku z neformátovaná data] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.note.loaddDataintotable.png "Vytvořit dočasná tabulku z neformátovaná data")

    Můžete také zadat název každého odstavce. V pravém rohu klikněte na ikonu **Nastavení** a potom klikněte na **Zobrazit název**.

5. Nyní můžete spustit příkazy Spark SQL v tabulce **TVK** . Vložte nového odstavce následující dotaz. Dotaz načte ID budovy a rozdíl mezi cílové a skutečné teploty pro každou vytváření k danému datu. Když stisknete **SHIFT + ENTER**.

        %sql
        select buildingID, (targettemp - actualtemp) as temp_diff, date from hvac where date = "6/1/13" 

    Výraz **% sql** na začátku, bude Poznámkový blok můžete video interpreter Livius Scala.

    Následující obrázek ukazuje výstupu.

    ![Spuštění příkazu Spark SQL poznámkového bloku] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.note.sparksqlquery1.png "Spuštění příkazu Spark SQL poznámkového bloku")

     Klikněte na tlačítko Možnosti zobrazení (zvýrazněné v obdélník) můžete přepínat mezi různých formátovaná jako pro stejný výstup. Klikněte na **Nastavení** a vyberte, jaké consitutes klíče a hodnoty do výstupu. Snímek obrazovky nad využívá **buildingID** klávesu a průměr **temp_diff** hodnotu.

    
6. Můžete taky spustit příkazy Spark SQL pomocí proměnných v dotazu. Další fragment kódu ukazuje, jak definovat proměnnou **Temp**v dotazu s možnými hodnotami, která že se má k vytvoření dotazu s. Při prvním spuštění dotazu rozevírací seznam automaticky naplní hodnoty, které jste zadali proměnné.

        %sql
        select buildingID, date, targettemp, (targettemp - actualtemp) as temp_diff from hvac where targettemp > "${Temp = 65,65|75|85}" 

    Vložte tento fragment kódu do nového odstavce a stiskněte klávesu **SHIFT + ENTER**. Následující obrázek ukazuje výstupu.

    ![Spuštění příkazu Spark SQL poznámkového bloku] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.note.sparksqlquery2.png "Spuštění příkazu Spark SQL poznámkového bloku")

    Dalších dotazů můžete vybrat novou hodnotu z rozevíracího seznamu a znovu spusťte dotaz. Klikněte na **Nastavení** a vyberte, jaké consitutes klíče a hodnoty do výstupu. Snímek obrazovky výše používá **buildingID** jako klíč, průměrnou hodnotu **temp_diff** jako hodnota a **targettemp** jako skupina.

7. Restartujte video interpreter Livius ukončete aplikaci. K tomu, otevřete video interpreter nastavení po kliknutí na přihlášenému do pole uživatelské jméno v pravém horním rohu a potom klikněte na **Video Interpreter**.

    ![Spustit video interpreter] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Podregistru výstup")

2. Přejděte na nastavení video interpreter Livius a potom klikněte na **Restartovat**.

    ![Restartujte intepreter Livius] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.zeppelin.restart.interpreter.png "Restartujte Zeppelin intepreter")

## <a name="how-do-i-use-external-packages-with-the-notebook"></a>Použití externích balíčků s poznámkovým blokem

Poznámkový blok Zeppelin můžete nakonfigurovat ve Apache Spark obrázku na HDInsight (Linux) používat externí, přispěla komunity balíčků, které nejsou součástí mimo předdefinovaných v clusteru. Můžete vyhledávat [Maven úložiště](http://search.maven.org/) pro celý seznam balíčků, které jsou k dispozici. Otevřete seznam dostupných balíčků taky z jiných zdrojů. Úplný seznam přispěla komunity balíčků je například jsou dostupné na webu [Spark balíčků](http://spark-packages.org/).

V tomto článku zobrazí se použití balíčku [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) s poznámkovým blokem Jupyter.

1. Otevřete video interpreter nastavení. V pravém horním rohu klikněte na přihlášenému do pole uživatelské jméno a potom klikněte na **Video Interpreter**.

    ![Spustit video interpreter] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Podregistru výstup")

2. Přejděte na nastavení video interpreter Livius a potom klikněte na **Upravit**.

    ![Změna nastavení video interpreter] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-1.png "Změna nastavení video interpreter")

3. Přidejte nový klíč s názvem **livy.spark.jars.packages** a nastavte jeho hodnotu ve formátu `group:id:version`. Ano, pokud chcete použil funkci balíček [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) , musíte přínosu klávesy `com.databricks:spark-csv_2.10:1.4.0`.

    ![Změna nastavení video interpreter] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-2.png "Změna nastavení video interpreter")

    Klikněte na tlačítko **Uložit** a znovu ho spusťte video interpreter Livius.

4. **Tip**: Pokud chcete zjistit, jak zahltit hodnoty klíče zadaná výše, tady je způsob.

    na. Vyhledejte balíček v úložišti Maven. Pro účely tohoto návodu jsme použili [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).
    
    b. Z úložiště shromážděte hodnoty **ID skupiny**, **ArtifactId**a **verze**.

    ![Použití externích balíčků s Jupyter poznámkového bloku] (./media/hdinsight-apache-spark-zeppelin-notebook/use-external-packages-with-jupyter.png "Použití externích balíčků s Jupyter poznámkového bloku")

    c. Zřetězí tři hodnot oddělených středníky (****:).

        com.databricks:spark-csv_2.10:1.4.0

## <a name="where-are-the-zeppelin-notebooks-saved"></a>Kam se ukládají Zeppelin poznámkové bloky?

Poznámkové bloky Zeppelin se uloží do headnodes obrázku. Ano Pokud odstraníte clusteru, poznámkové bloky se odstraní taky. Pokud chcete zachovat svoje poznámkové bloky pro pozdější použití na jiných clusterů, musíte je exportovat po spuštění úlohy. Export Poznámkový blok, klepněte na ikonu **Exportovat** , jak je znázorněno na následujícím obrázku.

![Stažení poznámkového bloku] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-download-notebook.png "Stažení poznámkového bloku")

Poznámkový blok se uloží jako soubor JSON kde stáhnout.

## <a name="livy-session-management"></a>Správa Livius relace

Když spustíte kód odstavec v poznámkovém bloku Zeppelin, vytvoří se novou relaci Livius ve svůj cluster HDInsight Spark. Ve všech poznámkových blocích Zeppelin následně vytvořené se sdílí pro tuto relaci. Pokud z nějakého důvodu Livius relace ukončit (restartujte počítač clusteru, atd.), nebude moct spuštění úlohy z poznámkového bloku Zeppelin.

V takovém případě musí proveďte následující kroky, aby mohli začít spuštění úlohy z poznámkového bloku Zeppelin. 

1. Restartujte video interpreter Livius z poznámkového bloku Zeppelin. K tomu, otevřete video interpreter nastavení po kliknutí na přihlášenému do pole uživatelské jméno v pravém horním rohu a potom klikněte na **Video Interpreter**.

    ![Spustit video interpreter] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Podregistru výstup")

2. Přejděte na nastavení video interpreter Livius a potom klikněte na **Restartovat**.

    ![Restartujte intepreter Livius] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.zeppelin.restart.interpreter.png "Restartujte Zeppelin intepreter")

3. Spuštění kódu buňky z existujícího poznámkového bloku Zeppelin. Tím vytvoříte novou relaci Livius clusteru HDInsight.

## <a name="seealso"></a>Viz taky


* [Přehled: Apache Spark na Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scénáře

* [Spark s BI: Analýza interaktivní dat pomocí Spark v HDInsight nástrojích BI](hdinsight-apache-spark-use-bi-tools.md)

* [Spark s výukové počítače: použití Spark v HDInsight pro analýzu stavební teplotu pomocí TVK dat](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Spark s výukové počítače: použití Spark v HDInsight odhadnout výsledků kontroly jídla](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Datových proudů Spark: Použití Spark v HDInsight vytvářet v reálném čase streamování aplikace](hdinsight-apache-spark-eventhub-streaming.md)

* [Analýza protokolu webu pomocí Spark HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Vytvoření a spuštění aplikací

* [Vytvoření samostatného aplikace pomocí Scala](hdinsight-apache-spark-create-standalone-application.md)

* [Spuštění úlohy vzdáleně Spark clusteru pomocí Livius](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Nástroje a rozšíření

* [Modul plug-in nástroje HDInsight IntelliJ představu umožňuje vytvořit a odeslat Spark Scala aplikace](hdinsight-apache-spark-intellij-tool-plugin.md)

* [Modul plug-in pro použití HDInsight nástroje pro IntelliJ NÁPAD vzdáleně ladění Spark aplikací](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [Oříšky umožňující Jupyter poznámkového bloku na Spark obrázku pro HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Použití externích balíčků s poznámkovými bloky Jupyter](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Instalace Jupyter ve vašem počítači a připojte k HDInsight Spark obrázku](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Přidávání a používání zdrojů

* [Přidávání a používání zdrojů pro Apache Spark cluster v Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Sledování a ladění úlohy výpočetnímu clusteru Apache Spark v HDInsight](hdinsight-apache-spark-job-debugging.md)


[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md 







