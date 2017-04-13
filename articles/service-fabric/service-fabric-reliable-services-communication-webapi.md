<properties
   pageTitle="Komunikace s ASP.NET rozhraní API webových služeb | Microsoft Azure"
   description="Zjistěte, jak implementovat komunikace služby podrobné pomocí rozhraní API webových ASP.NET OWIN vlastní hostování v spolehlivé rozhraní API služeb."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="get-started-service-fabric-web-api-services-with-owin-self-hosting"></a>Začínáme: rozhraní API webových služeb struktury služeb se OWIN vlastní hostování

Azure struktury služby umístí power rukou při při rozhodování způsobu služeb komunikovat s uživateli a navzájem. Tento kurz se zaměřuje na provádění komunikace služby pomocí rozhraní API webových ASP.NET otevřít webového rozhraní pro .NET (OWIN) vlastní hostování v služby struktury spolehlivé rozhraní API služeb. Budeme se delve velký do připojitelné komunikace spolehlivé služby rozhraní API. Budeme také používat rozhraní API webových v Podrobný příklad zobrazíte, jak nastavit vlastní komunikace posluchače.


## <a name="introduction-to-web-api-in-service-fabric"></a>Úvod do webového rozhraní API služeb struktury

ASP.NET webového rozhraní API je Oblíbené a výkonných rámec pro vytváření rozhraní API HTTP nad .NET Framework. Pokud ještě nejste zkušenosti s rámcem, přečtěte si článek [Začínáme s ASP.NET webového rozhraní API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) Další informace.

Webového rozhraní API služeb struktury je se stejným ASP.NET Web rozhraním API znáte a používat. Rozdíl je, jak můžete *hostitele* rozhraní API webových aplikací. Nebudete používat internetové informační služby (IIS). Chcete-li lépe porozumět rozdílu, Pojďme ho rozdělit na dvě části:

 1. Rozhraní API webových aplikací (včetně řadiče modelů)
 2. Host (webový server, obvykle IIS)

Rozhraní API webových aplikací, samotné nezmění. Je už liší od rozhraní API webových aplikací, které možná jste napsali v minulosti a by měla jednoduše přesunout přes většina kód aplikace. Ale pokud jste jste hostující na serveru IIS, kde může být trochu jinak, než co jste byli zvyklí hostitele aplikace. Před jsme dostali do části hostingu, Začněme s něco víc známé: rozhraní API webových aplikací.


## <a name="create-the-application"></a>Vytvoření aplikace

Začněte tím, že vytvoříte novou aplikaci služby struktury s do jednoho příslušnosti služby Visual Studio 2015:

![Vytvořit novou aplikaci služby struktury](media/service-fabric-reliable-services-communication-webapi/webapi-newproject.png)

Šablona aplikace Visual Studio příslušnosti služby pomocí rozhraní API webových je k dispozici. V tomto kurzu budeme budete sestavení rozhraní API webových projektu od začátku, jehož výsledkem co byste dostali, pokud jste vybrali tuto šablonu.

Vyberte prázdný projekt příslušnosti služby se dozvíte, jak vytvářet rozhraní API webových projektu od začátku nebo použití šablony rozhraní API webových služeb příslušnosti a postupujte podle.  

![Vytvoření jednoho příslušnosti služby](media/service-fabric-reliable-services-communication-webapi/webapi-newproject2.png)

Cílem prvního kroku je zobrazíte v některé NuGet balíčky pro rozhraní API webových. Balíček, který chcete použít je Microsoft.AspNet.WebApi.OwinSelfHost. Balíček obsahuje všechny potřebné balíčků rozhraní API webových a balíčků *Host (hostitel)* . To bude důležité později.

![Vytvoření rozhraní API webových pomocí Správce balíčků NuGet](media/service-fabric-reliable-services-communication-webapi/webapi-nuget.png)

Po instalaci balíčků můžete začít vytvoření základní struktura rozhraní API webových projektu. Pokud jste vypotřebovali rozhraní API webových, by měl vypadat povědomý struktury projektu. Začněte tím, že přidáte `Controllers` adresář a řadiči jednoduché hodnoty:

**ValuesController.cs**

```csharp
using System.Collections.Generic;
using System.Web.Http;
    
namespace WebService.Controllers
{
    public class ValuesController : ApiController
    {
        // GET api/values 
        public IEnumerable<string> Get()
        {
            return new string[] { "value1", "value2" };
        }

        // GET api/values/5 
        public string Get(int id)
        {
            return "value";
        }

        // POST api/values 
        public void Post([FromBody]string value)
        {
        }

        // PUT api/values/5 
        public void Put(int id, [FromBody]string value)
        {
        }

        // DELETE api/values/5 
        public void Delete(int id)
        {
        }
    }
}

```

Dále přidejte třída při spuštění v kořenovém projektu k registraci směrování, formatters a další nastavení konfigurace. Toto je také kde rozhraní API webových připojí na *Host (hostitel)*, který bude být znovu obrácena pozornost později. 

**Startup.cs**

```csharp
using System.Web.Http;
using Owin;

namespace WebService
{
    public static class Startup
    {
        public static void ConfigureApp(IAppBuilder appBuilder)
        {
            // Configure Web API for self-host. 
            HttpConfiguration config = new HttpConfiguration();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );

            appBuilder.UseWebApi(config);
        }
    }
}
```

To je pro část aplikace. V tomto okamžiku jsme jste nastavili jenom základní rozložení rozhraní API webových projektu. Zatím nesmí vypadat podobně jinak z rozhraní API webových projekty, které možná jste napsali v minulosti nebo šablona základního rozhraní API webových. Obchodní logiky jsou uvedeny v řadiče a modely jako obvykle.

Teď co můžeme udělat o hostování tak, aby jsme ve skutečnosti ho?

## <a name="service-hosting"></a>Hostování služby

Služba struktury služby spuštěn ve výrobním *procesu hostitelské služby*, spustitelný soubor spuštěná služba kódu. Při psaní do služby pomocí spolehlivé rozhraní API služeb projektu služby jenom kompilaci na spustitelný soubor, který se registruje typ služby a spustí váš kód. To platí ve většině případů při psaní do služby na struktury služby v .NET. Když otevřete Program.cs v projektu příslušnosti služby, měli byste vidět:

```csharp
using System;
using System.Diagnostics;
using System.Threading;
using Microsoft.ServiceFabric.Services.Runtime;

internal static class Program
{
    private static void Main()
    {
        try
        {
            ServiceRuntime.RegisterServiceAsync("WebServiceType",
                context => new WebService(context)).GetAwaiter().GetResult();

            ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(WebService).Name);

            // Prevents this host process from terminating so services keeps running. 
            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ServiceEventSource.Current.ServiceHostInitializationFailed(e.ToString());
            throw;
        }
    }
}

```

Pokud, která vypadá nápadně vstupní bod pro aplikace konzoly, je proto, že je.

Další informace o procesu hostitelské služby a služby registrace jsou nad rámec tohoto článku. Pořád důležitá pro tuto tento *že kód služby běží ve vlastním procesu*.

## <a name="self-host-web-api-with-an-owin-host"></a>Automatické hostovat rozhraní API webových od hostitele osobních OWIN

