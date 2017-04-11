<properties
    pageTitle="IOS verze 2.0 Azure AD aplikace | Microsoft Azure"
    description="Vytvoření aplikace pro iOS, který se přihlašuje uživatelé, kteří mají obou osobní účet Microsoft a pracovní nebo školní účty pomocí knihoven třetích stran."
    services="active-directory"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/28/2016"
    ms.author="brandwe"/>

# <a name="add-sign-in-to-an-ios-app-using-a-third-party-library-with-graph-api-using-the-v20-endpoint"></a>Přidání přihlášení do aplikace pro iOS knihovně třetích stran pomocí rozhraní API grafu pomocí koncový bod verze 2.0

Platformu Microsoft identity používá otevřít standardy například OAuth2 a OpenID připojení. Vývojáři můžete použít knihovnu, které chtějí integrace s služeb společnosti Microsoft. Aby vývojáři naše platformu pomocí jiných knihoven, jsme jste napsali několik návody zpráva a ukazuje, jak konfigurovat knihovny jiných výrobců pro připojení k platformu Microsoft identity. Většina knihoven, které implementace [specifikace RFC6749 OAuth2](https://tools.ietf.org/html/rfc6749) můžete připojit k platformu Microsoft identity.

Nechejte aplikaci, která vytvoří tohoto návodu a uživatelé Přihlaste se k jejich organizace a vyhledejte jinými uživateli z jejich organizace pomocí rozhraní API grafu.

Pokud jste ještě moc nedělali a OAuth2 OpenID připojení podobně tuto konfiguraci ukázkové nemusí můžete uspořádat. Doporučujeme číst [protokoly verze 2.0 – OAuth 2.0 se tak mohli ověřovat kód obrázku](active-directory-v2-protocols-oauth-code.md) pozadí.


> [AZURE.NOTE]
    Některé funkce naše platformy, které mají výraz standardy OAuth2 nebo OpenID připojit třeba podmíněného přístupu a Správa zásad Intune vyžadují, abyste použít naše otevřít zdroj Microsoft Azure Identity knihoven.

Koncový bod verze 2.0 nepodporuje všechny scénáře Azure Active Directory a funkce.

> [AZURE.NOTE]
    Pokud chcete zjistit, zda by měly používat koncový bod verze 2.0, přečtěte si informace o [omezení verze 2.0](active-directory-v2-limitations.md).

## <a name="download-code-from-github"></a>Stahování GitHub kód
Kód pro účely tohoto návodu spravován [na GitHub](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-v2).  Postup, můžete [Stáhnout osnovu v aplikaci jako ZIP](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) nebo klonovat kostra:

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

Můžete taky jen stáhnout vzorku a rovnou začít:

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

## <a name="register-an-app"></a>Registrace aplikace
Vytvoření nové aplikace na [portál registrace aplikace](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)nebo postupujte podle podrobných pokynů v tématu [jak si zaregistrovat aplikace s koncovým verze 2.0](active-directory-v2-app-registration.md).  Zkontrolujte, že:

- Zkopírujte **Id aplikace** přiřazená aplikace vzhledem k tomu, že ho musíte brzy bude k dispozici.
- Přidání platformu **mobilní** aplikace.
- Zkopírujte **Přesměrovat URI** z portálu. Musíte použít výchozí hodnoty `urn:ietf:wg:oauth:2.0:oob`.


## <a name="download-the-third-party-nxoauth2-library-and-create-a-workspace"></a>Stáhnout knihovnu NXOAuth2 třetích stran a vytvoření pracovního prostoru aplikace Groove

V tomto návodu použijete OAuth2Client z GitHub, což je knihovny OAuth2 pro Mac OS X a iOS (kakao a kakaové dotykového ovládání). Tuto knihovnu vychází z koncept 10 specifikace OAuth2. Implementuje nativní aplikaci profilu a podporuje koncový bod se tak mohli ověřovat uživatele. Toto jsou na všechno, co budete potřebovat integrovat s platformu Microsoft identity.

### <a name="add-the-library-to-your-project-by-using-cocoapods"></a>Přidání knihovny do projektu pomocí CocoaPods

CocoaPods je závislost vedoucí projektů Xcode. Spravuje předchozích kroků instalace automaticky.

```
$ vi Podfile
```
1. Na tomto podfile přidejte následující text:

    ```
     platform :ios, '8.0'

     target 'QuickStart' do

     pod 'NXOAuth2Client'

     end
    ```

2. Načtení podfile pomocí CocoaPods. Tím vytvoříte nový pracovní prostor Xcode, která načtete.

    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

## <a name="explore-the-structure-of-the-project"></a>Procházení struktury projektu

Následující strukturu nastavenou naše projektu v kostra:

- Zobrazení předlohy UPN vyhledávání
- Zobrazení podrobných dat o vybranému uživateli
- Zobrazení přihlášení kde uživatele se přihlásit k aplikaci k vytvoření dotazu v grafu

Jsme přesune do různých souborů v kostra přidáte ověřování. Další díly kód, například kód vizuální netýkají identity ale k dispozici máte.

## <a name="set-up-the-settingsplst-file-in-the-library"></a>Nastavení settings.plst souboru v knihovně

-   Otevřete v projektu rychlý úvod `settings.plist` soubor. Nahrazení hodnot prvky v části tak, aby odrážely hodnoty, které jste použili v portálu Azure. Pokaždé, když používala Active Directory Authentication Library kódu odkazovat tyto hodnoty.
    -   `clientId` Je ID klienta aplikace, kterou jste zkopírovali z portálu.
    -   `redirectUri` Je přesměrování adresy URL, kterých byly čerpány na portálu.

## <a name="set-up-the-nxoauth2client-library-in-your-loginviewcontroller"></a>Nastavení knihovny NXOAuth2Client ve vaší LoginViewController

Knihovna NXOAuth2Client vyžaduje některé hodnoty nastavit. Po dokončení tohoto úkolu, můžete pomocí získaných token zavolat rozhraní API grafu. Protože `LoginView` bude s názvem kdykoli potřebujeme k ověření, dává smysl vložit hodnoty konfigurace v na tento soubor.

- Přidání některé hodnoty, které mají `LoginViewController.m` soubor, který chcete nastavit kontext pro a tak mohli ověřovat. Podrobnosti o hodnoty podle kód.

    ```objc
    NSString *scopes = @"openid offline_access User.Read";
    NSString *authURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/authorize";
    NSString *loginURL = @"https://login.microsoftonline.com/common/login";
    NSString *bhh = @"urn:ietf:wg:oauth:2.0:oob?code=";
    NSString *tokenURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/token";
    NSString *keychain = @"com.microsoft.azureactivedirectory.samples.graph.QuickStart";
    static NSString * const kIDMOAuth2SuccessPagePrefix = @"session_state=";
    NSURL *myRequestedUrl;
    NSURL *myLoadedUrl;
    bool loginFlow = FALSE;
    bool isRequestBusy;
    NSURL *authcode;
    ```

Podívejme se na podrobné informace o kódu.

První řetězec je určený pro `scopes`.  `User.Read` Hodnotu umožňuje číst základní profilu přihlášený uživatel.

Další informace o všechny obory k dispozici v [aplikaci Microsoft Graph oprávnění obory](https://graph.microsoft.io/docs/authorization/permission_scopes).

Pro `authURL`, `loginURL`, `bhh`, a `tokenURL`, byste měli použít hodnoty zadané dříve. Pokud používáte otevřít zdroj Microsoft Azure Identity knihoven, jsme rozbalovací tato data můžete pomocí naše koncový bod metadata. Můžeme udělat práci o extrahování tyto hodnoty za vás.

`keychain` Hodnotu kontejneru knihovnu NXOAuth2Client bude použita pro vytvoření řetězce klíčů pro uložení vaší tokeny. Pokud chcete získat jednotné přihlašování (SSO) u spoustu aplikací, můžete zadat stejné řetězce klíčů ve všech vašich aplikací a požádat o použití tohoto řetězce klíčů v Xcode nároky. To je vysvětleno v dokumentaci Apple.

Zbytek tyto hodnoty jsou potřeba pro knihovnu a vytvoření místa, kde můžete provádět hodnot a ty pak kontextu.

### <a name="create-a-url-cache"></a>Vytvoření mezipaměť adresy URL

Uvnitř `(void)viewDidLoad()`, který je vždy jen po načtení zobrazení, tento kód primes mezipaměti pro použití.

Přidejte následující kód:

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    self.loginView.delegate = self;
    [self setupOAuth2AccountStore];
    [self requestOAuth2Access];
    NSURLCache *URLCache = [[NSURLCache alloc] initWithMemoryCapacity:4 * 1024 * 1024
                                                         diskCapacity:20 * 1024 * 1024
                                                             diskPath:nil];
    [NSURLCache setSharedURLCache:URLCache];

}
```

### <a name="create-a-webview-for-sign-in"></a>Vytvoření webové zobrazení pro přihlášení

Webové zobrazení můžete zobrazí výzvu další faktorů, jako jsou textová zpráva (Pokud je nakonfigurované) nebo vrátí chybové zprávy uživateli. Tady vytvoříte nahoru webové zobrazení a potom později kód pro zpracování zpětná, jejichž ve webovém zobrazení ze stránky služby na identity.

```objc
-(void)requestOAuth2Access {
    //to sign in to Microsoft APIs using OAuth2, we must show an embedded browser (UIWebView)
    [[NXOAuth2AccountStore sharedStore] requestAccessToAccountWithType:@"myGraphService"
                                   withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
                                       //navigate to the URL returned by NXOAuth2Client

                                       NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
                                       [self.loginView loadRequest:r];
                                   }];
}
```

### <a name="override-the-webview-methods-to-handle-authentication"></a>Přepsat webové zobrazení metod pro zpracování ověřování

Co se stane, když uživatel musí přihlásit jak bylo popsáno dříve zjistit webové zobrazení, můžete vložit následující kód.

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {

    // We get the auth token from a redirect so we need to handle that in the webview.

    if (![NSThread isMainThread]) {
        [self performSelectorOnMainThread:@selector(resolveUsingUIWebView:) withObject:URL waitUntilDone:YES];
        return;
    }

    NSURLRequest *hostnameURLRequest = [NSURLRequest requestWithURL:URL cachePolicy:NSURLRequestUseProtocolCachePolicy timeoutInterval:10.0f];
    isRequestBusy = YES;
    [self.loginView loadRequest:hostnameURLRequest];

    NSLog(@"resolveUsingUIWebView ready (status: UNKNOWN, URL: %@)", self.loginView.request.URL);
}

- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType {

    NSLog(@"webView:shouldStartLoadWithRequest: %@ (%li)", request.URL, (long)navigationType);

    // The webview is where all the communication happens. Slightly complicated.

    myLoadedUrl = [webView.request mainDocumentURL];
    NSLog(@"***Loaded url: %@", myLoadedUrl);

    //if the UIWebView is showing our authorization URL or consent URL, show the UIWebView control
    if ([request.URL.absoluteString rangeOfString:authURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:loginURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide the UIWebView, we've left the authorization flow
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:bhh options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide the UIWebView, we've left the authorization flow
        self.loginView.hidden = YES;
        [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }
    else {
        self.loginView.hidden = NO;
        //read the Location from the UIWebView, this is how Microsoft APIs is returning the
        //authentication code and relation information. This is controlled by the redirect URL we chose to use from Microsoft APIs
        //continue the OAuth2 flow
       // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }

    return YES;

}
```

