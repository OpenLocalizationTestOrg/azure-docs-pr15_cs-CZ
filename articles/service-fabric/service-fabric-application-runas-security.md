<properties
   pageTitle="Princip služby struktury aplikace a služby zásady zabezpečení | Microsoft Azure"
   description="Základní informace o tom, jak spustit aplikaci služby struktury v části systém a zabezpečení účty, včetně SetupEntry bodu, kde aplikace potřebuje k provedení některých privilegovaných akce před spuštěním"
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
   ms.date="09/22/2016"
   ms.author="mfussell"/>

# <a name="configure-security-policies-for-your-application"></a>Konfigurace zásad zabezpečení pro aplikace
Azure struktury služby umožňuje zajistit aplikace spuštěné v obrázku v části různé uživatelské účty. Služba struktury také zabezpečuje prostředky používané aplikace v době nasazení uživatelského účtu, jako jsou soubory, adresářů a certifikáty. Díky spuštěné aplikace, i ve sdíleném hostovanou prostředí, zabezpečené od sebe. 

Ve výchozím nastavení služby struktury aplikace spustit pod účtem, které se spustí proces Fabric.exe ve skupinovém rámečku. Služba struktury také poskytuje možnost spouštění aplikací v části místní uživatelský účet nebo místní systémový účet zadanou v rámci manifestu aplikace. Podporované místní systémový účet typy jsou **MístníUživatel**, **NetworkService**, **LocalService**a **LocalSystem**.

 Při spuštění služby struktury v systému Windows Server ve vašem Centru dat pomocí samostatný instalační program, můžete účty domény Active Directory (AD).

Skupiny uživatelů můžete definované a vytvořili, aby pro každou skupinu pohromadě spravovanou lze přidat jeden nebo víc uživatelů. To je užitečné, pokud existuje více uživatelů pro jiné službě vstupní body a budou muset mít některých běžných oprávnění, které jsou dostupné na úrovni skupiny.

## <a name="configure-the-policy-for-service-setupentrypoint"></a>Konfigurace zásad pro služby SetupEntryPoint

Jak je uvedeno v [modelu aplikace](service-fabric-application-model.md) je **SetupEntryPoint** privilegovaných vstupní bod, který spustí s stejné přihlašovací údaje jako služby struktury (obvykle účet *NetworkService* ) před další vstupní bod. Spustitelný soubor určený **vstupní bod** je obvykle hostitele služby dlouhodobě spuštěných, ke které mají zvláštní nastavení vstupní bod zabráněno museli spouštět hostitele služby spustitelný s vysokou úrovní oprávnění pro dlouhou dobu. Spustitelný soubor určený **vstupní bod** běží po **SetupEntryPoint** zavře úspěšně. Sledovat a restartování procesu výsledné znovu začínající **SetupEntryPoint**, pokud vypršela jeho někdy ukončí nebo dojde k chybě.

Následující obrázek je příklad seznamu jednoduché služby, který ukazuje SetupEntryPoint a hlavní vstupní bod pro službu.

~~~
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>
  <ConfigPackage Name="Config" Version="1.0.0" />
</ServiceManifest>
~~~

### <a name="configure-the-policy-using-a-local-account"></a>Konfigurace zásad pomocí místního účtu

Po konfiguraci služby mít vzhled vstupní bod, můžete změnit oprávnění, která je kompatibilní se v manifestu aplikace. Příklad followin znázorňuje konfigurace služby spuštění ve skupinovém rámečku oprávnění uživatelského účtu správce.

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupAdminUser" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupAdminUser">
            <MemberOf>
               <SystemGroup Name="Administrators" />
            </MemberOf>
         </User>
      </Users>
   </Principals>
</ApplicationManifest>
~~~

Nejdřív vytvořte oddíl **objekty** se uživatelské jméno, například SetupAdminUser. Tento údaj označuje, že je uživatel členem skupiny Správci systému.

Pak v části **ServiceManifestImport** konfigurace zásad pro použití tohoto jistinu **SetupEntryPoint**. To, bude služby struktury, že při spuštění **MySetup.bat** souboru, je třeba RunAs s oprávněními správce. Vzhledem k tomu, že máte *nejsou* použity zásady pro hlavní vstupní bod, kód v **MyServiceHost.exe** běží v části systém **účet** . Toto je výchozí účet, který všechny služby vstupní body pracují jako.

Pojďme nyní přidat soubor MySetup.bat projekt aplikace Visual Studio testování s oprávněními správce. Ve Visual Studiu klikněte pravým tlačítkem myši na projekt služby a přidejte nový soubor s názvem MySetup.bat.

