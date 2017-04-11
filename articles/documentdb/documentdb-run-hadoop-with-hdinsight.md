<properties
    pageTitle="Spuštění úlohy Hadoop pomocí DocumentDB a HDInsight | Microsoft Azure"
    description="Zjistěte, jak spustit jednoduchý podregistru, Prasátko a MapReduce úlohu s DocumentDB a dat Azure HDInsight."
    services="documentdb"
    authors="dennyglee"
    manager="jhubbard"
    editor="mimig"
    documentationCenter=""/>


<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="denlee"/>

#<a name="DocumentDB-HDInsight"></a>Spuštění úlohy Hadoop pomocí DocumentDB a HDInsight

Tento kurz se dozvíte, jak provádět [Apache podregistru][apache-hive], [Apache Prasátko][apache-pig]a [Apache Hadoop] [ apache-hadoop] MapReduce úlohy na Azure HDInsight s jeho DocumentDB Hadoop spojnice. Program na DocumentDB Hadoop connector umožňuje DocumentDB má fungovat jako zdroj i jímky pro podregistru, Prasátko a MapReduce práce. Tento kurz se používat DocumentDB jako zdroje dat a cílové Hadoop projektů.

Po dokončení tohoto kurzu, byste měli odpovězte na následující otázky:

- Jak načtení dat z DocumentDB pomocí podregistru, Prasátko nebo MapReduce úlohy
- Jak uložit data v DocumentDB pomocí podregistru, Prasátko nebo MapReduce úlohy

Doporučujeme Začínáme se můžete podívat v následujícím videu, které jsme projít podregistru úlohy pomocí DocumentDB a HDInsight.

> [AZURE.VIDEO use-azure-documentdb-hadoop-connector-with-azure-hdinsight]

Vraťte se do tohoto článku, kde budete dostávat úplné podrobnosti o způsobu spuštění úlohy technologie pro analýzu svých dat DocumentDB.

> [AZURE.TIP] Tento kurz předpokládá, že máte zkušenosti pomocí Apache Hadoop, podregistru nebo Prasátko. Pokud začínáte Apache Hadoop podregistru a Prasátko, doporučujeme, abyste návštěva [si přečtěte následující dokumentaci Apache Hadoop][apache-hadoop-doc]. Tento kurz také předpokládá, že máte zkušenosti s DocumentDB a s účtem DocumentDB. Pokud začínáte DocumentDB nebo nemáte účet DocumentDB, zkontrolujte si naše [Začínáme] [ getting-started] stránky.

Nemáte čas na dokončení kurzu a chcete získat úplné ukázky skriptů Powershellu pro podregistru, Prasátko a MapReduce? Není problém je získat [tady][documentdb-hdinsight-samples]. Stahování obsahuje také hql Prasátko a java souborů pro tyto příklady.

## <a name="NewestVersion"></a>Nejnovější verze

<table border='1'>
    <tr><th>Verze Hadoop spojnice</th>
        <td>1.2.0</td></tr>
    <tr><th>Identifikátor Uri skriptu</th>
        <td>https://portalcontent.BLOB.Core.Windows.NET/scriptaction/documentdb-hadoop-Installer-v04.ps1</td></tr>
    <tr><th>Datum změny</th>
        <td>04/26/2016</td></tr>
    <tr><th>Podporované HDInsight verze</th>
        <td>3.1, 3,2</td></tr>
    <tr><th>Protokol změn</th>
        <td>Aktualizované DocumentDB Java SDK 1.6.0</br>
            Přidaná podpora pro oddíly kolekce jako zdroj a jímky</br>
        </td></tr>
</table>

## <a name="Prerequisites"></a>Zjistit předpoklady pro
Před postupujte podle pokynů v tomto kurzu, ověřte, jestli máte takto:

- Účet DocumentDB databáze a kolekce s dokumenty uvnitř. Další informace najdete v tématu [Začínáme s DocumentDB][getting-started]. Import ukázkových dat do vašeho účtu DocumentDB s [DocumentDB importovat nástroj][documentdb-import-data].
- Výkon. Čtení a zápis z Hdinsightu ke spočítání na vašich jednotkách přiděleného žádost pro vaše kolekce. Další informace najdete v tématu [Provisioned výkon, žádost o jednotky a databáze operace][documentdb-manage-throughput].
- Kapacita pro další uložená procedura v rámci každé výstupní kolekce. Uložené procedury se používají pro přenos výsledné dokumentů. Další informace najdete v tématu [kolekcí a zřizování výkon][documentdb-manage-document-storage].
- Kapacita výsledné dokumenty z úlohy podregistru, Prasátko nebo MapReduce. Další informace najdete v tématu [Správa DocumentDB kapacitu a výkon][documentdb-manage-collections].
- [*Volitelné*] Kapacita další kolekce. Další informace najdete v tématu [ukládání dokumentů Provisioned a rejstřík nároky][documentdb-manage-document-storage].

> [AZURE.WARNING] A zabránit tak vystavením novou kolekci během všech projektů, můžete vytisknete výsledky STDOUT, ukládání výstupu do kontejneru WASB nebo zadat už existující kolekci. V případě zadání existující kolekce, vytvoří se nové dokumenty uvnitř kolekce a již existující dokumenty pouze ovlivní dojde ke konfliktu *ID*. **Spojnice automaticky přepíše existující dokumenty s id konflikty**. Tato funkce můžete vypnout tak, že vypnete možnost upsert pro. Pokud dojde ke konfliktu upsert hodnota je false, se nepovede úlohy Hadoop; hlásí chybu id konfliktu.


## <a name="ProvisionHDInsight"></a>Krok 1: Vytvoření nového clusteru HDInsight
Tento kurz používá k přizpůsobení svůj cluster HDInsight skript akci z portálu Microsoft Azure. V tomto kurzu používáme portálu Azure vytvořit svůj cluster HDInsight. Další informace o používání rutin prostředí PowerShell a HDInsight .NET SDK, podívejte se na [clusterů přizpůsobení HDInsight pomocí skriptu akce] [ hdinsight-custom-provision] článek.

1. Přihlaste se k [portálu Azure][azure-portal].

2. Klikněte na **+ Nový** nahoře na levém navigačním panelu hledání **HDInsight** v horním vyhledávací panel na nové zásuvné.

3. **HDInsight** publikované společnost **Microsoft** se zobrazí v horní části výsledky. Klikněte na ni a potom klikněte na **vytvořit**.

4. V novém clusteru HDInsight vytvořte zásuvné, zadejte svoje **Jméno obrázku** a vyberte **předplatné** , chcete-li vytvořit zdroj v části.

    <table border='1'>
        <tr><td>Název obrázku</td><td>Název clusteru.<br/>
   Název serveru DNS musí zahájení a ukončení znakem alfa číselné a může obsahovat pomlčky.<br/>
   Pole musí být řetězec mezi 3 a 63 znaky.</td></tr>
        <tr><td>Název předplatného</td>
            <td>Pokud máte víc předplatných Azure, vyberte předplatné, bude hostovat svůj cluster HDInsight. </td></tr>
    </table>

5. Klikněte na **Vybrat typ obrázku** a nastavit následující vlastnosti na zadané hodnoty.

    <table border='1'>
        <tr><td>Typ obrázku</td><td><strong>Hadoop</strong></td></tr>
        <tr><td>Shluk osy</td><td><strong>Standardní</strong></td></tr>
        <tr><td>Operační systém</td><td><strong>Windows</strong></td></tr>
        <tr><td>Verze</td><td>nejnovější verze</td></tr>
    </table>

    Teď klikněte na **Výběr**.

    ![Poskytování údajů počáteční clusteru Hadoop HDInsight][image-customprovision-page1]

