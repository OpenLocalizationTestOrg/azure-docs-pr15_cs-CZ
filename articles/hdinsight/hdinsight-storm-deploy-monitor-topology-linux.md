<properties
   pageTitle="Nasazením a správou Apache bouře topologií na základě Linux HDInsight | Microsoft Azure"
   description="Naučte se nasadit, sledování a správa Apache bouře topologií pomocí řídicího panelu bouře na základě Linux HDInsight. Použití Hadoop tools for Visual Studio."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/07/2016"
   ms.author="larryfr"/>

# <a name="deploy-and-manage-apache-storm-topologies-on-linux-based-hdinsight"></a>Nasazením a správou Apache bouře topologie na základě Linux HDInsight

V tomto dokumentu Naučte se základy Správa a sledování bouře topologií na základě Linux bouře na clusterů HDInsight.

> [AZURE.IMPORTANT] Kroky v tomto článku vyžadují bázi Linux bouře clusteru HDInsight. Informace o nasazení a sledování topologie na serveru s Windows Hdinsightu najdete v tématu [nasazení a správu Apache bouře topologie na serveru s Windows HDInsight](hdinsight-storm-deploy-monitor-topology.md)

## <a name="prerequisites"></a>Zjistit předpoklady pro

* **Na základě A Linux bouře HDInsight clusteru**: naleznete v tématu [Začínáme s Apache bouře na HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md) kroky k vytvoření clusteru

