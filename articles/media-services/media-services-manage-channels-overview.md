<properties 
    pageTitle="Základní informace o Live vodě pomocí Azure Media Services | Microsoft Azure" 
    description="Toto téma obsahuje přehled živou vodě pomocí Azure Media Services." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="10/17/2016"
    ms.author="juliako"/>

#<a name="overview-of-live-steaming-using-azure-media-services"></a>Základní informace o Live vodě pomocí Azure Media Services

##<a name="overview"></a>Základní informace

Při předvádění živou streamování událostí pomocí Azure Media Services jsou zahrnuty běžně tyto prvky:

- Fotoaparát, který se používá k vysílání události.
- Dynamický videa encoder, převede signály z fotoaparátu na datové proudy, které se odesílají službě živou streamování.

    V případě potřeby více živou čas synchronizován kodéry. U některých důležitých live události, že služba velmi vysoké dostupnosti a kvalitu prostředí, je vhodné používat aktivní nadbytečné kodéry se synchronizací čas dosáhnout bezproblémové převzetí bez ztráty dat.
- Dynamický služby datových proudů, který umožňuje následujícím způsobem:
    
    - jedí živou obsahu pomocí různých live protokolů datových proudů (například RTMP nebo hladký přenos)
    - (volitelně) kódujte vašeho proudu do adaptivní bitrate toku
    - zobrazení náhledu živou toku,
    - záznam a uložení požití obsahu za účelem se streamují vyšší (poskytují)
    - předvádění obsahu prostřednictvím běžné streamování protokoly (například MPEG POMLČKU hladký, HLS, konfigurace) přímo zákazníkům, nebo na doručení sítě obsahu (CDN) pro další rozdělení.


**Microsoft Azure Media Services** (AMS) umožňuje jedí kódovat, náhled, ukládání a doručení živou streamování obsahu.

Doručování obsahu pro zákazníky mohl při předvádění video ve vysoké kvalitě různá zařízení podmínkách jiné síti. K dosažení tohoto umožňuje živou kodéry kódovat toku více bitrate videa proudu (adaptivní bitrate).  Starat o přenos na různých zařízeních umožňuje Media Services [dynamické balení](media-services-dynamic-packaging-overview.md) dynamicky znovu balíček toku na různé protokoly. Media Services podporuje odprezentování následující adaptivní přenosová streamování technologie: HTTP Live datových proudů (HLS) hladký přenos, MPEG POMLČKU a konfigurace (pro Adobe PrimeTime/přístup nabyvatelů licence pouze).

V Azure Media Services **kanály**, **programy**a **StreamingEndpoints** zpracování všech živou streamování funkcí včetně ingest, formátování, DVR zabezpečení, škálovatelnost a redundance.

**Kanál** představuje příležitost pro zpracování živou streamování obsahu. Kanálu, který může přijímat o živé vstupní datových proudů následujícími způsoby:

- V místním live encoder odešle více bitrate **RTMP** nebo **Hladký přenos** (fragmentovaných MP4) kanál, který je nakonfigurovaný na **předávací** doručení. **Předávací** doručení při požití datových proudů projít **kanálu**s bez dalšího zpracování. Můžete použít následující živou kodéry, které výstup více bitrate hladký přenos: elementární Envivio, Cisco.  Následující živou kodéry výstup RTMP: Adobe Flash Media Live Encoder (FMLE), Telestream Wirecast a Tricaster transcoders.  Živé encoder můžete poslat taky jednoho bitrate toku na kanál, který není povolený živou kódování, ale, která se nedoporučuje. Při požadavku, nabízí Media Services proudu zákazníkům.

    >[AZURE.NOTE] Předávací metodou způsobem nejvhodnější live streamování při více událostí provádíte přes dlouhé časové období a jste již vložil do místního kodéry. Zobrazit podrobnosti [ceny](/pricing/details/media-services/) .
    
    
- Živé encoder místní odešle jednoduchým bitrate toku kanál, který je povoleno provádět živou kódování s Media Services v některém z následujících formátů: RTMP a přitom zajistit hladký přenos (fragmentovaných MP4). RTP (MPEG-TS) podporuje i, za předpokladu, že máte vyhrazené připojení Azure datacentrem. Následující živou kodéry s výkonem RTMP označují pro práci s kanály tohoto typu: Telestream Wirecast, FMLE. Kanál pak provede živou kódování příchozí jednoho bitrate toku proudu videa s víc bitrate (adaptivní). Při požadavku, nabízí Media Services proudu zákazníkům.


Počínaje Media Services 2.10, když vytvoříte kanál, můžete určit, jakým způsobem se má pro kanál pro příjem vstupního datového proudu a jestli chcete kanálu provádět živou kódování vašeho proudu. Máte dvě možnosti:

- **Žádná** (předávací) – zadejte hodnotu, pokud budete chtít použít živou encoder místní, který bude výstupní více bitrate toku (předávací toku). V tomto případě příchozí proudu zpráva předána do výstupu bez jakékoli kódování. Toto je chování kanálu před 2.10 vydání.  

- **Standardní** – zvolit to hodnotu, pokud budete chtít použít Media Services kódování vašeho jednoho bitrate živou proudu více bitrate proudu. Tento způsob je výhodnější pro změnu velikosti rychle pro časté události. Mějte na paměti, že je fakturační dopad pro živou kódování a jste měli mít na paměti, že byste museli opustit živou kódování kanálu ve stavu "Systém" je možné za fakturační poplatky.  Je vhodné po dokončení kvůli nižším poplatkům za extra hodinové živou streamování události okamžitě zastavit spuštěný kanálů. 

##<a name="comparison-of-channel-types"></a>Porovnání typů kanálu

Následující tabulka obsahuje příručku k porovnání dvou typů kanálu podporované ve službách médií

Funkce|Předávací kanálu|Standardní kanálu
---|---|---
Zadání vstupních hodnot jednoho bitrate zakódovaný do více bitrates v cloudu|Ne|Ano
Maximální rozlišení, počet vrstvy|1080p, 8 vrstvy 60 + snímků /|720p 6 vrstvy snímků 30 /
Zadávání protokoly|RTMP hladký přenos|RTMP hladký přenos a RTP
Price|V tématu [ceny stránky](/pricing/details/media-services/) a klikněte na kartu "Live Video"|V tématu [ceny stránky](/pricing/details/media-services/) 
Maximální doba běhu|24 denně, 7|8 hodin
Podpora pro vložení potřeby|Ne|Ano
Podpora ad signalizace|Ne|Ano
Předávací CEA 608/708 titulků|Ano|Ano
Možnost obnovit z stručný místa v příspěvku informačního kanálu|Ano|Ne (kanálu začne slating po 6 + sekund, než bez zadávání dat)
Podpora pro zadávání GOPs-uniform|Ano|Ne – vstupní musí opraví 2sec GOPs
Podpora k zadání vstupních hodnot proměnné rámeček sazba|Ano|Ne – vstupní musí opraví snímků.<br/>Menší varianty jsou přípustné, například během scény vysoké pohybu. Ale encoder nejde přetáhnout do 10 rámce/s.
Automatické přístupnými kanálů při zadávání kanálu dojde ke ztrátě|Ne|Po 12 hodin, pokud není spuštěný Program 

##<a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders-pass-through"></a>Práce s kanály, které dostanete více bitrate živou toku od místního kodéry (předávací)

Na následujícím obrázku vidíte hlavní části platformy AMS zapojené v pracovním postupu **předávací** .

![Dynamický pracovního postupu](./media/media-services-live-streaming-workflow/media-services-live-streaming-current.png)

Další informace najdete v tématu [práce s kanály, které více přijímání-bitrate živou tok z místního kodéry](media-services-live-streaming-with-onprem-encoders.md).

##<a name="working-with-channels-that-are-enabled-to-perform-live-encoding-with-azure-media-services"></a>Práce s kanály, které jsou povoleny k provedení živou kódování s Azure Media Services

Na následujícím obrázku vidíte hlavní části platformy AMS zapojené v Live streamování postupu, kde je povoleno kanálu provádět živou kódování s Media Services.

![Dynamický pracovního postupu](./media/media-services-live-streaming-workflow/media-services-live-streaming-new.png)

Další informace najdete v tématu [práce s kanály, které jsou povoleny k provedení Live kódování s Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).

##<a name="description-of-a-channel-and-its-related-components"></a>Popis kanálu a související součásti

###<a name="channel"></a>Kanálu

