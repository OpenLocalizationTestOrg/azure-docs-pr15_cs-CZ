<properties 
    pageTitle="Sledování a správa Azure Data Factory potrubí" 
    description="Naučte se používat ke sledování a správa dat Azure továrny a potrubí sledování a Správa aplikací." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016" 
    ms.author="spelluru"/>

# <a name="monitor-and-manage-azure-data-factory-pipelines-using-new-monitoring-and-management-app"></a>Sledování a správa Azure Data Factory potrubí pomocí nových sledování a Správa aplikací
> [AZURE.SELECTOR]
- [Pomocí prostředí PowerShell Azure portál/Azure](data-factory-monitor-manage-pipelines.md)
- [Pomocí sledování a Správa aplikací](data-factory-monitor-manage-app.md)

Tento článek popisuje, jak sledovat, Správa a ladění vaší kanály a vytvoření upozornění pro oznámení o selhání pomocí **sledování a Správa aplikací**. Můžete se taky podívat následující video informace o používání sledování a Správa aplikací.
   

> [AZURE.VIDEO azure-data-factory-monitoring-and-managing-big-data-piplines]
      
## <a name="launching-the-monitoring-and-management-app-a"></a>Spuštění sledování a Správa aplikací
Spuštění sledování a Správa aplikací klikněte na dlaždici **sledování a spravovat** na zásuvné **DATA FACTORY** pro vaše data factory.

![Sledování dlaždice na domovské stránce Data Factory](./media/data-factory-monitor-manage-app/MonitoringAppTile.png) 

Měli byste vidět, že sledování a Správa aplikací spuštění v samostatném okně/kartu.  

![Sledování a Správa aplikací](./media/data-factory-monitor-manage-app/AppLaunched.png)

> [AZURE.NOTE] Pokud se zobrazí, že webového prohlížeče se zarazí na "Autorizace...", **Blokovat cookies třetích stran a data webu** nastavení zakázat nebo zrušte zaškrtnutí políčka (nebo) v jednoduchosti je povolený vytvořit výjimku pro **login.microsoftonline.com** a pak to zkuste znovu spuštění aplikace.


Pokud nevidíte windows aktivity v seznamu dole, klikněte na tlačítko **Aktualizovat** na panelu nástrojů a aktualizujte seznam. Kromě toho nastavte správné hodnoty filtry **Počáteční čas** a **koncový čas** .  


## <a name="understanding-the-monitoring-and-management-app"></a>Principy, sledování a Správa aplikací
Existují tři karty (**Zdroje Explorer** **Sledování zobrazení**a **oznámení**) na levé straně a na první kartu (zdroje Explorer) je vybrané ve výchozím nastavení. 

### <a name="resource-explorer"></a>Průzkumník zdroje
Zobrazí se následující: 

