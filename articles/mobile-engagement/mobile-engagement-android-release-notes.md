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
    ms.date="08/10/2016"
    ms.author="piyushjo" />

# <a name="release-notes"></a>Poznámky k verzi

## <a name="423-08102016"></a>4.2.3 (08/10/2016)

- Žádná další lock Wi-Fi.
- Oprava zablokování při volání getDeviceId před inicializace (Chyba zavedený 4.2.0).

## <a name="422-05172016"></a>4.2.2 (05/17/2016)

- Vylepšení stability.

## <a name="421-05102016"></a>4.2.1 (05/10/2016)

- Zabezpečení: zakážete webové zobrazení místního souboru přístup.
- Zabezpečení: odebrání `EngagementPreferenceActivity` předmětu, který rozšiřuje zastaralé a nezabezpečené `PreferenceActivity` předmětu.
- Zabezpečení: reach aktivity popsanými teď můžete `exported="false"`, tento příznak lze také v předchozích verzích SDK.

## <a name="420-03112016"></a>4.2.0 (03/11 nebo 2016)

- V SDK je teď licencován MIT.
- Povolte použití identifikátoru vlastní zařízení v době inicializace SDK.

## <a name="415-02012016"></a>4.1.5 (02/01/2016)

- Vylepšení stability.

## <a name="414-01262016"></a>4.1.4 (01/26/2016)

- Vylepšení stability.

## <a name="413-1292015"></a>4.1.3 (12/9/2015)

- Vylepšení stability.

## <a name="412-11252015"></a>4.1.2 (11/25/2015)

- Vylepšení stability.

## <a name="411-11042015"></a>4.1.1 (11/04/2015)

- Vylepšení stability.

## <a name="410-08252015"></a>4.1.0 (08/25/2015)

- Zpracujte nový model oprávnění pro Android M.
- Teď funkce můžete nakonfigurovat tak místo za běhu místo použití `AndroidManifest.xml`.
- Oprava chyby oprávnění: Pokud používáte `ACCESS_FINE_LOCATION`, pak `ACCESS_COARSE_LOCATION` už není potřeba.
- Vylepšení stability.

## <a name="400-07062015"></a>4.0.0 (07/06/2015)

-   Vnitřní protokol změnám, aby technologie pro analýzu a nabízených spolehlivější.
-   Nativní nabízených (GCM/ADM) se teď používá taky pro v oznamování v aplikaci, musíte nakonfigurovat nativní nabízených přihlašovací údaje pro každý typ nabízených kampaně.
-   Řešení oznámení celkového: byly zobrazené pouze 10s po stisknuté.
-   Oprava chyby ve webovém zobrazení: Po kliknutí na odkaz také při spouštění adresa URL výchozí akce.
-   Oprava méně častých pád související se správou místní úložiště.
-   Oprava dynamické konfigurační řízení řetězec.
-   Aktualizujte EULA.

## <a name="300-02172015"></a>3.0.0 (02/17/2015)

-   Počáteční verzi Azure mobilní zapojení
-   Konfigurace ID aplikace nahrazuje konfigurace řetězec připojení.
-   Odebere rozhraní API pro odesílání a přijímání zpráv libovolného typu XMPP z libovolného typu XMPP entity.
-   Odebere rozhraní API pro odesílání a přijímání zpráv mezi zařízeními.
-   Vylepšení zabezpečení.
-   Sledování Google Play a SmartAd odeberou.
