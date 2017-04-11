<properties
    pageTitle="Použití R HDInsight přizpůsobení clusterů | Microsoft Azure"
    description="Zjistěte, jak nainstalovat R pomocí skriptu akce a použít pravidlo na clusterů HDInsight."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="jgao"/>

# <a name="install-and-use-r-on-hdinsight-hadoop-clusters"></a>Instalace a používání R v HDInsight Hadoop clusterů

Zjistěte, jak přizpůsobit Windows na základě HDInsight obrázku s R pomocí skriptu akce a clusterů používání R na HDInsight. [Vrstvy premium](https://azure.microsoft.com/pricing/details/hdinsight/) nabízející pro HDInsight obsahuje R Server v rámci svůj cluster HDInsight. Díky R skripty MapReduce a Spark spuštění distribuované výpočty. Další informace najdete v tématu [Začínáme s používáním R Server místní HDInsight](hdinsight-hadoop-r-server-get-started.md). Informace o použití R s Linux clusteru najdete v tématu [instalace a používání R na clusterů HDinsight Hadoop (Linux)](hdinsight-hadoop-r-scripts-linux.md).
 
R typy dosažených obrázku (Hadoop bouře, HBase, Spark) můžete nainstalovat na Azure Hdinsightu pomocí *Skriptu akce*. Ukázka skriptu nainstalovat R HDInsight clusteru je k dispozici jen pro čtení Azure úložiště objektů blob na [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1). 

**Související články**

- [Instalace a používání R v clusterů HDinsight Hadoop (Linux)](hdinsight-hadoop-r-scripts-linux.md)
- [Vytvoření Hadoop clusterů HDInsight](hdinsight-provision-clusters.md): Obecné informace o vytváření HDInsight clusterů
- [Přizpůsobení clusteru HDInsight pomocí skriptu akce][hdinsight-cluster-customize]: Obecné informace o úpravách clusterů HDInsight pomocí skriptu akce
- [Můžete vyvíjet skripty akci skriptu pro HDInsight](hdinsight-hadoop-script-actions.md)

## <a name="what-is-r"></a>Co je R?

<a href="http://www.r-project.org/" target="_blank">R projektu statistické výpočetních</a> je otevřít zdrojového jazyka a prostředí statistické výpočetních. R obsahuje stovky předdefinované statistických funkcí a vlastní programovací jazyk, který kombinuje aspekty funkční a objektově orientovaném programování. Také poskytuje rozsáhlé grafické možnosti. R je upřednostňované programovací prostředí pro většinu profesionální statistiků a vědeckých v celé řadě polí.

R je kompatibilní s úložiště objektů Blob Azure (WASB) tak, že data, která je uložena tam můžete zpracovány pomocí R na HDInsight.  

## <a name="install-r"></a>Instalace R

[Ukázka skriptu](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1) pro instalaci R HDInsight clusteru je k dispozici jen pro čtení objektů blob v úložišti Azure. Tato část obsahuje pokyny k tomu, jak používat ukázkový skript při vytváření obrázku na portálu Azure.

> [AZURE.NOTE] Ukázka skriptu byla zavedená s HDInsight clusteru podverzí 3.1. Další informace o verzích clusteru Hdinsightu najdete v článku [verze obrázku HDInsight](hdinsight-component-versioning.md).

1. Při vytváření HDInsight obrázku z portálu Microsoft klikněte na **Volitelná konfigurace**a pak klikněte na **Akce skriptu**.
2. Na stránce **Akce skriptu** zadejte následující hodnoty:

    ![Použití akce skript k přizpůsobení clusteru] (./media/hdinsight-hadoop-r-scripts/hdi-r-script-action.png "Použití akce skript k přizpůsobení clusteru")

    <table border='1'>
        <tr><th>Vlastnost</th><th>Hodnota</th></tr>
        <tr><td>Jméno</td>
            <td>Zadejte název akce skript, například <b>Instalace R</b>.</td></tr>
        <tr><td>Skript URI</td>
            <td>Zadejte identifikátor URI skript, který je zavolat a přizpůsobení obrázku, například <i>https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1</i></td></tr>
        <tr><td>Typ uzel</td>
            <td>Určete uzly, na kterých běží skript přizpůsobení. Můžete <b>Všechny uzly</b>, <b>pouze záhlaví uzlů</b>nebo <b>pracovní uzly</b> pouze.
        <tr><td>Parametry</td>
            <td>Zadejte parametry, v případě potřeby skriptem. Skript, který chcete nainstalovat R však nevyžaduje všechny parametry, takže to můžou být prázdné.</td></tr>
    </table>

    Můžete přidat víc akci skriptu pro instalaci více součástí na clusteru. Až přidáte skripty, zaškrtněte políčko Spustit crating clusteru.

Pomocí skriptu můžete taky nainstalovat R HDInsight pomocí prostředí PowerShell Azure nebo HDInsight .NET SDK. Pokyny pro tyto postupy jsou uvedeny dál v tomto článku.

## <a name="run-r-scripts"></a>Spouštěly skripty R
Tato část popisuje, jak spustit skript R clusteru Hadoop s HDInsight.

1. **Vytvoření připojení ke vzdálené ploše clusteru**: Z portálu povolit připojení ke vzdálené ploše pro clusteru jste vytvořili pomocí R nainstalovaný a připojte se k němu. Pokyny najdete v tématu [připojení clusterů HDInsight pomocí RDP](hdinsight-administer-use-management-portal.md#rdp).

2. **Spusťte konzolu R**: instalace R umístí odkazu ke konzole R na ploše hlavního uzlu. Klikněte na něj a otevřete konzole R.

3. **Spustit skript R**: R skript lze spustit přímo z konzoly R vložením, že ho vyberete a stisknutím klávesy ENTER. Tady je jednoduchý příklad skript, který vytváří čísla 1 až 100 a pak vynásobí číslem 2.

        library(rmr2)
        library(rhdfs)
        ints = to.dfs(1:100)
        calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))
        from.dfs(calc)

