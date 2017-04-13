<properties 
    pageTitle="Android webové zobrazení mostu s nativní Mobile zapojení Android SDK" 
    description="Popisuje, jak vytvořit most mezi službou Javascript a nativní SDK zapojení Android mobilní webové zobrazení"      
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-android" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="bridge-android-webview-with-native-mobile-engagement-android-sdk"></a>Android webové zobrazení mostu s nativní Mobile zapojení Android SDK

> [AZURE.SELECTOR]
- [Android mostu](mobile-engagement-bridge-webview-native-android.md)
- [iOS mostu](mobile-engagement-bridge-webview-native-ios.md)

Některé mobilní aplikace jsou určeny jako hybridní aplikace kdy samotnou aplikaci vyvíjí prostřednictvím rozvoje nativní Android, ale některé nebo všechny obrazovky se vykreslují v rámci Android webové zobrazení. Mobilní zapojení Android SDK můžete pořád používat tyto aplikace a tohoto kurzu se dozvíte, jak chcete přejít o tom, jak to. Kód vzorku je založena na Android dokumentace [tady](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript). Popisuje, jak lze použít tento dokumentovaného přístup pro běžně používaných metod Mobile zapojení Android SDK na stejné provádět tak, aby webové zobrazení pomocí funkčně hybridní aplikace můžete také zahájení žádosti o sledovat události, úkoly, chyby, informace o aplikaci při potrubí prostřednictvím naše Android SDK. 

1. Nejprve je třeba zajistit, aby šlo prostřednictvím naše [Začínáme kurz](mobile-engagement-android-get-started.md) integrovat Android SDK zapojení mobilní aplikaci hybridní. Až to uděláte, vaše `OnCreate` metoda bude vypadat takto.  
    
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("<Mobile Engagement Conn String>");
            EngagementAgent.getInstance(this).init(engagementConfiguration);
        }

2. Teď zajistěte hybridní aplikace na obrazovce webové zobrazení na něj. Kód ho bude vypadat podobně jako následující kde jsme načítání místní HTML soubor **Sample.html** ve webovém zobrazení v `onCreate` metody obrazovky. 

        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
        }

        protected void onCreate(Bundle savedInstanceState) {
            ...
            ...
            SetWebView();
        }

3. Nyní vytvořte mostu soubor s názvem **WebAppInterface** , která vytvoří Obálka přes některé běžně používaných metod Mobile zapojení Android SDK pomocí `@JavascriptInterface` podle [Android si přečtěte následující dokumentaci](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript):

        import android.content.Context;
        import android.os.Bundle;
        import android.util.Log;
        import android.webkit.JavascriptInterface;
        
        import com.microsoft.azure.engagement.EngagementAgent;
        
        import org.json.JSONArray;
        import org.json.JSONObject;
        
        public class WebAppInterface {
            Context mContext;
        
            /** Instantiate the interface and set the context */
            WebAppInterface(Context c) {
                mContext = c;
            }
        
            @JavascriptInterface
            public void sendEngagementEvent(String name, String extras ){
                EngagementAgent.getInstance(mContext).sendEvent(name, ParseExtras(extras));
            }
        
            @JavascriptInterface
            public void startEngagementJob(String name, String extras ){
                EngagementAgent.getInstance(mContext).startJob(name, ParseExtras(extras));
            }
        
            @JavascriptInterface
            public void endEngagementJob(String name){
                EngagementAgent.getInstance(mContext).endJob(name);
            }
        
            @JavascriptInterface
            public void sendEngagementError(String name, String extras ){
                EngagementAgent.getInstance(mContext).sendError(name, ParseExtras(extras));
            }
        
            @JavascriptInterface
            public void sendEngagementAppInfo(String appInfo){
                EngagementAgent.getInstance(mContext).sendAppInfo(ParseExtras(appInfo));
            }
        
            public Bundle ParseExtras(String input) {
                Bundle extras = new Bundle();
        
                try {
                    JSONObject jObject = new JSONObject(input);
                    extras.putString(jObject.names().getString(0),
                            jObject.get(jObject.names().getString(0)).toString());
                } catch (Exception e) {
                    e.printStackTrace();
                }
                return extras;
            }
        }  

