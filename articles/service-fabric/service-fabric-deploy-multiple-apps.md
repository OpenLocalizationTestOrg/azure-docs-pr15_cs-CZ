<properties
   pageTitle="Nasazení aplikace Node.js, který používá MongoDB | Microsoft Azure"
   description="Návod o tom, jak balíček více spustitelných hosta nasazení clusteru struktury služby Azure"
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
   ms.date="10/22/2016"
   ms.author="msfussell;mikhegn"/>


# <a name="deploy-multiple-guest-executables"></a>Nasazení více spustitelných hosta

Tento článek ukazuje, jak balíček a nasazení více spustitelných hosta do struktury služby Azure. Vytvoření a nasazení jeden balíček služby struktury číst nasazení [spustitelný služby struktury Host](service-fabric-deploy-existing-app.md).

Během tohoto návodu ukazuje, jak nasadit aplikaci front-end Node.js používající MongoDB jako datový úložiště, můžete použít kroky k libovolné aplikaci, která obsahuje závislostí na jiné aplikace.   

Visual Studio můžete použít k vytvoření balíčku aplikace, která obsahuje více spustitelných Host. V tématu [Použití Visual Studio můžete provést balíček existující aplikace](service-fabric-deploy-existing-app.md#using-visual-studio-to-package-an-existing-executable). Po přidání prvního spustitelný soubor hosta klikněte pravým tlačítkem na projekt aplikace a vyberte **Přidat -> nové služby struktury služby** přidat druhý projekt spustitelný hosta řešení. Poznámka: Pokud budete chtít vytvořit odkaz na zdroj v projektu Visual Studio, vytváření řešení Visual Studio se ujistěte se, že balíčku aplikace je aktuální změny ve zdrojovém. 

## <a name="manually-package-the-multiple-guest-executable-application"></a>Ruční balíček více hosta spustitelná aplikace
Můžete taky můžete ručně sbalit spustitelný Host. Tento článek pro ruční balení používá nástroj balení struktury služby, který je k dispozici na [http://aka.ms/servicefabricpacktool](http://aka.ms/servicefabricpacktool).

### <a name="packaging-the-nodejs-application"></a>Balení Node.js aplikace
Tomto článku se předpokládá, že Node.js nainstalována uzlů v clusteru struktury služby. V důsledku budete muset přidat Node.exe kořenového adresáře aplikace uzel před obalu. Strukturu adresáře aplikace Node.js (pomocí Express webové rozhraní a Jade šablony engine) by měl vypadat podobá níže:

```
|-- NodeApplication
  	|-- bin
        |-- www
  	|-- node_modules
        |-- .bin
        |-- express
        |-- jade
        |-- etc.
  	|-- public
        |-- images
        |-- etc.
  	|-- routes
        |-- index.js
        |-- users.js
  	|-- views
        |-- index.jade
        |-- etc.
  	|-- app.js
  	|-- package.json
  	|-- node.exe
```

Dalším krokem je vytvoření balíčku aplikace pro aplikaci Node.js. Následující kód vytvoří balíček aplikace služby struktury, který obsahuje Node.js aplikaci.

```
.\ServiceFabricAppPackageUtil.exe /source:'[yourdirectory]\MyNodeApplication' /target:'[yourtargetdirectory] /appname:NodeService /exe:'node.exe' /ma:'bin/www' /AppType:NodeAppType
```

Tady je popis parametrů, které jsou použity:

- **/ zdroj** odkazuje na adresář aplikace, která by měla být balíčku.
- **target** definuje v adresáři, ve kterém má být balíček vytvořen. Tento adresář musí být liší od zdrojový adresář.
- **příkazy/appname** definuje název aplikace z existující aplikace. Je důležité pochopit to převádí název služby v manifestu a ne na název aplikace služba struktury.
- **/exe** definuje spustitelný soubor, který má struktury služby spuštění, v tomto případě `node.exe`.
- **/ma** definuje argumentu, který slouží ke spuštění spustitelný soubor. Jak Node.js není nainstalovaný, struktury služby potřeba spusťte webový server Node.js spuštěním `node.exe bin/www`.  `/ma:'bin/www'`říká nástroje balení `bin/ma` jako argument node.exe.
- **/AppType** definuje název služby struktury aplikace typu.

Pokud přejdete na adresář, který byl určené parametrem Target, si můžete prohlédnout, nástroj vytvořil plně funkční balíčku služby struktury jak je ukázáno v následujícím příkladu:

```
|--[yourtargetdirectory]
  	|-- NodeApplication
        |-- C
              |-- bin
              |-- data
              |-- node_modules
              |-- public
              |-- routes
              |-- views
              |-- app.js
              |-- package.json
              |-- node.exe
        |-- config
              |--Settings.xml
        |-- ServiceManifest.xml
  	|-- ApplicationManifest.xml
```
Generované ServiceManifest.xml má nyní oddíl, který popisuje, jak má být spuštěno Node.js webového serveru, jak je znázorněno fragment kódu:

```xml
<CodePackage Name="C" Version="1.0">
    <EntryPoint>
        <ExeHost>
            <Program>node.exe</Program>
            <Arguments>'bin/www'</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
        </ExeHost>
    </EntryPoint>
</CodePackage>
```
V tomto příkladu webový server Node.js okolním port 3000, takže byste potřebovali aktualizovat informace koncového bodu v souboru ServiceManifest.xml, jak je ukázáno v následujícím příkladu.   

```xml
<Resources>
      <Endpoints>
        <Endpoint Name="NodeServiceEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
</Resources>
```
### <a name="packaging-the-mongodb-application"></a>Balení MongoDB aplikace
Teď sbalené Node.js aplikaci můžete pokračovat a balíček MongoDB. Jak je uvedeno před, nejsou specifické pro Node.js a MongoDB kroky, které teď absolvovat. Ve skutečnosti platí pro všechny aplikace, které jsou určeny sbalí společně jako jeden služby struktury aplikace.  

Chcete-li balíček MongoDB, ujistěte se, že sbalení Mongod.exe a Mongo.exe. Obě binární soubory jsou umístěny v `bin` adresáře adresáře instalace MongoDB. Strukturu adresářů vypadá podobně jako snímek dole.

```
|-- MongoDB
  	|-- bin
        |-- mongod.exe
        |-- mongo.exe
        |-- anybinary.exe
```
Služby struktury je potřeba začínat MongoDB příkaz podobnou té níže, tak je potřeba použít `/ma` parametr při sbalení MongoDB.

```
mongod.exe --dbpath [path to data]
```
> [AZURE.NOTE] Pokud umístění datového adresáře MongoDB na místního adresáře uzel není právě v případě selhání uzlu zachovány data. Buď použijte trvalé úložiště nebo implementace MongoDB otevřené nastavit, aby se ztrátou dat.  

V prostředí PowerShell nebo příkazového prostředí nemůžeme spustit nástroj balení pomocí následujících parametrů:

```
.\ServiceFabricAppPackageUtil.exe /source: [yourdirectory]\MongoDB' /target:'[yourtargetdirectory]' /appname:MongoDB /exe:'bin\mongod.exe' /ma:'--dbpath [path to data]' /AppType:NodeAppType
```

Přidat MongoDB balíčku aplikace služby struktury, budete muset Ujistěte se, že parametr/target z pole odkazuje na stejné adresář, který již obsahuje aplikace manifest spolu s aplikací Node.js. Budete potřebovat abyste měli jistotu, že používáte se stejným názvem ApplicationType.

Pojďme přejděte v adresáři a zkontrolujte co nástroj vytvořila.

```
|--[yourtargetdirectory]
  	|-- MyNodeApplication
  	|-- MongoDB
        |-- C
            |--bin
                |-- mongod.exe
                |-- mongo.exe
                |-- etc.
        |-- config
            |--Settings.xml
        |-- ServiceManifest.xml
  	|-- ApplicationManifest.xml
```
Jak můžete vidět, nástroj přidali do nové složky, MongoDB, adresář, který obsahuje binární data MongoDB. Pokud otevřete `ApplicationManifest.xml` souboru, zobrazí se balíček nyní obsahuje Node.js aplikace a MongoDB. Následující kód zobrazí obsah manifest aplikace.

```xml
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyNodeApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MongoDB" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeService" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="MongoDBService">
         <StatelessService ServiceTypeName="MongoDB">
            <SingletonPartition />
         </StatelessService>
      </Service>
      <Service Name="NodeServiceService">
         <StatelessService ServiceTypeName="NodeService">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
</ApplicationManifest>  
```

### <a name="publishing-the-application"></a>Publikování aplikace
Posledním krokem je publikovat aplikace místní clusteru služby struktury pomocí skriptů Powershellu níže:

```
Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath '[yourtargetdirectory]' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'NodeAppType'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'NodeAppType'

New-ServiceFabricApplication -ApplicationName 'fabric:/NodeApp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0  
```

Po úspěšném publikování aplikace pro místní cluster přistupujete k aplikaci Node.js na portu, které jsme zadali do manifest služby aplikace Node.js – například http://localhost:3000.

V tomto kurzu jste viděli, jak snadno zabalit dvou stávajících aplikací jako jednu žádost struktury služby. Můžete taky naučili ho nasadit do služby struktury tak, aby se vám může hodit z některé funkce struktury služby, jako jsou dostupnost a stav systému integrace.

## <a name="next-steps"></a>Další kroky

- Další informace o nasazení kontejnery [služby a kontejnery](service-fabric-containers-overview.md) přehled
