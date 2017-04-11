<properties
    pageTitle="Začínáme s aplikací rozhraní API a ASP.NET v aplikaci služby | Microsoft Azure"
    description="Naučte se vytvářet, nasazení a používání aplikace rozhraní API technologie ASP.NET v aplikaci služby Azure pomocí aplikace Visual Studio 2015."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="dotnet"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/20/2016"
    ms.author="rachelap"/>

# <a name="get-started-with-api-apps-aspnet-and-swagger-in-azure-app-service"></a>Začínáme s rozhraní API aplikace ASP.NET a Swagger v aplikaci služby Azure

[AZURE.INCLUDE [selector](../../includes/app-service-api-get-started-selector.md)]

Toto je první řady kurzy, které ukazují, jak používat funkce služby Azure aplikace, které jsou užitečné pro vývoj a hostování RESTful rozhraní API.  Tento kurz zahrnuje podpory pro rozhraní API metadata ve formátu Swagger.

Se dozvíte:

* Postup vytvoření a nasazení [aplikace rozhraní API](app-service-api-apps-why-best-platform.md) aplikace služby Azure pomocí nástroje integrovaná Visual Studio 2015.
* Jak k automatizaci rozhraní API zjišťování pomocí balíček Swashbuckle NuGet dynamicky generovat Swagger rozhraní API metadata.
* Jak používat Swagger rozhraní API metadat k automatickému vygenerování kódu klienta pro rozhraní API aplikace.

## <a name="sample-application-overview"></a>Ukázkové aplikace přehled

V tomto kurzu pracujete s aplikací ukázkový seznam jednoduché úkolů. Aplikace má front-end jednostránkové aplikace (zabezpečené ověřování HESLA), střední úroveň rozhraní API webových ASP.NET a osy dat rozhraní API webových ASP.NET.

![Rozhraní API aplikace ukázka aplikace diagramu](./media/app-service-api-dotnet-get-started/noauthdiagram.png)

Snímek obrazovky s [AngularJS](https://angularjs.org/) front-end tady je.

![Rozhraní API aplikace vzorové aplikace seznam úkolů](./media/app-service-api-dotnet-get-started/todospa.png)

Řešení Visual Studio zahrnuje tři projekty:

![](./media/app-service-api-dotnet-get-started/projectsinse.png)

* **ToDoListAngular** - front-end: AngularJS zabezpečené ověřování HESLA, volající střední vrstvě.

* **ToDoListAPI** – střední úroveň: rozhraní API webových ASP.NET projekt, který volá osy dat provádět operace CRUD na položek úkolů.

* **ToDoListDataAPI** - osy dat: rozhraní API webových ASP.NET projekt, který provádí operace CRUD na položek úkolů.

Tři vrstvy architektury je jednou z mnoha architektury, které tyto změny provést pomocí rozhraní API aplikace a je zde použít pouze pro účely ukázky. Kód v každé osy je co nejjednodušší, která ukazuje rozhraní API aplikace funkcí. například osy dat používá paměti serveru místo databáze jako jeho trvalé mechanismus.

K dokončení tohoto kurzu, budete mít dva projekty rozhraní API webových nahoru a spuštěné v cloudu v aplikacích pro rozhraní API aplikace služby.

Na další kurz v řadě nasadí front-end zabezpečené ověřování HESLA do cloudu.

## <a name="prerequisites"></a>Zjistit předpoklady pro

* Rozhraní API webových ASP.NET – kurz pokynů předpokládá, že máte základní znalost jak pracovat s ASP.NET [webového rozhraní API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) ve Visual Studiu.

* Azure účet – můžete [Otevřít účet Azure zdarma](/pricing/free-trial/?WT.mc_id=A261C142F) nebo [Aktivovat Visual Studio účastnická výhod](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

    Pokud chcete začít pracovat s aplikaci služby Azure před registraci účet Azure, přejděte na [Zkuste aplikaci služby](http://go.microsoft.com/fwlink/?LinkId=523751). Tady můžete okamžitě vytvořit aplikaci krátkodobý starter v aplikaci služby – **bez platební kartou povinné**a bez závazky.

* Visual Studio 2015 se v [Azure SDK pro .NET](https://azure.microsoft.com/downloads/archive-net-downloads/) – SDK Visual Studio 2015 nainstaluje automaticky pokud ještě nemáte ho.

    * Ve Visual Studiu, klikněte na tlačítko Nápověda -> o aplikaci Microsoft Visual Studio a přesvědčte se, že jste "Nástroje služby Azure aplikace v2.9.1" nebo vyšší.

    ![Azure vesion nástroje aplikace](./media/app-service-api-dotnet-get-started/apiversion.png)

    >[AZURE.NOTE] Podle toho, kolik závislostí SDK už máte v počítači instalací v SDK může chvíli trvat, z několika minut zabere nejméně půl hodiny.

## <a name="download-the-sample-application"></a>Stáhněte si aplikaci ukázka

1. Stáhněte si [Azure-Samples/app-service-api-dotnet-to-do-list](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) úložiště.

    Můžete kliknutím na tlačítko **Stáhnout ZIP** nebo klonovat úložiště na místním počítači.

2. Otevřete seznam úkolů řešení ve Visual Studiu 2015 nebo 2013.
   1. Musíte se s informacemi o důvěryhodnosti všech řešení.
        ![Upozornění zabezpečení](./media/app-service-api-dotnet-get-started/securitywarning.png)

3. Vytvoření řešení (CTRL + SHIFT + B) obnovíte balíčků NuGet.

    Pokud chcete zobrazit aplikace v operaci předtím, než ho nasadit, můžete ji spustit místně. Přesvědčte se, jestli ToDoListDataAPI po spuštění projektu a spusťte řešení. Můžete očekávat zobrazíte chybu HTTP 403 v prohlížeči.

## <a name="use-swagger-api-metadata-and-ui"></a>Použití metadat Swagger rozhraní API a uživatelské rozhraní

Podpora pro [Swagger](http://swagger.io/) 2.0 metadata rozhraní API je integrovaná v aplikaci služby Azure. Každý rozhraní API aplikace můžete určit URL koncový bod, který vrátí metadat pro rozhraní API ve formátu Swagger JSON. Metadata vrácených z tohoto koncového bodu mohou sloužit k vygenerování kódu pro klienta.

Rozhraní API webových ASP.NET projektu můžete dynamicky generovat Swagger metadat pomocí balíčku NuGet [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) . Balíček Swashbuckle NuGet už nainstalovaný v ToDoListDataAPI a ToDoListAPI projekty, které jste si stáhli.

V této části kurzu prohlédněte vygenerovaných metadata Swagger 2.0 a pak zkuste test uživatelské rozhraní, které je založená na Swagger metadata.

1. Nastavte ToDoListDataAPI projektu (**Ne** ToDoListAPI project) jako po spuštění projektu.

    ![Nastavení ToDoDataAPI jako projekt při spuštění](./media/app-service-api-dotnet-get-started/startupproject.png)

2. Stisknutím klávesy F5 nebo klikněte na **Ladění > Spustit ladění** ke spuštění projektu v režimu ladění.

    V prohlížeči se otevře a zobrazí chybovou stránku HTTP 403.

3. Do adresního řádku prohlížeče, přidejte `swagger/docs/v1` na konec řádku a stiskněte klávesu ENTER. (Adresa URL je `http://localhost:45914/swagger/docs/v1`.)

    Toto je výchozí adresu URL používané Swashbuckle vrátit Swagger 2.0 JSON metadata pro rozhraní API.

    Pokud používáte Internet Explorer, prohlížeči vás vyzve k stažení souboru *v1.json* .

    ![Stáhněte si JSON metadat v aplikaci Internet Explorer](./media/app-service-api-dotnet-get-started/iev1json.png)

    Pokud používáte Chrome, Firefox nebo okraje, prohlížeč ve formátu JSON zobrazí v okně prohlížeče. Jednotlivé prohlížeče zpracovat JSON jinak a okna prohlížeče nemusí vypadat stejně jako v příkladu.

    ![JSON metadata v chromu](./media/app-service-api-dotnet-get-started/chromev1json.png)

    Následující příklad ukazuje první část Swagger metadat pro rozhraní API s definicí pro metodu Get. Co jednotky Swagger uživatelského rozhraní použijete v následujících krocích je tato metadata a používáte v další části výukový kurz k automatickému vygenerování kódu klienta.

        {
          "swagger": "2.0",
          "info": {
            "version": "v1",
            "title": "ToDoListDataAPI"
          },
          "host": "localhost:45914",
          "schemes": [ "http" ],
          "paths": {
            "/api/ToDoList": {
              "get": {
                "tags": [ "ToDoList" ],
                "operationId": "ToDoList_GetByOwner",
                "consumes": [ ],
                "produces": [ "application/json", "text/json", "application/xml", "text/xml" ],
                "parameters": [
                  {
                    "name": "owner",
                    "in": "query",
                    "required": true,
                    "type": "string"
                  }
                ],
                "responses": {
                  "200": {
                    "description": "OK",
                    "schema": {
                      "type": "array",
                      "items": { "$ref": "#/definitions/ToDoItem" }
                    }
                  }
                },
                "deprecated": false
              },

4. Zavřete prohlížeč a ukončení aplikace Visual Studio ladění.

5. V projectu ToDoListDataAPI v **Okně Průzkumník**otevřít soubor *App_Start\SwaggerConfig.cs* a pak posuňte se dolů na řádku 174 a zrušte komentář následující kód.

        /*
            })
        .EnableSwaggerUi(c =>
            {
        */

    Soubor *SwaggerConfig.cs* se vytvoří při instalaci balíček Swashbuckle v projektu. Soubor přináší celou řadu možností, jak nakonfigurovat Swashbuckle.

    Kód, který jste uncommented umožňuje Swagger uživatelského rozhraní použijete v následujících krocích. Při vytváření rozhraní API webových projektu pomocí šablony rozhraní API aplikace project tento kód je přidán komentář se ve výchozím nastavení jako míra zabezpečení.

6. Znova spusťte projektu.

7. Do adresního řádku prohlížeče, přidejte `swagger` na konec řádku a stiskněte klávesu ENTER. (Adresa URL je `http://localhost:45914/swagger`.)

8. Až se zobrazí stránka Swagger uživatelského rozhraní, klikněte na **seznam úkolů** zobrazit dostupné metody.

    ![Dostupné metody swagger uživatelského rozhraní](./media/app-service-api-dotnet-get-started/methods.png)

9. Klikněte na první kliknutím na tlačítko **získat** v seznamu.

10. V části **Parametry** napište hvězdičku jako hodnotu `owner` parametr a potom klikněte na **vyzkoušet**.

    Po přidání ověřování na pozdější výukové programy pro střední úroveň poskytne skutečné uživatelské ID osy dat. Prozatím všechny úkoly bude mít hvězdička jako své ID vlastníka při spuštění aplikace bez ověření povolené.

    ![Swagger uživatelského rozhraní vyzkoušet](./media/app-service-api-dotnet-get-started/gettryitout1.png)

    Uživatelské rozhraní Swagger hovorů získat seznam úkolů metoda a zobrazí kód odpovědi a JSON výsledky.

    ![Swagger uživatelského rozhraní vyzkoušet výsledků](./media/app-service-api-dotnet-get-started/gettryitout.png)

11. Klikněte na **Publikovat**a potom klikněte na pole v části **Schéma modelu**.

    Kliknutím na toto schéma modelu prefills textové pole, které můžete zadat hodnotu parametru pro metodu příspěvek. (Pokud to nefunguje v Internet Exploreru, použijte jiný prohlížeč nebo ručně zadat hodnotu parametru v dalším kroku.)  

    ![Swagger uživatelského rozhraní vyzkoušet příspěvku](./media/app-service-api-dotnet-get-started/post.png)

12. Změna formátu JSON v `todo` parametr při zadávání pole tak, aby to vypadá jako v následujícím příkladu nebo nahrazovat text popisu:

        {
          "ID": 2,
          "Description": "buy the dog a toy",
          "Owner": "*"
        }

13. Klikněte na **vyzkoušet si**.

    Rozhraní API seznam úkolů vrací kód HTTP 204 odpověď, která označuje úspěch.

14. Klikněte na tlačítko **získat** první a v této části stránky klikněte na tlačítko **vyzkoušet si** .

    Metoda odpověď získat nyní obsahuje nové položky.

15. Volitelné: Vyzkoušejte taky umístění, odstranění a získat tak, že ID metody.

16. Zavřete prohlížeč a ukončení aplikace Visual Studio ladění.

Swashbuckle spolupracuje jakýkoli rozhraní API webových ASP.NET projekt. Pokud chcete přidat generování metadat Swagger do existujícího projektu, nainstalujte balíček Swashbuckle.

>[AZURE.NOTE] Swagger metadata obsahuje jedinečné ID pro každou rozhraní API operaci. Ve výchozím nastavení může způsobit Swashbuckle Swagger operace duplicitní ID metod řadiče rozhraní API webových. Tím se stane, když ovladači má přetížený HTTP metody, jako například `Get()` a `Get(id)`. Informace o tom, jak řešit přetížení najdete v tématu [definice přizpůsobení Swashbuckle generované rozhraní API](app-service-api-dotnet-swashbuckle-customize.md). Pokud jste vytvořili rozhraní API webových projekt ve Visual Studiu pomocí šablony Azure rozhraní API aplikace, kód generovaný operace jedinečné ID automaticky přidají i do *SwaggerConfig.cs* soubor.  

## <a id="createapiapp"></a>Vytvoření aplikace pro rozhraní API v Azure a nasazení kódu k němu

V této části použijete Azure nástroje, které jsou součástí průvodci Visual Studio **Publikovat Web** při vytváření nové aplikace rozhraní API v Azure. Potom nasazení projektu ToDoListDataAPI novou aplikaci rozhraní API a volání rozhraní API spuštěním Swagger uživatelského rozhraní.

1. V **Okně Průzkumník řešení**klikněte pravým tlačítkem ToDoListDataAPI projektu a potom klikněte na **Publikovat**.

    ![Klikněte na Publikovat ve Visual Studiu](./media/app-service-api-dotnet-get-started/pubinmenu.png)

2.  V kroku **profilu** průvodce **Publikovat Web** klikněte na **Microsoft Azure aplikaci služby**.

    ![Klikněte na Azure aplikaci služby v Publikovat Web](./media/app-service-api-dotnet-get-started/selectappservice.png)

3. Pokud jste tak již neučinili přihlášení k účtu Azure nebo aktualizovat svoje přihlašovací údaje, pokud jste platnost.

4. V dialogovém okně aplikaci služby vyberte Azure **předplatné** , které chcete použít a potom klikněte na **Nový**.

    ![Klikněte na tlačítko Nový v dialogovém okně aplikace služby](./media/app-service-api-dotnet-get-started/clicknew.png)

    Zobrazí se karta **hostitelské** dialogového okna **Vytvořit aplikaci služby** .

    Protože nasazujete rozhraní API webových projektu, který má Swashbuckle nainstalovaný, Visual Studio předpokládá, že chcete vytvořit aplikaci pro rozhraní API. To je uvedeno podle názvu **rozhraní API aplikace** a tím rozevíracího seznamu **Změnit typ** nastavený na **Rozhraní API aplikace**.

    ![Typ aplikace v dialogovém okně aplikace služby](./media/app-service-api-dotnet-get-started/apptype.png)

5. Zadejte **Název rozhraní API aplikace** , který je v doméně *azurewebsites.net* jedinečný. Můžete přijmout výchozí název, který navrhne Visual Studio.

    Pokud zadáte název, který někdo jiný má použít, se zobrazí červený vykřičník vpravo.

    Adresa URL aplikace rozhraní API bude `{API app name}.azurewebsites.net`.

6. V rozevíracím seznamu **Pole Skupina zdroje** klikněte na **Nový**a podle potřeby zadejte "ToDoListGroup" nebo jiný název.

    Skupina zdroje je kolekce Azure zdroje, jako jsou rozhraní API aplikace databází, VMs a tak dále. Pro účely tohoto návodu je vhodné vytvořit nové skupiny prostředků, protože, který usnadňuje odstranit v jednom kroku Azure prostředky, které je pro kurz vytvořit.

    Toto políčko umožňuje vybrat existující [pole Skupina zdroje](../azure-resource-manager/resource-group-overview.md) nebo vytvořit novou tak, že zadáte název, který se liší od libovolné existující skupině zdrojů ve vašem předplatném.

7. Klikněte na tlačítko **Nový** vedle **Plánu služby aplikace** rozevíracího seznamu.

    Snímek obrazovky zobrazuje ukázkové hodnoty **Název rozhraní API aplikace**, **předplatné**a **Pole Skupina zdroje** a že hodnoty budou lišit.

    ![Vytvoření dialogového okna aplikace služby](./media/app-service-api-dotnet-get-started/createas.png)

    V následujících krocích vytvořit aplikaci služby plánu pro nové skupiny prostředků. Plán služeb aplikací určuje výpočetním prostředky, které rozhraní API aplikace běží na. Například pokud se rozhodnete bezplatné osy, rozhraní API aplikace je spuštěn sdílené VMs spuštěn pro některé placené úrovní ho na vyhrazené VMs. Informace o různých plánech aplikaci služby najdete v tématu [Přehled plány aplikaci služby](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

8. V dialogovém okně **Konfigurovat plán služeb aplikace** zadejte "ToDoListPlan" nebo jiný název podle potřeby.

9. V rozevíracím seznamu **umístění** vyberte umístění, které je nejblíž vám.

    Tohle nastavení určuje, které Azure datacentra aplikace se spustí v. Vyberte umístění zavřít můžete minimalizovat [latence](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090).

10. V rozevíracím seznamu **velikost** klikněte na položku **Free**.

    Pro účely tohoto návodu poskytne bezplatná ceny osy dostatečně výkon.

11. V dialogovém okně **Konfigurovat plán služeb aplikací** klikněte na **OK**.

    ![Klikněte na OK v konfigurovat plán služeb aplikací](./media/app-service-api-dotnet-get-started/configasp.png)

12. V dialogovém okně **Vytvořit aplikaci služby** klikněte na **vytvořit**.

    ![Klikněte na vytvořit v dialogovém okně Vytvořit aplikaci služby](./media/app-service-api-dotnet-get-started/clickcreate.png)

    Visual Studio vytvoří rozhraní API aplikace a publikovat profil, který obsahuje všechny požadované možnosti pro rozhraní API aplikace. Potom otevře průvodce **Publikovat Web** , který budete používat pro nasazení projektu.

    Otevře se průvodce **Web publikování** na kartě **připojení** (viz níže).

    Na kartě **připojení** nastavení **serveru** a **název webu** přejděte na příkaz rozhraní API aplikace. **Uživatelské jméno** a **heslo** jsou nasazení přihlašovací údaje, které pro vás vytvoří Azure. Po nasazení Visual Studio otevře prohlížeči na adresu **Cílovou adresu URL** (slouží jenom pro **Cílovou adresu URL**).  

13. Klikněte na tlačítko **Další**.

    ![Klikněte na další na kartě připojení webu publikování](./media/app-service-api-dotnet-get-started/connnext.png)

    Na další kartu je karta **Nastavení** (viz níže). Tady můžete změnit na kartě vytvořit konfigurace nasazení sestavení ladění pro [vzdálené ladění](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug). Kartu také nabízí několik **Možností publikovat souborů**:

    * Odebrání další soubory v cílové
    * Předkompilovat při publikování
    * Vyloučit soubory ve složce App_Data

    Tento kurz nepotřebujete jeden z těchto kroků. Podrobné vysvětlení co dělat, najdete v tématu [Postup: nasazení Web projektu pomocí publikování jedním kliknutím ve Visual Studiu](https://msdn.microsoft.com/library/dd465337.aspx).

14. Klikněte na tlačítko **Další**.

    ![Klikněte na další v nastavení na kartě Publikovat Web](./media/app-service-api-dotnet-get-started/settingsnext.png)

    Je další karty **Náhled** (viz níže), která nabízí možnost zobrazit soubory, které se chystáte z projektu zkopírují do aplikace rozhraní API. Když máte nasazení projektu do aplikace rozhraní API pro, které jste už nasazené na dříve, zkopírovány pouze změněné soubory. Pokud chcete zobrazit seznam zkopírovat, kliknete na tlačítko **Start náhled** .

15. Klikněte na **Publikovat**.

    ![Klikněte na Publikovat v náhled na kartě Publikovat Web](./media/app-service-api-dotnet-get-started/clickpublish.png)

    Visual Studio nasazuje ToDoListDataAPI projektu do nového rozhraní API aplikace. V okně **výstupu** protokoly úspěšné nasazení a "úspěšně vytvořena" stránka zobrazená v okně prohlížeče otevřít na adresu URL rozhraní API aplikace.

    ![Výstup okno úspěšné nasazení](./media/app-service-api-dotnet-get-started/deploymentoutput.png)

    ![Nová stránka aplikace úspěšně vytvořili rozhraní API](./media/app-service-api-dotnet-get-started/appcreated.png)

16. Přidání adresy URL v adresním řádku prohlížeče "swagger" a stiskněte klávesu Enter. (Adresa URL je `http://{apiappname}.azurewebsites.net/swagger`.)

    Prohlížeč zobrazí stejného Swagger uživatelského rozhraní, které jste dříve viděli, ale teď běží v cloudu. Vyzkoušejte si metodu Get a zjistíte, zda jste zpátky na výchozí 2 úkoly. Dříve provedené změny byly uloženy v paměti v místním počítači.

17. Otevřete [portál Azure](https://portal.azure.com/).

    Azure portál je webového rozhraní pro správu Azure zdroje, jako jsou rozhraní API aplikace.

18. Klikněte na **Další služby > aplikace služby**.

    ![Procházet aplikace služby](./media/app-service-api-dotnet-get-started/browseas.png)

19. V **Aplikaci služby** zásuvné najděte a klikněte na nová rozhraní API aplikace. (Na portálu Azure okénka, která otevřete vpravo se označují jako *listy*.)

    ![Aplikace služby zásuvné](./media/app-service-api-dotnet-get-started/choosenewapiappinportal.png)

    Otevřete dva listy. Jeden zásuvné obsahuje základní informace o rozhraní API aplikace a jeden má dlouhý seznam nastavení, které můžou prohlížet a měnit.

20. V **Nastavení** zásuvné najděte část **rozhraní API LYNCU** a klikněte na **Rozhraní API definice**.

    ![Definice rozhraní API v nastavení zásuvné](./media/app-service-api-dotnet-get-started/apidefinsettings.png)

    **Rozhraní API definice** zásuvné umožňuje zadejte adresu URL, která vrací Swagger 2.0 metadat ve formátu JSON. Když Visual Studio vytvoří rozhraní API aplikace, nastaví adresu URL definice rozhraní API na výchozí hodnotu pro generované Swashbuckle metadata, která jste viděli dříve, což je rozhraní API aplikace je základní adresu URL plus `/swagger/docs/v1`.

    ![Rozhraní API definice URL](./media/app-service-api-dotnet-get-started/apidefurl.png)

    Po výběru generovat kód klienta pro něj rozhraní API aplikace Visual Studio načte metadata z této adresy URL.

## <a id="codegen"></a>Generovat kód klienta pro osy dat

Jednou z výhod integrace Swagger do Azure rozhraní API aplikace je generování automatické kódu. Třídy vygenerovaných klientů snadněji psát kód, který volá rozhraní API aplikace.

Projekt ToDoListAPI již obsahuje kód generovaný klienta, ale v následujících krocích budete ji odstranit a znovu vytvořit tuto značku zobrazíte postup generování kódu.

1. Ve Visual Studiu **Průzkumník řešení**v projektu ToDoListAPI odstraňte složku *ToDoListDataAPI* . **Upozornění: Odstraňte pouze složku, ne project ToDoListDataAPI.**

    ![Odstranění kód generovaný klienta](./media/app-service-api-dotnet-get-started/deletecodegen.png)

    Tato složka byla vytvořená pomocí proces vytváření kód, který se chystáte absolvovat.

2. Klikněte pravým tlačítkem myši na ToDoListAPI projekt a potom klikněte na **Přidat > REST API klienta**.

    ![Přidání rozhraní REST API klienta ve Visual Studiu](./media/app-service-api-dotnet-get-started/codegenmenu.png)

3. V dialogovém okně **Přidat REST API klienta** kliknout **Swagger adresu URL**a potom klikněte na **Vybrat materiály Azure**.

    ![Vyberte Azure materiálů](./media/app-service-api-dotnet-get-started/codegenbrowse.png)

4. V dialogovém okně **Aplikaci služby** rozbalte skupina zdroje používáte pro účely tohoto návodu a vyberte rozhraní API aplikace a potom klikněte na **OK**.

    ![Vyberte aplikaci rozhraní API pro generování kódu](./media/app-service-api-dotnet-get-started/codegenselect.png)

    Všimněte si, že když se vrátíte do dialogového okna **Přidat REST API klienta** , textového pole má vyplněn definici rozhraní API hodnotu adresy URL, která jste viděli v portálu.

    ![Rozhraní API definice URL](./media/app-service-api-dotnet-get-started/codegenurlplugged.png)

    >[AZURE.TIP] Alternativní způsob, jak získat metadat pro generování kódu je zadejte adresu URL přímo přímo – ne přes dialogové okno Procházet. Nebo pokud budete chtít generovat kód klienta před nasazením Azure, může projekt rozhraní API webových místně spustit, přejděte na adresu URL, které obsahuje soubor Swagger JSON uložit soubor a použít možnost **Vybrat existující soubor metadat Swagger** .

5. V dialogovém okně **Přidat REST API klient** klikněte na **OK**.

    Visual Studio vytvoří složku nazvanou rozhraní API aplikace a vygeneruje klientských tříd.

    ![Soubory kód generovaný klienta](./media/app-service-api-dotnet-get-started/codegenfiles.png)

6. V projectu ToDoListAPI otevřete *Controllers\ToDoListController.cs* zobrazíte kód v řádku 40, který volá rozhraní API pomocí vygenerovaných klienta.

    Následující úryvek ukazuje, jak kód vytvoří objektu klienta a volá metodu načíst.

        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));
            return client;
        }

        public async Task<IEnumerable<ToDoItem>> Get()
        {
            using (var client = NewDataAPIClient())
            {
                var results = await client.ToDoList.GetByOwnerAsync(owner);
                return results.Select(m => new ToDoItem
                {
                    Description = m.Description,
                    ID = (int)m.ID,
                    Owner = m.Owner
                });
            }
        }

    Parametr konstruktoru získá URL koncový bod z `toDoListDataAPIURL` nastavení aplikace. V souboru Web.config daná hodnota nastavenou místní IIS Express adres URL aplikace project rozhraní API tak, aby místní spuštění aplikace. Pokud vynechání parametru konstruktor je výchozí koncový bod adresu URL, které vygenerovalo kód z.

7. Vygeneruje se svojí třídě klient s jiným názvem podle názvu rozhraní API aplikace; Změňte kód v *Controllers\ToDoListController.cs* tak, aby odpovídalo název typu co byl vytvořen v projektu. Například pokud název svého rozhraní API aplikace ToDoListDataAPI071316 změníte tento kód:

        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));

takto:

        private static ToDoListDataAPI071316 NewDataAPIClient()
        {
            var client = new ToDoListDataAPI071316(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));


## <a name="create-an-api-app-to-host-the-middle-tier"></a>Vytvoření aplikace pro rozhraní API hostovat střední vrstvy

Dřív jste [vytvořili rozhraní API aplikace osy dat a nasazení kódu k němu](#createapiapp).  Teď použijte stejný postup rozhraní API aplikace střední vrstvě.

1. V **Okně Průzkumník řešení**, klikněte pravým tlačítkem myši střední úroveň ToDoListAPI project (ne dat úrovně ToDoListDataAPI) a potom klikněte na **Publikovat**.

    ![Klikněte na Publikovat ve Visual Studiu](./media/app-service-api-dotnet-get-started/pubinmenu2.png)

2.  Na kartě **profil** průvodce **Publikovat Web** klikněte na **Microsoft Azure aplikaci služby**.

3. V dialogovém okně **Aplikaci služby** klikněte na **Nový**.

4. Na kartě **hostitelské** dialogového okna **Vytvořit aplikaci služby** přijmout výchozí **Název rozhraní API aplikace** nebo zadejte název, který je v doméně *azurewebsites.net* jedinečný.

5. Zvolte Azure **předplatné** jste pracovali.

6. V rozevíracím seznamu **Pole Skupina zdroje** zvolte stejné skupiny prostředků, které jste dříve vytvořili.

7. V rozevíracím seznamu **Plán služeb aplikací** vyberte stejný plán, který jste vytvořili. Výchozí bude tato hodnota.

8. Klikněte na **vytvořit**.

    Visual Studio vytvoří rozhraní API aplikace, se vytvoří profilu publikování a zobrazí **připojení** kroku průvodce **Publikovat Web** .

9.  V kroku **připojení** průvodce **Publikovat Web** klikněte na **Publikovat**.

    Visual Studio nasadí ToDoListAPI projektu na nové rozhraní API aplikace a otevře prohlížeči na adresu URL rozhraní API aplikace. Zobrazí se stránka "úspěšně vytvořené".

## <a name="configure-the-middle-tier-to-call-the-data-tier"></a>Konfigurace střední úroveň volání osy dat

Pokud jste volali už někdy střední úroveň rozhraní API aplikace nyní, by zkuste volat osy dat pomocí localhost adresy URL, která je pořád v nastavení(Web.config)). V této části zadejte adresu URL dat osy rozhraní API aplikace do nastavení prostředí rozhraní API aplikace střední vrstvě. Nastavení prostředí kód v aplikaci střední úroveň rozhraní API načte nastavení adresa URL osy dat, bude jeho přepsání co je v nastavení(Web.config)).

1. Přejděte na [portál Azure](https://portal.azure.com/)a potom přejděte na zásuvné **Rozhraní API aplikace** rozhraní API aplikace, který jste vytvořili hostovat TodoListAPI (střední vrstvy) projektu.

2. V aplikaci rozhraní API zásuvné **Nastavení** klikněte na **Nastavení aplikace**.

3. V **Nastavení aplikace** zásuvné rozhraní API aplikace přejděte dolů do části **Nastavení aplikace** a přidejte následující klíče a hodnoty. Hodnota bude adresu URL rozhraní API aplikace první jste publikovali v tomto kurzu.

  	| **Klíč** | toDoListDataAPIURL |
  	|---|---|
  	| **Hodnota** | Název https://{your dat osy rozhraní API aplikace} .azurewebsites .net |
  	| **Příklad** | https://todolistdataapi.azurewebsites.NET |

4. Klikněte na **Uložit**.

    ![Klikněte na Uložit nastavení aplikace](./media/app-service-api-dotnet-get-started/asinportal.png)

    Až kód získáte v Azure tuto hodnotu teď přepsat localhost adresy URL v nastavení(Web.config)).

## <a name="test"></a>Test

1. V okně prohlížeče přejděte na adresu URL nového střední úroveň rozhraní API aplikace, který jste právě vytvořili pro ToDoListAPI. Je možné zobrazovat si je tak, že kliknete na adresu URL v hlavním zásuvné rozhraní API aplikace na portálu.

2. Přidání adresy URL v adresním řádku prohlížeče "swagger" a stiskněte klávesu Enter. (Adresa URL je `http://{apiappname}.azurewebsites.net/swagger`.)

    Prohlížeč zobrazí stejného Swagger uživatelského rozhraní, které jste viděli dříve pro ToDoListDataAPI, ale teď `owner` není povinná pole pro operaci Get, protože střední úroveň rozhraní API aplikace odesílá tuto hodnotu k aplikaci rozhraní API osy dat za vás. (Když provedete výukové programy pro ověřování, střední úroveň odešle skutečné uživatelské ID `owner` parametr; pro teď ji je pevně zadány hvězdičku.)

3. Vyzkoušejte si metodu Get a jiné metody k ověření, že střední úroveň rozhraní API aplikace úspěšně volá rozhraní API aplikace osy dat.

    ![Způsob získání uživatelského rozhraní swagger](./media/app-service-api-dotnet-get-started/midtierget.png)

## <a name="troubleshooting"></a>Řešení potíží

V případě, že nastanou potíže při procházení tohoto kurzu je několik možných řešení potíží:

* Ujistěte se, že používáte nejnovější verzi [Azure SDK pro .NET](http://go.microsoft.com/fwlink/?linkid=518003).

* Dvě názvů projektů jsou podobné (ToDoListAPI, ToDoListDataAPI). Pokud věci nevypadají podle pokynů při práci s projektem, zkontrolujte, že jste otevřeli správný projekt.

* Pokud se v podnikové síti a pokoušíte nasazení služby Azure aplikace přes bránu firewall, ujistěte se, jestli jsou porty 443 a 8172 otevřené pro nasazení webu. Pokud nemůžete otevřete tyto porty, můžete použít jiné metody nasazení.  V tématu [Nasaďte aplikaci služby Azure aplikace](../app-service-web/web-sites-deploy.md).

* "Směrování názvy musí být jedinečné" chyby – můžete získat tyto Pokud omylem nasazení nepovedlo projektu do aplikace pro rozhraní API a novější nasazení ten správný k němu. Tento problém můžete vyřešit, opětovném nasazení správný projekt rozhraní API aplikace a na kartě **Nastavení** v průvodci **Publikovat Web** vyberte **odebrat další soubory v cílové**.

Až budete mít ASP.NET rozhraní API aplikace spuštěné v aplikaci služby Azure, můžete další informace o funkcích aplikace Visual Studio usnadňujících Poradce při potížích. Informace o protokolování vzdálené ladění a informace, najdete v tématu [Poradce při potížích s aplikaci služby Azure aplikace ve Visual Studiu](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).

## <a name="next-steps"></a>Další kroky

Ukázali jsme si jak nasazení existující projekty rozhraní API webových rozhraní API aplikace, generovat kód klienta pro rozhraní API aplikace a používání rozhraní API aplikace klientů .NET. Zobrazuje na další kurz v této řadě používání [CORS využívat rozhraní API aplikace klientů JavaScript](app-service-api-cors-consume-javascript.md).

Další informace o generování kódu klienta najdete v článku [Azure/AutoRest](https://github.com/azure/autorest) úložiště na GitHub.com. Nápovědu k problémům pomocí vygenerovaných klienta se otevřete [problém v úložišti AutoRest](https://github.com/azure/autorest/issues).

Pokud chcete vytvořit nové projekty rozhraní API aplikace od začátku, použijte šablony **Azure rozhraní API aplikace** .

![Šablona rozhraní API aplikace ve Visual Studiu](./media/app-service-api-dotnet-get-started/apiapptemplate.png)

Šablona **Azure rozhraní API aplikace** project odpovídá výběr **prázdné** ASP.NET 4.5.2 šablony, kliknutím zaškrtněte políčko Přidat rozhraní API webových podporu a instalace balíček Swashbuckle NuGet. Kromě toho šablona přidá zabránit vytváření duplicitních Swagger operace ID některé Swashbuckle konfiguračního kódu. Po vytvoření rozhraní API aplikace project můžete můžete ho nasadit do aplikace pro rozhraní API stejným způsobem jako jste viděli v tomto kurzu.