6. Klikněte na **přihlašovací údaje** pro nastavení přihlášení a přihlašovací údaje vzdáleného přístupu. Vyberte **uživatelské clusteru přihlašovací jméno** a **heslo pro přihlášení obrázku**.

    Pokud budete chtít vzdálené do svého obrázku, vyberte *Ano* v dolní části zásuvné a zadejte uživatelské jméno a heslo.

7. Klikněte na **Zdroj dat** , aby se nastavení primární umístění pro přístup k datům. Vyberte **Způsob výběru** a zadejte již existujícího účtu úložiště nebo vytvořte novou.

8. Na stejné zásuvné zadejte **Výchozí kontejner** a **umístění**. A klikněte na **Výběr**.

    > [AZURE.NOTE] Vyberte umístění zavřít vaši DocumentDB účtu oblast pro zvýšení výkonu

8. Klikněte na **ceny** vyberte číslo a zadejte uzlů. Můžete ponechat výchozí konfigurace a měřítko počtu uzlů pracovníka později.

9. Klikněte na **volitelná konfigurace**pak **skript akce** v zásuvné volitelná konfigurace.

    V poli Akce skriptem zadejte tyto informace chcete-li přizpůsobit svůj cluster HDInsight.

    <table border='1'>
        <tr><th>Vlastnost</th><th>Hodnota</th></tr>
        <tr><td>Jméno</td>
            <td>Zadejte název akce skriptu.</td></tr>
        <tr><td>Skript URI</td>
            <td>Zadejte identifikátor URI skript, který je zavolat a přizpůsobení clusteru.</br></br>
   Zadejte: </br> <strong>https://portalcontent.blob.core.windows.net/scriptaction/documentdb-hadoop-installer-v04.ps1</strong>.</td></tr>
        <tr><td>Hlavy</td>
            <td>Klepnutím na zaškrtávací políčko Spustit skript Powershellu na uzel vedoucího.</br></br>
            <strong>Zaškrtněte toto políčko</strong>.</td></tr>
        <tr><td>Pracovní</td>
            <td>Klepnutím na zaškrtávací políčko Spustit skript Powershellu na uzel kolegy.</br></br>
            <strong>Zaškrtněte toto políčko</strong>.</td></tr>
        <tr><td>Zookeeper</td>
            <td>Klepnutím na zaškrtávací políčko Spustit skript Powershellu na Zookeeper.</br></br>
            <strong>Není potřeba</strong>.
            </td></tr>
        <tr><td>Parametry</td>
            <td>Zadejte parametry, v případě potřeby skriptem.</br></br>
            <strong>Požadována bez parametrů</strong>.</td></tr>
    </table>

10. Vytvořte nové **Pole Skupina zdroje** nebo použití existující skupiny zdrojů v rámci předplatného Azure.

11. Teď podívejte **kód Pin pro řídicí panel** sledovat jeho nasazení a klikněte na **vytvořit**.

## <a name="InstallCmdlets"></a>Krok 2: Instalace a konfigurace prostředí PowerShell Azure

1. Nainstalujte Azure Powershellu. Pokyny najdete [tady][powershell-install-configure].

    > [AZURE.NOTE] Můžete taky jenom pro dotazy podregistru, můžete je HDInsight online podregistru editoru. K tomu, přihlaste se k [Portálu Azure][azure-portal], klikněte v levém podokně zobrazte seznam HDInsight clusterů **HDInsight** . Klikněte na obrázku, který chcete spustit podregistru dotazy a potom klikněte na **Konzole dotazu**.

2. Otevřete integrované skriptu prostředí Azure Powershellu:
    - Na počítači se systémem Windows 8 nebo Windows Server 2012 nebo vyšší můžete pomocí integrovaného vyhledávání. Na obrazovce Start zadejte **powershell ise** a klepněte na **Enter**.
    - V počítači se systémem verze dřívější než Windows 8 nebo Windows Server 2012 pomocí nabídky Start. Z nabídky Start **příkazovém řádku** zadejte do vyhledávacího pole a potom v seznamu výsledků, klikněte na **příkazovém řádku**. Na příkazovém řádku zadejte **powershell_ise** a klepněte na **Enter**.

