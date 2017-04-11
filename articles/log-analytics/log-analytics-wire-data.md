<properties
    pageTitle="Drátu řešení dat v protokolu analýzy | Microsoft Azure"
    description="Drátěný data jsou sloučená data síť a výkonu z počítače s OMS agenti – včetně Operations Manager a připojené Windows agentů. Všechna data spolu s protokolu data týkající se sladit data."
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

# <a name="wire-data-solution-in-log-analytics"></a>Drátu řešení dat v protokolu analýzy

Drátěný data jsou sloučená data síť a výkonu z počítače s OMS agenti – včetně Operations Manager a připojené Windows agentů. Všechna data spolu s protokolu data týkající se sladit data. Na počítači v IT infrastrukturu monitor všechna data odeslané z těchto počítačů pro síť úrovně 2-3 v [modelu OSI](https://en.wikipedia.org/wiki/OSI_model) včetně různých protokoly a porty používané nainstalovanou agentů OMS.

>[AZURE.NOTE] Řešení drátěný dat není momentálně neexistuje které budou přidány do pracovních prostorech. Zákazníci, kteří už máte řešení drátěný dat povolený můžete dál používat drátěný datové řešení.

Ve výchozím nastavení OMS shromažďuje zaznamenán pro využití procesoru, paměti, disk a data o výkonu sítě z čítače integrovaná v systému Windows. Síť a jiných shromažďování dat se provádí v v reálném čase pro každý agent, včetně podsítí a úrovni aplikace protokoly používanými počítač. Na stránce nastavení na kartě protokoly můžete přidat další výkonnosti.

Pokud jste vypotřebovali [sFlow](http://www.sflow.org/) nebo jiného softwaru s [protokolem NetFlow přepínač](http://www.cisco.com/c/en/us/products/collateral/ios-nx-os-software/ios-netflow/prod_white_paper0900aecd80406232.html), Statistika a data, která se zobrazí z drátěný dat bude povědomá.

Některé typy předdefinované protokolu vyhledávacích dotazů patří:

- Agentů, které jsou zdrojem dat drátěný
- IP adresu agentů poskytování drátěný dat
- Odchozí komunikaci IP adresy
- Počet bajtů protokoly aplikací
- Počet bajtů poslán e aplikace služby
- Bajtů různé protokoly
- Celkový počet bajtů odešlete nebo přijmete IP
- IP adresy, které mají komunikovali agentů podsítě 10.0.0.0/8
- Průměrnou latenci pro připojení, které byly měří problémy se spolehlivým
- Počítač procesů, které iniciuje nebo přijatá v síti
- Částka provozu v síti procesu

Při hledání pomocí drátěný dat je můžete vyfiltrovat a seskupit data k zobrazení informací o horní agentů a horním protokoly. Nebo můžete hledat do kdy některých počítačích (adresy IP adresy a MAC) komunikovali sobě, jak dlouho a množství zpracovávaných dat odešla – v podstatě zobrazit metadata v síti, které je založené na vyhledávání.

Když si prohlížíte metadata, je však není nemusí být užitečné pro důkladnějšího odstraňování potíží. Drátěný dat v OMS není úplné zachycení všechna data. Takže není určená pro řešení potíží s tmavě úrovni paketů.
Výhodou používání agent ve srovnání s jiné metody kolekce je, že nebudete muset nainstalovat spotřebiče, překonfigurovat síťové přepínače nebo preform složité konfigurace. Drátěný dat je jednoduše agent založený na-agent nainstalovat na počítač a bude sledovat vlastní v síti. Další výhodou je, když chcete sledovat pracovní vytížení v cloudu poskytovatelů nebo poskytovatele hostingu nebo Microsoft Azure, kde uživatel nemá vlastní vrstvy struktury.

Naopak nemáte úplnou viditelnost co se stane v síti nainstalujete není agentů ve všech počítačích v vaši síťovou infrastrukturu.

## <a name="installing-and-configuring-the-solution"></a>Instalace a konfigurace řešení
Instalace a konfigurace řešení použijte následující informace.

- Drátěný datové řešení získává data z počítače se systémem Windows serveru 2012 R2, Windows 8.1 a novější operační systémy.
- Na místo, kam chcete získat drátěný data z počítače se vyžaduje rozhraní Microsoft .NET Framework 4.0 nebo novější.
- Přidání řešení drátěný dat do pracovního prostoru OMS pomocí proces popsaný v [řešení přidat analýzy protokolu z Galerie řešení](log-analytics-add-solutions.md).  Není nutná žádná další konfigurace.
- Pokud chcete zobrazit data drátěný pro konkrétní řešení, musíte mít řešení už přidali do pracovního prostoru OMS.

## <a name="wire-data-data-collection-details"></a>Drátu podrobnosti kolekce dat dat

Drátěný dat shromažďuje metadata o používání agentů, které jste zapnuli automatický přístup v síti.

Následující tabulka zobrazuje metody shromažďování dat a další podrobnosti o jak údaje pro drátěný Data.


| platformy | Přímé Agent | SCOM agent | Azure úložiště | SCOM povinné? | Data agent SCOM odeslaná pomocí správy skupiny | kolekce četnosti |
|---|---|---|---|---|---|---|
|Windows (2012 R2 / 8.1 nebo novější)|![Ano](./media/log-analytics-wire-data/oms-bullet-green.png)|![Ano](./media/log-analytics-wire-data/oms-bullet-green.png)|![Ne](./media/log-analytics-wire-data/oms-bullet-red.png)|            ![Ne](./media/log-analytics-wire-data/oms-bullet-red.png)|![Ne](./media/log-analytics-wire-data/oms-bullet-red.png)| každou 1 minutu|


## <a name="combining-wire-data-with-other-solution-data"></a>Kombinování dat drátěný s jinými daty řešení

Data vrácená z předdefinovaných dotazy zobrazené nad pravděpodobně zajímavé samostatně. Užitečnost drátěný dat je však realizují při kombinování s informacemi z jiných OMS řešení. Můžete například pomocí zabezpečení události data shromážděná zabezpečení a auditování řešení a kombinovat s daty drátěný můžete vyhledávat neobvyklé sítě přihlášení pojmenované procesů.  V tomto příkladu by v a DISTINCT operátory slouží k připojení datových bodů v vyhledávacího dotazu.

Požadavky: Abyste mohli používat v následujícím příkladu, musíte mít nainstalovaný řešení zabezpečení a auditování. Data z jiných řešení však může použít ke sloučení s daty drátěný podobných výsledků dosáhnout.

### <a name="to-combine-wire-data-with-security-events"></a>Sloučení dat drátěný zabezpečení události

1. Na stránce Přehled klikněte na dlaždici **WireData** .
2. V seznamu **Běžné dotazy WireData**klikněte na **Částku z v síti (v bajtech) procesem** zobrazíte seznam vrácených procesů.
    ![wiredata dotazů](./media/log-analytics-wire-data/oms-wiredata-01.png)
3. Pokud je příliš dlouhá snadno zobrazíte seznam procesů, můžete upravit vyhledávacího dotazu tak, aby připomínala:

    ```
    Type WireData | measure count() by ProcessName | where AggregatedValue <40
    ```
    Vidět v následujícím příkladu je proces s názvem DancingPigs.exe, která se může zobrazit podezřelé.
    ![výsledky hledání wiredata](./media/log-analytics-wire-data/oms-wiredata-02.png)

4. Použití data vrácená v seznamu, klikněte na pojmenované proces. V tomto příkladu DancingPigs.exe kliknuli. Výsledky ukázáno v následujícím příkladu popisují druhu v síti třeba odchozí komunikaci přes různé protokoly.
    ![výsledky wiredata zobrazující pojmenovanou obrázku](./media/log-analytics-wire-data/oms-wiredata-03.png)

5. Protože je už nainstalovaná zabezpečení a auditování řešení, které probe do události zabezpečení, které mají stejné hodnoty pole název_procesu změnou vyhledávání pomocí operátorů Palců a DISTINCT hledání dotazu. Můžete to udělat potom drátěný dat a dalších řešení protokoly obsahujících hodnoty ve stejném formátu. Změna vyhledávacího dotazu tak, aby připomínala:

    ```
    Type=SecurityEvent ProcessName IN {Type:WireData "DancingPigs.exe" | distinct ProcessName}
    ```    

    ![wiredata výsledků obsahující kombinovat data](./media/log-analytics-wire-data/oms-wiredata-04.png)
6. Ve výsledcích výše uvedené zobrazí se zobrazují informace o účtu. Teď můžete upřesnit vyhledávacího dotazu chcete zjistit, jak často účtu zobrazující zabezpečení a auditování data použil procesu s dotazu podobné:        

    ```
    Type=SecurityEvent ProcessName IN {Type:WireData "DancingPigs.exe" | distinct ProcessName} | measure count() by Account
    ```

    ![výsledky wiredata představující data účtu](./media/log-analytics-wire-data/oms-wiredata-05.png)



## <a name="next-steps"></a>Další kroky

- [Hledání protokoly](log-analytics-log-searches.md) zobrazíte podrobné drátěný dat vyhledávání záznamů.
- Najdete v tématu společnosti Dan [zveřejňují pomocí drátěný dat v blogu operace správy sadu protokolu vyhledávání](http://blogs.msdn.com/b/dmuscett/archive/2015/09/09/using-wire-data-in-operations-management-suite.aspx) obsahuje další informace o tom, jak často se data shromažďují a jak je možné upravit vlastnosti kolekce pro Operations Manager agentů.
