<properties
    pageTitle="Protokolování analýzy zobrazení návrhu | Microsoft Azure"
    description="Zobrazení návrhu v protokolu analýzy umožňuje vytvářet vlastní zobrazení v konzole OMS, které obsahují různé vizualizace dat v úložišti OMS. Tento článek obsahuje přehled zobrazení návrhu a postupy pro vytváření a úpravy vlastní zobrazení."
    services="log-analytics"
    documentationCenter=""
    authors="bwren"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="bwren"/>

# <a name="log-analytics-view-designer"></a>Návrhář zobrazení technologie pro analýzu protokolu
Zobrazení návrháře v protokolu analýzy umožňuje vytvářet vlastní zobrazení v konzole OMS, které obsahují různé vizualizace dat v úložišti OMS. Tento článek obsahuje přehled zobrazení návrhu a postupy pro vytváření a úpravy vlastní zobrazení.

Další články umožňující zobrazení návrhu jsou:

- [Dlaždice odkaz](log-analytics-view-designer-tiles.md) – odkaz na stránce nastavení pro jednotlivá pole k dispozici pro použití v vlastní zobrazení dlaždic. 
- [Odkaz část vizualizace](log-analytics-view-designer-parts.md) – odkaz na stránce nastavení pro jednotlivá pole k dispozici pro použití v vlastní zobrazení dlaždic. 


## <a name="concepts"></a>Koncepty
Zobrazení vytvořená pomocí nástroje Návrhář zobrazení obsahovat prvky v následující tabulce.

| Část | Popis |
|:--|:--|
| Dlaždice | Zobrazí na řídicím panelu hlavní přehled analýzy protokolu.  Obsahuje vizuální sumarizovat informace obsažené ve vlastním zobrazení.  Různé typy dlaždice poskytují různé vizualizace záznamů v úložišti OMS.  Klikněte na dlaždici otevřete vlastní zobrazení. |
| Vlastní zobrazení | Zobrazí poté, co uživatel klikne na dlaždici.  Obsahuje jednu nebo více částí vizualizace. |
| Vizualizace částí | Vizualizace dat v úložišti OMS podle jednoho nebo více [protokolu vyhledávání](log-analytics-log-searches.md).  Většina části bude obsahovat záhlaví, díky kterému poskytuje nejvyšší úrovně vizualizace a seznam nejlepších výsledků.  Typy jiné části poskytují různé vizualizace záznamů v úložišti OMS.  Klikněte na prvky v části log vyhledávání poskytuje podrobné záznamy. |

![Zobrazení návrháře přehledu](media/log-analytics-view-designer/overview.png)

## <a name="add-view-designer-to-your-workspace"></a>Přidání zobrazení návrhu do pracovního prostoru
Když Návrhář zobrazení je v náhledu, musíte ho přidat do pracovního prostoru tak, že vyberete **Funkce náhledu** v části **Nastavení** portálu OMS.

![Povolit náhled](media/log-analytics-view-designer/preview.png)

## <a name="creating-and-editing-views"></a>Vytváření a úpravy zobrazení

### <a name="create-a-new-view"></a>Vytvoření nového zobrazení
Kliknutím na dlaždici Návrhář zobrazení v řídicím panelu hlavní OMS otevřete nové zobrazení v **Zobrazení návrhu** .

![Zobrazení návrháře dlaždice](media/log-analytics-view-designer/view-designer-tile.png)

### <a name="edit-an-existing-view"></a>Upravit existující zobrazení
Chcete-li upravit existující zobrazení v Návrháři zobrazení otevřete zobrazení kliknutím na její dlaždici v řídicím panelu hlavní OMS.  Klikněte na tlačítko **Upravit** otevřete zobrazení v Návrháři zobrazení.

![Úprava zobrazení](media/log-analytics-view-designer/menu-edit.png)

### <a name="clone-an-existing-view"></a>Klonovat existující zobrazení
Když klonovat zobrazení vytvoří nové zobrazení a otevře v zobrazení návrhu.  Nové zobrazení bude mít stejný název jako má původní s "Kopírovat" připojených na konec.  Klonovat zobrazení, otevřete kliknutím na její dlaždici v řídicím panelu hlavní OMS existující zobrazení.  Klikněte na tlačítko **klonovat** otevřete zobrazení v zobrazení návrhu.

