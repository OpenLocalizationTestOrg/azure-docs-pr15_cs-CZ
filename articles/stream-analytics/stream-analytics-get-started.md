<properties
    pageTitle="Začínáme s analýzy toku: zjišťování v reálném čase podvod | Microsoft Azure"
    description="Naučte se vytvářet řešení duplicit v reálném čase podvod s toku analýzy. Použití rozbočovači událostí pro zpracování v reálném čase události."
    keywords="zjišťování odchylky, podvodům zjišťování, zjišťování odchylky reálném čase"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok" />



# <a name="get-started-using-azure-stream-analytics-real-time-fraud-detection"></a>Začínáme s používáním Azure toku analýzy: v reálném čase podvod zjišťování

Naučte se vytvářet začátku do konce řešení pro vyhledávání v reálném čase podvod s Azure toku analýzy. Přenést události rozbočovači Azure události, napsat toku analýzy dotazů pro agregaci nebo výstrahy a odešlete výsledky jímky výstup získání přehledu o nad daty v reálném čase zpracování. Zjišťování odchylky reálném čase telekomunikačních se vztahuje ale postup příkladu je rovnoměrně z vnější vhodný pro jiné typy podvod zjišťování například platební kartou nebo identity scénáře krádežím identity.

Technologie pro analýzu toku je plně spravovaných služba poskytuje zpracování nízkou latencí vysoce dostupné, scalable složité události nad přenos dat v cloudu. Další informace najdete v tématu [Úvod do technologie pro analýzu toku Azure](stream-analytics-introduction.md).


## <a name="scenario-telecommunications-and-sim-fraud-detection-in-real-time"></a>Scénář: Telekomunikačních SIM podvod zjišťování a v reálném čase

Telekomunikačních společnosti obsahuje velké množství dat pro příchozí hovory. Společnost musí od data:
* Porovnat tyto údaje dolů na samostatně zvládnutelné množství a získat další informace o použití zákazníka přes čas a geografických oblastech.
* Rozpoznání SIM podvod (více volání pocházejících z stejnou identitu ve stejnou dobu, ale na geograficky různých místech) v reálném čase tak, že můžete snadno odpovědět oznamování zákazníci nebo vypnutí služby.

V kanonický scénáře Internet věcí (IoT) je tuna telemetrie nebo senzor dat generovaná – a Zákazníci chcete agregovat je nebo výstrahy prostřednictvím odchylky v reálném čase.

## <a name="prerequisites"></a>Zjistit předpoklady pro

