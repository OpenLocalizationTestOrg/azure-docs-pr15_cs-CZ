<properties 
    pageTitle="Azure fronty a služby Bus fronty – porovnání a kontrastní | Microsoft Azure"
    description="Analyzuje rozdíly a podobnosti mezi dvěma druhy fronty nabízená Azure."
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
    ms.workload="tbd"
    ms.date="09/23/2016"
    ms.author="sethm" />

# <a name="azure-queues-and-service-bus-queues---compared-and-contrasted"></a>Azure fronty a služby Bus fronty – porovnání a kontrastní

Tento článek analyzuje rozdíly a podobnosti mezi dvěma druhy fronty nabízená Microsoft Azure dnes: frontách Azure fronty a Bus služby. Pomocí těchto informací můžete porovnat a kontrastu odpovídajících technologií a moct kvalifikovaně rozhodování o řešení nejlépe odpovídá vašim potřebám.

## <a name="introduction"></a>Úvod

Microsoft Azure podporuje dva typy fronty mechanismy: **Azure fronty** a **Fronty Bus služby**.

**Azure fronty**, které jsou součástí Infrastruktura [Azure úložiště](https://azure.microsoft.com/services/storage/) , funkce jednoduché rozhraní REST základě Get/umístění/náhled poskytující spolehlivé, trvalý zasílání zpráv v rámci a mezi službami.

**Fronty Bus služby** jsou součástí širší [Azure zpráv](https://azure.microsoft.com/services/service-bus/) infrastrukturu, který podporuje řízení fronty a publikovat a přihlášení k odběru webové služby Vzdálená a integrace vzorků. Další informace o fronty Bus služby, témata a předplatné a relé najdete v článku [zasílání zpráv služby Bus – přehled](service-bus-messaging-overview.md).

Během obě fronty technologie jsou současně, a Azure fronty byly vydané nejdřív jako mechanismus úložiště vyhrazené fronty této technologii postavená služby Azure úložiště. Integrované služby Bus fronty nad širší "brokered zpráv" infrastruktury navržen pro integraci aplikace nebo součásti aplikace, které může zahrnovat více komunikační protokoly, data smlouvy důvěřovat doméně, a/nebo sítě prostředí.

Tento článek porovnává dvě fronty technologie nabízená v Azure projednávat rozdílech mezi chování a provádění funkce poskytované každý. Článek obsahuje taky pokyny pro výběr funkcí, které může podle svých potřeb vývoj aplikací.

## <a name="technology-selection-considerations"></a>Důležité informace o technologii výběru

Azure fronty a služby Bus fronty jsou implementací služby, které aktuálně nabízíme na Microsoft Azure pro zpracování zpráv. Má každá sadu mírně odlišné funkce, což znamená, že můžete zvolit jeden z nich nebo použít obojí v závislosti na potřeb konkrétní řešení nebo obchodní/technické potíže, které jsou řešení.

Při určování, které fronty technologii vešel účel pro dané řešení, měli byste zvážit následující doporučení tvůrci řešení a vývojáři. Další informace najdete v další části.

Řešení Tvůrce/vývojář, **měli byste zvážit použití Azure fronty** při:

- Aplikace musí obsahují více než 80 GB zpráv ve frontě, kde zprávy mít životnost kratší než 7 dnů.

- Aplikace se chce sledovat průběh zpracování zprávy uvnitř fronty. Toto je užitečné, když dojde k chybě pracovního zpracování zprávy. Pozdější pracovní pak můžete tyto informace pokračovat v místě, kde předchozí pracovníka skončili.

- Požadujete protokoly straně serveru všech transakce spouštět oproti vaší fronty.

Řešení Tvůrce/vývojář, **měli byste zvážit použití služby Bus fronty** při:

- Řešení musí být moct přijímat zprávy bez nutnosti hlasování fronty. S Bus služby toho můžete dosáhnout použitím dlouhé hlasování přijímání operace pomocí protokolu TCP protokolů, které podporuje Bus služby.

- Řešení vyžaduje fronty poskytovat zaručené první in-first-out (FIFO) objednali doručení.

- Chcete symetrickou setkat i v případě v Azure a v systému Windows Server (soukromé cloudu). Další informace najdete v tématu [Bus služby systému Windows Server](https://msdn.microsoft.com/library/dn282144.aspx).

- Řešení musíte mít na podporu automatické duplicit.

- Má vaše aplikace zpracování zpráv jako paralelní datových proudů dlouho probíhajících (zprávy souvisí s datovými proudy pomocí vlastnosti [ID relace](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx) ve zprávě). V tomto modelu každý uzel v aplikaci náročný soutěží pro datové proudy namísto zprávy. Při proudu náročný uzel, uzel můžete zkontrolovat stav stavu toku aplikace pomocí transakce.

- Řešení vyžaduje transakční chování a nedělitelnost při odesílání a přijímání více zpráv z fronty.

- Typické time to live (TTL) pro pracovní zátěž specifické pro aplikaci můžete překročit 7 dní.

- Aplikace zpracování zpráv, které je větší než 64 KB ale omezí není pravděpodobně přístup 256 KB.

- Zpracovat žádost o zadání modelu přístupu na základě rolí fronty a jiná oprávnění a oprávnění pro odesílatele a příjemce.

- Velikost fronty nebude postupně se zvětšují větší než 80 GB.

- Chcete-li protokol AMQP 1.0 standardy zasílání zpráv. Další informace o AMQP najdete v tématu [Přehled AMQP Bus služby](./service-bus-amqp-overview.md).

- Můžete výsledcích případné migrace od založených na frontách bod k komunikace vzorek zprávu exchange, který umožňuje Bezproblémová integrace další příjemci (účastníci), z nichž každý obdrží nezávisle na kopii některé nebo všechny zprávy odeslané do fronty. Ten odkazuje na funkci publikování a přihlášení k odběru nativně poskytovanou Bus služby.

- Zasílání zpráv řešení musíte mít na podporu garance doručení "U většiny – jedno" bez nutnosti můžete vytvořit další infrastrukturu součásti.

- Rádi byste mohli publikovat a používání listy zpráv.

- Potřebujete úplnou integrace se službou Windows Communication Foundation (WCF) zásobníku komunikace .NET Framework.

## <a name="comparing-azure-queues-and-service-bus-queues"></a>Porovnání Azure fronty a služby Bus fronty

Tabulky v následujících částech poskytují logické seskupení fronty funkcí a umožňují porovnání na první pohled, možnosti dostupné v Azure fronty a fronty Bus služby.

## <a name="foundational-capabilities"></a>Základní funkce

V této části porovnává některé základní fronty funkce dostupné ve frontách Azure fronty a Bus služby.

|Kritéria porovnávání|Azure fronty|Service Bus fronty|
|---|---|---|
|Řazení záruky|**Ne** <br/><br>Další informace najdete v tématu první poznámku v části "Další informace o".</br>|**Ano - první v první ven (FIFO)**<br/><br>(pomocí zasílání zpráv relace)|
|Záruky doručení|**Na aspoň jednou**|**Na aspoň jednou**<br/><br/>**Jednou na většině**|
|Podpora atomová operace|**Ne**|**Ano**<br/><br/>|
|Příjem chování|**Bez blokování**<br/><br/>(dokončí okamžitě Pokud nenajdou žádné nové zprávy)|**Blokování s vypršení časového limitu nebo bez nich**<br/><br/>(nabídky dlouhé hlasování nebo ["Comet postup"](http://go.microsoft.com/fwlink/?LinkId=613759))<br/><br/>**Bez blokování**<br/><br/>(použitím .NET spravované rozhraní API pouze)|
|Rozhraní API nabízených stylu|**Ne**|**Ano**<br/><br/>[OnMessage](https://msdn.microsoft.com/library/azure/jj908682.aspx) a **OnMessage** relací rozhraní API .NET.|
|Příjem režimu|**Prohlížet a poskytovat na leasing**|**Náhled a zámek**<br/><br/>**Zobrazit a odstranit**|
|Režim výhradní přístup|**Na základě zapůjčení**|**Na základě zámek**|
|Zapůjčení/Lock doba trvání|**30 sekund (výchozí)**<br/><br/>**7 dní (maximální)** (Můžete prodloužit nebo uvolněte zapůjčenou zprávu pomocí [UpdateMessage](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.updatemessage.aspx) rozhraní API.)|**60 sekund (výchozí)**<br/><br/>Můžete prodloužit zámek zprávu pomocí [RenewLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.renewlock.aspx) rozhraní API.|
|Zapůjčení/Lock přesností|**Úroveň zprávy**<br/><br/>(každá zpráva můžete mít hodnotu různých limitu, který pak lze aktualizovat podle potřeby při zpracování zprávy pomocí [UpdateMessage](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.updatemessage.aspx) rozhraní API)|**Úroveň fronty**<br/><br/>(jednotlivé fronty má zamknout přesnost použít u všech své zprávy, ale můžete prodloužit zamknout pomocí [RenewLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.renewlock.aspx) rozhraní API.)|
|Dávce přijímání|**Ano**<br/><br/>(explicitně určující počet zpráv při načítání zpráv maximální hodnota je 32 zpráv)|**Ano**<br/><br/>(implicitně povolení vlastnost před načtení nebo explicitně pomocí transakce)|
|Dávkový odeslat|**Ne**|**Ano**<br/><br/>(použitím transakce nebo dávky klienta)|

### <a name="additional-information"></a>Další informace

- Zprávy ve frontách Azure jsou obvykle první v – první ven, ale někdy může být mimo pořadí; například trvání časového limitu zprávy viditelnost vypršení platnosti (například důsledku klientské aplikace k selhání během zpracování). Po vypršení časového limitu viditelnost zprávy znovu ve frontě pro jiné pracovníka dequeue je viditelná. V tomto okamžiku nově viditelné zprávy může umístí do fronty (třeba vyřazeno z fronty znovu) za zprávu, která byla původně byla zařazena do fronty za ni.

- Pokud už používáte Azure úložiště objektů BLOB nebo tabulky a můžete začít používat fronty, je zaručena dostupnost 99,9 %. Pokud používáte objektů BLOB nebo tabulek s služby Bus fronty, bude mít nižší dostupnosti.

- Zaručené vzorek FIFO ve frontách služby Bus vyžaduje použití relací zasílání zpráv. V případě, že aplikace dojde k chybě při zpracování zprávy přijaté v režimu **náhledu & Zamknout** , při příštím fronty sluchátko přijme relaci zasílání začne zprávou o nezdařeném uložení po uplynutí jeho období time to live (TTL).

- Azure fronty slouží k podpoře standardní fronty scénáře, jako například oddělovací součástí aplikace větší škálovatelnost a odchylku pro selhání vyrovnání a konečně sestavení proces pracovní postupy.

- Služba Bus fronty domovské stránce podpory garance doručení *U aspoň jednou* . *Jednou na většina* sémantického kromě toho můžete podporovaná, použitím stav relace k uložení stavu aplikace a pomocí transakce atomicky přijímat zprávy a aktualizovat stav relace.

- Azure fronty poskytují jednotné a konzistentní programovací model přes fronty, tabulky a objekty BLOB – pro vývojáře i pro operace týmy.

- Služba Bus fronty podporují místní transakce v kontextu jedné fronty.

- **Příjem a odstranění** režimu nepodporuje služby Bus umožňuje zmenšit počet zpráv operace (a odpovídající náklady) za assurance snížené doručení.

- Azure fronty zapůjčení poskytnout možnost rozšiřte zapůjčených zprávy. Díky zaměstnanců, kteří mají spravovat krátké zapůjčení zpráv. Tím Pokud dojde k chybě pracovního, zprávy můžete rychle zpracovat znovu jiného pracovníka. Kromě toho pracovník můžete rozšířit zápůjčku zprávy, pokud je nutné zpracovat jeho delší než aktuální čas zapůjčení.

- Azure fronty nabízejí viditelnost časový limit, můžete nastavit po enqueueing nebo vyřazení zprávy. Kromě toho můžete aktualizovat zprávy s hodnotami různých zapůjčení za běhu a různé hodnoty, aktualizovat všude zprávy ve frontě stejné. Služba Bus lock vypršení časového limitu jsou definované v metadatech fronty; uzamčení však můžete prodloužit tak, že zavoláte metodu [RenewLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.renewlock.aspx) .

- Maximální limit blokování přijímání operace ve frontách Bus služby je 24 dní. Na základě ZBÝVAJÍCÍ časové limity však mít maximální hodnotu 55 sekund.

- Klientské dávky poskytovaných služeb Bus klientovi fronty dávkové více zpráv do jednoho odeslat operace. Dávkové je dostupná jenom pro operace asynchronní odeslat.

- Funkce, jako jsou ceiling 200 TB Azure fronty (více při při virtualizaci účty) a neomezený fronty usnadnit platformu ideální pro SaaS poskytovatele.

- Azure fronty poskytují flexibilní a performant delegovat mechanismus řízení přístupu.

## <a name="advanced-capabilities"></a>Rozšířené možnosti

V této části porovnává pokročilých funkcí poskytovanou frontách Azure fronty a Bus služby.

|Kritéria porovnávání|Azure fronty|Service Bus fronty|
|---|---|---|
|Plánované doručení|**Ano**|**Ano**|
|Automatické nefunkční písma|**Ne**|**Ano**|
|Zvětšení hodnota time-to-live fronty|**Ano**<br/><br/>(přes místní aktualizace časového limitu viditelnost)|**Ano**<br/><br/>(poskytovat pomocí vyhrazené funkce rozhraní API)|
|Poškodit podpora zpráv|**Ano**|**Ano**|
|Aktualizace na místě|**Ano**|**Ano**|
|Serverový transakční protokol|**Ano**|**Ne**|
|Metriky úložišť|**Ano**<br/><br/>**Funkce MINUTE metriky**: poskytuje v reálném čase metriky pro rozhraní API dostupnost, TPS, volání počty počty chyby a informace najdete v reálném čase (agregované za minutu a vykázaného objevit během pár minut z právě příčinách ve výrobním. Další informace najdete v tématu [O metriky úložišť analýzy](https://msdn.microsoft.com/library/azure/hh343258.aspx).|**Ano**<br/><br/>(hromadné dotazy tak, že zavoláte [GetQueues](https://msdn.microsoft.com/library/azure/hh293128.aspx))|
|Státní správa|**Ne**|**Ano**<br/><br/>[Microsoft.ServiceBus.Messaging.EntityStatus.Active](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx), [Microsoft.ServiceBus.Messaging.EntityStatus.Disabled](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx) [Microsoft.ServiceBus.Messaging.EntityStatus.SendDisabled](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx), [Microsoft.ServiceBus.Messaging.EntityStatus.ReceiveDisabled](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx)|
|Automatické předávání zpráv|**Ne**|**Ano**|
|Vymazání fronty (funkce)|**Ano**|**Ne**|
|Zprávy skupiny|**Ne**|**Ano**<br/><br/>(pomocí zasílání zpráv relace)|
|Stav aplikace jednotlivé zprávy skupiny|**Ne**|**Ano**|
|Duplicit|**Ne**|**Ano**<br/><br/>(konfigurovat na straně odesílatele)|
|Integrace WCF|**Ne**|**Ano**<br/><br/>(nabízí vazby mimo pole WCF)|
|Integrace Bránu|**Vlastní**<br/><br/>(vyžaduje vytváření s vlastní aktivitou Bránu)|**Nativní**<br/><br/>(nabízí mimo pole Bránu aktivity)|
|Procházení zprávy skupiny|**Ne**|**Ano**|
|Načítání relace zpráv podle ID|**Ne**|**Ano**|

### <a name="additional-information"></a>Další informace

- Obě fronty technologie povolit zprávy naplánovaný pro doručování později.

- Automatické přeposílání fronty umožňuje tisíce fronty a jednotlivých zpráv do jediné fronty, ze kterého přijímání aplikace využívá zprávy automaticky předat dál. Použijte tento postup pro dosažení zabezpečení, řízení toku a izolovat úložiště mezi vydavatele zprávy.

- Azure fronty podporují aktualizace obsah zprávy. Můžete tuto funkci pro zachování informace o stavu a aktualizace průběhu dílčích do zprávy tak, že můžete zpracovány z poslední známé kontroly, abyste nemuseli začínat od začátku. Pomocí služby Bus fronty můžete povolit stejný scénář použitím relace zpráv. Relace umožňují uložit a získat stav zpracování aplikace (pomocí [SetState](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.setstate.aspx) a [GetState](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.getstate.aspx)).

- [Nefunkční písma](service-bus-dead-letter-queues.md), která je podporována pouze služby Bus fronty, může být užitečné pro izolace zprávy, které nelze úspěšně zpracovat přijímání aplikací nebo zpráv nelze dorazí do cíle kvůli vypršela platnost time to live (TTL) vlastnost aplikace. Hodnota TTL určuje, jak dlouho zůstane zpráva ve frontě. Pomocí služby Bus budou zprávy přesunuty do zvláštní fronty s názvem $DeadLetterQueue, když skončí platnost doby TTL.

- "Poškozená" zprávy ve frontách Azure při najít vyřazení zprávy aplikace kontroluje vlastnost **[DequeueCount](https://msdn.microsoft.com/library/azure/dd179474.aspx)** zprávy. Pokud **DequeueCount** nad dané mezní hodnoty, aplikace přesune zprávu do fronty definované aplikací "nepoužívaných písmeno".

- Azure fronty umožňují získat podrobný protokol všechny transakce spouštět oproti fronty jako dobře souhrnné metriky. Obě tyto možnosti jsou vhodné k ladění a vysvětlení, jak aplikace používá fronty Azure. Jsou také užitečné pro aplikace ladění výkonu a snížit náklady na použití fronty.

- Koncept "relace zpráv" nepodporuje služby Bus umožňuje zprávy, které patří do určité logické skupinu, do které být přidružené k dané sluchátko, která zase vytvoří spřažení profesionálové relace mezi zpráv a jejich odpovídajících příjemce. Povolit tento rozšířené funkce služby Bus nastavením vlastnosti [ID relace](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx) na zprávu. Příjemci můžete naslouchají relacím na ID konkrétní relace a přijímat zprávy, která sdílejí identifikátor zadaný relace.

- Funkce rozpoznávání duplikace podporované ve frontách služby Bus automaticky odstraní duplicitní zprávy odeslané do fronty nebo téma, na základě hodnoty vlastnost [MessageID](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) .

## <a name="capacity-and-quotas"></a>Funkce a kvót

V této části porovnává Azure fronty a služby Bus fronty z hlediska [kapacitu a kvót](service-bus-quotas.md) které může použít.

|Kritéria porovnávání|Azure fronty|Service Bus fronty|
|---|---|---|
|Maximální velikost fronty|**200 TB**<br/><br/>(pouze pro jeden účet kapacitou)|**1 GB 80 GB**<br/><br/>(definované při vytváření fronty a [Povolení rozdělení](service-bus-partitioning.md) – najdete v části "Dodatečné informace")|
|Maximální velikost zprávy|**64 KB**<br/><br/>(48 znalostní bázi Knowledge Base když kódováním **ve formátu Base 64** )<br/><br/>Azure podporuje dlouhé zprávy zkombinováním fronty a objekty BLOB – v tomto okamžiku můžete provést tyto akce enqueue až 200GB pro jednu položku.|**256 KB** nebo **1 MB**<br/><br/>(včetně záhlaví a textu záhlaví maximální velikost: 64 KB).<br/><br/>Závisí na [vrstvy služeb](service-bus-premium-messaging.md).|
|Maximální hodnota TTL zprávy|**7 dnů**|**`TimeSpan.Max`**|
|Maximální počet dotazů|**Neomezený**|**10 000**<br/><br/>(za obor názvů služby může být lepší)|
|Maximální počet souběžné klientů|**Neomezený**|**Neomezený**<br/><br/>(100 souběžné připojení omezení platí jenom pro komunikaci založené na protokolu TCP)|

### <a name="additional-information"></a>Další informace

- Služba Bus vynucuje limity velikosti fronty. Maximální velikost fronty je určen při vytváření fronty a může mít hodnotu mezi 1 a 80 GB. Je-li velikost fronty nastavenou na vytvoření fronty, odmítnuty další příchozích zpráv a výjimku obdrží volající kód. Další informace o kvótách v Bus služby najdete v článku [Kvóty Bus služby](service-bus-quotas.md).

- Můžete vytvořit služby Bus fronty o velikosti 1, 2, 3, 4 a 5 GB (výchozí hodnota je 1 GB). Pomocí rozdělení povolena (což je výchozí nastavení), služby Bus vytvoří 16 oddíly pro každou GB zadáte. Jako například pokud vytváříte fronta, v níž je 5 GB velikosti 16 oddílů maximální velikost fronty stane (5 * 16) = 80 GB. Zobrazí se maximální velikosti doručovaných rozdělený fronty nebo tématu pohledem na vstupu na [Azure portálu][].

- S Azure fronty nejsou-li obsah zprávy XML XLL s podporou, pak musí být **ve formátu Base 64** kódování. Pokud jste **ve formátu Base 64**-kódovat zprávu, uživatel může být až 48 KB místo 64 KB.

- Pomocí služby Bus fronty každá zpráva uložená ve frontě se skládá ze dvou částí: záhlaví a obsahu. Celková velikost zprávy nesmí překročit maximální velikost zprávy nepodporuje vrstvy služeb.

- Když klienty služby Bus fronty komunikujete prostřednictvím protokolu TCP, maximální počet souběžné připojení k jedné fronty služby Bus se omezí na 100. Toto číslo se budou sdílet mezi odesílatele a příjemce. Pokud tento přiděleného odmítnuty následující požadavky na další připojení a výjimku obdrží volající kód. Toto omezení není uložena v klientech připojení k fronty pomocí rozhraní REST API.

- Pokud požadujete víc než 10 000 fronty v jedné názvů Bus služby, můžete obraťte se na tým podpory Azure a požádat o zvýšení. Zobrazit za 10 000 fronty s Bus služby, můžete taky vytvořit další obory názvů pomocí [Azure portálu][].

## <a name="management-and-operations"></a>Správa a provoz

V této části porovnává funkce správy dostupné ve frontách Azure fronty a Bus služby.

|Kritéria porovnávání|Azure fronty|Service Bus fronty|
|---|---|---|
|Správa Protocol (protokol)|**UKAZATELE protokolu HTTP/HTTPS**|**UKAZATELE HTTPS**|
|Modul runtime Protocol (protokol)|**UKAZATELE protokolu HTTP/HTTPS**|**UKAZATELE HTTPS**<br/><br/>**AMQP 1.0 standardní (TCP s TLS)**|
|Spravované rozhraní API .NET|**Ano**<br/><br/>(.NET spravované rozhraní API úložiště klienta)|**Ano**<br/><br/>(.NET spravovaných brokered zpráv rozhraní API)|
|Nativní C++|**Ano**|**Ne**|
|Rozhraní API Java|**Ano**|**Ano**|
|ROZHRANÍ API PHP|**Ano**|**Ano**|
|Node.js rozhraní API|**Ano**|**Ano**|
|Podpora libovolného metadat|**Ano**|**Ne**|
|Pravidla pojmenování fronty|**Až 63 znaků**<br/><br/>(Písmena ve frontě název musí být malá písmena).|**Až 260 znaků**<br/><br/>(Fronty cesty a názvy jsou malá a velká písmena).|
|Funkce délka fronty GET|**Ano**<br/><br/>(Přibližnou hodnotu, pokud zprávy platnost za hodnota TTL bez odstraněním.)|**Ano**<br/><br/>(V okamžiku, přesné hodnoty.)|
|Náhled (funkce)|**Ano**|**Ano**|

### <a name="additional-information"></a>Další informace

- Azure fronty podporují libovolného atributy, které se dají použít pro popis fronty ve formě dvojice název hodnota.

- Obě technologie fronty nabízí možnost prohlížet zprávy bez nutnosti uzamknout, což je užitečné při provádění nástroji fronty explorer/prohlížeče.

- .NET Bus služby brokered zpráv rozhraní API účinek plně duplexní připojení TCP pro lepší výkon při porovnání s ZBÝVAJÍCÍ přes HTTP a podporují standardní protokol AMQP 1.0.

- Názvy Azure fronty může být 3 63 znaků dlouho, může obsahovat malá písmena, číslice a spojovníky. Další informace najdete v tématu [pojmenování fronty a Metadata](https://msdn.microsoft.com/library/azure/dd179349.aspx).

- Služba Bus fronty názvy lze až 260 znaků a používat méně omezující naming pravidla. Služba Bus fronty názvy mohou obsahovat písmena, čísla, tečky, pomlčky a podtržítka.

## <a name="authentication-and-authorization"></a>A mohli ověřovat

Tato část popisuje a tak mohli ověřovat funkce podporované jednotlivými frontách Azure fronty a Bus služby.

|Kritéria porovnávání|Azure fronty|Service Bus fronty|
|---|---|---|
|Ověřování|**Symetrickou klíč**|**Symetrickou klíč**|
|Model zabezpečení|Delegovaná přístup prostřednictvím tokeny přidružení zabezpečení.|PŘIDRUŽENÍ ZABEZPEČENÍ|
|Zprostředkovatel federaci identity|**Ne**|**Ano**|

### <a name="additional-information"></a>Další informace

- Každý požadavek buď fronty technologií musí ověřit. Veřejné fronty při anonymním přístupu nejsou podporované. Pomocí [přidružení zabezpečení](service-bus-sas-overview.md), můžete řešit jenom tento scénář publikováním jen pro zápis přidružení zabezpečení, přidružení zabezpečení jen pro čtení nebo dokonce úplný přístup přidružení zabezpečení.

- Režim ověřování poskytovanou Azure fronty zahrnuje použití symetrickou klávesy, která je základě hash zprávy ověřování kód (HMAC), spočítat pomocí algoritmu SHA 256 a kódovaný jako řetězec **ve formátu Base 64** . Další informace o odpovídajících Protocol (protokol) najdete v článku [ověření úložiště služby Azure](https://msdn.microsoft.com/library/azure/dd179428.aspx). Služba Bus fronty domovské stránce podpory podobné modelu pomocí symetrickou klíče. Další informace najdete v tématu [Sdílené ověřování přístupu k podpisu Bus služby](service-bus-shared-access-signature-authentication.md).

## <a name="cost"></a>Pole náklady

V této části porovnává Azure fronty a služby Bus fronty z hlediska nákladů.

|Kritéria porovnávání|Azure fronty|Service Bus fronty|
|---|---|---|
|Fronta transakce náklady|**$0.0036**<br/><br/>(cena za 100 000 transakce)|**Základní osy**: **$0,05**<br/><br/>(cena za milionů operace)|
|Fakturaci operace|**Všechny**|**Odesílání a přijímání pouze**<br/><br/>(Bezplatná dalších operací)|
|Nečinné transakce|**Fakturaci**<br/><br/>(Dotazování prázdné fronty se počítá jako fakturaci transakce.)|**Fakturaci**<br/><br/>(Přijímání proti prázdné fronty se považuje za zprávu fakturaci.)|
|Pole náklady úložiště|**$0.07**<br/><br/>(cena za GB/měsíc)|**0,00 Kč**|
|Výstupní data převést náklady|**$0.12 - $0.19**<br/><br/>(V závislosti na geography.)|**$0.12 - $0.19**<br/><br/>(V závislosti na geography.)|

### <a name="additional-information"></a>Další informace

- Přenosy dat bude účtovaná podle celkovou kapacitu dat byste museli odcházet z Azure datacentrech prostřednictvím Internetu v daném fakturační období.

- Přenosy dat mezi Azure služby umístěny ve stejné oblasti nejsou vyměřené poplatky.

- K této psaní nejsou všechny přenosy příchozí dat vyměřené poplatky.

- Možnost podpory pro dlouhé hlasování, pomocí služby Bus fronty může být nákladů efektivní v situacích, kdy nízkou latencí doručení povinný.

>[AZURE.NOTE] Všechny náklady, které se mohou změnit. V této tabulce odráží aktuální ceny a nezahrnuje propagační nabídek, které jsou aktuálně k dispozici. Aktuální informace o Azure ceny najdete na stránce [ceny Azure](https://azure.microsoft.com/pricing/) . Další informace o služby Bus ceny najdete v článku [ceny Bus služby](https://azure.microsoft.com/pricing/details/service-bus/).

## <a name="conclusion"></a>Uzavření

Tak, že převezme hlubší porozumění dvě technologie, bude možné provádět kvalifikovaně rozhodnutí o technologii fronty, kterou chcete použít a kdy. Rozhodnutí o použití Azure fronty nebo služby Bus fronty jasně závisí na několika faktorech. Následujících skutečností velkém závisí na jednotlivé potřeb aplikace a její architektury. Pokud už vaše aplikace používá základních funkcí aplikace Microsoft Azure, je možná budete chtít zvolte fronty Azure, zejména v případě, že potřebujete základní komunikaci a zpráv mezi službami nebo potřebujete fronty, které mohou být větší než 80 GB velikost.

Protože služby Bus fronty poskytují určitý počet pokročilé funkce, například relace, transakce, vyhledávání duplicit automatické nepoužívaných písma a trvalé publikovat nebo přihlášení odběru funkce mohou být preferovaný volba, pokud vytváříte hybridní aplikaci nebo jinak aplikace vyžaduje těchto funkcí.

## <a name="next-steps"></a>Další kroky

V následujících článcích poskytují další pokyny a informace o používání Azure fronty nebo služby Bus fronty.

- [Jak používat službu Bus fronty](service-bus-dotnet-get-started-with-queues.md)
- [Jak používat službu fronty úložiště](../storage/storage-dotnet-how-to-use-queues.md)
- [Doporučené postupy pro zvýšení výkonu pomocí služby Bus brokered zasílání zpráv](service-bus-performance-improvements.md)
- [Úvod do fronty a témat v Bus služby Azure](http://www.code-magazine.com/article.aspx?quickid=1112041)
- [Příručka pro vývojáře služby Bus](http://www.cloudcasts.net/devguide/Default.aspx?id=11030)
- [Použití službu zařazování v Azure](http://www.developerfusion.com/article/120197/using-the-queuing-service-in-windows-azure/)
- [Principy Azure úložiště fakturace – šířky pásma, transakce a kapacity](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx)

[Azure portálu]: https://portal.azure.com
 
