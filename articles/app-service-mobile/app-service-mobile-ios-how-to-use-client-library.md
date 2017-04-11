<properties
    pageTitle="Jak používat iOS SDK Azure mobilní aplikace"
    description="Jak používat iOS SDK k aplikacím Mobile Azure"
    services="app-service\mobile"
    documentationCenter="ios"
    authors="ysxu"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="how-to-use-ios-client-library-for-azure-mobile-apps"></a>Jak používat iOS klienta v knihovně Azure mobilní aplikace

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Tato příručka učí můžete provádět běžné scénáře pomocí nejnovější [Azure mobilní aplikace iOS SDK][1]. Pokud začínáte na Azure mobilní aplikace první dokončení [Azure mobilní aplikace rychlý Start] vytvořit back-end, vytvořte tabulku a stáhněte projektu Xcode předdefinovaných iOS. V této příručce jsme zaměřit na straně klienta iOS SDK. Další informace o straně serveru SDK back-end, najdete v článku HOWTOs SDK serveru.

## <a name="reference-documentation"></a>Si přečtěte následující dokumentaci odkaz

Tady je umístěn odkaz si přečtěte následující dokumentaci pro iOS klienta SDK: [Azure mobilní aplikace iOS klienta odkaz][2].

## <a name="supported-platforms"></a>Podporované platformy

IOS SDK podporuje projekty cíle-C, Swift 2.2 projekty a Swift 2.3 projektů pro iOS 8.0 nebo novější verze.

"Server toku" ověřování používá webového zobrazení pro prezentovaný uživatelského rozhraní.  Je-li zařízení není možné zobrazení webového zobrazení uživatelského rozhraní, požaduje jiný metody ověřování, který je mimo rozsah produktu.  
Proto není vhodná pro typ kukátka podobně zamčeném zařízení nebo tato SDK.

##<a name="Setup"></a>Instalační program a požadavky

Tato příručka předpokládá, že jste vytvořili back-end s tabulkou. Tato příručka předpokládá, že v tabulce má stejné schéma jako tabulky v těchto kurzech. Tato příručka také se předpokládá, že v kódu jazyka odkazujete `MicrosoftAzureMobile.framework` a import `MicrosoftAzureMobile/MicrosoftAzureMobile.h`.

##<a name="create-client"></a>Postup: vytvoření klienta

Přístup k backendovou Azure mobilní aplikace v projektu, vytvoření `MSClient`. Nahrazení `AppUrl` s adresou URL aplikace. Můžete ponechat `gatewayURLString` a `applicationKey` prázdné. Pokud jste nastavili bránu pro ověřování, naplnění `gatewayURLString` s adresou URL brány.

**Cíl C**:

```
MSClient *client = [MSClient clientWithApplicationURLString:@"AppUrl"];
```

**Swift**:

```
let client = MSClient(applicationURLString: "AppUrl")
```


##<a name="table-reference"></a>Postup: vytvoření odkaz na tabulku

Access nebo aktualizace dat vytvořit odkaz na tabulku back-end. Nahrazení `TodoItem` s názvem tabulky

**Cíl C**:

```
MSTable *table = [client tableWithName:@"TodoItem"];
```

**Swift**:

```
let table = client.tableWithName("TodoItem")
```


##<a name="querying"></a>Postup: Data dotazu

Chcete-li vytvořit dotaz na databázi, zadejte dotaz `MSTable` objektu. Následující dotaz může vstoupit všechny položky `TodoItem` a protokoly text jednotlivých položek.

**Cíl C**:

```
[table readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) { // error is nil if no error occured
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) { // items is NSArray of records that match query
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Swift**:

```
table.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

##<a name="filtering"></a>Postup: Filtr vrátil Data

K filtrování výsledků, jsou k dispozici řadu možností.

Filtrování pomocí predikátem, použijte `NSPredicate` a `readWithPredicate`. Následující filtry vrátí data, která chcete najít pouze neúplné úkol.

**Cíl C**:

```
// Create a predicate that finds items where complete is false
NSPredicate * predicate = [NSPredicate predicateWithFormat:@"complete == NO"];
// Query the TodoItem table
[table readWithPredicate:predicate completion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Swift**:

```
// Create a predicate that finds items where complete is false
let predicate =  NSPredicate(format: "complete == NO")
// Query the TodoItem table
table.readWithPredicate(predicate) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

