<properties 
    pageTitle="Azure Media Services koncepty | Microsoft Azure" 
    description="Toto téma obsahuje přehled Azure Media Services konceptů" 
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

#<a name="azure-media-services-concepts"></a>Azure Media Services koncepty 

Toto téma obsahuje přehled nejdůležitějších konceptů Media Services.

##<a id="assets"></a>Majetek i úložiště

###<a name="assets"></a>Prostředky

[Materiály](https://msdn.microsoft.com/library/azure/hh974277.aspx) obsahuje digitální soubory (včetně video, zvuk, obrázky, miniatur kolekce, skladeb textu skryté titulky souborů) a metadata o těchto souborech. Po digitálním soubory jsou odeslány do aktivum, může použít v Media Services kódování a datových proudů pracovní postupy.

Aktivum je namapovaných kontejneru objektů blob Azure úložiště účtu a soubory v majetek jsou uloženy jako objekty BLOB v tomto kontejneru.

Při rozhodování, jaký obsah médií uložit a uložte do aktivum, platí následující omezení:

- Aktivum smí obsahovat pouze jeden, jedinečný instanci médií obsahu. Například jednoho upravit televizního dílu, video nebo reklamní.
- Aktivum nesmí obsahovat více verzí nebo úpravy souboru audiovizuální. Příklad nesprávné použití aktiva by se pokoušíte obsahují více než jeden televizního dílu, reklamní nebo víc kamer z jedné výroby uvnitř aktivum. Uložení více verzí nebo úpravy souboru audiovizuální v aktivum může způsobit potíže odeslání kódování úlohy, datových proudů a zabezpečení doručení materiálů později v pracovním postupu.  

###<a name="asset-file"></a>Soubor materiálů 
[AssetFile](https://msdn.microsoft.com/library/azure/hh974275.aspx) představuje skutečný videoklipu nebo zvukového souboru, který je uložený v kontejneru objektů blob. Soubor materiálů je vždy přidružené k aktivum a aktivum může obsahovat jedno nebo více souborů. Úkol Media Encoder služby nezdaří, pokud soubor objektu materiálů je přidružená k souboru digitální v kontejneru objektů blob.

**AssetFile** instance a skutečné multimediálního souboru jsou dvě samostatné objekty. AssetFile instance obsahuje metadata multimediálního souboru, když mediální soubor s obsahem skutečné mediální.

Neměli byste změnit obsah kontejnery objektů blob generované Media Services bez použití rozhraní API služeb média.

###<a name="asset-encryption-options"></a>Možnosti šifrování materiálů

V závislosti na typu obsahu, který chcete nahrát, ukládat a předvádění obsahuje Media Services můžete vybírat různé možnosti šifrování.

**Žádná** Bez šifrování se používá. Toto je výchozí hodnota. Všimněte si, že použijete tuto možnost obsahu není zamčený při přenosu šifrovaná nebo u ostatních v úložišti.

Pokud budete chtít předvádění MP4 pomocí postupného stahování použijte tuto možnost nahrát obsah.

**StorageEncrypted** – tato možnost slouží k šifrování vymazat obsah místně pomocí šifrování AES 256 a pak ho nahrajte do úložiště Azure kde je uložena zašifrovat zbývající. Prostředky chráněné pomocí šifrování úložiště jsou automaticky bez šifrování umístí do systém zašifrovaných souborů před kódování a volitelně znovu zašifrovaných před odesláním zpět jako nový majetek výstupu. Případ použití primární šifrování úložiště je, když chcete zabezpečit vstupní mediální soubory kvalitní silné šifrování u ostatních na disku. 

K provedení úložiště šifrované materiálů, musíte nakonfigurovat majetku doručení zásad, aby Media Services věděli, jak budete chtít doručení obsahu. Než vaše materiálů můžete streamují, serveru datových proudů odebere šifrování úložiště a přenáší datové proudy obsahu pomocí zásad dodací (například AES PlayReady a bez šifrování). 

**CommonEncryptionProtected** - použít tuto možnost, pokud chcete šifrovat (nebo odešlete už zašifrovaných) obsahu s běžné šifrování nebo PlayReady DRM (například hladký přenos chráněné pomocí PlayReady DRM).

**EnvelopeEncryptionProtected** – použití tuto možnost, pokud chcete zamknout (nebo odešlete už chráněné) HTTP Live datových proudů (HLS) šifrovaná pomocí šifrování AES (Advanced Standard). Všimněte si, že pokud ukládáte HLS již zašifrovaných AES, je třeba zašifrovaný správcem transformace.

###<a name="access-policy"></a>Zásady přístupu 

[AccessPolicy](https://msdn.microsoft.com/library/azure/hh974297.aspx) definuje oprávnění (například čtení zapsat a seznam) a doby trvání přístup k aktivum. Obvykle byste měli objektu AccessPolicy předat URL, která se má použít pro přístup k souborům obsažené v aktivum.


###<a name="blob-container"></a>Kontejner objektů BLOB

Kontejner objektů blob poskytuje seskupení sadu objektů BLOB. Kontejnery objektů blob se používají v Media Services jako hranice bod pro řízení přístupu a Locator přidružení sdílené podpis aplikace Access (zabezpečení) pro uživatele. Účet Azure úložiště může obsahovat neomezený počet objektů blob kontejnery. Kontejner mohou být uloženy neomezený počet objektů BLOB.

>[AZURE.NOTE]Neměli byste změnit obsah kontejnery objektů blob generované Media Services bez použití rozhraní API služeb média.

###<a id="locators"></a>Locator

[Locator](https://msdn.microsoft.com/library/azure/hh974308.aspx)s vytvořit vstupní bod pro přístup k souborům obsažené v aktivum. Zásady přístupu slouží k určení oprávnění a doba trvání klienta má přístup k dané materiálů. Locator nemůžou mít typu n: n s zásad přístup tak, aby se různých Locator můžete poskytnout jiné počáteční čas a typy připojení různých klientů při všechny používání stejná oprávnění a nastavení doby trvání. z důvodu omezení zásad sdílený přístup nastavil služby Azure úložiště, však nesmí obsahovat více než pět jedinečné Locator přidružený k dané materiálů najednou. 

Media Services podporuje dva typy Locator: OnDemandOrigin Locator slouží k proudy médií (například MPEG POMLČKU HLS a přitom zajistit hladký přenos) nebo postupné stahování média a adresu URL přidružení zabezpečení Locator slouží k nahrávání nebo stahování multimediální soubory to\from Azure úložiště. 

Všimněte si, že seznam oprávnění (AccessPermissions.List) by neměly být použita při vytváření OrDemandOrigin locator. 

###<a name="storage-account"></a>Úložiště účtu

Všechny přístup k základnímu úložišti Azure probíhá prostřednictvím účtu úložiště. Účet služby média můžete přidružit jeden nebo více účtů úložiště. Účet může obsahovat neomezený počet kontejnerů, dokud jejich celková velikost v části 500TB na účtu úložiště.  Služba Media poskytuje SDK úrovně nástrojů umožňuje spravovat více účtů úložiště a rozdělení svém majetku během nahrát na tyto účty založené na metriky nebo náhodné distribuční Vyrovnávání zatížení. Další informace najdete v tématu Práce s [Úložištěm Azure](https://msdn.microsoft.com/library/azure/dn767951.aspx). 

##<a name="jobs-and-tasks"></a>Projektů a úkolů

[Práce](https://msdn.microsoft.com/library/azure/hh974289.aspx) se obvykle používá k obrázku (například index nebo kódovat) jedné prezentaci zvuk a video. Pokud zpracováváte víc videí, vytvořte úlohu pro každé video zakódovat.

Úlohy obsahuje metadata týkající se zpracování provádět. Jednotlivé úlohy obsahuje jeden nebo více [úkolů](https://msdn.microsoft.com/library/azure/hh974286.aspx)s, zadáte výstupu atomová zpracování úkolů, zadávání prostředky, prostředky, médií procesoru a jeho přidružený nastavení. Úkoly v projektu můžete zřetězený dohromady, kde výstup materiálů jeden úkol je předané jako vstupní materiálů dalšího úkolu. Tímto způsobem může obsahovat jednu úlohu všechny potřebné pro prezentaci médií zpracování.

##<a id="encoding"></a>Kódování

Azure Media Services obsahuje více možností pro šifrování médií v cloudu.

Když začnete s Media Services, je důležité pochopit rozdíl mezi formáty kodecích a soubor.
Kodeky jsou software implementující algoritmů kompresi/dekompresi vzhledem k tomu formátech souborů jsou kontejnery, které obsahují komprimované video.

Media Services poskytuje dynamické balení, který umožňuje poskytovat adaptivní bitrate MP4 a přitom zajistit hladký přenos zakódovaný úprav textu v datových proudů formáty podporované kodérem Media Services (MPEG POMLČKU HLS, hladký přenos, konfigurace) aniž by bylo nutné znovu balíček do těchto streamování formáty.

Umožní využít výhod [dynamické balení](media-services-dynamic-packaging-overview.md)potřebujete postupujte takto:

- Kódování mezzanine (zdrojový) souboru do sady adaptivní bitrate MP4 soubory nebo Adaptivní bitrate hladký přenos (kódování kroky jsou prokázáno dál v tomto kurzu).
- Potřebujete alespoň jednu jednotku streamování na vyžádání pro streamování koncový bod, ze kterého chcete doručení obsahu. Další informace najdete v tématu [jak okamžitou streamování rezervovaná jednotkách](media-services-portal-manage-streaming-endpoints.md).

Media Services podporuje následující na službu kodéry, které jsou popsané v tomto článku:

- [Standardní Media Encoder](media-services-encode-asset.md#media-encoder-standard)
- [Pracovní postup Premium Media Encoder](media-services-encode-asset.md#media-encoder-premium-workflow)

Informace o podporovaných kodéry najdete v tématu [kodéry](media-services-encode-asset.md).


##<a name="live-streaming"></a>Live streamování

V Azure Media Services představuje kanálu kanálem k odesílání zpráv pro zpracování živý streamování obsah. Kanálu obdrží živou vstupní datových proudů jedním ze dvou způsobů:

- Živou encoder místní odešle více bitrate RTMP nebo hladký přenos (fragmentován MP4) do kanálu. Můžete použít následující živou kodéry, které výstup více bitrate hladký přenos: elementární Envivio, Cisco. Následující živou kodéry výstup RTMP: Adobe Flash Live Telestream Wirecast a Tricaster transcoders. Požití datových proudů projít kanály bez dalšího zpracování. Při požadavku, nabízí Media Services proudu zákazníkům.

- Jeden bitrate toku (v některém z následujících formátů: RTP (MPEG-TS)), RTMP a přitom zajistit hladký přenos (fragmentován MP4)) se neodesílají kanál, který je povoleno provádět živou kódování s Media Services. Kanál pak provede živou kódování příchozí jednoho bitrate toku proudu videa s víc bitrate (adaptivní). Při požadavku, nabízí Media Services proudu zákazníkům.

###<a name="channel"></a>Kanálu

V Media Services jsou zodpovědní za zpracování živou streamování obsahu s [kanálu](https://msdn.microsoft.com/library/azure/dn783458.aspx). Kanálu poskytuje vstupní koncového bodu (jedí adresy URL), pak zadáte k živé transcoder. Kanál dostává živou vstupní datových proudů z živého transcoder a díky kterému je dostupný pro datových proudů prostřednictvím jeden nebo více StreamingEndpoints. Kanály taky poskytují koncový bod preview (Náhled URL), které používáte pro zobrazení náhledu a ověřit vašeho proudu před další zpracování a doručení.

Při vytváření kanálu můžete získat adresu URL ingest a adresu URL náhled. Chcete-li získat tyto adresy URL, kanál nemusí být spuštěného stavu. Až budete připraveni začít předání dat z živého transcoder do kanálu, musíte začít kanál. Jakmile živou transcoder začne ingesting dat, můžete zobrazit náhled vašeho proudu.

Každý účet Media Services může obsahovat více kanály, více programů a více StreamingEndpoints. Podle potřeby šířka pásma a zabezpečení může být StreamingEndpoint služby snaží jeden nebo více kanálů. Všechny StreamingEndpoint můžete si ho naimportovat z libovolné kanálu.


###<a name="program"></a>Program

[Program](https://msdn.microsoft.com/library/azure/dn783463.aspx) umožňuje určit, publikování a úložiště segmentů v živé proudu. Kanály spravovat aplikace. Vztah kanálu a Program je velmi podobné tradiční médií Pokud kanálu konstantní toku obsahu a program má obor vymezený na některé časových událost na tento kanál.
Můžete zadat počet hodin, chcete-li zachovat zaznamenané obsah do programu nastavením vlastnosti **ArchiveWindowLength** . Tato hodnota můžete nastavit z minimální 5 minut maximálně 25 hodin.

ArchiveWindowLength také určuje, že maximální množství času klientů můžete požádat o zpět v čase od aktuální pozice live. Programy mohlo by umožnit spuštění přes zadaný časový úsek, ale obsah, který zpozdit délka okno nepřetržitě ignorován. Tato hodnota tato vlastnost také určuje, jak dlouho rozrůstat manifesty klienta.

Jednotlivé aplikace je přidružený aktivum. URL pro přidružené materiálů musí vytvořit pro publikování program. Máte tento locator vám umožní vytváření datových proudů adresy URL, která můžete poskytnout klienty.

Kanálu, který podporuje až tři souběžně spuštěné programy můžete vytvořit více archivů stejné příchozí proudu. Můžete publikovat a archivace různé části události podle potřeby. Požadovanou firmy je například archivace 6 hodin program, ale vysílání pouze posledních 10 minut. K tomu potřebujete k vytvoření dvou souběžně spuštěné programy. Jeden program je nastavena na archivovat 6 hodin události, ale není publikován program. Jiném programu je nastavena pro archivaci 10 minut a tento program je publikován.


Další informace najdete v tématu:

- [Práce s kanály, které jsou povoleny k provedení Live kódování s Azure Media Services](media-services-manage-live-encoder-enabled-channels.md)
- [Práce s kanály, které dostanete více bitrate živou toku od místního kodéry](media-services-live-streaming-with-onprem-encoders.md)
- [Kvót a omezení](media-services-quotas-and-limitations.md).

##<a name="protecting-content"></a>Ochrana obsahu

###<a name="dynamic-encryption"></a>Dynamické šifrování

Azure Media Services umožňuje zabezpečená médií od okamžiku, kdy ponechá počítače pomocí úložiště, zpracování a doručení. Media Services umožňuje poskytovat obsah dynamicky zašifrovaných šifrování AES (Advanced Standard) (pomocí kláves 128bitové šifrování) a běžných šifrování (CENC) pomocí PlayReady a/nebo Widevine DRM. Media Services také poskytuje služby pro doručování AES klíče a PlayReady licence oprávněnými klientům.

V současnosti dá zašifrovat následujících datových proudů formátů: HLS MPEG POMLČKU a přitom zajistit hladký přenos. Nelze zašifrovat konfigurace streaming format nebo postupné stahování.

Pokud chcete pro Media Services šifrování aktivum, potřebujete přidružení šifrovací klíč (CommonEncryption nebo EnvelopeEncryption) s vaší materiálů a taky nakonfigurovat se tak mohli ověřovat zásady pro klávesu.

Pokud chcete vysílat úložiště šifrované materiálů, musíte nakonfigurovat majetku doručení zásad k určení způsobu předvádění vaší materiálů.

Požádání proudu přehrávačem Media Services pomocí zadaného klíče dynamicky zašifrovat obsah pomocí obálky (plus pár AES) nebo běžné šifrování (PlayReady nebo Widevine). Dešifrovat proudu přehrávač bude požádat klávesu služby klíčové doručení. Služba rozhodnout, zda je uživatel oprávnění k získání klíče, vyhodnotí se tak mohli ověřovat zásad, které jste zadali pro klávesu.


###<a name="token-restriction"></a>Tokenu omezení

Zásady obsahu klíčové autorizace může mít jeden nebo více omezení se tak mohli ověřovat: otevření, token omezení nebo IP omezení. Token s omezeným přístupem zásad musí opatřeny token Vystavitel tak, že Secure tokenu Service (Služba tokenů zabezpečení). Media Services podporuje tokeny jednoduché Web tokeny (SWT) a formátem JSON Web tokenu (JWT). Media Services neposkytuje zabezpečené tokenu služby. Můžete vytvořit vlastní STS nebo využít Microsoft Azure ACS tokenů problému. STS musí být nakonfigurované pro vytvoření token podepsané zadaný deklarace spolu s problém, které jste zadali v konfiguraci tokenu omezení. Služba klíčové doručení Media Services vrátí požadované klíč (nebo licenci) klienta, pokud platí tokenu a deklarací tokenu se shodují můžou být nakonfigurované pro klíč (nebo licence).

Při konfiguraci tokenu omezení zásad, je třeba určit ověření primární klíč, Vystavitel a parametrech cílové skupiny. Ověření primární klíč obsahuje klíč, který byl podepsán tokenu, které vystavitel služba zabezpečené tokenu problémy tokenu. Cílové skupiny (někdy se jí říká obor) popisuje záměr tokenu nebo zdroje tokenu povolí přístup k. Služby klíčové doručení Media Services ověří, že tyto hodnoty v tokenu odpovídat hodnotám na šabloně.

Další informace najdete v následujících článcích:

[Ochrana obsahu přehled](media-services-content-protection-overview.md)
[chránit pomocí AES 128](media-services-protect-with-aes128.md)
[chránit pomocí DRM](media-services-protect-with-drm.md)

##<a name="delivering"></a>Předvádění

###<a id="dynamic_packaging"></a>Dynamické balení

Při práci s Media Services se doporučuje kódování mezzanine souborů do adaptivní bitrate MP4 nastavte a následný převod sadu na požadovaný formát pomocí [Dynamického obalu](media-services-dynamic-packaging-overview.md).


###<a name="streaming-endpoint"></a>Přenos koncový bod

StreamingEndpoint představuje streamování služba, která přináší obsah přímo do aplikace přehrávače klienta nebo na doručení sítě obsahu (CDN) pro další rozdělení (Azure Media Services nyní obsahuje integrace Azure CDN.) Odchozí proudu od služby StreamingEndpoint může být živou toku nebo videa na vyžádání materiálů ve vašem účtu Media Services. Kromě toho můžete určit kapacitu StreamingEndpoint služby pro zpracování potřeb rostoucí šířky pásma úpravou jednotkách (označovaná taky jako streamování jednotky). Doporučujeme přidělit jednu nebo více jednotek měřítko pro aplikace v pracovním prostředí. Jednotkách umožňují vyhrazené výstupní kapacitu, můžete si koupili úsecích 200 MB / a další funkce, která zahrnuje aktuálně použití dynamického balení.

Doporučujeme používat dynamické balení nebo dynamické šifrování. Použití těchto funkcí, musí mít aspoň jeden streamování jednotku pro koncový bod, ze kterého chcete vysílat. Další informace najdete v tématu [Měřítko streamování jednotky](media-services-portal-manage-streaming-endpoints.md).

Ve výchozím nastavení můžete mít až 2 streamování koncové body ve vašem účtu Media Services. Požádat o vyšší limit, najdete v článku [kvóty](media-services-quotas-and-limitations.md).

Je pouze faktura po vaší StreamingEndpoint v stav po spuštění.

###<a name="asset-delivery-policy"></a>Zásady doručení materiálů

Jedním z kroků v pracovním postupu doručování obsahu Media Services je konfigurace [zásad doručení pro prostředky ](https://msdn.microsoft.com/library/azure/dn799055.aspx), které chcete být streamují. Zásady doručení materiálů říká Media Services způsobu položka nedoručila ji: do kterých streamování protokolu má vaše materiálů dynamicky sbalí (pro třeba MPEG POMLČKU, HLS, hladký přenos nebo všechny), zda chcete dynamicky zašifrovat vaší materiálů a jak (obálky nebo běžné šifrování).

Pokud máte úložiště šifrované materiálů, než můžete streamují vaší materiálů, streamování server Odebere šifrovací úložiště a přenáší datové proudy obsahu pomocí zásad zadaný doručení. Například předvádění vaší materiálů zašifrované pomocí šifrování AES (Advanced Standard) šifrovací klíč, nastavte typ zásad DynamicEnvelopeEncryption. Odebrat úložiště šifrování a materiálů v Vymazat můžete vysílat datovými proudy, nastavte typ zásad pro NoDynamicEncryption.

###<a name="progressive-download"></a>Postupné stahování

Postupné stahování můžete spustit přehrávání média před stažená celý soubor. Pouze postupně si můžete stáhnout soubor MP4.

Poznámka: šifrované materiálů musí dešifrovat, když chcete, aby je k dispozici pro postupné stahování.

Uživatelům poskytnout postupné stahování adresy URL, je třeba nejdříve vytvořit OnDemandOrigin locator. Vytvoříte locator, získáte základní cesta k majetku. Pak budete muset přidat název souboru MP4. Příklad:

http://amstest1.Streaming.mediaservices.Windows.NET/3c5fe676-199c-4620-9B03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.MP4

###<a name="streaming-urls"></a>Streamování adresy URL

Přenos obsahu pro klienty. Uživatelům poskytnout streamování adresy URL, je třeba nejdříve vytvořit OnDemandOrigin locator. Vytvoříte locator, získáte základní cesta k majetku s obsahem, kterou chcete vysílat. Však mohli tento obsah můžete vysílat datovými proudy potřebujete změnit tato cesta dál. Vytvářet úplnou adresu URL do datových proudů soubor, musí zřetězíte hodnotu cesty ikoně umístění a manifest (filename.ism) název souboru. Potom připojení k cesta URL /Manifest a vhodný formát (v případě potřeby).

Vysílat datovými proudy obsah přes připojení SSL. K tomuto účelu zkontrolujte, že streamování adresy URL začínat HTTPS.

Všimněte si, že můžete jenom přenášet přes připojení SSL Pokud streamování koncový bod, ze které doručení obsahu byl vytvořen za 10 září 2014. Pokud streamování adresy URL založené na datových proudů koncové body vytvořené po září 10, obsahuje adresa URL "streaming.mediaservices.windows.net" (nový formát). Streamování adresy URL, které obsahují "origin.mediaservices.windows.net" (starý formát) nepodporují SSL. Pokud je vaše adresa URL v původním formátu a chcete mít možnost streamování SSL, vytvořte nový streamování koncový bod. Použití adresy URL založeno na nový streamování koncový bod ke streamování obsahu SSL.

Následující seznam popisuje různé datové proudy formáty a uvádí příklady:

- Hladký přenos

{streamování koncového bodu služby media název účtu name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8B70-203e86b0df58/BigBuckBunny.ISM/manifest


- MPEG POMLČKA

{streamování koncového bodu služby media název účtu name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8B70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=MPD-Time-CSF)



- Apple HTTP Live streamování V4 (HLS)

{streamování koncového bodu služby media název účtu name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8B70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl)



- Apple HTTP Live streamování V3 (HLS)

{streamování koncového bodu služby media název účtu name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl-v3)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8B70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl-v3)

- Konfigurace (pro pouze nabyvatelů licence Adobe PrimeTime/Access)

{streamování koncového bodu služby media název účtu name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8B70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=f4m-f4f)


##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
