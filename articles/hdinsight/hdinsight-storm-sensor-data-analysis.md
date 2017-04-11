<properties
   pageTitle="Analýza dat pomocí Apache bouře a HBase senzor | Microsoft Azure"
   description="Zjistěte, jak se připojit k Apache bouře pomocí virtuální sítě. Pomocí bouře HBase data snímače obrázku z rozbočovači událostí a vizualizovat s D3.js."
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
   ms.date="09/20/2016"
   ms.author="larryfr"/>

# <a name="analyze-sensor-data-with-apache-storm-event-hub-and-hbase-in-hdinsight-hadoop"></a>Analýza dat senzor pomocí Apache bouře centrální událostí a HBase v HDInsight (Hadoop) 

Zjistěte, jak pomocí Apache bouře na HDInsight proces senzor dat z Azure události centrální, uložte ho do Apache HBase na HDInsight a vizualizovat pomocí D3.js spuštěný jako webovou aplikaci Azure.

Správce prostředků Azure Šablona použitá v tomto dokumentu ukazuje, jak vytvořit více Azure zdrojů ve skupině prostředků. Konkrétně vytvoří virtuální sítě Azure, dvě HDInsight clusterů (bouře a HBase) a webovou aplikaci Azure. Provádění node.js řídicího panelu v reálném čase web je automaticky nasazených na web appu.

> [AZURE.NOTE] Informace v tomto dokumentu a v příkladu, máte otestování pomocí na základě Linux 3.3 HDInsight a 3.4 verze obrázku.

## <a name="prerequisites"></a>Zjistit předpoklady pro

