<properties
    pageTitle="Azure AD verze 2.0 NodeJS webového rozhraní API | Microsoft Azure"
    description="Vytvoření rozhraní API webových NodeJS přijímá tokenů z obou osobní Account Microsoft a pracovní nebo školní účty."
    services="active-directory"
    documentationCenter="nodejs"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="secure-a-web-api-using-nodejs"></a>Zabezpečené rozhraní API webových pomocí node.js

> [AZURE.NOTE]
    Některé scénáře Azure Active Directory a funkce jsou podporovány koncový bod verze 2.0.  Pokud chcete zjistit, zda by měly používat koncový bod verze 2.0, přečtěte si informace o [omezení verze 2.0](active-directory-v2-limitations.md).

Službou Azure Active Directory, koncový bod verze 2.0 můžete chránit rozhraní API webových používání tokenů aplikace access [OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow) povolení uživatele s osobní účet Microsoft i pracovní nebo školní účty, na bezpečně přístup k rozhraní API webových.

**Passport** je middleware ověřování pro Node.js. Velmi flexibilní a moduly, Passport unobtrusively vyřadit každém Express založený nebo Resitify webové aplikace. Komplexní sady strategie podporuje ověřování pomocí uživatelské jméno a heslo, Facebook, Twitter a další. Jsme vytvořili strategie pro Microsoft Azure Active Directory. Budeme se nainstaluje tento modul a pak přidejte Microsoft Azure Active Directory `passport-azure-ad` modulu plug-in.

## <a name="download"></a>Ke stažení
Kód pro účely tohoto návodu spravován [na GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs).  Postup, můžete [Stáhnout osnovu v aplikaci jako ZIP](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/skeleton.zip) nebo klonovat kostra:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

Dokončené aplikace je k dispozici na konci kurzu, stejně.


## <a name="1-register-an-app"></a>1. zaregistrovat aplikace
Vytvoření nové aplikace na [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)nebo postupujte podle těchto [podrobných pokynů](active-directory-v2-app-registration.md).  Zkontrolujte, že:

- Kopírovat dolů **Id aplikace** přiřazené k aplikaci, musíte ho brzy bude k dispozici.
- Přidání platformu **mobilní** aplikace.
- Zkopírujte dolů **Přesměrovat URI** z portálu. Musíte použít výchozí hodnoty `urn:ietf:wg:oauth:2.0:oob`.


## <a name="2-download-nodejs-for-your-platform"></a>2: stažení node.js pro svoji platformu
Úspěšně použít tato ukázková, musíte mít spuštěna instalace Node.js.

