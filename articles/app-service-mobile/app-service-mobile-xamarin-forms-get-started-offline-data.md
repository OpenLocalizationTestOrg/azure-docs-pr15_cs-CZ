<properties
    pageTitle="Povolení offline synchronizace Azure mobilní aplikace (Xamarin.Forms) | Microsoft Azure"
    description="Naučte se používat aplikaci služby mobilní aplikace do mezipaměti a synchronizace dat offline v aplikaci Xamarin.Forms"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="yochayk"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-xamarinforms-mobile-app"></a>Povolení offline synchronizace Xamarin.Forms mobilní aplikaci

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Základní informace
Tento kurz uvádí funkce offline synchronizace Azure mobilních aplikací pro Xamarin.Forms. Offline synchronizace koncoví uživatelé chcete provést interakci s v mobilní aplikaci – zobrazení, přidání nebo úprava dat – i v případě žádné síťové připojení. Změny se ukládají do místní databázi. Po zase online zařízení se tyto změny se synchronizují s vzdálené služby.

Tento kurz se podle Xamarin.Forms rychlý úvod řešení pro mobilní aplikace, kterou vytvoříte po dokončení kurzu [vytvořit aplikace iOS Xamarin]. Rychlý úvod řešení Xamarin.Forms obsahuje kód podpory offline synchronizace, které právě, musí být aktivní. V tomto kurzu aktualizujete řešení rychlý úvod zapnout offline funkce Azure mobilní aplikace. Jsme také zvýraznit offline specifické pro kód v aplikaci. Pokud nepoužíváte stažený rychlý úvod řešení, musíte přidat balíčků rozšíření dat aplikace access do projektu. Další informace o serveru rozšíření balíčků najdete v tématu [práce s back-end serveru .NET SDK Azure mobilních aplikací][1].

Další informace o funkci offline synchronizace najdete v tématu [Offline synchronizace dat v mobilních aplikacích Azure][2].

## <a name="enable-offline-sync-functionality-in-the-quickstart-solution"></a>Povolení funkce offline synchronizace v řešení rychlý úvod

Offline synchronizace kód je součástí projektu pomocí C# předprocesoru. Když **OFFLINE\_SYNCHRONIZACE\_povoleno** definované symbol, stezky kód jsou součástí sestavení. Aplikace pro Windows nainstalujte platformu SQLite.

1. Ve Visual Studiu, klikněte pravým tlačítkem myši na řešení > **Spravovat NuGet balíčků řešení...**, a pak vyhledat a nainstalovat **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet pro všechny projekty na řešení.

2. V okně Průzkumník řešení otevřete soubor TodoItemManager.cs z projektu s **Portable** v názvu, který je přenosný knihovna tříd projektu, potom zrušte komentář následující preprocesoru směrnice:

        #define OFFLINE_SYNC_ENABLED

4. (Volitelné) Podpora zařízení Windows, nainstalujte jednu z následujících balíčků runtime SQLite:

    * **Windows 8.1 Runtime:** Instalace [SQLite ve Windows 8.1][3].
    * **Windows Phone 8.1:** Instalace [SQLite pro Windows Phone 8.1][4].
    * **Univerzální Windows platformy** Instalace [SQLite univerzální univerzální Windows][5].

    I když rychlý úvod neobsahuje univerzální Windows projektu, platformu univerzální Windows podporovaná pomocí služby Xamarin formuláře.

5. (Volitelné) V aplikaci každý Windows projektu klikněte pravým tlačítkem myši **odkazy** > **Přidat odkaz**, rozbalte složku **Windows** > **rozšíření**.
    Povolení příslušných **SQLite pro Windows** SDK spolu s **Vizuální C++ 2013 Runtime pro systém Windows** SDK.
    Názvy SQLite SDK se liší podle mírně každou platformu Windows.

## <a name="review-the-client-sync-code"></a>Revize kód klienta synchronizace

Tady je stručný přehled toho, co je už součástí výuková kód uvnitř `#if OFFLINE_SYNC_ENABLED` směrnice. Funkce offline synchronizace jsou v souboru projektu TodoItemManager.cs v přenosném knihovna tříd projektu. Přehled funkci najdete v článku [Offline synchronizace dat v mobilních aplikacích Azure][2].

