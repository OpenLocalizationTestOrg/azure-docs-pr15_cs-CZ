<properties 
    pageTitle="Přehled Azure Media Services a obvyklé scénáře | Microsoft Azure" 
    description="Toto téma obsahuje přehled Azure Media Services" 
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
    ms.topic="hero-article" 
    ms.date="10/12/2016"
    ms.author="juliako;anilmur"/>

#<a name="azure-media-services-overview-and-common-scenarios"></a>Přehled Azure Media Services a obvyklé scénáře

Microsoft Azure Media Services je extensible cloudové platformu, který umožňuje vývojářům vytvářet scalable médií řízení a doručení aplikace. Media Services vychází z REST API, které umožňují bezpečně nahrávat, ukládat, kódování a balíček videoklipu nebo zvukového obsahu na vyžádání a živou streamování doručování do různých klientů (například televizního počítače a mobilní zařízení).

Je možné vytvářet začátku do konce pracovní postupy pomocí úplně Media Services. Můžete taky použít třetích stran součásti pro některé části funkcí pracovní postup. Například kódujte pomocí encoder třetích stran. Potom nahrát, zamknout, balíček, poskytovat pomocí Media Services.

Je možné toku obsahu live nebo doručení obsahu na vyžádání. Toto téma ukazuje obvyklé scénáře předvádění vašeho obsahu [live](media-services-overview.md#live_scenarios) nebo [jako služba](media-services-overview.md#vod_scenarios). V tématu je také propojená další relevantní témata.

## <a name="sdks-and-tools"></a>Nástroje a SDK

Pokud chcete vytvářet řešení Media Services, můžete použít:

- [Mediální služby rozhraní REST API](https://msdn.microsoft.com/library/azure/hh973617.aspx)
- Jedna z dostupných klienta SDK:
- [Azure Media Services SDK pro .NET](https://github.com/Azure/azure-sdk-for-media-services)
- [Azure SDK jazyka Java](https://github.com/Azure/azure-sdk-for-java)
- [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php)
- [Azure Media Services pro Node.js](https://github.com/michelle-becker/node-ams-sdk/blob/master/lib/request.js) (Toto je jiných výrobců verzi Node.js SDK. To je spravován komunity a nyní nemá 100 % pokrytí AMS rozhraní API).
- Existující nástroje:
- [Azure portálu](https://portal.azure.com/)
- [Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (Azure Media Services Explorer (AMSE) je Winforms a C# aplikace pro Windows)

##<a name="media-services-learning-paths"></a>Media Services výukových postupů

Můžete zobrazit AMS naučné stezky tady:

- [AMS Live streamování pracovního postupu](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-live/)
- [AMS jako služba streamování pracovního postupu](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-on-demand/)

##<a name="prerequisites"></a>Zjistit předpoklady pro

Pokud chcete začít používat Azure Media Services, měli byste mít takto:

3. Účet Azure. Pokud nemáte účet, můžete vytvořit bezplatný účet zkušební v jenom pár minut. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com).
2. Účet Azure Media Services. Umožňuje Azure portálu, .NET nebo rozhraní REST API vytvořit účet Azure Media Services. Další informace najdete v tématu [Vytvoření účtu](media-services-portal-create-account.md).
3. (Volitelné) Nastavte si vývojové prostředí. Zvolte .NET rozhraní REST API pro vývojové prostředí. Další informace najdete v tématu [nastavení prostředí](media-services-dotnet-how-to-use.md).

Také informace o připojení programové [připojení](media-services-dotnet-connect-programmatically.md).
4. (Doporučeno) Přidělte jeden nebo více jednotkách. Doporučujeme přidělit jednu nebo více jednotek měřítko pro aplikace v pracovním prostředí.   Další informace najdete v tématu [Správa datových proudů koncové body](media-services-portal-manage-streaming-endpoints.md).

##<a name="concepts-and-overview"></a>Principy a základní informace

Azure Media Services koncepty najdete v článku [Koncepty](media-services-concepts.md).

Postupy řady, která vás seznámí s hlavní součásti Azure Media Services najdete v článku [Azure Media Services podrobné výukové programy pro](https://docs.com/fukushima-shigeyuki/3439/english-azure-media-services-step-by-step-series). Tato řada obsahuje skvělý přehled koncepty a používá nástroj AMSE k ukazují AMS úkoly. Všimněte si, že AMSE nástroje nástroj služby Windows. Tento nástroj podporuje většinu úkolů, které se [AMS SDK pro .NET](https://github.com/Azure/azure-sdk-for-media-services), [Azure SDK jazyka Java](https://github.com/Azure/azure-sdk-for-java)nebo [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php)dosáhnete programově.

##<a id="vod_scenarios"></a>Předvádění médií okamžitou s Azure Media Services: běžné scénářů a úkoly

Tato část popisuje obvyklé scénáře a obsahuje odkazy na příslušné témata. Na následujícím obrázku vidíte hlavní části platformy Media Services zapojené na doručování obsahu na vyžádání. 

![VoD pracovního postupu](./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png)


###<a name="protect-content-in-storage-and-deliver-streaming-media-in-the-clear-non-encrypted"></a>Ochrana obsahu v úložišti a poskytují datových proudů médií v vymazat (bez šifrování)

1. Nahrání souboru vysoce kvalitní mezzanine do aktivum.
    
    Doporučujeme použít možnost šifrovací úložiště vaší materiálů pro ochranu obsahu při nahrávání a na ostatních v úložišti.
 
1. Kódování sadu adaptivní bitrate MP4 souborů. 

    Doporučujeme použít možnost šifrování úložiště materiálů výstup pro ochranu obsahu u ostatních.
    
1. Konfigurace zásad doručení materiálů (používané dynamické balení). 
    
    Úložiště zašifrovaných při vaše materiálů **musí** konfigurace zásad doručení materiálů. 

1. Publikujte majetku vytvořením OnDemand locator.

    Je nutné mít aspoň jednu streamování rezervovaná jednotce na streamování koncový bod, ze kterého chcete proudů.

1. Toku publikovat obsah.

###<a name="protect-content-in-storage-deliver-dynamically-encrypted-streaming-media"></a>Ochrana obsahu v úložišti, doručeno dynamicky šifrované datové proudy médií  

Abyste mohli používat dynamické šifrování, musíte nejprve získat nejméně jednu jednotku streamování rezervovaná na streamování koncový bod, ze kterého chcete vysílat šifrovaného obsahu.

1. Nahrání souboru vysoce kvalitní mezzanine do aktivum. Použití možnosti šifrování úložiště majetek.
1. Kódování sadu adaptivní bitrate MP4 souborů. Použijte šifrování alternativy pro ukládání výstupu materiálů.
1. Vytvoření obsahu šifrovací klíč pro materiálů, které chcete dynamicky zašifrovat během přehrávání.
2. Konfigurace zásad obsahu klíčové ověření.
1. Konfigurace zásad doručení materiálů (používané dynamické balení a dynamické šifrování).
1. Publikujte majetku vytvořením OnDemand locator.
1. Toku publikovat obsah. 

###<a name="use-media-analytics-to-derive-actionable-insights-from-your-videos"></a>Použití analýzy médií odvodit akce přehledy z videa 

Technologie pro analýzu médií najdete součástí rozpoznávání řeči a vize, které usnadňují pro organizace a podniky odvodit akce přehledy z jejich videosoubory. Další informace najdete v tématu [Přehled Azure Media Services technologie pro analýzu](media-services-analytics-overview.md).

1. Nahrání souboru vysoce kvalitní mezzanine do aktivum.
2. Použijte jeden z těchto služeb médií analýzy zpracuje videa:
    
    - **Indexování** – [proces videa s Azure Media indexování 2](media-services-process-content-with-indexer2.md)
    - **Hyperlapse** – [Hyperlapse mediální soubory s Azure Media Hyperlapse](media-services-hyperlapse-content.md)
    - **Zjišťování pohybu** – [Pohybu rozpoznávání Azure Media analýzy](media-services-motion-detection.md).
    - **Nominální zjišťování a nominální emotikony** – [nominální a emoce rozpoznávání Azure Media analýzy](media-services-face-and-emotion-detection.md).
    - **Video souhrnu** – [Použití Azure Media Video miniatury k vytvoření souhrnu Video](media-services-video-summarization.md)
3. Média analýzy médií procesorů plodiny MP4 soubory nebo JSON. Procesor médií vyrobeno souboru MP4, který soubor postupně stáhnout. Pokud procesor médií vyrobeno souboru JSON, může soubor stáhnout z úložiště objektů blob Azure. 


###<a name="deliver-progressive-download"></a>Předvádění postupné stahování 

1. Nahrání souboru vysoce kvalitní mezzanine do aktivum.
1. Kódování do jednoho souboru MP4.
1. Publikujte majetku vytvořením OnDemand nebo přidružení zabezpečení locator.

    Pokud používáte OnDemand locator, zkontrolujte, že mít aspoň jednu streamování rezervovaná jednotce na streamování koncový bod, ze které budete postupně stáhnout obsah.

    Pokud používáte locator přidružení zabezpečení, obsah stáhnout z úložiště objektů blob Azure. V tomto případě se nemusí mít streamování rezervovaná jednotky.
  
1. Postupně stáhněte obsah.

##<a id="live_scenarios"></a>Prezentovat živá streamování událostí pomocí Azure Media Services

Při práci s obrázky Live streamování běžně jsou zahrnuty následující součásti:

- Fotoaparát, který se používá k vysílání události.
- Dynamický videa encoder, převede signály z fotoaparátu na datové proudy, které se odesílají službě živou streamování.

V případě potřeby více živou čas synchronizován kodéry. U některých důležitých live události, že služba velmi vysoké dostupnosti a kvalitu prostředí, je vhodné používat aktivní nadbytečné kodéry s časem synchronizace Chcete-li dosažení bezproblémové převzetí bez ztráty dat.
- Dynamický služby datových proudů, který umožňuje následujícím způsobem:

- jedí živou obsahu pomocí různých live protokolů datových proudů (například RTMP nebo hladký přenos)
- (volitelně) kódujte vašeho proudu do adaptivní bitrate toku
- zobrazení náhledu živou toku,
- záznam a uložení požití obsahu za účelem se streamují vyšší (poskytují)
- předvádění obsahu prostřednictvím běžné streamování protokoly (například MPEG POMLČKU hladký, HLS, konfigurace) přímo zákazníkům, nebo na doručení sítě obsahu (CDN) pro další rozdělení.


**Microsoft Azure Media Services** (AMS) umožňuje jedí kódovat, náhled, ukládání a doručení živou streamování obsahu.

Doručování obsahu pro zákazníky mohl při předvádění video ve vysoké kvalitě různá zařízení podmínkách jiné síti. Starat o kvality a sítě podmínky, umožňuje živou kodéry kódovat toku proudu videa s víc bitrate (adaptivní bitrate).  Starat o přenos na různých zařízeních umožňuje Media Services [dynamické balení](media-services-dynamic-packaging-overview.md) dynamicky znovu balíček toku na různé protokoly. Media Services podporuje odprezentování následující adaptivní přenosová streamování technologie: HTTP Live datových proudů (HLS) hladký přenos, MPEG POMLČKU a konfigurace (pro Adobe PrimeTime/přístup nabyvatelů licence pouze).

V Azure Media Services **kanály**, **programy**a **StreamingEndpoints** zpracování všech živou streamování funkcí včetně ingest, formátování, DVR zabezpečení, škálovatelnost a redundance.

**Kanál** představuje příležitost pro zpracování živou streamování obsahu. Kanálu, který může přijímat o živé vstupní datových proudů následujícími způsoby:

- V místním live encoder odešle více bitrate **RTMP** nebo **Hladký přenos** (fragmentovaných MP4) kanál, který je nakonfigurovaný na **předávací** doručení. **Předávací** doručení při požití datových proudů projít **kanálu**s bez dalšího zpracování. Můžete použít následující živou kodéry, které výstup více bitrate hladký přenos: elementární Envivio, Cisco.  Následující živou kodéry výstup RTMP: Adobe Flash Live Telestream Wirecast a Tricaster transcoders.  Živé encoder můžete poslat taky jednoho bitrate toku na kanál, který není povolený živou kódování, ale, která se nedoporučuje. Při požadavku, nabízí Media Services proudu zákazníkům.

>[AZURE.NOTE] Předávací metodou způsobem nejvhodnější live streamování při více událostí provádíte přes dlouhé časové období a jste již vložil do místního kodéry. Zobrazit podrobnosti [ceny](/pricing/details/media-services/) .

- Živé encoder místní odešle jednoduchým bitrate toku kanál, který je povoleno provádět živou kódování s Media Services v některém z následujících formátů: RTP (MPEG-TS) RTMP a přitom zajistit hladký přenos (fragmentován MP4). Kanál pak provede živou kódování příchozí jednoho bitrate toku proudu videa s víc bitrate (adaptivní). Při požadavku, nabízí Media Services proudu zákazníkům.


###<a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders-pass-through"></a>Práce s kanály, které dostanete více bitrate živou toku od místního kodéry (předávací)

Na následujícím obrázku vidíte hlavní části platformy AMS zapojené v pracovním postupu **předávací** .

![Dynamický pracovního postupu][live-overview2]

Další informace najdete v tématu [práce s kanály, které více přijímání-bitrate živou tok z místního kodéry](media-services-live-streaming-with-onprem-encoders.md).

###<a name="working-with-channels-that-are-enabled-to-perform-live-encoding-with-azure-media-services"></a>Práce s kanály, které jsou povoleny k provedení živou kódování s Azure Media Services

Na následujícím obrázku vidíte hlavní části platformy AMS zapojené v Live streamování postupu, kde je povoleno kanálu provádět živou kódování s Media Services.

![Dynamický pracovního postupu][live-overview1]

Další informace najdete v tématu [práce s kanály, které jsou povoleny k provedení Live kódování s Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).


##<a name="consuming-content"></a>Spotřebovávat obsah

Azure Media Services poskytuje nástroje, které potřebujete k vytvoření dynamických klientských přehrávače aplikací pro většinu platformy, včetně: iOS zařízení zařízení s Androidem, Windows, Windows Phone, Xbox a Set-top polí. Následující téma obsahuje odkazy na SDK a přehrávač rámce využívající vyvinout vlastní klientské aplikace, které můžete používat datových proudů médií od Media Services.

[Vývoj aplikací přehrávače videa](media-services-develop-video-players.md)

##<a name="enabling-azure-cdn"></a>Povolení Azure CDN

Media Services podporuje integrace se službou Azure CDN. Další informace o tom, jak povolit Azure CDN, najdete v tématu [Správa datových proudů koncové body v účtu služby média](media-services-portal-manage-streaming-endpoints.md).

##<a name="scaling-a-media-services-account"></a>Změna měřítka účet Media Services

**Media Services** můžete zobrazit jako určující počet **Datových proudů rezervovaná jednotky** a **Kódování rezervovaná jednotky** , kterou chcete svůj účet, abyste zřízení s.

Také je možné přizpůsobit účtu Media Services přidáním úložiště účty. Každý účet úložiště se omezí na 500 TB. Rozbalte úložišti za výchozí omezení, můžete připojit víc účtů úložiště na účet Media Services.

[Toto](media-services-portal-scale-streaming-endpoints.md) téma obsahuje odkazy na příslušné témata.

##<a name="support"></a>Podpora

[Azure podporují](https://azure.microsoft.com/support/options/) možnosti podpory pro Azure, včetně Media Services.

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="service-level-agreement-sla"></a>Smlouva o úrovni služeb (SLA)

- Pro Media Services kódování, které jsme zaručit 99,9 % dostupnost rozhraní REST API transakce.
- Pro datových proudů, doporučujeme se úspěšně žádosti o služby zaručují 99,9 % dostupnost pro stávající mediální obsah při nákupu nejméně jednu jednotku streamování.
- Pro Live kanály jsme zaručit systém kanály že externí připojení aspoň 99,9 % čas.
- Pro ochranu obsahu jsme zaručit, že jsme se úspěšně splnění klíčové požadavků aspoň 99,9 % od času.
- Pro indexování, jsme se úspěšně indexování úkolu žádosti o služby zpracování s kódování vyhrazeno 99,9 % jednotku doby.

Další informace najdete v tématu [Microsoft Azure SLA](https://azure.microsoft.com/support/legal/sla/).

<!-- Images -->
[overview]: ./media/media-services-overview/media-services-overview.png
[vod-overview]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png
[live-overview1]: ./media/media-services-live-streaming-workflow/media-services-live-streaming-new.png
[live-overview2]: ./media/media-services-live-streaming-workflow/media-services-live-streaming-current.png
 