3. Přidání účtu Azure.
    1. V podokně konzoly zadejte **Přidat AzureAccount** a klepněte na **Enter**.
    2. Zadejte e-mailovou adresu přidruženou k vašemu předplatnému Azure a klikněte na **pokračovat**.
    3. Zadejte do pole heslo k předplatnému Azure.
    4. Klikněte na **přihlásit**.

4. Následující obrázek označuje důležité části skriptování prostředí PowerShell Azure.

    ![Diagram pro Azure PowerShell][azure-powershell-diagram]

## <a name="RunHive"></a>Krok 3: Spuštění podregistru úlohy pomocí DocumentDB a HDInsight

> [AZURE.IMPORTANT] Všechny proměnné označen < > musí být vyplněná pomocí konfigurace nastavení.

1. Nastavení následujících proměnných v podokně skript Powershellu.

        # Provide Azure subscription name, the Azure Storage account and container that is used for the default HDInsight file system.
        $subscriptionName = "<SubscriptionName>"
        $storageAccountName = "<AzureStorageAccountName>"
        $containerName = "<AzureStorageContainerName>"

        # Provide the HDInsight cluster name where you want to run the Hive job.
        $clusterName = "<HDInsightClusterName>"

2. <p>A jdeme sestavování řetězce dotazu. Jsme budete psát podregistru dotaz, který má časová razítka generovat systém všechny dokumenty (_ts) a jedinečné ID (_rid) z kolekce DocumentDB sečte všechny dokumenty podle počtu minut a potom ukládá výsledků zpět do nové kolekce DocumentDB.</p>

    <p>Nejdřív Pojďme vytvořit tabulku podregistru z našich DocumentDB kolekce. Dodejte následující fragment kódu skript Powershellu podokno <strong>Po</strong> fragment kódu z #1. Ujistěte se, zahrnout volitelné DocumentDB.query parametr t PROČISTIT naše dokumenty jenom _ts a _rid.</p>

    > [AZURE.NOTE]**Pojmenování DocumentDB.inputCollections nebyl chybu.** Ano, jsme povolit přidávání víc kolekcí jako vstup: </br>

        '*DocumentDB.inputCollections*' = '*\<DocumentDB Input Collection Name 1\>*,*\<DocumentDB Input Collection Name 2\>*' A1A</br> The collection names are separated without spaces, using only a single comma.


        # Create a Hive table using data from DocumentDB. Pass DocumentDB the query to filter transferred data to _rid and _ts.
        $queryStringPart1 = "drop table DocumentDB_timestamps; "  +
                            "create external table DocumentDB_timestamps(id string, ts BIGINT) "  +
                            "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' "  +
                            "tblproperties ( " +
                                "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                "'DocumentDB.key' = '<DocumentDB Primary Key>', " +
                                "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                "'DocumentDB.inputCollections' = '<DocumentDB Input Collection Name>', " +
                                "'DocumentDB.query' = 'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "

3.  Pak vytvoříte tabulku podregistru kolekce výstupu. Vlastnosti dokumentu výstup budou měsíc, den, hodiny, minuty a celkový počet výskytů.

    > [AZURE.NOTE]**Ještě znovu pojmenování DocumentDB.outputCollections nebyl chybu.** Ano, jsme povolit přidávání víc kolekcí jako výstup: </br>
