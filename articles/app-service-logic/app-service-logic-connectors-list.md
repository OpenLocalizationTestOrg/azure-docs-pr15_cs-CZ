<properties
    pageTitle="Seznam dostupných spojnic a rozhraní API aplikace | Aplikace služby Microsoft Azure"
    description="Další informace o spojnic a rozhraní API aplikace služby Azure aplikace"
    services="logic-apps"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="erikre"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="mandia"/>


# <a name="list-of-connectors-and-api-apps-to-use-in-your-logic-apps"></a>Seznam spojnic a rozhraní API aplikace používat v aplikacích logiky
>[AZURE.NOTE] Tuto verzi článku platí pro použití logických operátorů aplikace 2014 12 01 náhled schématu verze. Verze logiku aplikace obecný dostupnosti (GA) najdete v článku [Nový seznam spojnice](../connectors/apis-list.md).

Informace o všech dostupných spojnicemi a rozhraní API aplikace vytvořil Microsoft použití logických operátorů aplikace.

Ceny informace a seznam toho, co je součástí jednotlivých vrstvy služeb, najdete v článku [Ceny služby Azure aplikace](https://azure.microsoft.com/pricing/details/app-service/).

> [AZURE.NOTE] Začínáme s aplikacemi jiných logiky před registrací účet Azure, přejděte na [Akci logiky aplikace](https://tryappservice.azure.com/?appservice=logic). Můžete okamžitě vytvořit aplikaci logiky krátkodobý starter v aplikaci služby. Žádné povinné; kreditní karty žádné závazky.

## <a name="core-connectors"></a>Základní spojnic
Následující tabulka uvádí všechny dostupné spojnic a rozhraní API aplikace vytvořil Microsoft, které jsou k dispozici jako základní konektory:

Jméno | Popis
--- | ---
[Překladač Bing](https://azure.microsoft.com/marketplace/partners/bing/microsofttranslator/) | Přeložení textu v jiném jazyce pomocí služby Bing.
[NASTAVIT INFORMACE HTTP](app-service-logic-connector-http.md) | Nastavit informace HTTP posluchače otevře koncový bod, který slouží jako serveru HTTP a přijímá žádosti o protokolu HTTP nebo HTTPS. Akce HTTP nevyžaduje aplikace rozhraní API a je podporován nativně logiky aplikace.
[Časová rezerva](app-service-logic-connector-slack.md) | Připojení k časová rezerva a odesílání zpráv na časové rezervy kanály.


## <a name="enterprise-integration-connectors"></a>Podnikové integrace spojnic
Následující tabulka uvádí dostupné spojnic a rozhraní API aplikace vytvořené společností Microsoft jako podnikové integrace spojnice k dispozici:

Jméno  | Popis
------------- | -------------
[BizTalk pravidel](app-service-logic-use-biztalk-rules.md) | Pomocí pravidel BizTalk definovat a budete moci určit obchodní logiky v rámci organizace. Obchodní zásady aktualizovat lze bez opětovné kompilace nebo bez znovu nasazení přidružené aplikace
[Analyzátor BizTalk výrazu XPath.](app-service-logic-xpath-extract.md) | Vyhledá hodnotu a extrahuje data z obsahu XML podle XPath zvolíte.
[Spojnice DB2](app-service-logic-connector-db2.md) | Připojí místní databáze IBM DB2 a Azure virtuálního počítače s operačním systémem Windows. Namapovat rozhraní API webových a rozhraní API OData operace Informix Structured Query Language příkazy. <br/><br/>Žádné aktivačních událostí. Akce zahrnout vybrat tabulku, vložení, aktualizace, odstranění a vlastní příkaz<br/><br/>Tato spojnice obsahuje taky Microsoft Client DRDA pro připojení k serveru Informix přes síť TCP/IP.
[Soubor](app-service-logic-connector-file.md) | Pomocí této Connectoru, můžete se připojit k systému souborů místní nebo sítě a dokončení jiný soubor úkolů, včetně odesílání, odstraňování, seznam souborů a další.
[Zprostředkovatel Informix](app-service-logic-connector-informix.md) | Připojení k databázi IBM Informix místní a na Azure virtuálního počítače s operačním systémem Windows. Namapovat rozhraní API webových a rozhraní API OData operace Informix Structured Query Language příkazy.<br/><br/>Žádné aktivačních událostí. Akce zahrnout tabulky vyberte Vložit, aktualizovat, odstranit a vlastní příkaz.<br/><br/>Pokud chcete použít místní, virtuální privátní sítě nebo Azure ExpressRoute lze. Tato spojnice obsahuje taky Microsoft Client pro DRDA pro připojení k serveru Informix TCP/IP sítí.
[Microsoft SQL Server](app-service-logic-connector-sql.md) | Připojení k místní databázi Azure SQL SQL Server nebo. Můžete vytvářet, aktualizovat, získat a odstraňovat položky z tabulky databáze SQL.
MQ | Připojí k IBM WebSphere MQ serveru verzi 8 místní a na Azure virtuálního počítače s operačním systémem Windows. Pokud chcete použít místní, virtuální privátní sítě nebo Azure ExpressRoute lze. Spojnice obsahuje taky Microsoft Client MQ.<br/><br/>Žádné aktivačních událostí. Žádné akce.<br/><br/>**Poznámka:** Momentálně není možné použít s aplikacemi jiných použití logických operátorů.

## <a name="connectors-as-triggers"></a>Spojnice jako aktivačních událostí
Několik spojnic poskytují aktivační události pro použití logických operátorů aplikace. Tyto aktivační události existují dva typy:

1. Aktivace hlasování: Tyto aktivační události hlasování služby zadaný frekvencí ke kontrole pro nová data. Při nová data k dispozici, spustí novou instanci aplikace logika s daty předávat na vstupu. Zabránit spotřebované množství jsou stejná data několikrát, aktivační událost může čištění dat, která byla číst a předávají funkcím aplikace použití logických operátorů. Tyto spojnic příklady soubor SQL a úložišti Azure.
2. Aktivace nabízených: Tyto aktivační události přehrávat data na koncový bod nebo zvláštní události dojít. Potom spustí novou instanci aplikace použití logických operátorů. Tyto spojnic příklady posluchače HTTP a Twitter.

## <a name="connectors-as-actions"></a>Spojnice jako akce
Spojnice lze také jako akce v rámci logiky aplikace. Akce jsou vhodné k vyhledání dat v rámci logiky aplikaci, můžete pak použít při provádění. Potřebujete příklad vyhledávání dat z databáze SQL Další informace o zákazníkovi po zpracování objednávky. Nebo můžete psát, aktualizovat a odstraňovat dat v cílové umístění. Lze provést pomocí akce poskytovanou spojnice. Akce namapujte operace v aplikacích pro rozhraní API (podle jejich Swagger metadata).

## <a name="create-your-own-connectors-and-api-apps"></a>Vytvoření vlastní spojnice a rozhraní API aplikace
[Spojnice a rozhraní API aplikace odkaz](http://aka.ms/appservicesconnectorreference)  
[Aktivace aplikací Azure rozhraní API aplikace služby](../app-service-api/app-service-api-dotnet-triggers.md)  
[Odkaz logiku aplikace](https://msdn.microsoft.com/library/azure/dn948510.aspx)

## <a name="more-on-connectors-and-api-apps"></a>Další informace o spojnic a rozhraní API aplikace
[Co je spojnic a BizTalk rozhraní API aplikace](app-service-logic-what-are-biztalk-api-apps.md)  
[Pomocí Connection Manager hybridní Azure aplikace služby](app-service-logic-hybrid-connection-manager.md)  
[Správa a sledování předdefinované rozhraní API aplikace a spojnic](app-service-logic-monitor-your-connectors.md)
