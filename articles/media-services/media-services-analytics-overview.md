<properties
    pageTitle="Mediální služby Azure přehled analýzy | Microsoft Azure"
    description="Azure Media Services nabízí veřejné náhled Azure Media analýzy kolekce rozpoznávání řeči a počítače zrakem společnosti Microsoft enterprise měřítko, dodržování předpisů, zabezpečení a globální sítě. Služby Azure Media analýzy jsou vytvořené pomocí základní součásti platformu Azure Media Services a tedy připraveni zpracovat médií zpracování ve velkém měřítku na den v měsíci. "
    services="media-services"
    documentationCenter=""
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/24/2016"   
    ms.author="milanga;juliako;johndeu"/>

# <a name="azure-media-services-analytics-overview"></a>Mediální služby Azure přehled analýzy

##<a name="overview"></a>Základní informace

Další organizace a podniky jsou zahrnuje video jako upřednostňované střední školte zaměstnance, jejich, zapojit jejich zákazníci a dokumentu obchodními funkcemi. Cloud computing umožňuje efektivní moct ukládat, můžete vysílat datovými proudy a používat tyto mediální velké soubory, ale jako společnosti postupně se zvětšují jejich videa Knihovna obsahu, musí mít stejně účinné prostředky pro extrahování nové informace z videa k vytvoření smysluplnější, individuální interakce s jejich cílových skupin a přejdou na vyšší úroveň.

Při řešení tohoto zvyšuje potřeba Tržiště, nabízí Azure Media Services médií analýzy, součástí rozpoznávání řeči a zrakem (na enterprise měřítko, dodržování předpisů, zabezpečení a globální reach), které usnadňují pro organizace a podniky odvodit akce přehledy z jejich videosoubory. Služby Azure Media analýzy jsou vytvořené pomocí základní součásti platformu Azure Media Services a tedy připraveni zpracovat médií zpracování ve velkém měřítku na den v měsíci.

Azure Media analýzy vývojářům můžete rychle začít pracovat s funkcí zrakem pro video ve velkém měřítku omezené a uvedení pokročilé funkce do aplikace. Azure Media analýzy vytvořené pro použití v prostředí organizace s celou škálu, dodržování předpisů, zabezpečení a globální reach vyžadované velké organizace.

Na následujícím obrázku vidíte **Médií technologie pro analýzu** a jiných hlavní část platformu Media Services. 

![VoD pracovního postupu](./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png)

Média analýzy médií procesorů plodiny MP4 soubory nebo JSON. Procesor médií vyrobeno souboru MP4, který soubor postupně stáhnout. Pokud procesor médií vyrobeno souboru JSON, může soubor stáhnout z úložiště objektů blob Azure. 

## <a name="azure-media-analytics-services"></a>Služby Azure Media analýzy

- **Indexování** – Azure Media indexování umožňuje zpřístupnění obsahu s možností vyhledávání, stejně jako generovat skrytých titulků stop. Azure Media Services uvolní **Azure Media indexování 2 Preview** se rychleji indexování a širší podpora jazyků. Podporované jazyky patří angličtina, španělština, francouzština, němčina, italština, čínština, portugalština a arabština. Podrobné informace a příklady najdete v tématu [proces videa s Azure Media indexování 2](media-services-process-content-with-indexer2.md)
 
- **Hyperlapse** – Microsoft Hyperlapse je výsledkem víc než 20 let počítač zrakem výzkumu v Microsoft Research (MSR), Diakritické znaménko videa stabilizačních a čas ukončení vytvořit rychlé, spotřební, krásné videa z dlouhé formuláře obsahu. Kromě vytváření dojde k chybě času, můžete také Hyperlapse vytvoření stabilní videa z zobrazuje rozostřený videa zachytit prostřednictvím mobilní telefony a kamery. Podrobné informace a příklady najdete v tématu [Hyperlapse mediální soubory s Azure Media Hyperlapse](media-services-hyperlapse-content.md)
 
- **Zjišťování pohybu** – používáte tuto službu ke zjištění pohybu v videa pomocí kodeků šablony pozadí. Toto je ideální pro zákazníky, kteří chtějí zkontrolujte falešně pozitivních o událostech pohybu zjištěná dohled kamery na sledování videa kanálů. Podrobné informace a příklady najdete v tématu [Zjišťování pohybu pro službu Azure Media Analytics](media-services-motion-detection.md).
 
- **Nominální zjišťování a nominální emotikony** – pomocí této služby dokáže rozpoznat ploch lidí a jejich emotikony, včetně štěstí sadness, překvapení, anger, cestou, obav, působící a indifference/neutrální. Tento postup je několik užitečných odvětví aplikací, popsáno níže, včetně agregaci a analýza reakce lidí účastnit se jich události. Podrobné informace a příklady najdete v tématu [nominální a emoce rozpoznávání Azure Media analýzy](media-services-face-and-emotion-detection.md).
 
