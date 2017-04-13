<properties
   pageTitle="Publikování pro vývojáře a Test prostředí pomocí skriptů Powershellu Windows | Microsoft Azure"
   description="Naučte se používat skripty Windows Powershellu z aplikace Visual Studio publikovat vývoj a testovacím prostředí."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="using-windows-powershell-scripts-to-publish-to-dev-and-test-environments"></a>Publikování pro vývojáře a testovacím prostředí pomocí skriptů Windows Powershellu

Když vytvoříte webové aplikace ve Visual Studiu, si můžete vygenerovat skriptu prostředí Windows PowerShell, který můžete později použít k automatizaci publikování na webu Azure jako Web App v aplikaci služby Azure nebo virtuálního počítače. Můžete upravit a rozšíření skriptu prostředí Windows PowerShell v editoru Visual Studio tak, aby vyhovovaly vašim požadavkům nebo skript integrovat existující sestavení test a publikování skriptů.

Tyto skripty můžete vytvořit vlastní verzích (označovaná taky jako vývojáře a testovací prostředí) webu pro dočasné použití. Například může nastavit konkrétní verzi webu Azure virtuální počítač nebo na pracovní pozici na web a spusťte test sadu, reprodukujte chybu, otestujte oprava chyb, zkušební verze navrhovanou změnu nebo nastavení vlastní prostředí pro ukázku nebo prezentace. Po vytvoření skript, který publikuje projektu můžete znovu identickými prostředí opětovným spuštěním skript podle potřeby nebo spustit skript s vlastními sestavení webové aplikace k vytvoření vlastní prostředí pro účely testování.

## <a name="what-you-need"></a>Co byste měli

