<properties
    pageTitle="Nepřetržitý nasazení služby Azure aplikace | Microsoft Azure"
    description="Zjistěte, jak povolit nepřetržitý nasazení služby Azure aplikace."
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
    ms.date="10/28/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="continuous-deployment-to-azure-app-service"></a>Nepřetržitý nasazení služby Azure aplikace

Tento kurz se dozvíte, jak nakonfigurovat nepřetržitý nasazení pracovního postupu pro aplikaci [Služby Azure aplikace] . Aplikace služby integrace s BitBucket, GitHub a Visual Studio týmu služeb (VSTS) umožňuje pracovního postupu nepřetržitý nasazení kde Azure použije v nejnovější aktualizace z vašeho projektu publikované na jednu z těchto služeb. Nepřetržitý nasazení je Skvělá volba pro projekty kde více a probíhá integrace časté příspěvky.

## <a name="overview"></a>Povolit nepřetržitý nasazení

Chcete-li povolit nepřetržitý nasazení 

1. Publikujte obsah aplikace do úložiště, který bude sloužit k nepřetržitý nasazení.  
    Další informace o publikování projektu do těchto služeb najdete v článku [Vytvoření repo (GitHub)] [vytvořit repo (BitBucket)]a [začít pracovat s VSTS].

2. V nabídce zásuvné vaše aplikace [Azure portál], klikněte na **nasazení aplikace > Možnosti nasazení**. Klikněte na **Zvolit zdroj**a potom vyberte zdroj výsledků nasazení.  

    ![](./media/app-service-continuous-deployment/cd_options.png)
    
    > [AZURE.NOTE] Konfigurace VSTS účtu v aplikaci služby nasazení najdete v tématu [kurz](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).
    
3. Dokončení pracovního postupu se tak mohli ověřovat. 

4. V zásuvné **Zdroj zavedení** vyberte požadovaný projekt a větev nasazení z. Až budete hotovi, klikněte na **OK**.
  
    ![](./media/app-service-continuous-deployment/github_option.png)

    > [AZURE.NOTE] Při aktivaci nepřetržitý nasazení se GitHub nebo BitBucket, zobrazí se veřejných a privátních projekty.

    Aplikace služby vytvoří přidružení s vybranou úložiště, použije v seznamu soubory ze zadané větví a udržuje klonovat úložiště aplikace aplikaci služby. Při konfiguraci VSTS nepřetržitý nasazení z portálu Microsoft Azure integrace používá aplikaci služby [Kudu nasazení engine](https://github.com/projectkudu/kudu/wiki), který už automaticky vytvořit a nasazení úkoly s každé `git push`. Nemusíte samostatně nastavení nepřetržitý nasazení VSTS. Po dokončení tohoto procesu zásuvné **Možnosti nasazení** aplikace zobrazí, aktivní nasazení, které označuje nasazení proběhla úspěšně.

5. Ověření, že aplikace úspěšně nasazení, klikněte na adresu **URL** v horní části aplikace na zásuvné Azure portálu. 

6. Potvrďte probíhající nepřetržitý nasazení z úložiště podle svého výběru nabízená změnu úložiště. Aplikace by měl aktualizuje podle změny brzy po dokončení nabízených úložiště. Můžete ověřit, že má přetáhne aktualizovat do ve zásuvné **Možnosti nasazení** aplikace.

## <a name="VSsolution"></a>Nepřetržitý nasazení řešení Visual Studio 

Předání řešení Visual Studia do služby Azure aplikace je právě stejně snadná jako vložení souboru jednoduché index.html. Procesu nasazení aplikace služby zjednodušuje všechny podrobnosti, včetně obnovování NuGet závislostí a vytváření binární soubory aplikace. Ovládací prvek zdroje osvědčenými postupy zachování doručení s kódem pouze v úložišti libovolná a nechat nasazení aplikace služby starat o ostatních.

Kroky pro předání vašeho řešení Visual Studia do aplikace služby jsou stejné jako v [předchozí části](#overview), za předpokladu, že takto konfigurace řešení a úložiště:

-   Umožňuje generovat možnost Visual Studio zdroj ovládacího prvku `.gitignore` souboru jako obrázek pod nebo ručně přidat `.gitignore` souboru v kořenu úložiště s obsahem podobně jako tento [Ukázkový .gitignore](https://github.com/github/gitignore/blob/master/VisualStudio.gitignore). 

    ![](./media/app-service-continuous-deployment/VS_source_control.png)
 
-   Přidáte celé řešení adresáře strom do úložiště, se souborem SLN v kořenové úložiště.

Po nastavení úložiště jak je popsáno a konfigurace aplikace v Azure pro nepřetržitý publikování z jednoho z online úložišť libovolná, můžete vyvíjet aplikace ASP.NET místně ve Visual Studiu a nepřetržitě jednoduše stisknutím změny do online úložiště libovolná nasazení kódu.

## <a name="disableCD"></a>Zakažte průběžných nasazení

Jak zakázat souvislé nasazení 

1. V nabídce zásuvné vaše aplikace [Azure portál], klikněte na **nasazení aplikace > Možnosti nasazení**. V **možnostech** zásuvné klepněte na **Odpojit** .

    ![](./media/app-service-continuous-deployment/cd_disconnect.png)    

2. Po odpověď **Ano** do žádosti o potvrzení, se vraťte do svého aplikace zásuvné a klikněte na **nasazení aplikace > Možnosti nasazení** Pokud chcete nastavit publikování z jiného zdroje.

## <a name="additional-resources"></a>Další zdroje informací

* [Jak prozkoumejte běžné problémy s nepřetržitý nasazení](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [Jak pomocí Powershellu pro Azure]
* [Jak používat nástroje Azure příkazového řádku pro Mac a Linux]
* [Libovolná si přečtěte následující dokumentaci]
* [Kudu projektu](https://github.com/projectkudu/kudu/wiki)

>[AZURE.NOTE] Pokud chcete začít pracovat s aplikaci služby Azure před registrací účet Azure, přejděte na [Zkuste aplikaci služby](http://go.microsoft.com/fwlink/?LinkId=523751), které můžete okamžitě vytvořit web appu krátkodobý starter v aplikaci služby. Žádné povinné; kreditní karty žádné závazky.

[Azure aplikace služby]: https://azure.microsoft.com/en-us/documentation/articles/app-service-changes-existing-services/ 
[Azure portálu]: https://portal.azure.com
[VSTS Portal]: https://www.visualstudio.com/en-us/products/visual-studio-team-services-vs.aspx
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Jak pomocí Powershellu pro Azure]: ../articles/powershell-install-configure.md
[Jak používat nástroje Azure příkazového řádku pro Mac a Linux]: ../articles/xplat-cli-install.md
[Libovolná si přečtěte následující dokumentaci]: http://git-scm.com/documentation

[Vytvoření repo (GitHub)]: https://help.github.com/articles/create-a-repo
[Vytvoření repo (BitBucket)]: https://confluence.atlassian.com/display/BITBUCKET/Create+an+Account+and+a+Git+Repo
[Začínáme s VSTS]: https://www.visualstudio.com/get-started/overview-of-get-started-tasks-vs
[Continuous delivery to Azure using Visual Studio Team Services]: ../articles/cloud-services/cloud-services-continuous-delivery-use-vso.md
