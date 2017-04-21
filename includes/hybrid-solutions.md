#<a name="azure-service-bus"></a>Bus služby Azure

Jestli aplikaci nebo službu získáte cloudu nebo místně často musí chcete provést interakci s jiných aplikací nebo služeb. Pokud chcete, aby obecně užitečný způsob, jak to udělat, nabízí Azure Bus služby. Tento článek trvá pohled na této technologii popisující co je a proč byste měli používat.

## <a name="service-bus-fundamentals"></a>Základy Bus služby
Různých situacích volání pro různé styly komunikace. Nechat aplikací odesílání a přijímání zpráv pomocí jednoduché fronty je někdy nejlepší řešení. V ostatních případech běžnému fronty není dost; fronta s mechanismus publikovat a odběr je lepší. A v některých případech, který má ve skutečnosti potřeby stačí spojení mezi aplikací a #151; fronty nejsou povinná. Služba Bus všechny třemi možnostmi, takže aplikace komunikovat několika různými způsoby.

Služba Bus je více klienta cloudové služby, což znamená, že služba sdílený více uživatelů. Každý uživatel, jako je vývojář aplikace, vytvoří *obor názvů*a potom definuje mechanismy komunikace, které potřebuje v rámci tohoto názvů. [Obrázek 1](#Fig1) znázorňuje, jak to vypadá.

<a name="Fig1"></a>![Diagram Bus služby Azure][svc-bus]
 
**Obrázek 1: Služba Bus obsahuje více klienta služby pro připojení aplikace do cloudu.**

V rámci obor můžete použít jednu nebo více instancí čtyři mechanismy komunikačním, z nichž každá připojí aplikací jiným způsobem. Možnosti jsou:

- *Fronty*, které umožní jeden obousměrnou komunikaci. Jednotlivé fronty funguje jako zprostředkovatele (někdy se jí říká *zprostředkovatele*), která jsou uložená odeslané zprávy, dokud jsou doručeny. Každá zpráva neobdrží jednoho příjemce.
- V části *témata*poskytující jeden obousměrnou komunikaci pomocí *předplatná*– jedno téma může obsahovat víc předplatných. Je do fronty téma funguje jako zprostředkovatele, ale každého předplatného můžete volitelně pomocí filtru zprávy pouze splňující zadaná kritéria.
- *Relé*, které zajišťují obousměrnou komunikaci. Na rozdíl od fronty a témat přenosu neukládá letu zprávy it na není zprostředkovatele. Místo toho jenom předá je k cílovou aplikaci.
- *Událost rozbočovače*poskytující událostí a telemetrie průniku do cloudu v rozsáhlé měřítku s nízkou latence a vysoký spolehlivost.

Při vytváření fronty, téma, předávací nebo centrální událost, můžete ji pojmenovat. Společně s jakéhokoliv s názvem vaší názvů, tento název vytvoří jedinečný identifikátor objektu. Aplikace můžete zadat Tento název Bus služby a potom použijte tento fronty téma, předávací nebo události rozbočovače vzájemně komunikovat. 

Pokud chcete použít některou z těchto objektů, můžete použít aplikace pro systém Windows Windows Communication Foundation (WCF). Fronty, témata a událostí Windows rozbočovače aplikace můžete také použít definované Bus služeb pro zasílání zpráv rozhraní API. Chcete-li tyto objekty snadněji používaly z aplikací systému Windows, poskytuje společnost Microsoft SDK Java, Node.js a jiných jazyků. Můžete taky přístup k fronty, témata a událostí rozbočovače pomocí rozhraní REST API přes HTTP. 

Je důležité vědět, že i když je služba Bus sám spouští v cloudu (to znamená v společnosti Microsoft Azure datacentrech), aplikace, které používá mohlo by umožnit spuštění kdekoli. Pomocí služby Bus se připojujete spuštěné v Azure, například nebo spuštěné uvnitř vlastní datacentra. Můžete taky ji použijete k připojení aplikace na Azure cloudu platformy s místní aplikací nebo tablety a telefony. Je možné i aplikace centrální nebo si připojit domácnost, senzory a jiných zařízeních. Služba Bus je obecný komunikace mechanismus v cloudu přístupný poměrně mnohem odkudkoli, kde. Způsob použití závisí na co aplikace nutné.


## <a name="queues"></a>Fronty

Předpokládejme, že se rozhodnete připojení dvě aplikace pomocí služby Bus fronty. [Obrázek 2](#Fig2) ukazuje tato situace.

<a name="Fig2"></a>![Diagram fronty Bus služby][queues]
 
**Obrázek 2: Služby Bus fronty poskytují jednosměrné řízení asynchronní fronty.**

Proces je jednoduchá: odesílatele odešle zprávu do fronty Bus služby a příjemce vyzvedne zprávy později. Fronta může mít jenom jeden příjemce, jak ukazuje [Obrázek 2](#Fig2) nebo více aplikací může číst z jedné fronty. Tato situace každá zpráva je jen pro čtení jediným sluchátko-více lité služby byste měli použít téma místo.

Každá zpráva má dvě části: sadu vlastností, každou dvojici klíč/hodnota a binární zprávy. Jak používají závisí na co je aplikace pokusíte dělat. Například aplikace odesláním každé zprávy o poslední obchod můžou obsahovat vlastnosti *Prodejce = "Ava"* a *Částka = 10000*. Zprávy můžou obsahovat naskenovaný obrázek prodej podepsané smlouvy nebo není k dispozici, jenom zůstat prázdná.

Příjemce můžete číst zprávy od služby Bus fronty dvěma různými způsoby. První možnost nazývá *ReceiveAndDelete*odebere zprávu z fronty a okamžitě odstraní. Toto je jednoduchý, ale pokud je zhroucení před dokončením zpracování zprávy, zprávy budou ztraceny. Protože je odebrané ve frontě, bez sluchátko k němu přístup. 

Druhá možnost *PeekLock*je určen pro tento problém vyřešit pomocí. Jako ReceiveAndDelete odebere PeekLock číst zprávy ve frontě. Zprávy, ale neodstraníte. Místo toho uzamkne zprávy neviditelný jiných příjemce a čeká nějaká tři události:

- Pokud příjemce zpracuje zprávu úspěšně, volá *Dokončeno*a fronty odstraní zprávu. 
- Pokud příjemce, aby ho se nedají zpracovat zprávu úspěšně, volá *opustit*. Ve frontě pak odebere uzamknutí ze zprávy a je k dispozici další příjemce.
- Pokud příjemce volá žádný z těchto v rámci konfigurovatelné dobu (ve výchozím nastavení 60 sekund), fronty předpokládá, že příjemce se nezdařila. V tomto případě se chová jako kdybyste je měli s názvem opuštění zpřístupnění zprávu do jiné příjemce.

Všimněte si, co může dojít, tady: stejná zpráva může doručit dvakrát, případně na dva různé příjemce. Aplikace pomocí služby Bus fronty musí být počítat to. Usnadnit duplicit, že každá zpráva obsahuje jedinečné MessageID vlastnost, která ve výchozím nastavení se nezmění bez ohledu na to, jak opakovaně pokoušeli přečtena z fronty. 

Fronty jsou užitečné úplně situacích. Zahrnut aplikací komunikovat, i když oba se nepoužívá současně, něco, co je užitečný zejména s dávku a mobilní aplikace. Fronta s více příjemcům obsahuje také automatické služby Vyrovnávání zatížení, protože odeslané zprávy jsou rozšířit mezi tyto příjemce.


## <a name="topics"></a>Témata nápovědy

Užitečné jsou fronty nejsou vždy správné řešení. V některých případech služby Bus tématech jsou lepší. [Obrázek 3](#Fig3) znázorňuje tento nápad.

<a name="Fig3"></a>![Diagram služby Bus témata a předplatného][topics-subs]
 
**Obrázek 3: Založená na filtr, který určuje předplatitelskou aplikace, pak přijímat některé nebo všechny zprávy poslané na téma Bus služby.**

Téma je podobný do fronty různými způsoby. Odeslat zprávy odesílatelům téma stejným způsobem jako odesláním zprávy do fronty a jeho zprávám vypadat stejně s fronty. Velký rozdíl je, že témata každá aplikace přijímání vytvořit vlastní předplatné definuje *Filtr*. Předplatitele pak uvidí pouze zprávy, které odpovídají filtr. [Obrázek 3](#Fig3) příkladem odesílatele a tematickým s tři předplatitele, oba objekty mají vlastní filtr:

- Odběratele 1 přijímá jenom zprávy, které obsahují vlastnost *Prodejce = "Ava"*.
- Odběratele 2 přijímá zprávy, které obsahují vlastnost *Prodejce = "Skutečné"* nebo obsahují proceduru *Částka* , jejichž hodnota je větší než 100 000. Možná skutečné je vedoucí prodeje a tak chce zjistit vlastní prodeje a všechny celkového prodeje bez ohledu na to, který vytváří.
- Odběratele 3 nastavil její filtr *True*, což znamená, že obdrží všechny zprávy. Například tato aplikace zodpovídat správy revizní a proto je třeba zobrazit všechny zprávy.

Stejně jako u fronty, můžete účastníkům na téma čtení zpráv pomocí ReceiveAndDelete nebo PeekLock. Na rozdíl od fronty ale jedné zprávy odeslané na téma stavu podle víc účastníky. Tento přístup běžně označovanému jako *publikování a přihlášení k odběru*, je užitečné při každém více aplikací může být máte zájem o stejné zprávy. Když definujete správné filtr, můžete do myší část proudu zpráv, které potřebuje zobrazíte klepnutím každého účastníka.


## <a name="relays"></a>Relé

Fronty a témata poskytují jednosměrné asynchronní komunikace přes zprostředkovatele. Přenosy přecházel jediným směrem a není přímé připojení mezi odesílatele a příjemce. Ale co dělat, když nechcete, aby to? Předpokládejme aplikace muset odesílání a přijímání zpráv, nebo případně má přímé připojení mezi nimi a nemusíte zprostředkovatele pro ukládání zpráv. Adresa scénáře, jako je tady Bus služba poskytuje relé, jak ukazuje [Obrázek 4](#Fig4) .

<a name="Fig4"></a>![Diagram Service Bus Relay][relay]
 
**Obrázek 4: Relay Service Bus poskytuje synchronní, obousměrnou komunikaci mezi aplikacemi.**

Zjevných otázku požádat o relé je to: Proč mám používat technologii jednu? Přestože nepotřebuji fronty, proč zkontrolujte aplikací komunikaci prostřednictvím do cloudové služby, nikoli pouze pracovat přímo? Odpověď je, že mluvit přímo může být těžší, než si myslíte.

Předpokládejme, že chcete připojení dvě místní aplikace, i běží uvnitř podnikové datacentrech. Všech těchto aplikacích nachází za bránou firewall a každý datacentra nejspíš používá překládání adres (překladu síťových adres). Brána firewall blokuje příchozí data na vyfiltrujete několik porty a překladu síťových adres znamená, že v počítači spuštěné v jednotlivých aplikací nemá pevné IP adresy, kterou můžete kontaktovat přímo z mimo datacentra. Bez některé další pomoc připojení tyto aplikace veřejné Internetu je problém.

Relay Service Bus obsahuje tento nápovědy. Komunikace bi obousměrně prostřednictvím předávací, každá aplikace odchozí připojení TCP s Bus služby a pak pořád otevřený. Veškeré komunikace mezi těmito dvěma aplikacemi přenos přes připojení. Protože každý byl připojení z uvnitř datacentra brána firewall umožní příchozích na každou žádost bez otevření nové porty. Tento přístup získá také vyřešit problém překladu síťových adres, protože každá aplikace má konzistentní koncového bodu v cloudu v celém komunikace. Podle výměny dat prostřednictvím přenos, vyhnete aplikace problémy, které by jinak ztížit komunikace. 

Použití služby Bus relé aplikací závisí na Windows Communication Foundation (WCF). Služba Bus obsahuje vazby WCF, díky kterým ji jednoduchých pro aplikace systému Windows můžete komunikovat prostřednictvím relé. Aplikace, které už používají WCF můžete obvykle jenom zadejte jednu z těchto vazby a pak vzájemné komunikaci prostřednictvím relay. Na rozdíl od fronty a témat ale pomocí relé z aplikace systému Windows, případně náročný některé programování; jsou k dispozici žádné standardní knihovny.

Na rozdíl od fronty a témata aplikací nevytvoříte explicitně relé. Místo toho když aplikace, která chce přijímat zprávy naváže připojení TCP se Bus služby, aby sloužil automaticky se vytvoří. Při přetažení připojení přenos se odstraní. Pokud chcete, aby aplikace najít relay vytvořil konkrétní posluchače, obsahuje služby Bus registru, která umožňuje aplikací k vyhledání konkrétních relay podle názvu.

Relé jsou správné řešení, když potřebujete přímé komunikaci mezi aplikacemi. Zvažte například systému rezervace aerolinek spuštěné v místním datacentra, které musí být přístupné z lze vrácení se změnami, mobilní zařízení a jiných počítačů. Aplikace spuštěné ve všech těchto systémů může závisí na služby Bus relé v cloudu, aby komunikovat, ať může běžet.

## <a name="event-hubs"></a>Rozbočovače události

Událost rozbočovače je velmi scalable požití systému, který může proces miliony události sekundu, povolení aplikace pro zpracování a analyzovat větší dat vytvořené pomocí připojení zařízení nebo aplikací. Třeba byste mohli shromažďovat data o výkonu živou engine z loďstva automobilů přes rozbočovači události. Jakmile získány do rozbočovače událost, můžete transformace a ukládání dat pomocí zprostředkovatelů v reálném čase analýzy nebo obrázku úložiště. Další informace o události rozbočovače najdete v článku [Přehled rozbočovače události][].

## <a name="summary"></a>Souhrn

Připojení aplikace byl vždy část vytváření úplné řešení a oblast scénáře, které vyžadují aplikací a služeb vzájemně komunikovat je nastavený ke zvýšení jako další aplikace a zařízení připojeni k Internetu. Zadáním cloudové technologie pro dosažení to až fronty, témata, relé a událostí rozbočovače služby Bus chce bude tato základní funkce snadněji implementovat a nerozhodnete k dispozici.

[svc-bus]: ./media/hybrid-solutions/SvcBus_01_architecture.png
[queues]: ./media/hybrid-solutions/SvcBus_02_queues.png
[topics-subs]: ./media/hybrid-solutions/SvcBus_03_topicsandsubscriptions.png
[relay]: ./media/hybrid-solutions/SvcBus_04_relay.png
[Přehled rozbočovače událostí]: https://msdn.microsoft.com/library/azure/dn836025.aspx
