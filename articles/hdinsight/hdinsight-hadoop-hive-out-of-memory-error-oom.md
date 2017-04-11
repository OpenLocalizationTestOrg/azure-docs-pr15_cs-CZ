<properties
    pageTitle="Odhlášení z paměti chyby (OOM) - podregistru nastavení | Microsoft Azure"
    description="Opravte nedostatku paměti (OOM) z dotazu podregistru v Hadoop v HDInsight. Scénář zákazníkům je dotaz přes hodně velkým tabulkám."
    keywords="odhlášení z nastavení podregistru chyby, OOM, paměti"
    services="hdinsight"
    documentationCenter=""
    authors="rashimg"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/02/2016"
    ms.author="rashimg;jgao"/>

# <a name="fix-an-out-of-memory-oom-error-with-hive-memory-settings-in-hadoop-in-azure-hdinsight"></a>Opravit chybu mimo paměti (OOM) s nastavením paměti podregistru v Hadoop v Azure HDInsight

Jedním z běžných problémů naši zákazníci nominální budete posílat chybu mimo paměti (OOM) při použití podregistru. Tento článek popisuje scénáři zákazníka a podregistru provedené doporučujeme vyřešit problém.

## <a name="scenario-hive-query-across-large-tables"></a>Scénář: Podregistru dotazu přes rozsáhlých tabulek

