<properties
    pageTitle="Optimalizace prostředí s řešením Active Directory hodnocení v protokolu analýzy | Microsoft Azure"
    description="Řešení Active Directory hodnocení umožňuje hodnocení rizik a stavu prostředí serveru v pravidelném intervalu."
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

# <a name="optimize-your-environment-with-the-active-directory-assessment-solution-in-log-analytics"></a>Optimalizace prostředí s řešením Active Directory hodnocení v protokolu analýzy

Řešení Active Directory hodnocení umožňuje hodnocení rizik a stavu prostředí serveru v pravidelném intervalu. Tento článek vám pomůže nainstalovat a používat řešení, takže můžete provést korekční akce pro potenciální problémy.

Toto řešení poskytuje uspořádaný seznam doporučení specifické pro infrastrukturu nasazení serveru. Doporučení zařazené přes čtyři fokus oblastech, které umožňují rychle získat a proveďte akce.

Doporučení vycházejí v knowledge a zkušeností pracovníci společnosti Microsoft z tisíců návštěvy zákazníka. Každý doporučení obsahuje pokyny, proč problému může vám záleží a jak implementovat navržené změny.

Můžete fokus oblasti, které jsou důležité pro vaši organizaci a sledovat průběh směrem k spuštění rizika zdarma a správný prostředí.

Poté, co jste přidali řešení a vyhodnocení je dokončený, souhrnné informace pro fokus oblasti se zobrazují na řídicím panelu **AD hodnocení** pro infrastrukturu ve vašem prostředí. Následující části popisují, jak používat informace na řídicím panelu **AD hodnocení** , kde můžete zobrazit a potom akcím doporučené serverové infrastruktury služby Active Directory.

![Obrázek rohu dlaždice hodnocení SQL](./media/log-analytics-ad-assessment/ad-tile.png)

![Obrázek s řídicím panelem hodnocení SQL](./media/log-analytics-ad-assessment/ad-assessment.png)


## <a name="installing-and-configuring-the-solution"></a>Instalace a konfigurace řešení
Instalace a konfigurace řešení použijte následující informace.

- Agentů musí být nainstalovaný na řadiče domény, které jsou součástí domény má být vyhodnocen.
- Řešení Active Directory hodnocení vyžaduje .NET Framework 4 na každém počítači, který má agenta OMS nainstalovaný.
- Přidání řešení Active Directory hodnocení do pracovního prostoru OMS pomocí proces popsaný v [řešení přidat analýzy protokolu z Galerie řešení](log-analytics-add-solutions.md).  Není nutná žádná další konfigurace.

    >[AZURE.NOTE] Po přidání řešení, soubor AdvisorAssessment.exe je přidán k serverům s agentů. Konfigurace dat je číst a odeslaný OMS službě v cloudu pro zpracování. Použití logických operátorů je použité pro přijaté data a cloudovou službu záznamů data.

## <a name="active-directory-assessment-data-collection-details"></a>Podrobnosti kolekce dat Active Directory zhodnocení

Active Directory zhodnocení shromažďuje WMI dat registru dat a dat o výkonu pomocí agentů, které jste zapnuli automatický přístup.

Následující tabulka zobrazuje metody kolekce dat pro agentů, jestli se vyžaduje Operations Manager (SCOM) a jak často se údaje agentem.

| platformy | Přímé Agent | SCOM agent | Azure úložiště | SCOM povinné? | Data agent SCOM odeslaná pomocí správy skupiny | kolekce četnosti |
|---|---|---|---|---|---|---|
|Windows|![Ano](./media/log-analytics-ad-assessment/oms-bullet-green.png)|![Ano](./media/log-analytics-ad-assessment/oms-bullet-green.png)|![Ne](./media/log-analytics-ad-assessment/oms-bullet-red.png)|   ![Ne](./media/log-analytics-ad-assessment/oms-bullet-red.png)|![Ano](./media/log-analytics-ad-assessment/oms-bullet-green.png)| 7 dnů|


## <a name="understanding-how-recommendations-are-prioritized"></a>Vysvětlení, jak přednostně doporučení

Každý doporučení přiřazena hodnota váhu, který identifikuje relativní důležitost doporučení. Zobrazují se jenom deset nejdůležitější doporučení.

### <a name="how-weights-are-calculated"></a>Způsob výpočtu gramáží

Koeficienty jsou souhrnné hodnoty založené na tři klíčové faktory:

- *Pravděpodobnost* , že zjištěn problém se způsobit problémy. Vyšší pravděpodobnost rovná větší celkové skóre pro doporučení.

- *Vliv* problém na organizace: Pokud způsobit potíže. Vyšší dopad rovná větší celkové skóre pro doporučení.

- *Plánování řízené úsilí* nutné provést doporučení. Vyšší úsilí rovná menší celkové skóre pro doporučení.

Váha pro každou doporučení je vyjádřený v procentech celkové skóre dostupné pro všechny oblasti fokus. Například pokud doporučení v oblasti fokus zabezpečení a dodržování předpisů skóre 5 %, provádění tohoto doporučení zvětšíte vaše celkové skóre tak, že 5 % zabezpečení a dodržování předpisů.

### <a name="focus-areas"></a>Výběr oblasti

**Zabezpečení a dodržování předpisů** – tato oblast fokus ukazuje doporučení pro potenciální hrozeb zabezpečení a narušením a podnikových zásad technické, právních a regulačních předpisů.

**Dostupnost a nepřerušený** - této oblasti fokus ukazuje doporučení pro dostupnost služby, odolnost proti chybám infrastruktury a obchodní zámek.

**Výkon a škálovatelnost** – tato oblast fokus ukazuje doporučení vaší organizaci pomoct IT infrastrukturu zvětšit, ujistěte se, že prostředí IT splňuje požadavky na aktuální výkonu a je možné reagovat na změny infrastruktury potřeb.

**Upgrade migrace a nasazení** - této oblasti fokus zobrazuje doporučení týkající se upgradovat, migrace a nasadit na stávající infrastruktury služby Active Directory.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>Měly by směřovat ke skóre 100 % ve všech oblastech fokus?

Ne nutně. Doporučení vycházejí v knowledge a zkušenosti získané Microsoft inženýrů přes tisíce návštěvy zákazníka. Ale žádné dva serverové infrastruktury jsou stejné a konkrétní doporučení může být více nebo méně jsou pro vás. Například může být některé zabezpečení doporučení méně důležité, pokud nejsou virtuálních počítačích vystaven na Internetu. Některé dostupnost doporučení může být menší relevantní pro služby, které poskytují nízkou prioritou ad hoc postupům shromažďování a vytváření sestav. Problémy, které jsou důležité pro zdokonaleným firmy může být starší důležité k zahájení. Můžete určit oblasti fokus, ve kterých jsou priorit a podívejte se na tom, jak v průběhu času mění skóre.

Každý doporučení obsahuje pokyny, proč je důležité. Pokud chcete zjistit, zda provádění doporučení určený pro vás přírodní služeb IT a obchodních potřeb vaší organizace, byste měli použít tyto pokyny.

## <a name="use-assessment-focus-area-recommendations"></a>Použití hodnocení fokus oblasti doporučení

Než budete moct použít řešení pro hodnocení ve OMS, musíte mít řešení nainstalovaný. Chcete další informace o instalaci řešení, najdete v článku [Přidání analýzy protokolu řešení z Galerie řešení](log-analytics-add-solutions.md). Po instalaci, můžete zobrazit přehled doporučení pomocí dlaždici hodnocení na stránce Přehled v OMS.

Zobrazení hodnocení souhrnné dodržování předpisů pro infrastrukturu a potom přejít k podrobnostem doporučení.


### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Prohlížení doporučení pro oblast fokus a jaké nápravné akce

1. Na stránce **Přehled** klikněte na dlaždici **hodnocení** pro infrastrukturu serveru.
2. Na stránce **hodnocení** zkontrolujte souhrnné informace v jednom z oblasti listy fokusu a potom klikněte na skupinu pro zobrazení doporučení pro tuto oblast fokus.
3. Na jakékoli stránce fokus oblasti můžete zobrazit uspořádaný doporučení pro prostředí. Doporučení klikněte v části **Vliv objekty** a zobrazit podrobnosti o Proč je doporučení.  
    ![Obrázek hodnocení doporučení](./media/log-analytics-ad-assessment/ad-focus.png)
4. Můžete provést korekční akce navržené v **Navržené akce**. Pokud byla určena položky, zaznamená novější hodnocení, které byly odebrány akce a dodržování předpisů skóre zvětšíte. Opravený položky se zobrazí jako **Předaný objekty**.

