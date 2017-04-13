<properties
   pageTitle="Obnovení pro Azure aplikací | Microsoft Azure"
   description="Technický přehled a podrobné informace o vytváření aplikací pro obnovení havárie na Microsoft Azure."
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

#<a name="disaster-recovery-for-applications-built-on-microsoft-azure"></a>Obnovení pro aplikace založený na Microsoft Azure

Vzhledem k tomu dostupnost je Správa dočasné selhání, havárie obnovení (DR) je o katastrofické ztrátě funkčnosti aplikace. Zvažte například scénář kde oblasti havaruje. V tomto případě je třeba plán pro spuštění aplikace nebo přístup ke svým datům mimo oblast Azure. Provádění tento plán zahrnuje lidi, procesy a podpůrné aplikace, které umožňují systému (funkce). Vlastníci business a technologické, kteří definování systému provozní režim pro selhání také určují úroveň funkce pro službu během selhání. Úroveň funkce může trvat několik formulářů: úplně není k dispozici, která je částečně dostupná (sníženou kvalitu funkce nebo zpoždění zpracování), nebo plně k dispozici.

##<a name="azure-disaster-recovery-features"></a>Funkce pro obnovení Azure havárie

Stejně jako u dostupnost aspektech Azure obsahuje [příručku odolnost proti chybám](./resiliency-technical-guidance.md) , pomocí které podporuje havárie obnovení. Je taky souvislost mezi některé z funkcí dostupnost Azure a havárie obnovení. Správa rolí v poruch doménách například zvyšuje dostupnost aplikace. Bez správy nezdařené neošetřené hardwaru stane scénáři "katastrofě". Tak, aby správné použití dostupnost funkcí a strategie důležitou součástí kontroly pravopisu havárie aplikace. Však v tomto článku přesahuje všeobecně dostupná problémy na více vážně (vzácnějších) havárie události.

##<a name="multiple-datacenter-regions"></a>Více oblastí datacentru

Azure udržuje datacentrech v mnoha oblastech ve světě. Tato infrastruktura podporuje několik scénářů obnovení havárie, například systém-za předpokladu, že geo replikace Azure úložiště u sekundárních oblastí. Také znamená to, že můžete snadno a efektivně nasadit do cloudové služby více míst po celém světě. Porovnejte toto s náklady a potíže při spuštění vlastní datacentrech ve více oblastech. Nasazení dat a služeb do více oblastí pomáhá chránit vaše aplikace z hlavní výpadků v jedné oblasti.

##<a name="azure-traffic-manager"></a>Azure přenosy správce

Pokud dojde k selhání příslušnou oblast, musí přesměrujete přenosy na nasazení v jiné oblasti nebo služeb. Můžete udělat toto směrování ručně, ale je výhodnější pomocí automatického procesu. Azure přenosy Manager je určený pro tento úkol. Můžete ho pro automatickou správu převzetí uživatele umožnění datových přenosů do jiné oblasti v případě, že oblasti primárního se nezdaří. Protože je Správa přenosy důležitou součástí celkové strategie, je důležité seznámení se základy správce přenosy.

