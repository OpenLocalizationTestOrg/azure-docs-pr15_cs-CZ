<properties 
    pageTitle="Použití služby Bus fronty s Python | Microsoft Azure" 
    description="Naučte se používat Bus služby Azure fronty z Python." 
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
    ms.date="09/21/2016" 
    ms.author="sethm;lmazuel"/>


# <a name="how-to-use-service-bus-queues"></a>Jak používat službu Bus fronty

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Tento článek popisuje, jak používat službu Bus fronty. Vzorky psaných Python a použil funkci [balíček Bus služby Azure Python][]. Scénáře nichž se uplatní zahrnují **vytváření dotazů, odesílání a přijímání zpráv**a **odstranění fronty**.

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

> [AZURE.NOTE] Instalaci Python nebo [Bus služby Azure Python balíčku][]naleznete v tématu [Průvodce instalací Python](../python-how-to-install.md).

## <a name="create-a-queue"></a>Vytvoření fronty

Objekt **ServiceBusService** umožňuje pracovat s fronty. Přidejte následující kód do horní všech souborů Python, ve kterém chcete přistupovat programově Bus služby:

```
from azure.servicebus import ServiceBusService, Message, Queue
```

Následující kód vytvoří objekt **ServiceBusService** . Nahrazení `mynamespace`, `sharedaccesskeyname`, a `sharedaccesskey` s obor názvů, název klíče sdílený přístup podpisu (přidružení zabezpečení) a hodnotu.

```
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

Hodnoty název klíče přidružení zabezpečení a hodnotu najdete v informacích o připojení [Azure klasické portál][] nebo v podokně Visual Studio **Vlastnosti** při výběru oboru Bus služby v Průzkumníku serveru (jak je uvedeno v předchozí části).

```
bus_service.create_queue('taskqueue')
```

**create_queue** podporuje i další možnosti, které umožňují buď přijměte výchozí nastavení fronty například zprávy hodnota time to live (TTL) nebo maximální velikost fronty. Následující příklad nastaví maximální velikost fronty až 5GB a hodnotu TTL 1 minutu:

```
queue_options = Queue()
queue_options.max_size_in_megabytes = '5120'
queue_options.default_message_time_to_live = 'PT1M'

bus_service.create_queue('taskqueue', queue_options)
```

## <a name="send-messages-to-a-queue"></a>Odeslání zprávy do fronty

Můžete poslat zprávu do fronty služby Bus aplikace zavolat **Odeslat\_fronty\_zprávy** metoda **ServiceBusService** objektu.

Následující příklad ukazuje, jak odeslat zprávu test do fronty s názvem *použití taskqueue* **Odeslat\_fronty\_zprávy**:

```
msg = Message(b'Test Message')
bus_service.send_queue_message('taskqueue', msg)
```

Fronty Bus služby podpory maximální velikost 256 KB [Standardní osy](service-bus-premium-messaging.md) a 1 MB v [Premium osy](service-bus-premium-messaging.md). Záhlaví obsahuje vlastností standardních a vlastních aplikací, může mít maximální velikosti doručovaných 64 KB. Tam bez omezení počtu zprávu pozdržet ve frontě je ale zakončení na celkové velikosti zprávy uskutečňuje fronty. Velikost fronty je definována v údaje o času vytvoření, s horní mez 5 GB. Další informace o kvótách najdete v článku [služby Bus kvóty][].

## <a name="receive-messages-from-a-queue"></a>Přijímání zpráv z fronty

Příjem zpráv od fronty pomocí **přijímání\_fronty\_zprávy** metoda **ServiceBusService** objektu:

```
msg = bus_service.receive_queue_message('taskqueue', peek_lock=False)
print(msg.body)
```

Ve frontě budou odstraněny zprávy, jak se budou číst při parametr **náhledu\_lock** nastavena na **hodnotu False**. Čtení (náhledu) a uzamknout zprávu aniž by odstranil z fronty nastavením parametru **náhledu\_lock** **logickou hodnotu PRAVDA**.

Chování pro čtení a odstranění zprávu jako součást operace přijímání je nejjednodušší modelu a je nejvhodnější pro scénáře, které aplikace nevadí vám není zpracování zprávy v případě se nepovede. Pokud chcete porozumět tomu, zvažte scénáři, ve kterém příjemci problémy žádost přijmout a potom dojde k chybě před zpracování. Protože služby Bus bude označené zprávy jako spotřebované, a když aplikace restartuje, nebude zahájen jinými zprávy znovu ho bude uniknout zprávu, která byla spotřebované množství před k chybě.

Pokud **Náhled\_zámek** parametr nastavena na **hodnotu True**, příjem změní operace dva stupně, která umožní podpory aplikace, které nelze tolerovat chybějící zprávy. Když služby Bus obdrží žádost, zjistí další zprávu do být spotřebované množství, ji nechcete, aby ostatní uživatelé jeho přijetí zamkne a vrátí hodnotu aplikace. Po aplikace dokončení zpracování zprávy (nebo problémy se spolehlivým ukládá pro budoucí zpracování), provede druhé fáze procesu přijímání tak, že zavoláte metodu **delete** na objekt **zprávy** . Způsob **Odstranění** označte zprávu jako právě spotřebované a odeberte ho z fronty.

```
msg = bus_service.receive_queue_message('taskqueue', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Postup v případě havárie aplikací a čitelný zprávy

Služba Bus poskytuje funkce vám řádně obnovení z chyb v aplikaci nebo potíže zpracování zprávy. Pokud je příjemce aplikace nelze ji zpracovat z nějakého důvodu, ho metodu **Odemknout** zavolat na objekt **zprávy** . To způsobí služby Bus odemknout zprávy ve frontě a jeho Příprava získaná znovu stejné náročný aplikací nebo jinou náročný aplikací.

Je také časového limitu zprávy uzamčené ve frontě a pokud aplikaci se nezdaří zpracování zprávy před časový limit zámku vyprší platnost (například pokud způsobí zhroucení aplikace), potom služby Bus bude automaticky odemknout zprávu a jeho Příprava získaná znovu.

V případě, že po zpracování zprávy, ale před s názvem metody **Odstranit** způsobí zhroucení aplikace, pak zpráva bude být znovu dodány do aplikace po restartování. To je někdy označovány jako **Aspoň jednou Processing**, tedy každá zpráva budou zpracovány aspoň jednou, ale v některých případech může být znovu dodány stejná zpráva. Pokud scénáře nelze tolerovat duplicitní zpracování, pak vývojáři bychom přidat další použití logických operátorů k aplikaci, která bude řešit duplicitní zprávu doručení. To je často dosáhnout pomocí vlastnosti **MessageId** zprávy, která se nemění přes pokusy o doručení.

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili základy služby Bus fronty, tyto odkazy vedou na další informace.

-   Přečtěte si článek [fronty, témata a][].

[Azure klasické portálu]: https://manage.windowsazure.com
[Bus služby Azure Python balíčku]: https://pypi.python.org/pypi/azure-servicebus  
[Fronty, témata a předplatné]: service-bus-queues-topics-subscriptions.md
[Služba Bus kvót]: service-bus-quotas.md
 
