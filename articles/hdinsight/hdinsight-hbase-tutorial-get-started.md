<properties
    pageTitle="Kurz HBase: Začínáme s HBase v Hadoop | Microsoft Azure"
    description="Postupujte podle kurzu HBase, který začít používat Apache HBase se Hadoop v HDInsight. Vytvoření tabulky z prostředí HBase a jejich dotazu pomocí podregistru."
    keywords="Apache hbase hbase, hbase prostředí hbase kurz"
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
    ms.date="10/21/2016"
    ms.author="jgao"/>



# <a name="hbase-tutorial-get-started-using-apache-hbase-with-windows-based-hadoop-in-hdinsight"></a>Kurz HBase: začít používat Apache HBase se systémem Windows Hadoop v HDInsight

[AZURE.INCLUDE [hbase-selector](../../includes/hdinsight-hbase-selector.md)]

Naučte se vytvořit HBase clusterů v HDInsight, HBase tabulkami a dotazy tabulky pomocí Apache podregistru. Obecné informace HBase najdete v tématu [Přehled HDInsight HBase][hdinsight-hbase-overview].

Informace v tomto dokumentu jsou specifické pro clusterů serveru s Windows HDInsight. Další informace o serveru s Windows clusterů umožňuje výběr umístění tabulátoru v horní části stránky přepínání.

> [AZURE.NOTE] HBase (verze 0.98.0) na serveru s Windows HDInsight je dostupný jenom u pomocí HDInsight 3.1 clusterů (na základě Apache Hadoop a vláken 2.4.0). Informace o verzi, najdete v článku [Co je nového v verze obrázku Hadoop poskytovanou HDInsight?][hdinsight-versions]

## <a name="before-you-begin"></a>Než začnete

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Před zahájením tohoto kurzu HBase, musíte mít takto:

