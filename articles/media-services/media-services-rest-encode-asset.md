<properties 
    pageTitle="Jak kódovat majetku pomocí Media Encoder standardní | Microsoft Azure" 
    description="Naučte se používat Media Encoder Standard kódování médií obsahu na Media Services. Ukázky pomocí rozhraní REST API." 
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
    ms.date="09/19/2016"
    ms.author="juliako"/>


#<a name="how-to-encode-an-asset-using-media-encoder-standard"></a>Jak kódovat majetku pomocí Media Encoder standardní


> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-encode-with-media-encoder-standard.md)
- [ZBÝVAJÍCÍ](media-services-rest-encode-asset.md)
- [Portál](media-services-portal-encode.md)

##<a name="overview"></a>Základní informace
Abyste mohli poskytování digitálního videa přes internet musí Komprimovat média. Digitální videosoubory jsou velmi velké a může být moc velký pro předvádění přes internet nebo pro vaši zákazníci zařízení správné zobrazení. Kódování je proces komprimace videa a zvuky, aby vaši zákazníci mohli sledovat médií.

Kódování úlohy jsou jednou nejběžnější zpracování v Media Services. Vytvoření kódování úlohy převést mediální soubory z jednoho kódování do jiného. Při kódování, můžete Media Services předdefinované encoder (Media Encoder standardní zobrazení). Můžete také encoder poskytovanou partnerem Media Services; třetí strany kodéry jsou dostupné prostřednictvím webu Azure Marketplace. Můžete zadat podrobnosti kódování úkolů pomocí přednastavených řetězců definované pro vaše encoder nebo pomocí přednastavených konfigurační soubory. Typy předvolby, které jsou k dispozici najdete v tématu [Úkolu předvolby pro Media Encoder standardní](http://msdn.microsoft.com/library/mt269960).

Jednotlivé úlohy může mít jeden či více úkolů v závislosti na typu zpracování, kterou chcete provést. Prostřednictvím rozhraní REST API můžete vytvořit úlohy a související úkoly jedním ze dvou způsobů:

- Úkoly mohou být definované vložené prostřednictvím vlastnosti navigace úkolů na projektu entity nebo
- až OData dávkové zpracování.


Doporučuje se vždy kódovat mezzanine souborů do adaptivní bitrate MP4 nastavení a následný převod sadu na požadovaný formát pomocí [Dynamického obalu](media-services-dynamic-packaging-overview.md). Umožní využít výhod dynamické balení musíte nejprve získat nejméně jednu jednotku datových proudů na vyžádání pro streamování koncový bod, ze kterého chcete doručení obsahu. Další informace najdete v tématu [jak měřítko Media Services](media-services-portal-manage-streaming-endpoints.md).

-Li výstup materiálů úložiště zašifrovaných, musíte nakonfigurovat zásady doručení materiálů. Další informace najdete v článku [Konfigurace zásad doručení materiálů](media-services-rest-configure-asset-delivery-policy.md).


>[AZURE.NOTE]Než začnete odkazování na média procesorů, zkontrolujte, jestli máte správná médií procesor ID. Další informace najdete v tématu [Získání procesory média](media-services-rest-get-media-processor.md).

##<a name="create-a-job-with-a-single-encoding-task"></a>Vytvoření úlohy pomocí jednoho kódování úkolu

>[AZURE.NOTE] Když pracujete s Media Services REST API, platí následující omezení:
>
>Při přístupu k entit v Media Services, musíte nastavit konkrétní záhlaví pole a hodnoty v žádostech HTTP. Další informace najdete v tématu [nastavení pro médií služby REST API vývoj](media-services-rest-how-to-use.md).

>Po úspěšném připojení k https://media.windows.net, zobrazí se 301 přesměrování zadání jiného identifikátor URI médií služeb. Musíte provést následující volání nových URI způsobem popsaným v části [připojování ke službě médií pomocí rozhraní REST API](media-services-rest-connect-programmatically.md).
>
>Když pomocí JSON a zadání použití klíčového **__metadata** v žádosti o (například odkazuje na propojený objekt) musíte nastavit záhlaví **přijmout** ve formátu [JSON podrobného](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/): přijmout: aplikace/json; odata = podrobného.

Následující příklad ukazuje, jak vytvářet a publikovat projekt se jeden úkol kódování videa v určitém rozlišení a kvalitu. Při kódování s Media Encoder standardní, můžete použít předvolby konfigurace úkolu zadán [v tomto poli](http://msdn.microsoft.com/library/mt269960).

Žádost o:

PŘÍSPĚVEK https://media.windows.net/API/Jobs protokolu HTTP/1.1 typ obsahu: aplikace/json; odata = podrobného přijmout: aplikace/json; odata = podrobného DataServiceVersion: 3.0 MaxDataServiceVersion: 3.0 ms verze x: 2.11 se tak mohli ověřovat: nosný <token value> 
 x-ms--žádost – id klienta: 00000000-0000-0000-0000-000000000000 Host: media.windows.net

    
    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aaab7f15b-3136-4ddf-9962-e9ecb28fb9d2')"}}],  "Tasks" : [{"Configuration" : "H264 Multiple Bitrate 720p", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",  "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"}]}

Odpověď:
    
    HTTP/1.1 201 Created

    . . . 

###<a name="set-the-output-assets-name"></a>Nastavení názvu aktiva výstup

Následující příklad ukazuje, jak nastavit atribut assetName:

    { "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"}

##<a name="considerations"></a>Co byste měli zvážit

- Vlastnosti TaskBody musí používat literál XML určete počet vstupní nebo výstup materiálů, které se použije úkolu. Téma úkolu obsahuje definice schématu XML pro XML.
- V definici TaskBody hodnota pro každou vnitřní <inputAsset> a <outputAsset> musí být nastavený jako JobInputAsset(value) nebo JobOutputAsset(value).
- Úkol může obsahovat více výstup prostředky. Jeden JobOutputAsset(x) lze použít pouze jednou jako výstup úkolu v projektu.
- Jako aktivum vstupní úkolu můžete zadat JobInputAsset nebo JobOutputAsset.
- Úkoly nesmí formuláře obrázku.
- Parametr hodnoty, které předáváte JobInputAsset nebo JobOutputAsset představuje hodnotu indexu aktivum. Skutečné prostředky jsou definované ve vlastnostech InputMediaAssets a OutputMediaAssets navigace na definici úlohy entity. 
- Protože Media Services jsou založeny na OData v3, jednotlivé prostředky v kolekcích vlastnosti navigace InputMediaAssets a OutputMediaAssets odkazují "__metadata: identifikátor uri" pár název hodnota.
- InputMediaAssets mapuje jeden nebo více prostředky nevytvoříte Media Services. OutputMediaAssets vzniká systém. Nelze odkazovat existující materiálů.
- OutputMediaAssets můžete s názvem použití atribut assetName. Pokud tento atribut není k dispozici a pak název OutputMediaAsset bude bez ohledu vnitřní textovou hodnotu <outputAsset> element je s příponou hodnotu z pole název projektu nebo hodnotu Id úlohy (v případě, kde není definované vlastnosti název). Například pokud nastavíte hodnotu assetName "Vzorku", pak vlastnost název OutputMediaAsset by nastavit "Vzorku". Ale pokud nenastavil hodnotu assetName, ale nastavena názvu projektu "NewJob", pak název OutputMediaAsset by "_NewJob JobOutputAsset (hodnota)". 


##<a name="create-a-job-with-chained-tasks"></a>Vytvoření projektu s zřetězené úkoly

V mnoha případech aplikace vývojáři chtít vytvořit řadu zpracování úkolů. V Media Services můžete vytvořit série zřetězené úkolů. Každý úkol provede kroky jiné zpracování a můžete použít jiné médium procesorů. Zřetězené úkoly můžete předat aktivum z jednoho úkolu do jiného provádění lineární posloupnost úloh majetku. Úkolů v projektu však nemusí být v pořadí. Při vytváření zřetězené úkolu zřetězené **ITask** objekty vytvořené v jediném **IJob** objektu.

>[AZURE.NOTE] Momentálně omezení 30 úkolů na projekt. Pokud potřebujete zřetězení více než 30 úkoly, vytvořte víc než jednu úlohu má být úkoly.


    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://testrest.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A910ffdc1-2e25-4b17-8a42-61ffd4b8914c')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          },
          {  
             "Configuration":"H264 Smooth Streaming 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-16\"?><taskBody><inputAsset>JobOutputAsset(0)</inputAsset><outputAsset>JobOutputAsset(1)</outputAsset></taskBody>"
          }
       ]
    }