- **Video souhrnu** – souhrnu Video vám pomůže vytvářet souhrny dlouhé videa automaticky výběrem zajímavé výstřižky z zdroje videa. To je užitečné, pokud chcete zadat rychlý přehled toho, co můžete očekávat dlouhé videa. Podrobné informace a příklady najdete v tématu [Použití Azure Media Video miniatury k vytvoření souhrnu Video](media-services-video-summarization.md)

- **Optické rozpoznávání znaků** - Azure Media analýzy OCR (optické rozpoznávání znaků) umožňuje převést textového obsahu v videosoubory upravovat, vyhledávání digitální text. Umožňuje provádět automatické extrahování smysluplné metadat z videa signálu médií.
 
- **Scalable nominální redigování** - **Azure Media Redactor** je Azure Media analýzy MP, nabízející redigování scalable obličej v cloudu. Nominální redigování umožňuje změnit videa k rozostření ploch vybraný osobám. Je vhodné použít službu redigování obličej v veřejné bezpečnosti a multimediální zprávy scénáře. Může trvat několik minut udělat střih, která obsahuje více ploch hodin redigovat ručně, ale k této službě procesu redigování nominální vyžadují jen pomocí několika jednoduchých kroků. Další informace najdete v [tomto](media-services-face-redaction.md) článku.

 
## <a name="common-scenarios"></a>Obvyklé scénáře

Tady je několik scénáře, kde Azure Media analýzy můžou pomoct organizace a podniky přes odvětví glean nové informace z video, pokud chcete vytvořit víc přizpůsobených cílové skupiny a závazky zaměstnance, stejně jako efektivněji spravovat velké množství obsahu videa:

- **Volání centra** – sudé s adventní sociálních médií, zákazníka volání centra pořád usnadnění velké procento služby transakce zákazníka. Zakódovaný v těchto datech zvuku je velkému množství informací o zákaznících, které lze analyzovat ke zlepšení plánování produktu a také školte zaměstnance Centrum volání k dosažení vyšší spokojenost zákazníků. Pomocí Azure Media indexování zákazníci se extrahovat text a vytvořit index vyhledávání a řídicí panely extrahovat intelligence kolem nejčastějším nahlásí, že, nahlásí, že zdroj a další relevantní data.

- **Moderování obsahu generovaný uživatelem** – z příspěvků mediálních policejní oddělením mnoho organizací má veřejné vystaveného portály kde přijmutí UGC médiu, jako jsou videa nebo obrázky. Objem obsahu můžete dosahuje kvůli neočekávané události. V těchto případech je poblíž možné vedení efektivní ruční revize obsahu u vhodnosti. Zákazníci můžete spolehnout na službu moderování obsahu zaměřit se na obsah, který je vhodné.

- **Sledování** - růst IP kamery, je rozbalení sledování videí. Ruční kontrola sledování videa je čas náročné a chybám lidské chybu. Azure Media analýzy obsahuje některé komponenty například pohybu zjišťování, nominální zjišťování a Hyperlapse proces revize, Správa a vytváření deriváty snadnější.

## <a name="media-services-analytics-media-processors"></a>Mediální služby technologie pro analýzu médií procesorů 

Tato část obsahuje seznam všech média služby technologie pro analýzu médií procesorů (MP) a zobrazí používání .NET nebo ZBÝVAJÍCÍ získat MP objekt.

### <a name="mp-names"></a>Názvy MP


- Náhled indexování 2 Azure Media
- Indexování Azure Media
- Azure Media Hyperlapse
- Nominální detekce Azure Media
- Azure Media pohybu detekce
- Azure Media Video miniatur
- Azure Media OCR

### <a name="net"></a>.NET

Následující funkce má jednu zadané názvy MP a vracet MP objekt.

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors
            .Where(p => p.Name == mediaProcessorName)
            .ToList()
            .OrderBy(p => new Version(p.Version))
            .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }


## <a name="rest"></a>ZBÝVAJÍCÍ

Žádost o:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Azure%20Media%20OCR' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.12
    Host: media.windows.net
    
Odpověď:
        
    . . .
    
    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:074c3899-d9fb-448f-9ae1-4ebcbe633056",
             "Description":"Azure Media OCR",
             "Name":"Azure Media OCR",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }

##<a name="demos"></a>Ukázky

[Azure Media analýzy ukázky](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

##<a name="next-steps"></a>Další kroky

Prohlédněte si Media Services naučné stezky.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-articles"></a>Související články

[Oznámení o dítěte Media Services analýzy](https://azure.microsoft.com/blog/introducing-azure-media-analytics/)
  

<!-- Images -->

[overview]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png
