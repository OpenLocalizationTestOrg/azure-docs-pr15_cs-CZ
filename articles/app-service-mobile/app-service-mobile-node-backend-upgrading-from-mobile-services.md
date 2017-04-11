<properties
    pageTitle="Upgrade z mobilních služeb služby Azure aplikace - Node.js"
    description="Zjistěte, jak snadno upgradovat aplikaci služby Mobile do aplikace pro mobilní služba aplikace"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile"
    ms.devlang="node"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="upgrade-your-existing-nodejs-azure-mobile-service-to-app-service"></a>Upgrade existující Node.js Azure Mobile Service pro aplikaci služby

Mobilní služba aplikace je nový způsob, jak vytvářet mobilní aplikace pomocí Microsoft Azure. Další informace najdete v tématu [jaké mobilní aplikace?].

Toto téma popisuje, jak upgradovat existující aplikaci back-end Node.js Mobile služby Azure nových aplikací Mobilní služba aplikace. Při provádění tento upgrade aplikace služby Mobile můžete dál pracovat.  Pokud je třeba upgradovat aplikaci back-end Node.js, v nápovědě k [upgradu služeb Mobile .NET](./app-service-mobile-net-upgrading-from-mobile-services.md).

Při upgradu mobilní back-end aplikace služby Azure má přístup ke všem funkcím aplikace služby a účtovány podle předpisů rozhraní [aplikace služby ceny], ne mobilních služeb ceny.

## <a name="migrate-vs-upgrade"></a>Migrace a upgrade

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

>[AZURE.TIP] Doporučujeme tuto [migrace](app-service-mobile-migrating-from-mobile-services.md) před přechodem prostřednictvím upgrade. Tímto způsobem můžete umístit obě verze aplikace na stejné služby aplikace plánování a vzniknou žádné další náklady.

### <a name="improvements-in-mobile-apps-nodejs-server-sdk"></a>Vylepšení v mobilních aplikacích Node.js serveru SDK

Upgrade na nové [Mobilní aplikace SDK](https://www.npmjs.com/package/azure-mobile-apps) nabízí spoustu vylepšení, včetně:

- Podle [Express framework](http://expressjs.com/en/index.html), nové SDK uzel je lehká a navržený pro zachování přehledu o nové verze uzel při přechodu. Můžete přizpůsobit chování aplikace s Express middleware.

- Vylepšení výkonu ve srovnání s Services SDK mobilní telefon.

- Web můžete hostovat teď společně s vaše mobilní back-end; Podobně se snadno sčítat Azure Mobile SDK libovolné existující express.v4 aplikace.

- Vytvořené pro vývojové platformy a místní SDK mobilní aplikace může být vyvinuté a spusťte místně na platformách Windows, Linux a OSX. Nyní je snadno se použije běžné uzel vývojových technik například pro provoz [téměř jako](https://mochajs.org/) testy před nasazením.

## <a name="overview"></a>Základní upgradu přehled

Pokud chcete pomoci při upgradu back-end Node.js, dodala aplikaci služby Azure balíčku kompatibility.  Po upgradu budete mít niew web, který může být nasazené na nový web aplikaci služby.

Klient služby Mobile SDK jsou **není** kompatibilní s novým serverem mobilní aplikace SDK. Poskytování kontinuitu služby aplikace neměli změny publikování na web aktuálně obsluhuje publikované klienty. Místo toho měli vytvořit nové mobilní aplikace, které bude sloužit jako duplicitní. Tato aplikace můžete umístit na stejný plán služeb aplikací účtovány další finanční náklady.

Bude pak mít dvě verze aplikace: aplikace, která se nezmění a slouží publikovaných v galerii přírody a druhé, které pak můžete upgradovat a cílové s novou verzi klienta. Přesunutí a testování kódu vašeho tempem, ale nezapomeňte, že všechny provedené opravy chyb získat použité pro obě. Jakmile máte pocit, že požadovaný počet klientské aplikace v přírody dokument aktualizují na nejnovější verzi, můžete odstranit původní migrované aplikace vyžadujete-li. Ho není vynakládá jakékoli další peněžní, pokud použitý ve stejné plán služeb aplikací jako mobilní aplikaci.

Úplné osnovu proces upgradu vypadá takto:

1. Stáhněte si svůj stávající (migrované) Mobilní služba Azure.
2. Převeďte projekt aplikace Mobile Azure pomocí balíčku kompatibility.
3. Opravte všechny rozdíly (například nastavení ověřování).
4. Nasazení převedený Azure mobilní aplikace project do nové služby aplikace.
4. Uvolněte novou verzi klienta aplikace, které používat novou aplikaci Mobile.
5. (Volitelné) Odstraňte původní migrované Mobilní služba aplikace.

Odstranění může dojít, pokud nevidíte všechny přenosy na váš původní migrované mobilních služeb.

## <a name="install-npm-package"></a>Nainstalovat předběžnou požadavky

[Uzel] nainstalujte na místním počítači.  Měli byste taky nainstalovat balíčku kompatibility.  Po instalaci uzel můžete z nových cmd nebo příkazovém řádku prostředí PowerShell spusťte tento příkaz:

```npm i -g azure-mobile-apps-compatibility```

## <a name="obtain-ams-scripts"></a>Získání skriptů Azure mobilní služby

- Přihlaste se k [portálu Azure].
- Pomocí **Všechny zdroje** nebo **Služby aplikace**, najděte webu služby Mobile.
- V rámci webu, klikněte na **Nástroje** -> **Kudu** -> **přejděte** k otevření webu Kudu.
- Klikněte na **Ladění konzoly** -> **prostředí PowerShell** spusťte konzolu ladění.
- Přejděte na `site/wwwroot/App_Data/config` zase kliknutím na všechny adresáře
- Klikněte na ikonu Stáhnout vedle pole `scripts` adresář.

To stáhne skripty v ZIP formátu.  Vytvoření nového adresáře na místním počítači a rozbalit `scripts.ZIP` souborů v rámci adresáře.  Tím vytvoříte `scripts` adresář.

## <a name="scaffold-app"></a>Scaffold novou back-end Azure mobilní aplikace

Z adresáře obsahujícího adresáře Skripty spusťte tento příkaz:

```scaffold-mobile-app scripts out```

Tím vytvoříte scaffolded Azure mobilní aplikace back-end v `out` adresář.  Přestože není povinné, je vhodné ověřit `out` adresáře do úložiště zdrojového kódu podle svého výběru.

## <a name="deploy-ama-app"></a>Nasazení backendovou Azure mobilní aplikace

Při nasazení budete potřebovat provést následující akce:

1. Vytvoření nové mobilní aplikace na [Portál Azure].
2. Spustit `createViews.sql` skriptu na připojení databáze.
3. Propojení databáze, která je propojená s služby Mobile do nové služby aplikace.
4. Další zdroje (třeba oznámení rozbočovače) odkaz na službu nové aplikace.
5. Nasazení generované kódem na nový web.

### <a name="create-a-new-mobile-app"></a>Vytvoření nové aplikace Mobile

1. Přihlaste se na [portál Azure].

2. Klikněte na **+ Nový** > **Web + Mobile** > **Mobilní aplikaci**a potom zadejte název pro mobilní aplikaci back.

3. **Pole Skupina zdroje**vyberte existující skupinu zdrojů nebo vytvořte nový účet (s využitím stejný název jako aplikace.) 
 
    Můžete vybrat jiné plán služeb aplikací nebo vytvořte nový účet. Další informace o aplikaci služby plány a jak vytvořit nový plán v jiné ceny úroveň a do požadovaného umístění v tématu [Přehled hloubkovou aplikaci služby Azure plány](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

4. **Plán služeb aplikací**je vybráno výchozí plán (ve [standardní vrstvu](https://azure.microsoft.com/pricing/details/app-service/)). Můžete také vybrat na jiný plán nebo [vytvořte nový účet](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan). Nastavení aplikace služeb plánu určují [umístění, funkce, náklady a výpočet zdroje](https://azure.microsoft.com/pricing/details/app-service/) přidružený k vaší aplikaci. 

    Pokud se rozhodnete na plán, klikněte na **vytvořit**. Tím vytvoříte back-end mobilní aplikaci. 


### <a name="run-createviewssql"></a>Spuštění CreateViews.SQL

Aplikaci scaffolded obsahuje soubor s názvem `createViews.sql`.  Tento skript musí spouštět oproti cílové databáze.  Připojovací řetězec pro cílovou databázi můžete získat z migrované mobilní služby z zásuvné **Nastavení** v části **Připojovací řetězec**.  Je název `MS_TableConnectionString`.

Tento skript v SQL Server Management Studio nebo Visual Studio můžete spustit.

### <a name="link-the-database-to-your-app-service"></a>Propojení databáze aplikace služby

Propojit existující databáze aplikací služby:

- Na [Portálu Azure]otevřete aplikaci služby.
- Vyberte **všechna nastavení** -> **datová připojení**.
- Klepněte na **+ Přidat**.
- V rozevíracím seznamu vyberte **Databázi SQL**
- Ve skupinovém rámečku **Databáze SQL**vyberte stávající databázi a potom klikněte na **Vybrat**.
- V části **připojovací řetězec**zadejte uživatelské jméno a heslo pro databázi a potom klikněte na **OK**.
- V zásuvné **Přidat datových připojení** klikněte na **OK**.

Uživatelské jméno a heslo, najdete zobrazením připojovací řetězec pro cílovou databázi služby migrované Mobile.


### <a name="set-up-authentication"></a>Nastavení ověřování

Azure mobilní aplikace umožňuje konfigurace Azure Active Directory, Facebook, Google, Microsoft Twitter ověřování a v rámci služby.  Vlastní ověřování muset vyvinuté samostatně.  Podívejte se do si přečtěte následující dokumentaci [Ověřování koncepty] a [Rychlý úvod ověřování] si přečtěte následující dokumentaci pro další informace.  

## <a name="updating-clients"></a>Aktualizace mobilní klienty

Až budete mít provozní mobilní aplikaci back-end, můžete pracovat s novou verzí klientská aplikace, které využívá ho. Mobilní aplikace obsahuje taky novou verzi klienta SDK a podobná upgradu serveru nad, budete muset odstranit všechny odkazy SDK služby Mobile před instalací aplikací Mobile verze.

Jednou z hlavních změn mezi verzemi je konstruktorů už požadování klávesu application. Teď jednoduše předáte v adrese URL mobilní aplikaci. Například v klientech .NET `MobileServiceClient` konstruktor je nyní:

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net", // URL of the Mobile App
        );

Další informace o instalaci nového SDK a práce s novou strukturu pomocí těchto odkazů:

- [Android verze 2.2 nebo novější](app-service-mobile-android-how-to-use-client-library.md)
- [iOS verze 3.0.0 nebo novější](app-service-mobile-ios-how-to-use-client-library.md)
- [.NET (Windows/Xamarin) verze 2.0.0 nebo novější](app-service-mobile-dotnet-how-to-use-client-library.md)
- [Apache Cordova verze 2.0 nebo novější](app-service-mobile-cordova-how-to-use-client-library.md)

Pokud vaše aplikace provede používat nabízená oznámení, poznamenejte si konkrétní registraci pokyny pro každou platformu, co byly některé změny stejně.

Když máte novou verzi klienta jste připravení vyzkoušejte proti upgradovaném serveru projektu. Po ověření, že funguje, můžete uvolnit nová verze aplikace pro zákazníky. Nakonec Jakmile se vaši zákazníci mohli dříve je moct přijímat tyto aktualizace, můžete odstranit služby mobilní verzi aplikace. V tomto okamžiku úplně upgradu na aplikaci služby mobilní aplikaci pomocí nejnovější serveru mobilní aplikace SDK.

<!-- URLs. -->

[Azure portálu]: https://portal.azure.com/
[Azure classic portal]: https://manage.windowsazure.com/
[Jaké mobilní aplikace?]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[Mobile App Server SDK]: https://www.npmjs.com/package/azure-mobile-apps
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications to your mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication to your mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure Scheduler]: /en-us/documentation/services/scheduler/
[Web Job]: ../app-service-web/websites-webjobs-resources.md
[How to use the .NET server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services to an App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[Aplikace služby ceny]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[.NET server SDK overview]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Ověřování koncepty]: ../app-service/app-service-authentication-overview.md
[Rychlý úvod ověřování]: app-service-mobile-auth.md

[Azure portálu]: https://portal.azure.com/
[OData]: http://www.odata.org
[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[todo sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[samples directory on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js Tools 1.1 for Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js package]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
