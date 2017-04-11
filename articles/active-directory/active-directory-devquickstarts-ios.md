<properties
    pageTitle="Azure AD iOS Začínáme | Microsoft Azure"
    description="Vytvoření iOS aplikace, která lze integrovat s Azure AD pro přihlášení a volá Azure AD zamknutí rozhraní API pomocí OAuth."
    services="active-directory"
    documentationCenter="ios"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="integrate-azure-ad-into-an-ios-app"></a>Integrace Azure AD do iOS aplikace

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)] 

Azure AD poskytuje Active Directory Authentication Library nebo ADAL, pro klienty iOS, které se potřebují dostat k chráněné prostředky.  ADAL na výhradní v životnost slouží k Usnadněte aplikace získat tokeny přístupu.  Která ukazuje, jak snadno, tady jsme budete vytvořit seznam úkolů C cíle aplikaci, která:

-   Získá přístup k tokeny pro volání rozhraní API grafu Azure AD pomocí [ověřování protokolem OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).
-   Hledá v adresáři pro uživatele s dané alias.

Vytvoření dokončení aplikace fungovat, musíte:

2. Zaregistrujte aplikace v Azure AD.
3. Instalace a konfigurace ADAL.
5. Pomocí ADAL tokenů z Azure AD.

Jak začít, [Stáhněte si aplikaci osnovu](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) nebo [Stáhnout dokončeným ukázkovým](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).  Taky musíte tenanta Azure AD, ve kterém můžete vytvářet uživatele a registrace aplikace.  Pokud ještě nemáte klienta, [Přečtěte si, jak si ho založit](active-directory-howto-tenant.md).

