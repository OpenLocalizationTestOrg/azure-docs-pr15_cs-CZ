<properties
    pageTitle="Počítač skupin v protokolu analýzy protokolu hledání | Microsoft Azure"
    description="Počítač skupin v protokolu analýzy vám umožní protokolu obor hledání na konkrétní sadu počítačů.  Tento článek popisuje různé metody, pomocí kterých můžete použít k vytvoření skupin počítače a jak je lze používat v protokolu hledání."
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
    ms.date="09/06/2016"
    ms.author="bwren"/>

# <a name="computer-groups-in-log-analytics-log-searches"></a>Počítač skupin v protokolu analýzy protokolu hledání
Počítač skupin v protokolu analýzy vám umožní obor [protokolu hledání](log-analytics-log-searches.md) na konkrétní sadu počítačů.  Každá skupina je vyplněné pomocí počítače buď pomocí dotazu, který určíte nebo importováním skupiny z různých zdrojů.  Při hledání protokolu je součástí skupiny, jsou omezené na záznamy, které odpovídají počítače ve skupině výsledky.

## <a name="creating-a-computer-group"></a>Vytvoření skupiny
Vytvoření skupiny počítače v protokolu technologie pro analýzu použití libovolného příkazu pro metody v následující tabulce.  Podrobnosti o obou metod jsou uvedeny níže. 

| Metoda | Popis |
|:---|:---|
| Hledání protokolu       | Vytvoření hledání protokol, který vrací seznam počítačů a výsledky uložit jako skupinu počítačů. |
| Hledání protokolu rozhraní API   | Pomocí rozhraní API pro hledání protokolu programově vytvořit skupinu počítačů, na základě výsledků hledání protokolu. |
| Služby Active Directory | Automaticky kontrolovat členství ve skupinách agent počítačů, které jsou součástí služby Active Directory a pro každou skupinu zabezpečení, vytvořte novou skupinu v protokolu analýzy.
| WSUS              | Automaticky vyhledat zacílení skupiny servery WSUS nebo klienty a vytvoření skupiny v protokolu Analytics pro jednotlivá pole. |


### <a name="log-search"></a>Hledání protokolu

Vytvořené z vyhledávání protokolu skupiny počítač bude obsahovat všechny počítače vrácené vyhledávacího dotazu, který určíte.  Tento dotaz je spuštěn pokaždé, když skupině počítač používá tak, aby všechny změny, protože byla vytvořená skupiny se projeví.

Umožňuje vytvořit skupinu počítače z vyhledávání protokolu následujícím způsobem.

1. [Vytvoření hledání protokol](log-analytics-log-searches.md) , který vrací seznam počítačů.  Hledání musí vrátit odlišné nastavení počítače pomocí něco jako **Různých počítače** nebo **Míra count() ve počítače** v dotazu.  
2. Kliknutím na tlačítko **Uložit** v horní části obrazovky.
3. Výběrem možnosti **Ano** **Uložte dotaz jako skupinu počítače:**.
4. Zadejte **název** a **kategorie** pro skupinu.  Pokud již existuje hledání se stejným názvem a kategorie, pak budete vyzváni k jeho přepsání.  Můžete mít víc hledání se stejným názvem v různých kategoriích. 

Následuje příklad hledání, které můžete uložit jako skupinu počítačů.

    Computer="Computer1" OR Computer="Computer2" | distinct Computer 
    Computer=*srv* | measure count() by Computer

### <a name="log-search-api"></a>Přihlaste se rozhraní API pro hledání

Počítač skupiny vytvořené pomocí rozhraní API pro hledání protokolu jsou stejná jako rozšířeného vyhledávání protokolu hledání.

