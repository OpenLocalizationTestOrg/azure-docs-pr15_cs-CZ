<properties
    pageTitle="Pokyny pro řešení úložiště | Microsoft Azure"
    description="Informace o klíčových návrh a implementace pokyny pro nasazení řešení úložiště služby Azure infrastruktury."
    documentationCenter=""
    services="virtual-machines-linux"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="storage-infrastructure-guidelines"></a>Pokyny pro infrastrukturu úložiště

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

Tento článek se zaměřuje na principy požadavky na úložiště a důležité informace o návrhu pro dosažení optimálního virtuálního počítače (OM) výkonu.


## <a name="implementation-guidelines-for-storage"></a>Implementace pokyny pro ukládání

Rozhodnutí:

- Potřebujete používat standardní nebo Premium úložiště pro vaše pracovní zátěž?
- Potřebujete prokládání disků k vytvoření disků větší než 1023 GB?
- Potřebujete prokládání disků k dosažení optimálního výkonu vstupu a výstupu pro vaše pracovní zátěž?
- K čemu nastavení účtů úložiště potřebujete hostovat pracovní zátěž IT nebo infrastruktury?

Úkoly:

- Zkontrolujte požadavky na vstupu a výstupu aplikací nasazujete a plánování příslušné číslo a zadejte úložiště účtů.
- Vytvoření účtů úložiště používat stejný systém názvů. Můžete použít Azure rozhraní příkazového řádku nebo na portálu.


## <a name="storage"></a>Úložiště

Azure úložiště je klíčové součástí nasazení a správu virtuálních počítačích (VMs) a aplikací. Azure úložiště poskytuje služby pro ukládání data souboru, Nestrukturovaná data a zprávy a je také část infrastruktury podpůrné VMs.

K dispozici pro podporu VMs existují dva typy úložiště účtů:

- Standardní úložiště účty umožňují přístup k úložišti objektů blob (slouží k ukládání Azure OM disků), úložiště tabulek a jejich ukládání fronty a ukládání souborů.
- Účty [Premium úložiště](../storage/storage-premium-storage.md) předvádění podpora výkonných maximum, minimum latence disku náročné úloh vstupu a výstupu, třeba MongoDB Sharded obrázku. Premium úložiště v současné době podporuje pouze u disků OM Azure.

Azure vytvoří VMs s disk operačního systému, dočasné disku a žádný nebo více disků volitelná data. Disk operačního systému a disků dat jsou objekty BLOB Azure stránky, zatímco dočasné disku je uložená místně na uzel, kde jsou umístěná v počítači. Dávejte při návrhu aplikace můžete jenom dočasná disk u dočasnou dat jako OM může poštovním mezi hosts během události údržby. Data uložená na dočasném disku bude ztracen.

Životnost a dostupnost poskytuje společnost podkladového prostředí úložišti Azure zajistit, zůstávají vaše data chráněn před neplánované selhání údržbu nebo hardwaru. Při návrhu prostředí Azure úložiště, můžete replikovat OM úložiště:

- místně v rámci dané Azure datacentru
- přes Azure datacentrech v dané oblasti
- přes Azure datacentrech mezi různými oblastmi.

Můžete si přečíst [Další informace o možnosti replikace vysoké dostupnosti](../storage/storage-introduction.md#replication-for-durability-and-high-availability).

Operační systém disků a disků data obsahují maximální velikosti doručovaných 1023 gigabajtů (GB). Maximální velikosti doručovaných objektů blob je 1024 GB a který musí obsahovat metadata (zápatí) souboru virtuálního pevného disku (GB je 1024<sup>3</sup> bajtů). Logické hlasitost správce (LVM) umožňuje překročí tímto limitem tak, že fond společně disků dat můžete prezentovat logické svazky větší než 1023GB vaší OM.

Existují určité datové limity škálovatelnost při návrhu úložišti Azure nasazení: Další informace najdete v článku [limity předplatné a přihlašovacích údajů Microsoft Azure, kvóty a omezení](azure-subscription-service-limits.md#storage-limits) . Viz také [Azure úložiště výkon a škálovatelnost cílů](../storage/storage-scalability-targets.md).

Pro uložení aplikace mohou být uloženy data nestrukturovaná objektu například dokumenty, obrázky, zálohy, konfiguraci protokoly, např. Použití úložiště objektů blob. Místo aplikace zápisu do virtuálního disku připojené k OM můžete přímo k úložišti objektů blob Azure zápisu aplikace. Úložiště objektů BLOB také poskytuje možnost [žádanou a zajímavých úložiště úrovní](../storage/storage-blob-storage-tiers.md) podle potřeby dostupnost a omezení nákladů.


## <a name="striped-disks"></a>Prokládané disků
Kromě umožňuje vytvářet disků větší než 1023 GB v mnoha případech pomocí prokládání dat disků rozšiřuje možnosti výkonu povolením více objektů BLOB do pozadí úložiště pro jeden hlasitost. S prokládání, vstupu/výstupu potřebná k psaní a načtení dat z jedné logický disk vytvářejí souběžně.

Azure ukládá kvůli omezením počtu disků dat a šířku pásma k dispozici v závislosti na velikosti OM. Podrobnosti najdete v tématu [velikosti virtuálních počítačích](virtual-machines-linux-sizes.md).

Pokud používáte prokládání disků disků Azure dat, zvažte následující pokyny:

- Data disků buďte vždy maximální velikost (1023 GB).
- Připojte disků maximální dat povolených pro OM velikost.
- Použijte LVM.
- Eliminování možnosti použití Azure datový disk možnosti ukládání do mezipaměti (ukládání do mezipaměti zásad = None).

Další informace najdete v tématu [Konfigurace LVM na OM Linux](virtual-machines-linux-configure-lvm.md).


## <a name="multiple-storage-accounts"></a>Více účtů úložiště

Při návrhu prostředí Azure úložiště, můžete použít více účtů úložiště jako počet poštovních VMs nasadíte zvýšení. Tento postup vám pomůže distribuování se vstupu a výstupu v podkladovém infrastrukturu Azure úložiště k udržení optimálního výkonu VMs a aplikací. Při návrhu aplikace, které nasazujete vezměte v úvahu požadavky na vstupu a výstupu, který má každý OM a rozložení, tyto VMs u účtů Azure úložiště. Zkuste abyste tak předešli seskupení všech vysokou v/v náročné VMs v jenom jednu nebo dvě úložiště účty.

Další informace o možnostech vstupu a výstupu různé možnosti Azure úložiště a některé doporučujeme maximální hodnoty najdete v tématu [Výkon a škálovatelnost cílů Azure úložiště](../storage/storage-scalability-targets.md).


## <a name="next-steps"></a>Další kroky

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 