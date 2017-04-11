<properties
    pageTitle="Azure centrální IoT jazyka Java Začínáme | Microsoft Azure"
    description="Azure centrální IoT s Java začíná začali kurz. Pomocí Azure IoT centrální a Java SDK Microsoft Azure IoT implementovat řešení pro Internet věci."
    services="iot-hub"
    documentationCenter="java"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="java"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/11/2016"
     ms.author="dobett"/>

# <a name="get-started-with-azure-iot-hub-for-java"></a>Začínáme s Azure IoT centrální jazyka Java

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Na konci tohoto kurzu máte tři Java konzoly aplikace:

* **vytvoření identity zařízení**, která vytvoří zařízení identit a klíč přidružené zabezpečení připojení simulovaný zařízení.
* **čtení d2c zpráv**, které zobrazuje telemetrie odeslaný simulovaný zařízení.
* **simulovaného zařízení**, které připojí k rozbočovači IoT zařízení identit dříve vytvořili a odešle zprávu telemetrie každý druhý pomocí protokolu AMQP.

> [AZURE.NOTE] Článek [IoT centrální SDK] [ lnk-hub-sdks] obsahuje informace o různých SDK, které můžete použít k vytvoření obou spouštět aplikace na zařízeních a vašeho řešení back-end.

Tento kurz, je potřeba k provedení těchto věcí:

+ Java SE 8. <br/> [Vývojové prostředí připravili] [ lnk-dev-setup] popisuje, jak nainstalovat Java pro účely tohoto návodu Windows nebo Linux.

+ Maven 3.  <br/> [Vývojové prostředí připravili] [ lnk-dev-setup] popisuje, jak nainstalovat Maven pro účely tohoto návodu Windows nebo Linux.

