<properties
    pageTitle="Vytvoření aplikace Node.js konverzace s Socket.IO v aplikaci služby Azure"
    description="Kurz, který ukazuje, jak pomocí socket.io ve web appu node.js hostitelem Azure."
    services="app-service\web"
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="create-a-nodejs-chat-application-with-socketio-in-azure-app-service"></a>Vytvoření aplikace Node.js konverzace s Socket.IO v aplikaci služby Azure

Socket.IO poskytuje v reálném čase komunikaci mezi klienty pomocí WebSockets a node.js serveru. Podporuje také nouzovým na jiných přenos (například dlouhé dotazování), které spolupracují s starší verze prohlížečů. Tento kurz se vás provede jednotlivými hostingu Socket.IO na základě chatovací aplikace jako Azure webovou aplikaci a vidíte, jak zobrazit aplikace pomocí [Azure Redis mezipaměti]. Další informace o Socket.IO najdete v tématu <http://socket.io/>.

> [AZURE.NOTE] Postupy uvedené v tomto úkolu platí pro [Aplikace služby Web Apps]; cloudové služby najdete v článku [Vytvoření aplikace Node.js konverzace s Socket.IO na cloudové služby Azure].

## <a name="download-the-chat-example"></a>Stáhněte si příklad konverzace

Pro tento projekt použijeme příklad konverzace z [úložiště Socket.IO GitHub]. Provedení následujících kroků můžete stáhnout příkladu a přiřadit ji někomu projektu, který jste vytvořili.

1.  Stáhněte si [ZIP nebo GZ archivovány vydání] Socket.IO projektu (verze 1.3.5 byl použit u tohoto dokumentu)

1.  Extrahování archivace a Kopírovat **Příklady\\konverzace** adresáře do nového umístění. Například ** \\uzel\\konverzace**.

## <a name="modify-appjs-and-install-modules"></a>Změna app.js a nainstalovat moduly

1.  Přejmenujte soubor **index.js** na **app.js**. Díky Azure ke zjištění, že se jedná o aplikaci Node.js.

1.  Otevřete soubor **app.js** v textovém editoru. Změna řádek obsahující `var io = require('../..')(server);` jak je ukázáno v následujícím příkladu:

        var express = require('express');
        var app = express();
        var server = require('http').createServer(app);
        // var io = require('../..')(server);
        // New:
        var io = require('socket.io')(server);
        var port = process.env.PORT || 3000;

1. Otevřete soubor **package.json** a přidejte odkaz na socket.io v části `dependencies`, jak je ukázáno v následujícím příkladu:

        "dependencies": {
          "express": "3.4.8",
          "socket.io": "1.3.5"
        }

1. Z příkazového řádku, přejděte ** \\uzel\\konverzace** adresář a použití npm nainstalovat moduly požadovaných touto aplikací:

        npm install

    Moduly nainstaluje do podsložky s názvem **node_modules**.

## <a name="create-an-azure-web-app"></a>Vytvoření aplikace pro Azure Web

Tímto postupem vytvoření Azure webovou aplikaci, povolit libovolná publikování a potom WebSocket podpory pro web app.

> [AZURE.NOTE] Pro dokončení tohoto kurzu, třeba účet Azure. Pokud nemáte účet, můžete vytvořit bezplatný účet zkušební v jenom pár minut. Podrobnosti najdete v tématu <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A7171371E" target="_blank">Bezplatnou zkušební verzi Azure</a>.

1. Instalace rozhraní příkazového řádku Azure (Azure rozhraní příkazového řádku) a připojte k předplatnému Azure. V tématu [instalace a konfigurace Azure rozhraní příkazového řádku](../xplat-cli-install.md).

1. Pokud je to vaše první nastavujete úložiště v Azure, je potřeba vytvořit přihlašovací údaje. Z Azure rozhraní příkazového řádku zadejte tento příkaz:

        azure site deployment user set [username] [password]

1. Přejděte ** \\node\chat** adresář a použijte následující příkaz a vytvořit nové Azure webové aplikace a místní úložiště libovolná. Tento příkaz můžete vytvořit taky libovolná vzdálené pojmenované "azure".

        azure site create mysitename --git

    "Mysitename" musí nahraďte jedinečný název pro web app.

1. Potvrzení existující soubory do místní úložiště pomocí následujících příkazů:

        git add .
        git commit -m "Initial commit"

