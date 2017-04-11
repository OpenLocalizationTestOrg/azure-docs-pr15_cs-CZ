<properties
   pageTitle="Začínáme s úložiště jezera dat | Azure"
   description="Vytvoření účtu úložiště jezera dat a provádění základních operací pomocí prostředí PowerShell Azure"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/04/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-azure-powershell"></a>Začínáme s úložiště jezera dat Azure pomocí prostředí PowerShell Azure

> [AZURE.SELECTOR]
- [Portál](data-lake-store-get-started-portal.md)
- [Prostředí PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [ROZHRANÍ REST API](data-lake-store-get-started-rest-api.md)
- [Azure rozhraní příkazového řádku](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Zjistěte, jak pomocí Powershellu Azure vytvořte účet Azure úložišti jezera a provádění základních operací, například při vytvoření složek, nahrávání a stahování souborů dat, odstraňte klienta, atd. Další informace o úložiště jezera dat najdete v článku [Přehled úložiště jezera dat](data-lake-store-overview.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Před zahájením tohoto kurzu, musíte mít takto:

* **Azure předplatného**. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).

* **Azure PowerShell 1.0 nebo vyšší**. Přečtěte si, [Jak nainstalovat a nakonfigurovat Azure Powershellu](../powershell-install-configure.md).

## <a name="authentication"></a>Ověřování

Tento článek používá jednodušší ověřování s úložiště jezera dat místo, kam se zobrazí výzva k zadání přihlašovacích údajů účet Azure. Úroveň přístupu k účtu a systém souborů se pak řídí úroveň přístupu přihlášený uživatel úložiště jezera dat. Můžou ale nastat jiných přístupů i k ověření dat jezera Store, které jsou **ověření koncových uživatelů** nebo **služeb**. Pokyny a další informace o tom, jak ověřit najdete v tématu [Ověřit s úložiště jezera dat pomocí služby Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="create-an-azure-data-lake-store-account"></a>Vytvoření účtu úložiště jezera dat Azure

1. V Excelu otevření nového okna prostředí Windows PowerShell a zadejte následující úryvek přihlásit ke svému účtu Azure, nastavte předplatné a zaregistrovat poskytovatele jezera úložiště. Po zobrazení výzvy k přihlášení, zkontrolujte, jestli že se přihlaste se jako jednu z admininistrators/vlastník předplatného:

        # Log in to your Azure account
        Login-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Azure Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"


2. Účet Azure dat jezera úložiště je přidružené ke skupině Azure zdroje. Začněte tak, že vytvoříte skupina zdroje Azure.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    ![Vytvoření skupiny zdroje Azure] (./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Vytvoření skupiny zdroje Azure")

2. Vytvořte účet Azure dat jezera úložiště přihlašovacích údajů. Název, který zadáte musí obsahovat pouze malá písmena a číslice.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    ![Vytvořit účet úložiště jezera dat Azure] (./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Vytvořit účet úložiště jezera dat Azure")

3. Ověřte, že účet je úspěšně.

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    Výstup pro tuto by měl být **pravdivé**.

## <a name="create-directory-structures-in-your-azure-data-lake-store"></a>Vytvoření struktury adresářů ve vašem úložišti jezera dat Azure

Pod svým účtem úložiště jezera dat Azure spravovat a uložení dat můžete vytvořit adresáře.

1. Zadejte kořenový adresář.

        $myrootdir = "/"

2. Vytvořte nový adresář s názvem **mynewdirectory** zadaný kořenové složce.

        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/mynewdirectory

3. Ověřte, že nový adresář je úspěšně.

        Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -Path $myrootdir

    Měl by se zobrazit výstup takto:

    ![Ověření adresáře] (./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Ověření adresáře")


## <a name="upload-data-to-your-azure-data-lake-store"></a>Odeslání dat do vašeho jezera Azure úložiště

Data můžete nahrát úložiště jezera dat přímo na kořenové úrovni nebo na adresář, který jste vytvořili v rámci účtu. Fragmenty níže ukazují, jak nahrát ukázkových dat k adresáři (**mynewdirectory**) jste vytvořili v předchozí části.

Pokud hledáte ukázkových dat chcete odeslat, můžete získat složce **Ambulance dat** z [Azure dat jezera libovolná úložiště](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData). Budou moct soubor stáhnout a uložte ho do místního adresáře ve vašem počítači, například C:\sampledata\.

    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\sampledata\vehicle1_09142014.csv" -Destination $myrootdir\mynewdirectory\vehicle1_09142014.csv


## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a>Přejmenovat, stažení a odstranění dat z úložiště jezera dat.

Přejmenování souboru, použijte tento příkaz:

    Move-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014.csv -Destination $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Pokud si Pokud chcete stáhnout soubor, použijte tento příkaz:

    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv -Destination "C:\sampledata\vehicle1_09142014_Copy.csv"

Při odstraňování souboru, použijte tento příkaz:

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Po zobrazení výzvy zadejte **Y** odstranit položku. Pokud máte víc než jeden soubor odstranit, můžete zadat všechny cesty oddělené čárkou.

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014.csv, $myrootdir\mynewdirectoryvehicle1_09142014_Copy.csv

## <a name="delete-your-azure-data-lake-store-account"></a>Odstranění účtu úložiště jezera dat Azure

Odstranění účtu úložiště jezera dat pomocí následujícího příkazu.

    Remove-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

Po zobrazení výzvy zadejte **Y** odstranit účet.


## <a name="next-steps"></a>Další kroky

- [Zabezpečení dat v úložišti jezera dat](data-lake-store-secure-data.md)
- [Použití analýzy jezera Azure dat s jezera úložiště dat](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Pomocí úložiště jezera dat Azure HDInsight](data-lake-store-hdinsight-hadoop-use-portal.md)
