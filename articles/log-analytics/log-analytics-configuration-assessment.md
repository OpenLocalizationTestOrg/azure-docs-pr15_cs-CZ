<properties
    pageTitle="Konfigurace hodnocení řešení v protokolu analýzy | Microsoft Azure"
    description="Konfigurace zhodnocení řešení v protokolu analýzy obsahuje podrobné informace o aktuálním stavu infrastrukturu serveru System Center Operations Manager při použití agentů Operations Manager nebo skupině správy pro Operations Manager."
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

# <a name="configuration-assessment-solution-in-log-analytics"></a>Konfigurace hodnocení řešení v protokolu analýzy

Konfigurace hodnocení řešení v protokolu analýzy vám pomůže najít potenciální problémy s konfigurací serveru pomocí upozornění a v knowledge doporučení.

Řešení System Center Operations Manager. Konfigurace hodnocení není dostupné, pokud používáte jenom přímo připojené agentů.

Zobrazení některé informace o konfiguraci hodnocení řešení vyžaduje modul plug-in Silverlight pro váš prohlížeč.

>[AZURE.NOTE] Na začátku 5 červenec 2016 řešení konfigurace hodnocení už není možné přidat k protokolu analýzy pracovní prostory a toto řešení již nebude k dispozici pro stávající uživatele po 1 srpen 2016. Zákazníkům, kteří používají tento řešení pro SQL Server nebo služby Active Directory doporučujeme že místo toho použít [SQL Server hodnocení](log-analytics-sql-assessment.md), řešení [Active Directory hodnocení](log-analytics-ad-assessment.md) a [Stav replikace Active Directory](log-analytics-ad-replication-status.md) . Zákazníkům, kteří používají konfigurace hodnocení pro systém Windows Hyper-V a správce pro System Center virtuálního počítače, doporučujeme použít kolekci událostí a funkcí k získání jedné komplexní zobrazení všech problémů v prostředí sledování změn.

![Konfigurace zhodnocení dlaždice](./media/log-analytics-configuration-assessment/oms-config-assess-tile.png)

Konfigurace data jsou shromáždili ze sledovaných servery a odeslaný OMS službě v cloudu pro zpracování. Použití logických operátorů je použité pro přijaté data a cloudovou službu záznamů data. Zpracovaných data serverů se zobrazují následujících oblastí:

- **Upozornění:** Zobrazí upozornění související s konfigurací, aktivní, které byly sledované serverů. Toto jsou vytvořené pomocí pravidel autorem Microsoft Customer a podpora organizace (CSS) s doporučené postupy z pole.
- **Knowledge doporučení:** Zobrazuje znalostní bázi Microsoft Knowledge Base článků, které jsou vám doporučené úloh, které se nacházejí infrastrukturu; Toto se automaticky návrhy podle konfigurace s popisem použití počítače učení.
- **Servery a úloh analyzovat:** Zobrazuje servery a úloh, které jsou sledován OMS.

![Konfigurace hodnocení řídicího panelu](./media/log-analytics-configuration-assessment/oms-config-assess-dash01.png)

### <a name="technologies-you-can-analyze-with-configuration-assessment"></a>Technologie můžete analyzovat pomocí konfigurace hodnocení

Hodnocení konfigurace OMS analyzuje následujících úloh:

- Windows Server 2012 a Microsoft Hyper-V Server 2012
- Windows Server 2008 a Windows Server 2008 R2, včetně:
    - Služby Active Directory
    - Host (hostitel) pro Hyper-V
    - Obecné operační systém
- SQL Server 2008 a novější
    - Databázový stroj SQL serveru
- Microsoft SharePoint 2010
- Microsoft Exchange Server 2010 a Microsoft Exchange serveru 2013
- Lync Server 2010 pro studenty a Microsoft Lync Server 2013
- System Center 2012 SP1 – správce virtuálního počítače

Pro systém SQL Server jsou podporovány následující 32bitové a 64bitové edice pro analýzu:

- SQL Server 2016 – všechny verze
- SQL Server 2014 - všech edic
- SQL Server 2008 a 2008 R2 - všech edic

Databázový stroj SQL Server je analyzovat na všechny podporované edice. Kromě toho je 32bitová verze SQL serveru podporované při spuštění v implementaci WOW64.

## <a name="installing-and-configuring-the-solution"></a>Instalace a konfigurace řešení
Instalace a konfigurace řešení použijte následující informace.

- Operations Manager je potřebné pro hodnocení konfigurace řešení.
- Agent Operations Manageru musíte mít na každém počítači, ve které chcete hodnotit konfigurace.
- Přidání hodnocení konfigurace řešení do pracovního prostoru OMS pomocí proces popsaný v [řešení přidat analýzy protokolu z Galerie řešení](log-analytics-add-solutions.md).  Není nutná žádná další konfigurace.

