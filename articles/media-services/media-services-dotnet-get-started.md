<properties
    pageTitle="Začínáme s doručení obsahu na vyžádání pomocí .NET | Azure"
    description="Tento kurz vás provede kroky provádění aplikace pro doručování obsahu na služba s Azure Media Services pomocí .NET."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/17/2016"
    ms.author="juliako"/>


# <a name="get-started-with-delivering-content-on-demand-using-net-sdk"></a>Začínáme s doručení obsahu na vyžádání pomocí .NET SDK

[AZURE.INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

>[AZURE.NOTE]
> Pro dokončení tohoto kurzu, třeba účet Azure. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](/pricing/free-trial/?WT.mc_id=A261C142F). 
 
##<a name="overview"></a>Základní informace 

Tento kurz vás provede jednotlivými kroky provádění aplikace doručování obsahu poskytují (VoD) pomocí Azure Media Services (AMS) SDK pro .NET.


Kurz představuje základní pracovního postupu Media Services a nejběžnější programového objekty a úkoly potřebné pro vývoj Media Services. Po dokončení kurzu budou moct můžete vysílat datovými proudy nebo postupně stáhnout ukázkový soubor médií nahráli, kódování a stáhnout.

## <a name="what-youll-learn"></a>Co se dozvíte

Kurz ukazuje, jak provádět následující úkoly:

1.  Vytvoření účtu Media Services (na portálu Azure).
2.  Konfigurace streamování koncového bodu (na portálu Azure).
3.  Vytváření a konfigurace aplikace Visual Studio projektu.
5.  Připojení k účtu Media Services.
6.  Vytvořte nový majetek a nahrajte soubor videa.
7.  Kódování zdrojového souboru do sadu adaptivní bitrate MP4 souborů.
8.  Publikujte majetek a získání adresy URL pro streamování a postupné stáhnout.
9.  Otestujte přehrávání obsahu.

## <a name="prerequisites"></a>Zjistit předpoklady pro

Toto jsou potřebná k dokončení kurzu.

- Pro dokončení tohoto kurzu, třeba účet Azure. 
    
    Pokud nemáte účet, můžete vytvořit bezplatný účet zkušební v jenom pár minut. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](/pricing/free-trial/?WT.mc_id=A261C142F). Potřebujete přeplatky, které lze použít k vyzkoušení placené služby Azure. I když přeplatky používají, můžete mít účet a pomocí bezplatné služby Azure a funkce, například s funkcí webových aplikací Web Apps v aplikaci služby Azure.
- Operační systémy: Windows 8 nebo novější, Windows 2008 R2, Windows 7.
- .NET framework 4.0 nebo novější
- Visual Studio 2010 SP1 (Professional, Premium, Ultimate nebo Express) nebo novější verze.


##<a name="download-sample"></a>Stáhnout ukázkový

Získání a spusťte výběru z [tady](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).

## <a name="create-an-azure-media-services-account-using-the-azure-portal"></a>Vytvořte účet Azure Media Services pomocí portálu Azure

Postup v této části ukazují, jak vytvořit účet AMS.

