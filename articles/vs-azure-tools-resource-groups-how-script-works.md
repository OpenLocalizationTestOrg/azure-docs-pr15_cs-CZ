<properties
    pageTitle="Základní informace o skript pro nasazení projektu Azure pole Skupina zdroje | Microsoft Azure"
    description="Popisuje, jak funguje skript Powershellu v projektu nasazení Azure pole Skupina zdroje."
    services="visual-studio-online"
    documentationCenter="na"
    authors="tfitzmac"
    manager="timlt"
    editor="" />

 <tags
    ms.service="azure-resource-manager"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="07/26/2016"
    ms.author="tomfitz" />

# <a name="overview-of-the-azure-resource-group-project-deployment-script"></a>Základní informace o skript pro nasazení projektu Azure pole Skupina zdroje

Pole Skupina zdroje Azure nasazení projekty vám pomůžou fáze a nasazení souborů a jiných artefakty Azure. Při vytváření projektu nasazení Správce prostředků Azure ve Visual Studiu skript Powershellu s názvem **AzureResourceGroup.ps1 nasazení** se přidá do projektu. Toto téma obsahuje podrobné informace o možnostech tento skript a jak jej vykonat v rámci i mimo aplikaci Visual Studio.

## <a name="what-does-the-script-do"></a>Skript k čemu slouží?

Skript nasazení AzureResourceGroup.ps1 provádí dvě věci, které jsou důležité pro nasazení pracovního postupu.

- Nahrání souborů nebo artefakty potřebné pro nasazení šablony
- Nasazení šablony

První část skript odesílání souborů a artefakty nasazení a poslední rutina ve skriptu skutečně zavést šablonu. Například pokud virtuálního počítače je třeba nakonfigurovat pomocí skriptu, skript pro nasazení nejdřív bezpečně odešlou skript konfiguraci účet Azure úložiště. Díky tomu k dispozici pro správce prostředků Azure pro konfiguraci virtuální počítač během vytváření.

Vzhledem k tomu ne všechny šablony nasazení potřebovat navíc artefakty, které je potřeba nahrát, parametr s názvem *uploadArtifacts* vyhodnocen. Pokud potřebujete všechny artefakty nahrát, přepínač *uploadArtifacts* při volání skriptem. Všimněte si, že není potřeba hlavní šablona souboru a požadovaného souboru parametrů nahrát. Pouze jiných souborů, například skripty konfigurace vnořené nasazení šablon a není potřeba nahrát soubory aplikace.

## <a name="detailed-script-description"></a>Popis podrobné skriptu

Toto je popis toho, jaké Vybrat oddíly skript Powershellu Azure AzureResourceGroup.ps1 nasazení.

>[AZURE.NOTE] Tento scénář vystihuje verze 1.0 skript AzureResourceGroup.ps1 nasazení.