1. Nabízená soubory do úložiště Azure webové aplikace pomocí následujícího příkazu:

        git push azure master

    Po zobrazení výzvy zadejte svoje přihlašovací údaje z kroku 2. Zprávy o stavu se zobrazí jako moduly naimportují na serveru. Po dokončení tohoto procesu aplikace hostitelem Azure webovou aplikaci.

    > [AZURE.NOTE] Během instalace modulu všimnout chyby, "importovaných projekt... nebyl nalezen". Tyto může být ignorovat.

1. Socket.IO používá WebSockets, které nejsou standardně zapnutá u Azure. Povolit sockets webu, použijte tento příkaz:

        azure site set -w

    Pokud se zobrazí výzva, zadejte název web appu.

    >[AZURE.NOTE]
    >Příkaz bude "azure nastavení webu povolené -w", jen s verzí 0.7.4 nebo vyšší rozhraní Azure příkazového řádku. Můžete taky povolit WebSocket podpory pomocí [Portálu Azure](https://portal.azure.com).
    >
    >Chcete-li WebSockets pomocí portálu Azure, klikněte na webová aplikace na zásuvné Web Apps, klikněte na **všechna nastavení** > **Nastavení aplikace**. V části **Web Sockets**klikněte **na**. Klepněte na tlačítko **Uložit**.

1. Web appu na Azure zobrazíte příkazem následující spusťte webový prohlížeč a přejděte do hostovanou web appu:

        azure site browse

Aplikace je teď spuštěna Azure a mohou přenášet zprávy v konverzaci mezi různých klientů pomocí Socket.IO.

## <a name="scale-out"></a>Rozšiřování

Pomocí __adaptér__ k distribuci zpráv a událostí mezi více instancí aplikace je možné Socket.IO aplikací měřítko. Když nejsou k dispozici několik adaptéry, adaptér [socket.io redis] snadno lze pomocí funkce Azure Redis mezipaměti.

> [AZURE.NOTE] Další požadavku na rozšiřování Socket.IO řešení je podpora rychlých relace. Rychlé relace jsou povoleny ve výchozím nastavení pro Web Apps Azure pomocí požádat o směrování Azure. Další informace najdete v tématu [Instance spřažení v Azure weby].

### <a name="create-a-redis-cache"></a>Vytvoření Redis mezipaměti

Postupujte podle kroků v tématu [vytvoření mezipaměti v mezipaměti Redis Azure] na vytvořit novou mezipaměť.

> [AZURE.NOTE] Uloží __název hostitele__ a __primární klíč__ pro mezipaměť, jak tyto bude potřeba v dalším krokům.

### <a name="add-the-redis-and-socketio-redis-modules"></a>Přidání modulů socket.io redis a redis

1. Z příkazového řádku, přejděte __ \\uzel\\konverzace__ adresář a použijte následující příkaz.

        npm install socket.io-redis@0.1.4 redis@0.12.1 --save

    > [AZURE.NOTE] Verze podle tohoto příkazu jsou při zkoušení tohoto článku.

1. Upravit soubor __app.js__ přidat následující řádky bezprostředně za`var io = require('socket.io')(server);`

        var pub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});
        var sub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});

        var redis = require('socket.io-redis');
        io.adapter(redis({pubClient: pub, subClient: sub}));

    Nahraďte název hostitele a klíč pro mezipaměť Redis __redishostname__ a __rediskey__

    Bude vytvářet publikovat a přihlášení k odběru klienta do mezipaměti Redis vytvořili v předchozích krocích. Klienti pak s adaptér ke konfiguraci používají Socket.IO používat Redis mezipaměti pro předávání zpráv a událostí mezi instancemi aplikace

    > [AZURE.NOTE] Během adaptér __socket.io redis__ komunikovat přímo do Redis, aktuální verze podporuje ověřování vyžadované Azure Redis mezipaměti. Takže počáteční připojení vytvoří se na základě modulu __redis__ a potom klienta předána __socket.io redis__ adaptér.
    >
    > Zatímco Azure Redis mezipaměti podporuje zabezpečené připojení pomocí portu 6380, moduly v tomto příkladě použita nepodporují zabezpečené připojení k 7/14/2014. Ve výše uvedeném kódu používá výchozí, nezabezpečené port 6379.

1. Uložte změny __app.js__

### <a name="commit-changes-and-redeploy"></a>Potvrďte změny a přeinstalujte

Z příkazového řádku v __ \\uzel\\konverzace__ adresáře, použijte následující příkazy potvrďte změny a přeinstalujte aplikace.

    git add .
    git commit -m "implementing scale out"
    git push azure master

Po změně mít posunuto na server, můžete změnit měřítko webu napříč několika instancích spuštěných pomocí následujícího příkazu.

    azure site scale instances --instances #

