<properties
    pageTitle="Připojení k úložišti Azure v aplikaci Xamarin.Forms"
    description="Přidání obrázků do mobilní aplikaci úkol seznamu Xamarin.Forms pomocí připojení k úložišti objektů blob Azure"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="erikre"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="connect-to-azure-storage-in-your-xamarinforms-app"></a>Připojení k úložišti Azure v aplikaci Xamarin.Forms

## <a name="overview"></a>Základní informace

Mobilní aplikace Azure klienta a serveru SDK domovské stránce podpory offline synchronizace strukturovaná data s operace CRUD u /tables koncový bod. Obecně tato data uložená v databázi nebo v podobné úložiště a obecně tyto úložiště dat nelze ukládat velké binární data efektivní. Některé aplikace obsahují také odpovídající data, která je uložený jinde (například úložiště objektů blob, sdílené soubory položky) a je užitečné, mají být k vytvoření přidružení mezi záznamy v /tables koncového bodu a dalších dat.

V tomto tématu se dozvíte, jak přidat podporu pro obrázky s rychlý úvod mobilní aplikace úkol seznamu. Nejdřív musíte nejdřív udělat kurz [Vytvoření Xamarin.Forms aplikace].

V tomto kurzu vytvoříte účet úložiště a přidání připojovací řetězec do back-end vaše mobilní aplikaci. Potom přidejte nové dědící z nového typu mobilní aplikace `StorageController<T>` na server project.

