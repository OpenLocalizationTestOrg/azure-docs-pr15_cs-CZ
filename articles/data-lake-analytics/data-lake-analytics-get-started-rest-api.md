<properties 
   pageTitle="Začínáme s jezera analýzy dat pomocí rozhraní REST API | Microsoft Azure" 
   description="Použití WebHDFS REST API provádět operace s jezera analýzy dat" 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/19/2016"
   ms.author="jgao"/>

# <a name="get-started-with-azure-data-lake-analytics-using-rest-apis"></a>Začínáme s Azure dat jezera technologie pro analýzu pomocí rozhraní REST API

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Naučte se používat dat jezera analýzy REST API a WebHDFS REST API pro správu dat jezera analýzy účty úlohy a katalogu. 

## <a name="prerequisites"></a>Zjistit předpoklady pro

- **Azure předplatného**. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).

- **Vytvoření aplikace služby Azure Active Directory**. Ověření dat jezera analýzy aplikaci Azure AD pomocí aplikace Azure AD. Způsoby různých ověření s Azure AD, které jsou **ověření koncových uživatelů** nebo **služeb**. Pokyny a další informace o tom, jak ověřit najdete v tématu [ověření s jezera analýzy dat pomocí služby Azure Active Directory](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).

- [otáčení](http://curl.haxx.se/). Tento článek ukazuje, jak lze volat rozhraní REST API proti účet jezera analýzy dat pomocí otočení.

## <a name="authenticate-with-azure-active-directory"></a>Ověření se službou Azure Active Directory

Existují dvě metody ověřování s Azure Active Directory.

### <a name="end-user-authentication-interactive"></a>Ověřování koncových uživatelů (interaktivní)

Tímto způsobem aplikace zobrazí výzvu k přihlášení a všechny operace provádí v souvislosti s uživateli. 

Postup pro interaktivní ověřování

1. Až aplikaci přesměrování uživatele na následující adresu URL:

        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<CLIENT-ID>&response_type=code&redirect_uri=<REDIRECT-URI>

    >[AZURE.NOTE] \<Identifikátor URI PŘESMĚROVÁNÍ > musí být zakódovaný pro použití v adrese URL. Ano, https://localhost, dosáhnete `https%3A%2F%2Flocalhost`)

    Pro účely tohoto kurzu můžete nahradit hodnoty zástupný symbol v adrese URL nad a vložit ho do panelu Adresa prohlížeči. Budete přesměrováni ověření pomocí Azure přihlašovací jméno. Jakmile se úspěšně přihlásit, zobrazí se v adresním řádku prohlížeče odpověď. Odpověď na bude mít v tomto formátu:
        
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>

2. Získání kódu se tak mohli ověřovat z odpovědi. Tento kurz zkopírujte kód pro ověření z panelu Adresa v prohlížeči a předávání v pozvánce na příspěvek tokenu koncový bod, jak je ukázáno v následujícím příkladu:

        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<CLIENT-ID> \
        -F code=<AUTHORIZATION-CODE>

    >[AZURE.NOTE] V tomto případě \<PŘESMĚROVÁNÍ URI > nemusí kódování.

3. Odpověď na je JSON objekt, který obsahuje přístupový token (například `"access_token": "<ACCESS_TOKEN>"`) a token aktualizace (například `"refresh_token": "<REFRESH_TOKEN>"`). Vaše aplikace používá přístupový token při přístupu k úložiště jezera dat Azure a aktualizace token získat další přístupový token, když skončí platnost přístupový token.

        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before": "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}

4.  Když skončí platnost přístupový token, se požádat o nové přístupový token pomocí tokenu aktualizace jak je ukázáno v následujícím příkladu:

         curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
            -F grant_type=refresh_token \
            -F resource=https://management.core.windows.net/ \
            -F client_id=<CLIENT-ID> \
            -F refresh_token=<REFRESH-TOKEN>
 
Další informace o ověřování uživatelů interaktivní najdete v článku [Povolení kód udělit toku](https://msdn.microsoft.com/library/azure/dn645542.aspx).

### <a name="service-to-service-authentication-non-interactive"></a>Služba Služba ověřování (interaktivním)

Tímto způsobem aplikace obsahuje vlastní přihlašovací údaje k provádění operací. V takovém případě musí problému žádost příspěvek jako takové, jaké vidíte níže: 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

Výstup tohoto požadavku bude obsahovat token se tak mohli ověřovat (symbolem `access-token` do výstupu níže), který bude následně předejte s rozhraní REST API volání. Uložte tento token ověřování z textového souboru; budete potřebovat, dál v tomto článku.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

Tento článek používá **interaktivním** přístup. Další informace o interaktivním (volání služby služby) najdete v článku [služby pro volání služeb pomocí přihlašovacích údajů](https://msdn.microsoft.com/library/azure/dn645543.aspx).
## <a name="create-a-data-lake-analytics-account"></a>Vytvoření účtu jezera analýzy dat

Než budete moct vytvářet účet jezera analýzy dat, musíte vytvořit skupina zdroje Azure a jezera úložišti účtu.  V tématu [Vytvoření účtu jezera úložiště](../data-lake-store/data-lake-store-get-started-rest-api.md#create-a-data-lake-store-account).

Příkaz otočení ukazuje, jak vytvořit účet:

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<NewAzureDataLakeAnalyticsAccountName>?api-version=2016-11-01 -d@"C:\tutorials\adla\CreateDataLakeAnalyticsAccountRequest.json"

Nahrazení \< `REDACTED` \> tokenu se tak mohli ověřovat \< `AzureSubscriptionID` \> pomocí svého ID předplatného \< `AzureResourceGroupName` \> s název existujícího pole Skupina zdroje Azure, a \< `NewAzureDataLakeAnalyticsAccountName` \> pod novým názvem účtu technologie pro analýzu dat jezera. Žádost o datové části pro tento příkaz je součástí **CreateDatalakeAnalyticsAccountRequest.json** soubor, který je k dispozici pro `-d` výše uvedených parametrů. Obsah souboru input.json vypadat takto:

    {  
        "location": "East US 2",  
        "name": "myadla1004",  
        "tags": {},  
        "properties": {  
            "defaultDataLakeStoreAccount": "my1004store",  
            "dataLakeStoreAccounts": [  
                {  
                    "name": "my1004store"  
                }     
            ]
        }  
    }  


## <a name="list-data-lake-analytics-accounts-in-a-subscription"></a>Seznam dat jezera analýzy účty v předplatné

Příkaz otočení ukazuje, jak seznam účtů v předplatného:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/providers/Microsoft.DataLakeAnalytics/Accounts?api-version=2016-11-01

Nahrazení \< `REDACTED` \> tokenu se tak mohli ověřovat \< `AzureSubscriptionID` \> pomocí svého ID předplatného. Výstup je podobný:

    {
        "value": [
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla0831.azuredatalakeanalytics.net",
                "accountId": "21e74660-0941-4880-ae72-b143c2615ea9",
                "creationTime": "2016-09-01T12:49:12.7451428Z",
                "lastModifiedTime": "2016-09-01T12:49:12.7451428Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla0831rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla0831",
            "name": "myadla0831",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            },
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla1004.azuredatalakeanalytics.net",
                "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
                "creationTime": "2016-10-04T20:46:42.287147Z",
                "lastModifiedTime": "2016-10-04T20:46:42.287147Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
            "name": "myadla1004",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            }
        ]
    }

## <a name="get-information-about-a-data-lake-analytics-account"></a>Získání informací o účtu jezera analýzy dat

Příkaz otočení ukazuje, jak získat informace o účtu:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>?api-version=2015-11-01

Nahrazení \< `REDACTED` \> tokenu se tak mohli ověřovat \< `AzureSubscriptionID` \> pomocí svého ID předplatného \< `AzureResourceGroupName` \> s název existujícího pole Skupina zdroje Azure, a \< `DataLakeAnalyticsAccountName` \> s názvem existujícího účtu technologie pro analýzu dat jezera. Výstup je podobný:

    {
        "properties": {
            "defaultDataLakeStoreAccount": "my1004store",
            "dataLakeStoreAccounts": [
            {
                "properties": {
                "suffix": "azuredatalakestore.net"
                },
                "name": "my1004store"
            }
            ],
            "provisioningState": "Creating",
            "state": null,
            "endpoint": null,
            "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
            "creationTime": null,
            "lastModifiedTime": null
        },
        "location": "East US 2",
        "tags": {},
        "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
        "name": "myadla1004",
        "type": "Microsoft.DataLakeAnalytics/accounts"
    }

## <a name="list-data-lake-stores-of-a-data-lake-analytics-account"></a>Seznam datových jezera úložišť účtu jezera analýzy dat

Příkaz otočení ukazuje, jak seznam úložiště jezera dat účtu:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>/DataLakeStoreAccounts/?api-version=2016-11-01

Nahrazení \< `REDACTED` \> tokenu se tak mohli ověřovat \< `AzureSubscriptionID` \> pomocí svého ID předplatného \< `AzureResourceGroupName` \> s název existujícího pole Skupina zdroje Azure, a \< `DataLakeAnalyticsAccountName` \> s názvem existujícího účtu analýzy jezera Data. Výstup je podobný:

    {
        "value": [
            {
            "properties": {
                "suffix": "azuredatalakestore.net"
            },
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004/dataLakeStoreAccounts/my1004store",
            "name": "my1004store",
            "type": "Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts"
            }
        ]
    }

## <a name="submit-u-sql-jobs"></a>Odeslat úlohy U SQL

Příkaz otočení ukazuje, jak můžou odeslat úlohu U SQL:

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs/<NewGUID>?api-version=2016-03-20-preview -d@"C:\tutorials\adla\SubmitADLAJob.json"

Nahrazení \< `REDACTED` \> tokenu se tak mohli ověřovat \< `DataLakeAnalyticsAccountName` \> s názvem existujícího účtu technologie pro analýzu dat jezera. Žádost o datové části pro tento příkaz je součástí **SubmitADLAJob.json** soubor, který je k dispozici pro `-d` výše uvedených parametrů. Obsah souboru input.json vypadat takto:

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "properties": {
            "type": "USql",
            "script": "@searchlog =\n    EXTRACT UserId          int,\n            Start           DateTime,\n            Region          string,\n            Query          
        string,\n            Duration        int?,\n            Urls            string,\n            ClickedUrls     string\n    FROM \"/Samples/Data/SearchLog.tsv\"\n    US
        ING Extractors.Tsv();\n\nOUTPUT @searchlog   \n    TO \"/Output/SearchLog-from-Data-Lake.csv\"\nUSING Outputters.Csv();"
        }
    }

