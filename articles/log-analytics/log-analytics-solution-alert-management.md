<properties
   pageTitle="Upozornit řešení pro správu v sadě správy operace (OMS) | Microsoft Azure"
   description="Řešení upozornění správy v protokolu analýzy pomáhá při analýze všechna upozornění ve vašem prostředí.  Vedle slučování výstrahy vytvořeny v rámci OMS ho importují data upozornění z připojeného skupiny správy systému Centrum Operations Manager (SCOM) do protokolu analýzy."
   services="log-analytics"
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
   ms.date="10/06/2016"
   ms.author="bwren" />

# <a name="alert-management-solution-in-operations-management-suite-oms"></a>Upozornit řešení pro správu v sadě správy operace (OMS)

![Ikona upozornění správy](media/log-analytics-solution-alert-management/icon.png) Řešení upozornění správy pomáhá při analýze všechna upozornění ve vašem prostředí.  Vedle slučování výstrahy vytvořeny v rámci OMS ho importují data upozornění z připojeného skupiny správy systému Centrum Operations Manager (SCOM) do protokolu analýzy.  V prostředí s více skupin Správa řešení upozornění správy poskytne konsolidované zobrazení upozornění ve všech skupinách správy.

## <a name="prerequisites"></a>Zjistit předpoklady pro

- Import upozornění z SCOM, toto řešení vyžaduje spojení mezi pracovního prostoru OMS a SCOM Správa skupiny pomocí procesu podle [Připojení Operations Manager do protokolu analýzy](log-analytics-om-agents.md).  


## <a name="configuration"></a>Konfigurace

Přidání řešení upozornění správy do pracovního prostoru OMS pomocí procesu podle [Přidat řešení](log-analytics-add-solutions.md).  Není nutná žádná další konfigurace.

## <a name="management-packs"></a>Správa sad

Pokud je skupina Správa SCOM připojené k OMS pracovního prostoru, pak následující management Pack bude mít nainstalovanou na SCOM při přidání toto řešení.  Nemůže žádným způsobem konfigurace ani údržbu management Pack povinné.  

- Microsoft System Center Advisor upozornění správy (Microsoft.IntelligencePacks.AlertManagement)

Další informace o aktualizace řešení management Pack najdete v článku [Připojení Operations Manager do protokolu analýzy](log-analytics-om-agents.md).

## <a name="data-collection"></a>Shromažďování dat

### <a name="agents"></a>Agentů

Následující tabulka popisuje připojení zdroje, které jsou podporovány toto řešení.

| Připojení zdroje | Podpora | Popis |
|:--|:--|:--|
| [Windows agentů](log-analytics-windows-agents.md) | Ne | Přímé agentů Windows nevytváří SCOM upozornění. |
| [Linux agentů](log-analytics-linux-agents.md) | Ne | Přímé agentů Linux nevytváří SCOM upozornění. |
| [Skupina Správa SCOM](log-analytics-om-agents.md) | Ano | Oznámení, která jsou vytvářeny na SCOM agentů jsou doručeny do skupiny Správa a pak předána protokolu analýzy.<br><br>Přímé připojení z agenta SCOM k protokolu analýzy není potřeba. Úložiště OMS předán ze skupiny správy upozornění data. |
| [Účet Azure úložiště](log-analytics-azure-storage.md) | Ne | Upozornění SCOM nejsou uloženy v Azure úložiště účty. |

### <a name="collection-frequency"></a>Kolekce četnosti

Výstrahy vytvořeny v rámci OMS jsou dostupné pro řešení okamžitě.  Údaje o oznámení se pošle ze skupiny správy SCOM protokolu analýzy každé 3 minuty.  

## <a name="using-the-solution"></a>Použití řešení

Když přidáte do pracovního prostoru OMS řešení upozornění správy, dlaždici **Upozornění správy** se přidá do řídícího OMS.  Této dlaždici se zobrazují počet a grafické znázornění počet aktuálně aktivní upozornění, které byly vytvořeny posledních 24 hodin.  Není možné změnit tohoto časového intervalu.

![Oznámení dlaždice správy](media/log-analytics-solution-alert-management/tile.png)

Klikněte na dlaždici **Upozornění správy** otevřete řídicí panel pro **Správu upozornění** .  Řídicí panel obsahuje sloupce v následující tabulce.  Každý sloupec oblasti seznamy podle tohoto sloupce kritéria pro zadaný obor a časový rozsah přiřazování počet horních deseti upozornění.  Můžete spustit hledání protokolu poskytující celého seznamu kliknutím na **Zobrazit všechny** v dolní části sloupce nebo kliknutím na záhlaví sloupce.

| Sloupec| Popis |
|:--|:--|
| Důležité upozornění | Všechna upozornění o závažnosti Kritický seskupené podle názvu upozornění.  Klikněte na název výstrahy spustit hledání protokolu vrácení všech záznamů pro tento upozornění. |
| Upozornění na upozornění | Všechna upozornění o závažnosti upozornění seskupené podle názvu upozornění.  Klikněte na název výstrahy spustit hledání protokolu vrácení všech záznamů pro tento upozornění. |
| Aktivní SCOM upozornění | Všechna upozornění SCOM pomocí kterékoli uveďte jedině *Uzavřeno* seskupenými podle zdroje, které vygenerovalo oznámení. |
| Všechna aktivní upozornění | Všechna upozornění se všechny závažnosti seskupené podle názvu upozornění. Obsahuje pouze upozornění SCOM stavem než *uzavřená*.|

