<properties
   pageTitle="Twitter trendu témata s Apache bouře na HDInsight | Microsoft Azure"
   description="Naučte se používat Trident vytvořit Apache bouře topologie, která určuje trendu témat na Twitter podle hashtags."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="larryfr"/>

#<a name="determine-twitter-trending-topics-with-apache-storm-on-hdinsight"></a>Určení Twitteru trendu témata s Apache bouře na HDInsight

Naučte se používat Trident vytvořit topologie bouře, která určuje trendu témat (hash značky) na Twitter.

Trident je uceleném odběru, který obsahuje nástroje, například spojení agregace, seskupování, funkce a filtry. Kromě toho Trident přidá základní prvky pro plnění stavové přírůstková zpracování. Tento příklad ukazuje, jak je možné vytvářet topologie pomocí vlastní hubičkou, funkce a několik předdefinované funkce poskytované Trident.

> [AZURE.NOTE] V tomto příkladu je často založeny na příklad [Trident bouře](https://github.com/jalonsoramos/trident-storm) Juan Alonso.

##<a name="requirements"></a>Požadavky

* <a href="http://www.oracle.com/technetwork/java/javase/downloads/index.html" target="_blank">Java a JDK 1.7</a>

* <a href="http://maven.apache.org/what-is-maven.html" target="_blank">Maven</a>

* <a href="http://git-scm.com/" target="_blank">Libovolná</a>

* Karta Vývojář v účtu na Twitteru

##<a name="download-the-project"></a>Stáhněte si projektu

Umožňuje vytvořit kopii projektu místně následující kód.

    git clone https://github.com/Blackmist/TwitterTrending

##<a name="topology"></a>Topologie

Topologie v tomto příkladu je takto:

![topologie](./media/hdinsight-storm-twitter-trending/trident.png)

> [AZURE.NOTE] Toto je zjednodušený topologii. Více instancí součásti budou rozvržena uzlů v clusteru.

Kód Trident pro implementující topologii vypadá takto:

    topology.newStream("spout", spout)
        .each(new Fields("tweet"), new HashtagExtractor(), new Fields("hashtag"))
        .groupBy(new Fields("hashtag"))
        .persistentAggregate(new MemoryMapState.Factory(), new Count(), new Fields("count"))
        .newValuesStream()
        .applyAssembly(new FirstN(10, "count"))
        .each(new Fields("hashtag", "count"), new Debug());

Tento kód dělá toto:

1. Vytvoří novou toku z hubičky. Hubičky načte tweety z Twitteru a filtry pro specifické klíčová slova (láska, Hudba a kávy v tomto příkladu).

2. HashtagExtractor, vlastní funkci slouží k extrahovat hash značky z každé tweetu. Toto jsou emitovány do datového proudu.

3. Proudu seskupené podle značky hash se předávají funkcím agregátor. Tento agregátor vytvoří počtu kolikrát došlo k každou hash značku. Tato data uložena v paměti. Nakonec je nové toku vyzařovaného obsahující značku hash a počet různých hodnot.

4. Protože jsme zajímají pouze nejoblíbenější značky hash pro danou sadu tweety, sestavení **FirstN** se použije pro vrátí jenom 10 nejvyšších hodnot, založené na poli count.

> [AZURE.NOTE] Než hubičky nebo HashtagExtractor používáme předdefinované funkce Trident.
>
> Informace o předdefinovaných operace najdete v tématu <a href="https://storm.apache.org/apidocs/storm/trident/operation/builtin/package-summary.html" target="_blank">storm.trident.operation.builtin balíčku</a>.
>
> Implementace Trident stavu než MemoryMapState najdete v těchto článcích:
>
> * <a href="https://github.com/fhussonnois/storm-trident-elasticsearch" target="_blank">Bouře Trident pružná hledání</a>
>
> * <a href="https://github.com/kstyrc/trident-redis" target="_blank">Trident redis</a>

###<a name="the-spout"></a>Hubičky

Hubičky **TwitterSpout**používá <a href="http://twitter4j.org/en/" target="_blank">Twitter4j</a> k načtení tweety z Twitter. Filtr se vytvoří (láska, Hudba a kávy v tomto příkladu) a příchozí tweety (stav), které odpovídají filtru jsou uloženy v propojených blokování fronty. (Další informace najdete v tématu <a href="http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/LinkedBlockingQueue.html" target="_blank">Třídy LinkedBlockingQueue</a>.) Nakonec položek doplněné vypnutí fronty a jejich vyzařovaného topologie.

###<a name="the-hashtagextractor"></a>HashtagExtractor

Chcete-li extrahovat hash značky, <a href="http://twitter4j.org/javadoc/twitter4j/EntitySupport.html#getHashtagEntities--" target="_blank">getHashtagEntities</a> použitý k načtení všechny hash značky, které jsou obsaženy tweetu. Potom jsou emitovány do datového proudu.

##<a name="enable-twitter"></a>Povolení Twitter

Umožňuje zaregistrovat novou aplikaci Twitteru a získat příjemce a přístup token informace potřebné k číst z Twitteru podle těchto kroků:

1. Přejděte na <a href="https://apps.twitter.com" target="_blank">Twitter aplikace</a> a klikněte na tlačítko **vytvořit novou aplikaci** . Při vyplňování formuláře, nechte do pole **Adresa URL zpětné** prázdné.

2. Pokud je aplikace vytvořená, klikněte na kartu **klíče a tokeny přístupu** .

3. Zkopírujte údaje **Klíč příjemce** a **Tajná příjemce** .

4. V dolní části stránky vyberte **vytvořit přístupový token** existenci žádné tokeny. Po vytvoření tokeny zkopírujte informace o **Přístupu tokenu** a **Tokenu tajná přístup** .

5. V **TwitterSpoutTopology** projektu, který dříve klonovat otevřete soubor **resources/twitter4j.properties** , přidejte informace, které jste shromáždili v předchozích krocích a potom soubor uložte.

##<a name="build-the-topology"></a>Vytvoření topologii

Následující kód použijte k vytvoření projektu:

        cd [directoryname]
        mvn compile

##<a name="test-the-topology"></a>Testování topologii

Pomocí následujícího příkazu otestujte topologie místně:

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.TwitterTrendingTopology

Když začnou topologii, byste měli vidět ladění informace, které obsahuje algoritmus hash značky a spočítá emitovaného tak, že topologii. Výstup by měl vypadat takto:

    DEBUG: [Quicktellervalentine, 7]
    DEBUG: [GRAMMYs, 7]
    DEBUG: [AskSam, 7]
    DEBUG: [poppunk, 1]
    DEBUG: [rock, 1]
    DEBUG: [punkrock, 1]
    DEBUG: [band, 1]
    DEBUG: [punk, 1]
    DEBUG: [indonesiapunkrock, 1]

##<a name="next-steps"></a>Další kroky

Teď testování topologii místně, můžete zjistit, jak pro nasazení topologii: [nasazení a správu Apache bouře topologií na HDInsight](hdinsight-storm-deploy-monitor-topology.md).

Možná by vás zajímaly i v následujících tématech bouře:

* [Můžete vyvíjet Java topologie pro bouře na HDInsight pomocí Maven](hdinsight-storm-develop-java-topology.md)

* [Můžete vyvíjet C# topologie pro bouře na HDInsight pomocí aplikace Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md)

Další příklady bouře pro HDinsight:

* [Příklad topologie pro bouře na HDInsight](hdinsight-storm-example-topology.md)