* **Znalost SSH a spojovací bod služby**: Další informace o použití SSH a spojovací bod služby s Hdinsightu najdete v článku následující:

    * **Klienti Linux, Unix nebo OS X**: najdete v článku [Použití SSH s bázi Linux Hadoop na HDInsight z Linux, OS X nebo Unix](hdinsight-hadoop-linux-use-ssh-unix.md)

    * **Klienti se systémem Windows**: najdete v článku [Použití SSH s Hadoop Linux založené na HDInsight z Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

* **Klient spojovací bod služby**: to je součástí všech systémy Linux, Unix a OS X. Pro klienty Windows doporučujeme PSCP, která je dostupná z [nátěrové stáhnout stránky](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

## <a name="start-a-storm-topology"></a>Zahájení bouře topologie

### <a name="using-ssh-and-the-storm-command"></a>Pomocí příkazu bouře a SSH

1. Připojení k obrázku HDInsight pomocí SSH. Nahrazení **uživatelské jméno** název SSH přihlášení. Nahraďte **NÁZEV_CLUSTERU** názvu HDInsight obrázku:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    Další informace o použití SSH se připojit k obrázku Hdinsightu najdete v článku tyto dokumenty:
    
    * **Klienti Linux, Unix nebo OS X**: najdete v článku [Použití SSH s Hadoop Linux založené na HDInsight z Linux, OS X nebo Unix](hdinsight-hadoop-linux-use-ssh-unix.md)
    
    * **Klienti se systémem Windows**: najdete v článku [Použití SSH s Hadoop Linux založené na HDInsight z Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Pomocí následujícího příkazu spustit příklad topologie:

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-0.9.3.2.2.4.9-1.jar storm.starter.WordCountTopology WordCount

    Příklad WordCount topologie bude vytvořena na clusteru. Bude náhodně generovat věty a určení počtu výskytů každého slova ve větě.

    > [AZURE.NOTE] Při odesílání topologie clusteru, je nutné nejprve zkopírovat sklenice soubor, ve kterém clusteru před použitím `storm` příkaz. Můžete to provést pomocí `scp` příkazu z klienta, kde soubor existuje. Například`scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`
    >
    > Příklad WordCount a další příklady starter bouře již nacházejí na svůj cluster v `/usr/hdp/current/storm-client/contrib/storm-starter/`.

### <a name="programmatically"></a>Programově

Topologie můžete bouře na HDInsight nasadit programově komunikace se službou Nimbus použitý ve svůj cluster. [https://github.com/Azure-Samples/hdinsight-Java-Deploy-Storm-Topology](https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology) poskytuje příklad aplikaci Java, která ukazuje, jak nasazení a začněte topologie prostřednictvím služby Nimbus.

## <a name="monitor-and-manage-using-the-storm-command"></a>Sledování a spravovat pomocí příkazu bouře

`storm` Nástroj umožňuje pracovat s spuštění topologií z příkazového řádku. Následuje seznam běžně používaných příkazů. Použití `storm -h` úplný seznam příkazů.

### <a name="list-topologies"></a>Seznam topologií

Pomocí následujícího příkazu seznam všech spuštěný topologií:

    storm list
    
Vrátí to informace podobná této:

    Topology_name        Status     Num_tasks  Num_workers  Uptime_secs
    -------------------------------------------------------------------
    WordCount            ACTIVE     29         2            263

### <a name="deactivate-and-reactivate"></a>Deaktivujte a opětovnou aktivaci

Deaktivace topologii pozastaví ho, dokud není ukončit nebo znovu aktivovat. Pomocí následujících deaktivujte a znovu aktivovat:

    storm Deactivate TOPOLOGYNAME
    
    storm Activate TOPOLOGYNAME

### <a name="kill-a-running-topology"></a>Ukončení pracovního topologie

Topologie bouře po spuštění, zůstanou spuštěna až do ukončení. Ukončit topologie, použijte tento příkaz:

    storm stop TOPOLOGYNAME

### <a name="rebalance"></a>Vyrovnání

Nové posouzení topologie umožňuje systému opravit paralelismus topologie. Například, pokud jste změnili clusteru přidávat další poznámky, nové posouzení vám umožní pracovního topologie aby použít nové uzlů.

> [AZURE.WARNING] Nové posouzení topologii nejdřív deaktivuje topologii, provede redistribuci pracovníků rovnoměrně přes clusteru, pak nakonec vrátí topologii do stavu, před vytvořením nové posouzení došlo k chybě. Takže pokud topologii byl aktivní, bude aktivní znovu. Pokud je deaktivovaná, bude platit deaktivovaný.

    storm rebalance TOPOLOGYNAME

## <a name="monitor-and-manage-using-the-storm-ui"></a>Sledování a spravovat pomocí bouře uživatelského rozhraní

Uživatelské rozhraní bouře poskytuje webového rozhraní pro práci se systémem topologií a zahrnuta clusteru HDInsight. Uživatelské rozhraní bouře zobrazíte pomocí webového prohlížeče otevřete __https://CLUSTERNAME.azurehdinsight.net/stormui__kde __NÁZEV_CLUSTERU__ je název vašeho obrázku.

> [AZURE.NOTE] Pokud budete vyzváni k zadání uživatelského jména a hesla, zadejte Správce clusteru (Správci) a heslo, které jste použili při vytváření clusteru.


### <a name="main-page"></a>Hlavní stránky

Hlavní stránky uživatelského rozhraní bouře poskytuje následující údaje:

* **Shluk souhrnné**: základní informace o bouře obrázku.

* **Topologie shrnutí**: seznam spuštěných topologií. Pomocí odkazů v této části zobrazíte další informace o konkrétních topologií.

* **Správce shrnutí**: informace o bouře správce.

* **Konfigurace nimbus**: Konfigurace Nimbus clusteru.

### <a name="topology-summary"></a>Topologie souhrn

Výběr odkazu z části **topologie souhrnné** zobrazí tyto informace o topologii:

* **Topologie souhrnné**: základní informace o topologii.

* **Topologie akce**: Správa akce, které můžete provádět pro topologii.

  * **Aktivace**: životopisy zpracování deaktivovaný topologie.

  * **Deaktivovat**: Pozastaví pracovního topologie.

  * **Vyrovnání**: upraví paralelismus topologie. Průběžný topologií by měl vyrovnání po změně počtu uzlů v clusteru. Díky topologii upravte paralelismus pro zvýšení nebo snížení počtu uzlů v clusteru.

    Další informace najdete v tématu <a href="http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">Princip paralelismus bouře topologie</a>.

  * **Odstranění**: ukončí bouře topologie po zadaného časového limitu.

* **Topologie stat**: statistických údajů o topologii. Chcete-li nastavit časový rámec pro zbývající položky na stránce pomocí odkazů ve sloupci **okna** .

* **Spouts**: spouts používaný topologie. Chcete-li zobrazit další informace o konkrétních spouts pomocí odkazů v této části.

* **Bolts**: prvky používaný topologii. Chcete-li zobrazit další informace o konkrétní prvky pomocí odkazů v této části.

* **Konfigurace topologie**: Konfigurace vybrané topologie.

### <a name="spout-and-bolt-summary"></a>Hubičky a blesku souhrn

Výběr hubičkou z části **Spouts** nebo **Bolts** zobrazí tyto informace o vybrané položky:

* **Součásti souhrnné**: základní informace o hubičky nebo blesku.

* **Hubičky/blesku stat**: statistických údajů o hubičky nebo blesku. Chcete-li nastavit časový rámec pro zbývající položky na stránce pomocí odkazů ve sloupci **okna** .

* **Vstupní stat** (pouze šroubu): informace o zadávání datových proudů využívané šroubu.

* **Výstup stat**: informace o datových proudů které to spout nebo šroubu.

* **Vykonavatelů**: informace o instancemi hubičky nebo blesku. Vyberte položku **Port** pro konkrétní prováděči zobrazení protokolu diagnostické informace pro tuto instanci.

* **Chyby**: všechny informace o chybě pro tuto spout nebo šroubu.

## <a name="rest-api"></a>ROZHRANÍ REST API

Uživatelské rozhraní bouře je této technologii postavené rozhraní REST API, můžete provádět podobné pro správu a sledování funkce pomocí rozhraní REST API. Rozhraní REST API slouží k vytvoření vlastního nástroje pro správu a sledování bouře topologie.

Další informace najdete v tématu [Bouře uživatelského rozhraní REST API](http://storm.apache.org/releases/0.9.6/STORM-UI-REST-API.html). Následující informace jsou specifické pro rozhraní REST API pomocí Apache bouře na HDInsight.

> [AZURE.IMPORTANT] Rozhraní REST API bouře není veřejně dostupný přes internet a musí se otevřít prostřednictvím SSH tunelem hlavy uzel clusteru HDInsight. Informace o vytváření a používání SSH tunelem najdete v tématu [Použití SSH Tunneling přístup k webu Ambari uživatelského rozhraní, ResourceManager, JobHistory, NameNode, Oozie a jiným webovým uživatelském rozhraní na](hdinsight-linux-ambari-ssh-tunnel.md).

### <a name="base-uri"></a>Base URI

Základní URI pro rozhraní REST API na základě Linux HDInsight clusterů je k dispozici na uzel hlavy na **https://HEADNODEFQDN:8744/rozhraní api/v1/**; název domény hlavního uzlu však generováno během vytváření clusteru a není statické.

Plně kvalifikovaný název domény (FQDN) pro hlavy clusteru můžete najít několika různými způsoby:

* __Z SSH relace__: použijte příkaz `headnode -f` z relaci SSH clusteru.

* __Z webu Ambari__: vyberte __služby__ v horní části stránky a potom vyberte __bouře__. Na kartě __souhrnné informace__ výběrem __Server bouře uživatelského rozhraní__. Plně kvalifikovaný název domény uzel bouře uživatelského rozhraní a rozhraní REST API běží na budou v horní části stránky.

* __Z Ambari REST API__: použijte příkaz `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/STORM/components/STORM_UI_SERVER"` k vyhledání informací o uzel bouře uživatelského rozhraní a rozhraní REST API spuštěných pro. Nahraďte __heslo__ heslo správce pro clusteru. __NÁZEV_CLUSTERU__ nahraďte názvem obrázku. Položka "název_hostitele" v odpovědi obsahuje plně kvalifikovaný název domény na uzel.

### <a name="authentication"></a>Ověřování

Požadavky na rozhraní REST API musíte použít **základní ověřování**, abyste používat HDInsight clusteru jména a hesla správce.

> [AZURE.NOTE] Protože základní ověřování odeslaný pomocí formátu prostého textu, měli byste **vždy** použít HTTPS k zabezpečení komunikace s clusteru.

### <a name="return-values"></a>Vrácené hodnoty

Informace, které vrácená rozhraní REST API lze pouze použitelné z obrázku nebo virtuálních počítačích ve stejné síti Azure virtuální jako clusteru. Například plně kvalifikovaný název domény (FQDN) pro Zookeeper servery vrací nebude přístupný z Internetu.

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili jak nasazení a sledovat pomocí řídicího panelu bouře topologií se dozvíte, jak [na základě vyvíjet Java topologií pomocí Maven](hdinsight-storm-develop-java-topology.md).

Seznam Další příklad topologie najdete v tématu [Příklad topologie pro bouře na HDInsight](hdinsight-storm-example-topology.md).
