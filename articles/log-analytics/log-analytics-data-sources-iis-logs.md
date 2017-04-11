<properties
   pageTitle="Služby IIS protokoly do protokolu analýzy | Microsoft Azure"
   description="Internetové informační služby (IIS) ukládá činnosti uživatelů protokoly, které můžete shromážděné protokolu analýzy.  Tento článek popisuje, jak konfigurovat kolekce protokoly služby IIS a podrobnosti o záznamy, které by vytvářeli v úložišti OMS."
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

# <a name="iis-logs-in-log-analytics"></a>Služby IIS protokoly do protokolu analýzy
Internetové informační služby (IIS) ukládá činnosti uživatelů protokoly, které můžete shromážděné protokolu analýzy.  

![Protokoly služby IIS](media/log-analytics-data-sources-iis-logs/overview.png)

## <a name="configuring-iis-logs"></a>Konfigurace služby IIS protokoly
Technologie pro analýzu log umožňuje shromáždit položky z souborů protokolů vytvořených funkcí služby IIS, takže musíte [nakonfigurovat službu IIS pro přihlášení](https://technet.microsoft.com/library/hh831775.aspx).

Technologie pro analýzu protokolu pouze podporuje IIS protokoly uložené ve formátu W3C a vlastní pole a Upřesnit protokolování internetové informační služby nepodporuje.  
Protokol analýzy není shromáždění protokolů ve formátu nativní NCSA nebo služby IIS.

Konfigurace služby IIS protokoly v protokolu analýzy v [nabídce Data v dialogovém okně nastavení technologie pro analýzu protokolu](log-analytics-data-sources.md#configuring-data-sources).  Není nutná žádná konfigurace kromě výběru **formátu shromáždit W3C IIS souborů protokolu**.

Doporučujeme, aby po povolení služby IIS protokolu kolekce nastavte nastavení služby IIS protokolu při přechodu myší na všech serverech.


## <a name="data-collection"></a>Shromažďování dat

Protokol analýzy shromažďuje položky protokolu služby IIS z každé připojení zdroje zhruba každých 15 minut.  Agent záznamů jeho umístění v jednotlivých shromáždí z protokolu událostí.  V případě agent potom protokolu analýzy shromažďuje události z tam, kde ho naposledy přestali, i když tyto události byly vytvořeny v době, kdy agent offline.


## <a name="iis-log-record-properties"></a>Dialogovém okně Vlastnosti záznamu služby IIS protokolu

Záznamů protokolu IIS mít typ **W3CIISLog** a mít vlastnosti v následující tabulce:

| Vlastnost | Popis |
|:--|:--|
| Počítač | Název počítače, na kterém byla odebrané události. |
| cIP | IP adresa klienta. |
| csMethod | Metoda požadavek GET ATP příspěvek. |
| csReferer | Web, aby uživatel a až pak odkaz z aktuálního webu. |
| csUserAgent | Prohlížeče typ klienta. |
| csUserName | Název ověřeného uživatele, který k nim získat přístup na server. Anonymní uživatelé jsou označeny pomlčku. |
| csUriStem | Cíl žádost například na webovou stránku. |
| csUriQuery | Dotaz, případně že klienta pokoušel provádět. |
| ManagementGroupName | Název skupiny správy pro Operations Manager agentů.  U jiných agenti – jedná AOI -\<ID pracovního prostoru\> |
| RemoteIPCountry | Země IP adresa klienta. |
| RemoteIPLatitude | Šířky IP adresa klienta. |
| RemoteIPLongitude | Délky IP adresa klienta. |
| scStatus | Nastavit informace HTTP stavový kód. |
| scSubStatus | Chybovém kódu. |
| scWin32Status | Kód stavu systému Windows. |
| sIP | IP adresu webového serveru. |
| SourceSystem  | OpsMgr – |
| sPort | Port serveru klienta připojen. |
| sSiteName | Název webu služby IIS. |
| TimeGenerated | Datum a čas zaznamenání položce. |
| TimeTaken | Doba zpracování požadavku v milisekundách. |

## <a name="log-searches-with-iis-logs"></a>Protokol vyhledávání pomocí služby IIS protokoly

V následující tabulce jsou uvedeny různých příklady protokolu dotazy, které načítat záznamy služby IIS protokolu.

| Dotaz | Popis |
|:--|:--|
| Typ = IISLog | Všechny záznamy služby IIS protokolu. |
| Typ = IISLog EventLevelName = chyba | Všechny událostí systému Windows s závažnosti chyby. |
| Typ = W3CIISLog & #124; Změřit count() cIP | Počet služby IIS protokolu položky podle IP adresa klienta. |
| Typ = W3CIISLog csHost = "www.contoso.com" & #124; Změřit count() csUriStem | Počet služby IIS protokolu položky tak, že adresa URL hostitele www.contoso.com. |
| Typ = W3CIISLog & #124; Míra Sum(csBytes) ve počítače a vysoké rozlišení #124; horní 500000| Celkový počet bajtů dostali každý IIS počítač. |

## <a name="next-steps"></a>Další kroky

- Konfigurace protokolu analýzy získat informace o jiných [zdrojů dat](log-analytics-data-sources.md) pro analýzu.
- Informace o [hledání protokolu](log-analytics-log-searches.md) k analýze dat shromážděných ze zdroje dat a jejich řešení.
- Konfigurace upozornění v protokolu analýzy včasným upozorňující důležité podmínky nalezené ve službě IIS protokoly.
