<properties
    pageTitle="Libovolná místního nasazení služby Azure aplikace"
    description="Zjistěte, jak povolit místní nasazení libovolná aplikace služby Azure."
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/13/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="local-git-deployment-to-azure-app-service"></a>Libovolná místního nasazení služby Azure aplikace

Tomto kurzu se dozvíte, jak pro nasazení aplikace služby [Azure aplikace] z úložiště libovolná ve vašem počítači. Služba aplikace podporuje tento přístup s možností nasazení **Místní libovolná** [Portálu Azure].  
V mnoha libovolná příkazy popisované v tomto článku provádí automaticky při vytváření aplikace pro aplikaci služby pomocí [rozhraní příkazového řádku Azure] jako popsán [v tomto poli](app-service-web-get-started.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

K provedení tohoto kurzu, budete potřebovat:

- Libovolná. Stáhněte si instalace binární [tady](http://www.git-scm.com/downloads).  
- Základní znalost libovolná.
- Účet Microsoft Azure. Pokud nemáte účet, můžete [Registrace bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial) nebo [Aktivujte své výhody odběratele Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details).

>[AZURE.NOTE] Pokud chcete začít pracovat s aplikaci služby Azure před registrací účet Azure, přejděte na [Zkuste aplikaci služby](http://go.microsoft.com/fwlink/?LinkId=523751), kde můžete okamžitě vytvořit aplikaci krátkodobý starter v aplikaci služby. Žádné povinné; kreditní karty žádné závazky.  

## <a name="Step1"></a>Krok 1: Vytvoření místní úložiště

Proveďte následující úkoly, čímž vytvoříte novou libovolná úložiště.

1. Spusťte nástroj příkazového řádku, například **GitBash** (Windows) nebo **flám** (Unix Shell). V systémech OS X dostanete příkazového řádku pomocí aplikace **terminálu** .

2. Přejděte v adresáři, kde bude umístěný obsahu pro nasazení.

3. Inicializace úložiště nové libovolná použijte tento příkaz:

        git init

## <a name="Step2"></a>Krok 2: Potvrzení obsahu

Služba aplikace podporuje aplikace vytvořené různými jazyky. 

1. Pokud už obsahu přeskočit úložiště to najeďte myší a přesuňte tak, aby ukazovaly 2 dole. Pokud úložiště už neobsahuje obsah jednoduše naplnění souboru statické .html následujícím způsobem: 

    - V textovém editoru, vytvoříte nový soubor s názvem **index.html** na kořenové úrovni libovolná úložiště
    - Přidejte následující text a obsah index.html soubor uložit, abyste ji: *Libovolná Ahoj!*
        
2. Z příkazového řádku ověřte, jestli jsou v kořenovém adresáři libovolná úložiště. Chcete-li přidat soubory do úložiště pak pomocí příkazu následující:

        git add -A 

4. Pak potvrďte změny provedené v úložišti pomocí tento příkaz:

        git commit -m "Hello Azure App Service"

## <a name="Step3"></a>Krok 3: Povolení aplikace úložiště aplikace služby

Provedení následujících kroků můžete povolit úložiště libovolná aplikace aplikaci služby.

1. Přihlaste se k [portálu Azure].

2. V aplikaci služby aplikace zásuvné klikněte na **Nastavení > Zdroj zavedení**. Klikněte na **Zvolit zdroj**, a pak klikněte na **Místní úložiště libovolná**a klikněte na **OK**.  

    ![Místní libovolná úložiště](./media/app-service-deploy-local-git/local_git_selection.png)

3. Pokud je to vaše první nastavujete úložiště v Azure, musíte pro něj vytvořte přihlašovací údaje. Je bude používat pro přihlášení na Azure změny úložiště a nabízených z místní úložiště libovolná. Na zásuvné vaše aplikace klikněte na **Nastavení > pověření nasazení**, potom i konfiguraci nasazení uživatelské jméno a heslo. Až budete hotovi, klikněte na **Uložit**.

    ![](./media/app-service-deploy-local-git/deployment_credentials.png)

## <a name="Step4"></a>Krok 4: Nasazení projektu

Publikování aplikace služby aplikace pomocí místní libovolná pomocí následujících kroků.

1. Ve vaší aplikaci zásuvné na portálu Azure, klikněte na **Nastavení > Vlastnosti** pro adresu **URL libovolná**.

    ![](./media/app-service-deploy-local-git/git_url.png)

    Je **Libovolná adresa URL** vzdálené odkaz na nasazení z místní úložiště. V následujících krocích, které budete používat tuto adresu URL.

2. Pomocí příkazového řádku, zkontrolujte, že jste v kořenovém místní úložiště libovolná.

3. Použití `git remote` k přidání odkazu na vzdáleném uvedené v **Adrese URL libovolná** z kroku 1. Váš příkaz bude vypadat podobně jako tento:

        git remote add azure https://<username>@localgitdeployment.scm.azurewebsites.net:443/localgitdeployment.git         
    > [AZURE.NOTE] **Vzdáleného** příkazu přidá pojmenované odkaz do vzdálené úložiště. V tomto příkladu vytvoří odkaz s názvem "azure" pro webovou aplikaci úložiště.

4. Použít obsahu pro aplikaci služby pomocí nové **azure** vzdálené jste právě vytvořili.

        git push azure master

    Zobrazí se výzva k zadání hesla, které jste vytvořili při obnovení pověření nasazení na portálu Azure. Zadejte heslo (Všimněte si, že Gitbash není vypsat hvězdičky na konzole zadávání svoje heslo). 
       
5. Přejděte zpět do aplikace na portálu Azure. Zadání protokolu posledních nabízených se má zobrazit v zásuvné **nasazeních** . 

    ![](./media/app-service-deploy-local-git/deployment_history.png)

6. Kliknutím na tlačítko **Procházet** v horní části aplikace na zásuvné ověřte, zda že byl používaný obsah. 
    
## <a name="Step5"></a>Řešení potíží

Následující seznam uvádí chyby nebo problémy běžně při publikování aplikace služby aplikace v Azure pomocí libovolná:

****

**Příznak**: Nelze získat přístup k [siteURL]: nepovedlo se připojit k [scmAddress]

**Příčina**: k této chybě může dojít, pokud není aplikace do začátků.

**Řešení**: Spusťte aplikaci na portálu Azure. Libovolná nasazení nebudou fungovat, pokud je spuštěná aplikace. 


****

**Příznak**: nelze přeložit hostitele hostname

**Příčina**: k této chybě může dojít, pokud adresa informace zadané při vytváření azure vzdálené nesprávný.

**Řešení**: použijte `git remote -v` příkazu zobrazíte všechny dálkových spolu s přidruženou adresu URL. Ověřte správnost adresu URL "azure" vzdálené. V případě potřeby odeberte a znovu vytvořit toto vzdáleného použití správnou adresu URL.

****

**Příznak**: žádné odkazy v společné a nikdo stanovit; žádným způsobem. Možná je třeba zadat pobočky například "předlohy".

**Příčina**: k této chybě může dojít, pokud nezadáte pobočky při provádění libovolná nabízená operace a nenastavili hodnotu push.default používaný libovolná.

**Řešení**: provádění nabízených akci, zadání větvi předlohy. Příklad:

    git push azure master

****

**Příznak**: src refspec [branchname] neodpovídá žádné.

**Příčina**: k této chybě může dojít, pokud se pokusíte Zatlačit větve než předlohy na azure vzdálené.

**Řešení**: provádění nabízených akci, zadání větvi předlohy. Příklad:

    git push azure master

****

**Příznak**: Chyba: změny potvrzeného do vzdálené úložiště, ale webovou aplikaci neaktualizuje.

**Příčina**: k této chybě může dojít, pokud jsou nasazení aplikace Node.js obsahující package.json soubor, který určuje další požadované moduly.

**Řešení**: další zprávy obsahující "npm chyba!" musí být protokolované před tuto chybu a můžete kontext další na chybu. Následující seznam uvádí známé příčiny této chyby a odpovídající "npm chyba!" zpráva:

* **Soubor poškozený package.json**: npm chyba! Nelze číst závislosti.

* **Nativní modul, který nemá binární rozdělení pro systém Windows**:

    * npm chyba! \`cmd "/ c" "uzel gyp opětovného vytvoření"\` failed číslem 1

        NEBO

    * npm chyba! [modulename@version]předinstalovat: \`zkontrolujte || gmake\`


## <a name="additional-resources"></a>Další zdroje informací

* [Libovolná si přečtěte následující dokumentaci](http://git-scm.com/documentation)
* [Si přečtěte následující dokumentaci Kudu projektu](https://github.com/projectkudu/kudu/wiki)
* [Nepřetržité nasazení služby Azure aplikace](app-service-continuous-deployment.md)
* [Jak pomocí Powershellu pro Azure](../powershell-install-configure.md)
* [Jak používat rozhraní příkazového řádku Azure](../xplat-cli-install.md)

[Azure aplikace služby]: https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/
[Azure Developer Center]: http://www.windowsazure.com/en-us/develop/overview/
[Azure portálu]: https://portal.azure.com
[Git website]: http://git-scm.com
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Azure rozhraní příkazového řádku]: https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-azure-resource-manager/

[Using Git with CodePlex]: http://codeplex.codeplex.com/wikipage?title=Using%20Git%20with%20CodePlex&referringTitle=Source%20control%20clients&ProjectName=codeplex
[Quick Start - Mercurial]: http://mercurial.selenic.com/wiki/QuickStart
