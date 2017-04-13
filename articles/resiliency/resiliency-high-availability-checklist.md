<properties
   pageTitle="Kontrolní seznam dostupnost | Microsoft Azure"
   description="Stručný kontrolní seznam nastavení a akce, které můžete provádět k zajištění jsou lepší dostupnost aplikací s Azure."
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

#<a name="high-availability-checklist"></a>Dostupnost kontrolního seznamu
Jedním ze velký výhod používání Azure je možnost zvětšit dostupnosti (a škálovatelnost) aplikace pomocí cloudu. Aby zkontrolovala, jestli že jsou optimální využití tyto možnosti, následující kontrolní seznam mysleli k zajištění pružné aplikace vám pomůže s některými základy infrastruktury klíče. 

>[AZURE.NOTE] Většina následující návrhy jsou věci, které lze provést kdykoli v aplikaci a proto jsou skvělé k "rychlé opravy". Nejlépe dlouhodobé řešení často zahrnuje aplikace návrhu, který je vytvořené pro cloudu.  Kontrolní seznam pro tyto (Další návrh určený plochy, přečtěte si našeho [kontrolního seznamu dostupnosti](../best-practices-availability-checklist.md).

###<a name="are-you-using-traffic-manager-in-front-of-your-resources"></a>Používáte před svých prostředcích přenosy správce?
Pomocí Správce přenosy pomáhá směrování internetových přenosů přes Azure oblastí nebo Azure a místních umístění. Akce pro spousta důvodů, včetně latence a dostupnosti. Chcete zjistit další informace o tom, jak pomocí Správce přenosy zvýšit vaše odolnost proti chybám a šířit přenosů do více oblastí, najdete v tématu [Spuštění VMs v několika datacentrech na Azure vysoké dostupnosti](../guidance/guidance-compute-multiple-datacenters.md).

__Co se stane, pokud nepoužíváte přenosy správce?__ Pokud nemáte správce přenosy před aplikace, můžete se omezuje jednu oblast pro zdroje. Toto omezení použít měřítko zvyšuje latence uživatelům, kteří nejsou Zavřít vybrané oblasti a sníží úroveň ochrany v případě přerušení služby celé oblasti.

###<a name="have-you-avoided-using-a-single-virtual-machine-for-any-role"></a>Máte můžete vyhnout se pomocí jednoho počítače virtuální pro každou rolí?
Dobrý návrh zabráněno libovolný jeden bod selhání. Co je důležité v návrhu všechny služby (místní nebo v cloudu), ale je užitečné v cloudu jako škálovatelnost a odolnost přes rozšiřování (přidání virtuálních počítačích) můžete zvětšit místo škálování (pomocí výkonnější virtuální počítač). Pokud chcete zjistit další informace o navrhování scalable aplikací, přečtěte si [dostupnost pro aplikace založený na Microsoft Azure](resiliency-high-availability-azure-applications.md).

__Co se stane, pokud máte jednom počítači virtuální pro role?__ Do jednoho počítače představuje jeden bod selhání a není k dispozici pro [Úroveň Azure virtuální počítač smlouvách](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/). V nejlepší případech aplikace se spustí pouze pořádku ale to není pružné návrhu, nevztahuje SLA Azure virtuálního počítače a jednoho místa zvětšuje selhání, kterou možnost prostoj když něco se nezdaří.

###<a name="are-you-using-a-load-balancer-in-front-of-your-applications-internet-facing-vms"></a>Používáte Vyrovnávání zatížení před VMs internetového aplikace?
Vyrovnávání zatížení umožňují rozšířit příchozích aplikaci mezi libovolný počet počítačích. Které můžete přidat nebo odebrat počítačích z vyrovnávání zatížení kdykoli, který pracuje s virtuálních počítačích (i s automatické měřítko sadami měřítko virtuální počítač) umožňuje snadno dělat zvýšení přenosy nebo selhání OM. Pokud chcete získat další informace o vyrovnávání zatížení, přečtěte si [Přehled Azure nástroj pro vyrovnávání zatížení](../load-balancer/load-balancer-overview.md) a [spouštění více virtuálních na Azure škálovatelnost a k dispozici](../guidance/guidance-compute-multi-vm.md).

__Co se stane, pokud nepoužíváte Vyrovnávání zatížení před internetového VMs?__ Bez vyrovnávání zatížení nebudete moct rozšiřování (přidat další virtuálních počítačích) a vaše jediná možnost bude škálování (zvětšení virtuálního počítače web vystavený). Pokud narazíte selhání s virtuální počítače v jednom místě. Bude taky potřebujete napsat kódu DNS a Všimněte si, pokud jste ztratili internetového počítač a znovu zmapovat položky DNS na novém počítači se začíná projevovat jeho proběhnout.

###<a name="are-you-using-availability-sets-for-your-stateless-application-and-web-servers"></a>Se, že používáte dostupnost nastaví příslušnosti serverů aplikace a web?
Vložení počítače do stejné vrstvy aplikace sady dostupnost zajišťuje vaše VMs nárok na SLA OM Azure. Součást dostupné taky nastavit zaručuje, že jsou počítače přepněte do různých aktualizace domény (tedy stroje jiného hostitele, které jsou opravené v různou) a poruchy domény (tedy hostitele stroje, které sdílejí power totiž a sítě zapnout). Aniž byste byli v sadě dostupnost, vaše VMs může být umístěny ve stejném počítači Host (hostitel) a tím může být ještě chyby, které nejsou viditelné pro vás v jednom místě. Pokud chcete zjistit další informace o prodloužení dostupnost vaší VMs pomocí sad dostupnost, přečtěte si prosím [Spravovat dostupnost virtuálních počítačích](../virtual-machines/virtual-machines-windows-manage-availability.md).

__Co se stane, pokud nepoužíváte dostupné sada se příslušnosti aplikace a webových serverů?__ Nepoužíváte dostupné nastavte znamená, že nejste využít SLA OM Azure. Také znamená to, že počítačích v dané vrstvě aplikace může všechny odejít offline, pokud dojde k aktualizaci na hostitelském počítači (počítač hostující VMs používáte) nebo běžné chyby hardwaru.

###<a name="are-you-using-virtual-machine-scale-sets-vmss-for-your-stateless-application-or-web-servers"></a>Používáte pro příslušnosti aplikace nebo webové servery sady měřítko virtuálního počítače (VMSS)?
Dobrý návrh scalable a pružné používá VMSS abyste měli jistotu, že je můžete zvětšovat a zmenšovat počtu počítačů ve vrstvě aplikace (třeba váš web vrstvy). VMSS umožňuje určit, jak vrstvě aplikace přizpůsobí (přidávání a odebírání servery na základě kritérií, jaká jste). Pokud chcete zjistit další informace o používání sad měřítko Virtual Machine Azure zvětšíte odolnosti na vrcholy pole přenosy, přečtěte si [Přehled sady měřítko virtuálního počítače](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

__Co se stane, když není nám nastavte měřítko virtuálního počítače s aplikace příslušnosti webového serveru?__ Bez VMSS omezit možnost zobrazit bez omezení a optimalizovat používání zdrojů. Návrh, který nemá VMSS má horní mez měřítka, které budou muset zachází s další kód (nebo ručně). Tento chybějící VMSS také znamená, že aplikace můžete není snadno přidat a odebrat počítačích (bez ohledu na měřítko) týkající se zpracování velké vrcholy pole přenosů (například při povýšení nebo v případě virové web nebo aplikaci/produkt).

###<a name="are-you-using-premium-storage-and-separate-storage-accounts-for-each-of-your-virtual-machines"></a>Používáte pro jednotlivá virtuálních počítačích premium úložiště a samostatné úložiště účtů?
Je nejvhodnější pro použití premium úložiště pro vaše výrobní virtuálních počítačích. Navíc nezapomeňte použít účet samostatný úložiště pro každou virtuálního počítače (to platí malých nasazení. Pro větší nasazení znovu použitelných účty úložiště u několika počítačů ale je vyrovnávání, který je třeba udělat zajistit vyvážení různé domény aktualizace a úrovní aplikace). Pokud chcete zjistit další informace o úložišti Azure výkon a škálovatelnost, přečtěte si [Microsoft Azure úložiště výkon a škálovatelnost kontrolního seznamu](../storage/storage-performance-checklist.md).

__Co se stane, pokud nepoužíváte účty samostatné úložiště pro každou virtuální počítač?__ Účet úložiště, jako je mnoho dalších zdrojů je selhání v jednom místě. Přestože mnoho ochrana a funkcí odolnost proti chybám Azure úložiště, selhání v jednom místě je nikdy dobrý návrh. Například pokud přístupových práv k poškození k účtu, přístupů limit úložiště, zda [procesorů omezit](../azure-subscription-service-limits.md#virtual-machine-disk-limits) dosažení, jsou ovlivněny všechny virtuálních počítačích pomocí tohoto účtu úložiště. Navíc při přerušení služby, které mají vliv razítko úložiště, které obsahuje tento účet určité úložiště může mít vliv na více virtuálních počítačích.

###<a name="are-you-using-a-load-balancer-or-a-queue-between-each-tier-of-your-application"></a>Používáte Vyrovnávání zatížení nebo fronty mezi jednotlivé vrstvy aplikace?
Použití vyrovnávání zatížení nebo fronty mezi jednotlivé vrstvy aplikace umožňuje snadno zobrazit jednotlivé vrstvy aplikace snadno a nezávisle na sobě. Byste měli zvolit mezi tyto technologie založený na vaší latence složitosti, a musí rozdělení (tedy jak často se distribuci aplikace). Obecně fronty mají za následek mít vyšší latence a přidáte složitosti, ale hodit právě více pružné a umožňuje distribuovat aplikace přes větší plochy (například různých oblastí). Pokud chcete zjistit další informace o používání vyrovnávání zatížení interní nebo fronty, přečtěte si prosím [interní služby Vyrovnávání zatížení – přehled](../load-balancer/load-balancer-internal-overview.md) a [fronty Azure a služby Bus fronty – porovnání a v porovnání s](../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md).

__Co se stane, pokud nepoužíváte Vyrovnávání zatížení nebo fronty mezi jednotlivé vrstvy aplikace?__ Bez vyrovnávání zatížení nebo fronty mezi jednotlivé vrstvy aplikace je těžké měřítko aplikace nahoru nebo dolů a distribuce jeho zatížení ve více počítačích. Není to může vést k nad nebo pod zřizování zdrojů a rizika prostoje nebo špatné uživatelské prostředí, pokud máte neočekávané změny selhání přenosy nebo systému.
 
###<a name="are-your-sql-databases-using-active-geo-replication"></a>Používáte SQL databáze aktivní geo replikace? 
Aktivní Geo replikace umožňují konfigurovat až 4 čitelné sekundární databáze ve stejném nebo jiném, oblastech. Sekundární databáze jsou dostupné v případě přerušení služby nebo připojení k databázi primární. Pokud chcete získat další informace o aktivní geo replikace databáze SQL, přečtěte si prosím [Přehled: SQL databáze aktivní Geo replikace](../sql-database/sql-database-geo-replication-overview.md).
 
 __Co se stane, pokud nepoužíváte aktivní geo replikace s SQL databáze?__ Bez aktivní geo replikace, pokud databázi primární někdy přejde do režimu offline (plánované údržby přerušení služby, selhání hardwaru, atd.) databáze aplikace budou v offline režimu, dokud přenést primární databázi zpět do stavu online v pořádku stavu. 
 
###<a name="are-you-using-a-cache-azure-redis-cache-in-front-of-your-databases"></a>Používáte před databáze mezipaměti (Azure Redis mezipaměť)?
Pokud aplikace zatížení vysoké databáze kde většina volání databáze je čtení, můžete zrychlit aplikace a zmenšení načíst na databázi implementací mezipaměti vrstvy před databáze pro převzít tyto operace čtení. Můžete urychlit aplikace a zmenšit zatížení vaší databáze (tedy zvětšení měřítka, které můžete dělat) tak, že zaškrtnete mezipaměti vrstvy před databáze. Pokud chcete další informace o mezipaměti Azure Redis, přečtěte si [pokyny pro ukládání do mezipaměti](../best-practices-caching.md).
 
 __Co se stane, pokud nepoužíváte mezipaměť před databázi?__ Pokud váš počítač databáze představuje dostatečně zpracovávání zatížení provozu přepnutím na něm aplikace odpoví jako normální, a pak kdyby to znamená, že na dolním zatížení si bude být platíte počítače v databázi, která je větší než potřebné. Pokud váš počítač databázi není výkonné tak, abyste zpracovávat vaše zatížení a pak se začíná projevovat nadměrná špatné uživatele zaznamenat s aplikací (latence časové limity a případně výpadek služeb).
 
###<a name="have-you-contacted-microsoft-azure-support-if-you-are-expecting-a-high-scale-event"></a>Pokud očekáváte událost nastavit jako vysoké měřítko mít kontaktovat podporu společnosti Microsoft Azure?
Azure podporu můžete zvětšit omezení služeb řešení události plánované vysoký provoz (třeba nové prezentace nových produktů nebo jinak svátky). Azure podpory taky můžou umožňují spojit s odborníky, kdo vám pomůžou Kontrola návrhu se svým týmem účtu a umožňují najdou nejlepším řešením vlastním potřebám události vysoké měřítko. Pokud chcete zjistit další informace o tom, jak kontaktovat podporu Azure, přečtěte si prosím [Azure podporují nejčastější dotazy týkající se](https://azure.microsoft.com/support/faq/).

__Co se stane, když není kontaktovat podporu Azure pro událost nastavit jako maximum měřítko?__ Pokud nevyberete komunikovat ani plánování, událost nastavit jako vysoké přenosy, můžete rizik zasažení určitých [Azure služeb omezuje](../azure-subscription-service-limits.md) a tedy vytvoření špatné uživatelské rozhraní se uživateli (nebo nejhorší, prostoje) během události. Architektonické recenze a sdělování před výkyvům mohou pomoci pomoci tato rizika zmírnit.

###<a name="are-you-using-a-content-delivery-network-azure-cdn-in-front-of-your-web-facing-storage-blobs-and-static-assets"></a>Používáte před web vystavený úložiště objektů BLOB a statické prostředky obsahu síť pro doručování (Azure CDN)?
Použití CDN pomůže vám s pořizováním zatížení vypnout serverech tak, že ukládání do mezipaměti obsahu do umístění CDN POP/okraje, které máte uložené ve světě. Můžete udělat toto tlačítko Zmenšit latence, zvětšit škálovatelnost, snížení vytížení serveru a jako součást strategie pro ochranu před útokům service(DOS). Pokud chcete zjistit další informace o tom, jak pomocí Azure CDN vaší odolnost proti chybám zvýšit a snížit latence zákazníka, přečtěte si [Přehled o obsahu doručení Network (CDN) Azure](../cdn/cdn-overview.md).

__Co se stane, pokud nepoužíváte CDN?__ Pokud nepoužíváte CDN pak všechny přenosy pro vaši zákazníka je přímo na zdroje. To znamená, že uvidíte vyšší na serverech, které snižuje jejich škálovatelnost. Kromě toho vaši zákazníci setkat vyšší čekacích dob CDN nabídne vám míst po celém světě, které pravděpodobně blízkosti zákazníkům.

##<a name="next-steps"></a>Další kroky:
Pokud chcete další informace o návrhu vaší žádosti o dostupnost, přečtěte si prosím [dostupnost pro aplikace založený na Microsoft Azure](resiliency-high-availability-azure-applications.md).
