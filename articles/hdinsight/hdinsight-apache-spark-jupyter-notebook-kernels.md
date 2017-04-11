<properties 
    pageTitle="Dostupné s poznámkovými bloky Jupyter na HDInsight Spark jádra clusterů Linux | Microsoft Azure" 
    description="Informace o další Jupyter Poznámkový blok jádra dostupné Spark obrázku na HDInsight Linux." 
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
    ms.date="10/05/2016" 
    ms.author="nitinme"/>


# <a name="kernels-available-for-jupyter-notebooks-with-apache-spark-clusters-on-hdinsight-linux"></a>Oříšky umožňující Jupyter poznámkových bloků pomocí Apache Spark clusterů HDInsight Linux

Apache Spark obrázku na HDInsight (Linux) obsahuje Jupyter poznámkové bloky, které můžete použít k testování aplikace. Jádra je program, který spustí a interpretovat kódu. HDInsight Spark clusterů obsahují dvě jádra používající s poznámkovým blokem Jupyter. Toto jsou:

1. **PySpark** (u aplikací psaných Python)
2. **Spark** (u aplikací napsané v Scala)

V tomto článku se dozvíte o použití těchto jádra a jaké jsou přínosy získané od jejich použití.

**Požadavky:**

Je nutné mít takto:

- Předplatné Azure. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Apache Spark obrázku na HDInsight Linux. Pokyny najdete v tématu [Vytvoření Spark Apache clusterů Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="how-do-i-use-the-kernels"></a>Používání jádra 

1. Z [Portálu Azure](https://portal.azure.com/)z startboard klikněte na dlaždici pro svůj cluster Spark (Pokud připnuté na startboard). Můžete taky přejít na svůj cluster v části **Procházet vše** > **Clusterů HDInsight**.   

2. Na zásuvné Spark obrázku klikněte na **Řídicí panel obrázku**a klikněte na **Poznámkový blok Jupyter**. Pokud se zobrazí výzva, zadejte přihlašovací údaje Správce clusteru.

    > [AZURE.NOTE] Může taky dostanete Jupyter Poznámkový blok pro svůj cluster otevřením následující adresu URL v prohlížeči. __NÁZEV_CLUSTERU__ nahraďte názvem vašeho obrázku:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Vytvoření nového poznámkového bloku s novou jádra. Klikněte na **Nový**a potom klikněte na **Pyspark** nebo **Spark**. Spark jádra Scala aplikací a PySpark jádra byste měli použít pro Python aplikací.

    ![Vytvoření nového poznámkového bloku Jupyter] (./media/hdinsight-apache-spark-jupyter-notebook-kernels/jupyter-kernels.png "Vytvoření nového poznámkového bloku Jupyter") 

3. To dokončeno otevřete nový poznámkový blok s jádra, který jste vybrali.

## <a name="why-should-i-use-the-pyspark-or-spark-kernels"></a>Proč mám používat jádra PySpark nebo Spark?

Tady je několik výhod použití nového jádra.

1. **Přednastavené kontexty**. S **PySpark** nebo **Spark** jádra, které jsou k dispozici s poznámkovými bloky Jupyter nepotřebujete explicitně nastavit kontexty Spark nebo podregistru můžete začít pracovat s aplikací, kterou vyvíjíte; Toto jsou k dispozici pro ve výchozím nastavení. Kontexty jsou:

    * **sc** - Spark kontextu
    * **sqlContext** - podregistru kontextu


    Ano nemusíte spusťte příkazy jako následující nastavení kontexty:

        ###################################################
        # YOU DO NOT NEED TO RUN THIS WITH THE NEW KERNELS
        ###################################################
        sc = SparkContext('yarn-client')
        sqlContext = HiveContext(sc)

    Místo toho přímo můžete přednastavené kontexty v aplikaci.
    
2. **Magics buňky**. Jádra PySpark obsahuje několik předdefinovaných "magics", které jsou speciální příkazy, které můžete volat s `%%` (například `%%MAGIC` <args>). Příkaz magic musí být na první slovo v buňce kód a povolit více řádků obsahu. Magic word by měl být na první slovo v buňce. Přidání nic před magické i komentáře, způsobí chybu.   Další informace o magics najdete v tématu [sem](http://ipython.readthedocs.org/en/stable/interactive/magics.html).

    Následující tabulka uvádí různé magics dostupné prostřednictvím jádra.

  	| Magické     | Příklad                         | Popis  |
  	|-----------|---------------------------------|--------------|
  	| Pomoc      | `%%help`                            | Vytvoří tabulku všechny dostupné magics s příklad a popis |
  	| informace o      | `%%info`                          | Informace o aktuálním koncový bod Livius výstupy relaci |
  	| Konfigurace | `%%configure -f`<br>`{"executorMemory": "1000M"`,<br>`"executorCores": 4`} | Konfiguruje parametry pro vytvoření relace. Příznak platnost (-f) je povinný, pokud relaci již byly vytvořeny a relace se nezobrazí a znovu vytvořit. Podívejte se [na Livius příspěvek /sessions požádat o textu](https://github.com/cloudera/livy#request-body) seznam platné parametry. Parametry musí být předaný jako řetězec formátu JSON a musí být na dalším řádku po magické, jak je uvedeno v příkladu sloupců. |
  	| SQL       |  `%%sql -o <variable name>`<br> `SHOW TABLES`    | Spustí dotaz podregistru proti sqlContext. Pokud `-o` předán parametr, je výsledek dotazu zachován v %% místní kontext Python jako [Pandas](http://pandas.pydata.org/) dataframe.   |
  	| místní     |     `%%local`<br>`a=1`              | Všechny kód v dalších řádcích bude spouštět místně. Kód musí být platná Python kód. |
  	| protokoly      | `%%logs`                        | Výstupy protokoly pro aktuální relaci Livius.  |
  	| odstranění    | `%%delete -f -s <session number>` | Odstraní konkrétní relaci aktuální Livius koncového bodu. Všimněte si, že nelze odstranit relaci, která je spuštěná pro jádra samotné. |
  	| Vyčištění   | `%%cleanup -f`                    | Odstraní všechny relace pro aktuální Livius koncový bod, včetně relace tento poznámkový blok. Platnost příznaku -f je povinný.  |

    >[AZURE.NOTE] Kromě magics přidal jádra PySpark, můžete také [vestavěné magics IPython](https://ipython.org/ipython-doc/3/interactive/magics.html#cell-magics), včetně `%%sh`. Můžete použít `%%sh` magické spustit skripty a blok kódu na headnode obrázku. 

3. **Automatické vizualizace**. Jádra **Pyspark** automaticky vizualizuje výstup podregistru a SQL dotazů. Máte možnost si vybrat mezi několika různých typů vizualizací včetně tabulky, výsečový, spojnicový, oblasti, panelu.

## <a name="parameters-supported-with-the-sql-magic"></a>Parametry podporované s %% magické sql

%% Sql magické podporuje různé parametry, které můžete použít k řízení druh výstup, která se zobrazí při spouštění dotazů. Následující tabulka obsahuje záhlaví.

| Parametr     | Příklad                         | Popis  |
|-----------|---------------------------------|--------------|
| -o      | `-o <VARIABLE NAME>`                          | Použijte tento parametr uchovávat výsledků dotazu v %% místní kontextu Python jako [Pandas](http://pandas.pydata.org/) dataframe. Název proměnné dataframe je proměnná název, který určíte. |
| -q      | `-q`                          | Slouží k vypnutí vizualizace na buňku. Pokud nechcete, aby automaticky vizualizace obsah buňky a chcete zachytit jako dataframe a potom použijte `-q -o <VARIABLE>`. Pokud chcete vypnout vizualizace bez zapsání výsledky (například pro spuštění příkazu jazyka SQL s vedlejší efekty, třeba `CREATE TABLE` údajů), budete moct používat jenom `-q` bez zadání `-o` argument. |
| -m       |  `-m <METHOD>`    | Kde je **Metoda** **trvat** nebo **vzorku** (výchozí hodnota je **přijmout**). Pokud je použita metoda **trvat**, použije jádra prvky v horní části nastavil MAXROWS (popsaných níže v této tabulce) uvedenou množinu dat výsledek. **Ukázka**při metodu jádra náhodně přehrajte prvky uvedenou množinu dat podle `-r` parametr, dále popsané v této tabulce.   |
| -r     |     `-r <FRACTION>`            | Tady je **ZLOMEK** 8bajtové číslo mezi 0,0 a 1.0. Pokud je ukázkový metody dotaz SQL zobrazený `sample`, pak jádra náhodně vzorky zadaný zlomek prvky výsledků. Například pokud spustíte dotaz SQL zobrazený pomocí argumentů `-m sample -r 0.01`, pak 1 % řádků výsledek bude náhodně odeberou. |
| -n      | `-n <MAXROWS>`                        | **MAXROWS** je celočíselnou hodnotu. Jádra omezí počet řádků výstup pro **MAXROWS**. **MAXROWS** je záporné číslo, například **-1**, počet řádků v sadě výsledků nesmí být omezené. |

**Příklad:**

    %%sql -q -m sample -r 0.1 -n 500 -o query2 
    SELECT * FROM hivesampletable

Příkaz výše dělá toto:

* Vybere všechny záznamy z **hivesampletable**.
* Protože používáme - q, vypne automatické vizualizace.
* Protože používáme `-m sample -r 0.1 -n 500` náhodně vzorky 10 % či více řádků hivesampletable a omezení velikosti 500 řádků sadu výsledků.
* Nakonec protože jsme použili `-o query2` také ukládání výstupu do dataframe s názvem **dotaz2**.
    

## <a name="considerations-while-using-the-new-kernels"></a>Co byste měli zvážit při používání nového jádra

Podle toho, která jádra používáte (PySpark, nebo Spark) přímo v poznámkových bloků s bude využívat zdroje obrázku.  Pomocí těchto jádra vzhledem k tomu, že jsou přednastavené kontextů, jednoduše ukončení poznámkové bloky nejsou ukončit kontextu a tím i prostředky clusteru, zůstanou používat. Vhodné s jádra PySpark a Spark bude použít možnost **Zavřít a zastavení** v nabídce **soubor** na Poznámkový blok. Ukončuje kontextu a pak ukončí Poznámkový blok.    


## <a name="show-me-some-examples"></a>Zobrazit několik příkladů

Když otevřete Poznámkový blok Jupyter, zobrazí se dvěma složkami na kořenové úrovni.

* Složka **PySpark** obsahuje ukázkové poznámkové bloky, které používají nový jádra **Python** .
* Složka **Scala** obsahuje ukázkové poznámkové bloky, které používají nový jádra **Spark** .

Otevřete Poznámkový blok **00 - [přečtěte si NEJPRVE] Spark Magické jádra funkce** ze složky **PySpark** nebo **Spark** Další informace o různých magics k dispozici. Můžete také další ukázkové poznámkových bloků dostupné v části dvěma složkami se dozvíte, jak dosáhnout různých scénářích Jupyter poznámkových bloků pomocí clusterů HDInsight Spark.

## <a name="where-are-the-notebooks-stored"></a>Kde jsou uloženy poznámkové bloky?

Poznámkové bloky Jupyter jsou uložené na účtu úložiště přidružený k obrázku ve složce **/HdiNotebooks** .  Poznámkové bloky, textových souborů a složky, které vytvoříte z v rámci Jupyter budou blokům WASB.  Například, pokud používáte Jupyter vytvářet složky **Moje_složka** a Poznámkový blok **myfolder/mynotebook.ipynb**, se dostanete poznámkového bloku na `wasbs:///HdiNotebooks/myfolder/mynotebook.ipynb`.  Platí také true, pokud nahrajte Poznámkový blok přímo ke svému účtu úložiště na `/HdiNotebooks/mynotebook1.ipynb`, Poznámkový blok, budou se zobrazovat z Jupyter stejně.  Poznámkové bloky zůstanou v účtu úložiště i po odstranění clusteru.

Způsob, jakým poznámkové bloky jsou uložené na účtu úložiště je kompatibilní se službou HDFS. Ano, pokud SSH do obrázku, které můžete použít příkazy pro správu takto:

    hdfs dfs -ls /HdiNotebooks                            # List everything at the root directory – everything in this directory is visible to Jupyter from the home page
    hdfs dfs –copyToLocal /HdiNotebooks                 # Download the contents of the HdiNotebooks folder
    hdfs dfs –copyFromLocal example.ipynb /HdiNotebooks   # Upload a notebook example.ipynb to the root folder so it’s visible from Jupyter


V případě, že problémy přístupu k účtu úložiště clusteru, poznámkové bloky jsou rovněž uloženo v headnode `/var/lib/jupyter`.

## <a name="supported-browser"></a>Podporované prohlížeče
Poznámkové bloky Jupyter spuštění HDInsight Spark clusterů podporuje jenom na Google Chrome.

## <a name="feedback"></a>Zpětné vazby

Nové jádra jsou v dlouhodobým fáze a bude splatné určitou dobu. To může znamenat, že rozhraní API může změnit tyto jádra zrát. Vážíme by nějaké názory, budete mít při používání tyto nové jádra. To bude velmi užitečné při přizpůsobení konečné verze těchto jádra. Komentáře a názory můžete ponechat ve skupinovém rámečku **komentáře** na konci tohoto článku.


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

* [Pomocí obrázku Spark na HDInsight Zeppelin poznámkových bloků](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Použití externích balíčků s poznámkovými bloky Jupyter](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Instalace Jupyter ve vašem počítači a připojte k HDInsight Spark obrázku](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Přidávání a používání zdrojů

* [Přidávání a používání zdrojů pro Apache Spark cluster v Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Sledování a ladění úlohy výpočetnímu clusteru Apache Spark v HDInsight](hdinsight-apache-spark-job-debugging.md)
