<properties
    pageTitle="Přidat nabízená oznámení pro aplikace Xamarin.Android | Azure aplikace služby"
    description="Naučte se používat aplikaci služby Azure a Azure oznámení rozbočovače odešlete aplikace Xamarin.Android nabízená oznámení"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-xamarinandroid-app"></a>Přidání aplikace Xamarin.Android nabízená oznámení

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>Základní informace


V tomto kurzu nabízená oznámení přidejte do projektu [Xamarin.Android rychlý start](app-service-mobile-windows-store-dotnet-get-started.md) tak, aby nabízená oznámení odeslaný do zařízení pokaždé, když je vložen záznamu.

Pokud nepoužíváte project serveru stažený úvodní, budete potřebovat balíček nabízená oznámení rozšíření. Další informace najdete v článku [práce s back-end serveru .NET SDK Azure mobilních aplikací](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .


##<a name="prerequisites"></a>Zjistit předpoklady pro

Tento kurz vyžaduje:

+ Aktivní účet Google. Můžete se přihlásit k účtu Google na [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).
+ [Google Cloud zpráv součást klienta](http://components.xamarin.com/view/GCMClient/).

##<a name="configure-hub"></a>Konfigurace rozbočovači oznámení

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

##<a id="register"></a>Povolení Firebase cloudu zasílání zpráv

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

##<a name="configure-azure-to-send-push-requests"></a>Konfigurace Azure odešlou připínáčku

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

##<a id="update-server"></a>Aktualizace project serveru odeslat nabízená oznámení

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##<a id="configure-app"></a>Konfigurace projekt klienta nabízených oznámení

[AZURE.INCLUDE [mobile-services-xamarin-android-push-configure-project](../../includes/mobile-services-xamarin-android-push-configure-project.md)]

##<a id="add-push"></a>Přidání kódu pro nabízená oznámení aplikace

[AZURE.INCLUDE [app-service-mobile-xamarin-android-push-add-to-app](../../includes/app-service-mobile-xamarin-android-push-add-to-app.md)]

## <a name="test"></a>Test nabízená oznámení v aplikaci

Aplikaci můžete otestovat pomocí virtuální zařízení v emulátor. Existují další konfiguraci kroky při používání s emulátoru.

1. Ujistěte se, že při nasazení nebo ladění na virtuální zařízení, které má rozhraní API Google nastavit jako cíle, jak je znázorněno níže ve Správci zařízení Android virtuální (AVD).

    ![](./media/app-service-mobile-xamarin-android-get-started-push/google-apis-avd-settings.png)

2. Přidejte účet Google do zařízení s Androidem kliknutím **aplikace** > **Nastavení** > **Přidat účet**podle zobrazených pokynů.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/add-google-account.png)

3. Aplikaci pro seznam úkolů jako před a vložte nového položky úkolu. Tentokrát, ikona upozornění se zobrazí v oznamovací oblasti. Můžete otevřít okraj oznámení zobrazit celý text oznámení.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/android-notifications.png)


<!-- URLs. -->
[Xamarin.Android quick start]: app-service-mobile-xamarin-android-get-started.md
[Google Cloud Messaging Client Component]: http://components.xamarin.com/view/GCMClient/
[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
