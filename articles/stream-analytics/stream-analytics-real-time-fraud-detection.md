<properties
    pageTitle="Analýzy toku: Zjišťování v reálném čase podvod | Microsoft Azure"
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

Naučte se vytvářet začátku do konce řešení pro vyhledávání v reálném čase podvod s Azure toku analýzy. Přenést událostí do rozbočovače události Azure, napsat toku analýzy dotazů pro agregaci nebo výstrahy a odešlete výsledky jímky výstup získat další informace na data v reálném čase zpracování. Vysvětlení v reálném čase odchylky rozpoznávání telekomunikačních, ale postup příkladu je rovnoměrně z vnější vhodný pro jiné typy podvod zjišťování například platební kartou nebo identity scénáře krádežím identity.

Technologie pro analýzu toku je plně spravovaného služba, která poskytuje zpracování nízkou latencí vysoce dostupné, scalable, složité události nad přenos dat v cloudu. Další informace najdete v tématu [Úvod do technologie pro analýzu toku Azure](stream-analytics-introduction.md).


## <a name="scenario-telecommunications-and-sim-fraud-detection-in-real-time"></a>Scénář: Telekomunikačních SIM podvod zjišťování a v reálném čase

Telekomunikačních společnosti obsahuje velké množství dat pro příchozí hovory. Společnost musí od data:

* Snížit množství dat na samostatně zvládnutelné množství a získat další informace o použití zákazníka v čase a přes geografických oblastech.
* Zjistit SIM podvod (více hovorů z stejné identity ve stejnou dobu, ale na geograficky různých místech) v reálném čase tak, aby společnost můžete snadno odpovědět oznamování zákazníci nebo vypnutí služby.

Scénáře kanonický Internet věcí (IoT) spousta telemetrie nebo data z senzory. Zákazníci chcete agregovat data nebo jim zobrazovat upozornění na odchylky v reálném čase.

## <a name="prerequisites"></a>Zjistit předpoklady pro