- Zdroje Průzkumníka **stromového zobrazení** v levém podokně.
- **Zobrazení diagramu** nahoře.
- Seznam **Aktivity Windows** dole v prostředním podokně.
- **Vlastnosti**/**Aktivitu okno Průzkumníka** karty v pravém podokně. 

V Průzkumníku zdroje se zobrazí všechny zdroje (kanály, datových sad a propojené služby) v factory dat ve stromovém zobrazení. Když vyberete objekt v Průzkumníku zdroje, zjistíte takto: 

- Přidružená entita Data Factory zvýrazněná v zobrazení diagramu.
- související aktivity windows (klikněte na [zde](data-factory-scheduling-and-execution.md) se dozvíte o aktivitách windows) jsou zvýrazněné v seznamu aktivity Windows dole.  
- vlastnosti vybraného objektu v okně Vlastnosti v podokně vpravo. 
- Definice JSON vybraného objektu v případě potřeby. Příklad: propojené služby nebo datovou sadu nebo kanálů. 

![Průzkumník zdroje](./media/data-factory-monitor-manage-app/ResourceExplorer.png)

Najdete v článku [plánování a provádění](data-factory-scheduling-and-execution.md) článku podrobné informace o aktivitách okna. 

### <a name="diagram-view"></a>Zobrazení diagramu
Zobrazení diagramu factory dat obsahuje jedno podokno sklenice ke sledování a správa dat továrny a jeho prostředky. Když vyberete Data Factory entity (datovou sadu/kanálem k odesílání zpráv) v zobrazení diagramu, zjistíte takto:
 
- Entita factory dat je vybraná ve stromovém zobrazení
- v seznamu aktivity Windows jsou zvýrazněné přidruženou aktivitu windows.
- vlastnosti vybraného objektu v okně vlastností

Při zapnuté funkci kanálu (ale ne v pozastaveném stavu), se zobrazují zelené řádku, díky kterému. 

![Kanálem k odesílání zpráv spuštění](./media/data-factory-monitor-manage-app/PipelineRunning.png)

Zjistíte, že jsou tři příkazová tlačítka profilace v zobrazení diagramu. Tlačítko druhého slouží k pozastavení kanálu. Pozastavení ukončit aktuálně spuštěných aktivity a nechat je přejít k dokončení. Třetí tlačítko pozastaví kanálu a ukončí stávající provádění aktivity. První tlačítko obnoví kanálu. V případě seznamu příležitostí je zjistíte, změna barvy profilace dlaždice následujícím způsobem.

![Pozastavit nebo životopis na dlaždici](./media/data-factory-monitor-manage-app/SuspendResumeOnTile.png)

Můžete vícenásobný výběr dvě nebo více potrubí (pomocí CTRL) a pomocí panelu příkazů pozastavit nebo životopis více potrubí najednou.

![Pozastavit nebo životopis na panelu s příkazy](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)

Zobrazí se všechny aktivity v kanálu, tak, že kliknete pravým tlačítkem myši na dlaždici kanálem k odesílání zpráv a kliknutím na **Otevřít kanálem k odesílání zpráv**.

![Otevření nabídky kanálem k odesílání zpráv](./media/data-factory-monitor-manage-app/OpenPipelineMenu.png)

V zobrazení otevřených kanálem k odesílání zpráv najdete v článku všechny aktivity v kanálu. V tomto příkladu je pouze jeden aktivity: Kopírovat aktivity. Vrátit na předchozí zobrazení, klikněte na název factory dat v nabídce navigace s popisem cesty v horní.

![Otevřená kanálem k odesílání zpráv](./media/data-factory-monitor-manage-app/OpenedPipeline.png)

V zobrazení kanálem k odesílání zpráv když kliknete na datovou sadu výstup nebo kdy můžete přesunutím ukazatele myši datovou sadu výstup se zobrazí automaticky otevírané okno aktivity Windows pro tuto sadu.

![Aktivita v oknech](./media/data-factory-monitor-manage-app/ActivityWindowsPopup.png)

Klikněte na okno aktivity zobrazíte podrobnosti jej v okně **Vlastnosti** v podokně vpravo. 

![Okno Vlastnosti činnosti](./media/data-factory-monitor-manage-app/ActivityWindowProperties.png)

V pravém podokně přejděte **aktivity okna** Průzkumník zobrazíte více podrobností.

![Okno Průzkumníka aktivity](./media/data-factory-monitor-manage-app/ActivityWindowExplorer.png) 

Taky uvidíte **vyřešit proměnné** pro každé spuštění aktivity pokusí v části **pokusy** . 

![Vyřešení proměnných](./media/data-factory-monitor-manage-app/ResolvedVariables.PNG)

Přejděte na kartu **skript** zobrazíte definici JSON skriptu pro vybraný objekt.   

![Karta skriptu](./media/data-factory-monitor-manage-app/ScriptTab.png)

Zobrazí se aktivity windows ve třech umístěních:

- Aktivity Windows automaticky otevírané okno v zobrazení diagramu (prostředním podokně).
- Okno Průzkumníka aktivity v pravém podokně.
- Seznam Windows aktivity v dolní části okna.

V místní systém Windows aktivity a aktivity okno Průzkumníka že se posunete do předchozího týdne a příští týden pomocí šipek vlevo a vpravo.

![Aktivity okno Průzkumníka vlevo/vpravo šipky](./media/data-factory-monitor-manage-app/ActivityWindowExplorerLeftRightArrows.png)

V dolní části zobrazení diagramu se zobrazí tlačítka pro zvětšení přiblížit, oddálit, přizpůsobit, zvětšit 100 %, zamknout rozložení. Tlačítko rozložení zámku zabrání omylem přesunutí tabulky a potrubí v zobrazení diagramu a je ve výchozím nastavení zapnuto. Můžete vypnout a pohyb entity v diagramu. Pokud jste tuto možnost vypněte, můžete umístit automaticky tabulek a potrubí tlačítko poslední. Taky můžete přiblížit nebo oddálit pomocí kolečkem myši.

![Diagram příkazy zvětšení zobrazení](./media/data-factory-monitor-manage-app/DiagramViewZoomCommands.png)


### <a name="activity-windows-list"></a>Seznam Windows aktivit
V seznamu windows aktivity v dolní části podokna uprostřed se zobrazí všechna okna aktivity pro datovou sadu, kterou jste vybrali v Průzkumníkovi zdrojů nebo zobrazení diagramu. Ve výchozím nastavení je seznam v sestupném pořadí, což znamená, že se zobrazí okno nejnovější aktivity v horní. 

![Seznam Windows aktivit](./media/data-factory-monitor-manage-app/ActivityWindowsList.png)

Tento seznam nemá automaticky aktualizovat, takže použijte tlačítko Aktualizovat na panelu nástrojů pro ruční aktualizaci.  


Aktivity windows může být v některém z následujících stavů:

<table>
<tr>
    <th align="left">Stav</th><th align="left">Dílčí stav</th><th align="left">Popis</th>
</tr>
<tr>
    <td rowspan="8">Čekání</td><td>ScheduleTime</td><td>Čas nebyla přechodu do okna aktivity ke spuštění.</td>
</tr>
<tr>
<td>DatasetDependencies</td><td>Odesílání dat závislosti připraveni není.</td>
</tr>
<tr>
<td>ComputeResources</td><td>Pro využití prostředků nejsou k dispozici.</td>
</tr>
<tr>
<td>ConcurrencyLimit</td> <td>Všechny instance aktivity jsou něčem pracuje s ostatními okny aktivity.</td>
</tr>
<tr>
<td>ActivityResume</td><td>Aktivity se pozastaví a nedá spustit windows aktivity, dokud je obnoven.</td>
</tr>
<tr>
<td>Opakovat</td><td>Opakované spuštění aktivity.</td>
</tr>
<tr>
<td>Ověření</td><td>Ověření ona nezačne.</td>
</tr>
<tr>
<td>ValidationRetry</td><td>Čekání na ověření k opakování.</td>
</tr>
<tr>
<tr
<td rowspan="2">Probíhající</td><td>Ověřování</td><td>Ověření průběhu.</td>
</tr>
<td></td>
<td>Okno aktivit zpracovávání.</td>
</tr>
<tr>
<td rowspan="4">Se nezdařila.</td><td>TimedOut</td><td>Spuštění přijal delší než, která je povolené na základě aktivity.</td>
</tr>
<tr>
<td>Zrušení</td><td>Zrušeno akci uživatele.</td>
</tr>
<tr>
<td>Ověření</td><td>Ověření se nezdařilo.</td>
</tr>
<tr>
<td></td><td>Nepovedlo se generovat a/nebo ověřit okna aktivity.</td>
</tr>
<td>Jste připravení</td><td></td><td>Okno aktivit je připraven k spotřebu.</td>
</tr>
<tr>
<td>Přeskočilo</td><td></td><td>Okno aktivit není zpracovat.</td>
</tr>
<tr>
<td>Žádná</td><td></td><td>Okno činnosti, které používá existují jiný stav, ale resetování.</td>
</tr>
</table>


Po klepnutí na tlačítko okno aktivity v seznamu zobrazit podrobnosti o ní v okně **Průzkumníka Windows aktivity** nebo **Vlastnosti** na pravé straně.

![Okno Průzkumníka aktivity](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-2.png)

### <a name="refresh-activity-windows"></a>Aktualizace windows aktivity  
Podrobnosti nejsou automaticky aktualizovány, abyste používat **aktualizace** tlačítka (tlačítko druhého) na panelu s příkazy pro ruční aktualizaci seznamu windows aktivit.  
 

### <a name="properties-window"></a>Okno Vlastnosti
V podokně pravý krajní aplikace pro sledování a správa je okno Vlastnosti. 

![Okno Vlastnosti](./media/data-factory-monitor-manage-app/PropertiesWindow.png)

Zobrazí vlastnosti pro položku, kterou jste vybrali v zdroje explorer (stromové zobrazení) (nebo) diagram zobrazení (nebo) seznam windows aktivit. 

### <a name="activity-window-explorer"></a>Okno Průzkumníka aktivity

**Aktivity okno** Průzkumníka se v podokně pravý krajní sledování a Správa aplikací. Zobrazí podrobnosti o okně činnosti, které jste vybrali v seznamu aktivity Windows místní nebo aktivity Windows. 

![Okno Průzkumníka aktivity](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-3.png)

Můžete přepnout do jiného aktivity okna tak, že na ni kliknete v zobrazení Kalendář v horní. Můžete také na **šipku vlevo**/tlačítka**Šipka vpravo** nahoře zobrazíte aktivity windows z předchozí/následující týden.

