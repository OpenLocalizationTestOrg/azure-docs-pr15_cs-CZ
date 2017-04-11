<properties
    pageTitle="Použití koordinátor Hadoop Oozie založená na čase v HDInsight | Microsoft Azure"
    description="Použití založená na čase koordinátor Hadoop Oozie v HDInsight velký datové služby. Zjistěte, jak definovat pracovní postupy Oozie a koordinátoři a odesílat úlohy."
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
    ms.date="07/25/2016"
    ms.author="jgao"/>


# <a name="use-time-based-oozie-coordinator-with-hadoop-in-hdinsight-to-define-workflows-and-coordinate-jobs"></a>Pomocí založená na čase koordinátor Oozie Hadoop v HDInsight definovat pracovní postupy a koordinaci úlohy

V tomto článku se dozvíte, jak definovat pracovní postupy a koordinátoři a jak aktivovat koordinátor úlohy na základě času. Je vhodné absolvovat [Použití Oozie s HDInsight] [ hdinsight-use-oozie] před v tomto článku. Kromě Oozie můžete naplánovat úlohy pomocí Azure Data Factory. Další Azure Data Factory najdete v tématu [použití Prasátko a podregistru s Data Factory](../data-factory/data-factory-data-transformation-activities.md).

> [AZURE.NOTE] Tento článek vyžaduje clusteru serveru s Windows HDInsight. Informace o použití Oozie včetně založená na čase úlohy na základě Linux clusteru, najdete v článku [Použití Oozie s Hadoop definovat a spustit pracovní postup na základě Linux HDInsight](hdinsight-use-oozie-linux-mac.md)

##<a name="what-is-oozie"></a>Co je Oozie

Apache Oozie je pracovní postup/koordinaci systému, který má na starosti Hadoop úlohy. Integrovaný s zásobníku Hadoop a podporuje Hadoop úlohy pro Apache MapReduce Apache Prasátko, Apache podregistru a Apache Sqoop. Jej lze také naplánovat úlohy, které jsou specifické pro systémem, třeba Java programů nebo skriptů prostředí.

Na tomto obrázku vidíte, které budete implementovat pracovní postup:

![Diagram pracovního postupu][img-workflow-diagram]

Pracovní postup obsahuje dva akce:

1. Spustí akce podregistru HiveQL skript k určení počtu výskytů jednotlivých typů úroveň protokolování v souboru protokolu log4j. Každý log4j protokolu se skládá z řádku pole, která obsahuje pole [úroveň protokolování] zobrazíte typ a závažnosti, například:

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

    Výstup skript podregistru je podobný:

        [DEBUG] 434
        [ERROR] 3
        [FATAL] 1
        [INFO]  96
        [TRACE] 816
        [WARN]  4

    Další informace o podregistru, najdete v článku [Použití podregistru s HDInsight][hdinsight-use-hive].

2.  Akce Sqoop exportuje akce Výstup HiveQL do tabulky v databázi Azure SQL. Další informace o Sqoop najdete v článku [Použití Sqoop s HDInsight][hdinsight-use-sqoop].

> [AZURE.NOTE] Podporované Oozie verzí HDInsight clusterů, najdete v článku [Co je nového v verze obrázku poskytovanou HDInsight?] [hdinsight-versions].


##<a name="prerequisites"></a>Zjistit předpoklady pro

Před zahájením tohoto kurzu, musíte mít takto:

- **Pracoviště se Azure Powershellu**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **HDInsight obrázku**. Informace o vytváření HDInsight obrázku najdete v tématu [Vytvoření HDInsight clusterů][hdinsight-provision], nebo [začít pracovat s HDInsight][hdinsight-get-started]. Budete potřebovat následujícími údaji projít kurzu:

    <table border = "1">
    <tr><th>Vlastnost obrázku</th><th>Název proměnné prostředí Windows PowerShell</th><th>Hodnota</th><th>Popis</th></tr>
    <tr><td>Název clusteru HDInsight</td><td>$clusterName</td><td></td><td>HDInsight obrázku, který se spustí tohoto kurzu.</td></tr>
    <tr><td>Uživatelské jméno HDInsight obrázku</td><td>$clusterUsername</td><td></td><td>HDInsight clusteru uživatelské jméno. </td></tr>
    <tr><td>HDInsight clusteru uživatelského hesla </td><td>$clusterPassword</td><td></td><td>Heslo uživatele clusteru HDInsight.</td></tr>
    <tr><td>Název účtu úložiště Azure</td><td>$storageAccountName</td><td></td><td>Azure úložiště účet k dispozici pro clusteru HDInsight. Pro účely tohoto návodu použijte výchozí účet úložiště, kterou jste zadali při poskytování obrázku.</td></tr>
    <tr><td>Azure jméno container objektů Blob</td><td>$containerName</td><td></td><td>V tomto příkladu použití kontejneru úložiště objektů Blob Azure, který se používá k systému souborů výchozí HDInsight obrázku. Ve výchozím nastavení má stejný název jako obrázku HDInsight.</td></tr>
    </table>

