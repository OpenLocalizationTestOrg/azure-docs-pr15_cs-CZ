<properties
    pageTitle="Přesuňte Data z úložiště objektů Blob Azure použití konektorů SSIS nebo | Microsoft Azure"
    description="Přesuňte Data z úložiště objektů Blob Azure použití konektorů SSIS nebo."
    services="machine-learning,storage"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="bradsev" />

# <a name="move-data-to-or-from-azure-blob-storage-using-ssis-connectors"></a>Přesunutí dat do nebo z úložiště objektů Blob Azure použití konektorů SSIS

[SQL Server Integration Services Feature Pack pro Azure](https://msdn.microsoft.com/library/mt146770.aspx) poskytuje součásti pro připojení k Azure, přenášet data mezi Azure a místních zdrojů dat a proces data uložená v Azure.

Pokyny pro technologií pro server slouží k přesunutí dat do a/nebo z úložiště objektů Blob Azure jsou propojené tady:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]


Jakmile zákazníci byly přesunuté místních dat do cloudu, že k němu přístup z Azure služby můžete využít úplný power sadu Azure technologií. Ji může používat, například v Azure počítače výukové nebo clusteru HDInsight.

To bude obvykle cílem prvního kroku návody [SQL](machine-learning-data-science-process-sql-walkthrough.md) a [HDInsight](machine-learning-data-science-process-hive-walkthrough.md) .

Diskusní kanonický scénáře, které umožňuje provádět obchodní potřeby běžné scénáře integrace dat hybridní SSIS najdete v článku [více s SQL Server Integration Services Feature Pack pro Azure](http://blogs.msdn.com/b/ssis/archive/2015/06/25/doing-more-with-sql-server-integration-services-feature-pack-for-azure.aspx) blogu.

> [AZURE.NOTE] Pro dokončení Úvod k úložišti objektů blob Azure odkažte na [Základy objektů Blob Azure](../storage/storage-dotnet-how-to-use-blobs.md) a službě [objektů Blob Azure](https://msdn.microsoft.com/library/azure/dd179376.aspx).

## <a name="prerequisites"></a>Zjistit předpoklady pro

K provádění úloh popisované v tomto článku, musíte mít předplatné Azure a nastavený Azure úložiště účet. Musíte znát svůj Azure úložiště název a účet klíč účtu nahrávání nebo stahování data.

- Nastavení **Azure předplatné**, najdete v článku [bezplatnou zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

- Pokyny pro vytvoření **účtu úložiště** a usnadňující účet a klíčové informace podívejte se na [účty adresářové služby Azure o úložiště](../storage/storage-create-storage-account.md).


Pokud chcete použít **SSIS spojnic**, je třeba stáhnout:

- **SQL Server 2014 nebo 2016 standardní (nebo novější)**: instalace obsahuje služby SQL Server Integration Services.
- **Microsoft SQL Server 2014 nebo 2016 Integration Services Feature Pack pro Azure**: tyto soubory můžete stáhnout, respektive ze stránky [Služby SQL Server 2014 Integration Services](http://www.microsoft.com/download/details.aspx?id=47366) a [Služby SQL Server 2016 Integration Services](https://www.microsoft.com/download/details.aspx?id=49492) .

> [AZURE.NOTE] SSIS je nainstalovaný se serverem SQL Server, ale není součástí Express verze. Další informace o jaké aplikace jsou součástí různých edicemi systému SQL Server najdete v článku [Edice serveru SQL](http://www.microsoft.com/en-us/server-cloud/products/sql-server-editions/)

Výukové materiály na SSIS najdete v článku [Rukou na školicí kurzy pro SSIS](http://www.microsoft.com/download/details.aspx?id=20766)

Informace o tom, jak získat nahoru a spuštění pomocí můžete vytvořit jednoduchý extrahování, transformace a balíčků načtení (ETL), přečtěte si téma SISS [SSIS kurz: vytvoření jednoduchého balíčku ETL](https://msdn.microsoft.com/library/ms169917.aspx).

## <a name="download-nyc-taxi-dataset"></a>Stáhnout sadu NYC Taxi  
Příklad popsané tady pomocí veřejně dostupný datovou sadu – datovou sadu [NYC Taxi cest](http://www.andresmh.com/nyctaxitrips/) . Datové se skládá z asi 173 milionů taxi jízdy autem do práce v NYC za rok 2013. Existují dva typy dat: cesta podrobností dat a tarif data. Je soubor pro každý měsíc máme 24 soubory ve všech, přičemž každý z nich je 2GB nekomprimované.


## <a name="upload-data-to-azure-blob-storage"></a>Odeslání dat do úložišti objektů blob Azure
Přesune dat pomocí SSIS feature pack z místního k úložišti objektů blob Azure, používáme instance [**Úkolu odeslat objektů Blob Azure**](https://msdn.microsoft.com/library/mt146776.aspx), ukazuje tento obrázek:

![Konfigurace dat – vědy OM](./media/machine-learning-data-science-move-data-to-azure-blob-using-ssis/ssis-azure-blob-upload-task.png)


Tady jsou popsané parametrů, které používá úkolu:


Pole|Popis|
----------------------|----------------|
**AzureStorageConnection**|Určuje existující Connection Manager úložiště Azure nebo vytvoří novou odkazující odkazující na hostitelem soubory objektů blob Azure úložiště účet.|
**BlobContainer**|Určuje název kontejneru objektů blob, stiskněte a podržte nahrané soubory jako objekty BLOB.|
**BlobDirectory**|Adresář objektů blob uložení uloženého souboru jako objektů blob blokovat. Adresář objektů blob je virtuální hierarchickou strukturu. Pokud objektů blob už existuje, nahradí i it.|
**LocalDirectory**|Určuje místní adresář, který obsahuje soubory, které chcete nahrát.|
**Název souboru**|Určuje název filtru a vyberte soubory se zadaným názvem vzorku. Například MySheet\*.xls\* obsahuje soubory, jako jsou MySheet001.xls a MySheetABC.xlsx|
**TimeRangeFrom/TimeRangeTo**|Určuje časový rozsah filtr. Soubory změnit po *TimeRangeFrom* a před *TimeRangeTo* jsou však započítávány.|


> [AZURE.NOTE] Musí být správné přihlašovací údaje **AzureStorageConnection** a **BlobContainer** musí existovat před pokusem o přenosu.

## <a name="download-data-from-azure-blob-storage"></a>Stažení dat z úložiště objektů blob Azure

Stažení dat z úložišti objektů blob Azure k místnímu úložiště s SSIS, použijte instanci [Úkolu odeslat objektů Blob Azure](https://msdn.microsoft.com/library/mt146779.aspx).

##<a name="more-advanced-ssis-azure-scenarios"></a>Další rozšířené scénáře SSIS Azure
Tady jsme nezapomeňte, že balíček SSIS funkcí umožňuje složitější toků uskutečněných jednotlivými balení úkoly pohromadě. Například data objektů blob může kanálu sociální sítě přímo do HDInsight clusteru, jejichž výstup mohlo být stažení zpátky objektů blob a klikněte na místní úložiště. SSIS je možné spouštět podregistru a Prasátko úlohy na clusteru služby HDInsight pomocí dalších SSIS konektorů:

- Spustit skript podregistru clusteru Azure Hdinsightu pomocí SSIS, použijte [Azure HDInsight podregistru úkolu](https://msdn.microsoft.com/library/mt146771.aspx).
- Spustit skript Prasátko clusteru Azure Hdinsightu pomocí SSIS, použijte [Azure HDInsight Prasátko úkolu](https://msdn.microsoft.com/library/mt146781.aspx).
