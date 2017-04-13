<properties
    pageTitle="Přehled zasílání zpráv služby Bus | Microsoft Azure"
    description="Zprávy Bus služby: flexibilní data doručení v cloudu"
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
    ms.date="09/27/2016"
    ms.author="sethm"/>


# <a name="service-bus-messaging-flexible-data-delivery-in-the-cloud"></a>Zasílání zpráv služby Bus: doručení flexibilní dat v cloudu

Microsoft Azure služby Bus je služba pro doručování spolehlivé informace. Má tuto službu účel usnadnit komunikace. Dva nebo víc stranami potřeby výměnu informací o potřebují mechanismus komunikace. Služba Bus je mechanismus komunikace brokered nebo jiných výrobců. Toto je podobný poštovní služby fyzické světě. Poštovní služby usnadňují velmi odeslat různé druhy dopisů a balíčků s různými dodání záruky kdekoli na světě.

Podobně jako poštovní služby předvádění písmena Bus služby je doručování flexibilní informací od odesílatele a příjemce. Zasílání zpráv služby zaručuje, informace se doručení i v případě obě strany nikdy obou online ve stejnou dobu, případně nejsou k dispozici současně přesné. Tímto způsobem zasílání zpráv je podobný dopisu, když není brokered komunikace je podobné jako zavolat (nebo použití telefonní hovor na - před čekání a volajícím volat ID, které jsou mnohem víc jako brokered zpráv).

Odesílatel zprávy můžete vyžadují navíc řadu doručení vlastnosti včetně transakce, duplicit, založená na čase vypršení platnosti a dávky. Tyto vzorce mít taky poštovní analogie: opakování dodání, požadovaný podpis a změnit adresu nebo odvolat.

Služby Bus podporuje dvou různých zpráv vzory: *Relay* a *brokered zasílání zpráv*.

## <a name="service-bus-relay"></a>Service Bus Relay

Součást [Relay](../service-bus-relay/service-bus-relay-overview.md) Service Bus je služba centralizované (ale vysoce Vyrovnávání zatížení), který podporuje spoustu různých transportní protokoly a standardy webových služeb. Tato volba zahrnuje SOAP WS-, * a to i ostatní. [Služby pro přenos dat](../service-bus-relay/service-bus-dotnet-how-to-use-relay.md) nabízí celou řadu možností připojení různých relay a pomáhají vyjednávání přímé připojení k účastníky, pokud je možné. Služba je optimalizována pro vývojáře .NET, kteří používají Windows Communication Foundation (WCF), jak pokud jde o výkonu i použitelnost a nabízí plný přístup k jeho služby pro přenos dat prostřednictvím rozhraní SOAP a PODRŽTE. Díky tomu SOAP nebo ZBÝVAJÍCÍ programovacím prostředím integrovat s Bus služby.

Služby pro přenos dat podporuje tradiční jednosměrné zpráv, žádostí a odpovědí zasílání zpráv a účastníky zasílání zpráv. Podporuje také události rozdělení v hodnotě Internet obor povolit publikovat přihlášením scénářů a obousměrné socket komunikace pro lepší bod k efektivity. Ve stavu zpráv vzorku služby místní připojí se k služby pro přenos dat pomocí odchozího portu a vytvoří obousměrné socket pro komunikaci se adresu konkrétní rendezvous stejným. Klient potom komunikovat s místní službou tak, že zprávy služby pro přenos dat cílovou adresu rendezvous. Služby pro přenos dat bude pak "přenášet" zprávy místní služby prostřednictvím socket obousměrné už na místě. Klient nemusí přímé připojení ke službě místní ani je potřeba vědět, kde je služba umístěna a službě místní nemusí jakýkoli příchozí otevřít v bráně firewall.

Aktivujete připojení mezi místním služby a služby pro přenos dat pomocí sady vazeb WCF "relay". Na pozadí vazby relay namapovat přenosová elementy vazby navržené tak, jak vytvořit WCF kanálu součásti integrovaných s Bus služby v cloudu.

Service Bus Relay nabízí mnoho výhod, ale vyžaduje serveru a klientských současně být v online ve stejnou dobu, abyste mohli odesílat a přijímat zprávy. Toto není optimální pro komunikaci HTTP stylu, ve kterém nemusí být žádosti obvykle dlouhodobé ani pro klienty připojit jenom v některých případech například prohlížeče, mobilní aplikace a tak dále. Brokered oddělené komunikace podporuje zasílání zpráv a má své výhody; Když potřeby a provádění svých operací asynchronní způsobem se můžete připojit klienty a servery.

## <a name="brokered-messaging"></a>Brokered zasílání zpráv

Na rozdíl od schématu relay [brokered zpráv](service-bus-queues-topics-subscriptions.md) lze představit jako asynchronní nebo "dočasně oddělené." Výrobci (odesílatelů) a které se zobrazují koncovým (příjemcům) nemusí být online ve stejnou dobu. Infrastruktura pro zasílání zpráv problémy se spolehlivým ukládá zprávy v "zprostředkovatele" (jako je do fronty), dokud náročný strana je připravená k jejich odběru. Díky komponent distribuované aplikace odpojení, buď dobrovolně; například pro údržbu nebo z důvodu zhroucení součásti bez ovlivnění celého systému. Kromě toho přijímání aplikace může obsahovat k přechodu do online určité době den, například systému správy zásob, jenom je potřeba spustit na konci pracovního dne.

Základní součásti infrastruktury služby Bus brokered zpráv jsou fronty, témata a předplatné.  Základní rozdíl je, že témata podporují možnosti publikování a přihlášení k odběru, které se dá použít pro sofistikované založené na obsahu směrování a doručení použití logických operátorů, včetně odesílání více příjemcům. Tyto součásti povolit nové asynchronní scénářů zasílání zpráv, například časový oddělení, publikování a přihlášení k odběru a vyrovnávání zatížení. Další informace o těchto zpráv entit najdete v článku [frontách Bus služby, témata a předplatná](service-bus-queues-topics-subscriptions.md).

Stejně jako u infrastruktury Relay brokered funkce zpráv slouží programátorům WCF a .NET Framework a taky prostřednictvím ZBÝVAJÍCÍ.

## <a name="next-steps"></a>Další kroky

Další informace o zasílání zpráv služby Bus, najdete v těchto tématech.

- [Základy Bus služby](service-bus-fundamentals-hybrid-solutions.md)
- [Služba Bus fronty, témata a předplatné](service-bus-queues-topics-subscriptions.md)
- [Jak používat službu Bus fronty](service-bus-dotnet-get-started-with-queues.md)
- [Používání služby Bus témata a předplatného](./service-bus-dotnet-how-to-use-topics-subscriptions.md)
 
