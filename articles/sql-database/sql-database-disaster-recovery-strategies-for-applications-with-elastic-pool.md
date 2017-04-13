<properties 
   pageTitle="Navrhování cloudové řešení pro obnovení havárie s použitím Geo replikace databáze SQL | Microsoft Azure"
   description="Zjistěte, jak navrhnout cloudové řešení pro obnovení havárie volbou vpravo převzetí vzorku."
   services="sql-database"
   documentationCenter="" 
   authors="anosov1960" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA" 
   ms.date="07/16/2016"
   ms.author="sashan"/>

# <a name="disaster-recovery-strategies-for-applications-using-sql-database-elastic-pool"></a>Strategie obnovení havárie aplikací pomocí fondu pružná databáze SQL 

V letech, kterou jsme naučili cloudovým službám se není dokonalé a katastrofické incidentem můžete a dojde. Databáze SQL přináší celou řadu možností stanovit nepřerušený aplikace při výskytu tyto incidentem. [Pružná fondů](sql-database-elastic-pool.md) a samostatnou databází podporovat stejný druh možnosti obnovení havárie. Tento článek popisuje několik DR strategie pro ohebné fondy, využít tyto funkce kontinuitu firmy databáze SQL.

Pro účely tohoto článku použijeme kanonický vzorek nezávislí výrobci softwaru SaaS aplikace:

<i>Moderní cloudu pomocí webové aplikace ustanovení jedné databáze SQL pro každou koncového uživatele. ISV obsahuje velké množství zákazníků a proto používá mnoho databází jmenoval klienta databází. Vzhledem k tomu klienta databáze obvykle vzorků neočekávané aktivity, ISV pomocí fondu pružná databázi nákladů velmi předvídatelná průběhu delšího období. Pružná fondu také zjednodušuje správy výkonu při špičky činnosti uživatelů. Kromě klienta databází aplikace taky používá několik databází spravovat profily uživatelů, zabezpečení, shromáždit vzorce použití atd. Dostupnost jednotlivé klienti nemá vliv na dostupnost aplikace jako celek. Však dostupnost a výkonu Správa databází, je naprosto zásadní funkce aplikace a Správa databází práci v offline režimu celé aplikace je offline.</i>  

Ve zbývající části papíru probereme strategie DR překrývajícím oblasti scénáře z náklady citlivé spouštění aplikací z nich požadavkům přísnější dostupnosti.  

## <a name="scenario-1-cost-sensitive-startup"></a>Scénář 1. Náklady citlivé spuštění

<i>Jsem firmy při spuštění a mi moc nákladů citlivé.  Chci zjednodušit nasazení a správu aplikace a jste ochotni mají omezený SLA pro jednotlivé zákazníky. Ale chcete zajistit, aby aplikace jako celek je nikdy offline.</i>

Splňují požadavek na jednoduché, by měl nasazení všechny databáze klienta do jednoho pružná fondu v oblasti Azure podle svého výběru a nasazení Správa databází jako samostatný replikovat geo databází. Obnovení havárie student použijte geo obnovení, který se žádná další platbou. Zajištění dostupnosti Správa databází, musí být geo replikovat na jiné oblasti (krok 1). Průběžné náklady konfigurace obnovení havárie v tomto scénáři je rovno celkové náklady na vedlejší databází. Toto nastavení je znázorněn v dalším diagramu.

![Obrázek 1](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-1.png)

V případě výpadku v oblasti hlavní kroky obnovení můžete získat aplikaci online doplněný další diagramu.

- Převzetí řízení okamžitě databáze (2) na web DR oblast. 
- Změna aplikace připojovací řetězec tak, aby ukazovaly na web DR oblast. Všechny nové účty a klientovi databáze se vytvoří v oblasti DR. Stávajících zákazníků uvidí způsobem dočasně nedostupný.
- Vytvoření fondu pružná s nastavením stejný jako fondu původní (3). 
- Obnovení geo slouží k vytvoření kopie klienta databáze (4). Můžete zvažte aktivaci jednotlivé obnovení připojení koncového uživatele nebo použít některé další schéma aplikace konkrétní priority (priorita).

