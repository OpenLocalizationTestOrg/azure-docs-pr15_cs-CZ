<properties 
    pageTitle="Začínáme s doručení obsahu na vyžádání pomocí ZBÝVAJÍCÍ | Microsoft Azure" 
    description="Tento kurz vás provede jednotlivými kroky implementace aplikace pro doručování obsahu na služba pomocí Azure Media Services pomocí rozhraní REST API." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016" 
    ms.author="juliako"/>

#<a name="get-started-with-delivering-content-on-demand-using-rest"></a>Začínáme s doručení obsahu na vyžádání pomocí REST 

[AZURE.INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]


>[AZURE.NOTE]
> Pro dokončení tohoto kurzu, třeba účet Azure. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](/pricing/free-trial/?WT.mc_id=A261C142F). 


Tento rychlý úvod vás provede kroky provádění aplikace doručování obsahu poskytují (VoD) pomocí rozhraní REST API Azure Media Services (AMS). 

Kurz představuje základní pracovního postupu Media Services a nejběžnější programovací objekty a úkoly potřebné pro vývoj Media Services. Po dokončení kurzu budou moct můžete vysílat datovými proudy nebo postupně stáhnout ukázkový soubor médií nahráli, kódování a stáhnout.  

## <a name="prerequisites"></a>Zjistit předpoklady pro
Spusťte vývoj s Media Services s rozhraní REST API neexistují následující požadavky.

