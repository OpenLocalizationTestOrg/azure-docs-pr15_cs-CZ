<properties
    pageTitle="Co je Azure relay? | Microsoft Azure"
    description="Základní informace o přenosu Azure"
    services="service-bus"
    documentationCenter=".net"
    authors="banisadr"
    manager="timlt"
    editor="" />

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="babanisa" />

# <a name="what-is-azure-relay"></a>Co je Azure Relay?

Azure služby pro přenos dat usnadňuje aplikace hybridní povolením bezpečně vystavit služby, které se nacházejí v rámci podnikové sítě pro veřejné cloudu, aniž byste museli otevřít připojení brány firewall nebo vyžadovat rušivá změny infrastrukturu podnikové sítě. Předávací podporuje spoustu různých transportní protokoly a standardy webových služeb.

Služby pro přenos dat podporuje tradiční jednosměrný žádostí a odpovědí a přenosy na účastníky. Podporuje také události rozdělení v hodnotě internet rozsah povolit publikovat nebo přihlášení k odběru scénáře a obousměrné socket komunikace pro lepší bod k efektivity. 

Ve vzorku přenesení přenášené dat služby místní připojí se k služby pro přenos dat pomocí odchozího portu a vytvoří obousměrné socket pro komunikaci se adresu konkrétní rendezvous stejným. Klient potom komunikovat s místní službou tak, že přenosy služby pro přenos dat cílovou adresu rendezvous. Služby pro přenos dat bude pak "přenášet" data službám místní prostřednictvím obousměrné socket snaží každého klienta. Klient nemusí přímé připojení ke službě místní, není potřeba vědět, kde je služba umístěna a službu místní nemusí všechny příchozí porty otevřít v bráně firewall.

Prvky klíčové funkce poskytované Relay jsou obousměrné, neuložený v mezipaměti komunikace přes hranice sítě s TCP profesionálové omezení koncový bod zjišťování, stav připojení a překryvném koncový bod zabezpečení. Předávací je funkce se liší od úrovni sítě integrace technologiemi usnadnění, například VPN v, že můžete omezené koncový bod jedné aplikace na jednom počítači, zatímco je mnohem více rušivá závisí na změny prostředí sítě VPN technologie.

Azure Relay má dvě funkce:

1. [Hybridní připojení](#hybrid-connections) - používá otevřít standardní Sockets Web podpora s více platformami scénářů

2. [WCF relé](#wcf-relays) - používá Windows Communication Foundation umožňující vzdálené řízení volání

Hybridní připojení a WCF relé povolit zabezpečené připojení k assests, která jsou v rámci podnikové sítě. Použití jednoho přednost před druhou jsou závislé na vašim potřebám podrobné níže:

|                                    | Předávací WCF | Hybridní připojení |
| ---------------------------------- |:---------:|:------------------:|
| **WCF**                            |     x     |                    |
| **Základní .NET**                      |           |         x          |
| **.NET framework**                 |     x     |         x          |
| **JavaScript/NodeJS**              |           |         x          |
| **Java***                          |           |         x          |
| **Standardy otevřít Protocol (protokol)**  |           |         x          |
| **Více modelů Programing RPC** |           |         x          |
*<sub>Podle všeobecně dostupná</sub>

## <a name="hybrid-connections"></a>Hybridní připojení

Azure Relay hybridní připojení, kterou možnost je zabezpečené, otevřít protokol vývoj existující Relay funkcí, které může být implementovat na libovolné platformě na libovolný jazyk, který obsahuje základní funkce WebSocket explicitně společné obsahuje rozhraní API WebSocket webových prohlížečů. Hybridní připojení je založený na HTTP a WebSockets.

## <a name="wcf-relays"></a>WCF relé

Předávací WCF lze použít pro úplné .NET Framework (NETFX) a WCF. Spusťte připojení mezi místním služby a služby pro přenos dat pomocí sady vazeb WCF "relay". Na pozadí vazby relay mapovat na nové prvky vazby transport navržené tak, jak vytvořit WCF kanálu součásti integrovaných s Bus služby v cloudu.

## <a name="service-history"></a>Historie aktualizací Service

Hybridní připojení školkovány bývalá rovnoměrně z vnější s názvem "BizTalk služby" funkce vytvořeném v Relay WCF Bus služby Azure. Nová funkce hybridního připojení doplňuje existující Relay WCF a tyto dva služby funkce bude existovat vedle sebe ve službě Relay pro blízké budoucnosti; sdílet běžné bránu, ale jsou jinak různých implementace.

Předávací WCF je starší Relay nabízející, že množství zákazníků mohou již pomocí své modely programing WCF.

## <a name="next-steps"></a>Další kroky:

- [Předávací časté otázky](relay-faq.md)
- [Vytvořit obor názvů](relay-create-namespace-portal.md)
- [Začínáme s .NET](relay-hybrid-connections-dotnet-get-started.md)
- [Začínáme s uzel](relay-hybrid-connections-node-get-started.md)