### <a name="write-code-to-handle-the-result-of-the-oauth2-request"></a>Napište kód pro zpracování výsledek OAuth2 žádosti

Následující kód zpracuje redirectURL vracející z webové zobrazení. Pokud ověřování nebyl úspěšný, kód bude opakujte. Mezitím knihovnu poskytne chybami, které můžete zobrazit v konzole nebo asynchronní zpracovat.

```objc
- (void)handleOAuth2AccessResult:(NSString *)accessResult {

    AppData* data = [AppData getInstance];

    //parse the response for success or failure
     if (accessResult)
    //if success, complete the OAuth2 flow by handling the redirect URL and obtaining a token
     {
         [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
    } else {
        //start over
        [self requestOAuth2Access];
    }
}
```

### <a name="set-up-the-oauth-context-called-account-store"></a>Nastavení kontextu OAuth (označované jako úložiště účtů)

Tady můžete volat `-[NXOAuth2AccountStore setClientID:secret:authorizationURL:tokenURL:redirectURL:forAccountType:]` v úložišti sdíleného účtu pro každou službu, kterou chcete mít přístup k aplikaci. Typ účtu je řetězec, který slouží jako identifikátor určité služby. Protože jsou přístup k rozhraní API graf, kód odkazuje ho ve formátu `"myGraphService"`. Potom nastavíte pozorovatel se dozvíte, když změní jakékoli okolnosti tokenu. Po získání tokenu návratu uživatel zpět `masterView`.