Zákazník spuštění dotazu pod pomocí podregistru.

    SELECT
        COUNT (T1.COLUMN1) as DisplayColumn1,
        …
        …
        ….
    FROM
        TABLE1 T1,
        TABLE2 T2,
        TABLE3 T3,
        TABLE5 T4,
        TABLE6 T5,
        TABLE7 T6
    where (T1.KEY1 = T2.KEY1….
        …
        …

Některé detailů tento dotaz:

* T1 je alias velký tabulky Tabulka1, která nabízí spousty typů sloupce řetězec.
* Ostatní tabulky není, velký, ale máte velký počet sloupců.
* Všechny tabulky jsou připojením se k sobě, v některých případech s víc sloupců tabulky TABLE1 a další.

Při zákazníka spuštění dotazu pomocí podregistru na MapReduce 24 uzlu A3 clusteru, spuštění dotazu v asi 26 minut. Zákazník možné je zaznamenat následující upozornění při byl spuštění dotazu pomocí podregistru na MapReduce:

    Warning: Map Join MAPJOIN[428][bigTable=?] in task 'Stage-21:MAPRED' is a cross product
    Warning: Shuffle Join JOIN[8][tables = [t1933775, t1932766]] in Stage 'Stage-4:MAPRED' is a cross product

Protože dotaz dokončení spouštění v asi 26 minut zákazníka ignorovány tato upozornění a místo toho spuštění za účelem zaměření se na tom, jak zlepšit tento další výkonu dotazu.

Zákazník projednání [Optimalizace podregistru dotazy na Hadoop v HDInsight](hdinsight-hadoop-optimize-hive-query.md)a rozhodli použít Tez spuštění prostředky. Po spuštění stejného dotazu s povoleným nastavením Tez dotaz spustili 15 minut a vyvolal tato chyba:

    Status: Failed
    Vertex failed, vertexName=Map 5, vertexId=vertex_1443634917922_0008_1_05, diagnostics=[Task failed, taskId=task_1443634917922_0008_1_05_000006, diagnostics=[TaskAttempt 0 failed, info=[Error: Failure while running task:java.lang.RuntimeException: java.lang.OutOfMemoryError: Java heap space
        at
    org.apache.hadoop.hive.ql.exec.tez.TezProcessor.initializeAndRunProcessor(TezProcessor.java:172)
        at org.apache.hadoop.hive.ql.exec.tez.TezProcessor.run(TezProcessor.java:138)
        at
    org.apache.tez.runtime.LogicalIOProcessorRuntimeTask.run(LogicalIOProcessorRuntimeTask.java:324)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable$1.run(TezTaskRunner.java:176)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable$1.run(TezTaskRunner.java:168)
        at java.security.AccessController.doPrivileged(Native Method)
        at javax.security.auth.Subject.doAs(Subject.java:415)
        at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1628)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable.call(TezTaskRunner.java:168)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable.call(TezTaskRunner.java:163)
        at java.util.concurrent.FutureTask.run(FutureTask.java:262)
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
        at java.lang.Thread.run(Thread.java:745)
    Caused by: java.lang.OutOfMemoryError: Java heap space

Zákazník pak rozhodli použít zvětšit OM (tedy D12) mysli větší OM budou mít víc místa haldy. Zákazníka i nadále zobrazí chyba. Zákazník dosažení týmu HDInsight o pomoc se ladění tento problém.

## <a name="debug-the-out-of-memory-oom-error"></a>Ladění chyb mimo paměti (OOM)

Náš podporu a technickým týmům společně zjistil jeden problémy způsobující chyby mimo paměti (OOM) byla [známé popsané v Apache JIRA](https://issues.apache.org/jira/browse/HIVE-8306). Z popisu v JIRA:

    When hive.auto.convert.join.noconditionaltask = true we check noconditionaltask.size and if the sum  of tables sizes in the map join is less than noconditionaltask.size the plan would generate a Map join, the issue with this is that the calculation doesnt take into account the overhead introduced by different HashTable implementation as results if the sum of input sizes is smaller than the noconditionaltask size by a small margin queries will hit OOM.

Potvrdili, že **hive.auto.convert.join.noconditionaltask** byl skutečně nastavit na **hodnotu true** pod podregistru site.xml soubor:

    <property>
        <name>hive.auto.convert.join.noconditionaltask</name>
        <value>true</value>
        <description>
            Whether Hive enables the optimization about converting common join into mapjoin based on the input file size.
            If this parameter is on, and the sum of size for n-1 of the tables/partitions for a n-way join is smaller than the
            specified size, the join is directly converted to a mapjoin (there is no conditional task).
        </description>
    </property>

Na základě upozornění a JIRA, byla náš hypotéz že mapu připojení byl příčinu Java haldy místo OOM chyby. Proto doporučujeme dug hlouběji do tento problém.

Způsobem popsaným v příspěvku na blogu [Hadoop vláken nastavení paměti v HDInsight](http://blogs.msdn.com/b/shanyu/archive/2014/07/31/hadoop-yarn-memory-settings-in-hdinsigh.aspx), kdy Tez spuštění modul je použít haldy místo, které používají patří ve skutečnosti kontejneru Tez. Viz obrázek pod popisující paměti Tez kontejner.

![Diagram paměti tez kontejner: podregistru mimo Chyba paměti OOM](./media/hdinsight-hadoop-hive-out-of-memory-error-oom/hive-out-of-memory-error-oom-tez-container-memory.png)


Jako příspěvku na blogu o tom, následující dvě paměti nastavení definovat paměti kontejneru pro haldu: **hive.tez.container.size** a **hive.tez.java.opts**. Z naše zkušeností výjimce OOM neznamená, že velikost kontejneru je příliš malá. Znamená to, že je příliš malá velikost haldy Java (hive.tez.java.opts). To pokaždé, když se zobrazí OOM, můžete zkusit zvýšit **hive.tez.java.opts**. V případě potřeby bude pravděpodobně nutné zvětšit **hive.tez.container.size**. Nastavení **java.opts** by měly tvořit přibližně 80 % **container.size**.

> [AZURE.NOTE]  Nastavení **hive.tez.java.opts** musí být vždy menší než **hive.tez.container.size**.

Od počítače D12 má 28GB paměti, jsme se rozhodli použít velikost kontejneru 10 GB (10240MB) a přiřadit java.opts 80 %. To byla v konzole podregistru pomocí následující nastavení:

    SET hive.tez.container.size=10240
    SET hive.tez.java.opts=-Xmx8192m

Na základě tohoto nastavení, dotaz úspěšně spuštěna za v části deset minut.

## <a name="conclusion-oom-errors-and-container-size"></a>Uzavření: OOM chyby a velikost kontejneru

Chybová OOM nemá znamenat, že velikost kontejneru je moc nízké. Místo toho měli byste nakonfigurovat nastavení paměti tak, aby velikost haldy zvýšen a je nejméně 80 % velikosti paměti kontejner.
