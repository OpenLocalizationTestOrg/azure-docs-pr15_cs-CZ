<properties
   pageTitle="Ukládání do mezipaměti pokyny | Microsoft Azure"
   description="Pokyny pro ukládání do mezipaměti zlepšit výkon a škálovatelnost."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/14/2016"
   ms.author="masashin"/>


# <a name="caching-guidance"></a>Pokyny pro ukládání do mezipaměti

[AZURE.INCLUDE [pnp-header](../includes/guidance-pnp-header-include.md)]

Ukládání do mezipaměti je běžné postup, který chce zlepšit výkon a škálovatelnost systému. Je to dočasně zkopírováním často používané dat do rychlé úložiště, který je umístěn Zavřít aplikace. Pokud tento rychlý úložný prostor nachází blíž k aplikaci než původní zdroj, pak ukládání do mezipaměti můžete výrazně zvýšit doby odezvy pro klientské aplikace podávání dat rychleji.

Ukládání do mezipaměti při co nejefektivněji instanci klienta opakovaně přečte stejná data, zejména pokud všechny následující podmínky použití původní úložiště dat:
- Zůstane relativně statický.
- Je pomalé ve srovnání s rychlost mezipaměti.
- Podléhají vysokou úroveň konflikty.
- Je daleko latence síti může způsobovat přístup pomalá.

## <a name="caching-in-distributed-applications"></a>Ukládání do mezipaměti distribuované aplikace

Distribuované aplikace obvykle implementace jednu nebo obě z následujících postupů při ukládání do mezipaměti dat:

- Použití soukromé mezipaměti dat rozdávání místně na počítači, na kterém běží instanci aplikace nebo služby.
- Použití sdíleného mezipaměti sloužící jako zdroj běžné, který je přístupný pomocí několika procesy a/nebo počítačích.

V obou případech ukládání do mezipaměti lze provést straně klienta a/nebo straně serveru. Ukládání do mezipaměti klientských vystavila společnost proces, který poskytuje uživatelské rozhraní pro systém, třeba z webového prohlížeče nebo desktopovou aplikaci.
Ukládání do mezipaměti serverovou vystavila společnost proces, který poskytuje podnikové vzdáleně spuštěné.

### <a name="private-caching"></a>Soukromé ukládání do mezipaměti

Základní typ mezipaměti je úložiště v paměti. Má uskutečňuje do adresního prostoru jednoho obrázku a přistupovat přímo pomocí kódu, který běží daného procesu. Tento typ mezipaměti je velmi rychlý přístup. Můžete taky dát velmi efektivní prostředky pro ukládání menší množství statická data, protože velikost mezipaměti obvykle omezena objemu paměti, která je k dispozici v počítači hostingu procesu.

Pokud budete potřebovat víc informací, než je možné fyzicky v paměti do mezipaměti, můžete napsat data uložená v mezipaměti na místním pevném disku. To bude pomaleji pro přístup k než data, která se uskutečňuje v paměti, ale měli byste být rychlejší a spolehlivější než načítání dat v síti.

Pokud máte víc instancí aplikace, která používá tento model souběžně s jednotlivé instance aplikace má vlastní nezávislé mezipaměť blokování vlastní kopii data.

Představit mezipaměti jako snímek původní data nastane okamžik v minulosti. Pokud se tato data statické, je pravděpodobné, že instancí jiné aplikace podržte různé verze aplikací data v mezipamětí. Stejný dotaz provedly tyto instance proto lze vracet různé výsledky, jak je vidět na obrázku 1.

![Použití v mezipaměti do jiné instance aplikace](media/best-practices-caching/Figure1.png)

_Obrázek 1: Použití v mezipaměti do jiné instance aplikace_

### <a name="shared-caching"></a>Sdílené ukládání do mezipaměti

Použití sdíleného mezipaměti můžete vyřešit problémy, že data se může lišit v jednotlivých mezipaměti, která může dojít při ukládání do mezipaměti v paměti. Sdílené ukládání do mezipaměti zajistí, že instancí jiné aplikace uvidí stejné zobrazení dat uložených v mezipaměti. Je to vyhledáním mezipaměti v samostatném umístění, obvykle hostované jako součást samostatná služba, jak je vidět na obrázku 2.

![Použití sdíleného mezipaměti](media/best-practices-caching/Figure2.png)

_Obrázek 2: Používání sdílené mezipaměti_

Důležité výhodou sdílené mezipaměti přístup je škálovatelnost, které poskytuje. Mnoho služeb sdílené mezipaměti implementovaná pomocí clusteru serverů a používat software, který rozdělí data clusteru průhledné. Instance aplikace jednoduše odešle žádost o službu mezipaměti.
Základní infrastruktura zodpovídá za určení umístění dat uložených v mezipaměti clusteru. Přidáním dalších serverů můžete snadno změnit velikost mezipaměti.

Existují dva hlavní nevýhody sdílené mezipaměti přístupu:
- Mezipaměť je pomaleji přístup, protože je už směřuje místně na jednotlivé instance aplikace.
- Požadavek implementovat služby samostatných mezipaměti může přidat složitost řešení.

## <a name="considerations-for-using-caching"></a>Důležité informace týkající se používání, ukládání do mezipaměti

Následující oddíly popisují podrobněji důležité informace týkající se vytváření a používání mezipaměti.

### <a name="decide-when-to-cache-data"></a>Rozhodnutí, zda do mezipaměti dat

Ukládání do mezipaměti můžete výrazně zvýšit výkon a škálovatelnost, dostupnost. Čím víc dat, které máte a větší počet uživatelů, které se potřebují dostat k těmto datům větší výhody ukládání do mezipaměti se stanou. Je proto, že ukládání do mezipaměti snižuje latence a konflikty, který máte přidružený k zpracování velkých objemů souběžně prováděné žádosti v úložišti původní data.

Například databáze může podporovat omezený počet souběžné připojení. Načtení dat z sdílené mezipaměti, ale místo databáze, umožňuje klientské aplikace k přístupu k těmto datům i v případě, že je počet dostupných připojení aktuálně vyčerpány. Kromě toho databáze nebude k dispozici, klientské aplikace může by mohl pokračujte pomocí ukládání dat v mezipaměti.

Zvažte možnost, ukládání do mezipaměti dat, která je často číst, ale upravovat zřídka (například data, která má větší podíl operace čtení než operace zápisu). Nedoporučujeme však použít mezipaměti jako autoritativní úložiště důležitých informací. Místo toho Ujistěte se, vždy uloženy všechny změny, které aplikace nemůžete dovolit ztratit trvalý datový úložiště. To znamená, že pokud mezipaměti není k dispozici, aplikace můžete nadále pracovat pomocí úložiště dat a nepřijdete o důležitých informací.

### <a name="determine-how-to-cache-data-effectively"></a>Zjistěte, jak efektivně mezipaměti dat

Kombinace kláves použité mezipaměť efektivně spočívá v určení nejvhodnější dat do mezipaměti a ukládání do mezipaměti včas. Data můžete přidat do mezipaměti službu poprvé, že je načtena podle aplikace. To znamená, že aplikace potřebuje k získání dat jenom jednou z úložiště dat a následný přístup může být splněny pomocí mezipaměti.

Můžete taky mezipaměť můžete být plně nebo částečně vyplněné data předem, obvykle při spuštění aplikace (přístupu k jmenoval ohlašovat). Však nemusí být vhodné implementovat ohlašovat pro velké mezipaměti, protože tento postup můžete ukládat náhlé, vysoké zatížení na původní úložiště při spuštění aplikace.

Často analýzu použití vzorků vám můžou pomoct rozhodnout, jestli plně nebo částečně předem mezipaměti a vyberte data, která chcete mezipaměti. Například může být užitečné pro mezipaměti s údaji statického uživatelského profilu pro zákazníky, kteří používají aplikaci pravidelně (třeba denně), ale ne pro zákazníky, kteří používají aplikaci jenom jednou týdně.

Ukládání do mezipaměti obvykle funguje dobře s daty, která je neměnný nebo který změní zřídka. Jako příklad lze uvést referenční informace o například produktu a ceny v aplikaci obchodování nebo sdílené statické prostředky, které jsou nákladné vytvářet. Některé nebo všechny tato data můžete načíst do mezipaměti při spuštění aplikace minimalizovat požadavky na prostředky a aby se zvýšil výkon. Také je vhodné mít pozadí obrázku, který aktualizuje pravidelně odkaz je aktuální data v mezipaměti tak, aby nebo, která aktualizuje mezipaměti při odkazují na data změny.

Ukládání do mezipaměti slouží méně dynamických dat sice existují některé výjimky pro posouzení (viz oddíl vysoce dynamické data v mezipaměti dál v tomto článku najdete další informace). Pokud se původní data změní pravidelně, informací uložených v mezipaměti zastaralá velmi rychle nebo režijních synchronizace mezipaměti se obchodu původní data zmenší účinnosti ukládání do mezipaměti.

Všimněte si, že mezipaměť není nutné zadávat úplná entity. Například pokud datová položka představuje objekt s více hodnotami třeba zákazník banky se jménem, adresou a zůstatek účtu, některé z těchto prvků může zůstat statické (například jméno a adresu), zatímco ostatní (například zůstatek účtu) může být dynamičtější. V těchto případech může být užitečné do mezipaměti statické částí data a získat (nebo vypočítat) pouze u zbývajících informací kdy není potřeba.

Doporučujeme provést výkonu testování a použití analýzu a zjistit, zda je vhodné před základního souboru nebo na vyžádání načítání mezipaměti nebo kombinaci obojího. Rozhodnutí by měl být na základě těkavost a způsob použití dat. Analýza využití a výkonu mezipaměti je velmi důležitým v aplikacích, které dojít vysoké zatížení a musí být vysoce scalable. Například v vysoce scalable scénáře může být vhodné pro mezipaměti zmenšit zatížení úložišti ve špičce.

Ukládání do mezipaměti lze také pro zabránění opakovaným výpočty je spuštěná aplikace. Pokud operace transformací dat nebo provede výpočet složité, jeho uložení výsledků operace v mezipaměti. Pokud stejný výpočet požaduje později, můžete jednoduše načítání dat aplikací výsledky z mezipaměti.

Aplikaci můžete změnit data, která je uskutečňuje v mezipaměti. Doporučujeme však přemýšlení mezipaměti jako úložiště přechodná dat, která zase zmizí kdykoli. Neukládejte cenný data v mezipaměti. Ujistěte se, správě informací v původní úložišti taky. To znamená, že pokud mezipaměti nebude k dispozici, můžete omezit možnost ztráty dat.

### <a name="cache-highly-dynamic-data"></a>Vysoce dynamické data v mezipaměti

