<properties
    pageTitle="Opakovat logiky Media Services SDK pro .NET | Microsoft Azure"
    description="V tématu najdete přehled použití logických operátorů opakovat v Media Services SDK pro .NET."
    authors="Juliako"
    manager="erikre"
    editor=""
    services="media-services"
    documentationCenter=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016" 
    ms.author="juliako"/>


# <a name="retry-logic-in-the-media-services-sdk-for-net"></a>Opakovat logiky Media Services SDK pro .NET

Při práci se službami Microsoft Azure, může dojít přechodná chyby. Pokud se vyskytne přechodná poruch ve většině případů, po pár pokusech operace se mu. Media Services SDK pro .NET používá opakovat logiky pro zpracování přechodná chyby související s výjimky a chyby, které jsou způsobená web požadavky, provádění dotazů, uložení změn a skladování.  Ve výchozím nastavení Media Services SDK pro .NET provede čtyři opakování před opakované vyvolání výjimky aplikaci. Kód v aplikaci musí potom zpracovat tato výjimka správně.  
  
 Následující obrázek je stručný návod webového požadavku, úložiště, dotazu a SaveChanges zásad:  
  
-   Zásady úložiště se používá pro operace úložiště objektů blob (nahrávání nebo stahování souborů majetku).  
  
-   Zásady požadavku Web používá pro obecný web požadavků na (například pro získání token ověřování a řešení problémů s clusteru koncového uživatele).  
  
-   Zásady dotazu se používá k dotazování entity ostatních (například mediaContext.Assets.Where(...)).  
  
-   Zásady SaveChanges se používá pro to něco nezměnilo dat v rámci služby (například vytvoření entity aktualizace entita volání funkce služby pro operaci).  
  
 Toto téma obsahuje seznam typů výjimky a kódy chyb, které zpracovávají Media Services SDK pro .NET opakování použití logických operátorů.  
  
## <a name="exception-types"></a>Typy výjimek  

Následující tabulka popisuje výjimky Media Services SDK pro .NET zpracovává nebo zpracovává pro některé operace, které tuto chybu může způsobovat přechodná chyby.  
  
Výjimky|Žádost o webu|Úložiště|Dotaz|SaveChanges
----|------|----|---|---
WebException<br/>Další informace naleznete v části [WebException stavů](media-services-retry-logic-in-dotnet-sdk.md#WebExceptionStatus) .|Ano|Ano|Ano|Ano  
DataServiceClientException<br/> Další informace najdete v tématu [kódy chyb stav HTTP](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|Ne|Ano|Ano|Ano
DataServiceQueryException<br/> Další informace najdete v tématu [kódy chyb stav HTTP](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|Ne|Ano|Ano|Ano  
DataServiceRequestException<br/> Další informace najdete v tématu [kódy chyb stav HTTP](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|Ne|Ano|Ano|Ano  
DataServiceTransportException|Ne|Ne|Ano|Ano
TimeoutException|Ano|Ano|Ano|Ne
SocketException|Ano|Ano|Ano|Ano  
StorageException|Ne|Ano|Ne|Ne 
IOException|Ne|Ano|Ne|Ne
  
###  <a name="WebExceptionStatus"></a>WebException stavů  

Následující tabulka zobrazuje, pro které kódy chyb WebException Logika opakování implementovaná. Výčet [WebExceptionStatus](http://msdn.microsoft.com/library/system.net.webexceptionstatus.aspx) definuje kódů stavu.  
  
Stav|Žádost o webu|Úložiště|Dotaz|SaveChanges  
-----|-----------------|-------------|-----------|----------  
ConnectFailure|Ano|Ano|Ano|Ano
NameResolutionFailure|Ano|Ano|Ano|Ano  
ProxyNameResolutionFailure|Ano|Ano|Ano|Ano  
SendFailure|Ano|Ano|Ano|Ano
PipelineFailure|Ano|Ano|Ano|Ne  
ConnectionClosed|Ano|Ano|Ano|Ne  
KeepAliveFailure|Ano|Ano|Ano|Ne  
UnknownError|Ano|Ano|Ano|Ne 
ReceiveFailure|Ano|Ano|Ano|Ne  
RequestCanceled|Ano|Ano|Ano|Ne  
Časový limit|Ano|Ano|Ano|Ne
Požadavku <br/>Opakování na požadavku řídí zpracování HTTP stavový kód. Další informace najdete v tématu [kódy chyb stav HTTP](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|Ano|Ano|Ano|Ano|  
  
###  <a name="HTTPStatusCode"></a>Kódy chyb stav HTTP  

Když operace dotazu a SaveChanges vyvolat DataServiceClientException, DataServiceQueryException nebo DataServiceQueryException, stavový kód chyby HTTP vrácen ve vlastnosti StatusCode.  Následující tabulka zobrazuje, pro které kódy chyb Logika opakování implementovaná.  
  
 
Stav|Žádost o webu|Úložiště|Dotaz|SaveChanges 
---|----|----|----|----
401|Ne|Ano|Ne|Ne
403|Ne|Ano<br/>Zpracování opakování s delší čeká.|Ne|Ne  
408|Ano|Ano|Ano|Ano
429|Ano|Ano|Ano|Ano  
500|Ano|Ano|Ano|Ne  
502|Ano|Ano|Ano|Ne  
503|Ano|Ano|Ano|Ano  
504|Ano|Ano|Ano|Ne  
  
Pokud chcete převzít pohled na skutečného provedení Services SDK médií pro .NET opakování použití logických operátorů najdete v tématu [azure sdk pro médií služby](https://github.com/Azure/azure-sdk-for-media-services/tree/dev/src/net/Client/TransientFaultHandling).

## <a name="next-steps"></a>Další kroky

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
