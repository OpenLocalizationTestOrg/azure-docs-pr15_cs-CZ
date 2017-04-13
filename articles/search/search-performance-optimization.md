<properties 
    pageTitle="Azure aspektech výkonu a optimalizace vyhledávacího | Microsoft Azure" 
    description="Ladění výkonu Azure vyhledávání a konfigurace optimální měřítko" 
    services="search" 
    documentationCenter="" 
    authors="LiamCavanagh" 
    manager="pablocas" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="10/17/2016" 
    ms.author="liamca"/>

# <a name="azure-search-performance-and-optimization-considerations"></a>Azure hledání výkon a optimalizace co byste měli zvážit

Skvělé vyhledávání je klíčová k úspěchu pro mnoho mobile a webových aplikací. Z nemovitostí a použít auta Tržiště na online katalogy rychlé hledání a odpovídajících výsledků ovlivní prostředí zákazníků. Tento dokument je určen nápovědě zjistíte osvědčené postupy při tom, jak naplno Azure vyhledat, zejména pokročilé scénáře sofistikované požadavkům pro škálovatelnost, více jazyků pro podporu nebo vlastní řazení.  Kromě toho tento dokument obsahuje přehled interní a zahrnuje postupů, které efektivně pracovat v aplikacích reálný zákazníka.

## <a name="performance-and-scale-tuning-for-search-services"></a>Výkon a měřítko optimalizace pro vyhledávací služby

Jsou všechny použijeme vyhledávače, jako je Bing a Google a vysoký výkon, které nabízejí.  Jako výsledek Pokud zákazníky pomocí vyhledávání s podporou webu nebo mobilní aplikaci, bude očekávají podobné ukazatele výkonu.  Optimalizace výkonu vyhledávání, jednu osvědčené postupy při zaměření na latence, což je doby trvání dokončení a vraťte se výsledky dotazu.  Když optimalizace pro vyhledávací latence je důležité:

1. Vyberte je cílový latence (nebo maximální počet hodin), požádat o typické hledání by měla být provedena.

2. Vytvoření a otestujte skutečné pracovní zátěž před vyhledávací službu s reálné datovou sadu změřit tyto latence sazby.

3. Začít s nízkou počet dotazů za sekundu (QPS) a pokračujte zvětšíte číslo spustit ve sloupci Podmínka, dokud nebude latence dotazu vynechává pod latence definovaným cílem.  Toto je důležité srovnávacích týkající se plánování měřítko narůstající aplikace v části použití.

