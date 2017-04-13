<properties
    pageTitle="Jak používat Azure diagnostiky ve virtuálních počítačích | Microsoft Azure"
    description="Použití Azure Diagnostika pro shromažďování dat z Azure virtuálních počítačích ladění, měření výkonu, sledování, analýza provozu a další."
    services="virtual-machines"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="virtual-machines"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="02/20/2016"
    ms.author="robb"/>



# <a name="enabling-diagnostics-in-azure-virtual-machines"></a>Povolení Diagnostika v Azure virtuálních počítačích

Pozadí na Diagnostika Azure naleznete v tématu [Přehled diagnostiky Azure](azure-diagnostics.md) .

## <a name="how-to-enable-diagnostics-in-a-virtual-machine"></a>Jak povolit Diagnostika v virtuálního počítače

Tento procházení popisuje, jak vzdáleně nainstalovat diagnostiky Azure virtuálního počítače z vývojového počítače. Také se naučíte, jak implementovat aplikace, která běží na Azure virtuální počítače a posílá telemetrickými daty pomocí.NET [Zdroj_události předmětu][]. Azure Diagnostika umožňuje shromáždit telemetrie a uložte ho do účet Azure úložiště.

### <a name="pre-requisites"></a>Předpoklady
Tento procházení předpokládá máte předplatné Azure a, kteří používají aplikaci Visual Studio 2013 s Azure SDK. Pokud nemáte předplatné Azure, můžete zaregistrovat [Bezplatnou zkušební verzi][]. Zkontrolujte, že [instalace a konfigurace prostředí PowerShell Azure verze 0.8.7 nebo novější][].

### <a name="step-1-create-a-virtual-machine"></a>Krok 1: Vytvoření virtuálního počítače
1.  V počítači vývoj spuštění aplikace Visual Studio 2013.
2.  Ve Visual Studiu **Průzkumník serveru** rozbalte uzel **Azure**, klikněte pravým tlačítkem na **virtuálních počítačích** a vyberte **vytvořit virtuální počítač**.
3.  V dialogovém okně **Zvolte předplatné** vyberte předplatné Azure a klikněte na tlačítko **Další**.
4.  Vyberte **Windows serveru 2012 R2 datacentru, listopadu 2014** v dialogovém okně **Vybrat obrázek virtuálního počítače** a klikněte na tlačítko **Další**.
5.  V části **Základní nastavení**nastavte názvu virtuálního počítače "wadexample". Nastavení správce uživatelské jméno a heslo a klikněte na tlačítko **Další**.
6.  V dialogovém okně **Nastavení služby cloudu** vytvořte nové Cloudová služba s názvem "wadexampleVM". Vytvoření nového účtu úložiště s názvem "wadexample" a klikněte na tlačítko **Další**.
7.  Klikněte na **vytvořit**.

### <a name="step-2-create-your-application"></a>Krok 2: Vytvoření aplikace
1.  V počítači vývoj spuštění aplikace Visual Studio 2013.
2.  Vytvoření nové vizuální C# konzoly aplikace, která používá .NET Framework 4.5. Zadejte název projektu "WadExampleVM".
    ![CloudServices_diag_new_project](./media/virtual-machines-dotnet-diagnostics/NewProject.png)
3.  Nahraďte obsah Program.cs následující kód. Třídy **SampleEventSourceWriter** implementuje čtyři metody protokolování: **SendEnums** **MessageMethod**, **SetOther** a **HighFreq**. První parametr metodu WriteEvent definuje ID odpovídajících události. Metoda spustit provádí nekonečné smyčky, volající všech metody protokolování implementovaná ve třídě **SampleEventSourceWriter** každých 10 sekund.

        using System;
        using System.Diagnostics;
        using System.Diagnostics.Tracing;
        using System.Threading;

        namespace WadExampleVM
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

        class Program
        {
        static void Main(string[] args)
        {
            Trace.TraceInformation("My application entry point called");

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
        }
        }


4.  Uložte soubor a vyberte z nabídky **sestavení** vytvářet kódu sestavit **Řešení** .


### <a name="step-3-deploy-your-application"></a>Krok 3: Nasazení aplikace
1.  Klikněte pravým tlačítkem myši na **WadExampleVM** projektu v **Průzkumníku řešení** a zvolte **Otevřít složku v Průzkumníku souborů**.
2.  Přejděte do složky *bin\Debug* a zkopírujte všechny soubory (WadExampleVM.*)
3.  V okně **Průzkumník serveru** klikněte pravým tlačítkem myši na virtuální počítač a klikněte na **Připojit pomocí vzdálené plochy**.
4.  Po připojení k OM vytvořte složku s názvem WadExampleVM a Vložit aplikace soubory do složky.
5.  Spusťte aplikaci WadExampleVM.exe. Měli byste vidět okna prázdné konzoly.

### <a name="step-4-create-your-diagnostics-configuration-and-install-the-extension"></a>Krok 4: Vytvoření konfiguraci diagnostických nástrojů a nainstalujte rozšíření
1.  Stáhněte si definice schématu veřejného konfigurace soubor do počítače vývoj spuštěním následujícího příkazu Powershellu:

        (Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File -Encoding utf8 -FilePath 'WadConfig.xsd'

2.  Otevřete nový soubor XML ve Visual Studiu, buď v projektu již máte otevřít nebo ve Visual Studiu instance se žádné otevřených projektů. Ve Visual Studiu, vyberte **Přidat** -> **Novou položku...**  ->  **Visual Basic položky** -> **dat** -> **Soubor XML**. Zadejte název souboru "WadExample.xml"
3.  WadConfig.xsd přidružit konfiguračního souboru. Zkontrolujte, jestli že je okno editor WadExample.xml aktivní okno. Stisknutím klávesy **F4** otevřete okno **Vlastnosti** . Klikněte na vlastnost **schémata** v okně **Vlastnosti** . Kliknutím **** ve vlastnosti **schémat** . Klikněte **Přidat** tlačítko a přejděte do umístění pro uložení souboru XSD a vyberte požadovaný soubor WadConfig.xsd. Klikněte na **OK**.
4.  Obsah konfiguračního souboru WadExample.xml nahraďte následujícím XML a soubor uložte. Tento soubor konfigurace definuje pár výkonnosti shromažďovat: jedno pro využití procesoru a jedno pro využití paměti. Po konfiguraci definuje čtyři události odpovídající metody ve třídě SampleEventSourceWriter.

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

### <a name="step-5-remotely-install-diagnostics-on-your-azure-virtual-machine"></a>Krok 5: Vzdáleně nainstalujte Diagnostika v počítači virtuální Azure
Rutiny prostředí PowerShell pro správu diagnostiku virtuálního počítače se: Set-AzureVMDiagnosticsExtension Get-AzureVMDiagnosticsExtension a AzureVMDiagnosticsExtension odebrat.

1.  V počítači vývojář otevřete Azure Powershellu.
2.  Spuštění skriptu vzdáleně nainstalovat diagnostiky na OM (nahradit *StorageAccountKey* s klíč účtu úložiště pro váš účet úložiště wadexamplevm):

        $storage_name = "wadexamplevm"
        $key = "<StorageAccountKey>"
        $config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExampleVM\WadExampleVM\WadExample.xml"
        $service_name="wadexamplevm"
        $vm_name="WadExample"
        $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
        $VM1 = Get-AzureVM -ServiceName $service_name -Name $vm_name
        $VM2 = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $config_path -Version "1.*" -VM $VM1 -StorageContext $storageContext
        $VM3 = Update-AzureVM -ServiceName $service_name -Name $vm_name -VM $VM2.VM


### <a name="step-6-look-at-your-telemetry-data"></a>Krok 6: Seznámení s telemetrickými daty
Ve Visual Studiu **Průzkumník serveru** přejděte k tomuto účtu úložiště wadexample. Po OM spuštěna přibližně 5 minut byste měli vidět **WADEnumsTable** **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** a **WADSetOtherTable**tabulkami. Poklikejte na jednu z tabulky, které chcete zobrazit telemetrie shromážděná.

![CloudServices_diag_wadexamplevm_tables](./media/virtual-machines-dotnet-diagnostics/WadExampleVMTables.png)

## <a name="configuration-file-schema"></a>Konfigurační soubor schématu

Konfigurační soubor diagnostiky definuje hodnoty, které se používají Inicializace diagnostiky nastavení při spuštění agenta diagnostických nástrojů. V části [nejnovější Přehled schématu](https://msdn.microsoft.com/library/azure/mt634524.aspx) platné hodnoty a příklady.

## <a name="troubleshooting"></a>Řešení potíží

Další informace najdete v článku [Poradce při potížích s diagnostiky Azure](azure-diagnostics-troubleshooting.md) .


## <a name="next-steps"></a>Další kroky
Změnit data, která shromažďujete, [projděte si seznam virtuálního počítače související články Azure Diagnostika](azure-diagnostics.md#virtual-machines-using-azure-diagnostics) problémů nebo Další informace o diagnostiky obecně.


[ZDROJ_UDÁLOSTI třídy]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Bezplatná zkušební verze]: http://azure.microsoft.com/pricing/free-trial/
[Instalace a konfigurace prostředí PowerShell Azure verze 0.8.7 nebo novější]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
