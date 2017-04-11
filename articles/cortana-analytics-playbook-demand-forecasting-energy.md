<properties
    pageTitle="Cortana Intelligence řešení šablony Playbook pro službu Prognózování energie | Microsoft Azure"
    description="Šablona řešení s Microsoft Cortana Intelligence, která vám pomůže prognózy služba pro firmu nástroj energie."
    services="cortana-analytics"
    documentationCenter=""
    authors="ilanr9"
    manager="ilanr9"
    editor="yijichen"/>

<tags
    ms.service="cortana-analytics"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/24/2016"
    ms.author="ilanr9;yijichen;garye"/>

# <a name="cortana-intelligence-solution-template-playbook-for-demand-forecasting-of-energy"></a>Cortana Intelligence řešení šablony Playbook pro službu Prognózování energie  

## <a name="executive-summary"></a>Shrnutí  

V několika posledních let mít Internet věcí (IoT), zdrojů energetické alternativní a velký dat sloučeny vytvářet obrovské příležitosti v doméně nástroj i energii. Ve stejnou dobu nástroj a odvětví celý energetický viděli spotřebu sloučení s příjemci náročné lepší možnosti máte při spravování jejich použití energie. Nástroj a inteligentní mřížky společnosti jsou skvělé potřebují inovování a obnovení sami. Mnoho power a nástroje mřížky se navíc stávají zastaralé a udržovat a spravovat návrhu. V posledním roce týmu pracuje na celá řada závazky v rámci domény energie. Během tyto závazky mít došlo k mnoha případech, kdy byla hledání nástroje nebo tvůrce programů (nezávisle na Software dodavatelé) do Prognózování pro budoucí spotřeby energie. Tyto odhady přehrát důležité role ve své firmě současné i budoucí a se základem různých případy použití. Jedná se o krátké a dlouhodobé power zatížení prognózy obchodování, Vyrovnávání zatížení, mřížky optimalizace atd. Velkých dat a pokročilé technologie pro analýzu AA metody například výukové počítače (ML) jsou klíčové enablers pro vytváření přesné a spolehlivé prognózy.  

V tomto playbook sestavili jsme business a analytické pokyny potřebné pro úspěšné vývoj a nasazení spotřeby energie odhadu řešení. Tyto navrhované pokyny slouží nástroje, vědeckých dat a odborníci dat při vytváření plně operationalized cloudové, služba Prognózování řešení. Organizacím, které začínají jenom jejich velkých dat a pokročilé technologie pro analýzu cesty můžete toto řešení představují počátečních v jejich dlouhodobé strategie inteligentní mřížky.

>[AZURE.TIP] Diagram, který poskytuje přehled architektury tuto šablonu stáhnout, najdete v článku [Architektura Cortana Intelligence řešení šablon pro službu Prognózování energie](cortana-analytics-architecture-demand-forecasting-energy.md).  

## <a name="overview"></a>Základní informace  

Tento dokument obsahuje firmy, dat a technické aspekty použití Cortana měřítka a v určité Azure počítače výukové (AML) pro implementaci a nasazení řešení Prognózování energie. Dokument se skládá ze tří hlavních částí:  

1. Principy Business  
2. Principy dat  
3. Technická implementace

Část **Obchodní principy** nastiňuje firmy poměr, které třeba porozumět a zvažte před rozhodování o investice. Vysvětluje, jak kvalifikovat problém firmy po ruce zajistit prediktivní technologie pro analýzu a počítač výukové skutečně efektivní a použitelné. Dokument další vysvětluje základy učení počítače a jak se používají k řešení problémů energie Prognózování. Popisuje požadavky a kritéria kvalifikace případu použití. Některé ukázkové pomocí soudní případy a obchodní případ, které jsou poskytovány i scénáře.

Data jsou hlavní složkou pro všechny počítače výukové řešení. **Principy datového** část tohoto dokumentu obsahuje několik důležitých aspektů data. Popisuje druh dat, který je nutný pro prognózování energetický požadavkům kvality dat a zdrojů dat, obvykle existují. Také popisují použití nezpracovanými daty Příprava dat funkce, které skutečně jednotka části modelování.

Třetí část dokumentu zahrnuje **Technické implementaci** aspekty řešení. Inženýrské funkce modelování jsou základní proces pro výzkum dat a proto jsou popsané v některých podrobností. Zahrnuje koncepci webové služby, které jsou důležité vozidla cloudu nasazení řešení prediktivní analýzy. Jsme také vytvořit přehled typické architektury řešení operationalized začátku do konce.

Kromě toho dokument obsahuje referenční materiály, které můžete použít k získání dalších Principy domén a technologií.

