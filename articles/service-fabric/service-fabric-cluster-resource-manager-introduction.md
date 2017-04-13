<properties
   pageTitle="Úvodní informace o správce služby struktury clusteru | Microsoft Azure"
   description="Úvod do služby struktury clusteru správce prostředků."
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# <a name="introducing-the-service-fabric-cluster-resource-manager"></a>Úvodní informace o správce služby struktury clusteru zdroje
Obvykle Správa IT systémy nebo sada služeb mysleli začíná několik fyzické nebo virtuálních počítačích snaží o těchto konkrétní službami nebo systémy. Počet hlavních služeb byly rozdělené podle osy "web" a "data" nebo "úložiště" osy, možná s několika dalšími specializované komponentami jako mezipaměti. Další typy aplikací budou mít zpráv vrstvu kde žádosti o naplněn nebo zmenšit, připojení k osy práce pro účely analýzy nebo transformace potřebné jako součást messaging. Každý typ pracovního vytížení máte konkrétní počítačích snaží ho: máte pár počítačích snaží o, webových serverů a málo databázi. Pokud určitý typ pracovního vytížení strojů byl na spuštění příliš žádanou a potom jste přidali více počítačích s tímto typem pracovního vytížení nakonfigurovaný na spuštění nebo několik strojů nahrazeny větší počítačích. Snadno. Pokud počítači selže, spustili tuhle část celkové aplikace na dolním kapacity dokud počítači může obnovit. Pořád poměrně snadná (je-li nutně zábavnou).

Nyní však Pojďme přivítejte nalezení třeba rozšiřování a udělali kontejnerů nebo silný pokles microservice. Neočekávaně zjistíte, desítky, stovky nebo tisíce počítačích, desítky různé typy služeb, případně stovky různých instancích těchto služeb, oba objekty mají jeden nebo víc instancí nebo repliky pro vysokou dostupnosti (HA).

