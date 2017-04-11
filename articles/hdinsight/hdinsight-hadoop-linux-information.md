<properties
   pageTitle="Tipy pro použití Hadoop na základě Linux HDInsight | Microsoft Azure"
   description="Získejte tipy, implementaci pro použití na základě Linux HDInsight (Hadoop) clusterů ve známém prostředí Linux spuštěné v Azure cloudu."
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
   ms.date="09/13/2016"
   ms.author="larryfr"/>

# <a name="information-about-using-hdinsight-on-linux"></a>Informace o používání HDInsight na Linux

Na základě Linux clusterů Azure HDInsight zadejte Hadoop ve známém prostředí Linux spuštěné v Azure cloudu. Pro většinu věcí, které měli spolupracovat stejným způsobem jako jakékoli jiné instalace Hadoop na Linux. Tento dokument hovorů určité rozdíly, které byste měli znát.

##<a name="prerequisites"></a>Zjistit předpoklady pro

Řadu kroků uvedených v tomto dokumentu použijte následující nástroje, které může být nutné mít nainstalovaný v počítači.

* [otáčení](https://curl.haxx.se/) – slouží k komunikovat s webové služby
* [jq](https://stedolan.github.io/jq/) – slouží k analýze JSON dokumentů
* [Azure rozhraní příkazového řádku](../xplat-cli-install.md) - sloužící ke správě vzdálené služby Azure

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)] 

## <a name="domain-names"></a>Názvy domén

Plně kvalifikovaný název domény (FQDN) pro použití při připojení ke clusteru z Internetu je ** &lt;Název_clusteru >. azurehdinsight.net** nebo (pouze SSH) ** &lt;název_clusteru-ssh >. azurehdinsight.net**.

Interně jednotlivých uzlech clusteru má název, který je během konfigurace clusteru přiřazeno. Zobrazíte jména obrázku můžete navštivte stránku __Hosts__ na uživatelské rozhraní webu Ambari nebo k vrácení seznam hosts z rozhraní REST API Ambari můžete použít následující:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts" | jq '.items[].Hosts.host_name'

Nahraďte __heslo__ heslem správce klienta a __NÁZEV_CLUSTERU__ s názvem svůj cluster. Tento postup vrátí JSON dokument, který obsahuje seznam hostitelů clusteru a potom jq použije `host_name` hodnotu prvku hostitelé clusteru.

Pokud potřebujete zjistit název uzel pro konkrétní službu, můžete dotaz Ambari pro tuto složku. Například najít hostitele uzel název HDFS, použijte následující.

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'

Tato akce vrátí dokumentu JSON popisující službu a potom jq použije jenom `host_name` hodnotu pro hostitele.

## <a name="remote-access-to-services"></a>Vzdálený přístup ke službám

* **Ambari (web)** - https://&lt;Název_clusteru >. azurehdinsight.net

    Ověření pomocí obrázku správce uživatele a heslo a pak se přihlaste k Ambari. Používá také clusteru správce uživatele a heslo.

    Ověřování je ve formátu prostého textu: vždy HTTPS umožňuje zajistit, že je zabezpečené připojení.

    > [AZURE.IMPORTANT] Při Ambari pro svůj cluster přístupných osobám s postižením přímo přes Internet, některé funkce závisí na přístupu uzly název interní domény používaný clusteru. Protože to je název interní domény a ne veřejné "server nebyl nalezen" chybové zprávy při pokusu o přístup k některé funkce přes Internet.
    >
    > Kompletním funkcím webu Ambari uživatelského rozhraní, použijte SSH tunelem na proxy serveru webové umožnění datových přenosů do hlavního uzlu obrázku. V tématu [Použití SSH Tunneling přístup k webu Ambari uživatelského rozhraní, ResourceManager, JobHistory, NameNode, Oozie a jiným webovým uživatelské rozhraní na](hdinsight-linux-ambari-ssh-tunnel.md)

* **Ambari (REST)** - https://&lt;Název_clusteru >.azurehdinsight.net/ambari

    > [AZURE.NOTE] Ověření pomocí obrázku správce uživatele a hesla.
    >
    > Ověřování je ve formátu prostého textu: vždy HTTPS umožňuje zajistit, že je zabezpečené připojení.

* **WebHCat (Templeton)** - https://&lt;Název_clusteru >.azurehdinsight.net/templeton

    > [AZURE.NOTE] Ověření pomocí obrázku správce uživatele a hesla.
    >
    > Ověřování je ve formátu prostého textu: vždy HTTPS umožňuje zajistit, že je zabezpečené připojení.