>[AZURE.TIP] Tento kurz obsahuje [companion ukázkové](https://azure.microsoft.com/documentation/samples/app-service-mobile-dotnet-todo-list-files/) k dispozici, které můžou být nasazené na Azure účtu. 

## <a name="prerequisites"></a>Zjistit předpoklady pro

* Použití výukového [vytvořit Xamarin.Forms aplikace] , které je uvedené ostatní požadavky. Tento článek používá aplikaci vyplněné z tohoto kurzu.

>[AZURE.NOTE] Pokud chcete začít pracovat s aplikaci služby Azure před registraci účet Azure, přejděte na [Zkuste aplikaci služby](https://tryappservice.azure.com/?appServiceName=mobile). Můžete okamžitě vytvořit mobilní aplikaci krátkodobý starter v aplikaci služby – bez platební kartou povinné a bez závazky.

## <a name="create-a-storage-account"></a>Vytvoření účtu úložiště

1. Vytvoření účtu úložiště pomocí následujících kurz [vytvořte účet Azure úložiště]. 

2. Na portálu Azure přejděte ke svému účtu nově vytvořený úložiště a klikněte na ikonu **klíče** . Zkopírujte **primární připojovací řetězec**.

3. Přejděte na vaše mobilní aplikace back-end. V části **Všechna nastavení** -> **Nastavení aplikace** -> **Připojovací řetězec**, vytvořte nový klíč s názvem `MS_AzureStorageAccountConnectionString` a použijte hodnotu z pole zkopírovali ze svého účtu úložiště. Použijte **vlastní** jako typ klíče.

## <a name="add-a-storage-controller-to-the-server"></a>Přidání řadiče úložiště na server

Budete muset přidat nového řadiče do projektu serveru, který bude odpověď na žádosti o token přidružení zabezpečení pro Azure úložiště, stejně jako seznam souborů, které odpovídají vrátit záznamu:

- [Přidání úložiště řadiče project serveru](#add-controller-code)
- [Směruje registrované správcem úložiště](#routes-registered)
- [Komunikace klienta a serveru](#client-communication)

###<a name="add-controller-code"></a>Přidání úložiště řadiče project serveru

1. Ve Visual Studiu otevřete projekt serveru .NET. Přidáte do Nuget [Microsoft.Azure.Mobile.Server.Files]. Ujistěte se, vyberte **Zahrnout zkušební**.

2. Ve Visual Studiu otevřete projekt serveru .NET. Klikněte pravým tlačítkem myši na složku **řadiče** a vyberte **Přidat** -> **řadiče** -> **Webového rozhraní API 2 řadiče - prázdné**. Pojmenování správce `TodoItemStorageController`.

3. Přidejte následující pomocí příkazů:

        using Microsoft.Azure.Mobile.Server.Files;
        using Microsoft.Azure.Mobile.Server.Files.Controllers;

4. Změna základní třídy pro `StorageController`:
    
        public class TodoItemStorageController : StorageController<TodoItem>

5. Přidejte předmětu následujících způsobů:

        [HttpPost]
        [Route("tables/TodoItem/{id}/StorageToken")]
        public async Task<HttpResponseMessage> PostStorageTokenRequest(string id, StorageTokenRequest value)
        {
            StorageToken token = await GetStorageTokenAsync(id, value);

            return Request.CreateResponse(token);
        }

        // Get the files associated with this record
        [HttpGet]
        [Route("tables/TodoItem/{id}/MobileServiceFiles")]
        public async Task<HttpResponseMessage> GetFiles(string id)
        {
            IEnumerable<MobileServiceFile> files = await GetRecordFilesAsync(id);

            return Request.CreateResponse(files);
        }

        [HttpDelete]
        [Route("tables/TodoItem/{id}/MobileServiceFiles/{name}")]
        public Task Delete(string id, string name)
        {
            return base.DeleteFileAsync(id, name);
        }

6. Rozhraní API webových konfiguraci nastavení směrování atribut aktualizujte. V **Startup.MobileApp.cs**, přidejte následující řádek `ConfigureMobileApp()` metoda po definici `config` proměnnou:

        config.MapHttpAttributeRoutes();

7. Server projekt publikujte do back-end vaše mobilní aplikace.

###<a name="routes-registered"></a>Směruje registrované správcem úložiště

Nové `TodoItemStorageController` poskytuje dva dílčí zdroje v části záznam spravuje:

- StorageToken

    + Nastavit informace HTTP příspěvku: Vytvoří token úložiště
    
        `/tables/TodoItem/{id}/MobileServiceFiles`
    
- MobileServiceFiles

    + HTTP GET: Načte seznam souborů přidružené k záznamu
    
        `/tables/TodoItem/{id}/MobileServiceFiles`

    + Nastavit informace HTTP odstranit: Odstraní souboru zadaného v identifikátor soubor prostředku
    
        `/tables/TodoItem/{id}/MobileServiceFiles/{fileid}`

###<a name="client-communication"></a>Komunikace klienta a serveru

Všimněte si, že `TodoItemStorageController` znamená *není* tras pro nahrávání nebo stahování objektů blob. Je proto, že k mobilnímu klientovi vzájemně objektů blob úložiště *přímo* k provedení těchto operací po první začíná token přidružení zabezpečení (sdílené přístup podpis) zabezpečený přístup k určité objektů blob nebo kontejner. To je důležité architektonických návrhů, protože jinak přístup k základnímu úložišti by být omezený škálovatelnost a dostupnost mobilních back-end. Místo toho přímé připojení k úložišti Azure, mobilního klienta můžete využít výhod jeho funkce, například automatického rozdělení a geo rozdělení.

Podpis sdílený přístup poskytuje delegované přístupu k prostředkům ve vašem účtu úložiště. To znamená, že udělit klienta omezené oprávnění k objektům ve vašem účtu úložiště stanovený počet minut a zadaný sadu oprávnění, aniž by bylo nutné sdílet svůj účet přístupové klávesy. Další informace najdete v tématu [Principy sdílené přístup podpisy].

Následující obrázek ukazuje interakcí klienta a serveru. Před odesláním souboru, klient požaduje token přidružení zabezpečení ze služby. Služba používá připojovací řetězec úložiště ke generování nová přidružení zabezpečení, která vrátí hodnotu k desktopovému klientovi. Přidružení zabezpečení časově omezené a omezením oprávnění k jenom příslušný soubor nebo kontejner. Mobilních klientů pomocí tohoto přidružení zabezpečení a Azure úložiště klienta SDK nahrát soubor k úložišti objektů blob.

![Požadování token přidružení zabezpečení](./media/app-service-mobile-xamarin-forms-blob-storage/storage-token-diagram.png)

## <a name="update-your-client-app-to-add-image-support"></a>Aktualizace aplikace klienta přidáte obrázek podpory

Otevřete projekt rychlý úvod Xamarin.Forms ve Visual Studiu nebo Xamarin Studio. Chcete nainstalovat Nuget balíčků a aktualizovat projekt portable knihovny a iOS, Android a Windows klientských projektů:

- [Přidání Nuget balíčků](#add-nuget)
- [Přidání IPlatform rozhraní](#add-iplatform)
- [Přidání FileHelper třídy](#add-filehelper)
- [Přidání rutiny synchronizaci souborů](#file-sync-handler)
- [Aktualizace TodoItemManager](#update-todoitemmanager)
- [Přidání zobrazení podrobností o](#add-details-view)
- [Aktualizace hlavního zobrazení](#update-main-view)
- [Aktualizovat Android projektu](#update-android), [iOS project](#update-ios) [Windows projektu](#update-windows)

>[AZURE.NOTE] Tento kurz obsahuje pouze pokyny pro Android, iOS a platformách pro Windows Store, ne Windows Phone.

###<a name="add-nuget"></a>Přidání Nuget balíčků

Klikněte pravým tlačítkem myši na řešení a vyberte **Spravovat Nuget balíčků řešení**. Přidejte následující balíčky Nuget pro **všechny** projekty na řešení. Nezapomeňte **Zahrnout zkušební**.

  - [Microsoft.Azure.Mobile.Client.Files]

  - [Microsoft.Azure.Mobile.Client.SQLiteStore]

  - [PCLStorage]

Pro usnadnění v tomto příkladu knihovnu [PCLStorage] , ale není povinné klientem Azure mobilní aplikace SDK.

[PCLStorage]: https://www.nuget.org/packages/PCLStorage/

###<a name="add-iplatform"></a>Přidání IPlatform rozhraní

Vytvoření nového rozhraní `IPlatform` v projektu hlavní portable knihovny. To následující [Xamarin.Forms DependencyService] načíst třídě vpravo specifické pro platformu za běhu. Specifické pro platformu implementace přidáte později ve všech klientských projektů.

1. Přidejte následující pomocí příkazů:

        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Sync;

2. Nahraďte provedení těchto věcí:

        public interface IPlatform
        {
            Task <string> GetTodoFilesPathAsync();

            Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata);

            Task<string> TakePhotoAsync(object context);

            Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename);
        }

###<a name="add-filehelper"></a>Přidání FileHelper třídy

1. Vytvoření nové třídy `FileHelper` v projektu hlavní přenosném knihovny. Přidejte následující pomocí příkazů:

        using System.IO;
        using PCLStorage;
        using System.Threading.Tasks;
        using Xamarin.Forms;

2. Přidání definici třídy:

        public class FileHelper
        {
            public static async Task<string> CopyTodoItemFileAsync(string itemId, string filePath)
            {
                IFolder localStorage = FileSystem.Current.LocalStorage;

                string fileName = Path.GetFileName(filePath);
                string targetPath = await GetLocalFilePathAsync(itemId, fileName);

                var sourceFile = await localStorage.GetFileAsync(filePath);
                var sourceStream = await sourceFile.OpenAsync(FileAccess.Read);

                var targetFile = await localStorage.CreateFileAsync(targetPath, CreationCollisionOption.ReplaceExisting);

                using (var targetStream = await targetFile.OpenAsync(FileAccess.ReadAndWrite)) {
                    await sourceStream.CopyToAsync(targetStream);
                }

                return targetPath;
            }

            public static async Task<string> GetLocalFilePathAsync(string itemId, string fileName)
            {
                IPlatform platform = DependencyService.Get<IPlatform>();

                string recordFilesPath = Path.Combine(await platform.GetTodoFilesPathAsync(), itemId);

                    var checkExists = await FileSystem.Current.LocalStorage.CheckExistsAsync(recordFilesPath);
                    if (checkExists == ExistenceCheckResult.NotFound) {
                        await FileSystem.Current.LocalStorage.CreateFolderAsync(recordFilesPath, CreationCollisionOption.ReplaceExisting);
                    }

                return Path.Combine(recordFilesPath, fileName);
            }

            public static async Task DeleteLocalFileAsync(Microsoft.WindowsAzure.MobileServices.Files.MobileServiceFile fileName)
            {
                string localPath = await GetLocalFilePathAsync(fileName.ParentId, fileName.Name);
                var checkExists = await FileSystem.Current.LocalStorage.CheckExistsAsync(localPath);

                if (checkExists == ExistenceCheckResult.FileExists) {
                    var file = await FileSystem.Current.LocalStorage.GetFileAsync(localPath);
                    await file.DeleteAsync();
                }
            }
        }

###<a name="file-sync-handler"></a>Přidání rutiny synchronizaci souborů

Vytvoření nové třídy `TodoItemFileSyncHandler` v projektu hlavní přenosném knihovny. Tato třída obsahuje zpětná z Azure SDK sdělit svůj kód při přidání nebo odebrání souboru.

Azure mobilního klienta SDK neukládá skutečně souborových dat: klient SDK vyvolá implementaci `IFileSyncHandler` která zároveň určuje, zda a jak souborů uložených na místním zařízením.

1. Přidejte následující pomocí příkazů:

        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Xamarin.Forms;

2. Nahraďte definici třídy takto: 

        public class TodoItemFileSyncHandler : IFileSyncHandler
        {
            private readonly TodoItemManager todoItemManager;

            public TodoItemFileSyncHandler(TodoItemManager itemManager)
            {
                this.todoItemManager = itemManager;
            }

            public Task<IMobileServiceFileDataSource> GetDataSource(MobileServiceFileMetadata metadata)
            {
                IPlatform platform = DependencyService.Get<IPlatform>();
                return platform.GetFileDataSource(metadata);
            }

            public async Task ProcessFileSynchronizationAction(MobileServiceFile file, FileSynchronizationAction action)
            {
                if (action == FileSynchronizationAction.Delete) {
                    await FileHelper.DeleteLocalFileAsync(file);
                }
                else { // Create or update. We're aggressively downloading all files.
                    await this.todoItemManager.DownloadFileAsync(file);
                }
            }
        }

###<a name="update-todoitemmanager"></a>Aktualizace TodoItemManager

1. V **TodoItemManager.cs**zrušte komentář řádku `#define OFFLINE_SYNC_ENABLED`.

2. V **TodoItemManager.cs**, přidejte následující text pomocí příkazů:

        using System.IO;
        using Xamarin.Forms;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Eventing;

3. V konstruktoru `TodoItemManager`, přidejte následující po volání `DefineTable()`:

        // Initialize file sync
        this.client.InitializeFileSyncContext(new TodoItemFileSyncHandler(this), store);

4. V konstruktoru, nahraďte volání `InitializeAsync` následujícím. Zajistíte, že jsou zpětná změně záznamů v místním úložišti. Funkce synchronizace soubor používá tyto zpětná aktivovat vaše obslužná rutina synchronizaci souborů.

        this.client.SyncContext.InitializeAsync(store, StoreTrackingOptions.NotifyLocalAndServerOperations);

5. V `SyncAsync()`, přidejte následující po volání `PushAsync()`:

        await this.todoTable.PushFileChangesAsync();

6. Přidání následujících metod k `TodoItemManager`:

        internal async Task DownloadFileAsync(MobileServiceFile file)
        {
            var todoItem = await todoTable.LookupAsync(file.ParentId);
            IPlatform platform = DependencyService.Get<IPlatform>();

            string filePath = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name); 
            await platform.DownloadFileAsync(this.todoTable, file, filePath);
        }

        internal async Task<MobileServiceFile> AddImage(TodoItem todoItem, string imagePath)
        {
            string targetPath = await FileHelper.CopyTodoItemFileAsync(todoItem.Id, imagePath);
            return await this.todoTable.AddFileAsync(todoItem, Path.GetFileName(targetPath));
        }

        internal async Task DeleteImage(TodoItem todoItem, MobileServiceFile file)
        {
            await this.todoTable.DeleteFileAsync(file);
        }

        internal async Task<IEnumerable<MobileServiceFile>> GetImageFilesAsync(TodoItem todoItem)
        {
            return await this.todoTable.GetFilesAsync(todoItem);
        }

###<a name="add-details-view"></a>Přidání zobrazení podrobností o

V této části budete přidávat nové zobrazení Podrobnosti pro položku úkol. Zobrazení se vytvoří, když uživatel vybere položky úkolu a umožňuje nové obrázky, které budou přidány do položky.

1. Přidání nové třídy **TodoItemImage** projektu portable knihovny prováděním následující:

        public class TodoItemImage : INotifyPropertyChanged
        {
            private string name;
            private string uri;

            public MobileServiceFile File { get; private set; }

            public string Name
            {
                get { return name; }
                set
                {
                    name = value;
                    OnPropertyChanged(nameof(Name));
                }
            }

            public string Uri
            {
                get { return uri; }      
                set
                {
                    uri = value;
                    OnPropertyChanged(nameof(Uri));
                }
            }

            public TodoItemImage(MobileServiceFile file, TodoItem todoItem)
            {
                Name = file.Name;
                File = file;

                FileHelper.GetLocalFilePathAsync(todoItem.Id, file.Name).ContinueWith(x => this.Uri = x.Result);
            }

            public event PropertyChangedEventHandler PropertyChanged;

            private void OnPropertyChanged(string propertyName)
            {
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
            }
        }

2. Úprava **App.cs**. Nahrazení inicializace `MainPage` následující:
    
        MainPage = new NavigationPage(new TodoList());

3. V **App.cs**přidejte následující vlastnosti:

        public static object UIContext { get; set; }

4. Projekt portable knihovny klikněte pravým tlačítkem myši a vyberte **Přidat** -> **Nová položka** -> **různé platformy** -> **Stránky Xaml formuláře**. Název zobrazení `TodoItemDetailsView`.

5. Otevřete **TodoItemDetailsView.xaml** a nahrazení textu ContentPage takto:

          <Grid>
            <Grid.RowDefinitions>
              <RowDefinition Height="Auto"/>
              <RowDefinition Height="Auto"/>
              <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            <Button Clicked="OnAdd" Text="Add image"></Button>
            <ListView x:Name="imagesList"
                      ItemsSource="{Binding Images}"
                      IsPullToRefreshEnabled="false"
                      Grid.Row="2">
              <ListView.ItemTemplate>
                <DataTemplate>
                  <ImageCell ImageSource="{Binding Uri}"
                             Text="{Binding Name}">
                  </ImageCell>
                </DataTemplate>
              </ListView.ItemTemplate>
            </ListView>
          </Grid>

6. Úprava **TodoItemDetailsView.xaml.cs** a přidejte následující pomocí příkazů:

        using System.Collections.ObjectModel;
        using Microsoft.WindowsAzure.MobileServices.Files;

7. Nahrazení provádění `TodoItemDetailsView` následující:

        public partial class TodoItemDetailsView : ContentPage
        {
            private TodoItemManager manager;

            public TodoItem TodoItem { get; set; }        
            public ObservableCollection<TodoItemImage> Images { get; set; }

            public TodoItemDetailsView(TodoItem todoItem, TodoItemManager manager)
            {
                InitializeComponent();
                this.Title = todoItem.Name;

                this.TodoItem = todoItem;
                this.manager = manager;

                this.Images = new ObservableCollection<TodoItemImage>();
                this.BindingContext = this;
            }

            public async Task LoadImagesAsync()
            {
                IEnumerable<MobileServiceFile> files = await this.manager.GetImageFilesAsync(TodoItem);
                this.Images.Clear();

                foreach (var f in files) {
                    var todoImage = new TodoItemImage(f, this.TodoItem);
                    this.Images.Add(todoImage);
                }
            }

            public async void OnAdd(object sender, EventArgs e)
            {
                IPlatform mediaProvider = DependencyService.Get<IPlatform>();
                string sourceImagePath = await mediaProvider.TakePhotoAsync(App.UIContext);

                if (sourceImagePath != null) {
                    MobileServiceFile file = await this.manager.AddImage(this.TodoItem, sourceImagePath);

                    var image = new TodoItemImage(file, this.TodoItem);
                    this.Images.Add(image);
                }
            }
        }

###<a name="update-main-view"></a>Aktualizace hlavního zobrazení 

Aktualizujte hlavní zobrazení otevřete zobrazení Podrobnosti, je-li vybrán položky úkolu.

V **TodoList.xaml.cs**nahraďte provádění `OnSelected` následující:

    public async void OnSelected(object sender, SelectedItemChangedEventArgs e)
    {
        var todo = e.SelectedItem as TodoItem;

        if (todo != null) {
            var detailsView = new TodoItemDetailsView(todo, manager);
            await detailsView.LoadImagesAsync();
            await Navigation.PushAsync(detailsView);
        }

        todoList.SelectedItem = null;
    }

###<a name="update-android"></a>Aktualizace Android projektu

Přidání kódu pro platformu Android projektu, včetně kódu pro stahování souboru a používání kamera k zaznamenání nového obrázku. 

Tento kód používá Xamarin.Forms [DependencyService](https://developer.xamarin.com/guides/xamarin-forms/dependency-service/) načíst třídě doprava specifické pro platformu za běhu.

1. Přidání komponentu **Xamarin.Mobile** Android projektu.

2. Přidání nového třídy `DroidPlatform` následující prováděním. Nahraďte "YourNamespace" oboru hlavního projektu.

        using System;
        using System.IO;
        using System.Threading.Tasks;
        using Android.Content;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Sync;
        using Xamarin.Media;

        [assembly: Xamarin.Forms.Dependency(typeof(YourNamespace.Droid.DroidPlatform))]
        namespace YourNamespace.Droid
        {
            public class DroidPlatform : IPlatform
            {
                public async Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename)
                {
                    var path = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name);
                    await table.DownloadFileAsync(file, path);
                }

                public async Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata)
                {
                    var filePath = await FileHelper.GetLocalFilePathAsync(metadata.ParentDataItemId, metadata.FileName);
                    return new PathMobileServiceFileDataSource(filePath);
                }

                public async Task<string> TakePhotoAsync(object context)
                {
                    try {
                        var uiContext = context as Context;
                        if (uiContext != null) {
                            var mediaPicker = new MediaPicker(uiContext);
                            var photo = await mediaPicker.TakePhotoAsync(new StoreCameraMediaOptions());

                            return photo.Path;
                        }
                    }
                    catch (TaskCanceledException) {
                    }

                    return null;
                }

                public Task<string> GetTodoFilesPathAsync()
                {
                    string appData = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);
                    string filesPath = Path.Combine(appData, "TodoItemFiles");

                    if (!Directory.Exists(filesPath)) {
                        Directory.CreateDirectory(filesPath);
                    }

                    return Task.FromResult(filesPath);
                }
            }
        }

3. Úprava **MainActivity.cs**. V `OnCreate`, přidejte následující před voláním `LoadApplication()`:

        App.UIContext = this;

###<a name="update-ios"></a>Aktualizovat projekt iOS

Přidání kódu pro platformu iOS projektu.

1. Přidání komponentu **Xamarin.Mobile** iOS projektu.

2. Přidání nového třídy `TouchPlatform` následující prováděním. Nahraďte "YourNamespace" oboru hlavního projektu.

        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Sync;
        using Xamarin.Media;

        [assembly: Xamarin.Forms.Dependency(typeof(YourNamespace.iOS.TouchPlatform))]
        namespace YourNamespace.iOS
        {
            class TouchPlatform : IPlatform
            {
                public async Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename)
                {
                    var path = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name);
                    await table.DownloadFileAsync(file, path);
                }

                public async Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata)
                {
                    var filePath = await FileHelper.GetLocalFilePathAsync(metadata.ParentDataItemId, metadata.FileName);
                    return new PathMobileServiceFileDataSource(filePath);
                }

                public async Task<string> TakePhotoAsync(object context)
                {
                    try {
                        var mediaPicker = new MediaPicker();
                        var mediaFile = await mediaPicker.PickPhotoAsync();
                        return mediaFile.Path;
                    }
                    catch (TaskCanceledException) {
                        return null;
                    }
                }

                public Task<string> GetTodoFilesPathAsync()
                {
                    string filesPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments), "TodoItemFiles");

                    if (!Directory.Exists(filesPath)) {
                        Directory.CreateDirectory(filesPath);
                    }

                    return Task.FromResult(filesPath);
                }
            }
        }

3. Úprava **AppDelegate.cs** a zrušte komentář volání `SQLitePCL.CurrentPlatform.Init()`.

###<a name="update-windows"></a>Aktualizace Windows projektu

1. Nainstalujte rozšíření Visual Studia [SQLite pro Windows 8.1](http://go.microsoft.com/fwlink/?LinkID=716919). Další informace najdete v tématu kurz [Povolení offline synchronizace pro aplikace systému Windows](app-service-mobile-windows-store-dotnet-get-started-offline-data.md). 

2. Upravte **Package.appxmanifest** a Kontrola možností **webové kamery** .

3. Přidání nového třídy `WindowsStorePlatform` následující prováděním. Nahraďte "YourNamespace" oboru hlavního projektu.

        using System;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Sync;
        using Windows.Foundation;
        using Windows.Media.Capture;
        using Windows.Storage;
        using YourNamespace;

        [assembly: Xamarin.Forms.Dependency(typeof(WinApp.WindowsStorePlatform))]
        namespace WinApp
        {
            public class WindowsStorePlatform : IPlatform
            {
                public async Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename)
                {
                    var path = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name);
                    await table.DownloadFileAsync(file, path);
                }

                public async Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata)
                {
                    var filePath = await FileHelper.GetLocalFilePathAsync(metadata.ParentDataItemId, metadata.FileName);
                    return new PathMobileServiceFileDataSource(filePath);
                }

                public async Task<string> GetTodoFilesPathAsync()
                {
                    var storageFolder = ApplicationData.Current.LocalFolder;
                    var filePath = "TodoItemFiles";

                    var result = await storageFolder.TryGetItemAsync(filePath);

                    if (result == null) {
                        result = await storageFolder.CreateFolderAsync(filePath);
                    }

                    return result.Name; // later operations will use relative paths
                }

                public async Task<string> TakePhotoAsync(object context)
                {
                    try {
                        CameraCaptureUI dialog = new CameraCaptureUI();
                        Size aspectRatio = new Size(16, 9);
                        dialog.PhotoSettings.CroppedAspectRatio = aspectRatio;

                        StorageFile file = await dialog.CaptureFileAsync(CameraCaptureUIMode.Photo);
                        return file.Path;
                    }
                    catch (TaskCanceledException) {
                        return null;
                    }
                }
            }
        }