Dále se ujistěte, že soubor MySetup.bat je součástí balíčku služby. Ve výchozím nastavení není. Vyberte soubor, klikněte pravým tlačítkem myši zobrazíte místní nabídku a vyberte **Vlastnosti**. V dialogovém okně Vlastnosti ověřte, zda **zkopírovat do výstupu** zkopírujte **Pokud novější**. Viz následující snímek obrazovky.

![Visual Studio CopyToOutput pro SetupEntryPoint dávkový soubor][image1]

Nyní otevřete soubor MySetup.bat a přidejte následující příkazy:

~~~
REM Set a system environment variable. This requires administrator privilege
setx -m TestVariable "MyValue"
echo System TestVariable set to > out.txt
echo %TestVariable% >> out.txt

REM To delete this system variable us
REM REG delete "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v TestVariable /f
~~~

Dále vytvořte a nasaďte řešení místní vývoj clusteru.  Jakmile službu začala, jak je vidět v Průzkumníku struktury služby, uvidíte, že MySetup.bat byl úspěšný dvěma způsoby. Otevřete okno příkazového prostředí PowerShell a zadejte:

~~~
PS C:\ [Environment]::GetEnvironmentVariable("TestVariable","Machine")
MyValue
~~~

Poznamenejte název uzel kde službu nasazené a na úvodní obrazovce Explorer struktury služby, třeba uzel 2. Další přejděte do složky pracovní instance aplikace out.txt soubor, který se zobrazuje hodnota **TestVariable**. Pro tuto službu nasazené uzel 2, a pak je-li například můžete přejít na tyto materiály pro **MyApplicationType**:

~~~
C:\SfDevCluster\Data\_App\Node.2\MyApplicationType_App\work\out.txt
~~~

###  <a name="configure-the-policy-using-local-system-accounts"></a>Konfigurace zásad pomocí místní systémový účet
Často je vhodnější spuštění spouštěcího skriptu pomocí místní systémový účet spíše než pomocí účtu správce viz před. Spuštění RunAs zásady jako správci obvykle nefunguje a od počítačích mít přístup k řízení Uživatelských účtů standardně zapnutá. V takovém případě **doporučujeme spouštět SetupEntryPoint LocalSystem místo místní uživatel přidaný do skupiny administrators**. Následující příklad ukazuje nastavení SetupEntryPoint spuštění LocalSystem.

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupLocalSystem" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupLocalSystem" AccountType="LocalSystem" />
      </Users>
   </Principals>
</ApplicationManifest>
~~~

##  <a name="launch-powershell-commands-from-a-setupentrypoint"></a>Spuštění prostředí PowerShell příkazy z SetupEntryPoint
Prostředí PowerShell od bodu **SetupEntryPoint** spustíte můžete spustit **PowerShell.exe** v dávkový soubor, který odkazuje na soubor Powershellu. Nejdřív přidejte do projektu služby, jako je **MySetup.ps1**souboru Powershellu. Nezapomeňte nastavit vlastnost *kopírování: když novější* tak, že soubor je také součástí balíčku služby. Následující příklad ukazuje ukázkový soubor dávku spustit prostředí PowerShell soubor s názvem MySetup.ps1, která nastavuje proměnnou prostředí systému s názvem **TestVariable**.


MySetup.bat spustit prostředí PowerShell soubor.

~~~
powershell.exe -ExecutionPolicy Bypass -Command ".\MySetup.ps1"
~~~

V souboru prostředí PowerShell přidáte následujícím postupem nastavit proměnnou prostředí systému:

~~~
[Environment]::SetEnvironmentVariable("TestVariable", "MyValue", "Machine")
[Environment]::GetEnvironmentVariable("TestVariable","Machine") > out.txt
~~~

**Poznámka:** Ve výchozím nastavení při spuštění dávkový soubor vypadá ve složce aplikace s názvem **práce** pro soubory. V tomto případě při spuštění MySetup.bat chceme najít MySetup.ps1 do stejné složky, která je složka **kód balíček** aplikace. Chcete-li změnit složku, nastavte pracovní složku jak je vidět v následujícím.

~~~
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    </ExeHost>
</SetupEntryPoint>
~~~

## <a name="using-console-redirection-for-local-debugging"></a>Používání přesměrování konzoly pro místní ladění
V některých případech je vhodné najdete v článku výstup konzoly spustit skript pro účely ladění. K tomu můžete nastavit zásady přesměrování konzoly, který data zapisuje výstup do souboru. Výstupní soubor zapsán do složky aplikace s názvem **protokolu** na uzel, kde je aplikace nasazeném a spuštění (viz téma vyhledání to v předchozím příkladu).

**Poznámka: nikdy** použití zásad přesměrování konzoly v aplikaci nasazenou v výrobní, protože to může být ovlivněné převzetí aplikace. **Pouze** pomocí tohoto příkazu pro účely ladění místní vývoje a.  

