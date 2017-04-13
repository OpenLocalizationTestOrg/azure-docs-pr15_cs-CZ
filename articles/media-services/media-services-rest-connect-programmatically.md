<properties 
    pageTitle="Připojení k účtu služby médií pomocí rozhraní REST API | Microsoft Azure" 
    description="Toto téma ukazuje, jak se připojit k Media Services uisng rozhraní REST API." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="juliako"/>


# <a name="connecting-to-media-services-account-using-media-services-rest-api"></a>Připojení k účtu služby médií pomocí Media Services REST API

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-connect-programmatically.md)
- [ZBÝVAJÍCÍ](media-services-rest-connect-programmatically.md)

Toto téma popisuje, jak při programování s Media Services REST API získat programové připojení ke službě Microsoft Azure Media Services.

Při přístupu k Microsoft Azure Media Services podporují dvě věci: přístupový token poskytovanou Azure Access Services (Control ACS) a identifikátor URI Media Services samotné. Můžete použít jakýmkoli způsobem, který chcete při vytváření tyto požadavky, dokud, zadejte hodnoty správné záhlaví a předávání v přístupový token správně při volání do Media Services.

Následující seznam popisuje nejčastější pracovního postupu při použití Media Services REST API pro připojení k Media Services:

1. Získání přístupový token 
2. Připojit se ke službám médií URI 

    >[AZURE.NOTE] Po úspěšném připojení k https://media.windows.net, zobrazí se 301 přesměrování zadání jiného identifikátor URI médií služeb. Musíte provést následující volání nových URI.
Taky můžete obdržet protokolu HTTP/1.1 200 odpověď, která obsahuje popis metadat rozhraní API ODATA.

3. Pokládat dotazy k další hovory rozhraní API na nová adresa URL. 

    Například pokud po pokouší připojit, jste získali takto:

        HTTP/1.1 301 Moved Permanently
        Location: https://wamsbayclus001rest-hs.cloudapp.net/api/

    Zveřejňujete další hovory rozhraní API https://wamsbayclus001rest-hs.cloudapp.net/api/.

##<a name="access-control-address"></a>Adresa řízení přístupu k

Adresa řízení přístupu k Media Services je https://wamsprodglobal001acs.accesscontrol.windows.net, s výjimkou severní Čína oblast, kde je https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn.

##<a name="getting-an-access-token"></a>Získání přístupový token

Media Services přímo prostřednictvím rozhraní REST API, načtení přístupový token z ACS a použít při každé žádost HTTP provedené do služby. Tento token je podobný další tokeny poskytovanou ACS podle deklarací přístup k dispozici v záhlaví na žádost HTTP a pomocí protokolu v2 OAuth. Další požadavky nemusí před přímo připojit se ke službám média.

Následující příklad ukazuje záhlaví žádost HTTP a textu použitý k načtení token.

**Záhlaví**:

    POST https://wamsprodglobal001acs.accesscontrol.windows.net/v2/OAuth2-13 HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Host: wamsprodglobal001acs.accesscontrol.windows.net
    Content-Length: 120
    Expect: 100-continue
    Connection: Keep-Alive
    Accept: application/json

    
**Text**:

Musíte prokázat client_id a client_secret hodnoty v těle tohoto požadavku; client_id a client_secret odpovídají hodnoty název účtu a AccountKey. Tyto hodnoty jsou poskytnuté Media Services po nastavení účtu. 

