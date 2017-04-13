<properties
    pageTitle="Zvolte jinou SKU nebo ceny osy mají být vyhledávány Azure | Microsoft Azure"
    description="Azure hledání můžete zřízení v těchto skladové jednotky: bezplatnou základní a standardní, kde Standard je k dispozici v různých zdrojů konfigurace a kapacita úrovně."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="jhubbard"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="10/24/2016"
    ms.author="heidist"/>

# <a name="choose-a-sku-or-pricing-tier-for-azure-search"></a>Zvolte jinou SKU nebo ceny osy mají být vyhledávány Azure

Do pole hledání Azure [zřízení služby](search-create-service-portal.md) na konkrétní ceny osy nebo SKU. Možnosti zahrnují **Uvolnit**, **základní**nebo **Standardní**, kde **Standardní** je k dispozici ve více konfigurace a kapacity. 

Části tohoto článku účel snadněji zvolíte vrstva. Pokud osy kapacita změní je příliš malá, bude potřebujete zřízení nového službě na vyšší úroveň a pak znovu načíst indexů. Neexistuje žádný upgradu stejné služby z jednoho SKU do jiného. 

> [AZURE.NOTE] Po zvolení příkazu osy a [poskytování vyhledávací služby](search-create-service-portal.md), je třeba rozšířit otevřené a oddílu spočítá v rámci služby. Najdete v článku [úrovně zdroje měřítko pro dotaz a indexování úloh](search-capacity-planning.md).

## <a name="how-to-approach-a-pricing-tier-decision"></a>Jak řešit ceny rozhodnutí osy

Do pole hledání Azure Určuje úrovně kapacitu, ne dostupnost funkcí. Funkce obecně vzato jsou k dispozici u každé vrstvy, včetně funkce. Jedinou výjimkou je nepodporuje žádný indexování ve vysokém rozlišení S3.

> [AZURE.TIP] Doporučujeme vždy zřízení služby **Free** (jedno jedno předplatné bez vypršení platnosti) tak, aby jeho snadno umožňující lehká projekty. Použijte službu **Free** pro testování a hodnocení; Vytvoření druhého fakturaci služby ve vrstvě **základní** nebo **Standardní** pro výrobní nebo větší pracovního vytížení testu.

Funkce a náklady službou přejděte ruky v ruky. Informace v tomto článku můžete můžete rozhodnout, která SKU poskytuje správné rozložení, ale jiných mohlo být užitečné, potřebujete alespoň hrubé odhady na následujících zdrojích:

- Počet a velikost indexy, který budete chtít vytvořit
- Počet a velikost dokumenty nahrávat
- Některé věci objemu dotazu z hlediska dotazů za druhý (QPS)

Počet a velikost jsou důležité, protože maximálního limitu přes pevných omezení počtu indexy nebo dokumenty ve službě nebo zdrojů (úložiště nebo repliky) používaný službou. Skutečné limit službě je podle toho, které získá čerpány první: zdrojů nebo objekty.

Odhady ruku by podle těchto kroků zjednodušit proces:

- **Krok 1** Prohlédněte si zobrazíte další informace o dostupných možnostech SKU popisy.
- **Krok 2** Odpovězte na otázky k dosažení předběžné rozhodnutí dole.
- **Krok 3** Dokončení vaše rozhodnutí revizí pevné omezení úložiště a ceny.

## <a name="sku-descriptions"></a>Popisy SKU

Následující tabulka popisuje jednotlivé vrstvy. 

