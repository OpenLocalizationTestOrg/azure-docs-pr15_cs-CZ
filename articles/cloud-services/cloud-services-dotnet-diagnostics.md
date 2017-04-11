<properties
    pageTitle="Jak používat Azure diagnostiky (.NET) Cloudovým službám | Microsoft Azure"
    description="Použití Azure Diagnostika pro shromažďování dat z Azure cloud Services pro ladění, měření výkonu, sledování, analýza provozu a další."
    services="cloud-services"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="01/25/2016"
    ms.author="robb"/>



# <a name="enabling-azure-diagnostics-in-azure-cloud-services"></a>Povolení Azure Diagnostika v Azure Cloudovým službám

Pozadí na Diagnostika Azure naleznete v tématu [Přehled diagnostiky Azure](../azure-diagnostics.md) .


## <a name="how-to-enable-diagnostics-in-a-worker-role"></a>Jak povolit Diagnostika v roli pracovníka

Tento procházení popisuje, jak implementovat Azure pracovníka roli, která posílá telemetrickými daty pomocí třídy ZDROJ_UDÁLOSTI .NET. Azure diagnostiky umožňuje shromáždit telemetrickými daty a uložte ho do účet Azure úložiště. Při vytváření pracovního role Visual Studio automaticky povolí diagnostiky 1.0 jako součást řešení v Azure SDK pro .NET 2.4 a starších verzích. Následující pokyny popisují proces vytváření roli pracovníka zákaz diagnostiky 1.0 řešení a nasazení diagnostiky 1.2 nebo 1.3 vaší pracovní roli.

### <a name="pre-requisites"></a>Předpoklady
V tomto článku se předpokládá, máte předplatné Azure a, kteří používají aplikaci Visual Studio 2013 s Azure SDK. Pokud nemáte předplatné Azure, můžete zaregistrovat [Bezplatnou zkušební verzi][]. Zkontrolujte, že [instalace a konfigurace prostředí PowerShell Azure verze 0.8.7 nebo novější][].

### <a name="step-1-create-a-worker-role"></a>Krok 1: Vytvoření roli kolegy
1.  Spuštění **aplikace Visual Studio 2013**.
2.  Vytvoření nového projektu **Cloudové služby Azure** z **cloudu** šablony, které cílů .NET Framework 4.5.  Název projektu "WadExample" a klikněte na Ok.
3.  Vyberte **Roli pracovní** a klikněte na Ok. Projekt bude vytvořen.
4.  V **Okně Průzkumník řešení**poklikejte na **WorkerRole1** vlastnosti souboru.
5.  **Konfigurace** pomocí klávesy tab zrušením zaškrtnutí políčka **Povolit diagnostiky** zakázat diagnostiky 1.0 (Azure SDK 2.4 a eariler).
6.  Sestavte řešení chcete ověřit, že máte bez chyb.

