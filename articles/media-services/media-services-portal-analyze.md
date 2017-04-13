<properties
    pageTitle="Analýza médií pomocí portálu Azure | Microsoft Azure"
    description="Toto téma popisuje, jak zpracuje médií s procesorů médií analýzy médií (MPs) pomocí portálu Azure."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="juliako"/>


# <a name="analyze-your-media-using-the-azure-portal"></a>Analýza médií pomocí portálu Azure

> [AZURE.NOTE] Pro dokončení tohoto kurzu, třeba účet Azure. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="overview"></a>Základní informace

Azure Media Services analýzy je součástí rozpoznávání řeči a vize (na enterprise měřítko, dodržování předpisů, zabezpečení a globální reach), které usnadňují pro organizace a podniky odvodit akce přehledy z jejich videosoubory. Podrobnější popis Azure Media Services analýzy najdete v části [tohoto](media-services-analytics-overview.md) tématu. 

Toto téma popisuje, jak zpracuje médií s procesorů médií analýzy médií (MPs) pomocí portálu Azure. Média analýzy MPs plodiny MP4 soubory nebo JSON. Procesor médií vyrobeno souboru MP4, který soubor postupně stáhnout. Pokud procesor médií vyrobeno souboru JSON, může soubor stáhnout z úložiště objektů blob Azure. 

## <a name="choose-an-asset-that-you-want-to-analyze"></a>Zvolte aktiva, která chcete analyzovat 
 
