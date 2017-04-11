<properties 
    pageTitle="Aplikace Node.js pomocí Socket.io | Microsoft Azure" 
    description="Naučte se používat socket.io aplikaci node.js hostitelem Azure." 
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

# <a name="build-a-nodejs-chat-application-with-socketio-on-an-azure-cloud-service"></a>Vytvoření aplikace Node.js konverzace s Socket.IO na Azure cloudové služby

Socket.IO poskytuje reálném čase komunikaci mezi klienty a node.js serveru. Tento kurz vás provede jednotlivými hostingu socket. Konverzace aplikace na Azure založené na vstupu a výstupu. Další informace o Socket.IO najdete v tématu <http://socket.io/>.

Snímek obrazovky s dokončeným aplikace je níže:

![Okno prohlížeče zobrazující služeb hostovaných na Azure][completed-app]  

## <a name="prerequisites"></a>Zjistit předpoklady pro

Zajistit, aby nainstalované následující produkty a verze k úspěšnému dokončení příkladu v tomto článku:

* Instalace [aplikace Visual Studio 2013](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx)
* Instalace [Node.js](https://nodejs.org/download/)
* Instalace [Python verze 2.7.10](https://www.python.org/)

## <a name="create-a-cloud-service-project"></a>Vytvoření projektu služby cloudu

Podle těchto kroků vytvořit projekt cloudové služby, který uspořádá Socket.IO aplikace.

1. V **Nabídce Start** nebo **Obrazovce Start**vyhledejte **Prostředí Windows PowerShell**. Nakonec klikněte pravým tlačítkem myši **Prostředí Windows PowerShell** a vyberte **Spustit jako správce**.

    ![Ikona Azure prostředí PowerShell][powershell-menu]

2. Vytvořte adresář s názvem **c:\\uzel**. 
 
        PS C:\> md node

3. Změna adresáře **c:\\uzel** adresáře
 
        PS C:\> cd node

4. Zadejte následující příkazy k vytvoření nové řešení s názvem **chatapp** a pracovní úlohu s názvem **WorkerRole1**:

        PS C:\node> New-AzureServiceProject chatapp
        PS C:\Node> Add-AzureNodeWorkerRole

    Zobrazí se následující odpověď:

    ![Výstupní nová azureservice a přidání azurenodeworkerrolecmdlets](./media/cloud-services-nodejs-chat-app-socketio/socketio-1.png)

## <a name="download-the-chat-example"></a>Stáhněte si příklad konverzace

Pro tento projekt použijeme příklad konverzace z [úložiště Socket.IO GitHub]. Provedení následujících kroků můžete stáhnout příkladu a přiřadit ji někomu projektu, který jste vytvořili.

1.  Vytvořte místní kopii úložiště pomocí tlačítka **klonovat** . Pomocí tlačítka **ZIP** mohou také stáhnout projektu.

    ![Zobrazení https://github.com/LearnBoost/socket.io/tree/master/examples/chat, se zvýrazněnou ikonou stažení ZIP okně prohlížeče][chat-example-view]

3.  Procházení struktury adresáře místní úložiště až na přejdete **Příklady\\konverzace** adresář. Zkopírujte obsah tohoto adresáře **C:\\uzel\\chatapp\\WorkerRole1** adresáře vytvořili.

    ![Průzkumník Windows zobrazení obsahu příklady\\konverzace adresáře extrahovaných z archivu][chat-contents]

    Zvýrazněné položky v snímek výše jsou soubory zkopírovali z **Příklady\\konverzace** adresáře

4.  V **C:\\uzel\\chatapp\\WorkerRole1** adresáře, odstraňte soubor **server.js** a přejmenujte soubor **app.js** na **server.js**. Tímto postupem odstraníte výchozí soubor **server.js** dříve vytvořené rutinu **Přidat AzureNodeWorkerRole** a nahradí ho souboru aplikace z příkladu konverzace.

### <a name="modify-serverjs-and-install-modules"></a>Úprava Server.js a nainstalovat moduly

Před testování aplikace v Azure emulátoru, jsme musíte provést některé menší změny. Proveďte následující kroky k souboru server.js:

1.  Otevřete soubor **server.js** ve Visual Studiu nebo jakémkoli textovém editoru.

2.  Vyhledání části **modul závislostí** na začátku server.js a změňte řádek obsahující **sio = require('.. //.. lib//Socket.IO ")** k **sio = require('socket.io')** jak je ukázáno v následujícím příkladu:

        var express = require('express')
        , stylus = require('stylus')
        , nib = require('nib')
        //, sio = require('..//..//lib//socket.io'); //Original
        , sio = require('socket.io');                //Updated

3.  Abyste měli jistotu, že aplikace sleduje správný port, otevřete server.js v programu Poznámkový blok nebo Oblíbené editor a poté změňte následující řádek nahrazením **3000** **process.env.port** jak je ukázáno v následujícím příkladu:

        //app.listen(3000, function () {            //Original
        app.listen(process.env.port, function () {  //Updated
          var addr = app.address();
          console.log('   app listening on http://' + addr.address + ':' + addr.port);
        });

Po uložení změn do **server.js**, použijte následující postup instalace požadovaných moduly a otestujte aplikace v Azure emulátoru:

1.  Pomocí **Prostředí PowerShell Azure**, změna adresáře **C:\\uzel\\chatapp\\WorkerRole1** adresář a použijte následující příkaz a nainstalovat moduly požadovaných touto aplikací:

        PS C:\node\chatapp\WorkerRole1> npm install

    Nainstaluje moduly jsou v něm package.json. Po dokončení příkazu byste měli vidět výstup následujícím způsobem:

    ![Výstup npm instalace příkaz][The-output-of-the-npm-install-command]

4.  Vzhledem k tomu v tomto příkladu byl původně součástí Socket.IO GitHub úložiště a knihovně Socket.IO přímo odkazováno relativní cestu, nebyla Socket.IO odkazuje v souboru package.json, takže jsme nainstalujte zadáním následujícího příkazu:

        PS C:\node\chatapp\WorkerRole1> npm install socket.io --save

### <a name="test-and-deploy"></a>Otestujte a nasazení

1.  Spuštění emulátor zadáním tento příkaz:

        PS C:\node\chatapp\WorkerRole1> Start-AzureEmulator -Launch

2.  Otevřete prohlížeč a přejděte na **http://127.0.0.1**.

3.  Až se otevře okno prohlížeče, zadejte zástupný a potom stisknutí klávesy enter.
    Budou všechny umožňuje publikovat zprávy jako zástupný konkrétní. Testování funkcí více uživatelů, otevřete další okna prohlížeče použití stejné adresy URL a zadejte jiné přezdívky.

    ![Zobrazení zprávy v konverzaci od uživatel 1 a uživatel2 dvě okna prohlížeče](./media/cloud-services-nodejs-chat-app-socketio/socketio-8.png)

3.  Po testování aplikace, ukončete emulátor zadáním následujícího příkazu:

        PS C:\node\chatapp\WorkerRole1> Stop-AzureEmulator

4.  Abyste mohli nasadit aplikaci Azure, získáte pomocí rutiny **AzureServiceProject publikovat** . Příklad:

        PS C:\node\chatapp\WorkerRole1> Publish-AzureServiceProject -ServiceName mychatapp -Location "East US" -Launch

    > [AZURE.IMPORTANT] Je nutné použít jedinečný název, jinak se nezdaří publikovat. Po dokončení nasazení prohlížeči otevřete a přejděte na nasazeném služby.
    > 
    > Pokud se zobrazí chybová zpráva s informacemi v profilu importovaných publikování neexistuje název zadané předplatného, musí stáhnout a importujte profil publikování pro vaše předplatné před nasazením Azure. **Nasazení aplikace Azure** si část [sestavovat a nasazovat aplikace Node.js cloudové služby Azure](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)

    ![Okno prohlížeče zobrazující služeb hostovaných na Azure][completed-app]

    > [AZURE.NOTE] Pokud se zobrazí chybová zpráva s informacemi v profilu importovaných publikování neexistuje název zadané předplatného, musí stáhnout a importujte profil publikování pro vaše předplatné před nasazením Azure. **Nasazení aplikace Azure** si část [sestavovat a nasazovat aplikace Node.js cloudové služby Azure](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)

Aplikace je teď spuštěna Azure a můžete přenášet zprávy konverzace mezi různých klientů pomocí Socket.IO.

> [AZURE.NOTE] Pro zjednodušení se omezí na konverzace mezi uživateli připojené k stejné instanci v tomto příkladu. To znamená, že pokud cloudovou službu vytvoří dvě instance pracovního role, uživatelé pouze budou moct chatovat s ostatními připojené k stejné instanci pracovního rolí. Zobrazit aplikace pro práci s několika instancích spuštěných role, můžete použít technologii jako služba Bus sdílení stavu úložiště Socket.IO všech instancí. Příklady najdete v článku příklady použití služby Bus fronty a témata v [Azure SDK Node.js GitHub úložiště](https://github.com/WindowsAzure/azure-sdk-for-node).

##<a name="next-steps"></a>Další kroky

V tomto kurzu se naučíte vytvořit základní chatovací aplikace použitý ve cloudové služby Azure. Informace o hostování tuto aplikaci webu Azure, najdete v článku [Vytvoření aplikace Node.js konverzace s Socket.IO na Azure webu technologie][chatwebsite].

Další informace najdete v tématu taky [Středisko pro vývojáře Node.js](/develop/nodejs/).

  [chatwebsite]: /develop/nodejs/tutorials/website-using-socketio/

  [Azure SLA]: http://www.windowsazure.com/support/sla/
  [Azure SDK for Node.js GitHub repository]: https://github.com/WindowsAzure/azure-sdk-for-node
  [completed-app]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-10.png
  [Azure SDK for Node.js]: https://www.windowsazure.com/develop/nodejs/
  [Node.js Web Application]: https://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
  [Socket.IO GitHub úložiště]: https://github.com/LearnBoost/socket.io/tree/0.9.14
  [Azure Considerations]: #windowsazureconsiderations
  [Hosting the Chat Example in a Worker Role]: #hostingthechatexampleinawebrole
  [Summary and Next Steps]: #summary
  [powershell-menu]: ./media/cloud-services-nodejs-chat-app-socketio/azure-powershell-start.png

  [chat example]: https://github.com/LearnBoost/socket.io/tree/master/examples/chat
  [chat-example-view]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-22.png
  
  
  [chat-contents]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-5.png
  [The-output-of-the-npm-install-command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-7.png
  [The output of the Publish-AzureService command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-9.png
  
 