- Azure SDK 2.3 nebo novější. Další informace najdete v článku [Stažení Visual Studio](http://go.microsoft.com/fwlink/?LinkID=624384) .

Nepotřebujete SDK Azure Generovat skripty pro webové projekty. Tato funkce je určený pro web projekty, ne web role ve službě cloud services.

- Azure Powershellu 0.7.4 nebo novější. Další informace najdete v tématu [instalace a konfigurace prostředí PowerShell Azure](powershell-install-configure.md) .

- [Prostředí Windows PowerShell 3.0](http://go.microsoft.com/?linkid=9811175) nebo novější.

## <a name="additional-tools"></a>Další nástroje

Další nástroji a materiály pro práci s prostředím PowerShell ve Visual Studiu Azure rozvoje jsou k dispozici. V tématu [prostředí PowerShell Tools for Visual Studio](http://go.microsoft.com/fwlink/?LinkId=404012).

## <a name="generating-the-publish-scripts"></a>Generování skripty publikovat

Si můžete vygenerovat skripty publikovat virtuálního počítače, který je hostitelem vašeho webu po vytvoření nového projektu následujícím [postupem](./virtual-machines/virtual-machines-windows-classic-web-app-visual-studio.md). Můžete taky [Generovat publikovat skripty pro webové aplikace v aplikaci služby Azure](./app-service-web/web-sites-dotnet-get-started.md).

## <a name="scripts-that-visual-studio-generates"></a>Skripty generované Visual Studio

Visual Studio generuje řešení úrovni složku s názvem **PublishScripts** obsahující dva soubory prostředí Windows PowerShell, publikovat skript virtuální počítač nebo na webu a moduly, které obsahuje funkce, které můžete použít v skriptů. Visual Studio také vytvoří soubor ve formátu JSON, která určuje podrobnosti projektu, který nasazujete.

### <a name="windows-powershell-publish-script"></a>Prostředí Windows PowerShell publikovat skriptu

Skript publikovat obsahuje určité publikovat kroky pro nasazení na webu nebo virtuálního počítače. Visual Studio poskytuje syntaxi barevné pro vývojové prostředí Windows PowerShell. Nápověda k funkce je k dispozici a volně úpravou funkce skript tak, aby vyhovovaly vašim požadavkům pro změnu.

### <a name="windows-powershell-module"></a>Modul Windows PowerShell

Modul Windows PowerShell, který generuje aplikace Visual Studio obsahuje funkce, které používá skript publikovat. Jedná se funkce Azure Powershellu, nejsou určeny chcete upravit. Další informace najdete v tématu [instalace a konfigurace prostředí PowerShell Azure](powershell-install-configure.md) .

### <a name="json-configuration-file"></a>Konfigurační soubor JSON

Soubor JSON se vytvoří ve složce **Konfigurace** a obsahuje konfigurační data, která určuje přesně které prostředky do nasazení Azure. Je v poli Název souboru, který generuje aplikace Visual Studio projektu název WAWS-dev.json Pokud jste vytvořili web nebo projekt název OM dev.json, pokud jste vytvořili virtuálního počítače. Tady je příklad JSON konfigurační soubor, který je přihlášení vygenerované při vytvoření webu. Většina hodnoty jsou zřejmé. Název webu je generováno aplikací Azure, aby nemusí odpovídaly název vašeho projektu.

```
{
"environmentSettings": {
"webSite": {
"name": "WebApplication26632",
"location": "West US"
},
"databases": [
{
"connectionStringName": "DefaultConnection",
"databaseName": "WebApplication26632_db",
"serverName": "YourDatabaseServerName",
"user": "sqluser2",
"password": "",
"edition": "",
"size": "",
"collation": "",
"location": "West US"
}
]
}
}
```
Při vytváření virtuálních počítačů konfiguračního souboru JSON vypadat takto. Všimněte si, že do cloudové služby je vytvořen jako kontejneru virtuálního počítače. Virtuální počítač obsahuje obvykle koncové body pro web access prostřednictvím protokolu HTTP a HTTPS, jakož i koncové body pro nasazení webu, který umožňuje publikovat na webu z místního počítače, Vzdálená plocha a prostředí Windows PowerShell.

```
{
"environmentSettings": {
"cloudService": {
"name": "myusernamevm1",
"affinityGroup": "",
"location": "West US",
"virtualNetwork": "",
"subnet": "",
"availabilitySet": "",
"virtualMachine": {
"name": "myusernamevm1",
"vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Win2K8R2SP1-Datacenter-201403.01-en.us-127GB.vhd",
"size": "Small",
"user": "vmuser1",
"password": "",
"enableWebDeployExtension": true,
"endpoints": [
{
"name": "Http",
"protocol": "TCP",
"publicPort": "80",
"privatePort": "80"
},
{
"name": "Https",
"protocol": "TCP",
"publicPort": "443",
"privatePort": "443"
},
{
"name": "WebDeploy",
"protocol": "TCP",
"publicPort": "8172",
"privatePort": "8172"
},
{
"name": "Remote Desktop",
"protocol": "TCP",
"publicPort": "3389",
"privatePort": "3389"
},
{
"name": "Powershell",
"protocol": "TCP",
"publicPort": "5986",
"privatePort": "5986"
}
]
}
},
"databases": [
{
"connectionStringName": "",
"databaseName": "",
"serverName": "",
"user": "",
"password": ""
}
],
"webDeployParameters": {
"iisWebApplicationName": "Default Web Site"
}
}
}
```

Můžete upravit konfigurace JSON chcete změnit, co se stane, když spustíte skripty publikovat. `cloudService` a `virtualMachine` oddíly jsou potřeba, lze ho však odstranit `databases` oddílu Pokud ji nepotřebujete. Vlastnosti, které jsou prázdné v souboru konfigurace výchozí generovaný Visual Studio jsou volitelné. ty, které obsahují ve konfiguračního souboru výchozí hodnoty jsou potřeba.

Pokud máte web, který obsahuje více prostředí (označované jako sloty) místo jedné výrobní webu v Azure, může obsahovat název úsek název webu v souboru konfigurace JSON. Například, pokud máte web, který má s názvem **osobního webu** a úsek jej s názvem **testování** potom URI je test.cloudapp.net osobního webu, ale správný název v souboru konfigurace je mysite(test). Se dají dělat jenom Pokud na webu a sloty již existují ve vašem předplatném. Jestliže neexistují, spuštěním skript bez zadání úsek, vytvořte webu a pak vytvořte úsek [Azure klasické portál](http://go.microsoft.com/fwlink/?LinkID=213885)a poté následujícím způsobem s názvem změněné webu. Další informace o nasazení sloty pro webových aplikací web apps najdete v tématu [Nastavení pracovní prostředí pro web apps v aplikaci služby Azure](./app-service-web/web-sites-staged-publishing.md).

## <a name="how-to-run-the-publish-scripts"></a>Jak spustit skripty publikovat

Pokud spustíte mít nikdy skriptu prostředí Windows PowerShell před, musíte nejdřív nastavení zásad spuštění povolte spouštění skriptů. Toto je funkce zabezpečení zabránit uživatelům spustit skripty Windows Powershellu ohledu na jejich ohroženo malware nebo viry, které se týkají provádění skriptů.

### <a name="run-the-script"></a>Spuštění skriptu

1. Vytvořte balíček nasazení webu projektu. Balíček nasazení webu je komprimované archiv (soubor .zip) obsahující soubory, které chcete zkopírovat do virtuálního počítače nebo webu. Nasazení webu balíčků ve Visual Studiu můžete vytvořit pro libovolné webové aplikace.

![Vytvořit Web nasazení balíčku](./media/vs-azure-tools-publishing-using-powershell-scripts/IC767885.png)

Další informace najdete v tématu [jak: vytvořit balíček pro nasazení Web ve Visual Studiu](https://msdn.microsoft.com/library/dd465323.aspx). Vytvoření balíčku nasazení webu automatizovat také podle popisu v části **přizpůsobení a rozšíření skripty publikovat** dál v tomto tématu.

1. V **Okně Průzkumník**otevře místní nabídku pro skript a potom zvolte **Otevřít pomocí prostředí PowerShell ISE**.

1. Pokud poprvé, se dostanete skripty Windows Powershellu na tomto počítači, otevřete okno příkazového řádku s oprávněními správce a zadejte tento příkaz:

`Set-ExecutionPolicy RemoteSigned`

1. Přihlaste se k Azure pomocí následujícího příkazu.

`Add-AzureAccount`

Po zobrazení výzvy zadejte svoje uživatelské jméno a heslo.

Všimněte si, že při automatizaci skript tento metodou pro poskytování Azure pověření nefunguje. Místo toho použijte soubor .publishsettings pověření. Jednorázové pouze, můžete pomocí příkazu **Get-AzurePublishSettingsFile** budou moct soubor stáhnout z Azure a poté **Import AzurePublishSettingsFile** umožňuje importovat soubor. Podrobné pokyny najdete v tématu [instalace a konfigurace prostředí PowerShell Azure](powershell-install-configure.md).

1. (Volitelné) Pokud chcete vytvořit Azure zdroje, jako jsou virtuálního počítače, databáze a webu bez publikování webové aplikace, použijte příkaz **Publikovat WebApplication.ps1** s **– Konfigurace** argument nastaven na konfiguračního souboru JSON. Příkazový řádek používá k určení, které prostředky k vytvoření konfiguračního souboru JSON. Protože používá výchozí nastavení pro další argumenty příkazového řádku, vytvoří zdroje, ale není publikovat webové aplikace. Řetězec-podrobné možnost nabízí další informace o co je nového.

`Publish-WebApplication.ps1 -Verbose –Configuration C:\Path\WebProject-WAWS-dev.json`

1. Příkaz **Publikovat WebApplication.ps1** uvedených v některém z následujících příkladech skript spustit a publikovat webové aplikace. Pokud potřebujete buď přijměte výchozí nastavení pro všechny další argumenty, například název předplatného, publikovat název balíčku, pověření virtuálního počítače nebo databáze serveru údaje, můžete určit tyto parametry. Použití **– podrobné** možnost zobrazíte další informace o průběhu procesu publikování.

```
Publish-WebApplication.ps1 –Configuration C:\Path\WebProject-WAWS-dev-json `
–SubscriptionName Contoso `
-WebDeployPackage C:\Documents\Azure\ADWebApp.zip `
-DatabaseServerPassword @{Name="dbServerName";Password="adminPassword"} `
-Verbose
```

Pokud vytváříte virtuálního počítače, na příkaz vypadá takto. Tento příklad ukazuje také informace o zadání přihlašovacích údajů pro více databází. Pro virtuálních počítačích, které tyto skripty vytvořit není certifikát SSL od důvěryhodné kořenové autority. Proto budete muset používat volbu **– AllowUntrusted** .

```
Publish-WebApplication.ps1 `
-Configuration C:\Path\ADVM-VM-test.json `
-SubscriptionName Contoso `
-WebDeployPackage C:\Path\ADVM.zip `
-AllowUntrusted `
-VMPassword @{name = "vmUserName"; password = "YourPasswordHere"} `
-DatabaseServerPassword @{Name="server1";Password="adminPassword1"}, @{Name="server2";Password="adminPassword2"} `
-Verbose
```

Skript můžete vytvořit databází, ale nevytvoří databázový server. Pokud chcete vytvořit databázový server, můžete použít funkci **Nový AzureSqlDatabaseServer** v modulu Azure.

## <a name="customizing-and-extending-the-publish-scripts"></a>Přizpůsobení a rozšíření skripty publikovat

Můžete přizpůsobit skript publikovat a soubor konfigurace JSON. Funkce v modulu Windows PowerShell **AzureWebAppPublishModule.psm1** nejsou určeny chcete upravit. Pokud chcete jenom zadat jinou databázi, nebo změnit některé vlastnosti virtuálního počítače, upravte konfiguračního souboru JSON. Pokud chcete rozšířit použitelnost funkce skript, který chcete automatizovat vytváření a testování projektu, můžete používat funkce kódy v **WebApplication.ps1 publikovat**.

Pokud chcete automatizovat vytváření projektu, přidejte kód, který volá MSBuild `New-WebDeployPackage` uvedené v tomto příkladu kódu. Cesta k příkazu MSBuild se liší v závislosti na verzi aplikace Visual Studio jste nainstalovali. Získat správnou cestu, můžete pomocí funkce **Get-MSBuildCmd**, jak je vidět v tomto příkladu.

### <a name="to-automate-building-your-project"></a>K automatizaci vytváření projektu

1. Přidejte `$ProjectFile` parametrů v části globální parametrů.

```
[Parameter(Mandatory = $false)]
  [ValidateScript({Test-Path $_ -PathType Leaf})]
  [String]
  $ProjectFile,
```

1. Zkopírujte funkci `Get-MSBuildCmd` do souboru skriptu.

```
function Get-MSBuildCmd
{
        process
{

             $path =  Get-ChildItem "HKLM:\SOFTWARE\Microsoft\MSBuild\ToolsVersions\" |
                                   Sort-Object {[double]$_.PSChildName} -Descending |
                                   Select-Object -First 1 |
                                   Get-ItemProperty -Name MSBuildToolsPath |
                                   Select -ExpandProperty MSBuildToolsPath
       
            $path = (Join-Path -Path $path -ChildPath 'msbuild.exe')

        return Get-Item $path
    }
}
```

1. Nahrazení `New-WebDeployPackage` s následující kód a nahraďte zástupný text v řádku stavba `$msbuildCmd`. Tento kód je určený pro Visual Studio 2015. Pokud používáte Visual Studio 2013, změnit **VisualStudioVersion** vlastnost pod `12.0`.

```
function New-WebDeployPackage
{
    #Write a function to build and package your web application
      
#To build your web application, use MsBuild.exe. For help, see MSBuild Command-Line Reference at: http://go.microsoft.com/fwlink/?LinkId=391339
      
Write-VerboseWithTime 'Build-WebDeployPackage: Start'
      
$msbuildCmd = '"{0}" "{1}" /T:Rebuild;Package /P:VisualStudioVersion=14.0 /p:OutputPath="{2}\MSBuildOutputPath" /flp:logfile=msbuild.log,v=d' -f (Get-MSBuildCmd), $ProjectFile, $scriptDirectory
      
Write-VerboseWithTime ('Build-WebDeployPackage: ' + $msbuildCmd)
      
#Start execution of the build command
$job = Start-Process cmd.exe -ArgumentList('/C "' + $msbuildCmd + '"') -WindowStyle Normal -Wait -PassThru
      
if ($job.ExitCode -ne 0)
{
throw('MsBuild exited with an error. ExitCode:' + $job.ExitCode)
}

#Obtain the project name
$projectName = (Get-Item $ProjectFile).BaseName
      
#Construct the path to web deploy zip package
$DeployPackageDir =  '.\MSBuildOutputPath\_PublishedWebsites\{0}_Package\{0}.zip' -f $projectName
      
      
#Get the full path for the web deploy zip package. This is required for MSDeploy to work
$WebDeployPackage = Resolve-Path –LiteralPath $DeployPackageDir
      
Write-VerboseWithTime 'Build-WebDeployPackage: End'
      
return $WebDeployPackage
}
```

1. Volání `New-WebDeployPackage` funkce před tento řádek: `$Config = Read-ConfigFile $Configuration` webových aplikací nebo `$Config = Read-ConfigFile $Configuration -HasWebDeployPackage:([Bool]$WebDeployPackage)` pro virtuálních počítačích.

```
if($ProjectFile)
{
$WebDeployPackage = New-WebDeployPackage
}
```

1. Spustit vlastní skript z příkazového řádku pomocí předávání `$Project` argument, stejně jako v následujícím příkladu příkazového řádku.

```
.\Publish-WebApplicationVM.ps1 -Configuration .\Configurations\WebApplication5-VM-dev.json `
-ProjectFile ..\WebApplication5\WebApplication5.csproj `
-VMPassword @{Name="VMUser";Password="Test.123"} `
-AllowUntrusted `
-Verbose
```

K automatizaci testování aplikace, přidejte kód pro `Test-WebApplication`. Ujistěte se, že zrušte komentář řádky v **Publikovat WebApplication.ps1** , kde je nazýván těchto funkcí. Pokud neposkytnete implementace, můžete ručně vytvořit projekt s Visual Studiu a spusťte skript publikovat publikujete Azure.

## <a name="publishing-function-summary"></a>Publikování souhrn funkcí

Potřebujete pomoct u funkcí můžete použít na příkazovém řádku prostředí Windows PowerShell, použijte příkaz `Get-Help function-name`. Nápověda obsahuje parametr nápovědy a příklady. Stejný text nápovědy je také zdrojové soubory skript, **AzureWebAppPublishModule.psm1** a **WebApplication.ps1 publikovat**. Skript a nápovědu jsou lokalizované v jazyce Visual Studio.

**AzureWebAppPublishModule**

|Název funkce|Popis|
|---|---|
|Přidání AzureSQLDatabase|Vytvoří novou databázi Azure SQL.|
|Přidání AzureSQLDatabases|Z hodnoty v souboru konfigurační JSON generovaný Visual Studio vytvoří databáze Azure SQL.|
|Přidání AzureVM|Vytvoří Azure virtuální počítač a vrátí URL nasazeném OM. Funkce nastaví požadavky a pak zavolá funkci **Nový AzureVM** (modul Azure) k vytvoření nového virtuálního počítače.|
|Přidání AzureVMEndpoints|Přidá nový vstupní koncové body virtuálního počítače a vrátí virtuálního počítače s nový koncový bod.|
|Přidání AzureVMStorage|Vytvoří nový účet Azure úložiště v aktuálního předplatného. Název účtu na začátku "devtest" následovaný jedinečný alfanumerický řetězec. Funkce vrátí název nového účtu úložiště. Zadejte umístění nebo skupinu spřažení nového účtu úložiště.|
|Přidání AzureWebsite|Vytvoří web se zadaným názvem a umístěním. Tato funkce volá **AzureWebsite nové** funkce v modulu Azure. Pokud předplatné, neobsahuje už web se zadaným názvem, tato funkce vytvoří na webu a vrátí objekt webu. V ostatních případech vrátí `$null`.|
|Zálohování předplatného|Uloží aktuální Azure předplatné v `$Script:originalSubscription` proměnných v rozsahu skriptu. Tato funkce slouží k uložení aktuálního předplatného Azure (získány `Get-AzureSubscription -Current`) a jeho účtu úložiště a předplatné, které se změní tak, že tento skript (uložené v proměnné `$UserSpecifiedSubscription`) a jeho úložiště účtu, klikněte v rozsahu skriptu. Uložení hodnoty, můžete pomocí funkce, jako například `Restore-Subscription`, pokud došlo ke změně aktuální stav obnovit původní aktuálního předplatného a úložiště účtu do aktuální stav.|
|Vyhledání AzureVM|Získá zadaný Azure virtuálního počítače.|
|Formát DevTestMessageWithTime|Označuje datum a čas do zprávy. Tato funkce je určený pro zprávy napsané do datových proudů chyby a podrobné.|
|Get-AzureSQLDatabaseConnectionString|Překládá připojovací řetězec pro připojení k databázi Azure SQL.|
|Get-AzureVMStorage|Vrátí první účet úložiště vzorkem název "devtest*" (velkých písmen) do zadaného umístění nebo spřažení skupiny. Pokud "devtest*" účtu úložiště neshoduje s umístění nebo spřažení skupiny, jsou ignorovány ho. Zadejte umístění nebo skupinu spřažení.|
|Get-MSDeployCmd|Vrátí příkaz spustit nástroj MsDeploy.exe.|
|Nové AzureVMEnvironment|Najde nebo vytvoří virtuálního počítače v předplatné, vraceny hodnoty v souboru konfigurace JSON.|
|Publikování WebPackage|Použití MsDeploy.exe a webu publikování balíčku. ZIP soubor, který chcete prostředky nasadit na web. Tato funkce není generovat jakýkoli výstup. Pokud se nezdaří volání MSDeploy.exe funkci výjimku. Podrobnější výstup, použijte **-podrobného** možnost.|
|Publikování WebPackageToVM|Ověřuje hodnoty parametrů a potom volá funkci **Publikování WebPackage** .|
|ConfigFile pro čtení|Ověří JSON konfiguračního souboru a vrací tabulku hash vybraných hodnot.|
|Obnovení předplatného|Obnoví aktuálního předplatného na původní předplatné.|
|Test-AzureModule|Vrátí `$true` je-li nainstalovaný modul Azure verze 0.7.4 nebo novější. Vrátí `$false` případně modulu není nainstalovaná starší verze. Tato funkce se bez parametrů.|
|Test-AzureModuleVersion|Vrátí `$true` verzi modulu Azure-li 0.7.4 nebo novější. Vrátí `$false` případně modulu není nainstalovaná starší verze. Tato funkce se bez parametrů.|
|Test-HttpsUrl|Převede vstupní adresa URL System.Uri objekt. Vrátí `$True` Pokud je absolutní adresa URL a jeho schéma https. Vrátí `$false` adresa URL je relativní, jeho schéma není HTTPS, zda vstupní řetězec nelze převést na adresu URL.|
|Test člena|Vrátí `$true` Pokud vlastnost nebo způsob je členem objektu. V ostatních případech vrátí `$false`.|
|Zápis ErrorWithTime|Data zapisuje chybová zpráva s předponou aktuální čas. Tato funkce volá funkce **Formát DevTestMessageWithTime** připojte předstih psaní zprávy při datového proudu.|
|Zápis HostWithTime|Data zapisuje zprávu do programu hostitele (**Zapsat hostitele**) s předponou aktuální čas. Efekt psaní na hostitelském programu se liší. Většinu programů, které hostují prostředí Windows PowerShell tyto zprávy k zápisu standardního výstupu.|
|Zápis VerboseWithTime|Zapisuje podrobného zprávu s předponou aktuální čas. Protože volá **Podrobné zapsat**, zobrazí zprávy pouze při spuštění skriptu s parametrem **podrobné** nebo při nastavení předvoleb **VerbosePreference** **pokračovat**.|

**Publikování webovou aplikaci**

|Název funkce|Popis|
|---|---|
|Nové AzureWebApplicationEnvironment|Vytvoří Azure zdrojů, jako je web nebo virtuálního počítače.|
|Nové WebDeployPackage|Není tato funkce implementovaná. Přidání příkazů v této funkce můžete vytvořit projekt.|
|Publikování AzureWebApplication|Publikování webové aplikace Azure.|
|Publikování webovou aplikaci|Vytvoří a nasadí Web Apps, virtuálních počítačích, SQL databáze a úložiště účty pro projekt webové aplikace Visual Studio.|
|Test webovou aplikaci|Není tato funkce implementovaná. Přidání příkazů v této funkce můžete otestovat aplikace.|

## <a name="next-steps"></a>Další kroky

Další informace o prostředí PowerShell skriptování [skriptování používat Windows PowerShell](https://technet.microsoft.com/library/bb978526.aspx) pro čtení a najdete v článku další skriptů Powershellu Azure [Centrum skriptů](https://azure.microsoft.com/documentation/scripts/).
