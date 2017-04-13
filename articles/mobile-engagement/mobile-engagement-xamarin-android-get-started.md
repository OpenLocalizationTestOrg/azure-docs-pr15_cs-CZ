<properties
    pageTitle="Začínáme s Azure mobilní zapojení pro Xamarin.Android"
    description="Zjistěte, jak pomocí služby Azure Mobile zapojení technologie pro analýzu a nabízená oznámení pro aplikace Xamarin.Android."
    services="mobile-engagement"
    documentationCenter="xamarin"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="06/16/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-xamarinandroid-apps"></a>Začínáme s Azure mobilní zapojení Xamarin.Android aplikací

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Toto téma ukazuje, jak používat Azure Mobile zapojení pochopit použití aplikace a jak odeslat nabízená oznámení Segmentovaný uživatelům aplikace Xamarin.Android.
Tento kurz ukazuje jednoduchý scénář vysílání pomocí mobilní Engagement. Vytvořte z prázdné aplikace Xamarin.Android, který shromažďuje základní data volání a přijímání nabízená oznámení pomocí zasílání zpráv Cloud Google (GCM).

Tento kurz vyžaduje:

+ [Xamarin Studio](http://xamarin.com/studio). Visual Studio můžete používat taky s Xamarin, ale tento kurz používá Xamarin Studio. Pokyny k instalaci najdete v článku [Nastavení a instalace for Visual Studio a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).
+ [Mobilní zapojení Xamarin SDK](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [AZURE.NOTE] Tento kurz, musíte mít účet Azure active. Pokud nemáte účet, můžete vytvořit bezplatný účet zkušební v jenom pár minut. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-android-get-started).

##<a id="setup-azme"></a>Nastavení mobilních zapojení pro aplikace pro Android

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Připojení k back-end zapojení mobilní aplikace

Tento kurz nabízí "základní integraci", což je sadu minimální potřebná k shromažďování dat a odeslání nabízená oznámení. 

Vytvoříme základní aplikace s Xamarin Studio prokázat integrace.

###<a name="create-a-new-xamarinandroid-project"></a>Vytvoření nového projektu Xamarin.Android

1. Spuštění **Xamarin Studio** přejděte na **soubor** -> **nové** -> **řešení** 

    ![][1]

2. Vyberte **Aplikace Androidu** a ujistěte se, že vybraném jazyce **C#** a klikněte na tlačítko **Další**.

    ![][2]

3. Zadejte **Název aplikace** a **identifikátor organizace**. Nejdřív musíte mít zaškrtnutí **Google Play Services** a klikněte na tlačítko **Další**. 

    ![][3]
    
4. V případě potřeby aktualizujte **Název projektu**, **Řešení název** a **umístění** a klikněte na **vytvořit**.

    ![][4]
 
Xamarin Studio vytvoří aplikace, ve kterém jsme integrace Mobile Engagement. 

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Připojení aplikace k mobilní zapojení back-end

1. Klikněte pravým tlačítkem myši na složku **balíčků** ve windows řešení a vyberte **Přidat balíčků**

    ![][5]

2. Hledání v **Aplikaci Microsoft Azure Mobile zapojení Xamarin SDK** a přiřadit ji někomu řešení.  

    ![][6]
   
3. Otevřete **MainActivity.cs** a přidejte následující pomocí příkazů:

        using Microsoft.Azure.Engagement;
        using Microsoft.Azure.Engagement.Activity;

4. V `OnCreate` metody, přidejte následující inicializace připojení k mobilní zapojení back-end. Ujistěte se, že přidáte **ConnectionString**. 

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.ConnectionString = "YourConnectionStringFromAzurePortal";
        EngagementAgent.Init(engagementConfiguration);

###<a name="add-permissions-and-a-service-declaration"></a>Přidat oprávnění a deklaraci služby

1. Otevřete soubor **Manifest.xml** ve složce vlastnosti. Vyberte kartu Zdroj tak, aby přímo aktualizovat zdroj dat XML.
 
2. Bezprostředně před datem start_date nebo po přidání těchto oprávnění k Manifest.xml (který najdete v části **Vlastnosti** složky) projektu `<application>` značku:

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

3. Přidejte následující mezi `<application>` a `</application>` značky deklarovat agentem:

        <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>"
            android:process=":Engagement"/>

4. Kód vložený, nahraďte `"<Your application name>"` v popisku. Zobrazí se v nabídce **Nastavení** , kde uživatelé můžou uvidět služby v zařízení. Slovo "Služby" můžete přidat například do tohoto popisku.

###<a name="send-a-screen-to-mobile-engagement"></a>Na obrazovce odešlete zapojení Mobile

Abyste mohli začít odesílání dat a zajistit, že jsou aktivní uživatelé, musí aspoň jednu obrazovku poslat back-end Mobile Engagement. To – zkontrolujte, že je `MainActivity` dědí od `EngagementActivity` namísto `Activity`.

    public class MainActivity : EngagementActivity
    
Můžete taky Pokud nemůžete Zdědit `EngagementActivity` musíte přidat a pak `.StartActivity` a `.EndActivity` metody v `OnResume` a `OnPause` v tomto pořadí.  

        protected override void OnResume()
            {
                EngagementAgent.StartActivity(EngagementAgentUtils.BuildEngagementActivityName(Java.Lang.Class.FromType(this.GetType())), null);
                base.OnResume();             
            }
    
            protected override void OnPause()
            {
                EngagementAgent.EndActivity();
                base.OnPause();            
            }

##<a id="monitor"></a>Připojení aplikace s sledování v reálném čase

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Zapnout nabízená oznámení a zasílání zpráv v aplikaci

Zapojení Mobile vám umožní pracovat s nimi a DOSÁHLA vaši uživatelé s nabízená oznámení a aplikace pro zasílání zpráv v rámci kampaní. Tento modul nazývá REACH na portálu Mobile Engagement.
V následujících částech nastaví aplikace k jejich odběru.

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[AZURE.INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[1]: ./media/mobile-engagement-xamarin-android-get-started/1.png
[2]: ./media/mobile-engagement-xamarin-android-get-started/2.png
[3]: ./media/mobile-engagement-xamarin-android-get-started/3.png
[4]: ./media/mobile-engagement-xamarin-android-get-started/4.png
[5]: ./media/mobile-engagement-xamarin-android-get-started/5.png
[6]: ./media/mobile-engagement-xamarin-android-get-started/6.png
