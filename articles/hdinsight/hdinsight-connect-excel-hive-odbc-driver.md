<properties
   pageTitle="Připojit Excel k Hadoop s ovladač ODBC podregistru | Microsoft Azure"
   description="Zjistěte, jak nastavit a používat ovladač ODBC podregistru Microsoft pro Excel dotazy na data HDInsight obrázku."
   services="hdinsight"
   documentationCenter=""
   authors="mumian"
   manager="jhubbard"
   tags="azure-portal"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/19/2016"
   ms.author="jgao"/>

#<a name="connect-excel-to-hadoop-with-the-microsoft-hive-odbc-driver"></a>Připojit Excel k Hadoop pomocí ovladač ODBC podregistru společnosti Microsoft

[AZURE.INCLUDE [ODBC-JDBC-selector](../../includes/hdinsight-selector-odbc-jdbc.md)]

Řešení společnosti Microsoft velký dat lze Microsoft Business Intelligence (BI) součásti integrovat s Apache Hadoop clusterů, které již byly nasazeny Azure HDInsight. Příklad Tato integrace je možnost připojit Excel k datový sklad podregistru clusteru Hadoop v HDInsight pomocí ovladače Microsoft podregistru připojení ODBC (Open Database).

Také je možné se připojit data spojená se HDInsight obrázku a jiných zdrojů dat, včetně dalších clusterů Hadoop (bez HDInsight), z Excelu pomocí doplňku Microsoft Power Query pro Excel. Informace o instalaci a pomocí Power Query najdete v tématu [Připojit Excel k Hdinsightu pomocí Power Query][hdinsight-power-query].

> [AZURE.NOTE] Během kroků v tomto článku je možné s Linux nebo serveru s Windows HDInsight clusteru je potřebný pro workstation klienta Windows.

**Požadavky**:

Než začnete v tomto článku, musíte mít takto:

- **HDInsight obrázku**. Vytvořte si ho, najdete v článku [Začínáme s Azure HDInsight][hdinsight-get-started].
- **A workstation** s Office 2013 Professional Plus, Office 365 Pro Plus, samostatný Excel 2013 nebo Office 2010 Professional Plus.


##<a name="install-microsoft-hive-odbc-driver"></a>Nainstalovat ovladač ODBC podregistru společnosti Microsoft

Stáhněte a nainstalujte Microsoft podregistru ovladač ODBC z [Webu služby Stažení softwaru][hive-odbc-driver-download].

Ovladač je možné nainstalovat 32bitovou nebo 64bitovou verzí systému Windows 7, Windows 8, Windows 10, Windows Server 2008 R2 a Windows Server 2012 a umožní připojení k Azure HDInsight (verze 1,6 a novější) a Azure HDInsight emulátoru (v.1.0.0.0 a novější). Měli byste nainstalovat verzi, která se shoduje s verzí aplikace, které budete používat ovladač ODBC. Pro účely tohoto návodu ovladač použijí z aplikace Office Excel.

##<a name="create-hive-odbc-data-source"></a>Vytvoření zdroje dat podregistru ODBC

Podle těchto kroků ukazují, jak vytvořit zdroj dat ODBC podregistru.

1. V aplikaci Windows 8 nebo Windows 10 stiskněte klávesu Windows a otevřete úvodní obrazovku a zadejte **zdroje dat**.
2. Klikněte na **Nastavení dat ODBC zdrojů (32bitový)** nebo **Nastavení zdrojů dat ODBC (64bitový)** podle verze Office. Pokud používáte Windows 7, zvolte z **Nástroje pro správu** **Zdrojů dat ODBC (32 bitů)** nebo **Zdrojů dat ODBC (64bitová)** . Toto dialogové okno **Správce zdrojů dat ODBC** , spustí.

    ![Správce zdrojů dat ODBC][img-hdi-simbahiveodbc-datasource-admin]

3. V DNS uživatele klikněte na **Přidat** otevřete průvodce **Vytvořit nový zdroj dat** .
4. Vyberte **Ovladač ODBC podregistru Microsoft**a potom klikněte na **Dokončit**. To, spustí se dialogové okno **Microsoft DNS ovladač ODBC podregistru nastavení** .

