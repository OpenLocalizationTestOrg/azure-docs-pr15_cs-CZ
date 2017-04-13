<properties 
    pageTitle="Použití Twilio pro hlasovou VoIP a zprávy SMS v Azure" 
    description="Zjistěte, jak chcete někomu zavolat a odeslání zprávy SMS ke službě rozhraní API Twilio na Azure. Ukázky napsané v Node.js." 
    services="" 
    documentationCenter="nodejs" 
    authors="devinrader" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="wpickett"/>


# <a name="using-twilio-for-voice-voip-and-sms-messaging-in-azure"></a>Použití Twilio pro hlasovou VoIP a zprávy SMS v Azure

Tato příručka ukazuje, jak vytvářet aplikace, které komunikovat s Twilio a node.js na Azure.

<a id="whatis"/>
## <a name="what-is-twilio"></a>Co je Twilio?

Twilio je rozhraní API platformy, která usnadňuje pro vývojáře zkontrolujte a přijímat telefonní hovory, odesílat a přijímat textové zprávy a vložení VoIP voláte do prohlížeče a nativní mobilních aplikací.  Pojďme stručně projít jak to funguje před potápěním.

### <a name="receiving-calls-and-text-messages"></a>Příjem hovorů a textové zprávy

Twilio umožňuje vývojářům [koupit programovatelnou telefonní čísla] [ purchase_phone] kterého mohou sloužit k odesílání a příjem hovorů a text zprávy.  Když čísla Twilio přijme příchozí volání nebo text, Twilio odešle webové aplikace HTTP příspěvku nebo požadavek GET, s dotazem, postupujte podle pokynů uvedených na zpracování hovorů nebo text.  Váš server odpoví na Twilio HTTP žádosti o pomocí [TwiML][twiml], jednoduché sadu značky XML, které obsahují pokyny pro zpracování hovorů nebo text.  Budeme se příklady TwiML jenom chvíli.

### <a name="making-calls-and-sending-text-messages"></a>Volání a odesílání textových zpráv

Provedením požadavků HTTP rozhraní API webových služeb Twilio vývojáři odesílání textových zpráv nebo zahajte odchozí telefonní hovory.  Pro odchozí hovory vývojář taky zadejte adresu URL, která vrací TwiML pokyny, jak provádět odchozí hovory po připojení.

### <a name="embedding-voip-capabilities-in-ui-code-javascript-ios-or-android"></a>Vložení VoIP funkcí v kódu uživatelského rozhraní (JavaScript, iOS nebo Android)

Twilio poskytuje straně klienta SDK, která můžete převést všechny plochy webového prohlížeče, aplikace iOS nebo Android aplikace telefon VoIP.  V tomto článku jsme se zaměřuje na používání VoIP volání v prohlížeči.  Kromě SDK JavaScript Twilio spuštěné v prohlížeči serverové aplikace (naše aplikace node.js) třeba zadat o vystavení token"možnost" klientovi JavaScript.  Další informace o používání VoIP s node.js [na blogu vývojáře Twilio][voipnode].

<a id="signup"/>
## <a name="sign-up-for-twilio-microsoft-discount"></a>Přihlášení Twilio (slevy Microsoft)

Před použitím Twilio služby, musíte první [zaregistrovat účet][signup].  Microsoft Azure zákazníkům speciální sleva – [je potřeba se zaregistrovat tady][signup]!

<a id="azuresite"/>
## <a name="create-and-deploy-a-nodejs-azure-website"></a>Vytvoření a nasazení webu Azure node.js

Další musíte vytvořit web node.js spuštěna Azure.  [Úřední dokumentaci k tím nachází][azure_new_site].  Na vysoké úrovni který bude být následujícím způsobem:

* Registrace k účet Azure, pokud už nemáte
* Vytvoření nového webu pomocí konzole pro správu Azure
* Přidání zdrojového ovládacího prvku podpory (jsme bude se předpokládá, že jste použili libovolná)
* Vytvoření souboru `server.js` s jednoduché node.js webovou aplikaci
* Nasazení této jednoduché aplikace Azure