Nezapomeňte, že AccountKey účtu Media Services musí být kódování URL (viz [Heslo Percent-Encoding](http://tools.ietf.org/html/rfc3986#section-2.1) při použití jako hodnotu client_secret v tokenu žádosti o přístup.

    grant_type=client_credentials&client_id=ams_account_name&client_secret=URL_encoded_ams_account_key&scope=urn%3aWindowsAzureMediaServices


Příklad: 

    grant_type=client_credentials&client_id=amstestaccount001&client_secret=wUNbKhNj07oqjqU3Ah9R9f4kqTJ9avPpfe6Pk3YZ7ng%3d&scope=urn%3aWindowsAzureMediaServices


Následující příklad ukazuje HTTP odpověď, která obsahuje přístup tokenu v těle odpovědi.

    HTTP/1.1 200 OK
    Cache-Control: no-cache, no-store
    Pragma: no-cache
    Content-Type: application/json; charset=utf-8
    Expires: -1
    request-id: a65150f5-2784-4a01-a4b7-33456187ad83
    X-Content-Type-Options: nosniff
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Thu, 15 Jan 2015 08:07:20 GMT
    Content-Length: 670
    
    {  
       "token_type":"http://schemas.xmlsoap.org/ws/2009/11/swt-token-profile-1.0",
       "access_token":"http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421330840&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=uf69n82KlqZmkJDNxhJkOxpyIpA2HDyeGUTtSnq1vlE%3d",
       "expires_in":"21600",
       "scope":"urn:WindowsAzureMediaServices"
    }
    

>[AZURE.NOTE]
Doporučujeme do mezipaměti "access_token" a "expires_in" hodnoty, které mají externí úložiště. Tokenu dat může později načtené z úložiště a znovu použít v Media Services REST API volání. Toto je užitečné především pro scénáře kde tokenu možné bezpečně sdílet mezi více procesů nebo počítačů.

Zkontrolujte, že sledovat hodnotu "expires_in" přístupový token a podle potřeby aktualizujte hovory rozhraní REST API s novou tokeny.

###<a name="connecting-to-the-media-services-uri"></a>Připojit se ke službám médií URI

Kořenové URI Media Services je https://media.windows.net/. By měl původně připojení k této URI a pokud se zobrazí 301 přesměrování zpět v odpovědi, je třeba následující volání nových URI. Kromě toho nepoužívejte jakékoli logiky automatického přesměrování a postupujte podle vašich požadavků. Akce protokolu HTTP a žádost o instituce nebude přepošlou nové URI.

Všimněte si, že kořenovou URI pro nahrávání a stahování souborů materiálů https://yourstorageaccount.blob.core.windows.net/ kde název účtu úložiště je stejný jako ten, který jste použili při nastavení účtu Media Services.

Následující příklad ukazuje žádost HTTP kořenové Media Services identifikátor URI (https://media.windows.net/). Žádost může vstoupit 301 přesměrování zpět odpověď. Následná žádost používá nové URI (https://wamsbayclus001rest-hs.cloudapp.net/api/).     

**Žádost HTTP**:
    
    GET https://media.windows.net/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: media.windows.net


**Nastavit informace HTTP odpověď**:
    
    HTTP/1.1 301 Moved Permanently
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/
    Server: Microsoft-IIS/8.5
    request-id: 987d7652-497a-44e5-b815-f492e02aef97
    x-ms-request-id: 987d7652-497a-44e5-b815-f492e02aef97
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sat, 17 Jan 2015 07:44:53 GMT
    Content-Length: 164
    
    <html><head><title>Object moved</title></head><body>
    <h2>Object moved to <a href="https://wamsbayclus001rest-hs.cloudapp.net/api/">here</a>.</h2>
    </body></html>


**Žádost HTTP** (pomocí nového URI):
            
    GET https://wamsbayclus001rest-hs.cloudapp.net/api/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: wamsbayclus001rest-hs.cloudapp.net


**Nastavit informace HTTP odpověď**:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 1250
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 5f52ea9d-177e-48fe-9709-24953d19f84a
    x-ms-request-id: 5f52ea9d-177e-48fe-9709-24953d19f84a
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sat, 17 Jan 2015 07:44:52 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata","value":[{"name":"AccessPolicies","url":"AccessPolicies"},{"name":"Locators","url":"Locators"},{"name":"ContentKeys","url":"ContentKeys"},{"name":"ContentKeyAuthorizationPolicyOptions","url":"ContentKeyAuthorizationPolicyOptions"},{"name":"ContentKeyAuthorizationPolicies","url":"ContentKeyAuthorizationPolicies"},{"name":"Files","url":"Files"},{"name":"Assets","url":"Assets"},{"name":"AssetDeliveryPolicies","url":"AssetDeliveryPolicies"},{"name":"IngestManifestFiles","url":"IngestManifestFiles"},{"name":"IngestManifestAssets","url":"IngestManifestAssets"},{"name":"IngestManifests","url":"IngestManifests"},{"name":"StorageAccounts","url":"StorageAccounts"},{"name":"Tasks","url":"Tasks"},{"name":"NotificationEndPoints","url":"NotificationEndPoints"},{"name":"Jobs","url":"Jobs"},{"name":"TaskTemplates","url":"TaskTemplates"},{"name":"JobTemplates","url":"JobTemplates"},{"name":"MediaProcessors","url":"MediaProcessors"},{"name":"EncodingReservedUnitTypes","url":"EncodingReservedUnitTypes"},{"name":"Operations","url":"Operations"},{"name":"StreamingEndpoints","url":"StreamingEndpoints"},{"name":"Channels","url":"Channels"},{"name":"Programs","url":"Programs"}]}
     


>[AZURE.NOTE] Po nastavení nové URI, který je URI, který má být použit komunikovat s Media Services. 


##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
