<properties
    pageTitle="Dostupnost a obnovení pro systém SQL Server | Microsoft Azure"
    description="Informace o různých typů HADR strategie pro SQL serveru ve virtuálních počítačích Azure."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/20/2016"
    ms.author="MikeRayMSFT" />

# <a name="high-availability-and-disaster-recovery-for-sql-server-in-azure-virtual-machines"></a>Vysoký dostupnost a havárie obnovení pro systém SQL Server ve virtuálních počítačích Azure

## <a name="overview"></a>Základní informace

Virtuálních počítačích Microsoft Azure (VMs) se serverem SQL Server pomůže snížit náklady vysoké dostupnosti a havárie obnovení (HADR) databáze řešení. Většina řešení SQL Server HADR podporují služby Azure virtuálních počítačích, jen Azure i jako hybridní řešení. Řešení pro jen Azure celého systému HADR spuštěna v Azure. Hybridní konfigurace součástí řešení spuštěna v Azure a další části spustí místní ve vaší organizaci. Flexibilní Azure prostředí umožňuje přesunutí úplně nebo částečně Azure uspokojili rozpočtu a HADR požadavky systémů databáze SQL serveru.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="understanding-the-need-for-an-hadr-solution"></a>Principy nutnosti HADR řešení

Můžete se rozhodnout, zajistit, že systém databáze má HADR možnosti, které vyžadují smlouva úrovni služeb (SLA). To, že Azure poskytuje mechanismy dostupnost, například služba retušování pro cloudovými službami a obnovení zjišťování chyb pro virtuálních počítačích sám zaručit, že splníte požadované SLA. Tyto mechanismy chránit dostupnost VMs, ale ne dostupnost běží uvnitř VMs serveru SQL Server. Je možné instanci systému SQL Server k chybě při OM online a správný. Kromě toho i dostupnost, které mechanismy poskytovanou Azure umožňují výpadku VMs kvůli události jako je třeba obnovení z software nebo selhání hardwaru a inovací operační systém.

