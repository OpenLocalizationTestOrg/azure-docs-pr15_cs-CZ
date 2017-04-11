<properties 
    pageTitle="Web App s Express (Node.js) | Microsoft Azure" 
    description="Kurz je založena na kurz cloudové služby, který demonstruje použití modulu Express." 
    services="cloud-services" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>






# <a name="build-a-nodejs-web-application-using-express-on-an-azure-cloud-service"></a>Vytvoření webové aplikace Node.js pomocí Express na cloudové služby Azure

Node.js obsahuje minimální sadu funkcí v modulu runtime core.
Často vývojáři 3 stran moduly poskytují další funkce při vytváření aplikace Node.js. V tomto kurzu vytvoříte novou aplikaci používání modulu [Express][] , který obsahuje rámec MVC k vytváření Node.js webových aplikací.

Snímek obrazovky s dokončeným aplikace je níže:

![Webový prohlížeč zobrazení Vítejte Express v Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

##<a name="create-a-cloud-service-project"></a>Vytvoření projektu služby cloudu

Proveďte následující kroky k vytvoření nového projektu služby cloudu s názvem "expressapp":

1. V **Nabídce Start** nebo **Obrazovce Start**vyhledejte **Prostředí Windows PowerShell**. Nakonec klikněte pravým tlačítkem myši **Prostředí Windows PowerShell** a vyberte **Spustit jako správce**.

    ![Ikona Azure prostředí PowerShell](./media/cloud-services-nodejs-develop-deploy-express-app/azure-powershell-start.png)

    [AZURE.INCLUDE [install-dev-tools](../../includes/install-dev-tools.md)]

2. Změna adresáře **c:\\uzel** adresář a potom zadejte následující příkazy k vytvoření nové řešení s názvem **expressapp** a roli webu s názvem **WebRole1**:

        PS C:\node> New-AzureServiceProject expressapp
        PS C:\Node\expressapp> Add-AzureNodeWebRole
        PS C:\Node\expressapp> Set-AzureServiceProjectRole WebRole1 Node 0.10.21

    > [AZURE.NOTE] Ve výchozím nastavení používá starší verzí systému Node.js **AzureNodeWebRole přidat** . Příkaz **Nastavení AzureServiceProjectRole** pokyn Azure používat v0.10.21 uzel.  Všimněte si, že že rozlišují velká.  Můžete ověřit že kontrolou vlastnost **moduly pro řeč** v **WebRole1\package.json**byla vybrána správná Node.js.

##<a name="install-express"></a>Instalace Express

1. Nainstalujte generátor Express zadáním následujícího příkazu:

        PS C:\node\expressapp> npm install express-generator -g

    Výstup příkazu npm by měla vypadat podobně jako výsledek dole. 

    ![Prostředí Windows PowerShell zobrazení výstup npm nainstalovat express příkaz.](./media/cloud-services-nodejs-develop-deploy-express-app/express-g.png)

2. Změna adresáře do adresáře **WebRole1** a příkazem express generovat nové aplikace:

        PS C:\node\expressapp\WebRole1> express

    Zobrazí se výzva k přepsání starší aplikaci. Zadejte **y** nebo **Ano** pokračovat. Express vygeneruje soubor app.js a struktura složek pro vytváření aplikace.

    ![Výstup příkazu express](./media/cloud-services-nodejs-develop-deploy-express-app/node23.png)


5.  Pokud chcete nainstalovat další závislosti definice v souboru package.json, zadejte tento příkaz:

        PS C:\node\expressapp\WebRole1> npm install

    ![Výstup npm instalace příkaz](./media/cloud-services-nodejs-develop-deploy-express-app/node26.png)

6.  Pomocí následujícího příkazu zkopírujte soubor **Koš/www** **server.js**. To je ke cloudové služby můžete najít vstupní bod pro tuto aplikaci.

        PS C:\node\expressapp\WebRole1> copy bin/www server.js

    Po dokončení tohoto příkazu byste si měli **server.js** souboru v adresáři WebRole1.

7.  Úprava **server.js** odebrat některého "." znaků z následujícího řádku.

        var app = require('../app');

    Po provedení této změny, by měl vypadat takto řádku.

        var app = require('./app');

    Tato změna požaduje od soubor (dřív to bylo **Koš/www**,) jsme přesunuli do stejného adresáře jako soubor aplikace požadovaných. Po provedení této změny uložit soubor **server.js** .

8.  Zadejte následující příkaz spustit aplikaci v Azure emulátoru:

        PS C:\node\expressapp\WebRole1> Start-AzureEmulator -launch

    ![Webová stránka obsahující Vítá vás express.](./media/cloud-services-nodejs-develop-deploy-express-app/node28.png)

## <a name="modifying-the-view"></a>Změna zobrazení

Nyní upravte zobrazení tak, aby se zpráva "Úvodní k Express v Azure".

1.  Zadejte tento příkaz Otevřít soubor index.jade:

        PS C:\node\expressapp\WebRole1> notepad views/index.jade

    ![Obsah souboru index.jade.](./media/cloud-services-nodejs-develop-deploy-express-app/getting-started-19.png)

    Jadeit je výchozí zobrazení modul využívány Express. Další informace o modul Jade zobrazení naleznete v tématu [http://jade-lang.com][].

2.  Úprava poslední řádek textu přidáním **v Azure**.

    ![Soubor index.jade poslední řádek nahrazuje: p Vítá vás \#{title} v Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node31.png)

3.  Uložte soubor a ukončete Poznámkový blok.

4.  Aktualizujte si prohlížeč a zobrazí se provedené změny.

    ![Okně prohlížeče stránka obsahuje Vítá vás Express v Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node32.png)

Po testování aplikace, získáte pomocí rutiny **Zastavit AzureEmulator** zastavit emulátor.

##<a name="publishing-the-application-to-azure"></a>Publikování aplikace Azure

V okně Azure PowerShell získáte pomocí rutiny **Publikovat AzureServiceProject** nasazení aplikace do cloudové služby

    PS C:\node\expressapp\WebRole1> Publish-AzureServiceProject -ServiceName myexpressapp -Location "East US" -Launch

Po dokončení operace nasazení prohlížeči otevřete a zobrazte webovou stránku.

![Webový prohlížeč zobrazení stránky Express. Adresa URL označuje, že je teď hostovaný na Azure.](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="next-steps"></a>Další kroky

Další informace najdete v tématu [Středisko pro vývojáře Node.js](/develop/nodejs/).

  [Node.js Web Application]: http://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
  [Express]: http://expressjs.com/
  [http://Jade-Lang.com]: http://jade-lang.com

 
