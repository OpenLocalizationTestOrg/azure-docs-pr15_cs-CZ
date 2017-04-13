<properties
    pageTitle="Doručení obsahu zákazníkům | Microsoft Azure"
    description="Toto téma obsahuje přehled o co je součástí doručování obsahu s Azure Media Services."
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


# <a name="deliver-content-to-customers"></a>Doručení obsahu zákazníkům
Přenos nebo poskytují obsah jste předvádění zákazníkům, mohl při předvádění videa ve vysoké kvalitě různá zařízení podmínkách jiné síti.

K tomuto účelu máte tyto možnosti:

- Kódování toku proudu videa s víc bitrate (adaptivní bitrate). To starat o kvality a síťové podmínky.
- Pomocí Microsoft Azure Media Services [dynamické balení](media-services-dynamic-packaging-overview.md) dynamicky znovu balíček vašeho proudu do různých protokoly. To starat o přenos na různých zařízeních. Media Services podporuje odprezentování následující adaptivní přenosová streamování technologie: HTTP Live datových proudů (HLS) hladký přenos, MPEG-POMLČKU a konfigurace (pro Adobe Primetime/přístup nabyvatelů licence pouze).

Tento článek obsahuje přehled důležitých doručování obsahu koncepty.

Kontrola známé problémy najdete v tématu [Známé problémy](media-services-deliver-content-overview.md#known-issues).

## <a name="dynamic-packaging"></a>Dynamické balení

Dynamické balení poskytující Media Services, můžete dodat adaptivní bitrate MP4 a přitom zajistit hladký přenos zakódovaný úprav textu v datových proudů formáty podporované kodérem Media Services (MPEG-POMLČKU HLS, hladký přenos, konfigurace) aniž by bylo nutné znovu balíček do těchto streamování formáty. Doporučujeme, abyste doručování obsahu s dynamické obalu.

Umožní využít výhod dynamické balení potřebujete postupujte takto:

- Kódování mezzanine (zdrojový) souboru do sady adaptivní bitrate MP4 soubory nebo Adaptivní bitrate hladký přenos souborů.
- Potřebujete alespoň jednu jednotku streamování na vyžádání streamování koncového bodu, ze kterého chcete dodávat obsah. Další informace najdete v tématu [jak měřítko streamování rezervovaná jednotky na vyžádání](media-services-portal-manage-streaming-endpoints.md).

Dynamické balení, ukládat a zaplatit soubory ve formátu jedné úložiště. Media Services vytvořit a vytisknout vhodnou odpovědi podle vašich požadavků.

Kromě poskytuje přístup k možnostem dynamické balení, datových proudů rezervovaná jednotky na vyžádání umožňují vyhrazené výstupní kapacitu, můžete si koupili úsecích 200 MB/s. Ve výchozím nastavení je na vyžádání streamování konfigurovat modelu sdílené instance který server zdrojů (například výpočetním nebo výstupní kapacita) jsou sdílené s ostatními uživateli. Zlepšete streamování výkon na vyžádání zakoupením na vyžádání streamování rezervovaná jednotky.

Další informace najdete v tématu [dynamické obalu](media-services-dynamic-packaging-overview.md).

## <a name="filters-and-dynamic-manifests"></a>Filtry a dynamické manifesty

Definování filtrů pro svém majetku s Media Services. Tyto filtry jsou pravidla na straně serveru, které pomáhají zákazníkům takové věci, jako přehrát určitou část videa nebo zadat na dílčí verze zvuku a videa, které zařízení zákazníka zpracovat (ne všechny verze, které jsou přidružené k majetku). Tento filtrování dosáhnete pomocí *dynamického manifesty* , která se vytvoří při váš zákazník požaduje ke streamování videa podle jednoho nebo více zadanými filtry.

Další informace najdete v tématu [filtry a dynamické manifesty](media-services-dynamic-manifest-overview.md).

## <a name="locators"></a>Locator

Pokud chcete, aby vaše uživatele adresu URL, která mohou sloužit k můžete vysílat datovými proudy nebo stáhnout obsah, musíte nejdřív publikovat svůj materiálů vytvořením locator. Locator představuje vstupní bod pro přístup k souborům obsažené v aktivum. Media Services podporuje dva typy Locator:

- OnDemandOrigin Locator. Tyto slouží k toku médií (například MPEG-POMLČKU HLS a přitom zajistit hladký přenos) nebo postupně stahovat soubory.
-  Adresa URL Locator sdílený přístup podpisu (přidružení zabezpečení). Jsou použity ke stažení mediální soubory do místního počítače.

*Zásady přístupu* slouží k definování oprávnění (například pro čtení, Zapsat a seznam) a dobu trvání, pro kterou má klient přístup pro konkrétní materiálů. Všimněte si, že seznam oprávnění (AccessPermissions.List) neměla používat ve vytváření OrDemandOrigin locator.

Locator mít data vypršení platnosti. Portál Azure nastaví datum vypršení platnosti v budoucnu 100 let Locator.

>[AZURE.NOTE] Pokud jste použili k vytvoření Locator před březnem 2015 portálu Azure, byly tyto Locator nastavte vyprší po dvou letech.

Aktualizovat data vypršení platnosti na locator, použijte [REST](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator ) API [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) . Všimněte si, že když aktualizujete datum vypršení platnosti přidružení zabezpečení URL adresu URL změní.

Locator nejsou určené ke správě řízení přístupu uživatelů. Různé přístupových práv k jednotlivým uživatelům můžete zobrazit pomocí řešení Správa digitálních práv (DRM). Další informace najdete v tématu [Zabezpečení média](http://msdn.microsoft.com/library/azure/dn282272.aspx).

Když vytvoříte locator, může být zpoždění 30 sekund kvůli požadované úložiště a šíření procesy v úložišti Azure.


## <a name="adaptive-streaming"></a>Adaptivní datových proudů

Technologie adaptivní bitrate umožňují aplikacím přehrávače videa určit stav sítě a vybrat z několika bitrates. Když snižuje síťová komunikace klienta vybrat nižší bitrate přehrávání mohli pokračovat s nižší kvalitu videa. Jak zlepšit stav sítě, můžete přepnout klienta vyšší bitrate se vylepšení kvality videa. Azure Media Services podporuje následující adaptivní bitrate technologie: HTTP živé datových proudů (HLS) hladký přenos, MPEG-POMLČKU a konfigurace.

Uživatelům poskytnout streamování adresy URL, je třeba nejdříve vytvořit OnDemandOrigin locator. Vytvoříte locator získáte základní cesta k majetku s obsahem, kterou chcete vysílat. Ale pokud chcete mít možnost stáhnete tento obsah, potřebujete změnit tato cesta dál. Vytvářet úplnou adresu URL do datových proudů soubor, musí zřetězíte hodnotu cesty ikoně umístění a manifest (filename.ism) název souboru. Potom **připojte/Manifest** a vhodný formát (v případě potřeby) cestu URL.

>[AZURE.NOTE]Vysílat datovými proudy obsah přes připojení SSL. K tomuto účelu zkontrolujte, že streamování adresy URL začínat HTTPS.

Můžete jenom přenášet přes připojení SSL Pokud streamování koncový bod, ze které doručení obsahu byl vytvořen za 10 září 2014. Pokud streamování adresy URL založené na datových proudů koncové body vytvořené po 10 září 2014 adresa URL obsahuje "streaming.mediaservices.windows.net." Streamování adresy URL, které obsahují "origin.mediaservices.windows.net" (starý formát) nepodporují SSL. Pokud je vaše adresa URL v původním formátu a chcete mít možnost streamování SSL, vytvořte nový streamování koncový bod. Použití adresy URL založené na nový streamování koncový bod ke streamování obsahu SSL.


## <a name="streaming-url-formats"></a>Přenos formáty adresy URL

### <a name="mpeg-dash-format"></a>Formát MPEG-pomlčka

{streamování koncového bodu služby media název účtu name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8B70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=MPD-Time-CSF)



### <a name="apple-http-live-streaming-hls-v4-format"></a>Formát V4 Apple HTTP Live datových proudů (HLS)

{streamování koncového bodu služby media název účtu name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8B70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl)

### <a name="apple-http-live-streaming-hls-v3-format"></a>Formát V3 Apple HTTP Live datových proudů (HLS)

{streamování koncového bodu služby media název účtu name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl-v3)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8B70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl-v3)

### <a name="apple-http-live-streaming-hls-format-with-audio-only-filter"></a>Apple HTTP Live datových proudů (HLS) formátu se jenom hlasový přenos filtru

Ve výchozím nastavení jsou jenom hlasový přenos skladeb součástí HLS manifestu. Toto je nutný pro certifikační Apple Store pro mobilní sítě. V tomto případě klienta nemá dostatečnou šířku pásma nebo pokud je připojený přes připojení 2G, přehrávání přepne do jenom hlasový přenos. Tímto způsobem můžete zabránit obsahu streamování bez ukládání, ale nezobrazí se žádné video. V některých případech může být přehrávače vyrovnávací paměť upřednostňované přes jenom hlasový přenos. Pokud chcete odebrat jenom hlasový přenos sledovat, přidejte **jenom hlasový přenos = false** na adresu URL.

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8B70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl-v3,Audio-only=false)

