<properties
   pageTitle="Model služby struktury aplikace | Microsoft Azure"
   description="Jak model a popisují aplikací a služeb v struktury služby."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor="mani-ramaswamy"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/10/2016"   
   ms.author="seanmck"/>

# <a name="model-an-application-in-service-fabric"></a>Vytváření modelů aplikace v struktury služby

Tento článek obsahuje přehled modelu aplikace struktury služby Azure. Také popisuje, jak definovat aplikace a služby prostřednictvím seznamu souborů a získat aplikaci sbalený a jste připraveni na nasazení.

## <a name="understand-the-application-model"></a>Principy modelu aplikace

Aplikace je sada základních služeb, které provést některé funkce a funkce. Do služby provádí dokončení a samostatnou funkci (ji můžete spustit a spusťte nezávisle na jiných služeb) a je tvořen kód, konfigurace a data. Pro každou službu kód obsahuje spustitelný binární soubory, nastavení se skládá ze nastavení služeb, které je možné načíst za běhu a data se skládají z libovolného statická data do určené pro službu. Jednotlivé součásti v tomto modelu hierarchické aplikací může být verzí a upgradovaném nezávisle na sobě.

![Model služby struktury aplikace][appmodel-diagram]


Typ aplikace je kategorizace aplikace a se skládá ze sady typy služeb. Typ služby je kategorizace služby. Kategorizaci mít různá nastavení a konfigurace, ale základní funkce se nezmění. Instancí služby jsou varianty konfigurace jiné službě stejného typu služby.  

Třídy (nebo "typy") aplikací a služeb jsou popsané prostřednictvím soubory XML (manifesty aplikace a služby manifesty), které jsou šablony, podle kterých může být vytvořena aplikací z úložiště obrázek obrázku. Definice schématu ServiceManifest.xml a ApplicationManifest.xml souboru je instalována jako součást SDK struktury služby a nástroje pro *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

Kód pro jiné aplikace instance spustí jako samostatné procesy, i když hostitelem stejný uzel struktury služby. Kromě toho lze (tedy spravovat cyklus jednotlivé instance aplikace nezávisle na sobě upgradovat). Na následujícím obrázku vidíte, jak jsou typy aplikací skládající se ze typy služeb, které zase se skládají z kód, konfigurace a balíčků. Zjednodušit diagram, jenom kód / / dat konfigurace balíčky `ServiceType4` se zobrazí, když je každý typ služby, obsahovat některé nebo všechny typy balíček.

![Typy aplikací služby struktury a typy služeb][cluster-imagestore-apptypes]

Dva různé seznamu soubory se používají k tomu, aplikací a služeb: manifest služby a manifestu aplikace. Toto jsou podrobně v následující části.

Aktivní v clusteru může být jeden nebo víc instancí služby typu. Například stavová služba instancí nebo repliky, dosáhnout vysoké spolehlivosti replikace stavu replik nachází na různých uzlů v clusteru. Tato replikace v podstatě poskytuje redundance služby je k dispozici i v případě, že jeden uzel clusteru se nezdaří. Další [oddíly služby](service-fabric-concepts-partitioning.md) rozdělí jeho stav (a vzorků přístup pro státu) mezi uzly clusteru.

Na následujícím obrázku vidíte vztah mezi aplikací a instancí služby, oddíly a repliky.

![Oddíly a repliky v rámci služby][cluster-application-instances]


>[AZURE.TIP] Zobrazení rozložení aplikací v clusteru nástrojem pro službu struktury Explorer k dispozici na http://&lt;yourclusteraddress&gt;: 19080/Průzkumníka. Další informace najdete v tématu [vizualizace svůj cluster v Průzkumníkovi struktury služby](service-fabric-visualizing-your-cluster.md).

## <a name="describe-a-service"></a>Popis služby

Manifest služby deklarativně určuje typ služby a verze. Určuje metadata služby například typ služby, Vlastnosti stavu, Vyrovnávání zatížení metriky, binární služby a konfigurace soubory.  Jinými slovy, popisuje kód, konfigurace a dat balíčků tvořící balíčku služby pro podporu alespoň jeden typ služby. Tady představuje jednoduchý příklad služby:

~~~
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="MyCode" Version="CodeVersion1">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>
  <ConfigPackage Name="MyConfig" Version="ConfigVersion1" />
  <DataPackage Name="MyData" Version="DataVersion1" />
</ServiceManifest>
~~~

