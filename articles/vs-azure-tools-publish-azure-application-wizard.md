<properties 
   pageTitle="Publishing Wizard Azure aplikace | Microsoft Azure"
   description="Publishing Wizard Azure aplikace"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="publish-azure-application-wizard"></a>Publishing Wizard Azure aplikace

## <a name="overview"></a>Základní informace

Po vývoj webové aplikace ve Visual Studiu můžete publikovat aplikace snadněji služby Azure cloudu pomocí průvodce **Publikování aplikací Azure** . První oddíl vysvětluje postup, musíte nejdřív udělat předtím, než použijete průvodce a v dalších částech vysvětlují funkce průvodce.

>[AZURE.NOTE] Toto téma je o nasazení ke cloudovým službám, nikoli na weby. Informace o nasazení na weby najdete v tématu [nasazení webový server Azure](https://social.msdn.microsoft.com/Search/windowsazure?query=How%20to%20Deploy%20an%20Azure%20Web%20Site&Refinement=138&ac=4#refinementChanges=117&pageNumber=1&showMore=false).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Musíte před publikováním webové aplikace Azure, musíte mít účet Microsoft a předplatné Azure a máte přiřazenou webové aplikace služby Azure cloudu. Pokud jste již provedli tyto úkoly, můžete přejít na další oddíl.

1. Pokud potřebujete Azure předplatného a svým účtem Microsoft. Můžete zkusit bezplatnou jeden měsíc Azure neváhejte [tady](https://azure.microsoft.com/pricing/free-trial/)

1. Vytvořte Cloudová služba a účet úložiště v Azure. Můžete to udělat z Průzkumníka serveru ve Visual Studiu nebo pomocí [Azure klasické portálu](http://go.microsoft.com/fwlink/?LinkID=213885).

1. Povolení webové aplikace pro Azure. Povolit webové aplikace z aplikace Visual Studio publikování Azure, musíte ji přidružit projekt služby Azure cloudu ve Visual Studiu. Pokud chcete vytvořit projekt přidružené cloudové služby, otevřete místní nabídky pro project pro webové aplikace a zvolte převodem, **Převod na projekt cloudové služby Azure**.

1. Po projekt cloudové služby je přidán do vašeho řešení, znovu otevřít stejný místní nabídku a pak vyberte **Publikovat**. Další informace o tom, jak povolit aplikace Azure najdete v tématu [jak: migrace a publikovat webové aplikace cloudové služby Azure z aplikace Visual Studio](https://msdn.microsoft.com/library/azure/hh420322.aspx).

>[AZURE.NOTE] Ujistěte se, chcete-li začít Visual Studio přihlašovací údaje správce (Spustit jako správce).

1. Až budete připravení publikovat aplikace, otevřete místní nabídky pro projekt služby Azure cloudu a pak vyberte **Publikovat**. Následující postup Průvodce publikování aplikací Azure.

## <a name="choosing-your-subscription"></a>Výběr předplatného

### <a name="to-choose-a-subscription"></a>Chcete-li zvolit předplatné

1. Před použitím průvodce poprvé, musíte se přihlásit. Zvolte odkaz **Sign In** . Přihlaste se k portálu Azure po zobrazení výzvy a zadat Azure uživatelské jméno a heslo. 

    ![To je jedna publikování obrazovky Průvodce](./media/vs-azure-tools-publish-azure-application-wizard/IC799159.png)

    Seznam předplatných naplní předplatná přidruženého k vašemu účtu. Může taky zobrazit předplatná z předplatného typů souborů, které jste importovali dříve.

1. V seznamu **Zvolte předplatné** vyberte předplatné pro účely tohoto nasazení.

   Pokud se rozhodnete **< spravovat >**, zobrazí se dialogové okno **Spravovat předplatná** a můžete si vybrat předplatné a uživatelský účet, který chcete použít. Karta **účty** zobrazuje všechny vaše účty a **předplatných** na kartě zobrazena všechna předplatná spojené s účty. Můžete taky zvolit oblast ze které budete používat Azure prostředky, jakož i možnost vytvořte nebo importujte certifikáty pro vaše předplatné z portálu Microsoft Azure. Pokud jste importovali žádné předplatné ze souboru předplatného, přidružené certifikáty se zobrazí na kartě **certifikáty** . Až budete hotovi, klikněte na tlačítko **Zavřít** .

    ![Správa předplatného](./media/vs-azure-tools-publish-azure-application-wizard/IC799160.png)

    >[AZURE.NOTE] Předplatné soubor může obsahovat více než jedno předplatné.

1. Zvolte tlačítko **Další** a pokračujte. 

    Pokud nejsou k dispozici žádné cloudovým službám ve vašem předplatném, je potřeba vytvořit do cloudové služby v Azure k hostování vašeho projektu. Zobrazí se dialogové okno **vytvořit Cloudová služba a úložiště účtu** .

    Zadejte nový název cloudovou službu. Název musí být jedinečný v Azure. Zadejte oblast nebo skupinu spřažení datovém centru, který je v blízkosti nebo většina klienty. Tento název slouží k vytvoření nového účtu úložiště, který Azure vytvoří cloudové službě.

1. Úprava nastavení chcete pro toto nasazení a potom ji znovu publikovat tak, že kliknete na tlačítko **Publikovat** (v další části najdete další informace o různých nastavení). Chcete-li zobrazit nastavení před publikováním, zvolte tlačítko **Další** .

    >[AZURE.NOTE] Pokud jste se rozhodli publikovat v tomto kroku, můžete sledovat stav tohoto nasazení ve Visual Studiu.

Společné a Upřesnit nastavení pro nasazení můžete změnit pomocí průvodce **Publikování aplikací Azure** . Můžete nastavení pro nasazení aplikace v testovacím prostředí musíte uvolnit dříve než. Následující obrázek znázorňuje kartě **Běžné nastavení** Azure nasazení.

![Běžné nastavení](./media/vs-azure-tools-publish-azure-application-wizard/IC749013.png)

## <a name="configuring-your-publish-settings"></a>Konfigurace vaše nastavení publikování

### <a name="to-configure-the-publish-settings"></a>Konfigurace nastavení publikování

1. V seznamu **cloudové služby** proveďte jednu z těchto kroků:

   1. V rozevíracím seznamu vyberte existující cloudové služby. Zobrazí se umístění Centrum dat pro službu. By měl poznamenejte si toto umístění a mít jistotu umístění účtu úložiště ve stejném datovém centru.

    1. Klikněte na **Vytvořit nový** vytvoříte do cloudové služby, která hostuje Azure. V dialogovém okně **Vytvořit cloudové služby** zadejte název pro službu a zadejte oblast nebo skupinu spřažení určit umístění datovém centru, které chcete hostovat cloudové služby. Název musí být jedinečný v Azure.

1. V seznamu **prostředí** zvolte **výrobní** nebo **pracovní**. Pracovní prostředí vyberte, pokud chcete nasazení aplikace v testovacím prostředí. Můžete přesouvat aplikace provozním prostředí později.

1. V seznamu **vytvořit konfigurace** vyberte **ladění** nebo **vydání**.

1. V seznamu **Konfigurace služby** zvolte **cloudu** a **místní**.

    Pokud chcete mít možnost vzdáleně připojovat ke službě, zaškrtněte políčko **Povolit ke vzdálené ploše pro všechny role** . Tuto možnost primárně používají pro řešení potíží. Když vyberete toto políčko, zobrazí se dialogové okno **Konfigurace vzdálené plochy** . Zvolte nastavení odkaz změnit konfiguraci.

    **Povolení nasazení webu pro všechny role web** zaškrtněte políčko Povolit nasazení webové služby. Je třeba povolit vzdálená plocha tuto funkci lze použít. Další informace najdete v tématu [[publikování do cloudové služby používat nástroje Azure](https://msdn.microsoft.com/library/azure/ff683672.aspx)](https://msdn.microsoft.com/library/azure/ff683672.aspx). Další informace o nasazení webu najdete v tématu [[publikování do cloudové služby používat nástroje Azure](https://msdn.microsoft.com/library/azure/ff683672.aspx)](https://msdn.microsoft.com/library/azure/ff683672.aspx).

1. Zvolte kartu **Upřesnit nastavení** . Do pole **Popisek nasazení** chvíli přijmout výchozí název, nebo zadejte název e. Přidání data do popisku nasazení, ponechejte toto políčko zaškrtnuté.

    ![Třetí obrazovky Publishing Wizard](./media/vs-azure-tools-publish-azure-application-wizard/IC749014.png)

1. V seznamu **úložiště účtu** zvolte účet úložiště pro účely tohoto nasazení. Porovnejte umístění datacentrech pro Cloudová služba a vaším účtem úložiště. V ideálním případě těmto místům se musí shodovat.

    >[AZURE.NOTE] Účet Azure úložiště ukládá balíček pro nasazení aplikace. Po nasazení aplikace balíček zmizí z účtu úložiště.

1. Pokud chcete nasadit pouze aktualizované součásti, zaškrtněte políčko **Aktualizovat nasazení** . Tento typ nasazení může být rychlejší než úplné nasazení. Zvolte **Nastavení** odkaz pro otevření dialogového okna **nasazení aktualizovat nastavení** , vidíte na následujícím obrázku. 

    ![Nastavení nasazení](./media/vs-azure-tools-publish-azure-application-wizard/IC617060.png)

    Můžete zvolit jednu ze dvou možností aktualizace nasazení, přírůstková nebo souběžné. Přírůstková nasazení aktualizuje jednu nasazeném instanci najednou, tak, aby vaše aplikace zůstává online a ostatní vás uživatelů. Souběžné nasazení aktualizuje všechny instance nasazeném najednou. Současné aktualizaci je rychlejší než přírůstková aktualizace, ale pokud zvolíte tuto možnost, aplikace nemusí být k dispozici během procesu aktualizace.

    Pokud nasazení nelze aktualizovat, by měl zaškrtněte políčko pro úplné nasazení dělat, když chcete úplné nasazení proběhne automaticky Pokud dochází k selhání nasazení aktualizace. Úplné nasazení obnoví virtuální adresy IP (VIP) služby cloudu. Další informace najdete v tématu [jak: uchovávat konstantní virtuální IP adresu do cloudové služby](https://msdn.microsoft.com/library/azure/jj614593.aspx).


1. Ladění služby, zaškrtněte políčko **Povolit IntelliTrace** nebo Pokud nasazujete konfiguraci **ladění** a chcete ladění cloudové služby v Azure, zaškrtněte políčko **Povolit vzdálené ladění pro všechny role** pro nasazení vzdálené ladění.

2. Profil aplikace, zaškrtněte políčko **Povolit vytváření profilu** a klikněte na odkaz **Nastavení** zobrazte možnosti pro vytváření profilů. 


    >[AZURE.NOTE] Musíte pomocí aplikace Visual Studio Ultimate povolit IntelliTrace nebo vrstvy interakce vytváření profilů TIP () a není možné povolit i ve stejnou dobu.

    Další informace najdete v tématu [ladění Cloudová služba publikované s IntelliTrace a Visual Studia](https://msdn.microsoft.com/library/azure/ff683671.aspx) a [Testování výkonu do cloudové služby](https://msdn.microsoft.com/library/azure/hh369930.aspx).

1. Vyberte **Další** zobrazíte souhrnnou stránku aplikace.

## <a name="publishing-your-application"></a>Publikování aplikace

1. Můžete vytvořit profil publikování z nastavení, které jste zvolili. Například může vytvořit jeden profil testovacím prostředí a jiné pro výroby. Uložte tento profil, vyberte ikonu **Uložit** . Průvodce vytvoří profilu a uloží jej ve Visual Studiu projektu. Chcete-li změnit název profilu, otevřete seznam **cílový profil** a zvolte **< spravovat >**.

    ![Souhrnná obrazovka Publishing Wizard](./media/vs-azure-tools-publish-azure-application-wizard/IC749015.png)

    >[AZURE.NOTE] Publikování profil se zobrazí v okně Průzkumník ve Visual Studiu a nastavení profilu je aby došlo k zápisu soubor s příponou .azurePubxml. Nastavení se uloží jako atributy značky XML.

1. Vyberte **Publikovat** publikovat aplikace. Můžete sledovat stav procesu v okně **výstupu** ve Visual Studiu.

## <a name="see-also"></a>Viz taky

[Postup: migrace a publikovat webové aplikace služby Azure cloudu z aplikace Visual Studio](https://msdn.microsoft.com/library/azure/hh420322.aspx)

[Publikování do cloudové služby používat nástroje Azure](https://msdn.microsoft.com/library/azure/ff683672.aspx)

[Ladění publikované Cloudová služba s IntelliTrace a Visual Studio](https://msdn.microsoft.com/library/azure/ff683671.aspx)

[Testování výkonu do cloudové služby](https://msdn.microsoft.com/library/azure/hh369930.aspx)

