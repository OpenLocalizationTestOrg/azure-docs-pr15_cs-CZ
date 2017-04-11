<properties
   pageTitle="Veřejné a soukromé Datacentrum/OS Agent fondy ACS | Microsoft Azure"
   description="Fondů veřejných a privátních agent fungování se služby Azure kontejneru obrázku."
   services="container-service"
   documentationCenter=""
   authors="Thraka"
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
   ms.date="08/16/2016"
   ms.author="timlt"/>

# <a name="dcos-agent-pools-for-azure-container-service"></a>Datacentrum/OS agenta skupiny služby Azure kontejneru

Služba Azure kontejneru Datacentrum/OS rozdělí agenti do veřejné nebo soukromé skupin. Nasazení můžete volání na některý z fondu došlo k ovlivnění usnadnění mezi počítačích služby kontejner. Strojů můžete vystaven na Internetu (veřejné) nebo interní (soukromé) k dispozici. Tento článek vám bude radit stručný přehled Proč jsou fond veřejných a privátních.

### <a name="private-agents"></a>Soukromé agenti

Soukromé agent uzly projít směrovat sítě. Této sítě je pouze z oblasti správce nebo přístupný pomocí směrovači okraj veřejné zóny. Ve výchozím nastavení otevře Datacentrum/OS aplikací na soukromé agent uzlů. Vyhledejte v [Datacentrum/OS si přečtěte následující dokumentaci](https://dcos.io/docs/1.7/administration/securing-your-cluster/) pro další informace o zabezpečení sítě.

### <a name="public-agents"></a>Veřejné agenti

Veřejné agent uzly spustit Datacentrum/OS aplikace a služby prostřednictvím veřejně přístupný sítě. Vyhledejte v [Datacentrum/OS si přečtěte následující dokumentaci](https://dcos.io/docs/1.7/administration/securing-your-cluster/) pro další informace o zabezpečení sítě.

## <a name="using-agent-pools"></a>Používání skupiny agenta

Ve výchozím nastavení nasadí **Marathon** nové aplikace do *soukromé* agent uzlů. Budete muset explicitně nasazení aplikace uzel *veřejná* při vytváření aplikace. Vyberte kartu **volitelné** a zadejte **slave_public** hodnotu **Přijaté role zdroje** . Tento postup je popsána [tady](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) a v dokumentaci [DC\OS](https://dcos.io/docs/1.7/administration/installing/custom/create-public-agent/) .

## <a name="next-steps"></a>Další kroky

Přečtěte si další informace o [správě Datacentrum/OS kontejnery](container-service-mesos-marathon-ui.md).

Zjistěte, jak [Otevřít bránu firewall](container-service-enable-public-access.md) poskytovanou Azure veřejné přístupu k Datacentrum/OS kontejner.