- Stáhněte si [TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip) z webu služby Stažení softwaru 
- Volitelné: Zdrojového kódu generátor událost z [GitHub](https://github.com/Azure/azure-stream-analytics/tree/master/DataGenerators/TelcoGenerator)

## <a name="create-an-azure-event-hubs-input-and-consumer-group"></a>Vytvoření vstup Azure události rozbočovače a spotř skupiny

Ukázková aplikace generování událostí a použít pro instance centrální událostí pro zpracování v reálném čase. Služba Bus události rozbočovače jsou upřednostňovaný způsob požití událostí pro analýzy toku a můžete další informace o události rozbočovače v [si přečtěte následující dokumentaci Bus služby Azure](/documentation/services/service-bus/).

Vytvoření rozbočovači událostí:

1.  [Azure portál](https://manage.windowsazure.com/) klikněte na **Nový** > **Aplikace služby** > **Služby Bus** > **Události centrální** > **Vytvořit**. Zadejte název, oblast a nové nebo existující názvů k vytvoření nové události centrální.  
2.  Osvědčený přečtěte si jednotlivé úlohy toku analýzy z jedné skupině spotř centrální události. Nemůžeme vás provede procesem vytvořit skupinu příjemce a je možné, [Přečtěte si další informace o spotř skupiny](https://msdn.microsoft.com/library/azure/dn836025.aspx). Pokud chcete vytvořit skupinu příjemce, přejděte do centra nově vytvořený události klikněte na kartu **Spotř skupiny** a pak klikněte na tlačítko **vytvořit** v dolní části stránky a zadejte název skupiny příjemce.
3.  Pokud chcete udělit přístup k centru události, bude potřeba vytvoření zásad sdílený přístup.  Klikněte na kartu **Konfigurovat** rozbočovače události.
4.  V části **Sdílené zásady přístupu**vytvoření nových zásad s **Spravovat** oprávnění.

    ![Sdílené zásady přístupu, které můžete vytvořit zásady s spravovat oprávnění.](./media/stream-analytics-get-started/stream-ananlytics-shared-access-policies.png)

5.  Klepněte na tlačítko **Uložit** v dolní části stránky.
6.  Přejděte do **řídicího panelu** a potom klikněte na **Informace o připojení** v dolní části stránky a zkopírujte uložit informace o připojení.

## <a name="configure-and-start-event-generator-application"></a>Konfiguraci a spuštění aplikace generátor události

Uvádíme klientská aplikace, která vygeneruje ukázkové příchozí volání metadat a použít pro centrální události. Postupujte podle návodu pro nastavení této aplikace.  

1.  Stáhněte si [soubor TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip). Potom ho unzip do adresáře.

    **Poznámka**: Windows blokuje zip stažený soubor. Klikněte pravým tlačítkem myši na soubor a vyberte vlastnosti. Pokud zpráva "Tento soubor pochází z jiného počítače a může být blokováno se můžete pomoci chrá tento počítač." Zaškrtněte políčko "Odblokovat" a klikněte na použít u souboru zip.

2.  Nahraďte hodnoty Microsoft.ServiceBus.ConnectionString a EventHubName v **telcodatagen.exe.config** události centrální připojovacího řetězce a název.

    **Poznámka**: zkopírovat připojovací řetězec z Azure portálu míst název připojení na konci. Ujistěte se, pokud chcete odebrat "; EntityPath =<value>"z klávesu přidat pole =.

3.  Spusťte aplikaci. Použití vypadá takto:

   telcodatagen.exe [#NumCDRsPerHour] [SIM karty podvod pravděpodobnost] [#DurationHours]

Následující příklad vytvoří události 1000 s pravděpodobností 20 procent podvodu v průběhu 2 hodin.

    telcodatagen.exe 1000 .2 2

Zobrazí se záznamy se odešlou rozbočovače události. Zde jsou některé klíčové pole, které budeme používat v této aplikaci zjišťování v reálném čase podvod definovány:

| Záznam | Definice |
| ------------- | ------------- |
| CallrecTime | Časové razítko pro čas zahájení hovoru. |
| SwitchNum | Telefonní přepínač připojuje hovor. |
| CallingNum | Telefonní číslo volajícího. |
| CallingIMSI | Mezinárodní mobilní odběratele Identity (společnosti IMSI).  Jedinečný identifikátor volající. |
| CalledNum | Telefonní číslo příjemce volání. |
| CalledIMSI | Mezinárodní mobilní odběratele Identity (společnosti IMSI).  Jedinečný identifikátor příjemce volání. |


## <a name="create-stream-analytics-job"></a>Vytvoření datového proudu analýzy úlohy
Teď máme toku telekomunikačních události, jsme nastavit úlohy toku technologie pro analýzu a analyzujte data těchto událostí v reálném čase.

### <a name="provision-a-stream-analytics-job"></a>Zřízení úlohy toku analýzy

1.  Na portálu Azure klikněte na **Nový > datové služby > toku analýzy > vytvořit**.
2.  Zadejte následující hodnoty a klikněte na **Vytvořit toku analýzy úlohy**:

    * **Název úlohy**: Zadejte název projektu.

    * **Oblast**: vyberte požadovanou oblast, ve které chcete spustit úlohu. Zvažte umístění úkoly a události centrální ve stejné oblasti zajistit lepší výkon a ujistěte se, že si nebudete platíte přenášet data mezi oblastmi.

    * **Úložiště účtu**: Zvolte účet Azure úložiště, který chcete používat k ukládání sledování data pro všechny úlohami toku analýzy v této oblasti. Máte možnost zvolit existujícího účtu úložiště, nebo vytvořte nový účet.

3.  Klikněte na **Analýza toku** v levém podokně seznamu úloh toku analýzy.

    ![Ikona služby technologie pro analýzu toku](./media/stream-analytics-get-started/stream-analytics-service-icon.png)

4.  Zobrazí se nová úloha se stavem **vytvořeno**. Všimněte si, že je zakázané v dolní části stránky na tlačítko **Start** . Než začnete úlohy musíte nakonfigurovat úlohu vstupní, výstup a dotazů.

### <a name="specify-job-input"></a>Určení úlohu vstupní
1.  V toku analýzy práce klikněte **vstupy** v horní části stránky a klikněte na **Přidat vstupní**. Dialogové okno, které se otevře vás provede procesem počet návodu pro nastavení vašeho zadání vstupních hodnot.
2.  Vyberte **toku dat**a klikněte na tlačítko vpravo.
3.  Vyberte **Událost centrální**a klikněte pravým tlačítkem.
4.  Zadejte nebo vyberte následující hodnoty na třetí stránce:

    * **Zadávání Alias**: Zadejte popisný název pro tuto úlohu vstupní například *CallStream*. Všimněte si, že použijete tento název v dotazu později.
    * **Centrální události**:-li centru události jste vytvořili ve stejném předplatném jako úlohu toku analýzy, vyberte obor názvů, který je v centru události.

    Pokud rozbočovače události se v jiné předplatné, vyberte **Použití centrální událost z jiné předplatné** a ruční zadání informací o ** **Namespace Bus služby**, **Centrální název události**, centrální zásad název události**, **Klíč zásad centrální události**a **Počet oddílů centrální události**.

    * **Název centrální události**: vyberte název centru události.

    * **Název zásady centrální události**: vyberte zásadu události centrální vytvořili dříve v tomto kurzu.

    * **Událost centrální spotř skupiny**: Zadejte skupině spotř vytvořili dříve v tomto kurzu.
5.  Klikněte pravým tlačítkem.
6.  Zadejte následující hodnoty:

    * **Formát Serializer události**: JSON
    * **Kódování**: UTF8
7.  Klikněte na tlačítko Kontrola přidat tento zdroj a pro ověření, že toku analýzy můžete úspěšně se připojit k centru události.

### <a name="specify-job-query"></a>Zadat dotaz projektu

Technologie pro analýzu toku podporuje model jednoduché, deklarativní dotazu pro popis transformace v reálném čase zpracování. Další informace o jazyce, najdete v článku [Azure toku technologie pro analýzu Query Language Reference](https://msdn.microsoft.com/library/dn834998.aspx). Tento kurz se můžete vytvářet a vyzkoušet několik dotazů přes v reálném čase toku dat volání.

#### <a name="optional-sample-input-data"></a>Volitelné: Ukázková vstupní data
Ověřte dotaz na data skutečné práce, můžete použít funkci **Ukázková Data** extrahovat události z vašeho proudu a vytvořit. Soubor JSON událostí pro účely testování.  Následující kroky ukazují, jak se to dělá a také uvádíme ukázkový soubor [telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json) pro účely testování.

1.  Vyberte událost centrální vstupní a klikněte na **Ukázková Data** v dolní části stránky.
2.  V dialogovém okně, které se zobrazí zadejte **Počáteční čas** spuštění shromažďování dat od a po **dobu trvání** kolik další data pro používání.
3.  Klikněte na tlačítko Kontrola od které začnete odběr data zadání.  Může to trvat jednu až dvě pro datový soubor pro vyrobit minuty.  Po dokončení procesu klikněte na **Podrobnosti** a stáhněte si a uložit. JSON soubor vytvořený.

    ![Stáhněte si a uložení dat zpracovaných v souboru JSON](./media/stream-analytics-get-started/stream-analytics-download-save-json-file.png)

#### <a name="passthrough-query"></a>Předávací dotaz

Pokud chcete archivovat všech událostí, můžete předávací dotaz zobrazíte všechna pole v datové událost nebo zprávy. Jednoduchý předávací dotaz začínat, proveďte tuto projekty všechna pole v případě.

1.  Klikněte na **dotaz** v horní části stránky úlohy toku analýzy.
2.  Přidejte následující text do editoru kódu:

        SELECT * FROM CallStream

    > Ujistěte se, že název vstupní zdroj shoduje s názvem vstupu, které jste zadali dříve.

3.  V části editoru dotazů klikněte na tlačítko **Testovat** .
4.  Zadat testovací soubor, který jste vytvořili v předchozích krocích jedné nebo použijte [telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json).
5.  Klikněte na tlačítko Zkontrolovat a zobrazte výsledky zobrazí pod definice dotazu.

    ![Definice výsledky dotazu](./media/stream-analytics-get-started/stream-analytics-sim-fraud-output.png)


### <a name="column-projection"></a>Promítání sloupce

Teď můžeme budete zmenšit vrácené pole, která chcete něco míň.

1.  Změna dotazu v editoru kódu pro:

        SELECT CallRecTime, SwitchNum, CallingIMSI, CallingNum, CalledNum
        FROM CallStream

2.  V části editoru dotazů a zobrazte výsledky dotazu klikněte na **znovu spustit** .

    ![Výstup v editoru dotazů.](./media/stream-analytics-get-started/stream-analytics-query-editor-output.png)

### <a name="count-of-incoming-calls-by-region-tumbling-window-with-aggregation"></a>Počet příchozí hovory podle regionů: Tumbling okno s agregace

A umožňuje porovnávat množství této příchozí hovory jednotlivých oblastech jsme budete využít [TumblingWindow](https://msdn.microsoft.com/library/azure/dn835055.aspx) získat počet příchozí hovory seskupené podle SwitchNum každých 5 sekund.

1.  Změna dotazu v editoru kódu pro:

        SELECT System.Timestamp as WindowEnd, SwitchNum, COUNT(*) as CallCount
        FROM CallStream TIMESTAMP BY CallRecTime
        GROUP BY TUMBLINGWINDOW(s, 5), SwitchNum

    Tento dotaz používá **Časové razítko podle** klíčových slov pro zadání časového razítka pole v datové se nemusí používat v časový výpočtu. Pokud toto pole nebyla zadána, operaci práce s okny by provést pomocí čas doručení každou událost v centrální události. V tématu ["Čas a aplikace čas doručení" v toku analýzy dotazu Reference jazyka](https://msdn.microsoft.com/library/azure/dn834998.aspx).

    Všimněte si, že se dostanete časového razítka na konci každé okno pomocí vlastnosti **System.Timestamp** .

2.  V části editoru dotazů a zobrazte výsledky dotazu klikněte na **znovu spustit** .

    ![Výsledky dotazu pro Timestand tak, že](./media/stream-analytics-get-started/stream-ananlytics-query-editor-rerun.png)

### <a name="sim-fraud-detection-with-a-self-join"></a>Zjišťování podvod SIM s vlastní spojení

Jak identifikovat potenciálně podvodné použití podíváme pro volání pocházející z jednoho uživatele, ale na různých místech za méně než 5 sekund.  Jsme [spojení](https://msdn.microsoft.com/library/azure/dn835026.aspx) toku události hovor s samotné kontrolovaní takovýchto případech.

1.  Změna dotazu v editoru kódu pro:

        SELECT System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1,
        CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
        JOIN CallStream CS2 TIMESTAMP BY CallRecTime
        ON CS1.CallingIMSI = CS2.CallingIMSI
        AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
        WHERE CS1.SwitchNum != CS2.SwitchNum

2.  V části editoru dotazů a zobrazte výsledky dotazu klikněte na **znovu spustit** .

    ![Výsledky dotazu spojení](./media/stream-analytics-get-started/stream-ananlytics-query-editor-join.png)

### <a name="create-output-sink"></a>Vytvoření jímky výstup

Teď, když nám definovali události toku, rozbočovači události vstup jedí událostí a dotazu k provedení transformace přes toku, posledním krokem je definovat jímky výstup pro daný úkol.  K úložišti objektů Blob jsme budete psát událostí pro podvodné chování.

Postupujte podle pokynů a vytvořte kontejneru k úložišti objektů Blob, pokud už nemáte.

1.  Použití existujícího účtu úložiště nebo vytvořte nový účet úložiště kliknutím **Nový > datové služby > úložiště > vytvořit** a postupujte podle pokynů.
2.  Vyberte účet, úložiště, **kontejnerů** v horní části stránky klikněte na a potom klikněte na **Přidat**.
3.  Zadejte **název** pro váš kontejner a nastavte jeho **přístup** na veřejné objektů Blob.

## <a name="specify-job-output"></a>Určení výstup projektu

1.  V toku analýzy práce klikněte **výstup** v horní části stránky a potom klikněte na **Přidat výstup**. Dialogové okno, které se otevře vás provede procesem počet návodu pro nastavení výstupu.
2.  Vyberte **Úložiště objektů BLOB**a potom klikněte na tlačítko vpravo.
3.  Zadejte nebo vyberte následující hodnoty na třetí stránce:

    * **Výstup ALIAS**: Zadejte popisný název pro tento výstup projektu.
    * **PŘEDPLATNÉ**: Pokud úložiště objektů Blob jste vytvořili ve stejném předplatném jako úlohu toku analýzy, vyberte **Použít úložiště účet z aktuálního předplatného**. Pokud je vaše úložiště v jiné předplatné, vyberte **Použít úložiště účet z jiné předplatné** a ruční zadání informací o **Účtu úložiště** **Klíč účtu úložiště** **CONTAINER**.
    * **Úložiště účtu**: vyberte název účtu úložiště.
    * **Kontejner**: vyberte název kontejner.
    * **PŘEDPONA FILENAME**: Zadejte předponu soubor používat při psaní objektů blob výstupu.

4.  Klikněte pravým tlačítkem.
5.  Zadejte následující hodnoty:

    * **Formát SERIALIZER události**: JSON
    * **Kódování**: UTF8

6.  Klikněte na tlačítko Zkontrolovat přidat tento zdroj a ověřte, že toku analýzy můžete úspěšně se připojit k účtu úložiště.

## <a name="start-job-for-real-time-processing"></a>Zahájení práce pro zpracování reálném čase

Protože úlohu vstupní, dotazu a výstup všech zadáno, jsme do ní začít úlohu toku Analytics pro zjišťování v reálném čase podvod.

1.  Z úlohy **řídicího PANELU**klikněte na tlačítko **START** v dolní části stránky.
2.  V dialogovém okně, které se zobrazí vyberte **čas zahájení projektu** a potom klikněte na tlačítko Zkontrolovat v dolní části dialogového okna. Stav úlohy se změní na **začátek** a krátce přesune do **spuštěný**.

## <a name="view-fraud-detection-output"></a>Zobrazení podvod zjišťování výstup

Pomocí nástroje jako [Úložiště Průzkumník Windows Azure](https://azurestorageexplorer.codeplex.com/) nebo [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) zobrazíte podvodné události, jako jsou napsali do výstupu v reálném čase.  

![Zjišťování podvod: podvodné události zobrazit v reálném čase](./media/stream-analytics-get-started/stream-ananlytics-view-real-time-fraudent-events.png)

## <a name="get-support"></a>Získání podpory
Další pomoc Vyzkoušejte naše [Fórum komunity Azure toku analýzy](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Další kroky

- [Úvod do technologie pro analýzu Azure toku](stream-analytics-introduction.md)
- [Začínáme s používáním analýzy toku Azure](stream-analytics-get-started.md)
- [Změna velikosti úlohy Azure toku analýzy](stream-analytics-scale-jobs.md)
- [Odkaz na Azure toku analýzy dotaz jazyka](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure toku analýzy správy REST API odkaz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
