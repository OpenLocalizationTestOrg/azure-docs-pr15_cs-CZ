<properties
   pageTitle="Spolehlivé Přehled komunikace služby | Microsoft Azure"
   description="Přehled modelu spolehlivé služby komunikace, včetně otevření posluchačů na služby řešení koncové body a komunikaci mezi službami."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor="BharatNarasimman"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="how-to-use-the-reliable-services-communication-apis"></a>Použití rozhraní API komunikaci spolehlivé služby

Azure struktury služby jako platformu je úplně která o komunikace mezi službami. Protokoly a štosech jsou přijatelné z UDP pro HTTP. Je pro vývojáře služby rozhodnout se, jak by měl služby komunikovat. Rozhraní spolehlivé služby poskytuje předdefinované komunikace štosech i rozhraní API, která slouží k vytvoření vlastní komunikační součásti. 

## <a name="set-up-service-communication"></a>Nastavení komunikace služby

Spolehlivé rozhraní API služeb používá jednoduché rozhraní pro komunikace služby. Otevřete koncový bod pro službu, jednoduše Implementujte toto rozhraní:

```csharp

public interface ICommunicationListener
{
    Task<string> OpenAsync(CancellationToken cancellationToken);

    Task CloseAsync(CancellationToken cancellationToken);

    void Abort();
}

```

Implementace posluchače komunikace pak přidáte vrácení v přepsání metody založené na službě předmětu.

Pro příslušnosti služby:

```csharp
class MyStatelessService : StatelessService
{
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        ...
    }
    ...
}
```

Stavová služby:

```csharp
class MyStatefulService : StatefulService
{
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
        ...
    }
    ...
}
```

V obou případech vrátí sadu posluchače. Díky služby poslouchat na více koncové body potenciálně pomocí různých protokoly více posluchače. Například může mít posluchače HTTP a samostatné posluchače WebSocket. Každý posluchače získá název a výsledná kolekce *Název: adresa* dvojice představuje jako objekt JSON klient požaduje naslouchají adresy instanci služby nebo oddíl.

Přepsání příslušnosti služby vrátí kolekci ServiceInstanceListeners. ServiceInstanceListener obsahuje funkci, kterou chcete vytvořit ICommunicationListener a název. Stavová státní vrátí přepsání kolekci ServiceReplicaListeners. Totiž mírně odlišné od jeho příslušnosti protějšku ServiceReplicaListener má možnost otevření ICommunicationListener na vedlejší repliky. Nejen můžete používat více komunikace posluchače ve službě, ale můžete je taky zadat které posluchače přijímaní žádostí o sekundární repliky a které z nich přijímat jen na primární repliky.

Například máte ServiceRemotingListener, který volání RPC jenom na primární repliky a druhý, vlastní posluchače, který má čtení požadavky na vedlejší repliky přes HTTP:

```csharp
protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[]
    {
        new ServiceReplicaListener(context =>
            new MyCustomHttpListener(context),
            "HTTPReadonlyEndpoint",
            true),

        new ServiceReplicaListener(context =>
            this.CreateServiceRemotingListener(context),
            "rpcPrimaryEndpoint",
            false)
    };
}
```

> [AZURE.NOTE] Při vytváření více posluchače pro službu, každý posluchače, **musí** mít jedinečný název.

Nakonec popisují koncové body, které jsou zapotřebí pro službu [služby manifest](service-fabric-application-model.md) části na koncové body.

```xml
<Resources>
    <Endpoints>
      <Endpoint Name="WebServiceEndpoint" Protocol="http" Port="80" />
      <Endpoint Name="OtherServiceEndpoint" Protocol="tcp" Port="8505" />
    <Endpoints>
</Resources>

```

Komunikace posluchače mají přístup k prostředkům koncový bod přidělit z `CodePackageActivationContext` v `ServiceContext`. Posluchače můžete pak začněte přijímá žádosti o při dalším otevření.

```csharp
var codePackageActivationContext = serviceContext.CodePackageActivationContext;
var port = codePackageActivationContext.GetEndpoint("ServiceEndpoint").Port;

```

> [AZURE.NOTE] Koncový bod zdroje jsou společná pro celou službu balíčku a jsou přiřazený tak, že služba struktury při aktivaci balíčku služby. Více repliky služby hostované ve stejné ServiceHost můžete sdílet stejný port. To znamená, že posluchače komunikace má podporovat port sdílení. Doporučené postupy to provést je pro posluchače komunikace provádějte vygeneruje adresu poslech oddílu ID a otevřené/instance.

### <a name="service-address-registration"></a>Registrace adresy služby

Systémová služba s názvem *Služba WINS* běží na clusterů struktury služby. Služba WINS je registrátora služeb a jejich adres, které jednotlivé instance nebo otevřené služby listening na. Když `OpenAsync` metodu `ICommunicationListener` dokončí, jeho vrácená hodnota získá registraci v služba WINS. Tuto hodnotu získá publikovaných v galerii služba WINS je řetězec, jejichž hodnota je něco vůbec. Tento řetězec hodnotu klientů uvidí při jejich po nich žádáte adresu pro službu z služba WINS.

