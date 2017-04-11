<properties
    pageTitle="Začínáme s Azure AD přihlášení a odhlášení pomocí node.js"
    description="Postup vytvoření node.js Express MVC webové aplikace, která lze integrovat s Azure AD k přihlášení."
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
    ms.date="08/15/2016"
    ms.author="brandwe"/>

# <a name="nodejs-web-app-sign-in--sign-out-with-azure-ad"></a>NodeJS webové aplikace přihlásit a podepsat mimo s Azure AD


Tady použijeme Passport:

- Uživatel se přihlaste do aplikace pomocí Azure AD.
- Zobrazte některé informace o uživateli.
- Nejdřív se př uživatele mimo aplikaci.

**Passport** je middleware ověřování pro Node.js. Velmi flexibilní a moduly, Passport unobtrusively vyřadit každém Express založený nebo Resitify webové aplikace. Komplexní sady strategie podporuje ověřování pomocí uživatelské jméno a heslo, Facebook, Twitter a další. Jsme vytvořili strategie pro Microsoft Azure Active Directory. Budeme se nainstaluje tento modul a pak přidejte Microsoft Azure Active Directory `passport-azure-ad` Plug-inu.

K tomuto účelu budete muset:

1. Registrace aplikace.
2. Nastavení aplikace na používání strategie Passport azure ad.
3. Použití pas vydat žádosti o přihlášení a odhlašovací Azure AD.
4. Vytiskněte dat o uživateli.

Kód pro účely tohoto návodu spravován [na GitHub](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS).  Postup, můžete [Stáhnout osnovu v aplikaci jako ZIP](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip) nebo klonovat kostra:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

Dokončené aplikace je k dispozici na konci kurzu, stejně.

## <a name="1-register-an-app"></a>1. zaregistrovat aplikace
- Přihlaste se k portálu Správa Azure.
- V levém navigačním podokně klikněte na **Služby Active Directory**.
- Vyberte klienta, kde se chcete zaregistrovat aplikace.
- Klikněte na kartu **aplikace** a klikněte na Přidat v dolní okraj.
- Postupujte podle pokynů a vytvořte nové **webové aplikace a/nebo WebAPI**.
    - **Název** aplikace bude popisovat aplikace pro koncové uživatele
    -   **Přihlašovací adresa URL** je základní adresa URL aplikace.  Osnovu výchozí hodnota je "http://localhost:3000/auth/openid/ENTER".
    - **Identifikátor URI aplikace** je jedinečný identifikátor aplikace.  Názvů, je použít `https://<tenant-domain>/<app-name>`, například`https://contoso.onmicrosoft.com/my-first-aad-app`
- Po dokončení zápisu AAD přiřadí aplikace klienta jedinečný identifikátor.  Musíte mít tuto hodnotu v dalších částech tak zkopírujte ho na kartě Konfigurovat.

## <a name="2-add-pre-requisities-to-your-directory"></a>2. přidejte pre requisities do adresáře

Z příkazového řádku změňte adresáře kořenové složky v opačném případě již došlo a následující příkazy:

- `npm install express`
- `npm install ejs`
- `npm install ejs-locals`
- `npm install restify`
- `npm install mongoose`
- `npm install bunyan`
- `npm install assert-plus`
- `npm install passport`

- Kromě toho, musíte mít naše `passport-azure-ad` taky:

- `npm install passport-azure-ad`

Nainstaluje knihoven, které passport azure-ad, závisí na.

## <a name="3-set-up-your-app-to-use-the-passport-node-js-strategy"></a>3. nastavení aplikace pro použití strategie passport uzel js
Tady jsme budete konfigurovat middleware Express použít ověřování protokolem OpenID připojení.  Passport se použijí k problému žádosti o přihlášení a odhlašovací, spravovat relace uživatele a získat informace o uživateli, mimo jiné.

-   Chcete-li začít, otevřete `config.js` souborů v kořenovém projektu a zadejte hodnoty konfigurace vaše aplikace v `exports.creds` oddíl.
    -   `clientID:` Je **Id aplikace** přiřazená vaši aplikaci distribuovali portálu registrace.
    -   `returnURL` Je **Přesměrovat Uri** jste zadali v portálu.
    - `clientSecret` Je tajná generovaného na portálu