##<a name="summary"></a>Souhrn

Tento článek popisující použití nové podpoře soubor v Azure mobilního klienta a serveru SDK pro práci s Azure úložiště. 

- Vytvoření účtu úložiště a přidejte připojovací řetězec do back-end vaše mobilní aplikace. Jen back-end má klíč k základnímu úložišti Azure: mobilní klient požaduje token přidružení zabezpečení (sdílené přístup podpis) pokaždé, když má přístup k úložišti Azure. Další informace o tokenů přidružení zabezpečení v úložišti Azure, najdete v článku [Principy sdílené přístup podpisy].

- Vytvoření řadiče této dílčí `StorageController` za účelem zpracování žádostí o tokenu přidružení zabezpečení a získat soubory, které jsou přidružené k záznamu. Ve výchozím nastavení jsou soubory přidružená k záznamu pomocí ID záznamu jako součást jméno container; chování můžete přizpůsobit tak, že zadá implementace `IContainerNameResolver`. Můžete přizpůsobit i zásady tokenu přidružení zabezpečení.

- Azure mobilního klienta SDK neukládá skutečně uložení dat soubor. Místo toho klienta SDK vyvolá vaše `IFileSyncHandler`, kterou rozhoduje o jak (a když) souborů uložených na místním zařízením. Obslužná rutina synchronizace je registrovaná následujícím způsobem:

        client.InitializeFileSync(new MyFileSyncHandler(), store);

      + `IFileSyncHandler.GetDataSource`se nazývá SDK Azure mobilních klientů potřebujete datový soubor (například jako součást procesu nahrát). To vám dá možnost spravovat jak (a když) soubory jsou uložené na místním zařízením a vraťte se tyto informace v případě potřeby.

      + `IFileSyncHandler.ProcessFileSynchronizationAction`vyvolání jako součást toku synchronizaci souborů. Odkaz na soubor a hodnotu FileSynchronizationAction výčtu jsou k dispozici, můžete se rozhodnout, jak aplikace zacházet dané události (například automatické stahování souboru se vytvořila nebo aktualizovala, odstranění souboru z místního zařízení, když tento soubor na serveru).

