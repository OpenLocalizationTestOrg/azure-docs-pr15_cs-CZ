<properties 
    pageTitle="Izolační aplikací služby Bus proti výpadků a havárie | Microsoft Azure"
    description="Popisuje postupů, které můžete použít k ochraně aplikací proti potenciální výpadku služby Bus."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="tysonn" /> 
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/02/2016"
    ms.author="sethm" />

# <a name="best-practices-for-insulating-applications-against-service-bus-outages-and-disasters"></a>Doporučené postupy pro izolační proti služby Bus výpadků a havárie aplikací

Kritické aplikace musí průběžně, pracovat i přítomnosti neplánované výpadků nebo havárie. Toto téma popisuje postupů, které můžete použít k ochraně aplikací služby Bus proti potenciální výpadku služby nebo havárie.

Výpadku je definována jako dočasné nedostupnosti Bus služby Azure. Výpadku může ovlivnit některé součásti Bus služby, třeba úložištěm zasílání zpráv nebo dokonce celý datacentra. Po problém, služby Bus znovu k dispozici. Obvykle výpadku nezpůsobí ztrátu zpráv nebo jiná data. Příklad Chyba součásti je nedostupnost úložištěm konkrétní zasílání zpráv. Příklad výpadku datacentra celé je dodávky elektrického proudu datacentra nebo přepínače sítě vadný datacentra. Výpadku může trvat několik minut až několik dnů.

Selhání je definována jako trvalou ztrátu jednotku měřítko služby Bus nebo datacentra. Datacentra může nebo nemusí opět k dispozici. Obvykle způsobí selhání ztrátě některých nebo všech zpráv nebo jiná data. Příklady havárie jsou fire zaplavení nebo zemětřesení.

## <a name="current-architecture"></a>Aktuální architektura

Služba Bus používá více zpráv ukládají k uložení zprávy, které jsou odeslány fronty nebo témata. Bez oddílů fronty nebo tématu přiřazen jeden zpráv úložiště. Pokud toto zpráv úložiště není k dispozici, všechny operace na danou fronty nebo téma se nezdaří.

Všechny entity Bus služeb zasílání zpráv (fronty, témata, relé) uložena v názvů služby, který je přidružen datacentra. Služby Bus neumožňuje automatické geo replikace dat ani nepovoluje obor zahrnovat více datacentrech.

## <a name="protecting-against-acs-outages"></a>Ochrana proti ACS výpadků

Pokud používáte ACS pověření a ACS nebude k dispozici, můžete už klientů tokeny. Klienti, které mají v době, kdy ACS havaruje můžete dál používat službu Bus dokud tokeny platnost. Životnost tokenů výchozí je 3 hodiny.

K ochraně proti ACS výpadků, použijte tokeny sdílené přidružení (zabezpečení aplikace Access podpis). V tomto případě klienta ověřuje přímo pomocí služby Bus tak, že se vlastním minted token tajné klíčem. Volání ACS jsou už není potřeba. Další informace o tokeny přidružení zabezpečení najdete v článku [Bus Služba ověřování][].

## <a name="protecting-queues-and-topics-against-messaging-store-failures"></a>Ochrana fronty a témata týkající se zasílání zpráv selhání úložiště přihlašovacích údajů

Bez oddílů fronty nebo tématu přiřazen jeden zpráv úložiště. Pokud toto zpráv úložiště není k dispozici, všechny operace na danou fronty nebo téma se nezdaří. Rozdělený fronty na druhé straně obsahuje více neúplné. Každou část uložený v jiném zpráv úložišti. Po odeslání zprávy do oddílů fronty nebo tématu služby Bus přiřadí zprávu jednu fragmenty. Pokud odpovídající zpráv úložiště je k dispozici, služby Bus data zapisuje zprávu do jiné fragment Pokud je to možné. Další informace o rozdělený entity najdete v článku [oddíly zpráv entity][].

## <a name="protecting-against-datacenter-outages-or-disasters"></a>Ochrana proti datacentra výpadků nebo havárie

