<properties
    pageTitle="Sledování a odstraňování případných problémů ochranu virtuálních počítačích a fyzické servery | Microsoft Auzre" 
    description="Obnovení Azure webu souřadnic replikace, převzetí a obnovení virtuálních počítačích umístěných na serverech místní Azure nebo vedlejší datacentra. Tento článek umožňuje sledovat a řešení potíží s zamknutí VMM nebo Hyper-V webu." 
    services="site-recovery" 
    documentationCenter="" 
    authors="anbacker" 
    manager="mkjain" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="10/13/2016"    
    ms.author="rajanaki"/>
    
# <a name="monitor-and-troubleshoot-protection-for-virtual-machines-and-physical-servers"></a>Sledování a odstraňování případných problémů ochranu virtuálních počítačích a fyzických serverů

Tato příručka Poradce při potížích s a sledování umožňuje zjistěte, sledování stavu replikace a řešení potíží s postupy pro obnovení webu Azure.

## <a name="understanding-the-components"></a>Principy složek

### <a name="vmwarephysical-site-deployment-for-replication-between-on-premises-and-azure"></a>Nasazení serveru VMware/fyzické replikace mezi místním prostředím a Azure.
Chcete-li nastavit DR mezi místním počítači VMware/fyzické; Konfigurace serveru, předlohy cílové a proces serveru musí nakonfigurované. Při aktivaci ochranu pro zdrojový server Azure obnovení webu nainstaluje mobilita služby. Pokládat dotazy k místním výpadku po zdrojovému serveru převrácení dojde k chybě na Azure, zákazníky potřebám nastavit proces serveru v Azure a cílové předlohy serveru místní chránit zdrojovému serveru zpět do znovu vytvořený v místním nasazení. 

![Nasazení serveru VMware/fyzické replikace mezi místním & Azure](media/site-recovery-monitoring-and-troubleshooting/image18.png)

### <a name="vmm-site-deployment-for-replication-between-on-premises-site"></a>Nasazení serveru VMM replikace mezi místních webů.

V rámci nastavení DR mezi dvěma místního pracoviště; Azure zprostředkovatele obnovení webů musí stáhnout a nainstalovat na VMM server. Zprostředkovatel vyžaduje připojení k Internetu zajistit, aby všechny operace spouštěný z portálu Azure převedeny na operace místního jako zamknout, vypnutí stranu primárního virtuálních počítačích jako součást převzetí služeb při selhání atd.

![Nasazení serveru VMM replikace mezi místních webů](media/site-recovery-monitoring-and-troubleshooting/image1.png)

### <a name="vmm-site-deployment-for-replication-between-on-premises--azure"></a>Nasazení serveru VMM replikace mezi místním & Azure.

V rámci nastavení DR mezi místním & Azure; Azure zprostředkovatele obnovení webů musí stáhnout a nainstalovat na server VMM spolu s agentem služeb obnovení Azure, které je potřeba nainstalovat v každém Hyper-V hostiteli. Další informace naleznete v tématu [Principy webu Azure ochranu](./site-recovery-understanding-site-to-azure-protection.md) .

![Nasazení serveru VMM replikace mezi místním & Azure](media/site-recovery-monitoring-and-troubleshooting/image2.png)

### <a name="hyper-v-site-deployment-for-replication-between-on-premises--azure"></a>Nasazení serveru pro Hyper-V replikace mezi místním & Azure

Toto je stejné jako u VMM nasazení – pouze rozdílem je poskytovatele & Agent získá nainstalovaným Hyper-V host. Další informace naleznete v tématu [Principy webu Azure ochranu](./site-recovery-understanding-site-to-azure-protection.md) .

## <a name="monitor-configuration-protection-and-recovery-operations"></a>Sledování operací konfigurace, ochranu a obnovení

Všechny operace ve funkci Automatické obnovení systému získá auditovány a sledovat klikněte v části "Práce". V případě konfigurace, ochranu nebo Chyba při obnovení přejděte na kartu úlohy a zjistit, zda jsou všechny chyby.

