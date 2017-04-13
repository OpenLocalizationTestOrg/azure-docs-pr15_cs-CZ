<properties
   pageTitle="Upgrade služeb struktury aplikace pomocí prostředí PowerShell | Microsoft Azure"
   description="Tento článek provede prostředí nasazení aplikace služby struktury, změna kódu a zavádění upgradovat pomocí prostředí PowerShell."
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


# <a name="service-fabric-application-upgrade-using-powershell"></a>Upgrade aplikace služby struktury pomocí prostředí PowerShell

> [AZURE.SELECTOR]
- [Prostředí PowerShell](service-fabric-application-upgrade-tutorial-powershell.md)
- [Visual Studio](service-fabric-application-upgrade-tutorial.md)

<br/>

Nejčastěji používané a upgrade doporučuje sledované inovace.  Azure struktury služby sleduje probíhá upgrade aplikace založené na sadu zásady stavu. Po upgradu update domain (UD) služby struktury vyhodnotí stavu aplikace a buď vytvářejí další aktualizace domény nebo neúspěšné upgrade v závislosti na zásadách stavu.

Upgrade sledovaných aplikací lze provést pomocí spravované nebo nativní rozhraní API, prostředí PowerShell nebo ZBÝVAJÍCÍ. Pokyny k provádění upgradovat pomocí aplikace Visual Studio najdete v článku [Upgrade aplikace pomocí aplikace Visual Studio](service-fabric-application-upgrade-tutorial.md).

S postupné inovace služby struktury sledovány správce aplikace konfigurace zásad hodnocení stavu, které služby struktury používá k zjistěte, jestli aplikaci není správný. Kromě toho můžete Správce konfigurace akci, která je třeba vzít při vyhodnocování stavu selže (například způsobem automatické vrácení zpět.) V této části provede sledované upgrade pro jednu vzorků SDK, která využívají Powershellu.

## <a name="step-1-build-and-deploy-the-visual-objects-sample"></a>Krok 1: Vytvoření a nasazení ukázku vizuální objekty


Vytváření a publikování aplikace tak, že pravým tlačítkem myši na projekt aplikace **VisualObjectsApplication** a vyberte příkaz **Publikovat** .  Další informace najdete v tématu [služby struktury aplikace upgrade kurz](service-fabric-application-upgrade-tutorial.md).  Můžete taky můžete Powershellu pro nasazení aplikace.

> [AZURE.NOTE] Před použitím některého z příkazů služby struktury může být v prostředí PowerShell, musíte nejdřív připojit k clusteru pomocí `Connect-ServiceFabricCluster` rutiny. Podobně předpokládá se, že clusteru je už nastavené na místním počítači. Naleznete v článku o [Nastavení služby struktury vývojové prostředí](service-fabric-get-started.md).

Po vytvoření projektu ve Visual Studiu, můžete příkazu Powershellu **ServiceFabricApplicationPackage kopírovat** zkopírujte balíčku aplikace ImageStore. Dalším krokem je k registraci aplikace modul runtime služby struktury pomocí rutiny **ServiceFabricApplicationPackage rejstříku** . Posledním krokem je s navrhováním instanci aplikace rutinu **New-ServiceFabricApplication** .  Tyto tři kroky jsou podobnou stránce pomocí položka nabídky **nasazení** ve Visual Studiu.

