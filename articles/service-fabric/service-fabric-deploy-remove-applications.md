<properties
   pageTitle="Nasazení aplikace služby struktury | Microsoft Azure"
   description="Jak nasazení a odebrání aplikací v struktury služby"
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>

# <a name="deploy-and-remove-applications-using-powershell"></a>Nasazení a odebrání aplikací pomocí prostředí PowerShell

> [AZURE.SELECTOR]
- [Prostředí PowerShell](service-fabric-deploy-remove-applications.md)
- [Visual Studio](service-fabric-publish-app-remote-cluster.md)

<br/>

Jednou [vytvořen balíček aplikace typu][10], je připraven k nasazení do struktury služby Azure clusteru. Nasazení zahrnuje následující tři kroky:

1. Nahrání balíčku aplikace
2. Registrace aplikace typu
3. Vytvoření instance aplikace

>[AZURE.NOTE] Pokud používáte Visual Studio pro nasazení a ladění aplikací na svůj cluster místní vývoj, všechny následující kroky, jsou zpracovány automaticky prostřednictvím skript Powershellu najdete ve složce skripty projektu aplikace. Tento článek obsahuje pozadí na co tyto skripty na projektu vedou tak, aby bylo možné provádět stejné operace mimo Visual Studio.

## <a name="upload-the-application-package"></a>Nahrání balíčku aplikace