V následujícím diagramu uživatelé připojit k adresu URL, která určil pro přenos správce (__http://myATMURL.trafficmanager.net__) a že přehledů skutečné adresy URL (__http://app1URL.cloudapp.net__ a __http://app2URL.cloudapp.net__). Podle jak konfigurovat kritéria pro směrování uživatele, uživatelé odešle správné skutečné web při Určuje zásadu. Možnosti zásad jsou kruhového, výkon nebo překlopení. Z tohoto článku budeme starali o jenom možnost překlopení.

![Směrování přes Azure přenosy správce](./media/resiliency-disaster-recovery-azure-applications/routing-using-azure-traffic-manager.png)

Při konfiguraci přenosy správce, zadejte novou předponu přenosy Správce DNS. Toto je předpona adresy URL, které budete zadat uživatelů pro přístup ke službě. Přenosy správce teď abstrahuje o jednu úroveň nahoru a ne na úrovni oblasti vyrovnávání zatížení. Přenosy Správce DNS mapuje záznam CNAME pro všechny nasazení, která spravuje.

Ve Správci přenosy zadáte priority nasazení, které uživatelé směrovány, když dojde k chybě. Přenosy správce sleduje koncové body nasazení a poznámky Pokud dochází k selhání primárního nasazení. Chyba přenosy správce analyzuje uspořádaný seznam nasazení a přesměrovává uživatelů na další stránku v seznamu.

I když přenosy správce rozhoduje o kam se obrátit v selhání, můžete se rozhodnout, jestli je vaše doména převzetí neaktivní nebo aktivní, když nejste v režimu překlopení. Tuto funkci nesouvisí s Azure přenosy správce. Přenosy správce zjistí selhání primární webu a vrátí web překlopení. Přenosy správce vrátí bez ohledu na to, zda tomuto webu aktuálně obsluhuje uživatele nebo ne.

Další informace o fungování Azure přenosy správce označovat:

 * [Přehled přenosy správce](../traffic-manager/traffic-manager-overview.md)
 * [Konfigurace metodu směrování překlopení](../traffic-manager/traffic-manager-configure-failover-routing-method.md)

##<a name="azure-disaster-scenarios"></a>Azure havárie scénáře

V následujících částech najdete několik různých typů havárie scénáře. Přerušení poskytování služeb celé oblasti nejsou pouze příčinou selhání celou aplikaci. Špatné návrh nebo Správa chyb také může vést k výpadků. Je důležité zvážit možných příčin selhání při návrhu a zkušební fáze plánu obnovení. Dobrý plán využívá Azure funkce a argumentech s strategie specifické pro aplikaci. Odpověď na vybraný dáno důležitost aplikací, cíle bodu obnovení (operace RPO) a cíle časového využití (RTO).

###<a name="application-failure"></a>Chyba aplikace

Azure přenosy správce automaticky zpracovává chyb, které vyplynou ze zdrojových hardware nebo operační systém software na hostitelském počítači virtuální. Azure vytvoří novou instanci role funkční serveru a přidá ji na požadované otočení Vyrovnávání zatížení. Pokud instancí role je číslo větší než 1, Azure směn zpracování jiných spuštěné instance rolí při nahrazení uzel.

Došlo k chybám vážně aplikace, které se provedou nezávisle na hardware nebo operační systém chyby. Aplikace může selhat kvůli katastrofické výjimky způsobená chybná logiky nebo otázek integrity dat. Dost telemetrie musí začlenit do kód tak, aby systém sledování můžete zjistit selhání podmínky a upozornit správce aplikace. Správce, který má znalostí obnovení procesů havárie můžete rozhodnout pro vyvolání překlopení obrázku. Můžete taky správce jednoduše jsou přijatelné dostupnost výpadku kritické chyby vyřešit.

###<a name="data-corruption"></a>Poškození dat

Azure automaticky ukládá dat Azure úložiště a databáze SQL Azure třikrát redundantně v rámci různých poruch domén ve stejné oblasti. Pokud používáte geo replikace, uložení dat další třikrát v jiné oblasti. Ale pokud vaši uživatelé nebo aplikace poškodí tato data do pole primární kopírovat, data rychle zreplikuje na ostatní kopie. Bohužel to výsledkem tři kopie poškozená data.

Správa potenciálních poškození dat, máte dvě možnosti. Nejdřív můžete spravovat vlastní strategie zálohování. Zálohování mohou být uloženy v Azure nebo místní, podle toho, obchodní požadavky nebo právní zásady správného řízení. Další možností je nová možnost obnovit v okamžiku použít pro obnovení databáze SQL. Další informace naleznete v části na [strategie dat pro havárie obnovení](#data-strategies-for-disaster-recovery).

###<a name="network-outage"></a>Výpadku sítě

Jakmile částech Azure sítě nedostupné, nemusí být dostali na váš aplikace nebo data. Pokud jeden nebo víc instancí role nejsou dostupné kvůli problémům se sítí, použije Azure zbývající dostupných instancí aplikace. Pokud aplikace přístup z důvodu výpadku Azure sítě její data, můžete potenciálně spustit v režimu snížené kvality místně pomocí data uložená v mezipaměti. Potřebujete architektonické havárie obnovení strategie pro práci v režimu snížené kvality v aplikaci. Pro některé aplikace nemusí být praktické.

Další možností je k ukládání dat do jiného umístění, dokud se obnoví připojení. Nejsou-li jejich funkčnost bude omezena režimu jednu z možností, jsou zbývající možnosti prostoje aplikace nebo převzetí alternativní oblasti. Návrh aplikace spuštěné v režimu snížené kvality je tolik obchodní rozhodnutí jako některou technické. To je popsáno dále v části týkající se [funkcí aplikace sníženou kvalitu](#degraded-application-functionality).

###<a name="failure-of-a-dependent-service"></a>Chyba při závislá služby

Azure poskytuje mnoho služeb, které můžete vyzkoušet pravidelných prostoje. Zvažte [Azure Redis mezipaměti](https://azure.microsoft.com/services/cache/) jako příklad. Tuto službu více klienta poskytuje možnosti ukládání do aplikace. Je důležité zvážit, co se stane v aplikaci, když závislá služba není k dispozici. Tento scénář je podobně jako v síti výpadku situaci různými způsoby. Však vzhledem k tomu každé služby nezávisle na sobě má za následek potenciální vylepšení plánu celková.

Azure Redis mezipaměť poskytuje ukládání do mezipaměti aplikace z vaše cloudové služby nasazení, která výhody obnovení havárie. Nejdřív službu teď běží na role, které jsou místní nasazení. Proto se vám lépe povede sledovat a správě stavu mezipaměti jako součást celkové procesů správy ke cloudové službě. Tento typ mezipaměti také poskytuje nových funkcí. Nové funkce reprodukujte dostupnost pro data uložená v mezipaměti. Díky selhání načítání udržovat duplicitní kopie na uzlech uchovávat data uložená v mezipaměti.

Všimněte si, že dostupnost snižuje výkon a zvyšuje latence kvůli aktualizace sekundární kopie o zápisů. Také zdvojnásobí velikosti paměti, který se používá pro každou položku, takže plánování pro tento. Tento konkrétní příklad ukazuje, že každé závislá služby může obsahovat funkcí, které zlepšit celkový dostupnost a odolnost proti chybám katastrofické.

U jednotlivých závislá služeb je třeba porozumět důsledky přerušení služby. V mezipaměti příkladu je možné přístupu k datům přímo z databáze, dokud obnovte mezipaměť. Bude jejich funkčnost bude omezena režimu z hlediska výkon, ale umožní plnou funkčnost pokud jde o data.

###<a name="region-wide-service-disruption"></a>Přerušení služby oblast

Předchozí selhání primárně byly chyb, které se dá ovládat ve stejné oblasti Azure. Musíte však také připravit možnost, že je přerušení služby celé oblasti. Pokud dojde k přerušení služby oblast, místně záložní kopie dat nejsou k dispozici. Pokud jste povolili geo replikace, existují tři dalších kopií objektů BLOB a tabulek v jiné oblasti. Pokud Microsoft deklaruje oblasti ztratili, znovu mapuje Azure všechny položky DNS k oblasti replikovat geo.

>[AZURE.NOTE] Mějte na paměti, že nemáte žádné publikum nemůže ovládat tento proces a se projeví pouze u přerušení služby oblast. Z toho důvodu musí přes v jiných specifické pro aplikaci záložní strategie dosažení nejvyšší úrovně dostupnosti. Další informace naleznete v části na [strategie dat pro havárie obnovení](#data-strategies-for-disaster-recovery).

###<a name="azure-wide-service-disruption"></a>Přerušení služby Azure

Při plánování havárie, je nutné zvážit celou škálu možné havárie. Všechny Azure regiony jednu z nejčastěji špatných přerušení poskytování služeb by zahrnoval současně. Stejně jako u jiných přerušení poskytování služeb se můžete rozhodnout, že je budete riskovat dočasné prostoje v tomto případě. Přerušení poskytování rozšířených služeb, které zahrnují oblastí by měl být mnohem vzácnějších než přerušení poskytování izolace služeb, které zahrnují závislé služby a jedné oblasti.

Však pro některé důležité aplikace se můžete rozhodnout, že musí být plán zálohování taky v tomto scénáři. Plánování události můžou obsahovat nedaří myši ke službám [alternativní cloudu](#alternative-cloud) nebo [hybridní místních i cloudových řešení](#hybrid-on-premises-and-cloud-solution).

###<a name="degraded-application-functionality"></a>Funkce jejich funkčnost bude omezena aplikace

Dobře navržená aplikace obvykle používá kolekce moduly, které ale provádění volně vázaný výměnu informací vzorků vzájemně komunikovat. Aplikace DR ovládáním vyžaduje oddělení úkolů na úrovni modulu. Toto je zabráníte jejich přenesením dolů celou aplikaci přerušení služby závislé. Třeba vzít v úvahu webovou aplikaci commerce společnosti y. Následující moduly může představují aplikace:

 * __Katalogu produktů__ umožňuje uživatelům umožňuje přecházet produkty.
 * Umožňuje přidat nebo odebrat produkty v jejich nákupní košík __Nákupní košík__ .
 * __Stav objednávky__ se zobrazí stav dodávky objednávek uživatele.
 * __Odeslání pořadí__ dokončí nákupní relace odesláním pořadí kterém výše splátky.
 * __Zpracování objednávek__ ověří pořadí integrity dat a provede kontrolu dostupnosti množství.

Když závislé modulu v této aplikaci přejde, jak modulu fungovat, dokud obnoví tuhle část? Dobře architekturu systém používá izolace hranice prostřednictvím oddělování úkolů v době návrhu a při spuštění. Zařadit do kategorií každé selhání i obnovitelné-obnovitelné. Bez obnovitelné chyby bude trvat dolů na modul, ale zmírnit Zotavitelná chyba prostřednictvím alternativy. Jak je popsáno v části Dostupnost vysoká, můžete skrýt některé problémy před uživateli zpracování chyb a alternativní akcí. Během více vážně přerušení služby může být aplikace úplně není k dispozici. Třetí možnost je ale pokračovat v obsluze uživatele v režimu jejich funkčnost bude omezena.

Například pokud databázi pro hostování objednávky přejde, modulu zpracování objednávek selhává zpracuje prodejní transakce. V závislosti na architekturu může to být pevné nebo úplně nesrozumitelný pro odeslání pořadí a zpracování objednávek část aplikaci pokračovat. Pokud aplikace neslouží k zpracovat tento scénář, může přechodu do offline režimu celé aplikace.

V tomto scénáři stejné je možné, uložení dat produktu do jiného umístění. V takovém případě modulu katalogu produktů pořád slouží k zobrazení produktů. V režimu snížené kvality aplikace nadále k dispozici uživatelům pro dostupné funkce jako zobrazení katalogu produktů. Jiné části aplikace, ale nejsou k dispozici, třeba řazení nebo zásob dotazů.

Jiné variant Center jejich funkčnost bude omezena režimu na výkon spíše než možnosti. Zvažte například situace kde katalogu produktů je do mezipaměti prostřednictvím Azure Redis mezipaměti. Pokud ukládání do mezipaměti nebude k dispozici, může aplikaci přejděte přímo k základnímu úložišti serveru načítat informace o produktu katalogu. Ale může být tento přístup k nižší než verze uložená v mezipaměti. Z toho důvodu aplikace je výkon až do mezipaměti služby plně obnovení.

Rozhodnutí, kolik aplikace, zůstanou v režimu snížené kvality jsou obchodní rozhodnutí a technické rozhodnutí. Aplikace musí taky rozhodnout, jak informovat uživatele o dočasné potíže. V tomto příkladu aplikace může způsobit zobrazení produktech a ještě jejich přidávání nastavili k nákupní košík. Ale když se uživatelé pokusí nákupu, aplikace upozorní uživatele modulu Prodej je dočasně nedostupný. Není ideální pro zákazníka, ale kvůli narušení celou aplikaci služby.

##<a name="data-strategies-for-disaster-recovery"></a>Strategie dat pro havárie obnovení

Zpracování dat správně je oblasti nejtěžší a nastavení správného v libovolné havárie obnovení plánu. Obnovení dat je také součástí obnovení proces, který má obvykle nejvíce času. Různé možnosti v režimu snížení úrovně za následek složité problémy při obnovení dat z selhání a konzistence po selhání.

Jednou z faktory je potřeba obnovit nebo udržovat kopii dat aplikace. Tato data bude používat pro odkaz a transakční účely na vedlejší webu. Místního nastavení vyžaduje drahé a dlouhé plánování si vypište implementovat strategii obnovení havárie více oblastí. Většina poskytovatelů cloudu, včetně Azure, pohodlně, umožňují snadno nasazení aplikace do více oblastí. Tyto regiony jsou zeměpisně distribuovaná tak, že přerušení služby více oblastí by měl být velmi málo. Strategie pro zpracování dat v oblasti je jedním z příspěvku faktorů praktické nějaký havárie obnovení plán.

Následující části se zabývají technik pro obnovení havárie související s zálohování dat, referenčních dat a transakční data.

###<a name="backup-and-restore"></a>Zálohování a obnovení

Pravidelného zálohování dat aplikace podporuje některé scénáře obnovení havárie. Různé úložiště prostředky vyžadují různých metod.

Pro vrstvách Basic, standardních a Premium SQL databáze můžete využít v okamžiku obnovení obnovit databázi. Další informace najdete v tématu [Přehled: cloudu firmy kontinuitu databáze havárie obnovení a s databáze SQL](../sql-database/sql-database-business-continuity.md). Další možností je pro účely aktivní Geo replikace databáze SQL. To automaticky zreplikuje změny databáze na vedlejší databází ve stejné oblasti Azure a dokonce i do jiné oblasti Azure. To poskytuje potenciální alternativu k některé z více ruční synchronizace postupů dat prezentovány v tomto článku. Další informace najdete v tématu [Přehled: SQL databáze aktivní Geo replikace](../sql-database/sql-database-geo-replication-overview.md).

Můžete taky použít více ruční přístup pro zálohování a obnovení. Pomocí příkazu kopie databáze vytvořit kopii databáze. Tímto příkazem musí získání zálohy s konzistence. Můžete taky pomocí služby import nebo export databáze SQL Azure. Tato možnost podporuje exportovat databází BACPAC souborů, které jsou uložené v úložišti objektů Blob Azure.

Předdefinované redundance úložišti Azure vytvoří dva kopie záložní soubor ve stejné oblasti. Však četnost záložní proces určí, jakým vaše operace RPO, tedy množství dat, která se může dojít ke ztrátě v havárie scénáře. Představte si, například, že vytvořením zálohy v horní části hodinu a selhání dojde k dvě minuty před začátek hodinu. Přijdete o 58 minutách data, která se stalo po provedení posledního zálohování. Navíc a chráněny před přerušení služby oblast, by měl zkopírujete BACPAC soubory alternativní oblasti. Můžete pak tyto obnovení tyto zálohy alternativní oblasti. Další informace najdete v tématu [Přehled: cloudu firmy kontinuitu databáze havárie obnovení a s databáze SQL](../sql-database/sql-database-business-continuity.md).

U Azure úložiště můžete vyvinout vlastní procesu záložní nebo použijte jeden z mnoha nástroje Zálohování třetích stran. Všimněte si, že většina návrhů aplikace obsahuje další složitostí kde prostředků úložiště odkazují na sobě. Zvažte například SQL databázi, která obsahuje sloupec propojující objektů blob v úložišti Azure. Pokud zálohy z toho důvodu současně, můžou databáze se ukazatel myši na objekt blob nebyl zálohovala před selháním. Aplikace nebo havárie obnovení plán musí implementovat procesy zpracovávání této nekonzistence po obnovení.

###<a name="reference-data-pattern-for-disaster-recovery"></a>Přehled dat vzor pro havárie obnovení

Přehled dat je jen pro čtení data, která podporuje funkci aplikace. Obvykle nemění často. Zálohování a obnovení je sice jeden způsob zpracování přerušení poskytování služeb celé oblasti, je RTO relativně dlouhých. Při nasazení aplikace do vedlejší oblasti některé strategie můžete vylepšit RTO pro referenční data.

Protože odkazovat změny dat málo, můžete zlepšit RTO zachování trvalou kopii referenčních dat v oblasti sekundární. Díky dobu potřebnou k obnovení záložní kopie v případě selhání. Požadavkům na více oblastí havárie obnovení, je nutné nasadit aplikaci a referenčních dat společně ve více oblastech. Jak je uvedeno v [odkaz dat vzor pro dostupnost](./resiliency-high-availability-azure-applications.md#reference-data-pattern-for-high-availability), nástroje můžete nasazovat referenčních dat k roli sebe sama externí úložiště, případně kombinaci obou.

Odkaz datového nasazení modelu ve výpočetním uzly implicitně vyhovuje havárie obnovení. Nasazení dat Reference k SQL databázi vyžaduje nasazení kopii referenčních dat pro každou oblast. Stejné strategie se vztahuje k základnímu úložišti Azure. Je třeba nasadit kopii jakékoli referenčních dat uložených v úložišti Azure na primárního a sekundárního oblasti.

![Referenční publikace dat primárních a sekundárních oblastí](./media/resiliency-disaster-recovery-azure-applications/reference-data-publication-to-both-primary-and-secondary-regions.png)

Je nutné implementovat vlastní záložní rutiny specifická pro všechna data, včetně referenčních dat. Replikovat GEO kopií různých oblastí se používá pouze na přerušení služby celé oblasti. Nechcete, aby prodloužit dobu, nasazení důležitých díly dat aplikace na vedlejší oblast. Příklad Tato topologie naleznete v tématu [aktivní pasivní modelu](#active-passive).

###<a name="transactional-data-pattern-for-disaster-recovery"></a>Transakční data vzor pro havárie obnovení

Provádění strategie režimu plně funkční havárie vyžaduje asynchronní replikace transakční data, která chcete sekundární oblast. Čas windows, ve kterých může dojít replikace určí vlastnosti operace RPO rohu aplikace. Stále může obnovit data, která byla ztratí z oblasti primární během replikace okna. Také je možné pro sloučení s vedlejší oblasti později.

Následující příklady architektura na různé způsoby zpracování transakční dat ve scénáři převzetí zadejte některé z těchto možností. Je důležité mít na paměti, že tyto příklady není vyčerpávající. Přechodné úložišť, jako je fronty může například nahrazen příkazem databáze SQL Azure. Samotné fronty může být úložišti Azure nebo Bus služby Azure fronty (viz [Azure fronty a služby Bus fronty – porovnání a v porovnání s](../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)). Server úložiště cíle také lišit, jako je místo databáze SQL Azure tabulek. Kromě toho kolegy role může být vložena jako zprostředkovatelů v různé kroky. Důležité je přesně emulovat tyto architektury, ale zvažte různé možnosti obnovení transakční údajů a související moduly.

####<a name="replication-of-transactional-data-in-preparation-for-disaster-recovery"></a>Replikace transakční dat při přípravě havárie obnovení

Zvažte aplikaci, která používá fronty Azure úložiště k ukládání transakční data. Díky pracovníka role zpracuje transakční dat do databáze serveru oddělené architektury. Při této akci musí transakce použít některý z dočasné do mezipaměti, pokud front-end role vyžaduje okamžitou dotazu tato data. V závislosti na úrovni odchylky ztrátou dat můžete replikovat frontách databáze nebo všechny zdroje úložiště. S pouze replikace databáze Pokud havaruje oblasti primárního stále můžete obnovit data ve frontách případě oblasti primárního vracejí.

Na následujícím obrázku vidíte architektura, kde se k databázi serveru synchronizuje v oblasti.

![Replikace transakční dat při přípravě havárie obnovení](./media/resiliency-disaster-recovery-azure-applications/replicate-transactional-data-in-preparation-for-disaster-recovery.png)

Největší výzvou k provádění tohoto architektura je strategie replikace mezi oblastmi. Synchronizace dat SQL Azure služba umožňuje tohoto typu replikační. Služba však je stále ve verzi preview a se nedoporučuje provozním prostředí. Další informace najdete v tématu [Přehled: cloudu firmy kontinuitu databáze havárie obnovení a s databáze SQL](../sql-database/sql-database-business-continuity.md). Pro výrobní aplikace musíte investovat v řešení třetích stran nebo vytvořte logiky replikace v kódu. V závislosti architektura replikace pravděpodobně obousměrný, která je také složitější.

Jiný, který může být potenciální implementaci pomocí intermediate fronty v předchozím příkladu. Role pracovníka, která zpracuje data do cíle konečný úložiště může proveďte změny v oblasti primárního a sekundárního oblasti. Nejsou důvodu úkoly a úplné pokyny pro replikace kód je nad rámec tohoto článku. Důležité, hodně času a testování by měly zaměřit se na jak replikovat dat do oblasti sekundární. Další zpracování a testování lze zajistit převzetí a obnovení procesů správně zacházet nekonzistence dat nebo duplicitní transakce.

>[AZURE.NOTE] Většina tohoto dokumentu se zaměřuje na platformě jako služba (PaaS). Další možnosti replikace a dostupnost pro hybridní aplikace však použít Azure virtuálních počítačích. Tyto aplikace hybridní hostovat SQL Server ve virtuálních počítačích v Azure pomocí infrastruktury jako služba (IaaS). Díky tradiční dostupnost postupů na serveru SQL Server, například skupiny dostupnosti AlwaysOn nebo protokolu. Některé postupy, například AlwaysOn, práce pouze mezi instancemi místního serveru SQL Server a Azure virtuálních počítačích. Další informace najdete v tématu [dostupnost a obnovení pro systém SQL Server ve virtuálních počítačích Azure](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

####<a name="degraded-application-mode-for-transaction-capture"></a>Režim jejich funkčnost bude omezena aplikace pro zachycení transakce

Zvažte druhý architektura, který pracuje v režimu jejich funkčnost bude omezena. Aplikace na vedlejší oblasti deaktivuje všechny funkce, například vytváření sestav funkce business intelligence (BI), nebo vyprázdnění fronty. Podle definice požadavků firmy přijímá jenom nejdůležitější typy transakční pracovní postupy. Systém zaznamenává transakce a zapíše do fronty. Systém může odložit zpracování dat během počáteční fáze přerušení služby. Pokud systému do oblasti primární aktivován v okně očekávaná doba, pracovní role v oblasti hlavní vyprázdnit frontách. Tento proces nemusí sloučení databáze. Přerušení služby primární oblast přesahuje přípustné okna, můžete aplikaci spustit zpracování frontách.

V tomto scénáři obsahuje databáze na sekundární přírůstková transakční data, která je nutno sloučit po aktivaci primární. Na následujícím obrázku vidíte této strategie pro dočasně ukládání transakční dat, dokud se obnoví primárního oblast.

![Režim jejich funkčnost bude omezena aplikace pro zachycení transakce](./media/resiliency-disaster-recovery-azure-applications/degraded-application-mode-for-transaction-capture.png)

Další informace o technik správy dat pro pružné Azure aplikací najdete v článku [bezporuchový: pokyny pro pružné architektury cloudu](https://channel9.msdn.com/Series/FailSafe).

##<a name="deployment-topologies-for-disaster-recovery"></a>Topologie nasazení havárie obnovení

Je třeba připravit kritické aplikace možnost přerušení služby celé oblasti. To uděláte zahrnutím do provozní plánování strategie více oblastí nasazení.

Procesů IT pro k publikování dat aplikace a funkce pro odkazy na vedlejší oblasti po selhání může zahrnovat více oblastí nasazení. Pokud aplikace vyžaduje rychlé překlopení, může zahrnovat procesu nasazení aktivní/pasivní instalaci nebo nastavování aktivní/aktivní. Tento typ nasazení má existující instance aplikace spuštěné v oblasti alternativní. Směrování nástroj, například Azure přenosy správce poskytuje Vyrovnávání zatížení služby na úrovni DNS. Může rozpoznat přerušení poskytování služeb nebo směrovat uživatele na různé regiony a v případě potřeby.

Část obnovení úspěšné Azure havárie je architecting této obnovení do řešení od začátku. Shluk poskytuje další možnosti obnovení z selhání selhání, které nejsou k dispozici v tradiční poskytovatele hostingu. Konkrétně dynamicky a rychle zdroje můžete přidělit do jiné oblasti. Proto nebude platíte mnohem nedělá zdrojů během čekání chyby problémy.

V následujících částech vysvětlovat topologie různých nasazení havárie obnovení. Obvykle obsahuje kompromis vyšší náklady nebo složitost další dostupnosti.

###<a name="single-region-deployment"></a>Nasazení jedné oblasti

Nasazení jedné oblasti se skutečně topologie obnovení havárie, ale je určen pro kontrast s jiným architekturám plochy. Nasazení jedné oblasti jsou společná pro aplikace v Azure. Tento typ nasazení však není vážně uchazeč o havárie obnovení plán.

Následující obrázek znázorňuje aplikace spuštěné v jedné oblasti Azure. Azure přenosy správce a používání poruch a upgrade domén zlepšit dostupnost aplikace v oblasti.

![Nasazení jedné oblasti](./media/resiliency-disaster-recovery-azure-applications/single-region-deployment.png)

Tady je zřejmé, zda je databáze selhání v jednom místě. I když je Azure zreplikuje data v různých poruch doménách interní replikám, to vše nachází ve stejné oblasti. Aplikace nemůže proběhnout Katastrofální selhání. Pokud oblast havaruje, všechny poruch domény přejděte dolů – včetně všech instancí služby a prostředky úložiště.

U všech nejméně důležité aplikací musíte navrhnout plán nasazení aplikace v rámci více oblastí. Měli byste taky zvažte RTO a nákladů omezení vzhledem k tomu, jaký nasazení topologie používat.

Podívejme se nyní na konkrétní vzorků pro podporu převzetí mezi různými oblastmi. Tyto příklady všechny pomocí dvou oblastí popište procesu.

###<a name="redeployment-to-a-secondary-azure-region"></a>Nové nasazení do vedlejší Azure oblasti

Ve vzorku nového nasazení do vedlejší oblasti jenom primární oblast má aplikace a spuštění databáze. Sekundární oblast není nastavená pro automatické převzetí. Takže když dojde k selhání, můžete musí číselník nahoru všechny části služby v oblasti nová. Jedná se o ukládání do cloudové služby Azure, nasazení cloudové služby, obnovení dat a změnou DNS přesměrovat komunikaci.

I když je většina dostupnou možností více oblastí, má nejhorší RTO vlastnosti. V tomto modelu jsou uloženy služby balíčku a databáze zálohování buď místně nebo v případě úložiště objektů Blob Azure sekundární oblasti. Musí však nasaďte novou službu a obnovit data před obnoví operace. I když plně automatizovat přenosu data ze záložní úložiště otáčející nové prostředí databáze spotřebovává spoustu času. Přesunutí dat ze záložní disk úložiště pro prázdnou databázi na vedlejší oblasti je většina drahé součást procesu obnovit. Je nutné, ale můžete získat nové databáze provozní stavu, protože není replikovat.

Nejlepším řešením je ukládat balíčků služeb v úložišti objektů Blob v oblasti sekundární. Nemusí k nahrání balíčku na Azure, což je, co se stane, když nasadíte z počítače vývoj místní. Rychle nasazením balíčků služeb do nového cloudové služby z úložiště objektů Blob pomocí skriptů Powershellu.

Tato možnost je vhodná pouze pro nekritické aplikace, které nevadí vám vysoké RTO. Například to vašem případě mohlo fungovat aplikace, může být dolů několik hodin, která by měla běžet znovu 24 hodin.

![Nové nasazení do vedlejší Azure oblasti](./media/resiliency-disaster-recovery-azure-applications/redeploy-to-a-secondary-azure-region.png)

###<a name="active-passive"></a>Aktivní pasivní

Aktivní pasivní vzorku je možnost kompresi mnoho společností. Tento způsob poskytuje vylepšení RTO s relativně malým zvýšení nákladů přes vzorek nového nasazení.
V tomto scénáři je znovu primární a sekundární Azure oblasti. Všechny přenosy přejde do aktivní nasazení primárního oblast. Sekundární oblast je lepší připravené havárie obnovení, protože databázi běží na obou oblastí. Kromě toho je mechanismus synchronizace na místě mezi nimi. Tento úsporném přístup, bude to vyžadovat dvě varianty: přístup jenom pro databáze nebo dokončeno nasazení sekundární oblasti.

####<a name="database-only"></a>Pouze databáze

V první vzorku aktivní pasivní jenom primární oblast má aplikace nasazeném cloudové služby. Na rozdíl od nového nasazení vzorku, ale obě oblasti jsou synchronizovány se obsah databáze. (Další informace naleznete v části na [transakční data vzor pro obnovení havárie](#transactional-data-pattern-for-disaster-recovery).) Pokud dojde k selhání, je potřeba méně aktivace věcí. Spuštění aplikace v oblasti sekundární, změňte připojovací řetězec do nové databáze a změnit položky DNS přesměrovat komunikaci.

Stejně jako nové nasazení vzorek by měl jsou již uloženy balíčků služeb v úložišti objektů Blob Azure v oblasti sekundární rychlejší nasazení. Na rozdíl od vzorek nového nasazení není vzniknou většinou režijních, které si žádá obnovení databáze. Databázi je připraven a spuštěna. Tím se uloží značné množství času, díky to dostupnou DR vzorku. Je taky nejoblíbenější DR vzorku.

![Aktivní – pasivní, platí pouze pro databázi](./media/resiliency-disaster-recovery-azure-applications/active-passive-database-only.png)

####<a name="full-replica"></a>Úplné otevřené

V druhá vzorku aktivní pasivní oblasti primárního a sekundárního oblasti mají úplnou nasazení. Toto nasazení i cloudových služeb synchronizované databáze. Pouze primární oblast však aktivně zpracovává síťové požadavky uživatelům. Sekundární oblast aktivuje, jenom v případě, že oblasti primární dojde přerušení služby. V takovém případě všech nových žádostí o sítě směrovat sekundární oblast. Azure přenosy správce může spravovat tento převzetí automaticky.

Převzetí příčinou rychleji než variant jen databáze služby jsou nasadili jste. Tento způsob poskytuje velmi nízký RTO. Oblast sekundární převzetí musí být připravená k odeslání bezprostředně po selhání primární oblasti.

Tento způsob spolu s rychlejší doba odezvy má tu výhodu předběžné přidělení a nasazení služby zálohování. Nemusíte bát oblast nemají místo přidělit nové instance v selhání. Toto je důležité: Pokud sekundárním Azure oblasti se blíží kapacity. Není nijak zaručené (úroveň služeb smlouva), můžete okamžitě budou moct nasadit počet nových cloudovým službám v jakékoli oblasti.

Nejrychlejší doba odezvy v tomto modelu musí mít podobné měřítko (počet instancí role) v oblasti primárních a sekundárních. Bez ohledu na výhod platební nevyužitá výpočetním instance je nákladné a to nemusí být nejčastěji rozumné finanční volba. Z toho důvodu je nejčastěji používaných na vedlejší oblasti používat mírně diagramů s měřítky nohama verzi cloudovým službám. Pak můžete rychle převzít a rozšiřování sekundární nasazení v případě potřeby. Proces překlopení by měl automatizovat tak, že po nedostupné oblasti primární aktivován další instance, v závislosti na načíst. To může zahrnovat použití mechanismus neobsahovaly text jako [Nastaví měřítko virtuálního počítače](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

Na následujícím obrázku vidíte modelu obsahují-li oblasti primárních a sekundárních plně nasazeném Cloudová služba ve aktivní pasivní vzorku.

![Aktivní pasivní, úplné otevřené](./media/resiliency-disaster-recovery-azure-applications/active-passive-full-replica.png)

###<a name="active-active"></a>Aktivní

Nyní by se budete pravděpodobně zjištění vývoj vzorky: Zmenšení RTO zvyšuje náklady a složitosti. Řešení aktivní skutečně konce tomto tendence s ohledem na náklady.

V aktivní vzorku jsou cloudovými službami a databáze plně nasazenou v obou oblastí. Na rozdíl od modelu aktivní pasivní přijímat obě oblasti přenosy uživatele. Tato možnost určuje nejrychlejší časového využití. Služby jsou už měřítko pro zpracování část načíst v jednotlivých oblastech. Použití sekundární oblasti DNS je už povolit. Je další složitost zjištění, jak chcete směrovat uživatele na příslušnou oblast. Plánování kruhového možné. Může se stát, že někteří uživatelé byste použili konkrétní oblasti, kde jsou uložená primární kopii svým datům.

V případě selhání zákaz jednoduše DNS k oblasti primární. To přesměrovává všechny přenosy na vedlejší oblast.

I v tomto modelu jsou některé varianty. Například následující diagram ukazuje modelu, kde oblasti primárního vlastní předlohy kopii databáze. Cloudových služeb v obou oblastí zapisovat primárního databázi. Sekundární nasazení může číst z primární nebo replikovanou databázi. Replikace v tomto příkladu se stane jedním ze způsobů.

![Aktivní](./media/resiliency-disaster-recovery-azure-applications/active-active.png)

Existuje nevýhodou architektury aktivní na předchozím obrázku. Druhá oblast musí přístup k databázi v první oblasti, protože tam je umístěn předlohu. Vypnutí při přístupu k datům z mimo oblast významně vynechává výkonu. V volání více oblastí databáze měli byste zvážit určitý typ dávky strategie pro zvýšení výkonu tyto hovory. Další informace najdete v tématu [použití dávky ke zvýšení výkonu aplikace databáze SQL](../sql-database/sql-database-use-batching-to-improve-performance.md).

Alternativní architektury může zahrnovat každou oblast přímý přístup vlastní databázi. V tomto modelu určitý typ obousměrný replikace potřebné k synchronizaci databází v jednotlivých oblastech.

V aktivní vzorku nebudete potřebovat tolik instancí v oblasti hlavní stejně jako v aktivní pasivní vzorku. Pokud máte 10 instance v oblasti primární architektura aktivní pasivní, můžete zkusit pouze 5 v jednotlivých oblastech architektura aktivní. Obě oblasti nyní sdílet načíst. Může to být náklady úspor nad aktivní pasivní vzorek když si Poznámkový blok teplé úsporném v oblasti pasivní s 10 instance čeká se na převzetí.

Třeba si uvědomíte, že dokud obnovení oblasti primárního oblasti sekundární obdržet náhlé nárůstu noví uživatelé. Pokud jsou 10 000 uživatelů na všech serverech případě, že oblasti primární dojde přerušení služby, oblasti sekundární neočekávaně musí zpracovat 20 000 uživatelů. Pravidla na vedlejší oblasti sledování musíte zjistit tato tlačítka Zvětšit písmo a dvojité instance v oblasti sekundární. Další informace o tom naleznete v části o [zjišťování chyb](#failure-detection).

##<a name="hybrid-on-premises-and-cloud-solution"></a>Hybridní místních i cloudových řešení

Jeden další strategie pro obnovení havárie má architektonické hybridní aplikaci, která poběží v místním i schránkami v cloudu. V závislosti na aplikaci a oblasti primárního může být buď umístění. Zvažte předchozí architektury a Představte si oblasti primární a sekundární jako místního pracoviště.

V těchto hybridní architektury jsou některé problémy. Většina Tento článek obsahuje nejdřív adresovaná PaaS architektura vzorků. Typické PaaS aplikací v Azure závisí na Azure specifické konstrukce například role, cloudovým službám a provoz správce. Vytvoření místní řešení pro tento typ PaaS aplikace vyžadují architekturu významně neliší. Toto nemusí být možné správy nebo nákladům.

Vyřešíte hybridní havárie obnovení má však méně výzvy pro tradiční architektury, které byly jednoduše přesunuté do cloudu. To platí pro architektury, které používají IaaS. Aplikace IaaS používat virtuálních počítačích v cloudu, můžete mít přímého místním ekvivalenty. Můžete taky virtuálních sítí spojení počítačích v cloudu s místní síti zdroje. Otevře se několik možností, které nejsou možné s pouze PaaS aplikace. Například SQL serveru můžete využít řešení obnovení havárie například skupiny dostupnosti AlwaysOn a odrážející strukturu databáze. Podrobnosti najdete v tématu [dostupnost a obnovení pro SQL Server ve virtuálních počítačích Azure](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

Řešení IaaS taky poskytují jednodušší cestu pro místní aplikací používat Azure jako možnost překlopení. Můžete mít plně funkční aplikaci do existující oblasti místní. Co když však nemáte zdroje, které chcete udržovat geograficky samostatném oblast pro překlopení? Můžete se rozhodnout pomocí virtuálních počítačích a virtuální sítě aplikace spuštěné v Azure. V takovém případě definujte procesy, které synchronizace dat do cloudu. Azure nasazení pak bude sekundárním oblasti, kterou chcete použít pro překlopení. Primární oblast zůstane místní aplikace. Další informace o IaaS architektury a funkcí najdete v [dokumentaci virtuálních počítačích](https://azure.microsoft.com/documentation/services/virtual-machines/).

##<a name="alternative-cloud"></a>Alternativní cloudu

Existují situace, kdy i odolnosti v cloudu společnosti Microsoft nesplňuje interní dodržování předpisů či zásad, které potřebuje vaše organizace. I nejlepší přípravu a návrh a implementace záložní systémy během selhání spadají krátké při přerušení globální služby cloudu poskytovatele služeb.

Je vhodné a umožňuje porovnávat požadavků na dostupnost náklady a složitosti vyšší dostupnost:. Analýza rizika a definovat operace RPO i RTO řešení pro. Pokud aplikace nelze tolerovat všechny prostoje, může být vhodné pro trocha zamyšlení nad prezentacemi pomocí jiné cloudové řešení. Pokud není na celém Internetu přejde, může být k dispozici, pokud Azure globálně nedostupné jiné cloudové řešení.

S hybridním scénář mohou také selhání nasazení v předchozí obnovení architektury havárie existovat v jiné cloudové řešení. Alternativní cloudu DR weby bude použito pouze pro řešení jehož RTO umožňuje velmi málo, případně prostoje. Všimněte si, že řešení, které využívá web DR mimo Azure budou vyžadovat víc práce ke konfiguraci, vývoj, nasazení a správě. Je také obtížnější implementovat doporučené postupy ve více cloudu architektuře. Podobné uceleném koncepce existovat cloudu platformách rozhraní API a architektury se liší.

Pokud se rozhodnete rozdělení DR mezi různé platformy, ho má smysl architektonické vrstvy v návrhu řešení. Pokud to uděláte, nemusíte vyvíjet a udržovat dvěma různými verzemi stejného aplikace pro jiné cloudové platformy v případě havárie. Jako ve scénáři hybridní použití virtuálních počítačích Azure nebo služba Azure kontejneru může být v těchto případech snadnější použití cloudu specifické PaaS návrhů.

##<a name="automation"></a>Automatizace

Některé vzorce, které právě popisované vyžadují snadné aktivace offline nasazení i obnovení určité části systému. Automatizace nebo skriptování podporuje možnost aktivovat zdroje na vyžádání a rychle nasazení řešení. V tomto článku související DR automatizaci odpovídající pomocí [Prostředí PowerShell Azure](https://msdn.microsoft.com/library/azure/jj156055.aspx), ale [Služby správy REST API](https://msdn.microsoft.com/library/azure/ee460799.aspx) je také možnost.

Vývoj skriptů pomáhá spravovat díly DR, který Azure zpracovává transparentně. Výhodou vytvářet stejné výsledky pokaždé, když, která minimalizuje možnost lidské chyby. S předdefinované DR skripty také snižuje na čase sestavit systému a jejích částí in the midst of selhání. Nechcete zkuste ručně zjistit, jak obnovit webu, dokud je dolů a ztracený peněz za minutu.

Po vytvoření skripty otestujete opakovaně od začátku do konce. Po ověření základní funkce Ujistěte se, abyste je otestovali v [havárie simulace](#disaster-simulation). Díky objevovat chyby skriptů nebo procesů.

Doporučený postup s automatizaci je vytvoření úložiště skriptů Powershellu nebo skriptů rozhraní příkazového řádku (rozhraní příkazového řádku) pro Azure havárie obnovení. Jasně označte a uspořádat pro snadné vyhledání. Určuje jedné osobě spravovat úložiště a správa verzí skriptů. Dokument je také vysvětlení parametry a příklady použití skriptu. Zkontrolujte taky, abyste tento si přečtěte následující dokumentaci synchronizované s Azure nasazení. To zdůrazňuje účel s jedné osobě zodpovědní za všechny části úložiště.

##<a name="failure-detection"></a>Zjišťování chyb

K správně řešit problémy s dostupností a obnovení, musíte mít ke zjištění a diagnostice k chybám. Měli byste udělat Upřesnit server a nasazení sledovat, abyste můžete rychle věděli, kdy systému nebo jeho části se neočekávaně dolů. Sledování nástroje, které podívejte se na celkové stavu Cloudová služba a závislými můžete provádět část této práce. Jeden nástroj Microsoft je [System Center 2016](https://www.microsoft.com/en-us/server-cloud/products/system-center-2016/). Nástrojů jiných výrobců můžete také zadat možnosti sledování. Většina sledování řešení sledování klíčových výkonnosti a dostupnost služby.

Sice zásadní těchto nástrojích, budou nezastavíte potřeb k plánování zjišťování chyb a vytváření sestav v rámci do cloudové služby. Plánujete musí správně pomocí diagnostických nástrojů Azure. Vlastní výkonnosti nebo zápisy v protokolu událostí lze také část celkové strategie. To umožňuje dalších dat během selhání rychle diagnostikovat potíže a obnovit veškeré funkce. Také poskytuje další metriky používající nástroje pro sledování aplikací stav. Další informace najdete v tématu [Povolení Azure Diagnostika v Azure Cloud Services](../cloud-services/cloud-services-dotnet-diagnostics.md). Informace o plánování celkové "stavu modelu", najdete v článku [bezporuchový: pokyny pro pružné architektury cloudu](https://channel9.msdn.com/Series/FailSafe).

##<a name="disaster-simulation"></a>Havárie simulace

Testování simulace zahrnuje vytvoření malých reálné situace na ZAOKR.dolů práce sledovat, jak členové týmu reagovat. Simulace zobrazit taky do jaké míry řešení jsou v plánu obnovení. Proveďte simulace tak, že vytvořené scénáře není přerušit skutečné firmy při pořád úplně jako reálné situace.

Zvažte architecting typu "přepínací panel" v aplikaci tak, aby napodobily problémy s dostupností ručně. Například pomocí měkké přepnout spuštění databáze aplikace access výjimky pro řazení modul tak, že ho nebude fungovat správně. Podobně jako lightweight postupů pro ostatní moduly může trvat na úrovni rozhraní sítě.

Simulace zvýrazní případných problémů s vlastním některých adresovaná. Simulovaný scénáře musí být úplně ovladatelné. To znamená, že, i v případě plánu obnovy zdá selže, bude možné obnovit situace zpátky do normálního snadnějšímu škodu významné. Je také důležité informovat vyšší úrovně správy o kdy a jak cvičení simulace bude spouštět. Tento plán by měl obsahovat informace o čas nebo zdroje, které se může stát neproduktivní je spuštěná test simulace. Pokud váš plán obnovení havárie jste aby na test, je také třeba určit, jak bude měřit úspěšnost.

Existuje několik jinými postupy, které můžete použít k testování havárie obnovení plány. Většina z nich jsou však jednoduše změněné verze těchto základních technik. Hlavní motiv za tento test, je vyhodnocení jak je to vhodné a jak funkční plánu obnovy. Testování obnovení havárie se zaměřuje na podrobnosti a seznamte se s holes v plánu základní obnovení.

##<a name="next-steps"></a>Další kroky

Tento článek je součástí řady článků věnovaných [havárie využití a dostupnost pro aplikace založený na Microsoft Azure](./resiliency-disaster-recovery-high-availability-azure-applications.md). Předchozí článek v této řadě je [dostupnost pro aplikace založený na Microsoft Azure](./resiliency-high-availability-azure-applications.md).
