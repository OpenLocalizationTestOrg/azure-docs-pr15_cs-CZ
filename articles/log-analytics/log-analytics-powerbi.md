<properties
   pageTitle="Export protokolu analýzy dat k Power BI | Microsoft Azure"
   description="Power BI je cloudový firmy analýzy služba od Microsoftu, který obsahuje propracovaných vizualizacích a zprávy pro analýzu různé sady dat.  Protokol analýzy můžete nepřetržitě exportovat data z úložiště OMS do Power BI, můžete využít jeho vizualizace a nástrojů pro analýzu.  Tento článek popisuje postup při konfiguraci dotazy v protokolu analýzy, která automaticky exportovat do Power BI v pravidelných intervalech."
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

# <a name="export-log-analytics-data-to-power-bi"></a>Export protokolu analýzy dat k Power BI

[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) je cloudový firmy analýzy služba od Microsoftu, který obsahuje propracovaných vizualizacích a zprávy pro analýzu různé sady dat.  Technologie pro analýzu protokolu můžete automaticky exportovat data z úložiště OMS do Power BI, můžete využít jeho vizualizace a nástrojů pro analýzu.

Při konfiguraci Power BI pomocí protokolu technologie pro analýzu vytvoříte protokolu dotazy, které jejich výsledky exportovat odpovídající datové sady v Power BI.  Dotazů a export nadále automaticky spuštěna podle plánu, který určíte aktuálnost datové sady s nejnovější data shromážděná protokolu analýzy.

![Protokol analýzy k Power BI](media/log-analytics-powerbi/overview.png)

## <a name="power-bi-schedules"></a>Plány Power BI

*Power BI plán* zahrnuje hledání protokolu exportuje množiny dat z úložiště OMS odpovídající datovou sadu v Power BI a plán, který definuje, jak často tohle vyhledávání spustit aktuálnost datové sady.

Pole v datové sadě se budou shodovat vlastnosti záznamy nalezené protokolu.  Pokud vyhledávání vrátí záznamy různých typů bude obsahovat datové všechny vlastnosti ze všech typů záznamů však započítávány.  

