<properties
    pageTitle="Zpracování zpráv zařízení do cloudu IoT centrální (Java) | Microsoft Azure"
    description="Podle jazyka Java kurzu se dozvíte užitečné vzorků pro zpracování zpráv IoT Centrum zařízení do cloudu."
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
     ms.date="09/01/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-process-iot-hub-device-to-cloud-messages-using-java"></a>Kurz: Způsobu zpracování zpráv IoT Centrum zařízení do cloudu pomocí jazyka Java

[AZURE.INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

## <a name="introduction"></a>Úvod

Azure centrální IoT je plně spravovaných služba, která umožňuje spolehlivý a zabezpečené obousměrnou komunikaci mezi miliony IoT zařízení a aplikace serverová. Další kurzy ([začít pracovat s IoT centrální] a [posílání zpráv cloudu zařízení s IoT centrální][lnk-c2d]) ukazují, jak použít základní funkce zařízení do cloudu a cloudu zařízení zpráv IoT rozbočovače.

Tento kurz je založena na kód zobrazený v kurz [Seznámení s IoT centrální] a zobrazí dvě scalable vzorky, které můžete použít ke zpracování zpráv zařízení do cloudu:

- Spolehlivé ukládání zpráv zařízení do cloudu v [úložišti objektů blob Azure]. Běžné situace je *studenou cestu* analýzy, ve které máte uložené telemetrickými daty v objektů BLOB můžete předávat na vstupu do analýzy procesů. Tyto procesy můžou úsilím pomocí nástrojů ATP [Factory dat Azure] [HDInsight (Hadoop)] vrstvě.

- Spolehlivé zpracování zpráv *interaktivní* zařízení do cloudu. Zařízení do cloudu zprávy jsou interaktivní okamžiku, kdy je okamžité aktivační události pro sady akcí v aplikaci back-end. Zařízení může třeba poslat zprávu upozornění, která aktivuje vkládání lístek na CRM systému. Naopak *datový bod* zprávy jednoduše kanálu sociální sítě do modul analýzy. Například telemetrie teploty ze zařízení, které je uložený pro pozdější analýzu je zpráva datový bod.

Protože IoT Centrum poskytuje [Události centrální][lnk-event-hubs]-kompatibilní koncový bod, aby se zprávy zařízení do cloudu, tohoto návodu používá instanci [EventProcessorHost] . Tato instance:

* Problémy se spolehlivým ukládá zprávy *datového bodu* v úložišti objektů blob Azure.
* K předávání *interaktivní* zařízení do cloudu zpráv Azure [fronty Bus služby] pro zpracování okamžitě.

Služba Bus zajistit spolehlivé zpracování interaktivní zpráv poskytuje jednotlivé zprávy kontroly a rušení duplikace na základě okno čas.

> [AZURE.NOTE] Instanci **EventProcessorHost** je pouze jeden způsob zpracování interaktivní zpráv. Další možnosti zahrnují [Struktury služby Azure] [ lnk-service-fabric] a [Analýzy toku Azure][lnk-stream-analytics].

Na konci tohoto kurzu spusťte tři Java konzoly aplikace:

* **simulovaného zařízení**upravenou verzi aplikace vytvořený v tématu [Začínáme s IoT centrální] kurz odešle datový bod zařízení do cloudu zprávy každý druhý a interaktivní zprávy zařízení do cloudu každých 10 sekund. Tato aplikace používá pro komunikaci s IoT centrální protokol AMQP.
* **proces d2c zprávy** používá třídu [EventProcessorHost] pro vzkazy z koncového bodu kompatibilní s prohlížečem události centrální. Pak problémy se spolehlivým ukládá zprávy datového bodu v úložišti objektů blob Azure a předá interaktivní zprávy do fronty Bus služby.
* **zpracování interaktivní zpráv** zrušte fronty interaktivní zprávy od fronty Bus služby.

> [AZURE.NOTE] Rozbočovač IoT podporuje SDK pro mnoho platformy zařízení a jazyků, včetně C, Java a JavaScript. Pokyny nahraďte simulovaný zařízení v tomto kurzu fyzické zařízení a připojte zařízení k rozbočovači IoT naleznete v článku [Středisko pro vývojáře IoT Azure].

Tento kurz přímo platí pro další způsoby, jak používat kompatibilní s prohlížečem události Centrum zpráv, třeba projekty [HDInsight (Hadoop)] . Další informace najdete v [příručce vývojář Azure IoT centrální - zařízení do cloudu].

Tento kurz, je potřeba k provedení těchto věcí:

+ Úplnou verzi pracovní kurz [Seznámení s IoT centrální] .

+ Java SE 8. <br/> [Vývojové prostředí připravili] [ lnk-dev-setup] popisuje, jak nainstalovat Java pro účely tohoto návodu Windows nebo Linux.

+ Maven 3.  <br/> [Vývojové prostředí připravili] [ lnk-dev-setup] popisuje, jak nainstalovat Maven pro účely tohoto návodu Windows nebo Linux.

+ Účet Azure active. <br/>Pokud nemáte účet, můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free/) v jenom pár minut.

Měli byste mít některé základní znalost [Azure úložiště] a [Bus služby Azure].


