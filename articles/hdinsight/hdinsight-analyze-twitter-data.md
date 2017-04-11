<properties
    pageTitle="Analýza dat pomocí Hadoop v HDInsight Twitter | Microsoft Azure"
    description="Naučte se používat podregistru k analýze dat Twitter na Hadoop v HDInsight zobrazíte použití četnost určitá slova."
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>

# <a name="analyze-twitter-data-using-hive-in-hdinsight"></a>Analýza dat Twitter pomocí podregistru v HDInsight

Sociální weby jsou jednou z hlavních Hybné faktory pro přijetí velký data. Veřejná rozhraní API poskytovanou podobné Twitter weby jsou užitečné zdroje dat pro analýzu a principy Oblíbené trendy. V tomto kurzu získat tweety pomocí Twitter streamování rozhraní API a potom použijte Apache podregistru v Azure HDInsight zobrazte seznam Twitter uživatelů, kterým odeslaná většina tweety obsahující určité slovo.

> [AZURE.NOTE] Kroky v tomto dokumentu vyžadují clusteru serveru s Windows HDInsight. Postup konkrétní na základě Linux obrázku najdete v tématech [Twitter analýza dat pomocí podregistru v HDInsight (Linux)](hdinsight-analyze-twitter-data-linux.md).


##<a name="prerequisites"></a>Zjistit předpoklady pro

Před zahájením tohoto kurzu, musíte mít takto:

- **Stanici** pomocí prostředí PowerShell Azure nainstalovali a nakonfigurovali. 

    Spustit skripty Windows Powershellu, musíte Azure PowerShell spustit jako správce a nastavit zásady spuštění *RemoteSigned*. V tématu [spuštění Windows PowerShell skriptů][powershell-script].

    Před spuštěním skripty Windows Powershellu, zkontrolujte, jestli že jste připojeni k předplatnému Azure pomocí následující rutinu:

        Login-AzureRmAccount

    Pokud máte víc předplatných Azure, můžete nastavit aktuálního předplatného pomocí následující rutinu:

        Select-AzureRmSubscription -SubscriptionID <Azure Subscription ID>

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Azure HDInsight obrázku**. Na obrázku zřizování najdete v článku [Začínáme s používáním HDInsight] [ hdinsight-get-started] nebo [poskytování HDInsight clusterů] [hdinsight-provision]. Je třeba název clusteru dál v tomto kurzu.

V následující tabulce jsou uvedeny soubory používané v tomto kurzu:

Soubory|Popis
---|---
/tutorials/Twitter/data/tweets.txt|Zdrojová data pro danou úlohu podregistru.
/tutorials/Twitter/Output|Výstupní složce pro danou úlohu podregistru. Výchozí podregistru úlohy výstup název souboru je **000000_0**.
tutorials/Twitter/Twitter.hql|Soubor skriptu HiveQL.
/tutorials/Twitter/JobStatus|Stav úlohy Hadoop.


##<a name="get-twitter-feed"></a>Získání Twitter informačního kanálu

V tomto kurzu budete používat [Twitter streamování rozhraní API][twitter-streaming-api]. Zvláštní Twitter streamování rozhraní API použijete je [stavy nebo filtr][twitter-statuses-filter].

>[AZURE.NOTE] Soubor obsahující 10 000 tweety a soubor skriptu podregistru (uvedené v následující části) jsou nahraná v kontejneru veřejné objektů Blob. Pokud chcete použít nahrané soubory, můžete tuto část přeskočit.

