<properties
    pageTitle="Přesunutí dat z Azure úložiště a | Microsoft Azure"
    description="Tento článek obsahuje přehled různých metod pro přesunutí dat z Azure úložiště a."
    services="storage"
    documentationCenter=""
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="micurd"/>

# <a name="moving-data-to-and-from-azure-storage"></a>Přesunutí dat z Azure úložiště a

Pokud chcete přesunout data v místním úložišti Azure (nebo naopak), je potřeba různé způsoby, jak to udělat. Přístup, který je pro vás nejlepší závisí na vaší situaci. V tomto článku najdete rychlý přehled různých scénářích a odpovídající nabídky pro každou z nich.

## <a name="building-applications"></a>Vytváření aplikací

Pokud vytváříte aplikaci, vývoj proti rozhraní REST API nebo jednu z našich mnoho knihoven klienta je skvělý způsob, jak přesouvat data do úložiště Azure.

Azure úložiště klienta RTF knihoven stanoví .NET, iOS, Java, Android, univerzální platformu systému Windows (UWP), Xamarin, C++, Node.JS, PHP, skutečné a Python. Knihoven klienta nabízejí pokročilé funkce, například Logika opakování, protokolování a paralelní nahrávání. Můžete taky vytvořit přímo proti rozhraní REST API, nazývaný tak, že libovolný jazyk, který zajišťuje žádosti o protokolu HTTP/HTTPS.

Najdete v článku [Začínáme s úložišti objektů Blob Azure](storage-dotnet-how-to-use-blobs.md) Další informace.

Nabízíme také [Pohyb Azure úložiště dat](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement) , která je určený pro výkonné kopírování dat z Azure a knihovny. Získáte naši pohyb dat [si přečtěte následující dokumentaci](https://github.com/Azure/azure-storage-net-data-movement) Další informace. 

## <a name="quickly-viewinginteracting-with-your-data"></a>Rychlé zobrazení/interakce s daty

Pokud budete potřebovat snadný způsob zobrazení dat Azure úložiště současně také mít možnost nahrávání a stahování dat, můžete pomocí Průzkumníka úložiště Azure.

Podívejte se na náš seznam [Azure úložiště Průzkumník](storage-explorers.md) Další informace.

## <a name="system-administration"></a>Správa systému

Pokud nevyžaduje ani jsou pohodlnější pomocí nástroje příkazového řádku (například správci systému), tady je několik možností pro trocha zamyšlení nad prezentacemi:

### <a name="azcopy"></a>AzCopy

AzCopy je určený pro výkonné kopírování dat z Azure úložiště a příkazového řádku nástroj Windows. Můžete taky zkopírovat data v rámci účtu úložiště nebo mezi účty různých úložiště.

Další informace v tématu [přenos dat pomocí nástroje příkazového řádku AzCopy](storage-use-azcopy.md) .

### <a name="azure-powershell"></a>Azure Powershellu

Azure Powershellu je moduly, které obsahuje rutiny pro správu služby Azure. Je založeným na úkolech příkazového řádku kostra domu a skriptovacího jazyka navržený zejména pro správce systému.

Naleznete na webu [Pomocí prostředí PowerShell Azure s úložištěm Azure](storage-powershell-guide-full.md) Další informace.

### <a name="azure-cli"></a>Azure rozhraní příkazového řádku

Azure rozhraní příkazového řádku obsahuje sadu otevřít zdroj různé platformy příkazy pro práci s Azure služby. Azure rozhraní příkazového řádku je k dispozici v systémech Windows, OSX a Linux.

V tématu [použití Azure rozhraní příkazového řádku s úložištěm Azure](storage-azure-cli.md) Další informace.

## <a name="moving-large-amounts-of-data-with-a-slow-network"></a>Přesunutí velké objemy dat se v pomalé síti

Jednou z největších problémů přidružený k přesunutí velké objemy dat je čas přenos. Pokud budete chtít načtení dat z Azure úložiště bez obav sítí náklady a vytváření kódu, Azure Import nebo Export je vhodné řešení.

Viz [Azure Import nebo Export](storage-import-export-service.md) zobrazíte další.

## <a name="backing-up-your-data"></a>Zálohování dat

Pokud potřebujete jednoduše zálohování dat k základnímu úložišti Azure, Azure záložní způsobem přejít. Jedná se o výkonné řešení pro zálohování místních dat a Azure VMs.

Najdete v článku [Zálohy Azure](../backup/backup-introduction-to-azure-backup.md) Další informace.

## <a name="accessing-your-data-on-premises-and-from-the-cloud"></a>Přístup k svůj data místně a z cloudu

Pokud potřebujete řešení pro přístup ke své data místně a v cloudu, měli zvážit, použití Azure na hybridní cloudové úložiště řešení, StorSimple. Tato řešení se skládá z fyzické StorSimple zařízení, aby inteligentně ukládá často používané dat na disky SSD, občas použité dat na pevných disků a neaktivní/zálohování/archivace dat v úložišti Azure.

V tématu [StorSimple](../storsimple/storsimple-overview.md) Další informace.

## <a name="recovering-your-data"></a>Obnovení dat

Pokud máte místní pracovního vytížení a aplikací, musíte mít řešení, které umožňuje vyhovovaly pokračovat v práci v případě selhání. Obnovení Azure webu zpracovává replikace, převzetí a obnovení virtuálních počítačích a fyzické servery. Replikovanou data se ukládají v úložišti Azure umožňuje eliminuje potřebu sekundární na místě datacentra.

V tématu [Obnovení webu Azure](../site-recovery/site-recovery-overview.md) Další informace.
