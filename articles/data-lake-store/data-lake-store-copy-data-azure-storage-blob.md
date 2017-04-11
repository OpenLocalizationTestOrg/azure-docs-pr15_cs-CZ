<properties
   pageTitle="Kopírování dat z Azure úložiště objektů BLOB do úložiště jezera dat | Microsoft Azure"
   description="Nástroj pro správu AdlCopy ke kopírování dat z Azure úložiště objektů BLOB jezera úložiště dat"
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
   ms.date="10/05/2016"
   ms.author="nitinme"/>

# <a name="copy-data-from-azure-storage-blobs-to-data-lake-store"></a>Kopírování dat z Azure úložiště objektů BLOB jezera úložiště dat

> [AZURE.SELECTOR]
- [Použití DistCp](data-lake-store-copy-data-wasb-distcp.md)
- [Použití AdlCopy](data-lake-store-copy-data-azure-storage-blob.md)

Úložiště jezera dat Azure poskytuje nástroj příkazového řádku, [AdlCopy](http://aka.ms/downloadadlcopy)ke kopírování dat z následujících zdrojů:

* Z Azure úložiště objektů BLOB do úložiště jezera Data. AdlCopy nelze použít ke kopírování dat z jezera úložišti objektů BLOB Azure úložiště.

* Mezi dvěma úložiště jezera dat Azure účty. 

Taky můžete použít nástroj AdlCopy dvěma různými způsoby:

* **Samostatné**, které používá nástroj pro zdroje dat jezera úložiště k provedení úkolu.

* **Pomocí účtu jezera analýzy dat**, použití jednotky přiřazené k vašemu účtu jezera analýzy dat pro operaci kopírovat. Můžete chtít tuto možnost použijte, když hledáte můžou provádět kopírovat předvídatelná způsobem.

##<a name="prerequisites"></a>Zjistit předpoklady pro

Než začnete v tomto článku, musíte mít takto:

- **Azure předplatného**. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).

- Kontejner **Azure úložiště objektů BLOB** s daty.

- **Účet azure dat jezera analýzy (volitelné)** – najdete v článku [Začínáme s jezera analýzy dat Azure](../data-lake-analytics/data-lake-analytics-get-started-portal.md) Další informace o tom, jak vytvořit účet jezera úložiště.