- Pochopení toho, jak se dají pomocí rozhraní REST API Media Services. Další informace najdete v tématu [media services zbývající přehled](http://msdn.microsoft.com/library/azure/hh973616.aspx).
- Aplikace podle svého výběru, který můžete poslat HTTP žádosti o schůzku a odpovědi. Tento kurz používá [Fiddler](http://www.telerik.com/download/fiddler). 

V tomto rychlý úvod jsou zobrazeny úkoly podle těchto pokynů.

1.  Vytvoření účtu Media Services (na portálu Azure).
2.  Konfigurace streamování koncového bodu (na portálu Azure).
1.  Připojení k účtu Media Services pomocí rozhraní REST API.
1.  Vytvořte nový majetek a nahrajte soubor videa pomocí rozhraní REST API.
1.  Konfigurace streamování jednotky pomocí rozhraní REST API.
2.  Kódování zdrojového souboru do sadu adaptivní bitrate MP4 souborů pomocí rozhraní REST API.
1.  Publikujte materiálů a získat streamování postupné stahování adresy URL a pomocí rozhraní REST API. 
1.  Přehrávání obsahu. 

## <a name="create-an-azure-media-services-account-using-the-azure-portal"></a>Vytvořte účet Azure Media Services pomocí portálu Azure

Postup v této části ukazují, jak vytvořit účet AMS.

1. Přihlaste se na [portál Azure](https://portal.azure.com/).
2. Klikněte na **+ Nový** > **médií + CDN** > **Media Services**.

    ![Vytvoření Media Services](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. **Vytvoření** účtu služby médií zadejte požadované hodnoty.

    ![Vytvoření Media Services](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. Do pole **Název účtu**zadejte název nového účtu AMS. Název účtu Media Services je všechna malá písmena číslic nebo písmen bez mezer a 3 až 24 znaků.
    2. V předplatného vyberte mezi různých Azure předplatné, které máte přístup.
    
    2. **Pole Skupina zdroje**vyberte zdroj nové nebo existující.  Skupina zdroje je kolekce zdroje, které mají životního cyklu, oprávnění a zásady. Další informace [v tomto poli](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. Do pole **umístění**vyberte zeměpisná oblast slouží k uložení záznamy média a metadat pro váš účet Media Services. Tato oblast slouží k obrázku a vysílání datových proudů. Do pole rozevíracího seznamu se zobrazí jenom k dispozici oblasti Media Services. 
    
    3. **Úložiště účtu**vyberte účet úložiště k poskytování úložiště objektů blob obsahu ze svého účtu Media Services. Můžete vybrat existující účet úložiště ve stejné zeměpisná oblast účtem Media Services nebo můžete vytvořit účet úložiště. Vytvoření nového účtu úložiště ve stejné oblasti. Pravidla pro názvy účtů úložiště jsou stejné jako u účtů Media Services.

        Další informace o ukládání [tady](storage-introduction.md).

    4. Vyberte **Připnout do řídicího panelu** zobrazíte průběh nasazení účtu.
    
7. Klikněte na **vytvořit** v dolní části formuláře.

    Po úspěšném vytvoření účtu stav se změní na **systém**. 

    ![Nastavení Media Services](./media/media-services-portal-vod-get-started/media-services-settings.png)

    Správa účtu AMS (například nahrát videa, kódovat prostředky, sledovat pokrok projektu) pomocí okna **Nastavení** .

## <a name="configure-streaming-endpoints-using-the-azure-portal"></a>Konfigurace streamování koncové body pomocí portálu Azure

Při práci s Azure Media Services jednu obvyklé scénáře je poskytly videa přes adaptivní bitrate streamování klienty. Media Services podporuje následující adaptivní přenosová streamování technologie: HTTP Live datových proudů (HLS) hladký přenos, MPEG POMLČKU a konfigurace (pro Adobe PrimeTime/přístup nabyvatelů licence pouze).

Služba Media poskytuje dynamické balení, který umožňuje poskytovat adaptivní bitrate MP4 zakódovaný obsah v datových proudů formáty podporované kodérem Media Services (MPEG POMLČKU HLS, hladký přenos, konfigurace) těsně za běhu, aniž byste museli ukládat předem sbalené verze každého z nich streamování formáty.

Umožní využít výhod dynamické balení potřebujete postupujte takto:

- Kódování mezzanine (zdrojový) souboru do sadu adaptivní bitrate MP4 souborů (kódování kroky jsou prokázáno dál v tomto kurzu).  
- Vytvořte alespoň jeden streamování jednotku pro *streamování koncový bod* ze které budete chtít doručení obsahu. Postupem zobrazení, jak lze změnit počet datových proudů jednotky.

Dynamické balení, stačí k ukládání a zaplatit soubory ve formátu jediný úložiště a Media Services vytvoří a slouží odpovídající odpověď založená na žádosti o z klienta.

Vytvořit a změnit počet datových proudů rezervovaná jednotky, postupujte takto:


1. V okně **Nastavení** klikněte na **datových proudů koncové body**. 

2. Klikněte na výchozí streamování koncového bodu. 

    Zobrazí se okno **Výchozí STREAMOVÁNÍ podrobnosti o koncovém bodu** .

3. Chcete-li zadat počet jednotek, datových proudů, posuňte jezdec **datových proudů jednotky** .

    ![Přenos jednotky](./media/media-services-portal-vod-get-started/media-services-streaming-units.png)

4. Kliknutím na tlačítko **Uložit** uložte provedené změny.

    >[AZURE.NOTE]Přidělení nové jednotky může trvat až 20 minut.

## <a id="connect"></a>Připojení k účtu Media Services pomocí rozhraní REST API

Při přístupu k Azure Media Services podporují dvě věci: přístupový token poskytovanou Azure Access Services (Control ACS) a identifikátor URI Media Services samotné. Můžete použít jakýmkoli způsobem, který chcete při vytváření tyto požadavky, dokud, zadejte hodnoty správné záhlaví a předávání v přístupový token správně při volání do Media Services.

Následující seznam popisuje nejčastější pracovního postupu při použití Media Services REST API pro připojení k Media Services:

1. Získání přístupový token. 
2. Připojit se ke službám médií URI.  

    Mějte na paměti, že po úspěšném připojení k https://media.windows.net, dostanete 301 přesměrování zadání jiného identifikátor URI médií služeb. Musíte provést následující volání nových URI. Taky můžete obdržet protokolu HTTP/1.1 200 odpověď, která obsahuje popis metadat rozhraní API ODATA.
3. Publikování příspěvku následné rozhraní API volání na nová adresa URL. 
    
    Například pokud po pokouší připojit, jste získali takto:
        
        HTTP/1.1 301 Moved Permanently
        Location: https://wamsbayclus001rest-hs.cloudapp.net/api/

    Zveřejňujete další hovory rozhraní API https://wamsbayclus001rest-hs.cloudapp.net/api/.

###<a name="getting-an-access-token"></a>Získání přístupový token

Media Services přímo prostřednictvím rozhraní REST API, načtení přístupový token z ACS a použít při každé žádost HTTP provedené do služby. Další požadavky nemusí před přímo připojit se ke službám média.

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

AccountKey pro váš účet Media Services, musíte být kódování URL při použití jako hodnotu client_secret v tokenu žádosti o přístup.

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
       "access_token":"http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421330840&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=uf69n82KlqZmkJDNxhJkOxpyIpA2HDyeGUTtSnq1vlE%3d",
       "expires_in":"21600",
       "scope":"urn:WindowsAzureMediaServices"
    }
    

>[AZURE.NOTE]
Doporučujeme do mezipaměti "access_token" a "expires_in" hodnoty, které mají externí úložiště. Tokenu dat může později načtené z úložiště a znovu použít v Media Services REST API volání. Toto je užitečné především pro scénáře kde tokenu možné bezpečně sdílet mezi více procesů nebo počítačů.

Zkontrolujte, že sledovat hodnotu "expires_in" přístupový token a podle potřeby aktualizujte hovory rozhraní REST API s novou tokeny.

###<a name="connecting-to-the-media-services-uri"></a>Připojit se ke službám médií URI

Kořenové URI Media Services je https://media.windows.net/. By měl původně připojení k této URI a pokud se zobrazí 301 přesměrování zpět v odpovědi, je třeba následující volání nových URI. Kromě toho nepoužívejte jakékoli logiky automatického přesměrování a postupujte podle vašich požadavků. Akce protokolu HTTP a žádost o instituce nebude přepošlou nové URI.

Kořenové URI pro nahrávání a stahování souborů materiálů je https://yourstorageaccount.blob.core.windows.net/ případů název účtu úložiště stejný jako ten, který jste použili při nastavení účtu Media Services.

Následující příklad ukazuje žádost HTTP kořenové Media Services identifikátor URI (https://media.windows.net/). Žádost může vstoupit 301 přesměrování zpět odpověď. Následná žádost používá nové URI (https://wamsbayclus001rest-hs.cloudapp.net/api/).     

**Žádost HTTP**:
    
    GET https://media.windows.net/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
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
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
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
     


>[AZURE.NOTE] Od této chvíle nové URI slouží v tomto kurzu.

## <a id="upload"></a>Vytvořte nový majetek a nahrajte soubor videa pomocí rozhraní REST API

V dialogovém okně médií služby nahrajte svoje soubory digitální do aktivum. Entita **materiálů** může obsahovat video, zvuk, obrázky, miniatur kolekce, text skladeb a titulků soubory (a metadata o těchto souborech.)  Jakmile soubory jsou odeslány do majetek, obsah uložena bezpečně v cloudu pro další zpracování a přenos. 

Jednou z hodnoty, které je třeba zadat při vytváření aktivum je možnosti vytvoření materiálů. Vlastnost **Možnosti** je hodnota výčtu, která jsou uvedeny dostupné šifrovací možnosti, které aktivum můžete vytvořené pomocí. Platná hodnota je taková hodnot v seznamu pod není kombinací hodnot v tomto seznamu:

 
- **Žádná** = **0** - bez šifrování se používá. Při použití tohoto parametru není zamknutí obsahu při přenosu šifrovaná nebo u ostatních v úložišti.
    Pokud budete chtít předvádění MP4 pomocí postupného stahování použijte tuto možnost. 
- **StorageEncrypted** = **1** - šifruje vymazat obsah místně pomocí šifrování AES 256 a odešle ji do úložiště Azure kde je uložena zašifrovat zbývající. Prostředky chráněné pomocí šifrování úložiště jsou automaticky bez šifrování umístí do systém zašifrovaných souborů před kódování a volitelně znovu zašifrovaných před odesláním zpět jako nový majetek výstupu. Primární použití případ pro šifrování úložiště je když budete chtít zabezpečené vysoce kvalitní vstupní mediální soubory s silné šifrování u ostatních na disku.
- **CommonEncryptionProtected** = **2** – použití tuto možnost, pokud ukládáte obsah, který už zašifrovaných a chráněny běžné šifrování nebo PlayReady DRM (například hladký přenos chráněné s PlayReady DRM).
- **EnvelopeEncryptionProtected** = **4** – použití tuto možnost, pokud ukládáte HLS zašifrovaných AES. Soubory, které musí mít kódování a zašifrovaných transformace správce.

### <a name="create-an-asset"></a>Vytvoření aktivum

Aktivum je kontejner pro více typů nebo sady objektů v Media Services, včetně video, zvuk, obrázky, miniatur kolekce, skladeb text a soubory skryté titulky. Vytvoření aktivum v rozhraní REST API vyžaduje poslat žádost o příspěvek Media Services a umístění vlastnost informace o vaší materiálů v hlavním textu žádosti o.

Následující příklad ukazuje, jak vytvořit aktivum.

**Žádost HTTP**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 45
    
    {"Name":"BigBuckBunny.mp4", "Options":"0"}
    

**Odpověď HTTP**

Pokud je úspěšná, je vrácena takto:
    
    HTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 452
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    request-id: e98be122-ae09-473a-8072-0ccd234a0657
    x-ms-request-id: e98be122-ae09-473a-8072-0ccd234a0657
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT
        
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny2.mp4",
       "Options":0,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }
    
### <a name="create-an-assetfile"></a>Vytvoření AssetFile

Entita [AssetFile](http://msdn.microsoft.com/library/azure/hh974275.aspx) představuje videoklipu nebo zvukového souboru, který je uložený v kontejneru objektů blob. Soubor materiálů je vždy přidružené k aktivum a aktivum může obsahovat jedno nebo více AssetFiles. Úkol Media Encoder služby nezdaří, pokud soubor objektu materiálů je přidružená k souboru digitální v kontejneru objektů blob.

Po odeslání souboru digitálních médií do kontejneru objektů blob slouží k aktualizaci AssetFile s informacemi o váš soubor médií (znázorněná na pozdější v tématu) žádost HTTP **Sloučit** . 

**Žádost HTTP**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 164
    
    {  
       "IsEncrypted":"false",
       "IsPrimary":"false",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**Odpověď HTTP**

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 535
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5')
    Server: Microsoft-IIS/8.5
    request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    x-ms-request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 00:34:07 GMT
    
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Files/@Element",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "Name":"BigBuckBunny.mp4",
       "ContentFileSize":"0",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "EncryptionVersion":null,
       "EncryptionScheme":null,
       "IsEncrypted":false,
       "EncryptionKeyId":null,
       "InitializationVector":null,
       "IsPrimary":false,
       "LastModified":"2015-01-19T00:34:08.1934137Z",
       "Created":"2015-01-19T00:34:08.1934137Z",
       "MimeType":"video/mp4",
       "ContentChecksum":null
    }


### <a name="creating-the-accesspolicy-with-write-permission"></a>Vytvoření AccessPolicy pomocí oprávnění k zápisu. 

Před nahráním všech souborů v úložišti objektů blob, nastavte přístup práva zásad pro psaní aktivum. Aby je dostala, ZVEŘEJŇUJÍ žádost HTTP na sadu AccessPolicies entity. Definování doba trvání v minutách hodnoty při vytváření nebo dostanete zprávu 500 Chyba interním serveru zpět v odpovědi. Další informace o AccessPolicies najdete v tématu [AccessPolicy](http://msdn.microsoft.com/library/azure/hh974297.aspx).

Následující příklad ukazuje, jak vytvořit AccessPolicy:
        
**Žádost HTTP**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 74
    
    {"Name":"NewUploadPolicy", "DurationInMinutes":"440", "Permissions":"2"} 

**Odpověď HTTP**

    If successful, the following response is returned:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 312
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae')
    Server: Microsoft-IIS/8.5
    request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    x-ms-request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:18:06 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#AccessPolicies/@Element",
       "Id":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "Created":"2015-01-18T22:18:06.6370575Z",
       "LastModified":"2015-01-18T22:18:06.6370575Z",
       "Name":"NewUploadPolicy",
       "DurationInMinutes":440.0,
       "Permissions":2
    }

### <a name="get-the-upload-url"></a>Získání adresy URL nahrát

Získat adresu URL skutečné nahrát, vytvořte Locator přidružení zabezpečení. Locator definovat počáteční čas a typ koncového bodu připojení klientů, kteří mají přístup k souborům ve aktivum. Můžete vytvořit více entity URL pro daný AccessPolicy a materiálů pár zpracovávat žádosti o jiného klienta a potřeb. Každá z těchto Locator používá hodnotu čas spuštění plus doba trvání v minutách hodnotu AccessPolicy určit dobu, kterou lze použít adresy URL. Další informace najdete v tématu [Locator](http://msdn.microsoft.com/library/azure/hh974308.aspx).


Adresa URL přidružení zabezpečení má v tomto formátu:

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

Některé aspekty, které platí:

- Nesmí obsahovat více než pět jedinečné Locator přidružený k dané materiálů najednou. Další informace najdete v tématu Locator.
- Pokud potřebujete okamžitě nahrajte svoje soubory, by měl nastavíte čas spuštění hodnotu pět minut, než aktuální čas. Je to proto může existovat hodiny skew mezi klientském počítači a Media Services. Také, váš čas spuštění hodnota musí být v v tomto formátu data a času: rrrr-MM-DDTHH:mm:ssZ (například "2014-05-23T17:53:50Z").   
- Doména může obsahovat podruhé 30-40 zpoždění po Locator k při vznikne je k dispozici. Týká se adresa URL přidružení zabezpečení a Locator Origin.

Následující příklad ukazuje, jak vytvořit URL adresy URL přidružení zabezpečení, způsobem definovaným ve vlastnosti typ v hlavním textu žádosti o ("1" pro locator přidružení zabezpečení) a "2" pro locator origin na vyžádání. Vlastnost **Path** vrácené obsahuje adresu URL, kterou musíte pomocí nahrajte soubor.
    
**Žádost HTTP**
    
    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=f7f09258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 178
    
    {  
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Type":1
    }


**Odpověď HTTP**

Pokud je úspěšná, je vrácena následující odpověď:
        
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 949
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54')
    Server: Microsoft-IIS/8.5
    request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    x-ms-request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 03:01:29 GMT
    
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Locators/@Element",
       "Id":"nb:lid:UUID:af57bdd8-6751-4e84-b403-f3c140444b54",
       "ExpirationDateTime":"2015-02-19T00:05:53",
       "Type":1,
       "Path":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "BaseUri":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247",
       "ContentAccessComponent":"?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Name":null
    }

### <a name="upload-a-file-into-a-blob-storage-container"></a>Nahrání souboru do kontejneru úložiště objektů blob
    
Až budete mít AccessPolicy a Locator nastavit, skutečné soubor nahrát do kontejneru úložišti objektů Blob Azure pomocí REST API Azure úložiště. Můžete nahrát na stránce nebo zablokovat objektů BLOB. 

>[AZURE.NOTE] Je třeba přidat název souboru soubor, který chcete nahrát na hodnotu Locator **cestu** dostali v předchozí části. Například https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4? . . . 

Další informace o práci s objekty BLOB Azure úložiště najdete v tématu [Objektů Blob služba REST API](http://msdn.microsoft.com/library/azure/dd135733.aspx).


### <a name="update-the-assetfile"></a>Aktualizace AssetFile 

Teď, když nahrajete soubor, aktualizujte informace o FileAsset velikost (a další). Příklad:
    
    MERGE https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5') HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    
    {  
       "ContentFileSize":"1186540",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**Odpověď HTTP**

Pokud úspěšné, následující vrátí se hodnota: protokolu HTTP/1.1 204 bez obsahu

## <a name="delete-the-locator-and-accesspolicy"></a>Odstranění Locator a AccessPolicy 

**Žádost HTTP**


    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

    
**Odpověď HTTP**

Pokud je úspěšná, je vrácena takto:

    HTTP/1.1 204 No Content 
    ...

**Žádost HTTP**

    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

**Odpověď HTTP**

Pokud je úspěšná, je vrácena takto:

    HTTP/1.1 204 No Content 
    ...

 
## <a id="configure_streaming_units"></a>Konfigurace streamování jednotky pomocí rozhraní REST API

Při práci s Azure Media Services jednu obvyklé scénáře je poskytly adaptivní bitrate streamování klienty. U streamování adaptivní bitrate klienta můžete přejít vyšších a nižších bitrate toku jako video se zobrazí na základě aktuální šířka pásma, využití procesoru a jiných faktorů. Media Services podporuje následující adaptivní přenosová streamování technologie: HTTP Live datových proudů (HLS) hladký přenos, MPEG POMLČKU a konfigurace (pro Adobe PrimeTime/přístup nabyvatelů licence pouze). 

Služba Media poskytuje dynamické balení, který umožňuje poskytovat adaptivní bitrate MP4 a přitom zajistit hladký přenos zakódovaný úprav textu v datových proudů formáty podporované kodérem Media Services (MPEG POMLČKU HLS, hladký přenos, konfigurace) aniž byste museli k opětovnému vytvoření balíčku do těchto streamování formáty. 

Umožní využít výhod dynamické balení potřebujete postupujte takto:

- Získejte alespoň jeden streamování jednotku pro **streamování koncový bod **ze které budete chtít doručení obsahu (popsaná v této části).
- zakódovat nebo program mezzanine (zdrojový) soubor do sady adaptivní bitrate MP4 soubory nebo Adaptivní bitrate hladký přenos souborů (kódování kroky jsou prokázáno dál v tomto kurzu)  

Dynamické balení, stačí k ukládání a zaplatit soubory ve formátu jediný úložiště a Media Services vytvoří a slouží odpovídající odpověď založená na žádosti o z klienta. 


>[AZURE.NOTE] Informace o cenách podrobnosti najdete v tématu [Media Services ceny podrobnosti](http://go.microsoft.com/fwlink/?LinkId=275107).

Pokud chcete změnit počet datových proudů rezervovaná jednotky, postupujte takto:
    
### <a name="get-the-streaming-endpoint-you-want-to-update"></a>Získání streamování koncový bod, který chcete aktualizovat

Například první streamování koncový bod nastavíme ve vašem účtu (může mít až dva streamování koncové body v stav po spuštění ve stejnou dobu.)

**Žádost HTTP**:

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/StreamingEndpoints()?$top=1 HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421466122&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=TiKGEOTporft4pFGU24sSZRZk5GRAWszFXldl5NXAhY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

**Odpověď HTTP**
    
Pokud je úspěšná, je vrácena takto:
    
    HTTP/1.1 200 OK
    . . . 
    
### <a name="scale-the-streaming-endpoint"></a>Změna velikosti datových proudů koncový bod
 
**Žádost HTTP**:

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/StreamingEndpoints('nb:oid:UUID:cd57670d-cc1c-0f86-16d8-3ad478bf9486')/Scale HTTP/1.1
    Content-Type: application/json;odata=verbose
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421466122&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=TiKGEOTporft4pFGU24sSZRZk5GRAWszFXldl5NXAhY%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 39f96c93-a4b1-43ce-b97e-b2aaa44ee2dd
    Host: wamsbayclus001rest-hs.cloudapp.net
    
    {"scaleUnits":1}

**Odpověď HTTP**

    HTTP/1.1 202 Accepted
    Cache-Control: no-cache
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 39f96c93-a4b1-43ce-b97e-b2aaa44ee2dd
    request-id: 3c1ba1c7-281c-4b2d-a898-09cb70a3a424
    x-ms-request-id: 3c1ba1c7-281c-4b2d-a898-09cb70a3a424
    operation-id: nb:opid:UUID:1853bcbf-b71f-4ed5-a4c7-a581d4f45ae7
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Fri, 16 Jan 2015 22:16:43 GMT
    Content-Length: 0

    
### <a id="long_running_op_status"></a>Kontrola stavu operace časově náročné

Přidělení nové jednotky trvá asi 20 minut. Kontrola stavu operace použijte metodu **operace** a určit, Id operace. Operace Id vrátil v odpovědi na žádosti o **Měřítko** .

    operation-id: nb:opid:UUID:1853bcbf-b71f-4ed5-a4c7-a581d4f45ae7
 
**Žádost HTTP**:
    
    GET https://wamsbayclus001rest-hs.cloudapp.net/api/Operations('nb:opid:UUID:1853bcbf-b71f-4ed5-a4c7-a581d4f45ae7') HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421466122&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=TiKGEOTporft4pFGU24sSZRZk5GRAWszFXldl5NXAhY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    
**Odpověď HTTP**
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 515
    Content-Type: application/json;odata=verbose;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 829e1a89-3ec2-4836-a04d-802b5aeff5e8
    request-id: f7ae8a78-af8d-4881-b9ea-ca072cfe0b60
    x-ms-request-id: f7ae8a78-af8d-4881-b9ea-ca072cfe0b60
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Fri, 16 Jan 2015 22:57:39 GMT
    
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.cloudapp.net/api/Operations('nb%3Aopid%3AUUID%3Acc339c28-6bba-4f7d-bee5-91ea4a0a907e')",
             "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Operations('nb%3Aopid%3AUUID%3Acc339c28-6bba-4f7d-bee5-91ea4a0a907e')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Operation"
          },
          "Id":"nb:opid:UUID:cc339c28-6bba-4f7d-bee5-91ea4a0a907e",
          "State":"Succeeded",
          "TargetEntityId":"nb:oid:UUID:cd57670d-cc1c-0f86-16d8-3ad478bf9486",
          "ErrorCode":null,
          "ErrorMessage":null
       }
    }


## <a id="encode"></a>Kódování zdrojového souboru do sadu adaptivní bitrate MP4 souborů

Po ingesting, které můžete zakódovaný prostředky do Media Services, médií transmuxed vodotiskem a tak dále než ho dostane klienty. Tyto činnosti naplánované se spustí hledání v několika instancích spuštěných pozadí role ověřit vysoký výkon a dostupnosti. Tyto činnosti nazývají úlohy a jednotlivé [úlohy](http://msdn.microsoft.com/library/azure/hh974289.aspx) se skládá z atomová úkoly, které se skutečnou práci se souborem materiálů. 

Byla zmínili o tom, kdy práce s Azure Media Services z obvyklé scénáře je předvádění adaptivní bitrate datových proudů klienty. Media Services můžete dynamicky balíček sadu adaptivní bitrate MP4 souborů do jedné z následujících formátů: HTTP Live datových proudů (HLS) hladký přenos, MPEG POMLČKU a konfigurace (pro Adobe PrimeTime/přístup nabyvatelů licence pouze). 

Umožní využít výhod dynamické balení potřebujete postupujte takto:

- zakódovat nebo program mezzanine (zdrojový) soubor do sady adaptivní bitrate MP4 soubory nebo Adaptivní bitrate hladký přenos souborů  
- potřebujete alespoň jeden streamování jednotku pro streamování koncový bod, ze které budete chtít doručení obsahu. 

V následující části ukazuje, jak vytvořit projekt, který obsahuje jeden úkol kódování. Úkol Určuje převod souboru mezzanine do sady adaptivní bitrate MP4s pomocí **Media Encoder standardní**. V části také ukazuje, jak sledovat průběh zpracování úlohu. Po dokončení projektu je bude moct vytvářet Locator, které jsou potřeba k získání přístupu k svém majetku. 

### <a name="get-a-media-processor"></a>Získání procesor médií

V Media Services procesor médií je součást, která zajišťuje konkrétní zpracování úkolu, například kódování převod formátu šifrování a dešifrování médií obsahu. Kódování úkolu v tomto kurzu budeme používat Media Encoder standardní.

Následující kód požadavky encoder id. 

**Žádost HTTP**

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=f7f09258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    

**Odpověď HTTP**

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 273
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 6beb04b4-55a7-480d-8aa8-e5c5d59ffa1f
    x-ms-request-id: 6beb04b4-55a7-480d-8aa8-e5c5d59ffa1f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 07:54:09 GMT
    
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }

### <a name="create-a-job"></a>Vytvoření projektu

Jednotlivé úlohy může mít jeden či více úkolů v závislosti na typu zpracování, kterou chcete provést. Pomocí rozhraní REST API můžete vytvořit úlohy a související úkoly jedním ze dvou způsobů: úkoly mohou být definované vložené prostřednictvím vlastnosti navigace úkoly v projektu entity, nebo OData dávkové zpracování. Media Services SDK používá dávkové zpracování. Pro snazší čitelnost příklady v tomto tématu, jsou úkoly definovaný vložené. Informace o dávkové zpracování najdete v tématu [Dávkové zpracování Data (OData Open Protocol)](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).

Následující příklad ukazuje, jak vytvářet a publikovat projekt se jeden úkol kódování videa v určitém rozlišení a kvalitu. V následující části si přečtěte následující dokumentaci obsahuje seznam všech [úkolů předvolby](http://msdn.microsoft.com/library/mt269960) podporovaných Media Encoder standardní procesorem.  

**Žádost HTTP**
    
    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: application/json
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 482
    
    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset>
                <outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          }
       ]
    }

**Odpověď HTTP**

Pokud je úspěšná, je vrácena následující odpověď:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1215
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')
    Server: Microsoft-IIS/8.5
    request-id: 532ac1ec-a475-4dce-b2d5-7c8ce94ac87c
    x-ms-request-id: 532ac1ec-a475-4dce-b2d5-7c8ce94ac87c
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 08:04:35 GMT
        
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')",
             "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Job"
          },
          "Tasks":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/Tasks"
             }
          },
          "OutputMediaAssets":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets"
             }
          },
          "InputMediaAssets":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')/InputMediaAssets"
             }
          },
          "Id":"nb:jid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
          "Name":"NewTestJob",
          "Created":"2015-01-19T08:04:34.3287057Z",
          "LastModified":"2015-01-19T08:04:34.3287057Z",
          "EndTime":null,
          "Priority":0,
          "RunningDuration":0,
          "StartTime":null,
          "State":0,
          "TemplateId":null,
          "JobNotificationSubscriptions":{  
             "__metadata":{  
                "type":"Collection(Microsoft.Cloud.Media.Vod.Rest.Data.Models.JobNotificationSubscription)"
             },
             "results":[  
    
             ]
          }
       }
    }