- A `MobileServiceFile` lze použít v online nebo offline režimu pomocí `IMobileServiceTable` nebo `IMobileServiceSyncTable`v. V offline scénář nahrávání dojde při volání aplikace `PushFileChangesAsync`. To způsobí, že offline operace fronty zpracovány; pro každou akci soubor Azure mobilních klientů SDK vyvolá `GetDataSource` metody `IFileSyncHandler` instance k načtení obsah souboru pro ukládání.

- Pokud chcete načíst soubory k položce, zavolejte "GetFilesAsync` method on the  `IMobileServiceTable<T> ` or IMobileServiceSyncTable<T>` instance. Tento způsob vrátí seznam souborů přidružené k položce data k dispozici. (Poznámka: Toto je operace *místního* a vrátí soubory, které při poslední synchronizace podle stavu objektu. Získat aktualizovaný seznam souborů ze serveru, mají aktivujete operace synchronizace nejdřív.)

        IEnumerable<MobileServiceFile> files = await myTable.GetFilesAsync(myItem);

- Funkce synchronizaci souborů načítá upozornění o změně záznamu v místním úložišti záznamy, které klienta získali v rámci operace nabízených nebo vložit. Dosahuje zapnete oznámení o místní a serveru pro používání místní synchronizace `StoreTrackingOptions` parametr. 

        this.client.SyncContext.InitializeAsync(store, StoreTrackingOptions.NotifyLocalAndServerOperations);

      + Další možnosti sledování úložiště jsou dostupné, například jenom místní nebo jen serveru oznámení. Můžete přidat nebo vlastníte pomocí vlastní zpětné `EventManager` vlastnosti `IMobileServiceClient`:

            jobService.MobileService.EventManager.Subscribe<StoreOperationCompletedEvent>(StoreOperationEventHandler);

