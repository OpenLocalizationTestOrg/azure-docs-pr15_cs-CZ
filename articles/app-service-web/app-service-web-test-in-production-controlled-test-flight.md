<properties
    pageTitle="Flighting nasazení (testování verze beta) v aplikaci služby Azure"
    description="Naučte se letu nové funkce v aplikaci nebo beta otestujte vaše aktualizace v tomto kurzu začátku do konce. Spojuje aplikaci služby funkcí, jako je nepřetržitý publikování sloty, směrování přenosu a integrace přehledy aplikace."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/02/2016"
    ms.author="cephalin"/>
# <a name="flighting-deployment-beta-testing-in-azure-app-service"></a>Flighting nasazení (testování verze beta) v aplikaci služby Azure

Tento kurz se dozvíte, jak provádět *flighting nasazení* integrací různé funkce [Aplikaci služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714) a [Přehledech aplikace Azure](/services/application-insights/). 

*Flighting* je proces nasazení, který ověří novou funkci nebo změna s omezený počet skutečné zákazníky a je hlavní testování v případě výroby. Podobá testování beta se a někdy označované jako "řízený test letu". Mnoho rozsáhlých společnostech prostřednictvím stavu webu pomocí tento přístup nejdříve možné ověřovací informace o jeho aktualizacích aplikace jejich prakticky [aktivní](https://en.wikipedia.org/wiki/Agile_software_development)vývoje. Služba Azure aplikací umožňuje test ve výrobním integrovat nepřetržité publikování a přehledech aplikace implementovat stejný scénář DevOps. K čemu tento přístup, patří:

- **Aktualizace _před_ skutečným názory jsou uvolnit výroby zisk** – jediné lepší než získat názory hned uvolníte získává názory musíte uvolnit dříve než. Aktualizace s reálným provozu a chování můžete otestovat brzy vyžadujete životního cyklu výrobku.
- **Vylepšení [vývoje nepřetržitý řízeného testováním (CTDD)](https://en.wikipedia.org/wiki/Continuous_test-driven_development) ** - integrací test ve výrobním s nepřetržitý integrace a přístrojového vybavení s aplikací přehledy ověřování uživatele se stane, nejdříve možné a automaticky životního cyklu výrobku. Díky snížit čas investic do ručního testu spuštění.
- **Optimalizace otestovat pracovní postup** – díky automatizaci test ve výrobním s nepřetržitý sledování přístrojového vybavení, můžete potenciálně provádět cíle různých druhů testů v jediném procesu, například [Integrace](https://en.wikipedia.org/wiki/Integration_testing) [regresní](https://en.wikipedia.org/wiki/Regression_testing), [použitelnosti](https://en.wikipedia.org/wiki/Usability_testing), přístupnost, lokalizace, [výkon](https://en.wikipedia.org/wiki/Software_performance_testing), [zabezpečení](https://en.wikipedia.org/wiki/Security_testing)a [přijetí](https://en.wikipedia.org/wiki/Acceptance_testing).

Flighting nasazení není prakticky směrování živou přenosy. V takové nasazení chcete získat přehled co nejdříve zda byla neočekávané chybě, snížení výkonu, problémy s zkušeností uživatelů. Nezapomeňte, že pracujete s reálným zákazníky. Tak na to správné, musí zajistíte, že jste nastavili flighting nasazení, můžete svolat všechna data, je nutné před uskutečněním informovaní rozhodnutí pro vaším dalším krokem. Tento kurz se dozvíte, jak získat informace o práci s aplikací přehledy, ale můžete použít nové Relic nebo jinými technologiemi, které se hodí nefunguje. 

## <a name="what-you-will-do"></a>Co bude dělat

V tomto kurzu se dozvíte, jak si přenést následujících scénářích můžete otestovat aplikaci služby aplikace ve výrobním:

- [Směrování výrobní dat](app-service-web-test-in-production-get-start.md) do aplikace beta
- [Nástroj aplikace](../application-insights/app-insights-web-track-usage.md) získat užitečné metriky
- Nasazení aplikace beta neustálé sledování metriky živou aplikace
- Porovnání metriky mezi aplikaci výrobní a aplikaci beta zobrazíte, jak změny kódu přeložení výsledků

## <a name="what-you-will-need"></a>Co budete potřebovat

-   Účet Azure
-   Účet [GitHub](https://github.com/)
- Visual Studio 2015 – můžete si stáhnout [komunity edition](https://www.visualstudio.com/en-us/products/visual-studio-express-vs.aspx).
-   Libovolná prostředí (součástí [GitHub pro Windows](https://windows.github.com/)) – umožňuje spuštění libovolná a prostředí PowerShell příkazů ve stejné relaci
-   Nejnovější bitů [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi)
-   Základní principy z následujících akcí:
    -   Nasazení šablony [Správce prostředků Azure](../azure-resource-manager/resource-group-overview.md) (viz [nasadit do složitých aplikací pro předvídatelností v Azure](app-service-deploy-complex-application-predictably.md))
    -   [Libovolná](http://git-scm.com/documentation)
    -   [Prostředí PowerShell](https://technet.microsoft.com/library/bb978526.aspx)

> [AZURE.NOTE] Musíte mít účet Azure tento kurz:
> + Můžete [Otevřít účet Azure zdarma](/pricing/free-trial/) – načtení přeplatky můžete vyzkoušet placené služby Azure a i když jste čerpány uchováváte účet a použití uvolnit Azure služby, jako je Web Apps.
> + Je možné [Aktivovat výhod odběratele Visual Studio](/pricing/member-offers/msdn-benefits-details/) : vaše aplikace Visual Studio předplatné vám přeplatky každý měsíc využívající služby placené Azure.
>
> Pokud chcete začít pracovat s aplikaci služby Azure před registrací účet Azure, přejděte na [Zkuste aplikaci služby](http://go.microsoft.com/fwlink/?LinkId=523751), které můžete okamžitě vytvořit web appu krátkodobý starter v aplikaci služby. Žádné povinné; kreditní karty žádné závazky.

## <a name="set-up-your-production-web-app"></a>Nastavení výrobní web appu

>[AZURE.NOTE] Skript použitý v tomto kurzu automaticky nakonfiguruje nepřetržitý publikování z úložiště GitHub. Tento postup vyžaduje, aby vaše přihlašovací údaje GitHub jsou už uložené v Azure, jinak skriptů nasazení se nezdaří, když se pokusíte konfigurace nastavení správy zdrojů pro webové aplikace.
>
>Pro ukládání přihlašovacích údajů GitHub v Azure, vytvořte webovou aplikaci [Portálu Azure](https://portal.azure.com/) a [Konfigurace GitHub nasazení](app-service-continuous-deployment.md#Step7). Stačí udělat jednou.

Ve scénáři typické DevOps aplikace, na kterém běží žijí Azure a chcete ho měnit prostřednictvím nepřetržitý publikování. V tomto scénáři nasadíte výroby šablonu, která jste vyvinuté a testovat.

1.  Vytvoření vlastního vidlice [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) úložiště. Informace týkající se vytváření vašeho vidlice najdete v tématu [vidlice Repo](https://help.github.com/articles/fork-a-repo/). Po vytvoření vaší vidlice uvidíte v prohlížeči.

    ![](./media/app-service-agile-software-development/production-1-private-repo.png)

2.  Otevřete relaci libovolná prostředí. Pokud ještě nemáte libovolná prostředí, nainstalujte [GitHub pro Windows](https://windows.github.com/) .
3.  Vytvoření místní klonovat vaší vidlice spuštěním následujícího příkazu:

        git clone https://github.com/<your_fork>/ToDoApp.git

4.  Až budete mít místní klonovat, přejděte na * &lt;repository_root >*\ARMTemplates a spustit deploy.ps1 skript s jedinečnou příponu, jak je ukázáno v následujícím příkladu:

        .\deploy.ps1 –RepoUrl https://github.com/<your_fork>/todoapp.git -ResourceGroupSuffix <your_suffix>

4.  Po zobrazení výzvy zadejte požadované uživatelské jméno a heslo pro přístup k databázi. Vzhledem k tomu, že budete muset zadat znovu při aktualizaci skupina zdroje Upozorňujeme, že vaše přihlašovací údaje pro databázi.

    Měli byste vidět zřizovací průběh různých Azure zdroje. Po dokončení nasazení skript spuštění aplikace v prohlížeči a umožňují popisný ZvukovýSignál.
    ![](./media/app-service-web-test-in-production-controlled-test-flight/00.1-app-in-browser.png)

6.  Zpátky v prostředí libovolná relace, spusťte tento příkaz:

        .\swap –Name ToDoApp<your_suffix>

    ![](./media/app-service-web-test-in-production-controlled-test-flight/00.2-swap-to-production.png)

7.  Po dokončení skript, vraťte se do přejděte na adresu frontend (http://ToDoApp*&lt;your_suffix >*.azurewebsites.net/) zobrazíte aplikace spuštěné v výroby.
5.  Přihlaste se k [Portálu Azure](https://portal.azure.com/) a přečtěte si téma co je vytvořen.

    Mají být k dispozici dva webových aplikací web apps ve stejné skupině prostředků, jedna z nich `Api` přípona názvu. Když se podíváte na skupiny zobrazení zdrojů, zobrazí se také databáze SQL a server, plán služeb aplikací a pracovní sloty pro webové aplikace. Projděte si různých zdrojů a porovnejte je s * &lt;repository_root >*\ARMTemplates\ProdAndStage.json zobrazíte jejich konfigurace v šabloně.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/00.3-resource-group-view.png)

Jste nastavili aplikaci výroby.  Teď pojďme si představte získávat názory ostatních zvýšení použitelnosti je špatné aplikace. Tak rozhodnete-li prozkoumat. Budete chtít nástroje aplikace chcete říct svůj názor.

## <a name="investigate-instrument-your-client-app-for-monitoringmetrics"></a>Prozkoumejte: Nástroje klientské aplikace pro sledování/metriky

5. Otevřít * &lt;repository_root >*\src\MultiChannelToDo.sln ve Visual Studiu.
6. Obnovení všechny balíčky Nuget kliknutím pravým tlačítkem na řešení > **Spravovat NuGet balíčků řešení** > **Obnovit**.
6. Klikněte pravým tlačítkem myši **MultiChannelToDo.Web** > **Přidat aplikaci přehledy Telemetrie** > **Konfigurovat nastavení** > změnit pole Skupina zdroje pro ToDoApp*&lt;your_suffix >* > **Přidat přehledy aplikace do projektu**.
7. Na portálu Azure otevřete zásuvné **MultiChannelToDo.Web** aplikace přehled zdroje. V části **Ochrana aplikací** klikněte **Zjistěte, jak získat informace o Prohlížeč stránky načtení dat** > Kopírovat kód.
7. Přidejte zkopírovaný kód přístrojového vybavení JS k * &lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml těsně před ukončením `<heading>` značku. By měl obsahovat jedinečné přístrojového vybavení klíč aplikace přehled zdrojů.

        <script type="text/javascript">
        var appInsights=window.appInsights||function(config){
            function s(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},r=document,f=window,e="script",o=r.createElement(e),i,u;for(o.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js",r.getElementsByTagName(e)[0].parentNode.appendChild(o),t.cookie=r.cookie,t.queue=[],i=["Event","Exception","Metric","PageView","Trace"];i.length;)s("track"+i.pop());return config.disableExceptionTracking||(i="onerror",s("_"+i),u=f[i],f[i]=function(config,r,f,e,o){var s=u&&u(config,r,f,e,o);return s!==!0&&t["_"+i](config,r,f,e,o),s}),t
        }({
            instrumentationKey:"<your_unique_instrumentation_key>"
        });

        window.appInsights=appInsights;
        appInsights.trackPageView();
        </script>

11. Odeslání vlastní události interpretace aplikace pro myši kliknutími můžete přidat následující kód na konec textu:

        <script>
            $(document.body).find("*").click(function(event) {

                appInsights.trackEvent(event.target.tagName + ": " + event.target.className);
            });
        </script>

    Tento JavaScriptový fragment kódu rozešle událost nastavit jako vlastní aplikace přehledy pokaždé, když uživatel klikne kdekoli ve web appu.

12. V prostředí libovolná potvrdit a použít změny pro vidlice v GitHub. Počkejte klientům aktualizujte prohlížeč.

        git add -A :/
        git commit -m "add AI configuration for client app"
        git push origin master

6.  Záměna nasazeném aplikace změny výrobní:

        .\swap –Name ToDoApp<your_suffix>

13. Přejděte do aplikace přehledy zdroje, které jste nakonfigurovali. Klikněte na vlastní události.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/01-custom-events.png)

    Pokud nevidíte metriky pro vlastní události, počkejte pár minut a klikněte na **Aktualizovat**.

Předpokládejme, že se zobrazí graf jako níže:

![](./media/app-service-web-test-in-production-controlled-test-flight/02-custom-events-chart-view.png)

A mřížka události pod ním:

![](./media/app-service-web-test-in-production-controlled-test-flight/03-custom-event-grid-view.png)

Podle předpisů rozhraní aplikace kódu ToDoApp události **TLAČÍTKA** odpovídá tlačítko Odeslat a událostí **vstupní** odpovídá textové pole. Zatím věci smysl. Však to vypadá, je dobré množství myší a málo kliknutí na položek úkolů ( **LI** události).

Podle toho můžete formuláře vaše hypotéz, které jsou někteří uživatelé nevyznáte, která část uživatelského rozhraní je můžete kliknout a je to proto, že kurzor u něhož je použit u vybraného textu při myši na seznam položek a jejich ikony.

![](./media/app-service-web-test-in-production-controlled-test-flight/04-to-do-list-item-ui.png)

Může to být contrived příklad. Budete přesto volání zlepšení do aplikace a proveďte flighting nasazení získat názory použitelnosti od živou zákazníků.

### <a name="instrument-your-server-app-for-monitoringmetrics"></a>Nástroje server aplikace pro sledování/metriky
Toto je tangens od scénář znázorněn v tomto kurzu pouze zabývá aplikaci klienta. Však pro dokončení chcete nastavit aplikace na straně serveru.

6. Klikněte pravým tlačítkem myši **MultiChannelToDo** > **Přidat aplikaci přehledy Telemetrie** > **Konfigurovat nastavení** > změnit pole Skupina zdroje pro ToDoApp*&lt;your_suffix >* > **Přidat přehledy aplikace do projektu**.
12. V prostředí libovolná potvrdit a použít změny pro vidlice v GitHub. Počkejte klientům aktualizujte prohlížeč.

        git add -A :/
        git commit -m "add AI configuration for server app"
        git push origin master

6.  Záměna nasazeném aplikace změny výrobní:

        .\swap –Name ToDoApp<your_suffix>

Je to!

## <a name="investigate-add-slot-specific-tags-to-your-client-app-metrics"></a>Prozkoumejte: Přidání značek úsek specifické pro vašeho klienta aplikace metriky

V této části nakonfigurujete sloty různých nasazení odeslat telemetrie úsek specifické pro stejný zdroj přehledy aplikace. Tímto způsobem můžete porovnat telemetrickými daty mezi přenosy z různých sloty (nasazení prostředí) snadno zobrazit výsledky změn aplikace. Ve stejnou dobu můžete oddělit přenosy výrobní od ostatních ke sledování výrobní aplikace v případě potřeby mohli pokračovat.

Vzhledem k tomu, že shromažďování dat na chování klienta, se dozvíte, jak [Přidat telemetrie inicializační kódu jazyka JavaScript](../application-insights/app-insights-api-custom-events-metrics.md#js-initializer) v index.cshtml. Pokud chcete testovat serverovou výkonu, například můžete taky udělat podobně v kódu serveru (viz [Aplikace přehledy rozhraní API pro vlastní události a metriky](../application-insights/app-insights-api-custom-events-metrics.md).

1. Nejdřív přidejte kód bewteen dvou `//` poznámky pod v JavaScriptu blokovat, které jste přidali do `<heading>` označení dříve.

        window.appInsights = appInsights;

        // Begin new code
        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["Environment"] = "@System.Configuration.ConfigurationManager.AppSettings["environment"]";
            });
        });
        // End new code

        appInsights.trackPageView();

    Tento kód inicializační způsobí, že `appInsights` objekt, který chcete přidat vlastní vlastnost s názvem `Environment` každé důležité telemetrie pošle.

2. Dále přidejte tuto vlastní vlastnost jako [úsek nastavení](web-sites-staged-publishing.md#AboutConfiguration) pro webovou aplikaci v Azure. K tomuto účelu spusťte následující příkazy v relaci libovolná prostředí.

        $app = Get-AzureWebsite -Name todoapp<your_suffix> -Slot production
        $app.AppSettings.Add("environment", "Production")
        $app.SlotStickyAppSettingNames.Add("environment")
        $app | Set-AzureWebsite -Name todoapp<your_suffix> -Slot production

    Web.config v projektu již definuje `environment` nastavení aplikace. Při tomto nastavení při testování aplikaci místně, vaše metriky bude být označené `VS Debugger`. Ale pokud použít změny pro Azure Azure bude vyhledání a použití `environment` nastavení ve web appu konfiguraci aplikace a vaše metriky být označené bude `Production`.

3. Potvrďte a použít změny kódu pro vaše vidlice na GitHub a počkejte, uživatelé můžou používat novou aplikaci (třeba aktualizujte prohlížeč). Trvá asi 15 minut nové vlastnosti objevit v aplikaci přehledy `MultiChannelToDo.Web` zdroje.

        git add -A :/
        git commit -m "add environment property to AI events for client app"
        git push origin master

4. Teď, přejděte na zásuvné **vlastní události** znovu a filtrovat metriky `Environment=Production`. Teď byste měli být schopní zobrazíte všechny nové vlastní události v úsek výrobní k tomuto filtru.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/05-filter-on-production-environment.png)

5. Klikněte na tlačítko **Oblíbené** na Uložit aktuální nastavení metriky Explorer nějak **vlastní události: výrobní**. Můžete snadno přepínat toto zobrazení a zobrazení úsek nasazení později.

    > [AZURE.TIP] Ještě víc výkonné analýzy zvažte možnost [Integrace aplikace přehledy zdroje s Power BI](../application-insights/app-insights-export-power-bi.md).

### <a name="add-slot-specific-tags-to-your-server-app-metrics"></a>Přidání značky specifické pro úsek metriky aplikace váš server
Ještě jednou pro dokončení chcete nastavit aplikace na straně serveru. Na rozdíl od aplikaci klienta, který využívá v JavaScript značky úsek specifické pro aplikaci serveru podporovány kódem .NET.

1. Otevřít * &lt;repository_root >*\src\MultiChannelToDo\Global.asax.cs. Přidejte blok kódu pod, těsně před pravou složenou závorku názvů.

        namespace MultiChannelToDo
        {
                ...

                // Begin new code
            public class ConfigInitializer
            : ITelemetryInitializer
            {
                void ITelemetryInitializer.Initialize(ITelemetry telemetry)
                {
                    telemetry.Context.Properties["Environment"] = System.Configuration.ConfigurationManager.AppSettings["environment"];
                }
            }
                // End new code
        }

2. Oprava chyby rozlišení názvů přidáním `using` příkazy pod na začátek souboru:

        using Microsoft.ApplicationInsights.Channel;
        using Microsoft.ApplicationInsights.Extensibility;

3. Přidání kódu pro pod se začátkem `Application_Start()` metodu:

        TelemetryConfiguration.Active.TelemetryInitializers.Add(new ConfigInitializer());

3. Potvrďte a použít změny kódu pro vaše vidlice na GitHub a počkejte, uživatelé můžou používat novou aplikaci (třeba aktualizujte prohlížeč). Trvá asi 15 minut nové vlastnosti objevit v aplikaci přehledy `MultiChannelToDo` zdroje.

        git add -A :/
        git commit -m "add environment property to AI events for server app"
        git push origin master

## <a name="update-set-up-your-beta-branch"></a>Aktualizace: Nastavení pobočkové beta

2. Otevřít * &lt;repository_root >*\ARMTemplates\ProdAndStagetest.json a hledání `appsettings` zdroje (vyhledat `"name": "appsettings"`). Existuje 4 z nich, jedno pro každý úsek. 

2. U každé `appsettings` zdroje, přidejte `"environment": "[parameters('slotName')]"` nastavení aplikace na konec `properties` pole. Nezapomeňte ukončit na předchozí řádek čárkou.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/06-arm-app-setting-with-slot-name.png)
    
    Jste právě přidali `environment` nastavení aplikace na všechny sloty v šabloně.
    
2. Ve stejném souboru najít `slotconfignames` zdroje (vyhledat `"name": "slotconfignames"`). Existuje 2 z nich, jedno pro každé aplikaci.

2. U každé `slotconfignames` zdroje, přidejte `"environment"` na konec `appSettingNames` pole. Nezapomeňte ukončit na předchozí řádek čárkou.

    Jste právě udělali `environment` aplikace nastavení zařízení na jeho pozici odpovídajících nasazení pro obě aplikace.  

3. V prostředí libovolná relaci, spusťte následující příkazy koncovku stejné zdroje skupiny, který jste použili před.

        git checkout -b beta
        git push origin beta
        .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch beta

4. Po zobrazení výzvy zadejte stejné SQL databáze přihlašovací údaje jako před. Po zobrazení výzvy k aktualizaci skupina zdroje, zadejte `Y`, pak `ENTER`.

    Jakmile skript, všechny zdroje ve skupině původní zdroj se zachovají, ale nové pozici s názvem "beta" vytvoří se v něm s nastavením stejný jako úsek "Pracovní", která byla vytvořená na začátku.

    >[AZURE.NOTE] Tento způsob vytvoření různých prostředí se liší od metoda [aktivní software development službou Azure aplikace](app-service-agile-software-development.md). Tady vytvořit nasazení prostředí s sloty nasazení, kde takto můžete vytvořit nasazení prostředí s skupiny zdrojů. Správa nasazení prostředí se skupinami zdrojů umožňuje udržovat off-limits provozním prostředí pro vývojáře, ale není těžké si nejdřív otestovat výroby, které můžete snadno dělat s sloty.

Pokud chcete, můžete taky vytvořit alfa aplikaci spuštěním

    git checkout -b alpha
    git push origin alpha
    .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch alpha

Pro účely tohoto návodu budete jenom dál používat aplikaci beta.

## <a name="update-push-your-updates-to-the-beta-app"></a>Aktualizace: Nabízená aktualizace do aplikace beta

Zpět do aplikace, kterou chcete vylepšit.

1. Ujistěte se, že se účastníte teď větev beta

        git checkout beta

2. V * &lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml, najdete `<li>` značky a přidejte `style="cursor:pointer"` atribut, jak je ukázáno v následujícím příkladu.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/07-change-cursor-style-on-li.png)

3. potvrzení a nabízených Azure.

4. Ověřte, že změny nyní se v úsek beta tak, že přejdete do http://todoapp*&lt;your_suffix >*-beta.azurewebsites.net/. Pokud zatím změnu nevidíte, aktualizujte svůj prohlížeč získat nový kód javascript.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/08-verify-change-in-beta-site.png)

Uložte změnu aplikaci úsek beta máte, jste připraveni k provedení flighting nasazení.

## <a name="validate-route-traffic-to-the-beta-app"></a>Ověření: Směrovat přenosy v síti do aplikace beta

V této části bude směrovat přenosy v aplikaci beta. For sake of přehlednosti ukázka budete chtít směrovat významné část přenosy uživatele. Ve skutečnosti množství přenosů, kterou chcete směrovat závisí na vašim potřebám. Například pokud je váš web ve velkém měřítku microsoft.com, pak možná budete muset menší než jedno procento celkového přenosy za účelem získání užitečná data.

1. V prostředí libovolná relaci, spusťte následující příkazy pro polovinu výrobní provoz směrovat úsek beta:

        $siteName = "ToDoApp<your suffix>"
        $rule = New-Object Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.RampUpRule
        $rule.ActionHostName = "$siteName-beta.azurewebsites.net"
        $rule.ReroutePercentage = 50
        $rule.Name = "beta"
        Set-AzureWebsite $siteName -Slot Production -RoutingRules $rule

  `ReroutePercentage=50` Vlastnost určuje, že budou 50 % přenosů výrobní směrovány na aplikaci beta URL (podle `ActionHostName` vlastnosti).

2. Teď přejděte do http://ToDoApp*&lt;your_suffix >*. azurewebsites.net. 50 % přenosů by nyní přesměrovaní na úsek beta.

3. V aplikaci přehledy zdroje vyjádřené vyfiltrovat metriky prostředí = "beta".

    > [AZURE.NOTE] Pokud toto filtrované zobrazení uložit jako jiný Oblíbené, můžete snadno překlopit zobrazení metrických Průzkumníka mezi výrobní a beta zobrazení.

Předpokládejme, že v aplikaci přehledy uvidíte podobnou následující:

![](./media/app-service-web-test-in-production-controlled-test-flight/09-test-beta-site-in-production.png)

Nejen je to zobrazující, že jsou na mnoha kliknutími navíc `<li>` značky, ale zdá nárůstu kliknutí na `<li>` značky. Můžete pak zavřete, že uživatelé mají zjistil nové `<li>` značky kliknout a teď zrušením zaškrtnutí všech jejich dříve dokončení úkolů v aplikaci.

Na základě dat flighting nasazení, rozhodnete, že je připraven k výrobní nového uživatelského rozhraní.

## <a name="go-live-move-your-new-code-into-production"></a>Přesuňte: Přesunutí nový kód do provozu

Teď jste připraveni přesunout aktualizaci výroby. Co je skvělý je, že nyní víte, že vaše aktualizace již ověřený _před_ jeho se posune výroby. Nyní můžete jistotou nasadit ho. Vzhledem k tomu, že jste provedli aktualizaci aplikaci AngularJS klienta, pouze na straně klienta kód ověřit. Pokud byly ke změnám rozhraní API webových aplikací back-end může ověřit změny snadno a podobně.

1. V prostředí libovolná odeberte pravidla směrování přenosu pomocí tohoto příkazu:

        Set-AzureWebsite $siteName -Slot Production -RoutingRules @()

2. Spusťte libovolná příkazy:

        git checkout master
        git pull origin master
        git merge beta
        git push origin master

2. Počkejte několik minut nového kódu do být nasazené na pracovní pozici a pak spusťte http://ToDoApp*&lt;your_suffix >*-staging.azurewebsites.net k ověření, že nové aktualizace je provozní teplotu v pracovní pozici. Zapamatovat, že vaše vidlice předlohy pobočky je propojená s pracovní pozici aplikace.

3. Teď zaměnit pracovní pozici do provozu

        cd <ROOT>\ToDoApp\ARMTemplates
        .\swap.ps1 -Name todoapp<your_suffix>

## <a name="summary"></a>Souhrn ##

Azure aplikaci služby usnadňuje malé - pro střední firmy a něco otestujte jejich zákazníka webových aplikací v výrobní, obvykle dokončená ve velké podniky. Zpravidla tento kurz vám Dal knowledge potřebných pro spojování aplikaci služby a aplikace přehledy možné flighting nasazení a dokonce ostatních případech test ve výrobním DevOps světě. 

## <a name="more-resources"></a>Další materiály ##

-   [Aktivní software development aplikace službou Azure](app-service-agile-software-development.md)
-   [Nastavení pracovní prostředí pro web apps v aplikaci služby Azure](web-sites-staged-publishing.md)
-   [Nasazení aplikace složité předvídatelností v Azure](app-service-deploy-complex-application-predictably.md)
-   [Vytváření Azure správce prostředků šablony](../resource-group-authoring-templates.md)
-   [JSONLint - JSON ověřování](http://jsonlint.com/)
-   [Libovolná větvení – základní větvení a sloučení](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
-   [Azure Powershellu](../powershell-install-configure.md)
-   [Projekt Kudu wikiwebu](https://github.com/projectkudu/kudu/wiki)
