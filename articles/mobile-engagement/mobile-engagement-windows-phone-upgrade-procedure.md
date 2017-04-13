<properties 
    pageTitle="Windows Phone Silverlight SDK postupů upgradu" 
    description="Windows Phone Silverlight SDK upgradu postupy pro Azure mobilní zasunutí"                  
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede"
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-phone-silverlight-sdk-upgrade-procedures"></a>Windows Phone Silverlight SDK postupů upgradu

Pokud jste už máte integrovaný starší verzí systému naše SDK aplikace, je nutné zvážit následující skutečnosti při upgradu SDK.

Možná bude potřeba postup několik Pokud zmeškané více verzích SDK. Pokud například můžete migrovat z 0.10.1 do 0.11.0 budete muset nejdřív použijte postup "z 0.9.0 0.10.1" pak "z 0.10.1 0.11.0" postup.

##<a name="from-200-to-330"></a>Z 2.0.0 3.3.0

### <a name="test-logs"></a>Testování protokoly

Protokoly konzoly vytvořené pomocí SDK můžete by se povolené/vypnutí/filtrování. Chcete-li přizpůsobit, aktualizujte vlastnost `EngagementAgent.Instance.TestLogEnabled` na jednu hodnotu poskytuje společnost `EngagementTestLogLevel` výčet, například:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

##<a name="from-111-to-200"></a>Z 1.1.1 2.0.0

Popisují následující kroky migrace SDK integrace služby Capptain zaměstnanecké přidružení Capptain zabezpečení, které do aplikace technologii Azure Mobile Engagement. 

> [Azure.IMPORTANT] Capptain a Mobile využití nejsou stejné služby a postupu uvedeného dole zvýrazní jenom jak migrovat aplikaci klienta. Migrace SDK v aplikaci se nemigrují dat z servery Capptain serverům zapojení Mobile

Při migraci ze starší verze, obraťte se na Capptain webu migrovat do 1.1.1 nejdřív a pak použijte následující postup

### <a name="nuget-package"></a>Nuget balíčku

**Capptain.WindowsPhone** nahrazení balíčkem Nuget **MicrosoftAzure.MobileEngagement** .

### <a name="applying-mobile-engagement"></a>Použití mobilního zapojení

V SDK používá termín `Engagement`. Budete muset aktualizovat projekt podle tuto změnu.

Budete muset odinstalovat aktuální balíčku nuget Capptain. Zvažte možnost, budou odebrány všechny změny ve složce Capptain zdroje. Pokud chcete zachovat soubory a pak vytvořit kopii.

Až to nainstalujte nový balíček nuget Microsoft Azure Engagement na projektu. Můžete najít ho přímo na [Nuget](http://www.nuget.org/packages/MicrosoftAzure.MobileEngagement). Tato akce nahradí všechny soubory zdrojů používaných zapojení a přidá nový DLL zapojení do odkazy projektu.

Budete muset vyčistit odkazy projektu odstraněním Capptain DLL odkazy. Pokud není uděláte toto, způsobí konflikt verzi Capptain a chyby dojde.

Pokud jste přizpůsobili Capptain zdrojů, zkopírujte staré obsahu soubory a vložte je do nové soubory můžete zapojit. Upozorňujeme, že soubory xaml a cs mají být aktualizovány.

Po provedení těchto kroků musíte pouze nahradit staré Capptain odkazy na nový odkaz Engagement.

1. Všechny obory názvů Capptain mají být aktualizovány.

    Před migrací:
    
        using Capptain.Agent;
        using Capptain.Reach;
    
    Po migraci:
    
        using Microsoft.Azure.Engagement;

2. Všechny tříd Capptain, které obsahují "Capptain" by měl obsahovat "Využití".

    Před migrací:
    
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
    
    Po migraci:
    
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }

3. Pro soubory xaml Capptain názvů a atributy taky změnit.

    Před migrací:
    
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
    
    Po migraci:
    
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>

4. Pro dalších zdrojů, jako jsou obrázky Capptain Všimněte si, že jsou také mít přejmenování používat "Využití".

### <a name="application-id--sdk-key"></a>ID aplikace / klíč SDK

Zapojení používá připojovací řetězec. Nemusíte zadat ID aplikace a klávesu se SDK zapojení Mobile, stačí zadat připojovací řetězec. Ho můžete nastavit v souboru EngagementConfiguration.

Konfigurace zapojení se dá nastavit vaší `Resources\EngagementConfiguration.xml` souboru projektu.

Upravte tento soubor:

-   Vaše aplikace připojovací řetězec mezi značky `<connectionString>` a `<\connectionString>`.

Pokud chcete určit za běhu, můžete volat metodu před inicializace agent můžete zapojit:

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        
        /* Initialize Engagement angent with above configuration. */
        EngagementAgent.Instance.Init(engagementConfiguration);

Připojovací řetězec pro aplikaci se zobrazí v portálu klasické Azure.

### <a name="items-name-change"></a>Změna názvu položky

Všechny položky s názvem *capptain* mít název *engagement*. Podobně jako pro *Capptain* k *Engagement*.

Příklady běžně používaných Capptain položek:

-   CapptainConfiguration teď s názvem EngagementConfiguration
-   CapptainAgent teď s názvem EngagementAgent
-   CapptainReach teď s názvem EngagementReach
-   CapptainHttpConfig teď s názvem EngagementHttpConfig
-   GetCapptainPageName teď s názvem GetEngagementPageName

Všimněte si, že přejmenovat také ovlivňuje přepsat metody.



 
