<properties
    pageTitle="Aktualizace řešení pro správu v OMS | Microsoft Azure"
    description="Tento článek je určený pro vám pomůže pochopit, jak používat toto řešení pro správu aktualizace z počítače Windows a Linux."
    services="operations-management-suite"
    documentationCenter=""
    authors="MGoedtel"
    manager="jwhit"
    editor=""
    />
<tags
    ms.service="operations-management-suite"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="magoedte"/>

# <a name="update-management-solution-in-omsmediaoms-solution-update-managementupdate-management-solution-iconpng-update-management-solution-in-oms"></a>![Řešení pro správu aktualizací v OMS](./media/oms-solution-update-management/update-management-solution-icon.png) Řešení pro správu aktualizací v OMS

Řešení pro správu aktualizace v OMS umožňuje spravovat aktualizace z počítače Windows a Linux.  Můžete rychle zjistit stav dostupné aktualizace všech počítačů agent a zahájí proces instalace požadovaných aktualizací pro servery. 

## <a name="prerequisites"></a>Zjistit předpoklady pro

-   Windows agentů musí být nakonfigurované buď pro komunikaci se serverem serveru služby WSUS (Windows Update) nebo mají přístup k webu Microsoft Update.  

    >[AZURE.NOTE] Agent systému Windows, nelze spravovat souběžně pomocí Správce konfigurace System Center.  
  
-   Linux agentů musí mít taky přístup k aktualizaci úložiště.  Agent OMS Linux můžete stáhnout z [GitHub](https://github.com/microsoft/oms-agent-for-linux). 

## <a name="configuration"></a>Konfigurace

Provedení následujících kroků můžete přidat řešení pro správu aktualizace do pracovního prostoru OMS a přidejte agentů Linux.  Windows agentů automaticky přidají s žádnou další konfiguraci.

1.  Řešení pro správu aktualizace dodejte OMS pracovního prostoru pomocí proces popsaný v [řešení OMS přidat](../log-analytics/log-analytics-add-solutions.md) z Galerie řešení.  
2.  Na portálu OMS vyberte **Nastavení** a potom **Připojení zdroje**.  Poznámka: **pracovní prostor ID** a buď **primárního klíče** nebo **vedlejší klíče**.
3.  Proveďte následující kroky pro každý počítač Linux.

    na.  Nainstalujte nejnovější verzi agenta OMS Linux spuštěním následující příkazy.  Nahrazení <Workspace ID> s ID pracovní prostor a <Key> primární a sekundární klíčem.

        cd ~
        wget https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/v1.2.0-75/omsagent-1.2.0-75.universal.x64.sh
        sudo bash omsagent-1.2.0-75.universal.x64.sh --upgrade -w <Workspace ID> -s <Key>

     b. Chcete-li odebrat zástupce, spusťte tento příkaz.

        sudo bash omsagent-1.2.0-75.universal.x64.sh --purge

## <a name="management-packs"></a>Správa sad

Pokud vaše skupina správy System Center Operations Manager je připojené k OMS pracovního prostoru, pak následující management Pack bude mít nainstalovanou na Operations Manager při přidání toto řešení. Nemůže žádným způsobem konfigurace ani údržbu management Pack povinné. 

-   Microsoft System Center Advisor aktualizace hodnocení Intelligence Pack (Microsoft.IntelligencePacks.UpdateAssessment)
-   Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
-   Aktualizace nasazení MP

Další informace o aktualizace řešení management Pack najdete v článku [Připojení Operations Manager do protokolu analýzy](../log-analytics/log-analytics-om-agents.md).

## <a name="data-collection"></a>Shromažďování dat

### <a name="supported-agents"></a>Podporované agentů

Následující tabulka popisuje připojení zdroje, které jsou podporovány toto řešení.

Připojení zdroje | Podporované | Popis|
----------|----------|----------|
Windows agentů | Ano | Řešení shromažďuje informace o aktualizacích pro systém Windows zástupci a zahájí instalace požadovaných aktualizací.|
Linux agentů | Ano | Řešení shromažďuje informace o aktualizacích pro systém zástupci Linux.|
Skupina Správa operace správce | Ano | Řešení shromažďuje informace o aktualizacích pro systém zástupci ve skupině připojení správy.<br>Přímé připojení z agenta Operations Manager k protokolu analýzy není potřeba. Úložiště OMS předán ze skupiny správy dat.|
Účet Azure úložiště | Ne | Azure úložiště neobsahuje informace o aktualizacích pro systém.|  

### <a name="collection-frequency"></a>Kolekce četnosti

Pro každý spravovaných počítač Windows prohledávání proběhne dvakrát po dnech.  Po instalaci aktualizace informací o se aktualizuje objevit během 15 minut.  

U každého spravovaných Linux počítače probíhá vyhledávání každé 3 hodiny.  

## <a name="using-the-solution"></a>Použití řešení

Když přidáte do pracovního prostoru OMS řešení pro správu aktualizace, dlaždici **Update Management** se přidá do řídícího OMS. Této dlaždici se zobrazují počet a grafické znázornění počet ve vašem prostředí aktuálně vyžadující aktualizace systému.<br><br>
![Aktualizace správy souhrnné dlaždice](media/oms-solution-update-management/update-management-summary-tile.png)  

## <a name="viewing-update-assessments"></a>Zobrazení aktualizace hodnocení

Klikněte na dlaždici **Správu aktualizací** otevřete řídicí panel pro **Správu aktualizace** . Řídicí panel obsahuje sloupce v následující tabulce. Každý sloupec oblasti seznamy maximálně deseti položek, které odpovídají tohoto sloupce kritéria pro zadaný obor a časový rozsah. Můžete spustit hledání protokol, který vrací všechny záznamy tak, že kliknete na **Zobrazit všechny** v dolní části sloupce nebo kliknutím na záhlaví sloupce.

Sloupec | Popis|
----------|----------|
**Chybějící aktualizace počítače** ||
Kritické nebo aktualizace zabezpečení | Seznamy, do kterých horní deseti počítačů, které nebyly nalezeny aktualizuje seřazené podle počtu aktualizace jsou chybějící. Klikněte na název počítače spustit hledání protokolu vrací všechny aktualizace záznamů pro tento počítač.|
Pole Kritický nebo aktualizace zabezpečení starší než 30 dní.| Určuje počet počítačů, které jsou chybějící kritické nebo seskupená podle časový úsek od publikování aktualizace aktualizace zabezpečení. Klikněte na jednu z položkách, které chcete spustit hledání protokolu vrací všechny chybějící a důležité aktualizace.|
**Povinné aktualizace chybí**||
Kritické nebo aktualizace zabezpečení | Seznam klasifikace aktualizace, že jsou počítače chybějící seřazené podle počtu počítačů chybějící aktualizace v kategorii. Klikněte na klasifikace spustit hledání protokolu vrací všechny aktualizace záznamů pro tuto kategorii.|
**Aktualizace nasazení**||
Aktualizace nasazení | Počet aktuálně plánované aktualizace nasazení a dobu trvání do příští automatické spuštění.  Klikněte na dlaždici a zobrazit plány, aktuálně spuštěných dokončené aktualizace nebo naplánování nového nasazení.|  
<br>  
![Řídicí panel pro správu souhrnné aktualizace](./media/oms-solution-update-management/update-management-deployment-dashboard.png)<br>  
<br>
![Aktualizace zobrazení počítač správy řídicího panelu](./media/oms-solution-update-management/update-management-assessment-computer-view.png)<br>  
<br>
![Aktualizace zobrazení balíčku správy řídicího panelu](./media/oms-solution-update-management/update-management-assessment-package-view.png)<br>  

## <a name="installing-updates"></a>Instalace aktualizací

Jakmile aktualizace byly posouzeny všechny počítače se systémem Windows ve vašem prostředí, můžete vyžadovaly nainstalované vytvořením *Aktualizace nasazení*aktualizace.  Nasazení aktualizace je naplánované instalace požadovaných aktualizací pro jeden nebo víc počítačů Windows.  Zadejte data a času pro nasazení kromě počítač nebo skupinu počítačů, které mají být použity.  

Podle runbooks v Azure automatizaci se mají instalovat aktualizace.  Nedají se zobrazit aktuálně tyto runbooks a není nutná žádná konfigurace.  Po vytvoření nasazení aktualizovat ho vytvoří plán v tomto spuštění postupu runbook předlohy aktualizace na zadaný časový zahrnuty počítačům.  Tento předlohy postupu runbook začíná každý agent systému Windows, které provede instalace požadovaných aktualizací podřízené postupu runbook.  

### <a name="viewing-update-deployments"></a>Zobrazení aktualizace nasazení

Klikněte na dlaždici **Aktualizace nasazení** seznam všech stávajících nasazení aktualizace.  Jsou seskupená podle stavu – **Naplánované** **spuštěný**a **Dokončeno**.<br><br> ![Stránka plán aktualizace nasazení](./media/oms-solution-update-management/update-updatedeployment-schedule-page.png)<br>  

Vlastnosti pro každou aktualizaci nasazení zobrazeny jsou popsané v následující tabulce.

Vlastnost | Popis|
----------|----------|
Jméno | Název nasazení aktualizace.|
Plán | Typ kalendáře.  *OneTime* momentálně pouze možná hodnota.|
Spuštění|Datum a čas, který aktualizace nasazení je naplánováno zahájení.|
Doba trvání | Počet minut, které nasazení aktualizace může spustit.  Pokud všechny aktualizace nejsou nainstalovány v rámci tuto dobu trvání, potom zbývající aktualizace musí čekat další aktualizace nasazení.|
Servery | Počet ovlivňují nasazení aktualizace počítače.|
Stav | Aktuální stav zavedení aktualizace.<br><br>Možné hodnoty jsou:<br>-Nezahájilo<br>– Spuštění<br>-Dokončení|  

Klikněte na nasazení aktualizace zobrazíte jeho obrazovky podrobných dat, která obsahuje sloupce v následující tabulce.  Pokud ještě nezačala nasazení aktualizace, nebudou tyto sloupce zadá.<br>

Sloupec | Popis|
----------|----------|
**Výsledky počítače**||
Byla úspěšně dokončena | Uvádí počet počítačů v nasazení aktualizovat podle stavu.  Klikněte na stav a spustit hledání protokolu vrací všechny aktualizace záznamů s tímto stavem nasazení aktualizovat.|
Stav instalace počítače| Seznam souvisejících nasazení aktualizace a procento aktualizace, které úspěšně nainstalovaný počítačů. Klikněte na jednu z položkách, které chcete spustit hledání protokolu vrací všechny chybějící a důležité aktualizace.|
**Aktualizace instanci výsledků**||
Stav instalace instanci | Seznam klasifikace aktualizace, že jsou počítače chybějící seřazené podle počtu počítačů chybějící aktualizace v kategorii. Klikněte na tlačítko Spustit hledání protokolu vrací všechny aktualizace záznamů pro tento počítač v počítači.|  
<br><br> ![Základní informace o aktualizaci nasazení výsledků](./media/oms-solution-update-management/update-la-updaterunresults-page.png)

### <a name="creating-an-update-deployment"></a>Vytváření nasazení aktualizace

Vytvořte nové nasazení aktualizace kliknutím na tlačítko **Přidat** v horní části obrazovky a otevře se stránka **Nový nasazení aktualizace** .  Je nutné zadat hodnoty vlastností v následující tabulce.

Vlastnost | Popis|
----------|----------|
Jméno | Jedinečný název pro identifikaci nasazení aktualizace.|
Časové pásmo | Časové pásmo pro účely počáteční čas.|
Spuštění | Datum a čas zahájení instalace aktualizace.|
Doba trvání | Počet minut, které nasazení aktualizace může spustit.  Pokud všechny aktualizace nejsou nainstalovány v rámci tuto dobu trvání, musíte zbývající aktualizace počkat do další aktualizace nasazení.|
Počítačů | Názvy počítače nebo skupiny počítačů zahrnout nasazení aktualizace.  Vyberte jeden nebo více položek z rozevíracího seznamu.|
<br><br> ![Nová stránka nasazení aktualizace](./media/oms-solution-update-management/update-newupdaterun-page.png)

### <a name="time-range"></a>Časový rozsah

Ve výchozím nastavení je oborem data analyzovat v řešení pro správu aktualizace ze všech skupin připojeného správy generování v poslední 1 den. 

Chcete-li změnit časový rozsah dat, vyberte **na základě dat** v horní části řídicího panelu. Můžete vybrat záznamy vytvořila nebo aktualizovala během posledních 7 dnů, 1 den nebo 6 hodin. Nebo můžete vybrat **vlastní** a zadejte vlastní časové období.<br><br> ![Možnost vlastní časového rozsahu](./media/oms-solution-update-management/update-la-time-range-scope-databasedon.png)  

## <a name="log-analytics-records"></a>Technologie pro analýzu záznamů protokolu

Řešení pro správu aktualizace vytvoří dva typy záznamů v úložišti OMS.

### <a name="update-records"></a>Aktualizace záznamů

Záznam s typem **Aktualizovat** je vytvořena pro každou aktualizaci, která je nainstalovaná nebo potřeby na každém počítači. Aktualizace záznamů mají vlastnosti v následující tabulce.

Vlastnost | Popis|
----------|----------|
Typ | *Aktualizace*|
SourceSystem | Zdroj, který schválené instalaci aktualizace.<br>Možné hodnoty jsou:<br>-Microsoft Update<br>– Windows Update<br>-SCCM<br>-Servery Linux (z balíčku správci načteno)|
Schválení | Určuje, zda aktualizace schválený pro instalaci.<br> Pro Linux servery Toto je nepovinný krok aktuálně jako oprava není spravuje OMS.|
Klasifikace pro Windows | Klasifikace aktualizace.<br>Možné hodnoty jsou:<br>-Aplikace<br>-Důležité aktualizace<br>– Aktualizace definic<br>-Feature Pack<br>– Aktualizace zabezpečení<br>-Service Pack<br>– Kumulativní aktualizace<br>– Aktualizace|
Klasifikace Linux | Cassification aktualizace.<br>Možné hodnoty jsou:<br>-Důležité aktualizace<br>– Aktualizace zabezpečení<br>– Další aktualizace|
Počítač | Název počítače.|
InstallTimeAvailable | Určuje, zda je k dispozici další agentů nainstalované stejná aktualizace průběhu instalace.|
InstallTimePredictionSeconds | Odhadovaná instalaci v sekundách založené na jiných agentů nainstalované stejná aktualizace.|
KBID | Článek ve znalostní bázi Knowledge Base popisující aktualizace ID.|
ManagementGroupName | Název skupiny správy SCOM agentů.  U jiných agenti – jedná AOI -<workspace ID>.|
MSRCBulletinID | ID bulletinu zabezpečení popisující aktualizace.|
MSRCSeverity | Závažnosti bulletinu zabezpečení.<br>Možné hodnoty jsou:<br>-Kritické<br>– Důležité<br>-Moderování|
Volitelné | Určuje, zda je volitelné aktualizace.|
Produktu | Název produktu aktualizace trvá.  Klikněte na **zobrazení** otevřete článek v prohlížeči.|
PackageSeverity | Závažnosti chyby pevně v tuto aktualizaci vykázání dodavatelé distro Linux. | 
PublishDate | Datum a čas, který byl nainstalovaný aktualizace.|
RebootBehavior | Určuje, pokud aktualizace vynutí restartovat počítač.<br>Možné hodnoty jsou:<br>-canrequestreboot<br>-neverreboots|
RevisionNumber | Počet revizí aktualizace.|
SourceComputerId | Identifikátor GUID identifikují počítač.|
TimeGenerated | Datum a čas poslední aktualizace záznamu.|
Název | Název aktualizace.|
UpdateID | Identifikátor GUID identifikují aktualizace.|
UpdateState | Určuje, zda je nainstalována na tomto počítači.<br>Možné hodnoty jsou:<br>-Nainstalovaný – je nainstalována na tomto počítači.<br>– Třeba – aktualizace není nainstalovaný a není potřeba na tomto počítači.|  

<br>
Při hledání všechny protokol, který vrací záznamy s typem **aktualizace** můžete vybrat zobrazení **aktualizace** , které vám zobrazí několik dlaždic shrnutí aktualizace nalezené. Kliknete na položky v **Nenalezeno a použité aktualizace** a **povinných a nepovinných aktualizace** dlaždice oboru zobrazení k této sadě aktualizace. Vyberte zobrazení **seznamu** nebo **tabulce** k vrácení jednotlivých záznamů.<br> 

![Zobrazení protokolu hledání aktualizace aktualizacích typ záznamu](./media/oms-solution-update-management/update-la-view-updates.png)  

V zobrazení **tabulky** můžete kliknout na **KBID** pro libovolný záznam otevřete v prohlížeči v článku znalostní bázi Knowledge Base. Umožňuje rychle číst podrobností o konkrétní aktualizace.<br> 

![Zobrazení tabulky protokolu vyhledávání s aktualizacemi dlaždice typ záznamu](./media/oms-solution-update-management/update-la-view-table.png)

V zobrazení **seznamu** klikněte na odkaz **Zobrazit** vedle KBID otevřete v článku znalostní bázi Knowledge Base.<br>

![Zobrazení seznamu hledání protokolu s aktualizacemi dlaždice typ záznamu](./media/oms-solution-update-management/update-la-view-list.png)


###<a name="updatesummary-records"></a>UpdateSummary záznamů

Záznam s typem **UpdateSummary** se vytvoří pro každý počítač agent Windows. Tento záznam se aktualizuje pokaždé, když v počítači naskenovaný aktualizací. **UpdateSummary** záznamy obsahují vlastností v následující tabulce.

Vlastnost | Popis|
----------|----------|
Typ | UpdateSummary|
SourceSystem | OpsManager |
Počítač | Název počítače.|
CriticalUpdatesMissing | Počet důležité aktualizace chybějící v počítači.|
ManagementGroupName | Název skupiny správy SCOM agentů. U jiných agenti – jedná AOI -<workspace ID>.|
NETRuntimeVersion | Verze modulu runtime .NET na počítači nainstalovaný.|
OldestMissingSecurityUpdateBucket | Byla vydána bloku zařadit do nějaké kategorie času od nejstaršího chybějící aktualizací tohoto počítače.<br>Možné hodnoty jsou:<br>-Starší<br>-180 dny<br>-150 dny<br>-120 dny<br>-90 dnů<br>-60 dny<br>-přejděte 30 dní.<br>-Poslední|
OldestMissingSecurityUpdateInDays | Byla vydána spočítá počet dnů od nejstaršího chybějící aktualizací tohoto počítače.|
OsVersion | Verze operačního systému na počítači nainstalovaný.|
OtherUpdatesMissing | Počet mezi další aktualizace chybějící v počítači.|
SecurityUpdatesMissing | Počet aktualizace zabezpečení chybějící v počítači.|
SourceComputerId | Identifikátor GUID identifikují počítač.|
TimeGenerated | Datum a čas poslední aktualizace záznamu.|
TotalUpdatesMissing |Celkový počet aktualizací chybějící v počítači.|
WindowsUpdateAgentVersion | Číslo verze Windows Update agent v počítači.|
WindowsUpdateSetting | Jak se počítač nainstalovat důležité aktualizace nastavení.<br>Možné hodnoty jsou:<br>-Zakázaný<br>-Upozornit před instalací<br>-Plánované instalace|
WSUSServer | Pokud počítač nakonfigurovaný pro použití jedné adresy URL WSUS server.|  

## <a name="sample-log-searches"></a>Ukázka protokolu hledání

Následující tabulka obsahuje ukázkové protokolu vyhledá aktualizovat záznamy shromážděná toto řešení. 

Dotaz | Popis|
----------|----------|
Všechny počítače s chybějící aktualizace | Typ = aktualizace UpdateState = potřeby volitelné = false & #124; Vyberte počítač, nadpis, KBID, zařazení, UpdateSeverity, PublishedDate|
Chybějící aktualizace pro počítač "COMPUTER01.contoso.com" (nahradit názvu počítače) | Typ = aktualizace UpdateState = potřeby volitelné = false počítač = "COMPUTER01.contoso.com" & #124; Vyberte počítač, nadpis, KBID, produktů, UpdateSeverity, PublishedDate|
Všechny počítače s chybějícím kritické a aktualizace zabezpečení | Typ = aktualizace UpdateState = potřeby volitelné rozdělení (klasifikace = "Aktualizace zabezpečení" nebo klasifikace = "Důležité aktualizace")|
Pole Kritický nebo zabezpečení aktualizací potřeby tak, že počítačích ručně použití aktualizací | Typ = UpdateState aktualizace = potřeby volitelné rozdělení (klasifikace = "Aktualizace zabezpečení" nebo klasifikace = "Důležité aktualizace") v počítači {typ = UpdateSummary WindowsUpdateSetting = ruční & #124; Jednoznačné počítači} & #124; Jednoznačné KBID|
Chybové události stroje, které chybí kritické nebo zabezpečení požadovaných aktualizací | Typ = události EventLevelName = chyby ve počítače {typ = aktualizace (klasifikace = "Aktualizace zabezpečení" nebo klasifikace = "Důležité aktualizace") UpdateState = potřeby volitelné = false & #124; Jednoznačné počítači}|
Kumulativní aktualizace všech počítačů s chybějícím | Typ = volitelné aktualizace = false klasifikace = "Kumulativní aktualizace" UpdateState = potřeby & #124; Vyberte počítač, nadpis, KBID, zařazení, UpdateSeverity, PublishedDate|
Různých chybějícími aktualizace ve všech počítačích | Typ = aktualizace UpdateState = potřeby volitelné = false & #124; Jednoznačné název|
Členství ve počítače WSUS | Typ = UpdateSummary & #124; míra count() tak, že WSUSServer|
Konfigurace automatické aktualizace | Typ = UpdateSummary & #124; míra count() tak, že WindowsUpdateSetting|
Počítačích s automatické aktualizace zakázaný | Typ = UpdateSummary WindowsUpdateSetting = ruční|  
Seznam všech počítačů Linux, které mají k dispozici balíček aktualizace | Typ = aktualizace a OSType = Linux a UpdateState! = "Není potřeba" & #124; míra count() ve počítače|
Seznam všech počítačů Linux, které mají k dispozici balíček aktualizace adresy zabezpečení nebo kritické chyby | Typ = aktualizace a OSType = Linux a UpdateState! = "Není potřeba" a (klasifikace = "Důležité aktualizace" nebo klasifikace = "Aktualizace zabezpečení") & #124; míra count() ve počítače|
Seznam všech balíčků, které mají k dispozici aktualizace | Typ = aktualizace a OSType = Linux a UpdateState! = "Není potřeba"|
Seznam všech balíčků, které mají k dispozici aktualizace, který řeší zabezpečení nebo kritické chyby | Typ = aktualizace a OSType = Linux a UpdateState! = "Není potřeba" a (klasifikace = "Důležité aktualizace" nebo klasifikace = "Aktualizace zabezpečení")|
Seznam všech počítačů "Se systémem Ubuntu" s všechny dostupné aktualizace | Typ = aktualizace a OSType = Linux a OSName = systémem Ubuntu & #124; míra count() ve počítače|

## <a name="next-steps"></a>Další kroky

- Pomocí protokolu vyhledávání v [Protokolu analýzy](../log-analytics/log-analytics-log-searches.md) zobrazíte podrobné aktualizovat data.

- [Vytvářet své vlastní řídicí panely](../log-analytics/log-analytics-dashboards.md) zobrazující aktualizace dodržování předpisů v rámci spravovaných počítačů.

- [Vytvořit upozornění](../log-analytics/log-analytics-alerts.md) při důležité aktualizace zjištění jako chybějící z počítače nebo počítač má automatické aktualizace jsou zakázány.  




