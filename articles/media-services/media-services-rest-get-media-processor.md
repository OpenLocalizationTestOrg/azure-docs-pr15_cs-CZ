<properties 
    pageTitle="Jak vytvořit médií procesor | Microsoft Azure" 
    description="Naučte se vytvářet součásti procesoru médií kódovat, převedení formátu, šifrování nebo dešifrování obsahu médií pro službu Azure Media Services." 
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
    ms.date="09/26/2016" 
    ms.author="juliako"/>


#<a name="how-to-get-a-media-processor-instance"></a>Postup: získání instanci procesor médií


> [AZURE.SELECTOR]
- [.NET](media-services-get-media-processor.md)
- [ZBÝVAJÍCÍ](media-services-rest-get-media-processor.md)

##<a name="overview"></a>Základní informace

V Media Services procesor médií je součást, která zajišťuje konkrétní zpracování úkolů, například kódování formátujte převodu šifrování nebo dešifrování mediální obsah. Obvykle vytvoříte médií procesoru při vytváření úkolu kódování, šifrování nebo převodu formátu obsahu médií.

Následující tabulka obsahuje název a popis procesorech dostupné média.

Název procesor média|Popis|Další informace
---|---|---
Standardní Media Encoder|Poskytuje standardní možnosti na vyžádání kódování. |[Přehled a porovnání Azure Media kodéry služba](media-services-encode-asset.md)
Pracovní postup Premium Media Encoder|Umožňuje kódování úlohy pomocí Media Encoder Premium pracovního postupu.|[Přehled a porovnání Azure Media kodéry služba](media-services-encode-asset.md)
Indexování Azure Media| Umožňuje vytvořit mediální soubory a obsahu s možností vyhledávání, stejně jako generovat skrytých titulků skladeb a klíčových slov.|[Indexování Azure Media](media-services-index-content.md)
Azure Media Hyperlapse (verze preview)|Umožňuje hladce, "hrboly" ve svém videu s videa ustálení. Lze také dosažení vyššího obsahu do spotřební klipartu.|[Azure Media Hyperlapse](media-services-hyperlapse-content.md)
Azure Media Encoder|Odepsat
Dešifrování úložiště| Odepsat|
Azure Media Balíčkovač|Odepsat|
Azure Media Encryptor|Odepsat|

##<a name="get-mediaprocessor"></a>Získání MediaProcessor

>[AZURE.NOTE] Když pracujete s Media Services REST API, platí následující omezení:
>
>Při přístupu k entit v Media Services, musíte nastavit konkrétní záhlaví pole a hodnoty v žádostech HTTP. Další informace najdete v tématu [nastavení pro médií služby REST API vývoj](media-services-rest-how-to-use.md).

>Po úspěšném připojení k https://media.windows.net, zobrazí se 301 přesměrování zadání jiného identifikátor URI médií služeb. Musíte provést následující volání nových URI způsobem popsaným v části [připojování ke službě médií pomocí rozhraní REST API](media-services-rest-connect-programmatically.md). 


Následující volání ZBÝVAJÍCÍ ukazuje, jak získat instanci procesor média podle názvu (v tomto případě **Media Encoder standardní**). 



    
Žádost o:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.11
    Host: media.windows.net
    
Odpověď:
        
    . . .
    
    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }


##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="next-steps"></a>Další kroky

Teď když víte, jak získat instanci procesor média, přejděte na téma [jak kódovat aktivum](media-services-rest-get-started.md) , které vám ukáže, jak používat Media Encoder Standard kódování aktivum.
