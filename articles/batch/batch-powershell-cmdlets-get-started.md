<properties
   pageTitle="Začínáme s Azure dávku PowerShell | Microsoft Azure"
   description="Stručný úvod k rutiny prostředí PowerShell Azure, které můžete použít pro správu služby Azure dávku"
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="powershell"
   ms.workload="big-compute"
   ms.date="10/20/2016"
   ms.author="marsma"/>

# <a name="get-started-with-azure-batch-powershell-cmdlets"></a>Začínáme s rutiny prostředí PowerShell dávku Azure
Pomocí rutin prostředí PowerShell dávku Azure můžete provádět a skriptů spoustu stejných úloh, které lze provést pomocí rozhraní API dávku, portálu Azure a Azure rozhraní příkazového řádku (rozhraní příkazového řádku). Toto je rychlý úvod popisující rutin sloužící ke správě účtů dávku a práce se zdroji dávku například fondů, projekty a úkoly.

Úplný seznam dávku rutiny a podrobné rutina syntaxe najdete v článku [reference pro rutiny dávku Azure](https://msdn.microsoft.com/library/azure/mt125957.aspx).

Tento článek je založena na rutin v prostředí PowerShell Azure verze 3.0.0. Doporučujeme aktualizovat využít aktualizací služeb a vylepšení často Azure Powershellu.

## <a name="prerequisites"></a>Zjistit předpoklady pro

Dělat následující operace pomocí Azure Powershellu ke správě dávky prostředky.

* [Instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md)

* Spusťte rutinu **Přihlášení AzureRmAccount** připojit k vašemu předplatnému (Azure dávku rutiny pro příjemce v modulu Azure správce):

    `Login-AzureRmAccount`

* **Zaregistrovat se oboru dávku poskytovatele**. Tuto operaci pouze musí být provedena **jednou na jedno předplatné**.

    `Register-AzureRMResourceProvider -ProviderNamespace Microsoft.Batch`

## <a name="manage-batch-accounts-and-keys"></a>Správa účtů dávku a klíče

### <a name="create-a-batch-account"></a>Vytvoření dávky účtu

**Nový AzureRmBatchAccount** vytvoří účet dávku ve skupině daný zdroj. Pokud ještě nemáte skupina zdroje, vytvořte spuštěním rutinu [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/azure/mt603739.aspx) . Zadejte jednu z Azure oblastí v **umístění** parametru, například "Střed USA". Příklad:

    New-AzureRmResourceGroup –Name MyBatchResourceGroup –location "Central US"

Vytvořte účet dávku ve skupinovém rámečku zdroj zadání názvu účtu v <*account_name*> umístění a název skupiny zdrojů. Vytvoření účtu dávky může chvíli trvat dokončete. Příklad:

    New-AzureRmBatchAccount –AccountName <account_name> –Location "Central US" –ResourceGroupName <res_group_name>

> [AZURE.NOTE] Název účtu listu musí být jedinečný Azure oblastí skupiny prostředků obsahovat 3 až 24 znaků a použít pouze malá písmena a číslice.

### <a name="get-account-access-keys"></a>Získání účtu přístupových kláves
**Get-AzureRmBatchAccountKeys** zobrazuje přístupových kláves přidružené k účtu dávku Azure. Například spusťte následující získat klávesy primárních a sekundárních účtu, který jste vytvořili.

    $Account = Get-AzureRmBatchAccountKeys –AccountName <account_name>

    $Account.PrimaryAccountKey

    $Account.SecondaryAccountKey

### <a name="generate-a-new-access-key"></a>Generovat nové přístupová klávesa
**Nový AzureRmBatchAccountKey** vygeneruje nové klíč účtu primární a sekundární pro účet Azure dávku. Generovat nové primární klíč účtu dávku, zadejte například:

    New-AzureRmBatchAccountKey -AccountName <account_name> -KeyType Primary

> [AZURE.NOTE] Generovat nové sekundární klíč, zadejte "Sekundární" pro parametr **Typ klíče** . Je potřeba obnovit klíče primárních a sekundárních samostatně.

### <a name="delete-a-batch-account"></a>Odstranění dávky účtu
**Odebrat AzureRmBatchAccount** odstraní dávku účet. Příklad:

    Remove-AzureRmBatchAccount -AccountName <account_name>

Po zobrazení výzvy, potvrďte, že chcete odebrat účet. Odebrání účtu může chvíli trvat dokončete.

## <a name="create-a-batchaccountcontext-object"></a>Vytvoření objektu BatchAccountContext

Ověření pomocí rutin prostředí PowerShell dávku při vytváření a správa dávku fondů projekty, úkoly a dalších zdrojů, vytvořte nejdřív objekt BatchAccountContext pro ukládání název účtu a klíče:

    $context = Get-AzureRmBatchAccountKeys -AccountName <account_name>

Předání BatchAccountContext objektu do rutinách použít parametr **BatchContext** .

> [AZURE.NOTE] Ve výchozím nastavení účtu primárního klíče slouží k ověření, ale můžete explicitně vybrat klíč, který používá změnou vlastnosti objektu BatchAccountContext **KeyInUse** : `$context.KeyInUse = "Secondary"`.

## <a name="create-and-modify-batch-resources"></a>Vytváření a změny dávky prostředky
Vytvoření zdrojů v části účet dávku pomocí rutin například **Nový AzureBatchPool**, **Nový AzureBatchJob**a **Nový AzureBatchTask** . Jsou odpovídající **Get -** a rutiny **Set -** aktualizovat vlastnosti existujících zdrojů a **Odebrat -** rutin zredukujte zdroje pod účtem dávku.

Při použití spoustu tyto rutiny, kromě předávání BatchContext objekt, budete muset vytvořit nebo předání objektů, které obsahují podrobné zdroje nastavení, jak je vidět v následujícím příkladu. V tématu podrobnou nápovědu pro každý rutinu Další příklady.

### <a name="create-a-batch-pool"></a>Vytvoření fondu dávku

Při vytváření nebo aktualizaci fond dávku, vyberete konfiguraci služby cloudu nebo konfiguraci virtuálního počítače pro operační systém v jednotlivých uzlech výpočetním (viz [Přehled funkcí dávku](batch-api-basics.md#pool)). Výběr určí, jestli uzly výpočetním jsou s obrázky s některým z [Azure hostovaného OS uvolní](../cloud-services/cloud-services-guestos-update-matrix.md#releases) nebo jeden z podporovaných Linux nebo Windows OM obrázků v Azure Marketplace.

Při spuštění **Nové AzureBatchPool**předáte nastavení operačního systému PSCloudServiceConfiguration nebo PSVirtualMachineConfiguration objektu. Například následující rutinu vytvoří nový fond dávku s velikost malé výpočetním uzlů v konfiguraci služby cloudu s obrázky v rámci nejnovější verze operačního systému rodiny 3 (Windows Server 2012). Tady **CloudServiceConfiguration** parametr určuje proměnnou *$configuration* jako objekt PSCloudServiceConfiguration. Parametr **BatchContext** určuje dříve definované proměnné *$context* jako objekt BatchAccountContext.

    $configuration = New-Object -TypeName "Microsoft.Azure.Commands.Batch.Models.PSCloudServiceConfiguration" -ArgumentList @(4,"*")

    New-AzureBatchPool -Id "AutoScalePool" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -AutoScaleFormula '$TargetDedicated=4;' -BatchContext $context

Cílové počtu uzlů výpočetním v nové fondu je určený podle vzorce neobsahovaly text. V tomto případě vzorec je jednoduše **$TargetDedicated = 4**, udává počet výpočetním uzlů v fondu je 4 nejvíce.

## <a name="query-for-pools-jobs-tasks-and-other-details"></a>Dotaz pro fondy, projekty, úkoly a další podrobnosti

Pomocí rutin například **Get-AzureBatchPool**, **Get-AzureBatchJob**a **Get-AzureBatchTask** dotazu pro entity vytvořené v části účet dávku.

### <a name="query-for-data"></a>Dotaz na data

Jako příklad použití **Get-AzureBatchPools** k nalezení vaší fondů. Ve výchozím nastavení této dotazů pro všechny fondy pod svým účtem, za předpokladu, že jste již uloženy BatchAccountContext objekt v *$context*:

    Get-AzureBatchPool -BatchContext $context

### <a name="use-an-odata-filter"></a>Použití filtru protokolu OData

Můžete zadat filtr OData pomocí **filtru** parametru zobrazíte jenom objekty, které vás zajímá. Všechny fondy můžete například najít pomocí ID od "myPool":

    $filter = "startswith(id,'myPool')"

    Get-AzureBatchPool -Filter $filter -BatchContext $context

Tento způsob je jako flexibilní, jako je použití "Where-Object" v místní kanálu. Však dotaz se odešlou do služby dávku přímo tak, aby všechny filtry se stane na straně serveru, ukládání šířky pásma Internetu.

### <a name="use-the-id-parameter"></a>Použití parametru ID.

Alternativy filtru protokolu OData se má použít parametr **Id** . K vytvoření dotazu pro fond konkrétních s id "myPool":

    Get-AzureBatchPool -Id "myPool" -BatchContext $context

Parametr **Id** podporuje pouze celé id vyhledávání, není zástupné znaky nebo styl OData filtry.

### <a name="use-the-maxcount-parameter"></a>Použít parametr MaxCount

Ve výchozím nastavení jednotlivých rutina vrací než 1000 objektů. Pokud dosažení maximálního počtu upřesnění filtru můžete získat zpět méně objektů nebo výslovně nastavené maximálně pomocí parametru **MaxCount** . Příklad:

    Get-AzureBatchTask -MaxCount 2500 -BatchContext $context

Pokud chcete odebrat horní mez, nastavte **MaxCount** na 0 nebo méně.

### <a name="use-the-powershell-pipeline"></a>Použití Powershellu kanálu

Rutiny pro dávku můžete využít prostředí PowerShell kanálu odeslat data mezi rutiny. Má stejně jako při zadání parametru, ale usnadňuje práci s několika entity.

Příklad hledání a zobrazit všechny úkoly v rámci vašeho účtu:

    Get-AzureBatchJob -BatchContext $context | Get-AzureBatchTask -BatchContext $context

Restartování (restartování) každé výpočetní uzly do fondu:

    Get-AzureBatchComputeNode -PoolId "myPool" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

## <a name="application-package-management"></a>Správa aplikací balíčku

Balíčků aplikací lze zjednodušené nasazení aplikace do výpočetního uzlů v vaší fondů. Pomocí rutin prostředí PowerShell dávku můžete nahrát a Správa balíčků aplikací ve vašem účtu dávku a nasazení balíčku verze pro výpočet uzlů.

**Vytvoření** aplikace:

    New-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

**Přidat** balíčku aplikace:

    New-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0" -Format zip -FilePath package001.zip

Nastavení **Výchozí verze** aplikace:

    Set-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -DefaultVersion "1.0"

**Seznam** balíčky aplikace

    $application = Get-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

    $application.ApplicationPackages

**Odstranění** balíčku aplikace

    Remove-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0"

**Odstranění** aplikace

    Remove-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

>[AZURE.NOTE] Před odstraněním aplikace je potřeba odstranit všechny verze aplikace balíček aplikace. Bude chybová "Konflikt" při pokusu o odstranění aplikace, který je právě balíčků aplikací.

### <a name="deploy-an-application-package"></a>Nasazení balíčku aplikace

Při vytváření fond můžete zadat jeden nebo více balíčků aplikací pro nasazení. Při zadávání balíčku času vytvoření fondu ho jako fondu spojení uzel používaný jednotlivých uzlech. Balíčky jsou také nasazeny po restartování nebo reimaged uzel.

Určení `-ApplicationPackageReference` při vytváření fond nasazení balíčku aplikace do fondu uzly při připojování fondu parametr. Nejdřív vytvořte objekt **PSApplicationPackageReference** a nakonfigurovat Id a balíčku verze aplikace, kterou chcete nasadit do fondu výpočetním uzly:

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "1.0"

Nyní vytvořte fondu a určete objekt odkaz balíček jako argument `ApplicationPackageReferences` možnost:

    New-AzureBatchPool -Id "PoolWithAppPackage" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -BatchContext $context -ApplicationPackageReferences $appPackageReference

Další informace o balíčků aplikací najdete v [nasazení aplikace s Azure dávku balíčků aplikací](batch-application-packages.md).

>[AZURE.IMPORTANT] [Odkaz účet Azure úložiště](#linked-storage-account-autostorage) , musíte ke svému účtu dávku používat balíčků aplikací.

### <a name="update-a-pools-application-packages"></a>Aktualizace fondu a balíčků aplikací

Aktualizace aplikací přiřazené k existující fond, vytvořte nejdřív PSApplicationPackageReference objekt s požadované vlastnosti (Id a balíčku verze aplikace):

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "2.0"

Pak získat fondu listu vymažte existujících balíčků, náš nový odkaz balíčku a přidejte tuto službu dávku aktualizovat nové nastavení fondu:

    $pool = Get-AzureBatchPool -BatchContext $context -Id "PoolWithAppPackage"

    $pool.ApplicationPackageReferences.Clear()

    $pool.ApplicationPackageReferences.Add($appPackageReference)

    Set-AzureBatchPool -BatchContext $context -Pool $pool

Teď jste aktualizovali do fondu vlastnosti ve službě dávku. Skutečně nasadit nové balíčku aplikace pro výpočet uzlů v fondu, ale musíte restartovat nebo reimage tyto uzly. Restartujte všechny uzly do fondu pomocí tohoto příkazu:

    Get-AzureBatchComputeNode -PoolId "PoolWithAppPackage" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

>[AZURE.TIP] Do výpočetního uzlů v fond nástroje můžete nasazovat více balíčků aplikací. Pokud chcete *Přidat* balíčku aplikace místo nahrazení aktuálně nasazeném balíčků, nevytvářejte `$pool.ApplicationPackageReferences.Clear()` řádku nad.

## <a name="next-steps"></a>Další kroky

* Podrobné rutina syntaxe a příklady najdete v tématu [reference pro rutiny dávku Azure](https://msdn.microsoft.com/library/azure/mt125957.aspx).

* Další informace o aplikacích a balíčků aplikací v listu najdete v článku [nasazení aplikace s Azure dávku balíčků aplikací](batch-application-packages.md).
