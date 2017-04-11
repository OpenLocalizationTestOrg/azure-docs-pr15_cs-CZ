<properties 
    pageTitle="Vytváření logiky aplikace definice | Microsoft Azure" 
    description="Zjistěte, jak psát definici JSON pro použití logických operátorů aplikace" 
    authors="jeffhollan" 
    manager="erikre" 
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
    
# <a name="author-logic-app-definitions"></a>Definice logiky aplikace Autor
Toto téma ukazuje, jak používat definice [Azure logiky aplikace](app-service-logic-what-are-logic-apps.md) , které je jednoduchý deklarativní JSON jazyk. Pokud jste tak dosud neučinili ještě, podívejte se na [Postup vytvoření nové aplikace logiky](app-service-logic-create-a-logic-app.md) nejdřív. Můžete taky přečíst [celou referenční materiály jazyka definice na webu MSDN](http://aka.ms/logicappsdocs).

## <a name="several-steps-that-repeat-over-a-list"></a>Několik kroků, které se opakují seznam

Můžete využít [foreach typ](app-service-logic-loops-and-scopes.md) opakování přes celou řadu maximálně 10 kB položek a provedení akce pro jednotlivá pole.

## <a name="a-failure-handling-step-if-something-goes-wrong"></a>Chyba při zpracování kroku Pokud dojde k chybě

Běžně mají mít možnost psát *remediation krok* – některé použití logických operátorů spustitelná, pokud **a jenom v případě**, jeden nebo více volání se nezdařila. V tomto příkladu jsme získávání dat z různých míst, ale pokud se nezdaří volání chci VYSTAVENÍ zprávy jinam, postupujte tak, aby se můžete vysledovat poruchy později:  

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            }
        },
        "postToErrorMessageQueue": {
            "type": "ApiConnection",
            "inputs": "...",
            "runAfter": {
                "readData": ["Failed"]
            }
        }
    },
    "outputs": {}
}
```

Může být použití `runAfter` vlastnost k určení `postToErrorMessageQueue` by měla běžet jenom `readData` je **Vadný**.  Také bude seznam možných hodnot, takže `runAfter` může být `["Succeeded", "Failed"]`.

Nakonec protože teď zpracování chyby jsme už označit spustit **Neúspěšný**. Jak můžete vidět tady, spuštěného se **byl úspěšný** i když jeden krok vadný, protože napsali krok zpracovávání tato chyba.

## <a name="two-or-more-steps-that-execute-in-parallel"></a>(Nejméně dvě) kroky, které paralelně provádět

Mít víc spuštění akce souběžně `runAfter` , musí být vlastnost odpovídající za běhu. 

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            }
        },
        "branch1": {
            "type": "Http",
            "inputs": "...",
            "runAfter": {
                "readData": ["Succeeded"]
            }
        },
        "branch2": {
            "type": "Http",
            "inputs": "...",
            "runAfter": {
                "readData": ["Succeeded"]
            }
        }
    },
    "outputs": {}
}
```

Jak vidíte v příkladu výše, obě `branch1` a `branch2` fungují `readData`. V důsledku toho obě větví spustí souběžně:

![Paralelní](./media/app-service-logic-author-definitions/parallel.png)

Uvidíte, že časové razítko pro obě pobočky je stejné. 

## <a name="join-two-parallel-branches"></a>Spojení dvou paralelní větve

Spojení dvou akce, které byly nastavené paralelně provádět přidáním položky `runAfter` vlastnost podobně jako výše.

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-04-01-preview/workflowdefinition.json#",
    "actions": {
        "readData": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {},
            "type": "Http"
        },
        "branch1": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {
                "readData": [
                    "Succeeded"
                ]
            },
            "type": "Http"
        },
        "branch2": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {
                "readData": [
                    "Succeeded"
                ]
            },
            "type": "Http"
        },
        "join": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {
                "branch1": [
                    "Succeeded"
                ],
                "branch2": [
                    "Succeeded"
                ]
            },
            "type": "Http"
        }
    },
    "contentVersion": "1.0.0.0",
    "outputs": {},
    "parameters": {},
    "triggers": {
        "manual": {
            "inputs": {
                "schema": {}
            },
            "kind": "Http",
            "type": "Request"
        }
    }
}
```

![Paralelní](./media/app-service-logic-author-definitions/join.png)

## <a name="mapping-items-in-a-list-to-some-different-configuration"></a>Mapování položek v seznamu na několik různých konfigurace

Další Řekněme, že nám chcete získat úplně různý obsah v závislosti na hodnotu vlastnosti. Jako parametr můžeme vytvořit mapy hodnot do míst:  

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "specialCategories": {
            "defaultValue": ["science", "google", "microsoft", "robots", "NSA"],
            "type": "Array"
        },
        "destinationMap": {
            "defaultValue": {
                "science": "http://www.nasa.gov",
                "microsoft": "https://www.microsoft.com/en-us/default.aspx",
                "google": "https://www.google.com",
                "robots": "https://en.wikipedia.org/wiki/Robot",
                "NSA": "https://www.nsa.gov/"
            },
            "type": "Object"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "getArticles": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "https://ajax.googleapis.com/ajax/services/feed/load?v=1.0&q=http://feeds.wired.com/wired/index"
            },
            "conditions": []
        },
        "getSpecialPage": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "@parameters('destinationMap')[first(intersection(item().categories, parameters('specialCategories')))]"
            },
            "conditions": [{
                "expression": "@greater(length(intersection(item().categories, parameters('specialCategories'))), 0)"
            }],
            "forEach": "@body('getArticles').responseData.feed.entries"
        }
    }
}
```

