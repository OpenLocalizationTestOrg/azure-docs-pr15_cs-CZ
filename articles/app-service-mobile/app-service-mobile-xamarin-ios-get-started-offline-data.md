<properties
    pageTitle="Povolení offline synchronizace Azure mobilní aplikace (Xamarin iOS)"
    description="Naučte se používat aplikaci služby mobilní aplikace do mezipaměti a synchronizace dat offline v aplikaci Xamarin iOS"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
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

# <a name="enable-offline-sync-for-your-xamarinios-mobile-app"></a>Povolení offline synchronizace Xamarin.iOS mobilní aplikaci

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Základní informace

Tento kurz přináší funkce offline synchronizace Azure mobilních aplikací pro Xamarin.iOS. Offline synchronizace koncoví uživatelé chcete provést interakci s v mobilní aplikaci – zobrazení, přidání nebo úprava dat – i v případě žádné síťové připojení. Změny se ukládají do místní databázi. Po návrat do online režimu zařízení se tyto změny se synchronizují s vzdálené služby.

V tomto kurzu aktualizujte projekt aplikace Xamarin.iOS [vytvořit aplikace iOS Xamarin] podpory offline funkcí Azure mobilní aplikace. Pokud nepoužíváte project serveru stažený úvodní, je nutné přidat balíčků rozšíření dat aplikace access do projektu. Další informace o serveru rozšíření balíčků najdete v tématu [práce s back-end serveru .NET SDK Azure mobilních aplikací](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Další informace o funkci offline synchronizace najdete v tématu [Offline synchronizace dat v mobilních aplikacích Azure].

## <a name="update-the-client-app-to-support-offline-features"></a>Aktualizace aplikace na klienta pro podporu offline funkcí

Azure mobilní aplikaci offline funkce umožňují chcete provést interakci s místní databázi při práci v offline scénář. Použití těchto funkcí v aplikaci, inicializace [SyncContext] k místním úložišti. Odkaz na tabulky pomocí rozhraní [IMobileServiceSyncTable]. SQLite slouží jako místní úložiště v zařízení.

1. Otevřete Správce balíčků NuGet v projektu, který jste dokončili v kurzu [Vytvoření aplikace iOS Xamarin] vyhledejte a nainstalovat balíček **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet.

2. Otevřete soubor QSTodoService.cs a zrušte komentář `#define OFFLINE_SYNC_ENABLED` definice.

3. Znovu vytvořit a spustit aplikaci klienta. Aplikace funguje stejně, stejně jako před povolením offline synchronizace. Však místní databázi je teď vyplněné dat, které můžete použít ve scénáři offline.

## <a name="update-sync"></a>Aktualizace aplikace odpojit od back-end

V této části přerušit back-end vaše mobilní aplikaci, aby napodobily offline stavu připojení. Když přidáte položky dat, rutinu výjimky zjistíte, že aplikace v offline režimu. V této fázi nové položky přidané v místním úložišti a budou pak spustíte nabízená v připojeném stavu synchronizovat do back-end mobilní aplikace.

1. Upravte QSToDoService.cs ve sdíleném projektu. Změna **applicationURL** tak, aby ukazovaly na neplatnou adresu URL:

         const string applicationURL = @"https://your-service.azurewebsites.fail";

    Můžete taky ukazují offline chování zakázáním Wi-Fi a mobilní sítě na zařízení nebo použití režimu letadla ve.

2. Vytvoření a spuštění aplikace. Všimněte si synchronizace při došlo k chybě při obnovení aplikace.

3. Zadejte nové položky a Všimněte si, že nabízených se nezdaří se stavem [CancelledByNetworkError] pokaždé, když kliknete na **Uložit**. Nové položky úkol však existovat v místním úložišti dokud můžete posune back-end mobilní aplikace.  V aplikaci výrobní Pokud potlačit tyto výjimky aplikaci klienta chová se jako pořád připojení k back-end mobilní aplikace.

4. Ukončete aplikaci a restartujte ji můžete ověřit, že jsou k místním úložišti zachován nové položky, kterou jste vytvořili.

5. (Volitelné) Pokud máte na počítači nainstalovali Visual Studio, spusťte **Průzkumníka serveru**. Přejděte do databáze v **Azure**-> **SQL databáze**. Klikněte pravým tlačítkem myši databáze a vyberte **Otevřít v Průzkumníkovi objektu SQL serveru**. Teď můžete přecházet na tabulky databáze SQL a její obsah. Ověřte, že se data z databáze back-end nezměnil.

6. (Volitelné) Použijte nástroj ZBÝVAJÍCÍ Fiddler ATP pošťák k vytvoření dotazu vaše mobilní back-end, použití GET dotazu ve formě `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Aktualizace aplikace na znovu připojit backendovou mobilní aplikaci

V této části znovu aplikace na back-end mobilní aplikace. To napodobuje aplikaci Přesunované z offline režimu pomocí mobilní aplikace back-end online stavu.   Pokud simulovaného poškození sítě – vypněte připojení k síti, je třeba žádné změny kód.
Síť zase zapněte.  Při prvním spuštění aplikace `RefreshDataAsync` s názvem metody. To volá `SyncAsync` synchronizovat místního úložiště back-end databázi.

1. Otevřete QSToDoService.cs ve sdíleném projektu a vrátit změny vlastnost **applicationURL** .

2. Opětovné vytvoření a spuštění aplikace. Aplikace synchronizuje místní změny s použitím a nabízené replikace operací back-end Azure mobilní aplikaci při `OnRefreshItemsSelected` metody.

3. (Volitelné) Zobrazení aktualizovaná data pomocí Průzkumníka objektu SQL serveru nebo nástroji ZBÝVAJÍCÍ jako Fiddler. Oznámení data synchronizování mezi Azure mobilní aplikaci back-end databáze a v místním úložišti.

4. V seznamu aplikací klepněte na zaškrtávací políčko vedle pár věcí, na dokončení v místním úložišti.

  `CompleteItemAsync`volání `SyncAsync` synchronizace každé dokončení položku s back-end mobilní aplikaci. `SyncAsync`zavolá nabízených a vložit.
  **Při každém spuštění vyžádané před tabulku, která klienta připíše změny, nabízená v kontextu synchronizace klienta je vždycky nejdřív automatické spuštění**. Implicitní nabízených zaručuje, že všechny tabulky v místním úložišti spolu s relace zůstávají stejné. Další informace o toto chování najdete v článku [Offline synchronizace dat v mobilních aplikacích Azure].

## <a name="review-the-client-sync-code"></a>Revize kód klienta synchronizace

Projekt klienta Xamarin, který jste stáhli po dokončení kurz [Vytvoření aplikace iOS Xamarin] již obsahuje kód podpora offline synchronizace pomocí místní databázi SQLite. Tady je stručný přehled co je už součástí výuková kód. Přehled funkci najdete v článku [Offline synchronizace dat v mobilních aplikacích Azure].

* Předtím, než lze provést všechny operace tabulky, musí být spuštěn v místním úložišti. Databáze místním úložišti inicializována při `QSTodoListViewController.ViewDidLoad()` provede `QSTodoService.InitializeStoreAsync()`. Tento způsob vytvoří novou místní SQLite databáze pomocí `MobileServiceSQLiteStore` třídy poskytovanou klientovi Azure mobilní aplikaci SDK.

    `DefineTable` Metoda vytvoří tabulku v místním úložišti odpovídající polím v poskytnutého typu `ToDoItem` v tomto případě. Typ nemá zahrnout všechny sloupce, které jsou obsaženy v vzdálené databázi. Je možné uložit jen podmnožinu sloupců.

        // QSTodoService.cs

        public async Task InitializeStoreAsync()
        {
            var store = new MobileServiceSQLiteStore(localDbPath);
            store.DefineTable<ToDoItem>();

            // Uses the default conflict handler, which fails on conflict
            await client.SyncContext.InitializeAsync(store);
        }


* `todoTable` Členem `QSTodoService` je `IMobileServiceSyncTable` zadejte místo `IMobileServiceTable`. IMobileServiceSyncTable směrovat tak, aby všechny vytvořit, číst, aktualizovat a odstraňovat operace s tabulkou (CRUD) k databázi místním úložišti.

    Je potřeba rozhodnout, kdy tyto změny se posunou back-end Azure mobilní aplikaci tak, že zavoláte `IMobileServiceSyncContext.PushAsync()`. Místní synchronizace umožňuje zachovat relací mezi tabulkami sledování a předání změny ve všech tabulkách v klientské aplikaci změnil špatně `PushAsync` se nazývá.

    Zadaný kód volá `QSTodoService.SyncAsync()` synchronizovat kdykoli aktualizovat seznam todoitem nebo todoitem vkládání nebo dokončení. Aplikace synchronizuje po každé změně místní. Pokud vložit je spouštět oproti tabulku, která obsahuje čekající místní aktualizace sledovány kontextem, operaci vyžádané automaticky spustí kontextu nabízených nejdřív.

    Zadaný kód všech záznamů v vzdáleného `TodoItem` dotazovaném tabulku, ale je také možné filtrovat záznamy předáním id dotazu a dotazů týkajících se `PushAsync`. Další informace naleznete v části *Přírůstková synchronizace* v [Offline synchronizace dat v mobilních aplikacích Azure].

        // QSTodoService.cs
        public async Task SyncAsync()
        {
            try
            {
                await client.SyncContext.PushAsync();
                await todoTable.PullAsync("allTodoItems", todoTable.CreateQuery()); // query ID is used for incremental sync
            }

            catch (MobileServiceInvalidOperationException e)
            {
                Console.Error.WriteLine(@"Sync Failed: {0}", e.Message);
            }
        }


## <a name="additional-resources"></a>Další zdroje informací

* [Synchronizace dat v režimu offline v Azure mobilní aplikace]
* [POSTUPY SDK .NET Azure mobilní aplikace][8]

<!-- Images -->

<!-- URLs. -->
[Vytvoření aplikace Xamarin iOS]: app-service-mobile-xamarin-ios-get-started.md
[Synchronizace dat v režimu offline v Azure mobilní aplikace]: app-service-mobile-offline-data-sync.md
[SyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md