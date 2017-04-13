<properties
    pageTitle="Použití úložiště objektů Blob z Xamarin | Microsoft Azure"
    description="Knihovnu Azure úložiště klienta pro Xamarin umožňuje vývojářům vytvářet iOS, Android a Windows Store aplikace s jejich nativní uživatelského rozhraní. Tento kurz ukazuje, jak používat Xamarin k vytvoření aplikace, která používá úložiště objektů Blob Azure."
    services="storage"
    documentationCenter="xamarin"
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/08/2016"
    ms.author="micurd"/>

# <a name="how-to-use-blob-storage-from-xamarin"></a>Použití úložiště objektů Blob z Xamarin

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

## <a name="overview"></a>Základní informace

Umožňuje vývojářům Xamarin použít sdílené C# codebase vytvořit iOS, Android a Windows Store aplikace s jejich nativní uživatelského rozhraní. Tento kurz se dozvíte, jak pomocí aplikace Xamarin úložiště objektů Blob Azure. Pokud chcete získat další informace o úložišti Azure před začte kód, najdete v článku [Úvod k úložišti tabulek Microsoft Azure](storage-introduction.md).

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-mobile-authentication-guidance](../../includes/storage-mobile-authentication-guidance.md)]

## <a name="create-a-new-xamarin-application"></a>Vytvořit novou aplikaci Xamarin

Pro tento Začínáme, jsme budete vytvářet aplikace, která používá Android, iOS a Windows. Tato aplikace jednoduše vytvořit kontejner a nahrát objektů blob do tento kontejner. Budeme používat Visual Studio v systému Windows pro zahájení práce, ale stejného learnings lze použít při vytváření aplikace pomocí Xamarin Studio v systému Mac OS.

Tímto postupem vytvoření aplikace:

1. Pokud jste to ještě neudělali, stáhněte si a nainstalujte [Xamarin for Visual Studio](https://www.xamarin.com/download).
2. Otevřete aplikaci Visual Studio a vytvořte z prázdné aplikace (nativní sdílená): **Soubor > Nový > Projekt > různé platformy > prázdný App(Native Shared)**.
3. Klikněte pravým tlačítkem myši řešení v podokně Průzkumník řešení a vyberte **Spravovat NuGet balíčků řešení**. Vyhledejte **WindowsAzure.Storage** a nainstalujte nejnovější verzi stabilní pro všechny projekty na řešení.
4. Vytvoření a spuštění svého projektu.

Teď byste měli mít aplikace, která umožňuje klikněte na tlačítko, které zvýší čítač.

> [AZURE.NOTE] Knihovnu Azure úložiště klienta pro Xamarin v současné době podporuje následující typy projektu: nativní sdílené Xamarin.Forms sdílené, Xamarin.Android a Xamarin.iOS.

## <a name="create-container-and-upload-blob"></a>Vytvoření kontejneru a nahrajte objektů blob

Pak přidáte některé kód sdílené třídy `MyClass.cs` , která vytvoří kontejneru a odešle objektů blob do tento kontejner. `MyClass.cs`by měl vypadat takto:

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    using System.Threading.Tasks;

    namespace XamarinApp
    {
        public class MyClass
        {
            public MyClass ()
            {
            }

            public static async Task createContainerAndUpload()
            {
                // Retrieve storage account from connection string.
                CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here");

                // Create the blob client.
                CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

                // Retrieve reference to a previously created container.
                CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

                // Create the container if it doesn't already exist.
                await container.CreateIfNotExistsAsync();

                // Retrieve reference to a blob named "myblob".
                CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

                // Create the "myblob" blob with the text "Hello, world!"
                await blockBlob.UploadTextAsync("Hello, world!");
            }
        }
    }

Zkontrolujte, že "your_account_name_here" a "your_account_key_here" nahraďte skutečný název svého účtu a klíč účtu. Pak můžete použít tento sdílené třídy v iOS, Android a Windows Phone aplikace. Můžete je jednoduše přidat `MyClass.createContainerAndUpload()` pro jednotlivé projekty. Příklad:

### <a name="xamarinappdroid--mainactivitycs"></a>XamarinApp.Droid > MainActivity.cs

    using Android.App;
    using Android.Widget;
    using Android.OS;

    namespace XamarinApp.Droid
    {
        [Activity (Label = "XamarinApp.Droid", MainLauncher = true, Icon = "@drawable/icon")]
        public class MainActivity : Activity
        {
            int count = 1;

            protected override async void OnCreate (Bundle bundle)
            {
                base.OnCreate (bundle);

                // Set our view from the "main" layout resource
                SetContentView (Resource.Layout.Main);

                // Get our button from the layout resource,
                // and attach an event to it
                Button button = FindViewById<Button> (Resource.Id.myButton);

                button.Click += delegate {
                    button.Text = string.Format ("{0} clicks!", count++);
                };

              await MyClass.createContainerAndUpload();
            }
        }
    }

### <a name="xamarinappios--viewcontrollercs"></a>XamarinApp.iOS > ViewController.cs

    using System;
    using UIKit;

    namespace XamarinApp.iOS
    {
        public partial class ViewController : UIViewController
        {
            int count = 1;

            public ViewController (IntPtr handle) : base (handle)
            {
            }

            public override async void ViewDidLoad ()
            {
                base.ViewDidLoad ();
                // Perform any additional setup after loading the view, typically from a nib.
                Button.AccessibilityIdentifier = "myButton";
                Button.TouchUpInside += delegate {
                    var title = string.Format ("{0} clicks!", count++);
                    Button.SetTitle (title, UIControlState.Normal);
                };

                await MyClass.createContainerAndUpload();
            }

            public override void DidReceiveMemoryWarning ()
            {
                base.DidReceiveMemoryWarning ();
                // Release any cached data, images, etc that aren't in use.
            }
        }
    }

### <a name="xamarinappwinphone--mainpagexaml--mainpagexamlcs"></a>XamarinApp.WinPhone > MainPage.xaml > MainPage.xaml.cs

    using Windows.UI.Xaml.Controls;
    using Windows.UI.Xaml.Navigation;

    // The Blank Page item template is documented at http://go.microsoft.com/fwlink/?LinkId=391641

    namespace XamarinApp.WinPhone
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            int count = 1;

            public MainPage()
            {
                this.InitializeComponent();

                this.NavigationCacheMode = NavigationCacheMode.Required;
            }

            /// <summary>
            /// Invoked when this page is about to be displayed in a Frame.
            /// </summary>
            /// <param name="e">Event data that describes how this page was reached.
            /// This parameter is typically used to configure the page.</param>
            protected override async void OnNavigatedTo(NavigationEventArgs e)
            {
                // TODO: Prepare page for display here.

                // TODO: If your application contains multiple pages, ensure that you are
                // handling the hardware Back button by registering for the
                // Windows.Phone.UI.Input.HardwareButtons.BackPressed event.
                // If you are using the NavigationHelper provided by some templates,
                // this event is handled for you.
                Button.Click += delegate {
                    var title = string.Format("{0} clicks!", count++);
                    Button.Content = title;
                };

                await MyClass.createContainerAndUpload();
            }
        }
    }


## <a name="run-the-application"></a>Spusťte aplikaci

Teď můžete spuštění této aplikace v emulátor Android a Windows Phone. Můžete taky spuštění této aplikace v iOS emulátoru, ale bude to vyžadovat Mac. Podrobné pokyny o tom, jak to udělat přečtěte si v dokumentaci k [připojení Visual Studio pro Mac](https://developer.xamarin.com/guides/ios/getting_started/installation/windows/connecting-to-mac/)

Po spuštění aplikace budou vytvořit kontejner `mycontainer` ve vašem účtu úložiště. By měl obsahovat objektů blob, `myblob`, který obsahuje text, `Hello, world!`. Ověřit to můžete pomocí [Průzkumníka úložiště Azure](http://storageexplorer.com/).

## <a name="next-steps"></a>Další kroky

V této příručce Začínáme, se naučíte vytvořit aplikaci různé platformy v Xamarin, který používá Azure úložiště. Tento Začínáme konkrétně zaměření na jeden scénář v úložišti objektů Blob. Však můžete udělat mnohem víc s nejen úložiště objektů Blob, ale také pomocí tabulky, soubor a úložiště fronty. Zkontrolujte si další informace v následujících článcích:
- [Začínáme s úložišti objektů Blob Azure pomocí .NET](storage-dotnet-how-to-use-blobs.md)
- [Začínáme s úložiště tabulek Azure pomocí .NET](storage-dotnet-how-to-use-tables.md)
- [Začínáme s úložištěm fronty Azure pomocí .NET](storage-dotnet-how-to-use-queues.md)
- [Začínáme s Azure úložiště souborů v systému Windows](storage-dotnet-how-to-use-files.md)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]