"*DocumentDB.outputCollections*"="*\<název kolekce DocumentDB výstup 1\>*,*\<název kolekce DocumentDB výstup 2\>*" </br> Názvy kolekce jsou oddělené bez mezer, pomocí pouze jeden čárku. </br></br>
Dokumenty budou distribuované kruhového v několika kolekcích. Sadu dokumentů budou uloženy v jedné kolekce a pak druhé dávku dokumenty budou uloženy v dalším shromažďování a tak dále.

        # Create a Hive table for the output data to DocumentDB.
        $queryStringPart2 = "drop table DocumentDB_analytics; " +
                              "create external table DocumentDB_analytics(Month INT, Day INT, Hour INT, Minute INT, Total INT) " +
                              "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' " +
                              "tblproperties ( " +
                                  "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                  "'DocumentDB.key' = '<DocumentDB Primary Key>', " +  
                                  "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                  "'DocumentDB.outputCollections' = '<DocumentDB Output Collection Name>' ); "

4. Nakonec Pojďme zaznamenávat dokumenty měsíc, den, hodiny a minuty a vložení výsledků zpět do výstupu tabulku podregistru.

        # GROUP BY minute, COUNT entries for each, INSERT INTO output Hive table.
        $queryStringPart3 = "INSERT INTO table DocumentDB_analytics " +
                              "SELECT month(from_unixtime(ts)) as Month, day(from_unixtime(ts)) as Day, " +
                              "hour(from_unixtime(ts)) as Hour, minute(from_unixtime(ts)) as Minute, " +
                              "COUNT(*) AS Total " +
                              "FROM DocumentDB_timestamps " +
                              "GROUP BY month(from_unixtime(ts)), day(from_unixtime(ts)), " +
                              "hour(from_unixtime(ts)) , minute(from_unixtime(ts)); "

5. Přidejte následující úryvek skript vytvořit definici podregistru projektu z předchozí dotazu.

        # Create a Hive job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $hiveJobDefinition = New-AzureHDInsightHiveJobDefinition -Query $queryString

    Můžete taky použít-přepínač soubor, který určuje soubor skriptu HiveQL na HDFS.

6. Přidejte následující úryvek ušetřit čas zahájení a odešlete podregistru projektu.

        # Save the start time and submit the job to the cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $hiveJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $hiveJobDefinition

7. Přidejte následující čekání na dokončení úlohy podregistru.

        # Wait for the Hive job to complete.
        Wait-AzureHDInsightJob -Job $hiveJob -WaitTimeoutInSeconds 3600

8. Přidejte následující vytisknout standardní výstup a čas zahájení a ukončení.

        # Print the standard error, the standard output of the Hive job, and the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $hiveJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green

9. **Spuštění** nové skript! **Klikněte na** tlačítko zelené spouštět.

10. Zkontrolujte výsledky. Přihlaste se do [portálu Azure][azure-portal].
    1. Na panelu vlevo klikněte na <strong>Procházet</strong> . </br>
    2. Klikněte na <strong>vše</strong> v pravém horním rohu panelu Procházet. </br>
    3. Najděte a klikněte na <strong>DocumentDB účty</strong>. </br>
    4. Pak hledání <strong>DocumentDB účtu</strong>a potom <strong>DocumentDB databáze</strong> a vaší <strong>Kolekce DocumentDB</strong> související s odběrem výstup podle podregistru dotazu.</br>
    5. Nakonec klikněte na položku <strong>Průzkumník dokumentů</strong> pod <strong>Nástrojů pro vývojáře</strong>.</br></p>

    Zobrazí se ve výsledcích dotazu podregistru.

    ![Podregistru výsledky dotazu][image-hive-query-results]

## <a name="RunPig"></a>Krok 4: Spuštění Prasátko úlohy pomocí DocumentDB a HDInsight

> [AZURE.IMPORTANT] Všechny proměnné označen < > musí být vyplněná pomocí konfigurace nastavení.

1. Nastavení následujících proměnných v podokně skript Powershellu.

        # Provide Azure subscription name.
        $subscriptionName = "Azure Subscription Name"

        # Provide HDInsight cluster name where you want to run the Pig job.
        $clusterName = "Azure HDInsight Cluster Name"

