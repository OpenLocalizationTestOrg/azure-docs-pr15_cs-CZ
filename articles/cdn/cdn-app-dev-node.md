<properties
    pageTitle="Začínáme s Azure CDN SDK pro Node.js | Microsoft Azure"
    description="Zjistěte, jak psát Node.js aplikace ke správě Azure CDN."
    services="cdn"
    documentationCenter="nodejs"
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="casoper"/>

# <a name="get-started-with-azure-cdn-development"></a>Začínáme s vývojem Azure CDN

> [AZURE.SELECTOR]
- [Node.js](cdn-app-dev-node.md)
- [.NET](cdn-app-dev-net.md)

[Azure CDN SDK Node.js](https://www.npmjs.com/package/azure-arm-cdn) můžete použít k automatizaci vytváření a Správa profilů CDN a koncové body.  Tento kurz provede vytvoření jednoduchého Node.js konzoly aplikace, která ukazuje některé z dostupných operací.  Tento kurz není určená k podrobně popisují všechny aspekty Azure CDN SDK Node.js.

Tento kurz se už měli [Node.js](http://www.nodejs.org) **4.x.x** nebo vyšší nainstalovali a nakonfigurovali.  Můžete použít jakémkoli textovém editoru, které chcete vytvořit Node.js aplikace.  Psaní tento kurz že používá [Visual Studio kód](https://code.visualstudio.com).  

> [AZURE.TIP] [Dokončení projektu z tohoto kurzu](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74) je k dispozici ke stažení na webu MSDN.

[AZURE.INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-npm-dependencies"></a>Vytvoření projektu a přidat NPM závislosti

Teď můžeme jste vytvořili skupina zdroje pro naše CDN profily a naše Azure AD aplikace oprávnění ke správě profilů CDN a koncové body v dané skupině, můžeme začít vytvářet naše aplikace.

Vytvoření složky pro ukládání aplikace.  Z konzoly Node.js nástroje v aktuální cesty nastavte vaše aktuální umístění nové složky a inicializace projektu spuštěním:
    
    npm init
    
Potom zobrazí řadu otázek inicializace projektu.  Tento kurz se používá pro **vstupní bod**, *app.js*.  Zobrazí se Moje zpřístupněte jiné volby v následujícím příkladu.

![Výstup inicializace NPM](./media/cdn-app-dev-node/cdn-npm-init.png)

Náš projektu je nyní inicializováno se souborem *packages.json* .  Náš projektu bude používat některé Azure knihovny obsažené v NPM balíčků.  Použijeme Azure Client Runtime pro Node.js ([ms azure rest) a knihovně Azure CDN klienta pro Node.js (azure arm cd).  Přidejte do něj můžou být projektu jako závislostí.
 
    npm install --save ms-rest-azure
    npm install --save azure-arm-cdn

Po dokončení balíčky instalaci *package.json* soubor by měl vypadat podobně jako tento příklad (verze čísla se můžou lišit):

``` json
{
  "name": "cdn_node",
  "version": "1.0.0",
  "description": "Azure CDN Node.js tutorial project",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Cam Soper",
  "license": "MIT",
  "dependencies": {
    "azure-arm-cdn": "^0.2.1",
    "ms-rest-azure": "^1.14.4"
  }
}
```

Nakonec v textovém editoru, vytvořte prázdný textový soubor a uložit do kořenové složky naše projektu jako *app.js*.  Teď připraveni začít psát kód.

## <a name="requires-constants-authentication-and-structure"></a>Vyžaduje konstanty, ověřování a struktury

S *app.js* otevřený v naší editor nastavíme základní struktura naše program napsaný.

1. Přidejte "vyžaduje" pro naše NPM balíčků nahoře s takto:

    ``` javascript
    var msRestAzure = require('ms-rest-azure');
    var cdnManagementClient = require('azure-arm-cdn');
    ```

2. Je třeba definovat některé konstanty používané naše metody.  Přidejte následující text.  Nezapomeňte místo zástupné symboly, včetně ** &lt;lomenými závorkami&gt;**, s vlastními hodnotami v případě potřeby.

    ``` javascript
    //Tenant app constants
    const clientId = "<YOUR CLIENT ID>";
    const clientSecret = "<YOUR CLIENT AUTHENTICATION KEY>"; //Only for service principals
    const tenantId = "<YOUR TENANT ID>";

    //Application constants
    const subscriptionId = "<YOUR SUBSCRIPTION ID>";
    const resourceGroupName = "CdnConsoleTutorial";
    const resourceLocation = "<YOUR PREFERRED AZURE LOCATION, SUCH AS Central US>";
    ```

3. Pak budete instanci klient management CDN jsme dodá naše přihlašovací údaje.

    ``` javascript
    var credentials = new msRestAzure.ApplicationTokenCredentials(clientId, tenantId, clientSecret);
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```
    
    Pokud používáte ověřování jednotlivých uživatelů, bude vypadat mírně odlišnou tyto dva řádky.

    >[AZURE.IMPORTANT] Tento příklad kód použijte, pouze pokud jste zvolili možnost ověření jednotlivé uživatele místo služby základní.  Dávejte chránit pověření jednotlivé uživatele a zachovat skryté.

    ``` javascript
    var credentials = new msRestAzure.UserTokenCredentials(clientId, 
        tenantId, '<username>', '<password>', '<redirect URI>');
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```

    Nezapomeňte místo položek v ** &lt;lomenými závorkami&gt; ** správné informace.  Pro `<redirect URI>`, použijte přesměrování URI jste zadali při registraci aplikace v Azure AD.
    

4.  Náš aplikace konzoly Node.js bude trvat některé parametry příkazového řádku.  Pojďme ověřit, které aspoň jeden předán parametr.

    ```javascript
    //Collect command-line parameters
    var parms = process.argv.slice(2);

    //Do we have parameters?
    if(parms == null || parms.length == 0)
    {
        console.log("Not enough parameters!");
        console.log("Valid commands are list, delete, create, and purge.");
        process.exit(1);
    }
    ```

5. Která přináší nám hlavní část náš program, které jsme větví na ostatní funkce podle předané jaké parametry.

    ```javascript
    switch(parms[0].toLowerCase())
    {
        case "list":
            cdnList();
            break;

        case "create":
            cdnCreate();
            break;
        
        case "delete":
            cdnDelete();
            break;

        case "purge":
            cdnPurge();
            break;

        default:
            console.log("Valid commands are list, delete, create, and purge.");
            process.exit(1);
    }
    ```

6.  Na několika místech v našem programu budete potřebujeme zkontrolujte, jestli správný počet parametrů předané a zobrazit trochu pomoct, pokud není správná.  Vytvoření funkce to udělat.

    ```javascript
    function requireParms(parmCount) {
        if(parms.length < parmCount) {
            usageHelp(parms[0].toLowerCase());
            process.exit(1);
        }
    }

    function usageHelp(cmd) {
        console.log("Usage for " + cmd + ":");
        switch(cmd)
        {
            case "list":
                console.log("list profiles");
                console.log("list endpoints <profile name>");
                break;

            case "create":
                console.log("create profile <profile name>");
                console.log("create endpoint <profile name> <endpoint name> <origin hostname>");
                break;
            
            case "delete":
                console.log("delete profile <profile name>");
                console.log("delete endpoint <profile name> <endpoint name>");
                break;

            case "purge":
                console.log("purge <profile name> <endpoint name> <path>");
                break;

            default:
                console.log("Invalid command.");
        }
    }
    ```

7. Nakonec funkce, které budeme používat na straně klienta správy CDN jsou asynchronní potřebují metodu pro volání zpět, když máte hotovo.  Pojďme vytvořit, můžete zobrazit výstup z klienta správy CDN (pokud existuje) a ukončete aplikaci řádně.

    ```javascript
    function callback(err, result, request, response) {
        if (err) {
            console.log(err);
            process.exit(1);
        } else {
            console.log((result == null) ? "Done!" : result);
            process.exit(0);
        }
    }
    ```

Teď číslovka základní struktura náš program vytvoříme by měl s názvem založené na naše parametrů funkce.

## <a name="list-cdn-profiles-and-endpoints"></a>Seznam CDN profily a koncové body

Začněme s kódem seznam naši stránku věnovanou existujících profilů a koncové body.  Vlastní kód komentář poskytují očekávané syntaxi tak nám vědět, kde každý parametr přejde.

```javascript
// list profiles
// list endpoints <profile name>
function cdnList(){
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        case "profiles":
            console.log("Listing profiles...");
            cdnClient.profiles.listByResourceGroup(resourceGroupName, callback);
            break;

        case "endpoints":
            requireParms(3);
            console.log("Listing endpoints...");
            cdnClient.endpoints.listByProfile(parms[2], resourceGroupName, callback);
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}
```

## <a name="create-cdn-profiles-and-endpoints"></a>Vytvoření CDN profily a koncové body

Pak budete psaní funkcí pro vytváření profilů a koncové body.

```javascript
function cdnCreate() {
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        case "profile":
            cdnCreateProfile();
            break;

        case "endpoint":
            cdnCreateEndpoint();
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}

// create profile <profile name>
function cdnCreateProfile() {
    requireParms(3);
    console.log("Creating profile...");
    var standardCreateParameters = {
        location: resourceLocation,
        sku: {
            name: 'Standard_Verizon'
        }
    };

    cdnClient.profiles.create(parms[2], standardCreateParameters, resourceGroupName, callback);
}

// create endpoint <profile name> <endpoint name> <origin hostname>        
function cdnCreateEndpoint() {
    requireParms(5);
    console.log("Creating endpoint...");
    var endpointProperties = {
        location: resourceLocation,
        origins: [{
            name: parms[4],
            hostName: parms[4]
        }]
    };

    cdnClient.endpoints.create(parms[3], endpointProperties, parms[2], resourceGroupName, callback);
}
```

## <a name="purge-an-endpoint"></a>Odstranění koncového bodu

Za předpokladu, že byl vytvořen koncový bod, je jeden běžné úkol, který jsme možná budete chtít v naší aplikaci provést vymazání obsahu v naší koncového bodu.

```javascript
// purge <profile name> <endpoint name> <path>
function cdnPurge() {
    requireParms(4);
    console.log("Purging endpoint...");
    var purgeContentPaths = [ parms[3] ];
    cdnClient.endpoints.purgeContent(parms[2], parms[1], resourceGroupName, purgeContentPaths, callback);
}
```

## <a name="delete-cdn-profiles-and-endpoints"></a>Odstranění CDN profily a koncové body

Na poslední funkci, kterou jsme bude obsahovat odstraní profily a koncové body.

```javascript
function cdnDelete() {
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        // delete profile <profile name>
        case "profile":
            requireParms(3);
            console.log("Deleting profile...");
            cdnClient.profiles.deleteIfExists(parms[2], resourceGroupName, callback);
            break;

        // delete endpoint <profile name> <endpoint name>
        case "endpoint":
            requireParms(4);
            console.log("Deleting endpoint...");
            cdnClient.endpoints.deleteIfExists(parms[3], parms[2], resourceGroupName, callback);
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}
```

## <a name="running-the-program"></a>Spuštění programu

Jsme nyní lze spustit naše Node.js programu pomocí našich Oblíbené ladění nebo v konzole.

> [AZURE.TIP] Pokud používáte jako svého ladění Visual Studio kód, musíte nastavit prostředí pro předávání parametry příkazového řádku.  Visual Studio kód to dělá v souboru **lanuch.json** .  Vyhledejte vlastnost s názvem **argumenty** a přidejte maticových řetězcové hodnoty parametrů, takže vypadá nějak takto: `"args": ["list", "profiles"]`.

Začneme tím, že seznam naše profily.

![Seznam profilů](./media/cdn-app-dev-node/cdn-list-profiles.png)

Jsme máte zpět prázdné pole.  Vzhledem k tomu nemáme žádné profilů v naší pole Skupina zdroje, které by měl být.  Vytvoření profilu nyní.

![Vytvoření profilu](./media/cdn-app-dev-node/cdn-create-profile.png)

Teď Pojďme přidat koncový bod.

![Vytvoření koncového bodu](./media/cdn-app-dev-node/cdn-create-endpoint.png)

Nakonec Pojďme odstranění naše profilu.

![Odstranění profilu](./media/cdn-app-dev-node/cdn-delete-profile.png)

## <a name="next-steps"></a>Další kroky

Chcete-li zobrazit hotového projektu z tohoto návodu, [Stáhněte si vzorku](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74).

Odkaz pro Azure CDN SDK pro Node.js zobrazíte zobrazení [odkazu](http://azure.github.io/azure-sdk-for-node/azure-arm-cdn/latest/).

Najít další si přečtěte následující dokumentaci na Azure SDK Node.js, zobrazte [úplný odkaz](http://azure.github.io/azure-sdk-for-node/).

Správa zdrojů CDN pomocí [prostředí PowerShell](./cdn-manage-powershell.md).