- Příštím otevření `app.js` soubor v kořenovém adresáři projektu a přidat volání naleznete vyvolat `OIDCStrategy` strategie, které jsou součástí`passport-azure-ad`


```JavaScript
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

// add a logger

var log = bunyan.createLogger({
    name: 'Microsoft OIDC Example Web Application'
});
```

- Po nastavení, použijte strategie jsme jenom odkazuje zpracovávání naše žádosti o přihlášení

```JavaScript
// Use the OIDCStrategy within Passport. (Section 2) 
// 
//   Strategies in passport require a `validate` function, which accept
//   credentials (in this case, an OpenID identifier), and invoke a callback
//   with a user object.
passport.use(new OIDCStrategy({
    callbackURL: config.creds.returnURL,
    realm: config.creds.realm,
    clientID: config.creds.clientID,
    clientSecret: config.creds.clientSecret,
    oidcIssuer: config.creds.issuer,
    identityMetadata: config.creds.identityMetadata,
    skipUserProfile: config.creds.skipUserProfile,
    responseType: config.creds.responseType,
    responseMode: config.creds.responseMode
  },
  function(iss, sub, profile, accessToken, refreshToken, done) {
    if (!profile.email) {
      return done(new Error("No email found"), null);
    }
    // asynchronous verification, for effect...
    process.nextTick(function () {
      findByEmail(profile.email, function(err, user) {
        if (err) {
          return done(err);
        }
        if (!user) {
          // "Auto-registration"
          users.push(profile);
          return done(null, profile);
        }
        return done(null, user);
      });
    });
  }
));
```
Passport používá podobný vzor pro všechny, které je strategie (Twitter, Facebook, atd.), které dodržovat všechny strategii autory. Prohlížíte strategii uvidíte, že nám předat function(), který má token a Hotovo jako parametry. Strategie zodpovědně chodily zpět do nám po dělá všechny uživatele práce. Po dělá jsme vhodné ukládat uživatele a schovat tokenu tak jsme nebudete muset požádat o ho znovu.


> [AZURE.IMPORTANT] 
Kód výše trvá všichni uživatelé, které dojde k ověření naše server. Jedná se o jako automatického registrace. Ve výrobním servery by chcete umožnit všem bez první je absolvovat procesu registrace, který se rozhodnete. Je to obvykle vzorku, který se zobrazí v aplikacích příjemce, kteří umožňuje registrovat se službou Facebook, ale pak vás vyzve k vyplňte další informace. Pokud to nebyl ukázku aplikace, jsme může mít jenom extrahovaných e-mailu z tokenu objekt, který je vrácena a požádán, aby vyplňte další informace. Vzhledem k tomu, že toto je testovací server můžeme jednoduše přidat databázi v paměti.

- Pak přidáte metody, které vám umožní nám ke sledování přihlášenému uživatelé podle potřeby pasem. Jedná se o serializace a deserializace informace o uživateli:

```JavaScript

// Passport session setup. (Section 2)

//   To support persistent login sessions, Passport needs to be able to
//   serialize users into and deserialize users out of the session.  Typically,
//   this will be as simple as storing the user ID when serializing, and finding
//   the user by ID when deserializing.
passport.serializeUser(function(user, done) {
  done(null, user.email);
});

passport.deserializeUser(function(id, done) {
  findByEmail(id, function (err, user) {
    done(err, user);
  });
});

// array to hold logged in users
var users = [];

var findByEmail = function(email, fn) {
  for (var i = 0, len = users.length; i < len; i++) {
    var user = users[i];
   log.info('we are using user: ', user);
    if (user.email === email) {
      return fn(null, user);
    }
  }
  return fn(null, null);
};
```

- Pak přidání kódu pro načtení modul express. Tady vidíte používáme /views výchozí a poskytuje /routes vzorku, který Express.

