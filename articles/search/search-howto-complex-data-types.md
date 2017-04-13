<properties
    pageTitle="Jak model složitými datovými typy Azure hledání | Hledání Microsoft Azure"
    description="Vnořené nebo hierarchické datové struktury můžete modelovat v indexu vyhledávání Azure pomocí sloučený řádků a datovým typem kolekcí."
    services="search"
    documentationCenter=""
    authors="LiamCa"
    manager="pablocas"
    editor=""
    tags="complex data types; compound data types; aggregate data types"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="09/07/2016"
    ms.author="liamca"
/>

# <a name="how-to-model-complex-data-types-in-azure-search"></a>Jak se složitými datovými typy modelu v Azure hledání

Externí datové sady použita k naplnění indexu vyhledávání Azure někdy zahrnout hierarchické nebo vnořený používání dílčích struktur, které nejsou rozdělí přehledně na tabulkovém řádků. Příklady těchto struktury může zahrnovat více míst a telefonních čísel pro jediného zákazníka, více barvy a velikosti pro jeden SKU několika dalšími autory jednoho knihy a tak dále. V modelování podmínky, může se zobrazit tyto struktury říká *složitými datovými typy* *složené datové typy*, *složených datových typů*a *Agregovat datové typy*, název několik.

Složitými datovými typy nejsou nativně podporované v Azure vyhledávání, ale ověřené řešení obsahuje krocích sloučení struktury a pomocí datový typ **kolekce** znovuvytvoření vnitřní strukturu. Následující postup popisované v tomto článku umožňuje obsah, který chcete prohledávat, působí, filtrování a řazení.

## <a name="example-of-a-complex-data-structure"></a>Příklad strukturu komplexních dat

Obvykle předmětné jsou uložená data jako sadu JSON nebo XML dokumentů nebo položek v úložišti NoSQL například DocumentDB. Tím největší oříškem hlediska vyplývá ušetřil práci několik podřízených položek, které je potřeba vyhledat a filtrovaná.  Jako výchozí bod pro znázornění řešení proveďte následující JSON dokument, který obsahuje sadu kontakty jako příklad:

~~~~~
[
  {
    "id": "1",
    "name": "John Smith",
    "company": "Adventureworks",
    "locations": [
      {
        "id": "1",
        "description": "Adventureworks Headquarters"
      },
      {
        "id": "2",
        "description": "Home Office"
      }
    ]
  }, 
  {
    "id": "2",
    "name": "Jen Campbell",
    "company": "Northwind",
    "locations": [
      {
        "id": "3",
        "description": "Northwind Headquarter"
      },
      {
        "id": "4",
        "description": "Home Office"
      }
    ]
}]
~~~~~

Během pole s názvem "identifikátor", "název" a "společnost" lze snadno namapovat přímé jako pole do indexu vyhledávání Azure, obsahuje pole "umístění" maticových umístění s obě množiny ID umístění a také umístění popisy. Předpokladu, že hledání Azure nemá datový typ, který podporuje, potřebujeme jiným způsobem model to v Azure vyhledávání. 