Následující příklad ukazuje nastavení přesměrování konzoly s hodnotou FileRetentionCount.

~~~
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="10"/>
    </ExeHost>
</SetupEntryPoint>
~~~

Pokud změníte teď souboru MySetup.ps1 psát příkaz **ozvěn tím** , to zapíše do ve výstupním souboru pro účely ladění.

~~~
Echo "Test console redirection which writes to the application log folder on the node that the application is deployed to"
~~~

**Jakmile dokončíte ladění skriptu, okamžitě odeberte tuto zásadu přesměrování konzoly**

## <a name="configure-policy-for-service-code-packages"></a>Konfigurace zásad pro služby kód balíčků 
V předchozích krocích viděli, jak chcete použití zásad RunAs SetupEntryPoint. Podívejme se trochu hlouběji do postup vytvoření různých objektů, které lze použít jako zásady služby.

### <a name="create-local-user-groups"></a>Vytvoření skupiny místních uživatelů
Skupiny uživatelů můžete definované a vytvořený povolit jednoho nebo víc uživatelů, které budou přidány do skupiny. To je užitečné, pokud existuje více uživatelů pro jiné službě vstupní body a budou muset mít některých běžných oprávnění, které jsou dostupné na úrovni skupiny. Následující příklad ukazuje místní skupiny nazvané **LocalAdminGroup** s oprávněními správce. Dva uživatelé Customer1 a Customer2, jsou určené členové této skupiny místních.

~~~
<Principals>
 <Groups>
   <Group Name="LocalAdminGroup">
     <Membership>
       <SystemGroup Name="Administrators"/>
     </Membership>
   </Group>
 </Groups>
  <Users>
     <User Name="Customer1">
        <MemberOf>
           <Group NameRef="LocalAdminGroup" />
        </MemberOf>
     </User>
    <User Name="Customer2">
      <MemberOf>
        <Group NameRef="LocalAdminGroup" />
      </MemberOf>
    </User>
  </Users>
</Principals>
~~~

### <a name="create-local-users"></a>Vytvoření místních uživatelů
Můžete vytvořit místního uživatele, které mohou sloužit k zabezpečení služeb v aplikaci. Pokud není zadán **MístníUživatel** typ účtu v části objekty manifestu aplikace, struktury služby vytvoří místních uživatelských účtů v počítačích, kde je aplikace nasazena. Ve výchozím nastavení tyto účty nemusíte stejný název jako jsou uvedeny v aplikaci instalované (třeba Customer3 v následujícím příkladu). Místo toho se dynamicky vytvářejí a náhodné hesla.

~~~
<Principals>
  <Users>
     <User Name="Customer3" AccountType="LocalUser" />
  </Users>
</Principals>
~~~

<!-- If an application requires that the user account and password be same on all machines (for example, to enable NTLM authentication), the cluster manifest must set NTLMAuthenticationEnabled to true. The cluster manifest must also specify an NTLMAuthenticationPasswordSecret that will be used to generate the same password across all machines.

<Section Name="Hosting">
      <Parameter Name="EndpointProviderEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationPassworkSecret" Value="******" IsEncrypted="true"/>
 </Section>
-->

### <a name="assign-policies-to-the-service-code-packages"></a>Přiřazení zásad balíčků kód služby
V části **RunAsPolicy** **ServiceManifestImport** Určuje účet v oddílu objekty použitý ke spuštění balíčku kód. Je taky přidružuje kód balíčků z manifest služby uživatelské účty v oddílu objekty. To můžete zadat nastavení nebo hlavní vstupní body, nebo můžete zadat `All` použít pro obě. Následující příklad ukazuje různé zásady nepoužijí:

~~~
<Policies>
<RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup"/>
<RunAsPolicy CodePackageRef="Code" UserRef="Customer3" EntryPointType="Main"/>
</Policies>
~~~

Pokud není zadán **EntryPointType** , výchozí hodnota je nastavena na EntryPointType = "Hlavní". Zadání **SetupEntryPoint** je užitečná, když budete chtít spustit operaci nastavení určitých vysokou prioritou pod účtem systému. V části účet nižší oprávnění by umožnit spuštění kódu aktuální služby.

### <a name="apply-a-default-policy-to-all-service-code-packages"></a>Použít výchozí zásady pro všechny balíčky kód služby
V části **DefaultRunAsPolicy** slouží k určení výchozí uživatelský účet u všech balíčků kód, ve kterých není konkrétní **RunAsPolicy** definované. Pokud většina balíčků kód podle manifest služby používané aplikace potřeba spustit podle jednoho uživatele, můžete aplikaci jenom definovat výchozí zásady RunAs se s uživatelským účtem. Následující příklad určuje, že pokud balíček kódu nemá **RunAsPolicy** zadali, balíček kód by měla běžet v části **MyDefaultAccount** uvedené v oddílu objekty.

