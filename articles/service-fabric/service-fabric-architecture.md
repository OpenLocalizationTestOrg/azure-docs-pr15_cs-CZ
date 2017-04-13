<properties
   pageTitle="Architektura služby struktury | Microsoft Azure"
   description="Služba struktury je distribuované systémy platformy slouží k vytvoření scalable spolehlivý a snadno spravuje žádosti o cloudu. Tento článek popisuje architektura struktury služby."
   services="service-fabric"
   documentationCenter=".net"
   authors="rishirsinha"
   manager="timlt"
   editor="rishirsinha"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/09/2016"
   ms.author="rsinha"/>

# <a name="service-fabric-architecture"></a>Služba struktury architektura

Služba struktury je integrována s ve vrstvách podsystémy. Tyto podsystémy umožňují psát aplikace, které jsou:

* Vysoce k dispozici
* Scalable
* Spravovat
* Lze

Na následujícím obrázku vidíte hlavní podsystémy struktury služby.

![Diagram architektury struktury služby](media/service-fabric-architecture/service-fabric-architecture.png)

Distribuované sady je důležité možnost bezpečně komunikaci mezi uzly v clusteru. Na základě zásobníku je podsystém transport, nabízející zabezpečená komunikace mezi uzly. Nad přenos leží podsystém subsystému federace, který clusterů různé uzly do jedné entity (pojmenovali clusterů) tak, aby služby struktury zjišťování chyb, provést volbu vodicí znak a poskytují konzistentní směrování. Podsystém spolehlivost vrstvy nad podsystém federation je zodpovědný za spolehlivost služeb struktury služby prostřednictvím mechanismy například replikace, řízení zdrojů a překlopení. Podsystém federace také základem podsystém hostování a aktivace, která spravuje životní cyklus aplikace na načítání. Správa subsystému spravuje životní cyklus aplikací a služeb. Podsystém testování pomůže otestovat své služby Simulovaný chyb před a po nasazení aplikací a služeb na provozním prostředí vývojáře aplikace. Služba struktury umožňuje vyřešit umístění služby prostřednictvím jeho podsystému komunikace. Modely programování aplikace vystaven vývojáři jsou vrstvy nad tyto podsystémy spolu s model aplikace povolit nástrojů.

## <a name="transport-subsystem"></a>Přenosová podsystém
Podsystém transport používá kanálu bod k datagram komunikace. Tento kanál se používá pro komunikaci v rámci služby struktury clusterů a mezi klienty a služby struktury obrázku. Podporuje jednosměrné a odpověď na žádost o komunikace vzorů, které poskytuje základ pro provádění vysílání a multicast ve vrstvě federace. Podsystém transport zabezpečuje komunikace pomocí X509 poukázkám nebo zabezpečení systému Windows. Tento podsystém používá interně služby struktury a není přístupné pro vývojáře aplikace programování.

## <a name="federation-subsystem"></a>Federace podsystém
Abyste mohli důvod o sada uzlů v distribuované systému, musíte mít konzistentní zobrazení systému. Podsystém federace používá základní komunikace poskytovanou podsystém transport a spojí různých uzly do jednoho jednotné clusteru, který můžete důvod o. Poskytuje distribuované systémy základní potřeby tak, že ostatními podsystémy – zjišťování chyb zvolení vodicí znak a konzistentní směrování. Podsystém federace je této technologii postavené distribuované hash tabulky mezerou 128bitové tokenu. Podsystém vytvoří topologie vyzvánět u uzlů se každý uzel v seznamu vyzvánět přidělenou podmnožinu tokenu místa pro vlastnictví. Pro zjišťování chyb vrstvu pracuje na předmětem na základě srdce prevenci a arbitráž. Podsystém federace také zaručuje prostřednictvím komplikované spojení a odeslání protokolů, které pouze jednoho vlastníka token existuje kdykoli. To poskytnout volbu vodicí znak a konzistentní směrování záruky.

## <a name="reliability-subsystem"></a>Podsystém spolehlivosti
Podsystém spolehlivosti poskytuje mechanismus zpřístupnit stav služby služby struktury vysoce použitím _replikace_, _Správce převzetí_a _Vyrovnávání zdrojů_.

