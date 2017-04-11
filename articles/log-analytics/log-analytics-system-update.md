<properties
    pageTitle="Systém hodnocení aktualizace řešení v protokolu analýzy | Microsoft Azure"
    description="Pomocí řešení aktualizace systému v protokolu analýzy můžete použít chybějící aktualizace na serverech infrastruktury."
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
    ms.date="08/11/2016"
    ms.author="banders"/>

# <a name="system-update-assessment-solution-in-log-analytics"></a>Systém hodnocení aktualizace řešení v protokolu analýzy

Pomocí řešení aktualizace systému v protokolu analýzy můžete použít chybějící aktualizace na serverech infrastruktury. Po nainstalování řešení, můžete zobrazit aktualizace, které chybí serverech sledované pomocí dlaždici **Systém aktualizace hodnocení** na stránce **Přehled** v OMS.

Pokud chybějící aktualizace najdete, informace se zobrazí na řídicím panelu **aktualizace** . Můžete na řídicím panelu **aktualizace** se dá pracovat s chybějícím aktualizace a sestavit plán použít k serverům, které potřebujete.

## <a name="installing-and-configuring-the-solution"></a>Instalace a konfigurace řešení
Instalace a konfigurace řešení použijte následující informace.

- Přidání hodnocení aktualizace systému řešení OMS pracovního prostoru pomocí proces popsaný v [řešení přidat analýzy protokolu z Galerie řešení](log-analytics-add-solutions.md).  Není nutná žádná další konfigurace.

## <a name="system-update-data-collection-details"></a>Podrobné informace o systému aktualizaci dat kolekce

Systém hodnocení aktualizace shromažďuje metadat a stav dat pomocí agentů, které jste zapnuli automatický přístup.

Následující tabulka zobrazuje metody shromažďování dat a další podrobnosti o jak údaje pro systém aktualizovat hodnocení.

| platformy | Přímé Agent | SCOM agent | Azure úložiště | SCOM povinné? | Data agent SCOM odeslaná pomocí správy skupiny | kolekce četnosti |
|---|---|---|---|---|---|---|
|Windows|![Ano](./media/log-analytics-system-update/oms-bullet-green.png)|![Ano](./media/log-analytics-system-update/oms-bullet-green.png)|![Ne](./media/log-analytics-system-update/oms-bullet-red.png)|            ![Ne](./media/log-analytics-system-update/oms-bullet-red.png)|![Ano](./media/log-analytics-system-update/oms-bullet-green.png)| Alespoň na úrovni 2 krát za den a 15 minut po instalaci aktualizace|

V následující tabulce jsou uvedeny příklady datových typů shromážděná zhodnocení aktualizace systému:

|**Datový typ**|**Pole**|
|---|---|
|Metadata|BaseManagedEntityId, ObjectStatus, názvu, ActiveDirectoryObjectSid, PhysicalProcessors, NázevSítě, adresa IP, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP adresu, NetbiosDomainName, LogicalProcessors, Název_dns, DisplayName, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime|
|Stav|StateChangeEventId StateId, NewHealthState, OldHealthState, kontext, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, změněno, LastGreenAlertGenerated, DatabaseTimeModified|


### <a name="to-work-with-updates"></a>Práce s aktualizací

1. Na stránce **Přehled** klikněte na dlaždici **Systém aktualizovat hodnocení** .  
    ![Systém hodnocení aktualizace dlaždice](./media/log-analytics-system-update/sys-update-tile.png)
2. Na řídicím panelu **aktualizace** zobrazení aktualizace kategorií.  
    ![Aktualizace řídicího panelu](./media/log-analytics-system-update/sys-updates02.png)
3. Přejděte na pravé straně stránky zobrazit zásuvné **Aktualizace kritický/zabezpečení systému Windows** a pak ve skupinovém rámečku **klasifikace**, klikněte na **Aktualizace zabezpečení**.  
    ![Aktualizace zabezpečení](./media/log-analytics-system-update/sys-updates03.png)
4. Na stránce Log vyhledávání se zobrazuje různé informace o aktualizace zabezpečení, které nebyly nalezeny chybí serverech infrastruktury. Klikněte na **seznam** zobrazíte podrobné informace o aktualizacích.  
    ![Výsledky hledání – seznam](./media/log-analytics-system-update/sys-updates04.png)
5. Na stránce Log vyhledávání se zobrazuje podrobné informace o každé aktualizaci. Vedle popisku číslo KBID klikněte na **zobrazení** zobrazující odpovídající článek na webu podpory společnosti Microsoft.  
    ![Přihlaste se výsledky hledání – zobrazení znalostní bázi Knowledge Base](./media/log-analytics-system-update/sys-updates05.png)
6. Webový prohlížeč otevře webovou stránku Microsoft Support k aktualizaci na nové kartě. Zobrazení informací o aktualizaci chybí.  
    ![Microsoft Support webovou stránku](./media/log-analytics-system-update/sys-updates06.png)
7. Pomocí pomocí informací najdete, můžete vytvořit plán pro ruční aktualizaci chybějící, nebo můžete dál po zbývajících kroků automatické aktualizace.
8. Pokud chcete automatické aktualizace chybí, přejděte zpátky na řídicím panelu **aktualizace** a v části **Aktualizovat spustí**klikněte **klikněte na naplánovat aktualizaci spustit**.  
    ![Spuštění aktualizace - plánované kartu](./media/log-analytics-system-update/sys-updates07.png)
9. Na stránce **Se spustí aktualizovat** na kartě **Naplánované** klikněte na tlačítko **Přidat** nové spuštění aktualizace.  
    ![Plánovaná tabulátorem – přidání](./media/log-analytics-system-update/sys-updates08.png)
10. Na **Aktualizovat spuštění nové** stránce zadejte název pro aktualizaci spustit, přidejte jednotlivé počítače nebo skupiny počítačů, definovat plán a klikněte na tlačítko **Uložit**.  
    ![Spuštění nové aktualizace](./media/log-analytics-system-update/sys-updates09.png)
11. Karta **naplánovaná** na stránky zobrazuje **Aktualizace spustí** novou aktualizaci spustit, které jste jste naplánovali.  
    ![Plánované kartu](./media/log-analytics-system-update/sys-updates10.png)
12. Po spuštění aktualizace spustit uvidíte informace na kartě **spuštěný** .  
    ![Průběžný kartu](./media/log-analytics-system-update/sys-updates11.png)
13. Až se dokončí aktualizace, spusťte, zobrazí se na kartě **Dokončeno** stav.
14. Pokud aktualizace byly použity z aktualizace v zásuvné **Aktualizace kritický/zabezpečení systému Windows** spustit uvidíte snižuje počet aktualizací.  
    ![Pole Kritický/aktualizací Windows zásuvné - snižuje počet aktualizací](./media/log-analytics-system-update/sys-updates12.png)



## <a name="next-steps"></a>Další kroky

- [Hledání protokoly](log-analytics-log-searches.md) zobrazíte podrobné systém aktualizovat data.