* Předtím, než lze provést všechny operace tabulky, musí být spuštěn v místním úložišti. Databáze místní úložiště iniciovány v konstruktoru **TodoItemManager** třídy pomocí následujícího kódu:

        var store = new MobileServiceSQLiteStore(OfflineDbPath);
        store.DefineTable<TodoItem>();

        //Initializes the SyncContext using the default IMobileServiceSyncHandler.
        this.client.SyncContext.InitializeAsync(store);

        this.todoTable = client.GetSyncTable<TodoItem>();

    Tento kód vytvoří novou místní databázi SQLite pomocí třídy **MobileServiceSQLiteStore** .

    Metoda **DefineTable** vytvoří tabulku v místním úložišti odpovídající polím v poskytnutého typu.  Typ nemá zahrnout všechny sloupce, které jsou obsaženy v vzdálené databázi. Je možné uložit podmnožinu sloupců.

* Pole **todoTable** v **TodoItemManager** je typ **IMobileServiceSyncTable** místo **IMobileServiceTable**. Tato třídy používá místní databázi pro všechny vytvořit, číst, aktualizovat a odstraňovat operace s tabulkou (CRUD). Se rozhodnete tyto změny se posunou back-end mobilní aplikaci tak, že zavoláte na **IMobileServiceSyncContext** **PushAsync** . Místní synchronizace pomáhá zachovat relací mezi tabulkami sledování a stisknutí změny ve všech tabulkách, které v klientské aplikaci změnil při volání **PushAsync** .

    Podle pokynů pro **SyncAsync** je místo toho možnost Synchronizovat s back-end mobilní aplikace:

        public async Task SyncAsync()
        {
            ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

            try
            {
                await this.client.SyncContext.PushAsync();

                await this.todoTable.PullAsync(
                    "allTodoItems",
                    this.todoTable.CreateQuery());
            }
            catch (MobileServicePushFailedException exc)
            {
                if (exc.PushResult != null)
                {
                    syncErrors = exc.PushResult.Errors;
                }
            }

            // Simple error/conflict handling.
            if (syncErrors != null)
            {
                foreach (var error in syncErrors)
                {
                    if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
                    {
                        //Update failed, reverting to server's copy.
                        await error.CancelAndUpdateItemAsync(error.Result);
                    }
                    else
                    {
                        // Discard local change.
                        await error.CancelAndDiscardItemAsync();
                    }

                    Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.",
                        error.TableName, error.Item["id"]);
                }
            }
        }

    V tomto příkladu jednoduché zpracování chyb ve vzorcích s výchozí synchronizace pro zpracování. Skutečné aplikaci by obsloužení různých chyb jako stav sítě a server konflikty pomocí vlastní implementaci **IMobileServiceSyncHandler** .

## <a name="offline-sync-considerations"></a>Co byste měli zvážit offline synchronizace

Ve vzorku metodu **SyncAsync** pouze volat při spuštění a při požadavku na synchronizace.  Zahajte synchronizace v aplikaci pro s Androidem nebo iOS, rozbalovací na položky seznamu. pro systém Windows použijte tlačítko **synchronizovat** . V aplikaci reálný může taky uděláte synchronizace aktivační události při změně stavu síť.

Pokud vyžádané spouštět oproti tabulku obsahující pole Čekání na místní aktualizace sledovány kontextem, který vyžádat operace automaticky aktivačních událostí předchozí nabízených kontextu. Při aktualizaci, přidávání a dokončení položky v tomto příkladu, je možné vynechat explicitní **PushAsync** volání.

Zadaný kód dotazovaném všechny záznamy v tabulce vzdálené TodoItem, ale je také možné filtrovat záznamy předáním **PushAsync**id dotazu a dotazů. Další informace naleznete v části *Přírůstková synchronizace* v [Offline synchronizace dat v mobilních aplikacích Azure][2].

## <a name="run-the-client-app"></a>Spusťte aplikaci klienta

S teď povolení offline synchronizace spouštět aspoň jednou klientské aplikaci na každou platformu k naplnění databázi místní úložiště přihlašovacích údajů. Později simulovat offline scénář a měnit data v místním úložišti, když aplikace je offline.

