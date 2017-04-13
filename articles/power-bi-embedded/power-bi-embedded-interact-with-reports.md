<properties
   pageTitle="Práce s těmito sestavami pomocí rozhraní API JavaScript | Microsoft Azure"
   description="Power BI vložený, interakci se sestavami pomocí rozhraní API JavaScript"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="interact-with-power-bi-reports-using-the-javascript-api"></a>Práce s těmito sestavami Power BI pomocí rozhraní API JavaScript

Rozhraní API JavaScript Power BI umožňuje snadno vložit do aplikace sestavách Power BI. Rozhraní API umožňuje aplikace programově práci s jinou zprávu prvků, jako jsou stránky a filtry. Tento interaktivity zajišťuje sestavách Power BI integrovanější část aplikace.

Vložení sestavy Power BI v aplikaci s použitím iframe, který je hostovaný jako součást aplikace. Do rámce iframe funguje jako hranici mezi aplikace a sestavy, jak vidíte na následujícím obrázku. 

![Power BI vložené iframe bez rozhraní API Javascript](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-1.png)

Do rámce iframe usnadňuje vložení obrázku výrazně, ale bez rozhraní API JavaScript nelze vzájemně spolupracovat na sestavu a aplikace. Tento chybějící interakce může být pocit jako skutečně sestavy není součástí aplikace. Sestavy a aplikaci skutečně potřeba komunikovat a zpátky, jako na následujícím obrázku.

![Power BI vložený iframe pomocí rozhraní API Javascript](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-2.png)

Rozhraní API JavaScript Power BI umožňuje zadat kód, který můžete bezpečně průchodu hranici iframe. Díky aplikaci programově prováděl určitou akci v sestavě a poslouchat události z akcí, díky kterým uživatelům v rámci sestavy.

## <a name="what-can-you-do-with-the-power-bi-javascript-api"></a>Co se dá dělat s rozhraní API JavaScript Power BI?
Rozhraní API JavaScript můžete spravovat sestavy, přejděte na stránky v sestavě, filtru sestavy a zpracování vložení události. Na následujícím obrázku vidíte struktury rozhraní API.

![Power BI JavaScript API diagramu](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-3.png)


### <a name="manage-reports"></a>Správa sestav
Rozhraní API Javascript umožňuje řízení chování na úrovni sestavy a stránky:

- Konkrétní sestavu Power BI pro bezpečné vložení v aplikaci – zkuste [vložit ukázku aplikace](http://azure-samples.github.io/powerbi-angular-client/#/scenario1)
  - Nastavení přístupový token
- Konfigurace sestavy
  - Povolení a zakázání podokno filtrů a podokno navigace na stránce: Zkuste [aktualizovat nastavení ukázku aplikace](http://azure-samples.github.io/powerbi-angular-client/#/scenario6)
  - Nastavení výchozích možností pro stránky a filtry – zkuste [nastavit výchozí hodnoty ukázku](http://azure-samples.github.io/powerbi-angular-client/#/scenario5)
- Zadejte a ukončit režim na celou obrazovku

[Další informace o vkládání sestavy](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embedding-Basics)


### <a name="navigate-to-pages-in-a-report"></a>Přejděte na stránky v sestavě
Rozhraní API JavaScript enbales objevit všechny stránky v sestavě a nastavit aktuální stránku. Vyzkoušejte [aplikaci ukázku navigace](http://azure-samples.github.io/powerbi-angular-client/#/scenario3).

[Další informace o navigaci na stránce](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Page-Navigation)

### <a name="filter-a-report"></a>Filtrování sestavy
Rozhraní API JavaScript poskytuje základní a rozšířené filtrování funkce vložený sestav a stránek sestavy. Zkuste [filtrování ukázku aplikace](http://azure-samples.github.io/powerbi-angular-client/#/scenario4)a zkontrolujte některé úvodní kód.  


#### <a name="basic-filters"></a>Základní filtry
Základní filtrování je umístěn na úrovni sloupce nebo hierarchie a seznamem hodnot a ty pak zahrnout nebo vyloučit.

```
const basicFilter: pbi.models.IBasicFilter = {
  $schema: "http://powerbi.com/product/schema#basic",
  target: {
    table: "Store",
    column: "Count"
  },
  operator: "In",
  values: [1,2,3,4]
}
```


#### <a name="advanced-filters"></a>Pokročilé filtry
Pokročilé filtry použít s logickým operátorem nebo nebo a přijměte podmínky jednu nebo dvě, oba objekty mají svoje vlastní operátor a hodnotu. Jsou podporované podmínky:

- Žádná
- LessThan
- LessThanOrEqual
- GreaterThan
- GreaterThanOrEqual
- Obsahuje
- DoesNotContain
- StartsWith
- DoesNotStartWith
- Je
- IsNot
- Je.prázdné
- IsNotBlank

```
const advancedFilter: pbi.models.IAdvancedFilter = {
  $schema: "http://powerbi.com/product/schema#advanced",
  target: {
    table: "Store",
    column: "Name"
  },
  logicalOperator: "Or",
  conditions: [
    {
      operator: "Contains",
      value: "Wash"
    },
    {
      operator: "Contains",
      value: "Park"
    }
  ]
}
```
[Další informace o filtrování](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Filters)


### <a name="handling-events"></a>Zpracování událostí
Kromě odesílání údajů do iframe aplikace také zobrazit informace o pocházejících z iframe následujících událostí:

- Vložení
  - načtení
  - Chyba
- Sestavy
  - pageChanged
  - dataSelected (už brzo)

[Další informace o zpracování událostí](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Handling-Events)


## <a name="next-steps"></a>Další kroky
Další informace o rozhraní API aplikace Power BI JavaScript podívejte se na následující odkazy:

- [Rozhraní API wikiwebu JavaScript](https://github.com/Microsoft/PowerBI-JavaScript/wiki)
- [Přehled modelu objektu](https://microsoft.github.io/powerbi-models/modules/_models_.html)
- Vzorky
  - [Úhlu](http://azure-samples.github.io/powerbi-angular-client)
  - [Členskými](https://github.com/Microsoft/powerbi-ember)
- [Živé ukázky](https://microsoft.github.io/PowerBI-JavaScript/demo/)