Kde __#__ označuje počet instance, které chcete vytvořit.

Do webové aplikace můžete připojit z více prohlížeče nebo počítačů pro ověření, že jsou všechny klienty správně odeslány zprávy.

## <a name="troubleshooting"></a>Řešení potíží

### <a name="connection-limits"></a>Omezení připojení

Azure Web Apps je k dispozici ve více SKU, které určují zdroje dostupné na vašem webu. Jedná se o počet povolených WebSocket připojení. Další informace najdete v tématu [ceny aplikace webové stránky].

### <a name="messages-arent-being-sent-using-websockets"></a>Nejsou odesílány zprávy pomocí WebSockets

Pokud klienta prohlížeče zachovat přejde k dlouho dotazování místo použití WebSockets, může to být způsobeno jednu z následujících kroků.

* **Pokuste se omezit přepravy do právě WebSockets**

    Aby Socket.IO jako zpráv transport použít WebSockets serveru a klientských podporoval WebSockets. Pokud jeden z nich nepodporuje, Socket.IO vyjednávání jiné dopravu, například dlouhé hlasování. Výchozí seznam přenosy používaný Socket.IO ` websocket, htmlfile, xhr-polling, jsonp-polling`. Vynutit jenom používat WebSockets přidáním následující kód do souboru **app.js** po řádku obsahující `, nicknames = {};`.

        io.configure(function() {
          io.set('transports', ['websocket']);
        });

    > [AZURE.NOTE] Všimněte si, že starší prohlížeče, které nepodporují WebSockets nebude moct připojit k webu, když ve výše uvedeném kódu je aktivní, omezuje sdělení WebSockets pouze.

* **Použít protokol SSL**

    WebSockets závisí na některé menší použité HTTP záhlaví, jako jsou záhlaví **upgradovat** . Některé intermediate síťových zařízení, jako jsou proxy servery web se asi odebere tato záhlaví. K tomu nedocházelo, můžete připojení k WebSocket přes připojení SSL.

    Jednoduše k tomu je třeba nakonfigurovat Socket.IO k `match origin protocol`. Tím se nastaví Socket.IO k zabezpečení komunikace WebSockets stejný jako původní žádost HTTP/HTTPS webové stránky. Pokud v prohlížeči používá HTTPS URL do svého webu, bude další WebSocket komunikace prostřednictvím Socket.IO zabezpečená přes připojení SSL.

    Chcete-li upravit tento příklad povolit tuto konfiguraci, přidejte následující kód k souboru **app.js** po řádku obsahující `, nicknames = {};`.

        io.configure(function() {
          io.set('match origin protocol', true);
        });