```objc
- (void)setupOAuth2AccountStore {


        AppData* data = [AppData getInstance];

    [[NXOAuth2AccountStore sharedStore] setClientID:data.clientId
                                             secret:data.secret
                                              scope:[NSSet setWithObject:scopes]
                                   authorizationURL:[NSURL URLWithString:authURL]
                                           tokenURL:[NSURL URLWithString:tokenURL]
                                        redirectURL:[NSURL URLWithString:data.redirectUriString]
                                      keyChainGroup: keychain
                                     forAccountType:@"myGraphService"];

    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreAccountsDidChangeNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      if (aNotification.userInfo) {
                                                          //account added, we have access
                                                          //we can now request protected data
                                                          NSLog(@"Success!! We have an access token.");
                                                          dispatch_async(dispatch_get_main_queue(),^ {

                                                              MasterViewController* masterViewController = [self.storyboard instantiateViewControllerWithIdentifier:@"masterView"];
                                                              [self.navigationController pushViewController:masterViewController animated:YES];
                                                          });
                                                      } else {
                                                          //account removed, we lost access
                                                      }
                                                  }];

    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreDidFailToRequestAccessNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      NSError *error = [aNotification.userInfo objectForKey:NXOAuth2AccountStoreErrorKey];
                                                      NSLog(@"Error!! %@", error.localizedDescription);
                                                  }];
}
```

