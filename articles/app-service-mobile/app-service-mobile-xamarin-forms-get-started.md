<properties
    pageTitle="Začínáme s aplikací Mobile pomocí Xamarin.Forms"
    description="Postupujte podle kurzu, který začít používat Azure mobilní aplikace pro vývoj Xamarin.Forms"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="create-a-xamarinforms-app"></a>Vytvoření aplikace Xamarin.Forms

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>Základní informace

Tento kurz se dozvíte, jak přidat do back-end cloudové služby v mobilní aplikaci Xamarin.Forms pomocí back-end Azure mobilní aplikaci. Vytvoříte novou back-end mobilní aplikaci a jednoduchý _úkol seznamu_ Xamarin.Forms aplikace, která jsou uložená data aplikace v Azure.

Dokončení tohoto kurzu je požadována pro všechny další mobilní aplikace výukové programy pro Xamarin.Forms.

##<a name="prerequisites"></a>Zjistit předpoklady pro

Tento kurz, je potřeba k provedení těchto věcí:

* Účet Azure active. Pokud nemáte účet, můžete zaregistrovat zkušební verzi Azure a získat až 10 volné mobilní aplikace, které můžete nadále používat i po uplynutí zkušebního období Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio s Xamarin. Postupujte podle pokynů uvedených v tématu [Nastavení a instalace aplikace Visual Studio a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) . 

* Mac s Xcode 7.0 nebo novější a Xamarin Studio komunity nainstalovaný. V tématu [Nastavení a nainstalujte Visual Studiu a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) a [nastavení, instalovat a ověření uživatelé počítačů Mac](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).
 
>[AZURE.NOTE] Pokud chcete začít pracovat s aplikaci služby Azure před registrací účet Azure, přejděte na [Zkuste aplikaci služby](https://tryappservice.azure.com/?appServiceName=mobile), které můžete okamžitě vytvořit krátkodobý starter mobilní aplikaci v aplikaci služby. Žádné povinné; kreditní karty žádné závazky.

## <a name="create-a-new-azure-mobile-app-backend"></a>Vytvořit novou back-end Azure mobilní aplikaci

Tímto postupem vytvořte novou back-end mobilní aplikaci.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]


Nyní máte zřízení backendovou Azure mobilní aplikaci, něhož aplikacemi mobilního klienta. Pak stáhne projekt server pro jednoduchou "úkol seznam" back-end a publikujte projekt do Azure.

## <a name="configure-the-server-project"></a>Konfigurace project serveru

Postupujte podle návodu pro nastavení project serveru používat Node.js nebo .NET back-end.