* Replikace zaručuje, že změny stavu v otevřené primární služby bude automaticky replikovat na vedlejší repliky zachovat konzistenci replik primárních a sekundárních v sadě otevřené služby. Replikace odpovídá za správu kvora mezi replikami v sadě otevřené. Interakce s jednotkou převzetí vytvořte požadovaný seznam operace replikovat a konfigurace agent poskytuje s konfigurací nastavení otevřené. Tuto konfiguraci označuje, které repliky operace je třeba replikovat. Služba struktury obsahuje výchozí replikace s názvem replikace struktury, které lze programové rozhraní API modelu zpřístupnit stavu služby vysoce a spolehlivé.
* Správce převzetí zajišťuje, že uzly jsou přidané do nebo odebrání z clusteru, načíst automaticky rozdělí mezi mezi uzly k dispozici. Pokud uzel clusteru, clusteru automaticky znovu repliky služby údržbu dostupnosti.
* Správce prostředků umístí služby repliky přes selhání domény clusteru a zajišťuje, aby byly všechny jednotky převzetí provozní. Správce prostředků také zůstatků prostředků služby přes fondu zdrojových sdílené uzlech k dosažení optimálního jednotné zatížení rozdělení

## <a name="management-subsystem"></a>Správa podsystém
Podsystém Správa poskytuje začátku do konce služby a správa životního cyklu aplikací. Rutiny prostředí PowerShell a rozhraní API pro správu umožňují zřízení nasazení, opravu, upgrade a zrušte zřízení aplikace bez ztráty dostupnost. Podsystém správy to provádí pomocí těchto služeb.

* **Správce clusteru**: Jedná se o primární službu, který spolupracuje s převzetí vedoucí z spolehlivost provést aplikace na základě omezení služeb umístění uzlů. Správce prostředků v převzetí podsystému zaručuje, omezení se nikdy přeruší. Správce clusteru spravuje životní cyklus aplikace z poskytování zrušeno zřízení. Lze integrovat s stavu správce k zajištění dostupnosti aplikace není ztratí z hlediska sémantického stavu během aktualizace.
* **Správce stavu**: umožňuje tato služba sledování stavu aplikací, službami a clusteru entity. Entity obrázku (například uzly služby oddíly a repliky) můžete vykázat jejich informace o stavu, které je pak agregován do úložiště centralizované stavu. Tyto informace stavu poskytují snímek celkové v daném okamžiku stavu služeb a uzly rozvržena více uzlů v clusteru umožňuje provádět všechny potřebné korekční akce. Stav dotazu, že můžete k vytvoření dotazu události stavu vymezit rozhraní API vykázaného stavu systému. Stav dotaz, který rozhraní API vrací nezpracovanými stavu dat uložených v úložišti stavu nebo agregované, interpretovaný stavu dat pro konkrétní clusteru entitu.
* **Obrázek Store**: Tato služba poskytuje skladování a distribuce binární soubory aplikace. Tato služba poskytuje jednoduché distribuované soubor úložiště kde nahráli a stahují z aplikací.


## <a name="hosting-subsystem"></a>Hostingu podsystém
Správce clusteru informuje hostingu podsystém (systém v jednotlivých uzlech) služby, které potřebuje ke správě pro konkrétní uzel. Podsystém hostingu potom spravuje životní cyklus aplikace na uzel. Interakce s spolehlivost a stav komponent zajistit, aby repliky umístěny správně a správný.

## <a name="communication-subsystem"></a>Komunikace podsystém
Tento podsystém zajišťuje spolehlivé zasílání zpráv v rámci obrázku a přihlašovacích údajů zjišťování prostřednictvím služby Naming. Služba Naming překládá názvy služby na místo v clusteru a uživatelům umožňuje spravovat názvy služeb a vlastnosti. Pomocí služby Naming, klienty bezpečně komunikovat s libovolný uzel v clusteru vyřešit název služby a získat metadata služby. Použití jednoduchého Naming klientského rozhraní API, můžete uživatelům služby struktury vyvíjet služeb a klientů může řešení aktuální umístění v síti bez ohledu na uzel dynamiky nebo znovu velikosti clusteru.

## <a name="testability-subsystem"></a>Testování podsystém
Testování je sada nástrojů speciálně pro zkušební služby založená na struktury služby. V sekci nástroje nechat developer snadno vyvolat smysluplné poruch a spusťte test scénáře výkon a ověřte spoustu států a přechody, které služby, budou mít v celém životnosti, všechny řízené a bezpečné způsobem. Testování také poskytuje mechanismus projít delší testů, které můžete zapracovávat ji různých možná selhání bez ztráty dostupnosti. To vám poskytne testovat v pracovním prostředí.