![Sledování operací konfigurace, ochranu a obnovení](media/site-recovery-monitoring-and-troubleshooting/image3.png)

Jakmile najdete chyby ve skupinovém rámečku zobrazení úlohy, vyberte ÚLOHU a klikněte na podrobnosti pro tento projekt.

![Sledování operací konfigurace, ochranu a obnovení](media/site-recovery-monitoring-and-troubleshooting/image4.png)

Podrobnosti o chybě vám pomůže identifikovat možných příčin a doporučení pro tento problém.

![Sledování operací konfigurace, ochranu a obnovení](media/site-recovery-monitoring-and-troubleshooting/image5.png)

V případě výše uvedené zdá jiné operaci, která probíhá kvůli které ochranu konfigurace neúspěšně. Ujistěte se, vyřešit problém podle doporučení – tam po klikněte na RESART znovu spustit operaci.

![Sledování operací konfigurace, ochranu a obnovení](media/site-recovery-monitoring-and-troubleshooting/image6.png)

Možnost RESTART není k dispozici pro všechny operace – ty, které nemá parametrem RESTART přejděte zpátky k objektu a znovu provést operaci ještě jednou. Každá práce lze zrušit kdykoli přímo během průběh pomocí na tlačítko Storno.

![Sledování operací konfigurace, ochranu a obnovení](media/site-recovery-monitoring-and-troubleshooting/image7.png)

## <a name="monitor-replication-health-for-virtual-machine"></a>Monitorování stavu replikace virtuálního počítače

Zprostředkovatel automatickým obnovením systému ústřední & vzdálené sledování portálu Azure pro každou z chráněné entity. Přejděte k položkám chráněné tam po vyberte VMM MRAČNECH nebo ochranu skupiny. Karta VMM OBLAKA se týká jen VMM na základě nasazení a všech ostatních případech mít chráněné entity klikněte v části skupiny na ochranu.

![Monitorování stavu replikace virtuálního počítače](media/site-recovery-monitoring-and-troubleshooting/image8.png)

Tam po vyberte chráněné entity podle příslušných cloudové nebo skupině zámek. Jakmile vyberete chráněné entity všechny povolené operace se zobrazí v dolní části okna.

![Monitorování stavu replikace virtuálního počítače](media/site-recovery-monitoring-and-troubleshooting/image9.png)

Uvedené výše v případě virtuální počítač, že stav je důležité – můžete kliknutím na tlačítko Podrobnosti o CHYBĚ v dolní části zobrazí chyba. Podle "možných příčin" a "Doporučení" uvedené řešení tohoto problému.

![Monitorování stavu replikace virtuálního počítače](media/site-recovery-monitoring-and-troubleshooting/image10.png)

![Monitorování stavu replikace virtuálního počítače](media/site-recovery-monitoring-and-troubleshooting/image11.png)

Poznámka: Pokud jsou aktivní operace, které jsou v průběhu nebo neúspěšných přejděte do zobrazení úlohy jak jsme zmínili dříve zobrazíte konkrétní chybu projektu.

## <a name="troubleshoot-on-premises-hyper-v-issues"></a>Poradce při potížích pro Hyper-V místním

Připojení konzoly správce pro Hyper-V místním, vyberte virtuální počítač a najdete v článku replikace stavu.

![Poradce při potížích pro Hyper-V místním](media/site-recovery-monitoring-and-troubleshooting/image12.png)

V tomto případě *Stavu replikace* se označuje jako kritický – *Zobrazení replikace stav* na Zobrazit podrobnosti.

![Poradce při potížích pro Hyper-V místním](media/site-recovery-monitoring-and-troubleshooting/image13.png)

Pro názvy případů místo, kam se pozastaví replikace virtuálního počítače, klikněte pravým tlačítkem vyberte *replikace*->*životopisu replikace*
![Poradce při potížích s místním problémy Hyper-V](media/site-recovery-monitoring-and-troubleshooting/image19.png)

