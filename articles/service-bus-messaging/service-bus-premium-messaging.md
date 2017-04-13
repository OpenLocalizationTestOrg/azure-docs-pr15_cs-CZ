<properties
    pageTitle="Služba Bus Premium a standardní zasílání zpráv ceny úrovní přehled | Microsoft Azure"
    description="Služba Bus Premium a standardní zasílání zpráv"
    services="service-bus"
    documentationCenter=".net"
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/02/2016"
    ms.author="darosa;sethm"/>

# <a name="service-bus-premium-and-standard-messaging-tiers"></a>Služba Bus Premium a standardní zpráv úrovní 

Služby Bus zasílání zpráv, včetně zpráv entity například fronty a témat, sloučí enterprise zpráv možnosti s formátovaným publikovat odběr sémantiku ve velkém měřítku cloudu. Zasílání zpráv služby Bus slouží jako páteřní komunikace pro mnoho propracovaných cloudové řešení.

Na úrovni *Premium* zasílání zpráv služby Bus adresy běžné požadavků zákazníků kolem měřítko, výkon a dostupnost pro kritické aplikace. Sice téměř identické sady funkcí těchto dvou úrovní zasílání zpráv služby Bus se mají vytisknout případy použití různých.

V následující tabulce jsou zvýrazněné některé rozdíly nejvyšší úrovně.

| Premium                               | Standardní                       |
|---------------------------------------|--------------------------------|
| Vysoký výkon                       | Proměnná výkon            |
| Předvídatelná výkonu               | Proměnná latence               |
| Předvídatelná ceny                   | Platba podle příjmů proměnné ceny |
| Možnost zobrazit nahoru nebo dolů zátěží na projektu | NENÍ K DISPOZICI                            |
| Velikost zprávy > 256 znalostní bázi Knowledge Base                  | Velikost zprávy je 256 znalostní bázi Knowledge Base          |

**Služby Bus Premium pro zasílání zpráv** poskytuje zdroje izolace ve vrstvě procesoru a paměti tak, aby každý pracovní zátěž zákazníka samostatně. Tento kontejner zdroje se nazývá *zpráv jednotku*. Obor názvů každý premium přiřazen nejméně jednu jednotku zasílání zpráv. Máte možnost si zakoupit 1, 2 nebo 4 jednotky zpráv pro každou službu Bus Premium názvů. Jeden pracovní zátěž či entitu může zahrnovat více zpráv jednotky a počet jednotek, zasílání zpráv bude možné měnit kdykoli, sice fakturace ve 24hodinovém nebo denní sazeb. Výsledek je výkon předvídatelná a opakující řešení založených na Bus služby pro.

Nejen je tento výkon předvídatelná a dostupný, ale je také rychlejší. Zasílání zpráv služby Bus Premium je založena na modul úložiště zavedená v [Azure události rozbočovače](https://azure.microsoft.com/services/event-hubs/). Pole Špička výkon odpovídá Premium zpráv, rychleji než s standardní osy.

## <a name="premium-messaging-technical-differences"></a>Technické rozdíly Premium zasílání zpráv

Následuje několik rozdílů mezi Premium a standardní zpráv úrovní.

### <a name="partitioned-queues-and-topics"></a>Rozdělený fronty a témata

Rozdělený fronty a témata podporuje Premium zasílání zpráv, ale nefungují stejně jako standardní a základní úrovní zasílání zpráv služby Bus. Zasílání zpráv Premium nepoužívá SQL jako datový úložiště a už není možné zdroje soutěže přidruženou s sdílených platformu. Jako výsledek rozdělení není nutné zadávat. Kromě toho počet oddílů změnil z 16 oddílů ve standardní zpráv do 2 oddíly v Premium. Dva oddíly zajišťuje dostupnost s je vhodnější číslo prostředí runtime Premium. Další informace o vytváření oddílů najdete v článku [Partitioned fronty a témata](service-bus-partitioning.md).

### <a name="express-entities"></a>Expresní entity

Protože Premium zpráv získáte v prostředí úplně izolace runtime express entity nepodporuje obory názvů Premium. Další informace o funkci express najdete v článku vlastnost [Microsoft.ServiceBus.Messaging.QueueDescription.EnableExpress](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enableexpress.aspx) .

## <a name="next-steps"></a>Další kroky

Další informace o zasílání zpráv služby Bus, najdete v těchto tématech.

- [Úvodní informace o Premium Bus služby Azure zasílání zpráv (příspěvek na blog)](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
- [Úvodní informace o Premium Bus služby Azure zasílání zpráv (Channel9)](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
- [Přehled služeb Bus zasílání zpráv](service-bus-messaging-overview.md)
- [Jak používat službu Bus fronty](service-bus-dotnet-get-started-with-queues.md)