4. Pokud je to možné, znovu použít HTTP připojení.  Pokud používáte .NET SDK vyhledávací Azure, znamená to měli znovu použít instance nebo [SearchIndexClient](https://msdn.microsoft.com/library/azure/microsoft.azure.search.searchindexclient.aspx) instance, a pokud používáte rozhraní REST API, by měl opakované použití jedné HttpClient.
 
Při vytváření tyto test zatížení, existují některé vlastnosti vyhledávání Azure mít na paměti:

1. Je možné nabízených tak mnoho hledání dotazů v jednom okamžiku, že bude mnoha dostupných služby Azure hledání zdrojů.  Důvody tohoto chování, zobrazí se HTTP 503 odpověď kódy.  Z toho důvodu je vhodné začít s různými oblastmi žádosti o hledání najdete v článku rozdíly latence sazeb, když přidáváte další žádosti o hledání.

2. Odeslání obsahu pro vyhledávání Azure bude mít vliv na celkový výkon a latence Azure vyhledávací služby.  Pokud budete chtít odesílání dat během uživatelé provádíte hledání, je důležité pořizování pracovního vytížení zohledňovala v testů.

3. Některé vyhledávacího dotazu provede na stejné úrovni výkonu.  Například návrh vyhledávání nebo hledání dokumentů bude obvykle pracovat rychleji než dotaz se významný počet fasetami a filtry.  Je vhodné vzít různých dotazy, které budete chtít zobrazit v úvahu při vytváření testů.  

4. Variant žádostí o hledání je důležité, protože neustále provést stejné požadavky vyhledávání, ukládání do mezipaměti dat bude vytvořena aby výkon lépe než se může s více různorodé dotazu nastavují vypadal.

> [AZURE.NOTE] [Testování Visual Studio zatížení](https://www.visualstudio.com/docs/test/performance-testing/run-performance-tests-app-before-release) je skutečně spolehlivých způsobů, jak provádět srovnávacích testů jako umožňuje provádět požadavků HTTP by potřebujete pro provádění dotazů Azure hledání a umožňuje paralelního zpracování žádostí o.

## <a name="scaling-azure-search-for-high-query-rates-and-throttled-requests"></a>Změna velikosti Azure vyhledejte vysoké dotazu sazby a omezena žádosti o

Když přijímají příliš mnoho omezené požadavky nebo překročit vaší cílovou latence kurzy z načítání lepší dotazu, může samostatně snížení sazeb latence jedním ze dvou způsobů:

1. **Zvětšit repliky:**  Otevřené je jako kopii dat povolení vyhledávání Azure načíst zůstatek požadavky více kopií.  Všechny Vyrovnávání zatížení a replikace dat přes repliky se spravuje Azure vyhledávání a mohou měnit počet repliky přidělit službě kdykoli.  Může přidělit až 12 replikami ve standardní vyhledávací služby a 3 replikami služby základní hledání.  Repliky lze upravit z [Portálu Azure](search-create-service-portal.md) nebo pomocí [rozhraní API správy hledání Azure](search-get-started-management-api.md).

2. **Zvýšit úroveň hledání:**  Azure hledání je [počet úrovní](https://azure.microsoft.com/pricing/details/search/) a každý z těchto úrovní nabízí různé úrovně výkonu.  V některých případech může mít tolik dotazů vrstvy, kterou jste na neposkytuje dostatečně vysoké latence sazby, i když jsou repliky maxed se.  V tomto případě je vhodné vzít v úvahu využívání jednu vyšší úrovně hledání například Azure hledání S3 osy, která je vhodný pro scénáře s většího počtu dokumentů a úloh velmi vysoké dotazu.

## <a name="scaling-azure-search-for-slow-individual-queries"></a>Změna měřítka Azure vyhledat snížení jednotlivé dotazy

Jiného důvodu, proč může být pomalé sazby latence je z jednoho dotazu trvá příliš dlouho k dokončení.  V tomto případě přidání repliky nezlepší latence sazby.  Pro tento případ dvěma způsoby k dispozici:

1. **Zvětšení oddíly** Oddíl je mechanismus při rozdělení dat mezi navíc prostředky.  Z tohoto důvodu při přidání druhý oddíl získá datům rozdělit na dvě.  Třetí oddíl indexu rozdělí tři atd.  Má i efekt, že v některých případech pomalé dotazy bude rychlejší kvůli paralelního zpracování výpočtu.  Existuje několik příkladů odkud mají vidět tento paralelního zpracování velmi dobře fungují s dotazy, které mají zhoršeným výběru dotazů.  To je tvořen dotazy, které odpovídají více dokumentů nebo kdy faceting je třeba počty přes většího počtu dokumentů.  Protože je hodně výpočtu potřeby bodování důležitosti dokumenty nebo ke zjištění počtu dokumentů, přidání dalších oddílů pomáhají poskytují další výpočtu.  

   Může být maximálně 12 oddílů ve standardní vyhledávací služby serveru a 1 oddíl služby základní hledání.  Oddíly můžete upravit, buď z [Portálu Azure](search-create-service-portal.md) nebo pomocí [rozhraní API správy hledání Azure](search-get-started-management-api.md).

2. **Omezit pole vysoká mohutnosti:** Pole s vysokou mohutnosti se skládá z facetable nebo filtrovat pole, které obsahuje významné počet jedinečných hodnot a v důsledku toho trvá velké množství prostředků pro výpočet výsledky myší.   Například nastavení pole ID produktu nebo popis jako facetable/filtrovatelné by usnadňují práci s vysokou mohutnosti vzhledem k tomu, že většina hodnot z dokumentu do dokumentu jsou jedinečné. Pokud je to možné, omezení počtu vysoké mohutnosti pole.

3. **Zvýšit úroveň hledání:**  Přesunutí do vyšší úroveň Azure hledání může být jiným způsobem, jak zlepšit výkon pomalé dotazů.  Každý vyšší úroveň umožňuje rychlejší procesor a víc paměti, která může obsahovat pozitivní dopad na výkon dotazu.

## <a name="scaling-for-availability"></a>Změna měřítka dostupnosti

Repliky nejen snížit latence dotazu, ale můžete taky povolit vysoké dostupnosti.  Pomocí jednoho otevřené můžete očekávat pravidelných prostoje kvůli serveru restartování po aktualizace softwaru nebo jiných údržbu událostí, ke kterým dojde.  Výsledkem je důležité zvážit, pokud vaše aplikace vyžaduje dostupnost hledání (dotaz) i zápisy (indexování události).  Azure hledání nabízí možnosti SLA na všechny nabídky placené vyhledávání s následujícími atributy:

- 2 repliky dostupnost pracovního vytížení jen pro čtení (dotaz)
- 3 nebo více replikách dostupnost pro čtení i zápis úloh (dotazy a indexování)

Podrobné informace o tom navštivte [Azure hledání smlouvách úroveň](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

Protože repliky kopie dat, s několika repliky umožňuje vyhledávání Azure počítače restartování počítače a údržba proti otevřené jeden po druhém zároveň dotazů nadále spouštět oproti dalšími replikami.  Z tohoto důvodu taky musíte vzít v úvahu tento prostoje může vliv dotazy, které teď mají být spouštět oproti jednu méně kopii data.

## <a name="scaling-geo-distributed-workloads-and-provide-geo-redundancy"></a>Změna velikosti geo distributed pracovního vytížení a poskytují geo redundance

Distribuované geo úloh zjistíte, že budou mít uživatelé umístěné vzdáleném od datovém centru, které je hostovaný Azure vyhledávací služby vyšší latence sazby.  Z toho důvodu je často důležité, abyste měli více vyhledávací služby v oblasti, které jsou v blíže k těmto uživatelům.  Azure hledání Microsoft neposkytuje žádnou aktuálně metoda automatické replikace geo Azure vyhledávací indexy různých oblastí, ale existují některé postupy, které můžete použít, se kterými bude tento proces jednoduchý implementovat a spravovat. Toto jsou zobrazené v dalším částem několik.

Cíl geo distributed sadu vyhledávací služby má mít dvě nebo více indexy k dispozici ve dvou nebo více oblastí, kde bude směrovány uživatele do služby Azure vyhledávání, která poskytuje nejnižší latence popsané v tomto příkladu:

   ![Křížové – karta služeb podle regionů][1]

### <a name="keeping-data-in-sync-across-multiple-azure-search-services"></a>Udržování dat synchronizace napříč několika služby Azure hledání

Existují dvě možnosti pro uchovávání distribuované vyhledávací služby synchronní které se skládají z pomocí [Indexování hledání Azure](search-indexer-overview.md) nebo rozhraní API nabízená (označuje také jako [Rozhraní REST API pro hledání Azure](https://msdn.microsoft.com/library/dn798935.aspx)).  

### <a name="azure-search-indexers"></a>Indexování hledání Azure

Pokud používáte indexování hledání Azure, už importujete změny dat z centrální úložiště, jako jsou databáze SQL Azure nebo DocumentDB. Při vytváření nové vyhledávání služby, můžete jednoduše taky vytvořit nové indexování hledání Azure pro službu odkazující na tento stejné úložiště. Po každém nové změny přijdou do úložiště dat se budou pak indexovat podle různých indexování.  

Tady je příklad danou architekturu bude vypadat takto.

   ![Jeden zdroj dat s distribuované indexování a služby kombinace][2]


### <a name="push-api"></a>Rozhraní API připínáčku 
Pokud používáte Azure nabízená rozhraní API pro hledání aktualizaci [obsahu v Azure vyhledávacím odkazu](https://msdn.microsoft.com/library/dn798930.aspx), můžete nechat různé služby hledání synchronní stisknutím změny ke všem službám hledání pokaždé, když je potřeba aktualizace.  Když to je důležité pochopit zpracovávání případech, kdy se nezdaří aktualizaci jeden vyhledávací služby serveru a jeden nebo více aktualizace proběhla úspěšně.

## <a name="leveraging-azure-traffic-manager"></a>Využívání Azure přenosy správce

[Správce přenosy Azure](../traffic-manager/traffic-manager-overview.md) umožňuje směrování požadavků na více webů nachází geo, pak podporovaným více vyhledávací služby Azure.  Jednou z výhod správce přenosy je, že ho probe Azure vyhledávání a pokusí se zajistila možnost je k dispozici a směrovat uživatele na alternativní vyhledávací služby v případě výpadek služeb.  Kromě toho pokud směrování požadavky search prostřednictvím Azure weby Azure přenosy Manager umožňuje načtení zůstatek případech, kdy na webu nahoru, ale ne Azure vyhledávání.  Tady je příklad architektury jaké, který využívá přenosy správce.

   ![Křížové – karta služeb oblast, s centrální přenosy správce][3]

## <a name="monitoring-performance"></a>Sledování výkonu

Azure hledání nabízí možnost analyzovat a sledovat výkon poskytování služby prostřednictvím [Hledání přenosy analýzy STA ()](search-traffic-analytics.md). Až STA můžete volitelně připojit jednotlivé vyhledávání operací, stejně jako souhrnné metriky k úložišti Azure účtu, který můžete pak zpracovávané pro analýzu a vizualizovat v Power BI.  Pomocí STA metriky, můžete zobrazit údaje o výkonu například průměrný počet dotazů nebo doby odezvy dotaz.  Kromě toho protokolování operace vám umožní procházejte podrobností o konkrétní vyhledávání operací.

STA je cenný nástroj pochopit latence sazby z této pohledu Azure vyhledávání.  Protože metriky výkonu dotazu přihlášení k lyncu vycházejí z doby trvání dotazu plně zpracovat v Azure hledání (od doby, kdy jsou požadovány k, když je odesláno), budete moci umožňuje určit, zda problémy latence ze strany služby Azure vyhledávání nebo mimo služby, jako je třeba z latence sítě.  

## <a name="next-steps"></a>Další kroky

Další informace o limitech ceny úrovní a služby pro každý z nich, najdete v článku [omezení služby Azure hledání](search-limits-quotas-capacity.md).

Navštěvujte blog o [plánování kapacity](search-capacity-planning.md) Další informace o kombinace oddíl a otevřené.

Další přecházení k podrobnějším na výkon a některé ukázky jak implementovat optimalizace popisované v tomto článku následujícím videu:

> [AZURE.VIDEO azurecon-2015-azure-search-best-practices-for-web-and-mobile-applications]

<!--Image references-->
[1]: ./media/search-performance-optimization/geo-redundancy.png
[2]: ./media/search-performance-optimization/scale-indexers.png
[3]: ./media/search-performance-optimization/geo-search-traffic-mgr.png