V tomto případě jsme nejprve získat seznam článků, a potom druhým krokem vyhledá v mapě založené na kategorii, která byla definována jako parametr, která adresa URL získat obsah z. 

Dvě položky věnujte pozornost tady: [`intersection()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#intersection) funkce se používá k zkusit, když kategorie, který odpovídá některému kategorii známých definované. Za druhé po jsme získáte kategorii jsme můžete si ho naimportovat položku mapě pomocí hranatých závorkách: `parameters[...]`. 

## <a name="working-with-strings"></a>Práce s řetězci

Existují různé funkce, které lze použít k manipulaci s řetězci. Podívejme se příklad kde máme řetězcem, který chcete předat systému, ale nemůžeme nejste jisti, že kódování znaků, které se bude přistupovat správně. Jednou z možností je ve formátu Base 64 kódování tohoto řetězce. Ale chcete-li předejít úniku v adrese URL budeme nahradit několik znaků. 

Chceme podřetězec z objednávky pojmenovat, protože nejsou použity nejprve 5 znaků.

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "order": {
            "defaultValue": {
                "quantity": 10,
                "id": "myorder1",
                "orderer": "NAME=Stèphén__Šīçiłianö"
            },
            "type": "Object"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "order": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://www.example.com/?id=@{replace(replace(base64(substring(parameters('order').orderer,5,sub(length(parameters('order').orderer), 5) )),'+','-') ,'/' ,'_' )}"
            }
        }
    },
    "outputs": {}
}
```

Práce z inside out:

1. Získání [`length()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#length) orderer názvu, tento příkaz vrátí zpět celkový počet znaků

2. Odečtení 5 (protože jsme budete chtít kratší řetězec)

3. Převzetí [`substring()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#substring) . Začneme v indexu `5` a přejděte zbytek řetězec.

4. Převod tohoto podřetězec k [`base64()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#base64) řetězec

5. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)všechny `+` znaků s`-`

6. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)všechny `/` znaků s`_`

## <a name="working-with-date-times"></a>Práce s data a času

Data a času může být užitečné, zejména pokud se pokoušíte načítat data ze zdroje dat, které nejsou podporovány přirozeným **aktivačních událostí**.  Chcete-li zjistit, jak dlouho různé kroky prováděnou můžete data a času. 

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "order": {
            "defaultValue": {
                "quantity": 10,
                "id": "myorder1"
            },
            "type": "Object"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "order": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://www.example.com/?id=@{parameters('order').id}"
            }
        },
        "timingWarning": {
            "actions" {
                "type": "Http",
                "inputs": {
                    "method": "GET",
                    "uri": "http://www.example.com/?recordLongOrderTime=@{parameters('order').id}&currentTime=@{utcNow('r')}"
                },
                "runAfter": {}
            }
            "expression": "@less(actions('order').startTime,addseconds(utcNow(),-1))"
        }
    },
    "outputs": {}
}
```

V tomto příkladu jsme extrahování `startTime` z předchozího kroku. Potom jsme zobrazuje aktuální čas nebo odečítání jedné sekundy:[`addseconds(..., -1)`](https://msdn.microsoft.com/library/azure/mt643789.aspx#addseconds) (můžete použít jiné časových hodnot, jako `minutes` nebo `hours`). Nakonec jsme můžete porovnat tyto dvě hodnoty. Je-li první menší než druhý a pak to znamená více než jedna sekunda uplynul od pořadí byla první umístit. 

Všimněte si také, že můžete používáme řetězec formatters formátování kalendářních dat: v řetězci dotazu se používá [`utcnow('r')`](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow) získat RFC1123. Všechna data, formátování [uvedených na webu MSDN](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow). 

## <a name="using-deployment-time-parameters-for-different-environments"></a>Použití parametrů nasazení času pro různých prostředích

Je běžné mít životního cyklu nasazení, kde máte vývojové prostředí testovacím prostředí a potom provozním prostředí. Ve všech těchto může má stejné definice, ale použít jinou databází, například. Můžete podobně používat stejnou definici napříč mnoho různých oblastí pro dostupnost, ale budete chtít pokaždé logiky aplikace chcete mluvit dané oblasti databáze. 

Všimněte si, že se jedná o liší od používáním vám umožní absolvování jinými parametry za *běhu*, že byste měli použít `trigger()` pracovat Schvalte dokument nad. 

Můžete začít s definicí velmi zneužívající vlastností prohlížeče zpráva:

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "uri": {
            "type": "string"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "@parameters('uri')"
            }
        }
    },
    "outputs": {}
}
```

Potom můžete v skutečné `PUT` žádosti o aplikaci logiky poskytnete parametr `uri`. Všimněte si, co je už výchozí hodnoty, které tento parametr je potřeba v aplikaci datové použití logických operátorů:

```
{
    "properties": {},
        "definition": {
          // Use the definition from above here
        },
        "parameters": {
            "connection": {
                "value": "https://my.connection.that.is.per.enviornment"
            }
        }
    },
    "location": "westus"
}
``` 

V každém prostředí potom můžete zadat jinou hodnotu pro `connection` parametr. 

Najdete v [dokumentaci rozhraní REST API](https://msdn.microsoft.com/library/azure/mt643787.aspx) pro všechny možnosti, které máte pro vytváření a Správa aplikací logiku. 
