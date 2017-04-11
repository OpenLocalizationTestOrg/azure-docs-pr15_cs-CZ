<properties 
    pageTitle="Nové schématu verze 2015-08-01-preview" 
    description="Zjistěte, jak psát definici JSON v nejnovější verzi aplikace použití logických operátorů" 
    authors="stepsic-microsoft-com" 
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
    ms.date="05/31/2016"
    ms.author="stepsic"/>
    
# <a name="new-schema-version-2015-08-01-preview"></a>Nové schématu verze 2015-08-01-preview

Nové schéma a rozhraní API verze aplikace logiky má řadu vylepšení, která zlepšit spolehlivost a snadnější použití logických operátorů aplikací. Existují 4 důležité rozdíly:

1. Byl aktualizován typ akce **APIApp** , aby nový typ akce **APIConnection** .
2. **Opakujte** byla přejmenoval na **Foreach**.
3. Rozhraní API aplikace **HTTP posluchače** už není potřeba.
4. Volání podřízených pracovních postupů používá nové schéma.

## <a name="1-moving-to-api-connections"></a>1. přesunutí do rozhraní API připojení

Největší změnit, aby se, že již nepotřebujete nasazení rozhraní API aplikace do vašeho předplatného Azure pomocí rozhraní API. Použití rozhraní API 2 způsoby:
* Spravované rozhraní API
* Vaše vlastní webového rozhraní API

Každá z těchto má na starosti trochu jinak protože jejich správy a hostování modely se liší. Jednou z výhod tento model je, že je jste už není omezena pouze zdroje, které jsou nasazeny ve skupině prostředků. 

### <a name="managed-apis"></a>Spravované rozhraní API

Existuje celá řada rozhraní API, která se spravuje Microsoft vaším jménem, například Office 365, Salesforce, Twitter atd FTP... Některé z těchto spravované rozhraní API mohou sloužit jako-je, jako je Bing přeložit, zatímco jiné vyžadují konfigurace. *Připojení*se nazývá tuto konfiguraci.

Například pokud používáte Office 365, je potřeba vytvořit připojení, která obsahuje tokenu přihlašování v Office 365. Tento token budou bezpečně uloženy a aktualizovat tak, aby aplikace logiky vždy upoutat rozhraní API Office 365. Můžete taky Pokud chcete připojit k serveru SQL nebo FTP, je potřeba vytvořit připojení, který má připojovací řetězec. 

Uvnitř definici tyto akce se označují jako `APIConnection`. Tady je příklad připojení, která volá služby Office 365 k e-mailovou zprávu:

```
{
    "actions": {
        "Send_Email": {
            "type": "ApiConnection",
            "inputs": {
                "host": {
                    "api": {
                        "runtimeUrl": "https://msmanaged-na.azure-apim.net/apim/office365"
                    },
                    "connection": {
                        "name": "@parameters('$connections')['shared_office365']['connectionId']"
                    }
                },
                "method": "post",
                "body": {
                    "Subject": "Reminder",
                    "Body": "Don't forget!",
                    "To": "me@contoso.com"
                },
                "path": "/Mail"
            }
        }
    }
}
```

Část vstupní hodnoty, které jsou jedinečné pro rozhraní API připojení je `host` objektu. Tato stránka obsahuje dvě části: `api` a `connection`.

`api` Má modul runtime adresu URL, kde spravované rozhraní API hostuje. Zobrazí se všechny dostupné spravované rozhraní API pro vás tak, že zavoláte `GET https://management.azure.com/subscriptions/{subid}/providers/Microsoft.Web/managedApis/?api-version=2015-08-01-preview`.

Při použití rozhraní API může nebo nemusí být všechny **parametry připojení** definován. Pokud není požadovaný je bez **připojení** . Pokud ano, budete muset vytvořit připojení. Když vytvoříte toto připojení budete mít název zvolíte, a poté v odkazování `connection` objekt uvnitř `host` objektu. Pokud chcete vytvořit připojení ve skupině zdroje, zavolejte:

```
PUT https://management.azure.com/subscriptions/{subid}/resourceGroups/{rgname}/providers/Microsoft.Web/connections/{name}?api-version=2015-08-01-preview
```

Pomocí následující text:


```
{
  "properties": {
    "api": {
      "id": "/subscriptions/{subid}/providers/Microsoft.Web/managedApis/azureblob"
    },
    "parameterValues" : {
        "accountName" : "{The name of the storage account -- the set of parameters is different for each API}"
    }
  },
  "location" : "{Logic app's location}"
}
```

### <a name="deploying-managed-apis-in-an-azure-resource-manager-template"></a>Nasazení spravované rozhraní API v šabloně správce zdrojů Azure

Úplné aplikace můžete vytvořit v šabloně aplikace ARM, dokud nepotřeboval interaktivní přihlásit. Pokud se musí přihlásit, můžete nastavení všech údajů šablonou ARM ale ještě Navštěvujte blog o portálu povolit připojení. 

```
    "resources": [{
        "apiVersion": "2015-08-01-preview",
        "name": "azureblob",
        "type": "Microsoft.Web/connections",
        "location": "[resourceGroup().location]",
        "properties": {
            "api": {
                "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
            },
            "parameterValues": {
                "accountName": "[parameters('storageAccountName')]",
                "accessKey": "[parameters('storageAccountKey')]"
            }
        }
    }, {
        "type": "Microsoft.Logic/workflows",
        "apiVersion": "2015-08-01-preview",
        "name": "[parameters('logicAppName')]",
        "location": "[resourceGroup().location]",
        "dependsOn": [
            "[resourceId('Microsoft.Web/connections', 'azureblob')]"
        ],
        "properties": {
            "sku": {
                "name": "[parameters('sku')]",
                "plan": {
                    "id": "[concat(resourceGroup().id, '/providers/Microsoft.Web/serverfarms/',parameters('svcPlanName'))]"
                }
            },
            "definition": {
                "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2015-08-01-preview/workflowdefinition.json#",
                "actions": {
                    "Create_file": {
                        "type": "apiconnection",
                        "inputs": {
                            "host": {
                                "api": {
                                    "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/azureblob"
                                },
                                "connection": {
                                    "name": "@parameters('$connections')['azureblob']['connectionId']"
                                }
                            },
                            "method": "post",
                            "queries": {
                                "folderPath": "[concat('/',parameters('containerName'))]",
                                "name": "helloworld.txt"
                            },
                            "body": "@decodeDataUri('data:,Hello+world!')",
                            "path": "/datasets/default/files"
                        },
                        "conditions": []
                    }
                },
                "contentVersion": "1.0.0.0",
                "outputs": {},
                "parameters": {
                    "$connections": {
                        "defaultValue": {},
                        "type": "Object"
                    }
                },
                "triggers": {
                    "recurrence": {
                        "type": "Recurrence",
                        "recurrence": {
                            "frequency": "Day",
                            "interval": 1
                        }
                    }
                }
            },
            "parameters": {
                "$connections": {
                    "value": {
                        "azureblob": {
                            "connectionId": "[concat(resourceGroup().id,'/providers/Microsoft.Web/connections/azureblob')]",
                            "connectionName": "azureblob",
                            "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
                        }

                    }
                }
            }
        }
    }]
```

Zobrazí se v tomto příkladu jsou jenom normální prostředky, které bloky v skupina zdroje připojení. K dispozici ve vašem předplatném managedAPIs odkazují.

### <a name="your-custom-web-apis"></a>Vlastní rozhraní API webových

Pokud používáte vlastní rozhraní API (konkrétně není Microsoft Správa přístupových práv z nich), měli byste zavolat je použít předdefinované akce **HTTP** . Aby mohl ideální prostředí, by měl vystavíte swagger koncový bod pro vaše rozhraní API. To vám umožní návrháře logiku aplikace k vykreslování vstupů a výstupů pro vaše rozhraní API. Bez swagger návrháři pouze budou moct zobrazit vstupů a výstupů jako neprůhledné JSON objekty.

Tady je příklad zobrazující nové `metadata.apiDefinitionUrl` vlastnosti:
```
{
   "actions": {
        "mycustomAPI": {
            "type": "http",
            "metadata" : {
              "apiDefinitionUrl" : "https://mysite.azurewebsites.net/api/apidef/"  
            },
            "inputs": {
                "uri": "https://mysite.azurewebsites.net/api/getsomedata",
                "method" : "GET"
            }
        }
    }
}
```

Pokud hostujete vaše rozhraní API webových **Aplikací** služby a pak je, že se automaticky objeví v seznamu dostupné v Návrháři akce. Pokud ne, budete muset vložte adresu URL přímo. Koncový bod swagger musí neověřený provést použitelné uvnitř návrháře logiky aplikace (i když může zabezpečené samotné rozhraní API s jakékoli metody jsou podporované v Swagger).

### <a name="using-your-already-deployed-api-apps-with-2015-08-01-preview"></a>Použití už nasazeném rozhraní API aplikace s 2015 08 01 náhledu

Pokud jste dříve nainstalovali aplikace rozhraní API, zavolejte ho prostřednictvím **protokolu HTTP** akce.

Například pokud používáte Dropbox seznam souborů, bude pravděpodobně přibližně takto v **2014 12 01 náhled** schématu verze definice:

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2014-12-01-preview/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token": {
            "defaultValue": "eyJ0eX...wCn90",
            "type": "String",
            "metadata": {
                "token": {
                    "name": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token"
                }
            }
        }
    },
    "actions": {
        "dropboxconnector": {
            "type": "ApiApp",
            "inputs": {
                "apiVersion": "2015-01-14",
                "host": {
                    "id": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector",
                    "gateway": "https://avdemo.azurewebsites.net"
                },
                "operation": "ListFiles",
                "parameters": {
                    "FolderPath": "/myfolder"
                },
                "authentication": {
                    "type": "Raw",
                    "scheme": "Zumo",
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

Můžete vytvářet ekvivalentní HTTP akci jako pod (parametry část Použití logických operátorů aplikace definice nezmění):

```
{
    "actions": {
        "dropboxconnector": {
            "type": "Http",
            "metadata" : {
              "apiDefinitionUrl" : "https://avdemo.azurewebsites.net/api/service/apidef/dropboxconnector/?api-version=2015-01-14&format=swagger-2.0-standard"  
            },
            "inputs": {
                "uri": "https://avdemo.azurewebsites.net/api/service/invoke/dropboxconnector/ListFiles?api-version=2015-01-14",
                "method" : "POST",
                "body": {
                    "FolderPath": "/myfolder"
                },
                "authentication": {
                    "type": "Raw",
                    "scheme": "Zumo",
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

Rozbor tyto vlastnosti po jednom:

| Vlastnost akce |  Popis |
| --------------- | -----------  |
| `type` | `Http`Namísto`APIapp` |
| `metadata.apiDefinitionUrl` | Pokud chcete použít tuto akci v Návrháři logiku aplikací, je vhodné zahrnout koncový bod metadata. To je vytvořen z:`{api app host.gateway}/api/service/apidef/{last segment of the api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard` |
| `inputs.uri` | To je vytvořen z:`{api app host.gateway}/api/service/invoke/{last segment of the api app host.id}/{api app operation}?api-version=2015-01-14` |
| `inputs.method` | Vždy`POST` |
| `inputs.body` | Shodné s parametry rozhraní api aplikace | 
| `inputs.authentication` | Stejný ověřovací rozhraní api aplikace |

Tento přístup mají práce pro všechny akce rozhraní API aplikace. Prosím pokračujte nezapomeňte, že už nejsou podporované tyto předchozí rozhraní API aplikace a byste měli přesunout do některé ze dvou ostatní možnosti nad (spravované rozhraní API nebo hostingu vlastní rozhraní API webových).

## <a name="2-repeat-renamed-to-foreach"></a>2. opakovat na Foreach

Předchozí verze schématu obdrželi spoustu reakcí zákazníků jsme tuto **opakujte** byla přehlednější a neměli zachytit správně, je skutečně pro každé smyčce. Výsledkem je které jsme mít přejmenovali na **Foreach**. Příklad:

```
{
    "actions": {
        "pingBing": {
            "type": "Http",
            "repeat": "@range(0,2)",
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com/search?q=@{repeatItem()}"
            }
        }
    }
}
```

By nyní uváděný jako:

```
{
    "actions": {
        "pingBing": {
            "type": "Http",
            "foreach": "@range(0,2)",
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com/search?q=@{item()}"
            }
        }
    }
}
```

Dříve funkci `@repeatItem()` byla použita neodkazuje aktuální položce vstupní myší. To byla zjednodušené těsně `@item()`. 

### <a name="referencing-the-outputs-of-the-foreach"></a>Odkazování na výstupy Foreach
A zjednodušit, nebude výstupy **Foreach** akce převázané objekt s názvem **repeatItems**. To znamená, že byly výstupy Nahoře opakovat:

```
{
    "repeatItems": [
        {
            "name": "pingBing",
            "inputs": {
                "uri": "https://www.bing.com/search?q=0",
                "method": "GET"
            },
            "outputs": {
                "headers": { },
                "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
            }
            "status": "Succeeded"
        }
    ]
}
```

Teď je:

```
[
    {
        "name": "pingBing",
        "inputs": {
            "uri": "https://www.bing.com/search?q=0",
            "method": "GET"
        },
        "outputs": {
            "headers": { },
            "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
        }
        "status": "Succeeded"
    }
]
```

Při odkazování na tyto výstupy, přejděte na část akci by musíte udělat:

```
{
    "actions": {
        "secondAction" : {
            "type" : "Http",
            "repeat" : "@outputs('pingBing').repeatItems",
            "inputs" : {
                "method" : "POST",
                "uri" : "http://www.example.com",
                "body" : "@repeatItem().outputs.body"
            }
        }
    }
}
```

Nyní můžete provést místo:

```
{
    "actions": {
        "secondAction" : {
            "type" : "Http",
            "foreach" : "@outputs('pingBing')",
            "inputs" : {
                "method" : "POST",
                "uri" : "http://www.example.com",
                "body" : "@item().outputs.body"
            }
        }
    }
}
```

Tyto změny těchto funkcích `@repeatItem()`, `@repeatBody()` a `@repeatOutputs()` se odeberou.

## <a name="3-native-http-listener"></a>3. nativní posluchače HTTP 
Funkce HTTP posluchače jsou předdefinované teď takže už není potřeba nasadit aplikaci pro rozhraní API posluchače HTTP. Přečtěte si o [Úplné podrobnosti pro vytvoření koncový bod logiky aplikace možné volat tady](app-service-logic-http-endpoint.md). 

Tyto změny funkci `@accessKeys()` odebrán a byla nahrazena `@listCallbackURL()` funkce za účelem získání koncového bodu (když je potřeba). Kromě toho teď musíte definovat alespoň jeden aktivační události v aplikaci logiky nyní. Pokud chcete `/run` pracovní postup, musíte mít jednu z `manual`, `apiConnectionWebhook` nebo `httpWebhook` aktivačních událostí. 

## <a name="4-calling-child-workflows"></a>4. pracovní postupy podřízené volání

Volání podřízených pracovních postupů povinné dříve, přechod na tento pracovní postup, začíná přístupový token a vkládání v definici logiky aplikace, které chcete volat, které podřízené. S novou verzí schématu modul aplikace logiky automaticky vygeneruje přidružení zabezpečení pro podřízený pracovní postup, což znamená, že nemáte žádné tajemství vložte definici za běhu.  Tady je příklad:

```
"mynestedwf" : {
    "type" : "workflow",
    "inputs" : {
        "host" : {
            "id" : "/subscriptions/xxxxyyyyzzz/resourceGroups/rg001/providers/Microsoft.Logic/mywf001",
            "triggerName" : "myendpointtrigger"
        },
        "queries" : {
            "extrafield" : "specialValue"
        },
        "headers" : {
            "x-ms-date" : "@utcnow()",
            "Content-type" : "application/json"
        },
        "body" : {
            "contentFieldOne" : "value100",
            "anotherField" : 10.001
        }
    },
    "conditions" : []
}
```

Druhá zlepšování je že jsme bude mít pojmenování podřízených pracovních postupů úplný přístup k příchozí žádosti o. To znamená, že můžete předáváte parametrů v části *dotazy* a v *záhlaví* objektu a můžete plně definovat celý text.

Nakonec jsou požadované změny podřízeného pracovního postupu. Zatímco před může jen zavoláte podřízený pracovní postup přímo. Teď musíte definovat koncový bod aktivační událost v pracovním postupu pro nadřazenou volání. Obecně to znamená, že budete přidávat aktivační událost z typ **ručně** a pak ji použít v nadřazené definici. Všimněte si, že `host` vlastnost konkrétně má `triggerName`, protože vždy je nutné zadat které aktivační událost jsou vyvolání.

## <a name="other-changes"></a>Další změny

### <a name="new-queries-property"></a>Nová vlastnost dotazů
Všechny typy akcí podporuje nové vstupní s názvem **dotazů**. Může to být na strukturovaného objekt spíše než byste museli sestavování řetězce od ruky.

### <a name="parse-function-renamed"></a>Funkce parse() přejmenovat
Jak můžeme bude brzy přidávat víc typů obsahu, `parse()` funkce přejmenoval na `json()`.

## <a name="coming-soon-enterprise-integration-apis"></a>Brzy k dispozici: podnikový API integrace
V tomto okamžiku v okamžiku co nedělat dosud podařilo verzích rozhraní API integrace Enterprise (například AS2) k dispozici. Tyto se už brzo podle pokynů v [Přehled](http://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/). Mezitím můžete použít svůj stávající nasazené BizTalk rozhraní API prostřednictvím protokolu HTTP akce uvedených výše v "pomocí aplikace už nasazené rozhraní API."
