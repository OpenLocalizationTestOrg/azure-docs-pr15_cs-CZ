<properties
    pageTitle="Import dat do počítače výukové Studio z online zdrojů | Microsoft Azure"
    description="Návod k importu dat školení Azure počítače výukové Studio z různých zdrojů online."
    keywords="Importujte dat, formát dat ve sloupcích, datové typy, zdrojů dat, školení dat"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="bradsev;garye" />


# <a name="import-data-into-azure-machine-learning-studio-from-various-online-data-sources-with-the-import-data-module"></a>Import dat do Azure počítače výukové Studio z různých zdrojů dat online s modulu importovat Data

Tento článek popisuje podporu pro import online dat z různých zdrojů a informace potřebné k přesunutí dat z těchto zdrojů do pokus výukové počítače Azure.

> [AZURE.NOTE] Tento článek obsahuje obecné informace o [Importu dat] [ import-data] modul. Podrobné informace o typech data se dostanete, formáty, parametry a odpovědi na běžné otázky, naleznete v tématu odkaz modulu pro [Import dat] [ import-data] modul.

<!-- -->

[AZURE.INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

## <a name="introduction"></a>Úvod

Dostanete dat z aplikace Studio Azure počítače výukové z několika zdrojů dat online pomocí [Importovat Data] je spuštěná vaší experiment[ import-data] modulu:

- Adresu URL webu pomocí protokolu HTTP
- Hadoop pomocí HiveQL
- Úložiště objektů blob Azure
- Tabulek Azure
- Azure SQL databáze nebo SQL Server na Azure OM
- Místní databáze SQL serveru
- Datového kanálu poskytovatele aktuálně OData
 
Pracovní postup pro provádění pokusů v Azure počítače výukové Studio se skládá z přetažením a odstranění součástí na plátno. Přístup ke zdrojům dat online, přidání [Importovat Data] [ import-data] modul testu, vyberte **zdroj dat**a zadejte potřebné pro přístup k datům parametry. V následující tabulce jsou rozpis online data zdroje, které jsou podporovány. Tato tabulka shrnuje také formáty souborů, které podporují a parametrů, které se používají pro přístup k datům.

Všimněte si, protože tato data školení přístupná je spuštěná vaší testu, je k dispozici pouze v testu. Naproti uložené v modulu datovou sadu dat jsou dostupné pro všechny experiment v pracovním prostoru.

> [AZURE.IMPORTANT] V současné době [Importovat Data] [ import-data] a [Export dat] [ export-data] moduly můžete čtení a zápis dat pouze z Azure úložiště vytvořené pomocí klasického nasazení modelu. Jinými slovy nový typ klienta úložišti objektů Blob Azure nabízející úroveň přístupu žádanou úložiště nebo úroveň přístupu zajímavých úložiště není zatím podporované. 

> Obecně vzato všechny účty Azure úložiště, že jste případně vytvořili před dostupná možnost služby neměli mít vliv na. Pokud potřebujete k vytvoření nového účtu, vyberte **klasické** pro nasazení modelu, nebo pomocí Správce prostředků a **Typ účtu**vyberte **univerzální** spíše než **úložiště objektů Blob**. 

> Další informace najdete v tématu [úložišti objektů Blob Azure: aktivní a zajímavých úložiště úrovní](../storage/storage-blob-storage-tiers.md).



## <a name="supported-online-data-sources"></a>Podporované zdroje dat online
Modul Azure počítače výukové **Importovat Data** podporuje následující zdroje dat:

Zdroj dat | Popis | Parametry |
---|---|---|
Webová adresa URL prostřednictvím protokolu HTTP |Načte data v hodnoty oddělené čárkami (CSV), odděleného tabulátory hodnoty (TSV), formát souboru atribut vztahem (ARFF) a formáty podpory vektorové počítačích (SVM light) z libovolné adresu URL, která používá HTTP | <b>Adresa URL</b>: Určuje úplný název souboru, včetně názvu souboru s libovolnou linku a adresu URL webu. <br/><br/><b>Formát dat ve sloupcích</b>: Určuje jeden z podporovaných data formáty: CSV, TSV, ARFF nebo SVM light. Pokud data obsahují řádek záhlaví, se používá k přiřazení názvů sloupců.|
Hadoop/HDFS|Načte data z distribuované úložiště na Hadoop. Zadejte data, která chcete pomocí HiveQL, což je dotazovací SQL profesionálové jazyk. HiveQL lze také agregovat data a provádět filtrování před přidávat data do počítače výukové Studio.|<b>Podregistru databázových dotazů</b>: Určuje podregistru dotazu použito k vygenerování data.<br/><br/><b>Identifikátor URI HCatalog serveru</b> : určení názvu svého obrázku ve formátu * &lt;názvu clusteru&gt;. azurehdinsight.net.*<br/><br/><b>Název účtu uživatele Hadoop</b>: Určuje název účtu uživatele Hadoop zřízení clusteru.<br/><br/><b>Hesla uživatelského účtu Hadoop</b> : Určuje přihlašovacích údajů použitých při zřizování clusteru. Další informace najdete v tématu [Vytvoření Hadoop clusterů HDInsight](../hdinsight/hdinsight-provision-clusters.md).<br/><br/><b>Umístění výstupní data</b>: Určuje, jestli jsou data uložena v Hadoop distributed file system (HDFS) nebo v Azure. <br/><ul>Pokud ukládáte výstupní data HDFS, zadejte do HDFS server URI. (Nezapomeňte použít název clusteru HDInsight bez předponou HTTPS://). <br/><br/>Pokud uložit výstupní data v Azure zadejte název účtu Azure úložiště, úložiště přístupové klávesy a jméno container úložiště.</ul>|
Databáze SQL |Načte data, která je uložena v databázi Azure SQL nebo v databázi serveru SQL Server Azure virtuální počítač se systémem. | <b>Název databázového serveru</b>: Určuje název serveru, na kterém běží databáze.<br/><ul>V případě databáze SQL Azure zadejte název serveru, který je generováno. Obvykle obsahuje formulář * &lt;generated_identifier&gt;. database.windows.net.* <br/><br/>V případě SQL server hostitelem Azure virtuální počítač zadejte *tcp:&lt;název DNS virtuálního počítače&gt;, 1433*</ul><br/><b>Název databáze </b>: Určuje název databáze na serveru. <br/><br/><b>Název serveru uživatelského účtu</b>: Určuje uživatelské jméno účtu, který má oprávnění pro databázi. <br/><br/><b>Server heslo</b>: heslo k účtu uživatele určí.<br/><br/><b>Přijmout všechny certifikát serveru</b>: Tato možnost (méně bezpečné) použijte, pokud chcete přeskočit revizí certifikát serveru před další data.<br/><br/><b>Databázových dotazů</b>: Zadejte příkaz SQL, který popisuje dat si chcete přečíst.|
Místní databáze SQL |Načte data, která je uložena v databázi SQL místní. | <b>Brána dat</b>: název brány pro správu dat nainstalovaný v počítači místo, kam má přístup k databázi SQL serveru. Informace o nastavení brány najdete v tématu [provést pokročilé analýzy s využívající data z místního serveru SQL Azure počítače výukové](machine-learning-use-data-from-an-on-premises-sql-server.md).<br/><br/><b>Název databázového serveru</b>: Určuje název serveru, na kterém běží databáze.<br/><br/><b>Název databáze </b>: Určuje název databáze na serveru. <br/><br/><b>Název serveru uživatelského účtu</b>: Určuje uživatelské jméno účtu, který má oprávnění pro databázi. <br/><br/><b>Uživatelské jméno a heslo</b>: klepněte na <b>Enter hodnot</b> a ty pak zadejte svoje přihlašovací údaje databáze. Můžete použít ověřování systému Windows nebo ověřování serveru SQL Server v závislosti na konfiguraci místního serveru SQL.<br/><br/><b>Databázových dotazů</b>: Zadejte příkaz SQL, který popisuje dat si chcete přečíst.|
Azure Table|Načte data z tabulky služby v úložišti Azure.<br/><br/>Pokud často číst velké objemy dat použijte tabulku služba Azure. Nabízí flexibilní,-relační (NoSQL), datových scalable levný a vysoce dostupné úložiště řešení.| Možnosti **Importu dat** změnit podle toho, zda přistupujete informace veřejné nebo soukromé úložiště účtu, který vyžaduje přihlašovací údaje. To je určený podle <b>Typ ověřování</b> která může obsahovat hodnotu "PublicOrSAS" nebo "Účtu", z nichž každá obsahuje vlastní sadu parametrů. <br/><br/><b>Veřejná nebo sdílené přístup podpisu (přidružení zabezpečení) URI</b>: parametry jsou:<br/><br/><ul><b>Identifikátor URI tabulky</b>: Určuje veřejné nebo adresu URL přidružení zabezpečení pro tabulku.<br/><br/><b>Určuje řádky, které chcete vyhledat názvy vlastností</b>: hodnoty jsou <i>funkce TopN</i> na zadaný počet řádků nebo <i>ScanAll</i> zobrazíte všechny řádky v tabulce. <br/><br/>Pokud jsou data homogenní a předvídatelná, je vhodné vyberte *TopN* a zadejte číslo N. Pro velké tabulky může být výsledkem rychlejší čtení časy.<br/><br/>Pokud se sadami vlastností, které se liší v závislosti na hloubka a umístění tabulky je strukturovaná data, Odsouhlasíte *ScanAll* prohledat všechny řádky. Díky integrity výsledné vlastnost a převod metadata.<br/><br/></ul><b>Soukromé úložiště účtu</b>: parametry jsou: <br/><br/><ul><b>Název účtu</b>: název účtu, který obsahuje tabulky pro čtení.<br/><br/><b>Klíč účtu</b>: Určuje klávesu úložiště přidružené k účtu.<br/><br/><b>Název tabulky</b> : název tabulky, která obsahuje data, která chcete číst.<br/><br/><b>Řádky, které chcete vyhledat názvy vlastností</b>: hodnoty jsou <i>TopN</i> na zadaný počet řádků nebo <i>ScanAll</i> zobrazíte všechny řádky v tabulce.<br/><br/>Pokud jsou data homogenní a předvídatelná, doporučujeme vyberte *TopN* a zadejte číslo N. Pro velké tabulky může být výsledkem rychlejší čtení časy.<br/><br/>Pokud se sadami vlastností, které se liší v závislosti na hloubka a umístění tabulky je strukturovaná data, Odsouhlasíte *ScanAll* prohledat všechny řádky. Díky integrity výsledné vlastnost a převod metadata.<br/><br/>|
Úložiště objektů Blob Azure | Načte data uložená ve službě objektů Blob Azure úložiště, včetně obrázků, nestrukturovaná text nebo binárními daty.<br/><br/>Službu objektů Blob veřejně vystavit data nebo soukromě ukládání dat aplikace. K datům mohli dostat odkudkoli pomocí připojení HTTP nebo HTTPS. | Možnosti v modulu **Importovat Data** se mění v závislosti na tom, jestli přistupujete informace veřejné nebo soukromé úložiště účtu, který vyžaduje přihlašovací údaje. To je určený podle <b>Typu ověřování</b> , která může obsahovat hodnotu "PublicOrSAS" nebo "Účtu".<br/><br/><b>Veřejná nebo sdílené přístup podpisu (přidružení zabezpečení) URI</b>: parametry jsou:<br/><br/><ul><b>Identifikátor URI</b>: Určuje veřejná nebo adresu URL přidružení zabezpečení pro úložiště objektů blob.<br/><br/><b>Formát souboru</b>: Určuje formát dat ve službě objektů Blob. Podporované formáty jsou CSV, TSV a ARFF.<br/><br/></ul><b>Soukromé úložiště účtu</b>: parametry jsou: <br/><br/><ul><b>Název účtu</b>: název účtu, který obsahuje objektů blob si chcete přečíst.<br/><br/><b>Klíč účtu</b>: Určuje klávesu úložiště přidružené k účtu.<br/><br/><b>Cesta k kontejneru, adresáře nebo objektů blob</b> : název, který obsahuje data, která chcete číst objektů blob.<br/><br/><b>Formát souboru objektů BLOB</b>: Určuje formát dat ve službě objektů blob. Formáty podporované dat se CSV TSV, ARFF, CSV uvedené kódování a aplikace Excel. <br/><br/><ul>Pokud je formátu CSV nebo TSV, je potřeba označuje, jestli soubor obsahuje řádek záhlaví.<br/><br/>Možnosti aplikace Excel umožňuje číst data z Excelových sešitů. V možnost <i>Formát dat aplikace Excel</i> určit, jestli jsou data v oblasti listu aplikace Excel nebo v tabulce aplikace Excel. V možnosti <i>listu aplikace Excel nebo vložené tabulky </i>zadejte název listu nebo tabulky, který chcete číst z.</ul><br/>|
Zprostředkovatel datových kanálů | Načte data z podporovaných informačních kanálů poskytovatele. Aktuálně podporované jenom formát Data (OData Open Protocol). | <b>Datový typ obsahu</b>: Určuje formát OData.<br/><br/><b>Adresa URL zdroje</b>: Určuje úplnou adresu URL datového kanálu. <br/>Například následující adresu URL přečte v ukázkové databázi Northwind: http://services.odata.org/northwind/northwind.svc/|


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C/
