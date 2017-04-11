<properties
   pageTitle="MapReduce s Hadoop na HDInsight | Microsoft Azure"
   description="Zjistěte, jak spustit MapReduce úlohy na Hadoop v clusterů HDInsight. Budete spustit operaci počet základní word implementovaná jako Java MapReduce úlohu."
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
   ms.date="08/23/2016"
   ms.author="larryfr"/>

# <a name="use-mapreduce-in-hadoop-on-hdinsight"></a>Použití MapReduce v Hadoop na HDInsight

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

V tomto článku se dozvíte, jak spustit MapReduce úlohy na Hadoop v clusterů HDInsight. Nemůžeme spustit operaci počet základní word implementovaná jako úlohu Java MapReduce.

##<a id="whatis"></a>Co je MapReduce?

Hadoop MapReduce je rámec software pro tvorbu úlohy, které zpracovávají velké objemy dat. Zadávání dat je rozdělit na nezávisle na bloky, které se pak provádí souběžně mezi uzly clusteru. Úlohy MapReduce se skládá ze dvou funkcí:

* **Mapování**: spotřebovává zadávání dat, analyzuje (obvykle se filtrování a řazení) a posílá n-ticové (klíč dvojice)
* **Reduktorem**: spotřebovává záznamů, které mapování a provádí souhrnné operaci, která vytvoří výsledek menší, kombinované z mapování dat

V příkladu základní word počet MapReduce projektu je znázorněn v následujícím diagramu:

![HDI. WordCountDiagram][image-hdi-wordcountdiagram]

Výstup tento úkol je počet kolikrát došlo každého slova v textu, který byl analyzovat.

* Mapování zabírá každý řádek zadávání textu jako vstup a rozdělí na slova. Posílá/hodnotu klíče pár pokaždé, když slovo vyskytuje slova následuje hodnotu 1. Výstup seřazena před odesláním reduktorem.

* Reduktorem Sečte tyto jednotlivé počty u každého slova a posílá dvojice jednoho klíč/hodnota, která obsahuje slovo a součet jeho výskytů.

MapReduce lze provést v různých jazycích. Java je nejběžnější implementace a se používá pro účely ukázky v tomto dokumentu.

### <a name="hadoop-streaming"></a>Hadoop datových proudů

Jazyky nebo rámce, které jsou založeny na Java a virtuálního počítače Java (například Scalding nebo s možností,) můžete spustili přímo jako MapReduce úlohu, podobně jako aplikaci Java. Ostatní, například C# nebo Python nebo samostatné spustitelných, musíte použít Hadoop streamování.

Přenos Hadoop informuje uživatele o s mapování a reduktorem prostřednictvím standardního a STDOUT - mapování a reduktorem číst data řádku směrem ze standardního a výstup zapsat StdOut. Každý řádek číst nebo které mapování a reduktorem musí být ve formátu pár klíč/hodnota odděleny charaacter kartu:

    [key]/t[value]