Kromě toho Geo nadbytečné úložiště (GRS) v Azure, které je prováděn za funkci geo replikace, nemusí být řešení obnovení odpovídající havárie pro databáze. Protože geo replikace odešle data asynchronní, může dojít ke ztrátě v případě havárie nejnovější aktualizace. Další informace o omezeních geo replikace jsou zahrnuty části [Geo replikace není podporována pro data a souborů protokolu na samostatném discích](#geo-replication-support) .

## <a name="hadr-deployment-architectures"></a>HADR nasazení architektury

Technologie HADR SQL serveru, které podporují služby Azure patří:

- [Vždy na dostupnost skupiny](https://technet.microsoft.com/library/hh510230.aspx)
- [Odrážející strukturu databáze](https://technet.microsoft.com/library/ms189852.aspx)
- [Protokol expedice](https://technet.microsoft.com/library/ms187103.aspx)
- [Zálohování a obnovení se službou úložiště objektů Blob Azure](https://msdn.microsoft.com/library/jj919148.aspx)
- [Vždy na instancí překlopení obrázku](https://technet.microsoft.com/library/ms189134.aspx)

Je možné zahrnout do sloučeného technologie společně provádět SQL Server řešení, které má dostupnost a možnosti obnovení havárie. V závislosti na technologii, který používáte může vyžadovat hybridního nasazení VPN tunelem s Azure virtuální sítě. Následující části obsahují zobrazí některých architekturu nasazení příklad.

## <a name="azure-only-high-availability-solutions"></a>Jen Azure: dostupnost řešení

Můžete mít řešení dostupnost pro databáze SQL serveru v Azure pomocí vždy na dostupnost nebo databáze odrážející strukturu.

| Technologie                               | Příklad architektury                    |
| ---------------------------------------- | ---------------------------------------- |
| **Vždy na dostupnost skupiny**        | Dostupnost ostatními spuštěné v Azure VMs vysoké dostupnosti ve stejné oblasti. Musíte nakonfigurovat řadiče domény OM, protože systému Windows Server převzetí clusterů (WSFC) vyžaduje doméně služby Active Directory.<br/> ![Vždy na dostupnost skupiny](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_ha_always_on.gif)<br/>Další informace najdete v tématu [Konfigurace vždy na dostupnost skupin v Azure (grafické)](virtual-machines-windows-portal-sql-alwayson-availability-groups.md). |
| **Vždy na instancí překlopení obrázku** | Překlopení obrázku instance (FCI), které vyžadují sdílené úložiště, můžete vytvořit 2 různými způsoby.<br/><br/>1. na FCI na dvěma uzly WSFC spuštěné v Azure VMs s úložištěm nepodporuje clusterů řešení jiných výrobců. Konkrétní příklad, který používá s DataKeeper najdete v článku [dostupnost pro sdílení souborů pomocí WSFC a 3 software výrobce s Datakeeper](https://azure.microsoft.com/blog/high-availability-for-a-file-share-using-wsfc-ilb-and-3rd-party-software-sios-datakeeper/).<br/><br/>2. na FCI na dvěma uzly WSFC spuštěné v Azure VMs s iSCSI vzdálené cílové sdílené úložiště bloků přes ExpressRoute. Například NetApp soukromé úložiště NPS () zpřístupňuje cíle iSCSI prostřednictvím ExpressRoute s Equinix Azure VMs.<br/><br/>Sdílené úložiště třetích stran a řešení replikace dat obraťte se dodavatele problémy týkající se přístupu k datům na překlopení.<br/><br/>Všimněte si, že FCI nad [úložiště souborů Azure](https://azure.microsoft.com/services/storage/files/) není podporované použití ještě, protože toto řešení nevyužívá Premium úložiště. Pracujeme na podporu to brzy bude k dispozici. |

## <a name="azure-only-disaster-recovery-solutions"></a>Jen Azure: řešení obnovení havárie

Můžete mít řešení havárie obnovení databáze systému SQL Server v Azure pomocí vždy na dostupnost, databázi odrážející strukturu nebo zálohování a obnovení s objekty BLOB úložiště.

| Technologie                               | Příklad architektury                    |
| ---------------------------------------- | ---------------------------------------- |
| **Vždy na dostupnost skupiny**        | Dostupnost repliky probíhají napříč několika datacentrech v Azure VMs havárie obnovení. Toto řešení více oblastí chrání před výpadku dokončení webu. <br/> ![Vždy na dostupnost skupiny](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_alwayson.png)<br/>V oblasti ostatními by měl být v rámci stejné Cloudová služba a stejné VNet. Vzhledem k tomu, že každá oblast bude mít samostatný VNet, tato řešení vyžadují VNet ke VNet připojení. Další informace najdete v tématu [Konfigurace VPN k webu Azure klasické portálu](../vpn-gateway/vpn-gateway-site-to-site-create.md). |
| **Odrážející strukturu databáze**                   | Hlavní a zrcadlové a servery v různých datacentrech havárie obnovení. Je třeba nasadit pomocí serveru certifikátů, protože doméně služby active directory nemůže zahrnovat více datacentrech.<br/>![Odrážející strukturu databáze](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_dbmirroring.gif) |
| **Zálohování a obnovení se službou úložiště objektů Blob Azure** | Provoz databází zálohovala přímo k úložišti objektů blob v různých datacentra havárie obnovení.<br/>![Zálohování a obnovení](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_backup_restore.gif)<br/>Další informace najdete v tématu [zálohování a obnovení pro systém SQL Server ve virtuálních počítačích Azure](virtual-machines-windows-sql-backup-recovery.md). |

## <a name="hybrid-it-disaster-recovery-solutions"></a>Hybridní IT: Řešení obnovení havárie

Můžete mít řešení obnovení havárie pro databáze systému SQL Server ve IT hybridní prostředí pomocí vždy na skupiny dostupnost odrážející strukturu databáze, protokol a zálohování a obnovení s úložištěm Azure blogu.

| Technologie                               | Příklad architektury                    |
| ---------------------------------------- | ---------------------------------------- |
| **Vždy na dostupnost skupiny**        | Některé dostupnost repliky spuštěné v Azure VMs a replikami systém místních webů havárie obnovení. Web výrobní může být buď místně nebo v Azure datacentra.<br/>![Vždy na dostupnost skupiny](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_alwayson.gif)<br/>Protože dostupnost ostatními musí být ve stejném WSFC clusteru, musí mít clusteru WSFC rozpětí obou sítích (WSFC clusteru více podsítě). Tato konfigurace vyžaduje připojení VPN mezi Azure a v místní síti.<br/><br/>Obnovení úspěšné havárie z vašich databází nainstalujte řadiče domény otevřené na webu obnovení havárie.<br/><br/>Je možné použít Průvodce přidáním otevřené v SSMS přidáte Azure otevřené do existující vždy na dostupnost skupiny. Další informace najdete v tématu kurz: rozšíření vždy na dostupnost skupiny Azure. |
| **Odrážející strukturu databáze**                   | Jeden partnerů aplikaci OM Azure a jiných chod místní obnovení webů havárie pomocí serveru certifikátů. Partneři nemusí být ve stejné doméně služby Active Directory a bez připojení VPN je nutný.<br/>![Odrážející strukturu databáze](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_dbmirroring.gif)<br/>Jiné databáze odrážející strukturu scénář zahrnuje jeden partnerů aplikaci OM Azure a ostatních pracovního místních ve stejné doméně služby Active Directory pro obnovení webů havárie. Je nutné [připojení VPN mezi Azure virtuální sítě a v místní síti](../vpn-gateway/vpn-gateway-site-to-site-create.md) .<br/><br/>Obnovení úspěšné havárie z vašich databází nainstalujte řadiče domény otevřené na webu obnovení havárie. |
| **Protokol expedice**                         | Jeden server se službou Azure OM a jiných spuštění místní obnovení webů havárie. Protokol dodávky závisí na sdílení souborů systému Windows, je nutné připojení VPN mezi Azure virtuální sítě a v místní síti.<br/>![Protokol expedice](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_log_shipping.gif)<br/>Obnovení úspěšné havárie z vašich databází nainstalujte řadiče domény otevřené na webu obnovení havárie. |
| **Zálohování a obnovení se službou úložiště objektů Blob Azure** | Místní databáze výrobní zálohovala přímo k úložišti objektů blob Azure havárie obnovení.<br/>![Zálohování a obnovení](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_backup_restore.gif)<br/>Další informace najdete v tématu [zálohování a obnovení pro systém SQL Server ve virtuálních počítačích Azure](virtual-machines-windows-sql-backup-recovery.md). |

## <a name="important-considerations-for-sql-server-hadr-in-azure"></a>Důležité informace týkající se SQL Server HADR v Azure

Azure VMs úložiště a networking mít různou charakteristikou provozní než místního, nevirtuálním IT infrastrukturu. Úspěšné provádění řešení HADR SQL serveru v Azure vyžaduje pochopit tyto rozdíly a řešení tak, aby pokryly je Navrhněte.

### <a name="high-availability-nodes-in-an-availability-set"></a>Dostupnost uzlů v sadě dostupnosti

Dostupnost sady v Azure umožňují umístit uzly dostupnost do samostatných poruch domény (FDs) a aktualizace domény (UDs). Pro Azure VMs být umístěny ve stejné sadě dostupnost třeba nasadíte je ve stejném cloudové služby. Pouze uzly ve stejném cloudové služby může účastnit stejnou sadu dostupnosti. Další informace najdete v tématu [Správa dostupnosti virtuálních počítačích](virtual-machines-windows-manage-availability.md).

### <a name="wsfc-cluster-behavior-in-azure-networking"></a>WSFC obrázku chování v sítích Azure

Službu RFC-nevyhovujícím DHCP v Azure mohou způsobit vystavením určité konfigurace clusteru WSFC selhání kvůli síťový název clusteru přiřazený duplicitní IP adresu, třeba na stejné IP adresu jako jednu z uzlů. Při je to problém implementace vždy na dostupnost skupiny, který závisí na funkci WSFC.

Zaměřte scénáře dvěma uzly clusteru je vytvořen a do online režimu:

1. Clusteru je online a potom Uzel1 žádosti dynamicky přiřazené IP adresu pro síťový název obrázku.

2. Žádná IP adresa než Uzel1 na vlastní IP adresu je dán službu DHCP od služby DHCP rozpozná žádost pochází Uzel1 samotné.

3. Windows zjistí, že duplicitní adresu přiřazen k Uzel1 i clusteru síťový název a výchozí skupiny clusteru přestane přechodu do online.

4. Výchozí skupiny clusteru přesune do Uzel2, zpracuje Uzel1 na IP adresu jako obrázku IP adresu, která přináší výchozí skupiny clusteru online.

5. Při Uzel2 pokusí připojení protokolem UZEL1, paketů zaměřené na Uzel1 nikdy ponechat Uzel2, protože se převádí na Uzel1 IP adresu samotné. Uzel2 pak nemůže vytvořit připojení protokolem Uzel1 dojde ke ztrátě kvora a vypne clusteru.

6. Mezitím Uzel1 poslat paketů Uzel2, ale nemůžete odpovídat Uzel2. Uzel1 dojde ke ztrátě kvora a vypne clusteru.

Tento scénář můžete předejít přiřazením nepoužitý statické IP adresu, například odkaz místní IP adresa v rámci jako 169.254.1.1, síťový název obrázku tak, aby síťový název clusteru online. Tento proces zjednodušit, najdete v článku [Konfigurace Windows překlopení obrázku v Azure skupin vždy na dostupnost](http://social.technet.microsoft.com/wiki/contents/articles/14776.configuring-windows-failover-cluster-in-windows-azure-for-alwayson-availability-groups.aspx).

Další informace najdete v tématu [Konfigurace vždy na dostupnost skupin v Azure (grafické)](virtual-machines-windows-portal-sql-alwayson-availability-groups.md).

### <a name="availability-group-listener-support"></a>Dostupnost podpory posluchače skupiny

Dostupnost skupiny posluchače jsou podporovány na Azure VMs systémy Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 a Windows Server 2016. Toto odborné pomoci je umožnily pomocí vyrovnávání zatížení koncové body zapnuta VMs Azure, které jsou dostupnost skupiny uzlů. Postup zvláštní kroky konfigurace pro posluchače práce pro oba klientské aplikace spuštěné v Azure jakož i s místní.

Existují dva hlavní možnosti pro nastavení vašeho posluchače: externí (veřejné) nebo interní. Externí (veřejné) posluchače používá internetové Vyrovnávání zatížení a souvisí s veřejné virtuální IP (VIP) přístupný přes internet. Vnitřní posluchače používá Vyrovnávání zatížení vnitřní a podporují pouze klienty v rámci stejné virtuální sítě. Buď načíst vyrovnávání typ, musíte povolit přímé Server vrátit. 

Pokud skupiny dostupnosti zahrnuje více Azure podsítí (například nasazení protínající Azure oblastí), musí obsahovat klienta připojovací řetězec "**MultisubnetFailover = True**". Výsledkem pokusy o paralelní připojení replikám v různých podsítí. Pokyny týkající se nastavení posluchače najdete v tématu

- [Konfigurace posluchače ILB skupin vždy na dostupnost v Azure](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md).
- [Konfigurace externí posluchače pro vždy na dostupnost skupin v Azure](virtual-machines-windows-classic-ps-sql-ext-listener.md).

Pořád připojením ke každé otevřené dostupnost samostatně připojením přímo na instance služby. Taky protože jsou vždy na skupiny dostupnost zpětně kompatibilní s databází odrážející strukturu klienty, můžete připojit k repliky dostupnost jako databáze odrážející strukturu partnerů, dokud replik jsou nakonfigurované tak, podobně jako odrážející strukturu databáze:

- Jeden primární otevřené a jednu vedlejší otevřené

- Sekundární otevřené nakonfigurovaný mezi možnostmi čitelný (**Čitelné sekundární** nastavena na hodnotu **Ne**)

Příklad klienta připojovací řetězec, který odpovídá této konfiguraci odrážející strukturu profesionálové databáze pomocí ADO.NET nebo SQL Server Native Client je níže:

    Data Source=ReplicaServer1;Failover Partner=ReplicaServer2;Initial Catalog=AvailabilityDatabase;

Další informace o připojení klientů najdete tady:

- [Používání klíčových slov řetězec připojení s SQL Server Native Client](https://msdn.microsoft.com/library/ms130822.aspx)
- [Připojení klientů k databázi odrážející strukturu relace (SQL Server)](https://technet.microsoft.com/library/ms175484.aspx)
- [Připojení k posluchače skupiny dostupnosti v hybridním IT](http://blogs.msdn.com/b/sqlalwayson/archive/2013/02/14/connecting-to-availability-group-listener-in-hybrid-it.aspx)
- [Skupiny dostupnosti posluchače, připojení klientů a aplikace záložní (SQL Server)](https://technet.microsoft.com/library/hh213417.aspx)
- [Použití odrážející strukturu databáze připojovací řetězec s skupiny dostupnosti](https://technet.microsoft.com/library/hh213417.aspx)

### <a name="network-latency-in-hybrid-it"></a>Latence sítě v hybridním IT

Nasadíte řešení HADR za předpokladu, že může být dobu s vysoká sítí latenci mezi místním sítí a Azure. Při nasazení repliky Azure, byste měli použít asynchronní potvrdit místo synchronní potvrdit synchronizace režimu. Při nasazení databáze odrážející strukturu servery v místním i v Azure, použití režimu Vysoký výkon místo režimu Vysoký zabezpečení.

### <a name="geo-replication-support"></a>Podporu replikace GEO

GEO replikace v Azure disků nepodporuje datového souboru a soubor protokolu stejné databáze uložený na samostatném discích. GRS zreplikuje změny na každém disku nezávisle na sobě a asynchronní. Tento postup zaručuje pořadí zápisu v rámci jednoho disku na výtisku geo replikovat, ale ne replikovat geo kopie více discích. Pokud konfigurace databáze pro ukládání datového souboru a jeho soubor protokolu na samostatném discích obnoveného disků po selhání může obsahovat aktuální kopii datový soubor než souboru protokolu konce dokončováním zápisu protokolu SQL serveru a vlastnosti ACID transakce. Pokud nemáte možnost zakázat geo replikace na účtu úložiště, měli byste zachovat všechny dat a souborů protokolu pro danou databázi na stejném disku. Pokud používáte více než jeden disk kvůli velikost databáze, budete muset jednu řešení obnovení havárie výše uvedené zajistit redundance dat nasazení.

## <a name="next-steps"></a>Další kroky

Pokud je potřeba vytvořit Azure virtuálního počítače se serverem SQL Server, přečtěte si článek [zřizování SQL Server virtuálního počítače na Azure](virtual-machines-windows-portal-sql-server-provision.md).

Nejlepší možný výkon ze serveru SQL Server na Azure OM, získáte informace v tématu [Výkon doporučené postupy pro systém SQL Server ve virtuálních počítačích Azure](virtual-machines-windows-sql-performance.md).

Další témata týkající se serverem SQL Azure VMs najdete v článku [SQL Server na virtuálních počítačích Azure](virtual-machines-windows-sql-server-iaas-overview.md).

### <a name="other-resources"></a>Další zdroje

- [Instalace nové doménové služby Active Directory v Azure](../active-directory/active-directory-new-forest-virtual-machine.md)
- [Vytvoření obrázku WSFC pro vždy na dostupnost skupin v Azure OM](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a)