**Verze** atributy nestrukturovaná řetězce a ne analyzovat systém. Toto jsou použité na verzi jednotlivé součásti upgradech.

**ServiceTypes** deklaruje, jaké typy služby **CodePackages** podporovaných v tomto manifestu. Když do služby je vytvořena proti jeden z těchto typů služby, všechny balíčky kód deklarované v tomto manifestu jsou aktivní spuštěním jejich vstupní body. Očekává se, že výsledné procesy zaregistrovat typy podporované službě za běhu. Všimněte si, že jsou typy služeb deklarovat na úroveň seznamu a ne na úrovni balíčku kód. Takže pokud existuje více balíčků kód, všechny aktivují se pokaždé, když systému vyhledá některý typ deklarované služby.

**SetupEntryPoint** je privilegovaných vstupní bod, který spustí s stejné přihlašovací údaje jako služby struktury (obvykle účet *LocalSystem* ) před další vstupní bod. Spustitelný soubor určený **vstupní bod** je obvykle hostitele služby dlouhodobě spuštěných. Informace o stavu vstupní bod samostatném nastavení zabráněno museli spouštět hostitele služby s vysokou úrovní oprávnění pro dlouhou dobu. Spustitelný soubor určený **vstupní bod** běží po **SetupEntryPoint** zavře úspěšně. Výsledný proces jsou sledovány a restartování (začínající znovu **SetupEntryPoint**), pokud vypršela jeho někdy ukončí nebo dojde k chybě.

**DataPackage** deklaruje do složky nazvané atributem **název** , obsahující libovolný statická data k tomu procesu za běhu.

**ConfigPackage** deklaruje do složky nazvané atributem **název** , který obsahuje soubor *Settings.xml* . Tento soubor obsahuje oddíly nastavení definované uživatelem, klíč hodnota dvojici psaných procesu zpět za běhu. Během upgradu pokud pouze **ConfigPackage** **verzi** změnilo, pak spuštěný proces není restartovat. Místo toho zpětné upozorněním procesu nastavení konfigurace změnily, se může být znovu načte dynamicky. Tady je příklad souboru *Settings.xml* :

~~~
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MyConfigurationSecion">
    <Parameter Name="MySettingA" Value="Example1" />
    <Parameter Name="MySettingB" Value="Example2" />
  </Section>
</Settings>
~~~

> [AZURE.NOTE] Manifest služby může obsahovat více kód, konfigurace a balíčků data. Každou z těchto může být verzí nezávisle na sobě.

<!--
For more information about other features supported by service manifests, refer to the following articles:

*TODO: LoadMetrics, PlacementConstraints, ServicePlacementPolicies
*TODO: Resources
*TODO: Health properties
*TODO: Trace filters
*TODO: Configuration overrides
-->

## <a name="describe-an-application"></a>Popisují aplikaci


Manifest aplikace deklarativně popisuje typ aplikace a verzi. Určuje metadata služby složení například stabilní názvy rozdělení schéma faktorem počet/replikace instance, zásady zabezpečení/izolace, umístění omezení, přepsání konfigurace a služby základní typy. Popsané také Vyrovnávání zatížení domény které je umístěn aplikace.

Proto manifest aplikace popisuje prvky na úrovni aplikace a odkazuje jeden nebo více manifesty služby při vytváření aplikace typu. Tady představuje jednoduchý příklad aplikace:

~~~
<?xml version="1.0" encoding="utf-8" ?>
<ApplicationManifest
      ApplicationTypeName="MyApplicationType"
      ApplicationTypeVersion="AppManifestVersion1"
      xmlns="http://schemas.microsoft.com/2011/01/fabric"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example application manifest</Description>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="MyServiceManifest" ServiceManifestVersion="SvcManifestVersion1"/>
  </ServiceManifestImport>
  <DefaultServices>
     <Service Name="MyService">
         <StatelessService ServiceTypeName="MyServiceType" InstanceCount="1">
             <SingletonPartition/>
         </StatelessService>
     </Service>
  </DefaultServices>
</ApplicationManifest>
~~~

Podobně jako služby manifesty **verze** atributy jsou nestrukturovaná řetězce a nejsou analyzovat systém. Toto jsou také použít verzi jednotlivé součásti upgradech.

**ServiceManifestImport** obsahuje odkazy na manifesty služby, které tvoří tento typ aplikace. Manifesty importovaného služby zjistit, jaké typy služeb jsou platné v rámci tohoto typu aplikace.

