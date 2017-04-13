<properties
    pageTitle="Azure aplikace ukázku Mobile zapojení | Microsoft Azure"
    description="Popisuje, odkud si můžete stáhnout, jak používat a výhody používání aplikace ukázku zapojení Mobile Azure"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/10/2016"
    ms.author="piyushjo" />

# <a name="azure-mobile-engagement-demo-app"></a>Azure aplikace ukázku zapojení Mobile

Jsme jste publikovali aplikace Azure Mobile zapojení ukázku pro **iOS**, **Android**a platformách **Windows** můžete najít užitečné zdroje a další informace o mobilní Engagement.

Aplikace umožňuje:

- Užitečné odkazy, které můžete zapojit Mobile zdrojů, jako jsou videa, si přečtěte následující dokumentaci, fórum podpory a kam se obrátit na vysokoškolskou úroveň poslat žádost o funkci snadno najdete.
- Možnosti výběru oznámení ve formě podporovaných službami zapojení Mobile, abyste získali představu pro mobilní aplikace.
- Pomocí odkazu implementaci zkoumá jak implementovat Mobile zapojení do vlastní aplikace. Další informace pro:

    - Shromáždit data analýzy.
    - Implementace složitých oznámení typů například *vkládaná celou obrazovku* nebo *místní*.
    - Implementace průzkumů a hlasování.
    - Implementace pasivní nabízených dat a nabízených scénáře.   

## <a name="app-installation"></a>Instalace aplikace
Tato aplikace je k dispozici v následujících úložištích aplikace:

- **Windows univerzální ukázku aplikace**:

    - Stáhněte si ji na na [uložení aplikace pro Windows](https://www.microsoft.com/en-us/store/apps/azure-mobile-engagement/9nblggh4qmh2).
    - Aplikaci vyvinutý jako Windows 10 univerzální aplikace. Zdrojový kód je k dispozici na [GitHub](https://github.com/Azure/azure-mobile-engagement-app-windows).

- **iOS Ukázka aplikace**:

    - Stáhněte si ji na [Apple ukládat](https://itunes.apple.com/us/app/azure%20mobile%20engagement/id1105090090).
    - Aplikaci vyvinutý v iOS Swift. Zdrojový kód je k dispozici na [GitHub](https://github.com/Azure/azure-mobile-engagement-app-ios).

- **Android ukázku aplikace**:

    - Stáhněte si aplikaci v [Google Play ukládat](https://play.google.com/store/apps/details?id=com.microsoft.azure.engagement).
    - Zdrojový kód je k dispozici na [GitHub](https://github.com/Azure/azure-mobile-engagement-app-android).

![Windows univerzální aplikace ukázka][1]

![iOS Ukázka aplikace][2]
![Android ukázku aplikace][3]


## <a name="usage"></a>Použití

Tato aplikace můžete použít následujícími způsoby:

**Stažení aplikace na svém zařízení z aplikace uložit odkazy (dříve):**

>[AZURE.IMPORTANT] Nemusíte Azure klientem nebo třeba připojení aplikace do back-end. Aplikace funguje nezávisle na sobě.

- Po aplikace ve vašem zařízení, pak přejdete kliknutím na odkazy v nabídce vlevo možnost najít užitečné zdroje informací o využití Mobile.
- Přidali jsme [informačního kanálu RSS služby](https://aka.ms/azmerssfeed) do této aplikace, aby se vždy aktualizovat informace o nejnovějších aktualizací produktů.
- Můžete taky absolvovat ukázkové situace oznámení projevovat nadměrná typu upozornění, které jsou podporovány zapojení Mobile pro každou platformu. Toto oznámení může dojít místně – to znamená, můžete kliknutím na tlačítka na obrazovkách zobrazit návyků oznámení, které je stejná jako odesílání oznámení z platformu Mobile Engagement.

![Nabídka aplikace pro Windows][4]

![Nabídka aplikace pro iOS][5]
![nabídky aplikace pro Android][6]

**Zdrojový kód stahování GitHub odkazů (dříve):**

- Po stažení zdrojového kódu, otevřete ho v příslušných vývojové prostředí – XCode pro iOS, Android Studio pro Android a Visual Studio pro Windows.
- Pak postupujte podle naše [základní kroky integrace SDK](mobile-engagement-windows-store-dotnet-get-started.md) tak, aby se vám povede připojení tuto aplikaci k vlastní instanci back-end zapojení Mobile.
    - Potřebujete konfigurovat připojovacího řetězce v aplikaci.
    - Potřebujete konfigurovat platformu nabízených oznámení pro aplikace.
- Zjistíte, že samotnou aplikaci využívá s Mobile Engagement. Proto při otevření aplikace po připojení k back-end si budete moct zobrazit relace uživatele, aktivity, událostí a tak dále, na kartě **Monitor** .
- Můžete taky měli o neodeslání oznámení pro tuto aplikaci z vlastní Mobile zapojení instance místo použití místní oznámení.
    - Sem můžete přidat zařízení jako test zařízení pomocí položku **získání ID zařízení** v aplikaci. To vám zaregistrujte jako test zařízení s instanci aplikace platformy back-end ID zařízení.

    ![ID zařízení ve Windows][7]

    ![ID zařízení IOS][8]
    ![ID zařízení na Android][9]

## <a name="key-features-of-the-demo-app"></a>Klíčové funkce aplikace ukázka

- Jak jsme zmínili dříve, s touto aplikací máte všechny klíčové zdroje pro mobilní zasunutí v ruce. Můžete procházet na odkazy v nabídce nalevo.

- Oznámení o nepřítomnosti v aplikaci pro každou platformu můžete zaznamenat. Toto oznámení můžete doručit jako **pouze oznámení**, pokud kliknete na oznámení jednoduše se objeví nativní obrazovce aplikace (pomocí **hloubkové propojení**) – nebo **Web oznámení**, kde můžete dodat dalšího obsahu HTML z mobilní telefon zapojení back-end zobrazený po kliknutí na upozornění.

    ![Oznámení o nepřítomnosti v aplikaci][29]

- IOS budete muset zavřete aplikaci zobrazíte nabízená oznámení mimo aplikaci nebo systému. Můžete si může samostatně prohlížet implementaci tady pro přidání **tlačítka akcí**, jako jsou ty, které se přidají do toto oznámení mimo aplikace pro *zpětnou vazbu* a *sdílet* (tak, že uživatel může trvat akce přímo z vlastního oznámení).

    ![Oznámení o nepřítomnosti v aplikaci na iOS][11] ![Zobrazit oznámení o nepřítomnosti v aplikaci na iOS][14]

- Možnosti, které jsou podporovány na Androidu, jsou přidáním víceřádkového textu (**Velký Text**) nebo obrázek oznámení (**Celkového rámce**) na oznámení, spolu s **tlačítky akcí** (podle nepodporuje iOS).

    ![Oznámení o nepřítomnosti v aplikaci na Androidu][12] ![Zobrazit oznámení o nepřítomnosti v aplikaci na Androidu][15]

- Ve Windows 10 zobrazí se vzhled oznámení v počítači. Toto oznámení také zobrazí ve Windows 10 **Centrum oznámení**. Neexistuje žádná podpora pro přidání **tlačítka akcí** v okamžiku, kdy Windows SDK.

    ![Oznámení o nepřítomnosti v aplikaci ve Windows][10] ![Zobrazení mimo aplikaci ve Windows][13]

- Můžete zaznamenat výchozí "v aplikaci" oznámení pro každou platformu. Toto je setkat i v případě ve dvou krocích, kde okno s **upozorněním** je zobrazen jako první. Když kliknete, otevře se nahoru o celou obrazovku **oznámení**, jak jsou zobrazeny v následující snímek. Obsah tohoto oznámení pochází z back-end instanci aplikace Mobile Engagement. V SDK obsahuje šablony pro oznámení a oznámení. Můžete snadno upravit, jak je uvedeno v této aplikace ukázku přidání naši stránku věnovanou logo a barvy.  

    ![V aplikaci upozornění v systému Windows][16]

    ![Oznámení v aplikace v iOS][17]  ![Oznámení v aplikaci na Androidu][18]

    **iOS**, **Android**

- Můžete taky použít funkci **kategorie** přijetí Mobile přizpůsobit tento výchozí. V aplikaci ukázku jsme jste prokázáno dva běžné způsoby, jak změnit možnosti upozornění. Všimněte si, že funkce kategorie ještě nepodporuje sady Windows SDK.

    **Celou obrazovku vkládaná:**

    ![Oznámení v aplikaci – vkládaná kategorie][30]

    ![Vkládaná kategorie v iOS][21]  ![Vkládaná kategorie na Androidu][22]

    **Automaticky otevírané oznámení:**

    ![Oznámení v aplikaci – místní kategorie][31]

    ![Automaticky otevírané oznámení na iOS][19]   ![Automaticky otevírané oznámení na Androidu][20]

**iOS**, **Android**

- Zapojení Mobile taky podporuje speciální druh ve aplikace oznámení s názvem **hlasování**. Umožňuje rozešlete snadné zjišťování uživatelům Segmentovaný aplikace. Můžete přidat otázky a možností pro každou otázku jako následující snímek. To získat zobrazí jako v aplikaci oznámení uživateli aplikace.   

    ![Upozornění na hlasování][32]

    ![Průzkum v systému Windows][26]

    ![Průzkum na iOS][27]   ![Průzkum na Androidu][28]

**iOS**, **Android**

- Zapojení Mobile taky podporuje odesílání pasivní **Dat nabízená** oznámení. Pomocí těchto upozornění můžete odeslat data z služby (třeba JSON údaj v následujícím příkladu), které můžete dělat v aplikaci a určitou akci. Příklad se, jak jsme měníte cena položky selektivním pomocí data nabízená oznámení.

    ![Data nabízená oznámení][33]

    ![Data nabízená oznámení v systému Windows][23]

    ![Data nabízená oznámení na iOS][24]  ![Data nabízená oznámení na Androidu][25]

**iOS**, **Android**

> [AZURE.NOTE] Podrobné pokyny pro některé z těchto oznámení můžete zobrazit kliknutím na **Kliknutím sem pokyny k odeslání toto oznámení z mobilních zapojení platformy** na libovolné obrazovce oznámení vzorku.


[1]: ./media/mobile-engagement-demo-apps/home-windows.png
[2]: ./media/mobile-engagement-demo-apps/home-ios.png
[3]: ./media/mobile-engagement-demo-apps/home-android.png
[4]: ./media/mobile-engagement-demo-apps/menu-windows.png
[5]: ./media/mobile-engagement-demo-apps/menu-ios.png
[6]: ./media/mobile-engagement-demo-apps/menu-android.png
[7]: ./media/mobile-engagement-demo-apps/device-id-windows.png
[8]: ./media/mobile-engagement-demo-apps/device-id-ios.png
[9]: ./media/mobile-engagement-demo-apps/device-id-android.png
[10]: ./media/mobile-engagement-demo-apps/out-of-app-windows.png
[11]: ./media/mobile-engagement-demo-apps/out-of-app-ios.png
[12]: ./media/mobile-engagement-demo-apps/out-of-app-android.png
[13]: ./media/mobile-engagement-demo-apps/out-of-app-display-windows.png
[14]: ./media/mobile-engagement-demo-apps/out-of-app-display-ios.png
[15]: ./media/mobile-engagement-demo-apps/out-of-app-display-android.png
[16]: ./media/mobile-engagement-demo-apps/in-app-windows.png
[17]: ./media/mobile-engagement-demo-apps/in-app-ios.png
[18]: ./media/mobile-engagement-demo-apps/in-app-android.png
[19]: ./media/mobile-engagement-demo-apps/pop-up-ios.png
[20]: ./media/mobile-engagement-demo-apps/pop-up-android.png
[21]: ./media/mobile-engagement-demo-apps/interstitial-ios.png
[22]: ./media/mobile-engagement-demo-apps/interstitial-android.png
[23]: ./media/mobile-engagement-demo-apps/data-push-windows.png
[24]: ./media/mobile-engagement-demo-apps/data-push-ios.png
[25]: ./media/mobile-engagement-demo-apps/data-push-android.png
[26]: ./media/mobile-engagement-demo-apps/survey-windows.png
[27]: ./media/mobile-engagement-demo-apps/survey-ios.png
[28]: ./media/mobile-engagement-demo-apps/survey-android.png
[29]: ./media/mobile-engagement-demo-apps/out-of-app.png
[30]: ./media/mobile-engagement-demo-apps/in-app-interstitial.png
[31]: ./media/mobile-engagement-demo-apps/in-app-pop-up.png
[32]: ./media/mobile-engagement-demo-apps/notification-poll.png
[33]: ./media/mobile-engagement-demo-apps/notification-data-push.png
