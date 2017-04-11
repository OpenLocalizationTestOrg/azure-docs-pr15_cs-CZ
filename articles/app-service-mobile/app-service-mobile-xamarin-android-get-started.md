<properties
    pageTitle="Začínáme s Azure mobilních aplikací pro Xamarin.Android aplikace"
    description="Postupujte podle kurzu, který začít používat Azure mobilní aplikace pro Xamarin Android vývoj"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="erikre"
    editor="" />

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha" />

#<a name="create-a-xamarinandroid-app"></a>Vytvoření aplikace Xamarin.Android

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>Základní informace

Tento kurz se dozvíte, jak přidat do back-end cloudové služby do Xamarin.Android aplikace. Další informace najdete v tématu [jaké mobilní aplikace](app-service-mobile-value-prop.md).

Snímek obrazovky z aplikace dokončený je níže:

![][0]

Dokončení tohoto kurzu je požadována pro všechny další mobilní aplikace výukové programy pro Xamarin.Android aplikace.

##<a name="prerequisites"></a>Zjistit předpoklady pro

K provedení tohoto kurzu, budete potřebovat následující požadavky:

* Účet Azure active. Pokud nemáte účet, registraci zkušební verzi Azure a získat až 10 bezplatná mobilní aplikace. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio s Xamarin. Postupujte podle pokynů uvedených v tématu [Nastavení a instalace aplikace Visual Studio a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) .

>[AZURE.NOTE]Pokud chcete začít pracovat s aplikaci služby Azure před registrací účet Azure, přejděte na [Zkuste aplikaci služby](https://tryappservice.azure.com/?appServiceName=mobile).  Krátkodobý starter mobilní aplikaci můžete okamžitě vytvořit v aplikaci služby. Žádné povinné; kreditní karty žádné závazky.

## <a name="create-an-azure-mobile-app-backend"></a>Vytvoření backendovou Azure mobilní aplikaci

Tímto postupem vytvoření back-end mobilní aplikaci.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Nyní máte zřízení backendovou Azure mobilní aplikaci, něhož aplikacemi mobilního klienta. Pak stáhnout projekt server pro jednoduchou "úkol seznam" back-end a publikujte projekt do Azure.

## <a name="configure-the-server-project"></a>Konfigurace project serveru

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinandroid-app"></a>Stáhnout a spustit aplikaci Xamarin.Android

1. V části **Stáhnout a spustit Xamarin.Android projektu**klikněte na tlačítko **Stáhnout** .

    Uložit soubor mu tuhle zkomprimovanou projektu do místního počítače a poznamenejte si kam ukládat.

2. Stisknutím klávesy **F5** sestavení projektu a spusťte aplikaci.

3. V aplikaci zadejte smysluplný text, jako je _Dokončeno kurzu_ a potom klikněte na tlačítko **Přidat** .

    ![][10]

    Data z žádost je vložen do tabulky TodoItem. Back-end mobilní aplikace jsou vráceny položky uložené v tabulce a data se zobrazí v seznamu.

    > [AZURE.NOTE] Můžete zkontrolovat kód, který má přístup k mobilní aplikace backendovou dotazu a vložte data, která je součástí souboru ToDoActivity.cs C#.

##<a name="next-steps"></a>Další kroky

* [Přidání Offline synchronizace do aplikace](app-service-mobile-xamarin-android-get-started-offline-data.md)
* [Přidání ověřování aplikace](app-service-mobile-xamarin-android-get-started-users.md)
* [Přidání aplikace Xamarin.Android nabízená oznámení](app-service-mobile-xamarin-android-get-started-push.md)
* [Používání spravovaných klienta pro Azure mobilní aplikace](app-service-mobile-dotnet-how-to-use-client-library.md)


<!-- Images. -->
[0]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-completed-android.png
[6]: ./media/app-service-mobile-xamarin-android-get-started/mobile-portal-quickstart-xamarin.png
[8]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-vs.png
[9]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-xs.png
[10]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-startup-android.png

<!-- URLs. -->
[Azure Portal]: https://azure.portal.com/
[Visual Studio]: https://go.microsoft.com/fwLink/p/?LinkID=534203
