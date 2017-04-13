<properties 
    pageTitle="Služba Bus ceny a fakturace | Microsoft Azure"
    description="Přehled služeb Bus ceny strukturu."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/06/2016"
    ms.author="sethm" />

# <a name="service-bus-pricing-and-billing"></a>Služba Bus ceny a fakturace

Služba Bus je k dispozici v vrstvách Basic, Standard a [Premium](service-bus-premium-messaging.md) . Můžete vybrat vrstvy služeb pro každý obor názvů Bus služby služby, které vytvoříte a tento výběr osy použije ve všech entity vytvořené v rámci tohoto názvů.

>[AZURE.NOTE] Podrobné informace o aktuální ocenění Bus služby najdete v článku [Bus služby Azure ceny stránky](https://azure.microsoft.com/pricing/details/service-bus/)a [Nejčastější dotazy týkající se služeb Bus](service-bus-faq.md#service-bus-pricing).

Služba Bus používá následující dva metry pro fronty a témata/předplatná:

1. **Zasílání zpráv operace**: rozumí volání rozhraní API proti fronty nebo tématu/předplatné koncové body služby. Tento metr nahradí zprávy odeslaná nebo přijatá jako primární jednotku fakturaci využití fronty a témata/předplatná.

2. **Brokered připojení**: definován jako číslo ve špičce trvalých připojení otevřete proti fronty, témata nebo předplatná období dané hodinovou odběr. Tento metr bude použito pouze v standardní osy, ve kterém můžete otevřít další připojení (dříve, připojení se omezí na 100 na fronty/téma/předplatné) za poplatek nominal na připojení.

**Standardní** osy uvádí odměrné ceny pro operací s fronty a témata/předplatná, výsledkem je založen multilicenční slevy až 80 % na nejvyšší úrovni použití. Je také standardní osy základní poplatku za 10 za měsíc, která umožňuje provádět operace 12,5 miliónů měsíčně bezplatně.

Vrstvy **Premium** obsahuje zdroje izolace ve vrstvě procesoru a paměti tak, aby každý pracovní zátěž zákazníka samostatně. Tento kontejner zdroje se nazývá *zpráv jednotku*. Obor názvů každý premium přiřazen nejméně jednu jednotku zasílání zpráv. Máte možnost si zakoupit 1, 2 nebo 4 jednotky zpráv pro každou službu Bus Premium názvů. Jeden pracovní zátěž či entitu může zahrnovat více zpráv jednotky a počet jednotek, zasílání zpráv bude možné měnit kdykoli, sice fakturace ve 24hodinovém nebo denní sazeb. Výsledek je výkon předvídatelná a opakující řešení založených na Bus služby pro. Nejen je tento výkon předvídatelná a dostupný, ale je také rychlejší. Zasílání zpráv Azure Premium Bus služby je založena na modul úložiště zavedená v Azure události rozbočovače. Pole Špička výkon odpovídá Premium zpráv, rychleji než standardní osy.

Všimněte si, že standardní základní poplatků bude účtováno pouze jednou za měsíc za Azure předplatného. To znamená, že po vytvoření jediný standardní nebo Premium osy služby Bus názvů, budete moct vytvářet tolik další standardní nebo Premium osy obory názvů libovolný v části tohoto stejné Azure předplatného, aniž by další základní poplatky.

Všechny obory názvů v existující služby Bus vytvořené před 1 listopadu 2014 se automaticky uloží do standardní osy. Díky nadále mít přístup ke všem funkcím momentálně dostupné s Bus služby. Následně můžete [Azure klasické portál][] přejít základní osy podle potřeby.

Následující tabulka shrnuje funkční rozdíly mezi vrstvách Basic a standardní/Premium.

|Funkce|Základní|Standardní/Premium|
|---|---|---|
|Fronty|Ano|Ano|
|Plánované zprávy|Ano|Ano|
|Témata/předplatná|Ne|Ano|
|Relé|Ne|Ano|
|Transakce|Ne|Ano|
|Odstranění duplicit|Ne|Ano|
|Relace|Ne|Ano|
|Dlouhé zprávy|Ne|Ano|
|ForwardTo|Ne|Ano|
|SendVia|Ne|Ano|
|Brokered připojení (součást)|100 na služby Bus názvů|1 000 na Azure předplatného|
|Brokered připojení (nadsazení povolené)|Ne|Ano (fakturaci)|

## <a name="messaging-operations"></a>Zasílání zpráv operace

Jako součást nový model ceny je změna fakturace za fronty a témata/předplatná. Tyto entity jsou přechodem z fakturace jednotlivých zpráv na fakturace na operaci. "Operace" odkazuje na libovolnou volání rozhraní API proti fronty nebo tématu/předplatné koncový bod služby. Jedná se o stavu operace správy pro odesílání a přijímání a relace.

|Typ operace|Popis|
|---|---|
|Správa|Vytvoření, čtení, aktualizace, odstranění (CRUD) proti fronty nebo témata/předplatná.|
|Zasílání zpráv|Odesílání a přijímání zpráv s fronty nebo témata/předplatná.|
|Stav relace|Získání nebo nastavení stavu relace na fronty nebo tématu/předplatné.|

Následující ceny byly efektivní od 1 listopadu 2014:

|Základní|Pole náklady|
|---|---|
|Operace|0,05 jednoho milionu operace|

|Standardní|Pole náklady|
|---|---|
|Základní poplatků|10/ měsíc|
|Nejdřív 12,5 milion operace nebo měsících|Součástí|
|12,5-100 milionů operace nebo měsících|0,80 jednoho milionu operace|
|100 milionů - 2 500 milionů operace nebo měsících|0,50 jednoho milionu operace|
|Víc než 2 500 milion operace nebo měsících|0,20 jednoho milionu operace|

|Premium|Pole náklady|
|---|---|
|Denní|$11.13 pevnou sazbu vztaženou na jednotky zpráv|

## <a name="brokered-connections"></a>Brokered připojení

*Připojení Brokered* pokryly vzorce použití zákazníka, které zahrnují velkého počtu "trvale připojené" odesílatelů/příjemce proti fronty, témata nebo předplatného. Trvale připojeného odesílatele nebo příjemce jsou ty, které připojit pomocí AMQP nebo HTTP se nenulového přijímání vypršení časového limitu (například HTTP dlouhé dotazování). HTTP odesílatelé a příjemci se okamžitě vypršení časového limitu negeneruje brokered připojení.

Dříve fronty a témata/předplatná dosáhl maximálně 100 souběžné připojení každou adresu URL. Aktuální fakturační schéma odebere za URL limit fronty a témata/předplatné a implementuje kvót a měření na brokered připojení na úrovni Azure předplatné a služby Bus názvů.

Základní osy obsahuje a sporná, i když jiné 100 brokered připojení služby Bus obor. Připojení nad toto číslo odmítnuta v základní osy. Standardní osy odeberete limit jednoho názvů a spočítá použití agregační brokered připojení přes Azure předplatného. Ve standardním osy budou povoleny 1 000 brokered připojení jedno předplatné Azure zvyšovat náklady (pokud už jste základní poplatek). Použití více než celkem 1 000 brokered připojení přes standardní osy služby Bus obory názvů v Azure předplatného se vám nebudou účtovat poplatky na plánu odměrné uvedeno v následující tabulce.

|Brokered připojení (standardní osy)|Pole náklady|
|---|---|
|První 1 000/měsíc|Zahrnutý v sadě základní poplatků|
|1 000-100,000 nebo měsících|0.03 připojení/měsíc|
|100,000-500 000 nebo měsících|0.025 připojení/měsíc|
|Přes 500 000 nebo měsících|0.015 připojení/měsíc|

>[AZURE.NOTE] 1 000 brokered připojení jsou součástí standardní zpráv vrstvu (přes základní poplatek) a mohou být sdíleny všechny fronty, témata a předplatná v rámci přidružené Azure předplatného.

<br />

>[AZURE.NOTE] Fakturace se podle maximální počet souběžné připojení a je průběžně hodinové na základě 744 hodin za měsíc.

|Premium osy
|---|
|Nejsou brokered připojení Účtovaná ve vrstvě Premium.|

Další informace o brokered připojení naleznete v části [Nejčastější dotazy týkající se](#faq) dál v tomto tématu.

## <a name="relay"></a>Předávání

Relé jsou k dispozici pouze v obory názvů standardní osy. V opačném ceny a připojení kvóty pro relé zůstat beze změny. To znamená, že relé budou nadále platit počtu zpráv (ne operace) a předávání zpráv hodin.

|Předávání zpráv ceny|Pole náklady|
|---|---|
|Předávací hodin|0,10 pro každé 100 relay hodiny|
|Zprávy|0,01 pro všech 10 000 zpráv|

## <a name="faq"></a>NEJČASTĚJŠÍ DOTAZY

### <a name="how-is-the-relay-hours-meter-calculated"></a>Jak se vypočítá měřiče Relay hodin?

V [tomto tématu](service-bus-faq.md#how-is-the-relay-hours-meter-calculated).

### <a name="what-are-brokered-connections-and-how-do-i-get-charged-for-them"></a>Jaké jsou brokered připojení a jak můžu získat Účtovaná pro ně?

Připojení k brokered je definována jako jednu z následujících akcí:

1. AMQP připojení z klienta služby Bus fronty nebo tématu/předplatné.

2. Nastavit informace HTTP hovor přijme zprávy od služby Bus téma nebo fronty, která má hodnotu časového limitu přijmout větší než nula.

Služba Bus poplatky za maximální počet souběžné brokered připojení překročit však započítávány množství (1 000 ve standardní vrstvě). Píků jsou měří hodinu průběžně tak, že vydělí 744 hodin za měsíc a sečtená měsíční fakturační období. Součástí množství (1 000 brokered připojení za měsíc) se použije na konci fakturační období proti součet píků poměrná každou hodinu.

Příklad:

1. Každý z 10 000 zařízení připojí pomocí jednoho připojení AMQP a od služby Bus téma přijímá příkazy. Zařízení odesílat telemetrie události k rozbočovači události. Pokud se všechna zařízení připojit 12 hodin denně, tyto připojení poplatky (kromě jiné služby Bus téma poplatky): 10 000 připojení *12 hodin* 31 dnech / 744 = 5 000 brokered připojení. Po měsíční příspěvek 1 000 brokered připojení by bude účtovaná za 4 000 brokered připojení sazbou 0.03 za brokered připojení pro celkových $120.

2. 10 000 zařízení přijímat zprávy z fronty Bus služby prostřednictvím protokolu HTTP určující nenulového časový limit. Pokud se všechna zařízení připojit 12 hodin denně, zobrazí se následující poplatky připojení (kromě jiné služby Bus poplatky): 10 000 HTTP přijímání připojení *12 hodin dne* 31 dnech / 744 hodin = 5 000 brokered připojení.

### <a name="do-brokered-connection-charges-apply-to-queues-and-topicssubscriptions"></a>Brokered připojení poplatky fronty a témata/předplatné?

Ano. Existuje žádné poplatky připojení pro odesílání událostí pomocí protokolu HTTP, bez ohledu na počet odesílání systémy nebo zařízení. Příjem události s podporou protokolu HTTP pomocí větší než nula, někdy se jí říká "dlouho dotazování," časový limit vygeneruje poplatky za brokered připojení. Připojení AMQP generovat poplatky za brokered připojení bez ohledu na to, zda jsou použily odesílat a přijímat. Všimněte si, že 100 brokered připojení jsou povoleny zdarma v základní názvů. Toto je maximální počet brokered připojení povolených pro Azure předplatného. Nejdřív 1 000 brokered připojení přes všechny standardní obory názvů v Azure předplatné jsou však započítávány bez dalších poplatků (pokud už jste základní poplatek). Vzhledem k tomu tyto příspěvky dostatečné, aby pokryly mnoho scénářů zasílání zpráv – služeb, poplatky za brokered připojení obvykle jenom stanou důležité, pokud budete chtít použít AMQP nebo HTTP dlouhé hlasování s velkým počtem klientů; Chcete-li například dosažení efektivnější streamování událost nebo povolení obousměrnou komunikaci s mnoha zařízeních nebo instancí aplikace služby.

## <a name="next-steps"></a>Další kroky

- Podrobné informace o služby Bus ceny najdete v článku [Bus služby Azure ceny stránky](https://azure.microsoft.com/pricing/details/service-bus/).

- Některé běžné nejčastější dotazy týkající se kolem služby bus ceny a fakturaci najdete v článku [Nejčastější dotazy týkající se služeb Bus](service-bus-faq.md#service-bus-pricing) .

[Azure klasické portálu]: http://manage.windowsazure.com