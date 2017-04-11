<properties 
   pageTitle="Začínáme s Azure dat jezera technologie pro analýzu pomocí rozhraní příkazového řádku Azure | Microsoft Azure" 
   description="Naučte se používat rozhraní Azure příkazového řádku pro vytvoření účtu úložiště jezera dat, vytvořit analýzy dat jezera úlohy pomocí U SQL a odesílat úkoly. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-azure-command-line-interface-cli"></a>Kurz: Začínáme s Azure dat jezera technologie pro analýzu pomocí rozhraní příkazového (rozhraní příkazového řádku Azure řádku)

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]


Naučte se používat rozhraní příkazového řádku Azure k vytvoření účtů jezera analýzy dat Azure, definujte úlohy jezera analýzy dat v [U SQL](data-lake-analytics-u-sql-get-started.md)a odesílat úlohy do analýzy dat jezera účty. Další informace o jezera analýzy dat najdete v tématu [Přehled analýzy jezera dat Azure](data-lake-analytics-overview.md).

V tomto kurzu se dají projektu, který bude číst na kartě soubor hodnot (TSV) oddělené a převede ho do souboru hodnoty oddělené čárkami (CSV). Projít stejné kurz používání jiných podporovaných nástrojů, klikněte na karty v horní v této části.

##<a name="prerequisites"></a>Zjistit předpoklady pro

Před zahájením tohoto kurzu, musíte mít takto:

