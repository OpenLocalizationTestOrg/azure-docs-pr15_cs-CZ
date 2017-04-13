<properties 
   pageTitle="Sledování porovnání produktů Microsoft | Microsoft Azure"
   description="Microsoft operace správy sady (OMS) je cloudu společnosti Microsoft na základě řešení pro správu IT, který vám pomáhá spravovat a chránit vaše místních a cloudových infrastruktury.  Tento článek uvádí různé služby součástí OMS a obsahuje odkazy na podrobný obsah."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="microsoft-monitoring-product-comparison"></a>Sledování porovnání produktů společnosti Microsoft

Tento článek poskytuje srovnání System Center operace správce (SCOM) a technologie pro analýzu protokolu v sadě správy operace (OMS) z hlediska jejich architektura, logickou jak budou sledovat prostředky a jak budou analýza dat, která budou shromáždit.  Toto je vám umožní základní principy jejich rozdílů a relativní pevnosti.  

## <a name="basic-architecture"></a>Základní architektura
### <a name="system-center-operations-manager"></a>System Center Operations Manager

Všechny SCOM součásti v datovém centru.  V systému Windows a Linux stroje, které jsou spravovány SCOM [nainstalovaná agenti](http://technet.microsoft.com/library/hh551142.aspx) .  Agentů připojit k [Servery správy](https://technet.microsoft.com/library/hh301922.aspx) , která komunikujete SCOM databáze a datový sklad.  Agentů spolehnout ověření domény připojit k serverům správy.  Můžou být mimo domény můžete provádět ověřovací certifikát nebo připojení k [Serveru brány](https://technet.microsoft.com/library/hh212823.aspx).

SCOM vyžaduje dva SQL databáze, jeden pro provozní data a jiné datový sklad k podpoře sestav a analýza dat.  Spustí na [Serveru sestav](https://technet.microsoft.com/library/hh298611.aspx) služby Reporting Services SQL vykázat data z datový sklad. 

SCOM můžete sledovat zdrojů cloudu pomocí management Pack pro produkty například [Azure](https://www.microsoft.com/download/details.aspx?id=38414), [Office 365](https://www.microsoft.com/download/details.aspx?id=43708)a [AWS](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/AWSManagementPack.html).  Tyto management Pack umožňuje jeden nebo více místní agentů jako proxy servery pro zjištění cloudové zdroje a spuštěné pracovní postupy změřit jejich výkon a dostupnosti.  Proxy agentů slouží ke [Sledování síťových zařízení](https://technet.microsoft.com/library/hh212935.aspx) a jiných externích zdrojů.

Konzola operace je aplikace pro systém Windows, ke kterému je připojen mezi servery správy a umožňuje správcům prohlížení a analýza dat shromážděných a konfigurace prostředí SCOM.  Konzoly založené na webu mohou být umístěny na libovolnou serveru IIS a poskytuje analýza dat pomocí prohlížeče.

![SCOM architektura](media/operations-management-suite-monitoring-product-comparison/scom-architecture.png)

### <a name="log-analytics"></a>Protokol analýzy

Většina komponent OMS jsou v Azure cloudu, takže můžete nasazovat a spravovat s minimálními náklady a pro správu plánování řízené úsilí.  Všechna data shromážděná protokolu analýzy uložené v úložišti OMS.

Protokol analýzy můžete shromáždit data od jedním ze tří zdrojů:

- Fyzické a virtuálních počítačích s Windows a [Microsoft sledování Agent (MMA)](https://technet.microsoft.com/library/mt484108.aspx) nebo Linux a [Agent pro správu sady operace Linux](https://technet.microsoft.com/library/mt622052.aspx).  Tyto počítače může být místní nebo virtuálních počítačích Azure nebo jiného cloudu.
- Účet Azure úložiště s [Azure diagnostiky](../cloud-services/cloud-services-dotnet-diagnostics.md) data shromážděná Azure pracovníka role, web nebo virtuálního počítače.
- [Připojení ke skupině správy SCOM](https://technet.microsoft.com/library/mt484104.aspx).  V této konfiguraci agentů komunikovat s SCOM správy servery, které jsou data v databázi SCOM místo, kam ho pak dostane OMS úložiště.
Správci analýze dat shromážděných a konfigurace protokolu analýzy pomocí portálu OMS, který je hostovaný v Azure a můžete k nim získat přístup z libovolného prohlížeče.  Mobilní aplikace pro přístup k tato data jsou dostupné pro standardní platformách.

![Architektura technologie pro analýzu protokolu](media/operations-management-suite-monitoring-product-comparison/log-analytics-architecture.png)

### <a name="integrating-scom-and-log-analytics"></a>Integrace SCOM a technologie pro analýzu protokolu

Když SCOM slouží jako zdroje dat pro analýzy protokolu můžete využít funkce oba produkty v hybridním prostředí sledování.  Můžete nakonfigurovat existující agentů SCOM prostřednictvím konzole operace spravovány OMS, kromě nadále spuštění balíčky správy z SCOM.  
Data z připojeného skupiny Správa SCOM dostane protokolu analýzy pomocí jedné ze čtyř metod:

- Události a data o výkonu se shromažďují agentem a doručila ji do SCOM.  Správa servery v SCOM potom dodat data protokolu analýzy.
- Některé například protokoly služby IIS a události zabezpečení nadále nedoručila ji přímo do protokolu analýzy z agenta.
- Řešení některých doručit agent další software nebo vyžadují instalaci softwaru ke shromáždění dalších informací.  Tato data bude obvykle odeslané přímo do protokolu analýzy.
- Řešení některých shromáždí data přímo z SCOM správy serverech nepochází z agenta.  Například [řešení upozornění správy](https://technet.microsoft.com/library/mt484092.aspx) shromažďuje upozornění od SCOM po jejich vytvoření.

## <a name="monitoring-logic"></a>Sledování použití logických operátorů
SCOM a technologie pro analýzu protokolu pracovat s podobná data odebrané agentů, ale jsou zásadní rozdíly v tom, jak definovat a implementace jejich logiky pro shromažďování dat a jak budou analyzovat data, která budou shromáždit.

### <a name="operations-manager"></a>Operations Manager
Sledování logiky pro SCOM je součástí [sady správy](https://technet.microsoft.com/library/hh457558.aspx) , které obsahují logiky pro zjišťování součásti ke sledování, měření stavu těchto složek a pro shromažďování dat a analyzujte data.  Sledování dat můžou skládat například shromažďování událost nebo výkonu čítač nebo můžou použít komplexní logiky implementovaná v skriptu.  Balíčky správy, které obsahují úplné sledování jsou k dispozici pro různé [Microsoft a aplikace jiných výrobců](http://go.microsoft.com/fwlink/?LinkId=82105) kromě zařízení hardware a sítě.  Můžete [vytvářet vlastní management Pack](http://aka.ms/mpauthor) pro vlastní aplikace.

Management Pack obsahovat více pracovních postupů každá provádí některé funkce distinct sledování například odběr čítače výkonu, kontrola stavu služby nebo spuštění skriptu.  Každý pracovní postup spuštěn nezávisle na sobě a definuje vlastních výsledků, například používané databáze bude zapisovat a zda vygeneruje upozornění. 

Je možné přepsat podrobnosti pracovního postupu, například požadovanou četnost spouštějí, prahové hodnoty, které považují za chybu a závažnosti upozornění, které vytvářejí.  Další funkce můžete zadat také přidáním vlastních pracovních postupů.

![Přepsání](media/operations-management-suite-monitoring-product-comparison/scom-overrides.png)

Management Pack jsou nainstalované v databázi Operations Manager a automaticky úměrně agentů správy servery.  Každý agent automaticky stahovaly management Pack a načíst pracovní postupy týkající se aplikace, které jste nainstalovali.  Dat shromážděných agentem dostane zpět server pro správu pro vložení do SCOM databáze a datový sklad.  Operace konzoly umožňuje prohlížení a analýze tato data pomocí vlastního zobrazení, řídicí panely a sestav součástí management pack.

Distribuce aktualizací management Pack je znázorněn v následujícím diagramu.

![Tok Management pack](media/operations-management-suite-monitoring-product-comparison/scom-mpflow.png)

### <a name="log-analytics"></a>Protokol analýzy
#### <a name="event-and-performance-collection"></a>Shromažďování výkonu a události
Protokol analýzy shromažďuje událostí a výkonnosti z agenta systémů pomocí zdrojům, jako jsou protokolu událostí systému Windows, protokoly služby IIS a Syslog.  Můžete určit kritéria pro které dat je shromážděných prostřednictvím portálu protokolu technologie pro analýzu a vytvořte protokolu dotazy pro účely analýzy dat shromážděných.  Standardní kritéria je definován při vytváření pracovního prostoru OMS a definujete další data pro konkrétní aplikace. 

![Definování protokoly událostí v protokolu analýzy](media/operations-management-suite-monitoring-product-comparison/log-analytics-definedata.png)

Zatímco SCOM má mnoho podrobné pracovní postupy, které obvykle definují určitá kritéria pro dat a akci, která by měl provést v odpovědi, analýzy protokolu má obecnější kritéria pro shromažďování dat.  Protokol dotazů a řešení poskytují další cílové kritéria pro analýzu a budou sloužit na určitá data v cloudu, až se shromažďují.

#### <a name="solutions"></a>Řešení
Řešení poskytují další logiky pro shromažďování dat a analýzy.  Můžete vybrat řešení ke svému předplatnému OMS přidat z Galerie řešení.

![Galerie řešení](media/operations-management-suite-monitoring-product-comparison/log-analytics-solutiongallery.png)

Řešení především v cloudu poskytující analýzy událostí a výkonnosti shromažďované v úložišti OMS spustit.  Mohou také definovat další data shromážděny, která lze analyzovat protokolu dotazy nebo další uživatelské rozhraní služby řešení v řídicím panelu OMS. 

[Sledování změn řešení](https://technet.microsoft.com/library/mt484099.aspx) například zjistí konfiguraci změny v systémech agent a zápisy události do úložiště OMS, která lze analyzovat s několika zobrazeními grafické, jež shrnují zjištěny změny.  Můžete k podrobnostem v souhrnné zobrazení do protokolu dotazů, které zobrazují podrobná data shromážděná řešení.


Když vyberete řešení ke svému předplatnému přidat, nemáte aktuálně možnost vytvořit vlastní řešení.  Můžete vybrat událostí a výkonnosti můžete shromažďovat a vytvářet vlastní zobrazení podle protokolu dotazů.

Sledování logiky pro analýzu protokolu je uveden v následujícím diagramu.

![Tok analýzy řešení protokolu](media/operations-management-suite-monitoring-product-comparison/log-analytics-solution-flow.png)

## <a name="health-monitoring"></a>Sledování stavu
### <a name="operations-manager"></a>Operations Manager
SCOM můžete modelovat jednotlivých součástí aplikace a zadat v reálném čase stavu pro jednotlivá pole.  To umožňuje nejen zobrazení zjišťování chyb výkonu a časem, ale taky ověřit skutečný stav aplikace nebo systému a jeho jednotlivé součásti v daném okamžiku.  Protože rozumí časová období, která aplikace je k dispozici, modul stavu v SCOM také podporují službu úroveň smlouvami (SLA) které analyzovat a informování o dostupnosti aplikace určitou dobu.

Například následující zobrazení: zobrazí v reálném čase stavu modulů SQL databáze kontroloval SCOM.  Stav všech databáze z jednoho z databázové stroje se zobrazuje v dolní polovině zobrazení.

![Zobrazení stavu](media/operations-management-suite-monitoring-product-comparison/scom-state-view.png)

Průzkumník stavu pro jeden z databázové stroje jsou uvedeny níže s monitory, které se používají k určení jeho celkový stav.  Tyto monitory jsou definované v SQL management pack a kontrolovat všechny stroje databáze SQL zjištěny SCOM.

![Průzkumník stavu](media/operations-management-suite-monitoring-product-comparison/scom-health-explorer.png)

Prvky na více systémů můžete kombinovat změřit stavu distribuované aplikace.  To může být užitečné pro aplikací line of business, které obsahují více distribuované součástí.  Můžete vytvořit model zobrazující stav jednotlivých součástí tohoto pole zahrnutí do dostupnost pro aplikaci.

Služba Active Directory je příklad jednu sadu správy, které obsahuje modelu pro analýzu distribuované součásti.  Ukázka diagramu pod zobrazuje stavu celkového prostředí a vztah mezi strukturami, domén a řadiče domény.  Každá z těchto součástí obsahuje dílčí součásti a víc monitorů podobně jako v SQL příkladu výše.

![Zobrazení diagramu SCOM](media/operations-management-suite-monitoring-product-comparison/scom-diagram-view.png)

### <a name="log-analytics"></a>Protokol analýzy
OMS nezahrnuje modul běžné aplikacím modelu a měření v reálném čase stavu.  Jednotlivé řešení mohou hodnotit celkový stav určitých služeb na základě dat shromážděných a jejich vlastní logiky nainstalovat agent v reálném čase analýza.  Protože řešení spustit v cloudu s přístupem k úložišti OMS, jsou často poskytují hlubší analýze než obvykle provádí management Pack. 

Například [hodnocení AD a SQL hodnocení řešení](https://technet.microsoft.com/library/mt484102.aspx) analýze dat shromážděných a zadání hodnocení různé aspekty prostředí.  Zahrnuje doporučení pro vylepšení, které lze zlepšit dostupnost a výkonu prostředí.

![AD zhodnocení řešení](media/operations-management-suite-monitoring-product-comparison/log-analytics-ad-assessment.png)

## <a name="data-analysis"></a>Analýza dat
SCOM a technologie pro analýzu protokolu poskytují různé funkce a analyzujte data shromážděná data.  SCOM obsahuje zobrazení a řídicí panely na konzole operace pro analýzu nejnovějších dat v různých formátech a sestav k prezentování dat z datový sklad ve formátu tabulky.  Protokol analýzy obsahuje celý protokol dotazovací jazyk a rozhraní pro analýzu dat v úložišti OMS.  Při použití SCOM jako zdroje dat pro analýzu protokolu úložiště obsahuje data shromážděná SCOM tak protokolu analytických nástrojů mohou sloužit k analýze dat z obou.

### <a name="operations-manager"></a>Operations Manager

#### <a name="views"></a>Zobrazení
Zobrazení v konzole operace umožní zobrazit odlišnými datovými typy shromažďované SCOM v různých formátech, obvykle tabulkových pro události, upozornění a data stavu a spojnicové grafy pro data o výkonu.  Zobrazení provést minimální analýza nebo sloučení dat, ale umožňují filtrovat podle určitého kritéria. 

![Zobrazení](media/operations-management-suite-monitoring-product-comparison/scom-views.png)

Správa sad obvykle poskytují více zobrazení podporující aplikace nebo systému, který sleduje.  Zobrazení stavu pro jiné objekty, které management pack zjistí, upozornění zjištěny problémy a výkon zobrazení čítačů může zahrnovat.

Zobrazení jsou vhodné zejména pro analýzu aktuální stav prostředí včetně otevřít upozornění a stavu sledované systémů a objekty.  Můžete procházet hierarchii na podrobné událost nebo podpůrné konkrétní upozornění k Diagnostika jeho příčina data o výkonu. Stejně tak můžete zobrazit výkon a stavu jiné součásti webové části aplikace k posouzení z aktuálního stavu.

#### <a name="dashboards"></a>Řídicí panely
Řídicí panely v konzole operace primárně funguje se stejnými daty, jako zobrazení ale jsou snáz přizpůsobitelná a mohou obsahovat bohatší vizualizace.  Sada standardní řídicí panely jsou k dispozici, že je možné snadno upravit vlastní důvodů.  Můžete taky použít widgety Powershellu, které můžete zobrazit data vrácená z dotazu Powershellu.

![Řídicí panel](media/operations-management-suite-monitoring-product-comparison/scom-dashboard.png)

Vývojáři mít možnost přidat vlastní součásti řídicí panely, které obsahují v jejich management Pack.  Tyto může být vysoce specializované pro konkrétní aplikaci například řídicího panelu v SQL management pack ukázáno v následujícím příkladu.  Tento řídicího panelu lze také jako šablonu pro vlastní verze.

![Řídicí panel SQL](media/operations-management-suite-monitoring-product-comparison/scom-sql-dashboard.png)

#### <a name="reports"></a>Sestavy
Sestavy v SCOM analýza dat z datový sklad ve formátu tabulky.  Může být tisku a naplánované pro automatické doručení v jiných formátech souboru včetně PDF, CSV a Word.  Sestavy pracovat s daty z datový sklad tak, aby byly vhodné zejména pro analýzu dlouhodobou trendů.

Správa sad obvykle poskytují vlastní sestavy pro konkrétní aplikaci.  Můžete také vybrat z knihovny obecný sestav, které můžete přizpůsobit pro své vlastní aplikace nebo k provádění analýz ad hoc.

Následuje výkonu ukázková sestava zobrazující data shromážděná Active Directory Management Pack.

![Sestava](media/operations-management-suite-monitoring-product-comparison/scom-report.png)

### <a name="log-analytics"></a>Protokol analýzy
Technologie pro analýzu protokolu má [dotazovací jazyk](https://technet.microsoft.com/library/mt484120.aspx) , který slouží k provádění analýz přes data z více aplikací bez nutnosti vytvořte vlastní zobrazení nebo sestavy.  Protože OMS implementovaná v cloudu, výkonu dotazů a analýza dat nejsou vyměřené poplatky za jeho případnými omezeními hardware a můžete rychle analyzovat dotazů včetně milióny záznamů. 

Dotazy v protokolu analýzy jsou také základ další funkce.  Můžete uložit do dotazu, exportovat výsledky do Excelu nebo ji automaticky spouštět v pravidelných intervalech a vytvářet upozornění, pokud jeho výsledky podle určitého kritéria.  

![Protokol dotazu toku](media/operations-management-suite-monitoring-product-comparison/log-analytics-query-flow.png)

Tady je příklad dotazu protokolu analýzy.  V tomto příkladu jsou získány a seskupené podle události všechny události s "na úvodní obrazovce" název ID.  Uživatel jednoduše poskytuje dotaz a technologie pro analýzu protokolu dynamicky generuje uživatelské rozhraní pro provádění analýzy.  Výběr všech položek v seznamu vrátí data podrobné události.

![Protokol dotazu](media/operations-management-suite-monitoring-product-comparison/log-analytics-query.png)

Kromě poskytování ad hoc analýzy, můžete uložit pro budoucí použití dotazů v protokolu technologie pro analýzu a taky přidá do [řídicího panelu OMS](http://technet.microsoft.com/library/mt484090.aspx) , jak je vidět v následujícím příkladu.

![Řídicí panel OMS](media/operations-management-suite-monitoring-product-comparison/log-analytics-dashboard.png)

## <a name="next-steps"></a>Další kroky

- Nasazení [System Center Operations Manager (SCOM)](https://technet.microsoft.com/library/hh205987.aspx).
- Registrace k [Protokolu analýzy](https://azure.microsoft.com/documentation/services/log-analytics).  
