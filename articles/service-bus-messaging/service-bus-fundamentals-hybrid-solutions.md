<properties 
    pageTitle="Služby Azure Bus | Microsoft Azure" 
    description="Úvod k používání služeb Bus připojení Azure aplikací pro jiný software." 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/31/2016" 
    ms.author="sethm"/>

# <a name="azure-service-bus"></a>Bus služby Azure

Jestli aplikaci nebo službu získáte cloudu nebo místně často musí chcete provést interakci s jiných aplikací nebo služeb. Pokud chcete, aby obecně užitečný způsob, jak to udělat, nabízí Microsoft Azure Bus služby. Tento článek trvá pohled na této technologii popisující co je a proč byste měli používat.

## <a name="service-bus-fundamentals"></a>Základy Bus služby

Různých situacích volání pro různé styly komunikace. Nechat aplikací odesílání a přijímání zpráv pomocí jednoduché fronty je někdy nejlepší řešení. V ostatních případech běžnému fronty není dost; fronta s mechanismus publikovat a odběr je lepší. V některých případech skutečně potřebné stačí spojení mezi aplikací. fronty nejsou povinná. Bus služba poskytuje všechny tři možnosti povolení aplikace můžete komunikovat několika různými způsoby.

Služba Bus je více klienta cloudové služby, což znamená, že služba sdílený více uživatelů. Každý uživatel, jako je vývojář aplikace, vytvoří *obor názvů*a potom definuje mechanismy komunikace, které potřebuje v rámci tohoto názvů. Obrázek 1 znázorňuje, jak to vypadá.

![][1]
 
**Obrázek 1: Služba Bus obsahuje více klienta služby pro připojení aplikace do cloudu.**

V rámci obor můžete použít jednu nebo více instancí čtyři mechanismy komunikačním, z nichž každá připojí aplikací jiným způsobem. Možnosti jsou:

- *Fronty*, které umožní jeden obousměrnou komunikaci. Jednotlivé fronty funguje jako zprostředkovatele (někdy se jí říká *zprostředkovatele*), která jsou uložená odeslané zprávy, dokud jsou doručeny. Každá zpráva neobdrží jednoho příjemce.
- V části *témata*poskytující jeden obousměrnou komunikaci pomocí *předplatná*– jedno téma může obsahovat víc předplatných. Je do fronty téma funguje jako zprostředkovatele, ale každého předplatného můžete volitelně pomocí filtru zprávy pouze splňující zadaná kritéria.
- *Relé*, které zajišťují obousměrnou komunikaci. Na rozdíl od fronty a témat předávací neukládá letu zprávy. není zprostředkovatele. Místo toho jenom předá je k cílovou aplikaci.

Když vytvoříte fronty, téma nebo relay, můžete ji pojmenovat. Společně s jakéhokoliv s názvem vaší názvů, tento název vytvoří jedinečný identifikátor objektu. Aplikace můžete zadat Tento název Bus služby a potom použijte tento fronty téma nebo předávání vzájemně komunikovat. 

Některé z těchto objektů ve scénáři relay aplikace pro systém Windows můžete použijete Windows Communication Foundation (WCF). Fronty a témat můžete použít aplikací systému Windows služby Bus definované zpráv rozhraní API. Chcete-li tyto objekty snadněji používaly z aplikací systému Windows, poskytuje společnost Microsoft SDK Java, Node.js a jiných jazyků. Můžete taky přístup k fronty a témat pomocí rozhraní REST API přes HTTP (s). 

Je důležité vědět, že i když je služba Bus sám spouští v cloudu (to znamená v společnosti Microsoft Azure datacentrech), aplikace, které používá mohlo by umožnit spuštění kdekoli. Pomocí služby Bus se připojujete spuštěné v Azure, například nebo spuštěné uvnitř vlastní datacentra. Můžete taky ji použijete k připojení aplikace na Azure cloudu platformy s místní aplikací nebo tablety a telefony. Je možné i aplikace centrální nebo si připojit domácnost, senzory a jiných zařízeních. Služba Bus je mechanismus komunikaci v cloudu přístupný poměrně mnohem odkudkoli, kde. Způsob použití záleží na co aplikace nutné.

## <a name="queues"></a>Fronty

Předpokládejme, že se rozhodnete připojení dvě aplikace pomocí služby Bus fronty. Obrázek 2 ukazuje tato situace.

![][2]
 
**Obrázek 2: Služby Bus fronty poskytují jednosměrné řízení asynchronní fronty.**

Proces je jednoduchá: odesílatele odešle zprávu do fronty Bus služby a příjemce vyzvedne zprávy později. Fronta může mít jenom jeden příjemce, jak ukazuje obrázek 2. Nebo více aplikací může číst z jedné fronty. Tato situace každá zpráva je jen pro čtení jenom přes jeden příjemce. Více lité služby vhodnější použít tématu.

