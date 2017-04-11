<properties
   pageTitle="Přehled funkcí Azure | Microsoft Azure"
   description="Pochopte, jak používat funkce Azure optimalizovat asynchronní zatížení v minutách."
   services="functions"
   documentationCenter="na"
   authors="mattchenderson"
   manager="erikre"
   editor=""
   tags=""
   keywords="Azure funkcí, funkce, zpracování události, webhooks, dynamické výpočetním, bez serveru architektura"/>

<tags
   ms.service="functions"
   ms.devlang="multiple"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/29/2016"
   ms.author="cfowler;mahender;glenga"/>
   
   
# <a name="azure-functions-overview"></a>Přehled funkcí Azure

Azure funkce je řešení pro snadné spuštění malých částech kód nebo "funkcí, mezi které" v cloudu. Můžete napsat jenom kód potřebný pro problém vždy po ruce, bez obav celou aplikaci nebo infrastruktury ho spusťte. To může být vývoj i produktivnější a používáte jazyka vývoj podle výběru, například C#, F #, Node.js, Python nebo PHP. Zaplatit pouze spuštění kódu a zabezpečení Azure zobrazit podle potřeby.

Toto téma obsahuje podrobný přehled funkcí Azure. Pokud chcete přejít přímo na a začít pracovat s funkcemi Azure, začněte s [vytvořit svůj první funkce Azure](functions-create-first-azure-function.md). Pokud hledáte další technické informace o funkcích, podívejte se na [referenční informace pro vývojáře](functions-reference.md).

## <a name="features"></a>Funkce

Tady jsou některé klíčové funkce Azure funkcí:
    