Vzhledem k tomu, že kódu rozhraní API webových aplikací je hostovaný ve vlastním procesu, jak můžete připojit ho nahoru na webový server? Zadejte [OWIN](http://owin.org/). OWIN je jednoduše smlouva mezi .NET webových aplikací a webových serverů. Obvykle použijete ASP.NET (až MVC 5) webové aplikace je pevně svázáno ke službě IIS pomocí System.Web. Rozhraní API webových však používá OWIN, takže psát webovou aplikaci, která je oddělené z webového serveru, který je hostitelem ho. Vzhledem k tomu můžete *vlastním hostované* OWIN webového serveru, který je možné spustit vlastní proces. To odpovídá dokonale hostingu modelu služby struktury, který právě popsány.

V tomto článku použijeme Katana jako hostitel OWIN pro rozhraní API webových aplikací. Katana je implementace hostitele OWIN otevřít zdroj založený na [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx) a [Rozhraní API serveru HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)systému Windows.

> [AZURE.NOTE] Další informace o Katana, přejděte na [Web Katana](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana). Rychlý přehled toho, jak používat Katana vlastním hostovat rozhraní API webových najdete v článku [Použití OWIN Self-Host ASP.NET webového rozhraní API 2](http://www.asp.net/web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api).


## <a name="set-up-the-web-server"></a>Nastavení serveru

Spolehlivé rozhraní API služeb poskytuje komunikace vstupní bod kde zapojte štosech komunikace, které umožňují uživatelům a klientů připojit ke službě:

```csharp

protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    ...
}

```

Webový server (a dalších zásobníku komunikace, používaného v budoucnu, například WebSockets) používejte rozhraní ICommunicationListener správně integrovat systému. Důvody stane lépe poznat v následujících krocích.

Nejprve vytvořte třídy s názvem OwinCommunicationListener implementující ICommunicationListener:

**OwinCommunicationListener.cs**

```csharp
using Microsoft.Owin.Hosting;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Owin;
using System;
using System.Fabric;
using System.Globalization;
using System.Threading;
using System.Threading.Tasks;

namespace WebService
{
    internal class OwinCommunicationListener : ICommunicationListener
    {
        public void Abort()
        {
        }

        public Task CloseAsync(CancellationToken cancellationToken)
        {
        }

        public Task<string> OpenAsync(CancellationToken cancellationToken)
        {
        }
    }
}
```

Rozhraní ICommunicationListener nabízí tři způsoby ke správě komunikace posluchače službě:

 - *OpenAsync*. Spusťte přijímá žádosti.
 - *CloseAsync*. Zastavit naslouchají požadavky, dokončení všech letu požadavků a neukončí.
 - *Přerušení*. Zrušit vše a okamžitě ukončit.

Nejdřív přidáte členy soukromé třídy pro činnosti, které bude nutné posluchače fungovat. Tyto bude inicializován pomocí konstruktoru a použít později, když nastavíte naslouchají adresy URL.

```csharp
internal class OwinCommunicationListener : ICommunicationListener
{
    private readonly ServiceEventSource eventSource;
    private readonly Action<IAppBuilder> startup;
    private readonly ServiceContext serviceContext;
    private readonly string endpointName;
    private readonly string appRoot;

    private IDisposable webApp;
    private string publishAddress;
    private string listeningAddress;

    public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName)
        : this(startup, serviceContext, eventSource, endpointName, null)
    {
    }

    public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName, string appRoot)
    {
        if (startup == null)
        {
            throw new ArgumentNullException(nameof(startup));
        }

        if (serviceContext == null)
        {
            throw new ArgumentNullException(nameof(serviceContext));
        }

        if (endpointName == null)
        {
            throw new ArgumentNullException(nameof(endpointName));
        }

        if (eventSource == null)
        {
            throw new ArgumentNullException(nameof(eventSource));
        }

        this.startup = startup;
        this.serviceContext = serviceContext;
        this.endpointName = endpointName;
        this.eventSource = eventSource;
        this.appRoot = appRoot;
    }
   

    ...

```

## <a name="implement-openasync"></a>Implementace OpenAsync

Pokud chcete nastavit webového serveru, musíte dvěma datovými informace:

 - *Předpona cestu adresy URL*. To je sice volitelný, že je vhodné pro vás to nyní nastavit tak, aby bylo možné bezpečně hostovat víc webovým službám v aplikaci.
 - *Port*.

Než se zobrazí port pro webový server, je důležité porozumět tomu, že služba struktury poskytuje vrstvu aplikace, která funguje jako vyrovnávací paměť mezi aplikace a operačního systému, která poběží na. Jako takové služby struktury umožňuje konfigurace *koncové body* pro své služby. Služba struktury zajišťuje, že koncové body k dispozici službě používat. Tímto způsobem, nemusíte nastavovat sami v podkladovém prostředí OS. Můžete snadno hostujete aplikace služby struktury v různých prostředích aniž byste museli proveďte požadované změny aplikaci. (Třeba budete moct hostovat stejné aplikaci v Azure nebo vlastní datacentra.)

Konfigurujte koncový bod HTTP PackageRoot\ServiceManifest.xml:

```xml

<Resources>
    <Endpoints>
        <Endpoint Name="ServiceEndpoint" Type="Input" Protocol="http" Port="8281" />
    </Endpoints>
</Resources>

```

Tento krok je důležité, protože proces hostitele služby kompatibilní s omezeným přístupem přihlašovacích údajů (služba síti ve Windows). To znamená, že vaše služby nebudou mít přístup k nastavení koncový bod HTTP na vlastní. Pomocí konfigurace koncového bodu služby struktury zná nastavení seznamu řízení správné přístupu (ACL) pro službu bude naslouchají relacím na adresy URL. Služba struktury také místo standardní konfigurace koncové body.


Po návratu do OwinCommunicationListener.cs můžete začít provádění OpenAsync. Toto je, kde začít webový server. Nejprve získat informace o koncového bodu a vytvořit adresu URL, kterou službu bude naslouchají relacím na. Adresa URL bude lišit podle toho, zda je použit posluchače příslušnosti služby nebo stavová služba. Stavové informace o službě posluchače potřebuje k vytvoření jedinečných adresu pro každého otevřené stavová služba, která sleduje na. Příslušnosti státní může být adresu mnohem jednodušší. 

```csharp
public Task<string> OpenAsync(CancellationToken cancellationToken)
{
    var serviceEndpoint = this.serviceContext.CodePackageActivationContext.GetEndpoint(this.endpointName);
    var protocol = serviceEndpoint.Protocol;
    int port = serviceEndpoint.Port;

    if (this.serviceContext is StatefulServiceContext)
    {
        StatefulServiceContext statefulServiceContext = this.serviceContext as StatefulServiceContext;

        this.listeningAddress = string.Format(
            CultureInfo.InvariantCulture,
            "{0}://+:{1}/{2}{3}/{4}/{5}",
            protocol,
            port,
            string.IsNullOrWhiteSpace(this.appRoot)
                ? string.Empty
                : this.appRoot.TrimEnd('/') + '/',
            statefulServiceContext.PartitionId,
            statefulServiceContext.ReplicaId,
            Guid.NewGuid());
    }
    else if (this.serviceContext is StatelessServiceContext)
    {
        this.listeningAddress = string.Format(
            CultureInfo.InvariantCulture,
            "{0}://+:{1}/{2}",
            protocol,
            port,
            string.IsNullOrWhiteSpace(this.appRoot)
                ? string.Empty
                : this.appRoot.TrimEnd('/') + '/');
    }
    else
    {
        throw new InvalidOperationException();
    }
    
    ...

```

Všimněte si, že "http://+" využívá tady. Toto je abyste měli jistotu, že je na všechny dostupné adresy, včetně localhost, plně kvalifikovaný název domény a IP počítače přijímá webový server.

Implementace OpenAsync je jednou z nejdůležitějších důvodů, proč je jako ICommunicationListener, nikoli pouze s otevřít přímo z implementovaná webový server (nebo libovolný komunikace zásobníku) `RunAsync()` ve službě. Hodnota vrácená OpenAsync je adresu webového serveru listening na. Když tuto adresu vrácena systému, zaregistruje adresy se službou. Služba struktury poskytuje rozhraní API, které umožňuje klienty a jiných služeb, pak po nich žádáte tuto adresu podle názvu služby. Důvodem je důležité, adresy služby není statické. Služby přesouvání obrázku pro účely vyrovnávání a dostupnosti zdrojů. Toto je mechanismus, který umožňuje klientům naslouchají adresy služby.

Si uvědomit OpenAsync spustí webového serveru a vrátí adresu listening na. Poznámka: aby sleduje na "http://+", ale před OpenAsync vrátí adresu, "a" se nahradí IP nebo plně kvalifikovaný název domény je na uzel. Adresa, který vám vrátil tento způsob je, co máte zaregistrované v rámci systému. Je taky co klienty a jiných služeb, najdete v článku když žádat adresu do služby. Pro klienty správně připojení k ní potřebují adresu skutečné IP nebo plně kvalifikovaný název domény.

```csharp
    ...

    this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

    try
    {
        this.eventSource.Message("Starting web server on " + this.listeningAddress);

        this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));

        this.eventSource.Message("Listening on " + this.publishAddress);

        return Task.FromResult(this.publishAddress);
    }
    catch (Exception ex)
    {
        this.eventSource.Message("Web server failed to open endpoint {0}. {1}", this.endpointName, ex.ToString());

        this.StopWebServer();

        throw;
    }
}

```

Poznámka: Toto odkazuje třídu pro spuštění předaný v OwinCommunicationListener v konstruktoru. Tato instance při spuštění lze webovým serverem bootstrap rozhraní API webových aplikací.

`ServiceEventSource.Current.Message()` Řádku se zobrazí v okně diagnostických událostí později při spuštění aplikace potvrďte, že byla úspěšně spuštěna webový server.

## <a name="implement-closeasync-and-abort"></a>Implementace CloseAsync a přerušení

Nakonec implementujte CloseAsync a přerušení zastavit webový server. Odstraňování úchyt serveru, který byl vytvořen během OpenAsync můžete zastavit webový server.

```csharp
public Task CloseAsync(CancellationToken cancellationToken)
{
    this.eventSource.Message("Closing web server on endpoint {0}", this.endpointName);
            
    this.StopWebServer();

    return Task.FromResult(true);
}

public void Abort()
{
    this.eventSource.Message("Aborting web server on endpoint {0}", this.endpointName);
    
    this.StopWebServer();
}

private void StopWebServer()
{
    if (this.webApp != null)
    {
        try
        {
            this.webApp.Dispose();
        }
        catch (ObjectDisposedException)
        {
            // no-op
        }
    }
}
```

V tomto příkladu implementace CloseAsync a přerušení jednoduše ukončíte webový server. Může se rozhodnout pro vypnutí více řádně koordinovaný webového serveru, v CloseAsync. Vypnutí může například počkejte letu požadavky na dokončen před vrácení.

## <a name="start-the-web-server"></a>Spusťte webový server

Teď budete chtít vytvořit a vraťte se instance OwinCommunicationListener spusťte webový server. Zpátky ve třídě služby (WebService.cs) přepsat `CreateServiceInstanceListeners()` metodu:

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    var endpoints = Context.CodePackageActivationContext.GetEndpoints()
                           .Where(endpoint => endpoint.Protocol == EndpointProtocol.Http || endpoint.Protocol == EndpointProtocol.Https)
                           .Select(endpoint => endpoint.Name);

    return endpoints.Select(endpoint => new ServiceInstanceListener(
        serviceContext => new OwinCommunicationListener(Startup.ConfigureApp, serviceContext, ServiceEventSource.Current, endpoint), endpoint));
}
```

Je to, kde rozhraní API webových *aplikací* a OWIN *hostitele* nakonec splňují. Host (OwinCommunicationListener) je uveden instanci *aplikace* (rozhraní API webových) prostřednictvím spuštění předmětu. Služba struktury pak spravuje svého životního cyklu. Tento stejné vzorek lze postupovat obvykle se všechny zásobníku komunikace.

## <a name="put-it-all-together"></a>Umístění vůbec

V tomto příkladu nemusíte nic dělat v `RunAsync()` metody, abyste tento přepsání můžete jednoduše odebrat.

Implementaci konečný služby by měl být velmi jednoduché. Jenom je potřeba vytvořit posluchače komunikace:

```csharp
using System;
using System.Collections.Generic;
using System.Fabric;
using System.Fabric.Description;
using System.Linq;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Microsoft.ServiceFabric.Services.Runtime;

namespace WebService
{
    internal sealed class WebService : StatelessService
    {
        public WebService(StatelessServiceContext context)
            : base(context)
        { }

        protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
        {
            var endpoints = Context.CodePackageActivationContext.GetEndpoints()
                                   .Where(endpoint => endpoint.Protocol == EndpointProtocol.Http || endpoint.Protocol == EndpointProtocol.Https)
                                   .Select(endpoint => endpoint.Name);

            return endpoints.Select(endpoint => new ServiceInstanceListener(
                serviceContext => new OwinCommunicationListener(Startup.ConfigureApp, serviceContext, ServiceEventSource.Current, endpoint), endpoint));
        }
    }
}
```

Kompletní `OwinCommunicationListener` třídy:

```csharp
using System;
using System.Diagnostics;
using System.Fabric;
using System.Globalization;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.Owin.Hosting;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Owin;

namespace WebService
{
    internal class OwinCommunicationListener : ICommunicationListener
    {
        private readonly ServiceEventSource eventSource;
        private readonly Action<IAppBuilder> startup;
        private readonly ServiceContext serviceContext;
        private readonly string endpointName;
        private readonly string appRoot;

        private IDisposable webApp;
        private string publishAddress;
        private string listeningAddress;

        public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName)
            : this(startup, serviceContext, eventSource, endpointName, null)
        {
        }

        public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName, string appRoot)
        {
            if (startup == null)
            {
                throw new ArgumentNullException(nameof(startup));
            }

            if (serviceContext == null)
            {
                throw new ArgumentNullException(nameof(serviceContext));
            }

            if (endpointName == null)
            {
                throw new ArgumentNullException(nameof(endpointName));
            }

            if (eventSource == null)
            {
                throw new ArgumentNullException(nameof(eventSource));
            }

            this.startup = startup;
            this.serviceContext = serviceContext;
            this.endpointName = endpointName;
            this.eventSource = eventSource;
            this.appRoot = appRoot;
        }

        public Task<string> OpenAsync(CancellationToken cancellationToken)
        {
            var serviceEndpoint = this.serviceContext.CodePackageActivationContext.GetEndpoint(this.endpointName);
            var protocol = serviceEndpoint.Protocol;
            int port = serviceEndpoint.Port;

            if (this.serviceContext is StatefulServiceContext)
            {
                StatefulServiceContext statefulServiceContext = this.serviceContext as StatefulServiceContext;

                this.listeningAddress = string.Format(
                    CultureInfo.InvariantCulture,
                    "{0}://+:{1}/{2}{3}/{4}/{5}",
                    protocol,
                    port,
                    string.IsNullOrWhiteSpace(this.appRoot)
                        ? string.Empty
                        : this.appRoot.TrimEnd('/') + '/',
                    statefulServiceContext.PartitionId,
                    statefulServiceContext.ReplicaId,
                    Guid.NewGuid());
            }
            else if (this.serviceContext is StatelessServiceContext)
            {
                this.listeningAddress = string.Format(
                    CultureInfo.InvariantCulture,
                    "{0}://+:{1}/{2}",
                    protocol,
                    port,
                    string.IsNullOrWhiteSpace(this.appRoot)
                        ? string.Empty
                        : this.appRoot.TrimEnd('/') + '/');
            }
            else
            {
                throw new InvalidOperationException();
            }

            this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

            try
            {
                this.eventSource.Message("Starting web server on " + this.listeningAddress);

                this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));

                this.eventSource.Message("Listening on " + this.publishAddress);

                return Task.FromResult(this.publishAddress);
            }
            catch (Exception ex)
            {
                this.eventSource.Message("Web server failed to open endpoint {0}. {1}", this.endpointName, ex.ToString());

                this.StopWebServer();

                throw;
            }
        }

        public Task CloseAsync(CancellationToken cancellationToken)
        {
            this.eventSource.Message("Closing web server on endpoint {0}", this.endpointName);

            this.StopWebServer();

            return Task.FromResult(true);
        }

        public void Abort()
        {
            this.eventSource.Message("Aborting web server on endpoint {0}", this.endpointName);

            this.StopWebServer();
        }

        private void StopWebServer()
        {
            if (this.webApp != null)
            {
                try
                {
                    this.webApp.Dispose();
                }
                catch (ObjectDisposedException)
                {
                    // no-op
                }
            }
        }
    }
}
```

Teď, když umístíte všechny části na místě projektu by měl vypadat aplikace typické rozhraní API webových s spolehlivé rozhraní API služeb vstupní body a OWIN host:


![Webového rozhraní API s spolehlivé rozhraní API služeb vstupní body a OWIN Host (hostitel)](media/service-fabric-reliable-services-communication-webapi/webapi-projectstructure.png)

## <a name="run-and-connect-through-a-web-browser"></a>Spuštění a připojte se pomocí webového prohlížeče

Pokud jste nevytvořili, [nastavení vývojové prostředí](service-fabric-get-started.md).


Teď můžete vytvořte a nasaďte služby. Stisknutím klávesy **F5** ve Visual Studiu sestavovat a nasazovat aplikace. V okně diagnostických událostí byste měli vidět zprávu, která označuje, že na webový server otevřít na http://localhost:8281 /.


![Okno Visual Studio diagnostických událostí](media/service-fabric-reliable-services-communication-webapi/webapi-diagnostics.png)

> [AZURE.NOTE] Pokud číslo portu již otevřen jiným procesem ve vašem počítači, může se zobrazit k chybě. Tento údaj označuje, že posluchače nelze otevřít. Pokud to je případ, zkuste použít jiný port pro konfiguraci koncového bodu v ServiceManifest.xml.


Jakmile se službou otevřete prohlížeč a přejděte na [http://localhost:8281/rozhraní api/hodnot](http://localhost:8281/api/values) a ty pak ji otestujte.

## <a name="scale-it-out"></a>Změna velikosti ji

Rozšiřování webových aplikací příslušnosti web apps obvykle znamená, že přidání více počítačů a otáčející se přes web apps na ně. Modul služby struktury průběhu lze provést můžete kdykoli nové uzly se přidají do clusteru. Při vytváření instancí příslušnosti služby můžete určit počet výskytů, který chcete vytvořit. Služba struktury umístí určeným počtem instance uzlů v clusteru. A zajišťuje, nevytvářet více než jedna instance v jednom uzlu. Můžete taky určit, aby služby struktury vždy na každý uzel vytvořit instance zadáním **-1** pro počet instancí. Zajistíte tak, že kdykoli přidat uzly rozšiřování svůj cluster instanci služby příslušnosti se vytvoří nový uzlech. Tato hodnota je vlastnost instance služby tak, že nastavíte při vytváření instanci služby. Můžete to udělat pomocí prostředí PowerShell:

```powershell

New-ServiceFabricService -ApplicationName "fabric:/WebServiceApplication" -ServiceName "fabric:/WebServiceApplication/WebService" -ServiceTypeName "WebServiceType" -Stateless -PartitionSchemeSingleton -InstanceCount -1

```

Můžete taky udělat toto při definování výchozí službu v projektu příslušnosti služby Visual Studio:

```xml

<DefaultServices>
  <Service Name="WebService">
    <StatelessService ServiceTypeName="WebServiceType" InstanceCount="-1">
      <SingletonPartition />
    </StatelessService>
  </Service>
</DefaultServices>

```

Další informace o tom, jak vytvořit aplikaci a instancí služby najdete v článku [nasazení aplikace](service-fabric-deploy-remove-applications.md).

## <a name="next-steps"></a>Další kroky

[Ladění aplikace služby struktury pomocí aplikace Visual Studio](service-fabric-debugging-your-application.md)
