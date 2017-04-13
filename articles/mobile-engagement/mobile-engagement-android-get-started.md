<properties
    pageTitle="Začínáme s Androidem zapojení Azure mobilní aplikace"
    description="Zjistěte, jak pomocí služby Azure Mobile zapojení technologie pro analýzu a nabízených oznámení pro Android aplikace."
    services="mobile-engagement"
    documentationCenter="android"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="hero-article"
    ms.date="08/10/2016"
    ms.author="piyushjo;ricksal" />

# <a name="get-started-with-azure-mobile-engagement-for-android-apps"></a>Začínáme s zapojení Azure Mobile pro Android aplikace

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Toto téma ukazuje, jak používat Azure Mobile zapojení pochopit použití aplikace a jak odeslat nabízená oznámení Segmentovaný uživatelům aplikace Android.
Tento kurz ukazuje jednoduchý scénář vysílání pomocí mobilní Engagement. Vytvořte z prázdné aplikace Android, která shromažďuje základní data volání a přijímání nabízená oznámení pomocí zasílání zpráv Cloud Google (GCM).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Dokončení tohoto kurzu vyžaduje [Android nástrojů pro vývojáře](https://developer.android.com/sdk/index.html), zahrnující integrované vývojové prostředí Android Studio a platformu nejnovější Android.

Také vyžaduje [Mobile Engagement Android SDK](https://aka.ms/vq9mfn).

> [AZURE.IMPORTANT] Pro dokončení tohoto kurzu, třeba účet Azure active. Pokud nemáte účet, můžete vytvořit bezplatný účet zkušební v jenom pár minut. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-android-get-started).

## <a name="set-up-mobile-engagement-for-your-android-app"></a>Nastavte si zapojení mobilní aplikace pro Android

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a name="connect-your-app-to-the-mobile-engagement-backend"></a>Připojení k back-end zapojení mobilní aplikace

Tento kurz nabízí "základní integraci", což je sadu minimální potřebná k shromažďování dat a odeslání nabízená oznámení. Vytvoření základní aplikace s Androidem Studio prokázat integrace.

Dokumentace dokončení integrace najdete v [mobilní zapojení Android SDK integrace](mobile-engagement-android-sdk-overview.md).

### <a name="create-an-android-project"></a>Vytvoření Android projektu

1. Otevřete **Android studiu**a v místní nabídce vyberte **zahájení nového projektu Android Studio**.

    ![][1]

2. Zadejte doménu aplikaci název a společnosti. Poznamenejte jsou vyplňování, protože později potřebovat. Klikněte na tlačítko **Další**.

    ![][2]

3. Vyberte cílový uspořádání a rozhraní API úroveň a klikněte na tlačítko **Další**.

    >[AZURE.NOTE] Zapojení Mobile vyžaduje rozhraní API úroveň 10 minimální (Android 2.3.3).

    ![][3]

4. Vyberte **Prázdnou aktivity** , což je obrazovce jenom pro tuto aplikaci a klikněte na tlačítko **Další**.

    ![][4]

5. Nakonec nechejte výchozí hodnoty a klikněte na tlačítko **Dokončit**.

    ![][5]

Android Studio teď vytvoří aplikaci ukázku kam jsme integrace Mobile Engagement.

### <a name="include-the-sdk-library-in-your-project"></a>Vložit SDK knihovny do projektu

1. Stáhněte si [SDK Android mobilní Engagement](https://aka.ms/vq9mfn).
2. Extrahování archiv do složky ve vašem počítači.
3. Rozpoznání knihovnu .jar pro aktuální verzi tohoto SDK a zkopírujte do schránky.

      ![][6]

4. Přejděte do části **projektu** (1) a vložte .jar ve složce knihoven (2).

      ![][7]

5. Načíst knihovnu, synchronizujte projektu.

      ![][8]

### <a name="connect-your-app-to-mobile-engagement-backend-with-the-connection-string"></a>Připojení aplikace do back-end Mobile zapojení pomocí připojovacího řetězce

1. Zkopírujte následující řádky kódu do vytvoření aktivity (musí udělat jenom na jednom místě aplikace, obvykle hlavní aktivity). Pro tuto aplikaci ukázkové otevřít tak MainActivity v části src -> hlavní -> java složky a přidejte následující text:

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
        EngagementAgent.getInstance(this).init(engagementConfiguration);

2. Odkazy na vyřešíte tak, že stisknete kombinaci kláves Alt + Enter nebo přidání následující příkazy pro import:

        import com.microsoft.azure.engagement.EngagementAgent;
        import com.microsoft.azure.engagement.EngagementConfiguration;

3. Vraťte se do portálu klasické Azure na stránce vaše aplikace **Informace o připojení** a zkopírujte **Připojovací řetězec**.

      ![][9]

4. Vložte je do `setConnectionString` parametr nahrazení celý řetězec ukazuje tento kód:

        engagementConfiguration.setConnectionString("Endpoint=my-company-name.device.mobileengagement.windows.net;SdkKey=********************;AppId=*********");

### <a name="add-permissions-and-a-service-declaration"></a>Přidat oprávnění a deklaraci služby

1. Přidání těchto oprávnění k Manifest.xml projektu těsně před datem start_date nebo po `<application>` značku:

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

2. Chcete-li deklarovat agentem, přidejte tento kód mezi `<application>` a `</application>` značky:

        <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>"
            android:process=":Engagement"/>

3. Kód vložený, nahraďte `"<Your application name>"` v popisku, který se zobrazí v nabídce **Nastavení** , kde navíc přehledně uvidíte služby v zařízení. Slovo "Služby" můžete přidat například do tohoto popisku.

### <a name="send-a-screen-to-mobile-engagement"></a>Na obrazovce odešlete zapojení Mobile

Zahájení odesílání dat a zajistit, aby uživatelé aktivní, musí aspoň jednu obrazovku (aktivita) poslat Mobile zapojení back-end.

Přejděte na **MainActivity.java** a přidejte následující nahrazení základní třídy **MainActivity** **EngagementActivity**:

    public class MainActivity extends EngagementActivity {

> [AZURE.NOTE] Nejsou-li základní třídy *aktivity*, použijte [Rozšířené Android vykazování](mobile-engagement-android-advanced-reporting.md#modifying-your-codeactivitycode-classes) informace o dědit z různých tříd.


Vkládání komentářů se na následující řádek pro tento scénář jednoduchý příklad:

    // setSupportActionBar(toolbar);

Pokud chcete, aby `ActionBar` ve své aplikaci najdete v článku [Upřesnit Android vytváření sestav](mobile-engagement-android-advanced-reporting.md#modifying-your-codeactivitycode-classes).

## <a name="connect-app-with-real-time-monitoring"></a>Připojení aplikace s sledování v reálném čase

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a name="enable-push-notifications-and-in-app-messaging"></a>Zapnout nabízená oznámení a zasílání zpráv v aplikaci

Během s kampaní zapojení Mobile vám umožní pracovat s nimi a dosáhnout uživatelů se nabízená oznámení a zasíláním zpráv v aplikaci. Tento modul nazývá REACH na portálu Mobile Engagement.
V následující části nastaví aplikace k jejich odběru.

### <a name="copy-sdk-resources-in-your-project"></a>Zkopírujte SDK zdrojů v projektu

1. Navigovat zpátky ke stahování obsahu SDK a zkopírujte složce **rozlišení** .

    ![][10]

2. Přejděte zpátky na Android Studio vyberte **hlavního** adresáře soubory projektu a potom je vložte do přidání zdrojů do projektu.

    ![][11]

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[AZURE.INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

## <a name="next-steps"></a>Další kroky

Přejděte na [Android SDK](mobile-engagement-android-sdk-overview.md) zobrazíte podrobné údaje o SDK integration.

<!-- Images. -->
[1]: ./media/mobile-engagement-android-get-started/android-studio-new-project.png
[2]: ./media/mobile-engagement-android-get-started/android-studio-project-props.png
[3]: ./media/mobile-engagement-android-get-started/android-studio-project-props2.png
[4]: ./media/mobile-engagement-android-get-started/android-studio-add-activity.png
[5]: ./media/mobile-engagement-android-get-started/android-studio-activity-name.png
[6]: ./media/mobile-engagement-android-get-started/sdk-content.png
[7]: ./media/mobile-engagement-android-get-started/paste-jar.png
[8]: ./media/mobile-engagement-android-get-started/sync-project.png
[9]: ./media/mobile-engagement-android-get-started/app-connection-info-page.png
[10]: ./media/mobile-engagement-android-get-started/copy-resources.png
[11]: ./media/mobile-engagement-android-get-started/paste-resources.png
