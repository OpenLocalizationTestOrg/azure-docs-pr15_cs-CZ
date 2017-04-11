<properties
    pageTitle="Nasazení aplikace od Sails.js webové aplikace služby Azure"
    description="Naučte se nasadit aplikaci Node.js aplikaci služby Azure. Tento kurz se dozvíte, jak nasadit do Sails.js webových aplikací."
    services="app-service\web"
    documentationCenter="nodejs"
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="09/23/2016"
    ms.author="cephalin"/>

# <a name="deploy-a-sailsjs-web-app-to-azure-app-service"></a>Nasazení aplikace od Sails.js webové aplikace služby Azure

Tento kurz se dozvíte, jak nasazení aplikace od Sails.js aplikace služby Azure. V procesu můžete glean obecné informace o tom, jak nakonfigurovat aplikaci Node.js spuštění v aplikaci služby. 

Měli byste mít pracovní znalost Sails.js. Tento kurz není určená k vám k problémům souvisejícím s systém Sail.js obecně.


## <a name="prerequisites"></a>Zjistit předpoklady pro

- [Node.js](https://nodejs.org/)
- [Sails.js](http://sailsjs.org/get-started)
- [Libovolná](http://www.git-scm.com/downloads)
- [Azure rozhraní příkazového řádku](../xplat-cli-install.md)
- Účet Microsoft Azure. Pokud nemáte účet, můžete [Registrace bezplatnou zkušební verzi](/pricing/free-trial/?WT.mc_id=A261C142F) nebo [Aktivujte své výhody odběratele Visual Studio](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

>[AZURE.NOTE] Pokud chcete zobrazit aplikaci služby Azure v akci před registrací účet Azure, klikněte na [Akci aplikaci služby](http://go.microsoft.com/fwlink/?LinkId=523751). Tady můžete okamžitě vytvořit aplikaci krátkodobý starter v aplikaci služby, žádné povinné platební kartou, žádné závazky.

## <a name="step-1-create-a-sailsjs-app-locally"></a>Krok 1: Vytvoření aplikace Sails.js místně

Nejdřív rychle vytvořte výchozí Sails.js aplikaci ve vašem prostředí vývoj provedením těchto kroků:

1. Otevřete terminál příkazového řádku podle svého výběru a `CD` pracovního adresáře.

2. Vytvoření Sails.js aplikace a spuštění:

        sails new <appname>
        cd <appname>
        sails lift

    Zkontrolujte, že přejdete na domovskou stránku výchozí na http://localhost:1377.

## <a name="step-2-create-the-azure-app-resource"></a>Krok 2: Vytvoření zdroje Azure aplikace

Dále vytvořte aplikaci služby zdrojů v Azure. Budete chtít později Nasaďte aplikaci Sails.js k němu.

1. Přihlaste se k Azure profesionálové takto:
1. Stejné Terminálová přejděte do režimu ASM a přihlaste se k Azure:

        azure config mode asm
        azure login

    Postupujte podle výzva k přihlášení pokračovat v prohlížeči pod svým účtem Microsoft, který má předplatné Azure.

2. Zkontrolujte, jestli že jste pořád v kořenovém adresáři Sails.js projektu. Vytvoření zdrojů aplikace aplikaci služby v Azure s názvem jedinečné aplikace pomocí příkazu Další. Adresa URL webové aplikace je http://&lt;název_aplikace >. azurewebsites.net.

        azure site create --git <appname>

    Sledování se zobrazí výzva ke vyberte Azure oblasti, kterou chcete nasadit. Pokud jste nikdy nastavíte libovolná/FTP nasazení pověření Azure předplatného, můžete se taky výzva k byla vytvořená.

    Po vytvoření zdroje aplikace aplikaci služby:

    - Je libovolná inicializován Sails.js aplikace
    - Místní inicializován libovolná úložiště je připojený k aplikaci novou aplikaci služby jako libovolná remote, odpovídající s názvem "azure", a
    - A iisnode.yml soubor vytvořený v kořenovém adresáři. Konfigurace [iisnode](https://github.com/tjanczuk/iisnode), které aplikace služby používá ke spuštění aplikace Node.js můžete tento soubor.

## <a name="step-3-configure-and-deploy-your-sailsjs-app"></a>Krok 3: Nastavení a nasazení aplikace Sails.js

 Práce s aplikací Sails.js v aplikaci služby se skládá ze tří hlavních kroků:

 - Konfigurace aplikace pro ni chcete spustit v aplikaci služby
 - Nasazení aplikace služby
 - Přečtěte si stderr a stdout protokoly při odstraňování všech problémů nasazení

Postupujte podle těchto kroků:

1. Otevřete nový soubor iisnode.yml v kořenovém adresáři a přidejte následující dva řádky:

        loggingEnabled: true
        logDirectory: iisnode

    Protokolování je nyní povoleno pro iisnode. Další informace o tom, jak to funguje najdete v článku  [získat protokoly stdout a stderr z iisnode](app-service-web-nodejs-get-started.md#iisnodelog).

2. Otevřete config/env/production.js konfigurovat provozním prostředí a nastavte `port` a `hookTimeout`:

        module.exports = {

            // Use process.env.port to handle web requests to the default HTTP port
            port: process.env.port,
            // Increase hooks timout to 30 seconds
            // This avoids the Sails.js error documented at https://github.com/balderdashy/sails/issues/2691
            hookTimeout: 30000,

            ...
        };

    Si přečtěte následující dokumentaci pro toto nastavení najdete v  [Dokumentaci Sails.js](http://sailsjs.org/documentation/reference/configuration/sails-config).

    Pak budete muset mít jistotu, že [Grunt](https://www.npmjs.com/package/grunt) kompatibilní se službou Azure jeho síťové jednotky. Verze grunt menší než 1.0.0 používá balíčku zastaralé [glob](https://www.npmjs.com/package/glob) (nižší než 5.0.14), které nejsou podporovány síťové jednotky. 

3. Otevřete package.json a změňte `grunt` verzi `1.0.0` a odeberte všechny `grunt-*` balíčků. Vaše `dependencies` vlastnost by měl vypadat takto:

        "dependencies": {
            "ejs": "<leave-as-is>",
            "grunt": "1.0.0",
            "include-all": "<leave-as-is>",
            "rc": "<leave-as-is>",
            "sails": "<leave-as-is>",
            "sails-disk": "<leave-as-is>",
            "sails-sqlserver": "<leave-as-is>"
        },

3. V package.json, přidejte následující text `engines` vlastnost nastavení verze Node.js, které chceme.

        "engines": {
            "node": "6.6.0"
        },

6. Uložte provedené změny a otestovat provedené změny, abyste měli jistotu, že aplikace stále běží místně. K tomuto účelu odstranit `node_modules` složku a potom spusťte:

        npm install
        sails lift

4. Teď umožňuje libovolná nasazení aplikace Azure:

        git add .
        git commit -m "<your commit message>"
        git push azure master

5. Nakonec stejně snadné spuštění živé Azure aplikace v prohlížeči:

        azure site browse

    Teď byste měli vidět domovské stránce stejné Sails.js.
    
    ![](./media/app-service-web-nodejs-sails/sails-in-azure.png)

## <a name="troubleshoot-your-deployment"></a>Poradce při potížích s nasazením

Když aplikace Sails.js nepovede z nějakého důvodu v aplikaci služby, najděte protokoly stderr můžou pomoct odstranit ho.
Další informace najdete v tématu [získat protokoly stdout a stderr z iisnode](app-service-web-nodejs-sails.md#iisnodelog).
Pokud spustila úspěšně, protokolu stdout by měl zobrazit známé zprávu:

                .-..-.

    Sails              <|    .-..-.
    v0.12.4             |\
                        /|.\
                        / || \
                    ,'  |'  \
                    .-'.-==|/_--'
                    `--'-------' 
    __---___--___---___--___---___--___
    ____---___--___---___--___---___--___-__

    Server lifted in `D:\home\site\wwwroot`
    To see your app, visit http://localhost:\\.\pipe\c775303c-0ebc-4854-8ddd-2e280aabccac
    To shut down Sails, press <CTRL> + C at any time.

Můžete určit granularity protokoly stdout v souboru [config/log.js](http://sailsjs.org/#!/documentation/concepts/Logging) . 

## <a name="connect-to-a-database-in-azure"></a>Připojení k databázi v Azure

Připojení k databázi v Azure, vytvořte databázi podle svého výběru v Azure, jako jsou databáze SQL Azure, MySQL, MongoDB, mezipaměti Azure (Redis) atd a pomocí odpovídající [úložiště adaptér](https://github.com/balderdashy/sails#compatibility) o připojení definován. Postup v této části ukazují, jak se připojit k databázi MySQL v Azure.

1. Postupujte podle výuková [tady](../store-php-create-mysql-database.md) k vytvoření databáze MySQL v Azure.

2. Z příkazového řádku terminálu nainstalujte adaptér MySQL:

        npm install sails-mysql --save

3. Otevřete config/connections.js a přidejte následující objekt připojení k seznamu: 

        mySql: {
            adapter: 'sails-mysql',
            user: process.env.dbuser,
            password: process.env.dbpassword,
            host: process.env.dbhost, 
            database: process.env.dbname,
            options: {
                encrypt: true
            }
        },

4. Pro každou proměnnou prostředí (`process.env.*`), je potřeba nastavit v aplikaci služby. K tomuto účelu spusťte následující příkazy z terminálu. Všechny informace o připojení potřebujete je Azure portálu (najdete v článku [připojení k databázi MySQL](../store-php-create-mysql-database.md#connect)).

        azure site appsetting add dbuser="<database user>"
        azure site appsetting add dbpassword="<database password>"
        azure site appsetting add dbhost="<database hostname>"
        azure site appsetting add dbname="<database name>"
        
    Vložení nastavení v dialogovém okně Nastavení Azure app zachová citlivá data z vašeho zdrojového (libovolná). Pak nakonfigurujete vývojové prostředí použít stejné informace o připojení.

4. Otevřete config/local.js a přidejte následující objekt připojení:

        connections: {
            mySql: {
                user: "<database user>",
                password: "<database password>",
                host: "<database hostname>", 
                database: "<database name>",
            },
        },
    
    Konfigurace přepíše nastavení v souboru config/connections.js místního prostředí. Tento soubor je vyňatých .gitignore výchozí v projektu, takže nebude uložené v libovolná. Teď můžete se připojit k databázi MySQL z Azure webovou aplikaci a z místní vývojové prostředí.

4. Otevřete config/env/production.js konfigurace provozním prostředí a přidejte následující `models` objektu:

        models: {
            connection: 'mySql',
            migrate: 'safe'
        },

4. Otevřete config/env/development.js konfigurace vývojové prostředí a přidejte následující `models` objektu:

        models: {
            connection: 'mySql',
            migrate: 'alter'
        },

    `migrate: 'alter'`umožňuje používat databázi migrace funkcí k vytváření a snadno aktualizovat databázových tabulek ve vaší MySQL. Však `migrate: 'safe'` se používá k prostředí Azure (výroba), protože Sails.js nelze použít `migrate: 'alter'` ve výrobním prostředí (viz  [Si přečtěte následující dokumentaci Sails.js](http://sailsjs.org/documentation/concepts/models-and-orm/model-settings)).

4. V terminálu [Generovat](http://sailsjs.org/documentation/reference/command-line-interface/sails-generate) Sails.js [rozhraní API přehled](http://sailsjs.org/documentation/concepts/blueprints) , jak běžným způsobem, spusťte `sails lift` k vytvoření databáze s migrací Sails.js databáze. Příklad:

         sails generate api mywidget
         sails lift

    `mywidget` Modelu generovaných tento příkaz je prázdná, ale nemůžeme mohli rychle zobrazit máme připojení k databázi.
    Když spustíte `sails lift`, předtím vytvořil chybějící tabulky pro modely aplikace používá.

6. Přístup k přehled rozhraní API jste právě vytvořili v prohlížeči. Příklad:

        http://localhost:1337/mywidget/create
    
    Rozhraní API by měly vrátit vytvořené položky zpět v okně prohlížeče, což znamená, že se úspěšně vytvoří databázi.

        {"id":1,"createdAt":"2016-09-23T13:32:00.000Z","updatedAt":"2016-09-23T13:32:00.000Z"}

5. Teď použít změny pro Azure a vyhledejte aplikaci, aby zkontrolovala, jestli že stále funguje.

        git add .
        git commit -m "<your commit message>"
        git push azure master
        azure site browse

6. Přístup k přehled rozhraní API Azure web appu. Příklad:

        http://<appname>.azurewebsites.net/mywidget/create

    Rozhraní API chybovou jiné nové položky, Azure webovou aplikaci mluví k databázi MySQL.

## <a name="more-resources"></a>Další materiály

- [Začínáme s Node.js web apps v aplikaci služby Azure](app-service-web-nodejs-get-started.md)
- [Používání Node.js moduly s Azure aplikacemi](../nodejs-use-node-modules-azure-apps.md)