Neočekávaně Správa prostředí není tak jednoduchá správa několika počítačích snaží jednoho typy úloh. Servery jsou virtuální a už mají názvy (budete *mít* po přechodu na jiný mindsets z [domácích zvířat](http://www.slideshare.net/randybias/architectures-for-open-and-scalable-clouds/20) nakonec). Konfigurace je menší o strojů a informace o službách sami. Vyhrazené hardwarové je věcí minulosti a služby stali malé distribuované systémy zahrnující více menší použitelné části komoditou hardwaru.

V důsledku rozdělení dřív monolitický a vrstvené aplikace do samostatných služeb spuštěných na hardware komoditou, teď máte mnoho dalších kombinací řešení. Kdo rozhoduje o jaké druhy pracovního vytížení mohlo by umožnit spuštění o hardwaru, nebo kolik? Které pracovního vytížení fungují dobře na stejný hardware a které konfliktu? Počítači při přechodu... co ještě spuštěná tam? Kdo je zodpovědní za a ujistěte se, že pracovní zátěž spuštění znovu? Čekáte (virtuální?) počítač bude doplněný zpět nebo svého pracovního vytížení automaticky převzít jiných počítačů a svázat s? Je nutný lidské zásah? Co dávat pozor upgrady v tomto seřadit prostředí?

Jako vývojáři a operátory žijí v tomto seřadit světě chceme třeba trochu pomoct Správa tento složitost a najděte smysl přijetí binge a pokusíte papír přes složitost s lidmi nejsou správné odpovědi.

Co dělat?

## <a name="introducing-orchestrators"></a>Úvodní informace o orchestrators
"Orchestrator" je obecný termín konkrétní software, který pomáhá správcům spravovat tyto typy prostředí. Orchestrators jsou součástí, které přebírají v žádosti o jako "Můžu libovolný text 5 kopií tuto službu Moje prostředím", zkontrolujte PRAVDA a potom (pokusíte) ponechte to tak.

Orchestrators (ne člověka) se, co kyvu do akce dojde k selhání do počítače nebo z nějakého důvodu neočekávané Ukončí úlohu. Většina Orchestrators větší kontrolu nad zacházet s nepovede, například pomáhá s novou nasazení, zpracování upgrady a práce s využití prostředků, ale jsou všechny zásadně o zachování že některé požadovaný stav konfigurace v prostředí. Chcete zjistit Orchestrator má a mít ji proveďte lifting těžké. Chronos Marathon nad Mesos loďstva, roj, Kubernetes a struktury služby všechny příkladů Orchestrators (nebo jejich součástí). Informace jsou vytvářeny vždy jako složitosti Správa nasazení reálné v různých typů prostředí a podmínky zvětšovat a měnit.

## <a name="orchestration-as-a-service"></a>Průběhu jako služba
Úlohy Orchestrator v rámci služby struktury clusteru má na starosti primárně správcem zdrojů obrázku. Správce prostředků služby struktury clusteru je jedním z těchto služeb systému v rámci služby struktury a se automaticky spustí v rámci každé clusteru.  Správce prostředků clusteru úlohy se obecně rozdělit na tři části:

1. Vynucení pravidel
2. Optimalizace prostředí
3. Pomoc při jinými postupy

### <a name="what-it-isnt"></a>Co ne
V tradiční N osy webové aplikace se vždy některé pojem "Při vyrovnávání zatížení", obvykle označovaný taky jako Vyrovnávání zatížení sítě (vyrovnávání) nebo aplikaci zatížení vyrovnávání (ALB) podle toho, kdy byla ve vrstvě sociální sítě. Některé Vyrovnávání zatížení hardwaru na základě jako nabízení BigIP F5 na ostatní podléhají, softwaru na základě například společnosti Microsoft Vyrovnávání zatížení sítě. V jiných prostředích nějak HAProxy může zobrazit tato role. V těchto architektury je úkol Vyrovnávání zatížení a ujistěte se, že všech počítačů různých příslušnosti front-end nebo různé stroje clusteru dostanou (přibližně) stejné množství práce. Strategie pro tuto měnit, odesílání různých volání na jiný server relace Připnutí/lepivost, k skutečné odhad a volání přiřazení na základě jeho odhadnutou hodnotu nákladů a aktuální zatížení počítače.

Všimněte si, že to byl nejlépe mechanismus zajišťující webová vrstva zůstaly zhruba rovnoměrně. Strategie pro vyrovnávání osy dat byly úplně rozdílnými a závislé na mechanismus ukládání dat, obvykle zarovnání na střed kolem sharding dat, ukládání do mezipaměti, zobrazení Spravovat databáze a uložené procedury, atd.

Době, kdy je některé z těchto strategií zajímavé, Správce služeb struktury clusteru zdroje není nic jako Vyrovnávání zatížení sítě nebo mezipaměti. Během síť služby Vyrovnávání zatížení zajistí přední konce jsou vyrovnání přesunutím přenosy na které služby používáte, Správce služeb clusteru struktury zdrojů, která bude úplně různé strategie – zásadně, struktury služby přesune *služby* na místo, kam se smysl nejvíce (a očekává přenosy nebo k načtení ke sledování). To může být, například uzly, které jsou aktuálně studenou, protože služeb, které jsou nejsou děláte teď před sebou hodně práce nebo které jste odstranili nebo přesunuli kdekoli jinde. Jiný příklad může správce prostředků clusteru také posouváním službu od počítače, která se chystá upgradovat nebo které přetížený kvůli zásobníku spotřeby služby, které byly spuštěna. Protože správce prostředků clusteru je zodpovědný za přesunutí služby kolem (ne poskytly v síti které služby jsou už uložené), obsahuje sady významně neliší funkce ve srovnání s co byste měli najít Vyrovnávání zatížení sítě a využívá zásadně různé strategie pro zajištění, aby se dobře používají prostředcích clusteru.

## <a name="next-steps"></a>Další kroky
- Další informace o toku architektura a informace v rámci obrázku správce podívejte se na [Tento článek](service-fabric-cluster-resource-manager-architecture.md)
- Správce prostředků clusteru má spoustu možností pro popis clusteru. Zjistit další informace o jejich najdete v tématech tohoto článku na [popisující služby struktury obrázku](service-fabric-cluster-resource-manager-cluster-description.md)
- Pro další informace o dalších možností konfigurace služeb podívejte se na téma na ostatní clusteru Správce konfigurace k dispozici [Další informace o konfiguraci služby](service-fabric-cluster-resource-manager-configure-services.md)
- Metriky jsou, jak správce služby struktury clusteru prostředků spravuje spotřeby a kapacita clusteru. Přečtěte si další informace o jejich a jak je nastavit najdete v tématech [v tomto článku](service-fabric-cluster-resource-manager-metrics.md)
- Správce prostředků clusteru funguje s možnostmi správy struktury služby. [Pokud chcete zjistit další informace o této integrace, v tomto](service-fabric-cluster-resource-manager-management-integration.md) článku
- Najdete informace o tom správce prostředků clusteru spravuje a zůstatky náklad v clusteru podívejte se na článek o [Vyrovnávání zatížení](service-fabric-cluster-resource-manager-balancing.md)
