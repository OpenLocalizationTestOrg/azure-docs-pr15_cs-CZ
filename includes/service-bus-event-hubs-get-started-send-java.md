## <a name="send-messages-to-event-hubs"></a>Odeslání zprávy rozbočovače události

Knihovnu Java klienta pro událost rozbočovače je k dispozici pro použití v Maven projekty v [Centrálním úložišti Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22)a můžete odkázat pomocí následující deklarace závislost do souboru projektu Maven:    

``` XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
```
 
Pro různé typy buildu prostředí explicitně získáte nejnovější vydanou SKLENICE soubory v [Centrálním úložišti Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22) nebo od [vydání rozdělení chvíle GitHub](https://github.com/Azure/azure-event-hubs/releases).  

Jednoduchý události Publisheru importujte balíčky *com.microsoft.azure.eventhubs* pro klienta třídy rozbočovače událostí a *com.microsoft.azure.servicebus* pro nástroj třídy například běžné výjimek, které jsou sdílené s klientem zpráv Bus služby Azure. 

V následujícím příkladu nejprve vytvořte nový projekt Maven konzoly/prostředí aplikace ve vašem Oblíbené vývojové prostředí Java. Volaná předmětu ```Send```.     

``` Java

import java.io.IOException;
import java.nio.charset.*;
import java.util.*;
import java.util.concurrent.ExecutionException;

import com.microsoft.azure.eventhubs.*;
import com.microsoft.azure.servicebus.*;

public class Send
{
    public static void main(String[] args) 
            throws ServiceBusException, ExecutionException, InterruptedException, IOException
    {
```

Obor názvů a události centrální názvy nahraďte hodnoty při vytvoření centra události.

``` Java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";
    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";
    ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
```

Vytvořte událost nastavit jako jednotném tak, že vypnete řetězec do jeho kódování bajt UTF-8. Potom vytvořte novou instanci klienta rozbočovače událost z připojovací řetězec jsme odešlete zprávu.   

``` Java 
                
    byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
    EventData sendEvent = new EventData(payloadBytes);
    
    EventHubClient ehClient = EventHubClient.createFromConnectionStringSync(connStr.toString());
    ehClient.sendSync(sendEvent);
    }
}

``` 
