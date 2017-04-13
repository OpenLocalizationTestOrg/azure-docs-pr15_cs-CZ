<properties 
    pageTitle="Azure Media Services fragmentovaných MP4 live jedí specifikace | Microsoft Azure" 
    description="Tato specifikace popisuje formátem fragmentován MP4 na základě živou streamování požití služby Microsoft Azure Media Services a Protocol (protokol). Microsoft Azure Media Services poskytuje živou streamování služby, která umožňuje zákazníkům živou události můžete vysílat datovými proudy a vysílání obsah v reálném čase pomocí Microsoft Azure jako platformu cloudové. V tomto dokumentu také popisuje doporučené postupy při vytváření vysoce nadbytečné a robustních služby live jedí mechanismy." 
    services="media-services" 
    documentationCenter="" 
    authors="cenkdin" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"     
    ms.author="cenkdin;juliako"/>

#<a name="azure-media-services-fragmented-mp4-live-ingest-specification"></a>Azure Media Services fragmentovaných MP4 live jedí specifikace

Tato specifikace popisuje formátem fragmentován MP4 na základě živou streamování požití služby Microsoft Azure Media Services a Protocol (protokol). Microsoft Azure Media Services poskytuje živou streamování služby, která umožňuje zákazníkům živou události můžete vysílat datovými proudy a vysílání obsah v reálném čase pomocí Microsoft Azure jako platformu cloudu. V tomto dokumentu také popisuje doporučené postupy při vytváření vysoce nadbytečné a robustní služby live jedí mechanismy.


##<a name="1-conformance-notation"></a>1. zápis shody

Klíčová slova "je", "Nesmí", "REQUIRED", "SHALL", "Není se", "SHOULD", "Není by MĚL", "Doporučené", "Může" a "Volitelné" v tomto dokumentu se vyhodnotí podle popisu v RFC 2119.

##<a name="2-service-diagram"></a>2. Diagram služby 

Následující obrázek ukazuje nejvyšší úrovně architektura služby live streamování ve službě Microsoft Azure Media Services:

1.  Dynamický Encoder posune živou kanálů kanály, které jsou vytvořené a zřízení prostřednictvím Microsoft Azure Media Services SDK.
2.  Kanály, aplikací a přenos koncového bodu v Microsoft Azure Media Services úchyt všechny živou streamování funkcí včetně ingest, formátování, cloudu DVR zabezpečení, škálovatelnost a redundance.
3.  Volitelně Zákazníci může můžete nasadit vrstvě CDN mezi datových proudů koncového bodu a koncové body klienta.
4.  Tok koncové body klienta z koncového bodu datových proudů prostřednictvím protokolu HTTP adaptivní streamování protokolů (například hladký přenos, ČÁRKU, konfigurace nebo HLS).

![obrazek1][image1]


##<a name="3-bit-stream-format--iso-14496-12-fragmented-mp4"></a>3. bit tokem formát – ISO 14496 12 fragmentován MP4

