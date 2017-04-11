<properties
    pageTitle="Přesunutí dat a od úložišti objektů Blob Azure | Microsoft Azure"
    description="Přesunutí dat a od úložišti objektů Blob Azure"
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
    ms.author="bradsev;sachouks" />

# <a name="move-data-to-and-from-azure-blob-storage"></a>Přesunutí dat a od úložišti objektů Blob Azure

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

Pokyny pro technologií pro server slouží k přesunutí dat do a/nebo z úložiště objektů Blob Azure jsou propojené tady:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]
 
Jaký způsob je pro vás nejlepší, závisí na vaší situaci. [Scénáře pro pokročilé technologie pro analýzu Azure počítač přečíst](machine-learning-data-science-plan-sample-scenarios.md) článek vám pomůže určit prostředky, které potřebujete pro různé pracovních postupů pro výzkum dat používá v procesu pokročilé technologie pro analýzu.

> [AZURE.NOTE] Pro dokončení Úvod k úložišti objektů blob Azure odkažte na [Základy objektů Blob Azure](../storage/storage-dotnet-how-to-use-blobs.md) a službě [objektů Blob Azure](https://msdn.microsoft.com/library/azure/dd179376.aspx).

Jako alternativu můžete použít [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) : 

- Vytvoření a naplánování kanálů, což načte data ze úložiště objektů blob Azure 
- předáte publikovaný Azure počítače výukové webové služby 
- Zobrazí výsledky prediktivní technologie pro analýzu a 
- Nahrajte výsledky k základnímu úložišti. 

Další informace najdete v tématu [prediktivní potrubí vytvořit pomocí Azure Data Factory a Azure počítače výuka](../data-factory/data-factory-azure-ml-batch-execution-activity.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Tento dokument předpokládá, že máte předplatné Azure, účtu úložiště a odpovídající klíč úložiště k tomuto účtu. Před nahrávání nebo stahování dat, musíte znát kód název a účet Azure úložiště účtu.

- Nastavení Azure předplatného, najdete v článku [bezplatnou zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).
- Pokyny pro vytvoření účtu úložiště a usnadňující účet a klíčové informace podívejte se na [účty adresářové služby Azure o úložiště](../storage/storage-create-storage-account.md).
