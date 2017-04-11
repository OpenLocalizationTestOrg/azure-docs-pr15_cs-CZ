<properties
   pageTitle="Vytvoření aplikace, která se přihlásit všichni uživatelé Azure Active Directory | Microsoft Azure"
   description="Pokyny k vytváření aplikace, které můžete krok za krokem přihlásit uživatele z libovolné Azure Active Directory klienta, nazývaný také aplikace více klienta."
   services="active-directory"
   documentationCenter=""
   authors="skwan"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/11/2016"
   ms.author="skwan;bryanla"/>

# <a name="how-to-sign-in-any-azure-active-directory-ad-user-using-the-multi-tenant-application-pattern"></a>Jak se přihlásit všichni uživatelé Azure Active Directory (AD) pomocí aplikace více klienta vzorku
Pokud nabízejí softwarová jako aplikaci služby pro mnoho organizace můžete nakonfigurovat aplikaci přijmout přihlášení z libovolné Azure AD klienta.  V Azure AD jde o nastavování vašeho klienta více aplikací.  Se přihlásit k aplikaci po souhlas svůj účet pomocí aplikace budou mít uživatelé v libovolné Azure AD klienta.  

Pokud máte existující aplikaci, která obsahuje vlastní účet systému nebo podporuje jiné druhy přihlásit od jiných poskytovatelů cloudu, přidání Azure AD přihlášení z libovolné klienta je jednoduchá – stačí registraci aplikace, přidání znak v kódu prostřednictvím OAuth2 OpenID připojení nebo SAML a přepnutím přihlásit se tlačítko do aplikace. Kliknutím na tlačítko Další informace o branding aplikace.

[! [Přihlášení tlačítko] [AAD – přihlašovací]][AAD-App-Branding]


Tento článek předpokládá, že jste už znáte vytváření aplikace jednoho klienta pro Azure AD.  Pokud nejste, vedoucí zpátky do [domovské stránce průvodce vývojář] [ AAD-Dev-Guide] a proveďte jednu z našich Snadné spuštění.

Existují čtyři jednoduché kroky pro převedení aplikace do aplikace pro Azure AD více klienta:

1.  Aktualizace aplikace registrace být více klienta
2.  Aktualizace kódu k odeslání žádosti o/Common vložíte koncový bod 
3.  Aktualizace kódu zpracovávání více hodnot vydavatel
4.  Princip souhlas uživatel a správce a proveďte příslušný kód změny

Podívejme se na každém kroku podrobně. Můžete taky přejít přímo do [seznamu nepromítnou více klienta vzorků][AAD-Samples-MT].

## <a name="update-registration-to-be-multi-tenant"></a>Aktualizovat registraci být více klienta
Ve výchozím nastavení jsou webové aplikace a rozhraní API registrace v Azure AD jednoho klienta.  Můžete vytvořit registrace více klientovi pomocí hledání "Aplikace je více klienta" přepnout na stránce konfigurace registrace aplikace v [Azure klasické portál] [ AZURE-classic-portal] a nastavíte na "Ano".

Poznámka: Před aplikace více klienta, vyžaduje Azure AD URI ID aplikace aplikace globálně jedinečná. Identifikátor URI aplikace je jedním ze způsobů, jak aplikace podle protokolu zprávy.  Aplikace jednoho klienta stačí pro identifikátor URI ID aplikace být jedinečný v rámci tohoto klienta.  Více klienta aplikace musí být jedinečné Azure AD našli aplikace přes všechny klienty.  Globální jedinečnost nevynucují vyžadováním URI ID aplikace mít odpovídající ověřenou doménou klienta Azure AD název hostitele.  Například pokud byl název vašeho klienta contoso.onmicrosoft.com pak platný URI ID aplikace by `https://contoso.onmicrosoft.com/myapp`.  Pokud vašemu klientovi bylo ověřenou doménou z `contoso.com`, pak platný identifikátor URI ID aplikace by také `https://contoso.com/myapp`.  Nastavení aplikace jako více klienta selže, pokud Identifikátor URI aplikace není podle tohoto vzorku.

Nativní klientský registrací jsou více klienta ve výchozím nastavení.  Nemusíte provádět žádnou akci provádět nativní aplikaci registrace více klienta.

## <a name="update-your-code-to-send-requests-to-common"></a>Aktualizace kódu k odeslání žádosti o/Common
V aplikaci jednoho klienta přihlásit žádosti o posílají klienta přihlásit koncového bodu.   Například pro contoso.onmicrosoft.com budou koncového bodu:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

Odeslané ke klientovi koncový bod žádosti o přihlášení můžou uživatelé (nebo hostů) v tomuto klientovi aplikace v tomuto klientovi.  Pomocí aplikace více klienta aplikace poslal, neví, předem jaké klienta je uživatel, tak, aby vám neodesílají žádosti o koncový bod ke klientovi.  Místo toho se odešle žádost o koncového multiplexes přes všechny Azure AD klienti:

    https://login.microsoftonline.com/common

Jakmile Azure AD obdrží žádost o/Common vložíte koncový bod, uživatel přihlásí a v důsledku zjistí, které uživatel pochází z klienta.  / Běžné koncový bod funguje se všemi protokolů ověřování podporovaných Azure AD: OpenID připojení OAuth 2.0, SAML 2.0 a WS Federation.

Přihlášení do aplikace pak odpověď obsahuje token představující uživatele.  Hodnotu Vystavitel v tokenu říká aplikace jaké uživatel pochází z klienta.  Pokud odpověď vrátí/Common vložíte koncový bod, odpovídat hodnotu Vystavitel v tokenu uživatele klienta.  Je důležité, je potřeba pamatovat / Common koncový bod není klienta a není vydavatel, je právě multiplexor.  Při použití/Common, musí logickou v aplikaci pro ověřování tokeny aktualizuje tak, aby vzít v úvahu. 

Jak jsme zmínili dříve, více klienta aplikace poskytuje rovněž jednotné přihlašování v setkat i v případě uživatelům Azure AD žádosti branding pokyny. Kliknutím na tlačítko Další informace o branding aplikace.

[! [Přihlášení tlačítko] [AAD – přihlašovací]][AAD-App-Branding]

Podívejme se na použití/Common vložíte koncového bodu a implementaci kódu podrobněji.

## <a name="update-your-code-to-handle-multiple-issuer-values"></a>Aktualizace kódu zpracovávání více hodnot vydavatel
Webových aplikací web rozhraní API přijímání a ověřte tokenů z Azure AD.  

> [AZURE.NOTE] Nativní klientské aplikace požadovat a přijímat tokenů z Azure AD, přehrávají tak jim můžete poslat rozhraní API, kde jsou ověřeny.  Nativní aplikace není ověřte tokeny a musí považovat za neprůhledné.

Pojďme se podívat na způsob, jakým aplikace ověřuje tokeny dostává z Azure AD.  Aplikace jednoho klienta obvykle potrvá, například hodnota koncového bodu:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

a použijte ji k vytvoření adresa URL metadat (v tomto případě připojení OpenID) jako:

    https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration

Pokud si chcete stáhnout dva zásadní informace, které se používají k ověření tokeny: klient podpisu klíče a Vystavitel hodnotu.  Každého klienta Azure AD má hodnotu jedinečné Vystavitel formuláře:

    https://sts.windows.net/31537af4-6d77-4bb9-a681-d2394888ea26/

kde GUID hodnotu XLL s podporou přejmenovat verzi klienta ID klienta.  Když kliknete na metadat odkaz nad `contoso.onmicrosoft.com`, zobrazí se tato hodnota Vystavitel v dokumentu.

Když aplikace jednoho klienta ověří token, zkontroluje podpis token proti podpisový kláves sady dokumentů metadat a zajišťuje, aby že vystavitel hodnota v tokenu odpovídá ten, který byl součástí dokument metadat.

Od/Common vložíte koncový bod neodpovídá klienta a není vystavitel, když si prohlédnete hodnotu Vystavitel metadat pro/běžné má šablon adresy URL místo skutečné hodnoty:

    https://sts.windows.net/{tenantid}/

Proto aplikace více klienta nemůže ověřit tokeny pouhým odpovídající hodnotě Vystavitel v metadatech se `issuer` hodnotu v tokenu.  Aplikace více klienta musí logiky rozhodnout Vystavitel hodnot, které jsou platné a které je není, založený na klientovi ID část Vystavitel hodnoty.  

Například pokud více klienta aplikace umožňuje přihlásit pouze z konkrétní klienti, kteří se zaregistrují pro své služby, ho musí zkontrolujte hodnotu Vystavitel nebo `tid` nárok na hodnotu v token musí být tomuto klientovi v jejich seznamu účastníků.  Pokud aplikace více klienta pouze se zabývá osobám a nemá rozhodování všechny přístupu na základě klienti, potom ho můžete ignorovat hodnotu Vystavitel úplně odebrat.

