<properties
    pageTitle="Test výkonu a měřítko výsledky pro místní replikace Hyper-V místním s obnovení webu | Microsoft Azure"
    description="Tento článek obsahuje informace o výkonu testování místním systémem místní replikace pomocí obnovení webu Azure."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="07/06/2016"
    ms.author="raynew"/>

# <a name="performance-test-and-scale-results-for-on-premises-to-on-premises-hyper-v-replication-with-site-recovery"></a>Výkon otestujte a zobrazit výsledky místních replikace Hyper-V místním s obnovení webu

Obnovení webu Microsoft Azure umožňuje organizovat a Správa replikace virtuálních počítačích a fyzické servery Azure nebo vedlejší datacentra. Tento článek obsahuje výsledky, které jsme době replikace Hyper-V virtuálních počítačích mezi dvěma místní datacentrech testování výkonu.



## <a name="overview"></a>Základní informace

Cílem testování bylo podívat, jak obnovení webu Azure provádí během replikace konstantní stavu. Replikace konstantní stavu dochází, když virtuálních počítačích dokončili počáteční replikace a synchronizují delta změny. Je důležité měření výkonu použití konstantní stavu, protože se může stát, ve kterém Většina virtuálních počítačích zůstanou, dokud neočekávané výpadků dojít.


Zkušební nasazení se skládá ze dvou webů v místním serverem VMM každého webu. Toto zkušební nasazení je typické hlavy office/větev nasazení office, se sídlem budou sloužit jako primární webu a na pobočce jako sekundární nebo využití webu.

### <a name="what-we-did"></a>Jsme akce

Tady je toho, co jsme ve sloupci Podmínka předat:

1. Vytvořené pomocí šablony VMM virtuálních počítačích.

1. Začínáme s virtuálních počítačích a měřítka podle směrného plánu zachycení přesahuje 12 hodin.

1. Vytvořený mračnech serverech VMM primární a obnovení.

1. Ochrana nakonfigurované cloudu v Azure využití webu, včetně mapování mraků zdroje a obnovení.

1. Povolit ochranu virtuálních počítačích nebo aby mu umožnil dokončení počáteční replikace.

1. Počkat několik hodin ustálení systému.

1. 12 hodin, nezaznamenávají měřítka, u zajistit, že všechny virtuálních počítačích zůstal ve stavu replikace očekávaného tyto 12 hodin.

1. Změřte rozdílu mezi měřítka podle směrného plánu a měřítka replikace.

## <a name="test-deployment-results"></a>Výsledky testů nasazení

### <a name="primary-server-performance"></a>Primární výkonu

- Pro Hyper-V otevřené asynchronní slouží ke sledování změn soubor protokolu s minimální úložiště nároky na primárním serveru.

- Pro Hyper-V otevřené využívá vlastním udržované mezipaměti minimalizovat procesorů nároky pro účely sledování. Uloží zapíše VHDX v paměti a vyprázdní do souboru protokolu před odesílané protokol obnovení webu. Vyčištění disku také se stane, když zápisy přístupů předem limit.

- Následující graf zobrazuje režijních procesorů konstantní stavu replikace. Jsme můžete vidět, procesorů nároky kvůli replikace přibližně 5 %, které je úplně nízká.

![Primární výsledků](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744913.png)

Pro Hyper-V otevřené využívá paměti na primárním serveru pro optimalizaci výkonu disku. Jak ukazuje následující graf, paměť nároky na všech serverech primární clusteru je mezní. Paměti nároky uvedeno je procento paměti používané replikace ve srovnání s celkové nainstalované paměti na serveru Hyper-V.

![Primární výsledků](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744914.png)

Pro Hyper-V otevřené má minimální zatížením procesoru. Jak je vidět v grafu, replikace režijních je v rozsahu 2-3 %.

![Primární výsledků](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744915.png)

### <a name="secondary-recovery-server-performance"></a>Výkon serveru sekundární (obnovení)

Pro Hyper-V otevřené bude použit malou paměti serveru pro obnovení optimalizovat počet operací úložiště. Zobrazí se v grafu shrnuje spotřebu paměti na serveru pro obnovení. Paměti nároky uvedeno je procento paměti používané replikace ve srovnání s celkové nainstalované paměti na serveru Hyper-V.

![Sekundární výsledků](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744916.png)

Počet operací na webu obnovení je funkce počet zápisu na primární webu. Pojďme podívejte se na celkové operací na webu obnovení ve srovnání s celkové operací a operací na primární webu zápisu. Grafy zobrazit součet procesorů na webu obnovení je