- **Databáze SQL Azure**. Musíte nakonfigurovat pravidla brány firewall pro databáze SQL serveru přístupu v počítači. Pokyny o vytváření Azure SQL databáze a konfigurace brány firewall, najdete v článku [Začínáme s používáním databáze Azure SQL][sqldatabase-get-started]. Tento článek obsahuje skriptu prostředí Windows PowerShell pro vytvoření tabulky databáze Azure SQL, které potřebujete pro účely tohoto návodu.

    <table border = "1">
    <tr><th>Vlastnosti databáze SQL</th><th>Název proměnné prostředí Windows PowerShell</th><th>Hodnota</th><th>Popis</th></tr>
    <tr><td>Název databázového serveru SQL</td><td>$sqlDatabaseServer</td><td></td><td>Databáze SQL serveru, ke kterému Sqoop exportovat data. </td></tr>
    <tr><td>Přihlašovací jméno SQL databáze</td><td>$sqlDatabaseLogin</td><td></td><td>Databáze SQL přihlašovací jméno.</td></tr>
    <tr><td>Přihlašovacího hesla pro SQL databáze</td><td>$sqlDatabaseLoginPassword</td><td></td><td>Databáze SQL přihlašovacího hesla.</td></tr>
    <tr><td>Název databáze SQL</td><td>$sqlDatabaseName</td><td></td><td>Databáze Azure SQL, ke kterému Sqoop exportovat data. </td></tr>
    </table>

    > [AZURE.NOTE] Ve výchozím nastavení databáze Azure SQL umožňuje připojení ze služby Azure, jako je Azure HDInsight. Pokud je toto nastavení brány firewall zakázané, je nutné jej povolit z portálu Microsoft Azure. Pokyny o vytvoření databáze SQL a konfiguraci pravidla brány firewall naleznete v tématu [Vytvoření a konfigurace databáze SQL][sqldatabase-get-started].


> [AZURE.NOTE] Vyplnit hodnoty v tabulkách. Může být vhodné pro filtrované pomocí tohoto kurzu.


##<a name="define-oozie-workflow-and-the-related-hiveql-script"></a>Definování pracovního postupu Oozie a související HiveQL skriptu

Definice pracovních postupů Oozie jsou napsané v hPDL (obrázku definice jazyk XML). Výchozí název souboru pracovního postupu je *workflow.xml*.  Bude uložit místně soubor pracovního postupu a potom ho nasadit do obrázku HDInsight pomocí prostředí PowerShell Azure dál v tomto kurzu.

Soubor skriptu HiveQL hovorů podregistru akce v pracovním postupu. Tento soubor skriptu obsahuje tři HiveQL příkazy:

1. **Pro tabulku PŘETÁHNOUT údajů** odstraní tabulku podregistru log4j, pokud existuje.
2. **Příkaz CREATE TABLE** vytvoří log4j podregistru externí tabulky, která odkazuje na umístění souboru protokolu log4j;
3.  **Umístění souboru protokolu log4j**. Oddělovač je ",". Výchozím oddělovačem řádek je "\n". Externí tabulku podregistru slouží k vyhnout datový soubor odebraná z původního umístění, když chcete spustit pracovní postup Oozie tisknutím.
3. **Příkaz pro vložení PŘEPSAT** počet výskytů jednotlivých typů úroveň protokolování z tabulku podregistru log4j a ukládání výstupu do polohy úložiště objektů Blob Azure.

**Poznámka**: je známý problém cestu podregistru. Tento problém se nastat po odeslání Oozie úlohy. Pokyny k řešení problému najdete na webu TechNet Wiki: [HDInsight podregistru chyb: nelze přejmenovat][technetwiki-hive-error].

**Definování soubor skriptu HiveQL volání pracovního postupu**