5. Zadejte nebo vyberte následující hodnoty:

    Vlastnost|Popis
    ---|---
    Název zdroje dat|Pojmenujte ke zdroji dat
    Host (hostitel)|Zadejte &lt;HDInsightClusterName >. azurehdinsight.net. Například myHDICluster.azurehdinsight.net
    Port|Použití <strong>443</strong>. (Tento port změnil z 563 k 443.)
    Databáze|Použijte <strong>výchozí</strong>.
    Typ serveru podregistru|Vyberte <strong>podregistru Server 2</strong>
    Mechanismus|Vyberte <strong>Službu Azure HDInsight</strong>
    Cesta HTTP|Nechejte pole prázdné.
    Uživatelské jméno|Zadejte uživatelské jméno uživatele clusteru HDInsight. To je vytvořen během procesu poskytování clusteru. Pokud jste použili možnost vytvořit, je výchozí uživatelské jméno <strong>Správce</strong>.
    Heslo|Zadejte heslo k uživatelskému clusteru HDInsight.
    </table>

    Existují některé důležité parametry nějaká ze kdy klikněte na **Upřesnit možnosti**:

    Parametr|Popis
    ---|---
    Nativní dotaz|Pokud je vybrána, ovladač ODBC není pokusí převést TSQL HiveQL. Se použije pouze v případě, že jste odeslali čistého příkazy HiveQL 100 %. Při připojení k databázi SQL Azure SQL Server nebo by měl nechte zaškrtnuté.
    Řádky načtena podle bloku|Při načítání velkého počtu záznamů, optimalizace tento parametr může být nutné zajistit optimální hudebního vystoupení.
    Výchozí sloupce délka řetězce, délka binární sloupec, měřítko desetinné sloupce|Datový typ délky a přesnosti by mohly ovlivnit způsob je vrácena data. Způsobují nesprávné informace budou vráceny kvůli ztráta přesnosti a/nebo oříznutí.


    ![Upřesnit možnosti][img-HiveOdbc-DataSource-AdvancedOptions]

6. Kliknutím na tlačítko **Testovat** testování zdroje dat. Zdroj dat je správně nakonfigurované, zobrazuje *testů se úspěšně DOKONČIL!*.
7. Klikněte na **OK** zavřete Test dialog. Nový zdroj dat teď měl seznam obsahovat na **Správce zdrojů dat ODBC**.
8. Klikněte na **OK** zavřete průvodce.

##<a name="import-data-into-excel-from-hdinsight"></a>Import dat do Excelu z Hdinsightu

Následující kroky popisují způsob, jak importovat data z tabulku podregistru do Excelového sešitu pomocí zdroj dat ODBC, kterou jste vytvořili ve výše uvedené kroky.

1. Otevřete nový nebo existující sešit v Excelu.
2. Na kartě **Data** klikněte na **Z jiných zdrojů dat**a potom klikněte na **Z Průvodce datovým připojením** ke spuštění **Průvodce datovým připojením**.

    ![Průvodce otevřít datovým připojením][img-hdi-simbahiveodbc.excel.dataconnection]

3. Vyberte **Zdroj dat ODBC** jako zdroje dat a klikněte na tlačítko **Další**.
4. Ze zdrojů dat ODBC vyberte název zdroje dat, který jste vytvořili v předchozím kroku a klikněte na tlačítko **Další**.
5. Zadejte znovu heslo pro cluster v průvodci a klikněte na **Test** k ověření konfigurace ještě jednou případné potíže.
6. Klikněte na **OK** zavřete test dialog.
7. Klikněte na **OK**. Počkejte na dialogové okno **Vybrat databázi a tabulku** otevřete. To může trvat několik sekund, než.
8. Vyberte tabulku, která chcete importovat a klikněte na tlačítko **Další**. *Hivesampletable* je ukázkovou tabulku podregistru, které jsou součástí clusterů HDInsight.  Pokud jste nevytvořili ji můžete. Další informace o spuštění podregistru dotazů a vytváření tabulek podregistru, najdete v článku [Použití podregistru s HDInsight][hdinsight-use-hive].
8. Klikněte na **Dokončit**.
9. V dialogovém okně **Importovat Data** můžete změnit nebo zadejte dotaz. K tomu, klikněte na **Vlastnosti**. To může trvat několik sekund, než.
10. Klikněte na kartu **definice** a pak připojit **LIMIT 200** k příkazu SELECT podregistru do textového pole **příkazu text** . Změna omezí sadu navráceném záznamu s prioritou 200.

    ![Vlastnosti připojení][img-hdi-simbahiveodbc-excel-connectionproperties]

11. Klikněte na **OK** zavřete dialogové okno Vlastnosti připojení.
12. Klikněte na **OK** zavřete dialogové okno **Importovat Data** .  
13. Zadejte heslo znovu a klikněte na **OK**. Trvá několik sekund, než se data importují do Excelu.

##<a name="next-steps"></a>Další kroky

V tomto článku se naučíte používat ovladač ODBC podregistru Microsoft k načtení dat z Hdinsightu služby do Excelu. Podobně načtení dat z Hdinsightu služby do SQL databáze. Je také možné odeslat data do služby HDInsight. Další informace najdete v tématu:

- [Analýza dat zpoždění letů pomocí HDInsight][hdinsight-analyze-flight-data]
- [Odeslání dat do HDInsight][hdinsight-upload-data]
- [Použití Sqoop s HDInsight] [hdinsight-use-sqoop]


[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-get-started]: hdinsight-hadoop-tutorial-get-started-windows.md

[hive-odbc-driver-download]: http://go.microsoft.com/fwlink/?LinkID=286698

[img-hdi-simbahiveodbc-datasource-admin]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.DataSourceAdmin1.png
[img-HiveOdbc-DataSource-AdvancedOptions]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.HiveOdbc.DataSource.AdvancedOptions1.png
[img-hdi-simbahiveodbc-excel-connectionproperties]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveODBC.Excel.ConnectionProperties1.png
[img-hdi-simbahiveodbc.excel.dataconnection]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.Excel.DataConnection1.png
