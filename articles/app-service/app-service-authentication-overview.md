<properties
    pageTitle="A mohli ověřovat v aplikaci služby Azure | Microsoft Azure"
    description="Koncepční odkaz a základní informace o ověřování / povolení funkce pro aplikaci služby Azure"
    services="app-service"
    documentationCenter=""
    authors="mattchenderson"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="mahender"/>

# <a name="authentication-and-authorization-in-azure-app-service"></a>A mohli ověřovat v aplikaci služby Azure

## <a name="what-is-app-service-authentication--authorization"></a>Co je aplikace služby ověřování / se tak mohli ověřovat?

Aplikace služby ověřování / se tak mohli ověřovat je funkce, která umožňuje aplikace pro přihlášení uživatele, takže nemusíte změňte kód v aplikaci back-end. Poskytuje snadný způsob, jak chránit vaše aplikace a pracovat s uživatelská data.

Aplikace služby používá federovaných identit, ve kterém zprostředkovatele identit třetích stran ukládá účty a ověřuje uživatele. Aplikace využívá zprostředkovatele identit informace tak, aby aplikace nemá uložit informace přímo. Služba aplikace podporuje pět Zprostředkovatelé identit jiní mimo pole: Azure Active Directory, Facebook, Google, Account Microsoft nebo Twitter. Aplikaci můžete použít libovolný počet tyto Zprostředkovatelé identit jiní Pokud chcete, aby uživatelé s možnostmi pro jak se přihlásit. Rozbalte integrovanou podporu, můžete integrovat jiného zprostředkovatele identit nebo [vlastní identitu řešení][custom-auth].

Pokud chcete začít pracovat, najdete v jednom z těchto kurzech:

- [Přidání ověřování aplikace pro iOS] [ iOS] (nebo [Androidem], [Windows], [Xamarin.iOS], [Xamarin.Android], [Xamarin.Forms]nebo [Cordova])
- [Ověřování uživatelů pro rozhraní API aplikace v aplikaci služby Azure][apia-user]
- [Začínáme s aplikací služby Azure – část 2][web-getstarted]

## <a name="how-authentication-works-in-app-service"></a>Jak funguje ověřování aplikace služby

Abyste mohli ověřovat pomocí jedné z Zprostředkovatelé identit jiní, musíte nejdřív pro nastavení zprostředkovatele identit vědět o aplikaci. Zprostředkovatele identit bude zadejte ID a tajemství poskytující služby aplikace. Vztah důvěryhodnosti tím dokončíte tak, aby aplikace služby můžete ověřit uživatele výrazy, jako je ověřování tokenů z zprostředkovatele identit.

Pro přihlášení uživatele pomocí některého z těchto poskytovatelů, musí uživatel přesměrovaní na koncový bod, který se přihlašuje uživatelé poskytovatele. Pokud zákazníky pomocí webového prohlížeče, můžete si aplikaci služby automaticky směrovat všichni uživatelé neověřených koncový bod, který přihlášení uživatele. Jinak, budete muset přímé zákazníky `{your App Service base URL}/.auth/login/<provider>`, kde `<provider>` je jedním z následujících hodnot: aad, facebook, google, microsoft nebo twitter. V částech dál v tomto článku jsou vysvětleny Mobile a rozhraní API scénáře.

