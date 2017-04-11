
<properties
   pageTitle="Ověřování scénáře Azure AD | Microsoft Azure"
   description="Základní informace o pět obvyklé scénáře ověřování pro Azure Active Directory (AAD)"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="authentication-scenarios-for-azure-ad"></a>Ověřování scénáře Azure AD

Azure Active Directory (Azure AD) zjednodušuje ověřování pro vývojáře poskytnutím identitu jako zdroj service s podporou standardní protokoly například OAuth 2.0 a OpenID připojení, stejně jako otevřené knihoven pro jiné platformy můžete začít rychle kódování. Tento dokument vám pomůže pochopit různých scénářích Azure AD podporuje a vám ukáže, jak začít. Je rozdělen do následujících částí:

- [Základní informace o ověřování v Azure AD](#basics-of-authentication-in-azure-ad)

- [Nároky v Azure AD tokenů zabezpečení](#claims-in-azure-ad-security-tokens)

- [Základní informace o registraci aplikace v Azure AD](#basics-of-registering-an-application-in-azure-ad)

- [Typy aplikací a scénáře](#application-types-and-scenarios)

  - [Webový prohlížeč webovou aplikaci](#web-browser-to-web-application)

  - [Použití jedné stránky (zabezpečené ověřování HESLA)](#single-page-application-spa)

  - [Nativní aplikaci webového rozhraní API](#native-application-to-web-api)

  - [Webová aplikace na Web rozhraní API](#web-application-to-web-api)

  - [Démon nebo serveru aplikací webového rozhraní API](#daemon-or-server-application-to-web-api)



## <a name="basics-of-authentication-in-azure-ad"></a>Základní informace o ověřování v Azure AD

Základní pojmy ověřování v Azure AD neznáte, přečtěte si tuto část. V ostatních případech je vhodné přejděte dolů do části [typy aplikací a scénáře](#application-types-and-scenarios).

Dále se podívejme základní scénáře, kde je potřeba identity: uživatel ve webovém prohlížeči potřebuje k ověření do webové aplikace. Tento scénář je popsán podrobněji v části [Webového prohlížeče webové aplikace](#web-browser-to-web-application) , ale je užitečné výchozí bod ilustrují funkce Azure AD a conceptualize fungování scénáře. Zvažte následující diagram v tomto scénáři:

![Základní informace o přihlašování k webové aplikace](./media/active-directory-authentication-scenarios/basics_of_auth_in_aad.png)

S diagramem nad na paměti tady je, co je potřeba vědět o různými součástmi:

- Azure AD je zprostředkovatele identit zodpovědný za ověření identity uživatele a aplikace, které jsou v adresáři organizace a nakonec vydání tokenů zabezpečení po úspěšném ověření těchto uživatelů a aplikací.


- Aplikace, který chce externí ověřování Azure AD musí být registrován v Azure AD, které zaregistruje a jednoznačně identifikuje aplikaci v adresáři.


- Vývojáři můžete pomocí knihoven ověřování otevřít zdroj Azure AD pro snadnou ověřování zpracováním podrobnosti protokolu za vás. Další informace najdete v tématu [Azure Active Directory Authentication knihoven](active-directory-authentication-libraries.md) .


• Po ověření uživatele aplikace musí ověřit uživatele tokenu zabezpečení jsme narazili zajistit, že ověření bylo úspěšné pro určené strany. Mohou vývojáři knihoven ujednaných ověřování zpracovat ověření kterýkoliv token z Azure AD, včetně JSON Web tokeny (JWT) nebo SAML 2.0. Pokud chcete provést ověření ručně, najdete v dokumentaci [JWT tokenu rutiny](https://msdn.microsoft.com/library/dn205065.aspx) .


> [AZURE.IMPORTANT] Azure AD pomocí veřejné klíčové kryptografický podepsat tokeny a ověřte platnost. Najdete [Důležité informace o přihlašování klíč přechodu v Azure AD](active-directory-signing-key-rollover.md) podrobnosti o potřebné logickou, že máte v aplikaci tak, aby se aktualizuje vždycky nejnovější klíče.


• Tok žádosti o schůzku a odpovědi do procesu ověřování, je určený podle ověřovací protokol, který jste použili, například OAuth 2.0 OpenID připojit, WS Federation nebo SAML 2.0. Tyto protokoly jsou podrobněji v tématu [Azure Active Directory Authentication protokoly](active-directory-authentication-protocols.md) a v následujících částech.

> [AZURE.NOTE] Azure AD podporuje OAuth 2.0 a OpenID připojení organizace pro standardy, díky kterým rozsáhlé pomocí nosný tokenů, včetně nosný tokeny tvaru JWTs. Nosný token je tokenu zabezpečení lightweight, který zajišťuje přístup "nosný" chráněné zdroji. V tomto smyslu "nosný" je osobě, která Visio svádět k zahrnutí tokenu. Když na oslavu nejdřív ověřuje s Azure AD pro příjem nosnému tokenu zabezpečení token pro předávání a úložiště nebudou přijata požadované kroky, můžete být zachyceny a použity před nechtěným osobou. Během pár tokenů zabezpečení mají předdefinované mechanismus zabránit neoprávněnými osobami je používat, nosný tokeny nemusí tento postup a musí být přepravovány v zabezpečeného kanálu například transport layer security (HTTPS). Pokud nosný token jsou přenášena v vymazat, člověka v prostředním útok lze škodlivým skupinou získat tokenu a použijte ji k neoprávněnému přístupu k chráněného zdroj. Platí stejné zásady zabezpečení při ukládání nebo ukládání do mezipaměti nosný tokeny pro pozdější použití. Vždy zajistěte, aby vaše aplikace přenáší a ukládá nosný tokenů zabezpečení způsobem. Další otázky bezpečnosti pro na nosný tokenů najdete v článku [RFC 6750 oddílu 5](http://tools.ietf.org/html/rfc6750).


Teď, když máte přehled základy, přečtěte si následující části obsahují pochopit, jak zřizování funguje v Azure AD a obvyklé scénáře Azure AD podporuje.


## <a name="claims-in-azure-ad-security-tokens"></a>Nároky v Azure AD tokenů zabezpečení

Tokenů zabezpečení vydán Azure AD obsahovat nároky nebo výrazy informací o tématu, které byl ověřen. Tyto deklarací lze použít v aplikaci pro různé úlohy. Například se mohou sloužit k ověření tokenu, identifikovat subjektu adresáře klienta, zobrazit informace o uživatelích, autorizaci předmět atd. Nároky prezentovat v libovolné tokenu zabezpečení jsou závislé na typ token zadejte přihlašovací údaje slouží k ověření uživatele a konfigurace aplikace. Stručný popis jednotlivých typů deklarace které Azure AD není uvedený v následující tabulce. Další informace najdete v příručce [podporovaných tokenů a typy deklarace](active-directory-token-and-claims.md).


| Deklarace | Popis |
|-------|-------------|
| ID aplikace | Určuje aplikace, která používá tokenu.
| Cílové skupiny | Určuje token je určená pro příjemce zdrojů. |
| Odkaz třídy kontextu ověřování aplikace | Určuje, jak klienta byl ověřené (veřejné klient porovnání důvěrné klienta). |
| Ověřování rychlých zpráv | Záznamů datum a čas, kdy došlo k ověření. |
| Metody ověřování | Označuje, jak byla ověřeny předmět tokenu (heslo, certifikát, atd.). |
| Křestní jméno | Poskytuje křestní jméno uživatele, jak je to nastavené v Azure AD. |
| Skupiny | Obsahuje skupiny ID Azure AD objektů, které uživatel patří do skupiny. |
| Zprostředkovatele identit | Zprostředkovatele identit, které ověřeny předmět tokenu záznamů. |
| Vydané | Zaznamenává čas, kdy byl vydán tokenu, často používá u tokenu aktuálnost. |
| Vydavatel | Určuje Služba tokenů zabezpečení, které vyzařovaného tokenu, jakož i Azure AD klienta. |
| Příjmení | Obsahuje příjmení uživatele, jak je to nastavené v Azure AD. |
| Jméno | Poskytuje lidské čitelné hodnotu, která určuje předmět tokenu. |
| Id objektu | V Azure AD obsahuje neměnný jedinečný identifikátor předmět. |
| Role | Obsahuje popisných názvů Azure AD aplikace role poskytovaná uživatele. |
| Rozsah | Označuje oprávněním ke klientské aplikaci. |
| Předmět | Označuje hlavního uživatele o tom, které tokenu uplatňuje informace. |
| Id klienta | Obsahuje neměnný jedinečný identifikátor klienta adresář, který vystavil tokenu. |
| Životnost tokenu | Definuje časový interval, ve kterém je token platný. |
| Hlavní jméno uživatele | Windows obsahuje uživatelské jméno hlavní předmětu. |
| Verze | Obsahuje číslo verze tokenu. |


## <a name="basics-of-registering-an-application-in-azure-ad"></a>Základní informace o registraci aplikace v Azure AD

Libovolné aplikaci, která outsources ověřování Azure AD musí být registrován v adresáři. Tento krok zahrnuje Azure AD informací o aplikaci, včetně URL, kde je umístěn, adresu URL odesílat odpovědi po ověření URI k identifikaci aplikace a další. Tyto informace jsou požadovány klíčové z několika důvodů:

- Azure AD musí souřadnice komunikovat s aplikací při zpracování přihlašování nebo výměnou tokeny. Jedná se o těchto věcí:

  - Identifikátor URI ID aplikace: Identifikátor aplikace. Tato hodnota se pošle Azure AD při ověřování určujících, které aplikace volající vyžaduje token. Tato hodnota kromě toho je součástí tokenu tak, aby aplikace ví, že byl zamýšleného cíle.


  - Odpovědět adresy URL a přesměrovat URI: v případě rozhraní API webových nebo spuštění webové aplikace je adresa URL odpovědět na místo, na který Azure AD posílat odpověď ověřování, včetně token Pokud ověření bylo úspěšné. V případě nativní aplikaci identifikátor URI přesměrovat je jedinečný identifikátor, do které vás přesměruje Azure AD uživatelského agenta v žádosti o OAuth 2.0.


  - ID klienta: ID pro aplikaci, která je generováno aplikací Azure AD při registraci aplikace. Při požadování se tak mohli ověřovat kód nebo token, ID klienta a klíč jsou odeslány Azure AD při ověřování.


  - Klíč: Klávesu odesílané spolu s ID klienta při ověřování Azure AD pro volání webového rozhraní API.


- Azure AD musí zajistit, aby že aplikace má požadovaná oprávnění pro přístup k datům adresáři, jiných aplikací ve vaší organizaci a tak dále

Zřízení změní přehlednější, když víte, že existují dvě kategorie aplikací, které se dají vyvinuté a integrovaný s Azure AD:

- Jednoduché klienta aplikace: aplikace jednoho klienta je určená pro použití v jedné organizace. Toto jsou obvykle řádku obchodní aplikací (LoB) napsal vývojář enterprise. Aplikace jednoho klienta pouze musí uživatelé v jednom adresáři nemají přístup k, a proto ho jenom musí zřízení v jednom adresáři. Tyto aplikace jsou obvykle registrované vývojářem v organizaci.


- Více klienta aplikace: aplikace více klienta jsou určeny pro použití v mnoha organizacích, nikoli pouze jednu organizace. Toto jsou obvykle software – jako-(SaaS) aplikace služeb napsal nezávisle prodejci. Je nutné zřízení do každého adresáře místo, kam se bude používat, která vyžaduje uživatel nebo správce souhlas k registraci je aplikace více klienta. Tento proces souhlas, spustí se aplikace registrované v adresáři a se jim přístup k rozhraní API grafu nebo možná jiné webového rozhraní API. Když uživatel nebo správce od jiné organizace zaregistruje k používání aplikace, zobrazí se dialogové okno zobrazující oprávnění, která vyžaduje aplikaci. Uživatel nebo správce můžete pak souhlas s aplikací, který dává přístup aplikace uvedená data a nakonec zaregistruje aplikace ve své adresář. Další informace najdete v tématu [Přehled Framework souhlas](active-directory-integrating-applications.md#overview-of-the-consent-framework).

Některé další aspekty, které můžou nastat při vytváření aplikace více klienta místo aplikace jednoho klienta. Například pokud aplikace jsou zpřístupnění uživatelům ve více adresářích, je třeba mechanismus rozhodnout, jaký klienta jsou. Jednoho tenanta potřebuje pouze náhledu určené vlastní adresář pro uživatele, zatímco aplikace více klienta potřebuje k identifikaci určitého uživatele ze všech adresářů v Azure AD. K provedení této úlohy, Azure AD obsahuje běžné koncový bod ověřování kde libovolné aplikaci více klienta můžete směrovat přihlašovací požadavky, místo koncový bod specifické pro klienta. Tento koncový bod probíhá https://login.microsoftonline.com/common pro všechny adresáře Azure AD, že koncový bod specifické pro klienta mohou být https://login.microsoftonline.com/contoso.onmicrosoft.com. Běžné koncový bod zejména je důležité zvážit při vytváření aplikace, protože budete potřebovat nezbytná logiku zpracovávání víc klientů během přihlašování, odhlašovací a tokenu ověření.

Pokud jsou v současné době vyvíjí aplikace jednoho klienta, ale mají být k dispozici pro mnoho organizací, můžete snadno provedete změny aplikace a její konfigurace v Azure AD snažíme usnadnit jeho více klienta může. Kromě toho Azure AD používá stejnou podpisový klíč pro všechny tokeny ve všech adresářích, zda zadání ověřování jednoho klienta nebo více klienta aplikace.

Každý scénář uvedené v tomto dokumentu obsahuje dílčí oddíl, který popisuje dodržováním předpisů zřizovací. Podrobnější informace o zřizujete aplikaci v Azure AD a rozdíly mezi aplikacemi jednoho a více klienta najdete další informace najdete [Aplikace integrace se službou Azure Active Directory](active-directory-integrating-applications.md) . Pokračujte ve čtení porozumět obvyklé scénáře aplikace v Azure AD.

## <a name="application-types-and-scenarios"></a>Typy aplikací a scénáře

Všech případech popsaných v tomto dokumentu lze vytvořit pomocí různých jazycích a platformách. Budou se všechny podporovaným dokončení ukázky, které jsou k dispozici v naší [Ukázky příručka](active-directory-code-samples.md)nebo přímo z odpovídající [Github vzorku úložištích](https://github.com/Azure-Samples?utf8=%E2%9C%93&query=active-directory). Kromě toho pokud aplikace potřebuje segmentu od začátku do konce scénář nebo konkrétní údaj, ve většině případů tuto funkci lze přidat nezávisle na sobě. Například pokud máte nativní aplikaci, která volá webového rozhraní API, můžete snadno přidat webovou aplikaci, která volá také webového rozhraní API. Následující obrázek znázorňuje tyto scénáře a typy aplikací, a jak můžete přidávat různé součásti:

![Typy aplikací a scénáře](./media/active-directory-authentication-scenarios/application_types_and_scenarios.png)

Toto jsou pět scénáře primární aplikací nepodporuje Azure AD:

- [Webový prohlížeč webové aplikace](#web-browser-to-web-application): uživatel musí se přihlásit k webové aplikace, která je zajištěná Azure AD.

- [Aplikace zabezpečené (jednoho ověřování HESLA stránka)](#single-page-application-spa): uživatel musí se přihlásit k aplikaci jednu stránku, která je zabezpečená Azure AD.

- [Nativní aplikaci rozhraní API webových](#native-application-to-web-api): nativní aplikace, která poběží na počítači, tabletu nebo telefonu potřebuje k ověření uživatele z webového rozhraní API zajištěná Azure AD získat materiály.

- [Webové aplikace k rozhraní API webových](#web-application-to-web-api): webové aplikace, musí z webového rozhraní API zajištěná Azure AD získat materiály.

- [Démon nebo rozhraní API webových aplikací serveru](#daemon-or-server-application-to-web-api): démon aplikace nebo aplikace serveru bez uživatelského rozhraní web potřebuje z webového rozhraní API zajištěná Azure AD získat materiály.

### <a name="web-browser-to-web-application"></a>Webový prohlížeč webovou aplikaci

Tato část popisuje aplikace, která ověřuje uživatele ve webovém prohlížeči do webové aplikace. V tomto scénáři webové aplikace směrovat tak, aby v prohlížeči uživatele přihlašovat je Azure AD. Azure AD vrátí odpověď přihlášení pomocí prohlížeče uživatele, který obsahuje tvrzení o uživatele systému tokenu zabezpečení. Tento scénář podporuje přihlašování pomocí protokolů WS Federation SAML 2.0 a OpenID připojení.


#### <a name="diagram"></a>Diagram
![Tok ověřování pro prohlížeče webové aplikace](./media/active-directory-authentication-scenarios/web_browser_to_web_api.png)


#### <a name="description-of-protocol-flow"></a>Popis směrování Protocol (protokol)


1. Pokud uživatel návštěvy aplikace a potřeb k přihlášení, budou přesměrováni prostřednictvím žádost přihlašovací koncový bod ověřování v Azure AD.


2. Uživatel přihlásí na přihlašovací stránce.


3. Pokud je ověření úspěšné, Azure AD vytvoří token ověřování a vrátí odpověď přihlašovací adresa URL aplikace odpověď, nakonfigurovaný na portálu Správa Azure. Produkční aplikaci třeba tato adresa URL odpovědět HTTPS. Vrácené token obsahuje tvrzení o uživatele a Azure AD, která jsou potřebná aplikací pro ověřování tokenu.


4. Aplikace ověří tokenu pomocí veřejným klíčem podpisu a Vystavitel informací dostupných na dokument metadat federace Azure AD. Po aplikace ověří tokenu, Azure AD spustí novou relaci s uživatelem. Pro tuto relaci umožňuje uživatelům přístup k aplikaci, dokud vypršením jeho platnosti.


#### <a name="code-samples"></a>Ukázky


Najdete v části ukázek kódu pro webový prohlížeč scénáře webové aplikace. A kontrolujte často – přičteme nové ukázky vždy. [Webový prohlížeč webové aplikace](active-directory-code-samples.md#web-browser-to-web-application).


#### <a name="registering"></a>Registrace


- Jednoho klienta: Pokud vytváříte aplikace určené pro organizace, ho musí být registrován do adresáře vaší společnosti pomocí portálu pro správu Azure.


- Více klienta: Pokud vytváříte aplikace, která může používat uživatelé mimo vaši organizaci, ho musí být registrován do adresáře vaší společnosti, ale musí být také registrován v každé organizace adresář, který budou používat aplikaci. Zpřístupnění aplikace ve své adresáře, můžete zadat proces registrace pro zákazníky, které umožňuje, aby souhlas aplikaci. Pokud se chcete zaregistrovat aplikace, se zobrazí se dialogové okno zobrazující oprávnění, která vyžaduje aplikaci a potom možnost souhlas. V závislosti na požadovaná oprávnění správce v rámci organizace může být nutné souhlas. Je-li uživatel nebo správce souhlasí, aplikace registrovány v jejich adresář. Další informace najdete v tématu [Aplikací integrace se službou Azure Active Directory](active-directory-integrating-applications.md).


#### <a name="token-expiration"></a>Vypršení platnosti tokenu

Relace uživatele vyprší po vypršení platnosti tokenu vydán Azure AD. Aplikace zkrátit tohoto časového období podle potřeby, jako je odhlášení uživatelů podle určité době nečinnosti. Když skončí platnost relace, se uživateli výzva k se znovu přihlaste.





### <a name="single-page-application-spa"></a>Použití jedné stránky (zabezpečené ověřování HESLA)

Tato část popisuje ověřování pro jednu stránku aplikaci, kterou používá Azure AD OAuth 2.0 implicitní uděluje a zajistit jeho webového rozhraní API zpět ukončit. Jednotlivé stránky aplikace jsou obvykle strukturovaný jako prezentaci vrstvy JavaScript (front-end), které se spouští v prohlížeči a rozhraní API webových back-end spustí na serveru a implementuje logiku aplikace. Další informace o implicitním povolení grant (udělit) a nápovědy se rozhodnete, zda je nejlepší pro aplikace nefunguje, najdete v článku [Principy toku implicitní udělit OAuth2 v Azure Active Directory](active-directory-dev-understanding-oauth2-implicit-grant.md).

V tomto scénáři Pokud se uživatel přihlásí, JavaScript přední konečná použití [Active Directory Authentication Library JavaScript (ADAL. JS)](https://github.com/AzureAD/azure-activedirectory-library-for-js/tree/dev) a udělení implicitní se tak mohli ověřovat získat token ID (id_token) z Azure AD. Cached tokenu a klientovi připojí se k žádosti o jako nosný token při volání na své rozhraní API webových zálohovat databázi, která je zabezpečená pomocí OWIN middleware. 
#### <a name="diagram"></a>Diagram

![Diagram jedné stránky aplikací](./media/active-directory-authentication-scenarios/single_page_app.png)

#### <a name="description-of-protocol-flow"></a>Popis směrování Protocol (protokol)

1. Uživatel přejde do webové aplikace.


2. Aplikace vrátí JavaScript front-end (prezentace layer) do prohlížeče.


3. Uživatel spustí přihlášení, například kliknutím na znaménko v odkazu. Koncový bod se tak mohli ověřovat Azure AD požádat o ID token odešle získáte prohlížeč. Tento požadavek zahrnuje klienta ID a odpovědět adresa URL parametry dotazu.


4. Azure AD ověří adresu URL odpovědět proti registrovanou adresu URL odpovědět nakonfigurovaný na portálu Správa Azure.


5. Uživatel přihlásí na přihlašovací stránce.


6. Pokud je ověření úspěšné, Azure AD vytvoří ID token a vrácena jako část adresy URL (#) na adresu URL odpovědět aplikace. Produkční aplikaci třeba tato adresa URL odpovědět HTTPS. Vrácené token obsahuje tvrzení o uživatele a Azure AD, která jsou potřebná aplikací pro ověřování tokenu.


7. Kód klienta JavaScript spuštěné v prohlížeči extrahuje tokenu z odpověď pro použití v zabezpečení volání webové aplikace, že ukončit rozhraní API zpět.


8. V prohlížeči hovorů webové aplikace rozhraní API zpět končit přístupový token v záhlaví se tak mohli ověřovat.

#### <a name="code-samples"></a>Ukázky


V tématu ukázky scénářích aplikace zabezpečené (jednoho ověřování HESLA stránky). Zkontrolujte často – přičteme nové ukázky vždy. [Použití jedné stránky (zabezpečené ověřování HESLA)](active-directory-code-samples.md#single-page-application-spa).


#### <a name="registering"></a>Registrace


- Jednoho klienta: Pokud vytváříte aplikace určené pro organizace, ho musí být registrován do adresáře vaší společnosti pomocí portálu pro správu Azure.


- Více klienta: Pokud vytváříte aplikace, která může používat uživatelé mimo vaši organizaci, ho musí být registrován do adresáře vaší společnosti, ale musí být také registrován v každé organizace adresář, který budou používat aplikaci. Zpřístupnění aplikace ve své adresáře, můžete zadat proces registrace pro zákazníky, které umožňuje, aby souhlas aplikaci. Pokud se chcete zaregistrovat aplikace, se zobrazí se dialogové okno zobrazující oprávnění, která vyžaduje aplikaci a potom možnost souhlas. V závislosti na požadovaná oprávnění správce v rámci organizace může být nutné souhlas. Je-li uživatel nebo správce souhlasí, aplikace registrovány v jejich adresář. Další informace najdete v tématu [Aplikací integrace se službou Azure Active Directory](active-directory-integrating-applications.md).

Po registraci aplikace, musí být nakonfigurované pro protokol OAuth 2.0 implicitní grant (udělit). Ve výchozím nastavení není k dispozici tento protokol pro aplikace. Povolení implicitní grant (udělit) OAuth2 protokolu aplikace, stáhnout manifestu aplikace z portálu Správa Azure, nastavte hodnotu "oauth2AllowImplicitFlow" true (pravda) a pak nahrajte seznamu zpět k portálu. Podrobné pokyny najdete v tématu [Povolení OAuth 2.0 implicitní grant (udělit) pro jednotlivé aplikace stránky](active-directory-integrating-applications.md).


#### <a name="token-expiration"></a>Vypršení platnosti tokenu

Při použití ADAL.js ke správě ověřování s Azure AD využívat několik funkcí, které usnadňují aktualizace token vypršela platnost i začíná tokeny pro další webové rozhraní API prostředky, které lze svolat aplikace. Pokud uživatel úspěšně s Azure AD ověřuje, je pro uživatele mezi prohlížečem a Azure AD vytvořena relace zajištěná soubor cookie. Je důležité mít na paměti, že existuje relace mezi uživateli a Azure AD a nikoli mezi uživateli a webové aplikace na serveru. Když skončí platnost token, ADAL.js používá pro tuto relaci tiše získat další token. Je to pomocí skryté iFrame odesílat a přijímat žádost pomocí protokolu implicitní grant (udělit) OAuth. ADAL.js můžete taky použít tentýž mechanismus tiše a získat přístup tokeny z Azure AD pro jiné webové API prostředky, které aplikace zavolat dlouhou, jak tyto materiály podpory více origin zdroje sdílení (CORS), jsou registrované v adresáři uživatele a všechny požadované byl souhlas uživatelem při přihlášení.


### <a name="native-application-to-web-api"></a>Nativní aplikaci webového rozhraní API


Tato část popisuje nativní aplikaci, která volá webového rozhraní API jménem uživatele. Tento scénář je založená na typ grant (udělit) OAuth 2.0 se tak mohli ověřovat kódu s klientem veřejné podle popisu v části 4.1 [specifikace OAuth 2.0](http://tools.ietf.org/html/rfc6749). Nativní aplikaci získá přístupový token pro uživatele pomocí protokolu OAuth 2.0. Tento přístupový token odeslaný klikněte v žádosti o webového rozhraní API, která povolí uživatele a vrací požadovanou zdroje.

#### <a name="diagram"></a>Diagram

![Nativní aplikaci webového rozhraní API diagramu](./media/active-directory-authentication-scenarios/native_app_to_web_api.png)

#### <a name="authentication-flow-for-native-application-to-api"></a>Tok ověřování pro nativní aplikaci rozhraní API

#### <a name="description-of-protocol-flow"></a>Popis směrování Protocol (protokol)


Pokud používáte knihoven ověřování AD, maximálně podrobnosti protokolu píše níže jsou zpracovány za vás, například automaticky otevírané okno prohlížeče, tokenu ukládání do mezipaměti a zpracování tokenů aktualizace.

1. V prohlížeči místní že nativní aplikaci žádá koncový bod se tak mohli ověřovat v Azure AD. Tento žádost o obsahuje kód klienta a přesměrování URI nativní aplikaci uvedeno v portálu pro správu a přihláška Identifikátor URI webového rozhraní API. Pokud uživateli nebyl už přihlásili, se výzva k přihlášení znovu


2. Azure AD ověřuje uživatele. Pokud je aplikace více klienta a souhlasu se členství požaduje pro aplikaci, uživatel bude muset souhlas, pokud je to ještě neudělali. Po udělení souhlas a po úspěšném ověření Azure AD problémy odpověď se tak mohli ověřovat kód zpátky na klientské aplikace přesměrování URI.


3. Pokud Azure AD povolení kód odpověď přesměrování URI, klientské aplikaci zastaví interakce prohlížeče a extrahuje kód se tak mohli ověřovat z odpovědi. Pomocí následujícího se tak mohli ověřovat kódu, klientské aplikaci pošle žádost o Azure AD tokenu koncový bod, který obsahuje kód se tak mohli ověřovat, podrobnosti o klientské aplikaci (ID klienta a přesměrování URI) a požadovaný zdroj (přihláška Identifikátor URI webového rozhraní API).


4. Povolení kód a informace o rozhraní API aplikace a webový klient ověřuje Azure AD. Po úspěšném ověření Azure AD vrátí dva tokeny: přístupový token JWT a token JWT aktualizace. Kromě toho Azure AD vrátí základní informace o uživateli, například jejich zobrazované jméno a klientovi ID.


5. Přes protokol HTTPS používá klientské aplikace vrácené přístupový token JWT přidáte JWT řetězec s označením "Nosný" v záhlaví se tak mohli ověřovat žádosti o webového rozhraní API. Webového rozhraní API potom ověří JWT token a pokud bude ověření úspěšné, vrátí funkce požadované zdroje.


6. Když skončí platnost přístupový token, klientské aplikaci dojde k chybě, která označuje, že uživatel je potřeba ověřit znovu. Pokud má aplikace token platnou, lze použít získat nové přístupový token bez dotázání uživatele se znova přihlásit. Když skončí platnost token aktualizace, aplikace muset interaktivně ještě jednou ověření uživatele.


> [AZURE.NOTE] Aktualizace token vydán Azure AD lze použít pro přístup k více zdrojů. Například pokud máte klientské aplikace, která má oprávnění k volání dvou webového rozhraní API, token aktualizace mohou sloužit k získání přístupový token na jiných webu API i.


#### <a name="code-samples"></a>Ukázky


Nativní aplikaci rozhraní API webových scénáře najdete v článku ukázek kódu. A kontrolujte často – přičteme nové ukázky vždy. [Nativní aplikaci webového rozhraní API](active-directory-code-samples.md#native-application-to-web-api).


#### <a name="registering"></a>Registrace


- Jednoho klienta: Nativní obou registrovat aplikaci a webového rozhraní API musí být ve stejném adresáři v Azure AD. Jak vystavit sadu oprávnění, která se používají k omezení nativní aplikaci přístup k jeho zdroje je možné konfigurovat webového rozhraní API. Klientská aplikace je pak vybere požadovaná oprávnění z rozevírací nabídky "Oprávnění do jiných aplikací" v portálu pro správu Azure.


- Více klienta: Nejdřív nativní aplikaci pouze v zaznamenává vývojář nebo vydavatele adresář. Za druhé nativní aplikaci nakonfigurovaný označíte oprávnění, která se musí být funkční. Tento seznam potřebná oprávnění se zobrazí v dialogu při uživatel nebo správce v adresáři cílový nabízí souhlas s aplikací, které zpřístupníte uživatelům ve své organizaci. Některé aplikace vyžadují jenom uživatelskou úrovní oprávnění, která mohou všichni uživatelé v organizaci souhlas. Další aplikace vyžadují oprávnění na úrovni správce, které nelze souhlas uživatele v organizaci. Pouze správce adresáře můžete udělovat souhlas aplikace, které vyžadují této úrovni oprávnění. Je-li uživatel nebo správce souhlasí, jenom webového rozhraní API registrovány v jejich adresář. Další informace najdete v tématu [Aplikací integrace se službou Azure Active Directory](active-directory-integrating-applications.md).


#### <a name="token-expiration"></a>Vypršení platnosti tokenu


Pokud nativní aplikaci používá jeho kód se tak mohli ověřovat se dostat JWT tokenu, obdrží také token JWT aktualizace. Když skončí platnost přístupový token, token aktualizace mohou sloužit k znovu ověření uživatele, aniž by bylo nutné, aby se znovu přihlaste. Tato aktualizace pak pomocí tokenu ověřuje uživatele, jehož výsledkem nové přístupový token a aktualizace tokenu.





### <a name="web-application-to-web-api"></a>Webová aplikace na Web rozhraní API


Tato část popisuje webovou aplikaci, kterou je potřeba získat materiály z webového rozhraní API. V tomto scénáři existují dva typy identity používající webové aplikace ověřování a volání webového rozhraní API: identitu aplikace nebo identity delegované uživatele.

*Identitu:* Tento scénář používá grant (udělit) OAuth 2.0 klienta přihlašovací údaje k ověření jako aplikace a přístup k webu rozhraní API. Při použití identitu aplikace webu API pouze zjistit, že webové aplikace volá, jako webu API neobdrží všechny informace o uživateli. Pokud aplikace dostává informace o uživateli, odešle se prostřednictvím protokolu aplikace a není podepsáno Azure AD. Vztah důvěryhodnosti webového rozhraní API webové aplikace ověření uživatele. Z tohoto důvodu tento způsob je místo toho možnost podsystému důvěryhodné.

*Delegované identita uživatele:* Tento scénář lze provést dvěma způsoby: OpenID připojení a OAuth 2.0 se tak mohli ověřovat kód grant (udělit) s klientem důvěrné. Webová aplikace získá přístupový token pro uživatele, který ukáže webového rozhraní API, který uživatel úspěšně ověřené webové aplikace a aby bylo možno získat identita uživatele delegované volání webového rozhraní API webové aplikace. Tento přístupový token odeslaný v žádosti o webového rozhraní API, která povolí uživatele a vrací požadovanou zdroje.

#### <a name="diagram"></a>Diagram

![Webové aplikace k rozhraní API webových diagramu](./media/active-directory-authentication-scenarios/web_app_to_web_api.png)



#### <a name="description-of-protocol-flow"></a>Popis směrování Protocol (protokol)

V toku níže jsou uvedeny identitu a typy identity delegované uživatele. Klíčové rozdíl mezi nimi je, že identita uživatele delegované musíte nejprve získat kód se tak mohli ověřovat před uživatel může přihlašovací a získat přístup k webu rozhraní API.

##### <a name="application-identity-with-oauth-20-client-credentials-grant"></a>Identita aplikace s klientem OAuth 2.0 pověření grant (udělit)

1. Je uživatel přihlášený Azure AD ve webové aplikaci (viz [Webového prohlížeče webové aplikace](#web-browser-to-web-application) nad).


2. Webová aplikace musí získat přístupový token tak, aby ho ověřování webového rozhraní API a načíst požadovaná zdroje. Vytvoří žádost Azure AD tokenu koncový bod, poskytnutí pověření, ID klienta a webového rozhraní API aplikace Identifikátor URI.


3. Azure AD ověří aplikace a vrátí přístupový token JWT, který slouží k volání webového rozhraní API.


4. Přes protokol HTTPS webové aplikace používá vrácené přístupový token JWT přidáte JWT řetězec s označením "Nosný" v záhlaví se tak mohli ověřovat žádosti o webového rozhraní API. Webového rozhraní API potom ověří JWT token a pokud bude ověření úspěšné, vrátí funkce požadované zdroje.

##### <a name="delegated-user-identity-with-openid-connect"></a>Identita uživatele delegované s OpenID připojení

1. Je uživatel přihlášený k webové aplikace pomocí Azure AD (viz části [Webového prohlížeče webové aplikace](#web-browser-to-web-application) ). Pokud uživatel webové aplikace není zatím souhlas umožňuje nastavit webovou aplikaci pro volání webového rozhraní API jejím jménem, uživatel bude muset souhlas. Aplikace se zobrazí oprávnění, která vyžaduje a pokud je některý z těchto oprávnění na úrovni správce, normální uživatele v adresáři nebude moct souhlas. Tento proces souhlas platí jenom pro více klienta aplikace, ne jednoho klienta aplikace podle aplikace se už máte potřebná oprávnění. Když uživatel přihlášený, webové aplikace doručeny token ID s informacemi o uživateli, jakož i kódem se tak mohli ověřovat.


2. Použití kódu se tak mohli ověřovat vydán Azure AD, webové aplikace pošle žádost o Azure AD tokenu koncový bod, který obsahuje kód se tak mohli ověřovat, podrobnosti o klientské aplikaci (ID klienta a přesměrování URI) a požadovaný zdroj (přihláška Identifikátor URI webového rozhraní API).


3. Povolení kód a informace o webové aplikace a rozhraní API webových ověřuje Azure AD. Po úspěšném ověření Azure AD vrátí dva tokeny: přístupový token JWT a token JWT aktualizace.


4. Přes protokol HTTPS webové aplikace používá vrácené přístupový token JWT přidáte JWT řetězec s označením "Nosný" v záhlaví se tak mohli ověřovat žádosti o webového rozhraní API. Webového rozhraní API potom ověří JWT token a pokud bude ověření úspěšné, vrátí funkce požadované zdroje.

##### <a name="delegated-user-identity-with-oauth-20-authorization-code-grant"></a>Identita uživatele delegované s OAuth 2.0 se tak mohli ověřovat kód grant (udělit)

1. Uživatel je už přihlášení do webové aplikace, jehož ověření mechanismus nezávisí Azure AD.


2. Webová aplikace vyžaduje se tak mohli ověřovat kód získat přístupový token, takže problémy žádost o pomocí prohlížeče Azure AD se tak mohli ověřovat koncový bod, poskytuje ID klienta a přesměrovat URI pro webovou aplikaci po úspěšném ověření. Uživatel přihlásí Azure AD.


3. Pokud uživatel webové aplikace není zatím souhlas umožňuje nastavit webovou aplikaci pro volání webového rozhraní API jejím jménem, uživatel bude muset souhlas. Aplikace se zobrazí oprávnění, která vyžaduje a pokud je některý z těchto oprávnění na úrovni správce, normální uživatele v adresáři nebude moct souhlas. Tento proces souhlas platí jenom pro více klienta aplikace, ne jednoho klienta aplikace podle aplikace se už máte potřebná oprávnění.


4. Poté, co uživatel souhlasí, obdrží webové aplikace se tak mohli ověřovat kód, který potřebuje získat přístupový token.


5. Použití kódu se tak mohli ověřovat vydán Azure AD, webové aplikace pošle žádost o Azure AD tokenu koncový bod, který obsahuje kód se tak mohli ověřovat, podrobnosti o klientské aplikaci (ID klienta a přesměrování URI) a požadovaný zdroj (přihláška Identifikátor URI webového rozhraní API).


6. Povolení kód a informace o webové aplikace a rozhraní API webových ověřuje Azure AD. Po úspěšném ověření Azure AD vrátí dva tokeny: přístupový token JWT a token JWT aktualizace.


7. Přes protokol HTTPS webové aplikace používá vrácené přístupový token JWT přidáte JWT řetězec s označením "Nosný" v záhlaví se tak mohli ověřovat žádosti o webového rozhraní API. Webového rozhraní API potom ověří JWT token a pokud bude ověření úspěšné, vrátí funkce požadované zdroje.

#### <a name="code-samples"></a>Ukázky

V tématu ukázek kódu pro webovou aplikaci pro rozhraní API webových scénáře. A kontrolujte často – přičteme nové ukázky vždy. Webová [aplikace na Web API](active-directory-code-samples.md#web-application-to-web-api).


#### <a name="registering"></a>Registrace

- Jednoho klienta: Identitu i případech identity delegované uživatele, webové aplikace a rozhraní API webových musí být registrována ve stejném adresáři v Azure AD. Jak vystavit sadu oprávnění, která se používají k omezení webové aplikace přístup k jeho zdroje je možné konfigurovat webového rozhraní API. Pokud typ identity pověřený uživatel musí webové aplikace vyberte požadované oprávnění z rozevírací nabídky "Oprávnění do jiných aplikací" v portálu pro správu Azure. Tento krok není povinný, pokud typ identity aplikace.


- Více klienta: Nejdřív webové aplikace nakonfigurovaný označíte oprávnění, která se musí být funkční. Tento seznam potřebná oprávnění se zobrazí v dialogu při uživatel nebo správce v adresáři cílový nabízí souhlas s aplikací, které zpřístupníte uživatelům ve své organizaci. Některé aplikace vyžadují jenom uživatelskou úrovní oprávnění, která mohou všichni uživatelé v organizaci souhlas. Další aplikace vyžadují oprávnění na úrovni správce, které nelze souhlas uživatele v organizaci. Pouze správce adresáře můžete udělovat souhlas aplikace, které vyžadují této úrovni oprávnění. Když uživatel nebo správce souhlasí, webové aplikace a rozhraní API webových jsou oba registrované v jejich adresář.

#### <a name="token-expiration"></a>Vypršení platnosti tokenu

Pokud webové aplikace používá jeho kód se tak mohli ověřovat se dostat JWT tokenu, obdrží také token JWT aktualizace. Když skončí platnost přístupový token, token aktualizace mohou sloužit k znovu ověření uživatele, aniž by bylo nutné, aby se znovu přihlaste. Tato aktualizace pak pomocí tokenu ověřuje uživatele, jehož výsledkem nové přístupový token a aktualizace token.


### <a name="daemon-or-server-application-to-web-api"></a>Démon nebo serveru aplikací webového rozhraní API


Tato část popisuje démon nebo serveru aplikace, která potřebuje z webového rozhraní API získat materiály. Existují dva dílčí scénáře, které se vztahují k tomuto: démon, který je potřeba zavolat webového rozhraní API, založený na typu OAuth 2.0 klienta pověření grant (udělit); a aplikace server (například webového rozhraní API), kterou je potřeba zavolat webového rozhraní API, založená na specifikace koncept OAuth 2.0 On-Behalf-Of.

Scénáře aplikace démon potřebujete-li zavolat webového rozhraní API, je důležité pochopit několik věcí. Interakce s uživatelem, není možné pomocí démon aplikace, která vyžaduje, aby aplikace mít vlastní identity. Příklad aplikace démon je dávku nebo služba operačního systému běží na pozadí. Tento typ aplikace žádosti přístupový token pomocí své identity aplikací a prezentování svému klientovi ID, přihlašovacích údajů (heslo nebo certifikát) a aplikace Identifikátor URI Azure AD. Po úspěšném ověření démona obdrží přístupový token z Azure AD, pak sloužící k volání webového rozhraní API.

Scénáře aplikace server potřebujete-li zavolat webového rozhraní API, je vhodné Podíváme se na příklad. Představte si, že uživatel ověřil na nativní aplikaci a tento nativní aplikaci je potřeba zavolat webového rozhraní API. Azure AD problémy JWT přístupový token volání webového rozhraní API. Pokud rozhraní API webových je potřeba zavolat další podřízené webového rozhraní API, ho můžete toku na jménem s delegáta identita uživatele a ověřit druhé úrovně webového rozhraní API.

#### <a name="diagram"></a>Diagram

![Démon aplikaci nebo serveru rozhraní API webových diagramu](./media/active-directory-authentication-scenarios/daemon_server_app_to_web_api.png)

#### <a name="description-of-protocol-flow"></a>Popis směrování Protocol (protokol)

##### <a name="application-identity-with-oauth-20-client-credentials-grant"></a>Identita aplikace s klientem OAuth 2.0 pověření grant (udělit)

1. Nejdřív serveru potřebuje ověření s Azure AD jako sebe sama bez zásahu lidské například interaktivním dialogovým oknem přihlašování. Vytvoří žádost Azure AD tokenu koncový bod, poskytnutí přihlašovacích údajů, ID klienta a aplikace Identifikátor URI.


2. Azure AD ověří aplikace a vrátí přístupový token JWT, který slouží k volání webového rozhraní API.


3. Přes protokol HTTPS webové aplikace používá vrácené přístupový token JWT přidáte JWT řetězec s označením "Nosný" v záhlaví se tak mohli ověřovat žádosti o webového rozhraní API. Webového rozhraní API potom ověří JWT token a pokud bude ověření úspěšné, vrátí funkce požadované zdroje.


##### <a name="delegated-user-identity-with-oauth-20-on-behalf-of-draft-specification"></a>Identita uživatele delegované s specifikace na jménem Of koncept OAuth 2.0

Tok popsána níže předpokládá, že uživatel ověřil na jiné aplikace (například nativní aplikaci) a jejich identita uživatele byla použita k získání přístupový token na první úrovně webu rozhraní API.

1. Nativní aplikaci pošle přístupový token první úrovně webového rozhraní API.


2. Rozhraní API webových první osy, odešle žádost o Azure AD tokenu koncový bod, poskytuje svému klientovi ID a přihlašovací údaje, stejně jako uživatele přístupový token. Kromě toho žádost odeslaným pomocí on_behalf_of parametr, který označuje webu API požaduje nové tokeny volání podřízený web API jménem původní uživatele.


3. Azure AD ověří první úrovně webového rozhraní API má oprávnění pro přístup k rozhraní API webových druhé úrovně a ověří pozvánce na schůzku, vrácení přístupový token JWT a JWT aktualizace token první úrovně webového rozhraní API.


4. Přes protokol HTTPS rozhraní API webových první úrovně pak zavolá druhé úrovně webového rozhraní API přidáním tokenu řetězec v záhlaví se tak mohli ověřovat v žádosti o. První úrovně webového rozhraní API můžete dál volání druhé úrovně webového rozhraní API, dokud přístupový token a tokeny aktualizace jsou platné.

#### <a name="code-samples"></a>Ukázky

Naleznete v tématu ukázek kódu démon nebo serveru aplikací rozhraní API webových scénáře. A kontrolujte často – přičteme nové ukázky vždy. [Server nebo démon aplikací webového rozhraní API](active-directory-code-samples.md#server-or-daemon-application-to-web-api)

#### <a name="registering"></a>Registrace

- Jednoho klienta: Identitu i případech identity delegované uživatele, aplikace démon nebo serveru musí být registrována ve stejném adresáři v Azure AD. Jak vystavit sadu oprávnění, která se používají k omezení démona nebo serveru přístup k jeho zdroje je možné konfigurovat webového rozhraní API. Pokud typ identity delegované uživatele serveru potřebuje vyberte požadovaná oprávnění z rozevírací nabídky "Oprávnění do jiných aplikací" v portálu pro správu Azure. Tento krok není povinný, pokud typ identity aplikace.


- Více klienta: Nejdřív aplikace démon nebo server nakonfigurovaný vyznačení oprávnění, která se musí být funkční. Tento seznam potřebná oprávnění se zobrazí v dialogu při uživatel nebo správce v adresáři cílový nabízí souhlas s aplikací, které zpřístupníte uživatelům ve své organizaci. Některé aplikace vyžadují jenom uživatelskou úrovní oprávnění, která mohou všichni uživatelé v organizaci souhlas. Další aplikace vyžadují oprávnění na úrovni správce, které nelze souhlas uživatele v organizaci. Pouze správci adresáře můžete udělovat souhlas aplikace, které vyžadují této úrovni oprávnění. Když uživatel nebo správce souhlasí, obě webového rozhraní API v zaznamenává jejich adresář.

#### <a name="token-expiration"></a>Vypršení platnosti tokenu

Při prvním aplikace používá jeho kód se tak mohli ověřovat se dostat JWT tokenu, obdrží také token JWT aktualizace. Když skončí platnost přístupový token, token aktualizace mohou sloužit k znovu ověření uživatele bez zobrazení výzvy k zadání přihlašovacích údajů. Tato aktualizace pak pomocí tokenu ověřuje uživatele, jehož výsledkem nové přístupový token a aktualizace token.

## <a name="see-also"></a>Viz taky

[Příručka pro vývojáře služby Azure Active Directory](active-directory-developers-guide.md)

[Ukázky Azure Active Directory](active-directory-code-samples.md)

[Důležité informace o přihlašování Azure AD přechodu klíče](active-directory-signing-key-rollover.md)

[OAuth 2.0 v Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx)
