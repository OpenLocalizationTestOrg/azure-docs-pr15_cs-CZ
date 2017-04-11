<properties
    pageTitle="Připojit Excel k Hadoop pomocí Power Query | Microsoft Azure"
    description="Informace o využití výhod komponenty business intelligence a přístup k datům uloženým v Hadoop na Hdinsightu pomocí Power Query pro Excel."
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
    ms.date="10/19/2016"
    ms.author="jgao"/>


#<a name="connect-excel-to-hadoop-by-using-power-query"></a>Připojit Excel k Hadoop pomocí Power Query

Jeden klíčové funkce ze velký datové řešení Microsoft je integrace komponenty Microsoft business intelligence (BI) s Hadoop clusterů Azure HDInsight. Hlavní příklady Tato integrace je možnost připojení k úložišti Azure účet, který obsahuje data spojená se svůj cluster Hadoop pomocí Microsoft Power Query pro Excel doplňku aplikace Excel. Tento článek vás provede jak nastavit a používat Power Query do dotazu údaje související s Hadoop clusteru správy pomocí HDInsight.

> [AZURE.NOTE] Během kroků v tomto článku je možné s Linux nebo serveru s Windows HDInsight clusteru je potřebný pro workstation klienta Windows.

### <a name="prerequisites"></a>Zjistit předpoklady pro

Než začnete v tomto článku, musíte mít takto:

- **HDInsight obrázku**. Abyste mohli nakonfigurovat jeden, najdete v článku [Začínáme s Azure HDInsight][hdinsight-get-started].
- **A workstation** se systémem Windows 7, Windows Server 2008 R2 nebo novější operační systém.
- **Office 2013 Professional Plus, Office 365 ProPlus, samostatný Excel 2013 nebo Office 2010 Professional Plus**.


## <a name="install-power-query"></a>Instalace Power Query

Power Query slouží k importu dat z různých zdrojů do aplikace Microsoft Excel, kde je power BI nástroje jako Powerpivotu a Power View. Zejména Power Query můžete importovat data, která výstup nebo, která obsahuje generované Hadoop úlohy výpočetnímu clusteru HDInsight.

Stažení Microsoft Power Query pro Excel ze [Služby Stažení softwaru] [ powerquery-download] a nainstalujte ji.

## <a name="import-hdinsight-data-into-excel"></a>HDInsight importovat data do Excelu

Doplněk Power Query pro Excel umožňuje snadno import dat z Hdinsightu obrázku do Excelu, kde nástrojích BI jako PowerPivot a Power Map lze zkontrolovat, analyzovat a prezentovat data.

**Import dat z Hdinsightu obrázku**

1. Otevřete Excel.

2. Vytvořte nový prázdný sešit.

3. Klikněte v nabídce **Power Query** , klikněte na **Z Azure**a potom klikněte na **Z Microsoft Azure HDInsight**.

    ![HDI. PowerQuery.SelectHdiSource][image-hdi-powerquery-hdi-source]

    **Poznámka:** Pokud nevidíte v nabídce **Power Query** , přejděte na **soubor** > **Možnosti** > **Doplňky**a vyberte **Doplňky modelu COM** z rozevíracího seznamu **Spravovat** v dolní části stránky. Klikněte na tlačítko **Přejít …** a ověřte, jestli je zaškrtnuté políčko pro Power Query pro Excel doplňku.

    **Poznámka:** Power Query můžete taky importovat data z HDFS po kliknutí na **Z jiných zdrojů**.

3. **Název účtu**zadejte název účtu úložiště objektů Blob Azure přidružený k vaší obrázku a klikněte na **OK**. To je [výchozí úložiště účet](hdinsight-administer-use-management-portal.md#find-the-default-storage-account) nebo účet propojené úložiště.  Formát je *https://<StorageAccountName>.blob.core.windows.net/*.

4. **Klíč účtu**zadejte klíč účtu úložiště objektů Blob a potom klikněte na **Uložit**. (Potřebujete udělat jenom poprvé přístupu k tomuto úložišti.)

5. V **navigačním** podokně na levé straně editoru dotazů poklikejte na jméno container úložiště objektů Blob. Ve výchozím nastavení je jméno container stejný název jako název clusteru.

6. Vyhledejte **hivesampledata.txt ze** ve sloupci **název** (cestu ke složce je **... / podregistru/sklad/hivesampletable/**) a potom klikněte na **binární** na levé straně hivesampledata.txt ze. Hivesampledata.txt ze je součástí všech obrázku. Pokud chcete můžete použít vlastní soubor.

    ![HDI. PowerQuery.ImportData][image-hdi-powerquery-importdata]

7. Pokud chcete, můžete přejmenovat názvy sloupců. Až budete připraveni, klikněte na **Zavřít a načíst**.  Načtení dat k sešitu:

    ![HDI. PowerQuery.ImportedTable][image-hdi-powerquery-imported-table]

## <a name="next-steps"></a>Další kroky

V tomto článku se naučíte používat Power Query k načtení dat z Hdinsightu do Excelu. Podobně můžete data načtete z Hdinsightu do databáze SQL Azure. Je také možné odeslat data do HDInsight. Další informace naleznete v následujících článcích:

* [Připojit Excel k Hdinsightu pomocí Microsoft podregistru ovladač ODBC][hdinsight-ODBC]
* [Odeslání dat do HDInsight][hdinsight-upload-data]

[hdinsight-ODBC]: hdinsight-connect-excel-hive-odbc-driver.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[image-hdi-powerquery-hdi-source]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.SelectHdiSource.png
[image-hdi-powerquery-importdata]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.ImportData.png
[image-hdi-powerquery-imported-table]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.ImportedTable.PNG

[powerquery-download]: http://go.microsoft.com/fwlink/?LinkID=286689
