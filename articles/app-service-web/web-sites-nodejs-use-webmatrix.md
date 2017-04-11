<properties 
    pageTitle="Vytvořte a nasaďte do Node.js webových aplikací do Azure pomocí WebMatrix" 
    description="Kurz, která se naučíte používat WebMatrix můžete vyvíjet aplikace Node.js a ho nasadit do Azure aplikace služby Web Apps." 
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


# <a name="build-and-deploy-a-nodejs-web-app-to-azure-using-webmatrix"></a>Vytvořte a nasaďte do Node.js webových aplikací do Azure pomocí WebMatrix

Tento kurz se dozvíte, jak používat WebMatrix můžete vyvíjet aplikace Node.js a nasazení [Služby Azure aplikací](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps. WebMatrix je bezplatná web vývojového nástroje od Microsoftu, který obsahuje všechno, co budete potřebovat pro vývoj aplikací Web nebo web. WebMatrix obsahuje několik funkcí, které umožňují snadno se použije Node.js včetně kódu dokončení, předdefinovaným šablonám a editoru podpora jadeit, méně a CoffeeScript. Další informace o [WebMatrix](https://www.microsoft.com/web/webmatrix/).

Po dokončení tohoto průvodce, budete mít do webových aplikací Node.js spuštění v aplikaci služby Azure.
 
Snímek obrazovky s dokončeným aplikace je níže:

![Na webu Azure uzel][webmatrix-node-completed]

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

>[AZURE.NOTE] Pokud chcete začít pracovat s aplikaci služby Azure před registrací účet Azure, přejděte na [Zkuste aplikaci služby](http://go.microsoft.com/fwlink/?LinkId=523751), které můžete okamžitě vytvořit web appu krátkodobý starter v aplikaci služby. Žádné povinné; kreditní karty žádné závazky.

## <a name="sign-into-azure"></a>Přihlaste se do Azure

Tímto postupem vytvořte webovou aplikaci pro aplikaci služby Azure.

1. Spuštění WebMatrix
2. Pokud se jedná první jste vypotřebovali WebMatrix, zobrazí se výzva a přihlaste se do Azure.  Můžete jinak, klikněte na tlačítko **Přihlásit** a zvolte **Přidat účet**.  Vyberte **přihlásit** pomocí Account Microsoft.

    ![Přidání účtu][addaccount]

3. Pokud budete mít nemáte svůj účet Azure, můžete se přihlásit pomocí Account Microsoft:

    ![Přihlaste se do Azure][signin]  


## <a name="create-a-site-using-a-built-in-template-for-azure"></a>Vytvořit web pomocí šablony předdefinovaný pro Azure

1. Na obrazovce start klikněte na tlačítko **Nový** a zvolte pro vytvoření nového webu z Galerie šablon na **Galerie šablon** :

    ![Nový web z Galerie šablon][sitefromtemplate]

2. V dialogovém okně **webu ze šablony** vyberte **uzel** a potom vyberte **Expresní web**. Nakonec klikněte na tlačítko **Další**. Pokud chybí všechny požadavky pro šablonu **Express webu** se výzva k instalaci je.

    ![Vyberte šablonu express][webmatrix-templates]

3. Pokud jste se přihlásili k Azure, nyní máte možnost vytvořit webovou aplikaci služby aplikace pro web místní.  Zvolte jedinečný název a vyberte datacentru, kde se mají vytvořit webovou aplikaci aplikaci služby: 

    ![Vytvoření webu na Azure][nodesitefromtemplateazure]
    
4. Až WebMatrix skončí vytváření místních webů a vytvoření aplikace služby webové aplikace, zobrazí se integrovaném vývojovém WebMatrix prostředí.

    ![WebMatrix integrovaném vývojovém prostředí][webmatrix-ide]

##<a name="publish-your-application-to-azure"></a>Publikování aplikace Azure

1. V WebMatrix klikněte na **Publikovat** z **Domů** pásu karet k zobrazení dialogového okna **Náhled publikování** pro daný web.

    ![Náhled publikování][webmatrix-node-publishpreview]

2. Klikněte na **pokračovat**. Po dokončení publikování adresa URL pro web app aplikaci služby se zobrazí v dolní části integrovaném vývojovém WebMatrix prostředí

    ![publikování dokončeno][webmatrix-publish-complete]

3. Klikněte na odkaz otevřít aplikaci služby web appu v prohlížeči.

    ![Expresní web appu][webmatrix-node-express-site]

##<a name="modify-and-republish-your-application"></a>Upravit a znovu publikovat aplikace

Můžete snadno upravovat a publikovat aplikace. Tady vytvoříte jednoduchý změnit na nadpis v souboru **index.jade** a znovu publikovat aplikace.

1. V WebMatrix vyberte **soubory**a potom rozbalte složku **zobrazení** . Otevřete soubor **index.jade** poklepáním.

    ![zobrazení index.jade WebMatrix][webmatrix-modify-index]

2. Změňte řádek odstavce následujícím způsobem:

        p Welcome to #{title} with WebMatrix on Azure!

3. Uložte provedené změny a pak klikněte na ikonu publikovat. Nakonec klikněte na **pokračovat** v dialogovém okně **Náhled publikování** a počkejte na zveřejňují aktualizovat.

    ![Náhled publikování][webmatrix-republish]

4. Po dokončení publikování pomocí odkazu vrácena dokončení procesu publikování zobrazíte aktualizované aplikace pro webovou aplikaci služby.

    ![Azure uzel v prohlížeči][webmatrix-node-completed]

##<a name="next-steps"></a>Další kroky

Další informace o verzích Node.js, které jsou součástí Azure a určení verze pro použití s aplikací, naleznete v tématu [nastavení v aplikaci Azure Node.js verze](../nodejs-specify-node-version-azure-apps.md).

Pokud po nasazení k Azure docházet k problémům s aplikací, informace najdete v tématu [jak ladění Node.js web app v aplikaci služby Azure](web-sites-nodejs-debug.md) na Diagnostika problémů.

## <a name="whats-changed"></a>Co se změnilo
* Průvodce na změnu z webů pro aplikaci služby v tématu: [aplikaci služby Azure a jeho dopad na existující služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

[WebMatrix WebSite]: http://www.microsoft.com/click/services/Redirect2.ashx?CR_CC=200106398
[WebMatrix for Azure]: http://go.microsoft.com/fwlink/?LinkID=253622&clcid=0x409

[webmatrix-node-completed]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-complete.png
[webmatrix-templates]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-templates.png

[webmatrix-node-publishpreview]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-publishpreview.png

[webmatrix-ide]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-ide.png
[webmatrix-publish-complete]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-publish-complete.png
[webmatrix-node-express-site]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-express-webiste.png
[webmatrix-modify-index]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-edit.png
[webmatrix-republish]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-republish.png
[addaccount]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-add-account.png
[signin]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-sign-in.png
[sitefromtemplate]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-site-from-template.png
[nodesitefromtemplateazure]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-site-azure.png
 