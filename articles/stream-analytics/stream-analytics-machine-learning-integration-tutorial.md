<properties
    pageTitle="Myšlenkou analýzy pomocí Azure toku technologie pro analýzu a Azure počítače výukové | Microsoft Azure"
    description="Použití uživatelem definovaných funkcí a počítač výuka v toku analýzy projektu"
    keywords=""
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>


<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="10/04/2016" 
    ms.author="jeffstok"
/>

# <a name="sentiment-analysis-by-using-azure-stream-analytics-and-azure-machine-learning"></a>Analýza myšlenkou pomocí Azure toku technologie pro analýzu a výukové počítače Azure #

Tento článek je určená vám pomůžou rychle nastavit jednoduché úlohy Azure toku analýzy, integrací výukové počítače Azure. Jsme pomocí modelu myšlenkou analýzy výukové počítače z Galerie Intelligence Cortana a analyzujte data streaming textových dat a určit skóre myšlenkou v reálném čase. Informace v tomto článku se dozvíte pochopit scénáře například v reálném čase myšlenkou analýzy na Twitter dat datových proudů, analyzovat záznamy o zákazníkovi konverzace s pracovníky podpory a zjistit komentáře na fóra, blogy a videa, kromě mnoho dalších v reálném čase, prediktivní bodování scénáře.

Tento článek poskytuje ukázkový soubor CSV s textem předávat na vstupu v úložišti objektů Blob Azure vidět na následujícím obrázku. Úlohy bude platit modelu analýzy myšlenkou funkce definované uživatelem (UDF) vzorová data text z úložiště objektů blob. V konečném důsledku je umístěn v úložišti objektů blob v jiném souboru CSV. 

![Technologie pro analýzu toku počítače výuka](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-2.png)  

