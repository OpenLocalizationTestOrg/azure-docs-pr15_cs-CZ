<properties
    pageTitle="Node.js rozhraní API aplikace služby Azure aplikace | Microsoft Azure"
    description="Informace o vytvoření Node.js RESTful rozhraní API a nasazení aplikace rozhraní API aplikace služby Azure."
    services="app-service\api"
    documentationCenter="node"
    authors="bradygaster"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="node"
    ms.topic="get-started-article"
    ms.date="05/26/2016"
    ms.author="rachelap"/>

# <a name="build-a-nodejs-restful-api-and-deploy-it-to-an-api-app-in-azure"></a>Vytvoření Node.js RESTful rozhraní API a nasazení do aplikace rozhraní API v Azure

[AZURE.INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

Tento kurz ukazuje, jak vytvořit jednoduchý [Node.js](http://nodejs.org) rozhraní API a nasazení k [rozhraní API aplikace](app-service-api-apps-why-best-platform.md) v [Aplikaci služby Azure](../app-service/app-service-value-prop-what-is.md) pomocí [Libovolná](http://git-scm.com). Můžete použít žádný operační systém, mohlo by umožnit spuštění Node.js a budete dělat celé svojí práci pomocí nástrojů příkazového řádku, jako jsou cmd.exe nebo flám.

## <a name="prerequisites"></a>Zjistit předpoklady pro

1. Účet Microsoft Azure ([otevřete bezplatného účtu](https://azure.microsoft.com/pricing/free-trial/))
1. [Node.js](http://nodejs.org) nainstalovaný (v tomto příkladu se předpokládá, že máte Node.js verze 4.2.2)
2. [Libovolná](https://git-scm.com/) nainstalovaný
1. [GitHub](https://github.com/) účtu

Zatímco aplikace služby podporuje mnoho způsobů, jak nasazení kódu do aplikace pro rozhraní API, tento kurz znázorňuje metodu libovolná a předpokládá, že máte základní věděli, jak pracovat s libovolná. Informace o jiných způsobech nasazení najdete v článku [nasazení aplikace služby Azure aplikace](../app-service-web/web-sites-deploy.md).

## <a name="get-the-sample-code"></a>Získat kód ukázka

1. Otevřete rozhraní příkazového řádku, mohlo by umožnit spuštění Node.js a libovolná příkazy.

1. Přejděte do složky, můžete použít pro místní úložiště libovolná a klonovat [GitHub úložiště obsahující ukázkový kód](https://github.com/Azure-Samples/app-service-api-node-contact-list).

        git clone https://github.com/Azure-Samples/app-service-api-node-contact-list.git

    Rozhraní API ukázkové poskytuje dva koncové body: Požadavek Get `/contacts` vrátí seznam jmen a e-mailové adresy ve formátu JSON, zatímco `/contacts/{id}` vrátí jenom vybraný kontakt.

## <a name="scaffold-auto-generate-nodejs-code-based-on-swagger-metadata"></a>Scaffold (automatické generování) Node.js kód na základě Swagger metadat

Formát souboru pro metadata, která popisují rozhraní API RESTful [swagger](http://swagger.io/) je. Azure aplikaci služby má [vestavěné podporu Swagger metadata](app-service-api-metadata.md). Tato část kurzu modely rozhraní API vývoj pracovního postupu aplikace ve kterém nejdřív vytvořit Swagger metadata a pomocí nich scaffold (automatické generování) kódu serveru pro rozhraní API. 

>[AZURE.NOTE] Pokud nechcete, aby se dozvíte, jak scaffold Node.js kód ze souboru Swagger metadata, můžete tuto část přeskočit. Pokud budete chtít stačí nasadit ukázkový kód do nové rozhraní API aplikace, přejdete přímo do části [Vytvoření aplikace pro rozhraní API v Azure](#createapiapp) .

### <a name="install-and-execute-swaggerize"></a>Instalace a spuštění Swaggerize

1. Provádět následující příkazy nainstalovat moduly NPM **Jo** a **swaggerize generátor** globálně.

        npm install -g yo
        npm install -g generator-swaggerize

    Swaggerize je nástroj generovaný kódu serveru pro rozhraní API označená souboru Swagger metadata. Swagger soubor, který budete používat s názvem *api.json* a je umístěn ve složce *start* úložiště, které jste klonovat.

2. Přejděte do složky *start* a potom spustit `yo swaggerize` příkaz. Swaggerize bude zeptejte se řadu otázek.  **Co volání tohoto projektu**zadejte **cestu k swagger dokumentu**"ContactList", zadejte "api.json" a **Express spokojení, nebo Restify**, zadejte "express".

        yo swaggerize

    ![Swaggerize příkazového řádku](media/app-service-api-nodejs-api-app/swaggerize-command-line.png)
    
    **Poznámka**: Pokud dojde k chybě v tomto kroku, dalším krokem je popsáno opravný nástroj fix it.

    Swaggerize vytvoří složku aplikace, scaffolds popisovače a konfigurace soubory a vytvoří soubor **package.json** . Modul expresní zobrazení bude použito k vygenerování stránku nápovědy Swagger.  

3. Pokud `swaggerize` příkazu nezdaří "neočekávané token" nebo "neplatné řídicí sekvence" chybu, opravte příčinu chyby do schránky úpravou vygenerovaných *package.json* souboru. V `regenerate` řádku pod `scripts`, změnit zpětné lomítko, který předchází *api.json* lomítko, takže řádku vypadá jako v následujícím příkladu:

        "regenerate": "yo swaggerize --only=handlers,models,tests --framework express --apiPath config/api.json"

1. Přejděte do složky, která obsahuje kód scaffolded (v tomto případě */start/ContactList* podsložky).

1. Spuštění `npm install`.
    
        npm install
        
2. Instalace modulu NPM **jsonpath** . 

        npm install --save jsonpath
        
    ![Instalace Jsonpath](media/app-service-api-nodejs-api-app/jsonpath-install.png)

1. Instalace modulu NPM **swaggerize uživatelského rozhraní** . 

        npm install --save swaggerize-ui
        
    ![Swaggerize nainstalovat uživatelského rozhraní](media/app-service-api-nodejs-api-app/swaggerize-ui-install.png)

### <a name="customize-the-scaffolded-code"></a>Při úpravě scaffolded kódu

1. Zkopírujte složce **knihovny** ze složky **zahájení** do složky **ContactList** vytvořené scaffolder. 

1. Nahraďte kód v souboru **handlers/contacts.js** následující kód. 

    Tento kód používá JSON dat uložených v souboru **lib/contacts.json** , která je poskytovaný **lib/contactRepository.js**. Nový kód contacts.js odpoví na HTTP požadavky na získání všechny kontakty a vrácená jako datové JSON. 

        'use strict';
        
        var repository = require('../lib/contactRepository');
        
        module.exports = {
            get: function contacts_get(req, res) {
                res.json(repository.all())
            }
        };

1. Nahraďte kód v souboru **handlers/contacts/{id}.js** fofllowing kód. 

        'use strict';

        var repository = require('../../lib/contactRepository');
        
        module.exports = {
            get: function contacts_get(req, res) {
                res.json(repository.get(req.params['id']));
            }    
        };

1. Nahraďte kód v **server.js** následující kód. 

    Změny provedené v souboru server.js jsou Schvalte dokument pomocí komentářů, zobrazí se změnami provedenými. 

        'use strict';

        var port = process.env.PORT || 8000; // first change

        var http = require('http');
        var express = require('express');
        var bodyParser = require('body-parser');
        var swaggerize = require('swaggerize-express');
        var swaggerUi = require('swaggerize-ui'); // second change
        var path = require('path');

        var app = express();

        var server = http.createServer(app);

        app.use(bodyParser.json());

        app.use(swaggerize({
            api: path.resolve('./config/api.json'), // third change
            handlers: path.resolve('./handlers'),
            docspath: '/swagger' // fourth change
        }));

        // change four
        app.use('/docs', swaggerUi({
          docs: '/swagger'  
        }));

        server.listen(port, function () { // fifth and final change
        });

### <a name="test-with-the-api-running-locally"></a>Otestujte pomocí rozhraní API spuštěn místně

1. Aktivace serveru pomocí příkazového řádku spustitelný soubor Node.js. 

        node server.js

1. Při prohlížení **http://localhost:8000/kontaktů**, najdete v článku výstupu JSON ze seznamu kontaktů (nebo se zobrazí výzva ke stažení, v závislosti na prohlížeči). 

    ![Všechny kontakty rozhraní Api volání](media/app-service-api-nodejs-api-app/all-contacts-api-call.png)

1. Při prohlížení **http://localhost:8000/kontaktů/2**, zobrazí se kontakt představované tuto hodnotu id.

    ![Volání kontaktu rozhraní Api konkrétní](media/app-service-api-nodejs-api-app/specific-contact-api-call.png)

1. Swagger JSON dat je podávané množství **prostřednictvím/swagger** koncového bodu:

    ![Kontakty Swagger Json](media/app-service-api-nodejs-api-app/contacts-swagger-json.png)

1. Uživatelské rozhraní Swagger podávané **prostřednictvím/Docs** koncového bodu. V uživatelském rozhraní Swagger slouží k vyzkoušejte vaše rozhraní API bohatých klienta funkcí HTML.

    ![Swagger uživatelského rozhraní](media/app-service-api-nodejs-api-app/swagger-ui.png)

## <a id="createapiapp"></a>Vytvoření nového rozhraní API aplikace

V této části používat portál Azure při vytváření nové aplikace rozhraní API v Azure. Rozhraní API aplikace představuje výpočetním prostředky, které Azure poskytne spustíte kód. V dalších částech nasadíte kódu nová rozhraní API aplikace.

1. Přejděte na [portál Azure](https://portal.azure.com/). 

1. Klikněte na **Nový > Web + mobilní > rozhraní API aplikace**. 

    ![Nová rozhraní API aplikace portálu](media/app-service-api-nodejs-api-app/new-api-app-portal.png)

4. Zadejte **název aplikace** , který je jedinečný v *azurewebsites.net* doménu, například NodejsAPIApp plus čísla, aby byl jedinečný. 

    Řekněme, že je v poli Název `NodejsAPIApp`, adresa URL bude `nodejsapiapp.azurewebsites.net`.

    Pokud zadáte název, který někdo jiný má použít, se zobrazí červený vykřičník vpravo.

6. V rozevíracím seznamu **Pole Skupina zdroje** klikněte na **Nový**a potom v **názvu nové skupiny prostředků** zadejte "NodejsAPIAppGroup" nebo jiný název podle potřeby. 

    [Pole Skupina zdroje](../azure-resource-manager/resource-group-overview.md) je kolekce Azure zdroje, jako jsou aplikace, databáze a VMs rozhraní API aplikace. Pro účely tohoto návodu je vhodné vytvořit nové skupiny prostředků, protože, který usnadňuje odstranit v jednom kroku Azure prostředky, které je pro kurz vytvořit.

4. Klikněte na **Plán/umístění aplikace služby**a potom klikněte na **Vytvořit nový**.

    ![Vytvořit plán služeb aplikací](./media/app-service-api-nodejs-api-app/newappserviceplan.png)

    V následujících krocích vytvoření plánu aplikace služby pro nové skupiny prostředků. Plán služeb aplikací určuje výpočetním prostředky, které rozhraní API aplikace běží na. Například pokud se rozhodnete bezplatné osy, rozhraní API aplikace je spuštěn sdílené VMs spuštěn pro některé placené úrovní ho na vyhrazené VMs. Informace o různých plánech aplikaci služby najdete v tématu [Přehled plány aplikaci služby](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

5. Zásuvné **Plán služeb aplikací** vyplňte "NodejsAPIAppPlan" nebo jiný název podle potřeby.

5. V rozevíracím seznamu **umístění** vyberte umístění, které je nejblíž vám.

    Tohle nastavení určuje, které Azure datacentra aplikace se spustí v. Pro účely tohoto návodu můžete vybrat všechny oblasti a neumožňují výrazná rozdíl. Ale pro výrobní aplikace je požadován váš server je co nejblíže klientům, které jsou přístupu minimalizovat [latence](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090).

5. Klikněte na **ceny osy > Zobrazit vše > F1 bezplatné**.

    Pro účely tohoto návodu poskytne bezplatná ceny osy dostatečně výkon.

    ![Vyberte uvolnit ceny osy](./media/app-service-api-nodejs-api-app/selectfreetier.png)

6. V zásuvné **Plán služeb aplikací** klikněte na **OK**.

7. V zásuvné **Rozhraní API aplikace** klikněte na **vytvořit**.

## <a name="set-up-your-new-api-app-for-git-deployment"></a>Nastavení aplikace nového rozhraní API pro libovolná nasazení

Rozhraní API aplikace budete nasazení kódu stisknutím potvrzení libovolná úložiště v aplikaci služby Azure. V této části kurzu vytvoříte přihlašovací údaje a libovolná úložiště v Azure, který budete chtít použít pro nasazení.  

1. Po vytvoření rozhraní API aplikace klikněte na tlačítko **aplikace služby > {rozhraní API aplikace}** na domovské stránce portálu. 

    Na portálu zobrazí listy **Rozhraní API aplikace** a **Nastavení** .

    ![Rozhraní API aplikaci portálu a nastavení zásuvné](media/app-service-api-nodejs-api-app/portalapiappblade.png)

1. V **Nastavení** zásuvné přejděte dolů k oddílu **publikování** a potom klikněte na **Nasazení pověření**.
 
3. V zásuvné **nastavit přihlašovací údaje nasazení** zadejte uživatelské jméno a heslo a klikněte na **Uložit**.

    Pomocí těchto přihlašovacích údajů pro publikování kódu Node.js rozhraní API aplikace. 

    ![Nasazení přihlašovací údaje](media/app-service-api-nodejs-api-app/deployment-credentials.png)

1. V **Nastavení** zásuvné, klikněte na **nasazení zdroj > Zvolit zdroj > místní úložiště libovolná**, klikněte na tlačítko **OK**.

    ![Vytvořte libovolná Repo](media/app-service-api-nodejs-api-app/create-git-repo.png)

1. Po změně zásuvné zobrazit aktivní nasazení vytvoření libovolná úložiště. Od úložiště je nový, máte v seznamu není aktivní nasazení. 

    ![Žádné aktivní nasazení](media/app-service-api-nodejs-api-app/no-active-deployments.png)

1. Zkopírujte adresu URL libovolná úložiště. K tomuto účelu přejděte zásuvné pro novou aplikaci rozhraní API a podívejte se na část zásuvné **Essentials** . Všimněte si **Libovolná klonovat URL** v části **Essentials** . Když najedete myší tuto adresu URL, zobrazí ikona na pravé straně, která bude zkopírujte adresu URL do schránky. Kliknutím na tuto ikonu zkopírujte adresu URL.

    ![Získání adresy Url libovolná z portálu](media/app-service-api-nodejs-api-app/get-the-git-url-from-the-portal.png)

    **Poznámka**: byste potřebovali klonovat libovolná URL v následující části Ujistěte se tedy ji chcete uložit jinam, postupujte v současné době.

Teď máte aplikaci pro rozhraní API s úložišti libovolná ho zálohování, můžete do úložiště pro nasazení kód do aplikace rozhraní API nabízená kód. 

## <a name="deploy-your-api-code-to-azure"></a>Nasazení kódu rozhraní API Azure

V této části vytvoříte místní úložiště libovolná, která obsahuje váš server kód pro rozhraní API a potom stisknete kódu z této úložiště do úložiště v Azure, který jste vytvořili.

1. Kopírovat `ContactList` složky do umístění, do kterého můžete použít pro nová místní úložiště libovolná. Pokud jste nenahráli první část kurzu, zkopírujte `ContactList` z `start` složce. v opačném zkopírujte `ContactList` z `end` složky.

1. Vaše příkazového řádku pomocí nástroje pro přejděte do nové složky a potom zadejte následující příkaz Vytvořit novou místní úložiště libovolná. 

        git init

     ![Nové místní Repo libovolná](media/app-service-api-nodejs-api-app/new-local-git-repo.png)

1. Spusťte následující příkaz přidat libovolná vzdálené rozhraní API aplikace úložiště. 

        git remote add azure YOUR_GIT_CLONE_URL_HERE

    **Poznámka**: Tento řetězec "YOUR_GIT_CLONE_URL_HERE" nahraďte vlastní libovolná klonovat adresu URL, kterou jste si zkopírovali. 

1. Provádět následující příkazy k vytvoření potvrdit zahrnující všechna kódu. 

        git add .
        git commit -m "initial revision"

    ![Libovolná potvrdit výstup](media/app-service-api-nodejs-api-app/git-commit-output.png)

1. Provedení příkazu Posunout kódu Azure. Po zobrazení výzvy k zadání hesla, zadejte ten, který jste vytvořili dříve v portálu Azure.

        git push azure master

    To spustí nasazení k rozhraní API aplikace.  

1. V prohlížeči přejděte zpátky na zásuvné **nasazení** rozhraní API aplikace a uvidíte, že se vyskytuje nasazení. 

    ![Nasazení nového?](media/app-service-api-nodejs-api-app/deployment-happening.png)

    Rozhraní příkazového řádku současně, odráží stav nasazení během se děje. 

    ![Uzel Js nasazení později](media/app-service-api-nodejs-api-app/node-js-deployment-happening.png)

    Po dokončení nasazení zásuvné **nasazení** odráží úspěšné zavedení změny kódu pro rozhraní API aplikace. 

## <a name="test-with-the-api-running-in-azure"></a>Otestujte pomocí rozhraní API spuštěné v Azure
 
3. Zkopírujte adresu **URL** v části **Základy** zásuvné rozhraní API aplikace. 

    ![Nasazení dokončení](media/app-service-api-nodejs-api-app/deployment-completed.png)

1. Pomocí rozhraní REST API klientů, jako je pošťák nebo Fiddler (nebo webový prohlížeč), zadejte adresu URL kontaktů volání rozhraní API, která je `/contacts` koncový bod rozhraní API aplikace. Adresa URL bude`https://{your API app name}.azurewebsites.net/contacts`

    Při této koncový bod problém požadavek GET, uslyšíte JSON výstup rozhraní API aplikace.

    ![Pošťák zasažení rozhraní Api](media/app-service-api-nodejs-api-app/postman-hitting-api.png)

2. V prohlížeči přejděte `/docs` koncový bod objevovat uživatelského rozhraní Swagger při spuštění v Azure.

Teď, když máte nepřetržitý doručení drátové nahoru, můžete měnit kód a nasazení je Azure pouhým předání potvrzení do úložiště libovolná Azure.

## <a name="next-steps"></a>Další kroky

V tomto okamžiku jste úspěšně rozhraní API aplikaci vytvořili a používaný rozhraní API Node.js kód. Další výuková zobrazuje jak [používat rozhraní API aplikace klientů JavaScript pomocí CORS](app-service-api-cors-consume-javascript.md).
