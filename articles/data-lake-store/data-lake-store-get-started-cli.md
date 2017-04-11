<properties
   pageTitle="Začínáme s úložiště jezera dat pomocí rozhraní příkazového řádku různé platformy | Microsoft Azure"
   description="Umožňuje vytvořit účet úložiště jezera dat a provádění základních operací Azure různé platformy příkazového řádku"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-azure-command-line"></a>Začínáme s Azure úložišti jezera Azure příkazového řádku

> [AZURE.SELECTOR]
- [Portál](data-lake-store-get-started-portal.md)
- [Prostředí PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [ROZHRANÍ REST API](data-lake-store-get-started-rest-api.md)
- [Azure rozhraní příkazového řádku](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Naučte se používat Azure rozhraní příkazového řádku a vytvořit účet Azure úložišti jezera provádění základních operací, například při vytvoření složek, nahrávání a stahování datové soubory a odstraňovat klienta, atd. Další informace o úložiště jezera dat najdete v článku [Přehled úložiště jezera dat](data-lake-store-overview.md).

Rozhraní příkazového řádku Azure je součástí Node.js. Lze jej využít platformy, který podporuje Node.js, včetně systému Windows, Mac a Linux. Rozhraní příkazového řádku Azure je otevřít zdroj. Zdrojový kód je spravovaný v GitHub na <a href= "https://github.com/azure/azure-xplat-cli">https://github.com/azure/azure-xplat-cli</a>. Tento článek se zabývá pouze pomocí úložiště jezera dat Azure rozhraní příkazového řádku. Obecné průvodce o používání rozhraní příkazového řádku Azure najdete v tématu [použití Azure rozhraní příkazového řádku] [azure-command-line-tools].


##<a name="prerequisites"></a>Zjistit předpoklady pro

Než začnete v tomto článku, musíte mít takto:

- **Azure předplatného**. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).

- **Rozhraní příkazového řádku azure** – přečtěte si téma [instalace a konfigurace rozhraní příkazového řádku Azure](../xplat-cli-install.md) informace o instalaci a konfiguraci. Zkontrolujte, že restartujete počítač po instalaci rozhraní příkazového řádku.

## <a name="authentication"></a>Ověřování

Tento článek používá jednodušší ověřování s úložiště jezera dat místo, kam jste přihlášení jako uživatel koncových uživatelů. Úroveň přístupu k účtu a systém souborů se pak řídí úroveň přístupu přihlášený uživatel úložiště jezera dat. Můžou ale nastat jiných přístupů i k ověření dat jezera Store, které jsou **ověření koncových uživatelů** nebo **služeb**. Pokyny a další informace o tom, jak ověřit najdete v tématu [Ověřit s úložiště jezera dat pomocí služby Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

##<a name="login-to-your-azure-subscription"></a>Přihlášení k předplatnému Azure

1. Postupujte podle kroků uvedených v části [připojit ke předplatné Azure z Azure rozhraní příkazového řádku (Azure rozhraní příkazového řádku)](../xplat-cli-connect.md) a připojovat se k odběru pomocí `azure login` metody.

2. Seznam předplatné, které jsou přidružené k účtu pomocí `azure account list` příkaz.

        info:    Executing command account list
        data:    Name              Id                                    Current
        data:    ----------------  ------------------------------------  -------
        data:    Azure-sub-1       ####################################  true
        data:    Azure-sub-2       ####################################  false

    Z výše uvedených výstupu je v současné době povolenou **Azure-sub-1** a jiné předplatné je **Azure-sub-2**. 

3. Vyberte předplatné, které chcete pracovat v části. Pokud chcete pracovat v rámci předplatného Azure-sub-2, použít `azure account set` příkaz.

        azure account set Azure-sub-2


## <a name="create-an-azure-data-lake-store-account"></a>Vytvoření účtu úložiště jezera dat Azure

Otevřete příkazový řádek, kostra domu nebo terminálové relace a následující příkazy.

2. Přepněte do režimu správce prostředků Azure pomocí následujícího příkazu:

        azure config mode arm


5. Vytvoření nové skupiny prostředků. V následující příkaz zadejte hodnoty parametrů, které chcete použít.

        azure group create <resourceGroup> <location>

    Pokud název umístění obsahuje mezery, umístěte ji v uvozovkách. Například "východoasijských US 2".

5. Vytvoření účtu jezera úložiště.

        azure datalake store account create <dataLakeStoreAccountName> <location> <resourceGroup>

## <a name="create-folders-in-your-data-lake-store"></a>Vytvoření složky ve vašem úložišti jezera dat

V části účtu úložiště jezera dat Azure spravovat a uložení dat můžete vytvořit složky. Pomocí následujícího příkazu vytvořte složku s názvem "mynewfolder" na kořenové úrovni obchodu jezera Data.

    azure datalake store filesystem create <dataLakeStoreAccountName> <path> --folder

Příklad:

    azure datalake store filesystem create mynewdatalakestore /mynewfolder --folder

## <a name="upload-data-to-your-data-lake-store"></a>Odeslání dat do úložiště jezera dat.

Data můžete nahrávat úložiště jezera dat přímo na kořenové úrovni nebo do složky, kterou jste vytvořili v rámci účtu. Fragmenty níže ukazují, jak nahrát ukázkových dat pro složku (**mynewfolder**) jste vytvořili v předchozí části.

Pokud hledáte ukázkových dat chcete odeslat, můžete získat složce **Ambulance dat** z [Azure dat jezera libovolná úložiště](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData). Budou moct soubor stáhnout a uložte ho do místního adresáře ve vašem počítači, například C:\sampledata\.

    azure datalake store filesystem import <dataLakeStoreAccountName> "<source path>" "<destination path>"

Příklad:

    azure datalake store filesystem import mynewdatalakestore "C:\SampleData\AmbulanceData\vehicle1_09142014.csv" "/mynewfolder/vehicle1_09142014.csv"


## <a name="list-files-in-data-lake-store"></a>Seznam souborů v úložišti jezera dat

Pomocí následujícího příkazu seznam souborů v úložišti jezera účtu.

    azure datalake store filesystem list <dataLakeStoreAccountName> <path>

Příklad:

    azure datalake store filesystem list mynewdatalakestore /mynewfolder

Výstup tohoto by měl být stejný následujícím způsobem:

    info:    Executing command datalake store filesystem list
    data:    accessTime: 1446245025257
    data:    blockSize: 268435456
    data:    group: NotSupportYet
    data:    length: 1589881
    data:    modificationTime: 1446245105763
    data:    owner: NotSupportYet
    data:    pathSuffix: vehicle1_09142014.csv
    data:    permission: 777
    data:    replication: 0
    data:    type: FILE
    data:    ------------------------------------------------------------------------------------
    info:    datalake store filesystem list command OK

## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a>Přejmenovat, stažení a odstranění dat z úložiště jezera dat.

* **Přejmenování souboru**, použijte tento příkaz:

        azure datalake store filesystem move <dataLakeStoreAccountName> <path/old_file_name> <path/new_file_name>

    Příklad:

        azure datalake store filesystem move mynewdatalakestore /mynewfolder/vehicle1_09142014.csv /mynewfolder/vehicle1_09142014_copy.csv

* **Pokud si chcete stáhnout soubor**, použijte následující příkaz. Zkontrolujte, jestli cíl cestu, kterou zadáte už existuje.

        azure datalake store filesystem export <dataLakeStoreAccountName> <source_path> <destination_path>

    Příklad:

        azure datalake store filesystem export mynewdatalakestore /mynewfolder/vehicle1_09142014_copy.csv "C:\mysampledata\vehicle1_09142014_copy.csv"

* **Při odstraňování souboru**, použijte tento příkaz:

        azure datalake store filesystem delete <dataLakeStoreAccountName> <path>

    Příklad:

        azure datalake store filesystem delete mynewdatalakestore /mynewfolder/vehicle1_09142014_copy.csv

    Po zobrazení výzvy zadejte **Y** odstranit položku.

## <a name="view-the-access-control-list-for-a-folder-in-data-lake-store"></a>Zobrazení seznamu řízení přístupu pro složky v úložišti jezera dat

Pomocí následujícího příkazu zobrazíte ACL pro složku jezera úložiště. V současné verzi ACL nastavit jenom na kořenové úložiště jezera. Ano parametru path bude vždy kořenové (/).

    azure datalake store permissions show <dataLakeStoreName> <path>

Příklad:

    azure datalake store permissions show mynewdatalakestore /


## <a name="delete-your-data-lake-store-account"></a>Odstranění účtu úložiště jezera dat

Pomocí následujícího příkazu odstraněním účtu jezera úložiště.

    azure datalake store account delete <dataLakeStoreAccountName>

Příklad:

    azure datalake store account delete mynewdatalakestore

Po zobrazení výzvy zadejte **Y** odstranit účet.


## <a name="next-steps"></a>Další kroky

- [Zabezpečení dat v úložišti jezera dat](data-lake-store-secure-data.md)
- [Použití analýzy jezera Azure dat s jezera úložiště dat](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Pomocí úložiště jezera dat Azure HDInsight](data-lake-store-hdinsight-hadoop-use-portal.md)


[azure-command-line-tools]: ../xplat-cli-install.md
