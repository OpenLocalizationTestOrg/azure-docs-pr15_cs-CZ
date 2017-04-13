<properties 
    pageTitle="Azure oznámení rozbočovače – pokyny pro diagnostiku" 
    description="Pokyny o tom, jak Diagnostika běžné problémy s Azure oznámení rozbočovače." 
    services="notification-hubs" 
    documentationCenter="Mobile" 
    authors="ysxu" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="NA" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs---diagnosis-guidelines"></a>Azure rozbočovače oznámení – pokyny pro diagnostiku

##<a name="overview"></a>Základní informace

Jednu nejčastější dotazy, které jsme dozvíme z Azure oznámení rozbočovače zákazníci je jak zjistit, proč se nezobrazuje oznámení odeslané z jejich back-end aplikace ukazují klientského zařízení - proč a kdy byly vynechány oznámení a jak tento problém odstranit. V tomto článku projdeme různých důvodů, proč oznámení mohou získat textu nebo nekončí na zařízeních. Podíváme se taky prostřednictvím způsoby, ve kterém lze analyzovat a zjistit příčinu kořenové. 

Nejprve je důležité pochopit, jak Azure oznámení rozbočovače posune oznámení na zařízeních.
![][0]

V toku typické odeslat oznámení zprávy z **aplikace back-end** **Azure oznámení centrální (NH)** , která zase nějaké zpracování na všechny registrace s ohledem na nakonfigurováno značky a výrazy značky k určení "cílů" tedy všechny registrace, které nepřišly nabízená oznámení. Tyto registrace může zahrnovat přes kteroukoli z našich podporované platformy – iOS, Google, Windows, Windows Phone Kindle a Baidu pro Android Čína. Jakmile se otevře cíle, NH pak posune oznámení, rozdělit do více dávku registrace zařízení platformu konkrétní **Nabízená oznámení služby (PNS)** – například APNS pro Apple, GCM pro Google atd. NH ověří s příslušnými PNS na základě přihlašovacích údajů, nastavené na portálu klasické Azure na stránce konfigurace centrální oznámení. PNS předá upozornění příslušné **klientských zařízení**. Toto je platformu doporučené umožňuje poskytovat nabízená oznámení a poznamenejte si, že konečný nohy oznámení o doručení probíhá mezi platformu PNS a zařízením. Proto máme čtyři hlavní součásti - *Klient*, *back-end aplikace*, *Azure oznámení rozbočovače (NH)* a *Nabízená oznámení služby (PNS)* a jeden z těchto kroků tuto chybu může způsobovat oznámení začíná nezobrazí. Další informace o tomto architektura je k dispozici v [Oznámení o rozbočovače přehledu].

Chyba při předvádění oznámení může dojít v průběhu počáteční test/pracovní fáze, které mohou poukazovat problém s konfigurací nebo může dojít v výrobní kde všechny nebo některé upozornění může být začíná zamítnuté označující některé hlubší aplikací nebo zpráv vzorek problém. V části pod se podíváme na různých situacích zamítnuté oznámení od běžné vzácnějších identifikovat některé z nich můžete narazit na zjevných a jiné není tolik. 

##<a name="azure-notifications-hub-mis-configuration"></a>Azure oznámení centrální špatné nastavení 

Azure rozbočovače oznámení musí dokončit ověření v kontextu vývojáře aplikace mohli úspěšně neodeslání oznámení odpovídajících PNS. To je možné vývojářem vytvoření účtu vývojář s příslušnými informacemi o platformě (Google, Apple Windows atd.) a potom registrace jejich použití kde dostanou přihlašovací údaje, které je možné konfigurovat portálu části Konfigurace rozbočovače oznámení. Pokud žádná upozornění prostřednictvím, prvním krokem třeba zajistit, že je v centru oznámení odpovídající aplikaci vytvořili pod svým účtem konkrétní vývojář platformy nakonfigurováno správné přihlašovací údaje. Zjistíte naše [Začíná začít výukové programy pro] užitečné projít Tenhle postup způsobem krok za krokem. Tady jsou některé běžné chybné konfigurace:

1. **Obecné**
 
    ) přepnutí jistotu, že vaše jméno centrální oznámení (bez překlepy) stejné:

    - Kde registrujete z klienta, 
    - Pokud odesíláte oznámení z back-end,  
    - Pokud jste nakonfigurovali PNS přihlašovací údaje a 
    - Jste nakonfigurovali na klienta a back-end jehož přidružení zabezpečení pověření. 
        
    b) nastavení jistotu, že používáte správné konfiguraci řetězce přidružení zabezpečení na klienta a back-end aplikace. Jako pravidlo musíte používat **DefaultListenSharedAccessSignature** na klienta a **DefaultFullSharedAccessSignature** na back-end aplikace, (který dává oprávnění mají být k odeslání oznámení NH)

2. **Konfigurace Apple nabízená oznámení služby (APNS)**
 
    Musíte udržovat dva různé rozbočovače – jeden pro výrobní a jiné pro testování účel. To znamená, že odeslání certifikát, který budete používat v prostředí izolovaného prostoru k rozbočovači samostatné a certifikát, který budete používat ve výrobním k rozbočovači samostatné. Není pokusíte nahrávat různých typů certifikátů ke stejnému rozbočovači jako mohou způsobit selhání oznámení dolů řádku. Pokud nenajdete sami na místě, kde jste nechtěně nahráli různých typů certifikátů ke stejnému rozbočovači, doporučuje se odstranit centru a začít pracovat. Pokud z nějakého důvodu není možné odstranit centrální potom přinejmenším, je potřeba odstranit všechny existující registrace z centra. 

3. **Konfigurace služby Google Cloud zasílání zpráv (GCM)** 

    ) přepnutí jistotu, že chcete povolit "Google Cloud zpráv pro Android" v cloudu projektu. 
    
    ![][2]
    
    b) nastavení pozor, abyste vytvořili klíči"Server" zatímco získání pověření které NH bude používat pro ověření s GCM. 
    
    ![][3]
    
    c) nastavit, že máte nakonfigurované "ID projektu" na straně klienta, což je úplně číselných entitu, kterou můžete získat z řídicího panelu:
    
    ![][1]

##<a name="application-issues"></a>Problémy s aplikací

1) **Značky / označení výrazů**

Pokud používáte značky nebo značku výrazy rozdělit vaší cílovou skupinou, je vždy možné, že při odesílání oznámení, že žádný cíl jsou zjištěny podle výrazů značky a značky, které zadáváte odeslat hovor. Doporučujeme zkontrolovat registrace zajistit, že jsou značky, které odpovídají při odesílání oznámení a ověřte potvrzení o doručení pouze klientů s tyto registrace. Například Pokud odesíláte oznámení slovem "Sports" všechny registrace s NH byly pracujte s vyslovte příkaz značky "Politickém", nebudou odeslány na libovolném zařízení. Komplexní případu může zahrnovat značku výrazů místo, kam jste pouze zaregistrovali s "Značky A" nebo "Značku B", ale při odesílání oznámení, směrujete "Značky A & & značku B". V následující části vlastním Diagnostika tipy způsoby ve kterém můžete zkontrolovat registrace spolu s značky, které mají. 

2) **Šablona problémy**

Pokud používáte šablony a ujistěte se, že jsou následující pokyny uvedeno na stránce [šablony pokyny]. 

3) **Neplatné registrace**

Za předpokladu, že centru oznámení o konfiguraci správně a všechny značky/značku výrazy jste správně výsledná do pole najít platné cíle, ke kterým potřebujete odeslat oznámení, NH aktivuje vypnout několik listů zpracování paralelně - každý dávku posílání zpráv do sady registrací. 

> [AZURE.NOTE] Protože jsme proveďte zpracování paralelně jsme není zaručit pořadí, ve které budete dostávat oznámení. 

Teď Azure oznámení centrální je optimalizována pro modelu doručení zprávy "v krajní jednou". To znamená, že jsme pokusí rušení duplikace tak, aby se zařízením doručované víckrát bez oznámení. Docílit jsme procházet registrace a ujistěte se, že pouze jedné zprávy odeslaný za identifikátor zařízení před skutečným odesláním zprávy PNS. Při každé dávku se neodesílají PNS, která zase přijímá a ověření registrace, je možné, že PNS rozpozná chybu s jedním nebo víc registrací v listu, vrátí chybu Azure NH a ukončí zpracování a tím tuto dávku úplně přetažením. Toto je především s APN, který používá protokol datového proudu TCP. I když jsme optimalizovaná pro u většiny po doručení v tomto případě jsme odebrat neškodné registrace v naší databázi a opakujte oznámení o doručení pro zbytek zařízení v tomto listu.