```JavaScript

// configure Express (Section 2)

var app = express();


app.configure(function() {
  app.set('views', __dirname + '/views');
  app.set('view engine', 'ejs');
  app.use(express.logger());
  app.use(express.methodOverride());
  app.use(cookieParser());
  app.use(expressSession({ secret: 'keyboard cat', resave: true, saveUninitialized: false }));
  app.use(bodyParser.urlencoded({ extended : true }));
  // Initialize Passport!  Also use passport.session() middleware, to support
  // persistent login sessions (recommended).
  app.use(passport.initialize());
  app.use(passport.session());
  app.use(app.router);
  app.use(express.static(__dirname + '/../../public'));
});

```

- Nakonec přidáte cesty, které bude předat žádostech skutečné přihlášení `passport-azure-ad` stroje:

```JavaScript

// Our Auth routes (Section 3)

// GET /auth/openid
//   Use passport.authenticate() as route middleware to authenticate the
//   request.  The first step in OpenID authentication will involve redirecting
//   the user to their OpenID provider.  After authenticating, the OpenID
//   provider will redirect the user back to this application at
//   /auth/openid/return
app.get('/auth/openid',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('Authentication was called in the Sample');
    res.redirect('/');
  });

// GET /auth/openid/return
//   Use passport.authenticate() as route middleware to authenticate the
//   request.  If authentication fails, the user will be redirected back to the
//   login page.  Otherwise, the primary route function function will be called,
//   which, in this example, will redirect the user to the home page.
app.get('/auth/openid/return',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('We received a return from AzureAD.');
    res.redirect('/');
  });

// POST /auth/openid/return
//   Use passport.authenticate() as route middleware to authenticate the
//   request.  If authentication fails, the user will be redirected back to the
//   login page.  Otherwise, the primary route function function will be called,
//   which, in this example, will redirect the user to the home page.
app.post('/auth/openid/return',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('We received a return from AzureAD.');
    res.redirect('/');
  });
  ```

## <a name="4-use-passport-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>4. pomocí Passport žádosti o přihlášení a odhlašovací vydat Azure AD

Aplikace je teď správně nakonfigurované komunikovat s koncový bod verze 2.0 ověřování protokolem OpenID připojení.  `passport-azure-ad`má stará všechny dobře podrobnosti věnujte zprávy o ověřování, ověřování tokenů z Azure AD a udržování relace uživatele.  Zbývá dát uživatelům umožňuje přihlásit, odhlaste se a získat další informace na přihlášeného uživatele.

- Nejdřív umožňuje přidat výchozí, přihlášení, účtu a odhlášení metody pro naše `app.js` souboru:

```JavaScript

//Routes (Section 4)

app.get('/', function(req, res){
  res.render('index', { user: req.user });
});

app.get('/account', ensureAuthenticated, function(req, res){
  res.render('account', { user: req.user });
});

app.get('/login',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('Login was called in the Sample');
    res.redirect('/');
});

app.get('/logout', function(req, res){
  req.logout();
  res.redirect('/');
});

```

-   Pojďme si rozebrat tyto podrobnosti:
    -   `/` Směrování vás přesměruje do zobrazení index.ejs předávání uživatele v žádosti (pokud existuje)
    - `/account` Směrování bude první ***Zajistěte, aby jsme ověření*** (jsme implementaci, který níže) a potom předejte uživatele v žádosti o tak, že bude další informace o uživateli.
    - `/login` Směrování bude zavolat ověřovacích naše azuread openidconnect dat z `passport-azuread` a pokud, který není úspěšně bude přesměrovat uživatele zpět Publikace1
    - `/logout` Se jednoduše volání logout.ejs (a směrování) který vymaže soubory cookie a vraťte se zpátky do index.ejs uživatele


- Pro poslední část `app.js`, přidání způsobu EnsureAuthenticated, který se používá v `/account` nad.

```JavaScript

// Simple route middleware to ensure user is authenticated. (Section 4)

//   Use this route middleware on any resource that needs to be protected.  If
//   the request is authenticated (typically via a persistent login session),
//   the request will proceed.  Otherwise, the user will be redirected to the
//   login page.
function ensureAuthenticated(req, res, next) {
  if (req.isAuthenticated()) { return next(); }
  res.redirect('/login')
}
```

