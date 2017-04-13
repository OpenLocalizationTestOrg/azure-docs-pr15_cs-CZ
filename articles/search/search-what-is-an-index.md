<properties
    pageTitle="Vytvoření indexu vyhledávání Azure | Microsoft Azure | Vyhledávací služby serveru hostovanou cloudu"
    description="Co je indexu v Azure hledání a jak se používá?"
    services="search"
    manager="jhubbard"
    documentationCenter=""
    authors="ashmaka"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="create-an-azure-search-index"></a>Vytvoření indexu vyhledávání Azure
> [AZURE.SELECTOR]
- [Základní informace](search-what-is-an-index.md)
- [Portál](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [ZBÝVAJÍCÍ](search-create-index-rest-api.md)

## <a name="what-is-an-index"></a>Co je rejstřík?

*Index* je trvalý úložiště *dokumentů* a jiných konstrukce použita službou Azure vyhledávání. Dokument je jednu jednotku vyhledávání dat ve odkazu. Například obchodování obchodě pravděpodobně dokumentu pro každou položku, kterou budou prodej, diskusní organizace může mít dokumentu pro každý článek atd. Mapování koncepce intuitivnější ekvivalenty databáze: *index* podobá entity *tabulky*a *dokumenty* odpovídají zhruba *řádků* v tabulce.

Po přidání/nahrát dokumenty a odesílání vyhledávacích dotazů Azure hledání, odešlete vašich požadavků na konkrétní index v vyhledávací služby.

## <a name="field-types-and-attributes-in-an-azure-search-index"></a>Typy polí a atributy v indexu vyhledávání Azure

Při definování schématu zadejte název, typ a atributy každého pole ve odkazu. Typ pole klasifikuje daty uloženými v tomto poli. Atributy jsou nastaveny na jednotlivá pole můžete určit, jak se používá pole. V následujících tabulkách umožňuje zobrazit výčet typů a atributy, které můžete použít.


### <a name="field-types"></a>Typy polí
|Typ|Popis|
|------------|-----------|
|*Edm.String*|Text, můžete volitelně tokenized mají být vyhledávány fulltextové (dělení slov, vyplývající a další).|
|*Collection(EDM.String)*|Seznam řetězce, které můžete volitelně tokenized mají být vyhledávány fulltextové. Neexistuje žádné teoretický horní omezení počtu položek v kolekci, ale horní mez 16 MB na velikost datové části platí pro kolekcí.|
|*Edm.Boolean*|Obsahuje hodnoty true nebo false.|
|*Edm.Int32*|32bitovou celočíselnou hodnoty.|
|*Edm.Int64*|64bitovou celočíselnou hodnoty.|
|*Edm.Double*|Číselná data s dvojitou přesností.|
|*Edm.DateTimeOffset*|Datum ve formátu protokolu OData verze 4 hodnoty času (například `yyyy-MM-ddTHH:mm:ss.fffZ` nebo `yyyy-MM-ddTHH:mm:ss.fff[+/-]HH:mm`).|
|*Edm.GeographyPoint*|Bod představuje zeměpisné polohy v celém světě.|

Můžete najít podrobnější informace o hledání Azure [podporované datové typy na webu MSDN](https://msdn.microsoft.com/library/azure/dn798938.aspx).



### <a name="field-attributes"></a>Atributy pole
|Atribut|Popis|
|------------|-----------|
|*Klíč*|Řetězec, který obsahuje jedinečné ID každý dokument pro vyhledejte dokument. Každý indexu musí mít vždycky jen jednu klávesu. Jenom jedno pole může být klávesu a jeho typ musí být nastavený na Edm.String.|
|*Zobrazitelné ve výsledcích vyhledávání*|Určuje, jestli pole může být vrácen ve výsledcích hledání.|
|*Filtrovat*|Umožňuje pole se nemusí používat v dotazů filtru.|
|*Řaditelnou*|Umožňuje dotaz: Pokud chcete řadit výsledky hledání pomocí tohoto pole.|
|*Facetable*|Umožňuje pole se nemusí používat v [působí navigační](search-faceted-navigation.md) struktury vlastním filtrování uživatele. Obvykle polí obsahujících opakujících hodnot, které můžete použít k seskupení několika dokumentů (například do několika dokumentů, které spadají jednu značku nebo kategorie služby) fungují nejlépe, jako fasetami.|
|*Vyhledávání*|Označí pole jako fulltextové vyhledávání.|

Můžete najít podrobnější informace o hledání Azure [atributy indexu na webu MSDN](https://msdn.microsoft.com/library/azure/dn798941.aspx).



## <a name="guidance-for-defining-an-index-schema"></a>Pokyny pro definování schématem indexu

Při návrhu index vyhraďte si dostatek času ve fázi plánování přemýšlet prostřednictvím rozhodnutí. Je důležité, aby byl vašich hledání uživatelské prostředí a obchodních potřeb myslet při návrhu index při každé pole, musí mít přiřazené [správné atributy](https://msdn.microsoft.com/library/azure/dn798941.aspx). Změna indexu po nasazení zahrnuje opětovné vytvoření a opětovné načítání dat.


Pokud požadavky na ukládání dat v průběhu času mění, můžete zvýšit nebo snížit kapacita přidáním nebo odebráním oddíly. Podrobnosti najdete v tématu [Správa vyhledávací služby v Azure](search-manage.md) nebo [Omezení služeb](search-limits-quotas-capacity.md).
