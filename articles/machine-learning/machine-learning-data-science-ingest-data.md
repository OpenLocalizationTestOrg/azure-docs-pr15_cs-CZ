<properties 
    pageTitle="Načtení dat do úložiště prostředí pro analýzy | Microsoft Azure" 
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
    ms.date="09/19/2016" 
    ms.author="bradsev" />

# <a name="load-data-into-storage-environments-for-analytics"></a>Načtení dat do úložiště prostředí pro analýzy

Proces týmu dat pro výzkum vyžaduje, že data nelze požití nebo načíst do spoustu různých prostředích zpracovávané nebo analyzovat v nejvhodnější v jednotlivých fázích procesu. Cíle dat běžně používané pro zpracování zahrnout úložiště objektů Blob Azure, databáze SQL Azure SQL Server na OM Azure HDInsight (Hadoop) a výukové počítače Azure. 

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

Tato **Nabídka** odkazuje na témata, která popisují, jak jedí dat do těchto prostředí cílové místo, kam se ukládají a zpracování data.

Technické a obchodních potřeb, stejně jako na počáteční místo formát a velikost dat bude určovat, do kterých je třeba data požití k dosažení cílů analýzách prostředí cílové. Není běžné scénáře vyžadovat data, která chcete přesunout mezi několika prostředích dosáhnout řadu úkolů potřebná k vytvoření prediktivní modelu. Tato posloupnost úkolů mohou obsahovat například průzkum, předzpracování, čištění, odběr dolů a školení modelu.
