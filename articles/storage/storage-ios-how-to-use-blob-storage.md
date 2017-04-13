<properties
    pageTitle="Použití úložiště objektů Blob Azure z iOS | Microsoft Azure"
    description="Obsahují Nestrukturovaná data v cloudu pomocí úložiště objektů Blob Azure (objektu úložiště)."
    services="storage"
    documentationCenter="ios"
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="micurd"/>

# <a name="how-to-use-blob-storage-from-ios"></a>Použití úložiště objektů Blob v iOS

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Základní informace

Tento článek vám ukáže, jak provádět běžné scénáře pomocí úložiště objektů Blob Microsoft Azure. Vzorky jsou napsané v jazyce C cíle a používat [Azure úložiště klienta knihovny pro iOS](https://github.com/Azure/azure-storage-ios). Scénáře nichž se uplatní zahrnují **nahrávání** **záznam**, **stažení**a **Odstranění** objektů BLOB. Další informace o objektů BLOB naleznete v části [Další kroky](#next-steps) . Taky můžete si stáhnout [Ukázkový aplikaci](https://github.com/Azure/azure-storage-ios/tree/master/BlobSample) můžete rychle zobrazit využívání Azure úložiště v aplikaci iOS.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="import-the-azure-storage-ios-library-into-your-application"></a>Import úložišti Azure iOS knihovny do aplikace

Knihovna iOS Azure úložiště můžete importovat do aplikace pomocí [CocoaPod úložiště Azure](https://cocoapods.org/pods/AZSClient) nebo importem souboru **Framework** .

## <a name="cocoapod"></a>CocoaPod

1. Pokud jste to ještě neudělali, [Nainstalujte CocoaPods](https://guides.cocoapods.org/using/getting-started.html#toc_3) na svém počítači otevřete okno terminálu a spuštěním následujícího příkazu

        sudo gem install cocoapods

2. Další v adresáři projektu (obsahující adresáře vaší `.xcodeproj` soubor), vytvoříte nový soubor s názvem `Podfile`(bez přípony souboru). Přidání podle následujících pokynů `Podfile` a uložení

        pod 'AZSClient'

3. V okně terminálu přejděte do adresáře projektu a spusťte tento příkaz

        pod install

4. Pokud vaše `.xcodeproj` je otevřena v Xcode, zavřete jej. V adresáři vašeho projektu otevřete soubor nově vytvořený projekt, který bude mít `.xcworkspace` rozšíření. Toto je soubor, který budete pracujete z pro schůzku.

## <a name="framework"></a>Rámec
Abyste mohli používat knihovnu iOS Azure úložiště, bude musíte nejdřív vytvořit soubor framework.

1. Nejdřív si stáhnout nebo klonovat [repo azure úložiště ios](https://github.com/azure/azure-storage-ios).

2. Přejděte do *azure úložiště ios* -> *knihovny* -> *Azure úložiště klienta knihovny*a otevřete `AZSClient.xcodeproj` v Xcode.

3. V levém horním rohu Xcode změňte aktivní schéma z "Azure úložiště klienta knihovny" na "Framework".

4. Vytvoření projektu (⌘ + B). Tím vytvoříte `AZSClient.framework` soubor na plochu.

Soubor framework je pak můžete importovat do aplikace následujícím způsobem:

1. Vytvoření nového projektu nebo otevřete existující projektu v Xcode.

2. Klikněte na projektu v levém navigačním panelu a klikněte na kartě *Obecné* v horní části editoru projektu.

3. V části *propojené rámce a knihovny* klikněte na tlačítko Přidat (+).

4. Klikněte na *Přidat další …*. Přejděte na a přidat `AZSClient.framework` souboru jste právě vytvořili.

5. V části *propojené rámce a knihovny* klikněte znovu na tlačítko Přidat (+).

6. V seznamu knihoven, abyste předtím zadali, vyhledejte `libxml2.2.dylib` a přiřadit ji někomu projektu.

7. Klikněte na kartu *Vytvoření nastavení* v horní části editoru projektu.

8. V části *Cesty hledání* poklikejte na *Cesty hledání Framework* a zadejte cestu k vaší `AZSClient.framework` soubor.

## <a name="import-statement"></a>Příkaz Import
Je třeba zahrnout následující příkaz Importovat soubor, ve které chcete volat rozhraní API úložiště Azure.

    // Include the following import statement to use blob APIs.
    #import <AZSClient/AZSClient.h>

[AZURE.INCLUDE [storage-mobile-authentication-guidance](../../includes/storage-mobile-authentication-guidance.md)]

## <a name="asynchronous-operations"></a>Asynchronní operace
> [AZURE.NOTE] Všechny provádějící požadavek na službu způsoby asynchronních operací. Ve vzorcích kódu zjistíte, že rutiny dokončení těchto postupů. Kód uvnitř rutiny dokončení se spustí **Po** prováděny žádost. Kód po **dokončení rutiny se spustí žádost** se provádí.

## <a name="create-a-container"></a>Vytvoření kontejneru
Každý blob Azure úložiště musí nacházet v kontejneru. Následující příklad ukazuje, jak vytvořit kontejner, s názvem *newcontainer*ve vašem účtu úložiště, pokud ho ještě neexistuje. Při výběru název pro váš kontejner, mějte na paměti pravidel pojmenování uvedených výše.

    -(void)createContainer{
      NSError *accountCreationError;

      // Create a storage account object from a connection string.
      AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

      if(accountCreationError){
         NSLog(@"Error in creating account.");
      }

      // Create a blob service client object.
      AZSCloudBlobClient *blobClient = [account getBlobClient];

      // Create a local container object.
      AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"newcontainer"];

      // Create container in your Storage account if the container doesn't already exist
      [blobContainer createContainerIfNotExistsWithCompletionHandler:^(NSError *error, BOOL exists) {
         if (error){
             NSLog(@"Error in creating container.");
         }
      }];
    }

Kde můžete potvrdit, že to funguje, prohlížíte [Microsoft Azure úložiště Explorer](http://storageexplorer.com) a ověřit, že tento *newcontainer* je v seznamu kontejnerů účtu úložiště.

## <a name="set-container-permissions"></a>Nastavení oprávnění kontejneru
Oprávnění kontejneru nakonfigurovaná **soukromé** přístup pomocí protokolu ve výchozím nastavení. Kontejnery však zadejte několik různých možností pro přístup k kontejner:

- **Soukromé**: kontejner a objektů blob dat můžete číst jenom vlastník účtu.

- **Kulatý**: objektů Blob data v tomto kontejneru může číst prostřednictvím anonymní požadavku, ale kontejneru dat není k dispozici. Klienti nelze vytvořit výčet objektů BLOB v kontejneru prostřednictvím anonymní žádosti.

- **Kontejner**: kontejner a objektů blob dat může číst prostřednictvím anonymní žádosti. Klienti můžete vytvořit výčet objektů BLOB v kontejneru prostřednictvím anonymní požadavku, ale nelze vytvořit výčet kontejnery v rámci účtu úložiště.

Následující příklad ukazuje, jak vytvořit kontejner s **kontejneru** přístupová oprávnění, které vám umožní přístup veřejnosti, jen pro čtení pro všechny uživatele na Internetu:

    -(void)createContainerWithPublicAccess{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create container in your Storage account if the container doesn't already exist
        [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists){
            if (error){
                NSLog(@"Error in creating container.");
            }
        }];
    }

## <a name="upload-a-blob-into-a-container"></a>Nahrání objektů blob do kontejneru
Jak je uvedeno v části [objektů Blob služby koncepty](#blob-service-concepts) , úložiště objektů Blob nabízí tři různé typy objektů blob: blokovat objektů BLOB, přidat objektů BLOB a stránky objektů BLOB. V tomto okamžiku knihovnu iOS úložišti Azure podporuje pouze objektů BLOB blokovat. Ve většině případů objektů blob blok je doporučený typ, který chcete použít.

Následující příklad ukazuje, jak nahrát blok objektů blob z NSString. Pokud objektů blob se stejným názvem už existuje v tomto kontejneru, bude obsah tohoto objektů blob přepsat.

    -(void)uploadBlobToContainer{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists)
         {
             if (error){
                 NSLog(@"Error in creating container.");
             }
             else{
                 // Create a local blob object
                 AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

                 // Upload blob to Storage
                 [blockBlob uploadFromText:@"This text will be uploaded to Blob Storage." completionHandler:^(NSError *error) {
                     if (error){
                         NSLog(@"Error in creating blob.");
                     }
                 }];
             }
         }];
    }

Kde můžete potvrdit, že to funguje, prohlížíte [Microsoft Azure úložiště Explorer](http://storageexplorer.com) a ověřit, že kontejner *containerpublic*obsahuje objektů blob *sampleblob*. V tomto příkladu jsme použili kontejneru veřejné, takže můžete taky ověřit, zda fungovala tak, že přejdete na objekty BLOB URI:

    https://nameofyourstorageaccount.blob.core.windows.net/containerpublic/sampleblob

Kromě nahrávání blok objektů blob z NSString, podobně jako metody existují pro NSData, NSInputStream nebo místní soubor.

## <a name="list-the-blobs-in-a-container"></a>Seznam objektů BLOB v kontejneru
Následující příklad ukazuje, jak chcete-li zobrazit všechny objekty BLOB v kontejneru. Při provádění operaci, mějte na paměti následující parametrů:     

- **continuationToken** - token pokračování představuje, kde by měly začít operaci seznam. Pokud žádný token zobrazí seznam objektů BLOB od začátku. Libovolný počet objektů BLOB může být uveden od nuly až maximální sady. I když tato metoda vrátí nulové hodnoty, pokud `results.continuationToken` není nulová, může být objekty další blob na službu, nejsou uvedené.
- **Předpona** – můžete určit předponu objektů blob seznam. Pouze objektů BLOB, které začínají tuto předponu budou uvedené.
- **useFlatBlobListing** - uvedené v části [zásady vytváření názvů odkazující kontejnery a objekty BLOB](#naming-and-referencing-containers-and-blobs) sice schéma ploché úložiště objektů Blob služby vytvoříte hierarchii virtuální pomocí názvů objektů BLOB informacemi cestu. Však není ploché výpis aktuálně nepodporuje; To je brzy k dispozici. Nyní by měl být tato hodnota`YES`
- **blobListingDetails** – můžete určit položek, které chcete zahrnout při výpisu objekty BLOB
    - `AZSBlobListingDetailsNone`: Seznam pouze potvrzené objektů BLOB a nevrací objektů blob metadata.
    - `AZSBlobListingDetailsSnapshots`: Seznam potvrzeného objektů BLOB a objektů blob snímků.
    - `AZSBlobListingDetailsMetadata`: Metadata objektů blob získání pro každou objektů blob vrácena v seznamu.
    - `AZSBlobListingDetailsUncommittedBlobs`: Seznam potvrzené a nepotvrzené objektů BLOB.
    - `AZSBlobListingDetailsCopy`: Zahrnout kopírovat vlastnosti v seznamu.
    - `AZSBlobListingDetailsAll`: Seznam všech dostupných potvrzené objektů BLOB nepotvrzené objektů BLOB a snímky a vrátit všechny metadata a kopírování stav u těchto objektů BLOB.
- **maxResults** - maximální počet výsledků vrátit pro tuto operaci. Umožňuje není nastavit omezení -1.
- **completionHandler** - bloku kódu provádět s výsledky výrazu operaci seznam.

V tomto příkladu se provede metodu pomocné objekty zpětně volání seznamu BLOB metoda pokaždé, když je vrácena token pokračování.

    -(void)listBlobsInContainer{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        //List all blobs in container
        [self listBlobsInContainerHelper:blobContainer continuationToken:nil prefix:nil blobListingDetails:AZSBlobListingDetailsAll maxResults:-1 completionHandler:^(NSError *error) {
            if (error != nil){
                NSLog(@"Error in creating container.");
            }
        }];
    }

    //List blobs helper method
    -(void)listBlobsInContainerHelper:(AZSCloudBlobContainer *)container continuationToken:(AZSContinuationToken *)continuationToken prefix:(NSString *)prefix blobListingDetails:(AZSBlobListingDetails)blobListingDetails maxResults:(NSUInteger)maxResults completionHandler:(void (^)(NSError *))completionHandler
    {
        [container listBlobsSegmentedWithContinuationToken:continuationToken prefix:prefix useFlatBlobListing:YES blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:^(NSError *error, AZSBlobResultSegment *results) {
            if (error)
            {
                completionHandler(error);
            }
            else
            {
                for (int i = 0; i < results.blobs.count; i++) {
                    NSLog(@"%@",[(AZSCloudBlockBlob *)results.blobs[i] blobName]);
                }
                if (results.continuationToken)
                {
                    [self listBlobsInContainerHelper:container continuationToken:results.continuationToken prefix:prefix blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:completionHandler];
                }
                else
                {
                    completionHandler(nil);
                }
            }
        }];
    }


## <a name="download-a-blob"></a>Stáhněte si objektů blob

Následující příklad ukazuje, jak stáhnout objektů blob NSString objekt.

    -(void)downloadBlobToString{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create a local blob object
        AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

        // Download blob
        [blockBlob downloadToTextWithCompletionHandler:^(NSError *error, NSString *text) {
            if (error) {
                NSLog(@"Error in downloading blob");
            }
            else{
                NSLog(@"%@",text);
            }
        }];
    }

## <a name="delete-a-blob"></a>Odstranění objektů blob

Následující příklad ukazuje, jak odstranit objektů blob.

    -(void)deleteBlob{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create a local blob object
        AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob1"];

        // Delete blob
        [blockBlob deleteWithCompletionHandler:^(NSError *error) {
            if (error) {
                NSLog(@"Error in deleting blob.");
            }
        }];
    }

## <a name="delete-a-blob-container"></a>Odstranění objektů blob kontejneru

Následující příklad ukazuje, jak lze odstranit kontejner.

    -(void)deleteContainer{
      NSError *accountCreationError;

      // Create a storage account object from a connection string.
      AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

      if(accountCreationError){
         NSLog(@"Error in creating account.");
      }

      // Create a blob service client object.
      AZSCloudBlobClient *blobClient = [account getBlobClient];

      // Create a local container object.
      AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

      // Delete container
      [blobContainer deleteContainerIfExistsWithCompletionHandler:^(NSError *error, BOOL success) {
         if(error){
             NSLog(@"Error in deleting container");
         }
      }];
    }

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili používání úložiště objektů Blob v iOS, tyto odkazy vedou na další informace o knihovně iOS a služba úložiště.

- [Azure knihovně úložiště klienta pro iOS](https://github.com/azure/azure-storage-ios)
- [Azure úložiště iOS si přečtěte následující dokumentaci odkaz](http://azure.github.io/azure-storage-ios/)
- [Služby Azure úložiště rozhraní REST API](https://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Blog týmu Azure úložiště](http://blogs.msdn.com/b/windowsazurestorage)

Pokud máte nějaké otázky týkající se tuto knihovnu neváhejte pro vystavení prezentace naše [fórum MSDN Azure](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=windowsazuredata) nebo [Přetečení zásobníku](http://stackoverflow.com/questions/tagged/windows-azure-storage+or+windows-azure-storage+or+azure-storage-blobs+or+azure-storage-tables+or+azure-table-storage+or+windows-azure-queues+or+azure-storage-queues+or+azure-storage-emulator+or+azure-storage-files).
Pokud máte funkce návrhy Azure úložiště, přejděte prosím přidávat příspěvky do [Azure úložiště názory](https://feedback.azure.com/forums/217298-storage/).