## <a name="send-interactive-messages-from-a-simulated-device"></a>Odesílání interaktivní zpráv z simulovaný zařízení

V této části můžete změnit simulovaný zařízení aplikaci, kterou jste vytvořili v kurzu [Začínáme s IoT centrální] interaktivní zařízení do cloudu zprávy odešlete centru IoT.

1. Umožňuje otevřít soubor simulated-device\src\main\java\com\mycompany\app\App.java textovém editoru. Tento soubor obsahuje kód **simulovaného zařízení** aplikaci, kterou jste vytvořili v kurzu [Začínáme s IoT centrální] .

2. Přidejte následující vnořené třídy třídy **aplikace** :

    ```
    private static class InteractiveMessageSender implements Runnable {
      public void run() {
        try {
          while (true) {
            String msgStr = "Alert message!";
            Message msg = new Message(msgStr);
            msg.setMessageId(java.util.UUID.randomUUID().toString());
            msg.setProperty("messageType", "interactive");
            System.out.println("Sending interactive message: " + msgStr);

            Object lockobj = new Object();
            EventCallback callback = new EventCallback();
            client.sendEventAsync(msg, callback, lockobj);

            synchronized (lockobj) {
              lockobj.wait();
            }
            Thread.sleep(10000);
          }
        } catch (InterruptedException e) {
          System.out.println("Finished sending interactive messages.");
        }
      }
    }
    ```

    Tento předmětu je podobný třídy **MessageSender** v aplikaci project **simulovaného zařízení** . Pouze rozdíly se, že teď nastavte vlastnost systém **MessageId** , se vlastní vlastnost s názvem **Typ zprávy**.
    Kód přiřadí vlastnost **MessageId** identifikátor (UUID). Služba Bus umožňuje tento identifikátor odstranit duplicitní zpráv, které obdrží. Příklad používá vlastnost **Typ zprávy** rozlišit interaktivní z datového bodu zprávy. Aplikace předá tyto informace v dialogovém okně Vlastnosti zprávy, ne v těle zprávy tak, aby procesor událostí nemusí rekonstrukci zprávy provádět směrování zpráv.

    > [AZURE.NOTE] Je důležité pro vytvoření **MessageId** lze odstranit duplicitní interaktivní zprávy v kódu zařízení. Komunikace chybovému sítě nebo jiné chyby, může vést k více oznámeními stejné zprávy od toto zařízení. Můžete taky použít ID sémantického zprávy, například hash příslušné zprávy datová pole, místo UUID.

3. Upravte metodu **hlavní** odesílat obou interaktivní zprávy a data bodů zprávy uvedeno v následující fragment kódu:

    ````
    MessageSender sender = new MessageSender();
    InteractiveMessageSender interactiveSender = new InteractiveMessageSender();

    ExecutorService executor = Executors.newFixedThreadPool(2);
    executor.execute(sender);
    executor.execute(interactiveSender);
    ````

4. Uložte a zavřete soubor simulated-device\src\main\java\com\mycompany\app\App.java.

    > [AZURE.NOTE] Za účelem zjednodušení tento kurz není implementovat zásady libovolný opakovat. Ve výrobním kódu měli byste implementovat zásadu opakovat například exponenciální zdvojnásobení navržené v článku MSDN [Přechodná zpracování poruch].

5. Vytvoření aplikace **simulovaného zařízení** pomocí Maven, spusťte následující příkaz příkazového řádku ve složce simulovaného zařízení:

    ```
    mvn clean package -DskipTests
    ```

## <a name="process-device-to-cloud-messages"></a>Zpracování zpráv zařízení do cloudu

V této části Vytvoření aplikace konzoly Java zpracovávající zařízení do cloudu zprávy od IoT centrální. Rozbočovač IOT zpřístupňuje [události rozbočovači]-kompatibilní koncový bod povolení aplikace čtení zpráv zařízení do cloudu. Tento kurz používá třídu [EventProcessorHost] zpracuje tyto zprávy v aplikaci konzoly. Další informace o tom, jak zpracování zpráv z rozbočovače události najdete v tématu kurz [Seznámení s rozbočovače události] .

V hlavním ověřovací kód programu při implementace spolehlivé úložiště zpráv datový bod nebo přesměrování interaktivní zpráv je, že závisí na příjemce zprávy stanovit kontroly průběhu zpracování události. Kromě toho dosáhnout vysoký výkon při čtení z události rozbočovače by měl obsahovat kontroly ve velkých listů. Tento přístup vytvoří možnost duplicitní zpracování velké množství zpráv, pokud dojde k selhání a návrat do předchozí kontroly. V tomto kurzu najdete v článku synchronizace zápisy Azure úložiště a služby Bus rušení duplikace windows se **EventProcessorHost** kontroly.

K psaní zpráv k základnímu úložišti Azure problémy se spolehlivým, vzorku používá tuto funkci potvrdit jednotlivé blok s [objekty BLOB blok][Azure Block Blobs]. Procesor událostí sečteny zprávy v paměti, dokud je potřeba zadat kontrolní bod. Například po Akumulované vyrovnávací paměť zpráv dosáhne maximální velikosti 4 MB, nebo odinstalaci duplikace Bus služby uplyne časového intervalu. Potom před kontrolu, kód potvrdí blok nový objekt blob.

