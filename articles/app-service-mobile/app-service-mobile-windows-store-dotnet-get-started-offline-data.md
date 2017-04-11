<properties
    pageTitle="Povolení offline synchronizace aplikace univerzální platformu systému Windows (UWP) s aplikací Mobile | Azure aplikace služby"
    description="Informace o používání mobilní aplikace Azure mezipaměti a synchronizace dat offline v aplikaci univerzální platformu systému Windows (UWP)."
    documentationCenter="windows"
    authors="adrianhall"
    manager="erikre"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-windows-app"></a>Povolení offline synchronizace pro aplikace pro Windows

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Základní informace

Tento kurz se dozvíte, jak přidat podpora offline režimu do aplikace univerzální platformu systému Windows (UWP) pomocí back-end Azure mobilní aplikaci. Offline synchronizace koncoví uživatelé chcete provést interakci s v mobilní aplikaci – zobrazení, přidání nebo úprava dat – i v případě žádné síťové připojení. Změny se ukládají do místní databázi. Po návrat do online režimu zařízení se tyto změny se synchronizují s vzdálené back-end.

V tomto kurzu aktualizovat projekt aplikace UWP z kurzu [Vytvoření aplikace pro Windows] se dál nebude podporovat funkce offline Azure mobilních aplikací. Pokud nepoužíváte project serveru stažený úvodní, je nutné přidat balíčků rozšíření dat aplikace access do projektu. Další informace o serveru rozšíření balíčků najdete v tématu [práce s back-end serveru .NET SDK Azure mobilních aplikací](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Další informace o funkci offline synchronizace najdete v tématu [Offline synchronizace dat v mobilních aplikacích Azure].

## <a name="requirements"></a>Požadavky

Tento kurz vyžaduje následující předpoklady:

* Visual Studio 2013 v systému Windows 8.1 nebo novější.
* Dokončení [Vytvoření aplikace pro Windows],[Vytvoření aplikace pro windows].
* [Úložiště SQLite mobilní služby Azure][sqlite store nuget]
* [SQLite rozvoje univerzální platformu Windows už](http://www.sqlite.org/downloads)

## <a name="update-the-client-app-to-support-offline-features"></a>Aktualizace aplikace na klienta pro podporu offline funkcí

Azure funkce offline mobilní aplikaci umožňují chcete provést interakci s místní databázi při práci v offline scénář. Použití těchto funkcí v aplikaci, inicializace [SyncContext] [ synccontext] k místním úložišti. Potom odkazovat tabulky pomocí rozhraní[IMobileServiceSyncTable] [IMobileServiceSyncTable]. SQLite slouží jako místní úložiště v zařízení.

1. Nainstalujte modul [runtime SQLite pro univerzální platformu Windows už](http://sqlite.org/2016/sqlite-uwp-3120200.vsix).

2. Ve Visual Studiu spusťte Správce NuGet balíček pro projekt aplikace UWP, který jste dokončili v kurzu [Vytvoření aplikace pro Windows] .
    Vyhledat a nainstalovat balíček NuGet **Microsoft.Azure.Mobile.Client.SQLiteStore** .

4. V Průzkumníku klikněte pravým tlačítkem myši **odkazy** > **Přidat odkaz**  >  **Univerzální Windows** > **rozšíření**, pak ji povolit **SQLite univerzální platformu Windows už** a **Visual C++ 2015 Runtime aplikací univerzální platformu Windows už**.

    ![Přidání odkazu na SQLite UWP][1]

5. Otevřete soubor MainPage.xaml.cs a zrušte komentář `#define OFFLINE_SYNC_ENABLED` definice.

6. Ve Visual Studiu stiskněte klávesu **F5** znovu vytvořit a spustit aplikaci klienta. Aplikace funguje stejně, stejně jako před povolením offline synchronizace. Však místní databázi je teď vyplněné dat, které můžete použít ve scénáři offline.

## <a name="update-sync"></a>Aktualizace aplikace odpojit od back-end

V této části přerušit back-end vaše mobilní aplikaci, aby napodobily offline stavu připojení. Když přidáte položky dat, rutinu výjimky zjistíte, že aplikace v offline režimu. V této fázi nové položky přidané v místním úložišti a budou pak spustíte nabízená v připojeném stavu synchronizovat do back-end mobilní aplikace.

1. Úprava App.xaml.cs ve sdíleném projektu. Poznámky inicializace **MobileServiceClient** a přidejte následující řádek, který využívá adresy URL neplatné mobilní aplikace:

         public static MobileServiceClient MobileService = new MobileServiceClient("https://your-service.azurewebsites.fail");

    Můžete taky ukazují offline chování zakázáním Wi-Fi a mobilní sítě na zařízení nebo v režimu letadla ve.

2. Stisknutím klávesy **F5** k vytvoření a spuštění aplikace. Všimněte si synchronizace při došlo k chybě při obnovení aplikace.

3. Zadejte nové položky a Všimněte si, že nabízených se nezdaří se stavem [CancelledByNetworkError] pokaždé, když kliknete na **Uložit**. Nové položky úkol však existovat v místním úložišti dokud můžete posune back-end mobilní aplikace.  V aplikaci výrobní Pokud potlačit následujícími výjimkami aplikaci klienta chová se jako pořád připojení k back-end mobilní aplikace.

4. Ukončete aplikaci a restartujte ji můžete ověřit, že jsou k místním úložišti zachován nové položky, kterou jste vytvořili.

5. (Volitelné) Ve Visual Studiu spusťte **Průzkumníka serveru**. Přejděte do databáze v **Azure**->**SQL databáze**. Klikněte pravým tlačítkem myši databáze a vyberte **Otevřít v Průzkumníkovi objektu SQL serveru**. Teď můžete přecházet na tabulky databáze SQL a její obsah. Ověřte, že se data z databáze back-end nezměnil.

6. (Volitelné) Použijte nástroj ZBÝVAJÍCÍ Fiddler ATP pošťák k vytvoření dotazu vaše mobilní back-end, použití GET dotazu ve formě `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Aktualizace aplikace na znovu připojit backendovou mobilní aplikaci

V této části opětovném aplikace na back-end mobilní aplikace. Tyto změny simulovat opětovné připojení sítě v aplikaci.

Při prvním spuštění aplikace `OnNavigatedTo` volání obslužné rutiny události `InitLocalStoreAsync`. Tento způsob volá `SyncAsync` synchronizovat místního úložiště back-end databázi. Aplikace se pokusí synchronizovat při spuštění.

1. Otevřete App.xaml.cs ve sdíleném projektu a zrušte komentář vaše předchozí inicializace `MobileServiceClient` můžete správnou adresu URL v mobilní aplikaci.

2. Stisknutím klávesy **F5** opětovné vytvoření a spuštění aplikace. Aplikace synchronizuje místní změny s použitím a nabízené replikace operací back-end Azure mobilní aplikaci při `OnNavigatedTo` obslužná rutina události spustí.

3. (Volitelné) Zobrazení aktualizovaná data pomocí Průzkumníka objektu SQL serveru nebo nástroji ZBÝVAJÍCÍ jako Fiddler. Oznámení data synchronizování mezi Azure mobilní aplikaci back-end databáze a v místním úložišti.

4. V seznamu aplikací klepněte na zaškrtávací políčko vedle pár věcí, na dokončení v místním úložišti.

  `UpdateCheckedTodoItem`volání `SyncAsync` synchronizace každé dokončení položku s back-end mobilní aplikaci. `SyncAsync`zavolá nabízených a vložit. Však **při každém spuštění vyžádané před tabulku, která klienta připíše změny, push vždy spuštěn automaticky**. Toto chování zajišťuje, že všechny tabulky v místním úložišti spolu s relace zůstávají stejné. Toto chování může mít neočekávané nabízených.  Další informace o toto chování najdete v článku [Offline synchronizace dat v mobilních aplikacích Azure].


##<a name="api-summary"></a>Rozhraní API souhrn

Podpora offline funkcí mobilních služeb, jsme použili rozhraní [IMobileServiceSyncTable] a inicializován [MobileServiceClient.SyncContext] [ synccontext] s místní databázi SQLite. V režimu offline normální operace CRUD pro mobilní aplikace pracovat jako aplikace je pořád připojení během operace dojít před v místním úložišti. Následujících metod se používají k synchronizaci v místním úložišti se serverem:

*  **[PushAsync]** Tento způsob je členem [IMobileServicesSyncContext], a proto se změny ve všech tabulkách posunou back-end. Pouze záznamy s místní změny se odesílají na server.

* **[PullAsync] ** 
   vložit je spuštěn z [IMobileServiceSyncTable]. Když jsou sledované změny v tabulce, implicitní nabízených spustit zajistit, aby všechny tabulky v místním úložišti spolu s relace zůstaly konzistentní. Parametr *pushOtherTables* Určuje, zda ostatní tabulky v kontextu odesílají v implicitní nabízených. Parametr *dotazu* , která bude [IMobileServiceTableQuery<T> ] [ IMobileServiceTableQuery] 
   nebo OData řetězce dotazu filtrovat vrácená data. Parametr *queryId* slouží k definování Přírůstková synchronizace. Další informace najdete v tématu  [Offline synchronizace dat v mobilních aplikacích Azure](app-service-mobile-offline-data-sync.md#how-sync-works).

* **[PurgeAsync]** Tuto metodu vyprázdnění zastaralá data z místního úložiště měli pravidelně volat aplikace. Pokud je potřeba vymazat všechny změny, které dosud nebyly synchronizovány používejte parametr *Vynutit* .

Další informace o těchto koncepty najdete v článku [Offline synchronizace dat v mobilních aplikacích Azure](app-service-mobile-offline-data-sync.md#how-sync-works).

## <a name="more-info"></a>Další informace

Další informace o funkci offline synchronizace mobilních aplikací v následujících tématech:

* [Synchronizace dat v režimu offline v Azure mobilní aplikace]
* [POSTUPY SDK .NET Azure mobilní aplikace][8]

<!-- Anchors. -->
[Update the app to support offline features]: #enable-offline-app
[Update the sync behavior of the app]: #update-sync
[Update the app to reconnect your Mobile Apps backend]: #update-online-app
[Next Steps]:#next-steps

<!-- Images -->
[1]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-reference-sqlite-dialog.png
[11]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-wp81-reference-sqlite-dialog.png
[13]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/cpu-architecture.png


<!-- URLs. -->
[Synchronizace dat v režimu offline v Azure mobilní aplikace]: app-service-mobile-offline-data-sync.md
[Vytvoření aplikace pro windows]: app-service-mobile-windows-store-dotnet-get-started.md
[SQLite for Windows 8.1]: http://go.microsoft.com/fwlink/?LinkID=716919
[SQLite for Windows Phone 8.1]: http://go.microsoft.com/fwlink/?LinkID=716920
[SQLite for Windows 10]: http://go.microsoft.com/fwlink/?LinkID=716921
[synccontext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[sqlite store nuget]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/
[IMobileServiceSyncTable]: https://msdn.microsoft.com/library/azure/mt691742(v=azure.10).aspx
[IMobileServiceTableQuery]: https://msdn.microsoft.com/library/azure/dn250631(v=azure.10).aspx
[IMobileServicesSyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.imobileservicesynccontext(v=azure.10).aspx
[MobileServicePushFailedException]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushfailedexception(v=azure.10).aspx
[Status]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushcompletionresult.status(v=azure.10).aspx
[CancelledByNetworkError]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushstatus(v=azure.10).aspx
[PullAsync]: https://msdn.microsoft.com/library/azure/mt667558(v=azure.10).aspx
[PushAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileservicesynccontextextensions.pushasync(v=azure.10).aspx
[PurgeAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.imobileservicesynctable.purgeasync(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