##<a name="query-object"></a>Postup: pomocí MSQuery

Lze provést složitý dotaz (včetně řazení a stránkování), vytvoření `MSQuery` objektu, přímo nebo pomocí predikátem:

**Cíl C**:

```
MSQuery *query = [table query];
MSQuery *query = [table queryWithPredicate: [NSPredicate predicateWithFormat:@"complete == NO"]];
```

**Swift**:

```
let query = table.query()
let query = table.queryWithPredicate(NSPredicate(format: "complete == NO"))
```

`MSQuery`vám umožní určit několik chování dotazu.

* Určení pořadí výsledků
* Omezení polí, která chcete vrátit
* Omezte počet záznamů vrátit
* Určení celkový počet různých hodnot v odpovědi
* Zadejte vlastní dotaz řetězec parametry v žádosti o
* Použití dalších funkcí

Spuštění `MSQuery` dotazu tak, že zavoláte `readWithCompletion` na požadovaném objektu.

## <a name="sorting"></a>Postup: řazení dat pomocí MSQuery

Pokud chcete řadit výsledky, Podívejme se na příklad. Pokud chcete řadit podle pole "text" vzestupně a pak podle "dokončení" sestupně, vyvolat `MSQuery` třeba takto:

**Cíl C**:

```
[query orderByAscending:@"text"];
[query orderByDescending:@"complete"];
[query readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Swift**:

```
query.orderByAscending("text")
query.orderByDescending("complete")
query.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```


## <a name="selecting"></a><a name="parameters"></a>Postup: omezení polí a rozbalte položku parametry řetězce dotazu s MSQuery

Pokud chcete omezit pole budou vráceny v dotazu, zadejte názvy polí ve vlastnosti **selectFields** . Tento příklad vrátí jenom text a dokončené pole:

**Cíl C**:

```
query.selectFields = @[@"text", @"complete"];
```

**Swift**:

```
query.selectFields = ["text", "complete"]
```

Obsahovat další řetězec parametry dotazu v pozvánce na serveru (například z toho důvodu je používá vlastní skript serverovou), naplnění `query.parameters` třeba takto:

**Cíl C**:

```
query.parameters = @{
    @"myKey1" : @"value1",
    @"myKey2" : @"value2",
};
```

**Swift**:

```
query.parameters = ["myKey1": "value1", "myKey2": "value2"]
```

## <a name="paging"></a>Jak: Konfigurace velikost stránky

S aplikací Mobile Azure řídí velikost stránky počtu zobrazených záznamů, které jsou doplněné v jednom z tabulek back-end. Volání `pull` data by pak dávkové dat založené na této velikosti stránky, dokud nejsou žádné další záznamy na něj.

Je možné ji nakonfigurovat velikost stránky pomocí **MSPullSettings** , jak je ukázáno v následujícím příkladu. Výchozí velikost stránky je 50 a níže uvedeném příkladu změní na 3.

Lze nakonfigurovat s jinou velikostí stránek důvodů výkonu. Pokud máte velký počet záznamů malé dat, velikosti vysoké stránky snižuje počet opakované serveru. 

Tohle nastavení určuje pouze velikost stránky na straně klienta. Pokud klient požaduje větší velikost stránky než back-end mobilní aplikace podporuje, velikost stránky uzavřeny v maximální back-end je nastavený na podporu. 

Toto nastavení je také _číslo_ záznamy, ne _velikost v bajtech_.

Pokud zvětšíte klienta stránky, [měli byste taky zvýšit velikost stránky na serveru](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#_how-to-adjust-the-table-paging-size).

**Cíl C**:

```
  MSPullSettings *pullSettings = [[MSPullSettings alloc] initWithPageSize:3];
  [table  pullWithQuery:query queryId:@nil settings:pullSettings
                        completion:^(NSError * _Nullable error) {
                               if(error) {
                    NSLog(@"ERROR %@", error);
                } 
                           }];
