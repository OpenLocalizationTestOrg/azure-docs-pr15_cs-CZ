<properties 
    pageTitle="Jak pomocí rozhraní API služeb Bus Java AMQP 1.0 | Microsoft Azure" 
    description="Jak služba zprávu jazyka Java (JMS) pomocí Bus služby Azure a Upřesnit zpráva řízení fronty Protodol (AMQP) 1.0." 
    services="service-bus" 
    documentationCenter="java" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-the-java-message-service-jms-api-with-service-bus-and-amqp-10"></a>Jak Java zprávy služby (JMS) rozhraní API aplikace pomocí služby Bus a AMQP 1.0

Rozšířený zpráva řízení fronty Protocol (AMQP) 1.0 je efektivně spolehlivé, drátěný úrovni zpráv protokol, který můžete použít k vytváření robustních a platformách zasílání zpráv aplikace.

Podpora pro AMQP 1.0 v služby Bus znamená, že můžete použít shromažďování zpráv a publikovat a přihlášení k odběru brokered funkce zasílání zpráv z oblasti platformách pomocí efektivně binární protokolu. Kromě toho je možné vytvářet aplikace skládá součástí vytvořené pomocí kombinací jazyky, rámců a operační systémy.

Tento článek vysvětluje, jak používat službu Bus zpráv funkcí (fronty a publikovat a přihlášení k odběru témata) z aplikací Java pomocí Oblíbené Java zprávy služby (JMS) rozhraní API standardní. Existuje [companion článek](service-bus-dotnet-advanced-message-queuing.md) vysvětluje, jak podobně pomocí rozhraní API služeb Bus .NET. Můžete tyto dva příručky společně se naučit používat různé platformy zpráv pomocí AMQP 1.0.

## <a name="get-started-with-service-bus"></a>Začínáme s Bus služby

