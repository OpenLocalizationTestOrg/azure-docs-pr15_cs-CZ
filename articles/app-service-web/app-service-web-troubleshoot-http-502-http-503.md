<properties
    pageTitle="Oprava 502 Chybná brána, 503 Služba není k dispozici chyby | Microsoft Azure"
    description="Poradce při potížích 502 Chybná brána a 503 Služba není k dispozici chyby ve vaší webové aplikaci hostovaný aplikací služby Azure."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
    tags="top-support-issue"
    keywords="502 Chybná brána 503 Služba není k dispozici, chyba 503, Chyba 502"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="cephalin"/>

# <a name="troubleshoot-http-errors-of-502-bad-gateway-and-503-service-unavailable-in-your-azure-web-apps"></a>Poradce při potížích HTTP "502 Chybná brána" a "503 Služba není k dispozici" v Azure web apps

"502 Chybná brána" a "503 Služba není k dispozici" jsou běžné chyby ve vaší webové aplikaci hostovaný [aplikací](http://go.microsoft.com/fwlink/?LinkId=529714)služby Azure. Tento článek vám pomůže při řešení těchto chyb.

Pokud potřebujete další pomoc kdykoli v tomto článku můžete kontaktovat Azure odborníků na [MSDN Azure a fórech přetečení zásobníku](https://azure.microsoft.com/support/forums/). Můžete taky můžete poslat taky incident Azure podpory. Přejděte na [Web podpory Azure](https://azure.microsoft.com/support/options/) a klikněte na **Získat podporu**.

## <a name="symptom"></a>Příznaku

Když přejdete na web appu, vrátí HTTP "502 Chybná brána" nebo dojde k HTTP Chyba "503 Služba není k dispozici".

## <a name="cause"></a>Příčina

Tento problém je často způsobená úrovně problémy s aplikací, například:

-   požadavky na trvá moc dlouho.
-   aplikace pomocí vysoké paměti/procesoru
-   aplikace k selhání kvůli výjimce.

## <a name="troubleshooting-steps-to-solve-502-bad-gateway-and-503-service-unavailable-errors"></a>Návody na řešení potíží vyřešit "502 Chybná brána" a "503 Služba není k dispozici" chyb

Poradce při potížích s můžete rozdělit na tři samostatné úkoly v pořadí:

1.  [Sledovat a monitorovat chování aplikace](#observe)
2.  [Shromažďování dat](#collect)
3.  [Zmírnění problém](#mitigate)

[Aplikace služby Web Apps](/services/app-service/web/) nabízí různé možnosti při každém kroku.

<a name="observe" />
### <a name="1-observe-and-monitor-application-behavior"></a>1. sledovat a monitorovat chování aplikace

####    <a name="track-service-health"></a>Sledovat stav služby

Microsoft Azure publicizes pokaždé, když je přerušení nebo výkonu snížení úrovně služeb. Sledovat stav služby na [Portálu Azure](https://portal.azure.com/). Další informace najdete v tématu [sledovat stav služby](../monitoring-and-diagnostics/insights-service-health.md).

####    <a name="monitor-your-web-app"></a>Sledovat web appu

Tato možnost umožňuje zjistit, pokud jakékoli problémy s aplikace. Na zásuvné webovou aplikaci klikněte na dlaždici **požadavky a chyby** . **Metriky** zásuvné řádku se zobrazí všechny metriky, které můžete přidat.

Některé z metriky, které můžete chtít sledovat pro webovou aplikaci

-   Průměrná pracovní sada paměti
-   Průměrná doba odezvy
-   Procesor času
-   Pracovní sada paměti
-   Požadavky

![sledovat web appu k řešení chyb HTTP 502 Chybná brána a 503 Služba není k dispozici](./media/app-service-web-troubleshoot-HTTP-502-503/1-monitor-metrics.png)

Další informace najdete v tématu:

-   [Sledování aplikací webové služby Azure aplikace](web-sites-monitor.md)
-   [Přijímání oznámení](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

<a name="collect" />
### <a name="2-collect-data"></a>2. shromažďování dat

####    <a name="use-the-azure-app-service-support-portal"></a>Používat portál Azure aplikaci služby podpory

Webová aplikace poskytuje možnost Poradce při potížích s problémům souvisejícím s webovou aplikaci prohlédnete HTTP protokoly, protokoly událostí, výpisy obrázku a další. Dostanete tyto informace naše portálu podpory na **http://&lt;název aplikace >.scm.azurewebsites.net/Support**

Portál Azure aplikace služby dál nebude podporovat nabízí tři samostatné karty podporuje tři kroky obvyklý řešení potíží:

1.  Sledujte aktuální chování
2.  Analýza shromažďování diagnostické informace a spuštěním předdefinované analyzátory
3.  Zmírnění

Pokud problém se děje teď, klikněte na **analyzovat** > **diagnostiky** > **Diagnostika teď** chcete vytvořit relaci diagnostické, který shromáždí HTTP protokoly, prohlížeč protokoly událostí, paměti výpisy, protokolů chyb PHP a PHP zpracovat sestavy.

Jakmile se shromažďují data, bude taky dělat analýzu dat a poskytnout zprávu ve formátu HTML.

V případě, že chcete stáhnout data, ve výchozím nastavení, byste měli uložené ve složce D:\home\data\DaaS.

Další informace o portálu Azure aplikace služby podpory najdete v článku [Nových aktualizacích na linku podpory webu Azure weby](/blog/new-updates-to-support-site-extension-for-azure-websites).

####    <a name="use-the-kudu-debug-console"></a>Použití konzoly ladění Kudu

Webové aplikace je součástí konzoly ladění, můžete použít pro ladění, zkoumání a nahrávání souborů, jakož i koncové body JSON pro získání informací o prostředí. Je místo toho možnost _Kudu konzoly_ nebo _Řídicího panelu Správce služeb_ pro web app.

Dostanete tento řídicí panel tak, že přejdete na odkaz **https://&lt;název aplikace >.scm.azurewebsites.net/**.

Některé věci, které poskytuje Kudu jsou:

-   nastavení prostředí aplikace
-   protokol toku
-   diagnostické výpisu
-   ladění konzoly, ve kterém spuštěním rutiny prostředí Powershell a základní DOS příkazy.


Další užitečné funkce Kudu je, v případě, že aplikace je aktivační první možnosti výjimky, můžete použít Kudu a vypíše nástroj SysInternals Procdump vytvořit paměti. Tyto výpisů paměti jsou snímky procesu a často vám můžou pomoct složitější potíží s webovou aplikaci.

Další informace o funkcí dostupných v Kudu najdete v článku [online nástroje Azure weby, které byste měli vědět o](/blog/windows-azure-websites-online-tools-you-should-know-about/).

<a name="mitigate" />
### <a name="3-mitigate-the-issue"></a>3. Tento problém zmírnění

####    <a name="scale-the-web-app"></a>Změna velikosti web appu

V aplikaci služby Azure pro zvýšení výkonu a výkon, můžete nastavit měřítko niž používáte aplikaci. Škálování do webových aplikací zahrnuje dvou souvisejících akcí: Změna plán služeb aplikací na vyšší úroveň ceny a konfiguraci některá nastavení po přepnutí do vyšší úrovně ceny.

Další informace o měřítka najdete v článku [Měřítko web app v aplikaci služby Azure](web-sites-scale.md).

Kromě toho můžete spustit aplikaci na více než jedna instance. Nejen to vám nabízí další možnosti zpracování, ale taky dává některé počtu odolnost proti chybám. Pokud tento proces přejde do jedna instance, jiné instance dál, podávání žádosti o.

Můžete nastavit měřítko jako ručně nebo automaticky.

####    <a name="use-autoheal"></a>Použití AutoHeal

AutoHeal recykluje pracovního procesu aplikace na základě nastavení, jaká jste (třeba změny konfigurace, požadavky, omezení na základě paměti nebo dobu potřebnou k provedení požadavku). Ve většině případů, Koš procesu je nejrychlejší způsob, jak obnovit z potíže. Když restartujete můžete vždy web app z přímo v portálu Azure, AutoHeal se to udělá automaticky za vás. Je třeba udělat stačí přidat některých aktivačních událostí v kořenovém web.config pro webovou aplikaci. Všimněte si, že tato nastavení vhodná stejným způsobem, i když je vaše aplikace není .net jednu.

Další informace najdete v tématu [Automatické retušování Azure weby](/blog/auto-healing-windows-azure-web-sites/).


####    <a name="restart-the-web-app"></a>Restartujte web appu

To je často nejjednodušší způsob, jak obnovit z jednorázový problémy. Na [Portálu Azure](https://portal.azure.com/)na zásuvné webovou aplikaci, máte možnost zastavení nebo opětovné spuštění aplikace.

 ![Restartujte aplikaci vyřešit chyby HTTP 502 Chybná brána a 503 Služba není k dispozici](./media/app-service-web-troubleshoot-HTTP-502-503/2-restart.png)

Můžete spravovat webovou aplikaci pomocí prostředí Powershell Azure. Další informace najdete v tématu [použití Azure pomocí Správce prostředků Azure](../powershell-azure-resource-manager.md).
