<properties
 pageTitle="Plánovač ověřování odchozích připojení"
 description="Plánovač ověřování odchozích připojení"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/15/2016"
 ms.author="deli"/>

# <a name="scheduler-outbound-authentication"></a>Plánovač ověřování odchozích připojení

Plánovač úloh muset volat na služby, které vyžadují ověřování. Tímto způsobem se nazývá služby můžete určit, pokud Plánovač úloh můžete přistupovat k prostředkům. Některé z těchto služeb zahrnovat další služby Azure Salesforce.com, Facebook a zabezpečené vlastní weby.

## <a name="adding-and-removing-authentication"></a>Přidávání a odebírání ověřování

Přidání ověřování Plánovač projektu je jednoduché – přidání podřízený prvek JSON `authentication` k `request` prvek při vytváření nebo aktualizaci projektu. Tajemství předán Plánovač v umístění, oprava nebo příspěvku žádost o – jako součást `authentication` objekt – nikdy vracejí v odpovědi. V odpovědích, tajné informace nastavena na hodnotu null nebo pravděpodobně veřejné token, který představuje ověřené entity.

Pokud chcete odebrat ověřování, umístění nebo oprava úlohy explicitně, nastavení `authentication` objektu na hodnotu null. Neuvidíte žádné ověřování vlastnosti zpět v odpovědi.

V současné době jsou pouze podporovaných typů ověřování `ClientCertificate` model (pro použití klientské certifikáty SSL/TLS), `Basic` model (pro základní ověřování) a `ActiveDirectoryOAuth` model (pro ověřování Active Directory OAuth.)

## <a name="request-body-for-clientcertificate-authentication"></a>Požadavku ClientCertificate ověřování

Při přidávání pomocí ověřování `ClientCertificate` model, zadejte tyto další prvky v hlavním textu žádosti o.  

|Element|Popis|
|:---|:---|
|_ověřování (nadřazený prvek)_|Ověřování objektu pro používání klientský certifikát SSL.|
|_Typ_|Povinné. Typ ověřování. Certifikáty SSL klienta, musí být hodnota `ClientCertificate`.|
|_PFX_|Povinné. Kódováním Base 64 obsah souboru PFX.|
|_heslo_|Povinné. Heslo pro přístup k souboru PFX.|


## <a name="response-body-for-clientcertificate-authentication"></a>Fórum odpovědí pro ClientCertificate ověřování

Při odesílání do žádosti o s informacemi o stavu ověření odpověď obsahuje následující elementy související s ověřování.

|Element |Popis |
|:--|:--|
|_ověřování (nadřazený prvek)_ |Ověřování objektu pro používání klientský certifikát SSL.|
|_Typ_ |Typ ověřování. Certifikáty SSL klienta, je hodnota `ClientCertificate`.|
|_Miniatura certifikátu_ |Miniatura certifikátu.|
|_certificateSubjectName_ |Rozlišující název subjektu certifikátu.|
|_certificateExpiration_ |Datum vypršení platnosti certifikátu.|

## <a name="sample-rest-request-for-clientcertificate-authentication"></a>Ukázka ZBÝVAJÍCÍ žádost o ClientCertificate ověřování

```
PUT https://management.azure.com/subscriptions/1fe0abdf-581e-4dfe-9ec7-e5cb8e7b205e/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "type": "clientcertificate",
          "password": "password",
          "pfx": "pfx key"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-clientcertificate-authentication"></a>Ukázka ZBÝVAJÍCÍ odpověď pro ClientCertificate ověřování

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 858
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: 56c7b40e-721a-437e-88e6-f68562a73aa8
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 1075219e-e879-4030-bc81-094e54fbabce
x-ms-routing-request-id: WESTUS:20160316T190424Z:1075219e-e879-4030-bc81-094e54fbabce
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:04:23 GMT

{
  "id": "/subscriptions/1fe0abdf-581e-4dfe-9ec7-e5cb8e7b205e/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
  "type": "Microsoft.Scheduler/jobCollections/jobs",
  "name": "southeastasiajc/httpjob",
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "certificateThumbprint": "88105CG9DF9ADE75B835711D899296CB217D7055",
          "certificateExpiration": "2021-01-01T07:00:00Z",
          "certificateSubjectName": "CN=Scheduler Mgmt",
          "type": "ClientCertificate"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
    "status": {
      "nextExecutionTime": "2016-03-16T19:05:00Z",
      "executionCount": 0,
      "failureCount": 0,
      "faultedCount": 0
    }
  }
}
```

## <a name="request-body-for-basic-authentication"></a>Žádost o textu pro základní ověřování

Při přidávání pomocí ověřování `Basic` model, zadejte tyto další prvky v hlavním textu žádosti o.

|Element|Popis|
|:--|:--|
|_ověřování (nadřazený prvek)_ |Ověřování objektu pro používání základní ověřování.|
|_Typ_ |Povinné. Typ ověřování. Základní ověřování, musí být hodnota `Basic`.|
|_uživatelské jméno_ |Povinné. Uživatelské jméno k ověření.|
|_heslo_ |Povinné. Heslo k ověření.|

## <a name="response-body-for-basic-authentication"></a>Fórum odpovědí pro základní ověřování

Při odesílání do žádosti o s informacemi o stavu ověření odpověď obsahuje následující elementy související s ověřování.

|Element|Popis|
|:--|:--|
|_ověřování (nadřazený prvek)_ |Ověřování objektu pro používání základní ověřování.|
|_Typ_ |Typ ověřování. Základní ověřování, je hodnota `Basic`.|
|_uživatelské jméno_ |Ověření uživatelské jméno.|

## <a name="sample-rest-request-for-basic-authentication"></a>Ukázka ZBÝVAJÍCÍ požadavku na základní ověřování

```
PUT https://management.azure.com/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Length: 562
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "type": "basic",
          "username": "user",
          "password": "password"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-basic-authentication"></a>Ukázka ZBÝVAJÍCÍ odpověď pro základní ověřování

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 701
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: a2dcb9cd-1aea-4887-8893-d81273a8cf04
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 7816f222-6ea7-468d-b919-e6ddebbd7e95
x-ms-routing-request-id: WESTUS:20160316T190506Z:7816f222-6ea7-468d-b919-e6ddebbd7e95
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:05:06 GMT

{  
   "id":"/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
   "type":"Microsoft.Scheduler/jobCollections/jobs",
   "name":"southeastasiajc/httpjob",
   "properties":{  
      "startTime":"2015-05-14T14:10:00Z",
      "action":{  
         "request":{  
            "uri":"https://mywebserviceendpoint.com",
            "method":"GET",
            "headers":{  
               "x-ms-version":"2013-03-01"
            },
            "authentication":{  
               "username":"user1",
               "type":"Basic"
            }
         },
         "type":"http"
      },
      "recurrence":{  
         "frequency":"minute",
         "endTime":"2016-04-10T08:00:00Z",
         "interval":1
      },
      "state":"enabled",
      "status":{  
         "nextExecutionTime":"2016-03-16T19:06:00Z",
         "executionCount":0,
         "failureCount":0,
         "faultedCount":0
      }
   }
}
```

## <a name="request-body-for-activedirectoryoauth-authentication"></a>Požadavku ActiveDirectoryOAuth ověřování

Při přidávání pomocí ověřování `ActiveDirectoryOAuth` model, zadejte tyto další prvky v hlavním textu žádosti o.

|Element |Popis |
|:--|:--|
|_ověřování (nadřazený prvek)_ |Ověřování objektu pro používání ActiveDirectoryOAuth ověřování.|
|_Typ_ |Povinné. Typ ověřování. Ověřování ActiveDirectoryOAuth hodnota musí být `ActiveDirectoryOAuth`.|
|_klienta_ |Povinné. Identifikátor klienta Azure AD klienta.|
|_cílové skupiny_ |Povinné. To je nastavena na https://management.core.windows.net/.|
|_clientId_ |Povinné. Aplikaci Azure AD identifikátor klienta.|
|_tajná_ |Povinné. Tajná klienta, který požaduje tokenu.|