Každá zpráva má dvě části: sadu vlastností, každou dvojici klíč/hodnota a binární zprávy. Jak používají závisí na co je aplikace pokusíte dělat. Například aplikace odesláním každé zprávy o poslední obchod můžou obsahovat vlastnosti *Prodejce = "Ava"* a *Částka = 10000*. Zprávy můžou obsahovat naskenovaný obrázek prodej podepsaného smlouvy nebo není k dispozici, jenom zůstat prázdná.

Příjemce můžete číst zprávy od služby Bus fronty dvěma různými způsoby. První možnost nazývá *ReceiveAndDelete*odebere zprávu z fronty a okamžitě odstraní. Toto je jednoduchý, ale pokud je zhroucení před dokončením zpracování zprávy, zprávy budou ztraceny. Protože je odebrané ve frontě, bez sluchátko k němu přístup. 

Druhá možnost *PeekLock*je určen pro tento problém vyřešit pomocí. Jako **ReceiveAndDelete**odebere **PeekLock** číst zprávy ve frontě. Zprávy, ale neodstraníte. Místo toho uzamkne zprávy neviditelný jiných příjemce a čeká nějaká tři události:

- Pokud příjemce zpracuje zprávu úspěšně, volá **Dokončeno**a fronty odstraní zprávu. 
- Pokud příjemce, aby ho se nedají zpracovat zprávu úspěšně, volá **opustit**. Ve frontě pak odebere uzamknutí ze zprávy a je k dispozici další příjemce.
- Pokud příjemce volá žádný z těchto v rámci konfigurovatelné dobu (ve výchozím nastavení 60 sekund), fronty předpokládá, že příjemce se nezdařila. V tomto případě se chová se jako příjemce měli s názvem **opustit**, zpřístupnění zprávu do jiné příjemce.

Všimněte si, co může dojít, tady: stejná zpráva může doručit dvakrát, případně na dva různé příjemce. Aplikace pomocí služby Bus fronty musí být počítat to. Usnadnit duplicit, že každá zpráva obsahuje jedinečné **MessageID** vlastnost, která ve výchozím nastavení se nezmění bez ohledu na to, jak opakovaně pokoušeli přečtena z fronty. 

Fronty popsané v úplně situacích. Umožňují aplikacím komunikovat, i když oba se nepoužívá současně, něco, co je užitečné především s list a mobilní aplikace. Fronta s více příjemcům obsahuje také automatické služby Vyrovnávání zatížení, protože odeslané zprávy jsou rozšířit mezi tyto příjemce.

## <a name="topics"></a>Témata nápovědy

Užitečné jsou fronty nejsou vždy správné řešení. V některých případech služby Bus tématech jsou lepší. Obrázek 3 znázorňuje tento nápad.

![][3]
 
**Obrázek 3: Založená na filtr, který určuje předplatitelskou aplikace, pak přijímat některé nebo všechny zprávy poslané na téma Bus služby.**

*Téma* je podobný do fronty různými způsoby. Odesílatelů odeslání zprávy na téma stejným způsobem jako odesláním zprávy do fronty a jeho zprávám vypadat stejně jako u fronty. Velký rozdíl se vypočítá následujícím témata povolit jednotlivé přijímání aplikace vytvořit vlastní *předplatné* definuje *Filtr*. Předplatitele pak uvidí pouze zprávy, které odpovídají filtr. Obrázek 3 příkladem odesílatele a tematickým s tři předplatitele, oba objekty mají vlastní filtr:

- Odběratele 1 přijímá jenom zprávy, které obsahují vlastnost *Prodejce = "Ava"*.
- Odběratele 2 obdrží zprávy, které obsahují vlastnost *Prodejce = "Skutečné"* nebo obsahují proceduru *Částka* , jejichž hodnota je větší než 100 000. Skutečné možná je prodejní správce, takže chce zjistit vlastní prodeje a všechny celkového prodeje bez ohledu na to, který vytváří.
- Odběratele 3 nastavil její filtr *True*, což znamená, že obdrží všechny zprávy. Například tato aplikace zodpovídat správy revizní a proto je třeba zobrazit všechny zprávy.

Stejně jako u fronty, můžete účastníkům na téma čtení zpráv pomocí **ReceiveAndDelete** nebo **PeekLock**. Na rozdíl od fronty ale jedné zprávy odeslané na téma stavu podle víc předplatných. Tento přístup běžně označovanému jako *publikování a přihlášení k odběru* (nebo *pub/sub*), je užitečné při každém více aplikací zajímá stejné zprávy. Když definujete správné filtr, můžete do myší část proudu zpráv, které potřebuje zobrazíte klepnutím každého účastníka.

