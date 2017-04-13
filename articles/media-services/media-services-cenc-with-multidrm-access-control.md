<properties 
    pageTitle="CENC s více DRM a řízení přístupu: Přehled návrh a implementace na Azure a Azure Media Services | Microsoft Azure" 
    description="Přečtěte si, jak na licence Microsoft® hladký přenos klienta přenos Kit." 
    services="media-services" 
    documentationCenter="" 
    authors="willzhan"  
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="willzhan;kilroyh;yanmf;juliako"/>

#<a name="cenc-with-multi-drm-and-access-control-a-reference-design-and-implementation-on-azure-and-azure-media-services"></a>CENC s více DRM a řízení přístupu: Přehled návrh a implementace na Azure a Azure Media Services

##<a name="key-words"></a>Klíčová slova
 
Azure Active Directory, Azure Media Services Azure Media Playeru, dynamické šifrování licenci doručení, PlayReady, Widevine, FairPlay, běžné Encryption(CENC), více DRM, Axinom, přerušované čáry, EME, MSE, JSON Web tokenu (JWT), deklarací, moderní prohlížeče, při přechodu myší klíč, symetrickou klíč, asymetrické klíč, OpenID propojení X509 certifikát. 

##<a name="in-this-article"></a>V tomto článku

V následujících tématech jsou uvedené v tomto článku:

