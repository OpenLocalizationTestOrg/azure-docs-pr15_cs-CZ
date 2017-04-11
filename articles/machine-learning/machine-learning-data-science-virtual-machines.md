<properties
    pageTitle="Data pro výzkum virtuálních počítačích v Azure | Microsoft Azure"
    description="Nastavení přes virtuální počítač vědy dat"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard" 
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="xibingao;bradsev" />

# <a name="data-science-virtual-machines-in-azure"></a>Data pro výzkum virtuálních počítačích v Azure

Pokyny tady, že, která popisují, jak nastavit OM Azure a OM Azure pomocí služby SQL jako servery IPython Poznámkový blok. Virtuální počítač Windows nastaven podpůrných nástrojů, jako jsou IPython Poznámkový blok, Průzkumníka úložišť Azure a AzCopy, jakož i další nástroje, které jsou vhodné pro datové vědy projekty. Azure Explorer úložiště a AzCopy, například nabídněte pohodlný předávat data k základnímu úložišti Azure ze svého místního počítače a stáhněte si ho do místního počítače z úložiště. 

Této nabídce odkazů na témata, která popisují, jak nastavit různých prostředích pro výzkum dat používaných [Týmu dat pro výzkum obrázku (TDSP)](data-science-process-overview.md).

[AZURE.INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

Některé typy Azure virtuálních počítačích můžete zřízení a nakonfigurovali má být použit jako součást prostředí pro výzkum cloudové data. Rozhodnutí o které charakter virtuálního počítače použít závisí na typu a množství dat modelovat pomocí počítače učení a cílová pro příslušná data v cloudu. 

* Otázky potřeba zvážit při rozhodování tento, najdete v článku [Plánování daného Azure počítače výukové dat pro výzkum prostředí](machine-learning-data-science-plan-your-environment.md). 
* Katalog některé scénáře, ke kterým může dojít při provádění pokročilé technologie pro analýzu najdete v článku [scénáře pro pokročilé technologie pro analýzu procesy a technologii Azure počítač přečíst](machine-learning-data-science-plan-sample-scenarios.md)

Dvě sady pokyny jsou k dispozici:

* [Nastavení Azure virtuálního počítače jako server IPython Poznámkový blok pro pokročilé technologie pro analýzu](machine-learning-data-science-setup-virtual-machine.md) ukazuje, jak zřídit Azure virtuálního počítače s IPython Poznámkový blok a dalších nástrojů byli zvyklí vědy dat pro případy, kdy formuláře Azure úložiště než SQL mohou sloužit k ukládání dat.

* [Nastavení Azure SQL Server virtuálního počítače jako server IPython Poznámkový blok pro pokročilé technologie pro analýzu](machine-learning-data-science-setup-sql-server-virtual-machine.md) ukazuje, jak zřídit virtuálního počítače serveru SQL Azure pomocí IPython Poznámkový blok a dalších nástrojů byli zvyklí vědy dat pro případy, kdy databáze SQL mohou sloužit k ukládání dat.

Jakmile zřizování a nakonfigurovanou, tyto virtuálních počítačích připraveni pro použití jako poznámkový blok IPython servery průzkum a zpracování dat a pro další úkoly potřeby ve spojení s Azure počítače učení s pomocí týmu dat pro výzkum obrázku (TDSP). Další kroky procesu vědy dat jsou namapované na [TDSP naučná stezka](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) a může zahrnovat kroky, které přesunout data do SQL Server nebo HDInsight, zpracovat a přehrajte si ho tam při přípravě na výukové z dat s Azure počítače učení.


> [AZURE.NOTE] Azure virtuálních počítačích jsou cenou jako **platí jenom pro můžete použít**. Abyste měli jistotu, že jste nejsou účtované bez použití virtuálního počítače, musí být ve stavu **Zastaveno (Deallocated)** z [Azure klasické portálu](http://manage.windowsazure.com/). Podrobné pokyny nebo jak zrušit virtuálního počítače, přečtěte si téma [vypnutí a zrušit virtuální počítač není při použití](machine-learning-data-science-setup-virtual-machine.md#shutdown)
 