2. <p>A jdeme stavba řetězce dotazu. Jsme budete psát Prasátko dotaz, který má časová razítka generovat systém všechny dokumenty (_ts) a jedinečné ID (_rid) z kolekce DocumentDB sečte všechny dokumenty podle počtu minut a potom ukládá výsledků zpět do nové kolekce DocumentDB.</p>
    <p>Nejdřív načtení dokumenty z DocumentDB do HDInsight. Dodejte následující fragment kódu skript Powershellu podokno <strong>Po</strong> fragment kódu z #1. Zkontrolujte, že přidání dotazu DocumentDB volitelné dotazu parametr DocumentDB střih naše dokumenty jenom _ts a _rid.</p>

    > [AZURE.NOTE]Ano, jsme povolit přidávání víc kolekcí jako vstup: </br>
"*\<Název kolekce DocumentDB vstupní 1\>*,*\<název kolekce DocumentDB vstupní 2\>*"</br> Názvy kolekce jsou oddělené bez mezer, pomocí pouze jeden čárku. </b>

    Dokumenty budou distribuované kruhového přes víc kolekcí. Sadu dokumentů budou uloženy v jedné kolekce a pak druhé dávku dokumenty budou uloženy v dalším shromažďování a tak dále.

        # Load data from DocumentDB. Pass DocumentDB query to filter transferred data to _rid and _ts.
        $queryStringPart1 = "DocumentDB_timestamps = LOAD '<DocumentDB Endpoint>' USING com.microsoft.azure.documentdb.pig.DocumentDBLoader( " +
                                                        "'<DocumentDB Primary Key>', " +
                                                        "'<DocumentDB Database Name>', " +
                                                        "'<DocumentDB Input Collection Name>', " +
                                                        "'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "

3.  Dále si zaznamenávat dokumenty měsíc, den, hodiny, minuty a celkový počet výskytů.

        # GROUP BY minute and COUNT entries for each.
        $queryStringPart2 = "timestamp_record = FOREACH DocumentDB_timestamps GENERATE `$0#'id' as id:int, ToDate((long)(`$0#'ts') * 1000) as timestamp:datetime; " +
                            "by_minute = GROUP timestamp_record BY (GetYear(timestamp), GetMonth(timestamp), GetDay(timestamp), GetHour(timestamp), GetMinute(timestamp)); " +
                            "by_minute_count = FOREACH by_minute GENERATE FLATTEN(group) as (Year:int, Month:int, Day:int, Hour:int, Minute:int), COUNT(timestamp_record) as Total:int; "

4. Pojďme obsahují nakonec výsledků do naší novou kolekci výstupu.

    > [AZURE.NOTE]Ano, jsme povolíte přidáváte víc kolekcí jako výstup takto: </br>
"\<Název kolekce DocumentDB výstup 1\>,\<název kolekce DocumentDB výstup 2\>"</br> Názvy kolekce jsou oddělené bez mezer, pomocí pouze jeden čárku.</br>
Dokumenty budou distribuované kruhového přes víc kolekcí. Sadu dokumentů budou uloženy v jedné kolekce a pak druhé dávku dokumenty budou uloženy v dalším shromažďování a tak dále.

        # Store output data to DocumentDB.
        $queryStringPart3 = "STORE by_minute_count INTO '<DocumentDB Endpoint>' " +
                            "USING com.microsoft.azure.documentdb.pig.DocumentDBStorage( " +
                                "'<DocumentDB Primary Key>', " +
                                "'<DocumentDB Database Name>', " +
                                "'<DocumentDB Output Collection Name>'); "

5. Přidejte následující úryvek skript vytvořit definici Prasátko projektu z předchozí dotazu.

        # Create a Pig job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $pigJobDefinition = New-AzureHDInsightPigJobDefinition -Query $queryString -StatusFolder $statusFolder

    Můžete taky použít-přepínač soubor, který určuje soubor skriptu Prasátko na HDFS.

