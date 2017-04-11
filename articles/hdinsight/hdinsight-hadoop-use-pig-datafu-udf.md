<properties
pageTitle="Použití DataFu s Prasátko na HDInsight"
description="DataFu je sada knihoven pro použití s Hadoop. Zjistěte, jak můžete DataFu s Prasátko clusteru HDInsight."
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
ms.date="08/23/2016"
ms.author="larryfr"/>

#<a name="use-datafu-with-pig-on-hdinsight"></a>Použití DataFu s Prasátko na HDInsight

DataFu najdete knihoven otevřít zdroj pro použití s Hadoop. V tomto dokumentu se dozvíte, jak používat DataFu HDInsight clusteru a jak používat DataFu uživatele definované funkce (UDF) s Prasátko.

##<a name="prerequisites"></a>Zjistit předpoklady pro

* Předplatné Azure.

* Azure HDInsight obrázku (Linux nebo systémem Windows)

* Základní znalost [pomocí Prasátko na HDInsight](hdinsight-use-pig.md)

##<a name="install-datafu-on-linux-based-hdinsight"></a>Instalace DataFu na základě Linux HDInsight

> [AZURE.NOTE] DataFu je nainstalovaná na základě Linux clusterů verze 3.3 nebo novější a na serveru s Windows clusterů. Není nainstalovaný na základě Linux clusterů starší než 3.3.
>
> Pokud používáte verze obrázku na základě Linux 3.3 nebo vyšší nebo obrázku se systémem Windows, můžete tuto část přeskočit.

DataFu můžete stáhnout a nainstalovat z úložiště Maven. Pomocí následujících kroků můžete přidat DataFu HDInsight clusteru:

1. Připojení k clusteru na základě Linux HDInsight pomocí SSH. Další informace o použití SSH s Hdinsightu najdete tyto dokumenty:

    * [Použití SSH s Hadoop Linux založené na HDInsight z Linux, OS X a Unix](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [Použití SSH s Hadoop Linux založené na HDInsight z Windows](hdinsight-hadoop-linux-use-ssh-unix.md)
    
2. Zadejte následující příkaz ke stažení souboru sklenice DataFu pomocí nástroje wget nebo zkopírujte a vložte odkaz do prohlížeče zahájíte stahování.

        wget http://central.maven.org/maven2/com/linkedin/datafu/datafu/1.2.0/datafu-1.2.0.jar

3. Pak nahrajte soubor do výchozí úložiště pro svůj cluster HDInsight. Díky soubor k dispozici pro všechny uzly clusteru a soubor bude ponechat v úložišti bez ohledu na odstraňte a znovu vytvořte clusteru.

        hdfs dfs -put datafu-1.2.0.jar /example/jars
    
    > [AZURE.NOTE] Výše uvedený příklad ukládá sklenice v `wasbs:///example/jars` od tohoto adresáře již existuje v úložišti clusteru. Můžete použít umístění, které chcete na úložiště clusteru HDInsight.

##<a name="use-datafu-with-pig"></a>Použití DataFu s Prasátko

Postup v této části předpokládá znají pomocí Prasátko na HDInsight a jenom poskytnout příslušným Prasátko (latinka), ne postup, jak je používat s clusteru. Další informace o použití Prasátko s Hdinsightu najdete v článku [Použití Prasátko s HDInsight](hdinsight-use-pig.md).

> [AZURE.IMPORTANT] Při použití DataFu z Prasátko na základě Linux HDInsight obrázku, musíte nejdřív zaregistrovat sklenice soubor pomocí následujícího příkazu Prasátko (latinka):
>
> ```register wasbs:///example/jars/datafu-1.2.0.jar```
>
> Ve výchozím nastavení na serveru s Windows HDInsight clusterů je registrovaná DataFu.

Obvykle definujete aliasu pro DataFu funkcí. Příklad:

    DEFINE SHA datafu.pig.hash.SHA();
    
Tato možnost definuje alias pojmenované `SHA` pro SHA algoritmus hash (funkce). Pak to můžete použít ke generování algoritmus hash pro zadávání dat pomocí skriptu Prasátko (latinka). Například následující nahradí názvům v řádcích zadávání dat hodnotu hash:

    raw = LOAD '/data/raw/' USING PigStorage(',') AS  
        (name:chararray, 
        int1:int, 
        int2:int,
        int3:int); 
    mask = FOREACH raw GENERATE SHA(name), int1, int2, int3; 
    DUMP mask;

Pokud používá se s následujícími údaji zadávání:

    Lana Zemljaric,5,9,1
    Qiong Zhong,9,3,6
    Sandor Harsanyi,0,7,3
    Roko Petkovic,2,6,2
    Tibor Rozsa,8,0,0
    Lea Hrastovsek,6,3,6
    Regina Toth,2,1,2
    Eva Makay,8,9,2
    Shi Liao,4,6,0
    Tjasa Zemljaric,0,2,5
    
Vygeneruje následující výstup:

    (c1a743b0f34d349cfc2ce00ef98369bdc3dba1565fec92b4159a9cd5de186347,5,9,1)
    (713d030d621ab69aa3737c8ea37a2c7c724a01cd0657a370e103d8cdecac6f99,9,3,6)
    (7a5f5abdd281f68168199319d98a1a662535f988d1443b3a3c497010937bac89,0,7,3)
    (a94818e93807e12079c4b35f8f3c8c8ef8e8acd1954e7f0476bc1a3a86fc96a9,2,6,2)
    (894ead4f48af91df7e088241218a23157bede7c52115272e417e95c046d48902,8,0,0)
    (6f99f163af3448fda672087db306f363e27a98a9e49c1f274a0860e303f8aec4,6,3,6)
    (a03de92a28be3c6a984c7a153fa9ed81c0413f76a9401955b5f7e04a5dd0ab9f,2,1,2)
    (6ceab977c8fb48d9ad0dc413e6bc646cabd89f22e7ab97a6b0133f3d225c6013,8,9,2)
    (fa9c436469096ff1bd297e182831f460501b826272ae97e921f5f6e3f54747e8,4,6,0)
    (bc22db7c238b86c37af79a62c78f61a304b35143f6087eb99c34040325865654,0,2,5)

##<a name="next-steps"></a>Další kroky

Další informace o DataFu nebo Prasátko najdete v tématu tyto dokumenty:

* [Příručka DataFu Prasátko Apache](http://datafu.incubator.apache.org/docs/datafu/guide.html).

* [Použití Prasátko s HDInsight](hdinsight-use-pig.md)