1. [Azure portál](https://portal.azure.com/)vyberte svůj účet Azure Media Services.
2. V okně **Nastavení** zvolte **prostředky**.  
.
    ![Analýza videa](./media/media-services-portal-analyze/media-services-portal-analyze001.png)

2. Vyberte materiálů, která chcete analyzovat a stiskněte tlačítko **Analýza** .
        
    ![Analýza videa](./media/media-services-portal-analyze/media-services-portal-analyze002.png)

3. V okně **majetku médií proces analýzy médií** vyberte procesor. 

    Zbytek článek vysvětluje, proč a jak používat každý procesor. 
   
4. Stisknutím klávesy **vytvořit** zahájení projektu.

## <a name="azure-media-indexer"></a>Indexování Azure Media

Procesor médií **Azure Media indexování** umožňuje vytvořit mediální soubory a obsahu s možností vyhledávání, stejně jako generovat skrytých titulků sleduje. Tato část nabízí některé informace o možnostech, které můžete použít pro tento MP.

![Analýza videa](./media/media-services-portal-analyze/media-services-portal-analyze003.png)

### <a name="language"></a>Jazyk

Přirozeného jazyka uplatnit v multimediálních souborů. Například v angličtině a španělštině. 

### <a name="captions"></a>Titulků

Můžete zvolit formát popisků k obrázkům, generovaný z obsahu. Indexovací úlohy můžete generovat titulků soubory v těchto formátech:  

- **Sami**
- **TTML**
- **WebVTT**

Zavřený titulek kopie soubory v těchto formátech mohou sloužit k usnadnění zvukových souborů a videosouborů pro osoby se sluchovým postižením.

### <a name="aib-file"></a>Soubor AIB

Tuto možnost vyberte, pokud chcete generovat souboru zvuk Index Blob pro použití s vlastní IFilter SQL serveru. Další informace najdete v tématu [Tento](https://azure.microsoft.com/blog/using-aib-files-with-azure-media-indexer-and-sql-server/) blogu.

### <a name="keywords"></a>Klíčová slova

Tuto možnost vyberte, pokud chcete vytvořit soubor XML klíčových slov. Tento soubor obsahuje klíčová slova extrahuje z obsahu řeči počet_plateb a posun informace.

### <a name="job-name"></a>Název úlohy

Popisný název, který umožňuje určit práci. [Tento](media-services-portal-check-job-progress.md) článek popisuje, jak můžete sledovat průběh projektu. 

### <a name="output-file"></a>Výstupní soubor

Popisný název, který umožňuje určit výstup obsah. 

## <a name="azure-media-hyperlapse"></a>Azure Media Hyperlapse

Azure Media Hyperlapse je MP, které vytvoří hladký zrušení čas videa z první osoba nebo akce kamery obsah.  Další informace najdete v tématu [Toto](media-services-hyperlapse-content.md) . Tato část nabízí některé informace o možnostech, které můžete použít pro tento MP.

![Analýza videa](./media/media-services-portal-analyze/media-services-portal-analyze004.png)

### <a name="speed"></a>Rychlost 

Určení rychlé zvládnutí k dosažení vyššího vstupní video. Výstup je stabilizované a zrušení čas verze vstupní video.

### <a name="job-name"></a>Název úlohy

Popisný název, který umožňuje určit práci. [Tento](media-services-portal-check-job-progress.md) článek popisuje, jak můžete sledovat průběh projektu. 

### <a name="output-file"></a>Výstupní soubor

Popisný název, který umožňuje určit výstup obsah. 

## <a name="azure-media-face-detector"></a>Nominální detekce Azure Media

Procesor **Azure Media nominální detekce** médií (MP) umožňuje spočítat, sledovat pohyby a dokonce odhadnout účast v cílové skupiny a reakce prostřednictvím výrazy obličeje. Tato služba obsahuje dvě funkce: 

- **Zjišťování obličej**

    Nominální zjišťování najde a sleduje lidské ploch v rámci videa. Více ploch lze zjistit a následně sledovat pohyb jejich s uvedením času a místa metadata vrácený v souboru JSON. Během sledování pokusí dávat konzistentní ID stejné nominální během je osoba manipulace na obrazovce, i když jsou brání nebo stručné ponechat rámce.

    >[AZURE.NOTE]Tato služba neprovádí obličeje rozpoznávání. Osoba, která ponechá rámce nebo změní brání pro příliš dlouho bude mít nové ID když se vrátí.

- **Zjišťování emoce**
    
    Zjišťování emoce je nepovinné součástí procesor médií nominální zjišťování, který vrátí analýzy na více duševní atributů z plochy nerozpoznal, včetně štěstí, sadness, obav, anger apod. 

![Analýza videa](./media/media-services-portal-analyze/media-services-portal-analyze005.png)

### <a name="detection-mode"></a>Režim zjišťování

Jeden z těchto režimů může používat procesor:

- zjišťování obličej
- za zjišťování emoce obličej
- agregační emoce zjišťování

### <a name="job-name"></a>Název úlohy

Popisný název, který umožňuje určit práci. [Tento](media-services-portal-check-job-progress.md) článek popisuje, jak můžete sledovat průběh projektu. 

### <a name="output-file"></a>Výstupní soubor

Popisný název, který umožňuje určit výstup obsah. 

## <a name="azure-media-motion-detector"></a>Azure Media pohybu detekce

Procesor **Azure Media pohybu detekce** médií (MP) umožňuje efektivní určit oddíly úrok v opačném případě dlouhé a bezproblémové video. Zjišťování pohybu lze na udělat střih statické kamery identifikují oddíly videa, které se vyskytuje pohybu. Vygeneruje JSON soubor obsahující metadatům časová razítka a ohraničující oblasti, kdy došlo k události.

Určené k zabezpečení videa kanálů, tento technologie je možné rozdělit pohybu do příslušné událostí a falešně pozitivní jako jsou stíny a změny osvětlení. Umožňuje generovat výstrah zabezpečení z fotoaparátu kanálů bez nevyžádané pošty nekonečné důležité události, a při tom extrahovat chvíli úrok z velmi dlouhý sledování videa.

![Analýza videa](./media/media-services-portal-analyze/media-services-portal-analyze006.png)

## <a name="azure-media-video-thumbnails"></a>Azure Media Video miniatur

Tento procesor vám pomůže vytvářet souhrny dlouhé videa automaticky výběrem zajímavé výstřižky z zdroje videa. To je užitečné, pokud chcete zadat rychlý přehled toho, co můžete očekávat dlouhé videa. Podrobné informace a příklady najdete v tématu [Použití Azure Media Video miniatury k vytvoření souhrnu Video](media-services-video-summarization.md)

![Analýza videa](./media/media-services-portal-analyze/media-services-portal-analyze008.png)

### <a name="job-name"></a>Název úlohy

Popisný název, který umožňuje určit práci. [Tento](media-services-portal-check-job-progress.md) článek popisuje, jak můžete sledovat průběh projektu. 

### <a name="output-file"></a>Výstupní soubor

Popisný název, který umožňuje určit výstup obsah. 


##<a name="next-steps"></a>Další kroky

Zobrazení Media Services naučné stezky.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


