<properties
    pageTitle="Vytvoření indexu vyhledávání Azure pomocí portálu Azure | Microsoft Azure | Vyhledávací služby serveru hostovanou cloudu"
    description="Vytvoření indexu pomocí portálu Azure."
    services="search"
    manager="jhubbard"
    authors="ashmaka"
    documentationCenter=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="create-an-azure-search-index-using-the-azure-portal"></a>Vytvoření indexu vyhledávání Azure pomocí portálu Azure
> [AZURE.SELECTOR]
- [Základní informace](search-what-is-an-index.md)
- [Portál](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [ZBÝVAJÍCÍ](search-create-index-rest-api.md)

Tento článek vás provede procesem vytváření Azure hledání [index](search-what-is-an-index.md) na portálu Azure.

Před sledováním tohoto průvodce a vytváření rejstříku, měli byste mít již [vytvořili Azure vyhledávací služby](search-create-service-portal.md).


## <a name="i-go-to-your-azure-search-blade"></a>Můžu. Přejděte na zásuvné Azure hledání
1. Klikněte na možnost "Všechny zdroje" v nabídce na levé straně aplikace [Portál Azure](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices)
2. Vyberte Azure vyhledávací služby

## <a name="ii-add-and-name-your-index"></a>II. Přidejte a pojmenujte index
1. Klikněte na tlačítko "Přidat index"
2. Pojmenujte Azure vyhledávacím odkazu. Protože jsme vytváření rejstříku vyhledat hotely v této příručce pojmenovali jsme ho mít náš index "hotely".
  * Název indexu musí začínat písmenem a musí obsahovat pouze malá písmena, číslice, pomlček nebo prázdných buněk ("-").
  * Podobně jako název služby, které jste vybrali název indexu bude také část adresy URL koncového bodu místo, kam chcete odeslat požadavků HTTP pro rozhraní API pro hledání Azure
3. Klikněte na položku "Pole" Chcete-li otevřít nový zásuvné

![](./media/search-create-index-portal/add-index.png)


## <a name="iii-create-and-define-the-fields-of-your-index"></a>III. Vytvoření a definujte pole index
1. Výběrem položky "Pole" nové zásuvné otevře se formulář pro zadávání definice indexu.
2. Přidání polí do indexu pomocí zobrazeného formuláře.

  * Pole *klíč* typu Edm.String je povinné pro každý indexu vyhledávání Azure. Toto pole klíče se vytvoří ve výchozím nastavení se pole název "identifikátor". Jsme změnili ho na "hotelId" v naší index.
  * Některé vlastnosti index schématu můžete nastavit jenom jednou a nelze aktualizovat v budoucnu. Proto všechny aktualizace schéma, které vyžadují přeindexování například změnit typy polí nejsou aktuálně možné po počátečním nastavení.
  * Pečlivě zvolili hodnot vlastností pro každé pole, podle jak jsme myslíte, že se použije v aplikaci. Při návrhu index při každé pole, musí mít přiřazené [odpovídající vlastnosti](https://msdn.microsoft.com/library/azure/dn798941.aspx), mějte na paměti vašich hledání uživatelské prostředí a obchodních potřeb. Platí pro pole, která těchto vlastností ovládacího prvku, který hledání funkcí (filtrování, faceting, řazení, fulltextové hledání atd.). Například je pravděpodobné, že vyhledáte hotely budou zajímat odpovídá klíčovému slovu na pole "Popis" tak jsme povolit fulltextové vyhledávání tohoto pole nastavením vlastnosti "Vhodné pro prohledávání".
    * [Analýza jazyk](https://msdn.microsoft.com/en-us/library/azure/dn879793.aspx) pro každé pole můžete nastavit taky tak, že kliknete na kartě "Analyzer" v horní části zásuvné. Zobrazí se pod, jsme zvolili francouzsky analyzer pro pole v indexu určené pro francouzsky text.

3. Klikněte na **OK** v zásuvné "Pole" potvrďte definice polí
4. Klikněte na **OK** v zásuvné "Přidat index" uložte a vytvořit index, který jste definovali.

V snímky obrazovek níže uvidíte, jak jsme s názvem a polí definovaných pro naše "hotely" index.

![](./media/search-create-index-portal/field-definitions.png)

![](./media/search-create-index-portal/set-analyzer.png)

## <a name="next"></a>Další
Po vytvoření indexu vyhledávání Azure, bude připravená k [odeslání obsahu do indexu](search-what-is-data-import.md) tak hledat data.