* **SSH** - &lt;Název_clusteru >-ssh.azurehdinsight.net na portu 22 nebo 23. Port 22 se používá pro připojení k primární headnode během 23 se používá pro připojení k sekundární. Další informace o hlavy uzlech najdete v článku [dostupnost a spolehlivost Hadoop clusterů HDInsight](hdinsight-high-availability-linux.md).

    > [AZURE.NOTE] Hlavy uzlů můžete přistupovat pouze prostřednictvím SSH z klientského počítače. Po připojení, pak se dostanete uzly pracovníka pomocí SSH od headnode.

## <a name="file-locations"></a>Umístění souboru

Soubory Hadoop týkající se nachází na uzlů v `/usr/hdp`. Tento adresář obsahuje následující podadresáře:

* __2.2.4.9-1__: Tento adresář jmenuje verze platformy dat Hortonworks používá HDInsight, aby čísla na svůj cluster se liší od tu uvedený tady.
* __aktuální__: Tento adresář obsahuje odkazy na adresáře v adresáři __2.2.4.9-1__ a existuje tak, že nemusíte zadejte číslo verze (může změnit adresa,) pokaždé, když chcete získat přístup k souboru.

Ukázková data a SKLENICE soubory se nachází na Hadoop Distributed soubor systému (HDFS) nebo objektů Blob Azure úložiště na "/ Příklad" nebo "wasbs: / / / Příklad".

## <a name="hdfs-azure-blob-storage-and-storage-best-practices"></a>HDFS, úložiště objektů Blob Azure a osvědčené postupy paměťových

Ve většině Hadoop distribuce HDFS získáváte místní úložiště na počítačích clusteru. Když to je efektivně, může být nákladně cloudové řešení kde bude účtovaná hodinové nebo minuty pro využití prostředků.

Úložiště objektů Blob Azure HDInsight používá jako výchozí úložiště, který poskytuje následující výhody:

* Levný dlouhodobé úložiště

* Přístupnost z externích služeb, jako je weby, nástroje nahrávání nebo stahování souboru, různých SDK jazyk a webových prohlížečích

Takové alternativní řešení je výchozí úložiště HDInsight, obvykle nemusíte udělat nic, aby se používala. Následující příkaz například zobrazí seznam souborů ve složce **/example/data** , který je uložen v úložišti objektů Blob Azure:

    hdfs dfs -ls /example/data

Některé příkazy, může být nutné zadat, že používáte úložiště objektů Blob. U těchto, můžete před příkazu s **wasb: / /**, nebo **wasbs: / /**.

HDInsight umožňuje více účtů úložiště objektů Blob přidružit clusteru. Přístup k datům na účtu úložiště objektů Blob jiné než výchozí, můžete použít formát **wasbs: / /&lt;container-name>@&lt;název účtu >.blob.core.windows.net/**. Například následující vypíše obsah adresáře **/example/data** pro zadaný kontejner a účtu úložiště objektů Blob:

    hdfs dfs -ls wasbs://mycontainer@mystorage.blob.core.windows.net/example/data

### <a name="what-blob-storage-is-the-cluster-using"></a>K čemu úložiště objektů Blob clusteru používá?

Při vytváření clusteru jste vybrali, použít existující účet Azure úložiště a kontejneru, nebo vytvořte nový účet. Potom pravděpodobně zapomněli jste k němu. Výchozí úložiště klienta a kontejneru můžete najít pomocí rozhraní REST API Ambari.