Ve vzorcích více klienta, který najdete v části [Související obsah](#related-content) na konci tohoto článku je vypnutá, Vystavitel ověření povolit jakékoli Azure AD klienta se přihlásit.

Teď Pojďme se podívat na uživatelské prostředí pro uživatele, kterým se přihlašujete k aplikacím více klienta.

## <a name="understanding-user-and-admin-consent"></a>Principy uživatel a správce souhlas
Pro uživatele se přihlásit k aplikaci v Azure AD musíte aplikace reprezentované v klientovi uživatele.  Tato funkce umožňuje organizaci takové věci, jako používání jedinečných zásad při přihlášení k aplikaci uživatelé z jejich klienta.  Pro použití jednoho klienta je tato registrace jednoduché. je ten, který se stane při registraci aplikace v [Azure klasické portál][AZURE-classic-portal].

Pro aplikace více klienta počáteční registrace pro aplikaci jsou umístěná v klientovi Azure AD používaný vývojář.  Když uživatel z různých klienta přihlásí k žádosti o poprvé, Azure AD zeptá, aby souhlas s oprávnění požaduje aplikace.  Pokud jsou souhlas, vytvoří na číselné aplikace se nazývá *služby základní* v klientovi uživatele a mohli znovu přihlásit. Delegování je také vytvořit v adresáři, který uchovává uživatele souhlas s aplikací. Viz [aplikace a služby jistinu objekty] [ AAD-App-SP-Objects] podrobné informace o aplikace aplikace a ServicePrincipal objekty a jak se vztahují k sobě.

![Souhlas s jednoúrovňových aplikace][Consent-Single-Tier] 

Tento souhlas prostředí je ovlivněna oprávnění požaduje aplikace.  Azure AD podporuje dva typy oprávnění jen aplikace a delegované:

- Delegovaná oprávnění uděluje aplikace, které můžete dělat s nimi pracovat jako přihlášený uživatel pro podmnožinu věci uživatele.  Můžete například udělit aplikace delegované oprávnění ke čtení Přihlášený uživatel kalendáře.
- Jen aplikace je povoleno přímo k identifikaci aplikace.  Například aplikaci můžete udělit oprávnění jen aplikace zobrazíte seznam uživatelů na klienta a budou moct provést bez ohledu na to, kdo je přihlášený k aplikaci.

Některá oprávnění můžete být souhlas běžný uživatel, zatímco jiné vyžadují souhlas správce klienta. 

### <a name="admin-consent"></a>Správce souhlas
Aplikaci – oprávnění jen vždy vyžadují souhlas správce klienta.  Pokud vaše aplikace požaduje jen aplikace oprávnění a normální při pokusu o přihlášení k aplikaci, aplikace zobrazí chybová zpráva oznamující, že uživatel nemůže připojit k souhlas.

Některé delegované oprávnění vyžadují navíc souhlas správce klienta.  Například možnost odepsat Azure AD jako přihlášený uživatel vyžaduje souhlas správce klienta.  Jako aplikaci – oprávnění jen pokud běžného uživatele se pokouší přihlásit do aplikace, která vyžádá delegované oprávnění, která vyžaduje souhlas správce aplikace dojde k chybě.  Zda vyžaduje oprávnění správce souhlas, je určený podle vývojář, který publikované zdroje a může být najdete v dokumentaci k zdroje.  Odkazy na témata popisující dostupná oprávnění pro rozhraní API Azure AD grafu a rozhraní API aplikace Microsoft Graph jsou v části [Související obsah](#related-content) tohoto článku.

Pokud vaše aplikace používá oprávnění, které vyžadují správce souhlas, musíte mít gesto v aplikaci například tlačítka nebo odkaz, kde může správce zahájit akce.  Žádost o aplikaci odešle tato akce je obvykle OAuth2/OpenID připojení se tak mohli ověřovat žádost, ale, která obsahuje taky `prompt=admin_consent` parametru řetězce dotazu.  Jakmile správce souhlasí a jistinu služby vytvořené v tenant zákazníka, další znak v žádosti o nemusí `prompt=admin_consent` parametr.   Protože rozhodnout, že jsou přijatelné požadovaná oprávnění správce vyzváni žádní jiní uživatelé v klientovi pro souhlas od tohoto místa dále.

`prompt=admin_consent` Parametru lze také tak, že aplikace, které vyžadují oprávnění, které není nutné zadávat správce souhlas, ale chcete dát prostředí, kde správce klienta "zaregistruje" aplikace jednorázové a žádní jiní uživatelé se zobrazí výzva k souhlasu od tohoto okamžiku na.

Pokud aplikace vyžaduje souhlas správce a správce přihlásí k aplikaci ale `prompt=admin_consent` parametr neodešle, správce budou moct úspěšně souhlas s aplikací, ale bude pouze svolení s svůj uživatelský účet.  Běžná uživatelé pořád nezobrazí se přihlásit a souhlas s aplikací.  To je užitečné, pokud chcete umožnit správce klienta zkoumání aplikace před umožněním jiným uživatelům přístup.

Správce klienta můžete zakázat běžná uživatelům souhlas s aplikací.  Pokud je tato možnost je zakázané, správce souhlas vždycky je nutný pro aplikaci nastavit v klientovi.  Pokud chcete testovat aplikace s běžný uživatel souhlas zakázané, můžete najít přepínačem konfigurace v klientovi Azure AD konfigurace část [Azure klasické portál][AZURE-classic-portal].

> [AZURE.NOTE] Některé aplikace má prostředí běžná uživatelé budou moct původně souhlas, kde později aplikace, bude to vyžadovat oprávnění správce a žádost, které vyžadují souhlas správce.  Nejde žádným způsobem akce se registrace jednoho aplikace v Azure AD dnes.  Nadcházející koncový bod v2 Azure AD vám umožní aplikací a požádat o oprávnění za běhu, ne v době registrace, které vám umožní tento scénář.  Další informace najdete v tématu [Průvodce vývojář v2 Model aplikací Azure AD][AAD-V2-Dev-Guide].

### <a name="consent-and-multi-tier-applications"></a>Souhlas a vícevrstvého aplikací
Aplikace může mít více úrovní, každý představované vlastní registrace v Azure AD.  Například nativní aplikaci, která volá rozhraní API webových nebo spuštění webové aplikace, který volá webového rozhraní API.  V obou případech žádosti klienta (nativní aplikaci nebo web app) oprávnění pro volání zdroje (webového rozhraní API).  Klient se úspěšně souhlas do zákazníků klienta všechny zdroje, ke kterým je vyžaduje oprávnění, musí existovat v tenant zákazníka.  Pokud není splněná tato podmínka, Azure AD vrátí chybu, že zdroje nejdříve je potřeba přidat.

Pokud logické aplikace se skládá ze dvou nebo více aplikací registrace, například samostatné klienta a zdroje to může být problém.  Jak můžete získat zdroje do klienta zákazníka první?  Azure AD popisuje tento případ povolením klienta a zdroje souhlas v jednom kroku, kdy uživatel vidí celkový součet oprávnění požadovaná mezi klientem a zdroje na stránce souhlas.  Chcete-li toto chování, musí obsahovat registrace aplikace zdroje ID aplikace klienta jako `knownClientApplications` v manifestu aplikace.  Příklad:

    knownClientApplications": ["94da0930-763f-45c7-8d26-04d5938baab2"]

Tato vlastnost je možné aktualizovat prostřednictvím prostředku [manifestu aplikace][AAD-App-Manifest]a je znázorněn v vícevrstvého Nativní klient volání ukázkové webového rozhraní API v části [Související obsah](#related-content) na konci tohoto článku. Následující diagram přehled souhlas aplikace vícevrstvého:

![Souhlas s vícevrstvého známé klient aplikace][Consent-Multi-Tier-Known-Client] 

Podobně jako případu se stane, když různých úrovní aplikace jsou registrované v různých klientů.  Zvažte například v případě vytváření native client – aplikace, která volá Office 365 Online rozhraní API systému Exchange.  Se dají nativní aplikaci a novější nativní aplikace spustit v klientovi zákazníků, musí obsahovat jistinu služby Exchange Online.  V tomto případě zákazník musí koupit služby základní byly vytvořeny v jejich klienta Exchange Online.  V případě rozhraní API integrované organizací než Microsoft developer rozhraní API musí umožňují svým zákazníkům souhlas jejich aplikace do zákazníka klienta, třeba na webovou stránku, která jednotky souhlas pomocí mechanismy popisované v tomto článku.  Po vytvoření hlavního uživatele služby v klientovi nativní aplikaci můžete získat tokeny rozhraní API.

Následující diagram přehled souhlas aplikace vícevrstvého registraci v různých klienti:

![Souhlas s vícevrstvého více aplikací][Consent-Multi-Tier-Multi-Party] 

### <a name="revoking-consent"></a>Odvolání souhlas
Souhlas aplikaci kdykoli můžete odvolat uživatele a správce:

- Uživatelé odvolat přístup pro jednotlivé aplikace tím, že odeberete jejich [Panely aplikace Access] [ AAD-Access-Panel] seznamu.
- Správci odvolat přístup k aplikacím tím, že odeberete z Azure AD pomocí oddílu Správa Azure AD [Azure klasické portál][AZURE-classic-portal].

Pokud správce souhlasí do aplikace pro všechny uživatele na klientovi, uživatelé nejde odvolat přístup jednotlivě.  Jen správce můžete odvolat přístup a jenom pro celou aplikaci.

### <a name="consent-and-protocol-support"></a>Souhlas podporu a protokolů
V Azure AD pomocí OAuth OpenID připojit, je podporovaná souhlas WS federace a SAML protokoly.  Protokoly SAML a WS Federation nepodporují `prompt=admin_consent` parametr, tak správce souhlas, je možné pouze prostřednictvím OAuth a OpenID připojení.

## <a name="multi-tenant-applications-and-caching-access-tokens"></a>Více klienta aplikací a ukládání do mezipaměti tokeny přístupu
Více klienta aplikace otevřete taky tokeny přístup k rozhraní API, které jsou chráněny Azure AD volat.  Běžné chyby při Active Directory ověřování knihovny (ADAL) pomocí aplikace více klienta původně požádat o token uživatelem, který používá/Common, dostanete odpověď a potom požádat o další token tomuto uživateli taky používá/Common.  Protože odpověď z Azure AD zobrazuje, pochází z klienta, není/běžné, ADAL ukládání do mezipaměti token jako z klienta. Pozdější volání/Common zobrazíte přístupový token pro uživatele neúspěšných přístupů do položky uložené v mezipaměti a uživatel bude muset se znovu přihlaste.  Chcete-li předejít chybějící mezipaměti, zkontrolujte, že následující již přihlášeni uživateli volání koncový bod klienta.

## <a name="related-content"></a>Související obsah

- [Použití více klienta vzorky][AAD-Samples-MT]
- [Branding pokyny pro aplikace][AAD-App-Branding]
- [Azure AD Developer Guide][AAD-Dev-Guide]
- [Aplikace a služby jistinu objekty][AAD-App-SP-Objects]
- [Integrace aplikace s Azure Active Directory][AAD-Integrating-Apps]
- [Základní informace o souhlas Framework][AAD-Consent-Overview]
- [Aplikace Microsoft Graph rozhraní API oprávnění oborů][MSFT-Graph-AAD]
- [Azure AD grafu rozhraní API oprávnění oborů][AAD-Graph-Perm-Scopes]

Stiskněte klávesovou zkratku Disqus komentáře v části poskytnutí zpětné vazby a Pomozte nám vylepšit a přizpůsobovat naše obsah.

<!--Reference style links IN USE -->
[AAD-Access-Panel]:  https://myapps.microsoft.com
[AAD-App-Branding]: ./active-directory-branding-guidelines.md
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Consent-Overview]: ./active-directory-integrating-applications.md#overview-of-the-consent-framework
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Overview]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-graph-api/
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Samples-MT]: https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multitenant
[AAD-Why-To-Integrate]: ./active-directory-how-to-integrate.md
[AZURE-classic-portal]: https://manage.windowsazure.com
[MSFT-Graph-AAD]: https://graph.microsoft.io/en-us/docs/authorization/permission_scopes

<!--Image references-->
[AAD-Sign-In]: ./media/active-directory-devhowto-multi-tenant-overview/sign-in-with-microsoft-light.png
[Consent-Single-Tier]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-single-tier.png
[Consent-Multi-Tier-Known-Client]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-known-clients.png
[Consent-Multi-Tier-Multi-Party]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-multi-party.png

<!--Reference style links -->
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: ./active-directory-token-and-claims.md
[AAD-V2-Dev-Guide]: ./active-directory-appmodel-v2-overview.md
[AZURE-classic-portal]: https://manage.windowsazure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[O365-Perm-Ref]: https://msdn.microsoft.com/en-us/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Code-Grant-Flow]: https://msdn.microsoft.com/library/azure/dn645542.aspx
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3 
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken














