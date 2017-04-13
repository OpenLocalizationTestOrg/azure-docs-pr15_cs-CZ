<properties 
    pageTitle="Šifrování obsahu s úložiště šifrování pomocí AMS REST API" 
    description="Naučte se zašifrovat obsah pomocí úložiště šifrování pomocí AMS REST API." 
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
    ms.date="09/26/2016"
    ms.author="juliako"/>


#<a name="encrypting-your-content-with-storage-encryption-using-ams-rest-api"></a>Šifrování obsahu s úložiště šifrování pomocí AMS REST API

Důrazně doporučujeme zašifrovat obsah místně pomocí šifrování AES 256 a pak ho nahrajte do úložiště Azure místa uložení zašifrovaných u ostatních.

Tento článek obsahuje přehled AMS úložiště šifrování a se dozvíte, jak nahrát obsah úložiště zašifrovaných:

- Vytvoření obsahu klíče.
- Vytvoření aktivum. Nastavte AssetCreationOption StorageEncryption při vytváření majetku.

     Šifrované materiálů musí být přidružené k obsahu klíče.
- Vazba klávesu obsahu na položku.  
- Nastavení šifrování související parametry na AssetFile entity.
 
>[AZURE.NOTE]Pokud chcete při ukládání šifrovaných materiálů, musíte nakonfigurovat zásady majetku doručení. Před vaší materiálů můžete streamují, serveru datových proudů odebere šifrování úložiště a přenáší datové proudy obsahu pomocí zásad zadaný doručení. Další informace najdete v tématu [Konfigurace zásad doručení materiálů](media-services-rest-configure-asset-delivery-policy.md).


>[AZURE.NOTE] Když pracujete s Media Services REST API, platí následující omezení:
>
>Při přístupu k entit v Media Services, musíte nastavit konkrétní záhlaví pole a hodnoty v žádostech HTTP. Další informace najdete v tématu [nastavení pro médií služby REST API vývoj](media-services-rest-how-to-use.md).

>Po úspěšném připojení k https://media.windows.net, zobrazí se 301 přesměrování zadání jiného identifikátor URI médií služeb. Musíte provést následující volání nových URI způsobem popsaným v části [připojování ke službě médií pomocí rozhraní REST API](media-services-rest-connect-programmatically.md). 

##<a name="storage-encryption-overview"></a>Přehled šifrování úložiště 

Šifrování úložiště AMS platí šifrování **AES PEV.cenu** režim pro celý soubor.  Režim AES PEV.cenu je blok šifrování zašifrovaná libovolné délky data bez nutnosti odsazení. Funguje zašifrováním blok čítač s algoritmus AES a XOR sestav výstup AES s daty do šifrovat.  Blokování čítač používá je vytvořen zkopírováním hodnoty InitializationVector bajtů 0 až 7 hodnota čítače a bajtů 8 až 15 hodnota čítače jsou nastavené na nulu. Bloku čítač 16 bajt bajtů 8 až 15 (tedy nejnižšímu bajtů) slouží jako znaménka jednoduché 64-bit, zvýší o jednu pro každý další blok dat zpracování a bude k dispozici v síťovém pořadí bajtů. Poznámka: Pokud této celé číslo dosáhne maximální hodnotu (0xFFFFFFFFFFFFFFFF) postupně ho obnovíte blok proti nula (bajtů 8 až 15) bez vlivu ostatní 64 bitů čítače (tedy bajtů 0 až 7).   Zajištění zabezpečení režimu šifrování AES PEV.cenu InitializationVector hodnota pro dané identifikátor klíč pro každý klíč obsahu musí být jedinečné pro každý soubor a soubory musí být menší než 2 ^ 64 bloky délce.  Toto je zajistit, že hodnota čítače nikdy opětovné použití dané klíčem. Další informace o režimu PEV.cenu podívejte se na [této stránce wikiwebu](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#CTR) (článek wikiwebu používá termín "Nonce" místo "InitializationVector").

Vymazat obsah místně pomocí šifrování AES 256 šifrování databáze pomocí **Šifrování úložiště** a pak ho nahrajte do úložiště Azure kde je uložena zašifrovat zbývající. Prostředky chráněné pomocí šifrování úložiště jsou automaticky bez šifrování umístí do systém zašifrovaných souborů před kódování a volitelně znovu zašifrovaných před odesláním zpět jako nový majetek výstupu. Případ použití primární šifrování úložiště je, když chcete zabezpečit vstupní mediální soubory kvalitní silné šifrování u ostatních na disku.

K provedení úložiště šifrované materiálů, musíte nakonfigurovat majetku doručení zásad, aby Media Services věděli, jak budete chtít doručení obsahu. Než vaše materiálů můžete streamují, serveru datových proudů odebere šifrování úložiště a přenáší datové proudy obsahu pomocí zásad dodací (například AES běžné šifrování a bez šifrování).

##<a name="create-contentkeys-used-for-encryption"></a>Vytvoření ContentKeys používá šifrování

Šifrované materiálů musí být přidružené k úložiště šifrovacího klíče. Klíč obsahu pro šifrování před vytvořením soubory materiálů musí vytvořit. Tato část popisuje jak vytvořit klíč obsahu.

Následují obecné kroky pro generování obsahu klíčů, které budete přidružovat materiálů, které chcete šifrovat. 

1. Šifrování úložiště náhodně Generovat klíč 32 8bajtový typ AES. 

    To bude klíč obsahu pro materiálů, což znamená, že všechny soubory přidružený k této materiálů budete potřebovat stejného klíče obsahu při dešifrování. 
2.  Zavolejte metody [GetProtectionKeyId](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkeyid) a [GetProtectionKey](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkey) získat správný certifikát X.509 použitý k šifrování klíč obsahu.
3.  Šifrování kód obsahu s veřejným klíčem certifikátu X.509. 

    Media Services .NET SDK používá RSA s OAEP při provádění šifrování.  Podívejte se na příklad .NET ve [EncryptSymmetricKeyData (funkce)](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).
4.  Vytvoření kontrolního hodnoty vypočítá z identifikátoru klíče a klíč obsahu. Následující příklad .NET vypočítá kontrolního pomocí části GUID identifikátor klíče a zrušte zaškrtnutí políčka obsahu klíče.
    

        public static string CalculateChecksum(byte[] contentKey, Guid keyId)
        {
            const int ChecksumLength = 8;
            const int KeyIdLength = 16;

            byte[] encryptedKeyId = null;

            // Checksum is computed by AES-ECB encrypting the KID
            // with the content key.
            using (AesCryptoServiceProvider rijndael = new AesCryptoServiceProvider())
            {
                rijndael.Mode = CipherMode.ECB;
                rijndael.Key = contentKey;
                rijndael.Padding = PaddingMode.None;

                ICryptoTransform encryptor = rijndael.CreateEncryptor();
                encryptedKeyId = new byte[KeyIdLength];
                encryptor.TransformBlock(keyId.ToByteArray(), 0, KeyIdLength, encryptedKeyId, 0);
            }

            byte[] retVal = new byte[ChecksumLength];
            Array.Copy(encryptedKeyId, retVal, ChecksumLength);

            return Convert.ToBase64String(retVal);
        }


5. Vytvoření klávesu obsahu s **EncryptedContentKey** (převést na kódováním Base 64 řetězec), **ProtectionKeyId**, **ProtectionKeyType**, **ContentKeyType**a **kontrolního** hodnoty, které jste přijali v předchozích krocích.

    
    Šifrování úložiště budou zahrnuty následující vlastnosti by měly být v hlavním textu žádosti o.
     
    Žádost o vlastnosti body   | Popis
    ---|---
    ID | ContentKey Id, které jsme generovat označována ve formátu následující "nb:kid:UUID:<NEW GUID>".
    ContentKeyType | Toto je typ obsahu klíčové jako celé číslo tohoto obsahu klíče. Nám předat hodnota 1 pro šifrování úložiště.
    EncryptedContentKey | Vytvoření nové hodnotu obsahu klíče, což je hodnota 256-bit (32 bajtů). Klíč musí být zašifrovaný pomocí certifikátu X.509 úložiště šifrování, která získáme z Microsoft Azure Media Services spuštěním žádost HTTP GET GetProtectionKeyId a GetProtectionKey metody. Jako příklad, viz následující kód .NET: metodu **EncryptSymmetricKeyData** definované [tady](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).
    ProtectionKeyId | Toto je id klíče ochranu pro úložiště šifrování X.509 certifikát, který byl používaný k šifrování naše klíč obsahu.
    ProtectionKeyType | Toto je typ šifrování pro ochranu klávesy, která byla použita k šifrování klávesu obsahu. Tato hodnota je StorageEncryption(1) našem příkladu.
    Kontrolní součet |Kontrolního počítané MD5 klávesu obsahu. Výpočet je zašifrováním obsah Id obsahu klíčem. Příklad ukazuje, jak vypočítat kontrolní součet.
    

###<a name="retrieve-the-protectionkeyid"></a>Načtení ProtectionKeyId 
 

Následující příklad ukazuje, jak načíst ProtectionKeyId miniaturu certifikátu, certifikátu, který se musí používat při šifrování klíč obsahu. Provést tento krok, abyste měli jistotu, že již máte příslušný certifikát v počítači.


Žádost o:
    
    
    GET https://media.windows.net/api/GetProtectionKeyId?contentKeyType=0 HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.11
    Host: media.windows.net
    

Odpověď:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 139
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 2b6aa7a4-3a09-4b08-b581-26b55667f817
    x-ms-request-id: 2b6aa7a4-3a09-4b08-b581-26b55667f817
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 04 Feb 2015 02:42:52 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Edm.String","value":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C"}

