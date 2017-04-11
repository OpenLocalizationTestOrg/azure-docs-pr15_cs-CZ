<properties
    pageTitle="Nepřetržitý doručení služby Visual Studio týmu v Azure | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat týmové projekty Visual Studio týmovou automaticky sestavovat a nasazovat funkcí v prohlížeči v aplikaci služby Azure nebo cloud services."
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

# <a name="continuous-delivery-to-azure-using-visual-studio-team-services"></a>Nepřetržitý doručení Azure služby Visual Studio týmu

Můžete nakonfigurovat týmové projekty Visual Studio týmovou automaticky vytvořit a nasadit na Azure webových aplikací nebo cloudových služeb.  (Informace o tom, jak nastavit nepřetržitý Tvůrce dotazů a nasazení systému pomocí *místního* serveru Team Foundation najdete v tématu [Nepřetržitý doručení služby cloudu v Azure](cloud-services-dotnet-continuous-delivery.md).)

Tento kurz předpokládá, že máte Visual Studio 2013 a SDK Azure nainstalovaný. Pokud ještě nemáte Visual Studio 2013, stáhněte si ho výběrem **Začínáme zdarma** odkaz na [www.visualstudio.com](http://www.visualstudio.com). Nainstalujte Azure SDK z [tady](http://go.microsoft.com/fwlink/?LinkId=239540).

> [AZURE.NOTE]Musíte mít účet Visual Studio týmovou tento kurz: můžete [zdarma si potřebujete založit účet služby týmu Visual Studio](http://go.microsoft.com/fwlink/p/?LinkId=512979).

Pokud chcete nastavit automaticky sestavovat a nasazovat Azure pomocí aplikace Visual Studio týmovou do cloudové služby, postupujte takto.

## <a name="1-create-a-team-project"></a>1: vytvoření projektu týmu

Postupujte podle pokynů [tady](http://go.microsoft.com/fwlink/?LinkId=512980) vytvářet projektové týmu a odkaz na Visual Studio. Tento návod předpokládá, že používáte Team Foundation verze ovládacího prvku (TFVC) jako zdroj ovládacího prvku řešení. Pokud chcete použít libovolná pro správu verzí, přečtěte si článek [libovolná verze tohoto návodu](http://go.microsoft.com/fwlink/p/?LinkId=397358).

## <a name="2-check-in-a-project-to-source-control"></a>2: vrácení se změnami projektu do ovládacího prvku zdroje

1. Ve Visual Studiu řešení, které chcete nasadit otevřete nebo vytvořte nový účet.
Podle pokynů uvedených v tomto návodu nástroje můžete nasazovat do webových aplikací nebo do cloudové služby (Azure aplikace).
Pokud chcete vytvořit nové řešení, vytvořte nový projekt cloudové služby Azure nebo nový projekt ASP.NET MVC. Ujistěte se, že projektu používá .NET Framework 4 nebo 4,5 a pokud vytváříte projekt cloudové služby, přidejte role web ASP.NET MVC a pracovní roli a zvolte Internet aplikace pro web roli. Po zobrazení výzvy vyberte **Internet**.
Pokud chcete vytvořit web appu, zvolte šablonu projektu ASP.NET webové aplikace a zvolte MVC. V tématu [Vytvoření webovou aplikaci ASP.NET v aplikaci služby Azure](../app-service-web/web-sites-dotnet-get-started.md).

    > [AZURE.NOTE] Odvětví nasazení aplikace Visual Studio webové aplikace Visual Studio týmovou podporují pouze v současné době. Projekty webu jsou mimo rozsah.

1. Otevření místní nabídky pro řešení a zvolte **Přidat řešení ovládacího prvku zdroje**.

    ![][5]

1. Přijměte nebo změňte výchozí hodnoty a klikněte na tlačítko **OK** . Po dokončení procesu zdroj ovládacího prvku ikon v **Průzkumníku řešení**.

    ![][6]

1. Otevření místní nabídky pro řešení a zvolte **Vrátit se změnami**.

    ![][7]

1. V oblasti **Neuložené změny** **Průzkumník týmů**zadejte komentář k vrácení se změnami a klikněte na tlačítko **Vrátit se změnami** .

    ![][8]

    Poznámka: možnost zahrnout nebo vyloučit konkrétní změny při vrátit se změnami. Pokud požadované změny vyloučíte, vyberte **Zahrnout všechna** odkaz.

    ![][9]

## <a name="3-connect-the-project-to-azure"></a>3: připojení k Azure projektu

1. Teď, když máte projektu týmu a týmovou se některé zdrojového kódu v něm budete chtít připojit projektu týmu Azure.  [Azure klasické portál](http://go.microsoft.com/fwlink/?LinkID=213885)vyberte cloudové služby nebo webové aplikace nebo vytvořte novou výběrem **+** ikonu v levém dolním rohu a zvolte ** **Cloudové služby** nebo Web App** a potom **Vytvořit**. Zvolte **Nastavení publikování pomocí aplikace Visual Studio týmovou** odkaz.

    ![][10]

1. V průvodci zadejte název účtu Visual Studio týmovou do textového pole a klikněte na odkaz **Nyní ověřit** . Můžete být požádáni o přihlášení.

    ![][11]

1. V **Žádosti o připojení** místní dialogovém okně zvolte tlačítko **přijmout** povolte Azure konfigurace projektu týmu ve službách týmu a.

    ![][12]

1. Po úspěšném ověření se zobrazí rozevírací seznam obsahující seznam týmové projekty Visual Studio týmovou. Vyberte název týmového projektu, který jste vytvořili v předchozích krocích a klikněte na tlačítko se zaškrtnutím v průvodci.

    ![][13]

1. Poté, co je propojený projektu, uvidíte některé pokyny k vrácení změn do projektu Visual Studio týmovou týmu.  Na vaše další vrácení se změnami Visual Studio týmovou vytvořte a nasaďte projekt do Azure.  Zkuste toto teď klepnutím na odkaz **Vrátit se změnami z aplikace Visual Studio** a potom na odkaz **Spusťte Visual Studio** (nebo na odpovídající tlačítko **Visual Studio** v dolní části okna portálu).

    ![][14]

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a>4: Aktivace nové vytvoření a přeinstalujte projektu

1. V aplikaci Visual Studio **Průzkumník týmu**zvolte odkaz **Průzkumník ovládacího prvku zdroje** .

    ![][15]

1. Přejděte na soubor řešení a otevřete ho.

    ![][16]

1. V **Okně Průzkumník**otevřete soubor a ho změnit. Například změnit soubor `_Layout.cshtml` v části zobrazení\\sdílené složky v roli MVC web.

    ![][17]

1. Upravit loga na web a zvolte **Kombinace kláves Ctrl + S** uložte soubor.

    ![][18]

1. V **Týmu Průzkumníka**zvolte odkaz **Neuložené změny** .

    ![][19]

1. Zadejte komentáře a potom klikněte na tlačítko **Vrátit se změnami** .

    ![][20]

1. Zvolte tlačítko **Domů** se vraťte do domovské stránky **Týmového Explorer** .

    ![][21]

1. Zvolte odkaz **vytvoří** zobrazíte buildy probíhá.

    ![][22]

    **Týmu Explorer** zobrazuje, aby byl spuštěn sestavení pro vaše vrácení se změnami.

    ![][23]

1. Poklikejte na název sestavení v průběhu zobrazíte podrobné protokol v průběhu Tvůrce dotazů.

    ![][24]

1. Při sestavování v průběhu, podívejte se na definici Tvůrce dotazů, která byla vytvořena, když jste vytvořili propojení TFS Azure pomocí průvodce.  Otevření místní nabídky pro definici Tvůrce dotazů a zvolte **Upravit definici vytvořit**.

    ![][25]

    Na kartě **aktivační událost** zobrazí se, že definice sestavení vytvářet na každé vrácení se změnami ve výchozím nastavení je.

    ![][26]

    Na kartě **proces** uvidíte, že prostředí nasazení je nastavený na název svojí cloudové služby nebo webové aplikace. Pokud pracujete s web apps, budou vlastnosti, které se zobrazí liší od zde uvedené.

    ![][27]

1. Hodnoty vlastností v případě potřeby nastavit odlišné hodnoty jsou výchozí hodnoty. Vlastnosti pro Azure publikování jsou v části **instalace** .

    Následující tabulka zobrazuje dostupné vlastnosti v části **nasazení** :

  	|Vlastnost|Výchozí hodnota|
  	|---|---|
  	|Povolit nedůvěryhodných certifikátů|Pokud je hodnota NEPRAVDA, musíte být přihlášení certifikáty SSL tak, že kořenová autorita.|
  	|Povolit Upgrade|Umožňuje nasazení k aktualizaci stávajícího nasazení místo abyste vytvářeli novou. Zachová IP adresu.|
  	|Neodstraňujte|Pokud je hodnota true, nedojde k přepsání stávajících nesouvisejících nasazení (upgrade je povoleno).|
  	|Cesta k nastavení nasazení|Cesta k souboru .pubxml pro web app, vzhledem ke kořenové složce repo. U cloudovým službám ignorovány.|
  	|Prostředí nasazení Sharepointu|Stejný jako název služby.|
  	|Prostředí Azure nasazení|Web app nebo cloudového název služby.|

1. Pokud používáte více konfigurace služeb (.cscfg soubory), můžete určit konfiguraci požadovanou služby v nastavení **vytvořit, Upřesnit, MSBuild argumenty** . Například použít ServiceConfiguration.Test.cscfg, nastavte MSBuild argumenty řádek `/p:TargetProfile=Test`.

    ![][38]

    V současné době vaše sestavení by měl být byla úspěšně dokončena.

    ![][28]

1. Pokud poklikejte na název Tvůrce dotazů aplikace Visual Studio zobrazuje **Vytvořit souhrn**, včetně žádné výsledky testů z přidružené Jednotková testovací projekty.

    ![][29]

1. [Azure klasické portál](http://go.microsoft.com/fwlink/?LinkID=213885)zobrazíte přidružené nasazení na kartě **nasazení** je-li vybrán testovacím prostředí.

    ![][30]

1.  Přejděte na adresu URL vašeho webu. Pro web app stačí klikněte na tlačítko **Procházet** na panelu s příkazy. Do cloudové služby vyberte adresu URL v části **Rychlé přehledně uspořádané** zobrazující pracovní prostředí do cloudové služby stránky **řídicího panelu** . Nasazení z nepřetržitý integrace služby cloudu jsou publikovány ve výchozím nastavení pracovním prostředí. Můžete určit nastavením vlastnosti **Alternativní prostředí služby cloudu** **výroby**. Toto snímek obrazovky ukazuje, kde je adresa URL webu na stránku řídicího panelu cloudové služby.

    ![][31]

    Na nové záložce prohlížeče se otevře zobrazíte pracovního webu.

    ![][32]

    Cloudové služby Pokud uděláte jiné změny na projekt, aktivační události můžete více vytvoří a shromáždí více nasazení. Nejnovější označené jako aktivní.

    ![][33]

## <a name="5-redeploy-an-earlier-build"></a>5: Přeinstalujte starší sestavení

Tento krok platí ke cloudovým službám a je nepovinný krok. Na portálu Azure klasické vyberte starší nasazení a klikněte na tlačítko **přeinstalujte** převinutí svůj web a starší vrácení se změnami.  Poznámka: Toto aktivace nové sestavení v TFS a vytvoření nové položky do historie nasazení.

![][34]

## <a name="6-change-the-production-deployment"></a>6: Změna provozní nasazení

Tento krok se týká jenom ke cloudovým službám, ne webových aplikací web apps. Jakmile budete připraveni, můžete převést pracovního prostředí provozním prostředí tak, že kliknete na tlačítko **Zaměnit** Azure klasické portálu. Nově nasazeném pracovního prostředí je stát výrobní a předchozí provozním prostředí, případně změní pracovním prostředí. Aktivní nasazení můžou být odlišná na produkční a pracovní prostředí, ale nasazení historii nedávných sestavení je stejná bez ohledu na prostředí.

![][35]

## <a name="7-run-unit-tests"></a>7: Spusťte testování

Tento krok platí pouze pro webové aplikace, není cloudových služeb. Brána jakosti v nasazení, spusťte testování a pokud se nezdaří, můžete zastavit nasazení.

1.  Ve Visual Studiu přidejte Jednotková testovací projekt.

    ![][39]

1.  Přidání odkazů projektu do projektu, který chcete testovat.

    ![][40]

1.  Přidání některé testování. Nejdřív zkuste formální test, který bude vždy projít.

        ```
        using System;
        using Microsoft.VisualStudio.TestTools.UnitTesting;

        namespace UnitTestProject1
        {
            [TestClass]
            public class UnitTest1
            {
                [TestMethod]
                [ExpectedException(typeof(NotImplementedException))]
                public void TestMethod1()
                {
                    throw new NotImplementedException();
                }
            }
        }
        ```

1.  Úprava definice Tvůrce dotazů, klikněte na kartu **proces** a rozbalte uzel **testu** .

1.  **Selhalo: sestavení na chybu testu** nastavena na hodnotu True. To znamená, že nasazení nedojde pokud zkoušku předat.

    ![][41]

1.  Fronta nové sestavení.

    ![][42]

    ![][43]

1. Během jdoucí sestavení zaškrtněte o průběhu.

    ![][44]

    ![][45]

1. Po dokončení sestavování zkontrolujte výsledky.

    ![][46]

    ![][47]

1.  Zkuste vytvořit test, který se nezdaří. Přidat nové zkušební zkopírováním první fázi, ji přejmenovat a řádek kód, který je uvedeno, že NotImplementedException je očekávaná výjimka.

        ```
        [TestMethod]
        //[ExpectedException(typeof(NotImplementedException))]
        public void TestMethod2()
        {
            throw new NotImplementedException();
        }
        ```

1. Aby se změna fronty nové sestavení se změnami.

    ![][48]

1. Zobrazení výsledků testu a zkontrolujte podrobnosti o chybě.

    ![][49]

    ![][50]

## <a name="next-steps"></a>Další kroky
Další informace o testování ve Visual Studiu týmovou částí najdete v článku [Spusťte testování ve vaší Tvůrce dotazů](http://go.microsoft.com/fwlink/p/?LinkId=510474). Pokud používáte libovolná, najdete v článku [sdílení kódu v libovolná](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) a [Nepřetržitý nasazení služby Azure aplikace](../app-service-web/app-service-continuous-deployment.md).  Další informace o Visual Studio týmovou najdete v článku [Visual Studio týmovou](http://go.microsoft.com/fwlink/?LinkId=253861).

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso/tfs1.png
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png

[5]: ./media/cloud-services-continuous-delivery-use-vso/tfs5.png
[6]: ./media/cloud-services-continuous-delivery-use-vso/tfs6.png
[7]: ./media/cloud-services-continuous-delivery-use-vso/tfs7.png
[8]: ./media/cloud-services-continuous-delivery-use-vso/tfs8.png
[9]: ./media/cloud-services-continuous-delivery-use-vso/tfs9.png
[10]: ./media/cloud-services-continuous-delivery-use-vso/tfs10.png
[11]: ./media/cloud-services-continuous-delivery-use-vso/tfs11.png
[12]: ./media/cloud-services-continuous-delivery-use-vso/tfs12.png
[13]: ./media/cloud-services-continuous-delivery-use-vso/tfs13.png
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso/tfs18.png
[19]: ./media/cloud-services-continuous-delivery-use-vso/tfs19.png
[20]: ./media/cloud-services-continuous-delivery-use-vso/tfs20.png
[21]: ./media/cloud-services-continuous-delivery-use-vso/tfs21.png
[22]: ./media/cloud-services-continuous-delivery-use-vso/tfs22.png
[23]: ./media/cloud-services-continuous-delivery-use-vso/tfs23.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso/tfs27.png
[28]: ./media/cloud-services-continuous-delivery-use-vso/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso/tfs37.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso/AdvancedMSBuildSettings.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso/AddUnitTestProject.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso/AddProjectReferences.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso/EditBuildDefinitionForTest.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso/QueueNewBuild.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso/QueueBuildDialog.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso/BuildInTeamExplorer.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso/BuildRequestInfo.PNG
[46]: ./media/cloud-services-continuous-delivery-use-vso/BuildSucceeded.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso/TestResultsPassed.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso/CheckInChangeToMakeTestsFail.PNG
[49]: ./media/cloud-services-continuous-delivery-use-vso/TestsFailed.PNG
[50]: ./media/cloud-services-continuous-delivery-use-vso/TestsResultsFailed.PNG
