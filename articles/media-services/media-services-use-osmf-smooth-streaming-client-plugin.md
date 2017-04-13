<properties 
    pageTitle="Hladký streamování modul plug-in pro Framework médií otevřít zdroj" 
    description="Informace o používání modulu plug-in Azure Media Services vyhlazenými streamování pro rozhraní Adobe otevřít zdroj médií." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="juliako"/>


# <a name="how-to-use-the-microsoft-smooth-streaming-plugin-for-the-adobe-open-source-media-framework"></a>Jak používat Microsoft hladký přenos modul plug-in pro rozhraní Adobe otevřít zdroj média

##<a name="overview"></a>Základní informace


Modul plug-in Microsoft hladký přenos pro otevřít zdroj médií Framework 2.0 (SS pro OSMF) rozšiřuje možnosti výchozí OSMF a přitom zajistit hladký přenos Microsoft přehrávání obsahu se přidá novým i dosavadním OSMF přehrávače. Modul plug-in taky přidá hladký přenos možností přehrávání pro zábleskové médií přehrávání SMP ().

SS OSMF obsahuje dvě verze modulu plug-in:

- Statická hladký přenos modul plug-in pro OSMF (.swc)

- Dynamické hladký přenos modul plug-in pro OSMF (.swf)

Tento dokument předpokládá, že čtenáře má obecné praxi OSMF a OSMF moduly plug-in. Další informace o OSMF najdete v dokumentaci na [úředního OSMF webu](http://osmf.org/).

###<a name="smooth-streaming-plugin-for-osmf-20"></a>Hladký datových proudů modul plug-in pro OSMF 2.0

Modul plug-in podporuje načítání a přehrávání obsahu na vyžádání hladký přenos pomocí následujících funkcí:

- Na vyžádání hladký přenos přehrávání (přehrát, pozastavit, hledání, zastavit)
- Přehrávání živou hladký přenos (Přehrát)
- Live DVR funkcí (pozastavit, hledání, DVR přehrávání přejít to-Live)
- Podpora pro Videokodeky - H.264
- Podpora pro zvukové kodeky - AAC
- Více zvukových jazyk přepínání s předdefinované rozhraní API OSMF
- Výběr kvality přehrávání Max předdefinované rozhraní API OSMF
- Něho skryté titulky s modul plug-in OSMF titulků
- Adobe&reg; Flash&reg; přehrávače 11.4 nebo vyšší.
- Tato verze podporuje pouze OSMF 2.0.

## <a name="supported-features-and-known-issues"></a>Podporované funkce a známé problémy

Úplný seznam podporovaných funkcí naleznete nepodporované funkce a známé problémy najdete [v tomto dokumentu](http://download.microsoft.com/download/3/1/B/31B63D97-574E-4A8D-BF8D-170744181724/Smooth_Streaming_Plugin_for_OSMF.pdf).


## <a name="loading-the-plugin"></a>Načtení modulu plug-in
Moduly plug-in OSMF lze načíst staticky (době překladu) nebo dynamicky (za běhu). Modul plug-in hladký přenos ke stažení OSMF obsahuje dynamické i statické verze.

- Statická načítání: staticky načtení souboru statické knihovny (SWC) požaduje. Moduly plug-in pro statické někdo přidá jako odkaz na projektech a sloučení uvnitř konečný výstupní soubor době překladu.

- Dynamické načítání: K načtení dynamicky, je potřeba neaktivním () souboru. Moduly plug-in pro dynamické načtena v modulu runtime se ale nezahrnutý do výstupu projektu. (Kompilovaný výstup) Moduly plug-in pro dynamické můžete načtena pomocí protokoly HTTP a soubor.

Další informace o statický a dynamické načítání najdete v článku oficiální [OSMF modul plug-in stránky](http://osmf.org/dev/osmf/OtherPDFs/osmf_plugin_dev_guide.pdf).

###<a name="ss-for-osmf-static-loading"></a>SS OSMF statické načítání
Fragment kódu ukazuje, jak načíst SS modul plug-in pro OSMF staticky videa a přehrávání základní pomocí OSMF MediaFactory třídy. Před včetně SS OSMF kódem, zkontrolujte, zda projektový odkaz obsahuje statické modul plug-in "MSAdaptiveStreamingPlugin v1.0.3 osmf2.0.swc".

```
package 
{
    
    import com.microsoft.azure.media.AdaptiveStreamingPluginInfo;
    
    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;
    
    
    
    [SWF(width="1024", height="768", backgroundColor='#405050', frameRate="25")]
    public class TestPlayer extends Sprite
    {        
        public var _container:MediaContainer;
        public var _mediaFactory:DefaultMediaFactory;
        private var _mediaPlayerSprite:MediaPlayerSprite;
        

        public function TestPlayer( )
        {
            stage.quality = StageQuality.HIGH;

            initMediaPlayer();

        }
    
        private function initMediaPlayer():void
        {
        
            // Create the container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);
            _mediaPlayerSprite.scaleMode = ScaleMode.NONE;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            //Adds the container to the stage
            addChild(_mediaPlayerSprite);
            
            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();
            
            // Add the listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );
            
            // Load the plugin class 
            loadAdaptiveStreamingPlugin( );  
            
        }
        
        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;
            
            pluginResource = new PluginInfoResource(new AdaptiveStreamingPluginInfo( )); 
            _mediaFactory.loadPlugin( pluginResource ); 
        }
        
        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // The plugin is loaded successfully.
            // Your web server needs to host a valid crossdomain.xml file to allow plugin to download Smooth Streaming files.
        loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")
        
        }
        
        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // The plugin is failed to load ...
        }
        
        
        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;
            
            state =  event.state;
            
            switch (state)
            {
                case MediaPlayerState.LOADING: 
                    
                    // A new source is started to load.
                    
                    break;
                
                case  MediaPlayerState.READY :   
                    // Add code to deal with Player Ready when it is hit the first load after a source is loaded. 
                    
                    break;
                
                case MediaPlayerState.BUFFERING :
                    
                    break;
                
                case  MediaPlayerState.PAUSED :
                    break;      
                // other states ...          
            }
        }
        
        private function onPlayerFailed(event:MediaErrorEvent) : void
        {
            // Media Player is failed .           
        }
        
        private function loadMediaSource(sourceURL : String):void 
        {
            // Take an URL of SmoothStreamingSource's manifest and add it to the page.
            
            var resource:URLResource= new URLResource( sourceURL );
            
            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            
            // Add the media element
            _mediaPlayerSprite.media = element;
        }     
        
    }
}
```


###<a name="ss-for-osmf-dynamic-loading"></a>SS pro dynamické načítání OSMF

Fragment kódu ukazuje, jak dynamicky načíst SS modul plug-in pro OSMF a přehrání základní videa pomocí aplikace onenotové OSMF MediaFactory. Před včetně SS OSMF kódem, zkopírujte dynamické modul plug-in "MSAdaptiveStreamingPlugin v1.0.3 osmf2.0.swf" do složky projektu, pokud chcete načíst pomocí souboru protokolu nebo zkopírujte ve skupinovém rámečku na webový server pro HTTP načíst. Je potřeba zahrnout "MSAdaptiveStreamingPlugin v1.0.3 osmf2.0.swc" do odkazů na projektu.

 
{balíčku
    
    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;
    import flash.events.Event;
    import flash.system.Capabilities;

    
    //Sets the size of the SWF
    
    [SWF(width="1024", height="768", backgroundColor='#405050', frameRate="25")]
    public class TestPlayer extends Sprite
    {        
        public var _container:MediaContainer;
        public var _mediaFactory:DefaultMediaFactory;
        private var _mediaPlayerSprite:MediaPlayerSprite;
        
        
        public function TestPlayer( )
        {
            stage.quality = StageQuality.HIGH;
            initMediaPlayer();
        }
        
        private function initMediaPlayer():void
        {
            
            // Create the container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);

            //Adds the container to the stage
            addChild(_mediaPlayerSprite);
            
            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();
            
            // Add the listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );
            
            // Load the plugin class 
            loadAdaptiveStreamingPlugin( );  
            
        }
        
        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;
            var adaptiveStreamingPluginUrl:String;

            // Your dynamic plugin web server needs to host a valid crossdomain.xml file to allow loading plugins.

            adaptiveStreamingPluginUrl = "http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf";
            pluginResource = new URLResource(adaptiveStreamingPluginUrl);
            _mediaFactory.loadPlugin( pluginResource ); 

        }
        
        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // The plugin is loaded successfully.

            // Your web server needs to host a valid crossdomain.xml file to allow plugin to download Smooth Streaming files.

    loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")
        }
        
        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // The plugin is failed to load ...
        }
        
        
        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;
            
            state =  event.state;
            
            switch (state)
            {
                case MediaPlayerState.LOADING: 
                    
                    // A new source is started to load.
                    
                    break;
                
                case  MediaPlayerState.READY :   
                    // Add code to deal with Player Ready when it is hit the first load after a source is loaded. 
                    
                    break;
                
                case MediaPlayerState.BUFFERING :
                    
                    break;
                
                case  MediaPlayerState.PAUSED :
                    break;      
                // other states ...          
            }
        }
        
        private function onPlayerFailed(event:MediaErrorEvent) : void
        {
            // Media Player is failed .           
        }
        
        private function loadMediaSource(sourceURL : String):void 
        {
            // Take an URL of SmoothStreamingSource's manifest and add it to the page.
            
            var resource:URLResource= new URLResource( sourceURL );
            
            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            // Add the media element
            _mediaPlayerSprite.media = element;
        }     
        
    }
}

