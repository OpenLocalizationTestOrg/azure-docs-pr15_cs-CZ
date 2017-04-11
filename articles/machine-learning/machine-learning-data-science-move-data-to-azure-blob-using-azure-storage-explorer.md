<properties 
    pageTitle="Přesunutí dat do a z úložiště objektů Blob Azure pomocí Průzkumníka úložišť Azure | Microsoft Azure" 
    description="Přesunutí dat do a z úložiště objektů Blob Azure pomocí Průzkumníka úložišť Azure" 
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
    ms.date="08/31/2016"
    ms.author="bradsev" />

# <a name="move-data-to-and-from-azure-blob-storage-using-azure-storage-explorer"></a>Přesunutí dat do a z úložiště objektů Blob Azure pomocí Průzkumníka úložišť Azure

Azure Explorer úložiště je libovolná nástroj od Microsoftu, který umožňuje pracovat s daty Azure úložiště v systému Windows, macOS a Linux. Toto téma popisuje, jak se používá k nahrávání a stahování dat z úložiště objektů blob Azure. Nástroj můžete stáhnout z [Microsoft Azure úložiště Průzkumníka](http://storageexplorer.com/).

Pokyny pro technologií pro server slouží k přesunutí dat do a/nebo z úložiště objektů Blob Azure jsou propojené tady:
 
[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]   

 
> [AZURE.NOTE] Pokud používáte OM, který byl nastaven pomocí skriptů poskytovanou [dat pro výzkum virtuálních počítačích v Azure](machine-learning-data-science-virtual-machines.md), pak Průzkumníka úložišť Azure už nainstalovaný v OM.
 
> [AZURE.NOTE] Pro dokončení Úvod k úložišti objektů blob Azure odkažte na [Základy objektů Blob Azure](../storage/storage-dotnet-how-to-use-blobs.md) a [Služba objektů Blob Azure](https://msdn.microsoft.com/library/azure/dd179376.aspx).   

## <a name="prerequisites"></a>Zjistit předpoklady pro

Tento dokument předpokládá, že máte předplatné Azure, účtu úložiště a odpovídající klíč úložiště k tomuto účtu. Před nahrávání nebo stahování dat, musíte znát kód název a účet Azure úložiště účtu. 

- Nastavení Azure předplatného, najdete v článku [bezplatnou zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).
- Pokyny pro vytvoření účtu úložiště a usnadňující účet a klíčové informace podívejte se na [účty adresářové služby Azure o úložiště](../storage/storage-create-storage-account.md). Poznamenejte si přístupová klávesa účtu úložiště podle potřeby tento klíč pro připojení k účtu pomocí nástroje pro Průzkumníka úložišť Azure.
- Nástroj Průzkumníka úložišť Azure můžete stáhnout z [Microsoft Azure úložiště Průzkumníka](http://storageexplorer.com/). Přijměte výchozí nastavení při instalaci.


<a id="explorer"></a>
## <a name="use-azure-storage-explorer"></a>Použití Průzkumníka Azure úložiště 

Jak nahrát nebo stáhnout dat pomocí Průzkumníka úložišť Azure podle těchto kroků dokumentu 

1.  Spusťte Průzkumníka úložišť Microsoft Azure.
2.  Vyvoláte průvodce **přihlásit ke svému účtu...** , vyberte ikonu **účet Azure nastavení** , potom **Přidat účet** a zadejte přihlašovací údaje. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/add-an-azure-store-account.png)
3.  Vyvoláte průvodce **připojení k úložišti Azure** , vyberte ikonu **připojit k základnímu úložišti Azure** . ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-1.png)
4. Zadejte přístupové klávesy ze svého účtu Azure úložiště na **připojení k úložišti Azure** průvodce a potom **Další**. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-2.png)
5. Zadejte název účtu úložiště do pole **název účtu** a potom vyberte **Další**. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/attach-external-storage.png)
6. Přidání účtu úložiště teď měl seznam obsahovat. Vytvoření kontejneru objektů blob v účtu úložiště, klikněte pravým tlačítkem myši na uzel **Objektů Blob kontejnery** v tomto účtu, vyberte **Vytvořit kontejner objektů Blob**a zadejte název.
7. Odeslání dat do kontejneru, vyberte cílový kontejner a klikněte na tlačítko **Odeslat** .![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/storage-accounts.png)
8. Klikněte na **...** napravo od pole **soubory** , vyberte jeden nebo více souborů nahrávat ze systému souborů a klikněte na tlačítko **Nahrát** na zahájit nahrávání souborů.![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/upload-files-to-blob.png)
7. Pokud si chcete stáhnout data, výběr objektů blob v kontejneru odpovídající ke stažení a klikněte na **Stáhnout**. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/download-files-from-blob.png)


