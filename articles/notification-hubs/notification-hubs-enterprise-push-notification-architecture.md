<properties
    pageTitle="Nabízená oznámení rozbočovače - Enterprise architektura"
    description="Pokyny k používání Azure oznámení rozbočovače v podnikovém prostředí"
    services="notification-hubs"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="enterprise-push-architectural-guidance"></a>Pokyny pro architektonické nabízených Enterprise

Podniky jsou dnes postupně přesunutí k vytvoření mobilních aplikací pro buď koncových uživatelů (externího) nebo pro zaměstnance (vnitřní). Mají existující back-end systémy na místě být sálové počítače nebo některé LoB aplikace, které musí být součástí Architektura mobilní aplikace. Tato příručka, bude si o jak vhodné provést integraci doporučující možné řešení pro běžné scénáře.

Požadavek na časté je určený pro odeslání nabízená oznámení uživatelům prostřednictvím mobilní aplikace při výskytu události potřebné v rámci back-end sítě. Například banky zákazníkům, kteří používají aplikaci bankovní banky na svůj iPhone chce chcete být upozorňováni debetní je volání nad určitou ze svého účtu nebo scénáře intranetu Pokud chce upozorněni dostával žádostí o schválení zaměstnanci z finance oddělení, který má v aplikaci schválení rozpočet na svém Windows Phone.

Bankovní účet nebo schválení zpracování se pravděpodobně v některých back-end systému, která musí zahájit nabízených uživateli. Doména může obsahovat více tyto back-end systémů, které musí vytvořit stejný druh použití logických operátorů k implementaci nabízených události spustí oznámení. Složitost tady leží v integrace několika systémů back-end společně s jednoho nabízených systém kde koncoví uživatelé mohou odběru jste se přihlásili k jiné oznámení a může i existovat více mobilních aplikací například v případě mobilních aplikací intranet kde jedno mobilní aplikace chtít dostávat oznámení o z více režimech back-end. Systémy back-end neznáte nebo znát sémantiku/technologie nabízená tak běžným řešením obvykle byl představující součást zjišťuje back-end systémů pro všechny události, která odpovídá pro odesílání zpráv nabízených klientovi.
Zde se budeme ještě lepších řešení pomocí Bus služby Azure - modelu téma/předplatné, které bude je zjednodušit při řešení scalable.

Tady je obecný architektury řešení (generalized více mobilních aplikacích, ale platí při jedinou mobilní aplikace)

## <a name="architecture"></a>Architektura

![][1]

Klíčovou v tomto diagram architektury je Bus služby Azure poskytující témata/předplatná programování modelu (Další na něm v [programovacím služby Bus Pub/Sub]). Sluchátko, což v tomto případě je mobilní back-end (obvykle [Azure Mobile Service], který zahájí nabízených k mobilních aplikací) neobdrží zpráv přímo z back-end systémů, ale místo toho máme vrstvu intermediate odběru poskytovanou [Bus služby Azure] , což umožňuje mobilní back-end přijímat zprávy z jednoho nebo více systémů back-end. Tematickým Bus služby, musí být vytvořené pro každou back-end systémů, například účtu HR, Finance, které jsou v podstatě "témata" potřebné, který zahájí zpráv jako nabízená oznámení. Systémy back-end bude tato témata odesílat zprávy. Mobile back-end můžete se přihlásit k odběru jeden nebo více následujících tématech vytvořením Bus služby předplatného. To bude nárok mobilní back-end pro příjem oznámení z odpovídající back-end systému. Mobilní back-end nadále přijímat zprávy na svoje předplatné a hned po doručení zprávy změní zpět a odešle jako oznámení k jeho centrální oznámení. Oznámení o rozbočovače potom se doručí do mobilní aplikaci. Sumarizovat klíčové součásti, máme:

1. Systémy back-end (LoB/starší systémy)
    - Vytvoří téma Bus služby
    - Odešle zprávu
2. Mobilní back-end
    - Vytvoří předplatné služeb
    - Přijímání zpráv (systém back-end)
    - Odešle oznámení klientům (přes Azure oznámení rozbočovači)
3. Mobilní aplikace
    - Přijme a zobrazit oznámení

###<a name="benefits"></a>Výhody:

1. Oddělení mezi sluchátko (mobilní aplikace nebo služby prostřednictvím centrální oznámení) a odesílatele (back-end systémy) umožňuje další back-end systémy integrovaný s minimálními změnit.
2. Díky taky scénář více mobilní apps nedostáváte události z jednoho nebo více systémů back-end.  

## <a name="sample"></a>Ukázka:

###<a name="prerequisites"></a>Zjistit předpoklady pro
Dokončení následující výukové programy pro seznámení se s koncepty i společné vytváření a konfigurace takto:

1. [Programování služby Bus Pub/Sub] – to vysvětluje podrobnosti o práci s služby Bus témata/předplatná postup vytvoření obor názvů obsahující témata/předplatná, jak odesílání a přijímání zpráv od nich.
2. [Oznámení o rozbočovače – Windows univerzální kurz] – to je popsáno nastavení aplikace pro Windows Store a používat oznámení rozbočovače k registraci a potom zasílat oznámení ve formě.

###<a name="sample-code"></a>Ukázkový kód

Úplné ukázkový kód je k dispozici [Ukázky centrální oznámení]. Je rozdělit do tří součástí:

1. **EnterprisePushBackendSystem**

    na. Tento projekt používá balíček Nuget *WindowsAzure.ServiceBus* a je založený na [služby Bus Pub/Sub programování].

    b. Toto je jednoduchý C# konzoly aplikace tak, aby napodobily LoB systému, který spustí zprávu doručit mobilní aplikaci.

        static void Main(string[] args)
        {
            string connectionString =
                CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

            // Create the topic where we will send notifications
            CreateTopic(connectionString);

            // Send message
            SendMessage(connectionString);
        }

    c. `CreateTopic`slouží k vytvoření služby Bus tématu kde pošleme zprávy.

        public static void CreateTopic(string connectionString)
        {
            // Create the topic if it does not exist already

            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);

            if (!namespaceManager.TopicExists(sampleTopic))
            {
                namespaceManager.CreateTopic(sampleTopic);
            }
        }

    d. `SendMessage`slouží k odeslání zpráv tématu Bus služby. Tady můžeme jednoduše odesíláte sada náhodné zpráv na téma pravidelně za účelem výběru. Za normálních okolností bude back-end systému, který bude posílat zprávy při výskytu události.

        public static void SendMessage(string connectionString)
        {
            TopicClient client =
                TopicClient.CreateFromConnectionString(connectionString, sampleTopic);

            // Sends random messages every 10 seconds to the topic
            string[] messages =
            {
                "Employee Id '{0}' has joined.",
                "Employee Id '{0}' has left.",
                "Employee Id '{0}' has switched to a different team."
            };

            while (true)
            {
                Random rnd = new Random();
                string employeeId = rnd.Next(10000, 99999).ToString();
                string notification = String.Format(messages[rnd.Next(0,messages.Length)], employeeId);

                // Send Notification
                BrokeredMessage message = new BrokeredMessage(notification);
                client.Send(message);

                Console.WriteLine("{0} Message sent - '{1}'", DateTime.Now, notification);

                System.Threading.Thread.Sleep(new TimeSpan(0, 0, 10));
            }
        }

2. **ReceiveAndSendNotification**

    na. Tohoto projektu používá balíčků *WindowsAzure.ServiceBus* a *Microsoft.Web.WebJobs.Publish* Nuget a je založený na [služby Bus Pub/Sub programování].

    b. Toto je jiné C# konzoly aplikaci čímž jsme se spustí jako [Azure WebJob] , protože má souvislá přijímat zprávy od systems LoB/back-end. To bude součástí mobilní back-end.

        static void Main(string[] args)
        {
            string connectionString =
                     CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

            // Create the subscription which will receive messages
            CreateSubscription(connectionString);

            // Receive message
            ReceiveMessageAndSendNotification(connectionString);
        }

    c. `CreateSubscription`slouží k vytvoření předplatné Bus služby v tématu kde bude systému back-end odesílat zprávy. V závislosti na obchodní scénář této součásti vytvořit jednu nebo víc předplatných na odpovídající témata (například některé může být přijímat zprávy z HR systému, některé z systém Finance a tak dál)

        static void CreateSubscription(string connectionString)
        {
            // Create the subscription if it does not exist already
            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);

            if (!namespaceManager.SubscriptionExists(sampleTopic, sampleSubscription))
            {
                namespaceManager.CreateSubscription(sampleTopic, sampleSubscription);
            }
        }

    d. ReceiveMessageAndSendNotification slouží k číst zprávy od téma pomocí svého předplatného a pokud číst proběhne úspěšně vytvořit nechat zasílat mobilní aplikace pomocí Azure oznámení rozbočovače potom oznámení (ve scénáři výběru Windows nativní oznámením).

        static void ReceiveMessageAndSendNotification(string connectionString)
        {
            // Initialize the Notification Hub
            string hubConnectionString = CloudConfigurationManager.GetSetting
                    ("Microsoft.NotificationHub.ConnectionString");
            hub = NotificationHubClient.CreateClientFromConnectionString
                    (hubConnectionString, "enterprisepushservicehub");

            SubscriptionClient Client =
                SubscriptionClient.CreateFromConnectionString
                        (connectionString, sampleTopic, sampleSubscription);

            Client.Receive();

            // Continuously process messages received from the subscription
            while (true)
            {
                BrokeredMessage message = Client.Receive();
                var toastMessage = @"<toast><visual><binding template=""ToastText01""><text id=""1"">{messagepayload}</text></binding></visual></toast>";

                if (message != null)
                {
                    try
                    {
                        Console.WriteLine(message.MessageId);
                        Console.WriteLine(message.SequenceNumber);
                        string messageBody = message.GetBody<string>();
                        Console.WriteLine("Body: " + messageBody + "\n");

                        toastMessage = toastMessage.Replace("{messagepayload}", messageBody);
                        SendNotificationAsync(toastMessage);

                        // Remove message from subscription
                        message.Complete();
                    }
                    catch (Exception)
                    {
                        // Indicate a problem, unlock message in subscription
                        message.Abandon();
                    }
                }
            }
        }
        static async void SendNotificationAsync(string message)
        {
            await hub.SendWindowsNativeNotificationAsync(message);
        }

    e. Pro publikování to jako **WebJob**, klikněte pravým tlačítkem na řešení ve Visual Studiu a vyberte **Publikovat jako WebJob**

    ![][2]

    f. Vyberte profil publikování a vytvořte nový web Azure ji už neexistuje, které uspořádá tento WebJob a až budete mít webu a **Publikovat**.

    ![][3]

    g. Konfigurace úlohy jako "Spustit pořád zobrazoval" tak, aby při přihlášení k [Portálu klasické Azure] byste měli vidět nějak takto:

    ![][4]