+ Účet Azure active. (Pokud nemáte účet, můžete vytvořit [bezplatný účet] [ lnk-free-trial] v jenom pár minut.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Jako poslední krok poznamenejte si hodnotu **primárního klíče** a klikněte na **zasílání zpráv**. Na zásuvné **zprávy** si poznamenejte **název události kompatibilního centrální** a **kompatibilní s prohlížečem události centrální koncového bodu**. Při vytváření aplikace **pro čtení d2c zprávy** musíte tyto tři hodnoty.

![Azure portálu zásuvné IoT Centrum zpráv][6]

Teď jste vytvořili rozbočovače IoT a máte IoT centrální hostname, IoT centrální připojovací řetězec, IoT centrální primární klíč, kompatibilní s prohlížečem události centrální název a koncový bod kompatibilní s prohlížečem události centrální potřebné k dokončení tohoto kurzu.

## <a name="create-a-device-identity"></a>Vytvoření zařízení identity

V této části vytvoříte aplikace konzoly Java, která vytvoří novou identitu zařízení v registru identity rozbočovače IoT. Zařízení nemůže připojit k centrální IoT, pokud má položka registru identity zařízení. Přečtěte si část **Zařízení Identity registru** [IoT centrální Developer Guide] [ lnk-devguide-identity] Další informace. Při spuštění této aplikace konzoly vygeneruje jedinečný zařízení ID a klávesy, která zařízení můžete použít k identifikaci k zasílání zpráv zařízení do cloudu k rozbočovači IoT.

1. Vytvořte novou prázdnou složku s názvem iot-java-get začít. Ve složce iot java-get spouštěné vytvořte nový projekt Maven s názvem **vytvoření identity zařízení** pomocí následujícího příkazu příkazového řádku. Poznámka: Jedná se o dlouhého příkaz:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=create-device-identity -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Na příkazovém řádku přejděte na nová složka vytvořte identity zařízení.

3. V textovém editoru, otevřete soubor pom.xml ve složce vytvořit zařízení identit a přidejte následující závislost na uzel **závislosti** . To umožňuje použil funkci balíček iothub služby sdk v aplikaci:

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-service-client</artifactId>
      <version>1.0.9</version>
    </dependency>
    ```
    
4. Uložte a zavřete soubor pom.xml.

5. V textovém editoru, otevřete soubor create-device-identity\src\main\java\com\mycompany\app\App.java.

6. Přidejte následující příkazy **importovat** soubor:

    ```
    import com.microsoft.azure.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.iot.service.sdk.Device;
    import com.microsoft.azure.iot.service.sdk.RegistryManager;

    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. Přidejte následující proměnné úrovni třídy třídy **aplikace** nahrazení vaší vyznačená dříve **{yourhubconnectionstring}** s hodnotou:

    ```
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "myFirstJavaDevice";
    
    ```
    
8. Úprava podpisu **hlavní** metoda zahrnout výjimky následujícím způsobem:

    ```
    public static void main( String[] args ) throws IOException, URISyntaxException, Exception
    ```
    
9. Přidejte následující kód v těle zprávy **hlavní** metody. Tento kód vytvoří zařízení s názvem *javadevice* v registru identity IoT centrální, pokud ještě neexistuje. Potom zobrazí id zařízení a klíč, který budete potřebovat později:

    ```
    RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);

    Device device = Device.createFromId(deviceId, null, null);
    try {
      device = registryManager.addDevice(device);
    } catch (IotHubException iote) {
      try {
        device = registryManager.getDevice(deviceId);
      } catch (IotHubException iotf) {
        iotf.printStackTrace();
      }
    }
    System.out.println("Device id: " + device.getDeviceId());
    System.out.println("Device key: " + device.getPrimaryKey());
    ```

10. Uložte a zavřete soubor App.java.

11. Vytvoření aplikace **vytvořit zařízení identit** pomocí Maven, spusťte následující příkaz příkazového řádku ve složce vytvořit zařízení identit:

    ```
    mvn clean package -DskipTests
    ```

12. Spusťte aplikaci **vytvořit zařízení identit** Maven, spusťte následující příkaz příkazového řádku ve složce vytvořit zařízení identit:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

13. Poznamenejte si **id zařízení** a **klíče zařízení**. Budete potřebovat tyto novější při vytváření aplikace, ke kterému je připojen k rozbočovači IoT jako zařízení.

> [AZURE.NOTE] Registru identity IoT centrální pouze ukládá zařízení identit povolit zabezpečený přístup k centru. Slouží k ukládání ID zařízení a klávesy se šipkami použít jako zabezpečovací přihlašovací údaje a povolit nebo zakázat příznak, který slouží k zakázání přístupu pro jednotlivé zařízení. Pokud aplikace potřebuje k ukládání ostatních metadatech specifické pro zařízení, používejte úložiště specifické pro aplikaci. [Karta Vývojář v] příručce IoT centrální[ lnk-devguide-identity] Další informace.

## <a name="receive-device-to-cloud-messages"></a>Zprávy zařízení do cloudu

V této části Vytvoření aplikace konzoly Java, který bude číst zprávy zařízení do cloudu z centrální IoT. Rozbočovači IoT zpřístupňuje [Události centrální][lnk-event-hubs-overview]-kompatibilní koncový bod umožňující přečtených zprávách: zařízení do cloudu. Jednodušší jednoduché, vytvoří tento kurz základní reader, který není vhodné pro nasazení vysoký výkon. [Zpracování zpráv zařízení do cloudu] [ lnk-process-d2c-tutorial] kurzu se dozvíte, jak ke zpracování zpráv ve velkém měřítku zařízení do cloudu. [Začínáme s události rozbočovače] [ lnk-eventhubs-tutorial] kurz poskytuje další informace o tom, k zpracování zpráv z rozbočovače události a platí pro koncové body kompatibilní s prohlížečem IoT centrální události centrální.

> [AZURE.NOTE] Kompatibilní s prohlížečem události centrální koncový bod pro čtení zpráv zařízení do cloudu vždy používá protokol AMQP.

1. V iot java-get spouštěné složku, kterou jste vytvořili v části *Vytvoření zařízení identit* vytvořte nový projekt Maven jen **pro čtení d2c zprávy** pomocí následujícího příkazu příkazového řádku. Poznámka: Jedná se o dlouhého příkaz:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Na příkazovém řádku přejděte na nová složka čtení d2c zpráv.

3. V textovém editoru, otevřete soubor pom.xml ve složce čtení d2c zpráv a přidejte následující závislost na uzel **závislosti** . To umožňuje použil funkci balíček eventhubs klienta v aplikaci číst ze kompatibilní s prohlížečem události centrální koncového bodu:

    ```
    <dependency> 
        <groupId>com.microsoft.azure</groupId> 
        <artifactId>azure-eventhubs</artifactId> 
        <version>0.8.2</version> 
    </dependency>
    ```

4. Uložte a zavřete soubor pom.xml.

5. V textovém editoru, otevřete soubor read-d2c-messages\src\main\java\com\mycompany\app\App.java.

6. Přidejte následující příkazy **importovat** soubor:

    ```
    import java.io.IOException;
    import com.microsoft.azure.eventhubs.*;
    import com.microsoft.azure.servicebus.*;
    
    import java.io.IOException;
    import java.nio.charset.Charset;
    import java.time.*;
    import java.util.Collection;
    import java.util.concurrent.ExecutionException;
    import java.util.function.*;
    import java.util.logging.*;
    ```

7. Přidejte následující proměnné úrovni třídy třídy **aplikace** . Nahraďte hodnoty, které je uvedeno dříve **{youriothubkey}**, **{youreventhubcompatibleendpoint}**a **{youreventhubcompatiblename}** :

    ```
    private static String connStr = "Endpoint={youreventhubcompatibleendpoint};EntityPath={youreventhubcompatiblename};SharedAccessKeyName=iothubowner;SharedAccessKey={youriothubkey}";
    ```

8. Přidejte následující metodu **receiveMessages** třídy **aplikace** . Tato metoda vytváří instanci **EventHubClient** připojení k kompatibilní s prohlížečem události centrální koncového bodu a asynchronní vytváří instanci **PartitionReceiver** číst z oddílu centrální události. Cykly nepřetržitě a ukončení aplikace, dokud se vytiskne podrobnosti zprávy.

    ```
    private static EventHubClient receiveMessages(final String partitionId)
    {
      EventHubClient client = null;
      try {
        client = EventHubClient.createFromConnectionStringSync(connStr);
      }
      catch(Exception e) {
        System.out.println("Failed to create client: " + e.getMessage());
        System.exit(1);
      }
      try {
        client.createReceiver( 
          EventHubClient.DEFAULT_CONSUMER_GROUP_NAME,  
          partitionId,  
          Instant.now()).thenAccept(new Consumer<PartitionReceiver>()
        {
          public void accept(PartitionReceiver receiver)
          {
            System.out.println("** Created receiver on partition " + partitionId);
            try {
              while (true) {
                Iterable<EventData> receivedEvents = receiver.receive(100).get();
                int batchSize = 0;
                if (receivedEvents != null)
                {
                  for(EventData receivedEvent: receivedEvents)
                  {
                    System.out.println(String.format("Offset: %s, SeqNo: %s, EnqueueTime: %s", 
                      receivedEvent.getSystemProperties().getOffset(), 
                      receivedEvent.getSystemProperties().getSequenceNumber(), 
                      receivedEvent.getSystemProperties().getEnqueuedTime()));
                    System.out.println(String.format("| Device ID: %s", receivedEvent.getProperties().get("iothub-connection-device-id")));
                    System.out.println(String.format("| Message Payload: %s", new String(receivedEvent.getBody(),
                      Charset.defaultCharset())));
                    batchSize++;
                  }
                }
                System.out.println(String.format("Partition: %s, ReceivedBatch Size: %s", partitionId,batchSize));
              }
            }
            catch (Exception e)
            {
              System.out.println("Failed to receive messages: " + e.getMessage());
            }
          }
        });
      }
      catch (Exception e)
      {
        System.out.println("Failed to create receiver: " + e.getMessage());
      }
      return client;
    }
    ```

    > [AZURE.NOTE] Tento způsob použití filtru při vytváření příjemce, aby příjemce přečte pouze zprávy poslané na IoT centrální po spuštění příjemce. To je užitečné v testovacím prostředí, takže uvidíte aktuální sadu zpráv. V prostředí výroby kódu Ujistěte se, že zpracuje všechny zprávy – přečtěte si, [jak zpracování zpráv IoT Centrum zařízení do cloudu] [ lnk-process-d2c-tutorial] výuková Další informace.

9. Úprava podpisu **hlavní** metoda zahrnout výjimky následujícím způsobem:

    ```
    public static void main( String[] args ) throws IOException
    ```

10. Přidejte následující kód **hlavní** metody ve třídě **aplikace** . Tento kód vytvoří dvě instance **EventHubClient** a **PartitionReceiver** a umožňuje ukončete aplikaci, když dokončíte zpracování zpráv:

    ```
    EventHubClient client0 = receiveMessages("0");
    EventHubClient client1 = receiveMessages("1");
    System.out.println("Press ENTER to exit.");
    System.in.read();
    try
    {
      client0.closeSync();
      client1.closeSync();
      System.exit(0);
    }
    catch (ServiceBusException sbe)
    {
      System.exit(1);
    }
    ```

    > [AZURE.NOTE] Tento kód předpokládá, že jste vytvořili rozbočovače IoT ve vrstvě F1 (zdarma). Bezplatné rozbočovač IoT obsahuje dva oddíly s názvem "0" a "1".

11. Uložte a zavřete soubor App.java.

12. Vytvoření aplikace **pro čtení d2c zprávy** pomocí Maven, spusťte následující příkaz příkazového řádku ve složce čtení d2c zpráv:

    ```
    mvn clean package -DskipTests
    ```

## <a name="create-a-simulated-device-app"></a>Vytvoření aplikace simulovaný zařízení

V této části vytvoříte aplikace konzoly Java, který napodobuje zařízení, které odesílá zařízení do cloudu zprávy k rozbočovači IoT.

1. V iot java-get spouštěné složku, kterou jste vytvořili v části *Vytvoření zařízení identit* vytvořte nový projekt Maven s názvem **simulovaného zařízení** pomocí následujícího příkazu příkazového řádku. Poznámka: Jedná se o dlouhého příkaz:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Na příkazovém řádku přejděte do nové složky simulovaného zařízení.

3. V textovém editoru, otevřete soubor pom.xml ve složce simulovaného zařízení a přidejte následující závislosti uzel **závislosti** . To umožňuje použil funkci balíček iothub java klienta v aplikaci komunikovat s rozbočovače IoT a serializovat Java objektů k JSON:

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-device-client</artifactId>
      <version>1.0.14</version>
    </dependency>
    <dependency>
      <groupId>com.google.code.gson</groupId>
      <artifactId>gson</artifactId>
      <version>2.3.1</version>
    </dependency>
    ```

4. Uložte a zavřete soubor pom.xml.

5. V textovém editoru, otevřete soubor simulated-device\src\main\java\com\mycompany\app\App.java.

6. Přidejte následující příkazy **importovat** soubor:

    ```
    import com.microsoft.azure.iothub.DeviceClient;
    import com.microsoft.azure.iothub.IotHubClientProtocol;
    import com.microsoft.azure.iothub.Message;
    import com.microsoft.azure.iothub.IotHubStatusCode;
    import com.microsoft.azure.iothub.IotHubEventCallback;
    import com.microsoft.azure.iothub.IotHubMessageResult;
    import com.google.gson.Gson;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Random;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```

7. Přidejte následující proměnné úrovni třídy **aplikace** onenotový **{youriothubname}** nahraďte svým jménem centrální IoT a **{yourdevicekey}** hodnotou zařízení klíčové vytvořeného v části *vytvoření identity zařízení* :

    ```
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myFirstJavaDevice;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.AMQP;
    private static String deviceId = "myFirstJavaDevice";
    private static DeviceClient client;
    ```

    Tato ukázková aplikace používá proměnnou **protokol** při vytvoří **DeviceClient** objektu. Protokol HTTP nebo AMQP slouží ke komunikaci s IoT centrální.

8. Přidejte že následující vnořené **TelemetryDataPoint** třídy uvnitř třídy **aplikace** můžete určit, které zařízení odešle rozbočovače IoT telemetrickými daty:

    ```
    private static class TelemetryDataPoint {
      public String deviceId;
      public double windSpeed;
        
      public String serialize() {
        Gson gson = new Gson();
        return gson.toJson(this);
      }
    }
    ```

9. Přidejte následující vnořené **EventCallback** třídě uvnitř třídy **aplikace** zobrazit stav potvrzení vracející centru IoT při zpracování zprávy z simulovaný zařízení. Tento způsob také oznámí, že hlavní podproces v aplikaci zpracovat zprávu:

    ```
    private static class EventCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to message with status: " + status.name());
      
        if (context != null) {
          synchronized (context) {
            context.notify();
          }
        }
      }
    }
    ```

10. Přidejte následující vnořené **MessageSender** třídě uvnitř třídy **aplikace** . Metoda **Spustit** v této třídě vygeneruje ukázkové telemetrickými daty odešlete rozbočovače IoT a čeká odpověď na odeslanou zprávu před odesláním zprávy následující:

    ```
    private static class MessageSender implements Runnable {
      public volatile boolean stopThread = false;
      
      public void run()  {
        try {
          double avgWindSpeed = 10; // m/s
          Random rand = new Random();
          
          while (!stopThread) {
            double currentWindSpeed = avgWindSpeed + rand.nextDouble() * 4 - 2;
            TelemetryDataPoint telemetryDataPoint = new TelemetryDataPoint();
            telemetryDataPoint.deviceId = deviceId;
            telemetryDataPoint.windSpeed = currentWindSpeed;
            
            String msgStr = telemetryDataPoint.serialize();
            Message msg = new Message(msgStr);
            System.out.println("Sending: " + msgStr);
            
            Object lockobj = new Object();
            EventCallback callback = new EventCallback();
            client.sendEventAsync(msg, callback, lockobj);
            
            synchronized (lockobj) {
              lockobj.wait();
            }
            Thread.sleep(1000);
          }
        } catch (InterruptedException e) {
          System.out.println("Finished.");
        }
      }
    }
    ```

    Tento způsob odešle novou zprávu zařízení do cloudu jedné sekundy po centru IoT potvrdí na předchozí zprávu. Zpráva obsahuje serializován JSON objekt s deviceId a náhodně generovaným číslem tak, aby napodobily senzoru rychlost větru.

11. **Hlavní** metoda nahraďte následující kód, který vytváří vlákno k odesílání zpráv zařízení do cloudu na vaše Centrum IoT:

    ```
    public static void main( String[] args ) throws IOException, URISyntaxException {
      client = new DeviceClient(connString, protocol);
      client.open();

      MessageSender sender = new MessageSender();

      ExecutorService executor = Executors.newFixedThreadPool(1);
      executor.execute(sender);

      System.out.println("Press ENTER to exit.");
      System.in.read();
      executor.shutdownNow();
      client.close();
    }
    ```

12. Uložte a zavřete soubor App.java.

13. Vytvoření aplikace **simulovaného zařízení** pomocí Maven, spusťte následující příkaz příkazového řádku ve složce simulovaného zařízení:

    ```
    mvn clean package -DskipTests
    ```

> [AZURE.NOTE] Jednodušší jednoduché, tento kurz není implementovat zásady libovolný opakovat. Ve výrobním kódu, měli byste zásady opakovat (například exponenciální zdvojnásobení) jako navrhované v článku MSDN [Přechodná zpracování poruch][lnk-transient-faults].

## <a name="run-the-applications"></a>Spuštění aplikace

Teď jste připraveni ke spuštění aplikace.

1. Příkazového řádku ve složce d2c pro čtení spusťte tento příkaz zahájíte sledování první oddíl v centrální IoT:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Centrální IoT Java služby klientské aplikace pro sledování zpráv zařízení do cloudu][7]

2. Příkazového řádku ve složce simulovaného zařízení spusťte tento příkaz začněte posílat rozbočovače IoT telemetrickými daty:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Rozbočovač IoT Java zařízení klientské aplikace k odesílání zpráv zařízení do cloudu][8]

3. **Použití** dlaždice [Azure portál] [ lnk-portal] zobrazuje počet odeslaných k Centru zpráv:

    ![Azure portálu použití dlaždic znázorňující počet odeslaných zpráv k rozbočovači IoT][43]

## <a name="next-steps"></a>Další kroky

V tomto kurzu nakonfigurované rozbočovači nové IoT Azure portálu a vytvoří zařízení identit v registru identity centrální. K povolení aplikace simulovaný zařízení k odesílání zpráv zařízení do cloudu k rozbočovači použít tuto identitu zařízení. Aplikaci, která se zobrazí na zprávy přijaté centru taky vytvořili. 

Pokračujte začínáte pracovat s IoT centrální a k prohlížení najdete v ostatních případech IoT:

- [Připojení zařízení][lnk-connect-device]
- [Začínáme používat správu zařízení][lnk-device-management]
- [Začínáme s SDK IoT brány][lnk-gateway-SDK]

Informace o prodloužení IoT řešení a zpracování zpráv zařízení do cloudu ve velkém měřítku, najdete v článku [zpracování zpráv zařízení do cloudu] [ lnk-process-d2c-tutorial] kurz.

<!-- Images. -->
[6]: ./media/iot-hub-java-java-getstarted/create-iot-hub6.png
[7]: ./media/iot-hub-java-java-getstarted/runapp1.png
[8]: ./media/iot-hub-java-java-getstarted/runapp2.png
[43]: ./media/iot-hub-java-java-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-devbox-setup.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/