Nahrání balíčku aplikace umístí do umístění, do kterého přístupný součástmi služby struktury vnitřní. Pomocí prostředí PowerShell provádět nahrávání. Před spuštěním všechny příkazy Powershellu v tomto článku vždy s navrhováním [ServiceFabricCluster připojit](https://msdn.microsoft.com/library/mt125938.aspx) se připojit k obrázku struktury služby.

Předpokládejme, že máte do složky nazvané *MyApplicationType* obsahující manifestu potřebné aplikace, služby manifesty a balíčků kód / / dat konfigurace. Příkaz [Kopírovat ServiceFabricApplicationPackage](https://msdn.microsoft.com/library/mt125905.aspx) odešlou balíček clusteru obrázek úložiště. Rutinu **Get-ImageStoreConnectionStringFromClusterManifest** , která je součástí modulu služby struktury SDK PowerShell, se používá k získání připojovacího řetězce obrázek úložiště přihlašovacích údajů.  Pokud chcete importovat modulu SDK, spusťte:

```
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

Balíčku aplikace z *2015\Projects\MyApplication\myapplication\pkg\debug C:\users\ryanwi\Documents\Visual Studio* můžete zkopírovat do *c:\temp\MyApplicationType* (Přejmenovat adresáři "ladění" na "MyApplicationType"). Následující příklad odešle balíček:

~~~
PS C:\temp> dir

    Directory: c:\temp


Mode                LastWriteTime         Length Name                                                                                   
----                -------------         ------ ----                                                                                   
d-----        8/12/2016  10:23 AM                MyApplicationType                                                                          

PS C:\temp> tree /f .\Stateless1Pkg

C:\TEMP\MyApplicationType
│   ApplicationManifest.xml
|
└───Stateless1Pkg
  	|   ServiceManifest.xml
    │
    └───Code
    │   │  Microsoft.ServiceFabric.Data.dll
    │   │  Microsoft.ServiceFabric.Data.Interfaces.dll
    │   │  Microsoft.ServiceFabric.Internal.dll
    │   │  Microsoft.ServiceFabric.Internal.Strings.dll
    │   │  Microsoft.ServiceFabric.Services.dll
    │   │  ServiceFabricServiceModel.dll
    │   │  MyService.exe
    │   │  MyService.exe.config
    │   │  MyService.pdb
    │   │  System.Fabric.dll
    │   │  System.Fabric.Strings.dll
    │   │
    │   └───en-us
  	|         Microsoft.ServiceFabric.Internal.Strings.resources.dll
  	|         System.Fabric.Strings.resources.dll
  	|
    ├───Config
    │     Settings.xml
    │
    └───Data
          init.dat

PS C:\temp> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath MyApplicationType -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest))
Copy application package succeeded

PS D:\temp>
~~~

## <a name="register-the-application-package"></a>Registraci balíčku aplikace

Typ aplikace a verzi deklarované v manifest aplikace dostupné pro použití registraci balíčku aplikace díky. Systém přečte balíček uložit v předchozím kroku, ověřte balíček (shodný s obsahem spuštěn [Test ServiceFabricApplicationPackage](https://msdn.microsoft.com/library/mt125950.aspx) místně), zpracování obsahu balíčku a zkopírujte balíček zpracovaných do polohy vnitřní systém.

~~~
PS D:\temp> Register-ServiceFabricApplicationType MyApplicationType
Register application type succeeded

PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
DefaultParameters      : {}

PS D:\temp>
~~~

Příkaz [Register ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125958.aspx) vrátí až po systému úspěšně zkopíroval balíček aplikace. Jak dlouho to trvat závisí na obsah balíček aplikace. V případě potřeby **- TimeoutSec** parametru lze zadat delší časový limit. (Výchozí časový limit je 60 sekund.)

Příkaz [Get-ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125871.aspx) uvádí všechny verze typ úspěšně registrovaných aplikace.

## <a name="create-the-application"></a>Vytvoření aplikace

Můžete vytvořit instanci aplikace pomocí libovolné verze aplikace typu registrované úspěšně pomocí příkazu [Nový ServiceFabricApplication](https://msdn.microsoft.com/library/mt125913.aspx) . Název každého aplikace musí začínat *struktury:* schématu a být jedinečné pro každou instanci aplikace. Jakékoli výchozí služby podle manifestu aplikace typ cílové aplikace vytvořené v současné době.

~~~
PS D:\temp> New-ServiceFabricApplication fabric:/MyApp MyApplicationType AppManifestVersion1

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
ApplicationParameters  : {}

PS D:\temp> Get-ServiceFabricApplication  

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
ApplicationStatus      : Ready
HealthState            : OK
ApplicationParameters  : {}

PS D:\temp> Get-ServiceFabricApplication | Get-ServiceFabricService

ServiceName            : fabric:/MyApp/MyService
ServiceKind            : Stateless
ServiceTypeName        : MyServiceType
IsServiceGroup         : False
ServiceManifestVersion : SvcManifestVersion1
ServiceStatus          : Active
HealthState            : Ok

PS D:\temp>
~~~

Příkaz [Get-ServiceFabricApplication](https://msdn.microsoft.com/library/mt163515.aspx) vypíše všechny instance aplikace, které byly úspěšně vytvořené spolu s jejich celkový stav.

Příkaz [Get-ServiceFabricService](https://msdn.microsoft.com/library/mt125889.aspx) vypíše všechny instance služby, které byly úspěšně vytvořené v rámci instance dané aplikace. Tady jsou vypsané výchozí služby (pokud existuje).

Více instancí aplikace lze vytvářet pro dané verzi typ registrovaných aplikace. Jednotlivé instance aplikace běží izolace s vlastním práce adresář a proces.

## <a name="remove-an-application"></a>Odebrání aplikace

Pokud již není potřeba instance aplikace, můžete ho odebrat trvale pomocí příkazu [Odebrat ServiceFabricApplication](https://msdn.microsoft.com/library/mt125914.aspx) . Příkaz automaticky odebere všechny služby patřící do aplikace také trvale odebrat všechny stav služby. Tuto operaci nejde vrátit zpět a nelze ji obnovit stav aplikace.

~~~
PS D:\temp> Remove-ServiceFabricApplication fabric:/MyApp

Confirm
Continue with this operation?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
Remove application instance succeeded

PS D:\temp> Get-ServiceFabricApplication
PS D:\temp>
~~~

Pokud již není potřeba konkrétní verzi typ aplikace, by měl zrušení registrace pomocí příkazu [Unregister ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125885.aspx) . Registrace nepoužitý typy vydání úložného prostoru používaný obsah balíčku aplikace tohoto typu v úložišti obrázek. Typ aplikace možné tak dlouho, dokud žádné aplikace jsou vytvořeny oproti ho a ne čeká na vyřízení aplikace upgrady odkazují ho.

~~~
PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v1
DefaultParameters      : {}

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v2
DefaultParameters      : {}

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
DefaultParameters      : {}

PS D:\temp> Unregister-ServiceFabricApplicationType MyApplicationType AppManifestVersion1
Unregister application type succeeded

PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v1
DefaultParameters      : {}

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v2
DefaultParameters      : {}

PS D:\temp>
~~~

## <a name="troubleshooting"></a>Řešení potíží

### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a>Kopírovat ServiceFabricApplicationPackage zeptá ImageStoreConnectionString

Prostředí služby struktury SDK už měli správné výchozích hodnot nastavení. Ale v případě potřeby ImageStoreConnectionString pro všechny příkazy musí odpovídat hodnota, která používá clusteru struktury služby. Můžete najít v manifest clusteru načtené pomocí příkazu [Get-ServiceFabricClusterManifest](https://msdn.microsoft.com/library/mt126024.aspx) :

~~~
PS D:\temp> Copy-ServiceFabricApplicationPackage .\MyApplicationType

cmdlet Copy-ServiceFabricApplicationPackage at command pipeline position 1
Supply values for the following parameters:
ImageStoreConnectionString:

PS D:\temp> Get-ServiceFabricClusterManifest
<ClusterManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]

PS D:\temp> Copy-ServiceFabricApplicationPackage .\MyApplicationType -ImageStoreConnectionString file:D:\ServiceFabric\Data\ImageStore
Copy application package succeeded

PS D:\temp>
~~~

## <a name="next-steps"></a>Další kroky

[Upgrade aplikace služby struktury](service-fabric-application-upgrade.md)

[Úvod stavu služby struktury](service-fabric-health-introduction.md)

[Diagnostika a řešení potíží s služby struktury služby](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Vytváření modelů aplikace v struktury služby](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-application-model.md
[11]: service-fabric-application-upgrade.md
