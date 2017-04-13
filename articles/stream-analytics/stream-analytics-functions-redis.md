<properties
    pageTitle="Výstup do Azure Redis mezipaměti, pomocí funkcí Azure z Azure toku analýzy | Microsoft Azure"
    description="Zjistěte, jak používat funkci Azure připojení Bus do služby fronty k naplnění plánováním Azure Redis mezipaměti z výstupu projektu toku analýzy."
    keywords="datového proudu redis mezipaměti bus frontě"
    services="stream-analytics"
    authors="ryancrawcour"
    manager="jhubbard"
    documentationCenter=""
    />

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/26/2016"
    ms.author="ryancraw"/>

# <a name="how-to-store-data-from-azure-stream-analytics-in-an-azure-redis-cache-using-azure-functions"></a>Postup při ukládání dat z Azure toku analýzy do mezipaměti Redis Azure pomocí funkce Azure

Azure toku analýzy můžete rychle vyvíjet a nasazení řešení nízké náklady získat další informace v reálném čase z zařízení, senzory, infrastruktura a aplikací nebo libovolnou toku dat. Umožňuje různých případy použití jako je v reálném čase Správa a sledování, příkaz ovládacího prvku, podvodům zjišťování, připojeného automobilů a spousta dalšího. V těchto případech je vhodné ukládat data výstupu Azure toku analýz do rozložení dat úložiště například Azure Redis mezipaměti.

Předpokládejme, že jsou součástí telekomunikačních společnosti. Chcete zjistit SIM podvod kde více volání pocházejících z stejnou identitu současně čas, ale v různých geograficky umístění. Jsou úkol s ukládáním všech možných podvodné telefonní hovory v Azure Redis mezipaměti. V tomto blogu nabízíme pokyny na tom, jak můžete snadno dokončení úkolu. 

## <a name="prerequisites"></a>Zjistit předpoklady pro
Dokončení [v reálném čase podvod zjišťování] [ fraud-detection] walk-through ASA

## <a name="architecture-overview"></a>Přehled
![Snímek obrazovky s architektura](./media/stream-analytics-functions-redis/architecture-overview.png)

Jak ukazuje předchozí obrázek, analýzy toku umožňuje zadávání dat do dotazu a odešle výstup datových proudů. Podle výstup můžete Azure funkce pak aktivace určitý typ události. 

V tomto blogu jsme zaměření se na funkce Azure součástí tohoto kanálu nebo konkrétněji na aktivaci události, která ukládá podvodné data do mezipaměti.
Po dokončení [v reálném čase podvod zjišťování] [ fraud-detection] kurzu máte vstup (rozbočovači události), dotazu a výstup (úložiště objektů blob) už nakonfigurovali a systémem. V tomto blogu Změníme výstup můžete Bus frontě. Až to jsme funkci Azure připojení k této fronty. 

## <a name="create-and-connect-a-service-bus-queue-output"></a>Vytvoření a připojte Bus frontě výstup
Bus frontě vytvoříte kroky 1 a 2 .NET v části [Začínáme s služby Bus fronty][servicebus-getstarted].
Teď Pojďme připojení fronty vytvořený v dřívější zjišťování walk-through podvod úlohy toku analýzy.



1. Na portálu Azure přejděte na zásuvné **výstupy** úlohy a klikněte na **Přidat** v horní části stránky.

    ![Přidání výstupy](./media/stream-analytics-functions-redis/adding-outputs.png)

2. Zvolte **Bus frontě** jako **dřez** a postupujte podle pokynů na obrazovce. Nezapomeňte zvolte obor názvů Bus frontě vytvořené v části [Začínáme s služby Bus fronty][servicebus-getstarted]. Až budete hotovi, klepněte na tlačítko "správné".
3. Zadejte následující hodnoty:
    - **Formát Serializer události**: JSON
    - **Kódování**: UTF8
    - **Formát**: řádek oddělené

4. Klikněte na tlačítko **vytvořit** přidat tento zdroj a ověřte, že toku analýzy můžete úspěšně se připojit k účtu úložiště.

5. Na kartě **Query** nahraďte aktuální dotaz takto. Nahraďte *[Služby BUS jméno]* výstup název, který jste vytvořili v kroku 3. 

    ```    

        SELECT 
            System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1, 
            CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2

        INTO [YOUR SERVICE BUS NAME]
    
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
        JOIN CallStream CS2 TIMESTAMP BY CallRecTime
            ON CS1.CallingIMSI = CS2.CallingIMSI AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
    
        WHERE CS1.SwitchNum != CS2.SwitchNum
    
    ```

