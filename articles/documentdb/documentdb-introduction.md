<properties 
    pageTitle="Úvod k DocumentDB, databázi JSON | Microsoft Azure" 
    description="Informace o Azure DocumentDB databázi NoSQL JSON. Tento dokument databáze vytvořené pro velké dat, pružná škálovatelnost a dostupnost." 
    keywords="JSON databáze, dokument databáze"
    services="documentdb" 
    authors="mimig1" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="09/13/2016" 
    ms.author="mimig"/>

# <a name="introduction-to-documentdb-a-nosql-json-database"></a>Úvod k DocumentDB: NoSQL JSON databáze

##<a name="what-is-documentdb"></a>Co je DocumentDB?

DocumentDB je plně spravované služby NoSQL databáze vytvořené pro rychlé a předvídatelná výkonu dostupnost, pružná měřítko, globální distribuční a snadnější vývoj. Jako NoSQL databázi uvolnit schématu DocumentDB poskytuje bohaté a známé funkce dotaz SQL s konzistentní zhoršeným čekacích dob JSON dat – zajistit, že jsou 99 % vaší čtení podávané množství ve skupinovém rámečku 10 milisekund a 99 % vaší zápisy jsou doručeny nižší než 15 milisekund. Tato nastavení jedinečných výhod DocumentDB skvělá přizpůsobení webových stránek, mobilní, hraní a IoT a mnoha dalších aplikací, které potřebují bezproblémové měřítko a globální replikace.

## <a name="how-can-i-learn-about-documentdb"></a>Kde najdu Další informace o DocumentDB? 

Rychlý způsob, jak informace o DocumentDB a podívejte se, jak se tyto tři kroky: 

