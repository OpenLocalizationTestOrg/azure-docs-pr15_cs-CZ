<properties
    pageTitle="Rozšířené možnosti vytváření sestav s MobileApps zapojení univerzální systému Windows"
    description="Jak integrovat univerzální aplikace Windows Azure mobilní zapojení do"                  
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-reporting-with-the-windows-universal-apps-engagement-sdk"></a>Rozšířené možnosti vytváření sestav s zapojení univerzální aplikace Windows SDK

> [AZURE.SELECTOR]
- [Univerzální Windows](mobile-engagement-windows-store-advanced-reporting.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-advanced-reporting.md)

Toto téma popisuje další scénáře vytváření sestav v aplikaci Windows univerzální. Podobnému sledu zahrnují možnosti, které můžete použít k aplikaci vytvořili v kurzu [Začínáme](mobile-engagement-windows-store-dotnet-get-started.md) .

## <a name="prerequisites"></a>Zjistit předpoklady pro

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

Před zahájením tohoto kurzu, musíte nejdřív dokončení kurz [Začínáme](mobile-engagement-windows-store-dotnet-get-started.md) , což je záměrně přímý a jednoduchý. Tento kurz zahrnuje další možnosti, které můžete vybírat.

## <a name="specifying-engagement-configuration-at-runtime"></a>Určení zapojení konfigurace za běhu

Konfigurace zapojení centralizované v `Resources\EngagementConfiguration.xml` souboru projektu, který je, kdy byla uvedená na téma [Začínáme](mobile-engagement-windows-store-dotnet-get-started.md) .

Ale můžete ho taky zadat za běhu: voláte metodu před inicializace agent zapojení:

          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set the Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);



## <a name="recommended-method-overload-your-page-classes"></a>Doporučená metoda: přetížení vaše `Page` třídy

Aktivace vykazování všechny požadované zapojení pro výpočet uživatelů, relací, aktivity, dojde k chybě a technických statistických protokoly, aby všechny vaše `Page` dílčí třídy dědí `EngagementPage` třídy.

Tady je příklad pro stránku aplikace. Umí totéž na všech stránkách aplikace.

### <a name="c-source-file"></a>C# zdrojového souboru

Upravte stránku `.xaml.cs` souboru:

-   Přidat do svého `using` příkazy:

        using Microsoft.Azure.Engagement;

-   Nahrazení `Page` s `EngagementPage`:

**Bez můžete zapojit:**

        namespace Example
        {
          public sealed partial class ExamplePage : Page
          {
            [...]
          }
        }

**S můžete zapojit:**

        using Microsoft.Azure.Engagement;

        namespace Example
        {
          public sealed partial class ExamplePage : EngagementPage
          {
            [...]
          }
        }

> [AZURE.IMPORTANT] Pokud stránku přepíše `OnNavigatedTo` metody, je potřeba zavolat `base.OnNavigatedTo(e)`. V opačném není možné vykázat aktivity ( `EngagementPage` volání `StartActivity` uvnitř jeho `OnNavigatedTo` metoda).

### <a name="xaml-file"></a>Soubor XAML

Upravte stránku `.xaml` souboru:

-   Přidání názvů deklarací:

        xmlns:engagement="using:Microsoft.Azure.Engagement"

-   Nahrazení `Page` s `engagement:EngagementPage`:

**Bez můžete zapojit:**

        <Page>
            <!-- layout -->
            ...
        </Page>

**S můžete zapojit:**

        <engagement:EngagementPage
            xmlns:engagement="using:Microsoft.Azure.Engagement">
            <!-- layout -->
            ...
        </engagement:EngagementPage >

### <a name="override-the-default-behaviour"></a>Přepsat výchozí chování

Ve výchozím nastavení je název třídy stránky hlášené jako název aktivity s není extra. Pokud předmětu používá příponu "Stránka", můžete zapojit ji odebere.

Chcete-li změnit výchozí chování na název, přidejte tento kód:

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

Vykazování dodatečné informace s vaši činnost, přidejte tento kód:

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

Tyto metody nazývají v rámci `OnNavigatedTo` metoda stránky.

### <a name="alternate-method-call-startactivity-manually"></a>Alternativní metody: volání `StartActivity()` ručně

Pokud nemůžete ani nebudete chtít přetížení vaše `Page` třídy, můžete místo toho začít vašich aktivit najdete tak, že zavoláte `EngagementAgent` metody přímo.

Doporučujeme, volající `StartActivity` uvnitř vaší `OnNavigatedTo` metoda stránky.

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [AZURE.IMPORTANT]  Zajistěte, aby že správně ukončit relaci.
>
> Windows SDK univerzální automaticky volá `EndActivity` metody, když máte zavřený aplikace. Je tedy **DŮRAZNĚ** doporučujeme zavolat `StartActivity` metoda kdykoli aktivity uživatele změnit a **nikdy** zavolat `EndActivity` metody. Tento způsob upozorní server zapojení že aktuálního uživatele opustil aplikace, která bude mít vliv na všechny protokoly aplikace.

## <a name="advanced-reporting"></a>Rozšířené možnosti vytváření sestav

Volitelně můžete chtít zprávu specifické pro aplikaci události, chyby a úlohy k tomu, použít jiné metody najdete v `EngagementAgent` předmětu. Rozhraní API zapojení umožňuje používat všechny zapojení pokročilých funkcí.

Další informace najdete v tématu [použití rozšířeného Mobile zapojení označování rozhraní API v aplikaci Windows univerzální](mobile-engagement-windows-store-use-engagement-api.md).
