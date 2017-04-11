<properties
    pageTitle="Začínáme s aplikací Mobile služby Azure aplikace k aplikacím Xamarin.iOS | Microsoft Azure"
    description="Postupujte podle kurzu, který začít pracovat s pomocí mobilní aplikace pro vývoj Xamarin.iOS."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>


#<a name="create-a-xamarinios-app"></a>Vytvoření aplikace Xamarin.iOS

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>Základní informace

Tento kurz se dozvíte, jak přidáte do back-end cloudové služby v mobilní aplikaci Xamarin.iOS pomocí back-end Azure mobilní aplikace.  Vytvoříte novou back-end mobilní aplikace a jednoduchý _úkol seznamu_ Xamarin.iOS aplikace, která jsou uložená data aplikace v Azure.

Dokončení tohoto kurzu je požadována pro všechny ostatní Xamarin.iOS výukové programy pro informace o použití funkce mobilní aplikace v aplikaci služby Azure.

##<a name="prerequisites"></a>Zjistit předpoklady pro

K provedení tohoto kurzu, budete potřebovat následující požadavky:

* Účet Azure active. Pokud nemáte účet, zaregistrovat ke zkušební verzi Azure a získat až 10 bezplatná – mobilní aplikace, které můžete nadále používat i po uplynutí zkušebního období. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio s Xamarin. Postupujte podle pokynů uvedených v tématu [Nastavení a instalace aplikace Visual Studio a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) .

* Mac s Xcode 7.0 nebo novější a Xamarin Studio komunity nainstalovaný. V tématu [Nastavení a nainstalujte Visual Studiu a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) a [nastavení, instalovat a ověření uživatelé počítačů Mac](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).

>[AZURE.NOTE]Pokud chcete začít pracovat s aplikaci služby Azure před registraci účet Azure, přejděte na [Zkuste aplikaci služby](https://tryappservice.azure.com/?appServiceName=mobile). V mobilní aplikaci krátkodobý starter můžete okamžitě vytvořit v aplikaci služby – bez platební kartou povinné a bez závazky.

## <a name="create-an-azure-mobile-app-backend"></a>Vytvoření backendovou Azure mobilní aplikaci

Tímto postupem vytvoření back-end mobilní aplikaci.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-the-server-project"></a>Konfigurace project serveru

Nyní máte zřízení backendovou Azure mobilní aplikaci, něhož aplikacemi mobilního klienta. Pak stáhnout projekt server pro jednoduchou "úkol seznam" back-end a publikujte projekt do Azure.

Postupujte podle následujících kroků můžete nakonfigurovat project serveru používat Node.js nebo .NET back-end.

[AZURE.INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinios-app"></a>Stáhnout a spustit aplikaci Xamarin.iOS

1. Otevřete [portál Azure] v okně prohlížeče.

2. Na zásuvné nastavení pro mobilní aplikaci, klikněte na **Začít** > **Xamarin.iOS**. V kroku 3 klikněte na **vytvořit novou aplikaci** , pokud už není vybraná.  Pak klikněte na tlačítko **Stáhnout** .

    Klientská aplikace, která se připojí k mobilní back-end se stáhne. Uložit soubor mu tuhle zkomprimovanou projektu do místního počítače a poznamenejte si kam ukládat.

3. Extrahování projekt, který jste stáhli a otevřete si ho v Xamarin Studio (nebo Visual Studio).

    ![][9]

    ![][8]

4. Stisknutím klávesy F5 sestavení projektu a spusťte aplikaci v emulátoru iPhone.

5. V aplikaci zadejte smysluplný text, například _Informace Xamarin_a klikněte **+** tlačítko.

    ![][10]

    Data z žádost je vložen do tabulky TodoItem. Back-end mobilní aplikace jsou vráceny položky uložené v tabulce a data se zobrazí v seznamu.

>[AZURE.NOTE]Můžete zkontrolovat kód, který má přístup k back-end vaše mobilní aplikace k vytváření dotazů a vložte data do souboru QSTodoService.cs C#.

##<a name="next-steps"></a>Další kroky

* [Přidání Offline synchronizace do aplikace](app-service-mobile-xamarin-ios-get-started-offline-data.md)
* [Přidání ověřování aplikace](app-service-mobile-xamarin-ios-get-started-users.md)
* [Přidání aplikace Xamarin.Android nabízená oznámení](app-service-mobile-xamarin-ios-get-started-push.md)
* [Používání spravovaných klienta pro Azure mobilní aplikace](app-service-mobile-dotnet-how-to-use-client-library.md)

<!-- Anchors. -->
[Getting started with mobile app backends]:#getting-started
[Create a new mobile app backend]:#create-new-service
[Next Steps]:#next-steps

<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-ios-get-started/xamarin-ios-quickstart.png
[8]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-vs.png
[9]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-xs.png
[10]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-quickstart-startup-ios.png

<!-- URLs. -->
[Azure portálu]: https://portal.azure.com/
