<properties
    pageTitle="Velkokapacitní řešení pro správu v protokolu analýzy | Microsoft Azure"
    description="Řešení plánování kapacity můžete použít v protokolu analýzy vám pomůže pochopit kapacita serverech Hyper-V spravovaných ve počítače virtuální správce pro System Center"
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="capacity-management-solution-in-log-analytics"></a>Velkokapacitní řešení pro správu v protokolu analýzy


Řešení plánování kapacity můžete v protokolu analýzy vám pomůže pochopit kapacita serverech Hyper-V spravovaných ve počítače virtuální správce pro System Center. Toto řešení vyžaduje System Center Operations Manager a správce pro System Center virtuálního počítače. Kapacita plánování není dostupné, pokud používáte jenom přímo připojené agentů. Nainstalovat řešení aktualizovat agenta Operations Manager. Řešení přečte výkonnosti na sledovaný server a odešle použití zásad správy informací ke službě OMS v cloudu pro zpracování. Použití logických operátorů je použité pro použití data a cloudovou službu záznamů data. V průběhu času jsou určeny vzorce využití a plánované doby podle aktuální spotřeba.

Projekci může například zjistit, jestli jádra Další procesoru nebo další paměti bude potřebovat pro jednotlivé server. V tomto příkladu může promítání označuje, že 30 dní serveru musí další paměti. To může pomoci při plánování upgradu na paměti při serveru další údržby, které může dojít k každé dva týdny.

>[AZURE.NOTE] Řešení pro správu kapacita není k dispozici které budou přidány do pracovních prostorech. Zákazníci, kteří mají řešení pro správu kapacita nainstalovaný můžete dál používat řešení.  

Plánování řešení kapacity probíhá aktualizované při řešení těchto zákazníka vykázaného úkoly:

- Požadavek na používání Správce virtuálního počítače a Operations Manager
- Nelze upravit nebo filtr podle skupin
- Každou hodinu agregace dat není častěji nestačí
- Úrovně přehledy žádné OM
- Spolehlivost dat.

Výhody nové řešení kapacita:

- Podpora shromažďování podrobného dat s vyšší spolehlivost a přesnosti
- Podpora pro Hyper-V bez nutnosti VMM
- Vizualizace metriky v PowerBI
- Další informace na úrovni využití OM


## <a name="installing-and-configuring-the-solution"></a>Instalace a konfigurace řešení
Instalace a konfigurace řešení použijte následující informace.

