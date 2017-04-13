<properties 
    pageTitle="Filtry a dynamické manifesty | Microsoft Azure" 
    description="Toto téma popisuje, jak vytvářet filtry tak svému klientovi můžete používat k určité oddíly toku datového proudu. Media Services vytvoří dynamické manifesty archivace výběrové streamování." 
    services="media-services" 
    documentationCenter="" 
    authors="cenkdin" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="cenkdin;juliako"/>

# <a name="filters-and-dynamic-manifests"></a>Filtry a dynamické manifesty

Začnete s 2.11 vydání, Media Services můžete definovat filtry majetku. Pravidla na straně serveru, které vám umožní zákazníky rozhodnete provádět věci, jako jsou tyto filtry: přehrávání pouze části videa (místo přehrávání celé video), nebo zadat jenom dílčí verze zvuku a videa, které zařízení zákazníka zpracovat (ne všechny verze, které jsou přidružené k majetku). Tento filtrování svém majetku archivované prostřednictvím **Dynamické Manifest**s vytvořenou požádání zákazníka ke streamování videa podle zadaného filtry.

Tato témata nápovědy popisuje běžné scénáře, ve kterých pomocí filtrů bude velmi užitečné pro vašich zákazníků a odkazů na témata, která ukazují, jak vytvářet filtry programově (v současné době můžete vytvářet filtry v rozhraní REST API pouze).

##<a name="overview"></a>Základní informace

Doručování obsahu pro zákazníky (streamování živou události nebo poskytují) mohl při předvádění video ve vysoké kvalitě různá zařízení podmínkách jiné síti. K dosažení tento cíl takto:

- kódování toku proudu videa s víc bitrate ([adaptivní bitrate](http://en.wikipedia.org/wiki/Adaptive_bitrate_streaming)) (to budou starat o kvality a sítě podmínky) a 
- pomocí Media Services [Dynamické balení](media-services-dynamic-packaging-overview.md) dynamicky znovu balíček vašeho proudu do různých protokoly (to budou starat o přenos na různých zařízeních). Media Services podporuje odprezentování následující adaptivní přenosová streamování technologie: HTTP Live datových proudů (HLS) hladký přenos, MPEG POMLČKU a konfigurace (pro Adobe PrimeTime/přístup nabyvatelů licence pouze). 

###<a name="manifest-files"></a>Seznamu souborů 

Při kódování aktiva pro přenos adaptivní bitrate se vytvoří soubor **manifest** (STOP) (soubor je založený na text nebo založeném na jazyce XML). Obsahuje soubor **manifest** jako datových proudů metadat: typ (zvuk, video nebo textu) můžete sledovat, sledovat jméno, čas zahájení a ukončení, bitrate (Vlastnosti), jazyky pro sledování prezentace okna (posuvné s pevnou dobou trvání), Videokodeky (FourCC). Nastaví také přehrávač k načtení další fragment zadáním informací o další umožňujícím přehrávání videa neúplné k dispozici a jejich umístění. Neúplné (nebo segmentů) jsou skutečný "bloky" obsahu videa.


Tady je příklad souboru seznamu: 

    
    <?xml version="1.0" encoding="UTF-8"?>  
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="0" Duration="330187755" TimeScale="10000000">
    
    <StreamIndex Chunks="17" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="8">
    <QualityLevel Index="0" Bitrate="5860941" FourCC="H264" MaxWidth="1920" MaxHeight="1080" CodecPrivateData="0000000167640028AC2CA501E0089F97015202020280000003008000001931300016E360000E4E1FF8C7076850A4580000000168E9093525" />
    <QualityLevel Index="1" Bitrate="4602724" FourCC="H264" MaxWidth="1920" MaxHeight="1080" CodecPrivateData="0000000167640028AC2CA501E0089F97015202020280000003008000001931100011EDC00002CD29FF8C7076850A45800000000168E9093525" />
    <QualityLevel Index="2" Bitrate="3319311" FourCC="H264" MaxWidth="1280" MaxHeight="720" CodecPrivateData="000000016764001FAC2CA5014016EC054808080A00000300020000030064C0800067C28000103667F8C7076850A4580000000168E9093525" />
    <QualityLevel Index="3" Bitrate="2195119" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FAC2CA503C045FBC054808080A000000300200000064C1000044AA0000ABA9FE31C1DA14291600000000168E9093525" />
    <QualityLevel Index="4" Bitrate="1469881" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FAC2CA503C045FBC054808080A000000300200000064C04000B71A0000E4E1FF8C7076850A4580000000168E9093525" />
    <QualityLevel Index="5" Bitrate="978815" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EAC2CA50280BFE5C0548303032000000300200000064C08001E8480004C4B7F8C7076850A45800000000168E9093525" />
    <QualityLevel Index="6" Bitrate="638374" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EAC2CA50280BFE5C0548303032000000300200000064C080013D60000C65DFE31C1DA1429160000000168E9093525" />
    <QualityLevel Index="7" Bitrate="388851" FourCC="H264" MaxWidth="320" MaxHeight="180" CodecPrivateData="000000016764000DAC2CA505067E7C054830303200000300020000030064C040030D40003D093F8C7076850A45800000000168E9093525" />
    
    <c t="0" d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="9600000"/>
    </StreamIndex>
    
    
    <StreamIndex Chunks="17" Type="audio" Url="QualityLevels({bitrate})/Fragments(AAC_und_ch2_128kbps={start time})" QualityLevels="1" Name="AAC_und_ch2_128kbps">
    <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="125658" FourCC="AACL" CodecPrivateData="1210" Channels="2" PacketSize="4" SamplingRate="44100" />
    
    <c t="0" d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="6965987" /></StreamIndex>
    
    
    <StreamIndex Chunks="17" Type="audio" Url="QualityLevels({bitrate})/Fragments(AAC_und_ch2_56kbps={start time})" QualityLevels="1" Name="AAC_und_ch2_56kbps">
    <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="53655" FourCC="AACL" CodecPrivateData="1210" Channels="2" PacketSize="4" SamplingRate="44100" />
    
    <c t="0" d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="6965987" /></StreamIndex>
    
    </SmoothStreamingMedia>
    
###<a name="dynamic-manifests"></a>Dynamické manifesty

Existují [scénáře](media-services-dynamic-manifest-overview.md#scenarios) klientem potřebujete větší flexibilitu než co popisují v seznamu souborů výchozí materiálů. Příklad:

- Zařízení konkrétní: předvádění nebo zadaný verze zadána skladeb jazyků podporovaných službami zařízení, které je pouze slouží k přehrávání obsahu ("verze filtrování"). 
- Zmenšení manifest zobrazíte dílčí klipartu živou události ("dílčí klipartu filtrování").
- Střih úvodní videa ("oříznutí videa").
- Upravte prezentaci okno (DVR) za účelem zajištění určitou část DVR okna prohlížeče ("okno s přizpůsobení prezentace").
 
Dosažení této flexibilní, Media Services nabízí **Dynamické manifesty** podle předdefinované [filtry](media-services-dynamic-manifest-overview.md#filters).  Po definování filtrů klienty je můžou použít k určité verze nebo dílčí klipy videa můžete vysílat datovými proudy. Filtry by určují v datových proudů adresu URL. Filtry může být použity pro adaptivní bitrate streamování protokolů podporovaných [Dynamické balení](media-services-dynamic-packaging-overview.md): HLS MPEG-POMLČKU, hladký přenos a konfigurace. Příklad:

Adresa URL POMLČKU MPEG pomocí filtru

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf,filter=MyLocalFilter)

Hladký přenos adresu URL s filtrem

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyLocalFilter)


Další informace o doručení obsahu a vytváření datových proudů adresy URL najdete v článku [Přehled doručování obsahu](media-services-deliver-content-overview.md).


>[AZURE.NOTE]Všimněte si, že dynamické manifesty se nemění majetek a výchozí instalované zdroje. Klient můžete požádat o toku pomocí hesla nebo bez filtry. 


###<a id="filters"></a>Filtry 

Existují dva typy filtrů materiálů: 

- Globální filtry (se dají použít pro všechny materiály na účet Azure Media Services, mít životnost účtu) a 
- Místní filtry (lze použít pouze pro aktivum, se kterým je spojené při vytvoření filtru, mít životnost majetku). 

Globální a místní filtr typy mají přesně stejné vlastnosti. Hlavní rozdíl mezi těmito dvěma je které scénáře jaký druh filer vhodnější. Globální filtry vyhovují obecně profily zařízení (verze filtrování) místo, kam lze použít místní filtry se střihu konkrétní materiálů.


##<a id="scenarios"></a>Obvyklé scénáře 

Uvedené byl před doručování obsahu pro zákazníky (streamování živou události nebo poskytují) mohl při předvádění video ve vysoké kvalitě různá zařízení podmínkách jiné síti. Navíc vaše může mít jiné požadavky, které se týkají filtrování svém majetku a pomocí **Dynamického Manifest**s. V následujících částech uveďte krátký základní informace o různých scénářích filtrování.

- Zadejte jen podmnožinu zvuku a videa verze zpracovávající některých zařízení (ne všechny verze, které jsou přidružené k majetku). 
- Přehrávání pouze části videa (místo přehrávání celé video).
- Upravte okno DVR prezentace.

##<a name="rendition-filtering"></a>Interpretace filtrování 

Můžete rozhodnout kódovat materiálů více kódování profilů (podle směrného plánu H.264 H.264 vysoké, AACL, AACH, Dolby Digital Plus) a více bitrates kvalitu. Ale ne všech klientských zařízení podporují všechny vaše majetku profily a bitrates. Například starších zařízení s Androidem podporuje pouze H.264 podle směrného plánu + AACL. Odeslání vyšší bitrates zařízení, které se nemůžou dostat výhod, zabírají šířku pásma a zařízení výpočtu. Tato zařízení musí dekódovat všechny zadané informace jenom pro Neomezovat pro zobrazení.

S Manifest dynamické vytvoříte profily zařízení například číslo na mobil, konzoly HD/ss atd a zahrnout skladeb a vlastnosti, které mají být součástí každého profilu.

 
![Příklad filtrování verze][renditions2]

V následujícím příkladu byl použit encoder kódování mezzanine materiálů do sedm videa verze ISO MP4s (z 180p 1080p). Zakódovaný materiálů můžete dynamicky sbalí do libovolného z následujících datových proudů protokolů: HLS, hladký, MPEG POMLČKU a konfigurace.  V horní části diagramu instalované HLS majetku s žádné filtry zobrazený (obsahuje všechny sedm verzí).  V levém dolním rohu zobrazí se manifest HLS, do které byl použit filtr s názvem "ott". Filtr "ott" Určuje, chcete-li odebrat všechny bitrates pod 1Mbps, které vedly k dolní dvě kvality úrovně právě odstranit v odpovědi.  V pravém dolním se zobrazuje manifest HLS, do které byl použit filtr s názvem "mobilní". "Mobilní" filtr určuje odebrat verze, kde je větší než 720p, které vedly ve dvou rozlišení 1080p verze odstranit.

![Interpretace filtrování][renditions1]

##<a name="removing-language-tracks"></a>Odebrání jazykové stopy

Svém majetku můžou obsahovat více zvukových jazyků, například angličtina, španělština, francouzština, atd. Obvykle manažéry Player SDK výchozí výběr zvukového sledování a k dispozici zvuk sleduje za výběr uživatele. Je náročné se dají takové SDK přehrávače, vyžaduje jinou implementace přes specifické pro zařízení přehrávače rámce. Na některých platformách také API přehrávače jsou omezené a nezahrnujte funkce zvukové výběru kde nelze uživatelům vybrat nebo změnit výchozí zvukové sledování. Pomocí filtrů materiálů můžete určit chování vytvořením filtry, které obsahovat pouze požadované jazyky zvuku.

![Jazykové stopy filtrování][language_filter]


##<a name="trimming-start-of-an-asset"></a>Oříznutí start aktiva 

Ve většině živou streamování události spusťte operátory některé testy před skutečné událostí. Například, můžou obsahovat stůl takto před začátkem události: "Program začne krátkodobě". Pokud archivace program test stůl data také archivovány a bude obsahovat v prezentaci. Tyto informace však neměli uvádějí klientům. Pomocí dynamického Manifest můžete vytvořit filtrem čas zahájení a odebrat nežádoucí data z manifestu.

![Zahájení oříznutí][trim_filter]

##<a name="creating-sub-clips-views-from-a-live-archive"></a>Vytvoření podřízené klipy (zobrazení) z živého archivu

Mnoho živou události, které jsou dlouhé spuštěný a živou archivu můžou obsahovat více událostí. Po živou události konce televizního chtít rozdělit živou archivovat do spuštění programu logické a zastavit řady. Pak publikujte aplikacích virtuální samostatně bez příspěvek zpracování živou archivu a nevytvářet samostatné prostředky, (které nebude vstoupit sítě CDN výhodou stávající mezipaměti neúplné). Příklady těchto virtuální programů (dílčí klipy) jsou čtvrtletí football nebo basketbalová hra, innings v basketbalu nebo jednotlivých událostí odpoledne programu olympijských hrách.

Pomocí dynamického Manifest můžete vytvořit filtrů s použitím počáteční a koncový čas a vytvořit virtuální zobrazení v horní části živou archivu. 

![Subclip filtru][subclip_filter]

Filtrované materiálů:

![Lyžování][skiing]

##<a name="adjusting-presentation-window-dvr"></a>Úprava okno prezentace (DVR)

V současné době Azure Media Services nabízí cyklický archivu kde možné konfigurovat dobu trvání mezi 5 minut - 25 hodin. Filtrování seznamu slouží k vytvoření postupné DVR okna nad začátek archiv, aniž by odstranil média. Existuje mnoho scénářů místo, kam chcete zadat omezené okno DVR které slouží k přesunutí živou okraje a současně zachovat větší archivace okno televizního. Televizního vysílání může být vhodnější použít data z okna DVR zvýrazněte klipy nebo he\she chtít poskytují různé DVR windows pro různá zařízení. Většina mobilních zařízení například nepracují velký DVR windows (okno DVR 2 minuty pro mobilní zařízení a 1 hodinu může mít pro desktopoví klienti).

![Okno DVR][dvr_filter]

##<a name="adjusting-livebackoff-live-position"></a>Úprava LiveBackoff (pozice live)

Filtrování seznamu mohou sloužit k odebrání živou okraji živou programu několik sekund. Díky televizního přehrát prezentaci v bodě náhled publikace a vytvářet reklamní body vložení než příjemci obdrží toku (obvykle záložní nepracovní 30 sekund). Televizního můžete použít tyto inzerce pro jejich rámce klienta v čase, je přijaté a zpracování informací před reklamní příležitost.

Kromě podpory reklamní se dá použít pro úpravu klienta živou stažení pozici tak, aby při klientů unášena a klepněte na okraji živou dál dostali neúplné ze serveru místo dochází k chybám HTTP 404 nebo 412 LiveBackoff.

![livebackoff_filter][livebackoff_filter]


##<a name="combining-multiple-rules-in-a-single-filter"></a>Kombinování víc pravidel v jednom filtru

Zkombinováním více pravidel filtrování v jednom filtru. Jako příklad můžete definovat pravidlo oblast z živého archivu odebrat stůl a filtrovat také k dispozici bitrates. V konečném důsledku více pravidel filtrování je složení (pouze průsečíku) jednotlivá pravidla.

![více pravidel][multiple-rules]

##<a name="create-filters-programmatically"></a>Vytváření filtrů programově

Následující téma popisuje entity Media Services, které se vztahují k filtry. Téma také ukazuje, jak programově vytvářet filtry.  

[Vytváření filtrů pomocí rozhraní REST API](media-services-rest-dynamic-manifest.md).

## <a name="combining-multiple-filters-filter-composition"></a>Kombinace více filtrů (filtr složení)

Můžete také sloučit více filtrů v jedné adresy URL. 

Následující scénář ukazuje, proč můžete chtít kombinovat filtry:

1. Potřebujete filtrováním videa vlastnosti pro mobilní zařízení Android ATP iPAD (omezit kvality videa). Pokud chcete odebrat nežádoucí kvality, vytvoříte globální filtr, který je vhodné pro profily zařízení. Výše uvedené, globální filtry můžete použít pro všechny prostředky v části stejný účet služby médií bez dalších přidružení. 
2. Můžete taky chtít přesně udělat střih čas zahájení a ukončení aktiva. K dosažení tohoto by vytvořte místního filtru a nastavte počáteční a koncový čas. 
3. Chcete sloučit obě tyto filtry (bez kombinaci by potřeba přidat kvality filtrování k oříznutí filtr, který bude ztížit použití filtru).

Sloučit filtry, je třeba nastavit názvy filtrů do manifestu/stop URL s středníky. Předpokládejme, že máte filtru s názvem *MyMobileDevice* filtry vlastností a že pojmenované jiné *MyStartTime* chcete nastavit konkrétní počáteční čas. Můžete kombinovat takto:

    http://teststreaming.streaming.mediaservices.windows.net/3d56a4d-b71d-489b-854f-1d67c0596966/64ff1f89-b430-43f8-87dd-56c87b7bd9e2.ism/Manifest(filter=MyMobileDevice;MyStartTime)

Můžete kombinovat až 3 filtry. 

Další informace najdete v [tomto](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blogu.


##<a name="know-issues-and-limitations"></a>Problémy a omezení

- Dynamické manifestu pracuje v GOP hranice (klíč rámce), tedy oříznutí má GOP přesnosti. 
- Můžete použít stejný název filtru pro místní a globální filtry. Všimněte si, že místního filtru vyšší prioritu a přepíše globální filtry.
- Pokud aktualizujete filtru, může trvat až 2 minuty streamování koncový bod k aktualizaci pravidla. Pokud obsah byl doručen pomocí některé filtry (a uložené v proxy servery a CDN mezipaměti), aktualizace tyto filtry může dojít k chybám player. Je doporučená vymazání mezipaměti po aktualizaci filtr. Pokud tato možnost není možné, zvažte použití různých filtru.


##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



##<a name="see-also"></a>Viz taky

[Doručení obsahu na přehled zákazníci](media-services-deliver-content-overview.md)

[renditions1]: ./media/media-services-dynamic-manifest-overview/media-services-rendition-filter.png
[renditions2]: ./media/media-services-dynamic-manifest-overview/media-services-rendition-filter2.png

[rendered_subclip]: ./media/media-services-dynamic-manifests/media-services-rendered-subclip.png
[timeline_trim_event]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-event.png
[timeline_trim_subclip]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-subclip.png

[multiple-rules]:./media/media-services-dynamic-manifest-overview/media-services-multiple-rules-filters.png

[subclip_filter]: ./media/media-services-dynamic-manifest-overview/media-services-subclips-filter.png
[trim_event]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-event.png
[trim_subclip]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-subclip.png
[trim_filter]: ./media/media-services-dynamic-manifest-overview/media-services-trim-filter.png
[redered_subclip]: ./media/media-services-dynamic-manifests/media-services-rendered-subclip.png
[livebackoff_filter]: ./media/media-services-dynamic-manifest-overview/media-services-livebackoff-filter.png
[language_filter]: ./media/media-services-dynamic-manifest-overview/media-services-language-filter.png
[dvr_filter]: ./media/media-services-dynamic-manifest-overview/media-services-dvr-filter.png
[skiing]: ./media/media-services-dynamic-manifest-overview/media-services-skiing.png
 