Uživatelé, kteří pracují s aplikací prostřednictvím webového prohlížeče bude mít soubor cookie nastavit tak, že budou zůstat ověřené procházení aplikace. Pro jiné typy klientů, například číslo na mobil, token web JSON (JWT), které by měly prezentovat `X-ZUMO-AUTH` záhlaví, bude vydán pro klienta. Mobilní aplikace klienta SDK to zpracuje za vás. Můžete taky token identity Azure Active Directory nebo přístupový token přímo do lze zahrnout `Authorization` záhlaví jako [nosný token](https://tools.ietf.org/html/rfc6750).

Aplikace služby ověří souborů cookie nebo token, který aplikace při potížích pro ověřování uživatelů. Pokud chcete omezit, kdo má přístup k aplikaci, naleznete v části [se tak mohli ověřovat](#authorization) dál v tomto článku.

### <a name="mobile-authentication-with-a-provider-sdk"></a>Mobilní ověřování zprostředkovateli SDK

Po všechno, co je nakonfigurovaný na back-end, můžete změnit mobilních klientů na přihlášení při aplikaci služby. Dvěma způsoby tady:

- Použití SDK, která zprostředkovatele dané identitě publikuje k vytvoření identity a získat přístup k aplikaci služby.
- Používá jedná plná čára kód tak, aby aplikace mobilního klienta SDK přihlásit uživatelé.

>[AZURE.TIP] Většina aplikací by měl pomocí zprostředkovatele SDK jednotný když uživatelé přihlašovat, používat podporu aktualizovat a získat další výhody, které určuje zprostředkovatele.

Při použití poskytovatele SDK mohli uživatelé přihlašovat prostředí, které více úzce integrována operační systém, který je spuštěna aplikace. To taky vám token poskytovatele a některé informace o uživatelích na straně klienta, což usnadňuje podobně používat grafu rozhraní API a přizpůsobení uživatelského prostředí. Občas blogů a fóra, zobrazí se to označovány jako "klienta tok" nebo "řízené klienta toku" protože kód v klientském počítači přihlásí uživatelů a kód klienta má přístup k token poskytovatele.

Po získání token poskytovatele musí nechat zasílat aplikaci služby pro ověření. Po aplikaci služby ověří tokenu, aplikaci služby vytvoří nový token aplikaci služby vrácená klientovi. Klient aplikace Mobile SDK má pomocné metody spravovat tento exchange a automaticky připojit token všechny žádosti o back-end aplikace. Vývojáři lze také umístit odkaz na token poskytovatele, pokud tak rozhodnete.

### <a name="mobile-authentication-without-a-provider-sdk"></a>Mobilní ověření bez zprostředkovatele SDK

Pokud nechcete nastavit poskytovatele SDK, můžete povolit funkci mobilní aplikace služby Azure aplikace přihlašovat za vás. Klient aplikace Mobile SDK otevřete zobrazení webového poskytovateli e a přihlášení uživatele. Občas blogů a fóra, zobrazí se to označovány jako "server toku" nebo "server řízené toku" protože server je spravuje procesu přihlášení uživatele a klient SDK nikdy nedostane token poskytovatele.

Kód spustíte tento toku je součástí kurz ověřování pro každou platformu. Na konci tok klient SDK má token aplikaci služby a tokenu je automaticky připojen k všechny žádosti o back-end aplikace.

### <a name="service-to-service-authentication"></a>Služba Služba ověřování

Přestože můžete uživatelům přístup k vaší aplikaci, kterým můžete důvěřovat jiné aplikace pro volání vlastní API. Je třeba mít jeden web appu volání rozhraní API v jiné aplikaci web. V tomto scénáři pomocí přihlašovacích údajů pro účet služby místo přihlašovací údaje uživatele pro získání token. Účet služby je označovaná taky jako *služby základní* v žargonu Azure Active Directory a ověřování pomocí těchto účtů je označovaná taky jako scénáři služeb.

>[AZURE.IMPORTANT] Mobilní aplikace spustit na zařízení zákazníka, proto se mobilní aplikace _není_ počet jako důvěryhodné aplikací a nepoužívejte služby základní vývojový. Místo toho by se měly používat uživatele toku, která byla dříve podrobné.

Scénáře služby service aplikaci služby chránit aplikace pomocí služby Azure Active Directory. Volání jenom potřebuje k poskytování token hlavní povolení služby Azure Active Directory vypočtený pomocí klienta ID a klient tajné ze služby Azure Active Directory. Příklad tato situace s použitím ASP.NET rozhraní API aplikace se vysvětluje kurz [služby základní ověřování pro rozhraní API aplikace][apia-service].

Pokud chcete použít ověřování aplikace služby pro zpracování scénáři služeb, můžete použít základní ověřování nebo certifikátů. Informace o certifikátů v Azure najdete v tématu [Jak chcete konfigurovat TLS ověřování Web Apps](../app-service-web/app-service-web-configure-tls-mutual-auth.md). Informace o základní ověřování technologie ASP.NET najdete v tématu [Ověření filtry v ASP.NET webového rozhraní API 2](http://www.asp.net/web-api/overview/security/authentication-filters).

Služba ověření pomocí účtu z aplikace logiky pro aplikaci služby do aplikace pro rozhraní API je speciální případ, který je uvedené v [Používání vlastní rozhraní API hostitelem služby aplikace s aplikacemi jiných použití logických operátorů](../app-service-logic/app-service-logic-custom-hosted-api.md).

## <a name="authorization"></a>Jak se tak mohli ověřovat funguje v aplikaci služby

Abyste měli úplnou kontrolu nad požadavky, které můžete získat přístup k aplikaci. Aplikace služby ověřování / možné konfigurovat tak mohli ověřovat pomocí kterékoli z těchto situací:

- Povolte jenom ověřené žádosti o přístup aplikace.

    Pokud v prohlížeči obdrží žádost o konverzaci anonymní, aplikaci služby vás přesměruje na stránku pro zprostředkovatele identit, které zvolíte tak, aby mohli uživatelé přihlašovat. Pokud chcete žádost pochází z mobilního zařízení, je vrácené odpověď odpověď HTTP _401 Neautorizováno_ .

    Vyberete tuto možnost nemusíte vůbec napsat všechny ověřovací kód v aplikaci. Pokud potřebujete jemnější se tak mohli ověřovat, informace o uživateli neexistuje kódu.

- Povolte všechny žádosti o dosáhla aplikace, ale ověřte ověřené žádosti a předání ověřovací informace v záhlavích HTTP.

    Tuto možnost odkládat údaje rozhodnutí o ověření kódu aplikace. Poskytuje větší flexibilitu při zpracování anonymní požadavků, ale budete muset zadat kód.

- Povolit všechny žádosti o přístup aplikace a žádná akce na ověřovací údaje do žádosti.

    V tomto případě ověřování / se tak mohli ověřovat funkce vypnutá. Úkoly a tak mohli ověřovat jsou úplně až kód aplikace.

Předchozí chování jsou řízeny možnost **akce provedeny, pokud není ověřen požadavek** na portálu Azure. Pokud se rozhodnete * *přihlášení *název poskytovatele* **, všechny žádosti o muset podpisům.** Povolte žádosti (žádná akce) ** odkládat údaje rozhodnutí o povolení kódu, ale pořád poskytuje informace o ověřování. Pokud chcete svůj kód všechno, co dělat, můžete zakázat ověřování / funkce se tak mohli ověřovat.

## <a name="working-with-user-identities-in-your-application"></a>Práce s identit uživatelů v aplikaci

Aplikace služby předává některé informace o uživateli aplikace pomocí zvláštní záhlaví. Externí požadavky zakázat tato záhlaví a bude jenom v případě prezentace nastaven tak, že aplikace služby ověřování / se tak mohli ověřovat. Příklad záhlaví, patří:

* X-MS--JISTINU – NÁZEV KLIENTA
* X-MS--JISTINU – ID KLIENTA
* X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN
* X-MS-TOKEN-FACEBOOK-EXPIRES-ON

Kód, který je psán v jazyk nebo framework můžete získat informace, které potřebuje z těchto záhlaví. K aplikacím ASP.NET 4.6 **ClaimsPrincipal** automaticky nastavit s příslušnými hodnotami.

Aplikaci můžete taky získat podrobnosti další uživatele přes HTTP GET na `/.auth/me` koncový bod aplikace. Platné token, který je součástí žádosti vrátí JSON datové s podrobnosti o zprostředkovatele, který se používá, základní token poskytovatele a některé další informace o uživateli. Mobilní aplikace server SDK poskytovat pomocné metody pro práci s těmito daty. Další informace najdete v tématu [použití SDK Node.js aplikace Mobile Azure](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-getidentity)a [Práce se serverem back-end .NET SDK pro aplikace Mobile Azure](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#user-info).

## <a name="documentation-and-additional-resources"></a>Dokumentace a další zdroje informací

### <a name="identity-providers"></a>Zprostředkovatelé identit jiní
Následující kurzy ukazují, jak nakonfigurovat aplikaci služby používat různé ověřování poskytovatelů:

- [Postup při konfiguraci aplikace pomocí služby Azure Active Directory přihlášení][AAD]
- [Postup při konfiguraci aplikace pomocí služby Facebook přihlášení][Facebook]
- [Postup při konfiguraci aplikace pomocí služby Google přihlášení][Google]
- [Postup při konfiguraci aplikace používat přihlašování pomocí zvláštního Account společnosti Microsoft][MSA]
- [Postup při konfiguraci aplikace používat Twitter přihlášení][Twitter]

Pokud chcete použít systému identit než těch, které jsou k dispozici tady, můžete [Zobrazit náhled vlastní ověřování podpory na serveru mobilní aplikace .NET SDK][custom-auth], které lze použít ve webových aplikacích, mobilní aplikace nebo rozhraní API aplikace.

### <a name="web-applications"></a>Webové aplikace
Následující výukové programy pro zobrazení, jak lze přidat ověřování do webové aplikace:

- [Začínáme s aplikací služby Azure – část 2][web-getstarted]

### <a name="mobile-applications"></a>Mobilní aplikace
Následující výukové programy pro zobrazení, jak lze přidat ověřování pro mobilní klienty pomocí toku řízené serveru:

- [Přidání ověřování aplikace pro iOS][iOS]
- [Přidání ověřování aplikace pro Android] [Android]
- [Přidání ověřování aplikace pro Windows] [Windows]
- [Ověřování přidat do aplikace Xamarin.iOS] [Xamarin.iOS]
- [Ověřování přidat do aplikace Xamarin.Android] [Xamarin.Android]
- [Ověřování přidat do aplikace Xamarin.Forms] [Xamarin.Forms]
- [Přidání ověřování aplikace Cordova] [Cordova]

Pokud chcete použít toku řízené klienta služby Azure Active Directory, použijte v následujících zdrojích:

- [Použití Active Directory Authentication Library pro iOS][ADAL-iOS]
- [Použití služby Active Directory Authentication Library pro Android][ADAL-Android]
- [Použití služby Active Directory Authentication Library v oknech a Xamarin][ADAL-dotnet]

Pokud chcete použít toku řízené klienta pro Facebook, použijte v následujících zdrojích:

- [Použití Facebook SDK pro iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#facebook-sdk)

Pokud chcete použít toku řízené klienta pro Twitter, použijte v následujících zdrojích:

- [Použití Twitter struktury pro iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#twitter-fabric)

Pokud chcete použít toku řízené klienta pro Google, použijte v následujících zdrojích:

- [Použití Google přihlašovací SDK pro iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#google-sdk)

### <a name="api-applications"></a>Rozhraní API aplikace
Následující kurzy ukazují, jak chránit vaše rozhraní API aplikace:

- [Ověřování uživatelů pro rozhraní API aplikace v aplikaci služby Azure][apia-user]
- [Služby základní ověřování pro rozhraní API aplikace v aplikaci služby Azure][apia-service]









[apia-user]: ../app-service-api/app-service-api-dotnet-user-principal-auth.md
[apia-service]: ../app-service-api/app-service-api-dotnet-service-principal-auth.md

[web-getstarted]: ../app-service-web/app-service-web-get-started-2.md#authenticate-your-users

[iOS]: ../app-service-mobile/app-service-mobile-ios-get-started-users.md
[Android]: ../app-service-mobile/app-service-mobile-android-get-started-users.md
[Xamarin.iOS]: ../app-service-mobile/app-service-mobile-xamarin-ios-get-started-users.md
[Xamarin.Android]: ../app-service-mobile/app-service-mobile-xamarin-android-get-started-users.md
[Xamarin.Forms]: ../app-service-mobile/app-service-mobile-xamarin-forms-get-started-users.md
[Windows]: ../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-users.md
[Cordova]: ../app-service-mobile/app-service-mobile-cordova-get-started-users.md

[AAD]: ../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook]: ../app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication.md
[Google]: ../app-service-mobile/app-service-mobile-how-to-configure-google-authentication.md
[MSA]: ../app-service-mobile/app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter]: ../app-service-mobile/app-service-mobile-how-to-configure-twitter-authentication.md

[custom-auth]: ../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth

[ADAL-Android]: ../app-service-mobile/app-service-mobile-android-how-to-use-client-library.md#adal
[ADAL-iOS]: ../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#adal
[ADAL-dotnet]: ../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#adal
