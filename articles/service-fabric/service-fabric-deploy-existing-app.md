<properties
   pageTitle="Nasazení stávající spustitelný soubor struktury služby Azure | Microsoft Azure"
   description="Informace o tom, jak balíček aplikace existující jako Host spustitelný soubor, může být nasazené na služby struktury obrázku"
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
   ms.workload="na"
   ms.date="10/22/2016"
   ms.author="msfussell;mikhegn"/>

# <a name="deploy-a-guest-executable-to-service-fabric"></a>Nasazení spustitelný Host struktury služby

Spusťte jakýkoli typ aplikaci, třeba node.js, Java nebo nativní aplikace v struktury služby Azure. Služba struktury odkazuje na tyto typy aplikací hosta aplikace.
Host spustitelných nakládá služby struktury jako příslušnosti služby. Výsledkem jsou umístěny na uzlech clusteru podle dostupnosti a jiné metriky. Tento článek popisuje, jak balíček a nasazení spustitelný Host do služby struktury clusteru pomocí aplikace Visual Studio nebo nástroj příkazového řádku.

V tomto článku najdete jsme kroky balíček spustitelný Host a nasazení do struktury služby.  

## <a name="benefits-of-running-a-guest-executable-in-service-fabric"></a>Výhody spuštění Host spustitelný v struktury služby

Existuje několik výhod, které jsou součástí systém Host spustitelný clusteru struktury služby:

- Dostupnost. Aplikace spuštěné v služby struktury jsou zpřístupnit. Služby struktury zaručuje, že jsou spuštěné instance aplikace.
- Sledování stavu. Sledování stavu služby struktury zjišťuje, pokud aplikace běží a poskytuje diagnostické informace, pokud se nepovede.   
- Správa životního cyklu aplikací. Kromě poskytuje upgrade bez výpadek služeb, struktury služba poskytuje automatické vrácení zpět předchozí verze při událost nastavit jako chybné stavu vykázaného během upgradu.    
- Hustota. Spusťte více aplikací v obrázku, které nemusí pro každé spuštění aplikace v samostatném hardwaru.


## <a name="overview-of-application-and-service-manifest-files"></a>Přehled aplikací a služeb seznamu souborů

Jako součást nasazení spustitelný Host je vhodné porozumět balení služby struktury a nasazení modelu jako popsané [model aplikace](service-fabric-application-model.md). Balení modelu služby struktury závisí na dvou souborů XML: manifesty aplikací a služeb. Definice schématu pro soubory ApplicationManifest.xml a ServiceManifest.xml se instaluje s SDK struktury služby do *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

* **Manifest aplikace** Manifest aplikace slouží k popisu aplikace. Seznam služeb, které vytváříte a ostatní parametry funkce, které se používají k definování, jak by měly nasazeny jedné nebo víc služeb, jako je třeba počet výskytů.

  V struktury služby aplikace je jednotka nasazení a upgrade. Aplikaci můžete upgradovat jako jednu jednotku, kde se spravuje potenciální selhání a potenciální vrácení zpět. Služba struktury zaručuje, že proces upgradu je buď úspěšné, nebo pokud se nezdaří upgrade nezanechá aplikace nestabilní nebo Neznámý stav.

* **Manifest služby** Manifest služby popisuje součástí služby. Zahrnuje data, třeba název a typ služby, jeho kód, konfigurace a Data. Manifest služby obsahuje taky některé další parametry, které lze použít ke konfiguraci služby po jejím nasazení.


## <a name="application-package-file-structure"></a>Struktura souboru balíčku aplikace
Abyste mohli nasadit aplikaci služby struktury, potřebuje ke sledování struktuře předdefinované adresářů. Následující příklad tuto strukturu.

```
|-- ApplicationPackageRoot
  	|-- GuestService1Pkg
        |-- Code
            |-- existingapp.exe
        |-- Config
            |-- Settings.xml
        |-- Data
        |-- ServiceManifest.xml
  	|-- ApplicationManifest.xml
```

ApplicationPackageRoot obsahuje ApplicationManifest.xml soubor, který definuje aplikace. Používá se podadresáře pro každou službu součástí aplikace má být všechny artefaktům, které služba vyžaduje – ServiceManifest.xml a obvykle následující tři adresářů:

- *Kód*. Tento adresář obsahuje kód služby.
- *Konfigurace*. Tento adresář obsahuje soubor Settings.xml (a další soubory v případě potřeby), jestli mají přístup k službě za běhu k načtení specifické nastavení.
- *Data*. Toto je další adresář k uložení další místní data, která může být nutné službu. Poznámka: Dat bude použito k ukládání pouze dočasné data. Služby struktury ne kopírovat/replikace změny datového adresáře Pokud službu musí být přemístěny – například při selhání.

Poznámka: Nemusíte vytvořit `config` a `data` adresáře, pokud je nepotřebujete.

## <a name="packaging-an-existing-executable"></a>Balení stávající spustitelný soubor

Pokud balení spustitelný soubor Host, můžete buď použít šablonu projektu Visual Studio nebo [Ruční vytvoření balíčku aplikace](#manually). Pomocí aplikace Visual Studio, struktury balíčku aplikace a seznamu soubory jsou vytvořené pomocí Průvodce nového projektu za vás.

>[AZURE.NOTE] Nejjednodušší způsob, jak balíček existující spustitelný do služby systému Windows je pomocí aplikace Visual Studio.

## <a name="using-visual-studio-to-package-an-existing-executable"></a>Zabalení stávající spustitelný soubor pomocí aplikace Visual Studio

Visual Studio poskytuje šablony služby služby struktury nasazení spustitelný Host clusteru struktury služby.

Projděte si následující kroky dokončete publikování:

1. Zvolte Soubor -> Nový projekt a vytvoření struktury aplikace služby.
2. Zvolte Host spustitelný soubor jako šablonu služby.
3. Klikněte na Procházet a vyberte složku s váš spustitelný soubor a vyplňte zbytek parametry vytvořit službu.
    - *Chování balíčku kódu* můžete nastavit zkopírujte veškerý obsah složky Visual Studio projektu, což je užitečné, když spustitelný soubor se nezmění. Pokud očekáváte spustitelný soubor chcete změnit a mají možnost vystopovat nové sestavení dynamicky, můžete připojit ke složce místo. Všimněte si, že propojené složky slouží při vytváření projekt aplikace ve Visual Studiu. Tento obsahuje odkazy na umístění zdroje z aplikace Microsoft Office project umožňující aktualizovat Host spustitelný v cíle zdroje s tyto aktualizace součástí balíčku aplikace na vytvořit.
    - *Program* - zvolte spustitelný soubor, který by měla běžet spuštění služby.
    - *Argumenty* - určete argumenty, které mají být odeslány do spustitelný soubor. Může být seznam parametrů s argumenty.
    - *WorkingFolder* - adresář pracovního procesu, který má dělat jej lze spustit. Je možné zadat tři hodnoty:
        - `CodeBase`Určuje, že pracovní adresář bude nastavit, aby adresáři kódu v okně balíček aplikace (`Code` adresáře ve struktuře soubor zobrazeny předcházejícího).
        - `CodePackage`Určuje, že pracovní adresář bude nastaven do kořenové balíčku aplikace (`GuestService1Pkg` ve struktuře soubor zobrazeny předcházejícího).
        - `Work`Určuje, že soubory jsou umístěny v podadresáře s názvem práce
4. Pojmenujte služby a klikněte na OK.
5. Pokud služby potřebuje koncový bod pro komunikaci, můžete teď přidat Protocol (protokol), portů a typ souboru ServiceManifest.xml. e.g. `<Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" UriScheme="http" PathSuffix="myapp/" Type="Input" />`.
6. Teď můžete použil funkci balíček a publikovat akce proti místní obrázku tak, že ladění řešení ve Visual Studiu. Až budete připraveni můžete publikovat aplikaci vzdálené clusteru nebo vrácení se změnami řešení ovládacího prvku zdroje.
7. Přejděte do konce tomto článku se dozvíte, jak zobrazit hosta spustitelný služba spuštěná v Průzkumníku struktury služby.

<a id="manually"></a>
## <a name="manually-packaging-and-deploying-an-existing-executable"></a>Ruční balení a nasazení stávající spustitelný soubor
Proces ručně balení hosta spustitelný soubor se podle následujících kroků:

1. Vytvoření struktury adresáře balíčku.
2. Přidání souborů kód a konfigurace aplikace.
3. Upravte soubor služby.
4. Upravte soubor aplikace.

<!--
>[AZURE.NOTE] We do provide a packaging tool that allows you to create the ApplicationPackage automatically. The tool is currently in preview. You can download it from [here](http://aka.ms/servicefabricpacktool).
-->

### <a name="create-the-package-directory-structure"></a>Vytvoření struktury adresáře balíčku
Vytvořením struktuře adresáře můžete začít, jak bylo popsáno dříve.

### <a name="add-the-applications-code-and-configuration-files"></a>Přidání souborů kód a konfigurace aplikace
Po vytvoření struktuře adresáře, můžete přidat kód aplikace a konfigurace souborů v části kód a konfigurace adresáře. Můžete taky vytvořit další adresáře nebo podadresáře ve skupinovém rámečku adresáře kód nebo konfigurace.

Služba struktury znamená xcopy obsahu v kořenovém adresáři aplikace, není k dispozici žádné předdefinované strukturu má být použita jedině vytváření dva horní adresáře, kód a nastavení. (Si můžete vybrat jiné názvy požadovaná. Další informace jsou uvedeny v následující části.)

>[AZURE.NOTE] Ujistěte se, zahrnout všechny soubory/závislostí, které potřebuje. Služba struktury slouží ke kopírování obsahu balíčku aplikace na všechny uzly clusteru kam aplikace služby nasazení. Balíček by měl obsahovat všechny kód vyžadující ke spuštění aplikace. Za předpokladu, že jsou už nainstalované závislosti nedoporučujeme.

### <a name="edit-the-service-manifest-file"></a>Upravit soubor služby
Dalším krokem je upravit služby seznamu zahrnout tyto informace:

- Název typu služby. To je ID, které služby struktury používá k identifikaci služby.
- Příkaz použít ke spuštění aplikace (ExeHost).
- Žádný skript, který je potřeba spustit nastavovaná nahoru a nakonfigurovat aplikaci (SetupEntrypoint).

Následuje příklad `ServiceManifest.xml` souboru:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="NodeApp" Version="1.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceTypes>
      <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true"/>
   </ServiceTypes>
   <CodePackage Name="code" Version="1.0.0.0">
      <SetupEntryPoint>
         <ExeHost>
             <Program>scripts\launchConfig.cmd</Program>
         </ExeHost>
      </SetupEntryPoint>
      <EntryPoint>
         <ExeHost>
            <Program>node.exe</Program>
            <Arguments>bin/www</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
         </ExeHost>
      </EntryPoint>
   </CodePackage>
   <Resources>
      <Endpoints>
         <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
   </Resources>
</ServiceManifest>
```

Projděme si různé části soubor, který byste potřebovali aktualizovat:

#### <a name="updating-the-servicetypes"></a>Aktualizace ServiceTypes

```xml
<ServiceTypes>
  <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true" />
</ServiceTypes>
```

- Můžete vybrat název, který chcete použít pro `ServiceTypeName`. Hodnota se používá v `ApplicationManifest.xml` soubor k identifikaci služby.
- Budete muset zadat `UseImplicitHost="true"`. Tenhle atribut, bude služby struktury, že služba je založená na samostatné aplikace tak, aby všechny služby struktury je potřeba udělat spuštění jako proces a sledovat její stav.

#### <a name="updating-the-codepackage"></a>Aktualizace CodePackage
CodePackage element určuje umístění (včetně verze) služby kódu.

```xml
<CodePackage Name="Code" Version="1.0.0.0">
```

`Name` Element slouží k určení názvu v adresáři v balíčku aplikace, který obsahuje kód služby. `CodePackage`má i `version` atribut. Slouží k určení verze kód – a také potenciálně lze upgrade kód služby pomocí služby struktury aplikace životního cyklu správu infrastruktury.
#### <a name="optional-updating-the-setupentrypoint"></a>Volitelné: Aktualizace SetupEntrypoint

```xml
<SetupEntryPoint>
   <ExeHost>
       <Program>scripts\launchConfig.cmd</Program>
   </ExeHost>
</SetupEntryPoint>
```
SetupEntrypoint element slouží k určení spustitelný soubor nebo dávkový soubor, který má být spuštěn předtím, než se spustí kód služby. Je volitelný krok, abyste nemusí být zahrnuta do při žádné inicializace/nastavení povinné. SetupEntryPoint se spustí pokaždé, když se nerestartuje službu.

Existuje pouze jeden SetupEntrypoint tak nastavení a konfigurace skripty muset v jedné dávce soubor seskupené, pokud aplikace instalace a konfigurace vyžaduje více skriptů. SetupEntrypoint můžete provést libovolný typ souboru – spustitelný souborů, dávku a rutinách Powershellu. Další informace najdete v tématu [Konfigurace SetupEntryPoint](service-fabric-application-runas-security.md).

V předchozím příkladu, spustí SetupEntrypoint dávkový soubor s názvem `LaunchConfig.cmd` umístěné v `scripts` podadresáře adresáři kód (za předpokladu, že WorkingFolder element nastavena na CodeBase).

#### <a name="updating-the-entrypoint"></a>Aktualizace vstupní bod

```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
  </ExeHost>
</EntryPoint>
```

`Entrypoint` Prvek soubor služby slouží k určení způsobu spuštění služby. `ExeHost` Prvek Určuje spustitelný soubor (včetně argumenty), která by měl použít ke spuštění služby.

- `Program`Určuje název spustitelný soubor, který má být provedena spuštění služby.
- `Arguments`Určuje argumenty, které mají být odeslány do spustitelný soubor. Může být seznam parametrů s argumenty.
- `WorkingFolder`Určuje adresář pracovní proces, který má dělat jej lze spustit. Je možné zadat tři hodnoty:
    - `CodeBase`Určuje, že pracovní adresář bude nastavit, aby adresáři kódu v okně balíček aplikace (`Code` adresáře v předchozím struktura souboru).
    - `CodePackage`Určuje, že pracovní adresář bude nastaven do kořenové balíčku aplikace (`GuestService1Pkg` v předchozím struktura souboru).
  - `Work`Určuje, že soubory jsou umístěny v podadresáře s názvem práce

WorkingFolder je užitečné nastavit správné pracovní adresář tak, že relativní cesty může používat aplikaci nebo inicializace skriptů.

#### <a name="updating-the-endpoints-and-registering-with-naming-service-for-communication"></a>Aktualizace koncové body a zaregistrovali služba WINS pro komunikaci

```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
</Endpoints>

```
V předchozím příkladu `Endpoint` element určuje koncové body, které aplikace můžete naslouchají relacím na. V tomto příkladu aplikace Node.js sleduje na http na portu 3000.

Kromě toho můžete požádat služby struktury tento koncový bod publikujete služba WINS tak jiných služeb, můžete zjistit adresa koncového bodu k této službě. Díky tomu vám umožní komunikovat mezi službami, které představují hosta programy.
Adresa koncového bodu publikované je formuláře `UriScheme://IPAddressOrFQDN:Port/PathSuffix`. `UriScheme`a `PathSuffix` jsou volitelné atributy. `IPAddressOrFQDN`je adresa IP nebo plně kvalifikovaný název domény uzlu tento spustitelný soubor se vloží na a počítá za vás.

V následujícím příkladu po zavedení služby v Průzkumníku struktury služby uvidíte podobně jako koncový bod `http://10.1.4.92:3000/myapp/` publikování pro instanci služby nebo pokud se jedná místního počítače vidíte `http://localhost:3000/myapp/`. 

```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000"  UriScheme="http" PathSuffix="myapp/" Type="Input" />
</Endpoints>
```
Tyto adresy s [reverzního proxy](service-fabric-reverseproxy.md) slouží ke komunikaci mezi službami.

### <a name="edit-the-application-manifest-file"></a>Upravit soubor aplikace

Po konfiguraci `Servicemanifest.xml` souboru, musíte provést změny `ApplicationManifest.xml` soubor, který chcete zajistit, aby použili správnou službu typ a název.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="NodeAppType" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
   </ServiceManifestImport>
</ApplicationManifest>
```

#### <a name="servicemanifestimport"></a>ServiceManifestImport

V `ServiceManifestImport` prvek, můžete zadat jeden nebo víc služeb, které chcete zahrnout do aplikace. Služby odkazují s `ServiceManifestName`, která určuje název adresáře místo, kam `ServiceManifest.xml` soubor nachází.

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
</ServiceManifestImport>
```

## <a name="set-up-logging"></a>Nastavení protokolování
Pro hosta spustitelných je užitečný pro neuvidíte konzoly protokoly chcete zjistit, pokud skripty aplikace a konfigurace obsahují všechny chyby.
Možné konfigurovat konzoly přesměrování `ServiceManifest.xml` soubor pomocí `ConsoleRedirection` prvek.

```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="5" FileMaxSizeInKb="2048"/>
  </ExeHost>
</EntryPoint>
```

* `ConsoleRedirection`mohou sloužit k přesměrování výstup konzoly (stdout a stderr) do adresáře pracovní tak mohou sloužit k ověření, že se během instalace nebo spuštění aplikace v clusteru služby struktury bez chyb.

    * `FileRetentionCount`Určuje, kolik se uloží v adresáři fungovat. Hodnoty 5, jako je například znamená, že protokoly pro předchozí pět spuštění jsou uložena v adresáři pracovní.
    * `FileMaxSizeInKb`Určuje maximální velikost souboru protokolu.

Protokoly se ukládají v jednom z adresáře pracovní služby. Pokud chcete určit, kde se nacházejí soubory, budete muset pomocí Průzkumníka struktury služby rozhodnout, který uzel, že se službou a které pracovní adresář je použit. Tento proces se vztahuje dál v tomto článku.

## <a name="deployment"></a>Nasazení
Posledním krokem je pro nasazení aplikace. Tento skript Powershellu ukazuje, jak nasazení aplikace clusteru místní rozvoj a začněte novou službu struktury služby.

```PowerShell

Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath 'C:\Dev\MultipleApplications' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'nodeapp'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'nodeapp'

New-ServiceFabricApplication -ApplicationName 'fabric:/nodeapp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0

New-ServiceFabricService -ApplicationName 'fabric:/nodeapp' -ServiceName 'fabric:/nodeapp/nodeappservice' -ServiceTypeName 'NodeApp' -Stateless -PartitionSchemeSingleton -InstanceCount 1

```
Služba služby struktury můžete nasazenou v různých "konfigurací." Například může být nasazené jako jednu nebo víc instancí nebo může být nasazené takovým způsobem, že je jednou instancí služby v jednotlivých uzlech clusteru struktury služby.

`InstanceCount` Parametr `New-ServiceFabricService` rutina slouží k určení, kolik instancí služby spuštěna v clusteru struktury služby. Můžete nastavit `InstanceCount` hodnoty v závislosti na typu aplikace, která jsou nasazení. Jsou dva nejběžnější scénáře:

* `InstanceCount = "1"`. V tomto případě jenom jednou instancí služby nasazenou v clusteru. Plánovač služby struktury určí, který uzel službu se má být nasazené na.

* `InstanceCount ="-1"`. V tomto případě jednou instancí služby používaný v každém uzlu clusteru struktury služby. Výsledek má jeden (a pouze jeden) instanci služby pro každý uzel clusteru.

To je užitečné konfigurace pro klientské aplikace (například ZBÝVAJÍCÍ koncový bod), protože klientské aplikace třeba "připojit" k některým uzlů v clusteru používat koncový bod. Konfigurace lze také Pokud například všechny uzly clusteru služby struktury připojeni k vyrovnávání zatížení, komunikace s klienty můžete rozvržena služby, na kterém běží ve všech uzlech clusteru.

## <a name="check-your-running-application"></a>Kontrola pracovního aplikace

V Průzkumníku služby struktury popsána uzel místo, kam se službou. V tomto příkladu běží na Uzel1:

![Pokud je spuštěná služba uzel](./media/service-fabric-deploy-existing-app/nodeappinsfx.png)

Pokud přejděte na uzel a přejděte do aplikace, zobrazit informace o základních uzel, včetně jeho umístění na disku.

![Místo na disku](./media/service-fabric-deploy-existing-app/locationondisk2.png)

Pokud přejdete do složky pomocí Průzkumníka serveru a můžete najít v adresáři pracovní složku služby protokolu jak ukazuje následující obrázek.

![Umístění protokolu](./media/service-fabric-deploy-existing-app/loglocation.png)


## <a name="next-steps"></a>Další kroky
V tomto článku se naučili hosta spustitelný soubor balíčku a ho nasadit do struktury služby. V dalším kroku můžete se podívat další obsah tohoto tématu.

- [Ukázka pro sbalení a nasazení Host spustitelný na GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/GuestExe/SimpleApplication), a odkaz na zkušební verzi nástroj balení
- [Nasazení více spustitelných hosta](service-fabric-deploy-multiple-apps.md)
- [Vytvoření první aplikace služby struktury pomocí aplikace Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md)
