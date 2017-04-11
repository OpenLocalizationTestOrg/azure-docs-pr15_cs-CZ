<properties
    pageTitle="CORS podpory pro aplikaci služby | Microsoft Azure"
    description="Naučte se používat CORS podpora služby Azure Azure aplikace."
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
    ms.topic="get-started-article"
    ms.date="08/27/2016"
    ms.author="rachelap"/>

# <a name="consume-an-api-app-from-javascript-using-cors"></a>Používání aplikace rozhraní API ze JavaScript pomocí CORS

Aplikace služby nabízí předdefinované podporu [Více Origin zdroje sdílení (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing), což umožňuje klientům JavaScript volat doménami rozhraní API, které jsou hostované v rozhraní API aplikace. Aplikace služby je možné konfigurovat CORS přístup k vaší API bez psaní kódu vašeho rozhraní API systému.

Tento článek obsahuje dva oddíly:

* V části [jak nakonfigurovat CORS](#corsconfig) uvedený důvod obecně jak nakonfigurovat CORS pro rozhraní API aplikace, v prohlížeči nebo mobilní aplikaci. Použije pro všechny rámce podporovaných službami aplikaci služby včetně .NET, Node.js a Java rovnoměrně z vnější. 

* S částí [pokračováním výukové programy pro .NET Začínáme –](#tutorialstart) , v článku bude kurzu, který ukazuje že CORS podporují vytvořením na akce v [První rozhraní API aplikace získání Začínáme kurz](app-service-api-dotnet-get-started.md). 

## <a id="corsconfig"></a>Postup při konfiguraci CORS v aplikaci služby Azure

CORS můžete nakonfigurovat v portálu Azure nebo pomocí nástrojů pro [Správce prostředků Azure](../azure-resource-manager/resource-group-overview.md) .

#### <a name="configure-cors-in-the-azure-portal"></a>Konfigurace CORS na portálu Azure

8. V prohlížeči přejděte na [portál Azure](https://portal.azure.com/).

2. Klikněte na **Aplikaci služby**a potom klikněte na název svojí rozhraní API aplikace.

    ![Vyberte rozhraní API aplikace portálu](./media/app-service-api-cors-consume-javascript/browseapiapps.png)

10. V zásuvné **Nastavení** , která se otevře napravo od zásuvné **rozhraní API aplikace** vyhledejte část **rozhraní API LYNCU** a klikněte na **CORS**.

    ![Vyberte CORS v nastavení zásuvné](./media/app-service-api-cors-consume-javascript/clicksettings.png)

11. Pole text zadejte adresu URL nebo adresy URL, které chcete povolit JavaScript volání pochází z.


    Například, pokud jste nainstalovali aplikace JavaScriptu pro web app s názvem todolistangular, zadejte "https://todolistangular.azurewebsites.net". Jako alternativu můžete zadat hvězdičku (*) můžete určit, že všechny origin domény přijaté.


13. Klikněte na **Uložit**.

    ![Klikněte na tlačítko Uložit](./media/app-service-api-cors-consume-javascript/corsinportal.png)

    Po klepnutí na tlačítko **Uložit**rozhraní API aplikace bude přijímat hovory JavaScript ze zadané adresy URL.

#### <a name="configure-cors-by-using-azure-resource-manager-tools"></a>Konfigurace CORS pomocí nástrojů pro správce prostředků Azure

Můžete taky nakonfigurovat CORS rozhraní API aplikace pro pomocí [Správce prostředků Azure šablon](../resource-group-authoring-templates.md) v nástrojích příkazového řádku například [Azure PowerShell](../powershell-install-configure.md) a [Azure rozhraní příkazového řádku](../xplat-cli-install.md). 

Příklad šablony správce prostředků Azure, která nastavuje vlastnosti CORS otevřete [azuredeploy.json souboru v úložišti aplikace ukázkové tohoto kurzu](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json). Vyhledání části šablony, která vypadá jako v následujícím příkladu:

        "cors": {
            "allowedOrigins": [
                "todolistangular.azurewebsites.net"
            ]
        }

## <a id="tutorialstart"></a>Pokračování .NET Začínáme – kurz

Když sledujete Node.js nebo Java Začínáme – řadu pro rozhraní API aplikace, jste dokončili dosáhnout toho, aby Začínáme řady. Přejděte do oddílu [Další kroky](#next-steps) k vyhledání návrhy pro získání dalších informací o rozhraní API aplikace.

Zbývající část tohoto článku je pokračování .NET Začínáme – řady a předpokládá, že se úspěšně dokončilo [první kurzu](app-service-api-dotnet-get-started.md).

## <a name="deploy-the-todolistangular-project-to-a-new-web-app"></a>Nasazení projektu ToDoListAngular novou webovou aplikaci

V [první kurzu](app-service-api-dotnet-get-started.md)jste vytvořili rozhraní API aplikace střední vrstvy a z dat osy rozhraní API aplikace. V tomto kurzu vytvoříte web appu jednostránkové aplikace (zabezpečené ověřování HESLA), které aplikace volání rozhraní API střední úroveň. Pro zabezpečené ověřování HESLA pro práci se musí povolit CORS na střední úroveň rozhraní API aplikace. 

V [seznamu úkolů ukázková aplikace](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list)projekt ToDoListAngular je jednoduchý AngularJS klienta, který volá projektu rozhraní API webových ToDoListAPI střední vrstvě. Kód JavaScript v souboru *app/scripts/todoListSvc.js* volá rozhraní API pomocí zprostředkovatele AngularJS HTTP. 

        angular.module('todoApp')
        .factory('todoListSvc', ['$http', function ($http) {

            $http.defaults.useXDomain = true;
            delete $http.defaults.headers.common['X-Requested-With']; 
        
            return {
                getItems : function(){
                    return $http.get(apiEndpoint + '/api/TodoList');
                },

                /* Get by ID, Put, and Delete methods not shown */

                postItem : function(item){
                    return $http.post(apiEndpoint + '/api/TodoList', item);
                }
            };
        }]);

### <a name="create-a-new-web-app-for-the-todolistangular-project"></a>Vytvořit nový web app pro ToDoListAngular project

Postup vytvoření nové webové aplikace aplikaci služby a nasazení projektu je podobný viděli pro [Vytvoření a nasazení aplikace rozhraní API v první kurz v této řadě](app-service-api-dotnet-get-started.md#createapiapp). Jediný rozdíl je typ aplikace **V prohlížeči** místo **Rozhraní API aplikace**.  Snímky obrazovky dialogová okna najdete v článku 

1. V **Okně Průzkumník řešení**klikněte pravým tlačítkem ToDoListAngular projektu a potom klikněte na **Publikovat**.

3.  Na kartě **profil** průvodce **Publikovat Web** klikněte na **Microsoft Azure aplikaci služby**.

5. V dialogovém okně **Aplikaci služby** klikněte na **Nový**.

3. Na kartě **hostitelské** dialogového okna **Vytvořit aplikaci služby** zadejte **Název webové aplikace** , který je v doméně *azurewebsites.net* jedinečný. 

5. Zvolte Azure **předplatného** , kterým chcete pracovat s.

6. V rozevíracím seznamu **Pole Skupina zdroje** zvolte stejné skupiny prostředků, které jste dříve vytvořili.

4. V rozevíracím seznamu **Plán služeb aplikací** zvolte stejný plán, který jste vytvořili. 

7. Klikněte na **vytvořit**.

    Visual Studio vytvoří web appu, se vytvoří profilu publikování a zobrazí **připojení** kroku průvodce **Publikovat Web** .

    **Publikovat** zatím ještě neklikejte na. V následující části nakonfigurujte novou webovou aplikaci pro volání rozhraní API aplikace střední vrstvy, na kterém běží v aplikaci služby. 

### <a name="set-the-middle-tier-url-in-web-app-settings"></a>Nastavit adresu URL střední úroveň v nastavení web appu

1. Přejděte na [portál Azure](https://portal.azure.com/)a potom přejděte na zásuvné **Web App** pro web app, kterou jste vytvořili hostovat projektu TodoListAngular (front-end).

2. Klikněte na **Nastavení > Nastavení aplikace**.

3. V části **Nastavení aplikace** přidejte následující klíče a hodnoty:

  	|Klíč|Hodnota|Příklad
  	|---|---|---|
  	|toDoListAPIURL|název aplikace střední úroveň rozhraní API https://{your} .azurewebsites .net|https://todolistapi0121.azurewebsites.NET|

4. Klikněte na **Uložit**.

    Až kód získáte v Azure tato hodnota potlačí localhost adresy URL v *nastavení(Web.config))* . 

    Kód, který je načtena nastavení hodnota je v *index.cshtml*:

        <script type="text/javascript">
            var apiEndpoint = "@System.Configuration.ConfigurationManager.AppSettings["toDoListAPIURL"]";
        </script>
        <script src="app/scripts/todoListSvc.js"></script>

    Kód v *todoListSvc.js* na základě nastavení:

        return {
            getItems : function(){
                return $http.get(apiEndpoint + '/api/TodoList');
            },
            getItem : function(id){
                return $http.get(apiEndpoint + '/api/TodoList/' + id);
            },
            postItem : function(item){
                return $http.post(apiEndpoint + '/api/TodoList', item);
            },
            putItem : function(item){
                return $http.put(apiEndpoint + '/api/TodoList/', item);
            },
            deleteItem : function(id){
                return $http({
                    method: 'DELETE',
                    url: apiEndpoint + '/api/TodoList/' + id
                });
            }
        };

### <a name="deploy-the-todolistangular-web-project-to-the-new-web-app"></a>Nasazení projektu ToDoListAngular web na nový web appu

*  Ve Visual Studiu v **připojení** kroku průvodce **Publikovat Web** , klikněte na **Publikovat**.

    Visual Studio nasadí ToDoListAngular projektu na nový web appu a otevře prohlížeči na adresu URL webové aplikace. 

### <a name="test-the-application-without-cors-enabled"></a>Testování aplikace bez CORS povolena 

2. V prohlížeči nástrojů pro vývojáře otevřete okno konzoly.

3. V okně prohlížeče, který se zobrazuje v uživatelském rozhraní AngularJS klikněte na odkaz **Se seznamem úkolů** .

    Kód jazyka JavaScript pokouší volat aplikaci střední úroveň rozhraní API, ale hovor selhala, protože front-end běží v jiné doméně než back-end. Okno prohlížeče vývojář nástroje konzoly zobrazuje křížově origin chybová zpráva.

    ![Křížové origin chybová zpráva](./media/app-service-api-cors-consume-javascript/consoleaccessdenied.png)

## <a name="configure-cors-for-the-middle-tier-api-app"></a>Konfigurace CORS rozhraní API aplikace střední vrstvy

V této části nakonfigurujte nastavení CORS v Azure prostředním osy ToDoListAPI rozhraní API aplikace. Toto nastavení vám umožní střední úroveň rozhraní API aplikace z web appu, kterou jste vytvořili pro projekt ToDoListAngular přijímat hovory JavaScript.

8. V prohlížeči přejděte na [portál Azure](https://portal.azure.com/).

2. Klikněte na **Aplikaci služby**a klikněte na aplikaci rozhraní API ToDoListAPI (střední vrstvy).

    ![Vyberte rozhraní API aplikace portálu](./media/app-service-api-cors-consume-javascript/browseapiapps.png)

10. V zásuvné **Nastavení** , která se otevře napravo od zásuvné **rozhraní API aplikace** vyhledejte část **rozhraní API LYNCU** a klikněte na **CORS**.

    ![Vyberte CORS portálu](./media/app-service-api-cors-consume-javascript/clicksettings.png)

12. Do textového pole zadejte adresu URL pro web app ToDoListAngular (front-end). Například nasazení ToDoListAngular project web appu s názvem todolistangular0121 umožňují voláním z adresy URL `https://todolistangular0121.azurewebsites.net`.

    Jako alternativu můžete zadat hvězdičku (*) můžete určit, že všechny origin domény přijaté.

13. Klikněte na **Uložit**.

    ![Klikněte na tlačítko Uložit](./media/app-service-api-cors-consume-javascript/corsinportal.png)

    Po klepnutí na tlačítko **Uložit**rozhraní API aplikace bude přijímat hovory JavaScript ze zadané adrese URL. V tomto snímek ToDoListAPI0223 rozhraní API aplikace přijímá volání klientů JavaScript z web appu ToDoListAngular.

### <a name="test-the-application-with-cors-enabled"></a>Testování aplikace s CORS povolena

* Otevřete prohlížeč na adresu URL HTTPS ve web appu. 

    Tentokrát aplikace umožňuje zobrazit, přidat, upravit a odstranit položky k vyřízení. 

    ![Stránky aplikace ukázkový seznam úkolů](./media/app-service-api-cors-consume-javascript/corssuccess.png)

## <a name="app-service-cors-versus-web-api-cors"></a>Aplikace služby CORS versus webového rozhraní API CORS

V rozhraní API webových projektu můžete nainstalovat balíček [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) NuGet zadáte v kódu, které domény vaší rozhraní API přijímá volá JavaScript.
 
Podpora CORS rozhraní API webových je flexibilnější než CORS aplikace služby podpory. Například v kódu můžete určit různých typů přípustném metod různé akce, zatímco pro aplikaci služby CORS zadejte jednu sadu přípustném typů pro všechny aplikace rozhraní API metody.

> [AZURE.NOTE] Není pokusíte použít webového rozhraní API CORS a aplikace služeb CORS v jedné aplikaci rozhraní API. Aplikace služby CORS bude přednost a webového rozhraní API CORS bude mít žádný vliv. Například pokud povolíte jednu doménu origin v aplikaci služby a povolení všech domén origin v rozhraní API webových kódu, Azure rozhraní API aplikace bude pouze přijímat hovory z doménu, kterou jste zadali v Azure.

### <a name="how-to-enable-cors-in-web-api-code"></a>Jak povolit CORS v rozhraní API webových kódu

Podle těchto kroků souhrn proces pro povolení webového rozhraní API CORS podpory. Další informace najdete v tématu [Povolení více Origin žádostí o ve ASP.NET webového rozhraní API 2](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api).

1. V rozhraní API webových projektu nainstalujte balíček NuGet [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) .

1. Zahrnout `config.EnableCors()` řádku kódu v metodu **zaregistrovat** třídy **WebApiConfig** jako v následujícím příkladu. 

        public static class WebApiConfig
        {
            public static void Register(HttpConfiguration config)
            {
                // Web API configuration and services
                
                // The following line enables you to control CORS by using Web API code
                config.EnableCors();
    
                // Web API routes
                config.MapHttpAttributeRoutes();
    
                config.Routes.MapHttpRoute(
                    name: "DefaultApi",
                    routeTemplate: "api/{controller}/{id}",
                    defaults: new { id = RouteParameter.Optional }
                );
            }
        }

1. Na ovladači rozhraní API webových přidat `using` údajů pro `System.Web.Http.Cors` názvů a přidejte `EnableCors` atribut třídy řadiče nebo metody jednotlivé akcí. V následujícím příkladu CORS podpory platí pro celou správce.

        namespace ToDoListAPI.Controllers 
        {
            [HttpOperationExceptionFilterAttribute]
            [EnableCors(origins:"https://todolistangular0121.azurewebsites.net", headers:"accept,content-type,origin,x-my-header", methods: "get,post")]
            public class ToDoListController : ApiController
 
## <a name="using-azure-api-management-with-api-apps"></a>Správa Azure rozhraní API pomocí rozhraní API aplikace

Pokud používáte Správa rozhraní API Azure pomocí rozhraní API aplikace, konfigurace CORS správy rozhraní API ne v aplikaci rozhraní API. Další informace najdete v následujících zdrojích:

* [Přehled správy Azure rozhraní API (videa: CORS začíná hodnotou 12:10)](https://azure.microsoft.com/documentation/videos/azure-api-management-overview/)
* [Správa rozhraní API křížové zásady domény](https://msdn.microsoft.com/library/azure/dn894084.aspx#CORS)
 
## <a name="troubleshooting"></a>Řešení potíží

V případě, že narazíte na problém při procházení tohoto kurzu, tady je několik možných řešení potíží.

* Ujistěte se, že používáte nejnovější verzi [Azure SDK pro .NET pro Visual Studio 2015](http://go.microsoft.com/fwlink/?linkid=518003).

* Ujistěte se, že jste zadali `https` v nastavení CORS a ujistěte se, že používáte `https` chcete spustit front-end web app.

* Ujistěte se, zadali nastavení CORS rozhraní API aplikace střední vrstvy, nikoliv front-end web appu.

* Pokud jste konfiguraci CORS v kódu aplikace a služby Azure aplikace, mějte na paměti, že nastavení aplikace služeb CORS přednost před něco jiného, co děláte v kódu aplikace. 

Další informace o Visual Studio funkce, které usnadňují Poradce při potížích, najdete v článku [Poradce při potížích s aplikaci služby Azure aplikací ve Visual Studiu](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).

## <a name="next-steps"></a>Další kroky 

V tomto článku viděli, jak povolit CORS aplikace služby podpory, aby kód v JavaScriptu klienta upoutat rozhraní API v jiné doméně. Zobrazíte další informace o rozhraní API aplikace přečtěte si [Úvod do ověřování služby aplikace](../app-service/app-service-authentication-overview.md)a potom přejděte k části kurz [ověřování uživatelů pro rozhraní API aplikace](app-service-api-dotnet-user-principal-auth.md) .