V tomto okamžiku aplikace návrat do online režimu v oblasti DR, ale někteří uživatelé budou mít zpoždění při přístupu ke svým datům.

![Obrázek 2](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-2.png)

Pokud výpadku dočasné, je možné, že oblasti primární obnoven tak, že Azure, předtím, než obnoví úplný v oblasti DR. V tomto případě by měl organizovat přesunutí aplikace zpět do oblasti primární. Proces bude kroků ukázáno další diagramu.
 
- Zrušení všechny žádosti o zbývající geo obnovit.   
- Převzetí řízení databází k oblasti primární (5). Poznámka: Po obnovení požadovanou oblast staré základní barvy automaticky stali druhotné. Teď se přepne role znovu. 
- Změna aplikace připojovací řetězec tak, aby ukazovaly na primární oblast. Teď všech nových účtů a klientovi databáze se vytvoří primární oblasti. Některé stávajících zákazníků uvidí způsobem dočasně nedostupný.   
- Nastavte všechny databáze ve fondu DR jen pro čtení zajistit že nelze upravovat v oblasti DR (6). 
- Pro každou databázi fondu DR, které se změnily od obnovení přejmenovat nebo odstranit odpovídající databází ve fondu primární (7). 
- Zkopírujte aktualizované databáze z fondu DR do fondu primární (8). 
- Odstranění fondu DR (9)

V tomto okamžiku bude vaše aplikace online v oblasti hlavní s všechny databáze klienta k dispozici ve fondu primární.

![Obrázek 3](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-3.png)

Klíčové **těžit** z této strategie je zhoršeným průběžné náklady redundance dat osy. Zálohování přesměrováni automaticky službou databáze SQL s žádné revize aplikace a bezplatně.  Náklady vzniká pouze v případě, že se obnoví pružná databází. **Nejvhodnější poměr** je dokončeno obnovení všechny databáze klienta bude významné dobu trvat. Závisí na celkový počet obnoví zahájí v oblasti DR a celkovou velikost databáze klienta. I když určit jejich prioritu některé klienti obnoví nad ostatními můžete být soutěží s všechny ostatní obnoví, které jsou, které iniciuje ve stejné oblasti jako služba bude přidělení žádá a omezení minimalizovat celkový dopad u stávajících zákazníků databází. Obnovení databáze klienta navíc nemůže začít, dokud se vytvoří nový pružná fond v oblasti DR.

## <a name="scenario-2-mature-application-with-tiered-service"></a>Scénář 2. Zdokonaleným aplikaci vrstvené služby 

<i>Můžu aplikace zdokonaleným SaaS s vrstvené služeb a jiné rozsahu zkušební zákazníci a platební zákazníky. Vyzkoušení zákazníci mám snížit náklady co nejvíc. Zkušební Zákazníci může trvat výpadek služeb, ale chcete snížit jeho pravděpodobnost. Platební zákazníci jsou všechny prostoje letu rizika. Takže chci zkontrolujte, že platíte zákazníci se vždycky přístup ke svým datům.</i> 

Podporuje tento scénář, je vhodné oddělit zkušební klienti od placené klienti jejich vložením do samostatných pružná fondů. Vyzkoušení zákazníci budou mít nižší eDTU jednoho klienta a nižší SLA s delší dobu obnovení. Platební zákazníci má být uvedené v fond s vyšší eDTU jednoho klienta a vyšší SLA. Zajistit nejnižší časového využití by měl být platící zákazníky klienta databází replikovat geo. Toto nastavení je znázorněn v dalším diagramu. 

![Obrázek 4](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-4.png)

Jako první scénář Správa databází bude úplně, použijte samostatnou geo replikovanou databázi pro něj (1). Zajistíte předvídatelná výkon u nových předplatných zákazníka, aktualizace profilu a další operace správy. Oblast, ve kterém jsou umístěny základní barvy Správa databází bude oblasti primární a oblasti, ve kterém jsou umístěny druhotné Správa databází bude oblasti DR.