## <a name="create-an-azure-redis-cache"></a>Vytvoření Azure Redis mezipaměti
Vytvoření mezipaměti Azure Redis podle pokynů v části .NET v [tom, jak používat Azure Redis mezipaměti] [ use-rediscache] dokud oddílu s názvem ***Konfigurace klientů mezipaměti***.
Po dokončení máte novou mezipaměť Redis. V části **všechna nastavení**vyberte **přístupových kláves z verze** a poznámky dolů ***primární připojovací řetězec***.

![Snímek obrazovky s architektura](./media/stream-analytics-functions-redis/redis-cache-keys.png)

## <a name="create-an-azure-function"></a>Vytvoření Azure funkce
Postupujte podle [vytvořit svůj první funkce Azure] [ functions-getstarted] kurz seznámení s funkcemi Azure. Pokud už máte Azure funkci, kterou chcete použít, pak pokračujte [psaní Redis mezipaměti](#Writing-to-Redis-Cache)

1. Na portálu vyberte aplikaci služby z navigace na levé straně klikněte na název Azure funkce aplikace dostali na web appu funkce.
    ![Snímek obrazovky se aplikace služeb (funkce)](./media/stream-analytics-functions-redis/app-services-function-list.png)

2. Klikněte na **nové funkce > ServiceBusQueueTrigger – C#**. Následující pole postupujte podle těchto pokynů:
    - **Název fronty**: stejný název jako název jste zadali při vytvoření fronty v [Začít pracovat s služby Bus fronty] [ servicebus-getstarted] (nikoli název služby bus). Zkontrolujte, že používáte fronty, který je spojený s výstupu toku analýzy.
    - **Služba Bus připojení**: klikněte na **Přidat připojovací řetězec**. Najít připojovací řetězec, přejděte na portál klasické, vyberte **Bus služby**, bus služby, kterou jste vytvořili a **Informace o připojení** v dolní části obrazovky. Zkontrolujte, že jste na hlavní obrazovce na této stránce. Kopírování a vkládání připojovací řetězec. Neváhejte zadejte libovolný název připojení.
    
        ![Snímek obrazovky s bus připojení služby](./media/stream-analytics-functions-redis/servicebus-connection.png)
    - **AccessRights**: Zvolte **Správa**


3. Klikněte na tlačítko **vytvořit**

## <a name="writing-to-redis-cache"></a>Psaní Redis mezipaměti
Teď jsme vytvořili Azure funkci, která bude číst z fronty Bus služby. Vše, co zbývá je naše funkce můžete začít psát pomocí tato data do mezipaměti Redis. 

1. Vyberte nově vytvořený **ServiceBusQueueTrigger**a v pravém horním rohu klikněte na **Nastavení aplikace (funkce)** . Vyberte **přejděte na nastavení služby aplikace > Nastavení > Nastavení aplikace**

2. V části připojení řetězců vytvořte název v oddílu **název** . Vložte primární připojovací řetězec, který jste našli v kroku **vytvořit mezipaměti Redis** do části **hodnota** . Vyberte **vlastní** místo s textem **Databáze SQL**.

3. Nahoře na tlačítko **Uložit** .

    ![Snímek obrazovky s bus připojení služby](./media/stream-analytics-functions-redis/function-connection-string.png)

4. Teď přejděte zpátky na nastavení služeb aplikací a vyberte **Nástroje > aplikace služby editoru (verze Preview) > na > Přejít**.

    ![Snímek obrazovky s bus připojení služby](./media/stream-analytics-functions-redis/app-service-editor.png)

5. V editoru podle svého výběru vytvořte JSON soubor s názvem **project.json** s tímto a uložení na místní disk.

        {
            "frameworks": {
                "net46": {
                    "dependencies": {
                        "StackExchange.Redis":"1.1.603",
                        "Newtonsoft.Json": "9.0.1"
                    }
                }
            }
        }

6. Tento soubor nahrajte do kořenového adresáře vaší funkci (ne KOŘENOVÝCH). Měli byste vidět umístit do souboru nazvaného **project.lock.json** automaticky potvrzení, že Nuget balíčky "StackExchange.Redis" a "Newtonsoft.Json" jste importovali.

7. V souboru **run.csx** nahraďte kód předem vygenerovaných následující kód. Ve funkci lazyConnection nahraďte názvem, který jste vytvořili v kroku 2 **uložte data do mezipaměti Redis**"Název připojení".

````

    using System;
    using System.Threading.Tasks;
    using StackExchange.Redis;
    using Newtonsoft.Json;
    using System.Configuration;
    
    public static void Run(string myQueueItem, TraceWriter log)
    {
        log.Info($"Function processed message: {myQueueItem}");

        // Connection refers to a property that returns a ConnectionMultiplexer
        IDatabase db = Connection.GetDatabase();
        log.Info($"Created database {db}");
    
        // Parse JSON and extract the time
        var message = JsonConvert.DeserializeObject<dynamic>(myQueueItem);
        string time = message.time;
        string callingnum1 = message.callingnum1;

        // Perform cache operations using the cache object...
        // Simple put of integral data types into the cache
        string key = time + " - " + callingnum1;
        db.StringSet(key, myQueueItem);
        log.Info($"Object put in database. Key is {key} and value is {myQueueItem}");

        // Simple get of data types from the cache
        string value = db.StringGet(key);
        log.Info($"Database got: {value}"); 
    }

    // Connect to the Service Bus
    private static Lazy<ConnectionMultiplexer> lazyConnection = 
        new Lazy<ConnectionMultiplexer>(() =>
            {
                var cnn = ConfigurationManager.ConnectionStrings["CONN NAME"].ConnectionString
                return ConnectionMultiplexer.Connect();
            });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

````

## <a name="start-the-stream-analytics-job"></a>Spuštění úlohy toku analýzy

1. Spusťte aplikaci telcodatagen.exe. Použití vypadá takto:````telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]````

