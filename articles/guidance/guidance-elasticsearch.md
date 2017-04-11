
<properties
   pageTitle="Elasticsearch Azure podle pokynů | Microsoft Azure"
   description="Elasticsearch Azure podle pokynů."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="bennage"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="masashin"/>

# <a name="elasticsearch-on-azure-guidance"></a>Elasticsearch Azure podle pokynů 

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Elasticsearch je velmi scalable otevřít zdroj vyhledávání a databáze. Je vhodné pro situace, které vyžadují rychlé analýzy a vyhledání informací v velký datové sady. Tento návod pohledů některé klíčové s navrhováním Elasticsearch clusteru, s fokusem na způsoby, jak optimalizovat a vyzkoušet konfiguraci.

> [AZURE.NOTE]Tento návod předpokládá některé základní znalost Elasticsearch. Podrobnější informace navštivte [Web Elasticsearch](https://www.elastic.co/products/elasticsearch). 

- **[Spuštění Elasticsearch na Azure][]** obsahuje stručný úvod do obecnou strukturu Elasticsearch a popisuje, jak implementovat Elasticsearch clusteru pomocí Azure. 
- **[Optimalizace výkonu požití dat pro Elasticsearch na Azure][]** nasazení clusteru Elasticsearch obsahuje popis a hloubkovou analýzu různé možnosti konfigurace zvážit, když je očekávaná vysoká rychlost požití data.
- **[Agregace dat optimalizace a výkonu dotazu pro Elasticsearch na Azure][]** poskytuje hloubkovou analýzu možnosti byste měli zvážit při rozhodování, jak optimalizovat systému výkon dotazů a vyhledávání.
- **[Konfigurace odolnost a obnovení na Elasticsearch na Azure][]** popisuje některé důležité funkce Elasticsearch obrázku, který můžete minimalizovat riziko ztráty dat a časů obnovení rozšířeného datového.
- **[Vytvoření výkonu testování prostředí pro Elasticsearch na Azure][]** popisuje, jak nastavit prostředí pro účely testování požití dat výkonu a úloh dotazu v Elasticsearch obrázku. 
- **[Provádění JMeter testování plán Elasticsearch][]** shrnuje pracovního testy výkonu implementovaná pomocí JMeter testovací plán společně s kódu jazyka Java začleněná jako JUnit test k provádění úloh, jako je odeslání dat do clusteru Elasticsearch.
- **[Nasazení JMeter JUnit vzorkování testování Elasticsearch výkonu][]** popisuje, jak vytvářet a používat JUnit vzorník, které můžete generovat a odeslání dat do Elasticsearch obrázku. To obsahuje vysoce flexibilní přístup k načtení testování, které můžete generovat velké objemy dat test bez v závislosti na externí datové soubory. 
- **[Spuštění automatické testů odolnost proti chybám Elasticsearch][]** shrnuje k tomu použitých v této řadě zkoušek odolnost proti chybám. 
- **[Spuštění automatické testů výkonu Elasticsearch][]** shrnuje, jak spustit testy výkonu použít v této řadě.


[Spuštění Elasticsearch na Azure]: guidance-elasticsearch-running-on-azure.md
[Ladění výkonu požití dat pro Elasticsearch na Azure]: guidance-elasticsearch-tuning-data-ingestion-performance.md
[Vytváření výkonu testování prostředí pro Elasticsearch na Azure]: guidance-elasticsearch-creating-performance-testing-environment.md
[Provádění JMeter testovací plán pro Elasticsearch]: guidance-elasticsearch-implementing-jmeter-test-plan.md
[Nasazení JMeter JUnit vzorkování testování Elasticsearch výkonu]: guidance-elasticsearch-deploying-jmeter-junit-sampler.md
[Optimalizace slučování dat a výkonu dotazu pro Elasticsearch na Azure]: guidance-elasticsearch-tuning-data-aggregation-and-query-performance.md
[Konfigurace odolnost a obnovení na Elasticsearch na Azure]: guidance-elasticsearch-configuring-resilience-and-recovery.md
[Spuštění testů odolnost proti chybám automatické Elasticsearch]: guidance-elasticsearch-running-automated-resilience-tests.md
[Spuštění testů automatické Elasticsearch výkonu]: guidance-elasticsearch-running-automated-performance-tests.md