Je důležité mít na paměti, že jsme nechtěli pokrýval v tomto dokumentu hlubší dat pro výzkum procesu, jeho matematických a technické aspekty. Tyto informace najdete v [dokumentaci Azure ML](http://azure.microsoft.com/services/machine-learning/) a [blogů](http://blogs.microsoft.com/blog/tag/azure-machine-learning/).

### <a name="target-audience"></a>Cílové skupiny   
Cílové skupiny pro tento dokument je business a technické zaměstnanců, kteří chtějí k získání knowledge a principy učení počítače na základě řešení a jak jsou použity konkrétně v rámci domény energie Prognózování.

Vědeckých dat můžete taky využít čtení tohoto dokumentu nejlépe pochopíte nejvyšší úrovně proces, který jednotky nasazení energie Prognózování řešení. V tomto kontextu jej lze také vytvořit dobrý podle směrného plánu a výchozí bod pro další podrobné a Upřesnit materiál.

### <a name="industry-trends"></a>Trendy odvětví  
V několika posledních let mít IoT, zdrojů energetické alternativní a velký dat sloučeny vytvářet obrovské příležitosti v prostoru nástroj i energii. Ve stejnou dobu nástroj a odvětví celý energetický viděli spotřebu sloučení s příjemci náročné lepší možnosti máte při spravování jejich použití energie.

Mnoho nástroje a Inteligentní energie společnosti byla nasazením počet případů, které usnadňují použití dat generovaných mřížky pomocí mít pioneering [inteligentní mřížky](https://en.wikipedia.org/wiki/Smart_grid) Mnoho případy použití základem základní vlastnosti elektrické výrobní: nemůže být sečtenou za ani uložených stranou jako zásob. Ano co je vyrobeno musí být použita. Nástroje, které má být efektivnější muset prognózy spotřebu energie jednoduše vzhledem k tomu, který bude umožnit je větší **a potřeb zůstatek**, takže energie ztráty **snížit skleníkových kniha emise**a řízení nákladů.

Při hovorech nákladů, je dalším důležitým aspektem, který je cena. Nové možnosti obchodu power mezi nástroje mít přenášet velmi potřebné **prognózy budoucí služba a budoucí cena elektrické**. To může pomoci společnosti určit jejich objemů výroby.

Použijete-li jsme na slovo "inteligentní", jsme skutečně v nápovědě jejich přichycením k mřížce, můžete informace a potom vyberte předpovědí. Můžete předcházet sezónní změny spotřebu stejně jako v **stanovit dočasné přetížení situacích a automaticky nastavit pro ni**. Úpravou vzdáleně spotřebu (pomocí těchto inteligentní metry), můžete přistupovat lokalizované přetížení situací. **Nejdřív předpovídání a pak budou sloužit**mřížky umožňuje efektivněji určitou dobu.

Ve zbytku tohoto dokumentu jsme se zaměřuje na konkrétní řady případy použití, které průvodní předpovídání budoucnost krátkodobá a dlouhodobé službu energie. Jste pracovali v těchto oblastech několik měsíců jsme získali některé znalosti a dovednosti, pomocí kterých cz k výsledkům testu odvětví. Další případy použití bude vztahovat i v dokumentu nevidět.

## <a name="business-understanding"></a>Principy Business

### <a name="business-goals"></a>Obchodní cíle
**Ukázka energie** cílem je ukazují typické prediktivní technologie pro analýzu a automatické učení řešení, které může být nasazené v rámečku velmi čas (krátký). Konkrétně je naše aktuální fokus o povolení energie služba prognózy řešení tak, že jeho hodnota firmy můžete rychle realizují a využít při. Informace v tomto playbook pomůže zákazníka provádění následující cíle:
-   Čas (krátký) hodnotu učení počítače na základě řešení
-   Možnost rozbalte pilotního nasazení případu ostatních případech použít nebo širší obor podle jejich obchodních potřeb použití
-   Rychle získat knowledge product Cortana Intelligence sady

S těmito cíli na paměti Toto playbook cílem je předvádění business a znalosti, které vám pomohou při dosažení těchto cílů.

### <a name="power-load-and-demand-forecasting"></a>Výkon načítání a služba Prognózování
V rámci odvětví energie může být mnoha způsoby, které služba Prognózování pomáhají řešit důležité obchodní problémy. Ve skutečnosti poptávka Prognózování považovat za základem mnoha případech použít základní v odvětví. Obecně doporučujeme vzít v úvahu dva typy energie služba prognózy: krátkodobá a dlouhodobou. Každého z nich mohou různé účel a využívání jiným způsobem. Hlavní rozdíl mezi těmito dvěma je prognózy horizont, což znamená časový rozsah do budoucna, pro kterou jsme by prognózy.

#### <a name="short-term-load-forecasting"></a>Krátké termínů zatížení Prognózování
V rámci spotřeby energie krátké termínů načíst Prognózování (STLF) rozumí agregované zatížení, které je naplánované v blízké budoucnosti na různé části mřížce (nebo jako celek). V tomto kontextu krátkodobá je definován časový horizont v rozsahu 1 hodinu 24 hodin. V některých případech horizont 48 hodin také je možné. STLF tedy velmi běžné v případě provozní použití mřížky. Tady je pár příkladů STLF řízený úsilím případy použití:
-   Nabídka a poptávka vyrovnávání
-   Obchodní podpora Power
-   Provádění Market (nastavení power cena)
-   Optimalizace provozní mřížky
-   [Služba odpověď](https://en.wikipedia.org/wiki/Demand_response)
-   Vrcholu službu Prognózování
-   Správa poptávek straně
-   Vyrovnávání zatížení a přetížení prevenci
-   Dlouhodobě zatížení Prognózování
-   Zjišťování poruch a odchylky
-   Pole Špička curtailment/vyrovnání 

STLF model převážně vycházejí blízké minulosti (poslední den nebo týden) Spotřeba dat a použití naplánované teploty jako důležité prognostických. Získání přesné teploty prognózy pro další hodinu a až 24 hodin, se menší výzvu nyní dnů. Tyto modely jsou menší citlivé na sezónní tendence nebo trendy dlouhodobé spotřebu.

Řešení SLTF mohou také generovat velkého množství předpovědí hovory (žádost o službu) od jejich jsou právě vyvolání na hodinu a v některých případech i vyšší četnosti. Je také velmi běžné zobrazíte implantaci kde každý jednotlivé transformovny nebo transformer představuje jako samostatný model, a proto je podrobnějších hlasitost předpovědí žádostí o.

#### <a name="long-term-load-forecasting"></a>Dlouhodobě zatížení Prognózování
Cílem části dlouhé termínů zatížení Prognózování (LTLF) je prognózy služba power s horizont časové rozmezí od 1 týden na několik měsíců (nebo v některých případech pro určitý počet let). Tato oblast horizont platí převážně pro plánování a případy použití investice.

Dlouhodobé scénáře je důležité, abyste měli kvalitní data, která zahrnuje období několika let (minimální 3 let). Tyto modely obvykle extrahovat vzorek sezónnost z historických dat a můžete využít externí predicators tak jako počasí a prostředí.

Je důležité chcete zvýraznit, že delší prognózy horizont, čím menší přesné prognózy může být. Proto je důležité pro vytvoření některé intervalu spolehlivosti spolu s skutečné prognózy, která by umožňovala člověka faktor možné variant do jejich plánování si vypište.

Protože převážně plánování spotřebu scénáře pro LTLF očekáváme mnohem nižší svazky prediktivní vkládání (v porovnání s STLF). Vidíme má obvykle tyto předpovědí vložený do nástroje vizualizace například Excelu nebo PowerBI a uplatnit ručně uživatelem.

### <a name="short-term-vs-long-term-prediction"></a>Krátké termínů porovnání Dlouhá citace termínů předpovědí
Následující tabulka porovnává STLF a LTLF z hlediska nejdůležitější atributy:

|Atribut|Krátkodobá zatížení prognózy|Dlouhodobě zatížení Forecast|
|---|---|---|
|Prognóza horizont|Na 1 hodinu 48 hodin|Od 1 do 6 měsíců nebo více|
|Rozlišení dat|Každou hodinu|Každou hodinu nebo denní|
|Typické použití případů|<ul><li>Služba/dodávky vyrovnávání</li><li>Vyberte hodinu Prognózování</li><li>Služba odpověď</li></ul>|<ul><li>Dlouhodobě plánování</li><li>Plánování prostředky mřížky</li><li>Plánování zdrojů</li></ul>|
|Typické prognostické|<ul><li>Den nebo týden</li><li>Hodinu dne</li><li>Každou hodinu teploty</li></ul>|<ul><li>Měsíc v roce</li><li>Den v měsíci</li><li>Teplotní dlouhodobou a prostředí</li></ul>|
|Oblast historických dat|Dvě nebo tři roky hodnotě dat|5 až 10 let hodnotě dat|
|Typické přesnosti|MAPE * 5 % nebo nižší|MAPE * 25 % nebo nižší|
|Prognóza četnosti|Každou hodinu nebo každých 24 hodin|Vytvořené po měsíční, čtvrtletní a roční|
\*[MAPE](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error) – střed_hodn průměr procentuální chyby

Jak je vidět z této tabulky, je velmi důležité k rozlišení mezi krátké a dlouhodobou Prognózování scénáře jako tyto představují různé obchodních potřeb a může mít různých nasazení a spotřební vzorce.

### <a name="example-use-case-1-esmart-systems--overload-optimization"></a>Případ použití Příklad 1: eSmart systémy – optimalizace přetížení
Důležitou roli [inteligentní mřížky](https://en.wikipedia.org/wiki/Smart_grid) se dynamicky neustále optimalizace a úprava pro změnu vzorků spotřebu. Může dojít k ovlivnění spotřebu energie změnou krátkou platností, které jsou hlavně způsobená kolísání teplota (*například*více power se používá pro air podmínka nebo vytápění). Ve stejnou dobu spotřebu energie je ovlivněny také dlouhodobé trendy. Patří k nim sezónnost efekty, státní svátky, dlouhodobé geometrického spotřeba a dokonce economic faktory například index spotřebitelských, oil cena a HDP.

V tomto případě použití [eSmart](http://www.esmartsystems.com/) chtěli nasazení cloudové řešení, které umožňuje předpovídání přestavující situace přetížení na dané transformovny mřížky. Zejména eSmart chtěli identifikovat trakčních, které by mohly přetížení do další hodiny, takže provést okamžité může být akci aby nedocházelo k řešení Tato situace.

Přesné a rychle provádění předpovědí vyžaduje provádění tři prediktivní modely:
-   Dlouhé modelu termín, který umožňuje Prognózování spotřeby energie v jednotlivých transformovny během další několik týdnů či měsíců
-   Krátkodobých modelu, který umožňuje předpověď přetížení situaci v dané transformovny během další hodinu
-   Model teplotu, který poskytuje Prognózování budoucí teploty přes více scénářů

Cílem dlouhodobé modelu je pořadí trakčních podle jejich tendenci přetížení (vzhledem k tomu, jejich kapacity přenosu power) během další týden nebo měsíc. Díky tomu vytvoření chcete použít krátký seznam trakčních, které bude sloužit jako vstup pro krátkodobých předpovědí. Teplotní je důležité prognostických dlouhodobé modelu, je třeba neustále plodiny více scénář teploty prognózy a kanálu předávat na vstupu do dlouhodobé modelu. Krátkodobá forecast je pak uplatněna odhadnout, které transformovny je pravděpodobně přetížení přes další hodinu.

Krátkodobá a dlouhodobé modely jsou za každou transformovny nasazeny jednotlivě. Proto praktické provedení těchto modelů vyžaduje rozsáhlé průběhu. Získat vyšší přesnosti předpovědí krátkodobě se snaží o odstupňovaný modelu pro každou hodinu dne. Všechny tyto modely zpracují každou hodinu a dokončete spuštění objevit během pár minut umožňuje dostatek času odpovědět a akcím preventivní v případě potřeby. Tento soubor modely bude k dispozici aktuální pomocí pravidelné rekvalifikaci nejnovější data.

Další informace o tomto případu použití najdete [tady](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18945).

#### <a name="use-case-qualification-criteria--prerequisites"></a>Mohli byste použít kritérium případu kvalifikace – požadavky
Hlavní: Zkontrolujte sílu Cortana Intelligence je v výkonné schopnost nasazení a měřítko na střed řešení výukové počítače. Slouží k podpory tisíců předpovědí souběžně provedených. Můžete automaticky přizpůsobit zahájit změnu struktury spotřeby. Zaměření řešení, tedy ve přesnosti a výpočetní výkon. Nástroj společnost je třeba zájem o vytváření přesné energie služba prognózy pro další hodinu a pro každou hodinu dne. Na druhé straně jsme zajímají méně odpověď otázku, proč je poptávka předpovídanými být ZobrazitFormulář.aspx je (samotného modelu zpracuje,).

Proto je třeba si uvědomit, že všechny případy použití a obchodních problémů můžete efektivně vyřešit pomocí počítače výukové.

Cortana měřítka a výuka počítači může být vysoce efektivní při řešení problému daný obchodní, pokud jsou splněny následující podmínky:
-   Obchodní ruku je **prediktivní** charakter. Příklad případu použití prediktivní je nástroj společnost, která chcete odhadnout power zatížení dané transformovny během další hodinu. Na druhé straně analýza a řazení ovladače historických služba bude **Popisný** v přírodě a tedy méně použitelné.
-   Jakmile neexistuje odhadu považuje se vymazat **cestu akce** . Například předpovídání přetížení na transformovny během další hodinu mohou být příčinou aktivní akci snížení zatížení, která je přidružená k této transformovny a tedy potenciálně ochrana před vytvářením přetížení.
-   Případ použití představuje **Typické typ problému** takové který po vyřešit ho můžete připravit tak, jak k řešení jiných podobné případy použití.
-   Zákazník můžete nastavit **množstevní a kvalitativní cíle** prokázat implementaci úspěšné řešení. Například pro forecast službu energetický dobré množstevní cíl bude mezní vyžaduje přesnost (*například*až 5 % Chyba je povolena) nebo kdy předpovídání transformovny přetížení potom přesnost (sazba true pozitivní) a odvolání (rozsah true pozitivní) by měl být dané prahovou hodnotu. Tyto cíle by měl odvozeno z zákazníka obchodní cíle.
-   Je jasné **Integrace** s pracovním postupem firmy společnosti. Například prognózy zatížení transformovny lze integrovat do mřížky řídicí centrum uživatele – umožňuje přetížení zabránění činnostem.
-   Zákazník musí připraven k použití **dat pomocí dostatečně kvalitní** podporuje případu použití (viz více v další části, **Kvality dat**, tento playbook).
-   Zákazník zahrnuje architektura cloudu dat na střed nebo **výukové cloudové počítače**, včetně Azure ML a jiných komponent Cortana měřítka sady.
-   Zákazník je nevadí stanovit **toku dat konce** tomto zařízení doručení dat do cloudu průběžně a je ochoten k **umožňují** řešení.
-   Zákazník je připravená k **určit zdroje** kdo bude aktivně uvažuje během počáteční pilotní implementace tak, aby znalosti a vlastnictví řešení může dojít ke ztrátě určitého zákazníkovi po úspěšném.
-   Zdroj zákazníka by měl být **kvalifikovaných dat profesionální**, nejlépe dat vědecký pracovník.

Kvalifikace případu použití podle výše uvedených kritérií můžete výrazně zvýšit úspěšnosti případu použití a vytvořit dobrý beachhead pro provádění případy budoucí použití.

### <a name="cloud-based-solutions"></a>Cloudové řešení
Cortana Intelligence Suite na Azure je integrované prostředí umístěné v cloudu. Nasazení řešení pro pokročilé technologie pro analýzu v prostředí cloudu obsahuje, který nabízí podstatně vyšší výhod pro firmy a současně může nechtěli velkou změnu organizacím, které dál používat místních IT řešení. V rámci odvětví energie je vymazat trend Postupná migrace operací do cloudu. Tento vývoj souvisí spolu s vývoj inteligentní mřížky jak bylo popsáno výše v **Odvětví trendy**. Podle tohoto playbook se zaměřuje na cloudové řešení v doméně energie, je důležité získáte informace o výhodách a dalších aspektech nasazení cloudové řešení.

Možná největších výhod cloudové řešení je cena. Jako řešení využívá součástí nasazený cloudu, nemůže žádným způsobem předem náklady ani náklady na prodané zboží (náklady z zboží prodal) součást náklady spojené s ním. To znamená, že není potřeba investovat do hardware, software a IT údržby, a proto že podstatné snížení rizika business.

Další výhodou důležité je strukturu systému průběžného financování nákladů cloudové řešení. Cloudové servery pro výpočet nebo úložiště můžete nasazeném a měřítko na základě právě dle potřeby. Představuje náklady efektivity výhod cloudové řešení.

Nakonec není nutné pro investici IT údržbu nebo budoucí infrastrukturu vývoj podle všechny části nabídky cloudové. Rozsahu Cortana Intelligence sadu obsahuje ty nejcennější v předmětu služby a sleduje dlouhodobým řízením projektů. Nové funkce, součásti a možnosti jsou neustále zavádí a vyvíjet.

Společnost, která je právě od jeho přechodu do cloudu jsme jsou se vysoce doporučující umožní postupného implementací cloudu migrace řízením projektů. Jsme myslíte, že pro nástroje a společnosti v doméně energie představují případy použití popisované v tomto playbook příležitost pracovníků době prediktivní analýzy řešení v cloudu.

#### <a name="business-case-justification-considerations"></a>Co byste měli zvážit obchodní změnu zarovnání
V mnoha případech může být zákazníka zájem obchodní odůvodnění pro dané použití případ, ve kterém jsou cloudové řešení a počítač výukové důležité součásti. Na rozdíl od místních řešení, v případě cloudové řešení je součást předem náklady minimální a většina elementů náklady jsou přidružené k skutečné použití. Pokud jde o nasazení energie Prognózování řešení na Cortana Intelligence sadu, více služeb lze integrovat s jednoho běžné strukturu nákladů. Například databáze (*například*SQL Azure) můžete využít k ukládání nezpracovanými daty a potom pro skutečné prognózy Azure ML slouží k hostiteli prognózy služeb. V tomto příkladu strukturu nákladů zahrnovat úložiště a transakční komponenty.

Na druhé straně jednu by měl mít dobře porozumět firmy přínosu provozní požadavek energie Prognózování (kratší nebo delší dobu). Kromě toho je třeba si uvědomit hodnotu firmy každé prognózy operace. Například přesně Prognózování power zatížení pro další 24 hodin můžete zabránit přebytky nebo pomáhá zabránit v mřížce přetížení a můžete kvantifikovány z hlediska finanční úspor denně.

Základní vzorce pro výpočet finanční výhodou služba prognózy řešením: ![základní vzorce pro výpočet finanční výhodou služba prognózy řešení](media/cortana-analytics-playbook-demand-forecasting-energy/financial-benefit-formula.png)

Protože Cortana Intelligence Suite poskytuje systému průběžného financování ceny modelu, je bez nutnosti by komponentu pevné náklady na tento vzorec. Tento vzorec můžete vypočítat na denně, měsíční nebo roční základna.

Aktuální sadu Intelligence Cortana a Azure ML ceny plánů najdete [tady](http://azure.microsoft.com/pricing/details/machine-learning/).

### <a name="solution-development-process"></a>Proces vývoje řešení
Cyklu vývoje požadavek energetický předpovídání řešení obvykle zahrnuje 4 fáze, ve které provedeme pomocí technologie cloudové a služeb v rámci sady měřítka Cortana.

To je znázorněno na následujícím obrázku:

![Inteligentní mřížka obrázku](media/cortana-analytics-playbook-demand-forecasting-energy/smart-grid-cycle.png)

Odstavec popisuje tento 4 krocích:

1.  **Shromažďování dat** – všechny pokročilé technologie pro analýzu na základě řešení závisí na data (najdete v článku **Principy dat**). Konkrétně až přijde na prediktivní technologie pro analýzu a Prognózování, jsme spolehnout probíhající a dynamické toku dat. V případě energie Prognózování služba, tato data můžete získaná přímo z inteligentní metry nebo už sečtou místní databázi. Jsme také spolehnout se na jiných externích zdrojů dat, například počasí a teploty. Tento probíhající toku dat musí orchestrated, naplánované a uložené. [Azure Data Factory](http://azure.microsoft.com/services/data-factory/) (ADF) je naše hlavní Centrem pro provedení tohoto úkolu.
2.  **Modelování** – přesné a spolehlivé energie prognóz, jeden musí vyvíjet (vlaku) a udržovat skvělé model, že něco v ní pomocí historických dat a extrahuje přehlednější a prediktivní vzorky dat. Oblasti výukové počítače (ML) obsahuje rychle Kvetoucí s pokročilejší algoritmů pravidelně vyvinutý. Azure ML Studio poskytuje vysoký výkon uživatele, který umožňuje využívat Většina rozšířených algoritmů ML v rámci dokončení pracovní postup. Tento pracovní postup je znázorněn v intuitivní vývojový diagram a obsahuje Příprava dat, funkce extrahování, modelování a model hodnocení. Uživatele můžete si ho naimportovat do stovky různých modelů, které jsou součástí tohoto prostředí. Na konci této fázi vědecký pracovník dat bude pracovní modelu, který je plně vyhodnocené a jste připraveni na nasazení.

    Na následujícím obrázku je znázorněna typické pracovního postupu:

    ![Modelování pracovního postupu](media/cortana-analytics-playbook-demand-forecasting-energy/modeling-workflow.png)

3.  **Nasazení** – pracovní model ruku, dalším krokem je se nasazení. Tady modelu převedena na webové služby, který poskytuje RESTful rozhraní API, kterou lze souběžně zavolat z různých klientů spotřebu na Internetu. Azure ML poskytuje jednoduché nasazení modelu přímo z Azure Studio ML jedním kliknutím na tlačítko. Rozšířená se stane celý proces nasazení. Toto řešení můžete automaticky přizpůsobit zahájit požadované spotřebu.

4.  **Spotřeba** – v této fázi skutečně provedeme umožňuje prognózy modelu plodiny předpovědí. Můžete úsilím z aplikace uživatelů (*například*, řídicího panelu) nebo přímo z operační systém jako služba/dodávky vyrovnávání spotřebu systému nebo řešení optimalizace mřížky. Více případy použití můžete úsilím z jediný model.

## <a name="data-understanding"></a>Principy dat
Po překrývajícím aspekty firmy požadavek energie Prognózování řešení (najdete v článku **Principy firmy**), doporučujeme jste připraveni diskutovat o část dat. Jakékoli prediktivní analýzy řešení závisí na spolehlivé data. Pro prognózování poptávka energie, jsme závisí na historické spotřebu dat pomocí různých úrovní podrobností. Tato historických data se použije jako suroviny. Bude projít opatrní analýzy, ve kterém bude vědecký pracovník dat identifikovat prognostické (označuje také jako funkcí), které můžete umístit do modelu, který se vytvoří požadované prognózy.

Ve zbývající části tohoto oddílu jsme popisuje různé kroky a důležité informace týkající se Principy data a jak můžete získat použitelné formuláře.

### <a name="the-model-development-cycle"></a>Cyklu vývoje modelu
Vytváření dobrý Prognózování modely vyžaduje některé opatrní přípravu a plánování. Rozdělení proces modelování do několika kroků a zaměřené na postupně po jednotlivých krocích současně může výrazně zvýšit výsledku celého procesu.

Následující obrázek znázorňuje, jak může proces modelování rozdělit na víc takto:

![Cyklu vývoje modelu](media/cortana-analytics-playbook-demand-forecasting-energy/model-development-cycle.png)

Jak je vidět, že cyklu se skládá z šest kroků:
-   Problém formulace
-   Požití dat a průzkum
-   Příprava dat a inženýrské funkce
-   Modelování
-   Model hodnocení
-   Vývojové

Ve zbytku tohoto oddílu jsme popisuje jednotlivé kroky a položek k zamyšlení při každém kroku.

### <a name="problem-formulation"></a>Problém formulace
Jsme můžete zvažte formulace problém jako nejdůležitější krok, který jednu je potřeba provést před implementace žádné řešení prediktivní analýzy. Tady jsme by transformace problém business a rozložit do konkrétní prvky, které můžete vyřešit tak, že použití dat a modelování postupy. Je vhodné formulace problém jako sady otázky, které nám chcete odpovědět. Tady jsou některé možné otázky, které může být v rámci Prognózování služba energii:
-   Co je očekávaná zatížení jednotlivé transformovny další hodinu nebo den?
-   Na jaké čas dojde mřížce Špička služba?
-   Jak pravděpodobně je mřížce udržet zatížení očekávané ve špičce?
-   Kolik power má elektrárny generovat během každou hodinu dne?

Zpracovávající tyto otázky umožňuje zaměřit na získání správná data a implementaci řešení, které je plně zarovnané s problémem firmy po ruce. Kromě toho jsme pak můžete nastavit některé klíčové metriky, které umožňují vyhodnocení výkonu modelu. Například jak přesný prognózy třeba a co je oblast s chybami, které by pořád přijatelná tak, že firmy?

### <a name="data-sources"></a>Zdroje dat
Moderní inteligentní mřížky shromáždí data z různých částí a komponent mřížky. Tato data představují různé aspekty operaci a využití power mřížky. V rozsahu poptávka energie prognózy jsme jsou omezení diskuse na zdroje dat, které odrážet spotřebu skutečná poptávka. Důležitým zdrojem spotřeby energie jsou inteligentní metry. Nástroje po celém světě rychle nasazujete inteligentní metry pro své uživatele. Inteligentní metry zaznamenat skutečný power spotřebu a neustále relay tato data zpět do nástroje společnosti. Data se shromažďují a odeslané zpět pravidelných intervalech, od každého zabere 5 minut na 1 hodinu. Další rozšířené inteligentní metry můžete vzdáleně naprogramován k ovládání a zůstatek skutečnou spotřebu v domácnosti. Inteligentní metr dat je poměrně spolehlivý a obsahuje s časovým razítkem. Díky kterému je důležité složky pro službu prognózy. Metr dat můžete sečtou (sečtené) na různých úrovních topologii Mřížka: transformer transformovny, oblasti, *atd*. Jsme můžete vyberte úroveň požadované agregace k vytvoření prognózy modelu pro něj. Například pokud společnosti nástroj chtěli prognózy budoucí zatížení na všech jeho trakčních mřížky pak všechny metry dat můžete být agregované pro každou jednotlivé transformovny a používá jako vstup pro model prognózy. Inteligentní metry označovány jako zdroji interních dat..

Služba prognózy spolehlivé energie bude taky závisí na dalších externím zdrojům dat. Počasí nebo přesněji teploty je důležitým faktorem ovlivňující spotřebu energie. Historických dat zobrazí silných korelace venkovní teploty a spotřeby energie. Během aktivní letních dnů, které se zobrazují koncovým zkontrolujte pomocí jejich klimatizace a během ze zimní zapněte vytápění. Důvěryhodného zdroje historických teploty mřížky místě je proto klíče. Kromě toho jsme také spolehnout přesné prognózy teploty jako prognostických spotřeby energie.

Jiné externí zdroje dat můžou pomoct taky při vytváření energie služba prognózy modelů na verzi. Pravděpodobně jedná se o dlouhodobou prostředí změny vyplatí indexy (*například*HDP) a ostatní. V tomto dokumentu jsme nezahrnuje tyto jiných zdrojů.

### <a name="data-structure"></a>Datová struktura
Po identifikaci zdroje požadované údaje, byste rádi zajistit, že nezpracovaná data, která byla vybrána zahrnuje funkce správná data. Vytvoření modelu prognózy spolehlivé poptávka, by potřebujeme zajistit, že dat shromážděných zahrnuje datové prvky, které vám mohou pomoci odhadu budoucích poptávka. Tady jsou některé základní požadavky datová struktura (schéma) nezpracovanými daty.

Původní data se skládá z řádků a sloupců. Míry představuje jeden řádek dat. Každý řádek dat obsahuje více sloupců (označuje také jako funkce nebo pole).

1.  **Časové razítko** – pole časového razítka představuje skutečný čas, kdy byla zaznamenané měření. Musí odpovídat s některým z běžné formáty data a času. Datum a čas části by měl být však započítávány. Ve většině případů je nutné přihlášení zaznamená do druhé úrovně podrobností. Je třeba zadat časové pásmo, ve kterém se nahrává data.
2.  **ID měřiče** – toto pole označuje měřiči nebo měrných jednotek zařízení. Je proměnnou kategorií a může být kombinací číslic a znaků.
3.  **Hodnota spotřeby** – to je skutečná spotřeba v dané datum a čas. Spotřebu lze změřit kWh (kilowatt-hour) nebo jiný upřednostňované jednotky. Je důležité mít na paměti, že měrnou jednotku musí zůstat konzistentní přes všechny hodnoty v data. V některých případech spotřebu můžete zadat víc než 3 fáze power. V takovém případě by potřebujeme shromáždit nezávislé spotřebu fáze.
4.  **Teplotní** – teploty se obvykle shromažďují z nezávisle na zdroje. Však by měl být kompatibilní s daty spotřebu. By měl obsahovat časové razítko jak jsme je popsali výše, které vám umožní ho k synchronizaci s daty vlastní spotřebu. Teploty může být zadán ve stupních Celsia nebo Fahrenheita ale může zůstat konzistentní přes všechny hodnoty.
5.  **Umístění:** Pole umístění obvykle souvisí s na místo, kde byly shromážděny teplotní data. Může být reprezentován jako poštovní směrovací čísla nebo ve formátu zeměpisná šířka a délka (lat/dlouhé).

V následujících tabulkách jsou uvedeny příklady dobré spotřeby a teplotu formátu dat:

|**Datum**|**Čas**|**ID měřiče**|**Fáze 1**|**Fáze 2**|**Fáze 3**|
|--------|--------|------------|-----------|-----------|-----------|
|7/1/2015|10:00:00|ABC1234     |7.0        |2.1        |5.3        |
|7/1/2015|10:00:01|ABC1234     |7.1        |2.2        |4.3        |
|7/1/2015|10:00:02|ABC1234     |6.0        |2.1        |4.0        |

|**Datum**|**Čas**|**Umístění**|**Teplotní**|
|--------|--------|-------------|---------------|
|7/1/2015|10:00:00|11242        |24,4           |
|7/1/2015|10:00:01|11242        |24,4           |
|7/1/2015|10:00:02|11242        |24.5           |

Jak je vidět nad, v tomto příkladu zahrnuje 3 odlišné hodnoty pro spotřebu přidružený k 3 power fáze. Navíc nezapomeňte, že jsou oddělené pole Datum a čas, ale budou lze také sloučit do jednoho sloupce. V tomto případě sloupci umístění představuje ve formátu 5místný PSČ a teploty ve formátu stupeň Celsia.

### <a name="data-format"></a>Formát dat ve sloupcích
Cortana měřítka sady podporují nejběžnější formáty data, jako jsou CSV TSV, JSON, *atd*. Je důležité, že formát dat zůstane konzistentní pro celou životního cyklu projektu.

### <a name="data-ingestion"></a>Požití dat
Od energie poptávka forecast je neustále a často předpovídat, jsme musíte se ujistit, že nezpracovanými daty toku procesem požití plné barvě a spolehlivé data. Proces požití musí být zaručena nezpracovanými daty je k dispozici pro proces prognózy v požadovaném čase. Která znamená, že požití četnost dat by měla být větší než prognózy četnosti.

Příklad: Pokud naše služba Prognózování řešení vygeneruje nové prognózy ve 8:00 denně pak potřebujeme zajistit všechna data, která byla vybrána za posledních 24 hodin byla plně požití do té a má i zahrnout poslední hodiny data.

K tomu Cortana Intelligence sadu způsoby různých podpory procesu požití spolehlivé data. Tato další diskuze v části **instalace** tohoto dokumentu.

### <a name="data-quality"></a>Zlepšení kvality dat
Neformátovaná data zdroje, který je potřebný k provádění spolehlivý a přesné služba Prognózování musí splňovat kritéria kvality některé základní data. Rozšířené statistických metod lze vyrovnávat některé nízké kvality dat, ale pořád potřebujeme zajistit, že jsme při ingesting nová data jsou přes mezní kvality některé základní data. Tady je několik tipů jakosti neformátovaná data:
-   **Chybějící hodnoty** – toto nastavení se týká situaci při nebyl shromážděny konkrétních hodnotách. Základní požadavek je, že chybí sazba hodnotu nesmí být větší než 10 % pro všechny daného období. V případě, že jedinou hodnotu chybí ho by měla být zadána pomocí předdefinovaných hodnot (například: "9999") a nikoli "0", které by mohly být platná hodnota.
-   **Měření přesnost** – skutečná hodnota spotřebu nebo teplotu přesně zaznamenávají. Nesprávná rozměry vytvoří nesprávné prognózy. Chyba měření obvykle by měl být menší než 1 % relativní hodnotu true.
-   **Měření** – je nutné, že aktuální časové razítko dat shromážděných nesmí lišit podle víc než 10 sekund relativní true čas skutečného měření.
-   **Synchronizace** – při použily různých zdrojů dat (*například*spotřebu a teploty) jsme musí zajistit, aby tam jsou žádné synchronizace času problémy mezi nimi. To znamená, že časový rozdíl mezi shromážděny časové razítko ze všech dva nezávisle na zdroje dat nepřekročí více než 10 sekund.
-   **Latence** – jak je popsáno výše v **Požití dat**, můžeme závisejí na spolehlivé dat tok a požití obrázku. Ovládací prvek, jež jsme musí zajistit, můžeme určit latence data. To je zadaný jako časový rozdíl mezi údaj o čase, vlastní měření pořízení a času pro niž byl načtena do úložiště Cortana Intelligence sady a je připraven k použití. Krátkodobá zatížení Prognózování Celková latence nesmí být větší než 30 minut. Pro načtení dlouhodobou Prognózování Celková latence nesmí být větší než 1 dnem.

### <a name="data-preparation-and-feature-engineering"></a>Příprava dat a inženýrské funkce
Po nezpracovanými daty byl požití (viz téma **Dat požití**) a bezpečné uložen, je připravená k zpracovat. Fáze Příprava dat v podstatě záznam nezpracovanými daty a převod (transformace, či) do formuláře pro modelování fáze. Jednoduchý operací, jako je použití neformátovaná data sloupce stejně jako se skutečné naměřené hodnoty, standardní hodnoty, složitější operací, jako je [čas vzhledem](https://en.wikipedia.org/wiki/Lag_operator)a ostatní, která může obsahovat. Nově vytvořený datové sloupce, které se označují jako funkce data a proces generování tyto se nazývá inženýrské funkce. Na konci tohoto procesu nám novou sadu dat, který má odvozeno z nezpracovanými daty a se dá použít pro modelování. Kromě toho fáze Příprava dat je potřeba starat o chybějících hodnot (viz **Kvality dat**) a jejich vyrovnání. V některých případech bude taky potřebujeme normalizovat data a ujistěte se, že všechny hodnoty jsou zobrazeny ve stejné měřítko.

V této části v seznamu některých funkcí běžné dat, které jsou součástí energie prognózy poptávka modely.

**Čas řízený úsilím funkcí:** Tyto funkce jsou odvozeny od dat Datum a časové razítko. Jedná se extrahovaných, převedený kategorií funkcí, jako je:
-   Čas dne – to je hodinu dne, který má hodnoty od 0 do 23
-   Den týdne – to představuje den v týdnu a trvá hodnoty od 1 (neděle) až 7 (sobota)
-   Den měsíce – to představuje aktuální datum a může obsahovat hodnoty od 1 do 31
-   Měsíc roku – to představuje měsíc a trvá hodnoty od 1 (leden) do 12 (prosinec)
-   Považováno za víkendové – jedná se o funkci binární hodnotu, která vezme hodnoty 0 pro považováno za víkendové dny v týdnu nebo 1
-   Sváteční – jedná se o funkci binární hodnotu, která přijímá hodnoty 0 běžná den nebo 1 pro svátek
-   Analytický nástroj Fourierova – podmínek Fourierova patří gramáží, které jsou odvozeny od časové razítko a slouží k zaznamenání sezónnost (cyklů) v datech. Protože jsme může obsahovat více období naše data potřebujeme více podmínek Fourierova. Například služba hodnoty pravděpodobně roční týdenní a denní období a cykly které vyplynou řečeno Fourierova 3.

**Funkce nezávislé měření:** Funkce nezávislé zahrnout všechny prvky dat, které nám chcete použít jako prognostické v naší modelu. Tady jsme vyloučit závislá funkci, která by potřeba předpovídání.
-   Funkce prodlevy – jedná se o čas posunuté hodnoty aktuální. Funkce prodlevy 1 například bude obsahovat hodnotu služba za předchozí hodiny (za předpokladu, že hodinové údaje) relativní aktuální časové razítko. Podobně jsme můžete přidat prodlevy 2, zpoždění 3, *atd*. Skutečné kombinací prodlevu funkcím, které jsou používány jsou určeny fázi modelování hodnocení výsledků modelu.
-   Dlouhodobě trendu – tato funkce představuje lineární nárůst poptávka mezi let.

**Závislá funkce:** Závislé funkce je sloupec dat, které budeme rádi našeho modelu odhadnout. S [výukové sledovaných počítače](https://en.wikipedia.org/wiki/Supervised_learning)potřebujeme nejdřív školení modelu pomocí funkce závislá (což je označovány jako popisky). Díky modelu další vzorků v data spojená se funkci závislá. V energie služba prognózy obvykle chceme předpovídání aktuální a proto by ji používáme jako funkce závislá.

**Zpracování chybějících hodnot:** Příprava fázi dat by potřebujeme určit nejlepší strategii zpracovávání chybějících hodnot. Důvodem je většinou pomocí různých statistických [metod imputace data](https://en.wikipedia.org/wiki/Imputation_(statistics)). V případě energie Prognózování služba, jsme obvykle dává chybějících hodnot pomocí klouzavý průměr z předchozí dostupné datové body.

**Normalizace dat:** Normalizace dat je jiný typ transformace, které se používá k přenesení všechny číselná data například služba prognózy do podobné měřítko. To obvykle vám pomůže zvýšit přesnost modelu a s přesností. Jsme by většinou to udělat tak, že vydělí hodnotu skutečné rozsah dat.
To bude Neomezovat původní hodnoty do menší rozsah, obvykle mezi -1 a 1.

## <a name="modeling"></a>Modelování
Fáze modelování je, kde k převodu dat do modelu dojde. V hlavní části tohoto procesu jsou rozšířené algoritmech skenování historických data (data, školení), extrahovat vzorek a sestavíte modelu. Tento model později lze předpovídání na nová data, která nepoužíval k vytvoření modelu.

Po máme pracovní spolehlivé model jsme pak ji můžete použít k získání nová data, která je strukturovanými zahrnout požadované funkce (X). Proces bodování pokusy pomocí trvalých modelu (objekt ve fázi školení) a předpovídání proměnnou cílové, která je označená Ŷ.

### <a name="demand-forecasting-modeling-techniques"></a>Služba Prognózování modelování postupy
V případě poptávka Prognózování částech budeme používat historických dat, která se seřazenými podle času. Obecně označovány data, která zahrnuje časovou dimenzi jako [časovou řadu](https://en.wikipedia.org/wiki/Time_series). Hledání v časové řady modelování je najít čas související trendů, sezónnost, automatického korelace (korelace v čase) a formulace můžou být do modelu.

Poslední roky Upřesnit algoritmů byla vytvořili tak, aby zahrnoval časové řady a ke zlepšení přesnosti prognózy. Tady je několik stručně probereme.

> [AZURE.NOTE] Tato část není určená k jako počítače učení a předpovídání přehled, ale raději jako krátké průzkumu modelování postupy, které se běžně používají pro službu Prognózování. Další informace a vzdělávací materiály o časové řady., doporučujeme příručce online [prognostických: Principy a praktické cvičení](https://www.otexts.org/book/fpp).

#### <a name="ma-moving-averagehttpswwwotextsorgfpp62"></a>[**MA (klouzavého průměru)**](https://www.otexts.org/fpp/6/2)
Klouzavý průměr je jednou z první analytické techniky použité pro časové řady a je pořád jedním z nejčastěji nejčastěji používaná postupy k dnešnímu dni. Je také základem rozšířené Prognózování postupy. S klouzavý průměr jsme jsou Prognózování další datový bod tak, že výpočet průměru přes K poslední body, kde K označuje pořadí klouzavého průměru.

Klouzavý průměr postup má za následek nástroj vyrovnání prognózy a tedy nemusí taky velké pohyblivostí data zpracovat.

#### <a name="ets-exponential-smoothinghttpswwwotextsorgfpp75"></a>[**ETS (analytický nástroj Exponenciální vyrovnání)**](https://www.otexts.org/fpp/7/5)
Exponenciální vyrovnání (ETS) je skupina různé metody, které používají váženého průměru posledním datovým bodům za účelem odhadu další datový bod. Myšlence je přiřadit novější hodnoty vyšší gramáží a postupně zmenšit tento weight (váha) pro starší naměřené hodnoty. Existuje několik různých metod s Tato řada, některé z nich zahrnout data například [Holt Winters sezónní způsob](https://www.otexts.org/fpp/7/5)zpracování hodnot sezónnosti.

Některé z těchto metod i faktor sezónnost data.

#### <a name="arima-auto-regression-integrated-moving-averagehttpswwwotextsorgfpp8"></a>[**ARIMA (automatického regresní integrované přesunutí průměr)**](https://www.otexts.org/fpp/8)
Automatické regresní integrované přesunutí průměr (ARIMA) je jiný rodině metod běžně používané pro předpovídání časové řady. Sloučí prakticky automatického regresní metody s klouzavý průměr. Automatické regresní metody pomocí regresní modely – stačí předchozí časové řady hodnoty pro výpočet další místo data. Metody ARIMA také použít rozdílový metody, které obsahují výpočet rozdílu mezi datovými body a pomocí můžou být místo původního naměřené hodnoty. Nakonec ARIMA také využívá technik klouzavého průměru popsaných výše. Co vytvoří řadu způsobů ARIMA je kombinace všech z těchto postupů různými způsoby.

ETS a ARIMA široce slouží dnes pro prognózování poptávka energie a mnoha dalších prognózy problémy. V mnoha případech tyto zkombinují společně při velmi přesné výsledky.

**Analytický nástroj Regrese více obecné** Regresní modely může být nejdůležitější přístup modelování v rámci domény výukové počítače a statistiky. V souvislosti s časovou řadu používáme regresní předpovídat budoucí hodnoty (*například*požadavků). V regresní jsme prohlédněte lineární kombinací prognostické a zjistěte hmotnost (označuje také jako část) tyto prognostické během procesu školení. Cílem je plodiny regresní čáry, které bude prognózy naše předpovězené hodnoty. Regresní způsoby vhodné při proměnná cílové číselné a tedy také vešel časové řady.. Existuje široká škála metod regresní včetně velmi jednoduché regresní modely například [Lineární regrese](https://en.wikipedia.org/wiki/Linear_regression) a pokročilejší z nich například stromové struktury rozhodování, [Náhodná strukturami](https://en.wikipedia.org/wiki/Random_forest), [Neural sítí](https://en.wikipedia.org/wiki/Artificial_neural_network)a zesílen stromové struktury rozhodování.

Služba energie Prognózování jako problematický regresní stavba dostaneme spoustu pružnost při výběru naše datové funkce, které můžete kombinovat z dat časové řady skutečné služba a externí faktory například teplotu. Další informace o vybrané funkcích jsou uvedeny ve funkci konstrukce (viz **Příprava dat a funkce technickým**) část tohoto playbook.

Z naší zkušenosti s implementaci a nasazení energie poptávka prognózy pilot byl zjištěn Upřesnit regresní modely, které jsou k dispozici v Azure ML mají za následek nejlepší výsledky a částech budeme používat z nich.

## <a name="model-evaluation"></a>Model hodnocení
Model hodnocení má kritické role v rámci **Cyklu vývoje modelu**. V tomto kroku podíváme do ověřování modelu a jeho výkon skutečných daty. Během kroku modelování používáme část dat, k dispozici pro vzdělávání modelu. Během zkušební fáze trvat jsme zbytek data, abyste vyzkoušeli modelu. Prakticky znamená to, že jsme podávání modelu nová data, která obsahuje změnili a obsahuje stejné funkce jako datovou sadu školení. Však během procesu ověření používáme modelu předpovídání proměnnou cílové spíše než poskytují proměnnou dostupné cílové. Se často označují tento proces jako model hodnocení. By pak použijte hodnoty true cílové jsme porovnat s předpovídané z nich. Cílem tady je změřit a minimalizace předpovědí chyba znamená rozdíl mezi předpovědí a skutečnou hodnotou. Klíč vypočte měření chyb je, protože rádi doladit modelu a ověřit, zda skutečně zmenšení chyby do schránky. Doladění model lze provést úpravou parametry modelu, kterými se řídí procesem naučná nebo přidáním nebo odebráním funkce dat (jako takzvaný [Možnosti Uklidit parametry](https://channel9.msdn.com/Blogs/Windows-Azure/Data-Science-Series-Building-an-Optimal-Model-With-Parameter-Sweep)). Prakticky, která znamená, že možná bude potřeba zapracovávat ji mezi inženýrské funkce modelování a model hodnocení fáze tisknutím dokud jsme snížit chybu na požadovanou úroveň.

Je důležité ji zdůraznění, po které chyby předpovědí nikdy bude nulové je nikdy model, který můžete přesně předpovídání každé výsledek. Je ale určitého rozsahu chyby, přijatelnou podniku. Během procesu ověření rádi zajistěte, aby byl naše chyby předpovědí modelu na úrovni nebo vyšší než úroveň odchylky business. Proto je třeba nastavit úroveň přípustné chyby na začátku obrázku ve fázi **Formulace problém** .

### <a name="typical-evaluation-techniques"></a>Typické hodnocení postupy
Existují různé způsoby, ve které prediktivní vkládání chyby měří a kvantifikovány. V této části jsme se zaměřuje diskuse o hodnocení postupech na časové řady a specifické pro službu energie prognózy.

#### <a name="mapehttpsenwikipediaorgwikimeanabsolutepercentageerror"></a>[**MAPE**](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error)
MAPE zastupuje nechtěli absolutní procentuální chyby. S MAPE jsme výpočet rozdílu mezi každý naplánované čárky a skutečnou hodnotou tomuto bodu. Potom jsme podpořte chyby na jeden bod výpočtem část rozdíl relativní skutečné hodnoty. V posledním kroku jsme průměrné tyto hodnoty. Matematický vzorec použitý pro MAPE je následující:

![Vzorec MAPE](media/cortana-analytics-playbook-demand-forecasting-energy/mape-formula.png)
*kde<sub>t</sub> aj je skutečná hodnota, F,<sub>t</sub> je předpovězené hodnoty a n je prognózy horizont.*

## <a name="deployment"></a>Nasazení
Jakmile jsme nailed dolů fáze modelování a ověřit výkonu modelu jsme připravená k odeslání do fáze nasazení. V tomto kontextu nasazení znamená, že povolení zákazníkovi používat model tak, že se systémem skutečné předpovědí jej širokou škálu. Nasazení se klíč v Azure ML od naším hlavní cílem je pořád vyvolat předpovědí namísto jenom získání přehled od data. Fáze nasazení zadejte tu část, kde zapnut model spotřebované ve velkém měřítku.

V rámci energie poptávka prognózy náš cílem je vyvolat prognózy nepřetržitý a pravidelných zároveň zajistit, že je k dispozici pro model aktualizovat data a odeslání Předpovězená data zpět náročné klientovi.

### <a name="web-services-deployment"></a>Nasazení webové služby
Hlavní nasadit stavebních bloků v Azure ML je webová služba. Toto je co nejefektivněji způsob, jak povolit spotřebu prediktivní modelu v cloudu. Webová služba zapouzdřuje modelu a zalamuje s [RESTful](http://www.restapitutorial.com/) rozhraní API (Application Programming Interface). Rozhraní API mohou sloužit jako součást jakýkoli kód klienta, jak je ukázáno v následujícím diagramu.

![Jsme nasazení služby a spotřeby](media/cortana-analytics-playbook-demand-forecasting-energy/web-service-deployment-and-consumption.png)

Jak je vidět, webová služba nasazenou v cloudu sadu Intelligence Cortana a lze zavolat a nad jeho vystavovaných rozhraní REST API koncový bod. Jiný typ klienty v různých doménách může vyvolat služby prostřednictvím rozhraní API webových současně. Webová služba můžete taky přizpůsobit podporují tisíce současné volání.

### <a name="a-typical-solution-architecture"></a>Typické řešení architektura
Když nasadíte požadavek energie Prognózování řešení, jsme zájem o nasazení konce řešení, které přesahuje webová služba předpovědí a usnadňuje celý datový tok. V době jsme vyvolat novou prognózu by potřebujeme abyste měli jistotu, že do modelu krmena s funkcemi aktuální data. Která znamená, že nově shromážděných nezpracovanými daty je pořád požití, zpracování a transformovat do požadované funkce nastavené na které tvořily původní modelu. Ve stejnou dobu byste rádi zpřístupnit Předpovězená data pro ukončení jinými klienty. Příklad dat toku obrázku (nebo s datového kanálu) je znázorněn v diagramu pod:

![Energetický poptávka prognózy toku dat konce](media/cortana-analytics-playbook-demand-forecasting-energy/energy-demand-forecase-end-data-flow.png)

Tyhle kroky, které proběhnout jako součást prognózy obrázku služba energii:
1.  Miliony metry nasazeném dat pořád generují dat spotřebu energie v reálném čase.
2.  Tato data, která má být shromažďované a nahrát do cloudové úložiště (*například*objektů Blob Azure).
3.  Před zpracovávání, je agregované nezpracovanými daty transformovny nebo místní úroveň podle podniku.
4.  Funkce zpracování (viz **Příprava dat a zpracování funkci**) potom probíhá a vrátí data, která je nutný pro vytváření modelů školení nebo bodování – funkce sadu data se ukládají v databázi (*například*SQL Azure).
5.  Volání znovu školení služby Pokud chcete znovu školení model prognózy – této aktualizovanou verzi modelu zachován tak, aby ji může používat bodování webové služby.
6.  Při plánování, která vyžaduje prognózy počet_plateb vyvolání bodování webové služby.
7.  Předpokládané data se ukládají v databázi, která můžete přistupovat klient spotřebu ukončit.
8.  Klient spotřebu načte prognózy, platí zpět do mřížky a spotřebovává v souladu s případu vyžaduje použití.

Je důležité mít na paměti, že tento celého obrázku je plně automatické a spustí podle plánu. Celý průběhu tato data obrázku lze provést pomocí nástrojů, jako jsou [Azure Data Factory](http://azure.microsoft.com/services/data-factory/).

### <a name="end-to-end-deployment-architecture"></a>Architektura koncové nasazení
Abyste mohli nasadit prakticky energie služba prognózy řešení pro na Cortana měřítka, potřebujeme ověřit, že požadované součásti zřídit a správně nakonfigurované.

Následující obrázek znázorňuje typické architektura Cortana řady na základě implementuje a orchestrates cyklus toku dat, který je výše uvedeného postupu:

![Architektura koncové nasazení](media/cortana-analytics-playbook-demand-forecasting-energy/architecture.png)

Další informace o všech součásti a celou architekturu získáte šablona řešení energie.