```csharp
public Task<string> OpenAsync(CancellationToken cancellationToken)
{
    EndpointResourceDescription serviceEndpoint = serviceContext.CodePackageActivationContext.GetEndpoint("ServiceEndpoint");
    int port = serviceEndpoint.Port;

    this.listeningAddress = string.Format(
                CultureInfo.InvariantCulture,
                "http://+:{0}/",
                port);
                        
    this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);
            
    this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));
    
    // the string returned here will be published in the Naming Service.
    return Task.FromResult(this.publishAddress);
}
```

Služba struktury poskytuje rozhraní API, která umožňuje klienty a jiných služeb, pak po nich žádáte tuto adresu podle názvu služby. Důvodem je důležité, adresy služby není statické. Služby přesouvání obrázku pro účely vyrovnávání a dostupnosti zdrojů. Toto je mechanismus, který umožňuje klientům naslouchají adresy služby.

> [AZURE.NOTE] Pro dokončení walk-through jak psát `ICommunicationListener`, najdete v článku [rozhraní API webových služeb struktury služeb se OWIN vlastní hostování](service-fabric-reliable-services-communication-webapi.md)

## <a name="communicating-with-a-service"></a>Komunikace se službou
Spolehlivé rozhraní API služeb poskytuje následující knihovny psát klientech komunikaci se službami.

### <a name="service-endpoint-resolution"></a>Překlad koncového bodu služby
Cílem prvního kroku komunikace se službou je vyřešit adresa koncového bodu oddílu nebo instanci služby, kterou chcete mluvit. `ServicePartitionResolver` Nástroj předmětu je základní základní, která vám pomůže zjistit koncového bodu služby za běhu klientů. V služby struktury terminologii proces určování koncového bodu služby označovány jako *Překlad koncového bodu služby*.

Připojení ke službám v rámci clusteru, `ServicePartitionResolver` lze vytvářet pomocí výchozího nastavení. Toto je doporučené použití pro většinu situací:

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();
```

Připojení ke službám v jiném clusteru, `ServicePartitionResolver` lze se sadou koncové body brány obrázku. Všimněte si, že koncové body brány jenom různých koncové body připojit se k stejného obrázku. Příklad:

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```

Můžete taky `ServicePartitionResolver` může být zadán s funkcí pro vytváření `FabricClient` interně používat: 
 
```csharp
public delegate FabricClient CreateFabricClientDelegate();
```

`FabricClient`je objekt, který slouží ke komunikaci s clusteru služby struktury při různých operacích správy na clusteru. To je užitečná, když chcete mít větší kontrolu nad jak `ServicePartitionResolver` pracuje s svůj cluster. `FabricClient`provede mezipaměti interně a je obecně drahé chcete vytvořit, takže je důležité ji znovu použít `FabricClient` instance co nejvíc. 

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver(() => CreateMyFabricClient());
```

Metodu vyřešení potom slouží k načtení adresy služby nebo oddíl služby pro oddíly služby.

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();

ResolvedServicePartition partition =
    await resolver.ResolveAsync(new Uri("fabric:/MyApp/MyService"), new ServicePartitionKey(), cancellationToken);
```

Adresy služby můžete vyřešit snadno `ServicePartitionResolver`, ale víc práce je nutné převedenou adresu lze správně. Klient muset zjistit zda pokus o připojení nebyl úspěšný přechodná chyby a opakovat lze (například služba přesunutý nebo je dočasně nedostupný), nebo trvalé chybě (například služba odstranil nebo požadovaný prostředek už existuje). Instancí služby nebo repliky se můžete pohybovat z uzel uzel kdykoli z několika důvodů. Adresy služby pomůže vyřešit `ServicePartitionResolver` může být zastaralé podle času kód klienta pokouší připojit. V takovém případě znovu klient bude muset znovu přeložit adresu. Poskytování předchozího `ResolvedServicePartition` označuje, že překladač vyžaduje znovu místo jednoduše načíst adres v mezipaměti.

Obvykle kód klienta nemusí fungovat s `ServicePartitionResolver` přímo. Se vytvoří a předána komunikace klienta továrny v spolehlivé rozhraní API služeb. Překladač závodů interně použít ke generování klientského objektu, který slouží ke komunikaci se službami.

### <a name="communication-clients-and-factories"></a>Komunikace klienty a továrny

Knihovna factory komunikace používá typické poruch zpracování opakovat vzorek který usnadňuje opakování připojení k koncové body přeložena služby. Knihovna factory poskytuje opakovat mechanismus při poskytuje rutiny chyby.