## <a name="set-up-the-master-view-to-search-and-display-the-users-from-the-graph-api"></a>Nastavení zobrazení předlohy na hledání a zobrazení uživatelů z rozhraní API grafu

Hlavní zobrazení řadiči (MVC) aplikace, která se zobrazí vrácená data v tabulce je nad rámec tohoto návodu a mnoho kurzy online je vysvětleno, jak můžete vytvořit jednu. Tento kód je v poli kostry soubor. Přesto musíte zacházet s několik věcí v této aplikaci MVC:

* Průsečík s osou y, pokud uživatel zadá něco do vyhledávacího pole
* Poskytovat objektu data zpět MasterView tak jej zobrazte výsledky v mřížce

Můžeme udělat můžou být dole.

### <a name="add-a-check-to-see-if-youre-logged-in"></a>Zaškrtněte políčko Zobrazit, pokud jste přihlášení

Aplikace se malou Pokud není přihlášení uživatele tak, aby byl inteligentní, zkontrolujte, jestli se už token v mezipaměti. V opačném případě přesměrování na LoginView pro uživatele se přihlásit. Pokud můžete odvolat, nejlepší způsob, jak při načtení zobrazení proveďte akce, je použít `viewDidLoad()` metoda poskytující Apple us.

```objc
- (void)viewDidLoad {
    [super viewDidLoad];


    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];

        if (accounts.count == 0) {

        dispatch_async(dispatch_get_main_queue(),^ {

            LoginViewController* userSelectController = [self.storyboard instantiateViewControllerWithIdentifier:@"LoginUserView"];
            [self.navigationController pushViewController:userSelectController animated:YES];
        });
        }
```

