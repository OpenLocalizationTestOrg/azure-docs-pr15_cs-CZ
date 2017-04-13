<properties
   pageTitle="Správce prostředků šablony funkce | Microsoft Azure"
   description="Popisuje funkce používat v šabloně aplikace Správce prostředků Azure k načtení hodnot, pracovat s řetězce a číslovky a získat informace o nasazení."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/12/2016"
   ms.author="tomfitz"/>

# <a name="azure-resource-manager-template-functions"></a>Funkce šablony Azure správce prostředků

Toto téma popisuje všechny funkce, které můžete použít v šabloně aplikace Správce prostředků Azure.

Funkce šablony a jejich parametry jsou malá a velká písmena. Správce prostředků například vyřeší **variables('var1')** a **VARIABLES('VAR1')** stejně. Při vyhodnocování, pokud funkci výslovně upraví velikost písmen (například toUpper nebo toLower), funkce zachovává malá a velká. Některé typy zdrojů mohou mít případu požadavky bez ohledu na to, jak jsou vyhodnoceny funkcí.

## <a name="numeric-functions"></a>Číselné funkce

Správce prostředků poskytuje následující funkce pro práci s celá čísla:

- [Přidání](#add)
- [copyIndex](#copyindex)
- [div](#div)
- [Funkce INT](#int)
- [MOD](#mod)
- [mul](#mul)
- [Sub](#sub)


<a id="add" />
### <a name="add"></a>Přidání

**Přidání (operand1, operand2)**

Vrátí součet dvou ujednaných celá čísla.

| Parametr                          | Povinné | Popis
| :--------------------------------: | :------: | :----------
| operand1                           |   Ano    | První celé číslo, pokud chcete přidat.
| operand2                           |   Ano    | Druhé číslo přidat.

Následující příklad přidává dvěma parametry.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "First integer to add"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Second integer to add"
        }
      }
    },
    ...
    "outputs": {
      "addResult": {
        "type": "int",
        "value": "[add(parameters('first'), parameters('second'))]"
      }
    }

<a id="copyindex" />
### <a name="copyindex"></a>copyIndex

**copyIndex(offset)**

Vrátí aktuální index smyčka iterace. 

| Parametr                          | Povinné | Popis
| :--------------------------------: | :------: | :----------
| Posun                           |   Ne    | Velikost přidáte aktuální hodnotu iterace.

Tato funkce je používat s objektem **Kopírovat** . Detailní popis používání **copyIndex**najdete v článku [vytvoření několika instancích zdrojů v Azure správce](resource-group-create-multiple.md).

Následující příklad ukazuje smyčku kopírovat a hodnota indexu součástí název. 

    "resources": [ 
      { 
        "name": "[concat('examplecopy-', copyIndex())]", 
        "type": "Microsoft.Web/sites", 
        "copy": { 
          "name": "websitescopy", 
          "count": "[parameters('count')]" 
        }, 
        ...
      }
    ]


<a id="div" />
### <a name="div"></a>div

**div (operand1, operand2)**

Vrátí rozdělení celé číslo dvě zadané celá čísla.

| Parametr                          | Povinné | Popis
| :--------------------------------: | :------: | :----------
| operand1                           |   Ano    | Celé číslo rozdělené.
| operand2                           |   Ano    | Celé číslo, které slouží k rozdělení. Nelze 0.

Následující příklad rozdělí jeden parametr parametrem jiného.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "Integer being divided"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Integer used to divide"
        }
      }
    },
    ...
    "outputs": {
      "divResult": {
        "type": "int",
        "value": "[div(parameters('first'), parameters('second'))]"
      }
    }

<a id="int" />
### <a name="int"></a>Funkce INT

**int(valueToConvert)**

Převede zadaná hodnota celé číslo.

| Parametr                          | Povinné | Popis
| :--------------------------------: | :------: | :----------
| valueToConvert                     |   Ano    | Hodnota převést na celé číslo. Typ hodnoty lze pouze řetězec nebo celé číslo.

Následující příklad převede hodnotu parametru uživatele-za předpokladu, že na celé číslo.

    "parameters": {
        "appId": { "type": "string" }
    },
    "variables": { 
        "intValue": "[int(parameters('appId'))]"
    }


<a id="mod" />
### <a name="mod"></a>MOD

**MOD (operand1, operand2)**

Vrátí zbytek celočíselné dělení pomocí dvou ujednaných celá čísla.

| Parametr                          | Povinné | Popis
| :--------------------------------: | :------: | :----------
| operand1                           |   Ano    | Celé číslo rozdělené.
| operand2                           |   Ano    | Celé číslo, které slouží k rozdělení, musí být liší od 0.

Následující příklad vrátí zbytek po dělení jeden parametr parametrem jiného.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "Integer being divided"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Integer used to divide"
        }
      }
    },
    ...
    "outputs": {
      "modResult": {
        "type": "int",
        "value": "[mod(parameters('first'), parameters('second'))]"
      }
    }

<a id="mul" />
### <a name="mul"></a>mul

**mul (operand1, operand2)**

Vrátí Násobení dvou ujednaných celá čísla.

| Parametr                          | Povinné | Popis
| :--------------------------------: | :------: | :----------
| operand1                           |   Ano    | První celé číslo k násobení.
| operand2                           |   Ano    | Druhé číslo k násobení.

Následující příklad vynásobí jeden parametr parametrem jiného.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "First integer to multiply"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Second integer to multiply"
        }
      }
    },
    ...
    "outputs": {
      "mulResult": {
        "type": "int",
        "value": "[mul(parameters('first'), parameters('second'))]"
      }
    }

<a id="sub" />
### <a name="sub"></a>Sub

**Sub (operand1, operand2)**

Vrátí odčítání dvě zadané celá čísla.

| Parametr                          | Povinné | Popis
| :--------------------------------: | :------: | :----------
| operand1                           |   Ano    | Celé číslo, které je hodnota odečtená od.
| operand2                           |   Ano    | Celé číslo, které je hodnota odečtená.

Následující příklad odečte jeden parametr z jiného parametru.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "Integer subtracted from"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Integer to subtract"
        }
      }
    },
    ...
    "outputs": {
      "subResult": {
        "type": "int",
        "value": "[sub(parameters('first'), parameters('second'))]"
      }
    }

## <a name="string-functions"></a>Funkce String

Správce prostředků poskytuje následující funkce pro práci s řetězce:

- [ve formátu Base 64](#base64)
- [propojit](#concat)
- [Délka](#lengthstring)
- [padLeft](#padleft)
- [nahrazení](#replace)
- [Přeskočit](#skipstring)
- [rozdělení](#split)
- [řetězec](#string)
- [podřetězec](#substring)
- [přijmout](#takestring)
- [toLower](#tolower)
- [toUpper](#toupper)
- [Střih](#trim)
- [uniqueString](#uniquestring)
- [identifikátor URI](#uri)


<a id="base64" />
### <a name="base64"></a>ve formátu Base 64

**ve formátu Base 64 (inputString)**

Vrátí ve formátu Base 64 reprezentaci vstupní řetězec.

| Parametr                          | Povinné | Popis
| :--------------------------------: | :------: | :----------
| inputString                        |   Ano    | Řetězec k vrácení jako reprezentaci ve formátu Base 64.

Následující příklad ukazuje, jak se používá funkce ve formátu Base 64.

    "variables": {
      "usernameAndPassword": "[concat('parameters('username'), ':', parameters('password'))]",
      "authorizationHeader": "[concat('Basic ', base64(variables('usernameAndPassword')))]"
    }

<a id="concat" />
### <a name="concat---string"></a>propojit - řetězec

**propojit (řetězec_1, řetězec_2, string3,...)**

Kombinuje více textových hodnot a vrátí zřetězený řetězec. 

| Parametr                          | Povinné | Popis
| :--------------------------------: | :------: | :----------
| řetězec_1                        |   Ano    | Hodnotu řetězce ke zřetězení.
| Další řetězce             |   Ne     | Hodnoty ke zřetězení řetězců.

Tato funkce můžete provést libovolný počet argumentů a jsou přijatelné řetězce nebo matice parametrů. Příklad zřetězení polí naleznete v tématu [propojit - pole](#concatarray).

Následující příklad ukazuje, jak kombinovat víc řetězcové hodnoty vrátit zřetězený řetězec.

    "outputs": {
        "siteUri": {
          "type": "string",
          "value": "[concat('http://', reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
        }
    }


<a id="lengthstring" />
### <a name="length---string"></a>Délka - řetězec

**Length(String)**

Vrátí počet znaků v řetězci.

| Parametr                          | Povinné | Popis
| :--------------------------------: | :------: | :----------
| řetězec                        |   Ano    | Řetězec použít k získání počtu znaků.

Příklad použití délka s polem, najdete v článku [Délka - pole](#length).

Následující příklad vrátí počet znaků v řetězci. 

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "nameLength": "[length(parameters('appName'))]"
    }
        

<a id="padleft" />
### <a name="padleft"></a>padLeft

**padLeft (valueToPad, hodnota totalLength paddingCharacter.)**

Vrátí hodnotu typu string zarovnán doprava přidáním znaky nalevo do dosažení celkové zadané délky.
  
| Parametr                          | Povinné | Popis
| :--------------------------------: | :------: | :----------
| valueToPad                         |   Ano    | Řetězec nebo int Zarovnat doprava.
| Hodnota totalLength                        |   Ano    | Celkový počet znaků v vrácený řetězec.
| paddingCharacter                   |   Ne     | Znak pro odsazení zleva do dosažení celkovou délku. Výchozí hodnota je mezera.

Následující příklad ukazuje, jak doplnit hodnota parametru poskytovaným uživatele přidáním znak nula až řetězec dosáhne 10 znaků. Pokud původní hodnotu parametru je delší než 10 znaků, přidají se žádné znaky.

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "paddedAppName": "[padLeft(parameters('appName'),10,'0')]"
    }

<a id="replace" />
### <a name="replace"></a>nahrazení

**nahrazení (originalString, oldCharacter newCharacter)**

Vrátí nový řetězec se všechny výskyty o jeden znak v zadaný řetězec nahrazuje jiný znak.

| Parametr                          | Povinné | Popis
| :--------------------------------: | :------: | :----------
| originalString                     |   Ano    | Řetězec, který obsahuje všechny výskyty nahrazuje jiný znak o jeden znak.
| oldCharacter                       |   Ano    | Znak má být odebrán z původního řetězce.
| newCharacter                       |   Ano    | Znak přidat místo odebrané znak.

Následující příklad ukazuje, jak odebrat všechny pomlčky z řetězce uživatelem.

    "parameters": {
        "identifier": { "type": "string" }
    },
    "variables": { 
        "newidentifier": "[replace(parameters('identifier'),'-','')]"
    }

<a id="skipstring" />
### <a name="skip---string"></a>Přeskočit - řetězec
**Přeskočit (vlastnosti, numberToSkip)**

Vrátí hodnotu typu string se všechny znaky za číslo zadané v řetězci.

| Parametr                          | Povinné | Popis
| :--------------------------------: | :------: | :----------
| Vlastnosti                      |   Ano    | Řetězec pro použití při vynechání.
| numberToSkip                       |   Ano    | Počet znaků přeskočit. Pokud je tato hodnota 0 nebo menší, budou vráceny všechny znaky v řetězci. Pokud je větší než délka řetězce, vrátí se prázdný řetězec. 

Příklad použití přeskočit s polem najdete v článku [Přeskočit - pole](#skip).

Následující příklad přeskočí na zadaný počet znaků v řetězci.

    "parameters": {
      "first": {
        "type": "string",
        "metadata": {
          "description": "Value to use for skipping"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Number of characters to skip"
        }
      }
    },
    "resources": [
    ],
    "outputs": {
      "return": {
        "type": "string",
        "value": "[skip(parameters('first'),parameters('second'))]"
      }
    }


<a id="split" />
### <a name="split"></a>rozdělení

**úkol rozdělit (inputString, delimiterString)**

**úkol rozdělit (inputString, delimiterArray)**

Vrátí matici řetězce, který obsahuje dílčí řetězce vstupního řetězce, které jsou odděleny zadaných oddělovačů.

| Parametr                          | Povinné | Popis
| :--------------------------------: | :------: | :----------
| inputString                        |   Ano    | Řetězec, který chcete rozdělit.
| oddělovače                          |   Ano    | Oddělovač, který chcete použít, může být jeden řetězec nebo matici řetězců.

Následující příklad rozdělí vstupní řetězec čárkou.

    "parameters": {
        "inputString": { "type": "string" }
    },
    "variables": { 
        "stringPieces": "[split(parameters('inputString'), ',')]"
    }

Následující příklad rozdělí vstupní řetězec čárkou nebo středníkem.

    "variables": {
      "stringToSplit": "test1,test2;test3",
      "delimiters": [ ",", ";" ]
    },
    "resources": [ ],
    "outputs": {
      "exampleOutput": {
        "value": "[split(variables('stringToSplit'), variables('delimiters'))]",
        "type": "array"
      }
    }

<a id="string" />
### <a name="string"></a>řetězec

**String(valueToConvert)**

Převede zadanou hodnotu řetězec.

| Parametr                          | Povinné | Popis
| :--------------------------------: | :------: | :----------
| valueToConvert                     |   Ano    | Hodnota k převodu řetězec. Libovolný typ hodnoty lze převést, včetně objektů a matic.

Následující příklad převede hodnoty parametrů uživatele-za předpokladu, že na řetězce.

    "parameters": {
      "jsonObject": {
        "type": "object",
        "defaultValue": {
          "valueA": 10,
          "valueB": "Example Text"
        }
      },
      "jsonArray": {
        "type": "array",
        "defaultValue": [ "a", "b", "c" ]
      },
      "jsonInt": {
        "type": "int",
        "defaultValue": 5
      }
    },
    "variables": { 
      "objectString": "[string(parameters('jsonObject'))]",
      "arrayString": "[string(parameters('jsonArray'))]",
      "intString": "[string(parameters('jsonInt'))]"
    }

<a id="substring" />
### <a name="substring"></a>podřetězec

**podřetězec (stringToParse, počáteční index, délka)**

Vrátí řetězec začínající určené pozice znaku a obsahuje zadaný počet znaků.

| Parametr                          | Povinné | Popis
| :--------------------------------: | :------: | :----------
| stringToParse                     |   Ano    | Původní řetězec, ze kterého se extrahuje podřetězec.
| Počáteční index                         | Ne      | Od nuly znak pozice pro podřetězec.
| Délka                             | Ne      | Počet znaků pro podřetězec.

Následující příklad získá první tři znaky z parametru.

    "parameters": {
        "inputString": { "type": "string" }
    },
    "variables": { 
        "prefix": "[substring(parameters('inputString'), 0, 3)]"
    }

<a id="takestring" />
### <a name="take---string"></a>převzetí - řetězec
**převzetí (vlastnosti, numberToTake)**

Vrátí řetězec s zadaný počet znaků od počátku řetězec.

| Parametr                          | Povinné | Popis
| :--------------------------------: | :------: | :----------
| Vlastnosti                      |   Ano    | Řetězec, který má brát znaků z.
| numberToTake                       |   Ano    | Počet znaků přijmout. Pokud je tato hodnota 0 nebo menší, vrátí se prázdný řetězec. Pokud je větší než délka řetězce dané, budou vráceny všechny znaky v řetězci.

Příklad použití dělat s polem najdete v článku [trvat - pole](#take).

Následující příklad popisuje zadaný počet znaků z řetězce.

    "parameters": {
      "first": {
        "type": "string",
        "metadata": {
          "description": "Value to use for taking"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Number of characters to take"
        }
      }
    },
    "resources": [
    ],
    "outputs": {
      "return": {
        "type": "string",
        "value": "[take(parameters('first'), parameters('second'))]"
      }
    }

<a id="tolower" />
### <a name="tolower"></a>toLower

**toLower(stringToChange)**

Převede zadaný řetězec na malá písmena.

| Parametr                          | Povinné | Popis
| :--------------------------------: | :------: | :----------
| stringToChange                     |   Ano    | Řetězec, který chcete převést na malá písmena.

Následující příklad převede hodnotu parametru uživatele-za předpokladu, že na malá písmena.

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "lowerCaseAppName": "[toLower(parameters('appName'))]"
    }

<a id="toupper" />
### <a name="toupper"></a>toUpper

**toUpper(stringToChange)**

Převede zadaný řetězec na velká písmena.

| Parametr                          | Povinné | Popis
| :--------------------------------: | :------: | :----------
| stringToChange                     |   Ano    | Řetězec, který chcete převést na velká písmena.

Následující příklad převede hodnotu parametru uživatele-za předpokladu, že na velká písmena.

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "upperCaseAppName": "[toUpper(parameters('appName'))]"
    }

<a id="trim" />
### <a name="trim"></a>Střih

**funkce Trim (stringToTrim)**

Odebere všechny úvodní a koncové prázdné znaky z určitým řetězcem.

| Parametr                          | Povinné | Popis
| :--------------------------------: | :------: | :----------
| stringToTrim                       |   Ano    | Řetězec, který chcete oříznout.

Následující příklad ořízne prázdných znaků z hodnota parametru uživatelem.

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "trimAppName": "[trim(parameters('appName'))]"
    }

<a id="uniquestring" />
### <a name="uniquestring"></a>uniqueString

**uniqueString (baseString,...)**

Vytvoří řetězec deterministický hash podle hodnoty zadané jako parametry. 

| Parametr                          | Povinné | Popis
| :--------------------------------: | :------: | :----------
| baseString      |   Ano    | Řetězec, který slouží k vytvoření jedinečných řetězce v funkci hash.
| Další parametry podle potřeby    | Ne       | Můžete přidat tolik řetězce v případě potřeby vytvořit hodnotu, která určuje úroveň jedinečnost.

Tato funkce je užitečné, když je potřeba vytvořit jedinečný název zdroje. Zadáte hodnoty parametrů, které omezení oboru jedinečnost výsledku. Můžete určit, zda je v poli název jedinečný dolů předplatného, skupina zdroje nebo nasazení. 

Vrácená hodnota není řetězec náhodné, ale raději výsledek funkce hash. Vrácená hodnota je 13 znaků. Není globálně jedinečný. Je vhodné hodnoty kombinovat s předponou z vaší konvence pro vytvoření smysluplný název. Následující příklad ukazuje formát vrácenou hodnotu. Samozřejmě skutečná hodnota se liší podle zadaných parametrů.

    tcvhiyu5h2o5o

Následující příklady ukazují, jak používat uniqueString k vytvoření jedinečné hodnoty pro běžně používaných úrovně.

Jedinečný rozsah k předplatnému

    "[uniqueString(subscription().subscriptionId)]"

Jedinečný obor vymezený na pole Skupina zdroje

    "[uniqueString(resourceGroup().id)]"

Jedinečný obor vymezený na nasazení skupina zdroje

    "[uniqueString(resourceGroup().id, deployment().name)]"
    
Následující příklad ukazuje, jak vytvořit jedinečný název účtu úložiště podle skupiny zdrojů (do této skupiny zdrojů název není jedinečný if vytvořen stejným způsobem jako).

    "resources": [{ 
        "name": "[concat('contosostorage', uniqueString(resourceGroup().id))]", 
        "type": "Microsoft.Storage/storageAccounts", 
        ...



<a id="uri" />
### <a name="uri"></a>identifikátor URI

**identifikátor URI (baseUri, relativeUri)**

Vytvoří absolutní identifikátor URI spojením baseUri a relativeUri řetězec.

| Parametr                          | Povinné | Popis
| :--------------------------------: | :------: | :----------
| baseUri                            |   Ano    | Základní uri řetězec.
| relativeUri                        |   Ano    | Relativní identifikátor uri řetězec přidáte řetězec základní uri.

Hodnota parametru **baseUri** mohou obsahovat konkrétním souborem, ale jenom základní cestě se používá při vytváření identifikátor URI. Například k předání **http://contoso.com/resources/azuredeploy.json** jako baseUri parametr výsledkem základní URI **http://contoso.com/resources/**.

Následující příklad ukazuje, jak vytvořit odkaz na vnořené šablony na základě hodnoty nadřazené šablony.

    "templateLink": "[uri(deployment().properties.templateLink.uri, 'nested/azuredeploy.json')]"

## <a name="array-functions"></a>Funkce Array

Správce prostředků nabízí několik funkcí pro práci s pole hodnoty.

- [propojit](#concatarray)
- [Délka](#length)
- [Přeskočit](#skip)
- [přijmout](#take)

Palety řetězcové hodnoty s oddělovači podle hodnot, získáte [rozdělení](#split).

<a id="concatarray" />
### <a name="concat---array"></a>propojit - matice

**propojit (pole1, pole2; pole3;...)**

Kombinuje více matice a vrátí zřetězený matici s odpovědí. 

| Parametr                          | Povinné | Popis
| :--------------------------------: | :------: | :----------
| Pole1                        |   Ano    | Matice ke zřetězení.
| Další pole             |   Ne     | Matice ke zřetězení.

Tato funkce můžete provést libovolný počet argumentů a jsou přijatelné řetězce nebo matice parametrů. Příklad řetězení textových hodnot najdete v článku [propojit - řetězec](#concat).

Následující příklad ukazuje, jak sloučení dvou matic.

    "parameters": {
        "firstarray": {
            type: "array"
        }
        "secondarray": {
            type: "array"
        }
     },
     "variables": {
         "combinedarray": "[concat(parameters('firstarray'), parameters('secondarray'))]
     }
        

<a id="length" />
### <a name="length---array"></a>Délka - matice

**Length(Array)**

Vrátí počet prvků v matici.

| Parametr                          | Povinné | Popis
| :--------------------------------: | :------: | :----------
| pole                        |   Ano    | Pole pro účely zobrazuje počet prvků.

Tato funkce s polem slouží k určení počtu iterací při vytváření zdroje. V následujícím příkladu by parametr **siteNames** podívejte se do pole názvů při vytváření na webech.

    "copy": {
        "name": "websitescopy",
        "count": "[length(parameters('siteNames'))]"
    }

Další informace o použití této funkce s polem najdete v článku [vytvoření několika instancích zdrojů v Azure správce](resource-group-create-multiple.md). 

Příklad použití délka s hodnotou řetězce najdete v článku [Délka - řetězec](#lengthstring).

<a id="skip" />
### <a name="skip---array"></a>Přeskočit - pole
**Přeskočit (vlastnosti, numberToSkip)**

Vrátí matici s všechny prvky za číslo zadané v poli.

| Parametr                          | Povinné | Popis
| :--------------------------------: | :------: | :----------
| Vlastnosti                      |   Ano    | Pole pro účely vynechání.
| numberToSkip                       |   Ano    | Počet prvků přeskočit. Pokud je tato hodnota 0 nebo menší, budou vráceny všechny prvky v poli. Pokud je větší než délka matice, vrátí se hodnota matici prázdné. 

Příklad použití přeskočit řetězcem najdete v článku [Přeskočit - řetězec](#skipstring).

Následující příklad přeskočí na zadaný počet prvků v poli.

    "parameters": {
      "first": {
        "type": "array",
        "metadata": {
          "description": "Values to use for skipping"
        },
        "defaultValue": [ "one", "two", "three" ]
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Number of elements to skip"
        }
      }
    },
    "resources": [
    ],
    "outputs": {
      "return": {
        "type": "array",
        "value": "[skip(parameters('first'), parameters('second'))]"
      }
    }

<a id="take" />
### <a name="take---array"></a>Prohlédněte - pole
**převzetí (vlastnosti, numberToTake)**

Vrátí matici s zadaný počet prvků od začátku pole.

| Parametr                          | Povinné | Popis
| :--------------------------------: | :------: | :----------
| Vlastnosti                      |   Ano    | Pole se elementů z.
| numberToTake                       |   Ano    | Počet prvků přijmout. Pokud je tato hodnota 0 nebo menší, bude vrácena prázdné pole. Pokud je větší než délka dané pole, budou vráceny všechny prvky v poli.

Příklad použití dělat s řetězcem najdete v článku [trvat - řetězec](#takestring).

Následující příklad popisuje zadaný počet prvků z pole.

    "parameters": {
      "first": {
        "type": "array",
        "metadata": {
          "description": "Values to use for taking"
        },
        "defaultValue": [ "one", "two", "three" ]
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Number of elements to take"
        }
      }
    },
    "resources": [
    ],
    "outputs": {
      "return": {
        "type": "array",
        "value": "[take(parameters('first'),parameters('second'))]"
      }
    }

## <a name="deployment-value-functions"></a>Funkce hodnota nasazení

Správce prostředků poskytuje následující funkce pro získání hodnoty z oddílů šablony a hodnoty související s nasazení:

- [nasazení](#deployment)
- [Parametry](#parameters)
- [proměnné](#variables)

Hodnoty ze zdroje, skupiny zdrojů nebo předplatná získáte v tématu [funkce zdroje](#resource-functions).

<a id="deployment" />
### <a name="deployment"></a>nasazení

**Deployment()**

Vrátí informaci o aktuálním operace nasazení.

Tato funkce vrátí objekt, které je při nasazení. Vlastnosti objektu vrácené se liší v závislosti na tom, jestli je objekt nasazení předán jako odkazu nebo objektu v řádku. 

Při nasazení objekt je předán vložených, jako je třeba, pokud chcete použít parametr **– TemplateFile** Azure PowerShell tak, aby ukazovaly na místní soubor, vrácené objektu nachází v tomto formátu:

    {
        "name": "",
        "properties": {
            "template": {
                "$schema": "",
                "contentVersion": "",
                "parameters": {},
                "variables": {},
                "resources": [
                ],
                "outputs": {}
            },
            "parameters": {},
            "mode": "",
            "provisioningState": ""
        }
    }

Kdy objekt předané jako odkaz, například pokud chcete použít parametr **– TemplateUri** tak, aby ukazovaly na objekt vzdálené, objekt je vrácena v v tomto formátu. 

    {
        "name": "",
        "properties": {
            "templateLink": {
                "uri": ""
            },
            "template": {
                "$schema": "",
                "contentVersion": "",
                "parameters": {},
                "variables": {},
                "resources": [],
                "outputs": {}
            },
            "parameters": {},
            "mode": "",
            "provisioningState": ""
        }
    }

Následující příklad ukazuje, jak použít deployment() pro propojení s jinou šablonu podle URI nadřazené šablony.

    "variables": {  
        "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"  
    }  

<a id="parameters" />
### <a name="parameters"></a>Parametry

**Parametry (název parametru)**

Vrátí hodnotu parametru. V části Parametry šablony je třeba definovat název zadaný parametr.

| Parametr                          | Povinné | Popis
| :--------------------------------: | :------: | :----------
| Název parametru                      |   Ano    | Název parametru vrátit.

Následující příklad ukazuje zjednodušené použití funkce parametry.

    "parameters": { 
      "siteName": {
          "type": "string"
      }
    },
    "resources": [
       {
          "apiVersion": "2014-06-01",
          "name": "[parameters('siteName')]",
          "type": "Microsoft.Web/Sites",
          ...
       }
    ]

<a id="variables" />
### <a name="variables"></a>proměnné

**proměnné (PromptText)**

Vrátí hodnotu proměnné. V části proměnné šablony je třeba definovat zadaný název proměnné.

| Parametr                          | Povinné | Popis
| :--------------------------------: | :------: | :----------
| Proměnná název                      |   Ano    | Název proměnné vrátit.

Následující příklad využívá hodnotu proměnné.

    "variables": {
      "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
      {
        "type": "Microsoft.Storage/storageAccounts",
        "name": "[variables('storageName')]",
        ...
      }
    ],

## <a name="resource-functions"></a>Funkce zdroje

Správce prostředků poskytuje následující funkce pro získání hodnoty zdroje:

- [listKeys a seznam {hodnota}](#listkeys)
- [poskytovatelé](#providers)
- [odkaz](#reference)
- [resourceGroup](#resourcegroup)
- [resourceId](#resourceid)
- [předplatné](#subscription)

Hodnoty parametrů, proměnné nebo aktuální nasazení získáte v tématu [funkce hodnota nasazení](#deployment-value-functions).

<a id="listkeys" />
<a id="list" />
### <a name="listkeys-and-listvalue"></a>listKeys a seznam {hodnota}

**listKeys (resourceName nebo resourceIdentifier, apiVersion)**

**seznam {hodnota} (resourceName nebo resourceIdentifier, apiVersion)**

Vrátí hodnoty pro každý typ zdroje, který podporuje operace list. Nejběžnější použití je **listKeys**. 
  
| Parametr                          | Povinné | Popis
| :--------------------------------: | :------: | :----------
| resourceName nebo resourceIdentifier |   Ano    | Jedinečný identifikátor zdroje.
| apiVersion                         |   Ano    | Rozhraní API verze zdroje průběžný stav.

Operaci, kterou lze začíná **seznamu** použít funkci v šabloně. K dispozici operace obsahovat pouze **listKeys**, ale taky operací, jako je **seznam** **listAdminKeys**a **listStatus**. Rozhodnout, které typy prostředků operace seznamu, pomocí následujícího příkazu Powershellu.

    Get-AzureRmProviderOperation -OperationSearchString *  | where {$_.Operation -like "*list*"} | FT Operation

Nebo načtení seznamu s Azure rozhraní příkazového řádku. Následující příklad načte všechny operace pro **apiapps**a používá nástroj JSON [jq](http://stedolan.github.io/jq/download/) filtrovat seznam způsobů.

    azure provider operations show --operationSearchString */apiapps/* --json | jq ".[] | select (.operation | contains(\"list\"))"

ResourceId lze pomocí [funkce resourceId](#resourceid) nebo pomocí formátu **{providerNamespace} / {resourceType} / {resourceName}**.

Následující příklad ukazuje, jak k vrácení klíče primárních a sekundárních z úložiště účtu v části výstupy.

    "outputs": { 
      "listKeysOutput": { 
        "value": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName')), '2016-01-01')]", 
        "type" : "object" 
      } 
    } 

Vrácené objektu z listKeys nachází v tomto formátu:

    {
      "keys": [
        {
          "keyName": "key1",
          "permissions": "Full",
          "value": "{value}"
        },
        {
          "keyName": "key2",
          "permissions": "Full",
          "value": "{value}"
        }
      ]
    }

<a id="providers" />
### <a name="providers"></a>poskytovatelé

**poskytovatelé (providerNamespace, [resourceType])**

Vrátí informace o zprostředkovatele zdroje a jeho typů podporovaných zdrojů. Pokud nezadáte typ zdroje, funkce vrátí podporované typy zprostředkovatele zdroje.

| Parametr                          | Povinné | Popis
| :--------------------------------: | :------: | :----------
| providerNamespace                  |   Ano    | Namespace poskytovatele
| resourceType                       |   Ne     | Typ zdroje v rámci zadaný názvů.

Každý podporovaný typ je vrácena v v tomto formátu. Pole řazení není zaručené.

    {
        "resourceType": "",
        "locations": [ ],
        "apiVersions": [ ]
    }

Následující příklad ukazuje, jak se používá funkce poskytovatele:

    "outputs": {
        "exampleOutput": {
            "value": "[providers('Microsoft.Storage', 'storageAccounts')]",
            "type" : "object"
        }
    }

<a id="reference" />
### <a name="reference"></a>odkaz

**odkaz (resourceName nebo resourceIdentifier, [apiVersion])**

Vrátí objekt reprezentující jinému zdroji průběžný stav.

| Parametr                          | Povinné | Popis
| :--------------------------------: | :------: | :----------
| resourceName nebo resourceIdentifier |   Ano    | Název nebo jedinečný identifikátor zdroje.
| apiVersion                         |   Ne     | Rozhraní API verzi je daný zdroj. Zahrnout tento parametr není ve stejné šablony zřízení zdroje.

Funkce **odkaz** je odvozena jeho hodnotu z průběžný stav a proto je nelze použít v části proměnné. Lze použít v části výstupy šablony.

Pomocí funkce odkaz je implicitně deklarovat, že jeden zdroj, závisí na jiný zdroj Pokud odkazované zdroje máte k dispozici v rámci stejné šablony. Není potřeba také použít vlastnost **dependsOn** . Funkce není vyhodnocen, dokud odkazované zdroj dokončil nasazení.

Následující příklad odkazuje úložiště účet, který je nasazenou v stejnou šablonu.

    "outputs": {
        "NewStorage": {
            "value": "[reference(parameters('storageAccountName'))]",
            "type" : "object"
        }
    }

Následující příklad odkazuje úložiště účet, který není používaný v této šabloně, ale existuje ve stejné skupině zdroje jako zdroje nasazení.

    "outputs": {
        "ExistingStorage": {
            "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01')]",
            "type" : "object"
        }
    }

Můžete načtete určitou hodnotu z vrácených objekt, například objektů blob identifikátor URI koncového bodu, jak je vidět v následujícím příkladu.

    "outputs": {
        "BlobUri": {
            "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01').primaryEndpoints.blob]",
            "type" : "string"
        }
    }

Následující příklad odkazuje úložiště účtu v jiné skupině zdrojů.

    "outputs": {
        "BlobUri": {
            "value": "[reference(resourceId(parameters('relatedGroup'), 'Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01').primaryEndpoints.blob]",
            "type" : "string"
        }
    }

Vlastnosti objektu vrácených z funkce **odkaz** se liší podle typu zdroje. Zobrazíte vlastnosti názvy a hodnoty pro typ zdroje vytvořte jednoduchý šablonu, která vrací objekt v části **výstup** . Pokud máte existující zdroj tohoto typu, vrátí šablony jenom objekt bez nasazení nového zdroje. Pokud nemáte existující zdroj tohoto typu, šablony nasadí pouze tento typ a vrátí objekt. Potom přidejte tyto vlastnosti do jiných šablon, které je potřeba dynamicky načtení hodnot během nasazení. 

<a id="resourcegroup" />
### <a name="resourcegroup"></a>resourceGroup

**resourceGroup()**

Vrátí objekt, který představuje aktuální skupina zdroje. 

Vrácené objekt je v tomto formátu:

    {
      "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}",
      "name": "{resourceGroupName}",
      "location": "{resourceGroupLocation}",
      "tags": {
      },
      "properties": {
        "provisioningState": "{status}"
      }
    }

Umístění skupiny zdroj přiřadit umístění webu v následujícím příkladu.

    "resources": [
       {
          "apiVersion": "2014-06-01",
          "type": "Microsoft.Web/sites",
          "name": "[parameters('siteName')]",
          "location": "[resourceGroup().location]",
          ...
       }
    ]

<a id="resourceid" />
### <a name="resourceid"></a>resourceId

**resourceId ([subscriptionId], [resourceGroupName] resourceType, resourceName1, [resourceName2]...)**

Vrátí jedinečný identifikátor zdroje. 
      
| Parametr         | Povinné | Popis
| :---------------: | :------: | :----------
| subscriptionId    |   Ne     | Výchozí hodnota je aktuálního předplatného. Zadejte tuto hodnotu, když potřebujete získat zdroj v jiné předplatné.
| resourceGroupName |   Ne     | Výchozí hodnota je aktuální pole Skupina zdroje. Zadejte tuto hodnotu, když potřebujete získat zdroj v jiné skupině zdroje.
| resourceType      |   Ano    | Typ zdroje včetně poskytovatele názvů zdrojů.
| resourceName1     |   Ano    | Název zdroje.
| resourceName2     |   Ne     | Další zdroje název segment Pokud vnořené zdroje.

Tuto funkci použijte, když je v poli Název zdroje dvojznačná nebo není vytvořený v rámci stejné šablony. Identifikátor je vrácena ve formátu následující:

    /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/{resourceProviderNamespace}/{resourceType}/{resourceName}

Následující příklad ukazuje, jak načíst ID zdrojů webu a databáze. Na webu existuje ve skupině zdroje s názvem **myWebsitesGroup** a databáze existuje ve skupině aktuální prostředků pro tuto šablonu.

    [resourceId('myWebsitesGroup', 'Microsoft.Web/sites', parameters('siteName'))]
    [resourceId('Microsoft.SQL/servers/databases', parameters('serverName'), parameters('databaseName'))]
    
Často budete muset používat tuto funkci úložiště klienta nebo virtuální sítě ve skupině alternativní zdroje. Úložiště klienta nebo virtuální sítě se může použít ve více zdrojů skupinách; proto nechcete je můžete odstranit při odstraňování skupina jednoho zdroje. Následující příklad ukazuje, jak zdroj z externího zdroje skupiny snadno lze:

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
          "virtualNetworkName": {
              "type": "string"
          },
          "virtualNetworkResourceGroup": {
              "type": "string"
          },
          "subnet1Name": {
              "type": "string"
          },
          "nicName": {
              "type": "string"
          }
      },
      "variables": {
          "vnetID": "[resourceId(parameters('virtualNetworkResourceGroup'), 'Microsoft.Network/virtualNetworks', parameters('virtualNetworkName'))]",
          "subnet1Ref": "[concat(variables('vnetID'),'/subnets/', parameters('subnet1Name'))]"
      },
      "resources": [
      {
          "apiVersion": "2015-05-01-preview",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[parameters('nicName')]",
          "location": "[parameters('location')]",
          "properties": {
              "ipConfigurations": [{
                  "name": "ipconfig1",
                  "properties": {
                      "privateIPAllocationMethod": "Dynamic",
                      "subnet": {
                          "id": "[variables('subnet1Ref')]"
                      }
                  }
              }]
           }
      }]
    }

<a id="subscription" />
### <a name="subscription"></a>předplatné

**Subscription()**

Vrátí podrobnosti předplatného v tomto formátu.

    {
        "id": "/subscriptions/#####",
        "subscriptionId": "#####",
        "tenantId": "#####"
    }

Následující příklad zobrazuje použití funkce předplatné s názvem v části výstupů. 

    "outputs": { 
      "exampleOutput": { 
          "value": "[subscription()]", 
          "type" : "object" 
      } 
    } 


## <a name="next-steps"></a>Další kroky
- Popis oddíly v šabloně aplikace Správce prostředků Azure najdete v tématu [vytváření správce prostředků Azure šablony](resource-group-authoring-templates.md)
- Ke sloučení více šablon, najdete v článku [použití šablon propojené s správce prostředků Azure](resource-group-linked-templates.md)
- Iterace zadaný počet při vytváření typu zdroje, najdete v článku [vytvoření několika instancích zdrojů v správce prostředků Azure](resource-group-create-multiple.md)
- Nasazení šablony, kterou jste vytvořili najdete v tématu [nasazení aplikace šablonou správce prostředků Azure](resource-group-template-deploy.md)