- **Předplatné A Microsoft Azure**. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Stanici** s Visual Studio 2013 nebo větší: pokyny najdete v tématu [Instalace aplikace Visual Studio](http://msdn.microsoft.com/library/e2h7fzkw.aspx).

### <a name="access-control-requirements"></a>Požadavky na řízení přístupu

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-hbase-cluster"></a>Vytvoření HBase obrázku

[AZURE.INCLUDE [provisioningnote](../../includes/hdinsight-provisioning.md)]

**K vytvoření clusteru HBase pomocí portálu Azure**

1. Přihlaste se k [portálu Azure][azure-management-portal].
2. Klikněte na **Nový** nebo **+** v levém horním rohu a potom klikněte na **Data + analýzy**, **HDInsight**.
3. Zadejte následující hodnoty:

    - **Název clusteru** - zadejte název pro identifikaci tomto obrázku.
    - **Typ obrázku** – vyberte **HBase**.
    - **Shluk operační systém** – vyberte **systému Windows**.  Při vytváření na základě Linux HBase clusteru najdete v článku [HBase kurz: Začínáme s používáním Apache HBase s Hadoop v HDInsight (Linux)](hdinsight-hbase-tutorial-get-started-linux.md).
    - **Verze** – vyberte HBase verze.
    - **Předplatné** - vyberte předplatné Azure použít k vytvoření tomto obrázku.
    - **Pole Skupina zdroje** : vytvoření nové skupiny prostředků Azure nebo vyberte stávající. Další informace najdete v tématu [Přehled Správce prostředků Azure](azure-resource-manager/resource-group-overview.md)
    - **Přihlašovací údaje** - pro Windows na základě obrázku, můžete vytvořit clusteru uživatel (uživatel také znám jako HTTP, uživatel webové služby protokolu HTTP) a Vzdálená plocha uživatele. Klikněte na **Povolit vzdálená plocha** , pokud chcete přidat vzdálené plochy přihlašovací údaje uživatele pro. Následující části vyžaduje RDP.
    - **Zdroj dat** – vytvořte nový účet Azure úložiště nebo vyberte stávající účet Azure úložiště a použít jako výchozí systém souborů pro clusteru. Výchozí úložiště účtu určuje umístění umístění obrázku. Výchozí úložiště účet a clusteru musí spoluvytváření vyhledejte ve stejném datovém centru.
    - **Ceny úrovní uzel** – vyberte počet servery oblast pro HBase obrázku

        > [AZURE.WARNING] Vysoké dostupnosti HBase služeb, musíte vytvořit clusteru, který obsahuje nejméně **tři** uzly. Zajistíte tak, že pokud jeden uzel přejde, oblasti dat HBase jsou k dispozici na jiných uzlů.

        > Pokud při osvojování si HBase, vždy zvolte 1 pro velikost obrázku a odstraňte clusteru po každém použití snížit náklady.

    - **Volitelné konfigurace** – konfigurace Azure virtuální sítě nakonfigurování akcí skriptu a přidejte další úložiště účty.

4. Klikněte na **vytvořit**.

>[AZURE.NOTE] Po odstranění HBase obrázku můžete vytvořit jiné HBase obrázku s použitím jednoho účtu úložiště výchozí a kontejneru objektů blob výchozí. Nový cluster vyzvedne HBase tabulkami, který jste vytvořili v původní clusteru. Chcete-li předejít nekonzistence, doporučujeme vypnout HBase tabulkami před odstraněním clusteru.

## <a name="create-tables-and-insert-data"></a>Vytvoření tabulky a vložte data

Přístup k HBase jsou v současné době obousměrná. Tato část popisuje pomocí prostředí HBase. Následující části se vztahuje pomocí .NET SDK.

Většina lidí dat se zobrazí ve formátu tabulky:

![tabulková data hbase hdinsight][img-hbase-sample-data-tabular]

V HBase, což je implementace BigTable stejná data vypadá takto:

![hdinsight hbase bigtable dat][img-hbase-sample-data-bigtable]

Po dokončení procesu další ho budete být lepší.  

**Použití HBase prostředí**

1. Připojení k HBase cluster v HDInsight pomocí RDP. RDP pokyny najdete v tématu [Správa Hadoop clusterů v portálu Azure HDInsight][hdinsight-manage-portal].
2. V relaci RDP klikněte na **přepínač příkazového řádku Hadoop** zkratku umístěný na ploše.
3. Otevřete HBase prostředí:

        cd %HBASE_HOME%\bin
        hbase shell

4. Vytvoření HBase se dvěma skupinami sloupce:

        create 'Contacts', 'Personal', 'Office'
        list
5. Vložení některá data:

        put 'Contacts', '1000', 'Personal:Name', 'John Dole'
        put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
        put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
        put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
        scan 'Contacts'

    ![hdinsight hadoop hbase prostředí][img-hbase-shell]

6. Získání do jednoho řádku

        get 'Contacts', '1000'

    Zobrazí se stejné výsledky jako při použití příkazu naskenovaný obrázek, protože je pouze jeden řádek.

    Další informace o schématu Hbase tabulky najdete v článku [Úvod k návrhu schématu HBase][hbase-schema]. Zobrazení dalších příkazů HBase, najdete v článku [Apache HBase referenční příručka][hbase-quick-start].


6. Ukončete prostředí

        exit

**Hromadně načtení dat do tabulky HBase kontaktů**

HBase obsahuje několik metody načítání dat do tabulky. Další informace najdete v tématu [Hromadná načítání](http://hbase.apache.org/book.html#arch.bulk.load).


Ukázka datového souboru byl odeslán do kontejneru veřejné objektů blob wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt. Obsah na datový soubor je:

    8396    Calvin Raji     230-555-0191    230-555-0191    5415 San Gabriel Dr.
    16600   Karen Wu        646-555-0113    230-555-0192    9265 La Paz
    4324    Karl Xie        508-555-0163    230-555-0193    4912 La Vuelta
    16891   Jonn Jackson    674-555-0110    230-555-0194    40 Ellis St.
    3273    Miguel Miller   397-555-0155    230-555-0195    6696 Anchor Drive
    3588    Osa Agbonile    592-555-0152    230-555-0196    1873 Lion Circle
    10272   Julia Lee       870-555-0110    230-555-0197    3148 Rose Street
    4868    Jose Hayes      599-555-0171    230-555-0198    793 Crawford Street
    4761    Caleb Alexander 670-555-0141    230-555-0199    4775 Kentucky Dr.
    16443   Terry Chander   998-555-0171    230-555-0200    771 Northridge Drive

Můžete vytvořit textový soubor a nahrajte soubor účtu úložiště, pokud chcete. Pokyny najdete v tématu [nahrání dat pro Hadoop projekty v HDInsight][hdinsight-upload-data].

> [AZURE.NOTE] Tento postup používá kontakty HBase tabulku, kterou jste vytvořili v posledním kroku.

1. V relaci RDP klikněte na **přepínač příkazového řádku Hadoop** zkratku umístěný na ploše.
2. Změna adresáře:

        cd %HBASE_HOME%\bin

3. Spusťte tento příkaz Převést na datový soubor StoreFiles a úložiště na relativní cestu nastavil Dimporttsv.bulk.output:

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name, Personal:Phone, Office:Phone, Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

4. Spusťte tento příkaz nahrát data z /example/data/storeDataFileOutput HBase tabulky:

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts

5. Otevřete HBase prostředí a obsah tabulky seznamu pomocí příkazu naskenovaný obrázek.



## <a name="use-hive-to-query-hbase-tables"></a>Použití podregistru do dotazu HBase tabulkami

Data uložená v HBase pomocí podregistru můžete dotaz. V této části vytvoří tabulku podregistru, která odpovídá tabulce HBase a používá k vytvoření dotazu data v tabulce HBase.

**Chcete-li otevřít řídicím panelu obrázku**

1. Přejděte na **https://<HDInsight Cluster Name>.azurehdinsight.net/**.
5. Zadejte Hadoop uživatelského účtu uživatelské jméno a heslo. Výchozí uživatelské jméno **Správce** označeným je heslo zadané při s vytvářením. Otevře na nové záložce prohlížeče.
6. Klepněte na **Podregistru Editor** v horní části stránky. Editor podregistru vypadat takto:

    ![Řídicí panel clusteru HDInsight.][img-hdinsight-hbase-hive-editor]

**Chcete-li spouštění dotazů podregistru**

1. Zadejte tento skript HiveQL do podregistru Editor a kliknutím na možnost **Odeslat** vytvořit tabulku podregistru, která odpovídá HBase tabulky. Ujistěte se, že jste vytvořili v ukázkové tabulce odkazováno dříve v tomto kurzu pomocí prostředí HBase dříve než spustíte tento příkaz.

        CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
        STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
        WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
        TBLPROPERTIES ('hbase.table.name' = 'Contacts');

    Počkejte, dokud aktualizací **stavu** na **Dokončit**.

2. Zadejte tento skript HiveQL do podregistru editoru a potom klikněte na **Odeslat**. Dotaz podregistru dotazy data v tabulce HBase:

        SELECT count(*) FROM hbasecontacts;

4. K načítání výsledků dotazu podregistru, klikněte na odkaz **Zobrazit podrobnosti** v okně **Relace úlohy** po dokončení projektu. Nastane jedinou úlohy výstupní soubor vzhledem k tomu, že umístíte záznamů do tabulky HBase.




**Přejděte ve výstupním souboru**

1. V konzole dotazu klikněte na **Soubor prohlížeče**.
2. Klikněte na účet Azure úložiště, který slouží jako výchozí systém souborů pro HBase obrázku.
3. Klikněte na název HBase obrázku. Kontejner účet Azure úložiště výchozí používá název obrázku.
4. Klikněte na **uživatele**a potom klikněte na **Správce**. (Toto je uživatelské jméno Hadoop.)
6. Klikněte na název projektu, která odpovídá čas při spuštění dotazu vyberte podregistru čas **Naposledy změněno** .
4. Klikněte na **stdout**. Uložte soubor a otevřete soubor programu Poznámkový blok. Nastane jedna výstupní soubor.

    ![HDInsight HBase podregistru Editor souboru prohlížeče][img-hdinsight-hbase-file-browser]

## <a name="use-the-net-hbase-rest-api-client-library"></a>Použít knihovnu klienta .NET HBase REST API

Musíte stáhnout knihovnu klienta HBase REST API pro .NET z GitHub a vytvořte projekt, aby mohli používat HBase .NET SDK. Následující postup obsahuje pokyny k tomuto úkolu.

1. Vytvořte novou aplikaci C# Visual Studio plochy konzoly Windows.
2. Spusťte konzolu Správce balíčků NuGet po kliknutí na **Nástroje** > **Správce balíčků NuGet** > **Konzoly balíčku správce**.
3. Spusťte tento příkaz NuGet v konzole:

        Install-Package Microsoft.HBase.Client

5. Přidejte následující příkazy **pomocí** v horní části souboru:

        using Microsoft.HBase.Client;
        using org.apache.hadoop.hbase.rest.protobuf.generated;

6. Nahraďte funkci **hlavní** takto:

        static void Main(string[] args)
        {
            string clusterURL = "https://<yourHBaseClusterName>.azurehdinsight.net";
            string hadoopUsername= "<yourHadoopUsername>";
            string hadoopUserPassword = "<yourHadoopUserPassword>";

            string hbaseTableName = "sampleHbaseTable";

            // Create a new instance of an HBase client.
            ClusterCredentials creds = new ClusterCredentials(new Uri(clusterURL), hadoopUsername, hadoopUserPassword);
            HBaseClient hbaseClient = new HBaseClient(creds);

            // Retrieve the cluster version.
            var version = hbaseClient.GetVersion();
            Console.WriteLine("The HBase cluster version is " + version);

            // Create a new HBase table.
            TableSchema testTableSchema = new TableSchema();
            testTableSchema.name = hbaseTableName;
            testTableSchema.columns.Add(new ColumnSchema() { name = "d" });
            testTableSchema.columns.Add(new ColumnSchema() { name = "f" });
            hbaseClient.CreateTable(testTableSchema);

            // Insert data into the HBase table.
            string testKey = "content";
            string testValue = "the force is strong in this column";
            CellSet cellSet = new CellSet();
            CellSet.Row cellSetRow = new CellSet.Row { key = Encoding.UTF8.GetBytes(testKey) };
            cellSet.rows.Add(cellSetRow);

            Cell value = new Cell { column = Encoding.UTF8.GetBytes("d:starwars"), data = Encoding.UTF8.GetBytes(testValue) };
            cellSetRow.values.Add(value);
            hbaseClient.StoreCells(hbaseTableName, cellSet);

            // Retrieve a cell by its key.
            cellSet = hbaseClient.GetCells(hbaseTableName, testKey);
            Console.WriteLine("The data with the key '" + testKey + "' is: " + Encoding.UTF8.GetString(cellSet.rows[0].values[0].data));
            // with the previous insert, it should yield: "the force is strong in this column"

            //Scan over rows in a table. Assume the table has integer keys and you want data between keys 25 and 35.
            Scanner scanSettings = new Scanner()
            {
                batch = 10,
                startRow = BitConverter.GetBytes(25),
                endRow = BitConverter.GetBytes(35)
            };

            ScannerInformation scannerInfo = hbaseClient.CreateScanner(hbaseTableName, scanSettings);
            CellSet next = null;
            Console.WriteLine("Scan results");

            while ((next = hbaseClient.ScannerGetNext(scannerInfo)) != null)
            {
                foreach (CellSet.Row row in next.rows)
                {
                    Console.WriteLine(row.key + " : " + Encoding.UTF8.GetString(row.values[0].data));
                }
            }

            Console.WriteLine("Press ENTER to continue ...");
            Console.ReadLine();
        }

7. Nastavte první tři proměnné ve funkci **hlavní** .
8. Stisknutím klávesy **F5** spustit aplikaci.

## <a name="check-cluster-status"></a>Kontrola stavu obrázku

HBase v HDInsight se dodává s uživatelským rozhraním webu pro sledování clusterů. Pomocí webového uživatelského rozhraní, můžete požádat o statistiky nebo informace o oblastech.

Otevřete uživatelské rozhraní webu, můžete třeba RDP do clusteru a potom klikněte na uživatelské rozhraní webu informace HMaster zástupce na ploše nebo pomocí následující adresu URL ve webovém prohlížeči:

    http://zookeeper[0-2]:60010/master-status

V cluster dostupnost najdete odkaz na aktuální aktivní HBase předlohy uzel, který je hostitelem uživatelské rozhraní webu.

##<a name="delete-the-cluster"></a>Odstranění clusteru
Chcete-li předejít nekonzistence, doporučujeme vypnout HBase tabulkami před odstraněním clusteru.
[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="whats-next"></a>Co je další krok?
V tomto kurzu HBase pro HDInsight jste se naučili postup vytvoření obrázku HBase a jak vytvořit tabulek a zobrazení dat v těchto tabulkách v prostředí, kde HBase. Můžete taky naučili používání podregistru dotaz na data v HBase tabulkami a jak používat rozhraní HBase C# REST API k vytvoření tabulky HBase a načtení dat z tabulky.

Další informace najdete v tématu:

- [Přehled HDInsight HBase][hdinsight-hbase-overview].
HBase je databáze NoSQL otevřít zdroje, Apache, založená na Hadoop poskytující RAM a silných konzistence pro velké objemy dat nestrukturovaná a semistructured.
- [Vytvoření HBase clusterů Azure virtuální síti se systémem][hdinsight-hbase-provision-vnet].
Integrace virtuální sítě můžete být nasazené HBase clusterů na stejné virtuální sítě jako aplikace tak, aby aplikace komunikovat s HBase přímo.
- [Konfigurace HBase replikace v HDInsight](hdinsight-hbase-geo-replication.md). Zjistěte, jak konfigurovat HBase replikace mezi dvěma Azure datacentrech.
- [Analýza Twitter myšlenkou s HBase v HDInsight][hbase-twitter-sentiment].
Zjistěte, jak provádět v reálném čase [myšlenkou analýza](http://en.wikipedia.org/wiki/Sentiment_analysis) velkých dat pomocí HBase v clusteru Hadoop v HDInsight.

[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hbase-reference]: http://hbase.apache.org/book.html#importtsv
[hbase-schema]: http://0b4af6cdc2f0c5998459-c0245c5c937c5dedcca3f1764ecc9b2f.r43.cf2.rackcdn.com/9353-login1210_khurana.pdf
[hbase-quick-start]: http://hbase.apache.org/book.html#quickstart





[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-versions]: hdinsight-component-versioning.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/

[img-hdinsight-hbase-cluster-quick-create]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-quick-create.png
[img-hdinsight-hbase-hive-editor]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-hive-editor.png
[img-hdinsight-hbase-file-browser]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-file-browser.png
[img-hbase-shell]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-shell.png
[img-hbase-sample-data-tabular]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-contacts-tabular.png
[img-hbase-sample-data-bigtable]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-contacts-bigtable.png
