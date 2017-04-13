<properties
    pageTitle="Rozšířené možnosti vytváření sestav pro Android SDK zapojení Azure Mobile"
    description="Popisuje, jak dělat pokročilé sestav k zaznamenání analytics pro Android SDK zapojení Azure Mobile"
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
    ms.date="08/10/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-reporting-with-engagement-on-android"></a>Rozšířené možnosti vytváření sestav s zapojení na Androidu

> [AZURE.SELECTOR]
- [Univerzální Windows](mobile-engagement-windows-store-integrate-engagement.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-advanced-reporting.md)

Toto téma popisuje další scénáře vytváření sestav v aplikaci Android. Tyto možnosti můžete použít k aplikaci vytvořili v kurzu [Začínáme](mobile-engagement-android-get-started.md) .

## <a name="prerequisites"></a>Zjistit předpoklady pro

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

Kurzu, které jste dokončili byla záměrně přímý a jednoduché, ale jsou rozšířené možnosti, které můžete vybrat.

## <a name="modifying-your-activity-classes"></a>Úprava vaší `Activity` třídy

V tomto [kurzu Začínáme](mobile-engagement-android-get-started.md)všechny jste měli udělat bylo umožnit vaše `*Activity` dílčí Zdědit odpovídající `Engagement*Activity` třídy. Například, pokud starší verze aktivitách rozšířené `ListActivity`, by zajistíte tak jeho rozšíření `EngagementListActivity`.

> [AZURE.IMPORTANT] Při použití `EngagementListActivity` nebo `EngagementExpandableListActivity`, ujistěte se, že některé volat `requestWindowFeature(...);` je volání před voláním `super.onCreate(...);`, jinak dojde k chybě.

Můžete najít těchto tříd v `src` složky a můžete zkopírovat do projektu. Třídy jsou také **JavaDoc**.

## <a name="alternate-method-call-startactivity-and-endactivity-manually"></a>Alternativní metody: volání `startActivity()` a `endActivity()` ručně

Pokud nemůžete ani nebudete chtít přetížení vaše `Activity` třídy, můžete místo toho počátečního a koncového vašich aktivit najdete tak, že zavoláte `EngagementAgent`společnosti metody přímo.

> [AZURE.IMPORTANT] Android SDK nikdy volání `endActivity()` metody, i když zavřený (na Android aplikací nikdy uzavřeny). Je tedy *DŮRAZNĚ* doporučujeme volání `startActivity()` metoda v `onResume` zpětné ze *všech* vašich aktivit a `endActivity()` metoda v `onPause()` zpětné ze *všech* vašich aktivit najdete. Toto je jediný způsob, jak nezapomeňte, že nejsou prozrazený relace. Pokud prozrazený relaci zapojení služba nikdy odpojí od back-end zapojení (protože službu zůstane připojen, dokud relaci čeká na vyřízení).

Tady je příklad:

    public class MyActivity extends Some3rdPartyActivity
    {
      @Override
      protected void onResume()
      {
        super.onResume();
        String activityNameOnEngagement = EngagementAgentUtils.buildEngagementActivityName(getClass()); // Uses short class name and removes "Activity" at the end.
        EngagementAgent.getInstance(this).startActivity(this, activityNameOnEngagement, null);
      }

      @Override
      protected void onPause()
      {
        super.onPause();
        EngagementAgent.getInstance(this).endActivity();
      }
    }

Tento příklad je podobný `EngagementActivity` předmětu a jeho variant, podle jehož zdrojového kódu `src` složky.

## <a name="using-applicationoncreate"></a>Použití Application.onCreate()

Jakýkoli kód, umístěte do `Application.onCreate()` a v jiné aplikaci zpětná spustíte pro všechny aplikace procesy, včetně službu Engagement. Může mít nežádoucí straně efekty, jako je přidělování paměti nepotřebné a vláknech zapojení obrázku nebo duplicitní vysílání příjemce nebo služeb.

Pokud je přepsat `Application.onCreate()`, doporučujeme přidání následující fragment kódu na začátku vaše `Application.onCreate()` funkce:

     public void onCreate()
     {
       if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
         return;

       ... Your code...
     }

Umí totéž `Application.onTerminate()`, `Application.onLowMemory()`, a `Application.onConfigurationChanged(...)`.

Můžete také rozšířit `EngagementApplication` místo rozšíření `Application`: zpětné `Application.onCreate()` provádí kontrolu obrázku a volání `Application.onApplicationProcessCreate()` pouze nejsou-li aktuální proces toho hostitelské službě zapojení, stejná pravidla požádat o další zpětná.

## <a name="tags-in-the-androidmanifestxml-file"></a>Značky v souboru AndroidManifest.xml

V značku služby v souboru AndroidManifest.xml `android:label` atribut umožňuje vybrat název služby zapojení zástupného koncovým uživatelům na obrazovce "Službami" Telefon. Doporučujeme nastavení Tenhle atribut `"<Your application name>Service"` (například `"AcmeFunGameService"`).

Určení `android:process` atribut zajistíte, že služba zapojení spustí ve vlastním procesu (aplikaci můžete zapojit stejným způsobem jako aplikace hlavní/uživatelské rozhraní vlákna potenciálně pomaleji).

## <a name="building-with-proguard"></a>Vytváření s ProGuard

Vytvoření balíčku aplikace s ProGuard potřebnosti zachovat některé třídy. Můžete použít následující fragment konfigurace:

    -keep public class * extends android.os.IInterface
    -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
    <methods>;
    }