> [AZURE.NOTE] Je nejvhodnější použít protokol vyhledávací dotaz, který vrací neformátovaná data namísto provádění všech sloučení pomocí příkazů, jako je [Míra](log-analytics-search-reference.md#measure).  Je možné provést agregaci a výpočty v Power BI z nezpracovanými daty.

## <a name="connecting-oms-workspace-to-power-bi"></a>Připojení pracovního prostoru OMS k Power BI

Před exportem z protokolu analýzy k Power BI, musí pracovního prostoru OMS připojit ke svému účtu Power BI pomocí následujícího postupu.  

1. V konzole OMS klikněte na dlaždici **Nastavení** .
2. Vyberte **účty**.
3. V části **Informace o pracovním prostoru** klikněte na **připojit k Power BI účtu**.
4. Zadejte přihlašovací údaje pro váš účet Power BI.

## <a name="create-a-power-bi-schedule"></a>Vytvoření plánu Power BI

Vytvoření plánu Power BI pro každou sadu pomocí následujícího postupu.

1. V konzole OMS klikněte na dlaždici **Protokolu vyhledávání** .
2. Zadejte nový dotaz nebo vyberte uložené hledání, které vrátí data, která chcete exportovat do **Power BI**.  
3. Klikněte na tlačítko **Power BI** v horní části stránky otevřete dialogové okno **Power BI** .
4. Zadejte informace v této tabulce a klikněte na **Uložit**.

| Vlastnost | Popis |
|:--|:--|
| Jméno | Název pro identifikaci plánu, když zobrazíte seznam plány Power BI. |
| Uložené hledání | Hledání protokolu spustit.  Můžete vybrat aktuální dotaz nebo vybrat existující uložené hledání z pole rozevíracího seznamu. |
| Plán | Jak často se má spuštění uloženého hledání a export sady dat Power BI.  Hodnota musí být 15 minut až 24 hodin. |
| Název sady dat | Název sady dat v Power BI.  Bude vytvořen, pokud ho neexistuje a aktualizovat Pokud neexistuje. |

## <a name="viewing-and-removing-power-bi-schedules"></a>Zobrazení a odebrání plány Power BI

Zobrazení seznamu existující Power BI pomocí následujícího postupu.

1. V konzole OMS klikněte na dlaždici **Nastavení** .
2. Vyberte **Power BI**.

Kromě podrobnosti plánu zobrazují počet spuštěné na plán v uplynulém týdnu a stav poslední synchronizace.  Pokud synchronizace došlo k chybám, kliknete na odkaz spusťte protokolu vyhledání záznamů s podrobnostmi chyby do schránky.

Plán můžete odebrat kliknutím na **X** v **Odeberte sloupec**.  Plán můžete zakázat tak, že vyberete **vypnout**.  K úpravám plánu, musíte ho odebrat a začínat nové nastavení.

![Plány Power BI](media/log-analytics-powerbi/schedules.png)

## <a name="sample-walkthrough"></a>Ukázka návod
V následující části provede příklad vytvoření plánu Power BI a jeho datovou sadu použít k vytvoření jednoduché sestavy.  V tomto příkladu všechna data o výkonu pro sadu počítačů exportu do Power BI a vytvoří spojnicovém grafu zobrazte využití procesoru.

### <a name="create-log-search"></a>Vytvoření protokolu hledání
Začneme vytvořením protokolu vyhledejte data, která nám chcete odesílat členům datové sady.  V tomto příkladu použijeme dotaz, který vrátí všechna data o výkonu pro počítače s názvem začínajícím s *srv*.  

![Plány Power BI](media/log-analytics-powerbi/walkthrough-query.png)

### <a name="create-power-bi-search"></a>Vytvoření vyhledávacího Power BI
Jsme klikněte **Power BI** s cílem otevřít okno Power BI a zadejte požadované informace.  Chceme tohle vyhledávání spustit jednou za hodinu a vytvářet datovou sadu s názvem *Contoso výkon*.  Protože již máme hledání otevřít, které vytvoří data chceme, takže jsme výchozí *Použít aktuální vyhledávacího dotazu* **Uložené**mají být vyhledávány.

![Power BI hledání](media/log-analytics-powerbi/walkthrough-schedule.png)

### <a name="verify-power-bi-search"></a>Ověření hledání Power BI
Pokud chcete ověřit, že se nám plánu správně, zobrazit seznam Power BI hledání v části **Nastavení** klikněte na řídicím panelu OMS.  Počkejte několik minut jsme aktualizace toto zobrazení, dokud zprávy spuštění synchronizace.

![Power BI hledání](media/log-analytics-powerbi/walkthrough-schedules.png)

### <a name="verify-the-dataset-in-power-bi"></a>Ověření sady dat v Power BI
Jsme Přihlaste se k naše účtu na [powerbi.microsoft.com](http://powerbi.microsoft.com/) a posuňte se na **datové sady** v dolní části levého podokna.  Vidíme, že je uvedený datovou sadu *Contoso výkon* označující, že naše export úspěšném.

![Sady dat Power BI](media/log-analytics-powerbi/walkthrough-datasets.png)

### <a name="create-report-based-on-dataset"></a>Vytvoření sestavy založené na datovou sadu
Jsme vyberte datovou sadu **Contoso výkon** a klikněte na **výsledky** v podokně **pole** na pravé straně zobrazíte pole, které jsou součástí tohoto datovou sadu.  Pokud chcete vytvořit spojnicový graf zobrazující využití procesoru pro každý počítač, jsme provádět následující akce.

1. Vyberte vizualizaci spojnicový graf.
2. Přetáhněte **název_objektu** na **úrovni filtru sestavy** a zkontrolujte **procesor**.
3. Přetáhněte **název_čítače** na **úrovni filtru sestavy** a zkontrolujte **% Procesor času**.
4. Přetáhněte **přepočtené** **hodnoty**.
5. Přetáhněte **počítač** **legendy**.
6. Přetáhněte **TimeGenerated** **OS**.

Vidíme, že výsledné spojnicový graf se zobrazí daty z našich datovou sadu.

![Power BI spojnicový graf](media/log-analytics-powerbi/walkthrough-linegraph.png)

### <a name="save-the-report"></a>Uložení sestavy
Můžeme sestavu uložíte kliknutím na tlačítko Uložit v horní části obrazovky a ověřte, že je teď uvedené v části sestavy v levém podokně.

![Sestavy Power BI](media/log-analytics-powerbi/walkthrough-report.png)

## <a name="next-steps"></a>Další kroky

- Informace o [hledání protokolu](log-analytics-log-searches.md) vytvářet dotazy, které je možné exportovat do Power BI.
- Další informace o [Power BI](http://powerbi.microsoft.com) můžete vytvořit vizualizace podle protokolu analýzy export.