- Stáhněte si [TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip) z webu služby Stažení softwaru
- Volitelné: Zdrojového kódu generátor událost z [GitHub](https://aka.ms/azure-stream-analytics-telcogenerator)

## <a name="create-azure-event-hubs-input-and-consumer-group"></a>Vytvoření skupiny pro zadávání a příjemce rozbočovače události Azure

Ukázková aplikace generování událostí a použít pro instance rozbočovače událostí pro zpracování v reálném čase. Služba Bus události rozbočovače jsou upřednostňovaný způsob požití událostí pro analýzy proudu. Další informace o události rozbočovače v [si přečtěte následující dokumentaci Bus služby Azure](/documentation/services/service-bus/).

Vytvoření rozbočovači událostí:

1.  V [Azure portál](https://manage.windowsazure.com/), klikněte na **Nový** > **Aplikace služby** > **Služby BUS** > **Události centrální** > **Vytvořit**. Zadejte název, oblast a nové nebo existující názvů k vytvoření nové události centrální.  
2.  Osvědčený přečtěte si každý toku analýzy projekt ze skupiny spotř centrální jedné události. Nemůžeme vás provede procesem vytvořit skupinu spotř později. [Další informace o skupinách příjemce](https://msdn.microsoft.com/library/azure/dn836025.aspx). Pokud chcete vytvořit skupinu příjemce, přejděte k centru nově vytvořený události klikněte na kartu **Spotř skupin** , klikněte na tlačítko **vytvořit** v dolní části stránky a potom zadejte název skupiny příjemce.
3.  Pokud chcete udělit přístup k centru událost, bude potřebujeme vytvoření zásad sdílený přístup. Klikněte na kartu **KONFIGUROVAT** rozbočovače události.
4.  V části **SDÍLENÉ zásady přístupu**vytvořte novou zásadu, který má oprávnění **SPRAVOVAT** .

    ![Sdílené zásady přístupu, které můžete vytvořit zásady s spravovat oprávnění.](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-shared-access-policies.png)

5.  Klepněte na tlačítko **Uložit** v dolní části stránky.
6.  Přejděte na **řídicí panel**, klikněte na **Informace o připojení** v dolní části stránky a zkopírujte uložit informace o připojení.

## <a name="configure-and-start-the-event-generator-application"></a>Konfiguraci a spuštění aplikace generátor události

Uvádíme klientská aplikace, která vygeneruje ukázkové příchozí volání metadat a použít pro rozbočovače události. Pomocí následujících kroků nastavení této aplikace.  

1.  Stáhněte si [soubor TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip)a rozbalte do adresáře.

    > [AZURE.NOTE] Windows blokuje zip stažený soubor. Klikněte pravým tlačítkem myši na soubor a vyberte **Vlastnosti**. Pokud se zobrazí zpráva "Tento soubor pochází z jiného počítače a mohou být blokovány se můžete pomoci chrá na tomto počítači", zaškrtněte políčko **Odblokovat** a potom klikněte na použijte u souboru zip.

2.  Nahraďte hodnoty Microsoft.ServiceBus.ConnectionString a EventHubName v telcodatagen.exe.config události centrální připojovacího řetězce a název.

    Připojovací řetězec, který jste zkopírovali z portálu Microsoft Azure umístí název připojení na konci. Ujistěte se, pokud chcete odebrat "; EntityPath =<value>"z" Přidání klíče = "pole.

3.  Spusťte aplikaci. Použití vypadá takto:

    telcodatagen.exe [#NumCDRsPerHour] [SIM karty podvod pravděpodobnost] [#DurationHours]

Následující příklad vytvoří 1 000 události s pravděpodobností 20 procent podvodu v průběhu dvou hodin.

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


## <a name="create-a-stream-analytics-job"></a>Vytvoření úlohy toku analýzy
Teď máme toku telekomunikačních události, jsme nastavit úlohy toku technologie pro analýzu a analyzujte data v reálném čase tyto události.

### <a name="provision-a-stream-analytics-job"></a>Zřízení úlohy toku analýzy

1.  Na portálu Azure klikněte na **Nový** > **Datové služby** > **Toku analýzy** > **Vytvořit**.
2.  Zadejte následující hodnoty a klikněte na **Vytvořit toku analýzy úlohy**:

    * **Název úlohy**: Zadejte název projektu.

    * **Oblast**: vyberte požadovanou oblast, ve které chcete spustit úlohu. Zvažte umístění úkoly a události centrální ve stejné oblasti zajistit lepší výkon a ujistěte se, že si nebudete platíte přenášet data mezi oblastmi.

    * **Úložiště účtu**: Zvolte účet Azure úložiště, který chcete použít pro uložení monitorování dat pro všechny úlohy toku analýz spuštěných v této oblasti. Máte možnost zvolit existujícího účtu úložiště nebo vytvořte nový účet.

3.  Klikněte na **Analýza toku** v levém podokně seznamu úloh toku analýzy.

    ![Ikona služby technologie pro analýzu toku](./media/stream-analytics-real-time-fraud-detection/stream-analytics-service-icon.png)

    Zobrazí se nová úloha se stavem **VYTVOŘENO**. Všimněte si, že je zakázané v dolní části stránky na tlačítko **START** . Než začnete úlohy musíte nakonfigurovat úlohu vstupní, výstup a dotazů.

### <a name="specify-job-input"></a>Určení úlohu vstupní
1.  V toku analýzy práce klikněte na **VSTUPY** v horní části stránky a potom klikněte na **Přidat vstupní**. Dialogové okno, které se otevře vás provede jednotlivými několik pokynů k pilotnímu zadané hodnoty.
2.  Klikněte na **toku dat**a potom klikněte na tlačítko vpravo.
3.  Klikněte na **Centrum událostí**a klikněte pravým tlačítkem.
4.  Zadejte nebo vyberte následující hodnoty na třetí stránce:

    * **Vstupní ALIAS**: Zadejte popisný název, například *CallStream*pro tento projekt. Všimněte si, že použijete tento název v dotazu později.

    * **Centrální události**:-li centrální událost, kterou jste vytvořili ve stejném předplatném jako úlohu toku analýzy, vyberte obor názvů, který je v centru události.

        Pokud vaše Centrum událostí v jiné předplatné, vyberte **Použití centrální událost z jiné předplatné**a ručně zadat informace o ** **Obor názvů BUS služby**, **Centrální název události**, centrální zásad název události**, **Klíč zásad centrální události**a **Počet oddílů centrální události**.

    * **Název centrální události**: vyberte název centru události.

    * **Název zásady centrální události**: vyberte zásadu centrální události, který jste vytvořili dříve v tomto kurzu.

    * **Událost centrální spotř skupiny**: Zadejte název skupiny příjemce, které jste vytvořili dříve v tomto kurzu.

5.  Klikněte pravým tlačítkem.
6.  Zadejte následující hodnoty:

    * **Formát SERIALIZER události**: JSON
    * **Kódování**: UTF8
7.  Kliknutím na tlačítko **Zkontrolovat** přidat tento zdroj a pro ověření, že toku analýzy můžete úspěšně se připojit k centru události.

### <a name="specify-job-query"></a>Zadat dotaz projektu

Technologie pro analýzu toku podporuje modelu jednoduché, deklarativní dotazu, který popisuje transformace v reálném čase zpracování. Další informace o jazyce, najdete v článku [Azure toku technologie pro analýzu Query Language Reference](https://msdn.microsoft.com/library/dn834998.aspx). Tento kurz se můžete vytvářet a vyzkoušet několik dotazů přes v reálném čase toku dat volání.

#### <a name="optional-sample-input-data"></a>Volitelné: Ukázková vstupní data
Ověřte dotaz na data skutečné práce, můžete použít funkci **UKÁZKOVÁ DATA** extrahovat události z vašeho proudu a vytvořit. Soubor JSON událostí pro účely testování.  Podle těchto kroků ukazují, jak to udělat. Ukázkový soubor [telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json) pro účely testování také k dispozici.

1.  Vyberte zadání centrální událostí a klikněte na **UKÁZKOVÁ DATA** v dolní části stránky.
2.  V dialogovém okně, které se otevře zadejte **Počáteční čas** spuštění shromažďování dat a po **dobu trvání** kolik další data pro používání.
3.  Kliknutím na tlačítko **Zkontrolovat** od které začnete odběr data zadání.  Může to trvat jednu až dvě pro datový soubor pro vyrobit minuty.  Po dokončení procesu klikněte na **Podrobnosti**, stáhněte si generované. JSON souboru a uložte jej.

    ![Stáhněte si a uložení dat zpracovaných v souboru JSON](./media/stream-analytics-real-time-fraud-detection/stream-analytics-download-save-json-file.png)

#### <a name="pass-through-query"></a>Předávací dotaz

Pokud chcete archivovat všech událostí, můžete předávací dotaz zobrazíte všechna pole v datové událost nebo zprávy. Zahájíte uděláte jednoduché předávací dotaz této projekty všechna pole v události.

1.  Klikněte na **dotaz** v horní části stránky úlohy toku analýzy.
2.  Přidejte následující text do editoru kódu:

        SELECT * FROM CallStream

    > [AZURE.IMPORTANT] Ujistěte se, že název vstupní zdroj shoduje s názvem vstupu, který jste zadali dříve.

3.  V části editoru dotazů klikněte na tlačítko **Testovat** .
4.  Zadejte testovací soubor. Takový, který jste vytvořili pomocí předchozích kroků nedá použít nebo použijte [telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/SampleDataFiles/Telco.json).
5.  Klikněte na tlačítko **Zkontrolovat** a zobrazte výsledky zobrazí pod definice dotazu.

    ![Definice výsledky dotazu](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sim-fraud-output.png)


### <a name="column-projection"></a>Promítání sloupce

Teď můžeme budete snížit vrácené pole na něco míň.

1.  Změna dotazu v editoru kódu pro:

        SELECT CallRecTime, SwitchNum, CallingIMSI, CallingNum, CalledNum
        FROM CallStream

2.  V části editoru dotazů a zobrazte výsledky dotazu klikněte na **znovu spustit** .

    ![Výstup v editoru dotazů.](./media/stream-analytics-real-time-fraud-detection/stream-analytics-query-editor-output.png)

### <a name="count-of-incoming-calls-by-region-tumbling-window-with-aggregation"></a>Počet příchozí hovory podle regionů: Tumbling okno s agregace

Pokud chcete porovnat počet příchozí volání na oblast, použijeme [TumblingWindow](https://msdn.microsoft.com/library/azure/dn835055.aspx) získat počet příchozí hovory seskupené podle **SwitchNum** každých pět sekund.

1.  Změna dotazu v editoru kódu pro:

        SELECT System.Timestamp as WindowEnd, SwitchNum, COUNT(*) as CallCount
        FROM CallStream TIMESTAMP BY CallRecTime
        GROUP BY TUMBLINGWINDOW(s, 5), SwitchNum

    Tento dotaz používá **Časové razítko podle** klíčových slov pro zadání časového razítka pole v datové se nemusí používat v časový výpočtu. Pokud toto pole nebyla zadána, by pomocí údaj o čase, každou událost jste se dostali na centra události provedena operace práce s okny. V tématu ["Čas a aplikace čas doručení" v toku analýzy dotazu Reference jazyka](https://msdn.microsoft.com/library/azure/dn834998.aspx).

    Všimněte si, že se dostanete časového razítka na konci každé okno pomocí vlastnosti **System.Timestamp** .

2.  V části editoru dotazů a zobrazte výsledky dotazu klikněte na **znovu spustit** .

    ![Výsledky dotazu pro Timestand tak, že](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-query-editor-rerun.png)

### <a name="sim-fraud-detection-with-a-self-join"></a>Zjišťování podvod SIM s vlastní spojení

Jak identifikovat potenciálně podvodné použití podíváme pro hovory, které pocházejí z jednoho uživatele, ale na různých místech za méně než 5 sekund.  Jsme [spojení](https://msdn.microsoft.com/library/azure/dn835026.aspx) toku události hovor s samotné kontrolovaní takovýchto případech.

1.  Změna dotazu v editoru kódu pro:

        SELECT System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1,
        CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
        JOIN CallStream CS2 TIMESTAMP BY CallRecTime
        ON CS1.CallingIMSI = CS2.CallingIMSI
        AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
        WHERE CS1.SwitchNum != CS2.SwitchNum

2.  V části editoru dotazů a zobrazte výsledky dotazu klikněte na **znovu spustit** .

    ![Výsledky dotazu spojení](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-query-editor-join.png)

### <a name="create-output-sink"></a>Vytvoření jímky výstup

Teď, když nám definovali události toku, rozbočovači události vstup jedí událostí a dotaz: Pokud chcete provádět transformace toku, posledním krokem je definovat jímky výstup pro daný úkol. Události podvodné chování jsme budete psát k úložišti objektů Blob Azure.

Umožňuje vytvořit kontejner pro úložiště objektů Blob, pokud ještě nemáte jeden podle těchto kroků.

1.  Použití existujícího účtu úložiště nebo vytvořte nový účet úložiště kliknutím **Nový > datové služby > úložiště > vytvořit**a postupujte podle pokynů.
2.  Vyberte účet, úložiště, **kontejnerů** v horní části stránky klikněte na a potom klikněte na **Přidat**.
3.  Zadejte **název** pro váš kontejner a nastavte jeho **přístup** na **Veřejné objektů Blob**.

## <a name="specify-job-output"></a>Určení výstup projektu

1.  V toku analýzy práce klikněte na **výstup** v horní části stránky a potom klikněte na **Přidat výstup**. Dialogové okno, které se otevře vás provede jednotlivými několik pokynů k pilotnímu výstup.
2.  Klikněte na **Úložiště objektů BLOB**a potom klikněte na tlačítko vpravo.
3.  Zadejte nebo vyberte následující hodnoty na třetí stránce:

    * **Výstup ALIAS**: Zadejte popisný název pro tento výstup projektu.
    * **PŘEDPLATNÉ**: Pokud úložiště objektů Blob, který jste vytvořili ve stejném předplatném jako úlohu toku analýzy, klepněte na **Použít úložiště účet z aktuálního předplatného**. Pokud je vaše úložiště v jiné předplatné, klikněte na **Použít úložiště účet z jiné předplatné**a ruční zadání informací o **Účtu úložiště**, **Klíč účtu úložiště**a **kontejner**.
    * **Úložiště účtu**: vyberte název účtu úložiště.
    * **Kontejner**: vyberte název kontejner.
    * **PŘEDPONA název_souboru**: Zadejte soubor předponu má být použita při psaní objektů blob výstupu.

4.  Klikněte pravým tlačítkem.
5.  Zadejte následující hodnoty:

    * **Formát SERIALIZER události**: JSON
    * **Kódování**: UTF8

6.  Kliknutím na tlačítko **Zkontrolovat** přidat tento zdroj a ověřte, že toku analýzy můžete úspěšně se připojit k účtu úložiště.

## <a name="start-job-for-real-time-processing"></a>Zahájení práce v reálném čase zpracování

Vzhledem k tomu úlohu vstupní, dotazu a výstup všech zadáno, jste připraveni začít úlohu toku Analytics pro zjišťování v reálném čase podvod.

1.  Z úlohy **řídicího PANELU**klikněte na tlačítko **START** v dolní části stránky.
2.  V dialogovém okně, které se otevře klikněte na **čas zahájení projektu**a potom klikněte na tlačítko **Zkontrolovat** v dolní části dialogového okna. Stav úlohy se změní na **začátek** a krátce změní na **systém**.

## <a name="view-fraud-detection-output"></a>Zobrazení podvod zjišťování výstup

Pomocí nástroje jako [Úložiště Průzkumník Windows Azure](http://storageexplorer.com/) nebo [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) zobrazíte podvodné události, jako jsou napsali do výstupu v reálném čase.  

![Zjišťování podvod: podvodné události zobrazit v reálném čase](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-view-real-time-fraudent-events.png)

## <a name="get-support"></a>Získání podpory
Další pomoc Vyzkoušejte naše [Fórum komunity Azure toku analýzy](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Další kroky

- [Úvod do technologie pro analýzu Azure toku](stream-analytics-introduction.md)
- [Změna velikosti úlohy Azure toku analýzy](stream-analytics-scale-jobs.md)
- [Odkaz na Azure toku analýzy dotaz jazyka](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure toku analýzy správy REST API odkaz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
