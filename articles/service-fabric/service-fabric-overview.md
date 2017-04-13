<properties
   pageTitle="Přehled služeb struktury | Microsoft Azure"
   description="Přehled služeb struktury, které aplikace se skládají z mnoha microservices poskytovat měřítko a odolnost. Služba struktury je distribuované systémy platformy slouží k vytvoření scalable, spolehlivost a snadno spravuje žádosti o cloudu"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor="masnider"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/22/2016"
   ms.author="mfussell"/>

# <a name="overview-of-service-fabric"></a>Přehled služeb struktury
Služba struktury je platformu distribuované systémy, která usnadňuje balíček, nasazení a správu scalable a spolehlivé microservices. Služba struktury také adresy významné výzvy ve vývoji a Správa aplikací cloudu. Vývojáři a správci se můžete vyhnout řešení složité infrastruktury problémy a fokus místo toho na provádění úloh velice důležité, náročné víte, že pocházejí scalable spolehlivý a spravovat. Služba struktury představuje platformu další generování middleware pro vytváření a správu tyto podnikových, úrovně 1 cloudu měřítko aplikace.

Toto [krátké video](https://aka.ms/servicefabricvideo) poskytuje úvod do služby struktury a microservices.


## <a name="applications-composed-of-microservices"></a>Aplikace skládající se ze microservices
Služba struktury umožňuje vytvářet a spravovat aplikace scalable a spolehlivé skládající se ze microservices spuštění v velmi vysoké hustoty na sdíleném fondu strojů (jako takzvaný clusteru). Poskytuje sofistikované runtime pro vytváření distribuované, scalable příslušnosti a stavové microservices. Také poskytuje možnosti správy komplexní aplikace pro vytváření nasazení, sledování, upgrade nebo oprava a odstraňování aplikací nasazena.

Proč záleží microservices přístup? Jsou dva hlavní důvody:

1. Umožňují zobrazit různé části aplikace podle svých potřeb.

2. Členům týmu vývoje můžou být více aktivní v zavádění změny a tedy poskytovat funkce vaši zákazníci rychleji a častěji.

Služba struktury zajišťuje mnoho služby dnes, včetně databáze SQL Azure, Azure DocumentDB, Cortanu, Power BI, Microsoft Intune, Azure události rozbočovače, Azure IoT, Skype pro firmy a mnoho Azure služby základní.

Služba struktury přizpůsobené pro vytváření cloudové nativní služby, které můžete začít malá, podle potřeby a postupně se zvětšují rozsáhlé měřítko s stovky nebo tisíce počítačích.

Služeb Internetu měřítko aktuálnímu jsou vytvářeny ze microservices. Microservices příklady bran Protocol (protokol), profily nákupní košík, odlišným způsobem, stavu skladových zásob zpracování dotazů a ukládání do mezipaměti. Služba struktury je microservices platformu, která poskytuje každé microservice jedinečný název, který může být příslušnosti nebo stavové.

Služby struktury obsahuje komplexní runtime životního cyklu funkce správy a aplikacím skládající se ze tyto microservices. Je hostitelem microservices uvnitř kontejnery nasazeném a aktivovali přes clusteru struktury služby. Přesunutí z VMs kontejnery něco v ní možné zvýšení pořadí velikosti hustoty. Jiné pořadí podle velikosti hustoty bude podobně možné přesunout z kontejnerů microservices. Například jednoho obrázku databáze SQL Azure zahrnuje stovky počítače se systémem desítky tisíce kontejnery hostingu celkových stovky tisíce databází. Každou databázi je microservice stavová služba struktury. Platí jiných služeb výše zmíněné, což je proč termín "mnoha" slouží k popisu služby struktury možnosti. Pokud kontejnery věnuje vysoké hustoty, pak microservices umožňují mnoha.

Další informace o přístupu microservices číst [Proč microservices přístupu k vytváření aplikací?](service-fabric-overview-microservices.md)

## <a name="container-deployment-and-orchestration"></a>Kontejner nasazení a průběhu
Služba struktury je [orchestrator](service-fabric-cluster-resource-manager-introduction.md) microservices napříč obrázku z počítače. Microservices můžete vyvinuté mnoha různými způsoby pomocí [programovací modely služby struktury](service-fabric-choose-framework.md) nasazením [spustitelných Host](service-fabric-deploy-existing-app.md). Služba struktury můžete nasadit služby v kontejneru obrázky a hlavně můžete kombinovat služby v procesy a služby v kontejnerech společně ve stejné aplikaci. Pokud chcete jenom [nasazením a správou kontejneru obrázky](service-fabric-containers-overview.md) přes clusteru strojů, struktury služby je ideální volba pro tuto.


## <a name="create-service-fabric-clusters-anywhere"></a>Vytvoření struktury služby clusterů kdekoli
Služba struktury clusterů můžete vytvořit v mnoha prostředích, včetně Azure nebo místní, Windows Server nebo Linux. Kromě toho je stejná jako provozním prostředí s žádné emulátorů souvisejících vývojové prostředí v SDK. Jinými slovy Pokud běží na svůj cluster místní rozvoj ho nasadí do stejného obrázku v jiných prostředích.

Další informace o vytváření clusterů místním v článku [Vytvoření obrázku ve Windows Server a v Linuxu](service-fabric-deploy-anywhere.md) nebo Azure vytváření obrázku [prostřednictvím portálu Azure](service-fabric-cluster-creation-via-portal.md).

![Služba struktury platformy][Image1]

## <a name="stateless-and-stateful-service-fabric-microservices"></a>Příslušnosti a stavové microservices struktury služby

Služba struktury umožňuje vytvářet aplikace tvořené microservices. Příslušnosti microservices (bran Protocol (protokol), proxy servery web atd.) nepracují proměnlivých stavu mimo dané žádost a její odpověď ze služby. Azure role pracovníka Cloudovým službám jsou příkladem příslušnosti služby. Stavová microservices (uživatelských účtů, databází, zařízení nákupní košík, odlišným způsobem, fronty, atd.) udržovat proměnlivých autoritativní stavu za žádost a její odpověď. Aplikace Internet měřítko aktuálnímu se skládají z kombinací příslušnosti a stavové microservices.

Proč mít stavové microservices spolu s příslušnosti z nich? Jsou dva hlavní důvody:

1. Možnost vytvoření výkon maximum, minimum latence, proti chybám online zpracování transakcí (OLTP) služby přijetím kód a zavřete data ve stejném počítači. Příklady jsou interaktivní obchodní poutače vyhledávání, systémů Internet věcí (IoT), obchodní systémy, platební kartou zpracování a podvod zjišťování systémů a Správa osobních záznamů.

2. Zjednodušení návrhu aplikace. Stavová microservices odebrat nutnosti další fronty a ukládání do mezipaměti, obvykle musí zahrnovat dostupnost a latence požadavků na čistě příslušnosti aplikace. Stavová služba se mají přirozený vysoké dostupnosti nízkou latencí, snižte počet proměnné části ke správě v aplikaci jako celek.

Další informace o vzorků aplikace pomocí služby struktury číst [scénáře aplikací](service-fabric-application-scenarios.md) a [kliknutím na programovací rámec modelu](service-fabric-choose-framework.md) službě

## <a name="application-lifecycle-management"></a>Správa životního cyklu aplikací
Služba struktury poskytuje prvotřídní podporu pro Správa životního cyklu úplné verze aplikace (ALM) cloudu aplikací – od vývoje přes nasazení, každodenní správu a údržba případné vyřadit z.

Možnosti služby struktury ALM umožňují správci aplikací / IT operátory použít jednoduchý a nízké dotykové pracovních postupů k poskytování, instalace opravy a sledování aplikací. Tyto předdefinované pracovní postupy značně snížit zatížení IT operátory zachovat aplikace průběžně dostupné.

Většinou aplikací tvořen kombinací příslušnosti a stavové microservices a jiných spustitelných/runtimes nasazený společně. Tím, že silných typy na daných aplikací a sbalený microservices, umožňuje služby struktury nasazení více instancí aplikace. Jednotlivé instance je spravovaných a upgradovat nezávisle na sobě. Je důležité struktury služby je možné nasazení *všechny* spustitelných nebo runtime a jejich spolehlivé. Například služba struktury nasadí ASP.NET Core 1, Node.js, Java VMs, skripty nebo něco jiného, které tvoří aplikace.

Další informace o Správa životního cyklu aplikací přečtěte si [životního cyklu aplikace](service-fabric-application-lifecycle.md) a o nasazení jakýkoli kód, najdete v článku [nasazení spustitelný Host](service-fabric-deploy-existing-app.md)

## <a name="key-capabilities"></a>Klíčové funkce
Pomocí služby struktury, máte tyto možnosti:

- Můžete vyvíjet datových scalable aplikace, které jsou vlastním retušovacího.

- Můžete vyvíjet aplikace skládající se ze microservices pomocí programového modelu služby struktury. Nebo jednoduše spustitelných hosta Host (hostitel) a jiných aplikací rámce podle svého výběru, například ASP.NET Core 1 nebo Node.js.

- Můžete vyvíjet vysoce spolehlivé příslušnosti a stavové microservices.

- Nasazení a organizovat kontejnery jsou kontejnery Windows a kontejnery Docker přes clusteru. Tyto kontejnery můžete kontejneru hosta spustitelných nebo spolehlivé příslušnosti a stavové microservices.  V obou případech získání kontejneru portu hostitele port mapování, kontejneru vyhledávání a automatické převzetí.

- Zjednodušte návrhu aplikace pomocí stavové microservices místo mezipaměti a dotazů.

- Nasazení Azure nebo mračnech místním systémem Windows Server nebo Linux změnami nula kód. Zápis a všechny Service struktury clusteru nasazení kdekoli.

- Můžete vyvíjet s přístupem "datacentru v počítači". Místní vývojové prostředí je stejný kód, který běží v Azure datacentrech.

- Nasazení aplikace v sekundách.

- Nasazení aplikace v hustoty vyšší než virtuálních počítačích nasazení stovky nebo tisíce aplikací jednotlivé počítače.

- Nasazení různými verzemi stejného aplikací vedle sebe, každý nezávisle na sobě inovovat.

- Správa životního cyklu stavové aplikací bez jakékoli prostoje, včetně inovace přerušení a pevné.

- Správa aplikací pomocí rozhraní API .NET Java (Linux), Powershellu, Azure rozhraní příkazového řádku (Linux) a rozhraní REST.

- Upgrade a oprava microservices v aplikacích nezávisle na sobě.

- Sledování a Diagnostika stav vaší žádosti a nastavit zásady provedením automatické opravy.

- Možnost škálování měřítko se změnami počtu uzlů v obrázku, jakož i měřítko zdola nahoru nebo dolů měřítko velikost jednotlivých uzlech, víte, že aplikace automaticky měřítko a rozloženy podle dostupné zdroje.

- Podívejte se na video vlastním retušovacího vyrovnávání zdrojů organizovat redistribuci aplikací přes clusteru. Služba obnoví z selhání a optimalizuje rozdělení zatížení podle dostupné zdroje.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Další kroky

* Další informace:
    * [Proč microservices přístupem k vytváření aplikací?](service-fabric-overview-microservices.md)
    * [Přehled terminologie](service-fabric-technical-overview.md)
* Nastavení služby struktury [Vývojové prostředí:](service-fabric-get-started.md)  
* [Výběr programovací rámec modelu](service-fabric-choose-framework.md) službě

[Image1]: media/service-fabric-overview/Service-Fabric-Overview.png
