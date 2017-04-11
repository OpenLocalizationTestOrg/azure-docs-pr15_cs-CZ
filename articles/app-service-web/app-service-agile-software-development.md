<properties
    pageTitle="Aktivní software development aplikace službou Azure"
    description="Naučte se vytvářet složité aplikace maximum měřítko aplikace službou Azure způsobem, který podporuje aktivní software development."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/01/2016"
    ms.author="cephalin"/>


# <a name="agile-software-development-with-azure-app-service"></a>Aktivní software development aplikace službou Azure #

V tomto kurzu se dozvíte, jak vytvářet složité aplikace maximum měřítko [aplikace](/services/app-service/) službou Azure způsobem, který podporuje [aktivní software development](https://en.wikipedia.org/wiki/Agile_software_development). Předpokládá, že už znáte nasazení [složitých aplikací předvídatelností v Azure](app-service-deploy-complex-application-predictably.md).

Omezení technických postupů můžete často bránit úspěšné provádění aktivní metody. Azure aplikaci služby pomocí funkce, jako jsou [Nepřetržitý publikování](app-service-continuous-deployment.md) [pracovní prostředí](web-sites-staged-publishing.md) (slotů) a [Sledování](web-sites-monitor.md), když uvážlivě doplněný průběhu a Správa nasazení ve [Správce prostředků Azure](../azure-resource-manager/resource-group-overview.md), může být součástí skvělé řešení pro vývojáře, kteří podpořit aktivní software development.

V následující tabulce je chcete použít krátký seznam požadavků na související s aktivní vývoj a jak služby Azure umožňují každý z nich.

| Požadavek | Jak povolí Azure |
|---------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| – Vytvoření s každé potvrzení<br>– Vytvoření automaticky a rychlé | Když nakonfigurována nepřetržitý nasazení, aplikaci služby Azure fungovat jako live spuštění buildy podle pobočky odchylka. Pokaždé, když se posune kód pobočky, je automaticky vytvořené a spuštění živé v Azure.|
| – Zkontrolujte sestavení vlastního testování | Načtení testů, webových testů, atd., může být nasazené šablonou správce prostředků Azure.|
| -Provádění testů v klonovat provozním prostředí | Azure šablony správce prostředků mohou sloužit k vytvoření klony Azure provozním prostředí (včetně nastavení aplikace připojení řetězec šablony, měřítko, atd.) pro účely testování rychle a předvídatelností.|
| – Slouží k zobrazení výsledků posledním buildu snadno | Nepřetržitý nasazení Azure z úložiště znamená, že můžete otestovat nový kód aplikace live hned po potvrdit změny. |
| -Potvrdit hlavní větev každý den<br>-Automatizovat nasazení | Nepřetržitý integrace praxi s hlavním větev v úložišti automaticky nasadí každé Potvrdit/hromadné korespondence na hlavní větev výroby. |

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="what-you-will-do"></a>Co bude dělat ##

Vás provede pracovního postupu typické vývojáře zkušební fáze výroby za účelem publikování nové změny s aplikací [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) vzorku, který se skládá ze dvou [webových aplikací web apps](/services/app-service/web/), jednu frontend (FE) a druhá je back-end rozhraní API webových (BE) a [databáze SQL](/services/sql-database/). Budou fungovat s architektura nasazení ukázáno v následujícím příkladu:

![](./media/app-service-agile-software-development/what-1-architecture.png)

Vložit obrázek do slova:

-   Architektura nasazení je rozdělit do tří různých prostředích (nebo [skupiny zdrojů](../azure-resource-manager/resource-group-overview.md) v Azure), oba objekty mají vlastní [plán služeb aplikací](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) [měřítka](web-sites-scale.md) nastavení a databáze SQL. 
-   Každé prostředí dá se ovládat samostatně. Existují můžete dokonce v různých předplatných.
-   Pracovní a výrobních jsou implementovaná jako dva sloty stejné aplikace aplikaci služby. Hlavní pobočky je nastaven pro nepřetržitý integrace se službou pracovní pozici.
-   Po ověření potvrdit předlohy větve na pracovní pozici (s daty výrobní) aplikaci ověřenou pracovní vyměnit do zobrazení výrobní úsek [s žádné výpadek služeb](web-sites-staged-publishing.md).

Výrobní a pracovní prostředí je definován šablonu v [ * &lt;repository_root >*/ARMTemplates/ProdandStage.json](https://github.com/azure-appservice-samples/ToDoApp/blob/master/ARMTemplates/ProdAndStage.json).

Odchylka a testovací prostředí jsou definovány šablonu v [ * &lt;repository_root >*/ARMTemplates/Dev.json](https://github.com/azure-appservice-samples/ToDoApp/blob/master/ARMTemplates/Dev.json).

Typické větvení strategie, budou používat taky s kódem přesouvající se z větví vývojáře až větvi test pak k předlohy větví (přesunutí nahoru kvality, tak na speak).

![](./media/app-service-agile-software-development/what-2-branches.png) 

## <a name="what-you-will-need"></a>Co budete potřebovat ##

-   Účet Azure
-   Účet [GitHub](https://github.com/)
-   Libovolná prostředí (součástí [GitHub pro Windows](https://windows.github.com/)) – umožňuje spuštění libovolná a prostředí PowerShell příkazů ve stejné relaci 
-   Nejnovější bitů [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/download/0.9.4-June2015/azure-powershell.0.9.4.msi)
-   Základní principy z následujících akcí:
    -   Nasazení šablony [Správce prostředků Azure](../azure-resource-manager/resource-group-overview.md) (viz také [nasadit do složitých aplikací pro předvídatelností v Azure](app-service-deploy-complex-application-predictably.md))
    -   [Libovolná](http://git-scm.com/documentation)
    -   [Prostředí PowerShell](https://technet.microsoft.com/library/bb978526.aspx)

> [AZURE.NOTE] Musíte mít účet Azure tento kurz:
> + Můžete [Otevřít účet Azure zdarma](/pricing/free-trial/) – načtení přeplatky můžete vyzkoušet placené služby Azure a i když jste čerpány uchováváte účet a použití uvolnit Azure služby, jako je Web Apps.
> + Je možné [Aktivovat výhod odběratele Visual Studio](/pricing/member-offers/msdn-benefits-details/) : vaše aplikace Visual Studio předplatné vám přeplatky každý měsíc využívající služby placené Azure.
>
> Pokud chcete začít pracovat s aplikaci služby Azure před registrací účet Azure, přejděte na [Zkuste aplikaci služby](http://go.microsoft.com/fwlink/?LinkId=523751), které můžete okamžitě vytvořit web appu krátkodobý starter v aplikaci služby. Žádné povinné; kreditní karty žádné závazky.

## <a name="set-up-your-production-environment"></a>Nastavení provozním prostředí ##

>[AZURE.NOTE] Skript použitý v tomto kurzu automaticky nakonfiguruje nepřetržitý publikování z úložiště GitHub. Tento postup vyžaduje, aby vaše přihlašovací údaje GitHub jsou už uložené v Azure, jinak skriptů nasazení se nezdaří, když se pokusíte konfigurace nastavení správy zdrojů pro webové aplikace. 
>
>Pro ukládání přihlašovacích údajů GitHub v Azure, vytvořte webovou aplikaci [Portálu Azure](https://portal.azure.com/) a [Konfigurace GitHub nasazení](app-service-continuous-deployment.md). Stačí udělat jednou. 

Ve scénáři typické DevOps aplikace, na kterém běží žijí Azure a chcete ho měnit prostřednictvím nepřetržitý publikování. V tomto scénáři máte šablonu, která vyvinuté, testováno a slouží k nasazení provozním prostředí. Chcete nastavit ho v této části.

1.  Vytvoření vlastního vidlice [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) úložiště. Informace týkající se vytváření vašeho vidlice najdete v tématu [vidlice Repo](https://help.github.com/articles/fork-a-repo/). Po vytvoření vaší vidlice uvidíte v prohlížeči.
 
    ![](./media/app-service-agile-software-development/production-1-private-repo.png)

2.  Otevřete relaci libovolná prostředí. Pokud ještě nemáte libovolná prostředí, nainstalujte [GitHub pro Windows](https://windows.github.com/) .

3.  Vytvoření místní klonovat vaší vidlice spuštěním následujícího příkazu:

        git clone https://github.com/<your_fork>/ToDoApp.git 

4.  Až budete mít místní klonovat, přejděte na * &lt;repository_root >*\ARMTemplates a spustit deploy.ps1 skriptovat následujícím způsobem:

        .\deploy.ps1 –RepoUrl https://github.com/<your_fork>/todoapp.git

4.  Po zobrazení výzvy zadejte požadované uživatelské jméno a heslo pro přístup k databázi.

    Měli byste vidět zřizovací průběh různých Azure zdroje. Po dokončení nasazení skript spuštění aplikace v prohlížeči a umožňují popisný ZvukovýSignál.

    ![](./media/app-service-agile-software-development/production-2-app-in-browser.png)
 
    >[AZURE.TIP] Podívejte se na * &lt;repository_root >*\ARMTemplates\Deploy.ps1 zobrazíte, jak vytváří zdroje s jedinečné ID. Stejný postup slouží k vytvoření klony stejného nasazení bez obav konfliktních názvů zdrojů.
 
6.  Zpátky v prostředí libovolná relace, spusťte tento příkaz:

        .\swap –Name ToDoApp<unique_string>master

    ![](./media/app-service-agile-software-development/production-4-swap.png)

7.  Po dokončení skript, vraťte se do přejděte na adresu frontend (http://ToDoApp*&lt;unique_string >*master.azurewebsites.net/) zobrazíte aplikace spuštěné v výroby.
 
5.  Přihlaste se k [Portálu Azure](https://portal.azure.com/) a přečtěte si téma co je vytvořen.

    Mají být k dispozici dva webových aplikací web apps ve stejné skupině prostředků, jedna z nich `Api` přípona názvu. Když se podíváte na skupiny zobrazení zdrojů, zobrazí se také databáze SQL a server, plán služeb aplikací a pracovní sloty pro webové aplikace. Projděte si různých zdrojů a porovnejte je s * &lt;repository_root >*\ARMTemplates\ProdAndStage.json zobrazíte jejich konfigurace v šabloně.

    ![](./media/app-service-agile-software-development/production-3-resource-group-view.png)

Teď jste nastavili provozním prostředí. Dále budou zahájit novou aktualizaci aplikace.

## <a name="create-dev-and-test-branches"></a>Vytvoření vývojáře a otestujte poboček ##

Teď, když máte složité aplikace spuštěné v výrobní v Azure, bude provedete aktualizaci aplikace v souladu s aktivní metodologie. V této části vytvoříte vývoj a otestujte větve, které budete potřebovat požadované aktualizace.

1.  Nejdřív vytvořte testovacím prostředí. V prostředí libovolná relaci, spusťte následující příkazy k vytvoření prostředí pro novou větev s názvem **NewUpdate**. 

        git checkout -b NewUpdate
        git push origin NewUpdate 
        .\deploy.ps1 -TemplateFile .\Dev.json -RepoUrl https://github.com/<your_fork>/ToDoApp.git -Branch NewUpdate

1.  Po zobrazení výzvy zadejte požadované uživatelské jméno a heslo pro přístup k databázi. 

    Po dokončení nasazení skript spuštění aplikace v prohlížeči a umožňují popisný ZvukovýSignál. A stejně jako toto Teď máte nové větev vlastní testovacím prostředí. Chvíli provést revizi co je potřeba o tomto testovacím prostředí:

    -   Můžete ji vytvořit v Azure předplatného. To znamená, že provozním prostředí můžete spravovaných odděleně od testovacím prostředí.
    -   Testovacím prostředí se systémem žijí Azure.
    -   Testovacím prostředí je shodný s provozním prostředí, s výjimkou pracovní sloty a měřítka nastavení. Protože jsou pouze rozdíly mezi ProdandStage.json a Dev.json to zjistíte.
    -   Správa testovacím prostředí v jeho vlastní aplikaci služby plánu s jinou cenu osy (například **zdarma**).
    -   Odstranění testovacím prostředí budou skládat například odstranění skupiny zdrojů. Zjistíte, jak můžete udělat toto [později](#delete).

2.  Přejděte vytvořit větev vývojáře spuštěním následující příkazy:

        git checkout -b Dev
        git push origin Dev
        .\deploy.ps1 -TemplateFile .\Dev.json -RepoUrl https://github.com/<your_fork>/ToDoApp.git -Branch Dev

3.  Po zobrazení výzvy zadejte požadované uživatelské jméno a heslo pro přístup k databázi. 

    Chvíli provést revizi co je potřeba o vývojovém prostředí: 

    -   Prostředí pro vývojáře obsahuje konfigurace shodné testovacím prostředí, protože nasazení používat stejnou šablonu.
    -   Každý vývojovém prostředí lze vytvořit v Azure předplatné pro vývojáře vlastní přímo v testovacím prostředí samostatně spravovaných.
    -   Prostředí pro vývojáře běží žijí Azure.
    -   Odstranění vývojovém prostředí je jednoduchá – stačí odstranění skupiny zdrojů. Zjistíte, jak můžete udělat toto [později](#delete).

>[AZURE.NOTE] Pokud máte víc vývojáři práce na nové aktualizace, každý z nich mohli snadno vytvářet větví a vyhrazené vývojovém prostředí následujícím způsobem:
>
>1. Vytvoření vlastní vidlice úložiště v GitHub (viz [větev Repo](https://help.github.com/articles/fork-a-repo/)).
>2. Klonovat vidlice v místním počítači
>3. Spusťte stejné příkazy k vytvoření vlastní vývojáře pobočky a prostředí.

Až budete hotovi, GitHub vidlice má tři pobočky:

![](./media/app-service-agile-software-development/test-1-github-view.png)

Vrácený byste si měli šest webových aplikací web apps (tři sady dvě) v tři samostatné skupiny prostředků

![](./media/app-service-agile-software-development/test-2-all-webapps.png)
 
>[AZURE.NOTE] Všimněte si, že ProdandStage.json určuje provozním prostředí používat **Standardní** ceny osy, který je vhodný pro škálovatelnost aplikace výroby.

## <a name="build-and-test-every-commit"></a>Vytvořte a otestujte každé potvrzení ##

Soubory šablon ProdAndStage.json a Dev.json už zadejte parametry Zdroj ovládacího prvku, který se ve výchozím nastavení nastaví nepřetržitý publikování pro web app. Proto každé potvrdit větvi GitHub spustí automatické nasazení na Azure z této větve. Zjistěme, jak funguje nastavení nyní.

1.  Ujistěte se, že se účastníte větvi vývojáře místní úložiště. K tomuto účelu v prostředí libovolná spusťte tento příkaz:

        git checkout Dev

2.  Provádění změn na jednoduché vrstvě uživatelského rozhraní aplikace na změnou kód použít [zavádění](http://getbootstrap.com/components/) seznamy. Otevřít * &lt;repository_root >*\src\MultiChannelToDo.Web\index.cshtml a aby se zvýrazněnou změna níže:

    ![](./media/app-service-agile-software-development/commit-1-changes.png)

    >[AZURE.NOTE] Pokud nemůže přečíst obrázek nad textem: 
    >
    >- V řádku 18 změnit `check-list` k `list-group`.
    >- V řádku 19, změňte `class="check-list-item"` k `class="list-group-item"`.

3.  Uložte změny. Zpátky v prostředí libovolná, spusťte následující příkazy:

        cd <repository_root>
        git add .
        git commit -m "changed to bootstrap style"
        git push origin Dev
 
    Tyto příkazy libovolná se podobají "změnami v kódu" jiné systému správy zdrojů jako TFS. Když spustíte `git push`, nové potvrdit spustí automatické kód nabízených na Azure, které pak znovu sestaví aplikace odrážet změny ve vývojovém prostředí.

4.  Ověřte, že došlo k tento kód nabízených prostředí vývojáře, přejděte na zásuvné vývojovém prostředí webové aplikace a podívejte se na část **nasazení** . Je třeba neuvidíte tam nejnovější zprávy potvrzení.

    ![](./media/app-service-agile-software-development/commit-2-deployed.png)

5.  Z tohoto umístění klikněte na **Procházet** zobrazíte nové změny v aplikaci živou v Azure.

    ![](./media/app-service-agile-software-development/commit-3-webapp-in-browser.png)

    Toto je poměrně menší změnu aplikace. Má však mnoho dobu nové změny ke složité webovou aplikaci před nechtěným a nežádoucí vedlejší účinky. Možnost snadno otestovat každé potvrdit v živé sestavení umožňují ještě před vaši zákazníci neuvidíte vznikem zabránit těchto problémů.

Nyní by se orientovali s uskutečňování, že vývojář na **NewUpdate** projektu, bude možné snadno vytvořit prostředí vývojářů pro sebe, a pak vytvořit každé potvrdit a otestovat každé Tvůrce dotazů.

## <a name="merge-code-into-test-environment"></a>Sloučení kódu v testovacím prostředí ##

Až budete chtít nabízená kódu z vývojáře větve až NewUpdate větev, je obrázku standardních libovolná:

1.  Všechny nové potvrzení NewUpdate sloučíte větvi vývojáře v GitHub, například potvrzení vytvořené jinými vývojáři. Nové potvrdit na GitHub aktivace nabízených kód a vytvořte ve vývojovém prostředí. Můžete pak zkontrolujte, že váš kód pobočky vývojáře pořád spolupracuje nejnovější bitů z NewUpdate větve.

2.  Všechny nové potvrzení od vývojáře větev sloučíte NewUpdate větví na GitHub. Tato akce spustí nabízených kód a Tvůrce dotazů v testovacím prostředí. 

Poznámka: znovu, a proto nepřetržitý nasazení je už nastavení se větví libovolná není potřeba provést další akci například pro provoz integrace vytvoří. Potřebujete pouze provést standardní zdroj ovládacího prvku postupy pomocí libovolná a Azure provede všech procesů sestavení za vás.

Teď Pojďme nabízená kódu pobočky **NewUpdate** . V prostředí libovolná spusťte následující příkazy:

    git checkout NewUpdate
    git pull origin NewUpdate
    git merge Dev
    git push origin NewUpdate

Je to! 

Přejděte na zásuvné web app pro testovacím prostředí zobrazíte nové potvrzení (sloučí do NewUpdate větve), teď posune testovacím prostředí. Potom klikněte na **Procházet** a zkontrolovat, že změna stylu teď spuštěním žijí Azure.

## <a name="deploy-update-to-production"></a>Nasazení aktualizaci výroby ##

Vložení kódu pro pracovní a na provozním prostředí by měli mít pocit žádné jiné než co už máte už hotové při stisknutí kód na testovacím prostředí. Je ale skutečně tak jednoduché. 

V prostředí libovolná spusťte následující příkazy:

    git checkout master
    git pull origin master
    git merge NewUpdate
    git push origin master

Nezapomeňte založený na způsobu, jakým pracovní a na provozním prostředí je nastavená v ProdandStage.json, nový kód se posune úsek **přípravu** a běží tam. Takže pokud přejdete na adresu URL pracovní pozici, zobrazí se nový kód spuštěný není. K tomuto účelu spustit `Show-AzureWebsite` rutiny prostředí libovolná.

    Show-AzureWebsite -Name ToDoApp<unique_string>master -Slot Staging
 
Teď, po ověření aktualizace v pracovní pozici, jediné zbývá se a připojte k němu do provozu. V prostředí libovolná spusťte následující příkazy:

    cd <repository_root>\ARMTemplates
    .\swap.ps1 -Name ToDoApp<unique_string>master

Blahopřejeme! Nová aktualizace jste publikovali úspěšně do webové aplikace výroby. Je týmovými stačí udělat i ho snadno vytvořením vývojáře a otestujte prostředí, vytváření a testování každé potvrzení. Toto jsou klíčové stavební bloky aktivní software development.

<a name="delete"></a>
## <a name="delete-dev-and-test-enviroments"></a>Odstranění vývojáře a otestujte enviroments ##

Vzhledem k tomu, že jste záměrně navržen prostředí vývojáře a otestujte být skupiny samostatné zdrojů, je velmi snadné je můžete odstranit. Chcete-li odstranit ty, které jste vytvořili v tomto kurzu GitHub pobočky a Azure artefakty, jednoduše spusťte následující příkazy v prostředí libovolná:

    git branch -d Dev
    git push origin :Dev
    git branch -d NewUpdate
    git push origin :NewUpdate
    Remove-AzureRmResourceGroup -Name ToDoApp<unique_string>dev-group -Force -Verbose
    Remove-AzureRmResourceGroup -Name ToDoApp<unique_string>newupdate-group -Force -Verbose

## <a name="summary"></a>Souhrn ##

Aktivní software development je musí mají pro mnoho společností, kteří chtějí přijmout Azure jako jejich platformy aplikace. V tomto kurzu jste se naučili, jak vytvořit a oddělení dolů přesné repliky obrázek nebo poblíž kopie provozním prostředí snadno to i v případě složitých aplikací. Můžete taky naučili využít tuto možnost vytvořit vývojový proces, který můžete vytvořit a otestujte každého jednoho potvrdit v Azure. Tento kurz zpravidla uvádí, jak nejlépe můžete aplikaci služby Azure a správce prostředků Azure společně vytvořit DevOps řešení, které caters aktivní metody. Dál je možné vytvářet v tomto scénáři provedením pokročilé techniky DevOps například [testování výroby](app-service-web-test-in-production-get-start.md). Běžné situace testování ve výrobním najdete v článku [Flighting nasazení (testování verze beta) v aplikaci služby Azure](app-service-web-test-in-production-controlled-test-flight.md).

## <a name="more-resources"></a>Další materiály ##

-   [Nasazení aplikace složité předvídatelností v Azure](app-service-deploy-complex-application-predictably.md)
-   [Aktivní vývoj ve skutečnosti: tipy a triky pro Modernized vývoj obrázku](http://channel9.msdn.com/Events/Ignite/2015/BRK3707)
-   [Rozšířené nasazování Azure webových aplikací pomocí šablon správce prostředků](http://channel9.msdn.com/Events/Build/2015/2-620)
-   [Vytváření Azure správce prostředků šablony](../resource-group-authoring-templates.md)
-   [JSONLint - JSON ověřování](http://jsonlint.com/)
-   [ARMClient – nastavení publikování GitHub na webu](https://github.com/projectKudu/ARMClient/wiki/Setup-GitHub-publishing-to-Site)
-   [Libovolná větvení – základní větvení a sloučení](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
-   [David Ebbo blogu](http://blog.davidebbo.com/)
-   [Azure Powershellu](../powershell-install-configure.md)
-   [Azure nástroje příkazového řádku různé platformy](../xplat-cli-install.md)
-   [Vytváření nebo úpravy uživatelů v Azure AD](https://msdn.microsoft.com/library/azure/hh967632.aspx#BKMK_1)
-   [Projekt Kudu wikiwebu](https://github.com/projectkudu/kudu/wiki)
