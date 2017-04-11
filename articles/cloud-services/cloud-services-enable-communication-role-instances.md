<properties 
pageTitle="Komunikace pro role ve službě Cloud Services | Microsoft Azure" 
description="Role instancí služby cloudu můžete mít koncové body (http https, tcp, udp) pro ně nadefinovali komunikující s vně nebo mezi instancemi další roli." 
services="cloud-services" 
documentationCenter="" 
authors="Thraka" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="09/06/2016" 
ms.author="adegeo"/>

# <a name="enable-communication-for-role-instances-in-azure"></a>Povolení komunikace pro instance role v azure

Role služby cloudu komunikovat pomocí připojení k interním a externím. Připojení k externím se označují jako **vstupní koncové body** , zatímco vnitřní připojení se označují jako **interní koncové body**. Toto téma popisuje, jak změnit [definici služby](cloud-services-model-and-package.md#csdef) vytvořit koncové body.


## <a name="input-endpoint"></a>Koncový bod při zadávání
Zadávání koncový bod se používá při chcete vystavit port na vnější. Určete typ protokolu a port koncový bod, který potom platí pro externích a interních porty koncový bod. Pokud chcete, můžete určit jiné interní port koncového bodu s atributem [Místní_port](https://msdn.microsoft.com/library/azure/gg557552.aspx#InputEndpoint) .

Zadávání koncový bod můžete použít následující protokoly: **http, https, tcp a udp**.

Pokud chcete vytvořit vstupní koncový bod, přidejte do prvku **koncové body** role webu nebo pracovního **InputEndpoint** podřízený prvek.

```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
</Endpoints> 
```

## <a name="instance-input-endpoint"></a>Zadávání koncový bod instanci
Zadávání koncové body instance jsou podobné koncové body ale umožňuje zadat namapovat konkrétní porty veřejné pokaždé jednotlivé role pomocí port přesměrování na Vyrovnávání zatížení. Můžete zadat jeden port veřejné nebo oblasti porty.

Zadávání koncový bod instanci můžete použít jenom **tcp** a **udp** protokol.

Vytvoření koncového bodu vstupní instance, přidáte podřízený prvek **InstanceInputEndpoint** do prvku **koncové body** webu nebo pracovního role.

```xml
<Endpoints>
  <InstanceInputEndpoint name="Endpoint2" protocol="tcp" localPort="10100">
    <AllocatePublicPortFrom>
      <FixedPortRange max="10109" min="10105" />
    </AllocatePublicPortFrom>
  </InstanceInputEndpoint>
</Endpoints>
```

## <a name="internal-endpoint"></a>Vnitřní koncový bod
Vnitřní koncové body jsou k dispozici pro komunikaci instance instance. Port je nepovinný krok a vynecháte, se přiřadí dynamické port koncový bod. Rozsah portů lze použít. Existuje limit pět interní koncové body každou roli.

Vnitřní koncový bod můžete použít následující protokoly: **http, tcp a udp, všechny**.

Pokud chcete vytvořit interní vstupní koncový bod, přidejte do prvku **koncové body** role webu nebo pracovního **InternalEndpoint** podřízený prvek.

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any" port="8999" />
</Endpoints> 
```

Můžete taky použít port oblasti.

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any">
    <FixedPortRange max="8995" min="8999" />
  </InternalEndpoint>
</Endpoints>
```


## <a name="worker-roles-vs-web-roles"></a>Role pracovníka porovnání Web role

Při práci s kolegy a web role je jeden menší rozdíl s koncové body. Role webového musíte mít alespoň jeden vstupní koncový bod pomocí protokolu **HTTP** .


```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
  <!-- more endpoints may be declared after the first InputEndPoint -->
</Endpoints>
```

## <a name="using-the-net-sdk-to-access-an-endpoint"></a>Použití .NET SDK pro přístup k koncový bod
Knihovna spravovaných Azure poskytuje metody role instance komunikovat za běhu. Z kód spuštěný v rámci instanci rolí můžete získat informace o ostatních rolí instancí a jejich koncové body a informace o aktuální instance role.

> [AZURE.NOTE] Pouze můžete získat informace o role instance se systémem do cloudové služby, které definují aspoň jeden interní koncový bod. Nemůže získat data o role instance spuštěna v jiné službě.

Vlastnost [instance](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.role.instances.aspx) slouží k načtení výskyty roli. Nejdřív pomocí [CurrentRoleInstance](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.currentroleinstance.aspx) vrátit odkaz na aktuální instanci rolí a pak pomocí vlastnost [Role](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.role.aspx) vrátit odkaz na roli samotné.

Při připojení k instanci rolí programově prostřednictvím .NET SDK je poměrně snadný přístup k informacím koncového bodu. Když už jste připojili ke prostředí konkrétní rolí, zobrazí se port konkrétní koncový bod se tento kód:

```csharp
int port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["StandardWeb"].IPEndpoint.Port;
```

Vlastnost **instance** vrátí kolekci **RoleInstance** objektů. Tato kolekce vždy obsahuje aktuální instance. Pokud roli není definujte interní koncový bod, kolekci obsahuje aktuální instance, ale žádné instance. Počet instancí role v kolekci vždycky 1 v případě, kde je definována žádný vnitřní koncový bod pro roli. Pokud roli definuje interní koncový bod, její instance lze zjistit za běhu a počet instancí v kolekci bude odpovídat počet instancí určeným pro roli v souboru konfigurace služby.

> [AZURE.NOTE] Knihovnu spravovaných Azure neposkytuje prostředků pro určování stavu ostatních rolí instancí, ale můžete používat tyto posuzování stavu sami Pokud služby potřebuje tuto funkci. [Diagnostika Azure](cloud-services-dotnet-diagnostics.md) můžete použít k získání informací o spuštěné instance role.

Vlastnost [InstanceEndpoints](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.instanceendpoints.aspx) k určení počtu port pro interní koncový bod na instanci rolí, slouží k vrácení objektu slovník, který obsahuje názvy koncového bodu a jejich odpovídající IP adres a porty. Vlastnost [IPEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstanceendpoint.ipendpoint.aspx) vrátí IP adresa a port pro určený koncový bod. Vlastnost **PublicIPEndpoint** vrátí číslo portu rozloženy koncového bodu. IP adresy část vlastnost **PublicIPEndpoint** nepoužívá.

Tady je příklad, která provádí iterace role instance.

```csharp
foreach (RoleInstance roleInst in RoleEnvironment.CurrentRoleInstance.Role.Instances)
{
    Trace.WriteLine("Instance ID: " + roleInst.Id);
    foreach (RoleInstanceEndpoint roleInstEndpoint in roleInst.InstanceEndpoints.Values)
    {
        Trace.WriteLine("Instance endpoint IP address and port: " + roleInstEndpoint.IPEndpoint);
    }
}
```

Tady je příklad roli pracovníka, která koncový bod zveřejněným dostane definice služby a spustí naslouchají připojením.

> [AZURE.WARNING] Tento kód použít pouze pro nasazení služeb. Při spuštění v Azure výpočet emulátoru prvky konfigurace služby, které vytvoří koncové body přímé port (**InstanceInputEndpoint** prvky) jsou ignorovány.

```csharp
using System;
using System.Diagnostics;
using System.Linq;
using System.Net;
using System.Net.Sockets;
using System.Threading;
using Microsoft.WindowsAzure;
using Microsoft.WindowsAzure.Diagnostics;
using Microsoft.WindowsAzure.ServiceRuntime;
using Microsoft.WindowsAzure.StorageClient;

namespace WorkerRole1
{
  public class WorkerRole : RoleEntryPoint
  {
    public override void Run()
    {
      try
      {
        // Initialize method-wide variables
        var epName = "Endpoint1";
        var roleInstance = RoleEnvironment.CurrentRoleInstance;
        
        // Identify direct communication port
        var myPublicEp = roleInstance.InstanceEndpoints[epName].PublicIPEndpoint;
        Trace.TraceInformation("IP:{0}, Port:{1}", myPublicEp.Address, myPublicEp.Port);

        // Identify public endpoint
        var myInternalEp = roleInstance.InstanceEndpoints[epName].IPEndpoint;
                
        // Create socket listener
        var listener = new Socket(
          myInternalEp.AddressFamily, SocketType.Stream, ProtocolType.Tcp);
                
        // Bind socket listener to internal endpoint and listen
        listener.Bind(myInternalEp);
        listener.Listen(10);
        Trace.TraceInformation("Listening on IP:{0},Port: {1}",
          myInternalEp.Address, myInternalEp.Port);

        while (true)
        {
          // Block the thread and wait for a client request
          Socket handler = listener.Accept();
          Trace.TraceInformation("Client request received.");

          // Define body of socket handler
          var handlerThread = new Thread(
            new ParameterizedThreadStart(h =>
            {
              var socket = h as Socket;
              Trace.TraceInformation("Local:{0} Remote{1}",
                socket.LocalEndPoint, socket.RemoteEndPoint);

              // Shut down and close socket
              socket.Shutdown(SocketShutdown.Both);
              socket.Close();
            }
          ));

          // Start socket handler on new thread
          handlerThread.Start(handler);
        }
      }
      catch (Exception e)
      {
        Trace.TraceError("Caught exception in run. Details: {0}", e);
      }
    }

    public override bool OnStart()
    {
      // Set the maximum number of concurrent connections 
      ServicePointManager.DefaultConnectionLimit = 12;

      // For information on handling configuration changes
      // see the MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.
      return base.OnStart();
    }
  }
}
```

## <a name="network-traffic-rules-to-control-role-communication"></a>Sítě přenosy pravidla pro řízení role komunikace
Po definování interní koncové body, můžete přidat sítě přenosy pravidla (koncové body, které jste vytvořili) jak můžou vzájemně komunikovat role instance ovládacího prvku. Na následujícím obrázku vidíte některé běžné scénáře pro řízení komunikace role:

![Sítě přenosy pravidla scénáře] (./media/cloud-services-enable-communication-role-instances/scenarios.png "Sítě přenosy pravidla scénáře")

Následující příklad ukazuje role definice role v předchozí diagramu. Každá definice role obsahuje aspoň jeden interní koncový bod definované:

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WebRole name="WebRole1" vmsize="Medium">
    <Sites>
      <Site name="Web">
        <Bindings>
          <Binding name="HttpIn" endpointName="HttpIn" />
        </Bindings>
      </Site>
    </Sites>
    <Endpoints>
      <InputEndpoint name="HttpIn" protocol="http" port="80" />
      <InternalEndpoint name="InternalTCP1" protocol="tcp" />
    </Endpoints>
  </WebRole>
  <WorkerRole name="WorkerRole1">
    <Endpoints>
      <InternalEndpoint name="InternalTCP2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
  <WorkerRole name="WorkerRole2">
    <Endpoints>
      <InternalEndpoint name="InternalTCP3" protocol="tcp" />
      <InternalEndpoint name="InternalTCP4" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

> [AZURE.NOTE] Omezení komunikace mezi role může dojít při interní koncové body pevné a automaticky získá porty.

Ve výchozím nastavení po definování interní koncový bod komunikace můžete flow z libovolné role interní koncový bod role bez omezení. Omezení komunikace, musí **NetworkTrafficRules** element dodejte element **ServiceDefinition** v souboru definice služby.

### <a name="scenario-1"></a>Scénář 1
Povolte pouze, v síti z **WebRole1** můžete **WorkerRole1**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-2"></a>Scénář 2
Umožňuje pouze v síti z **WebRole1** **WorkerRole1** a **WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-3"></a>Scénář 3
Umožňuje pouze v síti z **WebRole1** **WorkerRole1**a **WorkerRole1** **WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-4"></a>Scénář 4
Umožňuje pouze v síti z **WebRole1** **WorkerRole1**, **WebRole1** **WorkerRole2**a **WorkerRole1** **WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP4" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

Přehled schématu XML pro prvky použít nad najdete [tady](https://msdn.microsoft.com/library/azure/gg557551.aspx).

## <a name="next-steps"></a>Další kroky
Další informace o cloudové službě [modelu](cloud-services-model-and-package.md).