```


**Swift**:

```
let pullSettings = MSPullSettings(pageSize: 3)
table.pullWithQuery(query, queryId:nil, settings: pullSettings) { (error) in
    if let err = error {
        print("ERROR ", err)
    } 
}
```

##<a name="inserting"></a>Postup: vložení dat

Pokud chcete vložit nový řádek tabulky, vytváření `NSDictionary` a vyvolat `table insert`. Pokud je povoleno [Dynamické schématu] back-end mobilní aplikaci služby Azure automaticky vygeneruje nové sloupce na základě `NSDictionary`.

Pokud `id` není uvedeno, back-end automaticky vygeneruje nové jedinečné ID. Zadejte vlastní `id` pomocí e-mailové adresy, uživatelská jména, nebo vlastní vlastní hodnoty jako ID. Poskytování vlastní ID můžete zjednodušit spojení a logika orientovaného firmy databáze.

`result` Obsahuje nová položka vloženého. V závislosti na logiku serveru může obsahovat další či změněná data ve srovnání s co předané na server.

**Cíl C**:

```
NSDictionary *newItem = @{@"id": @"custom-id", @"text": @"my new item", @"complete" : @NO};
[table insert:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Swift**:

```
let newItem = ["id": "custom-id", "text": "my new item", "complete": false]
table.insert(newItem) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

##<a name="modifying"></a>Postup: změnit Data

K aktualizaci stávajícího řádku, upravit položku a volání `update`:

**Cíl C**:

```
NSMutableDictionary *newItem = [oldItem mutableCopy]; // oldItem is NSDictionary
[newItem setValue:@"Updated text" forKey:@"text"];
[table update:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Swift**:

```
if let newItem = oldItem.mutableCopy() as? NSMutableDictionary {
    newItem["text"] = "Updated text"
    table2.update(newItem as [NSObject: AnyObject], completion: { (result, error) -> Void in
        if let err = error {
            print("ERROR ", err)
        } else if let item = result {
            print("Todo Item: ", item["text"])
        }
    })
}
```

Můžete taky zadáte Identifikátor řádku a aktualizované pole:

**Cíl C**:

```
[table update:@{@"id":@"custom-id", @"text":"my EDITED item"} completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Swift**:

```
table.update(["id": "custom-id", "text": "my EDITED item"]) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

Minimálně `id` atributu musí být nastavený při vytváření aktualizace.

##<a name="deleting"></a>Postup: odstranění dat

Odstranit položku vyvolat `delete` s položkou:

**Cíl C**:

```
[table delete:item completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**Swift**:

```
table.delete(newItem as [NSObject: AnyObject]) { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

Můžete taky odstraňte zadáním Identifikátoru řádku:

**Cíl C**:

```
[table deleteWithId:@"37BBF396-11F0-4B39-85C8-B319C729AF6D" completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**Swift**:

```
table.deleteWithId("37BBF396-11F0-4B39-85C8-B319C729AF6D") { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

Minimálně `id` atributu musí být nastavený při provádění odstraní.

##<a name="customapi"></a>Postup: volání vlastní API

Vlastní rozhraním API můžete vystavit všechny funkce back-end. Nemá přiřadit operace tabulka. Nejen získáte větší kontrolu nad zasílání zpráv, můžete i pro čtení a nastavit záhlaví a změnit formát textu odpovědi. Naučte se vytvářet vlastní rozhraní API na back-end, přečtěte si [Vlastní rozhraní API](app-service-mobile-node-backend-how-to-use-server-sdk.md#work-easy-apis)

Chcete-li zavolat vlastní rozhraní API, zavolejte `MSClient.invokeAPI`. Žádostí a odpovědí obsah je považováno za JSON. Použití jiných typů médií [použít jiné přetížení `invokeAPI` ] [ 5].  Aby `GET` požádat o místo `POST` zadat požadavek, nastavení parametrů `HTTPMethod` k `"GET"` a parametr `body` k `nil` (protože požadavky na získání nemají zpráv.) Pokud váš vlastní rozhraní API podporuje jiné akce protokolu HTTP, změnit `HTTPMethod` řádně podporovat.

**Cíl C**:

```
[self.client invokeAPI:@"sendEmail"
                  body:@{ @"contents": @"Hello world!" }
            HTTPMethod:@"POST"
            parameters:@{ @"to": @"bill@contoso.com", @"subject" : @"Hi!" }
               headers:nil
            completion: ^(NSData *result, NSHTTPURLResponse *response, NSError *error) {
                if(error) {
                    NSLog(@"ERROR %@", error);
                } else {
                    // Do something with result
                }
            }];
```

**Swift**:

```
client.invokeAPI("sendEmail",
            body: [ "contents": "Hello World" ],
            HTTPMethod: "POST",
            parameters: [ "to": "bill@contoso.com", "subject" : "Hi!" ],
            headers: nil)
            {
                (result, response, error) -> Void in
                if let err = error {
                    print("ERROR ", err)
                } else if let res = result {
                          // Do something with result
                }
        }
```

##<a name="templates"></a>Postup: registrace nabízených šablony o neodeslání oznámení a platformy

Zaregistrujte šablony, předejte šablony s způsobu **client.push registerDeviceToken** v klientské aplikaci.

**Cíl C**:

```
[client.push registerDeviceToken:deviceToken template:iOSTemplate completion:^(NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    }
}];
```

**Swift**:

```
    client.push?.registerDeviceToken(NSData(), template: iOSTemplate, completion: { (error) in
        if let err = error {
            print("ERROR ", err)
        }
    })
```

Šablony jsou typu NSDictionary a může obsahovat více šablon ve formátu následující:

**Cíl C**:

```
NSDictionary *iOSTemplate = @{ @"templateName": @{ @"body": @{ @"aps": @{ @"alert": @"$(message)" } } } };
```

**Swift**:

```
let iOSTemplate = ["templateName": ["body": ["aps": ["alert": "$(message)"]]]]
```

Všechny značky vynechány požadavku cenného papíru.  Přidání značky na zařízení nebo šablony v zařízení, najdete v článku [Práce se serverem back-end .NET SDK k aplikacím Mobile Azure][4].  Odeslat oznámení pomocí těchto registrovaných šablon, pracovat s [Oznámení rozbočovače API][3].

##<a name="errors"></a>Postup: zpracování chyb

Při volání aplikaci služby Azure mobilní back-end blok dokončení obsahuje `NSError` parametr. Když dojde k chybě, je tento parametr není nula. Ve vašem kódu zkontrolujte tento parametr a zpracování chyb v případě potřeby, jak je ukázáno v předchozím fragmenty kódu.

Soubor [`<WindowsAzureMobileServices/MSError.h>`] [6] definuje konstanty `MSErrorResponseKey`, `MSErrorRequestKey`, a `MSErrorServerItemKey`. Pokud chcete získat další data týkající se chyby:

**Cíl C**:

```
NSDictionary *serverItem = [error.userInfo objectForKey:MSErrorServerItemKey];
```

**Swift**:

```
let serverItem = error.userInfo[MSErrorServerItemKey]
```

Kromě toho soubor definuje konstanty pro každý kód chyby:

**Cíl C**:

```
if (error.code == MSErrorPreconditionFailed) {
```

**Swift**:

```
if (error.code == MSErrorPreconditionFailed) {
```

## <a name="adal"></a>Postup: ověření uživatelé, kteří mají Active Directory Authentication Library

Můžete Active Directory ověřování knihovny (ADAL) a přihlaste se do aplikace pomocí služby Azure Active Directory uživatele. Klient tok ověřování pomocí zprostředkovatelem identit SDK je výhodnější `loginWithProvider:completion:` metody.  Klient tok ověřování obsahuje více nativní chování činnosti koncového uživatele a umožňuje dalších úprav.

1. Konfigurace back-end vaše mobilní aplikace pro přihlášení AAD tak, že po [tom, jak nakonfigurovat aplikaci služby Active Directory přihlášení] [ 7] kurzu. Zkontrolujte, že uděláte volitelné registrace aplikace native client –. Pro iOS, doporučujeme, aby přesměrování identifikátor URI je formuláře `<app-scheme>://<bundle-id>`. Další informace najdete v tématu [Rychlý úvod ADAL iOS][8].

2. Nainstalujte ADAL pomocí Cocoapods. Úprava Podfile zahrnout následující definice nahraďte názvem projektu Xcode **Svoje projektu** :

        source 'https://github.com/CocoaPods/Specs.git'
        link_with ['YOUR-PROJECT']
        xcodeproj 'YOUR-PROJECT'

   a Pod:

        pod 'ADALiOS'

3. Pomocí terminál spouštět `pod install` z adresáře obsahující projektu a potom otevřete vygenerovaných pracovního prostoru Xcode (není projekt).

4. Přidání kódu pro následující aplikace podle jazyk, který používáte. V každém Zkontrolujte tyto nahrazení:

    * **Vložení AUTORITA TADY** nahraďte názvem klienta, ve kterém zřízení aplikace. Formát by měl být https://login.windows.net/contoso.onmicrosoft.com. Tato hodnota můžete zkopírovat z karty Domain Azure Active Directory [Azure klasické portálu].
    * **Vložení zdroje ID TADY** nahraďte ID klienta pro mobilní aplikaci backendovou. Na kartě **Upřesnit** v části **Nastavení služby Azure Active Directory** v portálu můžete získat kód klienta.
    * Nahraďte **Vložení klienta ID TADY** jste zkopírovali z aplikace native client – ID klienta.
    * Nahraďte **Vložení PŘESMĚROVÁNÍ URI TADY** svého webu _/.auth/login/done_ koncový bod, pomocí schématu HTTPS. Tato hodnota by měl být stejný _https://contoso.azurewebsites.net/.auth/login/done_.

**Cíl C**:

    #import <ADALiOS/ADAuthenticationContext.h>
    #import <ADALiOS/ADAuthenticationSettings.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*))completionBlock;
    {
        NSString *authority = @"INSERT-AUTHORITY-HERE";
        NSString *resourceId = @"INSERT-RESOURCE-ID-HERE";
        NSString *clientId = @"INSERT-CLIENT-ID-HERE";
        NSURL *redirectUri = [[NSURL alloc]initWithString:@"INSERT-REDIRECT-URI-HERE"];
        ADAuthenticationError *error;
        ADAuthenticationContext *authContext = [ADAuthenticationContext authenticationContextWithAuthority:authority error:&error];
        authContext.parentController = parent;
        [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
        [authContext acquireTokenWithResource:resourceId
                                     clientId:clientId
                                  redirectUri:redirectUri
                              completionBlock:^(ADAuthenticationResult *result) {
                                  if (result.status != AD_SUCCEEDED)
                                  {
                                      completionBlock(nil, result.error);;
                                  }
                                  else
                                  {
                                      NSDictionary *payload = @{
                                                                @"access_token" : result.tokenCacheStoreItem.accessToken
                                                                };
                                      [client loginWithProvider:@"aad" token:payload completion:completionBlock];
                                  }
                              }];
    }


**Swift**:

    // add the following imports to your bridging header:
    //      #import <ADALiOS/ADAuthenticationContext.h>
    //      #import <ADALiOS/ADAuthenticationSettings.h>

    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let authority = "INSERT-AUTHORITY-HERE"
        let resourceId = "INSERT-RESOURCE-ID-HERE"
        let clientId = "INSERT-CLIENT-ID-HERE"
        let redirectUri = NSURL(string: "INSERT-REDIRECT-URI-HERE")
        var error: AutoreleasingUnsafeMutablePointer<ADAuthenticationError?> = nil
        let authContext = ADAuthenticationContext(authority: authority, error: error)
        authContext.parentController = parent
        ADAuthenticationSettings.sharedInstance().enableFullScreen = true
        authContext.acquireTokenWithResource(resourceId, clientId: clientId, redirectUri: redirectUri) { (result) in
                if result.status != AD_SUCCEEDED {
                    completion(nil, result.error)
                }
                else {
                    let payload: [String: String] = ["access_token": result.tokenCacheStoreItem.accessToken]
                    client.loginWithProvider("aad", token: payload, completion: completion)
                }
            }
    }

## <a name="facebook-sdk"></a>Postup: ověření uživatelé, kteří mají SDK Facebook pro iOS

Můžete SDK Facebook pro iOS a přihlaste se do aplikace pomocí služby Facebook uživatelů.  Tok ověřování klienta je výhodnější `loginWithProvider:completion:` metody.  Tok ověřování klienta obsahuje více nativní chování činnosti koncového uživatele a umožňuje dalších úprav.

1. Konfigurace back-end vaše mobilní aplikace pro přihlášení k Facebooku pomocí následujících [jak nakonfigurovat aplikaci služby pro přihlášení k Facebooku] [ 9] kurz.

2. Nainstalovat SDK Facebook pro iOS podle [SDK Facebook pro iOS – Začínáme] [ 10] si přečtěte následující dokumentaci. Místo abyste vytvářeli aplikace, můžete přidat platformu iOS do stávajícího zápisu. 

3. Si přečtěte následující dokumentaci na Facebooku obsahuje kód některé cíle-C v aplikaci delegáta. Pokud používáte **Swift**, můžete provádět následující překladů AppDelegate.swift:
  
        // Add the following import to your bridging header:
        //      #import <FBSDKCoreKit/FBSDKCoreKit.h>
        
        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            FBSDKApplicationDelegate.sharedInstance().application(application, didFinishLaunchingWithOptions: launchOptions)
            // Add any custom logic here.
            return true
        }

        func application(application: UIApplication, openURL url: NSURL, sourceApplication: String?, annotation: AnyObject?) -> Bool {
            let handled = FBSDKApplicationDelegate.sharedInstance().application(application, openURL: url, sourceApplication: sourceApplication, annotation: annotation)
            // Add any custom logic here.
            return handled
        }

4. Kromě přidání `FBSDKCoreKit.framework` do projektu, také přidat odkaz na `FBSDKLoginKit.framework` stejným způsobem. 

4. Přidání kódu pro následující aplikace podle jazyk, který používáte. 

**Cíl C**:

    #import <FBSDKLoginKit/FBSDKLoginKit.h>
    #import <FBSDKCoreKit/FBSDKAccessToken.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*)) completionBlock;
    {       
        FBSDKLoginManager *loginManager = [[FBSDKLoginManager alloc] init];
        [loginManager
         logInWithReadPermissions: @[@"public_profile"]
         fromViewController:parent
         handler:^(FBSDKLoginManagerLoginResult *result, NSError *error) {
             if (error) {
                 completionBlock(nil, error);
             } else if (result.isCancelled) {
                 completionBlock(nil, error);
             } else {
                 NSDictionary *payload = @{
                                           @"access_token":result.token.tokenString
                                           };
                 [client loginWithProvider:@"facebook" token:payload completion:completionBlock];
             }
         }];
    }

**Swift**:

    // Add the following imports to your bridging header:
    //      #import <FBSDKLoginKit/FBSDKLoginKit.h>
    //      #import <FBSDKCoreKit/FBSDKAccessToken.h>
    
    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let loginManager = FBSDKLoginManager()
        loginManager.logInWithReadPermissions(["public_profile"], fromViewController: parent) { (result, error) in
            if (error != nil) {
                completion(nil, error)
            }
            else if result.isCancelled {
                completion(nil, error)
            }
            else {
                let payload: [String: String] = ["access_token": result.token.tokenString]
                client.loginWithProvider("facebook", token: payload, completion: completion)
            }
        }
    }

## <a name="twitter-fabric"></a>Postup: ověřování uživatelů s Twitter struktury pro iOS

Použijte struktury pro iOS a přihlaste se do aplikace pomocí Twitter uživatelů. Ověření toku klienta je výhodnější `loginWithProvider:completion:` způsob, jak se obsahuje více nativní chování činnosti koncového uživatele a umožňuje dalších úprav.

1. Konfigurace back-end vaše mobilní aplikace pro Twitter přihlásit pomocí následujících kurz [jak nakonfigurovat aplikaci služby pro Twitter přihlášení](app-service-mobile-how-to-configure-twitter-authentication.md) .

2. Přidání struktury následující dokumentaci [struktury pro iOS – Začínáme] a nastavte si TwitterKit do projektu.

    > [AZURE.NOTE] Ve výchozím nastavení vytvoří struktury aplikace Twitter za vás. Vytvoření aplikace zaregistrováním klíč příjemce a příjemce tajná jste vytvořili dříve pomocí těchto částí kódu se můžete vyhnout.  Můžete taky můžete nahradit klíč příjemce a příjemce tajná hodnot zadaných aplikace služby s hodnotami, které vidíte v [Struktury řídicího panelu]. Pokud vyberete tuto možnost, je potřeba nejdřív nastavit adresu URL zpětné hodnotě zástupný symbol, například `https://<yoursitename>.azurewebsites.net/.auth/login/twitter/callback`.

    Pokud se rozhodnete sdělit nám tajemství, které jste dříve vytvořili, přidání kódu pro následující aplikace delegáta:
    
    **Cíl C**:

        #import <Fabric/Fabric.h>
        #import <TwitterKit/TwitterKit.h>
        // ...
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
            [[Twitter sharedInstance] startWithConsumerKey:@"your_key" consumerSecret:@"your_secret"];
            [Fabric with:@[[Twitter class]]];
            // Add any custom logic here.
            return YES;
        }
        
    **Swift**:
    
        import Fabric
        import TwitterKit
        // ...
        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            Twitter.sharedInstance().startWithConsumerKey("your_key", consumerSecret: "your_secret")
            Fabric.with([Twitter.self])
            // Add any custom logic here.
            return true
        }
    
3. Přidání kódu pro následující aplikace podle jazyk, který používáte. 

**Cíl C**:

    #import <TwitterKit/TwitterKit.h>
    // ...
    - (void)authenticate:(UIViewController*)parent completion:(void (^) (MSUser*, NSError*))completionBlock
    {
        [[Twitter sharedInstance] logInWithCompletion:^(TWTRSession *session, NSError *error) {
            if (session) {
                NSDictionary *payload = @{
                                            @"access_token":session.authToken,
                                            @"access_token_secret":session.authTokenSecret
                                        };
                [client loginWithProvider:@"twitter" token:payload completion:completionBlock];
            } else {
                completionBlock(nil, error);
            }
        }];
    }

**Swift**:

    import TwitterKit
    // ...
    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let client = self.table!.client
        Twitter.sharedInstance().logInWithCompletion { session, error in
            if (session != nil) {
                let payload: [String: String] = ["access_token": session!.authToken, "access_token_secret": session!.authTokenSecret]
                client.loginWithProvider("twitter", token: payload, completion: completion)
            } else {
                completion(nil, error)
            }
        }
    }