* Předplatné Azure. Viz [získání Azure bezplatnou zkušební verzi](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

    > [AZURE.IMPORTANT] Nepotřebujete existujícího HDInsight clusteru; kroky v tomto dokumentu vytvoří v následujících zdrojích:
    >
    > * Azure virtuální sítě
    > * Bouře HDInsight clusteru (na základě Linux, 2 uzly pracovníka)
    > * HBase HDInsight clusteru (na základě Linux, 2 uzly pracovníka)
    > * Azure v prohlížeči, který je hostitelem web řídicího panelu

* [Node.js](http://nodejs.org/): slouží k zobrazení náhledu řídicího panelu web místně na vývojové prostředí.

* [Java a JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/index.html): slouží k vývoji topologii bouře.

* [Maven](http://maven.apache.org/what-is-maven.html): slouží k vytváření a kompilaci projektu.

* [Libovolná](http://git-scm.com/): slouží ke stažení projektu z GitHub.

* Klient s __SSH__ : připojuje k clusterů na základě Linux HDInsight. Další informace o použití SSH s Hdinsightu najdete v článku tyto dokumenty.

    * [Pomocí SSH HDInsight z klienta se systémem Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

    * [Pomocí SSH HDInsight z klienta Linux, Unix nebo Mac](hdinsight-hadoop-linux-use-ssh-unix.md)

    > [AZURE.NOTE] Musí mít taky přístup k `scp` příkaz, který slouží ke kopírování souborů mezi místním vývojové prostředí a clusteru HDInsight pomocí SSH.

## <a name="architecture"></a>Architektura

![diagram architektury](./media/hdinsight-storm-sensor-data-analysis/devicesarchitecture.png)

V tomto příkladě se skládá z těchto složek:

* **Azure události rozbočovače**: obsahuje data, která se shromažďují ze senzorů. V tomto příkladu je aplikace za předpokladu, že data, která generuje.

* **Bouře na HDInsight**: poskytuje v reálném čase zpracování dat z centrální události.

* **HBase na HDInsight**: poskytuje trvalý úložiště dat NoSQL data po zpracování tak, že bouře.

* **Služby Azure virtuální sítě**: umožňuje zabezpečená komunikace mezi bouře na HDInsight a HBase na clusterů HDInsight.

    > [AZURE.NOTE] Virtuální sítě, je potřeba k použití rozhraní API klienta Java HBase jak nebude vystaven přes veřejnou brány HBase clusterů. Instalace HBase a bouře clusterů do stejné virtuální síti umožňuje bouře obrázku (nebo jiný systém virtuální síti se systémem) přímý přístup k HBase pomocí rozhraní API klienta.

* **Řídicí panel webu**: ukázkovém řídicím panelu, který grafy data v reálném čase.

    * Na webu je součástí Node.js tak, aby ho mohlo by umožnit spuštění na žádný operační systém klienta pro účely testování nebo může být nasazené na Azure weby.

    * [Socket.IO](http://socket.io/) se používá pro komunikaci v reálném čase mezi topologii bouře a webem.

        > [AZURE.NOTE] Toto je podrobností implementace. Můžete použít libovolný komunikační rámec nezpracovanými WebSockets ATP SignalR.

    * [D3.js](http://d3js.org/) slouží ke znázornění dat, která se odesílá na web.

> [AZURE.IMPORTANT] Dva clusterů podporují, je žádné podporované způsob vytvoření jednoho obrázku HDInsight bouře a HBase.

Topologie načte data z centrální událostí pomocí třídy [org.apache.storm.eventhubs.spout.EventHubSpout](http://storm.apache.org/releases/0.10.1/javadocs/org/apache/storm/eventhubs/spout/class-use/EventHubSpout.html) a zápisu dat do HBase pomocí třídy [org.apache.storm.hbase.bolt.HBaseBolt](https://storm.apache.org/javadoc/apidocs/org/apache/storm/hbase/bolt/class-use/HBaseBolt.html) . Komunikace s webem lze provést pomocí [socket.io client.java](https://github.com/nkzawa/socket.io-client.java).

Následující obrázek je příklad některých dalších funkcí topologii.

![diagram topologie](./media/hdinsight-storm-sensor-data-analysis/sensoranalysis.png)

> [AZURE.NOTE] Toto je velmi zjednodušený topologii. Za běhu je vytvořena instance jednotlivé součásti pro každý oddíl pro centrální události, která čte. Tyto instance jsou rozvržena uzlů v clusteru a data směrován mezi nimi následujícím způsobem:
>
> * Data z hubičky analyzátor jsou rozloženy.
> * Data z analyzátor řídicích panelů a HBase jsou seskupena podle ID zařízení tak, že zprávy od stejné zařízení vždy flow stejné součásti.

### <a name="topology-components"></a>Komponent topologie

* **EventHub hubičky**: hubičky je zadaný jako součást Apache bouře verze 0.10.0 nebo novější.

    > [AZURE.NOTE] V tomto příkladě použita hubičky rozbočovače události vyžaduje bouře verze obrázku HDInsight 3.3 nebo 3.4. Informace o tom, jak pomocí starší verzí systému bouře na HDInsight rozbočovače události najdete v tématu [Proces události z Azure události rozbočovače s bouře na HDInsight](hdinsight-storm-develop-java-event-hub-topology.md).

* **ParserBolt.java**: data, která je vyzařovaného hubičky je jako nezpracovaná JSON a občas víc než jednu událost vyzařovaného najednou. Tento blesku ukazuje, jak číst data, které hubičky a posílat do nového datového proudu jako řazené kolekce členů, která obsahuje více polí.

* **DashboardBolt.java**: Tento příklad ukazuje, jak používat knihovnu Socket.io klienta jazyka Java k odeslat data v reálném čase na řídicí panel web.

Tento příklad používá framework [tok](https://storm.apache.org/releases/0.10.0/flux.html) tak definici topologie jsou obsaženy v souborech YAML. Existují dvě:

* __bez hbase.yaml__ - použít tento soubor při testování topologii ve vývojovém prostředí. Součásti HBase nepoužívá vzhledem k tomu, že nebudete mít přístup k rozhraní API Java HBase z mimo virtuální sítě, který clusteru je umístěn v.

* __s hbase.yaml__ - použít tento soubor při nasazení topologii bouře obrázku. Protože běží ve stejné síti virtuální jako obrázku HBase používá HBase součásti.

## <a name="prepare-your-environment"></a>Vaše prostředí připravili

Před použitím v tomto příkladu je třeba vytvořit událost centrální Azure, která topologii bouře načte z.

### <a name="configure-event-hub"></a>Konfigurace události centrální

Centrální událostí je zdroj dat v tomto příkladu. Pomocí následujících kroků k vytvoření nové události centrální.

1. Z [Azure portál](https://portal.azure.com)vyberte **+ Nový** -> __Internet věci__ -> __Rozbočovače události__.

2. Na zásuvné __Vytvořit Namespace__ proveďte následující úkoly:

    1. Zadejte __název__ pro obor.
    2. Vyberte ceny osy. __Základní__ stačí v tomto příkladu.
    3. Vyberte Azure __předplatného__ použít.
    4. Vyberte existující skupiny zdrojů nebo vytvořte nový účet.
    5. Vyberte __umístění__ pro centrální události.
    6. Vyberte __Připnout do řídicího panelu__a pak klikněte na __vytvořit__.

3. Když s vytvářením dokončí, zobrazí se zásuvné rozbočovače událost pro daný obor názvů. Tady vyberte __+ Centrální přidat událost__. Na zásuvné __Vytvořit událost centrální__ zadejte název __sensordata__ a pak vyberte __vytvořit__. Nechte dalších polí na výchozí hodnoty.

4. Z zásuvné rozbočovače událost pro daný obor názvů vyberte __Rozbočovače události__. Vyberte položku __sensordata__ .

5. V zásuvné pro sensordata centrální události vyberte __sdílené zásady přístupu__. Pomocí odkazu na __+ Přidat__ přidejte následující zásady:


  	| Název zásady | Nároky |
  	| ----- | ----- |
  	| zařízení | Odeslání |
  	| bouře | Poslech |

5. Vyberte obě zásady a poznamenejte si hodnotu __PRIMÁRNÍHO klíče__ . Pro oba zásady v budoucích kroků budete potřebovat hodnotu.

## <a name="download-and-configure-the-project"></a>Stáhněte si a konfigurace projektu

Pomocí následujících stahování GitHub projektu.

    git clone https://github.com/Blackmist/hdinsight-eventhub-example

Po dokončení příkazu máte následující strukturu adresářů:

    hdinsight-eventhub-example/
        TemperatureMonitor/ - this contains the topology
            resources/
                log4j2.xml - set logging to minimal
                no-hbase.yaml - topology definition for local testing
                with-hbase.yaml - topology definition that uses HBase in a virutal network
            src/ - the Java bolts
            dev.properties - contains configuration values for your environment
        dashboard/nodejs/ - this is the node.js web dashboard
        SendEvents/ - utilities to send fake sensor data

> [AZURE.NOTE] Tento dokument nepřekročí úplné podrobnosti kód, který najdete v tomto příkladu; však kód je plně změněna na komentář.

Otevřete soubor **hdinsight-eventhub-example/TemperatureMonitor/dev.properties** a přidejte informace o události centrální následující řádky:

    eventhub.read.policy.name: storm
    eventhub.read.policy.key: KeyForTheStormPolicy
    eventhub.namespace: YourNamespace
    eventhub.name: sensordata

> [AZURE.NOTE] Tento příklad předpokládá používá __bouře__ jako název zásady, která obsahuje __Poslech__ převzít a, které je vaše Centrum události s názvem __sensordata__.

 Uložte soubor po přidání těchto informací.

## <a name="compile-and-test-locally"></a>Kompilace a otestujte místně

Před testováním, je třeba spustit na řídicím panelu zobrazit výstup topologii a generovat data uložená v centrální události.

> [AZURE.IMPORTANT] Při testování místně, jako Java rozhraní API pro clusteru HBase nelze získat přístup z mimo virtuální síť Azure, která obsahuje clusterů, není HBase součást Tato topologie aktivní.

### <a name="start-the-web-application"></a>Spuštění webové aplikace

1. Otevřete nový příkazový řádek nebo terminálu a změňte adresáře **hdinsight-eventhub ukázkovém/řídicím panelu**a pak zadejte následující příkaz a nainstalujte závislosti potřeby tak, že webové aplikace:

        npm install

2. Pomocí následujícího příkazu spuštění webové aplikace:

        node server.js

    Měli byste vidět zpráva podobná této:

        Server listening at port 3000

2. Otevřete webový prohlížeč a zadejte **http://localhost:3000 /** jako adresa. Měli byste vidět podobná této stránky:

    ![řídicí panel Web](./media/hdinsight-storm-sensor-data-analysis/emptydashboard.png)

    Nechte toto příkazový řádek nebo terminál otevřít. Po testování zastavit webového serveru pomocí kláves Ctrl + C.

### <a name="start-generating-data"></a>Zahájení vytváření dat

> [AZURE.NOTE] Postup v této části používá Node.js tak, aby bylo možné na libovolné platformě. Další příklady jazyka najdete v článku adresáři **SendEvents** .

1. Otevřete nový příkazový řádek, kostra domu nebo terminálu a změňte adresáře **hdinsight-eventhub příklad/SendEvents/nodejs**a potom zadejte následující příkaz a nainstalujte závislosti potřeby tak, že aplikace:

        npm install

2. Otevřete soubor **app.js** v textovém editoru a přidejte události centrální informace, které jste získali dříve:

        // ServiceBus Namespace
        var namespace = 'YourNamespace';
        // Event Hub Name
        var hubname ='sensordata';
        // Shared access Policy name and key (from Event Hub configuration)
        var my_key_name = 'devices';
        var my_key = 'YourKey';
    
    > [AZURE.NOTE] V tomto příkladu se předpokládá, že jste použili __sensordata__ jako název události centrální a __zařízení__ jako název zásady, která obsahuje deklaraci __Odeslat__ .

2. Pomocí následujícího příkazu Vložit nové položky v centrální událostí:

        node app.js

    Měli byste vidět několik řádků výstup, které obsahují data odeslaná k rozbočovači události. Tyto bude vypadat takto:

        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"0","Temperature":7}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"1","Temperature":39}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"2","Temperature":86}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"3","Temperature":29}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"4","Temperature":30}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"5","Temperature":5}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"6","Temperature":24}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"7","Temperature":40}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"8","Temperature":43}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"9","Temperature":84}

