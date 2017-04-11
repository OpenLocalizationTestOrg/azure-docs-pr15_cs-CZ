<properties
   pageTitle="Kontrolní seznam odolnost proti chybám | Microsoft Azure"
   description="Kontrolní seznam, který obsahuje pokyny k odolnost proti chybám pochybnosti průběhu návrhu."
   services=""
   documentationCenter="na"
   authors="petertaylor9999"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="petertay"/>

# <a name="azure-resiliency-guidance-resiliency-checklist"></a>Pokyny pro Azure odolnost proti chybám: odolnost proti chybám kontrolního seznamu

Návrh aplikace odolnosti vyžaduje plánování a zmírnit různé režimy selhání, ke kterým může dojít. Kontrola položky v tomto kontrolním seznamu proti návrhu aplikace snažíme usnadnit jeho více pružné.

## <a name="requirements"></a>Požadavky

- **Definice požadavků na dostupnost vašeho zákazníka.** Zákazníkovi bude obsahovat požadavků na dostupnost pro součásti v aplikaci a bude to mít vliv návrhu aplikace. Získat smlouva od vašeho zákazníka pro dostupnost cílů každou část aplikace, jinak návrhu nevyhovuje očekávání zákazníka. Další informace naleznete v části [Definování vašim požadavkům odolnost proti chybám](guidance-resiliency-overview.md#defining-your-resiliency-requirements) [navrhování pružné žádosti o Azure](guidance-resiliency-overview.md) dokumentu.

## <a name="failure-mode-analysis"></a>Chyba při režimu analýzy

- **Provádění analýzy režimu selhání (FMA) pro aplikace.** FMA je proces pro vytváření odolnost proti chybám do aplikace brzy v oblasti návrhu. Cíle FMA patří:  

    - Zjistěte, jaké druhy selhání aplikace může dojít. 
    - Zachycení potenciální efekty a dopad každý typ chyby na aplikace.
    - Určení strategie obnovení. 

    Další informace najdete v tématu [navrhování pružné žádosti o Azure: analýzy režimu selhání][fma].  

## <a name="application"></a>Aplikace

- **Nasazení více instancí služby.** Nevyhnutelně nezdaří služeb a aplikace závisí na jednu instanci služby nevyhnutelně selže taky. Zřízení několika instancí [aplikace](../app-service/app-service-value-prop-what-is.md)služby Azure, vyberte [Plánování služby aplikace](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) nabízející několika instancích spuštěných. Cloudové služby Azure nakonfigurujte každý použití [několika instancích spuštěných](../cloud-services/cloud-services-choose-me.md#scaling-and-management)role. [Azure virtuálních počítačích (VMs)](../virtual-machines/virtual-machines-windows-about.md), ověřit, že OM architektura obsahuje více než jeden OM a, že je součástí [Nastavte dostupnost]každé OM[availability-sets].   

- **Použijte při vyrovnávání zatížení k distribuci žádosti.** Vyrovnávání zatížení distribuuje aplikace požadavky na instance služby správný odebráním chybná instance otočení. Pokud vaše služba používá aplikaci služby Azure nebo Azure cloudové služby, je už rozloženy za vás. Ale pokud aplikace používá Azure VMs, musíte se zřízení vyrovnávání zatížení. V tématu Přehled [Vyrovnávání zatížení Azure](../load-balancer/load-balancer-overview.md) další podrobnosti. 

- **Konfigurace brány aplikace Azure používat více instancí.** Podle potřeby aplikace [Azure aplikace brány](../application-gateway/application-gateway-introduction.md) může být lepší hodí k distribuci žádosti o služby aplikace. Však jednoho instancí služby aplikace brány nejsou zaručena SLA tak, aby bylo možné, že aplikace selhat v případě instance aplikace brány se nezdaří. Zřízení víc střední větší aplikace brány instance nebo k zajištění dostupnosti služby za podmínek [SLA](https://azure.microsoft.com/support/legal/sla/application-gateway/v1_0/).

- **Použití sady dostupnost pro jednotlivé aplikace osy**. Umístění svého instancí [Nastavte dostupnost] [ availability-sets] zaručuje připojení k aspoň jednou instancí OM v rámci [SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_2/). Pokud váš VMs netvoří sady dostupnost, kterou jste si ještě zaručuje je chrání a je možné, že selhat všechny nebo aktualizovat současně. 

- **Zvažte nasazení aplikace napříč několika oblastí.** Pokud nasazení aplikace do jedné oblasti v méně častých události nebude k dispozici celá oblast, bude vaše aplikace taky není k dispozici. Může to nepřijatelná za podmínek SLA vaše aplikace. Pokud ano, zvažte nasazení aplikace a její služby napříč několika oblastí. Nasazení více oblastí můžete použít vzorek aktivní (distribuce žádosti o napříč několika aktivní instance) nebo vzorku aktivní pasivní (uchovávání instanci "teplé" v rezervovat, v případě, že se nezdaří primární instance). Doporučujeme nasazení více instancí aplikace služby přes místní dvojice. Další informace najdete v tématu [firmy kontinuitu a havárie obnovení (BCDR): Azure párovaný oblastí](../best-practices-availability-paired-regions.md).

- **V případě potřeby: implementujte odolnost proti chybám vzorků pro vzdálené operace.** Pokud vaše aplikace závisí na komunikaci mezi vzdálené, se nepovede nevyhnutelně cestu komunikace. Pokud jsou k více chybám, může se žádostí o mnoha zbývající správný instancí aplikace služby. Několik vzorků jsou vhodné k řešení běžných chyb včetně vzorek vypršení časového limitu [Opakovat vzorek][retry-pattern], [dělení okruh] [ circuit-breaker] vzorku a další. Další informace najdete v tématu [navrhování pružné žádosti o Azure](guidance-resiliency-overview.md#implementing-resiliency-strategies). 

- **Umožňuje odpovědět na zvýšení zatížení neobsahovaly text.** Pokud vaše aplikace není nakonfigurován pro rozšiřování automaticky při načítání, je možné, že vaše aplikace služby selže, pokud se stanou syté s koncových uživatelů. Další informace najdete v těchto článcích:

    - Obecné: [Kontrolní seznam škálovatelnost:](../best-practices-scalability-checklist.md) 
    - Azure aplikaci služby: [Měřítko počet instancí, ať už ručně nebo automaticky][app-service-autoscale]
    - Cloudové služby: [jak automatické měřítko do cloudové služby][cloud-service-autoscale]
    - [Nastaví automatické změny velikosti a virtuálního počítače měřítko] virtuálních počítačích:[vmss-autoscale]


- **Implementace asynchronní operace kdykoli je to možné.** Synchronní operace může zabrat všechny zdroje a blokovat další operace během volající čeká na dokončení procesu. Navrhněte každou část aplikace umožňující asynchronní operace kdykoli je to možné. Další informace o tom, jak implementovat asynchronní programování v jazyce C# najdete v tématu [asynchronní programování s asynchronní a očekávat][asynchronous-c-sharp].

- **Pomocí Azure přenosy správce aplikace provoz směrovat na různých oblastí.**  [Azure správce přenosy] [ traffic-manager] provede na úrovni DNS pro vyrovnávání zatížení a u různých oblastí podle [přenosy směrování] můžete směrovat přenosy v síti[ traffic-manager-routing] metoda zadáte a stavu koncové body vaše aplikace. 

- **Konfigurace a otestujte sond stavu pro vyrovnávání zatížení a provoz správce.** Zkontrolujte, zda logiky stavu zkontroluje kritické části systém a řádně podporovat odpovídá sond stavu.

    - Stavu hledá [Azure přenosy správce] [ traffic-manager] a [Vyrovnávání zatížení Azure] [ load-balancer] vytisknout konkrétní funkci. Pro správce přenosy zkušební stavu určuje, zda přenesou do jiné oblasti. Pro vyrovnávání zatížení ho Určuje, zda virtuálního počítače odeberete otočení.      

    - Pro zkušební provoz správce zkontrolujte koncový bod stavu nějaké důležité závislosti, které jsou ve stejné oblasti nasazeném a jehož selhání aktivují přepojení do jiné oblasti.  

    - Pro vyrovnávání zatížení by měl koncový bod stavu vykazování stavu OM. Nezahrnovat dalších úrovní nebo externí služby. V opačném se nepovede, který bude proveden mimo OM způsobí, že vyrovnávání zatížení OM odeberete otočení.

    - Provedení, sledování stavu v aplikaci, najdete v článku [Vzorek sledování stavu koncového bodu](https://msdn.microsoft.com/library/dn589789.aspx).

- **Sledování jiných výrobců služeb.** Pokud aplikace má závislosti u služby jiných výrobců, identifikaci kde a jak může selhat těchto služeb třetích stran a co efektem tyto selhání bude mít v aplikaci. Služba třetích stran nemusí připsat sledování a diagnostice, je důležité protokolování vyvolání z nich a sladit se vaše aplikace zdraví a diagnostické protokolování pomocí jedinečný identifikátor. Další informace o doporučených postupech pro monitorování a diagnostice najdete v článku [pokyny pro sledování a diagnostice] [ monitoring-and-diagnostics-guidance] dokumentu.

- **Ujistěte se, že všechny služby třetích stran, kterou můžete používat poskytuje SLA.** Pokud aplikace závisí na službě třetích stran, ale třetích stran poskytuje nijak zaručené dostupnost ve formuláři SLA, vaše aplikace dostupnost také nemůže být zaručena. Vaše SLA není jenom nejméně dostupné součásti aplikace.


## <a name="data-management"></a>Správa dat

- **Princip metody replikace pro zdroje dat aplikace.** Data aplikace budou uloženy v různých zdrojů dat a mají různé dostupnost požadavky. Vyhodnocení metody replikace u jednotlivých typů ukládání dat v Azure, včetně [Azure úložiště replikace](../storage/storage-redundancy.md) a [SQL databáze aktivní Geo replikace](../sql-database/sql-database-geo-replication-overview.md) zajistit, aby byly ve výsledcích požadavkům dat aplikace.

- **Zkontrolujte, že žádný jednoho uživatelského účtu nainstalovaný přístup k datům výrobní a zálohování.** Zálohování dat jsou ohroženo, pokud jeden jednoho uživatelského účtu má oprávnění k zápisu výrobní a zálohování zdrojů. Se zlými úmysly může záměrně odstranit všechna data, zatímco běžný uživatel může omylem odstranit. Navrhněte aplikaci pro omezení oprávnění jednotlivých uživatelských účtů, aby jenom uživatelé, které vyžadují přístup pro zápis přístup pro zápis a je pouze výrobní nebo zálohování, ale ne obě.

- **Dokument zdroje dat převzít a navrácení procesu a otestovat.** V případě, kdy zdroj dat nejsou catastrophically budou muset operátor oblasti lidských zdrojů podle sady uvedených pokynů převzít nový zdroj dat. Máte dokumentovaného kroky chyby, nepůjde operátor úspěšně sledovat a do svých je selhání zdroje. Otestujte pravidelně instrukční kroky k ověření, že operátor po jejich je možné úspěšně převzít a navrácení zdroje dat.

- **Ověření dat zálohy.** Pravidelně zkontrolujte, že zálohování dat očekáváte spuštěním skriptu pro ověřování integrity dat, schématu a dotazů. Neexistuje bod s zálohy, pokud je to není vhodné obnovit zdroje dat. Odhlaste se a vykazování veškeré nekonzistence, můžete opravit záložní služby.

- **Zvažte použití úložiště typu účtu, který je geo nadbytečné.** Dat uložených v úložišti Azure účet je vždy replikovat místně. Můžou ale nastat více strategie replikace můžete vybírat při zřízení účtu úložiště. Vyberte [Přístup pro čtení Geo nadbytečné úložišti Azure (Vzdálená pomoc GRS)](../storage/storage-redundancy.md#read-access-geo-redundant-storage) chránit vaše aplikace data před méně častých velikost písmen při celou oblast nebude k dispozici.

    > [AZURE.NOTE] Pro VMs nejsou založeny na Vzdálená pomoc GRS replikace obnovíte disků OM (soubory virtuálního pevného disku). Místo toho použít [Azure zálohování][azure-backup].   

## <a name="operations"></a>Operace

- **Provedení sledování a výstrahy osvědčené postupy v aplikaci.** Bez správného sledování diagnostických nástrojů a výstrahy, nejde žádným způsobem zjišťování chyb v aplikaci a upozornit operátor vyřešit. Další informace o doporučených postupech najdete v tématu [sledování a diagnostice pokyny] [ monitoring-and-diagnostics-guidance] dokumentu.

- **Změřte statistiku vzdálené volání a informace zpřístupnit týmu aplikace.**  Pokud nechcete sledovat a statistické vzdálené volání v reálném čase a poskytují snadný způsob, jak chcete-li zobrazit tyto informace, nebude mít týmu operace okamžité zobrazení do stavu aplikace. A pokud se jenom míra průměr vzdálené volání čas, nebudete mít dost informací zobrazíte problémy ve službách. Souhrn metriky remote volání například latence, výkon a chyb v percentily 99 a 95. Provádění statistických analýz metrice vám pomůžou odhalit chyby probíhajících v rámci každé percentilu.

- **Sledování počtu přechodná výjimky a opakování přes odpovídající časový rámec.** Pokud nechcete sledovat a monitorovat přechodná výjimky a pokusy časem opakování, je možné problému nebo selhání může ho skrýt – Logika opakování vaše aplikace. To znamená když se při sledování a protokolování pouze úspěšně nebo neúspěšně operaci, se na to, že operace musel být opakované tisknutím kvůli výjimky skryté. Spojnice trendu zvětšení výjimky v čase označuje, že služba má problém a se nemusí podařit. Další informace najdete v tématu [Opakovat pokyny pro konkrétní službu][retry-service-guidance].

- **Implementace upozornění systému včasného, který upozorní operátor.** Určení klíčové výkonu indikátory stavu aplikace, jako je třeba přechodná výjimky a vzdálené volání latence a nastavte příslušný prahové hodnoty pro každý z nich. Odešlete upozornění na operace při dosažení mezní hodnota. Nastavte tyto prahové hodnoty na úrovni identifikující problémy, než změní na kritické a vyžadovat obnovení odpověď.

- **Dokumentů procesu uvolnění aplikace.** Bez podrobné vydání proces si přečtěte následující dokumentaci může operátor nasaďte chybná aktualizace nebo nesprávně konfigurace nastavení pro aplikaci. Jasně definovat dokumentů procesu uvolnění a ujistěte se, že je k dispozici pro celý tým operace. Doporučené postupy pro pružné nasazení aplikace jsou uvedené v [pružné nasazení] [ guidance-resilient-deployment] část dokumentu pokyny odolnost proti chybám.

- **Ujistěte se, že je víc než jedné osobě členy týmu školení ke sledování aplikace a proveďte všechny kroky ručně.** Pokud máte jenom jednu operátor členy týmu, který můžete sledovat aplikace a spusťte tak obnovení kroky, stane se tato osoba selhání v jednom místě. Školení více osob o zjišťování a obnovení a ověřte, že vždy alespoň jedno aktivní kdykoli.

- **Automatizaci nasazení aplikace.** Pokud pracovníkům operace požaduje pro její ručně nasazení aplikace, lidské chyby mohou způsobit selhání nasazení. Další informace o doporučených postupech pro automatizaci nasazení aplikace najdete v tématu [pružné nasazení] [ guidance-resilient-deployment] část dokumentu pokyny odolnost proti chybám.

- **Navrhněte procesu uvolnění maximalizovat dostupnosti aplikace.** Pokud procesu uvolnění vyžaduje službu do offline režimu během nasazení, bude vaše aplikace není k dispozici, dokud se opětovném přechodu do online. Použijte [modré a zelené šipky](http://martinfowler.com/bliki/BlueGreenDeployment.html) nebo [uvolnění Kanárských](http://martinfowler.com/bliki/CanaryRelease.html) nasazení pro nasazení aplikace výroby. Obě z následujících postupů zahrnují tak, aby uživatelé vydání kódu můžete přesměrovaní na výrobní kód v případě selhání nasazení kódu vydání vedle výrobní kód. Další informace najdete v tématu [pružné nasazení] [ guidance-resilient-deployment] část dokumentu pokyny odolnost proti chybám.

- **Odhlaste se a auditování nasazení aplikace.** Pokud používáte přípravu nasazení techniky jako je modré a zelené šipky nebo lesknice verzí, budou více než jednu verzi aplikace spuštěné v výroby. Pokud problém by měla proběhnout, je naprosto zásadní určit, jakou verzi aplikace způsobuje problém. Implementace strategii robustní protokolování k zaznamenání co nejvíce specifické pro verze informací. 

- **Ujistěte se, že aplikace nespustí s desktopovým [omezuje Azure předplatného](../azure-subscription-service-limits.md).** Azure předplatná mají limity z určité typy zdrojů, jako počet skupiny zdrojů, číslo jádra a počet úložiště účtů.  Pokud vašim požadavkům pro aplikace překračují limity Azure předplatného, vytvořte jiné Azure předplatné a poskytování dostatečné prostředky tam.

- **Ujistěte se, že aplikace nespustí s desktopovým [omezuje počet služby](../azure-subscription-service-limits.md).** Jednotlivé služby Azure mají spotřebu limity &mdash; například omezení úložiště, výkon, počet připojení, požadavky za sekundu a jiné metriky. Aplikace se nepovede, pokud se pokusí články překračují tato omezení. Výsledkem bude služby omezení a možné výpadek ovlivněné uživatele. 

    V závislosti na konkrétní službu a požadavky aplikace, můžete často vyhnout tyto limity škálování (například výběr jiného ceny osy) nebo rozšiřování (přidání nové instance).  

- **Navrhněte požadavky na úložiště aplikace spadají do cíle výkon a škálovatelnost Azure úložiště.** Azure úložiště je určená fungovat v rámci předdefinované výkon a škálovatelnost cílů, takže návrhu aplikace pro využití úložiště v těchto cílů. Pokud překročíte tyto cíle aplikace, budou mít omezení úložiště. Pokud to pokud chcete opravit, zřízení další úložiště účty. Pokud spustíte s desktopovým limit úložiště účtu zřízení další předplatná Azure a zřízení dalších účtů úložiště. Další informace najdete v tématu [Azure úložiště škálovatelnost a výkonu cílů](../storage/storage-scalability-targets.md).

- **Vyberte správné velikosti OM aplikace.** Změřte skutečné procesoru, paměti, disk a vstupu/výstupu VMs ve výrobním a ověřte, zda je velikost OM, kterou jste vybrali dostatečné. Pokud ne, aplikace mohou vyskytnout problémy kapacita jako VMs přiblíží jejich omezení. V dokumentu [velikosti virtuálních počítačích v Azure](../virtual-machines/virtual-machines-windows-sizes.md) jsou píše OM velikosti.

- **Zjistěte, jestli pracovní zátěž vaše aplikace není stabilní nebo kolísání určitou dobu.** Pokud váš pracovní zátěž kolísání určitou dobu, použití Azure OM měřítko nastaví automaticky měřítko počet OM instancí. Jinak budete muset ručně zvětšit nebo zmenšit počet VMs. Další informace najdete v tématu [Přehled sady měřítko virtuálního počítače](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

- **Vyberte vrstvy správné služeb pro databázi SQL Azure.** Pokud vaše aplikace používá databáze SQL Azure, ujistěte se, že jste vybrali vrstvy odpovídající služeb. Pokud vyberete možnost osy, který není možné zpracovávat vaše aplikace databázové transakce jednotky (DTU) požadavky, použití dat se sníží. Další informace o výběru plán správné služeb, najdete v článku [Možnosti SQL databáze a výkonu: porozumět tomu, co je k dispozici v každé vrstvy služeb](../sql-database/sql-database-service-tiers.md) dokumentu. 

- **Připravte vrácení plán nasazení.** Je možné, že nasazení aplikace může selhat a způsobit aplikace nedostupná. Navrhněte proces vrácení zpět pro návrat k poslední známou správnou verzi a prostoje. V tématu [pružné nasazení] [ guidance-resilient-deployment] část dokumentu odolnost proti chybám pokyny pro další informace. 

- **Vytvoření procesu pro komunikaci s Azure podpory.** Není-li proces pro kontaktování [podpory Azure](https://azure.microsoft.com/support/plans/) nastaven před kontaktování podpory potřeby, prostoje prodlužuje při procesu podpory přešli poprvé. Zahrnout proces obrátit se na podporu a escalating problémy jako součást aplikace odolnosti od začátku.

- **Ujistěte se, že aplikace nepoužívá více než maximální počet úložiště účtů na jedno předplatné.** Azure umožňuje maximálně 200 účty úložiště na jedno předplatné. Pokud vaše aplikace vyžaduje další úložiště účtů, než jsou aktuálně dostupné ve vašem předplatném, budete muset vytvořit nové předplatné a vytvořte účty další úložiště tam. Další informace najdete v tématu [Azure předplatné a omezení služby, kvót a omezení](../azure-subscription-service-limits.md#storage-limits).

- **Ujistěte se, že aplikace nepřekročila cílů škálovatelnost disků virtuálního počítače.** OM IaaS Azure podporuje připojení počet disků dat v závislosti na několika faktorech, včetně OM velikost a typ účtu úložiště. Pokud aplikace překročí cílů škálovatelnost disků virtuálního počítače, zřízení další úložiště účty a vytvořte disků virtuálního počítače tam. Další informace najdete v tématu [Azure úložiště škálovatelnost a výkonu cílů] (... / storage/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks)

## <a name="test"></a>Test

- **Umožňuje proveďte převzetí a navrácení testování na aplikace.** Pokud jste netestovali plně překlopení a překlopení zpět, nemůžete být některé, že závislé služby v aplikaci přichází zpět synchronizované způsobem během obnovení havárie. Ujistěte se, že vaše aplikace závislé služby převzetí a selhání zpět ve správném pořadí.

- **Provedení pravděpodobnost vkládání testování na aplikace.** Aplikace může selhat mnoho různých důvodů, třeba vypršení platnosti certifikátu vyčerpání systémové prostředky do virtuálního počítače nebo selhání úložiště. Otestujte aplikaci v prostředí co nejblíže k výrobní, simulace nebo aktivaci skutečné k chybám. Například odstranit certifikáty uměle spotřebovávat systémové prostředky a odstranit zdroj úložiště. Ověření vaší aplikace možnost obnovit všechny typy chyb, beze změny a v kombinaci. Zkontrolujte, že nejste k chybám šíření nebo kaskádové prostřednictvím systému.

- **Spusťte testy ve výrobním pomocí obou syntetické a skutečnou uživatelská data.** Test a výrobních shodná jen zřídka, takže je důležité pomocí modré a zelené šipky nebo lesknice nasazení a otestovat aplikace ve výrobním. Tímto způsobem lze otestovat aplikace ve výrobním skutečné zatížení a zajistěte, aby že fungovaly očekávaným při plně nasazení.

## <a name="security"></a>Zabezpečení

- **Implementace aplikace úrovně ochrany proti distribuované útokům služby (Denial).** Azure služby jsou chráněny před útoky Denial síťové vrstvy. Však Azure nemůže zajistit ochranu před útoky aplikace vrstvy, protože je těžké rozlišit mezi požadavky true uživatelů z zlými úmysly žádostí o. Další informace o tom, jak chránit před útoky Denial aplikace naleznete v části "Nastavení ochrany proti Denial" [Microsoft Azure Network Security](http://download.microsoft.com/download/C/A/3/CA3FC5C0-ECE0-4F87-BF4B-D74064A00846/AzureNetworkSecurity_v3_Feb2015.pdf) (ke stažení PDF).

- **Implementace Princip nejnižších možných oprávnění k přístupu k prostředkům aplikace.** Výchozí nastavení pro přístup k prostředkům aplikace by měl být možná omezení. Udělení oprávnění na vyšší úrovni na základě schválení. Udělení příliš opravňující přístupu k prostředkům aplikace ve výchozím nastavení můžete mít za následek někdo záměrně nebo omylem odstraňování zdrojů. [Řízení přístupu na základě rolí](../active-directory/role-based-access-built-in-roles.md) ke správě oprávnění uživatelům Azure zajištěno, ale je třeba ověřit nejnižšími oprávněními oprávnění pro další zdroje, které obsahují vlastní oprávnění systémy, třeba SQL Server. 

## <a name="telemetry"></a>Telemetrie

- **Protokolování telemetrie dat v provozním prostředí je spuštěná aplikace.** Zachycení informací robustní telemetrie v režimu aplikace běží výrobní prostředí nebo nebudete mít dostatečnou informace pro diagnostiku způsobují problémy během aktivně slouží uživatelů. Další informace najdete v seznamu protokolování doporučené postupy je k dispozici v [sledování a diagnostice pokyny] [ monitoring-and-diagnostics-guidance] dokumentu.

- **Implementace protokolování pomocí asynchronní vzorku.** Pokud jsou operace protokolování synchronní, může blokovat kód aplikace. Zajištění provádění operací protokolování jako asynchronní operace. 

- **Sladit dat protokolu přes hranice služby.** Do aplikace typické n osy žádost uživatele procházet několik omezení služeb. Například žádost o uživatele obvykle pochází ve vrstvě web je předán osy business a nakonec zachován ve vrstvě data. V dalších složitějších scénáře může požadavek uživatele úměrně mnoho různých služeb a datový úložiště. Ujistěte se, že systému protokolování koreluje hovory přes hranice služby, takže žádost můžete sledovat v aplikaci.

##  <a name="azure-resources"></a>Azure zdroje 

- **Když použijete šablonu správce prostředků Azure k poskytování zdroje.** Správce prostředků šablony snadněji automatizovat nasazení prostřednictvím prostředí PowerShell nebo rozhraní příkazového řádku Azure, které vedou k spolehlivější procesu nasazení. Další informace najdete v tématu [Přehled Správce prostředků Azure][resource-manager].

- **Zadejte smysluplné názvy zdrojů.** Pojmenování zdroje smysluplné názvy usnadňuje vyhledání určitého zdroje a interpretaci svoji roli. Další informace najdete v tématu [Doporučené pojmenování konvence pro Azure zdroje](guidance-naming-conventions.md) 

- **Řízení přístupu na základě rolí použití (RBAC)**. Řízení přístupu k Azure prostředky, které nasadíte pomocí RBAC. RBAC můžete přiřadit role se tak mohli ověřovat členům týmu DevOps Chcete-li zabránit nechtěným odstraněním nebo změny nasazeném zdroje. Další informace najdete v tématu [Začínáme s řízení přístupu na portálu Azure](../active-directory/role-based-access-control-what-is.md) 

- **Použití uzamčení prostředků pro kritické zdroje, jako je třeba VMs.** Uzamčení prostředků zabránit operátor omylem odstranění zdroje. Další informace najdete v tématu [Lock zdrojů pomocí Správce prostředků Azure](../resource-group-lock-resources.md) 

- **Místní dvojice.** Při nasazení dvou oblastí, vyberte stejnou místní dvojici oblastí. V případě výpadku obecných obnovení jednom regionu přednostně mimo každou dvojici. Některé služeb, jako je Geo nadbytečné úložiště zadejte automatické replikace párových oblasti. Další informace najdete v tématu [firmy kontinuitu a havárie obnovení (BCDR): Azure párovaný oblastí](../best-practices-availability-paired-regions.md) 

- **Skupiny zdrojů uspořádat tak, že funkce a životního cyklu**.  Skupina zdroje obecně smí obsahovat zdroje, které mají stejné životního cyklu. To usnadňuje Správa nasazení, odstraňte zkušební nasazení a přiřadit oprávnění snížit šanci, že provozní nasazení omylem odstranit nebo změnit. Vytvořit samostatné skupiny prostředků pro výrobní, vývoj a otestujte prostředí. Ve více oblastech nasazení umístění zdroje pro každou oblast do samostatné skupiny prostředků. To usnadňuje přeinstalujte jednom regionu bez vlivu ostatní oblasti. 

## <a name="azure-services"></a>Služby Azure 

Následující položky kontrolního seznamu platí pro konkrétní služby Azure.

###  <a name="app-service"></a>Aplikace služby 

- **Pomocí standardní nebo Premium osy.** Tyto úrovně podpory pracovní sloty a automatické zálohy. Další informace najdete v tématu [Přehled hloubkovou plány aplikaci služby Azure](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) 

- **Vyhněte se měřítko nahoru nebo dolů.** Místo toho vyberte osy a instance velikost písma, které vyhovují vašim požadavkům výkonu ve skupinovém rámečku typické načítání a [měřítko,](../app-service-web/web-sites-scale.md) instance zpracovávání změn v objemem. Změna měřítka nahoru a dolů může spustit restartování aplikace.  

- **Obsahují konfigurace jako nastavení aplikace.** Nastavení aplikace umožňuje ukládat nastavení konfigurace jako nastavení aplikace. Určete nastavení v šablon správce prostředků nebo pomocí Powershellu, takže můžete použít je jako součást automatického nasazení / aktualizovat proces, který je spolehlivější. Další informace najdete v tématu [Konfigurace web apps v aplikaci služby Azure](../app-service-web/web-sites-configure.md). 

- **Vytvořte samostatné plány aplikaci služby pro výrobní a otestujte.** Nepoužívejte sloty v nasazení výrobní testování.  Všechny aplikace v rámci stejné plán služeb aplikací sdílet stejnou OM instance. Pokud vložíte výrobní a zkušební nasazení se stejným plánem, může negativně ovlivnit provozní nasazení. Testuje zatížení může například snížit provozním webu. Vložením zkušební nasazení do samostatného plánu vyčlenění je z verze výroby.  

- **Samostatné webové aplikace z webu rozhraní API**. Pokud má vaše řešení webových front-end a webového rozhraní API, zvažte decomposing do samostatné aplikace služeb aplikací. Tento návrh usnadňuje rozložit řešení tak, že pracovní zátěž. Můžete použít web app a rozhraní API v samostatných plánech aplikaci služby, může být zachován nezávisle na sobě. Pokud nepotřebujete úroveň škálovatelnost na nejdřív, můžete nasazení aplikace do plánu stejného a je přesuňte na samostatné plány později v případě potřeby.

- **Vyhněte se použití funkce zálohování aplikaci služby k obecnějším údajům databáze Azure SQL.** Použijte [databáze SQL automatické zálohy][sql-backup]. Zálohování aplikaci služby exportuje databáze do souboru .bacpac SQL, který náklady DTUs.  

- **Nasazení pracovní pozici.** Vytvoření pozici nasazení pro pracovní. Nasazení aktualizace aplikace na pracovní pozici a ověřte nasazení před výměna do výroby. To snižuje pravděpodobnost chybná aktualizace v výroby. Také zaručuje, že všechny instance jsou provozní teplotu před vyměnit do provozu. Řada aplikací mít významné zahřívání a studený start čas. Další informace najdete v tématu [Nastavení pracovní prostředí pro web apps v aplikaci služby Azure](../app-service-web/web-sites-staged-publishing.md). 

-  **Vytvoření nasazení úsek pro uložení nasazení poslední známý (LKG).** Při nasazení aktualizaci výroby přesunete do úsek LKG předchozí provozní nasazení. Usnadňuje vrátit zpět chybná nasazení. Pokud zjistíte problém později, můžete rychle vrátit k LKG verze. Další informace najdete v tématu [Architektura Azure odkaz: základní webové aplikace](guidance-web-apps-basic.md). 

- **Povolení protokolování diagnostiky**, včetně aplikace protokolování a protokolování webového serveru. Protokolování je důležité pro sledování a diagnostice. V tématu [Povolení diagnostiky protokolování pro web apps v aplikaci služby Azure](../app-service-web/web-sites-enable-diagnostic-log.md)

- **Úložiště objektů blob v protokolu**. Usnadňuje shromažďovat a analyzovat data. 

- **Vytvořte účet samostatný úložiště pro protokoly.** Nepoužívejte stejný účet úložiště pro protokolování a data aplikací. To umožňuje zabránit v snížení výkonu aplikace protokolování. 

- **Sledovat výkon.** Pomocí sledování služby, třeba [Nový Relic](http://newrelic.com/) nebo [Přehledy aplikace](../application-insights/app-insights-overview.md) pro sledování aplikací výkonu a chování zatížení výkonu.  Sledování výkonu můžete v reálném čase přehled o aplikaci. Umožňuje Diagnostika problémů a provést analýzu hlavních příčin chyb. 


###  <a name="application-gateway"></a>Aplikace brány 

- **Zřízení nejméně dvě instance.** Nasazení aplikace brány s nejméně dvě instance. Jedna instance představuje jeden bod selhání. Pomocí dvou nebo více instancí redundance a škálovatelnost. Abychom měli nárok pro [SLA](https://azure.microsoft.com/support/legal/sla/application-gateway/v1_0/), musí zřizovat dva nebo víc instancí střední nebo větší. 

### <a name="azure-search"></a>Azure hledání

- **Zřízení víc otevřené.** Použijte aspoň dva repliky pro čtení vysoké dostupnosti nebo tři pro vysokou dostupnost pro čtení i zápis.

- **Konfigurace indexování pro nasazení více oblastí.** Pokud máte víc oblast nasazení, zvažte možností pro souvislost indexování.

    - U zdroje dat se replikovat geo přejděte na každý indexování každou místní Azure vyhledávací službu jeho otevřené zdroj místní data.  

    - Pokud zdroj dat není geo replikovat, nastavte ukazatel myši více indexování na stejný zdroj dat, tak, aby služby Azure hledání ve více oblastech nepřetržitě a nezávisle na sobě indexu ze zdroje dat. Další informace najdete v tématu [vyhledávání Azure aspektech výkonu a optimalizace][search-optimization].

###  <a name="azure-storage"></a>Azure úložiště 

- **Data aplikace pomocí geo nadbytečné úložiště přístup pro čtení (Vzdálená pomoc GRS).** Vzdálená pomoc GRS úložiště zreplikuje data do vedlejší oblasti a poskytuje přístup z oblasti vedlejší jen pro čtení. Pokud je v oblasti hlavní výpadku úložiště, aplikace číst data z oblasti sekundární. Další informace najdete v tématu [replikace Azure úložiště](../storage/storage-redundancy.md).

- **Pro OM disků pomocí Premium úložiště** Další informace najdete v tématu [Premium úložiště: výkonné úložiště pro Azure virtuálního počítače úloh](../storage/storage-premium-storage.md).

- **Úložištěm fronty vytvořte záložní fronty v jiné oblasti.** Úložištěm fronty má otevřené jen pro čtení omezenou použití, protože nemůžete fronty nebo dequeue položky. Místo toho vytvořte záložní fronty úložiště účtu v jiné oblasti. Pokud existuje výpadku úložiště, aplikace pomocí záložní fronty, přejděte na primární oblasti znovu k dispozici. Tímto způsobem aplikace nadále zpracovávat nových žádostí o.  

###  <a name="documentdb"></a>DocumentDB 

- **Replikace databáze oblastí.** Pod svým účtem více oblastí DocumentDB databáze obsahuje jeden zápisu oblast a více oblastí pro čtení. Pokud dojde k selhání v oblasti zapsat, si můžete přečíst z jiného otevřené. Klienta SDK pracuje s tím automaticky. Můžete taky převzetí zápisu oblast do jiné oblasti. Další informace najdete v tématu [Rozmístit dat globálně s DocumentDB](../documentdb/documentdb-distribute-data-globally.md).


###  <a name="sql-database"></a>Databáze SQL 

- **Pomocí standardní nebo Premium osy.** Tyto úrovně zadejte tečku již v okamžiku obnovení (35 dní). Další informace najdete v tématu [Možnosti SQL databáze a výkonu](../sql-database/sql-database-service-tiers.md).

- **Povolte auditování databáze SQL.** Sestavy auditování mohou sloužit k Diagnostika škodlivými útoky nebo lidské chybu. Další informace najdete v tématu [Začínáme s SQL databáze auditování](../sql-database/sql-database-auditing-get-started.md). 

- **Použití aktivní Geo replikace** Použití aktivní Geo replikace vytvoření čitelné sekundární v jiné oblasti.  Když se vaše primární databáze nepovede, nebo jednoduše je potřeba vzít offline, provést ruční přepnutí do vedlejší databáze.  Dokud selhání, zůstane databázi vedlejší jen pro čtení.  Další informace najdete v tématu [SQL aktivní Geo-replikace databáze](../sql-database/sql-database-geo-replication-overview.md). 

- **Použití sharding**. Zvažte použití sharding oddíl databáze ve vodorovném směru. Sharding můžete poskytnout izolace chyb. Další informace najdete v tématu [měřítko, s databáze SQL Azure](../sql-database/sql-database-elastic-scale-introduction.md). 

- **Pomocí v okamžiku obnovení obnovit z oblasti lidských zdrojů chyby.**  V okamžiku obnovení databáze vrátí předchozí v čase. Další informace najdete v tématu [obnovení databáze Azure SQL pomocí zálohy databáze automatické][sql-restore].

- **Pomocí geo obnovení obnovení z výpadku služby.** Obnovení GEO obnoví databázi ze geo nadbytečné zálohy.  Další informace najdete v tématu [obnovení databáze Azure SQL pomocí zálohy databáze automatické][sql-restore].


###  <a name="sql-server-running-in-a-vm"></a>SQL Server (spuštění v virtuálního počítače)

- **Replikace databáze.** Umožňuje replikace databáze SQL serveru vždy na dostupnost skupiny. Zvyšuje dostupnost, když se nepovede jedna instance serveru SQL Server. Další informace najdete v tématu [Další informace...](guidance-compute-n-tier-vm.md) 

- **Zálohování databáze**. Pokud už používáte [Azure zálohování](https://azure.microsoft.com/documentation/services/backup/) k obecnějším údajům vaší VMs, zvažte použití [Zálohování Azure pro použití DPM úloh systému SQL Server](../backup/backup-azure-backup-sql.md). Tento přístup je jedna role náhradní správce organizace a jednotné vymáhání VMs a SQL Server. V opačném pomocí [SQL Server spravovaných nástroje Zálohování Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx). 


###  <a name="traffic-manager"></a>Přenosy správce 

- **Proveďte ruční navrácení.** Po převzetí přenosy správce provést ruční navrácení, spíše než automaticky selhání zpět. Než zpět, ověřte, zda všechny aplikace podsystémy správný.  Jinak můžete vytvořit situaci, kdy aplikace překlápět sebou poslalo mezi datacentrech. Další informace najdete v tématu [Spuštění VMs ve více oblastech](guidance-compute-multiple-datacenters.md).

- **Vytvoření koncového bodu zkušební stavu**. Vytvořte vlastní koncový bod, který sestavy z celkové stavu aplikace. Díky přenosy správce překlopení selže kritické cesty, nikoli pouze front-end. Koncový bod, měly vrátit kód chyby HTTP-li kritické závislost chybná nebo nedostupný. Není chybách nekritické státní, ale. Při není potřeba, vytváření falešně pozitivní jinak zkušební stavu může spustí překlopení. Další informace najdete v tématu [Sledování přenosy správce koncového bodu a překlopení](../traffic-manager/traffic-manager-monitoring.md).


###  <a name="virtual-machines"></a>Virtuálních počítačích 

- **Nemějte na jeden OM spuštěné pracovní zátěž výroby.** Jeden nasazení OM není pružné plánované nebo neplánované údržbu. Místo toho vložte víc VMs v dostupnost sadu nebo sadu měřítko OM, se při vyrovnávání zatížení před. Abychom měli nárok pro SLA, musí být v rámci sady stejné dostupnost nasazeny aspoň dva virtuálních počítačích. Další informace najdete v tématu [Přehled sady měřítko virtuálního počítače](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). 

- **Určení dostupnosti nastavení, když zřizujete OM.** V současné době nejde žádným způsobem přidáte dostupné po zřízení OM OM správce prostředků. Při přidávání nové OM existující dostupnosti nastavit, zkontrolujte, že vytvořit NIC OM a přidat NIC do fondu back-end adresu na Vyrovnávání zatížení. V opačném nebude Vyrovnávání zatížení sítě provoz směrovat na této OM. 

- **Vložte jednotlivé vrstvy aplikace do samostatné dostupnost nastavení.** V aplikaci N-osy nevkládejte VMs z různých úrovní do stejnou sadu dostupnosti. VMs sady dostupnost jsou umístěná v poruch doménách (FDs) a upravte domény (UD). Výhoda redundance FDs a UDs získáte musí být zpracovat stejné žádosti klienta o každé OM v sadě dostupnosti. 

- **Výběr správné velikosti OM výkonu požadavkům.** Při přesouvání existující pracovní zátěž do Azure, začněte s velikostí OM, který se nejvíc blíží hledanému místní servery. Potom měření výkonu svého vytížení procesoru, paměti a disku procesorů a v případě potřeby upravte velikost. To umožňuje zajistit, aby že aplikace se chová podle očekávání v prostředí cloudu. Také pokud potřebujete více nic, mějte na paměti NIC omezení pro každou velikost. 

- **Použití premium úložiště pro VHD.** Azure Premium úložiště podporuje výkonných maximum, minimum latence disku. Další informace najdete v tématu [Premium úložiště: výkonné úložiště pro Azure virtuálního počítače úloh](../storage/storage-premium-storage.md) zvolte velikost OM, který podporuje premium úložiště. 

- **Vytvořte účet samostatný úložiště pro každou OM.** Místo VHD pro jeden OM zohledňovala odděleně. Tímto způsobem se vyhnete zasažení procesorů omezení úložiště účtů. Další informace najdete v tématu [Azure úložiště škálovatelnost a výkonu cílů](../storage/storage-scalability-targets.md). Ale pokud nasazujete velkého počtu VMs, uvědomte za předplatné limit úložiště účty. V tématu [limity úložiště](../azure-subscription-service-limits.md#storage-limits).

- **Vytvořit účet samostatný úložiště pro diagnostické protokoly**. Neovlivňují protokoly pro diagnostiku zápisu ke stejnému účtu úložiště jako VHD, abyste nemuseli protokolování diagnostiky procesorů disků OM. Protokoly pro diagnostiku stačí standardní účet místně nadbytečné úložiště (LRS). 

- **Instalace aplikací na disk dat, nikoli disk s operačním systémem.** V opačném se dostanete do omezení velikosti disku. 

- **Použití Azure zálohování VMs.** Zálohování ochrana před ztrátou dat náhodné. Další informace najdete v tématu [Ochrana VMs Azure pomocí služby trezoru obnovení](../backup/backup-azure-vms-first-look-arm.md).

- **Povolit diagnostické protokoly**, včetně základní stav metriky, infrastruktura protokoly a [diagnostice spouštěcí][boot-diagnostics]. Spuštění diagnostiky můžete Diagnostika chyby spouštění vaší OM získá do-spouštěcí stavu. Další informace najdete v tématu [Přehled o Azure protokoly pro diagnostiku][diagnostics-logs].

- **Použití koncovku AzureLogCollector**. (Windows VMs pouze.) Tuto linku sloučí Azure platformu protokoly a odešlou se je Azure úložiště, bez operátor vzdáleně přihlášení OM. Další informace najdete v tématu [AzureLogCollector rozšíření](../virtual-machines/virtual-machines-windows-log-collector-extension.md).


###  <a name="virtual-network"></a>Virtuální sítě 

- **Povolených nebo blok adresy IP dodejte podsítě NSG.** Zablokování přístupu uživatelů se zlými úmysly nebo ji povolit přístup jenom od uživatelů, kteří mají oprávnění pro přístup k aplikaci.  

- **Vytvořte vlastní stav zkušební.** Sond stavu Vyrovnávání zatížení můžete otestovat HTTP nebo TCP. Pokud OM získáte serveru HTTP zkušební HTTP je lepší indikátor stavu než zkušební TCP. Zkušební HTTP když použijete vlastní koncový bod, který sestavy celkový stav aplikace, včetně všech závislostí důležité. Další informace najdete v tématu [Přehled Vyrovnávání zatížení Azure](../load-balancer/load-balancer-overview.md).

- **Nechcete blokovat zkušební stavu.** Zkušební stavu Vyrovnávání zatížení se pošle z známé IP adresu, 168.63.129.16. Nechcete blokovat přenosy do nebo z této IP v libovolné zásady brány firewall nebo pravidla skupiny (NSG) zabezpečení sítě. Blokování zkušební stavu způsobí Vyrovnávání zatížení OM odeberete otočení. 

- **Povolení protokolování Vyrovnávání zatížení.** Protokoly na back-end zobrazily kolik VMs nechodí v síti kvůli selhalo zkušební odpovědi. Další informace najdete v tématu [protokolu analýz pro vyrovnávání zatížení Azure](../load-balancer/load-balancer-monitor-log.md).


<!-- links -->
[app-service-autoscale]: ../monitoring-and-diagnostics/insights-how-to-scale.md
[asynchronous-c-sharp]:https://msdn.microsoft.com/library/mt674882.aspx
[availability-sets]:../virtual-machines/virtual-machines-windows-manage-availability.md
[azure-backup]: https://azure.microsoft.com/documentation/services/backup/
[boot-diagnostics]: https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/
[circuit-breaker]: https://msdn.microsoft.com/library/dn589784.aspx
[cloud-service-autoscale]: ../cloud-services/cloud-services-how-to-scale.md
[diagnostics-logs]: ../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md
[fma]: guidance-resiliency-failure-mode-analysis.md
[guidance-resilient-deployment]: guidance-resiliency-overview.md#resilient-deployment
[load-balancer]: load-balancer/load-balancer-overview.md
[monitoring-and-diagnostics-guidance]: ../best-practices-monitoring.md
[resource-manager]: ../azure-resource-manager/resource-group-overview.md
[retry-pattern]: https://msdn.microsoft.com/library/dn589788.aspx
[retry-service-guidance]: ../best-practices-retry-service-specific.md
[search-optimization]: ../search/search-performance-optimization.md
[sql-backup]: ../sql-database/sql-database-automated-backups.md
[sql-restore]: ../sql-database/sql-database-recovery-using-backups.md
[traffic-manager]: ../traffic-manager/traffic-manager-overview.md
[traffic-manager-routing]: ../traffic-manager/traffic-manager-routing-methods.md
[vmss-autoscale]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview.md
