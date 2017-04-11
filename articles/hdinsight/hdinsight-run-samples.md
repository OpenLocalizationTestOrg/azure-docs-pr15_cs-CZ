<properties
    pageTitle="Spuštění ukázek Hadoop v HDInsight | Microsoft Azure"
    description="Začínáte používat služby Azure HDInsight se uvedené. Pomocí skriptů Powershellu, spuštěné v operačním systému dat clusterů MapReduce programy."
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
    ms.date="10/21/2016"
    ms.author="jgao"/>

#<a name="run-hadoop-mapreduce-samples-in-windows-based-hdinsight"></a>Spuštění Hadoop MapReduce ukázek v systému Windows HDInsight

[AZURE.INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

Sada vzorků jsou k dispozici které vám pomohou zahájené pracovního MapReduce práce na Hadoop clusterů pomocí Azure HDInsight. Tyto příklady jsou k dispozici ve všech HDInsight spravovaných clusterů, které vytvoříte. Spuštění tyto ukázek se seznamte s pomocí rutin prostředí PowerShell Azure spustit úlohy na Hadoop clusterů.

- [**Word počet**][hdinsight-sample-wordcount]: Spočítá počet výskytů slova z textového souboru.
- [**C# streamování slov**][hdinsight-sample-csharp-streaming]: Vrátí počet výskytů slova z textového souboru pomocí rozhraní streamování Hadoop.
- [**Odhad pí**][hdinsight-sample-pi-estimator]: používá statistického (kvazi-Monte Carlo) způsob, jak odhadnout hodnotu čísla pí.
- [**10 GB Graysort**][hdinsight-sample-10gb-graysort]: Spusťte univerzální GraySort 10 GB soubor pomocí HDInsight. Existují tři úlohy na spustit: Teragen Generovat data, Terasort seřadíte data a Teravalidate potvrďte správně řadit data.

>[AZURE.NOTE] Zdrojový kód naleznete v dodatku. 

Nastavte, jaké další si přečtěte následující dokumentaci existuje na webu technologií související Hadoop, například na základě Java MapReduce programování a datových proudů a dokumentace o rutinách, které se používají v prostředí Windows PowerShell skriptování. Další informace o tyto materiály najdete tady:

- [Můžete vyvíjet Java MapReduce programy pro Hadoop v HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md)
- [Odeslat Hadoop úlohy v HDInsight](hdinsight-submit-hadoop-jobs-programmatically.md)
- [Úvod k Azure HDInsight][hdinsight-introduction]

V současné době Hodně lidí zvolte podregistru a Prasátko MapReduce.  Další informace najdete v tématu:

- [Použití podregistru v HDInsight](hdinsight-use-hive.md)
- [Použití Prasátko v HDInsight](hdinsight-use-pig.md)
 
**Požadavky**:

- **Azure předplatného**. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **HDInsight obrázku**. Na různé způsoby, ve kterých lze vytvořit takové clusterů najdete v článku [Vytvoření Hadoop clusterů HDInsight](hdinsight-provision-clusters.md).
- **Pracoviště se Azure Powershellu**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

## <a name="hdinsight-sample-wordcount"></a>Word počet - Java 

Odeslání MapReduce projektu, nejprve vytvořit definici MapReduce úlohy. V definici úlohy zadejte MapReduce program sklenice souborů a umístění souboru sklenice, což je **wasbs:///example/jars/hadoop-mapreduce-examples.jar**, název třídy a argumenty.  Wordcount MapReduce program se zadávají dva argumenty: zdrojového souboru, která se bude používat pro zjištění počtu slov a umístění výstupu.

Zdrojový kód naleznete v [dodatku A](#apendix-a---the-word-count-MapReduce-program-in-java).

Postup vývoje Java MapReduce programu, najdete v článku - [vyvíjet MapReduce Java programy pro Hadoop v HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md)
 
**Odeslání MapReduce úlohy počet aplikace word**

1. Otevřete **ISE v prostředí Windows PowerShell**. Pokyny najdete v tématu [instalace a konfigurace prostředí PowerShell Azure][powershell-install-configure].
2. Vložte tento skript Powershellu:

        $subscriptionName = "<Azure Subscription Name>"
        $resourceGroupName = "<Resource Group Name>"
        $clusterName = "<HDInsight cluster name>"             # HDInsight cluster name
        
        Select-AzureRmSubscription -SubscriptionName $subscriptionName
        
        # Define the MapReduce job
        $mrJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
                                    -JarFile "wasbs:///example/jars/hadoop-mapreduce-examples.jar" `
                                    -ClassName "wordcount" `
                                    -Arguments "wasbs:///example/data/gutenberg/davinci.txt", "wasbs:///example/data/WordCountOutput1"
        
        # Submit the job and wait for job completion
        $cred = Get-Credential -Message "Enter the HDInsight cluster HTTP user credential:" 
        $mrJob = Start-AzureRmHDInsightJob `
                            -ResourceGroupName $resourceGroupName `
                            -ClusterName $clusterName `
                            -HttpCredential $cred `
                            -JobDefinition $mrJobDefinition 
        
        Wait-AzureRmHDInsightJob `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $clusterName `
            -HttpCredential $cred `
            -JobId $mrJob.JobId 
        
        # Get the job output
        $cluster = Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName
        $defaultStorageAccount = $cluster.DefaultStorageAccount -replace '.blob.core.windows.net'
        $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccount)[0].Value
        $defaultStorageContainer = $cluster.DefaultStorageContainer
        
        Get-AzureRmHDInsightJobOutput `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $clusterName `
            -HttpCredential $cred `
            -DefaultStorageAccountName $defaultStorageAccount `
            -DefaultStorageAccountKey $defaultStorageAccountKey `
            -DefaultContainer $defaultStorageContainer  `
            -JobId $mrJob.JobId `
            -DisplayOutputType StandardError

        # Download the job output to the workstation
        $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccount -StorageAccountKey $defaultStorageAccountKey 
        Get-AzureStorageBlobContent -Container $defaultStorageContainer -Blob example/data/WordCountOutput/part-r-00000 -Context $storageContext -Force
        
        # Display the output file
        cat ./example/data/WordCountOutput/part-r-00000 | findstr "there"

    Úlohy MapReduce vytvoří do souboru nazvaného *část r-00000*, který obsahuje slova a počty. Skript používá příkaz **findstr** zobrazíte seznam všech slova, která obsahuje *"tam"*.

3. Nastavit první 3 proměnné a následujícím způsobem.

## <a name="hdinsight-sample-csharp-streaming"></a>Počet - C# datových proudů ve Wordu

Hadoop poskytuje streamování rozhraní API MapReduce, která umožňuje psaní mapy a zmenšení funkce v jazyce než Java.

> [AZURE.NOTE] Postup v tomto kurzu platí pouze pro clusterů serveru s Windows HDInsight. Příklad datových proudů na základě Linux HDInsight clusterů naleznete v tématu [vyvíjet Python streamování programů pro HDInsight](hdinsight-hadoop-streaming-python.md).

V příkladu mapování a reduktorem představují programy, které číst vstup ze [standardního] [ stdin-stdout-stderr] (řádek po řádku) a posílat výstup [stdout][stdin-stdout-stderr]. Program spočítá všechna slova v textu.

Pokud není zadán spustitelný soubor pro **mappers**, každý úkol mapování se spustí spustitelný soubor jako samostatný proces Pokud inicializován mapování. Při spuštění mapování úkolů, převede vstupní řádky a kanálů řádky ke [standardního] [ stdin-stdout-stderr] procesu.

Mapování mezitím shromažďuje výstupu řádkový z stdout procesu. Převede jednotlivé řádky na pár klíče/hodnoty, které se shromažďují jako výstup mapování. Ve výchozím nastavení předpona řádek nahoru první znak tabulátoru je klávesu a zbytek řádku (s výjimkou znak tabulátoru) je hodnota. Pokud se žádný znak tabulátoru v řádku celý řádek je považován za klávesu a hodnotu null.

Pokud není zadán spustitelný soubor pro **přechodky**, každý úkol reduktorem se spustí spustitelný soubor jako samostatný proces Pokud inicializován reduktorem. Při spuštění úlohy reduktorem, převede jeho vstupní hodnoty klíče/dvojice do řádků a kanálů řádky ke [standardního] [ stdin-stdout-stderr] procesu.

Mezitím reduktorem shromažďuje výstupu řádkový z [stdout] [ stdin-stdout-stderr] procesu. Převede jednotlivé řádky na pár klíče/hodnoty, které se shromažďují jako výstup reduktorem. Ve výchozím nastavení předpona řádek nahoru první znak tabulátoru je klávesu a zbytek řádku (s výjimkou znak tabulátoru) je hodnota.

Další informace o rozhraní Hadoop streamování najdete v článku [Hadoop datových proudů] [hadoop streamování].

**Odeslání C# streamování úlohy počet aplikace word**

- Postup podle [počtu slov - Java](#word-count-java)a nahraďte definici úlohy takto:

        $mrJobDefinition = New-AzureRmHDInsightStreamingMapReduceJobDefinition `
                                -Files "/example/apps/cat.exe","/example/apps/wc.exe" `
                                -Mapper "cat.exe" `
                                -Reducer "wc.exe" `
                                -InputPath "/example/data/gutenberg/davinci.txt" `
                                -OutputPath "/example/data/StreamingOutput/wc.txt"  


    Musí být ve výstupním souboru:
    
        example/data/StreamingOutput/wc.txt/part-00000      
                                
## <a name="hdinsight-sample-pi-estimator"></a>Odhad PÍ

Odhad pí používá statistického (kvazi-Monte Carlo) způsob, jak odhadnout hodnotu čísla pí. Ukazatele umístí náhodně uvnitř celku čtvercovou taky spadají do kruhu v rámci tohoto hranatých označeny pravděpodobnost odpovídající oblasti kruh, pí/4. Z hodnoty 4R, kde R představuje poměr hodnot počet bodů, které jsou uvnitř na kroužek celkový počet bodů, které jsou v rámci druhou mocninu můžete odhadovanou hodnotu čísla pí. Je větší ukázku bodů používá, tím lepší odhad.

Skript uvedené v tomto příkladu odešle úlohu sklenice Hadoop a je nastavena až spustit s hodnotou 16 mapy, přičemž každý z nich je potřeba k výpočtu 10 milion vzorku bodů hodnoty parametrů. Tyto hodnoty parametrů lze změnit zlepšit odhadovaná hodnota konstanty pí. Pro informaci jsou 3.1415926535 prvních 10 desetinná čísla pí.

**Odeslání úlohy odhad pí**

- Postup podle [počtu slov - Java](#word-count-java)a nahraďte definici úlohy takto:

        $mrJobJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
                                    -JarFile "wasbs:///example/jars/hadoop-mapreduce-examples.jar" `
                                    -ClassName "pi" `
                                    -Arguments "16", "10000000"

## <a name="hdinsight-sample-10gb-graysort"></a>10 GB Graysort

V tomto příkladu menší 10GB dat, takže ji můžete spustit relativně rychle. Použije aplikace MapReduce vyvinutý Owen O'Malley a Arun Murthy získané roční srovnávacích řazení terabajt univerzální ("daytona") v roce 2009 s rychlostí 0.578 TB/min (100 TB 173 minut). Další informace o těchto i jiných řazení ukazatele najdete v článku [Sortbenchmark](http://sortbenchmark.org/) webu.

Tento příklad používá třemi sady MapReduce programů:

1. **TeraGen** je MapReduce program, který můžete použít ke generování řádků dat k seřazení.
2. **TeraSort** vzorky vstupních dat a používá MapReduce seřadit data do celkového pořadí. TeraSort je standardní druh MapReduce funkcí, s výjimkou vlastní rozdělovač používající seřazené seznam zkratek N-1 vzorky, které definují klíčové oblasti pro každou snížit. Zejména všechny zkratky takové, které přehrajte [i-1] < = klíč < ukázkové [i] se odesílají zmenšit i. To záruky výstupy snížit i jsou všechny menší než výstup snížit i + 1.
3. **TeraValidate** je MapReduce program, který ověří, že výstup globálně seřazené. Vytvoří jedno mapování na jeden soubor v adresáři výstup a každé mapování zajišťuje, aby každý klíč menší nebo rovna na předchozí snímek. Funkce mapy taky generuje záznamy o první a poslední klíčích jednotlivých souborů a funkci zmenšení zajistí klávesu první souboru i větší než poslední klíč i-1 soubor. Problémy se hlášené jako výstup snížit pomocí klávesy, pomocí kterých jsou mimo pořadí.

Vstupní a výstupní formát využívány všechny tři, bude číst a zapíše textové soubory ve správném formátu. Výstup snížit má replikace nastavena na hodnotu 1 místo výchozí 3, protože kontext srovnávacích nevyžaduje replikovat výstupní data k více uzlů.

Tři úkoly požaduje výběru jednotlivých odpovídající sady MapReduce popsané v úvodu:

1. Generování dat pro řazení podle MapReduce úloha **TeraGen** .
2. Řadit data podle MapReduce úloha **TeraSort** .
3. Potvrďte, že data správně seřazené podle úloha **TeraValidate** MapReduce.

**Odeslání úloh**

- Postup podle [počtu slov - Java](#word-count-java)a použijte následující definice úlohy:

    $teragen = nový AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" ` 
   – název třídy "teragen" "-argumenty"-Dmapred.map.tasks=50 ","100000000","/ příklad / / 10 GB-řazení zadávání dat"
    
    $terasort = nový AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" ` 
   – název třídy "terasort" "-argumenty"-Dmapred.map.tasks=50 ","-Dmapred.reduce.tasks=25 ","/ příklad / / 10 GB-řazení zadávání dat","/ příklad / / 10 GB-řazení výstup dat"
    
    $teravalidate = nový AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" ` 
   – název třídy "teravalidate" "-argumenty"-Dmapred.map.tasks=50 ","-Dmapred.reduce.tasks=25 ","/ příklad / / 10 GB-řazení výstup dat","/ Příklad/data/10 GB-řazení – ověření"


##<a name="next-steps"></a>Další kroky 

Tento článek a článků ve všech vzorcích jste se naučili k tomu ukázky součástí clusterů HDInsight pomocí prostředí PowerShell Azure. Podrobné pokyny k používání Prasátko, podregistru a MapReduce s HDInsight najdete v těchto tématech:

* [Začínáme s používáním Hadoop s podregistru v HDInsight pro analýzu použití mobilního telefonu][hdinsight-get-started]
* [Použití Prasátko s Hadoop na HDInsight][hdinsight-use-pig]
* [Použití podregistru s Hadoop na HDInsight][hdinsight-use-hive]
* [Odeslat Hadoop úlohy v HDInsight] [hdinsight-submit-jobs]
* [Následující dokumentaci pro Azure HDInsight SDK][hdinsight-sdk-documentation]
* [Ladění Hadoop v HDInsight: chybové zprávy] [hdinsight-errors]


## <a name="appendix-a---the-word-count-source-code"></a>Dodatek A - Word počet zdrojového kódu

    package org.apache.hadoop.examples;
    import java.io.IOException;
    import java.util.StringTokenizer;
    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.IntWritable;
    import org.apache.hadoop.io.Text;
    import org.apache.hadoop.mapreduce.Job;
    import org.apache.hadoop.mapreduce.Mapper;
    import org.apache.hadoop.mapreduce.Reducer;
    import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
    import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
    import org.apache.hadoop.util.GenericOptionsParser;

    public class WordCount {

    public static class TokenizerMapper
       extends Mapper<Object, Text, Text, IntWritable>{

    private final static IntWritable one = new IntWritable(1);
    private Text word = new Text();

    public void map(Object key, Text value, Context context
                    ) throws IOException, InterruptedException {
      StringTokenizer itr = new StringTokenizer(value.toString());
      while (itr.hasMoreTokens()) {
        word.set(itr.nextToken());
        context.write(word, one);
        }
      }
    }

    public static class IntSumReducer
       extends Reducer<Text,IntWritable,Text,IntWritable> {
    private IntWritable result = new IntWritable();

    public void reduce(Text key, Iterable<IntWritable> values,
                       Context context
                       ) throws IOException, InterruptedException {
      int sum = 0;
      for (IntWritable val : values) {
        sum += val.get();
      }
      result.set(sum);
      context.write(key, result);
      }
    }

    public static void main(String[] args) throws Exception {
    Configuration conf = new Configuration();
    String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
    if (otherArgs.length != 2) {
      System.err.println("Usage: wordcount <in> <out>");
      System.exit(2);
        }
    Job job = new Job(conf, "word count");
    job.setJarByClass(WordCount.class);
    job.setMapperClass(TokenizerMapper.class);
    job.setCombinerClass(IntSumReducer.class);
    job.setReducerClass(IntSumReducer.class);
    job.setOutputKeyClass(Text.class);
    job.setOutputValueClass(IntWritable.class);
    FileInputFormat.addInputPath(job, new Path(otherArgs[0]));
    FileOutputFormat.setOutputPath(job, new Path(otherArgs[1]));
    System.exit(job.waitForCompletion(true) ? 0 : 1);
    }
    }


## <a name="appendix-b---the-word-count-streaming-source-code"></a>Dodatek B – slov streamování zdrojového kódu

MapReduce program používá aplikaci cat.exe jako rozhraní mapování stáhnete text do konzoly a aplikace wc.exe jako rozhraní zmenšení zjistit počet slov, která se streamují z dokumentu. Mapování a reduktorem číst znaky, řádek po řádku, ze standardní vstupní stream (standardního) a zapisovat do standardního výstupu toku (stdout).

    // The source code for the cat.exe (Mapper).

    using System;
    using System.IO;

    namespace cat
    {
        class cat
        {
            static void Main(string[] args)
            {
                if (args.Length > 0)
                {
                    Console.SetIn(new StreamReader(args[0]));
                }

                string line;
                while ((line = Console.ReadLine()) != null)
                {
                    Console.WriteLine(line);
                }
            }
        }
    }



Mapování kód v souboru cat.cs používá [StreamReader] [ streamreader] objekt, který chcete číst znaky příchozí proudu do konzoly zapíše proudu standardní výstupního datového proudu s statické [Console.Writeline] [ console-writeline] metody.


    // The source code for wc.exe (Reducer) is:

    using System;
    using System.IO;
    using System.Linq;

    namespace wc
    {
        class wc
        {
            static void Main(string[] args)
            {
                string line;
                var count = 0;

                if (args.Length > 0){
                    Console.SetIn(new StreamReader(args[0]));
                }

                while ((line = Console.ReadLine()) != null) {
                    count += line.Count(cr => (cr == ' ' || cr == '\n'));
                }
                Console.WriteLine(count);
            }
        }
    }


Kód reduktorem v souboru wc.cs používá [StreamReader] [ streamreader] objekt, který chcete číst znaků z standardní vstupní proudu, který byl výstup cat.exe mapování. Čte znaky se [Console.Writeline] [ console-writeline] metody, spočítá slova roven mezery a znaků konec řádku na konci každého slova. Potom zapíše celkem do standardního výstupu proudu s [Console.Writeline] [ console-writeline] metody.





## <a name="appendix-c---the-pi-estimator-source-code"></a>Dodatek C - pí odhad zdrojového kódu

Odhad pí Java kód, který obsahuje mapování a reduktorem funkce je k dispozici pro kontrolu dole. Program mapování vygeneruje zadaný počet bodů náhodně umístěn uvnitř obdélníku jednotky a potom spočítá počet tyto body, které jsou v kruhu. Program reduktorem sečteny body počítá podle mappers a vrátí hodnotu čísla pí z vzorce 4R, kde R představuje poměr hodnot počet bodů počítá uvnitř kruhu celkový počet bodů, které jsou v rámci druhou mocninu.

    /**
    * Licensed to the Apache Software Foundation (ASF) under one
    * or more contributor license agreements. See the NOTICE file
    * distributed with this work for additional information
    * regarding copyright ownership. The ASF licenses this file
    * to you under the Apache License, Version 2.0 (the
    * "License"); you may not use this file except in compliance
    * with the License. You may obtain a copy of the License at
    *
    * http://www.apache.org/licenses/LICENSE-2.0
    *
    * Unless required by applicable law or agreed to in writing, software
    * distributed under the License is distributed on an "AS IS" BASIS,
    * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or   implied.
    * See the License for the specific language governing permissions and
    * limitations under the License.
    */

    package org.apache.hadoop.examples;

    import java.io.IOException;
    import java.math.BigDecimal;
    import java.util.Iterator;

    import org.apache.hadoop.conf.Configured;
    import org.apache.hadoop.fs.FileSystem;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.BooleanWritable;
    import org.apache.hadoop.io.LongWritable;
    import org.apache.hadoop.io.SequenceFile;
    import org.apache.hadoop.io.Writable;
    import org.apache.hadoop.io.WritableComparable;
    import org.apache.hadoop.io.SequenceFile.CompressionType;
    import org.apache.hadoop.mapred.FileInputFormat;
    import org.apache.hadoop.mapred.FileOutputFormat;
    import org.apache.hadoop.mapred.JobClient;
    import org.apache.hadoop.mapred.JobConf;
    import org.apache.hadoop.mapred.MapReduceBase;
    import org.apache.hadoop.mapred.Mapper;
    import org.apache.hadoop.mapred.OutputCollector;
    import org.apache.hadoop.mapred.Reducer;
    import org.apache.hadoop.mapred.Reporter;
    import org.apache.hadoop.mapred.SequenceFileInputFormat;
    import org.apache.hadoop.mapred.SequenceFileOutputFormat;
    import org.apache.hadoop.util.Tool;
    import org.apache.hadoop.util.ToolRunner;


    //A Map-reduce program to estimate the value of Pi
    //using quasi-Monte Carlo method.
    //
    //Mapper:
    //Generate points in a unit square
    //and then count points inside/outside of the inscribed circle of the square.
    //
    //Reducer:
    //Accumulate points inside/outside results from the mappers.
    //Let numTotal = numInside + numOutside.
    //The fraction numInside/numTotal is a rational approximation of
    //the value (Area of the circle)/(Area of the square),
    //where the area of the inscribed circle is Pi/4
    //and the area of unit square is 1.
    //Then, Pi is estimated value to be 4(numInside/numTotal).
    //

    public class PiEstimator extends Configured implements Tool {
    //tmp directory for input/output
    static private final Path TMP_DIR = new Path(
    PiEstimator.class.getSimpleName() + "_TMP_3_141592654");

    //2-dimensional Halton sequence {H(i)},
    //where H(i) is a 2-dimensional point and i >= 1 is the index.
    //Halton sequence is used to generate sample points for Pi estimation.
    private static class HaltonSequence {
    // Bases
    static final int[] P = {2, 3};
    //Maximum number of digits allowed
    static final int[] K = {63, 40};

    private long index;
    private double[] x;
    private double[][] q;
    private int[][] d;

    //Initialize to H(startindex),
    //so the sequence begins with H(startindex+1).
    HaltonSequence(long startindex) {
    index = startindex;
    x = new double[K.length];
    q = new double[K.length][];
    d = new int[K.length][];
    for(int i = 0; i < K.length; i++) {
    q[i] = new double[K[i]];
    d[i] = new int[K[i]];
    }

    for(int i = 0; i < K.length; i++) {
    long k = index;
    x[i] = 0;

    for(int j = 0; j < K[i]; j++) {
    q[i][j] = (j == 0? 1.0: q[i][j-1])/P[i];
    d[i][j] = (int)(k % P[i]);
    k = (k - d[i][j])/P[i];
    x[i] += d[i][j] * q[i][j];
    }
    }
    }

    //Compute next point.
    //Assume the current point is H(index).
    //Compute H(index+1).
    //@return a 2-dimensional point with coordinates in [0,1)^2
    double[] nextPoint() {
    index++;
    for(int i = 0; i < K.length; i++) {
    for(int j = 0; j < K[i]; j++) {
    d[i][j]++;
    x[i] += q[i][j];
    if (d[i][j] < P[i]) {
    break;
    }
    d[i][j] = 0;
    x[i] -= (j == 0? 1.0: q[i][j-1]);
    }
    }
    return x;
    }
    }

    //Mapper class for Pi estimation.
    //Generate points in a unit square and then
    //count points inside/outside of the inscribed circle of the square.
    public static class PiMapper extends MapReduceBase
    implements Mapper<LongWritable, LongWritable, BooleanWritable, LongWritable> {

    //Map method.
    //@param offset samples starting from the (offset+1)th sample.
    //@param size the number of samples for this map
    //@param out output {ture->numInside, false->numOutside}
    //@param reporter
    public void map(LongWritable offset,
    LongWritable size,
    OutputCollector<BooleanWritable, LongWritable> out,
    Reporter reporter) throws IOException {

    final HaltonSequence haltonsequence = new HaltonSequence(offset.get());
    long numInside = 0L;
    long numOutside = 0L;

    for(long i = 0; i < size.get(); ) {
    //generate points in a unit square
    final double[] point = haltonsequence.nextPoint();

    //count points inside/outside of the inscribed circle of the square
    final double x = point[0] - 0.5;
    final double y = point[1] - 0.5;
    if (x*x + y*y > 0.25) {
    numOutside++;
    } else {
    numInside++;
    }

    //report status
    i++;
    if (i % 1000 == 0) {
    reporter.setStatus("Generated " + i + " samples.");
    }
    }

    //output map results
    out.collect(new BooleanWritable(true), new LongWritable(numInside));
    out.collect(new BooleanWritable(false), new LongWritable(numOutside));
    }
    }


    //Reducer class for Pi estimation.
    //Accumulate points inside/outside results from the mappers.
    public static class PiReducer extends MapReduceBase
    implements Reducer<BooleanWritable, LongWritable, WritableComparable<?>, Writable> {

    private long numInside = 0;
    private long numOutside = 0;
    private JobConf conf; //configuration for accessing the file system

    //Store job configuration.
    @Override
    public void configure(JobConf job) {
    conf = job;
    }


    // Accumulate number of points inside/outside results from the mappers.
    // @param isInside Is the points inside?
    // @param values An iterator to a list of point counts
    // @param output dummy, not used here.
    // @param reporter

    public void reduce(BooleanWritable isInside,
    Iterator<LongWritable> values,
    OutputCollector<WritableComparable<?>, Writable> output,
    Reporter reporter) throws IOException {
    if (isInside.get()) {
    for(; values.hasNext(); numInside += values.next().get());
    } else {
    for(; values.hasNext(); numOutside += values.next().get());
    }
    }

    //Reduce task done, write output to a file.
    @Override
    public void close() throws IOException {
    //write output to a file
    Path outDir = new Path(TMP_DIR, "out");
    Path outFile = new Path(outDir, "reduce-out");
    FileSystem fileSys = FileSystem.get(conf);
    SequenceFile.Writer writer = SequenceFile.createWriter(fileSys, conf,
    outFile, LongWritable.class, LongWritable.class,
    CompressionType.NONE);
    writer.append(new LongWritable(numInside), new LongWritable(numOutside));
    writer.close();
    }
    }

    //Run a map/reduce job for estimating Pi.
    //@return the estimated value of Pi.
    public static BigDecimal estimate(int numMaps, long numPoints, JobConf jobConf
    )
    throws IOException {
    //setup job conf
    jobConf.setJobName(PiEstimator.class.getSimpleName());

    jobConf.setInputFormat(SequenceFileInputFormat.class);

    jobConf.setOutputKeyClass(BooleanWritable.class);
    jobConf.setOutputValueClass(LongWritable.class);
    jobConf.setOutputFormat(SequenceFileOutputFormat.class);

    jobConf.setMapperClass(PiMapper.class);
    jobConf.setNumMapTasks(numMaps);

    jobConf.setReducerClass(PiReducer.class);
    jobConf.setNumReduceTasks(1);

    // turn off speculative execution, because DFS doesn't handle
    // multiple writers to the same file.
    jobConf.setSpeculativeExecution(false);

    //setup input/output directories
    final Path inDir = new Path(TMP_DIR, "in");
    final Path outDir = new Path(TMP_DIR, "out");
    FileInputFormat.setInputPaths(jobConf, inDir);
    FileOutputFormat.setOutputPath(jobConf, outDir);

    final FileSystem fs = FileSystem.get(jobConf);
    if (fs.exists(TMP_DIR)) {
     throw new IOException("Tmp directory " + fs.makeQualified(TMP_DIR)
     + " already exists. Please remove it first.");
     }
     if (!fs.mkdirs(inDir)) {
     throw new IOException("Cannot create input directory " + inDir);
     }

     //generate an input file for each map task
     try {
     for(int i=0; i < numMaps; ++i) {
     final Path file = new Path(inDir, "part"+i);
     final LongWritable offset = new LongWritable(i * numPoints);
     final LongWritable size = new LongWritable(numPoints);
     final SequenceFile.Writer writer = SequenceFile.createWriter(
     fs, jobConf, file,
     LongWritable.class, LongWritable.class, CompressionType.NONE);
     try {
     writer.append(offset, size);
     } finally {
     writer.close();
     }
     System.out.println("Wrote input for Map #"+i);
     }

     //start a map/reduce job
     System.out.println("Starting Job");
     final long startTime = System.currentTimeMillis();
     JobClient.runJob(jobConf);
     final double duration = (System.currentTimeMillis() - startTime)/1000.0;
     System.out.println("Job Finished in " + duration + " seconds");

     //read outputs
     Path inFile = new Path(outDir, "reduce-out");
     LongWritable numInside = new LongWritable();
     LongWritable numOutside = new LongWritable();
     SequenceFile.Reader reader = new SequenceFile.Reader(fs, inFile, jobConf);
     try {
     reader.next(numInside, numOutside);
     } finally {
     reader.close();
     }

     //compute estimated value
     return BigDecimal.valueOf(4).setScale(20)
     .multiply(BigDecimal.valueOf(numInside.get()))
     .divide(BigDecimal.valueOf(numMaps))
     .divide(BigDecimal.valueOf(numPoints));
     } finally {
     fs.delete(TMP_DIR, true);
     }
     }

    //Parse arguments and then runs a map/reduce job.
    //Print output in standard out.
    //@return a non-zero if there is an error. Otherwise, return 0.
     public int run(String[] args) throws Exception {
     if (args.length != 2) {
     System.err.println("Usage: "+getClass().getName()+" <nMaps> <nSamples>");
     ToolRunner.printGenericCommandUsage(System.err);
     return -1;
     }

     final int nMaps = Integer.parseInt(args[0]);
     final long nSamples = Long.parseLong(args[1]);

     System.out.println("Number of Maps = " + nMaps);
     System.out.println("Samples per Map = " + nSamples);

     final JobConf jobConf = new JobConf(getConf(), getClass());
     System.out.println("Estimated value of Pi is "
     + estimate(nMaps, nSamples, jobConf));
     return 0;
     }

     //main method for running it as a stand alone command.
     public static void main(String[] argv) throws Exception {
     System.exit(ToolRunner.run(null, new PiEstimator(), argv));
     }
     }
     
## <a name="appendix-d---the-10gb-graysort-source-code"></a>Dodatek D - 10gb graysort zdrojového kódu

Pro kontrolu v této části jsou uvedeny kód programu TeraSort MapReduce.


    /**
     * Licensed to the Apache Software Foundation (ASF) under one
     * or more contributor license agreements.  See the NOTICE file
     * distributed with this work for additional information
     * regarding copyright ownership.  The ASF licenses this file
     * to you under the Apache License, Version 2.0 (the
     * "License"); you may not use this file except in compliance
     * with the License.  You may obtain a copy of the License at
     *
     *     http://www.apache.org/licenses/LICENSE-2.0
     *
     * Unless required by applicable law or agreed to in writing, software
     * distributed under the License is distributed on an "AS IS" BASIS,
     * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
     * See the License for the specific language governing permissions and
     * limitations under the License.
     */

    package org.apache.hadoop.examples.terasort;

    import java.io.IOException;
    import java.io.PrintStream;
    import java.net.URI;
    import java.util.ArrayList;
    import java.util.List;

    import org.apache.commons.logging.Log;
    import org.apache.commons.logging.LogFactory;
    import org.apache.hadoop.conf.Configured;
    import org.apache.hadoop.filecache.DistributedCache;
    import org.apache.hadoop.fs.FileSystem;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.NullWritable;
    import org.apache.hadoop.io.SequenceFile;
    import org.apache.hadoop.io.Text;
    import org.apache.hadoop.mapred.FileOutputFormat;
    import org.apache.hadoop.mapred.JobClient;
    import org.apache.hadoop.mapred.JobConf;
    import org.apache.hadoop.mapred.Partitioner;
    import org.apache.hadoop.util.Tool;
    import org.apache.hadoop.util.ToolRunner;

    /**
     * Generates the sampled split points, launches the job,
     * and waits for it to finish.
     * <p>
     * To run the program:
     * <b>bin/hadoop jar hadoop-examples-*.jar terasort in-dir out-dir</b>
     */

    public class TeraSort extends Configured implements Tool {
      private static final Log LOG = LogFactory.getLog(TeraSort.class);

      /**
       * A partitioner that splits text keys into roughly equal
       * partitions in a global sorted order.
       */

      static class TotalOrderPartitioner implements Partitioner<Text,Text>{
        private TrieNode trie;
        private Text[] splitPoints;

        /**
         * A generic trie node
         */
        static abstract class TrieNode {
          private int level;
          TrieNode(int level) {
            this.level = level;
          }
          abstract int findPartition(Text key);
          abstract void print(PrintStream strm) throws IOException;
          int getLevel() {
            return level;
          }
        }

        /**
         * An inner trie node that contains 256 children based on the next
         * character.
         */
        static class InnerTrieNode extends TrieNode {
          private TrieNode[] child = new TrieNode[256];

          InnerTrieNode(int level) {
            super(level);
          }
          int findPartition(Text key) {
            int level = getLevel();
            if (key.getLength() <= level) {
              return child[0].findPartition(key);
            }
            return child[key.getBytes()[level]].findPartition(key);
          }
          void setChild(int idx, TrieNode child) {
            this.child[idx] = child;
          }
          void print(PrintStream strm) throws IOException {
            for(int ch=0; ch < 255; ++ch) {
              for(int i = 0; i < 2*getLevel(); ++i) {
                strm.print(' ');
              }
              strm.print(ch);
              strm.println(" ->");
              if (child[ch] != null) {
                child[ch].print(strm);
              }
            }
          }
        }

        /**
         * A leaf trie node that does string compares to figure out where the given
         * key belongs between lower..upper.
         */
        static class LeafTrieNode extends TrieNode {
          int lower;
          int upper;
          Text[] splitPoints;
          LeafTrieNode(int level, Text[] splitPoints, int lower, int upper) {
            super(level);
            this.splitPoints = splitPoints;
            this.lower = lower;
            this.upper = upper;
          }
          int findPartition(Text key) {
            for(int i=lower; i<upper; ++i) {
              if (splitPoints[i].compareTo(key) >= 0) {
                return i;
              }
            }
            return upper;
          }
          void print(PrintStream strm) throws IOException {
            for(int i = 0; i < 2*getLevel(); ++i) {
              strm.print(' ');
            }
            strm.print(lower);
            strm.print(", ");
            strm.println(upper);
          }
        }


        /**
         * Read the cut points from the given sequence file.
         * @param fs the file system
         * @param p the path to read
         * @param job the job config
         * @return the strings to split the partitions on
         * @throws IOException
         */
        private static Text[] readPartitions(FileSystem fs, Path p,
                                             JobConf job) throws IOException {
          SequenceFile.Reader reader = new SequenceFile.Reader(fs, p, job);
          List<Text> parts = new ArrayList<Text>();
          Text key = new Text();
          NullWritable value = NullWritable.get();
          while (reader.next(key, value)) {
            parts.add(key);
            key = new Text();
          }
          reader.close();
          return parts.toArray(new Text[parts.size()]);  
        }

        /**
         * Given a sorted set of cut points, build a trie that will find the correct
         * partition quickly.
         * @param splits the list of cut points
         * @param lower the lower bound of partitions 0..numPartitions-1
         * @param upper the upper bound of partitions 0..numPartitions-1
         * @param prefix the prefix that we have already checked against
         * @param maxDepth the maximum depth we will build a trie for
         * @return the trie node that will divide the splits correctly
         */
        private static TrieNode buildTrie(Text[] splits, int lower, int upper,
                                          Text prefix, int maxDepth) {
          int depth = prefix.getLength();
          if (depth >= maxDepth || lower == upper) {
            return new LeafTrieNode(depth, splits, lower, upper);
          }
          InnerTrieNode result = new InnerTrieNode(depth);
          Text trial = new Text(prefix);
          // append an extra byte on to the prefix
          trial.append(new byte[1], 0, 1);
          int currentBound = lower;
          for(int ch = 0; ch < 255; ++ch) {
            trial.getBytes()[depth] = (byte) (ch + 1);
            lower = currentBound;
            while (currentBound < upper) {
              if (splits[currentBound].compareTo(trial) >= 0) {
                break;
              }
              currentBound += 1;
            }
            trial.getBytes()[depth] = (byte) ch;
            result.child[ch] = buildTrie(splits, lower, currentBound, trial,
                                         maxDepth);
          }
          // pick up the rest
          trial.getBytes()[depth] = 127;
          result.child[255] = buildTrie(splits, currentBound, upper, trial,
                                        maxDepth);
          return result;
        }

        public void configure(JobConf job) {
          try {
            FileSystem fs = FileSystem.getLocal(job);
            Path partFile = new Path(TeraInputFormat.PARTITION_FILENAME);
            splitPoints = readPartitions(fs, partFile, job);
            trie = buildTrie(splitPoints, 0, splitPoints.length, new Text(), 2);
          } catch (IOException ie) {
            throw new IllegalArgumentException("can't read paritions file", ie);
          }
        }

        public TotalOrderPartitioner() {
        }

        public int getPartition(Text key, Text value, int numPartitions) {
          return trie.findPartition(key);
        }

      }

      public int run(String[] args) throws Exception {
        LOG.info("starting");
        JobConf job = (JobConf) getConf();
        Path inputDir = new Path(args[0]);
        inputDir = inputDir.makeQualified(inputDir.getFileSystem(job));
        Path partitionFile = new Path(inputDir, TeraInputFormat.PARTITION_FILENAME);
        URI partitionUri = new URI(partitionFile.toString() +
                                   "#" + TeraInputFormat.PARTITION_FILENAME);
        TeraInputFormat.setInputPaths(job, new Path(args[0]));
        FileOutputFormat.setOutputPath(job, new Path(args[1]));
        job.setJobName("TeraSort");
        job.setJarByClass(TeraSort.class);
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(Text.class);
        job.setInputFormat(TeraInputFormat.class);
        job.setOutputFormat(TeraOutputFormat.class);
        job.setPartitionerClass(TotalOrderPartitioner.class);
        TeraInputFormat.writePartitionFile(job, partitionFile);
        DistributedCache.addCacheFile(partitionUri, job);
        DistributedCache.createSymlink(job);
        job.setInt("dfs.replication", 1);
        TeraOutputFormat.setFinalSync(job, true);
        JobClient.runJob(job);
        LOG.info("done");
        return 0;
      }

      /**
       * @param args
       */

      public static void main(String[] args) throws Exception {
        int res = ToolRunner.run(new JobConf(), new TeraSort(), args);
        System.exit(res);
      }
    }














 




[hdinsight-errors]: hdinsight-debug-jobs.md

[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-introduction]: hdinsight-hadoop-introduction.md


[powershell-install-configure]: ../powershell-install-configure.md

[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[hdinsight-samples]: hdinsight-run-samples.md
[hdinsight-sample-10gb-graysort]: #hdinsight-sample-10gb-graysort
[hdinsight-sample-csharp-streaming]: #hdinsight-sample-csharp-streaming
[hdinsight-sample-pi-estimator]: #hdinsight-sample-pi-estimator
[hdinsight-sample-wordcount]: #hdinsight-sample-wordcount

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md

[streamreader]: http://msdn.microsoft.com/library/system.io.streamreader.aspx
[console-writeline]: http://msdn.microsoft.com/library/system.console.writeline
[stdin-stdout-stderr]: https://msdn.microsoft.com/library/3x292kth.aspx