Při uložení rychle změna informací v úložišti trvalý dat můžete ukládat režijních systému. Zvažte například zařízení, které neustále sestav stavu nebo jiných měrných jednotek. Pokud aplikace nezobrazovat, zobrazí se tato data na základě, že informace uložené v mezipaměti téměř vždycky zastaralé do mezipaměti může být platí při ukládání a načítání tyto informace z úložiště dat stejné pozornost. V časové náročnosti uložte a načíst tato data pravděpodobně jste změnili.

V případě například to zvažte výhody ukládání dynamické informace přímo do mezipaměti místo v úložišti trvalý data. Pokud jsou data nekritické a nevyžaduje auditování, pak nezáleží občas změny budou ztraceny.

### <a name="manage-data-expiration-in-a-cache"></a>Správa vypršení platnosti data v mezipaměti

Ve většině případů ukládání do mezipaměti dat je kopií data, která se uskutečňuje v úložišti původní data. Data v původní úložiště dat může změnit adresa po byl mezipaměti, které jsou příčinou data uložená v mezipaměti osvobozením od zastaralé. Mnoho mezipaměti systémy umožňují konfigurovat mezipaměť platnost dat a zmenšení období, pro kterou může být zastaralý data.

Když skončí platnost data uložená v mezipaměti, je odebraná z mezipaměti a aplikace musí načtení dat z obchodu původní data (ho můžete dát nově načítat informace zpět do mezipaměti). Při konfiguraci mezipaměti, můžete nastavit výchozí zásady vypršení platnosti. V mnoha služeb mezipaměti můžete také stanovit platnosti pro jednotlivé objekty při ukládání je programově v mezipaměti.
Některé mezipaměti umožňují určit doby vypršení platnosti jako absolutní hodnota, nebo jako klouzavé hodnota, která způsobí, že položku, kterou chcete odebrat z mezipaměti, pokud není otevírá v zadané doby. Toto nastavení potlačí všechny zásad vypršení platnosti celou mezipaměť, ale jenom pro zadané objekty.

> [AZURE.NOTE] Zvažte vypršení platnosti dobu trvání mezipaměti a objekty, které obsahuje pečlivě. Pokud jste provedli příliš krátký, objekty vyprší příliš rychlý a sníží výhody používání mezipaměti. Pokud uděláte doby moc dlouhá, rizika data zastaralé.

Je také možné, že mezipaměti může zaplnit Pokud dat bude moct zůstat rezidentní poměrně dlouho. V tomto případě všechny žádosti o přidání nové položky do mezipaměti, může být některé položky vynutit odebrání ve výrobním procesu jmenoval vyřazení. Mezipaměť služby obvykle vyčistit dat na základě nejmenších naposledy použitých (LRU), ale obvykle přepsat tuto zásadu a zabránit určený k vyloučení položek. Ale pokud přijmout tento přístup je rizika, vyšší než paměti, která jsou dostupná v mezipaměti. Aplikace, pokusí přidat položky do mezipaměti selže kvůli výjimce.

Některé mezipaměti implementace může obsahovat další vyřazení zásady. Existuje několik typů vyřazení zásad. Jedná se o:
- Naposledy použité zásady (v za předpokladu, že dat se nevyžaduje znovu).
- Zásady first v první out (od nejstaršího data je odstraněna).
- Explicitní odebrání zásad založených na aktivační událost (třeba data upravuje).

### <a name="invalidate-data-in-a-client-side-cache"></a>Platnost data v mezipaměti straně klienta

Ukládání do mezipaměti klientských dat se obecně považuje mimo pracuje služba, která obsahuje data k desktopovému klientovi. Služba nelze vynutit přímo klienta k přidání nebo odebrání informací z mezipaměti straně klienta.

To znamená, že je možné pro klienta, který používá špatně nakonfigurované mezipaměti nadále používat zastaralé informace. Například pokud nejsou správně implementace zásad vypršení platnosti mezipaměti, klienta pomocí zastaralé informace, které je do místní mezipaměti při změně informací v původní zdroj dat.

Pokud vytváříte webové aplikace, které bude sloužit dat přes připojení HTTP, můžete vzdáleně nejnovější informace implicitně vynutit webový klient (například prohlížeče nebo proxy serveru webové). To můžete udělat, pokud zdroje se aktualizuje při změně URI daný zdroj. Weboví klienti obvykle používají URI zdroj klíč v mezipaměti straně klienta, pokud se změní identifikátor URI webový klient ignoruje dříve cached verzích zdroj a místo toho načte novou verzi.

## <a name="managing-concurrency-in-a-cache"></a>Správa souběžné v mezipaměti

Mezipaměti často mají být sdíleny více instance aplikace. Jednotlivé instance aplikace můžete číst a upravovat data v mezipaměti. Proto stejné souběžné problémy, které nastanou jakékoli Store sdílených dat také použít v mezipaměti. V případě, že pokud aplikace potřebuje k úpravám ukládání do mezipaměti dat může být nutné ověřit, že aktualizace provedené jednou instancí aplikace nedojde k přepsání změn provedených v jiné instanci.

Podle povahy data a pravděpodobnost konflikty můžete přijímat jedním ze dvou postupů k souběžné:

- __Optimistická.__ Bezprostředně před aktualizaci dat, zkontroluje aplikace, zda se od byla načtena změnila data v mezipaměti. Pokud jsou data dál stejná, lze provést změny. V opačném má aplikace se rozhodnout, jestli chcete aktualizovat. (Obchodní logiky, jednotky toto rozhodnutí bude specifická.) Tento přístup je vhodné v situacích, kdy aktualizace jsou Časté nebo kde konflikty pravděpodobně problémy.
- __Pesimistická.__ Načte data, aplikace ho zamkne v mezipaměti další instanci zabránit změnám ze strany ho. Tento proces zajišťuje, že nelze provést konflikty, ale můžete také blokovat další instance, které je nutné zpracovat stejná data. Pesimistická souběžné může ovlivnit škálovatelnost řešení a doporučovány pouze pro krátkodobý operace. Tento postup může být vhodné v situacích, kdy konflikty pravděpodobně, zejména pokud aplikace aktualizuje několika položek v mezipaměti a ujistěte se, že tyto změny budou provedeny ve konzistentní.

### <a name="implement-high-availability-and-scalability-and-improve-performance"></a>Implementace dostupnost a škálovatelnost a zlepšit celkový výkon

Eliminování možnosti použití mezipaměti jako primární úložiště data. Toto je role původní úložišti, ze které se zobrazí v mezipaměti. Původní úložiště dat je zodpovědný za zajišťování trvalé data.

Dávejte pozor, abyste do vašeho řešení dejte kritické závislostí na dostupnost služby sdílené mezipaměti. Aplikace by měla nebude fungovat, pokud není dostupná služba, která poskytuje sdílené mezipaměti. Aplikace neměli reagovat nebo se nezdaří během čekání služby mezipaměti pokračovat.