6. Přidejte následující úryvek ušetřit čas zahájení a odešlete Prasátko projektu.

        # Save the start time and submit the job to the cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $pigJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $pigJobDefinition

7. Přidejte následující čekání na dokončení úlohy Prasátko.

        # Wait for the Pig job to complete.
        Wait-AzureHDInsightJob -Job $pigJob -WaitTimeoutInSeconds 3600

8. Přidejte následující vytisknout standardní výstup a čas zahájení a ukončení.

        # Print the standard error, the standard output of the Hive job, and the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $pigJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green

9. **Spuštění** nové skript! **Klikněte na** tlačítko zelená spouštět.

10. Zkontrolujte výsledky. Přihlaste se do [portálu Azure][azure-portal].
    1. Na panelu vlevo klikněte na <strong>Procházet</strong> . </br>
    2. Klikněte na <strong>vše</strong> v pravém horním rohu panelu Procházet. </br>
    3. Najděte a klikněte na <strong>DocumentDB účty</strong>. </br>
    4. Pak hledání <strong>DocumentDB účtu</strong>a potom <strong>DocumentDB databáze</strong> a vaší <strong>Kolekce DocumentDB</strong> související s odběrem výstup podle Prasátko dotazu.</br>
    5. Nakonec klikněte na položku <strong>Průzkumník dokumentů</strong> pod <strong>Nástrojů pro vývojáře</strong>.</br></p>

    Zobrazí se ve výsledcích dotazu Prasátko.

    ![Prasátko výsledky dotazu][image-pig-query-results]

## <a name="RunMapReduce"></a>Krok 5: Spuštění MapReduce úlohy pomocí DocumentDB a HDInsight

1. Nastavení následujících proměnných v podokně skript Powershellu.

        $subscriptionName = "<SubscriptionName>"   # Azure subscription name
        $clusterName = "<ClusterName>"             # HDInsight cluster name

2. Budeme se spustit MapReduce projektu, který sečte počet výskytů pro každou vlastnosti dokumentu v kolekci DocumentDB. Přidáte tento skript fragment **Po** výše fragment kódu.

        # Define the MapReduce job.
        $TallyPropertiesJobDefinition = New-AzureHDInsightMapReduceJobDefinition -JarFile "wasb:///example/jars/TallyProperties-v01.jar" -ClassName "TallyProperties" -Arguments "<DocumentDB Endpoint>","<DocumentDB Primary Key>", "<DocumentDB Database Name>","<DocumentDB Input Collection Name>","<DocumentDB Output Collection Name>","<[Optional] DocumentDB Query>"

    > [AZURE.NOTE] TallyProperties v01.jar získáváte vlastní instalace konektoru Hadoop DocumentDB.

3. Přidejte tento příkaz Odeslat MapReduce projektu.

        # Save the start time and submit the job.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $TallyPropertiesJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $TallyPropertiesJobDefinition | Wait-AzureHDInsightJob -WaitTimeoutInSeconds 3600  

    Kromě definici úlohy MapReduce také zadat název clusteru HDInsight místo, kam chcete spustit MapReduce úlohy a přihlašovací údaje. Zahájení AzureHDInsightJob je synchronního volání. Kontrola po skončení projektu, získáte pomocí rutiny *Čekání AzureHDInsightJob* .

4. Přidejte tento příkaz Zkontrolovat chyby s úloha MapReduce.

        # Get the job output and print the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $TallyPropertiesJob.JobId -StandardError
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green

5. **Spuštění** nové skript! **Klikněte na** tlačítko zelené spouštět.

