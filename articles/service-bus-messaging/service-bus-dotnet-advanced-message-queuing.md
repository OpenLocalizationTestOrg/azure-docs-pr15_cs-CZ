<properties 
    pageTitle="Jak pomocí rozhraní API služeb Bus .NET AMQP 1.0 | Microsoft Azure" 
    description="Naučte se používat Upřesnit zpráva řízení fronty Protodol (AMQP) 1.0 s API Bus služby Azure .NET." 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/29/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-amqp-10-with-the-service-bus-net-api"></a>Jak pomocí rozhraní API služeb Bus .NET AMQP 1.0

Rozšířený zpráva řízení fronty Protocol (AMQP) 1.0 je efektivně spolehlivé, drátěný úrovni zpráv protokol, který slouží k vytváření robustních, různé platformy zasílání.

Podpora pro AMQP 1.0 v služby Bus znamená, že můžete použít shromažďování zpráv a publikovat a přihlášení k odběru brokered funkce zasílání zpráv z oblasti platformách pomocí efektivně binární protokolu. Kromě toho je možné vytvářet aplikace skládá součástí vytvořené pomocí kombinací jazyky, rámců a operační systémy.

Tento článek vysvětluje, jak používat služby Bus brokered zpráv funkce (fronty a publikovat a přihlášení k odběru témata) z aplikací .NET pomocí rozhraní API služeb Bus .NET. Existuje [companion článek](service-bus-java-how-to-use-jms-api-amqp.md) vysvětluje, jak se to stejné pomocí standardní rozhraní API Java zprávy služby (JMS). Můžete tyto dva příručky společně se naučit používat různé platformy zpráv pomocí AMQP 1.0.

## <a name="get-started-with-service-bus"></a>Začínáme s Bus služby