Teď můžete použít [Službu struktury Explorer zobrazíte clusteru a aplikace](service-fabric-visualizing-your-cluster.md). Aplikace má webové služby, kterou můžete získat v Internet Exploreru zadáním [http://localhost:8081/visualobjects](http://localhost:8081/visualobjects) do adresního řádku.  Měli byste vidět některé plovoucích objektů vizuální pohyb na obrazovce.  Kromě toho můžete **Získat ServiceFabricApplication** zkontrolovat stav aplikací.

## <a name="step-2-update-the-visual-objects-sample"></a>Krok 2: Aktualizace ukázku vizuální objekty

Může se stát, že s verzí nasazené v kroku 1, není otočení vizuální objekty. Pojďme upgrade tuto aplikaci k jednomu kde také otočit vizuální objekty.

Vyberte VisualObjects.ActorService projektu v řešení VisualObjects a otevřete soubor StatefulVisualObjectActor.cs. V tomto souboru, přejděte na požadovanou metodu `MoveObject`, poznámky, `this.State.Move()`a zrušte komentář `this.State.Move(true)`. Tato změna otočí objekt po upgradu služby.

Také je potřeba aktualizovat soubor *ServiceManifest.xml* (v části PackageRoot) projektu **VisualObjects.ActorService**. Aktualizujte *CodePackage* a service verze 2.0 a odpovídajících řádků v souboru *ServiceManifest.xml* .
Volba aplikace Visual Studio *Upravovat soubory Manifest* po klepnutí pravým tlačítkem myši na řešení proveďte změny seznamu soubor.


Po provedení změn manifest by měl vypadat takto (zvýrazněný části zobrazit změny):

```xml
<ServiceManifestName="VisualObjects.ActorService" Version="2.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">

<CodePackageName="Code" Version="2.0">
```

Teď *ApplicationManifest.xml* soubor (v rámci projektu **VisualObjects** v části Řešení **VisualObjects** ) se aktualizuje na verze 2.0 **VisualObjects.ActorService** projektu. Kromě toho verze aplikace se aktualizuje na 2.0.0.0 z 1.0.0.0. *ApplicationManifest.xml* by měl vypadat takto fragment kódu:

```xml
<ApplicationManifestxmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="VisualObjects" ApplicationTypeVersion="2.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

 <ServiceManifestRefServiceManifestName="VisualObjects.ActorService" ServiceManifestVersion="2.0" />
```


Teď sestavte projekt výběrem jenom **ActorService** projektu a klikněte pravým tlačítkem myši a výběrem možnosti **vytvořit** ve Visual Studiu. Při výběru možnosti **všechny znovu**, je třeba aktualizovat verze pro všechny projekty, protože byste změnili kód. Další, Pojďme balíček aktualizovanou aplikaci tak, že pravým tlačítkem myši na ***VisualObjectsApplication***, vybere nabídce služby struktury a **balíčku**. Tato akce vytvoří balíčku aplikace, které můžou být nasazené.  Aktualizované aplikace je připraven k nasazení.


## <a name="step-3--decide-on-health-policies-and-upgrade-parameters"></a>Krok 3: Rozhodnout o zásady stavu a upgrade parametry

Seznámení s [aplikací upgrade parametry](service-fabric-application-upgrade-parameters.md) a [proces upgradu](service-fabric-application-upgrade.md) získat dobře porozumět různých upgradu parametrů, časové limity relací a stavu kritéria použita. V tomto návodu kritérium hodnocení stavu služby hodnotu do výchozího stavu (a doporučené) hodnoty, což znamená, že všechny služby a instance by měl být _správný_ po upgradu.  

Však Pojďme zvýšit *HealthCheckStableDuration* na 60 sekund (tak, aby služby správný 20 sekund před upgrade pokračuje na další aktualizovat doménu).  Podívejme se také nastavit *UpgradeDomainTimeout* být 1200 sekund a *UpgradeTimeout* být 3000 sekund.

Nakonec taky nastavení *UpgradeFailureAction* k vrácení zpět. Tato možnost vyžaduje službu struktury návrat aplikace na předchozí verzi v případě, že nastanou jakékoli problémy během upgradu. Proto při spuštění upgradu (v kroku 4), jsou určeny následujících parametrů:

FailureAction = vrácení zpět

HealthCheckStableDurationSec = 60

UpgradeDomainTimeoutSec = 1200

UpgradeTimeout = 3000


## <a name="step-4-prepare-application-for-upgrade"></a>Krok 4: Příprava žádost o upgradu

Nyní je aplikace vytvořená a připravená k upgradu. Pokud otevřete okno prostředí PowerShell jako správce a typ **Get-ServiceFabricApplication**ho měli umožňují vědět, že má aplikace typu 1.0.0.0 **VisualObjects** po nasazení.  

Balíček aplikace je uložen v části následující relativní cestu, kde můžete nekomprimované SDK struktury služby – *Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug*. V adresáři, kde je uložena balíčku aplikace by měl najděte složku "Balíček". Zaškrtněte časová razítka a ujistěte se, že je na nejnovější verzi (budete muset změnit cesty řádně podporovat taky).

Teď Pojďme kopírování balíčku aktualizované aplikace do struktury ImageStore služby (balíčků aplikací uloženými podle struktury služby). Parametr *ApplicationPackagePathInImageStore* informuje struktury služby, kde najdou balíček aplikace. Můžeme mít umístit aktualizované aplikace "VisualObjects\_V2" pomocí následujícího příkazu (budete muset znovu řádně podporovat upravit cesty).

```powershell
Copy-ServiceFabricApplicationPackage  -ApplicationPackagePath .\Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug\Package
-ImageStoreConnectionString fabric:ImageStore   -ApplicationPackagePathInImageStore "VisualObjects\_V2"
```

Dalším krokem je k registraci tato aplikace služby struktury, které lze provést pomocí následujícího příkazu:

```powershell
Register-ServiceFabricApplicationType -ApplicationPathInImageStore "VisualObjects\_V2"
```

Pokud předchozího příkazu nemá úspěšné, bude pravděpodobně nutné znovu vytvořit všechny služby. Jak je uvedeno v kroku 2, bude pravděpodobně aktualizovat svoji WebService verzi.

## <a name="step-5-start-the-application-upgrade"></a>Krok 5: Spusťte upgrade aplikace

Teď můžeme máte všechno nastavené zahájit upgrade aplikace pomocí následujícího příkazu:

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/VisualObjects -ApplicationTypeVersion 2.0.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000   -FailureAction Rollback -Monitored
```


Název aplikace je stejná jako byl popsaných v souboru *ApplicationManifest.xml* . Služba struktury používá tento název k identifikaci začíná upgradu která aplikace. Pokud jste nastavili časové limity relací příliš krátkým, můžete narazit zpráva Chyba, problém. V části řešení potíží nebo zvětšit časové limity relací.

Jako upgradu výnosů aplikaci teď můžete sledovat pomocí Průzkumníka struktury služby nebo pomocí prostředí PowerShell následující příkaz: **Get-ServiceFabricApplicationUpgrade struktury: / VisualObjects**.

Za několik minut, stav, který jste získali pomocí příkazu předchozí prostředí PowerShell by mělo být uvedeno, že jste upgradovali všechny aktualizace domény (Dokončit). A má zjistíte, že jste zahájili vizuální objekty v okně prohlížeče otočení.

Upgrade z verze 2 verzi 3 nebo z verze 2 verze 1 jako cvičení můžete zkusit. Přesunutí z verze 2 na verze 1 je také považované za upgrade. Přehrát s časové limity relací a zásady stavu proveďte vlastní seznámit s nimi. Při nasazení Azure clusteru, třeba parametry správně nastavená. Je vhodné nastavit časové limity relací konzervativně.


## <a name="next-steps"></a>Další kroky

[Upgrade aplikace pomocí aplikace Visual Studio](service-fabric-application-upgrade-tutorial.md) vás provede upgrade aplikace pomocí aplikace Visual Studio.

Řídit, jak aplikace aktualizuje pomocí [upgrade parametry](service-fabric-application-upgrade-parameters.md).

Proveďte upgrade aplikace kompatibilní naučit používat [serializaci dat](service-fabric-application-upgrade-data-serialization.md).

Naučte se používat rozšířené funkce při upgradu aplikace odkazem na [témata, Upřesnit](service-fabric-application-upgrade-advanced.md).

Oprava běžných problémů v aplikaci inovací odkazem kroků v tématu [Upgrade aplikace Poradce při potížích](service-fabric-application-upgrade-troubleshooting.md).