### <a name="update-the-table-view-when-data-is-received"></a>Po přijetí dat aktualizuje zobrazení tabulky

Když rozhraní API graf vrátí data, je třeba jej zobrazit data. Pro zjednodušení tady je kód aktualizovat tabulku. Ve vašem MVC často používaný kódu můžete vložit jenom nesprávné hodnoty.

```objc
#pragma mark - Table View

- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView {
    return 1;
}

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {

        return [upnArray count];
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {

    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"TaskPrototypeCell" forIndexPath:indexPath];

    if ( cell == nil ) {
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:@"TaskPrototypeCell"];
    }

    User *user = nil;
     user = [upnArray objectAtIndex:indexPath.row];


    // Configure the cell
    cell.textLabel.text = user.name;
    [cell setAccessoryType:UITableViewCellAccessoryDisclosureIndicator];

    return cell;
}

```

### <a name="provide-a-way-to-call-the-graph-api-when-someone-types-in-the-search-field"></a>Možnost, jak volat rozhraní API grafu, když zadají do vyhledávacího pole

Pokud uživatel zadá do pole vyhledávání, budete muset shove, rozhraní API grafu. `GraphAPICaller` Předmětu, které vytvoříte v následující kód, odděluje vyhledávací funkce v prezentaci. Prozatím Pojďme zadat kód, který kanály všechny znaky vyhledávání k rozhraní API grafu. Bychom zadáním metodu nazvanou `lookupInGraph`, která trvá řetězec, který chcete vyhledat.

```objc

-(void)lookupInGraph:(NSString *)searchText {
if (searchText.length > 0) {

    };



        [GraphAPICaller searchUserList:searchText completionBlock:^(NSMutableArray* returnedUpns, NSError* error) {
            if (returnedUpns) {


                upnArray = returnedUpns;


            }
            else
            {
                UIAlertView *alertView = [[UIAlertView alloc]initWithTitle:nil message:[[NSString alloc]initWithFormat:@"Error : %@", error.localizedDescription] delegate:nil cancelButtonTitle:@"Retry" otherButtonTitles:@"Cancel", nil];

                [alertView setDelegate:self];

                dispatch_async(dispatch_get_main_queue(),^ {
                    [alertView show];
                });
            }


        }];


}
```

## <a name="write-a-helper-class-to-access-the-graph-api"></a>Psaní třída Pomocníka pro přístup k rozhraní API grafu

Toto je základní naše aplikace. Zatímco ostatní byl vložení kódu v výchozí vzorek MVC od společnosti Apple, tady napíšete kód do dotazu v grafu se při psaní a vraťte se tato data. Tady je kód a podrobné vysvětlení následuje.

### <a name="create-a-new-objective-c-header-file"></a>Vytvoření nového souboru záhlaví cíle C

Zadejte název souboru `GraphAPICaller.h`a přidejte následující kód.

```objc
@interface GraphAPICaller : NSObject<NSURLConnectionDataDelegate>

+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray*, NSError* error))completionBlock;

@end
```

Tady vidíte, že zadanou metodu přijímá řetězec a vrátí completionBlock. Tento completionBlock při se může mít uhodnout, aktualizuje tabulce zadáním objekt s vyplněné data v reálném čase jako hledání uživatele.