### <a name="start-the-topology"></a>Zahájení topologii

2. Otevřete nový příkazového řádku, kostra domu nebo terminál a změna adresáře __Hdinsight-eventhub příklad/TemperatureMonitor__a pomocí následujícího příkazu Spustit topologii:

        mvn compile exec:java -Dexec.args="--local -R /no-hbase.yaml --filter dev.properties"
    
    Pokud používáte Powershellu, použijte následující:

        mvn compile exec:java "-Dexec.args=--local -R /no-hbase.yaml --filter dev.properties"

    > [AZURE.NOTE] Pokud jste v systému Linux/Unix/OS X a máte [nainstalovaný bouře ve vývojovém prostředí](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html), můžete použít následující příkazy místo:
    >
    > `mvn compile package`
    > `storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /no-hbase.yaml`

    Spustí se topologie definice v souboru __bez hbase.yaml__ v místním režimu. Hodnoty obsažené v souboru __dev.properties__ zadejte informace o připojení k události rozbočovače. Po spuštění topologii přečte položky z centrální událostí a odešle na řídicí panel spuštěná na místním počítači. Měli byste vidět řádky se zobrazí v řídicím panelu web, podobně jako tento:

    ![řídicí panel s daty](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

3. Použijte spuštěná řídicí panel `node app.js` příkazu z předchozích kroků nová data odešlete rozbočovače události. Protože náhodně vygenerování hodnot teploty grafu by měl aktualizuje a zobrazí velké změny teploty.

    > [AZURE.NOTE] Musí být v adresáři __Hdinsight-eventhub příklad/SendEvents/Nodejs__ při použití `node app.js` příkaz.

3. Po ověření, že to funguje, zastavte topologii pomocí kombinace kláves Ctrl + C. Kombinace kláves Ctrl + C můžete použít taky zastavení místní webový server.

## <a name="create-a-storm-and-hbase-cluster"></a>Vytvoření bouře a HBase obrázku

Chcete-li spustit topologii na HDInsight a povolení blesku HBase, musí vytvořit nové bouře obrázku a HBase obrázku. Postup v této části pomocí [Správce prostředků Azure šablony](../resource-group-template-deploy.md) pro vytvoření nového obrázku Azure virtuální sítě a bouře a HBase virtuální síti se systémem. Šablona také vytvoří webovou aplikaci Azure a nasadí kopii řídicího panelu do ní.

> [AZURE.NOTE] Virtuální sítě se používá, aby topologii výpočetnímu clusteru bouře přímo komunikovat s HBase clusteru pomocí rozhraní API Java HBase.

Správce prostředků Šablona použitá v tomto dokumentu se nachází v kontejneru veřejné objektů blob na __https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hbase-storm-cluster-in-vnet.json__.

1. Klikněte na toto tlačítko přihlášení k Azure a otevření šablony správce na portálu Azure.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-storm-cluster-in-vnet.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Z zásuvné **Parametry** zadejte následující údaje:

    ![HDInsight parametry](./media/hdinsight-storm-sensor-data-analysis/parameters.png)
    
    * **BASECLUSTERNAME**: tuto hodnotu bude sloužit jako základní název bouře a HBase clusterů. Například zadání __hdi__ vytvoří bouře obrázku s názvem __bouře hdi__ a clusteru služby HBase s názvem __hbase hdi__.
    * __CLUSTERLOGINUSERNAME__: uživatelské jméno správce clusterů bouře a HBase.
    * __CLUSTERLOGINPASSWORD__: uživatelská hesla správce clusterů bouře a HBase.
    * __SSHUSERNAME__: SSH uživateli vytvořit clusterů bouře a HBase.
    * __SSHPASSWORD__: heslo uživatele SSH clusterů bouře a HBase.
    * __Umístění__: požadovanou oblast clusterů vytvoří v.
    
    Kliknutím na __OK__ uložte parametry.
    
3. __Pole Skupina zdroje__ část slouží k vytvoření nové skupiny prostředků nebo k výběru již existujícího.

4. V rozevírací nabídce __umístění zdroje skupiny__ vyberte jako vybrané parametru __umístění__ na stejném místě.

5. __Právní termíny__vybrat a pak vyberte __vytvořit__.

6. Nakonec __Připnout do řídicího panelu__ a pak vyberte __vytvořit__. Vytvoření clusterů bude trvat asi o 20 minut.

Po vytvoření zdroje přesměrovaný na zásuvné pro skupina zdroje, který obsahuje clusterů a řídicích panelů web.

![Zásuvné skupina zdroje pro vnet a clusterů](./media/hdinsight-storm-sensor-data-analysis/groupblade.png)

> [AZURE.IMPORTANT] Všimněte si, že jsou jména clusterů HDInsight __Bouře BASENAME__ a __hbase BASENAME__BASENAME je název jste zadali do šablony. Tyto názvy použije v dalších krocích při připojování k clusterů. Navíc nezapomeňte, že je v poli Název webu řídicího panelu __basename řídicího panelu__. Použijete tento později při prohlížení na řídicím panelu.

## <a name="configure-the-dashboard-bolt"></a>Konfigurace blesku řídicího panelu

Mohli poslat data na řídicí panel nasazený jako web app, musí změnit následující řádek v souboru __dev.properties__ :

    dashboard.uri: http://localhost:3000

Změna `http://localhost:3000` k `http://BASENAME-dashboard.azurewebsites.net` a soubor uložte. Nahraďte __BASENAME__ základní název, který jste zadali v předchozím kroku. Můžete taky použít skupina zdroje nevytvořili vyberte řídicího panelu a zobrazení adresu URL.

## <a name="create-the-hbase-table"></a>Vytvoření tabulky HBase

K ukládání dat v HBase, musíte nejdřív vytvoříme tabulky. Chcete obvykle předem vytvořit prostředky, které bouře je potřeba k zápisu, jako pokusu o vytvoření prostředků z uvnitř bouře topologie může vést víc, distribuované kopií kód pokusu o vytvoření stejné zdroje. Vytvoření zdrojů mimo topologii a jenom pro čtení a psaní a technologie pro analýzu použití bouře.

1. Připojení k HBase clusteru pomocí SSH uživatele a heslo, které jste zadali do šablony během vytváření clusteru pomocí SSH. Řekněme, že připojení pomocí `ssh` příkaz, použijete tuto syntaxi:

        ssh USERNAME@hbase-BASENAME-ssh.azurehdinsight.net
    
    V tomto příkazu nahraďte __uživatelské jméno__ SSH uživatelské jméno, které jste zadali při vytváření obrázku a __BASENAME__ s základní název, který jste zadali. Po zobrazení výzvy zadejte heslo pro uživatele SSH.

2. V relaci SSH spusťte HBase prostředí.

        hbase shell
    
    Jakmile prostředí načetl, zobrazí se `hbase(main):001:0>` dotaz.

3. Z prostředí HBase zadejte tento příkaz Vytvořit tabulku pro uložení data snímače:

        create 'SensorData', 'cf'

4. Ověřte, že v tabulce byl vytvořen pomocí následujícího příkazu:

        scan 'SensorData'
        
    To vrátí informaci podobně jako v následujícím příkladu označující, že jsou 0 řádků v tabulce.
    
        ROW                   COLUMN+CELL                                       0 row(s) in 0.1900 seconds

5. Zadejte tento příkaz ukončíte HBase prostředí:

        exit

## <a name="configure-the-hbase-bolt"></a>Konfigurace blesku HBase

Zapisovat do HBase bouře clusteru, je nutné zadat blesku HBase s podrobnostmi konfigurace HBase obrázku. Nejjednodušší způsob je __hbase site.xml__ stahování clusteru a vložit ho do projektu. Několik závislosti v souboru __pom.xml__ , které načíst bouře hbase součásti a požadovaných závislostí musí taky zrušte komentář.

> [AZURE.IMPORTANT] Musí taky stáhnout soubor bouře hbase.jar umístěna na bouře HDInsight clusteru 3.3 nebo 3.4 clusteru; Tato verze kompilaci pro práci s HBase 1.1.x, které se používá pro HBase na HDInsight 3.3 a 3.4 clusterů. Pokud používáte komponentu bouře hbase z jiného místa, může kompilovaný proti starší verzí systému HBase.

### <a name="download-the-hbase-sitexml"></a>Stáhněte si hbase site.xml

Na příkazovém řádku umožňuje spojovací bod služby __hbase site.xml__ budou moct soubor stáhnout z clusteru. V následujícím příkladu nahraďte SSH uživatele, kterého jste použili k vytvoření obrázku a __BASENAME__ s názvem základní dříve zadané __uživatelské jméno__ . Po zobrazení výzvy zadejte heslo pro uživatele SSH. Nahrazení `/path/to/TemperatureMonitor/resources/hbase-site.xml` s cestou k tomuto souboru v aplikaci project TemperatureMonitor.

    scp USERNAME@hbase-BASENAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml /path/to/TemperatureMonitor/resources/hbase-site.xml

To stáhne __hbase site.xml__ do zadané cesty.

### <a name="download-and-install-the-storm-hbase-component"></a>Stáhněte a nainstalujte bouře hbase

1. Na příkazovém řádku umožňuje spojovací bod služby __bouře hbase.jar__ budou moct soubor stáhnout z bouře clusteru. V následujícím příkladu nahraďte SSH uživatele, kterého jste použili k vytvoření obrázku a __BASENAME__ s názvem základní dříve zadané __uživatelské jméno__ . Po zobrazení výzvy zadejte heslo pro uživatele SSH.

        scp USERNAME@storm-BASENAME-ssh.azurehdinsight.net:/usr/hdp/current/storm-client/contrib/storm-hbase/storm-hbase*.jar .

    To stáhne do souboru nazvaného `storm-hbase-####.jar`, kde ### je číslo verze bouře pro tento obrázku. Poznamenejte si tohoto čísla jako pracovní postup slouží později.

2. Pomocí následujícího příkazu nainstalujte ji do místní úložiště Maven na vývojové prostředí. Díky Maven při sestavování projektu najít balíček. Nahrazení __####__ s číslem verze zahrnuté do pole název souboru.

        mvn install:install-file -Dfile=storm-hbase-####.jar -DgroupId=org.apache.storm -DartifactId=storm-hbase -Dversion=#### -Dpackaging=jar
    
    Pokud používáte Powershellu, použijte tento příkaz:

        mvn install:install-file "-Dfile=storm-hbase-####.jar" "-DgroupId=org.apache.storm" "-DartifactId=storm-hbase" "-Dversion=####" "-Dpackaging=jar"

### <a name="enable-the-storm-hbase-component-in-the-project"></a>Povolení bouře hbase součástí projektu

1. Otevřete soubor __TemperatureMonitor/pom.xml__ a odstraňte následující řádky:

        <!-- uncomment this section to enable the hbase-bolt
        end comment for hbase-bolt section -->
    
    > [AZURE.IMPORTANT] Odstranit pouze tyto dva řádky; Neodstraňujte některý z řádků mezi nimi.
    
    Díky několik součástí, které jsou potřebné při komunikaci s HBase pomocí blesku hbase.

2. Následující řádky najít a nahradit __####__ s číslem verze souboru bouře hbase dříve stáhli.

        <dependency>
            <groupId>org.apache.storm</groupId>
            <artifactId>storm-hbase</artifactId>
            <version>####</version>
        </dependency>

    > [AZURE.IMPORTANT] Číslo verze se musí shodovat verzi, kterou jste použili při instalaci komponentu do místní úložiště Maven jako Maven použije tyto informace k načtení komponentu při vytváření projektu.

2. Uložte soubor __pom.xml__ .

## <a name="build-package-and-deploy-the-solution-to-hdinsight"></a>Vytvoření balíčku a nasazení řešení do HDInsight

V vývojové prostředí nasazení topologii bouře clusteru bouře pomocí následujících kroků.

1. V adresáři __TemperatureMonitor__ pomocí následujícího příkazu k provádění nové sestavení a vytvořit balíček SKLENICE z vašeho projektu:

        mvn clean compile package

    Tím vytvoříte do souboru nazvaného **TemperatureMonitor 1.0 SNAPSHOT.jar** v adresáři **cílového** projektu.

2. K nahrání souboru __TemperatureMonitor 1.0 SNAPSHOT.jar__ na svůj cluster bouře použijte spojovací bod služby. V následujícím příkladu nahraďte SSH uživatele, kterého jste použili k vytvoření obrázku a __BASENAME__ s názvem základní dříve zadané __uživatelské jméno__ . Po zobrazení výzvy zadejte heslo pro uživatele SSH.

        scp target\TemperatureMonitor-1.0-SNAPSHOT.jar USERNAME@storm-BASENAME-ssh.azurehdinsight.net:TemperatureMonitor-1.0-SNAPSHOT.jar
    
    > [AZURE.NOTE] Jak to bude několik megabajtů velikosti ho může trvat několik minut nahrát soubor.

    Umožňuje spojovací bod služby nahrát soubor __dev.properties__ , jak je to s informacemi používá pro připojení k události rozbočovače a na řídicím panelu.

        scp dev.properties USERNAME@storm-BASENAME-ssh.azurehdinsight.net:dev.properties

3. Po nahrání souborů připojte k obrázku pomocí SSH.

        ssh USERNAME@storm-BASENAME-ssh.azurehdinsight.net

4. Z SSH relace pomocí následujícího příkazu zahájíte topologii.

        storm jar TemperatureMonitor-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /with-hbase.yaml --filter dev.properties
    
    Spustí se topologii pomocí topologie definice v souboru __s hbase.yaml__ a konfigurace hodnoty v souboru __dev.properties__ .

3. Po spuštění topologii otevřete prohlížeč na webu publikování v Azure, pak můžete `node app.js` příkaz Odeslat data do centrální události. Měli byste vidět řídicím panelu web aktualizovat k zobrazení informací.

    ![řídicí panel](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

## <a name="view-hbase-data"></a>Zobrazení HBase dat

Poté, co jste odeslali dat pomocí topologie `node app.js`, pomocí následujících kroků pro připojení k HBase a ověřte, že data má byla došlo k zápisu tabulce jste dříve vytvořili.

1. Umožňuje připojit k obrázku HBase SSH.

        ssh USERNAME@hbase-BASENAME-ssh.azurehdinsight.net

2. V relaci SSH spusťte HBase prostředí.

        hbase shell
    
    Jakmile prostředí načetl, zobrazí se `hbase(main):001:0>` dotaz.

2. Zobrazení řádků z tabulky:

        scan 'SensorData'
        
    To, měly vrátit informace podobná, označující, že jsou 0 řádků v tabulce.
    
        hbase(main):002:0> scan 'SensorData'
        ROW                             COLUMN+CELL
        \x00\x00\x00\x00               column=cf:temperature, timestamp=1467290788277, value=\x00\x00\x00\x04
        \x00\x00\x00\x00               column=cf:timestamp, timestamp=1467290788277, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x01               column=cf:temperature, timestamp=1467290788348, value=\x00\x00\x00M
        \x00\x00\x00\x01               column=cf:timestamp, timestamp=1467290788348, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x02               column=cf:temperature, timestamp=1467290788268, value=\x00\x00\x00R
        \x00\x00\x00\x02               column=cf:timestamp, timestamp=1467290788268, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x03               column=cf:temperature, timestamp=1467290788269, value=\x00\x00\x00#
        \x00\x00\x00\x03               column=cf:timestamp, timestamp=1467290788269, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x04               column=cf:temperature, timestamp=1467290788356, value=\x00\x00\x00>
        \x00\x00\x00\x04               column=cf:timestamp, timestamp=1467290788356, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x05               column=cf:temperature, timestamp=1467290788326, value=\x00\x00\x00\x0D
        \x00\x00\x00\x05               column=cf:timestamp, timestamp=1467290788326, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x06               column=cf:temperature, timestamp=1467290788253, value=\x00\x00\x009
        \x00\x00\x00\x06               column=cf:timestamp, timestamp=1467290788253, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x07               column=cf:temperature, timestamp=1467290788229, value=\x00\x00\x00\x12
        \x00\x00\x00\x07               column=cf:timestamp, timestamp=1467290788229, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x08               column=cf:temperature, timestamp=1467290788336, value=\x00\x00\x00\x16
        \x00\x00\x00\x08               column=cf:timestamp, timestamp=1467290788336, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x09               column=cf:temperature, timestamp=1467290788246, value=\x00\x00\x001
        \x00\x00\x00\x09               column=cf:timestamp, timestamp=1467290788246, value=2015-02-10T14:43.05.00320Z
        10 row(s) in 0.1800 seconds

    > [AZURE.NOTE] Tento operace vyhledávání vrátí pouze maximálně 10 řádků z tabulky.

## <a name="delete-your-clusters"></a>Odstranění clusterů

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Odstranit clusterů, ukládání a v prohlížeči současně, odstranit zdroje, který je obsahuje.

## <a name="next-steps"></a>Další kroky

Jste se naučili teď použití bouře k načtení dat z rozbočovače události, uložte ho do HBase a vizualizovat informace na externí řídicího panelu pomocí Socket.io a D3.js.

* Další příklady bouře topologií s Hdinsightu najdete tady:

    * [Příklad topologie pro bouře na HDInsight](hdinsight-storm-example-topology.md)

* Další informace o Apache bouře najdete v článku [Apache bouře](https://storm.incubator.apache.org/) webu.

* Další informace o HBase na Hdinsightu najdete v tématu [HBase HDInsight přehled](hdinsight-hbase-overview.md).

* Další informace o Socket.io najdete v článku [socket.io](http://socket.io/) webu.

* Další informace o D3.js najdete v článku [D3.js - dat řízený úsilím dokumenty](http://d3js.org/).

* Informace o vytváření topologií v Java najdete v tématu [vyvíjet Java topologie pro Apache bouře na HDInsight](hdinsight-storm-develop-java-topology.md).

* Informace o vytváření topologií v .NET najdete v tématu [topologií vyvíjet C# pro Apache bouře na HDInsight pomocí aplikace Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

[azure-portal]: https://portal.azure.com
