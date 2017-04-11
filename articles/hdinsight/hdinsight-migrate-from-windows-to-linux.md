<properties
pageTitle="Migrace z Hdinsightu serveru s Windows na základě Linux HDInsight | Azure"
description="Zjistěte, jak migrovat z serveru s Windows HDInsight obrázku do obrázku na základě Linux HDInsight."
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
ms.date="10/28/2016"
ms.author="larryfr"/>

#<a name="migrate-from-a-windows-based-hdinsight-cluster-to-a-linux-based-cluster"></a>Migrace z serveru s Windows HDInsight obrázku do obrázku na základě Linux

Zatímco serveru s Windows HDInsight poskytuje snadný způsob, jak používat Hadoop v cloudu, můžete zjistit, že je třeba na základě Linux clusteru umožní využít výhod nástroje a technologií pro server, které jsou zapotřebí pro řešení. Hodně věcí v ekosystému Hadoop vyvíjejí na základě Linux systémy a některé nemusí být k dispozici pro použití se systémem Windows HDInsight. Více seznamů, videa a další školicí materiály předpokládá, že při práci s Hadoop používáte systém Linux.

Tento dokument obsahuje podrobnosti o rozdílech mezi HDInsight ve Windows a Linux a informace o tom, jak migrovat stávající úloh clusteru na základě Linux.

> [AZURE.NOTE] HDInsight clusterů používají se systémem Ubuntu dlouhodobou podpory (l) operační systém uzlů v clusteru. Další informace o verzi se systémem Ubuntu dostupné s HDInsight, spolu s dalšími informacemi součást správy verzí najdete v článku [HDInsight součást verzích](hdinsight-component-versioning.md).

## <a name="migration-tasks"></a>Úkoly při migraci

Obecné pracovního postupu pro migraci vypadá takto.

![Diagram pracovního postupu migrace](./media/hdinsight-migrate-from-windows-to-linux/workflow.png)

1.  Přečtěte si každou část tohoto dokumentu pochopit změny, které může být nutné při migraci stávajícího pracovního postupu, úkoly, atd. clusteru na základě Linux.