### <a name="create-a-new-objective-c-file"></a>Vytvoření nového souboru cíle C

Zadejte název souboru `GraphAPICaller.m`a přidejte následujícího postupu.

```objc
+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    AppData* data = [AppData getInstance];

    NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];

    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSDictionary* params = [self convertParamsToDictionary:searchString];

    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process the response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                           // Don't be thrown off by the key name being "value". It really is the name of the
                           // first node. :-)

                           //each object is a key value pair
                           NSDictionary *keyValuePairs;
                           NSMutableArray* Users = [[NSMutableArray alloc]init];

                           for(int i =0; i < graphDataArray.count; i++)
                           {
                               keyValuePairs = [graphDataArray objectAtIndex:i];

                               User *s = [[User alloc]init];
                               s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];


                               [Users addObject:s];
                           }

                           completionBlock(Users, nil);
                       }
                       else
                       {
                           completionBlock(nil, error);
                       }

                   }];
}

```

Pojďme si projít tato metoda podrobně.

Základní tento kód je v `NXOAuth2Request`, metodu, která používá parametry, který jste už nadefinovali v souboru settings.plist.

Cílem prvního kroku je vytvořit správné volání rozhraní API grafu. Protože voláte `/users`, určíte, že přidáním k rozhraní API grafu zdroje společně s verzi. Má smysl vložit soubor nastavení externího protože tyto můžete měnit podle vývoj rozhraní API.


```objc
NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];
```

Pak budete muset zadat parametrů, které se také poskytovat volání rozhraní API grafu. Je *velmi důležité* nevkládejte parametry v koncový bod zdroje, protože, které odstranil pro všechny znaky vyhovující – identifikátor URI za běhu. Všechny kód dotazu musí podle parametry.

```objc

NSDictionary* params = [self convertParamsToDictionary:searchString];
```

Můžete narazit na volá `convertParamsToDictionary` metodu, která nebyly dosud napsané. Pojďme se nyní na konci souboru:

```objc
+(NSDictionary*) convertParamsToDictionary:(NSString*)searchString
{
    NSMutableDictionary* dictionary = [[NSMutableDictionary alloc]init];

        NSString *query = [NSString stringWithFormat:@"startswith(givenName, '%@')", searchString];

           [dictionary setValue:query forKey:@"$filter"];



    return dictionary;
}

```
Pak použijeme `NXOAuth2Request` metoda vrátit data z rozhraní API ve formátu JSON.

```objc
NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process the response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];
```

Nakonec Podívejme se na tom, jak můžete vrátit data MasterViewController. Data vrátí jako serializované a musí být rekonstruován a načíst v objekt, který můžete používat MainViewController. K tomuto účelu kostra má `User.m/h` soubor, který vytvoří objektu uživatele. Vyplnění tento uživatel objekt s informacemi z grafu.

```objc
                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                           // Don't be thrown off by the key name being "value". It really is the name of the
                           // first node. :-)

                           //each object is a key value pair
                           NSDictionary *keyValuePairs;
                           NSMutableArray* Users = [[NSMutableArray alloc]init];

                           for(int i =0; i < graphDataArray.count; i++)
                           {
                               keyValuePairs = [graphDataArray objectAtIndex:i];

                               User *s = [[User alloc]init];
                               s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];


                               [Users addObject:s];
```


## <a name="run-the-sample"></a>Spuštění výběru

Pokud jste použili kostra nebo sledované spolu s návodu by měla běžet teď aplikace. Spusťte simulator a klikněte na **přihlásit** k aplikaci.

## <a name="get-security-updates-for-our-product"></a>Aktualizace zabezpečení naše produktu

Budeme rádi, když chcete dostávat oznámení ze kdy zabezpečení incidentem probíhala po návštěvě [Web TechCenter o zabezpečení](https://technet.microsoft.com/security/dd252948) a přihlášení k odběru poradní výstrah zabezpečení.
