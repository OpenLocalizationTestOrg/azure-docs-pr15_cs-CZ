<properties
    pageTitle="Upřesnit konfiguraci Azure Mobile zapojení Android SDK"
    description="Jsou uvedeny dostupné Upřesnit možnosti včetně Android Manifest s Azure Mobile zapojení Android SDK"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-configuration-for-azure-mobile-engagement-android-sdk"></a>Upřesnit konfiguraci Azure Mobile zapojení Android SDK

> [AZURE.SELECTOR]
- [Univerzální Windows](mobile-engagement-windows-store-advanced-configuration.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-advanced-configuration.md)

Tento postup popisuje, jak nakonfigurovat možnosti konfigurace Azure Mobile zapojení Android aplikací.

## <a name="prerequisites"></a>Zjistit předpoklady pro

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="permission-requirements"></a>Požadavky na oprávnění
Některé možnosti vyžadují určitá oprávnění, které tady jsou vypsané pro odkaz a v řádku v určité funkce. Bezprostředně před datem start_date nebo po přidání těchto oprávnění k AndroidManifest.xml projektu `<application>` značku.

Kód oprávnění musí vypadat takto, kde vyplňte příslušná oprávnění v následující tabulce.

    <uses-permission android:name="android.permission.[specific permission]"/>


| Oprávnění | Při použití |
| ---------- | --------- |
| INTERNET | Povinné. Pro základní sestavy |
| ACCESS_NETWORK_STATE | Povinné. Pro základní sestavy |
| RECEIVE_BOOT_COMPLETED | Povinné. Chcete-li zobrazit oznámení centra po restartování zařízení |
| WAKE_LOCK | Doporučené. Umožňuje shromažďování dat při používání Wi-Fi nebo když obrazovku |
| VIBRACE | Volitelné. Umožňuje vibrace po přijetí upozornění |
| DOWNLOAD_WITHOUT_NOTIFICATION | Volitelné. Umožňuje oznámení Android celkového rámce |
| WRITE_EXTERNAL_STORAGE | Volitelné. Umožňuje oznámení Android celkového rámce |
| ACCESS_COARSE_LOCATION | Volitelné. Umožňuje vytváření sestav v reálném čase umístění |
| ACCESS_FINE_LOCATION | Volitelné. Umožňuje vytváření sestav na základě GPS umístění |

Počínaje Android M [některá oprávnění jsou spravovány za běhu](mobile-engagement-android-location-reporting.md#Android-M-Permissions).

Pokud už používáte ``ACCESS_FINE_LOCATION``, pak nemusíte použít i ``ACCESS_COARSE_LOCATION``.

## <a name="android-manifest-configuration-options"></a>Možnosti Android seznamu konfigurace

### <a name="crash-report"></a>Zpráva o selhání

Zakázání pád sestavy, přidat tento kód mezi `<application>` a `</application>` značky:

    <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a>Mezní hodnota požadavků

Ve výchozím nastavení protokoly sestavy využití služeb v reálném čase. Pokud protokoly sestavy aplikace liší často, je lepší vyrovnávací paměť protokoly a informovat v celém dokumentu na pravidelných časových základní (se jí říká "Shlukový režim"). Postup přidání tento kód mezi `<application>` a `</application>` značky:

    <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

Režim požadavků mírně zvyšuje životnost battery ale má vliv na monitoru můžete zapojit: všechny relace a úlohy doba trvání zaokrouhleny mezní požadavků (tedy relace a úlohy kratší než mezní hodnota požadavků nemusí být vidět). Mezní hodnota požadavků by měl být delší než 30000 (30s).

### <a name="session-timeout"></a>Časový limit relace

 Ukončení aktivitu stisknutím klávesy **pro domácnosti** nebo **zpět** , nastavením nedělá telefon nebo přechod do jiné aplikace. Ve výchozím nastavení se ukončí relaci deset sekund po skončení poslední aktivity. Tím se zabrání rozdělení relace pokaždé, když uživatel zavře a znovu vrátí aplikaci rychle, které můžete objevil když uživatel vyzvedne obrázku, kontroluje oznámení atd. Je vhodné upravit tento parametr. Postup přidání tento kód mezi `<application>` a `</application>` značky:

    <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

## <a name="disable-log-reporting"></a>Zakázání protokol

### <a name="using-a-method-call"></a>Pomocí metody hovoru

Pokud budete potřebovat zapojení zastavit odesílání protokoly, můžete volat:

    EngagementAgent.getInstance(context).setEnabled(false);

Volání je trvalý: použije souboru sdíleného předvolby.

Pokud zapojení je aktivní, při volání na tuto funkci, může to trvat jednu minutu zastavení služby. Však ho nespustí službu vůbec při příštím spuštění aplikace.

Povolit vykazování znovu tak, že zavoláte stejnou funkci pomocí protokolu `true`.

### <a name="integration-in-your-own-preferenceactivity"></a>Integrace v vlastní`PreferenceActivity`

Místo volání tuto funkci, můžete také integrovat toto nastavení používají přímo ve vašem stávajícím `PreferenceActivity`.

Konfigurace zapojení používat předvolby soubor (požadované režimu) v `AndroidManifest.xml` soubor s `application meta-data`:

-   `engagement:agent:settings:name` Klíče slouží k definování název souboru s sdílených předvolby.
-   `engagement:agent:settings:mode` Klíče slouží k definování režimu souboru sdíleného předvolby. Použití režimu stejné jako u svého `PreferenceActivity`. Režim musí předané jako čísla: Pokud používáte kombinací konstantní příznaků v kódu, zkontrolujte celkové hodnoty.

Zapojení vždy používá `engagement:key` boolean klíč souboru aplikace na předvolby pro správu toto nastavení.

Následující příklad `AndroidManifest.xml` zobrazí výchozí hodnoty:

    <application>
        [...]
        <meta-data
          android:name="engagement:agent:settings:name"
          android:value="engagement.agent" />
        <meta-data
          android:name="engagement:agent:settings:mode"
          android:value="0" />

Pak můžete přidat `CheckBoxPreference` předvoleb rozložení jako je následující:

    <CheckBoxPreference
      android:key="engagement:enabled"
      android:defaultValue="true"
      android:title="Use Engagement"
      android:summaryOn="Engagement is enabled."
      android:summaryOff="Engagement is disabled." />