- Operations Manager je potřebný pro řízení kapacity řešení.
- Virtuální Machine Manager je potřebný pro řízení kapacity řešení.
- Je vyžadováno připojení správce operace s Virtual Machine Manager (VMM). Další informace o připojení v systémech najdete v článku [jak připojit VMM s Operations Manager](http://technet.microsoft.com/library/hh882396.aspx).
- Operations Manager musí být připojená ke protokolu analýzy.
- Řešení pro správu kapacita dodejte OMS pracovního prostoru pomocí proces popsaný v [řešení přidat analýzy protokolu z Galerie řešení](log-analytics-add-solutions.md).  Není nutná žádná další konfigurace.


## <a name="capacity-management-data-collection-details"></a>Podrobnosti kolekce kapacity správy dat

Kapacity správy shromáždí data o výkonu, metadat a používání agentů, které jste zapnuli automatický přístup data stavu.

Následující tabulka zobrazuje metody shromažďování dat a další podrobnosti o jak údaje pro řízení kapacity.

| platformy | Přímé Agent | SCOM agent | Azure úložiště | SCOM povinné? | Data agent SCOM odeslaná pomocí správy skupiny | kolekce četnosti |
|---|---|---|---|---|---|---|
|Windows|![Ne](./media/log-analytics-capacity/oms-bullet-red.png)|![Ano](./media/log-analytics-capacity/oms-bullet-green.png)|![Ne](./media/log-analytics-capacity/oms-bullet-red.png)|            ![Ano](./media/log-analytics-capacity/oms-bullet-green.png)|![Ano](./media/log-analytics-capacity/oms-bullet-green.png)| každou hodinu|

V následující tabulce jsou uvedeny příklady datových typů shromážděná řízení kapacity:

|**Datový typ**|**Pole**|
|---|---|
|Metadata|BaseManagedEntityId, ObjectStatus, názvu, ActiveDirectoryObjectSid, PhysicalProcessors, NázevSítě, adresa IP, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP adresu, NetbiosDomainName, LogicalProcessors, Název_dns, DisplayName, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime|
|Výkon|Název_objektu název_čítače, PerfmonInstanceName, PerformanceDataId, PerformanceSourceInternalID, SampleValue, TimeSampled, TimeAdded|
|Stav|StateChangeEventId StateId, NewHealthState, OldHealthState, kontext, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, změněno, LastGreenAlertGenerated, DatabaseTimeModified|

## <a name="capacity-management-page"></a>Kapacita stránku pro správu


 Po instalaci řešení plánování kapacity uvidí kapacita serverech sledované pomocí dlaždici **Plánování kapacity** na stránce **Přehled** v OMS.

![Obrázek rohu dlaždice plánování kapacity](./media/log-analytics-capacity/oms-capacity01.png)

Na dlaždici otevře řídicí panel pro **Správu kapacitu** , kde můžete zobrazit přehled kapacity serveru. Na stránce se zobrazí následující dlaždice, které můžete kliknout na:

- *Počet virtuálního počítače*: Zobrazí počet dní zbývající kapacity virtuálních počítačích
- *Výpočet*: zobrazuje jádra procesoru a paměti
- *Úložiště*: zobrazuje využití místa na disku a Průměrná latence disku
- *Hledání*: Průzkumník dat, které můžete použít k vyhledání všechna data v systému OMS

![Obrázek řídicího panelu pro správu kapacity](./media/log-analytics-capacity/oms-capacity02.png)


### <a name="to-view-a-capacity-page"></a>Pokud chcete zobrazit stránku kapacity

- Na stránce **Přehled** klikněte na **Řízení kapacity**a potom klikněte na **Výpočet** nebo **úložiště**.

## <a name="compute-page"></a>Výpočet stránky

**Výpočet** řídicího panelu v Microsoft Azure OMS slouží k zobrazení kapacita informací o využití, plánované dní kapacity a efektivitu související s infrastrukturu. Oblasti **využití** slouží k zobrazení využití procesoru core a velikosti paměti v hosts virtuálního počítače. Nástroj projekce slouží k odhadu kolik kapacita očekává se, aby byl k dispozici pro dané období. Použít oblast **efektivity** a zjistěte, jak efektivně jsou hosts virtuálního počítače. Zobrazit údaje o propojené položky tak, že na ně kliknete.

Si můžete vygenerovat sešitu aplikace Excel pro následující kategorie:

- Začátek hosts s nejvyšší core využití
- Začátek hosts s nejvyšší využití paměti
- Začátek hosts s neefektivní virtuálních počítačích
- Začátek hosts tak, že využití
- Dolní hosts tak, že využití

![Obrázek stránky výpočet správy kapacity](./media/log-analytics-capacity/oms-capacity03.png)


Na řídicím panelu **Výpočet** jsou zobrazené následujících oblastí:

**Využití**: zobrazení procesoru core a velikosti paměti využití v hosts virtuálního počítače.

- *Použít jádra*: součet všem hostitelé (% procesoru vynásobené počet fyzické jádra na hostitele).
- *Bezplatné jádra*: celkem fyzické jádra mínus použité jádra.
- *K dispozici jádra procento*: uvolnit fyzické jádra děleno počet fyzické jádra.
- *Virtual vzorky za OM*: celkové virtuálních jádra v systému děleno počet virtuálních počítačích v systému.
- *Virtual vzorky fyzické jádra stran*: poměr celkové fyzické jádra do pole fyzicky jádra, které využívají virtuálních počítačích v systému.
- *Počet dostupných virtual vzorky*: virtuální základní fyzické jádra poměr vynásobené dostupné fyzické jádra.
- *Použít paměti*: součet paměti, která je využívána všem hostitelé.
- *Bezplatné paměti*: celkem fyzické paměti minus použitá paměť.
- *Procento paměti dostupné*: uvolnit fyzické paměti vydělí celkové fyzické paměti.
- *Virtuální paměť za OM*: celkové virtuálních paměti v systému děleno počet virtuálních počítačích v systému.
- *Virtuální paměť fyzické paměti stran*: celkové virtuálních paměti v systému vydělí celkové fyzické paměti v systému.
- *K dispozici virtuální paměť*: virtuální paměť fyzické paměti poměr vynásobené dostupnou pamětí fyzické.

**Nástroj projekce**

Pomocí nástroje projekce uvidí historickými trendů pro využití prostředků. Platí to i pro použití trendů pro virtuálních počítačích, paměti, hlavní a úložiště. Funkce projekce pomocí algoritmu projekce zjistíte, když jsou spuštěné mimo všechny zdroje. Pomůže vám to výpočtu správné kapacity plánování tak, aby mohli vědět, kdy budete muset koupit další kapacita (například paměti jádra a úložiště).

**Efektivity**

- *Nečinné OM*: pomocí menší než 10 % paměti vytížení procesoru a 10 % pro zadané období.
- *Overutilized OM*: pomocí víc než 90 % paměti vytížení procesoru a 90 % zadané doby trvání.
- *Host (hostitel) nečinný*: pomocí menší než 10 % paměti vytížení procesoru a 10 % pro zadané období.
- *Host (hostitel) overutilized*: pomocí víc než 90 % paměti vytížení procesoru a 90 % zadané doby trvání.

### <a name="to-work-with-items-on-the-compute-page"></a>Můžete pracovat s položkami na stránce pro využití

1. Na řídicím panelu **Výpočet** v oblasti **využití** zobrazení kapacita informací o jádra procesoru a paměti používá.
2. Klikněte na položku, kterou chcete otevřít na stránce **vyhledávání** a zobrazit podrobné informace o ho.
3. V nástroji **projekce** posuňte jezdec datum zobrazíte projekci kapacitu, která se použije na datum, které zvolíte.
4. V oblasti **efektivity** zobrazte kapacitu efektivity informace o virtuálních počítačích a hostitelů virtuálních počítačů.

## <a name="direct-attached-storage-page"></a>Přímé stránce připojené úložiště

Řídicí panel **Přímo připojené úložiště** v OMS slouží k zobrazení kapacita informací o využití úložiště, výkon a plánované dnů kapacitu disku. Oblasti **využití** slouží k zobrazení místo na disku v hosts virtuálního počítače. Použít oblast **Výkon** zobrazíte disku výkonu a latence v hosts virtuálního počítače. Můžete také použít nástroj projekce k odhadu kolik kapacita očekává se, aby byl k dispozici pro dané období. Zobrazit údaje o propojené položky tak, že na ně kliknete.

Sešit aplikace Excel můžete vygenerovat z těchto informací kapacita pro následující kategorie:

- Začátek místo na disku hostitelem
- Začátek průměrnou latenci hostitelem

Na stránce **úložiště** jsou zobrazené následujících oblastí:

- *Využití*: zobrazení místo na disku v hosts virtuálního počítače.
- *Celkový na disku*: SUMA (logické na disku) pro všechny hostitelů
- *Použít místo na disku*: SUMA (logické disku) pro všechny hostitelů
- *Dostupné místo na disku*: celkový na disku mínus místa na disku
- *Procento používá disku*: použít vydělí celkové místa na disku
- *K dispozici disku procento*: volného místa na disku vydělí celkové místo na disku

![Obrázek stránky správy přímé připojené kapacitou](./media/log-analytics-capacity/oms-capacity04.png)

**Výkon**

Pomocí OMS můžete zobrazit historii použití trend místa na disku. Projekce schopností používá algoritmus pro budoucí použití projektu. Pro využití místa zejména projekce schopností umožňuje projektu při může docházet místa na disku. To vám pomůže naplánovat správné úložiště a vědět, když potřebujete zakoupit další úložiště.

**Nástroj projekce**

Pomocí nástroje projekce uvidí historických trendů pro využití místa disku. Projekce schopností umožňuje taky projektu při dost místa na disku. To vám umožní plánování správné kapacitu a vědět, když potřebujete koupit další kapacitu úložiště.

### <a name="to-work-with-items-on-the-direct-attached-storage-page"></a>Můžete pracovat s položkami na stránce přímo připojené úložiště

1. Na řídicím panelu **Přímo připojené úložiště** v oblasti **využití** můžete zobrazit informace o využití disku.
2. Klikněte na propojená položka otevřít v stránce **hledání** a zobrazení podrobných informací o ho.
3. Ve skupinovém rámečku **Disku výkon** můžete zobrazit informace o výkonu a latence disku.
4. **Nástroj projekce**posuňte jezdec datum zobrazíte projekci kapacitu, která se použije na datum, které zvolíte.


## <a name="next-steps"></a>Další kroky

- Slouží k zobrazení Podrobný kapacitu správy dat [protokolu vyhledávání v protokolu analýzy](log-analytics-log-searches.md) .
