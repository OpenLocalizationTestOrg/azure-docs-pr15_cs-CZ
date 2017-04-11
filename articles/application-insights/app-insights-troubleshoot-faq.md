<properties 
    pageTitle="Řešení potíží a otázky o aplikaci přehledy" 
    description="Něco ve Visual Studiu aplikace přehledy nejasné nebo nefunguje? Zkuste tady." 
    services="application-insights" 
    documentationCenter=".net"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="awills"/>
 
# <a name="questions---application-insights-for-aspnet"></a>Otázky – přehledy aplikace pro ASP.NET

## <a name="configuration-problems"></a>Problémy s konfigurací

*Mám potíže s nastavení Moje:*

* [.NET aplikace](app-insights-asp-net-troubleshoot-no-data.md)
* [Sledování již spuštěnou aplikaci](app-insights-monitor-performance-live-website-now.md#troubleshooting)
* [Azure diagnostiky](app-insights-azure-diagnostics.md)
* [Java v prohlížeči](app-insights-java-troubleshoot.md)
* [Dalších platformách](app-insights-platforms.md)

*Můžu získat žádná data ze serveru*

* [Nastavení výjimky brány firewall](app-insights-ip-addresses.md)
* [Nastavení serveru ASP.NET](app-insights-monitor-performance-live-website-now.md)
* [Nastavení serveru Java](app-insights-java-agent.md)


## <a name="can-i-use-application-insights-with-"></a>Dá se pomocí aplikace interpretaci s...?

[V tématu platformy][platforms]


## <a name="is-it-free"></a>Je to zadarmo?

* Ano, pokud se rozhodnete bezplatné [ceny osy](app-insights-pricing.md). Dostanete velkorysých kvóty dat a většina funkcí. 
* Zadejte data kreditní karty k registraci Microsoft Azure, ale žádné poplatky budou provedeny, pokud používáte jiný zaplacený Azure služby nebo výslovně upgradovat na platební osy.
* Pokud aplikace odešle víc dat než měsíční kvóta pro bezplatné osy, přestane protokolování. Pokud k tomu dojde, můžete buď zvolte spuštění platební nebo počkejte, dokud se neobnoví kvóty na konci měsíce.
* Základní použití a relace dat nepodléhá kvóty.
* Je také bezplatnou 30denní zkušební, po kterou funkci placené zdarma získat.
* Jednotlivé aplikace zdroje obsahuje samostatné kvót a nastavte jeho cenových osy nezávisle na všechna další.

#### <a name="what-do-i-get-if-i-pay"></a>Co můžu získat, když mám zaplatit?

* Větší [měsíční kvóty data](https://azure.microsoft.com/pricing/details/application-insights/).
* Možnost zaplatit průměrných pokračujte shromažďování dat prostřednictvím měsíční kvóty. Pokud vaše data se podíváme na kvóta, že účtovaná za Mb.
* [Export nepřetržitě](app-insights-export-telemetry.md).


## <a name="q14"></a>Co aplikace přehledy změnit v projektu?

Podrobnosti, závisí na typu projektu. Pro webovou aplikaci:


+ Přidá tyto soubory do projektu:

 + ApplicationInsights.config. 
 + AI.js


+ Instalace tyto balíčky NuGet:

 -  *Rozhraní API aplikace přehledy* – základní rozhraní API

 -  *Rozhraní API aplikace přehledy pro webové aplikace* – slouží k odeslání telemetrie ze serveru

 -  *Rozhraní API aplikace přehledy for JavaScript Applications* – slouží k odeslání telemetrie z klienta

    Balíčky zahrnout tyto sestavení:

 - Microsoft.ApplicationInsights

 - Microsoft.ApplicationInsights.Platform

+ Vloží položky do:

 - Web.config

 - Packages.config

+ (Nové projekty pouze - li [Přidat přehledy aplikace do existujícího projektu][start], je potřeba provést ruční.) Vloží fragmenty kódu klienta a serveru inicializace s ID aplikace přehledy zdroje. V aplikaci MVC, například kód je vložen do stránky předlohy Views/Shared/_Layout.cshtml


## <a name="how-do-i-upgrade-from-older-sdk-versions"></a>Jak upgradovat ze starších verzí SDK

Zobrazit [poznámky k verzi](app-insights-release-notes.md) pro SDK vhodný pro váš typ aplikace. 



## <a name="update"></a>Změna které Azure zdroje projekt odešle data

V Průzkumníku klikněte pravým tlačítkem myši `ApplicationInsights.config` a zvolte **Update aplikace přehledy**. Odešlete data do existující nebo nové zdroje v Azure. Průvodce aktualizací změní klíč přístrojového vybavení v ApplicationInsights.config, která určuje, kde server SDK odešle data. Pokud zrušíte "Aktualizovat vše", změní se také klávesu umístění na webových stránkách.


#### <a name="data"></a>Jak dlouho zůstane dat na portálu? Je zabezpečené?

Podívejte se na [uchovávání dat a osobních údajů][data].

## <a name="logging"></a>Zapnout protokolování

#### <a name="post"></a>Jak zobrazit data příspěvek v diagnostiky hledání?

Jsme není protokolování údajů o příspěvek automaticky, ale můžete použít TrackTrace volání: umístění dat v parametru zprávy. To může mít delší velikost než omezení řetězce vlastnosti, když se nedá se filtrovat v něm. 

## <a name="security"></a>Zabezpečení

#### <a name="is-my-data-secure-in-the-portal-how-long-is-it-retained"></a>Jsou moje data zabezpečené na portálu? Jak dlouho to se zachovají?

V tématu [dat uchovávání informací a ochrana osobních údajů][data].


## <a name="q17"></a>Povolili jsem všechno, co v aplikaci přehledy?

<table border="1">
<tr><th>Co byste měli vidět</th><th>Jak ho získat</th><th>Proč představ</th></tr>
<tr><td>Dostupnost grafy</td><td><a href="../app-insights-monitor-web-app-availability/">Testuje web</a></td><td>Vědět, že váš web appu je nahoru</td></tr>
<tr><td>Server aplikace výkon: doby odezvy …
</td><td><a href="../app-insights-asp-net/">Přidání aplikace přehledy do projektu</a><br/>nebo <br/><a href="../app-insights-monitor-performance-live-website-now/">Sledování stavu AI nainstalovat serveru</a> (nebo zadejte vlastní kód pro <a href="../app-insights-api-custom-events-metrics/#track-dependency">sledování závislostí</a>)</td><td>Zjišťování problémů výkonu</td></tr>
<tr><td>Závislost typu telemetrie</td><td><a href="../app-insights-monitor-performance-live-website-now/">Sledování stavu AI nainstalujte na server</a></td><td>Diagnostika problémů s databází nebo jiné externí součásti</td></tr>
<tr><td>Získání zásobníku trasování z výjimky</td><td><a href="../app-insights-search-diagnostic-logs/#exceptions">Vložení TrackException volání v kódu</a> (ale některé jsou automaticky uvedeny)</td><td>Zjištění a diagnostice výjimky</td></tr>
<tr><td>Hledání protokolu trasování</td><td><a href="../app-insights-search-diagnostic-logs/">Přidání adaptér protokolování</a></td><td>Diagnostika výjimek, výkon problémy</td></tr>
<tr><td>Základní informace o použití klienta: zobrazení stránky, relace,...</td><td><a href="../app-insights-javascript/">Inicializační JavaScriptu na webových stránkách</a></td><td>Analýza využití</td></tr>
<tr><td>Vlastní nastavení klienta</td><td><a href="../app-insights-api-custom-events-metrics/">Sledování volání na webových stránkách</a></td><td>Vylepšení uživatelského rozhraní</td></tr>
<tr><td>Vlastní nastavení serveru</td><td><a href="../app-insights-api-custom-events-metrics/">Sledování volání v kódu serveru</a></td><td>Funkce Business intelligence</td></tr>
</table>


## <a name="automation"></a>Automatizace

Můžete k vytvoření a aktualizace aplikací přehledy zdroje [psaní skriptů Powershellu](app-insights-powershell.md) .

## <a name="more-answers"></a>Další odpovědi

* [Fórum aplikace přehledy](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)


<!--Link references-->

[data]: app-insights-data-retention-privacy.md
[platforms]: app-insights-platforms.md
[start]: app-insights-overview.md
[windows]: app-insights-windows-get-started.md

 