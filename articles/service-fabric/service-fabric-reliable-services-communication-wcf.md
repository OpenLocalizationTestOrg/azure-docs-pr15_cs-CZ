<properties
   pageTitle="Spolehlivé zásobníku komunikace služby WCF | Microsoft Azure"
   description="Předdefinované zásobníku komunikace WCF struktury služba poskytuje komunikace klienta služby WCF spolehlivé služby."
   services="service-fabric"
   documentationCenter=".net"
   authors="BharatNarasimman"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="07/26/2016"
   ms.author="bharatn"/>

# <a name="wcf-based-communication-stack-for-reliable-services"></a>Na základě WCF komunikace zásobníku spolehlivé služby
Spolehlivé služby framework autorům služby zvolte zásobníku komunikace, který se má použít pro své služby. Můžete připojit ve vrstvě komunikace zvoleném prostřednictvím **ICommunicationListener** vrácených [CreateServiceReplicaListeners nebo CreateServiceInstanceListeners](service-fabric-reliable-services-communication.md) metody. Rámci poskytuje implementaci komunikace zásobníku založené na Windows Communication Foundation (WCF) autoři služby, které chcete použít na základě WCF komunikace.

## <a name="wcf-communication-listener"></a>WCF komunikace posluchače
Specifické pro WCF provádění **ICommunicationListener** poskytuje společnost **Microsoft.ServiceFabric.Services.Communication.Wcf.Runtime.WcfCommunicationListener** předmětu.

Přidejte přivítejte máme smlouvu typu`ICalculator`

```csharp
[ServiceContract]
public interface ICalculator
{
    [OperationContract]
    Task<int> Add(int value1, int value2);
}
```

Můžeme vytvořit posluchače komunikace WCF ve službě následujícím způsobem.

```csharp

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[] { new ServiceReplicaListener((context) =>
        new WcfCommunicationListener<ICalculator>(
            wcfServiceObject:this,
            serviceContext:context,
            //
            // The name of the endpoint configured in the ServiceManifest under the Endpoints section
            // that identifies the endpoint that the WCF ServiceHost should listen on.
            //
            endpointResourceName: "WcfServiceEndpoint",

            //
            // Populate the binding information that you want the service to use.
            //
            listenerBinding: WcfUtility.CreateTcpListenerBinding()
        )
    )};
}

```

## <a name="writing-clients-for-the-wcf-communication-stack"></a>Psaní klientů WCF komunikace zásobníku
Pro psaní klientům komunikaci se službami pomocí WCF, poskytuje rozhraní **WcfClientCommunicationFactory**, což je WCF specifické provádění [ClientCommunicationFactoryBase](service-fabric-reliable-services-communication.md).

```csharp

public WcfCommunicationClientFactory(
    Binding clientBinding = null,
    IEnumerable<IExceptionHandler> exceptionHandlers = null,
    IServicePartitionResolver servicePartitionResolver = null,
    string traceId = null,
    object callback = null);
```

Komunikační kanál WCF je přístupný z **WcfCommunicationClient** vytvořil **WcfCommunicationClientFactory**.

```csharp

public class WcfCommunicationClient : ServicePartitionClient<WcfCommunicationClient<ICalculator>>
   {
       public WcfCommunicationClient(ICommunicationClientFactory<WcfCommunicationClient<ICalculator>> communicationClientFactory, Uri serviceUri, ServicePartitionKey partitionKey = null, TargetReplicaSelector targetReplicaSelector = TargetReplicaSelector.Default, string listenerName = null, OperationRetrySettings retrySettings = null)
           : base(communicationClientFactory, serviceUri, partitionKey, targetReplicaSelector, listenerName, retrySettings)
       {
       }
   }

```

**WcfCommunicationClientFactory** spolu s **WcfCommunicationClient** **ServicePartitionClient** určit koncový bod služby a komunikovat s služby, které můžete použít kód klienta.

```csharp
// Create binding
Binding binding = WcfUtility.CreateTcpClientBinding();
// Create a partition resolver
IServicePartitionResolver partitionResolver = ServicePartitionResolver.GetDefault();
// create a  WcfCommunicationClientFactory object.
var wcfClientFactory = new WcfCommunicationClientFactory<ICalculator>
    (clientBinding: binding, servicePartitionResolver: partitionResolver);

//
// Create a client for communicating with the ICalculator service that has been created with the
// Singleton partition scheme.
//
var calculatorServiceCommunicationClient =  new WcfCommunicationClient(
                wcfClientFactory,
                ServiceUri,
                ServicePartitionKey.Singleton);

//
// Call the service to perform the operation.
//
var result = calculatorServiceCommunicationClient.InvokeWithRetryAsync(
                client => client.Channel.Add(2, 3)).Result;

```
>[AZURE.NOTE] Výchozí ServicePartitionResolver předpokládá, že ve stejném obrázku jako služba je spuštěn klienta. Pokud tedy v opačném případě vytvořit objekt ServicePartitionResolver a předejte koncové body připojení obrázku.

## <a name="next-steps"></a>Další kroky
* [Vzdálené volání procedur s Vzdálená spolehlivé služby](service-fabric-reliable-services-communication-remoting.md)

* [Webového rozhraní API s OWIN spolehlivé služby](service-fabric-reliable-services-communication-webapi.md)

* [Zabezpečení komunikace spolehlivé služby](service-fabric-reliable-services-secure-communication.md)