Platební zákazníci klienta databáze bude mít aktivní databází ve fondu "placené" zřízení v oblasti primární. Zřizujete sekundární fondu se stejným názvem v oblasti DR. Každého klienta podle velikosti zároveň geo replikovat do fondu sekundární (2). To vám umožní snadné obnovení všechny databáze klientovi pomocí překlopení. 

Pokud výpadek v oblasti primární, obnovení postup přenést aplikace online jsou uvedeny v následující diagram:

![Obrázek 5](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-5.png)

- Ihned převzetí řízení databází na web DR oblast (3).
- Změna aplikace připojovací řetězec tak, aby ukazovaly na web DR oblast. Teď všech nových účtů a klientovi databáze se vytvoří v oblasti DR. Existující zkušební zákazníky uvidí způsobem dočasně nedostupný.
- Převzetí placené klienta databáze do fondu v oblasti DR okamžitě obnovit dostupnosti (4). Protože záložní je změna úrovně snadné metadat zvažte optimalizace kde jednotlivé převzetí služeb při selhání byly aktivovány jako služba připojení koncového uživatele. 
- Pokud velikost eDTU sekundární fondu byl nižší než primární, protože sekundární databází pouze povinné kapacita zpracuje protokolů změn během byly druhotné, měli byste okamžitě zvýšit kapacitu fondu teď tak, aby zahrnoval celou pracovní zátěž všechny klienty (5). 
- Vytvoření nového pružná fondu se stejným názvem a stejnou konfiguraci v oblasti DR pro zkušební verzi zákazníci databáze (6). 
- Po vytvoření fondu zkušební zákazníci umožňuje geo obnovení obnovování databází jednotlivé zkušebního klienta do nového fondu (7). Můžete zvažte aktivaci jednotlivé obnovení připojení koncového uživatele nebo použít některé další schéma aplikace konkrétní priority (priorita).

Aplikace je v tomto okamžiku návrat do online režimu v oblasti DR. Všechny platební zákazníky mít přístup ke svým datům během zkušební zákazníci mít zpoždění při přístupu ke svým datům.

Když obnovit oblasti primární tak, že Azure *Po* obnovení aplikace v oblasti DR můžete dál spuštění aplikace v dané oblasti nebo můžete se rozhodnout, selhání zpátky do oblasti primární. Pokud je primární oblast obnoveného *před* proces překlopení dokončení, byste měli vědět navrácení hned. Navrácení bude kroků znázorněna na dalším obrázku: 
 
![Obrázek 6](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-6.png)

- Zrušení všechny žádosti o zbývající geo obnovit.   
- Převzetí řízení databází (8). Po obnovení požadovanou oblast staré primární automaticky stalo sekundární. Teď bude primární znovu.  
- Převzetí placené klienta databáze (9). Po obnovení požadovanou oblast je podobně staré základní barvy automaticky změní druhotné. Teď se stanou primárních barev znovu. 
- Nastavte obnovená zkušební verzi databáze, které se změnily v oblasti DR na jen pro čtení (10).
- Na každou databázi z fondu zkušební zákazníkům web DR změnila obnovení přejmenovat nebo odstranit databázi odpovídající ve fondu primární zkušební zákazníci (11). 
- Zkopírujte aktualizované databáze z fondu DR do fondu primární (12). 
- Odstranění fondu DR (13) 

> [AZURE.NOTE] Převzetí operace je asynchronní. Minimalizovat časového využití je důležité provedení příkazu převzetí klienta databáze v dávkách o aspoň 20 databází. 

Klíčové **prospěch** této strategie je, že nabízí nejvyšší SLA platební zákazníci. Je taky zajišťuje nové pokusy odblokování hned po vytvoření fondu zkušební DR. **Nejvhodnější poměr** je, že toto nastavení zvýší celkové náklady databází klienta náklady fondu sekundární DR placené zákazníci. Navíc má-li fondu sekundární jinou velikost, platící zákazníky bude zaznamenat nižší výkon po převzetí až do dokončení upgradu fondu v oblasti DR. 

