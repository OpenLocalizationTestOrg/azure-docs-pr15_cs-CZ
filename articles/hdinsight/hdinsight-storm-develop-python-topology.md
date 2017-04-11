<properties
   pageTitle="Pomocí Python komponent topologie bouře na HDinsight | Microsoft Azure"
   description="Zjistěte, jak pomocí Python součásti z s Apache bouře na Azure HDInsight. Se dozvíte, jak používat Python součásti z obou Java, na základě a Clojure na základě bouře topologie."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="python"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="larryfr"/>

#<a name="develop-apache-storm-topologies-using-python-on-hdinsight"></a>Můžete vyvíjet Apache bouře topologií pomocí Python na HDInsight

Apache bouře podporuje více jazyků i umožňují kombinovat součásti z několika jazyků v jedné topologii. V tomto dokumentu budou Naučte se používat Python komponenty ve vaší Java a na základě Clojure bouře topologie na HDInsight.

##<a name="prerequisites"></a>Zjistit předpoklady pro

* Python 2.7 nebo vyšší

* Java JDK 1.7 nebo vyšší

* [Leiningen](http://leiningen.org/)

##<a name="storm-multi-language-support"></a>Bouře podporu více jazyků

Bouře byl navržený pro práci s komponenty napsané pomocí programovacím jazykem, ale tento postup vyžaduje, aby komponenty pochopit, jak pro práci s [Thrift definici bouře](https://github.com/apache/storm/blob/master/storm-core/src/storm.thrift). Pro Python modulu zadané jako součást Apache bouře projekt, který umožňuje snadno rozhraní bouře. Vyhledání tento modul na [https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py](https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py).

Protože Apache bouře je proces Java spouštějící na Java Virtual Machine (JVM), jsou jako podprocesy spouštět komponenty napsané v jiných jazycích. Spuštění v JVM bitů bouře komunikaci se tyto podprocesy pomocí JSON zprávám odesílaným prostřednictvím standardního/stdout. Další informace o komunikaci mezi součástmi najdete v dokumentaci [protokol více jazyků](https://storm.apache.org/documentation/Multilang-protocol.html) .

###<a name="the-storm-module"></a>Modul bouře

Modul bouře (https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py,) poskytuje potřebné k vytvoření Python součástí, které spolupracují s bouře bitů.

Zajistíte, například `storm.emit` k posílat záznamů, a `storm.logInfo` k zápisu protokolů. Můžu byste rádi, když přes tento soubor čtení a interpretaci jej obsahuje.

##<a name="challenges"></a>Úkoly

Používání modulu __storm.py__ , můžete vytvořit spouts Python využívající data a prvky, které zpracovávají data, ale celkový bouře topologie definici, která vedení vodiče komunikace mezi součástmi je pořád zadána Java nebo Clojure. Kromě toho pokud používáte Java, musíte taky vytvořit součástí jazyka Java, které fungují jako rozhraní komponent Python.

Navíc od bouře clusterů spuštění distribuované způsobem, musíte se ujistit, že všechny moduly požadováno součástmi Python jsou k dispozici ve všech kolegy uzlech clusteru. Bouře nenabízí možnost snadný způsob, jak jsou k tomuto účelu pro více jazyků zdroje – buď musíte jako součást souboru sklenice topologii zahrnout všechny závislosti, nebo ruční instalace závislosti v jednotlivých uzlech pracovníka clusteru.

###<a name="java-vs-clojure-topology-definition"></a>Definice topologie Java porovnání Clojure

Dva způsoby definování topologii Clojure je mnohem nejjednodušší/cleanest jako přímo referenc python součástí definici topologie. Na základě Java topologie definice definujte součástí jazyka Java, kteří zpracovávat věci, jako deklarace pole v n-ticové vrácených z části Python.

V tomto dokumentu, spolu s projekty příkladu jsou popsány obě metody.

##<a name="python-components-with-a-java-topology"></a>Python komponent topologie Java

> [AZURE.NOTE] V tomto příkladu je k dispozici v adresáři __JavaTopology__ [https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount) . Toto je Maven na základě projektu. Pokud Maven neznáte, přečtěte si téma [na základě vyvíjet Java topologií s Apache bouře na HDInsight](hdinsight-storm-develop-java-topology.md) Další informace o vytváření Maven projektu pro bouře topologie.

Na základě Java topologie, která používá Python (nebo jiných komponent jazyk JVM) původně zdá se používá součástí jazyka Java; ale pokud hledáte ve všech Java spouts/prvky, zobrazí se podobně jako tento kód:

    public SplitBolt() {
        super("python", "countbolt.py");
    }

Toto je místo, kam Java vyvolá Python a spustí skript, který obsahuje logiku skutečné blesku. Java spouts/prvky (například) jednoduše deklarovat polí v n-tici, který bude vypuštění základní součást Python.

Skutečné Python soubory uložené v `/multilang/resources` adresáře v tomto příkladu. `/multilang` Adresáře se odkazuje ve __pom.xml__:

<resources>
    <resource>
        <!-- Where the Python bits are kept -->
        <directory>${basedir} / multilang</directory>
    </resource>
</resources>

Tato volba zahrnuje všechny soubory v `/multilang` složky v sklenice, který bude vytvořen z tohoto projektu.

> [AZURE.IMPORTANT] Všimněte si, že to jenom Určuje `/multilang` adresář a ne `/multilang/resources`. Bouře očekává není JVM zdrojů v `resources` adresáře, tak je interně už hledáte. Uvedení součásti v této složce umožňuje jenom odkaz podle názvu v kódu jazyka Java. Například `super("python", "countbolt.py");`. Další způsob, jak to je, že bouře uvidí `resources` adresáře jako kořenovou (/) při přístupu k prostředkům více jazyků.
>
> Pro tento projekt příklad `storm.py` modul je součástí `/multilang/resources` adresář.

###<a name="build-and-run-the-project"></a>Vytvoření a spuštění projektu

Spuštění tohoto projektu místně, můžete tento příkaz Maven k vytvoření a spuštění v místním režimu:

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCount

Podržením klávesy ctrl + c ukončit proces.

Nasazení projektu do HDInsight clusteru systémem Apache bouře, použijte následující kroky:

1. Vytvoření uber sklenice:

        mvn package

    Tím vytvoříte do souboru nazvaného __WordCount – 1.0 SNAPSHOT.jar__ v `/target` adresáře pro tento projekt.

2. Uložte soubor sklenice Hadoop clusteru pomocí jedné z těchto způsobů:

    * __Na základě Linux__ clusterů HDInsight: použití `scp WordCount-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:WordCount-1.0-SNAPSHOT.jar` zkopírujte soubor sklenice clusteru nahrazení uživatelské jméno SSH uživatelské jméno a NÁZEV_CLUSTERU s názvem clusteru HDInsight.

        Po dokončení nahrávání souboru připojení k clusteru pomocí SSH a začněte používat topologie`storm jar WordCount-1.0-SNAPSHOT.jar com.microsoft.example.WordCount wordcount`

    * U __serveru s Windows__ clusterů HDInsight: připojení k řídicím panelu bouře tak, že přejdete do HTTPS://CLUSTERNAME.azurehdinsight.net/ v prohlížeči. Nahraďte název clusteru HDInsight NÁZEV_CLUSTERU a zadejte jménem a heslem správce po zobrazení výzvy.

        Pomocí formuláře, proveďte následující akce:

        * __Soubor JAR__: vyberte __Procházet__a pak vyberte požadovaný soubor __WordCount 1.0 SNAPSHOT.jar__
        * __Název třídy__: Zadejte`com.microsoft.example.WordCount`
        * __Další prospívání__: Zadejte popisný název, jako `wordcount` k identifikaci topologii

        Nakonec vyberte __Odeslat__ zahájíte topologii.

> [AZURE.NOTE] Po spuštění bouře topologie spustí až do ukončení (usmrcenou.) Topologie ukončíte použijte některý `storm kill TOPOLOGYNAME` příkazu z příkazového řádku (SSH relace clusteru Linux například) nebo pomocí rozhraní bouře vyberte topologii a pak klikněte na tlačítko __Ukončit__ .

##<a name="python-components-with-a-clojure-topology"></a>Python komponent topologie Clojure

> [AZURE.NOTE] V tomto příkladu je k dispozici v adresáři __ClojureTopology__ [https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount) .

Tato topologie byl vytvořený pomocí [Leiningen](http://leiningen.org) k [Vytvoření nového projektu Clojure](https://github.com/technomancy/leiningen/blob/stable/doc/TUTORIAL.md#creating-a-project). Poté byly provedeny následující změny scaffolded projektu:

* __Project.CLJ__: přidán závislosti u bouře a vyloučení u položek, které mohou způsobit potíže při nasazení serveru HDInsight.
* __/ prostředky__: Leiningen vytvoří výchozí `resources` adresáře, ale soubory uložené v tomto poli zdá se přidá do kořenové sklenice soubor vytvořený z tohoto projektu a bouře očekává souborů v podřízené adresář s názvem `resources`. Aby bylo přidáno adresář a Python soubory, které jsou uložené v `resources/resources`. Za běhu to se použije jako kořenovou (/) pro přístup k Python součásti.
* __src/WordCount/Core.CLJ__: Tento soubor obsahuje definici topologie a odkazuje ze souboru __project.clj__ . Další informace o použití Clojure definovat bouře topologie najdete v článku [Clojure DSL](https://storm.apache.org/documentation/Clojure-DSL.html).

###<a name="build-and-run-the-project"></a>Vytvoření a spuštění projektu

__K vytvoření a spuštění projektu místně__, použijte tento příkaz:

    lein clean, run

Topologie ukončíte pomocí __Kombinace kláves Ctrl + C__.

__Uberjar sestavovat a nasazovat k Hdinsightu__, postupujte podle následujících kroků:

1. Vytvoření uberjar obsahující topologie a požadovaných závislostí:

        lein uberjar

    Tím vytvoříte nový soubor s názvem `wordcount-1.0-SNAPSHOT.jar` v `target\uberjar+uberjar` adresář.
    
2. Pomocí jedné z následujících metod nasazení a organizovat topologii HDInsight clusteru:

    * __Na základě Linux HDInsight__
    
        1. Kopírování souboru do hlavního uzlu clusteru HDInsight pomocí `scp`. Příklad:
        
                scp wordcount-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:wordcount-1.0-SNAPSHOT.jar
                
            Nahraďte uživatelské jméno uživatele SSH pro obrázku a NÁZEV_CLUSTERU se svým názvem clusteru HDInsight.
            
        2. Po zkopírování souboru do clusteru umožňuje SSH připojte se k němu a odesílat úkoly. Informace o použití SSH s HDInsight najdete v jednom z těchto věcí:
        
            * [Použití SSH s na základě Linux HDInsight z Linux, Unix nebo OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
            * [Použití SSH s na základě Linux HDInsight z Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
            
        3. Po připojení, použijte následující zahájíte topologii:
        
                storm jar wordcount-1.0-SNAPSHOT.jar wordcount.core wordcount
    
    * __HDInsight serveru s Windows__
    
        1. Připojení k řídicím panelu bouře tak, že přejdete do HTTPS://CLUSTERNAME.azurehdinsight.net/ v prohlížeči. Nahraďte název clusteru HDInsight NÁZEV_CLUSTERU a zadejte jménem a heslem správce po zobrazení výzvy.

        2. Pomocí formuláře, proveďte následující akce:

            * __Soubor JAR__: vyberte __Procházet__a pak vyberte požadovaný soubor __wordcount 1.0 SNAPSHOT.jar__
            * __Název třídy__: Zadejte`wordcount.core`
            * __Další prospívání__: Zadejte popisný název, jako `wordcount` k identifikaci topologii

            Nakonec vyberte __Odeslat__ zahájíte topologii.

> [AZURE.NOTE] Po spuštění bouře topologie spustí až do ukončení (usmrcenou.) Topologie ukončíte použijte některý `storm kill TOPOLOGYNAME` příkazu z příkazového řádku (SSH relace Linux clusteru) nebo pomocí rozhraní bouře vyberte topologii a pak klikněte na tlačítko __Ukončit__ .

##<a name="next-steps"></a>Další kroky

V tomto dokumentu můžete naučíte používat Python součásti z bouře topologie. Tyto dokumenty pro další způsoby, jak používat Python s Hdinsightu najdete tady:

* [Jak používat Python datového proudu MapReduce úlohy](hdinsight-hadoop-streaming-python.md)
* [Jak používat Python uživatele definované funkce (UDF) v Prasátko a podregistru](hdinsight-python.md)
