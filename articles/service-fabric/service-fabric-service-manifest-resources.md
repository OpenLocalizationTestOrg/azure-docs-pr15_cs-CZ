<properties
   pageTitle="Určení koncové body služby služby struktury | Microsoft Azure"
   description="Jak popisují koncový bod zdrojů v manifest služby včetně nastavíte koncové body HTTPS"
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>

# <a name="specify-resources-in-a-service-manifest"></a>Určení zdroje v manifest služby

## <a name="overview"></a>Základní informace

Manifest služby umožňuje prostředky, které jsou použity službou být deklarované/změnit beze změny zkompilovaný kód. Azure struktury služby podporuje konfiguraci koncový bod prostředků pro službu. Přístup ke zdroji, které jsou uvedeny v manifest služby můžete řídit přes SecurityGroup v manifestu aplikace. Prohlášení o zdroje umožňuje tyto materiály, které bude možné měnit při nasazení, což znamená, že služba nevyžaduje představující nový mechanismus konfigurace. Definice schématu ServiceManifest.xml souboru je instalována jako součást SDK struktury služby a nástroje pro *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

## <a name="endpoints"></a>Koncové body

Když koncový bod zdroje je definována v manifest služby, struktury služby přiřadí porty z oblasti vyhrazené aplikační port při port není výslovně zadaný. Podívejte se třeba na koncový bod *ServiceEndpoint1* uvedené v seznamu fragment za daném odstavci. Služby mohou také požadovat odchozího v prostředků. Repliky služeb spuštěných pro různé uzlech je možné stanovit různých čísel portů, zatímco kopie služba spuštěná ve stejném uzlu sdílet port. Služba repliky můžete používat tyto porty podle potřeby replikace a přijímá žádosti klienta o.

```xml
<Resources>
  <Endpoints>
    <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
    <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
    <Endpoint Name="ServiceEndpoint3" Protocol="https"/>
  </Endpoints>
</Resources>
```

Podívejte se do [Konfigurace stavovou spolehlivé služby](service-fabric-reliable-services-configuration.md) dozvědět víc o odkazování na koncové body v nastavení balíčku konfigurace souboru (settings.xml).

## <a name="example-specifying-an-http-endpoint-for-your-service"></a>Příklad: zadání protokolu HTTP koncového bodu služby

Následující manifest služby definuje jeden zdroj koncového bodu TCP a dva zdroje koncový bod HTTP v &lt;zdroje&gt; prvek.

Koncové body HTTP jsou automaticky že ACL by podle struktury služby.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="Stateful1Pkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         This name must match the string used in the RegisterServiceType call in Program.cs. -->
    <StatefulServiceType ServiceTypeName="Stateful1Type" HasPersistedState="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ExeHost>
        <Program>Stateful1.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <!-- Config package is the contents of the Config directoy under PackageRoot that contains an
       independently updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port number on which to
           listen. Note that if your service is partitioned, this port is shared with
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
      <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
      <Endpoint Name="ServiceEndpoint3" Protocol="https"/>

      <!-- This endpoint is used by the replicator for replicating the state of your service.
           This endpoint is configured through the ReplicatorSettings config section in the Settings.xml
           file under the ConfigPackage. -->
      <Endpoint Name="ReplicatorEndpoint" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```

## <a name="example-specifying-an-https-endpoint-for-your-service"></a>Příklad: zadání koncového HTTPS službě

Protokol HTTPS poskytuje ověřování serveru a taky se používá pro šifrování komunikace klient server. Povolit HTTPS službě služby struktury, zadejte v protokolu *zdroje -> Koncové body -> koncový bod* část manifest služby, jako je uvedené výše koncového bodu *ServiceEndpoint3*.

>[AZURE.NOTE] Protocol (protokol) do služby nelze změnit během upgradu aplikace bez ho představující jazycích změnit.


Tady je příklad ApplicationManifest, které je potřeba nastavit pro HTTPS. Musí být uveden Miniatura certifikátu. EndpointRef je odkaz na EndpointResource v ServiceManifest, u kterého jste nastavili protokol HTTPS. Můžete přidat víc EndpointCertificate.  

```
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="Application1Type"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Parameters>
    <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="2" />
    <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
    <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion
       should match the Name and Version attributes of the ServiceManifest element defined in the
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateful1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <EndpointBindingPolicy CertificateRef="TestCert1" EndpointRef="ServiceEndpoint3"/>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types when an instance of this
         application type is created. You can also create one or more instances of service type by using the
         Service Fabric PowerShell module.

         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="Stateful1">
      <StatefulService ServiceTypeName="Stateful1Type" TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]" MinReplicaSetSize="[Stateful1_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[Stateful1_PartitionCount]" LowKey="-9223372036854775808" HighKey="9223372036854775807" />
      </StatefulService>
    </Service>
  </DefaultServices>
  <Certificates>
    <EndpointCertificate Name="TestCert1" X509FindValue="FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF F0" X509StoreName="MY" />  
  </Certificates>
</ApplicationManifest>
```
