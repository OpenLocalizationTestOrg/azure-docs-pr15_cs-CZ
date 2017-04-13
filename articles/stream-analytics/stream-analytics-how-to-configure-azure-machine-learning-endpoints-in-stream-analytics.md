<properties 
    pageTitle="Postup při konfiguraci Azure počítače výukové koncové body v toku analýzy | Microsoft Azure" 
    description="Funkce definované uživatelem strojového jazyka v toku analýzy"
    keywords=""
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"
/>

# <a name="machine-learning-integration-in-stream-analytics"></a>Počítač výukové integrace v toku analýzy

Technologie pro analýzu toku podporuje funkcí definovaných uživatelem, volajících koncové body výukové počítače Azure. Podpora rozhraní REST API pro tuto funkci podrobné v [toku analýzy REST API knihovny](https://msdn.microsoft.com/library/azure/dn835031.aspx). Tento článek obsahuje doplňující informace potřebné pro úspěšné provádění tuto možnost v toku analýzy. Kurz taky odeslala a je dostupná [v tomto poli](stream-analytics-machine-learning-integration-tutorial.md).

## <a name="overview-azure-machine-learning-terminology"></a>Přehled: Azure výukové počítače terminologie

Microsoft Azure počítače Learning poskytuje nástroj pro spolupráci, a přetažením, které můžete použít k vytváření, otestujte a nasazení řešení prediktivní technologie pro analýzu svých dat. Tento nástroj se nazývá *Azure počítače výukové Studio*. Studio slouží k interakci s počítače výukové materiály a snadno vytvářet, otestujte a zapracovávat ji do návrhu. Níže jsou tyto materiály a jejich definice.

- **Pracovní prostor**: *pracovního prostoru* je kontejner obsahující všechny ostatní počítače výukové materiály pro pohromadě v kontejneru správy a ovládacích prvků.
- **Experiment**: *pokusy* jsou vytvářeny ve vědeckých dat využít datové sady a proškolit modelu Výuka počítače.
- **Koncový bod**: *koncové body* jsou Azure počítače výukové objekt použitý k přijmout funkce předávat na vstupu, použít model výuka zadaný počítače a vrátit dosáhne výstupu.
- **Webservice bodování**: *bodování webservice* je sada koncové body výše uvedené.

Každý koncový bod má rozhraní API pro spuštění dávky a synchronní spuštění. Toku analýzy využití synchronní spuštění. Konkrétní službu jmenuje [Služby žádostí a odpovědí](../machine-learning/machine-learning-consume-web-services.md#request-response-service-rrs) v AzureML studio.

## <a name="machine-learning-resources-needed-for-stream-analytics-jobs"></a>Počítač studijní materiály potřebné pro analýzy toku úlohy

Pro účely analýzy toku zpracování úlohy, koncový bod žádostí a odpovědí, [apikey](../machine-learning/machine-learning-connect-to-azure-machine-learning-web-service.md#get-an-azure-machine-learning-authorization-key)a definici swagger jsou všechny potřebné pro úspěšné spuštění. Technologie pro analýzu toku obsahuje další koncový bod, který vytvoří adresu url pro koncový bod swagger, vyhledává v rozhraní a vrátí definici UDF výchozí uživateli.

## <a name="configure-a-stream-analytics-and-machine-learning-udf-via-rest-api"></a>Konfigurace toku technologie pro analýzu a automatické učení UDF prostřednictvím rozhraní REST API

Pomocí rozhraní REST API můžete nakonfigurovat práce volání funkce Azure strojového jazyka. Tyto kroky jsou následujícím způsobem:

1. Vytvoření úlohy toku analýzy
2. Definování vstup
3. Definování výstup
4. Vytvoření funkce definované uživatelem (UDF)
5. Psaní transformaci toku analýzy, která volá souboru UDF
6. Zahájení projektu

## <a name="creating-a-udf-with-basic-properties"></a>Vytváření uživatelem definovanou FUNKCI s základní vlastnostmi

Jako příklad následující kód ukázkové vytvoří skalární UDF s názvem *newudf* spojující koncový bod výukové počítače Azure. Všimněte si, že *koncového bodu* (služba URI) najdete na stránce nápovědy rozhraní API pro službu zvolené a *apiKey* se nachází na hlavní stránce služby.

````
    PUT : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>  
````

Příklad žádost o textu:  

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77fb4b46bf2a30c63c078dca/services/b7be5e40fd194258796fb402c1958eaf/execute ",
                        "apiKey": "replacekeyhere"
                    }
                }
            }
        }
    }
````

## <a name="call-retrievedefaultdefinition-endpoint-for-default-udf"></a>Volání RetrieveDefaultDefinition koncový bod pro výchozí UDF

Jakmile kostra UDF se vytvoří úplnou definici souboru UDF není potřeba. Koncový bod RetreiveDefaultDefinition umožňuje získat výchozí definice skalární funkce, která je vázaný na koncový bod výukové počítače Azure. Následující datové vyžaduje, abyste získat definici UDF výchozí skalární funkce, která je vázaný na koncový bod výukové počítače Azure. Skutečné koncový bod ho není určit, jak je to již poskytnuté během žádost o umístění. Toku analýzy hovorů koncový bod, pokud není výslovně uvedený v pozvánce. V opačném případě použije tu původně odkazuje. Tady UDF trvá jeden řetězec parametr (větu) a vrátí jeden výstup typu řetězec, což znamená "myšlenkou" popisek věta.

````
POST : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>/RetrieveDefaultDefinition?api-version=<apiVersion>
````

Příklad žádost o textu:  

````
    {
        "bindingType": "Microsoft.MachineLearning/WebService",
        "bindingRetrievalProperties": {
            "executeEndpoint": null,
            "udfType": "Scalar"
        }
    }
````

Ukázka výstup tohoto vyhledejte něco mají pod.  

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
````

## <a name="patch-udf-with-the-response"></a>Oprava UDF s odpovědí 

Teď UDF musí opravené předchozí odpovědí, jak je ukázáno v následujícím příkladu.

````
PATCH : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>
````

Žádost o textu (výstup z RetrieveDefaultDefinition):

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
````

## <a name="implement-stream-analytics-transformation-to-call-the-udf"></a>Provádění analýzy toku transformace volání UDF

Nyní dotazu UDF (tady s názvem scoreTweet) pro každé vstupní události a napsat odpověď pro dané události výstup.  

````
    {
        "name": "transformation",
        "properties": {
            "streamingUnits": null,
            "query": "select *,scoreTweet(Tweet) TweetSentiment into blobOutput from blobInput"
        }
    }
````


## <a name="get-help"></a>Získání nápovědy
Další pomoc Vyzkoušejte naše [Fórum komunity analýzy toku Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky

- [Úvod do technologie pro analýzu Azure toku](stream-analytics-introduction.md)
- [Začínáme s používáním analýzy toku Azure](stream-analytics-get-started.md)
- [Změna velikosti úlohy Azure toku analýzy](stream-analytics-scale-jobs.md)
- [Odkaz na Azure toku analýzy dotaz jazyka](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure toku analýzy správy REST API odkaz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
