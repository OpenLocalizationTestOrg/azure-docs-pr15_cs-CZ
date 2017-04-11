<properties
pageTitle="Porty používané HDInsight | Azure"
description="Seznam porty používají Hadoop služby spuštěna HDInsight."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="10/03/2016"
ms.author="larryfr"/>

# <a name="ports-and-uris-used-by-hdinsight"></a>Porty a protokoly URI používaný HDInsight

Tento dokument obsahuje seznam porty používají Hadoop služby na základě Linux HDInsight clusterů. Obsahuje také informace o porty připojuje k clusteru pomocí SSH.

## <a name="public-ports-vs-non-public-ports"></a>Veřejné porty a jiné veřejné porty

Na základě Linux clusterů HDInsight zpřístupňuje pouze tři porty veřejně na Internetu. 22 23 a 443. Jsou použity zabezpečený přístup k obrázku pomocí SSH a služby prostřednictvím zabezpečeného protokol HTTPS.

Interně HDInsight prováděna tak, že několik Azure virtuálních počítačích (uzlů v rámci clusteru,) provozu v síti virtuální Azure. Z v rámci virtuální sítě, dostanete se k portů není přes internet. Například pokud se připojujete k jednomu hlavy uzlů pomocí SSH, z hlavního uzlu potom přímo dostanete služeb spuštěných uzlů.

> [AZURE.IMPORTANT] Když vytvoříte HDInsight clusteru, pokud nezadáte virtuální sítě Azure mezi možnostmi konfigurace, žádné vytvoří; přesto se nemůžete připojit jiných počítačů (například jiné virtuálních počítačích Azure nebo klientském počítači vývoj,) na roli automaticky vytvoří virtuální sítě. 

Ke spojení dalších počítačů a virtuální sítě, musíte nejprve vytvořit virtuální sítě a při vytváření svůj cluster HDInsight ho zadat. Další informace najdete v tématu [funkce rozšíření HDInsight pomocí virtuální sítě Azure](hdinsight-extend-hadoop-virtual-network.md)

## <a name="public-ports"></a>Veřejné porty

Všechny uzlů v clusteru HDInsight jsou umístěny v síti virtuální Azure a přístupná přímo z Internetu. Veřejné brány umožňuje přístup k Internetu na tyto porty, které jsou běžné všech typů clusteru HDInsight.

| Služba | Port | Protocol (protokol) | Popis |
| ---- | ---------- | -------- | ----------- | ----------- |
| sshd | 22 | SSH | Klienti se připojí k sshd na primární headnode. Přečtěte si článek [použití SSH s na základě Linux HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md) |
| sshd | 22 | SSH | Klienti se připojí k sshd na uzel okraje (pouze HDInsight Premium). V tématu [Začínáme s používáním R Server místní HDInsight](hdinsight-hadoop-r-server-get-started.md) |
| sshd | 23 | SSH | Klienti se připojí k sshd na vedlejší headnode. Přečtěte si článek [použití SSH s na základě Linux HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md) |
| Ambari | 443 | PROTOKOL HTTPS | Web Ambari uživatelského rozhraní. V tématu [Správa HDInsight pomocí webového Ambari uživatelského rozhraní](hdinsight-hadoop-manage-ambari.md) |
| Ambari | 443 | PROTOKOL HTTPS | Rozhraní REST API Ambari. V tématu [Správa HDInsight pomocí rozhraní REST API Ambari](hdinsight-hadoop-manage-ambari-rest-api.md) |
| WebHCat | 443 | PROTOKOL HTTPS | HCatalog REST API. V tématu [použití podregistru s otočení](hdinsight-hadoop-use-pig-curl.md), [použijte Prasátko s otočení](hdinsight-hadoop-use-pig-curl.md), [použijte MapReduce s otočení](hdinsight-hadoop-use-mapreduce-curl.md) |
| HiveServer2 | 443 | ROZHRANÍ ODBC | Připojí k podregistru pomocí rozhraní ODBC. V tématu [Připojit Excel k Hdinsightu pomocí ovladač Microsoft ODBC](hdinsight-connect-excel-hive-odbc-driver.md). |
| HiveServer2 | 443 | JDBC | Připojí k podregistru pomocí JDBC. Přečtěte si článek [připojení k podregistru na HDInsight pomocí ovladače podregistru JDBC](hdinsight-connect-hive-jdbc-driver.md) |

