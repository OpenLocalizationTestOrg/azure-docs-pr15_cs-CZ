<properties 
   pageTitle="Správa Azure prostředků v Průzkumníkovi mraků | Microsoft Azure"
   description="Naučte se používat cloudové Explorer procházet a spravovat Azure zdrojů ve Visual Studiu."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="managing-azure-resources-with-cloud-explorer"></a>Správa Azure prostředků v Průzkumníkovi cloudu

##<a name="overview"></a>Základní informace

Průzkumník cloudu slouží k vám umožní snadněji a rychleji Procházet a spravovat Azure zdroje ve Visual STUDIU. Můžete, například umožňuje otevřít Web appu [Azure portál](http://go.microsoft.com/fwlink/p/?LinkID=525040) nebo v prohlížeči nebo připojit ladění, nebo můžete zobrazovat vlastnosti kontejneru objektů blob a otevřete ho v editoru kontejneru objektů Blob.

Cloudu Průzkumníka je integrována do zásobníku správce Azure zdroje, stejně jako [Azure portálu](http://go.microsoft.com/fwlink/p/?LinkID=525040). Rozumí zdroje, jako jsou skupiny Azure zdrojů a Azure služeb, jako je použití logických operátorů aplikace a rozhraní API aplikace a podporuje [řízení přístupu na základě rolí](./active-directory/role-based-access-control-configure.md) (RBAC). Azure prostředky, které jste přidali nebo změnili zobrazíte zvolte tlačítko **Aktualizovat** na panelu nástrojů Průzkumníka cloudu.

Shluk Explorer nainstalovaný jako součást aplikace Visual Studio nástroje pro Azure SDK 2.7. 

## <a name="prerequisites"></a>Zjistit předpoklady pro

- Visual Studio 2015 RTM.

- Visual Studio Tools for Azure SDK. 
- Taky musíte mít účet Azure a Zaprotokolují do ní zobrazíte Azure zdrojů v Průzkumníku cloudu. Pokud nemáte, můžete vytvořit účet v jenom pár minut. Pokud máte předplatné MSDN, najdete v článku [Azure výhody pro předplatitele](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). V opačném najdete v článku [Vytvoření bezplatnou zkušební verzi účtu](https://azure.microsoft.com/pricing/free-trial/).

- Pokud mraků Explorer nezobrazuje, můžete ho zobrazit výběrem **zobrazení** **Ostatní okna** **Průzkumníka mraků** na řádek nabídek.

## <a name="manage-azure-accounts-and-subscriptions"></a>Správa Azure účty a předplatného

Azure zdrojů v cloudu Průzkumník zobrazíte budete muset přihlásit k Azure účet u jednoho nebo více aktivní předplatné. Pokud máte víc účtů Azure, můžete přidat v Průzkumníku cloudu a klikněte na předplatná, která chcete zahrnout do zobrazení zdroje cloudu Průzkumníka.

Pokud nepoužijete Azure před nebo jste do aplikace Visual Studio nepřidali potřebné účty, budete vyzváni k tomu nevyzve.

## <a name="to-add-azure-accounts-to-cloud-explorer"></a>Pokud chcete přidat do cloudu Explorer účtů Azure

1. Zvolte ikonu nastavení v panelu nástrojů Průzkumníka cloudu.

1. Zvolte odkaz **Přidat účet** . Přihlaste se k účet Azure jehož zdroje, chcete-li procházet. Účet, který jste právě přidali by měla být vybraná v rozevíracím seznamu pro výběr účtu. Předplatná pro jeho účet zobrazí pod položkou účtu.

    ![Přidání Azure předplatná](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819514.png)

    ![Výběr Azure předplatná](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819515.png)

1. Zaškrtněte políčka u předplatných účtu, který chcete procházet a potom klikněte na tlačítko **použít** .

    Azure zdroje vybraného předplatného se zobrazují v Průzkumníku cloudu.

## <a name="to-remove-an-azure-account"></a>Chcete-li odebrat účet Azure

1. Vyberte **soubor**, příkaz **Nastavení účtu** v řádku nabídek.

1. V části **Všechny účty** dialogového okna **Nastavení účtu** vyberte příkaz **Odebrat** vedle účtu, že který chcete odebrat. Poznámka: aby tento příkaz pouze odebere účet z aplikace Visual Studio – it nemá vliv přímo účet Azure.

## <a name="view-resource-types-or-groups"></a>Zobrazení typy prostředků nebo skupiny

Zobrazíte Azure zdroje můžete vybrat **Typy prostředků** nebo zobrazení **Zdrojů skupiny** .

![Rozevírací seznam zobrazení zdrojů](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819516.png)

- Zobrazení **Typy zdrojů** , které se také běžné zobrazení použité na [Azure portál](http://go.microsoft.com/fwlink/p/?LinkID=525040)zobrazuje Azure zdroje zařazené do kategorií podle typu, jako jsou webové aplikace, úložiště účty a virtuálních počítačích. Postup je podobný jako na tom, jak Azure zdroje se zobrazí v okně Průzkumník serveru.

- Zobrazení skupiny zdrojů Azure zdroje podle skupiny Azure zdroje, který máte přidružený k rozdělíte do kategorií.

 
    Skupina zdroje je sada Azure zdrojů, obvykle používají dané aplikace. Další informace o skupinách Azure prostředků najdete v tématu [Přehled Správce prostředků Azure](./resource-group-overview.md).

## <a name="view-and-navigate-resources"></a>Zobrazení a procházení zdroje

Přejděte na Azure zdroje a zobrazit její informace v cloudu Průzkumníka, rozbalte položku Typ položku nebo skupinu přidružené zdroje a pak zvolte zdroje. Když zvolíte zdroje, informace se zobrazí v těchto karet v dolní části cloudu Průzkumníka.

![Zvolte zobrazení zdrojů](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819517.png)

- Na kartu **Akce** zobrazuje akce, které můžete provést v cloudu Explorer pro vybraný zdroj. Najdete v článku dostupné akce v místní nabídce zdroje.

- Karta **Vlastnosti** zobrazí vlastnosti prostředku, například jeho typ, národní prostředí a zdroje skupiny, který je spojený s.

Akce **Otevřít portálu**má každý zdroj. Pokud zvolíte tuto akci, zobrazí cloudu Explorer vybraný zdroj [Azure portálu](http://go.microsoft.com/fwlink/p/?LinkID=525040). Tato funkce je užitečný zejména pro navigaci na velký vnořené zdroje.

Další akce a nemovitostí s hodnotou se může taky zobrazit založené na Azure zdroje. Například web apps a logika aplikace mít taky zajištěný akce **otevřený v prohlížeči** a **ladění připojit** kromě **Otevřít portálu**. Akce otevřete editory objeví, když vyberete účtu úložiště objektů blob, fronty nebo tabulku. Azure aplikace mají vlastnosti **adresy URL** a **Stav** , zatímco prostředků úložiště mají vlastnosti řetězec klíče a připojení.

## <a name="search-resources"></a>Hledání zdrojů

Vyhledání zdroje s určitým názvem v předplatných účet Azure, zadejte název do vyhledávacího pole v Průzkumníku cloudu.

![Hledání zdrojů v cloudu Průzkumníka](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC820394.png)

Zadávání znaků ve vyhledávacím poli se zobrazí pouze prostředky, které odpovídají tyto znaky ve stromové struktuře zdrojů.

