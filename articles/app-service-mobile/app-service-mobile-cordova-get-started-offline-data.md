<properties
    pageTitle="Povolení offline synchronizace Azure mobilní aplikace (Cordova) | Microsoft Azure"
    description="Naučte se používat aplikaci služby mobilní aplikace do mezipaměti a synchronizace dat offline v aplikaci Cordova"
    documentationCenter="cordova"
    authors="adrianhall"
    manager="erikre"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-cordova-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-cordova-mobile-app"></a>Povolení offline synchronizace Cordova mobilní aplikaci

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

Tento kurz uvádí funkce offline synchronizace Azure mobilních aplikací pro Cordova. Offline synchronizace umožňuje koncovým uživatelům chcete provést interakci s mobilní aplikaci&mdash;zobrazení, přidání nebo úprava dat&mdash;i v případě žádné síťové připojení. Změny se ukládají do místní databázi. Po zase online zařízení se tyto změny se synchronizují s vzdálené služby.

Tento kurz se podle řešení Cordova rychlý úvod k aplikacím Mobile, který vytváříte při použití výukového [Apache Cordova Snadné spuštění]. V tomto kurzu se aktualizuje rychlý úvod řešení pro přidání offline funkce Azure mobilních aplikací. Také zvýrazní offline specifické pro kód v aplikaci.

Další informace o funkci offline synchronizace najdete v tématu [Offline synchronizace dat v mobilních aplikacích Azure]. Podrobné informace o použití rozhraní API najdete v tématu souboru [README] v modulu plug-in.

## <a name="add-offline-sync-to-the-quickstart-solution"></a>Rychlý úvod řešení přidávat offline synchronizace

Kód offline synchronizace musí být přidán k aplikaci. Offline synchronizace vyžaduje modul plug-in cordova sqlite úložiště, které získá když automaticky přidají do aplikace modul plug-in aplikací Azure Mobile je součástí projektu. Rychlý úvod project obsahuje obě tyto moduly plug-in.

1. V Průzkumníku Visual Studio řešení otevřete index.js a nahraďte následující kód

        var client,            // Connection to the Azure Mobile App backend
          todoItemTable;      // Reference to a table endpoint on backend

    s Tento kód:

        var client,             // Connection to the Azure Mobile App backend
          todoItemTable, syncContext; // Reference to table and sync context

2. Potom nahraďte následující kód:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');

    s Tento kód:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');

        // Note: Requires at least version 2.0.0-beta6 of the Azure Mobile Apps plugin
        var store = new WindowsAzure.MobileServiceSqliteStore('store.db');

        store.defineTable({
          name: 'todoitem',
          columnDefinitions: {
              id: 'string',
              text: 'string',
              deleted: 'boolean',
              complete: 'boolean'
          }
        });

        // Get the sync context from the client
        syncContext = client.getSyncContext();

    Předchozí přidaných kód inicializace v místním úložišti a definovat místní tabulku, která odpovídá hodnoty sloupce použité ve vaší Azure serverová. (Není potřeba zahrnout všechny hodnoty sloupce v tomto kódu.)

    Získat odkaz ke kontextu synchronizace tak, že zavoláte **getSyncContext**. Místní synchronizace pomáhá zachovat relací mezi tabulkami sledování a stisknutí změny ve všech tabulkách, které v klientské aplikaci změnil při volání **připínáčku** .

3. Adresa URL aplikace aktualizujte svoji adresu URL aplikace mobilní aplikaci.