6. Zkontrolujte výsledky. Přihlaste se do [portálu Azure][azure-portal].
    1. Na panelu vlevo klikněte na <strong>Procházet</strong> .
    2. Klikněte na <strong>vše</strong> v pravém horním rohu panelu Procházet.
    3. Najděte a klikněte na <strong>DocumentDB účty</strong>.
    4. Pak hledání <strong>DocumentDB účtu</strong>a potom <strong>DocumentDB databáze</strong> a vaší <strong>Kolekce DocumentDB</strong> související s odběrem výstup podle MapReduce práce.
    5. Nakonec klikněte na položku <strong>Průzkumník dokumentů</strong> pod <strong>Nástrojů pro vývojáře</strong>.

    Zobrazí se výsledky MapReduce úlohy.

    ![Výsledky dotazu MapReduce][image-mapreduce-query-results]

## <a name="NextSteps"></a>Další kroky

Blahopřejeme! Právě jste spustili vaše první podregistru, Prasátko a MapReduce úlohy pomocí Azure DocumentDB a HDInsight.

Máme otevřít získaná naše Hadoop spojnice. Pokud jste zúčastněnými, můžete do ní můžete přidávat na [GitHub][documentdb-github].

Další informace naleznete v následujících článcích:

- [Můžete vyvíjet aplikace Java s Documentdb][documentdb-java-application]
- [Můžete vyvíjet Java MapReduce programy pro Hadoop v HDInsight][hdinsight-develop-deploy-java-mapreduce]
- [Začínáme s používáním Hadoop s podregistru v HDInsight pro analýzu použití mobilního telefonu][hdinsight-get-started]
- [Použití MapReduce s HDInsight][hdinsight-use-mapreduce]
- [Použití podregistru s HDInsight][hdinsight-use-hive]
- [Použití Prasátko s HDInsight][hdinsight-use-pig]
- [Přizpůsobení clusterů HDInsight pomocí skriptu akce][hdinsight-hadoop-customize-cluster]

[apache-hadoop]: http://hadoop.apache.org/
[apache-hadoop-doc]: http://hadoop.apache.org/docs/current/
[apache-hive]: http://hive.apache.org/
[apache-pig]: http://pig.apache.org/
[getting-started]: documentdb-get-started.md

[azure-portal]: https://portal.azure.com/
[azure-powershell-diagram]: ./media/documentdb-run-hadoop-with-hdinsight/azurepowershell-diagram-med.png

[documentdb-hdinsight-samples]: http://portalcontent.blob.core.windows.net/samples/documentdb-hdinsight-samples.zip
[documentdb-github]: https://github.com/Azure/azure-documentdb-hadoop
[documentdb-java-application]: documentdb-java-application.md
[documentdb-manage-collections]: documentdb-manage.md#Collections
[documentdb-manage-document-storage]: documentdb-manage.md#IndexOverhead
[documentdb-manage-throughput]: documentdb-manage.md#ProvThroughput
[documentdb-import-data]: documentdb-import-data.md

[hdinsight-custom-provision]: ../hdinsight/hdinsight-provision-clusters.md#powershell
[hdinsight-develop-deploy-java-mapreduce]: ../hdinsight/hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-hadoop-customize-cluster]: ../hdinsight/hdinsight-hadoop-customize-cluster.md
[hdinsight-get-started]: ../hdinsight/hdinsight-hadoop-tutorial-get-started-windows.md
[hdinsight-storage]: ../hdinsight/hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: ../hdinsight/hdinsight-use-hive.md
[hdinsight-use-mapreduce]: ../hdinsight/hdinsight-use-mapreduce.md
[hdinsight-use-pig]: ../hdinsight/hdinsight-use-pig.md

[image-customprovision-page1]: ./media/documentdb-run-hadoop-with-hdinsight/customprovision-page1.png
[image-hive-query-results]: ./media/documentdb-run-hadoop-with-hdinsight/hivequeryresults.PNG
[image-mapreduce-query-results]: ./media/documentdb-run-hadoop-with-hdinsight/mapreducequeryresults.PNG
[image-pig-query-results]: ./media/documentdb-run-hadoop-with-hdinsight/pigqueryresults.PNG

[powershell-install-configure]: ../powershell-install-configure.md
