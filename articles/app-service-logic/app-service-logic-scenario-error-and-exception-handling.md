<properties
    pageTitle="Protokolování a zpracování chyb v logických aplikace | Microsoft Azure"
    description="Zobrazení případ použití reálné rozšířené zpracování chyb a protokolování s aplikacemi jiných logiky"
    keywords=""
    services="logic-apps"
    authors="hedidin"
    manager="anneta"
    editor=""
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="b-hoedid"/>

# <a name="logging-and-error-handling-in-logic-apps"></a>Protokolování a zpracování chyb v logických aplikace

Tento článek popisuje, jak můžete rozšířit aplikace logiky pro lepší podporu výjimek. Je případ použití reálné a naše odpovědi na otázky, "Logiky aplikace podporuje výjimky a zpracování chyb?"

>[AZURE.NOTE]Aktuální verzi funkci logiku aplikace Microsoft Azure aplikace služby obsahuje standardní šablony pro odpovědi na akce.
>Platí to i vnitřní ověření a odpovědi chyby vrácené rozhraní API aplikace.

## <a name="overview-of-the-use-case-and-scenario"></a>Základní informace o případu použití a scénáři

Následující článek je případ použití tohoto článku.
Známé zdravotní péče organizace provozují nám se dají Azure řešení, které chcete vytvořit pacienta portál pomocí Microsoft Dynamics CRM Online. Potřebovali odeslat událost záznamy obsahující datum mezi Dynamics CRM Online pacienta portálem a služby Salesforce.  Jsme byly požádáni o využívat standard [HL7 FHIR](http://www.hl7.org/implement/standards/fhir/) pro všechny pacienta záznamy.

Projekt má dva hlavní požadavky:  

 -  Způsob, jak protokolovat záznamy odeslané z portálu Microsoft Dynamics CRM Online
 -  Způsob, jak zobrazit všechny chyby, ke kterým došlo v pracovním postupu


## <a name="how-we-solved-the-problem"></a>Jak můžeme vyřešit problém

>[AZURE.TIP] [Skupiny uživatelů integrace](http://www.integrationusergroup.com/do-logic-apps-support-error-handling/ "Integrace skupiny uživatelů")můžete zobrazit nejvyšší úrovně video projektu.

Zvolili jsme [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/ "Azure DocumentDB") jako úložiště pro protokolování a chyby záznamy (DocumentDB odkazuje na záznamy jako dokumenty). Protože aplikace logiku má standardní šablony pro všechny odpovědi, máme by vlastní schéma vytvořit. Jsme bylo možné vytvořit aplikaci pro rozhraní API a **vložte** **dotaz** chyby a protokolu záznamů. Definovat schéma jsme taky pro jednotlivá pole v rámci rozhraní API aplikace.  

Další požadavky bylo vymazání záznamů po určitém datu. DocumentDB má vlastnost s názvem [Time to Live](https://azure.microsoft.com/blog/documentdb-now-supports-time-to-live-ttl/ "Time to Live") (TTL), což povoleno cz nastavte hodnotu **Time to Live** pro každý záznam a nebo kolekce. To vyloučenou potřebujete odstranit záznamy v DocumentDB ručně.

### <a name="creation-of-the-logic-app"></a>Vytvoření aplikace pro použití logických operátorů

Cílem prvního kroku je vytvořit aplikaci logiky a načíst v návrháři. V tomto příkladu používáme aplikace logiky nadřazenosti a podřízenosti. Předpokládejme, že jste již vytvořili nadřazený jsme vytvoříte jedné aplikaci logiky podřízené.

Protože budeme být protokolování záznam pocházejících z Dynamics CRM Online, Začněme nahoře. Potřebujeme, používali aktivační žádosti o aplikaci logiky nadřazené spustí tento podřízené.

> [AZURE.IMPORTANT] Tento kurz bude potřeba vytvořit databázi DocumentDB a dvě kolekce (protokolování a chyby).

### <a name="logic-app-trigger"></a>Použití logických operátorů aplikace aktivační událost

Jak je vidět v následujícím příkladu používáme aktivační žádost.

```` json
"triggers": {
        "request": {
          "type": "request",
          "kind": "http",
          "inputs": {
            "schema": {
              "properties": {
                "CRMid": {
                  "type": "string"
                },
                "recordType": {
                  "type": "string"
                },
                "salesforceID": {
                  "type": "string"
                },
                "update": {
                  "type": "boolean"
                }
              },
              "required": [
                "CRMid",
                "recordType",
                "salesforceID",
                "update"
              ],
              "type": "object"
            }
          }
        }
      },

````


### <a name="steps"></a>Postup

Potřebujeme protokolu zdroj (žádost) pacienta záznam z portálu Microsoft Dynamics CRM Online.

1. Potřebujeme získat nový záznam událost z Dynamics CRM Online.
    Aktivační událost pocházejících z CRM nám nabízí **CRM PatentId**, **typ záznamu**, **Nový nebo aktualizace záznamu** (nové nebo aktualizovat logická hodnota) a **SalesforceId**. **SalesforceId** může být null, protože se používá pouze k aktualizaci.
    Jsme pošle záznam CRM pomocí CRM **PatientID** a **Typ záznamu**.
1. Pak potřebujeme přidání naše DocumentDB rozhraní API aplikace **InsertLogEntry** operace, jak vidíte na následujících obrázcích.


#### <a name="insert-log-entry-designer-view"></a>Vložení zobrazení návrhu položka protokolu

![Vložení záznam](./media/app-service-logic-scenario-error-and-exception-handling/lognewpatient.png)

#### <a name="insert-error-entry-designer-view"></a>Vložení zobrazení návrhu položku chyby
![Vložení záznam](./media/app-service-logic-scenario-error-and-exception-handling/insertlogentry.png)

#### <a name="check-for-create-record-failure"></a>Vyhledat vytvoření záznamu selhání

![Podmínky](./media/app-service-logic-scenario-error-and-exception-handling/condition.png)


## <a name="logic-app-source-code"></a>Použití logických operátorů aplikace zdrojového kódu

>[AZURE.NOTE]  Následují ukázky pouze. Protože tohoto kurzu je založená na implementaci aktuálně ve výrobním, hodnota **Zdrojový uzel** nemusí zobrazovat vlastnosti, které se vztahují k naplánování události.

### <a name="logging"></a>Zapnout protokolování
V následujícím příkladu použití logických operátorů aplikace kód ukazuje, jak řešit protokolování.

#### <a name="log-entry"></a>Záznam
Toto je použití logických operátorů aplikace zdrojového kódu pro vložení položka protokolu.

``` json
"InsertLogEntry": {
        "metadata": {
        "apiDefinitionUrl": "https://.../swagger/docs/v1",
        "swaggerSource": "website"
        },
        "type": "Http",
        "inputs": {
        "body": {
            "date": "@{outputs('Gets_NewPatientRecord')['headers']['Date']}",
            "operation": "New Patient",
            "patientId": "@{triggerBody()['CRMid']}",
            "providerId": "@{triggerBody()['providerID']}",
            "source": "@{outputs('Gets_NewPatientRecord')['headers']}"
        },
        "method": "post",
        "uri": "https://.../api/Log"
        },
        "runAfter":    {
            "Gets_NewPatientecord": ["Succeeded"]
        }
}
```

#### <a name="log-request"></a>Žádost o protokolu

Toto je zprávě s žádostí o protokolu publikované na rozhraní API aplikace.

``` json
    {
    "uri": "https://.../api/Log",
    "method": "post",
    "body": {
        "date": "Fri, 10 Jun 2016 22:31:56 GMT",
        "operation": "New Patient",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "providerId": "",
        "source": "{\"Pragma\":\"no-cache\",\"x-ms-request-id\":\"e750c9a9-bd48-44c4-bbba-1688b6f8a132\",\"OData-Version\":\"4.0\",\"Cache-Control\":\"no-cache\",\"Date\":\"Fri, 10 Jun 2016 22:31:56 GMT\",\"Set-Cookie\":\"ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1\",\"Server\":\"Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0\",\"X-AspNet-Version\":\"4.0.30319\",\"X-Powered-By\":\"ASP.NET\",\"Content-Length\":\"1935\",\"Content-Type\":\"application/json; odata.metadata=minimal; odata.streaming=true\",\"Expires\":\"-1\"}"
        }
    }

```


#### <a name="log-response"></a>Protokolovat odpověď

Toto je protokolu odpovědi z rozhraní API aplikace.

``` json
{
    "statusCode": 200,
    "headers": {
        "Pragma": "no-cache",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:32:17 GMT",
        "Server": "Microsoft-IIS/8.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "964",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "ttl": 2592000,
        "id": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0_1465597937",
        "_rid": "XngRAOT6IQEHAAAAAAAAAA==",
        "_self": "dbs/XngRAA==/colls/XngRAOT6IQE=/docs/XngRAOT6IQEHAAAAAAAAAA==/",
        "_ts": 1465597936,
        "_etag": "\"0400fc2f-0000-0000-0000-575b3ff00000\"",
        "patientID": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "timestamp": "2016-06-10T22:31:56Z",
        "source": "{\"Pragma\":\"no-cache\",\"x-ms-request-id\":\"e750c9a9-bd48-44c4-bbba-1688b6f8a132\",\"OData-Version\":\"4.0\",\"Cache-Control\":\"no-cache\",\"Date\":\"Fri, 10 Jun 2016 22:31:56 GMT\",\"Set-Cookie\":\"ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1\",\"Server\":\"Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0\",\"X-AspNet-Version\":\"4.0.30319\",\"X-Powered-By\":\"ASP.NET\",\"Content-Length\":\"1935\",\"Content-Type\":\"application/json; odata.metadata=minimal; odata.streaming=true\",\"Expires\":\"-1\"}",
        "operation": "New Patient",
        "salesforceId": "",
        "expired": false
    }
}

```

Teď Pojďme se podívat zpracování chyb ve vzorcích kroky.


### <a name="error-handling"></a>Zpracování chyb

Následující příklad použití logických operátorů aplikace kódu ukazuje, jak implementovat zpracování chyb.

#### <a name="create-error-record"></a>Vytvoření záznamu chyby

Toto je zdrojový kód logiku aplikace pro vytváření záznamu chyby.

``` json
"actions": {
    "CreateErrorRecord": {
        "metadata": {
        "apiDefinitionUrl": "https://.../swagger/docs/v1",
        "swaggerSource": "website"
        },
        "type": "Http",
        "inputs": {
        "body": {
            "action": "New_Patient",
            "isError": true,
            "crmId": "@{triggerBody()['CRMid']}",
            "patientID": "@{triggerBody()['CRMid']}",
            "message": "@{body('Create_NewPatientRecord')['message']}",
            "providerId": "@{triggerBody()['providerId']}",
            "severity": 4,
            "source": "@{actions('Create_NewPatientRecord')['inputs']['body']}",
            "statusCode": "@{int(outputs('Create_NewPatientRecord')['statusCode'])}",
            "salesforceId": "",
            "update": false
        },
        "method": "post",
        "uri": "https://.../api/CrMtoSfError"
        },
        "runAfter":
        {
            "Create_NewPatientRecord": ["Failed" ]
        }
    }
}          
```

#### <a name="insert-error-into-documentdb--request"></a>Vložení chyby do DocumentDB – požádat o

``` json

{
    "uri": "https://.../api/CrMtoSfError",
    "method": "post",
    "body": {
        "action": "New_Patient",
        "isError": true,
        "crmId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "message": "Salesforce failed to complete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "providerId": "",
        "severity": 4,
        "salesforceId": "",
        "update": false,
        "source": "{\"Account_Class_vod__c\":\"PRAC\",\"Account_Status_MED__c\":\"I\",\"CRM_HUB_ID__c\":\"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0\",\"Credentials_vod__c\",\"DTC_ID_MED__c\":\"\",\"Fax\":\"\",\"FirstName\":\"A\",\"Gender_vod__c\":\"\",\"IMS_ID__c\":\"\",\"LastName\":\"BAILEY\",\"MasterID_mp__c\":\"\",\"C_ID_MED__c\":\"851588\",\"Middle_vod__c\":\"\",\"NPI_vod__c\":\"\",\"PDRP_MED__c\":false,\"PersonDoNotCall\":false,\"PersonEmail\":\"\",\"PersonHasOptedOutOfEmail\":false,\"PersonHasOptedOutOfFax\":false,\"PersonMobilePhone\":\"\",\"Phone\":\"\",\"Practicing_Specialty__c\":\"FM - FAMILY MEDICINE\",\"Primary_City__c\":\"\",\"Primary_State__c\":\"\",\"Primary_Street_Line2__c\":\"\",\"Primary_Street__c\":\"\",\"Primary_Zip__c\":\"\",\"RecordTypeId\":\"012U0000000JaPWIA0\",\"Request_Date__c\":\"2016-06-10T22:31:55.9647467Z\",\"ONY_ID__c\":\"\",\"Specialty_1_vod__c\":\"\",\"Suffix_vod__c\":\"\",\"Website\":\"\"}",
        "statusCode": "400"
    }
}
```

#### <a name="insert-error-into-documentdb--response"></a>Vložení chyby do DocumentDB – odpověď


``` json
{
    "statusCode": 200,
    "headers": {
        "Pragma": "no-cache",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:31:57 GMT",
        "Server": "Microsoft-IIS/8.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "1561",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "id": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0-1465597917",
        "_rid": "sQx2APhVzAA8AAAAAAAAAA==",
        "_self": "dbs/sQx2AA==/colls/sQx2APhVzAA=/docs/sQx2APhVzAA8AAAAAAAAAA==/",
        "_ts": 1465597912,
        "_etag": "\"0c00eaac-0000-0000-0000-575b3fdc0000\"",
        "prescriberId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "timestamp": "2016-06-10T22:31:57.3651027Z",
        "action": "New_Patient",
        "salesforceId": "",
        "update": false,
        "body": "CRM failed to complete task: Message: duplicate value found: CRM_HUB_ID__c duplicates value on record with id: 001U000001c83gK",
        "source": "{\"Account_Class_vod__c\":\"PRAC\",\"Account_Status_MED__c\":\"I\",\"CRM_HUB_ID__c\":\"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0\",\"Credentials_vod__c\":\"DO - Degree level is DO\",\"DTC_ID_MED__c\":\"\",\"Fax\":\"\",\"FirstName\":\"A\",\"Gender_vod__c\":\"\",\"IMS_ID__c\":\"\",\"LastName\":\"BAILEY\",\"MterID_mp__c\":\"\",\"Medicis_ID_MED__c\":\"851588\",\"Middle_vod__c\":\"\",\"NPI_vod__c\":\"\",\"PDRP_MED__c\":false,\"PersonDoNotCall\":false,\"PersonEmail\":\"\",\"PersonHasOptedOutOfEmail\":false,\"PersonHasOptedOutOfFax\":false,\"PersonMobilePhone\":\"\",\"Phone\":\"\",\"Practicing_Specialty__c\":\"FM - FAMILY MEDICINE\",\"Primary_City__c\":\"\",\"Primary_State__c\":\"\",\"Primary_Street_Line2__c\":\"\",\"Primary_Street__c\":\"\",\"Primary_Zip__c\":\"\",\"RecordTypeId\":\"012U0000000JaPWIA0\",\"Request_Date__c\":\"2016-06-10T22:31:55.9647467Z\",\"XXXXXXX\":\"\",\"Specialty_1_vod__c\":\"\",\"Suffix_vod__c\":\"\",\"Website\":\"\"}",
        "code": 400,
        "errors": null,
        "isError": true,
        "severity": 4,
        "notes": null,
        "resolved": 0
        }
}
```

#### <a name="salesforce-error-response"></a>Odpověď chybu služby Salesforce

``` json
{
    "statusCode": 400,
    "headers": {
        "Pragma": "no-cache",
        "x-ms-request-id": "3e8e4884-288e-4633-972c-8271b2cc912c",
        "X-Content-Type-Options": "nosniff",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:31:56 GMT",
        "Set-Cookie": "ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1",
        "Server": "Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "205",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "status": 400,
        "message": "Salesforce failed to complete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "source": "Salesforce.Common",
        "errors": []
    }
}

```

### <a name="returning-the-response-back-to-the-parent-logic-app"></a>Vrací odpovědi zpátky do aplikace nadřazené použití logických operátorů

Až budete mít odpověď, předáte ji zpátky do aplikace logiky nadřazené.

#### <a name="return-success-response-to-the-parent-logic-app"></a>Vrátit úspěch odpověď aplikaci nadřazené logiky

``` json
"SuccessResponse": {
    "runAfter":
        {
            "UpdateNew_CRMPatientResponse": ["Succeeded"]
        },
    "inputs": {
        "body": {
            "status": "Success"
    },
    "headers": {
    "   Content-type": "application/json",
        "x-ms-date": "@utcnow()"
    },
    "statusCode": 200
    },
    "type": "Response"
}
```

#### <a name="return-error-response-to-the-parent-logic-app"></a>Vrátit chyba odpověď aplikaci nadřazené logiky

``` json
"ErrorResponse": {
    "runAfter":
        {
            "Create_NewPatientRecord": ["Failed"]
        },
    "inputs": {
        "body": {
            "status": "BadRequest"
        },
        "headers": {
            "Content-type": "application/json",
            "x-ms-date": "@utcnow()"
        },
        "statusCode": 400
    },
    "type": "Response"
}

```


## <a name="documentdb-repository-and-portal"></a>Portál a DocumentDB úložiště

Naše řešení přidat další možnosti s [DocumentDB](https://azure.microsoft.com/services/documentdb).

### <a name="error-management-portal"></a>Portál Správa chyby

Pokud chcete zobrazit chyby, můžete vytvořit webovou aplikaci MVC zobrazit chyby záznamy z DocumentDB. **Seznam** **Podrobnosti**, **Upravit**a **Odstranit** operace jsou součástí aktuální verzi.

> [AZURE.NOTE]Úprava operace: DocumentDB má nahradit celý dokument.
> Záznamy zobrazené v **seznamu** a **podrobností** zobrazení jsou uvedeny ukázky pouze. Nejsou záznamy skutečné pacienta schůzek.

Tady jsou příklady naše MVC podrobnosti aplikace vytvořená pomocí bylo popsáno dříve přístup.

#### <a name="error-management-list"></a>Seznam správy chyb

![Seznam chyb](./media/app-service-logic-scenario-error-and-exception-handling/errorlist.png)

#### <a name="error-management-detail-view"></a>Zobrazení podrobností správy chyby

![Podrobnosti o chybě](./media/app-service-logic-scenario-error-and-exception-handling/errordetails.png)

### <a name="log-management-portal"></a>Portál Správa protokolu

Pokud chcete zobrazit protokoly, taky jsme vytvořili webovou aplikaci MVC.  Tady jsou příklady náš MVC podrobnosti aplikace vytvořená pomocí bylo popsáno dříve přístup.

#### <a name="sample-log-detail-view"></a>Zobrazení podrobností protokolu ukázka

![Zobrazení podrobností protokolu](./media/app-service-logic-scenario-error-and-exception-handling/samplelogdetail.png)

### <a name="api-app-details"></a>Rozhraní API aplikace podrobnosti

#### <a name="logic-apps-exception-management-api"></a>Rozhraní API správy výjimce aplikace logiky

Náš otevřít zdroj logiky aplikace výjimce správy rozhraní API aplikace poskytuje následující funkce.

Existují dva řadiče:

- **ErrorController** vloží záznam chyby (dokument) v kolekci DocumentDB.
- **LogController** Vloží záznam protokolu (dokument) v kolekci DocumentDB.

> [AZURE.TIP] Použití obou řadiče `async Task<dynamic>` operace. Díky operace vyřešit za běhu, takže vytvoříme schématu DocumentDB v těle operace.

Musí mít každý dokument v DocumentDB jedinečné ID. Používáme `PatientId` a přidání časového razítka, která je převedena na hodnotu časového razítka Unix (dvojitě). Jsme Zkraťte údaj o odstranění zlomek hodnotu.

Zdrojový kód naše řadiče chybě rozhraní API [z GitHub](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi/blob/master/Logic App Exception Management API/Controllers/ErrorController.cs)můžete zobrazit.

Rozhraní API z app logiky jsme zavolat pomocí následující syntaxe.

``` json
 "actions": {
        "CreateErrorRecord": {
          "metadata": {
            "apiDefinitionUrl": "https://.../swagger/docs/v1",
            "swaggerSource": "website"
          },
          "type": "Http",
          "inputs": {
            "body": {
              "action": "New_Patient",
              "isError": true,
              "crmId": "@{triggerBody()['CRMid']}",
              "prescriberId": "@{triggerBody()['CRMid']}",
              "message": "@{body('Create_NewPatientRecord')['message']}",
              "salesforceId": "@{triggerBody()['salesforceID']}",
              "severity": 4,
              "source": "@{actions('Create_NewPatientRecord')['inputs']['body']}",
              "statusCode": "@{int(outputs('Create_NewPatientRecord')['statusCode'])}",
              "update": false
            },
            "method": "post",
            "uri": "https://.../api/CrMtoSfError"
          },
          "runAfter": {
              "Create_NewPatientRecord": ["Failed"]
            }
        }
 }
```

Výraz v předchozím příkladu je kontrola stavu *Create_NewPatientRecord* **se nezdařila**.

## <a name="summary"></a>Souhrn

- Můžete snadno implementovat protokolování a zpracování chyb v aplikaci použití logických operátorů.
- DocumentDB slouží jako úložiště pro protokolování a chyby záznamy (dokumenty).
- MVC slouží k vytvoření portálu zobrazíte záznamů protokolu a chyby.

### <a name="source-code"></a>Zdrojový kód
Zdrojový kód při správě výjimek logiky aplikace rozhraní API aplikace je k dispozici v této [GitHub úložiště](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi "Logiky rozhraní API správy výjimce aplikace").


## <a name="next-steps"></a>Další kroky
- [Zobrazit další příklady použití logických operátorů aplikace a scénáře](app-service-logic-examples-and-scenarios.md)
- [Další informace o použití logických operátorů aplikace nástroje pro sledování](app-service-logic-monitor-your-logic-apps.md)
- [Vytvoření šablony automatické implementace logiky aplikace](app-service-logic-create-deploy-template.md)
