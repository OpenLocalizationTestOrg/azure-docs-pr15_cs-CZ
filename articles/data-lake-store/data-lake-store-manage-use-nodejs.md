<properties 
   pageTitle="Začínáme s úložiště jezera dat Azure pomocí Azure SDK pro Node.js | Microsoft Azure"
   description="Naučte se používat Node.js pro práci s účty úložiště jezera dat a v systému souborů." 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-azure-sdk-for-nodejs"></a>Začínáme s použitím Azure SDK pro Node.js úložiště jezera dat Azure

> [AZURE.SELECTOR]
- [Portál](data-lake-store-get-started-portal.md)
- [Prostředí PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [ROZHRANÍ REST API](data-lake-store-get-started-rest-api.md)
- [Azure rozhraní příkazového řádku](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)


Naučte se používat SDK Azure pro Node.js vytvořte účet Azure úložišti jezera a provádění základních operací, například při vytvoření složek, nahrávání a stahování datové soubory a odstraňovat klienta, atd. Další informace o úložiště jezera dat najdete v článku [Přehled úložiště jezera dat](data-lake-store-overview.md). V současné době podporuje SDK

  *  **Verze Node.js: 0.10.0 nebo vyšší**
  *  **Verze rozhraní REST API pro účet: 2015 10 01 preview**
  *  **Verze rozhraní REST API pro systém souborů: 2015 10 01 preview**

## <a name="prerequisites"></a>Zjistit předpoklady pro

Než začnete v tomto článku, musíte mít takto:

- **Azure předplatného**. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).

- **Vytvoření aplikace služby Azure Active Directory**. Ověření dat jezera úložiště aplikaci Azure AD pomocí aplikace Azure AD. Způsoby různých ověření s Azure AD, které jsou **ověření koncových uživatelů** nebo **služeb**. Pokyny a další informace o tom, jak ověřit najdete v tématu [Ověřit s úložiště jezera dat pomocí služby Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="how-to-install"></a>Jak nainstalovat

```bash
npm install azure-arm-datalake-store
```

## <a name="authenticate-using-azure-active-directory"></a>Ověření pomocí služby Azure Active Directory

Fragmenty dole způsoby dvou samostatných ověřování dat jezera Store Azure AD pomocí. Podrobné informace o různých metod pro účely ověření dat jezera Store najdete v článku [Ověřit s úložiště jezera dat pomocí služby Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

Fragment dole také vyžaduje vstupů jako název domény Azure AD, ID klienta pro aplikaci Azure AD, atd. Tyto informace lze načtená z Azure AD aplikace, která je nutné vytvořili, podrobnosti, které jsou součástí výše uvedeného odkazu.

 ```javascript
 var msrestAzure = require('ms-rest-azure');
 //user authentication
 var credentials = new msRestAzure.UserTokenCredentials('your-client-id', 'your-domain', 'your-username', 'your-password', 'your-redirect-uri');
 //service principal authentication
 var credentials = new msRestAzure.ApplicationTokenCredentials('your-client-id', 'your-domain', 'your-secret');
 ```

## <a name="create-the-data-lake-store-clients"></a>Vytvoření klientů jezera úložiště dat

```javascript
var adlsManagement = require("azure-arm-datalake-store");
var acccountClient = new adlsManagement.DataLakeStoreAccountClient(credentials, "your-subscription-id");
var filesystemClient = new adlsManagement.DataLakeStoreFileSystemClient(credentials);
```

## <a name="create-a-data-lake-store-account"></a>Vytvoření účtu úložiště jezera dat

```javascript
var util = require('util');
var resourceGroupName = 'testrg';
var accountName = 'testadlsacct';
var location = 'eastus2';

// account object to create
var accountToCreate = {
  tags: {
    testtag1: 'testvalue1',
    testtag2: 'testvalue2'
  },
  name: accountName,
  location: location
};

client.account.create(resourceGroupName, accountName, accountToCreate, function (err, result, request, response) {
  if (err) {
    console.log(err);
    /*err has reference to the actual request and response, so you can see what was sent and received on the wire.
      The structure of err looks like this:
      err: {
        code: 'Error Code',
        message: 'Error Message',
        body: 'The response body if any',
        request: reference to a stripped version of http request
        response: reference to a stripped version of the response
      }
    */
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="create-a-file-with-content"></a>Vytvoření souboru s obsahem
```javascript
var util = require('util');
var accountName = 'testadlsacct';
var fileToCreate = '/myfolder/myfile.txt';
var options = {
  streamContents: new Buffer('some string content')
}

filesystemClient.fileSystem.listFileStatus(accountName, fileToCreate, options, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    // no result is returned, only a 201 response for success.
    console.log('response is: ' + util.inspect(response, {depth: null}));
  }
});
```

## <a name="get-a-list-of-files-and-folders"></a>Zobrazte seznam souborů a složek

```javascript
var util = require('util');
var accountName = 'testadlsacct';
var pathToEnumerate = '/myfolder';
filesystemClient.fileSystem.listFileStatus(accountName, pathToEnumerate, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="see-also"></a>Viz taky

- [Microsoft Azure SDK Node.js](https://github.com/azure/azure-sdk-for-node)
- [Microsoft Azure SDK Node.js – Správa jezera analýzy dat](https://www.npmjs.com/package/azure-arm-datalake-analytics)
