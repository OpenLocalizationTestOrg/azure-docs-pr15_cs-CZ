<properties 
    pageTitle="Rozšířené možnosti kódování s Media Encoder standardní" 
    description="Toto téma ukazuje, jak provádět pokročilé kódování přizpůsobením Media Encoder standardní předvolby úkolu. Téma ukazuje, jak používat Media Services .NET SDK k vytvoření kódování úkolu a projektu. Také ukazuje, jak zadat vlastní předvolby kódování úlohu." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/25/2016"   
    ms.author="juliako"/>


#<a name="advanced-encoding-with-media-encoder-standard"></a>Rozšířené možnosti kódování s Media Encoder standardní

##<a name="overview"></a>Základní informace

Toto téma ukazuje, jak můžou provádět pokročilé kódování s Media Encoder standardní. Téma ukazuje, [jak používat .NET vytvářet kódování úkolu a úlohy, která se spouští tohoto úkolu](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet). Také ukazuje, jak zadat vlastní předvolby kódování úkolu. Popis prvků, které využívají předvoleb najdete v tématu [tohoto dokumentu](https://msdn.microsoft.com/library/mt269962.aspx). 

Jsou prokázáno vlastní přednastavení provádějící kódování úkoly podle těchto pokynů:

- [Generování miniatur](media-services-custom-mes-presets-with-dotnet.md#thumbnails)
- [Střih videa (omezení)](media-services-custom-mes-presets-with-dotnet.md#trim_video)
- [Vytvoření překrytí](media-services-custom-mes-presets-with-dotnet.md#overlay)
- [Vložení pasivní zvuku sledování při zadávání neobsahuje žádný zvuk](media-services-custom-mes-presets-with-dotnet.md#silent_audio)
- [Zakázání automatického zrušte prokládání](media-services-custom-mes-presets-with-dotnet.md#deinterlacing)
- [Jenom hlasový přenos předvolby](media-services-custom-mes-presets-with-dotnet.md#audio_only)
- [Zřetězení dvou nebo více videosouborů](media-services-custom-mes-presets-with-dotnet.md#concatenate)
- [Oříznutí videa s Media Encoder standardní](media-services-custom-mes-presets-with-dotnet.md#crop)
- [Vložení videa sledování bez videa po zadání vstupních hodnot](media-services-custom-mes-presets-with-dotnet.md#no_video)
- [Otočení videa](media-services-custom-mes-presets-with-dotnet.md#rotate_video)

##<a id="encoding_with_dotnet"></a>Kódování s Media Services .NET SDK

Následující příklad využívá Media Services .NET SDK provádět následující úkoly:

- Vytvoření kódování úlohy.
- Získáte odkaz na Media Encoder standardní encoder.
- Načtení vlastní data XML nebo přednastavené JSON. Uložení souboru XML nebo JSON (například [XML](media-services-custom-mes-presets-with-dotnet.md#xml) nebo [JSON](media-services-custom-mes-presets-with-dotnet.md#json) v souboru a použití následující kód k načtení souboru.

        // Load the XML (or JSON) from the local file.
        string configuration = File.ReadAllText(fileName);  
- Kódování úkol přidáte do projektu. 
- Zadejte vstupní aktiva zakódovat.
- Vytvoření aktivum výstup obsahující zakódovaný materiálů.
- Přidání obslužné rutiny události kontroly průběhu projektu.
- Odeslání projektu.
    
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Net;
        using System.Security.Cryptography;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Newtonsoft.Json.Linq;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        using System.Web;
        using System.Globalization;
        
        namespace CustomizeMESPresests
        {
            class Program
            {
                // Read values from the App.config file.
                private static readonly string _mediaServicesAccountName =
                    ConfigurationManager.AppSettings["MediaServicesAccountName"];
                private static readonly string _mediaServicesAccountKey =
                    ConfigurationManager.AppSettings["MediaServicesAccountKey"];
        
                // Field for service context.
                private static CloudMediaContext _context = null;
                private static MediaServicesCredentials _cachedCredentials = null;
        
                private static readonly string _mediaFiles =
                    Path.GetFullPath(@"../..\Media");
        
                private static readonly string _singleMP4File =
                    Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");
        
                static void Main(string[] args)
                {
                    // Create and cache the Media Services credentials in a static class variable.
                    _cachedCredentials = new MediaServicesCredentials(
                                    _mediaServicesAccountName,
                                    _mediaServicesAccountKey);
                    // Used the chached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
        
                    // Get an uploaded asset.
                    var asset = _context.Assets.FirstOrDefault();
        
                    // Encode and generate the output using custom presets.
                    EncodeToAdaptiveBitrateMP4Set(asset);
        
                    Console.ReadLine();
                }
        
                static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("Media Encoder Standard Job");
                    // Get a media processor reference, and pass to it the name of the 
                    // processor to use for the specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
                
        
                    // Load the XML (or JSON) from the local file.
                    string configuration = File.ReadAllText("CustomPreset_JSON.json");
                
                    // Create a task
                    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
                        processor,
                        configuration,
                        TaskOptions.None);
                
                    // Specify the input asset to be encoded.
                    task.InputAssets.Add(asset);
                    // Add an output asset to contain the results of the job. 
                    // This output is specified as AssetCreationOptions.None, which 
                    // means the output asset is not encrypted. 
                    task.OutputAssets.AddNew("Output asset",
                        AssetCreationOptions.None);
                
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
                
                    return job.OutputMediaAssets[0];
                }
        
                static public IAsset UploadMediaFilesFromFolder(string folderPath)
                {
                    IAsset asset = _context.Assets.CreateFromFolder(folderPath, AssetCreationOptions.None);
        
                    foreach (var af in asset.AssetFiles)
                    {
                        // The following code assumes 
                        // you have an input folder with one MP4 and one overlay image file.
                        if (af.Name.Contains(".mp4"))
                            af.IsPrimary = true;
                        else
                            af.IsPrimary = false;
        
                        af.Update();
                    }
        
                    return asset;
                }
        
        
                static public IAsset EncodeWithOverlay(IAsset assetSource, string customPresetFileName)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("Media Encoder Standard Job");
                    // Get a media processor reference, and pass to it the name of the 
                    // processor to use for the specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
        
                    // Load the XML (or JSON) from the local file.
                    string configuration = File.ReadAllText(customPresetFileName);
        
                    // Create a task
                    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input assets to be encoded.
                    // This asset contains a source file and an overlay file.
                    task.InputAssets.Add(assetSource);
        
                    // Add an output asset to contain the results of the job. 
                    task.OutputAssets.AddNew("Output asset",
                        AssetCreationOptions.None);
        
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
        
                    return job.OutputMediaAssets[0];
                }
        

                private static void JobStateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine("Job state changed event:");
                    Console.WriteLine("  Previous state: " + e.PreviousState);
                    Console.WriteLine("  Current state: " + e.CurrentState);
                    switch (e.CurrentState)
                    {
                        case JobState.Finished:
                            Console.WriteLine();
                            Console.WriteLine("Job is finished. Please wait while local tasks or downloads complete...");
                            break;
                        case JobState.Canceling:
                        case JobState.Queued:
                        case JobState.Scheduled:
                        case JobState.Processing:
                            Console.WriteLine("Please wait...\n");
                            break;
                        case JobState.Canceled:
                        case JobState.Error:
        
                            // Cast sender as a job.
                            IJob job = (IJob)sender;
        
                            // Display or log error details as needed.
                            break;
                        default:
                            break;
                    }
                }
        
        
                private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
                {
                    var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
                    if (processor == null)
                        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
                    return processor;
                }
        
            }
        }


##<a name="support-for-relative-sizes"></a>Podpora pro relativní velikosti

Při generování miniatury ho nepotřebujete vždy zadat výstup šířku a výšku v pixelech. Můžete je zadat v procentech v oblasti [1 %,..., 100 %].

###<a name="json-preset"></a>Předvolby JSON 
    
    "Width": "100%",
    "Height": "100%"

###<a name="xml-preset"></a>Předvolby XML
    
    <Width>100%</Width>
    <Height>100%</Height>
    
##<a id="thumbnails"></a>Generování miniatur

Tato část popisuje přizpůsobit přednastavený generovaný miniatury. Předvolby níže uvedených obsahuje informace o způsobu kódovat váš soubor stejně jako informace potřebné pro generování miniatury. Můžete provést některou z MES předvolby dokumentovaného [tady](https://msdn.microsoft.com/library/mt269960.aspx) a přidat kód generovaný miniatury.  

>[AZURE.NOTE]Nastavení **SceneChangeDetection** pouze následující přednastavené lze nastavit na hodnotu true, pokud jsou kódování do jednoho bitrate videa. Pokud jsou kódování videa s víc bitrate a nastavit **SceneChangeDetection** true (pravda), encoder vrátí chybu.  


Informace o schématu najdete v tématu [v tomto](https://msdn.microsoft.com/library/mt269962.aspx) tématu.

Zkontrolujte, přečtěte si část [Co byste měli zvážit](media-services-custom-mes-presets-with-dotnet.md#considerations) .

###<a id="json"></a>Předvolby JSON


    {
      "Version": 1.0,
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": "true",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 4500,
              "MaxBitrate": 4500,
              "BufferWindow": "00:00:05",
              "Width": 1280,
              "Height": 720,
              "ReferenceFrames": 3,
              "EntropyMode": "Cabac",
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
       
            }
          ],
          "Type": "H264Video"
        },
        {
          "JpgLayers": [
            {
              "Quality": 90,
              "Type": "JpgLayer",
              "Width": 640,
              "Height": 360
            }
          ],
          "Start": "{Best}",
          "Type": "JpgImage"
        },
        {
          "PngLayers": [
            {
              "Type": "PngLayer",
              "Width": 640,
              "Height": 360,
            }
          ],
          "Start": "00:00:01",
          "Step": "00:00:10",
          "Range": "00:00:58",
          "Type": "PngImage"
        },
        {
          "BmpLayers": [
            {
              "Type": "BmpLayer",
              "Width": 640,
              "Height": 360
            }
          ],
          "Start": "10%",
          "Step": "10%",
          "Range": "90%",
          "Type": "BmpImage"
        },
        {
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "JpgFormat"
          }
        },
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "PngFormat"
          }
        },
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "BmpFormat"
          }
        },
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


###<a id="xml"></a>Předvolby XML


    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <SceneChangeDetection>true</SceneChangeDetection>
          <H264Layers>
            <H264Layer>
              <Bitrate>4500</Bitrate>
              <Width>1280</Width>
              <Height>720</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>4500</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <AACAudio>
          <Profile>AACLC</Profile>
          <Channels>2</Channels>
          <SamplingRate>48000</SamplingRate>
          <Bitrate>128</Bitrate>
        </AACAudio>
        <JpgImage Start="{Best}">
          <JpgLayers>
            <JpgLayer>
              <Width>640</Width>
              <Height>360</Height>
              <Quality>90</Quality>
            </JpgLayer>
          </JpgLayers>
        </JpgImage>
        <BmpImage Start="10%" Step="10%" Range="90%">
          <BmpLayers>
            <BmpLayer>
              <Width>640</Width>
              <Height>360</Height>
            </BmpLayer>
          </BmpLayers>
        </BmpImage>
        <PngImage Start="00:00:01" Step="00:00:10" Range="00:00:58">
          <PngLayers>
            <PngLayer>
              <Width>640</Width>
              <Height>360</Height>
            </PngLayer>
          </PngLayers>
        </PngImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <JpgFormat />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <BmpFormat />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <PngFormat />
        </Output>
      </Outputs>
    </Preset>

###<a name="considerations"></a>Co byste měli zvážit

Platí následující omezení:

- Používání rámců explicitní časová razítka na začátek/krok/oblast předpokládá, že vstupní zdroj aspoň jednu minutu.
- Prvky ve formátu JPG, Png/BmpImage spuštění, krok a oblasti atributy řetězce – to může být interpretován jako:

    - Snímek číslo, pokud jsou nezáporné celých čísel, třeba "Začínáme": "120"
    - Vzhledem k zdroje dobu trvání, pokud vyjádřený jako % dat, například "Start": "15 %"; nebo
    - Časové razítko Pokud vyjádřený hh... formátování, například "Začínáme": "00: 01:00"

    Můžete kombinovat a prosím podle formátu: při.
    
    Navíc Start taky podporuje speciální makra: {vhodné}, která pokouší určit první "zajímavé" snímek obsahu poznámky: (krok a oblasti ignorovány při počáteční nastavení {nejlepší})
    
    - Výchozí: Zahájení: {nejlepší}
- Výstupní formát je nutno explicitně pro každý powerpointovém: Jpg, Png/BmpFormat. Pokud jsou k dispozici, odpovídá MES JpgVideo JpgFormat a tak dál. OutputFormat představuje vytvořte nové makro konkrétní kodek obrázků: {Index}, která musí být prezentovat (jednou a pouze jednou) pro obrázek výstupních formátů.

##<a id="trim_video"></a>Střih videa (omezení)

Tato část pojednává o změně předvoleb encoder klipu nebo zadávání videa, kde je vstupní takzvanou mezzanine souboru nebo souboru na vyžádání. Encoder lze také k oříznutí nebo střih materiálů, které se nezaznamenávají, nebo archivují z živého toku – podrobnosti týkající se to jsou dostupné v [tomto blogu](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).

Pokud chcete střih videa, můžete provést některou z MES předvolby dokumentovaného [tady](https://msdn.microsoft.com/library/mt269960.aspx) a upravte element **zdrojů** (viz níže). Hodnota čas spuštění musí odpovídat absolutní časová razítka vstupní videa. Například pokud prvního snímku vstupní video má časové razítko 12:00:10.000, pak čas spuštění by měl být aspoň 12:00:10.000 a vyšší. V následujícím příkladu jsme Předpokládejme, že vstupní video obsahuje časové razítko počáteční nuly. **Zdroje** by měl být umístěny na začátku předvolby. 
 
###<a id="json"></a>Předvolby JSON
    
    {
      "Version": 1.0,
      "Sources": [
        {
          "StartTime": "00:00:04",
          "Duration": "00:00:16"
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 3400,
              "MaxBitrate": 3400,
              "BufferWindow": "00:00:05",
              "Width": 1280,
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
              "Bitrate": 2250,
              "MaxBitrate": 2250,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 540,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1500,
              "MaxBitrate": 1500,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 540,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1000,
              "MaxBitrate": 1000,
              "BufferWindow": "00:00:05",
              "Width": 640,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 650,
              "MaxBitrate": 650,
              "BufferWindow": "00:00:05",
              "Width": 640,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 400,
              "MaxBitrate": 400,
              "BufferWindow": "00:00:05",
              "Width": 320,
              "Height": 180,
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

###<a name="xml-preset"></a>Předvolby XML
    
Pokud chcete střih videa, můžete provést některou z MES předvolby dokumentovaného [tady](https://msdn.microsoft.com/library/mt269960.aspx) a upravte element **zdrojů** (viz níže).

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Sources>
        <Source StartTime="PT4S" Duration="PT14S"/>
      </Sources>
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <H264Layers>
            <H264Layer>
              <Bitrate>3400</Bitrate>
              <Width>1280</Width>
              <Height>720</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>3400</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>2250</Bitrate>
              <Width>960</Width>
              <Height>540</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>2250</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>1500</Bitrate>
              <Width>960</Width>
              <Height>540</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1500</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>1000</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1000</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>650</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>650</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>400</Bitrate>
              <Width>320</Width>
              <Height>180</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>400</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <AACAudio>
          <Profile>AACLC</Profile>
          <Channels>2</Channels>
          <SamplingRate>48000</SamplingRate>
          <Bitrate>128</Bitrate>
        </AACAudio>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
      </Outputs>
    </Preset>

##<a id="overlay"></a>Vytvoření překrytí

Standardní Media Encoder vám umožní překryvném obrázek do existujícího videa. V současné době podporuje následující formáty: png, jpg, gif a bmp. Předvolba definované níže je příklad základní překryvu videa.

Kromě definování přednastavené souboru, máte taky chcete, aby Media Services vědět, souborů, které v majetek je obrázek překryvném souborů, které je zdrojem videa do kterého chcete překryvný obrázek. Videosoubor musí být **primárním** souboru. 

Výše uvedený příklad .NET definuje dvou funkcí: **UploadMediaFilesFromFolder** a **EncodeWithOverlay**. Funkce UploadMediaFilesFromFolder odešle soubory ve složce (například BigBuckBunny.mp4 a Image001.png) a nastaví mp4 souboru jako primární soubor v majetek. Funkce **EncodeWithOverlay** , která používá vlastní přednastavené soubor, který byl předané (například položku předvolba, který následuje) kódování úkol vytvoříte. 

>[AZURE.NOTE]Aktuální omezení:
>
>Nastavení neprůhlednosti překryvném nepodporuje.
>
>Zdrojový soubor videa souboru s obrázkem překryvném musí být ve stejném materiálů a videosoubor je třeba nastavit jako primární soubor v tomto materiálů.

###<a name="json-preset"></a>Předvolby JSON
    
    {
      "Version": 1.0,
      "Sources": [
        {
          "Streams": [],
          "Filters": {
            "VideoOverlay": {
              "Position": {
                "X": 100,
                "Y": 100,
                "Width": 100,
                "Height": 50
              },
              "AudioGainLevel": 0.0,
              "MediaParams": [
                {
                  "OverlayLoopCount": 1
                },
                {
                  "IsOverlay": true,
                  "OverlayLoopCount": 1,
                  "InputLoop": true
                }
              ],
              "Source": "Image001.png",
              "Clip": {
                "Duration": "00:00:05"
              },
              "FadeInDuration": {
                "Duration": "00:00:01"
              },
              "FadeOutDuration": {
                "StartTime": "00:00:03",
                "Duration": "00:00:04"
              }
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
              "Bitrate": 1045,
              "MaxBitrate": 1045,
              "BufferWindow": "00:00:05",
              "ReferenceFrames": 3,
              "EntropyMode": "Cavlc",
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "Width": "640",
              "Height": "360",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Type": "CopyAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}{Extension}",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


###<a name="xml-preset"></a>Předvolby XML
    
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Sources>
        <Source>
          <Streams />
          <Filters>
            <VideoOverlay>
              <Source>Image001.png</Source>
              <Clip Duration="PT5S" />
              <FadeInDuration Duration="PT1S" />
              <FadeOutDuration StartTime="PT3S" Duration="PT4S" />
              <Position X="100" Y="100" Width="100" Height="50" />
              <Opacity>0</Opacity>
              <AudioGainLevel>0</AudioGainLevel>
              <MediaParams>
                <MediaParam>
                  <IsOverlay>false</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
                  <InputLoop>false</InputLoop>
                </MediaParam>
                <MediaParam>
                  <IsOverlay>true</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
                  <InputLoop>true</InputLoop>
                </MediaParam>
              </MediaParams>
            </VideoOverlay>
          </Filters>
          <Pad>true</Pad>
        </Source>
      </Sources>
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <H264Layers>
            <H264Layer>
              <Bitrate>1045</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>0</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cavlc</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1045</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <CopyAudio />
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}{Extension}">
          <MP4Format />
        </Output>
      </Outputs>
    </Preset>


##<a id="silent_audio"></a>Vložení pasivní zvuku sledování při zadávání neobsahuje žádný zvuk

Ve výchozím nastavení při odesílání předávat na vstupu encoder, který obsahuje pouze video a zvuk, pak materiálů výstup obsahuje soubory, které obsahují data jenom videa. Některé přehrávače nepůjdou zpracovávání těchto proudů výstupu. Toto nastavení slouží k vynucení encoder přidáte pasivní zvuku sledování do výstupu v tomto scénáři.

Chcete-li vynutit encoder k vytvoření aktiva obsahující pasivní zvuku sledování při zadávání neobsahuje žádný zvuk, zadejte hodnotu "InsertSilenceIfNoAudio".

Můžete provést některou z MES předvolby dokumentovaného [tady](https://msdn.microsoft.com/library/mt269960.aspx)a provést následující změny:

###<a name="json-preset"></a>Předvolby JSON

    {
      "Channels": 2,
      "SamplingRate": 44100,
      "Bitrate": 96,
      "Type": "AACAudio",
      "Condition": "InsertSilenceIfNoAudio"
    }

###<a name="xml-preset"></a>Předvolby XML

    <AACAudio Condition="InsertSilenceIfNoAudio">
      <Channels>2</Channels>
      <SamplingRate>44100</SamplingRate>
      <Bitrate>96</Bitrate>
    </AACAudio>

##<a id="deinterlacing"></a>Zakázání automatického zrušte prokládání

Zákazníci nemusíte nic dělat, když jako prokládání obsah je automaticky zrušte prokládaný. Po prokládání zrušit automatické na (výchozí nastavení) MES znamená automatické rozpoznávání prokládaný snímky a pouze zrušte interlaces snímky označený jako prokládaný.

Můžete vypnout prokládání zrušit automatické. Tato možnost se nedoporučuje.

###<a name="json-preset"></a>Předvolby JSON
    
    "Sources": [
    {
     "Filters": {
        "Deinterlace": {
          "Mode": "Off"
        }
      },
    }
    ]

###<a name="xml-preset"></a>Předvolby XML
    
    <Sources>
    <Source>
      <Filters>
        <Deinterlace>
          <Mode>Off</Mode>
        </Deinterlace>
      </Filters>
    </Source>
    </Sources>


##<a id="audio_only"></a>Jenom hlasový přenos předvolby

Tato část ukazuje dvou jenom hlasový přenos MES přednastavení: AAC zvuk a AAC dobré kvality zvuku.

###<a name="aac-audio"></a>AAC zvuku 

    {
      "Version": 1.0,
      "Codecs": [
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
          "FileName": "{Basename}_AAC_{AudioBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

###<a name="aac-good-quality-audio"></a>AAC dobré kvality zvuku

    {
      "Version": 1.0,
      "Codecs": [
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 192,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_AAC_{AudioBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

##<a id="concatenate"></a>Zřetězení dvou nebo více videosouborů

Následující příklad ukazuje, jak si můžete vygenerovat přednastavení ke zřetězení dvou nebo více videosoubory. Nejběžnější scénář je, když chcete přidat záhlaví nebo přívěsu hlavní video. Zamýšlené použití je, když videosoubory upravovaný společně sdílet vlastnosti (rozlišení snímků, zvukových sledování počtu, atd.). By měl dávejte pozor, abyste kombinovat videa sazeb jiného snímku, nebo společně s rozdílný počet zvukových stop.

###<a name="requirements-and-considerations"></a>Požadavky a co byste měli zvážit

- Zadávání videa, má jenom jedno zvukové sledování.
- Při zadávání videa by měl mít stejný rámec kurz.
- Musíte nahrání videa do samostatných prostředky a nastavit videí jako primární soubor v jednotlivých materiálů.
- Je potřeba vědět trvání videa.
- Přednastavené příklady dole předpokládá, zadávání videa začínáte s časovým razítkem nula. Potřebujete změnit hodnoty čas spuštění máte videí jiné výchozí časové razítko, stejně jako obvykle v případě s živou archivy.
- Předvolby JSON zajišťuje explicitní odkazy na hodnoty Id_položky vstupní prostředky.
- Ukázkový kód předpokládá přednastavení JSON uložil do místního souboru, například "C:\supportFiles\preset.json". Také předpokládá, že dva prostředky vytvořil nahrávání dvou videosoubory a znát výsledné hodnoty Id_položky.
- Fragment kódu a JSON přednastavení příklad zřetězení dvou videosoubory. Můžete ho rozšířit do více než dvě videa tak, že:

    1. Volání úkolu. InputAssets.Add() opakovaně přidáte další videa v pořadí.
    2. Díky odpovídající úpravy element "Zdrojů" JSON, přidáním dalších položek ve stejném pořadí. 


###<a name="net-code"></a>Kód .NET

    
    IAsset asset1 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:606db602-efd7-4436-97b4-c0b867ba195b").FirstOrDefault();
    IAsset asset2 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:a7e2b90f-0565-4a94-87fe-0a9fa07b9c7e").FirstOrDefault();
    
    // Declare a new job.
    IJob job = _context.Jobs.Create("Media Encoder Standard Job for Concatenating Videos");
    // Get a media processor reference, and pass to it the name of the 
    // processor to use for the specific task.
    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
    
    // Load the XML (or JSON) from the local file.
    string configuration = File.ReadAllText(@"c:\supportFiles\preset.json");
    
    // Create a task
    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
        processor,
        configuration,
        TaskOptions.None);
    
    // Specify the input videos to be concatenated (in order).
    task.InputAssets.Add(asset1);
    task.InputAssets.Add(asset2);
    // Add an output asset to contain the results of the job. 
    // This output is specified as AssetCreationOptions.None, which 
    // means the output asset is not encrypted. 
    task.OutputAssets.AddNew("Output asset",
        AssetCreationOptions.None);
    
    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
    job.Submit();
    job.GetExecutionProgressTask(CancellationToken.None).Wait();

###<a name="json-preset"></a>Předvolby JSON

Aktualizujte svůj vlastní přednastavení s ID prostředky, které chcete concatenate a odpovídající časového úseku pro každé video.

    {
      "Version": 1.0,
      "Sources": [
        {
          "AssetID": "606db602-efd7-4436-97b4-c0b867ba195b",
          "StartTime": "00:00:01",
          "Duration": "00:00:15"
        },
        {
          "AssetID": "a7e2b90f-0565-4a94-87fe-0a9fa07b9c7e",
          "StartTime": "00:00:02",
          "Duration": "00:00:05"
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": true,
          "H264Layers": [
            {
              "Level": "auto",
              "Bitrate": 1800,
              "MaxBitrate": 1800,
              "BufferWindow": "00:00:05",
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "Width": "640",
              "Height": "360",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
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

##<a id="crop"></a>Oříznutí videa s Media Encoder standardní

Přečtěte si téma [oříznutí videa s Media Encoder standardní](media-services-crop-video.md) .

##<a id="no_video"></a>Vložení videa sledování bez videa po zadání vstupních hodnot

Ve výchozím nastavení při odesílání předávat na vstupu encoder, který obsahuje pouze, bez audiovizuálnímu přenosu, pak výstup majetek obsahuje soubory, které obsahují data jenom zvuku. Některé přehrávače, včetně Azure Media Playeru (viz [Tento](https://feedback.azure.com/forums/169396-azure-media-services/suggestions/8082468-audio-only-scenarios)) nemusí být moct dělat tyto datových proudů. Toto nastavení slouží k vynucení encoder přidáte černobílé stopa videa do výstupu v tomto scénáři. 

>[AZURE.NOTE]Vynucení encoder vložit video sledování výstup zvětšuje velikost výstup materiálů, a tím náklady vzniklé v souvislosti s kódování úkolu. By měla běžet testy a ověřte, že toto výsledné zvýšení má jenom menší dopad na měsíční poplatky.

### <a name="inserting-video-at-only-the-lowest-bitrate"></a>Vložení videa na pouze nejnižší přenosová

Předpokládejme, že používáte v několika bitrate kódování přednastavené jako ["H264 více Bitrate 720p"](https://msdn.microsoft.com/library/mt269960.aspx) kódování celý vstupní katalogu datového proudu, která obsahuje kombinaci jenom hlasový přenos souborů a videosouborů. V tomto scénáři po zadání bez videa, je vhodné vynutit encoder vložte černobílé stopa videa na nejnižší přenosová namísto vložení videa na každé bitrate výstupu. Docílit, budete muset zadat příznak "InsertBlackIfNoVideoBottomLayerOnly".

Můžete provést některou z MES předvolby dokumentovaného [tady](https://msdn.microsoft.com/library/mt269960.aspx)a provést následující změny:

#### <a name="json-preset"></a>Předvolby JSON

    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideoBottomLayerOnly",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a>Předvolby XML

    <KeyFrameInterval>00:00:02</KeyFrameInterval>
    <StretchMode>AutoSize</StretchMode>
    <Condition>InsertBlackIfNoVideoBottomLayerOnly</Condition>

### <a name="inserting-video-at-all-output-bitrates"></a>Vložení videa ve všech výstupních bitrates

Předpokládejme, že používáte v několika bitrate kódování přednastavené jako ["H264 více Bitrate 720p](https://msdn.microsoft.com/library/mt269960.aspx) kódování celý vstupní katalogu datového proudu, která obsahuje kombinaci jenom hlasový přenos souborů a videosouborů. V tomto scénáři po zadání bez videa, je vhodné vynutit encoder vložit černobílé videa sledování vůbec bitrates výstupu. Zajistíte tak, že výstup prostředky jsou všechny jednotné týkající počet videa skladeb a zvukových stop. Docílit, budete muset zadat příznaku "InsertBlackIfNoVideo".

Můžete provést některou z MES předvolby dokumentovaného [tady](https://msdn.microsoft.com/library/mt269960.aspx)a provést následující změny:

#### <a name="json-preset"></a>Předvolby JSON

    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideo",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a>Předvolby XML
    
    <KeyFrameInterval>00:00:02</KeyFrameInterval>
    <StretchMode>AutoSize</StretchMode>
    <Condition>InsertBlackIfNoVideo</Condition>

##<a id="rotate_video"></a>Otočení videa

[Media Encoder standardní](media-services-dotnet-encode-with-media-encoder-standard.md) podporuje otočení tak, že úhlu 0/90/180/270 stupňů. Výchozí chování je "Automaticky", kde pokusí ke zjištění metadata otočení příchozí videosoubor a nahradit ho. Obsahují následující prvek **zdroje** na jeden z předvolby definovaný [tady](http://msdn.microsoft.com/library/azure/mt269960.aspx):

### <a name="json-preset"></a>Předvolby JSON

    "Sources": [
    {
      "Streams": [],
      "Filters": {
        "Rotation": "90"
      }
    }
    ],
    "Codecs": [
    
    ...
### <a name="xml-preset"></a>Předvolby XML

    <Sources>
        <Source>
          <Streams />
          <Filters>
            <Rotation>90</Rotation>
          </Filters>
        </Source>
    </Sources>

Další informace naleznete [v tomto](https://msdn.microsoft.com/library/azure/mt269962.aspx#PreserveResolutionAfterRotation) tématu Další informace na interpretace nastavení šířku a výšku v předvolby, encoder při otáčení vyrovnání.

Chcete-li encoder přeskočit metadata otočení v prostoru, pokud je k dispozici ve vstupním videa můžete použít hodnotu "0". 


##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Viz taky 

[Mediální služby základní kódování informace](media-services-encode-asset.md)