Následující obrázek ukazuje tuto konfiguraci. Pro více reálné situace můžete nahradíte úložiště objektů Blob streamování Twitter dat z Azure události rozbočovače vstup. Je také vytvářet vizualizace [Microsoft Power BI](https://powerbi.microsoft.com/) v reálném čase z agregační myšlenkou.    

![Technologie pro analýzu toku počítače výukové](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-1.png)  

## <a name="prerequisites"></a>Zjistit předpoklady pro

Požadavky pro dokončení úkolů, které je znázorněn v tomto článku jsou následujícím způsobem:

-   Aktivní Azure předplatné.
-   Soubor CSV s daty v ní. Můžete si stáhnout soubor vidíte na obrázku 1 z [GitHub](https://github.com/Azure/azure-stream-analytics/blob/master/Sample Data/sampleinput.csv)nebo můžete vytvořit vlastní soubor. V tomto článku předpokladu, že použijte dodaná ke stažení na GitHub.

Na vysoké úrovni k provedení těchto úkolů znázorněn v tomto článku můžete udělat následující:

1.  Nahrajte soubor CSV vstupní k úložišti objektů Blob Azure.
2.  Přidání modelu analýzy myšlenkou z Galerie Cortana Intelligence do pracovního prostoru Azure počítače výuka.
3.  Nasazení tento model jako webové služby v pracovním prostoru výukové počítače.
4.  Vytvoření úlohy toku analýzy, která volá tato webová služba jako funkce, a zjistit myšlenkou pro zadávání textu.
5.  Spuštění úlohy toku technologie pro analýzu a sledujte výstupu.

## <a name="upload-the-csv-input-file-to-blob-storage"></a>Nahrání souboru CSV vstupní k úložišti objektů Blob

V tomto kroku můžete použít libovolný soubor CSV, například je už určený jako k dispozici ke stažení na GitHub. [Průzkumník Windows Azure úložiště](http://storageexplorer.com/) nebo Visual Studio můžete použít k nahrání souboru nebo můžete použít vlastní kód. Příklady podle Visual Studio používáme.

1.  Ve Visual Studiu, klikněte na **Azure** > **úložiště** > **Připojit externí úložiště**. Zadejte **název účtu** a **klíč účtu**.  

    ![Technologie pro analýzu toku počítače výuka, Průzkumník serveru](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-server-explorer.png)  

2.  Rozšíření úložiště, která jste připojili v kroku 1, klikněte na **Vytvořit kontejner objektů Blob**a zadejte název logické. Po vytvoření kontejneru jej otevřete a zobrazit její obsah. (To bude prázdná v tomto okamžiku).  

    ![Můžete vysílat datovými proudy analýzy počítače výuka, vytvořte objektů blob](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-create-blob.png)  

3.  Uložit soubor CSV, klikněte na **Nahrát objektů Blob**a potom klikněte na **soubor z místního disku**.  

## <a name="add-the-sentiment-analytics-model-from-the-cortana-intelligence-gallery"></a>Přidání modelu analýzy myšlenkou z Galerie Cortana měřítka

1.  Stažení [prediktivní myšlenkou analýzy modelu](https://gallery.cortanaintelligence.com/Experiment/Predictive-Mini-Twitter-sentiment-analysis-Experiment-1) z Galerie Intelligence Cortana.  
2.  Ve počítače výukové studiu klikněte na **Otevřít v Studio**.  

    ![Technologie pro analýzu počítače výukové otevřít Studio výukové počítače můžete vysílat datovými proudy](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-open-ml-studio.png)  

3.  Přihlaste se k přejděte do pracovního prostoru. Vyberte umístění, které nejlépe odpovídá vlastní umístění.
4.  Klikněte na **Spustit** v dolní části stránky.  
5.  Po dokončení procesu úspěšně, klikněte na **Nasazení webové služby**.
6.  Model analýzy myšlenkou je připraven k použití. Ověřte, klikněte na tlačítko **Testovat** a zadávat text, třeba "Mám používat Microsoft". Test, měly vrátit výsledek podobný takto:

`'Predictive Mini Twitter sentiment analysis Experiment' test returned ["4","0.715057671070099"]...`  

![Toku analýzy počítače výuka, analýza dat](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-analysis-data.png)  

Ve sloupci **aplikace** klikněte na odkaz pro **Excel 2010 nebo starší sešitu** získat kód rozhraní API a adresu URL, kterou budete muset později nastavit úlohu toku analýzy. (Tento krok je nutné jenom pomocí modelu výukové počítače z jiného pracovního prostoru účet Azure. Tento článek se předpokládá, že toto je tento případ, který bude řešit tento scénář.)  

Všimněte si webové adresy URL a přístup klíč služby ze staženého souboru aplikace Excel, jak je ukázáno v následujícím příkladu:  

![Toku analýzy počítače výukové rychlý přehled](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-quick-glance.png)  

## <a name="create-a-stream-analytics-job-that-uses-the-machine-learning-model"></a>Vytvoření úlohy toku analýzy, která používá modelu počítače výuka

1.  Přejděte na [portál Azure](https://manage.windowsazure.com).  
2.  Klikněte na **Nový** > **datové služby** > **toku analýzy** > **vytvořit**. Zadejte název pro danou úlohu do pole **Název projektu**, zadejte příslušnou oblast pro práci v **oblasti**a **místní sledování úložiště**účtu vyberte účet.    
3.  Po vytvoření projektu na kartě **vstupů** klikněte na **Přidat zadání vstupních hodnot**.  

    ![Datového proudu výukové počítače analýzy, přidat vstupní výukové počítače](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-add-input-screen.png)  

4.  Na první stránce průvodce **Vstupní přidat** klikněte na **toku dat**a klikněte na tlačítko **Další**. Na další stránce klikněte na **Úložiště objektů Blob** jako vstup a klikněte na tlačítko **Další**.  
5.  Na stránce **Nastavení úložiště objektů Blob** Průvodce poskytují na úložiště objektů blob kontejneru název účtu, který jste definovali dříve, když jste nahráli data. Klikněte na tlačítko **Další**. **Formát serializace události**klikněte na **CSV**. Nechejte výchozí hodnoty pro ostatní stránky **Nastavení serializace** . Klikněte na **OK**.  
6.  Na kartě **výstupy** klikněte na **Přidat výstup**.  

    ![Můžete vysílat datovými proudy analýzy počítače výuka, přidejte výstupní](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-add-output-screen.png)  

7.  Klikněte na **Úložiště objektů Blob**a pak zadejte stejné parametry kromě kontejner. Hodnota k **zadání vstupních hodnot** nakonfigurovanou číst ze kontejneru s názvem "test", kdy byl odeslán soubor **CSV** . Jako **výstup**zadejte "testoutput". Kontejner názvy musí být různé. Ověřte tento kontejner.     
8.  Klepnutím na tlačítko **Další** konfigurace výstupu **Nastavení serializace**. Stejně jako u **vstupní**, klikněte na **CSV**a potom klikněte na tlačítko **OK** .
9.  Na kartě **funkce** klikněte na **Přidat funkce učení počítače**.  

    ![Výukové počítače analýzy můžete vysílat datovými proudy, přidejte výukové počítače (funkce)](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-add-ml-function.png)  

10. Na stránce **Nastavení počítače výukové webové služby** vyhledejte pracovního prostoru výukové počítače, webové služby a výchozí koncový bod. V tomto článku použijte nastavení ručně k získání znalost konfigurace webové služby pro libovolného pracovního prostoru dlouhou, jak najdu adresu URL a rozhraní API klíče. Zadejte koncový bod **adresy URL** a **rozhraní API klíče**. Klikněte na **OK**.    

    ![Toku analýzy počítače výukové počítače výukové webové služby](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-ml-web-service.png)    

11. Na kartě **Query** upravte dotaz takto:    

 ```
    WITH subquery AS (  
        SELECT text, sentiment(text) as result from input  
    )  
 
    Select text, result.[Scored Labels]  
    Into output  
    From subquery  
 ```    
12. Klepnutím na tlačítko **Uložit** Uložte dotaz.

## <a name="start-the-stream-analytics-job-and-observe-the-output"></a>Spuštění úlohy toku technologie pro analýzu a sledujte výstup

1.  Klikněte na tlačítko **Start** v dolní části na stránku projektu.
2.  V okně **Spustit dialogové okno dotazu**klikněte na **Vlastní čas**a vyberte čas před, kdy jste nahráli CSV k úložišti objektů Blob. Klikněte na **OK**.  

    ![Toku analýzy počítače výukové, vlastní čas](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-custom-time.png)  

3.  Přejděte k základnímu úložišti objektů Blob pomocí nástroje, které jste použili k nahrání soubor CSV, například Visual Studio.
4.  Několik minut po spuštění projektu kontejneru výstup se vytvoří a je nahrát soubor CSV.  
5.  Otevřete soubor v editoru výchozí CSV. Mají být zobrazeny podobnou následující:  

    ![Toku analýzy počítače výukové CSV zobrazení](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-csv-view.png)  

## <a name="conclusion"></a>Uzavření

Tento článek ukazuje, jak vytvořit úlohu toku analýzy přečte streamování textová data, která platí myšlenkou analytics pro data v reálném čase. Je možné provádět to všechno bez nutnosti starat o to rozbor všech sestavování modelu myšlenkou analýzy. Toto je jednou z výhod sady Intelligence Cortana.

Taky můžete zobrazit metriky související funkce učení počítače Azure. K tomuto účelu klikněte na kartě **Monitor** . Zobrazí se tři metriky související funkce.  

- **Funkce požadavky** označuje počet žádosti o odeslané do počítače výukové webové služby.  
- **Funkce události** označuje počet událostí v pozvánce. Ve výchozím nastavení obsahuje každý požadavek webové služby strojového výukové až 1 000 události.  
- **Zamítnuté žádosti funkce** označuje počet neúspěšných požadavků webové služby strojového výukové.  

    ![Toku analýzy počítače výukové zobrazení sledování výukové počítače](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-ml-monitor-view.png)  