Poznámka: v jakékoliv žádosti o úlohy několik důležitých věcí:

- Vlastnosti TaskBody musíte použít literál XML definice počet vstupní a výstupní materiálů, které využívají úkol. Téma úkolu obsahuje definice schématu XML pro XML.
- V definici TaskBody hodnota pro každou vnitřní <inputAsset> a <outputAsset> musí být nastavený jako JobInputAsset(value) nebo JobOutputAsset(value).
- Úkol může obsahovat více výstup prostředky. Jeden JobOutputAsset(x) lze použít pouze jednou jako výstup úkolu v projektu.
- Jako aktivum vstupní úkolu můžete zadat JobInputAsset nebo JobOutputAsset.
- Úkoly nesmí formuláře obrázku.
- Parametr hodnoty, které předáváte JobInputAsset nebo JobOutputAsset představuje hodnotu indexu aktivum. Skutečné prostředky jsou definované ve vlastnostech InputMediaAssets a OutputMediaAssets navigace na definici úlohy entity. 

>[AZURE.NOTE] Protože Media Services jsou založeny na OData v3, jednotlivé prostředky v kolekcích vlastnosti navigace InputMediaAssets a OutputMediaAssets odkazují "__metadata: identifikátor uri" pár název hodnota. 

- InputMediaAssets mapuje jeden nebo více prostředky nevytvoříte Media Services. OutputMediaAssets vzniká systém. Nelze odkazovat existující materiálů.
- OutputMediaAssets můžete s názvem použití atribut assetName. Pokud tento atribut není k dispozici, je název OutputMediaAsset jakéhokoliv vnitřní textovou hodnotu <outputAsset> element je s příponou hodnotu z pole název projektu nebo hodnotu Id úlohy (v případě, kde není definované vlastnosti název). Například pokud nastavíte hodnotu assetName "Vzorku", pak vlastnost název OutputMediaAsset by nastavit "Vzorku". Ale pokud nenastavil hodnotu assetName, ale nastavena názvu projektu "NewJob", pak název OutputMediaAsset by "_NewJob JobOutputAsset (hodnota)". 

    Následující příklad ukazuje, jak nastavit atribut assetName:
    
        "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"