### <a name="determining-your-tenant-identifier"></a>Určení identifikátor klienta

Identifikátor klienta o najdete klienta Azure AD spuštěním `Get-AzureAccount` v prostředí PowerShell Azure.

## <a name="response-body-for-activedirectoryoauth-authentication"></a>Fórum odpovědí pro ActiveDirectoryOAuth ověřování

Při odesílání do žádosti o s informacemi o stavu ověření odpověď obsahuje následující elementy související s ověřování.

|Element |Popis |
|:--|:--|
|_ověřování (nadřazený prvek)_ |Ověřování objektu pro používání ActiveDirectoryOAuth ověřování.|
|_Typ_ |Typ ověřování. ActiveDirectoryOAuth ověřování, je hodnota `ActiveDirectoryOAuth`.|
|_klienta_ |Identifikátor klienta Azure AD klienta. |
|_cílové skupiny_ |To je nastavena na https://management.core.windows.net/.|
|_clientId_ |Identifikátor klientské aplikaci Azure AD.|

## <a name="sample-rest-request-for-activedirectoryoauth-authentication"></a>Ukázka ZBÝVAJÍCÍ žádost o ActiveDirectoryOAuth ověřování

```
PUT https://management.azure.com/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Length: 757
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "tenant":"microsoft.onmicrosoft.com",
          "audience":"https://management.core.windows.net/",
          "clientId":"dc23e764-9be6-4a33-9b9a-c46e36f0c137",
          "secret": "G6u071r8Gjw4V4KSibnb+VK4+tX399hkHaj7LOyHuj5=",
          "type":"ActiveDirectoryOAuth"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-activedirectoryoauth-authentication"></a>Ukázka ZBÝVAJÍCÍ odpověď pro ActiveDirectoryOAuth ověřování

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 885
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: 86d8e9fd-ac0d-4bed-9420-9baba1af3251
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 5183bbf4-9fa1-44bb-98c6-6872e3f2e7ce
x-ms-routing-request-id: WESTUS:20160316T191003Z:5183bbf4-9fa1-44bb-98c6-6872e3f2e7ce
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:10:02 GMT

{  
   "id":"/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
   "type":"Microsoft.Scheduler/jobCollections/jobs",
   "name":"southeastasiajc/httpjob",
   "properties":{  
      "startTime":"2015-05-14T14:10:00Z",
      "action":{  
         "request":{  
            "uri":"https://mywebserviceendpoint.com",
            "method":"GET",
            "headers":{  
               "x-ms-version":"2013-03-01"
            },
            "authentication":{  
               "tenant":"microsoft.onmicrosoft.com",
               "audience":"https://management.core.windows.net/",
               "clientId":"dc23e764-9be6-4a33-9b9a-c46e36f0c137",
               "type":"ActiveDirectoryOAuth"
            }
         },
         "type":"http"
      },
      "recurrence":{  
         "frequency":"minute",
         "endTime":"2016-04-10T08:00:00Z",
         "interval":1
      },
      "state":"enabled",
      "status":{  
         "lastExecutionTime":"2016-03-16T19:10:00.3762123Z",
         "nextExecutionTime":"2016-03-16T19:11:00Z",
         "executionCount":5,
         "failureCount":5,
         "faultedCount":1
      }
   }
}
```

## <a name="see-also"></a>Viz taky


 [Co je Plánovač?](scheduler-intro.md)

 [Azure Plánovač koncepty, terminologie a entity hierarchie](scheduler-concepts-terms.md)

 [Začínáme s používáním Plánovač na portálu Azure](scheduler-get-started-portal.md)

 [Plány a fakturace v Azure Plánovač](scheduler-plans-billing.md)

 [Azure odkaz Plánovač REST API](https://msdn.microsoft.com/library/mt629143)

 [Azure odkaz rutiny prostředí PowerShell Plánovač](scheduler-powershell-reference.md)

 [Azure vysoké dostupnosti Plánovač a spolehlivosti](scheduler-high-availability-reliability.md)

 [Azure Plánovač omezení, výchozí hodnoty a kódy chyb](scheduler-limits-defaults-errors.md)