<a id="twiliomodule"/>
## <a name="configure-the-twilio-module"></a>Konfigurace modulu Twilio

Dále budou začneme psát jednoduché node.js aplikace, které využívá rozhraní API Twilio.  Než začneme, potřebujeme konfigurace naše přihlašovací údaje účtu Twilio.  

### <a name="configuring-twilio-credentials-in-system-environment-variables"></a>Konfigurace Twilio přihlašovacích údajů v systému proměnné

Před uskutečněním ověřené žádosti proti Twilio back-end, potřebujeme naši stránku věnovanou účtu ID zabezpečení a auth token, které slouží jako uživatelské jméno a heslo nastavené pro naše Twilio účet. Nejbezpečnější způsob, jak nakonfigurovat pro použití s modulu uzel v Azure spočívá ve využití proměnné prostředí systému, které můžete nastavit přímo v konzole pro správu Azure.

Vyberte svůj web node.js a klikněte na odkaz "KONFIGUROVAT".  Pokud při posouvání trochu, zobrazí se oblast kterých je možné nastavit vlastnosti konfigurace aplikace.  Zadejte svoje přihlašovací údaje účtu Twilio ([používaný řídícího Twilio][twilio_dashboard]) jak je vidět – zkontrolujte, že název je "TWILIO_ACCOUNT_SID" a "TWILIO_AUTH_TOKEN" v tomto pořadí:

![Konzole pro správu Azure][azure-admin-console]

Po konfiguraci tyto proměnné restartujte aplikaci v konzole Azure.

### <a name="declaring-the-twilio-module-in-packagejson"></a>Deklarace modulu Twilio package.json

Pak potřebujeme vytvořit package.json ke správě naše uzel modul závislosti prostřednictvím [npm].  Na stejné úrovni jako "server.js" soubor, který jste vytvořili v kurzu Azure/node.js vytvoření souboru s názvem "package.json".  V daném souboru umístěte takto:

  {"název": "název aplikace", "verze": "0.0.1", "Soukromé": PRAVDA, "skripty": {"start": "uzel server"}, "závislosti": {"express": "3.1.0", "ejs": "*", "twilio": "*"}}

To deklaruje modulu twilio jako závislostí, stejně jako oblíbené [Expresní web framework] [ express] a modul EJS šablony.  OK teď můžeme máte všechno nastavené - napíšete některé kód.

<a id="makecall"/>
## <a name="make-an-outbound-call"></a>Odchozí hovor

Pojďme vytvořit jednoduchý formulář, který se zavolat na číslo, že volíme.  Otevřete server.js a zadejte tento kód.  Poznámka: místo s textem "CHANGE_ME" - dát název webu azure zde:

    // Module dependencies
    var express = require('express'), 
      path = require('path'), 
      http = require('http'), 
      twilio = require('twilio');

    // Create Express web application
    var app = express();

    // Express configuration
    app.configure(function(){
      app.set('port', process.env.PORT || 3000);
      app.set('views', __dirname + '/views');
      app.set('view engine', 'ejs');
      app.use(express.favicon());
      app.use(express.logger('dev'));
      app.use(express.bodyParser());
      app.use(express.methodOverride());
      app.use(app.router);
      app.use(express.static(path.join(__dirname, 'public')));
    });
    app.configure('development', function(){
      app.use(express.errorHandler());
    });

    // Render an HTML user interface for the application's home page
    app.get('/', function(request, response) {
      response.render('index');
    });

    // Handle the form POST to place a call
    app.post('/call', function(request, response) {
      var client = twilio();
      client.makeCall({
          // make a call to this number
          to:request.body.number,

          // Change to a Twilio number you bought - see:
          // https://www.twilio.com/user/account/phone-numbers/incoming
          from:'+15558675309',

          // A URL in our app which generates TwiML
          // Change "CHANGE_ME" to your app's name
          url:'https://CHANGE_ME.azurewebsites.net/outbound_call'
      }, function(error, data) {
          // Go back to the home page
          response.redirect('/');
      });
    });

    // Generate TwiML to handle an outbound call
    app.post('/outbound_call', function(request, response) {
      var twiml = new twilio.TwimlResponse();

      // Say a message to the call's receiver 
      twiml.say('hello - thanks for checking out Twilio and Azure', {
          voice:'woman'
      });

      response.set('Content-Type', 'text/xml');
      response.send(twiml.toString());
    });

    // Start server
    http.createServer(app).listen(app.get('port'), function(){
      console.log("Express server listening on port " + app.get('port'));
    });

