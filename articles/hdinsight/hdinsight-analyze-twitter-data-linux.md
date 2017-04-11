<properties
    pageTitle="Analýza dat Twitter pomocí Apache podregistru na HDInsight | Microsoft Azure"
    description="Zjistěte, jak používat k ukládání Tweety, které obsahují určité klíčová slova a potom pomocí podregistru a Hadoop na HDInsight změní původní data TWitter vyhledávání tabulku podregistru Python."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="larryfr"/>

# <a name="analyze-twitter-data-using-hive-in-hdinsight"></a>Analýza dat Twitter pomocí podregistru v HDInsight

V tomto dokumentu tweety pošle pomocí Twitter streamování rozhraní API a potom použijte Apache podregistru na základě Linux HDInsight obrázku do obrázku ve formátu JSON formátovaná data. Výsledek bude seznam Twitteru uživatelů, kterým odeslaná většina tweety obsahující určité slovo.

> [AZURE.NOTE] Zatímco lze použít jednotlivé tohoto dokumentu se systémem Windows HDInsight clusterů (Python například), několika krocích vycházejí pomocí obrázku na základě Linux HDInsight. Postup konkrétní clusteru serveru s Windows najdete v tématech [Twitter analýza dat pomocí podregistru v HDInsight](hdinsight-analyze-twitter-data.md).

###<a name="prerequisites"></a>Zjistit předpoklady pro

Před zahájením tohoto kurzu, musíte mít takto:

- __Na základě Linux Azure HDInsight obrázku__. Informace o vytváření clusteru najdete v tématu [Začínáme s bázi Linux HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) postup vytvoření clusteru.