- Chcete-li povolit zřetězení úkolu:

    - Úlohy musíte mít aspoň dva úkoly
    - Musí být alespoň jeden úkol, jehož vstup je výstup jiného úkolu v projektu.

Další informace najdete [vytváření projektů kódování s Media Services REST API](http://msdn.microsoft.com/library/azure/jj129574.aspx).

### <a name="monitor-processing-progress"></a>Sledování průběhu zpracování

Můžete načíst stav úlohy pomocí vlastnosti státu, jak je vidět v následujícím příkladu. 

**Žádost HTTP**

    GET https://wamsbayclus001rest-hs.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/State HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=zf84471d-2233-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336908022&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=RYXOraO6Z%2f7l9whWZQN%2bypeijgHwIk8XyikA01Kx1%2bk%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 0


**Odpověď HTTP**

Pokud je úspěšná, je vrácena následující odpověď:

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 17
    Content-Type: application/json;odata=verbose;charset=utf-8
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 01324d81-18e2-4493-a3e5-c6186209f0c8
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Sun, 13 May 2012 05:16:53 GMT
    
    {"d":{"State":2}}


### <a name="cancel-a-job"></a>Zrušení úlohy

Media Services můžete zrušit spuštění úlohy pomocí funkce CancelJob. Volání vrátí kód 400 chyby při pokusu o zrušení úlohy při stavu zrušeno zrušení, chyby nebo dokončením.

Následující příklad ukazuje, jak volání CancelJob.


**Žádost HTTP**


    GET https://wamsbayclus001rest-hs.net/API/CancelJob?jobid='nb%3ajid%3aUUID%3a71d2dd33-efdf-ec43-8ea1-136a110bd42c' HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.2
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336908753&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=kolAgnFfbQIeRv4lWxKSFa4USyjWXRm15Kd%2bNd5g8eA%3d
    Host: wamsbayclus001rest-hs.net


Pokud úspěšné, vrátí se kód 204 odpovědi s žádné zprávy.

>[AZURE.NOTE] Je třeba adresu URL kódovat id práce (běžně nb:jid:UUID: somevalue) při přechodu v jako parametr CancelJob.


### <a name="get-the-output-asset"></a>Získání materiálů výstup 

Následující kód ukazuje, jak požádat o materiálů výstup Id. 


**Žádost HTTP**

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets() HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    

**Odpověď HTTP**

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 445
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 73cd605d-066a-46f1-8358-f4bd25a9220a
    x-ms-request-id: 73cd605d-066a-46f1-8358-f4bd25a9220a
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 08:28:13 GMT
        
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets",
       "value":[  
          {  
             "Id":"nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
             "State":0,
             "Created":"2015-01-19T07:52:15.603",
             "LastModified":"2015-01-19T07:52:15.603",
             "AlternateId":null,
             "Name":"Multibitrate MP4s",
             "Options":0,
             "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-71d2dd33-efdf-ec43-8ea1-136a110bd42c",
             "StorageAccountName":"storagetestaccount001"
          }
       ]
    }