4. Potom nahraďte tento kód:

        todoItemTable = client.getTable('todoitem'); // todoitem is the table name

    s Tento kód:

        // todoItemTable = client.getTable('todoitem');

        // Initialize the sync context with the store
        syncContext.initialize(store).then(function () {

        // Get the local table reference.
        todoItemTable = client.getSyncTable('todoitem');

        syncContext.pushHandler = {
            onConflict: function (serverRecord, clientRecord, pushError) {
                // Handle the conflict.
                console.log("Sync conflict! " + pushError.getError().message);
                // Update failed, revert to server's copy.
                pushError.cancelAndDiscard();
              },
              onError: function (pushError) {
                  // Handle the error
                  // In the simulated offline state, you get "Sync error! Unexpected connection failure."
                  console.log("Sync error! " + pushError.getError().message);
              }
        };

        // Call a function to perform the actual sync
        syncBackend();

        // Refresh the todoItems
        refreshDisplay();

        // Wire up the UI Event Handler for the Add Item
        $('#add-item').submit(addItemHandler);
        $('#refresh').on('click', refreshDisplay);

    Předchozí kód inicializuje kontextu synchronizovat a pak volá getSyncTable (místo jít) odkazovat na místní tabulky.

    Tento kód používá místní databázi pro všechny vytvořit, číst, aktualizovat a odstraňovat operace s tabulkou (CRUD).

    V tomto příkladu provede jednoduché zpracování chyb ve vzorcích na konflikty. Skutečné aplikaci by obsloužení chyb různých například stav sítě konflikty serveru a ostatní. Příklady kódu najdete v článku [ukázkové offline synchronizace].

5. Dále přidejte tuto funkci provádět skutečné synchronizace.

        function syncBackend() {

          // Sync local store to Azure table when app loads, or when login complete.
          syncContext.push().then(function () {
              // Push completed

          });

          // Pull items from the Azure table after syncing to Azure.
          syncContext.pull(new WindowsAzure.Query('todoitem'));
        }

    Je rozhodnout, kdy je vhodné použít změny pro mobilní aplikaci back-end tak, že zavoláte **nabízených** objektu **syncContext** používá klienta. Například můžete přidat volání **syncBackend** obslužné rutiny události tlačítka v aplikaci například nové tlačítko synchronizovat nebo můžete přidat volání funkce **addItemHandler** synchronizovat pokaždé, když se přidá nová položka.

##<a name="offline-sync-considerations"></a>Co byste měli zvážit offline synchronizace

Ve vzorku metodu **nabízených** **syncContext** nazývá pouze při spuštění aplikace ve funkci zpětné pro přihlášení.  V aplikaci reálný může taky uděláte tuto funkci synchronizace spouštěný ručně nebo při změně stavu sítě.

Při spuštění vložit před tabulku, která obsahuje čekající místní aktualizace sledovány kontextem operaci vyžádané automaticky aktivace předchozí nabízených kontextu. Při aktualizaci, přidávání a dokončení položky v tomto příkladu je možné vynechat explicitní **nabízených** volání a od může být nadbytečné (první kontrola [Soubor README] pro aktuální stav funkce).

Zadaný kód dotazovaném všechny záznamy v tabulce vzdálené todoItem, ale je také možné filtrovat záznamy předáním **nabízených**id dotazu a dotazů. Další informace naleznete v části *Přírůstková synchronizace* v [Offline synchronizace dat v mobilních aplikacích Azure].

## <a name="optional-disable-authentication"></a>(Volitelné) Zakázání ověřování

Pokud jste ještě nenastavili ověřování a nechcete, aby se nastavení ověřování před testování offline synchronizace, poznámky, funkce zpětné pro přihlášení, ale nechat kód uvnitř funkce zpětné uncommented.

Kód by měl vypadat takto po komentářích řádkům přihlášení.

      // Login to the service.
      // client.login('twitter')
      //    .then(function () {
        syncContext.initialize(store).then(function () {
          // Leave the rest of the code in this callback function  uncommented.
                ...
        });
      // }, handleError);

Aplikaci teď synchronizuje s Azure back-end při spuštění aplikace místo při přihlášení.

## <a name="run-the-client-app"></a>Spusťte aplikaci klienta

S teď povolení offline synchronizace můžete teď spustit klientské aplikaci aspoň jednou na každou platformu k naplnění databázi místním úložišti. Budou později simulovat offline scénář a měnit data v místním úložišti, když aplikace je offline.

## <a name="optional-test-the-sync-behavior"></a>(Volitelné) Otestování chování synchronizace

V této části upravíte projekt klienta tak, aby napodobily offline scénář pomocí Neplatná aplikace URL pro váš back-end. Při přidání nebo změna datové položky, tyto změny uskutečňuje v místním úložišti ale není synchronizovali se obchodu dat back-end po opětovném připojení.

1. V okně Průzkumník otevření souboru projektu index.js a změnit adresu URL aplikace tak, aby ukazovaly na neplatné adresu URL, třeba takto:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net-fail');

2. V index.html, aktualizované Zprostředkovatel `<meta>` elementem neplatné adresu URL.

        <meta http-equiv="Content-Security-Policy" content="default-src 'self' data: gap: http://yourmobileapp.azurewebsites.net-fail; style-src 'self'; media-src *">

3. Vytvořit a spustit aplikaci klienta a Všimněte si, že výjimku zaznamenané v konzole při aplikaci pokusí synchronizovat přes back-end po přihlášení. Nové položky, které přidáte bude existovat pouze v místním úložišti, dokud můžete posune mobilní back-end. Klient aplikace se chová jako kdybyste je připojen k back-end, podporuje všechny vytvořená číst, aktualizovat, operace odstranění (CRUD).

4. Ukončete aplikaci a restartujte ji můžete ověřit, že jsou k místním úložišti zachován nové položky, kterou jste vytvořili.

5. (Volitelné) Pomocí aplikace Visual Studio zobrazíte tabulky databáze SQL Azure zobrazíte, že se data z databáze back-end nezměnil.

    Ve Visual Studiu spusťte **Průzkumníka serveru**. Přejděte do databáze v **Azure**->**SQL databáze**. Klikněte pravým tlačítkem myši databáze a vyberte **Otevřít v Průzkumníkovi objektu SQL serveru**. Teď můžete přecházet na tabulky databáze SQL a její obsah.


## <a name="optional-test-the-reconnection-to-your-mobile-backend"></a>(Volitelné) Testování opětovné připojení k mobilní back-end

V tomto oddílu se znovu připojíte aplikace na mobilním back-end, které napodobuje aplikaci vraceli do stavu online. Když se přihlásíte, data synchronizovat do vašeho mobilního back-end.

1. Znovu otevřete index.js a opravte adresa URL aplikace tak, aby ukazovaly na správnou adresu URL.

2. Znovu otevřete index.html a oprava adresa URL aplikace v Zprostředkovatel `<meta>` prvek.

3. Znovu vytvořit a spustit aplikaci klienta. Aplikace se pokusí synchronizovat přes mobilní aplikaci back-end po přihlášení. Ověřte, že bez výjimek přihlášeni konzoly ladění.

4. (Volitelné) Zobrazení aktualizovaná data pomocí Průzkumníka objektu SQL serveru nebo nástroji ZBÝVAJÍCÍ jako Fiddler. Všimněte si, že data synchronizování mezi back-end databáze a v místním úložišti.

    Všimněte si data synchronizování mezi databáze a místní úložiště přihlašovacích údajů a obsahuje položek, které jste přidali při odpojení aplikace.

## <a name="additional-resources"></a>Další zdroje informací

* [Synchronizace dat v režimu offline v Azure mobilní aplikace]

* [Cloudu titulní: Offline synchronizace služby Azure mobilní] \(Poznámka: video o službách Mobile, ale offline synchronizace funguje podobně v mobilních aplikacích Azure\)

* [Visual Studio Tools for Apache Cordova]

## <a name="next-steps"></a>Další kroky

* Podívejte se na další rozšířené funkce offline synchronizace, jako jsou řešení konfliktů v [ukázkové offline synchronizace]
* Podívejte se na offline synchronizace rozhraní API odkaz v [souboru README]

<!-- ##Summary -->

<!-- Images -->

<!-- URLs. -->
[Představení aplikace Apache Cordova]: app-service-mobile-cordova-get-started.md
[SOUBOR README PRO]: https://github.com/Azure/azure-mobile-apps-js-client#offline-data-sync-preview
[Ukázka offline synchronizace]: https://github.com/shrishrirang/azure-mobile-apps-quickstarts/tree/samples/client/cordova/ZUMOAPPNAME
[Synchronizace dat v režimu offline v Azure mobilní aplikace]: app-service-mobile-offline-data-sync.md
[Titulních cloudu: Offline synchronizace v Azure mobilní služby]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Adding Authentication]: app-service-mobile-cordova-get-started-users.md
[authentication]: app-service-mobile-cordova-get-started-users.md
[Work with the .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Visual Studio Community 2015]: http://www.visualstudio.com/
[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