~~~
<Policies>
  <DefaultRunAsPolicy UserRef="MyDefaultAccount"/>
</Policies>
~~~
### <a name="using-an-active-directory-domain-group-or-user"></a>Použití služby Active Directory domain skupiny nebo uživatele
Pro struktury služby součástí samostatný instalační program v systému Windows Server, můžete spustit tato služba v části přihlašovací údaje Active Directory (AD) účet uživatele nebo skupiny. Poznámka: Toto je služby Active Directory v místní uvnitř vaší domény která není s Azure Active Directory (AAD). Pomocí domény uživatele nebo skupiny, pak se dostanete dalších zdrojů v doméně (například sdílené složky), které máte přidělená oprávnění.

Následující příklad ukazuje uživatele služby AD s názvem *TestUser* heslem své domény šifrovaná pomocí certifikátu s názvem *MyCert*. Můžete použít `Invoke-ServiceFabricEncryptText` Powershell command vytvořit tajné zašifrovaný text. Viz Tento článek [Správa tajemství v aplikacích služby struktury](service-fabric-application-secret-management.md) podrobnosti o. Soukromý klíč certifikát, který chcete dešifrování hesla musí být nasazené na místním počítači v metodu mimo pásma (v Azure jedná prostřednictvím Správce prostředků). Při služby struktury nasadí balíčku služby do počítače, bude moct dešifrování tajná a spolu s uživatelské jméno ověření s AD spouštět v těchto přihlašovacích údajů.

~~~
<Principals>
  <Users>
    <User Name="TestUser" AccountType="DomainUser" AccountName="Domain\User" Password="[Put encrypted password here using MyCert certificate]" PasswordEncrypted="true" />
  </Users>
</Principals>
<Policies>
  <DefaultRunAsPolicy UserRef="TestUser" />
  <SecurityAccessPolicies>
    <SecurityAccessPolicy ResourceRef="MyCert" PrincipalRef="TestUser" GrantRights="Full" ResourceType="Certificate" />
  </SecurityAccessPolicies>
</Policies>
<Certificates>
~~~


## <a name="assign-securityaccesspolicy-for-http-and-https-endpoints"></a>Přiřazení SecurityAccessPolicy pro koncové body HTTP a HTTPS
Pokud použijete zásady RunAs ke službě a manifest služby prohlašuje koncový bod zdroje s protokolu HTTP, je nutné zadat **SecurityAccessPolicy** zajistit porty přidělit tyto koncové body správně uvedeny RunAs uživatelský účet, který se spouští služba Řízení přístupu. V opačném **http** neměl přístup ke službě a najděte selhání s voláním z klienta. Příklad followin se týká účtu Customer3 koncový bod s názvem **ServiceEndpointName**, udělit práva plný přístup.

~~~
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
   <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
</Policies>
~~~

HTTPS koncového bodu máte taky uveďte název certifikát, který chcete vrátit k desktopovému klientovi. Lze provést pomocí **EndpointBindingPolicy**certifikátem certifikáty bodu v manifestu aplikace.

~~~
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
  <!--EndpointBindingPolicy is needed if the EndpointName is secured with https -->
  <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
</Policies
~~~


## <a name="a-complete-application-manifest-example"></a>Příklad seznamu dokončení aplikace
Následující manifestu aplikace zobrazí spoustu různých nastavení:

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application3Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Parameters>
      <Parameter Name="Stateless1_InstanceCount" DefaultValue="-1" />
   </Parameters>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
         <RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup" />
        <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
         <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
        <!--EndpointBindingPolicy is needed the EndpointName is secured with https -->
        <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
     </Policies>
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="Stateless1">
         <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
   <Principals>
      <Groups>
         <Group Name="LocalAdminGroup">
            <Membership>
               <SystemGroup Name="Administrators" />
            </Membership>
         </Group>
      </Groups>
      <Users>
         <User Name="LocalAdmin">
            <MemberOf>
               <Group NameRef="LocalAdminGroup" />
            </MemberOf>
         </User>
         <!--Customer1 below create a local account that this service runs under -->
         <User Name="Customer1" />
         <User Name="MyDefaultAccount" AccountType="NetworkService" />
      </Users>
   </Principals>
   <Policies>
      <DefaultRunAsPolicy UserRef="LocalAdmin" />
   </Policies>
   <Certificates>
     <EndpointCertificate Name="Cert1" X509FindValue="FF EE E0 TT JJ DD JJ EE EE XX 23 4T 66 "/>
  </Certificates>
</ApplicationManifest>
~~~


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Další kroky

* [Principy modelu aplikace](service-fabric-application-model.md)
* [Určení zdroje v manifest služby](service-fabric-service-manifest-resources.md)
* [Nasazení aplikace](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