Umožňuje pro přepojení mezi dvěma datacentrech můžete vytvořit obor názvů služby Bus služby v jednotlivých datacentra. Například služba Bus služby názvů **contosoPrimary.servicebus.windows.net** mohou být umístěny v oblasti Spojené státy Severní/střední a **contosoSecondary.servicebus.windows.net** mohou být umístěny v oblasti nám jih/webu Enomcentral. Pokud služby Bus zpráv entity musí zůstat přístupných osobám s postižením přítomnosti výpadku datacentru, můžete vytvořit entity v obou obory názvů.

Další informace naleznete v části "Chyba služby Bus v Azure datacentra" [asynchronní vzory zasílání zpráv][]a dostupnost.

## <a name="protecting-relay-endpoints-against-datacenter-outages-or-disasters"></a>Ochrana koncové body relay proti datacentra výpadků nebo havárie

GEO replikace koncové body relay umožňuje služba, která poskytuje relay koncový bod být přístupný přítomnosti výpadků Bus služby. Dosáhnout geo replikace službu vytvoříte dva relay koncové body v různé obory názvů. Obor názvů musí nacházet v různých datacentrech a dva koncové body musí mít různé názvy. Například primární koncový bod jsou dostupné v části **contosoPrimary.servicebus.windows.net/myPrimaryService**při jeho sekundární protějšku zastižení v části **contosoSecondary.servicebus.windows.net/mySecondaryService**.

Služba potom sleduje oba koncové body a klienta můžete volat služby prostřednictvím buď koncového bodu. Klientská aplikace náhodně zvolí jednu relé jako primární koncového bodu a odešle žádost aktivní koncový bod. Pokud se nezdaří s kódem chyby, tato chyba označuje koncový bod relay není k dispozici. Aplikace otevře kanálu, který chcete záložní koncového bodu a znovu vydá žádost. V tomto okamžiku aktivní a zálohování koncové body přepínače role: klientské aplikace byly použity starý aktivní koncový bod nové záložní koncového bodu a starý záložní koncový bod jako nový aktivní koncový bod. Pokud obě odeslat nezdaří, role dvě entity zůstat beze změny a zobrazí se chyba.

Ukázka [Geo replikace u těch, které služby Bus přenos][] demonstruje replikovat relé.

## <a name="protecting-queues-and-topics-against-datacenter-outages-or-disasters"></a>Ochrana fronty a témata proti datacentra výpadků nebo havárie

Abyste dosáhli odolnost proti datacentra výpadků při použití brokered zasílání zpráv, služby Bus podporuje dvěma způsoby: *aktivní* a *Pasivní* replikace. Pro každý přístup Pokud daný fronty nebo tématu musí zůstat přístupných osobám s postižením přítomnosti výpadku datacentra můžete vytvořit ho v obou obory názvů. Obě entity mohou mít stejný název. Například primární fronty jsou dostupné v části **contosoPrimary.servicebus.windows.net/myQueue**při jeho sekundární protějšku zastižení v části **contosoSecondary.servicebus.windows.net/myQueue**.

Pokud aplikace nevyžaduje trvalé komunikace odesílatele sluchátko, aplikace implementovat trvalé fronty klientských předešli ztrátě zprávy a chrání odesílatele z libovolné přechodná chyby Bus služby.

## <a name="active-replication"></a>Aktivní replikace

Aktivní replikace používá entity v obou obory názvů pro všechny operace. Klient, která odešle zprávu pošle dvě kopie stejnou zprávu. První kopie odeslaný do primární entity (například **contosoPrimary.servicebus.windows.net/sales**) a druhou kopii zprávy se neodesílají sekundární entity (například **contosoSecondary.servicebus.windows.net/sales**).

Klient obdrží zprávy z obou dotazů. Příjemce zpracuje první kopie zprávy a potlačit druhá kopie. Potlačí duplicitní zprávy, musíte odesílatele označení všech zpráv označen jedinečný identifikátor. Obě kopie zprávy musí být označené identifikátor stejné. Vlastnosti [BrokeredMessage.MessageId][] nebo [BrokeredMessage.Label][] nebo vlastních vlastností slouží k označení zprávy. Příjemce musí spravovat seznam zpráv, které už obdržel.

