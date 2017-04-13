<properties
   pageTitle="Služby struktury a nasazení kontejnery | Microsoft Azure"
   description="Služba struktury a používání kontejnery nasadit microservice aplikace. Tento článek popisuje funkce, které služby struktury poskytuje kontejnerů a nasazení kontejneru bitovou do clusteru"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/24/2016"
   ms.author="msfussell"/>

# <a name="preview-deploy-a-windows-container-to-service-fabric"></a>Náhled: Nasazení kontejneru Windows struktury služby

> [AZURE.SELECTOR]
- [Nasazení kontejnerem systému Windows](service-fabric-deploy-container.md)
- [Nasazení Docker kontejneru](service-fabric-deploy-container-linux.md)

Tento článek vás provede vytváření kontejnerizovaná služby v systému Windows kontejnery. 

>[AZURE.NOTE] Tato funkce je v náhledu Linux a aktuálně nedostupné pro použití s Windows serveru 2016. To až to bude k dispozici ve verzi preview pro Windows Server 2016 do příští verze aplikace služby struktury a podporovaných v na pozdější verzi.

Služba struktury má několik kontejneru funkcí, které vám pomohou s vytváření aplikací, které se skládají z microservices, které jsou kontejnerizovaná. Nazývají kontejnerizovaná služby. 

Možnosti zahrnují;

- Kontejner obrázek nasazení a aktivace
- Zásady správného řízení zdrojů
- Ověřování úložiště
- Kontejner port mapování portů Host (hostitel)
- Kontejner kontejneru zjišťování a hodnocení komunikace
- Možnost konfigurace a nastavení proměnné

Umožňuje, podívejte se na každý z funkcí zase při sbalení kontejnerizovaná služby chcete zahrnout do aplikace.

## <a name="packaging-a-windows-container"></a>Balení kontejneru systému Windows