Vrstvy|Základní scénáře
----|-----------------
**Uvolnit**|Sdílených služeb zdarma, používá pro hodnocení, vyšetřování nebo malé úloh. Protože se sdílí další účastníky, výkonu dotazu a indexování se liší v závislosti na kdo další ještě používá služby. Kapacita je small (50 MB nebo 3 indexy s až 10 000 dokumenty každý).
**Základní**|Malé výrobní úloh vyhrazené hardwaru. Vysoce je k dispozici. Kapacita je až 3 repliky a 1 oddíl (2 GB).
**S1**|Standardní 1 podporuje flexibilní kombinací oddíly (12) a repliky (12) pro střední výrobní úloh vyhrazené hardwaru. Můžete přidělit oddíly a replikami v kombinace nepodporuje maximální počet jednotek, 36 fakturaci vyhledávání. Na této úrovni oddíly jsou 25 GB a QPS je asi 15 dotazů za sekundu.
**S2**|Větší výrobní úloh pomocí stejných jednotkách 36 hledání jako S1, ale s větší velikost oddíly a repliky spustí standardní 2. Na této úrovni oddíly jsou 100 GB a QPS je asi 60 dotazů za sekundu.
**S3**|Standardní 3 běží proporčně větší pracovního vytížení výrobní systémech ukončit vyšší konfigurace až 12 oddíly nebo 12 repliky ve skupinovém rámečku 36 hledání jednotky. Na této úrovni oddíly jsou 200 GB a QPS je více než 60 dotazů za sekundu. 
**S3 HD**|Standardní hustota vysoké 3 je určený pro velké množství menší indexy. 200 GB můžete mít až 3 oddíly. QPS je více než 60 dotazů za sekundu. 

> [AZURE.NOTE] Otevřené a oddíl maximální hodnoty jsou fakturované se jako hledání jednotky (36 konstanta maximální počet služby), která ukládá dolní mez efektivní než maximální hodnota znamená na papíru o nominální hodnotě. Například použít maximálně 12 repliky, může obsahovat maximálně 3 oddíly (12 * 3 = 36 jednotky). Podobně používat maximální oddíly, snižte repliky 3. Graf povolenou kombinace naleznete v tématu [úrovně zdroje měřítko pro dotaz a indexování úloh v Azure vyhledávání](search-capacity-planning.md) .

## <a name="review-limits-per-tier"></a>Revize limity za osy

V následující tabulce je podmnožinu limity omezení [služby Azure hledání](search-limits-quotas-capacity.md). Zobrazí se seznam největší pravděpodobností ovlivnit rozhodnutí SKU faktory. Můžete vytvořit odkaz tento graf při prohlížení na následující otázky.

Zdroje|Uvolnit|Základní|S1|S2|S3 |S3 HD
---|---|---|---|----|---|----
Smlouva o úrovni služeb (SLA)|Žádné <sup>1</sup> |Ano |Ano  |Ano |Ano  |Ano 
Limity indexu|3|5|50|200|200|1 000 <sup>2</sup>
Limity dokumentu|10 000 celkem|1 milion služby|15 milion oddíl |60 milion oddíl|120 milion oddíl |1 milion indexu
Maximální oddíly|NENÍ K DISPOZICI |1 |12  |12 |12|3 <sup>2</sup>
Velikost oddílu|Součet 50 MB|2 GB za služby|25 GB na oddíl |100 GB na oddíl (až do velikosti 1,2 TB za služby)|200 GB na oddíl (až do velikosti 2,4 TB za služby)|200 GB (maximální hodnota je 600 GB za služby)
Maximální repliky|NENÍ K DISPOZICI |3 |12 |12 |12|12
Dotazů za sekundu|NENÍ K DISPOZICI|~ 3 otevřené|~ 15 otevřené|~ 60 za otevřené|> 60 za otevřené|> 60 za otevřené

<sup>1</sup> zdarma a náhled SKU není součástí rozsahu. Rozsahu se nevynucují po jinou SKU všeobecně dostupná.

<sup>2</sup> S3 a S3 HD jsou podporovaným identickými velkokapacitní infrastruktury, ale každý z nich dosažení maximálního limitu různými způsoby. S3 zaměřuje menší počet obrovské indexy. Jako takové maximální limit je vázaný zdroje (2,4 TB pro každou službu). S3 HD zaměřuje velkého počtu příliš malý indexy. Na 1 000 indexy dosáhne S3 HD omezení ve formě omezení indexu. Pokud jste zákazníkem S3 HD, který vyžaduje víc než 1 000 indexů, kontaktujte Microsoft Support informace o tom, jak pokračovat.

