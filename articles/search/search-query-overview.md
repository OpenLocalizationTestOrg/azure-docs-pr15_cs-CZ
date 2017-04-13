<properties
    pageTitle="Dotaz Azure vyhledávacím odkazu | Microsoft Azure | Vyhledávací služby serveru hostovanou cloudu"
    description="Vytvoření vyhledávacího dotazu v Azure hledání a použití parametrů hledání k filtrování a řazení výsledků hledání."
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

# <a name="query-your-azure-search-index"></a>Dotaz Azure vyhledávacím odkazu
> [AZURE.SELECTOR]
- [Základní informace](search-query-overview.md)
- [Portál](search-explorer.md)
- [.NET](search-query-dotnet.md)
- [ZBÝVAJÍCÍ](search-query-rest-api.md)

Po odeslání žádosti o hledání hledání Azure, existuje celá řada parametrů určených vedle skutečné slova zadaná do pole Hledat aplikace. Tyto parametry dotazu umožní dosáhnout některé hlubší kontroly fulltextové vyhledávání.

Tady je seznam, který stručně popisuje společné použití parametry dotazu v Azure vyhledávání. Úplné pokrytí parametry dotazu a jejich chování najdete v článku podrobné stránky pro [Rozhraní REST API](https://msdn.microsoft.com/library/azure/dn798927.aspx) a [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.searchparameters_properties.aspx).

## <a name="types-of-queries"></a>Typy dotazů

Azure hledání nabízí mnoho možností vytváření velmi výkonné dotazů. Jsou dva hlavní typy dotazů použijete `search` a `filter`. A `search` dotazu vyhledá jeden nebo více podmínek ve všech vrácených polích _vyhledávání_ v indexu a funguje tak, jak byste čekali vyhledávacího webu jako Google nebo Bing práce. A `filter` dotaz vyhodnotí logický výraz přes všechno _filtrovatelné_ polí v indexu. Na rozdíl od `search` dotazy `filter` dotazy podle přesné obsah pole, která znamená, že se rozlišují velká a řetězec polí.

Hledání a filtry můžete použít společně nebo samostatně. Pokud můžete použít společně, nejdřív použije filtr na celý index a potom na výsledky filtrování probíhá vyhledávání. Filtry proto může být užitečné postup pro zvýšení výkonu dotazu od zmenšit sadu dokumentů, které vyhledávacího dotazu je potřeba zpracovat.

Syntaxe pro výrazy filtru tvoří podmnožinu [OData filtr jazyk](https://msdn.microsoft.com/library/azure/dn798921.aspx). U vyhledávacích dotazů můžete [zjednodušené syntaxe](https://msdn.microsoft.com/library/azure/dn798920.aspx) nebo [Syntaxe dotazu Lucene](https://msdn.microsoft.com/library/azure/mt589323.aspx) , což jsou uvedeny níže.

### <a name="simple-query-syntax"></a>Syntaxe jednoduchého dotazu
[Syntaxe jednoduchého dotazu](https://msdn.microsoft.com/library/azure/dn798920.aspx) je výchozí jazyk dotaz používaný v Azure vyhledávání. Syntaxe jednoduchého dotazu podporuje počet běžné operátory hledání včetně a, nebo Ne, slovní spojení, přípony a priorita operátorů.

### <a name="lucene-query-syntax"></a>Syntaxe dotazu Lucene
[Syntaxe dotazu Lucene](https://msdn.microsoft.com/library/azure/mt589323.aspx) umožňuje široce přijmout a výrazové dotazovací jazyk vyvinuté jako součást [Apache Lucene](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html).

Pomocí následující syntaxe dotazu umožňuje snadno dosáhnout následující možnosti: [ [rozsah pole dotazů](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_fields), [rozmazaný vyhledávání](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_fuzzy), [blízkost hledání](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_proximity), termínů zvýšit](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_termboost), [hledání regulárních výrazů](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_regex), [hledání pomocí zástupných znaků](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_wildcard), [základní informace o syntaxi](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_syntax)a [dotazy použití logických operátorů](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_boolean).



## <a name="ordering-results"></a>Řazení výsledků
Při příjmu výsledků hledání dotazu, můžete požádat o hledání Azure slouží výsledky seřazené podle hodnot v konkrétním poli. Ve výchozím nastavení objednávek Azure vyhledávání podle pořadí skóre hledání každý dokument, které je odvozeno z [TF IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf)výsledků hledání.

Pokud chcete hledání Azure se výsledky seřazené podle hodnotu než skóre hledání můžete použít `orderby` parametr hledání. Můžete zadat hodnoty `orderby` parametr zahrnout názvy polí a volání [ `geo.distance()` funkce](https://msdn.microsoft.com/library/azure/dn798921.aspx) geografická hodnot. Každý výraz můžete následovat `asc` označíte, že výsledky jsou požadovány ve vzestupném pořadí a `desc` označíte, že jsou v sestupném pořadí požadované výsledky. Výchozí pořadí vzestupně.

## <a name="paging"></a>Stránkovací
Azure hledání usnadňuje implementaci stránkování výsledků hledání. Pomocí `top` a `skip` parametry, můžete hladce vydat hledání požadavky, které umožňují nedostáváte celkové sadu výsledků hledání v samostatně zvládnutelné, uspořádaných dílčí jednoduše povolit dobré hledání uživatelského rozhraní postupy. Při příjmu tyto menší podmnožiny výsledků, můžete taky dostanete počet dokumentů v sadě celkové výsledků hledání.

Je další informace o stránkování výsledků hledání v článku [jak stránky výsledků hledání Azure hledání](search-pagination-page-layout.md).


## <a name="hit-highlighting"></a>Přístupů zvýraznění
Do pole hledání Azure zvýraznění části přesné výsledky hledání, které odpovídají vyhledávacího dotazu je snadné pomocí `highlight`, `highlightPreTag`, a `highlightPostTag` parametry. Můžete určit _vyhledávání_ polí, která by měla i určující visačky přesné řetězce, které se připojit k zahájení a ukončení odpovídající text hledání Azure vrátí zvýrazněny odpovídající text.