[AZURE.INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

##<a name="download-and-run-the-xamarinforms-solution"></a>Stáhněte si a spusťte Xamarin.Forms řešení

Tady máte několik možností. Stáhnout řešení pro Mac a otevřete ji v Xamarin Studio nebo můžete stáhnout řešení na počítači s Windows a potom ho otevřete v aplikaci Visual Studio pomocí síťových Mac pro vytváření aplikace IOS. Pokud potřebujete podrobnější pokyny týkající se nastavení scénářů Xamarin, najdete v článku [Nastavení a nainstalujte Visual Studiu a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) .

Pojďme pokračovat:

 1. Na počítači Mac nebo na počítači s Windows, otevřete [Portál Azure] v okně prohlížeče.
 2. Na zásuvné nastavení pro mobilní aplikaci, klikněte na **Začínáme** (mobilní) > **Xamarin.Forms**. V kroku 3 klikněte na **vytvořit novou aplikaci** , pokud už není vybraná.  Pak klikněte na tlačítko **Stáhnout** .

    To je možné stáhnout projekt, který obsahuje klientská aplikace, která je připojený k mobilní aplikace. Uložit soubor mu tuhle zkomprimovanou projektu do místního počítače a poznamenejte si kam ukládat.

 3. Extrahování projekt, který jste stáhli a otevřete si ho v Xamarin Studio nebo Visual Studio.

    ![][9]

    ![][8]


##<a name="optional-run-the-ios-project"></a>(Volitelné) Spusťte iOS project

Tato část je určená pro systém iOS projektu Xamarin pro zařízení s iOS. Pokud nepracujete s zařízení s iOS, můžete tuto část přeskočit.

####<a name="in-xamarin-studio"></a>V Xamarin Studio

1. Klikněte pravým tlačítkem myši na iOS projekt a potom klikněte na **Nastavit jako spuštění aplikace Project**.
2. V nabídce **Spustit** klikněte na **Spustit ladění** sestavovat projektu a spusťte aplikaci v emulátoru iPhone.

####<a name="in-visual-studio"></a>Ve Visual Studiu
1. Klikněte pravým tlačítkem iOS projektu a potom klikněte na **nastavit jako projekt při spuštění**.
2. V nabídce **vytvořit** klikněte na **Správce konfigurace systému**.
3. V dialogovém okně **Správce konfigurace systému** zaškrtněte políčka **Vytvoření** a **nasazení** projektu iOS.
4. Stisknutím klávesy **F5** sestavení projektu a spusťte aplikaci v emulátoru iPhone.

    >[AZURE.NOTE] Pokud máte problémy budování, spusťte NuGet Správce balíčků a aktualizaci na nejnovější verzi aplikace Xamarin podporují balíčků. Rychlý úvod projekty někdy může zpoždění v aktualizaci na nejnovější.    

V seznamu aplikací, zadejte smysluplný text, například _Informace Xamarin_ a klikněte **+** tlačítko.

![][10]

To, odešle žádost o příspěvek novou back-end mobilní aplikace hostované v Azure. Data z žádost je vložen do tabulky TodoItem. Back-end mobilní aplikace jsou vráceny položky uložené v tabulce a data se zobrazí v seznamu.

>[AZURE.NOTE]
> Zjistíte, kód, který má přístup k back-end vaše mobilní aplikace v souboru TodoItemManager.cs C# projektu knihovny přenosných tříd řešení.

##<a name="optional-run-the-android-project"></a>(Volitelné) Spuštění Androidem projektu

Tato část je určená pro spuštění projektu droid Xamarin pro Android. Pokud nepracujete s zařízení s Androidem, můžete tuto část přeskočit.

####<a name="in-xamarin-studio"></a>V Xamarin Studio

1. Klikněte pravým tlačítkem myši na Android projekt a potom klikněte na **Nastavit jako spuštění aplikace Project**.
2. V nabídce **Spustit** klikněte na **Spustit ladění** sestavovat projektu a spusťte aplikaci v emulátor Android.

####<a name="in-visual-studio"></a>Ve Visual Studiu
1. Klikněte pravým tlačítkem myši na projekt Android (Droid) a klepněte na tlačítko **nastavit jako projekt při spuštění**.
4. V nabídce **vytvořit** klikněte na **Správce konfigurace systému**.
5. V dialogovém okně **Správce konfigurace systému** zaškrtněte políčka **Vytvoření** a **nasazení** projektu, Android.
6. Stisknutím klávesy **F5** budování projektu a spusťte aplikaci v emulátor Android.

    >[AZURE.NOTE] Pokud máte problémy budování, spusťte NuGet Správce balíčků a aktualizaci na nejnovější verzi aplikace Xamarin podporují balíčků. Rychlý úvod projekty někdy může zpoždění v aktualizaci na nejnovější.    


V seznamu aplikací, zadejte smysluplný text, například _Informace Xamarin_ a klikněte **+** tlačítko.

![][11]

To, odešle žádost o příspěvek novou back-end mobilní aplikace hostované v Azure. Data z žádost je vložen do tabulky TodoItem. Back-end mobilní aplikace jsou vráceny položky uložené v tabulce a data se zobrazí v seznamu.

> [AZURE.NOTE]
> Zjistíte, kód, který má přístup k back-end vaše mobilní aplikace v souboru TodoItemManager.cs C# projektu knihovny přenosné třídy řešení.


##<a name="optional-run-the-windows-project"></a>(Volitelné) Spuštění Windows projektu


Tento oddíl je pro spuštění Xamarin WinApp projektu pro zařízení s Windows. Pokud nepracujete s zařízení s Windows, můžete tuto část přeskočit.


####<a name="in-visual-studio"></a>Ve Visual Studiu
1. Klikněte pravým tlačítkem na některou z Windows projekty a klikněte na **nastavit jako po spuštění projektu**.
4. V nabídce **vytvořit** klikněte na **Správce konfigurace systému**.
5. V dialogovém okně **Správce konfigurace systému** zaškrtněte políčka **Vytvoření** a **nasazení** projektu Windows, který jste zvolili.
6. Stisknutím klávesy **F5** sestavení projektu a spuštění aplikace ve Windows emulátoru.

    >[AZURE.NOTE] Pokud máte problémy budování, spusťte NuGet Správce balíčků a aktualizaci na nejnovější verzi aplikace Xamarin podporují balíčků. Rychlý úvod projekty někdy může zpoždění v aktualizaci na nejnovější.    


V seznamu aplikací, zadejte smysluplný text, například _Informace Xamarin_ a klikněte **+** tlačítko.

To, odešle žádost o příspěvku novou back-end mobilní aplikace hostované v Azure. Data z žádost je vložen do tabulky TodoItem. Back-end mobilní aplikace jsou vráceny položky uložené v tabulce a data se zobrazí v seznamu.

![][12]

> [AZURE.NOTE]
> Zjistíte, kód, který má přístup k back-end vaše mobilní aplikace v souboru TodoItemManager.cs C# projektu knihovny přenosné třídy řešení.

##<a name="next-steps"></a>Další kroky

* [Přidání ověřování aplikace](app-service-mobile-xamarin-forms-get-started-users.md)  
Informace o ověřování uživatelů aplikace se zprostředkovatelem identit.

* [Přidání nabízených oznámení pro aplikace](app-service-mobile-xamarin-forms-get-started-push.md)  
Naučte se přidávat nabízená oznámení podpora k aplikaci a konfigurace back-end vaše mobilní aplikaci pro použití Azure oznámení rozbočovače k odeslání nabízená oznámení.

* [Povolení offline synchronizace aplikace](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Naučte se přidávat aplikace pomocí mobilní aplikace backendovou podpora offline režimu. Offline synchronizace umožňuje koncovým uživatelům chcete provést interakci s mobilní aplikaci&mdash;zobrazení, přidání nebo úprava dat&mdash;i v případě žádné síťové připojení.

* [Používání spravovaných klienta pro Azure mobilní aplikace](app-service-mobile-dotnet-how-to-use-client-library.md)  
Informace o práci s spravovaných klienta SDK v aplikaci Xamarin. 


<!-- Anchors. -->
[Getting started with mobile app backends]:#getting-started
[Create a new mobile app backend]:#create-new-service
[Next Steps]:#next-steps


<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart.png
[8]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-vs.png
[9]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-xs.png
[10]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-android.png
[12]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-windows.png


<!-- URLs. -->
[Visual Studio Professional 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Mobile app SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure portálu]: https://portal.azure.com/

