<properties 
    pageTitle="Jak implementovat působí navigace v Azure hledání | Microsoft Azure | kategorie navigace vyhledávání" 
    description="Přidání působí navigace k aplikacím, které integrace s Azure hledání vyhledávací služby cloudu hostovaný na Microsoft Azure." 
    services="search" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="mblythe" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="10/17/2016" 
    ms.author="heidist"/>

#<a name="how-to-implement-faceted-navigation-in-azure-search"></a>Jak implementovat působí navigace v Azure hledání

Představuje položka navigace mezi působí filtrovací mechanismus poskytující vlastním přecházení k podrobnějším navigace vyhledávání. Termín "působí navigace" můžou být cizí, je ale skoro předpokladu, že jste použili jednu mezeru před. Následující příklad ukazuje, působí představuje položka navigace mezi nic jiného než kategorie použít k filtrování výsledků.

## <a name="what-it-looks-like"></a>Jak to vypadá

 ![][1]
  
Fasetami může pomoct najít, co hledáte, a zajistit, že se nebudou výsledky nula. Vývojář fasetami vám umožní vystavit nejužitečnější kritéria vyhledávání pro navigace vyhledávání souhrnu. V aplikacích online maloobchod působí navigace často vytvořené přes značky oddělení (dítěte obuv), velikost, cena, oblíbenosti a hodnocení. 

Provádění působí navigace různých technologií vyhledávání a může být velmi složité. Do pole hledání Azure působí navigace je sestaveny v době dotazu atributové pole dříve zadané ve schématu. V dotazech, který vytváří aplikace musí dotazu odesílat *Parametry dotazu podmínka* zobrazí hodnoty filtru k dispozici podmínka tento výsledek sady dokumentů. Ve skutečnosti střih výsledku dokumentu nastavte, musíte použít aplikace `$filter` výraz.

Z hlediska vývoj aplikací psaní kódu, který vytváří dotazy představuje hromadné práce. V mnoha chování aplikace, která je vhodné z působí navigace pochází od služby včetně integrovanou podporu pro nastavení oblasti a zprovoznění počty výsledků podmínka. Služba obsahuje taky smysluplné výchozí hodnoty, které vám pomůžou zabránit nepraktické navigační struktury. 

Tento článek obsahuje následující části:

- [Naučíte se vytvářet](#howtobuildit)
- [Vytvoření prezentace vrstvy](#presentationlayer)
- [Vytvoření indexu](#buildindex)
- [Kontrola kvality dat](#checkdata)
- [Vytvoření dotazu](#buildquery)
- [Tipy pro působí navigace](#tips)
- [Působí navigace na základě rozsah hodnot](#rangefacets)
- [Působí navigace založené na GeoPoints](#geofacets)
- [Vyzkoušejte si to](#tryitout)

##<a name="why-use-it"></a>Proč používat
Co nejefektivněji aplikací Vyhledávací mít více modelů interakce vedle vyhledávacího pole. Působí představuje položka navigace mezi alternativní vstupní bod pro hledání, nabízející pohodlný alternativy rukou psát složitého hledaného výrazů.

##<a name="know-the-fundamentals"></a>Znáte základy

Pokud začínáte hledání vývoj, je nejlepší způsob, jak si můžete představit působí navigace zobrazuje možnosti mají být vyhledávány vlastním. Je typ podrobnostem vyhledávání, založené na předdefinované filtry, použité pro rychle zúžit dolů výsledky hledání pomocí akce čárky a kliknutím. 

**Interakce modelu**

Vyhledávání pro navigaci na působí je opakované a tak se Pojďme začněte tím, že principy jako posloupnost dotazy, které projevovat v odpovědi na akce uživatele.

Výchozí bod je stránku aplikace, které obsahuje působí navigace, obvykle ukládané obvodu. Působí představuje položka navigace mezi často stromové struktuře se zaškrtávacími políčky pro každou hodnotu nebo můžete kliknout text. 

1.  Odeslané Azure hledání dotazu určuje působí navigační struktury prostřednictvím jeden nebo více parametrů podmínka dotazu. Dotaz může zahrnovat `facet=Rating`, například s `:values` nebo `:sort` možnost kterými dále upřesníte prezentaci.
2.  Vrstvy prezentace se vykreslují pomocí vyhledávání stránku, která obsahuje působí navigace pomocí fasetami zadanou v žádosti.
3.  Možnost působí navigační struktury, který obsahuje hodnocení, kliknutí na "4" označíte, že mají být zobrazeny pouze produkty s hodnocení 4 nebo novější. 
4.  Odpověď aplikace odesílá dotaz, který obsahuje`$filter=Rating ge 4` 
5.  Vrstvy prezentace aktualizuje stránka zobrazující sadě omezená výsledků obsahující jenom položky, které splňují nová kritéria (v tomto případě produkty hodnocení 4 a až).

Podmínka je k dotazu parametr, ale Nezaměňujte s vstup dotazu. Pracovní postup slouží nikdy jako výběru kritérií v dotazu. Místo toho, že podmínka dotazu parametrů jako vstupů pro navigační struktury, které vracejí v odpovědi. Pro každý podmínka dotazu parametr poskytovaného Azure hledání vyhodnotí, kolik dokumenty jsou ve výsledcích částečné pro každou hodnotu podmínka.

Upozornění `$filter` v kroku 4. Toto je důležitým aspektem působí navigace. I když jsou nezávislé v rozhraní API fasetami a filtry, budete potřebovat obě poskytující možnosti, které chcete, aby byla. 

**Provedeních**

V kódu aplikace vzorku je použití parametry dotazu podmínka k vrácení působí navigační struktury spolu s výsledky podmínka plus $filter výraz, který zpracovává klikněte na událost. Představte si, že `$filter` výraz jako kódem skutečné oříznutí výsledků hledání vrátil k vrstvě prezentace. Možnost podmínka barvy, kliknutí na příkaz červenou barvu prováděna až `$filter` výraz, který slouží k výběru jenom položky, které mají barvu části červená. 

**Základní úkoly v dotazu v Azure hledání**

Do pole hledání Azure žádost určeny pomocí jeden nebo více parametrů dotazu (viz [Hledat dokumenty](http://msdn.microsoft.com/library/azure/dn798927.aspx) popis každého z nich). Žádná z parametry dotazu jsou potřeba, ale je nutné mít aspoň jednu v pořadí dotazu platit.

Přesnost, obvykle rozumí možnost odfiltrovat relevantní přístupů je dosáhnout jednu nebo obě z těchto výrazech:

- **hledání =**<br/>
Hodnota tento parametr představuje hledaný výraz. Může to být jeden souvislý text nebo složitého hledaného výrazu, který obsahuje více podmínek a operátory. Na serveru používá hledaný výraz mají být vyhledávány fulltextové, dotazování vyhledávání pole v indexu u odpovídajících podmínek, nevrací žádné výsledky v pořadí řazení. Pokud nastavíte `search` na hodnotu null, dotaz spuštění překročení celý index (to znamená `search=*`). V tomto případě dalších prvků dotazu, například `$filter` nebo bodování profilu, bude se vracejí hlavní faktory, které ovlivňují které dokumenty `($filter`) a v jakém pořadí (`scoringProfile` nebo `$orderb`y).

- **$filter =**<br/>
Filtr je výkonné mechanismus při omezení velikosti výsledky hledání na základě hodnot atributy konkrétního dokumentu. A `$filter` vyhodnocují jako první, následovaný faceting použití logických operátorů generovaný dostupné hodnoty a odpovídající počty pro každou hodnotu

Výrazy složitého hledaného sníží výkonu dotazu. Pokud je to možné, používat výrazy filtru dobře vyrobeno zvýšit přesnost a zlepšit výkonu dotazu.

Chcete-li lépe porozumět tomu, jak filtru přidá větší přesnost, porovnejte složitého hledaného výrazu, které obsahuje výrazu filtru:

- `GET /indexes/hotel/docs?search=lodging budget +Seattle –motel +parking`

- `GET /indexes/hotel/docs?search=lodging&$filter=City eq ‘Seattle’ and Parking and Type ne ‘motel’`

I když oba dotazy jsou platné, druhý je nadřazené Pokud hledáte není motely s parkování v Seattlu. První dotaz závisí na těchto určitá slova uvedené nebo nejsou uvedeny v polích řetězec jako je název, popis a další pole obsahující data s možností vyhledávání. Druhý dotaz hledá přesné shody na strukturovaného dat a je pravděpodobně přesnější.

Aplikace, které obsahují působí navigace budete chtít nezapomeňte, že každé akci uživatele přes působí navigační struktury připojen zúžení výsledků hledání, dosáhnout pomocí výrazu filtru.

<a name="howtobuildit"></a>
##<a name="how-to-build-it"></a>Naučíte se vytvářet

Působí navigace vyhledávání Azure implementovaná v kódu aplikace, která vytvoří žádost, ale závisí na předdefinované prvky ve vaší schématu.

Předdefinované v hledání index je `Facetable [true|false]` atribut indexu nastavit u vybraných polí povolit nebo zakázat jejich použití působí navigační struktury. Bez `"Facetable" = true`, pole nelze použít v navigaci podmínka.

V době dotazu, aplikace kód vytvoří žádost o obsahující `facet=[string]`, žádosti o parametr, který obsahuje pole omezující vlastnost tak, že. Dotaz může obsahovat více fasetami, jako například `&facet=color&facet=category&facet=rating`, každý z nich odděleni (znak ampersand &).

Kód aplikace musíte taky vytvořit `$filter` výraz, který má zpracování událostí klikněte v působí navigaci. A `$filter` snižuje výsledky hledání pomocí je hodnota jako kritéria filtru.

Azure vyhledávání vrátí výsledky hledání na termíny zadané uživatelem, spolu s aktualizací působí navigační struktury. V Azure hledání, působí navigace je jednoúrovňový konstrukce, s hodnotami podmínka a spočítá, kolik výsledků najdou pro každou z nich.

Vrstva prezentace v kódu poskytuje uživatelského prostředí. By měl seznam částí působí navigace, například popisek, hodnoty, zaškrtněte políčka a počet. Rozhraní REST API pro hledání Azure bez ohledu na platformy, aby používat libovolný jiný jazyk, tak i platformu chcete. Důležité je obsahovat prvky uživatelského rozhraní, které podporují přírůstková aktualizace stavem aktualizované uživatelské rozhraní vybírání jednotlivých další podmínka. 

V následujících částech budou následovat bližší pohled na k vytváření jednotlivé části počínaje vrstvy prezentace.

<a name="presentationlayer"></a>
##<a name="build-the-presentation-layer"></a>Vytvoření prezentace vrstvy

Pracovní zpět z vrstvy prezentace můžete objevovat požadavky, které se propásli, jinak a porozumět tomu, které funkce jsou zásadní pro vyhledávání.

Z hlediska působí navigace stránky web nebo aplikaci zobrazí působí navigační struktury, rozpozná vstup uživatele na stránce a vloží měněných prvků. 

U webových aplikací AJAX běžně slouží v prezentaci vrstvy protože umožňuje aktualizovat přírůstková změny. Můžete taky použít ASP.NET MVC nebo jinou vizualizaci platformu s připojením k Azure vyhledávací služby prostřednictvím protokolu HTTP. Ukázková aplikace uváděný v tomto článku – **AdventureWorks katalogu** – cokoliv MVC aplikace technologie ASP.NET.

Následující příklad pořízené ze souboru **index.cshtml** vzorové aplikace vytvoří dynamická struktura HTML pro zobrazování působí navigace na stránce s výsledky hledání. Ve vzorku působí navigace je zabudované do stránky výsledků hledání a se zobrazí poté, co uživatel odeslal hledaný termín.

Oznámení, že má každý podmínka popisku (barvy, kategorie, ceny), vazbu s polem působí (barvy, categoryName listPrice) a `.count` použitým k vrácení počtu položek pro tento výsledek podmínka nebyla nalezena.

  ![][2]
 

> [AZURE.TIP] Při návrhu stránky výsledků hledání, nezapomeňte si přidat mechanismus při vymazání fasetami. Pokud používáte zaškrtávací políčka, uživatelé můžete snadno intuit jak vymazat filtry. Pro jiné rozložení bude pravděpodobně nutné navigace s popisem cesty vzorek nebo jiné kreativní přístup. Například v aplikaci ukázkové databázi AdventureWorks katalogu kliknete nadpisu, AdventureWorks katalog, obnovte stránku vyhledávání.

<a name="buildindex"></a>
##<a name="build-the-index"></a>Vytvoření indexu

Faceting je povoleno na základě pole podle pole v indexu prostřednictvím Tenhle atribut index: `"Facetable": true`.  
Všechny typy polí, které pravděpodobně lze použít v působí navigace jsou `Facetable` ve výchozím nastavení. Zahrnout tyto typy polí `Edm.String`, `Edm.DateTimeOffset`a všechny typy číselné pole (v podstatě všech typů polí jsou facetable kromě `Edm.GeographyPoint`, které nelze použít v působí navigaci). 

Při vytváření rejstříku, je nejvhodnější pro navigaci na působí explicitně vypnout faceting u polí, která má být použit nikdy jako motivem fazeta.  Zejména řetězec polí pro jednoznačné hodnot, například ID nebo produktu název, je třeba nastavit na `"Facetable": false` nechcete, aby jejich náhodné (a neefektivní) použití působí navigace.

Následuje schématu aplikace ukázkové databázi AdventureWorks katalogu (odstřihnuté některé atributů zmenšete celkovou velikost):

 ![][3]
 
Všimněte si, že `Facetable` je normálně vypnuté řetězec polí, které by neměly být použity jako fasetami, jako jsou ID nebo název. Zapnutí faceting vypnout místo, kam ho nepotřebujete pomáhá menší velikosti index a obecně zlepšuje výkon.

> [AZURE.TIP] Doporučený postup zahrnout úplnou sadu atributy indexu u každého pole. I když `Facetable` je ve výchozím skoro všech polí, záměrně nastavení jednotlivých atributů můžete představit prostřednictvím důsledky každé schématu rozhodnutí. 

<a name="checkdata"></a>
##<a name="check-for-data-quality"></a>Kontrola kvality dat 

Při vytváření zaměřené na data aplikace, Příprava dat je často jedním z větší části projektu. Hledání aplikace jsou žádná výjimka. Kvality dat má přímé vliv na tom, zda bude působí navigační struktury realizována podle očekávání nastavovaná, i její účinnosti pomoci při vytváření filtrů, které snížit výsledek.

Do pole hledání Azure souhrnu hledání tvořená dokumenty, které naplnění rejstříku. Dokumenty obsahují hodnoty, které slouží k výpočtu fasetami. Pokud chcete omezující vlastnost značku nebo cena, každý dokument by měl obsahovat hodnoty pro *položku BrandName* a *ProductPrice* , které jsou platné konzistentní a produktivní jako některou možnost filtru.

Několik připomenutí, které je potřeba přesouváním pro jsou uvedeny níže:

- Pro každé pole, která chcete podmínka tak, že položte si otázku, zda obsahuje hodnoty, které jsou vhodné jako filtry ve vlastním vyhledávání. Hodnoty musí být krátké, popisný a dostatečně odlišný nabízet vymazat volba mezi konkurenční možnosti.
- Slova s chybným pravopisem nebo téměř odpovídající hodnoty. Pokud podmínka na barvy a hodnoty polí zahrnout oranžová a Ornage (chybně napsané), fasetový založené na poli barvy by vystopovat obě.
- Smíšené změnu textu můžete také způsobí zmatek v působí navigace s oranžová a oranžová vypadají jako dva různé hodnoty. 
- Jeden i plurální verzi stejné hodnoty můžete mít za následek samostatné podmínka pro jednotlivá pole.

Jak si můžete představit, pečlivosti při přípravě dat je významnou efektivní působí navigace.

<a name="buildquery"></a>
##<a name="build-the-query"></a>Vytvoření dotazu

Kód, který napíšete pro vytváření dotazů zadají všechny části platné dotazu, včetně hledaných výrazů, fasetami, filtry, bodování profily – nic používá formulace žádost. V této části jsme budete prozkoumání kde fasetami zapadají do celkového konceptu dotazu a použití filtrů s podmínkami při předvádění sadě omezená výsledků.

Příklad je často vhodným místem pro začít. Následující příklad pořízené ze souboru **CatalogSearch.cs** vytvoří žádost vytvoří navigační podmínka podle barvy, kategorie a cena. 

Všimněte si, že fasetami nedílnou v této aplikaci vzorku. Vyhledávání v katalogu AdventureWorks slouží kolem působí navigace a filtry. Toto je zřejmé výrazném umístění působí navigace na stránce. Ukázková aplikace obsahuje identifikátor URI parametry fasetami (barvy, kategorie, ceny) jako vlastnosti na metodu Search (jak je vytvořen v ukázkové aplikaci).

  ![][4]
 
Podmínka dotazu parametr nastaven na pole a v závislosti na typu dat, můžete být další s parametry podle s hodnotami oddělenými čárkou, který obsahuje `count:<integer>`, `sort:<>`, `intervals:<integer>`, a `values:<list>`. Seznam hodnot je podporovaný pro číselná data při nastavování oblastí. Použití podrobnosti najdete v části [Hledat dokumenty (hledání Azure rozhraní API)](http://msdn.microsoft.com/library/azure/dn798927.aspx) .

Spolu s fasetami měli žádosti ze aplikace také vytvořit filtry upřesnění nastavení candidate dokumentů na základě výběru hodnotu podmínka. Bike úložiště působí navigace poskytuje vodítek na otázky, například "jaká barvy výrobci a typy jízdní kola jsou k dispozici", při filtrování odpovědi otázky jako "které přesné jízdní kola jsou červené, horská kola v tomto cena oblast".

Poté, co uživatel klikne "Červený" označíte, že mají být zobrazeny pouze červené produkty, by obsahoval další dotaz aplikace odesílá `$filter=Color eq ‘Red’`.

## <a name="best-practices-for-faceted-navigation"></a>Doporučené postupy pro působí navigace

Následující seznam obsahuje souhrn několik doporučené postupy.

- **Přesnost**<br/>
Pomocí filtrů. Pokud používáte jenom hledaných výrazů sám o sobě vyplývající může způsobit dokumentu má být vrácena, že v některém z jejích polí, která neobsahuje je přesné hodnota. 

- **Cílových polí**<br/>
V působí k podrobnostem obvykle chcete zahrnout jenom dokumenty, které mají je hodnota v konkrétním poli (působí), ne kdekoli ve všech oblastech vyhledávání. Přidání filtru zvyšuje pole Cíl přesměrováním službu hledání pouze v poli působí odpovídající hodnoty.

- **Index efektivity**<br/>
Pokud vaše aplikace používá působí navigace výhradně (to znamená bez vyhledávacím polem), můžete označit pole jako `searchable=false`, `facetable=true` k vytvoření indexu založeného na kompaktnější. Kromě toho indexování dojde, jen na celé podmínka hodnot pomocí bez konce slova nebo indexování součásti Víceslovná hodnoty.

- **Výkon**<br/>
Filtry upřesnění nastavení candidate dokumentů pro hledání a vyloučit z řazení. Pokud máte velký sadu dokumentů, pomocí velmi výběrové podmínka přechod dolů často získáte mnohem lepší výkon.


<a name="tips"></a> 
##<a name="tips-on-how-to-control-faceted-navigation"></a>Tipy pro působí navigace

Tady je list tip s pokyny pro určité problémy.

**Přidat popisky pro jednotlivá pole v navigačním panelu omezující vlastnost**

Popisky jsou obvykle definována ve formuláři (**index.cshtml** v ukázkové aplikaci) nebo HTML. Neexistuje žádné rozhraní API do pole prohledat Azure podmínka navigace štítků nebo jiný druh metadata.

**Definování polí, která mohou sloužit jako podmínka**

Odvolání určující schématu indexu polí, která jsou k dispozici pro použití motivem fazeta Za předpokladu, že je pole facetable, dotaz Určuje, která pole omezující vlastnost tak, že. Pole, podle kterého jsou faceting obsahuje hodnoty, které se zobrazí pod nápisem. 

Hodnoty, které se zobrazí pod každou popisek jsou načtená z kontingenčního seznamu index. Pokud je podmínka pole *Barva*, například umožňující další filtrování hodnoty budou hodnoty tohoto pole (červená černé a tak dále).

Číselné a data a času pouze hodnoty, je možné nastavit explicitně hodnoty v poli omezující vlastnost (například `facet=Rating,values:1|2|3|4|5`). Seznam hodnot je povolena u následujících typů polí zjednodušit oddělení podmínka výsledky v souvislé oblasti (na základě číselné hodnoty nebo časová období některý z oblasti). 

**Střih podmínka výsledků**

Podmínka výsledky jsou dokumenty nacházející se ve výsledcích hledání, které splňují podmínku podmínka. V následujícím příkladu ve výsledcích hledání *výpočetních cloudu*, 254 položky taky i *vnitřní specifikace* typu obsahu. Položky se ne nutně vzájemně vylučují. Pokud položku kritériím oba filtry, se počítá v každé z nich. Je to možné, kdy faceting na `Collection(Edm.String)` polí, které se často používá implementovat označování dokumentu.

        Search term: "cloud computing"
        Content type
           Internal specification (254)
           Video (10) 

Obecně Pokud zjistíte, že podmínka výsledky jsou trvale příliš velká, doporučujeme, aby můžete přidat další filtry způsobem popsaným v předchozích částech a udělte tak aplikaci uživatelů další možnosti pro upřesnění vyhledávání.

**Omezit počet položek v navigačním panelu omezující vlastnost**

U každého působí pole ve stromové struktuře navigace je výchozí limit 10 hodnot. Výchozí bude výstižně popisovat navigační struktury, protože udržuje seznam hodnot spravovatelnosti velikost. Přepsat výchozí nastavení přiřazením hodnoty spočítat.

- `&facet=city,count:5`Určuje, že pouze prvních 5 měst v horní seřazené výsledky jsou vráceny jako výsledek podmínka. Mít hledaný termín "letiště" a 32 shody, je-li dotaz Určuje `&facet=city,count:5`, pouze prvních pět jedinečné města s největší dokumenty ve výsledcích hledání jsou zahrnuty ve výsledcích podmínka.

Oznámení o rozdílu mezi výsledky podmínka a výsledky hledání. Výsledky vyhledávání se všechny dokumenty, které vyhovují dotazu. Podmínka výsledky jsou odpovídající položky pro jednotlivé hodnoty podmínka. V příkladu bude obsahovat výsledky hledání názvy měst, které nejsou v seznamu klasifikace faseta (5 v našem příkladu). Výsledky, které jsou filtrovány prostřednictvím působí navigace se stanou viditelnými uživateli, když se u něho vymaže fasetami nebo zvolí pořádáním kromě města. 

> [AZURE.NOTE] Projednávat `count` pokud existuje více než jeden typ může být matoucí. Následující tabulka nabízí stručný přehled použití termínu v rozhraní API pro hledání Azure, ukázkový kód a si přečtěte následující dokumentaci. 

- `@colorFacet.count`<br/>
V prezentaci kódu byste měli vidět parametr počet na podmínka, zobrazí počet výsledků podmínka. Ve výsledcích podmínka označuje počet počet dokumentů, které odpovídají na omezující vlastnost termínu nebo oblast.

- `&facet=City,count:12`<br/>
Podmínka dotazu můžete nastavit počet hodnotě.  Výchozí hodnota je 10, ale můžete nastavit tak nahoru nebo dolů. Nastavení `count:12` získá horní 12 odpovídá ve výsledcích podmínka tak, že počet dokumentů.

- "`@odata.count`"<br/>
V odpovědi na dotaz tato hodnota označuje počet odpovídající položky v seznamu výsledků hledání. Průměrně je větší než součet výsledků podmínka kombinované kvůli výskyt položky, které odpovídají hledaný termín, ale žádné shody hodnotu podmínka.


**Úrovně při působí navigace** 

Jak je uvedeno, je nepodporuje žádný přímé vnoření fasetami v hierarchii. Mimo pole působí navigace podporuje pouze jednu úroveň filtry. Však neexistuje řešení. Kódování podmínka hierarchické struktury v `Collection(Edm.String)` s jednou položkou najeďte myší na hierarchii. Implementace toto alternativní řešení je nad rámec v tomto článku, ale si můžete přečíst o kolekce OData [příkladu](http://msdn.microsoft.com/library/ff478141.aspx). 

**Ověření pole**

Pokud vytvoříte seznamu fasetami dynamicky podle nedůvěryhodných vstup, je třeba buď ověřit názvy působí polí nejsou platné, zda uniknout názvy při vytváření pomocí adresy URL `Uri.EscapeDataString()` .NET nebo na ekvivalent v svoji platformu podle výběru.

**Spočítá ve výsledcích omezující vlastnost**

Při přidávání filtru působí dotazu, můžete zachovat příkazu faseta (například `facet=Rating&$filter=Rating ge 4`). Podmínka, = není potřeba hodnocení, ale jeho vrátí počet hodnot podmínka pro hodnocení 4 nebo novější. Například pokud uživatel klikne na "4" a dotaz obsahuje filtr pro větší nebo rovno "4", počty budou vráceny pro každou hodnocení, který je 4 a nahoru.  

**Důsledky sharding na počty omezující vlastnost**

Za určitých okolností můžete zjistit, že podmínka počty neodpovídají sady výsledků (viz [působí navigace v Azure hledání (příspěvek ve fóru)](https://social.msdn.microsoft.com/Forums/azure/06461173-ea26-4e6a-9545-fbbd7ee61c8f/faceting-on-azure-search?forum=azuresearch)).

Podmínka počty může být nesprávné kvůli architektura sharding. Každý indexu vyhledávání obsahuje více shards a každý z nich sestavy fasetami horních N tak, že počet dokumentů, které jsou potom sloučena do jednoho výsledku. Některé shards transpozici velkého odpovídajících hodnot, zatímco ostatní mají menší, můžete zjistit, že některé hodnoty podmínka chybějící nebo v části nezapočítávají do výsledků.

I když toto chování může změnit kdykoli, když dojde k toto chování dnes, můžete vyřešit ji tak, že uměle nafouknutí počet:<number> obrovské číslo k jejímu vynucení úplné vykazování z každé shard. Pokud hodnota počet: je větší než nebo rovno počet jedinečných hodnot v oblasti, které jsou zaručené přesné výsledky. Však jsou-li dokument počty skutečně velká, že je snížení výkonu, takže používat tuto možnost uvážlivě.

<a name="rangefacets"></a>
##<a name="facet-navigation-based-on-a-range-values"></a>Podmínka navigace na základě rozsah hodnot

Faceting nad oblastí je běžným aplikaci požadavkem vyhledávání. Rozsahy jsou podporovány u číselná data a hodnot data a času. Další informace o jednotlivých přístupů v [Dokumentech Search (hledání Azure rozhraní API)](http://msdn.microsoft.com/library/azure/dn798927.aspx).

Azure hledání zjednodušuje oblast konstrukce zadáním dvou postupů výpočetních oblasti. Pro oba přístupy vytvoří Azure hledání odpovídající oblasti zadaných vstupní hodnoty, které jste uvedli. Například pokud zadáte hodnoty rozsahu 10 | 20 | 30 se automaticky vytvoří oblasti 0 -10 10 – 20, 20 až 30. Ukázková aplikace odeberete intervalech, které jsou prázdné. 

**Postup 1: Použít parametr interval**<br/>
Nastavovaná cena fasetami úsecích 10 zadáte:`&facet=price,interval:10`


**Postup 2: Použití seznamu hodnot**<br/>
Pro číselná data můžete pomocí seznamu hodnot.  Zvažte podmínka rozsah listPrice vykreslovat následujícím způsobem:

  ![][5]

Oblast není zadán v souboru **CatalogSearch.cs** pomocí seznamu hodnot:

    facet=listPrice,values:10|25|100|500|1000|2500

Každou oblast je vytvořené pomocí 0 jako výchozí bod, hodnotu ze seznamu jako koncový bod a potom ořízne z předchozí oblast, kterou chcete vytvořit samostatné intervalů. Azure hledání to jako součást působí navigace. Nemáte kódu pro strukturování jednotlivé intervaly.

### <a name="build-a-filter-for-facet-ranges"></a>Vytvoření filtru pro oblasti podmínka ###

Pokud chcete filtrovat dokumenty založené na oblasti vybrané uživatelem, můžete použít `"ge"` a `"lt"` filtrovat operátorů složené ze dvou částí výraz, který definuje koncové body oblasti. Například, pokud uživatel vybere rozsah 10 až 25 filtr by `$filter=listPrice ge 10 and listPrice lt 25`.

V ukázkové aplikaci pro výraz filter nastaví koncové body pomocí **priceFrom** a **priceTo** parametry. Metoda **BuildFilter** **CatalogSearch.cs** obsahuje pro výraz filter poskytující dokumentů, které tvoří oblasti.

  ![][6]

<a name="geofacets"></a> 
##<a name="filtered-navigation-based-on-geopoints"></a>Filtrované podle GeoPoints navigace

Společný jeho zobrazíte filtry, které pomůžou, jaká úložiště, restaurace nebo cíl v závislosti na jeho blízkosti vaše aktuální umístění. Tento typ filtru mohl vypadat působí navigace, je ale skutečně jenom filtru. Jsme zmínit ho tady výsledky Komu konkrétně hledáte implementaci doporučení pro tento problém konkrétní návrh.

Existují dvě funkce geografická v Azure vyhledávání **geo.distance** a **geo.intersects**.

- Funkce **geo.distance** vrátí vzdálenost km mezi dvěma body, jednoho pole a jeden je konstanty předávány jako součást filtru. 

- Funkce **geo.intersects** vrátí hodnotu true, pokud je daný bodu v rámci dané mnohoúhelníku, kde bodu je pole a mnohoúhelník není zadán jako seznam konstantní souřadnic předané jako součást filtru.

Příklady filtru můžete najít v [syntaxi výrazů OData (Azure hledání)](http://msdn.microsoft.com/library/azure/dn798921.aspx).

<a name="tryitout"></a>
##<a name="try-it-out"></a>Vyzkoušejte si to

Azure hledání Adventure Works ukázku na webu Codeplex obsahuje příklady odkazuje v tomto článku. Když pracujete s výsledky hledání, podívejte se na adresu URL pro změny v vytváření dotazu. Přidání fasetami do identifikátor URI vyberte každého z nich se stane této aplikace.

1.  Konfigurace aplikace Ukázka použití adresy URL služby a rozhraní api klíče. 

    Všimněte si schéma, která je definovaná v souboru Program.cs CatalogIndexer projektu. Určuje facetable pole Barva listPrice, velikost, weight (váha), categoryName a modelName.  Jenom některých tyto (barvy listPrice, categoryName) jsou ve skutečnosti implementovaná v působí navigaci.

3.  Spusťte aplikaci. 

    Nejdřív se zobrazuje jenom do vyhledávacího pole. Klikněte na tlačítko Hledat hned zobrazíte všechny výsledky nebo zadejte hledaný termín.

    ![][7]
 
4.  Zadejte hledaný termín, třeba bike a klikněte na **Hledat**. Dotaz spustí rychle.
 
    Působí navigační struktury je také vrácena s výsledky hledání.  Do pole Adresa URL jsou null fasetami barvy, kategorie a ceny. Na stránce výsledků vyhledávání obsahuje působí navigační struktury počty pro každý podmínka výsledek.

     ![][8]
 
5.  Klikněte na barvu, kategorie a cena oblast. Fasetami jsou null na počáteční vyhledávání, ale přijmou hodnot, jsou z položek, které již zadanému odstřihnuté výsledky hledání. Všimněte si, že identifikátor URI vyzvedne změny do dotazu.

    ![][9]
 
6.  Vymazat působí dotazu tak, aby si můžete vyzkoušet chování dotazu, klikněte na **Katalog AdventureWorks** v horní části stránky.

    ![][10]
 
<a name="nextstep"></a>
##<a name="next-step"></a>Další krok

Chcete-li otestovat své znalosti, můžete přidat pole Podmínka pro *modelName*. Index je nastaven pro tento podmínka, žádné změny do indexu jsou potřeba. Ale budete muset upravit kód HTML a zahrnují nové podmínka pro modely, přidejte pole omezující vlastnost konstruktoru dotazu.

Další zajímavosti návrh zásady pro působí navigace doporučujeme následující odkazy:

- [Návrhy působí hledání](http://www.uie.com/articles/faceted_search/)
- [Provedeních: Navigace působí](http://alistapart.com/article/design-patterns-faceted-navigation)

Můžete se taky podívat [Azure hledání hloubkové postupy](http://channel9.msdn.com/Events/TechEd/Europe/2014/DBI-B410). Na 45:25 je na ukázku o tom, jak implementovat fasetami.

<!--Anchors-->
[How to build it]: #howtobuildit
[Build the presentation layer]: #presentationlayer
[Build the index]: #buildindex
[Check for data quality]: #checkdata
[Build the query]: #buildquery
[Tips on how to control faceted navigation]: #tips
[Faceted navigation based on range values]: #rangefacets
[Faceted navigation based on GeoPoints]: #geofacets
[Try it out]: #tryitout

<!--Image references-->
[1]: ./media/search-faceted-navigation/Facet-1-slide.PNG
[2]: ./media/search-faceted-navigation/Facet-2-CSHTML.PNG
[3]: ./media/search-faceted-navigation/Facet-3-schema.PNG
[4]: ./media/search-faceted-navigation/Facet-4-SearchMethod.PNG
[5]: ./media/search-faceted-navigation/Facet-5-Prices.PNG
[6]: ./media/search-faceted-navigation/Facet-6-buildfilter.PNG
[7]: ./media/search-faceted-navigation/Facet-7-appstart.png
[8]: ./media/search-faceted-navigation/Facet-8-appbike.png
[9]: ./media/search-faceted-navigation/Facet-9-appbikefaceted.png
[10]: ./media/search-faceted-navigation/Facet-10-appTitle.png

<!--Link references-->
[Designing for Faceted Search]: http://www.uie.com/articles/faceted_search/
[Design Patterns: Faceted Navigation]: http://alistapart.com/article/design-patterns-faceted-navigation
[Create your first application]: search-create-first-solution.md
[OData expression syntax (Azure Search)]: http://msdn.microsoft.com/library/azure/dn798921.aspx
[Azure Search Adventure Works Demo]: https://azuresearchadventureworksdemo.codeplex.com/
[http://www.odata.org/documentation/odata-version-2-0/overview/]: http://www.odata.org/documentation/odata-version-2-0/overview/ 
[Faceting on Azure Search forum post]: ../faceting-on-azure-search.md?forum=azuresearch
[Search Documents (Azure Search API)]: http://msdn.microsoft.com/library/azure/dn798927.aspx

 