<properties
   pageTitle="Používání Data jezera úložiště .NET SDK pro vývoj aplikací | Microsoft Azure"
   description="Používání Azure Data jezera úložiště .NET SDK pro vývoj aplikací"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-net-sdk"></a>Začínáme s úložiště jezera dat Azure pomocí .NET SDK

> [AZURE.SELECTOR]
- [Portál](data-lake-store-get-started-portal.md)
- [Prostředí PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [ROZHRANÍ REST API](data-lake-store-get-started-rest-api.md)
- [Azure rozhraní příkazového řádku](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Naučte se používat [Azure Data jezera úložiště .NET SDK](https://msdn.microsoft.com/library/mt581387.aspx) k provádění základních operací, například při vytvoření složek, nahrávání a stahování datové soubory, atd. Další informace o jezera dat najdete v článku [Úložiště jezera dat Azure](data-lake-store-overview.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

* **Visual Studio 2013 nebo 2015**. Tyto kroky udělat pomocí aplikace Visual Studio 2015.

* **Azure předplatného**. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).

* **Úložiště jezera dat azure účtu**. Další informace o tom, jak vytvořit účet najdete v článku [Začínáme s úložiště jezera dat Azure](data-lake-store-get-started-portal.md)

* **Vytvoření aplikace služby Azure Active Directory**. Ověření dat jezera úložiště aplikaci Azure AD pomocí aplikace Azure AD. Způsoby různých ověření s Azure AD, které jsou **ověření koncových uživatelů** nebo **služeb**. Pokyny a další informace o tom, jak ověřit najdete v tématu [Ověřit s úložiště jezera dat pomocí služby Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="create-a-net-application"></a>Vytvořit aplikaci pro .NET

1. Otevřete aplikaci Visual Studio a vytvořte aplikace konzoly.

2. V nabídce **soubor** klikněte na **Nový**a potom klikněte na **projekt**.

3. Z **Nového projektu**zadejte nebo vyberte následující hodnoty:

  	| Vlastnost | Hodnota                       |
  	|----------|-----------------------------|
  	| Kategorie | Šablony/Visual Basic a Windows |
  	| Šablony | Aplikace konzoly         |
  	| Jméno     | CreateADLApplication        |

4. Klikněte na **OK** vytvořte projekt.

5. Přidání balíčků Nuget do projektu.

    1. Klikněte pravým tlačítkem myši na název projektu v Průzkumníku řešení a klikněte na **Spravovat balíčků NuGet**.
    2. Na kartě **Správce balíčků Nuget** zkontrolujte **balíčku zdroje** je nastavený na **nuget.org** a je zaškrtnuté políčko, které **obsahují zkušební** .
    3. Vyhledat a nainstalovat následující balíčky NuGet:

        * `Microsoft.Azure.Management.DataLake.Store`-Tento kurz používá v0.12.5 náhled.
        * `Microsoft.Azure.Management.DataLake.StoreUploader`-Tento kurz používá v0.10.6 náhled.
        * `Microsoft.Rest.ClientRuntime.Azure.Authentication`-Tento kurz používá v2.2.8 náhled.

        ![Přidat zdroj Nuget] (./media/data-lake-store-get-started-net-sdk/ADL.Install.Nuget.Package.png "Vytvořit nový účet jezera dat Azure")

    4. Zavřete dialogové okno **Správce Nuget balíčků**.

6. Otevřete **Program.cs**, odstranit existující kód a potom použít následující příkazy přidat odkazy na obory názvů.

        using System;
        using System.Threading;
        
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.StoreUploader;

7. Deklarovat proměnných, jak je ukázáno v následujícím příkladu a poskytují hodnoty pro úložiště jezera dat název a název skupiny zdroje, která již existuje. Nezapomeňte místní cestu a název souboru, který tady zadáte musí existovat v počítači. Přidejte následující fragment kódu po deklarace oboru názvů.

        namespace SdkSample
        {
            class Program
            {
                private static DataLakeStoreAccountManagementClient _adlsClient;
                private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;
                
                private static string _adlsAccountName;
                private static string _resourceGroupName;
                private static string _location;
                private static string _subId;

                
                private static void Main(string[] args)
                {
                    _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; // TODO: Replace this value with the name of your existing Data Lake Store account.
                    _resourceGroupName = "<RESOURCE-GROUP-NAME>"; // TODO: Replace this value with the name of the resource group containing your Data Lake Store account.
                    _location = "East US 2";
                    _subId = "<SUBSCRIPTION-ID>";
                    
                    string localFolderPath = @"C:\local_path\"; // TODO: Make sure this exists and can be overwritten.
                    string localFilePath = localFolderPath + "file.txt"; // TODO: Make sure this exists and can be overwritten.
                    string remoteFolderPath = "/data_lake_path/";
                    string remoteFilePath = remoteFolderPath + "file.txt";
                }
            }
        }

Ve zbývající části tohoto článku můžete zjistěte, jak používat k dispozici způsobů .NET k provádění operací, jako je ověřování, nahrávání souboru atd.

## <a name="authentication"></a>Ověřování

### <a name="if-you-are-using-end-user-authentication-recommended-for-this-tutorial"></a>Pokud používáte koncových uživatelů ověřování (doporučeno pro účely tohoto návodu)

To pomocí existujícího Azure AD "Native Client –" aplikace. jednu navrhovaný dole. Aby se tento kurz rychleji, doporučujeme že použít tento postup.

    // User login via interactive popup
    // Use the client ID of an existing AAD "Native Client" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "common"; // Replace this string with the user's Azure Active Directory tenant ID or domain name, if needed.
    var nativeClientApp_clientId = "1950a258-227b-4e31-a9cf-717495945fc2";
    var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(nativeClientApp_clientId, new Uri("urn:ietf:wg:oauth:2.0:oob"));
    var creds = UserTokenProvider.LoginWithPromptAsync(domain, activeDirectoryClientSettings).Result;

Několik co je třeba vědět o této fragment výše.

* Umožňují rychlejší výukového, používá tento fragment an, chyby Azure AD domény a klient ID, které je k dispozici ve výchozím nastavení pro všechna předplatná Azure. Ano, můžete to udělat **použít tento fragment jako-je v aplikaci**.
* Však pokud chcete používat vlastní Azure AD domény a aplikace ID klienta, musíte vytvořit nativní aplikaci Azure AD a potom použijte domain Azure AD ID klienta a přesměrovat URI pro aplikaci, kterou jste vytvořili. Postupujte podle pokynů uvedených v tématu [Vytvoření aplikace Active Directory](../resource-group-create-service-principal-portal.md#create-an-active-directory-application) .

>[AZURE.NOTE] Pokyny v výše odkazy jsou určené pro webovou aplikaci Azure AD. Ale kroky se přesně shodují i v případě, že jste zvolili vytvoření aplikace native client – místo. 

### <a name="if-you-are-using-service-to-service-authentication-with-client-secret"></a>Pokud používáte k Služba ověřování s tajná klienta 

Následující úryvek mohou sloužit k ověření aplikace interaktivně, pomocí klienta tajné / klíč pro jistinu aplikace a služby. Pomocí tohoto příkazu s existujícím [Azure AD "v prohlížeči" aplikace](../resource-group-create-service-principal-portal.md).

    // Service principal / appplication authentication with client secret / key
    // Use the client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    var clientSecret = "<AAD-application-client-secret>";
    var clientCredential = new ClientCredential(webApp_clientId, clientSecret);
    var creds = ApplicationTokenProvider.LoginSilentAsync(domain, clientCredential).Result;

### <a name="if-you-are-using-service-to-service-authentication-with-certificate"></a>Pokud používáte službu služby ověřování pomocí certifikátu

Jako třetí možnost následující úryvek mohou sloužit k ověření aplikace interaktivně, pomocí certifikátu pro aplikace a služby jistinu. Pomocí tohoto příkazu s existujícím [Azure AD "v prohlížeči" aplikace](../resource-group-create-service-principal-portal.md).

    // Service principal / application authentication with certificate
    // Use the client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    System.Security.Cryptography.X509Certificates.X509Certificate2 clientCert = <AAD-application-client-certificate>
    var clientAssertionCertificate = new ClientAssertionCertificate(webApp_clientId, clientCert);
    var creds = ApplicationTokenProvider.LoginSilentWithCertificateAsync(domain, clientAssertionCertificate).Result;

## <a name="create-client-objects"></a>Vytvoření klientů objektům

Následující úryvek vytvoří úložiště jezera dat účtu a systému souborů klienta objekty, které se používají k problému žádosti o služby.

    // Create client objects and set the subscription ID
    _adlsClient = new DataLakeStoreAccountManagementClient(creds);
    _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(creds);

    _adlsClient.SubscriptionId = _subId;

## <a name="list-all-data-lake-store-accounts-within-a-subscription"></a>Seznam všech účtů úložiště jezera dat v rámci předplatného

Následující úryvek obsahuje seznam všech účtů úložiště jezera dat v rámci dané Azure předplatné.

    // List all ADLS accounts within the subscription
    public static List<DataLakeStoreAccount> ListAdlStoreAccounts()
    {
        var response = _adlsClient.Account.List();
        var accounts = new List<DataLakeStoreAccount>(response);
        
        while (response.NextPageLink != null)
        {
            response = _adlsClient.Account.ListNext(response.NextPageLink);
            accounts.AddRange(response);
        }
        
        return accounts;
    }

## <a name="create-a-directory"></a>Vytvoření adresáře

Následující úryvek `CreateDirectory` metodu, která slouží k vytvoření adresáře v úložišti jezera účtu.

    // Create a directory
    public static void CreateDirectory(string path)
    {
        _adlsFileSystemClient.FileSystem.Mkdirs(_adlsAccountName, path);
    }

## <a name="upload-a-file"></a>Odeslání souboru

Následující úryvek `UploadFile` metodu, která slouží k nahrání souborů na úložiště jezera dat účtu.

    // Upload a file
    public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
    {
        var parameters = new UploadParameters(srcFilePath, destFilePath, _adlsAccountName, isOverwrite: force);
        var frontend = new DataLakeStoreFrontEndAdapter(_adlsAccountName, _adlsFileSystemClient);
        var uploader = new DataLakeStoreUploader(parameters, frontend);
        uploader.Execute();
    }

`DataLakeStoreUploader`podporuje rekurzivní nahrávání a stahování mezi místním souborovou cestu a cestu k souboru jezera úložiště.    

## <a name="get-file-or-directory-info"></a>Získat informace o souboru nebo složky

Následující úryvek `GetItemInfo` metodu, která slouží k načtení informací o souboru nebo složky v úložišti jezera. 

    // Get file or directory info
    public static FileStatusProperties GetItemInfo(string path)
    {
        return _adlsFileSystemClient.FileSystem.GetFileStatus(_adlsAccountName, path).FileStatus;
    }

## <a name="list-file-or-directories"></a>Soubor se seznamem nebo adresáře

Následující úryvek `ListItem` metody, můžete použít k vytvoření seznamu souborů a adresáře v úložišti jezera účtu.

    // List files and directories
    public static List<FileStatusProperties> ListItems(string directoryPath)
    {
        return _adlsFileSystemClient.FileSystem.ListFileStatus(_adlsAccountName, directoryPath).FileStatuses.FileStatus.ToList();
    }

## <a name="concatenate-files"></a>CONCATENATE souborů

Následující úryvek `ConcatenateFiles` metody, použili ke zřetězení soubory. 

    // Concatenate files
    public static void ConcatenateFiles(string[] srcFilePaths, string destFilePath)
    {
        _adlsFileSystemClient.FileSystem.Concat(_adlsAccountName, destFilePath, srcFilePaths);
    }

## <a name="append-to-a-file"></a>Připojení k souboru

Následující úryvek `AppendToFile` způsob data připojit k souboru uloženého v úložišti jezera účtu.

    // Append to file
    public static void AppendToFile(string path, string content)
    {
        var stream = new MemoryStream(Encoding.UTF8.GetBytes(content));
        
        _adlsFileSystemClient.FileSystem.Append(_adlsAccountName, path, stream);
    }

## <a name="download-a-file"></a>Stažení souboru

Následující úryvek `DownloadFile` metody, které používáte pro stažení souboru z úložiště jezera dat účtu.

    // Download file
    public static void DownloadFile(string srcPath, string destPath)
    {
        var stream = _adlsFileSystemClient.FileSystem.Open(_adlsAccountName, srcPath);
        var fileStream = new FileStream(destPath, FileMode.Create);
        
        stream.CopyTo(fileStream);
        fileStream.Close();
        stream.Close();
    }

## <a name="next-steps"></a>Další kroky

- [Zabezpečení dat v úložišti jezera dat](data-lake-store-secure-data.md)
- [Použití analýzy jezera Azure dat s jezera úložiště dat](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Pomocí úložiště jezera dat Azure HDInsight](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Úložiště jezera dat .NET SDK Reference](https://msdn.microsoft.com/library/mt581387.aspx)
- [Odkaz ZBÝVAJÍCÍ jezera úložiště dat](https://msdn.microsoft.com/library/mt693424.aspx)