Další informace najdete v tématu [dynamické Manifest složení podporu a HLS výstup další funkce](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/).


### <a name="smooth-streaming-format"></a>Hladký datových proudů formát

{streamování koncového bodu služby media název účtu name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Příklad:

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8B70-203e86b0df58/BigBuckBunny.ISM/manifest

### <a id="fmp4_v20"></a>Hladký přenos 2.0 manifestu (starší verze manifestu)

Ve výchozím nastavení obsahuje hladký přenos seznamu Formát značku opakování (r značky úkol). Některé přehrávače nepodporují značku r. Klienti s tyto přehrávače můžete použít formát, který zakáže r značku:

{streamování koncového bodu služby media název účtu name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=fmp4-v20)

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=fmp4-v20)

### <a name="hds-for-adobe-primetimeaccess-licensees-only"></a>Konfigurace (pro pouze nabyvatelů licence Adobe PrimeTime/Access)

{streamování koncového bodu služby media název účtu name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=f4m-f4f)

## <a name="progressive-download"></a>Postupné stahování

Postupné stahování můžete spouštět přehrávání média před stažená celý soubor. Nelze stáhnout postupně .ism * (ismv isma, ismt soubory nebo ismc).

Postupně stahování obsahu, použijte OnDemandOrigin typ locator. Následující příklad ukazuje adresu URL, který je založený na typu OnDemandOrigin locator:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