Procesor událostí používá události rozbočovače odsadí zprávu jako blokovat ID. Tento postup umožňuje procesoru události kontrolovat rušení duplikace před potvrdí nový blok k základnímu úložišti, starat možné zhroucení mezi potvrzování bloku a kontrolu.

> [AZURE.NOTE] Tento kurz používá jeden účet Azure úložiště k zápisu všechny zprávy načtené z centrální IoT. Zjištění potřebnosti používat více účtů Azure úložiště ve vašem řešení, najdete v článku [úložišti Azure škálovatelnost pokyny].

Chcete-li předejít duplicitní položky při zpracování interaktivní zprávy používá aplikace služby Bus rušení duplikování. Simulovaný zařízení razítkem každé interaktivní zprávy s jedinečnými **MessageId**. Toto id služeb Bus zajistit, že v okně čas zadaný rušení duplikace žádné dva s stejné **MessageId** doručování zpráv příjemce. Tento rušení duplikace společně s sémantiku dokončení jednotlivých zpráv poskytovaných služeb Bus fronty snadno implementovat spolehlivé zpracování interaktivní zpráv.

Abyste měli jistotu, že se žádná zpráva znovu mimo oknem rušení duplikace, kód synchronizuje mechanismus kontrolní bod **EventProcessorHost** s oknem rušení duplikace fronty Bus služby. Tato synchronizace vystavila společnost vynucení kontrolní bod aspoň jednou pokaždé, když má oknem rušení duplikace (v tomto kurzu okno je jedna hodina).

> [AZURE.NOTE] Tento kurz používá jediné rozdělený služby Bus fronty procesu interaktivní zprávy načtená z kontingenčního seznamu IoT centrální. Další informace o použití služby Bus fronty požadavkům škálovatelnost řešení najdete v dokumentaci [Bus služby Azure] .

### <a name="provision-an-azure-storage-account-and-a-service-bus-queue"></a>Zřízení účtu Azure úložiště a služby Bus fronty

Použití třídy [EventProcessorHost] , musíte mít účet Azure úložiště povolit **EventProcessorHost** pořizovat záznam kontrolní bod informace. Můžete použít existující účet Azure úložiště nebo postupujte podle pokynů v [O úložišti Azure] a vytvořte nový účet. Poznamenejte si úložišti Azure účtu připojovací řetězec.

> [AZURE.NOTE] Když zkopírujete a vložíte připojovací řetězec účet Azure úložiště, ujistěte se, jsou zahrnuty bez mezer.

Budete potřebovat služby Bus fronty umožníte spolehlivé zpracování interaktivní zpráv. Můžete vytvořit fronty programově s oknem rušení duplikace hodinu, způsobem popsaným v tématu [jak používat službu Bus fronty][fronty Bus služby]. Můžete taky můžete použít [Azure klasické portál][lnk-classic-portal], pomocí následujících kroků:

1. V levém dolním rohu klikněte na **Nový** . Klikněte na položku **Aplikace služby** > **Služby Bus** > **fronty** > **Vytvořit vlastní**. Zadejte název **d2ctutorial**, vyberte oblast a použijte existující obor názvů nebo vytvořte novou. Poznamenejte si název oboru názvů, musíte dál v tomto kurzu. Na další stránce zaškrtněte políčko **Povolit duplicit**a nastavte **duplicitní zjišťování historie časového intervalu** na 1 hodinu. Klepněte na značku zaškrtnutí v pravém dolním rohu uložení fronty konfigurace.

    ![Vytvoření fronty Azure portálu][30]

2. V seznamu fronty Bus služby klikněte na **d2ctutorial**a potom klikněte na **Konfigurovat**. Vytvoření dvě zásady sdílený přístup jeden jen **Odeslat** s oprávnění **Odeslat** a jeden jen **Poslech** s oprávněními **Poslech** . Poznamenejte si hodnoty **primární klíč** pro obě zásady, budete potřebovat dál v tomto kurzu. Až budete hotovi, klepněte na tlačítko **Uložit** dole.

    ![Konfigurace fronty Azure portálu][31]

### <a name="create-the-event-processor"></a>Vytvoření procesor událostí

V této části vytvoříte aplikace Java zpracování zpráv z koncového bodu kompatibilní s prohlížečem události centrální.

První úkol je přidat projekt Maven s názvem **proces d2c zprávy** přijímá zařízení do cloudu zprávy od koncového bodu kompatibilní s prohlížečem IoT centrální události centrální, který přesměrovává tyto zprávy na jiných služeb back-end.

1. V iot java-get spouštěné složku, kterou jste vytvořili v kurzu [Začínáme s IoT centrální] vytvořte Maven projekt s názvem **zpracování d2c zpráv** pomocí následujícího příkazu příkazového řádku. Poznámka: Jedná se o dlouhého příkaz:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=process-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Příkazového řádku přejděte do nové složky, zpracování d2c zpráv.

