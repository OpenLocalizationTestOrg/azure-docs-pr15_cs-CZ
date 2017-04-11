<properties 
    pageTitle="Nasazení webovou aplikaci první PHP Azure pět minut | Microsoft Azure" 
    description="Zjistěte, jak je snadné spuštění webových aplikací web apps v aplikaci služby nasazením ukázkové aplikace. Spusťte rychle udělat skutečný rozvoj a okamžitě zobrazit výsledky." 
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/13/2016" 
    ms.author="cephalin"
/>
    
# <a name="deploy-your-first-php-web-app-to-azure-in-five-minutes"></a>Nasazení webovou aplikaci první PHP Azure pět minut

Tento kurz vám pomůže publikovat svoji první webovou aplikaci PHP [aplikace](../app-service/app-service-value-prop-what-is.md)služby Azure.
Aplikace služby slouží k vytváření webových aplikací web apps, [mobilní aplikace zpět koncích](/documentation/learning-paths/appservice-mobileapps/)a [rozhraní API aplikace](../app-service-api/app-service-api-apps-why-best-platform.md).

Udělejte toto: 

- Vytvořte web appu v aplikaci služby Azure.
- Nasazení kódu PHP vzorku.
- Zobrazit kód spuštěný žijí výroby.
- Aktualizujte svoji webovou aplikaci stejným způsobem jako by [push, které potvrdí libovolná](https://git-scm.com/docs/git-push).

## <a name="prerequisites"></a>Zjistit předpoklady pro

- [Libovolná](http://www.git-scm.com/downloads).
- [Azure rozhraní příkazového řádku](../xplat-cli-install.md).
- Účet Microsoft Azure. Pokud nemáte účet, můžete [Registrace bezplatnou zkušební verzi](/pricing/free-trial/?WT.mc_id=A261C142F) nebo [Aktivujte své výhody odběratele Visual Studio](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

>[AZURE.NOTE] Pokud nepoužíváte účet Azure můžete [Zkuste aplikaci služby](http://go.microsoft.com/fwlink/?LinkId=523751) . Vytvoření aplikace starter a přehrávat s ním pro až jednu hodinu – žádné povinné platební kartou, žádné závazky.

## <a name="deploy-a-php-web-app"></a>Nasazení aplikace od web PHP

1. Otevřete nový příkazového řádku systému Windows, okna prostředí PowerShell, Linux prostředí nebo terminálu OS X. Spuštění `git --version` a `azure --version` můžete ověřit, jestli libovolná a rozhraní příkazového řádku Azure jsou nainstalované v počítači.

    ![Otestování instalace nástroje rozhraní příkazového řádku pro první web app v Azure](./media/app-service-web-get-started/1-test-tools.png)

    Pokud jste nenainstalovali nástroje, najdete v článku [požadavky](#Prerequisites) pro stahování odkazy.

3. Přihlaste se k Azure takto:

        azure login

    Postupujte podle nápovědu pokračujte proces přihlášení.

    ![Přihlaste se k Azure při vytváření prvního webové aplikace](./media/app-service-web-get-started/3-azure-login.png)

4. Změňte Azure rozhraní příkazového řádku do režimu ASM a pak nastavení uživatele nasazení pro aplikaci služby. Budete nasazovat kód pomocí pověření později.

        azure config mode asm
        azure site deployment user set --username <username> --pass <password>

1. Změna pracovního adresáře (`CD`) a klonovat aplikaci vzorku takto:

        git clone https://github.com/Azure-Samples/app-service-web-php-get-started.git

2. Změna do úložiště aplikace vzorku. Příklad:

        cd app-service-web-php-get-started

4. Vytvoření zdrojů aplikace aplikaci služby v Azure s názvem jedinečné aplikace a nasazení uživatele, která odrážejí dříve konfigurované. Pokud se zobrazí výzva, zadejte počet na požadovanou oblast.

        azure site create <app_name> --git --gitusername <username>

    ![Vytvoření Azure prostředků pro webovou aplikaci první v Azure](./media/app-service-web-get-started-languages/php-site-create.png)

    Vaše je aplikace vytvořená v Azure nyní. Aktuálnímu adresáři je také inicializován libovolná a připojit k aplikaci novou aplikaci služby jako libovolná vzdálené.
    Přejdete na adresu URL aplikace (http://&lt;název_aplikace >. azurewebsites.net) najdete v článku krásné výchozí stránky HTML, ale ve skutečnosti nastavíme kódu tam nyní.

4. Nasazení ukázkový kód Azure aplikace, jako by push jakýkoli kód s libovolná. Po zobrazení výzvy: použijte heslo, která odrážejí dříve konfigurované.

        git push azure master

    ![Použít kód pro první webovou aplikaci v Azure](./media/app-service-web-get-started-languages/php-git-push.png)

    `git push`pouze umístí kód Azure, ale taky spouští úlohy nasazení v modulu nasazení. Můžete taky  [Povolit rozšíření autora](web-sites-php-mysql-deploy-use-git.md#composer) automaticky zpracovávat composer.json souborů v aplikaci PHP.

Gratulujeme, nasazení aplikace služby Azure aplikace.

## <a name="see-your-app-running-live"></a>V tématu spuštění živé aplikace

Aplikace spuštěné žijí Azure zobrazíte spusťte z libovolného adresáře v úložišti tento příkaz:

    azure site browse

## <a name="make-updates-to-your-app"></a>Provést aktualizace aplikace

Teď můžete libovolná posunout z kdykoli kořenu projektu (úložiště) Pokud chcete provést aktualizaci živou webu. Můžete to udělat stejně jako při nasazení kódu poprvé. Například pokaždé, když budete chtít nabízená nové změny, které jste testováno místně, jednoduše spusťte následující příkazy z kořenového adresáře projektu (úložiště):

    git add .
    git commit -m "<your_message>"
    git push azure master

## <a name="next-steps"></a>Další kroky

[Vytvoření, konfigurace a nasadit web app Laravel Azure](app-service-web-php-get-started.md). Provedením tohoto kurzu se dozvíte základní dovednosti, které je potřeba spustit nějakou aplikaci web PHP v Azure, například:

- Vytváření a konfigurace aplikací v Azure z prostředí PowerShell/flám.
- Nastavte PHP verze.
- Použijte start soubor, který není uveden v kořenovém adresáři aplikace.
- Povolte automatické autora.
- Access prostředí specifické proměnné.
- Odstranění běžných chyb.

Nebo další s webovou aplikaci první. Příklad:

- Vyzkoušejte [Další způsoby nasazení kódu Azure](../app-service-web/web-sites-deploy.md). Například pro nasazení z jednoho z úložiště GitHub, jednoduše vyberte **GitHub** místo **Místní úložiště libovolná** v dialogovém okně **Možnosti nasazení**.
- Používání aplikace Azure vyšší úrovni. Ověřování uživatelů. Škála podle služba. Nastavte si některé výstrahy výkonu. Všechny několika kliknutími. V tématu [Přidání funkcí do prvního webové aplikace](app-service-web-get-started-2.md).