V případě, že virtuální počítač migruje nové Hyper-V hostitel (v rámci clusteru, nebo do samostatného počítače) nakonfiguroval prostřednictvím automatickým obnovením systému, nebudou mít vliv na replikace virtuálního počítače. Zajistěte, aby nový hostitel Hyper-V splňuje všechny na za – požadavky a se konfigurují automatickým obnovením systému.

### <a name="event-log"></a>Protokol událostí

| Zdroje událostí                | Podrobnosti                                                                                                                                                                                           |
|-------------------------  |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------    |
| **Aplikace a protokoly/Microsoft/VirtualMachineManager/serveru/Správce služeb** (VMM Server)   |  Díky tomu užitečné protokolování pro řešení potíží s mnoha různých VMM problémy. |
| **Aplikace a služby protokoly/MicrosoftAzureRecoveryServices/replikace** (Hyper-V hostitel)   | Díky tomu užitečné protokolování pro mnoho potíží s agentem služeb Microsoft Azure obnovení. <br/> ![Zdroj události hostitele Hyper-V](media/site-recovery-monitoring-and-troubleshooting/eventviewer03.png) |
| **Aplikace a služby protokoly/Microsoft/Azure webu obnovení/poskytovatele/provozní** (Hyper-V hostitel)   | Díky tomu užitečné protokolování pro odstraňování potíží mnoho Služba obnovení webu Microsoft Azure. <br/> ![Zdroj události hostitele Hyper-V](media/site-recovery-monitoring-and-troubleshooting/eventviewer02.png) |
| **Aplikace a protokoly/Microsoft/Windows/Hyper-V-VMMS/Správce služeb** (Hyper-V hostitel) | Díky tomu užitečné protokolování k řešení potíží mnoho Hyper-V počítači virtuální správy. <br/> ![Zdroj události hostitele Hyper-V](media/site-recovery-monitoring-and-troubleshooting/eventviewer01.png) |


### <a name="hyper-v-replication-logging-options"></a>Možnosti protokolování replikace Hyper-V

Všechny události týkající se Hyper-V otevřené přihlášeni Hyper-V-VMMS\\správce protokolu umístěné pod **protokoly aplikací a služeb\\Microsoft\\Windows**. Analytický protokol navíc může být užitečné pro Hyper-V-VMMS. Chcete-li tento protokol, nejdřív protokoly ladění a zobrazitelný v Prohlížeč událostí. Otevřete Prohlížeč událostí, klikněte v **nabídce Zobrazit**, klikněte na **Zobrazit ladění protokoly a**.

![Poradce při potížích pro Hyper-V místním](media/site-recovery-monitoring-and-troubleshooting/image14.png)

Analytický protokol se zobrazuje v části Hyper-V-VMMS

![Poradce při potížích pro Hyper-V místním](media/site-recovery-monitoring-and-troubleshooting/image15.png)

V podokně **akcí** klikněte na **Povolit protokol**. Po povolení to vypadá v **Nástroji Sledování výkonu** tak, jak relaci trasování událostí umístěné pod **sady dat kolekcí.**

![Poradce při potížích pro Hyper-V místním](media/site-recovery-monitoring-and-troubleshooting/image16.png)

Zobrazíte informace shromážděné nejdřív ukončit relaci trasování zakázáním protokol a potom protokol uložit a znovu ho otevřete v prohlížeči událostí nebo pomocí dalších nástrojů převeďte ji podle potřeby.



## <a name="reaching-out-for-microsoft-support"></a>Zastihnout for Microsoft Support

### <a name="log-collection"></a>Kolekce protokolů