1. Vytvořte textový soubor s obsahem takto:

        DROP TABLE ${hiveTableName};
        CREATE EXTERNAL TABLE ${hiveTableName}(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
        INSERT OVERWRITE DIRECTORY '${hiveOutputFolder}' SELECT t4 AS sev, COUNT(*) AS cnt FROM ${hiveTableName} WHERE t4 LIKE '[%' GROUP BY t4;

    Existují ve skriptu používá třemi proměnnými:

    - ${hiveTableName}
    - ${hiveDataFolder}
    - ${hiveOutputFolder}

    Soubor definice pracovního postupu (workflow.xml v tomto kurzu) předá tyto hodnoty tohoto skriptu HiveQL za běhu.

2. Uložte soubor jako **C:\Tutorials\UseOozie\useooziewf.hql** pomocí kódování ANSI (ASCII). (Poznámkový blok v případě použití textovém editoru nenabízí možnost, tato volba.) Tento soubor skriptu má být nasazené na obrázku HDInsight dál v tomto kurzu.



**Definování pracovního postupu**

1. Vytvořte textový soubor s obsahem takto:

        <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
            <start to = "RunHiveScript"/>

            <action name="RunHiveScript">
                <hive xmlns="uri:oozie:hive-action:0.2">
                    <job-tracker>${jobTracker}</job-tracker>
                    <name-node>${nameNode}</name-node>
                    <configuration>
                        <property>
                            <name>mapred.job.queue.name</name>
                            <value>${queueName}</value>
                        </property>
                    </configuration>
                    <script>${hiveScript}</script>
                    <param>hiveTableName=${hiveTableName}</param>
                    <param>hiveDataFolder=${hiveDataFolder}</param>
                    <param>hiveOutputFolder=${hiveOutputFolder}</param>
                </hive>
                <ok to="RunSqoopExport"/>
                <error to="fail"/>
            </action>

            <action name="RunSqoopExport">
                <sqoop xmlns="uri:oozie:sqoop-action:0.2">
                    <job-tracker>${jobTracker}</job-tracker>
                    <name-node>${nameNode}</name-node>
                    <configuration>
                        <property>
                            <name>mapred.compress.map.output</name>
                            <value>true</value>
                        </property>
                    </configuration>
                <arg>export</arg>
                <arg>--connect</arg>
                <arg>${sqlDatabaseConnectionString}</arg>
                <arg>--table</arg>
                <arg>${sqlDatabaseTableName}</arg>
                <arg>--export-dir</arg>
                <arg>${hiveOutputFolder}</arg>
                <arg>-m</arg>
                <arg>1</arg>
                <arg>--input-fields-terminated-by</arg>
                <arg>"\001"</arg>
                </sqoop>
                <ok to="end"/>
                <error to="fail"/>
            </action>

            <kill name="fail">
                <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
            </kill>

           <end name="end"/>
        </workflow-app>

    Existují dvě akce definované v pracovním postupu. Zahájení k akce je *RunHiveScript*. Pokud akce získáte *OK*příští akcí má *RunSqoopExport*.

    RunHiveScript má několik proměnných. Hodnoty předá při odeslání Oozie úlohy v počítači pomocí prostředí PowerShell Azure.

    <table border = "1">
    <tr><th>Proměnné pracovního postupu</th><th>Popis</th></tr>
    <tr><td>${jobTracker}</td><td>Zadejte adresu URL sledování Hadoop projektu. Použití <strong>jobtrackerhost:9010</strong> na HDInsight clusteru verze 3.0 a 2.0.</td></tr>
    <tr><td>${nameNode}</td><td>Zadejte adresu URL uzel Hadoop název. Použití wasbs systému souborů výchozí: / / adresu, například <i>wasbs: / /&lt;Název_kontejneru&gt;@&lt;storageAccountName&gt;. blob.core.windows.net</i>.</td></tr>
    <tr><td>${název_fronty}</td><td>Určuje název fronty úlohy se odesílání do. Použijte <strong>výchozí</strong>.</td></tr>
    </table>


    <table border = "1">
    <tr><th>Proměnná podregistru akce</th><th>Popis</th></tr>
    <tr><td>${hiveDataFolder}</td><td>Zdrojový adresář příkazu tabulku podregistru vytvořit.</td></tr>
    <tr><td>${hiveOutputFolder}</td><td>Výstupní složce pro příkazu Vložit PŘEPSAT.</td></tr>
    <tr><td>${hiveTableName}</td><td>Název tabulku podregistru, který odkazuje log4j datové soubory.</td></tr>
    </table>


    <table border = "1">
    <tr><th>Proměnná Sqoop akce</th><th>Popis</th></tr>
    <tr><td>${sqlDatabaseConnectionString}</td><td>Připojovací řetězec SQL databáze.</td></tr>
    <tr><td>${sqlDatabaseTableName}</td><td>Tabulky databáze Azure SQL na místo, kam bude možné exportovat data.</td></tr>
    <tr><td>${hiveOutputFolder}</td><td>Výstupní složce pro výkaz podregistru vložit PŘEPSAT. Toto je stejné složky pro export Sqoop (export dir).</td></tr>
    </table>

    Další informace o pracovní postup Oozie a práce s akcemi pracovního postupu najdete v článku [si přečtěte následující dokumentaci Apache Oozie 4.0] [ apache-oozie-400] (pro HDInsight clusteru verze 3.0) nebo [si přečtěte následující dokumentaci Apache Oozie 3.3.2] [ apache-oozie-332] (pro HDInsight clusteru verze 2.1).

2. Uložte soubor jako **C:\Tutorials\UseOozie\workflow.xml** pomocí kódování ANSI (ASCII). (Poznámkový blok v případě použití textovém editoru nenabízí možnost, tato volba.)

**Definování koordinátor**

1. Vytvořte textový soubor s obsahem takto:

        <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
           <action>
              <workflow>
                 <app-path>${wfPath}</app-path>
              </workflow>
           </action>
        </coordinator-app>

    Existuje pět proměnných v souboru definice:

  	| Proměnná          | Popis |
  	| ------------------|------------ |
  	| ${coordFrequency} | Doba pozastavit. Četnost je vždy vyjádřený v minutách. |
  	| ${coordStart}     | Čas spuštění úlohy. |
  	| ${coordEnd}       | Čas ukončení projektu. |
  	| ${coordTimezone}  | Oozie zpracuje koordinátor úlohy v pevné časového pásma s žádné letní čas (obvykle představované pomocí UTC). Časové pásmo se označuje jako "zpracovávání Oozie podle časového pásma." |
  	| ${wfPath}         | Cesta k workflow.xml.  Název souboru pracovního postupu není výchozí název souboru (workflow.xml), je nutné zadat. |

2. Uložte soubor jako **C:\Tutorials\UseOozie\coordinator.xml** pomocí kódování ANSI (ASCII). (Poznámkový blok v případě použití textovém editoru nenabízí možnost, tato volba.)

##<a name="deploy-the-oozie-project-and-prepare-the-tutorial"></a>Nasazení projektu Oozie a připravit kurzu

Spustí skriptu prostředí PowerShell Azure proveďte následující kroky:

- Zkopírujte skript HiveQL (useoozie.hql) k úložišti objektů Blob Azure wasbs:///tutorials/useoozie/useoozie.hql.
- Zkopírujte workflow.xml wasbs:///tutorials/useoozie/workflow.xml.
- Zkopírujte coordinator.xml wasbs:///tutorials/useoozie/coordinator.xml.
- Zkopíroval soubor dat (/ example/data/sample.log) do wasbs:///tutorials/useoozie/data/sample.log.
- Vytvoření tabulky databáze Azure SQL pro ukládání Sqoop exportovat data. Název tabulky je *log4jLogCount*.

**Princip HDInsight úložiště**

Úložiště objektů Blob Azure HDInsight používá pro ukládání dat. wasbs: / / je společnosti Microsoft zavádění systému Hadoop distributed souboru (HDFS) v úložišti objektů Blob Azure. Další informace najdete v tématu [úložiště objektů Blob Azure použití s HDInsight][hdinsight-storage].

Když zřizujete HDInsight obrázku, účtu úložiště objektů Blob Azure a konkrétní kontejneru z tohoto účtu označen jako výchozí systém souborů, jako je v HDFS. Kromě tohoto účtu úložiště můžete přidat další úložiště účty z stejné Azure předplatné nebo z různých Azure předplatných během procesu zřizovací. Pokyny pro přidání dalšího úložiště účtů najdete v článku [poskytnutí HDInsight clusterů][hdinsight-provision]. Pro zjednodušení skript Powershellu Azure použitý v tomto kurzu, všechny soubory uložené v kontejner systému souborů výchozí c:\ */tutorials/useoozie*. Ve výchozím nastavení obsahuje tento kontejner stejný název jako název clusteru HDInsight.
Syntaxe je:

    wasb[s]://<ContainerName>@<StorageAccountName>.blob.core.windows.net/<path>/<filename>

> [AZURE.NOTE] Pouze *wasb: / /* syntaxe je podporované v HDInsight clusteru verze 3.0. Starší *objemových: / /* syntaxe je podporované v HDInsight 2.1 a 1,6 clusterů, ale není podporováno v clusterů HDInsight 3.0.

> [AZURE.NOTE] Wasb: / / cesta je virtuální cestu. Další informace najdete v článku [úložiště objektů Blob Azure použití s HDInsight][hdinsight-storage].

Soubor, který je uložený v kontejneru výchozí soubor systému můžete k nim získat přístup z Hdinsightu pomocí některého z těchto věcí identifikátory URI (používám workflow.xml jako příklad):

    wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/workflow.xml
    wasbs:///tutorials/useoozie/workflow.xml
    /tutorials/useoozie/workflow.xml

Podle potřeby můžete získat přístup k souboru přímo z účtu úložiště objektů blob název souboru je:

    tutorials/useoozie/workflow.xml

**Princip tabulek interním a externím podregistru**

Které byste měli vědět o interní a externí tabulku podregistru několik věcí:

- Příkaz CREATE TABLE vytvoří vnitřní tabulku, nazývaný také spravovaných tabulky. Datový soubor nacházet v kontejneru výchozí.
- Příkaz CREATE TABLE přesune datový soubor /hive/sklad/<TableName> složek kontejneru výchozí.
- Příkaz Vytvořit externí tabulka vytvoří externí tabulku. Datový soubor mohou být umístěny mimo výchozí kontejner.
- Příkaz CREATE TABLE externích nepřesune datového souboru.
- Příkaz Vytvořit externí tabulka neumožňuje podsložek ve složce, uvedená v klauzuli umístění. Toto je důvod, proč kurzu vytvoří kopii souboru sample.log.

Další informace najdete v tématu [HDInsight: podregistru interní a externí tabulky Úvod][cindygross-hive-tables].

**Příprava na kurz**

1. Otevřete Windows PowerShell ISE (na obrazovce Start ve Windows 8 zadejte **PowerShell_ISE**a klikněte na **ISE v prostředí Windows PowerShell**. Další informace najdete v tématu [Spuštění prostředí Windows PowerShell ve Windows 8 a Windows][powershell-start]).
2. V dolním podokně spusťte tento příkaz připojit k předplatnému Azure:

        Add-AzureAccount

    Zobrazí se výzva k zadání přihlašovacích údajů účet Azure. Tento způsob přidání připojení k předplatné vyprší její časový limit a po 12 hodin, budete muset znovu spusťte rutinu.

    > [AZURE.NOTE] Pokud máte víc předplatných Azure a předplatné výchozí není tu, kterou chcete použít, získáte pomocí rutiny <strong>Vyberte AzureSubscription</strong> vyberte předplatné.

3. Zkopírujte následující skript do podokna Skript a pak nastavte prvních šest proměnné:

        # WASB variables
        $storageAccountName = "<StorageAccountName>"
        $containerName = "<BlobStorageContainerName>"

        # SQL database variables
        $sqlDatabaseServer = "<SQLDatabaseServerName>"  
        $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
        $sqlDatabaseLoginPassword = "SQLDatabaseLoginPassword>"
        $sqlDatabaseName = "<SQLDatabaseName>"  
        $sqlDatabaseTableName = "log4jLogsCount"

        # Oozie files for the tutorial
        $hiveQLScript = "C:\Tutorials\UseOozie\useooziewf.hql"
        $workflowDefinition = "C:\Tutorials\UseOozie\workflow.xml"
        $coordDefinition =  "C:\Tutorials\UseOozie\coordinator.xml"

        # WASB folder for storing the Oozie tutorial files.
        $destFolder = "tutorials/useoozie"  # Do NOT use the long path here


    Další popisy proměnných naleznete v části [požadavky](#prerequisites) v tomto kurzu.

3. Připojte následující skript v podokně skript:

        # Create a storage context object
        $storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

        function uploadOozieFiles()
        {
            Write-Host "Copy HiveQL script, workflow definition and coordinator definition ..." -ForegroundColor Green
            Set-AzureStorageBlobContent -File $hiveQLScript -Container $containerName -Blob "$destFolder/useooziewf.hql" -Context $destContext
            Set-AzureStorageBlobContent -File $workflowDefinition -Container $containerName -Blob "$destFolder/workflow.xml" -Context $destContext
            Set-AzureStorageBlobContent -File $coordDefinition -Container $containerName -Blob "$destFolder/coordinator.xml" -Context $destContext
        }

        function prepareHiveDataFile()
        {
            Write-Host "Make a copy of the sample.log file ... " -ForegroundColor Green
            Start-CopyAzureStorageBlob -SrcContainer $containerName -SrcBlob "example/data/sample.log" -Context $destContext -DestContainer $containerName -destBlob "$destFolder/data/sample.log" -DestContext $destContext
        }

        function prepareSQLDatabase()
        {
            # SQL query string for creating log4jLogsCount table
            $cmdCreateLog4jCountTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                    [Level] [nvarchar](10) NOT NULL,
                    [Total] float,
                CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
                (
                [Level] ASC
                )
                )"

            #Create the log4jLogsCount table
            Write-Host "Create Log4jLogsCount table ..." -ForegroundColor Green
            $conn = New-Object System.Data.SqlClient.SqlConnection
            $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
            $conn.open()
            $cmd = New-Object System.Data.SqlClient.SqlCommand
            $cmd.connection = $conn
            $cmd.commandtext = $cmdCreateLog4jCountTable
            $cmd.executenonquery()

            $conn.close()
        }

        # upload workflow.xml, coordinator.xml, and ooziewf.hql
        uploadOozieFiles;

        # make a copy of example/data/sample.log to example/data/log4j/sample.log
        prepareHiveDataFile;

        # create log4jlogsCount table on SQL database
        prepareSQLDatabase;

4. Klikněte na **Spustit skript** nebo stiskněte **F5** následujícím způsobem. Výstup bude vypadat podobně jako:

    ![Výuková Příprava výstup][img-preparation-output]

##<a name="run-the-oozie-project"></a>Spuštění Oozie projektu

Azure Powershellu aktuálně nenabízí možnost, všechny rutiny pro definování Oozie úloh. Můžete použít rutinu **Vyvolat RestMethod** vyvolat Oozie webové služby. Rozhraní API webových služeb Oozie je rozhraní API HTTP ZBÝVAJÍCÍ JSON. Další informace o webové služby Oozie rozhraní API, najdete v článku [si přečtěte následující dokumentaci Apache Oozie 4.0] [ apache-oozie-400] (pro HDInsight clusteru verze 3.0) nebo [si přečtěte následující dokumentaci Apache Oozie 3.3.2] [ apache-oozie-332] (pro HDInsight clusteru verze 2.1).

**Odeslání Oozie projektu**

1. Otevřete Windows PowerShell ISE (na obrazovce Start ve Windows 8 zadejte **PowerShell_ISE**a klikněte na **ISE v prostředí Windows PowerShell**. Další informace najdete v tématu [Spuštění prostředí Windows PowerShell ve Windows 8 a Windows][powershell-start]).

3. Zkopírujte následující skript do podokna Skript a pak nastavte nejdřív čtrnáct proměnné (však přeskočte **$storageUri**).

        #HDInsight cluster variables
        $clusterName = "<HDInsightClusterName>"
        $clusterUsername = "<HDInsightClusterUsername>"
        $clusterPassword = "<HDInsightClusterUserPassword>"

        #Azure Blob storage (WASB) variables
        $storageAccountName = "<StorageAccountName>"
        $storageContainerName = "<BlobContainerName>"
        $storageUri="wasbs://$storageContainerName@$storageAccountName.blob.core.windows.net"

        #Azure SQL database variables
        $sqlDatabaseServer = "<SQLDatabaseServerName>"
        $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
        $sqlDatabaseLoginPassword = "<SQLDatabaseloginPassword>"
        $sqlDatabaseName = "<SQLDatabaseName>"  

        #Oozie WF/coordinator variables
        $coordStart = "2014-03-21T13:45Z"
        $coordEnd = "2014-03-21T13:45Z"
        $coordFrequency = "1440"    # in minutes, 24h x 60m = 1440m
        $coordTimezone = "UTC"  #UTC/GMT

        $oozieWFPath="$storageUri/tutorials/useoozie"  # The default name is workflow.xml. And you don't need to specify the file name.
        $waitTimeBetweenOozieJobStatusCheck=10

        #Hive action variables
        $hiveScript = "$storageUri/tutorials/useoozie/useooziewf.hql"
        $hiveTableName = "log4jlogs"
        $hiveDataFolder = "$storageUri/tutorials/useoozie/data"
        $hiveOutputFolder = "$storageUri/tutorials/useoozie/output"

        #Sqoop action variables
        $sqlDatabaseConnectionString = "jdbc:sqlserver://$sqlDatabaseServer.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServer;password=$sqlDatabaseLoginPassword;database=$sqlDatabaseName"
        $sqlDatabaseTableName = "log4jLogsCount"

        $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
        $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    Další popisy proměnných naleznete v části [požadavky](#prerequisites) v tomto kurzu.

    $coordstart a $coordend jsou pracovního postupu počáteční a koncový čas. Pokud chcete zjistit, UTC a časem, vyhledejte "času utc" na bing.com. $CoordFrequency intervalu probíhá minut, který chcete spustit pracovní postup.

3. Připojte následující skriptu. Tato část definuje datové Oozie:

        #OoziePayload used for Oozie web service submission
        $OoziePayload =  @"
        <?xml version="1.0" encoding="UTF-8"?>
        <configuration>

           <property>
               <name>nameNode</name>
               <value>$storageUrI</value>
           </property>

           <property>
               <name>jobTracker</name>
               <value>jobtrackerhost:9010</value>
           </property>

           <property>
               <name>queueName</name>
               <value>default</value>
           </property>

           <property>
               <name>oozie.use.system.libpath</name>
               <value>true</value>
           </property>

           <property>
               <name>oozie.coord.application.path</name>
               <value>$oozieWFPath</value>
           </property>

           <property>
               <name>wfPath</name>
               <value>$oozieWFPath</value>
           </property>

           <property>
               <name>coordStart</name>
               <value>$coordStart</value>
           </property>

           <property>
               <name>coordEnd</name>
               <value>$coordEnd</value>
           </property>

           <property>
               <name>coordFrequency</name>
               <value>$coordFrequency</value>
           </property>

           <property>
               <name>coordTimezone</name>
               <value>$coordTimezone</value>
           </property>

           <property>
               <name>hiveScript</name>
               <value>$hiveScript</value>
           </property>

           <property>
               <name>hiveTableName</name>
               <value>$hiveTableName</value>
           </property>

           <property>
               <name>hiveDataFolder</name>
               <value>$hiveDataFolder</value>
           </property>

           <property>
               <name>hiveOutputFolder</name>
               <value>$hiveOutputFolder</value>
           </property>

           <property>
               <name>sqlDatabaseConnectionString</name>
               <value>&quot;$sqlDatabaseConnectionString&quot;</value>
           </property>

           <property>
               <name>sqlDatabaseTableName</name>
               <value>$SQLDatabaseTableName</value>
           </property>

           <property>
               <name>user.name</name>
               <value>admin</value>
           </property>

        </configuration>
        "@

    >[AZURE.NOTE] Hlavní rozdíl v porovnání s pracovní postup odeslání datovém souboru je proměnná **oozie.coord.application.path**. Po odeslání úlohy pracovního postupu použijete **oozie.wf.application.path** místo.

4. Připojte následující skriptu. Tato část kontroly stavu Oozie webové služby:

        function checkOozieServerStatus()
        {
            Write-Host "Checking Oozie server status..." -ForegroundColor Green
            $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/admin/status"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds -OutVariable $OozieServerStatus

            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $oozieServerSatus = $jsonResponse[0].("systemMode")
            Write-Host "Oozie server status is $oozieServerSatus..."

            if($oozieServerSatus -notmatch "NORMAL")
            {
                Write-Host "Oozie server status is $oozieServerSatus...cannot submit Oozie jobs. Check the server status and re-run the job."
                exit 1
            }
        }

5. Připojte následující skriptu. Tato část vytvoří Oozie úlohy:

        function createOozieJob()
        {
            # create Oozie job
            Write-Host "Sending the following Payload to the cluster:" -ForegroundColor Green
            Write-Host "`n--------`n$OoziePayload`n--------"
            $clusterUriCreateJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
            $response = Invoke-RestMethod -Method Post -Uri $clusterUriCreateJob -Credential $creds -Body $OoziePayload -ContentType "application/xml" -OutVariable $OozieJobName -debug -Verbose

            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $oozieJobId = $jsonResponse[0].("id")
            Write-Host "Oozie job id is $oozieJobId..."

            return $oozieJobId
        }

    > [AZURE.NOTE] Když odešlete úlohy pracovního postupu, musíte podat volání úlohu spustíte po vytvoření úlohy jiné webové služby. V tomto případě úlohy koordinátor spouštěný kliknutím čas. Úkoly se spustí automaticky.

6. Připojte následující skriptu. Tato část kontroly stavu úloh Oozie:

        function checkOozieJobStatus($oozieJobId)
        {
            # get job status
            Write-Host "Sleeping for $waitTimeBetweenOozieJobStatusCheck seconds until the job metadata is populated in the Oozie metastore..." -ForegroundColor Green
            Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck

            Write-Host "Getting job status and waiting for the job to complete..." -ForegroundColor Green
            $clusterUriGetJobStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?show=info"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $JobStatus = $jsonResponse[0].("status")

            while($JobStatus -notmatch "SUCCEEDED|KILLED")
            {
                Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state...waiting $waitTimeBetweenOozieJobStatusCheck seconds for the job to complete..."
                Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck
                $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
                $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
                $JobStatus = $jsonResponse[0].("status")
            }

            Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state!"
            if($JobStatus -notmatch "SUCCEEDED")
            {
                Write-Host "Check logs at http://headnode0:9014/cluster for detais."
                exit -1
            }
        }

7. (Volitelné) Připojte následující skriptu.

        function listOozieJobs()
        {
            Write-Host "Listing Oozie jobs..." -ForegroundColor Green
            $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds

            write-host "Job ID                                   App Name        Status      Started                         Ended"
            write-host "----------------------------------------------------------------------------------------------------------------------------------"
            foreach($job in $response.workflows)
            {
                Write-Host $job.id "`t" $job.appName "`t" $job.status "`t" $job.startTime "`t" $job.endTime
            }
        }

        function ShowOozieJobLog($oozieJobId)
        {
            Write-Host "Showing Oozie job info..." -ForegroundColor Green
            $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/$oozieJobId" + "?show=log"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds
            write-host $response
        }

        function killOozieJob($oozieJobId)
        {
            Write-Host "Killing the Oozie job $oozieJobId..." -ForegroundColor Green
            $clusterUriStartJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?action=kill" #Valid values for the 'action' parameter are 'start', 'suspend', 'resume', 'kill', 'dryrun', 'rerun', and 'change'.
            $response = Invoke-RestMethod -Method Put -Uri $clusterUriStartJob -Credential $creds | Format-Table -HideTableHeaders -debug
        }

7. Připojte následující skript:

        checkOozieServerStatus
        # listOozieJobs
        $oozieJobId = createOozieJob($oozieJobId)
        checkOozieJobStatus($oozieJobId)
        # ShowOozieJobLog($oozieJobId)
        # killOozieJob($oozieJobId)

    Pokud chcete spustit další funkce odeberte značky #.

7. Pokud svůj cluster HDinsight je verze 2.1, nahraďte připojení "https://$clusterName.azurehdinsight.net:443/oozie/v2/" s "https://$clusterName.azurehdinsight.net:443/oozie/v1/". HDInsight clusteru verze 2.1 znamená není podporuje verze 2 webové služby.

7. Klikněte na **Spustit skript** nebo stiskněte **F5** následujícím způsobem. Výstup bude vypadat podobně jako:

    ![Kurz spustit výstup pracovního postupu][img-runworkflow-output]

8. Připojení k databázi SQL zobrazíte exportovaná data.

**Chcete-li zkontrolovat protokol chyb projektu**

Řešení problémů s pracovního postupu, soubor protokolu Oozie najdete na C:\apps\dist\oozie-3.3.2.1.3.2.0-05\oozie-win-distro\logs\Oozie.log z headnode obrázku. Informace o RDP, najdete v článku [Správa HDInsight clusterů pomocí portálu Azure][hdinsight-admin-portal].

**Chcete-li znovu spustit kurzu**

Chcete-li znovu spustit pracovní postup, musíte provádět následující úkoly:

- Odstranění výstupní soubor skriptu podregistru.
- Odstranění dat v tabulce log4jLogsCount.

Tady je ukázka skript prostředí Windows PowerShell, který můžete použít:

    $storageAccountName = "<AzureStorageAccountName>"
    $containerName = "<ContainerName>"

    #SQL database variables
    $sqlDatabaseServer = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"
    $sqlDatabaseTableName = "log4jLogsCount"

    Write-host "Delete the Hive script output file ..." -ForegroundColor Green
    $storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey
    Remove-AzureStorageBlob -Context $destContext -Blob "tutorials/useoozie/output/000000_0" -Container $containerName

    Write-host "Delete all the records from the log4jLogsCount table ..." -ForegroundColor Green
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
    $conn.open()
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.connection = $conn
    $cmd.commandtext = "delete from $sqlDatabaseTableName"
    $cmd.executenonquery()

    $conn.close()


##<a name="next-steps"></a>Další kroky
V tomto kurzu jste se naučili definování pracovního postupu Oozie a Oozie koordinátor a k tomu úlohy koordinátor Oozie pomocí prostředí PowerShell Azure. Další informace naleznete v následujících článcích:

- [Začínáme s HDInsight][hdinsight-get-started]
- [Pomocí úložiště objektů Blob Azure HDInsight][hdinsight-storage]
- [Spravovat pomocí prostředí PowerShell Azure HDInsight][hdinsight-admin-powershell]
- [Odeslání dat do HDInsight][hdinsight-upload-data]
- [Použití Sqoop s HDInsight][hdinsight-use-sqoop]
- [Použití podregistru s HDInsight][hdinsight-use-hive]
- [Použití Prasátko s HDInsight][hdinsight-use-pig]
- [Můžete vyvíjet aplikace Java MapReduce pro HDInsight][hdinsight-develop-java-mapreduce]



[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563


[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md


[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-develop-java-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md

[sqldatabase-get-started]: ../sql-database/sql-database-get-started.md

[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: ../storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: ../powershell-install-configure.md
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Preparation.Output1.png  
[img-runworkflow-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.RunCoord.Output.png  

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
