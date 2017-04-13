<properties 
    pageTitle="Použití služby Bus témata s Python | Microsoft Azure" 
    description="Naučte se používat Bus služby Azure témata a předplatná z Python." 
    services="service-bus" 
    documentationCenter="python" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Používání služby Bus témata a předplatného

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Tento článek popisuje, jak používat službu Bus témata a předplatná. Vzorky psaných Python a použil funkci [balíček Python Azure][]. Scénáře nichž se uplatní patří **vytváření témata a předplatná**, **vytváření filtrů předplatného**, **odesílání zpráv na téma**, **přijímat zprávy z předplatného**a **Odstranění témata a předplatná**. Další informace o témata a předplatná naleznete v části [Další kroky](#next-steps) .

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

**Poznámka:** Pokud budete potřebovat k instalaci Python nebo [Python Azure balíčku][], najdete v článku [Průvodce instalací Python](../python-how-to-install.md).

## <a name="create-a-topic"></a>Vytvoření tematického

Objekt **ServiceBusService** umožňuje pracovat s témata. Přidejte následující v horní části libovolný soubor Python, ve kterém chcete přistupovat programově Bus služby:

```
from azure.servicebus import ServiceBusService, Message, Topic, Rule, DEFAULT_RULE_NAME
```

Následující kód vytvoří objekt **ServiceBusService** . Nahrazení `mynamespace`, `sharedaccesskeyname`, a `sharedaccesskey` s skutečné názvů, přidružení sdílené podpis aplikace Access (zabezpečení) základní hodnoty název a klíče.

```
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

Hodnoty název klíče přidružení zabezpečení a hodnotu můžete získat z [Azure portálu][].

```
bus_service.create_topic('mytopic')
```

**vytvořit\_téma** podporuje i další možnosti, které umožňují buď přijměte výchozí nastavení téma například zprávy hodnota time to live nebo tématu maximální velikost. Následující příklad nastaví velikost maximální téma 5 GB a časem live (TTL) hodnotu 1 minuty:

```
topic_options = Topic()
topic_options.max_size_in_megabytes = '5120'
topic_options.default_message_time_to_live = 'PT1M'

bus_service.create_topic('mytopic', topic_options)
```

## <a name="create-subscriptions"></a>Vytvoření předplatného

Předplatná pro témata vzniká za **ServiceBusService** objekt. Předplatné se s názvem a nemůže mít volitelné filtr, s omezením nastavení doručování zpráv do fronty virtuální předplatného.

> [AZURE.NOTE] Předplatná jsou trvalý a budou dál existují až budou, nebo téma, ke kterému přihlášeni, budou odstraněny.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Vytvoření předplatné s filtrem výchozí (MatchAll)

Filtr **MatchAll** je výchozí filtr, který se používá, pokud zadaný žádný filtr při vytvoření nové předplatné. Při použití filtru **MatchAll** všech zpráv publikovaných na téma jsou umístěny ve frontě virtuální předplatného. Následující příklad vytvoří předplatné s názvem "AllMessages" a používá jako výchozí **MatchAll** filtr.

```
bus_service.create_subscription('mytopic', 'AllMessages')
```

### <a name="create-subscriptions-with-filters"></a>Vytvoření předplatného s filtry

Můžete taky určit filtry, které vám umožní určit, které zprávy poslané na téma má zobrazit v rámci předplatného konkrétním tématu.

Flexibilní typ filtru nepodporuje pro předplatná je **SqlFilter**, které podmnožinu SQL92. Filtry SQL pracují s vlastnostmi zprávy, které jsou publikované na téma. Další informace o výrazech, které lze použít s filtrem SQL najdete v článku [SqlFilter.SqlExpression][] syntaxe.

Filtry můžete přidat k předplatnému pomocí **vytvořit\_pravidlo** metodu **ServiceBusService** objektu. Tento způsob umožňuje přidat nové filtry k existujícímu předplatnému.

> [AZURE.NOTE] Vzhledem k tomu, výchozí filtr je nepoužije automaticky pro všechny nové předplatné, je nutné nejprve odebrat výchozí filtr nebo **MatchAll** potlačí všechny filtry, které je možné zadat. Odebrání výchozí pravidlo pomocí **Odstranit\_pravidlo** metodu **ServiceBusService** objektu.

Následující příklad vytvoří předplatné s názvem `HighMessages` s **SqlFilter** pouze vybere zprávy, které mají vyšší než 3 vlastní **messagenumber** vlastnost:

```
bus_service.create_subscription('mytopic', 'HighMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber > 3'

bus_service.create_rule('mytopic', 'HighMessages', 'HighMessageFilter', rule)
bus_service.delete_rule('mytopic', 'HighMessages', DEFAULT_RULE_NAME)
```

Podobně následující příklad vytvoří předplatné s názvem `LowMessages` s **SqlFilter** pouze vybere zprávy, které mají **messagenumber** vlastnost menším větší nebo rovnu 3:

```
bus_service.create_subscription('mytopic', 'LowMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber <= 3'

bus_service.create_rule('mytopic', 'LowMessages', 'LowMessageFilter', rule)
bus_service.delete_rule('mytopic', 'LowMessages', DEFAULT_RULE_NAME)
```

Teď, když je odeslána zpráva `mytopic` vždy Doručená do příjemce, nemáte předplatné předplatné téma **AllMessages** a selektivně doručila ji do příjemce, nemáte předplatné **HighMessages** a **LowMessages** téma předplatná (podle toho, obsah zprávy).

## <a name="send-messages-to-a-topic"></a>Odeslání zprávy na téma

Odeslání zprávy na téma Bus služby, musíte použít aplikace **Odeslat\_téma\_zprávy** metodu **ServiceBusService** objektu.

Následující příklad ukazuje, jak poslat pět zkušební zprávy, které chcete `mytopic`. Všimněte si, že hodnotu **messagenumber** každé zprávy se mění na opakování smyčky (Tato možnost určuje které předplatná obdrží):

```
for i in range(5):
    msg = Message('Msg {0}'.format(i).encode('utf-8'), custom_properties={'messagenumber':i})
    bus_service.send_topic_message('mytopic', msg)
```

Služba Bus témata podpory pro maximální velikost 256 KB [Standardní osy](service-bus-premium-messaging.md) a 1 MB [Premium osy](service-bus-premium-messaging.md). Záhlaví obsahuje vlastností standardních a vlastních aplikací, může mít maximální velikosti doručovaných 64 KB. Tam bez omezení počtu zpráv v tématu je ale zakončení na celkové velikosti zprávy uskutečňuje podle tématu. Toto téma velikost je definována na údaje o času vytvoření, s horní mez 5 GB. Další informace o kvótách najdete v článku [služby Bus kvóty][].

## <a name="receive-messages-from-a-subscription"></a>Přijímání zpráv z předplatného

Příjem zpráv od předplatného pomocí **přijímání\_předplatné\_zprávy** metoda **ServiceBusService** objektu:

```
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=False)
print(msg.body)
```

Zprávy se odstraní z předplatného, jak se budou číst při parametr **Náhled\_zamknout** nastavena na **hodnotu False**. Čtení (náhledu) a uzamknout zprávu aniž by odstranil z fronty nastavením parametru **náhledu\_lock** **logickou hodnotu PRAVDA**.

Chování pro čtení a odstranění zprávu jako součást operace přijímání je nejjednodušší modelu a je nejvhodnější pro scénáře, které aplikace nevadí vám není zpracování zprávy v případě se nepovede. Pokud chcete porozumět tomu, zvažte scénáři, ve kterém příjemci problémy žádost přijmout a potom dojde k chybě před zpracování. Protože služby Bus bude označené zprávy jako spotřebované, a když aplikace restartuje, nebude zahájen jinými zprávy znovu ho bude uniknout zprávu, která byla spotřebované množství před k chybě.

Pokud **Náhled\_zámek** parametr nastavena na **hodnotu True**, příjem změní operace dva stupně, která umožní podpory aplikace, které nelze tolerovat chybějící zprávy. Když služby Bus obdrží žádost, zjistí další zprávu do být spotřebované množství, ji nechcete, aby ostatní uživatelé jeho přijetí zamkne a vrátí hodnotu aplikace. Po aplikace dokončení zpracování zprávy (nebo problémy se spolehlivým ukládá pro budoucí zpracování), provede druhé fáze procesu přijímání tak, že zavoláte **Odstranit** metodu objektu **zprávy** . **Odstranit** metody označí zprávu jako právě spotřebované a ji odebere z předplatného.

```
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Postup v případě havárie aplikací a čitelný zprávy

Služba Bus poskytuje funkce vám řádně obnovení z chyb v aplikaci nebo potíže zpracování zprávy. Pokud je příjemce aplikace nelze ji zpracovat z nějakého důvodu, ho metodu **Odemknout** zavolat na objekt **zprávy** . To způsobí služby Bus odemknout zpráv v rámci předplatného a jeho Příprava získaná znovu stejné náročný aplikací nebo jinou náročný aplikací.

Je také časového limitu zprávy Uzamčeno v rámci předplatného a pokud aplikaci se nezdaří zpracování zprávy před časový limit zámku vyprší platnost (například pokud způsobí zhroucení aplikace), pak služby Bus odemčena zprávy automaticky a díky kterému je dostupný získaná znovu.

V případě, že po zpracování zprávy, ale před s názvem metody **Odstranit** způsobí zhroucení aplikace, pak zpráva bude být znovu dodány do aplikace po restartování. To je někdy označovány jako **Aspoň jednou Processing**, tedy každá zpráva budou zpracovány aspoň jednou, ale v některých případech může být znovu dodány stejná zpráva. Pokud scénáře nelze tolerovat duplicitní zpracování, pak vývojáři bychom přidat další použití logických operátorů k aplikaci, která bude řešit duplicitní zprávu doručení. To je často dosáhnout pomocí vlastnosti **MessageId** zprávy, která se nemění přes pokusy o doručení.

## <a name="delete-topics-and-subscriptions"></a>Odstranění témata a předplatného

Témata předplatná jsou trvalý a explicitně odstraníte přes [Azure portál][] nebo programově. Následující příklad ukazuje, jak odstranit téma s názvem `mytopic`:

```
bus_service.delete_topic('mytopic')
```

Odstranění tématu odstraněny také žádné předplatné, které jsou registrované s tématem. Předplatné můžete odstraní taky nezávisle na sobě. Následující kód ukazuje, jak odstranit předplatné s názvem `HighMessages` z `mytopic` tématu:

```
bus_service.delete_subscription('mytopic', 'HighMessages')
```

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili základy služby Bus témata, tyto odkazy vedou na další informace.

-   Přečtěte si článek [fronty, témata a][].
-   Odkaz pro [SqlFilter.SqlExpression][].

[Azure portálu]: https://portal.azure.com
[Balíček Python Azure]: https://pypi.python.org/pypi/azure  
[Fronty, témata a předplatné]: service-bus-queues-topics-subscriptions.md
[SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[Služba Bus kvót]: service-bus-quotas.md 