- Kolem 1,5 zápisu procesorů na primární.

- Okolo 37 % celkového procesorů na primární webu.

![Sekundární výsledků](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744917.png)

![Sekundární výsledků](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744918.png)

### <a name="effect-of-replication-on-network-utilization"></a>Efekt replikace na využití sítě

Průměr 275 MB za sekundu šířka pásma používal mezi uzly primární a obnovení (plus pár komprese povolené) proti existující pásma 5 GB za sekundu.

![Využití sítě výsledků](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744919.png)

### <a name="effect-of-replication-on-virtual-machine-performance"></a>Efekt replikace na výkon virtuálního počítače

Důležitá poznámka je dopad replikace na výrobní pracovní vytížení virtuálních počítačích. Jestliže primární webu je dostatečně zřízení replikace, nesmí být žádný vliv na pracovní zatížení. Sledování mechanismus lightweight Hyper-V otevřené zaručuje, že nejsou během konstantní stavu replikace dopad pracovní vytížení ve virtuálních počítačích. To je znázorněn v následujících grafy.

Tento graf zobrazuje procesorů provedených ve virtuálních počítačích s jinou pracovního vytížení před a po povolení replikace. Můžete sledovat, že je žádný rozdíl mezi nimi.

![Výsledky otevřené efektu](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744920.png)

Následující diagram ukazuje výkon virtuálních počítačích s jinou pracovního vytížení před a po povolení replikace. Můžete sledovat, že replikace nemá žádný vliv významný.

![Výsledky otevřené efekty](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744921.png)

### <a name="conclusion"></a>Uzavření