V tomto článku se předpokládá, že už máte službu Bus obor názvů obsahující fronty s názvem "Frontě1." Pokud není, pak můžete vytvořit obor názvů a fronty pomocí [Azure portálu][]. Podrobnosti o postupu při vytváření názvů Bus služby a fronty najdete v článku [Začínáme s fronty Bus služby](service-bus-dotnet-get-started-with-queues.md#1-create-a-namespace-using-the-Azure-portal).

## <a name="download-the-service-bus-sdk"></a>Stažení služby Bus SDK

Podpora AMQP 1.0 je k dispozici v služby Bus SDK verze 2.1 nebo vyšší. Nejnovější bitů si můžete stáhnout z NuGet na [http://nuget.org/packages/WindowsAzure.ServiceBus/](http://nuget.org/packages/WindowsAzure.ServiceBus/).

## <a name="code-net-applications"></a>Aplikací .NET kódu

Ve výchozím nastavení knihovny klienta služby Bus .NET informuje uživatele o se službou Bus služby prostřednictvím vyhrazené protokol SOAP. Použití AMQP 1.0 namísto výchozí protokol vyžaduje explicitní konfigurace Bus služby základní připojovací řetězec popsané v následující části. Než tato změna kódu aplikace se nezmění v podstatě při použití AMQP 1.0.

V současné verzi existuje několik funkcí rozhraní API, které nejsou podporovány při použití AMQP. Tyto nepodporované funkce jsou uvedené dále v části [nepodporované funkce a omezení](#unsupported-features-and-restrictions). Některá upřesňující nastavení taky s významem různých při použití AMQP. Žádné z těchto nastavení se používají v tomto článku, ale podrobnosti jsou k dispozici v [AMQP Bus služby základní informace](service-bus-amqp-dotnet.md#unsupported-features-restrictions-and-behavioral-differences).

### <a name="configure-via-appconfig"></a>Konfigurace prostřednictvím App.config

Je doporučený postup, že aplikace umožňuje konfiguračního souboru konfigurace uložit nastavení. V aplikacích služby Bus můžete App.config pro ukládání služby Bus **ConnectionString**. Tato ukázková aplikace taky používá App.config uložit název služby Bus osoba, která používá zasílání zpráv.

Ukázkový soubor App.config je zobrazena zde:

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://[namespace].servicebus.windows.net;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp" />
            <add key="EntityName" value="queue1" />
    </appSettings>
</configuration>
```

### <a name="configure-the-service-bus-connection-string"></a>Konfigurace služby Bus připojovacího řetězce

Nastavení **Microsoft.ServiceBus.ConnectionString** hodnotu služby Bus připojovací řetězec, který se používá ke konfiguraci připojení služby Bus. Formát vypadá takto:

```
Endpoint=sb://[namespace].servicebus.windows.net;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp
```

Kde `[namespace]` a `[SAS key]` jsou získány z [Azure portálu][]. Další informace najdete postup [pomocí služby Bus fronty] [].

Pokud chcete použít AMQP, připojovací řetězec je přidán s `;TransportType=Amqp`, která říká klienta knihovny tak, aby jeho připojení služby Bus pomocí AMQP 1.0.

### <a name="configure-the-entity-name"></a>Konfigurace název entity

Tato ukázková aplikace používá `EntityName` nastavení v části **appSettings** konfiguračního souboru pro nastavení názvu fronty, se kterým aplikace výměny zprávy.

### <a name="a-simple-net-application-using-a-service-bus-queue"></a>Jednoduchý .NET aplikace pomocí služby Bus fronty

Následující příklad umožňuje volání a přijímání zpráv do a z fronty Bus služby.

```
// SimpleSenderReceiver.cs
    
using System;
using System.Configuration;
using System.Threading;
using Microsoft.ServiceBus.Messaging;
    
namespace SimpleSenderReceiver
{
    class SimpleSenderReceiver
    {
        private static string connectionString;
        private static string entityName;
        private static Boolean runReceiver = true;
        private MessagingFactory factory;
        private MessageSender sender;
        private MessageReceiver receiver;
        private MessageListener messageListener;
        private Thread listenerThread;
    
        static void Main(string[] args)
        {
            try
            {
                if ((args.Length > 0) && args[0].ToLower().Equals("sendonly"))
                    runReceiver = false;
                    
                string ConnectionStringKey = "Microsoft.ServiceBus.ConnectionString";
                string entityNameKey = "EntityName";
                entityName = ConfigurationManager.AppSettings[entityNameKey];
                connectionString = ConfigurationManager.AppSettings[ConnectionStringKey];
                SimpleSenderReceiver simpleSenderReceiver = new SimpleSenderReceiver();
    
                Console.WriteLine("Press [enter] to send a message. " +
                    "Type 'exit' + [enter] to quit.");
    
                while (true)
                {
                    string s = Console.ReadLine();
                    if (s.ToLower().Equals("exit"))
                    {
                        simpleSenderReceiver.Close();
                        System.Environment.Exit(0);
                    }
                    else
                        simpleSenderReceiver.SendMessage();
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Caught exception: " + e.Message);
            }
        }
    
        public SimpleSenderReceiver()
        {
            factory = MessagingFactory.CreateFromConnectionString(connectionString);
            sender = factory.CreateMessageSender(entityName);
    
            if (runReceiver)
            {
                receiver = factory.CreateMessageReceiver(entityName);
                messageListener = new MessageListener(receiver);
                listenerThread = new Thread(messageListener.Listen);
                listenerThread.Start();
            }
        }
    
        public void Close()
        {
            messageListener.RequestStop();
            listenerThread.Join();
            factory.Close();
        }
    
        private void SendMessage()
        {
            BrokeredMessage message = new BrokeredMessage("Test AMQP message from .NET");
            sender.Send(message);
            Console.WriteLine("Sent message with MessageID = " 
                + message.MessageId);
        }

    }
    
    public class MessageListener
    {
        private MessageReceiver messageReceiver;
        public MessageListener(MessageReceiver receiver)
        {
            messageReceiver = receiver;
        }
    
        public void Listen()
        {
            while (!_shouldStop)
            {
                try
                {
                    BrokeredMessage message = 
                        messageReceiver.Receive(new TimeSpan(0, 0, 10));
                    if (message != null)
                    {
                        Console.WriteLine("Received message with MessageID = " + 
                            message.MessageId);
                        message.Complete();
                    }
                }
                catch (Exception e)
                {
                    Console.WriteLine("Caught exception: " + e.Message);
                    break;
                }
            }
        }
    
        public void RequestStop()
        {
            _shouldStop = true;
        }
    
        private volatile bool _shouldStop;
    }
}
```

### <a name="run-the-application"></a>Spusťte aplikaci

Spuštění aplikace vytvoří výstup formuláře:

```
> SimpleSenderReceiver.exe
Press [enter] to send a message. Type 'exit' + [enter] to quit.
    
Sent message with MessageID = fb7f5d3733704e4ba4bd55f759d9d7cf
Received message with MessageID = fb7f5d3733704e4ba4bd55f759d9d7cf
    
Sent message with MessageID = d00e2e088f06416da7956b58310f7a06
Received message with MessageID = d00e2e088f06416da7956b58310f7a06
    
Received message with MessageID = f27f79ec124548c196fd0db8544bca49
Sent message with MessageID = f27f79ec124548c196fd0db8544bca49
exit
```

## <a name="cross-platform-messaging-between-jms-and-net"></a>Různé platformy zasílání zpráv mezi JMS a .NET

Toto téma ukázal pro odeslání zprávy do služby Bus pomocí .NET a taky jak přijímat zprávy pomocí .NET. Však jednou z hlavních výhod AMQP 1.0 je, že umožňuje aplikace integrované z komponenty napsané v různých jazycích u těch, které jste si vyměnili problémy se spolehlivým a u celé věrností.

Pomocí aplikace .NET ukázkové jsme je popsali výše a podobné Java aplikace z příručku Průvodce vyhledáváním, [jak používat Java zprávy služby (JMS) rozhraní API aplikace pomocí služby Bus & AMQP 1.0](service-bus-java-how-to-use-jms-api-amqp.md), je možné k výměně zpráv mezi .NET a Java. 

Další informace o podrobnosti a platformách zasílání zpráv pomocí služby Bus a AMQP 1.0 najdete v tématu [1.0 AMQP Bus služby základní informace](service-bus-amqp-overview.md).

### <a name="jms-to-net"></a>JMS pro .NET

Prokázat JMS .NET zpráv:

* Spusťte aplikaci ukázkové .NET bez argumentů příkazového řádku.
* Spusťte aplikaci ukázkové Java příkazového řádku argumentem "sendonly". V tomto režimu aplikace nebudou zprávy ve frontě pouze odešle.
* Stisknutím klávesy **Enter** klikejte na v konzole aplikace Java, který má za následek odesílání zpráv.
* Tyto zprávy obdrží .NET aplikace.

### <a name="output-from-jms-application"></a>Výstup JMS aplikace

```
> java SimpleSenderReceiver sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with JMSMessageID = ID:4364096528752411591
Sent message with JMSMessageID = ID:459252991689389983
Sent message with JMSMessageID = ID:1565011046230456854
exit
```

### <a name="output-from-net-application"></a>Výstup z aplikace .NET

```
> SimpleSenderReceiver.exe  
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with MessageID = 4364096528752411591
Received message with MessageID = 459252991689389983
Received message with MessageID = 1565011046230456854
exit
```

## <a name="net-to-jms"></a>.NET JMS

Prokázat .NET JMS zpráv:

* Spusťte aplikaci ukázkové .NET s argumentem příkazového řádku "sendonly". V tomto režimu aplikace nebudou zprávy ve frontě pouze odešle.
* Spusťte aplikaci ukázkové Java bez všechny argumenty příkazového řádku.
* Stisknutím klávesy **Enter** klikejte na v konzole aplikace .NET, který má za následek odesílání zpráv.
* Tyto zprávy obdrží žádost Java.

#### <a name="output-from-net-application"></a>Výstup z aplikace .NET

```
> SimpleSenderReceiver.exe sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with MessageID = d64e681a310a48a1ae0ce7b017bf1cf3  
Sent message with MessageID = 98a39664995b4f74b32e2a0ecccc46bb
Sent message with MessageID = acbca67f03c346de9b7893026f97ddeb
exit
```

#### <a name="output-from-jms-application"></a>Výstup JMS aplikace

```
> java SimpleSenderReceiver 
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with JMSMessageID = ID:d64e681a310a48a1ae0ce7b017bf1cf3
Received message with JMSMessageID = ID:98a39664995b4f74b32e2a0ecccc46bb
Received message with JMSMessageID = ID:acbca67f03c346de9b7893026f97ddeb
exit
```

## <a name="unsupported-features-and-restrictions"></a>Nepodporované funkce a omezení

Při použití AMQP nejsou aktuálně podporované tyto funkce rozhraní API Bus .NET služby:

 * Transakce
 * Odeslat přenos cíl

Další informace najdete v článku [nepodporované funkce, omezení, chování rozdíly](service-bus-amqp-dotnet.md#unsupported-features-restrictions-and-behavioral-differences).

## <a name="summary"></a>Souhrn

Tento článek ukázal, jak získat přístup k funkcím zasílání zpráv služby Bus brokered (fronty a publikovat a přihlášení k odběru témata) z .NET pomocí AMQP 1.0 a rozhraní API služeb Bus .NET.

Můžete taky služby Bus AMQP 1.0 z jiných jazyků, včetně Java, C, Python a PHP. Součásti vytvořené pomocí těchto jazycích můžete vyměňovat zprávy problémy se spolehlivým a na úplné věrností pomocí AMQP 1.0 v Bus služby. Další informace najdete v tématu [Přehled AMQP Bus služby](service-bus-amqp-dotnet.md).

## <a name="next-steps"></a>Další kroky

Teď jste si projděte Přehled služby Bus a AMQP s .NET, v následujících tématech Další informace.

* [Podpora AMQP 1.0 v Bus služby Azure](service-bus-amqp-overview.md)
* [Používání jazyka Java zprávy služby (JMS) rozhraní API aplikace pomocí služby Bus & AMQP 1.0](service-bus-java-how-to-use-jms-api-amqp.md)
* [Jak používat službu Bus fronty](service-bus-dotnet-get-started-with-queues.md)
 
[Azure portálu]: https://portal.azure.com
