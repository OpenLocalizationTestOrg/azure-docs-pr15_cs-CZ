<properties
    pageTitle="Povolení offline synchronizace Azure mobilní aplikace (iOS)"
    description="Naučte se používat aplikaci služby mobilní aplikace do mezipaměti a synchronizace dat offline v aplikaci iOS"
    documentationCenter="ios"
    authors="ysxu"
    manager="yochayk"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="enable-offline-sync-for-your-ios-mobile-app"></a>Povolení offline synchronizace pro mobilní aplikace pro iOS

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Základní informace

Tento kurz zahrnuje funkce offline synchronizace Azure mobilní aplikace pro iOS. Offline synchronizace umožňuje koncovým uživatelům chcete provést interakci s mobilní aplikaci&mdash;zobrazení, přidání nebo úprava dat&mdash;i v případě žádné síťové připojení. Změny se ukládají do místní databázi. Po návrat do online režimu zařízení se tyto změny se synchronizují s vzdálené back-end.

Pokud je to vaše první zkušenosti s aplikací Mobile Azure, byste měli dokončit kurz [Vytvoření iOS aplikace]. Pokud nepoužíváte project serveru stažený úvodní, je nutné přidat balíčků rozšíření dat aplikace access do projektu. Další informace o serveru rozšíření balíčků najdete v tématu [práce s back-end serveru .NET SDK Azure mobilních aplikací](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Další informace o funkci offline synchronizace najdete v tématu [Offline synchronizace dat v mobilních aplikacích Azure].

## <a name="review-sync"></a>Revize kód klienta synchronizace

Klient projekt, který jste si stáhli pro kurz [Vytvoření iOS aplikace] již obsahuje kód podpora offline synchronizace pomocí místní databázi na základě základních údajů. Tento oddíl je souhrn co je už součástí výuková kód. Přehled funkci najdete v článku [Offline synchronizace dat v mobilních aplikacích Azure].

Data v režimu offline synchronizace synchronizační funkce Azure mobilních aplikací koncoví uživatelé chcete provést interakci s místní databázi, když není network přístupných osobám s postižením. Použití těchto funkcí v aplikaci, inicializace kontextu synchronizace `MSClient` a odkaz na místním úložišti. Odkaz tabulky pomocí `MSSyncTable` rozhraní.

1. V **QSTodoService.m** (cíl-C) nebo **ToDoTableViewController.swift** (Swift), Všimněte si typ člena `syncTable` je `MSSyncTable`. Offline synchronizace používá tento synchronizace tabulky rozhraní namísto `MSTable`. Při použití tabulky synchronizovat všechny operace přejděte na místní úložiště a pouze synchronizovány s vzdálené back-end s explicitní operace nabízených a vložit.

    Odkaz na tabulku synchronizace, použijte metodu `syncTableWithName` na `MSClient`. Chcete-li odebrat funkce offline synchronizace, použijte `tableWithName` místo.

2. Předtím, než lze provést všechny operace tabulky, musí být spuštěn v místním úložišti. Tady je příslušný kód. 
    
    **Cíl C**:
    
    V `QSTodoService.init` metodu:
    
    
            MSCoreDataStore *store = [[MSCoreDataStore alloc] initWithManagedObjectContext:context];
            self.client.syncContext = [[MSSyncContext alloc] initWithDelegate:nil dataSource:store callback:nil];
    
    
    **Swift**:
    
    V `ToDoTableViewController.viewDidLoad` metodu:
    
    
            let client = MSClient(applicationURLString: "http:// ...") // URI of the Mobile App
            let managedObjectContext = (UIApplication.sharedApplication().delegate as! AppDelegate).managedObjectContext!
            self.store = MSCoreDataStore(managedObjectContext: managedObjectContext)
            client.syncContext = MSSyncContext(delegate: nil, dataSource: self.store, callback: nil)
    

    Tím vytvoříte místním úložišti pomocí rozhraní `MSCoreDataStore`, který není uvedený v mobilních aplikacích SDK. Můžete místo toho poskytovat různých místním úložišti implementací `MSSyncContextDataSource` protokol. 
    
    Také jako první parametr `MSSyncContext` slouží k určení rutiny konfliktu. Protože jsme uplynou `nil`, jsme pošle obslužné rutiny konflikt výchozí v libovolné konfliktu.
    
3. Teď Pojďme operaci skutečné synchronizace a načtení dat ze vzdáleného back-end.

    **Cíl C**:
    
    `syncData`nejdřív posune nové změny, pak zavolá `pullData` k načtení dat ze vzdáleného back-end. Zase, metodu `pullData` se nová data, která odpovídá dotazu:
    
    
            -(void)syncData:(QSCompletionBlock)completion
            {
                // push all changes in the sync context, then pull new data
                [self.client.syncContext pushWithCompletion:^(NSError *error) {
                    [self logErrorIfNotNil:error];
                    [self pullData:completion];
                }];
            }
    
            -(void)pullData:(QSCompletionBlock)completion
            {
                MSQuery *query = [self.syncTable query];
    
                // Pulls data from the remote server into the local table.
                // We're pulling all items and filtering in the view
                // query ID is used for incremental sync
                [self.syncTable pullWithQuery:query queryId:@"allTodoItems" completion:^(NSError *error) {
                    [self logErrorIfNotNil:error];
    
                    // Let the caller know that we have finished
                    if (completion != nil) {
                        dispatch_async(dispatch_get_main_queue(), completion);
                    }
                }];
            }
        
        
      **Swift**:
        
        
        func onRefresh(sender: UIRefreshControl!) {
            UIApplication.sharedApplication().networkActivityIndicatorVisible = true
            
            self.table!.pullWithQuery(self.table?.query(), queryId: "AllRecords") {
                (error) -> Void in
                
                UIApplication.sharedApplication().networkActivityIndicatorVisible = false
                
                if error != nil {
                    // A real application would handle various errors like network conditions,
                    // server conflicts, etc via the MSSyncContextDelegate
                    print("Error: \(error!.description)")
                    
                    // We will just discard our changes and keep the servers copy for simplicity
                    if let opErrors = error!.userInfo[MSErrorPushResultKey] as? Array<MSTableOperationError> {
                        for opError in opErrors {
                            print("Attempted operation to item \(opError.itemId)")
                            if (opError.operation == .Insert || opError.operation == .Delete) {
                                print("Insert/Delete, failed discarding changes")
                                opError.cancelOperationAndDiscardItemWithCompletion(nil)
                            } else {
                                print("Update failed, reverting to server's copy")
                                opError.cancelOperationAndUpdateItem(opError.serverItem!, completion: nil)
                            }
                        }
                    }
                }
                self.refreshControl?.endRefreshing()
            }
        } 
    
    
    Ve verzi cíle-C v `syncData`, doporučujeme nejdřív volání `pushWithCompletion` na místní synchronizace. Tento způsob je členem `MSSyncContext` (místo synchronizace tabulka samotné) vzhledem k tomu, že budou nabízená změny ve všech tabulek. Jenom záznamy, které byly změněny nějak místně (přes vytvoření operace) odešle na server. Klikněte Pomocník `pullData` je místo toho, který volání `MSSyncTable.pullWithQuery` k načtení Vzdálená data a jejich ukládání v místní databázi.
    
    Rychlé verze je bez volání `pushWithCompletion`. Je to proto operace nabízených nebyla nezbytných. Pokud jsou v kontextu synchronizace pro tabulku, která dělá operace nabízených žádné změny čekající na uložení, si ho naimportovat vždy problémy push nejdřív. Pokud máte víc než jednu tabulku synchronizace, je však nejlépe explicitně volat nabízených ověřit, jestli je všechno konzistentní přes souvisejících tabulek.
    
    Cíl-C a Swift verze, metodu `pullWithQuery` umožňuje zadat dotaz: Pokud chcete filtrovat záznamy chcete načíst. V tomto příkladu dotaz jenom vyhledá všechny záznamy v vzdáleného `TodoItem` tabulky.
    
    Druhý parametr `pullWithQuery` je ID dotazu, který se používá pro *Přírůstková synchronizace*. Přírůstková synchronizace načte pouze záznamy o změny od posledního synchronizace pomocí záznamu `UpdatedAt` časové razítko (s názvem `updatedAt` v místním úložišti.) ID dotazu by měl být popisný řetězce, které jsou jedinečné pro každý logické dotazu v aplikaci. Chcete-li odhlásit Přírůstková synchronizace, předejte `nil` jako ID dotazu. Všimněte si, že to může být potenciálně neefektivní, protože načte všechny záznamy na každou Vložit funkci.

5. Synchronizuje cíle-C aplikace, když nám upravit nebo přidat data, uživatel provádí gesto aktualizace a při spuštění. Rychlé aplikace synchronizuje když uživatel provádí gesto aktualizace a při spuštění. 

Protože aplikace synchronizuje pokaždé, když jsou data upravil (cíl-C) nebo při každém spuštění aplikace (cíl-C a Swift), aplikaci předpokládá, že uživatel online. Zadejte do jiného budeme aktualizovat aplikaci tak, aby uživatelé mohou upravovat, i když jsou v režimu offline.

## <a name="review-core-data"></a>Revize základní datového modelu

Pokud chcete použít základní offline úložiště, je třeba definovat konkrétní tabulek a polí v datovém modelu. Ukázka aplikace již obsahuje datový model s vhodný formát. V této části bude projdeme tyto tabulky a jejich použití.

- Otevřete **QSDataModel.xcdatamodeld**. Existují čtyři tabulky definovaný tři používaném SDK, a jednu tabulku úkol položky samotné:     *MS_TableOperations: pro sledování položek, které je potřeba synchronizovat se serverem    * MS_TableOperationErrors: sledování všechny chyby, ke kterým dojde při offline synchronizace     *MS_TableConfig: sledování poslední aktualizace dobu poslední synchronizace operace pro všechny operace vyžádané    * TodoItem : Pro ukládání položek úkol. Systém sloupce **createdAt**, **updatedAt**a **verze** jsou volitelné systém vlastnosti.

>[AZURE.NOTE] SDK Azure mobilní aplikace si vyhrazuje názvy sloupců, které jsou s "**``**". Tuto předponu neměli používat v jiném než systémové sloupce, jinak změněny názvy sloupců při použití vzdálené back-end.

- Při použití funkce offline synchronizace, musíte definovat systémových tabulek, jak je ukázáno v následujícím příkladu.

    ### <a name="system-tables"></a>Systémové tabulky

    **MS_TableOperations**

    ![][defining-core-data-tableoperations-entity]

  	| Atribut  |    Typ     |
  	|----------- |   ------    |
  	| ID         | Celé číslo 64  |
  	| itemId     | Řetězec      |
  	| Vlastnosti | Binární Data |
  	| tabulky      | Řetězec      |
  	| tableKind  | Celé číslo 16  |

    <br>**MS_TableOperationErrors**

    ![][defining-core-data-tableoperationerrors-entity]

  	| Atribut  |    Typ     |
  	|----------- |   ------    |
  	| ID         | Řetězec      |
  	| operationId | Celé číslo 64 |
  	| Vlastnosti | Binární Data |
  	| tableKind  | Celé číslo 16  |

    <br>**MS_TableConfig**

    ![][defining-core-data-tableconfig-entity]

  	| Atribut  |    Typ     |
  	|----------- |   ------    |
  	| ID         | Řetězec      |
  	| klíč        | Řetězec      |
  	| Typ klíče    | Celé číslo 64  |
  	| tabulky      | Řetězec      |
  	| Hodnota      | Řetězec      |

    ### <a name="data-table"></a>Tabulka dat

    **TodoItem**

  	| Atribut    |  Typ   | Poznámka:                                                   |
  	|-----------   |  ------ | -------------------------------------------------------|
  	| ID           | Řetězec označen jako požadovaný  | primární klíč v vzdálené úložiště                            |
  	| dokončení     | Logická hodnota | pole úkol položky                                        |
  	| text         | Řetězec  | pole úkol položky                                        |
  	| createdAt | Datum    | (volitelné) mapy createdAt systém vlastnost         |
  	| updatedAt | Datum    | (volitelné) mapy updatedAt systém vlastnost         |
  	| verze   | Řetězec  | (volitelné) používat ke zjištění konflikty, mapy verzi |


## <a name="setup-sync"></a>Změna chování synchronizační aplikace

V této části upravíte aplikace tak, aby nesynchronizuje při spuštění aplikace nebo při vkládání nebo aktualizace položek, ale jenom při provádění gesto tlačítko Aktualizovat.

**Cíl C**:

1. V **QSTodoListViewController.m**změnit způsob **viewDidLoad** odebrání volání `[self refresh]` na konci metodu. Teď data nebudou synchronizovat se serverem při spuštění aplikace a místo toho bude obsah místní úložiště.

2. V **QSTodoService.m**upravíte definici `addItem` tak, aby se nesynchronizuje po vložení položky. Odebrání `self syncData` blokovat a nahraďte následujícím:

            if (completion != nil) {
                dispatch_async(dispatch_get_main_queue(), completion);
            }

3. Úprava definice `completeItem` jako nahoře; odebrání bloku pro `self syncData` a nahraďte následujícím:

            if (completion != nil) {
                dispatch_async(dispatch_get_main_queue(), completion);
            }

**Swift**:

1. V `viewDidLoad` v **ToDoTableViewController.swift**poznámky, tyto dva řádky na Zastavit synchronizaci při spuštění aplikace. Při psaní v tomto článku aplikaci Swift úkol službu při aktualizaci někdo přidá nebo dokončí položky pouze při spuštění aplikace.

        self.refreshControl?.beginRefreshing()
        self.onRefresh(self.refreshControl)


## <a name="test-app"></a>Testování aplikace

V této části se připojíte ke neplatnou adresu URL, aby napodobily scénáři offline. Když přidáte položky dat, budou bude uskutečňuje v místním úložišti základních dat, ale není synchronizované s mobilním back-end.

1. Změnit adresu URL mobilní aplikace v **QSTodoService.m** neplatné adresy URL a znova spusťte aplikaci:

    **Cíl C** v QSTodoService.m:
    
            self.client = [MSClient clientWithApplicationURLString:@"https://sitename.azurewebsites.net.fail"];
    
    **Swift** v ToDoTableViewController.swift:

        let client = MSClient(applicationURLString: "https://sitename.azurewebsites.net.fail")

2. Přidání některé položky úkol. Ukončení simulator (nebo vynutit zavření aplikace) a restartujte. Ověřte, že byl zachován změny.

3. Zobrazení obsahu vzdálené TodoItem tabulky:

    + Pro Node.js back-end, přejděte na [portál Azure](https://portal.azure.com/)a v mobilní aplikaci back-end klikněte na **Snadno tabulky** > **TodoItem** zobrazíte obsah `TodoItem` tabulky.
    + .NET back-end zobrazení obsahu pomocí nástroje SQL třeba SQL Server Management Studio nebo ZBÝVAJÍCÍ klientů, jako je Fiddler nebo pošťák.

    Ověřte, že nové položky *mají neproběhla k serveru* :

4. Změnit adresu URL zpět správné na v **QSTodoService.m** a znovu spusťte aplikaci. Proveďte aktualizaci gesto roztažením dolů v seznamu položek. Zobrazí se průběhu číselník.

5. Zobrazení dat TodoItem znovu. Nové a změněné TodoItems by teď měla zobrazovat.

## <a name="summary"></a>Souhrn

Abyste mohli podporují funkce offline synchronizace, jsme použili `MSSyncTable` rozhraní a inicializovány `MSClient.syncContext` s místním úložišti. V tomto případě v místním úložišti je databáze na základě základních údajů.

Pokud chcete použít místní úložiště Core dat, je třeba definovat několik tabulek s [Vlastnosti správné systému](#review-core-data).

Normální operace CRUD pro Azure mobilní aplikace pracovat, jako je pořád připojení aplikace, ale všechny operace dojít před v místním úložišti.

Když chtěli jsme synchronizovat se serverem v místním úložišti, jsme použili `MSSyncTable.pullWithQuery`metody.


## <a name="additional-resources"></a>Další zdroje informací

* [Synchronizace dat v režimu offline v Azure mobilní aplikace]

* [Cloudu titulních: Offline synchronizace služby Azure mobilní] \(Poznámka: video o službách Mobile, ale offline synchronizace funguje podobně v mobilních aplikacích Azure\)

<!-- URLs. -->


[Vytvoření aplikace iOS]: app-service-mobile-ios-get-started.md
[Synchronizace dat v režimu offline v Azure mobilní aplikace]: app-service-mobile-offline-data-sync.md

[defining-core-data-tableoperationerrors-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperationerrors-entity.png
[defining-core-data-tableoperations-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperations-entity.png
[defining-core-data-tableconfig-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableconfig-entity.png
[defining-core-data-todoitem-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-todoitem-entity.png

[Obálka cloudu: Offline synchronizace v Azure mobilní služby]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/en-us/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/