Následující jsou dostupné pro konkrétní clusteru typy:

| Služba | Port | Protocol (protokol) |Typ obrázku | Popis |
| ------------ | ---- |  ----------- | --- | ----------- |
| Stargate | 443 | PROTOKOL HTTPS | HBase | Rozhraní REST API HBase. V tématu [Začínáme s používáním HBase](hdinsight-hbase-tutorial-get-started-linux.md) |
| Livius | 443 | PROTOKOL HTTPS |  Spark | Rozhraní REST API Spark. V tématu [odeslání Spark úlohy vzdáleně používat Livius](hdinsight-apache-spark-livy-rest-interface.md) |
| Bouře | 443 | PROTOKOL HTTPS | Bouře | Web bouře uživatelského rozhraní. V tématu [nasazení a správu bouře topologií na HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="authentication"></a>Ověřování

Musí být ověřeny všechny služby veřejně vystaven na Internetu:

| Port | Přihlašovací údaje |
| ---- | ----------- |
| 22 nebo 23 | Přihlašovací údaje uživatele SSH zadané při vytváření obrázku |
| 443 | Přihlašovací jméno (výchozí: správce) a heslo, které byly během vytváření clusteru nastavit |

## <a name="non-public-ports"></a>Jiné veřejné porty

> [AZURE.NOTE] Některé služby jsou dostupné pro konkrétní clusteru typy jenom. Například HBase je k dispozici pouze u typů HBase obrázku.

### <a name="hdfs-ports"></a>Porty HDFS

| Služba | Uzly | Port | Protocol (protokol) | Popis |
| ------- | ------- | ---- | -------- | ----------- | 
| Web NameNode uživatelského rozhraní | Hlavy uzlů | 30070 | PROTOKOL HTTPS | Web uživatelského rozhraní pro zobrazit aktuální stav |
| Služba NameNode metadat | hlavy uzlů | 8020 | IPC | Soubor systému metadat 
| DataNode | Všechny pracovní uzly | 30075 | PROTOKOL HTTPS | Web uživatelského rozhraní pro zobrazení stavu protokoly, atd. |
| DataNode | Všechny pracovní uzly | 30010 | &nbsp; | Přenos dat |
| DataNode | Všechny pracovní uzly | 30020 | IPC | Operace metadat |
| Sekundární NameNode | Hlavy uzlů | 50090 | NASTAVIT INFORMACE HTTP | Kontrolní bod pro NameNode metadata |

### <a name="yarn-ports"></a>Porty vláken

| Služba | Uzly | Port | Protocol (protokol) | Popis |
| ------- | ------- | ---- | -------- | ----------- |
| Správce prostředků web uživatelského rozhraní | Hlavy uzlů | 8088 | NASTAVIT INFORMACE HTTP | Web uživatelského rozhraní pro správce prostředků |
| Správce prostředků web uživatelského rozhraní | Hlavy uzlů | 8090 | PROTOKOL HTTPS | Web uživatelského rozhraní pro správce prostředků |
| Správce prostředků správce rozhraní | hlavy uzlů | 8141 | IPC | Pro odeslání aplikace (podregistru, podregistru serveru Prasátko, atd.) |
| Správce prostředků Plánovač | hlavy uzlů | 8030 | NASTAVIT INFORMACE HTTP | Rozhraní pro správu |
| Správce prostředků rozhraní | hlavy uzlů | 8050 | NASTAVIT INFORMACE HTTP |Adresa rozhraní Správce aplikací |
| NodeManager | Všechny pracovní uzly | 30050 | &nbsp; | Adresa správce kontejneru |
| Web NodeManager uživatelského rozhraní | Všechny pracovní uzly | 30060 | NASTAVIT INFORMACE HTTP | Rozhraní Správce zdrojů |
| Adresa časové osy | Hlavy uzlů | 10200 | VZDÁLENÉ VOLÁNÍ PROCEDUR | Časovou osu RPC služba. |
| Časová osa web uživatelského rozhraní | Hlavy uzlů | 8181 | NASTAVIT INFORMACE HTTP | Web služby časovou osu uživatelského rozhraní |

### <a name="hive-ports"></a>Porty podregistru

| Služba | Uzly | Port | Protocol (protokol) | Popis |
| ------- | ------- | ---- | -------- | ----------- |
| HiveServer2 | Hlavy uzlů | 10001 | Thrift | Služba programové připojení ke podregistru (Thrift/JDBC) |
| HiveServer | Hlavy uzlů | 10000 | Thrift | Služba programové připojení ke podregistru (Thrift/JDBC) |
| Podregistru Metastore | Hlavy uzlů | 9083 | Thrift | Služba programové připojení ke podregistru metadata (Thrift/JDBC) |

### <a name="webhcat-ports"></a>Porty WebHCat

| Služba | Uzly | Port | Protocol (protokol) | Popis |
| ------- | ------- | ---- | -------- | ----------- |
| WebHCat serveru | Hlavy uzlů | 30111 | NASTAVIT INFORMACE HTTP | Webového rozhraní API nad HCatalog a dalších službách Hadoop |

### <a name="mapreduce-ports"></a>Porty MapReduce

| Služba | Uzly | Port | Protocol (protokol) | Popis |
| ------- | ------- | ---- | -------- | ----------- |
| JobHistory | Hlavy uzlů | 19888 | NASTAVIT INFORMACE HTTP | Web MapReduce JobHistory uživatelského rozhraní |
| JobHistory | Hlavy uzlů | 10020 | &nbsp; | MapReduce JobHistory serveru |
| ShuffleHandler | &nbsp; | 13562 | &nbsp; | Přenosy intermediate mapy uloží do žádosti o přechodky |

### <a name="oozie"></a>Oozie

| Služba | Uzly | Port | Protocol (protokol) | Popis |
| ------- | ------- | ---- | -------- | ----------- |
| Oozie serveru | Hlavy uzlů | 11000 | NASTAVIT INFORMACE HTTP | Adresa URL služby Oozie |
| Oozie serveru | Hlavy uzlů | 11001 | NASTAVIT INFORMACE HTTP | Pro správu Oozie port |

### <a name="ambari-metrics"></a>Metriky Ambari

| Služba | Uzly | Port | Protocol (protokol) | Popis |
| ------- | ------- | ---- | -------- | ----------- |
| Časová osa (aplikace historie) | Hlavy uzlů | 6188 | NASTAVIT INFORMACE HTTP | Web služby časovou osu uživatelského rozhraní |
| Časová osa (aplikace historie) | Hlavy uzlů | 30200 | VZDÁLENÉ VOLÁNÍ PROCEDUR | Web služby časovou osu uživatelského rozhraní |

### <a name="hbase-ports"></a>Porty HBase

| Služba | Uzly | Port | Protocol (protokol) | Popis |
| ------- | ------- | ---- | -------- | ----------- |
| HMaster | Hlavy uzlů | 16000 | &nbsp; | &nbsp; |
| Informace o HMaster uživatelské rozhraní webu | Hlavy uzlů | 16010 | NASTAVIT INFORMACE HTTP | Port pro HBase předlohy webu uživatelského rozhraní |
| Oblast serveru | Všechny pracovní uzly | 16020 | &nbsp; | &nbsp; |
| &nbsp; | &nbsp; | 2181 | &nbsp; | Číslo portu, klienty používáte pro připojení k ZooKeeper |

### <a name="kafka-ports"></a>Porty Kafka

| Služba | Uzly | Port | Protocol (protokol) | Popis |
| ------- | ------- | ---- | -------- | ----------- |
| Zprostředkovatel  | Pracovní uzlů | 9092 | [Kafka síťový protokol](http://kafka.apache.org/protocol.html) | Používá pro komunikaci s klientem |
| &nbsp; | Zookeeper uzlů | 2181 | &nbsp; | Číslo portu, klienty používáte pro připojení k Zookeeper |