2. Z zásuvné toku analýzy úlohy na portálu klikněte na tlačítko **Start** v horní části stránky.

    ![Snímek obrazovky s zahájení práce](./media/stream-analytics-functions-redis/starting-job.png)

3. V zobrazeném zásuvné **zahájení projektu** vyberte **Spustit** a potom klikněte na tlačítko **Start** v dolní části obrazovky. Změny stavu úlohy počáteční a po některé změny čas spuštění.
 
    ![Snímek obrazovky výběru času zahájení projektu](./media/stream-analytics-functions-redis/start-job-time.png)

## <a name="run-solution-and-check-results"></a>Spusťte řešení a zkontrolujte výsledky
Přejdete zpět na stránku **ServiceBusQueueTrigger** , teď byste měli vidět příkazy protokolu. Tyto protokoly zobrazit máte něco z Bus frontě, přepněte do databáze a načíst pomocí času jako klíč!

Ověřte, jestli je vaše data v mezipaměti Redis, přejděte na stránku Redis mezipaměti v novém portálu (jak je uvedeno v předchozím kroku [vytvořit mezipaměti Redis Azure](#Create-an-Azure-Redis-Cache) ) a vyberte konzoly.

Teď můžete napsat Redis příkazů, které chcete potvrdit, že data ve skutečnosti v mezipaměti.

![Snímek obrazovky s Redis konzoly](./media/stream-analytics-functions-redis/redis-console.png)

## <a name="next-steps"></a>Další kroky
Připojte se k oslavám o věcech nové funkce Azure a toku analýzy umí společně a jsme ať že tím se odemknou nové možnosti pro vás. Pokud máte nějaké názory na požadovaná další, neváhejte [Azure UserVoice webu](https://feedback.azure.com/forums/270577-stream-analytics)používat.

Pokud jste nový Microsoft Azure, pomozte vyzkoušejte si to tak, že [bezplatný účet Azure zkušební verzi](https://azure.microsoft.com/pricing/free-trial/). Pokud začínáte toku analýzy, potom jsme pozvat k [vytvoření prvního úlohy toku analýzy](stream-analytics-create-a-job.md).

Pokud potřebujete některý Nápověda nebo máte nějaké dotazy, publikování na [webu MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) nebo [Stackoverflow](http://stackoverflow.com/questions/tagged/azure-stream-analytics) fóra. 

Zobrazí se také v následujících zdrojích:

- [Referenční informace pro vývojáře Azure funkcí](../azure-functions/functions-reference.md)
- [Azure funkce C# referenční informace pro vývojáře](../azure-functions/functions-reference-csharp.md)
- [Azure funkce F # referenční informace pro vývojáře](../azure-functions/functions-reference-fsharp.md)
- [Referenční informace pro vývojáře Azure NodeJS funkce](../azure-functions/functions-reference.md)
- [Azure aktivace funkcí a vazby](../azure-functions/functions-triggers-bindings.md)
- [Jak sledovat Azure Redis mezipaměti](../redis-cache/cache-how-to-monitor.md)

Pokud chcete upozorňovat na nové zprávy na nejnovější příspěvky a funkce, postupujte [@AzureStreaming](https://twitter.com/AzureStreaming) na Twitter.


[fraud-detection]: stream-analytics-real-time-fraud-detection.md
[servicebus-getstarted]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[use-rediscache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md
[functions-getstarted]: ../azure-functions/functions-create-first-azure-function.md
