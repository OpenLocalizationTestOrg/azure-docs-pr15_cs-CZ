<properties
    pageTitle="Odesílání zpráv cloudu zařízení s IoT centrální | Microsoft Azure"
    description="Postupujte podle tohoto kurzu se dozvíte, jak k odesílání zpráv cloudu zařízení centrální IoT Azure pomocí jazyka Java."
    services="iot-hub"
    documentationCenter="java"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="java"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/13/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-send-cloud-to-device-messages-with-iot-hub-and-java"></a>Kurz: Jak odesílání zpráv cloudu zařízení s IoT centrální a Java

[AZURE.INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Úvod

Azure centrální IoT je plně spravovaných služba, která pomáhá povolit spolehlivý a zabezpečené obousměrné komunikaci mezi miliony IoT zařízení a ukončení aplikace zpět. Kurz [Seznámení s IoT rozbočovači] ukazuje, jak vytvořit rozbočovači IoT zřízení zařízení identit v něm a kód simulovaný zařízení, které odesílá zprávy zařízení do cloudu.

Tento kurz je založena na [Začínáme s IoT centrální]. To se dozvíte, jak chcete:

- Z vaší aplikace cloudu back-end odešlete zprávy cloudu zařízení jednoho zařízení pomocí IoT rozbočovače.
- Přijímání zpráv cloudu zařízení na zařízení.
- Z aplikace cloudu serverová, požádat o potvrzení o doručení (*názory*) u zpráv odeslaných z centrální IoT zařízení.

Další informace o zprávách cloudu zařízení můžete najít v [IoT centrální Developer Guide][IoT Hub Developer Guide - C2D].

Na konci tohoto kurzu spusťte dvě aplikace konzoly Java:

* **simulovaného zařízení**upravenou verzi aplikace vytvořené v [začít pracovat s IoT centrální], která se připojí k centrální IoT volání a přijímání zpráv cloudu zařízení.
* **odesílání c2d zpráv**, které odesílá zprávu cloudu zařízení simulovaný zařízení pomocí IoT rozbočovače a potom přijímá jeho potvrzení doručení.

> [AZURE.NOTE] Centrální IoT podporuje SDK pro mnoho platformy zařízení a jazyky (včetně C, Java Javascript) až SDK Azure IoT zařízení. Podrobný návod k připojení zařízení tento kurz kód a obecně k rozbočovači IoT Azure naleznete v článku [Středisko pro vývojáře IoT Azure].

Tento kurz, je potřeba k provedení těchto věcí:

+ Java SE 8. <br/> [Vývojové prostředí připravili] [ lnk-dev-setup] popisuje, jak nainstalovat Java pro účely tohoto návodu Windows nebo Linux.

+ Maven 3.  <br/> [Vývojové prostředí připravili] [ lnk-dev-setup] popisuje, jak nainstalovat Maven pro účely tohoto návodu Windows nebo Linux.

+ Účet Azure active. (Pokud nemáte účet, můžete vytvořit [bezplatný účet] [ lnk-free-trial] v jenom pár minut.)

## <a name="receive-messages-on-the-simulated-device"></a>Přijímání zpráv na simulovaný zařízení

V této části můžete změnit simulovaný zařízení aplikaci vytvořené v části [Začínáme s IoT centrální] do cloudu zařízení zprávy z centra IoT.

1. V textovém editoru, otevřete soubor simulated-device\src\main\java\com\mycompany\app\App.java.

2. Přidejte následující třídy **MessageCallback** jako vnořené třídy uvnitř třídy **aplikace** . **Spustit** metodu vyvolání přijaté zařízení zprávy od IoT rozbočovače. V tomto příkladu zařízení vždy s upozorněním centru splnění zprávy:

    ```
    private static class MessageCallback implements
    com.microsoft.azure.iothub.MessageCallback {
      public IotHubMessageResult execute(Message msg, Object context) {
        System.out.println("Received message from hub: "
          + new String(msg.getBytes(), Message.DEFAULT_IOTHUB_MESSAGE_CHARSET));

        return IotHubMessageResult.COMPLETE;
      }
    }
    ```

3. Změna **hlavní** metodu pro vytvoření **MessageCallback** instance a volat metodu **setMessageCallback** před otevřením klienta následujícím způsobem:

    ```
    client = new DeviceClient(connString, protocol);

    MessageCallback callback = new MessageCallback();
    client.setMessageCallback(callback, null);
    client.open();
    ```

    > [AZURE.NOTE] Pokud používáte HTTP místo MQTT nebo AMQP jako přenos, **DeviceClient** instance hledá zprávy od IoT centrální zřídka (nižší než každých 25 minut). Další informace o rozdílech mezi MQTT, AMQP a HTTP podporu a IoT centrální omezení, najdete v článku [IoT centrální Developer Guide][IoT Hub Developer Guide - C2D].

## <a name="send-a-cloud-to-device-message"></a>Odeslání zprávy cloudu zařízení

V této části Vytvoření aplikace konzoly Java zasílání zpráv cloudu zařízení aplikaci simulovaný zařízení. Potřebujete Id zařízení, které jste přidali v kurzu [Začínáme s IoT Centrum] zařízení. Budete potřebovat pro vaše Centrum IoT, které můžete najít v [Azure portál]připojovací řetězec.

1. Vytvoření projektu Maven s názvem **odesílání c2d zpráv** pomocí následujícího příkazu příkazového řádku. Poznámka: Jedná se o dlouhého příkaz:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=send-c2d-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Na příkazovém řádku přejděte do nová složka odesílání c2d zpráv.

3. V textovém editoru, otevřete soubor pom.xml ve složce odesílání c2d zpráv a přidejte následující závislost na uzel **závislosti** . Přidání závislost umožňuje použil funkci balíček **iothub java služby klienta** v aplikaci komunikovat s vašimi službami centrální IoT:

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-service-client</artifactId>
      <version>1.0.7</version>
    </dependency>
    ```

4. Uložte a zavřete soubor pom.xml.

5. V textovém editoru, otevřete soubor send-c2d-messages\src\main\java\com\mycompany\app\App.java.

6. Přidejte následující příkazy **importovat** soubor:

    ```
    import com.microsoft.azure.iot.service.sdk.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. Přidejte následující proměnné úrovni třídy třídy **aplikace** nahrazení **{yourhubconnectionstring}** a **{yourdeviceid}** hodnoty vaší vyznačená dříve:

    ```
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "{yourdeviceid}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQP;
    ```
    
8. **Hlavní** metoda nahraďte následující kód, který se připojí k centrální IoT, odešle zprávu do zařízení a počká potvrzení, že zařízení přijetí a zpracování zprávy:

    ```
    public static void main(String[] args) throws IOException,
        URISyntaxException, Exception {
      ServiceClient serviceClient = ServiceClient.createFromConnectionString(
        connectionString, protocol);
      
      if (serviceClient != null) {
        serviceClient.open();
        FeedbackReceiver feedbackReceiver = serviceClient
          .getFeedbackReceiver(deviceId);
        if (feedbackReceiver != null) feedbackReceiver.open();

        Message messageToSend = new Message("Cloud to device message.");
        messageToSend.setDeliveryAcknowledgement(DeliveryAcknowledgement.Full);

        serviceClient.send(deviceId, messageToSend);
        System.out.println("Message sent to device");

        FeedbackBatch feedbackBatch = feedbackReceiver.receive(10000);
        if (feedbackBatch != null) {
          System.out.println("Message feedback received, feedback time: "
            + feedbackBatch.getEnqueuedTimeUtc().toString());
        }

        if (feedbackReceiver != null) feedbackReceiver.close();
        serviceClient.close();
      }
    }
    ```

    > [AZURE.NOTE] V jednoduchosti saké tento kurz není implementovat zásady libovolný opakovat. Ve výrobním kódu, měli byste zásady opakovat (například exponenciální zdvojnásobení), jako navrhované v článku MSDN [Přechodná zpracování chyby].

## <a name="run-the-applications"></a>Spuštění aplikace

Teď jste připraveni ke spuštění aplikace.

1. Příkazového řádku ve složce simulovaného zařízení spusťte tento příkaz začátek odesílání telemetrie rozbočovače IoT a poslouchat cloudu zařízení zprávám odesílaným z vaší centrální:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Spusťte aplikaci simulovaný zařízení][img-simulated-device]

2. Příkazového řádku ve složce odesílání c2d zpráv spusťte tento příkaz Odeslat zprávu cloudu zařízení a počkejte potvrzení zpětné vazby:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Spusťte příkaz Odeslat zprávu cloudu zařízení][img-send-command]

## <a name="next-steps"></a>Další kroky

V tomto kurzu se naučíte odesílání a přijímání zpráv cloudu zařízení. 

Příklady úplné řešení začátku do konce, které používají IoT centrální najdete [Azure IoT sadu].

Další informace o vytváření řešení s IoT centrální, najdete v článku [IoT centrální vývojář Průvodce].


<!-- Images -->
[img-simulated-device]: media/iot-hub-java-java-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-java-java-c2d/sendc2d.png
<!-- Links -->

[Začínáme s IoT rozbočovači]: iot-hub-java-java-getstarted.md
[IoT Hub Developer Guide - C2D]: iot-hub-devguide-messaging.md
[Příručka IoT centrální vývojář]: iot-hub-devguide.md
[Středisko pro vývojáře IoT Azure]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-devbox-setup.md
[Zpracování přechodná poruch]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure portálu]: https://portal.azure.com
[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/