<properties
    pageTitle="Nepřetržitý doručování s libovolná a Visual Studio týmovou v Azure | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat týmové projekty Visual Studio týmovou použít libovolná automaticky sestavovat a nasazovat funkcí v prohlížeči v aplikaci služby Azure nebo cloud services."
    services="cloud-services"
    documentationCenter=".net"
    authors="mlearned"
    manager="douge"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="mlearned"/>

# <a name="continuous-delivery-to-azure-using-visual-studio-team-services-and-git"></a>Nepřetržitý doručení Azure pomocí aplikace Visual Studio týmovou a libovolná

Projekty Visual Studio týmovou týmový můžete hostovat úložišti libovolná zdrojového kódu a automaticky vytvořit a nasadit na Azure webových aplikací nebo cloudových služeb pokaždé, když nabízená potvrzení úložiště.

Budete potřebovat Visual Studio 2013 a SDK Azure nainstalovaný. Pokud ještě nemáte Visual Studio 2013, stáhněte si ho výběrem **Začínáme zdarma** odkaz na [www.visualstudio.com](http://www.visualstudio.com). Nainstalujte Azure SDK z [tady](http://go.microsoft.com/fwlink/?LinkId=239540).


> [AZURE.NOTE]Musíte mít účet Visual Studio týmovou tento kurz: můžete [zdarma si potřebujete založit účet služby týmu Visual Studio](http://go.microsoft.com/fwlink/p/?LinkId=512979).

Pokud chcete nastavit automaticky sestavovat a nasazovat Azure pomocí aplikace Visual Studio týmovou do cloudové služby, postupujte takto.

## <a name="1-create-a-git-repository"></a>1: vytvoření libovolná úložiště

1. Pokud ještě nemáte účet aplikace Visual Studio týmovou, můžete ho získat [tady](http://go.microsoft.com/fwlink/?LinkId=397665). Při vytváření týmu projektu, klikněte na libovolná jako zdrojového ovládacího prvku systému. Postupujte podle pokynů pro připojení aplikace Visual Studio k týmovému projektu.

2. V **Týmu Průzkumníka**zvolte odkaz **klonovat tohoto úložiště** .

    ![][3]

3. Zadejte umístění místní kopii a potom klikněte na tlačítko **vytvořit kopii** .

## <a name="2-create-a-project-and-commit-it-to-the-repository"></a>2: vytvoření projektu a potvrďte do úložiště

1. V programu **Průzkumník týmů**, v části **řešení** zvolte **Nový** odkaz pro vytvoření nového projektu v místním úložišti.

    ![][4]

2. Podle pokynů uvedených v tomto návodu nástroje můžete nasazovat do webových aplikací nebo do cloudové služby (Azure aplikace). Vytvoření nového projektu Azure cloudové služby, nebo do nového projektu ASP.NET MVC. Nastavení, ujistěte se, že projekt zaměřuje .NET Framework 4 nebo novější. Pokud vytváříte projekt cloudové služby, přidejte role web ASP.NET MVC a roli pracovní.
Pokud chcete vytvořit web appu, zvolte šablonu projektu **ASP.NET webové aplikace** a zvolte **MVC**. Další informace najdete v tématu [vytvořit webovou aplikaci ASP.NET v aplikaci služby Azure](../app-service-web/web-sites-dotnet-get-started.md) .

3. Otevření místní nabídky pro řešení a zvolte **potvrzení**.

    ![][7]

4. Pokud jste použili libovolná ve Visual Studiu týmovou poprvé, musíte zadat některé informace Identifikujte v libovolná. V oblasti **Neuložené změny** **Průzkumník týmů**zadejte uživatelské jméno a e-mailovou adresu. Zadejte poznámku o potvrzení a potom klikněte na tlačítko **Potvrdit** .

    ![][8]

5. Poznámka: možnost zahrnout nebo vyloučit konkrétní změny při vrátit se změnami. Pokud jsou změny, které chcete vyloučit, vyberte **Zahrnout všechny**.

6. Teď jste potvrzeného změny v místní kopii úložiště. Výběrem odkazu **synchronizace** dál synchronizujte tyto změny se serverem.

## <a name="3-connect-the-project-to-azure"></a>3: připojení k Azure projektu

1. Teď, když máte libovolná úložiště ve Visual Studiu týmovou s některé zdrojového kódu v něm budete chtít připojit libovolná úložiště Azure.  [Azure klasické portál](http://go.microsoft.com/fwlink/?LinkID=213885)vyberte cloudové služby nebo webové aplikace nebo vytvořte novou výběrem ikonu + v levém dolním rohu a zvolte ** **Cloudové služby** nebo Web App** a potom **Vytvořit**.

    ![][9]

3. Cloudové služby zvolte odkaz **Nastavení publikování pomocí aplikace Visual Studio týmovou** . Web apps zvolte odkaz **nastavení nasazení z ovládacího prvku zdroje** .

    ![][10]

2. V Průvodci do textového pole zadejte název účtu Visual Studio týmovou a zvolte odkaz **Povolit** . Můžete být požádáni o přihlášení.

    ![][11]

3. V **Žádosti o připojení** místní dialogu zvolením možnosti **přijmout** povolte Azure konfigurace projektu týmu ve Visual Studiu týmovou.

    ![][12]

4. Po úspěšném ověření se zobrazí rozevírací seznam, který obsahuje týmové projekty Visual Studio týmovou.  Vyberte název týmového projektu, který jste vytvořili v předchozích krocích a zvolte tlačítko se zaškrtnutím v průvodci.

    ![][13]

    Při příštím nabízená potvrzení do úložiště, Visual Studio týmovou bude vytvořte a nasaďte projekt Azure.

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a>4: Aktivace nové vytvoření a přeinstalujte projektu

1. Ve Visual Studiu otevřete soubor a ho změnit. Například změnit soubor `_Layout.cshtml` v části zobrazení\\sdílené složky v roli MVC web.

    ![][17]

2. Upravte text zápatí pro daný web a soubor uložte.

    ![][18]

3. V **Okně Průzkumník**otevřete místní nabídky pro uzel řešení, uzel projektu nebo soubor, který jste změnili a klikněte na **Potvrdit**.

4. Napište komentář a zvolte **potvrzení**.

    ![][20]

5. Zvolte odkaz **synchronizace** .

    ![][38]

6. Zvolte odkaz **nabízených** posunout vaše potvrdit do úložiště v aplikaci Visual Studio týmovou. (Také můžete na tlačítko **synchronizovat** zkopírujte vaše potvrzení úložiště. Rozdíl je, že **synchronizovat** taky použije posledním změnám v úložišti.)

    ![][39]

7. Zvolte tlačítko **Domů** se vraťte do domovské stránky **Týmového Explorer** .

    ![][21]

8. Zvolte **vytvoří** zobrazíte buildy probíhá.

    ![][22]

    **Tým Explorer** zobrazuje, že byl spuštěn sestavení pro vaše vrácení se změnami.

    ![][23]

9. Podrobný protokol v průběhu Tvůrce dotazů zobrazíte poklikejte na název sestavení v průběhu.

10. Při sestavování v průběhu, podívejte se na definici Tvůrce dotazů, které byly nastavené při použití průvodce k propojení s Azure.  Otevření místní nabídky pro definici Tvůrce dotazů a zvolte **Upravit definici vytvořit**.

    ![][25]

11. Na kartě **aktivační událost** zobrazí se, že definice sestavení vytvářet na každé vrácení se změnami, ve výchozím nastavení je. (Do cloudové služby Visual Studio týmovou vytvoří a nasadí větvi předlohy na testovacím prostředí automaticky. Máte pořád ještě ruční krok pro nasazení na online Web. Pro web app, která neobsahuje pracovní prostředí nasadí větvi předlohy přímo na webu v režimu online.

    ![][26]

1. Na kartě **proces** uvidíte, že prostředí nasazení je nastavený na název svojí cloudové služby nebo webové aplikace.

    ![][27]

1. Hodnoty vlastností v případě potřeby nastavit odlišné hodnoty jsou výchozí hodnoty. Vlastnosti pro Azure publikování jsou v části **nasazení** a může být potřeba nastavit MSBuild parametry. V obláčkem projekt služby, chcete-li zadat konfiguraci služby než "Cloudu", nastavte parametry MSbuild na `/p:TargetProfile=[YourProfile]` kde soubor konfigurace služby s názvem třeba ServiceConfiguration odpovídá *[YourProfile]* . *YourProfile*.cscfg.

    Následující tabulka zobrazuje dostupné vlastnosti v části **nasazení** :

  	|Vlastnost|Výchozí hodnota|
  	|---|---|
  	|Povolit nedůvěryhodných certifikátů|Pokud je hodnota NEPRAVDA, musíte být přihlášení certifikáty SSL tak, že kořenová autorita.|
  	|Povolit Upgrade|Umožňuje nasazení k aktualizaci stávajícího nasazení místo abyste vytvářeli novou. Zachová IP adresu.|
  	|Neodstraňujte|Pokud je hodnota true, nedojde k přepsání stávajících nesouvisejících nasazení (upgrade je povoleno).|
  	|Cesta k nastavení nasazení|Cesta k souboru .pubxml pro web app, vzhledem ke kořenové složce repo. U cloudovým službám ignorovány.|
  	|Prostředí nasazení Sharepointu|Stejný jako název služby.|
  	|Prostředí Azure nasazení|Web app nebo cloudového název služby.|

1. V současné době vaše sestavení by měl být byla úspěšně dokončena.

    ![][28]

1. Pokud poklikejte na název Tvůrce dotazů aplikace Visual Studio zobrazuje **Vytvořit souhrn**, včetně žádné výsledky testů z přidružené Jednotková testovací projektů.

    ![][29]

1. [Azure klasické portál](http://go.microsoft.com/fwlink/?LinkID=213885)zobrazíte přidružené nasazení na kartě **nasazení** je-li vybrán testovacím prostředí.

    ![][30]

1.  Přejděte na adresu URL vašeho webu. Pro web app jenom v portálu zvolte tlačítko **Procházet** . Do cloudové služby vyberte adresu URL v části **Rychlé přehledně uspořádané** stránky **řídicího panelu** , který ukazuje pracovním prostředí.

    Nasazení z nepřetržitý integrace služby cloudu jsou publikovány ve výchozím nastavení pracovním prostředí. Můžete určit nastavením vlastnosti **Alternativní prostředí služby cloudu** **výroby**. Tady je místo, kam adresu URL webu na stránku řídicího panelu cloudové služby.

    ![][31]

    Na nové záložce prohlížeče se otevře zobrazíte pracovního webu.

    ![][32]

1.  Pokud uděláte jiné změny na projekt, můžete aktivační událost více vytvoří a shromáždí více nasazení. Nejnovější je označen jako aktivní.

    ![][33]

## <a name="5-redeploy-an-earlier-build"></a>5: Přeinstalujte starší sestavení

Tento krok je nepovinný krok. Na portálu Azure klasické vyberte starší nasazení a zvolte **přeinstalujte** převinout svůj web a starší vrácení se změnami zpět na. Poznámka: Toto aktivace nové sestavení v TFS a vytvoření nové položky do historie nasazení.

![][34]

## <a name="6-change-the-production-deployment"></a>6: Změna provozní nasazení

Jakmile budete připraveni, můžete převést pracovního prostředí provozním prostředí výběrem **Zaměnit** Azure klasické portálu. Nově nasazeném pracovního prostředí je stát výrobní a předchozí provozním prostředí, případně změní pracovním prostředí. Aktivní nasazení můžou být odlišná na produkční a pracovní prostředí, ale nasazení historii nedávných sestavení je stejná bez ohledu na prostředí.

![][35]

## <a name="7-deploy-from-a-working-branch"></a>7: nasazení z pracovní větve.

Pokud používáte libovolná, obvykle provedete změny větve pracovní a integrovat do předlohy pobočky váš vývojový dosažení konečném stavu. Ve fázi vývoj projektu je vhodné sestavovat a nasazovat větvi pracovní Azure.

1. V **Týmu Explorer**klikněte na tlačítko **Domů** a pak zvolte tlačítko **poboček** .

    ![][40]

2. Zvolte **Nová pobočka** odkaz.

    ![][41]

3. Zadejte název větvi, například "práce" a zvolte **Vytvořit větev**. Tím vytvoříte novou místní větev.

    ![][42]

4. Publikujte větvi. Vyberte název pobočky **nepublikované**oborů a vyberte **Publikovat**.

    ![][44]

6. Ve výchozím nastavení pouze změny předlohy pobočky aktivace nepřetržitý Tvůrce dotazů. Nastavit nepřetržité vytváření pro pracovní větev, zvolte stránku, která **vytvoří** v **Průzkumníku týmu**a zvolte **Upravit definici vytvořit**.

7. Otevřete kartu **Nastavení zdroje** . Ve skupinovém rámečku **sledované odvětví nepřetržitý integrace a Tvůrce dotazů**zvolte **Kliknutím sem můžete přidat nový řádek**.

    ![][47]

8. Určení větev, kterou jste vytvořili, jako jsou odkazy/hlava/pracovní.

    ![][48]

9. Provádění změn na kód, otevřete místní nabídky pro změněný soubor a pak zvolte **potvrzení**.

    ![][43]

10. Zvolte odkaz **Nesynchronizovaný potvrzení** a zvolte tlačítko **synchronizovat** nebo **nabízená** odkaz zkopírovat změny kopii větvi pracovní ve Visual Studiu Team Services.

    ![][45]

11. Přejděte do zobrazení **vytvoří** a najděte Tvůrce dotazů, který máte jenom spustil pro pracovní větev.

## <a name="next-steps"></a>Další kroky

Další tipy k použití libovolná s Visual Studio týmovou najdete v tématu [vývoje a sdílení kódu v libovolná pomocí aplikace Visual Studio](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) a informace o používání libovolná úložiště, které není spravuje Visual Studio týmovou publikujete Azure najdete v tématu [Nepřetržitý nasazení služby Azure aplikace](../app-service-web/app-service-continuous-deployment.md). Další informace o Visual Studio týmovou najdete v článku [Visual Studio týmovou](http://go.microsoft.com/fwlink/?LinkId=253861).

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateTeamProjectInGit.PNG
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png
[3]: ./media/cloud-services-continuous-delivery-use-vso-git/CloneThisRepository.PNG
[4]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateNewSolutionInClonedRepo.PNG
[7]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitMenuItem.PNG
[8]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[9]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateCloudService.PNG
[10]: ./media/cloud-services-continuous-delivery-use-vso-git/SetUpPublishingWithVSO.PNG
[11]: ./media/cloud-services-continuous-delivery-use-vso-git/AuthorizeConnection.PNG
[12]: ./media/cloud-services-continuous-delivery-use-vso-git/ConnectionRequest.PNG
[13]: ./media/cloud-services-continuous-delivery-use-vso-git/ChooseARepo3.PNG
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso-git/MakeACodeChange.PNG
[20]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[21]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerHome.png
[22]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerBuilds.PNG
[23]: ./media/cloud-services-continuous-delivery-use-vso-git/BuildInQueue.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso-git/ProcessTab.PNG
[28]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateANewAccount.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso-git/PushCurrentBranch.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso-git/BranchesInTeamExplorer.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso-git/NewBranch.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateBranch.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso-git/PublishBranch.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso-git/SourceSettingsPage.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso-git/IncludeWorkingBranch.PNG
