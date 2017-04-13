<properties
    pageTitle="Hybridní na místní/cloudu aplikaci (.NET) | Microsoft Azure"
    description="Naučte se vytvářet aplikace na místní/cloudu hybridní .NET pomocí přenosových Bus služby Azure."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="09/16/2016"
    ms.author="sethm"/>

# <a name="net-on-premisescloud-hybrid-application-using-azure-service-bus-relay"></a>Aplikace na místní/cloudu hybridní .NET pomocí předávací Bus služby Azure

## <a name="introduction"></a>Úvod

Tento článek popisuje, jak vytvářet cloudu hybridní aplikaci Microsoft Azure a Visual Studia. Kurz předpokládá, že máte žádné zkušenosti pomocí Azure. Za méně než 30 minut budete mít aplikaci, která používá více Azure zdrojů nahoru a spuštěné v cloudu.

Naučíte se:

-   Jak vytvořit nebo upravit stávající webové služby pro spotřebu webového řešení.
-   Jak sdílet data mezi Azure aplikací a webové služby pomocí služby pro přenos dat Bus služby Azure uložen jinde.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="how-the-service-bus-relay-helps-with-hybrid-solutions"></a>Jak relay Service Bus pomáhá slouží k řešení hybridní

Podniková řešení obvykle se skládají z kombinací vlastním kódem napsaným k řešení novou a jedinečnou obchodní požadavky a existující funkce poskytované řešení a systémy, které jsou již na místě.

Řešení tvůrci začínají využívat cloud pro snadnější zpracování hodnot měřítko požadavky a nižší provozní náklady. Přitom zjistí, že existující majetek služby, který chce využíval jsou stavební bloky jejich řešení uvnitř podnikovou bránu firewall a odhlášení od něj snadno dosáhnout přístup pomocí protokolu cloudové řešení. Mnoho interní služeb nejsou vytvořené nebo hostované tak, že budou můžete snadno umístěné na okraji podnikové síti.

Relay Service Bus je určený pro případ použití přijetí stávající webové služby Windows Communication Foundation (WCF) a zpřístupnění tyto služby zabezpečené řešení, které jsou umístěny mimo podnikovou síť bez nutnosti rušivá změny infrastruktury podnikové sítě. Tyto služby relay Service Bus dál hostovaný do existující prostředí, ale budou delegáta naslouchají příchozí relace a žádosti o služby Bus hostující v cloudu. Služba Bus také zabraňuje tyto služby neoprávněnému přístupu pomocí ověřování [Sdílené podpis aplikace Access](../service-bus-messaging/service-bus-sas-overview.md) (přidružení zabezpečení).

## <a name="solution-scenario"></a>Scénář řešení

V tomto kurzu vytvoříte ASP.NET Web, který umožňuje zobrazit seznam produktů na stránce stavu skladových zásob produktu.

![][0]

Kurz předpokládá, že mají informace o produktu v existující místní systém a používá relay Service Bus přístup do systému. To je simulovaného pomocí webové služby, která běží ve aplikace jednoduché konzoly a zálohy podle sady v paměti produktů. Bude moct spuštění této aplikace konzoly ve vašem počítači a nasazení roli web do Azure. Tímto způsobem, zobrazí se jak roli webovou aplikaci Azure datacentra skutečně vyzvedne do počítače, i když je váš počítač bude pravděpodobností nacházet za alespoň jednu bránu firewall a vrstvě sítě adresu překladu.

Následujícím obrázku je snímek obrazovky s úvodní stránka dokončené webové aplikace.

![][1]

## <a name="set-up-the-development-environment"></a>Nastavení vývojové prostředí:

Abyste mohli začít vývoj Azure aplikací, získat nástrojů a nastavení vývojové prostředí.

1.  Nainstalujte Azure SDK pro .NET ze stránky [získat nástroje a SDK][] .

2.  Klikněte na **instalovat v SDK** pro verzi aplikace Visual Studio používáte. Kroky v tomto kurzu pomocí aplikace Visual Studio 2015.

4.  Po zobrazení výzvy ke spuštění nebo uložení instalační program, klikněte na **Spustit**.

5.  **Webové platformy**klikněte na tlačítko **instalovat** a přejděte k instalaci.

6.  Po dokončení instalace budete mít všechno potřebné ke spuštění se dají aplikace. V SDK obsahuje nástroje, které umožňují snadno vyvíjet Azure aplikace ve Visual Studiu. Pokud nemáte nainstalované Visual Studio, nainstaluje SDK taky bezplatné Visual Studio Express.

## <a name="create-a-namespace"></a>Vytvořit obor názvů

Chcete-li v práci začít používat funkce Bus služby Azure, je nutné nejprve vytvořit obor názvů služby. Obor názvů poskytuje oboru kontejner adresování Bus služby zdrojů v aplikaci.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-an-on-premises-server"></a>Vytvoření místního serveru

Nejdřív vytvoříte (mock) místní systém katalogu produktů. Je velmi jednoduché. Zobrazí se tato jako představující skutečné místní systém katalogu produktů s úplnou služby povrchu, který jsme se snažíte integrace.

Tohoto projektu je aplikace Visual Studio konzoly a používá [NuGet Bus služby Azure balíčku](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) zahrnout služby Bus knihoven a nastavení.

### <a name="create-the-project"></a>Vytvoření projektu

1.  Pomocí oprávnění správce, spusťte aplikaci Microsoft Visual Studio. Spusťte aplikaci Visual Studio s oprávněními správce, klikněte pravým tlačítkem myši na ikonu programu **Visual Studio** a pak klikněte na **Spustit jako správce**.

2.  Ve Visual Studiu, v nabídce **soubor** klikněte na **Nový**a potom klikněte na **projekt**.

3.  Z **Nainstalované šablony**v části **Visual Basic**, klikněte na **Aplikace konzoly**. Do pole **název** zadejte název **ProductsServer**:

    ![][11]

4.  Klikněte na **OK** vytvořte **ProductsServer** projektu.

7.  Pokud jste již nainstalovali balíček správce NuGet for Visual Studio, přejděte k dalšímu kroku. Jinak Navštěvujte blog o [NuGet][] a klikněte na [Nainstalovat NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c). Postupujte podle pokynů k instalaci Správce balíčků NuGet a pak znova spusťte aplikaci Visual Studio.

7.  V Průzkumníku **ProductsServer** projektu klikněte pravým tlačítkem myši a potom klikněte na **Spravovat balíčků NuGet**.

8.  Klikněte na kartu **Procházet** a potom vyhledejte `Microsoft Azure Service Bus`. Klikněte na tlačítko **instalovat**a přijměte podmínky použití.

    ![][13]

    Všimněte si, že jsou teď uváděný sestavení požadovaný klient.

9.  Přidání nového třídy pro produkt smlouvy. V Průzkumníku klikněte pravým tlačítkem **ProductsServer** projektu a klikněte na tlačítko **Přidat**a potom klikněte na **Onenotové**.

10. Do pole **název** zadejte název **ProductsContract.cs**. Klikněte na **Přidat**.

11. V **ProductsContract.cs**Nahraďte definici názvů následující kód, který definuje smlouva pro službu.

    ```
    namespace ProductsServer
    {
        using System.Collections.Generic;
        using System.Runtime.Serialization;
        using System.ServiceModel;
    
        // Define the data contract for the service
        [DataContract]
        // Declare the serializable properties.
        public class ProductData
        {
            [DataMember]
            public string Id { get; set; }
            [DataMember]
            public string Name { get; set; }
            [DataMember]
            public string Quantity { get; set; }
        }
    
        // Define the service contract.
        [ServiceContract]
        interface IProducts
        {
            [OperationContract]
            IList<ProductData> GetProducts();
    
        }
    
        interface IProductsChannel : IProducts, IClientChannel
        {
        }
    }
    ```

12. V Program.cs Nahraďte definici názvů následující kód, který sečte služby profilů a hostitele ho.

    ```
    namespace ProductsServer
    {
        using System;
        using System.Linq;
        using System.Collections.Generic;
        using System.ServiceModel;
    
        // Implement the IProducts interface.
        class ProductsService : IProducts
        {
    
            // Populate array of products for display on website
            ProductData[] products =
                new []
                    {
                        new ProductData{ Id = "1", Name = "Rock",
                                         Quantity = "1"},
                        new ProductData{ Id = "2", Name = "Paper",
                                         Quantity = "3"},
                        new ProductData{ Id = "3", Name = "Scissors",
                                         Quantity = "5"},
                        new ProductData{ Id = "4", Name = "Well",
                                         Quantity = "2500"},
                    };
    
            // Display a message in the service console application
            // when the list of products is retrieved.
            public IList<ProductData> GetProducts()
            {
                Console.WriteLine("GetProducts called.");
                return products;
            }
    
        }
    
        class Program
        {
            // Define the Main() function in the service application.
            static void Main(string[] args)
            {
                var sh = new ServiceHost(typeof(ProductsService));
                sh.Open();
    
                Console.WriteLine("Press ENTER to close");
                Console.ReadLine();
    
                sh.Close();
            }
        }
    }
    ```

13. V Průzkumníku poklikejte na soubor **App.config** a otevřete ji v editoru Visual Studio. V dolní části ** &lt;systému. ServiceModel&gt; ** prvek (ale pořád v rámci &lt;systému. ServiceModel&gt;), přidat následující kód XML. Ujistěte se, že *yourServiceNamespace* nahraďte názvem obor názvů a *yourKey* s klávesou přidružení zabezpečení se obnovená dříve z portálu:

    ```
    <system.serviceModel>
    ...
      <services>
         <service name="ProductsServer.ProductsService">
           <endpoint address="sb://yourServiceNamespace.servicebus.windows.net/products" binding="netTcpRelayBinding" contract="ProductsServer.IProducts" behaviorConfiguration="products"/>
         </service>
      </services>
      <behaviors>
         <endpointBehaviors>
           <behavior name="products">
             <transportClientEndpointBehavior>
                <tokenProvider>
                   <sharedAccessSignature keyName="RootManageSharedAccessKey" key="yourKey" />
                </tokenProvider>
             </transportClientEndpointBehavior>
           </behavior>
         </endpointBehaviors>
      </behaviors>
    </system.serviceModel>
    ```
14. Pořád v App.config, v ** &lt;appSettings&gt; ** prvek, nahradit připojení řetězcem hodnotu připojení dříve získat z portálu. 

    ```
    <appSettings>
    <!-- Service Bus specific app settings for messaging connections -->
    <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```

14. Stiskněte **Kombinaci kláves Ctrl + Shift + B** nebo v nabídce **sestavení** klikněte na tlačítko **Sestavit řešení** sestavení aplikace a ověřte správnost pracovní zatím.

## <a name="create-an-aspnet-application"></a>Vytvoření aplikace technologie ASP.NET

V této části vytvoříte jednoduché ASP.NET aplikace, která se zobrazí data načtená z služby produktu.

### <a name="create-the-project"></a>Vytvoření projektu

1.  Ujistěte se, jestli je spuštěný Visual Studio s oprávněními správce.

2.  Ve Visual Studiu, v nabídce **soubor** klikněte na **Nový**a potom klikněte na **projekt**.

3.  Z **Nainstalované šablony**v části **Visual Basic**, klikněte na **Webová aplikace ASP.NET**. Zadejte název projektu **ProductsPortal**. Klikněte na **OK**.

    ![][15]

4.  V seznamu **Vyberte šablonu** klikněte na **MVC**. 

6.  Zaškrtněte políčko u **hostitele v cloudu**.

    ![][16]

5. Klikněte na tlačítko **Změnit ověřování** . V dialogovém okně **Změnit ověřování** klikněte na položku **Bez ověření**a klikněte na **OK**. Pro účely tohoto návodu nasazujete aplikace, která není potřeba přihlašovací jméno uživatele.

    ![][18]

6.  V části **Microsoft Azure** dialogového okna **Nový projekt ASP.NET** zkontrolujte, že je zaškrtnuto **hostovat v cloudu** a že v rozevíracím seznamu je vybrán **Aplikaci služby** .

    ![][19]

7. Klikněte na **OK**. 

