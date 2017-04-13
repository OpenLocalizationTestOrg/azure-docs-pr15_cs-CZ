<properties
    pageTitle="Jak oříznout videa | Microsoft Azure"
    description="Tento článek ukazuje, jak chcete-li oříznout videa s Media Encoder standardní."
    services="media-services"
    documentationCenter=""
    authors="anilmur"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/26/2016"  
    ms.author="anilmur;juliako;"/>

# <a name="crop-videos-with-media-encoder-standard"></a>Oříznutí videa s Media Encoder standardní

Chcete-li oříznout zadání videa můžete Media Encoder standardní (MES). Oříznutí je proces výběr obdélníkové okno v rámci na rámeček videa a kódování jenom pixelů v tomto okně. Následující diagram pomáhá znázornění procesu.

![Oříznutí videa](./media/media-services-crop-video/media-services-crop-video01.png)

Předpokládejme, že máte předávat na vstupu videa, které má rozlišení 1920 × 1080 pixelů (poměr stran 16:9), ale má černé pruhy (pilíře políčka) na levé a pravé, takže jenom okna 4:3 nebo 1440 × 1080 pixelů obsahuje aktivní video. Můžete použít MES oříznutí nebo úprava, černé pruhy a kódovat oblasti 1440 × 1080.

Oříznutí v MES je předem zpracování fázi, takže oříznutí parametrů v předvolby kódování použít původní vstupní video. Kódování je další fázi a vhodná nastavení výška a šířka *předem zpracování* videa a nikoli původní video. Při návrhu přednastavení musíte provést následující akce: (a) vyberte parametry oříznutí založený na původní vstupní video a (b) vaší kódovat nastavení podle oříznutá videa. Pokud se neshodují vaší kódovat nastavení oříznuté video, výstup nebudou podle očekávání.

[Následující](media-services-advanced-encoding-with-mes.md#encoding_with_dotnet) téma ukazuje, jak vytvořit kódování úlohu s MES a jak zadat vlastní přednastavení kódování úkolu. 

## <a name="creating-a-custom-preset"></a>Vytváření vlastních předvolby

V příkladu v diagramu:

1. Původní vstupní je 1920 × 1080
1. Musí být oříznutý výstup 1440 × 1080, který je umístěn v rámci vstupní
1. Znamená to, že X posun (1920 – 1440) / 2 = 240 a Y posun nulovou
1. Zadejte šířku a výšku obdélníku oříznutí jsou 1440 a 1080, respektive
1. V oblasti kódovat položit má plodiny tři vrstvy jsou, rozlišení 1440 × 1080 960 x 720 a 480 x 360, respektive

###<a name="json-preset"></a>Předvolby JSON


    {
      "Version": 1.0,
      "Sources": [
        {
          "Streams": [],
          "Filters": {
            "Crop": {
                "X": 240,
                "Y": 0,
                "Width": 1440,
                "Height": 1080
            }
          },
          "Pad": true
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 3400,
              "MaxBitrate": 3400,
              "BufferWindow": "00:00:05",
              "Width": 1440,
              "Height": 1080,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 2250,
              "MaxBitrate": 2250,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 720,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1250,
              "MaxBitrate": 1250,
              "BufferWindow": "00:00:05",
              "Width": 480,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


##<a name="restrictions-on-cropping"></a>Omezení pro oříznutí

Funkce oříznutí má být ruční. Je zapotřebí načíst vstupní videa do vhodného nástroje pro úpravu, které můžete vybrat snímky potřebné, umístěte kurzor a zjistit pořadová čísla pro oříznutí obdélník, chcete-li zjistit předvolby kódování, který je nastaven pro tuto konkrétní videa atd. Tato funkce není určen k povolení například: Automatické rozpoznávání a odebrání ohraničení černé letterbox/pillarbox v zadaných videa.

Následující omezení platí pro funkci oříznutí. Pokud nejsou splněné tyto, kódovat úkolu můžete nezdaří, nebo vytvořit neočekávané výstup.

1. Souřadnice a velikost obdélníku oříznutí mají tak, aby odpovídaly vstupní video
1. Výše uvedené, šířku a výšku v nastavení kódovat musí odpovídat oříznutá videa
1. Oříznutí platí pro videa zachytit v režimu na šířku (tedy netýká se nahrává pomocí smartphonu videa uskutečňuje svisle nebo v režimu na výšku)
1. Funguje nejlíp u postupné video zachyceny s Čtvereček pixelů

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="next-step"></a>Další krok
 
V tématu Azure Media Services naučné stezky týkající se seznámíte s funkcemi skvělé nabízená AMS.  

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]