Můžete získat informace o chybě při pokusu o nezdařeném uložení doručení proti registrace pomocí Azure oznámení rozbočovače REST API: [za Telemetrie zprávy: získání Telemetrie zprávu oznámení](https://msdn.microsoft.com/library/azure/mt608135.aspx) a [PNS názory](https://msdn.microsoft.com/library/azure/mt705560.aspx). V tématu [SendRESTExample](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/SendRestExample) například kód.

##<a name="pns-issues"></a>PNS problémy

Jakmile obdrží upozornění podle příslušných PNS je jeho odpovědností předvádění oznámení zařízení. Azure rozbočovače oznámení je mimo na obrázek a nemá žádný vliv když nebo oznámení bude nedoručila ji do zařízení. Protože oznámení služby platformy poměrně robustní, mají za následek oznámení kontaktovat zařízení v několik sekund, než z PNS. Pokud ale omezení PNS rozbočovače Azure oznámení přiřadíte exponenciální zpět vypnutí strategie a pokud PNS zůstane není dostupný na 30 minut potom máme zásadu na místě na vypršení platnosti a vložte tuto zprávu trvale. 

Pokud PNS snaží poskytovat oznámení, ale zařízení je offline, oznámení se ukládají tak, že PNS po omezenou dobu a Doručená do zařízení, až bude dostupný. Jsou uloženy pouze jeden poslední oznámení pro určité aplikaci. Pokud je v režimu zařízení offline posílají oznámení více o, každý upozornění na novou způsobí, že předchozí oznámení zrušit. Toto chování zachování jenom nejnovější oznámení nazývá utvářením oznámení v APNS a sbalování v GCM (které využívá sbalení klíče). Pokud zařízení zůstane v režimu offline poměrně dlouho, budou odstraněny všechny oznámení uloženými jej. Zdroj – [pokyny pro APNS] & [GCM pokyny]

S Azure oznámení rozbočovače – můžete předáte slučovací klíč prostřednictvím záhlaví HTTP použití obecného `SendNotification` rozhraní API (například .NET SDK – `SendNotificationAsync`) který taky trvá HTTP záhlaví, které jsou předané jako má odpovídajících PNS. 

##<a name="self-diagnose-tips"></a>Automatické Diagnostika tipy

Tady jsme prozkoumá různé možnosti a Diagnostika kořenové způsobit všech problémů, centrální oznámení:

###<a name="verify-credentials"></a>Přihlašovací údaje

1. **Portál pro vývojáře PNS**

    Ověřte je u odpovídajících PNS portálu pro vývojáře (APNS GCM, WNS atd) pomocí naše [Začíná začít výukové programy pro].

2. **Azure klasické portálu**

    Přejděte na kartu konfigurovat zkontrolovat a odpovídat přihlašovací údaje s výsledky získané z portálu pro vývojáře PNS. 

    ![][4]

###<a name="verify-registrations"></a>Ověření registrace

1. **Visual Studio**

    Pokud používáte Visual Studio pro vývoj můžete připojit k Microsoft Azure a zobrazení a správa spoustu služby Azure včetně centrální oznámení z "Server Průzkumníka". To je užitečné především pro vývojáře/testovacím prostředí. 

    ![][9]

    Můžete zobrazit a spravovat všechny registrace v rozbočovače, které se dobře zařazené do kategorií platformy, nativní nebo šablona registrace, značky, PNS identifikátor, id registrace a datum vypršení platnosti. Můžete taky upravovat registrace rychlé úpravy – což je užitečné například pokud chcete upravit značky. 

    ![][8]
 
    > [AZURE.NOTE] Visual Studio funkce úpravy registrace lze používat pouze při zkoušce vývojáře/s omezený počet registrace. Pokud potřebujete opravíte vaše registrace hromadně potřeby zvažte použití funkce registrace pro Export a Import popsané - [Registrace pro Export a Import] (https://msdn.microsoft.com/library/dn790624.aspx)

2. **Průzkumník Bus služby**

    Množství zákazníků pomocí ServiceBus explorer popsané - [ServiceBus Explorer] pro zobrazení a správu jejich centrální oznámení. Je k dispozici code.microsoft.com - [kód ServiceBus Průzkumníka] otevřít zdroj projekt

###<a name="verify-message-notifications"></a>Ověření oznámení

1. **Azure Classic portálu**

    Můžete přejít na kartu "Ladění" o neodeslání oznámení test klienty bez nutností všechny back-end služby nahoru a systémem. 

    ![][7]

2. **Visual Studio**

    Test oznámení můžete poslat taky z comforts Visual Studio:

    ![][10]

    Další na funkci explorer Visual Studio oznámení centrální Azure tady: 
    
    - [Přehled Průzkumníka serveru a]
    - [Příspěvek na Blog Průzkumníka serveru a - 1]
    - [Příspěvek na Blog Průzkumníka serveru a - 2]

###<a name="debug-failed-notifications-review-notification-outcome"></a>Ladění oznámení o nezdařeném uložení / zkontrolujte výsledek oznámení

**Vlastnost EnableTestSend**

Při odeslání oznámení pomocí oznámení rozbočovače nejprve ji jenom získá ve frontě pro NH dělat zpracování k určení prodejce, všech cílů a potom postupně NH ji pošle PNS. To znamená, že při používání rozhraní REST API nebo některou z klienta SDK, úspěšně vrácená odeslat hovor pouze znamená, že zpráva byla úspěšně do fronty s centrální oznámení. Přehled o příčinách při NH postupně máte zprávu odešlete PNS nenabízí. Pokud svého oznámení nedojde na klientského zařízení, je možné, že při pokusu o doručení zprávy PNS NH, došlo k chybě, například velikost datové části překročení maximální povolený PNS nebo konfigurovat NH přihlašovací údaje jsou neplatné atd. Pokud chcete získat přehled o PNS chyby, jsme zavedli vlastnost nazvanou [EnableTestSend funkce]. Tato vlastnost je povolena automaticky při odesílání zpráv test z portálu nebo Visual Studio klienta a tedy vám umožní zobrazit podrobné informace o ladění. Pomocí této prostřednictvím rozhraní API přijímat příklad .NET SDK, kde je k dispozici teď a se přidají do všech klienta SDK postupně. Pomocí služby to ZBÝVAJÍCÍ volání, jednoduše přidejte parametru řetězce dotazu s názvem "test" na konci hovor odeslat jako 

    https://mynamespace.servicebus.windows.net/mynotificationhub/messages?api-version=2013-10&test

*Příklad (.NET SDK)*
 
Předpokládejme, že používáte .NET SDK odeslat nativní oznámením:

    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName);
    var result = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(result.State);
 
`result.State`bude jednoduše stavu `Enqueued` na konci spuštění bez jakékoli pochopení co se stalo s vaší nabízených. Teď můžete použít `EnableTestSend` logická vlastnost při inicializace `NotificationHubClient` a dostali podrobný stav o chybách PNS narazili při odesílání oznámení. Odeslat hovor tady bude trvat déle vrátit, protože ji jenom vrací po NH má oznámení na PNS určete výsledek. 
 
    bool enableTestSend = true;
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName, enableTestSend);
    
    var outcome = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(outcome.State);
    
    foreach (RegistrationResult result in outcome.Results)
    {
        Console.WriteLine(result.ApplicationPlatform + "\n" + result.RegistrationId + "\n" + result.Outcome);
    }