[Geo replikace služby Bus Brokered zpráv][] příklad znázorňuje aktivní replikace zpráv entity.

> [AZURE.NOTE] Přístup aktivní replikace zdvojnásobí počet operací, proto může vést tento přístup k vyšší náklady.

## <a name="passive-replication"></a>Trpný replikace

V případě uvolnit poruch pasivní replikace používá pouze jednu ze dvou entit, zasílání zpráv. Klient odešlete zprávu s aktivní entity. Pokud se u aktivní entity nezdaří s kódem chyby, která označuje, že datacentru, který je hostitelem aktivní entity může být k dispozici, klient odešle kopii zprávy záložní entity. V tomto okamžiku aktivní a zálohování entity přepnout role: odesílání klienta považovat za starý aktivní entitu nová záložní entita a starý záložní entita je nové aktivní entity. Pokud obě odeslat nezdaří, role dvě entity zůstat beze změny a zobrazí se chyba.

Klient obdrží zprávy z obou dotazů. Protože je pravděpodobné, že příjemce obdrží dvě kopie stejná zpráva, příjemce musí potlačení duplicitní zprávy. Můžete potlačit duplicitní položky stejným způsobem popsaným aktivní replikace.

Obecně pasivní replikace totiž výhodnější než aktivní replikace ve většině případů jedinou operace. Scénář – replikovat shodná latence, výkon a peněžní náklady.

Když používáte pasivní replikace, v následujících situacích zprávy můžete být ztracené nebo přijatá dvakrát:

-   **Zpoždění zprávy nebo ztráty**: předpokládá, že odesílatel úspěšně odeslané zprávy m1 primární fronty, a potom fronty není k dispozici před příjemce obdrží m1. Odesílatel pošle pozdější zprávy m2 sekundární fronty. Pokud primární fronty je dočasně nedostupný, příjemce obdrží m1 po fronty znovu k dispozici. V případě havárie příjemce obdržet nikdy m1.

-   **Duplikování recepci**: se předpokládá, že odesílatel odešle zprávu m primární fronty. Služba Bus úspěšně zpracovává m, ale nepodaří odeslat odpověď. Po operaci odesílání vypršení časového limitu, odešle odesílatele identickými kopii m sekundární fronty. Pokud příjemce je nedostáváte první kopii m před primární fronty nebude k dispozici, příjemce obdrží obě kopie m přibližně najednou. Pokud příjemce není nedostáváte první kopii m před primární fronty nebude k dispozici, příjemce původně přijímá jenom druhou kopii m, ale pak obdrží kopii m až primární fronty bude dostupný.

[Replikace Geo služby Bus Brokered zpráv][] příklad znázorňuje pasivní replikace zpráv entity.

## <a name="next-steps"></a>Další kroky

Další informace o obnovení havárie, najdete v těchto článcích:

- [Nepřerušený provoz databáze Azure SQL][]
- [Azure odolnost proti chybám příručku][]

  [Ověření Bus služby]: service-bus-authentication-and-authorization.md
  [Rozdělený entity zasílání zpráv]: service-bus-partitioning.md
  [Asynchronní zpráv vzorů a dostupnost]: service-bus-async-messaging.md#failure-of-service-bus-within-an-azure-datacenter
  [Replikace GEO pomocí služby Bus přenos zprávy]: http://code.msdn.microsoft.com/Geo-replication-with-16dbfecd
  [BrokeredMessage.MessageId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx
  [BrokeredMessage.Label]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx
  [Replikace GEO pomocí služby Bus Brokered zprávy]: http://code.msdn.microsoft.com/Geo-replication-with-f5688664
  [Nepřerušený provoz databáze Azure SQL]: ../sql-database/sql-database-business-continuity.md
  [Azure odolnost proti chybám příručku]: ../resiliency/resiliency-technical-guidance.md