Proto musí být aplikace připravena zjistit dostupnost služby mezipaměti a přejdou na původní úložiště dat, pokud je nedostupný mezipaměti. [Dělení okruh vzorku](http://msdn.microsoft.com/library/dn589784.aspx) je užitečné pro zpracování tento scénář. Služba, která poskytuje mezipaměti lze obnovit a jakmile je k dispozici, mezipaměti můžete naplnit dat je číst formulář původní úložišti dat, sledovat strategii například [mezipaměti odložené vzorku](http://msdn.microsoft.com/library/dn589799.aspx).

Však může existovat dopad škálovatelnost systému aplikace, které patří zpátky na původní úložiště dat při mezipaměť je dočasně nedostupný.
Během obnovuje úložišti, původní úložiště dat může swamped s žádostí o data, výsledkem časové limity a se nezdařilo připojení.

Implementace místní, soukromé mezipaměti pokaždé aplikaci, spolu s sdílených mezipaměti, které všechny instance aplikace přístup. Pokud aplikace načte položky, ho můžete, zkontrolujte nejdřív v místní mezipaměti, a potom v sdílené mezipaměti a nakonec na původní data obsahují. Místní mezipaměti můžete vyplní pomocí data v mezipaměti sdílené nebo v databázi, pokud je k dispozici sdílené mezipaměť.

Tento postup vyžaduje opatrní konfigurace zabráníte příliš zastaralé týkající mezipaměť sdílené místní mezipaměti. Však místní mezipaměti chová jako vyrovnávací paměť sdílené mezipaměti není dostupný. Obrázek 3 zobrazuje tuto strukturu.

![Použití místní, soukromé mezipaměti s sdílených mezipaměti](media/best-practices-caching/Caching3.png)
_Obrázek 3: Použití soukromé, místní mezipaměti s sdílených mezipaměti_

K podpoře velké mezipaměti, které obsahují relativně dlouhodobé dat, některých služeb mezipaměti nabízí možnost Vysoká dostupnost implementující automatické převzetí Pokud mezipaměti nebude k dispozici. Tento přístup obvykle zahrnuje replikace data uložená v mezipaměti, která jsou uložená na serveru primární mezipaměti k serveru sekundární mezipaměti a přechod na vedlejší serveru selže primární server nebo připojení se ztratí.

Snížit latence, který máte přidružený k psaní na více míst, replikace sekundární server může dojít k asynchronní po zápisu dat do mezipaměti na primárním serveru. Tento přístup vede k možnost některé informace uložené v mezipaměti může dojít ke ztrátě v případě se nepovede, že část tato data by měly být stručné ve srovnání s celkovou velikost mezipaměti.

Pokud je sdílený mezipaměť velký, může to být skutečným oddíl mezipaměti dat mezi uzly snížit šanci konflikty a zlepšit škálovatelnost. Počet sdílených mezipaměti podporují možnost dynamicky přidávat (a odebrání) uzlů a vyrovnání data mezi oddíly. Tento postup může zahrnovat clusterů, ve kterém jsou kolekce uzlů na klientské aplikace vyjádřená bezproblémové s jednou mezipaměti. Interně ale data rozděleny mezi uzly následující předdefinované rozdělení strategie, která rovnoměrně zůstatky načíst. Další informace o možných rozdělení strategie obsahuje [rozdělení pokynech dat](http://msdn.microsoft.com/library/dn589795.aspx) na webu společnosti Microsoft.

Clusterů, můžete zvýšit dostupnost mezipaměti. Pokud se nezdaří uzel zbytek mezipaměti přístupný pořád.
Clusterů často používá ve spojení s replikace překlopení. Každý uzel replikovat a otevřené můžete rychle do online režimu selže uzel.

V mnoha číst a zápisu mívají zahrnují jednoduché datové hodnoty a objekty. Však v některých případech může být nutné pro uložení nebo načtení velkých objemů dat rychle.
Například ohlašovat mezipaměť může vztahovat na psaní stovky nebo tisíce položky do mezipaměti. Aplikace taky muset načíst velké množství souvisejících položek z mezipaměti jako součást stejné žádost.

Mnoho velkých mezipaměti poskytují dávku operace k těmto účelům. To umožňuje klientské aplikaci zabalit velké množství položek do jedné žádost a snižuje režijních, který máte přidružený k provádění velkého počtu malé žádosti.

## <a name="caching-and-eventual-consistency"></a>Ukládání a případné konzistenci

Pro vzorek upravující mezipaměti pro práci instanci aplikace, který naplní mezipaměti musí mít přístup k poslední a konzistentní verze data. V systému implementující případný konzistenci (například replikovanou úložišti) nemusí k tomu.

Jedna instance aplikace může upravit položku data a platnost režim cached verze dané položky. Jiné instanci aplikace zkusit číst tuto položku z mezipaměti, který způsobí, že mezipaměti a neúspěšných, takže načte data z úložiště dat a přidá ji do mezipaměti. Pokud úložišti nebyl plně synchronizován s dalšími replikami, instance aplikace může číst a naplnit staré hodnotou mezipaměť.

Další informace o zpracování konzistence dat naleznete na stránce [Základy konzistence dat](http://msdn.microsoft.com/library/dn589800.aspx) na webu společnosti Microsoft.

### <a name="protect-cached-data"></a>Ochrana dat uložených v mezipaměti

Bez ohledu na službu mezipaměti používejte, zvažte jak chránit data, která se uskutečňuje v mezipaměti z neoprávněnému přístupu. Existují dva hlavní pochybnosti:

- Ochrana osobních údajů v mezipaměti
- Ochrana osobních údajů tak, jak se přetékat mezi mezipaměti a aplikace, která používá mezipaměti

Chránit data v mezipaměti, může služby mezipaměti implementovat mechanismus ověřování, který vyžaduje, aby aplikace zadejte následující údaje:
- Které identit zpřístupníte data v mezipaměti.
- Operací, které (pro čtení a zápis), které tyto identity jsou povoleny k provedení.

Snížení režijních, který máte přidružený k čtení a zápis dat, po identitě udělení zapsat a/nebo čtení do mezipaměti, identity používaný všechna data v mezipaměti.

Pokud potřebujete omezit přístup k podmnožin dat uložených v mezipaměti, můžete udělat jednu z následujících akcí:

- Rozdělení mezipaměti do oddílů (pomocí různých mezipaměti servery) a jenom poskytnout přístup ke identit oddíly, které mohou použít.
- Šifrování dat v každé podmnožina pomocí různých klíčů a poskytují šifrovacího klíče jenom s identitami, které mají mít přístup ke každé podmnožina. Klientská aplikace může pořád moct načíst všechna data v mezipaměti, ale jenom budou moct dešifrovat data, pro kterou má klíče.

Musí taky chránit data jako přecházel a odhlášení z mezipaměti. K tomuto účelu závisí na zabezpečení funkce poskytované síťovou infrastrukturu používaný klientské aplikace připojení do mezipaměti. Pokud mezipaměti je implementovaná pomocí serveru na místě ve stejné organizaci, která hostuje klientské aplikace, pak izolace přímo do sítě nemusí vyžadují, abyste provést další kroky. Pokud mezipaměti nachází vzdáleně, vyžaduje připojení s TCP a HTTP veřejnou sítí (jako je Internet) implementace SSL.

## <a name="considerations-for-implementing-caching-with-microsoft-azure"></a>Co zvážit při provádění ukládání do mezipaměti s Microsoft Azure

Azure poskytuje mezipaměť Redis Azure. Toto je implementace otevřít zdroj Redis mezipaměti, který běží jako službu v Azure datacentra. Poskytuje služby ukládání do mezipaměti, které můžete k nim získat přístup z Azure aplikace, jestli aplikaci implementace jako do cloudové služby, na webu nebo v Azure virtuálního počítače. Mezipaměti smí zobrazovat klientské aplikace, které mají odpovídající přístupové klávesy.

Azure Redis mezipaměť je výkonné mezipaměti řešení, které poskytuje dostupnost, škálovatelnost a zabezpečení. Obvykle spuštěna jako služba rozšířit mezi jeden nebo více vyhrazené počítačích. Pokusí se obsahují neodeberete informace, jak můžete v paměti zajistit rychlý přístup. Tato architektura slouží k poskytování s nízkou latencí a s vysokou jejich zmenšením potřebné k provádění operací pomalé.

 Azure Redis mezipaměť je kompatibilní s mnoha různých rozhraní API, které se používají v klientských aplikacích. Pokud máte existující aplikace, které už používají Azure Redis mezipaměti systém místní mezipaměť Redis Azure poskytuje rychlá migrace cestu k ukládání v cloudu.

> [AZURE.NOTE] Azure obsahuje také služba spravovaných mezipaměti. Tato služba je založena na modul Azure služby struktury mezipaměti. Umožňuje vytvořit distribuované mezipaměti, které smí zobrazovat volně svázáno aplikací. Mezipaměť je hostovaný ve výkonné serverech musí běžet v Azure datacentra.
Tuto možnost, doporučuje se už a je k dispozici pouze pro podporu existující aplikace, které jste vytvořili používat. Pro všechny nové vývoj použijte Azure Redis mezipaměti.
>
> Kromě toho Azure podporuje, ukládání do mezipaměti v roli. Tato funkce umožňuje vytvořit mezipaměť, která je specifická do cloudové služby.
Mezipaměť je hostovaný aplikací instance webu nebo pracovního rolí a přístupný pouze podle rolí, které fungují v rámci stejné jednotka nasazení služby cloudu. (Jednotku nasazení je sada instancí role nasazené jako do cloudové služby do konkrétní oblasti.) Skupinový mezipaměti a všechny výskyty roli v rámci stejné jednotky nasazení, který je hostitelem mezipaměti budou stejné clusteru mezipaměti. Tuto možnost, doporučuje se už a je k dispozici pouze pro podporu existující aplikace, které jste vytvořili používat. Pro všechny nové vývoj použijte Azure Redis mezipaměti.
>
> Služba spravovaných mezipaměti Azure a Azure v roli mezipaměti jsou aktuálně slated na důchodové pojištění na 16 listopad 2016.
Je vhodné migrovat do mezipaměti Redis Azure při přípravě na tento důchodové pojištění. Další informace naleznete na stránce   [Co je Azure Redis mezipaměti nabízení a jakou velikost mám použít?](redis-cache/cache-faq.md#what-redis-cache-offering-and-size-should-i-use) na webu společnosti Microsoft.


### <a name="features-of-redis"></a>Funkce Redis

 Redis je větší než serveru jednoduché mezipaměti. Poskytuje distribuované databáze v paměti sadou rozsáhlé příkaz, který podporuje mnoho obvyklé scénáře. Toto jsou popsané dál v tomto dokumentu, v části Použití Redis ukládání do mezipaměti. V této části shrnuje některé klíčové funkce, které poskytuje Redis.

### <a name="redis-as-an-in-memory-database"></a>Redis jako databázi v paměti

Redis podporuje i čtení a zápis operace. V Redis můžete chráněny zápisy z selhání systému buď tak, že je pravidelně uložených v souboru místní snímku nebo v souboru protokolu připojovacím. To neplatí pro v mnoha mezipaměti, (které považovat za úložiště přechodně dat).

 Všechny zápisy jsou asynchronní a klienty z čtení a zápis dat nebudou blokovat. Kdy Redis spuštění systém, načte data ze souboru protokolu nebo snímku a používá vytvářet mezipaměti. Další informace najdete v tématu [Redis trvalé](http://redis.io/topics/persistence) na webu Redis.

> [AZURE.NOTE] Redis nezaručuje všechny zápisy budou uloženy v případě Katastrofální selhání, že v nejhorší může dojít ke ztrátě jenom několik sekund, než vejde dat. Mějte na paměti, mezipaměti není určená k následným akcím jako zdroj dat autoritativní a odpovídá aplikace, které používají mezipaměti zajistit, že důležitá data uložena úspěšně k úložišti příslušná data. Další informace najdete v tématu [mezipaměti odložené vzorku](http://msdn.microsoft.com/library/dn589799.aspx).

#### <a name="redis-data-types"></a>Redis datové typy

Redis klíč hodnotu úložiště, kde může obsahovat hodnoty typy jednoduché nebo složité struktury dat například hash, seznamy a sady. Podporuje množiny atomová operace na tyto datové typy. Klávesy může být trvalé nebo příznakem s omezenou time-to-live, v tomto okamžiku klávesu a odpovídající hodnotou se automaticky odeberou z mezipaměti. Další informace o Redis klíče a hodnoty navštivte stránku [Úvod k datovým typům a odběrů Redis](http://redis.io/topics/data-types-intro) na webu Redis.

#### <a name="redis-replication-and-clustering"></a>Replikace redis a clusterů

Redis podporuje předlohy/podřízeného replikace zajistit dostupnost a udržovat výkon. Zápis operace předlohy uzel Redis jsou replikovat na jeden nebo více podřízené uzly. Operace čtení můžou hodit předlohy nebo některou z podřízené.

V případě oddíl sítě podřízenými nadále sloužit dat a transparentně synchronizovat s hlavním, když znovu navázat spojení. Další informace naleznete na stránce [replikace](http://redis.io/topics/replication) na webu Redis.

Redis také poskytuje clusterů, které vám umožní transparentně oddílů dat do shards na serverech a šířit načíst. Tato funkce zvyšuje škálovatelnost, protože lze přidávat nové Redis servery a zvyšují data přeformátován jako velikost mezipaměti.

Kromě toho každý server clusteru replikovat pomocí předlohy/podřízeného replikace. Díky dostupnost v jednotlivých uzlech clusteru. Další informace o clusterů a sharding Navštěvujte blog na webu Redis [Redis výuková stránky obrázku](http://redis.io/topics/cluster-tutorial) .

### <a name="redis-memory-use"></a>Redis využití paměti

Mezipaměť Redis velikostí konečných, závisí na dostupných na hostitelském počítači zdrojů. Když nakonfigurujete Redis server, můžete určit maximální velikost paměti, které můžete použít. Můžete taky nakonfigurovat klíč v mezipaměti Redis mít vypršení platnosti dobu, po jejímž uplynutí se automaticky odeberou z mezipaměti. Tato funkce může pomoci zabránit plnění starých nebo zastaralá data mezipaměti.

Jak paměť zaplní, Redis můžete automaticky vyčistit klíče a jejich hodnoty tak, že následující počet zásady. Výchozí hodnota je LRU (nejméně nabízet), ale je taky možné vybrat jiné zásady ATP jeho vyloučení klíče náhodně vypnutí vyřazení úplně odebrat (ve kterých, změnu pokusí přidat položky do mezipaměti selhání, pokud je celé). Na stránce [Pomocí Redis jako LRU mezipaměť](http://redis.io/topics/lru-cache) s dalšími informacemi.

### <a name="redis-transactions-and-batches"></a>Redis transakce a listů

Redis umožňuje klientské aplikace předložit řadu operacích, které čtení a zápis dat do mezipaměti jako atomová transakce. Všechny příkazy v transakce je zaručena postupně spustit a bez příkazy jinými souběžné klienty bude vpleteno mezi nimi.

Tyto jsou však není true transakce relační databáze by provádět je. Zpracování transakcí se skládá ze dvou fází: – první při frontě příkazy a druhý je při spuštění příkazy. Příkaz fronty fázi příkazy, které tvoří transakce odeslány klientem. Pokud se vyskytne některé řazení chybová v tomto okamžiku (například syntaktickou chybu nebo nesprávný počet parametrů) pak Redis odmítne zpracuje celé transakce a zahodí ho.

Spuštění fázi Redis provede jednotlivé ve frontě příkazy v pořadí. Pokud příkaz nepovede v této fázi, Redis pokračovat další příkaz ve frontě a nevrací efekty některého z příkazů, které jste už spustili. Tento zjednodušené formulář transakce pomáhá spravovat výkon a vyhněte se problémy s výkonem, které jsou způsobená konflikty.

Redis implementace forma optimistické uzamčení pomáhají udržovat konzistenci. Podrobné informace o transakce a uzamčení s Redis naleznete na webu Redis [transakce stránky](http://redis.io/topics/transactions) .

Redis taky podporuje transakční dávky žádostí o. Protokol Redis klienti pomocí příkazů odešlete na server Redis klientovi zasílejte operace v rámci stejné žádost. To může pomoci zmenšit paketu fragmentace v síti. Při zpracování dávku každého příkazu probíhá. Pokud je některým z následujících příkazů poškozený, budou odmítnuta (které se nestane s transakce), ale proběhne zbývající příkazy. Není nijak zaručené o pořadí, ve kterém budou zpracovány příkazy na listu.

### <a name="redis-security"></a>Redis zabezpečení

Redis je věnovaných čistě poskytující rychlý přístup k datům a slouží k spouštějí v důvěryhodném prostředí, můžete k nim získat přístup jenom důvěryhodných klienty. Redis podporuje omezené zabezpečení model podle ověřování hesla. (Je možné odebrat ověřování úplně, i když to nedoporučujeme.)

Všechny ověřené klienty sdílet stejné heslo globální a mít přístup k stejné zdroje. V případě potřeby komplexnější zabezpečení přihlašovací musí implementovat vlastní vrstvy zabezpečení před Redis serveru a všechny žádosti klienta o by měl procházet tento další úroveň. Redis neměli přímo vystaven nedůvěryhodných nebo neověřených klientů.

Můžete omezit přístup k příkazům zakázáním je nebo přejmenování je (a zadáním pouze privilegovaných klienti s novým názvem).

Redis podporuje přímo nezpůsobují šifrování, tak všechny kódování musí provést klientské aplikace. Kromě toho Redis neposkytuje nezpůsobují transport cenného papíru. Pokud potřebujete ochraně dat jako toky v síti, doporučujeme provádění proxy serveru pro protokol SSL.

Další informace naleznete na webu Redis na stránce [Redis zabezpečení](http://redis.io/topics/security) .

> [AZURE.NOTE] Azure Redis mezipaměti obsahuje vlastní vrstva zabezpečení, přes který připojují klienti. Základní servery Redis nejsou přístupné veřejné sítě.

### <a name="using-the-azure-redis-cache"></a>Použití mezipaměti Azure Redis

Mezipaměť Redis Azure poskytuje přístup k serverům Redis umístěnými na serverech hostovaná u Azure datacentra; funguje jako fasádou poskytující řízení přístupu a zabezpečení. Zřízení mezipaměť pomocí portálu pro správu Azure. Na portálu přináší celou řadu předdefinované konfigurace od 53GB mezipaměti spuštěný jako vyhrazené služba, která podporuje protokol SSL komunikaci (soukromí) a hlavní/podřízeného replikace s SLA dostupnosti 99,9 % až 250 MB mezipaměti bez opakování (bez záruky dostupnosti) spuštěna sdílené hardwaru.

Na portálu Správa Azure, můžete taky nakonfigurovat zásady vyřazení mezipaměti a řízení přístupu do mezipaměti tím, že přidáte uživatele k rolím k dispozici; Vlastník skupiny přispěvatelů, Readeru. Tyto role definovat operace, které členové mohou provádět. Například členy této role vlastníka měli úplnou kontrolu nad do mezipaměti (včetně zabezpečení) a její obsah, členy této role přispěvatele můžete čtení a zápis informací do mezipaměti a členem role Čtenář můžete jenom načtení dat z mezipaměti.

Většina úkolů správy provádí pomocí portálu pro správu Azure a z tohoto důvodu spoustu dostupné ve standardní verzi aplikace Redis pro správu příkazy není k dispozici, včetně možnost upravit konfiguraci programově vypnutí serveru Redis konfigurace další slaves nebo vynutit uložení dat na disk.

Na portálu Správa Azure obsahuje pohodlný grafické zobrazení, které vám umožní sledovat výkon mezipaměti. Například můžete zobrazit počet připojení uskutečňuje počet požadavků provedených, objemu čtení a zápis, a počet mezipaměti narazí versus Neúspěšné přístupy do mezipaměti. Pomocí těchto informací můžete určit účinnosti mezipaměti a podle potřeby přepnout na jinou konfiguraci nebo změna zásad vyřazení. Kromě toho můžete vytvořit upozornění, které odesílají e-mailové zprávy Správce Pokud jedné nebo několika metrik kritické spadající mimo očekávaný rozsah. Například pokud počet neúspěšných mezipaměti přístupů překročí určitou hodnotu za poslední hodinu, Správce může být upozorněni mezipaměti může být moc malé nebo dat může být určený k vyloučení příliš rychlý.

Můžete také sledovat procesoru, paměti a použití sítě pro ukládání do mezipaměti.

Další informace a příklady znázorňující, jak můžete vytvořit a nakonfigurovat mezipaměti Redis Azure navštivte stránku [břišní kolem mezipaměti Redis Azure](https://azure.microsoft.com/blog/2014/06/04/lap-around-azure-redis-cache-preview/) na Azure blogu.

## <a name="caching-session-state-and-html-output"></a>Ukládání do mezipaměti stav relace a výstup ve formátu HTML

Pokud jste vytváření ASP.NET webových aplikací, spusťte role Azure web, můžete uložit informace o stavu relace a výstup do mezipaměti Redis Azure. Poskytovatele stavu relace pro Azure Redis mezipaměť umožňuje sdílet informace o relaci mezi jiné instance webové aplikace technologie ASP.NET a je velmi užitečné v situacích farmy web, kde klient server spřažení není k dispozici a ukládání do mezipaměti relace dat v paměti nebylo vhodné.

Použití poskytovatele stavu relace s Azure Redis mezipaměti nabízí několik výhod, včetně:

- Můžete sdílet stav relace mezi velkého počtu výskytů webové aplikace ASP.NET poskytuje lepší rozšiřitelnost
- Podporuje řízené, souběžné přístup k stejná data stavu relace pro více čtenářů a jeden Autor, a
- To umožňuje komprese ušetříte paměti a zlepšíte výkon sítě.

Další informace naleznete na stránce [Poskytovatele stavu relace ASP.NET mezipaměti Redis Azure](redis-cache/cache-aspnet-session-state-provider.md) na webu společnosti Microsoft.

> [AZURE.NOTE] Nepoužívejte poskytovatele stavu relace Azure Redis mezipaměti pro aplikace ASP.NET spuštěné mimo Azure prostředí. Latence přístupu k mezipaměť z mimo Azure vyloučit výkonu výhody ukládání dat do mezipaměti.

Podobně poskytovatele výstupní mezipaměti pro Azure Redis mezipaměti umožňuje uložit odpovědi HTTP vygenerované webové aplikace technologie ASP.NET. Použití poskytovatele výstupní mezipaměti s Azure Redis mezipaměti můžete vylepšit doby odezvy aplikací, které vykreslení složité výstup; instancí aplikace služby může být generování podobné odpovědi využívání neúplné sdílené výstupu do mezipaměti spíše než generování HTML výstup znovu.  Další informace naleznete na stránce [ASP.NET poskytovatele výstupní mezipaměti pro mezipaměti Redis Azure](redis-cache/cache-aspnet-output-cache-provider.md) na webu společnosti Microsoft.

### <a name="azure-redis-cache"></a>Azure Redis mezipaměti

Azure Redis mezipaměť poskytuje přístup k serverům Redis, které jsou hostované na Azure datacentra. Funguje jako fasádou poskytující řízení přístupu a zabezpečení. Zřízení mezipaměť pomocí portálu Azure.

Na portálu obsahuje několik předdefinovaných konfigurací. Tyto oblasti z mezipaměti 53 GB spuštěný jako vyhrazené služby, který podporuje SSL komunikaci (soukromí) a replikace předlohy/podřízeného s SLA 99,9 % dostupnost dolů 25 mezipaměti 0 MB bez opakování (bez záruky dostupnosti) spuštěna sdílené hardwaru.

Na portálu Azure, můžete taky konfigurace zásad vyřazení mezipaměti a řízení přístupu do mezipaměti tím, že přidáte uživatele k rolím k dispozici.  Tyto role, které definovat operace, které členové mohou provádět, patří vlastníka, přispěvatelů a Readeru. Například členy této role vlastníka měli úplnou kontrolu nad do mezipaměti (včetně zabezpečení) a její obsah, členy této role přispěvatele můžete čtení a zápis informací do mezipaměti a členem role Čtenář můžete jenom načtení dat z mezipaměti.

Většina úkolů správy provádí portálu Azure. Z tohoto důvodu mnoho pro správu příkazů, které jsou dostupné ve standardní verzi aplikace Redis nejsou k dispozici, včetně možnosti upravit konfiguraci programově, vypnout Redis server, konfigurovat další podřízené nebo vynutit ukládání dat na disk.

Portál Azure obsahuje pohodlný grafické zobrazení, které vám umožní sledovat výkon mezipaměti. Můžete třeba zobrazit počet připojení udělali, počet požadavků provádí, objemu čtení a zápis a počet přístupů do mezipaměti a Neúspěšné přístupy do mezipaměti. Pomocí těchto informací můžete zjistit působení mezipaměti a v případě potřeby přepnout na jinou konfiguraci nebo změna zásad vyřazení.

Kromě toho můžete vytvořit upozornění, které odesílají e-mailové zprávy Správce Pokud jedné nebo několika metrik kritické spadající mimo očekávaný rozsah. Můžete třeba upozornit správce, pokud počet neúspěšných přístupů mezipaměti překročí určitou hodnotu za poslední hodinu, protože znamená mezipaměti můžou být moc malé nebo dat může být určený k vyloučení příliš rychlý.

Taky můžete sledovat procesoru, paměti a použití sítě pro ukládání do mezipaměti.

Další informace a příklady znázorňující, jak můžete vytvořit a nakonfigurovat mezipaměti Redis Azure navštivte stránku [břišní kolem mezipaměti Redis Azure](https://azure.microsoft.com/blog/2014/06/04/lap-around-azure-redis-cache-preview/) na Azure blogu.

## <a name="caching-session-state-and-html-output"></a>Ukládání do mezipaměti stav relace a výstup ve formátu HTML

Pokud vytváříte ASP.NET webových aplikací, spusťte role Azure web, můžete uložit relace uveďte informace a HTML výstupu do mezipaměti Redis Azure. Poskytovatele stavu relace pro Azure Redis mezipaměť umožňuje sdílet informace o relaci mezi jiné instance webové aplikace technologie ASP.NET a je velmi užitečné v situacích farmy web, kde klient server spřažení není k dispozici a ukládání do mezipaměti relace dat v paměti nebylo vhodné.

Použití poskytovatele stavu relace s Azure Redis mezipaměti nabízí několik výhod, včetně:

- Stav relace sdílení s velkým počtem výskyty ASP.NET webových aplikací.
- Poskytování rozšiřitelnost.
- Podpora kontrolovanou, souběžné přístup k stejná data stavu relace pro více čtenářů a jeden Autor.
- Pomocí komprese pro ukládání paměti a zlepšit výkon sítě.

Další informace naleznete na stránce [poskytovatele stavu relace ASP.NET mezipaměti Redis Azure](redis-cache/cache-aspnet-session-state-provider.md) na webu společnosti Microsoft.

> [AZURE.NOTE] Nepoužívejte poskytovatele stavu relace pro mezipaměť Redis Azure pomocí aplikace ASP.NET spuštěné mimo Azure prostředí. Latence přístupu k mezipaměť z mimo Azure vyloučit výkonu výhody ukládání dat do mezipaměti.

Podobně poskytovatele výstupní mezipaměti pro Azure Redis mezipaměti umožňuje uložit odpovědi HTTP vygenerované webové aplikace technologie ASP.NET. Použití poskytovatele výstupní mezipaměti s Azure Redis mezipaměti můžete vylepšit doby odezvy aplikací, které vykreslení složité výstup ve formátu HTML. Může být instancí aplikace služby, které mohou generovat podobné odpovědi využívání neúplné sdílené výstupu do mezipaměti spíše než generování HTML výstup znovu. Další informace naleznete na stránce [ASP.NET poskytovatele výstupní mezipaměti pro mezipaměti Redis Azure](redis-cache/cache-aspnet-output-cache-provider.md) na webu společnosti Microsoft.

## <a name="building-a-custom-redis-cache"></a>Vytvoření vlastní Redis mezipaměti

Azure Redis mezipaměti funguje jako fasádou podkladového serverům Redis. Nyní podporuje pevnou sadu konfigurace ale neposkytuje Redis clusterů. Pokud požadujete Upřesnit konfiguraci, které nejsou zahrnuty Azure Redis mezipaměti (například mezipaměť větší než 53 GB) můžete vytvářet a hostovat serverech Redis pomocí Azure virtuálních počítačích.

To je potenciálně složitý proces, protože může být potřeba vytvořit několik VMs má fungovat jako hlavní a podřízené uzly, pokud chcete provádět replikace. Kromě toho pokud chcete vytvořit clusteru, musíte více předloh a podřízené servery. Topologie minimální skupinový replikace, která obsahuje vysoký stupeň dostupnost a škálovatelnost obsahuje aspoň šest VMs uspořádané tří dvojic předlohy/podřízeného serverů (clusteru musí obsahovat aspoň tři hlavní uzly).

Každou dvojici předlohy/podřízeného by měl nachází těsně minimalizovat latence. Každou sadu dvojice můžete však běžet v různých Azure datacentrech nachází v různých oblastech, pokud chcete najít data uložená v mezipaměti Zavřít aplikace, které se používá nejčastěji. Stránku [Systém Redis na OM Linux CentOS v Azure](http://blogs.msdn.com/b/tconte/archive/2012/06/08/running-redis-on-a-centos-linux-vm-in-windows-azure.aspx) na web Microsoft provede příklad, který ukazuje, jak můžete vytvořit a nakonfigurovat Redis uzel spuštěný jako OM Azure.

[AZURE.NOTE] Všimněte si, že pokud implementovat vlastní Redis mezipaměti tímto způsobem, můžete je odpovědná za jednotlivé sledování, Správa a zabezpečení služby.

## <a name="partitioning-a-redis-cache"></a>Rozdělení Redis mezipaměti

Rozdělení mezipaměti zahrnuje rozdělení mezipaměti do více počítačů. Tuto strukturu nabízí několik výhod pomocí serveru jednoho mezipaměti, včetně:

- Vytvoření mezipaměti, které je mnohem větší než mohou být uloženy na jednom serveru.
- Distribuci dat na serverech zlepšit dostupnost. Pokud jeden server selže nebo nedostupné, data, která obsahuje není dostupný, ale data zbývající serverech můžete pořád k nim získat přístup. Pro mezipaměť to totiž není důležitá data uložená v mezipaměti pouze přechodná kopii data, která se uskutečňuje v databázi. Data uložená v mezipaměti na serveru, který nedostupné můžete místo toho mezipaměti na jiném serveru.
- Na serverech, tím lepší výkon a škálovatelnost šíření načíst.
- Geolocating data zavřete uživatelům, kteří přístup, čímž se zmenší latence.

Nejběžnější formulář rozdělení pro mezipaměť, je sharding. V této strategie je každý oddíl (nebo shard) mezipaměť Redis vlastní vpravo. Data přesměrován konkrétní oddíl pomocí sharding použití logických operátorů, které můžete použít různé přístupy k distribuci data. Další informace o implementaci sharding obsahuje [Sharding vzorku](http://msdn.microsoft.com/library/dn589797.aspx) .

Provádět oddílů v mezipaměti Redis můžete provést jednu z následujících postupů:

- _Směrování dotazu na straně serveru._ V tento postup klientské aplikaci odešle žádost o jednotlivých Redis servery, které přispívají do mezipaměti (pravděpodobně nejbližší server). Každý Redis server ukládá metadata, která popisují oddílu jej obsahuje a obsahuje také informace o tom, které jsou umístěné oddíly na jiné servery. Redis server kontroluje požadavek klienta. Pokud můžete vyřešit místně, provede požadované operace. V opačném případě předá požadavek na příslušný server. Tento model prováděn Redis clusterů a se podrobněji píše na stránce [Redis kurz obrázku](http://redis.io/topics/cluster-tutorial) na webu Redis. Redis clusterů je průhledné na klientské aplikace a další servery Redis můžete přidat clusteru (a data znovu oddíly) bez nutnosti překonfigurovat klienty.

- _Klientské rozdělení._ V tomto modelu klientské aplikaci obsahuje logiku (případně ve formě knihovny), který směruje požadavky na příslušný Redis server. Tento postup lze použít s Azure Redis mezipaměti. Vytvoření více Azure Redis mezipaměti (jedno pro každý oddíl dat) a implementace logiky klienta, který přesměrovává žádosti správné mezipaměti. Pokud schématu rozdělení oddílů změny (pokud další Azure Redis mezipaměti se vytvářejí, například), třeba může klientské aplikace překonfigurovat.

- _Proxy operátorem rozdělení._ V tomto schématu Redis klient aplikace odeslat žádosti o služby Zprostředkovatel proxy, které rozpozná, jak se oddíly data a směruje požadavek na příslušné serveru. Tento přístup je rovněž možné používat s Azure Redis mezipaměti; služba proxy lze provést jako Azure cloudové služby. Tento postup vyžaduje další úroveň složitost implementovat službu a žádosti o může trvat déle než pomocí rozdělení klienta.

Na stránce [Partitioning: jak rozdělit dat mezi několika instancích spuštěných Redis](http://redis.io/topics/partitioning) na Redis webu poskytuje další informace o implementaci rozdělení s Redis.

### <a name="implement-redis-cache-client-applications"></a>Implementace Redis mezipaměti klientské aplikace

Redis podporuje klientské aplikace napsané v mnoha jazyky. Pokud vytváříte nové aplikace pomocí .NET Framework, doporučuje používat knihovnu StackExchange.Redis klienta. Tuto knihovnu obsahuje .NET Framework objektového modelu, který abstrahuje podrobnosti o připojení k serveru Redis, příkazy odesílání a přijímání odpovědi. Je k dispozici ve Visual Studiu jako balíček NuGet. Připojení k mezipaměti Redis Azure nebo vlastní mezipaměti Redis hostitelem virtuálního počítače můžete použít tuto stejnou knihovnu.

Připojení k serveru Redis použít statickou `Connect` metodu `ConnectionMultiplexer` předmětu. Připojení, které vytvoří tento způsob je určen po celou dobu klientské aplikace a stejné připojení může používat více vláken souběžné. Nechcete připojení a odpojení pokaždé, když provedení operace Redis, protože to snížit výkon.

Můžete zadat parametry připojení, například adresu hostiteli Redis a heslo. Pokud používáte Azure Redis mezipaměti, heslo je buď primární a sekundární klávesu vygenerovaný mezipaměti Redis Azure pomocí portálu pro správu Azure.

Po připojení k serveru Redis, můžete získat ty Redis databázi, která funguje jako mezipaměti. Připojení Redis poskytuje `GetDatabase` způsob, jak to udělat. Můžete načítat položky z mezipaměti a uložit data v mezipaměti pomocí `StringGet` a `StringSet` metody. Tyto metody očekávat klíč jako parametr a vrátit položku v mezipaměti, které obsahuje odpovídající hodnoty (`StringGet`) nebo přidejte položku do mezipaměti k tomuto klíči (`StringSet`).

V závislosti na umístění serveru Redis mnoha operací může být některé latence účtovány během žádost o odeslaných na server a odpověď je vrácena ke klientovi. Knihovna StackExchange poskytuje asynchronní verze spoustu metody, které poskytuje pomůže zůstat neodpovídá klientské aplikace. Tyto metody podporují [Založeným na úkolech asynchronní](http://msdn.microsoft.com/library/hh873175.aspx) .NET Framework.

Následující fragment kódu ukazuje metodu pojmenovanou `RetrieveItem`. Ukazuje implementace mezipaměti odložené vzoru na základě Redis a knihovně StackExchange. Metoda hodnotu řetězce klíčů, která bude a pokusí se načíst odpovídající položku z mezipaměti Redis tak, že zavoláte `StringGetAsync` metoda (asynchronní verzi `StringGet`).

Pokud není nalezen položky, je načtena z podkladového zdroje dat, pomocí `GetItemFromDataSourceAsync` (což je místní metodu a nejsou součástí knihovnu StackExchange). Je pak přidán do mezipaměti pomocí `StringSetAsync` metoda tak, aby je lze získat rychleji příště.

```csharp
// Connect to the Azure Redis cache
ConfigurationOptions config = new ConfigurationOptions();
config.EndPoints.Add("<your DNS name>.redis.cache.windows.net");
config.Password = "<Redis cache key from management portal>";
ConnectionMultiplexer redisHostConnection = ConnectionMultiplexer.Connect(config);
IDatabase cache = redisHostConnection.GetDatabase();
...
private async Task<string> RetrieveItem(string itemKey)
{
    // Attempt to retrieve the item from the Redis cache
    string itemValue = await cache.StringGetAsync(itemKey);

    // If the value returned is null, the item was not found in the cache
    // So retrieve the item from the data source and add it to the cache
    if (itemValue == null)
    {
        itemValue = await GetItemFromDataSourceAsync(itemKey);
        await cache.StringSetAsync(itemKey, itemValue);
    }

    // Return the item
    return itemValue;
}
```

`StringGet` a `StringSet` metody nejsou omezené načítání nebo ukládání textových hodnot. Můžou používat všechny položky, která je serializován jako pole bajtů. Pokud potřebujete uložit objektu .NET, můžete serializovat jako tok bajtů a používat `StringSet` metoda zapisovat do mezipaměti.

Podobně si můžete přečíst objektu z mezipaměti pomocí `StringGet` metoda a převodu ze sériového tvaru jako objekt .NET. Následující kód ukazuje sadu rozšíření metod pro rozhraní IDatabase ( `GetDatabase` metoda připojení Redis vrátí `IDatabase` objekt) a ukázkový kód, který používá těchto metod pro čtení a zápis `BlogPost` objektu do mezipaměti:

```csharp
public static class RedisCacheExtensions
{
    public static async Task<T> GetAsync<T>(this IDatabase cache, string key)
    {
        return Deserialize<T>(await cache.StringGetAsync(key));
    }

    public static async Task<object> GetAsync(this IDatabase cache, string key)
    {
        return Deserialize<object>(await cache.StringGetAsync(key));
    }

    public static async Task SetAsync(this IDatabase cache, string key, object value)
    {
        await cache.StringSetAsync(key, Serialize(value));
    }

    static byte[] Serialize(object o)
    {
        byte[] objectDataAsStream = null;

        if (o != null)
        {
            BinaryFormatter binaryFormatter = new BinaryFormatter();
            using (MemoryStream memoryStream = new MemoryStream())
            {
                binaryFormatter.Serialize(memoryStream, o);
                objectDataAsStream = memoryStream.ToArray();
            }
        }

        return objectDataAsStream;
    }

    static T Deserialize<T>(byte[] stream)
    {
        T result = default(T);

        if (stream != null)
        {
            BinaryFormatter binaryFormatter = new BinaryFormatter();
            using (MemoryStream memoryStream = new MemoryStream(stream))
            {
                result = (T)binaryFormatter.Deserialize(memoryStream);
            }
        }

        return result;
    }
}
```

Následující kód ukazuje metodu s názvem `RetrieveBlogPost` používající těchto postupů rozšíření pro čtení a zápis serializovatelný `BlogPost` objektu do mezipaměti sledovat vzorek mezipaměti upravující:

```csharp
// The BlogPost type
[Serializable]
private class BlogPost
{
    private HashSet<string> tags = new HashSet<string>();

    public BlogPost(int id, string title, int score, IEnumerable<string> tags)
    {
        this.Id = id;
        this.Title = title;
        this.Score = score;
        this.tags = new HashSet<string>(tags);
    }

    public int Id { get; set; }
    public string Title { get; set; }
    public int Score { get; set; }
    public ICollection<string> Tags { get { return this.tags; } }
}
...
private async Task<BlogPost> RetrieveBlogPost(string blogPostKey)
{
    BlogPost blogPost = await cache.GetAsync<BlogPost>(blogPostKey);
    if (blogPost == null)
    {
        blogPost = await GetBlogPostFromDataSourceAsync(blogPostKey);
        await cache.SetAsync(blogPostKey, blogPost);
    }

    return blogPost;
}
```

Redis podporuje příkaz kanálů klientské aplikaci pošle více asynchronní požadavků. Redis můžete multiplex žádosti o použití stejné připojení spíše než přijímání zpráv a reagování na příkazy v sekvenci striktních.

Tento přístup pomůže snížit latence efektivnější využívání sítě. Následující fragment kódu příklad, který načítá podrobnosti o dva zákazníci současně. Kód odešle dva požadavky a pak provede nějakého jiného zpracování (není vidět) před očekáváte výsledky. `Wait` Metoda objektu mezipaměti je podobná .NET Framework `Task.Wait` metodu:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
var task1 = cache.StringGetAsync("customer:1");
var task2 = cache.StringGetAsync("customer:2");
...
var customer1 = cache.Wait(task1);
var customer2 = cache.Wait(task2);
```

Na stránce [si přečtěte následující dokumentaci mezipaměti Redis Azure](https://azure.microsoft.com/documentation/services/cache/) na webu Microsoft poskytuje další informace o tom, jak napsat klientské aplikace, které můžete použít mezipaměť Redis Azure. Další informace najdete na [základní použití stránky](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Basics.md) na webu StackExchange.Redis.

Další informace o asynchronních operací a kanálů s Redis a knihovně StackExchange obsahuje stránky [kanály a multiplexers](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/PipelinesMultiplexers.md) na stejném webu.  Dál v tomto článku se pomocí Redis ukládání do mezipaměti, jsou uvedeny příklady některých složitější postupy, které můžete použít k ukládání do mezipaměti Redis dat.

## <a name="using-redis-caching"></a>Použití Redis ukládání do mezipaměti

Nejjednodušší použití Redis pro ukládání do mezipaměti pochybnosti je klíč dvojice, jejichž hodnota je neinterpretované řetězec libovolné délky, která může obsahovat binární data. (Je v podstatě matice počet bajtů, které lze považovat za řetězec). Tento scénář je znázorněn v klientských aplikacích implementace Redis mezipaměti oddílu dříve v tomto článku.

Všimněte si, že také klíče obsahují neinterpretované data, abyste mohli používat všechny binární informace jako klíč. Čím delší klíčem je však víc místa bude trvat k ukládání a tím déle bude trvat vyhledávání operací. Pro zvýšení použitelnosti a snadné údržbu návrhu vaší keyspace opatrně a klávesami smysluplné (ale ne podrobného).

Například klávesami strukturovaných například "zákazník: 100" představují klíč pro zákazníka s ID 100 spíše než jednoduše "100". Tohoto režimu umožňuje snadno rozlišení mezi hodnoty, které obsahují různé datové typy. Pomocí klávesy "objednávky: 100" například může taky představují klíč pro pořadí s ID 100.

Kromě jednorozměrné binární řetězce můžou obsahovat hodnoty v pár klíče hodnoty Redis taky další strukturované informace, včetně seznamů, nastaví (seřazené a neseřazené) a vytvoří hodnotu hash. Redis obsahuje sadu komplexní příkazů, které tyto typy a spoustu tyto příkazy jsou k dispozici pro .NET Framework aplikace přes klienta knihovnu například StackExchange. Na stránce [Úvod k datovým typům a odběrů Redis](http://redis.io/topics/data-types-intro) na webu Redis poskytuje podrobnější přehled těchto typů a příkazy, které můžete použít s nimi pracovat.

V této části shrnuje některé běžné případy použití pro tyto datových typů a příkazy.

### <a name="perform-atomic-and-batch-operations"></a>Provedení atomová a dávkové operace

Redis podporuje řadu atomová get a set operace textových hodnot. Tyto operace odebrat nebezpečí možné závodní probíhajících může při použití samostatné `GET` a `SET` příkazy. Operace, které jsou k dispozici, patří:

- `INCR`, `INCRBY`, `DECR`, a `DECRBY`, které provádí atomová zvýší a sníží operace s hodnotami číselná data celé číslo. Knihovna StackExchange poskytuje přetížených verzí `IDatabase.StringIncrementAsync` a `IDatabase.StringDecrementAsync` metody provádět tyto operace a vrátí výslednou hodnotu, která je uložená v mezipaměti. Následující fragment kódu ukazuje, jak pomocí těchto postupů:

  ```csharp
  ConnectionMultiplexer redisHostConnection = ...;
  IDatabase cache = redisHostConnection.GetDatabase();
  ...
  await cache.StringSetAsync("data:counter", 99);
  ...
  long oldValue = await cache.StringIncrementAsync("data:counter");
  // Increment by 1 (the default)
  // oldValue should be 100

  long newValue = await cache.StringDecrementAsync("data:counter", 50);
  // Decrement by 50
  // newValue should be 50
  ```

- `GETSET`, které vyhledá hodnotu, který máte přidružený k klíče a změní na novou hodnotu. Knihovna StackExchange zpřístupní operaci prostřednictvím `IDatabase.StringGetSetAsync` metody. Fragment kódu znázorňuje příklad této metody. Tento kód vrací aktuální hodnotu, kterou máte přidruženou s klávesou "data: čítač" z předchozího příkladu. Potom obnoví hodnotu pro tento klíč zpět na hodnotu nula všech jako součást stejnou operaci:

  ```csharp
  ConnectionMultiplexer redisHostConnection = ...;
  IDatabase cache = redisHostConnection.GetDatabase();
  ...
  string oldValue = await cache.StringGetSetAsync("data:counter", 0);
  ```

- `MGET`a `MSET`, který vrátí nebo změna sady řetězcových hodnot jako jediná operace. `IDatabase.StringGetAsync` a `IDatabase.StringSetAsync` metody jsou přetížené podporuje tuto funkci, jak je vidět v následujícím příkladu:

  ```csharp
  ConnectionMultiplexer redisHostConnection = ...;
  IDatabase cache = redisHostConnection.GetDatabase();
  ...
  // Create a list of key-value pairs
  var keysAndValues =
      new List<KeyValuePair<RedisKey, RedisValue>>()
      {
          new KeyValuePair<RedisKey, RedisValue>("data:key1", "value1"),
          new KeyValuePair<RedisKey, RedisValue>("data:key99", "value2"),
          new KeyValuePair<RedisKey, RedisValue>("data:key322", "value3")
      };

  // Store the list of key-value pairs in the cache
  cache.StringSet(keysAndValues.ToArray());
  ...
  // Find all values that match a list of keys
  RedisKey[] keys = { "data:key1", "data:key99", "data:key322"};
  RedisValue[] values = null;
  values = cache.StringGet(keys);
  // values should contain { "value1", "value2", "value3" }
  ```

Můžete také sloučit do jedné transakce Redis více operací podle popisu v části Redis transakce a listy dříve v tomto článku. Knihovna StackExchange podporuje transakce prostřednictvím `ITransaction` rozhraní.

Vytvoření `ITransaction` pomocí objektu `IDatabase.CreateTransaction` metody. Pomocí metody poskytované vyvoláte příkazy pro transakce `ITransaction` objektu.

`ITransaction` Rozhraní poskytuje přístup k sadě metody se podobají profilům kliknutím `IDatabase` rozhraní, s tím rozdílem, že jsou všechny metody asynchronní. To znamená, že jsou pouze provést, když `ITransaction.Execute` metoda. Hodnota, která vám vrátil `ITransaction.Execute` metoda označuje, zda byl úspěšně vytvořen transakce (pravda) nebo pokud ale bezúspěšně (NEPRAVDA).

Následující fragment kódu ukázka dvou čítače této částech a sníží v rámci stejné transakce:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
ITransaction transaction = cache.CreateTransaction();
var tx1 = transaction.StringIncrementAsync("data:counter1");
var tx2 = transaction.StringDecrementAsync("data:counter2");
bool result = transaction.Execute();
Console.WriteLine("Transaction {0}", result ? "succeeded" : "failed");
Console.WriteLine("Result of increment: {0}", tx1.Result);
Console.WriteLine("Result of decrement: {0}", tx2.Result);
```

Mějte na paměti, že budou transakce Redis na rozdíl od transakce v relační databáze. `Execute` Metoda jednoduše fronty všechny příkazy, které tvoří transakce proběhnout a pokud některé z nich je poškozený transakce ukončit. Pokud úspěšně frontě všechny příkazy, každý příkaz spustí asynchronní.

Když libovolnému příkazu nepovede, ostatní pořád pokračovat ve zpracování. Pokud potřebujete k ověření, že byla úspěšně dokončena příkazu, musí načíst výsledky příkazu pomocí vlastnost **výsledek** odpovídající úkolu, jak je vidět v předchozím příkladu. Čtení vlastnost **výsledek** blokuje volání vlákna až do dokončení úkolu.

Další informace naleznete na stránce [transakce v Redis](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Transactions.md) na webu StackExchange.Redis.

Při provádění operací dávku, můžete použít `IBatch` rozhraní StackExchange knihovny. Toto rozhraní poskytuje přístup k sadě metod podobají profilům kliknutím `IDatabase` rozhraní, s tím rozdílem, že jsou všechny metody asynchronní.

Vytvoření `IBatch` pomocí objektu `IDatabase.CreateBatch` metoda a spusťte dávku pomocí `IBatch.Execute` způsob, jak je vidět v následujícím příkladu. Tento kód jednoduše nastaví hodnotu řetězce, částech a sníží stejné čítače použité v předchozím příkladu a zobrazí výsledky:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
IBatch batch = cache.CreateBatch();
batch.StringSetAsync("data:key1", 11);
var t1 = batch.StringIncrementAsync("data:counter1");
var t2 = batch.StringDecrementAsync("data:counter2");
batch.Execute();
Console.WriteLine("{0}", t1.Result);
Console.WriteLine("{0}", t2.Result);
```

Je důležité pochopit rozdíl od transakce, pokud příkaz v jedné dávce selhala, protože je poškozený, další příkazy může pořád bezchybnou. `IBatch.Execute` Metoda nevrací žádné údaj o úspěšném nebo neúspěšném řešení.

### <a name="perform-fire-and-forget-cache-operations"></a>Provedení fire a zapomenete operace mezipaměti

Redis podporuje fire a zapomenete operace pomocí příkazu příznaků. V takovém případě klient jednoduše spustí operaci, ale má žádný úrok ve výsledku a nečeká příkazu k dokončení. Následující příklad ukazuje, jak provedení příkazu INCR jako fire a operace v žádném případě nezapomeňte:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
await cache.StringSetAsync("data:key1", 99);
...
cache.StringIncrement("data:key1", flags: CommandFlags.FireAndForget);
```

### <a name="specify-automatically-expiring-keys"></a>Určete automaticky vypršení jejich platnosti klíče

Při ukládání položky v mezipaměti Redis můžete určit vypršení časového limitu po jejímž uplynutí položky se automaticky odeberou z mezipaměti. Můžete taky dotaz, kolik delší dobu klíč nemá před vypršením jeho platnosti pomocí `TTL` příkaz. Tento příkaz je k dispozici pro StackExchange aplikace pomocí `IDatabase.KeyTimeToLive` metody.

Následující fragment kódu ukazuje, jak nastavit dobu platnosti 20 sekund ke klíči a dotaz životnost klíče:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
// Add a key with an expiration time of 20 seconds
await cache.StringSetAsync("data:key1", 99, TimeSpan.FromSeconds(20));
...
// Query how much time a key has left to live
// If the key has already expired, the KeyTimeToLive function returns a null
TimeSpan? expiry = cache.KeyTimeToLive("data:key1");
```

Můžete také nastavit dobu platnosti určité datum a čas pomocí příkazu vypršení platnosti, který je dostupný v knihovně StackExchange jako `KeyExpireAsync` metodu:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
// Add a key with an expiration date of midnight on 1st January 2015
await cache.StringSetAsync("data:key1", 99);
await cache.KeyExpireAsync("data:key1",
    new DateTime(2015, 1, 1, 0, 0, 0, DateTimeKind.Utc));
...
```

> _Tip:_ Položky můžete ručně odeberte z mezipaměti pomocí příkazu DEL, což je k dispozici prostřednictvím knihovnu StackExchange jako `IDatabase.KeyDeleteAsync` metody.

### <a name="use-tags-to-cross-correlate-cached-items"></a>Pomocí značek korelace více položek v mezipaměti

Sada Redis najdete více položek, které sdílejí jediného klíče. Vytvořit sadu pomocí příkazu SADD. Pomocí příkazu SMEMBERS lze načítat položky v sadě. Knihovna StackExchange implementuje příkaz SADD s `IDatabase.SetAddAsync` metoda a SMEMBERS příkaz s `IDatabase.SetMembersAsync` metody.

Můžete také sloučit existující sady a vytvoření nové sady pomocí SDIFF (nastavení rozdíl), AGLOMERAČNÍCH (nastavení průsečíku) a příkazy SUNION (unie nastavení). Knihovna StackExchange sjednocuje tyto operace v `IDatabase.SetCombineAsync` metody. První parametr tato metoda určuje operaci sadu provádět.

Následující fragmenty kódu ukazují, jak může být užitečné pro rychlé ukládání a načítání kolekce souvisejících položek sady. Tento kód používá `BlogPost` typ, který byl popsaných v oddílu implementace Redis mezipaměti klientské aplikace dříve v tomto článku.

A `BlogPost` objekt obsahuje čtyřmi poli – ID, název, jejich pořadí skóre a sbírka značky. První fragment kódu dole zobrazuje ukázková data, který se používá k naplnění C# seznam `BlogPost` objekty:

```csharp
List<string[]> tags = new List<string[]>()
{
    new string[] { "iot","csharp" },
    new string[] { "iot","azure","csharp" },
    new string[] { "csharp","git","big data" },
    new string[] { "iot","git","database" },
    new string[] { "database","git" },
    new string[] { "csharp","database" },
    new string[] { "iot" },
    new string[] { "iot","database","git" },
    new string[] { "azure","database","big data","git","csharp" },
    new string[] { "azure" }
};

List<BlogPost> posts = new List<BlogPost>();
int blogKey = 0;
int blogPostId = 0;
int numberOfPosts = 20;
Random random = new Random();
for (int i = 0; i < numberOfPosts; i++)
{
    blogPostId = blogKey++;
    posts.Add(new BlogPost(
        blogPostId,               // Blog post ID
        string.Format(CultureInfo.InvariantCulture, "Blog Post #{0}",
            blogPostId),          // Blog post title
        random.Next(100, 10000),  // Ranking score
        tags[i % tags.Count]));   // Tags--assigned from a collection
                                  // in the tags list
}
```

U každé mohou být uloženy značky `BlogPost` objektu jako sady v mezipaměti Redis a každou sadu přidružit ID `BlogPost`. Díky aplikaci můžete rychle najít všechny značky, které patří k určitému blogovému příspěvku. Povolit hledání v opačném směru a najděte všechny příspěvky na blogu, která sdílejí určité značky, můžete vytvořit další dvojice obsahující odkazování na ID značky v klíči příspěvky blogu:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
// Tags are easily represented as Redis Sets
foreach (BlogPost post in posts)
{
    string redisKey = string.Format(CultureInfo.InvariantCulture,
        "blog:posts:{0}:tags", post.Id);
    // Add tags to the blog post in Redis
    await cache.SetAddAsync(
        redisKey, post.Tags.Select(s => (RedisValue)s).ToArray());

    // Now do the inverse so we can figure how which blog posts have a given tag
    foreach (var tag in post.Tags)
    {
        await cache.SetAddAsync(string.Format(CultureInfo.InvariantCulture,
            "tag:{0}:blog:posts", tag), post.Id);
    }
}
```

Tyto struktury umožňují velmi efektivně mnoho běžných dotazů. Můžete například najít a zobrazit všechny značky k blogovému příspěvku 1 takto:

```csharp
// Show the tags for blog post #1
foreach (var value in await cache.SetMembersAsync("blog:posts:1:tags"))
{
    Console.WriteLine(value);
}
```

Můžete najít všechny značky, která jsou společná pro blog pokládat dotazy k 1 a blogu příspěvek 2 tak operace sadu průniku, následujícím způsobem:

```csharp
// Show the tags in common for blog posts #1 and #2
foreach (var value in await cache.SetCombineAsync(SetOperation.Intersect, new RedisKey[]
    { "blog:posts:1:tags", "blog:posts:2:tags" }))
{
    Console.WriteLine(value);
}
```

A můžete vyhledat všechny příspěvky, které obsahují určité značky:

```csharp
// Show the ids of the blog posts that have the tag "iot".
foreach (var value in await cache.SetMembersAsync("tag:iot:blog:posts"))
{
    Console.WriteLine(value);
}
```

### <a name="find-recently-accessed-items"></a>Vyhledání nedávno použitých položek

Běžné úkoly nutné z mnoha aplikací je najít nejčastěji nedávno použitých položek. Blogování webu může chcete zobrazit informace o naposledy čtení příspěvků na blogu.

Tuto funkci lze implementovat pomocí seznamu Redis. Seznam Redis obsahuje více položek, které sdílení stejného klíče. V seznamu funguje jako fronty dvojité ukončen. Položky můžete použít pro buď konec seznamu pomocí příkazů (vpravo nabízená) RPUSH a LPUSH (levé nabízená). Pomocí příkazů LPOP a RPOP lze načítat položky z obou konec seznamu. Sada prvků můžete vrátit taky pomocí příkazů LRANGE a USPOŘÁDAT.

Dole fragmenty kódu ukazují, jak můžete dělat tyto operace pomocí knihovnu StackExchange. Tento kód používá `BlogPost` typ z předchozích příkladů. Příspěvek na blogu číst uživatelem, `IDatabase.ListLeftPushAsync` metoda posune název příspěvku na blogu do seznamu, který máte přidružený k klávesu "blog:recent_posts" v mezipaměti Redis.

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
string redisKey = "blog:recent_posts";
BlogPost blogPost = ...; // Reference to the blog post that has just been read
await cache.ListLeftPushAsync(
    redisKey, blogPost.Title); // Push the blog post onto the list
```

Jak se budou číst další příspěvků na blogu, jejich názvy odesílají do stejného seznamu. Seznam podle pořadí, ve kterém se přidaly názvy. Naposledy čtení příspěvků na blogu jsou na levém konci seznamu. (Pokud stejné příspěvek na blog čte víckrát, bude mít více položek v seznamu.)

Zobrazení názvů naposledy čtení příspěvků pomocí `IDatabase.ListRange` metody. Tento způsob trvá klávesy, která obsahuje seznam, výchozí bod a koncového bodu. Následující kód načte názvů 10 příspěvků na blogu (položky od 0 až 9) na konci úplně vlevo v seznamu:

```csharp
// Show latest ten posts
foreach (string postTitle in await cache.ListRangeAsync(redisKey, 0, 9))
{
    Console.WriteLine(postTitle);
}
```

Všimněte si, že `ListRangeAsync` metoda neodebírat položek ze seznamu. K tomuto účelu použijte `IDatabase.ListLeftPopAsync` a `IDatabase.ListRightPopAsync` metody.

Zabránit Kvetoucí donekonečna udržovat seznamu, můžete pravidelně jatečné položky ořízne seznamu. Fragment kódu se dozvíte, jak odebrat všechny ale pět úplně vlevo položek ze seznamu:

```csharp
await cache.ListTrimAsync(redisKey, 0, 5);
```

### <a name="implement-a-leader-board"></a>Implementace vývěsky vodicí znak

Ve výchozím nastavení nejsou v určitém pořadí uskutečňuje položek sady. Můžete vytvořit uspořádanou sadu pomocí příkazu ZADD ( `IDatabase.SortedSetAdd` metoda v knihovně StackExchange). Jsou položky seřazené pomocí číselné hodnoty s názvem skóre, které je k dispozici jako parametr příkazu.

Následující fragment kódu přidá název příspěvku blogu uspořádaných seznamů. V tomto příkladu každý příspěvek na blog také skóre pole obsahuje, která obsahuje hodnocení příspěvku na blogu.

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
string redisKey = "blog:post_rankings";
BlogPost blogPost = ...; // Reference to a blog post that has just been rated
await cache.SortedSetAddAsync(redisKey, blogPost.Title, blogpost.Score);
```

Je možné načíst blogu příspěvek nadpisy a dosažených skóre vzestupně pomocí `IDatabase.SortedSetRangeByRankWithScores` metodu:

```csharp
foreach (var post in await cache.SortedSetRangeByRankWithScoresAsync(redisKey))
{
    Console.WriteLine(post);
}
```

> [AZURE.NOTE] Knihovna StackExchange také poskytuje `IDatabase.SortedSetRangeByRankAsync` metodu, která vrací data popořádku skóre, ale nevrací výsledky.

Můžete taky načítat položky v sestupném pořadí výsledků a omezení počtu položek, které vracejí zadáním další parametry `IDatabase.SortedSetRangeByRankWithScoresAsync` metody. Následující příklad zobrazuje názvy a skóre horní 10 příspěvky seřazené blogu:

```csharp
foreach (var post in await cache.SortedSetRangeByRankWithScoresAsync(
                               redisKey, 0, 9, Order.Descending))
{
    Console.WriteLine(post);
}
```

V dalším příkladu jsou použity `IDatabase.SortedSetRangeByScoreWithScoresAsync` metodu, která můžete omezit položky, které vracejí ty, které spadají do dané skóre oblasti:

```csharp
// Blog posts with scores between 5000 and 100000
foreach (var post in await cache.SortedSetRangeByScoreWithScoresAsync(
                               redisKey, 5000, 100000))
{
    Console.WriteLine(post);
}
```

### <a name="message-by-using-channels"></a>Zprávu pomocí kanálů

Kromě budou sloužit jako mezipaměti dat, Redis server poskytuje zasílání zpráv prostřednictvím mechanismus výkonné publisher/účastníka. Klientské aplikace můžete se přihlásit k odběru kanálu a jiných aplikací nebo služeb můžete publikovat zprávy na kanál. Přihlášení k odběru aplikací poté se zobrazí tyto zprávy a jejich zpracování.

Redis obsahuje příkaz přihlásit k odběru pro klientské aplikace použít k přihlášení k odběru kanály. Tento příkaz předpokládá, že název jeden nebo více kanálů, na kterých aplikace přijímat zprávy. Knihovna StackExchange obsahuje `ISubscription` rozhraní, což umožňuje aplikaci .NET Framework pro přihlášení k odběru a publikovat kanály.

Vytvoření `ISubscription` pomocí objektu `GetSubscriber` způsob připojení k serveru Redis. Pak můžete přijímat zprávy na kanál pomocí `SubscribeAsync` metoda tento objekt. Následující příklad ukazuje, jak přihlášení k odběru kanálu s názvem "zprávy: blogPosts":

```csharp
ConnectionMultiplexer redisHostConnection = ...;
ISubscriber subscriber = redisHostConnection.GetSubscriber();
...
await subscriber.SubscribeAsync("messages:blogPosts", (channel, message) =>
{
    Console.WriteLine("Title is: {0}", message);
});
```

První parametr `Subscribe` způsob je název kanálu. Tento název sleduje stejné konvence používaných klíčů v mezipaměti. Název může obsahovat binárními daty, i když je vhodné použít relativně krátký, smysluplný řetězce chcete, aby dobrý výkon a udržovatelnost.

Nezapomeňte, že oboru názvů používaný službou kanály je nezávislý používaném klíče. To znamená, že budete moci kanály a zkratky, které mají stejný název, i když to může být kód aplikace obtížnější údržbu.

Druhý parametr je delegáta akce. Tento delegát spustí asynchronní pokaždé, když se zobrazí na kanál novou zprávu. V tomto příkladu jednoduše zobrazí zprávu v konzole (zpráva bude obsahovat název příspěvku blogu).

Publikovat na kanál, můžete aplikaci pomocí příkazu Redis publikovat. Knihovna StackExchange poskytuje `IServer.PublishAsync` způsob, jak tuto operaci. Další fragment kódu ukazuje, jak publikovat zprávy "zprávy: blogPosts" kanál:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
ISubscriber subscriber = redisHostConnection.GetSubscriber();
...
BlogPost blogpost = ...;
subscriber.PublishAsync("messages:blogPosts", blogPost.Title);
```

Existuje několik bodů, které je třeba porozumět o mechanismus publikovat nebo přihlášení k odběru:

- Více účastníky můžete se přihlásit k odběru stejný kanál a bude přijímat zprávy, které jsou publikované na tento kanál.
- Předplatitelé pouze zprávy, které jste publikovali po jejich odběru služeb. Nejsou paměti kanály a po publikování zprávu infrastruktury Redis přesune zprávu do každého účastníka a potom ji odebere.
- Ve výchozím nastavení zprávy obdrží předplatitele v pořadí, ve kterém jsou odeslány. Systému vysoce aktivní s velkým počtem zpráv a mnoho předplatitele a vydavatelé zaručené sekvenční doručování zpráv můžete zpomalit systému. Pokud je nezávislý každou zprávu a pořadí je důležité, můžete zapnout souběžné zpracování Redis systému, která pomáhají zlepšit rychlostí reakce. Můžete dosáhnout v klientovi StackExchange nastavením PreserveAsyncOrder připojení používá odběratele NEPRAVDA:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
redisHostConnection.PreserveAsyncOrder = false;
ISubscriber subscriber = redisHostConnection.GetSubscriber();
```

## <a name="related-patterns-and-guidance"></a>Související vzorků a doporučené postupy

Následujícího vzorce může být pro váš scénář důležité také při implementaci ukládání do mezipaměti v aplikacích:

- [Zrušení mezipaměti vzorek](http://msdn.microsoft.com/library/dn589799.aspx): Tento způsob se dozvíte, jak načíst data v případě potřeby do mezipaměti z úložiště dat. Tento způsob taky pomůže zajistit soulad mezi ukládání do mezipaměti dat a dat v úložišti původní data.
- Informace o provádění vodorovné rozdělení ke zlepšení škálovatelnost při ukládání a přístup k velké objemy dat obsahuje [Sharding vzorku](http://msdn.microsoft.com/library/dn589797.aspx) .

## <a name="more-information"></a>Další informace

- Na stránce [MemoryCache třídy](http://msdn.microsoft.com/library/system.runtime.caching.memorycache.aspx) na webu společnosti Microsoft
- [Si přečtěte následující dokumentaci Azure Redis mezipaměti](https://azure.microsoft.com/documentation/services/cache/) stránky na webu společnosti Microsoft
- Na stránce [Azure Redis mezipaměti nejčastější dotazy týkající se](redis-cache/cache-faq.md) na webu společnosti Microsoft
- Na stránce [Konfigurace modelu](http://msdn.microsoft.com/library/windowsazure/hh914149.aspx) na webu společnosti Microsoft
- Na stránce [Asynchronní zpracování úkolů](http://msdn.microsoft.com/library/hh873175.aspx) na webu společnosti Microsoft
- Na stránce [kanály a multiplexers](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/PipelinesMultiplexers.md) na repo StackExchange.Redis GitHub
- Na stránce [Redis trvalé](http://redis.io/topics/persistence) na webu Redis
- [Replikace stránky](http://redis.io/topics/replication) na webu Redis
- Na stránce [Redis kurz obrázku](http://redis.io/topics/cluster-tutorial) na webu Redis
- [Partitioning: jak rozdělit dat mezi několika instancích spuštěných Redis](http://redis.io/topics/partitioning) stránky na webu Redis
- [Použití Redis jako LRU mezipaměti](http://redis.io/topics/lru-cache) stránky na webu Redis
- Na stránce [transakce](http://redis.io/topics/transactions) na webu Redis
- Na stránce [Redis zabezpečení](http://redis.io/topics/security) na webu Redis
- Na stránce [břišní kolem Azure Redis mezipaměti](https://azure.microsoft.com/blog/2014/06/04/lap-around-azure-redis-cache-preview/) v Azure blogu
- [Spuštění Redis na OM Linux CentOS v Azure](http://blogs.msdn.com/b/tconte/archive/2012/06/08/running-redis-on-a-centos-linux-vm-in-windows-azure.aspx) stránky na webu společnosti Microsoft
- [Poskytovatele stavu relace ASP.NET Azure Redis mezipaměti](redis-cache/cache-aspnet-session-state-provider.md) stránky na webu Microsoft
- Stránka [ASP.NET poskytovatele výstupní mezipaměti pro mezipaměti Redis Azure](redis-cache/cache-aspnet-output-cache-provider.md) na webu společnosti Microsoft
- Na stránce [Úvod k datovým typům a odběrů Redis](http://redis.io/topics/data-types-intro) na webu Redis
- [Základní použití](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Basics.md) stránky na webu StackExchange.Redis
- Na stránce [transakce v Redis](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Transactions.md) na StackExchange.Redis repo
- [Rozdělení Průvodce dat](http://msdn.microsoft.com/library/dn589795.aspx) na webu společnosti Microsoft
