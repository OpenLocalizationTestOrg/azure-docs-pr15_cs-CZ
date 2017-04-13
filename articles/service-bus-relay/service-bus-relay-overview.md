<properties
    pageTitle="Přehled relay Service Bus | Microsoft Azure"
    description="Základní informace o relay Service Bus."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="sethm"/>


# <a name="overview-of-service-bus-relay"></a>Základní informace o relay Service Bus

Hlavní součásti Bus služby je služba centralizované (ale vysoce Vyrovnávání zatížení) *relay* , který umožňuje vytvářet hybridní aplikace, které se spouštějí v Azure datacentru a vlastní místní organizace prostředí.  Relay Service Bus podporuje spoustu různých transportní protokoly a standardy webových služeb. Tato volba zahrnuje SOAP WS-, * a to i ostatní. Služby pro přenos dat usnadňuje aplikace hybridní povolením bezpečně zpřístupňují služby Windows Communication Foundation (WCF), které se nacházejí v rámci podnikové sítě pro veřejné cloudu, aniž byste museli otevřít připojení brány firewall nebo vyžadovat rušivá změny infrastrukturu podnikové sítě. 

![Předávací koncepty](./media/service-bus-relay-overview/sb-relay-01.png)

Služby pro přenos dat podporuje tradiční jednosměrné zpráv, žádostí a odpovědí zasílání zpráv a účastníky zasílání zpráv. Podporuje také události rozdělení v hodnotě internet rozsah povolit publikovat nebo přihlášení k odběru scénáře a obousměrné socket komunikace pro lepší bod k efektivity. 

Ve stavu zpráv vzorku služby místní připojí se k služby pro přenos dat pomocí odchozího portu a vytvoří obousměrné socket pro komunikaci se adresu konkrétní rendezvous stejným. Klient potom komunikovat s místní službou tak, že zprávy služby pro přenos dat cílovou adresu rendezvous. Služby pro přenos dat bude pak "přenášet" zprávy místní služby prostřednictvím socket obousměrné už na místě. Klient nemusí přímé připojení ke službě místní, není potřeba vědět, kde je služba umístěna a službu místní nemusí všechny příchozí porty otevřít v bráně firewall.

Spusťte připojení mezi místním služby a služby pro přenos dat pomocí sady vazeb WCF "relay". Na pozadí vazby relay mapovat na nové prvky vazby transport navržené tak, jak vytvořit WCF kanálu součásti integrovaných s Bus služby v cloudu. 

## <a name="next-steps"></a>Další kroky

Podrobné informace o relay Service Bus naleznete v následujících tématech.

- [Přehled Bus služby Azure](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
- [Jak používat službu Service Bus Relay](service-bus-dotnet-how-to-use-relay.md)

 