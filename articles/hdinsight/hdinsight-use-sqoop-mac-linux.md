<properties
    pageTitle="Použití Hadoop Sqoop v na základě Linux HDInsight | Microsoft Azure"
    description="Informace o spouštění Sqoop import a export mezi Hadoop na základě Linux HDInsight clusteru a databáze Azure SQL."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="larryfr"/>

#<a name="use-sqoop-with-hadoop-in-hdinsight-ssh"></a>Použití Sqoop s Hadoop v HDInsight (SSH)

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Naučte se používat Sqoop import a export mezi na základě Linux HDInsight obrázku a databáze SQL Azure nebo SQL Server databáze.

> [AZURE.NOTE] Připojení k obrázku bázi Linux HDInsight pomocí kroků v tomto článku SSH. Klienty Windows můžete také použít Azure PowerShell a HDInsight .NET SDK pro práci s Sqoop na základě Linux clusterů. Otevřete tyto články pomocí výběr umístění tabulátoru.

##<a name="prerequisites"></a>Zjistit předpoklady pro

Před zahájením tohoto kurzu, musíte mít takto:

- **A Hadoop cluster v HDInsight** a __Databáze SQL Azure__: kroky v tomto dokumentu jsou založeny na obrázku a databáze vytvořené pomocí dokument o [Vytvoření obrázku a databáze SQL](hdinsight-use-sqoop.md#create-cluster-and-sql-database) . Pokud už máte HDInsight obrázku a SQL databáze, můžete nahradit můžou být hodnoty v tomto dokumentu.
- **Workstation**: počítač s klientem SSH.

##<a name="install-freetds"></a>Instalace FreeTDS

1. Připojení k obrázku na základě Linux HDInsight pomocí SSH. Adresa pro použití při připojení `CLUSTERNAME-ssh.azurehdinsight.net` a port je `22`.

    Další informace o využití SSH k připojení k Hdinsightu najdete v tématu tyto dokumenty:

    * **Linux, Unix nebo OS X klienti**: Přečtěte si článek [připojení k obrázku na základě Linux HDInsight z Linux, OS X nebo Unix](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster)

    * **Klienti se systémem Windows**: Přečtěte si článek [připojení k obrázku na základě Linux HDInsight z Windows](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster)

3. Umožňuje instalaci FreeTDS tento příkaz:

        sudo apt-get --assume-yes install freetds-dev freetds-bin

    FreeTDS se použije v několika krocích připojení k databázi SQL.

##<a name="create-the-table-in-sql-database"></a>Vytvoření tabulky databáze SQL

> [AZURE.IMPORTANT] Pokud používáte HDInsight obrázku a SQL databáze vytvořené pomocí kroků [vytvořit obrázku](hdinsight-use-sqoop.md)i SQL databázi, ignorujte kroků v této části jako databázi a tabulku byly vytvořeny v rámci kroky v tomto dokumentu.

1. Z SSH připojení k Hdinsightu pomocí následujícího příkazu pro připojení k databázi SQL serveru a Kréty tabulky, která se použije ve zbývající části takto:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Zobrazí se výstup následujícím způsobem:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to sqooptest
        1>

5. V `1>` objeví se výzva, zadejte následující příkazy:

        CREATE TABLE [dbo].[mobiledata](
        [clientid] [nvarchar](50),
        [querytime] [nvarchar](50),
        [market] [nvarchar](50),
        [deviceplatform] [nvarchar](50),
        [devicemake] [nvarchar](50),
        [devicemodel] [nvarchar](50),
        [state] [nvarchar](50),
        [country] [nvarchar](50),
        [querydwelltime] [float],
        [sessionid] [bigint],
        [sessionpagevieworder] [bigint])
        GO
        CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)
        GO

    Když `GO` údajů se zadává, bude vyhodnocen předchozí příkazy. Nejdřív se vytvoří tabulka **mobiledata** , potom seskupený index se přidá k němu (musí tak, že databáze SQL).

    Použijte následující ověřit, že byl vytvořen v tabulce:

        SELECT * FROM information_schema.tables
        GO

    Měli byste vidět výstup následujícím způsobem:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        sqooptest       dbo     mobiledata      BASE TABLE

8. Zadejte `exit` v `1>` výzva k ukončení nástroje tsql.

##<a name="sqoop-export"></a>Sqoop export

3. Z SSH připojení k Hdinsightu, se tento příkaz ověření, že Sqoop vidí databázi SQL:

        sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>

    To, měly vrátit seznam databází, včetně **sqooptest** databáze, který jste vytvořili.

4. Pomocí následujícího příkazu Exportovat data z **hivesampletable** **mobiledata** tabulky:

        sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --export-dir 'wasbs:///hive/warehouse/hivesampletable' --fields-terminated-by '\t' -m 1

    Tím se nastaví Sqoop se můžete připojit k databázi SQL **sqooptest** databáze a exportovat data z **wasbs: / / / podregistru/sklad/hivesampletable** (fyzické soubory pro *hivesampletable*) k tabulce **mobiledata** .