*Ukázkový výstup*

    DetailedStateAvailable
    windows
    7619785862101227384-7840974832647865618-3
    The Token obtained from the Token Provider is wrong
 
Tato zpráva značí buď neplatné údaje nakonfigurováno v centrální oznámení nebo problém s registrací v centru a doporučené kurzu bude odstranit tuto registraci a zpřístupněte klienta začínat před odesláním zprávy. 
 
> [AZURE.NOTE] Poznámka: použití této vlastnosti velkém omezena tak musíte použít toto v prostředí zařízením nebo zkoušení s omezenou sadu registrací. Pouze pošleme ladění oznámení 10 zařízení. Máme také omezení zpracování ladění odešle je 10 za minutu. 

###<a name="review-telemetry"></a>Revize telemetrie 

1. **Použití Azure klasické portálu**

    Na portále můžete získat rychlý přehled o všech aktivitu na vaše Centrum oznámení. 
    
    a) z karty "řídicího panelu" můžete zobrazit souhrnné zobrazení registrace, oznámení, jakož i chyb za platformu. 
    
    ![][5]
    
    b) můžete taky přidat mnoho jiné platformy konkrétní metriky z karty "Monitor" na podívejte hlubší zejména některé PNS konkrétní chyby při NH pokusí poslat oznámení PNS vrátí. 
    
    ![][6]
    
    c) měli začnete s revizí **Příchozích zpráv** **Registrace operace** **Upozornění na úspěšné** a přejděte na kartu platformy ke kontrole PNS konkrétních chybách. 
    
    d) používáte rozbočovač oznámení špatně nakonfigurované s nastavení ověřování zobrazí chyba ověřování PNS. Toto je dobré označení kontrola PNS pověření. 

