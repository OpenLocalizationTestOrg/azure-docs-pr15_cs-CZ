<properties
   pageTitle="Glosář služby Azure Active Directory vývojář | Microsoft Azure"
   description="Seznam podmínek pro běžně používaných konceptů vývojář Azure Active Directory a funkcí."
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/31/2016"
   ms.author="bryanla"/>

# <a name="azure-active-directory-developer-glossary"></a>Glosář vývojář Azure Active Directory
Tento článek obsahuje definice pro některé základní konceptů vývojář Azure Active Directory (AD), které jsou užitečné, pokud získání informací o vývoji aplikací pro Azure AD.

## <a name="access-token"></a>přístupový token
Typ [tokenu zabezpečení](#security-token) vystavil [se tak mohli ověřovat serveru](#authorization-server)a [klientské aplikaci](#client-application) použít pro přístup k [chráněné prostředků serveru](#resource-server). Obvykle ve formátu [JSON Web tokenu (JWT)][JWT], tokenu ztělesňuje vydaný klientovi [vlastníka zdroje](#resource-owner)pro požadovanou úroveň přístupu. Token obsahuje použitelná [deklarované](#claim) o předmětu povolení klientské aplikaci můžete ho jako formulář pověření při přístupu k daného zdroje. Také nemusí vlastníka zdroje vystavit přihlašovací údaje ke klientovi.

Tokeny přístupu jsou někdy označovány jako "Uživatele + aplikace" nebo "Aplikace jen", v závislosti na zastupování přihlašovací údaje. Když klientské aplikaci například používá:

- [Povolení "Se tak mohli ověřovat kód" udělit](#authorization-grant), koncový uživatel se ověří nejdřív jakožto vlastníka zdroje, delegování se tak mohli ověřovat ke klientovi pro přístup k prostředku. Klient ověřuje později při získání přístupový token. Token může někdy být uvedená konkrétněji jako token "Uživatele + aplikace" představuje obou uživatele, který oprávnění klientské aplikace a aplikace.
- [Udělit povolení "Pověření klienta"](#authorization-grant), klienta obsahuje jediný ověřování fungování bez vlastníka zdroje ověřování/se tak mohli ověřovat, takže tokenu může být někdy označovaný taky jako token "Jen aplikace".

Přečtěte si článek Principy [Azure AD tokenu] [ AAD-Tokens-Claims] další podrobnosti.

## <a name="application-manifest"></a>manifestu aplikace  
Funkce poskytované [Azure klasické portál][AZURE-classic-portal], produkuje formátovanými jako JSON identity konfigurace aplikace, používá jako mechanismus při aktualizaci přidružená [aplikace] [ AAD-Graph-App-Entity] a [ServicePrincipal] [ AAD-Graph-Sp-Entity] entity. Najdete v článku [Principy manifest aplikace služby Azure Active Directory] [ AAD-App-Manifest] další podrobnosti.

## <a name="application-object"></a>objekt aplikace  
Kdy se rejstříku/aktualizace aplikace [Azure klasické portál][AZURE-classic-portal], portálu vytvoří/zavedli objekt aplikace a odpovídající [objekt služby](#service-principal-object) tomuto klientovi. Aplikační objekt *definuje* aplikace je identity konfigurace globálně (přes všechny klienty místo, kam má access), poskytuje šablony odkud odpovídající hlavní objekty služby jsou *odvozeny* pro použití místně za běhu (v určitých klienta).

V části [aplikace a služby jistinu objektů] [ AAD-App-SP-Objects] Další informace.

## <a name="application-registration"></a>registrace aplikace  
Pokud chcete povolit aplikaci integrovat a delegování identit a správu přístupu funkcí Azure AD, musí být registrován u Azure AD [klienta](#tenant). Při registraci aplikace s Azure AD zadání identity konfigurace aplikace, povolením a integrace s Azure AD pomocí funkcí jako:

- Robustní správu z jednotné přihlašování pomocí služby Azure AD Správa identit a [Připojte OpenID] [ OpenIDConnect] implementaci Protocol (protokol)
- Brokered přístup k [chráněné zdrojů](#resource-server) v [klientských aplikacích](#client-application), prostřednictvím implementace [se tak mohli ověřovat server](#authorization-server) Azure AD OAuth 2.0
- [Svolení framework](#consent) pro správu klientského přístupu k chráněným prostředkům podle autorizaci vlastníka zdroje.

Podívejte se na [aplikace integrace se službou Azure Active Directory] [ AAD-Integrating-Apps] další podrobnosti.

## <a name="authentication"></a>ověřování
Příznaky náročné strany legitimní přihlašovací údaje poskytující základ pro vytvoření jistiny zabezpečení pro identitu a řízení přístupu. Během [OAuth2 se tak mohli ověřovat udělit](#authorization-grant) například je stran ověřování vyplňování role [zdroje vlastník](#resource-owner) nebo [klientské aplikaci](#client-application), podle toho, poskytnutí použít.

## <a name="authorization"></a>povolení
Příznaky udělení hlavní oprávnění ověřené zabezpečení něco udělat. Model programování Azure AD existují dva případy primární použití:

- Během toku [OAuth2 se tak mohli ověřovat udělit](#authorization-grant) : když [vlastníka zdroje](#resource-owner) udělí povolení ke [klientské aplikaci](#client-application), povolení klientovi přístup k prostředku vlastníka zdroje.
- Při přístupu k prostředkům klientem: prováděn [prostředků serveru](#resource-server)pomocí [převzetí](#claim) hodnoty prezentovat v [přístupový token](#access-token) rozhodovat přístup ovládací prvek na jejich základě.

## <a name="authorization-code"></a>povolení kódu
Krátké životnost "token" poskytovanou na [klientské aplikace](#client-application) [koncový bod se tak mohli ověřovat](#authorization-endpoint)jako součást tok "se tak mohli ověřovat kód" jednu ze čtyř OAuth2 [uděluje se tak mohli ověřovat](#authorization-grant). Kód je vrácena klientské aplikace v odpověď ověřování [vlastníka zdroje](#resource-owner), označovat tak, že vlastníka zdroje delegoval se tak mohli ověřovat a získat požadované informace. V rámci toku kód později uplatnili [přístupový token](#access-token).

## <a name="authorization-endpoint"></a>koncový bod se tak mohli ověřovat
Koncové body implementovaná [se tak mohli ověřovat server](#authorization-server)slouží k interakci se na [vlastníka zdroje](#resource-owner) k poskytování [se tak mohli ověřovat udělit](#authorization-grant) během povolení OAuth2 udělit toku. V závislosti na grant (udělit) se tak mohli ověřovat tok použili se může lišit skutečné udělit k dispozici, včetně [se tak mohli ověřovat kód](#authorization-code) nebo [tokenu zabezpečení](#security-token).

Najdete v článku specifikace OAuth2 [se tak mohli ověřovat udělit typy] [ OAuth2-AuthZ-Grant-Types] a [koncový bod se tak mohli ověřovat] [ OAuth2-AuthZ-Endpoint] oddíly a [OpenIDConnect specifikace] [ OpenIDConnect-AuthZ-Endpoint] další podrobnosti.

## <a name="authorization-grant"></a>povolení grant (udělit)
Přihlašovací údaje představující [vlastníka zdroje](#resource-owner) [se tak mohli ověřovat](#authorization) pro přístup k jeho chráněným prostředkům udělil [klientské aplikaci](#client-application). Klientské aplikaci můžete použít jednu [čtyři typy grant (udělit) definovaného Framework se tak mohli ověřovat OAuth2] [ OAuth2-AuthZ-Grant-Types] získat grant (udělit), v závislosti na typu/požadavků klienta: "se tak mohli ověřovat kód grant (udělit)", "pověření klienta udělit", "implicitní grant (udělit)" a "pověření heslo vlastníka zdroje udělit". Přihlašovací údaje do klienta je buď [přístupový token](#access-token), nebo [se tak mohli ověřovat kód](#authorization-code) (které jste si vyměnili dál pro přístupový token), v závislosti na typu se tak mohli ověřovat udělit použít.

## <a name="authorization-server"></a>Povolení serveru
Podle [OAuth2 se tak mohli ověřovat Framework][OAuth2-Role-Def], zodpovědný za vydání přístup serveru tokeny [Klient](#client-application) po úspěšném ověření [vlastníka zdroje](#resource-owner) a získání jeho povolení. Interakce [klientské aplikaci](#client-application) se serverem se tak mohli ověřovat pomocí [se tak mohli ověřovat](#authorization-endpoint) za běhu a [tokenu](#token-endpoint) koncové body v souladu s OAuth2 definované [uděluje se tak mohli ověřovat](#authorization-grant).

V případě integraci aplikace Azure AD Azure AD implementuje roli se tak mohli ověřovat server Azure AD aplikací a služeb Microsoft rozhraní API, například [Microsoft Graph API][Microsoft-Graph].

## <a name="claim"></a>deklarace
[Tokenu zabezpečení](#security-token) obsahuje deklarací, které poskytují tvrzení o jedné entity (jako je [klientská aplikace](#client-application) nebo [vlastníka zdroje](#resource-owner)) jiné osobě (například [prostředků serveru](#resource-server)). Deklarace jsou název/dvojice, které převádět informace o tokenu předmět (například objekt zabezpečení, který byl ověřen [se tak mohli ověřovat serveru](#authorization-server)). Nároky prezentovat v dané tokenu jsou závislé na několik proměnných, včetně typu token, typ pověření slouží k ověření předmět konfigurace aplikace, atd.

Přečtěte si článek Principy [Azure AD tokenu] [ AAD-Tokens-Claims] další podrobnosti.

## <a name="client-application"></a>klientské aplikace  
Podle [OAuth2 se tak mohli ověřovat Framework][OAuth2-Role-Def], aplikace, která zajišťuje chráněné požadavky zdroje jménem [vlastníka zdroje](#resource-owner). Termín "klienta" neznamená jakékoli vlastnosti implementaci konkrétní hardwarovou (například jestli aplikace spustí na serveru, ploše nebo jiná zařízení).  

Klientské aplikaci požaduje [se tak mohli ověřovat](#authorization) od vlastníka zdroje k účasti v toku [udělit OAuth2 autorizace](#authorization-grant) a mohou získat přístup k rozhraní API/datům jménem vlastníka zdroje. OAuth2 se tak mohli ověřovat Framework [definuje dva typy klientů][OAuth2-Client-Types]"důvěrné" a "veřejné", podle klienta možnost Udržovat důvěrnost své přihlašovací údaje. Aplikace můžete provádět [webový klient (důvěrné)](#web-client) , který běží na webový server [native client – (veřejné)](#native-client) nainstalovaný na zařízení nebo [na základě uživatelského agenta klienta (veřejné)](#user-agent-based-client) , který běží v prohlížeči mobilního zařízení.

## <a name="consent"></a>svolení
Proces [vlastníka zdroje](#resource-owner) registraci na [klientské aplikace](#client-application), konkrétní [oprávnění](#permissions) k přístupu k chráněným prostředkům jménem vlastníka zdroje. V závislosti na oprávněních požadovaných klientem správce nebo uživatel bude požádán, aby souhlas přístupu k jejich organizace/jednotlivé data v tomto pořadí. Všimněte si ve scénáři [více klienta](#multi-tenant-application) že aplikace [služby základní](#service-principal-object) nastavené také v klientovi consenting uživatele.

## <a name="id-token"></a>ID tokenu
[Připojení OpenID] [ OpenIDConnect-ID-Token] [tokenu zabezpečení](#security-token) poskytovanou [serveru se tak mohli ověřovat](#authorization-server) [koncový bod se tak mohli ověřovat](#authorization-endpoint), která obsahuje [deklarací](#claim) vztahující se k ověření koncový uživatel [vlastníka zdroje](#resource-owner). Jako přístupový token, ID tokeny jsou také tvaru digitálně podepsanou [JSON Web tokenu (JWT)][JWT]. Na rozdíl od přístupový token však ID token deklarací nepoužívaná pro účely týkající se přístupu k prostředkům a konkrétně řízení přístupu.

Přečtěte si článek Principy [Azure AD tokenu] [ AAD-Tokens-Claims] další podrobnosti.

## <a name="multi-tenant-application"></a>použití více klienta
Přihlaste se třídy [klientské aplikaci](#client-application) , která umožňuje a [souhlas](#consent) uživatelé zřízení v libovolné Azure AD [klienta](#tenant), včetně klientů než tu, kde je zaregistrovaná klienta. Naopak aplikace registrovaná jako jednoho klienta, pouze umožní přihlášení z uživatelských účtů zřízení ve stejném klientovi jako ten, kde je aplikace zaregistrovaná. [Nativní klientské](#native-client) aplikace jsou více klienta ve výchozím nastavení, zatímco [webový klient](#web-client) aplikace mít možnost zvolit mezi jednoho a více klienta.

Přečtěte si, [jak se přihlásit všichni uživatelé Azure AD pomocí aplikace více klienta vzorku] [ AAD-Multi-Tenant-Overview] další podrobnosti.

## <a name="native-client"></a>Native client –
Typ [klientské aplikaci](#client-application) , která je nativně nainstalovaný na zařízení. Protože všechny kód se spustí na zařízení, nebude považována za "veřejné" klienta kvůli společnosti ukládání přihlašovacích údajů soukromě/důvěrně. Přečtěte si téma [typy klientů OAuth2 a profily] [ OAuth2-Client-Types] další podrobnosti.

## <a name="permissions"></a>oprávnění
[Klientská aplikace](#client-application) získá přístup k [prostředků serveru](#resource-server) deklarací žádosti o oprávnění. Existují dva typy:

- "Delegované" oprávnění, která vyžádání přístupu [na základě obor](#scopes) klikněte v části delegované se tak mohli ověřovat od přihlášený [vlastníka zdroje](#resource-owner), jsou uvedeny tomuto zdroji za běhu jako ["spojovací bod služby" nároky](#claim) v klienta [přístupový token](#access-token).
- "Aplikace" oprávnění, která vyžádání přístupu [na základě rolí](#roles) přihlašovací údaje/identitou klientské aplikaci, jsou vyjádřená pomocí tomuto zdroji za běhu [deklarace "role"](#claim) v klienta přístupový token.

Jsou taky povrchový v průběhu [souhlas](#consent) pojmenování správce nebo vlastník zdroje příležitosti grant (udělit) nebo odepřít přístup klienta k prostředkům ve své klienta.

Žádosti o oprávnění jsou nakonfigurovány "Aplikace" / "Konfigurace" kartu [Azure klasické portál][AZURE-classic-portal], v části "Oprávnění pro ostatní aplikace", tak, že vyberou požadovaná "delegovat oprávnění" a "Oprávnění aplikace" (potřebuje členství v roli globálního správce). Protože [veřejné klienta](#client-application) nelze zachovat přihlašovací údaje, ji můžete jenom požádat o delegované oprávnění, zatímco [důvěrné klienta](#client-application) má možnost žádost o obou delegované a oprávnění aplikace. Klienta [objekt aplikace](#application-object) ukládá deklarované oprávnění v jeho [vlastnost requiredResourceAccess][AAD-Graph-App-Entity].

## <a name="resource-owner"></a>vlastníka zdroje
Podle [OAuth2 se tak mohli ověřovat Framework][OAuth2-Role-Def], může udělení přístupu k chráněného zdroj entity. Je-li vlastníka zdroje osoby, ji se nazývá koncového uživatele. Třeba když [klientské aplikaci](#client-application) vyžaduje přístup k poštovní schránky uživatele pomocí rozhraní [API aplikace Microsoft Graph][Microsoft-Graph], vyžaduje oprávnění vlastníka zdroje poštovní schránky.

## <a name="resource-server"></a>prostředků serveru
Podle [OAuth2 se tak mohli ověřovat Framework][OAuth2-Role-Def], serveru hosts chráněném zdrojů, může přijímání zpráv a reagování na chráněné požadavky zdroje na [klientské aplikace](#client-application) prezentovat [tokenu přístupu](#access-token). Označovaná taky jako chráněného zdroj serveru nebo použití zdrojů.

Server zdroje poskytuje rozhraní API a zajišťuje přístup k jeho chráněné prostředky prostřednictvím [obory](#scopes) a [role](#roles), pomocí rozhraní OAuth 2.0 se tak mohli ověřovat. Jako příklad lze uvést rozhraní API Azure AD grafu, které poskytuje přístup k datům Azure AD klienta a rozhraní API Office 365, která umožňují přístup k datům například pošta a kalendář. Obě tyto jsou k dispozici prostřednictvím rozhraní [API aplikace Microsoft Graph][Microsoft-Graph].  

Stejně jako klientské aplikaci konfigurace identity zdrojů aplikace je založenou na [Registrace](#application-registration) v klientovi Azure AD poskytuje aplikace a služby objekt. Některé poskytované společností Microsoft rozhraní API, například rozhraní API Azure AD grafu předem zaregistrovali k dispozici ve všech klienti během vytváření objektů služby.

## <a name="roles"></a>role
Například [obory](#scopes)role umožňují [prostředků serveru](#resource-server) k řízení přístupu k chráněným prostředkům. Existují dva typy: role "uživatel" implementuje řízení přístupu na základě rolí pro uživatele nebo skupiny, které vyžadují přístup k tomuto prostředku, zatímco role "aplikace" implementuje stejný pro [klientské aplikace](#client-application) , která vyžadují přístup.

Role jsou definované zdroje řetězce (například "výdajů schvalovatel", "Jen pro čtení", "Directory.ReadWrite.All") spravovaných [Azure klasické portál] [ AZURE-classic-portal] prostřednictvím zdroje [manifest aplikace](#application-manifest)a uložené v prostředků [appRoles vlastnost][AAD-Graph-Sp-Entity]. Portál Azure klasické slouží také můžete přiřadit uživatele "uživatel" rolím a nakonfigurovat klienta [aplikace oprávnění](#permissions) pro přístup k roli "aplikace".

Podrobné informace o role aplikací vystaveným Azure AD grafu API, najdete v článku [Grafu rozhraní API oprávnění obory][AAD-Graph-Perm-Scopes]. Příklad podrobné implementaci naleznete v tématu [řízení přístupu v cloudu aplikací pomocí Azure AD na základě rolí][Duyshant-Role-Blog].

## <a name="scopes"></a>oborů
Podobně jako [role](#roles)obory lze [prostředků serveru](#resource-server) k řízení přístupu k chráněným prostředkům. Obory slouží k provádění [obor] [ OAuth2-Access-Token-Scopes] řízení přístupu, pro [klientské aplikace](#client-application) , která přidělil delegované přístup k prostředku jejím vlastníkem.

Obory jsou definované zdroje řetězce (například "Mail.Read", "Directory.ReadWrite.All") spravovaný v [Azure klasické portál] [ AZURE-classic-portal] prostřednictvím zdroje [manifest aplikace](#application-manifest)a uložené v prostředků [oauth2Permissions vlastnost][AAD-Graph-Sp-Entity]. Portál Azure klasické taky slouží ke konfiguraci klient aplikace [Delegovat oprávnění](#permissions) pro přístup k obor.

Nejlepší praktické cvičení pojmenování, je použití formátu "resource.operation.constraint". Podrobné informace o obory vystaveným Azure AD grafu API, najdete v článku [Grafu rozhraní API oprávnění obory][AAD-Graph-Perm-Scopes]. Pro obory zveřejněné pro služby Office 365, přečtěte si článek Principy [Office 365 API oprávnění][O365-Perm-Ref].

## <a name="security-token"></a>tokenu zabezpečení
Podepsaný dokument obsahující deklarace, například dlouhé OAuth2 token nebo výraz SAML 2.0. [Povolení udělit](#authorization-grant)OAuth2 služby [přístupový token](#access-token) (OAuth2) a [ID Token](OpenID Connect) jsou typy tokenů zabezpečení, které jsou implementovaná jako [JSON Web tokenu (JWT)][JWT].

## <a name="service-principal-object"></a>objekt služby
Kdy se rejstříku/aktualizace aplikace [Azure klasické portál][AZURE-classic-portal], portálu vytvoří/zavedli [aplikace objektu](#application-object) a objekt zabezpečení odpovídající služby tomuto klientovi. Aplikační objekt *definuje* konfigurace identity aplikace globálně (přes všechny klienty kde přidruženou aplikaci udělený přístup), a je šablona, ze kterého odpovídající hlavní objekty služby jsou *odvozeny* pro použití místně za běhu (v určitých klienta).

V části [aplikace a služby jistinu objektů] [ AAD-App-SP-Objects] Další informace.

## <a name="sign-in"></a>přihlášení
Proces [klientské aplikaci](#client-application) zahájení koncový uživatel ověřování a zachycení týkající se stavu, za účelem získání [tokenu zabezpečení](#security-token) nebo definování rozsahu relaci aplikace do tohoto stavu. Uveďte mohou obsahovat artefakty například informace o profilech uživatelů a informace odvozeno z tokenu deklarované.

Implementace jednotného přihlašování (SSO) se obvykle používá funkci přihlašovací aplikace. Ji může taky předcházet "registrace" funkce jako vstupní bod pro koncového uživatele k získání přístupu k aplikaci (při prvním přihlášení). Registrace funkce se používá k shromažďovat a další stavu specifická pro uživatele trvalého a může vyžadovat [svolení uživatele](#consent).

## <a name="sign-out"></a>odhlašovací
Proces zrušením ověřovací koncového uživatele, odpojení stavu uživatele přidružený relaci [klientské aplikaci](#client-application) při [přihlášení](#sign-in)

## <a name="tenant"></a>klienta
Instanci adresáře služby Azure AD nazývá Azure AD klienta. Nabízí celou řadu funkcí, včetně:

- Služba registr integrovaných aplikací
- ověřování uživatelských účtů a registrované aplikace
- ZBÝVAJÍCÍ koncové body je potřebný kvůli podpoře různých protokolů včetně OAuth2 a SAML, včetně [koncový bod se tak mohli ověřovat](#authorization-endpoint), [tokenu koncového bodu](#token-endpoint) a "běžné" koncový bod využívány [více klienta](#multi-tenant-application).

Klienta, kterého je také přidružený Azure AD nebo předplatné Office 365 během vytváření předplatného poskytnout podpoře funkcí identit a správu přístupu pro předplatné. Přečtěte si, [Jak získat tenanta služby Azure Active Directory] [ AAD-How-To-Tenant] podrobné informace o různých způsobech, můžete získat přístup do klienta. V tématu [jak Azure předplatná souvisí s Azure Active Directory] [ AAD-How-Subscriptions-Assoc] podrobné informace o vztah mezi předplatná a Azure AD klienta.

## <a name="token-endpoint"></a>tokenu koncový bod
Jedna z koncové body implementovaná [se tak mohli ověřovat server](#authorization-server) podporuje OAuth2 [uděluje se tak mohli ověřovat](#authorization-grant). V závislosti grant (udělit), je lze získat [přístupový token](#access-token) (a související token "aktualizace") [klientského](#client-application)počítače nebo [ID token](#ID-token) při použití s webová část Seznam [Připojit OpenID] [ OpenIDConnect] protokol.

## <a name="user-agent-based-client"></a>Na základě uživatelského agenta klienta
Typ [klientské aplikaci](#client-application) , která spustí v rámci uživatelského agenta (například webový prohlížeč), například jedné stránce aplikace (zabezpečené ověřování HESLA) a soubory ke stažení kód z webového serveru. Protože všechny kód se spustí na zařízení, nebude považována za "veřejné" klienta kvůli společnosti ukládání přihlašovacích údajů soukromě/důvěrně. Přečtěte si téma [typy klientů OAuth2 a profily] [ OAuth2-Client-Types] další podrobnosti.

## <a name="user-principal"></a>UPN
Podobně jako způsob, jakým služba objekt se používá pro znázornění instance aplikace, uživatel objekt je jiný typ zabezpečení, které představuje uživatele. Azure AD grafu [uživatel] [ AAD-Graph-User-Entity] definuje schéma objektu uživatele, včetně související s uživatelem vlastnosti například jméno a příjmení hlavní uživatelské jméno, členství v adresáři roli, atd. Díky tomu konfigurace identita uživatele pro Azure AD stanovit UPN za běhu. UPN se používá pro ověřeného uživatele pro jednotné přihlašování, nahrávání [souhlas](#consent) delegování, přičemž rozhodnutí řízení přístupu atd.

## <a name="web-client"></a>Webový klient
Typ [klientské aplikaci](#client-application) , která provádí všechny kód na webový server a mohli budou fungovat jako "důvěrné" klient bezpečně uložením své přihlašovací údaje na serveru. Přečtěte si téma [typy klientů OAuth2 a profily] [ OAuth2-Client-Types] další podrobnosti.

## <a name="next-steps"></a>Další kroky
[Příručka pro vývojáře Azure AD] [ AAD-Dev-Guide] je portál pro účely všechny vývojové Azure AD Příbuzná témata, včetně přehledu informací o [integraci aplikace] [ AAD-How-To-Integrate] a základní informace o [Azure AD ověřování a scénáře podporovaného ověřování][AAD-Auth-Scenarios].

Stiskněte klávesovou zkratku následující oddíl komentáře Disqus k poskytnutí zpětné vazby a Pomozte nám vylepšit a přizpůsobovat naše obsah.

<!--Image references-->

<!--Reference style links -->
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-Subscriptions-Assoc]: ./active-directory-how-subscriptions-associated-directory.md
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-How-To-Tenant]: active-directory-howto-tenant.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Multi-Tenant-Overview]: active-directory-devhowto-multi-tenant-overview.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: ./active-directory-token-and-claims.md
[AZURE-classic-portal]: https://manage.windowsazure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[Microsoft-Graph]: https://graph.microsoft.io
[O365-Perm-Ref]: https://msdn.microsoft.com/en-us/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Endpoint]: https://tools.ietf.org/html/rfc6749#section-3.1
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-AuthZ-Endpoint]: http://openid.net/specs/openid-connect-core-1_0.html#AuthorizationEndpoint
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken
