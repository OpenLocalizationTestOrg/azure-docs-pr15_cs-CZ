<properties
    pageTitle="Povolení offline synchronizace Azure mobilní aplikace (Xamarin Android)"
    description="Naučte se používat aplikaci služby mobilní aplikace do mezipaměti a synchronizace dat offline v aplikaci Xamarin Android"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-xamarinandroid-mobile-app"></a>Povolení offline synchronizace Xamarin.Android mobilní aplikaci

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Základní informace

Tento kurz uvádí funkce offline synchronizace Azure mobilních aplikací pro Xamarin.Android. Offline synchronizace koncoví uživatelé chcete provést interakci s v mobilní aplikaci – zobrazení, přidání nebo úprava dat – i v případě žádné síťové připojení. Změny se ukládají do místní databázi.
Po návrat do online režimu zařízení se tyto změny se synchronizují s vzdálené služby.

V tomto kurzu aktualizovat projekt klienta výuková [vytvořit z aplikace Xamarin Android] podpory offline funkcí Azure mobilní aplikace. Pokud nepoužíváte project serveru stažený úvodní, je nutné přidat balíčků rozšíření dat aplikace access do projektu. Další informace o serveru rozšíření balíčků najdete v tématu [práce s back-end serveru .NET SDK Azure mobilních aplikací](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Další informace o funkci offline synchronizace najdete v tématu [Offline synchronizace dat v mobilních aplikacích Azure].

## <a name="update-the-client-app-to-support-offline-features"></a>Aktualizace aplikace na klienta pro podporu offline funkcí

Azure mobilní aplikaci offline funkce umožňují chcete provést interakci s místní databázi při práci v offline scénář. Použití těchto funkcí v aplikaci, inicializace [SyncContext] k místním úložišti. Potom odkazovat tabulky pomocí rozhraní [IMobileServiceSyncTable] [IMobileServiceSyncTable]. SQLite slouží jako místní úložiště v zařízení.

1. Ve Visual Studiu spusťte Správce balíčku NuGet v projektu, který jste dokončili v kurzu [Vytvoření Xamarin Android aplikace] .  Vyhledat a nainstalovat balíček NuGet **Microsoft.Azure.Mobile.Client.SQLiteStore** .

2. Otevřete soubor ToDoActivity.cs a zrušte komentář `#define OFFLINE_SYNC_ENABLED` definice.

3. Ve Visual Studiu stiskněte klávesu **F5** znovu vytvořit a spustit aplikaci klienta. Aplikace funguje stejně, stejně jako před povolením offline synchronizace. Však místní databázi je teď vyplněné dat, které můžete použít ve scénáři offline.

## <a name="update-sync"></a>Aktualizace aplikace odpojit od back-end

V této části přerušit back-end vaše mobilní aplikaci, aby napodobily offline stavu připojení. Když přidáte položky dat, rutinu výjimky zjistíte, že aplikace v offline režimu. V tomto stavu nové položky přidané v místním úložišti a jsou synchronizovány back-end mobilní aplikace při spuštění push v připojeném stavu.

1. Úprava ToDoActivity.cs ve sdíleném projektu. Změna **applicationURL** tak, aby ukazovaly na neplatnou adresu URL:

         const string applicationURL = @"https://your-service.azurewebsites.fail";

    Můžete taky demonstrují offline chování zakázáním Wi-Fi a mobilní sítě na zařízení nebo použití režimu letadla ve.

2. Stisknutím klávesy **F5** k vytvoření a spuštění aplikace. Všimněte si synchronizace při došlo k chybě při obnovení aplikace.

3. Zadejte nové položky a Všimněte si, že nabízených se nezdaří se stavem [CancelledByNetworkError] pokaždé, když kliknete na **Uložit**. Nové položky úkol však existovat v místním úložišti dokud můžete posune back-end mobilní aplikace.  V aplikaci výrobní Pokud potlačit následujícími výjimkami aplikaci klienta chová se jako pořád připojení k back-end mobilní aplikace.

4. Ukončete aplikaci a restartujte ji můžete ověřit, že jsou k místním úložišti zachován nové položky, kterou jste vytvořili.

5. (Volitelné) Ve Visual Studiu spusťte **Průzkumníka serveru**. Přejděte do databáze v **Azure**->**SQL databáze**. Klikněte pravým tlačítkem myši databáze a vyberte **Otevřít v Průzkumníkovi objektu SQL serveru**. Teď můžete přecházet na tabulky databáze SQL a její obsah. Ověřte, že data z databáze back-end nezměnil.

6. (Volitelné) Použijte nástroj ZBÝVAJÍCÍ Fiddler ATP pošťák k vytvoření dotazu vaše mobilní back-end, použití GET dotazu ve formě `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Aktualizace aplikace na znovu připojit backendovou mobilní aplikaci

V této části znovu aplikace na back-end mobilní aplikace. Při prvním spuštění aplikace `OnCreate` volání obslužné rutiny události `OnRefreshItemsSelected`. Tato metoda volá `SyncAsync` synchronizovat místního úložiště back-end databázi.

1. Otevřete ToDoActivity.cs ve sdíleném projektu a vrátit změny vlastnost **applicationURL** .

2. Stisknutím klávesy **F5** opětovné vytvoření a spuštění aplikace. Aplikace synchronizuje místní změny s použitím a nabízené replikace operací back-end Azure mobilní aplikaci při `OnRefreshItemsSelected` metody.

3. (Volitelné) Zobrazení aktualizovaná data pomocí Průzkumníka objektu SQL serveru nebo nástroji ZBÝVAJÍCÍ jako Fiddler. Oznámení data synchronizování mezi Azure mobilní aplikaci back-end databáze a v místním úložišti.

4. V seznamu aplikací klepněte na zaškrtávací políčko vedle pár věcí, na dokončení v místním úložišti.

  `CheckItem`volání `SyncAsync` synchronizace každé dokončení položku s back-end mobilní aplikaci. `SyncAsync`zavolá nabízených a vložit. **Při každém spuštění vyžádané před tabulku, která klienta připíše změny, push vždy spuštěn automaticky**. Zajistíte, že všechny tabulky v místním úložišti spolu s relace zůstávají stejné. Toto chování může mít neočekávané nabízených. Další informace o toto chování najdete v článku [Offline synchronizace dat v mobilních aplikacích Azure].

## <a name="review-the-client-sync-code"></a>Revize kód klienta synchronizace

Projekt klienta Xamarin, který jste stáhli po dokončení kurz [Vytvoření aplikace Xamarin Android] již obsahuje kód podpora offline synchronizace pomocí místní databázi SQLite. Tady je stručný přehled toho, co je už součástí výuková c kód. Přehled funkci najdete v článku [Offline synchronizace dat v mobilních aplikacích Azure].

* Předtím, než lze provést všechny operace tabulky, musí být spuštěn v místním úložišti. Databáze místním úložišti inicializována při `ToDoActivity.OnCreate()` provede `ToDoActivity.InitLocalStoreAsync()`. Tento způsob vytvoří místní databázi SQLite pomocí `MobileServiceSQLiteStore` třídy určené klientem Azure mobilní aplikace SDK.

    `DefineTable` Metoda vytvoří tabulku v místním úložišti odpovídající polím v poskytnutého typu `ToDoItem` v tomto případě. Typ nemá zahrnout všechny sloupce, které jsou obsaženy v vzdálené databázi. Je možné uložit jen podmnožinu sloupců.

        // ToDoActivity.cs
        private async Task InitLocalStoreAsync()
        {
            // new code to initialize the SQLite store
            string path = Path.Combine(System.Environment
                .GetFolderPath(System.Environment.SpecialFolder.Personal), localDbFilename);

            if (!File.Exists(path))
            {
                File.Create(path).Dispose();
            }

            var store = new MobileServiceSQLiteStore(path);
            store.DefineTable<ToDoItem>();

            // Uses the default conflict handler, which fails on conflict
            // To use a different conflict handler, pass a parameter to InitializeAsync.
            // For more details, see http://go.microsoft.com/fwlink/?LinkId=521416.
            await client.SyncContext.InitializeAsync(store);
        }


* `toDoTable` Členem `ToDoActivity` je `IMobileServiceSyncTable` zadejte místo `IMobileServiceTable`. IMobileServiceSyncTable směrovat tak, aby všechny vytvořit, číst, aktualizovat a odstraňovat operace s tabulkou (CRUD) k databázi místním úložišti.

    Je potřeba rozhodnout, kdy změny se posunou back-end Azure mobilní aplikaci tak, že zavoláte `IMobileServiceSyncContext.PushAsync()`. Místní synchronizace umožňuje zachovat relací mezi tabulkami sledování a předání změny ve všech tabulkách v klientské aplikaci změnil špatně `PushAsync` se nazývá.

    Zadaný kód volá `ToDoActivity.SyncAsync()` synchronizovat kdykoli aktualizovat seznam todoitem nebo todoitem vkládání nebo dokončení. Kód synchronizuje po každé změně místní.

    Zadaný kód všech záznamů v vzdáleného `TodoItem` dotazovaném tabulku, ale je také možné filtrovat záznamy předáním id dotazu a dotazů týkajících se `PushAsync`. Další informace naleznete v části *Přírůstková synchronizace* v [Offline synchronizace dat v mobilních aplikacích Azure].

        // ToDoActivity.cs
        private async Task SyncAsync()
        {
            try {
                await client.SyncContext.PushAsync();
                await toDoTable.PullAsync("allTodoItems", toDoTable.CreateQuery()); // query ID is used for incremental sync
            } catch (Java.Net.MalformedURLException) {
                CreateAndShowDialog (new Exception ("There was an error creating the Mobile Service. Verify the URL"), "Error");
            } catch (Exception e) {
                CreateAndShowDialog (e, "Error");
            }
        }

## <a name="additional-resources"></a>Další zdroje informací

* [Synchronizace dat v režimu offline v Azure mobilní aplikace]
* [POSTUPY SDK .NET Azure mobilní aplikace][8]

<!-- URLs. -->
[Vytvoření aplikace Xamarin Android]: ../app-service-mobile-xamarin-android-get-started.md
[Synchronizace dat v režimu offline v Azure mobilní aplikace]: ../app-service-mobile-offline-data-sync.md

<!-- Images -->

<!-- URLs. -->
[Vytvoření aplikace Xamarin Android]: app-service-mobile-xamarin-android-get-started.md
[Synchronizace dat v režimu offline v Azure mobilní aplikace]: app-service-mobile-offline-data-sync.md
[Xamarin Studio]: http://xamarin.com/download
[Xamarin extension]: http://xamarin.com/visual-studio
[SyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
