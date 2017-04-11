<properties
    pageTitle="A mohli ověřovat v Azure mobilních aplikací | Microsoft Azure"
    description="Koncepční odkaz a základní informace o ověřování / povolení funkcí k aplikacím Mobile Azure"
    services="app-service\mobile"
    documentationCenter=""
    authors="mattchenderson"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="mahender"/>

# <a name="authentication-and-authorization-in-azure-mobile-apps"></a>A mohli ověřovat v Azure mobilní aplikace

## <a name="what-is-app-service-authentication--authorization"></a>Co je aplikace služby ověřování / se tak mohli ověřovat?

> [AZURE.NOTE] Toto téma se přesunou do konsolidované [ověřování aplikace služby / se tak mohli ověřovat](../app-service/app-service-authentication-overview.md) téma, který se zabývá webový server, mobilní telefon a rozhraní API aplikace.

Aplikace služby ověřování / se tak mohli ověřovat je funkce, které umožňuje aplikace pro přihlášení uživatelé s žádné změny kód povinné na back-end aplikace. Poskytuje snadný způsob, jak chránit vaše aplikace a pracovat s uživatelská data.

Aplikace služby používá federovaných identit, ve kterém 3rd výrobců **zprostředkovatele identit** ("IDP") ukládá účty a ověřuje uživatele, a aplikace tuto identitu místo vlastní. Služba aplikace podporuje pět Zprostředkovatelé identit jiní mimo pole: _Azure Active Directory_, _Facebook_, _Google_, _Účet Microsoft_nebo _Twitter_. Můžete taky rozšíříte tuto podporu pro aplikace integrací jiného zprostředkovatele identit nebo vlastní identitu řešení.

Aplikaci můžete využít jiné číslo z těchto poskytovatelů identit tak můžete svým koncovým uživatelům poskytnout volby pro způsob je přihlásit se.

Pokud chcete začít pracovat, přečtěte si téma jednu z následujících výukové programy pro:

- [Přidání ověřování aplikace pro iOS]
- [Přidání ověřování aplikace Xamarin.iOS]
- [Přidání ověřování aplikace Xamarin.Android]
- [Přidání ověřování aplikace pro Windows]

## <a name="how-authentication-works"></a>Jak funguje ověřování

Abyste mohli ověřovat pomocí jednoho z poskytovatelů identit, musíte nejdřív pro nastavení zprostředkovatele identit vědět o aplikaci. Zprostředkovatele identit potom poskytne ID a tajemství, které jsou zdrojem zpátky na aplikace. Dokončení vztah důvěryhodnosti a umožňuje aplikaci služby k ověření identity poskytnuté.

Tyto kroky jsou podrobně v následujících tématech:

- [Postup při konfiguraci aplikace pomocí služby Azure Active Directory přihlášení]
- [Postup při konfiguraci aplikace pomocí služby Facebook přihlášení]
- [Postup při konfiguraci aplikace pomocí služby Google přihlášení]
- [Postup při konfiguraci aplikace používat přihlašování pomocí zvláštního Account společnosti Microsoft]
- [Postup při konfiguraci aplikace používat Twitter přihlášení]

Jakmile všechno, co je nakonfigurovaný na back-end, můžete změnit klientem a přihlaste se. Dvěma způsoby zde:

- Pomocí jednoho řádku kódu, informujte mobilní aplikace klienta SDK přihlášení uživatele.
- Využití SDK publikována poskytovatelem dané identitě k vytvoření identity a získat přístup k aplikaci služby.

>[AZURE.TIP] Většina aplikací použít zprostředkovatele SDK získat setkat i v případě nativní pocit přihlášení a můžete využít podpory aktualizace a další výhody zprostředkovatele.

### <a name="how-authentication-without-a-provider-sdk-works"></a>Jak funguje ověřování bez zprostředkovatele SDK

Pokud nechcete nastavit poskytovatele SDK, můžete povolit mobilní aplikace provádět přihlášení za vás. Mobilní aplikace klienta SDK otevřete zobrazení webového poskytovateli e a dokončení přihlášení. Občas blogy a fórech, které uvidíte, že to označuje jako "server toku" nebo "server směrované toku" od serveru spravuje přihlášení a klient SDK nikdy nedostane token zprostředkovatele.

Kód potřebné ke spuštění této toku jsou obsaženy v kurzu ověřování pro každou platformu. Na konci tok klient SDK má token aplikaci služby a tokenu je automaticky připojen k všechny žádosti o back-end.

