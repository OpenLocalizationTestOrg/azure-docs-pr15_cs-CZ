<properties
   pageTitle="Principy implicitní OAuth2 udělit toku v Azure Active Directory | Microsoft Azure"
   description="Přečtěte si další informace o Azure Active Directory provádění implicitní OAuth2 udělit toku, a jestli je nejlepší pro aplikaci."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="vibronet"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/17/2016"
   ms.author="vittorib;bryanla"/>

# <a name="understanding-the-oauth2-implicit-grant-flow-in-azure-active-directory-ad"></a>Principy tok implicitní udělit OAuth2 v Azure Active Directory (AD)

Udělení implicitní OAuth2 je notorious je udělení nejdelší seznam zabezpečení se ve specifikaci OAuth2. A ještě, který je přístup prováděným ADAL JS a tu, kterou doporučujeme při psaní aplikací zabezpečené ověřování HESLA. Co nabízí? Je všechna předmětem nevýhody: a jak zjistíte, udělení implicitní je nejlepším řešením stíhat pro aplikace, které používání rozhraní API webových prostřednictvím JavaScript v prohlížeči.

## <a name="what-is-the-oauth2-implicit-grant"></a>Co je OAuth2 implicitní grant (udělit)?

Quintessential [udělit OAuth2 se tak mohli ověřovat kód](https://tools.ietf.org/html/rfc6749#section-1.3.1) je se tak mohli ověřovat grant (udělit), který využívá dvou samostatných koncové body. Koncový bod se tak mohli ověřovat slouží k interakci fáze uživatele, jehož výsledkem je kódem se tak mohli ověřovat. Tokenu koncového bodu potom používá klienta pro výměnu kód přístupový token a často token aktualizace. Webové aplikace podporují prezentovat vlastní pověření aplikací tokenu koncový bod tak, aby se tak mohli ověřovat serveru může ověřovat klienta.

[Udělit OAuth2 implicitní](https://tools.ietf.org/html/rfc6749#section-1.3.2) je varianta povolení klienta přístupový token (a id_token v případě [Připojení OpenId](http://openid.net/specs/openid-connect-core-1_0.html)) a získat přímo z se tak mohli ověřovat koncový bod bez kontaktování tokenu koncový bod ani ověřování klientské aplikaci. Tato varianta byl speciálně pro JavaScript založené spuštěné ve webovém prohlížeči: ve specifikaci původní OAuth2 tokeny vracejí ve URI fragmentu. Které zpřístupní tokenu bitů kód JavaScript v klientovi, ale zaručuje, že nezahrnou do přesměrování směrem k serveru. Vrácení tokeny prostřednictvím prohlížeče přesměruje přímo z koncový bod se tak mohli ověřovat. Je také výhod odstraňování požadavky křížové origin hovory, nezbytná Pokud JavaScript aplikací je potřeba kontaktovat token koncový bod.

Důležité znakem implicitní udělit OAuth2 je fakt takové přecházel nikdy zpáteční aktualizace tokeny klientovi. Jsme bude vidět v následujícím oddílu, který není skutečně nezbytné a bude ve skutečnosti potíže se zabezpečením.

## <a name="suitable-scenarios-for-the-oauth2-implicit-grant"></a>Vhodné scénáře pro udělení implicitní OAuth2

Jako specifikace OAuth2 samotné deklaruje, má implicitní udělit navržen umožníte uživatelského agenta aplikací, to znamená JavaScript aplikace spouštění v prohlížeči. Definování charakteristikami tyto aplikace je kód v JavaScriptu je pro přístup k prostředkům serveru (obvykle webového rozhraní API) a příslušným způsobem aktualizace aplikace činnosti koncového uživatele. Představte si, že aplikace, jako je Gmail nebo Outlook Web Access: při vyberte zprávu ze složky Doručená pošta pouze panelu zpráv vizualizace změní zobrazíte nový výběr, zatímco zůstane změněna zbytku stránky. Toto je na rozdíl od tradiční na základě přesměrování Web apps, kde každý uživatel interakce má za následek zpětné volání celou stránku a vykreslení Úplná stránka Nový reakce serveru.

Aplikace, které potřebují JavaScript založeny přístup k jeho krajní nazývají jedné stránky aplikací ani SPA: myšlence je, že tyto aplikace sloužit pouze úvodní stránku HTML a související JavaScript, se všechny další interakcemi tak rozhraní API webových hovorů provedených prostřednictvím JavaScript. Však hybridní postupů, kdy aplikace je většinou zpětné volání řízené úsilím, ale provede občas JS hovory, nejsou běžné – informace o implicitním toku použití platí pro můžou být stejně.

Aplikace založené na přesměrování obvykle zabezpečený jejich požadavků prostřednictvím soubory cookie, ale, které postup nelze použít i pro JavaScript aplikace. Soubory cookie funkční pouze v doméně, které budou byly vytvořeny, zatímco JavaScript volání můžou být zaměřen na ostatních doménami. Kromě toho, které často bude v případě: Představte si, že aplikace vyvolání Microsoft Graph API rozhraní API Office Azure rozhraní API – všechny umístěných mimo doménu ze kdy je aplikace podávané množství. Rostoucího trendu JavaScript aplikací má mít žádný back-end vůbec, může 100 % 3 straně rozhraní API webových provádět obchodní funkci.

Upřednostňovaný způsob ochrany volání rozhraní API webových je v současné době OAuth2 nosný token možnost používat proto kde připojí přístupový token OAuth2 každého volání. Rozhraní API webových zkontroluje příchozí přístupový token a pokud najde je nezbytné obory, zajišťuje přístup k požadovanou operaci. Implicitní tok poskytuje mechanismus užitečné pro aplikace JavaScript získat přístup tokeny pro rozhraní API webových nabízí řadu výhod z hlediska soubory cookie:

- Tokeny problémy se spolehlivým získat bez nutnosti křížové origin hovory – povinnost zápisu přesměrování URI, ke kterému jsou tokeny zpáteční záruky, že nejsou posunut tokenů
- JavaScript aplikacím získat tolik tokeny přístupu jako potřebují, a pro tolik webového rozhraní API budou směrovat – bez omezení domén
- Funkce HTML5 jako relace nebo místní úložiště udělit úplnou kontrolu nad tokenu ukládání a správa životnost, že je Správa soubory cookie neprůhledné do aplikace
- Tokeny přístupu nejsou nebezpečí útoků padělání (CSRF) žádost o webů

Implicitní udělit tok není problém aktualizace tokeny převážně z bezpečnostních důvodů. Aktualizace token není omezené co přesně jako tokeny přístupu, poskytnutí mnohem více power tedy inflicting mnohem více poškození v případě, že je neuniknou. V implicitní toku tokeny doručované v adrese URL, tedy riziko zachycení vyšší než v udělení kód ověření.

Však, že aplikace JavaScript má jiný mechanismus k dispozici pro obnovení aplikace access tokeny bez opakovaně výzvy k zadání přihlašovacích údajů. Aplikaci můžete provádět skryté iframe nové tokenu požadavky týkající se tak mohli ověřovat koncový bod služby Azure AD: dokud prohlížeči stále obsahuje aktivní relace (číst: obsahuje soubor cookie relace) proti domain Azure AD požadavek na ověření úspěšně provést bez nutnosti interakci uživatele. 

Tento model umožňuje aplikaci JavaScript nezávisle na sobě obnovení tokeny přístupu a dokonce získat nové pro nové rozhraní API (za předpokladu, že uživatel dříve pro ně souhlas. To je zabráněno přidané zatížení získáním, Správa a ochrana artefaktem vysoká hodnota například token aktualizace. Artefakt, které umožní pasivní prodloužení cookie relace Azure AD spravují mimo aplikaci. Další výhodou tento přístup je, že uživatel může odhlásit se z Azure AD pomocí některé z aplikací přihlášeni k Azure AD spuštěné v některém z kartami prohlížeče. Výsledkem odstraňování souborů cookie Azure AD relace a aplikace JavaScript automaticky ztratí možnost prodloužit tokeny podepsané se uživateli.

## <a name="is-the-implicit-grant-suitable-for-my-app"></a>Je vhodné pro aplikace Moje udělení implicitní?

Udělení implicitní představuje další rizik než ostatní podpory. Oblasti potřebujete věnujte pozornost jsou popsány (viz příklad [Zneužití přístup Token vlastníkovi zosobnění zdroje implicitní tok] [ OAuth2-Spec-Implicit-Misuse] a [OAuth 2.0 hrozbou, že modelu a otázky bezpečnosti pro][OAuth2-Threat-Model-And-Security-Implications]). Vyšší profilu riziko je však převážně vzhledem k tomu, že je určen pro povolení aplikace, které spustit aktivní kód, podávané množství vzdálené zdrojem na prohlížeč. Pokud se chystáte architektura zabezpečené ověřování HESLA, bez back-end komponenty nebo chtěli vyvolat rozhraní API webových prostřednictvím JavaScript, doporučujeme použít implicitní směrování pro tokenu pořízení.

Pokud je nativní klient aplikace není implicitní tok skvělé přizpůsobit. Absence cookie relace Azure AD v souvislosti native client – zbaví aplikace střední zachování dlouho povahy relace. To znamená aplikace vyzve opakovaně uživatele při získání tokeny přístupu pro nové zdroje.

Pokud vyvíjíte webové aplikace, který obsahuje back-end a jinými rozhraní API z jeho back-end kódu, implicitní tok není také přesně hodí. Další příspěvky zobrazit mnohem více power. Například udělení OAuth2 klienta přihlašovacích údajů umožňuje získat klíčovými odrážela oprávnění přiřazená k aplikaci sebe sama namísto delegování uživatelů. To znamená, že má klient možnost Udržovat programové přístupu k prostředkům i když uživatel se nezabývá aktivně relace a tak dál. Kromě toho ale takové podpory získáte vyšší zabezpečení záruky. Například tokeny přístupu nikdy projít prohlížeči, není riziko ukládané do seznamu historie prohlížeče a tak dál. Klientské aplikaci můžete také ověřování silných při požadování token.

## <a name="next-steps"></a>Další kroky

- Úplný seznam vývojář zdrojů včetně referenční informace pro protokoly a OAuth2 se tak mohli ověřovat udělit toků nepodporuje Azure AD, odkažte [Azure AD pro vývojáře průvodce][AAD-Developers-Guide]
- Přečtěte si, [jak integrovat aplikaci Azure AD]  [ ACOM-How-To-Integrate] pro další název hloubkové v procesu integrace aplikace.

<!--Image references-->

<!--Reference style links in use-->
[AAD-Developers-Guide]: active-directory-developers-guide.md
[ACOM-How-And-Why-Apps-Added-To-AAD]: active-directory-how-applications-are-added.md
[ACOM-How-To-Integrate]: active-directory-how-to-integrate.md
[OAuth2-Spec-Implicit-Misuse]: https://tools.ietf.org/html/rfc6749#section-10.16 
[OAuth2-Threat-Model-And-Security-Implications]: https://tools.ietf.org/html/rfc6819

