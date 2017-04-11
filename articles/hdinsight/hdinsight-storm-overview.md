<properties
    pageTitle="Úvod k Apache bouře na HDInsight | Microsoft Azure"
    description="Úvod k Apache bouře a zjistěte, jak můžete bouře na HDInsight vytvářet řešení analýzy data v reálném čase v cloudu."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/11/2016"
   ms.author="larryfr"/>

#<a name="introduction-to-apache-storm-on-hdinsight-real-time-analytics-for-hadoop"></a>Úvod k Apache bouře na HDInsight: v reálném čase analýzy Hadoop

Apache bouře na HDInsight umožňuje vytvářet řešení analýzy distribuované, v reálném čase v Azure prostředí pomocí [Apache Hadoop](http://hadoop.apache.org).

##<a name="what-is-apache-storm"></a>Co je Apache bouře?

Apache bouře je systém distribuované chybám, otevřít zdroj výpočtu, který umožňuje zpracování dat v reálném čase s Hadoop. Řešení bouře můžete taky poskytnout možnost k přehrání data, která nebyla zpracována úspěšně první zaručené zpracování dat.

##<a name="why-use-storm-on-hdinsight"></a>Proč používat bouře na HDInsight?

Apache bouře na HDInsight je spravovaných clusteru integrovaný Azure prostředí. Poskytuje tyto klíčové výhody:

* Provede jako služba spravovaných s SLA 99,9 % čas

* Použití jazyka podle svého výběru: podporuje bouře komponenty napsané v **Java** **C#**a **Python**

    * Podporuje kombinaci programovacím jazyce: Přečtěte si dat pomocí jazyka Java a potom zpracovat pomocí C#
    
        > [AZURE.NOTE] C# topologií podporují pouze na serveru s Windows HDInsight clusterů.

    * Použití rozhraní Java **Trident** vytvořit topologií bouře, které podporují "po přesně" zpracování zpráv, trvalé "transakční" úložiště a sadu společná operace analýzy toku

* Obsahuje integrované funkce měřítko zdola nahoru a dolů měřítko: změnit velikost obrázku HDInsight s žádný vliv spuštění bouře topologií

* Integrace s dalšími Azure službami, včetně centrální události, Azure virtuální sítě, databáze SQL, úložiště objektů Blob a DocumentDB

    * Sloučení funkce více clusterů HDInsight pomocí Azure virtuální sítě: vytváření analytických kanály, které používají HDInsight, HBase nebo Hadoop clusterů

Seznam společností, které používáte Apache bouře jejich řešení v reálném čase analýzy najdete v tématu [Bouře Apache pomocí společnosti](https://storm.apache.org/documentation/Powered-By.html).

Začít používat bouře, najdete v článku [Začínáme s bouře na HDInsight][gettingstarted].

###<a name="ease-of-provisioning"></a>Snadnější zřízení

Zřízení nového bouře HDInsight clusteru v minutách. Zadejte název obrázku, velikost, správce účtu a účtu úložiště. Azure vytvoří obrázku, včetně topologií vzorku a správy webového řídicího panelu.

> [AZURE.NOTE] Můžete taky zřizujete bouře clusterů pomocí [Rozhraní příkazového řádku Azure](../xplat-cli-install.md) nebo [Azure Powershellu](../powershell-install-configure.md).

Během 15 minut po odeslání žádosti budete mít nové bouře clusteru se systémem a jste připraveni na jméno v reálném čase analýzy profilace.

###<a name="ease-of-use"></a>Snadnější použití

__Na základě pro Linux bouře na HDInsight clusterů__, se můžete připojit k obrázku pomocí SSH a jejich použití `storm` příkaz Spustit a spravovat topologií. Kromě toho můžete Ambari sledovat službu bouře a bouře uživatelského rozhraní pro sledování a správa pracovního topologií.

Další informace o práci s založené na Linux bouře clusterů najdete v článku [Začínáme s Apache bouře na základě Linux HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md).

__Pro systém Windows bouře na HDInsight clusterů__nástroje HDInsight for Visual Studio umožňují vytvářet C# a hybridní C# / Java topologií a odešlete je bouře clusteru HDInsight.  

![Bouřka vytvářeného projektu](./media/hdinsight-storm-overview/createproject.png)

HDInsight Tools for Visual Studio také poskytuje rozhraní, které vám umožní sledovat a spravovat bouře topologií clusteru.

![Správa bouře](./media/hdinsight-storm-overview/stormview.png)

Příklad použití nástroje HDInsight vytvoření aplikace bouře najdete v článku [vyvíjet C# bouře topologií pomocí nástrojů HDInsight for Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

Další informace o nástrojích HDInsight for Visual Studio najdete v článku [Začínáme s používáním nástroje HDInsight for Visual Studio](../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md).

Každý bouře HDInsight clusteru také poskytuje webová bouře řídicího panelu, kterou odešlete, sledování a správa bouře topologií spuštěné v clusteru.

![Řídicí panel bouře](./media/hdinsight-storm-overview/dashboard.png)

Další informace o používání řídicích panelů bouře najdete v tématu [nasazení a správu Apache bouře topologií v HDInsight](hdinsight-storm-deploy-monitor-topology.md).

Bouře na HDInsight umožňuje snadno integrace se službou Azure rozbočovače události prostřednictvím **Spout centrální události**. Nejnovější verzi této součásti je k dispozici na [https://github.com/hdinsight/hdinsight-storm-examples/tree/master/lib/eventhubs](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/lib/eventhubs). Další informace o použití této součásti najdete v tématu tyto dokumenty.

* [Můžete vyvíjet topologie C#, která používá rozbočovače události Azure](hdinsight-storm-develop-csharp-event-hub-topology.md)

* [Můžete vyvíjet topologie Java, která používá rozbočovače události Azure](hdinsight-storm-develop-java-event-hub-topology.md)

###<a name="reliability"></a>Spolehlivosti

Apache bouře vždy zaručuje, že každé příchozí zprávy budou plně zpracovány, i když analýza dat rozděleno stovky uzlů.

**Nimbus uzel** poskytuje podobnou funkci obsahují Hadoop JobTracker a slouží k přiřazování úkolů na jiné uzly clusteru prostřednictvím **Zookeeper**. Uzly zookeeper koordinaci clusteru a zjednodušíte komunikaci mezi Nimbus a proces **Správce** v jednotlivých uzlech pracovníka. Pokud jeden uzel zpracování přejde, uzel Nimbus bude informován o tom a slouží k přiřazování úkolů a Spojenými daty jiný uzel.

Výchozí konfigurace Apache bouře má mít jenom jeden uzel Nimbus. Bouře na HDInsight spustí dvou Nimbus uzlů. Pokud uzel primárního nezdaří, clusteru HDInsight přepne sekundární uzel během primárního uzlu obnovit.

![Diagram nimbus, zookeeper a správce](./media/hdinsight-storm-overview/nimbus.png)

###<a name="scale"></a>Stupnice

I když můžete zadat počtu uzlů v svůj cluster při vytváření, můžete zvětšit nebo zmenšit clusteru podle zátěží na projektu. Všechny HDInsight clusterů umožňují změnit počtu uzlů v clusteru, dokonce i při zpracování dat.

> [AZURE.NOTE] Umožní využít výhod nových uzlů přidat s použitím měřítka musíte vyrovnání topologií spuštěna před zvýšit velikost obrázku.

###<a name="support"></a>Podpora

Bouře na HDInsight získáváte podpora 24/7 úplné úrovni organizace. Bouře na HDInsight má i SLA 99,9 %. To znamená, že nemůžeme zaručit, aby clusteru měl externí připojení aspoň 99,9 % čas.

##<a name="common-use-cases-for-real-time-analytics"></a>Běžné případy použití pro analýzy v reálném čase

Zde jsou některé běžné scénáře, ke kterým může používáte Apache bouře v HDInsight. Informace o skutečných scénáře přečtěte si [využití bouře společnosti](https://storm.apache.org/documentation/Powered-By.html).

* Internet věcí (IoT)
* Podvod zjišťování
* Sociální analýzy
* Extrahování, transformace, načtení (ETL)
* Sledování sítě
* Hledání
* Mobilní zapojení

##<a name="how-is-data-in-hdinsight-storm-processed"></a>Data v HDInsight bouře zpracování?

Apache bouře spustí **topologií** místo MapReduce úloh, které mohou být znají HDInsight nebo Hadoop. Bouře HDInsight clusteru obsahuje dva typy uzlů: sídlo uzly spuštěné **Nimbus** a pracovní uzly spuštěné **Správce**.

* **Nimbus**: podobně jako JobTracker v Hadoop je zodpovědný za distribuce kód v celém clusteru, přiřazení úkolů na virtuálních počítačích a sledování chyby. HDInsight obsahuje dva Nimbus uzly, není k dispozici žádné jednoho místa selhání bouře na HDInsight

* **Správce**: správce pro každý uzel pracovníka zodpovídá za spuštění a ukončení **pracovní procesy** na uzel.

* **Pracovní proces**: spustí podmnožinu **topologie**. Průběžný topologie rozvržena mnoho pracovní procesy v celé clusteru.

* **Topologie**: definuje graf výpočtu, který procesy datové **proudy** . Na rozdíl od MapReduce úlohy topologií spustit až po ukončení.

* **Toku**: nevázaného sady **záznamů**. Datové proudy jsou vytvořené pomocí **spouts** a **bolts**a jejich využívané **bolts**.

* **N-tici**: Pojmenovaný seznam dynamicky vkládané hodnoty.

* **Spout**: využívá data ze zdroje dat a vydává jeden nebo více **datových proudů**.

    > [AZURE.NOTE] V mnoha případech dat přečíst ze fronty, například Kafka, Bus služby Azure fronty nebo rozbočovače události. Fronty zaručuje, že data uložena při výpadku.

* **Blesku**: spotřebovává **datových proudů**, provede zpracování na **n-ticové**a může posílat **datových proudů**. Prvky procesy zodpovídají taky zápisu dat do externích úložiště, jako je do fronty HDInsight, HBase, objektů blob nebo jiném zdroji dat.

* **Apache Thrift**: rámec software pro vývoj scalable služby mezi jazyky. Umožňuje vytvořit službách, které spolupracují mezi C++, Java, Python, PHP, skutečné, Erlangovo, Perl, Haskell, C#, kakao, JavaScript, Node.js, Smalltalk a jiných jazyků.

    * **Nimbus** je služba Thrift a **topologie** je definici Thrift, takže je možné se dají topologií pomocí různé jazyky.

Další informace o součástech bouře najdete v tématu [kurz bouře] [ apachetutorial] na apache.org.


##<a name="what-programming-languages-can-i-use"></a>Jaké jazyky můžu použít?

Bouře HDInsight clusteru podporuje C# Java a Python.

### <a name="c35"></a>C a #35;

Nástroje HDInsight for Visual Studio .NET vývojářům návrh a implementace topologii v jazyce C# povolit. Můžete taky vytvořit hybridní topologií používající součásti Java a C#.

Další informace najdete v tématu [topologií vyvíjet C# pro Apache bouře na HDInsight pomocí aplikace Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

###<a name="java"></a>Java

Většina Java příkladů, ke kterým dojít bude jednoduchým Java nebo Trident. Trident je uceleném odběru, který usnadňuje postupy například spojení, agregace, seskupení a filtrování. Trident však zpracovává jako sady záznamů, že jako nezpracovaná řešení Java zpracovává jeden n-tici toku najednou.

Další informace o Trident najdete v tématu [Trident kurz](https://storm.apache.org/documentation/Trident-tutorial.html) na apache.org.

Příklady Java a Trident topologií najdete v článku [seznam příklad bouře topologií](hdinsight-storm-example-topology.md) nebo příklady bouře starter clusteru HDInsight.

Příklady bouře starter se nacházejí v adresáři __/usr/hdp/current/storm-client/contrib/storm-starter__ na základě Linux clusterů a adresáři **%storm_home%\contrib\storm-starter** na serveru s Windows clusterů.

##<a name="what-are-some-common-development-patterns"></a>Jaké jsou některé běžné vzory vývoj?

###<a name="guaranteed-message-processing"></a>Zpracování zaručené zprávy

Bouře můžete poskytují různé úrovně zpracování zaručené zprávy. Například aplikace základní bouře zaručit zpracování na aspoň jednou a přesně zaručit Trident-jednou zpracování.

Další informace najdete v tématu [záruky na zpracování dat](https://storm.apache.org/about/guarantees-data-processing.html) na apache.org.

###<a name="ibasicbolt"></a>IBasicBolt

Vzorek čtení vstupní záznam, výstupu nula nebo více záznamů a potom acking vstupní n-tici hned na konci metody execute je velmi běžné a bouře poskytuje rozhraní [IBasicBolt](https://storm.apache.org/apidocs/backtype/storm/topology/IBasicBolt.html) k automatizaci tohoto vzorku.

###<a name="joins"></a>Spojení

Připojení ke dvěma datovými proudy dat se budou lišit mezi aplikacemi. Například může připojit každý záznam z více datových proudů do jedné nové toku nebo by mohl připojit pouze listy řazená pro konkrétní okna. V obou případech připojení lze provést pomocí [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29), která je způsob definování směrování n-ticové na prvky.

V následujícím příkladu Java fieldsGrouping slouží ke směrování n-ticové, že pocházejí z součásti "1", "2" a "3" blesku **MyJoiner** .

    builder.setBolt("join", new MyJoiner(), parallelism) .fieldsGrouping("1", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("2", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("3", new Fields("joinfield1", "joinfield2"));

###<a name="batching"></a>Dávkové

Dávky lze provést několika způsoby. Základní topologie bouře Java může před výstupu je použít jednoduchý proti číslo dávky X řazená nebo použít mechanismus vnitřní časování označovány jako "popisky řazené kolekce členů" posílat dávky každých X sekund.

Příklad použití n-ticové popisky najdete v článku [Analýza dat senzor s bouře a HBase na HDInsight](hdinsight-storm-sensor-data-analysis.md).

Pokud používáte Trident, je založena na zpracování sady záznamů.

###<a name="caching"></a>Ukládání do mezipaměti

Ukládání do mezipaměti v paměti často používá mechanismus pro urychlení zpracování důvodu udržuje často používané prostředky v paměti. Protože topologii rozvržena více uzlů a více procesů v jednotlivých uzlech, zvažte použití [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29) zajistit, že n-ticové obsahující pole, která se používají pro vyhledávání v mezipaměti, budou vždycky přesměrované do stejným způsobem. Duplikování mezipaměť se vyhnete přes procesů.

###<a name="streaming-top-n"></a>Přenos horních N

Pokud topologii závisí na výpočtech v hodnotě "horních N", například horního 5 trendů na Twitter, by měl výpočet hodnoty horních N paralelně a sloučit výstup z těchto výpočtů do globální hodnotu. Lze provést pomocí [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29) ke směrování polem paralelní prvky, (které oddíly data podle hodnot polí) a potom směrovat role globálně určující hodnotu horních N.

Příklad tohoto podívejte se na příklad [RollingTopWords](https://github.com/nathanmarz/storm-starter/blob/master/src/jvm/storm/starter/RollingTopWords.java) .

##<a name="what-type-of-logging-does-storm-use"></a>Jaký druh protokolování bouřka použít?

Bouře používá Apache Log4j přihlásit informace. Ve výchozím nastavení zaznamenané velkého množství dat a může být obtížné seřadit prostřednictvím informací. Konfigurační soubor protokolování můžete zahrnout jako součást topologii bouře ovládacímu prvku protokolování.

Příklad topologie, který ukazuje, jak nakonfigurovat protokolování najdete v článku [na základě Java WordCount](hdinsight-storm-develop-java-topology.md) příklad pro bouře na HDInsight.

##<a name="next-steps"></a>Další kroky

Další informace o řešení v reálném čase analýzy pomocí Apache bouře v HDInsight:

* [Začínáme s bouře HDInsight][gettingstarted]

* [Příklad topologie pro bouře na HDInsight](hdinsight-storm-example-topology.md)

[stormtrident]: https://storm.apache.org/documentation/Trident-API-Overview.html
[samoa]: http://yahooeng.tumblr.com/post/65453012905/introducing-samoa-an-open-source-platform-for-mining
[apachetutorial]: https://storm.apache.org/documentation/Tutorial.html
[gettingstarted]: hdinsight-apache-storm-tutorial-get-started-linux.md