> [AZURE.NOTE] Tento postup je také podle tak, že zařízení Kirk Evans příspěvek na blogu [Indexování DocumentDB pomocí funkce hledání Azure](https://blogs.msdn.microsoft.com/kaevans/2015/03/09/indexing-documentdb-with-azure-seach/), který ukazuje techniku s názvem "sloučení dat", kterým chcete mít u pole s názvem `locationsID` a `locationsDescription` , které jsou oba [kolekce](https://msdn.microsoft.com/library/azure/dn798938.aspx) (nebo pole řetězců).   

## <a name="part-1-flatten-the-array-into-individual-fields"></a>Část 1: Sloučení pole do jednotlivých polí

Hledání Azure index, který bude vyhovovat této sadě dat, vytvořit jednotlivá pole vnořené podkladní: `locationsID` a `locationsDescription` s datovým typem [kolekce](https://msdn.microsoft.com/library/azure/dn798938.aspx) (nebo pole řetězců). V těchto polí by index hodnotu "1" a "2" do `locationsID` polí Jan a hodnoty "3" & "4" do `locationsID` pole pro společnosti Jen Campbell.  

Data v rámci Azure hledání by vypadal takto: 

![Ukázková data, 2 řádky](./media/search-howto-complex-data-types/sample-data.png)


## <a name="part-2-add-a-collection-field-in-the-index-definition"></a>Část 2: Přidání pole kolekce v definici indexu

Ve schématu index vypadat podobně jako tento příklad definice polí.

~~~~
var index = new Index()
{
    Name = indexName,
    Fields = new[]
    {
        new Field("id", DataType.String) { IsKey = true },
        new Field("name", DataType.String) { IsSearchable = true, IsFilterable = false, IsSortable = false, IsFacetable = false },
        new Field("company", DataType.String) { IsSearchable = true, IsFilterable = false, IsSortable = false, IsFacetable = false },
        new Field("locationsId", DataType.Collection(DataType.String)) { IsSearchable = true, IsFilterable = true, IsFacetable = true },
        new Field("locationsDescription", DataType.Collection(DataType.String)) { IsSearchable = true, IsFilterable = true, IsFacetable = true }
    }
};
~~~~

## <a name="validate-search-behaviors-and-optionally-extend-the-index"></a>Ověřte chování při hledání a volitelně rozšíření indexu

Za předpokladu, že jste vytvořili index a načíst data, můžete teď otestovat řešení pro ověření spuštění dotazu hledání podle datové sady. Každé **kolekce** pole by měl být **vyhledávání**, **filtrovat** a **facetable**. Je třeba moct spouštění dotazů jako:

* Vyhledání všech lidí, kteří pracují na Adventureworks Headquarters.
* Zjištění počtu počet lidí, které pracují ve Domů Office.  
* Lidí, kteří pracují na Domů Office zobrazí jaké další poboček fungují spolu s počtu lidí v každém umístění.  

Po sobě číselnou tento postup je, když je třeba udělat vyhledávání, který kombinuje id umístění jak popis polohy. Příklad:

* Vyhledání všech lidí místo, kam mají Domů Office a byla umístění ID 4.  

Pokud odvolání původní obsah vypadalo takto:

~~~~
   {
        id: '4',
        description: 'Home Office'
   }
~~~~

Ale teď můžeme mít rozdělené data do samostatných polích, máme žádným způsobem, že pokud Domů Office pro se týká Jen společnosti Campbell `locationsID 3` nebo `locationsID 4`.  

Zpracování tento případ, definujte jiné pole v index, který kombinuje všechna data do jedné kolekce.  Pro zpět k našemu příkladu jsme vyzvedne toto pole `locationsCombined` a jsme oddělí obsah s `||` však můžete zvolit oddělovač, který by podle vás bude jedinečnou sadu znaků pro obsah. Příklad: 

![Ukázková data, 2 řádky s oddělovače](./media/search-howto-complex-data-types/sample-data-2.png)

Pomocí `locationsCombined` pole, jsme teď vejdou ještě víc dotazů, například:

* Zobrazí počet uživatelů, kteří pracují na "Domů Office" umístění Id "4".  
* Vyhledat osoby, které pracují na Domů Office umístění Id "4". 

## <a name="limitations"></a>Omezení

Tento postup je užitečný pro počet scénáře, ale není vhodná v každém případě.  Příklad:

1. Pokud nemáte statické sadu polí v složité datový typ a došlo žádným způsobem namapujte možnými způsoby na jedno pole. 
2. Aktualizace vnořené objekty vyžaduje některé další práce k určení přesně co je třeba aktualizovat do indexu vyhledávání Azure

## <a name="sample-code"></a>Ukázkový kód

Viz příklad o tom, jak index složité JSON k sadám dat v Azure vyhledávání a konfigurovat celou řadu možností dotazů přes tento datovou sadu v tomto [GitHub repo](https://github.com/liamca/AzureSearchComplexTypes).

## <a name="next-step"></a>Další krok

[Hlasování nativní podpora složitými datovými typy](https://feedback.azure.com/forums/263029-azure-search) na UserVoice hledání Azure stránky a zadejte jakékoli další zadání, kterou chcete, abychom vám zvažte týkající se funkcí implementaci. Můžete taky nedostanete mi přímo na Twitter na @liamca.


 