Podrobné informace o vytváření skupiny počítače pomocí rozhraní API pro hledání protokolu najdete v článku [skupiny počítačů v protokolu analýzy protokolu hledání rozhraní REST API](log-analytics-log-search-api.md#computer-groups).

### <a name="active-directory"></a>Služby Active Directory

Při konfiguraci protokolu analýzy k importu členství ve skupinách služby Active Directory, bude analyzovat členství ve skupinách z libovolné domény připojen počítače s agentem OMS.  Vytvoření skupiny počítače v protokolu Analytics pro každou skupinu zabezpečení ve službě Active Directory a jednotlivé počítače se přidá do skupiny počítače odpovídající skupiny zabezpečení, které patří.  Tento členství průběžně aktualizují každé 4 hodiny.  

Konfigurace protokolu analýzy k importu skupin zabezpečení služby Active Directory z nabídky **Skupiny počítačů** v protokolu analýzy **Nastavení**.  Vyberte **automatizaci** a **členství ve skupinách služby Active Directory Import z počítače**.  Není nutná žádná další konfigurace.

![Počítač skupin služby Active Directory](media/log-analytics-computer-groups/configure-activedirectory.png)

Po importu skupin v nabídce zobrazí seznam počet počítačích s členství ve skupinách zjišťování a počet jejích skupiny importovat.  Můžete kliknout na jednu z těchto odkazů k vrácení **ComputerGroup** záznamů pomocí těchto informací.

### <a name="windows-server-update-service"></a>Služba Windows Update serveru

Při konfiguraci protokolu analýzy k importu členství ve skupinách WSUS bude analyzovat cílení členství ve skupině všech počítačích s agentem OMS.  Pokud používáte klienta při zacílení na libovolném počítači, který je připojen k OMS a je součástí všech WSUS zacílení skupiny bude mít jeho členství ve skupině importovat do protokolu analýzy. Pokud používáte serverovou zacílení OMS agent měli byste mít nainstalované na serveru WSUS, aby informace členství chcete importovat do OMS.  Tento členství průběžně aktualizují každé 4 hodiny. 

Konfigurace protokolu analýzy k importu skupin zabezpečení služby Active Directory z nabídky **Skupiny počítačů** v protokolu analýzy **Nastavení**.  Vyberte **Služby Active Directory** a **členství ve skupinách služby Active Directory Import z počítače**.  Není nutná žádná další konfigurace.

![Počítač skupin služby Active Directory](media/log-analytics-computer-groups/configure-wsus.png)

Po importu skupin v nabídce zobrazí seznam počet počítačích s členství ve skupinách zjišťování a počet jejích skupiny importovat.  Můžete kliknout na jednu z těchto odkazů k vrácení **ComputerGroup** záznamů pomocí těchto informací.

## <a name="managing-computer-groups"></a>Správa skupin počítačů

Můžete zobrazit skupin počítačů, které byly vytvořené z protokolu vyhledávání nebo v protokolu rozhraní API pro hledání v nabídce **Skupiny počítačů** v protokolu analýzy **Nastavení**.  Klikněte na **x** ve sloupci **Odebrat** skupinu počítače odebrat.  Klikněte na ikonu **Zobrazit členy** skupiny spustit hledání protokolu skupiny, který vrací její členy. 

![Skupiny uložené počítačů](media/log-analytics-computer-groups/configure-saved.png)

Chcete-li upravit skupinu, vytvořte novou skupinu stejné **kategorie** a **názvem** uvedeného postupu přepsat původní skupiny.

## <a name="using-a-computer-group-in-a-log-search"></a>Použití skupiny v protokolu hledání
Pomocí následující syntaxe v nápovědě k počítači skupiny v protokolu vyhledávání.  Určení, jestli **kategorie** je nepovinné a pouze musí mít počítač skupiny se stejným názvem v jednotlivými kategoriemi. 

    $ComputerGroups[Category: Name]

Při spuštění hledání se rozlišují nejdřív členy všechny počítače skupiny zahrnuté do pole Hledat.  Pokud skupiny se podle protokolu vyhledávání, hledání spusťte vrátíte členy této skupiny před provedením hledání nejvyšší úrovně protokolu.

Skupina počítače se obvykle používají se klauzule **IN** do pole Hledat protokolu jako v následujícím příkladu.

    Type=UpdateSummary Computer IN $ComputerGroups[My Computer Group]

## <a name="computer-group-records"></a>Seskupování počítače

Záznam se vytvoří v úložišti OMS pro jednotlivé počítače členství ve skupinách vytvořené ze služby Active Directory nebo WSUS.  Tyto záznamy s typem **ComputerGroup** a mít vlastnosti v následující tabulce.  Záznamy nevytváří skupin počítač podle protokolu vyhledávání.

| Vlastnost | Popis |
|:--|:--|
| Typ                | *ComputerGroup* |
| SourceSystem        | *SourceSystem*  |
| Počítač            | Název počítače člena. |
| Skupina               | Název skupiny. |
| GroupFullName       | Úplná cesta ke skupině včetně zdroje a název zdroje.
| GroupSource         | Zdroje danou skupinu byly odebrané. <br><br>Active Directory<br>WSUS<br>WSUSClientTargeting |
| GroupSourceName     | Název zdroje shromážděné ze skupiny.  Služby Active Directory to je název domény. |
| ManagementGroupName | Název skupiny správy SCOM agentů.  U jiných agenti – jedná AOI -\<ID pracovního prostoru\> |
| TimeGenerated       | Datum a čas vytvoření nebo aktualizace počítače skupiny. |



## <a name="next-steps"></a>Další kroky

- Informace o [hledání protokolu](log-analytics-log-searches.md) k analýze dat shromážděných ze zdroje dat a jejich řešení.  