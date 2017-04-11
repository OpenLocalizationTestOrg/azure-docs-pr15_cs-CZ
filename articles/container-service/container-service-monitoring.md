<properties
   pageTitle="Sledování služby Azure kontejneru obrázku s Datadog | Microsoft Azure"
   description="Sledujte služba Azure kontejneru obrázku s Datadog. Pomocí webového Datacentrum/OS uživatelského rozhraní nasazení agentů Datadog svůj cluster."
   services="container-service"
   documentationCenter=""
   authors="rbitia"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Kontejnery, Datacentrum/OS, Docker roj Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure"   
   ms.date="07/28/2016"
   ms.author="t-ribhat"/>

# <a name="monitor-an-azure-container-service-cluster-with-datadog"></a>Sledování služby Azure kontejneru obrázku s Datadog

V tomto článku jsme nasazovat Datadog agenti do všech agent uzlů v kontejneru služby Azure obrázku. Účet s Datadog budete potřebovat pro tuto konfiguraci. 

## <a name="prerequisites"></a>Zjistit předpoklady pro 

[Nasazení](container-service-deployment.md) a [připojení](container-service-connect.md) clusteru nakonfigurované službou Azure kontejner. Prozkoumání [Marathon uživatelského rozhraní](container-service-mesos-marathon-ui.md). Přejděte na [http://datadoghq.com](http://datadoghq.com) nastavit účet Datadog. 

## <a name="datadog"></a>Datadog 

Datadog je sledování služba, která shromažďuje monitorování dat z kontejnerů v rámci služby Azure kontejneru clusteru. Datadog má řídicího integrace Docker kde navíc přehledně uvidíte konkrétní metriky v rámci vašeho kontejnerů. Metriky shromážděné z vaší kontejnery jsou organizovány pomocí procesoru, paměti, sítě a vstupu a výstupu. Datadog metriky rozdělí kontejnerů a obrázky. Příklad vypadá rozhraní pro využití procesoru je menší než.

![Uživatelské rozhraní Datadog](./media/container-service-monitoring/datadog4.png)

## <a name="configure-a-datadog-deployment-with-marathon"></a>Konfigurace Datadog nasazení s Marathon

Tento postup se dozvíte o konfiguraci a nasazení aplikace Datadog svůj cluster s Marathon. 

Přístup k rozhraní Datacentrum/OS prostřednictvím [http://localhost:80 /](http://localhost:80/). Jednou v uživatelském rozhraní Datacentrum/OS přejděte "Universe" což je v levém dolním rohu vyhledejte "Datadog" a potom klikněte na "Nainstalujte."

![Datadog balíčku v rámci Universe Datacentrum/OS](./media/container-service-monitoring/datadog1.png)

Teď dokončete konfiguraci byste potřebovali Datadog účet nebo účet bezplatnou zkušební verzi. Po přihlášení k vzhledu webu Datadog doleva a přejděte na integraci -> potom rozhraní API. 

![Rozhraní API Datadog klíč](./media/container-service-monitoring/datadog2.png)

Potom zadejte kód rozhraní API do konfiguraci Datadog v rámci Universe Datacentrum/OS. 

![Konfigurace Datadog ve Universe Datacentrum/OS](./media/container-service-monitoring/datadog3.png) 

Ve výše uvedené konfigurace jsou nastavené instance 10000000 tak pokaždé, když je přidán nový uzel clusteru Datadog automaticky nasadit agent na uzel. Toto je dočasné řešení. Po instalaci balíček by měl přejděte zpátky na web Datadog a najděte "Řídicích panelů." Tady uvidíte vlastní a integrace řídicí panely. Řídicí panel integrace Docker budou mít všechny metriky kontejneru, které potřebujete pro sledování svůj cluster. 
