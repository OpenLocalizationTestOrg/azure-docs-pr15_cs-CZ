<properties
    pageTitle="Vytvoření aplikace Cordova na mobilních aplikací služby Azure aplikace | Microsoft Azure"
    description="Postupujte podle kurzu, který Začínáme s používáním back-end Azure mobilní aplikace pro vývoj Apache Cordova"
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""
    tags=""
    keywords="cordova, javascript, mobilního, klienta" />

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="create-an-apache-cordova-app"></a>Vytvoření Apache Cordova aplikace

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Základní informace

Tento kurz se dozvíte, jak přidat do back-end cloudové služby do mobilní aplikace Apache Cordova pomocí back-end Azure mobilní aplikace.  Vytvoříte novou back-end mobilní aplikace a jednoduchý _úkol seznamu_ Apache Cordova aplikace, která jsou uložená data aplikace v Azure.

Dokončení tohoto kurzu je požadována pro všechny ostatní Apache Cordova výukové programy pro informace o použití funkce mobilní aplikace v aplikaci služby Azure.

## <a name="prerequisites"></a>Zjistit předpoklady pro

Tento kurz, je potřeba k provedení těchto věcí:

* Počítač s [Visual Studio komunity 2015] nebo novější.
* [Visual Studio Tools for Apache Cordova].
* [Aktivní účet Azure](https://azure.microsoft.com/pricing/free-trial/).

Taky můžete obejít Visual Studio a používat příkazového řádku Apache Cordova přímo.  To je užitečné při dokončování kurz na počítači Mac.  Tento kurz nevztahuje kompilace Apache Cordova klientské aplikace pomocí příkazového řádku.

## <a name="create-a-new-azure-mobile-app-backend"></a>Vytvořit novou back-end Azure mobilní aplikaci

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

[Podívejte se na video zobrazující podobné kroky](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-1-Create-an-Azure-Mobile-App)

## <a name="configure-the-server-project"></a>Konfigurace project serveru

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-apache-cordova-app"></a>Stáhnout a spustit aplikaci Apache Cordova

[AZURE.INCLUDE [app-service-mobile-cordova-run-app](../../includes/app-service-mobile-cordova-run-app.md)]

## <a name="next-steps"></a>Další kroky

Teď, když jste dokončili tento kurz úvodní, přejděte jednu z následujících výukové programy pro:

* [Přidání ověření] vaší Apache Cordova aplikace.
* [Přidání nabízená oznámení] na vaše Apache Cordova aplikace.

Další informace o základních koncepcí službou Azure aplikace.

* [Ověřování]
* [Nabízená oznámení]

Naučte se používat SDK.

* [Apache Cordova SDK]
* [Serverového SDK]
* [Node.js Server SDK]

<!-- Images. -->

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Komunita aplikace Visual Studio 2015]: http://www.visualstudio.com/
[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Přidání ověřování]: app-service-mobile-cordova-get-started-users.md
[Přidání nabízená oznámení]: app-service-mobile-cordova-get-started-push.md
[Ověřování]: app-service-mobile-auth.md
[Nabízená oznámení]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[Serverového SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