`ICommunicationClientFactory`Definuje prováděným factory komunikace klienta, vypočítávající klienti, které můžete mluvit do služby struktury služby základní rozhraní. Provádění CommunicationClientFactory závisí na komunikace zásobníku služby struktury služba používá Pokud chce komunikace klienta. Spolehlivé rozhraní API služeb poskytuje `CommunicationClientFactoryBase<TCommunicationClient>`. To umožňuje základní implementace `ICommunicationClientFactory` rozhraní a provádění úkolů, která jsou společná pro všechny balíčky komunikace. (Pomocí zahrnout tyto úkoly `ServicePartitionResolver` k určení koncový bod služby). Klienti obvykle implementovat abstraktní třídy CommunicationClientFactoryBase pro zpracování použití logických operátorů, která je specifická do zásobníku komunikace.

Klient komunikace jenom obdrží adresu a používá k připojení ke službě. Klient můžete použít jakéhokoliv protokol požaduje.

```csharp
class MyCommunicationClient : ICommunicationClient
{
    public ResolvedServiceEndpoint Endpoint { get; set; }

    public string ListenerName { get; set; }

    public ResolvedServicePartition ResolvedServicePartition { get; set; }
}
```

Factory klienta je primárně odpovědná za vytvoření klientů komunikace. Pro klienty, které nechcete zachovat trvalý připojení, například klientovi HTTP továrny stačí vytvořit a vraťte se klient. Další protokolů, které udržovat trvalý připojení, například některé binární protokoly by měla být ověřena taky tak, že factory a zjistit, zda připojení není nutné znovu vytvořit.  

```csharp
public class MyCommunicationClientFactory : CommunicationClientFactoryBase<MyCommunicationClient>
{
    protected override void AbortClient(MyCommunicationClient client)
    {
    }

    protected override Task<MyCommunicationClient> CreateClientAsync(string endpoint, CancellationToken cancellationToken)
    {
    }

    protected override bool ValidateClient(MyCommunicationClient clientChannel)
    {
    }

    protected override bool ValidateClient(string endpoint, MyCommunicationClient client)
    {
    }
}
```

Obslužná rutina výjimky navíc je příslušné pro zjištění, jakou akci provedeny, pokud dojde k výjimce. Výjimky zařazené do **retriable** a **jiných retriable**. 

 - Výjimky **nejsou retriable** jednoduše získat znovu vyvolá zpět volajícímu. 
 - Výjimky **Retriable** jsou další zařadit do **přechodná** a **nepřechodná**.
  - **Přechodné** výjimky používané jednoduše opakovat lze bez znovu řešení adresa koncového bodu služby. Tyto bude obsahovat přechodná sítě problémů nebo odpovědi Chyba služby než těch, které označuje, že adresa koncového bodu služby neexistuje. 
  - **Bez přechodové** výjimkou jsou ty, které vyžadují adresa koncového bodu služby být znovu přeložena. Jedná se o výjimek, které označují že koncový bod služby nemůže získat přístup na stránku, označující službu přesunul různých uzel. 

`TryHandleException` Provádí rozhodnutí o dané výjimku. Pokud to **není ví,** co rozhodnutí o výjimce, měly vrátit **hodnotu false**. Pokud je **ví,** co rozhodnutí provádět, by podle toho nastavit výsledek a vrátí **hodnotu true**.
 
```csharp
class MyExceptionHandler : IExceptionHandler
{
    public bool TryHandleException(ExceptionInformation exceptionInformation, OperationRetrySettings retrySettings, out ExceptionHandlingResult result)
    {
        // if exceptionInformation.Exception is known and is transient (can be retried without re-resolving)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, true, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;


        // if exceptionInformation.Exception is known and is not transient (indicates a new service endpoint address must be resolved)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, false, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;

        // if exceptionInformation.Exception is unknown (let the next IExceptionHandler attempt to handle it)
        result = null;
        return false;
    }
}
```
### <a name="putting-it-all-together"></a>Vložení vůbec
S `ICommunicationClient`, `ICommunicationClientFactory`, a `IExceptionHandler` integrované kolem komunikační protokol `ServicePartitionClient` je zalamuje vůbec a poskytuje smyčka rozlišení adresu poruch zpracování a service oddíl kolem tyto součásti.

```csharp
private MyCommunicationClientFactory myCommunicationClientFactory;
private Uri myServiceUri;

var myServicePartitionClient = new ServicePartitionClient<MyCommunicationClient>(
    this.myCommunicationClientFactory,
    this.myServiceUri,
    myPartitionKey);

var result = await myServicePartitionClient.InvokeWithRetryAsync(async (client) =>
   {
      // Communicate with the service using the client.
   },
   CancellationToken.None);

```

## <a name="next-steps"></a>Další kroky
 - Viz příklad HTTP komunikace mezi službami v [ukázkové projektu na GitHUb](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/Services/WordCount).

 - [Vzdálené volání procedur s Vzdálená spolehlivé služby](service-fabric-reliable-services-communication-remoting.md)

 - [Webového rozhraní API, který používá OWIN spolehlivé služby](service-fabric-reliable-services-communication-webapi.md)

 - [Komunikace pomocí spolehlivé služby WCF](service-fabric-reliable-services-communication-wcf.md)
