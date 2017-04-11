<properties
   pageTitle="Sledování služby Azure kontejneru obrázku s Sysdig | Microsoft Azure"
   description="Sledujte služba Azure kontejneru obrázku s Sysdig."
   services="container-service"
   documentationCenter=""
   authors="rbitia"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Kontejnery, Datacentrum/s operačním systémem, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/08/2016"
   ms.author="t-ribhat"/>

# <a name="monitor-an-azure-container-service-cluster-with-sysdig"></a>Sledování služby Azure kontejneru obrázku s Sysdig

V tomto článku jsme nasazovat Sysdig agenti do všech agent uzlů v kontejneru služby Azure obrázku. Potřebujete pro účet Sysdig pro tuto konfiguraci. 

## <a name="prerequisites"></a>Zjistit předpoklady pro 

[Nasazení](container-service-deployment.md) a [připojení](container-service-connect.md) clusteru nakonfigurované službou Azure kontejner. Prozkoumání [Marathon uživatelského rozhraní](container-service-mesos-marathon-ui.md). Přejděte na [http://app.sysdigcloud.com](http://app.sysdigcloud.com) na nastavení účtu Sysdig cloudu. 

## <a name="sysdig"></a>Sysdig

Sysdig je sledování služba, která umožňuje sledovat kontejnery do svého obrázku. Je známo, že Sysdig pomoci při řešení ale také základní sledování metriky procesoru, sítě, paměti a vstupu a výstupu. Sysdig usnadňuje vidíte, které kontejnery pracují hardest nebo v podstatě používá nejčastěji paměti a procesoru. Tohle zobrazení je v části "sestava Přehled", která je aktuálně beta. 

![Uživatelské rozhraní Sysdig](./media/container-service-monitoring-sysdig/sysdig6.png) 

## <a name="configure-a-sysdig-deployment-with-marathon"></a>Konfigurace Sysdig nasazení s Marathon

Tento postup se dozvíte o konfiguraci a nasazení aplikace Sysdig svůj cluster s Marathon. 

Přístup k rozhraní Datacentrum/OS prostřednictvím [http://localhost:80 /](http://localhost:80/) jednou v uživatelském rozhraní Datacentrum/OS přejděte "Universe", který je v levém dolním rohu a vyhledejte "Sysdig."

![Sysdig v Datacentrum/OS Universe](./media/container-service-monitoring-sysdig/sysdig1.png)

Teď dokončete konfiguraci potřebujete účet cloudu Sysdig nebo bezplatnou zkušební účet. Jakmile jste přihlášení k lyncu cloudu web Sysdig, klikněte na své uživatelské jméno a na stránce byste měli vidět váš "přístupová klávesa." 

![Rozhraní API Sysdig klíč](./media/container-service-monitoring-sysdig/sysdig2.png) 

Potom zadejte přístupová klávesa do konfiguraci Sysdig v rámci Universe Datacentrum/OS. 

![Konfigurace Sysdig ve Universe Datacentrum/OS](./media/container-service-monitoring-sysdig/sysdig3.png)

Nyní sadu instance, které chcete 10000000 tak pokaždé, když je přidán nový uzel clusteru Sysdig bude automaticky nasazení agenta do tohoto nového uzlu. Toto je dočasné řešení, aby zkontrolovala, jestli že sysdig nasadí pro všechny nové agentů v rámci clusteru. 

![Konfigurace Sysdig ve Datacentrum/Universe s operačním systémem-instance](./media/container-service-monitoring-sysdig/sysdig4.png)

Po instalaci balíček přejděte zpátky do uživatelského rozhraní Sysdig a budete moct prozkoumat metriky různých použití kontejnerů v rámci svého obrázku. 