##<a name="strobe-media--playback-with-the-ss-odmf-dynamic-plugin"></a>Přehrávání média zábleskové s SS ODMF dynamické modulu plug-in

Hladký přenos pro dynamické modul plug-in OSMF je kompatibilní se službou [Zábleskové médií přehrávání SMP ()](http://osmf.org/strobe_mediaplayback.html). SS modul plug-in OSMF slouží k přidání hladký přenos přehrávání obsahu na SMP. K tomuto účelu zkopírujte "MSAdaptiveStreamingPlugin v1.0.3 osmf2.0.swf" v části na webový server pro načtení HTTP pomocí následujících kroků:

1.  Přejděte na [přehrávání média zábleskové vzhled stránky](http://osmf.org/dev/2.0gm/setup.html). 
2.  Nastavení src na hladký přenos zdroj (například http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest) 
3.  Proveďte požadované změny nastavení a klikněte na Náhled a aktualizace.
 
    **Poznámka:** Obsahu webového serveru musí platné crossdomain.xml. 
4.  Kopírování a vložení kódu na stránku jednoduchý HTML v oblíbených textovém editoru, jako je třeba v následujícím příkladu:



        <html>
        <body>
        <object width="920" height="640"> 
        <param name="movie" value="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf"></param>
        <param name="flashvars" value="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest &autoPlay=true"></param>
        <param name="allowFullScreen" value="true"></param>
        <param name="allowscriptaccess" value="always"></param>
        <param name="wmode" value="direct"></param>
        <embed src="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf" 
            type="application/x-shockwave-flash" 
            allowscriptaccess="always" 
            allowfullscreen="true" 
            wmode="direct" 
            width="920" 
            height="640" 
            flashvars=" src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true">
        </embed>
        </object>
        </body>
        </html>



5. Přidání kódu pro vložení hladký přenos OSMF plug-in a uložte.

        <html>
        <object width="920" height="640"> 
        <param name="movie" value="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf"></param>
        <param name="flashvars" value="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true&plugin_AdaptiveStreamingPlugin=http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf&AdaptiveStreamingPlugin_retryLive=true&AdaptiveStreamingPlugin_retryInterval=10"></param>
        <param name="allowFullScreen" value="true"></param>
        <param name="allowscriptaccess" value="always"></param>
        <param name="wmode" value="direct"></param>
        <embed src="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf" 
            type="application/x-shockwave-flash" 
            allowscriptaccess="always" 
            allowfullscreen="true" 
            wmode="direct" 
            width="920" 
            height="640" 
            flashvars="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true&plugin_AdaptiveStreamingPlugin=http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf&AdaptiveStreamingPlugin_retryLive=true&AdaptiveStreamingPlugin_retryInterval=10">
        </embed>
        </object>
        </html>


6.  Uložení stránky HTML a publikovat na webový server. Přejděte na publikované webové stránce pomocí Oblíbené Flash&reg; přehrávače povolené internetového prohlížeče (Internet Explorer, Chrome, Firefox, tak dál).
7.  Využijte hladký přenos obsahu uvnitř Adobe&reg; Flash&reg; Player.

Další informace o obecný vývoj OSMF najdete oficiální [OSMF vývoj stránky](http://osmf.org/resources.html).

##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



##<a name="see-also"></a>Viz taky

[Microsoft adaptivní streamování modul plug-in pro OSMF aktualizace](https://azure.microsoft.com/blog/2014/10/27/microsoft-adaptive-streaming-plugin-for-osmf-update/) 
