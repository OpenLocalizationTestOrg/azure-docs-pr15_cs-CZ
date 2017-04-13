<properties 
    pageTitle="Služba Bus brokered zpráv kurz .NET | Microsoft Azure"
    description="Brokered zpráv .NET kurz."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/27/2016"
    ms.author="sethm" />

# <a name="service-bus-brokered-messaging-net-tutorial"></a>Kurz .NET pro zasílání zpráv služby Bus brokered

Azure Bus služba poskytuje dva komplexní zpráv řešení – jeden prostřednictvím centralizované "relay" spuštěné v cloudu, který podporuje spoustu různých transportní protokoly a webové služby organizace pro standardy, včetně SOAP, WS-* a PODRŽTE. Klient nemusí přímé připojení ke službě místní ani ho musím vědět, kde je služba umístěna a služby místní nemusí všechny příchozí porty otevřít v bráně firewall.

Druhá zpráv řešení umožňuje "brokered" funkce zasílání zpráv. Tyto lze představit jako asynchronní nebo oddělené zpráv funkcí, které podpory publikovat přihlášením časový oddělení a scénáře pomocí zasílání infrastruktury služby Bus Vyrovnávání zatížení. Oddělené komunikace obsahuje mnoho výhod; například klienty a servery můžete připojit v případě potřeby a provádění svých operací asynchronní způsobem.

Tento kurz se má vám dává přehled a praktických zkušeností s fronty, jednu z základní součásti webové části služby Bus brokered zasílání zpráv. Po dokončení práce řadou témata v tomto kurzu, máte aplikaci, která naplní seznam zpráv, vytvoří fronty a odešle zprávy do této fronty. Nakonec aplikace obdrží a slouží k zobrazení zprávy z fronty a pak vyčistí ukončí a materiály. Odpovídající kurzu, který popisuje, jak vytvářet aplikace, která používá Service Bus Relay najdete v článku [služby Bus přenos zpráv kurz](../service-bus-relay/service-bus-relay-tutorial.md).

## <a name="introduction-and-prerequisites"></a>Úvod a požadavky

Fronty nabízejí první v první mimo FIFO doručení zprávy pro jednoho nebo více konkurenční spotřebitele. FIFO znamená, že zprávy jsou obvykle podle očekávání měly být přijetí a zpracování tak, že příjemce v časový pořadí, ve kterém byly byla zařazena do fronty a každá zpráva dostali a budou zpracovány službou spotř pouze jedné zprávy. Klíčové výhodou používání fronty je dosáhnout *časový oddělení* součástí aplikace: jinými slovy, výrobci a které se zobrazují koncovým nemusí být odesílání a přijímání zpráv ve stejnou dobu, protože zprávy jsou uloženy trvale ve frontě. Související dávku je *načtení vyrovnání*, což umožňuje výrobce a spotřebitele k odesílání a přijímání zpráv podle různých sazeb.

Následují některé pro správu a základní kroky, které byste měli postupovat před zahájením kurzu. První je vytvoření obor názvů služby a získat sdílené přístupová klávesa podpisu (přidružení zabezpečení). Obor názvů obsahuje hranice aplikace pro každou aplikaci vystaven prostřednictvím služby Bus. Přidružení zabezpečení klíč vytváří automaticky systém při vytváření názvů služby. Kombinace obor názvů služby a přidružení zabezpečení klíč poskytuje pověření, se kterým služby Bus ověří přístup k aplikaci.

### <a name="create-a-service-namespace-and-obtain-a-sas-key"></a>Vytvoření obor názvů služby a získat klíč přidružení zabezpečení

