<properties
    pageTitle="Azure Active Directory B2C: Zavolejte webového rozhraní API z aplikace iOS použití knihoven třetích stran | Microsoft Azure"
    description="Tento článek vám ukáže, jak k vytvoření aplikace pro iOS úkolů seznam, která volá Node.js webového rozhraní API pomocí OAuth 2.0 nosný tokenů knihovně třetí strany."
    services="active-directory-b2c"
    documentationCenter="ios"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

< značky ms.workload="identity ms.service="active directory b2c"" ms.tgt_pltfrm="na" ms.devlang="objectivec" ms.topic= "obrázek – článek"

    ms.date="07/26/2016"
    ms.author="brandwe"/>

# <a name="azure-ad-b2c--call-a-web-api-from-an-ios-application-using-a-third-party-library"></a>Azure AD B2C: Zavolejte webového rozhraní API z aplikace iOS přes knihovně třetí strany.

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Platformu Microsoft identity používá otevřít standardy například OAuth2 a OpenID připojení. Vývojáři můžete využít knihovny, které chtějí integrace s služeb společnosti Microsoft. Na podporu vývojáři v naší platformy pomocí jiných knihoven jsme jste napsali několik návody zpráva k demonstate jak nakonfigurovat třetích stran knihoven pro připojení k platformu Microsoft identity. Většina knihoven, které implementace [specifikace RFC6749 OAuth2](https://tools.ietf.org/html/rfc6749) bude moct připojit k platformu Microsoft Identity.


Pokud jste ještě moc nedělali a OAuth2 OpenID připojení hodně této ukázkové konfigurace nemusí mnohem můžete uspořádat. Doporučujeme, aby že se podíváte na stručný [Přehled protokol, který jsme jste uvedených v tomto poli](active-directory-b2c-reference-protocols.md).

> [AZURE.NOTE]
    Některé funkce naše platformy, které mají výrazu v těchto organizace pro standardy, například Správa zásad podmíněného přístupu a Intune, vyžadují, abyste použít naše otevřít zdroj Microsoft Azure Identity knihoven. 
   
Některé scénáře Azure Active Directory a funkce jsou podporovány platformu B2C.  Pokud byste měli použít platformu B2C nejste jistí, přečtěte si o [B2C omezení](active-directory-b2c-limitations.md).


## <a name="get-an-azure-ad-b2c-directory"></a>Získat Azure AD B2C adresář

Než budete moct použít Azure AD B2C, musíte vytvořit adresář nebo klient. Adresář je kontejner pro všechny uživatele aplikace, skupiny a další. Pokud nemáte už, [vytvořte adresář B2C](active-directory-b2c-get-started.md) před pokračovat.

## <a name="create-an-application"></a>Vytvoření aplikace

Dál je potřeba vytvořit aplikace ve vašem adresáři B2C. To vám Azure AD informace, které potřebuje bezpečně komunikovat s aplikací. Aplikace a webového rozhraní API představují jednoho **ID aplikace** v tomto případě protože představují jedné logické aplikaci. Vytvoření aplikace pro, postupujte podle [těchto pokynů](active-directory-b2c-app-registration.md). Nezapomeňte:

- Patří prostřednictvím **mobilního zařízení** v aplikaci.
- Zkopírujte **ID aplikace** přiřazená aplikace. Budete taky potřebovat to později.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Vytvoření pravidla

V Azure AD B2C každé uživatelské prostředí je definován [zásad](active-directory-b2c-reference-policies.md). Tato aplikace obsahuje jeden identity prostředí: kombinované sign in a registrace. Je potřeba vytvořit tuto zásadu každého typu způsobem popsaným v [zásad referenční článek](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Pokud budete vytvářet zásady, nezapomeňte:

- Zvolte v svoje zásady **zobrazované jméno** a registrace atributy.
- V každé zásady zvolte deklarace aplikace **zobrazované jméno** a **ID objektu** . Můžete použít i jiné deklarované.
- Zkopírujte **název** každého zásadu po jeho vytvoření. By měla být předponu `b2c_1_`.  Název zásady budete potřebovat později.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Po vytvoření pravidla, budete chtít vytvořit aplikace.


## <a name="download-the-code"></a>Stáhněte si tento kód

Kód pro účely tohoto návodu spravován [na GitHub](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-b2c).  Sledování, můžete [si stáhnout aplikaci jako ZIP](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-b2c)/archive/master.zip) nebo ho zkopírovat:

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-b2c.git
```

Jenom stáhnout Dokončený kód nebo rovnou začít: 

```
git clone --branch complete git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-b2c.git
```

## <a name="download-the-third-party-library-nxoauth2-and-launch-a-workspace"></a>Stažení nxoauth2 knihovny třetích stran a spuštění pracovního prostoru aplikace Groove

V tomto návodu použijeme OAuth2Client z GitHub, OAuth2 knihovny pro systém Mac OS X a iOS (kakao a kakao dotykového ovládání). Tuto knihovnu vychází z koncept 10 specifikace OAuth2. Implementuje nativní aplikaci profilu a podporuje koncový bod se tak mohli ověřovat koncových uživatelů. Toto jsou všechny věci, které budeme potřebovat za účelem integrat s platformu Microsoft identity.

### <a name="adding-the-library-to-your-project-using-cocoapods"></a>Přidání knihovny do projektu přes CocoaPods

CocoaPods je závislost vedoucí projektů Xcode. Spravuje výše uvedené kroky instalace automaticky.

```
$ vi Podfile
```
Na tomto podfile přidejte následující text:

```
 platform :ios, '8.0'
 
 target 'SampleforB2C' do
 
 pod 'NXOAuth2Client'
 
 end
```

Teď načtěte podfile pomocí cocoapods. Tím vytvoříte nový pracovní prostor XCode načtete.

```
$ pod install
...
$ open SampleforB2C.xcworkspace

```

## <a name="the-structure-of-the-project"></a>Struktury projektu

Máme následující strukturu, nastavení pro naše projektu v kostra:

* **Zobrazení předlohy** pomocí podokna úloh
* **Přidání úkolů** pro data o vybraného úkolu
* **Přihlášení zobrazení** , které umožňují uživateli přihlásit k aplikaci.

Jsme přeskočí v různých souborů do projektu přidat ověřování. Jiná část kódu například kód vizuální není germane identity a k dispozici máte.

## <a name="create-the-settingsplist-file-for-your-application"></a>Vytvoření `settings.plist` souboru aplikace

Je jednodušší konfigurace aplikace Pokud máme centralizované umístění pro naše hodnot konfigurace. Je taky vám pomůže porozumět tomu, jaký dopad jednotlivá nastavení v aplikaci. V *Seznamu vlastností* jsme budou využívat jako způsob, jak poskytnout tyto hodnoty aplikace.

* Vytvořit nebo otevřít `settings.plist` souborů v části `Supporting Files` v pracovním prostoru aplikace

* Zadejte následující hodnoty (budeme věnovat jejich procházení podrobně brzy bude k dispozici)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>accountIdentifier</key>
    <string>B2C_Acccount</string>
    <key>clientID</key>
    <string><client ID></string>
    <key>clientSecret</key>
    <string></string>
    <key>authURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/oauth2/v2.0/authorize?p=<policy name></string>
    <key>loginURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/login</string>
    <key>bhh</key>
    <string>urn:ietf:wg:oauth:2.0:oob</string>
    <key>tokenURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/oauth2/v2.0/token?p=<policy name></string>
    <key>keychain</key>
    <string>com.microsoft.azureactivedirectory.samples.graph.QuickStart</string>
    <key>contentType</key>
    <string>application/x-www-form-urlencoded</string>
    <key>taskAPI</key>
    <string>https://aadb2cplayground.azurewebsites.net</string>
</dict>
</plist>
```

Pojďme si v těchto podrobně.


Pro `authURL`, `loginURL`, `bhh`, `tokenURL` zjistíte, co potřebujete k vyplnění pod vaším jménem klienta. To je název klienta vašeho B2C klienta, který vám byl přiřazen. Například `kidventusb2c.onmicrosoft.com`. Pokud používáte naše otevřít zdroj Microsoft Azure Identity knihoven jsme by rozbalovací tato data pomocí naše koncový bod metadata. Můžeme udělat práci o extrahování tyto hodnoty za vás.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

`keychain` Hodnotu kontejneru knihovnu NXOAuth2Client bude použita pro vytvoření řetězce klíčů pro uložení vaší tokeny. Pokud chcete získat jednotné přihlašování v aplikacích pro mnoho je můžete zadat stejné řetězce klíčů ve všech aplikacích i požádat o použití tohoto řetězce klíčů v XCode entitements. Toto jsou obsaženy v dokumentaci Apple.

`<policy name>` Na konci každé adresy URL jsou místa, kde místo, kam chcete umístit zásady vytvořené nad. Aplikace vyzve tyto zásady v závislosti na tok.

`taskAPI` Koncový bod ZBÝVAJÍCÍ jsme vyzvedne s tokenu B2C buď přidávají nebo existující úkoly dotazu. To je nastavené pro tento příklad. Není potřeba změnit vzorku pracovat.

Zbytek tyto hodnoty jsou potřeba pro knihovnu a jednoduše vytvořit místa, kde můžete provádět hodnot a ty pak kontextu.

Teď, když máme `settings.plist` soubor vytvořený, potřebujeme kód číst.

## <a name="set-up-a-appdata-class-to-read-our-settings"></a>Nastavení třída AppData číst naše nastavení

Pojďme vytvořit jednoduchý soubor, který právě analyzuje naše `settngs.plist` soubor jsme vytvořili výše a tak, abyste tyto nastavení avaialble v budoucnu libovolné třídy. Protože jsme nechcete vytvářet novou kopii data pokaždé, když blok předmětu se zeptá ho, bude použít vzorek jednoznačné jsme pouze vrátí stejné instanci každé nově vytvořené žádosti o nastavení

* Vytvoření `AppData.h` souboru:

```objc
#import <Foundation/Foundation.h>

@interface AppData : NSObject

@property(strong) NSString *accountIdentifier;
@property(strong) NSString *taskApiString;
@property(strong) NSString *authURL;
@property(strong) NSString *clientID;
@property(strong) NSString *loginURL;
@property(strong) NSString *bhh;
@property(strong) NSString *keychain;
@property(strong) NSString *tokenURL;
@property(strong) NSString *clientSecret;
@property(strong) NSString *contentType;

+ (id)getInstance;

@end
```

* Vytvoření `AppData.m` souboru:

```objc
#import "AppData.h"

@implementation AppData

+ (id)getInstance {
  static AppData *instance = nil;
  static dispatch_once_t onceToken;

  dispatch_once(&onceToken, ^{
    instance = [[self alloc] init];

    NSDictionary *dictionary = [NSDictionary
        dictionaryWithContentsOfFile:[[NSBundle mainBundle]
                                         pathForResource:@"settings"
                                                  ofType:@"plist"]];
    instance.accountIdentifier = [dictionary objectForKey:@"accountIdentifier"];
    instance.clientID = [dictionary objectForKey:@"clientID"];
    instance.clientSecret = [dictionary objectForKey:@"clientSecret"];
    instance.authURL = [dictionary objectForKey:@"authURL"];
    instance.loginURL = [dictionary objectForKey:@"loginURL"];
    instance.bhh = [dictionary objectForKey:@"bhh"];
    instance.tokenURL = [dictionary objectForKey:@"tokenURL"];
    instance.keychain = [dictionary objectForKey:@"keychain"];
    instance.contentType = [dictionary objectForKey:@"contentType"];
    instance.taskApiString = [dictionary objectForKey:@"taskAPI"];

  });

  return instance;
}
@end
```

Teď můžeme snadno dostali na naše data zavoláním na oddělení jednoduše `  AppData *data = [AppData getInstance];` v některém z našich třídy při pod objeví.



## <a name="set-up-the-nxoauth2client-library-in-your-appdelegate"></a>Nastavení knihovny NXOAuth2Client ve vaší AppDelegate

Knihovna NXOAuthClient vyžaduje některé hodnoty nastavit. Jednou tedy dokončení můžete token, který je aquired volání rozhraní REST API. Protože jsme vědět, že `AppDelegate` volaná kdykoli jsme načíst aplikaci dává smysl jsme umístění naše konfigurace hodnot v na tento soubor.
* Otevřít `AppDelegate.m` souboru

* Importujte nepotřebných souborů záhlaví použijeme později.

```objc
#import "NXOAuth2.h" // the Identity library we are using
#import "AppData.h" // the class we just created we will use to load the settings of our application
```

* Přidejte `setupOAuth2AccountStore` metoda v AppDelegate

Potřebujeme vytvářet AccountStore a založte ji jsme právě četl v z dat `settings.plist` soubor.

Existují některé věci, které byste měli znát z týkající se B2C služby v tomto okamžiku, které bude přesně tento kód srozumitelnost:


1. Azure AD B2C pomocí *zásad* podle parametry dotazu žádosti o služby. Díky Azure Active Directory má fungovat jako nezávislé služby jenom pro aplikace. Abyste mohli poskytnout tyto parametry musíme poskytovat extra dotazu `kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters:` metodu s naše vlastní zásady parametry. 

2. Azure AD B2C používá obory velmi stejně jako další OAuth2 servery. Ale protože použití B2C tolik o ověřování uživatele jako přístupu k prostředkům některé obory jsou opravdu nutná tok fungovat správně. Toto je `openid` obor. Náš identit SDK automaticky poskytují `openid` obor za vás, neuvidíte, který v našem SDK konfiguraci. Protože používáme knihovně třetích stran, ale jsme musí zadat Tento obor.

```objc
- (void)setupOAuth2AccountStore {
  AppData *data = [AppData getInstance]; // The singleton we use to get the settings

  NSDictionary *customHeaders =
      [NSDictionary dictionaryWithObject:@"application/x-www-form-urlencoded"
                                  forKey:@"Content-Type"];

  // Azure B2C needs
  // kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters for
  // sending policy to the server,
  // therefore we use -setConfiguration:forAccountType:
  NSDictionary *B2cConfigDict = @{
    kNXOAuth2AccountStoreConfigurationClientID : data.clientID,
    kNXOAuth2AccountStoreConfigurationSecret : data.clientSecret,
    kNXOAuth2AccountStoreConfigurationScope :
        [NSSet setWithObjects:@"openid", data.clientID, nil],
    kNXOAuth2AccountStoreConfigurationAuthorizeURL :
        [NSURL URLWithString:data.authURL],
    kNXOAuth2AccountStoreConfigurationTokenURL :
        [NSURL URLWithString:data.tokenURL],
    kNXOAuth2AccountStoreConfigurationRedirectURL :
        [NSURL URLWithString:data.bhh],
    kNXOAuth2AccountStoreConfigurationCustomHeaderFields : customHeaders,
    //      kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters:customAuthenticationParameters
  };

  [[NXOAuth2AccountStore sharedStore] setConfiguration:B2cConfigDict
                                        forAccountType:data.accountIdentifier];
}
```
Potom se ujistěte, zavolejte do AppDelegate v části `didFinishLaunchingWithOptions:` metody. 

```
[self setupOAuth2AccountStore];
```


## <a name="create-a-loginviewcontroller-class-that-we-will-use-to-handle-authentication-requests"></a>Vytvoření `LoginViewController` třídy, která použijeme zpracovávat požadavky ověřování

Webové zobrazení budeme používat pro přihlášení k účtu. Díky tomu cz zobrazí výzvu další faktorů, jako jsou textová zpráva (Pokud je nakonfigurované) nebo chybové zprávy vrátit uživateli. Zde se nastavení nahoru webové zobrazení a potom později kód pro zpracování zpětná, jejichž ve webovém zobrazení ze služby Microsoft Identity.

* Vytvoření `LoginViewController.h` třídy

```objc
@interface LoginViewController : UIViewController <UIWebViewDelegate>
@property(weak, nonatomic) IBOutlet UIWebView *loginView; // Our webview that we will use to do authentication

- (void)handleOAuth2AccessResult:(NSURL *)accessResult; // Allows us to get a token after we've received an Access code.
- (void)setupOAuth2AccountStore; // We will need to add to our OAuth2AccountStore we setup in our AppDelegate
- (void)requestOAuth2Access; // This is where we invoke our webview.
```

Vytvoříme každá z těchto metod dole.

> [AZURE.NOTE] 
    Ujistěte se, že svážete `loginView` k skutečné webové zobrazení, které je uvnitř vaší scénáře. V opačném případě nebudete mít webové zobrazení, který můžete objevovat po je potřeba ověřit.

* Vytvoření `LoginViewController.m` třídy

* Přidání některé proměnné provést státu, jako jsme ověřování

```objc
NSURL *myRequestedUrl; \\ The URL request to Azure Active Directory 
NSURL *myLoadedUrl; \\ The URL loaded for Azure Active Directory
bool loginFlow = FALSE; 
bool isRequestBusy; \\ A way to give status to the thread that the request is still happening
NSURL *authcode; \\ A placeholder for our auth code.
```

* Přepsat webové zobrazení metod pro zpracování ověřování

Potřeba dát webové zobrazení chování, které chceme, když uživatel musí přihlásit se, jak je popsáno výše. Můžete jednoduše vyjmout a vložit následující kód.

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {
  // We get the auth token from a redirect so we need to handle that in the
  // webview.

  if (![NSThread isMainThread]) {
    [self performSelectorOnMainThread:@selector(resolveUsingUIWebView:)
                           withObject:URL
                        waitUntilDone:YES];
    return;
  }

  NSURLRequest *hostnameURLRequest =
      [NSURLRequest requestWithURL:URL
                       cachePolicy:NSURLRequestUseProtocolCachePolicy
                   timeoutInterval:10.0f];
  isRequestBusy = YES;
  [self.loginView loadRequest:hostnameURLRequest];

  NSLog(@"resolveUsingUIWebView ready (status: UNKNOWN, URL: %@)",
        self.loginView.request.URL);
}

- (BOOL)webView:(UIWebView *)webView
    shouldStartLoadWithRequest:(NSURLRequest *)request
                navigationType:(UIWebViewNavigationType)navigationType {
  AppData *data = [AppData getInstance];

  NSLog(@"webView:shouldStartLoadWithRequest: %@ (%li)", request.URL,
        (long)navigationType);

  // The webview is where all the communication happens. Slightly complicated.

  myLoadedUrl = [webView.request mainDocumentURL];
  NSLog(@"***Loaded url: %@", myLoadedUrl);

  // if the UIWebView is showing our authorization URL or consent URL, show the
  // UIWebView control
  if ([request.URL.absoluteString rangeOfString:data.authURL
                                        options:NSCaseInsensitiveSearch]
          .location != NSNotFound) {
    self.loginView.hidden = NO;
  } else if ([request.URL.absoluteString rangeOfString:data.loginURL
                                               options:NSCaseInsensitiveSearch]
                 .location != NSNotFound) {
    // otherwise hide the UIWebView, we've left the authorization flow
    self.loginView.hidden = NO;
  } else if ([request.URL.absoluteString rangeOfString:data.bhh
                                               options:NSCaseInsensitiveSearch]
                 .location != NSNotFound) {
    // otherwise hide the UIWebView, we've left the authorization flow
    self.loginView.hidden = YES;
    [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
  } else {
    self.loginView.hidden = NO;
    // read the Location from the UIWebView, this is how Microsoft APIs is
    // returning the
    // authentication code and relation information. This is controlled by the
    // redirect URL we chose to use from Microsoft APIs
    // continue the OAuth2 flow
    // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
  }

  return YES;
}

```

* Napište kód pro zpracování výsledek OAuth2 žádosti

Budeme potřebovat kód, který bude řešit redirectURL, které vracejí ze webové zobrazení. Pokud nebyl úspěšný, pokusíme znovu. Mezitím knihovnu poskytne chyby, můžete zobrazit v konzole nebo zpracovat asyncronously. 

```objc
- (void)handleOAuth2AccessResult:(NSURL *)accessResult {
  // parse the response for success or failure
  if (accessResult)
  // if success, complete the OAuth2 flow by handling the redirect URL and
  // obtaining a token
  {
    [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
  } else {
    // start over
    [self requestOAuth2Access];
  }
}
```

* Nastavte si továrny oznámení.

Vytvoříme stejným způsobem kroků uvedených v `AppDelegate` výše, ale tentokrát přidáme pár `NSNotification`s Řekněte nám, co je nového v naší služby. Nastavení pozorovatel se nám při změní jakékoli okolnosti tokenu. Jakmile jsme získání tokenu vrácených uživatel zpět `masterView`.



```objc
- (void)setupOAuth2AccountStore {
  [[NSNotificationCenter defaultCenter]
      addObserverForName:NXOAuth2AccountStoreAccountsDidChangeNotification
                  object:[NXOAuth2AccountStore sharedStore]
                   queue:nil
              usingBlock:^(NSNotification *aNotification) {
                if (aNotification.userInfo) {
                  // account added, we have access
                  // we can now request protected data
                  NSLog(@"Success!! We have an access token.");
                  dispatch_async(dispatch_get_main_queue(), ^{

                    MasterViewController *masterViewController =
                        [self.storyboard
                            instantiateViewControllerWithIdentifier:@"master"];
                    [self.navigationController
                        pushViewController:masterViewController
                                  animated:YES];
                  });
                } else {
                  // account removed, we lost access
                }
              }];

  [[NSNotificationCenter defaultCenter]
      addObserverForName:NXOAuth2AccountStoreDidFailToRequestAccessNotification
                  object:[NXOAuth2AccountStore sharedStore]
                   queue:nil
              usingBlock:^(NSNotification *aNotification) {
                NSError *error = [aNotification.userInfo
                    objectForKey:NXOAuth2AccountStoreErrorKey];
                NSLog(@"Error!! %@", error.localizedDescription);
              }];
}

```
* Přidat kód, který zpracovává uživatel pokaždé, když je žádost o zahájená pro znaménko nativní

Vytvoření metodu, která se nazývá pokaždé, když máme žádost o ověření. Bude si metodu, která skutečně vytvoří webovém zobrazení

```objc
- (void)requestOAuth2Access {
  AppData *data = [AppData getInstance];

  // in order to login to Mircosoft APIs using OAuth2 we must show an embedded
  // browser (UIWebView)
  [[NXOAuth2AccountStore sharedStore]
           requestAccessToAccountWithType:data.accountIdentifier
      withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
        // navigate to the URL returned by NXOAuth2Client

        NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
        [self.loginView loadRequest:r];
      }];
}
```

* Nakonec Pojďme zavolejte těchto postupů jsme jste napsali nad pokaždé, když `LoginViewController` načte. Doporučujeme provést přidáním těchto postupů k naše `viewDidLoad` metoda Apple dostaneme

```objc
  [super viewDidLoad];
  // Do any additional setup after loading the view.

  // OAuth2 Code

  self.loginView.delegate = self;
  [self requestOAuth2Access];
  [self setupOAuth2AccountStore];
  NSURLCache *URLCache =
      [[NSURLCache alloc] initWithMemoryCapacity:4 * 1024 * 1024
                                    diskCapacity:20 * 1024 * 1024
                                        diskPath:nil];
  [NSURLCache setSharedURLCache:URLCache];
```

Teď dokončíte vytvořit hlavní způsob, jak jsme budete pracovat s naše žádosti o přihlášení. Po jsme jste přihlášení, potřebujeme budete naše tokenů, kterou jsme dostali. K těmto vytvoříme některé helper kód, který bude volat rozhraní REST API pro nás pomocí této knihovny.


## <a name="create-a-graphapicaller-class-to-handle-our-requests-to-a-rest-api"></a>Vytvoření `GraphAPICaller` třídy pro zpracování naše požadavky na rozhraní REST API

Máme konfigurace načíst pokaždé, když nám načíst naše aplikace. Teď potřebujeme pracovat s ním po máme token. 

* Vytvoření `GraphAPICaller.h` souboru

```objc
@interface GraphAPICaller : NSObject <NSURLConnectionDataDelegate>

+ (void)addTask:(Task *)task
completionBlock:(void (^)(bool, NSError *error))completionBlock;

+ (void)getTaskList:(void (^)(NSMutableArray *, NSError *error))completionBlock;

@end
```

Od vidí tento kód, jsme bude vytvářet dvěma způsoby: jedna z rozhraní API a jiné, jak přidat úkoly k rozhraní API získat úkoly.

Teď, jsme jste nastavili naše rozhraní, přidejte skutečného provedení:

* Vytvoření`GraphAPICaller.m file`

```objc
@implementation GraphAPICaller

// 
// Gets the tasks from our REST endpoint we specified in settings
//

+ (void)getTaskList:(void (^)(NSMutableArray *, NSError *))completionBlock

{
  AppData *data = [AppData getInstance];

  NSString *taskURL =
      [NSString stringWithFormat:@"%@%@", data.taskApiString, @"/api/tasks"];

  NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
  NSMutableArray *Tasks = [[NSMutableArray alloc] init];

  NSArray *accounts = [store accountsWithAccountType:data.accountIdentifier];
  [NXOAuth2Request performMethod:@"GET"
      onResource:[NSURL URLWithString:taskURL]
      usingParameters:nil
      withAccount:accounts[0]
      sendProgressHandler:^(unsigned long long bytesSend,
                            unsigned long long bytesTotal) {
        // e.g., update a progress indicator
      }
      responseHandler:^(NSURLResponse *response, NSData *responseData,
                        NSError *error) {
        // Process the response
        if (!error) {
          NSDictionary *dataReturned =
              [NSJSONSerialization JSONObjectWithData:responseData
                                              options:0
                                                error:nil];
          NSLog(@"Graph Response was: %@", dataReturned);

          if ([dataReturned count] != 0) {

            for (NSMutableDictionary *theTask in dataReturned) {

              Task *t = [[Task alloc] init];
              t.name = [theTask valueForKey:@"Text"];

              [Tasks addObject:t];
            }
          }

          completionBlock(Tasks, nil);
        } else {
          completionBlock(nil, error);
        }

      }];
}

// 
// Adds a task from our REST endpoint we specified in settings
//

+ (void)addTask:(Task *)task
completionBlock:(void (^)(bool, NSError *error))completionBlock {

  AppData *data = [AppData getInstance];

  NSString *taskURL =
      [NSString stringWithFormat:@"%@%@", data.taskApiString, @"/api/tasks"];

  NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
  NSDictionary *params = [self convertParamsToDictionary:task.name];

  NSArray *accounts = [store accountsWithAccountType:data.accountIdentifier];
  [NXOAuth2Request performMethod:@"POST"
      onResource:[NSURL URLWithString:taskURL]
      usingParameters:params
      withAccount:accounts[0]
      sendProgressHandler:^(unsigned long long bytesSend,
                            unsigned long long bytesTotal) {
        // e.g., update a progress indicator
      }
      responseHandler:^(NSURLResponse *response, NSData *responseData,
                        NSError *error) {
        // Process the response
        if (responseData) {
          NSDictionary *dataReturned =
              [NSJSONSerialization JSONObjectWithData:responseData
                                              options:0
                                                error:nil];
          NSLog(@"Graph Response was: %@", dataReturned);

          completionBlock(TRUE, nil);
        } else {
          completionBlock(FALSE, error);
        }

      }];
}

+ (NSDictionary *)convertParamsToDictionary:(NSString *)task {
  NSMutableDictionary *dictionary = [[NSMutableDictionary alloc] init];

  [dictionary setValue:task forKey:@"Text"];

  return dictionary;
}

@end
```

## <a name="run-the-sample-app"></a>Spusťte aplikaci ukázka

Nakonec vytvoření a spuštění aplikace v Xcode. Registraci nebo přihlášení k aplikaci a vytváření úkolů pro přihlášený uživatel. Odhlásit a znovu se přihlaste jako jiný uživatel a vytvoření úkolů pro daného uživatele.

Všimněte si, že úkoly uložené uživatelská na rozhraní API, protože rozhraní API extrahuje identita uživatele z přístupový token, který obdrží.


## <a name="next-steps"></a>Další kroky

Teď můžete přesunout do pokročilejší B2C témata. Můžete zkusit:

[Zavolat Node.js webového rozhraní API z web appu Node.js]()

[Přizpůsobení činnosti koncového uživatele aplikace B2C]()
