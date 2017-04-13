<properties
    pageTitle="Kódování majetku pomocí pomocí portálu Azure Media Encoder standardní | Microsoft Azure"
    description="Tento kurz vás provede jednotlivými kroky kódování majetku pomocí pomocí portálu Azure Media Encoder standardní."
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


# <a name="encode-an-asset-using-media-encoder-standard-with-the-azure-portal"></a>Kódování majetku pomocí Media Encoder standardní pomocí portálu Azure

> [AZURE.NOTE] Pro dokončení tohoto kurzu, třeba účet Azure. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/). 

Při práci s Azure Media Services jednu obvyklé scénáře je poskytly adaptivní bitrate streamování klienty. Media Services podporuje následující adaptivní přenosová streamování technologie: HTTP Live datových proudů (HLS) hladký přenos, MPEG POMLČKU a konfigurace (pro Adobe PrimeTime/přístup nabyvatelů licence pouze). Příprava adaptivní bitrate streamování videa, budete muset kódovat zdroje videa do více bitrate souborů. Měli byste použít **Media Encoder standardní** encoder kódování videa.  

Media Services také poskytuje dynamické balení, který umožňuje poskytovat více bitrate MP4s v následujících formátech streamování: MPEG ČÁRKU, HLS, hladký přenos nebo běhu, aniž by bylo nutné znovu balíček do těchto streamování formáty. Dynamické balení potřebujete jenom k ukládání a zaplatit soubory ve formátu jedné úložiště a Media Services bude vytvořit a vytisknout odpovídající odpověď založená na žádosti o z klienta.

Umožní využít výhod dynamické balení potřebujete postupujte takto:

- Kódování zdrojový soubor do sadu souborů, více bitrate MP4 (kódování kroky jsou v této části prokázáno později).
- Potřebujete alespoň jeden streamování jednotku pro streamování koncový bod, ze kterého chcete doručení obsahu. Další informace najdete v tématu [Konfigurace streamování koncové body](media-services-portal-vod-get-started.md#configure-streaming-endpoints). 

Zobrazit zpracování média, najdete v [tomto](media-services-portal-scale-media-processing.md) tématu.

## <a name="encode-with-the-azure-portal"></a>Kódování pomocí portálu Azure

Tato část popisuje kroky, kterými můžete kódování obsahu s Media Encoder standardní.

1.  [Azure portál](https://portal.azure.com/)vyberte svůj účet Azure Media Services.
2.  V okně **Nastavení** zvolte **prostředky**.  
2.  V okně **prostředky** vyberte majetek, který byste chtěli kódování.
3.  Stiskněte tlačítko **kódovat** .
4.  V okně **kódovat aktivum** vyberte "Media Encoder standardní" procesoru a předvolby. Například, pokud znáte vstupní videa má rozlišení 1920 × 1080 pixelů, pak můžete použít "H264 více Bitrate 1080p" přednastavené. Další informace o předvolby, najdete v [tomto](https://msdn.microsoft.com/library/azure/mt269960.aspx) článku – je důležité vyberte přednastavení, která je nejvhodnější pro zadání videa. Pokud máte nízkém rozlišení (640 x 360) a pak neměli používat jako výchozí "H264 více Bitrate 1080 p" přednastavené.
    
    Pro snadnější správu máte možnost upravit název výstup materiálů a název projektu.
        
    ![Kódování prostředky](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. Stisknutím klávesy **vytvořit**.


##<a name="next-step"></a>Další krok

Kódování pokrok projektu pomocí portálu Azure můžete sledovat způsobem popsaným v [tomto](media-services-portal-check-job-progress.md) článku.  

##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


