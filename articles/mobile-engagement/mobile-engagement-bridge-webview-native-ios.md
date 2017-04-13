<properties 
    pageTitle="Most iOS webové zobrazení nativního Mobile zapojení iOS SDK" 
    description="Popisuje, jak vytvořit most mezi službou Javascript a nativní iOS Mobile zapojení SDK webové zobrazení"      
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-ios" 
    ms.devlang="objective-c" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="bridge-ios-webview-with-native-mobile-engagement-ios-sdk"></a>Most iOS webové zobrazení nativního Mobile zapojení iOS SDK

> [AZURE.SELECTOR]
- [Android mostu](mobile-engagement-bridge-webview-native-android.md)
- [iOS mostu](mobile-engagement-bridge-webview-native-ios.md)

Některé mobilní aplikace jsou určeny jako hybridní aplikace kdy samotnou aplikaci vyvíjí prostřednictvím rozvoje nativní iOS cíle-C, ale některé nebo všechny obrazovky se vykreslují v iOS webové zobrazení. Můžete pořád používat iOS Mobile zapojení SDK tyto aplikace a tohoto kurzu se dozvíte, jak chcete přejít o tom, jak to. 

Když jsou obě nezdokumentovaný dosáhnout dvěma způsoby:

- Nejdřív jednu popsaného na tento [odkaz](http://stackoverflow.com/questions/9826792/how-to-invoke-objective-c-method-from-javascript-and-send-back-data-to-javascrip) , který zahrnuje registraci `UIWebViewDelegate` na zobrazení webové stránky a skutečnou a okamžitě zrušení Změna umístění udělat i v JavaScriptu. 
- Druhé jednu vychází z této [relace WWDC 2013](https://developer.apple.com/videos/play/wwdc2013/615), přístupu k tedy opět dostanete čistší než první a které jsme podle tohoto průvodce. Všimněte si, že tento postup lze použít pouze na iOS7, výše. 

Pro iOS most vzorku, postupujte následujícím způsobem:

1. Nejprve je třeba zajistit, aby šlo prostřednictvím naše [Začínáme kurz](mobile-engagement-ios-get-started.md) integrovat Mobile zapojení iOS SDK v aplikaci hybridní. V případě potřeby můžete taky povolíte test protokolování takto tak, aby metody SDK můžete vidět, jak jsme aktivace z webové zobrazení. 
    
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
           ....
            [EngagementAgent setTestLogEnabled:YES];
           ....
        }

2. Teď zajistěte hybridní aplikace na obrazovce webové zobrazení na něj. Můžete ho chcete přidat `Main.storyboard` aplikace. 

3. Přidružit k této webové zobrazení **ViewController** kliknutím a přetažením webové zobrazení z scény řadiče zobrazení na `ViewController.h` upravit obrazovky, umístěte ji přímo pod `@interface` řádku. 

4. Až to uděláte, objevovat se dialogové okno s žádostí o název. Zadejte název jako **webové zobrazení**. Vaše `ViewController.h` soubor by měl vypadat takto:

        #import <UIKit/UIKit.h>
        #import "EngagementViewController.h"
        
        @interface ViewController : EngagementViewController
        @property (strong, nonatomic) IBOutlet UIWebView *webView;
        
        @end

5. Budeme aktualizovat `ViewController.m` soubor později, ale nejdřív vytvoříme mostu soubor, který vytvoří Obálka přes některé často používané iOS Mobile zapojení SDK metody. Vytvoření nového souboru záhlaví s názvem **EngagementJsExports.h** , který využívá `JSExport` mechanismus podle výše uvedených [relace](https://developer.apple.com/videos/play/wwdc2013/615) vystavit metody nativní iOS. 

        #import <Foundation/Foundation.h>
        #import <JavaScriptCore/JavascriptCore.h>
        
        @protocol EngagementJsExports <JSExport>
        
        + (void) sendEngagementEvent:(NSString*) name :(NSString*)extras;
        + (void) startEngagementJob:(NSString*) name :(NSString*)extras;
        + (void) endEngagementJob:(NSString*) name;
        + (void) sendEngagementError:(NSString*) name :(NSString*)extras;
        + (void) sendEngagementAppInfo:(NSString*) appInfo;
        
        @end

        @interface EngagementJs : NSObject <EngagementJsExports>

        @end

6. Další vytvoříme druhé části mostu soubor. Vytvoření souboru s názvem **EngagementJsExports.m** , který bude obsahovat implementaci vytváření skutečné obálky tak, že zavoláte metody Mobile zapojení iOS SDK. Všimněte si také, jsme analýza `extras` předávány z webového zobrazení javascript a umístění, do `NSMutableDictionary` objekt předávat s volání metody zapojení SDK.  

        #import <UIKit/UIKit.h>
        #import "EngagementAgent.h"
        #import "EngagementJsExports.h"
        
        @implementation EngagementJs
        
        +(void) sendEngagementEvent:(NSString*)name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] sendEvent:name extras:extrasInput];
        }
        
        + (void) startEngagementJob:(NSString*) name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] startJob:name extras:extrasInput];
        }
        
        + (void) endEngagementJob:(NSString*) name {
           [[EngagementAgent shared] endJob:name];
        }
        
        + (void) sendEngagementError:(NSString*) name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] sendError:name extras:extrasInput];
        }
        
        + (void) sendEngagementAppInfo:(NSString*) appInfo {
           NSMutableDictionary* appInfoInput = [self ParseExtras:appInfo];
           [[EngagementAgent shared] sendAppInfo:appInfoInput];
        }
        
        + (NSMutableDictionary*) ParseExtras:(NSString*) input {
           NSData *data = [input dataUsingEncoding:NSUTF8StringEncoding];
           NSError* error = nil;
           NSMutableDictionary* extras = [NSJSONSerialization JSONObjectWithData:data options:0 error:&error];
           
           return extras;
        }
        
        @end