- __SSH klienta__. Další informace o použití SSH s na základě Linux Hdinsightu najdete v následujících článcích:

    * [Použití SSH s Hadoop Linux založené na HDInsight z Linux, Unix nebo OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Použití SSH s Hadoop Linux založené na HDInsight z Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

- __Python__ a [pip](https://pypi.python.org/pypi/pip)

##<a name="get-a-twitter-feed"></a>Získání Twitter informačního kanálu

Twitter umožňuje načítat [data pro každou tweetu](https://dev.twitter.com/docs/platform-objects/tweets) jako dokument JavaScript Object Notation (JSON) prostřednictvím rozhraní REST API. [OAuth](http://oauth.net) je nutný pro ověřování k rozhraní API. Musíte taky vytvořit _Twitter aplikace_ , která obsahuje nastavení pro přístup k rozhraní API.

###<a name="create-a-twitter-application"></a>Vytvoření aplikace Twitter

1. Z webového prohlížeče Přihlaste se k [https://apps.twitter.com/](https://apps.twitter.com/). Pokud nemáte účet Twitter, klikněte na odkaz **Přihlásit se** .
2. Klikněte na **vytvořit novou aplikaci**.
3. Zadejte **název**a **Popis** **webu**. Můžete mít adresu URL pro pole **Web** . V následující tabulce jsou uvedeny některé ukázkové hodnoty můžete:

  	| Pole | Hodnota |
  	|:----- |:----- |
  	| Jméno  | MyHDInsightApp |
  	| Popis | MyHDInsightApp |
  	| Web | http://www.myhdinsightapp.com |
    
4. Kontrola **souhlasím**a klikněte na **vytvořit aplikace Twitter**.
5. Klikněte na kartu **oprávnění** . Výchozí oprávnění je **jen pro čtení**. To je dostatečné u tohoto kurzu.
6. Klikněte na kartu **klíče a tokeny přístupu** .
7. Klikněte na **vytvořit přístupový token**.
8. V pravém horním rohu stránky klepněte na tlačítko **Testovat OAuth** .
9. Poznamenejte si **klíč příjemce**, **příjemce tajná**, **přístupový token**a **tokenu tajná přístup**. Byste potřebovali hodnoty později.

>[AZURE.NOTE] Při použití příkazu otočení v systému Windows pro hodnoty možnost použijte dvojité uvozovky místo apostrofy.

###<a name="download-tweets"></a>Stáhněte si tweety

Následující kód Python 10 000 tweety stahování Twitteru a uložit je do souboru nazvaného __tweets.txt__.

> [AZURE.NOTE] Podle těchto kroků provádí HDInsight clusteru, protože je už nainstalovaná Python.

1. Připojení k clusteru HDInsight pomocí SSH:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
        
    Pokud jste použili heslo zabezpečení SSH uživatelský účet, zobrazí se výzva k zadání ho. Pokud jste použili veřejný klíč, bude pravděpodobně nutné použít `-i` parametr určuje odpovídající privátním klíčem. Například `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.
        
    Další informace o použití SSH s bázi Linux Hdinsightu najdete v následujících článcích:
    
    * [Použití SSH s Hadoop Linux založené na HDInsight z Linux, Unix nebo OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Použití SSH s Hadoop Linux založené na HDInsight z Windows](hdinsight-hadoop-linux-use-ssh-windows)
    
2. Ve výchozím nastavení není nainstalovaný nástroj __pip__ na uzel hlavy HDInsight. Použijte následující nainstalovat a pak aktualizujte tento nástroj:

        sudo apt-get install python-pip
        sudo pip install --upgrade pip

3. Kód ke stažení tweety využívá [Tweepy](http://www.tweepy.org/) a [Progressbar](https://pypi.python.org/pypi/progressbar/2.2). Pokud chcete nainstalovat tyto, zadejte tento příkaz:

        sudo apt-get install python-dev libffi-dev libssl-dev
        sudo apt-get remove python-openssl
        sudo pip install tweepy progressbar pyOpenSSL requests[security]
        
    > [AZURE.NOTE] Bitů informace o odebrání python openssl, instalaci python vývojáře, libffi vývoj, libssl vývoj, pyOpenSSL a žádosti o [zabezpečení], je vyhnout InsecurePlatform upozornění při připojování k Twitter prostřednictvím SSL od Python.
    >
    > Chcete-li předejít [chybu](https://github.com/tweepy/tweepy/issues/576) , které může dojít při zpracování tweety se používá Tweepy v3.2.0.

4. Umožňuje vytvořit nový soubor s názvem __gettweets.py__tento příkaz:

        nano gettweets.py

5. Pomocí následujících jako obsah souboru __gettweets.py__ . Nahrazení zástupného symbolu informace pro __spotř\_tajná__, __spotř\_klíč__, __přístup /\_tokenu__, a __přístup\_tokenu\_tajná__ s informacemi z aplikace Twitter.

        #!/usr/bin/python

        from tweepy import Stream, OAuthHandler
        from tweepy.streaming import StreamListener
        from progressbar import ProgressBar, Percentage, Bar
        import json
        import sys

        #Twitter app information
        consumer_secret='Your consumer secret'
        consumer_key='Your consumer key'
        access_token='Your access token'
        access_token_secret='Your access token secret'

        #The number of tweets we want to get
        max_tweets=10000

        #Create the listener class that will receive and save tweets
        class listener(StreamListener):
            #On init, set the counter to zero and create a progress bar
            def __init__(self, api=None):
                self.num_tweets = 0
                self.pbar = ProgressBar(widgets=[Percentage(), Bar()], maxval=max_tweets).start()

            #When data is received, do this
            def on_data(self, data):
                #Append the tweet to the 'tweets.txt' file
                with open('tweets.txt', 'a') as tweet_file:
                    tweet_file.write(data)
                    #Increment the number of tweets
                    self.num_tweets += 1
                    #Check to see if we have hit max_tweets and exit if so
                    if self.num_tweets >= max_tweets:
                        self.pbar.finish()
                        sys.exit(0)
                    else:
                        #increment the progress bar
                        self.pbar.update(self.num_tweets)
                return True

            #Handle any errors that may occur
            def on_error(self, status):
                print status

        #Get the OAuth token
        auth = OAuthHandler(consumer_key, consumer_secret)
        auth.set_access_token(access_token, access_token_secret)
        #Use the listener class for stream processing
        twitterStream = Stream(auth, listener())
        #Filter for these topics
        twitterStream.filter(track=["azure","cloud","hdinsight"])

6. Uložte soubor pomocí __kombinace kláves Ctrl + X__a __Y__ .

7. Pomocí následujícího příkazu a spusťte soubor stáhnout tweety:

        python gettweets.py

    Indikátor průběhu by měl umístit a počítat při stahování a uložení souboru tweety až 100 %.

    > [AZURE.NOTE] Pokud trvá příliš dlouho pruhu průběhu přejdete, měli byste změnit filtr, který chcete sledovat trendu témata; Po spoustu tweety o na téma, které filtrují na můžete získat velmi rychle 10000 tweety potřeby.

###<a name="upload-the-data"></a>Odeslání dat

Pokud chcete odeslat data do WASB (distribuovaný systém souborů používaný HDInsight), použijte následující příkazy:

    hdfs dfs -mkdir -p /tutorials/twitter/data
    hdfs dfs -put tweets.txt /tutorials/twitter/data/tweets.txt

Toto jsou uložená data v umístění, které mají přístup k všech uzlů v clusteru.

##<a name="run-the-hiveql-job"></a>Spuštění úlohy HiveQL


1. Umožňuje vytvořit soubor obsahující příkazy HiveQL tento příkaz:

        nano twitter.hql
    
    Použijte následující jako obsah souboru:

        set hive.exec.dynamic.partition = true;
        set hive.exec.dynamic.partition.mode = nonstrict;
        -- Drop table, if it exists
        DROP TABLE tweets_raw;
        -- Create it, pointing toward the tweets logged from Twitter
        CREATE EXTERNAL TABLE tweets_raw (
            json_response STRING
        )
        STORED AS TEXTFILE LOCATION '/tutorials/twitter/data';
        -- Drop and recreate the destination table
        DROP TABLE tweets;
        CREATE TABLE tweets
        (
            id BIGINT,
            created_at STRING,
            created_at_date STRING,
            created_at_year STRING,
            created_at_month STRING,
            created_at_day STRING,
            created_at_time STRING,
            in_reply_to_user_id_str STRING,
            text STRING,
            contributors STRING,
            retweeted STRING,
            truncated STRING,
            coordinates STRING,
            source STRING,
            retweet_count INT,
            url STRING,
            hashtags array<STRING>,
            user_mentions array<STRING>,
            first_hashtag STRING,
            first_user_mention STRING,
            screen_name STRING,
            name STRING,
            followers_count INT,
            listed_count INT,
            friends_count INT,
            lang STRING,
            user_location STRING,
            time_zone STRING,
            profile_image_url STRING,
            json_response STRING
        );
        -- Select tweets from the imported data, parse the JSON,
        -- and insert into the tweets table
        FROM tweets_raw
        INSERT OVERWRITE TABLE tweets
        SELECT
            cast(get_json_object(json_response, '$.id_str') as BIGINT),
            get_json_object(json_response, '$.created_at'),
            concat(substr (get_json_object(json_response, '$.created_at'),1,10),' ',
            substr (get_json_object(json_response, '$.created_at'),27,4)),
            substr (get_json_object(json_response, '$.created_at'),27,4),
            case substr (get_json_object(json_response, '$.created_at'),5,3)
                when "Jan" then "01"
                when "Feb" then "02"
                when "Mar" then "03"
                when "Apr" then "04"
                when "May" then "05"
                when "Jun" then "06"
                when "Jul" then "07"
                when "Aug" then "08"
                when "Sep" then "09"
                when "Oct" then "10"
                when "Nov" then "11"
                when "Dec" then "12" end,
            substr (get_json_object(json_response, '$.created_at'),9,2),
            substr (get_json_object(json_response, '$.created_at'),12,8),
            get_json_object(json_response, '$.in_reply_to_user_id_str'),
            get_json_object(json_response, '$.text'),
            get_json_object(json_response, '$.contributors'),
            get_json_object(json_response, '$.retweeted'),
            get_json_object(json_response, '$.truncated'),
            get_json_object(json_response, '$.coordinates'),
            get_json_object(json_response, '$.source'),
            cast (get_json_object(json_response, '$.retweet_count') as INT),
            get_json_object(json_response, '$.entities.display_url'),
            array(
                trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[1].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[2].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[3].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[4].text')))),
            array(
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[1].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[2].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[3].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[4].screen_name')))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
            get_json_object(json_response, '$.user.screen_name'),
            get_json_object(json_response, '$.user.name'),
            cast (get_json_object(json_response, '$.user.followers_count') as INT),
            cast (get_json_object(json_response, '$.user.listed_count') as INT),
            cast (get_json_object(json_response, '$.user.friends_count') as INT),
            get_json_object(json_response, '$.user.lang'),
            get_json_object(json_response, '$.user.location'),
            get_json_object(json_response, '$.user.time_zone'),
            get_json_object(json_response, '$.user.profile_image_url'),
            json_response
        WHERE (length(json_response) > 500);
        
        
3. Stiskněte __kombinaci kláves Ctrl + X__a potom stiskněte __Y__ pro uložení souboru.

4. Zadejte následující příkaz Spustit HiveQL obsažené v souboru:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -i twitter.hql
        
    Načtení prostředí podregistru, spustí HiveQL v souboru __twitter.hql__ a nakonec vrátit `jdbc:hive2//localhost:10001/>` dotaz.
    
5. Z příkazového řádku beeline použijte k ověření, že můžete vybrat data v tabulce __tweety__ vytvořil HiveQL v souboru __twitter.hql__ následující:
        
        SELECT name, screen_name, count(1) as cc
            FROM tweets
            WHERE text like "%Azure%"
            GROUP BY name,screen_name
            ORDER BY cc DESC LIMIT 10;

    Vrátí maximálně 10 tweety obsahujících slovo __Azure__ v textu zprávy.

##<a name="next-steps"></a>Další kroky

V tomto kurzu budeme viděli, jak transformace nestrukturovaná datovou sadu JSON na strukturovaného tabulku podregistru do dotazu, zkoumání a analýza dat z Twitter pomocí Azure HDInsight. Další informace najdete v tématu:

- [Začínáme s HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
- [Analýza dat zpoždění letů pomocí HDInsight](hdinsight-analyze-flight-delay-data-linux.md)

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter
