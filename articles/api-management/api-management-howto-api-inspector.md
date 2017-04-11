<properties 
    pageTitle="Použití rozhraní API inspektor trasování volá správy rozhraní API Azure" 
    description="Zjistěte, jak sledovat hovory pomocí funkce Kontrola rozhraní API správy rozhraní API Azure." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-use-the-api-inspector-to-trace-calls-in-azure-api-management"></a>Jak používat inspektor API pro sledování volá správy API Azure

Rozhraní API Správa poskytuje nástroj Kontrola rozhraní API vám pomůže při ladění a řešení problémů s vaší rozhraní API. Kontrola rozhraní API mohou sloužit programově a lze také přímo na portálu pro vývojáře. 

Kromě trasování operace rozhraní API inspektor také sleduje [výraz zásady](https://msdn.microsoft.com/library/azure/dn910913.aspx) hodnocení. Ukázka, najdete v článku [177 dílu průvodní cloudu: Další funkce správy API](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) a rychlé převíjení vpřed 21:00.

Tato příručka obsahuje walk-through použití rozhraní API Kontrola metadat.

>[AZURE.NOTE] Rozhraní API inspektor trasování jsou pouze vytvářejí a k dispozici pro požadavků, které obsahují klíče předplatné, které patří k účtu [Správce](api-management-howto-create-groups.md) .

## <a name="trace-call"></a> Použití rozhraní API inspektor trasování hovoru

Použít kontrola rozhraní API, přidat **ocp apim sledování: true** požádat o záhlaví k tomuto hovoru operace a pak budou moct stáhnout a zkontrolovat sledování pomocí adresy URL označen záhlaví odpověď **ocp apim sledování umístění** . Tím se teď dá programově a lze rovněž provést přímo na portálu pro vývojáře.

Tento kurz ukazuje, jak pomocí funkce Kontrola rozhraní API pro sledování operace pomocí základní rozhraní API kalkulačky nakonfigurovaný v kurzu [Spravovat první rozhraní API](api-management-get-started.md) dosáhnout toho, aby Začínáme. Pokud jste nedokončili tento kurz trvá jenom chvíli k importu základní rozhraní API kalkulačce nebo můžete použít jiné API rohu vašeho výběru například rozhraní API ozvěn tím. Jednotlivé instance služby správy rozhraní API je předkonfigurovaná pomocí rozhraní API ozvěn tím, které mohou sloužit k vyzkoušet a další informace o správě rozhraní API. Rozhraní API ozvěnu vrátí zpět se neodesílají jakékoli zadání vstupních hodnot. Můžete ho vyvoláte všechny akce HTTP a bude vrácena hodnota jednoduše jste odeslali. 



Začněte tím, klepněte na položku **Vývojář portal** Azure klasické portálu správy API službě. Operace můžete volat přímo na portálu pro vývojáře poskytující pohodlný způsob, jak prohlédněte si a otestujte operace rozhraní API.

>Pokud jste ještě nevytvořili instanci služby správy rozhraní API, v tématu [Vytvoření instanci služby správy rozhraní API][] v kurzu [začít používat správu rozhraní API Azure][] .

![Karta Vývojář v portálu Správa rozhraní API][api-management-developer-portal-menu]

Klikněte na **rozhraní API** z nabídky nejvyšší a klepněte na položku **Základní Kalkulačka**.

![Rozhraní API ozvěn tím][api-management-api]

Klikněte na **vyzkoušet** akci **Přidat dvě celá čísla** .

![Vyzkoušejte si to][api-management-open-console]

Zachovat výchozí hodnoty parametrů a vyberte klíč předplatného pro produkt, který chcete použít z **předplatného klíč** rozevíracího seznamu.

Ve výchozím nastavení v portálu pro vývojáře **Ocp Apim sledování** záhlaví již nastavena na **hodnotu true**. Toto záhlaví nakonfiguruje též generovaného trasování.

![Odeslání][api-management-http-get]

Zaškrtněte políčko **Odeslat** vyvolat operaci.

![Odeslání][api-management-send-results]

V odpovědi záhlaví bude **ocp apim sledování umístění** s hodnotou podobně jako v následujícím příkladu.

    ocp-apim-trace-location : https://contosoltdxw7zagdfsprykd.blob.core.windows.net/apiinspectorcontainer/ZW3e23NsW4wQyS-SHjS0Og2-2?sv=2013-08-15&sr=b&sig=Mgx7cMHsLmVDv%2B%2BSzvg3JR8qGTHoOyIAV7xDsZbF7%2Bk%3D&se=2014-05-04T21%3A00%3A13Z&sp=r&verify_guid=a56a17d83de04fcb8b9766df38514742

Sledování můžete stáhnout ze zadaného umístění a zkontrolována jak je ukázáno v následujícím kroku.

## <a name="inspect-trace"> </a>Zkontrolovat trasování

Chcete-li zobrazit hodnoty v trasování, stáhněte si soubor sledování z adresy URL **ocp apim sledování umístění** . Textový soubor ve formátu JSON a obsahuje položky podobně jako v následujícím příkladu.

    {
        "traceId": "abcd8ea63d134c1fabe6371566c7cbea",
        "traceEntries": {
            "inbound": [
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.2998610Z",
                    "elapsed": "00:00:00.0725926",
                    "data": {
                        "request": {
                            "method": "GET",
                            "url": "https://contoso5.azure-api.net/calc/add?a=51&b=49",
                            "headers": [
                                {
                                    "name": "Ocp-Apim-Subscription-Key",
                                    "value": "5d7c41af64a44a68a2ea46580d271a59"
                                },
                                {
                                    "name": "Connection",
                                    "value": "Keep-Alive"
                                },
                                {
                                    "name": "Host",
                                    "value": "contoso5.azure-api.net"
                                }
                            ]
                        }
                    }
                },
                {
                    "source": "mapper",
                    "timestamp": "2015-06-23T19:51:35.2998610Z",
                    "elapsed": "00:00:00.0726213",
                    "data": {
                        "configuration": {
                            "api": {
                                "from": "/calc",
                                "to": {
                                    "scheme": "http",
                                    "host": "calcapi.cloudapp.net",
                                    "port": 80,
                                    "path": "/api",
                                    "queryString": "",
                                    "query": {},
                                    "isDefaultPort": true
                                }
                            },
                            "operation": {
                                "method": "GET",
                                "uriTemplate": "/add?a={a}&b={b}"
                            },
                            "user": {
                                "id": 1,
                                "groups": [
                                    "Administrators",
                                    "Developers"
                                ]
                            },
                            "product": {
                                "id": 1
                            }
                        }
                    }
                },
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.2998610Z",
                    "elapsed": "00:00:00.0727522",
                    "data": {
                        "message": "Request is being forwarded to the backend service.",
                        "request": {
                            "method": "GET",
                            "url": "http://calcapi.cloudapp.net/api/add?a=51&b=49",
                            "headers": [
                                {
                                    "name": "Ocp-Apim-Subscription-Key",
                                    "value": "5d7c41af64a44a68a2ea46580d271a59"
                                },
                                {
                                    "name": "X-Forwarded-For",
                                    "value": "33.52.215.35"
                                }
                            ]
                        }
                    }
                }
            ],
            "outbound": [
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.4256650Z",
                    "elapsed": "00:00:00.1960601",
                    "data": {
                        "response": {
                            "status": {
                                "code": 200,
                                "reason": "OK"
                            },
                            "headers": [
                                {
                                    "name": "Pragma",
                                    "value": "no-cache"
                                },
                                {
                                    "name": "Content-Length",
                                    "value": "124"
                                },
                                {
                                    "name": "Cache-Control",
                                    "value": "no-cache"
                                },
                                {
                                    "name": "Content-Type",
                                    "value": "application/xml; charset=utf-8"
                                },
                                {
                                    "name": "Date",
                                    "value": "Tue, 23 Jun 2015 19:51:35 GMT"
                                },
                                {
                                    "name": "Expires",
                                    "value": "-1"
                                },
                                {
                                    "name": "Server",
                                    "value": "Microsoft-IIS/8.5"
                                },
                                {
                                    "name": "X-AspNet-Version",
                                    "value": "4.0.30319"
                                },
                                {
                                    "name": "X-Powered-By",
                                    "value": "ASP.NET"
                                }
                            ]
                        }
                    }
                },
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.4256650Z",
                    "elapsed": "00:00:00.1961112",
                    "data": {
                        "message": "Response headers have been sent to the caller. Starting to stream the response body."
                    }
                },
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.4256650Z",
                    "elapsed": "00:00:00.1963155",
                    "data": {
                        "message": "Response body streaming to the caller is complete."
                    }
                }
            ]
        }
    }

## <a name="next-steps"> </a>Další kroky

-   Podívejte se na ukázku trasování zásad výrazů v [177 dílu průvodní cloudu: Další funkce správy API](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/). Převinutí souboru 21:00 podívejte se ukázku.

>[AZURE.VIDEO episode-177-more-api-management-features-with-vlad-vinogradsky]

[Use API Inspector to trace a call]: #trace-call
[Inspect the trace]: #inspect-trace
[Next steps]: #next-steps

[Configure API settings]: api-management-howto-create-apis.md#configure-api-settings
[Responses]: api-management-howto-add-operations.md#responses
[How create and publish a product]: api-management-howto-add-products.md

[Začít používat správu rozhraní API Azure]: api-management-get-started.md
[Vytvořit instanci služby správy rozhraní API]: api-management-get-started.md#create-service-instance
[Azure Classic Portal]: https://manage.windowsazure.com/


[api-management-developer-portal-menu]: ./media/api-management-howto-api-inspector/api-management-developer-portal-menu.png
[api-management-api]: ./media/api-management-howto-api-inspector/api-management-api.png
[api-management-echo-api-get]: ./media/api-management-howto-api-inspector/api-management-echo-api-get.png
[api-management-developer-key]: ./media/api-management-howto-api-inspector/api-management-developer-key.png
[api-management-open-console]: ./media/api-management-howto-api-inspector/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-api-inspector/api-management-http-get.png
[api-management-send-results]: ./media/api-management-howto-api-inspector/api-management-send-results.png




 