### <a name="how-authentication-with-a-provider-sdk-works"></a>Jak funguje ověřování zprostředkovateli SDK

Práce s poskytovatelem SDK umožňuje prostředí Přihlaste se k více úzce spolupracovat s informacemi o platformě OS spuštěna aplikace. To taky vám token poskytovatele a některé informace o uživatelích na straně klienta, což usnadňuje podobně používat grafu rozhraní API a přizpůsobení uživatelského prostředí. Občas na blogy a fór uvidíte to označovány jako "klienta tok" nebo "směrované klienta toku" od kódu na straně klienta je zpracování přihlášení a kód klienta má přístup k token zprostředkovatele.

Po získání token poskytovatele musí nechat zasílat aplikaci služby pro ověření. Na konci tok klient SDK má token aplikaci služby a tokenu je automaticky připojen k všechny žádosti o back-end. Vývojář lze také umístit odkaz na token poskytovatele, pokud tak rozhodnete.

## <a name="how-authorization-works"></a>Jak funguje se tak mohli ověřovat

Aplikace služby ověřování / se tak mohli ověřovat poskytuje několik možností pro **akce provedeny, pokud není požadavek ověřit**. Dříve než kódu obdrží žádost o dané, může být zkontrolujte aplikaci služby, pokud je ověření žádosti a pokud ne, odmítne a pokusí se máte přihlásit před pokusem znovu.

Jednou z možností je mít neověřený přesměrování žádosti na jeden z Zprostředkovatelé identit jiní. Ve webovém prohlížeči tím byste převzetí uživatele na novou stránku. Však mobilních klientů nelze přesměrována tímto způsobem a neověřený odpovědi se zobrazí na HTTP _401 Neautorizováno_ odpověď. Tato možnost, první žádost, který díky svému klientovi buďte vždy koncový bod přihlášení a proveďte hovory na jiné rozhraní API. Pokud budete chtít zavolat jiné rozhraní API před přihlášením, bude váš klient dojít k chybě.

Pokud chcete mít přesnější určit, přes který koncové body vyžadují ověřování, můžete také vybrat "žádná akce (povolte žádosti o)" pro neověřené požadavky. V tomto případě všechna rozhodnutí ověřování odložit kódu aplikace. To umožňuje povolit přístup k určitým uživatelům založené na vlastních ověřovacích pravidel.

## <a name="documentation"></a>Si přečtěte následující dokumentaci

Následující výukové programy pro zobrazení, jak lze přidat ověřování pro mobilní klienty pomocí aplikace služby:

- [Přidání ověřování aplikace pro iOS]
- [Přidání ověřování aplikace Xamarin.iOS]
- [Přidání ověřování aplikace Xamarin.Android]
- [Přidání ověřování aplikace pro Windows]

Následující kurzy ukazují, jak nakonfigurovat aplikaci služby pomocí zprostředkovatelů ověřování různých:

- [Postup při konfiguraci aplikace pomocí služby Azure Active Directory přihlášení]
- [Postup při konfiguraci aplikace pomocí služby Facebook přihlášení]
- [Postup při konfiguraci aplikace pomocí služby Google přihlášení]
- [Postup při konfiguraci aplikace používat přihlašování pomocí zvláštního Account společnosti Microsoft]
- [Postup při konfiguraci aplikace používat Twitter přihlášení]

Pokud chcete použít systému identit než těch, které jsou k dispozici tady, můžete taky využít [Náhled podporu vlastní ověřování serveru .NET SDK](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth).

[Přidání ověřování aplikace pro iOS]: app-service-mobile-ios-get-started-users.md
[Přidání ověřování aplikace Xamarin.iOS]: app-service-mobile-xamarin-ios-get-started-users.md
[Přidání ověřování aplikace Xamarin.Android]: app-service-mobile-xamarin-android-get-started-users.md
[Přidání ověřování aplikace pro Windows]: app-service-mobile-windows-store-dotnet-get-started-users.md

[Postup při konfiguraci aplikace pomocí služby Azure Active Directory přihlášení]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Postup při konfiguraci aplikace pomocí služby Facebook přihlášení]: app-service-mobile-how-to-configure-facebook-authentication.md
[Postup při konfiguraci aplikace pomocí služby Google přihlášení]: app-service-mobile-how-to-configure-google-authentication.md
[Postup při konfiguraci aplikace používat přihlašování pomocí zvláštního Account společnosti Microsoft]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Postup při konfiguraci aplikace používat Twitter přihlášení]: app-service-mobile-how-to-configure-twitter-authentication.md
