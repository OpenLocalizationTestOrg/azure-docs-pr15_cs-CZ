<properties 
    pageTitle="Konzoly a datových proudů protokolování" 
    description="Přehled konzoly a datových proudů protokolování" 
    authors="btardif" 
    manager="wpickett" 
    editor="" 
    services="app-service\web" 
    documentationCenter=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="10/12/2016" 
    ms.author="byvinyal"/>

# <a name="streaming-logs-and-the-console"></a>Konzole a datových proudů protokolování

## <a name="streaming-logs"></a>Streamování protokoly

**Azure portál** poskytuje integrované streamování protokolu prohlížeč formátu, který umožňuje zobrazit trasování událostí z **Aplikace služeb** aplikací v reálném čase.  

Nastavte si tato funkce vyžaduje několika jednoduchými kroky:

- Psaní trasování v kódu
- Povolení aplikace **protokoly pro diagnostiku** aplikace
- Zobrazte tok z předdefinovaných uživatelského rozhraní **Datových proudů protokoly** **Azure portálu**.

### <a name="how-to-write-traces-in-your-code"></a>Jak psát trasování v kódu ###

Psaní trasování v kódu je snadné.  V jazyce C# je stejně snadná jako psaní následující kód:

`````````````````````````
Trace.TraceInformation("My trace statement");
`````````````````````````

`````````````````````````
Trace.TraceWarning("My warning statement");
`````````````````````````

`````````````````````````
Trace.TraceError("My error statement");
`````````````````````````

Sledování třídy jsou umístěná v oboru System.Diagnostics.

V aplikaci node.js můžete napsat tento kód dosáhnout stejný výsledek:

`````````````````````````
console.log("My trace statement").
`````````````````````````

### <a name="how-to-enable-and-view-the-streaming-logs"></a>Jak povolit a zobrazení streamování protokoly
![][BrowseSitesScreenshot]Diagnostika jsou povolené na základě na aplikace. Začněte tím, že přejdete na web chcete tuto funkci povolit na.  
  
![][DiagnosticsLogs]V nabídce Nastavení přejděte dolů do části **Sledování** a klikněte na **Protokoly pro diagnostiku (1)**. Potom **(2) povolit** **Protokolování (systém souborů) aplikace** nebo **Aplikace protokolování (blob)** možnost **úroveň** umožňuje změnit úroveň závažnosti trasování k zaznamenání. Pokud zkoušíte jenom se seznámíte s funkcí, nastavte úroveň na **podrobné** zajistit, aby že všechny příkazy trasování odebírají.

Klikněte na tlačítko **Uložit** v horní části zásuvné a budete chtít zobrazit protokoly.

>[AZURE.NOTE] Čím je vyrobeno požadovanou **úroveň závažnosti** další zdroje jsou spotřebované množství do protokolu a další trasování. Zkontrolujte, že **úroveň závažnosti** nakonfigurovaný tak, aby správné podrobností výrobní nebo vysoký provoz webu. 

![][StreamingLogsScreenshot]**Datových proudů protokoly** z v Azure portálu zobrazíte klikněte na **(1) toku protokolu** také v části **Sledování** nabídky nastavení. Pokud aplikace aktivně zapisuje příkazy trasování, měli je vidět v **(2) streamování protokoly uživatelského rozhraní** do blízké reálném čase.

## <a name="console"></a>Konzoly
**Azure portál** poskytuje konzoly přístup k aplikaci. Můžete prozkoumat systém souborů vaše aplikace a spuštění skriptů powershellu/cmd. Jsou vázaná stejné oprávněními jako spuštění aplikace kódu při provádění konzoly příkazů. Přístup k chráněné adresáře nebo spouštění skriptů, které vyžadují zvýšenými oprávněními je blokován.  

![][ConsoleScreenshot]V nabídce nastavení **(2) konzoly** uživatelského rozhraní a posuňte se dolů k oddílu **Vývojového nástroje** a klikněte na **(1) konzoly** kartu se otevře napravo.

Kde se seznámíte s **konzoly**, zkuste základní příkazům typu:

`````````````````````````
dir
`````````````````````````

`````````````````````````
cd
`````````````````````````

<!-- Images. -->
[DiagnosticsLogs]: ./media/web-sites-streaming-logs-and-console/diagnostic-logs.png
[BrowseSitesScreenshot]: ./media/web-sites-streaming-logs-and-console/browse-sites.png
[StreamingLogsScreenshot]: ./media/web-sites-streaming-logs-and-console/streaming-logs.png
[ConsoleScreenshot]: ./media/web-sites-streaming-logs-and-console/console.png