[Tweety data](https://dev.twitter.com/docs/platform-objects/tweets) uložena ve formátu JavaScript Object Notation (JSON) obsahující složité struktuře vnořené. Místo psaní počet řádků kódu pomocí běžných programovací jazyk, můžete transformovat tuto strukturu vnořené do tabulku podregistru, tak, že je možné zjistit tak, že jazyce SQL (Structured Query) – jako jazyk s názvem HiveQL.

Twitter používá k poskytování oprávněnými přístup k jeho API OAuth. OAuth je ověřovací protokol, který umožňuje uživatelům schválit aplikace chovalo na jejich jménem přitom sdílet své heslo. Další informace najdete v [oauth.net](http://oauth.net/) nebo pracovníků [Průvodce začátečníky OAuth](http://hueniverse.com/oauth/) z Hueniverse.

Cílem prvního kroku používat OAuth je k vytvoření nové aplikace na web pro vývojáře Twitter.

**Vytvoření aplikace Twitter**

1. Přihlaste se k [https://apps.twitter.com/](https://apps.twitter.com/). Pokud nemáte účet Twitter, klikněte na odkaz **Přihlásit se** .
2. Klikněte na **vytvořit novou aplikaci**.
3. Zadejte **název**a **Popis** **webu**. Můžete mít adresu URL pro pole **Web** . V následující tabulce jsou uvedeny některé ukázkové hodnoty můžete:

    Pole|Hodnota
    ---|---
    Jméno|MyHDInsightApp
    Popis|MyHDInsightApp
    Web|http://www.myhdinsightapp.com

4. Kontrola **souhlasím**a klikněte na **vytvořit aplikace Twitter**.
5. Klikněte na kartu **oprávnění** . Výchozí oprávnění je **jen pro čtení**. To je dostatečné u tohoto kurzu.
6. Klikněte na kartu **klíče a tokeny přístupu** .
7. Klikněte na **vytvořit přístupový token**.
8. V pravém horním rohu stránky klepněte na tlačítko **Testovat OAuth** .
9. Poznamenejte si **klíč příjemce**, **příjemce tajná**, **přístupový token**a **tokenu tajná přístup**. Budete potřebovat hodnoty dál v tomto kurzu.

V tomto kurzu se používat Windows PowerShell aby volání webové služby. .NET C# výběru, najdete v článku [analyzovat data v reálném Twitter myšlenkou s HBase v HDInsight][hdinsight-hbase-twitter-sentiment]. Další oblíbené nástroj pro volání webové služby je [*otáčení*][curl]. Otočení si můžete stáhnout z [tady][curl-download].

>[AZURE.NOTE] Při použití příkazu otočení v systému Windows pro hodnoty možnost použijte dvojité uvozovky místo apostrofy.

**Chcete-li získat tweety**

1. Otevřete Windows PowerShell integrovaném skriptu prostředí (ISE). (Na obrazovce Start ve Windows 8 zadejte **PowerShell_ISE** a klikněte na **ISE v prostředí Windows PowerShell**. V tématu [spuštění prostředí Windows PowerShell ve Windows 8 a Windows][powershell-start].)

2. Zkopírujte tento skript do podokna skript:

        #region - variables and constants
        $clusterName = "<HDInsightClusterName>" # Enter the HDInsight cluster name

        # Enter the OAuth information for your Twitter application
        $oauth_consumer_key = "<TwitterAppConsumerKey>";
        $oauth_consumer_secret = "<TwitterAppConsumerSecret>";
        $oauth_token = "<TwitterAppAccessToken>";
        $oauth_token_secret = "<TwitterAppAccessTokenSecret>";

        $destBlobName = "tutorials/twitter/data/tweets.txt" # This script saves the tweets into this blob.

        $trackString = "Azure, Cloud, HDInsight" # This script gets the tweets containing these keywords.
        $track = [System.Uri]::EscapeDataString($trackString);
        $lineMax = 10000  # The script will get this number of tweets. It is about 3 minutes every 100 lines.
        #endregion

        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        Login-AzureRmAccount
        #endregion

        #region - Create a block blob object for writing tweets into Blob storage
        Write-Host "Get the default storage account name and Blob container name using the cluster name ..." -ForegroundColor Green
        $myCluster = Get-AzureRmHDInsightCluster -Name $clusterName
        $resourceGroupName = $myCluster.ResourceGroup
        $storageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
        $containerName = $myCluster.DefaultStorageContainer
        Write-Host "`tThe storage account name is $storageAccountName." -ForegroundColor Yellow
        Write-Host "`tThe blob container name is $containerName." -ForegroundColor Yellow

        Write-Host "Define the Azure storage connection string ..." -ForegroundColor Green
        $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
        $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$storageAccountName;AccountKey=$storageAccountKey"
        Write-Host "`tThe connection string is $storageConnectionString." -ForegroundColor Yellow

        Write-Host "Create block blob object ..." -ForegroundColor Green
        $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
        $storageClient = $storageAccount.CreateCloudBlobClient();
        $storageContainer = $storageClient.GetContainerReference($containerName)
        $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)
        #end region

        # region - Format OAuth strings
        Write-Host "Format oauth strings ..." -ForegroundColor Green
        $oauth_nonce = [System.Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes([System.DateTime]::Now.Ticks.ToString()));
        $ts = [System.DateTime]::UtcNow - [System.DateTime]::ParseExact("01/01/1970", "dd/MM/yyyy", $null)
        $oauth_timestamp = [System.Convert]::ToInt64($ts.TotalSeconds).ToString();

        $signature = "POST&";
        $signature += [System.Uri]::EscapeDataString("https://stream.twitter.com/1.1/statuses/filter.json") + "&";
        $signature += [System.Uri]::EscapeDataString("oauth_consumer_key=" + $oauth_consumer_key + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_nonce=" + $oauth_nonce + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_signature_method=HMAC-SHA1&");
        $signature += [System.Uri]::EscapeDataString("oauth_timestamp=" + $oauth_timestamp + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_token=" + $oauth_token + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_version=1.0&");
        $signature += [System.Uri]::EscapeDataString("track=" + $track);

        $signature_key = [System.Uri]::EscapeDataString($oauth_consumer_secret) + "&" + [System.Uri]::EscapeDataString($oauth_token_secret);

        $hmacsha1 = new-object System.Security.Cryptography.HMACSHA1;
        $hmacsha1.Key = [System.Text.Encoding]::ASCII.GetBytes($signature_key);
        $oauth_signature = [System.Convert]::ToBase64String($hmacsha1.ComputeHash([System.Text.Encoding]::ASCII.GetBytes($signature)));

        $oauth_authorization = 'OAuth ';
        $oauth_authorization += 'oauth_consumer_key="' + [System.Uri]::EscapeDataString($oauth_consumer_key) + '",';
        $oauth_authorization += 'oauth_nonce="' + [System.Uri]::EscapeDataString($oauth_nonce) + '",';
        $oauth_authorization += 'oauth_signature="' + [System.Uri]::EscapeDataString($oauth_signature) + '",';
        $oauth_authorization += 'oauth_signature_method="HMAC-SHA1",'
        $oauth_authorization += 'oauth_timestamp="' + [System.Uri]::EscapeDataString($oauth_timestamp) + '",'
        $oauth_authorization += 'oauth_token="' + [System.Uri]::EscapeDataString($oauth_token) + '",';
        $oauth_authorization += 'oauth_version="1.0"';

        $post_body = [System.Text.Encoding]::ASCII.GetBytes("track=" + $track);
        #endregion

        #region - Read tweets
        Write-Host "Create HTTP web request ..." -ForegroundColor Green
        [System.Net.HttpWebRequest] $request = [System.Net.WebRequest]::Create("https://stream.twitter.com/1.1/statuses/filter.json");
        $request.Method = "POST";
        $request.Headers.Add("Authorization", $oauth_authorization);
        $request.ContentType = "application/x-www-form-urlencoded";
        $body = $request.GetRequestStream();

        $body.write($post_body, 0, $post_body.length);
        $body.flush();
        $body.close();
        $response = $request.GetResponse() ;

        Write-Host "Start stream reading ..." -ForegroundColor Green

        Write-Host "Define a MemoryStream and a StreamWriter for writing ..." -ForegroundColor Green
        $memStream = New-Object System.IO.MemoryStream
        $writeStream = New-Object System.IO.StreamWriter $memStream

        $sReader = New-Object System.IO.StreamReader($response.GetResponseStream())

        $inrec = $sReader.ReadLine()
        $count = 0
        while (($inrec -ne $null) -and ($count -le $lineMax))
        {
            if ($inrec -ne "")
            {
                Write-Host "`n`t $count tweets received." -ForegroundColor Yellow

                $writeStream.WriteLine($inrec)
                $count ++
            }

            $inrec=$sReader.ReadLine()
        }
        #endregion

        #region - Write tweets to Blob storage
        Write-Host "Write to the destination blob ..." -ForegroundColor Green
        $writeStream.Flush()
        $memStream.Seek(0, "Begin")
        $destBlob.UploadFromStream($memStream)

        $sReader.close()
        #endregion

        Write-Host "Completed!" -ForegroundColor Green

3. Nastavení prvního pět až osm proměnných v skript:


    Proměnná|Popis
    ---|---
    $clusterName|To je název HDInsight clusteru, ve které chcete spustit aplikaci.
    $oauth_consumer_key|Jedná se o Twitter aplikace **klíč příjemce** , který jste si zapsali dříve při vytváření aplikace Twitter.
    $oauth_consumer_secret|Toto je aplikace Twitter **tajná příjemce** , který jste si zapsali dříve.
    $oauth_token|Toto je aplikace Twitter **přístupový token** , který jste si zapsali dříve.
    $oauth_token_secret|Toto je aplikace Twitter **tokenu tajná přístupu** , kterou jste si zapsali dříve.
    $destBlobName|To je název objektů blob výstupu. Výchozí hodnota je **tutorials/twitter/data/tweets.txt**. Pokud změníte výchozí hodnotu, bude potřebujete aktualizovat skripty Windows Powershellu příslušným způsobem.
    $trackString|Webová služba vrátí tweety související s těmito klíčových slov. Výchozí hodnota je **Azure, cloudu, HDInsight**. Pokud změníte výchozí hodnotu, aktualizujete skripty Windows Powershellu příslušným způsobem.
    $lineMax|Hodnota, určuje, kolik tweety skript přečte. Číst 100 tweety trvá asi tři minut. Můžete nastavit větší číslo, ale bude trvat delší dobu stáhnout.

5. Stisknutím klávesy **F5** následujícím způsobem. Pokud narazíte na problémy, jako alternativu, vyberte všechny řádky a stiskněte klávesu **F8**.
6. Uvidíte "Hotovo!" na konci výstupu. Zobrazí se žádná chybová zpráva v červené barvě.

Ověření postupem můžete zkontrolovat výstupní soubor **/tutorials/twitter/data/tweets.txt**v úložišti objektů Blob Azure pomocí Průzkumníka Azure úložiště nebo Azure Powershellu. Ukázka skriptu prostředí Windows PowerShell pro vypsání soubory, najdete v článku [úložiště objektů Blob použití s HDInsight][hdinsight-storage-powershell].



##<a name="create-hiveql-script"></a>Vytvoření HiveQL skriptu

Pomocí prostředí PowerShell Azure, můžete spustit několik výrokových HiveQL jeden po druhém nebo balíček HiveQL údajů do soubor skriptu. V tomto kurzu vytvoříte skript HiveQL. Soubor skriptu musí nahrát k úložišti objektů Blob Azure. V následující části se spustí soubor skriptu pomocí prostředí PowerShell Azure.

>[AZURE.NOTE] Soubor skriptu podregistru a soubor obsahující 10 000 tweety jsou nahraná v kontejneru veřejné objektů Blob. Pokud chcete použít nahrané soubory, můžete tuto část přeskočit.

Skript HiveQL bude postupujte takto:

1. **Odstranit tabulku tweets_raw** v případě tabulky již existuje.
2. **Vytvořit tabulku podregistru tweets_raw**. Tento dočasná strukturovaných tabulku podregistru obsahuje data pro další extrahovat, transformace a načíst zpracování (ETL). Informace o oddílech najdete v tématu [podregistru kurz][apache-hive-tutorial].  
3. **Načtení dat** ze zdroje složky /tutorials/twitter/data. Velké tweety datovou sadu ve vnořené formátu JSON má nyní převedeny na dočasná strukturu tabulky podregistru.
3. **Odstranit tabulku tweety** v případě tabulky již existuje.
4. **Vytvoření tabulky tweety**. Před můžete dotaz proti datovou sadu tweety pomocí podregistru, potřebujete k spuštění jiným procesem ETL. Tento proces ETL definuje schéma podrobnější tabulku dat, které máte uložené v tabulce "twitter_raw".  
5. **Vložení tabulky přepsat**. Tento složité skript podregistru bude zahájit sadu dlouhé úlohy MapReduce cluster Hadoop. V závislosti na vaší datovou sadu a velikosti clusteru může to trvat asi 10 minut.
6. **Vložení přepsat adresář**. Spuštění dotazu a výstup datovou sadu do souboru. Dotaz vrátí seznam Twitter uživatelů, kterým odeslaná většina tweety obsahující slovo "Azure".

**Vytvořit skript podregistru a nahrajte na Azure**

1. Otevřete ISE v prostředí Windows PowerShell.
2. Zkopírujte tento skript do podokna skript:

        #region - variables and constants
        $clusterName = "<Existing HDInsight Cluster Name>" # Enter your HDInsight cluster name
        $subscriptionID = "<Azure Subscription ID>"
        
        $sourceDataPath = "/tutorials/twitter/data"
        $outputPath = "/tutorials/twitter/output"
        $hqlScriptFile = "tutorials/twitter/twitter.hql"
        
        $hqlStatements = @"
        set hive.exec.dynamic.partition = true;
        set hive.exec.dynamic.partition.mode = nonstrict;
        
        DROP TABLE tweets_raw;
        CREATE EXTERNAL TABLE tweets_raw (
            json_response STRING
        )
        STORED AS TEXTFILE LOCATION '$sourceDataPath';
        
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
        
        INSERT OVERWRITE DIRECTORY '$outputPath'
        SELECT name, screen_name, count(1) as cc
            FROM tweets
            WHERE text like "%Azure%"
            GROUP BY name,screen_name
            ORDER BY cc DESC LIMIT 10;
        "@
        #endregion
        
        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        
        Try{
            Get-AzureRmSubscription
        }
        Catch{
            Login-AzureRmAccount
        }
        
        Select-AzureRmSubscription -SubscriptionId $subscriptionID
        
        #endregion
        
        #region - Create a block blob object for writing the Hive script file
        Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
        $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroupName = $myCluster.ResourceGroup
        $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
        $defaultBlobContainerName = $myCluster.DefaultStorageContainer
        Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
        Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow
        
        Write-Host "Define the connection string ..." -ForegroundColor Green
        $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
        $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"
        
        Write-Host "Create block blob objects referencing the hql script file" -ForegroundColor Green
        $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
        $storageClient = $storageAccount.CreateCloudBlobClient();
        $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
        $hqlScriptBlob = $storageContainer.GetBlockBlobReference($hqlScriptFile)
        
        Write-Host "Define a MemoryStream and a StreamWriter for writing ... " -ForegroundColor Green
        $memStream = New-Object System.IO.MemoryStream
        $writeStream = New-Object System.IO.StreamWriter $memStream
        $writeStream.Writeline($hqlStatements)
        #endregion
        
        #region - Write the Hive script file to Blob storage
        Write-Host "Write to the destination blob ... " -ForegroundColor Green
        $writeStream.Flush()
        $memStream.Seek(0, "Begin")
        $hqlScriptBlob.UploadFromStream($memStream)
        #endregion
        
        Write-Host "Completed!" -ForegroundColor Green

        

4. Nastavte první dvě proměnné skript:

    Proměnná|Popis
    ---|---
    $clusterName|Zadejte název clusteru HDInsight, ve které chcete spustit aplikaci.
    $subscriptionID|Zadejte ID Azure předplatného.
    $sourceDataPath|Umístění úložiště objektů Blob Azure dotazů podregistru bude načteno data z. Není potřeba změnit tuto proměnnou.
    $outputPath|Umístění úložiště objektů Blob Azure kde dotazů podregistru vypíše výsledky. Není potřeba změnit tuto proměnnou.
    $hqlScriptFile|Umístění a název souboru soubor skriptu HiveQL. Není potřeba změnit tuto proměnnou.

5. Stisknutím klávesy **F5** následujícím způsobem. Pokud narazíte na problémy, jako alternativu, vyberte všechny řádky a stiskněte klávesu **F8**.
6. Uvidíte "Hotovo!" na konci výstupu. Zobrazí se žádná chybová zpráva v červené barvě.

Ověření postupem můžete zkontrolovat výstupní soubor **/tutorials/twitter/twitter.hql**v úložišti objektů Blob Azure pomocí Průzkumníka Azure úložiště nebo Azure Powershellu. Ukázka skriptu prostředí Windows PowerShell pro vypsání soubory, najdete v článku [úložiště objektů Blob použití s HDInsight][hdinsight-storage-powershell].  


##<a name="process-twitter-data-by-using-hive"></a>Proces Twitter dat pomocí podregistru

Dokončíte přípravu práce. Můžete teď vyvolat skript podregistru a ověřte výsledky.

### <a name="submit-a-hive-job"></a>Odeslání podregistru úlohy

Použijte tento skript prostředí Windows PowerShell spusťte skript podregistru. Bude třeba nastavit proměnnou první.

>[AZURE.NOTE] V části poslední dvě použít tweety a HiveQL skript jste nahráli, nastavte $hqlScriptFile na "/ tutorials/twitter/twitter.hql". Pokud chcete použít ty, které jsou nahraná na veřejné objektů blob vám, nastavte $hqlScriptFile "wasbs://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql".

    #region variables and constants
    $clusterName = "<Existing Azure HDInsight Cluster Name>"
    $httpUserName = "admin"
    $httpUserPassword = "<HDInsight Cluster HTTP User Password>"
    
    #use one of the following
    $hqlScriptFile = "wasbs://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql"
    $hqlScriptFile = "/tutorials/twitter/twitter.hql"
    
    $statusFolder = "/tutorials/twitter/jobstatus"
    #endregion
    
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    
    
    #region - Invoke Hive
    Write-Host "Invoke Hive ... " -ForegroundColor Green
    
    # Create the HDInsight cluster
    $pw = ConvertTo-SecureString -String $httpUserPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)
    
    Use-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName -HttpCredential $httpCredential 
    $response = Invoke-AzureRmHDInsightHiveJob -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -file $hqlScriptFile -StatusFolder $statusFolder #-OutVariable $outVariable
    
    Write-Host "Display the standard error log ... " -ForegroundColor Green
    $jobID = ($response | Select-String job_ | Select-Object -First 1) -replace ‘\s*$’ -replace ‘.*\s’
    Get-AzureRmHDInsightJobOutput -ClusterName $clusterName -JobId $jobID -DefaultContainer $defaultBlobContainerName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -HttpCredential $httpCredential
    #endregion

### <a name="check-the-results"></a>Zkontrolujte výsledky

Použijte tento skript prostředí Windows PowerShell ke kontrole výstupu projektu podregistru. Bude třeba nastavit první dvě proměnné.

    #region variables and constants
    $clusterName = "<Existing Azure HDInsight Cluster Name>"
    
    $blob = "tutorials/twitter/output/000000_0" # The name of the blob to be downloaded.
    #engregion
    
    #region - Create an Azure storage context object
    Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    
    Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
    Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow
    
    Write-Host "Create a context object ... " -ForegroundColor Green
    $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey  
    #endregion
    
    #region - Download blob and display blob
    Write-Host "Download the blob ..." -ForegroundColor Green
    cd $HOME
    Get-AzureStorageBlobContent -Container $defaultBlobContainerName -Blob $blob -Context $storageContext -Force
    
    Write-Host "Display the output ..." -ForegroundColor Green
    Write-Host "==================================" -ForegroundColor Green
    cat "./$blob"
    Write-Host "==================================" -ForegroundColor Green
    #end region

> [AZURE.NOTE] Tabulku podregistru používá \001 jako oddělovač. Oddělovač není viditelná do výstupu.

Po výsledky analýzy byly umístěny v úložišti objektů Blob Azure, export dat do databáze nebo SQL server Azure SQL, export dat do Excelu pomocí Power Query nebo připojení k datům aplikace pomocí podregistru ovladač ODBC. Další informace najdete v tématu [Použití Sqoop s HDInsight][hdinsight-use-sqoop], [daty zpoždění letů analyzovat pomocí HDInsight][hdinsight-analyze-flight-delay-data], [Připojit Excel k Hdinsightu pomocí Power Query][hdinsight-power-query]a [Připojit Excel k Hdinsightu pomocí ovladač ODBC podregistru Microsoft][hdinsight-hive-odbc].

##<a name="next-steps"></a>Další kroky

V tomto kurzu budeme viděli, jak transformace nestrukturovaná datovou sadu JSON na strukturovaného tabulku podregistru do dotazu, zkoumání a analýza dat z Twitter pomocí Azure HDInsight. Další informace najdete v tématu:

- [Začínáme s HDInsight][hdinsight-get-started]
- [Analyzovat data v reálném myšlenkou Twitter s HBase v HDInsight][hdinsight-hbase-twitter-sentiment]
- [Analýza dat zpoždění letů pomocí HDInsight][hdinsight-analyze-flight-delay-data]
- [Připojit Excel k Hdinsightu pomocí Power Query][hdinsight-power-query]
- [Připojit Excel k Hdinsightu pomocí Microsoft podregistru ovladač ODBC][hdinsight-hive-odbc]
- [Použití Sqoop s HDInsight][hdinsight-use-sqoop]

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176961.aspx


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]: ../hdinsight-hadoop-use-blob-storage.md#powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-odbc-driver.md
[hdinsight-hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