## <a name="scenario-3-geographically-distributed-application-with-tiered-service"></a>Scénář 3. Serverová aplikaci vrstvené služby

<i>Mám zdokonaleným SaaS aplikaci vrstvené služeb. Chci nabízejí velmi agresivní SLA zákazníkům placeným a minimalizace rizika dopad při výpadků nastat i stručný přerušení mohou způsobit nespokojenost zákazníka. Je důležité, že platící zákazníky můžete vždycky přístup ke svým datům. Pokusy jsou zdarma a SLA nenabízí zkušební období.</i> 

K podpoře tento scénář, byste měli mít tři samostatné pružná fondů. Dva stejné velikosti fondů s vysokou eDTUs podle databáze by měl zřízení ve dvou různých oblastí má být placené zákazníci klienta databází. Třetí fondu obsahující zkušební klienti by mělo dolním eDTUs na databázi a zřízení jedním ze dvou oblastí.

Chcete-li zajistit nejnižší obnovení dobu výpadků platící zákazníky klienta by měl být databází geo replikovat 50 % primární databází v každém z obou oblastí. Každou oblast podobně, bude mít 50 % sekundární databází. Tímto způsobem, pokud oblast offline pouze 50 % placené zákazníci databází byste měli mít vliv na a musel překlopení. Další databáze zůstane nedotčený. Toto nastavení je znázorněn v následujícím diagramu:

![Obrázek 4](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-7.png)

Stejně jako v předchozí scénáře správy databází bude úplně takže byste měli označit jako samostatný geo replikace databáze (1). Zajistíte předvídatelná výkon nové předplatné zákazníka, aktualizace profilu a další operace správy. Oblast A měl by primární oblast pro správu databáze a oblasti B bude sloužit k obnovení Správa databází.

Platební zákazníci klienta databáze bude taky geo replikovat, ale s základní barvy a druhotné rozdělení oblast A od oblast B (2). Přepnutí jiné oblasti, můžete tímto způsobem primárních databází klienta ovlivněny výpadku a k dispozici. Druhá polovina databází klienta nebude mít vliv na vůbec. 

Následující obrázek znázorňuje obnovení kroky mají provést, když výpadek v oblasti A.

![Obrázek 5](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-8.png)

- Ihned převzetí řízení databází oblast B (3).
- Změna aplikace připojovací řetězec přejděte na Správa databází v oblasti B. změnit správu databází a přesvědčit se nové účty a klientovi databáze se vytvoří v oblasti B a existující databáze klienta najdete tu taky. Existující zkušební zákazníky uvidí způsobem dočasně nedostupný.
- Převzetí placené klienta databáze do fondu 2 v oblasti B okamžitě obnovit dostupnosti (4). Protože záložní je změna úrovně snadné metadat zvažte optimalizace kde jednotlivé převzetí služeb při selhání byly aktivovány jako služba připojení koncového uživatele. 
- Fond 2 od nyní obsahuje jenom primární databáze, ke kterým celkové pracovní zátěž ve fondu zvětšíte tak, aby měli okamžitě zvýšit velikost eDTU (5). 
- Vytvoření nového pružná fondu se stejným názvem a stejnou konfiguraci v oblasti B pro zkušební verzi zákazníci databáze (6). 
- Po vytvoření fondu umožňuje geo obnovení obnovit databázi jednotlivé zkušebního klienta do fondu (7). Můžete zvažte aktivaci jednotlivé obnovení připojení koncového uživatele nebo použít některé další schéma aplikace konkrétní priority (priorita).


> [AZURE.NOTE] Převzetí operace je asynchronní. Minimalizovat časového využití je důležité provedení příkazu převzetí klienta databáze v dávkách o aspoň 20 databází. 

V tomto okamžiku aplikace je návrat do online režimu v oblasti B. Všechny platební zákazníky mít přístup ke svým datům během zkušební zákazníci mít zpoždění při přístupu ke svým datům.

