<properties 
   pageTitle="Zdroje dat v protokolu analýzy | Microsoft Azure"
   description="Zdroje dat definovat data protokolu analýzy shromažďuje z agentů a jiné připojení zdroje.  Tento článek popisuje koncepci jak protokolu technologie pro analýzu použití zdrojů dat, vysvětluje podrobnosti o tom, jak nakonfigurovat a poskytuje přehled různých zdrojů dat k dispozici."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="data-sources-in-log-analytics"></a>Zdroje dat v protokolu analýzy

Technologie pro analýzu protokolu shromažďuje údaje o připojení zdroje v pracovním prostoru OMS a uloží jej v úložišti OMS.  Data, která se shromažďují ze všech je definován zdroje dat, které můžete konfigurovat.  Data v úložišti OMS jsou uložena jako sady záznamů.  Každý zdroj dat vytvoří záznamy určitého typu s každým s vlastní sadu vlastností.

![Přihlaste se analýzy shromažďování dat](./media/log-analytics-data-sources/overview.png)

Zdroje dat se liší od OMS řešení, které také shromáždit data od připojení zdroje a vytvořte záznamy v úložišti OMS.  Řešení přidáním do pracovního prostoru z Galerie řešení a obvykle poskytují další analytické nástroje v portálu OMS.  

## <a name="summary-of-data-sources"></a>Souhrn zdrojů dat

Zdroje dat, které jsou aktuálně k dispozici v protokolu analýzy jsou uvedené v následující tabulce.  U každého je odkaz na samostatný článek poskytuje podrobnosti k tomuto zdroji dat.

| Zdroj dat | Typ události | Popis |
|:--|:--|:--|
| [Vlastní protokoly](log-analytics-data-sources-custom-logs.md) | \<Název protokolu\>_CL | Soubory ve formátu RTF na Windows nebo Linux agentů obsahující informace v protokolu. |
| [Protokoly událostí systému Windows](log-analytics-data-sources-windows-events.md) | Události | Události odebrané v protokolu událostí Windows počítačů. |
| [Windows výkonnosti](log-analytics-data-sources-performance-counters.md) | Výkon | Výkonnosti shromážděné z počítače se systémem Windows. |
| [Linux výkonnosti](log-analytics-data-sources-performance-counters.md) | Výkon | Výkonnosti shromážděné z počítače Linux. |
| [Protokoly služby IIS](log-analytics-data-sources-iis-logs.md) | W3CIISLog | Internetové informační služby protokoly do formátu W3C. |
| [Syslog](log-analytics-data-sources-syslog.md) | Syslog | Syslog události počítačích Windows nebo Linux. |

## <a name="configuring-data-sources"></a>Konfigurace zdroje dat

Konfigurace zdroje dat v nabídce **Data** v protokolu analýzy **Nastavení**.  Jakékoli konfigurace dostane všech propojených zdrojů v pracovním prostoru OMS.  Všechny agentů nelze aktuálně vyloučit z této konfigurace.

![Konfigurace událostí systému Windows](./media/log-analytics-data-sources/configure-events.png)

2. V konzole OMS vyberte dlaždici **Nastavení** .
3. Vyberte **Data**.
4. Klikněte na zdroj dat pro nastavení.
5. Kliknutím na odkaz v dokumentaci pro jednotlivé zdroje dat v tabulce Podrobnosti o jejich konfigurace.

## <a name="data-collection"></a>Shromažďování dat

Konfigurace zdroje dat jsou Doručená do agentů připojené přímo k OMS objevit během pár minut.  Zadaná data je odebrané agent a doručila ji přímo do protokolu analýzy intervalech specifické pro jednotlivé zdroje dat.  Najdete v dokumentaci pro jednotlivé zdroje dat pro tyto podrobnosti.

Konfigurace zdroje dat pro System Center operace správce (SCOM) agentů ve skupině připojení správy, jsou převést na balíčky správy a doručila ji do skupiny Správa každých 5 minut ve výchozím nastavení.  Agent management pack jako jakékoli jiné a soubory ke stažení shromažďuje zadaná data. V závislosti na zdroj dat data budou buď odeslaných na server pro správu, které předá protokolu analýzy dat nebo agent odešle data protokolu analýzy bez opakovaného server pro správu. Podívejte se do [Podrobnosti kolekce dat pro OMS funkce a řešení](log-analytics-add-solutions.md#data-collection-details-for-oms-features-and-solutions) podrobnosti.  Další informace o podrobnosti o připojení SCOM a OMS a úprava tuto frekvenci tuto konfiguraci Doručená na [Nakonfigurovat integraci s System Center Operations Manager](log-analytics-om-agents.md).

## <a name="log-analytics-records"></a>Technologie pro analýzu záznamů protokolu

Všechna data shromážděná protokolu analýzy uložený v úložišti OMS jako záznamy.  Záznamy shromážděná různých zdrojů dat bude mít vlastní sadu vlastností a identifikovat podle jejich **typu** vlastnosti.  Na jednotlivé typy záznamů najdete v dokumentaci pro jednotlivé zdroje dat a řešení podrobnosti.


## <a name="next-steps"></a>Další kroky

- Informace o [řešení](log-analytics-add-solutions.md) , která přidat funkce Log technologie pro analýzu a také shromáždit data do úložiště OMS.
- Informace o [hledání protokolu](log-analytics-log-searches.md) k analýze dat shromážděných ze zdroje dat a jejich řešení.  
- Konfigurace [upozornění](log-analytics-alerts.md) včasným upozorňující důležitá data shromážděné ze zdroje dat a jejich řešení.