4. Jakmile jsme vytvořili výše soubor mostu, potřebujeme a ujistěte se, že je přidružená k naše webové zobrazení. Tento postup stát, budete muset upravit svůj `SetWebview` metody, že bude vypadat takto:

        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
            WebSettings webSettings = myWebView.getSettings();
            webSettings.setJavaScriptEnabled(true);
            myWebView.addJavascriptInterface(new WebAppInterface(this), "EngagementJs");
        }

5. Ve výše uvedený fragment kódu, doporučujeme s názvem `addJavascriptInterface` naše mostu třídy přidružit naše webové zobrazení a taky vytvořit ty s názvem **EngagementJs** volat metody ze souboru mostu. 

6. Nyní vytvořte následující soubor s názvem **Sample.html** v projektu ve složce s názvem **prostředky** načtení do webového zobrazení a odkud metody bude volat ze souboru mostu.

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
                      log('window.onerror: ' + err);
                    }
        
                    send = function(inputId)
                    {
                        var input = document.getElementById(inputId);
                        if(input)
                        {
                            var value = input.value;
                            // Example of how extras info can be passed with the Engagement logs
                            var extras = '{"CustomerId":"MS290011"}';
        
                            if(value && value.length > 0)
                            {
                                switch(inputId)
                                {
                                    case "event":
                                    EngagementJs.sendEngagementEvent(value, extras);
                                    break;
        
                                    case "job":
                                    EngagementJs.startEngagementJob(value, extras);
                                    window.setTimeout( function()
                                    {
                                      EngagementJs.endEngagementJob(value);
                                    }, 10000 );
                                    break;
        
                                    case "error":
                                    EngagementJs.sendEngagementError(value, extras);
                                    break;
        
                                    case "appInfo":
                                    EngagementJs.sendEngagementAppInfo({"customer_name":value});
                                    break;
                                }
                            }
                        }
                    }
                </script>
            </head>
            <body>
                <h1>Bridge Tester</h1>
                <div id='engagement'>
                    <h2>Event</h2>
                    <input type="text" id="event" size="35">
                    <button onclick="send('event')">Send</button>
        
                    <h2>Job</h2>
                    <input type="text" id="job" size="35">
                    <button onclick="send('job')">Send</button>
        
                    <h2>Error</h2>
                    <input type="text" id="error" size="35">
                    <button onclick="send('error')">Send</button>
        
                    <h2>AppInfo</h2>
                    <input type="text" id="appInfo" size="35">
                    <button onclick="send('appInfo')">Send</button>
                </div>
            </body>
        </html>

8. Mějte na paměti následující body o souboru ve formátu HTML:

    -   Obsahuje vstupních polí, kde je možné poskytnout data, která chcete použít jako názvy pro události, úlohy, chyba, AppInfo. Po kliknutí na tlačítko vedle této položky při volání Javascript, které se volá metody ze souboru konferenčního mostu pro předávání volání Android SDK zapojení Mobile. 
    -   Jsme označování na některé statické dodatečné informace o události, projekty a dokonce chyby ukazují, jak to lze provést. Tyto další informace o odeslaný jako JSON řetězec, pokud oblast hledání `WebAppInterface` souboru, je analyzovat a v Androidu `Bundle` a předaný spolu s odesílání chyby události, projekty. 
    -   Zapojení úlohy Mobile je vykazuje postavu s názvem zadejte do pole vstupní spustit 10 sekund a vypnout. 
    -   Zapojení Mobile appinfo nebo značky předán s "jméno_zákazníka" jako statické klíč a hodnota, která jste zadali ve vstupním jako hodnota značku. 
 
9. Spusťte aplikaci a zobrazí se následující. Teď zadejte některé název událost nastavit jako test například následující a pod komentářem klikněte na **Odeslat** . 

    ![][1]

10. Teď když přejdete na kartě **Monitor** aplikace a vzhled v části **události -> Podrobnosti**, zobrazí se tato událost neprojevila spolu s statické app – informace, jsme odesíláte. 

    ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-android/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-android/event-output.png