Tato příručka předpokládá, že už máte službu Bus obor názvů obsahující fronty s názvem **Frontě1**. Pokud nemáte, můžete [vytvořit obor názvů a fronty](service-bus-create-namespace-portal.md) pomocí [Azure portál](https://portal.azure.com). Podrobnosti o postupu při vytváření názvů Bus služby a fronty najdete v článku [používání služby Bus fronty](service-bus-dotnet-get-started-with-queues.md).

> [AZURE.NOTE] Rozdělený fronty a témata také podporují AMQP. Další informace najdete v tématu [Partitioned zpráv entity](service-bus-partitioning.md) a [AMQP 1.0 podpora služby Bus oddíly fronty a témata](service-bus-partitioned-queues-and-topics-amqp-overview.md).

## <a name="downloading-the-amqp-10-jms-client-library"></a>Stahování knihovny klienta AMQP 1.0 JMS

Informace o tom, odkud chcete stáhnout nejnovější verzi knihovnu Apache Qpid JMS AMQP 1.0 klienta Navštěvujte blog o [https://qpid.apache.org/download.html](https://qpid.apache.org/download.html).

Následující čtyři SKLENICE soubory z archivu rozdělení Apache Qpid JMS AMQP 1.0 musíte cesty tříd jazyka Java přidat při vytváření a spouštění aplikací JMS pomocí služby Bus:

- geronimo jms\_1.1\_specifikace 1.0.jar
- qpid-amqp-1-0-Client-[Version].JAR
- qpid-amqp-1-0-Client-jms-[Version].JAR
- qpid-amqp-1-0-Common-[Version].JAR

## <a name="coding-java-applications"></a>Kódování aplikace Java

### <a name="java-naming-and-directory-interface-jndi"></a>Pojmenování Java a rozhraní služby Directory (JNDI)

JMS slouží k vytváření oddělení logické názvy a pole fyzicky Java pojmenovávání a adresáře rozhraní (JNDI). Dva typy objektů JMS jsou převáděny JNDI: ConnectionFactory a cíl. JNDI používá poskytovatele model, do kterého můžete připojit různých adresářové služby pro zpracování úkolů rozlišení název. Formátování Apache Qpid JMS AMQP 1.0 knihovně je součástí jednoduché vlastnosti souborové JNDI poskytovatele nakonfigurovaný pomocí vlastnosti souboru z následujících akcí:

```
# servicebus.properties - sample JNDI configuration
    
# Register a ConnectionFactory in JNDI using the form:
# connectionfactory.[jndi_name] = [ConnectionURL]
connectionfactory.SBCF = amqps://[SASPolicyName]:[SASPolicyKey]@[namespace].servicebus.windows.net
    
# Register some queues in JNDI using the form
# queue.[jndi_name] = [physical_name]
# topic.[jndi_name] = [physical_name]
queue.QUEUE = queue1
```

#### <a name="configure-the-connectionfactory"></a>Konfigurace ConnectionFactory

Položka slouží k definování **ConnectionFactory** JNDI zprostředkovatel Qpid vlastnosti souboru je v tomto formátu:

```
connectionfactory.[jndi_name] = [ConnectionURL]
```

Kde **[jndi_name]** a **[ConnectionURL]** mají následující význam:

- **[jndi_name]**: logický název ConnectionFactory. Toto je název, který bude převést v aplikaci Java metodou JNDI IntialContext.lookup().
- **[ConnectionURL]**: adresy URL, které obsahuje knihovnu JMS informace potřebné k AMQP zprostředkovatele.

Formát **ConnectionURL** vypadá takto:

```
amqps://[SASPolicyName]:[SASPolicyKey]@[namespace].servicebus.windows.net
```
**[Názvů]**, **[SASPolicyName]** a **[SASPolicyKey]** místo, kam mají následující význam:

- **[názvů]**: Bus služby názvů.
- **[SASPolicyName]**: název zásady fronty sdílené podpis přístup.
- **[SASPolicyKey]**: klíč zásad fronty sdílené podpisu přístup.

> [AZURE.NOTE] Je třeba adresu URL kódovat heslo ručně. Užitečné nástroj kódování adresa URL je k dispozici v [http://www.w3schools.com/tags/ref_urlencode.asp](http://www.w3schools.com/tags/ref_urlencode.asp).

#### <a name="configure-destinations"></a>Konfigurace cíle

Položka slouží k definování cílové umístění v JNDI poskytovatele Qpid vlastnosti souboru je v tomto formátu:

```
queue.[jndi_name] = [physical_name]
```

nebo

```
topic.[jndi_name] = [physical_name]
```

Kde **[jndi\_název]** a **[fyzické\_název]** rozumí:

- **[jndi_name]**: logický název cíle. Toto je název, který bude převést v aplikaci Java metodou JNDI IntialContext.lookup().
- **[physical_name]**: název Bus služby entity, ke kterému aplikace odeslání nebo přijetí zprávy.

> [AZURE.NOTE] Při příjmu ze služby Bus téma předplatného, název pole fyzicky podle JNDI by měl být na název tématu. Vytvoření trvalé předplatné v kódu aplikace JMS je k dispozici na název předplatného. [Služba Bus AMQP 1.0 Příručka pro vývojáře](service-bus-amqp-dotnet.md) najdete další informace o práci s služby Bus téma předplatná z JMS.

### <a name="write-the-jms-application"></a>Napište JMS aplikaci

Neexistují žádné zvláštní rozhraní API nebo možnosti nutné v případě JMS pomocí služby Bus. Můžou ale nastat pár omezení, která se bude vztahovat později. Stejně jako u jakékoli aplikace JMS nejdřív thing požadovaná konfigurace JNDI prostředí, které mají vyřešit **ConnectionFactory** a cíle.

#### <a name="configure-the-jndi-initialcontext"></a>Konfigurace JNDI InitialContext

Konfigurace prostředí JNDI předáním hashtable informace o konfiguraci do konstruktoru třídy javax.naming.InitialContext. Dva požadované prvky v hashtable jsou název třídy počáteční Factory kontext a adresu URL poskytovatele. Následující kód ukazuje, jak nakonfigurovat prostředí JNDI pomocí vlastnosti souboru Qpid základě JNDI poskytovatele s do vlastnosti souboru nazvaného **servicebus.properties**.

```
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
InitialContext context = new InitialContext(env);
``` 

### <a name="a-simple-jms-application-using-a-service-bus-queue"></a>Jednoduchý JMS aplikace pomocí služby Bus fronty

V následujícím příkladu programu umožňuje JMS TextMessages do fronty služby Bus s názvem logické JNDI fronty, volání a přijímání zpráv zpět.

```
// SimpleSenderReceiver.java
    
import javax.jms.*;
import javax.naming.Context;
import javax.naming.InitialContext;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.Hashtable;
import java.util.Random;
    
public class SimpleSenderReceiver implements MessageListener {
    private static boolean runReceiver = true;
    private Connection connection;
    private Session sendSession;
    private Session receiveSession;
    private MessageProducer sender;
    private MessageConsumer receiver;
    private static Random randomGenerator = new Random();
    
    public SimpleSenderReceiver() throws Exception {
        // Configure JNDI environment
        Hashtable<String, String> env = new Hashtable<String, String>();
        env.put(Context.INITIAL_CONTEXT_FACTORY, 
                   "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory");
        env.put(Context.PROVIDER_URL, "servicebus.properties");
        Context context = new InitialContext(env);
    
        // Look up ConnectionFactory and Queue
        ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");
        Destination queue = (Destination) context.lookup("QUEUE");
    
        // Create Connection
        connection = cf.createConnection();
    
        // Create sender-side Session and MessageProducer
        sendSession = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
        sender = sendSession.createProducer(queue);
    
        if (runReceiver) {
            // Create receiver-side Session, MessageConsumer,and MessageListener
            receiveSession = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);
            receiver = receiveSession.createConsumer(queue);
            receiver.setMessageListener(this);
            connection.start();
        }
    }
    
    public static void main(String[] args) {
        try {
    
            if ((args.length > 0) && args[0].equalsIgnoreCase("sendonly")) {
                runReceiver = false;
            }
    
            SimpleSenderReceiver simpleSenderReceiver = new SimpleSenderReceiver();
            System.out.println("Press [enter] to send a message. Type 'exit' + [enter] to quit.");
            BufferedReader commandLine = new java.io.BufferedReader(new InputStreamReader(System.in));
    
            while (true) {
                String s = commandLine.readLine();
                if (s.equalsIgnoreCase("exit")) {
                    simpleSenderReceiver.close();
                    System.exit(0);
                } else {
                    simpleSenderReceiver.sendMessage();
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    
    private void sendMessage() throws JMSException {
        TextMessage message = sendSession.createTextMessage();
        message.setText("Test AMQP message from JMS");
        long randomMessageID = randomGenerator.nextLong() >>>1;
        message.setJMSMessageID("ID:" + randomMessageID);
        sender.send(message);
        System.out.println("Sent message with JMSMessageID = " + message.getJMSMessageID());
    }
    
    public void close() throws JMSException {
        connection.close();
    }
    
    public void onMessage(Message message) {
        try {
            System.out.println("Received message with JMSMessageID = " + message.getJMSMessageID());
            message.acknowledge();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}   
```

### <a name="run-the-application"></a>Spusťte aplikaci

Spuštění aplikace vytvoří výstup formuláře:

```
> java SimpleSenderReceiver
Press [enter] to send a message. Type 'exit' + [enter] to quit.
    
Sent message with JMSMessageID = ID:2867600614942270318
Received message with JMSMessageID = ID:2867600614942270318
    
Sent message with JMSMessageID = ID:7578408152750301483
Received message with JMSMessageID = ID:7578408152750301483
    
Sent message with JMSMessageID = ID:956102171969368961
Received message with JMSMessageID = ID:956102171969368961
exit
```

## <a name="cross-platform-messaging-between-jms-and-net"></a>Různé platformy zasílání zpráv mezi JMS a .NET

Tato příručka ukázal, jak odesílání a přijímání zpráv do a z služby Bus pomocí JMS. Však jednou z hlavních výhod AMQP 1.0 je, že umožňuje aplikace integrované z komponenty napsané v různých jazycích u těch, které jste si vyměnili problémy se spolehlivým a u celé věrností.

Pomocí aplikace JMS vzorku jsme je popsali výše a podobné .NET aplikace z průvodce Průvodce vyhledáváním, [jak používat AMQP 1.0 pomocí rozhraní API .NET služby Bus .NET](service-bus-dotnet-advanced-message-queuing.md)si můžete vyměňovat zpráv mezi .NET a Java. 

Další informace o podrobnosti a platformách zasílání zpráv pomocí služby Bus a AMQP 1.0 najdete v článku [služby Bus AMQP 1.0 Příručka pro vývojáře](service-bus-amqp-dotnet.md).

### <a name="jms-to-net"></a>JMS pro .NET

Prokázat JMS .NET zpráv:

* Spusťte aplikaci ukázkové .NET bez argumentů příkazového řádku.
* Spusťte aplikaci ukázkové Java příkazového řádku argumentem "sendonly". V tomto režimu aplikace nebudou zprávy ve frontě pouze odešle.
* Stisknutím klávesy **Enter** klikejte na v konzole aplikace Java, který má za následek odesílání zpráv.
* Tyto zprávy obdrží .NET aplikace.

#### <a name="output-from-jms-application"></a>Výstup JMS aplikace

```
> java SimpleSenderReceiver sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with JMSMessageID = ID:4364096528752411591
Sent message with JMSMessageID = ID:459252991689389983
Sent message with JMSMessageID = ID:1565011046230456854
exit
```

#### <a name="output-from-net-application"></a>Výstup z aplikace .NET

```
> SimpleSenderReceiver.exe  
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with MessageID = 4364096528752411591
Received message with MessageID = 459252991689389983
Received message with MessageID = 1565011046230456854
exit
```

### <a name="net-to-jms"></a>.NET JMS

Prokázat .NET JMS zpráv:

* Spusťte aplikaci ukázkové .NET příkazového řádku argumentem "sendonly". V tomto režimu aplikace nebudou zprávy ve frontě pouze odešle.
* Spusťte aplikaci ukázkové Java bez argumentů příkazového řádku.
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

Při používání JMS přes AMQP 1.0 s Bus služby, a to existují následující omezení:

* Pouze s jedním **MessageProducer** nebo **MessageConsumer** je povolených pro jednoho **relace**. Pokud je potřeba vytvořit více **MessageProducers** nebo **MessageConsumers** v aplikaci, vytvořte vyhrazené **relaci** pro každý z nich.
* Stále přepočítávané téma předplatné aktuálně nepodporuje.
* **MessageSelectors** aktuálně nepodporuje.
* Dočasné míst, tedy **TemporaryQueue**, **TemporaryTopic** nepodporuje doplňky aktuálně, spolu s **QueueRequestor** a **TopicRequestor** rozhraní API, která je použít.
* Transakční relace a distribuované transakce nejsou podporované.

## <a name="summary"></a>Souhrn

Tato příručka, postupy ukázal používání služby Bus brokered zpráv funkcí (fronty a publikovat a přihlášení k odběru témata) z Java pomocí rozhraní API Oblíbené JMS a AMQP 1.0.

Můžete taky služby Bus AMQP 1.0 z jiných jazyků, včetně .NET, C, Python nebo PHP. Součásti vytvořené pomocí těchto jazyky můžete vyměňovat zprávy problémy se spolehlivým a na úplné věrností pomocí AMQP 1.0 podpory pro službu Bus. Další informace najdete v tématu [služby Bus AMQP 1.0 Příručka pro vývojáře](service-bus-amqp-dotnet.md).

## <a name="next-steps"></a>Další kroky

* [Podpora AMQP 1.0 v Bus služby Azure](service-bus-amqp-overview.md)
* [Jak pomocí rozhraní API služeb Bus .NET AMQP 1.0](service-bus-dotnet-advanced-message-queuing.md)
* [Služby Bus AMQP 1.0 Příručka pro vývojáře](service-bus-amqp-dotnet.md)
* [Jak používat službu Bus fronty](service-bus-dotnet-get-started-with-queues.md)
* [Středisko pro vývojáře Java](/develop/java/).



 