Další informace najdete v tématu [Přenos Hadoop](http://hadoop.apache.org/docs/r1.2.1/streaming.html).

Příklady použití Hadoop streaming s HDInsight najdete v těchto článcích:

* [Můžete vyvíjet Python MapReduce úlohy](hdinsight-hadoop-streaming-python.md)

##<a id="data"></a>Informace o ukázkových dat

V tomto příkladu ukázková data, použijete poznámkové bloky Leonardo Da Vinci, které jsou k dispozici jako textový dokument v svůj cluster HDInsight.

Ukázková data se ukládají v úložišti objektů Blob Azure HDInsight používá jako výchozí systém souborů Hadoop clusterů. HDInsight přístup k souborům uloženým v úložišti objektů Blob pomocí předponou **wasb** . Například pro přístup k souboru sample.log, použijete tuto syntaxi:

    wasbs:///example/data/gutenberg/davinci.txt

Úložiště objektů Blob Azure je výchozí úložiště pro HDInsight, můžete taky přístup k souboru pomocí **/example/data/gutenberg/davinci.txt**.

> [AZURE.NOTE] V předchozí syntaxi **wasbs: / / /** se používá pro přístup k souborům, které jsou uložené v kontejneru výchozí úložiště pro svůj cluster HDInsight. Pokud jste zadali účty další úložiště až zřízení svůj cluster a chcete získat přístup k souborům uloženým v těchto účtů, můžete zadáním adresy účtu a jeho název kontejneru přístup k datům. Například **wasbs://mycontainer@mystorage.blob.core.windows.net/example/data/gutenberg/davinci.txt**.

##<a id="job"></a>Informace o příklad MapReduce

MapReduce projektu, který je v tomto příkladě použita je umístěn na **wasbs://example/jars/hadoop-mapreduce-examples.jar**a je součástí svůj cluster HDInsight. Tato stránka obsahuje příklad počet Wordu, který se spustí hledání **davinci.txt**.

> [AZURE.NOTE] Ve HDInsight 2.1 umístění souboru je **wasbs:///example/jars/hadoop-examples.jar**.

Pro informaci následuje kód jazyka Java MapReduce úlohy počet Wordu:

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

Pokyny k zápisu MapReduce práce najdete v tématu [vyvíjet MapReduce Java aplikace HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md).

##<a id="run"></a>Spustit MapReduce

HDInsight poběží HiveQL úlohy pomocí různých způsobů. Rozhodnout, jaký způsob je pro vás nejlepší, použijte následující tabulku a pak kliknutím na odkaz informace.

| **Pomocí tohoto příkazu**...                                                    | **.. .a udělejte toto**                                       | .. .with **clusteru operační systém** | .. .from tohoto **klienta operační systém** |
|:-------------------------------------------------------------------|:--------------------------------------------------------|:------------------------------------------|:-----------------------------------------|
| [SSH](hdinsight-hadoop-use-mapreduce-ssh.md)                       | Použití příkazu Hadoop prostřednictvím **SSH**                  | Linux                                     | Linux, Unix, systému Mac OS X nebo systému Windows        |
| [Otočení](hdinsight-hadoop-use-mapreduce-curl.md)                     | Odeslání úlohy vzdáleně pomocí **REST**               | Linux nebo Windows                          | Linux, Unix, systému Mac OS X nebo systému Windows        |
| [Prostředí Windows PowerShell](hdinsight-hadoop-use-mapreduce-powershell.md) | Odeslání úlohy vzdáleně pomocí **Prostředí Windows PowerShell** | Linux nebo Windows                          | Windows                                  |
| [Vzdálená plocha](hdinsight-hadoop-use-mapreduce-remote-desktop)    | Použití příkazu Hadoop prostřednictvím **Vzdálené plochy**       | Windows                                   | Windows                                  |

##<a id="nextsteps"></a>Další kroky

Přestože MapReduce poskytuje výkonné diagnostické možností, může být trochu náročné předlohy. Existuje několik rámy na základě Java, které usnadňují definovat MapReduce aplikací, jakož i technologiemi usnadnění, například Prasátko a podregistru, které poskytují snadný způsob, jak pracovat s daty v HDInsight. Další informace naleznete v následujících článcích:

* [Můžete vyvíjet aplikace Java MapReduce pro HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md)

* [Vývoj Python přenos MapReduce programy pro HDInsight](hdinsight-hadoop-streaming-python.md)

* [Můžete vyvíjet napařování MapReduce úlohy s Apache Hadoop na HDInsight](hdinsight-hadoop-mapreduce-scalding.md)

* [Použití podregistru s HDInsight][hdinsight-use-hive]

* [Použití Prasátko s HDInsight][hdinsight-use-pig]

* [Spuštění ukázek HDInsight][hdinsight-samples]


[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-develop-mapreduce-jobs]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-samples]: hdinsight-run-samples.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[powershell-install-configure]: ../powershell-install-configure.md

[image-hdi-wordcountdiagram]: ./media/hdinsight-use-mapreduce/HDI.WordCountDiagram.gif
