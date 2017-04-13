<properties
    pageTitle="Akce: v případě, že přerušení služby Azure ovlivňuje Azure virtuálních počítačích | Microsoft Azure"
    description="Zjistěte, jak postupovat v případě, že přerušení služby Azure ovlivňuje Azure virtuálních počítačích."
    services="virtual-machines"
    documentationCenter=""
    authors="kmouss"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines"
    ms.workload="virtual-machines"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="kmouss;aglick"/>

#<a name="what-to-do-in-the-event-that-an-azure-service-disruption-impacts-azure-virtual-machines"></a>Akce: v případě, že přerušení služby Azure ovlivňuje Azure virtuálních počítačích

Společnost Microsoft pracujeme pevně a ujistěte se, že služeb společnosti Microsoft jsou vždy dostupné a když je potřebujete. Přinutí naše ovlivnit někdy vliv na to, nám způsoby, které způsobují přerušení poskytování služeb neplánované.

Microsoft poskytuje smlouva úrovni služeb (SLA) pro své služby jako potvrzení provozu a připojení. SLA pro jednotlivé služby Azure najdete v [Azure smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/).

Azure už má mnoho předdefinované platformy, které podporují vysoce dostupných aplikací. Další informace o těchto služeb přečtěte si [havárie využití a dostupnost pro Azure aplikace](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Tento článek popisuje obnovení scénáři platí havárie případě, že celé oblasti dojde výpadku kvůli hlavní přírodní katastrofě nebo přerušení poskytování služeb široké. Toto jsou méně častých výskytů, ale je nutné připravit možnost, že je výpadku celé oblasti. Pokud celá oblast dojde k přerušení služby, místně záložní kopie dat dočasně bude k dispozici. Pokud jste povolili geo replikace, tři dalších kopií objektů BLOB Azure úložiště a tabulek jsou uložené v jiné oblasti. V případě výpadku dokončení místní nebo selhání, ve kterém oblasti primární je znovu mapuje Azure, všechny položky DNS k oblasti replikovat geo.

>[AZURE.NOTE]Mějte na paměti, že nemáte žádné publikum nemůže ovládat tento proces a proběhne pouze při přerušení poskytování služeb celé oblasti. Z toho důvodu musí taky přes v jiných specifické pro aplikaci záložní strategie dosažení nejvyšší úrovně dostupnosti. Další informace naleznete v části na [strategie dat pro havárie obnovení](../resiliency/resiliency-disaster-recovery-azure-applications.md#data-strategies-for-disaster-recovery).

Můžete dělat tyto méně častých výskytů, nabízíme následující pokyny pro Azure virtuálních počítačích v případě přerušení služby celou oblast, kde je vaše aplikace Azure virtuálního počítače nasazena.

##<a name="option-1-wait-for-recovery"></a>Možnost 1: Počkejte obnovení
V tomto případě není nutná žádná akce na druhé straně. Víte, že pracujeme pečlivě obnovíte dostupnost služby. Zobrazí se aktuální stav služby na naše [Řídicí panel stavu služby Azure](https://azure.microsoft.com/status/).

>[AZURE.NOTE]Toto je nejlepší možností, pokud jste si obnovení webu Azure, zálohování virtuálního počítače, geo nadbytečné úložiště přístup pro čtení nebo geo nadbytečné úložiště před narušení nenastavili. Pokud jste nastavili geo nadbytečné ukládání nebo čtení geo nadbytečné úložiště účtu úložiště uloženými virtuální pevných discích OM (VHD), můžete hledat obnovit základní obrázek virtuálního pevného disku a zkuste zřízení nového OM z něho. Toto není upřednostňovaná možnost, protože neexistují žádné záruky synchronizace dat pokud nejsou použity Azure OM zálohování a obnovení webu Azure. Proto tato možnost není zaručena.

Zákazníkům, kteří mají okamžitý přístup k virtuálních počítačích jsou k dispozici následující dvě možnosti.  

>[AZURE.NOTE]Mějte na paměti, že z těchto možností mít možnost ztrátu dat.     

##<a name="option-2-restore-a-vm-from-a-backup"></a>Možnost 2: Obnovení ze zálohy virtuálního počítače
Pro zákazníky, kteří nakonfigurovali zálohy OM můžete obnovit OM od bodu zálohování a obnovení.

Obnovení ze zálohy Azure nové OM, najdete v článku [obnovení virtuálních počítačích v Azure](../backup/backup-azure-restore-vms.md).

Vám usnadní plánování infrastrukturu záložní Azure virtuálních počítačích, najdete v článku [plánování záložní infrastrukturu OM v Azure](../backup/backup-azure-vms-introduction.md).

##<a name="option-3-initiate-a-failover-by-using-azure-site-recovery"></a>Možnost 3: Zahájení přepojení pomocí obnovení webu Azure
Pokud jste nakonfigurovali obnovení webu Azure pro práci s dotčeném Azure virtuální počítače, můžete obnovit vaše VMs z jejich napodobeniny. Tyto můžete replikami na Azure nebo místní. V takovém případě můžete vytvořit nový OM z existující otevřené. Obnovení svého VMs z otevřené obnovení webu Azure, najdete v článku [migrace IaaS Azure virtuálních počítačích mezi Azure oblasti s obnovení webu Azure](../site-recovery/site-recovery-migrate-azure-to-azure.md).

>[AZURE.NOTE]I když Azure virtuálního počítače operační systém a disků dat bude replikovat na vedlejší virtuálního pevného disku, pokud jsou v geo nadbytečné ukládání nebo čtení geo nadbytečné úložiště účet, je každý virtuální pevný disk replikovat nezávisle na sobě. Tato úroveň replikace nezaručuje konzistence přes replikovanou VHD. Pokud aplikace nebo databáze, které používají discích dat jsou závislé na sobě, není zaručit, že jsou všechny VHD replikovat jako jeden snímek. Není také zaručit, že virtuální pevný disk otevřené z geo nadbytečné ukládání nebo čtení geo nadbytečné úložiště způsobí aplikace konzistentní snímek, který chcete spustit OM.

##<a name="next-steps"></a>Další kroky
Další informace o tom, jak implementovat havárie využití a dostupnost strategie najdete v tématu [obnovení havárie a dostupnost pro Azure aplikace](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Se dají podrobné technické informace o platformy cloudové možnosti, najdete v článku [příručku Azure odolnost proti chybám](../resiliency/resiliency-technical-guidance.md).

Informace o obecnějším údajům VMs najdete v tématu [obecnějším údajům Azure virtuálních počítačích](../backup/backup-azure-vms.md).

Naučte se organizovat a automatizovat ochrany vaší fyzické (a virtuální) Windows a Linux stroje, které VMWare a Hyper-V VMs najdete v článku [Obnovení webu Azure](https://azure.microsoft.com/documentation/learning-paths/site-recovery/)pomocí obnovení webu Azure.

Pokud pokyny jsou vymažte nebo libovolný text Microsoft provádět operace vaším jménem, kontaktujte [Zákaznickou podporu](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