Cílem prvního kroku je vytvořit obor názvů služby a získat klíč [Sdílené podpis aplikace Access](service-bus-sas-overview.md) (přidružení zabezpečení). Obor názvů obsahuje hranice aplikace pro každou aplikaci vystaven prostřednictvím služby Bus. Přidružení zabezpečení klíč vytváří automaticky systém při vytváření názvů služby. Kombinace obor názvů služby a přidružení zabezpečení klíč poskytuje přihlašovacích údajů pro službu Bus ověření přístup k aplikaci.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Dalším krokem je vytvoření projekt Visual Studia a dvě pomocné funkce, které načtení seznam oddělený středníkem zpráv do .NET [seznamu](https://msdn.microsoft.com/library/6sh2ey19.aspx) [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) silného typu objektu.

### <a name="create-a-visual-studio-project"></a>Vytvoření projektu Visual Studio

1. Jako správce otevřete aplikaci Visual Studio pravým tlačítkem myši na program v nabídce Start a klikněte na příkaz **Spustit jako správce**.

1. Vytvořte nový projekt aplikace konzoly. Klikněte na nabídku **soubor** a vyberte **Nový**a potom klikněte na **projekt**. V dialogovém okně **Nový projekt** klikněte na tlačítko **Visual Basic** (Pokud **Visual Basic** nezobrazí, vyhledejte v části **Další jazyky**), klikněte na položku Šablona **Aplikace konzoly** a nazvěte ji **QueueSample**. Použijte výchozí **umístění**. Klikněte na **OK** vytvořte projekt.

1. Pomocí Správce balíčku NuGet přidat knihovny služby Bus do projektu:
    1. V okně Průzkumník projektu **QueueSample** pravým tlačítkem klikněte na **Spravovat balíčků NuGet**.
    2. V dialogovém okně **Spravovat balíčků Nuget** klikněte na kartu **Procházet** a vyhledejte **Bus služby Azure**a potom klikněte na tlačítko **nainstalovat**.
<br />
1. V Průzkumníku poklikejte na soubor Program.cs a otevřete ji v editoru Visual Studio. Změna názvu názvů z jeho výchozí název `QueueSample` k `Microsoft.ServiceBus.Samples`.

    ```
    Microsoft.ServiceBus.Samples
    {
        ...
    ```

1. Změnit `using` příkazy uvedené v následující kód.

    ```
    using System;
    using System.Collections.Generic;
    using System.Data;
    using System.IO;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.ServiceBus.Messaging;
    ```

1. Vytvoření textového souboru s názvem Data.csv a kopírovat v následující text oddělený čárkami.

    ```
    IssueID,IssueTitle,CustomerID,CategoryID,SupportPackage,Priority,Severity,Resolved
    1,Package lost,1,1,Basic,5,1,FALSE
    2,Package damaged,1,1,Basic,5,1,FALSE
    3,Product defective,1,2,Premium,5,2,FALSE
    4,Product damaged,2,2,Premium,5,2,FALSE
    5,Package lost,2,2,Basic,5,2,TRUE
    6,Package lost,3,2,Basic,5,2,FALSE
    7,Package damaged,3,7,Premium,5,3,FALSE
    8,Product defective,3,2,Premium,5,3,FALSE
    9,Product damaged,4,6,Premium,5,3,TRUE
    10,Package lost,4,8,Basic,5,3,FALSE
    11,Package damaged,5,4,Basic,5,4,FALSE
    12,Product defective,5,4,Basic,5,4,FALSE
    13,Package lost,6,8,Basic,5,4,FALSE
    14,Package damaged,6,7,Premium,5,5,FALSE
    15,Product defective,6,2,Premium,5,5,FALSE
    ```

    Uložte a zavřete soubor Data.csv pamatovat na místo, ve kterém jste ho uložili.

1. V Průzkumníku klikněte pravým tlačítkem myši na název projektu (v tomto příkladu **QueueSample**), klikněte na tlačítko **Přidat**a potom klikněte na **Existující položky**.

1. Přejděte k souboru Data.csv, který jste vytvořili v kroku 6. Klikněte na soubor a potom klikněte na **Přidat**. Zkontrolujte, že je * *všechny soubory (*.*) ** je vybraná v seznamu typů souborů.

### <a name="create-a-method-that-parses-a-list-of-messages"></a>Vytvořte metodu, která analyzuje seznam zpráv

1. V `Program` třídy před `Main()` metody, deklarovat dvě proměnné: jeden typ **datové tabulky**má být v seznamu zpráv v Data.csv. Druhý by měl být typu objektu seznamu silného [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx). Ten je seznam brokered zprávy, které se používají následující kroky v tomto kurzu.

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        class Program
        {
    
            private static DataTable issues;
            private static List<BrokeredMessage> MessageList;
    ```

1. Vnější `Main()`, definování `ParseCSV()` metodu, která analyzuje seznamu zpráv v Data.csv a načte zprávy do tabulky [datové tabulky](https://msdn.microsoft.com/library/azure/system.data.datatable.aspx) , jak ukazuje tato část. Metoda vrátí objekt **datové tabulky** .

    ```
    static DataTable ParseCSVFile()
    {
        DataTable tableIssues = new DataTable("Issues");
        string path = @"..\..\data.csv";
        try
        {
            using (StreamReader readFile = new StreamReader(path))
            {
                string line;
                string[] row;
    
                // create the columns
                line = readFile.ReadLine();
                foreach (string columnTitle in line.Split(','))
                {
                    tableIssues.Columns.Add(columnTitle);
                }
    
                while ((line = readFile.ReadLine()) != null)
                {
                    row = line.Split(',');
                    tableIssues.Rows.Add(row);
                }
            }
        }
        catch (Exception e)
        {
            Console.WriteLine("Error:" + e.ToString());
        }
    
        return tableIssues;
    }
    ```

1. V `Main()` způsob, přidáte příkaz, který volá `ParseCSVFile()` metodu:

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
    
    }
    ```

### <a name="create-a-method-that-loads-the-list-of-messages"></a>Vytvořte metodu, která načte seznamu zpráv

1. Vnější `Main()`, definování `GenerateMessages()` metodu, která přijímá objekt **datové tabulky** vrácené `ParseCSVFile()` a načte tabulku do seznamu silného typu brokered zpráv. Metoda vrátí **seznam** objekt jako v následujícím příkladu. 

    ```
    static List<BrokeredMessage> GenerateMessages(DataTable issues)
    {
        // Instantiate the brokered list object
        List<BrokeredMessage> result = new List<BrokeredMessage>();
    
        // Iterate through the table and create a brokered message for each row
        foreach (DataRow item in issues.Rows)
        {
            BrokeredMessage message = new BrokeredMessage();
            foreach (DataColumn property in issues.Columns)
            {
                message.Properties.Add(property.ColumnName, item[property]);
            }
            result.Add(message);
        }
        return result;
    }
    ```

1. V `Main()`, přímo za volání `ParseCSVFile()`, přidáte příkaz, který volání `GenerateMessages()` metodu s vrácenou hodnotu z `ParseCSVFile()` jako argument:

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
        MessageList = GenerateMessages(issues);
    }
    ```

### <a name="obtain-user-credentials"></a>Získejte oprávnění uživatele

1. Nejprve vytvořte tři proměnných globální řetězce a podržte tyto hodnoty. Deklarovat tyto proměnné přímo za předchozí deklarace proměnných; Příklad:

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        public class Program
        {
    
            private static DataTable issues;
            private static List<BrokeredMessage> MessageList; 

            // Add these variables
            private static string ServiceNamespace;
            private static string sasKeyName = "RootManageSharedAccessKey";
            private static string sasKeyValue;
            …
    ```

1. Dále vytvořte funkci, která přijímá a ukládá obor názvů služby a přidružení zabezpečení klíče. Přidejte tuto metodu mimo `Main()`. Příklad: 

    ```
    static void CollectUserInput()
    {
        // User service namespace
        Console.Write("Please enter the namespace to use: ");
        ServiceNamespace = Console.ReadLine();
    
        // Issuer key
        Console.Write("Enter the SAS key to use: ");
        sasKeyValue = Console.ReadLine();
    }
    ```

1. V `Main()`, přímo za volání `GenerateMessages()`, přidáte příkaz, který volání `CollectUserInput()` metodu:

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
        MessageList = GenerateMessages(issues);
        
        // Collect user input
        CollectUserInput();
    }
    ```

### <a name="build-the-solution"></a>Vytvoření řešení

Z nabídky **sestavení** ve Visual Studiu můžete klikněte na tlačítko **Sestavit řešení** nebo stisknutím **Kombinace kláves Ctrl + Shift + B** zatím potvrďte přesnost vašem čísle do zaměstnání.

## <a name="create-management-credentials"></a>Vytvoření mapování přihlašovacích správy

V tomto kroku definovat operace správy, které se používají k vytvoření mapování přihlašovacích podpisu (přidružení zabezpečení) s nimiž aplikace oprávnění sdílený přístup.

1. Tento kurz pro přehlednost umístí všechny operace fronty samostatné metody. Vytvoření asynchronní `Queue()` metoda `Program` třídy po `Main()` metody. Příklad:
 
    ```
    public static void Main(string[] args)
    {
    …
    }
    static async Task Queue()
    {
    }
    ```

1. Dalším krokem je vytvoření přidružení zabezpečení přihlašovací údaje pomocí [Poskytovatel TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) objektu. Metoda vytváření zabírá název klíče přidružení zabezpečení a hodnotu získáte v `CollectUserInput()` metody. Přidat následující kód, který `Queue()` metodu:

    ```
    static async Task Queue()
    {
        // Create management credentials
        TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName,sasKeyValue);
    }
    ```

2. Vytvořte nový objekt správy obor názvů s URI obsahující název oboru názvů a správa přihlašovacích údajů získaných v předchozím kroku, jako argumenty. Přidáte tento kód přímo za kód přidali v předchozím kroku. Ujistěte se, pokud chcete nahradit `<yourNamespace>` s názvem obor názvů služby:
    
    ```
    NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    ```

### <a name="example"></a>Příklad

V tomto okamžiku by měla vypadat podobně jako tento kód:

```
using System;
using System.Collections.Generic;
using System.Data;
using System.IO;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.ServiceBus.Samples
{
  public class Program
  {
    private static DataTable issues;
    private static List<BrokeredMessage> MessageList;
    private static string ServiceNamespace;
    private static string sasKeyName = "RootManageSharedAccessKey";
    private static string sasKeyValue;

    static void Main(string[] args)
    {
      // Populate test data
      issues = ParseCSVFile();
      MessageList = GenerateMessages(issues);

      // Collect user input
      CollectUserInput();

      // Add this call
      Task.WaitAll(Queue());
    }

    static async Task Queue()
    {
      // Create management credentials
      TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName, sasKeyValue);
      NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    }

    static DataTable ParseCSVFile()
    {
      DataTable tableIssues = new DataTable("Issues");
      string path = @"..\..\data.csv";
      try
      {
        using (StreamReader readFile = new StreamReader(path))
        {
          string line;
          string[] row;

          // create the columns
          line = readFile.ReadLine();
          foreach (string columnTitle in line.Split(','))
          {
            tableIssues.Columns.Add(columnTitle);
          }

          while ((line = readFile.ReadLine()) != null)
          {
            row = line.Split(',');
            tableIssues.Rows.Add(row);
          }
        }
      }
      catch (Exception e)
      {
        Console.WriteLine("Error:" + e.ToString());
      }

      return tableIssues;
    }

    static List<BrokeredMessage> GenerateMessages(DataTable issues)
    {
      // Instantiate the brokered list object
      List<BrokeredMessage> result = new List<BrokeredMessage>();

      // Iterate through the table and create a brokered message for each row
      foreach (DataRow item in issues.Rows)
      {
        BrokeredMessage message = new BrokeredMessage();
        foreach (DataColumn property in issues.Columns)
        {
          message.Properties.Add(property.ColumnName, item[property]);
        }
        result.Add(message);
      }
      return result;
    }

    static void CollectUserInput()
    {
      // User service namespace
      Console.Write("Please enter the service namespace to use: ");
      ServiceNamespace = Console.ReadLine();

      // Issuer key
      Console.Write("Please enter the issuer key to use: ");
      sasKeyValue = Console.ReadLine();
    }
  }
}
```

## <a name="send-messages-to-the-queue"></a>Odeslání zprávy do fronty

V tomto kroku je vytvořit fronty a potom odesílat zprávy obsažené v seznamu brokered zpráv do této fronty.

### <a name="create-queue-and-send-messages-to-the-queue"></a>Vytvoření fronty a posílání zpráv do fronty

1. Nejprve vytvořte fronty. Například volat `myQueue`a deklarovat přímo za operace správy jste přidali `Queue()` metoda v posledním kroku:

    ```
    QueueDescription myQueue;

    if (namespaceClient.QueueExists("IssueTrackingQueue"))
    {
        namespaceClient.DeleteQueue("IssueTrackingQueue");
    }

    myQueue = namespaceClient.CreateQueue("IssueTrackingQueue");
    ```

1. V `Queue()` metody, vytvořte zpráv factory objekt s nově vytvořený služby Bus identifikátor URI jako argument. Přidejte následující kód přímo za správy operace, které jste přidali v posledním kroku. Ujistěte se, pokud chcete nahradit `<yourNamespace>` s názvem obor názvů služby:

    ```
    MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    ```

1. Dále vytvořte objekt fronty pomocí třídy [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) . Hned za kód, který jste přidali v posledním kroku přidání následující kód:

    ```
    QueueClient myQueueClient = factory.CreateQueueClient("IssueTrackingQueue");
    ```

1. Dále přidejte kód, který prochází seznamu brokered zprávy, kterou jste dříve vytvořili, odesláním každé do fronty. Přidejte následující kód přímo za `CreateQueueClient()` následný výběr v předchozím kroku:
    
    ```
    // Send messages
    Console.WriteLine("Now sending messages to the queue.");
    for (int count = 0; count < 6; count++)
    {
        var issue = MessageList[count];
        issue.Label = issue.Properties["IssueTitle"].ToString();
        await myQueueClient.SendAsync(issue);
        Console.WriteLine(string.Format("Message sent: {0}, {1}", issue.Label, issue.MessageId));
    }
    ```

## <a name="receive-messages-from-the-queue"></a>Zprávy ve frontě

V tomto kroku dostanete v seznamu zpráv ve frontě, kterou jste vytvořili v předchozím kroku.

### <a name="create-a-receiver-and-receive-messages-from-the-queue"></a>Vytvoření příjemce a přijímání zpráv ve frontě

V `Queue()` způsobem iteraci fronty a přijímání zpráv metodou [QueueClient.ReceiveAsync](https://msdn.microsoft.com/library/azure/dn130423.aspx) vytištění každá zpráva ke konzole. Přidejte následující kód přímo za kód, který jste přidali v předchozím kroku:

```
Console.WriteLine("Now receiving messages from Queue.");
BrokeredMessage message;
while ((message = await myQueueClient.ReceiveAsync(new TimeSpan(hours: 0, minutes: 1, seconds: 5))) != null)
    {
        Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
        message.Complete();
    
        Console.WriteLine("Processing message (sleeping...)");
        Thread.Sleep(1000);
    }