Pokud balení kontejneru, můžete buď použít šablonu projektu Visual Studio nebo [Ruční vytvoření balíčku aplikace](#manually). Pomocí aplikace Visual Studio, struktury balíčku aplikace a seznamu soubory jsou vytvořené pomocí Průvodce nového projektu za vás (Toto je tu do příští verze).

## <a name="using-visual-studio-to-package-an-existing-container-image"></a>Používání aplikace Visual Studio zabalit existující obrázek kontejneru

>[AZURE.NOTE] V budoucí verzi aplikace Visual Studio nástroje SDK budou moct kontejneru do aplikace podobným způsobem, můžete přidat spustitelný Host dnes. V tématu [nasazení spustitelný služby struktury Host](service-fabric-deploy-existing-app.md) . Momentálně je potřeba udělat ruční balení podle níže uvedeného popisu.

<a id="manually"></a>
## <a name="manually-packaging-and-deploying-a-container"></a>Ruční balení a nasazení kontejneru
Proces ručně balení kontejnerizovaná služby vychází z následujících kroků:

1. Kontejnery publikovat do úložiště.
2. Vytvoření struktury adresáře balíčku.
3. Upravte soubor služby.
4. Upravte soubor aplikace.

## <a name="container-image-deployment-and-activation"></a>Kontejner obrázek nasazení a aktivace.
V části služba struktury [model aplikace](service-fabric-application-model.md)představuje kontejneru hostitele aplikace ve kterém jsou umístěná více replikách služby. Zavedení a aktivace kontejneru, umístění jméno container obrázek do `ContainerHost` prvek manifest služby.

V manifest služby přidejte `ContainerHost` položka najeďte myší a nastavte `ImageName` je jméno container úložiště a obrázek. Částečné manifest následující příklad nasazení kontejneru s názvem *myimage:v1* z úložiště s názvem *myrepo*

    <CodePackage Name="Code" Version="1.0">
        <EntryPoint>
          <ContainerHost>
            <ImageName>myrepo/myimagename:v1</ImageName>
            <Commands></Commands>
          </ContainerHost>
        </EntryPoint>
    </CodePackage>

Zadáte vstupní příkazy na kontejner obrázek zadáním volitelné `Commands` prvek čárkou oddělený sady příkazů ke spuštění uvnitř kontejneru. 

## <a name="resource-governance"></a>Zásady správného řízení zdrojů
Zásady správného řízení zdrojů je funkce kontejneru a omezením prostředky, které kontejneru můžete použít na hostiteli. `ResourceGovernancePolicy`, Podle manifest aplikace, umožňuje deklarovat omezení zdrojů kódu balíčku služby. Omezení prostředků můžete nastavit

- Paměti
- MemorySwap
- CpuShares (relativní důležitost procesoru)
- MemoryReservationInMB  
- BlkioWeight (relativní důležitost BlockIO). 

>[AZURE.NOTE] V budoucí verzi podpory určete omezení vstupu a výstupu konkrétní blok například procesorů plánu najdete pro čtení i zápis, že b/s i další uživatelé bude možné.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ResourceGovernancePolicy CodePackageRef="FrontendService.Code" CpuShares="500" 
            MemoryInMB="1024" MemorySwapInMB="4084" MemoryReservationInMB="1024" />
        </Policies>
    </ServiceManifestImport>


## <a name="repository-authentication"></a>Ověřování úložiště
Pokud si chcete stáhnout kontejneru bude pravděpodobně nutné zadat přihlašovací údaje do úložiště kontejner. Přihlašovací údaje zadané v manifestu *aplikace* používají k určení přihlašovací informace nebo klíč SSH pro její stažení kontejneru obrázek z úložiště obrázek.  Následující příklad ukazuje účet s názvem *TestUser* spolu s heslo ve formátu prostého textu. To **nedoporučujeme** .


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="12345" PasswordEncrypted="false"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>

Hesla můžete a zašifrovat pomocí certifikát používaný k počítači.

Následující příklad ukazuje účet s názvem *TestUser* heslem šifrovaná pomocí certifikátu s názvem *MyCert*. Můžete použít `Invoke-ServiceFabricEncryptText` Powershell command vytvořit tajné zašifrovaný text k zadání hesla. Viz Tento článek [Správa tajemství v aplikacích služby struktury](service-fabric-application-secret-management.md) podrobnosti o. Soukromý klíč certifikát, který chcete dešifrování hesla musí být nasazené na místním počítači v metodu mimo pásma (v Azure to spočívá ve využití ARM). Při služby struktury nasadí balíčku služby do počítače, bude moct dešifrování tajná a spolu s názvu účtu ověřování s úložištěm kontejneru pomocí těchto přihlašovacích údajů.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="[Put encrypted password here using MyCert certificate ]" PasswordEncrypted="true"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>

## <a name="container-port-to-host-port-mapping"></a>Kontejner port mapování portů Host (hostitel)
Konfigurace hostitele port slouží ke komunikaci s kontejnerem zadáním `PortBinding` v manifestu aplikace. Vazba port mapy port služby je přijímá na uvnitř kontejneru s portem na hostiteli.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>


## <a name="container-to-container-discovery-and-communication"></a>Kontejner kontejneru zjišťování a hodnocení komunikace
Použití `PortBinding` zásad mohli porovnat kontejneru port `Endpoint` v manifest služby, jak je vidět v následujícím příkladu. Koncový bod `Endpoint1` můžete zadat pevné číslo portu, například porty 80 nebo zadejte žádné port vůbec v takovém případě náhodné port z oblasti port aplikace clusterů zvolili za vás.

Pro hosta kontejnery určující `Endpoint` jako v manifest služby díky služby struktury automaticky publikujete tento koncový bod služby Naming tak, aby jiných služeb spuštěných v clusteru můžete zjistit pomocí dotazů vyřešit služba REST tento kontejner. 

    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>

Zaregistrováním ke službě Naming můžete snadno udělat-kontejnery komunikace kód v rámci vašeho kontejneru pomocí [reverzního proxy](service-fabric-reverseproxy.md). Je třeba udělat stačí zadat listening port http reverzního proxy a název služby, které chcete komunikovat s nastavením jako proměnné. V tomto postupu najdete v článku v další části.  

## <a name="configure-and-set-environment-variables"></a>Konfigurace a nastavte proměnné
Proměnné může být zadané nepřítel obalu kódu ve službě manifest pro obě služby nasazený v kontejnerech nebo procesy/hosta aplikace. Tyto prostředí proměnnými hodnotami můžete přepsat konkrétně na manifestu aplikace nebo zadaný během nasazení jako parametry aplikace.

Následující manifest služby fragmentu kódu XML příklad určení proměnné balíčku kód. 

    <ServiceManifest Name="FrontendServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description>a guest executable service in a container</Description>
        <ServiceTypes>
            <StatelessServiceType ServiceTypeName="StatelessFrontendService"  UseImplicitHost="true"/>
        </ServiceTypes>
        <CodePackage Name="FrontendService.Code" Version="1.0">
            <EntryPoint>
            <ContainerHost>
                <ImageName>myrepo/myimage:v1</ImageName>
                <Commands></Commands>
            </ContainerHost>
            </EntryPoint>
            <EnvironmentVariables>
                <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
                <EnvironmentVariable Name="BackendServiceName" Value=""/>
            </EnvironmentVariables>
        </CodePackage>
    </ServiceManifest>

Tyto proměnné můžete přepsat na úrovni seznamu aplikací:

    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <EnvironmentOverrides CodePackageRef="FrontendService.Code">
            <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvc]"/>
            <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
        </EnvironmentOverrides>
    </ServiceManifestImport>