## <a name="update-the-sync-behavior-of-the-client-app"></a>Aktualizace synchronizační chování aplikace klienta

V této části upravte projekt klienta tak, aby napodobily offline situace s použitím adresy URL Neplatná aplikace pro váš back-end. Můžete taky můžete vypnout síťových připojení přesunutím zařízení na "Letadla ve režim."  Pokud přidat nebo změnit datové položky, tyto změny uskutečňuje v místním úložišti, ale není synchronizovali se obchodu dat back-end po opětovném připojení.

1. V okně Průzkumník otevření souboru projektu Constants.cs z **přenosném** projektu a změňte hodnotu `ApplicationURL` tak, aby ukazovaly na neplatnou adresu URL:

        public static string ApplicationURL = @"https://your-service.azurewebsites.net/";

2. Otevřete soubor TodoItemManager.cs z **přenosná** projektu a přidejte **akci... skutečné** bloku v **SyncAsync** **zachycení** pro základní třídy **výjimky** . Tento blok **zachytit** data zapisuje zpráva o výjimce ke konzole, následujícím způsobem:

            catch (Exception ex)
            {
                Console.Error.WriteLine(@"Exception: {0}", ex.Message);
            }

3. Vytvoření a spuštění aplikace klienta.  Přidáte nové položky. Všimněte si, že výjimku zaznamenané v konzole pro každý pokus o synchronizaci s back-end. Tyto nové položky, které pouze v místním úložišti dokud můžete posune mobilní back-end. Klient aplikace se chová jako kdybyste je připojené k back-end, podporuje všechny vytvořená číst, aktualizovat, operace odstranění (CRUD).

4. Ukončete aplikaci a restartujte ji můžete ověřit, že jsou k místním úložišti zachován nové položky, kterou jste vytvořili.

5. (Volitelné) Pomocí aplikace Visual Studio zobrazíte tabulky databáze SQL Azure zobrazíte, že se data z databáze back-end nezměnil.

    Ve Visual Studiu spusťte **Průzkumníka serveru**. Přejděte do databáze v **Azure**->**SQL databáze**. Klikněte pravým tlačítkem myši databáze a vyberte **Otevřít v Průzkumníkovi objektu SQL serveru**. Teď můžete přecházet na tabulky databáze SQL a její obsah.

## <a name="update-the-client-app-to-reconnect-your-mobile-backend"></a>Aktualizace aplikace na klienta připojit mobilní back-end

V této části znovu aplikace na mobilním back-end, které napodobuje aplikaci vraceli do stavu online. Při provádění gesto aktualizace dat synchronizovat do vašeho mobilního back-end.

1. Znovu otevřete Constants.cs. Oprava `applicationURL` tak, aby ukazovaly na správnou adresu URL.

2. Znovu vytvořit a spustit aplikaci klienta. Aplikace se pokusí synchronizovat přes mobilní aplikaci back-end po spuštění. Ověřte, že bez výjimek přihlášeni konzoly ladění.

3. (Volitelné) Zobrazení aktualizovaná data pomocí Průzkumníka objektu SQL serveru nebo nástroji ZBÝVAJÍCÍ jako Fiddler nebo [pošťák][6]. Všimněte si, že data synchronizování mezi back-end databáze a v místním úložišti.

    Všimněte si data synchronizování mezi databáze a místní úložiště přihlašovacích údajů a obsahuje položek, které jste přidali při odpojení aplikace.

## <a name="additional-resources"></a>Další zdroje informací

* [Synchronizace dat v režimu offline v Azure mobilní aplikace][2]
* [POSTUPY SDK .NET Azure mobilní aplikace][8]

<!-- URLs. -->
[1]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[2]: app-service-mobile-offline-data-sync.md
[3]: http://go.microsoft.com/fwlink/p/?LinkID=716919
[4]: http://go.microsoft.com/fwlink/p/?LinkID=716920
[5]: http://sqlite.org/2016/sqlite-uwp-3120200.vsix
[6]: https://www.getpostman.com/
[7]: http://www.telerik.com/fiddler
[8]: app-service-mobile-dotnet-how-to-use-client-library.md