<properties 
    pageTitle="Architektura služby Bus | Microsoft Azure"
    description="Popisuje zprávy a předávací zpracování architektura Bus služby Azure."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="07/11/2016"
    ms.author="sethm" />

# <a name="service-bus-architecture"></a>Služba Bus architektura

Tento článek popisuje zprávy a přenosu zpracování architektura Bus služby Azure.

## <a name="service-bus-scale-units"></a>Služba Bus jednotkách

Služba Bus uspořádaný podle *jednotkách*. Jednotky měřítka je jednotka nasazení a obsahuje všechny komponenty požadované spuštění služby. Každou oblast nasadí jeden nebo více jednotkách Bus služby.

Obor názvů služby Bus namapovala měřítko jednotku. Jednotky měřítka úchyty pro všechny typy entit služby Bus: relé a brokered zpráv entity (fronty, témata, předplatná). Jednotky měřítka služby Bus se skládá z těchto složek:

- **Nastavení brány uzlů.** Uzly brány ověření příchozí žádosti a obsloužení přenosu požadavků. Každý uzel brány má veřejnou IP adresu.

- **Sada zpráv zprostředkovatele uzlů.** Zasílání zpráv uzly zprostředkovatel zpracovávat požadavky týkající se zasílání zpráv entity.

- **Úložiště jedné brány.** Úložiště brány obsahuje data pro každou entitu, která je definovaná v této jednotce měřítko. Úložiště brány je implementovaná nad k databázi SQL Azure.

- **Více zpráv stores.** Zasílání zpráv ukládají podržte zprávy fronty, témata a předplatné, které jsou definované v této jednotce měřítko. Obsahuje také všechna data předplatného. Pokud je povolené [oddíly, zasílání zpráv entity](service-bus-partitioning.md) , fronty nebo tématu namapovala do jednoho zpráv úložiště. Předplatná jsou uložené v úložišti stejné zpráv jako své téma nadřazené. Kromě služby Bus [Premium zpráv](service-bus-premium-messaging.md)zasílání zpráv ukládají provádění nad databáze SQL Azure.

## <a name="containers"></a>Kontejnery

Každá zpráv entita se přiřadí konkrétní kontejner. Kontejner je logické konstrukce, která používá právě jeden úložiště pro ukládání zpráv všechna příslušná data pro tento kontejner. Každý kontejner přiřazen zpráv uzel zprostředkovatele. Obvykle jsou k dispozici další kontejnery než zpráv zprostředkovatele uzlů. Proto jednotlivých zpráv uzel zprostředkovatel načte více kontejnery. Rozdělení kontejnery zpráv uzel zprostředkovatel jsou uspořádána tak, aby se rovnoměrně z vnější nakládá všech zpráv uzlů zprostředkovatele. Pokud načíst vzorek změny (například nějaká velmi zaneprázdněné kontejnery získá) nebo pokud dočasně nedostupný zpráv uzel zprostředkovatele, kontejnery jsou rozdělí mezi mezi zpráv uzly zprostředkovatele.

## <a name="processing-of-incoming-messaging-requests"></a>Zpracovávání příchozích zpráv požadavků

Pokud klient odešle žádost o službu Bus, Vyrovnávání zatížení Azure nasměruje ji jednotlivých uzlech brány. Uzel brány povoluje žádost. Pokud žádost se týká zpráv entity (fronty, téma, předplatné), uzel brány vyhledá entitu v úložišti brány a určuje, ve které zpráv úložiště entitu nachází. Potom vyhledává který zpráv uzel zprostředkovatel aktuálně zpracovává tento kontejner a odešle žádost zpráv uzel zprostředkovatele. Zasílání zpráv uzel zprostředkovatel žádost zpracuje a aktualizace stavu entity v úložišti kontejner. Zasílání zpráv uzel zprostředkovatel pošle odpověď zpátky na uzel brány, která odešle na příslušnou odpověď zpátky ke klientovi, která vystavila původní žádost o.

![Zpracovávání příchozích zpráv požadavků](./media/service-bus-architecture/IC690644.png)

## <a name="processing-of-incoming-relay-requests"></a>Zpracování příchozí žádosti o přenosu

Pokud klient odešle žádost o službu Bus, Vyrovnávání zatížení Azure nasměruje ji jednotlivých uzlech brány. Žádost o naslouchají při žádosti uzel brány vytvoří nový relay. Žádost o připojení konkrétní přenosu při žádosti uzel brány předá žádost o připojení k uzel brány, který vlastní přenos. Uzel brány, který vlastní přenos odešle žádost o rendezvous naslouchají klientovi, s dotazem, posluchače vytvořit dočasné kanálu, který chcete uzel brány, který dostali žádost o připojení.

Při předávání připojení klientů exchange zprávy prostřednictvím uzel brány, který se používá k rendezvous.

![Zpracování příchozí žádosti o přenosu](./media/service-bus-architecture/IC690645.png)

## <a name="next-steps"></a>Další kroky

Teď jste přečetli Přehled architektury Bus služby začít získáte následující odkazy:

- [Přehled služeb Bus zasílání zpráv](service-bus-messaging-overview.md)
- [Základy Bus služby](service-bus-fundamentals-hybrid-solutions.md)
- [Ve frontě zpráv řešení pomocí služby Bus fronty](service-bus-dotnet-multi-tier-app-using-service-bus-queues.md)