## <a name="eliminate-skus-that-dont-meet-requirements"></a>Odstranění skladové jednotky, které nevyhovují požadavky 

Následující otázky vám mohou pomoci zahltit správné SKU rozhodnutí pro vaše pracovní zátěž.

1. Máte požadavky **Smlouva úrovni služeb (SLA)** ? Filtrování rozhodnutí SKU na standardní základní nebo bez náhledu.
2. **Kolik indexy** se vyžadují? Jednou z největších proměnné řešení do rozhodnutí SKU je počet indexů nepodporuje každý SKU. Podpora index je na výrazně různých úrovních v dolním cenách úrovní. Požadavky na počet indexů může být primární determinant SKU rozhodnutí.
3. **Kolik dokumentů** se načte do každého index? Počet a velikost dokumentů bude určovat případnou velikost index. Za předpokladu, že odhadu plánované velikost rejstříku, můžete Porovnejte toto číslo proti velikost oddílu za SKU prodloužilo o počet oddílů potřebných pro uložení index zadané velikosti. 
4. **Co je načítání očekávané dotazu**? Jakmile se rozumí požadavky na úložiště, zvažte pracovního vytížení dotazu. S2 i obě položky S3 nabízejí v blízkosti ekvivalent výkon, ale SLA požadavky bude vyloučit všechny náhledu skladové jednotky. 
5. Pokud jsou rozhodování úrovně S2 nebo S3, zjistěte, zda [indexování](search-indexer-overview.md). Indexování ještě nejsou k dispozici pro úroveň S3 HD. Alternativní přístup je pro účely modelu nabízených aktualizace rejstříku, kde se zadávají kód aplikace chcete použít k sadám dat pro index.

Většina zákazníků můžete pravidlo konkrétní SKU či oddálení podle jejich odpovědi na otázky výše. Pokud pořád nevíte, jaký SKU zvolit, můžete pokládat dotazy k MSDN nebo StackOverflow fóra nebo kontaktujte podporu Azure další pokyny.

## <a name="decision-validation-does-the-sku-offer-sufficient-storage-and-qps"></a>Rozhodnutí ověření: Skladovou nabízí dostatečné úložiště a QPS?

V posledním kroku návštěvě [ceny stránky](https://azure.microsoft.com/pricing/details/search/) a [oddíly za služby a za indexu v omezení služeb](search-limits-quotas-capacity.md) ověřte svými odhady omezení předplatné a přihlašovacích údajů. 

Pokud cena nebo úložiště požadavky mimo rozsah, můžete chtít Refaktorovat vytížení více menší služeb (například). Podrobnější úroveň může změnit návrh indexy menší nebo pomocí filtrů zefektivnit dotazů.

> [AZURE.NOTE] Požadavky na úložiště může být povolená zvýšeným, pokud dokumenty obsahovat nadbytečné data. V ideálním případě dokumenty obsahovat pouze vyhledávání dat nebo metadata. Binární data je není s možností vyhledávání a je třeba uložit odděleně (třeba v Azure tabulky nebo objektů blob úložiště) s polem v indexu pozastavení adresy URL odkazu na externí data. Maximální velikost jednotlivé dokument je 16 MB (nebo nižší když jste hromadné odeslání více dokumentů v jednu žádost o). Další informace najdete v tématu [limity služby Azure hledání](search-limits-quotas-capacity.md) .

## <a name="next-step"></a>Další krok

Když víte, což je SKU správné přizpůsobení, dál na pomocí následujících kroků:

- [Vytvoření vyhledávací služby na portálu](search-create-service-portal.md)
- [Změna přiřazení oddíly a repliky zobrazit služby](search-capacity-planning.md)