![Klonovat zobrazení](media/log-analytics-view-designer/edit-menu-clone.png)

### <a name="delete-an-existing-view"></a>Odstranění existující zobrazení
Pokud chcete odstranit existující zobrazení, zobrazení kliknutím na její dlaždici v řídicím panelu hlavní OMS.  Klikněte na tlačítko **Upravit** otevřete zobrazení v Návrháři zobrazení a klikněte na příkaz **Odstranit zobrazení**.

![Odstranění zobrazení](media/log-analytics-view-designer/edit-menu-delete.png)

### <a name="export-an-existing-view"></a>Export existující zobrazení
Zobrazení můžete exportovat do souboru JSON, která můžete importovat do jiného pracovního prostoru nebo použít [šablonu správce prostředků Azure](../resource-group-authoring-templates.md).  Export existující zobrazení, otevřete kliknutím na její dlaždici v řídicím panelu hlavní OMS zobrazení.  Klikněte na tlačítko **Exportovat** vytvoříte soubor ve složce stažení prohlížeče.  Název souboru bude název zobrazení s příponou *omsview*.

![Exportovat zobrazení](media/log-analytics-view-designer/edit-menu-export.png)

### <a name="import-an-existing-view"></a>Import existující zobrazení
Můžete importovat soubor *omsview* , který jste exportovali z jiné skupiny správy.  K importu existující zobrazení, nejprve vytvořte nové zobrazení.  Potom klikněte na tlačítko **importovat** a vyberte soubor *omsview* .  Konfigurace v souboru budou zkopírovány do existujícího zobrazení.

![Exportovat zobrazení](media/log-analytics-view-designer/edit-menu-import.png)

## <a name="working-with-view-designer"></a>Práce s aplikací Návrhář zobrazení
Zobrazení návrhu má tři podokna.  Podokno **návrhu** představuje vlastní zobrazení.  Když přidáte dlaždic a částí z podokna **ovládací prvek** podokna **Návrh** se přidají do zobrazení.  V podokně **Vlastnosti** se zobrazí vlastnosti dlaždice nebo vybranou část.

![Zobrazení návrhu](media/log-analytics-view-designer/view-designer-screenshot.png)

### <a name="configure-view-tile"></a>Konfigurace zobrazení vedle sebe
Vlastní zobrazení může mít jenom jednu dlaždici.  Vyberte kartu **dlaždice** v **ovládacích** panelech zobrazit aktuální dlaždice nebo vyberte některou alternativní.  V podokně **Vlastnosti** se zobrazí vlastnosti pro aktuální dlaždice.  Konfigurace vlastností dlaždice podle podrobné informace [Dlaždice odkaz](log-analytics-view-designer-tiles.md) a klikněte na tlačítko **použít** uložte změny.

### <a name="configure-visualization-parts"></a>Konfigurace částí vizualizace
Zobrazení můžete zahrnout libovolný počet částí vizualizace.  Vyberte kartu **zobrazení** a potom část vizualizace, přidejte do zobrazení.  V podokně **Vlastnosti** se zobrazí vlastnosti pro vybranou část.  Konfigurace vlastností zobrazení podrobné informace podle předpisů rozhraní [vizualizace část odkaz](log-analytics-view-designer-parts.md) a klikněte na tlačítko **použít** uložte změny.

### <a name="delete-a-visualization-part"></a>Odstranění části vizualizace
Část vizualizace v zobrazení můžete odebrat kliknutím na tlačítko **X** v pravém horním rohu části.

### <a name="rearrange-visualization-parts"></a>Změna uspořádání částí vizualizace
Zobrazení obsahovat pouze jeden řádek částí vizualizace.  Změna uspořádání existující části v zobrazení kliknutím a přetažením do nového umístění.


## <a name="next-steps"></a>Další kroky

- Přidání [dlaždic](log-analytics-view-designer-tiles.md) na vlastní zobrazení.
- Přidání [Částí vizualizace](log-analytics-view-designer-parts.md) vlastní zobrazení.
