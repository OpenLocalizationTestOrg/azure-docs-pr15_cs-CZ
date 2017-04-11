<properties
    pageTitle="Určení verze Node.js"
    description="Zjistěte, jak můžete určit verzi Node.js používaných Azure weby a Cloudovým službám"
    services=""
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="specifying-a-nodejs-version-in-an-azure-application"></a>Určení verze Node.js v aplikaci Azure

Hostování aplikace Node.js, můžete zajistit, aby používala vaši aplikaci konkrétní verzi Node.js. Existuje několik způsobů, jak provést aplikací hostitelem Azure.

##<a name="default-versions"></a>Výchozí verze

Verze Node.js poskytovanou Azure jsou průběžně aktualizovány. Pokud není uvedeno jinak, na výchozí verzi, který je uveden v `WEBSITE_NODE_DEFAULT_VERSION` proměnnou se použijí. Chcete-li změnit výchozí hodnotu, postupujte podle pokynů v následující části tohoto článku

> [AZURE.NOTE] Pokud jste hostitelem aplikace v cloudu služby Azure (webu nebo pracovního role) a je poprvé nasadili aplikace, Azure pokusí se použít stejnou verzi aplikace Node.js při instalaci na vývojové prostředí se shoduje s některou k dispozici v Azure výchozí verze.

##<a name="versioning-with-packagejson"></a>Správa verzí s package.json

Je možné zadat verzi Node.js se nemusí používat přidáním následující **package.json** soubor:

    "engines":{"node":version}

Kde *verze* je číslo konkrétní verzi používat. Je možné můžete zadat složitější podmínky pro verzi, jako například:

    "engines":{"node": "0.6.22 || 0.8.x"}

Protože 0.6.22 není k dispozici v hostitelské prostředí verze, nejvyšší verzi 0,8 řady, která je dostupná bude použita - 0.8.4.

##<a name="versioning-websites-with-app-settings"></a>Správa verzí weby s nastavením aplikace
Pokud jste hostitelem aplikace na webu, můžete nastavit proměnnou prostředí **WEBSITE_NODE_DEFAULT_VERSION** na požadovanou verzi. 

##<a name="versioning-cloud-services-with-powershell"></a>Správa verzí Cloudovým službám pomocí prostředí PowerShell

Pokud jste hostitelem aplikaci do cloudové služby a nasazení aplikace pomocí prostředí PowerShell Azure, můžete přepsat výchozí verze Node.js pomocí rutiny prostředí PowerShell **Set-AzureServiceProjectRole** . Příklad:

    Set-AzureServiceProjectRole WebRole1 Node 0.8.4

Poznámka: parametrů v předchozím výrazu malá a velká písmena.  Můžete ověřit že kontrolou vlastnost **moduly pro řeč** v vaše role **package.json**byla vybrána správná Node.js.

Můžete taky **Get-AzureServiceProjectRoleRuntime** načtěte seznam Node.js verzí k dispozici pro aplikace hostované jako do cloudové služby.  Vždy ověření verze Node.js projektu závisí na tom, v tomto seznamu.

##<a name="using-a-custom-version-with-azure-websites"></a>Použití vlastní verze se Azure weby

Zatímco Azure poskytuje několik výchozí verze Node.js, je vhodné používat verzi, která není k dispozici ve výchozím nastavení. Pokud aplikace je hostovaný jako Azure webu, můžete toho můžete dosáhnout pomocí souboru **iisnode.yml** . Projděte si proces používání vlastní verzi Node.Js s webem Azure podle těchto kroků:

1. Vytvořte nový adresář a pak vytvořit soubor **server.js** v rámci adresáře. Soubor **server.js** by měl obsahovat takto:

        var http = require('http');
        http.createServer(function(req,res) {
          res.writeHead(200, {'Content-Type': 'text/html'});
          res.end('Hello from Azure running node version: ' + process.version + '</br>');
        }).listen(process.env.PORT || 3000);

    Zobrazí se verze Node.js použitá při procházení webu.

2. Vytvořit nový web a zapište si název webu. Například následující používá [Azure nástroje příkazového řádku] pro vytvoření nového webu Azure s názvem **mywebsite**a pak ji povolit úložiště libovolná webu.

        azure site create mywebsite --git

3. Vytvořte nový adresář s názvem **Koš** jako podřízenou položku adresáři souborem **server.js** .

4. Stáhněte si konkrétní verzi **node.exe** (verze Windows), který chcete použít s aplikací. Například následující používá **otáčení** pro verzi 0.8.1 stáhnout:

        curl -O http://nodejs.org/dist/v0.8.1/node.exe

    Uložte soubor **node.exe** do složky **Koš** vytvořili v předchozích krocích.

5. Vytvoření souboru **iisnode.yml** v adresáři stejný jako soubor **server.js** a potom přidejte následující obsah do souboru **iisnode.yml** :

        nodeProcessCommandLine: "D:\home\site\wwwroot\bin\node.exe"

    Tato cesta je místo, kam **node.exe** souborů v rámci projektu budou umístěné Jakmile jste publikovali aplikace na web Azure.

6. Publikování aplikace. Třeba od můžu nový web s parametrem – libovolná dříve vytvořili, následující příkazy přidáte soubory aplikace do mé místní úložiště libovolná a potom nabízená do úložiště webu:

        git add .
        git commit -m "testing node v0.8.1"
        git push azure master

    Po provedení publikoval, otevřete web v prohlížeči. Měla by se zobrazit zpráva "Ahoj z Azure pracovního uzel verze: v0.8.1".

##<a name="next-steps"></a>Další kroky

Teď, když víte, jak chcete zadat verzi Node.js používané aplikací, zjistěte, jak [pracovat s modulů kontroly], [sestavovat a nasazovat Node.js webu]a [jak používat nástroje Azure příkazového řádku pro Mac a Linux].

Další informace najdete v tématu [Středisko pro vývojáře Node.js](/develop/nodejs/).

[Jak používat nástroje Azure příkazového řádku pro Mac a Linux]: xplat-cli-install.md
[Azure nástroje příkazového řádku]: xplat-cli-install.md
[Práce s moduly]: nodejs-use-node-modules-azure-apps.md
[Vytvořte a nasaďte Node.js webu]: web-sites-nodejs-develop-deploy-mac.md