V Media Services jsou zodpovědní za zpracování živou streamování obsahu s [kanálu](https://msdn.microsoft.com/library/azure/dn783458.aspx). Kanálu poskytuje vstupní koncového bodu (jedí adresy URL), pak zadáte k živé transcoder. Kanál dostává živou vstupní datových proudů z živého transcoder a díky kterému je dostupný pro datových proudů prostřednictvím jeden nebo více StreamingEndpoints. Kanály taky poskytují koncový bod preview (Náhled URL), které používáte pro zobrazení náhledu a ověřit vašeho proudu před další zpracování a doručení.

Při vytváření kanálu můžete získat adresu URL ingest a adresu URL náhled. Chcete-li získat tyto adresy URL, kanál nemusí být spuštěného stavu. Až budete připraveni začít předání dat z živého transcoder do kanálu, musíte začít kanál. Jakmile živou transcoder začne ingesting dat, můžete zobrazit náhled vašeho proudu.

Každý účet Media Services může obsahovat více kanály, více programů a více StreamingEndpoints. Podle potřeby šířka pásma a zabezpečení může být StreamingEndpoint služby snaží jeden nebo více kanálů. Všechny StreamingEndpoint můžete si ho naimportovat z libovolné kanálu.


###<a name="program"></a>Program 

[Program](https://msdn.microsoft.com/library/azure/dn783463.aspx) umožňuje určit, publikování a úložiště segmentů v živé proudu. Kanály spravovat aplikace. Vztah kanálu a Program je velmi podobné tradiční médií Pokud kanálu konstantní toku obsahu a program má obor vymezený na některé časových událost na tento kanál.
Můžete zadat počet hodin, chcete-li zachovat zaznamenané obsah do programu nastavením vlastnosti **ArchiveWindowLength** . Tato hodnota můžete nastavit z minimální 5 minut maximálně 25 hodin. 

ArchiveWindowLength také určuje, že maximální množství času klientů můžete požádat o zpět v čase od aktuální pozice live. Programy mohlo by umožnit spuštění přes zadaný časový úsek, ale obsah, který zpozdit délka okno nepřetržitě ignorován. Tato hodnota tato vlastnost také určuje, jak dlouho rozrůstat manifesty klienta.

Jednotlivé aplikace je přidružený aktivum. URL pro přidružené materiálů musí vytvořit pro publikování program. Máte tento locator vám umožní vytváření datových proudů adresy URL, která můžete poskytnout klienty.

Kanálu, který podporuje až tři souběžně spuštěné programy můžete vytvořit více archivy stejné příchozí proudu. Můžete publikovat a archivace různé části události podle potřeby. Požadovanou firmy je například archivace 6 hodin program, ale vysílání pouze posledních 10 minut. K tomu potřebujete k vytvoření dvou souběžně spuštěné programy. Jeden program je nastavena na archivovat 6 hodin události, ale není publikován program. Jiném programu je nastavena pro archivaci 10 minut a tento program je publikován.


##<a name="billing-implications"></a>Důsledky fakturace

Fakturace hned, jak se stát přechody "Spuštění" prostřednictvím rozhraní API, nebude zahájen kanálu.  

Následující tabulka uvádí, jak kanálu státy mapování fakturační stav na portálu rozhraní API a Azure. Všimněte si, že se stav mírně liší rozhraní API a UX portálu Jakmile kanálu, který je ve stavu "Spuštění" prostřednictvím rozhraní API nebo ve stavu "Připravených" nebo "Datových proudů" Azure portálu bude fakturace aktivní.

Ukončit kanálu z fakturace můžete dál, budete muset zastavit kanálu prostřednictvím rozhraní API nebo na portálu Azure.
Zodpovídáte za zastavení kanálů po dokončení práce s kanál. Pokračování fakturace způsobí selhání zastavení kanál.

>[AZURE.NOTE]Když pracujete s standardní kanály, bude se AMS roční přístupnými kanál, která je stále ve "Systém" stavu 12 hodin od vstupní informačního kanálu se ztratí a nejsou žádné programy spuštěné. Však můžete budou pořád vám nebudou účtovat poplatky pro čas, kdy byl kanál ve stavu "Systém".

###<a id="states"></a>Kanálu států a jak se mapují na fakturační režim 

Aktuální stav kanálu. Možné hodnoty:

- **Zastaveno**. Pokud je to výchozí stav kanálu po jeho vytvoření (automaticky spustit byla vybrána na portálu.) V tomto stavu dojde k žádné fakturace. V tomto stavu můžete aktualizovat vlastnosti kanálu ale streamování není povolená.
- **Spuštění**. Bude spuštěn kanál. V tomto stavu dojde k žádné fakturace. Žádné aktualizace nebo datových proudů smí během tento stav. Pokud dojde k chybě kanálu vrátí do ukončení uživatelem stavu.
- **Spuštění**. Kanál je schopen zpracování živou datových proudů. Je teď fakturace použití. Je třeba ukončit kanálu, který chcete zabránit dalším fakturace. 
- **Zastavení**. Kanál je zastavení. V tomto dočasném stavu dojde k žádné fakturace. Žádné aktualizace nebo datových proudů smí během tento stav.
- **Odstranění**. Probíhá odstranění kanálu. V tomto dočasném stavu dojde k žádné fakturace. Žádné aktualizace nebo datových proudů smí během tento stav.

Následující tabulka uvádí, jak kanálu státy mapování na fakturační režimu. 
 
Stav kanálu|Indikátory portálu uživatelského rozhraní|To je fakturace?
---|---|---
Spuštění|Spuštění|Žádný (přechodná kraj)
Spuštění|Jste připravení (bez spuštěné programy)<br/>nebo<br/>Streamování (alespoň jeden spuštěného programu)|Ano
Zastavení|Zastavení|Žádný (přechodná kraj)
Přerušili|Přerušili|Ne


##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



##<a name="related-topics"></a>Příbuzná témata

[Azure Media Services fragmentovaných MP4 Live jedí specifikace](media-services-fmp4-live-ingest-overview.md)

[Práce s kanály, které jsou povoleny k provedení Live kódování s Azure Media Services](media-services-manage-live-encoder-enabled-channels.md)

[Práce s kanály, které dostanete více bitrate živou toku od místního kodéry](media-services-live-streaming-with-onprem-encoders.md)

[Kvót a omezení](media-services-quotas-and-limitations.md).  

[Mediální služby koncepty](media-services-concepts.md)