###<a name="retrieve-the-protectionkey-for-the-protectionkeyid"></a>Obnovení ProtectionKey pro ProtectionKeyId

Následující příklad ukazuje, jak získat X.509 certifikát pomocí ProtectionKeyId, že jste dostali v předchozím kroku.

Žádost o:
        
    GET https://media.windows.net/api/GetProtectionKey?ProtectionKeyId='7D9BB04D9D0A4A24800CADBFEF232689E048F69C' HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    Host: media.windows.net
    


Odpověď:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 1227
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    request-id: 1523e8f3-8ed2-40fe-8a9a-5d81eb572cc8
    x-ms-request-id: 1523e8f3-8ed2-40fe-8a9a-5d81eb572cc8
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Thu, 05 Feb 2015 07:52:30 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Edm.String",
    "value":"MIIDSTCCAjGgAwIBAgIQqf92wku/HLJGCbMAU8GEnDANBgkqhkiG9w0BAQQFADAuMSwwKgYDVQQDEyN3YW1zYmx1cmVnMDAxZW5jcnlwdGFsbHNlY3JldHMtY2VydDAeFw0xMjA1MjkwNzAwMDBaFw0zMjA1MjkwNzAwMDBaMC4xLDAqBgNVBAMTI3dhbXNibHVyZWcwMDFlbmNyeXB0YWxsc2VjcmV0cy1jZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAzR0SEbXefvUjb9wCUfkEiKtGQ5Gc328qFPrhMjSo+YHe0AVviZ9YaxPPb0m1AaaRV4dqWpST2+JtDhLOmGpWmmA60tbATJDdmRzKi2eYAyhhE76MgJgL3myCQLP42jDusWXWSMabui3/tMDQs+zfi1sJ4Ch/lm5EvksYsu6o8sCv29VRwxfDLJPBy2NlbV4GbWz5Qxp2tAmHoROnfaRhwp6WIbquk69tEtu2U50CpPN2goLAqx2PpXAqA+prxCZYGTHqfmFJEKtZHhizVBTFPGS3ncfnQC9QIEwFbPw6E5PO5yNaB68radWsp5uvDg33G1i8IT39GstMW6zaaG7cNQIDAQABo2MwYTBfBgNVHQEEWDBWgBCOGT2hPhsvQioZimw8M+jOoTAwLjEsMCoGA1UEAxMjd2Ftc2JsdXJlZzAwMWVuY3J5cHRhbGxzZWNyZXRzLWNlcnSCEKn/dsJLvxyyRgmzAFPBhJwwDQYJKoZIhvcNAQEEBQADggEBABcrQPma2ekNS3Wc5wGXL/aHyQaQRwFGymnUJ+VR8jVUZaC/U/f6lR98eTlwycjVwRL7D15BfClGEHw66QdHejaViJCjbEIJJ3p2c9fzBKhjLhzB3VVNiLIaH6RSI1bMPd2eddSCqhDIn3VBN605GcYXMzhYp+YA6g9+YMNeS1b+LxX3fqixMQIxSHOLFZ1G/H2xfNawv0VikH3djNui3EKT1w/8aRkUv/AAV0b3rYkP/jA1I0CPn0XFk7STYoiJ3gJoKq9EMXhit+Iwfz0sMkfhWG12/XO+TAWqsK1ZxEjuC9OzrY7pFnNxs4Mu4S8iinehduSpY+9mDd3dHynNwT4="}

### <a name="create-the-content-key"></a>Vytvoření obsahu klíče 