## <a id="publish_get_urls"></a>Publikovat materiálů a získat streamování postupné stahování adresy URL a pomocí rozhraní REST API

Můžete vysílat datovými proudy nebo stahování aktivum, musíte nejdřív "publikovat" vytvořením locator. Locator poskytnutí přístupu k souborům obsažené v majetek. Media Services podporuje dva typy Locator: OnDemandOrigin Locator slouží k toku médií (například MPEG POMLČKU HLS a přitom zajistit hladký přenos) a Locator přidružení přístup podpisu (zabezpečení) slouží ke stažení mediální soubory.

Jakmile vytvoříte Locator, je možné vytvářet adresy URL, které se používají můžete vysílat datovými proudy nebo stahování souborů. 


Streamování adresy URL pro hladký přenos má v tomto formátu:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Streamování adresy URL pro HLS má v tomto formátu:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Streamování adresy URL pro MPEG POMLČKU má v tomto formátu:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


Adresa URL přidružení zabezpečení použít ke stahování souborů má v tomto formátu:

    {blob container name}/{asset name}/{file name}/{SAS signature}

Tato část popisuje provádět následující úkoly třeba "publikovat" svém majetku.  

- Vytváření AccessPolicy s oprávněním ke čtení 
- Vytvoření přidružení zabezpečení adresy URL pro stahování obsahu 
- Vytvoření původní URL pro přenos obsahu 