## <a name="configuration-assessment-data-collection-details"></a>Podrobnosti kolekce dat konfigurace hodnocení

Konfigurace hodnocení shromažďuje dat konfigurace, metadat a používání agentů, které jste zapnuli automatický přístup data stavu.

Následující tabulka zobrazuje metody shromažďování dat a další podrobnosti o jak údaje pro konfiguraci hodnocení.

| platformy | Přímé Agent | SCOM agent | Azure úložiště | SCOM povinné? | Data agent SCOM odeslaná pomocí správy skupiny | kolekce četnosti |
|---|---|---|---|---|---|---|
|Windows|![Ne](./media/log-analytics-configuration-assessment/oms-bullet-red.png)|![Ano](./media/log-analytics-configuration-assessment/oms-bullet-green.png)|![Ne](./media/log-analytics-configuration-assessment/oms-bullet-red.png)|            ![Ano](./media/log-analytics-configuration-assessment/oms-bullet-green.png)|![Ano](./media/log-analytics-configuration-assessment/oms-bullet-green.png)| dvakrát denně|

V následující tabulce jsou uvedeny příklady datových typů shromážděná zhodnocení konfigurace:

|**Datový typ**|**Pole**|
|---|---|
|Konfigurace|CustomerID AgentID, ID entit, ManagedTypeID, ManagedTypePropertyID, currentvalue současné, ChangeDate|
|Metadata|BaseManagedEntityId, ObjectStatus, názvu, ActiveDirectoryObjectSid, PhysicalProcessors, NázevSítě, adresa IP, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP adresu, NetbiosDomainName, LogicalProcessors, Název_dns, DisplayName, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime|
|Stav|StateChangeEventId StateId, NewHealthState, OldHealthState, kontext, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, změněno, LastGreenAlertGenerated, DatabaseTimeModified|

## <a name="configuration-assessment-alerts"></a>Konfigurace hodnocení upozornění
Můžete zobrazit a spravovat upozornění z konfigurace hodnocení na stránce upozornění. Upozornění řekněte problém, který byl zjištěn příčinu a jak lze řešit potíže. Také obsahují informace o nastavení konfigurace prostředí, která může způsobit problémy s výkonem.

![Zobrazit upozornění](./media/log-analytics-configuration-assessment/oms-config-assess-alerts01.png)

>[AZURE.NOTE] Upozornění konfigurace hodnocení se liší od jiných oznámení v protokolu analýzy. Zobrazení upozornění vyžaduje modul plug-in Silverlight pro váš prohlížeč.

Po výběru položky nebo kategorie položek na stránce upozornění, zobrazí se seznam servery nebo pracovního vytížení se upozornění, které platí pro každou položku.

![Seznam výstrah](./media/log-analytics-configuration-assessment/oms-config-assess-alerts-view-config.png)

Každý upozornění obsahuje odkaz na článek znalostní báze Microsoft Knowledge Base. Tyto články poskytují další informace o upozornění.

>[AZURE.TIP] Ve výchozím nastavení je maximální počet upozornění 2 000. Pokud chcete zobrazit další upozornění, klikněte na v oznamovacím pruhu nad seznamem upozornění.

Klikněte na libovolnou položku v seznamu zobrazit článek KB, který vám mohou pomoci odstranit příčinu problému, které vygenerovalo oznámení.

![Článek znalostní bázi Knowledge Base](./media/log-analytics-configuration-assessment/oms-config-assess-alerts-details-kb.png)

Můžete spravovat pravidla výstrah přeskočit určitého pravidla nebo třídy pravidla.

![Správa pravidel upozornění](./media/log-analytics-configuration-assessment/oms-config-assess-alert-rules.png)

## <a name="knowledge-recommendations"></a>Knowledge doporučení
Kódu knowledge doporučení, budete prezentovat výsledky hledání protokolu seznam článků znalostní báze Microsoft KB doporučeného pro typ pracovního vytížení a počítačů, které poskytují další informace o upozornění.

![výsledky hledání v knowledge doporučení](./media/log-analytics-configuration-assessment/oms-config-assess-knowledge-recommendations.png)

## <a name="servers-and-workloads-analyzed"></a>Servery a analyzovat pracovního vytížení
Při prohlížení knowledge doporučení budete prezentovat výsledky hledání protokolu výpis všech serverů a úloh, které je známo OMS z Operations Manager.

![Servery a úloh](./media/log-analytics-configuration-assessment/oms-config-assess-servers-workloads.png)

## <a name="next-steps"></a>Další kroky

- Pomocí [protokolu vyhledávání v protokolu analýzy](log-analytics-log-searches.md) zobrazíte podrobné konfigurační hodnocení data.