1.  Deklarujte parametry potřeby tak, že správce prostředků Azure nasazení projektu. Některé parametry mají výchozí hodnoty, které byly nastavené při vytvoření projektu. Můžete změnit tyto výchozí hodnoty v skript nebo přidání různých parametr hodnoty před spuštěním skript.

    ```
    Param(
      [string] [Parameter(Mandatory=$true)] $ResourceGroupLocation,
      [string] $ResourceGroupName = 'AzureResourceGroup1',
      [switch] $UploadArtifacts,
      [string] $StorageAccountName,
      [string] $StorageAccountResourceGroupName,
      [string] $StorageContainerName = $ResourceGroupName.ToLowerInvariant() + '-stageartifacts',
      [string] $TemplateFile = '..\Templates\azuredeploy.json',
      [string] $TemplateParametersFile = '..\Templates\azuredeploy.parameters.json',
      [string] $ArtifactStagingDirectory = '..\bin\Debug\staging',
      [string] $AzCopyPath = '..\Tools\AzCopy.exe',
      [string] $DSCSourceFolder = '..\DSC'
    )
    ```

  	|Parametr|Popis|
  	|---|---|
  	|$ResourceGroupLocation|Oblast nebo dat centra umístění zdroje skupiny, například **Západní USA** nebo **východní Asie**.|
  	|$ResourceGroupName|Název skupiny Azure zdrojů.|
  	|$UploadArtifacts|Binární hodnotu, která označuje, zda artefakty potřeba nahrát Azure z počítače.|
  	|$StorageAccountName|Název účtu Azure úložiště, kde jsou uložená vaše artefakty.|
  	|$StorageAccountResourceGroupName|Název skupiny Azure zdroje, který obsahuje účet úložiště.|
  	|$StorageContainerName|Název kontejneru úložiště použít pro odeslání artefakty.|
  	|$TemplateFile|Cesta k souboru nasazení (`<app name>.json`) v projektu Azure pole Skupina zdroje.|
  	|$TemplateParametersFile|Cesta k souboru parametrů (`<app name>.parameters.json`) v projektu Azure pole Skupina zdroje.|
  	|$ArtifactStagingDirectory|Cesta v počítači, kde artefakty místně odeslány, včetně kořenové složce skript Powershellu. Tato cesta lze absolutní nebo relativní umístění skriptů.|
  	|$AzCopyPath|Cesta kde nástroj AzCopy.exe kopíruje jeho zip soubory, včetně kořenové složce skript Powershellu. Tato cesta lze absolutní nebo relativní umístění skriptů.|
  	|$DSCSourceFolder|Cestu ke složce zdroj DSC (žádoucí konfigurace stavu), včetně kořenové složce skript Powershellu. Tato cesta lze absolutní nebo relativní umístění skriptů. V případě potřeby další informace najdete v článku [Úvod do rozšíření Azure prostředí PowerShell DSC (žádoucí konfigurace stavu)](http://blogs.msdn.com/b/powershell/archive/2014/08/07/introducing-the-azure-powershell-dsc-desired-state-configuration-extension.aspx).|

1.  Zkontrolujte, zda artefakty potřeba nahrát Azure. V opačném případě přejděte ke kroku 11. Jinak proveďte následující kroky.

1.  Převod všech proměnných s relativní cesty na absolutní cesty. Například změnit cestu `..\Tools\AzCopy.exe` k `C:\YourFolder\Tools\AzCopy.exe`. Navíc inicializace proměnné *ArtifactsLocationName* a *ArtifactsLocationSasTokenName* na hodnotu null. *ArtifactsLocation* a *SaSToken* může být parametry do šablony. Pokud jejich hodnoty null po čtení v souboru parametrů, skript vygeneruje hodnoty pro ně.

    Nástroje Azure pomocí hodnoty parametru *_artifactsLocation* a *_artifactsLocationSasToken* v šabloně spravovat artefakty. Pokud skript Powershellu najde parametry s názvy, ale nejsou k dispozici hodnoty parametrů, skript nahraje artefaktům a vrátí odpovídající hodnoty pro tyto parametry. Ho předá je rutinu prostřednictvím `@OptionsParameters`.

  	|Proměnná|Popis|
  	|---|---|
  	|ArtifactsLocationName|Cesta k kde Azure artefakty nacházejí.|
  	|ArtifactsLocationSasTokenName|Přidružení zabezpečení (sdílené přístup podpis) tokenu název používaný skript k ověření Bus služby. Další informace najdete v tématu [Sdílené ověřování přístupu k podpisu Bus služby](service-bus-shared-access-signature-authentication.md) .|

    ```
    if ($UploadArtifacts) {
    # Convert relative paths to absolute paths if needed
    $AzCopyPath = [System.IO.Path]::Combine($PSScriptRoot, $AzCopyPath)
    $ArtifactStagingDirectory = [System.IO.Path]::Combine($PSScriptRoot, $ArtifactStagingDirectory)
    $DSCSourceFolder = [System.IO.Path]::Combine($PSScriptRoot, $DSCSourceFolder)

    Set-Variable ArtifactsLocationName '_artifactsLocation' -Option ReadOnly
    Set-Variable ArtifactsLocationSasTokenName '_artifactsLocationSasToken' -Option ReadOnly

    $OptionalParameters.Add($ArtifactsLocationName, $null)
    $OptionalParameters.Add($ArtifactsLocationSasTokenName, $null)
    ```

1.  Tento oddíl kontroly zda <app name>. parameters.json souboru (jako takzvaný "souboru parametrů") je nadřazený uzel s názvem **parametrů** (v `else` bloku). V opačném má žádný nadřazený uzel. Formátem je přijatelná.
    
    ```
    if ($JsonParameters -eq $null) {
            $JsonParameters = $JsonContent
        }
        else {
            $JsonParameters = $JsonContent.parameters
        }
    ```

1.  Iteraci kolekci JSON parametry. Pokud hodnota parametru přiřadila *_artifactsLocation* nebo *_artifactsLocationSasToken*, nastavte variabilní *$OptionalParameters* s těmito hodnotami. To zabrání skript omylem přepsat všechny hodnoty parametrů, které zadáte.

    ```
    $JsonParameters | Get-Member -Type NoteProperty | ForEach-Object {
        $ParameterValue = $JsonParameters | Select-Object -ExpandProperty $_.Name

        if ($_.Name -eq $ArtifactsLocationName -or $_.Name -eq $ArtifactsLocationSasTokenName) {
            $OptionalParameters[$_.Name] = $ParameterValue.value
        }
    }
    ```

1.  Potřebujete klíč účtu úložiště a kontext pro zdroj účtu úložiště používá k ukládání artefakty nasazení.

    ```
    $StorageAccountKey = (Get-AzureRMStorageAccountKey -ResourceGroupName $StorageAccountResourceGroupName -Name $StorageAccountName).Key1

    $StorageAccountContext = (Get-AzureRmStorageAccount -ResourceGroupName $StorageAccountResourceGroupName -Name $StorageAccountName).Context
    ```

1.  Pokud používáte prostředí PowerShell DSC konfigurace virtuálního počítače, koncovku DSC vyžaduje artefakty v souboru jednoho zip. Ano vytvořte soubor archivu ZIP pro konfiguraci DSC. K tomuto účelu zkontrolujte, zda $DSCSourceFolder existuje. Pokud konfigurace DSC existuje, odeberte ho a pak vytvořte nový soubor s názvem dsc.zip.

    ```
    # Create DSC configuration archive
    if (Test-Path $DSCSourceFolder) {
    Add-Type -Assembly System.IO.Compression.FileSystem
        $ArchiveFile = Join-Path $ArtifactStagingDirectory "dsc.zip"
        Remove-Item -Path $ArchiveFile -ErrorAction SilentlyContinue
        [System.IO.Compression.ZipFile]::CreateFromDirectory($DSCSourceFolder, $ArchiveFile)
    }
    ```

1.  Pokud žádná cesta k Azure artefakty je k dispozici v souboru parametrů, nastavte cesty pro skript Powershellu použijte při odesílání artefakty. K tomuto účelu vytvořte cestu pomocí kombinace účtu úložiště koncový bod cestu a název kontejneru úložiště. Aktualizujte parametry souboru s touto nové cestou.

    ```
    # Generate the value for artifacts location if it is not provided in the parameter file
    $ArtifactsLocation = $OptionalParameters[$ArtifactsLocationName]
    if ($ArtifactsLocation -eq $null) {
        $ArtifactsLocation = $StorageAccountContext.BlobEndPoint + $StorageContainerName
        $OptionalParameters[$ArtifactsLocationName] = $ArtifactsLocation
    }
    ```

1.  Zkopírujte všechny soubory ze svého místního rozevírací cesty úložiště k účtu online úložiště Azure pomocí nástroje **AzCopy** (nachází ve složce **Nástroje pro** nasazení projektu Azure pole Skupina zdroje). Když se tento krok nepovede, ukončete skript od nasazení není pravděpodobně úspěšné bez požadované artefakty.

    ```
    # Use AzCopy to copy files from the local storage drop path to the storage account container
    & $AzCopyPath """$ArtifactStagingDirectory""", $ArtifactsLocation, "/DestKey:$StorageAccountKey", "/S", "/Y", "/Z:$env:LocalAppData\Microsoft\Azure\AzCopy\$ResourceGroupName"
    if ($LASTEXITCODE -ne 0) { return }
    ```

1.  Pokud token přidružení zabezpečení pro umístění artefakty není uvedena v souboru parametrů, vytvořte ho zpřístupnit dočasné jen pro čtení kontejneru online úložiště. Potom předejte token přidružení zabezpečení k cmdline jako "optionalParameter." Všimněte si, že všechny parametry předané v cmdline přednost hodnoty zadané v souboru parametrů.

    ```
    # Generate the value for artifacts location SAS token if it is not provided in the parameter file
    $ArtifactsLocationSasToken = $OptionalParameters[$ArtifactsLocationSasTokenName]
    if ($ArtifactsLocationSasToken -eq $null) {
       # Create a SAS token for the storage container - this gives temporary read-only access to the container
       $ArtifactsLocationSasToken = New-AzureStorageContainerSASToken -Container $StorageContainerName -Context $StorageAccountContext -Permission r -ExpiryTime (Get-Date).AddHours(4)
       $ArtifactsLocationSasToken = ConvertTo-SecureString $ArtifactsLocationSasToken -AsPlainText -Force
       $OptionalParameters[$ArtifactsLocationSasTokenName] = $ArtifactsLocationSasToken
    }
    ```

1.  Pokud již není existovat a zkontrolujte šablony a parametrech soubor neobsahuje žádné chybových zpráv funkce ověření zabraňující nasazení úspěšnému vytvořte skupina zdroje.

    ```
    # Create or update the resource group using the specified template file and template parameters file
    New-AzureRMResourceGroup -Name $ResourceGroupName -Location $ResourceGroupLocation -Verbose -Force -ErrorAction Stop

    Test-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroupName -TemplateFile $TemplateFile -TemplateParameterFile $TemplateParametersFile @OptionalParameters -ErrorAction Stop
    ```

1. Nakonec nasazení šablony. Tento kód vytvoří jedinečný název pro nasazení pomocí časové razítko.

    ```
    New-AzureRMResourceGroupDeployment -Name ((Get-ChildItem $TemplateFile).BaseName + '-' + ((Get-Date).ToUniversalTime()).ToString('MMdd-HHmm')) `
        -ResourceGroupName $ResourceGroupName `
        -TemplateFile $TemplateFile `
        -TemplateParameterFile $TemplateParametersFile `
        @OptionalParameters `
        -Force -Verbose
    ```

## <a name="deploy-the-resource-group"></a>Nasazení skupina zdroje

### <a name="to-deploy-the-resource-group-in-visual-studio"></a>Skupina zdroje ve Visual Studiu nasazení

1. V místní nabídce projektu Azure pole Skupina zdroje, zvolte **instalovat** > **Nové nasazení**.

    ![][0]

1. V dialogovém okně **nasadit do pole Skupina zdroje** buď zvolte existující skupiny zdrojů v poli rozevírací seznam a nasadit na vyberte ** &lt;vytvořit nový... &gt;** Vytvoření nové skupiny prostředků.

    ![][1]

1. Pokud se zobrazí výzva, zadejte název skupiny prostředků a umístění v dialogovém okně **Vytvořit skupinu zdroje** a klikněte na tlačítko **vytvořit** .

    ![][2]

1. Klikněte na tlačítko **Upravit parametry** k zobrazení dialogového okna **Upravit parametry** a zadejte všechny chybějící hodnoty parametrů.

    ![][3]

    >[AZURE.NOTE] V případě všechny požadované parametry hodnoty, toto dialogové okno automaticky se zobrazí při nasazení.

    ![][4]

1. Až to budete mít zadávat hodnoty parametrů, klikněte na tlačítko **Uložit** a potom klikněte na tlačítko **instalovat** .

    Spustí skript pro nasazení (nasazení AzureResourceGroup.ps1) a šablony, spolu s všechny artefakty nasadí Azure.

### <a name="to-deploy-the-resource-group-by-using-powershell"></a>Nasazení skupina zdroje pomocí prostředí PowerShell

Pokud budete chtít spustit skript bez použití příkazu Visual Studio nasazení a uživatelské rozhraní, v místní nabídce pro skript, vyberte **Otevřít v prostředí PowerShell ISE**.

![][5]


## <a name="command-deployment-examples"></a>Příkaz nasazení příklady

### <a name="deploy-using-default-values"></a>Nasazení pomocí výchozích hodnot

Tento příklad ukazuje, jak spustit skript pomocí výchozí hodnoty parametrů. (Protože parametr umístění nemá výchozí hodnotu, musíte zadat jeden.)

`.\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation eastus`

### <a name="deploy-overriding-the-default-values"></a>Nasazení přepsání výchozích hodnot

Tento příklad ukazuje, jak spustit skript pro nasazení šablony a parametrech soubory, které se liší od výchozí hodnoty.

```
.\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation eastus –TemplateFile ..\templates\AnotherTemplate.json –TemplateParametersFile ..\templates\AnotherTemplate.parameters.json
```

### <a name="deploy-using-uploadartifacts-for-staging"></a>Nasazení pomocí UploadArtifacts pro pracovní

Tento příklad ukazuje, jak spustit skript, který chcete nahrát artefakty ze složky vydání a nasazení jiné než výchozí šablony.

```
.\Deploy-AzureResourceGroup.ps1 -StorageAccountName 'mystorage' -StorageAccountResourceGroupName 'Default-Storage-EastUS' -ResourceGroupName 'myResourceGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\windowsvirtualmachine.json' -TemplateParametersFile '..\templates\windowsvirtualmachine.parameters.json' -UploadArtifacts -ArtifactStagingDirectory ..\bin\release\staging
```

Tento příklad ukazuje, jak spustit skript Powershellu Azure úkolu ve Visual Studiu Online.

```
$(Build.StagingDirectory)/AzureResourceGroup1/Scripts/Deploy-AzureResourceGroup.ps1 -StorageAccountName 'mystorage' -StorageAccountResourceGroupName 'Default-Storage-EastUS' -ResourceGroupName 'myResourceGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\windowsvirtualmachine.json' -TemplateParametersFile '..\templates\windowsvirtualmachine.parameters.json' -UploadArtifacts -ArtifactStagingDirectory $(Build.StagingDirectory)
```

## <a name="next-steps"></a>Další kroky
Další informace o správce prostředků Azure [Přehled Správce prostředků Azure](azure-resource-manager/resource-group-overview.md).

Další příklady práce s projekty Azure pole Skupina zdroje, najdete v článku [nasazení a správu Azure zdrojů](https://github.com/Microsoft/HealthClinic.biz/wiki/Deploy-and-manage-Azure-resources) z připojit [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 [ukázku](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). Rychlé další prohlídky z ukázku HealthClinic.biz najdete v článku [Rychlé Azure vývojář nástroje prohlídky](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).

[0]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy1c.png
[1]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy2bc.png
[2]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy3bc.png
[3]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy4bc.png
[4]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy5c.png
[5]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy6c.png
