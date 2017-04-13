<properties
    pageTitle="Kurz: Šifrování a dešifrování objektů BLOB v úložišti tabulek Microsoft Azure pomocí Azure klíč trezoru | Microsoft Azure"
    description="Tento kurz provede vás jak postup pro šifrování a dešifrování objektů blob pomocí klienta šifrování úložišti tabulek Microsoft Azure s Azure klíč trezoru."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="required"
    ms.date="10/18/2016"
    ms.author="lakasa;robinsh"/>

# <a name="tutorial-encrypt-and-decrypt-blobs-in-microsoft-azure-storage-using-azure-key-vault"></a>Kurz: Šifrování a dešifrování objektů BLOB v úložišti tabulek Microsoft Azure pomocí trezoru klíč Azure

## <a name="introduction"></a>Úvod

Tento kurz popisuje, jak lze použít šifrování klientskou úložišť s Azure klíč trezoru. Ho provede vás jak postup pro šifrování a dešifrování objektů blob v aplikaci konzoly pomocí těchto technologií.

**Předpokládaná doba dokončete:** 20 minut

Základní informace o trezoru klíč Azure, najdete v článku [Co je Azure klíč trezoru?](../key-vault/key-vault-whatis.md).

Základní informace o šifrování klienta služby Azure úložiště najdete v článku [šifrování na straně klienta a Azure trezoru klíč pro úložišti tabulek Microsoft Azure](storage-client-side-encryption.md).


## <a name="prerequisites"></a>Zjistit předpoklady pro

Tento kurz musí mít takto:

- Účet Azure úložiště
- Visual Studio 2013 nebo novější
- Azure Powershellu


## <a name="overview-of-client-side-encryption"></a>Základní informace o šifrování na straně klienta

Základní informace o šifrování klienta služby Azure úložiště najdete v článku [šifrování na straně klienta a Azure trezoru klíč pro úložišti tabulek Microsoft Azure](storage-client-side-encryption.md)

Tady je stručný popis fungování šifrování na straně klienta:

1. Azure úložiště klienta SDK vygeneruje obsahu šifrovací klíč (CEK), která je pomocí jednoho úvazek symetrickou klávesy.
2. Data o zákaznících je šifrovaná pomocí tohoto CEK.
3. CEK je pak omotané (šifrovaná) pomocí klíčových šifrovací klíč (KEK). KEK je určen identifikátoru klíče a mohou být asymetrické pár klíčů nebo symetrickou klíče a můžete být spravované místně nebo uloženy v Azure klíč trezoru. Úložiště klienta samotné nikdy má přístup k KEK. Jenom vyvolá algoritmus klíčové obtékání, který poskytuje společnost klíč trezoru. Zákazníci můžete rozhodnete sdělit nám vlastní poskytovatelů klíče obtékání/obtékání případě zájmu.
4. Šifrovaná data se pak nahrané služby Azure úložiště.


## <a name="set-up-your-azure-key-vault"></a>Nastavení trezoru klíč Azure
Abyste mohli pokračovat v tomto kurzu, musíte udělat následující kroky, které jsou uvedené v tomto kurzu [Začínáme s Azure klíč trezoru](../key-vault/key-vault-get-started.md):

- Vytvoření klíčových trezoru.
- Přidejte klíč nebo tajná do klíčové trezoru.
- Registrace aplikace pomocí služby Azure Active Directory.
- Povolte aplikaci pro použití klávesy nebo tajná.

Poznamenejte si ClientID a ClientSecret generovaný při registraci aplikace pomocí služby Azure Active Directory.

Vytvoření oba klíče klíčové trezoru. Předpokladu, že pro zbytek kurzu, že jste použili tyto názvy: ContosoKeyVault a TestRSAKey1.


## <a name="create-a-console-application-with-packages-and-appsettings"></a>Vytvoření aplikace konzoly s balíčků a AppSettings

Ve Visual Studiu vytvořte nové aplikace konzoly.

Přidejte nezbytné nuget balíčků v konzole balíčku správce.

    Install-Package WindowsAzure.Storage

    // This is the latest stable release for ADAL.
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

    Install-Package Microsoft.Azure.KeyVault
    Install-Package Microsoft.Azure.KeyVault.Extensions


Přidání AppSettings ke App.Config.

    <appSettings>
        <add key="accountName" value="myaccount"/>
        <add key="accountKey" value="theaccountkey"/>
        <add key="clientId" value="theclientid"/>
        <add key="clientSecret" value="theclientsecret"/>
        <add key="container" value="stuff"/>
    </appSettings>