Pokud se posuňte zobrazení doprava, řídicího panelu seznam několik běžné dotazy, které můžete kliknout na k provedení [protokolu hledání](log-analytics-log-searches.md) pro upozornění data.

![Upozornit řídicí panel pro správu](media/log-analytics-solution-alert-management/dashboard.png)

## <a name="scope-and-time-range"></a>Rozsah a časového rozsahu

Ve výchozím nastavení je oborem upozornění analyzovat v řešení upozornění správy ze všech skupin připojeného správy generované během posledních 7 dnů.  

![Upozornění řízení rozsahu](media/log-analytics-solution-alert-management/scope.png)

- Pokud chcete změnit správu skupiny zahrnuté do analýzy, klikněte na **rozsah** v horní části řídicího panelu.  Lze vybrat **globální** pro všechny skupiny připojeného správy a **Ve skupině Správa** vyberte skupinu, do jednoho správy.

- Chcete-li změnit časový rozsah upozornění, vyberte **na základě dat** v horní části řídicího panelu.  Můžete vybrat výstrahy generované během posledních 7 dnů, 1 den nebo 6 hodin.  Nebo můžete vybrat **vlastní** a zadejte vlastní časové období.

## <a name="log-analytics-records"></a>Technologie pro analýzu záznamů protokolu

Řešení upozornění správy analyzuje libovolný záznam u typu **upozornění**.  Bude taky importovat upozornění z SCOM a vytvořte odpovídající záznam pro každého typu **upozornění** a SourceSystem **OpsManager**.  Tyto záznamy obsahují vlastností v následující tabulce.  

| Vlastnost | Popis |
|:--|:--|
| Typ | *Upozornit* |
| SourceSystem | *OpsManager* |
| AlertContext | Podrobnosti o položka data, která způsobila upozornění generovat ve formátu XML. |
| AlertDescription | Podrobný popis upozornění. |
| AlertId | Identifikátor GUID upozornění. |
| AlertName | Název upozornění. |
| AlertPriority | Úroveň priority upozornění. |
| AlertSeverity | Úroveň závažnosti upozornění. |
| AlertState | Nejnovější stav rozlišení upozornění. |
| Naposledy upravil | Upozornění na jméno uživatele, kterému naposledy upravil |
| ManagementGroupName | Název skupiny správy kdy byl vytvořen upozornění. |
| RepeatCount | Počet časových vzniku stejné upozornění pro stejný sledovat objekt od vyřešit. |
| ResolvedBy | Jméno uživatele, kterému vyřešit upozornění. Vyprázdnění Pokud upozornění ještě nebyla přeložit. |
| SourceDisplayName | Zobrazovaný název sledování objektu, které vygenerovalo oznámení. |
| SourceFullName | Úplný název sledování objektů, které vygenerovalo oznámení. |
| TicketId | Lístek ID pro upozornění, pokud je prostředí SCOM integrovaný s proces pro přiřazení požadavky pro upozornění.  Prázdný žádné lístek se přiřadí ID. |
| TimeGenerated | Funkce data a času vytvoření upozornění. |
| TimeLastModified | Datum a čas poslední změny na upozornění. |
| TimeRaised | Datum a čas, které vygenerovalo oznámení. |
| TimeResolved | Datum a čas, který převést na upozornění. Vyprázdnění Pokud upozornění ještě nebyla přeložit. |

## <a name="sample-log-searches"></a>Ukázka protokolu hledání

Následující tabulka obsahuje ukázkové protokolu vyhledá upozornění záznamy shromážděná toto řešení.  

| Dotaz | Popis |
|:--|:--|
| Typ = upozornění SourceSystem = OpsManager AlertSeverity = chyba TimeRaised > nyní 24 hodin | Kritické upozornění mocninu během posledních 24 hodin |
| Typ = upozornění AlertSeverity = upozornění TimeRaised > nyní 24 hodin | Upozornění upozornění mocninu během posledních 24 hodin  |
| Typ = upozornění SourceSystem = OpsManager AlertState! = skrytých TimeRaised > nyní 24 hodin a #124; míra count() jako počet podle SourceDisplayName | Zdrojů pomocí aktivní upozornění mocninu během posledních 24 hodin |
| Typ = upozornění SourceSystem = OpsManager AlertSeverity = chyba TimeRaised > nyní 24 hodin AlertState! = uzavřeno | Kritické upozornění mocninu během posledních 24 hodin, které jsou stále aktivní |
| Typ = upozornění SourceSystem = OpsManager TimeRaised > nyní 24 hodin AlertState = zavřený | Upozornění na mocninu během posledních 24 hodin, které jsou teď uzavřít |
| Typ = upozornění SourceSystem = OpsManager TimeRaised > nyní - 1 den a #124; míra count() jako počet podle AlertSeverity | Upozornění na mocninu během posledních 1 den seskupené podle závažnosti |
| Typ = upozornění SourceSystem = OpsManager TimeRaised > nyní - 1 den a #124; řazení RepeatCount desc | Upozornění na mocninu během posledních 1 den seřazené podle jejich počet opakování hodnot |

## <a name="next-steps"></a>Další kroky

- Informace o [upozornění v protokolu analýzy](log-analytics-alerts.md) podrobné informace o generování upozornění z protokolu analýzy.
