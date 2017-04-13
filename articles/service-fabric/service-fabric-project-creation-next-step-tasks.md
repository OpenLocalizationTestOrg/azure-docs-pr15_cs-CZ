<properties
   pageTitle="Další kroky vytvoření služby struktury projektu | Microsoft Azure"
   description="Tento článek obsahuje odkazy na sadu core vývoj úkolů pro službu struktury"
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/08/2016"
   ms.author="seanmck"/>

# <a name="your-service-fabric-application-and-next-steps"></a>Aplikace služby struktury a další kroky
Vytvoření struktury služby Azure aplikace. Tento článek popisuje způsob vytvoření projektu a potenciální různě.

## <a name="your-application"></a>Aplikace
Každé nové aplikace obsahuje projekt aplikace. Doména může obsahovat jednu nebo dvě další projekty v závislosti na typu služby zvolili.

### <a name="the-application-project"></a>Projekt aplikace
Projekt aplikace se skládá z:

- Sada odkazy na služby, které tvoří aplikace.

- Dvě publikovat profily (místní a cloudu), které slouží ke správě předvolby pro práci s různých prostředích – například předvolby související s clusteru koncového bodu a jestli se mají provést upgrade nasazení ve výchozím nastavení.

- Dva parametr soubory aplikace (místní a cloudu), které můžete použít ke správě konfigurací prostředí specifické aplikací, jako je třeba počet oddílů a vytvořte pro službu.

- Skript pro nasazení využívající pro nasazení aplikace z příkazového řádku nebo jako součást automatické nepřetržitý kanálu integrace a nasazení.

- Manifestu aplikace, který popisuje aplikace. Manifest můžete najít ve složce ApplicationPackageRoot.

### <a name="stateless-service"></a>Příslušnosti služby
Po přidání nového příslušnosti služby Visual Studio přidá projekt služby do vašeho řešení, která obsahuje typ následníky `StatelessService`. Služba zvýší lokální proměnnou čítač.

### <a name="stateful-service"></a>Stavová služba
Po přidání nového stavové služby Visual Studio přidá projekt služby do vašeho řešení, která obsahuje typ následníky `StatefulService`. Služba zvýší čítač v jeho `RunAsync` metoda a ukládá výsledek v `ReliableDictionary`.

### <a name="actor-service"></a>Objekt actor služby
Po přidání nového spolehlivé actor Visual Studio přidá do vašeho řešení dva projekty: projekt actor a rozhraní projektu.

Actor project nabízí metod pro nastavení a získání hodnoty čítač problémy se spolehlivým trvalý je uvnitř objektu actor státu. Rozhraní project poskytuje rozhraní využívající jiné služby pro vyvolání actor.

### <a name="stateless-web-api"></a>Příslušnosti webového rozhraní API
Příslušnosti rozhraní API webových project nabízí základní webové služby, které slouží k otevření aplikace externí klientům. Další informace o projektu strukturovaná, najdete v článku [rozhraní API webových služeb struktury služeb se OWIN vlastní hostování](service-fabric-reliable-services-communication-webapi.md).

### <a name="aspnet-core"></a>Základní technologie ASP.NET

SDK struktury služba poskytuje stejná skupina základní ASP.NET šablony, které jsou k dispozici pro samostatnou ASP.NET základní projekty: prázdný, [Rozhraní API webových][aspnet-webapi]a [Webovou aplikaci][aspnet-webapp].

## <a name="next-steps"></a>Další kroky
### <a name="create-an-azure-cluster"></a>Vytvoření clusteru služby Azure
SDK struktury služba poskytuje místní clusteru vývoj a testování. Vytvoření cluster v Azure najdete v tématu [Nastavení služby struktury obrázku z portálu Microsoft Azure][create-cluster-in-portal].

### <a name="try-deploying-to-azure-for-free-with-party-clusters"></a>Zkuste s nasazováním Azure zdarma s účastníky clusterů

Pokud chcete vyzkoušet nasazení a Správa aplikací v Azure bez nastavování vlastního clusterů, můžete pomocí bezplatné [služby stran obrázku](http://aka.ms/tryservicefabric).

### <a name="publish-your-application-to-azure"></a>Publikování aplikace Azure
Můžete publikovat aplikace přímo z aplikace Visual Studio Azure clusteru. Další informace naleznete v tématu [publikování aplikace Azure][publish-app-to-azure].

### <a name="use-service-fabric-explorer-to-visualize-your-cluster"></a>Vizualizace svůj cluster pomocí Průzkumníka struktury služby
Služba struktury Explorer nabízí snadný způsob, jak vizualizovat clusteru, včetně aplikací nasazena a fyzické rozložení. Další informace najdete v tématu [vizualizace svůj cluster v programu Průzkumník struktury služby][visualize-with-sfx].

### <a name="version-and-upgrade-your-services"></a>Verze a upgradu služeb
Služba struktury umožňuje nezávislé správy verzí a upgradu služeb nezávisle v aplikaci. Další informace najdete v tématu [Správa verzí a upgradu služeb][app-upgrade-tutorial].

### <a name="configure-continuous-integration-with-visual-studio-team-services"></a>Konfigurace nepřetržitý integrace služby týmu Visual Studio
Zjistěte, jak můžou nastavit integrace nepřetržitý proces pro aplikaci služby struktury, najdete v článku [Konfigurace nepřetržitý integrace s Visual Studio týmovou][ci-with-vso].


<!-- Links -->
[add-web-frontend]: service-fabric-add-a-web-frontend.md
[create-cluster-in-portal]: service-fabric-cluster-creation-via-portal.md
[publish-app-to-azure]: service-fabric-publish-app-remote-cluster.md
[visualize-with-sfx]: service-fabric-visualizing-your-cluster.md
[ci-with-vso]: service-fabric-set-up-continuous-integration.md
[reliable-services-webapi]: service-fabric-reliable-services-communication-webapi.md
[app-upgrade-tutorial]: service-fabric-application-upgrade-tutorial.md
[aspnet-webapi]: https://docs.asp.net/en/latest/tutorials/first-web-api.html
[aspnet-webapp]: https://docs.asp.net/en/latest/tutorials/first-mvc-app/index.html
