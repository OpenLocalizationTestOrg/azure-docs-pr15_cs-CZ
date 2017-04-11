<properties 
    pageTitle="Řešení potíží s aplikací přehledy v aplikaci project web Java" 
    description="Průvodce pro řešení: sledování živou Java aplikace s přehledy aplikace." 
    services="application-insights" 
    documentationCenter="java"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="03/01/2016" 
    ms.author="awills"/>
 
# <a name="troubleshooting-and-q-and-a-for-application-insights-for-java"></a>Řešení potíží a A a Q pro přehledy aplikace jazyka Java

Otázky nebo problémy s [Přehledy aplikace Visual Studio v Java][java]? Tady je několik tipů.


## <a name="build-errors"></a>Vytvoření chyby

*V zatmění při přidávání SDK přehledy aplikace prostřednictvím Maven nebo Gradle mi sestavení nebo kontrolního chybových zpráv funkce ověření.*

* Pokud závislost <version> prvek používá vzorek se zástupnými znaky (například (Maven) `<version>[1.0,)</version>` nebo (Gradle) `version:'1.0.+'`), zkuste místo toho zadat určitou verzi jako `1.0.2`. Najdete v článku [poznámky k verzi](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) pro nejnovější verzi.

## <a name="no-data"></a>Žádná data 

*Úspěšně přidán přehledy aplikace a spuštění aplikace Moje, ale nikdy, uloží se dat na portálu.*

* Počkejte chvíli a klikněte na aktualizovat. Grafy aktualizovat sami pravidelně, ale můžete taky aktualizovat ručně. Interval aktualizace závisí na časový rozsah grafu.
* Zkontrolujte, že máte klávesu se přístrojového vybavení definice v souboru ApplicationInsights.xml (ve složce zdrojů v projektu)
* Zkontrolujte, že je žádné `<DisableTelemetry>true</DisableTelemetry>` uzel v souboru xml.
* V bráně firewall bude pravděpodobně nutné otevřete porty TCP 80 a 443 pro odchozí přenosy dat pro dc.services.visualstudio.com. Prohlédněte si [kompletní seznam výjimky brány firewall](app-insights-ip-addresses.md)
* V Microsoft Azure začít vývěsky, podívejte se na mapě stav služby. Pokud existují některá upozornění údaje, počkejte, až budou vrátili na OK a potom zavřete a znovu otevřete svůj zásuvné aplikace přehledy aplikace.
* Zapnout protokolování do okna konzoly integrovaném vývojovém prostředí přidáním `<SDKLogger />` prvek uzlu kořenové v souboru ApplicationInsights.xml (ve složce zdrojů v projektu) a vyhledat položky začíná [Chyba].
* Ujistěte se, že správný ApplicationInsights.xml soubor úspěšně načetl Java SDK pohledem zpráv do konzoly výstup pro výkaz "konfigurační soubor obsahuje byly úspěšně nebyl nalezen".
* Pokud soubor konfigurace nebyl nalezen, zaškrtněte políčko zprávy výstup zobrazíte, kde konfiguračního souboru je vyhledáván a ujistěte se, že ApplicationInsights.xml se nachází v jednom z těchto umístění vyhledávání. Jako pravidlo můžete vložit soubor config poblíž JARs SDK přehledy aplikace. Příklad: v Tomcat, bude to znamenat složce WEB-informace nebo knihovny.



#### <a name="i-used-to-see-data-but-it-has-stopped"></a>Mám použít k zobrazení dat, ale přestal

