<properties
    pageTitle="Node.js příručku Začínáme s | Microsoft Azure"
    description="Zjistěte, jak vytvořit jednoduchý webovou aplikaci Node.js a nasazení služby Azure cloudu."
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
    ms.topic="hero-article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="build-and-deploy-a-nodejs-application-to-an-azure-cloud-service"></a>Vytvoření a nasazení aplikace Node.js cloudové služby Azure

> [AZURE.SELECTOR]
- [Node.js](cloud-services-nodejs-develop-deploy-app.md)
- [.NET](cloud-services-dotnet-get-started.md)

Tento kurz ukazuje, jak vytvořit jednoduchý Node.js aplikace do cloudové služby Azure. Cloud Services jsou stavebních bloků scalable cloudové aplikace v Azure. Umožňují oddělení a nezávislé správní a škálování front-end a back-end komponenty aplikace.  Cloudovým službám poskytují robustní vyhrazené virtuálního počítače pro každou roli problémy se spolehlivým hostingu.

Další informace o Cloudovým službám a jaký je rozdíl mezi veřejným Azure weby a virtuálních počítačích najdete v článku [Azure weby, cloudovými službami a porovnání virtuálních počítačích].

>[AZURE.TIP] Hledáte, jak vytvořit jednoduchý webu? Pokud váš scénář zahrnuje jenom jednoduché Web front-end, zvažte [použití zjednodušené web appu]. Můžete snadno upgradujete do cloudové služby webovou aplikaci roste a změnit vašim požadavkům.

Provedením tohoto kurzu vytvoříte jednoduché webovou aplikaci umístěn uvnitř roli web. Bude pomocí emulátoru výpočetním otestovat místně aplikace a potom nasazena pomocí prostředí PowerShell nástroje příkazového řádku.

Aplikace je jednoduchý "Vítáme" aplikace:

![Zobrazení webové stránky Vítáme webového prohlížeče][A web browser displaying the Hello World web page]

## <a name="prerequisites"></a>Zjistit předpoklady pro

> [AZURE.NOTE] Tento kurz používá Azure PowerShell, který vyžaduje operační systém Windows.

- Instalace a konfigurace [Prostředí Powershell Azure].
- Stáhněte a nainstalujte [Azure SDK pro .NET 2.7]. V nastavení instalace vyberte:
    - MicrosoftAzureAuthoringTools
    - MicrosoftAzureComputeEmulator


## <a name="create-an-azure-cloud-service-project"></a>Vytvoření projektu cloudové služby Azure

Proveďte následující úkoly k vytvoření nového projektu Azure cloudové služby, spolu s základní vygenerovaných Node.js:

1. Spustit jako správce; **prostředí Windows PowerShell** v **Nabídce Start** nebo **Obrazovce Start**vyhledejte **Prostředí Windows PowerShell**.

2. [Prostředí PowerShell připojit] k předplatnému.

3. Zadejte následující rutinu Powershellu vytvořit vytvořte projekt:

        New-AzureServiceProject helloworld

    ![Výsledku příkazu Nový AzureService Hello World][The result of the New-AzureService helloworld command]

    Rutinu **New-AzureServiceProject** generuje pro publikování aplikace Node.js do cloudové služby základní struktura. Konfigurace soubory potřebné pro publikování Azure v ní. Rutiny k adresáři služby změníte pracovní adresář.

    Rutiny vytvoří následující soubory:

    -   **ServiceConfiguration.Cloud.cscfg**, **ServiceConfiguration.Local.cscfg** a **ServiceDefinition.csdef**: Azure konkrétní soubory potřebné pro publikování aplikace. Další informace najdete v tématu [základní informace o vytváření hostované služby Azure].

    -   **deploymentSettings.json**: ukládá místní nastavení, které využívají rutiny prostředí PowerShell Azure nasazení.

4.  Zadejte tento příkaz Přidat novou roli web:

        Add-AzureNodeWebRole

    ![Výstup příkazu přidat AzureNodeWebRole][The output of the Add-AzureNodeWebRole command]

    Rutina **Přidat AzureNodeWebRole** vytvoří základní Node.js aplikaci. Také upraví **.csfg** a **.csdef** soubory k přidání položek konfigurace pro novou roli.

    > [AZURE.NOTE] Pokud nezadáte název role, použije se výchozí název. Zadejte název jako první parametr rutinu:`Add-AzureNodeWebRole MyRole`

Aplikaci Node.js je definována v souboru **server.js**, umístěnou v adresáři webu rolí (**WebRole1** ve výchozím nastavení). Tady je kód:

    var http = require('http');
    var port = process.env.port || 1337;
    http.createServer(function (req, res) {
        res.writeHead(200, { 'Content-Type': 'text/plain' });
        res.end('Hello World\n');
    }).listen(port);

Tento kód je v podstatě totéž jako ukázka "Ahoj světe" na webu [nodejs.org] kromě používala číslo portu přiřazeného prostředím cloudu.