Ve výše uvedeném příkladu jsme určili explicitní hodnotu pro `HttpGateway` proměnné (19000) při hodnotu pro `BackendServiceName` parametr nastaven prostřednictvím `[BackendSvc]` parametr aplikace. To umožňuje zadejte hodnotu pro `BackendServiceName`hodnota, pro čas nasazení aplikace a v manifest není k dispozici pevných hodnot. 

## <a name="complete-examples-for-application-and-service-manifest"></a>Dokončení příklady použití a manifest služby
Následujícím obrázku je manifest aplikace příklad, který ukazuje, jaké funkce kontejner.


    <ApplicationManifest ApplicationTypeName="SimpleContainerApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description>A simple service container application</Description>
        <Parameters>
            <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
            <Parameter Name="BackEndSvcName" DefaultValue="bkend"></Parameter>
        </Parameters>
        <ServiceManifestImport>
            <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
            <EnvironmentOverrides CodePackageRef="FrontendService.Code">
                <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvcName]"/>
                <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
            </EnvironmentOverrides>
            <Policies>
                <ResourceGovernancePolicy CodePackageRef="Code" CpuShares="500" MemoryInMB="1024" MemorySwapInMB="4084" MemoryReservationInMB="1024" />
                <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                    <RepositoryCredentials AccountName="username" Password="****" PasswordEncrypted="true"/>
                    <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
                </ContainerHostPolicies>
            </Policies>
        </ServiceManifestImport>
    </ApplicationManifest>


Následuje příklad manifest služby (zadané v předchozím manifestu aplikace), který ukazuje, jaké funkce kontejneru

    <ServiceManifest Name="FrontendServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description> A service that implements a stateless frontend in a container</Description>
        <ServiceTypes>
            <StatelessServiceType ServiceTypeName="StatelessFrontendService"  UseImplicitHost="true"/>
        </ServiceTypes>
        <CodePackage Name="FrontendService.Code" Version="1.0">
            <EntryPoint>
            <ContainerHost>
                <ImageName>myrepo/myimage:v1</ImageName>
                <Commands></Commands>
            </ContainerHost>
            </EntryPoint>
            <EnvironmentVariables>
                <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
                <EnvironmentVariable Name="BackendServiceName" Value=""/>
            </EnvironmentVariables>
        </CodePackage>
        <ConfigPackage Name="FrontendService.Config" Version="1.0" />
        <DataPackage Name="FrontendService.Data" Version="1.0" />
        <Resources>
            <Eendpoints>
                <Endpoint Name="Endpoint1" Port="80"  UriScheme="http" />
            </Eendpoints>
        </Resources>
    </ServiceManifest>