### <a name="step-2-instrument-your-code"></a>Krok 2: Nástroje kódu
Nahraďte obsah WorkerRole.cs následující kód. Třídě SampleEventSourceWriter, dědí od [Zdroj_události třídy][]implementuje čtyři metody protokolování: **SendEnums** **MessageMethod**, **SetOther** a **HighFreq**. První parametr metodu **WriteEvent** definuje ID odpovídajících události. Metoda spustit provádí nekonečné smyčky, volající všech metody protokolování implementovaná ve třídě **SampleEventSourceWriter** každých 10 sekund.

    using Microsoft.WindowsAzure.ServiceRuntime;
    using System;
    using System.Diagnostics;
    using System.Diagnostics.Tracing;
    using System.Net;
    using System.Threading;

    namespace WorkerRole1
    {
    sealed class SampleEventSourceWriter : EventSource
    {
        public static SampleEventSourceWriter Log = new SampleEventSourceWriter();
        public void SendEnums(MyColor color, MyFlags flags) { if (IsEnabled())  WriteEvent(1, (int)color, (int)flags); }// Cast enums to int for efficient logging.
        public void MessageMethod(string Message) { if (IsEnabled())  WriteEvent(2, Message); }
        public void SetOther(bool flag, int myInt) { if (IsEnabled())  WriteEvent(3, flag, myInt); }
        public void HighFreq(int value) { if (IsEnabled()) WriteEvent(4, value); }

    }

    enum MyColor
    {
        Red,
        Blue,
        Green
    }

    [Flags]
    enum MyFlags
    {
        Flag1 = 1,
        Flag2 = 2,
        Flag3 = 4
    }

    public class WorkerRole : RoleEntryPoint
    {
        public override void Run()
        {
            // This is a sample worker implementation. Replace with your logic.
            Trace.TraceInformation("WorkerRole1 entry point called");

            int value = 0;

            while (true)
            {
                Thread.Sleep(10000);
                Trace.TraceInformation("Working");

                // Emit several events every time we go through the loop
                for (int i = 0; i < 6; i++)
                {
                    SampleEventSourceWriter.Log.SendEnums(MyColor.Blue, MyFlags.Flag2 | MyFlags.Flag3);
                }

                for (int i = 0; i < 3; i++)
                {
                    SampleEventSourceWriter.Log.MessageMethod("This is a message.");
                    SampleEventSourceWriter.Log.SetOther(true, 123456789);
                }

                if (value == int.MaxValue) value = 0;
                SampleEventSourceWriter.Log.HighFreq(value++);
            }
        }

        public override bool OnStart()
        {
            // Set the maximum number of concurrent connections
            ServicePointManager.DefaultConnectionLimit = 12;

            // For information on handling configuration changes
            // see the MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.

            return base.OnStart();
        }
    }
    }


### <a name="step-3-deploy-your-worker-role"></a>Krok 3: Nasazení pracovníka rolí
1.  Nasazení vaše role pracovníka Azure z aplikace Visual Studio tak, že vyberete **WadExample** projektu v Průzkumníku řešení pak **Publikovat** v nabídce **sestavení** .
2.  Vyberte předplatné.
3.  V dialogovém okně **Nastavení publikování Microsoft Azure** vyberte **Vytvořit nový …**.
4.  V dialogovém okně **vytvořit cloudové službě a úložišti účtu** zadejte **název** (například "WadExample") a vyberte oblast nebo spřažení skupinu.
5.  Nastavte **prostředí** **pracovní**.
6.  Upravte jakékoli další **Nastavení** a klikněte na **Publikovat**.
7.  Po dokončení nasazení ověřte Azure klasické portálu cloudové služby stavu **spuštěný** .

### <a name="step-4-create-your-diagnostics-configuration-file-and-install-the-extension"></a>Krok 4: Vytvoření konfiguračního souboru diagnostických nástrojů a nainstalujte rozšíření
1.  Definice schématu veřejného konfigurace soubor stáhněte spuštěním následujícího příkazu Powershellu:
2.
        (Get AzureServiceAvailableExtension - Název_rozšíření "PaaSDiagnostics" - ProviderNamespace "Microsoft.Azure.Diagnostics"). PublicConfigurationSchema | Odchozí souboru-kódování utf8 – cesta k souboru "WadConfig.xsd"

2.  Přidání souboru XML do projektu **WorkerRole1** kliknutím pravým tlačítkem myši na **WorkerRole1** projektu a vyberte **Přidat** -> **Novou položku...**  ->  **Visual Basic položky** -> **dat** -> **Soubor XML**. Zadejte název souboru "WadExample.xml".

    ![CloudServices_diag_add_xml](./media/cloud-services-dotnet-diagnostics/AddXmlFile.png)

3.  WadConfig.xsd přidružit konfiguračního souboru. Zkontrolujte, jestli že je okno editor WadExample.xml aktivní okno. Stisknutím klávesy **F4** otevřete okno **Vlastnosti** . Klikněte na vlastnost **schémata** v okně **Vlastnosti** . Kliknutím **** ve vlastnosti **schémat** . Klikněte **Přidat** tlačítko a přejděte do umístění pro uložení souboru XSD a vyberte požadovaný soubor WadConfig.xsd. Klikněte na **OK**.
4.  Obsah konfiguračního souboru WadExample.xml nahraďte následujícím XML a soubor uložte. Tento soubor konfigurace definuje pár výkonnosti shromažďování: jedno pro využití procesoru a jedno pro využití paměti. Po konfiguraci definuje čtyři události odpovídající metody ve třídě SampleEventSourceWriter.

```
        <?xml version="1.0" encoding="utf-8"?>
        <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
            <WadCfg>
                <DiagnosticMonitorConfiguration overallQuotaInMB="25000">
                <PerformanceCounters scheduledTransferPeriod="PT1M">
                    <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT1M" unit="percent" />
                    <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT1M" unit="bytes"/>
                    </PerformanceCounters>
                    <EtwProviders>
                        <EtwEventSourceProviderConfiguration provider="SampleEventSourceWriter" scheduledTransferPeriod="PT5M">
                            <Event id="1" eventDestination="EnumsTable"/>
                            <Event id="2" eventDestination="MessageTable"/>
                            <Event id="3" eventDestination="SetOtherTable"/>
                            <Event id="4" eventDestination="HighFreqTable"/>
                            <DefaultEvents eventDestination="DefaultTable" />
                        </EtwEventSourceProviderConfiguration>
                    </EtwProviders>
                </DiagnosticMonitorConfiguration>
            </WadCfg>
        </PublicConfig>
```

### <a name="step-5-install-diagnostics-on-your-worker-role"></a>Krok 5: Instalace diagnostiky na vaše Role kolegy
Rutiny prostředí PowerShell pro správu diagnostiky na webu nebo pracovního roli jsou: Set-AzureServiceDiagnosticsExtension Get-AzureServiceDiagnosticsExtension a AzureServiceDiagnosticsExtension odebrat.

1.  Otevřete Azure Powershellu.
2.  Spuštění skriptu nainstalovat diagnostiky na vaše role pracovníka (nahradit *StorageAccountKey* klíč účtu úložiště pro váš účet úložiště wadexample):

```
    $storage_name = "wadexample"
    $key = "<StorageAccountKey>"
    $config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExample\WorkerRole1\WadExample.xml"
    $service_name="wadexample"
    $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Staging -Role WorkerRole1
```

### <a name="step-6-look-at-your-telemetry-data"></a>Krok 6: Seznámení s telemetrickými daty
Ve Visual Studiu **Průzkumník serveru** přejděte k tomuto účtu úložiště wadexample. Po cloudu má byla spuštěná služba přibližně 5 minut byste měli vidět **WADEnumsTable**, **WADHighFreqTable** **WADMessageTable** **WADPerformanceCountersTable** a **WADSetOtherTable**tabulkami. Poklikejte na jednu z tabulky, které chcete zobrazit telemetrie shromážděná.
    ![CloudServices_diag_tables](./media/cloud-services-dotnet-diagnostics/WadExampleTables.png)


## <a name="configuration-file-schema"></a>Konfigurační soubor schématu

Konfigurační soubor diagnostiky definuje hodnoty, které se používají Inicializace diagnostiky nastavení při spuštění agenta diagnostických nástrojů. V části [nejnovější Přehled schématu](https://msdn.microsoft.com/library/azure/mt634524.aspx) platné hodnoty a příklady.

## <a name="troubleshooting"></a>Řešení potíží

Pokud máte potíže, naleznete v tématu [Poradce při potížích s diagnostiky Azure](../azure-diagnostics-troubleshooting.md) s běžných problémů.

## <a name="next-steps"></a>Další kroky
Změnit data, která shromažďujete, [projděte si seznam virtuálního počítače související články Azure Diagnostika](azure-diagnostics.md#cloud-services) problémů nebo Další informace o Diagnostika obecně.


[ZDROJ_UDÁLOSTI třídy]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Bezplatná zkušební verze]: http://azure.microsoft.com/pricing/free-trial/
[Instalace a konfigurace prostředí PowerShell Azure verze 0.8.7 nebo novější]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