1. Získání informací o konfiguraci HDFS pomocí otočení pomocí následujícího příkazu a filtrovat pomocí [jq](https://stedolan.github.io/jq/):

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
    
    > [AZURE.NOTE] Tento postup vrátí první konfigurace použitý k serveru (`service_config_version=1`,) obsahující tyto informace. Pokud dostáváte hodnota, která byla změněna po vytvoření obrázku, budete muset obsahoval konfigurace verze a načíst nejnovější.

    To vrátí hodnotu podobná, kde je __kontejner__ kontejneru výchozí a __název účtu__ je název účtu úložiště Azure:

        wasbs://CONTAINER@ACCOUNTNAME.blob.core.windows.net

1. Získání skupina zdroje pro účet úložiště, použijte [Azure rozhraní příkazového řádku](../xplat-cli-install.md). V následující příkaz nahraďte název účtu úložiště načtená z Ambari __název účtu__ :

        azure storage account list --json | jq '.[] | select(.name=="ACCOUNTNAME").resourceGroup'
    
    Vrátí název skupiny prostředků pro účet.
    
    > [AZURE.NOTE] Pokud nic vrácená tento příkaz, budete muset změnit Azure rozhraní příkazového řádku do režimu správce prostředků Azure a znovu spusťte příkaz. Přepněte do režimu správce prostředků Azure, použijte následující příkaz.
    >
    > `azure config mode arm`
    
2. Pokud potřebujete klíč účtu úložiště. Nahraďte __název skupiny__ skupina zdroje v předchozím kroku. Nahraďte název účtu úložiště __název účtu__ :

        azure storage account keys list -g GROUPNAME ACCOUNTNAME --json | jq '.[0].value'

    Vrátí primárního klíče pro účet.

Taky můžete najít informace úložiště na portálu Azure:

1. Na [Portálu Azure](https://portal.azure.com/)vyberte svůj cluster HDInsight.

2. V části __Základy__ vyberte __všechna nastavení__.

3. V __Nastavení__vyberte __Azure úložiště klíče__.

4. Z __Azure úložiště klíčů__, vyberte jednu z úložiště účty uvedené. Zobrazí se informace o účtu úložiště.

5. Vyberte ikonu klíče. Zobrazí se klávesové zkratky pro tento účet úložiště.

### <a name="how-do-i-access-blob-storage"></a>Jak se dostanu úložiště objektů Blob?

Jiné než pomocí příkazu Hadoop z clusteru existuje celá řada způsobů, jak získat přístup k objektů blob:

* [Azure rozhraní příkazového řádku pro Mac a Linux Windows](../xplat-cli-install.md): rozhraní příkazového řádku příkazy pro práci s Azure. Po instalaci používat `azure storage` příkazu nápovědu k používání úložiště, nebo `azure blob` objektů blob specifické příkazů.

* [blobxfer.PY](https://github.com/Azure/azure-batch-samples/tree/master/Python/Storage): python skriptu pro práci s objekty BLOB v úložišti Azure.

* Různé sady SDK:

    * [Java](https://github.com/Azure/azure-sdk-for-java)

    * [Node.js](https://github.com/Azure/azure-sdk-for-node)

    * [PHP](https://github.com/Azure/azure-sdk-for-php)

    * [Python](https://github.com/Azure/azure-sdk-for-python)

    * [Skutečné](https://github.com/Azure/azure-sdk-for-ruby)

    * [.NET](https://github.com/Azure/azure-sdk-for-net)

* [Úložiště REST API](https://msdn.microsoft.com/library/azure/dd135733.aspx)

##<a name="scaling"></a>Změna měřítka svůj cluster

Funkci změny velikosti obrázku můžete změnit počet datových uzlů použít clusteru, na kterém běží v Azure HDInsight aniž byste museli odstraňte a znovu vytvořte clusteru.

Můžete dělat změny měřítka operace při jiných projektů nebo procesy jsou výpočetnímu clusteru.

Různých typů se to týká roztažením následujícím způsobem:

* __Hadoop__: Po zmenšení počtu uzlů v clusteru, některé z těchto služeb v clusteru restartovat. Může dojít k úlohy spuštění nebo čeká na vyřízení selhání po dokončení operace změny měřítka. Po dokončení operace můžete znovu odešlete úlohy.

* __HBase__: místní servery jsou automaticky rovnováha objevit během pár minut po dokončení operace změny měřítka. Ruční vyrovnání místní servery, použijte následující kroky:

    1. Připojení k clusteru HDInsight pomocí SSH. Další informace o použití SSH s Hdinsightu najdete tyto dokumenty:

        * [Použití SSH s HDInsight z Linux Unix a Mac OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

        * [Použití SSH s HDInsight z Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

    1. Spuštění prostředí HBase pomocí následujících akcí:

            hbase shell

    2. Jakmile prostředí HBase načtená, použijte následující ručně vyrovnání místní servery:

            balancer

* __Bouře__: všechny spuštěné topologií bouře by měl vyrovnání po provedení operace změny měřítka. Díky topologii změnit nastavení paralelismus podle nového počtu uzlů v clusteru. Vyrovnání pracovního topologií, můžete jedním z následujících možností:

    * __SSH__: připojení k serveru a umožňuje vyrovnání topologii tento příkaz:

            storm rebalance TOPOLOGYNAME

        Můžete taky určit parametry přepsat pokyny paralelismus původně poskytovanou topologii. Například `storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10` bude překonfigurovat topologii 5 pracovní procesy, 3 vykonavatelů součásti modrá hubičky a 10 vykonavatelů součásti žlutá blesku.

    * __Uživatelské rozhraní bouře__: vyrovnání topologie pomocí rozhraní bouře pomocí následujících kroků.

        1. Otevřete __https://CLUSTERNAME.azurehdinsight.net/stormui__ ve webovém prohlížeči, kde NÁZEV_CLUSTERU je název bouře obrázku. Pokud se zobrazí výzva, zadejte HDInsight obrázku (Správci) jména a hesla správce, které jste zadali při vytváření clusteru.

        3. Vyberte topologii, kterou chcete vyrovnání a potom klikněte na tlačítko __vyrovnání__ . Zadejte zpoždění před je provedena operace rebalance.

Konkrétní informace o měřítko obrázku Hdinsightu najdete tady:

* [Správa Hadoop clusterů HDInsight pomocí portálu Azure](hdinsight-administer-use-portal-linux.md#scaling)

* [Správa Hadoop clusterů HDinsight pomocí prostředí PowerShell Azure](hdinsight-administer-use-command-line.md#scaling)

## <a name="how-do-i-install-hue-or-other-hadoop-component"></a>Jak můžu nainstalovat odstín (nebo jiné součásti Hadoop)?

HDInsight je služba spravovaných, což znamená, že uzlů v clusteru může odstraněna a reprovisioned automaticky tak, že Azure Pokud byl zjištěn problém. Proto se nedoporučuje ruční instalace věci přímo na uzlů. Místo toho provádějte [Akce skriptu HDInsight](hdinsight-hadoop-customize-cluster.md) budete potřebovat k instalaci takto:

* Služba nebo webu Spark ATP odstín.
* Součásti, která vyžaduje změny konfigurace více uzlů v clusteru. Například proměnnou požadované prostředí vytváření protokolování adresáři nebo vytvoření konfiguračního souboru.

Skript akce jsou flám skripty, které jsou spustili během vytváření obrázku a mohou sloužit k instalace a konfigurace dodatečný na clusteru. Příklad skripty jsou k dispozici pro instalaci tyto prvky:

* [Odstín](hdinsight-hadoop-hue-linux.md)
* [Giraph](hdinsight-hadoop-giraph-install-linux.md)
* [Solr](hdinsight-hadoop-solr-install-linux.md)

Informace o vývoji vlastní skript akce, najdete v článku [akci skriptu vývoj s HDInsight](hdinsight-hadoop-script-actions-linux.md).

###<a name="jar-files"></a>Sklenice souborů

Některé Hadoop technologie jsou k dispozici v samostatné sklenice souborů obsahujících funkcí sloužící jako součást MapReduce úlohy a od uvnitř Prasátko nebo podregistru. Když to je možné nainstalovat pomocí skriptu akce, jsou často nevyžadují jakékoliv nastavení a lze jenom nahráli clusteru po zajištění služby a použít přímo. Pokud budete chtít makle, že komponentu odolává reimaging clusteru, můžete sklenice soubor uložíte ve WASB.

Například pokud chcete použít nejnovější verzi [DataFu](http://datafu.incubator.apache.org/), můžete stáhnout sklenice obsahující projektu a ho nahrajte do obrázku HDInsight. Pak postupujte podle DataFu dokumentace o tom, jak používat ze Prasátko nebo podregistru.

> [AZURE.IMPORTANT] Některé součásti, které jsou samostatné sklenice soubory k dispozici HDInsight, ale nejsou ve cestu. Pokud hledáte konkrétní součásti, můžete ho vyhledat na svůj cluster následujícím:
>
> ```find / -name *componentname*.jar 2>/dev/null```
>
> Vrátí cestu sklenice soubory.

Pokud clusteru již obsahuje verzi součásti jako samostatný soubor sklenice, ale chcete používat jinou verzi, můžete nahrajte novou verzi komponentu clusteru a zkuste použít ve své úlohy.

> [AZURE.WARNING] Součásti součástí clusteru HDInsight jsou plně podporovány a Microsoft Support vám pomůže izolovat a vyřešit problémy týkající se tyto součásti.
>
> Vlastní součásti dostanou komerčně rozumné podpory pomáhají při další řešení problému. To může mít tento problém vyřešit nebo s žádostí o zapojit dostupných kanálů technologie otevřít zdroj, kde se nacházejí hloubkové odborných informací, které technologie. Například existuje mnoho webů komunity, které lze použít, třeba: [fórum MSDN pro HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Také Apache mít projekty webů projektů na [http://apache.org](http://apache.org), například: [Hadoop](http://hadoop.apache.org/), [jiskrou](http://spark.apache.org/).

## <a name="next-steps"></a>Další kroky

* [Migrace z Hdinsightu serveru s Windows na základě Linux](hdinsight-migrate-from-windows-to-linux.md)
* [Použití podregistru s HDInsight](hdinsight-use-hive.md)
* [Použití Prasátko s HDInsight](hdinsight-use-pig.md)
* [Použití MapReduce úlohy s HDInsight](hdinsight-use-mapreduce.md)
