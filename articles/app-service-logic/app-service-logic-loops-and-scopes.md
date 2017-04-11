<properties
   pageTitle="Použití logických operátorů aplikace smyčky obory a Debatching | Microsoft Azure"
   description="Použití logických operátorů aplikace smyčka obor a debatching koncepty"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/14/2016"
   ms.author="jehollan"/>
   
# <a name="logic-apps-loops-scopes-and-debatching"></a>Použití logických operátorů aplikace smyčky obory a Debatching
  
>[AZURE.NOTE] Tuto verzi článku platí schéma logiku aplikace 2016 – 04-01náhled a v novějších verzích.  Jsou podobné starší schémat koncepty, ale obory jsou pouze k dispozici pro toto schéma a novější.
  
## <a name="foreach-loop-and-arrays"></a>Smyčka ForEach a maticemi
  
Použití logických operátorů aplikace umožňuje smyčku množiny dat a provedení akce pro každou položku.  Toto je možné prostřednictvím `foreach` akce.  V návrháři, můžete zadat přidáte pro každé smyčce.  Po výběru pole, které chcete iterace, můžete začít přidávat akce.  Nyní jsou omezené na pouze jednu akci za smyčky foreach, ale toto omezení se zruší během příštích týdnů.  Pokud v obraze můžete začnete můžete určit, co by měla proběhnout v každé hodnotě matici.

Pokud používáte zobrazení kódu, můžete určit, pro každou smyčka jako pod.  Toto je příklad pro každou smyčky, která odešle e-mailu pro každé e-mailovou adresu, která obsahuje "microsoft.com":

```
{
    "email_filter": {
        "type": "query",
        "inputs": {
            "from": "@triggerBody()['emails']",
            "where": "@contains(item()['email'], 'microsoft.com')
        }
    },
    "forEach_email": {
        "type": "foreach",
        "foreach": "@body('email_filter')",
        "actions": {
            "send_email": {
                "type": "ApiConnection",
                "inputs": {
                "body": {
                    "to": "@item()",
                    "from": "me@contoso.com",
                    "message": "Hello, thank you for ordering"
                }
                "host": {
                    "connection": {
                        "id": "@parameters('$connections')['office365']['connection']['id']"
                    }
                }
                }
            }
        },
        "runAfter":{
            "email_filter": [ "Succeeded" ]
        }
    }
}
```
  
  A `foreach` akce můžete zapracovávat ji přes maticových až 5 000 řádků.  Každé opakování lze spustit paralelně, takže možná budete muset přidat zprávy do fronty v případě potřeby řízení toku.
  
## <a name="until-loop"></a>Až smyčky
  
  Dokud splnění podmínky, můžete provádět akce nebo posloupnosti akcí.  Nejběžnější scénář je volání koncový bod, až se dostanete odpověď, kterou hledáte.  V návrháři, můžete zadat přidání do smyčky.  Po přidání akce uvnitř opakovat, můžete nastavit stav ukončení, jakož i opakovat omezení.  Mezi smyčka cyklů je prodleva minutu.
  
  Pokud používáte zobrazení kódu, můžete určit,, dokud smyčka líbí pod.  Toto je příklad volání koncový bod HTTP, dokud obsah odpovědí má hodnotu "Dokončeno".  Kdy dokončí buď 
  
  * Odpověď HTTP má stav "Dokončeno"
  * Má vyzkoušeli pro 1 hodinu
  * Má opakuje 100 časy
  
  ```
  {
      "until_successful":{
        "type": "until",
        "expression": "@equals(actions('http')['status'], 'Completed'),
        "limit": {
            "count": 100,
            "timeout": "PT1H"
        },
        "actions": {
            "create_resource": {
                "type": "http",
                "inputs": {
                    "url": "http://provisionRseource.com",
                    "body": {
                        "resourceId": "@triggerBody()"
                    }
                }
            }
        }
      }
  }
  ```
  
## <a name="spliton-and-debatching"></a>SplitOn a Debatching

Někdy aktivační obdržet maticových položky, které chcete debatch a spustit pracovní postup podle předmětu.  Můžete to provést prostřednictvím `spliton` příkaz.  Ve výchozím nastavení, pokud aktivační událost swagger Určuje data, která je polem `spliton` se přidá a začněte spustit podle předmětu.  SplitOn můžete přidat jenom aktivační událost.  To může ručně nakonfigurovali nebo přepsat v definice zobrazení kódu.  Nyní můžete debatch SplitOn maticových až 5 000 položek.  Nesmí obsahovat `spliton` a také implementovat vzorek syncronous odpověď.  Všechny pracovní postupy s názvem, který má `response` akce kromě `spliton` spustí asyncronously a odeslání okamžitého `202 Accepted` odpověď.  

SplitOn může být zadán v zobrazení kódu jako v následujícím příkladu.  Tento recieves maticových položek a debatches na každý řádek.

```
{
    "myDebatchTrigger": {
        "type": "Http",
        "inputs": {
            "url": "http://getNewCustomers",
        },
        "recurrence": {
            "frequency": "Second",
            "interval": 15
        },
        "spliton": "@triggerBody()['rows']"
    }
}
```

## <a name="scopes"></a>Oborů

Je možné zařadit do skupiny posloupnosti akcí spolupráce pomocí obor.  Toto je užitečné pro implementaci výjimek.  V Návrháři můžete přidat nový obor a začít přidávat akcích do něj.  Definování oborů v zobrazení kódu takto:


```
{
    "myScope": {
        "type": "scope",
        "actions": {
            "call_bing": {
                "type": "http",
                "inputs": {
                    "url": "http://www.bing.com"
                }
            }
        }
    }
}
```
