<properties
    pageTitle="Začínáme s Azure toku analýzy dat obrázku z IoT zařízení. | Microsoft Azure"
    description="IoT senzor značky a datové proudy s toku technologie pro analýzu a v reálném čase zpracování dat."
    keywords="řešení IOT začít pracovat s iot"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="10/19/2016"
    ms.author="jeffstok"
/>

# <a name="get-started-with-azure-stream-analytics-to-process-data-from-iot-devices"></a>Začínáme s Azure toku analýzy dat obrázku z IoT zařízení

V tomto kurzu se dozvíte, jak vytvořit toku zpracování použití logických operátorů k získání dat z Internetu věcí (IoT) zařízení. Použijeme případu použití Internet věcí (IoT) reálný, která ukazuje, jak vytvářet řešení rychle a ekonomicky.

## <a name="prerequisites"></a>Zjistit předpoklady pro

-   [Azure předplatného](https://azure.microsoft.com/pricing/free-trial/)
-   Ukázka dotazu a datových souborů ke stažení z [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot)

## <a name="scenario"></a>Scénář

Contoso, což je společnosti do pole průmyslové automatizaci, má zcela automatické výrobní proces. Různé stroje, které v tomto zařízení má senzory, které umožňují výstupu toků data v reálném čase. V tomto scénáři vedoucí prostorového uspořádání výroby chce mít v reálném čase přehledy z senzor data, která chcete vyhledat vzorků a provést akce u nich. Použijeme jazyk dotazu analýzy toku (SAQL) nad daty senzor najít zajímavé vzorků z příchozí toku dat.

Tady je vygenerování dat z nástroje Texas senzorického značku zařízení.

![Značka senzor Texas nástrojů](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-01.jpg)

Datové data jsou ve formátu JSON a vypadá takto:


    {
        "time": "2016-01-26T20:47:53.0000000",  
        "dspl": "sensorE",  
        "temp": 123,  
        "hmdt": 34  
    }  

Ve scénáři reálný může mít stovky tato čidla vytvářet události jako proudu. V ideálním případě zařízení brány by spustit kód umožňující nabízená tyto události [Rozbočovače události Azure](https://azure.microsoft.com/services/event-hubs/) nebo [Rozbočovače IoT Azure](https://azure.microsoft.com/services/iot-hub/). Technologie pro analýzu toku práce by jedí tyto události z rozbočovače událostí a spouštění dotazů v reálném čase analýzy proti datové proudy. Pak můžete poslat výsledky na jeden z [podporovaných výstupy](stream-analytics-define-outputs.md).

Pro snadnější použití obsahuje příručku Začínáme dosáhnout toho, aby ukázka datového souboru, který byl nezaznamenávají z skutečné senzor značku zařízení. Můžete spouštění dotazů na ukázkových datech a zobrazte výsledky. V následující kurzy dozvíte, jak se připojit práce k zadávání a výstup a nasazení služby Azure.

## <a name="create-a-stream-analytics-job"></a>Vytvoření úlohy toku analýzy

1. V [Azure portál](http://portal.azure.com)klepněte na znaménko plus a zadejte **Toku analýzy** v okně text doprava. V seznamu výsledků klikněte na **úlohy toku analýzy** .

    ![Vytvořte nový projekt toku analýzy](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-02.png)

2. Zadejte název jedinečný úlohy a ověřte, že předplatné je ten správný pro danou úlohu. Pak vytvořit nové skupiny prostředků nebo vyberte stávající vašeho předplatného.

3. Vyberte umístění pro danou úlohu. Rychlé zpracování a snížení nákladů v datech převod výběru na stejném místě jako pole Skupina zdroje a určeným úložiště účet je vhodné.

    ![Vytvořte nový projekt toku analýzy podrobnosti](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03.png)

    > [AZURE.NOTE] Vytvoříte tohoto úložiště účtu jenom jednou jednotlivých oblastech. Toto úložiště budou sdíleny všechny úlohy toku analýzy vytvořené v dané oblasti.

4. Zaškrtněte políčko umístit práce na řídicím panelu a pak klikněte na **vytvořit**.

    ![Vytvoření projektu v průběhu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03a.png)

5. Měli byste vidět "Nasazení začít..." zobrazené v pravém horním rohu okna prohlížeče. Brzy změní dokončené oknu jak je ukázáno v následujícím příkladu.

    ![Vytvoření projektu v průběhu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03b.png)

### <a name="create-an-azure-stream-analytics-query"></a>Vytvoření dotazu toku analýzy Azure

Po vytvoření práce obsahuje čas a otevřete ho vytvořit dotaz. Práce můžete zpřístupnit po kliknutí na dlaždici pro něj.

![Dlaždice projektu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-04.png)

V podokně **Úloh topologie** klikněte na pole **dotazu** přejdete do editoru dotazů. Editor **dotazů** umožňuje zadat dotaz T-SQL, který provádí transformace přes příchozí data události.

![Pole dotazu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-05.png)

### <a name="query-archive-your-raw-data"></a>Dotaz: Archivace nezpracovaná data

Nejjednodušší způsob dotazu je předávací dotaz, který archivy všech zadávání dat do svého určené výstupu. Ukázková data moct soubor stáhněte z [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot) na místo ve vašem počítači. 

1. Vložte dotaz ze souboru PassThrough.txt. 

    ![Zadávání toku – test](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06.png)

2. Klikněte na tři tečky u zadání a zaškrtněte políčko **Odeslat ukázkových dat ze souboru** .

    ![Zadávání toku – test](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06a.png)

3. Podokně vpravo se otevře jako výsledek, v vyberte datový soubor HelloWorldASA InputStream.json stažený umístění a klikněte na **OK** v dolní části podokna.

    ![Zadávání toku – test](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06b.png)

4. Potom klikněte na **Testovat** ozubeného kola v horní oblasti okna doleva a zpracování testovat dotaz na datovou sadu vzorku. Při zpracování dokončení, otevře se okno výsledky pod dotazu.

    ![Výsledky testů](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-07.png)

### <a name="query-filter-the-data-based-on-a-condition"></a>Dotaz: Filtrování dat na základě nějaké podmínky

Zkusme k filtrování výsledků na základě nějaké podmínky. Rádi zobrazující výsledky pro pouze události, které jsou z "sensorA." Dotaz není v souboru Filtering.txt.

![Filtrování toku dat](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-08.png)

Všimněte si, že porovnává velká a malá písmena dotaz hodnotu řetězce. Klikněte na ozubené kolo **Test** znovu k provedení dotazu. Dotaz, měly vrátit 389 řádky mimo. 1860 události.

![Druhá výstup výsledkem testu dotazu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-09.png)

### <a name="query-alert-to-trigger-a-business-workflow"></a>Dotaz: Upozornění pro spuštění pracovního postupu business

Podrobnější provedeme naše dotazu. Pro každý typ čidla chceme sledovat průměrná teplota za 30 sekund okno a zobrazení výsledků jenom v případě, že průměrná teplota je větší než 100 stupňů. Změníme napsat následující dotaz a klepněte na tlačítko **Testovat** uvidíte výsledky. Dotaz není v souboru ThresholdAlerting.txt.

![dotaz 30 sekund filtru](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-10.png)

Teď byste měli vidět výsledky, které obsahují pouze 245 řádky a názvy čidel jejichž průměr mírného je větší než 100. Tento dotaz seskupí podle **dspl**, což je název senzor přes **Tumbling okno** 30 sekund toku události. Časový dotazů musí být uvedeno, jak chceme dobu průběhu. Pomocí klauzule **Časové razítko tak, že** jsme určili sloupci **OUTPUTTIME** časy přidružit všechny časový výpočty. Podrobné informace přečtěte si články MSDN týkající se [Správy času](https://msdn.microsoft.com/library/azure/mt582045.aspx) a [práce s okny funkcí](https://msdn.microsoft.com/library/azure/dn835019.aspx).

### <a name="query-detect-absence-of-events"></a>Dotaz: Zjišťování absence událostí

Jak můžeme psát dotazu pro vyhledání chybí vstupní událostí? Posledního najít Řekněme, že senzoru odeslaná data a potom neodeslal událostí pro další minuty. Dotaz není v souboru AbsenseOfEvent.txt.

![Zjištění absence událostí](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-11.png)

Tady používáme **LEVÉ vnější** spojení stejného datového proudu (vlastní spojení). Pro **vnitřní** spojení vrácený výsledek je pouze nalezený shodu.  **LEVÉ vnější** spojení pokud události na levé straně spojení neshodných, řádek, který má hodnotu NULL pro všechny sloupce pravé bude vrácena. Tento postup je velmi užitečné zobrazíte nepřítomnosti události. Viz náš MSDN si přečtěte následující dokumentaci pro další informace o [připojení](https://msdn.microsoft.com/library/azure/dn835026.aspx).

## <a name="conclusion"></a>Uzavření

Tomto kurzu účel a ukazuje, jak napsat různých toku analýzy dotazovací jazyk dotazů a zobrazte výsledky v prohlížeči. Však to je právě začínáte pracovat. Máte tolik více s toku analýzy. Technologie pro analýzu toku podporuje spoustu vstupy a výstupy a dokonce slouží Azure počítač přečíst snažíme usnadnit jeho bohaté nástroje pro analýzu datových proudů. Můžete začít, které můžete prozkoumat více o toku analýzy pomocí naše [výukové mapa](https://azure.microsoft.com/documentation/learning-paths/stream-analytics/). Další informace o vytváření dotazů najdete v článku o [běžné vzorce dotazu](./stream-analytics-stream-analytics-query-patterns.md).