1. Podívejte se na dvě minuty [Co je DocumentDB?](https://azure.microsoft.com/documentation/videos/what-is-azure-documentdb/) videa, která představuje výhody používání DocumentDB.
2. Podívejte se na tři minuty [Vytvořit DocumentDB na Azure](https://azure.microsoft.com/documentation/videos/create-documentdb-on-azure/) videa, která zvýrazní jak začít s DocumentDB pomocí portálu Azure.
3. Přejděte [Hřišť dotazu](http://www.documentdb.com/sql/demo), kde můžete procházet různé činnosti Další informace o propracovaných dotazu funkce dostupné v DocumentDB. Potom sídlo myši na kartu izolovaného prostoru spustit vlastní dotazy SQL a Experimentujte s DocumentDB.

Vraťte se na tento článek kde jsme budete Prozkoumat důkladněji.  

## <a name="what-capabilities-and-key-features-does-documentdb-offer"></a>Jaké funkce a klíčové funkce nabízí DocumentDB?  

Azure DocumentDB nabízí následující klíčové funkce a výhody:

-   **Elastically scalable výkonu a úložiště:** Snadno škálování nebo Neomezovat DocumentDB JSON databáze podle vašich potřeb aplikace. Data uložena na plnou stavu discích (SSD) pro nízkou předvídatelná čekacích dob. DocumentDB podporuje kontejnery pro ukládání JSON dat s názvem kolekce, které lze přizpůsobit velikosti neomezený úložiště a zřizování výkon. Můžete elastically zmenšit DocumentDB předvídatelná výkon Bezproblémová narůstající velikostí aplikace. 

-   **Replikace více oblastí:** DocumentDB transparentně zreplikuje dat na všechny oblasti, které jste přidruženého k vašemu účtu DocumentDB umožňuje vyvíjet aplikace, které vyžadují globální přístup k datům zároveň poměry mezi konzistence, dostupnost a výkonu, všechna odpovídající záruky. DocumentDB obsahuje průhledné místního převzetí s více homing rozhraní API a možnost elastically měřítko výkon a úložiště po celém světě. Přečtěte si další v [distribuovat data globálně s DocumentDB](documentdb-distribute-data-globally.md).

-   **Ad hoc dotazů pomocí známé syntaxe jazyka SQL:** Ukládání nesourodými JSON dokumentů, které tvoří DocumentDB a dotaz tyto dokumenty pomocí známé syntaxe jazyka SQL. DocumentDB využívá vysoce souběžné zámek zdarma, protokolu strukturovanými technologie indexování automaticky indexovat celého obsahu dokumentu. Díky bohaté v reálném čase dotazů, aniž by bylo nutné zadat schématu tipů, sekundární indexy nebo zobrazení. Další informace najdete v [DocumentDB dotazu](documentdb-sql-query.md). 

-   **JavaScript spuštění databáze aplikace:** Expresní aplikační logiku uložené procedury a aktivačních událostí a funkce definované uživatelem (UDF) pomocí standardní JavaScript. Díky logiky aplikace k ovládání nad daty bez obav neshodu mezi aplikací a schématu databáze. DocumentDB obsahuje celá transakční provádění logických aplikace JavaScript přímo do databázový stroj. Rozsáhlá integrace JavaScript Povolí provedení Vložit nahradit, odstranit a vyberte operace v rámci sady JavaScript jako izolace transakce. Další informace najdete v [DocumentDB serverovou programování](documentdb-programming.md).

-   **Optimalizovat konzistence úrovně:** Výběr textu od čtyři úrovně jasně definované konzistence dosažení optimálního poměr konzistenci a výkonu. Pro dotazy a další operace DocumentDB nabízí čtyři úrovně různých konzistence: silné ohraničených staleness, relace a případné. Tyto limity podrobného, dobře definovaný konzistence umožňují zvukové střídání mezi konzistence, dostupnost a latence. Další informace najdete v [pomocí úrovní konzistence maximalizovat dostupnost a výkon DocumentDB](documentdb-consistency-levels.md).

-   **Plně spravovaných:** Odstranění potřeba spravovat databáze a automatické zdroje. Jako do služby Microsoft Azure plně Správa přístupových práv není nutné spravovat virtuálních počítačích, nasazení a software nakonfigurovat tak, spravovat měřítka nebo zacházet s upgrady složité datové vrstvy. Všechny databáze je automaticky zálohovala a zamknut místní k chybám. Můžete snadno přidat účet DocumentDB a zřízení kapacitu, jako je třeba, umožňuje zaměřit na vaše verze aplikace místo provozní a Správa databáze. 

-   **Otevřete návrh:** Začněte rychle pomocí existujících dovedností a nástrojů. Programování proti DocumentDB je jednoduché, přístupné a nevyžaduje přijmout nové nástroje nebo řídit vlastní rozšíření JSON nebo JavaScriptu. Přístup ke všem databáze funkcí včetně CRUD dotazu a JavaScript zpracování prostřednictvím jednoduchých rozhraní RESTful HTTP. DocumentDB zahrnuje existující formáty, jazycích a standardy a nabízí vysoké hodnoty možností databáze nad nimi.

-   **Automatické indexování:** Ve výchozím nastavení DocumentDB [automaticky indexy](documentdb-indexing.md) všechny dokumenty v databázi a očekávat ani vyžadují schématu nebo vytvoření sekundární indexy. Nechcete, aby indexovat všechno? Nedělejte si starosti, můžete [vyjádření výslovného nesouhlasu cesty v souborech JSON](documentdb-indexing-policies.md) příliš.

##<a name="data-management"></a>Jak DocumentDB Správa dat

Azure DocumentDB spravuje JSON dat prostřednictvím dobře definovaný databáze zdrojů. Tyto materiály replikovat vysoké dostupnosti a jsou jedinečně s možností zadání podle jejich logické URI. DocumentDB nabízí jednoduchý HTTP na základě RESTful programovací model pro všechny zdroje. 

Účet DocumentDB databáze je jedinečný obor názvů, který vám umožňuje přístup k Azure DocumentDB. Než budete moct vytvářet účet databáze, musíte mít předplatné Azure, která vám umožňuje přístup k různým služby Azure. 

Všechny zdroje v rámci DocumentDB jsou modelovat a uložených jako JSON dokumenty. Zdroje je spravováno jako položky, které jsou JSON dokumenty obsahující metadat a jako informační kanály, což jsou kolekce položek. Sady položek, jsou obsaženy v příslušných informační kanály RSS.

Na následujícím obrázku vidíte vztahy mezi DocumentDB zdroje:

![Hierarchické struktury mezi zdrojů v DocumentDB, NoSQL JSON databáze][1] 

Účet databáze obsahuje sadu databází, každá obsahuje víc kolekcí, z nichž každá obsahuje uložené procedury aktivačních událostí, funkce definované uživatelem a dokumenty související přílohy. Databáze také přidruženou uživatelů, oba objekty mají úroveň oprávnění pro přístup k různé jiné kolekce, uložené procedury, aktivačních událostí, funkce definované uživatelem, dokumenty nebo přílohy. Během databází, uživatele, oprávnění a kolekcí jsou definované systém zdroje s známý schématy – dokumenty uložené procedury, aktivačních událostí, funkce definované uživatelem a přílohy obsahují libovolné definované uživatelem JSON obsah.  

##<a name="develop"></a>Jak můžete vyvíjet aplikace s DocumentDB?

Azure DocumentDB zpřístupňuje zdrojů prostřednictvím rozhraní REST API, kterou můžete volat jazykem schopný provádět žádosti o protokolu HTTP/HTTPS. Kromě toho DocumentDB nabízí programovací knihoven pro více jazyků Oblíbené. Tyto knihovny zjednodušit aspektech o práci s Azure DocumentDB zpracováním podrobnosti, například adresu ukládání do mezipaměti, správě výjimek, automatické opakování a podobně. Knihovny jsou momentálně dostupné pro následující jazyky a platformy:  

Ke stažení | Si přečtěte následující dokumentaci
--- | ---
[.NET SDK](http://go.microsoft.com/fwlink/?LinkID=402989) | [.NET knihovny](https://msdn.microsoft.com/library/azure/dn948556.aspx)
[Node.js SDK](http://go.microsoft.com/fwlink/?LinkID=402990) | [Node.JS knihovny](http://azure.github.io/azure-documentdb-node/)
[Java SDK](http://go.microsoft.com/fwlink/?LinkID=402380) | [Knihovna Java](http://azure.github.io/azure-documentdb-java/)
[JavaScript SDK](http://go.microsoft.com/fwlink/?LinkID=402991) | [Knihovna JavaScript](http://azure.github.io/azure-documentdb-js/)
není k dispozici | [Serverový JavaScript SDK](http://azure.github.io/azure-documentdb-js-server/)
[Python SDK](https://pypi.python.org/pypi/pydocumentdb) | [Python knihovny](http://azure.github.io/azure-documentdb-python/)

Rámec základních vytvořit, číst, aktualizovat a odstraňovat operace DocumentDB poskytuje bohaté SQL dotazu pro načítání JSON dokumenty a na straně serveru podpory transakční provádění logických aplikace JavaScript. Rozhraní spuštění dotazu a skript jsou k dispozici prostřednictvím všechny knihovny platformy, jakož i rozhraní REST API. 

### <a name="sql-query"></a>Dotaz SQL
Azure DocumentDB podporuje zjišťování dokumenty pomocí jazyka SQL, který se zobrazuje v JavaScriptu typ systému a výrazy s podporou relační hierarchické a prostorové dotazů. Dotazovací jazyk DocumentDB je jednoduchá a zároveň velmi užitečná rozhraní pro dokumenty JSON dotazu. Jazyk podporuje podmnožinu ANSI SQL gramatiky a přidá hloubkové integrace JavaScript object matice, vytváření objektů a vyvolání funkce. DocumentDB poskytuje svůj dotaz model bez explicitní schématu a indexování Tipy od vývojáře.

Uživatelsky definované funkce (UDF) můžete registrovaný u DocumentDB a uváděný jako součást příkazu jazyka SQL a tím rozšíření gramatiky pro podporu logiky vlastní aplikaci. Tyto funkce definované uživatelem jsou uváděný jako JavaScript programy a spouštět v databázi. 

Pro vývojáře .NET DocumentDB také nabízí zprostředkovatele LINQ dotazu jako součást [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.aspx). 

### <a name="transactions-and-javascript-execution"></a>Transakce a spuštění JavaScript
DocumentDB umožňuje napsat logiku aplikace jako pojmenovaný programy napsané úplně v JavaScriptu. Tyto aplikace jsou registrované pro kolekci a můžete problému s dokumenty v rámci dané kolekce operace databáze. JavaScript můžete registrované pro spuštění jako aktivační událost, uložené procedury nebo funkce definované uživatelem. Aktivačních událostí a uložené procedury můžete vytvořit, číst, aktualizovat a odstraňovat dokumenty, že uživatelsky definované funkce Spustit jako součást logiku spuštění dotazu bez přístup pro zápis ke kolekci.

Spuštění JavaScript v rámci DocumentDB modelovat po koncepty podporované systémy relační databáze, s JavaScript jako moderní náhrady samolepicích jazyce Transact-SQL. Použití logických operátorů všechny JavaScript se spustí v rámci okolního kyseliny transakce s izolace snímek. V průběhu spuštění Pokud JavaScript výjimku, potom přerušení celé transakce.

## <a name="next-steps"></a>Další kroky
Už máte účet Azure? Potom mohli rovnou začít s DocumentDB [Azure portál](https://portal.azure.com/#gallery/Microsoft.DocumentDB) vytvořením [účet DocumentDB databáze](documentdb-create-account.md).

Nepoužíváte účet Azure? Můžeš:

- Registraci [Azure bezplatnou zkušební verzi](https://azure.microsoft.com/free/), která vám 30 dní a 200 $ zkusit Azure služby. 
- Pokud máte předplatné MSDN, máte nárok na [150 v bezplatné Azure přeplatky měsíčně](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) na Azure služby používat. 

Až budete připravení na další informace, navštivte naše [cesta výuky](https://azure.microsoft.com/documentation/learning-paths/documentdb/) přejděte k dispozici výukové materiály. 


[1]: ./media/documentdb-introduction/json-database-resources1.png
 
