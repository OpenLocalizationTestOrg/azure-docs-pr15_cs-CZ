<properties
    pageTitle="Integrace Azure mobilní zapojení Android SDK"
    description="Nejnovější aktualizace a postupy pro Android SDK pro zapojení Mobile Azure"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />


#<a name="how-to-integrate-adm-with-engagement"></a>Jak integrovat ADM zapojení do

> [AZURE.IMPORTANT] Postup integrace popsaná v dialogu jak chcete integrovat zapojení na Android dokumentu před provedením této příručce.
>
> Tento dokument je užitečná pouze v případě, že již integrovanou Reach modul a budete chtít nabízená Amazon zařízení. Při integraci Reach kampaně v aplikaci, přečtěte si nejprve jak integrovat zapojení na Androidu.

##<a name="introduction"></a>Úvod

Integrace ADM umožňuje aplikacím posune při zaměřené na zařízení Amazon Android.

Datové části ADM Posune SDK vždy obsahovat `azme` klíčů v objektu data. Tedy pokud používáte ADM pro jiné účely v aplikaci, můžete filtrovat posune založené na tuto klávesu.

> [AZURE.IMPORTANT] Pouze Amazon Kindle zařízení s Androidem 4.0.3 nebo nad podporovaných Amazon zařízení zasílání zpráv; však můžete integrovat tento kód bezpečně na jiných zařízeních.

##<a name="sign-up-to-adm"></a>Registrace k ADM

Pokud dosud neučinili, musíte povolit ADM ve vašem účtu Amazon.

Postup podrobné na: [<https://developer.amazon.com/sdk/adm/credentials.html>].

Po dokončení procesu, zobrazí se:

-   OAuth přihlašovacích údajů (ID klienta a tajná klienta) pro zapojení moct nabízená svých zařízeních.
-   Rozhraní API klíč, který musí být integrována do aplikace.

##<a name="sdk-integration"></a>Integrace SDK

### <a name="managing-device-registrations"></a>Správa registrace zařízení

Každý zařízení musí registrace příkaz Odeslat serverům ADM, jinak nelze přejít.

Pokud používáte už [ADM klienta knihovny]a už máte [integrované ADM] můžete přímo přejít na android – sdk-adm – zobrazí.

Pokud jste ještě ADM ještě, můžete zapojit má jednodušší způsob, jak povolit v aplikaci:

Úprava vaší `AndroidManifest.xml` souboru:

-   Přidání názvů Amazon soubor má začít takto:

        <?xml version="1.0" encoding="utf-8"?>
        <manifest xmlns:android="http://schemas.android.com/apk/res/android"
                  xmlns:amazon="http://schemas.amazon.com/apk/res/android"

-   V `<application/>` označení, přidejte tento oddíl:

        <amazon:enable-feature
           android:name="com.amazon.device.messaging"
           android:required="false"/>

        <meta-data android:name="engagement:adm:register" android:value="true" />

-   Po přidání značku amazon, může mít chybu sestavení-li vytvořit cílový projekt pod Android 2.1. Je nutné použít cíl sestavení **Android 2.1 +** (Nedělejte si starosti, lze přesto se vám `minSdkVersion` nastavit až 4).
-   Integrace klávesu rozhraní API ADM jako aktivum následující [postup].

Postupujte podle pokynů v následujících částech.

### <a name="communicate-registration-id-to-the-engagement-push-service-and-receive-notifications"></a>Komunikovat id registrace ke službě zapojení nabízená a oznámení

Abyste komunikovat id registrace zařízení služby nabízená zapojení a jeho zobrazovat oznámení, přidejte následující text do svého `AndroidManifest.xml` souboru uvnitř `<application/>` označení (i když používáte ADM bez zapojení):

        <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
          android:exported="false">
          <intent-filter>
            <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
          </intent-filter>
        </receiver>

         <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
           android:permission="com.amazon.device.messaging.permission.SEND">
          <intent-filter>
            <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
            <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
            <category android:name="<your_package_name>"/>
          </intent-filter>
        </receiver>   

Zajištění máte v těchto oprávnění vaše `AndroidManifest.xml` (před `</application>` značku).

        <uses-permission android:name="android.permission.WAKE_LOCK"/>
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE"/>
        <uses-permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE"/>
        <permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE" android:protectionLevel="signature"/>

##<a name="grant-engagement-oauth-credentials"></a>Přihlašovací údaje OAuth zapojení grant (udělit)

Odeslání OAuth přihlašovacích údajů (ID klienta a tajná klienta) v portálu Engagement.

[< https://developer.amazon.com/sdk/adm/credentials.html>]:https://developer.amazon.com/sdk/adm/credentials.html
[Knihovna ADM klienta]:https://developer.amazon.com/sdk/adm/setup.html
[integrované ADM]:https://developer.amazon.com/sdk/adm/integrating-app.html
[Tento postup]:https://developer.amazon.com/sdk/adm/integrating-app.html#Asset