* Zkontrolujte [Stav blogu](http://blogs.msdn.com/b/applicationinsights-status/).
* Máte klikněte na měsíční kvóty datových bodů? Otevřít nastavení/kvóty a ceny, pokud chcete zjistit. Pokud ano, můžete upgradovat plánu nebo platit za další kapacity. V tématu [ceny schéma](https://azure.microsoft.com/pricing/details/application-insights/).

#### <a name="i-dont-see-all-the-data-im-expecting"></a>Proč nevidím všechna data, která se mi očekává

* Otevřete kvót a ceny zásuvné a zkontrolujte, zda [odběr](app-insights-sampling.md) je v operaci. (100 % přenosu znamená, že odběr není v operaci.) Služby aplikace přehledy můžete nastaven na příjem jenom část telemetrie, které přichází z aplikace. Pomůže vám to nedovoluje vaše měsíční kvóta telemetrie. 

## <a name="no-usage-data"></a>Bez použití dat

*Zobrazuje se zpráva údaje o žádosti a doby odezvy bez zobrazení stránky, ale prohlížeče nebo uživatelská data.*

Úspěšně nastavíte aplikace odeslat telemetrie ze serveru. Teď dalším krokem je [nastavení webové stránky odeslat telemetrie z webového prohlížeče][usage].

Můžete taky Pokud je váš klient aplikace v [telefonu nebo jiné zařízení][platforms], odešlete telemetrie odtud. 

Pomocí klávesy stejné přístrojového vybavení nastavení klienta a serveru telemetrie. Data se zobrazí v stejný zdroj přehledy aplikace a budete moct sladit události z klienta a serveru.



## <a name="disabling-telemetry"></a>Zakázání telemetrie

*Jak můžu vypnout telemetrie kolekce?*

V kódu:

    TelemetryConfiguration config = TelemetryConfiguration.getActive();
    config.setTrackingIsDisabled(true);


**Nebo** 

Aktualizujte ApplicationInsights.xml (ve složce zdrojů v projektu). Přidání pod nadpisem kořenový:

    <DisableTelemetry>true</DisableTelemetry>

Pomocí metody XML, musíte restartujte aplikaci při změně hodnotu.

## <a name="changing-the-target"></a>Změna cíle

*Změna které Azure zdroje projekt odešle data*

* [Pokud potřebujete klávesu přístrojového vybavení nového prostředku.][java]
* Pokud jste přidali přehledy aplikace do projektu pomocí nástrojů Azure pro Eclipse, klikněte pravým tlačítkem web projektu, vyberte **Azure**, **Konfigurovat přehledy aplikace**a změna klíče.
* V opačném aktualizujte klíč ApplicationInsights.xml ve složce zdrojů v projektu.


## <a name="the-azure-start-screen"></a>Azure úvodní obrazovky

*Mám hledat v [portálu Azure](https://portal.azure.com). Mapy Řekněte mi něco o aplikace Moje?*

Ne, zobrazuje stav Azure serverů ve světě.

*Z Azure start vývěsky (domovská obrazovka) jak lze zjistit dat o aplikace Moje?*

Za předpokladu, že jste [Nastavení aplikace pro přehledy aplikace][java], klikněte na tlačítko Procházet, vyberte přehledy aplikace a vyberte zdroj aplikaci vytvořili aplikace. Získat tam rychleji v budoucích můžete připnout aplikaci na vývěsku start.

## <a name="intranet-servers"></a>Servery intranetu

*Můžete sledovat serveru na můj intranet?*

Ano, pokud že váš server poslat telemetrie přehledy aplikace portál veřejné Internetu. 

V bráně firewall bude pravděpodobně nutné otevřete porty TCP 80 a 443 pro odchozí přenosy dat pro dc.services.visualstudio.com a f5.services.visualstudio.com.

## <a name="data-retention"></a>Uchovávání informací 

*Jak dlouho zůstane dat na portálu? Je zabezpečené?*

Přečtěte si téma [uchovávání dat a osobních údajů][data].

## <a name="next-steps"></a>Další kroky

*Můžu nastavit aplikace přehledy aplikace Moje Java serveru. Co dalšího můžete dělat?*

* [Sledovat dostupnost webových stránek][availability]
* [Sledovat použití webovou stránku][usage]
* [Sledovat použití a Diagnostika problémy v aplikacích pro zařízení][platforms]
* [Kódu můžete sledovat použití aplikace][track]
* [Zachycení protokoly pro diagnostiku][javalogs]


## <a name="get-help"></a>Získání nápovědy

* [Přetečení zásobníku](http://stackoverflow.com/questions/tagged/ms-application-insights)

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[data]: app-insights-data-retention-privacy.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[platforms]: app-insights-platforms.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-web-track-usage.md

 