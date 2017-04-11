<properties
    pageTitle="Přenos dat Azure Diagnostika v žádanou cestu pomocí rozbočovače události | Microsoft Azure"
    description="Ukazuje, jak nakonfigurovat Azure diagnostiky události rozbočovače konce, včetně pokyny pro běžné scénáře."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags
    ms.service="event-hubs"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="07/14/2016"
    ms.author="sethm" />

# <a name="streaming-azure-diagnostics-data-in-the-hot-path-by-using-event-hubs"></a>Přenos dat Azure Diagnostika v žádanou cestu pomocí rozbočovače události

Azure diagnostiky způsoby flexibilní shromáždit protokoly metriky a od cloud services virtuálních počítačích (VMs) a přenos výsledky k základnímu úložišti Azure. Spuštění v časové rozmezí březen 2016 (SDK 2.9), můžete dřez diagnostiky úplně vlastní zdroje dat a přenášet data kritická cesta v sekundách pomocí [Rozbočovače Azure události](https://azure.microsoft.com/services/event-hubs/).

Podporované datové typy, patří:

- Událost trasování pro Windows (trasování událostí pro Windows) události
- Výkonnosti
- Protokoly událostí systému Windows
- Protokoly aplikace
- Azure infrastruktury protokolování diagnostiky

Tento článek popisuje, jak nakonfigurovat Azure diagnostiky rozbočovače událost z konce. Pokyny slouží také k následující obvyklé scénáře:

- Přizpůsobení metriky, získáte sinked rozbočovače událostí a protokolování
- Změna konfigurace v každém prostředí
- Jak zobrazit události rozbočovače toku dat
- Jak řešit problémy s připojení  

## <a name="prerequisites"></a>Zjistit předpoklady pro

Událost rozbočovače zpracování v Azure diagnostiky je podporované v cloudové služby, VMs, sady měřítko virtuálního počítače a struktury služby počáteční na Azure SDK 2.9 a odpovídající nástroje Azure for Visual Studio.

- Azure diagnostiky rozšíření 1,6 ([SDK Azure .NET 2.9 nebo novější](https://azure.microsoft.com/downloads/) zaměřuje to ve výchozím nastavení)
- [Visual Studio 2013 nebo novější](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
- Existující konfigurace Azure Diagnostika v aplikaci pomocí souboru *.wadcfgx* a jednu z těchto způsobů:
    - Visual Studio: [Konfigurace diagnostických nástrojů pro Azure cloudovými službami a virtuálních počítačích](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md)
    - Prostředí Windows PowerShell: [Povolení Diagnostika v Azure Cloud Services pomocí prostředí PowerShell](../cloud-services/cloud-services-diagnostics-powershell.md)
- Obor názvů rozbočovače události zřízení na článek [Začínáme s rozbočovače události](./event-hubs-csharp-ephcs-getstarted.md)

## <a name="connect-azure-diagnostics-to-event-hubs-sink"></a>Připojení k události rozbočovače jímky diagnostiky Azure

Azure diagnostiky vždy propadů protokoly a metriky, ve výchozím nastavení účet Azure úložiště. Aplikace může navíc dřez události rozbočovače přidáním nového oddílu **propadů** do prvku **WadCfg** v části **PublicConfig** *.wadcfgx* souboru. Ve Visual Studiu *.wadcfgx* soubor uložený v následujícím umístění: **Projekt služby cloudu** > **role** >  **(RoleName)** > **diagnostics.wadcfgx** soubor.

```
<SinksConfig>
  <Sink name="HotPath">
    <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" />
  </Sink>
</SinksConfig>
```

V tomto příkladu adresa URL centrální události nastavena na plně kvalifikovaný názvů centru události: událost rozbočovače názvů + "/" + události centrální jméno.  

Adresa URL rozbočovače událostí se zobrazí v [Azure portál](http://go.microsoft.com/fwlink/?LinkID=213885) na řídicím panelu rozbočovače události.  

Název **dřez** můžete nastavit libovolný platný řetězcový, dokud se stejnou hodnotu konzistentní použít v celém konfiguračního souboru.

> [AZURE.NOTE]  Doména může obsahovat další propadů, například *applicationInsights* nakonfigurovány v této části. Azure diagnostiky umožňuje jeden nebo více propadů definovat pokud každý jímky je také deklarováno v části **PrivateConfig** .  

Jímce rozbočovače události musí být také deklarované a v části **PrivateConfig** konfiguračního souboru *.wadcfgx* definované.

```
<PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <StorageAccount name="" key="" endpoint="" />
  <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" SharedAccessKey="9B3SwghJOGEUvXigc6zHPInl2iYxrgsKHZoy4nm9CUI=" />
</PrivateConfig>
```

`SharedAccessKeyName` Hodnota musí odpovídat klíč přidružení sdílené podpis aplikace Access (zabezpečení) a zásady, která byla definována v oboru **Rozbočovače události** . Přejděte na řídicí panel rozbočovače události na [portálu Azure](https://manage.windowsazure.com), klikněte na kartu **Konfigurovat** a nastavení pojmenované zásadu (například "SendRule"), který má oprávnění *Odeslat* . **StorageAccount** je také deklarované v **PrivateConfig**. Je potřeba změnit tady hodnoty, pokud pracují. V tomto příkladu jsme nechte hodnoty prázdné, tedy znak, že podřízené materiálů nastaví hodnoty. Například, konfiguračního souboru prostředí *ServiceConfiguration.Cloud.cscfg* nastaví danou prostředí názvy a klíče.  

> [AZURE.WARNING] Klíč přidružení události rozbočovače zabezpečení je uložený ve formátu prostého textu v souboru *.wadcfgx* . Tento klíč často se změnami do zdrojového kódu nebo je k dispozici jako datový zdroj v dialogovém okně Vytvořit server, měli byste ho chránit podle potřeby. Doporučujeme použít klávesovou přidružení zabezpečení s oprávnění *Odeslat jenom* tak, aby se zlými úmysly nelze zapisovat do centra událost, ale poslouchat ho nebo spravovat.

## <a name="configure-azure-diagnostics-logs-and-metrics-to-sink-with-event-hubs"></a>Konfigurace protokolování diagnostiky Azure a metriky Dřez s rozbočovače události

Jak bylo popsáno dříve, všechna výchozí a vlastní diagnostiky data, metriky a protokolů, je automaticky sinked k základnímu úložišti Azure v nakonfigurované intervalů. Události rozbočovače a jakékoli další jímky můžete určit kořenové nebo listu uzel v hierarchii být sinked s centru události. Jedná se o události trasování událostí pro Windows, výkonnosti, protokoly událostí systému Windows a aplikace protokoly.   

Je důležité zvážit počet datových bodů má ve skutečnosti přenesou rozbočovače události. Obvykle vývojáři přenášet data aktivní cestu nízkou latencí, který musí být spotřebované množství a interpretovaný rychle. Systémy, které sledují upozornění nebo Automatické měřítko pravidla příkladů. Vývojář může taky nakonfigurovat úložiště alternativní analýza nebo storu – například Azure toku analýzy, Elasticsearch, vlastní sledování systému nebo Oblíbené systém sledování provedenými ostatními uživateli.

Zde jsou některé příklady konfigurace.

```
<PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
</PerformanceCounters>
```

V následujícím příkladu se použije jímce uzel **jako** nadřazený v hierarchii, což znamená, že všechny podřízené **jako** se pošle rozbočovače události.  

```
<PerformanceCounters scheduledTransferPeriod="PT1M">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" sinks="HotPath" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" sinks="HotPath"/>
  <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" sinks="HotPath"/>
</PerformanceCounters>
```

V předchozím příkladu, jímce platí pouze tři čítačů: **Požadavky ve frontě** **Požadavky odmítnuty**a **% Procesor času**.  

Následující příklad ukazuje, jak můžete vývojář omezit množství odeslaných dat je důležité metriky, které se používají pro tuto službu stavu.  

```
<Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
```

V tomto příkladu jímce se použije pro protokolování a filtrována pouze na úroveň trasování chyby.

## <a name="deploy-and-update-a-cloud-services-application-and-diagnostics-config"></a>Nasazení a aktualizovat Cloudovým službám aplikace a diagnostice konfigurace

Visual Studio poskytuje nejjednodušší cestu pro nasazení aplikace a konfigurace jímky rozbočovače události. Prohlížení a úpravy souboru, otevřete soubor *.wadcfgx* ve Visual Studiu, upravovat a uložit, abyste ji. **Projekt cloudové služby**je tato cesta > **role** > **(RoleName)** > **diagnostics.wadcfgx**.  

V tomto okamžiku nasazení a nasazení aktualizovat akce ve Visual Studiu, Visual Studio Team System a všechny příkazy nebo skripty, které jsou založeny na užívání a MSBuild **/t: publikování** cílové zahrnout *.wadcfgx* procesu obalu. Kromě toho nasazení a aktualizace nasadit soubor Azure pomocí příslušné rozšíření agenta Azure diagnostiky na vaše VMs.

Po nasazení aplikace a konfigurace diagnostiky Azure, zobrazí se okamžitě aktivita v řídicím panelu Centra události. Tento údaj označuje, že budete chtít přejít na zobrazení dat aktivní cesty v posluchače klienta nebo analýzy nástroj podle svého výběru.  

Na následujícím obrázku řídicím panelu událost rozbočovače zobrazuje správný odesílání dat diagnostiky centru události spuštění někdy po 11 odp. Kdy aplikace nasazené se souborem aktualizované *.wadcfgx* a jímce nakonfigurovanou správně.

![][0]  

> [AZURE.NOTE] Když vyberete jednotlivé aktualizace k souboru konfigurace Azure diagnostiky (.wadcfgx), je vhodné použít aktualizace pro celou aplikaci, jakož i konfiguraci pomocí aplikace Visual Studio publikování nebo skriptu prostředí Windows PowerShell.  

## <a name="view-hot-path-data"></a>Zobrazení klávesových cestu dat

Jak bylo popsáno dříve, existuje mnoha případech použít pro příjem a zpracování dat rozbočovače události.

Jedním ze způsobů jednoduché je vytvoření aplikace malý zkušební konzoly poslouchat Centru událostí a tisknout výstupního proudu. Můžete provádět následující kód, který je vysvětleno podrobněji [začít pracovat s rozbočovače události](./event-hubs-csharp-ephcs-getstarted.md)), v aplikaci konzoly.  

Všimněte si, že aplikace konzoly musí obsahovat [události procesor hostitele Nuget balíčku](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).  

Nezapomeňte hodnoty v lomených závorkách v **hlavní** funkce nahradit hodnoty pro zdroje.   

```
//Console application code for EventHub test client
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace EventHubListener
{
    class SimpleEventProcessor : IEventProcessor
    {
        Stopwatch checkpointStopWatch;

        async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
            if (reason == CloseReason.Shutdown)
            {
                await context.CheckpointAsync();
            }
        }

        Task IEventProcessor.OpenAsync(PartitionContext context)
        {
            Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
            this.checkpointStopWatch = new Stopwatch();
            this.checkpointStopWatch.Start();
            return Task.FromResult<object>(null);
        }

        async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (EventData eventData in messages)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                    Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                        context.Lease.PartitionId, data));

                foreach (var x in eventData.Properties)
                {
                    Console.WriteLine(string.Format("    {0} = {1}", x.Key, x.Value));
                }
            }

            //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
            if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
            {
                await context.CheckpointAsync();
                this.checkpointStopWatch.Restart();
            }
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string eventHubConnectionString = "Endpoint= <your connection string>”
            string eventHubName = "<Event Hub name>";
            string storageAccountName = "<Storage account name>";
            string storageAccountKey = "<Storage account key>”;
            string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);

            string eventProcessorHostName = Guid.NewGuid().ToString();
            EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
            Console.WriteLine("Registering EventProcessor...");
            var options = new EventProcessorOptions();
            options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
            eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();

            Console.WriteLine("Receiving. Press enter key to stop worker.");
            Console.ReadLine();
            eventProcessorHost.UnregisterEventProcessorAsync().Wait();
        }
    }
}
```

## <a name="troubleshoot-event-hubs-sink"></a>Poradce při potížích s jímky rozbočovače události

- Centrální události nezobrazuje aktivity události příchozí nebo odchozí podle očekávání.

    Podívejte se, že je vaše Centrum události úspěšně zřízení. V části **PrivateConfig** *.wadcfgx* všechny informace o připojení musí odpovídat hodnoty příslušného zdroje jak je vidět na portálu. Ujistěte se, že máte zásad přidružení zabezpečení definované ("SendRule" v příkladu) na portálu a udělení oprávnění *Odeslat* .  

- Po aktualizaci centrální události už zobrazuje aktivity události příchozí nebo odchozí.

    Nejprve zkontrolujte, zda informace centrální událostí a konfigurace správnost jak je popsáno dříve. Někdy **PrivateConfig** obnovit v aktualizaci nasazení. Doporučené opravný nástroj je proveďte všechny změny *.wadcfgx* v projektu a nabízená aktualizace dokončení aplikací. Pokud to není možné, ujistěte se, že aktualizace diagnostiky posune dokončení **PrivateConfig** obsahující klávesu přidružení zabezpečení.  

- Vyzkoušeli na návrhy řešení a centru události stále nefunguje.

    Podívejte se v úložišti Azure tabulku, která obsahuje protokoly a chyb diagnostických Azure samotné: **WADDiagnosticInfrastructureLogsTable**. Jednou z možností je nástroj například [Průzkumníka úložišť Azure](http://www.storageexplorer.com) a připojení k tomuto účtu úložiště v této tabulce a přidejte do dotazu pro časové razítko posledních 24 hodin. Nástroj slouží k exportu souboru .csv a potom ho otevřete v aplikaci jako je Microsoft Excel. Excel snadno vyhledat řetězce, volající karty, například **EventHubs**zobrazíte, k čemu se chybová zpráva.  

## <a name="next-steps"></a>Další kroky

• [Další informace o rozbočovače události](https://azure.microsoft.com/services/event-hubs/)

## <a name="appendix-complete-azure-diagnostics-configuration-file-wadcfgx-example"></a>Dodatek: Dokončení příkladu Azure diagnostiky konfigurační soubor (.wadcfgx)

```
<?xml version="1.0" encoding="utf-8"?>
<DiagnosticsConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <WadCfg>
      <DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="applicationInsights.errors">
        <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error" />
        <Directories scheduledTransferPeriod="PT1M">
          <IISLogs containerName="wad-iis-logfiles" />
          <FailedRequestLogs containerName="wad-failedrequestlogs" />
        </Directories>
        <PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
        </PerformanceCounters>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
          <DataSource name="Application!*" />
        </WindowsEventLog>
        <CrashDumps>
          <CrashDumpConfiguration processName="WaIISHost.exe" />
          <CrashDumpConfiguration processName="WaWorkerHost.exe" />
          <CrashDumpConfiguration processName="w3wp.exe" />
        </CrashDumps>
        <Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
      </DiagnosticMonitorConfiguration>
      <SinksConfig>
        <Sink name="HotPath">
          <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" />
        </Sink>
        <Sink name="applicationInsights">
          <ApplicationInsights />
          <Channels>
            <Channel logLevel="Error" name="errors" />
          </Channels>
        </Sink>
      </SinksConfig>
    </WadCfg>
    <StorageAccount />
  </PublicConfig>
  <PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <StorageAccount name="" key="" endpoint="" />
    <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" SharedAccessKey="YOUR_KEY_HERE" />
  </PrivateConfig>
  <IsEnabled>true</IsEnabled>
</DiagnosticsConfiguration>
```

Doplňková *ServiceConfiguration.Cloud.cscfg* v tomto příkladu vypadá takto:

```
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="MyFixItCloudService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2015-04.2.6">
  <Role name="MyFixIt.WorkerRole">
    <Instances count="1" />
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="YOUR_CONNECTION_STRING" />
    </ConfigurationSettings>
  </Role>
</ServiceConfiguration>
```

<!-- Images. -->
[0]: ./media/event-hubs-streaming-azure-diags-data/dashboard.png