Zámek VMM webů naleznete v tématu sběr požadovaných protokoly [kolekce protokolů funkci Automatické obnovení systému pomocí nástroje pro podporu diagnostiky Platform (SDP)](http://social.technet.microsoft.com/wiki/contents/articles/28198.asr-data-collection-and-analysis-using-the-vmm-support-diagnostics-platform-sdp-tool.aspx) .

Pro ochranu pro Hyper-V webu stáhněte si [nástroj](https://dcupload.microsoft.com/tools/win7files/DIAG_ASRHyperV_global.DiagCab) a spouštět na hostiteli Hyper-V shromáždit protokoly.

Scénáře VMware/fyzické naleznete v tématu [Azure obnovení protokolu u kolekce webů VMware a fyzické webu ochranu](http://social.technet.microsoft.com/wiki/contents/articles/30677.azure-site-recovery-log-collection-for-vmware-and-physical-site-protection.aspx) sběr požadovaných protokoly.

Nástroj shromažďují protokoly místně v části náhodně pojmenované podsložku ve skupinovém rámečku **%LocalAppData%\ElevatedDiagnostics**

![Ukázka kroky zobrazené z ochrany Hyper-V webu.](media/site-recovery-monitoring-and-troubleshooting/animate01.gif)

### <a name="opening-a-support-ticket"></a>Otevření požadavek podpory můžete

Zvýšit požadavek podpory můžete obnovení systému, kontaktujte podporu Azure pomocí adresy URL na <http://aka.ms/getazuresupport>

## <a name="kb-articles"></a>Články

-   [Jak zachováte písmeno chráněné virtuálních počítačích, které jsou selhání nebo migruje se na Azure](http://support.microsoft.com/kb/3031135)
-   [Správa místních využití šířky pásma sítě Azure ochrany](https://support.microsoft.com/kb/3056159)
-   [Funkci Automatické obnovení systému: Chyba "prostředek clusteru nebyl nalezen" při pokusu o povolení ochrany virtuálního počítače](http://support.microsoft.com/kb/3010979)
-   [Pochopení a řešení potíží s Hyper-V otevřené Průvodce](http://www.microsoft.com/en-in/download/details.aspx?id=29016) 

## <a name="common-asr-errors-and-their-resolutions"></a>Běžné chyby automatickým obnovením systému a jejich řešení

Tady jsou běžné chyby, které může přístupů a jejich řešení. Každá chyba jsou popsány v samostatné stránky WIKIWEBU.

### <a name="general"></a>Obecné
-   <span style="color:green;">Nový</span> Úlohy [Nedaří se chyba "operaci probíhá." Chyba 505, 514, 532](http://social.technet.microsoft.com/wiki/contents/articles/32190.azure-site-recovery-jobs-failing-with-error-an-operation-is-in-progress-error-505-514-532.aspx)
-   <span style="color:green;">Nový</span> Pokud se chyba "Serveru není připojený k Internetu" [úlohy. Chyba 25018](http://social.technet.microsoft.com/wiki/contents/articles/32192.azure-site-recovery-jobs-failing-with-error-server-isn-t-connected-to-the-internet-error-25018.aspx)

### <a name="setup"></a>Nastavení
-   [VMM server nelze zaregistrovat vzhledem k interní chybě. Získáte zobrazení úlohy na portálu webu obnovení podrobné informace o chybě. Opětovným spuštěním instalačního programu zaregistrovat serveru.](http://social.technet.microsoft.com/wiki/contents/articles/25570.the-vmm-server-cannot-be-registered-due-to-an-internal-error-please-refer-to-the-jobs-view-in-the-site-recovery-portal-for-more-details-on-the-error-run-setup-again-to-register-the-server.aspx)
-   [Nelze se připojit k trezoru Hyper-V obnovení správce. Zkontrolujte nastavení proxy serveru a zkuste to později.](http://social.technet.microsoft.com/wiki/contents/articles/25571.a-connection-cant-be-established-to-the-hyper-v-recovery-manager-vault-verify-the-proxy-settings-or-try-again-later.aspx)

### <a name="configuration"></a>Konfigurace
-   [Nejde vytvořit ochranu skupiny: došlo k chybě při načítání seznamu serverů.](http://blogs.technet.com/b/somaning/archive/2015/08/12/unable-to-create-the-protection-group-in-azure-site-recovery-portal.aspx)
-   [Host (hostitel) pro Hyper-V obrázku obsahuje aspoň jeden statické síťový adaptér nebo žádné připojeného adaptéry je nakonfigurovaný na používání DHCP.](http://social.technet.microsoft.com/wiki/contents/articles/25498.hyper-v-host-cluster-contains-at-least-one-static-network-adapter-or-no-connected-adapters-are-configured-to-use-dhcp.aspx)
-   [VMM nemá oprávnění k provedení akce](http://social.technet.microsoft.com/wiki/contents/articles/31110.vmm-does-not-have-permissions-to-complete-an-action.aspx)
-   [Nelze vybrat účtu úložiště v rámci předplatného při konfiguraci ochrany](http://social.technet.microsoft.com/wiki/contents/articles/32027.can-t-select-the-storage-account-within-the-subscription-while-configuring-protection.aspx)

### <a name="protection"></a>Zámek
- <span style="color:green;">Nový</span> Selhání [Povolit ochranu se chyba "ochranu nejde nakonfigurovat pro virtuální počítač". Chyba 60007, 40003](http://social.technet.microsoft.com/wiki/contents/articles/32194.azure-site-recovery-enable-protection-failing-with-error-protection-couldn-t-be-configured-for-the-virtual-machine-error-60007-40003.aspx)
- <span style="color:green;">Nový</span> Selhání [Povolit ochranu se chyba "ochranu nebylo povoleno pro virtuální počítač." Chyba 70094](http://social.technet.microsoft.com/wiki/contents/articles/32195.azure-site-recovery-enable-protection-failing-with-error-protection-couldn-t-be-enabled-for-the-virtual-machine-error-70094.aspx)
- <span style="color:green;">Nový</span> [Migraci chyby 23848 - virtuální počítač bude přesouvat pomoci typ Live. Tato akce může přerušit stav ochrany obnovení virtuální počítač.](http://social.technet.microsoft.com/wiki/contents/articles/32021.live-migration-error-23848-the-virtual-machine-is-going-to-be-moved-using-type-live-this-could-break-the-recovery-protection-status-of-the-virtual-machine.aspx) 
- [Povolení ochrany se nezdařilo, protože Agent není nainstalovaná na hostitelském počítači](http://social.technet.microsoft.com/wiki/contents/articles/31105.enable-protection-failed-since-agent-not-installed-on-host-machine.aspx)
- [Vhodné hostitele virtuálního počítače otevřené nebyl nalezen - kvůli nízké využití prostředků](http://social.technet.microsoft.com/wiki/contents/articles/25501.a-suitable-host-for-the-replica-virtual-machine-can-t-be-found-due-to-low-compute-resources.aspx)
- [Vhodné hostitele virtuálního počítače otevřené nebyl nalezen - kvůli žádné logické síti připojených](http://social.technet.microsoft.com/wiki/contents/articles/25502.a-suitable-host-for-the-replica-virtual-machine-can-t-be-found-due-to-no-logical-network-attached.aspx)
- [Nelze se připojit k na hostitelském počítači otevřené - nelze vytvořit připojení](http://social.technet.microsoft.com/wiki/contents/articles/31106.cannot-connect-to-the-replica-host-machine-connection-could-not-be-established.aspx)


### <a name="recovery"></a>Obnovení
- VMM nelze dokončit Host (hostitel):
    -   [Převzetí bod vybrané obnovení virtuálního počítače: Obecné chyby odepření přístupu.](http://social.technet.microsoft.com/wiki/contents/articles/25504.fail-over-to-the-selected-recovery-point-for-virtual-machine-general-access-denied-error.aspx)
    -   [Pro Hyper-V nepodařilo převzít bod vybrané obnovení virtuálního počítače: přerušen zkuste novější bod obnovení. (0x80004004)](http://social.technet.microsoft.com/wiki/contents/articles/25503.hyper-v-failed-to-fail-over-to-the-selected-recovery-point-for-virtual-machine-operation-aborted-try-a-more-recent-recovery-point-0x80004004.aspx)
    -   Připojení k serveru nelze založení (0x00002EFD)
        -   [Povolení zpětné replikace virtuálního počítače se nezdařilo pro Hyper-V](http://social.technet.microsoft.com/wiki/contents/articles/25505.a-connection-with-the-server-could-not-be-established-0x00002efd-hyper-v-failed-to-enable-reverse-replication-for-virtual-machine.aspx)
        -   [Povolení replikace virtuálního počítače virtuálního počítače se nezdařilo pro Hyper-V](http://social.technet.microsoft.com/wiki/contents/articles/25506.a-connection-with-the-server-could-not-be-established-0x00002efd-hyper-v-failed-to-enable-replication-for-virtual-machine-virtual-machine.aspx)
    -   [Nelze potvrdit převzetí virtuálního počítače](http://social.technet.microsoft.com/wiki/contents/articles/25508.could-not-commit-failover-for-virtual-machine.aspx)
-   [Obnovení plán obsahuje virtuálních počítačích, které nejsou připravena k plánované překlopení](http://social.technet.microsoft.com/wiki/contents/articles/25509.the-recovery-plan-contains-virtual-machines-which-are-not-ready-for-planned-failover.aspx)
-   [Virtuální počítač není připraveno k plánované překlopení](http://social.technet.microsoft.com/wiki/contents/articles/25507.the-virtual-machine-isn-t-ready-for-planned-failover.aspx)
-   [Virtuální počítač není spuštěný a není vypnutý](http://social.technet.microsoft.com/wiki/contents/articles/25510.virtual-machine-is-not-running-and-is-not-powered-off.aspx)
-   [Operace pásma v nepřítomnosti se stalo na virtuální počítač a převzetí potvrdit se nezdařila.](http://social.technet.microsoft.com/wiki/contents/articles/25507.the-virtual-machine-isn-t-ready-for-planned-failover.aspx)
-   Převzetí test
    -   [Převzetí nelze spuštěná, protože probíhá test převzetí](http://social.technet.microsoft.com/wiki/contents/articles/31111.failover-could-not-be-initiated-since-test-failover-is-in-progress.aspx)
-   <span style="color:green;">Nový</span>  Převzetí vyprší její časový limit "PreFailoverWorkflow úkolu WaitForScriptExecutionTaskTimeout" kvůli nastavení skupiny zabezpečení sítě přidružené virtuálního počítače nebo podsítě, ke kterému patří počítače. Podívejte se do ["PreFailoverWorkflow úkolu WaitForScriptExecutionTaskTimeout"](https://aka.ms/troubleshoot-nsg-issue-azure-site-recovery) podrobnosti.


### <a name="configuration-server-process-server-master-target"></a>Cíl předlohy konfigurace serveru, serveru obrázku
Konfigurace serveru (CS), Server procesu (PS), předlohy Targer (MT)
-   [Host ESXi, který je hostitelem PS/CS jako virtuálního počítače se nezdaří s fialovou obrazovku smrti.](http://social.technet.microsoft.com/wiki/contents/articles/31107.vmware-esxi-host-experiences-a-purple-screen-of-death.aspx)

### <a name="remote-desktop-troubleshooting-after-failover"></a>Vzdálená plocha Poradce při potížích po překlopení
-   Množství zákazníků mít před problémy se připojit k selhalo přes OM v Azure. [Použijte Poradce při potížích dokument RDP do OM](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx)

#### <a name="adding-a-public-ip-on-a-resource-manager-virtual-machine"></a>Přidání veřejnou IP počítače virtuální správce zdrojů
Pokud je tlačítko **Připojit** na portálu zašedlé a nejste připojení k Azure pomocí postupu Express nebo připojení VPN k webu, potřebujete k vytvoření a přiřazení vaší OM veřejnou IP adresu, než budete moct použít RDP/SSH. Postupujte podle návodu pro přidání veřejnou IP rozhraní sítě virtuálního počítače.  

![Přidání veřejnou IP na rozhraní sítě se nezdařila přes virtuální počítač](media/site-recovery-monitoring-and-troubleshooting/createpublicip.gif)