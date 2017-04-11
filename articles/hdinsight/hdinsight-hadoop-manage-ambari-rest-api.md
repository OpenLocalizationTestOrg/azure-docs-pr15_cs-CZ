<properties
   pageTitle="Sledování a Správa clusterů HDInsight pomocí rozhraní REST API Ambari Apache | Microsoft Azure"
   description="Naučte se používat Ambari ke sledování a Správa clusterů na základě Linux HDInsight. V tomto dokumentu se dozvíte, jak používat rozhraní REST API Ambari zahrnutý v sadě HDInsight clusterů."
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
   ms.date="09/20/2016"
   ms.author="larryfr"/>

#<a name="manage-hdinsight-clusters-by-using-the-ambari-rest-api"></a>Správa clusterů HDInsight pomocí rozhraní REST API Ambari

[AZURE.INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Apache Ambari usnadňuje správu a sledování clusteru Hadoop zadáním snadno použít pro web uživatelského rozhraní a rozhraní REST API. Ambari je k dispozici na základě Linux HDInsight clusterů a slouží ke sledování clusteru a provést změny konfigurace. V tomto dokumentu Naučte se základy práce pomocí rozhraní REST API Ambari tak, že provádění běžných úloh pomocí otočení.

##<a name="prerequisites"></a>Zjistit předpoklady pro

* [otáčení](http://curl.haxx.se/): otočení je nástroj různé platformy, který slouží k práci s rozhraní REST API z příkazového řádku. V tomto dokumentu se používá ke komunikaci s rozhraní REST API Ambari.
* [jq](https://stedolan.github.io/jq/): jq je nástroj různé platformy příkazového řádku pro práci s dokumenty JSON. V tomto dokumentu je použit k analýze vrácených z rozhraní REST API Ambari dokumentů JSON.
* [Azure rozhraní příkazového řádku](../xplat-cli-install.md): nástroj různé platformy příkazového řádku pro práci se službou Azure.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 

##<a id="whatis"></a>Co je Ambari?

[Apache Ambari](http://ambari.apache.org) usnadňuje správu Hadoop zadáním web snadno použitelné uživatelské rozhraní, které slouží ke zřízení, Správa a sledování Hadoop clusterů. Vývojáři můžete integrovat tyto funkce do svých aplikací pomocí rozhraní [REST API Ambari](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

Ambari není uvedený ve výchozím nastavení se clusterů na základě Linux HDInsight.

##<a name="rest-api"></a>ROZHRANÍ REST API

Základní URI pro rozhraní REST API Ambari na HDInsight je https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME, kde __NÁZEV_CLUSTERU__ clusteru.

> [AZURE.IMPORTANT] Při velká a malá písmena názvu obrázku v části úplný název (FQDN) identifikátor URI (CLUSTERNAME.azurehdinsight.net) další události do identifikátor URI rozlišují velká. Pokud svůj cluster jmenuje clusteru, následující příklady platné identifikátory URI:
>
> `https://mycluster.azurehdinsight.net/api/v1/clusters/MyCluster`
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/MyCluster`
>
> Následující URI vrátí chybu, protože druhý výskyt název neplatí pro správné.
>
> `https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster`
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/mycluster`

Připojení k Ambari na HDInsight vyžaduje HTTPS. Při ověřování připojení, musíte použít název účtu správce (výchozí hodnota je __Správce__) a heslo, které jste zadali při vytvoření clusteru.

Otočení aby požadavek GET proti rozhraní REST API v následujícím příkladu. Nahraďte __heslo__ heslo správce pro clusteru. Nahraďte název clusteru __NÁZEV_CLUSTERU__ :

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME"
    
Odpověď na je JSON dokument, který začíná informacemi podobná této:

    {
    "href" : "http://10.0.0.10:8080/api/v1/clusters/CLUSTERNAME",
    "Clusters" : {
        "cluster_id" : 2,
        "cluster_name" : "CLUSTERNAME",
        "health_report" : {
        "Host/stale_config" : 0,
        "Host/maintenance_state" : 0,
        "Host/host_state/HEALTHY" : 7,
        "Host/host_state/UNHEALTHY" : 0,
        "Host/host_state/HEARTBEAT_LOST" : 0,
        "Host/host_state/INIT" : 0,
        "Host/host_status/HEALTHY" : 7,
        "Host/host_status/UNHEALTHY" : 0,
        "Host/host_status/UNKNOWN" : 0,
        "Host/host_status/ALERT" : 0

Protože je to JSON, je snadněji pracovat s daty pomocí analyzátoru formátu JSON. Například v následujícím příkladu jq zobrazíte jenom `health_report` prvek.

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME" | jq '.Clusters.health_report'

##<a name="example-get-the-fqdn-of-cluster-nodes"></a>Příklad: Získání plně kvalifikovaný název domény uzlech

Když pracujete s HDInsight, je potřeba vědět plně kvalifikovaný název domény (FQDN) uzlu. Můžete snadno získat plně kvalifikovaný název domény pro různé uzlů v clusteru následujícími způsoby:

* __Hlavy uzly__:`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'`
* __Pracovní uzly__:`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/DATANODE" | jq '.host_components[].HostRoles.host_name'`
* __Uzly zookeeper__:`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" | jq '.host_components[].HostRoles.host_name'`

Poznámka: každý z těchto příkladů postupujte stejným:

1. Spuštění dotazu komponentu víme na tyto uzly.
2. Načtení `host_name` prvky, které obsahují plně kvalifikovaný název domény pro tyto uzly.

`host_components` Prvek zpáteční dokument obsahuje více položek. Použití `.host_components[]`, a zadáním cesty v rámci elementu bude opakovaně prochází všechny požadované položky a vysunout hodnotu z konkrétní cestu. Pokud chcete pouze jednu hodnotu, jako je první položku plně kvalifikovaný název domény, můžete vrátit položky jako kolekce a vyberte konkrétní položky:

    jq '[.host_components[].HostRoles.host_name][0]'

To z kolekce vrátí první plně kvalifikovaný název domény.

##<a name="example-get-the-default-storage-account-and-container"></a>Příklad: Získání výchozí úložiště klienta a kontejneru

Když vytvoříte HDInsight clusteru, je nutné použít účet Azure úložiště a kontejneru objektů blob jako výchozí úložiště clusteru. Ambari slouží k načtení tyto informace po vytvoření clusteru. Například pokud chcete programově zápis dat přímo do kontejneru.

Identifikátor URI WASB výchozí úložiště clusterů načte:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
    
> [AZURE.NOTE] Tato akce vrátí první konfigurace použitý k serveru (`service_config_version=1`,) obsahující tyto informace. Pokud hodnota, která byla změněna po vytvoření obrázku, budete muset obsahoval konfigurace verze a načíst nejnovější.

Tento příkaz vrátí hodnotu podobně jako v následujícím příkladu, kde je __kontejner__ kontejneru výchozí a __název účtu__ je název účtu úložiště Azure:

    wasbs://CONTAINER@ACCOUNTNAME.blob.core.windows.net

Pak můžete tyto informace se [Rozhraní příkazového řádku Azure](../xplat-cli-install.md) nahrávání nebo stahování dat z kontejneru.

1. Získejte skupina zdroje pro účet úložiště. Nahraďte název účtu úložiště získána Ambari __název účtu__ :

        azure storage account list --json | jq '.[] | select(.name=="ACCOUNTNAME").resourceGroup'
    
    Tento příkaz vrátí název skupiny prostředků pro účet.
    
    > [AZURE.NOTE] Pokud nic vrácená tento příkaz, budete muset změnit Azure rozhraní příkazového řádku do režimu správce prostředků Azure a znovu spusťte příkaz. Přepněte do režimu správce prostředků Azure, použijte tento příkaz:
    >
    > `azure config mode arm`
    
2. Pokud potřebujete klíč účtu úložiště. Nahraďte __název skupiny__ skupina zdroje v předchozím kroku. Nahraďte název účtu úložiště __název účtu__ :

        azure storage account keys list -g GROUPNAME ACCOUNTNAME --json | jq '.storageAccountKeys.key1'

    V tomto příkladu vrací primární klíč účtu.
    
3. Pomocí příkazu Odeslat pro uložení souboru v kontejneru:

        azure storage blob upload -a ACCOUNTNAME -k ACCOUNTKEY -f FILEPATH --container __CONTAINER__ -b BLOBPATH
        
    Nahraďte __název účtu__ název účtu úložiště. Nahraďte __ACCOUNTKEY__ klávesu dříve načíst. __Cesta k souboru__ je cesta k souboru, který chcete nahrát, zatímco __BLOBPATH__ je cesta v kontejneru.

    Například pokud chcete soubor se zobrazují v HDInsight wasbs://example/data/filename.txt, pak __BLOBPATH__ by `example/data/filename.txt`.

##<a name="example-update-ambari-configuration"></a>Příklad: Konfigurace aktualizace Ambari

1. Získání aktuální konfigurace, který ukládá Ambari jako "požadovaná konfigurace":

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME?fields=Clusters/desired_configs"
        
    V tomto příkladu vrací JSON dokument obsahující aktuální konfigurace (označených _příznak_ hodnota) pro součásti nainstalovaným clusteru. V následujícím příkladu je úryvek z dat vrácených typu Spark obrázku.
    
        "spark-metrics-properties" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        },
        "spark-thrift-fairscheduler" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        },
        "spark-thrift-sparkconf" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        }

    V tomto seznamu je nutné zkopírovat názvu součásti (například __spark\_thrift\_sparkconf__ a hodnoty __značky__ .
    
2. Pomocí následujícího příkazu načtení konfigurace pro součást a značky. Nahraďte __spark thrift sparkconf__ a __Počáteční__ součásti a značky, která se mají načíst konfiguraci.

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations?type=spark-thrift-sparkconf&tag=INITIAL" | jq --arg newtag $(echo version$(date +%s%N)) '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
    
    Otočení načte JSON dokumentu a potom jq slouží k provádění změn dat k vytvoření šablony. Šablony je pak použít přidat/upravit hodnoty konfigurace. Konkrétně dělá toto:
    
    * Vytvoří jedinečnou hodnotu obsahující řetězec "verze" a data, která je uložena v __newtag__.
    * Vytvoří v kořenovém dokumentu nové požadovaná konfigurace.
    * Získá obsah `.items[]` pole a přidá ji v prvku __desired_config__ .
    * Slouží k odstranění __href__, __verze__a __Konfigurace__ prvků odešlete novou konfiguraci nejsou potřeby tyto prvky.
    * Přidá nový prvek __značky__ a nastaví jeho hodnotu na __verzi ###__. Číselná část je založena na aktuální datum. Jednotlivé konfigurace musí mít jedinečný značku.
    
    Nakonec data budou uložena v dokumentu __newconfig.json__ . Strukturu dokumentu by měla vypadat podobně jako v následujícím příkladu:
    
        {
            "Clusters": {
                "desired_config": {
                "tag": "version1459260185774265400",
                "type": "spark-thrift-sparkconf",
                "properties": {
                    ....
                 },
                 "properties_attributes": {
                     ....
                 }
            }
        }

3. Otevřete hodnoty __newconfig.json__ dokument a upravit nebo přidat do __Vlastnosti__ objektu. Následující příklad změní hodnotu __"spark.yarn.am.memory"__ od __"1 g"__ na __"3 g"__ a a přidá nový prvek pro __"spark.kryoserializer.buffer.max"__ s hodnotou __"256 m"__.

        "spark.yarn.am.memory": "3g",
        "spark.kyroserializer.buffer.max": "256m",

    Jakmile dokončíte všechny změny, uložte soubor.

4. Pomocí následujícího příkazu odeslat aktualizované konfigurace Ambari.

        cat newconfig.json | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME"
        
    Tento příkaz potrubí obsah souboru __newconfig.json__ k pozvánce na schůzku otočení, která odešle ji obrázku jako nový požadovaná konfigurace. Žádost o otočení vrátí JSON dokumentu. Element __versionTag__ v tomto dokumentu měly odpovídat verzi, kterou jste odeslali a na objekt __configs__ bude obsahovat požadovaných změn konfigurace.

###<a name="example-restart-a-service-component"></a>Příklad: Restartujte součásti služby

V tomto okamžiku když se podíváte na web Ambari uživatelského rozhraní, službu Spark výskyt znamená, že ho se musí restartovat se projeví nové konfigurace můžete. Pomocí následujících kroků restartujte službu.

1. Povolit režim údržby pro službu Spark použijte následující:

        echo '{"RequestInfo": {"context": "turning on maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

    Tento příkaz odešle JSON dokument na serveru (obsažené v `echo` údajů,) které Zapne režim údržby.
    Můžete ověřit, že služba je teď v režimu údržby pomocí následující požadavek:
    
        curl -u admin:PASSWORD -H "X-Requested-By: ambari" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK" | jq .ServiceInfo.maintenance_state
        
    To bude vracet hodnotu `"ON"`.

3. Pak službu vypnout pomocí následující:

        echo '{"RequestInfo": {"context" :"Stopping the Spark service"}, "Body": {"ServiceInfo": {"state": "INSTALLED"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"
        
    Tento příkaz vrátí podobně jako tento odpověď.
    
        {
            "href" : "http://10.0.0.18:8080/api/v1/clusters/CLUSTERNAME/requests/29",
            "Requests" : {
                "id" : 29,
                "status" : "Accepted"
            }
        }
    
    `href` Hodnota této URI používá interní IP adresu uzel clusteru. Můžete ho z mimo clusteru, nahraďte znamenající "10.0.0.18:8080" plně kvalifikovaný název domény clusteru. Následující příkaz například načte stav žádosti.
    
        curl -u admin:PASSWORD -H "X-Requested-By: ambari" "https://CLUSTERNAME/api/v1/clusters/CLUSTERNAME/requests/29" | jq .Requests.request_status
    
    Pokud tento příkaz vrátí hodnotu `"COMPLETED"` pak žádost proběhla.

4. Po dokončení předchozího žádosti umožňuje následující spustit službu.

        echo '{"RequestInfo": {"context" :"Restarting the Spark service"}, "Body": {"ServiceInfo": {"state": "STARTED"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

    Po restartování služby je nové nastavení.

5. Nakonec pomocí následující vypněte režim údržbu.

        echo '{"RequestInfo": {"context": "turning off maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

##<a name="next-steps"></a>Další kroky

Kompletní odkaz na rozhraní REST API najdete v článku [Ambari rozhraní API odkaz V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

> [AZURE.NOTE] Některé funkce Ambari zakázané, jak se spravuje cloudovou službu HDInsight; například přidáte nebo odeberete hosts clusteru nebo přidání nové služby.