Výsledky jasně zobrazení, obnovení webu Azure svázáno s Hyper-V otevřené změní i s minimálního nároky pro velké obrázku.  Obnovení Azure webu poskytuje jednoduché nasazení, replikace, Správa a sledování. Pro Hyper-V otevřené poskytuje potřebné infrastrukturu pro změnu velikosti úspěšné replikace. Plánování optimální nasazení měli byste že Stáhnout [Hyper-V otevřené kapacita plánování](https://www.microsoft.com/download/details.aspx?id=39057).

## <a name="test-environment-details"></a>Podrobnosti o prostředí testu

### <a name="primary-site"></a>Hlavní web

- Hlavní web má clusteru, který obsahuje pět Hyper-V servery 470 virtuálních počítačích.

- Virtuálních počítačích spusťte různých pracovního vytížení a všichni mají povolenou ochranou obnovení webu Azure.

- Prostor úložiště pro uzel clusteru poskytuje společnost iSCSI SAN. Model – Hitachi HUS130.

- Každý server clusteru má čtyři kartami () jeden GB/s každý.

- Dvě síťových karet připojeni k privátní sítě iSCSI a dvě připojeni k externí podnikové sítě. Jedna z těchto externích sítí je rezervován pro pouze komunikace clusteru.

![Primární hardwarové požadavky](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744922.png)

|Server|RAM|Model|Procesor|Počet procesorů|NIC|Software|
|---|---|---|---|---|---|---|
|Pro Hyper-V servery na obrázku: <br />ESTLAB HOST11<br />ESTLAB HOST12<br />ESTLAB HOST13<br />ESTLAB HOST14<br />ESTLAB HOST25|128ESTLAB HOST25 má 256|Dell™ PowerEdge™ R820|Intel(R) Xeon(R) procesoru E5-4620 0 @ 2,20 GHz|4|Můžu s technologií x 4|Windows Server Datacentra 2012 R2 (x64) + role Hyper-V|
|VMM serveru|2|||2|1 GB/s|Windows Server databáze 2012 R2 (x 64) + VMM 2012 R2|

### <a name="secondary-recovery-site"></a>Sekundární (obnovení) webu

- Sekundární webu má šest uzel překlopení obrázku.

- Prostor úložiště pro uzel clusteru poskytuje společnost iSCSI SAN. Model – Hitachi HUS130.

![Specifikace primární hardwaru](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744923.png)

|Server|RAM|Model|Procesor|Počet procesorů|NIC|Software|
|---|---|---|---|---|---|---|
|Pro Hyper-V servery na obrázku: <br />ESTLAB HOST07<br />ESTLAB HOST08<br />ESTLAB HOST09<br />ESTLAB HOST10|96|Dell™ PowerEdge™ R720|Intel(R) Xeon(R) procesoru E5-2630 0 @ 2.30GHz|2|Můžu s technologií x 4|Windows Server Datacentra 2012 R2 (x64) + role Hyper-V|
|ESTLAB HOST17|128|Dell™ PowerEdge™ R820|Intel(R) Xeon(R) procesoru E5-4620 0 @ 2,20 GHz|4||Windows Server Datacentra 2012 R2 (x64) + role Hyper-V|
|ESTLAB HOST24|256|Dell™ PowerEdge™ R820|Intel(R) Xeon(R) procesoru E5-4620 0 @ 2,20 GHz|2||Windows Server Datacentra 2012 R2 (x64) + role Hyper-V|
|VMM serveru|2|||2|1 GB/s|Windows Server databáze 2012 R2 (x 64) + VMM 2012 R2|

### <a name="server-workloads"></a>Server úloh

- Pro účely testování jsme vybraných kontakty pracovního vytížení běžně používaných v podnikové zákazníka scénáře.

- Použijeme [IOMeter](http://www.iometer.org) s vlastnostmi pracovní zátěž shrnuté v tabulce simulace.

- Všechny profily IOMeter jsou nastavené zápisy náhodné bajtů tak, aby napodobily nejhorší psaní vzorce pro úloh.

|Pracovní zátěž|Velikost vstupu a výstupu (KB)|% Přístup|% Pro čtení|Zbývající vstupně-výstupních|Vzorek vstupu a výstupu|
|---|---|---|---|---|---|
|Souborovém serveru|48163264|60 % 20 %5 %5 10 %|80 80 % 80 % 80 % 80 %|88888|Náhodné 100 %|
|SQL Server (objem 1) SQL Server (objem 2)|864|100 % 100 %|70 %0 %|88|100 % random100 % sekvenční|
|Exchange|32|100 %|67 %|8|100 % náhodné|
|Workstation/VDI|464|66 % 34 %|95 70 %|11|Obě náhodné 100 %|
|Web souborový Server|4864|33 % 34 % 33|95 95 % 95 %|888|Náhodné 75 %|

### <a name="virtual-machine-configuration"></a>Konfigurace virtuálního počítače

- 470 virtuálních počítačích primární clusteru.

- Všechny virtuálních počítačích s diskem VHDX.

- Spuštění pracovního vytížení shrnuté v tabulce virtuálních počítačích. Všechny jsou vytvořené pomocí šablony VMM.

|Pracovní zátěž|# VMs|Minimální RAM (GB)|Maximální RAM (GB)|Velikost logické disku (GB) za OM|Maximální procesorů|
|---|---|---|---|---|---|
|SQL Server|51|1|4|167|10|
|Exchange Server|71|1|4|552|10|
|Souborovém serveru|50|1|2|552|22|
|VDI|149|.5|1|80|6|
|Webový server|149|.5|1|80|6|
|SOUČET|470|||96.83 TB|4108|

### <a name="azure-site-recovery-settings"></a>Azure nastavení obnovení webu

- Obnovení Azure webu nakonfigurovanou místním systémem místní ochrany

- VMM server má čtyři mračnech nakonfigurován, obsahující servery Hyper-V obrázku a jejich virtuálních počítačích.

|Primární VMM cloudu|Chráněný virtuálních počítačích v cloudu|Replikace četnosti|Další obnovení body|
|---|---|---|---|
|PrimaryCloudRpo15m|142|15 minut|Žádná|
|PrimaryCloudRpo30s|47|30 sekund|Žádná|
|PrimaryCloudRpo30sArp1|47|30 sekund|1|
|PrimaryCloudRpo5m|235|5 minut|Žádná|

### <a name="performance-metrics"></a>Měřítka

Následující tabulka souhrn měřítka a čítače, které byly měří se v nasazení.

|Metriky|Čítač|
|---|---|
|PROCESOR|\Processor(_Total)\% Procesor času|
|Dostupnou pamětí|\Memory\Available MB|
|PROCESORŮ|\PhysicalDisk (_Total) \Disk přenosy/sec|
|Čtení (procesorů) operace a s OM|\Hyper-V virtuální úložné zařízení (<VHD>) \Read operace/Sec|
|Zápis (procesorů) OM operace/sec|\Hyper-V virtuální úložné zařízení (<VHD>) \Write operace a S|
|Přečtěte si výkon OM|\Hyper-V virtuální úložné zařízení (<VHD>) \Read bajtů/sec|
|Výkon zápisu OM|\Hyper-V virtuální úložné zařízení (<VHD>) \Write bajtů/sec|


## <a name="next-steps"></a>Další kroky

- [Nastavení ochrany mezi dvěma místní VMM weby](site-recovery-vmm-to-vmm.md)
