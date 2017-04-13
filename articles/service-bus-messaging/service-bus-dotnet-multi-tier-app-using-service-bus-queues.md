<properties
    pageTitle="Aplikace vícevrstvého .NET | Microsoft Azure"
    description="Kurz .NET, který vám pomůže vytvořit vícevrstvého aplikace v Azure využívající služby Bus fronty ke komunikaci mezi úrovní."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="sethm"/>

# <a name="net-multi-tier-application-using-azure-service-bus-queues"></a>.NET vícevrstvého aplikaci fronty Bus služby Azure

## <a name="introduction"></a>Úvod

Vývoj pro Microsoft Azure je snadné pomocí aplikace Visual Studio a bezplatné SDK Azure pro .NET. Tento kurz vás provede kroky k vytvoření aplikace, která používá více Azure zdrojů v místním prostředí. Postup předpokládá, že máte žádné zkušenosti pomocí Azure.

Naučíte se takto:

-   Jak povolit počítač, aby Azure vývoj s jednoho stažení a instalace.
-   Jak se dají pro Azure pomocí aplikace Visual Studio.
-   Postup vytvoření vícevrstvého aplikace v Azure pomocí web a pracovní rolí.
-   Postup pro komunikaci mezi úrovní pomocí služby Bus fronty.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

