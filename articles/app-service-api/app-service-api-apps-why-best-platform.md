<properties 
    pageTitle="Rozhraní API aplikace Úvod | Microsoft Azure" 
    description="Přečtěte si aplikaci služby Azure vám pomůže vytvořit, host a používání RESTful rozhraní API." 
    services="app-service\api" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/23/2016" 
    ms.author="rachelap"/>

# <a name="api-apps-overview"></a>Přehled rozhraní API aplikace

Rozhraní API aplikace v aplikaci služby Azure nabízejí funkce, které usnadňují vývoj, host a používání rozhraní API v cloudu a místní. Se rozhraní API aplikace získáte enterprise testu zabezpečení ovládací prvek snadný přístup, hybridní připojení, automatické generování SDK a Bezproblémová integrace s [Aplikacemi jiných použití logických operátorů](../app-service-logic/app-service-logic-what-are-logic-apps.md).

[Aplikace služby Azure](../app-service/app-service-value-prop-what-is.md) je plně spravovaných platformy pro web, mobilní a scénáře integrace. Rozhraní API aplikace je taková typů čtyři aplikace nabízené [Služby Azure aplikace](../app-service/app-service-value-prop-what-is.md).

![Typy aplikace v aplikaci služby Azure](./media/app-service-api-apps-why-best-platform/appservicesuite.png)

## <a name="why-use-api-apps"></a>Proč používat rozhraní API aplikace?

Tady jsou některé klíčové funkce rozhraní API aplikace:

- **Přenést svůj existující rozhraní API jako-je** -nemáte, abyste nezměnili žádné kódu v existující rozhraní API využít rozhraní API aplikace – stačí nasadit kódu do aplikace pro rozhraní API. Vaše rozhraní API můžete použít libovolný jazyk nebo framework nepodporuje aplikaci služby včetně prvků ASP.NET a C# Java, PHP, Node.js a Python.

- **Snadno spotřebu** - integrovaná podpora [Swagger rozhraní API metadat](http://swagger.io/) zajišťuje vaše rozhraní API snadno spotřební pomocí různých klientů.  Automaticky generovat kód klienta pro vaše rozhraní API v různých jazycích včetně C# Java a Javascript. Snadno nakonfigurujte [CORS](app-service-api-cors-consume-javascript.md) beze změny kódu. Další informace najdete v tématu [aplikace služby rozhraní API aplikace metadat pro rozhraní API zjišťování a hodnocení kód generování](app-service-api-metadata.md) a [spotřebovat aplikace rozhraní API ze JavaScript pomocí CORS](app-service-api-cors-consume-javascript.md). 

- **Řízení přístupu jednoduché** – ochrana rozhraní API aplikace z neověřené Accessu s žádné změny kódu. Předdefinované ověřovacích služeb rozhraní API zabezpečený přístup pomocí protokolu jiných služeb nebo klienty představující uživatelů. Poskytovatelé podporované identity zahrnout Azure Active Directory, Facebook, Twitter, Google a Account Microsoft. Klienti můžou pomocí mobilní aplikace SDK nebo Active Directory Authentication knihovny (ADAL). Další informace najdete v tématu [a tak mohli ověřovat pro rozhraní API aplikace v aplikaci služby Azure](app-service-api-authentication.md).

- **Integrace aplikace visual Studio** - vyhrazené nástroje ve Visual Studiu zjednodušit práce pro vytváření nasazení, jinými, ladění a správa rozhraní API aplikace. Další informace najdete v tématu [oznámení SDK Azure 2.8.1 pro .NET](/blog/announcing-azure-sdk-2-8-1-for-net/).

- **Integrace s aplikacemi jiných logiky** - rozhraní API aplikace, které jste vytvořili určené pro [Aplikaci služby logiky aplikace](../app-service-logic/app-service-logic-what-are-logic-apps.md).  Další informace najdete v tématu [Použití vlastní rozhraní API hostitelem služby aplikace s aplikacemi jiných logiky](../app-service-logic/app-service-logic-custom-hosted-api.md) a [nové schématu verze 2015-08-01-preview](../app-service-logic/app-service-logic-schema-2015-08-01.md).

Kromě toho rozhraní API aplikace můžete využít funkce nabízené [Web Apps](../app-service-web/app-service-web-overview.md) a [Mobilní aplikace](../app-service-mobile/app-service-mobile-value-prop.md). Platí také true: Pokud používáte v prohlížeči nebo mobilní aplikaci hostovat rozhraní API, ho můžete využít funkce rozhraní API aplikace například Swagger metadat pro klienta generování kódu a CORS pro přístup pomocí prohlížeče doménami. Jediný rozdíl mezi tři aplikace typu (rozhraní API, web, mobilní) je název a ikony použité pro ně Azure portálu.

## <a name="whats-the-difference-between-api-apps-and-azure-api-management"></a>Jaký je rozdíl mezi aplikacemi pro rozhraní API a správa rozhraní API Azure?

[Správa rozhraní API Azure](../api-management/api-management-key-concepts.md) a rozhraní API aplikace jsou doplňková služby:

* Správa rozhraní API je o správě rozhraní API. Umístění uživatelského rozhraní API správy na rozhraní API pro sledování a omezení použití, pracovat s vstupní a výstupní, několik rozhraní API sloučit do jedné koncového bodu a tak dále. Rozhraní API, které spravují mohou být umístěny kdekoli.
* Rozhraní API aplikace je o hostování rozhraní API. Službu obsahuje funkce, které usnadňují vývoj a náročné rozhraní API, ale neprovádí druhy sledování, omezení, manipulaci s nebo sloučení uvedená rozhraní API Správa znamená. Pokud nepotřebujete funkce správy rozhraní API, budete moct hostovat rozhraní API v aplikacích pro rozhraní API bez použití rozhraní API správy.

Zde je diagram, který bude ilustrovat Správa rozhraní API pro rozhraní API hostované v aplikacích pro rozhraní API a kdekoli jinde.

![Správa Azure rozhraní API a rozhraní API aplikace](./media/app-service-api-apps-why-best-platform/apia-apim.png)

Některé funkce správy rozhraní API a rozhraní API aplikace mají podobné funkce.  Například obě automatizovat CORS podpory. Při použití dvou službách společně použijete správy rozhraní API pro CORS od funguje jako front-end ke svým aplikacím rozhraní API. 

## <a name="getting-started"></a>Začínáme

Začínáme s aplikacemi jiných rozhraní API nasazením ukázkový kód do jedné, najdete v článku kurz pro ten framework dáváte přednost:

* [TECHNOLOGIE ASP.NET](app-service-api-dotnet-get-started.md) 
* [Node.js](app-service-api-nodejs-api-app.md) 
* [Java](app-service-api-java-api-app.md) 

Otázky můžete pokládat o rozhraní API aplikace, spusťte podproces ve [fóru rozhraní API aplikace](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps). 
