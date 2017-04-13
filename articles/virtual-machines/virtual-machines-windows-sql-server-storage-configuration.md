<properties
    pageTitle="Konfigurace úložiště pro SQL Server VMs | Microsoft Azure"
    description="Toto téma popisuje, jak Azure nakonfiguruje úložiště pro SQL Server VMs během vytváření (Správce prostředků nasazení modelu). Také vysvětluje, jak můžete nakonfigurovat úložiště pro vaše existující VMs SQL serveru."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="ninarn"
    manager="jhubbard"    
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/04/2016"
    ms.author="ninarn" />

# <a name="storage-configuration-for-sql-server-vms"></a>Konfigurace úložiště pro SQL Server VMs

Při konfiguraci obrázek virtuálního počítače SQL serveru v Azure portálu pomáhá automatické konfigurace úložiště. Jedná se o programovém připojování úložiště bude v angličtině, zpřístupnění úložiště SQL serveru a konfigurace pro optimalizaci výkonu určitých požadavky.

Toto téma vysvětluje, jak nakonfiguruje Azure úložiště pro vaše VMs SQL Server během vytváření i pro existující VMs. Toto nastavení je založena na [výkon doporučené postupy](virtual-machines-windows-sql-performance.md) pro systém SQL Server Azure VMs.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klasický nasazení modelu.

## <a name="prerequisites"></a>Zjistit předpoklady pro
Konfigurace nastavení automatické ukládání virtuální počítač vyžaduje následující vlastnosti:

- Zřízení [Galerie SQL serveru](virtual-machines-windows-sql-server-iaas-overview.md#option-1-deploy-a-sql-vm-per-minute-licensing).
- Používá [Správce prostředků nasazení modelu](../resource-manager-deployment-model.md).
- Používá [Premium úložiště](../storage/storage-premium-storage.md).

## <a name="new-vms"></a>Nové VMs
Následující části popisují, jak nakonfigurovat úložiště pro nové virtuálních počítačích SQL serveru.

### <a name="azure-portal"></a>Azure portálu
Když zřizujete OM Azure pomocí Galerie serveru SQL Server, můžete automatické konfigurace úložiště pro nové OM. Určete omezení velikosti úložiště, omezení výkonu a typ pracovního vytížení. Následující obrázek ukazuje zásuvné konfigurace úložiště používaný během SQL OM zřizování.

![SQL Server OM úložiště konfigurace během vytváření](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-provisioning.png)

Na základě vašeho výběru, Azure provádí následující úkoly konfigurace úložiště po vytvoření OM:

- Vytvoří a disků datový úložiště premium k virtuální počítač.
- Konfiguruje discích dat, které mají být přístupné pro SQL Server.
- Konfiguruje disků dat do fondu úložiště na základě zadaného velikost a výkon (procesorů a výkon) požadavků.
- Nové jednotky počítače virtuální přiřadí fondu úložiště.
- Optimalizuje toto nové jednotky podle povahy zadaný pracovní zátěž (datové sklady, transakční zpracování nebo Obecné).

Další podrobnosti o tom, jak Azure konfiguruje nastavení úložiště najdete v [části konfigurace úložiště](#storage-configuration). Úplné informace o tom, jak vytvořit OM Server SQL Azure portálu najdete v článku [vytváření kurz](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="resource-manage-templates"></a>Správa šablon zdroje
Pokud používáte následující šablony správce prostředků, dvou discích dat premium se připojují ve výchozím nastavení se žádné konfigurace fondu úložiště. Můžete však upravíte tyto šablony změnit počet zobrazených disků dat premium, které jsou připojené k virtuální počítač.

- [Vytvoření OM pomocí automatického zálohování](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autobackup)
- [Vytvoření OM s automatické opravy](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autopatching)
- [Vytvoření OM integrací AKV](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-keyvault)

## <a name="existing-vms"></a>Existující VMs
Pro existující VMs SQL Server můžete změnit některé nastavení úložiště na portálu Azure. Vyberte svůj OM, přejděte do oblasti, nastavení a vyberte SQL Server – konfigurace. SQL Server – konfigurace zásuvné zobrazuje aktuální využití úložiště vaší OM. Všechny jednotky, které jsou vaše OM zobrazené v tomto grafu. Pro každou jednotku úložný prostor zobrazí čtyři části:

- SQL data
- Protokol SQL
- Ostatní (úložiště než SQL)
- K dispozici

![Nakonfigurovat úložiště pro systém SQL Server existující OM](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-existing.png)

Pokud chcete nakonfigurovat úložiště k přidání nové jednotky nebo rozšířit existující jednotky, klikněte na odkaz upravit nad grafem.

Možnosti konfigurace, které se zobrazí, se liší podle toho, zda použili jste tuto funkci před. Pokud chcete použít poprvé, můžete určit vašim požadavkům úložiště pro nové jednotky. Pokud jste dříve použili tato funkce k vytvoření jednotky, můžete rozšířit tohoto disku.

### <a name="use-for-the-first-time"></a>Použití první
Je-li poprvé pomocí této funkce, můžete určit limitů úložiště velikost a výkon pro nové jednotky. Toto chování je podobný byste měli vidět při zřizování čas. Hlavní rozdíl je, že nejsou povoleny určit typ pracovního vytížení. Toto omezení chrání přerušení jakákoliv existující konfigurace systému SQL Server na počítač virtuální.

![Konfigurace posuvníky úložiště SQL serveru](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-usage-sliders.png)

Azure vytvoří novou jednotku podle vašich požadavků. V tomto scénáři Azure provádí následující úkoly konfigurace úložiště:

- Vytvoří a disků datový úložiště premium k virtuální počítač.
- Konfiguruje discích dat, které mají být přístupné pro SQL Server.
- Konfiguruje disků dat do fondu úložiště na základě zadaného velikost a výkon (procesorů a výkon) požadavků.
- Nové jednotky počítače virtuální přiřadí fondu úložiště.

Další podrobnosti o tom, jak Azure konfiguruje nastavení úložiště najdete v [části konfigurace úložiště](#storage-configuration).

### <a name="add-a-new-drive"></a>Přidání nové jednotky
Pokud jste už nakonfigurovali úložiště na vaše OM SQL serveru, rozbalování úložiště vyvolá dvou možností. První možnost je přidat do nové složky můžete zvýšit úroveň výkonu svého OM.

![Přidání nové jednotky OM SQL](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-add-new-drive.png)

Po přidání jednotku, je však nutné provést některé další ruční konfigurace dosáhnout zvýšení výkonu.

### <a name="extend-the-drive"></a>Rozšíření jednotku
Další možností pro rozšíření úložiště je rozšířit existující jednotky. Tato možnost zvyšuje volného jednotky, ale zvýšení výkonu. Úložiště fondy nelze změnit počet sloupců po vytvoření fondu úložiště. Počet sloupců zjistí počet paralelní zápisy, které můžete jsou na discích data. Všechny přidané dat disků nemůžete proto zvýšit výkon. Můžete jenom poskytují další úložiště pro data zapisuje. Toto omezení také znamená, že, když rozšíření jednotku, určí počet sloupců minimální počet disků dat, které se dají přidat. Pokud vytváříte fondu úložiště s disků čtyři dat požadovaný počet sloupců je vlastně taky čtyři. Kdykoli rozšíření úložiště, musíte přidat nejméně čtyři dat discích.

![Rozšíření jednotku pro OM SQL](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-extend-a-drive.png)

## <a name="storage-configuration"></a>Konfigurace úložiště
Tato část obsahuje odkaz pro požadované změny konfigurace úložiště Azure automaticky provádí během SQL OM zřizování nebo konfigurace na portálu Azure.

- Pokud jste zvolili míň než dva TBs úložiště pro vaše OM, nevytvoří Azure fondu úložiště.
- Pokud vyberete aspoň dva TBs úložiště pro vaše OM Azure nakonfiguruje fondu úložiště. Další část Toto téma obsahuje podrobné informace o konfiguraci fondu úložiště.
- Automatické ukládání konfigurace vždy používá disků datový [úložiště premium](../storage/storage-premium-storage.md) P30. Tedy 1:1 mapování mezi zvolený počet TB a počet jejích dat disků připojené k vaší OM.

Ceny informace, najdete v článku stránce [ceny úložiště](https://azure.microsoft.com/pricing/details/storage) na kartě **Místa na disku** .

### <a name="creation-of-the-storage-pool"></a>Vytvoření fondu úložiště
K vytvoření fondu úložiště na VMs serveru SQL Azure používá následující nastavení.

| Nastavení | Hodnota |
|-----|-----|
| Velikost pruhu  | 256 znalostní bázi Knowledge Base (datové sklady); 64 KB (transakční) |
| Disk velikosti | 1 TB |
| Mezipaměti | Pro čtení |
| Pole přidělení velikost | Velikost 64 KB NTFS přidělení jednotky |
| Inicializace rychlé soubor | Povoleno |
| Zamknout stránky v paměti | Povoleno |
| Obnovení | Jednoduchý obnovení (bez odolnost proti chybám) |
| Počet sloupců | Počet datových disků<sup>1</sup> |
| TempDB umístění | Uložené na data disků<sup>2</sup> |

<sup>1</sup> po vytvoření fondu úložiště nelze změnit počet sloupců ve fondu úložiště.

<sup>2</sup> toto nastavení platí jenom pro první jednotku, kterou vytvoříte pomocí funkce Konfigurace úložiště.

## <a name="workload-optimization-settings"></a>Nastavení optimalizace zátěží na projektu
Následující tabulka popisuje dostupné možnosti Typ tři pracovního vytížení a jejich odpovídající optimalizaci:

| Typ pracovního vytížení | Popis | Optimalizace |
|-----|-----|-----|
| **Obecné** | Výchozí nastavení, které podporuje většina úloh | Žádná |
| **Transakční zpracování** | Optimalizuje úložný prostor pro tradiční databáze OLTP úloh | Příznak. 1117 trasování<br/>Příznak 1118 trasování |
| **Data skladování** | Optimalizuje úložný prostor pro vytváření sestav a analýze úloh | Příznak 610 trasování<br/>Příznak. 1117 trasování |

>[AZURE.NOTE] Typ pracovního vytížení můžete zadat pouze když zřizujete virtuálního počítače SQL tak, že vyberete v kroku konfigurace úložiště.

## <a name="next-steps"></a>Další kroky
Další témata týkající se serverem SQL Azure VMs najdete v článku [SQL Server na virtuálních počítačích Azure](virtual-machines-windows-sql-server-iaas-overview.md).