3. **EnterprisePushMobileApp**

    na. Toto je aplikace pro Windows Store, které dostanete upozornění na zprávu od WebJob spuštěný jako součást mobilní back-end a zobrazte ji. To je založena na [Oznámení rozbočovače – Windows univerzální kurz].  

    b. Povolena aplikace pro příjem oznámení ve formě uvidí.

    c. Ujistěte se, že následující oznámení rozbočovače registračního kódu je volání v aplikaci otevřou (po nahrazení *HubName* a *DefaultListenSharedAccessSignature*:

        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            var hub = new NotificationHub("[HubName]", "[DefaultListenSharedAccessSignature]");
            var result = await hub.RegisterNativeAsync(channel.Uri);

            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

### <a name="running-sample"></a>Spuštění vzorku:

1. Zajistěte, aby vaše WebJob běží úspěšně a právě "Spustit pořád zobrazoval".
2. Spusťte **EnterprisePushMobileApp** , který se spustí aplikace pro Windows Store.
3. Spuštění aplikace konzoly **EnterprisePushBackendSystem** , který bude simulovat back-end LoB a začnou odesílání zpráv a byste měli vidět oznámení oznámení zobrazená jako následující:

    ![][5]

4. Zprávy odeslaly původně Bus služby témata, která byla sledován Bus služby předplatného na Web projektu. Jakmile zpráva byla přijata, oznámení o byl vytvořen a poslané na mobilní aplikaci. Můžete procházet protokolech WebJob potvrďte zpracování při přechodu na protokoly odkaz v [Portálu klasické Azure] pro Web projektu:

    ![][6]

<!-- Images -->
[1]: ./media/notification-hubs-enterprise-push-architecture/architecture.png
[2]: ./media/notification-hubs-enterprise-push-architecture/WebJobsContextMenu.png
[3]: ./media/notification-hubs-enterprise-push-architecture/PublishAsWebJob.png
[4]: ./media/notification-hubs-enterprise-push-architecture/WebJob.png
[5]: ./media/notification-hubs-enterprise-push-architecture/Notifications.png
[6]: ./media/notification-hubs-enterprise-push-architecture/WebJobsLog.png

<!-- Links -->
[Oznámení o centrální vzorky]: https://github.com/Azure/azure-notificationhubs-samples
[Mobilní služba Azure]: http://azure.microsoft.com/documentation/services/mobile-services/
[Bus služby Azure]: http://azure.microsoft.com/documentation/articles/fundamentals-service-bus-hybrid-solutions/
[Plánování služby Bus Pub/Sub]: http://azure.microsoft.com/documentation/articles/service-bus-dotnet-how-to-use-topics-subscriptions/
[Azure WebJob]: http://azure.microsoft.com/documentation/articles/web-sites-create-web-jobs/
[Oznámení o rozbočovače – Windows univerzální kurz]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[Azure Classic portálu]: https://manage.windowsazure.com/
