<properties
    pageTitle="Vytvoření univerzální Windows platformu (UWP), pomocí mobilní aplikace | Microsoft Azure"
    description="Postupujte podle kurzu, který Začínáme s používáním back-end Azure mobilní aplikace pro vývoj aplikací univerzální platformu systému Windows (UWP) v jazyce C# Visual Basic a JavaScript."
    services="app-service\mobile"
    documentationCenter="windows"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="create-a-windows-app"></a>Vytvoření aplikace pro Windows

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>Základní informace

Tento kurz se dozvíte, jak přidat do back-end cloudové služby do aplikace univerzální platformu systému Windows (UWP). Další informace najdete v tématu [jaké mobilní aplikace](app-service-mobile-value-prop.md). Následující seznam uvádí obrazovky zachycený obsah z aplikace dokončený:

![Dokončení desktopové aplikace](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed-desktop.png)   
Spuštění na ploše. 

![Aplikace dokončený phone](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)  
Spuštění na telefonu

Dokončení tohoto kurzu je požadována pro všechny další mobilní aplikace výukové programy pro UWP aplikace. 

##<a name="prerequisites"></a>Zjistit předpoklady pro

Tento kurz, je potřeba k provedení těchto věcí:

* Účet Azure active. Pokud nemáte účet, můžete zaregistrovat ke zkušební verzi Azure a získat až 10 bezplatná – mobilní aplikace, které můžete nadále používat i po uplynutí zkušebního období. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/).

* [Visual Studio komunity 2015] nebo jeho novější verzí.

>[AZURE.NOTE] Pokud chcete začít pracovat s aplikaci služby Azure před registraci účet Azure, přejděte na [Zkuste aplikaci služby](https://tryappservice.azure.com/?appServiceName=mobile). Můžete okamžitě vytvořit mobilní aplikaci krátkodobý starter v aplikaci služby – bez platebních karet povinné a bez závazky.

##<a name="create-a-new-azure-mobile-app-backend"></a>Vytvořit novou back-end Azure mobilní aplikaci

Tímto postupem vytvořte novou back-end mobilní aplikaci.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Nyní máte zřízení backendovou Azure mobilní aplikaci, něhož aplikacemi mobilního klienta. Pak stáhne projekt server pro jednoduchou "úkol seznam" back-end a publikujte projekt do Azure.

## <a name="configure-the-server-project"></a>Konfigurace project serveru

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

##<a name="download-and-run-the-client-project"></a>Stáhnout a spustit projektu klienta

Po konfiguraci vašeho back-end mobilní aplikaci můžete vytvářet nové aplikace klienta nebo úprava aplikace pro stávající připojení k Azure. V této části stáhnout UWP šablonou projektu aplikace, která používá vlastní nastavení pro připojení k serverové části aplikace Mobile.

1. Po návratu do zásuvné **úvodní** pro mobilní aplikaci backendovou klikněte na **vytvořit novou aplikaci** > **Stáhnout**a pak extrahovat mu tuhle zkomprimovanou projektu soubory do místního počítače.

    ![Stáhnout Windows rychlý úvod projekt](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-app-windows-quickstart.png)

3. (Volitelné) Přidání UWP aplikace project do stejného řešení jako projektového serveru. To usnadňuje ladění a testování aplikace a back-end ve stejném Visual Studio řešení, pokud se rozhodnete k tomu nevyzve. Pokud chcete přidat projekt aplikace UWP řešení, musíte používat Visual Studio 2015 nebo jeho novější verzí.

4. V aplikaci UWP jako projekt při spuštění stisknutím klávesy F5 nasadit a spustit aplikaci.

5. V aplikaci zadejte smysluplný text, například *Dokončeno kurzu*, do textového pole **vložte TodoItem** a potom klikněte na **Uložit**.

    ![Rychlý úvod dokončení počítač s Windows](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-startup.png)

    To, odešle žádost o příspěvek novou back-end mobilní aplikace, který je umístěn v Azure.

6. (Volitelné) Ukončete aplikaci a restartujte na jiné zařízení nebo mobilní emulátoru.

    ![Systém Windows phone dokončení rychlý úvod](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)

    Všimněte si, že se data uložená v předchozím kroku načte z Azure spuštěna aplikace UWP. 

##<a name="next-steps"></a>Další kroky

* [Přidání ověřování aplikace](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  Informace o ověřování uživatelů aplikace se zprostředkovatelem identit.

* [Přidání nabízených oznámení pro aplikace](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  Naučte se přidávat nabízená oznámení podpora k aplikaci a konfigurace back-end vaše mobilní aplikaci pro použití Azure oznámení rozbočovače k odeslání nabízená oznámení.

* [Povolení offline synchronizace aplikace](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Naučte se přidávat aplikace pomocí mobilní aplikace backendovou podpora offline režimu. Offline synchronizace umožňuje koncovým uživatelům chcete provést interakci s mobilní aplikaci&mdash;zobrazení, přidání nebo úprava dat&mdash;i v případě žádné síťové připojení.

<!-- Anchors. -->
<!-- Images. -->
<!-- URLs. -->
[Mobile App SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure portal]: https://portal.azure.com/
[Komunita aplikace Visual Studio 2015]: https://go.microsoft.com/fwLink/p/?LinkID=534203
