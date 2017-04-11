<properties
    pageTitle="Vytvoření aplikace HBase přes Maven a Java a pak nasadit na základě Linux HDInsight | Microsoft Azure"
    description="Naučte se používat Apache Maven vytvořit na základě Java HBase Apache aplikaci a pak ho nasadit do na základě Linux HDInsight v Azure cloudu."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="larryfr"/>

#<a name="use-maven-to-build-java-applications-that-use-hbase-with-linux-based-hdinsight-hadoop"></a>Použít Maven k vytvoření Java aplikace, které používají HBase se systémem Linux HDInsight (Hadoop)

Naučte se vytvářet a vytváření [Apache HBase](http://hbase.apache.org/) aplikaci Java pomocí Apache Maven. Potom použijte aplikaci MONTI s obrázku na základě Linux HDInsight.

[Maven](http://maven.apache.org/) je software řízení projektů a porozumění informacím nástroj, který umožňuje vytvářet software, si přečtěte následující dokumentaci a sestav pro Java projekty. V tomto článku se dozvíte, jak se používá k vytvoření základní Java aplikace, které vytvoří, dotazy a odstraní tabulku HBase clusteru na základě Linux HDInsight.

> [AZURE.NOTE] Kroky v tomto dokumentu se předpokládá, že používáte obrázku na základě Linux HDInsight. Informace o použití clusteru HDInsight serveru s Windows najdete v tématu [Maven použít k vytváření Java aplikací používajících HBase se systémem Windows HDInsight](hdinsight-hbase-build-java-maven.md)

##<a name="requirements"></a>Požadavky

* [Platformu Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 nebo novější

* [Maven](http://maven.apache.org/)

* [Na základě Linux Azure HDInsight obrázku s HBase](../hdinsight-hbase-tutorial-get-started-linux.md#create-hbase-cluster)

    > [AZURE.NOTE] Kroky v tomto dokumentu bylo testováno se verze obrázku HDInsight 3,2, 3.3 a 3.4. Výchozí hodnoty zadané v příkladech se clusteru HDInsight 3.4.

* **Znalost SSH a spojovací bod služby**. Další informace o používání SSH a spojovací bod služby s Hdinsightu najdete v těchto článcích:

    * **Linux, Unix nebo OS X klienti**: najdete v článku [Použití SSH s Hadoop Linux založené na HDInsight z Linux, OS X nebo Unix](hdinsight-hadoop-linux-use-ssh-unix.md)

    * **Klienti se systémem Windows**: najdete v článku [Použití SSH s Hadoop Linux založené na HDInsight z Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

##<a name="create-the-project"></a>Vytvoření projektu

1. Z příkazového řádku v prostředí vývoj, změňte adresáře do umístění, ve které chcete vytvořit projekt, například `cd code/hdinsight`.

2. Příkazem __mvn__ , který se instaluje s Maven, generovat vygenerovaných projektu.

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    Vytvoří nový adresář v adresáři aktuální s názvem určené parametrem __artifactID__ (**hbaseapp** v tomto příkladu.) Tento adresář obsahuje následující položky:

    * __pom.XML__: objektovému modelu projektu ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) obsahuje podrobnosti o informace a konfigurace slouží k vytváření projektu.

    * __src__: adresář, který obsahuje adresáři __hlavní/java/com/microsoft/příklady__ , kde se vytvářet aplikace.

3. Odstraňte soubor __src/test/java/com/microsoft/examples/apptest.java__ , protože nebude v tomto příkladě použita.

##<a name="update-the-project-object-model"></a>Aktualizace objektovému modelu projektu

1. Upravte soubor __pom.xml__ a přidejte následující kód do `<dependencies>` oddíl:

        <dependency>
          <groupId>org.apache.hbase</groupId>
          <artifactId>hbase-client</artifactId>
          <version>1.1.2</version>
        </dependency>

    To, bude Maven projekt vyžaduje __hbase klienta__ verze __1.1.2__. V době kompilace to bude možné stáhnout z úložiště Maven výchozí. Další informace o tomto závislost můžete [Maven centrálním úložišti vyhledávání](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) .

    > [AZURE.IMPORTANT] Číslo verze se musí shodovat verzi HBase, která je součástí svůj cluster HDInsight. Použijte v následující tabulce najdete na správné číslo verze.

  	| HDInsight verze obrázku | Verze HBase se má použít |
  	| ----- | ----- |
  	| 3,2 | 0.98.4-hadoop2 |
  	| 3.3 a 3.4 | 1.1.2 |

    Další informace o verzích HDInsight a součásti [se](hdinsight-component-versioning.md)podívejte jiné součásti Hadoop dostupné s HDInsight.

2. Pokud používáte HDInsight 3.3 nebo 3.4 clusteru, musíte taky přidáte podle následujících pokynů `<dependencies>` oddíl:

        <dependency>
            <groupId>org.apache.phoenix</groupId>
            <artifactId>phoenix-core</artifactId>
            <version>4.4.0-HBase-1.1</version>
        </dependency>
    
    To načte phoenix základní součásti, které jsou potřebné verzí Hbase 1.1.x.

2. Přidejte následující kód k souboru __pom.xml__ . Musí být uvnitř `<project>...</project>` značky v souboru, například mezi `</dependencies>` a `</project>`.

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

    To nakonfiguruje zdroj (__conf/hbase site.xml__,), který obsahuje informace o konfiguraci pro HBase.

    > [AZURE.NOTE] Můžete také nastavit konfigurace hodnoty pomocí kódu. Zobrazit komentáře v následujícím příkladu __CreateTable__ pro školá.

    Konfiguruje také modul [Plug-in Maven stín](http://maven.apache.org/plugins/maven-shade-plugin/)a [Modul plug-in kompilátoru Maven](http://maven.apache.org/plugins/maven-compiler-plugin/) . Modul plug-in kompilátoru slouží sestavíte topologii. Modul plug-in stín slouží k zabránit duplikace licence, která je integrovaná Maven balíček SKLENICE. Důvod, proč tato možnost slouží je, aby duplicitní licenčních souborů mít za následek chybu za běhu clusteru HDInsight. Použití maven stín-modul plug-in se `ApacheLicenseResourceTransformer` implementaci zabrání tato chyba.

    Plugin stín maven taky vznikne uber sklenice (nebo tuku sklenice), která obsahuje všechny závislosti vyžaduje aplikace.

3. Uložte soubor __pom.xml__ .

4. Vytvořte nový adresář s názvem __conf__ v adresáři __hbaseapp__ . Používá se pro uložení informací o připojení k HBase konfiguraci.

5. Pomocí následujícího příkazu kopírování konfigurace HBase ze serveru HDInsight k adresáři __conf__ . Nahrazení **uživatelské jméno** název SSH přihlášení. Nahraďte **NÁZEV_CLUSTERU** názvu HDInsight obrázku:

        scp USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml

    > [AZURE.NOTE] Pokud jste použili heslo účtu SSH se výzva k zadání hesla. Pokud jste použili SSH klíč s účtem, budete muset používat `-i` parametr zadejte cestu k souboru klíče. Následující příklad načte privátním klíčem z `~/.ssh/id_rsa`:
    >
    > `scp -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml`

##<a name="create-the-application"></a>Vytvoření aplikace

1. Přejděte do adresáře __hbaseapp/src/hlavní/java/com/microsoft/příklady__ a přejmenujte soubor app.java __CreateTable.java__.

2. Otevřete soubor __CreateTable.java__ a nahradit existující obsah takto:

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
            //
            //NOTE: Actual zookeeper host names can be found using Ambari:
            //curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts"
            
            //Linux-based HDInsight clusters use /hbase-unsecure as the znode parent
            config.set("zookeeper.znode.parent","/hbase-unsecure");

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

4. V adresáři __hbaseapp/src/hlavní/java/com/microsoft/příklady__ vytvoření nového souboru s názvem __SearchByEmail.java__. Použijte následující jako obsah tohoto souboru:

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

6. V adresáři __hbaseapp/src/hlavní/hava/com/microsoft/příklady__ vytvoření nového souboru s názvem __DeleteTable.java__. Použijte následující jako obsah tohoto souboru:

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

2. V adresáři __hbaseapp__ pomocí následujícího příkazu vytvářet SKLENICE soubor, který obsahuje aplikace:

        mvn clean package

    To vyčistí všechny předchozí artefakty Tvůrce dotazů, stáhne nějaké závislosti, které již byly nenainstalovali, potom vytvoří a balíčky aplikace.

3. Po provedení příkazu __hbaseapp/cílový__ adresář bude obsahovat do souboru nazvaného __hbaseapp 1.0 SNAPSHOT.jar__.

    > [AZURE.NOTE] Soubor __hbaseapp 1.0 SNAPSHOT.jar__ je uber sklenice (někdy se jí říká tuku sklenice), která obsahuje všechny závislosti potřebné ke spuštění aplikace.

##<a name="upload-the-jar-file-and-run-jobs"></a>Nahrání souboru SKLENICE a spuštění úlohy

1. Použijte následující k nahrání sklenice clusteru HDInsight. Nahrazení **uživatelské jméno** název SSH přihlášení. Nahraďte **NÁZEV_CLUSTERU** názvu HDInsight obrázku:

        scp ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.

    To bude nahrání souboru se adresáři vašeho uživatelského účtu SSH.

    > [AZURE.NOTE] Pokud jste použili heslo účtu SSH se výzva k zadání hesla. Pokud jste použili SSH klíč s účtem, budete muset používat `-i` parametr zadejte cestu k souboru klíče. Následující příklad načte privátním klíčem z `~/.ssh/id_rsa`:
    >
    > `scp -i ~/.ssh/id_rsa ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.`

2. Připojení k obrázku HDInsight pomocí SSH. Nahrazení **uživatelské jméno** název SSH přihlášení. Nahraďte **NÁZEV_CLUSTERU** názvu HDInsight obrázku:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    > [AZURE.NOTE] Pokud jste použili heslo účtu SSH se výzva k zadání hesla. Pokud jste použili SSH klíč s účtem, budete muset používat `-i` parametr zadejte cestu k souboru klíče. Následující příklad načte privátním klíčem z `~/.ssh/id_rsa`:
    >
    > `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`

3. Po připojení, použijte následující možnosti vytvořit novou tabulku HBase pomocí aplikace Java:

        hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.CreateTable

    To vytvořit novou tabulku HBase s názvem __lidi__a doplníte data.

4. Pak použijte následující vyhledat e-mailové adresy uložené v tabulce:

        hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.SearchByEmail contoso.com

    Zobrazí se následující výsledky:

        Franklin Holtz - ID: 2
        Franklin Holtz - franklin@contoso.com - ID: 2
        Rae Schroeder - ID: 4
        Rae Schroeder - rae@contoso.com - ID: 4
        Gabriela Ingram - ID: 6
        Gabriela Ingram - gabriela@contoso.com - ID: 6

##<a name="delete-the-table"></a>Odstranění tabulky

Po dokončení v příkladu, zadejte následující příkaz z relaci Powershellu Azure odstranit tabulku __lidé__ v tomto příkladě použita:

    hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.DeleteTable

