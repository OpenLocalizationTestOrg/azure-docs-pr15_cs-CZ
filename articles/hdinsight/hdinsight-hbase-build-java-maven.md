<properties
pageTitle="Vytvoření aplikace HBase přes Maven a nasazení do HDInsight serveru s Windows | Microsoft Azure"
description="Naučte se používat Apache Maven vytvořit na základě Java HBase Apache aplikaci a pak ho nasadit do clusteru serveru s Windows Azure HDInsight."
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
ms.date="10/03/2016"
ms.author="larryfr"/>

#<a name="use-maven-to-build-java-applications-that-use-hbase-with-windows-based-hdinsight-hadoop"></a>Použít Maven k vytvoření Java aplikace, které používají HBase se systémem Windows HDInsight (Hadoop)

Naučte se vytvářet a vytváření [Apache HBase](http://hbase.apache.org/) aplikaci Java pomocí Apache Maven. Potom použijte aplikaci MONTI s Azure HDInsight (Hadoop).

[Maven](http://maven.apache.org/) je software řízení projektů a porozumění informacím nástroj, který umožňuje vytvářet software, si přečtěte následující dokumentaci a sestav pro Java projekty. V tomto článku se naučíte, jak se používá k vytvoření základní Java aplikace, který slouží k vytváření, dotazů a odstraní tabulku HBase clusteru Azure HDInsight.

> [AZURE.NOTE] Kroky v tomto dokumentu se předpokládá, že používáte clusteru serveru s Windows HDInsight. Informace o použití na základě Linux HDInsight obrázku najdete v tématu [Maven použít k vytváření Java aplikací používajících HBase se systémem Linux HDInsight](hdinsight-hbase-build-java-maven-linux.md)

##<a name="requirements"></a>Požadavky

* [Platformu Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 nebo novější

* [Maven](http://maven.apache.org/)


* [Clusteru serveru s Windows HDInsight s HBase](hdinsight-hbase-tutorial-get-started.md#create-hbase-cluster)


    > [AZURE.NOTE] Kroky v tomto dokumentu bylo testováno se verze obrázku HDInsight 3,2 a 3.3. Výchozí hodnoty zadané v příkladech se pro cluster HDInsight 3.3.

##<a name="create-the-project"></a>Vytvoření projektu

1. Z příkazového řádku v prostředí vývoj, změňte adresáře do umístění, ve které chcete vytvořit projekt, například `cd code\hdinsight`.

2. Příkazem __mvn__ , který se instaluje s Maven, generovat vygenerovaných projektu.

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    Tento příkaz vytvoří v adresáři v aktuálním umístění s názvem určené parametrem __artifactID__ (**hbaseapp** v tomto příkladu.) Tento adresář obsahuje následující položky:

    * __pom.XML__: objektovému modelu projektu ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) obsahuje podrobnosti o informace a konfigurace slouží k vytváření projektu.

    * __src__: adresář, který obsahuje __main\java\com\microsoft\examples__ adresáře, kde se vytvářet aplikace.

3. Odstraňte soubor __src\test\java\com\microsoft\examples\apptest.java__ , protože není v tomto příkladě použita.

##<a name="update-the-project-object-model"></a>Aktualizace objektovému modelu projektu

1. Upravte soubor __pom.xml__ a přidejte následující kód do `<dependencies>` oddíl:

        <dependency>
          <groupId>org.apache.hbase</groupId>
          <artifactId>hbase-client</artifactId>
          <version>1.1.2</version>
        </dependency>

    V této části, bude Maven projekt vyžaduje __hbase klienta__ verze __1.1.2__. V době kompilace se tato závislost stahují z úložiště Maven výchozí. Další informace o tomto závislost můžete [Maven centrálním úložišti vyhledávání](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) .

    > [AZURE.IMPORTANT] Číslo verze se musí shodovat verzi HBase, která je součástí svůj cluster HDInsight. Použijte v následující tabulce najdete na správné číslo verze.

  	| HDInsight verze obrázku | Verze HBase se má použít |
  	| ----- | ----- |
  	| 3,2 | 0.98.4-hadoop2 |
  	| 3.3 | 1.1.2 |

    Další informace o verzích HDInsight a součásti [se](hdinsight-component-versioning.md)podívejte jiné součásti Hadoop dostupné s HDInsight.

2. Pokud používáte HDInsight 3.3 clusteru, musíte taky přidáte podle následujících pokynů `<dependencies>` oddíl:

        <dependency>
            <groupId>org.apache.phoenix</groupId>
            <artifactId>phoenix-core</artifactId>
            <version>4.4.0-HBase-1.1</version>
        </dependency>
    
    Tato závislost načte phoenix základní součásti, které využívají Hbase verze 1.1.x.

2. Přidejte následující kód k souboru __pom.xml__ . V této části musí být uvnitř `<project>...</project>` značky v souboru, například mezi `</dependencies>` a `</project>`.

        <build>
          <sourceDirectory>src</sourceDirectory>
          <resources>
              <resource>
                <directory>${basedir}/conf</directory>
                <filtering>false</filtering>
                <includes>
                  <include>hbase-site.xml</include>
                </includes>
              </resource>
            </resources>
          <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.3</version>
                <configuration>
                    <source>1.7</source>
                    <target>1.7</target>
                </configuration>
              </plugin>
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-shade-plugin</artifactId>
              <version>2.3</version>
              <configuration>
                <transformers>
                  <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                    </transformer>
                  </transformers>
              </configuration>
              <executions>
                <execution>
                  <phase>package</phase>
                  <goals>
                    <goal>shade</goal>
                  </goals>
                </execution>
              </executions>
            </plugin>
          </plugins>
        </build>

    `<resources>` Oddíl nakonfiguruje zdroj (__conf\hbase site.xml__), který obsahuje informace o konfiguraci pro HBase.

    > [AZURE.NOTE] Můžete také nastavit konfigurace hodnoty pomocí kódu. Zobrazit komentáře v následujícím příkladu __CreateTable__ pro školá.

    To `<plugins>` oddíl nakonfiguruje [Maven vystínovat plug-in](http://maven.apache.org/plugins/maven-shade-plugin/)a [Modul plug-in kompilátoru Maven](http://maven.apache.org/plugins/maven-compiler-plugin/) . Modul plug-in kompilátoru slouží k kompilace topologii. Modul plug-in stín slouží k zabránit duplikace licence, která je integrovaná Maven balíček SKLENICE. Důvod, proč tato možnost slouží je, aby duplicitní licenčních souborů mít za následek chybu za běhu clusteru HDInsight. Použití maven stín-modul plug-in se `ApacheLicenseResourceTransformer` implementaci zabrání tato chyba.

    Plugin stín maven taky vznikne uber sklenice (nebo tuku sklenice), která obsahuje všechny závislosti vyžaduje aplikace.

3. Uložte soubor __pom.xml__ .

4. Vytvořte nový adresář s názvem __conf__ v adresáři __hbaseapp__ . V adresáři __conf__ vytvořte do souboru nazvaného __hbase site.xml__. Použijte následující jako obsah souboru:

        <?xml version="1.0"?>
        <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
        <!--
        /**
          * Copyright 2010 The Apache Software Foundation
          *
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
        -->
        <configuration>
          <property>
            <name>hbase.cluster.distributed</name>
            <value>true</value>
          </property>
          <property>
            <name>hbase.zookeeper.quorum</name>
            <value>zookeeper0,zookeeper1,zookeeper2</value>
          </property>
          <property>
            <name>hbase.zookeeper.property.clientPort</name>
            <value>2181</value>
          </property>
        </configuration>

    Tento soubor se použijí k načtení HBase konfiguraci clusteru HDInsight.

    > [AZURE.NOTE] Toto je soubor minimální hbase site.xml a obsahuje úplné minimální nastavení clusteru HDInsight.

3. Uložte soubor __hbase site.xml__ .

##<a name="create-the-application"></a>Vytvoření aplikace

1. Přejděte do adresáře __hbaseapp\src\main\java\com\microsoft\examples__ a přejmenujte soubor app.java __CreateTable.java__.

2. Otevřete soubor __CreateTable.java__ a nahradit existující obsah následující kód:

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HBaseAdmin;
        import org.apache.hadoop.hbase.HTableDescriptor;
        import org.apache.hadoop.hbase.TableName;
        import org.apache.hadoop.hbase.HColumnDescriptor;
        import org.apache.hadoop.hbase.client.HTable;
        import org.apache.hadoop.hbase.client.Put;
        import org.apache.hadoop.hbase.util.Bytes;

        public class CreateTable {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Example of setting zookeeper values for HDInsight
            // in code instead of an hbase-site.xml file
            //
            // config.set("hbase.zookeeper.quorum",
            //            "zookeepernode0,zookeepernode1,zookeepernode2");
            //config.set("hbase.zookeeper.property.clientPort", "2181");
            //config.set("hbase.cluster.distributed", "true");
            // The following sets the znode root for Linux-based HDInsight
            //config.set("zookeeper.znode.parent","/hbase-unsecure");

            // create an admin object using the config
            HBaseAdmin admin = new HBaseAdmin(config);

            // create the table...
            HTableDescriptor tableDescriptor = new HTableDescriptor(TableName.valueOf("people"));
            // ... with two column families
            tableDescriptor.addFamily(new HColumnDescriptor("name"));
            tableDescriptor.addFamily(new HColumnDescriptor("contactinfo"));
            admin.createTable(tableDescriptor);

            // define some people
            String[][] people = {
                { "1", "Marcel", "Haddad", "marcel@fabrikam.com"},
                { "2", "Franklin", "Holtz", "franklin@contoso.com" },
                { "3", "Dwayne", "McKee", "dwayne@fabrikam.com" },
                { "4", "Rae", "Schroeder", "rae@contoso.com" },
                { "5", "Rosalie", "burton", "rosalie@fabrikam.com"},
                { "6", "Gabriela", "Ingram", "gabriela@contoso.com"} };

            HTable table = new HTable(config, "people");

            // Add each person to the table
            //   Use the `name` column family for the name
            //   Use the `contactinfo` column family for the email
            for (int i = 0; i< people.length; i++) {
              Put person = new Put(Bytes.toBytes(people[i][0]));
              person.add(Bytes.toBytes("name"), Bytes.toBytes("first"), Bytes.toBytes(people[i][1]));
              person.add(Bytes.toBytes("name"), Bytes.toBytes("last"), Bytes.toBytes(people[i][2]));
              person.add(Bytes.toBytes("contactinfo"), Bytes.toBytes("email"), Bytes.toBytes(people[i][3]));
              table.put(person);
            }
            // flush commits and close the table
            table.flushCommits();
            table.close();
          }
        }

    Toto je třída __CreateTable__ , která vytvoří tabulku s názvem __lidi__ a doplníte někteří uživatelé předdefinované.

3. Uložte soubor __CreateTable.java__ .

4. V adresáři __hbaseapp\src\main\java\com\microsoft\examples__ vytvoření nového souboru s názvem __SearchByEmail.java__. Následující kód použijte jako obsah tohoto souboru:

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HTable;
        import org.apache.hadoop.hbase.client.Scan;
        import org.apache.hadoop.hbase.client.ResultScanner;
        import org.apache.hadoop.hbase.client.Result;
        import org.apache.hadoop.hbase.filter.RegexStringComparator;
        import org.apache.hadoop.hbase.filter.SingleColumnValueFilter;
        import org.apache.hadoop.hbase.filter.CompareFilter.CompareOp;
        import org.apache.hadoop.hbase.util.Bytes;
        import org.apache.hadoop.util.GenericOptionsParser;

        public class SearchByEmail {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Use GenericOptionsParser to get only the parameters to the class
            // and not all the parameters passed (when using WebHCat for example)
            String[] otherArgs = new GenericOptionsParser(config, args).getRemainingArgs();
            if (otherArgs.length != 1) {
              System.out.println("usage: [regular expression]");
              System.exit(-1);
            }

            // Open the table
            HTable table = new HTable(config, "people");

            // Define the family and qualifiers to be used
            byte[] contactFamily = Bytes.toBytes("contactinfo");
            byte[] emailQualifier = Bytes.toBytes("email");
            byte[] nameFamily = Bytes.toBytes("name");
            byte[] firstNameQualifier = Bytes.toBytes("first");
            byte[] lastNameQualifier = Bytes.toBytes("last");

            // Create a new regex filter
            RegexStringComparator emailFilter = new RegexStringComparator(otherArgs[0]);
            // Attach the regex filter to a filter
            //   for the email column
            SingleColumnValueFilter filter = new SingleColumnValueFilter(
              contactFamily,
              emailQualifier,
              CompareOp.EQUAL,
              emailFilter
            );

            // Create a scan and set the filter
            Scan scan = new Scan();
            scan.setFilter(filter);

            // Get the results
            ResultScanner results = table.getScanner(scan);
            // Iterate over results and print  values
            for (Result result : results ) {
              String id = new String(result.getRow());
              byte[] firstNameObj = result.getValue(nameFamily, firstNameQualifier);
              String firstName = new String(firstNameObj);
              byte[] lastNameObj = result.getValue(nameFamily, lastNameQualifier);
              String lastName = new String(lastNameObj);
              System.out.println(firstName + " " + lastName + " - ID: " + id);
              byte[] emailObj = result.getValue(contactFamily, emailQualifier);
              String email = new String(emailObj);
              System.out.println(firstName + " " + lastName + " - " + email + " - ID: " + id);
            }
            results.close();
            table.close();
          }
        }

    Třídy __SearchByEmail__ mohou sloužit k dotazu pro řádky e-mailové adresy. Protože ho používá regulárních výrazů filtru, můžete zadat řetězec nebo regulárních výrazů při použití předmětu.

5. Uložte soubor __SearchByEmail.java__ .

6. V adresáři __hbaseapp\src\main\hava\com\microsoft\examples__ vytvoření nového souboru s názvem __DeleteTable.java__. Následující kód použijte jako obsah tohoto souboru:

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HBaseAdmin;

        public class DeleteTable {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Create an admin object using the config
            HBaseAdmin admin = new HBaseAdmin(config);

            // Disable, and then delete the table
            admin.disableTable("people");
            admin.deleteTable("people");
          }
        }

    Tento předmětu je určený pro čištění v tomto příkladu zakázání a umístěním tabulky vytvořené __CreateTable__ předmětu.

7. Uložte soubor __DeleteTable.java__ .

##<a name="build-and-package-the-application"></a>Vytvoření a balíček aplikace

1. Otevřete příkazový řádek a přejděte v adresáři __hbaseapp__ adresáře.

2. Umožňuje vytvořit SKLENICE soubor, který obsahuje aplikace tento příkaz:

        mvn clean package

    To vyčistí všechny předchozí artefakty Tvůrce dotazů, stáhne nějaké závislosti, které již byly nenainstalovali, potom vytvoří a balíčky aplikace.

3. Po provedení příkazu adresář __hbaseapp\target__ obsahuje do souboru nazvaného __hbaseapp 1.0 SNAPSHOT.jar__.

    > [AZURE.NOTE] Soubor __hbaseapp 1.0 SNAPSHOT.jar__ je uber sklenice (někdy se jí říká tuku sklenice), která obsahuje všechny závislosti potřebné ke spuštění aplikace.

##<a name="upload-the-jar-file-and-start-a-job"></a>Nahrání souboru SKLENICE a zahájení projektu

Způsoby mnoho k nahrání souboru na svůj cluster HDInsight podle popisu v [Odeslat data pro Hadoop projekty v HDInsight](hdinsight-upload-data.md). Následující postup používá Azure Powershellu.

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


1. Po instalaci a konfigurace prostředí PowerShell Azure, vytvoření nového souboru s názvem __hbase runner.psm1__. Použijte následující jako obsah tohoto souboru:

        <#
        .SYNOPSIS
        Copies a file to the primary storage of an HDInsight cluster.
        .DESCRIPTION
        Copies a file from a local directory to the blob container for
        the HDInsight cluster.
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.CreateTable"
        -clusterName "MyHDInsightCluster"
        
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
        -clusterName "MyHDInsightCluster"
        -emailRegex "contoso.com"
        
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
        -clusterName "MyHDInsightCluster"
        -emailRegex "^r" -showErr
        #>
        
        function Start-HBaseExample {
        [CmdletBinding(SupportsShouldProcess = $true)]
        param(
        #The class to run
        [Parameter(Mandatory = $true)]
        [String]$className,
        
        #The name of the HDInsight cluster
        [Parameter(Mandatory = $true)]
        [String]$clusterName,
        
        #Only used when using SearchByEmail
        [Parameter(Mandatory = $false)]
        [String]$emailRegex,
        
        #Use if you want to see stderr output
        [Parameter(Mandatory = $false)]
        [Switch]$showErr
        )
        
        Set-StrictMode -Version 3
        
        # Is the Azure module installed?
        FindAzure
        
        # Get the login for the HDInsight cluster
        $creds = Get-Credential
        
        # Get storage information
        $storage = GetStorage -clusterName $clusterName
        
        # The JAR
        $jarFile = "wasbs:///example/jars/hbaseapp-1.0-SNAPSHOT.jar"
        
        # The job definition
        $jobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
            -JarFile $jarFile `
            -ClassName $className `
            -Arguments $emailRegex
        
        # Get the job output
        $job = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $jobDefinition `
            -HttpCredential $creds
        Write-Host "Wait for the job to complete ..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
        if($showErr)
        {
        Write-Host "STDERR"
        Get-AzureRmHDInsightJobOutput `
                    -Clustername $clusterName `
                    -JobId $job.JobId `
                    -DefaultContainer $storage.container `
                    -DefaultStorageAccountName $storage.storageAccount `
                    -DefaultStorageAccountKey $storage.storageAccountKey `
                    -HttpCredential $creds `
                    -DisplayOutputType StandardError
        }
        Write-Host "Display the standard output ..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
                    -Clustername $clusterName `
                    -JobId $job.JobId `
                    -DefaultContainer $storage.container `
                    -DefaultStorageAccountName $storage.storageAccount `
                    -DefaultStorageAccountKey $storage.storageAccountKey `
                    -HttpCredential $creds
        }
        
        <#
        .SYNOPSIS
        Copies a file to the primary storage of an HDInsight cluster.
        .DESCRIPTION
        Copies a file from a local directory to the blob container for
        the HDInsight cluster.
        .EXAMPLE
        Add-HDInsightFile -localPath "C:\temp\data.txt"
        -destinationPath "example/data/data.txt"
        -ClusterName "MyHDInsightCluster"
        .EXAMPLE
        Add-HDInsightFile -localPath "C:\temp\data.txt"
        -destinationPath "example/data/data.txt"
        -ClusterName "MyHDInsightCluster"
        -Container "MyContainer"
        #>
        
        function Add-HDInsightFile {
            [CmdletBinding(SupportsShouldProcess = $true)]
            param(
                #The path to the local file.
                [Parameter(Mandatory = $true)]
                [String]$localPath,
                
                #The destination path and file name, relative to the root of the container.
                [Parameter(Mandatory = $true)]
                [String]$destinationPath,
                
                #The name of the HDInsight cluster
                [Parameter(Mandatory = $true)]
                [String]$clusterName,
                
                #If specified, overwrites existing files without prompting
                [Parameter(Mandatory = $false)]
                [Switch]$force
            )
            
            Set-StrictMode -Version 3
            
            # Is the Azure module installed?
            FindAzure
            
            # Get authentication for the cluster
            $creds=Get-Credential
            
            # Does the local path exist?
            if (-not (Test-Path $localPath))
            {
                throw "Source path '$localPath' does not exist."
            }
            
            # Get the primary storage container
            $storage = GetStorage -clusterName $clusterName
            
            # Upload file to storage, overwriting existing files if -force was used.
            Set-AzureStorageBlobContent -File $localPath `
                -Blob $destinationPath `
                -force:$force `
                -Container $storage.container `
                -Context $storage.context
        }
        
        function FindAzure {
            # Is there an active Azure subscription?
            $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
            if(-not($sub))
            {
                throw "No active Azure subscription found! If you have a subscription, use the Login-AzureRmAccount cmdlet to login to your subscription."
            }
        }
        
        function GetStorage {
            param(
                [Parameter(Mandatory = $true)]
                [String]$clusterName
            )
            $hdi = Get-AzureRmHDInsightCluster -ClusterName $clusterName
            # Does the cluster exist?
            if (!$hdi)
            {
                throw "HDInsight cluster '$clusterName' does not exist."
            }
            # Create a return object for context & container
            $return = @{}
            $storageAccounts = @{}
            
            # Get storage information
            $resourceGroup = $hdi.ResourceGroup
            $storageAccountName=$hdi.DefaultStorageAccount.split('.')[0]
            $container=$hdi.DefaultStorageContainer
            $storageAccountKey=(Get-AzureRmStorageAccountKey `
                -Name $storageAccountName `
            -ResourceGroupName $resourceGroup)[0].Value
            # Get the resource group, in case we need that
            $return.resourceGroup = $resourceGroup
            # Get the storage context, as we can't depend
            # on using the default storage context
            $return.context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
            # Get the container, so we know where to
            # find/store blobs
            $return.container = $container
            # Return storage accounts to support finding all accounts for
            # a cluster
            $return.storageAccount = $storageAccountName
            $return.storageAccountKey = $storageAccountKey
            
            return $return
        }
        # Only export the verb-phrase things
        export-modulemember *-*

    Tento soubor obsahuje dva moduly:

    * __Přidat HDInsightFile__ – slouží k nahrání souborů na HDInsight

    * __Zahájení HBaseExample__ - použít ke spuštění třídy vytvořili

2. Uložte soubor __hbase runner.psm1__ .

3. Otevření nového okna Azure Powershellu, přejděte v adresáři __hbaseapp__ adresáře a znovu spusťte tento příkaz.

        PS C:\ Import-Module c:\path\to\hbase-runner.psm1

    Změňte cestu k umístění souboru __hbase runner.psm1__ vytvořili. To zaregistruje modul Azure Powershellu pro tuto relaci.

2. Pomocí následujícího příkazu nahrajete __hbaseapp 1.0 SNAPSHOT.jar__ svůj cluster HDInsight.

        Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName hdinsightclustername

    __Hdinsightclustername__ nahraďte názvem svůj cluster HDInsight. Příkaz odešlou __hbaseapp 1.0 SNAPSHOT.jar__ __Příklad/sklenic po g__ umístění v úložišti primární pro svůj cluster HDInsight.

3. Po nahrání souborů použijte následující kód k vytvoření tabulky pomocí __hbaseapp__:

        Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName hdinsightclustername

    __Hdinsightclustername__ nahraďte názvem svůj cluster HDInsight.

    Příkaz vytvoří nová tabulka s názvem __lidé__ v svůj cluster HDInsight. Tento příkaz není zobrazena jakýkoli výstup v tomto okně.

2. Hledání položek v tabulce, použijte tento příkaz:

        Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName hdinsightclustername -emailRegex contoso.com

    __Hdinsightclustername__ nahraďte názvem svůj cluster HDInsight.

    Tento příkaz používá třídu **SearchByEmail** vyhledat všechny řádky, kde řady sloupce __contactinformation__ a ve sloupci __e-mailu__ obsahuje řetězec __contoso.com__. Zobrazí se následující výsledky:

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    Použití __fabrikam.com__ pro `-emailRegex` hodnota vrátí uživatelů, kteří mají __fabrikam.com__ do pole e-mailu. Protože hledání implementovaná pomocí běžná založené na výrazu filtru, můžete také zadat regulárních výrazů, například __^ r__, které vrátí položky kterých e-mailu začíná na písmeno "r".

##<a name="delete-the-table"></a>Odstranění tabulky

Po dokončení v příkladu, zadejte následující příkaz z relaci Powershellu Azure odstranit tabulku __lidé__ v tomto příkladě použita:

    Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName hdinsightclustername

__Hdinsightclustername__ nahraďte názvem svůj cluster HDInsight.

##<a name="troubleshooting"></a>Řešení potíží

###<a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a>Žádné výsledky nebo neočekávané výsledky při použití Start HBaseExample

Použití `-showErr` parametr zobrazíte standardní chyba (STDERR), který je vytvořen během spuštění úlohy.