2.  Vytvoření clusteru na základě Linux jako prostředí assurance test a kvality. Další informace o vytváření Linux clusteru najdete v článku [na základě vytvořit Linux clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

3.  Zkopírujte existující projektů, zdrojů dat a propadů nového prostředí. Zobrazit data kopírovat do oddílu test prostředí další podrobnosti.

4.  Proveďte, abyste měli jistotu, že vaše úlohy fungovaly očekávaným nové clusteru testování ověření.

Po ověření, že všechno funguje očekávaným naplánujte výpadek služeb pro migraci. Během této prostoje proveďte následující akce.

1.  Zálohování přechodná data uložená místně na uzlů. Pokud například máte data uložená přímo na hlavní uzel.

2.  Odstranění obrázku serveru s Windows.

3.  Vytvořte na základě Linux clusteru pomocí stejného výchozí úložišti dat, který používá clusteru serveru s Windows. To vám umožní nového obrázku a pokračujte v práci proti dat do existující výroby.

4.  Importujte přechodná data, která je zálohovat.

5.  Zahájení práce/pokračovat ve zpracování pomocí nového obrázku.

### <a name="copy-data-to-the-test-environment"></a>Zkopírujte data do testovacím prostředí

Ke kopírování dat a úlohy mnoha způsoby, dvě popisované v této části jsou ale nejjednodušší metody, které se přímo přesunutí souborů do test obrázku.

#### <a name="hdfs-dfs-copy"></a>Kopírovat distribuovaného systému souborů HDFS

Příkazem Hadoop HDFS přímo kopírování dat z úložiště existujícího výrobní clusteru k základnímu úložišti pro nový cluster test pomocí následujících kroků.

1. Najděte úložiště informace o účtu a výchozí kontejner existujícího clusteru. Můžete to udělat pomocí tento skript Powershellu Azure.

        $clusterName="Your existing HDInsight cluster name"
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        write-host "Storage account name: $clusterInfo.DefaultStorageAccount.split('.')[0]"
        write-host "Default container: $clusterInfo.DefaultStorageContainer"

2. Postupujte podle kroků v na základě vytvořit Linux clusterů HDInsight dokumentu k vytvoření nové testovacím prostředí. Ukončení před vytvořením clusteru a místo toho vyberte **Volitelná konfigurace**.

3. Z zásuvné volitelná konfigurace vyberte **Propojené účty úložiště**.

4. Vyberte **Přidat klíč úložiště**a po zobrazení výzvy vyberte úložiště účet, který vrátil skript Powershellu v kroku 1. Klepněte na tlačítko **Vybrat** na každý zásuvné je ani zavřít. Nakonec vytvořte clusteru.

5. Po vytvoření clusteru připojit pomocí **SSH.** Pokud není znáte SSH pomocí HDInsight, přejděte k některému z těchto článků.

    * [Použití SSH s na základě Linux HDInsight klientů se systémem Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

    * [Použití SSH s na základě Linux HDInsight klientů Linux Unix a Mac](hdinsight-hadoop-linux-use-ssh-unix.md)

6. Z SSH relace pomocí následujícího příkazu kopírování souborů z propojené úložiště účtu do nového účtu úložiště výchozí. KONTEJNER a nahraďte účtu kontejner a informace o účtu vrácené skript Powershellu v kroku 1. Cesta k datům nahraďte cestu do datového souboru.

        hdfs dfs -cp wasbs://CONTAINER@ACCOUNT.blob.core.windows.net/path/to/old/data /path/to/new/location

    [AZURE.NOTE] Pokud na testovacím prostředí neexistuje struktuře adresář, který obsahuje data, můžete ho pomocí následujícího příkazu vytvořit.

        hdfs dfs -mkdir -p /new/path/to/create

    `-p` Přepnout umožňuje vytváření všechny adresáře v cestu.

#### <a name="direct-copy-between-azure-storage-blobs"></a>Přímá kopie mezi objekty BLOB Azure úložiště

Můžete taky, je vhodné použít `Start-AzureStorageBlobCopy` rutiny prostředí PowerShell Azure kopírování objektů BLOB mezi účty úložiště mimo HDInsight. Další informace najdete v tématu jak spravovat část pomocí prostředí PowerShell Azure s Azure úložiště objektů BLOB Azure.

##<a name="client-side-technologies"></a>Technologie straně klienta

Obecně klientských technologiemi usnadnění, například [rutiny prostředí PowerShell Azure](../powershell-install-configure.md), [Rozhraní příkazového řádku Azure](../xplat-cli-install.md) nebo [SDK .NET pro Hadoop](https://hadoopsdk.codeplex.com/) budou dál funguje s na základě Linux clusterů, stejně jako spolehnout REST API, které jsou stejná napříč obou typů obrázku s operačním systémem.

##<a name="server-side-technologies"></a>Technologie straně serveru

Následující tabulka obsahuje pokyny na migrace komponenty na serveru, které jsou specifické systému Windows.

| Pokud používáte tuto technologii... | Tato akce... |
| ----- | ----- |
| **Prostředí PowerShell** (skripty na straně serveru, včetně akce skriptu používá se při vytváření clusteru) | Revize jako flám skriptů. Skript akce najdete v článku [přizpůsobení Linux základě Hdinsightu pomocí skriptu akce](hdinsight-hadoop-customize-cluster-linux.md) a [skript akce vývoj na základě Linux HDInsight](hdinsight-hadoop-script-actions-linux.md). |
| **Azure rozhraní příkazového řádku** (skripty na straně serveru) | Když rozhraní příkazového řádku Azure je k dispozici na Linux, nepochází předinstalovaná na uzlů hlavy HDInsight. Pokud budete potřebovat pro skriptování na straně serveru, naleznete v tématu [instalace rozhraní příkazového řádku Azure](../xplat-cli-install.md) informace o instalaci na základě Linux platformách. |
| **Komponenty .NET** | .NET není plně podporované na základě Linux HDInsight clusterů. Na základě Linux bouře na HDInsight clusterů vytvořené po 10/28/2017 podpory C# bouře topologií pomocí rozhraní SCP.NET. Další podporu pro .NET se přidají v budoucích aktualizacích. |
| **Součásti Win32 nebo jiné technologie jenom pro Windows** | Pokyny pro závisí na součást nebo technologie; Možná budete moct najít verzi, která je kompatibilní se službou Linux nebo budete muset najít alternativní řešení nebo přepsat tuto součást. |

##<a name="cluster-creation"></a>Vytvoření obrázku

Tato část obsahuje informace o rozdílech mezi vytváření clusteru.

### <a name="ssh-user"></a>SSH uživatele

Na základě Linux clusterů HDInsight pomocí protokolu **Zabezpečit prostředí (SSH)** vzdáleného přístupu k uzlů. Na rozdíl od na základě vzdálené ploše pro systém Windows clusterů většina klientů SSH nenabízejí grafické uživatelské prostředí, ale místo poskytuje příkazového řádku, který vám umožní pracovat příkazy na clusteru. V některých klientech (například [MobaXterm](http://mobaxterm.mobatek.net/),) zadejte Prohlížeč grafických souborů systému kromě vzdáleného příkazového řádku.

Při vytváření clusteru je nutné zadat uživatel SSH a buď **heslo** nebo **certifikát veřejného klíče** pro ověřování.

Doporučujeme použít certifikát veřejného klíče, jako je bezpečnější než pomocí hesla. Ověření certifikátů funguje generovat podepsané veřejné a privátní klíče a následným poskytující veřejným klíčem při vytváření clusteru. Při připojení k serveru pomocí SSH privátním klíčem v klientském počítači poskytuje ověřování pro připojení.

Další informace o použití SSH s Hdinsightu najdete tady:

- [Pomocí SSH HDInsight klientů se systémem Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

- [Pomocí SSH HDInsight klientů Linux Unix a OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

### <a name="cluster-customization"></a>Úpravy obrázku

**Akce skriptu** používá se systémem Linux clusterů musí být napsáno flám skriptu. Zatímco akce skriptu lze použít při vytváření clusteru, na základě Linux clusterů také lze použít k provádění úprav po clusteru nahoru a pracovního. Další informace najdete v tématu [přizpůsobení Linux základě Hdinsightu pomocí skriptu akce](hdinsight-hadoop-customize-cluster-linux.md) a [skript akce vývoj na základě Linux HDInsight](hdinsight-hadoop-script-actions-linux.md).

Jiné funkce vlastního nastavení je **zavádění**. Windows clusterů umožňuje určit umístění další knihovny pro použití s podregistru. Po vytvoření clusteru, se automaticky k dispozici pro použití s dotazy podregistru bez nutnosti použití těchto knihoven `ADD JAR`.

Zavádění na základě Linux clusterů neposkytuje tuto funkci. Místo toho použijte akci skriptu popsány v [knihovnách přidat podregistru během vytváření clusteru](hdinsight-hadoop-add-hive-libraries.md).

### <a name="virtual-networks"></a>Virtuální sítě

Serveru s Windows HDInsight clusterů funkční pouze klasické virtuální sítích, zatímco na základě Linux HDInsight clusterů vyžadují správce prostředků virtuální sítě. Pokud máte zdrojů v síti klasické virtuální, který clusteru Linux HDInsight musí připojení k najdete v článku [připojení klasické virtuální sítě k síti virtuální správce prostředků](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

Další informace o konfiguraci požadavky pro pomocí virtuálních sítí Azure Hdinsightu najdete v článku [funkce rozšíření HDInsight pomocí virtuální sítě](hdinsight-extend-hadoop-virtual-network.md).

##<a name="management-and-monitoring"></a>Správa a sledování

V mnoha web uživatelská rozhraní jste použili s HDInsight serveru s Windows, třeba historie úlohy nebo vláken uživatelské rozhraní aplikace jsou dostupné prostřednictvím Ambari. Kromě toho zobrazení podregistru Ambari umožňuje spouštění dotazů podregistru pomocí webového prohlížeče. Uživatelské rozhraní webu Ambari je dostupná na základě Linux clusterů na https://CLUSTERNAME.azurehdinsight.net.

Další informace o práci s Ambari najdete v tématu tyto dokumenty:

- [Ambari Web](hdinsight-hadoop-manage-ambari.md)

- [Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md)

### <a name="ambari-alerts"></a>Ambari upozornění

Ambari má upozornění systému, který se dozvíte, potenciálních problémů s clusteru. Upozornění se zobrazí jako červené nebo žlutá položky v uživatelském rozhraní Web Ambari však můžete také zadat pomocí rozhraní REST API.

> [AZURE.IMPORTANT] Upozornění na Ambari označuje, které tam *může* jít o problém, že tam není *je* problém. Například může se zobrazit upozornění, že nejsou k dispozici HiveServer2, i když se k němu přístup běžným způsobem.
>
> Mnoho upozornění implementovaná jako dotazům interval službu a očekávají, že odpověď v určitém časovém rozmezí. Tak, aby upozornění nemusí znamenat, že služba není dostupný, právě, že jste se vlastně vrátit výsledky v očekávané časovém období.

Obecně byste měli hodnotit upozornění byla výskytu delší dobu i zrcadlí uživatele problémy, které dřív byly oznámeny s clusteru před provedením akcí u ho.

##<a name="file-system-locations"></a>Umístění systému souborů

Systém souborů Linux obrázku je rozložení jinak než clusterů serveru s Windows HDInsight. Umožňuje najít běžně používaných soubory v následující tabulce.

| Když potřebujete najít... | Je umístěn... |
| ----- | ----- |
| Konfigurace | `/etc`. Například`/etc/hadoop/conf/core-site.xml` |
| Protokoly | `/var/logs` |
| Hortonworks Data platformu (HDP) | `/usr/hdp`. Existují dva adresáře najdete tady je aktuální verzi HDP (například `2.2.9.1-1`,) a `current`. `current` Adresář obsahuje symbolické odkazů na soubory a adresáře umístěnou v adresáři číslo verze a je k dispozici a tak pohodlný přístup k souborům HDP od verze číslo se měnila aktualizovanou verzi HDP. |
| hadoop streaming.jar | `/usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar` |

Obecně Pokud znáte název souboru, můžete tento příkaz z relaci SSH zobrazíte cesta k souboru:

    find / -name FILENAME 2>/dev/null

Můžete taky použít zástupné znaky s názvem souboru. Například `find / -name *streaming*.jar 2>/dev/null` vrátí cestu sklenice souborů, které obsahují slovo streamování jako součást názvu souboru.

##<a name="hive-pig-and-mapreduce"></a>Podregistru, Prasátko a MapReduce

Prasátko a MapReduce pracovního vytížení se hodně podobají na základě Linux clusterů – hlavní rozdíl je-li vzdálené plochy umožňuje připojit k obrázku serveru s Windows a spuštění úloh použijete SSH s clusterů na základě Linux.

- [Použití Prasátko s SSH](hdinsight-hadoop-use-pig-ssh.md)

- [Použití MapReduce s SSH](hdinsight-hadoop-use-mapreduce-ssh.md)

### <a name="hive"></a>Podregistru

Následující diagram obsahuje pokyny o migraci podregistru úloh.

| Na serveru s Windows, můžu použijte v těchto případech | Na základě Linux... |
| ----- | ----- |
| **Editor podregistru** | [Zobrazení podregistru v Ambari](hdinsight-hadoop-use-hive-ambari-view.md) |
| `set hive.execution.engine=tez;`Chcete-li povolit Tez | Tez je výchozí modul spuštění na základě Linux clusterů, takže příkaz nastavit již není potřeba. |
| Soubory CMD nebo skriptů na serveru vyvolat jako součást podregistru projektu | použití flám skriptů |
| `hive`příkaz Vzdálená plocha | Použijte [Beeline](hdinsight-hadoop-use-hive-beeline.md) [podregistru z SSH relace](hdinsight-hadoop-use-hive-ssh.md) |

##<a name="storm"></a>Bouře

| Na serveru s Windows, můžu použijte v těchto případech | Na základě Linux... |
| ----- | ----- |
| Řídicí panel bouře | Řídicí panel bouře není k dispozici. V tématu [nasazení a správu bouře topologií na základě Linux HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md) způsoby, jak odeslat topologií |
| Bouře uživatelského rozhraní | Uživatelské rozhraní bouře je k dispozici na https://CLUSTERNAME.azurehdinsight.net/stormui |
| Visual Studio k vytváření, nasazení a správu C# nebo hybridní topologií | Visual Studio můžete použít k vytváření, nasazení a správu C# (SCP.NET) nebo hybridní topologií na základě Linux bouře na HDInsight clusterů vytvořené po 10/28/2017. |

##<a name="hbase"></a>HBase

Na základě Linux clusterů je nadřazený znode pro HBase `/hbase-unsecure`. Musíte to v konfiguraci pro každého klienta Java aplikace, které využívají nativní API Java HBase.

Příklad klienta, který tuto hodnotu nastavuje najdete v článku [Vytvoření aplikace založené na Java HBase](hdinsight-hbase-build-java-maven.md) .

##<a name="spark"></a>Spark

Během náhledu a byly k dispozici v systému Windows clusterů Spark clusterů však vydání Spark je k dispozici pouze s clusterů na základě Linux. Neexistuje cesta migrace z clusteru náhled založený na Windows Spark clusteru na základě Linux Spark vydání.

##<a name="known-issues"></a>Známé problémy

### <a name="azure-data-factory-custom-net-activities"></a>Azure Data Factory vlastních .NET aktivit

Azure Data Factory vlastních .NET aktivit aktuálně nepodporuje na základě Linux HDInsight clusterů. Místo toho používejte jednu z následujících metod implementovat vlastní aktivity jako součást vašeho ADF kanálem k odesílání zpráv.

-   Na Azure dávku fondu proveďte .NET aktivity. Projděte si část propojené dávku Azure pomocí služby [použít vlastní aktivity v Azure Data Factory kanálu](../data-factory/data-factory-use-custom-activities.md#AzureBatch)

-   Implementace aktivity jako MapReduce aktivity. Další informace najdete v tématu [Vyvolat programů MapReduce Data Factory](../data-factory/data-factory-map-reduce.md) .

### <a name="line-endings"></a>Konce řádků

Konce řádků v systémech Windows se obvykle používá Line FEED, zatímco Linux se systémem používají LF. Pokud plodiny nebo očekávat, data se Line FEED konce řádků, budete muset upravit výrobci nebo spotřebitele pro práci s koncovkou LF řádku.

Například online přes Azure HDInsight dotazu clusteru serveru s Windows vrátí dat pomocí Line FEED. Stejný dotaz s Linux clusteru vrátí LF. V mnoha případech to nezáleží příjemci dat však je zabývat před migrací do clusteru na základě Linux.

Pokud máte skripty provedených přímo na Linux uzlech (například Python skript použitý pomocí úloh MapReduce nebo podregistru), byste měli vždy použít LF jako poslední řádek. Pokud používáte Line FEED, může se objevit chyby skriptů výpočetnímu clusteru na základě Linux.

Pokud víte, že tyto skripty neobsahují řetězce s tímto CR znaky, se dají dělat hromadné změnit konce řádků pomocí jedné z těchto způsobů:

-   **Pokud máte skripty, které plánujete odesílá clusteru**, pomocí následující příkazy Powershellu můžete změnit konce řádků z Line FEED LF před nahráním skript ke clusteru.

        $original_file ='c:\path\to\script.py'
        $text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
        [IO.File]::WriteAllText($original_file, $text)

-   **Pokud máte skripty, které jsou již v úložišti používaný clusteru**, můžete tento příkaz z relaci SSH obrázku na základě Linux upravte skript.

        hdfs dfs -get wasbs:///path/to/script.py oldscript.py
        tr -d '\r' < oldscript.py > script.py
        hdfs dfs -put -f script.py wasbs:///path/to/script.py

##<a name="next-steps"></a>Další kroky

-   [Naučte se vytvářet na základě Linux HDInsight clusterů](hdinsight-hadoop-provision-linux-clusters.md)

-   [Připojení k obrázku na základě Linux pomocí SSH z klienta se systémem Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

-   [Připojení k obrázku na základě Linux pomocí SSH z klienta Linux, Unix nebo Mac](hdinsight-hadoop-linux-use-ssh-unix.md)

-   [Správa na základě Linux clusteru pomocí Ambari](hdinsight-hadoop-manage-ambari.md)