- Nakonec, ve skutečnosti Vytvořme samotném serveru v `app.js`:

```JavaScript

app.listen(3000);

```


## <a name="5-create-the-views-and-routes-in-express-to-display-our-user-in-the-website"></a>5. Vytvořte zobrazení a trasy v express zobrazíte naše uživatele na webu

Máme naše `app.js` dokončení. Teď potřebujeme jednoduše přidat cesty a zobrazení, která se zobrazí informace jsme seznámení s uživateli, stejně jako úchyt `/logout` a `/login` směruje jsme vytvořili.

- Vytvoření `/routes/index.js` trasa v kořenovém adresáři.

```JavaScript
/*
 * GET home page.
 */

exports.index = function(req, res){
  res.render('index', { title: 'Express' });
};
```

- Vytvoření `/routes/user.js` směrování v kořenovém adresáři

```JavaScript
/*
 * GET users listing.
 */

exports.list = function(req, res){
  res.send("respond with a resource");
};
```

Tyto jednoduché trasy jenom předá podél žádost naše zobrazení, včetně uživatelů, pokud je k dispozici.

- Vytvoření `/views/index.ejs` zobrazení v kořenovém adresáři. Toto je jednoduchý stránku, která budou volání naše přihlášení a odhlášení metod a umožňují získat informace o účtu. Oznámení, že můžete použít vybrané podmíněné `if (!user)` jako uživatel se zpráva předána v žádosti o je důkaz máme přihlášenému v uživatele.

```JavaScript
<% if (!user) { %>
    <h2>Welcome! Please log in.</h2>
    <a href="/login">Log In</a>
<% } else { %>
    <h2>Hello, <%= user.displayName %>.</h2>
    <a href="/account">Account Info</a></br>
    <a href="/logout">Log Out</a>
<% } %>
```

- Vytvoření `/views/account.ejs` zobrazit v kořenovém adresáři, takže můžete zobrazit další informace, které `passport-azuread` byla umístění v pozvánce na uživatele.

```Javascript
<% if (!user) { %>
    <h2>Welcome! Please log in.</h2>
    <a href="/login">Log In</a>
<% } else { %>
<p>displayName: <%= user.displayName %></p>
<p>givenName: <%= user.name.givenName %></p>
<p>familyName: <%= user.name.familyName %></p>
<p>UPN: <%= user._json.upn %></p>
<p>Profile ID: <%= user.id %></p>
<p>Full Claimes</p>
<%- JSON.stringify(user) %>
<p></p>
<a href="/logout">Log Out</a>
<% } %>
```

- Nakonec provedeme to vypadá poměrně přidáním rozložení. Vytvoření "/ views/layout.ejs' zobrazení v kořenovém adresáři

```HTML

<!DOCTYPE html>
<html>
    <head>
        <title>Passport-OpenID Example</title>
    </head>
    <body>
        <% if (!user) { %>
            <p>
            <a href="/">Home</a> | 
            <a href="/login">Log In</a>
            </p>
        <% } else { %>
            <p>
            <a href="/">Home</a> | 
            <a href="/account">Account</a> | 
            <a href="/logout">Log Out</a>
            </p>
        <% } %>
        <%- body %>
    </body>
</html>
```

Nakonec vytvoření a spuštění aplikace. 

Spuštění `node app.js` a přejděte na`http://localhost:3000`


Přihlášení pomocí osobního Account Microsoft nebo pracovního nebo školního účtu a Všimněte si, jak se projeví identita uživatele v seznamu /account.  Nyní máte web app zabezpečený pomocí standardních protokolů, které můžete ověřování uživatelů s jejich osobní a pracovní/školní účty.

Pro informaci dokončeným ukázkovým (bez konfigurace hodnoty) [je k dispozici jako .zip tady](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/complete.zip), nebo můžete zkopírovat ho GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```


Teď můžete přesunout na další upřesňující témata.  Můžete zkusit:

[Zabezpečeného webového rozhraní API s Azure AD >>](active-directory-devquickstarts-webapi-nodejs.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