> [AZURE.TIP] Vyzkoušejte předběžnou verzi našeho nového [portál pro vývojáře](https://identity.microsoft.com/Docs/iOS) , které vám pomůžou do začátků s Azure Active Directory za několik okamžiků!  Portálu pro vývojáře vás provede jednotlivými procesu registrace aplikace a integrace Azure AD do kódu.  Když budete mít hotové, bude mít jednoduchou aplikaci, která může ověřovat uživatele do vašeho tenanta a back-end, které můžete přijmout tokeny a ověření. 

## <a name="1-determine-what-your-redirect-uri-will-be-for-ios"></a>*1. zjistit, co bude vaše přesměrovat URI pro iOS*

Abyste mohli bezpečné spuštění aplikace v některých scénářích jednotné přihlašování budete potřebovat vytvořit **Přesměrování URI** v určitém formátu. Identifikátor URI přesměrovat slouží k zajištění tokeny vrátit správné aplikaci, která požádáni o nich.

Formát iOS URI přesměrovat je:

```
<app-scheme>://<bundle-id>
```

-   **schéma aap** – to je zaregistrovaná v XCode projektu. Je jak jinými aplikacemi můžete volat. Můžete najít to v části Info.plist -> adresa URL typy -> identifikátor URL. Měli byste ho vytvořit Pokud ještě nemáte jeden nebo více nakonfigurované.
-   **příruček id** – to je sada identifikátor "identitou" zrušením zaškrtnutí nastavení projektu v XCode.

Příklad pro tento kód rychlý úvod: ***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***

## <a name="2-register-the-directorysearcher-application"></a>*2. aplikace DirectorySearcher zaregistrovat*
Povolit aplikaci získat tokenů, Nejdřív musíte zaregistrovat v Azure AD klienta a udělit oprávnění pro přístup k rozhraní API Azure AD grafu:

-   Přihlaste se k portálu Správa Azure
-   V levém navigačním podokně klikněte na **Služby Active Directory**
-   Vyberte klienta, ve kterém si zaregistrovat aplikace.
-   Klikněte na kartu **aplikace** a klikněte na **Přidat** do dolní okraj.
-   Postupujte podle pokynů a vytvořte novou **Nativní klientské aplikaci**.
    -   **Název** aplikace bude popisovat aplikace pro koncové uživatele
    -   **Přesměrovat Uri** tvoří kombinace schéma a řetězec využívající Azure AD vrátíte tokenu odpovědi.  Zadejte hodnotu konkrétní aplikaci podle výše uvedených informací.
-   Po dokončení zápisu AAD přiřadí aplikace klienta jedinečný identifikátor.  Musíte mít tuto hodnotu v dalších částech tak zkopírujte ho na kartě **Konfigurovat** .
- Taky na kartě **Konfigurovat** vyhledejte oddíl "Oprávnění do jiných aplikací".  Aplikace "Azure Active Directory" přidáte oprávnění k **adresáři přístup vaše organizace** ve skupinovém rámečku **Delegovat oprávnění**.  To vám umožní aplikace k vytvoření dotazu grafu rozhraní API pro uživatele.

## <a name="3-install--configure-adal"></a>*3. instalace a konfigurace ADAL*
Teď, když máte aplikace v Azure AD, můžete nainstalovat ADAL a zadejte kód týkající se identity.  Aby ADAL mohli komunikovat s Azure AD budete muset poskytnout některé informace o aplikaci registrace.
-   Začněte tím, že přidáte ADAL DirectorySearcher projektu pomocí Cocapods.

```
$ vi Podfile
```
Na tomto podfile přidejte následující text:

```
source 'https://github.com/CocoaPods/Specs.git'
link_with ['QuickStart']
xcodeproj 'QuickStart'

pod 'ADALiOS'
```

Teď načtěte podfile pomocí cocoapods. Tím vytvoříte nový pracovní prostor XCode načtete.

```
$ pod install
...
$ open QuickStart.xcworkspace
```

-   Rychlý úvod projektu, otevřete soubor plist `settings.plist`.  Nahrazení hodnot prvků v části tak, aby odrážely hodnoty, které vkládáte do portálu Azure.  Pokaždé, když se použije ADAL kódu odkazovat tyto hodnoty.
    -   `tenant` Je domain Azure AD klienta, například contoso.onmicrosoft.com
    -   `clientId` Je clientId aplikace, kterou jste zkopírovali z portálu.
    -   `redirectUri` Je přesměrování adresy url registrovaná na portálu.

## <a name="4--use-adal-to-get-tokens-from-aad"></a>*4. pomocí ADAL tokenů z AAD*
Základní zásada za ADAL je, že vždy, když aplikace je přístupový token, jednoduše volá completionBlock `+(void) getToken : `, a ADAL ostatních.  

-   V `QuickStart` otevřeného projekt `GraphAPICaller.m` a vyhledejte `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` komentář v horní.  Toto je místo, kam předáte ADAL souřadnice prostřednictvím CompletionBlock komunikovat s Azure AD a určit, jak uložit do mezipaměti tokeny.

```ObjC
+(void) getToken : (BOOL) clearCache
           parent:(UIViewController*) parent
completionHandler:(void (^) (NSString*, NSError*))completionBlock;
{
    AppData* data = [AppData getInstance];
    if(data.userItem){
        completionBlock(data.userItem.accessToken, nil);
        return;
    }

    ADAuthenticationError *error;
    authContext = [ADAuthenticationContext authenticationContextWithAuthority:data.authority error:&error];
    authContext.parentController = parent;
    NSURL *redirectUri = [[NSURL alloc]initWithString:data.redirectUriString];

    [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
    [authContext acquireTokenWithResource:data.resourceId
                                 clientId:data.clientId
                              redirectUri:redirectUri
                           promptBehavior:AD_PROMPT_AUTO
                                   userId:data.userItem.userInformation.userId
                     extraQueryParameters: @"nux=1" // if this strikes you as strange it was legacy to display the correct mobile UX. You most likely won't need it in your code.
                          completionBlock:^(ADAuthenticationResult *result) {

                              if (result.status != AD_SUCCEEDED)
                              {
                                  completionBlock(nil, result.error);
                              }
                              else
                              {
                                  data.userItem = result.tokenCacheStoreItem;
                                  completionBlock(result.tokenCacheStoreItem.accessToken, nil);
                              }
                          }];
}

```

- Teď je třeba použít tento token hledání uživatelů v grafu. Najděte `// TODO: implement SearchUsersList` zadejte metoda umožňuje požadavek GET rozhraní API Azure AD grafu do dotazu pro uživatele, jehož UPN začíná dané hledaný termín.  Ale k dotazu rozhraní API grafu, potřebujete zahrnout access_token v `Authorization` záhlaví žádosti – to přichází ADAL.

```ObjC
+(void) searchUserList:(NSString*)searchString
                parent:(UIViewController*) parent
       completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    AppData* data = [AppData getInstance];

    NSString *graphURL = [NSString stringWithFormat:@"%@%@/users?api-version=%@&$filter=startswith(userPrincipalName, '%@')", data.taskWebApiUrlString, data.tenant, data.apiversion, searchString];


    [self craftRequest:[self.class trimString:graphURL]
                parent:parent
     completionHandler:^(NSMutableURLRequest *request, NSError *error) {

         if (error != nil)
         {
             completionBlock(nil, error);
         }
         else
         {

             NSOperationQueue *queue = [[NSOperationQueue alloc]init];

             [NSURLConnection sendAsynchronousRequest:request queue:queue completionHandler:^(NSURLResponse *response, NSData *data, NSError *error) {

                 if (error == nil && data != nil){

                     NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:data options:0 error:nil];

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
                         s.name =[keyValuePairs valueForKey:@"givenName"];

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
     }];

}

```
- Pokud aplikace žádosti token tak, že zavoláte `getToken(...)`, ADAL pokusí vrátit token bez výzvou k zadání přihlašovacích údajů.  Pokud ADAL zjistí, že uživatel vyžaduje se přihlásit k získání token, bude zobrazení přihlašovací dialogové okno, shromáždit přihlašovací údaje uživatele a vrátí token po úspěšném ověření.  Pokud ADAL nejde vrátit token z nějakého důvodu, vyvolá `AdalException`.
- Všimněte si, že `AuthenticationResult` objekt obsahuje `tokenCacheStoreItem` objekt, který můžete použít shromažďování informací aplikace může být nutné.  V rychlý úvod `tokenCacheStoreItem` určíte, pokud authenitcation již došlo k.


## <a name="step-5-build-and-run-the-application"></a>Krok 5: Vytvoření a spuštění aplikace



Blahopřejeme! Nyní máte funkční iOS aplikaci, která umožňuje ověřování uživatelů, bezpečně volat rozhraní API webových pomocí OAuth 2.0 a získat základní informace o uživateli.  Pokud jste to ještě neudělali, teď je čas na naplnění vašeho klienta s někteří uživatelé.  Spusťte aplikaci rychlý úvod a přihlášení pomocí jedné z těchto uživatelů.  Hledání podle jejich Přípony ostatním uživatelům.  Ukončete aplikaci a znovu ho spusťte.  Všimněte si, jak relaci, zůstane nedotčený.

ADAL usnadňuje všechny tyto běžné funkce identity začlenit do aplikace.  Zabezpečuje dirty práce za vás – Správa mezipaměti, podpora protokolu OAuth prezentace uživatel s přihlášením uživatelského rozhraní, aktualizace vypršela platnost tokeny a další.  Opravdu potřebujete stačí jedno volání rozhraní API `getToken`.

Pro informaci je k dispozici dokončeným ukázkovým (bez konfigurace hodnot) [v tomto poli](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).  

## <a name="additional-scenarios"></a>Další scénáře
Teď přejdete k další scénáře.  Můžete zkusit:

- [Zabezpečené Node.JS webového rozhraní API s Azure AD](active-directory-devquickstarts-webapi-nodejs.md)
- Zjistěte, [jak povolit SSO křížově aplikace v iOS pomocí ADAL](active-directory-sso-ios.md)  

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