- Je možné přidat nebo odebrat soubory ze záznamu typu změnou úložiště objektů blob přímo, protože přidružení je dosáhnout pojmenování. Však v tomto případě byste měli vždy **Aktualizovat časové razítko při změně přidružené objektů BLOB**. Azure mobilní klient SDK vždy aktualizuje záznam při přidání nebo odebrání souboru. 

    Důvody tohoto požadavku je, že někteří mobilní klienti už máte záznam v místním úložišti. Po těchto klientů provést přírůstková vyžádané, tento záznam nejsou k dispozici a klienta nebude dotazování na nové souvisejících souborů. Problému můžete předejít, doporučujeme při každé změně úložiště objektů blob, který nepoužívá Azure mobilních klientů SDK aktualizace časového razítka záznamu.

- Použije aplikace project klienta vzorek [Xamarin.Forms DependencyService] načíst třídy vpravo specifické pro platformu za běhu. V tomto příkladu jsme definovali rozhraní `IPlatform` s implementace ve všech projektů specifické pro platformu.

<!-- URLs. -->

[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
[Vytvoření aplikace Xamarin.Forms]: app-service-mobile-xamarin-forms-get-started.md
[Xamarin.Forms DependencyService]: https://developer.xamarin.com/guides/xamarin-forms/dependency-service/
[Microsoft.Azure.Mobile.Client.Files]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.Files/
[Microsoft.Azure.Mobile.Client.SQLiteStore]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/
[Microsoft.Azure.Mobile.Server.Files]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Files/
[Principy sdílené přístup podpisy]: ../storage/storage-dotnet-shared-access-signature-part-1.md
[Vytvořte účet Azure úložiště]:  ../storage/storage-create-storage-account.md#create-a-storage-account