Výstup je podobný:

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "myadl@SPI",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "2016-10-05T13:54:59.9871859+00:00",
        "state": "Compiling",
        "result": "Succeeded",
        "stateAuditRecords": [
            {
            "newState": "New",
            "timeStamp": "2016-10-05T13:54:59.9871859+00:00",
            "details": "userName:myadl@SPI;submitMachine:N/A"
            }
        ],
        "properties": {
            "owner": "myadl@SPI",
            "resources": [],
            "runtimeVersion": "default",
            "rootProcessNodeId": "00000000-0000-0000-0000-000000000000",
            "algebraFilePath": "adl://myadls0831.azuredatalakestore.net/system/jobservice/jobs/Usql/2016/10/05/13/54/8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a/algebra.xml",
            "compileMode": "Semantic",
            "errorSource": "Unknown",
            "totalCompilationTime": "PT0S",
            "totalPausedTime": "PT0S",
            "totalQueuedTime": "PT0S",
            "totalRunningTime": "PT0S",
            "type": "USql"
        }
    }


## <a name="list-u-sql-jobs"></a>Seznam U SQL úlohy

Příkaz otočení ukazuje, jak seznamu úloh U SQL:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs?api-version=2016-11-01 

Nahrazení \< `REDACTED` \> tokenu se tak mohli ověřovat a \< `DataLakeAnalyticsAccountName` \> s názvem existujícího účtu technologie pro analýzu dat jezera. 


Výstup je podobný:

    {
    "value": [
        {
        "jobId": "65cf1691-9dbe-43cd-90ed-1cafbfb406fb",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someone@microsoft.com",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:46:53 GMT",
        "startTime": "Wed, 05 Oct 2016 13:47:33 GMT",
        "endTime": "Wed, 05 Oct 2016 13:48:07 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        },
        {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someoneadl@SPI",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:54:59 GMT",
        "startTime": "Wed, 05 Oct 2016 13:55:43 GMT",
        "endTime": "Wed, 05 Oct 2016 13:56:11 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        }
    ],
    "nextLink": null,
    "count": null
    }


## <a name="get-catalog-items"></a>Získání položky katalogu

Příkaz otočení ukazuje, jak získat databáze z katalogu:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/catalog/usql/databases?api-version=2016-11-01

Výstup je podobný:

    {
    "@odata.context":"https://myadla0831.azuredatalakeanalytics.net/sqlip/$metadata#databases","value":[
        {
        "computeAccountName":"myadla0831","databaseName":"mytest","version":"f6956327-90b8-4648-ad8b-de3ff09274ea"
        },{
        "computeAccountName":"myadla0831","databaseName":"master","version":"e8bca908-cc73-41a3-9564-e9bcfaa21f4e"
        }
    ]
    }

## <a name="see-also"></a>Viz taky

- Složitější dotaz najdete v tématu [protokoly analyzovat webu pomocí analýzy jezera dat Azure](data-lake-analytics-analyze-weblogs.md).
- Začínáme s vývojem aplikací U SQL, najdete v článku [pomocí nástroje jezera datového Visual Studio skripty vyvíjet U-SQL](data-lake-analytics-data-lake-tools-get-started.md).
- Další U SQL najdete v tématu [Začínáme s jazykem dat Azure jezera analýzy U-SQL](data-lake-analytics-u-sql-get-started.md).
- Úlohy správy najdete v článku [Správa jezera analýzy dat Azure Azure portálu](data-lake-analytics-manage-use-portal.md).
- Získejte přehled o jezera analýzy dat, najdete v článku [Přehled analýzy jezera dat Azure](data-lake-analytics-overview.md).
- Stejný kurz pomocí dalších nástrojů zobrazíte kliknutím na kartu voliče v horní části stránky.
- Protokolování diagnostiky informace, naleznete v [protokolech diagnostiky přístup k jezera analýzy dat Azure](data-lake-analytics-diagnostic-logs.md)