Formát drátěný pro živou streamování jedí, které je popisované v tomto dokumentu se podle [ISO 14496 12]. Naleznete v tématu [[MS-SSTR]](http://msdn.microsoft.com/library/ff469518.aspx) podrobné vysvětlení formátu MP4 fragmentován a rozšíření pro oba soubory poskytují a live streamování požití.

###<a name="live-ingest-format-definitions"></a>Živá jedí definice formát 

Tady je seznam speciálních formátovacích, které definice, které se vztahují k živé jedí do Microsoft Azure Media Services:

1. "Ftyp", LiveServerManifestBox a "okno moov" musí být odesílány spolu s každou žádost (HTTP POST).  Je třeba odeslat na začátku proudu a pokaždé, když encoder musí znovu připojíte k životopisu jedí proudu.  Další informace naleznete v části 6 v části [1].
2. Oddíl 3.3.2 [1] definuje volitelná pole s názvem StreamManifestBox pro živou jedí. Z důvodu směrování logickou Vyrovnávání zatížení Microsoft Azure použití tohoto políčka se nedá použít a by MĚL být při ingesting do Microsoft Azure Media služby. Toto pole neobsahuje data, Azure Media Services tiše ji ignoruje.
3. TrackFragmentExtendedHeaderBox podle 3.2.3.2 v [1] je nutný pro každou část.
4. Verze 2 TrackFragmentExtendedHeaderBox by MĚL být použito generovat segmenty médií s identickými adresy URL v několika datacentrech. Pole index fragment je potřeba k více datacentra převzetí indexu založeného streamování formáty jako je Apple HTTP Live streamování (HLS) a na základě index MPEG-přerušované čáry.  Povolit více datacentra překlopení, fragment indexu musí být synchronizovány napříč několika kódovací zařízení a tlačítka Zvětšit 1 pro každou část po sobě jdoucí médií i přes encoder restartování nebo k chybám.
5. Oddíl 3.3.6 [1] definuje pole s názvem MovieFragmentRandomAccessBox (mfra), kterou může na konci živou požití naznačit SESTAVENÁ (koncové z toku) kanál. Z důvodu logickou ingest Azure Media Services zastaralé použití SESTAVENÁ (koncové z toku) a pole "mfra" pro živou požití by MĚL být neodeslaných. Odeslání Azure Media Services tiše ji ignoruje. Je vhodné použít [Kanálu obnovit](https://msdn.microsoft.com/library/azure/dn783458.aspx#reset_channels) obnovit stav bodu ingest a také se doporučuje používat [Program přestane](https://msdn.microsoft.com/library/azure/dn783463.aspx#stop_programs) ukončit prezentaci a proudu.
6. Doba trvání fragment MP4 by MĚL být konstanta, abyste mohli zmenšení manifesty klienta a zlepšit klienta stažení heuristiku pomocí značek opakování.  Doba trvání mohou kolísání za účelem kompenzace sazby není celé číslo snímku.
7. Doba trvání fragment MP4 by MĚL být mezi přibližně 2 a 6 sekund.
8. Časová razítka fragment MP4 a indexy (TrackFragmentExtendedHeaderBox fragment_ absolute_ čas a fragment_index) by se ukládají vzestupně.  Sice pružné duplicitní fragmentů Azure Media Services obsahuje hodně omezenou možnost uspořádání fragmentů podle předpisů rozhraní na časovou osu média.

##<a name="4-protocol-format--http"></a>4 formát protocol – HTTP

ISO fragmentován MP4 na základě služby live jedí pro standardní dlouhodobě spuštěné HTTP POST požádat o přenosu zakódovaný multimediální data balíčku ve formátu MP4 fragmentován ke službě Microsoft Azure Media Services používá. Každý příspěvek by měl HTTP přesměruje dokončení fragmentován MP4 tokem bit ("toku") počínaje začínající pole záhlaví (políčko "ftyp", "Live Manifest krabice Server" a "moov") a pokračováním posloupnost fragmenty (políčka "moof" a "mdat"). Syntaxe adresy URL pro žádost HTTP POST naleznete v části 9.2 v [1]. Příklad POST URL je: 

    http://customer.channel.mediaservices.windows.net/ingest.isml/streams(720p)

###<a name="requirements"></a>Požadavky

Tady jsou podrobné požadavky:

1. Encoder by měly začít vysílání tak, že pošlete žádost HTTP POST s prázdné "text" (nulové délky obsahu) pomocí požití adresu URL. Takto můžete zjistit, rychle pokud platí živou požití koncového bodu a pokud žádné ověřování a další podmínky povinné. Za protokolu HTTP serveru nebude moct odesílat odpovědi zpět HTTP do dostali celá žádost textem příspěvku. Zadaných dlouhodobé přírodní živou událost, bez tento krok encoder nejspíš nebudou moct zjistit všechny chyby, dokud se dokončí svou činnost, odesílání všechna data.
2. Encoder musí zpracovat chyby nebo problémy při ověřování důsledku (1). Pokud (1) se mu 200 odpovědí, pokračovat.
3. Encoder musí začínat fragmentovaných proudu MP4 novou žádost o HTTP příspěvek.  Datové musí začínat pole záhlaví a za ním uveďte neúplné.  Poznámka: pole "ftyp", "Live Manifest krabice Server" a "moov" (v tomto pořadí) musí být odeslána při každém požadavku, i když encoder musí připojit, protože předchozí žádost byl ukončen před koncem proudu. 
4. Encoder musíte použít blokovém kódování přenosu pro odeslání takové alternativní řešení je možné předpovídání celý délka obsahu živou události.
5. Po události, po odeslání fragment poslední encoder musí končit řádně blokovém kódování přenosu posloupnosti zpráv (většina HTTP klienta štosech zpracovat jeho automaticky). Encoder čekat pro službu vrátit kód konečné odpovědi a potom ukončit připojení. 
6. Encoder nesmí použijte jmenné Events() způsobem popsaným v 9.2 v [1] pro živou požití do Microsoft Azure Media Services.
7. Pokud žádost HTTP POST ukončí nebo vyprší její časový limit před koncem proudu s chybou TCP, musí encoder problémů příspěvek žádost o nové pomocí nové připojení a postupujte podle požadavky nad další požadavku, že encoder musí opakované předchozí dva MP4 neúplné pro každou sledovat v proudu vrátit a obnovit bez úvodní informace o nespojitosti na časové ose médií.  Opětovné odeslání poslední dvě neúplné MP4 pro každou sledování zajišťuje, že je beze ztráty dat.  Jinými slovy Pokud proudu obsahuje audiovizuálnímu přenosu sledování a aktuální žádosti příspěvek se nezdaří, encoder musí opětovné připojení a poslal vám poslední dvě fragmentů pro zvuk plánu, které byly dříve úspěšně odesláno, a poslední dvě fragmentů stopa videa, které jste předtím úspěšně odeslaly, aby bylo zajištěno, že je beze ztráty dat.  Encoder vedou "předané" vyrovnávací fragmenty média, která nových po opětovném připojení.

##<a name="5-timescale"></a>5. časová osa 

[[MS-SSTR]](https://msdn.microsoft.com/library/ff469518.aspx) popisuje použití "Osy" SmoothStreamingMedia (oddíl 2.2.2.1), StreamElement (oddíl 2.2.2.3), StreamFragmentElement(2.2.2.6) a LiveSMIL (oddíl 2.2.7.3.1). Pokud hodnota osy není k dispozici použita výchozí hodnota 10 000 000 (10 MHz). Přestože specifikace formátu hladký přenos nebude bránit použití dalších hodnot osy většina používá implementace encoder výchozí hodnotu (10 MHz) generovat hladký přenos jedí data. Vzhledem k funkci [Dynamické balení Azure Media](media-services-dynamic-packaging-overview.md) je doporučená pro účely zvukových datových proudů 90 kHz osy datových proudů videa a 44,1 nebo 48.1 kHz. Pokud pro různé datové proudy používají různé osy hodnot, musí být odeslána časové osy úrovně proudu. Získáte [[MS-SSTR]](https://msdn.microsoft.com/library/ff469518.aspx).     

##<a name="6-definition-of-stream"></a>6. definice "Proudu"  

"Toku" je základní jednotka operace živou požití pro vytváření prezentace online, zpracování datových proudů převzetí a redundance scénáře. "Toku" je definována jako jeden jedinečný fragmentován MP4 bit tokem která může obsahovat jednoho sledování nebo víc skladeb. Živého prezentace může obsahovat jedno nebo více datových proudů v závislosti na konfiguraci živou encoder(s). Následující příklady ilustrují různé možnosti použití stream(s) při vytváření prezentace úplné online.

**Příklad:** 

Zákazník požaduje se vytvořit prezentaci živou streamování, která zahrnuje následující bitrates zvuk a video:

Videa – 3000 kB/s, 1 500 kb/s, 750 kb/s

Audio – 128 kb/s

###<a name="option-1-all-tracks-in-one-stream"></a>Možnost 1: Všechny stopy v jedné toku

V tuto možnost jednoho encoder vygeneruje všechny stopy Zvuk a video a seskupit do jedné fragmentován MP4 bit tokem které pak se odešlou pomocí jednoho připojení HTTP příspěvek. V tomto příkladu je pouze jeden toku pro prezentaci online:

![obrazek2][image2]

###<a name="option-2-each-track-in-a-separate-stream"></a>Možnost 2: Všechny stopy v samostatné proudu

V této možnosti encoder(s) pouze jednu umístění sledovat do každého Fragment MP4 dat a zveřejňují všech datových proudů prostřednictvím několik samostatných HTTP připojení. To může provést kodéry jeden nebo více kodéry. Z živého požití je prezentaci online skládající se ze čtyř datových proudů.

![image3][image3]

###<a name="option-3-bundle-audio-track-with-the-lowest-bitrate-video-track-into-one-stream"></a>Možnost 3: Seskupit zvuku souladu s plánu nejnižší videa bitrate do jednoho toku

V této možnosti zákazník vybere sbalení zvuku souladu s nejnižší stopa bitrate videa do jednoho Fragment MP4 dat a nechat ostatní dvě videa drah každý právě vlastní toku. 


![image4][image4]


###<a name="summary"></a>Souhrn

Co je uveden nad není úplný seznam všech možných požití možnosti v tomto příkladu. Co matter z toho seskupení skladeb do datových proudů nepodporuje živou požití. Zákazníci a kódovací dodavatelé můžete zvolit vlastní implementace podle technickým složitosti, kvalifikace encoder a redundance a důležité informace o převzetí. Ale je třeba poznamenat, že ve většině případů Přibyla jedinou zvuku sledování celé živou prezentaci tak, aby bylo nutné zajistit healthiness proudu ingest obsahující zvukové sledování. Tento pozornost často výsledkem použití zvuku sledování do vlastního datového proudu (například možnost 2) nebo sadu s nejnižší stopa bitrate videa (například možnost 3). Taky pro lepší redundance a odolnost proti chybám odesláním sledovat stejné zvuku ve dvou různých proudů (možnost 2 s nadbytečné zvukové stopy) nebo navazují, které zvukové sledování aspoň dva nejnižší bitrate videa drah (možnost 3 se zvukem seskupeny v aspoň dva proudů videa) je vhodné pro služby live jedí do Microsoft Azure Media Services.

##<a name="7-service-failover"></a>7. převzetí služby 

Zadaný druh živou streamování je dobré selháním kritické pro zajištění dostupnosti služby. Microsoft Azure Media Services slouží ke zpracování různé typy potíží včetně chyby sítě chyby serveru, úložiště problémy, atd. Při použití ve spojení s velkými převzetí logikou ze strany živou encoder zákazníka dosáhnete vysoce spolehlivé služby live datových proudů z cloudu.

V této části bude probereme tato služba převzetí scénáře. V tomto případě selhání děje jinam, postupujte v rámci služby a projevuje jako chybu v síti. Tady jsou některé doporučení pro provádění encoder zpracování převzetí služby:


1. Použijte 10 druhý vypršení časového limitu pro vytvoření připojení pomocí protokolu TCP.  Pokud pokus o připojení k trvá déle než 10 sekund, přerušit operaci a zkuste to znovu. 
2. Při odesílání zprávy bloky žádost HTTP použijte krátké časový limit.  Pokud N sekund, než je doba trvání fragment cílové MP4, použijte časový limit odeslání mezi N a 2 n sekund. časový limit 6 až 12 sekund pomocí například fragment MP4 trvá 6 sekund.  Pokud časový limit, obnovení připojení, otevřete nové připojení a obnovit toku jedí na nové připojení. 
3. Udržujte postupné vyrovnávací paměť obsahující poslední dvě fragmenty, pro každou sledování, které byly úspěšně a úplně odeslány službu.  Pokud žádost HTTP příspěvku o proudu skončí nebo vyprší její časový limit před na konec datového proudu, otevřete nové připojení zahajte další žádost HTTP příspěvek, opakované záhlaví toku, opakované poslední dvě neúplné pro každou sledování a pokračovat proudu bez úvodní informace o přerušení na časové ose média.  To bude snížit šanci ztrátou dat.
4. Doporučujeme, aby encoder není omezena počet opakování navázat spojení nebo obnovení streamování po dojde k chybě TCP.
5. Po TCP Chyba:
    1. Aktuální připojení, je potřeba zavřít a pro novou žádost o HTTP POST musí vytvořit nové připojení.
    2. Nové HTTP příspěvek URL musí být stejný jako počáteční adresu URL příspěvek.
    3. Nový příspěvek HTTP musí obsahovat záhlaví toku (políčko "ftyp", "Live Manifest krabice Server" a "moov") shodné do záhlaví toku v prvním příspěvkem DISKUSE.
    4. Poslední dvě neúplné poslali ke každé sledování musí dotaz a datových proudů obnovit, aniž by úvodní informace o přerušení na časové ose média.  Časová razítka fragment MP4 nutné zvětšit průběžně, i přes HTTP POST žádosti.
6. Encoder by MĚL ukončit žádost HTTP příspěvek, pokud dat není odesílaného sazbou odpovídající s dobou trvání MP4 fragmentu.  Požadavek na HTTP příspěvek, která neodesílá dat můžete zabránit rychle odpojení od jej v případě aktualizace služby Azure Media Services.  Z tohoto důvodu HTTP POST pro sparse skladeb (ad signál) by MĚL být krátké životnost, ukončení hned, jak se odesílá sparse fragment.

##<a name="8-encoder-failover"></a>8. převzetí encoder

Převzetí Encoder je druhý typ převzetí scénář, který musí být adresovanou na začátku do konce živou streamování doručení. V tomto případě se na straně encoder stalo chybovém stavu. 

![image5][image5]


Tady jsou očekávání z koncového bodu živou požití důvody encoder převzetí chování:

1. Abyste mohli pokračovat streamování by MĚL vytvořena nová instance encoder, jak je ukázáno v diagramu nahoře (kanál profilu z 3000 k videa s přerušované čáry).
2. Nové encoder používají stejnou adresu URL pro požadavky HTTP POST jako selhalo instance.
3. Žádost o nové encoder příspěvek musí obsahovat stejné pole fragmentovaných MP4 záhlaví jako nezdařeném uložení instance.
4. Nové encoder musí správně synchronizuje s všechny ostatní pracovního kodéry pro stejný prezentace online generovat synchronizované vzorky hlasových/video s hranice zarovnanými fragmentu.
5. Nové toku musí být sémanticky srovnatelná s předchozí proudu a zaměnit úrovni záhlaví a fragmentu.
6. Nové encoder měli zkusit minimalizovat ztrátou dat.  Fragment_absolute_time a fragment_index médií fragmenty měli zvýšit z bodu, kde encoder poslední přerušili.  Fragment_absolute_time a fragment_index měli zvýšit nepřetržitý způsobem, ale je dovolená představující přerušení v případě potřeby.  Azure Media Services bude ignorovat neúplné, které je už obdržel a zpracování tak, aby byl vhodnější chyba okraji opětovné odeslání neúplné než chcete Představte nespojitosti na časové ose média. 

##<a name="9-encoder-redundancy"></a>9. redundance encoder 

U některých důležitých live události, že služba i vyšší dostupnost a kvalitu prostředí, je vhodné používat aktivní nadbytečné kodéry dosáhnout bezproblémové převzetí bez ztráty dat.

![image6][image6]

Jak je ukázáno v diagramu nahoře, existují dva okruhem kodéry současné stisknutí dvě kopie každé toku do služby live. Toto nastavení je podporováno, protože Microsoft Azure Media Services má možnost odfiltrovat duplicitní neúplné podle časového razítka ID a fragment proudu. Výsledný živou toku a archiv budou typu single kopii všechny proudů, která je nejlepší možné agregace ze dvou zdrojů. Například v hypotetické krajní případ, pokud je jeden encoder (nemusí být stejný jako ten) systém v libovolném bodě v čase pro každý toku, výsledný živou proudu ze služby, budou nepřetržitý bez ztráty dat. 

Požadavek na tento scénář je téměř shodný požadavky v případě Encoder převzetí s výjimkou spuštěný druhé sadě kodéry ve stejnou dobu jako primární kodéry.

##<a name="10-service-redundancy"></a>10. redundance služby  

Pro vysoce nadbytečné globální rozdělení je někdy potřebná k více oblastí zálohu případě havárie místní. Rozbalení na topologii "Encoder redundance", Zákazníci můžete zvolit nasazení nadbytečné služby v jiné oblasti, které je připojené 2 sadou kodéry. Zákazníci může taky spolupracovat s zprostředkovatele CDN pro nasazení GTM (globální správce přenosy) před nasazení dvou služeb Bezproblémová směrovat přenosy v síti klienta. Požadavky kodéry jsou stejná jako "Encoder redundance" případ s jedinou výjimkou, že druhá sada kodéry nutné zdůraznit k jiné služby live jedí koncový bod. Následující obrázek ukazuje toto nastavení:

![image7][image7]

##<a name="11-special-types-of-ingestion-formats"></a>11. zvláštní typy formátů požití 

Tato část popisuje některé speciální druh živou požití formáty navržené tak, aby zpracovat některé konkrétních situacích.

###<a name="sparse-track"></a>Sparse sledování

Při předvádění prezentace online streamování pomocí klienta RTF prostředí, je často nutné čas synchronizován událostí odeslání nebo signalizuje uvnitř pásma s hlavním multimediální data. Příkladem to je dynamické živou vložení služby Active Directory. Tento typ signalizace událostí se liší od běžná streamování svou sparse charakteristikou zvuk a video. Jinými slovy signalizační data obvykle nedojde z toho důvodu nepřetržitě a interval může být obtížné předpovídání. Koncepce Sparse sledování bylo speciálně jedí a vysílání signalizační dat uvnitř pásma.

Tady je doporučené implementace pro ingesting sparse sledování:

1. Vytvoření samostatného fragmentován MP4 bit proudu obsahující pouze sparse stop bez skladeb zvuk a video, které.
2. Do pole"Live serveru manifestu" v části 6 podle [1] "parentTrackName" parametr použijte k určení název nadřazené sledovat. Získáte další informace v části 4.2.1.2.1.2 v [1].
3. Do pole"Live serveru manifestu", musí být manifestOutput nastavena na hodnotu "true".
4. Možnost sparse přírodní signalizační události, doporučujeme:
    1. Na začátku živou události, odešle encoder do polí Počáteční záhlaví do služby, která by umožňovala službu zaregistrujte sparse sledovat v manifest klienta.
    2. Encoder by MĚL ukončit žádost HTTP POST při dat není právě odesílá.  Dlouhodobé HTTP POST neodesílá dat můžete zabránit Azure Media Services rychle odpojení encoder v případě restartování aktualizace nebo serveru služby jako serveru médií bude dočasně blokované v operaci přijímání v socket. 
    3. V době, kdy signalizační dat není k dispozici encoder SHOULD zavřít HTTP POST požádat o.  Při aktivní žádost příspěvek encoder má odeslat data 
    4. Při odesílání sparse neúplné encoder můžete nastavit explicitní délka obsahu záhlaví, pokud je k dispozici.
    5. Při odesílání sparse fragment se nové připojení, by měly začít encoder odesílání z pole záhlaví a za ním uveďte nový neúplné. Toto je zpracovávání případě, že se mezi stalo převzetí a nové sparse je připojení k serveru nový, který není vidět sparse sledování před.
    6. Fragment sparse sledování bude k dispozici ke klientovi při odpovídající nadřazené sledovat část, která má stejné nebo větší časové razítko hodnota je k dispozici ke klientovi. Například pokud sparse fragment má časové razítko t = 1 000, předpokládá se, po klienta uvidí video (Pokud je v poli Název nadřazené sledování videa) fragmentu časové razítko 1000 nebo za, můžete stáhnout sparse fragment t = 1 000. Upozorňujeme, že skutečné signálu dobře lze použít pro na jiné místo na časové ose prezentace pro určené účel. Ve výše uvedeném příkladu je možné, sparse fragment t = 1 000 obsahuje data XML, která je pro vložení do umístění, která je několik sekund, než Ad později.
    7. Datové části sparse sledování fragment může být různé způsob změny formátu (například XML nebo text nebo binární, atd.) v závislosti na různých scénářích. 


###<a name="redundant-audio-track"></a>Nadbytečné zvuková stopa

V protokolu HTTP adaptivní streamování běžný (například hladký přenos nebo přerušované čáry) je často pouze jeden zvuku sledovat v celé prezentaci. Na rozdíl od videa skladeb, které mají víc kvalitě pro klienta můžete vybírat v chybové podmínky zvukových sledování může být v jednom místě Chyba při požití toku, který obsahuje zvukové sledování se přeruší. 

Tento problém můžete vyřešit, Microsoft Azure Media Services podporuje živou požití nadbytečné zvukových stop. Cílem je, že stejný zvuku sledování můžou posílat tisknutím v různých proudů. Během službu pouze zaregistruje zvuku sledování jednou na manifestu klienta, je možné používat nadbytečné zvukových stop jako zálohy pro načítání zvuku neúplné Pokud primární zvuku sledování je potíže. Aby bylo možné jedí nadbytečné zvukových stop encoder musí:

1. Vytvoření stejné zvuku sledování ve více Fragment MP4 bit-datových proudů. Nadbytečné zvukových stop musí být sémanticky srovnatelná s přesně stejné fragment časová razítka a zaměnit úrovni záhlaví a fragmentu.
2. Zkontrolujte, že je "zvuk" položka v živé serveru manifestu (oddíl 6 v části [1]) shodovat s pro všechny nadbytečné zvukových stop.

Tady je doporučené implementaci u nadbytečné zvukových stop:

1. Odeslání každý jedinečný zvuku sledování v toku samostatně.  Odeslat taky nadbytečné toku pro každou z těchto datových proudů zvuku sledování, kde 2 proudu se liší od 1st pouze podle identifikátoru v adrese URL příspěvek HTTP: {protokol} :// {adresa serveru} / {publikování path}/Streams({identifier}) bodu.
2. Odeslání dvou nejnižší videa bitrates pomocí samostatných datových proudů. Každý z těchto datových proudů SMÍ obsahovat také kopie každé jedinečné zvuku sledování.  Třeba když více jazyků podporovaných, tyto proudů SMÍ obsahovat zvukových stop pro každý jazyk.
3. Pomocí instance samostatný server (encoder) na kódování a odeslat nadbytečné datových proudů podle (1) a (2). 



##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


[image1]: ./media/media-services-fmp4-live-ingest-overview/media-services-image1.png
[image2]: ./media/media-services-fmp4-live-ingest-overview/media-services-image2.png
[image3]: ./media/media-services-fmp4-live-ingest-overview/media-services-image3.png
[image4]: ./media/media-services-fmp4-live-ingest-overview/media-services-image4.png
[image5]: ./media/media-services-fmp4-live-ingest-overview/media-services-image5.png
[image6]: ./media/media-services-fmp4-live-ingest-overview/media-services-image6.png
[image7]: ./media/media-services-fmp4-live-ingest-overview/media-services-image7.png

 