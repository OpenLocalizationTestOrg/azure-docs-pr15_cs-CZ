<properties
    pageTitle="Kurz HBase: Začínáme s na základě Linux HBase clusterů Hadoop | Microsoft Azure"
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
    ms.topic="get-started-article"
    ms.date="10/19/2016"
    ms.author="jgao"/>



# <a name="hbase-tutorial-get-started-using-apache-hbase-with-linux-based-hadoop-in-hdinsight"></a>Kurz HBase: začít používat Apache HBase se systémem Linux Hadoop v HDInsight 

[AZURE.INCLUDE [hbase-selector](../../includes/hdinsight-hbase-selector.md)]

Naučte se vytvořit HBase obrázku v HDInsight, HBase tabulkami a dotazy tabulky pomocí podregistru. Obecné informace HBase najdete v tématu [Přehled HDInsight HBase][hdinsight-hbase-overview].

Informace v tomto dokumentu je specifické pro clusterů na základě Linux HDInsight. Další informace o serveru s Windows clusterů umožňuje výběr umístění tabulátoru v horní části stránky přepínání.

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

##<a name="prerequisites"></a>Zjistit předpoklady pro

Před zahájením tohoto kurzu HBase, musíte mít takto:

- **Azure předplatného**. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- [Zabezpečené Shell(SSH)](hdinsight-hadoop-linux-use-ssh-unix.md). 
- [otáčení](http://curl.haxx.se/download.html).

### <a name="access-control-requirements"></a>Požadavky na řízení přístupu

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-hbase-cluster"></a>Vytvoření HBase obrázku

Následující postup používá správce prostředků Azure šablonu k vytvoření obrázku na základě Linux HBase verze 3.4 a závislá výchozí účet Azure úložiště. Parametry použité v procesu a jiné způsoby vytvoření obrázku, najdete v tématu [na základě vytvořit Linux Hadoop clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

1. Klikněte na následujícím obrázku otevření šablony na portálu Azure. Šablona se nachází v kontejneru veřejné objektů blob. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-cluster-in-hdinsight.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Z zásuvné **Vlastní nasazení** zadejte následující údaje:

    - **Předplatné**: Vyberte předplatné Azure, která bude použita k vytvoření clusteru.
    - **Pole Skupina zdroje**: vytvoření nové skupiny řízení zdrojů Azure nebo použít existující.
    - **Umístění**: Zadejte umístění skupina zdroje. 
    - **Název_clusteru**: Zadejte název HBase obrázku, který chcete vytvořit.
    - **Shluk přihlašovací jméno a heslo**: výchozí přihlašovací jméno je **Správce**.
    - **SSH uživatelské jméno a heslo**: výchozí uživatelské jméno je **sshuser**.  Můžete ji přejmenovat.
     
    Další parametry jsou volitelné.  
    
    Každý cluster má závislost účtu úložiště objektů Blob Azure. Po odstranění clusteru zachová data v účtu úložiště. Název účtu úložiště výchozí clusteru je název clusteru "Store" přidaným. Je pevně kódovaná v části šablona proměnné.
        
3. Vyberte **můžu souhlasíte s podmínkami a ujednáními výše uvedených**a potom klikněte na **koupit**. Vytvoření clusteru trvá asi o 20 minut.


>[AZURE.NOTE] Po odstranění HBase obrázku můžete vytvořit jiné HBase obrázku s použitím stejného kontejneru objektů blob výchozí. Nový cluster vyzvedne HBase tabulkami, který jste vytvořili v původní clusteru. Chcete-li předejít nekonzistence, doporučujeme vypnout HBase tabulkami před odstraněním clusteru.

## <a name="create-tables-and-insert-data"></a>Vytvoření tabulky a vložte data

SSH slouží k připojení k HBase clusterů a pak pomocí prostředí HBase vytvořit HBase tabulkami, vložit dat a dotazy na data. Informace o použití SSH najdete v tématu [Použití SSH s Hadoop Linux založené na HDInsight z Linux, Unix nebo OS X](hdinsight-hadoop-linux-use-ssh-unix.md) a [Použití SSH s Hadoop Linux založené na HDInsight z Windows](hdinsight-hadoop-linux-use-ssh-windows.md).
 

Většina lidí dat se zobrazí ve formátu tabulky:

![Tabulková data HDInsight HBase][img-hbase-sample-data-tabular]

V HBase, což je implementace BigTable stejná data vypadá takto:

![HDInsight HBase bigtable dat][img-hbase-sample-data-bigtable]

Bude vhodnější po dokončení procesu další.  


**Použití HBase prostředí**

1. Z SSH spusťte tento příkaz:

        hbase shell

4. Vytvoření HBase se dvěma sloupci skupinami:

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

    Stejné výsledky se zobrazí jako při použití příkazu naskenovaný obrázek, protože je pouze jeden řádek.

    Další informace o schématu HBase tabulky najdete v článku [Úvod k návrhu schématu HBase][hbase-schema]. Zobrazení dalších příkazů HBase, najdete v článku [Apache HBase referenční příručka][hbase-quick-start].

6. Ukončete prostředí

        exit



**Hromadně načtení dat do tabulky HBase kontaktů**

HBase obsahuje několik metody načítání dat do tabulky.  Další informace najdete v tématu [Hromadná načítání](http://hbase.apache.org/book.html#arch.bulk.load).


Ukázka datového souboru byl odeslán do kontejneru veřejné objektů blob *wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt*.  Obsah na datový soubor je:

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

> [AZURE.NOTE] Tento postup používá kontakty HBase tabulku, kterou jste vytvořili v posledním postupu.

1. Z SSH, spusťte tento příkaz Převést na datový soubor StoreFiles a úložiště na relativní cestu nastavil Dimporttsv.bulk.output:.  Pokud pracujete v prostředí HBase, použijte příkaz Konec ukončíte.

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name,Personal:Phone,Office:Phone,Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

4. Spusťte tento příkaz nahrát data z /example/data/storeDataFileOutput HBase tabulky:

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts

5. Otevřete HBase prostředí a obsah tabulky seznamu pomocí příkazu naskenovaný obrázek.



## <a name="use-hive-to-query-hbase"></a>Použití podregistru do dotazu HBase

Dotaz dat v HBase tabulkami pomocí podregistru. V této části vytvoří tabulku podregistru, která odpovídá tabulce HBase a používá k vytvoření dotazu data v tabulce HBase.

1. Otevřete **nátěrové**a připojte se k němu.  Postupujte podle pokynů v předchozím postupu.
2. Otevřete podregistru prostředí.

       hive
3. Spuštěním následujícího skriptu HiveQL vytvořit tabulku podregistru, který odpovídá HBase tabulky. Ujistěte se, že jste vytvořili v ukázkové tabulce odkazováno dříve v tomto kurzu pomocí prostředí HBase dříve než spustíte tento příkaz.

        CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
        STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
        WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
        TBLPROPERTIES ('hbase.table.name' = 'Contacts');

2. Spuštěním následujícího skriptu HiveQL k vytvoření dotazu data v tabulce HBase:

        SELECT count(*) FROM hbasecontacts;

## <a name="use-hbase-rest-apis-using-curl"></a>Použití HBase REST API pomocí otočení

> [AZURE.NOTE] Když používáte otočení nebo jiné ostatní komunikace s WebHCat, je třeba ověřit žádosti poskytnutím uživatelské jméno a heslo pro správce clusteru HDInsight. Také je nutné použít název obrázku jako součást aplikace identifikátor URI (Uniform Resource) slouží k odesílání požadavky na server.
>
> Příkazy v této části nahraďte **uživatelské jméno** uživatele k ověření clusteru a **HESLA** pomocí hesla k uživatelskému účtu. **NÁZEV_CLUSTERU** nahraďte názvem vašeho obrázku.
>
> Rozhraní REST API je zabezpečená prostřednictvím [základní ověřování](http://en.wikipedia.org/wiki/Basic_access_authentication). Vždy je třeba žádosti o pomocí zabezpečené HTTP (HTTPS) zajistit, aby vaše přihlašovací údaje bezpečně odesílány serveru.

1. Z příkazového řádku ověřte, že se můžete připojit ke svůj cluster HDInsight pomocí tento příkaz:

        curl -u <UserName>:<Password> \
        -G https://<ClusterName>.azurehdinsight.net/templeton/v1/status

    Dostanete odpověď podobná této:

        {"status":"ok","version":"v1"}

    Parametry použité v tento příkaz je takto:

    * **-u** – uživatelské jméno a heslo pro požadavek ověřit.
    * **-G** - značí, že to je požadavek GET.

2. Pomocí následujícího příkazu zobrazíte seznam existující HBase tabulkami:

        curl -u <UserName>:<Password> \
        -G https://<ClusterName>.azurehdinsight.net/hbaserest/

3. Umožňuje vytvořit novou tabulku HBase se dvěma skupinami sloupec tento příkaz:

        curl -u <UserName>:<Password> \
        -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/schema" \
        -H "Accept: application/json" \
        -H "Content-Type: application/json" \
        -d "{\"@name\":\"Contact1\",\"ColumnSchema\":[{\"name\":\"Personal\"},{\"name\":\"Office\"}]}" \
        -v

    Schéma je k dispozici ve formátu JSon.

4. Pomocí následujícího příkazu Vložit některá data:

        curl -u <UserName>:<Password> \
        -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/false-row-key" \
        -H "Accept: application/json" \
        -H "Content-Type: application/json" \
        -d "{\"Row\":{\"key\":\"MTAwMA==\",\"Cell\":{\"column\":\"UGVyc29uYWw6TmFtZQ==\", \"$\":\"Sm9obiBEb2xl\"}}}" \
        -v

    Je třeba ve formátu Base 64 kódovat hodnoty uvedené v přepínači -d.  V exmaple:

    - MTAwMA ==: 1 000
    - UGVyc29uYWw6TmFtZQ ==: osobní: název
    - Sm9obiBEb2xl: Dole Jan

    [Nepravda klíče řádku](https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/rest/package-summary.html#operation_cell_store_single) umožňuje vložení několika (dávkový) hodnoty.

5. Získat řádku zadejte následující příkaz:

        curl -u <UserName>:<Password> \
        -X GET "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/1000" \
        -H "Accept: application/json" \
        -v

Další informace o zbývající HBase najdete v tématu [Apache HBase referenční příručka](https://hbase.apache.org/book.html#_rest).

## <a name="check-cluster-status"></a>Kontrola stavu obrázku

HBase v HDInsight se dodává s uživatelským rozhraním webu pro sledování clusterů. Pomocí webového uživatelského rozhraní, můžete požádat o statistiky nebo informace o oblastech.

SSH lze také tunelu místní požadavky, například web požadavky, clusteru HDInsight. Žádost bude pak směrovaná k požadovanému prostředku, jako by měla pochází z hlavního uzlu clusteru HDInsight. Další informace najdete v tématu [Použití SSH s Hadoop Linux založeny na HDInsight z Windows](hdinsight-hadoop-linux-use-ssh-windows.md#tunnel).

**Abyste mohli vytvořit SSH tunneling relace**

1. Otevřete **nátěrové**.  
2. Pokud jste SSH klíč po vytvoření uživatelského účtu během s vytvářením, postupujte podle následujících pokynů vyberte privátním klíčem používat při ověřování clusteru:

    V seznamu **kategorie**rozbalte **připojení**, rozbalte **SSH**a vyberte **Auth**. Nakonec klikněte na tlačítko **Procházet** a vyberte .ppk soubor, který obsahuje privátním klíčem.

3. V seznamu **kategorie**klikněte na **relaci**.
4. Z základní možností obrazovce nátěrové relace zadejte tyto hodnoty:

    - **Host Name**: SSH adresy pole serveru v Host name (nebo IP adresa) HDInsight. Adresa SSH je název vašeho obrázku potom **-ssh.azurehdinsight.net**. Například *ssh.azurehdinsight.net clusteru*.
    - **Port**: 22. Ssh port na primární headnode je 22.  
5. V části **kategorie** nalevo od dialogového okna rozbalte **připojení**, rozbalte **SSH**a klikněte na **tunelů**.
6. V nabídce Možnosti řízení SSH port přesměrování formuláře zadejte následující informace:

    - **Zdrojový port** - port na straně klienta, který chcete předat dál. Například 9876.
    - **Dynamické** – umožňuje dynamické proxy serveru SOCKS směrování.
7. Kliknutím na **Přidat** přidejte nastavení.
8. Kliknutím na **Otevřít** v dolní části dialogového okna Otevřít SSH připojení.
9. Po zobrazení výzvy, přihlaste se k serveru pomocí účtu SSH. Tím vytvořit relaci SSH a povolit tunelem.

**K vyhledání plně kvalifikovaný název domény zookeepers pomocí Ambari**

1. Přejděte na https://<ClusterName>.azurehdinsight.net/.
2. Zadejte svoje přihlašovací údaje účtu uživatele clusteru dvakrát.
3. V nabídce nalevo klikněte na **zookeeper**.
4. Klikněte na jednu z tři odkazy **ZooKeeper Server** ze seznamu souhrn.
5. Zkopírujte **název hostitele**. Například zk0-CLUSTERNAME.xxxxxxxxxxxxxxxxxxxx.cx.internal.cloudapp.net.

**Ke konfiguraci klientským programem (Firefox) a zkontrolujte stav obrázku**

1. Otevřete Firefox.
2. Klikněte na tlačítko **Otevřít nabídku** .
3. Klikněte na **Možnosti**.
4. Klikněte na **Upřesnit**, klikněte na **síť**a potom klikněte na **Nastavení**.
5. Vyberte **proxy ruční konfiguraci**.
6. Zadejte následující hodnoty:

    - **Host (hostitel) SOCKS**: localhost
    - **Port**: používat stejný port jste nakonfigurovali v SSH nátěrové tunneling.  Například 9876.
    - **SOCKS verze 5**: (vybraná)
    - **Vzdálené DNS**: (vybraná)
7. Kliknutím na **OK** uložte změny.
8. Přejděte na http://&lt;plně kvalifikovaný název domény ZooKeeper >: 60010/předlohy stav.

Dostupnost clusteru bude hledat odkaz na aktuální aktivní HBase předlohy uzel, který je hostitelem uživatelské rozhraní webu.

##<a name="delete-the-cluster"></a>Odstranění clusteru

Chcete-li předejít nekonzistence, doporučujeme vypnout HBase tabulkami před odstraněním clusteru.

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Další kroky

V tomto kurzu HBase pro HDInsight jste se naučili postup vytvoření obrázku HBase a jak vytvořit tabulek a zobrazení dat v těchto tabulkách v prostředí, kde HBase. Naučili jste používání podregistru dotaz na data v HBase tabulkami a jak používat rozhraní HBase C# REST API k vytvoření tabulky HBase a načtení dat z tabulky.

Další informace najdete v tématu:

- [Přehled HDInsight HBase][hdinsight-hbase-overview]: HBase je databáze NoSQL otevřít zdroje, Apache, založená na Hadoop poskytující RAM a silných konzistence pro velké objemy dat nestrukturovaná a semistructured.


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
[azure-portal]: https://portal.azure.com/
[azure-create-storageaccount]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/

[img-hdinsight-hbase-cluster-quick-create]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-quick-create.png
[img-hdinsight-hbase-hive-editor]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-hive-editor.png
[img-hdinsight-hbase-file-browser]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-file-browser.png
[img-hbase-shell]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-shell.png
[img-hbase-sample-data-tabular]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-tabular.png
[img-hbase-sample-data-bigtable]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-bigtable.png
