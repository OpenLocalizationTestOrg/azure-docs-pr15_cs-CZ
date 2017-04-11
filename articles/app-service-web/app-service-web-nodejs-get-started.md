<properties
    pageTitle="Začínáme s Node.js web apps v aplikaci služby Azure"
    description="Naučte se nasadit aplikaci Node.js web app v aplikaci služby Azure."
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
    ms.topic="get-started-article"
    ms.date="07/01/2016"
    ms.author="cephalin"/>

# <a name="get-started-with-nodejs-web-apps-in-azure-app-service"></a>Začínáme s Node.js web apps v aplikaci služby Azure

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Tento kurz ukazuje, jak vytvořit jednoduchý aplikaci [Node.js] a nasazení služby [Azure aplikace] z příkazového řádku prostředí, například cmd.exe nebo flám. Pokyny v tomto kurzu může být zahájen v operačním systému, který mohlo by umožnit spuštění Node.js.

>[AZURE.INCLUDE [app-service-linux](../../includes/app-service-linux.md)] 

<a name="prereq"></a>
## <a name="prerequisites"></a>Zjistit předpoklady pro

- [Node.js]
- [Bower]
- [Yeoman]
- [Libovolná]
- [Azure rozhraní příkazového řádku]
- Účet Microsoft Azure. Pokud nemáte účet, můžete [Registrace bezplatnou zkušební verzi] nebo [Aktivujte své výhody odběratele Visual Studio].

## <a name="create-and-deploy-a-simple-nodejs-web-app"></a>Vytvoření a nasazení jednoduché Node.js webové aplikace

1. Otevřete terminál příkazového řádku podle svého výběru a nainstalujte [Express generátor pro Yeoman].

        npm install -g generator-express

2. `CD`pracovního adresáře a následné vytvoření aplikace pro expresní pomocí následující syntaxe:

        yo express
        
    Výběr z následujících možností po zobrazení výzvy:  

    `? Would you like to create a new directory for your project?`**Ano**  
    `? Enter directory name`**{název_aplikace}**  
    `? Select a version to install:`**MVC**  
    `? Select a view engine to use:`**Jade**  
    `? Select a css preprocessor to use (Sass Requires Ruby):`**Žádná**  
    `? Select a database to use:`**Žádná**  
    `? Select a build tool to use:`**Grunt**

3. `CD`do kořenové složky novou aplikaci a začněte ji ujistili běží ve vašem vývojové prostředí:

        npm start

    V prohlížeči přejděte na <http://localhost:3000> a ujistěte se, zda vidíte domovské stránce Express. Po spuštění aplikace ověření správně, použijte `Ctrl-C` ukončíte ho.
    
1. Přejděte do režimu ASM a přihlaste se k Azure (nutné [Rozhraní příkazového řádku Azure](#prereq)):

        azure config mode asm
        azure login

    Postupujte podle výzva k přihlášení pokračovat v prohlížeči pod svým účtem Microsoft, který má předplatné Azure.

2. Ujistěte se, pořád v kořenovém adresáři aplikace a pak vytvořit aplikaci služby zdrojů aplikace v Azure s názvem jedinečné aplikace pomocí příkazu Další. Příklad: http://{appname}.azurewebsites.net

        azure site create --git {appname}

    Sledování se zobrazí výzva ke vyberte Azure oblasti, kterou chcete nasadit. Pokud jste nikdy nastavíte libovolná/FTP nasazení pověření Azure předplatného, můžete se taky výzva k byla vytvořená.

3. Otevřete soubor./config/config.js z kořenové složky aplikace a změňte port výrobní `process.env.port`; vaše `production` vlastnosti v `config` objekt by měla vypadat podobně jako v následujícím příkladu:

        production: {
            root: rootPath,
            app: {
                name: 'express1'
            },
            port: process.env.port,
        }

    Díky aplikaci Node.js odpovědět na web požadavky na výchozí port této iisnode přijímá.
    
4. Otevřete./package.json a přidejte `engines` vlastnost k [Určení požadovaného Node.js verze](#version).

        "engines": {
            "node": "6.6.0"
        }, 

4. Uložte provedené změny a pak pomocí libovolná nasadit aplikaci Azure:

        git add .
        git commit -m "{your commit message}"
        git push azure master

    Generátor Express již obsahuje soubor .gitignore, protože váš `git push` nemá používat šířky pásma pokusíte nahrát node_modules / adresář.

5. Nakonec spusťte živou Azure aplikace v prohlížeči:

        azure site browse

    Teď byste měli vidět webovou aplikaci Node.js spuštění živé v aplikaci služby Azure.
    
    ![Příklad procházení nasazení aplikace.][deployed-express-app]

## <a name="update-your-nodejs-web-app"></a>Aktualizujte svoji Node.js webovou aplikaci

Provést aktualizace webovou aplikaci Node.js spuštění v aplikaci služby, stačí znovu spustit `git add`, `git commit`, a `git push` jako jste provedli nasazené nejdřív webovou aplikaci.
     
## <a name="how-app-service-deploys-your-nodejs-app"></a>Jak služba aplikací nasazuje Node.js aplikace

Azure aplikace služby používá [iisnode] ke spuštění aplikace Node.js. Kudu modul (libovolná nasazení) a rozhraní příkazového řádku Azure vzájemně ladily a můžete setkat i v případě optimalizovaného při vytvoření a nasazení aplikace Node.js z příkazového řádku. 

- `azure site create --git`rozpozná běžné Node.js vzorek server.js nebo app.js a vytvoří iisnode.yml v kořenovém adresáři. Chcete-li přizpůsobit iisnode můžete tento soubor.
- Na `git push azure master`, Kudu automaticky následující úlohy nasazení:

    - Pokud package.json je kořenové úložiště, spusťte `npm install --production`.
    - Generování Web.config pro iisnode odkazující na skript start v package.json (například server.js nebo app.js).
    - Přizpůsobení Web.config připravené aplikace pro ladění s uzel Kontrola metadat.
    
## <a name="use-a-nodejs-framework"></a>Používat Node.js rámec

Pokud používáte Oblíbené rámec Node.js, například [Sails.js] [ SAILSJS] nebo [MEAN.js] [ MEANJS] se dají aplikace, nasazením můžou být aplikace služby. Oblíbené rámce Node.js máte konkrétní quirks a jejich balíčku závislostmi zachovat zobrazuje aktualizace. Však aplikaci služby zpřístupní stdout a stderr protokoly, abyste mohli vědět přesně, co se stalo s aplikací a měnit podle toho. Další informace najdete v tématu [získat protokoly stdout a stderr z iisnode](#iisnodelog).

Následující kurzy vám ukáže, jak pracovat s konkrétní framework v aplikaci služby:

- [Nasazení aplikace od Sails.js webové aplikace služby Azure]
- [Vytvoření aplikace Node.js konverzace s Socket.IO v aplikaci služby Azure]
- [Jak používat io.js s Azure aplikace služby Web Apps]

<a name="version"></a>
## <a name="use-a-specific-nodejs-engine"></a>Použití určitého modulu Node.js

Typické pracovního postupu řekněte aplikaci služby používat určitého modulu Node.js běžným způsobem v package.json.
Příklad:

    "engines": {
        "node": "6.6.0"
    }, 

Modul nasazení Kudu určí, které modul Node.js použít v následujícím pořadí:

- Nejprve se podívejte na iisnode.yml jestlil `nodeProcessCommandLine` není zadán. Pokud ano, pomocí které.
- Další, podívejte se na package.json jestlil `"node": "..."` podle `engines` objektu. Pokud ano, pomocí které.
- Vyberte výchozí verze Node.js ve výchozím nastavení.

>[AZURE.NOTE] Je vhodné explicitně definovat modul Node.js, které chcete. Můžete změnit výchozí verze Node.js a vzhledem k tomu, že na výchozí verzi Node.js není vhodná pro aplikace se může zobrazit chyby v Azure web appu.

<a name="iisnodelog"></a>
## <a name="get-stdout-and-stderr-logs-from-iisnode"></a>Získat protokoly stdout a stderr z iisnode

Číst iisnode protokoly, postupujte takto:

> [AZURE.NOTE] Po dokončení těchto kroků, protokoly neexistuje, dokud dojde k chybě.

1. Otevřete soubor iisnode.yml poskytujícím rozhraní příkazového řádku Azure.

2. Nastavte dvou následujících parametrů: 

        loggingEnabled: true
        logDirectory: iisnode
    
    Dohromady, budou řekněte iisnode v aplikaci služby chcete umístit jeho výstup stdout a stderror D:\home\site\wwwroot\**iisnode** adresář.

3. Uložte provedené změny a potom použít změny pro Azure pomocí následujících příkazů libovolná:

        git add .
        git commit -m "{your commit message}"
        git push azure master
   
    iisnode je teď nakonfigurované. Další kroky ukazují, jak získat přístup k tyto protokoly.
     
4. V prohlížeči přístup ke konzole Kudu ladění aplikace, který je v:

        https://{appname}.scm.azurewebsites.net/DebugConsole 

    Tato adresa URL se liší od adresa URL web appu přidáním "*.scm.*" do názvu DNS. Pokud vynecháte, které sčítání na adresu URL, zobrazí se vám chyba 404.

5. Přejděte na D:\home\site\wwwroot\iisnode

    ![Přechod do umístění souboru protokolu iisnode.][iislog-kudu-console-find]

6. Klikněte na ikonu **Úpravy** pro protokol, který si chcete přečíst. Pokud chcete, aby se můžete taky kliknout **Stáhnout** nebo **Odstranit** .

    ![Otevření souboru protokolu iisnode.][iislog-kudu-console-open]

    Teď můžete vidět protokol pomoci při ladění nasazení aplikace služby.
    
    ![Kontrola souborů protokolu iisnode.][iislog-kudu-console-read]

## <a name="debug-your-app-with-node-inspector"></a>Ladění aplikace s uzel Kontrola metadat

Pokud používáte uzel inspektor ladění Node.js aplikace, můžete pro aplikace live aplikaci služby. Kontrola uzel je předinstalovaný v průběhu instalace iisnode pro aplikaci služby. A pokud nasazení prostřednictvím libovolná Web.config automaticky generované z Kudu již obsahuje všechny konfigurace budete muset povolit uzel Kontrola metadat.

Povolit uzel inspektor, postupujte takto:

1. Otevřete iisnode.yml kořenové úložiště a zadejte následujících parametrů: 

        debuggingEnabled: true
        debuggerExtensionDll: iisnode-inspector.dll

3. Uložte provedené změny a potom použít změny pro Azure pomocí následujících příkazů libovolná:

        git add .
        git commit -m "{your commit message}"
        git push azure master
   
4. Teď jenom přejděte k vaší aplikaci start souboru určené skriptem start ve vaší package.json se přidá k adrese URL/Debug. Například

        http://{appname}.azurewebsites.net/server.js/debug
    
    Nebo
    
        http://{appname}.azurewebsites.net/app.js/debug

## <a name="more-resources"></a>Další materiály

- [Určení verze Node.js v aplikaci Azure](../nodejs-specify-node-version-azure-apps.md)
- [Osvědčené postupy a příručka pro řešení potíží pro Node.js aplikace na Azure](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md)
- [Ladění Node.js web app v aplikaci služby Azure](web-sites-nodejs-debug.md)
- [Používání Node.js moduly s Azure aplikacemi](../nodejs-use-node-modules-azure-apps.md)
- [Služba Azure aplikací Web Apps: Node.js](http://blogs.msdn.com/b/silverlining/archive/2012/06/14/windows-azure-websites-node-js.aspx)
- [Středisko pro vývojáře Node.js](/develop/nodejs/)
- [Začínáme s aplikací web apps v aplikaci služby Azure](app-service-web-get-started.md)
- [Prozkoumání konzoly ladění velmi skryté Kudu]

<!-- URL List -->

[Azure rozhraní příkazového řádku]: ../xplat-cli-install.md
[Azure aplikace služby]: ../app-service/app-service-value-prop-what-is.md
[Aktivujte své výhody odběratele Visual Studio]: http://go.microsoft.com/fwlink/?LinkId=623901
[Bower]: http://bower.io/
[Vytvoření aplikace Node.js konverzace s Socket.IO v aplikaci služby Azure]: ./web-sites-nodejs-chat-app-socketio.md
[Nasazení aplikace od Sails.js webové aplikace služby Azure]: ./app-service-web-nodejs-sails.md
[Prozkoumání konzoly ladění velmi skryté Kudu]: /documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[Expresní generátor pro Yeoman]: https://github.com/petecoop/generator-express
[Libovolná]: http://www.git-scm.com/downloads
[Jak používat io.js s Azure aplikace služby Web Apps]: ./web-sites-nodejs-iojs.md
[iisnode]: https://github.com/tjanczuk/iisnode/wiki
[MEANJS]: http://meanjs.org/
[Node.js]: http://nodejs.org
[SAILSJS]: http://sailsjs.org/
[zaregistrovat bezplatnou zkušební verzi]: http://go.microsoft.com/fwlink/?LinkId=623901
[web app]: ./app-service-web-overview.md
[Yeoman]: http://yeoman.io/

<!-- IMG List -->

[deployed-express-app]: ./media/app-service-web-nodejs-get-started/deployed-express-app.png
[iislog-kudu-console-find]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-navigate.png
[iislog-kudu-console-open]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-open.png
[iislog-kudu-console-read]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-read.png