2) **Programový přístup**

Tady – Další informace 

- [Programový Telemetrie přístup]
- [Aplikace Access telemetrie prostřednictvím rozhraní API ukázka] 

> [AZURE.NOTE] Několik telemetrie týkající se funkcí jako **Registrace pro Export a Import** **Telemetrie aplikace Access prostřednictvím rozhraní API** atd jsou k dispozici pouze v standardní vrstvě. Pokud se pokusíte použít tyto funkce v případě účastníte Free nebo základní osy se vám zpráva výjimek k tomuto účelu při používání SDK a 403 HTTP (zakázáno) při použití přímo z rozhraní REST API. Ujistěte se, že jste přesunuli do standardní úroveň prostřednictvím portálu Classic Azure.  

<!-- IMAGES -->
[0]: ./media/notification-hubs-diagnosing/Architecture.png
[1]: ./media/notification-hubs-diagnosing/GCMConfigure.png
[2]: ./media/notification-hubs-diagnosing/GCMEnable.png
[3]: ./media/notification-hubs-diagnosing/GCMServerKey.png
[4]: ./media/notification-hubs-diagnosing/PortalConfigure.png
[5]: ./media/notification-hubs-diagnosing/PortalDashboard.png
[6]: ./media/notification-hubs-diagnosing/PortalMonitoring.png
[7]: ./media/notification-hubs-diagnosing/PortalTestNotification.png
[8]: ./media/notification-hubs-diagnosing/VSRegistrations.png
[9]: ./media/notification-hubs-diagnosing/VSServerExplorer.png
[10]: ./media/notification-hubs-diagnosing/VSTestNotification.png
 
<!-- LINKS -->
[Přehled rozbočovače oznámení]: notification-hubs-push-notification-overview.md
[Výukové programy Začínáme]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Pokyny pro šablony]: https://msdn.microsoft.com/library/dn530748.aspx 
[Pokyny pro APNS]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW4
[Pokyny pro GCM]: http://developer.android.com/google/gcm/adv.html
[Export a Import registrace]: http://msdn.microsoft.com/library/dn790624.aspx
[Průzkumník ServiceBus]: http://msdn.microsoft.com/library/dn530751.aspx
[Kód ServiceBus Explorer]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Explorer-f2abca5a
[Přehled Průzkumníka serveru a]: http://msdn.microsoft.com/library/windows/apps/xaml/dn792122.aspx 
[Příspěvek na Blog Průzkumníka serveru a - 1]: http://azure.microsoft.com/blog/2014/04/09/deep-dive-visual-studio-2013-update-2-rc-and-azure-sdk-2-3/#NotificationHubs 
[Příspěvek na Blog Průzkumníka serveru a - 2]: http://azure.microsoft.com/blog/2014/08/04/announcing-release-of-visual-studio-2013-update-3-and-azure-sdk-2-4/ 
[Funkce EnableTestSend]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx
[Access programový Telemetrie]: http://msdn.microsoft.com/library/azure/dn458823.aspx
[Aplikace Access telemetrie prostřednictvím rozhraní API ukázka]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel

 