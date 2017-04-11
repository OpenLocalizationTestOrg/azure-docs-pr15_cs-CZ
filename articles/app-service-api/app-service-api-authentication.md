<properties
    pageTitle="A mohli ověřovat pro rozhraní API aplikace v aplikaci služby Azure | Microsoft Azure"
    description="Informace o službách a tak mohli ověřovat, které aplikaci služby Azure poskytuje pro rozhraní API aplikace."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/23/2016"
    ms.author="rachelap"/>

# <a name="authentication-and-authorization-for-api-apps-in-azure-app-service"></a>A mohli ověřovat pro rozhraní API aplikace v aplikaci služby Azure

## <a name="overview"></a>Základní informace 

> [AZURE.NOTE] Toto téma se přesunou do konsolidované [ověřování aplikace služby / se tak mohli ověřovat](../app-service/app-service-authentication-overview.md) téma, který se zabývá webový server, mobilní telefon a rozhraní API aplikace.

Azure aplikaci služby nabízí předdefinované a tak mohli ověřovat služby, které implementace [OAuth 2.0](#oauth) a [OpenID připojení](#oauth). Tento článek popisuje možnosti, které jsou dostupné pro rozhraní API aplikace služby Azure aplikace a služby.

Následující obrázek znázorňuje některé klíčové vlastnosti Ověřovací služba aplikací:

* Ho upraví příchozí žádosti rozhraní API, což znamená, že to funguje s jazyk nebo framework nepodporuje aplikaci služby.
* Máte několik možností kolik ověřování práce můžete požadovaným v vlastního kódu.
* To funguje pro koncového uživatele a ověření účtu služby. 
* Podporuje pět Zprostředkovatelé identit jiní: Azure Active Directory, Facebook, Google, Twitteru nebo Account Microsoft.
* Funguje stejně pro rozhraní API aplikace, Web Apps a mobilní aplikace.

![](./media/app-service-api-authentication/api-apps-overview.png)

## <a name="language-agnostic"></a>Bez ohledu na jazyk

Před žádosti o přístup rozhraní API aplikace, což znamená, že fungovat funkce ověřování pro rozhraní API aplikace napsané v jazyce nebo framework neděje zpracování ověřování aplikace služby  Vaše rozhraní API můžou založená na ASP.NET, Java, Node.js nebo libovolnou systém, který podporuje aplikaci služby.

Aplikace služby předá token web JSON (JWT) v záhlaví se tak mohli ověřovat žádost HTTP a kód napsaný v jazyce nebo framework můžete získat informace, které potřebuje z tokenu. Kromě toho aplikaci služby umožňuje jednodušší přístup k nejčastěji používaná deklarací nastavením některé zvláštní záhlaví, třeba takto:

* X-MS--JISTINU – NÁZEV KLIENTA
* X-MS--JISTINU – ID KLIENTA
* X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN
* X-MS-TOKEN-FACEBOOK-EXPIRES-ON
 
V .NET API, můžete použít `Authorize` atribut a jemně odstupňovaná se tak mohli ověřovat můžete snadno vytvořit kódu na základě deklarované vzhledem k tomu, že je deklarací informace vyplní .NET předmětů.

## <a name="multiple-protection-options"></a>Více možností ochrany

Aplikaci služby můžete zabránit anonymní HTTP požadavků sahající rozhraní API aplikace, můžete přenést všechny žádosti o a ověřte tokeny pro požadavky, které je zahrnout nebo můžete nechat prostřednictvím všechny žádosti o bez provedení akce na nich:

1. Povolte jenom ověřené žádosti o přístup rozhraní API aplikace.

    Z prohlížeče obdrží žádost o konverzaci anonymní-li aplikaci služby vás přesměruje na přihlašovací stránku pro poskytovatele ověřování (Azure AD, Google, Twitter, atd.), které zvolíte. 

    Vyberete tuto možnost nemusíte vůbec napsat všechny ověřovací kód v aplikaci a povolení kód je zjednodušené protože nejdůležitější deklarací jsou k dispozici v záhlavích HTTP.

2. Povolte všechny žádosti o dosáhla rozhraní API aplikace, ale ověřte ověřené žádosti a předání ověřovací informace v záhlavích HTTP.

    Tato možnost umožňuje větší flexibilitu při zpracování anonymní požadavků, ale budete muset zadat kód, jestli chcete zabránit anonymním uživatelům v používání vaší rozhraní API. Protože nejoblíbenější deklarací předávají v záhlavích požadavků HTTP, se tak mohli ověřovat kód je celkem jednoduchá.
    
3. Povolte všechny žádosti o kontaktovat vaše rozhraní API, čeká na ověření informací v požadavcích žádné zpracování.

    Tuto možnost ponechá úkoly a tak mohli ověřovat úplně až kód aplikace.

V [Azure portál](https://portal.azure.com/), vyberte požadovanou možnost v **ověřování / se tak mohli ověřovat** zásuvné.

![](./media/app-service-api-authentication/authblade.png)

Možnosti 1 a 2 zapněte **Ověřování aplikace služby**a v rozevíracím seznamu **akce provedeny, pokud není ověřen žádost o** zvolte **přihlásit** se nebo **Povolit požadavek (žádná akce)**.  Pokud se rozhodnete, **Přihlaste se**, budete muset zvolte zprostředkovatele ověřování a konfigurace tohoto poskytovatele.

![](./media/app-service-api-authentication/actiontotake.png)

Podrobné informace o tom, jak nakonfigurovat ověřování najdete v článku [jak nakonfigurovat aplikaci služby aplikace pomocí služby Azure Active Directory přihlášení](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). Tento článek platí pro rozhraní API aplikace, jakož i mobilní aplikace a odkazuje na jiné články pro ostatní zprostředkovatelé ověřování.
 
## <a id="internal"></a>Ověření pomocí účtu služby

Ověřování aplikace služby funguje pro interní scénáře, jako je třeba k voláte z jedné aplikaci rozhraní API pro jiné rozhraní API aplikace. V tomto scénáři dostanete token pomocí přihlašovacích údajů pro účet služby místo pověření koncového uživatele. Účet služby je označovaná taky jako *služby základní* v Azure Active Directory a ověření pod účtem je označovaná taky jako scénáři služeb. 

Scénáře služby service chránit jen rozhraní API aplikace pomocí služby Azure Active Directory a poskytovat token AAD služby základní se tak mohli ověřovat při volání rozhraní API aplikace. Získání token pomocí klienta ID a tajné z aplikace AAD klienta. Kód bez speciálního Azure požaduje, například bývá platí pro zpracování token Zumo služby Mobile. Příklad tento scénář pomocí rozhraní API technologie ASP.NET aplikace se vztahuje konsolidovaná kurz [služby základní ověřování pro rozhraní API aplikace](app-service-api-dotnet-service-principal-auth.md).

Pokud chcete zpracovat scénáři služeb bez použití aplikace služby ověřování, můžete použít základní ověřování nebo certifikátů. Informace o certifikátů v Azure najdete v tématu [Jak chcete konfigurovat TLS ověřování Web Apps](../app-service-web/app-service-web-configure-tls-mutual-auth.md). Informace o základní ověřování technologie ASP.NET najdete v tématu [Ověření filtry v ASP.NET webového rozhraní API 2](http://www.asp.net/web-api/overview/security/authentication-filters).

Služba ověření pomocí účtu z aplikace logiky pro aplikaci služby do aplikace pro rozhraní API je speciální případ, který je popsaným v tématu [Použití vlastní rozhraní API hostitelem služby aplikace s aplikacemi jiných použití logických operátorů](../app-service-logic/app-service-logic-custom-hosted-api.md).

## <a name="mobile-client-authentication"></a>Ověření mobilního klienta

Další informace o tom, jak řešit ověřování z mobilních klientů najdete v článku [si přečtěte následující dokumentaci na ověřování pro mobilní aplikace](../app-service-mobile/app-service-mobile-ios-get-started-users.md). Ověřování aplikace služby lze použít stejnou výzvu mobilních aplikací a rozhraní API aplikace.
  
## <a name="more-information"></a>Další informace

Další informace o ověřování a povolení aplikace služby Azure najdete v následujících zdrojích:

* [Rozbalení ověřování aplikace služby / se tak mohli ověřovat](/blog/announcing-app-service-authentication-authorization/)
* [Jak nakonfigurovat aplikaci služby aplikace pomocí služby Azure Active Directory přihlášení](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md) (Včetně odkazů pro ostatní zprostředkovatelé ověřování v horní části stránky.) 

Další informace o OAuth 2.0 OpenID připojení a JSON Web tokeny (JWT) najdete v následujících zdrojích.

* [Začínáme s OAuth 2.0] (http://shop.oreilly.com/product/0636920021810.do "Začínáme s OAuth 2.0") 
* [Úvod k OAuth2, OpenID připojení a JSON Web tokeny (JWT) - PluralSight kurzu](http://www.pluralsight.com/courses/oauth2-json-web-tokens-openid-connect-introduction) 
* [Vytváření a zabezpečení RESTful rozhraní API pro více klientů v ASP.NET - PluralSight kurzu](http://www.pluralsight.com/courses/building-securing-restful-api-aspdotnet)

Další informace o Azure Active Directory najdete v následujících zdrojích.

* [Azure AD scénáře](http://aka.ms/aadscenarios)
* [Příručka Azure AD vývojáři](http://aka.ms/aaddev)
* [Azure AD vzorky](http://aka.ms/aadsamples)

## <a name="next-steps"></a>Další kroky

Tento článek obsahuje vysvětlení a tak mohli ověřovat funkce aplikace, které můžete použít pro rozhraní API aplikace. Na další kurz řady dosáhnout toho, aby Začínáme ukazuje, jak implementovat [ověřování uživatele v aplikaci služby rozhraní API aplikace](app-service-api-dotnet-user-principal-auth.md).