Tlačítka panelu nástrojů v dolním podokně **znovu** okna aktivity nebo **Aktualizovat** můžete použít v podokně Podrobnosti. 

### <a name="script"></a>Skript 
Karta **skript** k zobrazení JSON definici vybraná Data Factory entita (propojené služby datovou sadu a kanálem k odesílání zpráv). 

![Karta skriptu](./media/data-factory-monitor-manage-app/ScriptTab.png)

## <a name="using-system-views"></a>Použití zobrazení systému
Sledování a Správa aplikací obsahuje zobrazení předdefinovaných systému (**poslední aktivity windows**, **windows se nezdařilo aktivity**, **Probíhá aktivity windows**), které můžete zobrazit posledních/se nepodařilo/probíhající aktivity windows pro vaše data factory. 

Přejděte na kartu **Zobrazení sledování** na levé straně kliknutím na něj. 

![Karta zobrazení sledování](./media/data-factory-monitor-manage-app/MonitoringViewsTab.png)

V současné době jsou tři zobrazení systémové podporované. Vyberte možnost vidět poslední aktivity windows (nebo) selhalo aktivity windows (nebo) windows v průběhu aktivity v seznamu aktivity Windows (v dolní části podokna uprostřed). 

Když vyberete možnost **poslední aktivity windows** , uvidíte všechna okna poslední aktivity v sestupném pořadí **poslední pokusí čas**. 

Můžete zobrazit **Vadný aktivity windows** zobrazíte všechna okna selhalo aktivity v seznamu. Selhalo aktivity okno vyberte v seznamu zobrazíte její podrobnosti v okně **Vlastnosti** (nebo) **Aktivity okno Průzkumníka**. Můžete také stáhnout všechny protokoly pro okno selhalo aktivity. 


## <a name="sorting-and-filtering-activity-windows"></a>Řazení a filtrování aktivitu windows
Změna nastavení **Počáteční čas** a **koncový čas** v panelu s příkazy k windows aktivitu filtr. Po změně počáteční čas a koncový čas, klikněte na tlačítko vedle čas ukončení aktualizujte seznam aktivity Windows.

![Počáteční a koncové časy](./media/data-factory-monitor-manage-app/StartAndEndTimes.png)

> [AZURE.NOTE] V současné době vždy jsou ve formátu času UTC ve sledování a Správa aplikací. 

V **seznamu aktivity Windows**, klikněte na název sloupce (například: stav). 

![Nabídka sloupce Windows seznam aktivity](./media/data-factory-monitor-manage-app/ActivityWindowsListColumnMenu.png)

Můžete udělat toto:

- Řadit ve vzestupném pořadí.
- Řazení v sestupném pořadí.
- Filtrování podle jednoho nebo více hodnot (jsou připravené, čekání atd.)

Pokud zadáte filtr na nějaký sloupec, se zobrazí na tlačítko Filtr povoleno pro daný sloupec označíte, že jsou hodnoty ve sloupci filtrovaných hodnot. 

![Filtrování sloupce seznam aktivity Windows](./media/data-factory-monitor-manage-app/ActivityWindowsListFilterInColumn.png)

Pokud chcete vymazat filtry můžete stejné automaticky otevírané okno. Vymazání všech filtrů pro seznam aktivity windows, klikněte na tlačítko Vymazat filtr na panelu s příkazy. 

![Vymazání všech filtrů v seznamu aktivitu Windows](./media/data-factory-monitor-manage-app/ClearAllFiltersActivityWindowsList.png)


## <a name="performing-batch-actions"></a>Provádění akcí dávku

### <a name="rerun-selected-activity-windows"></a>Spusťte windows vybranou aktivitu
Vyberte okno činnost, klikněte na šipku dolů u prvního příkazové tlačítko panelu a vyberte **znovu spustit** / **spusťte s před v kanálu**. Když vyberete možnost **znovu s před v kanálu** , znovu spustí všechna okna nadřazeného aktivity stejně. 
    ![Znovu spustit okno aktivity](./media/data-factory-monitor-manage-app/ReRunSlice.png)

Můžete také vybrat více oken aktivity v seznamu a znovu ve stejnou dobu. Můžete filtrovat podle stavu systému windows aktivitu (například: **Neúspěšný**) a pak znovu spusťte windows selhalo aktivitu po opravě problém, který způsobuje windows aktivitu selhání. Následující části Podrobnosti o filtrování windows aktivity v seznamu.  

### <a name="pauseresume-multiple-pipelines"></a>Pozastavit nebo životopis více potrubí
Můžete vícenásobný výběr dvě nebo více potrubí (pomocí CTRL) a pomocí panelu příkazů (zvýrazněné v červené obdélník na následujícím obrázku) pozastavit nebo životopis je po druhém.

![Pozastavení nebo životopis na panelu s příkazy](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)

## <a name="creating-alerts"></a>Vytvoření upozornění 
Na stránce upozornění umožňuje vytvořit upozornění, zobrazení, úpravy nebo odstranění existující upozornění. Můžete taky zakázat/povolíte upozornění. Chcete-li zobrazit upozornění na stránku, klikněte na kartu výstrahy.

![Kartu výstrahy](./media/data-factory-monitor-manage-app/AlertsTab.png)

### <a name="to-create-an-alert"></a>Vytvořit upozornění

1. Kliknutí na **Upozornění na Přidat** přidáte upozornění. Zobrazí se stránka Podrobnosti. 

    ![Vytvořit upozornění – stránka Podrobnosti](./media/data-factory-monitor-manage-app/CreateAlertDetailsPage.png)
1. Zadejte **název** a **Popis** pro upozornění a klikněte na tlačítko **Další**. Byste měli vidět stránce **filtry** .

    ![Vytvořit upozornění - filtry stránky](./media/data-factory-monitor-manage-app/CreateAlertFiltersPage.png)

2. Vyberte **událost**, **Stav**a **dílčí stav** (volitelné) podle kterého chcete službu Data Factory vás upozorní a klikněte na tlačítko **Další**. Byste měli vidět stránce **příjemce** .

    ![Vytvořit upozornění - příjemci stránky](./media/data-factory-monitor-manage-app/CreateAlertRecipientsPage.png) 
3. Vyberte možnost **správce e-mailu předplatné** a/nebo zadejte **Další správce e-mailu**a klikněte na tlačítko **Dokončit**. Měli byste vidět oznámení v seznamu. 
    
    ![Seznam oznámení](./media/data-factory-monitor-manage-app/AlertsList.png)

V seznamu oznámení pomocí tlačítek u upozornění na upravit nebo odstranit/vypnout nebo zapnout upozornění. 

### <a name="eventstatussubstatus"></a>Události a stav/podřízeného stavu
Následující tabulka obsahuje seznam dostupných událostí a stavy (a dílčí stavy).

Název události | Stav | Stav Sub
-------------- | ------ | ----------
Aktivity spustit Začínáme | Začínáme | Spuštění
Aktivity spustit dokončení | Úspěšně | Úspěšně 
Aktivity spustit dokončení | Se nezdařila.| Přidělení zdrojů nezdařeném uložení<br/><br/>Spuštění nezdařeném uložení<br/><br/>Vypršení časového limitu<br/><br/>Neúspěšné ověření<br/><br/>Opuštěné
Vytvoření obrázku na vyžádání HDI Začínáme | Začínáme | &nbsp; |
Na vyžádání HDI clusteru úspěšně vytvořena | Úspěšně | &nbsp; |
Na vyžádání HDI clusteru odstranit | Úspěšně | &nbsp; |
### <a name="to-editdeletedisable-an-alert"></a>Chcete-li upravit nebo odstranit/zakázat upozornění


![Tlačítka upozornění](./media/data-factory-monitor-manage-app/AlertButtons.png)



    
 