První dva řádky volání RHadoop knihoven, které jsou instalovány spolu R. Poslední řádek vytiskne výsledky konzole. Výstup by měl vypadat takto:

    [1,]  1 2
    [2,]  2 4
    .
    .
    .
    [98,]  98 196
    [99,]  99 198
    [100,] 100 200


## <a name="install-r-using-aure-powershell"></a>Instalace pomocí prostředí Aure PowerShell R

V tématu [přizpůsobení HDInsight clusterů pomocí skriptu akce](hdinsight-hadoop-customize-cluster.md#call_scripts_using_powershell).  Vzorku ukazuje, jak nainstalovat Spark pomocí Powershellu Azure. Budete muset přizpůsobit skript, který chcete použít [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1).

## <a name="install-r-using-net-sdk"></a>Instalace R pomocí .NET SDK

V tématu [přizpůsobení HDInsight clusterů pomocí skriptu akce](hdinsight-hadoop-customize-cluster.md#call_scripts_using_azure_powershell). Vzorku ukazuje, jak nainstalovat Spark pomocí .NET SDK. Budete muset přizpůsobit skript, který chcete použít [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps11).


## <a name="see-also"></a>Viz taky

- [Instalace a používání R v clusterů HDinsight Hadoop (Linux)](hdinsight-hadoop-r-scripts-linux.md)
- [Vytvoření Hadoop clusterů HDInsight](hdinsight-provision-clusters.md): Obecné informace o vytváření HDInsight clusterů
- [Přizpůsobení clusteru HDInsight pomocí skriptu akce][hdinsight-cluster-customize]: Obecné informace o úpravách clusterů HDInsight pomocí skriptu akce
- [Můžete vyvíjet skripty akci skriptu pro HDInsight](hdinsight-hadoop-script-actions.md)
- [Instalace a používání Spark v HDInsight clusterů][hdinsight-install-spark]: Ukázka skriptu akci o instalaci Spark
- [Instalace Giraph na HDInsight clusterů](hdinsight-hadoop-giraph-install.md): Ukázka skriptu akci o instalaci Giraph
- [Instalace Solr na HDInsight clusterů](hdinsight-hadoop-solr-install-linux.md): Ukázka skriptu akci o instalaci Solr.

[powershell-install-configure]: powershell-install-configure.md
[hdinsight-provision]: ../hdinsight-provision-clusters/
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
[hdinsight-install-spark]: hdinsight-apache-spark-jupyter-spark-sql.md