Nainstalujte Node.js z [http://nodejs.org](http://nodejs.org).

## <a name="3-install-mongodb-on-to-your-platform"></a>3: instalace MongoDB k svoji platformu

Úspěšně použít tato ukázková, musíte mít funkční instalací MongoDB. Použijeme MongoDB být naše rozhraní REST API trvalá celé instance serveru.

Nainstalujte MongoDB z [http://mongodb.org](http://www.mongodb.org).

> [AZURE.NOTE] Tento návod předpokládá použít výchozí server a instalaci koncové body pro MongoDB, tedy v době tento psaní: mongodb://localhost

## <a name="4-install-the-restify-modules-in-to-your-web-api"></a>4: nainstalovat moduly Restify v rozhraní API webových

Použijeme Resitfy vytvářet naše rozhraní REST API. Restify minimální a flexibilním framework aplikace Node.js pochází z Express obsahující robustní sadu funkcí pro vytváření REST API přes připojení.

### <a name="install-restify"></a>Instalace Restify

Z příkazového řádku přejděte do adresáře azuread adresáře. Pokud v adresáři **azuread** neexistuje, vytvořte ji.

`cd azuread`– nebo –`mkdir azuread;`

Zadejte tento příkaz:

`npm install restify`

Nainstaluje Restify.

#### <a name="did-you-get-an-error"></a>Dojde k chybě?

Pokud chcete použít npm u některých operačních systémů, může zobrazit chybová chyby: EPERM chmod "/ usr/místní/Koš /..." a žádost o spusťte účet jako správce. V takovém případě příkazem sudo spustit npm na vyšší úrovni oprávnění.

#### <a name="did-you-get-an-error-regarding-dtrace"></a>Zobrazila se vám chyba týkající se DTrace?

Při instalaci Restify se může zobrazit přibližně takto:

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


Restify poskytuje mechanismus výkonné trasování ZBÝVAJÍCÍ volání pomocí DTrace. Operačních systémech nemají DTrace k dispozici. Tyto chyby můžete ignorovat.


Výstup tohoto příkazu by měla vypadat podobně jako tento:


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
    └── bunyan@0.22.0(mv@0.0.5)


## <a name="5-install-passportjs-into-your-web-api"></a>5: instalace Passport.js do webového rozhraní API

[Passport](http://passportjs.org/) je middleware ověřování pro Node.js. Velmi flexibilní a moduly, Passport unobtrusively vyřadit každém Express založený nebo Resitify webové aplikace. Komplexní sady strategie podporuje ověřování pomocí uživatelské jméno a heslo, Facebook, Twitter a další. Jsme vytvořili strategie pro službu Azure Active Directory. Budeme se nainstaluje tento modul a přidejte plug-in strategie Azure Active Directory.

Z příkazového řádku přejděte do adresáře azuread adresáře.

Zadejte následující příkaz a nainstalujte passport.js

`npm install passport`

Výstup příkazu by měla vypadat podobně jako tento:

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## <a name="6-add-passport-azure-ad-to-your-web-api"></a>6: Přidání Passport Azure AD do webového rozhraní API

Pak přidáme strategie OAuth pomocí passport-azuread, sady strategií, která se připojují Azure Active Directory s Passport. Použijeme tento strategie pro nosný tokeny v tomto příkladu rozhraní Rest API.

> [AZURE.NOTE] Přestože OAuth2 poskytuje framework, ve kterém můžete vydána libovolný známé tokenu, jenom určité typy tokenů získali wide šíření použití. Pro ochranu koncové body, které má být nosný tokeny vypadá. Tokeny nosný jsou nejčastěji široce Vystavitel typ token v OAuth2 a mnoho implementace předpokládají, že jsou tokeny nosný jediný druh token nebyl vystaven.

Z příkazového řádku přejděte do adresáře azuread adresáře

Zadejte následující příkaz a nainstalujte modul passport azure ad Passport.js:

`npm install passport-azure-ad`

Výstup příkazu by měla vypadat podobně jako tento:

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

## <a name="7-add-mongodb-modules-to-your-web-api"></a>7: Přidání MongoDB modulů pro rozhraní API webových

Použijeme MongoDB jako naše úložiště z toho důvodu, potřebujeme nainstalovat obou možnostech sloužící ke správě modely a schémata modul plug-in se nazývá Mongoose, jakož i ovladače pro MongoDB, taky se mu říká MongoDB.


* `npm install mongoose`
* `npm install mongodb`

## <a name="8-install-additional-modules"></a>8: instalace další modulů

Dále jsme budete instalovat zbývajících požadovaných moduly.


Z příkazového řádku změňte adresáře ke složce **azuread** v opačném případě už není:

`cd azuread`


Zadejte následující příkazy k instalaci modulů v adresáři node_modules:

* `npm install crypto`
* `npm install assert-plus`
* `npm install posix-getopt`
* `npm install util`
* `npm install path`
* `npm install connect`
* `npm install xml-crypto`
* `npm install xml2js`
* `npm install xmldom`
* `npm install async`
* `npm install request`
* `npm install underscore`
* `npm install grunt-contrib-jshint@0.1.1`
* `npm install grunt-contrib-nodeunit@0.1.2`
* `npm install grunt-contrib-watch@0.2.0`
* `npm install grunt@0.4.1`
* `npm install xtend@2.0.3`
* `npm install bunyan`
* `npm update`


## <a name="9-create-a-serverjs-with-your-dependencies"></a>9: vytvoření server.js s závislosti mezi úkoly

Soubor server.js poskytovat většinou náš funkce pro naše rozhraní API webových server. Jsme na tento soubor přidávání většina naše kódu. Pro účely výrobní by Refaktorovat funkci v menší soubory, jako jsou samostatné směruje a řadiče. Pro účely Tato ukázka používáme server.js pro tuto funkci.

Z příkazového řádku změňte adresáře ke složce **azuread** v opačném případě už není:

`cd azuread`

Vytvoření `server.js` soubor v naší Oblíbené editoru a přidejte následující informace:

```Javascript
'use strict';
/**
* Module dependencies.
*/
var util = require('util');
var assert = require('assert-plus');
var mongoose = require('mongoose/');
var bunyan = require('bunyan');
var restify = require('restify');
var config = require('./config');
var passport = require('passport');
var OIDCBearerStrategy = require('passport-azure-ad').OIDCStrategy;
```

Uložte soubor. Budeme se brzy vrátí na to.

## <a name="10-create-a-config-file-to-store-your-azure-ad-settings"></a>10: vytvoření konfiguračního souboru pro ukládání Azure AD nastavení

Tento soubor kód předává parametry konfigurace z portálu Azure Active Directory Passport.js. Když si vás přidá rozhraní API webových na portál v první část návodu vytvořené tyto hodnoty konfigurace. Jsme vysvětluje, co chcete umístit do pole hodnoty parametrů po zkopírování kódu.


Z příkazového řádku změňte adresáře ke složce **azuread** v opačném případě už není:

`cd azuread`

Vytvoření `config.js` soubor v naší Oblíbené editoru a přidejte následující informace:

```Javascript
// Don't commit this file to your public repos. This config is for first-run
exports.creds = {
mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
issuer: 'https://sts.windows.net/**<your application id>**/',
audience: '<your redirect URI>',
identityMetadata: 'https://login.microsoftonline.com/common/.well-known/openid-configuration' // For using Microsoft you should never need to change this.
};

```



### <a name="required-values"></a>Požadované hodnoty

*IdentityMetadata*: Toto je místo, kam bude vypadat passport azure ad pro typ vašich dat konfigurace IdP i klávesy pro ověřování tokeny JWT. Pravděpodobně nechcete změnit pomocí služby Azure Active Directory.

*cílové skupiny*: přesměrování URI z portálu.

> [AZURE.NOTE]
Doporučujeme vrátit naše klíčů v pravidelných intervalech. Ujistěte se, že je vždy zavedení z adresy URL "openid_keys" a že aplikaci přístup k Internetu.


## <a name="11-add-configuration-to-your-serverjs-file"></a>11: Přidání konfigurace do souboru server.js

Potřebujeme číst tyto hodnoty z konfiguračního souboru, který jste právě vytvořili napříč naše aplikace. K tomuto účelu můžeme jednoduše přidat souboru config jako požadovaný zdroj v aplikaci a poté nastavte globální proměnné odkazy v dokumentu config.js

Z příkazového řádku změňte adresáře ke složce **azuread** v opačném případě už není:

`cd azuread`

Otevřete svůj `server.js` soubor v naší Oblíbené editoru a přidejte následující informace:

```Javascript
var config = require('./config');
```
Potom přidejte nový oddíl na `server.js` s kódem takto:

```Javascript
// We pass these options in to the ODICBearerStrategy.
var options = {
// The URL of the metadata document for your app. We will put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
identityMetadata: config.creds.identityMetadata,
issuer: config.creds.issuer,
audience: config.creds.audience
};
// array to hold logged in users and the current logged in user (owner)
var users = [];
var owner = null;
// Our logger
var log = bunyan.createLogger({
name: 'Microsoft Azure Active Directory Sample'
});
```

## <a name="step-12-add-the-mongodb-model-and-schema-information-using-moongoose"></a>Krok 12: Přidání MongoDB modelu a schématu informací pomocí Moongoose

Teď tento Příprava bude zahájíte platebních, jak jsme větru tyto tři soubory spolupráce ve službě rozhraní REST API.

V tomto návodu použijeme MongoDB pro ukládání naše úkoly, jak je uvedeno v ***kroku 4***.

Pokud vyvoláte ze zdrojového souboru config.js, kterou jsme vytvořili v kroku 11 jsme s názvem naše databáze *tasklist*stejně jako, co jsme zadávat na konci adresa URL mogoose_auth_local připojení. Není potřeba vytvářet tuto databázi předem v MongoDB, je to pro vytvoření cz při prvním spuštění aplikace náš server (za předpokladu, že už neexistuje).

Teď můžeme jste v aplikaci serveru jaké MongoDB databáze nám chcete použít, potřebujeme některé další kódu při tvorbě modelu a schématu pro naše serveru úkoly.

#### <a name="discussion-of-the-model"></a>Diskusní modelu

Našeho schématu modelu je velmi jednoduché a rozbalte podle potřeby.

NÁZEV - název kdo přiřazených k úkolu. ***Řetězec***

ÚKOL – samotný úkol. ***Řetězec***

Datum - datum, které je daný úkol termín. ***Data a času***

DOKONČENO – Pokud je úkol dokončen nebo ne. ***LOGICKÉ***

#### <a name="creating-the-schema-in-the-code"></a>Vytváření schématu kód


Z příkazového řádku změňte adresáře ke složce **azuread** v opačném případě už není:

`cd azuread`

Otevřete svůj `server.js` souborů v naše Oblíbené editoru a přidejte tyto informace pod položkou konfigurace:

```Javascript
// MongoDB setup
// Setup some configuration
var serverPort = process.env.PORT || 8080;
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
// Connect to MongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');
```
Tím se připojit k serveru MongoDB a rukou zpět objekt schématu námi.

#### <a name="using-the-schema-create-our-model-in-the-code"></a>Pomocí schématu vytvořte našeho modelu kód

Pod kód, který jste sami napsali nad přidejte tento kód:

```Javascript
// Here we create a schema to store our tasks and users. Pretty simple schema for now.
var TaskSchema = new Schema({
owner: String,
task: String,
completed: Boolean,
date: Date
});
// Use the schema to register a model
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```
Jak odlišíte od kód, jsme náš schématu a potom modelu objektu, který použijeme pro uložení naše data v celém kód, když je definována náš ***trasy***.

## <a name="step-13-add-our-routes-for-our-task-rest-api-server"></a>Krok 13: Přidání naše směruje pro naše server úkolu REST API

Teď, máme modelu databáze pro práci s, přidejte trasy, které použijeme naše rozhraní REST API serveru.

### <a name="about-routes-in-restify"></a>O směrování v Restify

Směruje pracovat v Restify přesně stejným způsobem, který přehrávají použití zásobníku Express. Definování postupy pomocí identifikátor URI, který očekáváte klientské aplikace volání. Obvykle definujete svoje směry do samostatného souboru. Naše potřeby jsme v souboru server.js umístí naše trasy. Doporučujeme, abyste že tyto faktor své vlastní souboru pro použití v praxi.

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


Toto je vzorek základní úrovni. Resitfy (a Express) zadejte objem hlubší functionaltiy například definování typů aplikace a dělají složité směrování přes různé koncové body. Pro naše potřeby jsme zachová tyto trasy velmi jednoduše.

#### <a name="add-default-routes-to-our-server"></a>Přidání výchozí trasy naše server

Jsme teď přidáte základní CRUD směruje vytvořit, načíst, aktualizovat a odstranit.

Z příkazového řádku změňte adresáře ke složce **azuread** v opačném případě už není:

`cd azuread`

Otevřete svůj `server.js` souborů v naší Oblíbené editoru a přidejte tyto informace pod jste provedli nad položek databáze.:

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
if (!req.params.task) {
req.log.warn({
params: p
}, 'createTodo: missing task');
next(new MissingTaskError());
return;
}
_task.owner = owner;
_task.task = req.params.task;
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
// Delete a task by name
function removeTask(req, res, next) {
Task.remove({
task: req.params.task,
owner: owner
}, function(err) {
if (err) {
req.log.warn(err,
'removeTask: unable to delete %s',
req.params.task);
next(err);
} else {
log.info('Deleted task:', req.params.task);
res.send(204);
next();
}
});
}
// Delete all tasks
function removeAll(req, res, next) {
Task.remove();
res.send(204);
return next();
}
// Get a specific task based on name
function getTask(req, res, next) {
log.info('getTask was called for: ', owner);
Task.find({
owner: owner
}, function(err, data) {
if (err) {
req.log.warn(err, 'get: unable to read %s', owner);
next(err);
return;
}
res.json(data);
});
return next();
}
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

### <a name="add-some-error-handling-for-the-routes"></a>Přidání některé zpracování chyb ve vzorcích pro trasy

Dává smysl přidáte některé zpracování chyb tak jsme komunikovat klientovi problém, který jsme narazili způsobem můžete srozumitelný.

Přidejte následující kód pod kód, který jste napsali nad:

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


## <a name="step-14-create-your-server"></a>Krok 14: Vytvoření serveru!

Máme naši databázi definované, máme naše tras na místě a posledním krokem je přidat naše instanci serveru, která bude spravovat naše volání.

Restify (a Express) je značné množství hloubkové přizpůsobení můžete udělat pro server rozhraní REST API, ale znovu použijeme základní nastavení pro naše potřeby.

```Javascript
/**
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
}));
```
## <a name="15-adding-the-routes-without-authentication-for-now"></a>15: přidávání tras (bez ověřování pro tuto)

```Javascript
/// Now the real handlers. Here we just CRUD
/**
/*
/* Each of these handlers are protected by our OIDCBearerStrategy by invoking 'oidc-bearer'
/* in the pasport.authenticate() method. We set 'session: false' as REST is stateless and
/* we don't need to maintain session state. You can experiement removing API protection
/* by removing the passport.authenticate() method like so:
/*
/* server.get('/tasks', listTasks);
/*
**/
server.get('/tasks', listTasks);
server.get('/tasks', listTasks);
server.get('/tasks/:owner', getTask);
server.head('/tasks/:owner', getTask);
server.post('/tasks/:owner/:task', createTask);
server.post('/tasks', createTask);
server.del('/tasks/:owner/:task', removeTask);
server.del('/tasks/:owner', removeTask);
server.del('/tasks', removeTask);
server.del('/tasks', removeAll, function respond(req, res, next) {
res.send(204);
next();
});
// Register a default '/' handler
server.get('/', function root(req, res, next) {
var routes = [
'GET /',
'POST /tasks/:owner/:task',
'POST /tasks (for JSON body)',
'GET /tasks',
'PUT /tasks/:owner',
'GET /tasks/:owner',
'DELETE /tasks/:owner/:task'
];
res.send(200, routes);
next();
});
server.listen(serverPort, function() {
var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
consoleMessage += '\n %s server is listening at %s';
consoleMessage += '\n Open your browser to %s/tasks\n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';
});
```
## <a name="16-before-we-add-oauth-support-lets-run-the-server"></a>16: před přičteme OAuth podpory, Pojďme spustit serveru.

Vyzkoušejte serveru před přičteme ověřování

Nejjednodušší způsob je pomocí otočení na příkazovém řádku. Předtím jsme potřebujeme jednoduchý nástroj, který umožňuje analyzovat výstup jako JSON. Aby je dostala, nainstalujte nástroj json jako použít na příklady dole.

`$npm install -g jsontool`

Nainstalujete nástroje JSON globálně. Teď můžeme jste provést, – promítání se serverem:

Nejdřív se podívejte, jestli je spuštěný monogoDB isntance.

`$sudo mongod`

Potom přejděte do adresáře a začněte kulmy.

`$ cd azuread`
`$ node server.js`

`$ curl -isS http://127.0.0.1:8080 | json`

```Shell
HTTP/1.1 2.0OK
Connection: close
Content-Type: application/json
Content-Length: 171
Date: Tue, 14 Jul 2015 05:43:38 GMT
[
"GET /",
"POST /tasks/:owner/:task",
"POST /tasks (for JSON body)",
"GET /tasks",
"PUT /tasks/:owner",
"GET /tasks/:owner",
"DELETE /tasks/:owner/:task"
]
```

Pak můžete přičteme úkolu tímto způsobem:

`$ curl -isS -X POST http://127.0.0.1:8888/tasks/brandon/Hello`

Odpověď na by měl být:

```Shell
HTTP/1.1 201 Created
Connection: close
Access-Control-Allow-Origin: *
Access-Control-Allow-Headers: X-Requested-With
Content-Type: application/x-www-form-urlencoded
Content-Length: 5
Date: Tue, 04 Feb 2014 01:02:26 GMT
Hello
```
A můžete v seznamu úkolů pro Brandon tímto způsobem:

`$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

Pokud to všechno funguje jsme chtít přidat OAuth rozhraní REST API server.

**Máte rozhraní REST API server s MongoDB!**

## <a name="17-add-authentication-to-our-rest-api-server"></a>17: přidání ověřování naše REST API server

Teď, když máme pracovního rozhraní REST API (congrats, btw!) nastavíme užitečnost proti Azure AD.

Z příkazového řádku změňte adresáře ke složce **azuread** v opačném případě už není:

`cd azuread`

### <a name="1-use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>1: použití oidcbearerstrategy, která je součástí passport azure ad

Pokud jsme jste vytvořili typické server ZBÝVAJÍCÍ úkol bez jakýkoli druh se tak mohli ověřovat. Je to, kde můžeme začít zadávat, společně.

Nejdřív potřeba znamená to, doporučujeme používat Passport. Toto oprávnění umístěte za jiné konfigurace serveru:

```Javascript
// Let's start using Passport.js

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support
```

> [AZURE.TIP]
Při psaní rozhraní API vždy propojení dat na něco jedinečné z tokenu, uživatel nemůže falšují. Pokud tento server uchovává úkol položky, uloží je na základě předplatného ID uživatele v token (volat prostřednictvím token.sub), který jsme umístění v poli "vlastník". Zajistíte, že pouze tento uživatel přístup k jeho úkoly a nikdo jiný přístup úkoly zadali. Obsahuje vystavení rozhraní API "vlastník" takže externího uživatele můžete požádat o dalších uživatelů úkoly, i když se ověření.

Pak použijeme strategie nosný připojení otevřít ID, které jsou součástí passport azure ad. Podívejte se jen na kód pro tuto, budete popisuji ho brzy. Umístěte to za co všechno vám pated nad:

```Javascript
/**
/*
/* Calling the OIDCBearerStrategy and managing users
/*
/* Passport pattern provides the need to manage users and info tokens
/* with a FindorCreate() method that must be provided by the implementor.
/* Here we just autoregister any user and implement a FindById().
/* You'll want to do something smarter.
**/
var findById = function(id, fn) {
for (var i = 0, len = users.length; i < len; i++) {
var user = users[i];
if (user.sub === id) {
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
log.info('User was added automatically as they were new. Their sub is: ', token.sub);
users.push(token);
owner = token.sub;
return done(null, token);
}
owner = token.sub;
return done(null, user, token);
});
}
);
passport.use(oidcStrategy);
```

Passport používá podobný vzor pro všechny, které je strategie (Twitter, Facebook, atd.), které dodržovat všechny strategie autory. Prohlížíte strategie uvidíte, že nám předat function(), který má token a Hotovo jako parametry. Strategie zodpovědně chodily zpět do nám po dělá všechny uživatele práce. Po dělá jsme vhodné ukládat uživatele a schovat tokenu, takže nebude potřebujeme požádání uživatele o zadání ho znovu.

> [AZURE.IMPORTANT]
Kód výše trvá všichni uživatelé, které dojde k ověření naše server. Jedná se o jako automatického registrace. Ve výrobním servery by chcete umožnit všem aniž by bylo nejdříve je absolvovat procesu registrace, který se rozhodnete. Je to obvykle vzorku, který se zobrazí v aplikacích příjemce, kteří nastavil, abyste registrovat se službou Facebook, ale pak vás vyzve k vyplňte další informace. Pokud to nebyl programu příkazového řádku, jsme může mít jenom extrahovaných e-mailu z tokenu objekt, který je vrácena a požádán, aby vyplňte další informace o. Vzhledem k tomu, že toto je testovací server můžeme jednoduše přidat databázi v paměti.

### <a name="2-finally-protect-some-endpoints"></a>2. konečně chránit některé koncové body

Ochrana koncové body zadáním passport.authenticate() hovor s protokol, který chcete použít.

Úprava naše směrování v naší kódu serveru zajímavější něco udělat:

```Javascript
server.get('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), listTasks);
server.get('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), listTasks);
server.get('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), getTask);
server.head('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), getTask);
server.post('/tasks/:owner/:task', passport.authenticate('oidc-bearer', {
session: false
}), createTask);
server.post('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), createTask);
server.del('/tasks/:owner/:task', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), removeAll, function respond(req, res, next) {
res.send(204);
next();
});
```

## <a name="18-run-your-server-application-again-and-ensure-it-rejects-you"></a>18: Spusťte znova aplikaci serveru a zajistit, aby ho můžete odmítne

Použijeme `curl` znovu zobrazíte Pokud máme teď OAuth2 ochrany proti naše koncové body. Jsme to před spuštěna některou z našich klienta SDK proti tento koncový bod. Vrátí záhlaví by měl být tak, abyste námi o to, že nám jsou správné cestu.

Nejdřív se podívejte, jestli je spuštěný monogoDB isntance.

    $sudo mongod

Potom přejděte do adresáře a začněte kulmy.

    $ cd azuread
    $ node server.js

Vyzkoušejte základní příspěvku:

`$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

401 je odpověď, kterou hledáte, protože označuje, že vrstvy Passport pokouší k přesměrování na koncový bod ověřit, což je přesně co potřebujete.


## <a name="congratulations-you-have-a-rest-api-service-using-oauth2"></a>Blahopřejeme! Máte služba REST API pomocí OAuth2!

Jste se jde můžete provést tyto akce se tento server bez použití kompatibilní klienta OAuth2. Je třeba projít další návodu.

Pokud právě hledáte informace o tom, jak implementovat rozhraní REST API pomocí Restify a OAuth2, máte víc než dost kódu a zachovat vývoje služby a zjistěte, jak vytvářet v tomto příkladu.

## <a name="next-steps"></a>Další kroky

Pro informaci dokončeným ukázkovým (bez konfigurace hodnoty) [je k dispozici jako ZIP tady](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/complete.zip), nebo můžete zkopírovat ho GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

Teď můžete přesunout na další upřesňující témata.  Můžete zkusit:

[Zabezpečené Node.js web appu pomocí koncový bod verze 2.0 >>](active-directory-v2-devquickstarts-node-web.md)

Další zdroje informací najdete v tématech:
- [Průvodce pro vývojáře verze 2.0 >>](active-directory-appmodel-v2-overview.md)
- [Značka StackOverflow "azure active directory" >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Aktualizace zabezpečení pro naše produkty

Budeme rádi, když chcete dostávat oznámení ze kdy zabezpečení incidentem probíhala po návštěvě [tuto stránku](https://technet.microsoft.com/security/dd252948) a přihlášení k odběru poradní výstrah zabezpečení.