## <a name="google-sdk"></a>Postup: ověření uživatelé, kteří mají Google přihlašovací SDK pro iOS

Google přihlašovací SDK pro iOS umožňuje přihlášení uživatele k aplikaci pomocí účtu Google.  Google naposledy ohlásí změny jejich zásady zabezpečení OAuth.  Tyto změny zásad v budoucnu vyžaduje použití Google SDK.

1. Konfigurace back-end vaše mobilní aplikace pro Google přihlásit pomocí následujících kurz [jak nakonfigurovat aplikaci služby Google přihlášení](app-service-mobile-how-to-configure-google-authentication.md) .

2. Podle pokynů [Google pro sprá iOS - zahájení integrace](https://developers.google.com/identity/sign-in/ios/start-integrating) si přečtěte následující dokumentaci nainstalujte SDK Google pro iOS. Můžete vynechat část "Ověřovat pomocí a back-end Server".

3. Přidejte následující text do svého delegáta `signIn:didSignInForUser:withError:` metoda podle jazyka, který používáte.

**Cíl C**:

        NSDictionary *payload = @{
                                  @"id_token":user.authentication.idToken,
                                  @"authorization_code":user.serverAuthCode
                                  };
        
        [client loginWithProvider:@"google" token:payload completion:^(MSUser *user, NSError *error) {
            // ...
        }];

**Swift**:

        let payload: [String: String] = ["id_token": user.authentication.idToken, "authorization_code": user.serverAuthCode]
        client.loginWithProvider("google", token: payload) { (user, error) in
            // ...
        }

4. Ujistěte se, můžete také přidat podle následujících pokynů `application:didFinishLaunchingWithOptions:` v aplikaci delegáta, nahrazení "SERVER_CLIENT_ID" stejné ID, které jste použili nakonfigurovat aplikaci služby v kroku 1.

**Cíl C**:

        [GIDSignIn sharedInstance].serverClientID = @"SERVER_CLIENT_ID";
 
 **Swift**:
 
        GIDSignIn.sharedInstance().serverClientID = "SERVER_CLIENT_ID"

 
 5. Přidání kódu pro následující aplikace v UIViewController implementující `GIDSignInUIDelegate` protokol podle jazyka, který používáte.  Odhlášení před jsou přihlášení znovu a sice nemusíte znovu zadejte svoje přihlašovací údaje, se zobrazí dialogové okno souhlas.  Tuto metodu volat pouze token relace vypršela.
 
 **Cíl C**:

        #import <Google/SignIn.h>
        // ...
        - (void)authenticate
        {
                [GIDSignIn sharedInstance].uiDelegate = self;
                [[GIDSignIn sharedInstance] signOut];
                [[GIDSignIn sharedInstance] signIn];
        }
 
 **Swift**:
    
        // ...
        func authenticate() {
            GIDSignIn.sharedInstance().uiDelegate = self
            GIDSignIn.sharedInstance().signOut()
            GIDSignIn.sharedInstance().signIn()
        }
        
<!-- Anchors. -->

[What is Mobile Services]: #what-is
[Concepts]: #concepts
[Setup and Prerequisites]: #Setup
[How to: Create the Mobile Services client]: #create-client
[How to: Create a table reference]: #table-reference
[How to: Query data from a mobile service]: #querying
[Filter returned data]: #filtering
[Sort returned data]: #sorting
[Return data in pages]: #paging
[Select specific columns]: #selecting
[How to: Bind data to the user interface]: #binding
[How to: Insert data into a mobile service]: #inserting
[How to: Modify data in a mobile service]: #modifying
[How to: Authenticate users]: #authentication
[Cache authentication tokens]: #caching-tokens
[How to: Upload images and large files]: #blobs
[How to: Handle errors]: #errors
[How to: Design unit tests]: #unit-testing
[How to: Customize the client]: #customizing
[Customize request headers]: #custom-headers
[Customize data type serialization]: #custom-serialization
[Next Steps]: #next-steps
[How to: Use MSQuery]: #query-object

<!-- Images. -->

<!-- URLs. -->
[Azure mobilní aplikace rychlým startem]: app-service-mobile-ios-get-started.md

[Add Mobile Services to Existing App]: /develop/mobile/tutorials/get-started-data
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Validate and modify data in Mobile Services by using server scripts]: /develop/mobile/tutorials/validate-modify-and-augment-data-ios
[Mobile Services SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[Authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[iOS SDK]: https://developer.apple.com/xcode

[Handling Expired Tokens]: http://go.microsoft.com/fwlink/p/?LinkId=301955
[Live Connect SDK]: http://go.microsoft.com/fwlink/p/?LinkId=301960
[Permissions]: http://msdn.microsoft.com/library/windowsazure/jj193161.aspx
[Service-side Authorization]: mobile-services-javascript-backend-service-side-authorization.md
[Use scripts to authorize users]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[Dynamické schématu]: http://go.microsoft.com/fwlink/p/?LinkId=296271
[How to: access custom parameters]: /develop/mobile/how-to-guides/work-with-server-scripts#access-headers
[Create a table]: http://msdn.microsoft.com/library/windowsazure/jj193162.aspx
[NSDictionary object]: http://go.microsoft.com/fwlink/p/?LinkId=301965
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[CLI to manage Mobile Services tables]: ../virtual-machines-command-line-tools.md#Mobile_Tables
[Conflict-Handler]: mobile-services-ios-handling-conflicts-offline-data.md#add-conflict-handling

[Řídicí panel struktury]: https://www.fabric.io/home
[Struktury pro iOS: Začínáme]: https://docs.fabric.io/ios/fabric/getting-started.html
[1]: https://github.com/Azure/azure-mobile-apps-ios-client/blob/master/README.md#ios-client-sdk
[2]: http://azure.github.io/azure-mobile-apps-ios-client/
[3]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[4]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags
[5]: http://azure.github.io/azure-mobile-services/iOS/v3/Classes/MSClient.html#//api/name/invokeAPI:data:HTTPMethod:parameters:headers:completion:
[6]: https://github.com/Azure/azure-mobile-services/blob/master/sdk/iOS/src/MSError.h
[7]: app-service-mobile-how-to-configure-active-directory-authentication.md
[8]: ../active-directory/active-directory-devquickstarts-ios.md#em1-determine-what-your-redirect-uri-will-be-for-iosem
[9]: app-service-mobile-how-to-configure-facebook-authentication.md
[10]: https://developers.facebook.com/docs/ios/getting-started
