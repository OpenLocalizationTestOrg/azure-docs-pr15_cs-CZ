<properties
    pageTitle="Azure Powershellu službou Azure úložiště | Microsoft Azure"
    description="Informace o používání rutin prostředí PowerShell Azure Azure úložištěm můžete vytvořit a spravovat úložiště účty; Práce s objekty BLOB, tabulek, dotazů a soubory. Konfigurace dotazu úložiště analýzy a vytvoření podpisy sdílený přístup."
    services="storage"
    documentationCenter="na"
    authors="robinsh"
    manager="carmonm"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>

# <a name="using-azure-powershell-with-azure-storage"></a>Azure Powershellu službou Azure úložiště

## <a name="overview"></a>Základní informace

Azure Powershellu je moduly, které poskytuje rutiny pro správu Azure přes Windows PowerShell. Je založeným na úkolech příkazovém řádku prostředí a skriptovacího jazyka navržený zejména pro správce systému. Pomocí prostředí PowerShell můžete snadno ovládat a automatizovat správu Azure služeb a aplikací. Můžete třeba rutin k provádění úloh, které můžete provádět prostřednictvím [Portálu Azure](https://portal.azure.com).

V této příručce jsme budete zjistit, jak provádět různé vývoj a Správa úkolů s úložištěm Azure pomocí [Rutiny úložiště Azure](https://msdn.microsoft.com/library/azure/mt269418.aspx) .

Tato příručka předpokládá, že máte zkušenosti pomocí [Azure úložiště](https://azure.microsoft.com/documentation/services/storage/) a [Prostředí Windows PowerShell](http://technet.microsoft.com/library/bb978526.aspx). Průvodce přináší celou řadu skripty, které demonstrují použití Powershellu s úložištěm Azure. Měli byste aktualizovat proměnné skriptu na základě vaší konfigurace před spuštěním každý skript.

První oddíl v této příručce obsahuje stručný přehled Azure úložiště a Powershellu. Podrobné informace a pokyny spusťte ze [požadavcích na používání Azure Powershellu s úložištěm Azure](#prerequisites-for-using-azure-powershell-with-azure-storage).


## <a name="getting-started-with-azure-storage-and-powershell-in-5-minutes"></a>Začínáme s Azure úložiště a Powershellu v 5 minut

V této části se dozvíte, jak získat přístup k úložišti Azure pomocí prostředí PowerShell v 5 minut.

**Začínáte s Azure:** Pokud potřebujete předplatné Microsoft Azure a účet Microsoft spojený s, které předplatné. Informace o možnostech Azure nákupu najdete v článku [Bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/) [Možnostmi nákupu](https://azure.microsoft.com/pricing/purchase-options/)a [Nabízí člena](https://azure.microsoft.com/pricing/member-offers/) (u členů MSDN, Microsoft Partner Network a BizSpark a dalších aplikacích Microsoftu).

Další informace o předplatném najdete Azure naleznete v tématu [přiřazení rolí Správce služby Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) .

**Po vytvoření účtu a Microsoft Azure předplatného:**

1.  Stáhněte a nainstalujte [Prostředí PowerShell Azure](http://go.microsoft.com/?linkid=9811175&clcid=0x409).
2.  Start systému Windows PowerShell integrované skriptování prostředí (ISE): V místním počítači, přejděte do nabídky **Start** . Zadejte **Nástroje pro správu** a kliknutím ho spusťte. V okně **Nástroje pro správu** klikněte pravým tlačítkem myši **ISE v prostředí Windows PowerShell**, klikněte na **Spustit jako správce**.
3.  Ve **Windows PowerShell ISE**, klikněte na **soubor** > **Nový** vytvoříte nový soubor skriptu.
4.  Teď můžeme se vám jednoduchý skript, který ukazuje základní příkazy Powershellu pro přístup k úložišti Azure. Skript nejdřív vyzve vaší Azure přihlašovací údaje účtu přidat váš Azure účtu do místního prostředí PowerShell. Potom bude skript nastavit výchozí Azure předplatné a vytvoření nového účtu úložiště v Azure. Pak skript vytvoříte nové kontejneru do tohoto nového účtu úložiště a nahrát existující soubor obrázku (blob), které obsahují. Po skript uvádí všechny objekty BLOB v tomto kontejneru, bude v místním počítači vytvořit novou cílovou složku a stažení souboru obrázku.
5.  V části následující kód vyberte mezi poznámky skript **#begin** a **#end**. Stiskněte kombinaci kláves CTRL + C je zkopírujte do schránky.

        #begin
        # Update with the name of your subscription.
        $SubscriptionName = "YourSubscriptionName"

        # Give a name to your new storage account. It must be lowercase!
        $StorageAccountName = "yourstorageaccountname"

        # Choose "West US" as an example.
        $Location = "West US"

        # Give a name to your new container.
        $ContainerName = "imagecontainer"

        # Have an image file and a source directory in your local computer.
        $ImageToUpload = "C:\Images\HelloWorld.png"

        # A destination directory in your local computer.
        $DestinationFolder = "C:\DownloadImages"

        # Add your Azure account to the local PowerShell environment.
        Add-AzureAccount

        # Set a default Azure subscription.
        Select-AzureSubscription -SubscriptionName $SubscriptionName –Default

        # Create a new storage account.
        New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $Location

        # Set a default storage account.
        Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName

        # Create a new container.
        New-AzureStorageContainer -Name $ContainerName -Permission Off

        # Upload a blob into a container.
        Set-AzureStorageBlobContent -Container $ContainerName -File $ImageToUpload

        # List all blobs in a container.
        Get-AzureStorageBlob -Container $ContainerName

        # Download blobs from the container:
        # Get a reference to a list of all blobs in a container.
        $blobs = Get-AzureStorageBlob -Container $ContainerName

        # Create the destination directory.
        New-Item -Path $DestinationFolder -ItemType Directory -Force  

        # Download blobs into the local destination directory.
        $blobs | Get-AzureStorageBlobContent –Destination $DestinationFolder
        #end

5.  Ve **Windows PowerShell ISE**stisknutím kláves CTRL + V zkopírujte skript. Klikněte na **soubor** > **Uložit**. V dialogovém okně **Uložit jako** zadejte název souboru skript, třeba "mystoragescript". Klikněte na **Uložit**.

6.  Teď budete muset aktualizovat proměnné skriptu na základě nastavení konfigurace. Proměnná **$SubscriptionName** musíte aktualizovat vlastní předplatného. Můžete ponechat jiné proměnné podle skript nebo aktualizovat podle potřeby.

    - **$SubscriptionName:** Tuto proměnnou musíte aktualizovat názvu předplatného. Proveďte jeden z následujících tři způsoby, jak najít název vašeho předplatného:

        na. Ve **Windows PowerShell ISE**, klikněte na **soubor** > **Nový** vytvoříte nový soubor skriptu. Zkopírujte tento skript nový soubor skriptu a klikněte na tlačítko **ladění** > **Spustit**. Tento skript nejdřív vyzve vaší Azure přihlašovací údaje účtu přidat váš Azure účtu do místního prostředí PowerShell a potom zobrazit všechna předplatná, která jsou připojená k místní relaci Powershellu. Tady je pár poznámek na název předplatného, který chcete použít při tématu kurz:

            Add-AzureAccount
                Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName

        b. Nalezení a zkopírování název vašeho předplatného na [Portálu Azure](https://portal.azure.com), v nabídce centrální na levé straně klikněte na **předplatná**. Zkopírujte jména předplatné, které chcete použít při spuštění skripty v této příručce.

        ![Azure portálu][Image2]

        c. Nalezení a zkopírování název vašeho předplatného na [Portálu klasické Azure](https://manage.windowsazure.com/), posuňte se dolů a klikněte na **Nastavení** na levé straně na portálu. Klikněte na **předplatná** zobrazíte seznam předplatné. Zkopírujte jména předplatné, které chcete použít při spuštění skriptů, které jsou uvedené v této příručce.

        ![Azure klasické portálu][Image1]

    - **$StorageAccountName:** Použijte křestní jméno ve skriptu nebo zadejte nový název účtu úložiště. **Důležité:** Název účtu úložiště musí být jedinečný v Azure. Musí být malá písmena, příliš!

    - **$Location:** Použití dané "západ USA" v skript nebo zvolit jiné Azure místech, například východoasijských USA, severní Europe atd.

    - **$ContainerName:** Použijte křestní jméno ve skriptu nebo zadejte nový název pro váš kontejner.

    - **$ImageToUpload:** Zadejte cestu k obrázku ve vašem počítači, například: "C:\Images\HelloWorld.png".

    - **$DestinationFolder:** Zadejte cestu do místního adresáře ukládat soubory stažené ze Azure úložiště, jako je třeba: "C:\DownloadImages".

7.  Po aktualizaci proměnné skript v souboru "mystoragescript.ps1", klikněte na **soubor** > **Uložit**. Kliknutím na **ladění** > **Spustit** nebo stiskněte klávesu **F5** následujícím způsobem.  

Po spuštění skript, měli byste mít místní cílovou složku obsahující obrázek stažený soubor. Následující obrázek ukazuje příklad výstupu:

![Stáhněte si objekty BLOB][Image3]


> [AZURE.NOTE] V části "Začínáme s Azure úložiště a Powershellu v 5 minut" k dispozici rychlý úvod o tom, jak pomocí Powershellu Azure s úložištěm Azure. Podrobné informace a pokyny doporučujeme zobrazíte v následujících částech.

## <a name="prerequisites-for-using-azure-powershell-with-azure-storage"></a>Požadavky k úložišti Azure pomocí prostředí PowerShell Azure
Potřebujete Azure předplatné a účet, abyste mohli spouštět rutiny prostředí PowerShell uvedené v této příručce, jak jsme je popsali výše.

Azure Powershellu je moduly, které poskytuje rutiny pro správu Azure přes Windows PowerShell. Informace o instalaci a nastavení Azure Powershellu najdete v tématu [jak instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md). Doporučujeme stáhnout a nainstalovat nebo upgradovat na nejnovější modul Azure Powershellu před použitím tohoto průvodce.

Spusťte rutiny v konzole standardní prostředí Windows PowerShell nebo Windows PowerShell integrované skriptování prostředí (ISE). Například otevřete **ISE v prostředí Windows PowerShell**, přejděte do nabídky Start, zadejte nástroje pro správu a kliknutím ho spusťte. V okně nástroje pro správu klikněte pravým tlačítkem myši ISE v prostředí Windows PowerShell, klikněte na Spustit jako správce.

## <a name="how-to-manage-storage-accounts-in-azure"></a>Jak spravovat úložiště účty v Azure

### <a name="how-to-set-a-default-azure-subscription"></a>Jak nastavit výchozí Azure předplatného
Správa úložiště Azure pomocí prostředí PowerShell Azure, budete muset ověření prostředí klienta s Azure pomocí služby Azure Active Directory authentication nebo ověřování na základě certifikát. Podrobné informace najdete v tématu [instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md) kurz. Tato příručka používá ověřování služby Azure Active Directory.

1.  Ve Windows PowerShell ISE, zadejte tento příkaz Přidat váš Azure účtu do místního prostředí PowerShell:

    `Add-AzureAccount`

2.  V okně "Znak v na Microsoft Azure" Zadejte e-mailové adresy a hesla přidruženého k vašemu účtu. Azure ověří uloží přihlašovacími údaji a potom slouží k zavření okna.

3.  Další spusťte tento příkaz Zobrazit Azure účty ve vašem místním prostředí PowerShell a ověřit, že váš účet je uvedený:

    `Get-AzureAccount`

4.  Potom spusťte následující rutinu zobrazíte všechna předplatná, která jsou připojená k místní relaci Powershellu a ověřte, že je uvedená předplatného:

    `Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName`

5.  Chcete-li nastavit výchozí Azure předplatného, spusťte rutinu AzureSubscription vyberte:

        $SubscriptionName = 'Your subscription Name'
        Select-AzureSubscription -SubscriptionName $SubscriptionName –Default

6.  Ověřte název předplatného výchozí spuštěním rutiny Get-AzureSubscription:

    `Get-AzureSubscription -Default`

7.  Pokud chcete zobrazit všechny dostupné rutiny prostředí PowerShell Azure úložištěm, spusťte:

    `Get-Command -Module Azure -Noun *Storage*`

### <a name="how-to-create-a-new-azure-storage-account"></a>Jak vytvořit nový účet Azure úložiště
Použití Azure úložiště, budete potřebovat účet úložiště. Po dokončení konfigurace připojení k vašemu předplatnému, můžete vytvořit nový účet Azure úložiště.

1.  Spusťte rutinu Get-AzureLocation zobrazíte všechny dostupné datacentra umístění:

    `Get-AzureLocation | Format-Table -Property Name, AvailableServices, StorageAccountTypes`

2.  Potom spusťte rutinu New-AzureStorageAccount vytvořte nový účet úložiště. Následující příklad vytvoří nový účet úložiště ve datacentru "Západ USA".

        $location = "West US"
        $StorageAccountName = "yourstorageaccount"
        New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $location

> [AZURE.IMPORTANT] Název účtu úložiště musí být jedinečný v rámci Azure a musí být malá písmena. Konvence pro zadávání názvů a omezení najdete v článku [O úložiště účty adresářové služby Azure](storage-create-storage-account.md) a [Naming odkazování na kontejnery, objekty BLOB a Metadata](http://msdn.microsoft.com/library/azure/dd135715.aspx).

### <a name="how-to-set-a-default-azure-storage-account"></a>Jak nastavit výchozí účet Azure úložiště
Ve vašem předplatném může mít více účtů úložiště. Můžete zvolit jeden z nich a jeho nastavení jako výchozího účtu úložiště pro všechny příkazy úložiště ve stejné relaci Powershellu. Umožňuje spuštění příkazu prostředí PowerShell Azure úložiště aby neobsahovalo explicitně místní úložiště.

1.  Chcete-li nastavit výchozí účet úložiště u předplatného, můžete použít rutinu Set-AzureSubscription.

        $SubscriptionName = "Your subscription name"
        $StorageAccountName = "yourstorageaccount"  
        Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName

2.  Potom spusťte rutinu Get-AzureSubscription ověřte, zda je účet úložiště přidružené k účtu výchozí předplatného. Tento příkaz vrátí vlastnosti odběru aktuálního předplatného včetně jeho aktuálním účtu úložiště.

        Get-AzureSubscription –Current

### <a name="how-to-list-all-azure-storage-accounts-in-a-subscription"></a>Jak získat seznam všech účtů Azure úložiště v předplatné
Každý Azure předplatného můžete mít až 100 úložiště účty. Nejaktuálnější informace o limitech najdete v článku [Azure předplatné a omezení služby, kvót a omezení](../azure-subscription-service-limits.md).

Spusťte následující rutinu Zjistěte název a stav účtů úložiště v aktuálního předplatného:

    Get-AzureStorageAccount | Format-Table -Property StorageAccountName, Location, AccountType, StorageAccountStatus

### <a name="how-to-create-an-azure-storage-context"></a>Postup vytvoření místní Azure úložiště
Kontext Azure úložiště je objekt v prostředí PowerShell zapouzdřit úložiště pověření. Při spuštění následující rutinu umožňuje ověřování žádosti o aby neobsahovalo explicitně úložiště účtu a jeho přístupové klávesy, pomocí úložiště kontextu. Můžete vytvořit místní úložiště mnoha způsoby, například pomocí název a přístup klíč účtu úložiště, sdílené přístupový token podpisu (přidružení zabezpečení), připojovací řetězec nebo anonymní. Další informace najdete v tématu [AzureStorageContext nový](http://msdn.microsoft.com/library/azure/dn806380.aspx).  

Použijte jeden z následujících tři způsoby, jak vytvořit kontext úložiště:

- Spusťte rutinu [Get-AzureStorageKey](http://msdn.microsoft.com/library/azure/dn495235.aspx) zjistěte přístupová klávesa primární úložiště pro váš účet Azure úložiště. Dále zavolejte rutinu [New-AzureStorageContext](http://msdn.microsoft.com/library/azure/dn806380.aspx) k vytvoření úložiště kontextu:

        $StorageAccountName = "yourstorageaccount"
        $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
        $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary


- Generovat sdílené přístupový token podpisu pro kontejneru Azure úložiště a použijte ji k vytvoření místní úložiště:

        $sasToken = New-AzureStorageContainerSASToken -Container abc -Permission rl
        $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -SasToken $sasToken

    Další informace najdete v tématu [Nový AzureStorageContainerSASToken](http://msdn.microsoft.com/library/azure/dn806416.aspx) a [Používání sdílených přístup podpisy (přidružení zabezpečení)](storage-dotnet-shared-access-signature-part-1.md).

- V některých případech je vhodné zadat koncové body služby při vytvoření nové místní úložiště. To může být potřeba při zaregistrujete vlastní název domény pro váš účet úložiště objektů Blob službou nebo chcete použít podpis sdílený přístup pro získání přístupu k prostředkům úložiště. Nastavení služby koncové body v připojovacím řetězci a použijte ji k vytvoření nové místní úložiště jak je ukázáno v následujícím příkladu:

        $ConnectionString = "DefaultEndpointsProtocol=http;BlobEndpoint=<blobEndpoint>;QueueEndpoint=<QueueEndpoint>;TableEndpoint=<TableEndpoint>;AccountName=<AccountName>;AccountKey=<AccountKey>"
        $Ctx = New-AzureStorageContext -ConnectionString $ConnectionString

Další informace o tom, jak nakonfigurovat úložiště připojovací řetězec najdete v článku [Konfigurace připojovací řetězec](storage-configure-connection-string.md).

Teď máte nastavte si svůj počítač a naučili ke správě předplatná a úložiště účty pomocí prostředí PowerShell Azure, přejděte k následující části se dozvíte, jak spravovat objekty BLOB Azure a objektů blob snímky.

### <a name="how-to-retrieve-and-regenerate-azure-storage-keys"></a>Jak načíst a obnovovat Azure úložiště klíče

Účet Azure úložiště je součástí dva účtu klíče. Následující rutinu slouží k načtení klíče.

    Get-AzureStorageKey -StorageAccountName "yourstorageaccount"

Použijte následující rutinu k načtení určitého klíče. Platné hodnoty jsou primární a sekundární.  

    (Get-AzureStorageKey -StorageAccountName $StorageAccountName).Primary

    (Get-AzureStorageKey -StorageAccountName $StorageAccountName).Secondary

Pokud chcete obnovit klíče, použijte následující rutinu. Platné hodnoty – typ klíče jsou "Primární" a "Sekundární"

    New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType “Primary”

    New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType “Secondary”

## <a name="how-to-manage-azure-blobs"></a>Správa objektů BLOB Azure
Úložiště objektů Blob Azure je služba pro ukládání velkých objemů Nestrukturovaná data, například text nebo binární data, která je přístupný z kdekoli na světě prostřednictvím protokolu HTTP nebo HTTPS. Tato část se předpokládá, že znáte už služba úložiště objektů Blob Azure koncepty. Podrobné informace najdete v tématu [Začínáme s úložiště objektů Blob pomocí .NET](storage-dotnet-how-to-use-blobs.md) a [Objektů Blob služby koncepty](http://msdn.microsoft.com/library/azure/dd179376.aspx).

### <a name="how-to-create-a-container"></a>Vytvoření kontejneru
Každý blob Azure úložiště musí být v kontejneru. Můžete vytvořit kontejner soukromé pomocí rutinu New-AzureStorageContainer:

    $StorageContainerName = "yourcontainername"
    New-AzureStorageContainer -Name $StorageContainerName -Permission Off

> [AZURE.NOTE] Existují tři úrovně anonymní přístup pro čtení: **vypnutí** **objektů Blob**a **kontejner**. Chcete-li zabránit anonymní přístup objektů BLOB, nastavte parametr oprávnění na **vypnout**. Ve výchozím nastavení nového kontejneru je soukromá a k nim získat přístup jenom vlastník účtu. Povolení anonymního přístupu ke čtení veřejné objektů blob zdroje, ale ne na kontejner metadata nebo do seznamu objektů BLOB v kontejneru nastavit parametr oprávnění **objektů Blob**. Umožňuje důkladné veřejné přístup pro čtení objektů blob zdrojů, container metadat a seznamu objektů BLOB v kontejneru nastavte parametr oprávnění na **kontejner**. Další informace najdete v tématu [Správa anonymní přístup pro čtení kontejnerů a objektů BLOB](storage-manage-access-to-resources.md).

### <a name="how-to-upload-a-blob-into-a-container"></a>Jak nahrát objektů blob do kontejneru
Úložiště objektů Blob Azure podporuje objektů BLOB bloku a objekty BLOB stránky. Další informace najdete v tématu [Principy blok objektů BLOB, přidat objektů BLOB a objekty BLOB stránky](http://msdn.microsoft.com/library/azure/ee691964.aspx).

K nahrání objektů BLOB v kontejneru, můžete použít rutinu [Set-AzureStorageBlobContent](http://msdn.microsoft.com/library/azure/dn806379.aspx) . Ve výchozím nastavení tento příkaz, odešlou se místní soubory objektů blob blokovat. Chcete-li určit typ objektů blob, můžete použít parametr – BlobType.

Následující příklad spustí rutinu [Get-ChildItem](http://technet.microsoft.com/library/hh849800.aspx) zobrazíte všechny soubory do zadané složky a předá je další rutina pomocí operátoru kanálem k odesílání zpráv. Rutina [Set-AzureStorageBlobContent](http://msdn.microsoft.com/library/azure/dn806379.aspx) odešle místní soubory do váš kontejner:

    Get-ChildItem –Path C:\Images\* | Set-AzureStorageBlobContent -Container "yourcontainername"

### <a name="how-to-download-blobs-from-a-container"></a>Jak stáhnout objektů blob z kontejneru
Následující příklad ukazuje, jak stáhnout objektů blob z kontejneru. V příkladu nejdřív naváže připojení k úložišti Azure v kontextu úložiště účtu, který obsahuje název účtu úložiště a jeho primární přístupové klávesy. Příklad pak načte objektů blob odkaz pomocí rutiny [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) . Pak v příkladu rutinu [Get-AzureStorageBlobContent](http://msdn.microsoft.com/library/azure/dn806418.aspx) ke stažení objektů BLOB do místní cílovou složku.

    #Define the variables.
    $ContainerName = "yourcontainername"
    $DestinationFolder = "C:\DownloadImages"

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #List all blobs in a container.
    $blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

    #Download blobs from a container.
    New-Item -Path $DestinationFolder -ItemType Directory -Force
    $blobs | Get-AzureStorageBlobContent -Destination $DestinationFolder -Context $Ctx

### <a name="how-to-copy-blobs-from-one-storage-container-to-another"></a>Jak zkopírovat objektů blob z jednoho kontejneru úložiště
Můžete zkopírovat objektů BLOB různých úložiště účty a oblastí asynchronní. Následující příklad ukazuje, jak zkopírovat objektů blob z jednoho úložiště container do jiného ve dvou různých úložiště účty. Příklad nejdřív nastaví proměnné zdrojového a cílového úložiště účtů a potom vytvoří místní úložiště pro každý účet. Pak v příkladu slouží ke kopírování objektů blob z kontejneru zdroj do kontejneru cíl pomocí rutiny [Start AzureStorageBlobCopy](http://msdn.microsoft.com/library/azure/dn806394.aspx) . Příklad předpokládá, že zdrojového a cílového úložiště účty a kontejnery existovat.

    #Define the source storage account and context.
    $SourceStorageAccountName = "yoursourcestorageaccount"
    $SourceStorageAccountKey = "Storage key for yoursourcestorageaccount"
    $SrcContainerName = "yoursrccontainername"
    $SourceContext = New-AzureStorageContext -StorageAccountName $SourceStorageAccountName -StorageAccountKey $SourceStorageAccountKey

    #Define the destination storage account and context.
    $DestStorageAccountName = "yourdeststorageaccount"
    $DestStorageAccountKey = "Storage key for yourdeststorageaccount"
    $DestContainerName = "destcontainername"
    $DestContext = New-AzureStorageContext -StorageAccountName $DestStorageAccountName -StorageAccountKey $DestStorageAccountKey

    #Get a reference to blobs in the source container.
    $blobs = Get-AzureStorageBlob -Container $SrcContainerName -Context $SourceContext

    #Copy blobs from one container to another.
    $blobs| Start-AzureStorageBlobCopy -DestContainer $DestContainerName -DestContext $DestContext

Všimněte si, že v tomto příkladu provádí asynchronní kopírovat. Sledování stavu kopií spuštěním rutiny [Get-AzureStorageBlobCopyState](http://msdn.microsoft.com/library/azure/dn806406.aspx) .

### <a name="how-to-copy-blobs-from-a-secondary-location"></a>Kopírování objektů BLOB ze sekundárního umístění
Objekty BLOB můžete zkopírovat z sekundární umístění Vzdálená pomoc podporou GRS účtu.

    #define secondary storage context using a connection string constructed from secondary endpoints.
    $SrcContext = New-AzureStorageContext -ConnectionString "DefaultEndpointsProtocol=https;AccountName=***;AccountKey=***;BlobEndpoint=http://***-secondary.blob.core.windows.net;FileEndpoint=http://***-secondary.file.core.windows.net;QueueEndpoint=http://***-secondary.queue.core.windows.net; TableEndpoint=http://***-secondary.table.core.windows.net;"
    Start-AzureStorageBlobCopy –Container *** -Blob *** -Context $SrcContext –DestContainer *** -DestBlob *** -DestContext $DestContext

### <a name="how-to-delete-a-blob"></a>Jak odstranit objektů blob
Odstranit objektů blob, nejprve získat odkaz objektů blob a potom zavolat rutinu AzureStorageBlob odebrat. Následující příklad odstraní všechny objekty BLOB v kontejneru dané. Příklad nejdřív nastaví proměnné účtu úložiště a potom vytvoří místní úložiště. V příkladu potom načte objektů blob odkaz pomocí rutiny [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) a, spustí se [Odebrat AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806399.aspx) odeberete kontejneru v Azure úložiště objektů BLOB.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $ContainerName = "containername"
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Get a reference to all the blobs in the container.
    $blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

    #Delete blobs in a specified container.
    $blobs| Remove-AzureStorageBlob

## <a name="how-to-manage-azure-blob-snapshots"></a>Správa objektů blob Azure snímky
Azure umožňuje vytvořit snímek objektů blob. Snímek je jen pro čtení verze objektů blob, které využívá v určitém bodě v čase. Po vytvoření snímek ho můžete číst, zkopírovali, nebo odstranit, ale nebyly změněny. Snímky lze k obecnějším údajům objektů blob zobrazená v časovém okamžiku. Další informace najdete v tématu [Vytvoření snímek objektů Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).

### <a name="how-to-create-a-blob-snapshot"></a>Jak vytvořit snímek objektů blob
Pokud chcete vytvořit snímek objektů blob, nejprve získat odkaz objektů blob a zavolejte `ICloudBlob.CreateSnapshot` metoda v něm. Následující příklad nejdřív nastaví proměnné účtu úložiště a vytvoří místní úložiště. V příkladu potom načte objektů blob odkaz pomocí rutiny [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) a spustí metodu [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) k vytvoření snímku.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $ContainerName = "yourcontainername"
    $BlobName = "yourblobname"
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Get a reference to a blob.
    $blob = Get-AzureStorageBlob -Context $Ctx -Container $ContainerName -Blob $BlobName

    #Create a snapshot of the blob.
    $snap = $blob.ICloudBlob.CreateSnapshot()

### <a name="how-to-list-a-blobs-snapshots"></a>Jak získat seznam snímky objektů blob
Můžete vytvořit tolik snímky, jak se má zobrazovat objektů blob. Můžete vytvořit seznam snímky přidružený k vaší objektů blob ke sledování aktuální snímky. Následující příklad používá předdefinované objektů blob a volá rutinu [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) seznam snímky, které objektů blob.  

    #Define the blob name.
    $BlobName = "yourblobname"

    #List the snapshots of a blob.
    Get-AzureStorageBlob –Context $Ctx -Prefix $BlobName -Container $ContainerName  | Where-Object  { $_.ICloudBlob.IsSnapshot -and $_.Name -eq $BlobName }

### <a name="how-to-copy-a-snapshot-of-a-blob"></a>Kopírování snímku objektů blob
Můžete zkopírovat snímek objektů blob Obnovit snímek. Podrobné informace a omezení najdete v tématu [Vytvoření snímku objektů Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx). Následující příklad nejdřív nastaví proměnné účtu úložiště a vytvoří místní úložiště. Příklad pak definuje proměnné jméno container a objektů blob. Příklad načte objektů blob odkaz pomocí rutiny [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) a spustí metodu [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) k vytvoření snímku. Pak v příkladu, spustí se [Start AzureStorageBlobCopy](http://msdn.microsoft.com/library/azure/dn806394.aspx) Kopírovat snímek objektů blob pomocí objektu ICloudBlob pro objektů blob zdroje. Je potřeba aktualizovat proměnné na základě vaší konfigurace před spuštěním příkladu. Všimněte si, že v následujícím příkladu předpokládá, že zdroji a kontejnery cíl a objektů blob zdroj existovat.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Define the variables.
    $SrcContainerName = "yoursourcecontainername"
    $DestContainerName = "yourdestcontainername"
    $SrcBlobName = "yourblobname"
    $DestBlobName = "CopyBlobName"

    #Get a reference to a blob.
    $blob = Get-AzureStorageBlob -Context $Ctx -Container $SrcContainerName -Blob $SrcBlobName

    #Create a snapshot of a blob.
    $snap = $blob.ICloudBlob.CreateSnapshot()

    #Copy the snapshot to another container.
    Start-AzureStorageBlobCopy –Context $Ctx -ICloudBlob $snap -DestBlob $DestBlobName -DestContainer $DestContainerName

Teď, když jste se naučili jak spravovat objekty BLOB Azure a objektů blob snímků pomocí prostředí PowerShell Azure, přejděte k následující části se dozvíte, jak spravovat tabulek, dotazů a soubory.

## <a name="how-to-manage-azure-tables-and-table-entities"></a>Jak spravovat Azure tabulek a entity tabulky
Azure tabulku služba úložiště je NoSQL úložiště, které slouží k ukládání a dotaz velké množství dat strukturovaných, -relační. Hlavní součásti služby jsou tabulky, entity a vlastnosti. Tabulky je kolekce entity. Entita je sada vlastností. Každá entita může mít až 252 vlastnosti, které se shodují všechny dvojice název hodnota. Tato část se předpokládá, že znáte už služba úložiště tabulek Azure koncepty. Podrobné informace najdete v tématu [Principy datového modelu tabulku služba](http://msdn.microsoft.com/library/azure/dd179338.aspx) a [začít pracovat s úložiště tabulek Azure pomocí .NET](storage-dotnet-how-to-use-tables.md).

V následujících pododdílu se dozvíte, jak spravovat Služba úložiště tabulek Azure pomocí prostředí PowerShell Azure. Scénáře nichž se uplatní zahrnují **Vytvoření**, **Odstranění**a **načtení** **tabulky**, jakož i **Přidání**, **dotazování**a **Odstranění tabulky entity**.

### <a name="how-to-create-a-table"></a>Jak vytvořit tabulku
Každé tabulce musí být umístěny ve účet Azure úložiště. Následující příklad ukazuje, jak vytvořit tabulku v úložišti Azure. V příkladu nejdřív naváže připojení k úložišti Azure v kontextu úložiště účtu, který obsahuje název účtu úložiště a jeho přístupové klávesy. Pak používá rutinu [New-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806417.aspx) k vytvoření tabulky v úložišti Azure.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Create a new table.
    $tabName = "yourtablename"
    New-AzureStorageTable –Name $tabName –Context $Ctx

### <a name="how-to-retrieve-a-table"></a>Jak k načtení tabulky
Můžete vyhledat a načtení jedné nebo všech tabulek v účtu úložiště. Následující příklad ukazuje, jak získat danou tabulku pomocí rutiny [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) .

    #Retrieve a table.
    $tabName = "yourtablename"
    Get-AzureStorageTable –Name $tabName –Context $Ctx

Pokud jste volali rutinu Get-AzureStorageTable bez parametrů, dostane všechny tabulky úložiště účtu úložiště.

### <a name="how-to-delete-a-table"></a>Postup odstranění tabulky
Odstranění tabulky z úložiště účet pomocí rutiny [AzureStorageTable odebrat](http://msdn.microsoft.com/library/azure/dn806393.aspx) .  

    #Delete a table.
    $tabName = "yourtablename"
    Remove-AzureStorageTable –Name $tabName –Context $Ctx

### <a name="how-to-manage-table-entities"></a>Jak spravovat entity tabulky
V současné době Azure PowerShell neposkytuje rutiny pro správu entity tabulky přímo. Provádět operace s entity tabulky můžete použít třídy zadaných v [Azure úložiště klienta knihovny pro .NET](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx).

#### <a name="how-to-add-table-entities"></a>Jak přidat entity tabulky
Entita do tabulky přidáte nejprve vytvořte objekt, který definuje vlastnosti entity. Entita může mít až 255 vlastnosti, včetně 3 Vlastnosti systému: **PartitionKey**, **RowKey**a **časové razítko**. Zodpovídáte za režimem vkládání a aktualizace hodnoty **PartitionKey** a **RowKey**. Server spravuje hodnotu **časového razítka**, které nelze změnit. Společně **PartitionKey** a **RowKey** jednoznačně identifikovat každou entitu uvnitř tabulky.

-   **PartitionKey**: Určuje oddílu, který entitu uložený ve.
-   **RowKey**: jednoznačně identifikuje entity v oddílu.

Můžete definovat maximálně 252 vlastní vlastnosti entity. Další informace najdete v článku [Principy datového modelu tabulku služba](http://msdn.microsoft.com/library/azure/dd179338.aspx).

Následující příklad ukazuje, jak přidávat do tabulky entity. Příklad ukazuje, jak načíst tabulce zaměstnanců a do něj přidat několik entity. Nejdřív naváže připojení k úložišti Azure v kontextu úložiště účtu, který obsahuje název účtu úložiště a jeho přístupové klávesy. Pak načte do dané tabulky pomocí rutiny [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) . Pokud v tabulce neexistuje, rutinu [New-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806417.aspx) slouží k vytvoření tabulky v úložišti Azure. Příklad pak definuje vlastní funkci Přidat Entity přidáte entity do tabulky tak, že zadá oddíl Každá entita a klíče řádku. Funkce Přidat Entity volá rutinu [New-objektu](http://technet.microsoft.com/library/hh849885.aspx) ve třídě [Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.dynamictableentity.aspx) entity objekt vytvořte. V příkladu později, volá metodu [Microsoft.WindowsAzure.Storage.Table.TableOperation.Insert](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.insert.aspx) na tento objekt entity přidáte do tabulky.

    #Function Add-Entity: Adds an employee entity to a table.
    function Add-Entity() {
        [CmdletBinding()]
        param(
           $table,
           [String]$partitionKey,
           [String]$rowKey,
           [String]$name,
           [Int]$id
        )

      $entity = New-Object -TypeName Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity -ArgumentList $partitionKey, $rowKey
      $entity.Properties.Add("Name", $name)
      $entity.Properties.Add("ID", $id)

      $result = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Insert($entity))
    }

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    $TableName = "Employees"

    #Retrieve the table if it already exists.
    $table = Get-AzureStorageTable –Name $TableName -Context $Ctx -ErrorAction Ignore

    #Create a new table if it does not exist.
    if ($table -eq $null)
    {
       $table = New-AzureStorageTable –Name $TableName -Context $Ctx
    }

    #Add multiple entities to a table.
    Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row1 -Name Chris -Id 1
    Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row2 -Name Jessie -Id 2
    Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row1 -Name Christine -Id 3
    Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row2 -Name Steven -Id 4

#### <a name="how-to-query-table-entities"></a>Jak entity tabulky dotazů
K vytvoření dotazu tabulky, použijte [Microsoft.WindowsAzure.Storage.Table.TableQuery](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tablequery.aspx) předmětu. V následujícím příkladu se předpokládá, že jste již spuštěním skriptu zadaných v dialogu jak chcete přidat oddíl entity této příručky. V příkladu nejdřív naváže připojení k úložišti Azure v kontextu úložiště, který obsahuje název účtu úložiště a jeho přístupové klávesy. Další pokusí se získat dříve vytvořené pomocí rutiny [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) tabulka "Zaměstnanci". Volání rutinu [New-objektu](http://technet.microsoft.com/library/hh849885.aspx) ve třídě Microsoft.WindowsAzure.Storage.Table.TableQuery vytvoří nový dotaz objekt. Může vzorec vypadat třeba entit, které mají sloupec "ID", jejichž hodnota je 1 podle filtr řetězce. Podrobné informace najdete v tématu [dotazování tabulek a entity](http://msdn.microsoft.com/library/azure/dd894031.aspx). Když spustíte tento dotaz vrátí všech entit, které splňují daná kritéria filtru.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    $TableName = "Employees"

    #Get a reference to a table.
    $table = Get-AzureStorageTable –Name $TableName -Context $Ctx

    #Create a table query.
    $query = New-Object Microsoft.WindowsAzure.Storage.Table.TableQuery

    #Define columns to select.
    $list = New-Object System.Collections.Generic.List[string]
    $list.Add("RowKey")
    $list.Add("ID")
    $list.Add("Name")

    #Set query details.
    $query.FilterString = "ID gt 0"
    $query.SelectColumns = $list
    $query.TakeCount = 20

    #Execute the query.
    $entities = $table.CloudTable.ExecuteQuery($query)

    #Display entity properties with the table format.
    $entities  | Format-Table PartitionKey, RowKey, @{ Label = "Name"; Expression={$_.Properties["Name"].StringValue}}, @{ Label = "ID"; Expression={$_.Properties["ID"].Int32Value}} -AutoSize

#### <a name="how-to-delete-table-entities"></a>Jak odstranit entity tabulky
Můžete odstranit entita použít jeho oddílů a řádek. V následujícím příkladu se předpokládá, že jste již spuštěním skriptu zadaných v dialogu jak chcete přidat oddíl entity této příručky. V příkladu nejdřív naváže připojení k úložišti Azure v kontextu úložiště, který obsahuje název účtu úložiště a jeho přístupové klávesy. Další pokusí se získat dříve vytvořené pomocí rutiny [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) tabulka "Zaměstnanci". Pokud tabulka existuje, příklad volá metodu [Microsoft.WindowsAzure.Storage.Table.TableOperation.Retrieve](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.retrieve.aspx) k načtení entita na základě jeho oddílů a řádek klíčových hodnot. Potom předejte entitu metodě [Microsoft.WindowsAzure.Storage.Table.TableOperation.Delete](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.delete.aspx) odstranit.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

    #Retrieve the table.
    $TableName = "Employees"
    $table = Get-AzureStorageTable -Name $TableName -Context $Ctx -ErrorAction Ignore

    #If the table exists, start deleting its entities.
    if ($table -ne $null) {
       #Together the PartitionKey and RowKey uniquely identify every  
       #entity within a table.
       $tableResult = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Retrieve("Partition2", "Row1"))
       $entity = $tableResult.Result
    if ($entity -ne $null)
    {
       #Delete the entity.
       $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Delete($entity))
    }
    }

## <a name="how-to-manage-azure-queues-and-queue-messages"></a>Jak spravovat Azure fronty a zpráv
Azure fronty úložiště je služba pro ukládání většího počtu zpráv, které jsou přístupné z kdekoli na světě prostřednictvím ověřené volání pomocí protokolu HTTP nebo HTTPS. Tato část se předpokládá, že znáte už úložiště služby Azure frontě konceptů. Podrobné informace najdete v tématu [Začínáme s úložištěm fronty Azure pomocí .NET](storage-dotnet-how-to-use-queues.md).

V této části se zobrazí Správa úložiště služby Azure fronty pomocí prostředí PowerShell Azure. Scénáře nichž se uplatní zahrnují **Vložení** a **Odstranění** zprávy fronty i **Vytvoření**, **Odstranění**a **načítání dotazů**.

### <a name="how-to-create-a-queue"></a>Postup vytvoření fronty
Následující příklad nejdřív naváže připojení k úložišti Azure v kontextu úložiště účtu, který obsahuje název účtu úložiště a jeho přístupové klávesy. Dále volá rutinu [New-AzureStorageQueue](http://msdn.microsoft.com/library/azure/dn806382.aspx) k vytvoření fronty s názvem "název_fronty".

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    $QueueName = "queuename"
    $Queue = New-AzureStorageQueue –Name $QueueName -Context $Ctx

Informace o vytváření názvů pro službu Azure fronty najdete v článku [pojmenování fronty a Metadata](http://msdn.microsoft.com/library/azure/dd179349.aspx).

### <a name="how-to-retrieve-a-queue"></a>Jak získat fronty
Můžete vyhledat a získat konkrétní fronty nebo seznam všech frontách v účtu úložiště. Následující příklad ukazuje, jak získat určené frontě pomocí rutiny [Get-AzureStorageQueue](http://msdn.microsoft.com/library/azure/dn806377.aspx) .

    #Retrieve a queue.
    $QueueName = "queuename"
    $Queue = Get-AzureStorageQueue –Name $QueueName –Context $Ctx

Pokud jste volali rutinu [Get-AzureStorageQueue](http://msdn.microsoft.com/library/azure/dn806377.aspx) bez parametrů, obdrží seznam všech frontách.

### <a name="how-to-delete-a-queue"></a>Jak odstranit fronty
Pokud chcete odstranit fronty a všech zpráv, na které se nacházejí v ní, kontaktujte podporu rutinu AzureStorageQueue odebrat. Následující příklad ukazuje, jak odstranit určené frontě pomocí rutiny AzureStorageQueue odebrat.

    #Delete a queue.
    $QueueName = "yourqueuename"
    Remove-AzureStorageQueue –Name $QueueName –Context $Ctx

#### <a name="how-to-insert-a-message-into-a-queue"></a>Jak vložit zprávu do fronty
Vložit zprávu do existující fronty, nejprve vytvořte nové instance třídy [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) . Dále zavolejte metodu [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) . CloudQueueMessage může vytvořit řetězec (ve formátu UTF-8) nebo pole bajtů.

Následující příklad ukazuje, jak přidat zprávu do fronty. V příkladu nejdřív naváže připojení k úložišti Azure v kontextu úložiště účtu, který obsahuje název účtu úložiště a jeho přístupové klávesy. Pak načte zadaný fronty pomocí rutiny [Get-AzureStorageQueue](https://msdn.microsoft.com/library/azure/dn806377.aspx) . Pokud existuje fronta rutinu [New-objekt](http://technet.microsoft.com/library/hh849885.aspx) slouží k vytvoření instance třídy [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) . V příkladu později, volá metodu [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) na tento objekt zpráva přidáte do fronty. Tady je kód, který načítá fronty a vloží se zpráva "MessageInfo":

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

    #Retrieve the queue.
    $QueueName = "queuename"
    $Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

    #If the queue exists, add a new message.
    if ($Queue -ne $null) {
       # Create a new message using a constructor of the CloudQueueMessage class.
       $QueueMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList MessageInfo

       #Add a new message to the queue.
       $Queue.CloudQueue.AddMessage($QueueMessage)
    }


#### <a name="how-to-de-queue-at-the-next-message"></a>Jak odstranit fronty další zprávy
Kód zrušte fronty zprávu z fronty ve dvou krocích. Při volání metodu [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.GetMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.getmessage.aspx) získat další zprávu ve frontě. Zprávy vrácených **GetMessage** přestane být viditelné pro jiné kódy čtení zpráv z této fronty. Dokončete zprávu odebráním fronty musí taky zavoláte [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.DeleteMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.deletemessage.aspx) metodu. Krocích popsaných odebrání zprávy zajišťuje, že pokud kódu nepovede zpracuje zprávy kvůli chybě hardware a software, jiné instanci aplikace kódu můžete zobrazí stejná zpráva a začněte odznovu. Váš kód volá **DeleteMessage** vpravo po zpracování zprávy.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

    #Retrieve the queue.
    $QueueName = "queuename"
    $Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

    $InvisibleTimeout = [System.TimeSpan]::FromSeconds(10)

    #Get the message object from the queue.
    $QueueMessage = $Queue.CloudQueue.GetMessage($InvisibleTimeout)
    #Delete the message.
    $Queue.CloudQueue.DeleteMessage($QueueMessage)

## <a name="how-to-manage-azure-file-shares-and-files"></a>Jak spravovat Azure sdílených položek a souborů
Azure úložiště souborů nabízí sdílené úložiště pro aplikace, které používají protokol standardní SMB. Virtuálních počítačích Microsoft Azure a cloudovým službám můžete zužitkování datového souboru součástí aplikace prostřednictvím připojené sdílené položky a místní aplikací přístup dat souborů ve sdílené složce prostřednictvím rozhraní API úložiště souborů nebo Azure Powershellu.

Další informace o ukládání souborů Azure najdete v článku [Začínáme s úložiště souborů Azure v systému Windows](storage-dotnet-how-to-use-files.md) a [Rozhraní REST API soubor služby](http://msdn.microsoft.com/library/azure/dn167006.aspx).

## <a name="how-to-set-and-query-storage-analytics"></a>Jak nastavit a analýzy úložiště dotazů
[Azure úložiště analýzy](storage-analytics.md) umožňuje shromáždit metriky Azure úložiště účty a data protokolu o žádosti o odeslané ke svému účtu úložiště. Metriky úložišť slouží ke sledování stavu účtu úložiště a úložiště protokolování Diagnostika a Poradce při potížích s účtem úložiště.
Ve výchozím nastavení není povolený metriky úložišť úložiště služby. Můžete povolit sledování pomocí portálu Azure nebo prostředí Windows PowerShell nebo programově pomocí knihovně úložiště klienta. Protokolování úložiště děje serverovou a umožňuje podrobností záznamů pro úspěšné a neúspěšné požadavky ve vašem účtu úložiště. Tyto protokoly umožňují zobrazit podrobnosti o čtení, psaní a odstranění operace u tabulek, dotazů a objekty BLOB stejně jako důvody žádosti o nezdařeném uložení.

Informace o povolení a zobrazení dat metriky úložišť pomocí prostředí PowerShell najdete v tématu [jak povolit metriky úložišť pomocí Powershellu](http://msdn.microsoft.com/library/azure/dn782843.aspx#HowtoenableStorageMetricsusingPowerShell).

Se dozvíte, jak povolit a načtení protokolování úložiště dat pomocí prostředí PowerShell, najdete v článku [Povolení úložiště protokolování pomocí prostředí PowerShell](http://msdn.microsoft.com/library/azure/dn782840.aspx#HowtoenableStorageLoggingusingPowerShell) a [hledání protokolu dat úložiště protokolování](http://msdn.microsoft.com/library/azure/dn782840.aspx#FindingyourStorageLogginglogdata).
Podrobné informace o použití metriky úložišť and Logging úložiště k řešení problémů úložiště najdete v tématu [sledování, diagnostikování a řešení potíží s úložišti tabulek Microsoft Azure](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="how-to-manage-shared-access-signature-sas-and-stored-access-policy"></a>Jak spravovat přidružení sdílené podpis aplikace Access (zabezpečení) a uložené zásady přístupu
Sdílený přístup podpisy jsou důležitou součástí model zabezpečení pro všechny aplikace používají Azure úložiště. Jsou vhodné pro zajištění omezená oprávnění ke svému účtu úložiště pro klienty, kteří nemají klíč účtu. Ve výchozím nastavení mohou přístup jenom vlastník účtu úložiště objektů BLOB, tabulky a fronty v rámci tohoto účtu. Pokud služby nebo aplikace, musí být k dispozici tyto materiály jiným klientům přitom sdílet přístupové klávesy, máte tři možnosti:

- Nastavení oprávnění kontejner pro povolení anonymního přístupu pro čtení a jeho objektů BLOB kontejner. To není povoleno pro tabulek nebo dotazů.
- Používejte podpis sdílený přístup, že podpory omezená přístupová práva kontejnerů, objekty BLOB, dotazů a tabulek pro určitého časového intervalu.
- K získání další úroveň publikum nemůže ovládat podpisy sdílený přístup pro kontejner nebo jeho objektů BLOB, fronty nebo tabulky použijte uložené přístupu. Zásady uložené přístupu můžete změnit čas zahájení, dobu platnosti nebo oprávnění pro podpis nebo odvolat za ni byl vydán.

Sdílený přístup podpis může být jedním ze dvou formulářů:

- **Ad hoc přidružení zabezpečení**: při vytváření ad hoc přidružení zabezpečení, čas zahájení, dobu platnosti, a oprávnění pro přidružení zabezpečení jsou uvedena na URI přidružení zabezpečení. Tento typ přidružení lze vytvořit kontejner, objektů blob, tabulku nebo fronty a je není revokable.
- **Přidružení s uložené přístupu**: uložené přístupu je definován na kontejner zdroje kontejneru objektů blob, tabulce nebo fronty – a slouží ke správě omezení pro jeden nebo více podpisů sdílený přístup. Přidružení přidružení zabezpečení zásadami uložené přístupu dědí přidružení zabezpečení omezení - čas zahájení, čas vypršení a oprávnění - definované pro uložené přístupu. Tento typ přidružení je revokable.

Další informace najdete v tématu [Používání sdílených přístup podpisy (přidružení zabezpečení)](storage-dotnet-shared-access-signature-part-1.md) a [Spravovat anonymní přístup pro čtení kontejnerů a objektů BLOB](storage-manage-access-to-resources.md).

V dalších částech se dozvíte, jak k vytvoření zásad tokenu a uložené přístup podpis sdílený přístup pro Azure tabulky. Azure Powershellu poskytuje podobné rutiny kontejnerů, objekty BLOB a taky fronty. Pokud chcete spustit skripty v této části, stáhnout [Azure PowerShell verze 0.8.14](http://go.microsoft.com/?linkid=9811175&clcid=0x409) nebo novější.

### <a name="how-to-create-a-policy-based-shared-access-signature-token"></a>Jak vytvořit token zásadami sdílené podpis přístup
Použijte rutinu New-AzureStorageTableStoredAccessPolicy k vytvoření nových zásad uložené přístup. Zavolejte rutinu [New-AzureStorageTableSASToken](http://msdn.microsoft.com/library/azure/dn806400.aspx) vytvořit nový podpis token sdílené přístupu na základě zásad pro tabulku Azure úložiště.

    $policy = "policy1"
    New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx
    New-AzureStorageTableSASToken -Name $tableName -Policy $policy -Context $Ctx

### <a name="how-to-create-an-ad-hoc-non-revokable-shared-access-signature-token"></a>Jak vytvořit token ad hoc sdílené podpis aplikace Access (není revokable)
Použijte rutinu [New-AzureStorageTableSASToken](http://msdn.microsoft.com/library/azure/dn806400.aspx) k vytvoření nové ad hoc (není revokable) sdílené podpis přístup tokenu pro tabulku úložišti Azure:

    New-AzureStorageTableSASToken -Name $tableName -Permission "rqud" -StartTime "2015-01-01" -ExpiryTime "2015-02-01" -Context $Ctx

### <a name="how-to-create-a-stored-access-policy"></a>Jak vytvořit zásady uložené přístupu
Použijte rutinu New-AzureStorageTableStoredAccessPolicy k vytvoření nové zásady uložené přístupu pro tabulku úložišti Azure:

    $policy = "policy1"
    New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx

### <a name="how-to-update-a-stored-access-policy"></a>Pokyny k aktualizaci uložených přístupu
Použijte rutinu Set-AzureStorageTableStoredAccessPolicy k aktualizaci existující zásady uložené přístup pro úložišti Azure tabulku:

    Set-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Permission "rd" -NoExpiryTime -NoStartTime -Context $Ctx

### <a name="how-to-delete-a-stored-access-policy"></a>Jak odstranit uložené přístupu
Odstranění zásady přístupu uložené v úložišti Azure tabulce získáte pomocí rutiny AzureStorageTableStoredAccessPolicy odebrat:

    Remove-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Context $Ctx


## <a name="how-to-use-azure-storage-for-us-government-and-azure-china"></a>Jak používat Azure úložiště pro vládní organizace a Čína Azure
Azure prostředí je nezávislý nasazení Microsoft Azure, například [Azure Government pro vládní organizace](https://azure.microsoft.com/features/gov/), [AzureCloud pro globální Azure](https://portal.azure.com)a [AzureChinaCloud Azure provozovaný společností 21Vianet v Číně](http://www.windowsazure.cn/). Můžete nasadit nové Azure prostředí pro státní správu Americká a Azure Čína.

Úložiště Azure pomocí služby AzureChinaCloud, je potřeba vytvořit úložiště kontextu, který je spojený s AzureChinaCloud. Tyto kroky vám usnadní zahájení práce:

1.  Spusťte rutinu [Get-AzureEnvironment](https://msdn.microsoft.com/library/azure/dn790368.aspx) zobrazíte dostupná Azure prostředí:

    `Get-AzureEnvironment`

2.  Přidání účtu Azure Čína prostředí Windows PowerShell:

    `Add-AzureAccount –Environment AzureChinaCloud`

3.  Vytvoření úložiště kontext pro účet AzureChinaCloud:

        $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureChinaCloud

Úložiště Azure pomocí služby [Azure vládní organizace](https://azure.microsoft.com/features/gov/), by měl definovat nové prostředí a vytvoření nové místní úložiště s toto prostředí:

1.  Spusťte rutinu [Get-AzureEnvironment](https://msdn.microsoft.com/library/azure/dn790368.aspx) zobrazíte dostupná Azure prostředí:

    `Get-AzureEnvironment`

2.  Přidání účtu Azure nám pro státní správu prostředí Windows PowerShell:

    `Add-AzureAccount –Environment AzureUSGovernment`

3.  Vytvoření úložiště kontext pro účet AzureUSGovernment:

        $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureUSGovernment

Další informace najdete v tématu:

- [Příručka pro vývojáře pro státní správu Microsoft Azure](../azure-government-developer-guide.md).
- [Základní informace o rozdíly při vytváření aplikace v Číně služby](https://msdn.microsoft.com/library/azure/dn578439.aspx)

## <a name="next-steps"></a>Další kroky
V této příručce jste se naučili Správa úložiště Azure pomocí prostředí PowerShell Azure. Tady jsou některé související články a zdroje pro dozvědět více o nich znáte.

- [Si přečtěte následující dokumentaci Azure úložiště](https://azure.microsoft.com/documentation/services/storage/)
- [Rutiny prostředí PowerShell Azure úložiště](http://msdn.microsoft.com/library/azure/dn806401.aspx)
- [Informace o prostředí Windows PowerShell](https://msdn.microsoft.com/library/ms714469.aspx)

[Image1]: ./media/storage-powershell-guide-full/Subscription_currentportal.png
[Image2]: ./media/storage-powershell-guide-full/Subscription_Previewportal.png
[Image3]: ./media/storage-powershell-guide-full/Blobdownload.png


[Getting started with Azure Storage and PowerShell in 5 minutes]: #getstart
[Prerequisites for using Azure PowerShell with Azure Storage]: #pre
[How to manage storage accounts in Azure]: #manageaccount
[How to set a default Azure subscription]: #setdefsub
[How to create a new Azure storage account]: #createaccount
[How to set a default Azure storage account]: #defaultaccount
[How to list all Azure storage accounts in a subscription]: #listaccounts
[How to create an Azure storage context]: #createctx
[How to manage Azure blobs and blob snapshots]: #manageblobs
[How to create a container]: #container
[How to upload a blob into a container]: #uploadblob
[How to download blobs from a container]: #downblob
[How to copy blobs from one storage container to another]: #copyblob
[How to delete a blob]: #deleteblob
[How to manage Azure blob snapshots]: #manageshots
[How to create a blob snapshot]: #createshot
[How to list snapshots of a blob]: #listshot
[How to copy a snapshot of a blob]: #copyshot
[How to manage Azure tables and table entities]: #managetables
[How to create a table]: #createtable
[How to retrieve a table]: #gettable
[How to delete a table]: #remtable
[How to manage table entities]: #mngentity
[How to add table entities]: #addentity
[How to query table entities]: #queryentity
[How to delete table entities]: #deleteentity
[How to manage Azure queues and queue messages]: #managequeues
[How to create a queue]: #createqueue
[How to retrieve a queue]: #getqueue
[How to delete a queue]: #remqueue
[How to manage queue messages]: #mngqueuemsg
[How to insert a message into a queue]: #addqueuemsg
[How to de-queue at the next message]: #dequeuemsg
[How to manage Azure file shares and files]: #files
[How to set and query storage analytics]: #stganalytics
[How to manage Shared Access Signature (SAS) and Stored Access Policy]: #sas
[How to use Azure Storage for U.S. government and Azure China]: #gov
[Next Steps]: #next
