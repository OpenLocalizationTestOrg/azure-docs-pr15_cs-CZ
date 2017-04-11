<properties
    pageTitle="Použití odstín s Hadoop na HDInsight Linux clusterů | Microsoft Azure"
    description="Zjistěte, jak nainstalovat a používat odstín systému Hadoop clusterů na HDInsight Linux."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/13/2016" 
    ms.author="nitinme"/>

# <a name="install-and-use-hue-on-hdinsight-hadoop-clusters"></a>Instalace a používání odstín v HDInsight Hadoop clusterů

Zjistěte, jak nainstalovat odstínem HDInsight Linux clusterů, můžete tunneling směrovat požadavky na odstín.

## <a name="what-is-hue"></a>Co je odstín?

Odstín je sada webových aplikací slouží k interakci s Hadoop obrázku. Odstín umožňuje procházet úložiště přidružen clusteru Hadoop (v případě HDInsight clusterů WASB), spusťte podregistru úlohy a skripty Prasátko atd. Následující součásti jsou dostupné instalace odstín clusteru HDInsight Hadoop.

* Editor podregistru včelí vosk
* Prasátko
* Správce Metastore
* Oozie
* FileBrowser (který mluví WASB výchozí kontejneru)
* Prohlížeč projektu

> [AZURE.WARNING] Součásti součástí clusteru HDInsight jsou plně podporovány a Microsoft Support vám pomůže izolovat a vyřešit problémy týkající se tyto součásti.
>
> Vlastní součásti dostávat komerčně rozumné podpory pomáhají při další řešení problému. To může mít za následek tento problém vyřešit nebo s žádostí o zapojení dostupných kanálů technologie otevřít zdroj, kde se nacházejí hloubkové odborných informací, které technologie. Například existuje mnoho webů komunity, které lze použít, třeba: [fórum MSDN pro HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Také Apache mít projekty webů projektů na [http://apache.org](http://apache.org), například: [Hadoop](http://hadoop.apache.org/).

## <a name="install-hue-using-script-actions"></a>Instalace odstín pomocí skriptu akce

Následující akci skriptu lze použít k instalaci odstín clusteru na základě Linux HDInsight.
https://hdiconfigactions.BLOB.Core.Windows.NET/linuxhueconfigactionv02/Install-Hue-uber-v02.SH
    
Tato část obsahuje pokyny k tomu, jak používat skript, když zřizujete obrázku na portálu Azure. 

> [AZURE.NOTE] Azure Powershellu, rozhraní příkazového řádku Azure, HDInsight .NET SDK nebo správce prostředků Azure šablony lze také použít skript akce. Můžete taky použít akce skriptu na spuštěná clusterů. Další informace najdete v tématu [přizpůsobení HDInsight clusterů akcím skriptu](hdinsight-hadoop-customize-cluster-linux.md).

1. Spuštění zřizování clusteru pomocí kroků v [poskytování HDInsight clusterů na Linux](hdinsight-hadoop-provision-linux-clusters.md#portal), ale ne dokončování zřizování.

    > [AZURE.NOTE] Pokud chcete nainstalovat odstín HDInsight clusterů, doporučené headnode velikost je nejméně A4 (8 jádra, 14 GB paměti).

2. Na zásuvné **Volitelná konfigurace** vyberte **Akce skriptu**a zadejte informace, jak je ukázáno v následujícím příkladu:

    ![Parametry akce poskytněte skriptu pro odstín] (./media/hdinsight-hadoop-hue-linux/hue_script_action.png "Parametry akce poskytněte skriptu pro odstín")

    * __Název__: Zadejte popisný název akci skriptu.
    * __Identifikátor URI skript__: https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
    * __Hlavy__: Toto políčko zaškrtněte,
    * __PRACOVNÍKA__: nechte toto pole prázdné.
    * __ZOOKEEPER__: nechte toto pole prázdné.
    * __Parametry__: nechte toto pole prázdné.

3. V dolní části **Script akce**uložit konfiguraci pomocí tlačítka **Vybrat** . Nakonec pomocí tlačítka **Vybrat** v dolní části zásuvné **Volitelná konfigurace** pro uložení informací o volitelná konfigurace.

4. Pokračujte v zřizování clusteru podle [ustanovení HDInsight clusterů na Linux](hdinsight-hadoop-provision-linux-clusters.md#portal).

## <a name="use-hue-with-hdinsight-clusters"></a>Použití odstín s HDInsight clusterů

SSH Tunneling je jediný způsob, jak získat přístup k odstín na clusteru se po spuštění. Tunneling prostřednictvím SSH umožňuje přenos přejdete přímo na headnode clusteru, kde je spuštěný odstín. Po dokončení clusteru zřizování pomocí následujících kroků využívat odstín clusteru HDInsight Linux.

1. Informace o [Použití SSH Tunneling dostanete k Ambari web uživatelského rozhraní, ResourceManager, JobHistory, NameNode, Oozie a jiným webovým uživatelské rozhraní na](hdinsight-linux-ambari-ssh-tunnel.md) vytvoření pomocí SSH tunelem ze systému klienta clusteru HDInsight a potom i konfiguraci webového prohlížeče používat tunelem SSH jako proxy server.

2. Když jste vytvořili SSH tunelem a konfiguraci prohlížeče proxy přenosů na to, musí zjistíte název hostitele primárního hlavního uzlu. Lze provést pomocí připojení k clusteru pomocí SSH na port 22. Například `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net` kde __uživatelské jméno__ je vaše SSH uživatelské jméno a __NÁZEV_CLUSTERU__ je název vašeho obrázku.

    Další informace o použití SSH najdete v tématu tyto dokumenty:

    * [Použití SSH s na základě Linux HDInsight z klienta Linux, Unix nebo Mac OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [Použití SSH s na základě Linux HDInsight z klienta se systémem Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

3. Po připojení použijte získat plně kvalifikovaný název domény primární headnode tento příkaz:

        hostname -f

    Vrátí tento název podobná této:

        hn0-myhdi-nfebtpfdv1nubcidphpap2eq2b.ex.internal.cloudapp.net
    
    Toto je hostname primárního headnode, které je umístěn na webu odstín.

2. Otevřete portál odstín na http://HOSTNAME:8888 pomocí prohlížeče. Nahraďte název hostitele název, který jste získali v předchozím kroku.

    > [AZURE.NOTE] Při přihlášení poprvé, budete vyzváni k vytvoření účet pro přihlášení k portálu odstín. Přihlašovací údaje, který tady zadáte se omezí na portálu a netýkají správce nebo jste zadali při poskytování clusteru SSH přihlašovací údaje uživatele.

    ![Přihlášení k portálu odstín] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Login.png "Zadejte přihlašovací údaje pro odstín portálu")

### <a name="run-a-hive-query"></a>Spuštění dotazu podregistru

1. Z portálu odstín klikněte na **Editory dotaz**a potom klikněte na tlačítko k otevření editoru podregistru **podregistru** .

    ![Použití podregistru] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Hive.png "Použití podregistru")

2. Na kartě **pomáhat** ve skupinovém rámečku **databáze**byste měli vidět **hivesampletable**. Toto je ukázkové tabulky, která je součástí všech clusterů Hadoop na HDInsight. Zadejte ukázkový dotaz v pravém podokně a najdete v článku výstup na kartě **výsledky** v podokně pod, jak je znázorněno snímek obrazovky.

    ![Spuštění podregistru dotazu] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Hive.Query.png "Spuštění podregistru dotazu")

    Můžete taky použít kartu **graf** zobrazíte vizuální znázornění atributu výsledku.

### <a name="browse-the-cluster-storage"></a>Procházet úložiště obrázku

1. Z portálu odstínem klikněte v pravém horním rohu na řádek nabídek **Prohlížeče souborů** .

2. Ve výchozím nastavení otevře prohlížeč souborů v adresáři **/user/myuser** . Klikněte na tlačítko lomítko přímo před adresáři uživatele v parametru path přejděte do kořenové kontejneru Azure úložiště přidružené clusteru.

    ![Použití souborů prohlížeče] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.File.Browser.png "Použití souborů prohlížeče")

3. Klikněte pravým tlačítkem myši na soubor nebo složku, kterou chcete zobrazit dostupné operace. Pomocí tlačítka **Odeslat** v pravém rohu nahrát soubory k aktuálnímu adresáři. Umožňuje vytvořit nové soubory nebo adresáře na tlačítko **Nový** .

> [AZURE.NOTE] Prohlížeče souborů odstín jenom zobrazit obsah kontejneru výchozí přidružen clusteru HDInsight. Žádné další úložiště účty/kontejnery přidružené může mít clusteru nebudou přístupný pomocí prohlížeče souborů. Ale další kontejnery přidružené clusteru bude vždy přístupné pro podregistru úlohy. Řekněme, že zadejte příkaz `dfs -ls wasbs://newcontainer@mystore.blob.core.windows.net` v editoru podregistru, zobrazí se obsah i další kontejnery. V tomto příkazu **newcontainer** není kontejneru výchozí přidružené clusteru.

## <a name="important-considerations"></a>Důležité informace

1. Skript použili k instalaci odstínem nainstaluje ji jenom primární headnode clusteru.

2. Během instalace více služeb Hadoop (HDFS vláken, MR2, Oozie) se po restartování aktualizaci konfigurace. Po dokončení instalace odstín skript může trvat delší dobu jiných služeb Hadoop chcete spustit aplikaci. Může to mít vliv na odstín výkon původně. Po spuštění všechny služby, budou odstín plně funkční.

3.  Odstín nerozumí Tez úlohy, což je aktuální výchozí podregistru. Pokud chcete použít MapReduce jako modul spuštění podregistru, aktualizujte skript, který chcete používat příkaz ve vašem skriptu:

        set hive.execution.engine=mr;

4.  Pomocí Linux clusterů může mít situace kde služeb spuštěných pro primární headnode spuštěná správce prostředků může být na sekundární. Tato situace může způsobit chyby (viz níže) při zobrazení podrobností o spuštění úlohy na clusteru pomocí odstín. Po dokončení projektu, můžete zobrazit podrobnosti projektu.

    ![Odstín portálu chyby] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Error.png "Odstín portálu chyby")

    Toto je kvůli známý problém. Jako alternativu upravte tak, aby aktivní správce prostředků taky na primární headnode Ambari.

5.  Odstínem rozumí WebHDFS během clusterů HDInsight pomocí úložišti Azure pomocí `wasbs://`. Ano vlastní skript použít s akci skriptu nainstaluje WebWasb, které je služba kompatibilní s prohlížečem WebHDFS rozhovor WASB. Ano, i když je portál odstín říká HDFS v seznamu místa (třeba když jste přesunutím ukazatele myši **Prohlížeči souborů**), ho by měl být interpretován jako WASB.


## <a name="next-steps"></a>Další kroky

- [Instalace Giraph na clusterů HDInsight](hdinsight-hadoop-giraph-install-linux.md). Pomocí úprav obrázku Giraph nainstalovat clusterů HDInsight Hadoop. Giraph umožňuje provádět zpracování grafu pomocí Hadoop a lze jej využít s Azure HDInsight.

- [Instalace Solr na clusterů HDInsight](hdinsight-hadoop-solr-install-linux.md). Pomocí úprav obrázku Solr nainstalovat clusterů HDInsight Hadoop. Solr umožňuje provádět výkonné vyhledávání operací na uložená data.

- [Instalace R na clusterů HDInsight](hdinsight-hadoop-r-scripts-linux.md). Pomocí úprav obrázku R nainstalovat clusterů HDInsight Hadoop. R je otevřít zdrojového jazyka a prostředí statistické výpočetních. Poskytuje stovky předdefinované statistických funkcí a vlastní programovací jazyk, který kombinuje aspekty funkční a objektově orientovaném programování. Také poskytuje rozsáhlé grafické možnosti.

[powershell-install-configure]: install-configure-powershell-linux.md
[hdinsight-provision]: hdinsight-provision-clusters-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