Po načtené certifikátu X.509 a jeho veřejný klíč používaný k šifrování klíč obsahu, vytvořte **ContentKey** entita a nastavte jeho nemovitostí s hodnotou příslušným způsobem.

Jedna z hodnot, že je nutné nastavit při vytváření obsahu je klíčem požadovaný typ. V případě šifrovací úložiště hodnotu "1". 

Následující příklad ukazuje, jak vytvořit **ContentKey** sadou **ContentKeyType** šifrování úložiště ("1") a **ProtectionKeyType** hodnotu "0", který označuje, že klíče ochrany Id miniaturu X.509 certifikátu.  


Žádost o

    POST https://media.windows.net/api/ContentKeys HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.11
    Host: media.windows.net
    {
    "Name":"ContentKey",
    "ProtectionKeyId":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C", 
    "ContentKeyType":"1", 
    "ProtectionKeyType":"0",
    "EncryptedContentKey":"your encrypted content key",
    "Checksum":"calculated checksum"
    }


Odpověď:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 777
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/ContentKeys('nb%3Akid%3AUUID%3A9c8ea9c6-52bd-4232-8a43-8e43d8564a99')
    Server: Microsoft-IIS/8.5
    request-id: 76e85e0f-5cf1-44cb-b689-b3455888682c
    x-ms-request-id: 76e85e0f-5cf1-44cb-b689-b3455888682c
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 04 Feb 2015 02:37:46 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#ContentKeys/@Element",
    "Id":"nb:kid:UUID:9c8ea9c6-52bd-4232-8a43-8e43d8564a99","Created":"2015-02-04T02:37:46.9684379Z",
    "LastModified":"2015-02-04T02:37:46.9684379Z",
    "ContentKeyType":1,
    "EncryptedContentKey":"your encrypted content key",
    "Name":"ContentKey",
    "ProtectionKeyId":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C",
    "ProtectionKeyType":0,
    "Checksum":"calculated checksum"}

## <a name="create-an-asset"></a>Vytvoření aktivum

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
    
    {"Name":"BigBuckBunny" "Options":1}


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
       "Options":1,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }
    
##<a name="associate-the-contentkey-with-an-asset"></a>ContentKey přidružit aktivum

Po vytvoření ContentKey přidružte vaší materiálů $links myší, jak je vidět v následujícím příkladu:
    
Žádost o:
    
    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Afbd7ce05-1087-401b-aaae-29f16383c801')/$links/ContentKeys HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.11
    Host: media.windows.net

    
    {"uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeys('nb%3Akid%3AUUID%3A01e6ea36-2285-4562-91f1-82c45736047c')"}

Odpověď:

    HTTP/1.1 204 No Content 

##<a name="create-an-assetfile"></a>Vytvoření AssetFile

Entita [AssetFile](http://msdn.microsoft.com/library/azure/hh974275.aspx) představuje videoklipu nebo zvukového souboru, který je uložený v kontejneru objektů blob. Soubor materiálů je vždy přidružené k aktivum a aktivum může obsahovat jedno nebo více souborů materiálů. Úkol Media Encoder služby nezdaří, pokud soubor objektu materiálů je přidružená k souboru digitální v kontejneru objektů blob.

Všimněte si, že **AssetFile** instance a skutečné multimediálního souboru dva odlišné objekty. AssetFile instance obsahuje metadata multimediálního souboru, když mediální soubor s obsahem skutečné mediální.

Po nahrání souboru digitálních médií do kontejneru objektů blob bude používat žádost HTTP **SLOUČENÍ** aktualizovat AssetFile s informacemi o souboru médií (není vidět v tomto tématu). 

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
       "IsEncrypted":"true",
       "EncryptionScheme" : "StorageEncryption", 
       "EncryptionVersion" : "1.0",       
       "EncryptionKeyId" : "nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510",
       "InitializationVector" : "397304628502661816</d:InitializationVector",
       "Options":0,
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
       "EncryptionVersion": "1.0",
       "EncryptionScheme": "StorageEncryption",
       "IsEncrypted":true,
       "EncryptionKeyId":"nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510",
       "InitializationVector":"397304628502661816</d:InitializationVector",
       "IsPrimary":false,
       "LastModified":"2015-01-19T00:34:08.1934137Z",
       "Created":"2015-01-19T00:34:08.1934137Z",
       "MimeType":"video/mp4",
       "ContentChecksum":null
    }