- **Nástroj AdlCopy**. Nainstalujte nástroj AdlCopy z [http://aka.ms/downloadadlcopy](http://aka.ms/downloadadlcopy).

## <a name="syntax-of-the-adlcopy-tool"></a>Syntaxe AdlCopy nástroje

Pomocí následující syntaxe pro práci s nástroji AdlCopy

    AdlCopy /Source <Blob or Data Lake Store source> /Dest <Data Lake Store destination> /SourceKey <Key for Blob account> /Account <Data Lake Analytics account> /Unit <Number of Analytics units> /Pattern 

Parametry v syntaxi jsou uvedeny níže:

| Možnost    | Popis                                                                                                                                                                                                                                                                                                                                                                                                          |
|-----------|------------|
| Zdroje    | Určuje umístění zdroje dat v Azure úložiště objektů blob. Zdroje můžou být kontejneru objektů blob, objektů blob nebo jiný účet jezera úložiště.                                                                                                                                                                                                                                                                                                 |
| Cíl      | Určuje cílového úložiště jezera dat zkopírovat.                                                                                                                                                                                                                                                                                                                                                                |
| SourceKey | Určuje přístupová klávesa úložiště pro tento zdroj objektů blob Azure úložiště. To je nutné pouze Jestliže je zdrojem objektů blob kontejneru nebo objektu blob.                                                                                                                                                                                                                                                                                                                                                 |
| Účet   | **Volitelné**. Použijte, pokud chcete použít účet Azure dat jezera Analytics pro spuštění úlohy kopírovat. Pokud použijete možnost /Account v syntaxi ale nezadáte účet jezera analýzy dat, používá AdlCopy výchozí účet pro spuštění úlohy. Také pokud tuto možnost použít, musíte přidat zdroj (úložiště objektů Blob Azure) a cíl (úložiště Azure dat jezera) jako zdroje dat pro váš účet jezera analýzy dat.  |
| Jednotky     |  Určuje počet jednotek jezera analýzy dat, které bude sloužit k Kopírovat projekt. Tato možnost je povinný, pokud použijete možnost **/Account** určete analýzy dat jezera účet.
| Vzor   | Určuje regex vzorec, který označuje které objekty BLOB nebo souborů, které chcete kopírovat. AdlCopy používá odpovídající malá a velká písmena. Výchozí vzorek vyvolají není zadán žádný vzorku je zkopírujte všechny položky. Zadání více vzorů soubor není podporovaná.                                                                                                                                                                                                                                                                                                                                               



## <a name="use-adlcopy-as-standalone-to-copy-data-from-an-azure-storage-blob"></a>Umožňuje AdlCopy (jako samostatný) zkopírujte data z objektů blob Azure úložiště

1. Otevřete příkazový řádek a přejděte do adresáře AdlCopy nainstalovanou, obvykle `%HOMEPATH%\Documents\adlcopy`.

2. Spusťte tento příkaz Kopírovat konkrétní objektů blob kontejneru zdroj k úložišti jezera dat:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    Příklad:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

    
    >[AZURE.NOTE] Syntaxe výše Určuje soubor, který budou zkopírovány do složky v úložišti jezera účtu. Nástroj AdlCopy vytvoří složku, pokud zadaný název složky neexistuje.

    Zobrazí se výzva k zadání přihlašovací údaje pro Azure předplatné, pod kterým máte svůj účet jezera úložiště. Zobrazí se výstup podobná této:

        Initializing Copy.
        Copy Started.
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

3. K tomuto účtu úložiště jezera dat pomocí následujícího příkazu můžete taky zkopírovat všechny objekty BLOB z jednoho kontejner:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/ /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>        

    Příklad:

        AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

## <a name="use-adlcopy-as-standalone-to-copy-data-from-another-data-lake-store-account"></a>Kopírování dat z jiného účtu úložiště jezera dat pomocí AdlCopy (jako samostatný)

Můžete taky AdlCopy přesouvat data mezi dva účty jezera úložiště.

1. Otevřete příkazový řádek a přejděte do adresáře AdlCopy nainstalovanou, obvykle `%HOMEPATH%\Documents\adlcopy`.

2. Spusťte tento příkaz zkopírovat konkrétním souborem z jednoho účtu úložiště jezera dat do druhého.

        AdlCopy /Source adl://<source_adls_account>.azuredatalakestore.net/<path_to_file> /dest adl://<dest_adls_account>.azuredatalakestore.net/<path>/

    Příklad:

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/909f2b.log /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

    >[AZURE.NOTE] Syntaxe výše Určuje soubor, který budou zkopírovány do složky do cílového úložiště jezera dat účtu. Nástroj AdlCopy vytvoří složku, pokud zadaný název složky neexistuje.

    Zobrazí se výzva k zadání přihlašovací údaje pro Azure předplatné, pod kterým máte svůj účet jezera úložiště. Zobrazí se výstup podobná této:

        Initializing Copy.
        Copy Started.|
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

3. Následující příkaz slouží ke kopírování všechny soubory z určité složky ve zdroji dat jezera úložiště účtu do složky v cílového úložiště jezera dat účtu.

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/ /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

## <a name="use-adlcopy-with-data-lake-analytics-account-to-copy-data"></a>Pomocí AdlCopy (účet analýzy dat jezera) ke kopírování dat

Můžete také účtu jezera analýzy dat pro spuštění úlohy AdlCopy ke kopírování dat z Azure úložiště objektů BLOB jezera úložiště. Obvykle použijete tuto možnost, pokud data, která chcete přesunout je v oblasti gigabajty a TB a chcete, aby výkon lepší a předvídatelná výkon.

Zkopírujte z úložiště objektů Blob Azure pomocí účtu jezera analýzy dat v AdlCopy, musí být přidán zdroj (úložiště objektů Blob Azure) jako zdroje dat pro váš účet analýzy dat jezera. Postup pro přidání dalších zdrojů dat pořád ke svému účtu jezera analýzy dat najdete v článku [Správa analýzy dat jezera účet zdroje dat](..//data-lake-analytics/data-lake-analytics-manage-use-portal.md#manage-account-data-sources).

>[AZURE.NOTE] Pokud kopírujete účet Azure úložišti jezera jako zdroj pomocí účtu jezera analýzy dat, nepotřebujete přidružíte účtu úložiště jezera dat pomocí analýzy dat jezera účtu. Požadavek na přidružit zdroj úložiště účtu jezera analýzy dat je pouze zdroji účet Azure úložiště.

Spusťte tento příkaz Kopírovat z objektů blob Azure úložiště k účtu úložiště jezera dat pomocí analýzy dat jezera účtu:

    AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Account <data_lake_analytics_account> /Unit <number_of_data_lake_analytics_units_to_be_used>

Příklad:

    AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Account mydatalakeanalyticaccount /Units 2


Podobně spusťte tento příkaz Kopírovat z objektů blob Azure úložiště k účtu úložiště jezera dat pomocí analýzy dat jezera účtu:

    AdlCopy /Source adl://mysourcedatalakestore.azuredatalakestore.net/mynewfolder/ /dest adl://mydestdatastore.azuredatalakestore.net/mynewfolder/ /Account mydatalakeanalyticaccount /Units 2

## <a name="use-adlcopy-to-copy-data-using-pattern-matching"></a>Slouží ke kopírování dat pomocí vzorů AdlCopy

V tomto oddílu informace o použití AdlCopy ke kopírování dat ze zdroje (v našem následujícím příkladu budeme používat Azure úložiště objektů Blob) k účtu cílového úložiště jezera dat pomocí vzorů. Například můžete kroků zkopírujte všechny soubory s příponou souboru .csv z objektů blob zdroj do cíle.

1. Otevřete příkazový řádek a přejděte do adresáře AdlCopy nainstalovanou, obvykle `%HOMEPATH%\Documents\adlcopy`.

2. Spusťte tento příkaz zkopírujte všechny soubory s příponou *.csv z konkrétní objektů blob z kontejneru zdroj k úložišti jezera dat:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Pattern *.csv

    Příklad:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/FoodInspectionData/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Pattern *.csv

    
## <a name="billing"></a>Fakturace

* Pokud používáte nástroj AdlCopy jako samostatný se bude vám nebudou účtovat poplatky výstupní nákladů přesunutí dat, jestliže je zdrojem účet Azure úložiště nejsou ve stejné oblasti jako jezera úložišti.

* Pokud používáte nástroj AdlCopy s účtem jezera analýzy dat, se použije standardní [analýzy dat jezera fakturační sazby](https://azure.microsoft.com/pricing/details/data-lake-analytics/) .

## <a name="considerations-for-using-adlcopy"></a>Důležité informace týkající se používání AdlCopy

* AdlCopy (pro verzi 1.0.5) podporuje kopírování dat z zdrojů, jejichž souhrnně více než tisíce souborů a složek. Ale pokud narazíte na problémy kopírování velké datovou sadu, můžete distribuce souborů a složek do různých podsložek a použijte cestu k těmto podsložky jako zdroj.

## <a name="next-steps"></a>Další kroky

- [Zabezpečení dat v úložišti jezera dat](data-lake-store-secure-data.md)
- [Použití analýzy jezera Azure dat s jezera úložiště dat](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Pomocí úložiště jezera dat Azure HDInsight](data-lake-store-hdinsight-hadoop-use-portal.md)
