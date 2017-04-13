<properties 
    pageTitle="Poznámky k verzi Media Services | Microsoft Azure" 
    description="Mediální služby poznámky k verzi" 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="media" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="juliako"/>

# <a name="azure-media-services-release-notes"></a>Poznámky k verzi Azure Media Services

Tyto poznámky k verzi sumarizovat změny z předchozích verzí a známé problémy.

>[AZURE.NOTE] Chcete slyšet z jejích zákazníků a zaměření se na řešení problémů, které ovlivňují můžete. Pokud chcete ohlásit problém nebo klást otázky, publikovat na [Fórum MSDN Azure Media Services].


##<a id="issues"></a>Aktuálně známé problémy

### <a id="general_issues"></a>Mediální služby Obecné problémy

Problém|Popis
---|---
Několik běžných HTTP záhlaví nezobrazují v rozhraní REST API.|Pokud se zabývají vývojem aplikací Media Services pomocí rozhraní REST API, zjistíte, že některé běžné záhlaví pole HTTP (včetně klienta žádost o ID, ID žádosti a VRÁCENÁ klienta žádost o ID) nejsou podporované. Záhlaví se přidají v budoucích aktualizace.
Heslo PERCENT-encoding není povolená.|Media Services použije hodnotu vlastnost IAssetFile.Name při vytváření adresy URL pro streamování obsahu (například http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Z tohoto důvodu heslo percent-encoding není povolená. Hodnoty **název** vlastnosti nemůže obsahovat žádný z následujících [znaků procent kódování vyhrazené](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Navíc může být pouze jednu "." pro přípona názvu souboru.
Metoda ListBlobs, která je součástí Azure úložiště SDK verze 3.x dojde k chybě.|Media Services generují adresy URL přidružení zabezpečení na základě verze [2012 02 12](http://msdn.microsoft.com/library/azure/dn592123.aspx) . Pokud chcete použít SDK Azure úložiště objektů BLOB seznamu v kontejneru objektů blob, použijte metodu [CloudBlobContainer.ListBlobs](http://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobs.aspx) , která je součástí verzi Azure úložiště SDK 2.x. Metoda ListBlobs, která je součástí verzi Azure úložiště SDK 3.x nezdaří.
Media Services omezení mechanismus omezuje využití prostředků pro aplikace, díky kterým nadbytečné žádosti o služby. Služba může vrátit stavový kód služba není k dispozici (503) HTTP.|Další informace najdete v tématu Popis stavový kód 503 HTTP v tématu [Kódy chyb Azure Media Services](http://msdn.microsoft.com/library/azure/dn168949.aspx) .
Při dotazování entity, je omezení na 1000 entity vrátit najednou, protože veřejné v2 ZBÝVAJÍCÍ omezení výsledků dotazu 1000 výsledky. | Je potřeba použít **Přeskočit** a **Prohlédněte** (.NET) / **horní** (REST) jako popsaných v [tomto příkladu .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) a [v tomto příkladu rozhraní REST API](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). 
V některých klientech můžou pocházet přes problém opakování značku v manifestu hladký přenos.|Další informace najdete v tématu [v této](media-services-deliver-content-overview.md#known-issues) části.
Azure Media Services .NET SDK objekty nemůžete řadit a jako výsledek nepracují při ukládání do mezipaměti Azure.|Pokud se pokusíte serializovat SDK AssetCollection objekt, který chcete přidat do mezipaměti Azure, k výjimce.
Kódování úlohy nezdaří řetězcem zprávu "fázi: DownloadFile. Kód: System.NullReferenceException ".|Typické kódování pracovního postupu je nahrajte vstupní videa soubory aktivum zadávání a odeslat jeden nebo více kódování úlohy pro vstupní majetek, bez dalších provádění úprav, zadávání materiálů. Ale pokud úpravou vstupní materiálů (například tak, že přidání nebo odstranění a přejmenování souborů v rámci majetku) potom následující úlohy nemusí s chybou DownloadFile. Toto alternativní řešení je odstranit vstupní materiálů a znovu odeslat vstupní souborů do nového materiálů. 

##<a id="rest_version_history"></a>Historie REST API

Informace o historii verzí Media Services REST API najdete v článku Principy [Azure Media Services ZBÝVAJÍCÍ rozhraní API].

##<a id="july_changes16"></a>Uvolnění červenec 2016

###<a name="updates-to-manifest-file-ism-generated-by-encoding-tasks"></a>Aktualizace se projeví souboru (*. Správci služeb sítě Internet) generovaných kódování úkoly

Po odeslání kódování úkolu Media Encoder standardní nebo Azure Media Encoder kódování úkolu vygeneruje [streamování seznamu souboru](media-services-deliver-content-overview.md) (* .ism) souboru do výstupu materiálů. Nejnovější verze služby byl aktualizován syntaxe tohoto streamování seznamu souboru.

>[AZURE.NOTE]Syntaxe streamování manifestu (.ism) soubor je rezervován pro interní potřebu a je předmětem změnu v budoucích verzích. Zkontrolujte Neupravujte a měnit obsah tohoto souboru.

###<a name="a-new-client-manifest-ismc-file-is-generated-in-the-output-asset-when-an-encoding-task-outputs-one-or-more-mp4-files"></a>Nový klient manifest (*. Vygeneruje se soubor ISMC) do výstupu majetku při kódování úkolu výstupy jeden nebo víc souborů MP4

Začínající na nejnovější verzi služby po skončení kódování úkol, který vytváří jeden další soubory MP4, výstup materiálů obsahoval také streamování souboru manifestu (*.ismc) klienta. Soubor .ismc pomůže zvýšit výkon dynamické streamování. 

>[AZURE.NOTE]Syntaxe soubor manifestu (.ismc) klienta je vyhrazena pro interní potřebu a je předmětem změnu v budoucích verzích. Zkontrolujte Neupravujte a měnit obsah tohoto souboru.

Další informace najdete v tématu [Tento](https://blogs.msdn.microsoft.com/randomnumber/2016/07/08/encoder-changes-within-azure-media-services-now-create-ismc-file/) blogu.

### <a name="known-issues"></a>Známé problémy

V některých klientech můžou pocházet aross problém opakování značku v manifestu hladký přenos. Další informace najdete v tématu [v této](media-services-deliver-content-overview.md#known-issues) části.

##<a id="apr_changes16"></a>2016 s dubnovou vydání

### <a name="azure-media-analytics"></a>Azure Media analýzy

Azure Media Servces zavádí Azure Media Analytics pro výkonné videa měřítka. Podrobné informace najdete v tématu [Přehled Azure Media Services technologie pro analýzu](media-services-analytics-overview.md).

### <a name="apple-fairplay-preview"></a>Apple FairPlay (verze Preview)

Azure Media Services umožňuje dynamicky šifrování vaší HTTP Live datových proudů (HLS) s Apple FairPlay obsahu. Můžete také služba pro doručování AMS licence k předvádění FairPlay licence pro klienty. Podrobnější informace najdete v tématu [použití Azure Media Services stáhnete vaší HLS obsahu chráněného s Apple FairPlay ](media-services-protect-hls-with-fairplay.md).
  
##<a id="feb_changes16"></a>Uvolnění únor 2016

Nejnovější verzi Azure Media Services SDK pro .NET (3.5.3) obsahuje Widevine související oprava chyb. Problém byl: AssetDeliveryPolicy nelze znovu použít pro více prostředky zašifrovaných Widevine. Jako součást oprava chyb následující vlastnost jsme přidali SDK: **WidevineBaseLicenseAcquisitionUrl**.
    
    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
    {
        {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl,"http://testurl"},
        
    };

##<a id="jan_changes_16"></a>Leden 2016 vydání

Kódování rezervovaná jednotky přejmenovat snížit zapomíná a to s názvy Encoder.

K S1, S2, jsou přejmenované Basic, Standard a Premium kódování rezervovaná jednotky a S3 vyhrazená jednotky, respektive.  Zákazníkům, kteří používají základní kódování RUs dnes vidíte S1 jako popisek v Azure portál (a účtu), při standardní a Premium, uvidí popisky S2 a S3 v tomto pořadí. 

##<a id="dec_changes_15"></a>Uvolnění 2015 prosinec

Tým Azure SDK publikovat novou verzi [Azure SDK PHP](http://github.com/Azure/azure-sdk-for-php) balíček, který obsahuje aktualizace a nových funkcí pro Microsoft Azure Media Services. Zejména Azure Media Services SDK pro PHP nyní podporuje nejnovější funkce [ochranu obsahu](media-services-content-protection-overview.md) : dynamické šifrování AES a DRM (PlayReady a Widevine) mezi šablonami a bez omezení Token. Podporuje také měřítka [Kódování jednotky](media-services-dotnet-encoding-units.md).

Další informace najdete v tématu:

- Blog [Microsoft Azure Media Services SDK PHP](http://southworks.com/blog/2015/12/09/new-microsoft-azure-media-services-sdk-for-php-release-available-with-new-features-and-samples/) .
- Následující [Ukázky](http://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices) vám pomůžou začít rychle:
    - **vodworkflow_aes.php**: Toto je PHP soubor, který ukazuje, jak používat dynamické šifrování AES 128 a služba pro doručování klíče. Je založena na ukázku .NET popsaných v [tomto](media-services-protect-with-aes128.md) článku.
    - **vodworkflow_aes.php**: Toto je PHP soubor, který ukazuje, jak používat dynamické šifrování PlayReady a služba pro doručování licenci. Je založena na ukázku .NET popsaných v [tomto](media-services-protect-with-drm.md) článku.
    - **scale_encoding_units.php**: Toto je PHP soubor, který ukazuje, jak zobrazit kódování rezervovaná jednotku.


##<a id="nov_changes_15"></a>Uvolnění listopad 2015

Azure Media Services nyní nabízí služba pro doručování licenci Google Widevine v cloudu. Další informace najdete v příručce [Toto oznámení blogu](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/). Naleznete v [tomto kurzu](media-services-protect-with-drm.md) a [GitHub úložiště](http://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm). 

Všimněte si, že Widevine licence doručení služby Azure Media Sevices ve verzi preview. Další informace najdete v [tomto blogu](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/).

##<a id="oct_changes_15"></a>Uvolnění říjen 2015

Azure Media Services (AMS) je teď živou následující datacentrech: Brazílie jih Indie západní, Indie jih a Indie centrální. Nyní můžete používat portál klasické Azure k [Vytvoření účtů služeb média](media-services-portal-create-account.md) a dělat nejrůznější věci popsané [v tomto poli](https://azure.microsoft.com/documentation/services/media-services/). Však Live kódování není povolený tyto datacentrech. Kromě toho ne všechny typy zboží kódování rezervovaná jsou dostupné v těchto datacentrech.

- Brazílie jih: Standardních a základní kódování rezervovaná jednotky jsou k dispozici pouze
- Indie Západní Indie jih a Indie centrální: pouze základní kódování rezervovaná jednotky jsou k dispozici


##<a id="september_changes_15"></a>Uvolnění září 2015 

- AMS nyní nabízí možnost Zamknout živou proudy technologií moduly DRM Widevine a poskytují (VOD). Můžete použít následující služby partnery doručení vám poskytovat Widevine licence: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/) [castLabs](http://castlabs.com/company/partners/azure/). Další informace najdete v tématu [Tento blogu](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/).

    Můžete [AMS.NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (počínaje číslem verze 3.5.1) nebo rozhraní REST API pro nastavení vaší AssetDeliveryConfiguration používat Widevine.  

- AMS přidá podpory pro Apple ProRes videa. Teď můžete nahrávat QuickTime zdrojové soubory videa, které používají Apple ProRes nebo další kodeky. Další informace najdete v tématu [Tento blogu](https://azure.microsoft.com/blog/announcing-support-for-apple-prores-videos-in-azure-media-services/).

- Teď můžete Media Encoder standardní dělat podřízeného výřez a živou extrahování archivu. Další informace najdete v tématu [Tento blogu](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).

- Následující filtrování aktualizace provedené: 

    - Teď můžete Apple HTTP Live datových proudů (HLS) formát s filtrem jenom hlasový přenos. Tato aktualizace umožňuje odebrat jenom hlasový přenos sledování zadáním (jenom hlasový přenos = false) v adrese URL.
    - Při definování filtrů pro svém majetku nyní máte možnost kombinace více (až 3) filtry v jedné adresy URL.

    Další informace najdete v [tomto](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blogu.

- Můžu rámce AMS nyní podporuje v HLS v4. Můžu snímku podpory optimalizuje operace rychlé převíjení vpřed nebo vzad. Ve výchozím nastavení zahrnovat všechny výstupy v4 HLS stop můžu snímku (EXT-X-I-FRAME-STREAM-INF).
 
    Další informace najdete v [tomto](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blogu.

##<a id="august_changes_15"></a>Uvolnění srpen 2015

- Azure Media Services SDK Java V0.8.0 vydání a nové ukázky jsou teď k dispozici. Další informace najdete v tématu:

    - [Příspěvek na blog](http://southworks.com/blog/2015/08/25/microsoft-azure-media-services-sdk-for-java-v0-8-0-released-and-new-samples-available/)
    - [Úložiště ukázky Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- Azure Media Playeru aktualizace s více zvukových toku podpory. Další informace najdete v tématu:
    - [Příspěvek na blog](https://azure.microsoft.com/blog/2015/08/13/azure-media-player-update-with-multi-audio-stream-support/)

##<a id="july_changes_15"></a>Uvolnění červenec 2015

- Oznámení o obecné dostupnosti Media Encoder standardní. Další informace najdete v tématu [Tento příspěvek blogu](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/).

    Media Encoder standardní používá předvolby popsaná v [této](http://go.microsoft.com/fwlink/?LinkId=618336) části. Všimněte si, že při použití předvolby pro 4k kódování by měla získat typ **Premium** vyhrazená jednotky. Další informace najdete v tématu [jak kódování měřítko](media-services-scale-media-processing-overview.md).
- Živé v reálném čase seznam titulků s Azure Media Services a Player. Další informace najdete v tématu [Tento příspěvek blogu](https://azure.microsoft.com/blog/2015/07/08/live-real-time-captions-with-azure-media-services-and-player/)

###<a name="media-services-net-sdk-updates"></a>Mediální služby .NET SDK aktualizace

Azure Media Services .NET SDK je teď verze 3.4.0.0. Následující funkce byly přidány v této verzi:  

- Implementace podpora živou archivu. Všimněte si, že nejde stáhnout aktiva obsahující živou archivu.
- Implementace podpora dynamických filtrů.
- Implementace funkcí, které umožňuje uživatelům zachovat kontejner úložiště při odstraňování materiálů.
- Opravy chyb souvisejících s opakovat zásady kanálů.
- Povolit **Media Encoder Premium pracovního postupu**.

##<a id="june_changes_15"></a>Uvolnění června 2015

###<a name="media-services-net-sdk-updates"></a>Mediální služby .NET SDK aktualizace

Azure Media Services .NET SDK je teď verze 3.3.0.0. Následující funkce byly přidány v této verzi:  

- Podpora pro zjišťování připojení OpenId specifikace
- Podpora pro zpracování klíče při přechodu myší na straně poskytovatele identity. 

Pokud používáte zprostředkovatelem identit, která poskytuje OpenID připojení dokumentu zjišťování (stejně jako v těchhle poskytovatelů: služby Azure Active Directory, Google, Salesforce), můžete určit, aby Azure Media Services získat podpisový klíče ověření JWT token z OpenID připojení zjišťování specifikace. 

Další informace najdete v tématu [Použití Json Web kláves sady připojení OpenID zjišťování specifikace pro práci s JWT tokenu ověřování v Azure Media Services](http://gtrifonov.com/2015/06/07/using-json-web-keys-from-openid-connect-discovery-spec-to-work-with-jwt-token-authentication-in-azure-media-services/).


##<a id="may_changes_15"></a>Vydání květen 2015

Oznámení o následující nové funkce:

- [Náhled Live kódování s Media Services](media-services-manage-live-encoder-enabled-channels.md)
- [Dynamické manifestu](media-services-dynamic-manifest-overview.md)
- [Náhled Azure Media Hyperlapse médií procesor](https://azure.microsoft.com/blog/?p=286281&preview=1&_ppp=61e1a0b3db)

##<a id="april_changes_15"></a>Uvolnění duben 2015

 ###<a name="general-media-services-updates"></a>Obecné mediální služby aktualizací

- [Oznámení o Azure Media Player](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/).
- Počínaje Media Services ZBÝVAJÍCÍ 2.10 kanály, které není nakonfigurován jedí protokol RTMP se vytvářejí primární a sekundární jedí adresy URL. Další informace najdete v tématu [Konfigurace jedí kanálu](media-services-live-streaming-with-onprem-encoders.md#channel_input)
- Aktualizuje Azure Media indexování
- Podpora pro španělskou
- Nový formát xml konfigurace

Další informace najdete v [tomto blogu](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/).
###<a name="media-services-net-sdk-updates"></a>Mediální služby .NET SDK aktualizace

Azure Media Services .NET SDK je teď verze 3.2.0.0.

Tady jsou uvedené některé zákazníků protilehlé aktualizace:

- **Zrušení změn**: změnit **TokenRestrictionTemplate.Issuer** a **TokenRestrictionTemplate.Audience** být typu řetězec.
- Aktualizace týkající se vytváření vlastních opakovat zásady.
- Opravy chyb souvisejících s nahrávání nebo stahování souborů.
- Třídy **MediaServicesCredentials** teď přijme koncový bod řízení primárních a sekundárních přístup k ověřování.



##<a id="march_changes_15"></a>Uvolnění březnem 2015

### <a name="general-media-services-updates"></a>Obecné mediální služby aktualizací

- Media Services nyní obsahuje Azure CDN integrace. K podpoře integrace, jsme přidali vlastnost **CdnEnabled** **StreamingEndpoint**.  **CdnEnabled** se dá používat se REST API počínaje verzí 2.9 (Další informace najdete v tématu [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx)).  **CdnEnabled** se dá používat se .NET SDK od verze 3.1.0.2 (Další informace najdete v tématu [StreamingEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.istreamingendpoint(v=azure.10).aspx)).
- Oznámení o dítěte **Media Encoder Premium**pracovního postupu. Další informace najdete v tématu [Představení Premium kódování v Azure Media Services](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/).
 


##<a id="february_changes_15"></a>Uvolnění únor 2015

### <a name="general-media-services-updates"></a>Obecné mediální služby aktualizací

Media Services REST API je teď verze 2.9. Začnete s touto verzí, můžete povolit Azure CDN integrace s streamování koncové body. Další informace najdete v tématu [StreamingEndpoint](https://msdn.microsoft.com/library/dn783468.aspx).

##<a id="january_changes_15"></a>Uvolnění ledna 2015

### <a name="general-media-services-updates"></a>Obecné mediální služby aktualizací

Oznámení o obecné dostupnosti (GA) ochrany obsahu pomocí dynamického šifrování. Další informace najdete v tématu [Azure Media Services zlepšuje streamování cenného papíru s technologie obecné dostupnost DRM](https://azure.microsoft.com/blog/2015/01/29/azure-media-services-enhances-streaming-security-with-general-availability-of-drm-technology/).

###<a name="media-services-net-sdk-updates"></a>Mediální služby .NET SDK aktualizace

Azure Media Services .NET SDK je teď verze 3.1.0.1.

Tato verze označené výchozí konstruktor Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization.TokenRestrictionTemplate jako zastaralé. Nový konstruktor trvá TokenType jako argument.

    TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);


##<a id="december_changes_14"></a>Uvolnění prosince 2014

###<a name="general-media-services-updates"></a>Obecné mediální služby aktualizací

- Některé aktualizace a nových funkcí jsme přidali procesor Azure Media indexování. Další informace najdete v tématu [Poznámky k verzi Azure Media indexování verze 1.1.6.7](https://azure.microsoft.com/blog/2014/12/03/azure-media-indexer-version-1-1-6-7-release-notes/).
- Přidali nového rozhraní REST API, který umožňuje aktualizovat kódování vyhrazená jednotky: [EncodingReservedUnitType s ZBÝVAJÍCÍ](http://msdn.microsoft.com/library/azure/dn859236.aspx).
- Přidané CORS podporu služba pro doručování klíče.
- Zvýšení výkonu popsané možnosti zásad se tak mohli ověřovat pomocí dotazů na byly Hotovo.
- V Číně datovém centru, adresu [URL doručení klíč](http://msdn.microsoft.com/library/azure/ef4dfeeb-48ae-4596-ab28-44d6b36d8769#get_delivery_service_url) odteď umožňuje zadávání jednotlivé zákazníky (stejně jako v jiných datacentrech).
- Přidané HLS automatického cílové doby trvání. Při živou streamování HLS je vždy sbalí dynamicky. Ve výchozím nastavení Media Services automaticky vypočítá HLS segmentu balení poměr (FragmentsPerSegment) na základě klíčový intervalu (KeyFrameInterval) bývá označovaná taky jako seskupení z obrázků – GOP, získaná z živého encoder. Další informace najdete v tématu [práce s Azure Media Services Live datových proudů].
 
###<a name="media-services-net-sdk-updates"></a>Mediální služby .NET SDK aktualizace

- [Azure Media Services.NET SDK](http://www.nuget.org/packages/windowsazure.mediaservices/) je teď verze 3.1.0.0.
- Závislost typu .net SDK upgradovat na .NET 4.5 Framework.
- Přidání nového rozhraní API, který umožňuje aktualizovat kódování rezervovaná jednotky. Další informace najdete v tématu [aktualizace rezervovaná typ jednotky a zvětšení kódování RUs pomocí .NET](http://msdn.microsoft.com/library/azure/jj129582.aspx).
- Přidané JWT (JSON Web tokenu) podporu tokenu ověřování. Další informace najdete v tématu [JWT tokenu ověřování v Azure Media Services a dynamické šifrování](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).
- Přidané relativní odsazení BeginDate a Datumvypršení platnosti v šabloně PlayReady licenci.


##<a id="november_changes_14"></a>Uvolnění listopadu 2014

- Media Services umožňuje jedí živý hladký přenos (FMP4) obsah přes připojení SSL. Chcete-li jedí přes připojení SSL, dbejte aktualizovat adresu URL ingest na HTTPS.  Další informace o live streamování najdete v tématu [práce s Azure Media Services Live datových proudů].
- Všimněte si, že v současnosti je nelze jedí živou toku RTMP přes připojení SSL.
- Vysílat datovými proudy obsah přes připojení SSL. K tomuto účelu zkontrolujte, že streamování adresy URL začínat HTTPS.
- Všimněte si, že můžete jenom přenášet přes připojení SSL Pokud streamování koncový bod, ze které doručení obsahu byl vytvořen za 10 září 2014. Pokud streamování adresy URL založené na datových proudů koncové body vytvořené po září 10, obsahuje adresa URL "streaming.mediaservices.windows.net" (nový formát). Streamování adresy URL, které obsahují "origin.mediaservices.windows.net" (starý formát) nepodporují SSL. Pokud je vaše adresa URL v původním formátu a chcete mít možnost streamování SSL, [vytvořte nový streamování koncový bod](media-services-portal-manage-streaming-endpoints.md). Použití adresy URL založeno na nový streamování koncový bod ke streamování obsahu SSL.

##<a id="october_changes_14"></a>Uvolnění říjen 2014

### <a id="new_encoder_release"></a>Mediální služby Encoder vydání

Oznámení o nové verzi médií služby Azure Media Encoder. S nejnovějšími Azure Media Encoder pouze vám bude účtovaná za výstup GB, ale jinak nové encoder je funkce kompatibilní s předchozí encoder. Další informace [Media Services ceny podrobnosti]).

### <a id="oct_sdk"></a>Mediální služby .NET SDK 

Media Services SDK pro .NET rozšíření je teď verze 2.0.0.3.

Media Services SDK pro .NET je teď verze 3.0.0.8.

Byly provedeny následující změny:

- Refaktoring předmětů zásad opakovat.
- Přidání identifikační řetězec do záhlaví žádost http.
- Přidání kroku sestavení nuget obnovit.
- Řešení scénář testů používat x509 certifikátu z úložiště.
- Ověření nastavení při aktualizaci kanálu a přenos ukončit.
 

### <a name="new-github-repository-to-host-media-services-samples"></a>Nové GitHub úložiště ukázek Media Services host

Ukázky nacházejí v [úložišti GitHub vzorky Azure Media Services](https://github.com/Azure/Azure-Media-Services-Samples).


##<a id="september_changes_14"></a>Uvolnění září 2014

Media Services ZBÝVAJÍCÍ metadat je teď verze 2.7. Další informace o nejnovějších aktualizacích pro ZBÝVAJÍCÍ najdete v článku Principy [Azure Media Services ZBÝVAJÍCÍ rozhraní API].

Media Services SDK pro .NET je teď verze 3.0.0.7
 
### <a id="sept_14_breaking_changes"></a>Zrušení změn

* **Origin** přejmenovaná [StreamingEndpoint].
* Změna výchozího chování při použití **Klasické portál Azure** kódování a pak publikovat MP4 souborů.

Dříve, když na portálu Azure klasické publikovat je tvořená jedním souborem MP4 videa majetek přidružení zabezpečení adresa URL bude vytvořen (adresy URL přidružení zabezpečení umožňují stáhnout video z úložiště objektů blob). V současné době při použití portálu klasické Azure kódování a pak publikovat tvořená jedním souborem videa materiálů MP4 vygenerovaných adresa URL odkazuje na Azure Media Services streamování koncového bodu.  Tato změna nemá vliv na MP4 videí, která jsou přímo nahráli Media Services a publikovány bez právě zakódovaný službou Azure Media Services.

Nyní máte následujících dvou možností problém vyřešit.

* Povolit streamování jednotky a pomocí dynamického balení stáhnete materiálů MP4 jako prezentaci vyhlazenými streamování.

* Vytvoření přidružení zabezpečení url pro stažení (nebo postupně přehrát) .mp4. Další informace o tom, jak vytvořit locator přidružení zabezpečení najdete v článku [Doručování obsahu].


### <a id="sept_14_GA_changes"></a>Nové funkce/scénáře, které jsou součástí GA vydání

* **Procesor pro platformu indexování média**. Další informace najdete v článku [Indexování mediální soubory s Azure Media indexování].

* Entita [StreamingEndpoint] umožňuje přidat vlastní doménu (hostitel) názvy.

    Vlastní název domény má být použit jako název koncového bodu streamování Media Services potřebujete přidat vlastní hostitele streamování koncový bod. Pomocí médií služby REST API nebo .NET SDK můžete přidat vlastní hostitele.
    
    Platí následující omezení:
    
    * Vlastnictví názvu vlastní domény, musíte mít.
    
    * Azure Media Services musí ověřit vlastnictví názvu domény. Ověřte doménu, vytvořit záznam CName, který mapy <MediaServicesAccountId>.<parent domain> k verifydns < mediaservices zóny dns >. 
    
    * Je třeba vytvořit další CName, který odpovídá názvu hostitele vaše služby StreamingEndpont médií (například amstest.streaming.mediaservices.windows.net) název vlastní hostitele (například sports.contoso.com).


    Další informace najdete v tématu vlastnost **CustomHostNames** v tématu [StreamingEndpoint] .

### <a id="sept_14_preview_changes"></a>Nové funkce/scénáře, které jsou součástí verzi veřejných preview

* Dynamický náhled streamování. Další informace najdete v tématu [práce s Azure Media Services Live datových proudů].

* Služba pro doručování klíče. Další informace najdete v tématu [Použití dynamické šifrování AES 128 a služba pro doručování klíč].

* Dynamické šifrování AES. Další informace najdete v tématu [Použití dynamické šifrování AES 128 a služba pro doručování klíč].

* Služba pro doručování PlayReady licenci. Další informace najdete v tématu [Použití dynamické šifrování PlayReady a služba pro doručování licence].

* Dynamické PlayReady šifrování. Další informace najdete v tématu [Použití dynamické šifrování PlayReady a služba pro doručování licence].

* Mediální služby PlayReady licenci šablony. Další informace najdete v tématu [Přehled šablony médií služby PlayReady licence].

* Přenos úložiště zašifrovaných prostředky. Další informace najdete v tématu [Obsahu zašifrovaných datových proudů úložiště].

##<a id="august_changes_14"></a>Uvolnění srpen 2014

Při kódování aktivum aktivum výstup vyrobeno po skončení kódování projektu. Do této verzi Azure Media Services Encoder vyrobeno metadata výstup prostředky. Spuštění s touto verzí programu encoder vytváří také metadata o zadávání prostředky. Další informace najdete v tématech [Metadat vstupní] a [Výstupní Metadata] .


##<a id="july_changes_14"></a>Uvolnění červenec 2014

Tyto opravy chyb provedené Balíčkovač Azure Media Services a Encryptor:

* Jenom zvuk se přehraje zpět po transmuxing materiálů živou archiv na HTTP Live streamování – to má byla pevné a teď přehrávání zvuku a videa.

* Po balení aktivum a Live streamování HTTP AES 128bitové šifrování obálky, sbalené proudů není přehrát na zařízeních s Androidem – tato chyba má pevné a sbalené proudu přehrávání na zařízeních s Androidem, které podporují Live streamování HTTP.

##<a id="may_changes_14"></a>Vydání květen 2014

### <a id="may_14_changes"></a>Obecné mediální služby aktualizací

Teď můžete [Dynamické balení] v3 HTTP Live datových proudů (HLS) proudu. Můžete vysílat datovými proudy HLS v3, přidejte v tomto formátu na cestu URL origin: * .ism/manifest(format=m3u8-aapl-v3). Další informace najdete v tématu [Blog Drouin Přezdívka].

Dynamické balení nyní také podle hladký přenos staticky zašifrovaných PlayReady podporuje předvádění HLS (v3 a v4) zašifrovaných PlayReady. Informace o tom, jak šifrování hladký přenos s PlayReady najdete v tématu [Ochrana vyhlazenými toku s PlayReady].

### <a name="may_14_donnet_changes"></a>Mediální služby .NET SDK aktualizace

Tato vylepšení jsou zahrnuty ve verzi Media Services .NET SDK 3.0.0.5:

* Lepší rychlosti a odolnost pro nahrávání nebo stahování multimediálních materiálů.

* Vylepšení při opakování použití logických operátorů a přechodové výjimek: 

    * Přechodné chybě zjišťování a opakování použití logických operátorů byly lepší výjimek, které jsou příčinou dotazování, uložení změn, nahrávání nebo stahování souborů. 
    
    * Při výskytu Web výjimky (například při ACS tokenu žádost o konverzaci), zjistíte, že kritické chyby selhává rychleji nyní.

Další informace najdete v tématu [Opakovat logiky Media Services SDK pro .NET].

##<a id="april_changes_14"></a>Uvolnění Encoder dubna 2014

### <a name="april_14_enocer_changes"></a>Mediální služby Encoder aktualizace

* Přidaná podpora ingesting AVI soubory vytvořené v editoru tráva sedla EDIUS nelineárních problémů, kde je mírným komprimovány pomocí tráva sedla Centrála/HQX kodeku. Další informace najdete v tématu [Tráva sedla oznamuje EDIUS 7 datových proudů prostřednictvím cloudu].

* Určení konvence pro soubory vytvořené pomocí Media Encoder přidaná podpora. Další informace najdete v tématu [Řízení médií služby Encoder výstup názvy souborů].

* Přidaná podpora videa nebo zvuku překrytí. Další informace najdete v tématu [Vytvoření překrytí].

* Přidaná podpora ve hřbetu společně více videa segmentů. Další informace najdete v tématu [Ve hřbetu segmentů Video].

* Pevná chyb souvisejících s převod kódování MP4s místo, kam má byla kódování zvuku s MPEG 1 Audio layer 3 (označovaná taky jako MP3).


##<a id="jan_feb_changes_14"></a>Leden/únor 2014 verzí

### <a name="jan_fab_14_donnet_changes"></a>Mediální služby Azure .NET SDK 3.0.0.1, 3.0.0.2 a 3.0.0.3

Změny v 3.0.0.1 a 3.0.0.2 patří:

* Pevné otázky týkající se používání dotazů LINQ s příkazy řadit podle.

* Rozdělení test řešení v [GitHub] do jednotkové testů a scénářů testů.

Další podrobnosti o změnách najdete v tématu: [Azure Media Services .NET SDK 3.0.0.1 a 3.0.0.2 vydání].

Následující změny provedené v 3.0.0.3:

* Upgradované Azure úložiště závislosti používat verzi 3.0.3.0. 

* Podařilo odstranit problém s zpětná kompatibilita nástroje pro 3.0. *.* uvolnění. 


##<a id="december_changes_13"></a>Uvolnění prosinec 2013

### <a name="dec_13_donnet_changes"></a>Mediální služby Azure .NET SDK 3.0.0.0

>[AZURE.NOTE] uvolnění 3.0.x.x nejsou zpětně kompatibilní s 2.4.x.x verzemi.

Nejnovější verzi Media Services SDK je teď 3.0.0.0. Můžete stáhnout nejnovější balíček z Nuget nebo získejte bitů ze [GitHub].

Začnete s Media Services SDK verze 3.0.0.0, můžete znovu použít tokeny [Azure Active Directory Access ovládací prvek služby ACS ()] . Další informace naleznete v části "Opakované použití přístupových ovládací prvek Služba tokenů" v tématu [připojení k Media Services s Media Services SDK pro .NET] .

### <a name="dec_13_donnet_ext_changes"></a>Mediální služby Azure .NET SDK rozšíření 2.0.0.0

Rozšíření Azure Media Services .NET SDK je sada rozšíření metod a pomocná funkce, které zjednoduší kódu a usnadňují se dají s Azure Media Services. Nejnovější bitů můžete získat z [Azure Media Services .NET SDK rozšíření].

##<a id="november_changes_13"></a>Uvolnění listopadu 2013

### <a name="nov_13_donnet_changes"></a>Mediální služby Azure .NET SDK změny

Začnete s touto verzí, Media Services SDK pro .NET zpracovává přechodná poruch chyby, ke kterým může dojít při volání vrstvy Media Services REST API.

##<a id="august_changes_13"></a>Uvolnění srpen 2013

### <a name="aug_13_powershell_changes"></a>Rutiny prostředí PowerShell médií služby součástí nástrojů Sdk Azure

Tyto rutiny prostředí PowerShell Media Services jsou teď součástí [nástrojů sdk azure].

* Get-AzureMediaServices 

    Například `Get-AzureMediaServicesAccount`.

* Nové AzureMediaServicesAccount 

    Například `New-AzureMediaServicesAccount -Name “MediaAccountName” -Location “Region” -StorageAccountName “StorageAccountName”`.

* Nové AzureMediaServicesKey 

    Například `New-AzureMediaServicesKey -Name “MediaAccountName” -KeyType Secondary -Force`.

* Odebrat AzureMediaServicesAccount 

    Například `Remove-AzureMediaServicesAccount -Name “MediaAccountName” -Force`.

##<a id="june_changes_13"></a>Uvolnění června 2013

### <a name="june_13_general_changes"></a>Azure Media Services změny

Změny uvedené v této části jsou aktualizace součástí vydání června 2013 Media Services.

* Umožňuje propojit více účtů úložiště k účtu služby média. 

    StorageAccount
    
    Asset.StorageAccountName a Asset.StorageAccount

* Možnost aktualizace Job.Priority. 

* Oznámení týkající se entity a vlastnosti: 

    JobNotificationSubscription
    
    NotificationEndPoint
    
    Úlohy

* Asset.Uri 

* Locator.Name 

### <a name="june_13_dotnet_changes"></a>Změní Azure Media Services .NET SDK

Jsou zahrnuty následující změny v června 2013 Media Services SDK vydání. Nejnovější Media Services SDK je dostupná na GitHub.

* Od verze 2.3.0.0 Media Services SDK podporuje propojení více úložiště účty k účtu Media Services. Tuto funkci podporují následující rozhraní API:
    
    Typ IStorageAccount.
    
    Vlastnost Microsoft.WindowsAzure.MediaServices.Client.CloudMediaContext.StorageAccounts.
    
    Vlastnost StorageAccount.
    
    Vlastnost StorageAccountName.
    
    Další informace najdete v tématu [Správa služby datových zdrojů médií u více účtů úložiště].

* Oznámení týkající se rozhraní API. Od verze 2.2.0.0, jestli že máte možnost poslouchat Azure fronty úložiště oznámení. Další informace najdete [Zpracování Media Services úlohy oznámení].
    
    Vlastnost Microsoft.WindowsAzure.MediaServices.Client.IJob.JobNotificationSubscriptions.
    
    Typ Microsoft.WindowsAzure.MediaServices.Client.INotificationEndPoint.
    
    Typ Microsoft.WindowsAzure.MediaServices.Client.IJobNotificationSubscription.
    
    Typ Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointCollection.
    
    Typ Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointType.
    
    Typ Microsoft.WindowsAzure.MediaServices.Client.NotificationJobState.

* Počet závislostí na Azure úložiště klienta SDK 2.0 (Microsoft.WindowsAzure.StorageClient.dll).

* Počet závislostí na OData 5.5 (Microsoft.Data.OData.dll).


##<a id="december_changes_12"></a>Uvolnění prosinec 2012

### <a name="dec_12_dotnet_changes"></a>Změní Azure Media Services .NET SDK

* Technologie IntelliSense: Přidat chybějící technologie Intellisense si přečtěte následující dokumentaci pro mnoho typů.

* Microsoft.Practices.TransientFaultHandling.Core: Podařilo odstranit problém místo, kam SDK pořád vám to závislost na některé ze starších verzí tohoto sestavení. V SDK teď odkazuje 5.1.1209.1 verzi tohoto sestavení.

Opravy problémů s součástí listopad 2012 SDK:

* IAsset.Locators.Count: Tento počet je teď správně vykázanému nová rozhraní IAsset po odstranění všech Locator.

* IAssetFile.ContentFileSize: Tuto hodnotu je teď správně nastavil po nahrávání IAssetFile.Upload(filepath).

* IAssetFile.ContentFileSize: Tato vlastnost lze nyní nastavit při vytváření souboru materiálů. Byla dříve určené jen pro čtení.

* IAssetFile.Upload(filepath): Podařilo odstranit problém místo, kam byla tato metoda synchronní nahrát vyvolání tato chyba při nahrávání více souborů do majetku. Došlo k chybě "Server se nepodařilo ověřit žádost. Ujistěte se, že hodnota se tak mohli ověřovat záhlaví je vytvořen správně včetně podpis. "

* IAssetFile.UploadAsync: Oprava problému, kde může být víc než 5 soubory nahrané současně.

* IAssetFile.UploadProgressChanged: Tato událost je teď poskytovanou SDK.

* IAssetFile.DownloadAsync (řetězec, BlobTransferClient, ILocator, CancellationToken): Tato metoda přetížení je teď k dispozici.

* IAssetFile.DownloadAsync: Oprava problému, kde může současně stáhnout víc než 5 soubory.

* IAssetFile.Delete(): Podařilo odstranit problém kde volající odstranit může výjimku pokud žádný soubor byl odeslán pro IAssetFile.

* Úlohy: Podařilo odstranit problém kde by vytvořit zřetězení "MP4 hladkým toků úkolu" s "PlayReady ochranu úkolu" pomocí šablony úlohy všechny úkoly vůbec.

* EncryptionUtils.GetCertificateFromStore(): Tato metoda už výjimku null odkaz kvůli selhání najít certifikát, který podle problémů s konfigurací certifikát.

##<a id="november_changes_12"></a>Uvolnění listopadu 2012

Změny uvedené v této části byly aktualizace součástí listopad 2012 (verze 2.0.0.0) SDK. Tyto změny může vyžadovat jakýkoli kód napsané pro náhled června 2012 SDK uvolněte změnili nebo přepsat.

* Prostředky
    
    IAsset.Create(assetName) je funkce vytváření jen materiálů. IAsset.Create už odesílání souborů v rámci volání metody. Použití IAssetFile pro odeslání.
    
    Metoda IAsset.Publish a hodnota výčtu AssetState.Publish mohly být odebrány z SDK služby. Jakýkoli kód, který závisí na tato hodnota musí být znovu ručně psaných.

* FileInfo

    Tato třída byly odebrány a nahrazuje IAssetFile.

    IAssetFiles

    IAssetFile nahradí FileInfo a má odlišné chování. Můžete ho instanci IAssetFiles objektu a za ním uveďte nahrání souboru buď pomocí Media Services SDK nebo SDK úložiště Azure. Následující přetížení IAssetFile.Upload lze použít:

    * IAssetFile.Upload(filePath): Synchronní metodu, která blokuje vlákna a doporučuje se pouze v případě nahrávání tvořená jedním souborem.
    
    * IAssetFile.UploadAsync (cesta k souboru blobTransferClient, locator, cancellationToken): asynchronní metody. Toto je způsob upřednostňované odeslání. 

    Známý problém: pomocí cancellationToken skutečně ukončíte nahrávání; zrušení stav úkolů však může být jednotlivých stavů. Musíte správně zachycení a obsloužení výjimky.

* Locator
    
    Mohly být odebrány verze specifické Origin. Přidružení zabezpečení specifické kontext. Locators.CreateSasLocator (materiálů, accessPolicy) budou označeny zastaralý nebo odstraněna JM. Naleznete v části Locator ve skupinovém rámečku nové funkce pro aktualizované chování.


##<a id="june_changes_12"></a>Verze Preview. června 2012

Následující funkce byly nové verze dne v SDK.

* Odstranění entity

    IAsset IAssetFile, ILocator, IAccessPolicy, objekty IContentKey odstraňují teď na úrovni objektů, tedy IObject.Delete(), místo vyžadující odstranit v kolekci, která je cloudMediaContext.ObjCollection.Delete(objInstance).

* Locator

    Locator musí být vytvořena metodou CreateLocator a použijte výčet hodnoty LocatorType.SAS nebo LocatorType.OnDemandOrigin jako argument pro určitý typ URL, že kterou chcete vytvořit.

    Byly přidány nové vlastnosti Locator usnadňuje získat použitelné URI pro obsah. Tato změna návrhu Locator má flexibilnější budoucí rozšiřitelnost třetích stran a zvětšení snadné použití pro médií klientské aplikace.

* Podpora asynchronní metody

    Asynchronní podporu přidala všechny metody.


##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


<!-- Anchors. -->

<!-- Images. -->

<!--- URLs. --->
[Mediální služby Azure fórum MSDN]: http://social.msdn.microsoft.com/forums/azure/home?forum=MediaServices
[Mediální služby Azure REST API Reference]: http://msdn.microsoft.com/library/azure/hh973617.aspx 
[Mediální služby podrobnosti o cenách]: http://azure.microsoft.com/pricing/details/media-services/
[Zadávání metadat]: http://msdn.microsoft.com/library/azure/dn783120.aspx
[Výstup metadat]: http://msdn.microsoft.com/library/azure/dn783217.aspx
[Doručení obsahu]: http://msdn.microsoft.com/library/azure/hh973618.aspx
[Indexování mediální soubory s Azure Media indexování]: http://msdn.microsoft.com/library/azure/dn783455.aspx
[StreamingEndpoint]: http://msdn.microsoft.com/library/azure/dn783468.aspx
[Práce s Azure Media Services Live streamování]: http://msdn.microsoft.com/library/azure/dn783466.aspx
[Použití dynamického šifrování AES 128 a přihlašovacích údajů klíčové doručení]: http://msdn.microsoft.com/library/azure/dn783457.aspx
[Pomocí dynamického PlayReady šifrování a služba pro doručování licence]: http://msdn.microsoft.com/library/azure/dn783467.aspx
[Preview features]: http://azure.microsoft.com/services/preview/
[Mediální služby přehled šablon PlayReady licence]: http://msdn.microsoft.com/library/azure/dn783459.aspx
[Přenos úložiště zašifrovaných obsahu]: http://msdn.microsoft.com/library/azure/dn783451.aspx
[Azure Classic Portal]: https://manage.windowsazure.com
[Dynamické balení]: http://msdn.microsoft.com/library/azure/jj889436.aspx
[Přezdívka Drouin blogu]: http://blog-ndrouin.azurewebsites.net/hls-v3-new-old-thing/
[Ochrana vyhlazenými toku s PlayReady]: http://msdn.microsoft.com/library/azure/dn189154.aspx
[Opakovat logiky Media Services SDK pro .NET]: http://msdn.microsoft.com/library/azure/dn745650.aspx
[Tráva sedla oznamuje EDIUS 7 datových proudů prostřednictvím cloudu]: http://www.streamingmedia.com/Producer/Articles/ReadArticle.aspx?ArticleID=96351&utm_source=dlvr.it&utm_medium=twitter
[Řízení médií služby Encoder výstupních názvů souborů]: http://msdn.microsoft.com/library/azure/dn303341.aspx
[Vytvoření překrytí]: http://msdn.microsoft.com/library/azure/dn640496.aspx
[Více segmentů videa]: http://msdn.microsoft.com/library/azure/dn640504.aspx
[Azure Media Services .NET SDK 3.0.0.1 a 3.0.0.2 vydání]: http://www.gtrifonov.com/2014/02/07/windows-azure-media-services-.net-sdk-3.0.0.2-release/
[Služba řízení přístupu Azure Active Directory (ACS)]: http://msdn.microsoft.com/library/hh147631.aspx
[Připojit se ke službám médium s Media Services SDK pro .NET]: http://msdn.microsoft.com/library/azure/jj129571.aspx
[Mediální služby Azure .NET SDK rozšíření]: https://github.com/Azure/azure-sdk-for-media-services-extensions/tree/dev
[Azure nástrojů sdk]: https://github.com/Azure/azure-sdk-tools
[GitHub]: https://github.com/Azure/azure-sdk-for-media-services
[Správa médií služby prostředky u více účtů úložiště]: http://msdn.microsoft.com/library/azure/dn271889.aspx
[Zpracování Media Services úlohy oznámení]: http://msdn.microsoft.com/library/azure/dn261241.aspx
 
