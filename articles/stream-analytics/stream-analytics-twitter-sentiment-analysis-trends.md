<properties
    pageTitle="Data v reálném Twitter myšlenkou analýzy pomocí analýzy toku | Microsoft Azure"
    description="Zjistěte, jak pro účely analýzy myšlenkou v reálném čase Twitter toku analýzy. Podrobné pokyny z události generování s daty v živé řídicího panelu."
    keywords="analýza trendů v reálném čase twitter myšlenkou analýzy, sociálních médií, analýzy příklad analýzy trendu"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>


# <a name="social-media-analysis-real-time-twitter-sentiment-analysis-in-azure-stream-analytics"></a>Analýza sociálních médií: Analýza myšlenkou Twitter v reálném čase v Azure toku analýzy

Naučte se vytvořit řešení analýzy myšlenkou pro analýzy sociálních médií tak, že v reálném čase Twitter událostí do rozbočovače události Azure. Budete psát dotazu toku analýzy Azure k analýze dat. Pak budete buď uložení výsledků pro pozdější perusal nebo poskytnout přehledy v reálném čase pomocí řídicích panelů a [Power BI](https://powerbi.com/) .

Sociální média analytických nástrojů organizacím pochopit trendu témata, to znamená předměty a postoje, které mají velkého množství příspěvků v sociální média. Analýza myšlenkou, též nazývanou *názoru dolování*, sociální média analytických nástrojů používá k určení postoje směrem k produktu, myšlence a tak dál. Analýza trendů v reálném čase Twitter je skvělé příklad, protože model předplatné značka umožňuje poslouchat konkrétní klíčová slova a vývoj myšlenkou analýzy na informační kanál.

## <a name="scenario-sentiment-analysis-in-real-time"></a>Scénář: Myšlenkou analýzu můžete v reálném čase

Společnost, která se má web multimediální zprávy je zájem začíná výhodu nad jeho konkurence tak, že nabízí obsahu webu, který okamžitě vztahující se k jeho čtenáři. Společnost používá sociálních médií analýzy na témata, která odpovídají čteček provedením v reálném čase myšlenkou analýzy dat Twitter. Konkrétně k identifikaci trendu témata v reálném čase na Twitter, musí společnosti v reálném čase analýzy o objem tweetu a myšlenkou klíčové témata. Ano v podstatě nutnosti je myšlenkou analýzy analýzy modul založený na této sociálních médií kanálu.

## <a name="prerequisites"></a>Zjistit předpoklady pro
-   Twitter účet a [přístupový token OAuth](https://dev.twitter.com/oauth/overview/application-owner-access-tokens)
-   [TwitterClient.zip](http://download.microsoft.com/download/1/7/4/1744EE47-63D0-4B9D-9ECF-E379D15F4586/TwitterClient.zip) ze služby Stažení softwaru
-   Volitelné: Zdrojového kódu pro klienta twitter z [GitHub](https://aka.ms/azure-stream-analytics-twitterclient)

## <a name="create-an-event-hub-input-and-a-consumer-group"></a>Vytvoření rozbočovači události při zadávání a spotř skupiny

Ukázková aplikace generování událostí a nabízená k instanci rozbočovače události (události rozbočovači, pro krátký). Služba Bus události rozbočovače jsou upřednostňovaný způsob požití událostí pro analýzy proudu. Dokumentaci rozbočovače událostí v [si přečtěte následující dokumentaci Bus služby](/documentation/services/service-bus/).

Pomocí následujících kroků k vytvoření rozbočovači události.

1.  Na portálu Azure klikněte na **Nový** > **Aplikace služby** > **Služby BUS** > **Události centrální** > **Vytvořit**a zadejte název, oblast a nové nebo existující názvů.  
2.  Osvědčený přečtěte si jednotlivé úlohy toku analýzy z jedné skupině spotř rozbočovače události. Nemůžeme vás provede procesem vytvořit skupinu spotř později. Další informace o spotř skupin v [Azure události rozbočovače přehled](https://msdn.microsoft.com/library/azure/dn836025.aspx). Pokud chcete vytvořit skupinu příjemce, přejděte k centru nově vytvořený události klikněte na kartu **Spotř skupin** , klikněte na tlačítko **vytvořit** v dolní části stránky a potom zadejte název skupiny příjemce.
3.  Pokud chcete udělit přístup k centru událost, bude potřebujeme vytvoření zásad sdílený přístup. Klikněte na kartu **KONFIGUROVAT** rozbočovače události.
4.  V části **SDÍLENÉ zásady přístupu**vytvoření nových zásad s **SPRAVOVAT** oprávnění.

    ![Sdílené zásady přístupu, které můžete vytvořit zásady s spravovat oprávnění.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-ananlytics-shared-access-policies.png)

5.  Klepněte na tlačítko **Uložit** v dolní části stránky.
6.  Přejděte na **řídicí panel**, klikněte na **Informace o připojení** v dolní části stránky a zkopírujte uložit informace o připojení. (Použijte ikonu kopírovat, která se zobrazí v části na ikonu Hledat.)

## <a name="configure-and-start-the-twitter-client-application"></a>Konfiguraci a spuštění Twitter klientské aplikace

Uvádíme klientská aplikace, která se připojí k datům Twitter prostřednictvím [Rozhraní API streamování na Twitter](https://dev.twitter.com/streaming/overview) shromažďovat Tweetu události o parametry sadu témat. Nástroj otevřít zdroj [Sentiment140](http://help.sentiment140.com/) slouží k přiřaďte každé tweetu myšlenkou hodnotu.

- 0 = záporné
- 2 = neutrální
- 4 = kladné

Potom se posunou Tweetu události centru události.  

Tyto kroky nastavení aplikace:

1.  [Stáhněte si TwitterClient řešení](http://download.microsoft.com/download/1/7/4/1744EE47-63D0-4B9D-9ECF-E379D15F4586/TwitterClient.zip).
2.  Otevřete TwitterClient.exe.config a nahradit oauth_consumer_key, oauth_consumer_secret, oauth_token a oauth_token_secret Twitter tokenů, které obsahují požadované hodnoty.  

    [Postup pro vytvoření přístupový token OAuth](https://dev.twitter.com/oauth/overview/application-owner-access-tokens)  

    Nezapomeňte, že se musí být prázdné aplikace generovat token.  
3.  Nahraďte hodnoty EventHubConnectionString a EventHubName v TwitterClient.exe.config připojovacího řetězce a název rozbočovače události. Připojovací řetězec, který jste si zkopírovali vám připojovací řetězec a název rozbočovače události, takže nezapomeňte oddělte je a kde si ji jednotlivá pole v poli správné. Například zvažte následující připojovací řetězec:

        Endpoint=sb://your.servicebus.windows.net/;SharedAccessKeyName=yourpolicy;SharedAccessKey=yoursharedaccesskey;EntityPath=yourhub

    Soubor TwitterClient.exe.config by měl obsahovat nastavení jako v následujícím příkladu:

        add key="EventHubConnectionString" value="Endpoint=sb://your.servicebus.windows.net/;SharedAccessKeyName=yourpolicy;SharedAccessKey=yoursharedaccesskey"
        add key="EventHubName" value="yourhub"

    Je důležité si všimněte si, že text "EntityPath =" znamená __není__ se zobrazí v poli EventHubName hodnotu.

4.  *Volitelné:* Upravte klíčová slova pro hledání.  Ve výchozím tuto aplikaci vyhledá "Azure Skype, XBox, Microsoft, Olomouc".  Hodnoty pro **twitter_keywords** TwitterClient.exe.config, můžete upravit podle potřeby.
5.  Spusťte TwitterClient.exe ke spuštění aplikace. Zobrazí se události Tweetu hodnotami **CreatedAt** **téma**a **SentimentScore** se odešlou rozbočovače události.

    ![Analýza myšlenkou: SentimentScore hodnoty odeslaná k rozbočovači události.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-twitter-sentiment-output-to-event-hub.png)

## <a name="create-a-stream-analytics-job"></a>Vytvoření úlohy toku analýzy

Teď jsou streamování Tweetu událostí v reálném čase z Twitter jsme nastavit toku analýzy úlohy a analyzujte data v reálném čase tyto události.

### <a name="provision-a-stream-analytics-job"></a>Zřízení úlohy toku analýzy

1.  V [Azure portál](https://manage.windowsazure.com/), klikněte na **Nový** > **Datové služby** > **Toku analýzy** > **Vytvořit**.
2.  Zadejte následující hodnoty a klikněte na **Vytvořit toku analýzy úlohy**:

    * **Název úlohy**: Zadejte název projektu.
    * **Oblast**: vyberte požadovanou oblast, ve které chcete spustit úlohu. Zvažte umístění úkoly a události centrální ve stejné oblasti zajistit lepší výkon a ujistěte se, že si nebudete platíte přenášet data mezi oblastmi.
    * **Úložiště účtu**: Zvolte účet Azure úložiště, který chcete použít pro uložení monitorování dat pro všechny úlohy toku analýz spuštěných v této oblasti. Máte možnost zvolit existujícího účtu úložiště, nebo vytvořte nový účet.

3.  Klikněte na **Analýza toku** v levém podokně seznamu úloh toku analýzy.  
    ![Ikona služby technologie pro analýzu toku](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-service-icon.png)

    Zobrazí se nová úloha se stavem **VYTVOŘENO**. Všimněte si, že je zakázané v dolní části stránky na tlačítko **START** . Než začnete úlohy musíte nakonfigurovat úlohu vstupní, výstup a dotazů.


### <a name="specify-job-input"></a>Určení úlohu vstupní
1.  V toku analýzy práce klikněte na **VSTUPY** v horní části stránky a potom klikněte na **Přidat vstupní**. Dialogové okno, které se otevře vás provede procesem počet návodu pro nastavení vašeho zadání vstupních hodnot.
2.  Klikněte na **toku dat**a potom klikněte na tlačítko vpravo.
3.  Klikněte na **Centrum událostí**a klikněte pravým tlačítkem.
4.  Zadejte nebo vyberte následující hodnoty na třetí stránce:

    * **Zadávání ALIAS**: Zadejte popisný název pro tuto úlohu vstupní, například *TwitterStream*. Všimněte si, že použijete tento název v dotazu později.
    **Centrální události**:-li centrální událost, kterou jste vytvořili ve stejném předplatném jako úlohu toku analýzy, vyberte obor názvů, který je v centru události.

        Pokud vaše Centrum událostí v jiné předplatné, klikněte na **Použití centrální událost z jiné předplatné**a ručně zadejte informace o ** **Obor názvů BUS služby**, **Centrální název události**, centrální zásad název události**, **Klíč zásad centrální události**a **Počet oddílů centrální události**.

    * **Název centrální události**: vyberte název centru události.

    * **Název zásady centrální události**: vyberte zásadu centrální události, který jste vytvořili dříve v tomto kurzu.

    * **Událost centrální spotř skupiny**: Zadejte název skupiny příjemce, které jste vytvořili dříve v tomto kurzu.
5.  Klikněte pravým tlačítkem.
6.  Zadejte následující hodnoty:

    * **Formát SERIALIZER události**: JSON
    * **Kódování**: UTF8

7.  Kliknutím na tlačítko **Zkontrolovat** přidat tento zdroj a pro ověření, že toku analýzy můžete úspěšně se připojit k centru události.

### <a name="specify-job-query"></a>Zadat dotaz projektu

Technologie pro analýzu toku podporuje modelu jednoduché, deklarativní dotazu, který popisuje transformace. Další informace o jazyce, najdete v článku [Azure toku technologie pro analýzu Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx).  Tento kurz se můžete vytvářet a vyzkoušet několik dotazů přes Twitter data.

#### <a name="sample-data-input"></a>Vložení ukázkových dat

Ověřte dotaz na data skutečné práce, můžete použít funkci **UKÁZKOVÁ DATA** extrahovat události z vašeho proudu a vytvoření souboru .json událostí pro účely testování.

1.  Vyberte zadání centrální událostí a klikněte na **UKÁZKOVÁ DATA** v dolní části stránky.
2.  V dialogovém okně, které se otevře zadejte **Počáteční čas** spuštění shromažďování dat a po **dobu trvání** kolik další data pro používání.
3.  Klikněte na tlačítko **Podrobnosti** a potom klikněte na odkaz **Kliknutím sem** ke stažení a uložit soubor vygenerovaných .json.

#### <a name="pass-through-query"></a>Předávací dotaz
Zahájíte jsme udělá jednoduchý předávací dotaz této projekty všechna pole v události.

1.  Klikněte na **dotaz** v horní části stránky úlohy toku analýzy.
2.  V editoru kódu nahraďte šablony počáteční dotazu takto:

        SELECT * FROM TwitterStream

    Ujistěte se, že název vstupní zdroj shoduje s názvem vstupu, který jste zadali dříve.

3.  V části editoru dotazů klikněte na tlačítko **Testovat** .
4.  Přejděte na soubor .json vzorku.
5.  Klikněte na tlačítko **Zkontrolovat** a zobrazte výsledky pod definice dotazu.

    ![Výsledky zobrazené níže definice dotazu](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-sentiment-by-topic.png)

#### <a name="count-of-tweets-by-topic-tumbling-window-with-aggregation"></a>Počet tweety podle tématu: Tumbling okno s agregace

Pokud chcete porovnat počet zmínky mezi tématy, použijeme [TumblingWindow](https://msdn.microsoft.com/library/azure/dn835055.aspx) získat počet zmínky podle tématu každých pět sekund.

1.  Změna dotazu v editoru kódu pro:

        SELECT System.Timestamp as Time, Topic, COUNT(*)
        FROM TwitterStream TIMESTAMP BY CreatedAt
        GROUP BY TUMBLINGWINDOW(s, 5), Topic

    Tento dotaz používá **Časové razítko podle** klíčových slov pro zadání časového razítka pole v datové se nemusí používat v časový výpočtu. Pokud toto pole nebyla zadána, by pomocí údaj o čase, každou událost jste se dostali na centra události provedena operace práce s okny.  Další informace najdete v části "Čas a aplikace příletu" [toku analýzy dotazu](https://msdn.microsoft.com/library/azure/dn834998.aspx)odkazu.

    Tento dotaz umožňuje časového razítka na konci každé okno také přístup pomocí vlastnosti **System.Timestamp** .

2.  V části editoru dotazů a zobrazte výsledky dotazu klikněte na **znovu spustit** .

#### <a name="identify-trending-topics-sliding-window"></a>Určení trendu témata: posuvná okna

Jak identifikovat trendu témata, podíváme pro témata, která křížové mezní hodnota pro zmínky v dané časový úsek. Pro účely tohoto kurzu budeme budete vyhledat témata, která jsou podle víc než 20 časy posledních pět sekund pomocí [SlidingWindow](https://msdn.microsoft.com/library/azure/dn835051.aspx).

1.  Změna dotazu v editoru kódu pro: Vyberte System.Timestamp jako dobu, tématu počet (*) jako zmínky z TwitterStream časové razítko tak, že CreatedAt skupiny tak, že SLIDINGWINDOW(s, 5), téma s POČÍTAT (*) > 20

2.  V části editoru dotazů a zobrazte výsledky dotazu klikněte na **znovu spustit** .

    ![Výstup dotazu posuvná okna](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-query-output.png)

#### <a name="count-of-mentions-and-sentiment-tumbling-window-with-aggregation"></a>Počet zmínky a myšlenkou: Tumbling okno s agregace
Výsledný dotaz, který bude testování používá **TumblingWindow** získat počet zmínky, průměr, minimum, maximum a směrodatnou odchylkou myšlenkou skóre pro každé téma každých pět sekund.

1.  Změna dotazu v editoru kódu pro:

        SELECT System.Timestamp as Time, Topic, COUNT(*), AVG(SentimentScore), MIN(SentimentScore),
        Max(SentimentScore), STDEV(SentimentScore)
        FROM TwitterStream TIMESTAMP BY CreatedAt
        GROUP BY TUMBLINGWINDOW(s, 5), Topic

2.  V části editoru dotazů a zobrazte výsledky dotazu klikněte na **znovu spustit** .
3.  Toto je dotaz, který použijete pro naše řídicího panelu.  Klepněte na tlačítko **Uložit** v dolní části stránky.


## <a name="create-output-sink"></a>Vytvoření jímky výstup

Teď, když nám definovali události toku, rozbočovači události vstup jedí událostí a dotaz: Pokud chcete provádět transformace toku, posledním krokem je definovat jímky výstup pro daný úkol.  K úložišti objektů Blob Azure jsme budete zapsat agregované tweetu události z našich úlohy dotazu.  Výsledky může taky nabízená k databázi SQL Azure, úložiště tabulek Azure nebo rozbočovače události, v závislosti na určité aplikace potřebuje.

Pomocí následujících kroků vytvořit kontejner pro úložiště objektů Blob Pokud ještě nemáte jeden:

1.  Použití existujícího účtu úložiště nebo vytvořte nový účet úložiště po kliknutí na **Nový** > **Datové služby** > **úložiště** > **Vytvořit**a pak postupujte podle pokynů na obrazovce.
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

## <a name="start-job"></a>Zahájení práce

Vzhledem k tomu úlohu vstupní, dotazu a výstup všech zadáno, jste připraveni začít úlohy toku analýzy.

1.  Z úlohy **řídicího PANELU**klikněte na tlačítko **START** v dolní části stránky.
2.  V dialogovém okně, které se otevře klikněte na **čas zahájení projektu**a potom klikněte na tlačítko **Zkontrolovat** v dolní části dialogového okna. Stav úlohy se změní na **začátek** a krátce změní na **systém**.


## <a name="view-output-for-sentiment-analysis"></a>Zobrazit výstup pro analýzu myšlenkou

Až práce je spuštěná a zpracování v reálném čase Twitter toku, zvolte, jak budete chtít zobrazit výstup pro analýzu myšlenkou. Pomocí nástroje jako [Úložiště Průzkumník Windows Azure](https://azurestorageexplorer.codeplex.com/) nebo [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) zobrazit výstup projektu v reálném čase. Tady slouží k rozšíření aplikace zahrnout podobné následující snímek obrazovky přizpůsobeného řídicího panelu [Power BI](https://powerbi.com/) .

![Analýza sociálních médií: toku analýzy myšlenkou analýzy (názoru dolování) výstup v řídicím panelu Power BI.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-output-power-bi.png)

## <a name="get-support"></a>Získání podpory
Další pomoc Vyzkoušejte naše [Fórum komunity Azure toku analýzy](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Další kroky

- [Úvod do technologie pro analýzu Azure toku](stream-analytics-introduction.md)
- [Začínáme s používáním analýzy toku Azure](stream-analytics-get-started.md)
- [Změna velikosti úlohy Azure toku analýzy](stream-analytics-scale-jobs.md)
- [Odkaz na Azure toku analýzy dotaz jazyka](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure toku analýzy správy REST API odkaz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