## <a name="relays"></a>Relé

Fronty a témata poskytují jednosměrné asynchronní komunikace přes zprostředkovatele. Přenosy přecházel jediným směrem a není přímé připojení mezi odesílatele a příjemce. Ale co dělat, když nechcete, aby to? Předpokládejme, že vaše aplikace potřebují odesílání a přijímání zpráv, nebo případně má přímé připojení mezi nimi a nemusíte zprostředkovatele pro ukládání zpráv. Adresa scénáře, jako je tady Bus služba poskytuje, *přenášet*, jak ukazuje obrázek 4.

![][4]
 
**Obrázek 4: Relay Service Bus poskytuje synchronní, obousměrnou komunikaci mezi aplikacemi.**

Zjevných otázku požádat o relé je to: Proč mám používat technologii jednu? Přestože nepotřebuji fronty, proč zkontrolujte aplikací komunikaci prostřednictvím do cloudové služby, nikoli pouze pracovat přímo? Odpověď je, že mluvit přímo může být těžší, než si myslíte.

Předpokládejme, že chcete připojení dvě místní aplikace, i běží uvnitř podnikové datacentrech. Všech těchto aplikacích nachází za bránou firewall a každý datacentra nejspíš používá překládání adres (překladu síťových adres). Brána firewall blokuje příchozí data na vyfiltrujete několik porty a překladu síťových adres znamená, že v počítači spuštěné v jednotlivých aplikací nemá pevné IP adresy, kterou můžete kontaktovat přímo z mimo datacentra. Bez některé další pomoc připojení tyto aplikace veřejné Internetu je problém.

Můžete se tím relay Service Bus. Komunikace bi obousměrně prostřednictvím předávací, každá aplikace odchozí připojení TCP s Bus služby a pak pořád otevřený. Veškeré komunikace mezi těmito dvěma aplikacemi přenášena v těchto připojení. Protože každý byl připojení z uvnitř datacentra brány umožňuje příchozích na každou žádost bez otevření nové porty. Tento přístup získá také vyřešit problém překladu síťových adres, protože každá aplikace má konzistentní koncového bodu v cloudu v celém komunikace. Podle výměny dat prostřednictvím přenos, vyhnete aplikace problémy, které by jinak ztížit komunikace. 

Použití služby Bus relé aplikací závisí na Windows Communication Foundation (WCF). Služba Bus obsahuje vazby WCF, díky kterým ji jednoduchých pro aplikace systému Windows můžete komunikovat prostřednictvím relé. Aplikace, které už používají WCF můžete obvykle jenom zadejte jednu z těchto vazby a pak vzájemné komunikaci prostřednictvím relay. Na rozdíl od fronty a témat ale pomocí relé z aplikace systému Windows, případně náročný některé programového; jsou k dispozici žádné standardní knihovny.

Na rozdíl od fronty a témata aplikací nevytvoříte explicitně relé. Místo toho když aplikace, která chce přijímat zprávy naváže připojení TCP se Bus služby, aby sloužil automaticky se vytvoří. Při přetažení připojení přenos se odstraní. Povolení aplikace zobrazíte relay vytvořil konkrétní posluchače, obsahuje služby Bus registru, která umožňuje aplikací k vyhledání konkrétních relay podle názvu.

Relé jsou správné řešení, když potřebujete přímé komunikaci mezi aplikacemi. Zvažte například systému rezervace aerolinek spuštěné v místním datacentra, které musí být přístupné z lze vrácení se změnami, mobilní zařízení a jiných počítačů. Aplikace spuštěné ve všech těchto systémů může závisí na služby Bus relé v cloudu, aby komunikovat, ať může běžet.

## <a name="summary"></a>Souhrn

Připojení aplikace byl vždy část vytváření úplné řešení a oblast scénáře, které vyžadují aplikací a služeb vzájemně komunikovat je nastavený ke zvýšení jako další aplikace a zařízení připojeni k Internetu. Zadáním cloudové technologie pro dosažení to až fronty, témata a relé služby Bus chce bude tato základní funkce snadněji implementovat a nerozhodnete k dispozici.

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili základy Bus služby Azure, tyto odkazy vedou na další informace.

- Jak používat [službu Bus fronty](service-bus-dotnet-get-started-with-queues.md)
- Jak používat [službu Bus témata](service-bus-dotnet-how-to-use-topics-subscriptions.md)
- Jak používat [relay Service Bus](../service-bus-relay/service-bus-dotnet-how-to-use-relay.md)
- [Služba Bus vzorky](service-bus-samples.md)

[1]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_01_architecture.png
[2]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_02_queues.png
[3]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_03_topicsandsubscriptions.png
[4]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_04_relay.png