## <a name="ignore-recommendations"></a>Ignorovat doporučení

Pokud máte doporučení, která chcete ignorovat, můžete vytvořit textový soubor, který OMS použije nechcete, aby doporučení zobrazoval ve výsledcích hodnocení.

### <a name="to-identify-recommendations-that-you-will-ignore"></a>Jak identifikovat doporučení, která bude ignorovat

1.  Přihlaste se do pracovního prostoru a otevřete protokolu vyhledávání. Použijte následující dotaz na seznam doporučení, která se nezdařil pro počítače v prostředí.

    ```
    Type=ADAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

    Tady je snímek obrazovky zobrazující protokolu vyhledávacího dotazu: ![se nepodařilo doporučení](./media/log-analytics-ad-assessment/ad-failed-recommendations.png)

2.  Zvolte doporučení, která chcete ignorovat. Použijete hodnoty pro RecommendationId v následujícím postupu.


### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Vytvoření a použití IgnoreRecommendations.txt textového souboru

1.  Vytvoření souboru s názvem IgnoreRecommendations.txt.
2.  Vložení nebo zadejte každou RecommendationId pro každou doporučení má protokolu analýzy ignorovat na samostatném řádku a uložte a zavřete soubor.
3.  Uložte soubor v následující složce na každém počítači místo, kam chcete OMS přeskočit doporučení.
    - Na počítačích s Microsoft sledování agentem (připojené přímo nebo prostřednictvím Operations Manager) - *systémová_jednotka*: \Program Files\Microsoft Agent\Agent sledování
    - Na serveru správy Operations Manager - *systémová_jednotka*: \Program Files\Microsoft System Center 2012 R2\Operations Manager\Server

### <a name="to-verify-that-recommendations-are-ignored"></a>K ověření, že jsou ignorovány doporučení

Po další plánovaná hodnocení se spustí, ve výchozím nastavení každých 7 dnů, zadané doporučení jsou označeny jako *ignorován* a na řídicím panelu hodnocení se nezobrazí.

1. Seznam všech ignorované doporučení můžete použít následující protokolu vyhledávacích dotazů.

    ```
    Type=ADAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

2.  Pokud se později rozhodnete, že chcete zobrazit ignorované doporučení, odeberte všechny soubory IgnoreRecommendations.txt nebo odeberete RecommendationIDs z nich.

## <a name="ad-assessment-solutions-faq"></a>AD zhodnocení řešení nejčastější dotazy

*Jak často hodnocení spustit?*
- Hodnocení se spustí každých 7 dnů.

*Existuje způsob, jak nakonfigurovat, jak často hodnocení spuštěna?*
- V této chvíli ne.

*Pokud jiný server pro zjištěno, když jste přidali hodnocení řešení, budou ho posouzena?*
- Ano, když se zjistí, že je pochopení z té, každých 7 dnů.

*Pokud server je vyřadit z provozu, kdy ji odeberou z hodnocení?*
- Pokud serveru není odeslání dat pro 3 týdnů, je odebraná.

*Co je název proces, který nemá shromažďování dat?*
- AdvisorAssessment.exe

*Jak dlouho trvá pro se shromažďují?*
- Shromažďování dat skutečné na serveru trvá asi 1 hodinu. Může trvat déle na servery, které máte velký počet servery služby Active Directory.

*Jaký typ dat se shromažďují?*
- Byly shromážděny následující typy dat:
    - WMI
    - Registru
    - Výkonnosti

*Je možné nastavit v případě se shromažďují data?*
- V této chvíli ne.

*Proč zobrazí pouze prvních 10 doporučení?*
- Místo pojmenování vyčerpávající jednoznačná seznam úkolů, doporučujeme zaměřit na první adresování uspořádaný doporučení. Po jejich řešení budou k dispozici další doporučení. Pokud chcete zobrazit podrobný seznam, můžete zobrazit všechny doporučení pomocí protokolu vyhledávání.

*Existuje způsob, jak ignorovat doporučení?*
- Ano, najdete v článku [Ignorovat doporučení](#ignore-recommendations) oddílu výše.


## <a name="next-steps"></a>Další kroky

- Pomocí [protokolu vyhledávání v protokolu analýzy](log-analytics-log-searches.md) zobrazíte podrobná data AD hodnocení a doporučení.
