<properties 
   pageTitle="Protokoly událostí Windows do protokolu analýzy | Microsoft Azure"
   description="Protokoly událostí Windows jsou jednou z slouží jako protokolu analýzy zdroje dat vyskytuje nejčastěji.  Tento článek popisuje, jak konfigurovat kolekce protokoly událostí Windows a podrobnosti o záznamy, které by vytvářeli v úložišti OMS."
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

# <a name="windows-event-log-data-sources-in-log-analytics"></a>Zdroje dat v protokolu událostí systému Windows v protokolu analýzy

Protokoly událostí Windows jsou jednou z použité nejběžnější [zdroje dat](log-analytics-data-sources.md) pro Windows agentů protože jde o způsob využívány většina přihlásit informace a chyby.  Události můžete shromažďovat standardní protokoly, například systému a aplikace kromě zadání všech vlastních protokolů vytvořené pomocí aplikace, které chcete sledovat.

![Událostí systému Windows](media/log-analytics-data-sources-windows-events/overview.png)     

## <a name="configuring-windows-event-logs"></a>Konfigurace událostí systému Windows protokoly

Konfigurace protokolování událostí systému Windows v [nabídce Data v dialogovém okně nastavení technologie pro analýzu protokolu](log-analytics-data-sources.md#configuring-data-sources).

Protokol analýzy pouze shromažďovat události z protokoly událostí Windows, které jsou uvedené v nastavení.  Přidat nový protokol zadáním názvu do protokolu a kliknutím na **+**.  Pro jednotlivé protokoly budou shromážděny pouze události se vybrané severities.  Zaškrtněte políčko severities pro konkrétní protokol, který chcete shromažďovat.  Nelze zadat další kritéria pro filtrování událostí.

![Konfigurace událostí systému Windows](media/log-analytics-data-sources-windows-events/configure.png)


## <a name="data-collection"></a>Shromažďování dat

Protokol analýzy shromáždí každý události, která odpovídají vybrané závažnosti sledované protokoly událostí systému, která se vytvoří událost.  Agent zaznamená jeho umístění v jednotlivých shromáždí z protokolu událostí.  Pokud agent přejde do režimu offline pro určitého časového období, bude události z tam, kde ho naposledy přestali, shromažďovat protokolu analýzy i v případě, že tyto události byly vytvořeny v době, kdy agent offline.


## <a name="windows-event-records-properties"></a>Vlastnosti záznamy událostí systému Windows

Záznamy událostí Windows máte typ **události** a mít vlastnosti v následující tabulce.

| Vlastnost | Popis |
|:--|:--|
| Počítač            | Název počítače, na kterém byla odebrané události. |
| EventCategory       | Kategorie události. |
| EventData           | Všechna data události v původním formátu. |
| EventID             | Počet události. |
| EventLevel          | Závažnosti události v číselných formuláře. |
| EventLevelName      | Závažnosti událostí v textovém formátu. |
| Protokol událostí            | Název shromážděné událost z protokolu událostí. |
| ParameterXml        | Hodnoty parametrů události ve formátu XML. |
| ManagementGroupName | Název skupiny správy SCOM agentů.  U jiných agenti – jedná AOI-<workspace ID> |
| RenderedDescription | Popis události se hodnoty parametrů |
| Zdroje              | Zdroj události. |
| SourceSystem  | Typ agent událost byly odebrané. <br> OpsManager – agent systému Windows, buď přímé připojení nebo SCOM <br> Linux – všechny Linux agentů  <br> AzureStorage – Azure diagnostiky |
| TimeGenerated       | Datum a čas vytvoření události v systému Windows. |
| Uživatelské jméno            | Uživatelské jméno účtu, který přihlášení k lyncu na událost. |



## <a name="log-searches-with-windows-events"></a>Vyhledávání protokolu událostí systému Windows

Následující tabulka obsahuje jiné příklady protokolu vyhledávání, které načítat záznamy událostí systému Windows.

| Dotaz | Popis |
|:--|:--|
| Typ = události | Všechny událostí systému Windows. |
| Typ = události EventLevelName = chyba | Všechny událostí systému Windows s závažnosti chyby. |
| Typ = události & #124; Míra count() podle zdroje | Počet Windows události zdrojem. |
| Typ = události EventLevelName = chyby & #124; Míra count() podle zdroje | Počet Windows chybové události zdrojem. |

## <a name="next-steps"></a>Další kroky

- Konfigurace protokolu analýzy získat informace o jiných [zdrojů dat](log-analytics-data-sources.md) pro analýzu.
- Informace o [hledání protokolu](log-analytics-log-searches.md) k analýze dat shromážděných ze zdroje dat a jejich řešení.  
- Použijte [Vlastní pole](log-analytics-custom-fields.md) analyzovat záznamy událostí do jednotlivých polí.
- Konfigurace [sady výkonnosti](log-analytics-data-sources-performance-counters.md) zástupci Windows.