Přidejte následující `using` příkazy a ujistěte se, že chcete-li přidat odkaz na System.Configuration do projektu.

    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System.Configuration;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    using Microsoft.Azure.KeyVault;
    using System.Threading;     
    using System.IO;


## <a name="add-a-method-to-get-a-token-to-your-console-application"></a>Přidání způsob, jak získat token aplikaci konzoly

Podle pokynů pro používanou klíč trezoru třídy, které je potřeba ověřit pro přístup k trezoru klíče.

    private async static Task<string> GetToken(string authority, string resource, string scope)
    {
        var authContext = new AuthenticationContext(authority);
        ClientCredential clientCred = new ClientCredential(
            ConfigurationManager.AppSettings["clientId"],
            ConfigurationManager.AppSettings["clientSecret"]);
        AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

        if (result == null)
            throw new InvalidOperationException("Failed to obtain the JWT token");

        return result.AccessToken;
    }

## <a name="access-storage-and-key-vault-in-your-program"></a>Přístup k ukládání a klíč trezoru v aplikaci

Ve funkci hlavní přidáte následující kód.

    // This is standard code to interact with Blob storage.
    StorageCredentials creds = new StorageCredentials(
        ConfigurationManager.AppSettings["accountName"],
        ConfigurationManager.AppSettings["accountKey"]);
    CloudStorageAccount account = new CloudStorageAccount(creds, useHttps: true);
    CloudBlobClient client = account.CreateCloudBlobClient();
    CloudBlobContainer contain = client.GetContainerReference(ConfigurationManager.AppSettings["container"]);
    contain.CreateIfNotExists();

    // The Resolver object is used to interact with Key Vault for Azure Storage.
    // This is where the GetToken method from above is used.
    KeyVaultKeyResolver cloudResolver = new KeyVaultKeyResolver(GetToken);


> [AZURE.NOTE] Klíčové trezoru objektové modely
>
>Je třeba porozumět tomu, že jsou ve skutečnosti dvě klíč trezoru objektové modely je potřeba vědět: jeden založenou na rozhraní REST API (KeyVault názvů) a druhý je rozšíření pro šifrování na straně klienta.

> Klient trezoru klíč bude spolupracovat pomocí rozhraní REST API a rozumí JSON Web klíče a tajemství pro dva typy věcí, které jsou součástí klíč trezoru.

> Rozšíření trezoru klíč jsou třídy, které vypadá přesně vytvořen pro šifrování na straně klienta v úložišti Azure. Obsahují rozhraní pro klávesy (IKey) a třídy podle koncepci překládání klíče. Existují dva implementace IKey, které byste měli vědět: RSAKey a SymmetricKey. Teď probíhají se shoduje s věcí, které jsou součástí trezoru klíče, ale v tomto okamžiku jsou nezávislé třídy (za klávesu spolu s tajná načítané klienta trezoru klíč není implementace IKey).


## <a name="encrypt-blob-and-upload"></a>Šifrování objektů blob a odeslat
Přidejte následující kód postup pro šifrování objektů blob a nahrajte ho ke svému účtu Azure úložiště. Metoda **ResolveKeyAsync** , který se používá vrátí IKey.


    // Retrieve the key that you created previously.
    // The IKey that is returned here is an RsaKey.
    // Remember that we used the names contosokeyvault and testrsakey1.
    var rsa = cloudResolver.ResolveKeyAsync("https://contosokeyvault.vault.azure.net/keys/TestRSAKey1", CancellationToken.None).GetAwaiter().GetResult();


    // Now you simply use the RSA key to encrypt by setting it in the BlobEncryptionPolicy.
    BlobEncryptionPolicy policy = new BlobEncryptionPolicy(rsa, null);
    BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

    // Reference a block blob.
    CloudBlockBlob blob = contain.GetBlockBlobReference("MyFile.txt");

    // Upload using the UploadFromStream method.
    using (var stream = System.IO.File.OpenRead(@"C:\data\MyFile.txt"))
        blob.UploadFromStream(stream, stream.Length, null, options, null);


