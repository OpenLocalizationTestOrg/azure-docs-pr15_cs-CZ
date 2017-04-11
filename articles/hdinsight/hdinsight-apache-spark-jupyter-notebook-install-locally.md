<properties 
    pageTitle="Nainstalujte Jupyter Poznámkový blok na svém počítači a připojte ji k obrázku HDInsight Spark | Microsoft Azure" 
    description="Informace o tom, jak nainstalovat Jupyter místně na vašem počítači a připojte ji k Apache Spark clusteru v Azure HDInsight." 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="nitinme"/>


# <a name="install-jupyter-notebook-on-your-computer-and-connect-to-apache-spark-cluster-on-hdinsight-linux"></a>Nainstalujte Jupyter Poznámkový blok na svém počítači a připojte k obrázku Apache Spark na HDInsight Linux

V tomto článku se zjistěte, jak nainstalovat Jupyter Poznámkový blok s vlastní PySpark (pro Python) a jádra Spark (pro Scala) s Spark magické a připojit Poznámkový blok clusteru HDInsight. Může být několik důvodů, proč instalace Jupyter ve vašem počítači a může být i některé problémy. Seznam důvodů, proč a úkolů naleznete v části [Proč by měli nainstalovat Jupyter v počítači](#why-should-i-install-jupyter-on-my-computer) na konci tohoto článku.

Zahrnuje tři základní kroky při instalaci Jupyter a magické Spark ve vašem počítači.

* Instalace Jupyter Poznámkový blok
* Nainstalovat jádra PySpark a Spark magické Spark
* Konfigurace Spark magické pro přístup k obrázku Spark na HDInsight

Další informace o vlastních jádra a Spark magické umožňující Jupyter poznámkových bloků pomocí obrázku Hdinsightu najdete v článku [jádra umožňující Jupyter poznámkových bloků pomocí Apache Spark Linux clusterů v HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md).

##<a name="prerequisites"></a>Zjistit předpoklady pro

Zjistit předpoklady pro uvedený tady nejsou při instalaci Jupyter. Toto jsou připojit se k obrázku HDInsight Poznámkový blok Jupyter Poznámkový blok po instalaci používat.

- Předplatné Azure. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Apache Spark obrázku na HDInsight Linux. Pokyny najdete v tématu [Vytvoření Spark Apache clusterů Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="install-jupyter-notebook-on-your-computer"></a>Instalace Jupyter Poznámkový blok na počítači

Před instalací Jupyter poznámkové bloky, musíte nainstalovat Python. Python a Jupyter jsou k dispozici v rámci [Ananconda rozdělení](https://www.continuum.io/downloads). Při instalaci Anaconda nainstalujete skutečně rozdělení množiny Python. Po instalaci Anaconda přidáte instalace Jupyter spuštěním příkazu. Tato část obsahuje pokyny, které je potřeba provést.

1. Stáhnout [Instalační služby systému Anaconda](https://www.continuum.io/downloads) pro svoji platformu a spusťte instalaci. Při spuštění Průvodce instalací, ujistěte se, že vyberete možnost Přidat Anaconda proměnná PATH.

2. Spusťte tento příkaz nainstalovat Jupyter.

        conda install jupyter

    Další informace o installting Jupyter najdete v článku [Instalace Jupyter pomocí Anaconda](http://jupyter.readthedocs.io/en/latest/install.html).

## <a name="install-the-kernels-and-spark-magic"></a>Instalace jádra a Spark magické

Pokyny k instalaci magické Spark jádra PySpark a Spark najdete v článku [si přečtěte následující dokumentaci sparkmagic](https://github.com/jupyter-incubator/sparkmagic#installation) na GitHub.

## <a name="configure-spark-magic-to-access-the-hdinsight-spark-cluster"></a>Konfigurace Spark magické pro přístup ke clusteru HDInsight Spark

V této části můžete nakonfigurovat magické Spark nainstalovanou dřívější připojení k Apache Spark clusteru, musíte mít již vytvořené v Azure HDInsight.

1. Informace o konfiguraci Jupyter obvykle uložený v adresáři domácí uživatele. Vyhledejte domácí adresáře na libovolnou platformu operačního systému, zadejte následující příkazy.

    Spusťte Python prostředí. Na okno příkazového řádku zadejte tento příkaz:

        python

    Na prostředí Python zadejte tento příkaz zjistit adresáři.

        import os
        print(os.path.expanduser('~'))

2. Přejděte na domovskou adresář a vytvořte složku s názvem **.sparkmagic** , pokud už neexistuje.

3. Ve složce vytvořte soubor s názvem **config.json** a přidejte následující úryvek JSON uvnitř.

        {
          "kernel_python_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          },
          "kernel_scala_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          }
        }

4. Nahraďte **{USERNAME}**, **{CLUSTERDNSNAME}**a **{BASE64ENCODEDPASSWORD}** s příslušnými hodnotami. Řada nástrojů v seznamu Oblíbené programovacím jazyce nebo online umožňuje generovat heslo ve formátu Base 64 kódováním pro hesla actualy. Jednoduchý fragment Python spustit z příkazového řádku budou:

        python -c "import base64; print(base64.b64encode('{YOURPASSWORD}'))"

5. Spusťte Jupyter. Zadejte následující příkaz z příkazového řádku.

        jupyter notebook

6. Ověřte, že můžete připojit k obrázku Jupyter poznámkového bloku a použít k dispozici Spark magické s jádra. Proveďte následující kroky.

    1. Vytvoření nového poznámkového bloku. V pravém dolním rohu klikněte na **Nový**. Měli byste vidět jádra výchozí **Python2** a dva nové jádra instalované, **PySpark** a **Spark**.

        ![Vytvoření nového poznámkového bloku Jupyter] (./media/hdinsight-apache-spark-jupyter-notebook-install-locally/jupyter-kernels.png "Vytvoření nového poznámkového bloku Jupyter")

    
        Klikněte na tlačítko **PySpark**.


    2. Spusťte následující fragment kódu.

            %%sql
            SELECT * FROM hivesampletable LIMIT 5

        Pokud můžete úspěšně vyvolat výstup, připojíte se k obrázku HDInsight testování.

    >[AZURE.TIP] Pokud chcete aktualizovat konfiguraci poznámkového bloku se připojit k jiné clusteru, aktualizujte config.json novou sadu hodnot, jak je vidět v kroku 3 výše. 

## <a name="why-should-i-install-jupyter-on-my-computer"></a>Proč nainstalovat Jupyter na svém počítači?

Může být spousta důvodů, proč můžete chtít instalace Jupyter ve vašem počítači a připojte ji k obrázku Spark na HDInsight.

* I když už jsou k dispozici clusteru Spark v Azure HDInsight Jupyter poznámkové bloky, instalace Jupyter ve vašem počítači poskytuje je možnost vytvořit poznámkových bloků místně, otestujte aplikaci proti pracovního obrázku a pak nahrajte poznámkové bloky do clusteru. K nahrání poznámkových bloků na clusteru, můžete jejich pomocí Jupyter poznámkového bloku, na kterém běží nebo clusteru nahrání nebo je uložíte do složky /HdiNotebooks v účtu úložiště spojeného s clusteru. Další informace o ukládání poznámkových bloků na clusteru najdete v článku [uložení Jupyter poznámkové bloky](hdinsight-apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored)?
* S poznámkovými bloky k dispozici místně, můžete se připojit k různých clusterů Spark založené na požadovanou aplikaci.
* Můžete GitHub implementace systému správy zdrojů a jejich správy verzí pro poznámkových bloků. Mohou také obsahovat prostředí pro spolupráci, kde můžete pracovat několik uživatelů se stejným poznámkovým.
* Můžete pracovat s poznámkovými bloky místně i bez clusteru. Potřebujete jenom clusteru otestovat svoje poznámkové bloky, pozor, abyste ručně spravovat svoje poznámkové bloky nebo vývojové prostředí.
* Možná bude snadněji konfigurace prostředí místní vývoj, než ji nakonfigurovat Jupyter instalaci na clusteru.  Můžete využít výhod všechny software, který jste nainstalovali místně bez konfigurace jeden nebo více vzdálené clusterů.

>[AZURE.WARNING] S Jupyter na místním počítači nainstalovaný více uživatelů je možné spouštět stejným poznámkovým na stejném clusteru Spark ve stejnou dobu. V takovém případě se vytvářejí více relací Livius. Pokud narazíte na problém a chcete ladění, který bude, že složitou úlohu ke sledování relace Livius patří které uživateli.




## <a name="seealso"></a>Viz taky


* [Přehled: Apache Spark na Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scénáře

* [Spark s BI: Analýza interaktivní dat pomocí Spark v HDInsight nástrojích BI](hdinsight-apache-spark-use-bi-tools.md)

* [Spark s výukové počítače: použití Spark v HDInsight pro analýzu stavební teploty pomocí TVK dat](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Spark s výukové počítače: použití Spark v HDInsight odhadnout výsledků kontroly jídla](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Datových proudů Spark: Použití Spark v HDInsight vytvářet v reálném čase streamování aplikace](hdinsight-apache-spark-eventhub-streaming.md)

* [Analýza protokolu webu pomocí Spark HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Vytvoření a spuštění aplikací

* [Vytvoření samostatného aplikace pomocí Scala](hdinsight-apache-spark-create-standalone-application.md)

* [Spuštění úlohy vzdáleně Spark clusteru pomocí Livius](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Nástroje a rozšíření

* [Modul plug-in nástroje HDInsight IntelliJ představu umožňuje vytvořit a odeslat Spark Scala aplikace](hdinsight-apache-spark-intellij-tool-plugin.md)

* [Modul plug-in pro použití HDInsight nástroje pro IntelliJ NÁPAD vzdáleně ladění Spark aplikací](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [Pomocí obrázku Spark na HDInsight Zeppelin poznámkových bloků](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Oříšky umožňující Jupyter poznámkového bloku na Spark obrázku pro HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Použití externích balíčků s poznámkovými bloky Jupyter](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

### <a name="manage-resources"></a>Přidávání a používání zdrojů

* [Přidávání a používání zdrojů pro Apache Spark cluster v Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Sledování a ladění úlohy výpočetnímu clusteru Apache Spark v HDInsight](hdinsight-apache-spark-job-debugging.md)
