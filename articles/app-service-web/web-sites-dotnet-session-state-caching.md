<properties 
    pageTitle="Stav relace s Azure Redis mezipaměti aplikace služby Azure" 
    description="Naučte se používat službu Azure mezipaměti pro podporu technologie ASP.NET relace stavu ukládání do mezipaměti." 
    services="app-service\web" 
    documentationCenter=".net" 
    authors="Rick-Anderson" 
    manager="wpickett" 
    editor="none"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="get-started-article" 
    ms.date="06/27/2016" 
    ms.author="riande"/>


# <a name="session-state-with-azure-redis-cache-in-azure-app-service"></a>Stav relace s Azure Redis mezipaměti aplikace služby Azure


Toto téma vysvětluje, jak používat službu Azure Redis mezipaměti pro stav relace.

Jestliže webovou aplikaci ASP.NET používá stav relace, bude potřeba nakonfigurovat poskytovatele externí relace stavu (služby Redis mezipaměti nebo poskytovatele stavu relace serveru SQL Server). Pokud používáte stav relace a nepoužívejte externí poskytovatele, budete bude omezené výskyt webovou aplikaci. Služby Redis mezipaměti je nejrychlejší a nejjednodušší povolit.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

##<a id="createcache"></a>Vytvoření mezipaměti
Postupujte podle [těchto pokynů](../cache-dotnet-how-to-use-azure-redis-cache.md#create-cache) a vytvořte mezipaměti.

##<a id="configureproject"></a>Přidání balíček RedisSessionStateProvider NuGet do webové aplikace
Instalace NuGet `RedisSessionStateProvider` balíčku.  Pomocí následujícího příkazu můžete nainstalovat z konzoly Správce balíčku (**Nástroje** > **Správce balíčků NuGet** > **Konzoly balíčku správce**):

  `PM> Install-Package Microsoft.Web.RedisSessionStateProvider`
  
Můžete nainstalovat z **Nástroje** > **Správce balíčků NuGet** > **Správa NugGet balíčků řešení**, vyhledejte `RedisSessionStateProvider`.

Další informace najdete v článku [NuGet RedisSessionStateProvider stránky](http://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider/ ) a [Konfigurace klienta mezipaměti](../cache-dotnet-how-to-use-azure-redis-cache.md#NuGet).

##<a id="configurewebconfig"></a>Úprava nastavení(Web.config))
Kromě vytváření odkazů na sestavení pro mezipaměti, přidává balíček NuGet *nastavení(Web.config))* zástupná procedura položky. 

1. Otevřete *web.config* a najděte elementu **sessionState** .

1. Zadejte požadované hodnoty pro `host`, `accessKey`, `port` (SSL port by měl být 6380) a nastavte `SSL` k `true`. Tyto hodnoty můžete získat z [Azure portál](http://go.microsoft.com/fwlink/?LinkId=529715) zásuvné instance mezipaměti. Další informace najdete v tématu [připojení do mezipaměti](../cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-cache). Všimněte si, že je ve výchozím nastavení pro nové mezipaměti zakázaná port a protokol SSL. Další informace o povolení port není SSL naleznete v části [Přístup porty](https://msdn.microsoft.com/library/azure/dn793612.aspx#AccessPorts) v tématu [Konfigurace mezipaměti v Azure Redis mezipaměti](https://msdn.microsoft.com/library/azure/dn793612.aspx) . Následující kód ukazuje změny *nastavení(Web.config)), konkrétně změny *portu*, *host*, accessKey* *, a *ssl *.

          <system.web>;
            <customErrors mode="Off" />;
            <authentication mode="None" />;
            <compilation debug="true" targetFramework="4.5" />;
            <httpRuntime targetFramework="4.5" />;
            <sessionState mode="Custom" customProvider="RedisSessionProvider">;
              <providers>;  
                  <!--<add name="RedisSessionProvider" 
                    host = "127.0.0.1" [String]
                    port = "" [number]
                    accessKey = "" [String]
                    ssl = "false" [true|false]
                    throwOnError = "true" [true|false]
                    retryTimeoutInMilliseconds = "0" [number]
                    databaseId = "0" [number]
                    applicationName = "" [String]
                  />;-->;
                 <add name="RedisSessionProvider" 
                      type="Microsoft.Web.Redis.RedisSessionStateProvider" 
                      port="6380"
                      host="movie2.redis.cache.windows.net" 
                      accessKey="m7PNV60CrvKpLqMUxosC3dSe6kx9nQ6jP5del8TmADk=" 
                      ssl="true" />;
              <!--<add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="127.0.0.1" accessKey="" ssl="false" />;-->;
              </providers>;
            </sessionState>;
          </system.web>;


##<a id="usesessionobject"></a>Použití objektu Session v kódu
Posledním krokem je v práci začít používat objekt relace kódu ASP.NET. Přidání objektů stavu relace pomocí metody **Session.Add** . Tato metoda používá klíč dvojice ukládat položky v mezipaměti stavu relace.

    string strValue = "yourvalue";
    Session.Add("yourkey", strValue);

Následující kód načte tuto hodnotu ze stavu relace.

    object objValue = Session["yourkey"];
    if (objValue != null)
       strValue = (string)objValue; 

Můžete taky Redis mezipaměti objektů mezipaměti ve web appu. Další informace najdete v tématu [aplikace movie MVC s Azure Redis mezipaměti 15 minut](https://azure.microsoft.com/blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/).
Podrobné informace o tom, jak používat stav relace ASP.NET najdete v článku [Přehled stavu relace ASP.NET][].

>[AZURE.NOTE] Pokud chcete začít pracovat s aplikaci služby Azure před registrací účet Azure, přejděte na [Zkuste aplikaci služby](http://go.microsoft.com/fwlink/?LinkId=523751), které můžete okamžitě vytvořit web appu krátkodobý starter v aplikaci služby. Žádné povinné; kreditní karty žádné závazky.

## <a name="whats-changed"></a>Co se změnilo
* Průvodce na změnu z webů pro aplikaci služby v tématu: [aplikaci služby Azure a jeho dopad na existující služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

  *Podle [Anderson RTF](https://twitter.com/RickAndMSFT)*
  
  [installed the latest]: http://www.windowsazure.com/downloads/?sdk=net  
  [Přehled stavu relace ASP.NET]: http://msdn.microsoft.com/library/ms178581.aspx

  [NewIcon]: ./media/web-sites-dotnet-session-state-caching/CacheScreenshot_NewButton.png
  [NewCacheDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CreateOptions.png
  [CacheIcon]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheIcon.png
  [NuGetDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_NuGet.png
  [OutputConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_OC_WebConfig.png
  [CacheConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheConfig.png
  [EndpointURL]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_EndpointURL.png
  [ManageKeys]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_ManageAccessKeys.png
 