###<a name="considerations"></a>Co byste měli zvážit

Chcete-li povolit zřetězení úkolu:

- Úlohy musíte mít alespoň na úrovni 2 úkoly
- Musí být alespoň jeden úkol, jehož vstup je výstup jiného úkolu v projektu.

## <a name="use-odata-batch-processing"></a>Použití OData dávkové zpracování 

Následující příklad ukazuje, jak používat OData dávkové zpracování k vytvoření úlohy a úkoly. Informace o dávkové zpracování najdete v tématu [Dávkové zpracování Data (OData Open Protocol)](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).
 
    POST https://media.windows.net/api/$batch HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: multipart/mixed; boundary=batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Accept: multipart/mixed
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    Host: media.windows.net
    
    
    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Content-Type: multipart/mixed; boundary=changeset_122fb0a4-cd80-4958-820f-346309967e4d
    
    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary
    
    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-ID: 1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    
    {"Name" : "NewTestJob", "InputMediaAssets@odata.bind":["https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A2a22445d-1500-80c6-4b34-f1e5190d33c6')"]}
    
    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary
    
    POST https://media.windows.net/api/$1/Tasks HTTP/1.1
    Content-ID: 2
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    
    {  
       "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
       "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
       "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"Custom output name\">JobOutputAsset(0)</outputAsset></taskBody>"
    }

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d--
    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e--
 


