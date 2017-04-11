<properties
    pageTitle="Sledování tok v aplikaci služby cloudu pomocí Azure diagnostiky | Microsoft Azure"
    description="Přidání sledování zpráv do Azure aplikace pomůže ladění, měření výkonu, sledování, analýza provozu a další."
    services="cloud-services"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="02/20/2016"
    ms.author="robb"/>



# <a name="trace-the-flow-of-a-cloud-services-application-with-azure-diagnostics"></a>Sledování tok aplikace Cloudovým službám pomocí diagnostiky Azure

Sledování je způsob, jak můžete monitorovat spuštění aplikace je spuštěná. Použijte třídy [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx), [System.Diagnostics.Debug](https://msdn.microsoft.com/library/system.diagnostics.debug.aspx)a [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) uložit informace o chybách a spuštění aplikace v protokoly, soubory ve formátu RTF nebo jiných zařízeních pro pozdější analýzu. Další informace o sledování najdete v článku [trasování a nastavení aplikace](https://msdn.microsoft.com/library/zs6s4h68.aspx).


## <a name="use-trace-statements-and-trace-switches"></a>Použijte příkazy trasování a sledování přepínače

Implementace trasování v aplikaci Cloudovým službám přidáním [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) konfiguraci aplikace a volání System.Diagnostics.Trace nebo System.Diagnostics.Debug v kódu aplikace. Pomocí souboru konfigurace *app.config* pracovníka rolí a *web.config* pro web role. Při vytváření nové hostovanou službu pomocí šablony aplikace Visual Studio Azure diagnostiky automaticky přidají i do projektu a DiagnosticMonitorTraceListener se přidá do příslušné konfiguračního souboru pro role, které můžete přidat.

Další informace o umístění příkazy trasování, najdete v článku [jak: Přidání příkazy trasování kódu aplikace](https://msdn.microsoft.com/library/zd83saa2.aspx).

[Sledování přepínače](https://msdn.microsoft.com/library/3at424ac.aspx) v kódu tak, že umístíte můžete určit, zda dojde k sledování a jak rozsáhlé je. Toto oprávnění umožňuje sledovat stav vaší žádosti v pracovním prostředí. To je zvlášť důležité v podnikové aplikaci používající více součástí spuštěné ve více počítačích. Další informace najdete v tématu [jak: konfigurace přepínače sledování](https://msdn.microsoft.com/library/t06xyy08.aspx).

## <a name="configure-the-trace-listener-in-an-azure-application"></a>Konfigurace posluchače trasování v aplikaci Azure

Sledování ladění a TraceSource, je nutné nastavit "posluchače" můžete shromažďovat a záznam odesílaných zpráv. Posluchače shromáždit, ukládat a směrování sledování zpráv. Budou směrovat výstup trasování do příslušné cíle, například protokolu, okno nebo textový soubor. Azure diagnostiky používá třídu [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) .

Než se pustíte do následující postup, musíte inicializace Azure diagnostiky monitor. K tomuto účelu najdete v článku [Povolení Diagnostika v Microsoft Azure](cloud-services-dotnet-diagnostics.md).

Všimněte si, že pokud pomocí šablony, které jsou dostupné ve Visual Studiu konfigurace posluchače se automaticky přidá za vás.


### <a name="add-a-trace-listener"></a>Přidání posluchače trasování

1. Otevřete soubor web.config nebo app.config pro vaše role.
2. Přidejte následující kód k souboru. Změňte atribut verze používat číslo verze sestavení, na které odkazujete. Verze sestavení měňte nutně při každém vydání Azure SDK jsou aktualizace.

    ```
    <system.diagnostics>
        <trace>
            <listeners>
                <add type="Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener,
                  Microsoft.WindowsAzure.Diagnostics,
                  Version=2.8.0.0,
                  Culture=neutral,
                  PublicKeyToken=31bf3856ad364e35"
                  name="AzureDiagnostics">
                  <filter type="" />
                </add>
            </listeners>
        </trace>
    </system.diagnostics>
    ```
    >[AZURE.IMPORTANT] Zkontrolujte, jestli že máte odkaz na projekt Microsoft.WindowsAzure.Diagnostics sestavení. Číslo verze ve výše uvedené xml podle verzi odkazované sestavení Microsoft.WindowsAzure.Diagnostics aktualizujte.

3. Uložte soubor konfigurace.

Další informace o posluchače najdete v článku [Posluchače trasování](https://msdn.microsoft.com/library/4y5y10s7.aspx).

Po dokončení kroků přidání posluchače, můžete přidat příkazy trasování kódu.


### <a name="to-add-trace-statement-to-your-code"></a>Chcete-li přidat příkaz sledování kódu

1. Otevřete zdrojový soubor pro aplikace. Například <RoleName>cs souboru pracovní nebo web role.
2. Přidejte následující pomocí příkazu, pokud nebyl už přidali:
    ```
        using System.Diagnostics;
    ```
3. Přidáte místo, kam chcete zachytit informace o stavu aplikace příkazy trasování. Různé metody můžete použít k formátování výstupu příkaz sledování. Další informace najdete v tématu [jak: Přidání příkazy trasování kódu aplikace](https://msdn.microsoft.com/library/zd83saa2.aspx).
4. Zdrojový soubor uložte.