3. V textovém editoru, otevřete soubor pom.xml ve složce zpracování d2c zpráv a přidejte následující závislosti uzel **závislosti** . Tyto závislosti umožňují slouží k interakci s IoT centrální a fronty Bus služby azure eventhubs, azure eventhubs eph a balíčků azure servicebus v aplikaci:

    ```
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-eventhubs</artifactId>
      <version>0.8.0</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-eventhubs-eph</artifactId>
      <version>0.8.0</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-servicebus</artifactId>
      <version>0.9.4</version>
    </dependency>
    ```

4. Uložte a zavřete soubor pom.xml.

Dalším úkolem je přidejte třídu **ErrorNotificationHandler** do projektu.

1. Umožňuje vytvořit soubor process-d2c-messages\src\main\java\com\mycompany\app\ErrorNotificationHandler.java textovém editoru. Přidejte následující kód soubor zobrazíte chybové zprávy z **EventProcesssorHost** instance:

    ```
    package com.mycompany.app;

    import java.util.function.Consumer;
    import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;

    public class ErrorNotificationHandler implements
        Consumer<ExceptionReceivedEventArgs> {
      @Override
      public void accept(ExceptionReceivedEventArgs t) {
        System.out.println("EventProcessorHost: Host " + t.getHostname()
            + " received general error notification during " + t.getAction() + ": "
            + t.getException().toString());
      }
    }
    ```

2. Uložte a zavřete soubor ErrorNotificationHandler.java.

Teď můžete přidat třídu implementující rozhraní **IEventProcessor** . Třídy **EventProcessorHost** hovorů této třídy zpracuje zařízení do cloudu zprávy přijaté od IoT centrální. Kód v této třídy používá logických ukládání zpráv problémy se spolehlivým v kontejneru objektů blob a předávání interaktivní zpráv do fronty Bus služby.

Metoda **onEvents** nastaví proměnnou **latestEventData** , která sleduje posun a pořadí počet nejnovější zpráva jen pro čtení procesoru události. Myslete na to, že procesorech je zodpovědný za jeden oddíl. Metoda **onEvents** pak dostává sadu zpráv z centrální IoT a zpracuje následujícím způsobem: přesměruje interaktivní zprávy do fronty Bus služby a připojí datový bod zprávy vyrovnávací paměť **toAppend** . Pokud vyrovnávací paměť dosáhne omezit blok 4 MB nebo odinstalaci duplikace čas windows uplyne (hodinu po poslední kontroly v tomto kurzu), metodu spustí kontrolní bod.

Metoda **AppendAndCheckPoint** nejdřív vygeneruje **blockId** bloku chcete připojit k objektů blob. Azure úložiště vyžaduje že všechny blokovat ID mít stejnou délku tak metodu ohraničí posun s úvodními nulami. Potom pokud blok s tímto ID je už objektů blob, metodu přepíše ho aktuální obsah rezervy.

> [AZURE.NOTE] Zjednodušit kód, používá tento kurz jednoho objektů blob na oddíl pro uložení zpráv. Skutečné řešení implementovat postupných vytvořením další soubory za určitou dobu nebo když dostanete určitou velikost souboru. Myslete na to, že objektů blob Azure bloku může obsahovat maximálně 195 GB data.

Je další úkol implementovat rozhraní **IEventProcessor** :

1. Umožňuje vytvořit soubor process-d2c-messages\src\main\java\com\mycompany\app\EventProcessor.java textovém editoru.

2. Přidejte následující importy a definice třídy EventProcessor.java soubor. Třídy **EventProcessor** implementuje **IEventProcessor** rozhraní, který definuje chování klientů rozbočovače události:

    ```
    package com.mycompany.app;

    import java.io.ByteArrayInputStream;
    import java.io.ByteArrayOutputStream;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.nio.charset.StandardCharsets;
    import java.time.Duration;
    import java.time.Instant;
    import java.util.ArrayList;
    import java.util.Base64;
    import java.util.concurrent.ExecutionException;

    import com.microsoft.azure.eventhubs.EventData;
    import com.microsoft.azure.eventprocessorhost.*;
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;
    import com.microsoft.windowsazure.services.servicebus.*;
    import com.microsoft.windowsazure.services.servicebus.models.BrokeredMessage;

    public class EventProcessor implements IEventProcessor {

    }
    ```

3. Přidejte do **EventProcessor** třídy implementovat rozhraní **IEventProcessor** následujících metod:

    ```
    @Override
    public void onOpen(PartitionContext context) throws Exception {
      System.out.println("EventProcessorHost: Partition "
          + context.getPartitionId() + " is opening");
    }

    @Override
    public void onClose(PartitionContext context, CloseReason reason)
        throws Exception {
      System.out.println("EventProcessorHost: Partition "
          + context.getPartitionId() + " is closing for reason "
          + reason.toString());
    }

    @Override
    public void onError(PartitionContext context, Throwable error) {
      System.out.println("EventProcessorHost: Partition "
          + context.getPartitionId() + " onError: " + error.toString());
    }

    @Override
    public void onEvents(PartitionContext context, Iterable<EventData> messages)
        throws Exception {
    }
    ```

4. Přidejte následující proměnné úrovni třídy třídy **EventProcessor** :

    ```
    public static CloudBlobContainer blobContainer;
    public static ServiceBusContract serviceBusContract;

    // Use a smaller MAX_BLOCK_SIZE value to test.
    final private int MAX_BLOCK_SIZE = 4 * 1024 * 1024;
    final private Duration MAX_CHECKPOINT_TIME = Duration.ofHours(1);

    private ByteArrayOutputStream toAppend = new ByteArrayOutputStream(
        MAX_BLOCK_SIZE);
    private Instant start = Instant.now();
    private EventData latestEventData;
    ```

