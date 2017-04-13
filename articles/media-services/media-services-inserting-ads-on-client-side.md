<properties 
    pageTitle="Vložení reklam na straně klienta | Microsoft Azure" 
    description="Toto téma ukazuje, jak vložit reklam na straně klienta." 
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
    ms.date="09/26/2016" 
    ms.author="juliako"/>


#<a name="inserting-ads-on-the-client-side"></a>Vložení reklam na straně klienta

Toto téma obsahuje informace o tom, jak vložit různé typy reklam na straně klienta.

Informace o skryté titulky a ad podpora v Live streamování videa najdete v článku [podporované titulky a standardy vložení Ad](media-services-live-streaming-with-onprem-encoders.md#cc_and_ads).

>[AZURE.NOTE] Azure Media Playeru aktuálně nepodporuje služby Active Directory.

##<a id="insert_ads_into_media"></a>Vložení služby Active Directory do svého média

Azure Media Services podporuje vkládání ad prostřednictvím platformy Windows Media: přehrávače rámce. Přehrávač rámce s podporou ad jsou k dispozici pro zařízení s Windows 8, Silverlight, Windows Phone 8 a iOS. Každý framework přehrávače obsahuje ukázkový kód, který se dozvíte, jak implementovat aplikace player. Existují tři různé druhy reklam, které se dají vložit do média: seznamu.

- **Lineární** – celého snímku reklam, které Zastavte ukazatel myši na hlavní video.
- **Nonlinear** – překryvném reklam, které se zobrazí při přehrávání hlavní videa, obvykle logo nebo jiný statický obrázek umístí do prohlížeče.
- **Průvodce vyhledáváním** – reklam, které se zobrazují mimo přehrávač.

Služby Active Directory mohou být umístěny v jakémkoli okamžiku hlavní video časová osa. Přehrávač musí řekněte přehrávaných ad a které reklam přehrát. Důvodem je pomocí sady standardní založeném na jazyce XML soubory: Video Ad služby šablony (VAST), více Ad stop (VMAP) ve digitální Video, šablony abstraktní klasifikace na médií (STOŽÁRŮ) a digitální Video přehrávače Ad rozhraní definice (VPAID). VELKÉ soubory zadejte, jaké reklam k zobrazení. Soubory VMAP určit, kdy má přehrát různé služby Active Directory a obsahují OBROVSKÉ XML. Soubory STOŽÁRŮ jsou další způsob, jak sekvence služby Active Directory, která může obsahovat také OBROVSKÉ XML. Soubory VPAID definovat rozhraní mezi přehrávače videa a ad nebo ad server.

Každý přehrávače framework funguje jinak a každý vztahovat v samostatném tématu. Toto téma popisuje základní mechanismy slouží k vložení služby Active Directory. Aplikace přehrávače videa požadovat reklam ze serveru ad. Ad server můžete odpovědět několika způsoby:

- Vrácení OBROVSKÉ souboru
- Vraťte se do souboru VMAP (s tímto VAST)
- Vrátit soubor STOŽÁRŮ (s tímto VAST)
- Vrátí OBROVSKÉ soubor s VPAID služby Active Directory

 
###<a name="using-a-video-ad-service-template-vast-file"></a>Použití soubor šablony (VAST) služby Ad videa

Soubor OBROVSKÉ určuje jaké ad nebo reklam k zobrazení. Následující kód XML je příklad souboru OBROVSKÉ pro lineární ad:
    
    <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
      <Ad id="115571748">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://www.myserver.com/tracking-resource]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:32</Duration>
                <TrackingEvents>
                  <Tracking event="start"><![CDATA[http://www.myserver.com/start-tracking-resource]]></Tracking>
                  <Tracking event="midpoint"><![CDATA[http://www.myserver.com/midpoint-tracking-resource]]></Tracking>
                  <Tracking event="complete"><![CDATA http://www.myserver.com/complete-tracking-resource]]></Tracking>
                  <Tracking event="expand"><![CDATA[http://www.myserver.com/expand-tracking-resource]]></Tracking>
                </TrackingEvents>
                <VideoClicks>
                  <ClickThrough id="Atlas Redirect"><![CDATA[http://www.myserver.com/click-resource]]></ClickThrough>
                  <ClickTracking id="Spare"></ClickTracking>
                </VideoClicks>
                <MediaFiles>
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                    <![CDATA[http://www.myserver.com/media/myad_200_4x3.wmv]]>
                  </MediaFile>
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
                    <![CDATA[http://www.myserver.com/media/myad_300_4x3.wmv]]>
                  </MediaFile>
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
          <Extensions>
            <Extension type="Atlas">
            </Extension>
          </Extensions>
        </InLine>
      </Ad>
    </VAST>
    
Lineární ad je popsán v **<Linear>** prvek. Určuje trvání ad, sledování událostí, klikněte na Procházet, klikněte na sledování a celá řada **<MediaFile>** prvky. Sledování událostí jsou jazyku v rámci **<TrackingEvents>** prvek a povolit server ad ke sledování různých událostí, ke kterým dochází při zobrazení ad. V tomto případě start, střední bod dokončení a rozbalit sledovány události. Při zobrazení ad výskytu události start. Střední bod události při aspoň, že byl zobrazen 50 % ad osy. Dokončení události při spuštění ad do konce. Rozbalení události při uživatel přehrávače videa na celou obrazovku. Jsou určena Clickthroughs **<ClickThrough>** element **<VideoClicks>** prvek a určuje identifikátor URI zdroje, chcete-li zobrazit poté, co uživatel klikne na ad. Podle ClickTracking **<ClickTracking>** elementu také v rámci **<VideoClicks>** prvek a určuje sledování zdroj přehrávačem požádat o poté, co uživatel klikne na ad. **<MediaFile>** Prvky zadejte informace o konkrétních kódování ad. Pokud existuje více než jedno **<MediaFile>** elementu přehrávače videa můžete zvolit nejlepší kódování pro platformu. 

Lineární reklam můžete zobrazit v uvedeném pořadí. K tomuto účelu přidat další <Ad> prvky, které mají VAST souboru a zadejte požadované pořadí pomocí atribut pořadí. Následující příklad ukazuje toto:
    
    <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
      <Ad id="1" sequence="0">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad1trackingResouce]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:32</Duration>
                <MediaFiles>
                  <!-- ... -->
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
        </InLine>
      </Ad>
      <Ad id="2" sequence="1">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad2trackingResouce]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:30</Duration>
                <MediaFiles>
                  <!-- ... -->
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
        </InLine>
      </Ad>
    </VAST>
    
Jsou v jazyku nelineárních problémů služby Active Directory <Creative> prvek i. Následující příklad ukazuje <Creative> prvek, který popisuje ad nelineárních problémů.

    <Creative id="video" sequence="1" AdID="">
      <NonLinearAds>
        <NonLinear width="216" height="121" minSuggestedDuration="00:00:15">
          <StaticResource creativeType="image/png"><![CDATA[http://myserver/images/image.png]]></StaticResource>
          <StaticResource creativeType="image/jpg"><![CDATA[http://myserver/images/image.jpg]]></StaticResource>
        </NonLinear>
        <TrackingEvents>
             <Tracking event="acceptInvitation"><![CDATA[http://myserver/tracking/trackingID]></Tracking>
             <Tracking event="collapse"><![CDATA[http://myserver/tracking/trackingID2]]></Tracking>
         </TrackingEvents>
       </NonLinearAds>
    </Creative>

 
**<NonLinearAds>** Element může obsahovat jednu nebo více **<NonLinear>** prvky, které popisují nelineárních problémů ad. **<NonLinear>** Element určuje je zdroj u ad nelineárních problémů. Zdroj lze **<StaticResouce>**je **<IFrameResource>**, ani znak **<HTMLResouce>**.**<StaticResource>** Popisuje prostředku není HTML a definuje creativeType atribut, který určuje způsob zobrazení zdroje:

Obrázek/gif, obrázek nebo jpeg, obrázek nebo png – zdroje se zobrazí v HTML **<img>** značku.

Aplikace/x-javascript – zdroje se zobrazí v značky HTML <**skript**>.

Aplikace/x-shockwave-flash – zdroje se zobrazí v Flash Playeru.

**<IFrameResource>**Popisuje prostředek HTML, který mohou být zobrazena v prvku IFrame. **<HTMLResource>**popisuje část kódu HTML, který jde vložit do webové stránky. **<TrackingEvents>**určení sledování událostí a identifikátor URI požádat o dojde k události. V tomto příkladu jsou sledovány acceptInvitation a sbalit události. Další informace o **<NonLinearAds>** prvku a jeho podřízeným objektům, najdete v tématu IAB.NET/VAST. Všimněte si, že **<TrackingEvents>** element je umístěn v rámci** <NonLinearAds> ** prvek spíše než **<NonLinear>** prvek.

Průvodce vyhledáváním reklam jsou definované v rámci <CompanionAds> prvek. <CompanionAds> Element může obsahovat jednu nebo více <Companion> prvky. Každý <Companion> prvek popisuje companion ad a může obsahovat <StaticResource>, <IFrameResource>, nebo <HTMLResource> kterého byla vyplněná stejným způsobem jako ad nelineárních problémů. VELKÉ soubor může obsahovat více companion služby Active Directory a aplikační Playeru můžete zvolit nejvhodnější ad zobrazíte. Další informace o VAST najdete v tématu [OBROVSKÉ 3.0](http://www.iab.net/media/file/VASTv3.0.pdf).

###<a name="using-a-digital-video-multiple-ad-playlist-vmap-file"></a>Používání digitálních videa více Ad (VMAP) stop

Soubor VMAP vám umožní určit při ad konců, jak dlouho je každým koncem, kolik reklam mohou být zobrazena v rámci konec oddílu a co typy reklam mohou být zobrazena během konec oddílu. Následující příklad VMAP souboru, který definuje jeden ad konec:
    
    <vmap:VMAP xmlns:vmap="http://www.iab.net/vmap-1.0" version="1.0">
      <vmap:AdBreak breakType="linear" breakId="mypre" timeOffset="start">
        <vmap:AdSource allowMultipleAds="true" followRedirects="true" id="1">
          <vmap:VASTData>
            <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
              <Ad id="115571748">
                <InLine>
                  <AdSystem version="2.0 alpha">Atlas</AdSystem>
                  <AdTitle>Unknown</AdTitle>
                  <Description>Unknown</Description>
                  <Survey></Survey>
                  <Error></Error>
                  <Impression id="Atlas"><![CDATA[http://view.atdmt.com/000/sview/115571748/direct;ai.201582527;vt.2/01/634364885739970673]]></Impression>
                  <Creatives>
                    <Creative id="video" sequence="0" AdID="">
                      <Linear>
                        <Duration>00:00:32</Duration>
                        <MediaFiles>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_200_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_300_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_500" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="500" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_500_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_700" maintainAspectRatio="true" scaleable="true" delivery="progressive" bitrate="700" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_700_4x3.wmv]]>
                          </MediaFile>
                        </MediaFiles>
                      </Linear>
                    </Creative>
                  </Creatives>
                </InLine>
              </Ad>
            </VAST>
          </vmap:VASTData>
        </vmap:AdSource>
        <vmap:TrackingEvents>
          <vmap:Tracking event="breakStart">
            http://MyServer.com/breakstart.gif
          </vmap:Tracking>
        </vmap:TrackingEvents>
      </vmap:AdBreak>
    </vmap:VMAP>
     
Soubor VMAP začíná <VMAP> prvek, který obsahuje jeden nebo více <AdBreak> prvky každý definování ad konec. Každým koncem ad Určuje typ ukončení, ID Konec a časový posun. Atribut breakType Určuje typ ad, který můžete přehrát během koncem: lineární, nelineárních problémů, nebo zobrazit. Zobrazení mapování služby Active Directory a služby Active Directory OBROVSKÉ Průvodce vyhledáváním. Více než jeden typ ad může být zadán v seznamu čárkami (bez mezer). BreakID je nepovinný identifikátor pro ad. TimeOffset Určuje, kdy mají být zobrazeny ad. Může být zadán v jednom z těchto způsobů:

1. Čas – hh nebo hh:mm:ss.mmm formát, kde .mmm milisekund. Hodnota atributu určuje čas od začátku videa časové osy na začátek koncem ad.
1. Procento – ve formátu n %, kde n je procento videa časová osa přehrávání před přehrávání ad
1. Začátek/konec – Určuje, že ad mají být zobrazeny před nebo po byla zobrazena videa
1. Umístěte – určuje pořadí ad konce po časování konce ad Neznámý stav, například živou streamování. Pořadí každým koncem ad není zadán ve formátu #n kde n je celé číslo 1 nebo větší. 1 označuje ad by měl přehrávaného při prvním příležitosti 2 označuje ad by měl přehrávaného při druhém příležitosti a tak dál.

V rámci elementu <**AdBreak**> může být jeden prvek <**AdSource**>. Element <**AdSource**> obsahuje následující atributy:

1. ID – Určuje identifikátor pro tento zdroj ad
1. allowMultipleAds – logická hodnota, která určuje, zda více reklam mohou být zobrazena během koncem ad
1. followRedirects – volitelné logická hodnota, která určuje, pokud mají respektovat přehrávače videa přesměruje do odpověď ad

Element <**AdSource**> poskytuje přehrávač odpověď ad vložený nebo odkaz na ad odpověď. Může obsahovat jednu z následujících prvků:

- <VASTAdData>Označuje, že odpověď OBROVSKÉ ad vložený do souboru VMAP
- <AdTagURI>identifikátor URI, který odkazuje na ad odpověď z jiného systému
- <CustomAdData>-na libovolný řetězec tuto respresents-OBROVSKÉ odpověď

V tomto příkladu je zadaným vložených ad odpověď <VASTAdData> prvek, který obsahuje velké ad odpověď. Další informace o dalších prvků najdete v tématu [VMAP](http://www.iab.net/guidelines/508676/digitalvideo/vsuite/vmap).

Element <**AdBreak**> mohou také obsahovat jeden prvek <**TrackingEvents**>. Element <**TrackingEvents**> umožňuje sledovat začátek nebo konec konec ad nebo zda došlo k chybě při koncem ad. Element <**TrackingEvents**> obsahuje jeden nebo více <**Sledování**> prvků, z nichž každá určuje událost nastavit jako sledování a sledování URI. Je možné sledování událostí:

1. breakStart – sleduje začátku ad konec
1. breakEnd – sledování dokončení ad konec
1. Chyba – sleduje chybu vyskytl koncem ad

Následující příklad ukazuje VMAP soubor, který určuje sledování událostí

    <vmap:VMAP xmlns:vmap="http://www.iab.net/vmap-1.0" version="1.0">
      <vmap:AdBreak breakType="linear" breakId="mypre" timeOffset="start">
        <vmap:AdSource allowMultipleAds="true" followRedirects="true" id="1">
          <vmap:VASTData>
            <!--Inline VAST -->
          </vmap:VASTData>
        </vmap:AdSource>
        <vmap:TrackingEvents>
          <vmap:Tracking event="breakStart">
            http://MyServer.com/breakstart.gif
          </vmap:Tracking>
          <vmap:Tracking event="breakend">
            http://MyServer.com/breakend.gif
          </vmap:Tracking>
          <vmap:Tracking event="error">
            http://MyServer.com/error.gif
          </vmap:Tracking>
        </vmap:TrackingEvents>
      </vmap:AdBreak>
    </vmap:VMAP>

Další informace o elementu <**TrackingEvents**> a jeho podřízeným objektům najdete v článku http://iab.org/VMAP.pdf

###<a name="using-a-media-abstract-sequencing-template-mast-file"></a>Pomocí médií abstraktní klasifikace soubor šablony (STOŽÁRŮ)

Soubor STOŽÁRŮ vám umožní určit aktivačních událostí, které definují, kdy se má zobrazit ad. Následující obrázek je příklad STOŽÁRŮ souboru, který obsahuje aktivační události pro ad zahrnout pre, střední zahrnout ad a ad po zahrnout.

    <MAST xsi:schemaLocation="http://openvideoplayer.sf.net/mast http://openvideoplayer.sf.net/mast/mast.xsd" xmlns="http://openvideoplayer.sf.net/mast" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      <triggers>
        <trigger id="preroll" description="preroll every item"  >
          <startConditions>
            <condition type="event" name="OnItemStart" />
          </startConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
    
        <trigger id="midroll" description="midroll at 15 sec."  >
          <startConditions>
            <condition type="property" name="Position" value="00:00:15.0" operator="GEQ" />
          </startConditions>
          <endConditions>
            <condition type="event" name="OnItemEnd"/>
            <!--This 'resets' the trigger for the next clip-->
          </endConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
    
        <trigger id="postroll" description="postroll"  >
          <startConditions>
            <condition type="event" name="OnItemEnd"/>
          </startConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
      </triggers>
    </MAST>

 

Soubor STOŽÁRŮ začíná **<MAST>** prvek, který obsahuje jeden **<triggers>** prvek. <triggers> Element obsahuje jednu nebo více **<trigger>** prvky, které definují, kdy se bude přehrávat ad. 

**<trigger>** Element obsahuje **<startConditions>** prvek, který určit, kdy má začít přehrávat ad. **<startConditions>** Element obsahuje jednu nebo více <condition> prvky. Při každé <condition> vyhodnotí jako PRAVDA aktivační je zahájená nebo odvolaný podle toho, jestli se <condition> je obsažená v **< startConditions**> nebo **<endConditions>** prvek v tomto pořadí. Když více <condition> prvky jsou k dispozici, je považováno za implicitní OR, všech podmínek vyhodnocení true (pravda) způsobí aktivační událost zahajte. <condition>může být vnořeno do prvky. Když podřízené <condition> přednastavené prvky, je považováno za implicitní a, všechny podmínky musí být vyhodnocen jako true aktivační události zahajte. <condition> Prvek obsahuje následující atributy, které definují podmínky: 

1. **Typ** – Určuje typ podmínky, událost nebo vlastnosti
1. **název** – název na vlastnost nebo událost, bude použita při vyhodnocování
1. **hodnoty** – hodnota, která vlastnost bude vyhodnocen
1. **operátor** – operaci použitý při vyhodnocení: EQ (znaménko rovná) NEQ (není rovno), GTR (vyšší), GEQ (větší nebo rovno), LT (menší než), LEQ (menší nebo rovna), MOD (modulo)

**<endConditions>**také obsahovat <condition> prvky. Když podmínka vyhodnocena jako PRAVDA aktivační událost je obnovit. <trigger> Prvek obsahuje také <sources> prvek, který obsahuje jeden nebo více <source> prvky. <source> Prvky definovat identifikátor URI ad odpověď a typ ad odpověď. V tomto příkladu je uveden identifikátor URI odpověď na velké. 


    <trigger id="postroll" description="postroll"  >
      <startConditions>
        <condition/>
      </startConditions>
      <sources>
        <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
          <sources />
        </source>
      </sources>
    </trigger>
 

###<a name="using-video-player-ad-interface-definition-vpaid"></a>Používání přehrávače videa Ad rozhraní definice (VPAID)

VPAID je rozhraní API pro povolení spustitelný ad jednotky komunikovat s přehrávače videa. Díky ad vysoce interaktivní prostředí. Uživatele můžete pracovat s ad a ad můžete odpovědět na akce v prohlížeči. Například ad může zobrazit tlačítka, která umožňují uživatelům zobrazit další informace a delší verze ad. Přehrávače videa musí podporovat rozhraní API VPAID a spustitelný ad musí implementovat rozhraní API. Když přehrávač požadavky že AD ze serveru ad serveru odpoví OBROVSKÉ odpověď, která obsahuje VPAID ad.

Spustitelný ad se vytvoří v kódu, který je nutné provést v prostředí runtime například Adobe Flash™ nebo JavaScript, které mohou být provedeny ve webovém prohlížeči. Pokud ad server vrátí OBROVSKÉ odpověď obsahující VPAID ad, hodnotu apiFramework atribut <MediaFile> prvek musí být "VPAID". Tenhle atribut určuje, že obsažené ad spustitelný ad VPAID. Typ atributu musí nastavit typ MIME spustitelný soubor, například "application/x-shockwave-flash" nebo "aplikace/x-javascript". Následující úryvek XML <MediaFile> element z OBROVSKÉ odpověď obsahující spustitelný ad VPAID. 

    
    <MediaFiles>
       <MediaFile id="1" delivery="progressive" type=”application/x-shockwaveflash”
                  width=”640” height=”480” apiFramework=”VPAID”>
           <!-- CDATA wrapped URI to executable ad -->
       </MediaFile>
    </MediaFiles>
 

Spustitelný ad může být inicializována pomocí <AdParameters> prvek v <Linear> nebo <NonLinear> prvky v odpovědi na velké. Další informace o <AdParameters> prvek, najdete v článku [OBROVSKÉ 3.0](http://www.iab.net/media/file/VASTv3.0.pdf). Další informace o rozhraní API VPAID najdete v tématu [VPAID 2.0](http://www.iab.net/media/file/VPAID_2.0_Final_04-10-2012.pdf).

##<a name="implementing-a-windows-or-windows-phone-8-player-with-ad-support"></a>Provádění s Windows nebo Windows Phone 8 přehrávače s podporou Ad

Platformu Microsoft Media: Přehrávače Framework pro Windows 8 a Windows Phone 8 obsahuje kolekci ukázkové aplikace, které ukazují, jak implementovat přehrávače videa aplikace pomocí rozhraní. Přehrávač Framework a vzorky si můžete stáhnout z [přehrávače Framework pro Windows 8 a Windows Phone 8](https://playerframework.codeplex.com).

Při otevření řešení Microsoft.PlayerFramework.Xaml.Samples uvidíte počtem složek v rámci projektu. Složka reklamě obsahuje důležité pro vytvoření přehrávače videa s podporou ad ukázkový kód. Uvnitř reklamu je složka počet XAML/cs souborů z nich ukazují, jak vložit reklam jiným způsobem. Následující seznam popisuje všechny:

- AdPodPage.xaml ukazuje, jak zobrazit ad pod.
- AdSchedulingPage.xaml ukazuje, jak naplánovat služby Active Directory.
- FreeWheelPage.xaml ukazuje, jak používat modul plug-in FreeWheel k plánování služby Active Directory.
- MastPage.xaml ukazuje, jak naplánovat služby Active Directory se souborem STOŽÁRŮ.
- ProgrammaticAdPage.xaml ukazuje, jak programově naplánování reklam vytvářet video.
- ScheduleClipPage.xaml ukazuje, jak naplánovat ad bez OBROVSKÉ souboru.
- VastLinearCompanionPage.xaml ukazuje, jak vložit lineární a ad Průvodce vyhledáváním.
- VastNonLinearPage.xaml ukazuje, jak vložit nelineárních ad.
- VmapPage.xaml ukazuje, jak můžete určit služby Active Directory pomocí souboru VMAP.

Každý z těchto vzorků používá třídu MediaPlayer definované rámcem player. Většina vzorky pomocí moduly plug-in pro, přidat podporu pro různé formáty ad odpověď. Ukázka ProgrammaticAdPage programově vzájemně MediaPlayer instance.

###<a name="adpodpage-sample"></a>Ukázka AdPodPage

Tento příklad používá AdSchedulerPlugin k definování při zobrazování reklam. V tomto příkladu je naplánováno polovině zahrnout reklamní přehrát po 5 sekund. Pod ad (skupiny služby Active Directory zobrazíte v pořadí) jsou zadány v souboru OBROVSKÉ vrácených ad server. Identifikátor URI OBROVSKÉ soubor podle <RemoteAdSource> prvek.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
    
        <mmppf:MediaPlayer.Plugins>
            <ads:AdSchedulerPlugin>
                <ads:AdSchedulerPlugin.Advertisements>
    
                    <ads:MidrollAdvertisement Time="00:00:05">
                        <ads:MidrollAdvertisement.Source>
                            <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_adpod.xml" Type="vast"/>
                        </ads:MidrollAdvertisement.Source>
                    </ads:MidrollAdvertisement>
    
                </ads:AdSchedulerPlugin.Advertisements>
            </ads:AdSchedulerPlugin>
            <ads:AdHandlerPlugin/>
        </mmppf:MediaPlayer.Plugins>
    </mmppf:MediaPlayer>

Další informace o AdSchedulerPlugin najdete v tématu [reklamu v rámci přehrávače ve Windows 8 a Windows Phone 8](http://playerframework.codeplex.com/wikipage?title=Advertising&referringTitle=Windows%208%20Player%20Documentation)

###<a name="adschedulingpage"></a>AdSchedulingPage

Tento příklad používá také AdSchedulerPlugin. Plánuje tři reklam, před zahrnout ad, střední zahrnout ad a ad po zahrnout. Identifikátor URI VAST pro každou reklamu podle <RemoteAdSource> prvek.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:PrerollAdvertisement>
                                <ads:PrerollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:PrerollAdvertisement.Source>
                            </ads:PrerollAdvertisement>
    
                            <ads:MidrollAdvertisement Time="00:00:15">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
    
                            <ads:PostrollAdvertisement>
                                <ads:PostrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:PostrollAdvertisement.Source>
                            </ads:PostrollAdvertisement>
    
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>


###<a name="freewheelpage"></a>FreeWheelPage

V tomto příkladu FreeWheelPlugin, která určuje atribut zdroj, který určuje URI odkazující na SmartXML soubor, který určuje ad obsahu, jakož i ad informace o plánování.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:FreeWheelPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/freewheel.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="mastpage"></a>MastPage

V tomto příkladu MastSchedulerPlugin, kterou můžete použít soubor STOŽÁRŮ. Zdrojový atribut určuje umístění souboru STOŽÁRŮ.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:MastSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/mast.xml" />
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="programmaticadpage"></a>ProgrammaticAdPage

V tomto příkladu programově vzájemně MediaPlayer. Soubor ProgrammaticAdPage.xaml vytvoří MediaPlayer:

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4"/>

Soubor ProgrammaticAdPage.xaml.cs vytvoří AdHandlerPlugin přidá TimelineMarker můžete určit, když ad mají být zobrazeny a potom je přidá obslužné rutiny události MarkerReached, která načte RemoteAdSource určující URI OBROVSKÉ soubor a potom přehrává ad.
    
    public sealed partial class ProgrammaticAdPage : Microsoft.PlayerFramework.Samples.Common.LayoutAwarePage
        {
            AdHandlerPlugin adHandler;
    
            public ProgrammaticAdPage()
            {
                this.InitializeComponent();
                adHandler = new AdHandlerPlugin();
                player.Plugins.Add(new AdHandlerPlugin());
                player.Markers.Add(new TimelineMarker() { Time = TimeSpan.FromSeconds(5), Type = "myAd" });
                player.MarkerReached += pf_MarkerReached;
            }
    
            async void pf_MarkerReached(object sender, TimelineMarkerRoutedEventArgs e)
            {
                if (e.Marker.Type == "myAd")
                {
                    var adSource = new RemoteAdSource() { Type = VastAdPayloadHandler.AdType, Uri = new Uri("http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml") };
                    //var adSource = new AdSource() { Type = DocumentAdPayloadHandler.AdType, Payload = SampleAdDocument };
                    var progress = new Progress<AdStatus>();
                    try
                    {
                        await player.PlayAd(adSource, progress, CancellationToken.None);
                    }
                    catch { /* ignore */ }
                }
            }

###<a name="scheduleclippage"></a>ScheduleClipPage

Tento příklad používá AdSchedulerPlugin k naplánování polovině zahrnout ad zadáním WMV soubor, který obsahuje ad.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.cloudapp.net/html5/media/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:AdSource Type="clip">
                                        <ads:AdSource.Payload>
                                            <ads:ClipAdPayload MediaSource="http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_700_4x3.wmv" MimeType="video/x-ms-wmv" />
                                        </ads:AdSource.Payload>
                                    </ads:AdSource>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
    
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="vastlinearcompanionpage"></a>VastLinearCompanionPage

Tento příklad ukazuje, jak používat AdSchedulerPlugin naplánování polovině zahrnout lineární ad se službou ad Průvodce vyhledáváním. <RemoteAdSource> Element určuje OBROVSKÉ soubor.
    
    <mmppf:MediaPlayer Grid.Row="1"  x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear_companions.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
    
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="vastlinearnonlinearpage"></a>VastLinearNonLinearPage

Tento příklad používá AdSchedulerPlugin naplánování lineární a nelineárních ad. Zadaným OBROVSKÉ umístění <RemoteAdSource> prvek.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear_nonlinear.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
                            
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="vmappage"></a>VMAPPage

Tento ukázky používá VmapSchedulerPlugin k plánování služby Active Directory pomocí souboru VMAP. Identifikátor URI souboru VMAP podle zdrojovou vlastnost <VmapSchedulerPlugin> prvek.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:VmapSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/vmap.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

##<a name="implementing-an-ios-video-player-with-ad-support"></a>Provádění iOS přehrávače videa s podporou Ad


Platformu Microsoft Media: Přehrávače rámec pro iOS obsahuje kolekci ukázkové aplikace, které ukazují, jak implementovat přehrávače videa aplikace pomocí rozhraní. Přehrávač Framework a vzorky si můžete stáhnout z [Azure Media Player Framework](https://github.com/Azure/azure-media-player-framework). Stránka github má odkaz wikiwebu, která obsahuje další informace o rozhraní přehrávače a úvod do výběru přehrávače: [Azure Media Player wikiwebu](https://github.com/Azure/azure-media-player-framework/wiki/How-to-use-Azure-media-player-framework).


###<a name="scheduling-ads-with-vmap"></a>Plánování služby Active Directory s VMAP

Následující příklad ukazuje, jak naplánovat služby Active Directory pomocí souboru VMAP.

    // How to schedule an Ad using VMAP.
    //First download the VMAP manifest
    
    if (![framework.adResolver downloadManifest:&manifest withURL:[NSURL URLWithString:@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVMAP.xml"]])
            {
                [self logFrameworkError];
            }
            else
            {
                // Schedule a list of ads using the downloaded VMAP manifest
                if (![framework scheduleVMAPWithManifest:manifest])
                {
                    [self logFrameworkError];
                }          
            }

###<a name="scheduling-ads-with-vast"></a>Plánování služby Active Directory s VAST

Následující příklad ukazuje, jak naplánovat zpožděné OBROVSKÉ ad vazby.
    
    //Example:3 How to schedule a late binding VAST ad.
    // set the start time for the ad
    adLinearTime.startTime = 13;
    adLinearTime.duration = 0;
    // Specify the URI of the VAST file
    NSString *vastAd1=@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml";
    // Create an AdInfo object
     AdInfo *vastAdInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URL to VAST file
    vastAdInfo1.clipURL = [NSURL URLWithString:vastAd1];
    // set running time of ad
    vastAdInfo1.mediaTime = [[[MediaTime alloc] init] autorelease];
    vastAdInfo1.mediaTime.clipBeginMediaTime = 0;
    vastAdInfo1.mediaTime.clipEndMediaTime = 10;
    vastAdInfo1.policy = @"policy for late binding VAST";
    // specify ad type
    vastAdInfo1.type = AdType_Midroll;
    vastAdInfo1.appendTo=-1;
    adIndex = 0;
    // schedule ad
    if (![framework scheduleClip:vastAdInfo1 atTime:adLinearTime forType:PlaylistEntryType_VAST andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }
         
   Následující příklad ukazuje, jak naplánování nejdříve možné OBROVSKÉ ad vazby.
Příklad: 4 plánu nejdříve možné //Download OBROVSKÉ ad vazby VAST souboru, pokud (! [ framework.adResolver downloadManifest: & seznamu withURL: [NSURL URLWithString:@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml"]]) {[vlastní logFrameworkError];} dalšího {adLinearTime.startTime = 7; adLinearTime.duration = 0;
        
        // Create AdInfo instance
        AdInfo *vastAdInfo2 = [[[AdInfo alloc] init] autorelease];
        vastAdInfo2.mediaTime = [[[MediaTime alloc] init] autorelease];
        vastAdInfo2.policy = @"policy for early binding VAST";
        // specify ad type
        vastAdInfo2.type = AdType_Midroll;
        vastAdInfo2.appendTo=-1;
        // schedule ad
        if (![framework scheduleVASTClip:vastAdInfo2 withManifest:manifest atTime:adLinearTime andGetClipId:&adIndex])
        {
            [self logFrameworkError];
        }
    }

Následující příklad ukazuje, jak vložit ad pomocí hrubé vyjmout úpravy (RCE)

    //Example:1 How to use RCE.
    // specify manifest for ad content
    NSString *secondContent=@"http://wamsblureg001orig-hs.cloudapp.net/6651424c-a9d1-419b-895c-6993f0f48a26/The%20making%20of%20Microsoft%20Surface-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    
    // specify ad length
    mediaTime.currentPlaybackPosition = 0;
    mediaTime.clipBeginMediaTime = 0;
    mediaTime.clipEndMediaTime = 80;
    // append ad content
    if (![framework appendContentClip:[NSURL URLWithString:secondContent] withMediaTime:mediaTime andGetClipId:&clipId])
    {
        [self logFrameworkError];
    }

Následující příklad ukazuje, jak naplánovat ad pod.

    //Example:5 Schedule an ad Pod.
    // Set start time for ad
    adLinearTime.startTime = 23;
    adLinearTime.duration = 0;
    
    // Specify URL to content
    NSString *adpodSt1=@"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    // Create an AdInfo instance
    AdInfo *adpodInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URI to ad content
    adpodInfo1.clipURL = [NSURL URLWithString:adpodSt1];
    // Set ad running time
    adpodInfo1.mediaTime = [[[MediaTime alloc] init] autorelease];
    adpodInfo1.mediaTime.clipBeginMediaTime = 0;
    adpodInfo1.mediaTime.clipEndMediaTime = 17;
    adpodInfo1.policy = @"policy for ad pod 1";
    // Set ad type
    adpodInfo1.type = AdType_Midroll;
    adpodInfo1.appendTo=-1;
    adIndex = 0;
    // Schedule ad
    if (![framework scheduleClip:adpodInfo1 atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

Následující příklad ukazuje, jak naplánovat – rychlé ad polovině zahrnout. Bez rychlé ad pouze přehraje po bez ohledu na libovolnou hledání v prohlížeči provádí.
    
    //Example:6 Schedule a single non sticky mid roll Ad
    // specify URL to content
    NSString *oneTimeAd=@"http://wamsblureg001orig-hs.cloudapp.net/5389c0c5-340f-48d7-90bc-0aab664e5f02/Windows%208_%20You%20and%20Me%20Together-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    
    // create an AdInfo instance
    AdInfo *oneTimeInfo = [[[AdInfo alloc] init] autorelease];
    // set URL of ad
    oneTimeInfo.clipURL = [NSURL URLWithString:oneTimeAd];
    oneTimeInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    oneTimeInfo.mediaTime.clipBeginMediaTime = 0;
    oneTimeInfo.mediaTime.clipEndMediaTime = -1;
    oneTimeInfo.policy = @"policy for one-time ad";
    // set ad start time
    adLinearTime.startTime = 43;
    adLinearTime.duration = 0;
    // set ad type
    oneTimeInfo.type = AdType_Midroll;
    // non sticky ad
    oneTimeInfo.deleteAfterPlayed = YES;
    // schedule ad
    if (![framework scheduleClip:oneTimeInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

Následující příklad ukazuje, jak naplánovat rychlé ad polovině zahrnout. Rychlé ad zobrazí pokaždé, když se dosáhne určité bodu na časové ose videa.

    //Example:7 Schedule a single sticky mid roll Ad
    NSString *stickyAd=@"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *stickyAdInfo = [[[AdInfo alloc] init] autorelease];
    // set URI to ad
    stickyAdInfo.clipURL = [NSURL URLWithString:stickyAd];
    stickyAdInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    stickyAdInfo.mediaTime.clipBeginMediaTime = 0;
    stickyAdInfo.mediaTime.clipEndMediaTime = 15;
    stickyAdInfo.policy = @"policy for sticky mid-roll ad";
    // set ad start time
    adLinearTime.startTime = 64;
    adLinearTime.duration = 0;
    // set ad type
    stickyAdInfo.type = AdType_Midroll;
    stickyAdInfo.deleteAfterPlayed = NO;
    // schedule ad
    if (![framework scheduleClip:stickyAdInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }


Následující příklad ukazuje, jak naplánovat ad po zahrnout.

    //Example:8 Schedule Post Roll Ad
    NSString *postAdURLString=@"http://wamsblureg001orig-hs.cloudapp.net/aa152d7f-3c54-487b-ba07-a58e0e33280b/wp-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *postAdInfo = [[[AdInfo alloc] init] autorelease];
    postAdInfo.clipURL = [NSURL URLWithString:postAdURLString];
    postAdInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    postAdInfo.mediaTime.clipBeginMediaTime = 0;
    // set ad length
    postAdInfo.mediaTime.clipEndMediaTime = 45;
    postAdInfo.policy = @"policy for post-roll ad";
    // set ad type
    postAdInfo.type = AdType_Postroll;
    adLinearTime.duration = 0;
    if (![framework scheduleClip:postAdInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

Následující příklad ukazuje, jak naplánovat před zahrnout ad.
    
    //Example:9 Schedule Pre Roll Ad
    NSString *adURLString = @"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    AdInfo *adInfo = [[[AdInfo alloc] init] autorelease];
    adInfo.clipURL = [NSURL URLWithString:adURLString];
    adInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    adInfo.mediaTime.currentPlaybackPosition = 0;
    adInfo.mediaTime.clipBeginMediaTime = 40; //You could play a portion of an Ad. Yeh!
    adInfo.mediaTime.clipEndMediaTime = 59;
    adInfo.policy = @"policy for pre-roll ad";
    adInfo.appendTo = -1;
    adInfo.type = AdType_Preroll;
    adLinearTime.duration = 0;
    // schedule ad
    if (![framework scheduleClip:adInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

Následující příklad ukazuje, jak naplánovat překryvný uprostřed zahrnout ad.
    
    // Example10: Schedule a Mid Roll overlay Ad
    NSString *adURLString = @"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    //Create AdInfo instance
    AdInfo *adInfo = [[[AdInfo alloc] init] autorelease];
    adInfo.clipURL = [NSURL URLWithString:adURLString];
    adInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    adInfo.mediaTime.currentPlaybackPosition = 0;
    adInfo.mediaTime.clipBeginMediaTime = 0;
    // specify ad length
    adInfo.mediaTime.clipEndMediaTime = 20;
    adInfo.policy = @"policy for mid-roll overlay ad";
    adInfo.appendTo = -1;
    // specify ad type
    adInfo.type = AdType_Midroll;
    // specify ad start time & duration
    adLinearTime.startTime = 300;
    adLinearTime.duration = 20;
    // schedule ad            if (![framework scheduleClip:adInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }



##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

 
##<a name="see-also"></a>Viz taky

[Můžete vyvíjet aplikace přehrávače videa](media-services-develop-video-players.md)
