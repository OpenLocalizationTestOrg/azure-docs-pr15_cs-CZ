<properties
 pageTitle="Jak spravovat vypršení platnosti Azure webové aplikace/cloudové služby, ASP.NET a IIS obsahu v Azure CDN | Microsoft Azure"
 description="Popisuje, jak spravovat vypršení platnosti cloudové služby obsahu v Azure CDN"
 services="cdn"
 documentationCenter=".NET"
 authors="camsoper"
 manager="erikre"
 editor=""/>
<tags
 ms.service="cdn"
 ms.workload="media"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="09/19/2016"
 ms.author="casoper"/>

# <a name="how-to-manage-expiration-of-azure-web-appscloud-services-aspnet-or-iis-content-in-azure-cdn"></a>Jak spravovat vypršení platnosti Azure webové aplikace/cloudové služby, ASP.NET nebo služby IIS obsahu v Azure CDN

> [AZURE.SELECTOR]
- [Azure webové aplikace/cloudové služby, ASP.NET nebo služby IIS](cdn-manage-expiration-of-cloud-service-content.md)
- [Služba Azure úložiště objektů blob](cdn-manage-expiration-of-blob-content.md)

Soubory z libovolné veřejně přístupný origin webového serveru můžete uložené v Azure CDN, dokud uplyne time to live (TTL).  Hodnota TTL, je určený podle [záhlaví *Cache Control* ](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) v odpovědi HTTP ze serveru origin.  Tento článek popisuje, jak nastavit `Cache-Control` záhlaví pro Azure Web Apps, Azure Cloud Services, aplikace a Internetové informační služby weby, které můžete konfigurovat podobně.

>[AZURE.TIP] Můžete nastavit žádné TTL na soubor.  Azure CDN v tomto případě použije automaticky výchozí hodnota TTL sedmi dnů.
>
>Další informace o tom, jak funguje Azure CDN urychlit přístup k souborům a další zdroje informací najdete v článku [Přehled CDN Azure](./cdn-overview.md).

## <a name="setting-cache-control-headers-in-configuration"></a>Nastavení záhlaví mezipaměti ovládacích prvků v konfiguraci

Na statický obsah, například obrázky a šablony stylů můžete určit interval aktualizace změnou **applicationHost.config** nebo **web.config** souborů pro webové aplikace.  Element **system.webServer\staticContent\clientCache** v souboru konfigurace nastaví `Cache-Control` záhlaví pro obsah. Pro **web.config**nastavení konfigurace ovlivní všechny položky ve složce a všech jejích podsložkách, pokud přepsat na úrovni podsložky.  Můžete třeba nastavit výchozí time-to-live na kořenové úrovni chcete, aby všechny statický obsah v mezipaměti pro tři dny, ale máte podsložku, která má více proměnné obsah s nastavením mezipaměti 6 hodin.  **ApplicationHost.config**všech aplikací na web bude to mít vliv na, ale můžete přepsat v souborech **web.config** v aplikacích.

Následující zobrazuje XML a příklady nastavení **clientCache** můžete určit maximální doba 3 dnů:  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

Určení **UseMaxAge** přidá `Cache-Control: max-age=<nnn>` záhlaví k odpovědi na základě hodnoty uvedené v atributu **CacheControlMaxAge** . Formát timespan je pro atribut **cacheControlMaxAge** <days>. <hours>:<min>:<sec>. Další informace na uzel **clientCache** najdete v tématu [mezipaměti klienta <clientCache> ](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).  

## <a name="setting-cache-control-headers-in-code"></a>Nastavení záhlaví mezipaměti ovládacích prvků v kódu

U aplikací ASP.NET můžete nastavit CDN ukládání do mezipaměti chování programově nastavením vlastnosti **HttpResponse.Cache** . Další informace o vlastnost **HttpResponse.Cache** najdete v článku [HttpResponse.Cache vlastnost](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) a [HttpCachePolicy předmětu](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

Pokud chcete programově mezipaměti obsah aplikace v ASP.NET, ujistěte se, že obsah je označen jako vytvářených nastavením HttpCacheability na *veřejné*. Také ověřte, zda ověřování mezipaměti. Ověřování mezipaměti může být naposledy změněno časové razítko nastavit tak, že zavoláte SetLastModified nebo hodnota etag nastavit tak, že zavoláte SetETag. Pokud chcete můžete také nastavit dobu platnosti mezipaměti tak, že zavoláte SetExpires nebo můžete spolehnout na výchozí heuristiky mezipaměti popsané výše v tomto dokumentu.  

Například do mezipaměti obsahu hodinu, přidejte tyto informace:  

```csharp
// Set the caching parameters.
Response.Cache.SetExpires(DateTime.Now.AddHours(1));
Response.Cache.SetCacheability(HttpCacheability.Public);
Response.Cache.SetLastModified(DateTime.Now);
```

## <a name="next-steps"></a>Další kroky

- [Podrobné informace o elementu **clientCache**](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)
- [Přečtěte si dokumentaci pro vlastnost **HttpResponse.Cache**](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) 
- [Pro čtení v dokumentaci k **HttpCachePolicy předmětu**](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  