###<a name="creating-the-accesspolicy-with-read-permission"></a>Vytváření AccessPolicy s oprávněním ke čtení

Před stažením nebo datových proudů veškerý obsah média, nejdřív definovat AccessPolicy s oprávnění pro čtení a vytvořte odpovídající Locator osoba, která určuje typ mechanismy, kterou chcete povolit pro klienty. Další informace o vlastnostech k dispozici najdete v článku [AccessPolicy Entity vlastnosti](https://msdn.microsoft.com/library/azure/hh974297.aspx#accesspolicy_properties).

Následující příklad ukazuje, jak můžete určit AccessPolicy oprávnění pro čtení pro dané materiálů.

    POST https://wamsbayclus001rest-hs.net/API/AccessPolicies HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 74
    Expect: 100-continue
    
    {"Name": "DownloadPolicy", "DurationInMinutes" : "300", "Permissions" : 1}

Pokud úspěšné, kód 201 úspěšnosti vrátí se hodnota popisující AccessPolicy entitu, kterou jste vytvořili. Potom pomocí AccessPolicy Id spolu s Id majetku materiálů, která obsahuje soubor, který chcete vytvořit entitu Locator dodat (například aktivum výstup).

>[AZURE.NOTE]
Základní postup je stejná jako nahrávání souboru při ingesting aktiva (jak bylo popsáno dříve v tomto tématu). Také jako nahrávání souborů, v případě vy (nebo klienty) se okamžitě přístup k souborům, nastavte svůj čas spuštění hodnotu pět minut, než aktuální čas. Tato akce je nutné, protože může existovat hodiny skew mezi klientem a Media Services. Čas spuštění hodnota musí být v následujícím formátu data a času: YYYY-MM-DDTHH:mm:ssZ (například "2014-05-23T17:53:50Z").


###<a name="creating-a-sas-url-for-downloading-content"></a>Vytvoření přidružení zabezpečení adresy URL pro stahování obsahu 

Následující kód ukazuje, jak získat adresu URL, která slouží ke stažení mediální soubor vytvořili a jste předtím nahráli. AccessPolicy četl sadu oprávnění a cestu URL odkazuje na adresu URL pro stažení přidružení zabezpečení.

    POST https://wamsbayclus001rest-hs.net/API/Locators HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=zf84471d-b1ae-2233-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 182
    Expect: 100-continue
    
    {"AccessPolicyId": "nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8", "AssetId" : "nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c", "StartTime" : "2014-05-17T16:45:53", "Type":1}

Pokud je úspěšná, je vrácena následující odpověď:

    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1150
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 8cfd8556-3064-416a-97f2-67d9e35f0911
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Mon, 14 May 2012 21:41:32 GMT
        
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')",
             "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Locator"
          },
          "AccessPolicy":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')/AccessPolicy"
             }
          },
          "Asset":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/Asset"
             }
          },
          "Id":"nb:lid:UUID:8e5a821d-2194-4d00-8884-adf979856874",
          "ExpirationDateTime":"\/Date(1337049393000)\/",
          "Type":1,
          "Path":"https://storagetestaccount001.blob.core.windows.net/asset-71d2dd33-efdf-ec43-8ea1-136a110bd42c?st=2012-05-14T21%3A36%3A33Z&se=2012-05-15T02%3A36%3A33Z&sr=c&si=8e5a821d-2194-4d00-8884-adf979856874&sig=y75dViDpC5V8WutrXM%2B%2FGpR3uOtqmlISiNlHU1YUBOg%3D",
          "AccessPolicyId":"nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8",
          "AssetId":"nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
          "StartTime":"\/Date(1337031393000)\/"
       }
    }


