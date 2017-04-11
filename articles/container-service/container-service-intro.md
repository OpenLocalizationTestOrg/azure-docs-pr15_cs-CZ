<properties
   pageTitle="Představení služby Azure kontejneru | Microsoft Azure"
   description="Služba Azure kontejneru umožňuje zjednodušit vytvoření, konfigurace a správy clusteru virtuálních počítačích nakonfigurovaných spustit kontejnerizovaná aplikace."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, kontejnerů, Micro služby, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="rogardle"/>

# <a name="azure-container-service-introduction"></a>Úvod kontejneru služby Azure

Služba Azure kontejneru usnadňuje ho vytvořit, konfigurace a Správa clusteru virtuálních počítačích nakonfigurovaných spustit kontejnerizovaná aplikace. Použije optimalizované konfigurace Oblíbené nástrojů plánování a průběhu otevřít zdroj. Umožňuje použít existujících dovedností nebo čerpat velkých a rostoucí text komunity odbornost, nasazení a spravovat aplikace založené na kontejner na Microsoft Azure.


![Služba Azure kontejneru umožňuje spravovat kontejnerizovaná aplikace ve více hostitelů na Azure.](./media/acs-intro/acs-cluster.png)


Služba Azure kontejneru využívá formátu kontejneru Docker k zajištění plně portable kontejnery aplikace. Taky podporuje u možnosti Marathon a Datacentrum/OS nebo Docker Swarm, takže můžete zobrazit těmito aplikacemi tisíce kontejnerů nebo dokonce desítky tisíců.

Pomocí služby Azure kontejneru můžete využít funkce podnikové Azure, ale zachová přenosnost aplikace – včetně přenosnost v průběhu vrstvy.

<a name="using-azure-container-service"></a>Použití služby Azure kontejneru
-----------------------------

Kontejner službou Azure Naším cílem je poskytovat kontejneru hostitelské prostředí pomocí nástroje otevřít zdroj a technologií pro server, které jsou u jejích zákazníků Oblíbené dnes. K tomuto účelu jsme vystavit standardní koncové body rozhraní API pro vaše zvolené orchestrator (s operačním systémem/Datacentrum nebo Docker Swarm). Pomocí těchto koncové body, můžete využít software, který může spolu mluvit tyto koncové body. Například v případě koncový bod Docker Swarm, můžete používat rozhraní příkazového řádku Docker (rozhraní příkazového řádku). Pro Datacentrum/s operačním systémem může se rozhodnete sdělit nám DCOS rozhraní příkazového řádku.

<a name="creating-a-docker-cluster-by-using-azure-container-service"></a>Vytvoření clusteru Docker pomocí služby Azure kontejneru
-------------------------------------------------------

Pokud chcete začít používat služby Azure kontejneru, nasazení služby Azure kontejneru clusteru prostřednictvím portálu (hledání "Služba Azure kontejneru"), pomocí Správce prostředků Azure šablony ([Docker Swarm](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm) nebo [Datacentrum/OS](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)) nebo pomocí [rozhraní příkazového řádku](/documentation/articles/xplat-cli-install/). Zadané rychlý úvod šablony můžete upravit tak, aby zahrnovat další nebo rozšířené Azure konfigurace. Další informace o nasazení služby Azure kontejneru clusteru najdete v článku [nasazení služby Azure kontejneru obrázku](container-service-deployment.md).

<a name="deploying-an-application"></a>Nasazení aplikace
------------------------

Služba Azure kontejneru umožňuje výběr Docker Swarm nebo Datacentrum/OS pro průběhu. Nasazení aplikace závisí na svojí volbě orchestrator.

### <a name="using-dcos"></a>Použití Datacentrum/OS

Datacentrum/OS je distribuované operační systém podle jádra Apache Mesos distribuované systémy. Apache Mesos nachází na Apache Software Foundation a jsou uvedeny některé [největších názvy v IT](http://mesos.apache.org/documentation/latest/powered-by-mesos/) jako uživatele a spolupracovníky.

![Služba Azure kontejneru nakonfigurovány roj zobrazující agentů a předloh.](media/acs-intro/dcos.png)

Datacentrum/OS a Apache Mesos obsahují sadu působivé funkcí:

-   Osvědčené škálovatelnost

-   Chybám replikovat předlohy a slaves pomocí Apache ZooKeeper

-   Podpora pro převod Docker kontejnery

-   Nativní izolace mezi úkoly s Linux kontejnery

-   Multiresource plánování (paměti procesoru, disk a porty)

-   Java, Python a rozhraní API C++ k vývoji aplikací paralelní

-   Web uživatelského rozhraní pro zobrazení informací o stavu obrázku

Ve výchozím nastavení zahrnuje Datacentrum/s operačním systémem spuštěných pro službu Azure kontejneru platformu Marathon průběhu při plánování úloh. Zahrnutý v sadě Datacentrum/operačním systémem rozmístění ACS je však mezosféře Universe služby, které lze přidat do služby, jedná se o Spark Hadoop, Cassandra a řadu dalších objektů.

![Datacentrum/OS Universe služby Azure kontejneru](media/dcos/universe.png)

#### <a name="using-marathon"></a>Použití Marathon

Marathon je inicializace nevyžádaného obrázku a systému správy služby v cgroups – nebo v případě Azure kontejneru služby formátovaných Docker kontejnery. Marathon poskytuje uživatelské rozhraní webu odkud nástroje můžete nasazovat aplikace. Dostanete se k němu na adresu URL, která vypadá podobně jako `http://DNS_PREFIX.REGION.cloudapp.azure.com` kde DNS\_PŘEDPONA a oblasti jsou oba definované při nasazení. Samozřejmě můžete zadat také názvu DNS. Další informace o spuštění kontejneru pomocí webového Marathon uživatelského rozhraní najdete v tématu [kontejneru správy prostřednictvím webového uživatelského rozhraní](container-service-mesos-marathon-ui.md).

![Seznam aplikací Marathon](media/dcos/marathon-applications-list.png)

Můžete taky rozhraní REST API pro komunikaci s Marathon. Existuje celá řada knihoven klienta, které jsou k dispozici pro každý z nástrojů. Průvodní různé jazyky – a samozřejmě můžete použít protokol HTTP v jiném jazyce. Kromě toho mnoho oblíbených nástroje DevOps podporují Marathon. To poskytuje správcům maximální flexibilitu operace týmu při práci s služba Azure kontejneru obrázku. Další informace o spuštění kontejneru pomocí rozhraní REST API Marathon najdete v tématu [Správa kontejneru pomocí rozhraní REST API](container-service-mesos-marathon-rest.md).

### <a name="using-docker-swarm"></a>Použití Docker roj

Docker roj poskytuje nativní clusterů pro Docker. Protože Docker Swarm slouží standardní rozhraní API Docker, nástroj, už komunikující s Docker démon umožňuje roj transparentně přizpůsobit více hostitelů na kontejner služby Azure.

![Služba Azure kontejneru nakonfigurovaný na používání Datacentrum/s operačním systémem – zobrazující jumpbox, agentů a předloh.](media/acs-intro/acs-swarm2.png)

Podporované nástroje pro správu kontejnery clusteru roj zahrnout, ale nejsou omezeny takto:

-   Dokku

-   Vytvořte docker rozhraní příkazového řádku a Docker

-   Krane

-   Jenkins

<a name="videos"></a>Videa
------

Začínáme se službou Azure kontejneru (101):  

> [AZURE.VIDEO azure-container-service-101]

Vytváření aplikací pomocí služby Azure kontejneru (sestavení 2016)

> [AZURE.VIDEO build-2016-building-applications-using-the-azure-container-service]