V tomto kurzu budete vytvářet a spusťte aplikaci vícevrstvého v Azure cloudové službě. Front-end bude role web ASP.NET MVC a back-end bude pracovní roli, který používá fronty Bus služby. Můžete vytvořit stejnou vícevrstvé aplikace s front-end jako webový projekt, který je používaný Azure web místo do cloudové služby. Pokyny k tomu, co dělat, jinak na Azure webu front-end naleznete v části [Další kroky](#nextsteps) . Můžete taky vyzkoušet kurz [.NET na místní/cloudu hybridní aplikaci](../service-bus-relay/service-bus-dotnet-hybrid-app-using-service-bus-relay.md) .

Následující snímek obrazovky znázorňuje dokončené aplikace.

![][0]

## <a name="scenario-overview-inter-role-communication"></a>Přehled scénářů: komunikace mezi role

Můžou Odeslat objednávku pro zpracování, musí front-end součásti uživatelského rozhraní spuštěné v roli web komunikovat s logickou střední úroveň spuštěné v roli pracovní. Tento příklad používá služby Bus brokered zasílání zpráv pro komunikaci mezi úrovně.

Pomocí brokered zpráv mezi web a prostřední úrovní zruší párování dvě složky. Na rozdíl od přímé zasílání zpráv (to znamená, TCP a HTTP) na úrovni webu nepřipojí k střední úroveň přímo. Místo toho ho posune jednotky práce, jako zprávy do Bus služby, které problémy se spolehlivým uloží, dokud se dá používat a jejich zpracování střední vrstvě.

Služba Bus nabízí dvě entity podporuje brokered zasílání zpráv: fronty a témata. S fronty každé zprávy odeslané do fronty využívané jeden příjemce. Témata podpory publikovat nebo přihlášení k odběru vzorku, ve kterém je k dispozici pro předplatné zaregistrovanými v tématu každé publikované zprávy. Každého předplatného logicky udržuje vlastní fronty zpráv. Také je možné konfigurovat předplatná s pravidly filtru, které omezit počet zpráv do fronty předplatné ty, které odpovídají filtru. Následující příklad využívá služba Bus fronty.

![][1]

Tento postup komunikace má několik výhod přes přímé zasílání zpráv:

-   **Časový oddělení.** S zpráv asynchronní výrobci a které se zobrazují koncovým nemusí být online ve stejnou dobu. Služba Bus problémy se spolehlivým ukládá zprávy, dokud náročný strana je připravená k jejich odběru. Díky komponent distribuované aplikace odpojení, buď dobrovolně, například na údržbu nebo z důvodu zhroucení součásti bez vlivu na systém jako celek. Kromě toho náročný aplikace potřebovat pouze přechodu do online určité době dne.

-   **Načtení vyrovnání.** V mnoha aplikacích zatížení systému se mění v průběhu času během zpracování dobu potřebnou pro každý jednotky práce je obvykle konstanty. Intermediating výrobci zprávy a uživatelé s fronty znamená, že náročný aplikace (pracovní) stačí zřízení tak, aby zahrnoval průměr zatížení spíše než zatížení ve špičce. Název hloubkové fronty roste a smlouvy jako příchozí načíst se liší. Tím se uloží přímo peníze z hlediska množství infrastruktury muset služby načítání aplikace.

-   **Vyrovnávání zatížení.** Jak načíst zvýšení, další pracovních postupů můžete zní ve frontě. Každá zpráva zpracován jenom jeden pracovní procesy. Kromě toho tento na základě vyžádané Vyrovnávání zatížení, dovoluje optimálně používat strojů pracovního i v případě, že pracovní počítačích lišit z hlediska zpracování power budou získávat zprávy vlastní maximální sazbou. Tento způsob je často označují vzorek *soutěží příjemce* .

    ![][2]

Následující části se zabývají kód pro implementující tato architektura.

## <a name="set-up-the-development-environment"></a>Nastavení vývojové prostředí:

Abyste mohli začít vývoj Azure aplikací, získat nástrojů a nastavení vývojové prostředí.

1.  Instalace Azure SDK pro .NET na [Nástroje a SDK][].

2.  Klikněte na **instalovat v SDK** pro verzi aplikace Visual Studio používáte. Kroky v tomto kurzu pomocí aplikace Visual Studio 2015.

4.  Po zobrazení výzvy ke spuštění nebo uložení instalační program, klikněte na **Spustit**.

5.  **Webové platformy**klikněte na tlačítko **instalovat** a přejděte k instalaci.

6.  Po dokončení instalace budete mít všechno potřebné ke spuštění se dají aplikace. V SDK obsahuje nástroje, které umožňují snadno vyvíjet Azure aplikace ve Visual Studiu. Pokud nemáte nainstalované Visual Studio, nainstaluje SDK taky bezplatné Visual Studio Express.

## <a name="create-a-namespace"></a>Vytvořit obor názvů

Dalším krokem je vytvoření obor názvů služby a získat klíč sdílené přidružení (zabezpečení aplikace Access podpis). Obor názvů obsahuje hranice aplikace pro každou aplikaci vystaven prostřednictvím služby Bus. Klíč přidružení zabezpečení je generováno aplikací systému při vytvoření obor. Kombinace názvů a přidružení zabezpečení klíč poskytuje pro službu Bus ověření přístup k aplikaci její přihlašovací údaje.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-web-role"></a>Vytvoření webu role

V této části vytvoříte front-end aplikace. Nejprve vytvořte stránky, které se zobrazí aplikace.
Až to přidejte kód, který odešle položky do fronty Bus služby a zobrazí informace o fronty stavu.

### <a name="create-the-project"></a>Vytvoření projektu

1.  Pomocí oprávnění správce, spusťte aplikaci Microsoft Visual Studio. Spusťte aplikaci Visual Studio s oprávněními správce, klikněte pravým tlačítkem myši na ikonu programu **Visual Studio** a pak klikněte na **Spustit jako správce**. Azure výpočetním emulátoru popsáno dále v tomto článku se vyžaduje spuštění aplikace Visual Studio s oprávněními správce.

    Ve Visual Studiu, v nabídce **soubor** klikněte na **Nový**a potom klikněte na **projekt**.

2.  V **Nainstalované šablony**, klikněte v části **Visual Basic**klikněte na **Cloud** a potom na **Cloudové služby Azure**. Zadejte název projektu **MultiTierApp**. Klikněte na **OK**.

    ![][9]

3.  **.NET Framework 4.5** rolí poklikejte na **Role webového ASP.NET**.

    ![][10]

4.  Najeďte myší na **WebRole1** v části **řešení Azure cloudové služby**, klepněte na ikonu tužky a přejmenujte roli web na **FrontendWebRole**. Klikněte na **OK**. (Zkontrolujte, že zadáte "Frontend" s malá písmena "e" není "FrontEnd".)

    ![][11]

5.  V dialogovém okně **Nový projekt ASP.NET** v seznamu **Vyberte šablonu** klikněte na **MVC**.

    ![][12]

6. V dialogovém okně **Nový projekt ASP.NET** kliknutím na tlačítko **Změnit ověřování** . V dialogovém okně **Změnit ověřování** klikněte na položku **Bez ověření**a klikněte na **OK**. Pro účely tohoto návodu nasazujete aplikace, která není potřeba přihlašovací jméno uživatele.

    ![][16]

7. V dialogovém okně **Nový projekt ASP.NET** kliknutím na **OK** vytvořte projekt.

6.  V **Okně Průzkumník**ve **FrontendWebRole** projektu klikněte pravým tlačítkem na **odkazy**a potom klikněte na **Spravovat balíčků NuGet**.

7.  Klikněte na kartu **Procházet** a potom vyhledejte `Microsoft Azure Service Bus`. Klikněte na tlačítko **instalovat**a přijměte podmínky použití.

    ![][13]

    Poznámka: sestavení požadovaný klient jsou teď uváděný byly přidány některé nové soubory kódu.

9.  V **Okně Průzkumník řešení**klikněte pravým tlačítkem myši **modely** a klikněte na tlačítko **Přidat**a potom na **předmětu**. Do pole **název** zadejte název **OnlineOrder.cs**. Klikněte na **Přidat**.

### <a name="write-the-code-for-your-web-role"></a>Vytvoření kódu pro vaše role web

V této části vytvoříte různé stránky, které se zobrazí aplikace.

1.  V souboru OnlineOrder.cs ve Visual Studiu nahraďte stávající definici názvů následující kód:

    ```
    namespace FrontendWebRole.Models
    {
        public class OnlineOrder
        {
            public string Customer { get; set; }
            public string Product { get; set; }
        }
    }
    ```

2.  V **Okně Průzkumník**poklepejte na **Controllers\HomeController.cs**. Přidejte následující příkazy **pomocí** v horní části souboru zahrnout obory názvů modelu, který jste právě vytvořili, zvolte i Bus služby.

    ```
    using FrontendWebRole.Models;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.ServiceBus;
    ```

3.  I v souboru HomeController.cs ve Visual Studiu Nahraďte definici existující názvů následující kód. Tento kód obsahuje metod pro zpracování odesílání položky do fronty.

    ```
    namespace FrontendWebRole.Controllers
    {
        public class HomeController : Controller
        {
            public ActionResult Index()
            {
                // Simply redirect to Submit, since Submit will serve as the
                // front page of this application.
                return RedirectToAction("Submit");
            }
    
            public ActionResult About()
            {
                return View();
            }
    
            // GET: /Home/Submit.
            // Controller method for a view you will create for the submission
            // form.
            public ActionResult Submit()
            {
                // Will put code for displaying queue message count here.
    
                return View();
            }
    
            // POST: /Home/Submit.
            // Controller method for handling submissions from the submission
            // form.
            [HttpPost]
            // Attribute to help prevent cross-site scripting attacks and
            // cross-site request forgery.  
            [ValidateAntiForgeryToken]
            public ActionResult Submit(OnlineOrder order)
            {
                if (ModelState.IsValid)
                {
                    // Will put code for submitting to queue here.
    
                    return RedirectToAction("Submit");
                }
                else
                {
                    return View(order);
                }
            }
        }
    }
    ```

4.  V nabídce **vytvořit** klikněte na **Sestavit řešení** zatím otestovat přesnost vašem čísle do zaměstnání.

5.  Dále vytvořte zobrazení `Submit()` metoda jste dříve vytvořili. Klikněte pravým tlačítkem myši v `Submit()` metoda (přetížení `Submit()` bez parametrů, která má) a pak zvolte **Přidat zobrazení**.

    ![][14]

6.  Zobrazí se dialogové okno pro vytváření zobrazení. V seznamu **šablony** vyberte **vytvořit**. V seznamu **Model třídy** klikněte na **OnlineOrder** předmětu.

    ![][15]

7.  Klikněte na **Přidat**.

8.  Teď Změna zobrazeného název aplikace. V **Okně Průzkumník**, poklikejte **Views\Shared\\_Layout.cshtml** soubor a otevřete ho v editoru Visual Studio.

9.  Nahraďte všechny výskyty **Moje aplikace ASP.NET** s **produkty společnosti LITWARE**.

10. Odebrání odkazů **pro domácnosti**, **o**nebo **kontakt** . Odstranění zvýrazněný kód:

    ![][28]

11. Nakonec upravte na stránce odeslání obsahují některé informace o fronty. V **Okně Průzkumník**poklikejte na soubor **Views\Home\Submit.cshtml** a otevřete ji v editoru Visual Studio. Přidejte následující řádek po `<h2>Submit</h2>`. Prozatím `ViewBag.MessageCount` je prázdná. Který naplní ho později.

    ```
    <p>Current number of orders in queue waiting to be processed: @ViewBag.MessageCount</p>
    ```

12. Teď jste implementovali uživatelského rozhraní. Stisknutím klávesy **F5** spusťte aplikaci a potvrďte, že vypadá podle očekávání.

    ![][17]

### <a name="write-the-code-for-submitting-items-to-a-service-bus-queue"></a>Vytvoření kódu pro odeslání položky do fronty Bus služby

Teď přidejte kód pro odeslání položky do fronty. Nejprve vytvořte předmětu, který obsahuje informace o připojení fronty Bus služby. Potom inicializace připojení z Global.aspx.cs. Nakonec aktualizovat kód odeslání jste vytvořili v HomeController.cs skutečně odešlete položky do fronty Bus služby.

1.  V **Okně Průzkumník řešení**klikněte pravým tlačítkem myši **FrontendWebRole** (klikněte pravým tlačítkem myši projektu není roli). Klikněte na **Přidat**a potom klikněte na **předmětu**.

2.  Název třídy **QueueConnector.cs**. Klikněte na tlačítko **Přidat** předmětu.

3.  Teď přidejte kód, který zapouzdřuje informace o připojení a inicializuje připojení do služby Bus fronty. Nahradit celý obsah QueueConnector.cs následující kód a zadejte hodnoty pro `your Service Bus namespace` (vaše jméno názvů) a `yourKey`, což je **primární klíč** dříve získat z portálu Microsoft Azure.

    ```
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.ServiceBus;
    
    namespace FrontendWebRole
    {
        public static class QueueConnector
        {
            // Thread-safe. Recommended that you cache rather than recreating it
            // on every request.
            public static QueueClient OrdersQueueClient;
    
            // Obtain these values from the portal.
            public const string Namespace = "your Service Bus namespace";
    
            // The name of your queue.
            public const string QueueName = "OrdersQueue";
    
            public static NamespaceManager CreateNamespaceManager()
            {
                // Create the namespace manager which gives you access to
                // management operations.
                var uri = ServiceBusEnvironment.CreateServiceUri(
                    "sb", Namespace, String.Empty);
                var tP = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                    "RootManageSharedAccessKey", "yourKey");
                return new NamespaceManager(uri, tP);
            }
    
            public static void Initialize()
            {
                // Using Http to be friendly with outbound firewalls.
                ServiceBusEnvironment.SystemConnectivity.Mode =
                    ConnectivityMode.Http;
    
                // Create the namespace manager which gives you access to
                // management operations.
                var namespaceManager = CreateNamespaceManager();
    
                // Create the queue if it does not exist already.
                if (!namespaceManager.QueueExists(QueueName))
                {
                    namespaceManager.CreateQueue(QueueName);
                }
    
                // Get a client to the queue.
                var messagingFactory = MessagingFactory.Create(
                    namespaceManager.Address,
                    namespaceManager.Settings.TokenProvider);
                OrdersQueueClient = messagingFactory.CreateQueueClient(
                    "OrdersQueue");
            }
        }
    }
    ```

4.  Teď Ujistěte se, že se s názvem způsobu **Inicializace** . V **Okně Průzkumník**poklepejte na **Global.asax\Global.asax.cs**.

5.  Na konci metody **Application_Start** přidáte následující kód.

    ```
    FrontendWebRole.QueueConnector.Initialize();
    ```

6.  Nakonec aktualizujte webové kód jste vytvořili, můžou odeslat položky do fronty. V **Okně Průzkumník**poklepejte na **Controllers\HomeController.cs**.

7.  Aktualizace `Submit()` metoda (přetížení bez parametrů) takto zobrazíte počtu zpráv pro fronty.

    ```
    public ActionResult Submit()
    {
        // Get a NamespaceManager which allows you to perform management and
        // diagnostic operations on your Service Bus queues.
        var namespaceManager = QueueConnector.CreateNamespaceManager();
    
        // Get the queue, and obtain the message count.
        var queue = namespaceManager.GetQueue(QueueConnector.QueueName);
        ViewBag.MessageCount = queue.MessageCount;
    
        return View();
    }
    ```

8.  Aktualizace `Submit(OnlineOrder order)` metoda (přetížení, která přijímá jeden parametr) takto můžou odeslat informace o objednávce do fronty.

    ```
    public ActionResult Submit(OnlineOrder order)
    {
        if (ModelState.IsValid)
        {
            // Create a message from the order.
            var message = new BrokeredMessage(order);
    
            // Submit the order.
            QueueConnector.OrdersQueueClient.Send(message);
            return RedirectToAction("Submit");
        }
        else
        {
            return View(order);
        }
    }
    ```

9.  Nyní můžete spustit aplikaci znova. Pokaždé, když odešlete smysluplném pořadí zvyšuje počet zpráv.

    ![][18]

## <a name="create-the-worker-role"></a>Vytvoření pracovního role

Teď vytvoříte role pracovníka, která zpracuje odeslání pořadí. Tento příklad používá šablonu **Pracovního roli ve frontě Bus** Visual Studio projektu. Už jste získali požadovaných pověření z portálu.

1. Zkontrolujte, že jste připojili ke svému účtu Azure Visual Studio.

2.  Ve Visual Studiu v **Okně Průzkumník** klikněte pravým tlačítkem myši **role** ve složce **MultiTierApp** projektu.

3.  Klikněte na tlačítko **Přidat**a potom klikněte na **Nový projekt Role pracovní**. Zobrazí se dialogové okno **Přidat nový projekt Role** .

    ![][26]

4.  V dialogovém okně **Přidat nový projekt Role** klikněte na **Pracovní Role s Bus frontě**.

    ![][23]

5.  Do pole **název** zadejte název projektu **OrderProcessingRole**. Klikněte na **Přidat**.

6.  Zkopírujte připojovací řetězec, který jste získali v kroku 9 části "Vytvořit obor služby Bus" do schránky.

7.  V **Okně Průzkumník řešení**, klikněte pravým tlačítkem myši **OrderProcessingRole** jste vytvořili v kroku 5 (zkontrolujte, že kliknete pravým tlačítkem **OrderProcessingRole** podle **rolí**a nikoli třídu). Klikněte na **Vlastnosti**.

8.  Na kartě **Nastavení** v dialogovém okně **Vlastnosti** klikněte do pole **hodnota** pro **Microsoft.ServiceBus.ConnectionString**a potom je vložte koncový bod hodnotu, kterou jste zkopírovali v kroku 6.

    ![][25]

9.  Vytvoření **OnlineOrder** třídy představující objednávky podle obrázku ve frontě. Opakované použití předmětu, které jste už vytvořili. V **Okně Průzkumník řešení**klikněte pravým tlačítkem myši třídě **OrderProcessingRole** (klikněte pravým tlačítkem na ikonu předmětu, ne roli). Klikněte na tlačítko **Přidat**a potom klikněte na **Existující položku**.

10. Přejděte do podsložky **FrontendWebRole\Models**a poklepejte **OnlineOrder.cs** přidáte do tohoto projektu.

11. V **WorkerRole.cs**, změňte hodnotu proměnné **Název_fronty** z `"ProcessingQueue"` k `"OrdersQueue"` uvedeno v následující kód.

    ```
    // The name of your queue.
    const string QueueName = "OrdersQueue";
    ```

12. Přidejte následující pomocí příkazu v horní části souboru WorkerRole.cs.

    ```
    using FrontendWebRole.Models;
    ```

13. V `Run()` fungovat uvnitř `OnMessage()` volání, nahradit obsah `try` klauzule s následující kód.

    ```
    Trace.WriteLine("Processing", receivedMessage.SequenceNumber.ToString());
    // View the message as an OnlineOrder.
    OnlineOrder order = receivedMessage.GetBody<OnlineOrder>();
    Trace.WriteLine(order.Customer + ": " + order.Product, "ProcessingMessage");
    receivedMessage.Complete();
    ```

14. Jste dokončili aplikace. Úplné verze aplikace můžete otestovat tak, že pravým tlačítkem myši na MultiTierApp projektu v okně Průzkumník, vyberete **nastavit jako projekt při spuštění**a stiskněte klávesu F5. Všimněte si, že počtu zpráv není zvýšit, protože roli pracovníka zpracuje položky ve frontě a označí jako dokončený. Výstup trasování pracovníka role vidí zobrazením Azure výpočet emulátoru uživatelského rozhraní. Můžete to uděláte tak, že kliknete pravým tlačítkem myši na ikonu emulátoru v oznamovací oblasti na hlavním panelu a vyberete **Zobrazit výpočet emulátoru uživatelského rozhraní**.

    ![][19]

    ![][20]

## <a name="next-steps"></a>Další kroky  

Další informace o Bus služby, naleznete v následujících zdrojích:  

* [Bus služby Azure][sbmsdn]  
* [Stránku služby Bus služby][sbwacom]  
* [Jak používat službu Bus fronty][sbwacomqhowto]  

Další informace o vícevrstvého scénáře, najdete tady:  

* [.NET vícevrstvého aplikaci úložiště tabulek, dotazů a objekty BLOB][mutitierstorage]  

  [0]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-01.png
  [1]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-100.png
  [2]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-101.png
  [Získání nástroje a SDK]: http://go.microsoft.com/fwlink/?LinkId=271920


  [GetSetting]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.getsetting.aspx
  [Microsoft.WindowsAzure.Configuration.CloudConfigurationManager]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx
  [NamespaceMananger]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx

  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx

  [TopicClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx

  [EventHubClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx

  [9]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-10.png
  [10]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-11.png
  [11]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-02.png
  [12]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-12.png
  [13]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-13.png
  [14]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-33.png
  [15]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-34.png
  [16]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-14.png
  [17]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-36.png
  [18]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-37.png

  [19]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-38.png
  [20]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-39.png
  [23]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRole1.png
  [25]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRoleProperties.png
  [26]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBNewWorkerRole.png
  [28]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-40.png

  [sbmsdn]: http://msdn.microsoft.com/library/azure/ee732537.aspx  
  [sbwacom]: /documentation/services/service-bus/  
  [sbwacomqhowto]: service-bus-dotnet-get-started-with-queues.md  
  [mutitierstorage]: https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36
  