<properties
    pageTitle="Povolení offline synchronizace Azure mobilní aplikace (Android)"
    description="Naučte se používat aplikaci služby mobilní aplikace do mezipaměti a synchronizace dat offline v aplikaci Android"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="enable-offline-sync-for-your-android-mobile-app"></a>Povolení offline synchronizace Android mobilní aplikaci

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Základní informace

Tento kurz zahrnuje funkce offline synchronizace Azure mobilní aplikace pro Android. Offline synchronizace koncoví uživatelé chcete provést interakci s mobilní aplikaci&mdash;zobrazení, přidání nebo úprava dat&mdash;i v případě žádné síťové připojení. Změny se ukládají do místní databázi. Po zase online zařízení se tyto změny se synchronizují s vzdálené back-end.

Pokud je to vaše první zkušenosti s aplikací Mobile Azure, byste měli dokončit kurz [Vytvoření aplikace pro Android]. Pokud nepoužíváte project serveru stažený úvodní, je nutné přidat balíčků rozšíření dat aplikace access do projektu. Další informace o serveru rozšíření balíčků najdete v tématu [práce s back-end serveru .NET SDK Azure mobilních aplikací](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Další informace o funkci offline synchronizace najdete v tématu [Offline synchronizace dat v mobilních aplikacích Azure].

## <a name="update-the-app-to-support-offline-sync"></a>Aktualizace aplikace na podpory offline synchronizace

S offline synchronizace přečtěte si a zápisu dat ze *synchronizace tabulky* (pomocí rozhraní *IMobileServiceSyncTable* ), která je součástí **SQLite** databáze na vašem zařízení.

Nabízená a vyžádat změny mezi zařízení a Mobile služby Azure, použijete *místní synchronizace* (*MobileServiceClient.SyncContext*), které inicializace s místní databázi k ukládání dat místně.

1. V `TodoActivity.java`, komentáře definici existující `mToDoTable` a zrušte komentář verze synchronizace tabulky:

        private MobileServiceSyncTable<ToDoItem> mToDoTable;

2. V `onCreate` metody, komentáře existující inicializace `mToDoTable` a zrušte komentář tato definice:

        mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);

3. V `refreshItemsFromTable` komentáře definici `results` a zrušte komentář tato definice:

        // Offline Sync
        final List<ToDoItem> results = refreshItemsFromMobileServiceTableSyncTable();

4. Komentáře se definici `refreshItemsFromMobileServiceTable`.

5. Zrušte komentář definici `refreshItemsFromMobileServiceTableSyncTable`:

        private List<ToDoItem> refreshItemsFromMobileServiceTableSyncTable() throws ExecutionException, InterruptedException {
            //sync the data
            sync().get();
            Query query = QueryOperations.field("complete").
                    eq(val(false));
            return mToDoTable.read(query).get();
        }

6. Zrušte komentář definici `sync`:

        private AsyncTask<Void, Void, Void> sync() {
            AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
                @Override
                protected Void doInBackground(Void... params) {
                    try {
                        MobileServiceSyncContext syncContext = mClient.getSyncContext();
                        syncContext.push().get();
                        mToDoTable.pull(null).get();
                    } catch (final Exception e) {
                        createAndShowDialogFromTask(e, "Error");
                    }
                    return null;
                }
            };
            return runAsyncTask(task);
        }

## <a name="test-the-app"></a>Testování aplikace

V této části testování chování s Wi-Fi na a potom vypněte Wi-Fi na vytvořit offline scénář.

Když přidáte položky dat, jsou uskutečňuje v místním úložišti SQLite, ale není synchronizované s mobilních služeb, dokud stiskněte tlačítko **Aktualizovat** . Jiných aplikací může být různé požadavky při potřeba synchronizovat data, ale pro účely ukázky tento kurz má uživatel explicitně něj požádat.

Když stisknete tlačítko, spustí se nový úkol pozadí. Posune nejdřív všechny změny provedené v místním úložišti pomocí místní synchronizace a pak si změnili dat z Azure místní tabulku.

### <a name="offline-testing"></a>Offline testování

1. Umístěte v *Režimu letadla ve*zařízení nebo simulator. Tím vytvoříte scénáři offline.

2. Přidání některé položky *úkol* nebo označit některé položky jako dokončený. Ukončete zařízení nebo simulator (nebo vynutit zavření aplikace) a restartujte. Ověřte, že změny byly přetrvává na zařízení vzhledem k tomu, že se uskutečňuje v místním úložišti SQLite.

3. Zobrazení obsahu Azure *TodoItem* tabulky, ať už pomocí SQL nástroje třeba *SQL Server Management Studio*nebo ZBÝVAJÍCÍ klientů, jako je *Fiddler* nebo *pošťák*. Ověřte, že nové položky _mají neproběhla na server_

    + Pro Node.js back-end, přejděte na [portál Azure](https://portal.azure.com/)a v mobilní aplikaci back-end klikněte na **Snadno tabulky** > **TodoItem** zobrazíte obsah `TodoItem` tabulky.
    + .NET back-end zobrazení obsahu pomocí nástroje SQL třeba *SQL Server Management Studio*nebo ZBÝVAJÍCÍ klientů, jako je *Fiddler* nebo *pošťák*.

4. V zařízení nebo simulator zapněte Wi-Fi. Potom na tlačítko **Aktualizovat** .

5. Zobrazení dat TodoItem znovu Azure portálu. Nové a změněné TodoItems by teď měla zobrazovat.

## <a name="additional-resources"></a>Další zdroje informací

* [Synchronizace dat v režimu offline v Azure mobilní aplikace]

* [Cloudu titulních: Offline synchronizace služby Azure mobilní] \(Poznámka: video o službách Mobile, ale offline synchronizace funguje podobně v mobilních aplikacích Azure\)


<!-- URLs. -->

[Synchronizace dat v režimu offline v Azure mobilní aplikace]: app-service-mobile-offline-data-sync.md

[Vytvoření aplikace pro Android]: app-service-mobile-android-get-started.md

[Obálka cloudu: Offline synchronizace v Azure mobilní služby]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/