8. Teď je třeba nakonfigurovat Azure zdroje pro nové webové aplikace. Proveďte všechny kroky v části [Konfigurace Azure zdroje pro nové webové aplikace](../app-service-web/web-sites-dotnet-get-started.md#configure-azure-resources-for-a-new-web-app). Potom vraťte se do tohoto kurzu a přejděte k dalšímu kroku.

5.  V Průzkumníku **modely** klikněte pravým tlačítkem myši a klikněte na tlačítko **Přidat**a potom klikněte na **Onenotové**. Do pole **název** zadejte název **Product.cs**. Klikněte na **Přidat**.

    ![][17]

### <a name="modify-the-web-application"></a>Úprava webové aplikace

1.  V souboru Product.cs ve Visual Studiu nahraďte stávající definici názvů následující kód.

    ```
    // Declare properties for the products inventory.
    namespace ProductsWeb.Models
    {
        public class Product
        {
            public string Id { get; set; }
            public string Name { get; set; }
            public string Quantity { get; set; }
        }
    }
    ```

2.  V Průzkumníku rozbalte složku **řadiče** a potom poklikejte na soubor **HomeController.cs** otevřete ve Visual Studiu.

3. V **HomeController.cs**nahraďte stávající definici názvů následující kód.

    ```
    namespace ProductsWeb.Controllers
    {
        using System.Collections.Generic;
        using System.Web.Mvc;
        using Models;
    
        public class HomeController : Controller
        {
            // Return a view of the products inventory.
            public ActionResult Index(string Identifier, string ProductName)
            {
                var products = new List<Product>
                    {new Product {Id = Identifier, Name = ProductName}};
                return View(products);
            }
         }
    }
    ```

3.  V Průzkumníku rozbalte složku Views\Shared a potom poklikejte na **_Layout.cshtml** a otevřete ji v editoru Visual Studio.

5.  Změňte všechny výskyty **Moje aplikace ASP.NET** odpovídaly **produktům LITWARE společnosti**.

6. Odebrání odkazů **pro domácnosti**, **o**nebo **kontakt** . V následujícím příkladu odstraňte zvýrazněný kód.

    ![][41]

7.  V Průzkumníku rozbalte složku Views\Home a potom poklikejte na **Index.cshtml** a otevřete ji v editoru Visual Studio.
    Nahradíte celý obsah souboru následující kód.

    ```
    @model IEnumerable<ProductsWeb.Models.Product>
    
    @{
            ViewBag.Title = "Index";
    }
    
    <h2>Prod Inventory</h2>
    
    <table>
            <tr>
                <th>
                    @Html.DisplayNameFor(model => model.Name)
                </th>
                  <th></th>
                <th>
                    @Html.DisplayNameFor(model => model.Quantity)
                </th>
            </tr>
    
    @foreach (var item in Model) {
            <tr>
                <td>
                    @Html.DisplayFor(modelItem => item.Name)
                </td>
                <td>
                    @Html.DisplayFor(modelItem => item.Quantity)
                </td>
            </tr>
    }
    
    </table>
    ```

9.  Pro ověření správnosti pracovní zatím, můžete stisknout **Kombinaci kláves Ctrl + Shift + B** k vytvoření projektu.


### <a name="run-the-app-locally"></a>Spusťte aplikaci místně

Spusťte aplikaci můžete ověřit, že funguje.

1.  Zajistěte, aby byl **ProductsPortal** aktivního projektu. Klikněte pravým tlačítkem myši na název projektu v Průzkumníku řešení a vyberte **Nastavit jako spuštění aplikace Project**.
2.  Ve Visual Studiu stiskněte klávesu F5.
3.  Aplikace by se měly, spuštěné v prohlížeči.

    ![][21]

## <a name="put-the-pieces-together"></a>Připravili kusů

Dalším krokem je připojit místního serveru produkty s aplikací ASP.NET.

1.  Pokud ještě není otevřena, ve Visual Studiu znovu otevřít **ProductsPortal** projektu, který jste vytvořili v části [Vytvoření aplikace ASP.NET](#create-an-aspnet-application) .

2.  Podobné krok v části "Vytvoření na místní Server" balíček NuGet dodejte odkazů projektu. V Průzkumníku **ProductsPortal** projektu klikněte pravým tlačítkem myši a potom klikněte na **Spravovat balíčků NuGet**.

3.  Vyhledejte "Služby Bus" a vyberte položku **Bus služby Microsoft Azure** . Pak dokončete instalaci a zavřete dialogové okno.

4.  V Průzkumníku **ProductsPortal** projektu klikněte pravým tlačítkem myši a potom klikněte na tlačítko **Přidat** **Existující položky**.

5.  Přejděte k souboru **ProductsContract.cs** z konzoly projektu **ProductsServer** . Kliknutím zvýrazněte ProductsContract.cs. Klikněte na šipku dolů vedle tlačítka **Přidat**a potom klikněte na **Přidat jako odkaz**.

    ![][24]

6.  Nyní otevřete soubor **HomeController.cs** v editoru Visual Studiu a nahraďte definici názvů následující kód. Ujistěte se, že *yourServiceNamespace* nahraďte názvem obor názvů služby a *yourKey* klíčem přidružení zabezpečení. To vám umožní klienta volat službu místní vrácení výsledek hovor.

    ```
    namespace ProductsWeb.Controllers
    {
        using System.Linq;
        using System.ServiceModel;
        using System.Web.Mvc;
        using Microsoft.ServiceBus;
        using Models;
        using ProductsServer;
    
        public class HomeController : Controller
        {
            // Declare the channel factory.
            static ChannelFactory<IProductsChannel> channelFactory;
    
            static HomeController()
            {
                // Create shared access signature token credentials for authentication.
                channelFactory = new ChannelFactory<IProductsChannel>(new NetTcpRelayBinding(),
                    "sb://yourServiceNamespace.servicebus.windows.net/products");
                channelFactory.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior {
                    TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                        "RootManageSharedAccessKey", "yourKey") });
            }
    
            public ActionResult Index()
            {
                using (IProductsChannel channel = channelFactory.CreateChannel())
                {
                    // Return a view of the products inventory.
                    return this.View(from prod in channel.GetProducts()
                                     select
                                         new Product { Id = prod.Id, Name = prod.Name,
                                             Quantity = prod.Quantity });
                }
            }
        }
    }
    ```

7.  V Průzkumníku klikněte pravým tlačítkem myši na **ProductsPortal** řešení (Nezapomeňte klikněte pravým tlačítkem myši na řešení, ne project). Klikněte na tlačítko **Přidat**a potom klikněte na **Existující projekt**.

8.  Přejděte na projektu **ProductsServer** a pak poklikáním soubor řešení **ProductsServer.csproj** ho přidejte.

9.  Pokud chcete zobrazit data na **ProductsPortal**musí běžet **ProductsServer** . V Průzkumníku řešení **ProductsPortal** pravým tlačítkem klikněte na **Vlastnosti**. Zobrazí se dialogové okno **Vlastnosti stránky** .

10. Na levé straně klikněte na **Projekt při spuštění**. Na pravé straně klikněte na **více projektech spuštění**. Zajistit, aby **ProductsServer** a **ProductsPortal** zobrazily, v tomto pořadí s **Start** sadu jako akce pro obě.

      ![][25]

11. V dialogovém okně **Vlastnosti** klikněte na **Projekt závislostí** na levé straně.

12. V seznamu **projektů** klikněte na **ProductsServer**. Zajistěte, aby byl **ProductsPortal** **není** vybraná.

14. V seznamu **projektů** klikněte na **ProductsPortal**. Ujistěte se, že je zaškrtnuto **ProductsServer** . 

    ![][26]

15. Klikněte na **OK** v dialogovém okně **Vlastnosti stránky** .

## <a name="run-the-project-locally"></a>Spuštění projektu místně

Testování aplikace místně ve Visual Studiu stisknutím klávesy **F5**. Místního serveru (**ProductsServer**) by měly začít první a potom aplikaci **ProductsPortal** by měly začít v okně prohlížeče. Tentokrát, uvidíte, že skladových zásob produktu jsou uvedeny data načtená z místní systém služby produktu.

![][10]

Na stránce **ProductsPortal** stiskněte **aktualizace** . Pokaždé, když aktualizujete stránku, uvidíte aplikaci serveru zobrazit zprávu po `GetProducts()` z **ProductsServer** je místo toho možnost.

Zavřete obě aplikace teprve potom přejděte k dalšímu kroku.

## <a name="deploy-the-productsportal-project-to-an-azure-web-app"></a>Nasazení projektu ProductsPortal Azure webovou aplikaci

Dalším krokem je převést **ProductsPortal** frontend Azure webovou aplikaci. Nejdřív nasazení projektu **ProductsPortal** všech kroků v části [nasadit web projektu Azure web appu](../app-service-web/web-sites-dotnet-get-started.md#deploy-the-web-project-to-the-azure-web-app). Po dokončení nasazení vrátit zpět do této příručky a přejděte k dalšímu kroku.

> [AZURE.NOTE] Může se objevit chybová zpráva v okně prohlížeče projektu webové **ProductsPortal** se automaticky spustí po nasazení. Očekává se a dochází, pokud ještě není spuštěna aplikace **ProductsServer** .

Zkopírujte adresu URL nasazené webové aplikace, jako byste potřebovali adresu URL v dalším kroku. Tato adresa URL lze získat také z okna aktivitu aplikace služby Azure aplikace Visual Studio:

![][9] 

### <a name="set-productsportal-as-web-app"></a>Nastavení ProductsPortal jako web app

Před spuštěním aplikace v cloudu, musíte se ujistit, že je vyvolaná **ProductsPortal** aplikace Visual Studio jako web app.

1. Ve Visual Studiu klikněte pravým tlačítkem **ProjectsPortal** projektu a potom klikněte na **Vlastnosti**.

3. V levém sloupci klikněte na **webu**.

5. V části **Spustit akci** klikněte na tlačítko **Start adresy URL** a do textového pole zadejte adresu URL pro dříve nasazeném webovou aplikaci; například `http://productsportal1234567890.azurewebsites.net/`.

    ![][27]

6. V nabídce **soubor** ve Visual Studiu klikněte na **Uložit vše**.

7. V nabídce vytvořit ve Visual Studiu klikněte na **Znovu sestavit řešení**.

## <a name="run-the-application"></a>Spusťte aplikaci

2.  Stisknutím klávesy F5 k vytvoření a spuštění aplikace. Místního serveru (aplikace konzoly **ProductsServer** ) by měly začít první a potom aplikaci **ProductsPortal** by měly začít v okně prohlížeče, jak je vidět na následující obrazovce snímek. Oznámení znovu skladových zásob produktu seznamy data načtená z výrobků služby pro místní systém a tato data zobrazí ve web appu. Zkontrolujte adresu URL, abyste měli jistotu, že **ProductsPortal** běží v cloudu, jako Azure web app. 

    ![][1]

    > [AZURE.IMPORTANT] Aplikace konzoly **ProductsServer** musí být spuštěna a mohli sloužit data, která chcete aplikaci **ProductsPortal** . Pokud prohlížeč zobrazí chyba, počkejte další chvíli pro **ProductsServer** načíst a zobrazit tato zpráva. Stiskněte **aktualizace** v prohlížeči.

    ![][37]

3. Zpět v prohlížeči klepněte na stránce **ProductsPortal** **aktualizace** . Pokaždé, když aktualizujete stránku, uvidíte aplikaci serveru zobrazit zprávu po `GetProducts()` z **ProductsServer** je místo toho možnost.

    ![][38]

## <a name="next-steps"></a>Další kroky  

Další informace o Bus služby, naleznete v následujících zdrojích:  

* [Bus služby Azure][sbwacom]  
* [Jak používat službu Bus fronty][sbwacomqhowto]  


  [0]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hybrid.png
  [1]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
  [Získání nástroje a SDK]: http://go.microsoft.com/fwlink/?LinkId=271920
  [NuGet]: http://nuget.org
  
  [11]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-con-1.png
  [13]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-13.png
  [15]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-2.png
  [16]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-4.png
  [17]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-7.png
  [18]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-5.png
  [19]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-6.png
  [9]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-9.png
  [10]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App3.png

  [21]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App1.png
  [24]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-12.png
  [25]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-13.png
  [26]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-14.png
  [27]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-8.png
  
  [36]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
  [37]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service1.png
  [38]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service2.png
  [41]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-40.png
  [43]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-hybrid-43.png


  [sbwacom]: /documentation/services/service-bus/  
  [sbwacomqhowto]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md

