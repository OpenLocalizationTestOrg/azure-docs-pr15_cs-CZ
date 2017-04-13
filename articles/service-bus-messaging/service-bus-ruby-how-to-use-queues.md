<properties
    pageTitle="Použití služby Bus fronty s skutečné | Microsoft Azure"
    description="Naučte se používat službu Bus fronty v Azure. Ukázky napsané v skutečné."
    services="service-bus"
    documentationCenter="ruby"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-queues"></a>Jak používat službu Bus fronty

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Tato příručka popisuje, jak používat službu Bus fronty. Vzorky psaných skutečné a použijte Azure gem. Scénáře nichž se uplatní zahrnují **vytváření dotazů, odesílání a přijímání zpráv**a **odstranění fronty**. Další informace o služby Bus fronty naleznete v části [Další kroky](#next-steps) .

## <a name="what-are-service-bus-queues"></a>Co jsou služby Bus fronty?

Služba Bus fronty podporují *brokered zpráv* model komunikace. S fronty, komponent distribuované aplikace není přímo vzájemně komunikovat; Místo toho exchange zprávy prostřednictvím fronty, který slouží jako zprostředkovatel. Zpráva výrobce (odesílatele) ruce zprávu do fronty vypnout a poté pokračuje zpracování.
Asynchronní příjemce zprávy (sluchátko) použije zprávy ve frontě a zpracuje ji. Výrobce nemá potřeba počkat, abyste mohli pokračovat ve zpracování a odeslání další zprávy odpovědi od příjemce. Fronty nabízejí **první – první mimo FIFO** doručení zprávy pro jednoho nebo více konkurenční spotřebitele. Zprávy jsou to znamená, obvykle dostali a zpracována příjemce v pořadí, ve kterém se přidala do fronty a každá zpráva přijetí a zpracování klientem pouze jedné zprávy.

![QueueConcepts](./media/service-bus-ruby-how-to-use-queues/sb-queues-08.png)

Služba Bus fronty jsou univerzální technologie, která se dá použít pro celou řadu scénáře:

-   Komunikace mezi web a pracovní role v [vícevrstvého Azure aplikace](service-bus-dotnet-multi-tier-app-using-service-bus-queues.md).
-   Komunikace mezi aplikacemi pro místní a Azure hostovaný aplikací v [hybridní řešení](service-bus-dotnet-hybrid-app-using-service-bus-relay.md).
-   Komunikace mezi součástmi distribuované aplikace spuštěné místní v různých organizace nebo oddělení organizace.

Pomocí fronty můžete umožňují rozšiřování lepší aplikace a povolit více odolnosti architektury serveru.

## <a name="create-a-namespace"></a>Vytvořit obor názvů

Chcete-li v práci začít používat služby Bus fronty Azure, je nutné nejprve vytvořit obor. Obor názvů poskytuje oboru kontejner adresování Bus služby zdrojů v aplikaci. Obor názvů prostřednictvím rozhraní příkazového řádku musí vytvořit, protože portálu Azure nezpůsobí obor názvů s připojením k ACS.

Vytvoření obor názvů:

1. Spusťte konzolu Azure Powershellu.

2. Zadejte následující příkaz a vytvořte obor Bus služby. Zadejte vlastní hodnotou oboru názvů a určete stejné oblasti jako aplikace.

    ```
    New-AzureSBNamespace -Name 'yourexamplenamespace' -Location 'West US' -NamespaceType 'Messaging' -CreateACSNamespace $true

    ![Create Namespace](./media/service-bus-ruby-how-to-use-queues/showcmdcreate.png)
    ```

## <a name="obtain-management-credentials-for-the-namespace"></a>Získejte oprávnění správy pro obor názvů

K provedení správy operací, jako je vytvoření fronty na obor nové musíte získat pověření správy pro obor.

Rutiny prostředí PowerShell, který jste spustili vytvořit obor názvů bus Azure služby zobrazí klíč, který slouží ke správě oboru. Zkopírujte hodnotu **DefaultKey** . Použijete tuto hodnotu v kódu dál v tomto kurzu.

![Kopírování klíče](./media/service-bus-ruby-how-to-use-queues/defaultkey.png)

> [AZURE.NOTE] Můžete taky zjistíte tento klíč Pokud přihlášení k [portálu Azure](https://portal.azure.com/) a přejděte na informace o připojení pro službu Bus názvů.

## <a name="create-a-ruby-application"></a>Vytvoření skutečné aplikace

Vytvoření aplikace skutečné. Pokyny najdete v tématu [Vytvoření skutečné aplikace na Azure](/develop/ruby/tutorials/web-app-with-linux-vm/).

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurace aplikace pomocí služby Bus

Použít Bus služby Azure, stáhněte si a použil funkci balíček skutečné Azure, který obsahuje sadu pohodlí knihoven, které komunikaci se službami ZBÝVAJÍCÍ úložiště.

### <a name="use-rubygems-to-obtain-the-package"></a>Použití RubyGems získání balíčku

1. Pomocí příkazového řádku rozhraní, jako je **prostředí PowerShell** (Windows), **terminálu** (Mac) nebo **flám** (Unix).

2. Do okna příkazového nainstalovat gem a závislosti zadejte "gem nainstalovat azure".

### <a name="import-the-package"></a>Import balíčku

V seznamu Oblíbené textovém editoru, přidejte následující text do horní části souboru skutečné místo, kam chcete použít úložiště:

```
require "azure"
```

## <a name="set-up-an-azure-service-bus-connection"></a>Nastavení připojení Bus služby Azure

Modul Azure přečte proměnné prostředí **AZURE\_SERVICEBUS\_obor názvů** a **AZURE\_SERVICEBUS\_ACCESS_KEY** informace potřebné pro připojení do služby Bus názvů. Pokud nejsou tyto proměnné, musíte zadat informace názvů před použitím **Azure::ServiceBusService** s kódem takto:

```
Azure.config.sb_namespace = "<your azure service bus namespace>"
Azure.config.sb_access_key = "<your azure service bus access key>"
```

Nastavte hodnotu názvů na hodnotu, kterou jste vytvořili, místo úplnou adresu URL. Použijte například **"yourexamplenamespace"**, nejsou "yourexamplenamespace.servicebus.windows.net".

## <a name="how-to-create-a-queue"></a>Postup vytvoření fronty

Objekt **Azure::ServiceBusService** umožňuje pracovat s fronty. Pokud chcete vytvořit fronty, použijte metodu **create_queue()** . Následující příklad vytvoří fronty nebo vytiskne všechny chyby.

```
azure_service_bus_service = Azure::ServiceBusService.new
begin
  queue = azure_service_bus_service.create_queue("test-queue")
rescue
  puts $!
end
```

Můžete také předat **Azure::ServiceBus::Queue** objekt s dalšími možnostmi, která umožňuje přepsat výchozí nastavení fronty, jako je zpráva hodnota time to live nebo maximální velikost fronty. Následující příklad ukazuje, jak nastavit maximální velikost fronty 5GB a čas live 1 minutu:

```
queue = Azure::ServiceBus::Queue.new("test-queue")
queue.max_size_in_megabytes = 5120
queue.default_message_time_to_live = "PT1M"

queue = azure_service_bus_service.create_queue(queue)
```

## <a name="how-to-send-messages-to-a-queue"></a>Odeslání zprávy do fronty

Můžete poslat zprávu do fronty služby Bus aplikace zavolat **Odeslat\_fronty\_message()** metoda **Azure::ServiceBusService** objektu. Zprávy zasílané (a přijaté od) služby Bus fronty jsou **Azure::ServiceBus::BrokeredMessage** objekty a mít sadu standardní vlastnosti (například **štítky** a **čas\_k\_live**), slovník, který se používá k ukládání specifické vlastnosti vlastní aplikace a textu data libovolného aplikací. Aplikaci můžete nastavit textu zprávy předáním řetězcovou hodnotu jako zprávu a všechny požadované standardní vlastnosti budou vyplněné s výchozími hodnotami.

Následující příklad ukazuje, jak odeslat zprávu test do fronty s názvem "zkušební fronty" pomocí **Odeslat\_fronty\_message()**:

```
message = Azure::ServiceBus::BrokeredMessage.new("test queue message")
message.correlation_id = "test-correlation-id"
azure_service_bus_service.send_queue_message("test-queue", message)
```

Fronty Bus služby podpory maximální velikost 256 KB [Standardní osy](service-bus-premium-messaging.md) a 1 MB v [Premium osy](service-bus-premium-messaging.md). Záhlaví obsahuje vlastností standardních a vlastních aplikací, může mít maximální velikosti doručovaných 64 KB. Tam bez omezení počtu zprávu pozdržet ve frontě je ale zakončení na celkové velikosti zprávy uskutečňuje fronty. Velikost fronty je definována v údaje o času vytvoření, s horní mez 5 GB.

## <a name="how-to-receive-messages-from-a-queue"></a>Jak přijímat zprávy z fronty

Příjem zpráv od fronty pomocí **přijímání\_fronty\_message()** metoda **Azure::ServiceBusService** objektu. Ve výchozím nastavení jsou zprávy čtení a zamknuté bez odstraněním ve frontě. Však můžete odstranit zprávy ve frontě budou číst nastavením **: peek_lock** možnost na **hodnotu false**.

Výchozí chování usnadňuje čtení a odstranění operaci dvoufázový také umožňuje na podporu aplikace, které nelze tolerovat chybějící zprávy. Když služby Bus obdrží žádost, zjistí další zprávu do být spotřebované množství, ji nechcete, aby ostatní uživatelé jeho přijetí zamkne a vrátí hodnotu aplikace. Po aplikace dokončení zpracování zprávy (nebo problémy se spolehlivým ukládá pro budoucí zpracování), dokončení druhé fáze procesu přijímání tak, že zavoláte **Odstranit\_fronty\_message()** metoda a poskytování zpráva odstraněna jako parametr. **Odstranit\_fronty\_message()** metoda označte zprávu jako právě spotřebované množství a odeberte ho z fronty.

Pokud **: Náhled\_zamknout** je parametr nastaven na **Nepravda**, čtení a odstranění zprávy může být nejjednodušší modelu a je nejvhodnější pro scénáře, které aplikace nevadí vám není zpracování zprávy v případě se nepovede. Pokud chcete porozumět tomu, zvažte scénáři, ve kterém příjemci problémy žádost přijmout a potom dojde k chybě před zpracování. Protože služby Bus budou označena zprávu jako spotřebované, když se aplikace restartování a jinými zprávy znovu, nebude zahájen, ho bude zmeškané zprávu, která byla spotřebované množství před k chybě.

Následující příklad ukazuje, jak zobrazit a zpracování zpráv pomocí **přijímání\_fronty\_message()**. V příkladu nejdřív přijme a odstraní zprávu pomocí **: Náhled\_lock** nastavena na **hodnotu NEPRAVDA**, obdrží další zprávu a odstraní zprávu pomocí **Odstranit\_fronty\_message()**:

```
message = azure_service_bus_service.receive_queue_message("test-queue",
  { :peek_lock => false })
message = azure_service_bus_service.receive_queue_message("test-queue")
azure_service_bus_service.delete_queue_message(message)
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Postup v případě havárie aplikací a čitelný zprávy

Služba Bus poskytuje funkce vám řádně obnovení z chyb v aplikaci nebo potíže zpracování zprávy. Pokud nelze ji zpracovat z nějakého důvodu sluchátko aplikace a pak můžete volat **Odemknout\_fronty\_message()** metoda **Azure::ServiceBusService** objektu. To způsobí, že služba Bus odemknout zprávy ve frontě a jeho Příprava získaná znovu stejné náročný aplikací nebo jinou náročný aplikací.

Je také časového limitu zprávy uzamčené ve frontě a pokud aplikaci se nezdaří zpracování zprávy před časový limit zámku vyprší platnost (například pokud způsobí zhroucení aplikace), pak služby Bus odemčena zprávy automaticky a díky kterému je dostupný získaná znovu.

V případě, že aplikace padá po zpracování zprávy ale předtím, než **Odstranit\_fronty\_message()** metodu volat a pak se po restartování zprávu do aplikace znovu dodány. Toto je někdy označovány jako **Aspoň jednou zpracování**; To znamená každou zprávu zpracovat aspoň jednou, ale v některých případech může být znovu dodány stejnou zprávu. Pokud scénáře nelze tolerovat duplicitní zpracování, pak vývojáři bychom přidat další použití logických operátorů k aplikaci, která bude řešit duplicitní zprávu doručení. To je často dosáhnout pomocí **zprávy\_id** vlastností zprávy, která konstantní přes neúspěšných pokusech o doručení.

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili základy služby Bus fronty, tyto odkazy vedou na další informace.

-   Základní informace o [fronty, témata a předplatné](service-bus-queues-topics-subscriptions.md).
-   Navštivte [Azure SDK skutečné](https://github.com/Azure/azure-sdk-for-ruby) úložiště na GitHub.

Srovnání fronty Bus služby Azure popisované v tomto článku a Azure fronty popisované v článku znalostní báze [jak používat úložiště fronty z skutečné](../storage/storage-ruby-how-to-use-queue-storage.md) najdete v článku [Azure fronty a fronty Bus služby Azure – porovnání a Contrasted](service-bus-azure-and-service-bus-queues-compared-contrasted.md)
 