**DefaultServices** deklaruje instancí služby, které jsou automaticky vytvořené pokaždé, když aplikace vytvořeny oproti tento typ aplikace. Výchozí služby jsou jenom pohodlí a chovat jako normální služby, každý poté, co jste vytvořili. Upgradované spolu s dalšími službami v instance aplikace a je možné odebrat taky.

> [AZURE.NOTE] Manifest aplikace může obsahovat více seznamu Importy služby a služby výchozí. Import seznamu každé služby může být verzí nezávisle na sobě.

Naučte se spravovat jiné aplikace a služby parametrů pro jednotlivé prostředí, najdete v článku [Správa parametry aplikace na více prostředí](service-fabric-manage-multiple-environment-app-configuration.md).

<!--
For more information about other features supported by application manifests, refer to the following articles:

*TODO: Application parameters
*TODO: Security, Principals, RunAs, SecurityAccessPolicy
*TODO: Service Templates
-->

## <a name="package-an-application"></a>Balíček aplikace

### <a name="package-layout"></a>Rozložení balíčku

Manifest aplikace, služby manifest(s) a jiné soubory potřebné balíčku je nutné uspořádat určitým rozložením nasazení do služby struktury clusteru. Manifesty příklad v tomto článku jsou potřeba ke se seřazenými do následující strukturu adresářů:

~~~
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat
~~~

Složky se název odpovídá **názvu** atributy jednotlivých odpovídajících prvků. Například pokud manifest služby obsahovalo hodnotu dva balíčky kód s názvy **MyCodeA** a **MyCodeB**, pak dvěma složkami se stejnými názvy by obsahoval potřebné binární soubory pro každý balíček kódu.

### <a name="use-setupentrypoint"></a>Použití SetupEntryPoint

Obvyklé scénáře pro používání **SetupEntryPoint** jsou při potřebujete k spuštění spustitelný soubor před spuštěním služby nebo potřebujete provést operaci se zvýšenými oprávněními. Příklad:

- Nastavení a inicializace proměnné, které potřebuje spustitelný soubor služby. Toto není omezena do pouze programů jiného napsali prostřednictvím programové modely služby struktury. Například npm.exe vyžaduje některé proměnné nakonfigurován pro nasazení aplikace node.js.

- Nastavení řízení přístupu instalací certifikáty zabezpečení.

### <a name="build-a-package-by-using-visual-studio"></a>Vytvořit balíček pomocí aplikace Visual Studio

Pokud používáte k vytvoření aplikace Visual Studio 2015, můžete balíček příkaz automaticky vytvořit balíček, který odpovídá rozložení jsme je popsali výše.

Vytvoření balíčku aplikace project v okně Průzkumník pravým tlačítkem příkazem balíčku, jak je ukázáno v následujícím příkladu:

![Balení aplikace Visual Studio][vs-package-command]

Po dokončení balení bude umístění balíčku Hledat v okně **výstupu** . Všimněte si, že balení nastane automaticky při nasazení nebo ladění aplikace ve Visual Studiu.

### <a name="test-the-package"></a>Testování balíčku

Pomocí příkazu **Test ServiceFabricApplicationPackage** můžete ověřit struktury balíčku místně prostřednictvím Powershellu. Příkaz Zkontrolovat seznamu analýzu problémů a ověřte všechny odkazy. Všimněte si, že tento příkaz pouze ověří strukturální správnosti adresářů a souborů v okně balíček. Nebudou ověřte jakýkoli obsah balíčku kód nebo dat za kontrola, zda jsou k dispozici všechny potřebné soubory.

~~~
PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
False
Test-ServiceFabricApplicationPackage : The EntryPoint MySetup.bat is not found.
FileName: C:\Users\servicefabric\AppData\Local\Temp\TestApplicationPackage_7195781181\nrri205a.e2h\MyApplicationType\MyServiceManifest\ServiceManifest.xml
~~~

Tato chyba se zobrazí, že *MySetup.bat* odkazuje na manifestu služby **SetupEntryPoint** chybí soubor balíčku kód. Po přidání chybějící soubor předá ověřování aplikace:

~~~
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │       MySetup.bat
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat

PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
True
PS D:\temp>
~~~

Jakmile aplikace je součástí správně a předá ověření, je připravený na nasazení.

## <a name="next-steps"></a>Další kroky

[Nasazení a odebrání aplikací][10]

[Správa parametry aplikace na více prostředí][11]

[RunAs: Aplikaci služby struktury s jinou oprávnění][12]

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png
[vs-package-command]: ./media/service-fabric-application-model/vs-package-command.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md