Dále vytvořte adresář s názvem "zobrazení" - uvnitř tohoto adresáře, vytvořte do souboru nazvaného "index.ejs" pomocí následujícího obsahu:

    <!DOCTYPE html>
    <html>
    <head>
      <title>Twilio Test</title>
      <style>
        input { height:20px; width:300px; font-size:18px; margin:5px; padding:5px; }
      </style>
    </head>
    <body>
      <h1>Twilio Test</h1>
      <form action="/call" method="POST">
          <input placeholder="Enter a phone number" name="number"/>
          <br/>
          <input type="submit" value="Call the number above"/>
      </form>
    </body>
    </html>

Teď nasazení webu Azure a otevřete vaší domácnosti.  Je třeba mít textového pole zadejte telefonní číslo a přijmete volání z vašeho čísla Twilio!

<a id="sendmessage"/>
## <a name="send-an-sms-message"></a>Odeslání zprávy SMS

Teď Pojďme nastavení uživatelského rozhraní a formuláře zpracování použití logických operátorů k odeslání textové zprávy.  Otevřete "server.js" a přidat následující kód za poslední volání "app.post":

    app.post('/sms', function(request, response) {
      var client = twilio();
      client.sendSms({
          // send a text to this number
          to:request.body.number,

          // A Twilio number you bought - see:
          // https://www.twilio.com/user/account/phone-numbers/incoming
          from:'+15558675309',

          // The body of the text message
          body: request.body.message
          
      }, function(error, data) {
          // Go back to the home page
          response.redirect('/');
      });
    });

"Views/index.ejs" přidejte jiném formuláři za první fázi můžou odeslat číslo a textové zprávy:

    <form action="/sms" method="POST">
      <input placeholder="Enter a phone number" name="number"/>
      <br/>
      <input placeholder="Enter a message to send" name="message"/>
      <br/>
      <input type="submit" value="Send text to the number above"/>
    </form>

Přeinstalujte aplikaci Azure a teď byste měli být možné odeslat, formulářů a textovou zprávu odeslat sami sobě (nebo na jakékoli přátele nejbližší)?

<a id="nextsteps"/>
## <a name="next-steps"></a>Další kroky

Jste se naučili teď základy používání node.js a Twilio vytvářet aplikace, které komunikovat.  Ale tyto příklady stěží pomocná povrchu toho, co je možné s Twilio a node.js.  Další informace o používání Twilio s node.js podívejte se na následující články:

* [Úřední modul dokumentů][docs]
* [Kurz VoIP s aplikacemi node.js][voipnode]
* [Votr – v reálném čase služby SMS hlasování aplikaci node.js a CouchDB (tři části)][votr]
* [Pár programování v prohlížeči s node.js][pair]

Jsme ať že používat hackery node.js a Twilio na Azure!

[purchase_phone]: https://www.twilio.com/user/account/phone-numbers/available/local
[twiml]: https://www.twilio.com/docs/api/twiml
[signup]: http://ahoy.twilio.com/azure
[azure_new_site]: /app-service-web/web-sites-nodejs-develop-deploy-mac.md
[twilio_dashboard]: https://www.twilio.com/user/account
[npm]: http://npmjs.org
[express]: http://expressjs.com
[voipnode]: http://www.twilio.com/blog/2013/04/introduction-to-twilio-client-with-node-js.html
[docs]: http://twilio.github.io/twilio-node/
[votr]: http://www.twilio.com/blog/2012/09/building-a-real-time-sms-voting-app-part-1-node-js-couchdb.html
[pair]: http://www.twilio.com/blog/2013/06/pair-programming-in-the-browser-with-twilio.html
[azure-admin-console]: ./media/partner-twilio-nodejs-how-to-use-voice-sms/twilio_1.png



