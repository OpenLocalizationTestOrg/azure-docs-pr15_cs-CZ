<properties 
    pageTitle="Poradce při potížích s žádná data - aplikace přehledy pro .NET" 
    description="Nevidím dat v přehledy aplikace Visual Studio? Zkuste tady." 
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
    ms.date="10/24/2016" 
    ms.author="awills"/>
 
# <a name="troubleshooting-no-data---application-insights-for-net"></a>Poradce při potížích s žádná data - aplikace přehledy pro .NET

## <a name="some-of-my-telemetry-is-missing"></a>Část Moje telemetrie chybí

*V aplikaci přehledy vidím jenom část vytvořených pomocí aplikace Moje události.*

* Pokud se zobrazí konzistentní stejné zlomek, to být způsobené adaptivní [odběr](app-insights-sampling.md). Potvrďte to otevřete hledání (z zásuvné přehled) a podívejte se na instanci žádosti o nebo jiné události. V dolní části Vlastnosti klikněte na trojtečkou (...) k získání celé vlastnost podrobností. Pokud žádost o Count > 1 a potom odběr je v operaci. 
* V ostatních případech je možné, že se máte zasažení [omezit data sazba](app-insights-pricing.md#limits-summary) cenových plánu. Za minutu, použijí se tyto limity.

## <a name="no-data-from-my-server"></a>Žádná data ze serveru

*Po instalaci aplikace Moje ve webovém serveru a teď nevidím všechny telemetrie z něho. Tato funkce fungovala OK v počítači vývojáře.*

* Pravděpodobně problém bránu firewall. [Nastavení výjimky brány firewall pro přehledy aplikace odeslat data](app-insights-ip-addresses.md).

*Mám [nainstalovanou sledování stavu](app-insights-monitor-performance-live-website-now.md) na webovém serveru sledování existující aplikace. Proč nevidím žádné výsledky.*

* V tématu [Poradce při potížích s sledování stavu](app-insights-monitor-performance-live-website-now.md#troubleshooting). 


## <a name="q01"></a>Možnost bez přidání aplikace přehledy ve Visual Studiu

*Po vytvoření nového projektu ve Visual Studiu, případně klikněte pravým tlačítkem existujícího projektu v okně Průzkumník, nevidím všechny aplikace přehledy možnosti.*

+ Ne všechny typy .NET projektu podporovaných nástrojů. Jsou podporované pro web a WCF projekty. U jiných typů projektů například plochy nebo služby aplikace můžete pořád [ručně přidat sadu SDK přehledy aplikace do projektu](app-insights-windows-desktop.md).
+ Zkontrolujte, jestli že máte [Visual Studio 2013 aktualizaci 3 nebo novější](http://go.microsoft.com/fwlink/?LinkId=397827). Přijde předinstalovaná pomocí nástroje pro přehledy aplikací.
+ Vyberte **Nástroje** **rozšíření a aktualizace** a zkontrolujte, že **Aplikace přehledy nástroje** nainstalované a povolené. Pokud ano, klikněte na **aktualizace** zobrazíte, pokud je dostupná nějaká aktualizace.
+ Otevření dialogového okna Nový projekt a zvolte webové aplikace. Pokud se zobrazí možnost aplikace přehledy tam, jsou nainstalované nástroje. Pokud ne, zkuste odinstalovat a znova nainstalovat nástroje pro přehledy aplikací.


## <a name="q02"></a>Přidání aplikace přehledy se nezdařila.

*Při vytváření nového projektu nebo při pokusu o přidání přehledy aplikace do existujícího projektu, zobrazuje se chybová zpráva.*

Pravděpodobně příčin:

* Komunikace s přehledy aplikace portál se nezdařil. nebo
* Existuje nějaký problém s účtem Azure;
* Stačí [přístup pro čtení předplatné nebo skupinu, kde chcete vytvořit nový zdroj](app-insights-resources-roles-access-control.md).

Tento problém odstranit:

+ Zaškrtněte políčko uvedli přihlašovací údaje pro vpravo Azure účet. 
+ V prohlížeči zkontrolujte, že máte přístup na [portál Azure](https://portal.azure.com). Otevřete nastavení a podívejte se, pokud je omezení.
+ [Přidání aplikace přehledy do existujícího projektu](app-insights-asp-net.md): V Průzkumníku klikněte pravým tlačítkem myši projektu a zvolte "Přidat aplikaci přehledy."
+ Pokud stále nefunguje, použijte [Ruční postup](app-insights-windows-services.md) přidání zdroje do portálu a pak přidat SDK do projektu. 

## <a name="emptykey"></a>Dojde k chybě "přístrojového vybavení klíč nemůže být prázdné"

Vypadá to, něco se nepovedlo při byly instalaci aplikace přehledy nebo možná adaptér protokolování.

V Průzkumníku klikněte pravým tlačítkem myši `ApplicationInsights.config` a vyberte **Konfigurovat přehledy aplikace**. Zobrazí se dialog s vyzývá k přihlášení k Azure a vytvořte prostředek přehledy aplikace nebo znovu použít existující úrovně.


##<a name="NuGetBuild"></a>"NuGet balíčky chybí" na serveru Tvůrce dotazů

*Všechno, co když se mi ladění v počítači vývoj, ale o chybě NuGet na serveru vytvořit vytvoří OK.*

Najdete [NuGet balíčku obnovit](http://docs.nuget.org/Consume/Package-Restore) a [Automatické obnovení balíčku](http://docs.nuget.org/Consume/package-restore/migrating-to-automatic-package-restore).

## <a name="missing-menu-command-to-open-application-insights-from-visual-studio"></a>Chybí příkaz nabídky otevřete aplikaci přehledy z aplikace Visual Studio

*Můžu pravým tlačítkem myši projekt Průzkumník řešení, proč nevidím všechny aplikace přehledy příkazy nebo nevidím příkaz Otevřít přehledy aplikace.*

Pravděpodobně příčin:

* Pokud jste vytvořili zdroje aplikace přehledy ručně nebo pokud je projekt typ, který neposkytuje podporu nástroje přehledy aplikace.
* V sekci nástroje aplikace přehledy jsou zakázány ve Visual Studiu.
* Visual Studio je starší než 2013 aktualizace 3.

Tento problém odstranit:

* Zkontrolujte, jestli že vaše verze aplikace Visual Studio není aktualizace 2013 3 nebo novější.
* Vyberte **Nástroje** **rozšíření a aktualizace** a zkontrolujte, že **Aplikace přehledy nástroje** nainstalované a povolené. Pokud ano, klikněte na **aktualizace** zobrazíte, pokud je dostupná nějaká aktualizace.
* Klikněte pravým tlačítkem v okně Průzkumník projektu. Pokud se zobrazí příkaz **Konfigurovat přehledy aplikace**, umožňuje připojit projektu tomuto zdroji ve službě přehledy aplikace.


V opačném povahy projektu se nepodporuje přímo v sekci nástroje přehledy aplikace. Chcete-li zobrazit telemetrie, přihlaste se k [portálu Azure](https://portal.azure.com), zvolte přehledy aplikace v levém navigačním panelu a vyberte aplikace.

## <a name="access-denied-on-opening-application-insights-from-visual-studio"></a>"Přístup odepřen" o otevření aplikace přehledy z aplikace Visual Studio

*Příkaz nabídky "Otevřít aplikaci přehledy" přenese do portálu Azure, ale, dojde k chybě "přístup odepřen".*

![](./media/app-insights-asp-net-troubleshoot-no-data/access-denied.png)

Aplikaci Microsoft přihlašovací naposledy použité na výchozí prohlížeč nemá přístup k [tomuto prostředku, který byl vytvořený při přidání aplikace přehledy pro tuto aplikaci](app-insights-asp-net.md). Existují dvě pravděpodobné důvodů: 

* Máte víc než jednoho účtu Microsoft – možná čísle do zaměstnání, osobním účtem Microsoft? Přihlašování, které jste použili naposledy na výchozí prohlížeč byl pro jiný účet než ten, který má přístup k [Přidání přehledy aplikace do projektu](app-insights-asp-net.md). 

 * Oprava: Klikněte na své jméno v horní pravé straně okna prohlížeče a odhlášení. Pak se přihlaste pomocí účtu, který má oprávnění k přístupu. Klikněte na levém navigačním panelu klikněte na aplikace přehledy a vyberte aplikace.

* Někdo jiný přidal aplikace přehledy k projektu a zapomněli dát [přístup ke skupině zdroje](app-insights-resources-roles-access-control.md) , ve kterém byl vytvořen. 

 * Oprava: Pokud používají účet organizace, mohou přidat můžete týmu; nebo se vám může udělit jednotlivé přístup ke skupině zdroje.



## <a name="asset-not-found-on-opening-application-insights-from-visual-studio"></a>"Materiálů nebyl nalezen" o otevření aplikace přehledy z aplikace Visual Studio

*Příkaz nabídky "Otevřít aplikaci přehledy" přenese do portálu Azure, ale, dojde k chybě "materiálů nebyl nalezen".*

Pravděpodobně příčin:

* Aplikace přehledy zdroje pro aplikaci odstraněn. nebo
* Klávesu přístrojového vybavení nastavit nebo změnit v ApplicationInsights.config úpravou ho přímo, bez aktualizace projektového souboru. 

Klíč přístrojového vybavení v ovládacích prvcích ApplicationInsights.config místo, kam se odesílá telemetrie. Řádek v souboru projektu ovládací prvky, které zdroje se otevře, když použijete příkaz ve Visual Studiu. 

Tento problém odstranit:

* V Průzkumníku projektu klikněte pravým tlačítkem myši a zvolte aplikace přehledy konfigurovat přehledy aplikace. V dialogovém okně buď můžete existující zdroji zašlete telemetrie nebo vytvořte nový účet. Nebo:
* Otevřete zdroj přímo. Přihlaste se k [portálu Azure](https://portal.azure.com), klikněte na přehledy aplikace v levém navigačním panelu a vyberte aplikace.



## <a name="where-do-i-find-my-telemetry"></a>Kde najdu svůj telemetrie?

*Přihlášení k [portálu Microsoft Azure](https://portal.azure.com)a mám hledat na řídicím panelu Azure Domů. Takže kde najdu Moje aplikace přehledy data?*

* Na levém navigačním panelu klikněte na přehledy aplikace a pak název aplikace. Pokud už nemáte žádné projekty tam, budete muset [Přidat nebo konfigurovat přehledy aplikace na web projektu](app-insights-asp-net.md).

    Tam uvidíte některé souhrnné grafy. Můžete procházet mít možnost vidět podrobněji.

* Ve Visual Studiu při ladění aplikace, klikněte na tlačítko přehledy aplikace.


## <a name="q03"></a>Žádná data serveru (nebo žádná data vůbec)

*Spuštění aplikace Moje a jeho otevření službu aplikace přehledy v Microsoft Azure, ale všechny grafy zobrazují "Zjistěte, jak získat informace o..." nebo "Není nakonfigurován."* Případně *pouze zobrazení stránky a uživatelských dat, ale žádná data serveru.*

+ Spusťte aplikaci v režimu ladění ve Visual Studiu (F5). Použijte aplikaci MONTI tak, aby generovat některé telemetrie. Zkontrolujte, že se události zaznamenané v okně výstupu Visual Studio. 

    ![](./media/app-insights-asp-net-troubleshoot-no-data/output-window.png)

+ Na portálu aplikace přehledy otevřete [Diagnostiky hledání](app-insights-diagnostic-search.md). Data se obvykle zobrazí tady nejdřív.
+ Klikněte na tlačítko Aktualizovat. Pravidelně zásuvné samotné aktualizuje, ale udělat ji můžete taky ho ručně. Interval aktualizace je delší pro větší časového rozsahu.
+ Zaškrtněte políčko že odpovídají klíči přístrojového vybavení. Na hlavní zásuvné pro vaši aplikaci distribuovali portálu aplikace přehledy v rozevíracím seznamu **Essentials** zkontrolujte **přístrojového vybavení klíče**. Pak v projektu ve Visual Studiu, otevřete ApplicationInsights.config a najděte `<instrumentationkey>`. Zkontrolujte, zda jsou dvě klávesy rovností. V opačném případě:
 + Na portálu klikněte na aplikace přehledy a vyhledejte aplikaci zdroje s pravé klávesy; nebo
 + Ve Visual Studiu Průzkumníku projektu klikněte pravým tlačítkem myši a zvolte aplikace přehledy konfigurovat. Obnovení aplikace na pravém zdroji zašlete telemetrie.
 + Pokud nemůžete najít odpovídající klíče, zkontrolujte, že používáte stejné přihlašovací údaje ve Visual Studiu podle k portálu.

    ![](./media/app-insights-asp-net-troubleshoot-no-data/ikey-check.png)
    
+ V [domovské řídicího panelu Microsoft Azure](https://portal.azure.com)prohlédněte mapu stav služby. Pokud existují některá upozornění údaje, počkejte, až budou vrátili na OK a potom zavřete a znovu otevřete svůj zásuvné aplikace přehledy aplikace.
+ Zkontrolujte taky [našeho blogu stav](http://blogs.msdn.com/b/applicationinsights-status/).
+ Začaly psát jakýkoli kód pro [serverovou SDK](app-insights-api-custom-events-metrics.md) , která se mohou měnit klíč přístrojového vybavení v `TelemetryClient` instancí nebo `TelemetryContext`? Začaly psaní [Konfigurace filtru nebo vzorků](app-insights-api-filtering-sampling.md) , která může filtrování se příliš mnoho?
+ Při úpravě ApplicationInsights.config pečlivě zkontrolujte konfiguraci [TelemetryInitializers a TelemetryProcessors](app-insights-api-filtering-sampling.md). Nesprávně názvem typu nebo parametr mohou způsobit SDK odeslat žádná data.

## <a name="q04"></a>Žádná data o využití zobrazení stránky, prohlížeče,

*Zobrazuje se zpráva data v grafech doba odezvy serveru a požadavky serveru, ale žádná data načítání stránek zobrazení, nebo v prohlížeči nebo použití listy.*

Skripty na webových stránkách se berou data. 

+ Pokud jste přidali přehledy aplikace na stávající web projekt, [že budete muset přidat skripty od ruky](app-insights-javascript.md).
+ Zkontrolujte, že Internet Explorer nezobrazuje váš web v režimu kompatibility.
+ Funkce prohlížeče ladění (F12 v některých prohlížečích, klikněte na síť) pro ověření, že se data odeslaný do `dc.services.visualstudio.com`.

## <a name="no-dependency-or-exception-data"></a>Žádná data závislost nebo výjimce

V tématu [závislost telemetrie](app-insights-asp-net-dependencies.md) a [telemetrie výjimku](app-insights-asp-net-exceptions.md).

## <a name="no-performance-data"></a>Žádná data o výkonu

Data o výkonu (procesor, vstupu a výstupu sazby a tak dál) je k dispozici pro [Java webové služby](app-insights-java-collectd.md) [Windows aplikace klasické pracovní plochy](app-insights-windows-desktop.md), [služby IIS webové aplikace a služby nainstalujete sledování stavu](app-insights-monitor-performance-live-website-now.md)a [Azure Cloud Services](app-insights-azure.md). najdete ji v části nastavení servery.

Není k dispozici Azure weby.

## <a name="no-server-data-since-i-published-the-app-to-my-server"></a>Žádná data (server) od publikování aplikace na server

+ Kontrola skutečně zkopírovány Microsoft. ApplicationInsights DLL Server společně s Microsoft.Diagnostics.Instrumentation.Extensions.Intercept.dll
+ V bráně firewall bude pravděpodobně nutné [Otevřít některé porty TCP](app-insights-ip-addresses.md#data-access-api).
+ Pokud máte k poslání mimo vaší podnikové sítě použít na proxy server, v okně nastavte [defaultProxy](https://msdn.microsoft.com/library/aa903360.aspx) Web.config
+ Windows Server 2008: Ujistěte se, máte nainstalovaný tyto aktualizace: [KB2468871](https://support.microsoft.com/kb/2468871), [KB2533523](https://support.microsoft.com/kb/2533523) [KB2600217](https://support.microsoft.com/kb/2600217).


## <a name="i-used-to-see-data-but-it-has-stopped"></a>Mám použít k zobrazení dat, ale přestal

* Zkontrolujte [Stav blogu](http://blogs.msdn.com/b/applicationinsights-status/).
* Máte klikněte na měsíční kvóty datových bodů? Otevřete nastavení/kvóty a ceny, pokud chcete zjistit. Pokud ano, můžete upgradovat plánu nebo platit za další kapacity. V tématu [ceny schéma](https://azure.microsoft.com/pricing/details/application-insights/).


## <a name="i-dont-see-all-the-data-im-expecting"></a>Proč nevidím všechna data, která se mi očekává

Pokud používáte aplikaci přehledy SDK 2.0.0-beta3 verze technologie ASP.NET nebo novější aplikace odešle velké množství dat, může funkce [adaptivní analytický nástroj vzorkování](app-insights-sampling.md) ovládání a odeslat jenom procento vaší telemetrie. 

Můžete zakázat, ale tato možnost se nedoporučuje. Analytický nástroj vzorkování je navržen tak, že související telemetrie správně přenášena, pro účely diagnostiky. 

## <a name="wrong-geographical-data-in-user-telemetry"></a>Nepovedlo zeměpisnými údaji v telemetrie uživatele

Město, oblast a dimenzí země jsou odvozeny od IP adresy a není vždy přesné.

## <a name="exception-method-not-found-on-running-in-azure-cloud-services"></a>Výjimky "metoda nebyl nalezen" o spuštění cloudové služby Azure

Je vytvářet pro .NET 4.6? 4.6 nepodporuje automaticky role Azure Cloud Services. [Instalace 4.6 na každou roli](../cloud-services/cloud-services-dotnet-install-dotnet.md) před spuštěním aplikace.

## <a name="still-not-working"></a>Pořád to nefunguje...

* [Fórum aplikace přehledy](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)