Až bude obnoven oblast A bude potřeba rozhodnout, jestli chcete použít oblast B pro zkušební verzi zákazníky nebo navrácení pomocí fondu zkušební zákazníky v oblasti A. Jedno kritérium může být % zkušebního klienta databází od obnovení změněn. Bez ohledu na toto rozhodnutí bude muset znovu zůstatek placené klienti mezi dvěma fondy. Následující diagram znázorňuje proces když databází zkušebního klienta zpět na oblast A.  
 
![Obrázek 6](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-9.png)

- Všechny zbývající geo obnovení požadavky k vyzkoušení fondu DR zrušte.   
- Převzetí řízení databáze (8). Po obnovení požadovanou oblast měli staré primární automaticky stal sekundární. Teď bude primární znovu.  
- Vyberte které placené klienta databáze se nepovede zpátky do fondu 1 až zahájení převezme jejich druhotné (9). Po obnovení požadovanou oblast všechny databáze ve fondu 1 automaticky stal druhotné. Teď 50 % z nich základní barvy opět změní. 
- Zmenšení velikosti fond 2 původní eDTU (10).
- Nastavení všech obnoví zkušební databází v oblasti B jen pro čtení (11).
- Na každou databázi ve fondu zkušební DR, které se změnily od obnovení přejmenovat nebo odstranit databázi odpovídající ve fondu zkušební primární (12). 
- Zkopírujte aktualizované databáze z fondu DR do fondu primární (13). 
- Odstranění fondu DR (14) 

Klíčové **výhody** této strategie jsou:

- Podporuje nejvíce agresivní SLA platební zákazníci, protože zajišťuje, že výpadku nelze vliv na to, více než 50 % databází klienta. 
- Ho zajišťuje nové pokusy odblokování hned záznam DR fondu je vytvořen během obnovení. 
- Umožňuje efektivnější využití kapacity fondu zaručená 50 % sekundární databází ve fondu 1 a 2 fondu být menší aktivní pak primárních databází.

Hlavní **střídání** jsou:

- Operace CRUD u databází správy budou mít nižší latence koncovým uživatelům připojení k oblast A než koncoví uživatelé připojení k oblasti B, jak se bude spouštět oproti hlavní databází správy.
- Vyžaduje složitější návrhu databáze pro správu. Každý záznam klienta například by musel být umístění značku, kterou je potřeba změnit během překlopení a překlopení zpět.  
- Platební Zákazníci může výkon nižší než obvykle až do dokončení upgradu fondu v oblasti B. 

## <a name="summary"></a>Souhrn

Tento článek se zaměřuje na strategie obnovení havárie osy databázi používá více klienta aplikace SaaS nezávislí výrobci softwaru. Strategie, jaká jste měli podle potřeb aplikaci, třeba obchodní model, SLA chcete nabízení zákazníkům, rozpočet omezení atd. Každý popsané strategie osnovy výhody a nejvhodnější poměr tak, aby bylo možné provádět informovaní rozhodnutí. Navíc určité aplikace zahrne pravděpodobně jiné Azure součásti. Abyste měli Zkontrolujte své firmy kontinuitu pokyny a organizovat využití vrstvě databáze s nimi. Další informace o správě obnovení databázových aplikací v Azure najdete v nápovědě k [navrhování cloudové řešení pro havárie obnovení](./sql-database-designing-cloud-solutions-for-disaster-recovery.md) .  


## <a name="next-steps"></a>Další kroky

- Další informace o databáze SQL Azure automatické zálohy, najdete v článku [automatické zálohy databáze SQL](sql-database-automated-backups.md)
- Přehled kontinuitu business a scénáře najdete v tématu [Přehled kontinuitu Business](sql-database-business-continuity.md)
- Další informace o použití zálohy pro automatické obnovení najdete v tématu [obnovení databáze ze zálohy spuštěná služba](sql-database-recovery-using-backups.md)
- Další informace o rychlejší možnosti obnovení najdete v tématu [Aktivní Geo replikace](sql-database-geo-replication-overview.md)  
- Další informace o použití zálohy pro automatické archivace najdete v tématu [kopie databáze](sql-database-copy.md)