```

Všimněte si, že `Thread.Sleep` se používá pouze tak, aby napodobily zprávu a nejspíš nemusí ho skutečné zpráv aplikace.

### <a name="end-the-queue-method-and-clean-up-resources"></a>Ukončení metodu fronty a vyčistit zdroje

Hned za předchozí kód přidáte následující kód vyčištění zprávy factory a fronty prostředky:

```
factory.Close();
myQueueClient.Close();
namespaceClient.DeleteQueue("IssueTrackingQueue");
```

### <a name="call-the-queue-method"></a>Volat metodu fronty

Posledním krokem je přidat příkaz, který volá `Queue()` podle `Main()`. Přidejte následující zvýrazněný řádek kód na konci Main():
    
```
public static void Main(string[] args)
{
    // Collect user input
    CollectUserInput();
    
    // Populate test data
    issues = ParseCSVFile();
    MessageList = GenerateMessages(issues);
    
    // Add this call
    Queue();
}
```

### <a name="example"></a>Příklad

Následující kód obsahuje kompletní **QueueSample** aplikaci.

```
using System;
using System.Collections.Generic;
using System.Data;
using System.IO;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.ServiceBus.Samples
{
    public class Program
    {
        private static DataTable issues;
        private static List<BrokeredMessage> MessageList;

        // Add these variables
        private static string ServiceNamespace;
        private static string sasKeyName = "RootManageSharedAccessKey";
        private static string sasKeyValue;

        static void Main(string[] args)
        {

            // Populate test data
            issues = ParseCSVFile();
            MessageList = GenerateMessages(issues);

            // Collect user input
            CollectUserInput();

            Queue();

        }

        static async Task Queue()
        {
            // Create management credentials
            TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName, sasKeyValue);
            NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);

            QueueDescription myQueue;

            if (namespaceClient.QueueExists("IssueTrackingQueue"))
            {
                namespaceClient.DeleteQueue("IssueTrackingQueue");
            }
            
            myQueue = namespaceClient.CreateQueue("IssueTrackingQueue");
            
            MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);

            QueueClient myQueueClient = factory.CreateQueueClient("IssueTrackingQueue");

            // Send messages
            Console.WriteLine("Now sending messages to the queue.");
            for (int count = 0; count < 6; count++)
            {
                var issue = MessageList[count];
                issue.Label = issue.Properties["IssueTitle"].ToString();
                await myQueueClient.SendAsync(issue);
                Console.WriteLine(string.Format("Message sent: {0}, {1}", issue.Label, issue.MessageId));
            }

            Console.WriteLine("Now receiving messages from Queue.");
            BrokeredMessage message;
            while ((message = await myQueueClient.ReceiveAsync(new TimeSpan(hours: 0, minutes: 1, seconds: 5))) != null)
            {
                Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
                message.Complete();

                Console.WriteLine("Processing message (sleeping...)");
                Thread.Sleep(1000);
            }

            factory.Close();
            myQueueClient.Close();
            namespaceClient.DeleteQueue("IssueTrackingQueue");


        }

        static void CollectUserInput()
        {
            // User service namespace
            Console.Write("Please enter the namespace to use: ");
            ServiceNamespace = Console.ReadLine();

            // Issuer key
            Console.Write("Enter the SAS key to use: ");
            sasKeyValue = Console.ReadLine();
        }

        static List<BrokeredMessage> GenerateMessages(DataTable issues)
        {
            // Instantiate the brokered list object
            List<BrokeredMessage> result = new List<BrokeredMessage>();

            // Iterate through the table and create a brokered message for each row
            foreach (DataRow item in issues.Rows)
            {
                BrokeredMessage message = new BrokeredMessage();
                foreach (DataColumn property in issues.Columns)
                {
                    message.Properties.Add(property.ColumnName, item[property]);
                }
                result.Add(message);
            }
            return result;
        }

        static DataTable ParseCSVFile()
        {
            DataTable tableIssues = new DataTable("Issues");
            string path = @"..\..\data.csv";
            try
            {
                using (StreamReader readFile = new StreamReader(path))
                {
                    string line;
                    string[] row;

                    // create the columns
                    line = readFile.ReadLine();
                    foreach (string columnTitle in line.Split(','))
                    {
                        tableIssues.Columns.Add(columnTitle);
                    }

                    while ((line = readFile.ReadLine()) != null)
                    {
                        row = line.Split(',');
                        tableIssues.Rows.Add(row);
                    }
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Error:" + e.ToString());
            }

            return tableIssues;
        }
    }
}
```

## <a name="build-and-run-the-queuesample-application"></a>Vytvoření a spuštění aplikace QueueSample

Teď, když jste dokončili předchozích kroků, můžete vytvořit a spustit aplikaci **QueueSample** .

### <a name="build-the-queuesample-application"></a>Vytvoření aplikace QueueSample

Ve Visual Studiu, v nabídce **sestavení** klikněte na tlačítko **Sestavit řešení**nebo stiskněte **Kombinaci kláves Ctrl + Shift + B**. Pokud narazíte na chyby, ověřte správnost kódu podle dokončení příkladu prezentovat na konci v předchozím kroku.

## <a name="next-steps"></a>Další kroky

Tento kurz ukázal k vytváření klienta služby Bus aplikace a přihlašovacích údajů pomocí funkce zasílání zpráv služby Bus brokered. Výukové podobné využívající služby Bus [Relay](service-bus-messaging-overview.md#Relayed-messaging)najdete v článku [služby Bus přenos zpráv kurz](../service-bus-relay/service-bus-relay-tutorial.md).

Další informace o [Bus služby](https://azure.microsoft.com/services/service-bus/), najdete v těchto tématech.

- [Přehled služeb Bus zasílání zpráv](service-bus-messaging-overview.md)
- [Základy Bus služby](service-bus-fundamentals-hybrid-solutions.md)
- [Služba Bus architektura](service-bus-architecture.md)