Vlastnost vrácené **cesta** obsahuje adresa URL přidružení zabezpečení.

>[AZURE.NOTE]
Stahování zašifrovaných úložiště obsahu se musí ručně dešifrování před vykreslování ho, nebo použijte MediaProcessor dešifrování úložiště v úkol zpracování výstupních zpracovaných souborů v vymazat OutputAsset a pak budou moct stáhnout z této materiálů. Další informace o zpracování najdete v článku Vytvoření kódování úlohu s Media Services REST API. Navíc přidružení zabezpečení adres URL Locator po nelze aktualizovat jste vytvořili. Například nelze znovu použít stejné Locator s aktualizovanou hodnotou čas spuštění. Toto je kvůli způsob, jakým se vytvářejí adresy URL přidružení zabezpečení. Pokud chcete získat přístup materiály ke stažení po vypršení platnosti Locator, musíte vytvořit nový účet s nový čas spuštění.

###<a name="download-files"></a>Stahování souborů

Až budete mít AccessPolicy a Locator nastavení, můžete si stáhnout soubory pomocí Azure úložiště REST API.  

>[AZURE.NOTE] Je třeba přidat název souboru soubor, který chcete stáhnout hodnotě Locator **cestu** dostali v předchozí části. Například https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4? . . . 

Další informace o práci s objekty BLOB Azure úložiště najdete v tématu [Objektů Blob služba REST API](http://msdn.microsoft.com/library/azure/dd135733.aspx).

Důsledku kódování projektu, se kterým jste provedli starší (kódování do sady adaptivní MP4) máte několik MP4 soubory, které si můžete stáhnout postupně. Příklad:    
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


### <a name="creating-a-streaming-url-for-streaming-content"></a>Vytváření datových proudů adresy URL pro přenos obsahu


Následující kód ukazuje, jak vytvořit datové proudy URL adresy URL:

    POST https://wamsbayclus001rest-hs/API/Locators HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs
    Content-Length: 182
    Expect: 100-continue
    
    {"AccessPolicyId": "nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8", "AssetId" : "nb:cid:UUID:eb5540a2-116e-4d36-b084-7e9958f7f3c3", "StartTime" : "2014-05-17T16:45:53",, "Type":2}

Pokud je úspěšná, je vrácena následující odpověď:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 981
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 2eac4158-fc78-4fa2-81ee-c9f582dc385f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Mon, 14 May 2012 21:41:39 GMT
        
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')",
             "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Locator"
          },
          "AccessPolicy":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')/AccessPolicy"
             }
          },
          "Asset":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')/Asset"
             }
          },
          "Id":"nb:lid:UUID:52034bf6-dfae-4d83-aad3-3bd87dcb1a5d",
          "ExpirationDateTime":"\/Date(1337049395000)\/",
          "Type":2,
          "Path":"http://wamsbayclus001rest-hs.net/52034bf6-dfae-4d83-aad3-3bd87dcb1a5d/",
          "AccessPolicyId":"nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8",
          "AssetId":"nb:cid:UUID:eb5540a2-116e-4d36-b084-7e9958f7f3c3",
          "StartTime":"\/Date(1337031395000)\/"
       }
    }

Ke streamování hladký přenos adresa URL origin v datových proudů media Playeru, musíte přidat cestu vlastnost s názvem hladký přenos souboru, následovaný manifestu "/ manifest".

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

Ke streamování HLS připojení (formát = m3u8 aapl) po "/ manifest".

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

Ke streamování MPEG POMLČKU, připojení (formát = mpd čas csf) po "/ manifest".

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)


## <a id="play"></a>Přehrávání obsahu  

Ke streamování videa, použijte [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

Testování postupné stahování, vložte adresu URL do prohlížeče (například aplikaci Internet Explorer, Chrome, Safari).


##<a name="next-steps-media-services-learning-paths"></a>Další kroky: Media Services naučné stezky

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="looking-for-something-else"></a>Hledáte něco jiného?

Toto téma neobsahoval, co jste očekávali, něco chybí, zda v jiným způsobem neměli vašim potřebám, zadejte nám svůj názor pomocí níže uvedených Disqus vlákna.



