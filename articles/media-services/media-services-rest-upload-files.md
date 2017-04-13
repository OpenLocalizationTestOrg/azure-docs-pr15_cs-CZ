<properties 
    pageTitle="Nahrajte soubory do účtu Media Services pomocí ZBÝVAJÍCÍ | Microsoft Azure" 
    description="Zjistěte, jak získat mediální obsah do Media Services – vytvoření a odeslání prostředky." 
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
    ms.date="09/19/2016"
    ms.author="juliako"/>


# <a name="upload-files-into-a-media-services-account-using-rest"></a>Nahrajte soubory do účtu Media Services pomocí REST

 > [AZURE.SELECTOR]
 - [.NET](media-services-dotnet-upload-files.md)
 - [ZBÝVAJÍCÍ](media-services-rest-upload-files.md)
 - [Portál](media-services-portal-upload-files.md)

V dialogovém okně médií služby nahrajte svoje soubory digitální do aktivum. Entita [materiálů](https://msdn.microsoft.com/library/azure/hh974277.aspx) může obsahovat video, zvuk, obrázky, miniatur kolekce, text skladeb a titulků soubory (a metadata o těchto souborech.)  Jakmile soubory jsou odeslány do majetek, obsah uložena bezpečně v cloudu pro další zpracování a přenos. 

>[AZURE.NOTE]Při výběru název souboru materiálů platí následující omezení:
>
>- Media Services použije hodnotu vlastnost IAssetFile.Name při vytváření adresy URL pro streamování obsahu (například http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Z tohoto důvodu heslo percent-encoding není povolená. Hodnoty **název** vlastnosti nemůže obsahovat žádný z následujících [znaků procent kódování vyhrazené](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Navíc může být pouze jednu "." pro přípona názvu souboru.
>
>- Délka názvu nesmí být větší než 260 znaků.

Základní pracovního postupu pro odeslání prostředky se dělí na následující části:

- Vytvoření aktivum
- Šifrování aktiva (volitelné)
- Odeslání souboru k úložišti objektů blob

AMS umožňuje nahrát prostředky hromadně. Další informace najdete v tématu [v této](media-services-rest-upload-files.md#upload_in_bulk) části.

##<a name="upload-assets"></a>Nahrání prostředky

###<a name="create-an-asset"></a>Vytvoření aktivum

>[AZURE.NOTE] Když pracujete s Media Services REST API, platí následující omezení:
>
>Při přístupu k entit v Media Services, musíte nastavit konkrétní záhlaví pole a hodnoty v žádostech HTTP. Další informace najdete v tématu [nastavení pro médií služby REST API vývoj](media-services-rest-how-to-use.md).

>Po úspěšném připojení k https://media.windows.net, zobrazí se 301 přesměrování zadání jiného identifikátor URI médií služeb. Musíte provést následující volání nových URI způsobem popsaným v části [připojování ke službě médií pomocí rozhraní REST API](media-services-rest-connect-programmatically.md). 
 
Aktivum je kontejner pro více typů nebo sady objektů v Media Services, včetně video, zvuk, obrázky, miniatur kolekce, skladeb text a titulků soubory. Vytvoření aktivum v rozhraní REST API vyžaduje poslat žádost o příspěvek Media Services a umístění vlastnost informace o vaší materiálů v hlavním textu žádosti o.

Vlastnosti, které můžete použít při vytváření aktivum reprodukujte **Možnosti**. **Možnosti** je hodnota výčtu, která jsou uvedeny dostupné šifrování možnosti, které aktivum můžete vytvořené pomocí. Platná hodnota je taková hodnot v seznamu pod není kombinací hodnot. 

- **Žádná** = **0**: bez šifrování se použijí. Toto je výchozí hodnota. Všimněte si, že použijete tuto možnost obsahu není zamčený při přenosu šifrovaná nebo u ostatních v úložišti.
    Pokud budete chtít předvádění MP4 pomocí postupného stahování použijte tuto možnost. 

- **StorageEncrypted** = **1**: Určete, jestli se má soubory šifrování s šifrování AES 256 pro odesílání a úložiště.

    -Li vaše materiálů úložiště zašifrovaných, musíte nakonfigurovat zásady doručení materiálů. Další informace najdete v článku [Konfigurace zásad doručení materiálů](media-services-rest-configure-asset-delivery-policy.md).

- **CommonEncryptionProtected** = **2**: Určete, pokud ukládáte soubory chráněné pomocí společná metoda šifrování (například PlayReady). 

- **EnvelopeEncryptionProtected** = **4**: Určete, pokud ukládáte HLS zašifrovaných souborů AES. Všimněte si, že soubory, které musí být kódování a zašifrovaných transformace správce.

>[AZURE.NOTE]Pokud vaše materiálů použije šifrování, musíte vytvořit **ContentKey** a odkaz na svůj materiálů podle popisu v tématu:[Postup vytvoření ContentKey](media-services-rest-create-contentkey.md). Poznámka: po nahrajte soubory do majetku budete muset aktualizovat vlastnosti šifrování entity **AssetFile** s hodnotami, které jste získali při šifrování **materiálů** . To udělejte pomocí žádost HTTP **Sloučit** . 


Následující příklad ukazuje, jak vytvořit aktivum.

**Žádost HTTP**

    POST https://media.windows.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    
    {"Name":"BigBuckBunny.mp4"}
    

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
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny.mp4",
       "Options":0,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }
    
###<a name="create-an-assetfile"></a>Vytvoření AssetFile

Entita [AssetFile](http://msdn.microsoft.com/library/azure/hh974275.aspx) představuje videoklipu nebo zvukového souboru, který je uložený v kontejneru objektů blob. Soubor materiálů je vždy přidružené k aktivum a aktivum může obsahovat jedno nebo více souborů materiálů. Úkol Media Encoder služby nezdaří, pokud soubor objektu materiálů je přidružená k souboru digitální v kontejneru objektů blob.

Všimněte si, že **AssetFile** instance a skutečné multimediálního souboru dva odlišné objekty. AssetFile instance obsahuje metadata multimediálního souboru, když mediální soubor s obsahem skutečné mediální.

Po nahrání souboru digitálních médií do kontejneru objektů blob bude používat žádost HTTP **SLOUČENÍ** aktualizovat AssetFile s informacemi o váš soubor médií (znázorněná na pozdější v tématu). 

**Žádost HTTP**

    POST https://media.windows.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
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

Před nahráním všech souborů v úložišti objektů blob, nastavte přístup práva zásad pro psaní aktivum. Aby je dostala, ZVEŘEJŇUJÍ žádost HTTP na sadu AccessPolicies entity. Definování doba trvání v minutách hodnoty při vytváření nebo obdržíte zprávu 500 Chyba interním serveru zpět odpověď. Další informace o AccessPolicies najdete v tématu [AccessPolicy](http://msdn.microsoft.com/library/azure/hh974297.aspx).

Následující příklad ukazuje, jak vytvořit AccessPolicy:
        
**Žádost HTTP**

    POST https://media.windows.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    
    {"Name":"NewUploadPolicy", "DurationInMinutes":"440", "Permissions":"2"} 

**Žádost HTTP**

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

###<a name="get-the-upload-url"></a>Získání adresy URL nahrát

Získat adresu URL skutečné nahrát, vytvořte Locator přidružení zabezpečení. Locator definovat počáteční čas a typ koncového bodu připojení klientů, kteří mají přístup k souborům ve aktivum. Můžete vytvořit více entity URL pro daný AccessPolicy a materiálů pár zpracovávat žádosti o jiného klienta a potřeb. Každý z těchto Locator pomocí hodnotu čas spuštění plus doba trvání v minutách hodnotu AccessPolicy určit dobu, kterou lze použít adresy URL. Další informace najdete v tématu [Locator](http://msdn.microsoft.com/library/azure/hh974308.aspx).


Adresa URL přidružení zabezpečení má v tomto formátu:

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

Některé aspekty, které platí:

- Nesmí obsahovat více než pět jedinečné Locator přidružený k dané materiálů najednou. Další informace najdete v tématu Locator.
- Pokud potřebujete okamžitě nahrajte svoje soubory, by měl nastavíte čas spuštění hodnotu pět minut, než aktuální čas. Je to proto může existovat hodiny skew mezi klientském počítači a Media Services. Také, váš čas spuštění hodnota musí být v v tomto formátu data a času: YYYY-MM-DDTHH:mm:ssZ (například "2014-05-23T17:53:50Z").   
- Doména může obsahovat podruhé 30-40 zpozdit po Locator k při vznikne je k dispozici. Týká se adresa URL přidružení zabezpečení a Locator Origin.

Následující příklad ukazuje, jak vytvořit URL adresy URL přidružení zabezpečení, způsobem definovaným ve vlastnosti typ v hlavním textu žádosti o ("1" pro locator přidružení zabezpečení) a "2" pro locator origin na vyžádání. Vlastnost **Path** vrácené obsahuje adresu URL, kterou musíte pomocí nahrajte soubor.
    
**Žádost HTTP**
    
    POST https://media.windows.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
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
    
    MERGE https://media.windows.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5') HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net
    
    {  
       "ContentFileSize":"1186540",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**Odpověď HTTP**

Pokud úspěšné, následující vrácena: protokolu HTTP/1.1 204 bez obsahu

### <a name="delete-the-locator-and-accesspolicy"></a>Odstranění Locator a AccessPolicy 

**Žádost HTTP**


    DELETE https://media.windows.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net

    
**Odpověď HTTP**

Pokud je úspěšná, je vrácena takto:

    HTTP/1.1 204 No Content 
    ...

**Žádost HTTP**

    DELETE https://media.windows.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net

**Odpověď HTTP**

Pokud je úspěšná, je vrácena takto:

    HTTP/1.1 204 No Content 
    ...

##<a id="upload_in_bulk"></a>Nahrání prostředky hromadně

###<a name="create-the-ingestmanifest"></a>Vytvoření IngestManifest

IngestManifest je kontejner pro sadu prostředky, soubory materiálů a statistické informace, které lze použít k určení průběhu hromadné ingesting pro sadu.


**Žádost HTTP**

    POST https:// media.windows.net/API/IngestManifests HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 36
    Expect: 100-continue
    
    { "Name" : "ExampleManifestREST" }

###<a name="create-assets"></a>Vytvořit prostředky

Před vytvořením IngestManifestAsset, je potřeba vytvořit materiálů, která bude dokončena pomocí hromadného ingesting. Aktivum je kontejner pro více typů nebo sady objektů v Media Services, včetně video, zvuk, obrázky, miniatur kolekce, skladeb text a titulků soubory. Vytvoření aktivum v rozhraní REST API vyžaduje poslat žádost HTTP POST Microsoft Azure Media Services a umístění vlastnost informace o vaší materiálů v hlavním textu žádosti o. V tomto příkladu je vytvořen majetku pomocí možnosti StorageEncrption(1) zahrnutý v sadě požadavku.

**Odpověď HTTP**

    POST https://media.windows.net/API/Assets HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 55
    Expect: 100-continue
    
    { "Name" : "ExampleManifestREST_Asset", "Options" : 1 }

###<a name="create-the-ingestmanifestassets"></a>Vytvoření IngestManifestAssets

IngestManifestAssets představují majetku v rámci IngestManifest, které se používají s ingesting hromadně. V podstatě propojit majetku manifest. Azure Media Services sleduje interně nahrávání souboru podle IngestManifestFiles kolekce přidružené k IngestManifestAsset. Po nahrání tyto soubory majetku dokončen. Můžete vytvořit nový IngestManifestAsset s žádost HTTP příspěvek. V hlavním textu žádosti o zahrnout IngestManifest Id a Id materiálů, které IngestManifestAsset by měl vzájemném propojování pro hromadné ingesting.

**Odpověď HTTP**

    POST https://media.windows.net/API/IngestManifestAssets HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 152
    Expect: 100-continue
    { "ParentIngestManifestId" : "nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048", "Asset" : { "Id" : "nb:cid:UUID:b757929a-5a57-430b-b33e-c05c6cbef02e"}}


###<a name="create-the-ingestmanifestfiles-for-each-asset"></a>Vytvoření IngestManifestFiles pro každou materiálů

IngestManifestFile představuje skutečný videoklipu nebo zvukového objektů blob objekt, který bude možné odeslat jako součást hromadné ingesting majetku. Šifrování související vlastnosti není povinný, pokud majetku používá šifrování možnost. Příklad použitý v této části ukazuje vytvoření IngestManifestFile, který používá StorageEncryption položka dřív vytvořili.


**Odpověď HTTP**

    POST https://media.windows.net/API/IngestManifestFiles HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 367
    Expect: 100-continue
    
    { "Name" : "REST_Example_File.wmv", "ParentIngestManifestId" : "nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048", "ParentIngestManifestAssetId" : "nb:maid:UUID:beed8531-9a03-9043-b1d8-6a6d1044cdda", "IsEncrypted" : "true", "EncryptionScheme" : "StorageEncryption", "EncryptionVersion" : "1.0", "EncryptionKeyId" : "nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510" }
    
###<a name="upload-the-files-to-blob-storage"></a>Nahrajte soubory do úložiště objektů Blob

Můžete použít libovolné vysokorychlostní klientské aplikace, může nahrávání souborů materiálů do kontejneru úložiště objektů blob Uri poskytovanou vlastnost BlobStorageUriForUpload IngestManifest. Jedna důležitá vysokorychlostní nahrát služba je [Aspera na vyžádání aplikace Azure](http://go.microsoft.com/fwlink/?LinkId=272001).

###<a name="monitor-bulk-ingest-progress"></a>Sledování hromadné jedí průběhu

Můžete sledovat průběh hromadné ingesting operace pro IngestManifest tak, že dotazování vlastnost statistiky IngestManifest. Vlastnost je komplexní typ [IngestManifestStatistics](https://msdn.microsoft.com/library/azure/jj853027.aspx). Dotazování vlastnost statistiky, odešlete žádost HTTP GET předáním IngestManifest.
 

##<a name="create-contentkeys-used-for-encryption"></a>Vytvoření ContentKeys pro šifrování

Pokud vaše materiálů použije šifrování, musíte vytvořit ContentKey pro šifrování před vytvořením soubory materiálů. Šifrování úložiště budou zahrnuty následující vlastnosti by měly být v hlavním textu žádosti o.
 
Žádost o vlastnosti body   | Popis
---|---
ID | ContentKey Id, které jsme generovat označována ve formátu následující "nb:kid:UUID:<NEW GUID>".
ContentKeyType | Toto je typ obsahu klíčové jako celé číslo tohoto obsahu klíče. Nám předat hodnota 1 pro šifrování úložiště.
EncryptedContentKey | Vytvoření nové hodnotu obsahu klíče, což je hodnota 256-bit (32 bajtů). Klíč musí být zašifrovaný pomocí certifikátu X.509 úložiště šifrování, která získáme z Microsoft Azure Media Services spuštěním žádost HTTP GET GetProtectionKeyId a GetProtectionKey metody.
ProtectionKeyId | Toto je id klíče ochranu pro úložiště šifrování X.509 certifikát, který byl používaný k šifrování naše klíč obsahu.
ProtectionKeyType | Toto je typ šifrování pro ochranu klávesy, která byla použita k šifrování klávesu obsahu. Tato hodnota je StorageEncryption(1) našem příkladu.
Kontrolní součet |Kontrolního počítané MD5 klávesu obsahu. Výpočet je zašifrováním obsah Id obsahu klíčem. Příklad ukazuje, jak vypočítat kontrolní součet.


**Odpověď HTTP**
    
    POST https://media.windows.net/api/ContentKeys HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 572
    Expect: 100-continue
    
    {"Id" : "nb:kid:UUID:316d14d4-b603-4d90-b8db-0fede8aa48f8", "ContentKeyType" : 1, "EncryptedContentKey" : "Y4NPej7heOFa2vsd8ZEOcjjpu/qOq3RJ6GRfxa8CCwtAM83d6J2mKOeQFUmMyVXUSsBCCOdufmieTKi+hOUtNAbyNM4lY4AXI537b9GaY8oSeje0NGU8+QCOuf7jGdRac5B9uIk7WwD76RAJnqyep6U/OdvQV4RLvvZ9w7nO4bY8RHaUaLxC2u4aIRRaZtLu5rm8GKBPy87OzQVXNgnLM01I8s3Z4wJ3i7jXqkknDy4VkIyLBSQvIvUzxYHeNdMVWDmS+jPN9ScVmolUwGzH1A23td8UWFHOjTjXHLjNm5Yq+7MIOoaxeMlKPYXRFKofRY8Qh5o5tqvycSAJ9KUqfg==", "ProtectionKeyId" : "7D9BB04D9D0A4A24800CADBFEF232689E048F69C", "ProtectionKeyType" : 1, "Checksum" : "TfXtjCIlq1Y=" }

### <a name="link-the-contentkey-to-the-asset"></a>Odkaz ContentKey majetku

ContentKey souvisí s jeden nebo více prostředky tak, že pošlete žádost HTTP příspěvek. Následující žádost o je příklad propojíte příklad ContentKey materiálů příklad podle ID.

**Odpověď HTTP**
    
    POST https://media.windows.net/API/Assets('nb:cid:UUID:b3023475-09b4-4647-9d6d-6fc242822e68')/$links/ContentKeys HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 113
    Expect: 100-continue
    
    { "uri": "https://media.windows.net/api/ContentKeys('nb%3Akid%3AUUID%3A32e6efaf-5fba-4538-b115-9d1cefe43510')"}

**Odpověď HTTP**

    GET https://media.windows.net/API/IngestManifests('nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net



##<a name="next-step"></a>Další krok

Prohlédněte si Media Services naučné stezky.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



 
[How to Get a Media Processor]: media-services-get-media-processor.md
 
