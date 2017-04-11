<properties
    pageTitle="Azure AD B2C: Zabezpečeného webového rozhraní API pomocí Node.js | Microsoft Azure"
    description="Vytvoření Node.js webového rozhraní API, která přijímá tokenů z klienta B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="hero-article"
    ms.date="08/30/2016"
    ms.author="brandwe"/>

# <a name="azure-ad-b2c-secure-a-web-api-by-using-nodejs"></a>Azure AD B2C: Zabezpečeného webového rozhraní API pomocí Node.js

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

S Azure Active Directory (Azure AD) B2C můžete zabezpečit webového rozhraní API pomocí tokeny aplikace access OAuth 2.0. Těchto tokenů povolit klientské aplikace, které používají Azure AD B2C ověření rozhraní API. Tento článek popisuje, jak vytvořit "seznam úkolů" rozhraní API, který umožňuje uživatelům a přidejte seznam úkolů. Na webu rozhraní API je zabezpečeným pomocí Azure AD B2C a umožňuje pouze ověřené uživatele ke správě seznamu úkolů.

> [AZURE.NOTE]  V tomto příkladu napsané být připojeni k pomocí našich [iOS B2C ukázková aplikace](active-directory-b2c-devquickstarts-ios.md). Nejdřív se aktuální walk-through a postupujte podle spolu s této ukázkové.

**Passport** je middleware ověřování pro Node.js. Flexibilní a modulární, Passport je možné unobtrusively nainstalovat v některém expresní nebo Restify webové aplikace. Komplexní sady strategie podporuje ověřování pomocí uživatelského jména a hesla, Facebook, Twitter apod. Jsme vytvořili strategie pro službu Azure Active Directory (Azure AD). Tento modul nainstalujete a potom přidáte Azure AD `passport-azure-ad` modulu plug-in.

V tomto příkladu proveďte potřebujete:

1. Registrace aplikace s Azure AD.
2. Nastavení aplikace pomocí prvku Passport `azure-ad-passport` modulu plug-in.
3. Konfigurace klientské aplikace k volání "seznam úkolů" webového rozhraní API.


## <a name="get-an-azure-ad-b2c-directory"></a>Získat Azure AD B2C adresář

Než budete moct použít Azure AD B2C, musíte vytvořit adresář nebo klient.  Adresář je kontejner pro všechny uživatele, aplikace, skupiny a další.  Pokud nemáte už, [vytvořte adresář B2C](active-directory-b2c-get-started.md) před pokračovat.

## <a name="create-an-application"></a>Vytvoření aplikace

Dál je potřeba vytvořit aplikace ve vašem adresáři B2C, která nabízí Azure AD některé informace, které potřebuje k bezpečnému komunikovat s aplikací. V tomto případě klientské aplikace a webového rozhraní API představují jednoho **ID aplikace**, protože představují jedné logické aplikaci. Vytvoření aplikace pro, postupujte podle [těchto pokynů](active-directory-b2c-app-registration.md). Nezapomeňte:

- Zahrnout **webové aplikace nebo webové rozhraní api** v aplikaci
- Zadejte `http://localhost/TodoListService` jako **Adresa URL odpověď**. Je výchozí adresu URL Tato ukázka kódu.
- Vytvoření **aplikace tajná** aplikace a zkopírujte je. Budete potřebovat tato data později. Všimněte si, že tato hodnota musí být před použitím [uvést XML](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) .
- Zkopírujte **ID aplikace** přiřazeného k aplikaci. Budete potřebovat tato data později.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Vytvoření pravidla

V Azure AD B2C každé uživatelské prostředí je definován [zásad](active-directory-b2c-reference-policies.md). Tato aplikace obsahuje dvěma prostředími identity: registrace a přihlaste se. Je potřeba vytvořit jednu zásadu každého typu popsanou v [článku Přehled zásad](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy).  Při vytváření tři zásady nezapomeňte:

- Zvolte **zobrazované jméno** a dalších registrace atributů registrace zásad.
- V každé zásady zvolte deklarace aplikace **zobrazované jméno** a **ID objektu** .  Můžete použít i jiné deklarované.
- Zkopírujte si **název** každého zásadu po jeho vytvoření. By měla být předponu `b2c_1_`.  Budete potřebovat názvy zásad později.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Po vytvoření tři zásad, budete chtít vytvořit aplikace.

Další informace o tom, jak fungují zásady v Azure AD B2C, začněte s [.NET webové aplikace Začínáme kurzu](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="download-the-code"></a>Stáhněte si tento kód

Kód tohoto kurzu [spravován na GitHub](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS). K vytvoření ukázky průběžně, můžete si [Stáhnout kostry projekt jako soubor ZIP](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/skeleton.zip). Můžete taky zkopírovat kostra:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS.git
```

Dokončené aplikace je také [k dispozici jako soubor ZIP](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/complete.zip) nebo `complete` větev stejném úložišti.

## <a name="download-nodejs-for-your-platform"></a>Stažení Node.js pro svoji platformu

Úspěšně použít tato ukázková, musíte spuštěna instalace Node.js. 

Nainstalujte Node.js z [nodejs.org](http://nodejs.org).

## <a name="install-mongodb-for-your-platform"></a>Instalace MongoDB pro svoji platformu

Úspěšně použít tato ukázková, musíte spuštěna instalace MongoDB. Použijeme MongoDB být rozhraní REST API trvalý celé instance serveru.

Nainstalujte MongoDB z [mongodb.org](http://www.mongodb.org).

> [AZURE.NOTE] Tento walk-through předpokládá použít výchozí server a instalaci koncové body pro MongoDB, tedy v době tento psaní `mongodb://localhost`.

## <a name="install-the-restify-modules-in-your-web-api"></a>Nainstalovat moduly Restify na webu rozhraní API

Vytvoření rozhraní REST API používáme Restify. Restify minimální a flexibilním framework aplikace Node.js pochází z Express. Má robustní sadu funkcí pro vytváření REST API přes připojení.

### <a name="install-restify"></a>Instalace Restify

Z příkazového řádku změňte adresář na `azuread`. Pokud `azuread` adresáře nemá neexistuje, vytvořte ji.

`cd azuread`nebo`mkdir azuread;`

Zadejte tento příkaz:

`npm install restify`

Nainstaluje Restify.

#### <a name="did-you-get-an-error"></a>Dojde k chybě?

V některých operačních systémů, když použijete `npm`, může se zobrazit chyba `Error: EPERM, chmod '/usr/local/bin/..'` a žádost o spuštění účet jako správce. Pokud dojde k potížím, použít `sudo` spuštění příkazu `npm` na vyšší úrovni oprávnění.

#### <a name="did-you-get-a-dtrace-error"></a>Zobrazila se vám chyba DTrace?

Může se objevit vypadá podobně jako tento text při instalaci Restify:

```Shell
clang: error: no such file or directory: 'HD/azuread/node_modules/restify/node_modules/dtrace-provider/libusdt'
make: *** [Release/DTraceProviderBindings.node] Error 1
gyp ERR! build error
gyp ERR! stack Error: `make` failed with exit code: 2
gyp ERR! stack     at ChildProcess.onExit (/usr/local/lib/node_modules/npm/node_modules/node-gyp/lib/build.js:267:23)
gyp ERR! stack     at ChildProcess.EventEmitter.emit (events.js:98:17)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (child_process.js:789:12)
gyp ERR! System Darwin 13.1.0
gyp ERR! command "node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /Volumes/Development HD/azuread/node_modules/restify/node_modules/dtrace-provider
gyp ERR! node -v v0.10.11
gyp ERR! node-gyp -v v0.10.0
gyp ERR! not ok
npm WARN optional dep failed, continuing dtrace-provider@0.2.8
```

Restify poskytuje výkonné mechanismus při sledování ZBÝVAJÍCÍ volá pomocí DTrace. Operačních systémech nemají DTrace k dispozici. Tyto chyby můžete ignorovat.

Výstup příkazu by měla vypadat podobně jako tento text:

    restify@2.6.1 node_modules/restify
    ├── assert-plus@0.1.4
    ├── once@1.3.0
    ├── deep-equal@0.0.0
    ├── escape-regexp-component@1.0.2
    ├── qs@0.6.5
    ├── tunnel-agent@0.3.0
    ├── keep-alive-agent@0.0.1
    ├── lru-cache@2.3.1
    ├── node-uuid@1.4.0
    ├── negotiator@0.3.0
    ├── mime@1.2.11
    ├── semver@2.2.1
    ├── spdy@1.14.12
    ├── backoff@2.3.0
    ├── formidable@1.0.14
    ├── verror@1.3.6 (extsprintf@1.0.2)
    ├── csv@0.3.6
    ├── http-signature@0.10.0 (assert-plus@0.1.2, asn1@0.1.11, ctype@0.5.2)
    └── bunyan@0.22.0 (mv@0.0.5)

## <a name="install-passport-in-your-web-api"></a>Instalace Passport na webu rozhraní API

Z příkazového řádku změňte adresář na `azuread`, pokud ještě není.

Nainstalujte Passport pomocí následujícího příkazu:

`npm install passport`

Výstup příkazu by měl být stejný tento text:

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## <a name="add-passport-azuread-to-your-web-api"></a>Přidání passport azuread do webového rozhraní API

Dále přidejte strategie OAuth pomocí `passport-azuread`, sady strategií, která se připojují Azure AD pomocí Passport. Pomocí této strategie pro nosný tokeny ve výběru rozhraní REST API.

> [AZURE.NOTE] Přestože OAuth2 poskytuje framework, ve kterém můžete vydána libovolný známých tokenu, jenom určité typy tokenů získali obecnému. Tokeny chránících koncové body jsou nosný tokeny. Tyto typy tokenů jsou nejčastěji vydané v OAuth2. Mnoho implementace předpokládají, že jsou tokeny nosný jediný druh token nebyl vystaven.

Z příkazového řádku změňte adresář na `azuread`, pokud ještě není.

Instalace pasu `passport-azure-ad` modul pomocí následujícího příkazu:

`npm install passport-azure-ad`

Výstup příkazu by měl být stejný tento text:

``
passport-azure-ad@1.0.0 node_modules/passport-azure-ad
├── xtend@4.0.0
├── xmldom@0.1.19
├── passport-http-bearer@1.0.1 (passport-strategy@1.0.0)
├── underscore@1.8.3
├── async@1.3.0
├── jsonwebtoken@5.0.2
├── xml-crypto@0.5.27 (xpath.js@1.0.6)
├── ursa@0.8.5 (bindings@1.2.1, nan@1.8.4)
├── jws@3.0.0 (jwa@1.0.1, base64url@1.0.4)
├── request@2.58.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, tunnel-agent@0.4.1, oauth-sign@0.8.0, isstream@0.1.2, extend@2.0.1, json-stringify-safe@5.0.1, node-uuid@1.4.3, qs@3.1.0, combined-stream@1.0.5, mime-types@2.0.14, form-data@1.0.0-rc1, http-signature@0.11.0, bl@0.9.4, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
└── xml2js@0.4.9 (sax@0.6.1, xmlbuilder@2.6.4)
``

## <a name="add-mongodb-modules-to-your-web-api"></a>Přidání modulů MongoDB do webového rozhraní API

Tento příklad používá MongoDB jako datový úložiště. Pro které instalace Mongoose, často používaný modul plug-in pro správu modely a schémat.

* `npm install mongoose`

## <a name="install-additional-modules"></a>Instalace další moduly

Potom nainstalujte zbývající požadované moduly.

Z příkazového řádku změňte adresář na `azuread`, pokud ještě není:

`cd azuread`

Nainstalovat moduly ve vaší `node_modules` directory:

* `npm install assert-plus`
* `npm install ejs`
* `npm install ejs-locals`
* `npm install express`
* `npm install bunyan`


## <a name="create-a-serverjs-file-with-your-dependencies"></a>Vytvoření souboru server.js s závislosti mezi úkoly

`server.js` Soubor obsahuje většina funkcí pro rozhraní API webových serveru. 

Z příkazového řádku změňte adresář na `azuread`, pokud ještě není:

`cd azuread`

Vytvoření `server.js` soubor v editoru. Přidejte tyto informace:

```Javascript
'use strict';
/**
* Module dependencies.
*/
var fs = require('fs');
var path = require('path');
var util = require('util');
var assert = require('assert-plus');
var mongoose = require('mongoose/');
var bunyan = require('bunyan');
var restify = require('restify');
var config = require('./config');
var passport = require('passport');
var OIDCBearerStrategy = require('passport-azure-ad').BearerStrategy;
```

Uložte soubor. Můžete do ní vrátíte později.

## <a name="create-a-configjs-file-to-store-your-azure-ad-settings"></a>Vytvoření souboru config.js uložit nastavení Azure AD

Tento soubor kód předá parametry konfigurace z portálu Azure AD pro `Passport.js` soubor. Když si vás přidá webového rozhraní API k portálu v první část walk-through vytvořené tyto hodnoty konfigurace. Jsme je vysvětleno, co chcete umístit do pole hodnoty parametrů po zkopírování kódu.

Z příkazového řádku změňte adresář na `azuread`, pokud ještě není:

`cd azuread`

Vytvoření `config.js` soubor v editoru. Přidejte tyto informace:

```Javascript
// Don't commit this file to your public repos. This config is for first-run
exports.creds = {
clientID: <your client ID for this Web API you created in the portal>
mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
audience: '<your audience URI>', // the Client ID of the application that is calling your API, usually a web API or native client
identityMetadata: 'https://login.microsoftonline.com/<tenant name>/.well-known/openid-configuration', // Make sure you add the B2C tenant name in the <tenant name> area
tenantName:'<tenant name>', 
policyName:'b2c_1_<sign in policy name>' // This is the policy you'll want to validate against in B2C. Usually this is your Sign-in policy (as users sign in to this API)
passReqToCallback: false // This is a node.js construct that lets you pass the req all the way back to any upstream caller. We turn this off as there is no upstream caller.
};

```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

### <a name="required-values"></a>Požadované hodnoty

`clientID`: Klient ID rozhraní API webových aplikací.

`IdentityMetadata`: Toto je umístění `passport-azure-ad` vyhledá poskytovatele identity pro typ vašich dat konfigurace. Taky hledá pomocí kláves ověřit web tokeny JSON. 

`audience`: Identifikátor URI (Uniform) z portálu Microsoft, který identifikuje volání aplikace. 

`tenantName`: Klienta názvu (například **contoso.onmicrosoft.com**).

`policyName`: Zásada, kterou chcete ověřit tokeny přichází do serveru. Tuto zásadu by měl být stejný zásad využívající na klientské aplikace pro přihlášení.

> [AZURE.NOTE] Náš B2C (verze Preview) použijte stejné zásady napříč nastavení klienta a serveru. Pokud jste už uskutečněné walk-through a vytvořili tyto zásady, nemusíte znovu. Protože dokončit walk-through neměli potřebujete nastavení nové zásady pro klienta kurzy na webu.

## <a name="add-configuration-to-your-serverjs-file"></a>Přidání konfiguračního do souboru server.js

Číst hodnoty z `config.js` souboru jste vytvořili, přidejte `.config` zařadit jako požadovaný zdroj v aplikaci a pak nastavte globální proměnné profilům ve `config.js` dokumentu.

Z příkazového řádku změňte adresář na `azuread`, pokud ještě není:

`cd azuread`

Otevřít `server.js` soubor v editoru. Přidejte tyto informace:

```Javascript
var config = require('./config');
```
Přidejte nový oddíl na `server.js` , který bude zahrnovat následující kód:

```Javascript
// We pass these options in to the ODICBearerStrategy.

var options = {
    // The URL of the metadata document for your app. We put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
    identityMetadata: config.creds.identityMetadata,
    clientID: config.creds.clientID,
    tenantName: config.creds.tenantName,
    policyName: config.creds.policyName,
    validateIssuer: config.creds.validateIssuer,
    audience: config.creds.audience,
    passReqToCallback: config.creds.passReqToCallback

};
```

Pak přidáte některé zástupné symboly pro uživatele získaná z naší aplikací, volající.

```Javascript
// array to hold logged in users and the current logged in user (owner)
var users = [];
var owner = null;
```

Pojďme si napřed a vytvořte naše protokolování příliš.

```Javascript
// Our logger
var log = bunyan.createLogger({
    name: 'Microsoft Azure Active Directory Sample'
});
```

## <a name="add-the-mongodb-model-and-schema-information-by-using-mongoose"></a>Přidání informací o MongoDB modelu a schématu pomocí Mongoose

Dřívější Příprava platí jako přenést tyto tři soubory spolupráce ve službě rozhraní REST API.

Pro tento walk-through pomocí MongoDB ukládání svých úkolů, jak bylo popsáno dříve.

V `config.js` soubor, název databáze **tasklist**. Tento název se taky umístění na konci `mongoose_auth_local` URL připojení. Vytvoření databáze předem v MongoDB nemusíte. Vytvoří databázi můžete při prvním spuštění aplikace serveru.

Jakmile serveru MongoDB databázi, kterou chcete použít, budete muset některé další kódu při tvorbě modelu a schématu serveru úkolů.

### <a name="expand-the-model"></a>Rozšíření modelu

Tento model schématu je jednoduché. Rozbalit podle potřeby.

`owner`: Kdo je přiřazených k úkolu. Tento objekt je **řetězec**.  

`Text`: Samotný úkol. Tento objekt je **řetězec**.

`date`: Datu, která je splatná úkolu. Tento objekt je **data a času**.

`completed`: Pokud úkol je dokončený. Tento objekt je **logické**.

### <a name="create-the-schema-in-the-code"></a>Vytvoření schématu kód

Z příkazového řádku změňte adresář na `azuread`, pokud ještě není:

`cd azuread`

Otevřít `server.js` soubor v editoru. Přidejte následující položky konfigurace následující informace:

```Javascript
// MongoDB setup
// Setup some configuration
var serverPort = process.env.PORT || 3000; // Note we are hosting our API on port 3000
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;

// Connect to MongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');

// Here we create a schema to store our tasks and users. Pretty simple schema for now.
var TaskSchema = new Schema({
    owner: String,
    Text: String,
    completed: Boolean,
    date: Date
});

// Use the schema to register a model
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```
Nejdřív vytvořte schématu a vytvořte modelu objektu, který používáte pro ukládání dat v rámci kód při definování **trasy**.

## <a name="add-routes-for-your-rest-api-task-server"></a>Přidání cest úkolu serveru rozhraní REST API

Teď, když máte modelu databáze pro práci s, přidejte trasy, který používáte pro rozhraní REST API serveru.

### <a name="about-routes-in-restify"></a>O směrování do Restify

Směruje pracovat v Restify způsobem pracují při použití zásobníku Express. Definování směruje pomocí identifikátor URI, který očekáváte klientské aplikace volání. 

Typické vzor pro směrování Restify je:

```Javascript
function createObject(req, res, next) {
// do work on Object
_object.name = req.params.object; // passed value is in req.params under object
///...
return next(); // keep the server going
}
....
server.post('/service/:add/:object', createObject); // calls createObject on routes that match this.
```

Restify a Express může poskytovat hlubší funkcí, například definování typů aplikace a udělat složité směrování přes různé koncové body. Pro účely tohoto kurzu budeme zachovat tyto trasy jednoduché.

#### <a name="add-default-routes-to-your-server"></a>Přidání výchozích postupů na serveru

Teď přidejte základní CRUD postupy **vytváření** a **seznam** pro naše rozhraní REST API. Jiné způsoby najdete v `complete` větev vzorku.

Z příkazového řádku změňte adresář na `azuread`, pokud ještě není:

`cd azuread`

Otevřít `server.js` soubor v editoru. Tady vidíte položky databáze, které jste udělali nad přidejte následující informace:

```Javascript
/**
 *
 * APIs for our REST Task server
 */

// Create a task

function createTask(req, res, next) {

    // Resitify currently has a bug which doesn't allow you to set default headers
    // This headers comply with CORS and allow us to mongodbServer our response to any origin

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    // Create a new task model, fill it up and save it to Mongodb
    var _task = new Task();

    if (!req.params.Text) {
        req.log.warn({
            params: req.params
        }, 'createTodo: missing task');
        next(new MissingTaskError());
        return;
    }

    _task.owner = owner;
    _task.Text = req.params.Text;
    _task.date = new Date();

    _task.save(function(err) {
        if (err) {
            req.log.warn(err, 'createTask: unable to save');
            next(err);
        } else {
            res.send(201, _task);

        }
    });

    return next();

}
```

```Javascript
/// Simple returns the list of TODOs that were loaded.

function listTasks(req, res, next) {
    // Resitify currently has a bug which doesn't allow you to set default headers
    // This headers comply with CORS and allow us to mongodbServer our response to any origin

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    log.info("listTasks was called for: ", owner);

    Task.find({
        owner: owner
    }).limit(20).sort('date').exec(function(err, data) {

        if (err)
            return next(err);

        if (data.length > 0) {
            log.info(data);
        }

        if (!data.length) {
            log.warn(err, "There is no tasks in the database. Add one!");
        }

        if (!owner) {
            log.warn(err, "You did not pass an owner when listing tasks.");
        } else {

            res.json(data);

        }
    });

    return next();
}
```


#### <a name="add-error-handling-for-the-routes"></a>Přidání zpracování chyb pro směrování

Přidejte některé zpracování chyb, abyste zpět k desktopovému klientovi tak, aby mohli porozumět komunikovat všech problémů, ke kterým dojít.

Přidejte následující kód:

```Javascript
///--- Errors for communicating something interesting back to the client
function MissingTaskError() {
restify.RestError.call(this, {
statusCode: 409,
restCode: 'MissingTask',
message: '"task" is a required parameter',
constructorOpt: MissingTaskError
});
this.name = 'MissingTaskError';
}
util.inherits(MissingTaskError, restify.RestError);
function TaskExistsError(owner) {
assert.string(owner, 'owner');
restify.RestError.call(this, {
statusCode: 409,
restCode: 'TaskExists',
message: owner + ' already exists',
constructorOpt: TaskExistsError
});
this.name = 'TaskExistsError';
}
util.inherits(TaskExistsError, restify.RestError);
function TaskNotFoundError(owner) {
assert.string(owner, 'owner');
restify.RestError.call(this, {
statusCode: 404,
restCode: 'TaskNotFound',
message: owner + ' was not found',
constructorOpt: TaskNotFoundError
});
this.name = 'TaskNotFoundError';
}
util.inherits(TaskNotFoundError, restify.RestError);
```


## <a name="create-your-server"></a>Vytvoření serveru

Teď jste definovali databáze a zavedený svoje směry. Na co pro vás je přidání instance serveru, která spravuje vaše hovory.

Restify Express poskytnutí hloubkové přizpůsobení serveru rozhraní REST API, ale tady používáme základní nastavení. 

```Javascript

**
 * Our Server
 */


var server = restify.createServer({
    name: "Microsoft Azure Active Directroy TODO Server",
    version: "2.0.1"
});

// Ensure we don't drop data on uploads
server.pre(restify.pre.pause());

// Clean up sloppy paths like //todo//////1//
server.pre(restify.pre.sanitizePath());

// Handles annoying user agents (curl)
server.pre(restify.pre.userAgentConnection());

// Set a per request bunyan logger (with requestid filled in)
server.use(restify.requestLogger());

// Allow 5 requests/second by IP, and burst to 10
server.use(restify.throttle({
    burst: 10,
    rate: 5,
    ip: true,
}));

// Use the common stuff you probably want
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
    mapParams: true
})); // Allows for JSON mapping to REST
server.use(restify.authorizationParser()); // Looks for authorization headers

// Let's start using Passport.js

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support


```
## <a name="add-the-routes-to-the-server-without-authentication"></a>Přidejte trasy serveru (bez ověřování)

```Javascript
server.get('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), listTasks);
server.get('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), listTasks);
server.get('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), getTask);
server.head('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), getTask);
server.post('/api/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
    session: false
}), createTask);
server.post('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), createTask);
server.del('/api/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), removeAll, function respond(req, res, next) {
    res.send(204);
    next();
});


// Register a default '/' handler

server.get('/', function root(req, res, next) {
    var routes = [
        'GET     /',
        'POST    /api/tasks/:owner/:task',
        'POST    /api/tasks (for JSON body)',
        'GET     /api/tasks',
        'PUT     /api/tasks/:owner',
        'GET     /api/tasks/:owner',
        'DELETE  /api/tasks/:owner/:task'
    ];
    res.send(200, routes);
    next();
});
```

```Javascript

server.listen(serverPort, function() {

    var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
    consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
    consoleMessage += '\n %s server is listening at %s';
    consoleMessage += '\n Open your browser to %s/api/tasks\n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
    consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';

    //log.info(consoleMessage, server.name, server.url, server.url, server.url);

});

``` 

## <a name="add-authentication-to-your-rest-api-server"></a>Přidání ověřování serveru rozhraní REST API

Teď, když máte spuštěný server rozhraní REST API, může být užitečné proti Azure AD.

Z příkazového řádku změňte adresář na `azuread`, pokud ještě není:

`cd azuread`

### <a name="use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>Použití OIDCBearerStrategy, která je součástí passport azure ad


> [AZURE.TIP]
Při psaní rozhraní API vždy propojení dat na něco jedinečné z tokenu, uživatel nemůže falšují. Pokud server uchovává úkol položky, nemá tak podle **ID objektu** uživatele v token (volat prostřednictvím token.oid), které jsou uvedeny v poli "vlastník". Tato hodnota zajišťuje přístup pouze tento uživatel své vlastní položky úkol. Obsahuje vystavení rozhraní API "vlastníka", takže externího uživatele můžete požádat o ostatních položek úkol, i když se ověření.

V dalším kroku pomocí strategie nosný, které jsou součástí `passport-azure-ad`.

```Javascript
var findById = function(id, fn) {
    for (var i = 0, len = users.length; i < len; i++) {
        var user = users[i];
        if (user.oid === id) {
            log.info('Found user: ', user);
            return fn(null, user);
        }
    }
    return fn(null, null);
};


var oidcStrategy = new OIDCBearerStrategy(options,
    function(token, done) {
        log.info('verifying the user');
        log.info(token, 'was the token retreived');
        findById(token.sub, function(err, user) {
            if (err) {
                return done(err);
            }
            if (!user) {
                // "Auto-registration"
                log.info('User was added automatically as they were new. Their sub is: ', token.oid);
                users.push(token);
                owner = token.oid;
                return done(null, token);
            }
            owner = token.sub;
            return done(null, user, token);
        });
    }
);

passport.use(oidcStrategy);
```

Passport používá stejný vzor pro všechny strategie. Předáte `function()` obsahující `token` a `done` jako parametry. Strategie vracejí pro vás, když to bude hotové, všechny své práce. By pak ukládat uživatele a uložit tokenu, takže nemusíte nezobrazovat jej.

> [AZURE.IMPORTANT]
Kód výše trvá všichni uživatelé, kteří se stane s ověřují k serveru. Tento proces se nazývá autoregistration. V části provozní servery Nenechte v libovolné uživatelům přístup rozhraní API aniž by bylo nejdříve je absolvovat proces registrace. Tento proces se obvykle vzorku, který se zobrazí v aplikacích příjemce, které umožňují zaregistrovat pomocí služby Facebook, ale pak vás vyzve k vyplňte další informace o. Pokud tento program nebyl příkazového řádku programu, jsme může extrahovaných e-mailu z tokenu objekt, který je vrácena a potom vyzváni uživatelům vyplňte další informace. Protože je to výběru, jsme je přidat do databáze aplikace v paměti.



## <a name="run-your-server-application-to-verify-that-it-rejects-you"></a>Spusťte aplikaci serveru pro ověření, že odmítne můžete

Můžete použít `curl` zobrazíte Teď máte OAuth2 ochrany proti koncových bodů. Záhlaví vrácena by měl být dost vám to říct, že používáte správné cestě.

Ujistěte se, jestli je spuštěný MongoDB instance:

    $sudo mongodb

Přejděte do adresáře a spusťte serveru:

    $ cd azuread
    $ node server.js

V novém okně terminálu, spusťte`curl`

Vyzkoušejte základní příspěvku:

`$ curl -isS -X POST http://127.0.0.1:3000/api/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

Chyba 401 je odpověď, kterou chcete. Znamená to, že vrstvy Passport pokouší přesměrovat koncový bod udělit oprávnění.


## <a name="you-now-have-a-rest-api-service-that-uses-oauth2"></a>Teď máte službu rozhraní REST API, která používá OAuth2

Rozhraní REST API implementovali pomocí Restify a OAuth! Nyní máte dostatečnou kód tak, že můžete dál vývoje služby a sestavení v tomto příkladu. Máte pryč jde můžete provést tyto akce se tento server bez použití kompatibilní s prohlížečem OAuth2 klienta. Pro každý další krok použijte další walk-through jako naše [připojení k webu rozhraní API pomocí iOS B2C](active-directory-b2c-devquickstarts-ios.md) návodu.


## <a name="next-steps"></a>Další kroky

Teď přejdete k pokročilejší témata, například:

[Připojení k webu rozhraní API pomocí B2C iOS](active-directory-b2c-devquickstarts-ios.md)