- **Azure předplatného**. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).
- **Azure rozhraní příkazového řádku**. V tématu [instalace a konfigurace rozhraní příkazového řádku Azure](../xplat-cli-install.md).
    - Stáhnout a nainstalovat **předběžnou verzi** [Azure rozhraní příkazového řádku nástroje](https://github.com/MicrosoftBigData/AzureDataLake/releases) pro dokončení Tato ukázka.
- **Ověření**pomocí následujícího příkazu:

        azure login
    Další informace o ověřování pomocí pracovního nebo školního účtu najdete v článku [připojení k předplatnému Azure z Azure rozhraní příkazového řádku](../xplat-cli-connect.md).
- **Přepnutí do režimu správce prostředků Azure**pomocí následujícího příkazu:

        azure config mode arm
        
## <a name="create-data-lake-analytics-account"></a>Vytvoření účtu jezera analýzy dat

Před spuštěním všechny úlohy musíte mít účet analýzy dat jezera. Vytvoření účtu jezera analýzy dat, je nutné zadat takto:

- **Pole Skupina zdroje Azure**: je třeba vytvořit účet jezera analýzy dat A ve skupině Azure prostředků. [Správce prostředků Azure](../azure-resource-manager/resource-group-overview.md) umožňuje práce se zdroji v aplikaci jako skupinu. Můžete nasadit, aktualizace nebo odstranění všechny zdroje pro aplikaci v operaci jediné, koordinovaný.  

    Vytvořit výčet skupiny zdrojů ve vašem předplatném:
    
        azure group list 
    
    Vytvoření nové skupiny prostředků:

        azure group create -n "<Resource Group Name>" -l "<Azure Location>"

- **Název účtu technologie pro analýzu dat jezera**
- **Umístění**: jeden z Azure datacentrech, které podporuje jezera analýzy dat.
- **Výchozí Data jezera účtu**: každý účet analýzy dat jezera má výchozí účet jezera Data.

    Chcete-li zobrazit existujícího účtu jezera dat:
    
        azure datalake store account list

    Vytvoření nového účtu jezera dat:

        azure datalake store account create "<Data Lake Store Account Name>" "<Azure Location>" "<Resource Group Name>"

    > [AZURE.NOTE] Název účtu dat jezera musí obsahovat pouze malá písmena a číslice.



**Vytvoření účtu jezera analýzy dat**

        azure datalake analytics account create "<Data Lake Analytics Account Name>" "<Azure Location>" "<Resource Group Name>" "<Default Data Lake Account Name>"

        azure datalake analytics account list
        azure datalake analytics account show "<Data Lake Analytics Account Name>"          

![Zobrazení analýzy dat jezera účtu](./media/data-lake-analytics-get-started-cli/data-lake-analytics-show-account-cli.png)

> [AZURE.NOTE] Název účtu analýzy dat jezera musí obsahovat pouze malá písmena a číslice.


## <a name="upload-data-to-data-lake-store"></a>Odeslání dat do úložiště jezera dat

V tomto kurzu zpracuje některé protokoly vyhledávání.  Protokol hledání můžete uložené v úložišti jezera dat nebo úložiště objektů Blob Azure. 

Portál Azure poskytuje uživatelské rozhraní pro zkopírování některé ukázkové datové soubory do výchozího účtu jezera dat, který zahrnout soubor protokolu vyhledávání. V tématu [Příprava zdroje dat](data-lake-analytics-get-started-portal.md#prepare-source-data) odešlete data do výchozího úložiště jezera dat účtu.

Pokud chcete nahrát soubory pomocí rozhraní příkazového řádku, použijte tento příkaz:

    azure datalake store filesystem import "<Data Lake Store Account Name>" "<Path>" "<Destination>"
    azure datalake store filesystem list "<Data Lake Store Account Name>" "<Path>"

Analýzy dat jezera taky přístup k úložišti objektů Blob Azure.  Odeslání dat k úložišti objektů Blob Azure, najdete v článku [použití Azure rozhraní příkazového řádku s úložištěm Azure](../storage/storage-azure-cli.md).

## <a name="submit-data-lake-analytics-jobs"></a>Odeslat úlohy jezera analýzy dat

Úlohy jezera analýzy dat jsou napsané v jazyce U SQL. Další informace o U SQL, najdete v článku [Začínáme s jazyka U SQL](data-lake-analytics-u-sql-get-started.md) a funkce pro [odkazy jazyka U SQL](http://go.microsoft.com/fwlink/?LinkId=691348).

**Vytvořit skript úlohy jezera analýzy dat**

- Vytvoření textového souboru s následující skript U SQL a uložte textový soubor na počítači:

        @searchlog =
            EXTRACT UserId          int,
                    Start           DateTime,
                    Region          string,
                    Query           string,
                    Duration        int?,
                    Urls            string,
                    ClickedUrls     string
            FROM "/Samples/Data/SearchLog.tsv"
            USING Extractors.Tsv();
        
        OUTPUT @searchlog   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

    Tento skript U SQL přečte souboru zdroje dat pomocí **Extractors.Tsv()**a vytvoří soubor csv pomocí **Outputters.Csv()**. 
    
    Neupravujte dvě cesty, pokud zkopírujete zdrojového souboru do jiného umístění.  Jezera analýzy dat vytvoří složku výstupu, pokud ho neexistuje.
    
    Je jednodušší použít relativní cesty pro soubory uložené ve výchozí data jezera účty. Můžete taky použít absolutní cesty.  Příklad 
    
        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
        
    Je nutné použít absolutní cesty pro přístup k souborům v propojené účty úložiště.  Syntaxe pro soubory uložené v propojený klient Azure úložiště je:
    
        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Azure kontejneru objektů Blob s objekty BLOB veřejné nebo veřejné kontejnery přístupová oprávnění aktuálně nepodporuje.      

    
**Odeslání projektu**


    azure datalake analytics job create  "<Data Lake Analytics Account Name>" "<Job Name>" "<Script>"
    
    
Seznam úlohy, získat podrobnosti projektu a zrušit úlohy můžete použít následující příkazy:

    azure datalake analytics job cancel "<Data Lake Analytics Account Name>" "<Job Id>"
    azure datalake analytics job list "<Data Lake Analytics Account Name>"
    azure datalake analytics job show "<Data Lake Analytics Account Name>" "<Job Id>"

Po dokončení projektu, můžete pomocí těchto rutin zobrazíte seznam souboru a budou moct soubor stáhnout:
    
    azure datalake store filesystem list "<Data Lake Store Account Name>" "/Output"
    azure datalake store filesystem export "<Data Lake Store Account Name>" "/Output/SearchLog-from-Data-Lake.csv" "<Destination>"
    azure datalake store filesystem read "<Data Lake Store Account Name>" "/Output/SearchLog-from-Data-Lake.csv" <Length> <Offset>

## <a name="see-also"></a>Viz taky

- Stejný kurz pomocí dalších nástrojů zobrazíte kliknutím na kartu voliče v horní části stránky.
- Složitější dotaz najdete v tématu [protokoly analyzovat webu pomocí analýzy jezera dat Azure](data-lake-analytics-analyze-weblogs.md).
- Začínáme s vývojem aplikací U SQL, najdete v článku [pomocí nástroje jezera datového Visual Studio skripty vyvíjet U-SQL](data-lake-analytics-data-lake-tools-get-started.md).
- Další U SQL najdete v tématu [Začínáme s jazykem dat Azure jezera analýzy U-SQL](data-lake-analytics-u-sql-get-started.md).
- Úlohy správy najdete v článku [Správa Azure dat jezera analýzy pomocí portálu Azure](data-lake-analytics-manage-use-portal.md).
- Získejte přehled o jezera analýzy dat, najdete v článku [Přehled analýzy jezera dat Azure](data-lake-analytics-overview.md).

