<properties
   pageTitle="Příručku: obnovení z místního Azure | Microsoft Azure"
   description="Článek principy a obnovení navrhování systémy z místního infrastruktury Azure"
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="azure-resiliency-technical-guidance-recovery-from-on-premises-to-azure"></a>Azure odolnost proti chybám příručku: obnovení z místního Azure

Azure poskytuje komplexní sady služby pro povolení rozšíření místních datacentra na Azure důvodů vysoké dostupnosti a havárie obnovení:

* __Připojení sítě__: S virtuální privátní sítě bezpečně rozšíření místních sítě do cloudu.
* __Výpočet__: zákazníkům, kteří používají Hyper-V místním můžete "zvedněte a shift" virtuálních počítačích existující (VMs) na Azure.
* __Úložiště__: StorSimple rozšiřuje systému souborů k základnímu úložišti Azure. Azure záložní služba poskytuje zálohování pro soubory a databáze SQL Azure úložiště.
* __Replikace databáze__: Díky SQL Server 2014 (nebo novější) dostupnost skupin, můžete používat vysoké dostupnost a havárie obnovení pro vaše data místní.

##<a name="networking"></a>Sítě

Virtuální sítě Azure slouží k vytvoření logicky izolace oddílu v Azure a zabezpečené připojení místních datacentra nebo jeden klientský počítač pomocí připojení IPsec. Virtuální síti můžete využít výhod infrastruktury scalable na vyžádání, v Azure zároveň connectivity data a aplikace do místního nasazení, včetně systémů v systému Windows Server, sálové počítače a UNIX. Další informace najdete v tématu [Azure sítě si přečtěte následující dokumentaci](../virtual-network/virtual-networks-overview.md) .

##<a name="compute"></a>Výpočet

Pokud používáte Hyper-V místním, můžete "zvedněte a shift" stávající virtuálních počítačích Azure a systémem Windows Server 2012 (nebo novější) bez provádění změn OM nebo převedení OM formáty poskytovatelů služeb. Další informace najdete v tématu [o discích a VHD Azure virtuálních počítačích](../virtual-machines/virtual-machines-linux-about-disks-vhds.md).

##<a name="azure-site-recovery"></a>Obnovení Azure webu

Pokud chcete havárie obnovení jako služba (DRaaS), Azure poskytuje [Obnovení webu Azure](https://azure.microsoft.com/services/site-recovery/). Obnovení Azure webu nabízí komplexní ochranu VMware, Hyper-V a fyzické servery. Obnovení webu Azure můžete pomocí jiného místního serveru nebo Azure jako webu obnovení. Další informace o obnovení webu Azure najdete v [dokumentaci k obnovení webu Azure](https://azure.microsoft.com/documentation/services/site-recovery/).

##<a name="storage"></a>Úložiště

Existuje několik možností pro používání Azure jako záložní webu pro místní data.

###<a name="storsimple"></a>StorSimple

StorSimple bezpečně a transparentně integruje cloudového úložiště pro místní aplikace. Nabízí taky jednoho zařízení, které poskytuje vrstvené místní výkonné a cloudového úložiště, live archivace, ochrana cloudové dat a obnovení havárie. Další informace najdete v tématu [StorSimple product stránky](https://azure.microsoft.com/services/storsimple/).

###<a name="azure-backup"></a>Azure zálohování

Azure zálohování umožňuje zálohování cloudu pomocí známé nástroje Zálohování v systému Windows Server 2012 (nebo novější), Windows Server 2012 Essentials (nebo novější) a System Center 2012 Data Protection Manager (nebo novější). Tyto nástroje stanovit záložní správy nezávisle na umístění úložiště zálohy, zda místním disku nebo v úložišti Azure pracovního postupu. Po zálohování do cloudu, můžete snadno uživatelé obnovit zálohování na libovolný server.

S přírůstková zálohy se převedou pouze změn souborů v cloudu. Díky efektivní využití úložného prostoru zmenšete šířku pásma a podporu v okamžiku využívání více verzí data. Můžete taky použít další funkce, jako jsou zásady uchovávání informací dat, komprese dat a systémy přepravy dat omezení. Použití Azure jako umístění zálohy má zjevných výhod zálohy se automaticky "externí". Díky navíc požadavky na zabezpečení a ochrana na místě záložní média.

Další informace najdete v tématu [Co je Azure zálohování?](../backup/backup-introduction-to-azure-backup.md) a [Konfigurace Azure zálohování dat DPM](https://technet.microsoft.com/library/jj728752.aspx).

##<a name="database"></a>Databáze

Může být řešení obnovení havárie pro databáze SQL serveru ve IT hybridní prostředí pomocí skupiny dostupnosti AlwaysOn odrážející strukturu databáze, protokol a zálohování a obnovení s úložiště objektů Blob Azure. Všechny tato řešení pomocí serveru SQL Server na virtuálních počítačích Azure spuštěné.

Skupiny dostupnosti AlwaysOn lze použít v prostředí hybridní IT místo, kam databáze repliky jsou oba místní i schránkami v cloudu. To je ukázáno v následujícím diagramu.

![SQL Server AlwaysOn dostupnost skupiny v hybridním cloudu architektura](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-3.png)

Odrážející strukturu databáze může taky zahrnovat místní servery a cloudu v nastavení na certifikát. Následující obrázek znázorňuje tohoto konceptu.

![SQL Server databáze odrážející strukturu v cloudu architektura hybridní](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-4.png)

Protokol dodávky mohou sloužit k synchronizaci místních databáze se databáze SQL serveru v Azure virtuálního počítače.

![Protokol serveru SQL Server v cloudu architektura hybridní](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-5.png)

Nakonec můžete zálohujete místní databázi přímo k úložišti objektů Blob Azure.

![Obecnějším údajům SQL serveru k úložišti objektů Blob Azure v cloudu architektura hybridní](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-6.png)

Další informace najdete v tématu [vysoké dostupnost a havárie obnovení pro systém SQL Server ve virtuálních počítačích Azure](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md) a [zálohování a obnovení pro systém SQL Server ve virtuálních počítačích Azure](../virtual-machines/virtual-machines-windows-sql-backup-recovery.md).

##<a name="checklists-for-on-premises-recovery-in-microsoft-azure"></a>Seznamy úkolů pro místní obnovení Microsoft Azure

###<a name="networking"></a>Sítě

  1. Přečtěte si část sítě tohoto dokumentu.
  2. Použití virtuální síť, aby zajistila zabezpečené připojení místních do cloudu.

###<a name="compute"></a>Výpočet

  1. Přečtěte si část výpočetním tohoto dokumentu.
  2. Změna umístění VMs mezi Hyper-V a Azure.

###<a name="storage"></a>Úložiště

  1. Přečtěte si část úložiště v tomto dokumentu.
  2. Využití výhod StorSimple služby pro používání cloudového úložiště.
  3. Pomocí služby Azure zálohování.

###<a name="database"></a>Databáze

  1. Přečtěte si část databáze v tomto dokumentu.
  2. Zvažte použití systému SQL Server na Azure VMs jako zálohování.
  3. Nastavení skupiny dostupnosti AlwaysOn.
  4. Konfigurace certifikátu očekáváte odrážející strukturu.
  5. Použití protokolu dodávky.
  6. Zálohování databáze místní k úložišti objektů Blob Azure.

##<a name="next-steps"></a>Další kroky

Tento článek je součástí řady zaměření na [příručku Azure odolnost proti chybám](./resiliency-technical-guidance.md). Další článek v této řadě je [obnovení z poškození dat nebo náhodného odstranění](./resiliency-technical-guidance-recovery-data-corruption.md).