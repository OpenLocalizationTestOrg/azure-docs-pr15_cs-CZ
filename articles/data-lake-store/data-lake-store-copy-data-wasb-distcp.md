<properties
   pageTitle="Zkopírujte data do a z WASB do úložiště jezera dat pomocí Distcp | Microsoft Azure"
   description="Nástroj pro správu Distcp ke kopírování dat z Azure úložiště objektů BLOB a k úložišti jezera dat."
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="use-distcp-to-copy-data-between-azure-storage-blobs-and-data-lake-store"></a>Použití Distcp přesouvat data mezi objekty BLOB Azure úložiště a jezera úložiště dat

> [AZURE.SELECTOR]
- [Použití DistCp](data-lake-store-copy-data-wasb-distcp.md)
- [Použití AdlCopy](data-lake-store-copy-data-azure-storage-blob.md)


Po vytvoření HDInsight clusteru, který má oprávnění k přístupu k účtu úložiště jezera dat můžete pomocí nástrojů ekosystému Hadoop jako Distcp ke kopírování dat **do a z** úložiště clusteru HDInsight (WASB) do úložiště jezera dat účtu. Tento článek obsahuje pokyny o tom, jak dosáhnout.

##<a name="prerequisites"></a>Zjistit předpoklady pro

Než začnete v tomto článku, musíte mít takto:

- **Azure předplatného**. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).
- **Povolení předplatného Azure** jezera úložiště veřejného (verze Preview). V tématu [pokyny](data-lake-store-get-started-portal.md#signup).
- **Azure HDInsight obrázku** s přístupem k úložišti jezera účtu. V tématu [Vytvoření HDInsight obrázku s úložištěm jezera Data](data-lake-store-hdinsight-hadoop-use-portal.md). Zkontrolujte, jestli že povolíte připojení ke vzdálené ploše pro clusteru.

## <a name="do-you-learn-fast-with-videos"></a>Zjistěte jak rychle se videa?

[Podívejte se na toto video](https://mix.office.com/watch/1liuojvdx6sie) o tom, jak přesouvat data mezi objekty BLOB Azure úložiště a jezera úložiště dat pomocí DistCp.

## <a name="use-distcp-from-remote-desktop-windows-cluster-or-ssh-linux-cluster"></a>Použití Distcp z Vzdálená plocha (Windows obrázku) nebo SSH (Linux clusteru)

HDInsight clusteru je součástí Distcp nástroje, které slouží ke kopírování dat z různých zdrojů do clusteru HDInsight. Pokud jste nakonfigurovali clusteru HDInsight používat úložiště jezera dat jako dalšího úložiště, může být nástroj Distcp použité mimo předdefinovaných ke kopírování dat z účet úložiště jezera dat a. V této části podíváme na to, jak používat nástroj Distcp.

1. Pokud máte Windows obrázku, vzdálené do HDInsight obrázku, který má přístup k účtu úložiště jezera dat. Pokyny najdete v tématu [připojení clusterů pomocí RDP](../hdinsight/hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp). Z obrázku plochy otevřete Hadoop příkazového řádku.

    Pokud máte Linux clusteru, umožňuje SSH připojte se k němu. Přečtěte si článek [připojení k obrázku na základě Linux HDInsight](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster). Spusťte příkazy z příkazového řádku SSH.

3. Zkontrolujte, jestli máte přístup k Azure úložiště objektů BLOB (WASB). Spusťte tento příkaz:

        hdfs dfs –ls wasb://<container_name>@<storage_account_name>.blob.core.windows.net/

    To by měl obsahovat seznam obsahu v úložišti objektů blob.

4. Podobně ověřte, zda můžete získat přístup k účtu úložiště jezera dat z clusteru. Spusťte tento příkaz:

        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net:443/

    To by měl obsahovat seznamu souborů a složek v úložišti jezera účtu.

5. Kopírování dat z WASB k účtu úložiště jezera dat pomocí Distcp.

        hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder

    Zkopírujete WASB obsah **** složky/Příklad/data/gutenberg/k **/myfolder** v úložišti jezera účtu.

6. Podobně použijte Distcp ke kopírování dat z účtu úložiště jezera dat do WASB.

        hadoop distcp adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg

    Zkopírujete účtu úložiště jezera dat obsah **** **** /myfolder/Příklad/data/gutenberg/v do složky WASB.

## <a name="see-also"></a>Viz taky

- [Kopírování dat z Azure úložiště objektů BLOB jezera úložiště dat](data-lake-store-copy-data-azure-storage-blob.md)
- [Zabezpečení dat v úložišti jezera dat](data-lake-store-secure-data.md)
- [Použití analýzy jezera Azure dat s jezera úložiště dat](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Pomocí úložiště jezera dat Azure HDInsight](data-lake-store-hdinsight-hadoop-use-portal.md)
