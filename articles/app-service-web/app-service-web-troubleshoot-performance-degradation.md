<properties
    pageTitle="Výkon webové aplikace v aplikaci služby | Microsoft Azure"
    description="Tento článek pomáhá pomalé webové aplikace řešení problémů s výkonem v aplikaci služby Azure."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
    tags="top-support-issue"
    keywords="web app výkon pomalé aplikace aplikace pomalé"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="cephalin"/>

# <a name="troubleshoot-slow-web-app-performance-issues-in-azure-app-service"></a>Pomalé webové aplikace řešení problémů s výkonem v aplikaci služby Azure

Tento článek pomáhá pomalé webové aplikace řešení problémů s výkonem v [Aplikaci služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714).

Pokud potřebujete další pomoc kdykoli v tomto článku můžete kontaktovat Azure odborníků na [webu MSDN Azure a fórech přetečení zásobníku](https://azure.microsoft.com/support/forums/). Můžete taky můžete poslat taky případ Azure podpory. Přejděte na [Web podpory Azure](https://azure.microsoft.com/support/options/) a klikněte na **Získat podporu**.

## <a name="symptom"></a>Příznaku

Během prohlížení webové aplikace načítání stránek pomalu a někdy časový limit.

## <a name="cause"></a>Příčina

Tento problém je často způsobená úrovně problémy s aplikací, například:

-   požadavky na trvá moc dlouho.
-   aplikace pomocí vysoké paměti/procesoru
-   aplikace k selhání kvůli výjimce.

## <a name="troubleshooting-steps"></a>Návody na řešení potíží

Poradce při potížích s můžete rozdělit na tři samostatné úkoly v pořadí:

1.  [Sledovat a monitorovat chování aplikace](#observe)
2.  [Shromažďování dat](#collect)
3.  [Zmírnění problém](#mitigate)

[Aplikace služby Web Apps](/services/app-service/web/) nabízí různé možnosti při každém kroku.

<a name="observe" />
### <a name="1-observe-and-monitor-application-behavior"></a>1. sledovat a monitorovat chování aplikace

#### <a name="track-service-health"></a>Sledovat stav služby

Microsoft Azure publicizes pokaždé, když je přerušení nebo výkonu snížení úrovně služeb. Sledovat stav služby na [Portálu Azure](https://portal.azure.com/). Další informace najdete v tématu [sledovat stav služby](../monitoring-and-diagnostics/insights-service-health.md).

#### <a name="monitor-your-web-app"></a>Sledovat web appu

Tato možnost umožňuje zjistit, pokud jakékoli problémy s aplikace. Na zásuvné webovou aplikaci klikněte na dlaždici **požadavky a chyby** . **Metriky** zásuvné řádku se zobrazí všechny metriky, které můžete přidat.

Některé z metriky, které můžete chtít sledovat pro webovou aplikaci

-   Průměrná pracovní sada paměti
-   Průměrná doba odezvy
-   Procesor času
-   Pracovní sada paměti
-   Požadavky

![sledovat výkon webové aplikace](./media/app-service-web-troubleshoot-performance-degradation/1-monitor-metrics.png)

Další informace najdete v tématu:

-   [Sledování aplikací webové služby Azure aplikace](web-sites-monitor.md)
-   [Přijímání oznámení](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

#### <a name="monitor-web-endpoint-status"></a>Sledování stavu koncový bod web

Pokud používáte webové aplikace ve **Standardní** ceny osy, Web Apps můžete sledovat koncové body 2 ze 3 zeměpisné lokality.

Sledování koncový bod nakonfiguruje webových testů z geo distributed umístění, které otestujte doby odezvy a provozu adresy URL webových. Test provádí operace HTTP GET na adresu URL k určení doby odezvy a provozu každé umístění. Každý nakonfigurovaného umístění spustí testu každých pět minut.

Provozu je sledovat pomocí kódů odpovědí HTTP a měří se doby odezvy v milisekundách. Sledování test nezdaří, pokud kód odpovědi HTTP je větší než nebo rovno 400 nebo odpověď na trvá déle než 30 sekund. Koncový bod se považuje za k dispozici, pokud jeho sledovací testy úspěšné ze zadaných umístění.

Nastavit, najdete v článku [jak: sledování stavu koncový bod web](web-sites-monitor.md#webendpointstatus).

Další informace naleznete [uchovávání Azure weby nahoru plus sledování koncového bodu - se vám Stefanova situace Schackow](/documentation/videos/azure-web-sites-endpoint-monitoring-and-staying-up/) video o sledování koncového bodu.

#### <a name="application-performance-monitoring-using-extensions"></a>Sledování výkonu aplikace pomocí rozšíření

Taky můžete sledovat výkon aplikace využívání _rozšíření webů_.

V prohlížeči každé aplikaci služby obsahuje extensible správy koncový bod, který umožňuje využívat výkonných sadu nástrojů nasazené jako rozšíření webů. Tyto nástroje sahají od zdrojového kódu editoři jako [Visual Studio týmovou](https://www.visualstudio.com/products/what-is-visual-studio-online-vs.aspx) s nástroji pro správu pro připojení zdroje, jako jsou databáze MySQL připojené k web appu.

[Přehledy Azure aplikace](/services/application-insights/) a [Novou Relic](/marketplace/partners/newrelic/newrelic/) jsou dvě rozšíření webů, které jsou k dispozici pro sledování výkonu. Pokud chcete použít nový Relic, nainstalujete agent za běhu. Pokud chcete použít přehledy aplikace Azure, znovu kódu SDK a můžete taky nainstalovat rozšíření, které poskytuje přístup k další data. V SDK umožňuje kódu sledování použití a výkonu aplikace podrobněji.

Použití aplikace přehledy, najdete v článku [sledovat výkon ve webových aplikacích](../application-insights/app-insights-web-monitor-performance.md).

Pokud chcete použít nový Relic, najdete v tématu [Nové Relic aplikace výkonu správy na Azure](../store-new-relic-cloud-services-dotnet-application-performance-management.md).

<a name="collect" />
### <a name="2-collect-data"></a>2. shromažďování dat

####    <a name="enable-diagnostics-logging-for-your-web-app"></a>Povolení protokolování diagnostiky pro webovou aplikaci

Web Apps prostředí funguje, diagnostické protokolování informace z webového serveru a webové aplikace. Toto jsou logicky rozdělené do webového serveru diagnostiky a složka Doručená pošta aplikace Outlook.

##### <a name="web-server-diagnostics"></a>Diagnostika webový server

Můžete povolit nebo zakázat následující typy protokolů:

-   **Podrobné protokolování chyb** – podrobné informace o chybě pro HTTP stavů označující selhání (stavový kód 400 nebo vyšší). To může obsahovat informace, které vám můžou pomoct zjistit, proč server vrátil kód chyby.
-   **Nepodařilo trasování žádost** - podrobné informace o nezdařeném uložení požadavků, včetně trasování součástí služby IIS slouží k zpracovat žádosti a doba v jednotlivé součásti. To může být užitečné, když se pokoušíte výkon webové aplikace nebo izolovat co způsobuje specifickou chybu HTTP.
-   **Protokolování na webový Server** – informace o formátu W3C rozšířené protokolu HTTP transakce. To je užitečné při určování celkové metriky webové aplikace, jako je třeba počet zpracování žádostí o nebo kolik žádosti se z konkrétní IP adresy.

##### <a name="application-diagnostics"></a>Složka Doručená pošta aplikace Outlook

Diagnostika aplikace umožňuje zachycení informací vytvořené pomocí webové aplikace. Pomocí aplikace ASP.NET `System.Diagnostics.Trace` třídy přihlásit informace k protokolu aplikace diagnostických nástrojů.

Podrobné informace o tom, jak nakonfigurovat aplikaci pro protokolování najdete v článku [Povolení diagnostiky protokolování pro web apps v aplikaci služby Azure](web-sites-enable-diagnostic-log.md).

#### <a name="use-remote-profiling"></a>Použijte vzdálené použití profilů

V aplikaci služby Azure Web Apps, aplikace rozhraní API a WebJobs může být vzdáleně vytvořený profil. Pokud procesu běží pomaleji, než byste čekali, nebo latence požadavků HTTP vyšší než normální a využití procesoru procesu je také vysoká, můžete vzdáleně profilu procesu a získat štosech volání odběr procesoru a analyzujte data aktivitu procesu a žádanou cesty kódu.

Další informace o najdete v článku [Podpora vzdálené použití profilů v aplikaci služby Azure](/blog/remote-profiling-support-in-azure-app-service).


#### <a name="use-the-azure-app-service-support-portal"></a>Používat portál Azure aplikaci služby podpory

Webová aplikace poskytuje možnost Poradce při potížích s problémům souvisejícím s webovou aplikaci pohledem HTTP protokoly, protokoly událostí, výpisy obrázku a další. Dostanete tyto informace naše portálu podpory na **http://&lt;název aplikace >.scm.azurewebsites.net/Support**

Portál Azure aplikace služby dál nebude podporovat nabízí tři samostatné karty podporuje tři kroky běžné situace Poradce při potížích:

1.  Sledujte aktuální chování
2.  Analýza shromažďování diagnostické informace a spuštěním předdefinované analyzátory
3.  Zmírnění

Pokud problém se děje teď, klikněte na **analyzovat** > **diagnostiky** > **Diagnostika teď** chcete vytvořit relaci diagnostické, který shromáždí HTTP protokoly, prohlížeč protokoly událostí, paměť výpisy, protokolů chyb PHP a PHP zpracovat sestavy.

Jakmile se shromažďují data, bude taky dělat analýzu dat a poskytnout zprávu ve formátu HTML.

V případě, že chcete stáhnout data, ve výchozím nastavení, byste měli uložené ve složce D:\home\data\DaaS.

Další informace o portálu Azure aplikace služby podpory najdete v článku [Nových aktualizacích na linku podpory webu Azure weby](/blog/new-updates-to-support-site-extension-for-azure-websites).

#### <a name="use-the-kudu-debug-console"></a>Použití konzoly ladění Kudu

Webové aplikace je součástí konzoly ladění využívající ladění průzkumu, nahrávání souborů, jakož i koncové body JSON pro získání informací o prostředí. Je místo toho možnost _Kudu konzoly_ nebo _Řídicího panelu Správce služeb_ pro web app.

Dostanete tento řídicí panel tak, že přejdete na odkaz **https://&lt;název aplikace >.scm.azurewebsites.net/**.

Některé věci, které poskytuje Kudu jsou:

-   nastavení prostředí aplikace
-   protokol toku
-   diagnostické výpisu
-   ladění konzoly, ve kterém spuštěním rutiny prostředí Powershell a základní DOS příkazy.


Další užitečné funkce Kudu je, v případě, že aplikace je aktivační první možnosti výjimky, můžete použít Kudu a vypíše nástroj SysInternals Procdump vytvořit paměti. Tyto výpisů paměti jsou snímky procesu a často vám můžou pomoct složitější potíží s webovou aplikaci.

Další informace o funkcí dostupných v Kudu najdete v článku [Azure weby týmovou nástroje, které byste měli vědět o](/blog/windows-azure-websites-online-tools-you-should-know-about/).

<a name="mitigate" />
### <a name="3-mitigate-the-issue"></a>3. Tento problém zmírnění

####    <a name="scale-the-web-app"></a>Změna velikosti web appu

V aplikaci služby Azure pro zvýšení výkonu a výkon, můžete nastavit měřítko niž používáte aplikaci. Škálování do webových aplikací zahrnuje dvou souvisejících akcí: Změna plán služeb aplikací na vyšší úroveň ceny a konfiguraci některá nastavení po přepnutí do vyšší úrovně ceny.

Další informace o měřítka najdete v článku [Měřítko web app v aplikaci služby Azure](web-sites-scale.md).

Kromě toho můžete spustit aplikaci na více než jedna instance. Nejen to vám nabízí další možnosti zpracování, ale taky dává některé počtu odolnost proti chybám. Pokud tento proces přejde do jedna instance, jiné instance dál, podávání žádosti o.

Můžete nastavit měřítko jako ručně nebo automaticky.

####    <a name="use-autoheal"></a>Použití AutoHeal

AutoHeal recykluje pracovního procesu aplikace na základě nastavení, které vyberete (třeba změny konfigurace, požadavky, omezení na základě paměti nebo dobu potřebnou k provedení požadavku). Ve většině případů, Koš procesu je nejrychlejší způsob, jak obnovit z problém. Když restartujete můžete vždy web app z přímo v portálu Azure, AutoHeal se to udělá automaticky za vás. Je třeba udělat stačí přidat některých aktivačních událostí v kořenovém web.config pro webovou aplikaci. Všimněte si, že tato nastavení vhodná stejným způsobem, i když je vaše aplikace není .net jednu.

Další informace najdete v tématu [Automatické retušování Azure weby](/blog/auto-healing-windows-azure-web-sites/).

####    <a name="restart-the-web-app"></a>Restartujte web appu

To je často nejjednodušší způsob, jak obnovit z jednorázové problémy. Na [Portálu Azure](https://portal.azure.com/)na zásuvné webovou aplikaci, máte možnost zastavení nebo opětovné spuštění aplikace.

 ![Restartujte web appu můžete vyřešit problémy s výkonem](./media/app-service-web-troubleshoot-performance-degradation/2-restart.png)

Můžete spravovat webovou aplikaci pomocí prostředí Powershell Azure. Další informace najdete v tématu [použití Azure pomocí Správce prostředků Azure](../powershell-azure-resource-manager.md).
