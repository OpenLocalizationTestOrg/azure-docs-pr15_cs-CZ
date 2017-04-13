<properties
    pageTitle="Azure Mobile zapojení iOS poznámky k verzi SDK | Microsoft Azure"
    description="Nejnovější aktualizace a postupy pro iOS SDK pro zapojení Mobile Azure"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="piyushjo" />

#<a name="azure-mobile-engagement-ios-sdk-release-notes"></a>Poznámky k verzi Azure Mobile zapojení iOS SDK

##<a name="400-09122016"></a>4.0.0 (09/12/2016)

-   Pevné oznámení o není actioned na zařízení s iOS 10.
-   Neschválení XCode 7.

##<a name="324-06302016"></a>3.2.4 (06/30/2016)

-   Pevné agregace mezi technické protokoly a další.

##<a name="323-06072016"></a>3.2.3 (06/07/2016)

-   Pevná chyby, kde není doručení názory vykázaného po aplikace na pozadí.
-   Optimalizované odesílání technické protokoly.

##<a name="322-04072016"></a>3.2.2 (04/07/2016)

-   Pevné chyb při zrušení žádost HTTP, které někdy dochází k selhání.

##<a name="321-12112015"></a>3.2.1 (11/12/2015)

-   Pevná zpoždění při spuštění nové instance aplikace tak, že oznámení s hloubkové odkazy

##<a name="320-10082015"></a>3.2.0 (10/08/2015)

-   Povolit Bitcode v SDK snažíme usnadnit jeho práce s **Xcode 7**.
-   Pevné chyb souvisejících s oznámení ve aplikace.
-   Volání v aplikaci oznámení spolehlivější v případě zhoršeným battery a ostatních případech.
-   Odebere navíc konzoly protokolů generovaných knihovnou stran 3.

##<a name="310-08262015"></a>3.1.0 (08/26/2015)

-   Oprava chyb iOS 9 kompatibility s knihovnou třetích stran. Při odesílání zjišťuje výsledky, informace o aplikaci nebo data, která je byl způsobí dojde k chybě.

##<a name="300-06192015"></a>3.0.0 (06/19/2015)

-   Zapojení mobilní používá pasivní nabízená oznámení.
-   Podpora pro iOS nezobrazí 4.X. Spuštění z této verze cíl nasazení aplikace musí být alespoň iOS 6.

##<a name="220-05212015"></a>2.2.0 (05/21/2015)

-   Id zapojení mobilních zařízení pro zařízení < iOS 6 se teď podle GUID generovaného při instalaci.

##<a name="210-04242015"></a>2.1.0 (04/24/2015)

-   Přidané rychlé kompatibility.
-   Po kliknutí na informační akci, která je adresa URL spouštět vpravo po otevření aplikace.
-   Přidaný chybějící záhlaví soubor balíčku SDK.
-   Oprava problému, když byl zakázán zpravodaj pád zapojení Mobile.

##<a name="200-02172015"></a>2.0.0 (02/17/2015)

-   Počáteční verzi Azure mobilní zapojení
-   Konfigurace ID aplikace/sdkKey nahrazuje konfigurace řetězec připojení.
-   Odebere rozhraní API pro odesílání a přijímání zpráv libovolného typu XMPP z libovolného typu XMPP entity.
-   Odebere rozhraní API pro odesílání a přijímání zpráv mezi zařízeními.
-   Vylepšení zabezpečení.
-   Sledování SmartAd odeberou.