* **Ověření konfigurace**

    Azure webových aplikací web apps hostujících Node.js aplikace umožňuje **nastavení(Web.config))** směrovat příchozí žádosti o aplikaci Node.js. Pro WebSockets fungovat správně s aplikacemi Node.js **web.config** musí obsahovat následující položky.

        <webSocket enabled="false"/>

    Zakáže modul služby IIS WebSockets, který obsahuje vlastní provádění WebSockets a konflikty s konkrétními moduly WebSocket Node.js například Socket.IO. Pokud tento řádek není k dispozici nebo je nastavený na `true`, může to být důvod, proč WebSocket transport nefunguje aplikace.

    Za normálních okolností Node.js aplikací neobsahují **nastavení(Web.config)),** takže Azure weby automaticky vygeneruje jednu Node.js aplikací při jejich nasazení. Když se tento soubor je automaticky generované na serveru, musíte se pomocí FTP nebo FTPS adresy URL pro váš web zobrazením tohoto souboru. Najdete FTP a FTPS adresy URL pro váš web ve službě portálu klasické tak, že vyberete webovou aplikaci a potom na odkaz **řídicího panelu** . Adresy URL se zobrazí v části **Rychlý přehled** .

    > [AZURE.NOTE] **Nastavení(Web.config))** vytváří Azure weby pouze, pokud vaše aplikace Microsoft neposkytuje žádnou jednu. Pokud zadáte **nastavení(Web.config)) v kořenovém projekt aplikace** , použijí se tak, že Azure Web Apps.

    Pokud není k dispozici položka, nebo je nastavena na hodnotu `true`, mají vytvořit **web.config** v kořenovém adresáři aplikace Node.js a zadejte hodnotu `false`.  Pro informaci níže je výchozí **web.config** pro aplikace, která používá **app.js** jako vstupní bod.

        <?xml version="1.0" encoding="utf-8"?>
        <!--
             This configuration file is required if iisnode is used to run node processes behind
             IIS or IIS Express.  For more information, visit:

             https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config
        -->

        <configuration>
          <system.webServer>
            <!-- Visit http://blogs.msdn.com/b/windowsazure/archive/2013/11/14/introduction-to-websockets-on-windows-azure-web-sites.aspx for more information on WebSocket support -->
            <webSocket enabled="false" />
            <handlers>
              <!-- Indicates that the server.js file is a node.js web app to be handled by the iisnode module -->
              <add name="iisnode" path="app.js" verb="*" modules="iisnode"/>
            </handlers>
            <rewrite>
              <rules>
                <!-- Do not interfere with requests for node-inspector debugging -->
                <rule name="NodeInspector" patternSyntax="ECMAScript" stopProcessing="true">
                  <match url="^app.js\/debug[\/]?" />
                </rule>

                <!-- First we consider whether the incoming URL matches a physical file in the /public folder -->
                <rule name="StaticContent">
                  <action type="Rewrite" url="public{REQUEST_URI}"/>
                </rule>

                <!-- All other URLs are mapped to the node.js web app entry point -->
                <rule name="DynamicContent">
                  <conditions>
                    <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="True"/>
                  </conditions>
                  <action type="Rewrite" url="app.js"/>
                </rule>
              </rules>
            </rewrite>
            <!--
              You can control how Node is hosted within IIS using the following options:
                * watchedFiles: semi-colon separated list of files that will be watched for changes to restart the server
                * node_env: will be propagated to node as NODE_ENV environment variable
                * debuggingEnabled - controls whether the built-in debugger is enabled

              See https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config for a full list of options
            -->
            <!--<iisnode watchedFiles="web.config;*.js"/>-->
          </system.webServer>
        </configuration>

    Pokud vaše aplikace používá vstupní bod než **app.js**, třeba všechny výskyty **app.js** nahradit správné vstupní bod. Například nahrazení **app.js** **server.js**.

>[AZURE.NOTE] Pokud chcete začít pracovat s aplikaci služby Azure před registrací účet Azure, přejděte na [Zkuste aplikaci služby], které můžete okamžitě vytvořit web appu krátkodobý starter v aplikaci služby. Žádné povinné; kreditní karty žádné závazky.

## <a name="next-steps"></a>Další kroky

V tomto kurzu se naučíte vytvořit chatovací aplikace hostované v Azure webovou aplikaci. Můžete taky hostovat tuto aplikaci jako cloudové služby Azure. Pokyny k tomu najdete v článku [Vytvoření aplikace Node.js konverzace s Socket.IO na cloudové služby Azure].

Další informace najdete v tématu taky [Středisko pro vývojáře Node.js].

## <a name="whats-changed"></a>Co se změnilo

* Průvodce na změnu z webů pro aplikaci služby v tématu: [aplikaci služby Azure a jeho dopad na existující služby Azure].

<!-- URL List -->

[Azure Redis mezipaměti]: /documentation/services/redis-cache/
[Služba aplikací Web Apps]: http://go.microsoft.com/fwlink/?LinkId=529714
[Webová stránka aplikace ceny]: http://go.microsoft.com/fwlink/?LinkId=511643
[Vytvoření aplikace Node.js konverzace s Socket.IO na Azure cloudové služby]: ../cloud-services/cloud-services-nodejs-chat-app-socketio.md
[Install and Configure the Azure CLI]: ../xplat-cli-install.md
[Služba Azure aplikací a jejich dopad na existující služby Azure]: http://go.microsoft.com/fwlink/?LinkId=529714
[Středisko pro vývojáře Node.js]: /develop/nodejs/
[Vyzkoušejte aplikaci služby]: http://go.microsoft.com/fwlink/?LinkId=523751
[Instance spřažení Azure webových stránek]: https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/
[Vytvořit mezipaměť v mezipaměti Redis Azure]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md

[Socket.IO redis]: https://github.com/socketio/socket.io-redis
[Socket.IO GitHub úložiště]: https://github.com/socketio/socket.io
[ZIP nebo GZ archivovány vydání]: https://github.com/socketio/socket.io/releases

<!-- IMG List -->

[chat-example-view]: ./media/web-sites-nodejs-chat-app-socketio/socketio-2.png
[npm-output]: ./media/web-sites-nodejs-chat-app-socketio/socketio-7.png
[completed-app]: ./media/web-sites-nodejs-chat-app-socketio/websitesocketcomplete.png