5. Přidání metody **AppendAndCheckPoint** následující podpisem třídy **EventProcessor** :

    ```
    private void AppendAndCheckPoint(PartitionContext context)
      throws URISyntaxException, StorageException, IOException,
      IllegalArgumentException, InterruptedException, ExecutionException {
    }
    ```

6. Přidejte následující kód metody **AppendAndCheckPoint** pro získání aktuální zprávy posun a pořadí čísla v oddílu:

    ```
    String currentOffset = latestEventData.getSystemProperties().getOffset();
    Long currentSequence = latestEventData.getSystemProperties().getSequenceNumber();
    System.out
        .printf(
            "\nAppendAndCheckPoint using partition: %s, offset: %s, sequence: %s\n",
            context.getPartitionId(), currentOffset, currentSequence);
    ```

7. V metodě **AppendAndCheckPoint** použijte aktuální posun hodnotu pro vytvoření instance **BlockEntry** další bloku uložit do objektů blob:

    ```
    Long blockId = Long.parseLong(currentOffset);
    String blockIdString = String.format("startSeq:%1$025d", blockId);
    String encodedBlockId = Base64.getEncoder().encodeToString(
        blockIdString.getBytes(StandardCharsets.US_ASCII));
    BlockEntry block = new BlockEntry(encodedBlockId);
    ```

8. V metodě **AppendAndCheckPoint** nahrajte nejnovější sadu zpráv objektů blob bloku a načtení aktuální seznam bloky:

    ```
    String blobName = String.format("iothubd2c_%s", context.getPartitionId());
    CloudBlockBlob currentBlob = blobContainer.getBlockBlobReference(blobName);

    currentBlob.uploadBlock(block.getId(),
        new ByteArrayInputStream(toAppend.toByteArray()), toAppend.size());
    ArrayList<BlockEntry> blockList = currentBlob.downloadBlockList();
    ```

9. V metodu **AppendAndCheckPoint** vytvoření počáteční bloku do nového objektů blob nebo připojení k existující objektů blob bloku:

    ```
    if (currentBlob.exists()) {
      // Check if we should append new block or overwrite existing block
      BlockEntry last = blockList.get(blockList.size() - 1);
      if (blockList.size() > 0 && !last.getId().equals(block.getId())) {
        System.out.printf("Appending block %s to blob %s\n", blockId, blobName);
        blockList.add(block);
      } else {
        System.out.printf("Overwriting block %s in blob %s\n", blockId,
            blobName);
      }
    } else {
      System.out.printf("Creating initial block %s in new blob: %s\n", blockId,
          blobName);
      blockList.add(block);
    }
    currentBlob.commitBlockList(blockList);
    ```

10. Nakonec v metodu **AppendAndCheckPoint** na oddílu vytvořit kontrolní bod a připravit uložit další blokování zpráv:

    ```
    context.checkpoint(latestEventData);

    // Reset everything after the checkpoint.
    toAppend.reset();
    start = Instant.now();
    System.out.printf("Checkpointed on partition id: %s\n",
        context.getPartitionId());
    ```

11. V metodě **onEvents** přidáte následující kód přijímat zprávy z centrální IoT koncového bodu a předávání interaktivní zpráv do fronty Bus služby. Pokud je už plná blokování nebo vyprší časový limit zavolejte metodu **AppendAndCheckPoint** :

    ```
    if (messages != null) {
      for (EventData eventData : messages) {
        latestEventData = eventData;
        byte[] data = eventData.getBody();
        if (eventData.getProperties().containsKey("messageType")
            && eventData.getProperties().get("messageType")
                .equals("interactive")) {
          String messageId = (String) eventData.getSystemProperties().get(
              "message-id");
          BrokeredMessage message = new BrokeredMessage(data);
          message.setMessageId(messageId);
          serviceBusContract.sendQueueMessage("d2ctutorial", message);
          continue;
        }
        if (toAppend.size() + data.length > MAX_BLOCK_SIZE
            || Duration.between(start, Instant.now()).compareTo(
                MAX_CHECKPOINT_TIME) > 0) {
          AppendAndCheckPoint(context);
        }
        toAppend.write(data);
      }
    }
    ```

12. Nakonec v metodu **onEvents** přidejte klauzuli 'else if' volání **AppendAndCheckPoint** Pokud vyprší časový limit, zatímco žádné zprávy pocházejících z centrální IoT:

    ```
    else if ((toAppend.size() > 0)
        && Duration.between(start, Instant.now())
            .compareTo(MAX_CHECKPOINT_TIME) > 0) {
      AppendAndCheckPoint(context);
    }
    ```

13. Uložte změny do souboru EventProcessor.java.

Poslední úkol v projektu **zpracování d2c zpráv** je do **hlavního** metodu, která vytvoří instanci **EventProcessorHost** přidat kód.

1. Umožňuje otevřít soubor process-d2c-messages\src\main\java\com\mycompany\app\App.java textovém editoru.