## <a name="deploy-the-application-to-azure"></a>Nasazení aplikace Azure

    [AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

### <a name="download-the-azure-publishing-settings"></a>Stáhněte si Azure nastavení publikování

Abyste mohli nasadit aplikaci Azure, budete muset nejdřív stahovat nastavení publikování předplatného Azure.

1.  Spusťte následující rutinu Powershellu Azure:

        Get-AzurePublishSettingsFile

    Přejděte na stránku pro stažení nastavení publikovat to bude pomocí prohlížeče. Můžete být vyzváni k přihlášení pomocí Account Microsoft. Pokud ano, pomocí účtu spojeného s předplatným Azure.

    Stažený profil uložte do souboru umístěného, ke kterým můžete snadno přistupovat.

2.  Spusťte následující rutinu importu publikování profil, který jste stáhli:

        Import-AzurePublishSettingsFile [path to file]


    > [AZURE.NOTE] Po importu nastavení publikování, zvažte možnost odstranění .publishSettings staženého souboru, protože obsahuje informace, které by mohly umožnit někomu přístup ke svému účtu.

### <a name="publish-the-application"></a>Publikování aplikace

Pokud chcete publikovat, spusťte následující příkazy:

    $ServiceName = "NodeHelloWorld" + $(Get-Date -Format ('ddhhmm'))   
    Publish-AzureServiceProject -ServiceName $ServiceName  -Location "East US" -Launch

- **-** Název_služby název pro nasazení. Musí být jedinečný název, jinak se nepovede procesu publikování. Příkaz **Get-datum** cvočků na řetězec datum a čas, který připomínat název jedinečný.

- **– Umístění** určuje datacentra, které aplikaci se použitý ve. Zobrazíte seznam dostupných datacentrech získáte pomocí rutiny **Get-AzureLocation** .

- **– Spuštění** otevřete okno prohlížeče a zobrazí hostovanou službu po dokončení nasazení.

Po úspěšném publikování, zobrazí se podobně jako tento odpověď:

![Výstup příkaz Publikovat AzureService][The output of the Publish-AzureService command]

> [AZURE.NOTE]
> Může trvat několik minut pro nasazení a je k dispozici při prvním publikování aplikace.

Po dokončení nasazení okně prohlížeče otevřít a přejděte do cloudové služby.

![Zobrazení stránky světě Ahoj; okně prohlížeče Adresa URL označuje, že na stránce je hostovaný ve Azure.][A browser window displaying the hello world page; the URL indicates the page is hosted on Azure.]

Aplikace je teď v Azure spuštěna.

Rutina **Publikovat AzureServiceProject** provede následující kroky:

1.  Vytvoří balíček pro nasazení. Balíček obsahuje všechny soubory ve složce aplikace.

2.  Vytvoří nový **účet úložiště** , pokud ještě neexistuje. Azure úložiště účet se používá k ukládání balíčku aplikace během nasazení. Bezpečné odstraněním účtu úložiště po dokončení nasazení.

3.  Vytvoří novou **cloudové služby** , pokud už neexistuje. Do **cloudové služby** je kontejner hostuje vaše aplikace nasazené na Azure. Další informace najdete v tématu [základní informace o vytváření hostované služby Azure].

4.  Publikuje balíček pro nasazení na Azure.


## <a name="stopping-and-deleting-your-application"></a>Zastavení a odstranění části aplikace

Po nasazení aplikace, můžete ho zakázat tak, aby se můžete vyhnout navíc náklady. Azure faktury webové role instancí za hodinu spotřebované množství času serveru. Čas serveru je spotřebované množství po nasazení aplikace, i když instance se nepoužívá a jsou v přestal stavu.

1.  V okně prostředí Windows PowerShell ukončení nasazení služby vytvořený v předchozí části s následující rutinu:

        Stop-AzureService

    Zastavení služby může trvat několik minut. Zastavení služby, zobrazí se zpráva oznamující, že přestal.

    ![Stav příkazu AzureService zastavit][The status of the Stop-AzureService command]

2.  Pokud chcete odstranit službu, kontaktujte následující rutinu:

        Remove-AzureService

    Po zobrazení výzvy zadejte **Y** k odstranění služby.

    Odstranění služby může trvat několik minut. Po odstranění službu zobrazí se zpráva oznamující, že došlo k odstranění službu.

    ![Stav příkazu odebrat AzureService][The status of the Remove-AzureService command]

    > [AZURE.NOTE] Odstranění služby neodstraní úložiště účet, který byl vytvořen při služby počáteční publikování a vám budou zasílané vám nebudou účtovat poplatky úložiště. Pokud nic jiného používá úložiště, můžete ho odstranit.

## <a name="next-steps"></a>Další kroky

Další informace najdete v tématu [Node.js středisko pro vývojáře].

<!-- URL List -->

[Azure porovnání weby, cloudovými službami a virtuálních počítačích]: ../app-service-web/choose-web-site-cloud-service-vm.md
[použití zjednodušené web appu]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
[Azure Powershellu]: ../powershell-install-configure.md
[Azure SDK pro .NET 2.7]: http://www.microsoft.com/en-us/download/details.aspx?id=48178
[Připojení prostředí PowerShell]: ../powershell-install-configure.md#how-to-connect-to-your-subscription
[nodejs.org]: http://nodejs.org/
[Přehled vytváření hostovanou službu Azure]: https://azure.microsoft.com/documentation/services/cloud-services/
[Středisko pro vývojáře Node.js]: https://azure.microsoft.com/develop/nodejs/

<!-- IMG List -->

[The result of the New-AzureService helloworld command]: ./media/cloud-services-nodejs-develop-deploy-app/node9.png
[The output of the Add-AzureNodeWebRole command]: ./media/cloud-services-nodejs-develop-deploy-app/node11.png
[A web browser displaying the Hello World web page]: ./media/cloud-services-nodejs-develop-deploy-app/node14.png
[The output of the Publish-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node19.png
[A browser window displaying the hello world page; the URL indicates the page is hosted on Azure.]: ./media/cloud-services-nodejs-develop-deploy-app/node21.png
[The status of the Stop-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node48.png
[The status of the Remove-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node49.png