Musíte dešifrovat případně zašifrovaných úložiště zdroje, které chcete vysílat z origin služby pro postupné stahování.

## <a name="download"></a>Ke stažení

Pokud si Pokud chcete stáhnout obsah klientského zařízení, musíte vytvořit Locator přidružení zabezpečení. Přidružení zabezpečení locator vám umožňuje přístup k úložišti Azure kontejneru kde je soubor umístěn. Pokud chcete vytvořit adresu URL pro stažení, budete muset vložení názvu souboru mezi Host (hostitel) a přidružení zabezpečení podpis.

Následující příklad ukazuje adresu URL, která je založená na locator přidružení zabezpečení:

    https://test001.blob.core.windows.net/asset-ca7a4c3f-9eb5-4fd8-a898-459cb17761bd/BigBuckBunny.mp4?sv=2012-02-12&se=2014-05-03T01%3A23%3A50Z&sr=c&si=7c093e7c-7dab-45b4-beb4-2bfdff764bb5&sig=msEHP90c6JHXEOtTyIWqD7xio91GtVg0UIzjdpFscHk%3D

Platí následující omezení:

- Musíte dešifrovat případně zašifrovaných úložiště zdroje, které chcete vysílat z origin služby pro postupné stahování.
- Stažení, který nebylo dokončeno do 12 hodin se nezdaří.