2. Přidejte následující příkazy **importovat** soubor:

    ```
    import com.microsoft.azure.eventprocessorhost.*;
    import com.microsoft.azure.servicebus.ConnectionStringBuilder;
    import com.microsoft.azure.storage.CloudStorageAccount;
    import com.microsoft.azure.storage.StorageException;
    import com.microsoft.azure.storage.blob.CloudBlobClient;
    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.services.servicebus.ServiceBusConfiguration;
    import com.microsoft.windowsazure.services.servicebus.ServiceBusService;

    import java.net.URISyntaxException;
    import java.security.InvalidKeyException;
    import java.util.concurrent.*;
    ```

3. Přidejte následující proměnnou úrovni třídy třídy **aplikace** . **{Yourstorageaccountconnectionstring}** nahraďte úložišti Azure připojovací řetězec účtu vytvořený poznamenat dříve v části [vytvořit účet Azure úložiště a služby Bus fronty](#provision-an-azure-storage-account-and-a-service-bus-queue) :

    ```
    private final static String storageConnectionString = "{yourstorageaccountconnectionstring}";
    ```

4. Přidejte následující proměnné úrovni třídy třídy **aplikace** a nahraďte **{yourservicebusnamespace}** s služby Bus názvů a **{yourservicebussendkey}** vaší frontě **Poslat** klíč. V části [vytvořit účet Azure úložiště a služby Bus fronty](#provision-an-azure-storage-account-and-a-service-bus-queue) dříve udělali poznámky obor názvů a **poslouchat** klíčových hodnot:

    ```
    private final static String serviceBusNamespace = "{yourservicebusnamespace}";
    private final static String serviceBusSasKeyName = "send";
    private final static String serviceBusSASKey = "{yourservicebussendkey}";
    private final static String serviceBusRootUri = ".servicebus.windows.net";
    ```

5. Přidejte následující proměnné úrovni třídy třídy **aplikace** . **{Youreventhubcompatibleendpoint}** nahraďte hodnotou kompatibilní s prohlížečem události centrální koncového bodu. Hodnota koncového bodu vypadá **jeho... obor názvů** tak, aby měli odebrat **sb: / /** předponu nebo příponu **.servicebus.windows.net/** . **{Youreventhubcompatiblename}** nahraďte názvem kompatibilní s prohlížečem události centrální. Nahraďte **{youriothubkey}** klávesu **iothubowner** . Jste provedli poznámky z těchto hodnot v tématu [Vytvoření rozbočovači IoT] [ lnk-create-an-iot-hub] v tématu kurz *Seznámení s Azure IoT centrální jazyka Java* :

    ```
    private final static String consumerGroupName = "$Default";
    private final static String namespaceName = "{youreventhubcompatibleendpoint}";
    private final static String eventHubName = "{youreventhubcompatiblename}";
    private final static String sasKeyName = "iothubowner";
    private final static String sasKey = "{youriothubkey}";
    ```

6. Upravte podpis **hlavní** metoda následujícím způsobem:

    ```
    public static void main(String args[]) throws InvalidKeyException,
      URISyntaxException, StorageException {
    }
    ```

7. Přidejte následující kód metody **hlavní** získat odkaz na kontejner objektů blob, který ukládá zprávy:

    ```
    System.out.println("Process D2C messages using EventProcessorHost");
    CloudStorageAccount account = CloudStorageAccount
        .parse(storageConnectionString);
    CloudBlobClient client = account.createCloudBlobClient();
    EventProcessor.blobContainer = client
        .getContainerReference("d2cjavatutorial");
    EventProcessor.blobContainer.createIfNotExists();
    ```

8. Přidejte následující kód **hlavní** metody můžete získat odkaz ke službě Bus služby:

    ```
    Configuration config = ServiceBusConfiguration
        .configureWithSASAuthentication(serviceBusNamespace,
            serviceBusSasKeyName, serviceBusSASKey, serviceBusRootUri);
    EventProcessor.serviceBusContract = ServiceBusService.create(config);
    ```

9. V **hlavním** metody konfigurace a vytvořte instanci **EventProcessorHost** . Možnost **setInvokeProcessorAfterReceiveTimeout** zaručuje, že **EventProcessorHost** instance vyvolá metodu **onEvents** v rozhraní **IEventProcessor** i v případě, že nejsou žádné zprávy zpracuje. Metoda **onEvents** potom vždy vyvolá **AppendAndCheckPoint** metodu když vyprší časový limit.

    ```
    ConnectionStringBuilder eventHubConnectionString = new ConnectionStringBuilder(
        namespaceName, eventHubName, sasKeyName, sasKey);
    EventProcessorHost host = new EventProcessorHost(eventHubName,
        consumerGroupName, eventHubConnectionString.toString(),
        storageConnectionString);
    EventProcessorOptions options = new EventProcessorOptions();
    options.setExceptionNotification(new ErrorNotificationHandler());
    options.setInvokeProcessorAfterReceiveTimeout(true);
    ```

10. V **hlavním** metoda zaregistrujte implementaci **IEventProcessor** pomocí **EventProcessorHost** instance:

    ```
    try {
      System.out.println("Registering host named " + host.getHostName());
      host.registerEventProcessor(EventProcessor.class, options).get();
    } catch (Exception e) {
      System.out.print("Failure while registering: ");
      if (e instanceof ExecutionException) {
        Throwable inner = e.getCause();
        System.out.println(inner.toString());
      } else {
        System.out.println(e.toString());
      }
      System.out.println(e.toString());
    }
    ```

11. Nakonec přidání logiky do **hlavní** metoda vypnout **EventProcessorHost** instance:

    ```
    System.out.println("Press enter to stop");
    try {
      System.in.read();
      host.unregisterEventProcessor();

      System.out.println("Calling forceExecutorShutdown");
      EventProcessorHost.forceExecutorShutdown(120);
    } catch (Exception e) {
      System.out.println(e.toString());
      e.printStackTrace();
    }

    System.out.println("End of sample");
    ```

12. Uložte a zavřete soubor process-d2c-messages\src\main\java\com\mycompany\app\App.java.

13. Vytvoření aplikace **zpracování d2c zpráv** pomocí Maven, spusťte následující příkaz příkazového řádku ve složce zpracování d2c zpráv:

    ```
    mvn clean package -DskipTests
    ```

## <a name="receive-interactive-messages"></a>Interaktivní zprávy

V této části napíšete aplikace konzoly Java, která přijímá interaktivní zprávy ve frontě Bus služby.

První úkol je přidat projekt Maven s názvem **proces interaktivní zprávy** , které přijímá zprávám odesílaným z instance **EventProcessor** ve frontě Bus služby.

1. V iot java-get spouštěné složku, kterou jste vytvořili v kurzu [Začínáme s IoT centrální] vytvořte Maven projekt s názvem **zpracování interaktivní zpráv** pomocí následujícího příkazu příkazového řádku. Poznámka: Jedná se o dlouhého příkaz:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=process-interactive-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Příkazového řádku přejděte do nové složky, zpracování interaktivní zpráv.

3. V textovém editoru, otevřete soubor pom.xml ve složce zpracování interaktivní zpráv a přidejte následující závislost na uzel **závislosti** . Tato závislost umožňuje použil funkci balíček azure servicebus v aplikaci Pokud chcete provést interakci s fronty Bus služby:

    ```
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-servicebus</artifactId>
      <version>0.9.4</version>
    </dependency>
    ```

4. Uložte a zavřete soubor pom.xml.

Dalším úkolem je přidání kódu pro vzkazy z fronty Bus služby.

1. Umožňuje otevřít soubor process-interactive-messages\src\main\java\com\mycompany\app\App.java textovém editoru.

2. Přidejte následující `import` příkazy k souboru:

    ```
    import java.io.IOException;
    import java.util.concurrent.ExecutorService;
    import java.util.concurrent.Executors;

    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.exception.ServiceException;
    import com.microsoft.windowsazure.services.servicebus.*;
    import com.microsoft.windowsazure.services.servicebus.models.*;
    ```

3. Přidejte následující proměnné úrovni třídy třídy **aplikace** a nahraďte **{yourservicebusnamespace}** služby Bus názvů a **{yourservicebuslistenkey}** s vaší frontě **poslechněte** klíčem. V části [vytvořit účet Azure úložiště a služby Bus fronty](#provision-an-azure-storage-account-and-a-service-bus-queue) dříve udělali poznámky obor názvů a **poslouchat** klíčových hodnot:

    ```
    private final static String serviceBusNamespace = "{yourservicebusnamespace}";
    private final static String serviceBusSasKeyName = "listen";
    private final static String serviceBusSASKey = "{yourservicebuslistenkey}";
    private final static String serviceBusRootUri = ".servicebus.windows.net";
    private final static String queueName = "d2ctutorial";
    private static ServiceBusContract service = null;
    ```

4. Přidejte následující vnořené třídy třídy **aplikace** přijímat zprávy ve frontě:

    ```
    private static class MessageReceiver implements Runnable {
      public void run() {
        ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
        try {
          while (true) {
            ReceiveQueueMessageResult resultQM = service.receiveQueueMessage(
                queueName, opts);
            BrokeredMessage message = resultQM.getValue();
            if (message != null && message.getMessageId() != null) {
              System.out.println("MessageID: " + message.getMessageId());
              System.out.print("From queue: ");
              byte[] b = new byte[200];
              String s = null;
              int numRead = message.getBody().read(b);
              while (-1 != numRead) {
                s = new String(b);
                s = s.trim();
                System.out.print(s);
                numRead = message.getBody().read(b);
              }
              System.out.println();
            } else {
              Thread.sleep(1000);
            }
          }
        } catch (InterruptedException e) {
          System.out.println("Finished.");
        } catch (ServiceException e) {
          System.out.println("ServiceException: " + e.getMessage());
        } catch (IOException e) {
          System.out.println("IOException: " + e.getMessage());
        }
      }
    }
    ```

5. Upravte podpis **hlavní** metoda následujícím způsobem:

    ```
    public static void main(String args[]) throws ServiceException, IOException {
    }
    ```

6. V **hlavním** metody přidáte následující kód se spustit poslech nových zpráv:

    ```
    System.out.println("Process interactive messages");

    Configuration config = ServiceBusConfiguration
        .configureWithSASAuthentication(serviceBusNamespace,
            serviceBusSasKeyName, serviceBusSASKey, serviceBusRootUri);
    service = ServiceBusService.create(config);

    MessageReceiver receiver = new MessageReceiver();

    ExecutorService executor = Executors.newFixedThreadPool(2);
    executor.execute(receiver);

    System.out.println("Press ENTER to exit.");
    System.in.read();
    executor.shutdownNow();
    ```

7. Uložte a zavřete soubor process-interactive-messages\src\main\java\com\mycompany\app\App.java.

8. Vytvoření aplikace **zpracování interaktivní zpráv** pomocí Maven, spusťte následující příkaz příkazového řádku ve složce zpracování interaktivní zpráv:

    ```
    mvn clean package -DskipTests
    ```

## <a name="run-the-applications"></a>Spuštění aplikace

Teď jste připraveni spustit tři aplikace.

1.  Spusťte aplikaci **proces interaktivní zprávy** , v příkazovém řádku nebo prostředí přejděte do složky zpracování interaktivní zpráv a spusťte následující příkaz:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Spustit proces interaktivní zprávy][processinteractive]

2.  Spusťte aplikaci **proces d2c zprávy** , v příkazovém řádku nebo prostředí přejděte do složky zpracování d2c zpráv a spusťte následující příkaz:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Spustit proces d2c zprávy][processd2c]

3.  Pokud chcete spustit aplikaci **simulovaného zařízení** , v příkazový řádek nebo prostředí přejděte do složky simulovaného zařízení a spusťte následující příkaz:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Spuštění simulovaného zařízení][simulateddevice]

> [AZURE.NOTE] Pokud chcete zobrazit aktualizace vaší objektů blob, budete muset zmenšit konstanta **MAX_BLOCK_SIZE** ve třídě **StoreEventProcessor** menší hodnotu, například **1 024**. Tato změna je užitečná, protože trvá určitou dobu kontaktovat limit velikosti blok s daty odeslaný simulovaný zařízení. Menší velikosti blok nepotřebujete až za dlouhou zobrazíte objektů blob vytvořit a aktualizovat. Ale s větší velikostí blok způsobí, že aplikace větší škálovatelnost.

## <a name="next-steps"></a>Další kroky

V tomto kurzu se naučíte problémy se spolehlivým procesu datového bodu a interaktivní zprávy zařízení do cloudu pomocí [EventProcessorHost] předmětu.

[Odeslání zprávy cloudu zařízení s IoT centrální] [ lnk-c2d] se dozvíte, jak posílat zprávy do svého zařízení ze svého back-end.

Příklady úplné řešení začátku do konce, které používají IoT centrální najdete v tématu [Azure IoT sadu][lnk-suite].

Další informace o vytváření řešení s IoT centrální, najdete v článku [IoT centrální vývojář Průvodce].

<!-- Images. -->
[simulateddevice]: ./media/iot-hub-java-java-process-d2c/runsimulateddevice.png
[processinteractive]: ./media/iot-hub-java-java-process-d2c/runprocessinteractive.png
[processd2c]: ./media/iot-hub-java-java-process-d2c/runprocessd2c.png

[30]: ./media/iot-hub-java-java-process-d2c/createqueue2.png
[31]: ./media/iot-hub-java-java-process-d2c/createqueue3.png

<!-- Links -->

[Úložiště objektů blob Azure]: ../storage/storage-dotnet-how-to-use-blobs.md
[Azure Data Factory]: https://azure.microsoft.com/documentation/services/data-factory/
[HDInsight (Hadoop)]: https://azure.microsoft.com/documentation/services/hdinsight/
[Služba Bus fronty]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md

[Azure IoT centrální vývojář Průvodce - zařízení do cloudu]: iot-hub-devguide-messaging.md

[Azure úložiště]: https://azure.microsoft.com/documentation/services/storage/
[Bus služby Azure]: https://azure.microsoft.com/documentation/services/service-bus/

[Příručka IoT centrální vývojář]: iot-hub-devguide.md
[Začínáme s IoT centrální]: iot-hub-java-java-getstarted.md
[Středisko pro vývojáře IoT Azure]: https://azure.microsoft.com/develop/iot
[lnk-service-fabric]: https://azure.microsoft.com/documentation/services/service-fabric/
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-hubs]: https://azure.microsoft.com/documentation/services/event-hubs/
[Zpracování přechodná poruch]: https://msdn.microsoft.com/library/hh675232.aspx

<!-- Links -->
[Informace o Azure úložiště]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Začínáme s rozbočovače události]: ../event-hubs/event-hubs-java-ephjava-getstarted.md
[Azure úložiště škálovatelnost pravidla]: ../storage/storage-scalability-targets.md
[Azure Block Blobs]: https://msdn.microsoft.com/library/azure/ee691964.aspx
[Event Hubs]: ../event-hubs/event-hubs-overview.md
[EventProcessorHost]: https://github.com/Azure/azure-event-hubs/tree/master/java/azure-eventhubs-eph
[Zpracování přechodná poruch]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-c2d]: iot-hub-java-java-process-d2c.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-devbox-setup.md
[lnk-create-an-iot-hub]: iot-hub-java-java-getstarted.md#create-an-iot-hub