1. Přihlaste se na [portál Azure](https://portal.azure.com/).
2. Klikněte na **+ Nový** > **médií + CDN** > **Media Services**.

    ![Vytvoření Media Services](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. **Vytvoření** účtu služby médií zadejte požadované hodnoty.

    ![Vytvoření Media Services](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. Do pole **Název účtu**zadejte název nového účtu AMS. Název účtu Media Services je všechna malá písmena číslic nebo písmen bez mezer a 3 až 24 znaků.
    2. V předplatného vyberte mezi různých Azure předplatné, které máte přístup.
    
    2. **Pole Skupina zdroje**vyberte zdroj nové nebo existující.  Skupina zdroje je kolekce zdroje, které mají životního cyklu, oprávnění a zásady. Další informace [v tomto poli](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. Do pole **umístění**vyberte zeměpisná oblast slouží k uložení záznamy média a metadat pro váš účet Media Services. Tato oblast slouží k obrázku a vysílání datových proudů. Do pole rozevíracího seznamu se zobrazí jenom k dispozici oblasti Media Services. 
    
    3. **Úložiště účtu**vyberte účet úložiště k poskytování úložiště objektů blob obsahu ze svého účtu Media Services. Můžete vybrat existující účet úložiště ve stejné zeměpisná oblast účtem Media Services nebo můžete vytvořit účet úložiště. Vytvoření nového účtu úložiště ve stejné oblasti. Pravidla pro názvy účtů úložiště jsou stejné jako u účtů Media Services.

        Další informace o ukládání [tady](storage-introduction.md).

    4. Vyberte **Připnout do řídicího panelu** zobrazíte průběh nasazení účtu.
    
7. Klikněte na **vytvořit** v dolní části formuláře.

    Po úspěšném vytvoření účtu stav se změní na **systém**. 

    ![Nastavení Media Services](./media/media-services-portal-vod-get-started/media-services-settings.png)

    Správa účtu AMS (například nahrát videa, kódovat prostředky, sledovat pokrok projektu) pomocí okna **Nastavení** .

## <a name="configure-streaming-endpoints-using-the-azure-portal"></a>Konfigurace streamování koncové body pomocí portálu Azure

Při práci s Azure Media Services jednu obvyklé scénáře je poskytly videa přes adaptivní bitrate streamování klienty. Media Services podporuje následující adaptivní přenosová streamování technologie: HTTP Live datových proudů (HLS) hladký přenos, MPEG POMLČKU a konfigurace (pro Adobe PrimeTime/přístup nabyvatelů licence pouze).

Služba Media poskytuje dynamické balení, který umožňuje poskytovat adaptivní bitrate MP4 zakódovaný obsah v datových proudů formáty podporované kodérem Media Services (MPEG POMLČKU HLS, hladký přenos, konfigurace) těsně za běhu, aniž byste museli ukládat předem sbalené verze každého z nich streamování formáty.

Umožní využít výhod dynamické balení potřebujete postupujte takto:

- Kódování mezzanine (zdrojový) souboru do sadu adaptivní bitrate MP4 souborů (kódování kroky jsou prokázáno dál v tomto kurzu).  
- Vytvořte alespoň jeden streamování jednotku pro *streamování koncový bod* ze které budete chtít doručení obsahu. Postupem zobrazení, jak lze změnit počet datových proudů jednotky.

Dynamické balení, stačí k ukládání a zaplatit soubory ve formátu jedné úložiště a Media Services vytvoří a slouží odpovídající odpověď založená na žádosti o z klienta.

Vytvořit a změnit počet datových proudů rezervovaná jednotky, postupujte takto:


1. V okně **Nastavení** klikněte na **datových proudů koncové body**. 

2. Klikněte na výchozí streamování koncového bodu. 

    Zobrazí se okno **Výchozí STREAMOVÁNÍ podrobnosti o koncovém bodu** .

3. Chcete-li zadat počet jednotek, datových proudů, posuňte jezdec **datových proudů jednotky** .

    ![Přenos jednotky](./media/media-services-portal-vod-get-started/media-services-streaming-units.png)

4. Kliknutím na tlačítko **Uložit** uložte provedené změny.

    >[AZURE.NOTE]Přidělení nové jednotky může trvat až 20 minut.

##<a name="create-and-configure-a-visual-studio-project"></a>Vytváření a konfigurace projekt aplikace Visual Studio

1. Vytvořte novou aplikaci konzoly C# ve Visual Studiu 2013, Visual Studio 2012 nebo Visual Studio 2010 SP1. Zadejte **název**, **umístění**a **název řešení**a klikněte na **OK**.

2. Použil funkci balíček NuGet [windowsazure.mediaservices.extensions](https://www.nuget.org/packages/windowsazure.mediaservices.extensions) nainstalovat **Azure Media Services .NET SDK rozšíření**.  Rozšíření SDK Media Services .NET je sada rozšíření metod a pomocná funkce, které zjednoduší kódu a usnadňují se dají s Media Services. Při instalaci tohoto balíčku, také nainstaluje **Media Services.NET SDK** a přidá všechny ostatní požadované závislosti.

3. Přidání odkazu do System.Configuration sestavení. Sestavení obsahuje **System.Configuration.ConfigurationManager** třídy, která se používá pro přístup k souborům konfiguraci, třeba App.config.

4. Otevřete konfiguračního souboru (přidání souboru do projektu Pokud nepřidalo ve výchozím nastavení) a přidejte oddíl *appSettings* k souboru. Nastavení hodnot pro svůj Azure Media Services název a účet klíč účtu, jak je vidět v následujícím příkladu. Získat název účtu a klíčové informace, přejděte na [portál Azure](https://portal.azure.com/) a vyberte svůj účet AMS. Vyberte **Nastavení** > **klíče**. Spravovat klávesy windows se zobrazí název účtu a zobrazí se primární a sekundární klíče.

        <configuration>
        ...
          <appSettings>
            <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
            <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
          </appSettings>
          
        </configuration>

5. Přepsat existující **pomocí** příslušným na začátku souboru Program.cs následující kód.

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Text;
        using System.Threading.Tasks;
        using System.Configuration;
        using System.Threading;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;
        

6. Vytvoření nové složky v adresáři projekty a zkopírujte soubor .mp4 nebo soubor .wmv, který chcete-li zakódovat a můžete vysílat datovými proudy nebo postupné stahování. V tomto příkladu je použita cesta "C:\VideoFiles".

##<a name="connect-to-the-media-services-account"></a>Připojení k účtu Media Services

Při použití Media Services s .NET, musíte použít třídu **CloudMediaContext** pro většinu Media Services programování úkoly: připojení k účtu služby Media; Vytvoření, aktualizace, přístup a odstranění následující objekty: prostředky soubory materiálů, úlohy, zásady přístupu, Locator, atd.

Přepsat výchozí Program třídy následující kód. Kód ukazuje, jak číst hodnoty připojení ze konfiguračního souboru a jak vytvořit objekt **CloudMediaContext** budete moct připojit k Media Services. Další informace o připojování k Media Services naleznete v tématu [připojení k Media Services s Media Services SDK pro .NET](http://msdn.microsoft.com/library/azure/jj129571.aspx).

Funkce **hlavní** volá metody, které bude možné definovat dál v tomto oddílu.

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

        static void Main(string[] args)
        {
            try
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Used the chached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);

                // Add calls to methods defined in this section.

                IAsset inputAsset =
                    UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.None);

                IAsset encodedAsset =
                    EncodeToAdaptiveBitrateMP4s(inputAsset, AssetCreationOptions.None);

                PublishAssetGetURLs(encodedAsset);
            }
            catch (Exception exception)
            {
                // Parse the XML error message in the Media Services response and create a new
                // exception with its content.
                exception = MediaServicesExceptionParser.Parse(exception);

                Console.Error.WriteLine(exception.Message);
            }
            finally
            {
                Console.ReadLine();
            }
        }

##<a name="create-a-new-asset-and-upload-a-video-file"></a>Vytvořte nový majetek a nahrajte soubor videa

V Media Services můžete nahrát (nebo jedí) digitální souborů do aktivum. Entita **materiálů** může obsahovat video, zvuk, obrázky, miniatur kolekce, text sleduje a titulků soubory (a metadata o těchto souborech.)  Po nahrání souborů obsahu je bezpečně uložený v cloudu pro další zpracování a datových proudů. Soubory v majetek nazývají **Materiálů soubory**.

Metoda **UploadFile** definované pod volání **CreateFromFile** (podle .NET SDK přípony). **CreateFromFile** vytvoří nový materiálů, do kterého určený zdrojový soubor odeslat.

Metoda **CreateFromFile** jen **AssetCreationOptions** , který umožňuje zadat jednu z následujících možností vytváření materiálů:

- **Žádná** – bez šifrování se používá. Toto je výchozí hodnota. Všimněte si, že používáte-li tuto možnost, obsahu není zamčený při přenosu šifrovaná nebo u ostatních v úložišti.
Pokud budete chtít předvádění MP4 pomocí postupného stahování použijte tuto možnost.
- **StorageEncrypted** - použití zašifrovaných tuto možnost šifrování vymazat obsah místně pomocí šifrování AES (Advanced Standard)-256 šifrování, která odešle ji do úložiště Azure, kde je uložena v ostatních. Prostředky chráněné pomocí šifrování úložiště jsou automaticky bez šifrování umístí do systém zašifrovaných souborů před kódování a volitelně znovu zašifrovaných před odesláním zpět jako nový majetek výstupu. Primární použití případ pro šifrování úložiště je když budete chtít zabezpečené vysoce kvalitní vstupní mediální soubory s silné šifrování u ostatních na disku.
- **CommonEncryptionProtected** - použít tuto možnost, pokud ukládáte obsah, který už zašifrovaných a chráněny běžné šifrování nebo PlayReady DRM (například hladký přenos chráněné s PlayReady DRM).
- **EnvelopeEncryptionProtected** – použití tuto možnost, pokud ukládáte HLS zašifrovaných AES. Všimněte si, že soubory, které musí být zakódovaný a zašifrovaných transformace správce.

Metoda **CreateFromFile** také umožňuje zadat zpětné abyste mohli vykazovat průběh nahrát soubor.

V následujícím příkladu jsme určete **žádné** možnosti materiálů.

Přidejte následující metodu třídy Program.

    static public IAsset UploadFile(string fileName, AssetCreationOptions options)
    {
        IAsset inputAsset = _context.Assets.CreateFromFile(
            fileName,
            options,
            (af, p) =>
            {
                Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Asset {0} created.", inputAsset.Id);

        return inputAsset;
    }


##<a name="encode-the-source-file-into-a-set-of-adaptive-bitrate-mp4-files"></a>Kódování zdrojového souboru do sadu adaptivní bitrate MP4 souborů

Po ingesting prostředky do Media Services, může být médium s kódováním transmuxed vodotiskem a tak dále, než Doručená klientům. Tyto činnosti naplánované se spustí hledání v několika instancích spuštěných pozadí role ověřit vysoký výkon a dostupnosti. Tyto činnosti nazývají úlohy a jednotlivé úlohy se skládá z atomová úkoly, které se skutečnou práci se souborem materiálů.

Jak byla jsme zmínili dříve, při práci s Azure Media Services, jednu z obvyklé scénáře poskytuje adaptivní bitrate datových proudů klienty. Media Services můžete dynamicky balíček sadu adaptivní bitrate MP4 souborů do jedné z následujících formátů: HTTP Live datových proudů (HLS) hladký přenos, MPEG POMLČKU a konfigurace (pro Adobe PrimeTime/přístup nabyvatelů licence pouze).

Umožní využít výhod dynamické balení potřebujete postupujte takto:

- Kódování nebo program mezzanine (zdrojový) souborů do sady adaptivní bitrate MP4 soubory nebo Adaptivní bitrate hladký přenos souborů.  
- Potřebujete alespoň jeden streamování jednotku pro streamování koncový bod, ze kterého chcete doručení obsahu.

Následující kód ukazuje, jak můžou odeslat kódování projektu. Úkoly obsahuje jeden úkol, který určuje, že převod souboru mezzanine do sady adaptivní bitrate MP4s pomocí **Media Encoder standardní**. Kód odešle úlohy a čeká, dokud nebude dokončena.

Po dokončení projektu bude moct vaše materiálů můžete vysílat datovými proudy postupně soubory nebo stáhnout MP4, které jsou vytvořené důsledku převod kódování.
Všimněte si, že se nemusí být větší než 0 streamování jednotky, abychom postupně stahovat soubory MP4.

Přidejte následující metodu třídy Program.

    static public IAsset EncodeToAdaptiveBitrateMP4s(IAsset asset, AssetCreationOptions options)
    {
    
        // Prepare a job with a single task to transcode the specified asset
        // into a multi-bitrate asset.
    
        IJob job = _context.Jobs.CreateWithSingleTask(
            "Media Encoder Standard",
            "H264 Multiple Bitrate 720p",
            asset,
            "Adaptive Bitrate MP4",
            options);
    
        Console.WriteLine("Submitting transcoding job...");
    
    
        // Submit the job and wait until it is completed.
        job.Submit();
    
        job = job.StartExecutionProgressTask(
            j =>
            {
                Console.WriteLine("Job state: {0}", j.State);
                Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
            },
            CancellationToken.None).Result;
    
        Console.WriteLine("Transcoding job finished.");
    
        IAsset outputAsset = job.OutputMediaAssets[0];
    
        return outputAsset;
    }

##<a name="publish-the-asset-and-get-urls-for-streaming-and-progressive-download"></a>Publikování materiálů a získání adresy URL pro stažení streamování a postupné

Můžete vysílat datovými proudy nebo stahování aktivum, musíte nejdřív "publikovat" vytvořením locator. Locator poskytnutí přístupu k souborům obsažené v majetek. Media Services podporuje dva typy Locator: OnDemandOrigin Locator slouží k toku médií (například MPEG POMLČKU HLS a přitom zajistit hladký přenos) a Locator přidružení přístup podpisu (zabezpečení) slouží ke stažení mediální soubory.

Po vytvoření Locator je možné vytvářet adresy URL, které se používají můžete vysílat datovými proudy nebo stahování souborů.


Streamování adresy URL pro hladký přenos má v tomto formátu:

     {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Streamování adresy URL pro HLS má v tomto formátu:

     {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Streamování adresy URL pro MPEG POMLČKU má v tomto formátu:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

Adresa URL přidružení zabezpečení použít ke stahování souborů má v tomto formátu:

    {blob container name}/{asset name}/{file name}/{SAS signature}

Rozšíření Media Services .NET SDK poskytují pohodlný pomocné metody, jejichž výsledkem je formátovaný adresy URL pro publikované materiálů.

Následující kód používá .NET SDK rozšíření k vytvoření Locator a k získání streamování a postupné stahování adresy URL. Kód také ukazuje, jak stahovat soubory do místní složky.

Přidejte následující metodu třídy Program.

    static public void PublishAssetGetURLs(IAsset asset)
    {
        // Publish the output asset by creating an Origin locator for adaptive streaming,
        // and a SAS locator for progressive download.

        _context.Locators.Create(
            LocatorType.OnDemandOrigin,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));

        _context.Locators.Create(
            LocatorType.Sas,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));


        IEnumerable<IAssetFile> mp4AssetFiles = asset
                .AssetFiles
                .ToList()
                .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));

        // Get the Smooth Streaming, HLS and MPEG-DASH URLs for adaptive streaming,
        // and the Progressive Download URL.
        Uri smoothStreamingUri = asset.GetSmoothStreamingUri();
        Uri hlsUri = asset.GetHlsUri();
        Uri mpegDashUri = asset.GetMpegDashUri();

        // Get the URls for progressive download for each MP4 file that was generated as a result
        // of encoding.
        List<Uri> mp4ProgressiveDownloadUris = mp4AssetFiles.Select(af => af.GetSasUri()).ToList();


        // Display  the streaming URLs.
        Console.WriteLine("Use the following URLs for adaptive streaming: ");
        Console.WriteLine(smoothStreamingUri);
        Console.WriteLine(hlsUri);
        Console.WriteLine(mpegDashUri);
        Console.WriteLine();

        // Display the URLs for progressive download.
        Console.WriteLine("Use the following URLs for progressive download.");
        mp4ProgressiveDownloadUris.ForEach(uri => Console.WriteLine(uri + "\n"));
        Console.WriteLine();

        // Download the output asset to a local folder.
        string outputFolder = "job-output";
        if (!Directory.Exists(outputFolder))
        {
            Directory.CreateDirectory(outputFolder);
        }

        Console.WriteLine();
        Console.WriteLine("Downloading output asset files to a local folder...");
        asset.DownloadToFolder(
            outputFolder,
            (af, p) =>
            {
                Console.WriteLine("Downloading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Output asset files available at '{0}'.", Path.GetFullPath(outputFolder));
    }

##<a name="test-by-playing-your-content"></a>Otestujte přehrávání obsahu  

Po spuštění programu definované v předchozí části, zobrazí se v okně konzoly podobně jako tento adresy URL.

Adaptivní streamování adresy URL:

Hladký přenos

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

HLS

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

MPEG POMLČKA

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)

Postupné stahování URL (zvuk a video).

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


Ke streamování videa, použijte [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

Testování postupné stahování, vložte adresu URL do prohlížeče (například Internet Explorer, Chrome nebo Safari).


##<a name="next-steps-media-services-learning-paths"></a>Další kroky: Media Services naučné stezky

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


### <a name="looking-for-something-else"></a>Hledáte něco jiného?

Toto téma neobsahoval, co jste očekávali, něco chybí, zda v jiným způsobem neměli vašim potřebám, zadejte nám svůj názor pomocí níže uvedených Disqus vlákna.


<!-- Anchors. -->


<!-- URLs. -->
  [Web Platform Installer]: http://go.microsoft.com/fwlink/?linkid=255386
  [Portal]: http://manage.windowsazure.com/
