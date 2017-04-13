<properties
    pageTitle="Jak používat službu Bus témata (skutečné) | Microsoft Azure"
    description="Naučte se používat službu Bus témata a předplatná v Azure. Ukázky jsou určena skutečné aplikací."
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

# <a name="how-to-use-service-bus-topicssubscriptions"></a>Používání předplatného témata Bus služby

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Tento článek popisuje, jak používat službu Bus témata a předplatná z skutečné aplikací. Scénáře nichž se uplatní zahrnují **vytváření témata a předplatná, vytváření filtrů předplatné posílání zpráv** tématu **přijímat zprávy z předplatného**a **Odstranění témata a předplatná**. Další informace o tématech a předplatná naleznete v části [Další kroky](#next-steps) .

## <a name="service-bus-topics-and-subscriptions"></a>Služba Bus témata a předplatného

Služba Bus témata a předplatná domovské stránce podpory *publikování a přihlášení k odběru* zpráv modelu komunikace. Pokud chcete použít, témata a předplatná, komponent distribuované aplikace není přímo vzájemně komunikovat; budou místo vymění informace přes tematickým, který slouží jako zprostředkovatele.

![TopicConcepts](./media/service-bus-ruby-how-to-use-topics-subscriptions/sb-topics-01.png)

Na rozdíl od služby Bus fronty, kde každá zpráva zpracován jednoho příjemce, témata a předplatná poskytují **- n** formu komunikace pomocí vzorek publikovat nebo přihlášení k odběru. Je možné k registraci víc předplatných na téma. Po odeslání zprávy na téma ho je pak k dispozici každého předplatného zpracuje nezávisle na sobě.

Téma předplatného se podobá virtuální fronta, která přijímá kopie zpráv, které byly odeslány tématu. Volitelně můžete zaregistrovat pravidla filtru tématu na základě za předplatného, která umožňuje filtru nebo omezit zprávy, které mají na téma obdrží které téma předplatná.

Služba Bus témata a předplatná umožňují přizpůsobit zpracovat velké množství zpráv přes velkého počtu uživatelů a aplikací.

## <a name="create-a-namespace"></a>Vytvořit obor názvů

Chcete-li v práci začít používat služby Bus fronty Azure, je nutné nejprve vytvořit obor. Obor názvů poskytuje oboru kontejner adresování Bus služby zdrojů v aplikaci. Obor názvů prostřednictvím rozhraní příkazového řádku musí vytvořit, protože [Azure portál][] nezpůsobí obor názvů s připojením k ACS.

Vytvoření obor názvů:

1. Otevřete okno konzoly Azure Powershellu.

2. Zadejte následující příkaz a vytvořte obor. Zadejte vlastní hodnotou oboru názvů a určete stejné oblasti jako aplikace.

    ```
    New-AzureSBNamespace -Name 'yourexamplenamespace' -Location 'West US' -NamespaceType 'Messaging' -CreateACSNamespace $true
    ```

    ![Vytvoření Namespace](./media/service-bus-ruby-how-to-use-topics-subscriptions/showcmdcreate.png)

## <a name="obtain-default-management-credentials-for-the-namespace"></a>Získejte výchozí oprávnění správy pro obor názvů

K provedení správy operací, jako je vytvoření fronty na obor nové musíte získat pověření správy pro obor.

Rutiny prostředí PowerShell, který jste spustili vytvořit oboru služby Bus zobrazí klíč, který slouží ke správě oboru. Zkopírujte hodnotu **DefaultKey** . Použijete tuto hodnotu v kódu dál v tomto kurzu.

![Kopírování klíče](./media/service-bus-ruby-how-to-use-topics-subscriptions/defaultkey.png)

> [AZURE.NOTE]
> Tento klíč najdete taky Pokud přihlášení k [portálu Azure][] a přejděte na informace o připojení pro vaše názvů.

## <a name="create-a-ruby-application"></a>Vytvoření skutečné aplikace

Pokyny najdete v tématu [Vytvoření skutečné aplikace na Azure](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurace aplikace pomocí služby Bus

Použití služby Bus, stáhněte si a použil funkci balíček skutečné Azure, který obsahuje sadu pohodlí knihoven, které komunikaci se službami ZBÝVAJÍCÍ úložiště.

### <a name="use-rubygems-to-obtain-the-package"></a>Použití RubyGems získání balíčku

1. Pomocí příkazového řádku rozhraní, jako je **prostředí PowerShell** (Windows), **terminálu** (Mac) nebo **flám** (Unix).

2. Do okna příkazového nainstalovat gem a závislosti zadejte "gem nainstalovat azure".

### <a name="import-the-package"></a>Import balíčku

Na začátek skutečné soubor, ve které chcete použít úložiště v oblíbených textovém editoru, přidejte následující:

```
require "azure"
```

## <a name="set-up-a-service-bus-connection"></a>Vytvoření připojení služby Bus

Modul Azure přečte proměnné prostředí **AZURE\_SERVICEBUS\_obor názvů** a **AZURE\_SERVICEBUS\_přístup\_klíč** informace potřebné pro připojení k oboru názvů. Pokud nejsou tyto proměnné, musíte zadat informace názvů před použitím **Azure::ServiceBusService** s kódem takto:

```
Azure.config.sb_namespace = "<your azure service bus namespace>"
Azure.config.sb_access_key = "<your azure service bus access key>"
```

Nastavte hodnotu názvů na hodnotu, kterou jste vytvořili místo úplnou adresu URL. Použijte například **"yourexamplenamespace"**, nejsou "yourexamplenamespace.servicebus.windows.net".

## <a name="create-a-topic"></a>Vytvoření tematického

Objekt **Azure::ServiceBusService** umožňuje pracovat s témata. Následující kód vytvoří **Azure::ServiceBusService** objekt. Chcete-li vytvořit téma, použijte **vytvořit\_topic()** metody. Následující příklad vytvoří téma nebo vytiskne chyby, pokud jsou k dispozici.

```
azure_service_bus_service = Azure::ServiceBusService.new
begin
  topic = azure_service_bus_service.create_queue("test-topic")
rescue
  puts $!
end
```

Můžete také předat **Azure::ServiceBus::Topic** objekt s dalšími možnostmi, které umožňují buď přijměte výchozí nastavení téma například čas zprávy s dynamickými nebo maximální velikost fronty. Následující příklad ukazuje, nastavení velikosti maximální fronty 5GB a čas live 1 minutu:

```
topic = Azure::ServiceBus::Topic.new("test-topic")
topic.max_size_in_megabytes = 5120
topic.default_message_time_to_live = "PT1M"

topic = azure_service_bus_service.create_topic(topic)
```

## <a name="create-subscriptions"></a>Vytvoření předplatného

Téma předplatná vzniká za **Azure::ServiceBusService** objekt. Předplatné se s názvem a nemůže mít volitelné filtr, s omezením nastavení doručování zpráv do fronty virtuální předplatného.

Předplatná jsou trvalý a budou dál existují až do buď nebo tématu souvisejí s, budou odstraněny. Pokud aplikace obsahuje použití logických operátorů k vytvoření předplatné, ho nejdřív zkontrolujte Pokud předplatné už existuje pomocí metody getSubscription.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Vytvoření předplatné s filtrem výchozí (MatchAll)

Filtr **MatchAll** je výchozí filtr, který se používá, pokud zadaný žádný filtr při vytvoření nové předplatné. Při použití filtru **MatchAll** všech zpráv publikovaných na téma jsou umístěny ve frontě virtuální předplatného. Následující příklad vytvoří předplatné s názvem "všechny zprávy" a používá jako výchozí **MatchAll** filtr.

```
subscription = azure_service_bus_service.create_subscription("test-topic", "all-messages")
```

### <a name="create-subscriptions-with-filters"></a>Vytvoření předplatného s filtry

Můžete taky určit filtry, které vám umožní určit, které zprávy poslané na téma má zobrazit v rámci konkrétní předplatné.

Flexibilní typ filtru nepodporuje pro předplatná je **Azure::ServiceBus::SqlFilter**, které podmnožinu SQL92. Filtry SQL pracují s vlastnostmi zprávy, které jsou publikované na téma. Podrobné informace o výrazech, které lze použít s filtrem SQL prohlédněte si [SqlFilter.SqlExpression](http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx) syntaxe.

Filtry můžete přidat k předplatnému pomocí **vytvořit\_rule()** metodu **Azure::ServiceBusService** objektu. Tento způsob umožňuje přidat nové filtry k existujícímu předplatnému.

Vzhledem k tomu, výchozí filtr je nepoužije automaticky pro všechny nové předplatné, je nutné nejprve odebrat výchozí filtr nebo **MatchAll** potlačí všechny filtry, které je možné zadat. Odebrání výchozí pravidlo pomocí **Odstranit\_rule()** metoda **Azure::ServiceBusService** objektu.

Následující příklad vytvoří předplatné s názvem "maximum zprávy" s **Azure::ServiceBus::SqlFilter** pouze vybere zprávy, které mají vlastní **zprávy\_číslo** vlastnost větší než 3:

```
subscription = azure_service_bus_service.create_subscription("test-topic", "high-messages")
azure_service_bus_service.delete_rule("test-topic", "high-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("high-messages-rule")
rule.topic = "test-topic"
rule.subscription = "high-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number > 3" })
rule = azure_service_bus_service.create_rule(rule)
```

Následující příklad vytvoří podobně předplatné s názvem "minimum zprávy" s **Azure::ServiceBus::SqlFilter** pouze vybere zprávy, které mají **message_number** vlastnost menším větší nebo rovnu 3:

```
subscription = azure_service_bus_service.create_subscription("test-topic", "low-messages")
azure_service_bus_service.delete_rule("test-topic", "low-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("low-messages-rule")
rule.topic = "test-topic"
rule.subscription = "low-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number <= 3" })
rule = azure_service_bus_service.create_rule(rule)
```

Když teď odeslání zprávy "test téma" je vždy Doručená příjemce, nemáte předplatné předplatné téma "všechny zprávy" a selektivním doručila ji do příjemce, nemáte předplatné předplatná téma "maximum-" a "minimum zprávy" (v závislosti na obsah zprávy).

## <a name="send-messages-to-a-topic"></a>Odeslání zprávy na téma

Odeslání zprávy na téma Bus služby, musíte použít aplikace **Odeslat\_téma\_message()** metoda **Azure::ServiceBusService** objektu. Zprávy zasílané do služby Bus témata v některých **Azure::ServiceBus::BrokeredMessage** objektů. **Azure::ServiceBus::BrokeredMessage** objekty mají sadu standardní vlastnosti (například **Popisek** a **čas\_k\_live**), slovník, který se používá k ukládání specifické vlastnosti vlastní aplikace a textu data řetězce. Aplikaci můžete nastavit textu zprávy předáním řetězcovou hodnotu na **Odeslat\_téma\_message()** metoda a všechny potřebné standardní vlastnosti se zobrazí výchozí hodnoty.

Následující příklad ukazuje, jak chcete poslat pět zkušební zprávy "test téma". Všimněte si, že hodnota vlastních vlastností **message_number** každé zprávy se liší podle na opakování smyčky (Tato možnost určuje které předplatné dopis dostane):

```
5.times do |i|
  message = Azure::ServiceBus::BrokeredMessage.new("test message " + i,
    { :message_number => i })
  azure_service_bus_service.send_topic_message("test-topic", message)
end
```

Služba Bus témata podpory pro maximální velikost 256 KB [Standardní osy](service-bus-premium-messaging.md) a 1 MB [Premium osy](service-bus-premium-messaging.md). Záhlaví obsahuje vlastností standardních a vlastních aplikací, může mít maximální velikosti doručovaných 64 KB. Tam bez omezení počtu zpráv v tématu je ale zakončení na celkové velikosti zprávy uskutečňuje podle tématu. Toto téma velikost je definována na údaje o času vytvoření, s horní mez 5 GB.

## <a name="receive-messages-from-a-subscription"></a>Přijímání zpráv z předplatného

Zprávy jsou doručeny z předplatného pomocí **přijímání\_předplatné\_message()** metoda **Azure::ServiceBusService** objektu. Ve výchozím nastavení read(peak) a zpráv uzamčené aniž by odstranil z předplatného. Čtení a odstraníte zprávu z předplatného pomocí nastavení **náhledu\_lock** možnost na **hodnotu false**.

Výchozí chování usnadňuje čtení a odstranění operaci dvoufázový také umožňuje na podporu aplikace, které nelze tolerovat chybějící zprávy. Když služby Bus obdrží žádost, zjistí další zprávu do být spotřebované množství, ji nechcete, aby ostatní uživatelé jeho přijetí zamkne a vrátí hodnotu aplikace. Po aplikace dokončení zpracování zprávy (nebo problémy se spolehlivým ukládá pro budoucí zpracování), dokončení druhé fáze procesu přijímání tak, že zavoláte **Odstranit\_předplatné\_message()** metoda a poskytování zpráva odstraněna jako parametr. **Odstranit\_předplatné\_message()** metoda označte zprávu jako právě spotřebované množství a odebrat z předplatného.

Pokud **: Náhled\_lock** je parametr nastaven na **Nepravda**, čtení a odstranění zprávy může být nejjednodušší modelu a je nejvhodnější pro scénáře, které aplikace nevadí vám není zpracování zprávy v případě se nepovede. Pokud chcete porozumět tomu, zvažte scénáři, ve kterém příjemci problémy žádost přijmout a potom dojde k chybě před zpracování. Protože služby Bus bude označené zprávy jako spotřebované, a když aplikace restartuje, nebude zahájen jinými zprávy znovu ho bude uniknout zprávu, která byla spotřebované množství před k chybě.

Následující příklad ukazuje, jak můžete přijaté zprávy a zpracovaných pomocí **přijímání\_předplatné\_message()**. V příkladu nejdřív přijme a odstraní zprávu z předplatného "minimum zprávy" pomocí **: Náhled\_lock** nastavena na **hodnotu NEPRAVDA**, obdrží další zprávu od "maximum zpráv" a odstraní zprávu pomocí **Odstranit\_předplatné\_message()**:

```
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "low-messages", { :peek_lock => false })
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "high-messages")
azure_service_bus_service.delete_subscription_message(message)
```

## <a name="handle-application-crashes-and-unreadable-messages"></a>Selhání aplikace úchyt a čitelný zprávy

Služba Bus poskytuje funkce vám řádně obnovení z chyb v aplikaci nebo potíže zpracování zprávy. Pokud nelze ji zpracovat z nějakého důvodu sluchátko aplikace a pak můžete volat **Odemknout\_předplatné\_message()** metoda **Azure::ServiceBusService** objektu. To způsobí, že služba Bus odemknout zpráv v rámci předplatného a jeho Příprava získaná znovu stejné náročný aplikací nebo jinou náročný aplikací.

Je také časového limitu zprávy uzamčené v rámci předplatného a pokud aplikaci se nezdaří zpracování zprávy před časový limit zámku vyprší platnost (například pokud způsobí zhroucení aplikace), potom služby Bus bude automaticky odemknout zprávu a jeho Příprava získaná znovu.

V případě, že aplikace padá po zpracování zprávy ale předtím, než **Odstranit\_předplatné\_message()** s názvem metody a potom po restartování, je znovu zprávu dodány do aplikace. Toto je někdy označovány jako **Aspoň jednou zpracování**; To znamená každá zpráva budou zpracovány aspoň jednou, ale v některých případech může být znovu dodány stejnou zprávu. Pokud scénáře nelze tolerovat duplicitní zpracování, pak vývojáři bychom přidat další použití logických operátorů k aplikaci, která bude řešit duplicitní zprávu doručení. Tato logika je často dosáhnout pomocí **zprávy\_id** vlastností zprávy, která se nemění přes neúspěšných pokusech o doručení.

## <a name="delete-topics-and-subscriptions"></a>Odstranění témata a předplatného

Témata předplatná jsou trvalý a explicitně odstraníte přes [Azure portál][] nebo programově. Následující příklad ukazuje, jak odstranit téma s názvem "Testovací téma".

```
azure_service_bus_service.delete_topic("test-topic")
```

Odstranění tématu odstraněny také žádné předplatné, které jsou registrované s tématem. Předplatné můžete odstraní taky nezávisle na sobě. Následující kód ukazuje, jak odstranit předplatné s názvem "maximum zprávy" v tématu "Testovací téma":

```
azure_service_bus_service.delete_subscription("test-topic", "high-messages")
```

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili základy služby Bus témata, tyto odkazy vedou na další informace.

- Přečtěte si článek [fronty, témata a](service-bus-queues-topics-subscriptions.md).
- Přehled rozhraní API pro [SqlFilter](http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx).
- Navštivte [Azure SDK skutečné](https://github.com/Azure/azure-sdk-for-ruby) úložiště na GitHub.
 
[Azure portálu]: https://portal.azure.com