- [Úvod](media-services-cenc-with-multidrm-access-control.md#introduction)
    - [Základní informace o tomto článku](media-services-cenc-with-multidrm-access-control.md#overview-of-this-article)
- [Návrh odkaz](media-services-cenc-with-multidrm-access-control.md#a-reference-design)
- [Mapování návrh technologie pro implementaci](media-services-cenc-with-multidrm-access-control.md#mapping-design-to-technology-for-implementation)
- [Implementace](media-services-cenc-with-multidrm-access-control.md#implementation)
    - [Implementace postupy](media-services-cenc-with-multidrm-access-control.md#implementation-procedures)
    - [K provedení některých problematická místa](media-services-cenc-with-multidrm-access-control.md#some-gotchas-in-implementation)
- [Další témata pro implementaci](media-services-cenc-with-multidrm-access-control.md#additional-topics-for-implementation)
    - [Nastavit informace HTTP nebo HTTPS](media-services-cenc-with-multidrm-access-control.md#http-or-https)
    - [Azure Active Directory podepisování přechodu klíče](media-services-cenc-with-multidrm-access-control.md#azure-active-directory-signing-key-rollover)
    - [Kde je tokenu přístup?](media-services-cenc-with-multidrm-access-control.md#where-is-the-access-token)
    - [Na co dávat Live streamování?](media-services-cenc-with-multidrm-access-control.md#what-about-live-streaming)
    - [Co dávat pozor licenční servery mimo Azure Media Services?](media-services-cenc-with-multidrm-access-control.md#what-about-license-servers-outside-of-azure-media-services)
    - [Co když chci používat vlastní služba tokenů zabezpečení?](media-services-cenc-with-multidrm-access-control.md#what-if-i-want-to-use-a-custom-sts)
- [Dokončené systémové a test](media-services-cenc-with-multidrm-access-control.md#the-completed-system-and-test)
    - [Přihlašovací jméno uživatele](media-services-cenc-with-multidrm-access-control.md#user-login)
    - [Použití rozšíření šifrované médií pro PlayReady](media-services-cenc-with-multidrm-access-control.md#using-encrypted-media-extensipons-for-playready)
    - [Použití EME pro Widevine](media-services-cenc-with-multidrm-access-control.md#using-eme-for-widevine)
    - [Není oprávnění uživatelům](media-services-cenc-with-multidrm-access-control.md#not-entitled-users)
    - [Spuštění vlastní tokenu služba zabezpečené](media-services-cenc-with-multidrm-access-control.md#running-custom-secure-token-service)
- [Souhrn](media-services-cenc-with-multidrm-access-control.md#summary)

##<a name="introduction"></a>Úvod

Je známý se složitou úlohu navrhování a vytváření podsystému DRM pro OTT nebo online streamování řešení. A je obvyklé operátory/online videa poskytovatelé externí tuto část specializované DRM poskytovatele služeb. Cílem tento dokument je prezentovat odkaz návrh a implementace Podsystém DRM začátku do konce OTT nebo online streamování řešení.

Cílových čteček tohoto dokumentu jsou inženýrů práci v Podsystém DRM OTT datových proudů/více-screen řešení online nebo libovolnou čteček zájem o Podsystém DRM. Za předpokladu je znát alespoň jedno z technologie DRM na trh, například PlayReady, Widevine, FairPlay nebo Adobe přístup čtenáři.

Podle DRM abychom se týkají taky CENC (běžné šifrování) s více DRM. Hlavní trend v online streamování a OTT odvětví je pomocí služby CENC více-native-DRM na různé platformy klienta, což je shift z předchozí trend pomocí jednoho DRM a svému klientovi SDK pro různé platformy klienta. Pokud chcete použít CENC s více native DRM, PlayReady a Widevine jsou šifrovány za specifikaci [Běžné šifrování (CENC ISO/IEC 23001-7)](http://www.iso.org/iso/home/store/catalogue_ics/catalogue_detail_ics.htm?csnumber=65271/) .

Výhody CENC s více DRM jsou následujícím způsobem:

1. Snižuje šifrování nákladů od zpracování jednoho šifrování se používá zacílení různé platformy s jeho nativní DRMs;
1. Snižuje náklady správy šifrované prostředky, protože je potřeba pouze jednu kopii šifrované prostředky;
1. Eliminuje DRM licencování náklady native client – DRM je obvykle volného místa na nativní platformy.

Microsoft byl aktivní promotorem přerušované čáry a CENC společně s některé hlavní průmyslové přehrávače. Microsoft Azure Media Services má poskytující podporu přerušované čáry a CENC. Poslední oznámení, najdete v článku na Mingfei blogy: [Widevine oznámení dostupnosti aplikace Google licenci doručení služby Azure Media Services](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/)a [Azure Media Services přidá Google Widevine balení pro doručování s víc DRM proudu](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/).  

### <a name="overview-of-this-article"></a>Základní informace o tomto článku

Cíl tento článek obsahuje následující:

1. Poskytuje odkazy návrhu Podsystém DRM CENC pomocí více DRM;
1. Poskytuje implementaci odkaz na Microsoft Azure/Azure Media Services platformě;
1. Tento článek popisuje pár témat návrh a implementace.

V následujícím článku "více DRM" obsahuje následující:

1. Microsoft PlayReady
1. Google Widevine
1. Apple FairPlay (není podporován Azure Media Services)

Následující tabulka shrnuje nativní platformy/nativní aplikaci a prohlížeče podporované kodérem každý DRM.

**Klient platformy**|**Nativní DRM podpory**|**Prohlížeče a aplikací**|**Streamování formáty**
----|------|----|----
**Inteligentní televizory operátor externích zařízení, externích OTT zařízení**|PlayReady především a/nebo Widevine nebo jiné|Linux, Opera, WebKit, jiné|Různé formáty
**Zařízení Windows 10 (Windows PC, tabletů s Windows, Windows Phone, Xbox)**|PlayReady|MS okraj/IE11/EME<br/><br/><br/>UWP|Přerušované čáry (pro HLS, PlayReady nepodporuje)<br/><br/>Přerušované čáry, hladký přenos (pro HLS, PlayReady nepodporuje) 
**Zařízení s androidem (telefon, Tablet, Televizi)**|Widevine|Chrome/EME|PŘERUŠOVANÉ ČÁRY
**iOS (iPhone, iPad), klienty OS X a Televizi Apple**|FairPlay|Safari 8 +/ EME|HLS
**Modul plug-in: Adobe Primetime**|Primetime přístup|Modul plug-in prohlížeče|KONFIGURACE, HLS

Vzhledem k tomu aktuální stav nasazení pro každou DRM službu obvykle moci provádět 2 nebo 3 DRMs, abyste měli jistotu, že adresa všechny typy koncové body v nejlepší způsob, jak.

Existuje vytvářenou složitost logiky služby a složitosti na straně klienta k dosažení úrovně uživatelské prostředí v různých klientech.

Chcete-li výběr, mějte na paměti následující skutečnosti:

- PlayReady nativně implementovaná v každé zařízení ve Windows, v některých zařízeních se systémem Android a k dispozici prostřednictvím software SDK platformy téměř žádné
- Widevine je nativně implementovaná v každé zařízení s Androidem, Chrome a jiných zařízeních
- FairPlay je k dispozici pouze v iOS a Mac OS klienti nebo prostřednictvím iTunes.

Typické DRM více by proto:

- Možnost 1: PlayReady a Widevine
- Možnost 2: PlayReady Widevine a FairPlay


## <a name="a-reference-design"></a>Návrh odkaz

V této části jsme prezentovat která technologie použit k jejímu vzoru odkaz.

Podsystému DRM může obsahovat tyto prvky:

1. Správy klíčů
1. Balení DRM
1. DRM licenci doručení
1. Kontrola oprávnění
1. Ověřování a ověření
1. Přehrávač
1. Origin/CDN

Následující diagram ukazuje nejvyšší úrovně interakce mezi součástí podsystému DRM.

![Podsystém DRM s CENC](./media/media-services-cenc-with-multidrm-access-control/media-services-generic-drm-subsystem-with-cenc.png)
 
Existují tři základní "vrstvy" v návrhu:

1. Pozadí office vrstvy (na černém pozadí), které nejsou přístupné externě;
1. Vrstvy "DMZ" (modrý) obsahující všechny koncové body protilehlé veřejné;
1. Veřejné Internet vrstvy (světle modrý) obsahující CDN a přehrávače přenosy veřejné Internetu.
 
By měl být nástroj správy obsahu, k nimž ochranu DRM, bez ohledu na to statické nebo dynamické šifrování. By měl obsahovat vstupů DRM šifrování:

1. Hlavní spouštěcí záznam obsahu videa;
1. Klíč obsahu;
1. Získání adresy URL licenci.

Během přehrávání období nejvyšší úrovně tok je:

1. Ověření;
1. Povolení token se vytvoří pro uživatele.
1. Obsah DRM chráněné (manifestu) se stahuje do přehrávače;
1. Přehrávač odešle požadavek pořízení licence k serverům licencí společně s klíčové ID a povolení tokenu.

Přejde na další téma krátké o návrhu správy klíčů.

**ContentKey – materiálů**|**Scénář**
------|---------------------------
1 – 1|Nejjednodušší písmena. Poskytuje nejlepší kontrolu. Ale obecně výsledkem nejvyšší náklady doručení licenci. Na minimální licenci je potřebný pro každý chráněné majetek žádost o.
1 –: N|Můžete použít stejného klíče obsahu pro více prostředky. Například pro všechny prostředky ve skupině logické například žánr nebo podmnožinu žánr (nebo genu film), můžete použít jednu klíč obsahu.
N – 1|Více obsahu klíčů jsou potřebné pro každou materiálů. <br/><br/>Například je nutné použít dynamické CENC protection více DRM pro MPEG-POMLČKU a dynamické šifrování AES 128 pro HLS, potřebnosti dva různé obsahu klíče, oba objekty mají vlastní ContentKeyType. (Klíč obsahu pro dynamické ochranu CENC ContentKeyType.CommonEncryption bude použit pro klávesu obsahu pro dynamické šifrování AES 128, bude použito ContentKeyType.EnvelopeEncryption.)<br/><br/>Jiný příklad CENC ochranu obsahu POMLČKU teoreticky jiný, který klíč obsahu mohou sloužit k ochraně stream videa a jiného obsahu klíč chránit zvuk. 
N – - řada|Kombinace výše dva scénáře: jednu sadu obsahu zkratky se používají pro každý více majetek ve stejném materiálů "skupina".


Dalším faktorem důležité zvážit je použití trvalý a dočasnou licence.

Proč jsou tyto další skutečnosti důležité? 

Pokud používáte veřejný cloudu pro doručování licenci mají přímý dopad licenci doručení nákladů. Zvažte následující dva různé návrh případy ke znázornění:



1. Měsíční předplatné: použití trvalý licence a 1 typu obsahu klíč materiálů mapování. Například pro všechny děti filmy používáme jediný obsahu klíč pro šifrování. V tomto případě: 

    Celkový počet licencí # požadované pro všechny děti filmy/zařízení = 1

1. Měsíční předplatné: použití dočasnou licence a 1 a 1 mapování mezi obsahu spolu s materiálů. V tomto případě:

    Celkový počet licencí # požadované pro všechny děti filmy/zařízení = [# filmy sledované] x [# relace]

Jak snadno můžete vidět, dva různé designy za následek odlišnými licenci žádost o tedy vzorků licenci doručení nákladů Pokud služba pro doručování licenci poskytuje společnost veřejné obláčkem například Azure Media Services.

## <a name="mapping-design-to-technology-for-implementation"></a>Mapování návrh technologie pro implementaci

Dále jsme namapujte naše Obecný návrh technologie platformy Microsoft Azure/Azure Media Services, zadáním technologii, která má použít pro každou stavební blok.

V následující tabulce jsou uvedeny mapování:

**Stavební blok**|**Technologie**
------|-------
**Přehrávač**|[Azure Media Player](https://azure.microsoft.com/services/media-services/media-player/)
**Zprostředkovatele identit (IDP)**|Azure Active Directory
**Služba tokenů zabezpečení (STS)**|Azure Active Directory
**Ochrana DRM pracovního postupu**|Mediální služby Azure dynamické ochrany
**DRM licenci doručení**|1. mediální služby azure licenci doručení (PlayReady Widevine, FairPlay), nebo <br/>2. Server licence Axinom <br/>3. vlastní PlayReady licenční Server
**Origin**|Mediální služby Azure streamování koncový bod
**Správy klíčů**|Není potřebné pro implementaci odkaz
**Správa obsahu**|Aplikace konzoly C#

Jinými slovy zprostředkovatele identit (IDP) a zabezpečené tokenu Service (Služba tokenů zabezpečení) bude Azure AD. Pro přehrávání použijeme [Rozhraní API Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/). Azure Media Services a Azure Media Player podporují přerušované čáry a CENC s více DRM.

Na následujícím obrázku vidíte celkové struktuře a toku výše technologii mapování.

![CENC na AMS](./media/media-services-cenc-with-multidrm-access-control/media-services-cenc-subsystem-on-AMS-platform.png)

K nastavení dynamické CENC šifrování, bude používat nástroj pro správu obsahu následující vstupů:

1. Otevření obsahu;
1. Klíč obsahu z klíče generování/správy;
1. Získání adresy URL licence;
1. Přehled informací z Azure AD.

Výstup nástroje pro správu obsahu bude:

1. ContentKeyAuthorizationPolicy obsahující specifikace jak licenci doručení ověří JWT token a DRM licenci specifikace;
1. AssetDeliveryPolicy obsahující specifikace na datových proudů formát, Ochrana DRM a licence získání adresy URL.

Za běhu, řízení toku je jako níže:

1. Po ověření uživatele je generováno JWT token;
1. Jednou z deklarací obsažené v tokenu JWT je "skupiny" deklarace obsahující Identifikátor objektu skupiny "EntitledUserGroup". Toto tvrzení se použije pro předávání "nároku zaškrtnutí".
1. Přehrávač stahování klienta manifestu CENC chráněný obsah a "uvidí" toto:
    1. ID, klíče 
    1. obsah je CENC zamknutý,
    1. Získání adresy URL licenci.

1. Přehrávač žádá licence pořízení podle podporované prohlížeče/DRM. V pozvánce na schůzku licenci pořízení, klíče ID a JWT token bude taky odeslat. Služba pro doručování licenci ověří JWT token a deklarací obsažené před vydáním potřebné licence.

##<a name="implementation"></a>Implementace

###<a name="implementation-procedures"></a>Implementace postupy

Provedení bude obsahovat podle těchto kroků:

1. Příprava majetek test: kódovat/balíčku test videa ve službě více bitrate fragmentován MP4 v Azure Media Services. Tento majetek není DRM chráněné. Ochrana DRM provede ochranou dynamické později.
1. Vytvářet klíčové ID a obsah key (Pokud chcete z klíčové počáteční hodnota). Pro naše účel není potřeba systému správy klíčů, protože jsme se zabývají pouze jednu sadu klíčové ID a klíč obsahu z několika test aktiv;
1. Rozhraní API AMS používá ke konfiguraci více DRM licenci doručení služby pro test materiálů. Pokud používáte vlastní licenční servery tak, že vaše firma nebo Dodavatelé vaší společnosti místo licenci služby Azure Media Services, můžete tento krok přeskočit a zadat adresy URL pořízení licencí v seznamu krok pro konfiguraci licenci doručení. Rozhraní API AMS je potřeba k určení vyžadovaného že některé podrobné konfigurace, například omezení zásad se tak mohli ověřovat šablony odpověď licence pro různé služby DRM licence, atd. V současné době portálu Azure zatím nenabízí potřebné uživatelské rozhraní pro tuto konfiguraci. Najděte úrovně informací rozhraní API a přehrajte si doručení s kódem v dokumentu Julie Kornich: [pomocí PlayReady a/nebo Widevine dynamické běžné šifrování](media-services-protect-with-drm.md). 
1. Použití rozhraní API AMS konfigurace zásad doručení materiály pro test materiálů. Najděte úrovně informací rozhraní API a přehrajte si doručení s kódem v dokumentu Julie Kornich: [pomocí PlayReady a/nebo Widevine dynamické běžné šifrování](media-services-protect-with-drm.md).
1. Vytvoření a konfigurace klienta služby Azure Active Directory v Azure;
1. Vytvoření několik uživatelské účty a skupiny ve vašem klientovi Azure Active Directory: je třeba vytvořit aspoň "EntitledUser" seskupovat a přidání uživatele do této skupiny. Uživatelé v této skupině předá oprávnění zaškrtněte v získání licence a uživatelé není v této skupině se nepovede k předání kontroly ověřování a nebudou moct získat licence. Byli členy této skupiny "EntitledUser" je deklaraci požadované "skupiny" JWT tokenu vydán Azure AD. Tohoto požadavku na deklaraci identity by měl být zadán v konfiguraci více DRM licenci doručení služby kroku.
1. Vytvoření aplikace pro ASP.NET MVC, který bude hostitelem přehrávače videa. Tato aplikace ASP.NET bude chránit pomocí ověřování uživatelů proti klienta služby Azure Active Directory. VELKÁ2 deklarací bude obsahovat tokeny přístupu získané po ověření uživatele. Rozhraní API připojení OpenID je vhodné pro tento krok. Budete potřebovat k instalaci následující balíčky NuGet:
    - Instalační balíček Microsoft.Azure.ActiveDirectory.GraphClient
    - Instalační balíček Microsoft.Owin.Security.OpenIdConnect
    - Instalační balíček Microsoft.Owin.Security.Cookies
    - Instalační balíček Microsoft.Owin.Host.SystemWeb
    - Instalační balíček Microsoft.IdentityModel.Clients.ActiveDirectory
1. Vytvoření přehrávače pomocí [Rozhraní API Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/). [Rozhraní API ProtectionInfo Azure Media Playeru](http://amp.azure.net/libs/amp/latest/docs/) vám umožní určit technologii DRM, která má použít na jiném platformě DRM.
1. Test obrázku:

**DRM**|**Prohlížeče**|**Výsledek oprávnění uživatele**|**Výsledek pro zrušení oprávnění uživatele**
---|---|-----|---------
**PlayReady**|MS okraje nebo IE11 ve Windows 10|Úspěšné|Selhalo:
**Widevine**|Chrome ve Windows 10|Úspěšné|Selhalo:
**FairPlay** |TBD||

Jirka Trifonov Azure Media Services týmu napsal blogu poskytuje podrobné pokyny v nastavení služby Azure Active Directory pro aplikace přehrávače ASP.NET MVC: [Integrace Azure Media Services OWIN MVC na základě aplikace pomocí služby Azure Active Directory a omezení doručování obsahu klíčové založené na deklaraci JWT](http://gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

Jirka taky napsal blogu na [JWT tokenu ověřování v Azure Media Services a dynamické šifrování](http://gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/). A tady jsou jeho [vzorku na Azure AD integrace se službou Azure Media Services klíčové doručení](https://github.com/AzureMediaServicesSamples/Key-delivery-with-AAD-integration/).

Další informace o Azure Active Directory:

- Informace pro vývojáře najdete v [příručce Azure Active Directory pro vývojáře](../active-directory/active-directory-developers-guide.md).
- Informace pro správce můžete najít v [Spravovat svůj Azure AD Directory](../active-directory/active-directory-administer.md).

### <a name="some-gotchas-in-implementation"></a>K provedení některých problematická místa

Při provádění existují některé "problematická místa". Zpravidla tento seznam "problematická místa" vám pomůže Poradce při potížích v případě, že narazíte na problémy.

1. **Vydavatel** Adresa URL by měl končit **"/"**.  

    **Cílové skupiny** je třeba ID klienta aplikace přehrávače a měli byste taky přidat **"/"** na konci adresy URL vydavatel.

        <add key="ida:audience" value="[Application Client ID GUID]" />
        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/" /> 

    V [JWT dekódovací](http://jwt.calebb.net/)byste měli vidět **oblast** a **iss** jako pod tokenu JWT:
    
    ![1st gotcha](./media/media-services-cenc-with-multidrm-access-control/media-services-1st-gotcha.png)


2. Přidáte oprávnění k aplikaci AAD (karta konfigurovat aplikace). Toto je nutný pro jednotlivé aplikace (místní a nasazené verze).

    ![gotcha 2.](./media/media-services-cenc-with-multidrm-access-control/media-services-perms-to-other-apps.png)


3. Pomocí správné vydavatele při nastavování dynamické ochranu CENC:

        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/"/>
    
    Nebudou fungovat takto:
    
        <add key="ida:issuer" value="https://willzhanad.onmicrosoft.com/" />
    
    Identifikátor GUID je ID AAD klienta. Identifikátor GUID najdete v místním koncové body Azure portálu.

4. Deklarace členství ve skupinách udělit oprávnění. Ujistěte se, v seznamu souboru AAD aplikace, máme takto

    "groupMembershipClaims": "Všechny" (výchozí hodnota je null)

5. Nastavení správného TokenType při vytváření omezení požadavky.

        objTokenRestrictionTemplate.TokenType = TokenType.JWT;

    Protože přidání podpory JWT (AAD) kromě SWT ACS (), je výchozí TokenType TokenType.JWT. Pokud používáte SWT/ACS, musíte nastavit TokenType.SWT.

## <a name="additional-topics-for-implementation"></a>Další témata pro implementaci
Další jsme bude disku uss některé další témata v naší návrh a implementace.

###<a name="http-or-https"></a>Nastavit informace HTTP nebo HTTPS?

Aplikaci přehrávače ASP.NET MVC, kterou jsme vytvořili musí podporují těchto věcí:

1. Ověřování uživatelů prostřednictvím Azure AD, které musí být v části HTTPS;
1. JWT tokenu exchange mezi klientem a Azure AD, které musí být v části HTTPS;
1. Získání licence DRM klientem, které musí být v části HTTPS, pokud licence doručení službou Azure Media Services. Samozřejmě sadu produktů PlayReady není pověřit HTTPS pro doručování licenci. Pokud licenční server PlayReady překročí Azure Media Services, můžete použít HTTP nebo HTTPS.

Proto použije aplikace přehrávače ASP.NET HTTPS osvědčený. To znamená, že Azure Media Playeru bude na stránce v části HTTPS. U datových proudů však jsme raději HTTP, tedy je třeba vzít v úvahu smíšený obsah problém.

1. Prohlížeč nepovoluje smíšený obsah. Umožňoval moduly plug-in jako Silverlight a OSMF modul plug-in hladkých a přerušované čáry. Smíšený obsah je problém zabezpečení: Toto je kvůli hrozbou, že možnost vloží škodlivým JS, což může způsobit data o zákaznících na rizika.  Prohlížeče blokovat tuto ve výchozím nastavení a zatím jedině informace k alternativním řešením je zapnuté umožňuje všechny domény (bez ohledu na to https nebo http) na straně serveru (původ). Toto není pravděpodobně vhodné buď.
1. Jsme neměli smíšený obsah: používají HTTP nebo používají HTTPS. Při přehrávání smíšený obsah, silverlightSS Odborný vyžaduje vymazání smíšený obsah upozornění. flashSS Odborný zpracovává smíšený obsah bez smíšený obsah upozornění.
1. Pokud streamování koncový bod byla vytvořená před srpen 2014, nebude podporovat HTTPS. V tomto případě vytvořte a použít nový streamování koncový bod pro HTTPS.

K provedení odkaz pro obsah DRM chráněné aplikací a přenos bude na HTTTPS. Pro otevřít obsah přehrávač nemusí ověřování nebo licenci, tak, abyste měli Svoboda použít HTTP nebo HTTPS.

### <a name="azure-active-directory-signing-key-rollover"></a>Azure Active Directory podepisování přechodu klíče

To je důležitá vzít v úvahu implementace. Pokud není považujete za to ve vaší implementací, dokončené systému postupně přestane fungovat úplně do maximálně 6 týdnů.

Azure AD pomocí standardní vytvořit vztah důvěryhodnosti mezi sebe sama a aplikací pomocí Azure AD. Konkrétně Azure AD pomocí podpisový klíč, který se skládá z veřejných a privátních klíčů pár. Když Azure AD vytvoří tokenu zabezpečení, který obsahuje informace o uživateli, je tento token podepsáno Azure AD pomocí privátním klíčem před odesláním zpátky do aplikace. Pokud chcete ověřit, že tokenu platnou a skutečně původu z Azure AD, třeba aplikace ověřit podpis tokenu pomocí veřejným klíčem zveřejněné příslušným Azure AD obsažené v dokument metadat federace klienta. Veřejný klíč – a podpisový klíč, ze kterého je odvozena – je stejný jako ten použít pro všechny klienty v Azure AD.

Podrobné informace o přechodu klíče Azure AD najdete v dokumentu: [Důležité informace o přihlašování klíče při přechodu myší v Azure AD](../active-directory/active-directory-signing-key-rollover.md).

Mezi [pár veřejný privátní klíč](https://login.windows.net/common/discovery/keys/) 

- Soukromý klíč používá Azure Active Directory pro generování JWT token;
- Veřejný klíč aplikací například DRM licenci doručení služby v AMS slouží k ověření tokenu JWT;
 
Za účelem zabezpečení: otočí Azure Active Directory certifikát pravidelně (každých 6 týdnů). V případě narušením zabezpečení může dojít přechodu klíče kdykoli. Proto službám doručení licenci AMS nutné veřejným klíčem procentové Azure AD otočí pár klíčů, jinak tokenu ověření v AMS se nezdaří, a vytvoří webová žádná licence. 

Dosahuje se nastavením TokenRestrictionTemplate.OpenIdConnectDiscoveryDocument při konfiguraci služby doručení DRM licenci.

Tok tokenu JWT je jako níže:

1.  Azure AD vydá JWT token s aktuální privátním klíčem pro ověřeného uživatele.
2.  Když přehrávač uvidí CENC s obsahem více DRM chráněné, vyhledá nejdřív JWT token vydané Azure AD.
3.  Přehrávač odešle žádost pořízení licenci tokenu JWT licenci doručení služby v AMS;
4.  Služby doručení licence v AMS použije aktuální/platné veřejný klíč z Azure AD pro ověření tokenu JWT před vydáním licence.

DRM licenčních doručení služeb bude vždy vyhledávat aktuální/platné veřejný klíč z Azure AD. Veřejný klíč předložený Azure AD budou klíče slouží k ověření token JWT vydán Azure AD.

Co když přechodu klíče se stane po AAD generuje JWT token, ale před JWT token odešle přehrávače DRM licence doručení služby v rámci AMS pro ověření? 

Protože klíč může být vrácena okamžiku, vždycky je víc platné veřejným klíčem k dispozici v dokumentu metadat federace. Azure Media Services licenci doručení lze pomocí libovolné zkratky uvedené v dokumentu, protože vždycky jen jednu klávesu může být vrácena brzy bude k dispozici, jiné může jeho náhrada atd.

### <a name="where-is-the-access-token"></a>Kde je tokenu přístup?

Když se podíváte na tom, jak do webových aplikací volání aplikace rozhraní API identitou [aplikace s OAuth 2.0 klienta pověření grant (udělit)](active-directory-authentication-scenarios.md#web-application-to-web-api), jako je tok ověřování níže:

1.  Je uživatel přihlášený Azure AD ve webové aplikaci (viz [Webového prohlížeče webové aplikace](active-directory-authentication-scenarios.md#web-browser-to-web-application).
2.  Koncový bod se tak mohli ověřovat Azure AD přesměruje uživatelského agenta zpátky ke klientské aplikaci s kódem se tak mohli ověřovat. Uživatelského agenta vrátí kód se tak mohli ověřovat klientské aplikaci přesměrování URI.
3.  Webová aplikace musí získat přístupový token tak, aby ho ověřování webového rozhraní API a načíst požadovaná zdroje. Vytvoří žádost Azure AD tokenu koncový bod, poskytnutí pověření, ID klienta a webového rozhraní API aplikace Identifikátor URI. Autorizační kód, aby bylo prokázáno, že uživatel souhlasí prezentuje.
4.  Azure AD ověří aplikace a vrátí přístupový token JWT, který slouží k volání webového rozhraní API.
5.  Přes protokol HTTPS webové aplikace používá vrácené přístupový token JWT přidáte JWT řetězec s označením "Nosný" v záhlaví se tak mohli ověřovat žádosti o webového rozhraní API. Webového rozhraní API potom ověří JWT token a pokud bude ověření úspěšné, vrátí funkce požadované zdroje.

V tomto toku "identitu" webového rozhraní API považuje za důvěryhodnou webové aplikace ověření uživatele. Z tohoto důvodu tento způsob je místo toho možnost podsystému důvěryhodné. [Diagram na této stránce](http://msdn.microsoft.com/library/azure/dn645542.aspx/) popisuje, jak se tak mohli ověřovat kód udělit toku funguje.

V získání licence pomocí tokenu omezení jsme sledují stejné důvěryhodných podsystém vzorku. A služba pro doručování licenci v Azure Media Services rozhraní API zdroje, "back-end zdroji" webovou aplikaci potřebuje přístup k webu. Kde je vlastně přístupový token?

Nakonec jsme jsou získání přístupový token z Azure AD. Po ověření úspěšné uživatele je vrácena kód ověření. Kód se tak mohli ověřovat je pak použít, spolu s ID a aplikace klíč klienta, na exchange pro přístupový token. A přístupový token pro přístup k aplikaci "ukazatel" ukazujícím nebo představující služba pro doručování licenci Azure Media Services.

Potřebujeme k registraci a konfiguraci aplikace "ukazatel" v Azure AD pomocí následujících kroků:

1.  V klientovi Azure AD

    - Přidání aplikace (zdroje) s přihlašování URL: 

    https://[resource_name].azurewebsites.NET/ a 

    - Adresa URL ID aplikace: 
    
    https://[aad_tenant_name].onmicrosoft.com/[resource_name]; 
2.  Přidejte nový klíč pro aplikaci zdroje;
3.  Aktualizace aplikace soubor tak, že vlastnost groupMembershipClaims má následující hodnotu: "groupMembershipClaims": "Všechny"  
4.  V aplikaci Azure AD přejdete na web appu Playeru v části "oprávnění pro ostatní aplikace", přidáte aplikaci zdroje, přidaný v kroku 1. V části "oprávněními delegovaného" zaškrtněte políčko "Access [resource_name]". To vám oprávnění web app pro vytvoření tokenů Accessu pro přístup k aplikaci zdroje. Je vhodné provést pro místní a nasazené verze aplikace pro web případě vyvíjíte Visual Studia a Azure webovou aplikaci.
    
JWT token vydán Azure AD tedy skutečně přístupový token pro přístup k tomuto prostředku "ukazatel".

### <a name="what-about-live-streaming"></a>Na co dávat Live streamování?

Ve výše uvedené má byla naše diskuse zaměřené na okamžitou prostředky. Na co dávat live streamování?

Dobrá zpráva je, že můžete přesně stejné návrh a implementace chránících živou datových proudů v Azure Media Services úpravou materiálů přidružených k programu jako "VOD majetek".

Konkrétně je známý, že dělat živou streamování v Azure Media Services, je potřeba vytvořit kanál a potom program v části kanálu. Pokud chcete vytvořit program, je potřeba vytvořit materiálů, které bude obsahovat živou archivace k programu. Abyste mohli poskytnout CENC ochrana vícenásobný DRM živou obsahu, vše, co jste potřeba udělat, je použít stejné nastavení/zpracování majetku jako by byl "VOD materiálů" před spuštěním programu.

### <a name="what-about-license-servers-outside-of-azure-media-services"></a>Co dávat pozor licenční servery mimo Azure Media Services?

Často může mít v licenci serverové farmy buď v svá data zarovnat na střed nebo hostované poskytovateli služeb DRM investovat zákazníci. Naštěstí Azure Media Services ochranu obsahu umožňuje pracovat v hybridním režimu: obsah hostované a dynamické chráněné v Azure Media Services, zatímco DRM licence doručované servery mimo Azure Media Services. V tomto případě v úvahu následující změny:

1. Služba tokenů zabezpečení musí o vystavení tokenů, které jsou přijatelné a lze ověřit licenci serverové farmy. Například licenční servery Widevine poskytovanou Axinom vyžaduje konkrétní token JWT, který obsahuje "nároku zprávu". Proto musíte mít STS o vystavení tokenu takové JWT. Autoři dokončili takové implementaci a podrobnosti najdete v následující dokument uprostřed [Azure si přečtěte následující dokumentaci](https://azure.microsoft.com/documentation/): [Pomocí Axinom předvádění Widevine licence Azure Media Services](media-services-axinom-integration.md). 
1. Již nepotřebujete službu licenci doručení (ContentKeyAuthorizationPolicy) v Azure Media Services. Co je potřeba udělat, je poskytovat licence získání adresy URL (PlayReady Widevine a FairPlay) při konfiguraci AssetDeliveryPolicy při nastavování CENC s více DRM.
 
### <a name="what-if-i-want-to-use-a-custom-sts"></a>Co když chci používat vlastní služba tokenů zabezpečení?

Může to být z několika důvodů, které může používat vlastní STS (Secure Služba tokenů) k poskytování JWT tokeny zvolit zákazníka. Některé z nich jsou:

1.  Poskytovatele Identity (IDP), které jsou používá zákazníkem nepodporuje Služba tokenů zabezpečení. V tomto případě vlastní služba tokenů zabezpečení může být jednu z možností.
2.  Zákazník může být nutné flexibility nebo užší ovládací prvek v integrace STS s zákazníka odběratele fakturace systému. Operátor MVPD může například nabízejí více OTT odběratele balíčků například premium, základní sportovní, atd. Operátor může má posuzovat shoda nároky v token s účastníka balíčku tak, že je k dispozici pouze obsah v pravém balíčku. V tomto případě vlastní STS nabízí potřebné flexibilitu ovládacího prvku.

Dvě změny třeba provést při použití vlastního Služba tokenů zabezpečení:

1.  Při konfiguraci služba pro doručování licenci pro aktivum, budete muset zadat klíč zabezpečení sloužit k ověření vlastní STS (podrobnosti níže) místo aktuálního klíče ze služby Azure Active Directory.
2.  Při generování JTW tokenu zabezpečení klíč není zadán místo privátním klíčem aktuální X509 certifikát v Azure Active Directory.

Existují dva typy klíčů zabezpečení:

1.  Symetrickou klíč: stejné používá se pro generování i ověření JWT token;
2.  Asymetrické klíč: pár veřejný privátní klíč v X509 certifikát se používá s privátním klíčem pro šifrování/generování JWT token a veřejný klíč pro ověření tokenu.

####<a name="tech-note"></a>Poznámka pro technické

Pokud používáte .NET Framework a C# jako vývojové platformy, X509 certifikát používaný pro klíč asymetrické zabezpečení může mít délku nejméně 2048 klíče. Toto je povinné třídy System.IdentityModel.Tokens.X509AsymmetricSecurityKey .NET Framework. V opačném následující výjimce:

IDX10630: "System.IdentityModel.Tokens.X509AsymmetricSecurityKey" pro podepisování nesmí být menší než bitů "2048". 

## <a name="the-completed-system-and-test"></a>Dokončené systémové a test

Bude projdeme několik scénáře v systému dokončené začátku do konce tak, aby čtenáři můžou basic "vzhledu" chování než se pustíte přihlašovací účet.

Přehrávač webové aplikace a její přihlašovací najdete [tady](https://openidconnectweb.azurewebsites.net/).

Pokud budete potřebovat "zjistili jste" scénář: videa prostředky hostované v Azure Media Services, který se nebude buď chráněn nebo chráněné DRM, ale bez tokenu ověřování (vystavení licence pro ten, kdo ho žádosti o), ji můžete otestovat bez přihlášení (přepnutím do HTTP-li vaše streamování videa přes HTTP).

Pokud budete potřebovat je integrovaný scénář začátku do konce: videa prostředky je chráněné dynamické DRM v Azure Media Services s tokenu ověřování a JWT token použitým Azure AD, budete potřebovat přihlásit.

### <a name="user-login"></a>Přihlašovací jméno uživatele

K testování systému integrované DRM začátku do konce, musíte mít "účet" vytvořené nebo přidat. 

Jaký účet?

Přestože Azure původně povolených přístup jenom uživatelé účtu Microsoft, teď umožňuje přístup uživatelů z obou. To platné tím, že všechny Azure vlastnosti důvěřovat Azure AD pro ověřování s Azure AD ověřování organizační uživatelů a vytvořením relace federace kde Azure AD důvěryhodnosti systému Microsoft účtu spotř identity pro ověřování uživatelů příjemce. Výsledkem je Azure AD ověření účtů Microsoft "hosty" i "nativní" účty Azure AD.

Protože Azure AD vztahy důvěryhodnosti domény účtu Microsoft (MSA), můžete přidat jakékoli účty z kterékoli z těchto domén vlastní Azure AD tenant a používat účet pro přihlášení:

**Název domény**|**Domény**
-----------|----------
**Vlastní Azure AD klienta domény**|somename.onmicrosoft.com
**Domény společnosti**|Microsoft.com
**Účet Microsoft (MSA) domény**|Outlook.com, live.com, hotmail.com

Autorům máte nastavený účet vytvořené nebo přidat za vás můžou kontaktovat. 

V následující tabulce jsou snímky obrazovek jiné přihlašovací stránky používané účty jinou doménu.

**Vlastní Azure AD klienta doménovým účtem**: V tomto případě se zobrazí stránka přihlašovacích přizpůsobenou vlastní domain Azure AD klienta.

![Vlastní Azure AD klienta doménovým účtem](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain1.png)

**Doménovým účtem Microsoft s čipovou kartou**: V tomto případě se zobrazí na přihlašovací stránce Přizpůsobit Microsoft podnikové IT s dvojúrovňové ověřování.

![Vlastní Azure AD klienta doménovým účtem](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain2.png)

**Účet Microsoft (MSA)**: V tomto případě se zobrazí na přihlašovací stránku Account Microsoft pro uživatele.

![Vlastní Azure AD klienta doménovým účtem](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain3.png)

### <a name="using-encrypted-media-extensions-for-playready"></a>Použití rozšíření šifrované médií pro PlayReady

Moderní prohlížeče s zašifrovaných rozšíření médií (EME) PlayReady podporu, například aplikaci Internet Explorer 11 ve Windows 8.1 a nahoru a prohlížeče Microsoft Edge na Windows 10 budou PlayReady podkladového DRM EME.

![Použití EME pro PlayReady](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready1.png)

Oblast tmavě přehrávače je tomu, že PlayReady chrání jednu provádět snímku obrazovky chráněné videa. 

Následující obrazovka ukazuje přehrávače moduly plug-in a MSE/EME podporu.

![Použití EME pro PlayReady](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready2.png)

EME v Microsoft Edge a aplikace Internet Explorer 11 na Windows 10 umožňuje vyvolání z [PlayReady SL3000](https://www.microsoft.com/playready/features/EnhancedContentProtection.aspx/) na zařízeních s Windows 10, které podporují. PlayReady SL3000 odemknou tok rozšířené premium obsahu (4K, záhlaví, atd.) a nové modely doručování obsahu (nejdříve možné okno pro rozšířený obsah).

Zaměření na zařízeních Windows: PlayReady je pouze DRM v Shoduje k dispozici na zařízeních Windows (PlayReady SL3000). Služby datových proudů pomocí PlayReady prostřednictvím EME nebo aplikace UWP a nabízejí vyšší kvalitu videa pomocí PlayReady SL3000 než jiné DRM. 2K obsahu se zpravidla postupují Chrome a Firefox a 4K obsahu pomocí Microsoft Edge/IE11 nebo aplikace UWP na stejné zařízení (v závislosti na nastavení služeb a implementaci).


#### <a name="using-eme-for-widevine"></a>Použití EME pro Widevine

Moderní prohlížeče podporující EME/Widevine, například Chrome 41 + na Windows 10, Windows 8.1, Mac OSX Yosemite a Chrome na Android bodu 4.4.4 je Google Widevine DRM za EME.

![Použití EME pro Widevine](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine1.png)

Všimněte si, že Widevine nezabrání jednu provádět snímku obrazovky chráněné videa.

![Použití EME pro Widevine](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine2.png)

### <a name="not-entitled-users"></a>Není oprávnění uživatelům

Pokud uživatel není členem skupiny "Nárok Users", uživatel nebude moct "Kontrola oprávnění" a službě licenci s víc DRM odmítne vystavení požadované licencí, jak je ukázáno v následujícím příkladu. Podrobný popis je "získat licenci se nezdařila", což je vytvořena.

![Zrušení oprávnění uživatelům](./media/media-services-cenc-with-multidrm-access-control/media-services-unentitledusers.png)

### <a name="running-custom-secure-token-service"></a>Spuštění vlastní tokenu služba zabezpečené

Pro situace spuštěných vlastní zabezpečené tokenu Service (Služba tokenů zabezpečení) JWT token vytvoří webová služba tokenů zabezpečení vlastní pomocí klávesy symetrickou nebo asymetrické. 

Případ použití symetrickou klíč (Chrome):

![Spuštění vlastní služba tokenů zabezpečení](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts1.png)

V případě použití asymetrické klíč prostřednictvím X509 certifikátů (prohlížeči Microsoft moderní).

![Spuštění vlastní služba tokenů zabezpečení](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts2.png)

V obou případech ověřování uživatelů se nezmění – prostřednictvím Azure AD. Jediný rozdíl je, že JWT tokeny vydává vlastní STS místo Azure AD. Samozřejmě při konfiguraci dynamické ochranu CENC omezení služba pro doručování licenci Určuje typ JWT token symetrickou nebo asymetrické klíče.

## <a name="summary"></a>Souhrn

V tomto dokumentu, jsme popisované CENC s více native DRM a řízení přístupu přes tokenu ověření: jeho návrh a implementace pomocí Azure, Azure Media Services a Azure Media Player.

- Návrh odkaz jsou uvedeny obsahující všechny potřebné součásti podsystému DRM/CENC;
- Implementace odkaz na Azure, Azure Media Services a Azure Media Player.
- Některé přímo zapojené návrh a implementace také témata.


##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

###<a name="acknowledgments"></a>Potvrzení 

William Potokar Mingfei Jan, Roland Le frank, Kilroy Hughes, Julie Kornich