## <a name="streaming-endpoints"></a>Přenos koncové body

Streamování koncový bod představuje streamování služba, která přináší obsah přímo na klientské aplikace přehrávače nebo síť pro doručování obsahu (CDN) pro další rozdělení. Odchozí proudu z datových proudů koncového bodu služby může být živou toku nebo poskytují materiálů ve vašem účtu Media Services. Můžete taky určit kapacitu streamování koncového bodu služby pro zpracování potřeb rostoucí šířky pásma úpravou streamování rezervovaná jednotky. Nejméně jednu jednotku rezervovaná přidělovat aplikací v prostředí výroby. Další informace najdete v tématu [jak měřítko médií služby](media-services-portal-manage-streaming-endpoints.md).

## <a name="known-issues"></a>Známé problémy

### <a name="changes-to-smooth-streaming-manifest-version"></a>Změny hladký přenos manifest verze

Před uvolněním služby 2016 červenec – Pokud prostředky vytvořené pomocí Media Encoder standardní Media Encoder Premium pracovního postupu nebo starší Azure Media Encoder byly streamují pomocí dynamického balení – přitom zajistit hladký přenos manifestu vrácené by odpovídají verze 2.0. Verze 2.0 dobu trvání fragment nepoužívejte značky takzvané opakovat (r). Příklad:

<?xml version="1.0" encoding="UTF-8"?>
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="0" Duration="8000" TimeScale="1000">
        <StreamIndex Chunks="4" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="3" Subtype="" Name="video" TimeScale="1000">
            <QualityLevel Index="0" Bitrate="1000000" FourCC="AVC1" MaxWidth="640" MaxHeight="360" CodecPrivateData="00000001674D4029965201405FF2E02A100000030010000003032E0A000F42400040167F18E3050007A12000200B3F8C70ED0B16890000000168EB7352" />
            <c t="0" d="2000" n="0" />
            <c d="2000" />
            <c d="2000" />
            <c d="2000" />
        </StreamIndex>
    </SmoothStreamingMedia>

Ve verzi služby červenec 2016 manifest vygenerovaných hladký přenos odpovídá verze 2.2, s dobou trvání fragment pomocí značek opakování. Příklad:

    <?xml version="1.0" encoding="UTF-8"?>
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="2" Duration="8000" TimeScale="1000">
        <StreamIndex Chunks="4" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="3" Subtype="" Name="video" TimeScale="1000">
            <QualityLevel Index="0" Bitrate="1000000" FourCC="AVC1" MaxWidth="640" MaxHeight="360" CodecPrivateData="00000001674D4029965201405FF2E02A100000030010000003032E0A000F42400040167F18E3050007A12000200B3F8C70ED0B16890000000168EB7352" />
            <c t="0" d="2000" r="4" />
        </StreamIndex>
    </SmoothStreamingMedia>

Některé ze starších klientů hladký přenos nemusí podporovat opakování značky a načtení manifest se nezdaří. Zmírnění tento problém, můžete použít parametr starší verze seznamu formát **(formát = fmp4 v20)** nebo aktualizovat svému klientovi na nejnovější verzi, který podporuje opakování značky. Další informace najdete v tématu [hladký přenos 2.0](media-services-deliver-content-overview.md#fmp4_v20).

## <a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>Příbuzná témata

[Aktualizace Media Services Locator po pohybu kláves úložiště](media-services-roll-storage-access-keys.md)