## <a name="create-a-job-using-a-jobtemplate"></a>Vytvoření úlohy pomocí JobTemplate


Při zpracovávání více aktiv pomocí společnou sadu úkoly, JobTemplates jsou užitečné můžete zadat výchozí přednastavení úkolu pořadí úkolů a tak dále.

Následující příklad ukazuje, jak vytvářet JobTemplate u vloženého TaskTemplate definované. TaskTemplate používá Media Encoder standardní jako MediaProcessor kódování souboru materiálů. ale další MediaProcessors může taky. 


    POST https://media.windows.net/API/JobTemplates HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net

    
    {"Name" : "NewJobTemplate25", "JobTemplateBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><jobTemplate><taskBody taskTemplateId=\"nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789\"><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody></jobTemplate>", "TaskTemplates" : [{"Id" : "nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789", "Configuration" : "H264 Smooth Streaming 720p", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56", "Name" : "SampleTaskTemplate2", "NumberofInputAssets" : 1, "NumberofOutputAssets" : 1}] }
 

>[AZURE.NOTE]Na rozdíl od jiných entity Media Services musí definovat nové identifikátor GUID pro každou TaskTemplate a jeho umístění v taskTemplateId a vlastnost Id v žádosti o textu. Schéma obsahu identifikace postup schéma popsané v identifikovat Azure Media Services entity. Navíc JobTemplates nelze aktualizovat. Místo toho musíte vytvořit nový účet s aktualizovanými změny.
 

Pokud je úspěšná, je vrácena následující odpověď:
    
    HTTP/1.1 201 Created
    
    . . .


Následující příklad ukazuje, jak vytvořit úlohu odkazování na JobTemplate Id:

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net

    
    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A3f1fe4a2-68f5-4190-9557-cd45beccef92')"}}], "TemplateId" : "nb:jtid:UUID:15e6e5e6-ac85-084e-9dc2-db3645fbf0aa"}
     

Pokud je úspěšná, je vrácena následující odpověď:
    
    HTTP/1.1 201 Created
    
    . . . 



##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="next-steps"></a>Další kroky
Teď když víte, jak vytvořit úlohu kódování assset, přejděte na téma [Jak chcete zkontrolovat úlohy průběh s Media Services](media-services-rest-check-job-progress.md) .


##<a name="see-also"></a>Viz taky

[Získání procesorů médií](media-services-rest-get-media-processor.md)