5. Po dokončení příkazu připojení k databázi pomocí TSQL pomocí následující:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Po připojení, použijte následující příkazy pro ověření, že se data byla exportovat tabulku **mobiledata** :

        SELECT * FROM mobiledata
        GO

    Měli byste vidět přehled dat v tabulce. Typ `exit` ukončete nástroj tsql.

##<a name="sqoop-import"></a>Sqoop import

1. Umožňuje import dat z tabulky **mobiledata** do SQL databáze na následující **wasbs: / / / výukové programy pro/usesqoop/importeddata** adresáře na HDInsight:

        sqoop import --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasbs:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1

    Importovaná data bude obsahovat pole, která jsou odděleny znak tabulátoru a řádcích bude ukončena znakem nový řádek.

2. Po dokončení importu zadejte následující příkaz do seznamu se data v novém adresáři:

        hadoop fs -text wasbs:///tutorials/usesqoop/importeddata/part-m-00000

##<a name="using-sql-server"></a>Pomocí serveru SQL Server

Taky můžete Sqoop import a export dat z SQL serveru, v datovém centru nebo na počítač virtuální hostované v Azure. Rozdíly mezi použitím databáze a SQL serveru SQL Server je:

* HDInsight i ze serveru SQL musí být ve stejné síti virtuální Azure

    > [AZURE.NOTE] HDInsight podporuje pouze na základě umístění virtuální sítě a nefunguje aktuálně spřažení skupinových virtuální sítích.

    Při použití systému SQL Server ve vaší datacentru, musíte nakonfigurovat virtuální sítě jako *webu webu* nebo *čárky webu*.

    > [AZURE.NOTE] Virtuální sítích **čárky webu** SQL Server musí běžet klienta VPN konfigurace aplikace, která je dostupná z **řídicího panelu** konfigurace Azure virtuální sítě.

    Podrobnosti Azure virtuální sítě najdete v článku [Přehled virtuální sítě](../virtual-network/virtual-networks-overview.md).

* SQL Server musí konfigurace umožňuje ověřování serveru SQL. Další informace najdete v tématu [Volba režimu ověřování](https://msdn.microsoft.com/ms144284.aspx)

* Možná budete muset konfigurace systému SQL Server přijmout vzdáleného připojení. Další informace najdete v článku [jak řešit problémy s připojením k databázový stroj SQL serveru](http://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx)

* Je nutné vytvořit **sqooptest** databázi pomocí nástroje **SQL Server Management Studio** ATP **tsql** serveru SQL Server – pokyny pro použití rozhraní příkazového řádku Azure fungují jen pro databázi SQL Azure

    Podobají TSQL příkazy k vytvoření tabulky **mobiledata** klávesovými zkratkami databáze SQL, s výjimkou vytváření clusterd indexu - tento krok není povinný pro systém SQL Server:

        CREATE TABLE [dbo].[mobiledata](
        [clientid] [nvarchar](50),
        [querytime] [nvarchar](50),
        [market] [nvarchar](50),
        [deviceplatform] [nvarchar](50),
        [devicemake] [nvarchar](50),
        [devicemodel] [nvarchar](50),
        [state] [nvarchar](50),
        [country] [nvarchar](50),
        [querydwelltime] [float],
        [sessionid] [bigint],
        [sessionpagevieworder] [bigint])

* Při připojení k systému SQL Server z Hdinsightu, bude pravděpodobně nutné použít IP adresu serveru SQL Server, pokud jste nakonfigurovali systému DNS (Domain Name) jmen v síti virtuální Azure. Příklad:

        sqoop import --connect 'jdbc:sqlserver://10.0.1.1:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasbs:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1

##<a name="limitations"></a>Omezení

* Hromadné export - na základě s Linux HDInsight, konektoru Sqoop použít pro export dat do aplikace Microsoft SQL Server nebo databáze SQL Azure aktuálně nepodporuje vloží hromadně.

* Dávkové - se systémem Linux HDInsight při použití `-batch` při provádění vloží přepnout, Sqoop provede více vloží místo dávky operace vložení.

##<a name="next-steps"></a>Další kroky

Teď jste se naučili používání Sqoop. Další informace najdete v tématu:

- [Použití Oozie s HDInsight][hdinsight-use-oozie]: použití Sqoop akce v pracovním postupu Oozie.
- [Analýza dat zpoždění letů pomocí HDInsight][hdinsight-analyze-flight-data]: použití podregistru a analyzujte data letu zpozdit dat a pak pomocí Sqoop export dat do databáze Azure SQL.
- [Odeslání dat do HDInsight][hdinsight-upload-data]: Najít další způsoby uložení dat k úložišti objektů Blob HDInsight/Azure.



[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database-create-configure.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