Následuje snímek z [Azure klasické portálu](https://manage.windowsazure.com) pro objektů blob zašifrovaná pomocí klienta šifrování s klíčem uloženým v klíč trezoru. Vlastnost **ID klíče** je URI pro klávesu trezoru klávesy, která funguje jako KEK. Vlastnost **EncryptedKey** obsahuje šifrované verzi CEK.

![Snímek obrazovky zobrazující objektů Blob metadata, která zahrnuje šifrování metadata][1]

> [AZURE.NOTE] Když se podíváte na konstruktoru BlobEncryptionPolicy, zobrazí se, že můžete přijmout klíče a/nebo překladač. Mějte na paměti vpravo teď překladač nelze použít šifrování, protože nemá nesynchronizujete podporující výchozí klíč.



## <a name="decrypt-blob-and-download"></a>Dešifrování objektů blob a stahování
Dešifrování je ale skutečně při použití tříd překládání smysl. ID klíč používaný k šifrování přidružen objektů blob v jeho metadata, není k dispozici žádné důvod, proč se při načtení klávesu a nezapomeňte spojení klíče objektů blob. Potřebujete Ujistěte se, klávesu zůstane v klíč trezoru.   

Soukromý klíč klíč RSA zůstane v trezoru klíč, takže pro dešifrování problémy klávesu zašifrovaných z objektů blob metadata, která obsahuje CEK odeslány klíč trezoru dešifrování.

Přidejte následující dešifrovat objektů blob, který jste právě uložili.

    // In this case, we will not pass a key and only pass the resolver because
    // this policy will only be used for downloading / decrypting.
    BlobEncryptionPolicy policy = new BlobEncryptionPolicy(null, cloudResolver);
    BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

    using (var np = File.Open(@"C:\data\MyFileDecrypted.txt", FileMode.Create))
        blob.DownloadToStream(np, null, options, null);


> [AZURE.NOTE] Existuje několik jiné druhy překládání usnadnit správy klíčů, včetně: AggregateKeyResolver a CachingKeyResolver.


## <a name="use-key-vault-secrets"></a>Pomocí klávesy trezoru tajemství
Způsob, jak pomocí šifrování na straně klienta tajná totiž prostřednictvím třídy SymmetricKey tajná v podstatě symetrickou klíče. Ale uvedených výše tajná klíč trezoru neodpovídají přesně SymmetricKey. Pokud chcete zjistit, několik věcí:


- Klíč v SymmetricKey musí být pevná délka: 128 192, 256, 384 nebo 512 bitů.
- Klíč v SymmetricKey by měl být ve formátu Base 64 kódování.
- Tajná trezoru klíč, který bude sloužit jako SymmetricKey musí mít typu obsahu "application/osmičkové stream" klíč trezoru.

Tady je příklad v prostředí PowerShell vytváření tajná v trezoru klíč použitý jako SymmetricKey.
Poznámka: Pevně zakódovaný hodnota $key, je za účelem ukázka pouze. Do vlastního kódu je vhodné generovat tohoto klíče.

    // Here we are making a 128-bit key so we have 16 characters.
    //  The characters are in the ASCII range of UTF8 so they are
    //  each 1 byte. 16 x 8 = 128.
    $key = "qwertyuiopasdfgh"
    $b = [System.Text.Encoding]::UTF8.GetBytes($key)
    $enc = [System.Convert]::ToBase64String($b)
    $secretvalue = ConvertTo-SecureString $enc -AsPlainText -Force

    // Substitute the VaultName and Name in this command.
    $secret = Set-AzureKeyVaultSecret -VaultName 'ContoseKeyVault' -Name 'TestSecret2' -SecretValue $secretvalue -ContentType "application/octet-stream"

V aplikaci konzoly můžete použít stejné volání jako před k načtení tento tajná jako SymmetricKey.

    SymmetricKey sec = (SymmetricKey) cloudResolver.ResolveKeyAsync(
        "https://contosokeyvault.vault.azure.net/secrets/TestSecret2/",
        CancellationToken.None).GetAwaiter().GetResult();

To je vše. Využijte!

## <a name="next-steps"></a>Další kroky

Další informace o použití úložišti tabulek Microsoft Azure s C# najdete v článku [Microsoft Azure úložiště klienta knihovny pro .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

Další informace o rozhraní REST API objektů Blob najdete v tématu [Objektů Blob služba REST API](https://msdn.microsoft.com/library/azure/dd135733.aspx).

Nejnovější informace o úložišti tabulek Microsoft Azure přejděte na [Microsoft Azure úložiště týmového blogu](http://blogs.msdn.com/b/windowsazurestorage/).


<!--Image references-->
[1]: ./media/storage-encrypt-decrypt-blobs-key-vault/blobmetadata.png