5. Teď vraťte **ViewController.m** a aktualizovat následující kód: 
        
        #import <JavaScriptCore/JavaScriptCore.h>
        #import "ViewController.h"
        #import "EngagementJsExports.h"
        
        @interface ViewController ()
        
        @end
        
        @implementation ViewController
        
        - (void)viewDidLoad
        {
           self.webView.delegate = self;
           [super viewDidLoad];
           [self loadWebView];
        }
        
        - (void)loadWebView {
           NSBundle* mainBundle = [NSBundle mainBundle];
           NSURL* htmlPage = [mainBundle URLForResource:@"LocalPage" withExtension:@"html"];
           
           NSURLRequest* urlReq = [NSURLRequest requestWithURL:htmlPage];
           [self.webView loadRequest:urlReq];
        }
        
        - (void)webViewDidFinishLoad:(UIWebView*)wv
        {
           JSContext* context = [wv valueForKeyPath:@"documentView.webView.mainFrame.javaScriptContext"];
           
           context[@"EngagementJs"] = [EngagementJs class];
        }
        
        - (void)webView:(UIWebView*)wv didFailLoadWithError:(NSError*)error
        {
           NSLog(@"Error for WEBVIEW: %@", [error description]);
        }
        
        - (void)didReceiveMemoryWarning {
           [super didReceiveMemoryWarning];
           // Dispose of any resources that can be recreated.
        }
        
        @end

6. Mějte na paměti následující body o **ViewController.m** souboru:

    - V `loadWebView` metody, jsme načítání místní soubor HTML s názvem **LocalPage.html** jehož kód zvážíme možnost Další. 
    - V `webViewDidFinishLoad` metody, jsme přetahování `JsContext` a přidružení naše třídy obálky s ním. To vám umožní volání naše obálky SDK metod pomocí úchytu **EngagementJs** z webové zobrazení. 

7. Vytvoření souboru s názvem **LocalPage.html** se tento kód:

        <!doctype html>
        <html>
           <head>
               <style type='text/css'>
                   html { font-family:Helvetica; color:#222; }
                   h1 { color:steelblue; font-size:22px; margin-top:16px; }
                   h2 { color:grey; font-size:14px; margin-top:18px; margin-bottom:0px; }
               </style>
               
               <script type="text/javascript">
               
               window.onerror = function(err)
               {
                   alert('window.onerror: ' + err);
               }
               
               function send(inputId)
               {
                   var input = document.getElementById(inputId);
                   if(input)
                   {
                       var value = input.value;
                       // Example of how extras info can be passed with the Engagement logs
                       var extras = '{"CustomerId":"MS290011"}';
                   }
                   
                   if(value && value.length > 0)
                   {
                       switch(inputId)
                       {
                           case "event":
                           EngagementJs.sendEngagementEvent(value, extras);
                           break;
                           
                           case "job":
                           EngagementJs.startEngagementJob(value, extras);
                           window.setTimeout(
                           function(){
                               EngagementJs.endEngagementJob(value);
                           }, 10000 );
                           break;
        
                           case "error":
                           EngagementJs.sendEngagementError(value, extras);
                           break;
                           
                           case "appInfo":
                           var appInfo = '{"customer_name":"' + value + '"}';
                           EngagementJs.sendEngagementAppInfo(appInfo);
                           break;
                       }
                   }
               }
               </script>
               
           </head>
           <body>
               <h1>Bridge Tester</h1>
               
               <div id='engagement'>
                   
                   <br/>
                   <h2>Event</h2>
                   <input type="text" id="event" size="35">
                   <button onclick="send('event')">Send</button>
                   
                   <br/>
                   <h2>Job</h2>
                   <input type="text" id="job" size="35">
                   <button onclick="send('job')">Send</button>
                   
                   <br/>
                   <h2>Error</h2>
                   <input type="text" id="error" size="35">
                   <button onclick="send('error')">Send</button
                   
                   <br/>
                   <h2>AppInfo</h2>
                   <input type="text" id="appInfo" size="35">
                   <button onclick="send('appInfo')">Send</button>
               
               </div>
           </body>
        </html>

8. Mějte na paměti následující body o souboru ve formátu HTML:

    -   Obsahuje vstupních polí, kde je možné poskytnout data, která chcete použít jako názvy pro události, úlohy, chyba, AppInfo. Po kliknutí na tlačítko vedle této položky při volání Javascript, které se volá metody ze souboru konferenčního mostu pro předávání tento hovor na mobilní zapojení iOS SDK. 
    -   Jsme označování na některé statické dodatečné informace o události, projekty a dokonce chyby ukazují, jak to lze provést. Tyto další informace o odeslaný jako JSON řetězec, který, pokud oblast hledání `EngagementJsExports.m` souboru, je analyzovat a spolu s odesílání chyby události, projekty. 
    -   Zapojení úlohy Mobile je vykazuje postavu s názvem zadejte do pole vstupní spustit 10 sekund a vypnout. 
    -   Zapojení Mobile appinfo nebo značky předán s "jméno_zákazníka" jako statické klíč a hodnota, která jste zadali ve vstupním jako hodnota značku. 
 
9. Spusťte aplikaci a zobrazí se následující. Teď zadejte některé název událost nastavit jako test například následující a klikněte na **Odeslat** vedle ní. 

    ![][1]

10. Teď když přejdete na kartě **Monitor** aplikace a vzhled v části **události -> Podrobnosti**, zobrazí se tato událost neprojevila spolu s statické app – informace, jsme odesíláte. 

    ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-ios/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-ios/event-output.png
