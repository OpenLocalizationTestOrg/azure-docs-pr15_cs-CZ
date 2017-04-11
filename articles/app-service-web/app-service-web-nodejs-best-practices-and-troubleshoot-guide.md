<properties
    pageTitle="Poradce při potížích Příručka pro uzel aplikace na Azure Web Apps a osvědčené postupy"
    description="Přečtěte si doporučené postupy a řešení potíží pro uzel aplikace na Azure webové aplikace."
    services="app-service\web"
    documentationCenter="nodejs"
    authors="ranjithr"
    manager="wadeh"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="06/06/2016"
    ms.author="ranjithr;wadeh"/>
    
# <a name="best-practices-and-troubleshooting-guide-for-node-applications-on-azure-web-apps"></a>Poradce při potížích Příručka pro uzel aplikace na Azure Web Apps a osvědčené postupy

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

V tomto článku se dozvíte osvědčených postupů a řešení potíží pro [uzel aplikace](app-service-web-nodejs-get-started.md) spuštěné v Azure Webapps (plus pár [iisnode](https://github.com/azure/iisnode)).

>[AZURE.WARNING] Při použití řešení potíží na webu výrobní buďte opatrní. Doporučujeme Poradce při potížích s aplikace na testovacím nastavení například pracovní pozici a pokud je podařilo odstranit problém, záměna vaší pracovní pozici k výrobní pozici.

## <a name="iisnode-configuration"></a>Konfigurace IISNODE

Tento [soubor schématu](https://github.com/Azure/iisnode/blob/master/src/config/iisnode_schema_x64.xml) zobrazuje všechna nastavení, které můžete konfigurovat pro iisnode. Některá nastavení, která mohou být užitečné pro aplikaci jsou:

* nodeProcessCountPerApplication

    Tohle nastavení určuje počet uzel procesů spuštěné za aplikace služby IIS. Výchozí hodnota 1. Samostatné předplatné tolik node.exe jako svého počet základních OM nastavením to 0. Doporučené hodnotu 0 u většiny aplikací, můžete využít všechny jádra ve vašem počítači. Node.exe je jediný zřetězený tak jeden node.exe bude využívat maximálně 1 core a získat ze uzel aplikace budete chtít používat všechny vzorky maximální výkon.

* nodeProcessCommandLine

    Tohle nastavení určuje cestu k node.exe. Můžete nastavit tuto hodnotu tak, aby ukazovaly na node.exe verzi.

* maxConcurrentRequestsPerProcess

    Tohle nastavení určuje maximální počet souběžně prováděné žádosti odeslaný iisnode pro každou node.exe. Na azure webapps jako výchozí hodnota je nekonečno. Nemusíte bát, toto nastavení. Mimo azure webapps výchozí hodnota je 1 024. Můžete chtít nakonfigurovat podle toho, kolik žádosti, které že získá aplikace a jak rychle aplikace zpracuje každou žádost o.

* maxNamedPipeConnectionRetry

    Tohle nastavení určuje maximální počet iisnode zopakuje vytváření připojení na pojmenovaných kanálů odešlete žádost přes node.exe. Toto nastavení v kombinaci s namedPipeConnectionRetryDelay určuje celkové vypršení časového limitu každý požadavek v rámci iisnode. Výchozí hodnota je 200 na Azure Webapps. Celková vypršení časového limitu v sekundách = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000

* namedPipeConnectionRetryDelay

    Toto nastavení ovládacích prvků množství času ([ms) iisnode bude čekat mezi jednotlivými opakováními odešlete žádost o node.exe přes pojmenovaných kanálů. Výchozí hodnota je 250ms.
    Celková vypršení časového limitu v sekundách = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000

    Ve výchozím nastavení je celková časový limit v iisnode na azure webapps 200 \* 250ms = 50 sekund.

* logDirectory

    Tohle nastavení určuje v adresáři, kde bude iisnode protokolu stdout/stderr. Výchozí hodnota je iisnode, což je vzhledem k adresáři hlavní skript (kde je hlavní server.js prezentovat adresář)

* debuggerExtensionDll

    Tohle nastavení určuje, jakou verzi uzel inspektor iisnode použije při ladění uzel aplikace. Aktuálně iisnode inspektor 0.7.3.dll a iisnode inspector.dll jsou pouze 2 platné hodnoty pro toto nastavení. Výchozí hodnota je iisnode inspektor 0.7.3.dll. iisnode kontrola 0.7.3.dll verze používá uzel inspektor 0.7.3 a websockets, takže budete muset povolit websockets na vaše azure web appu používat tuto verzi. V tématu <http://www.ranjithr.com/?p=98> podrobné informace o tom, jak konfigurovat iisnode použít nové uzel kontrola.

* flushResponse

    Služby IIS se ve výchozím nastavení je vyrovnávací paměti data odpovědi nahoru 4 MB před vyčištění nebo až do konce odpověď, podle toho, co je v ní dřív. iisnode nabízí nastavení konfigurace přepsat toto chování: vyprázdnění fragment obsah entity odpověď hned iisnode dopis dostane z node.exe, budete muset nastavit iisnode/@flushResponse atribut web.config na hodnotu "true":
    
    ```
    <configuration>    
        <system.webServer>    
            <!-- ... -->    
            <iisnode flushResponse="true" />    
        </system.webServer>    
    </configuration>
    ```

    Povolení vyprázdnění každé část obsah entity odpověď přidá nároky na výkon, snižuje výkon systému ~ 5 % (od v0.1.13), je vhodné rozsah toto nastavení pouze pro koncové body, které vyžadují odpověď streaming (například pomocí <location> prvek web.config)

    Kromě toho datového proudu aplikací, musíte nastavit taky responseBufferLimit iisnode rutiny, aby 0.
    
    ```
    <handlers>    
        <add name="iisnode" path="app.js" verb="\*" modules="iisnode" responseBufferLimit="0"/>    
    </handlers>
    ```

* watchedFiles

    Toto je seznam oddělených středníkem souborů, které budou sledovány změny. Změnu do souboru způsobí, že aplikace do koše. Každá položka obsahuje název volitelné adresáře plus název požadovaný soubor, který se vzhledem k v adresáři, kde je uložena vstupní bod hlavní aplikace. V části název souboru pouze jsou povoleny zástupné znaky. Výchozí hodnota je "\*. js;web.config"

* recycleSignalEnabled

    Výchozí hodnota je false. Pokud je povoleno, aplikace uzel můžete připojit k pojmenovaných kanálů (proměnné IISNODE\_ovládací PRVEK\_kanálu) a pošlete zprávu "Koš". To způsobí w3wp do koše řádně.

* idlePageOutTimePeriod

    Výchozí hodnota je 0, což znamená, že tato funkce je zakázané. Při nastavena na hodnotu větší než 0, iisnode bude stránka se na všech jeho podřízených procesů každých "idlePageOutTimePeriod" milisekund. Pokud chcete zjistit, jaké stránky se označuje, získáte této [si přečtěte následující dokumentaci](https://msdn.microsoft.com/library/windows/desktop/ms682606.aspx). Toto nastavení bude užitečný pro aplikace, které využívat velké množství paměti a občas chcete pageout paměti na disk a uvolnit tak některé RAM.

>[AZURE.WARNING] Provádějte opatrně povolení následující nastavení konfigurace aplikace výroby. Doporučení je to není povolíte provozním aplikace.

* debugHeaderEnabled

    Výchozí hodnota je false. Nastavit na hodnotu true, iisnode přidá do každá odpověď z HTTP HTTP odpověď záhlaví iisnode ladění pošle hodnotu záhlaví iisnode ladění je adresy URL. Jednotlivé diagnostické informace lze gleaned pohledem na část adresy URL, ale mnohem lepší vizualizace nedosáhnete otevřením adresu URL v prohlížeči.

* loggingEnabled

    Tohle nastavení určuje protokolování stdout a stderr tak, že iisnode. Iisnode zachyťte stdout/stderr z uzel procesů, které se otevře a zapisovat do adresáře určené v nastavení "logDirectory". Po povolení aplikace vytváříte protokoly systému souborů a v závislosti na počtu protokolování provedenou aplikace, může mít vliv na výkon.

* devErrorsEnabled

    Výchozí hodnota je false. Když nastavena na hodnotu true, iisnode zobrazí HTTP stavový kód a kód chyby Win32 v prohlížeči. Kód win32 bude užitečný ladění určité typy potíží.
    
* debuggingEnabled (ne povolit na webu provozním)

    Tohle nastavení určuje funkce ladění. Iisnode je integrována s uzel Kontrola metadat. Povolením toto nastavení povolíte ladění uzel aplikace. Po povolení toto nastavení bude iisnode rozložení potřebné uzel inspektor soubory v adresáři "debuggerVirtualDir" první ladění žádosti o aplikaci uzel. Načtení inspektor uzel tak, že žádost o http://yoursite/server.js/debug. Adresa URL segmentu ladění můžete ovládat pomocí nastavení "debuggerPathSegment". Ve výchozím nastavení debuggerPathSegment = "ladění". Nastavíte takto GUID například tak, aby je obtížnější zjistit ostatní uživatelé.

    Zaškrtněte políčko Tento [odkaz](https://tomasz.janczuk.org/2011/11/debug-nodejs-applications-on-windows.html) podrobné informace o ladění.

## <a name="scenarios-and-recommendationstroubleshooting"></a>Scénáře a doporučení/Poradce při potížích s

### <a name="my-node-application-is-making-too-many-outbound-calls"></a>Aplikace uzel vyvíjí příliš mnoho odchozí hovory.

Řada aplikací by mají být odchozí připojení jako součást pravidelné operaci. Třeba při příchodu žádost o aplikaci uzel by chcete kontaktovat rozhraní REST API kdekoli jinde a určité informace ke zpracování požadavku. Je vhodné použít zůstat aktivní agent při volání http nebo https. Provedení těchto odchozí hovory může například pomocí modulu agentkeepalive jako vaše zástupce zůstat aktivní. Zajistíte tím, že sockets jsou znova použít na svůj web appu azure OM a snížení režijních vytváření nových sockets pro každého odchozího požadavku. Navíc tím zajistíte, že používáte menší počet sockets provést mnoho žádosti o odchozí připojení, a proto není překročit maxSockets, které jsou přidělený na OM. Doporučení ohledně Azure Webapps bude hodnota agentKeepAlive maxSockets celkových 160 sockets za OM. To znamená, že máte 4 node.exe spuštěných OM by chcete nastavit agentKeepAlive maxSockets 40 na node.exe což je 160 celkem za OM.

Příklad agentKeepALive konfigurace:

```
var keepaliveAgent = new Agent({    
    maxSockets: 40,    
    maxFreeSockets: 10,    
    timeout: 60000,    
    keepAliveTimeout: 300000    
});
```

V tomto příkladu předpokládá, že máte 4 node.exe spuštěných pro vaše OM. Pokud máte rozdílný počet node.exe spuštěných OM, budete muset změnit maxSockets příslušným způsobem nastavení.

### <a name="my-node-application-is-consuming-too-much-cpu"></a>Aplikace uzel spotřebovává příliš mnoho procesoru.

Bude pravděpodobně dostanete doporučení z Azure Webapps na portálu o vysoké spotřebu procesoru. Můžete také nastavit monitory sledovat pro určité [metriky](web-sites-monitor.md). Při kontrole využití procesoru na [Řídicí panel portál Azure](../application-insights/app-insights-web-monitor-performance.md), zkontrolujte maximální hodnoty pro procesor abyste nezmeškali mimo maximální hodnoty.
V případech, kdy by podle vás aplikace spotřebovává příliš mnoho procesoru a nelze vysvětlením musíte se profilu aplikace uzel.

### 

#### <a name="profiling-your-node-application-on-azure-webapps-with-v8-profiler"></a>Vytvoření profilu aplikace uzel na azure webapps pomocí nástroje v8:

Například přivítejte vám umožní být aplikace Ahoj prezentace, kterou chcete profilu, jak je ukázáno v následujícím příkladu:

```
var http = require('http');    
function WriteConsoleLog() {    
    for(var i=0;i<99999;++i) {    
        console.log('hello world');    
    }    
}

function HandleRequest() {    
    WriteConsoleLog();    
}

http.createServer(function (req, res) {    
    res.writeHead(200, {'Content-Type': 'text/html'});    
    HandleRequest();    
    res.end('Hello world!');    
}).listen(process.env.PORT);
```

Přejděte na svůj web https://yoursite.scm.azurewebsites.net/DebugConsole Správce služeb

Zobrazí se příkazový řádek a jak je ukázáno v následujícím příkladu. Přejděte do adresáře webu/kořenových

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_install_v8.png)

Spuštění příkazu "npm instalace nástroje v8:"

To měli nainstalovat v8: nástroje uzlu\_moduly adresář a závislými.
Teď server.js do profilu aplikace.

```
var http = require('http');    
var profiler = require('v8-profiler');    
var fs = require('fs');

function WriteConsoleLog() {    
    for(var i=0;i<99999;++i) {    
        console.log('hello world');    
    }    
}

function HandleRequest() {    
    profiler.startProfiling('HandleRequest');    
    WriteConsoleLog();    
    fs.writeFileSync('profile.cpuprofile', JSON.stringify(profiler.stopProfiling('HandleRequest')));    
}

http.createServer(function (req, res) {    
    res.writeHead(200, {'Content-Type': 'text/html'});    
    HandleRequest();    
    res.end('Hello world!');    
}).listen(process.env.PORT);
```

Výše uvedené změny budou profilu funkci WriteConsoleLog a pak napište výstupu profilu "profile.cpuprofile" soubor v části kořenových webů. Pošlete žádost o aplikaci. Zobrazí se spojkou "profile.cpuprofile" soubor vytvořený v části kořenových webů.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_profile.cpuprofile.png)

Stáhněte si tento soubor a budete muset tento soubor otevřít pomocí nástrojů F12 Chrome. Dopad F12 stylu webové části a potom klikněte na kartu"profily". Klikněte na tlačítko "Zatížení". Vyberte profile.cpuprofile soubor, který jste právě stáhli. Klikněte na profil, který jste právě zavedli.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/chrome_tools_view.png)

Zobrazí se, že 95 % času byla využívané WriteConsoleLog funkce jak je ukázáno v následujícím příkladu. To taky vidíte výška řádku čísel a zdrojové soubory, které způsobují problém.

### <a name="my-node-application-is-consuming-too-much-memory"></a>Aplikace uzel spotřebovává příliš mnoho paměti.

Bude pravděpodobně dostanete doporučení z Azure Webapps na portálu o spotřebu paměti vysoká. Můžete také nastavit monitory sledovat pro určité [metriky](web-sites-monitor.md). Při kontrole spotřebu paměti na [Řídicí panel portálu Azure](../application-insights/app-insights-web-monitor-performance.md), zkontrolujte maximální hodnoty pro paměti, abyste nezmeškali mimo maximální hodnoty.

#### <a name="leak-detection-and-heap-diffing-for-nodejs"></a>Nevrací zjišťování a haldy Diffing pro node.js 

Můžete použít [uzel memwatch](https://github.com/lloyd/node-memwatch) vám pomůže identifikovat nevrací paměti.
Instalace memwatch stejně jako v8: nástroje a upravit kód zachycení a změn haldách k identifikaci paměť nevrací v aplikaci.

### <a name="my-nodeexes-are-getting-killed-randomly"></a>Moje node.exe jsou začíná ukončit náhodně 

Existuje několik důvodů, proč to může děje se:

1.  Aplikace je aktivační nezachycené výjimky – prosím zaškrtnutí d:\\domácí\\LogFiles\\aplikace\\protokolování errors.txt soubor informace na výjimku. Tento soubor obsahuje zásobníku, takže můžete opravit aplikace založené na to.

2.  Aplikace je využití příliš mnoho paměti, která má vliv na ostatní procesy Začínáme. Pokud celkové paměti OM je tomu vašemu nejbližší 100 %, může vaše node.exe ukončit správcem proces chcete, aby ostatní procesů ještě udělat několik práce. Pokud to pokud chcete opravit, zkontrolujte, jestli že vaše aplikace není nevrací paměť nebo pokud aplikaci skutečně potřebujete-li použít spoustu paměti, přejděte prosím škálování větší angličtině se jich RAM.

### <a name="my-node-application-does-not-start"></a>Mé uzel aplikace se nespustí

Pokud aplikace vrací 500 chyby při spuštění, může být z několika důvodů:

1.  Node.exe není správného umístění prezentovat. Zkontrolujte nastavení nodeProcessCommandLine.

2.  Soubor skriptu v hlavním není na správné místo. Zaškrtněte políčko web.config a ujistěte se, že název souboru hlavní skriptu v části popisovače odpovídá soubor hlavního skriptu.

3.  Konfigurace Web.config není správného – zkontrolujte nastavení názvy/hodnoty.

4.  Studený Start – aplikace trvá příliš dlouho spuštění. Pokud aplikace trvá déle než (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000 sekund iisnode vrátí 500 Chyba. Zvětšení hodnot podle času zahájení aplikace nechcete, aby iisnode z vypršel časový limit a vrácení 500 Chyba tato nastavení.

### <a name="my-node-application-crashed"></a>Aplikace uzel havaruje.

Aplikace je aktivační nezachycené výjimky – prosím zaškrtnutí d:\\domácí\\LogFiles\\aplikace\\protokolování errors.txt soubor informace na výjimku. Tento soubor obsahuje zásobníku, takže můžete opravit aplikace založené na to.

### <a name="my-node-application-takes-too-much-time-to-startup-cold-start"></a>Aplikace uzel trvá příliš dlouho spuštění (studenou zahájení)

Nejběžnější to proto, že má aplikace velké množství souborů v uzlu\_moduly a aplikace pokusí tyto soubory většinou načítalo při spuštění. Ve výchozím nastavení protože vaše soubory jsou umístěny na sdílené síťové složky na Azure Webapps načítání tolik souborů může nějakou dobu trvat.
Řešení některých urychlit to jsou:

1.  Zkontrolujte, jestli že máte ploché závislost struktury a žádné duplicitní závislosti pomocí npm3 nainstalovat moduly.

2.  Pokuste se pustí načíst uzel\_moduly a nenačte všechny moduly při spuštění. To znamená, že volání require('module') chcete provést potřebujete-li ve skutečnosti ho ve funkci se pokusíte použít modulu.

3.  Azure Webapps nabízí funkci místní mezipaměti. Tato funkce slouží ke kopírování obsahu ze sdílené síťové složky na místním disku v OM. Protože soubory, které jsou místní, doby načítání uzlu\_moduly je rychleji. -Tento si přečtěte následující [dokumentaci](../app-service/app-service-local-cache.md) vysvětluje, jak použít místní mezipaměti podrobněji.

## <a name="iisnode-http-status-and-substatus"></a>Stav protokolu http IISNODE a podřízeného stavu

[Zdrojový soubor](https://github.com/Azure/iisnode/blob/master/src/iisnode/cnodeconstants.h) obsahuje seznam všech zaručuje iisnode kombinaci možné stav/podřízeného stavu v případě chyby.

Povolení FREB aplikace zobrazíte kód chyby win32 (zkontrolujte, jestli povolíte FREB pouze na webech testovacím důvodů výkonu).

| Stav protokolu HTTP | Dílčí stav HTTP | Důvodem?                                                                                                                                                                                            
|-------------|----------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------
| 500         | 1 000           | Došlo nějaký problém odesílání žádost IISNODE – zjistěte, pokud byla node.exe spuštění. Node.exe může mít havaruje při spuštění. Kontrola konfigurace web.config chyby.                                                                                                                                                                                                                                                                                     |
| 500         | 1001           | -Win32Error 0x2 - aplikace neodpovídá na adresu URL. Zkontrolujte adresu URL přepisu pravidla nebo pokud express aplikace má správné trasy definované. -Teď na něčem pracuje Win32Error 0x6d – pojmenovaných kanálů – Node.exe nepřijímá požadavky, protože kanálu teď na něčem pracuje. Zaškrtněte políčko vysoký využití procesoru. -Jiné chyby – zaškrtněte, pokud node.exe havaruje.
| 500         | 1002           | Node.exe havaruje – zjistěte d:\\domácí\\LogFiles\\errors.txt protokolování pro zásobníku.                                                                                                                                                                                                                                                                                                                                                                                        |
| 500         | 1003           | Kanálu konfigurační problém – nikdy byste měli vidět to ale když uděláte, je nesprávná konfigurace pojmenovaných kanálů.                                                                                                                                                                                                                                                                                                                                                          |
| 500         | 1004-1018      | Při odesílání žádosti a zpracování odpovědi z node.exe došlo některé chyby. Zkontrolujte, zda node.exe havaruje. Kontrola d:\\domácí\\LogFiles\\errors.txt protokolování pro zásobníku.                                                                                                                                                                                                                                                                                    |
| 503         | 1 000           | Nedostatku paměti více pojmenovaných připojení kanálu. Proč aplikace spotřebovává tolik paměti zaškrtnutí. Zkontrolujte hodnoty nastavení maxConcurrentRequestsPerProcess. Pokud nelze nekonečno a můžete je značné množství požadavky, zvýšit tuto hodnotu k této chybě zabránit.                                                                                                                                                                                                                                                                                                                  |
| 503         | 1001           | Žádost o nebyla odeslána do node.exe, protože recyklace aplikace. Po má recykloval aplikace, žádosti o by měl být podávané množství běžným způsobem.                                                                                                                                                                                                                                                                                                               |
| 503         | 1002           | Kód chyby win32 zaškrtnutí důvodu skutečné – žádost o nebyla odeslána do node.exe.                                                                                                                                                                                                                                                                                                                                                                               |
| 503         | 1003           | Pojmenovaný kanál je přeplněné – zjistěte, pokud uzel spotřebovává spoustu procesoru                                                                                                                                                                                                                                                                                                                                                                                                        
                                                                                                                                                                                                                                                                                                            
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         
Nastavení v rámci NODE.exe s názvem uzel\_čekání\_kanálu\_instance. Ve výchozím nastavení mimo azure webapps tato hodnota je 4. To znamená, že node.exe můžete jenom přijímaní žádostí 4 současně na pojmenovaných kanálů. Na Azure Webapps je tato hodnota nastavená na 5000 a tato hodnota musí být to nestačí pro většinu uzel spuštěné v azure webapps. Neměli 503.1003 na uvidíte azure webapps protože máme vysoká hodnota pro uzel\_čekání\_kanálu\_instance.  |

## <a name="more-resources"></a>Další materiály

Tyto odkazy vedou na další informace o aplikacích node.js v aplikaci služby Azure.

* [Začínáme s Node.js web apps v aplikaci služby Azure](app-service-web-nodejs-get-started.md)
* [Ladění Node.js web app v aplikaci služby Azure](web-sites-nodejs-debug.md)
* [Používání Node.js moduly s Azure aplikacemi](../nodejs-use-node-modules-azure-apps.md)
* [Služba Azure aplikací Web Apps: Node.js](https://blogs.msdn.microsoft.com/silverlining/2012/06/14/windows-azure-websites-node-js/)
* [Středisko pro vývojáře Node.js](../nodejs-use-node-modules-azure-apps.md)
* [Prozkoumání konzoly ladění velmi skryté Kudu](https://azure.microsoft.com/documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/)