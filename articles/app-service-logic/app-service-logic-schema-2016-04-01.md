<properties 
    pageTitle="Novou verzi schématu 2016 06 01 | Microsoft Azure" 
    description="Zjistěte, jak psát definici JSON v nejnovější verzi aplikace použití logických operátorů" 
    authors="jeffhollan" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="jehollan"/>
    
# <a name="new-schema-version-2016-06-01"></a>Novou verzi schématu 2016 06 01

Nové schéma a rozhraní API verze aplikace logiky má řadu vylepšení, která zlepšit spolehlivost a snadnější použití logických operátorů aplikací. Existují 3 důležité rozdíly:

1. Přidání oborů, které jsou akce, které obsahují sadu akcí.
1. Podmínky a smyčky se prvotřídní akce
1. Spuštění řazení podrobnější prostřednictvím `runAfter` vlastnost (které slouží k nahrazení `dependsOn`)

Informace o upgradu logiku aplikace z schématu 2015 08 01 náhled schématu 2016 06 01 [najdete v článku upgrade oddíl níž.](#upgrading-to-2016-06-01-schema)


## <a name="1-scopes"></a>1. obory

Jednou z největších změn ve toto schéma je přidání obory a možnost vnořit akce v rámci každé.  To je užitečné, když seskupování sady akcí, nebo když museli vnořovat akce v rámci každé (například podmínku může obsahovat další podmínku).  Další informace o syntaxi rozsahu může být nalezených [tady](app-service-logic-loops-and-scopes.md), ale příklad jednoduchého obor najdete níže:


```
{
    "actions": {
        "My_Scope": {
            "type": "scope",
            "actions": {                
                "Http": {
                    "inputs": {
                        "method": "GET",
                        "uri": "http://www.bing.com"
                    },
                    "runAfter": {},
                    "type": "Http"
                }
            }
        }
    }
}
```

## <a name="2-conditions-and-loops-changes"></a>2. podmínky a cykly změny

V předchozích verzích aplikace schématu podmínky a smyčky byly parametry související s jedinou akcí.  Toto omezení bylo zrušeno v toto schéma a teď podmínky a smyčky zobrazovaly jako typ akce.  Další informace najdete [v tomto článku](app-service-logic-loops-and-scopes.md)a jednoduchý příklad podmínka akce jsou uvedeny níže:

```
{
    "If_trigger_is_foo": {
        "type": "If",
        "expression": "@equals(triggerBody(), 'foo')",
        "runAfter": { },
        "actions": {
            "Http_2": {
                "inputs": {
                    "method": "GET",
                    "uri": "http://www.bing.com"
                },
                "runAfter": {},
                "type": "Http"
            }
        },
        "else": 
        {
            "if_trigger_is_bar": "..."
        }      
    }
}
```

## <a name="3-runafter-property"></a>3. vlastnost RunAfter

Nové `runAfter` vlastnost nahrazuje `dependsOn` pomůže povolit přesnější v spuštění řazení.  `dependsOn`je synonymem "akce spustili a bylo úspěšné," ale opakovaně pokoušeli potřebujete k spuštění akce, pokud je úspěšně předchozí akce, se nezdařila nebo vynechány.  `runAfter`Umožňuje tuto možnost.  Je objekt, který určuje všechny akce názvy, které se spustí po a definuje maticových stav účastníků jsou přijatelné na aktivační událost z.  Pro příklad Kdybyste chtěli po kroků bylo úspěšné A a B dostanete byl úspěšný nebo, by vytvářet následující `runAfter` vlastnosti:

```
{
    "...",
    "runAfter": {
        "A": ["Succeeded"],
        "B": ["Succeeded", "Failed"]
    }
}
```

## <a name="upgrading-to-2016-06-01-schema"></a>Upgrade na 2016 06 01 schématu

Upgrade na nové 2016 06 01 schéma, stačí jen několik kroků.  Podrobnosti o změně ze schématu najdete [v tomto článku](app-service-logic-schema-2016-04-01.md).  Proces upgradu obsahuje spuštěním skriptu upgradu, uložení jako novou aplikaci logiku a potenciálně přepisování starou aplikaci logiku v případě potřeby.

1. Otevřete aplikaci aktuální logiky.
1. Kliknutím na tlačítko **Aktualizovat schématu** v panelu nástrojů
   
    ![][1]
   
    Vrátí definici upgradovaný.  Může zkopírujte a vložte takto do definici zdrojů v případě potřeby, ale nemůžeme **důrazně doporučujeme** použít na tlačítko **Uložit jako** zajistit připojení pro všechny odkazy jsou platné v aplikaci použití logických operátorů upgradované.
1. Klikněte na tlačítko **Uložit jako** na panelu nástrojů upgradu zásuvné.
1. Vyplňte název a logiku stav aplikací a kliknutím na tlačítko **vytvořit** Nasaďte aplikaci upgradu logiku.
1. Ověření, že aplikace upgradovaném logiku funguje očekávaným způsobem.

    >[AZURE.NOTE] Pokud používáte ručně nebo žádosti o aktivační událost, adresu URL zpětné se změnily v novou aplikaci logiky.  Použití nová adresa URL k ověření funguje začátku do konce, a můžete vytvořit kopii nad existující aplikace logiky zachovat předchozí adresy URL.

1. *Volitelné* Přepsat předchozí aplikace logika s novou verzí schématu pomocí tlačítka **klonovat** na panelu nástrojů (vedle na ikonu **Aktualizace schématu** na obrázku nahoře).  Toto je nutné pouze v případě, že chcete zachovat stejnou číslo ID zdroje nebo žádost o aktivační událost URL logiky aplikace.

### <a name="upgrade-tool-notes"></a>Upgrade nástrojů poznámek

#### <a name="condition-mapping"></a>Podmínka mapování

Nástroj bude snažte zařadit do skupiny akce PRAVDA a NEPRAVDA větev pohromadě v oboru v upgradovaném definici.  Konkrétně návrháře vzorek `@equals(actions('a').status, 'Skipped')` má zobrazit jako `else` akce.  Ale pokud nástroj nalezne vzorků nerozpozná, že potenciálně vytvoříte samostatný podmínky hodnotu true a false pobočky.  Akce je možné znovu mapovaných zveřejňují upgrade v případě potřeby.

#### <a name="foreach-with-condition"></a>ForEach s podmínku
  
V nové schéma s akce filtru replikovat předchozí vzorek smyčku foreach s podmínkou podle předmětu.  Dojde automaticky při upgradu.  Podmínka přestane akce filtru před smyčka foreach (k vrácení pouze pole položek, které splňují podmínku) a dané pole je předaným foreach akce.  Můžete zobrazit příklad [v tomto článku](app-service-logic-loops-and-scopes.md)

#### <a name="resource-tags"></a>Značky zdroje

Značky zdroje se odstraní při upgradu a bude muset znovu nastavit upgradované pracovního postupu.

## <a name="other-changes"></a>Další změny

### <a name="manual-trigger-renamed-to-request-trigger"></a>Ruční spuštění na žádost o aktivační událost

Typ `manual` byly změněny a přejmenovat na `request` s typ z `http`.  Toto je další konzistentní s typ vzorku, použité k vytvoření aktivační událost.

### <a name="new-filter-action"></a>Novou akci "filtr.

Pokud pracujete s velké maticové a potřebujete filtrovat dolů na něco míň položek, můžete použít nový typ "filtr".  Přijme matice a podmínky a bude vyhodnocení podmínky pro všechny požadované položky a vrátí matici položky, které splňují podmínku.

### <a name="foreach-and-until-action-restrictions"></a>ForEach do omezení akce

Foreach a až smyčka jsou omezeny pouze jednu akci.

### <a name="trackedproperties-on-actions"></a>TrackedProperties na akce

Akce Teď máte další vlastnosti (na stejné úrovni chcete `runAfter` a `type`) s názvem `trackedProperties`.  Je objekt, který určuje určité akce vstupů nebo výstupy mají být součástí Azure diagnostiky telemetrie, která je součástí pracovního postupu údaj.  Příklad:

```
{                
    "Http": {
        "inputs": {
            "method": "GET",
            "uri": "http://www.bing.com"
        },
        "runAfter": {},
        "type": "Http",
        "trackedProperties": {
            "responseCode": "@action().outputs.statusCode",
            "uri": "@action().inputs.uri"
        }
    }
}
```

## <a name="next-steps"></a>Další kroky
- [Použití definici pracovního postupu aplikace logiky](app-service-logic-author-definitions.md)
- [Vytvoření šablony nasazení aplikace logiky](app-service-logic-create-deploy-template.md)


<!-- Image references -->
[1]: ./media/app-service-logic-schema-2016-04-01/upgradeButton.png