* **Volba jazyka** – funkce zápisu pomocí C#, F #, Node.js, Python, PHP, dávku, flám, Java nebo libovolnou spustitelný soubor.
* **Mzdy za použití ceny modelu** - platu pouze pro čas strávený spuštěním kódu. Podívejte se na volbu plánování dynamické aplikace služby v [ceny oddíl](#pricing) pod.  
* **Přenést vlastní závislosti** – funkce podporuje NuGet a NPM, abyste mohli používat knihoven Oblíbené.  
* **Integrované zabezpečení** – ochrana HTTP spouštěný funkcí s jinými poskytovateli služeb OAuth například Azure Active Directory, Facebook, Google, Twitteru nebo Account Microsoft.  
* **Integrace zjednodušené** - využít služby Azure a software – jako-(SaaS) nabízené služby. Naleznete v [části Integrace](#integrations) pod pár příkladů.  
* **Flexibilní vývojové** - kódu funkce přímo na portálu nebo nastavit nepřetržitý integraci a nasazení kódu prostřednictvím GitHub Visual Studio týmovou a jiných [podporované vývojového nástroje](../app-service-web/web-sites-deploy.md#deploy-using-an-ide).  
* **Otevřít zdroj** – funkcí runtime je otevřít zdroj a [v GitHub k dispozici](https://github.com/azure/azure-webjobs-sdk-script).  

## <a name="what-can-i-do-with-functions"></a>Co mám dělat s funkcemi?

Azure funkce je skvělý řešení pro zpracování dat, integrace systémy, práci s internet z – například (IoT) a konečně sestavení jednoduché rozhraní API a microservices. Zvažte funkce úlohami, například obrázek nebo pořadí zpracování, soubor údržby, dlouho probíhajících úkoly, které chcete spustit podproces pozadí nebo úkoly, které chcete spustit podle plánu. 

Funkce poskytuje šablony, které vám usnadní zahájení práce s uvedené hlavní příklady situací, včetně následujících:

* **BlobTrigger** - proces Azure úložiště objektů BLOB při přidání do kontejnery. Je možné využít pro změnu velikosti obrázku.
* **EventHubTrigger** - reagovat na události Doručená k rozbočovači Azure události. Užitečné v přístrojového vybavení aplikace, uživatelské prostředí nebo pracovního postupu zpracování a scénáře Internet věcí (IoT).
* **Obecný webhook** - proces webhook HTTP žádosti o služby, který podporuje webhooks.
* **GitHub webhook** - reagovat na události probíhajících v GitHub úložištích. Příklad najdete v článku [Vytvoření webhook nebo rozhraní API (funkce)](functions-create-a-web-hook-or-api-function.md).
* **HTTPTrigger** - aktivační událost provádění kódu pomocí žádost HTTP.
* **QueueTrigger** - odpovědět na zprávy doručenými ve frontě Azure úložiště. Příklad naleznete v tématu [Vytvoření funkci Azure, která se váže služby Azure](functions-create-an-azure-connected-function.md).
* **ServiceBusQueueTrigger** - připojit kódu pro další služby Azure nebo místní služby tak, že poslech fronty zpráv. 
* **ServiceBusTopicTrigger** - připojit kódu pro další služby Azure nebo místní služby pomocí odběru na témata. 
* **TimerTrigger** - spustit vyčištění nebo jinými úkoly dávku předdefinované plánu. Příklad najdete v článku [Vytvoření události zpracování (funkce)](functions-create-an-event-processing-function.md).

Azure funkce podporuje *aktivačních událostí*, které jsou způsoby, jak začít provádění kódu a *vazby*, které umožňují zjednodušení kódování pro vstupní a výstupní data. Podrobný popis aktivačními událostmi a vazby, které poskytuje funkce Azure najdete v článku [funkce Azure aktivačními událostmi a referenční informace pro vývojáře vazby](functions-triggers-bindings.md).


## <a name="integrations"></a>Integrace

Azure funkce je integrována různých Azure a služby 3rd výrobců. Můžete tyto aktivace funkce a spuštění nebo sloužit jako vstupní a výstupní kódu. Funkce Azure jsou podporovány následující integrace služby. 

* Azure DocumentDB
* Rozbočovače Azure události 
* Azure aplikace Mobile (tabulky)
* Rozbočovače Azure oznámení
* Azure Bus služby (fronty a témata)
* Azure úložiště (objektů blob dotazů a tabulek) 
* GitHub (webhooks)
* Místní (pomocí služby Bus)

## <a name="pricing"></a>Jaká funkce náklady?

Azure funkce má dva typy ceny plány, zvolte to, které nejlépe odpovídá vašim potřebám: 

* **Dynamické hostingu plán** – při spuštění funkce Azure poskytuje všechny potřebné systémové prostředky. Nemusíte bát, řízení zdrojů a pouze zaplatit údaj o čase, spuštění kódu. Úplné podrobnosti o cenách jsou dostupné na [funkce ceny stránky](/pricing/details/functions). 

* **Plán služeb aplikací** – spuštění funkce stejně jako WWW, mobilní telefon a rozhraní API aplikace. Pokud už používáte aplikaci služby pro ostatní aplikace, můžete použít funkce ve stejném plánu bezplatně. Podrobnosti najdete v [aplikaci služby ceny stránky](/pricing/details/app-service/).

Další informace o měřítka funkce zjistěte, [jak zobrazit Azure funkcí](functions-scale.md).

##<a name="next-steps"></a>Další kroky

+ [Vytvoření první funkce Azure](functions-create-first-azure-function.md)  
Rovnou a vytvořte svůj první funkce pomocí funkce Azure rychlý úvod. 
+ [Referenční informace pro vývojáře Azure funkcí](functions-reference.md)  
Další technické informace o runtime modul Azure funkcí a odkaz obsahuje pro definování aktivačních událostí a vazby a kódování funkcí.
+ [Testování funkcí Azure](functions-test-a-function.md)  
Popisuje různé nástroje a postupy pro účely testování funkce.
+ [Jak zobrazit Azure funkcí](functions-scale.md)  
Tento článek popisuje služby plány dostupných funkcí Azure včetně plán dynamické služeb a výběr správné plán. 
+ [Další informace o aplikaci služby Azure](../app-service/app-service-value-prop-what-is.md)  
Azure funkce využívá platformu Azure aplikaci služby základní funkce jako nasazení, proměnné a diagnostických nástrojů. 