<properties
   pageTitle="Začínáme se šablonami soukromé | Microsoft Azure"
   description="Přidání, spravovat a sdílet soukromé šablon pomocí portálu Azure, rozhraní příkazového řádku Azure nebo Powershellu."
   services="marketplace-customer"
   documentationCenter=""
   authors="VybavaRamadoss"
   manager="asimm"
   editor=""
   tags="marketplace, azure-resource-manager"
   keywords=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/18/2016"
   ms.author="vybavar"/>

# <a name="get-started-with-private-templates-on-the-azure-portal"></a>Začínáme s soukromé šablony na portálu Azure

[Správce prostředků Azure](../resource-group-authoring-templates.md) šablona je deklarativní šablonu použít k definování nasazení. Můžete definovat prostředky do nasazení řešení a zadejte parametry a proměnných, které vám umožní vstupní hodnoty na jiném prostředí. Šablona se skládá z JSON a výrazy, které můžete použít k vytvoření hodnoty pro nasazení.

Můžete funkci nové **šablony** na [Portál Azure](https://portal.azure.com) spolu s zprostředkovatele prostředků **Microsoft.Gallery** jako rozšíření [Azure Marketplace](https://azure.microsoft.com/marketplace/) umožníte uživatelům k vytváření, Správa a instalace soukromé šablon z osobní knihovny.

Tento dokument provede vás přidávání, Správa a sdílení soukromých **šablony** pomocí portálu Azure.

## <a name="guidance"></a>Pokyny pro

Následující návrhy se můžete využít úplný **šablony** při práci s řešení:

- **Šablona** je encapsulating prostředek, který obsahuje šablony správce prostředků a další metadata. K nějaké položce v Tržiště chová velmi podobně. Klíčové rozdíl je jeho Soukromá položka namísto veřejné položky Marketplace.
- **Šablony** knihovny vhodný pro uživatele, kteří potřebují přizpůsobit své nasazení.
- **Šablony** fungují dobře pro uživatele, kteří potřebují jednoduchý úložiště v Azure.
- Použití existující šablony správce prostředků. Vyhledání šablony v [github](https://github.com/Azure/azure-quickstart-templates) nebo [Exportovat šablonu](../resource-manager-export-template.md) z existující skupiny zdrojů.
- **Šablony** je stejným uživateli, který je publikuje. Název aplikace publisher je viditelný všem, kdo má přístup pro čtení.
- **Šablony** je správce prostředků zdrojů a nelze přejmenovat jednou publikované.

## <a name="add-a-template-resource"></a>Přidání zdroje šablony

Existují dva způsoby, jak vytvořit **šablonu** zdroje na portálu Azure.

### <a name="method-1--create-a-new-template-resource-from-a-running-resource-group"></a>Metoda 1: Vytvoření nového prostředku šablony ze skupiny pracovního zdroje

1. Přejděte do existující skupiny zdroje na portálu Azure. V **dialogovém okně Nastavení**zaškrtněte políčko **exportovat šablony** .
2. Po exportu správce prostředků šablona umožňuje na tlačítko **Uložit šablonu** si ji uložit do úložiště **šablony** . Dokončení podrobné informace o exportu šablony najdete [tady](../resource-manager-export-template.md).
<br /><br />
![Export skupina zdroje](media/rg-export-portal1.PNG)  <br />

3. Vyberte příkazové tlačítko **Uložit do šablony** .
<br /><br />

4. Zadejte následující informace:

    - Název – objektu šablony (Poznámka: to je název správce prostředků Azure založena. Použití všechna naming omezení a nejde je změnit po vytvoření).
    - Popis – rychlý přehled o šabloně.

    ![Uložení šablony](media/save-template-portal1.PNG)  <br />

5. Klikněte na **Uložit**.

    > [AZURE.NOTE] Šablona zásuvné exportovat zobrazí oznámení, pokud exportovaných šablon správce prostředků obsahuje chyby, ale pořád moct uložit tuto šablonu správce prostředků k šablonám. Zajistěte, aby kontrola a oprava problémů s šablony některý správce prostředků před znovu nasazení exportovaných šablon správce prostředků.

### <a name="b-method-2--add-a-new-template-resource-from-browse"></a>B. Metoda 2: Přidání nového prostředku šablony z Procházet

Můžete také přidat novou **šablonu** z pomocné pomocí + přidat příkazové tlačítko v **Procházet > šablon**. Je třeba zadat název, popis a šabloně správce prostředků JSON.

![Přidání šablony](media/add-template-portal1.PNG)  <br />

> [AZURE.NOTE] Microsoft.Gallery je klienta na základě zprostředkovatele Azure prostředků. Šablonu zdroje je svázané se uživatel, který ho vytvořili. Není stejným do určitého předplatného. Předplatné, musí být vybrali pouze v případě, že nasazení šablony.

## <a name="view-template-resources"></a>Zobrazení zdrojů šablony

Všechny **šablony** jsou k dispozici pro vás uvidí na **Procházet > šablon**. Jedná se o **šablony** nevytvoříte i ty, které sdílí s vámi s různé úrovně oprávnění. Další informace v části [řízení přístupu](#access-control-for-a-tenant-resource-provider) .

![Šablona zobrazení](media/view-template-portal1.PNG)  <br />

Podrobnosti o **šablony** můžete zobrazit kliknutím na položku v seznamu.

![Šablona zobrazení](media/view-template-portal2c.png)  <br />

## <a name="edit-a-template-resource"></a>Úprava šablony prostředku

Tok upravit **šablonu** můžete zahájit klepnutím pravým tlačítkem myši na položku v seznamu procházet nebo tak, že kliknete na tlačítko příkaz Upravit.

![Úprava šablony](media/edit-template-portal1a.PNG)  <br />

Nebo můžete upravit popis text šablony správce prostředků. Název nelze upravit, protože to je název zdroje správce prostředků. Při úpravě správce prostředků šablony JSON jsme ověří zajistit, který je platný JSON. Vyberte **OK** a pak **Uložit** pro uložení aktualizované šablony.

![Úprava šablony](media/edit-template-portal2a.PNG)  <br />

Po uložení **šablony** se zobrazí oznámení s potvrzením.

![Úprava šablony](media/edit-template-portal3b.png)  <br />

## <a name="deploy-a-template-resource"></a>Nasazení šablony zdroje

Můžete nasadit si nějakou **šablonu** , kterou máte oprávnění **ke čtení** na. Tok nasazení spustí standardní zásuvné nasazení šablony Azure. Vyplňte hodnoty parametrů šablony správce prostředků pokračujte nasazení.

![Nasazení šablony](media/deploy-template-portal1b.png)  <br />

## <a name="share-a-template-resource"></a>Sdílení prostředku šablony

**Šablonu** zdroje můžete sdílet s ostatními uživateli. Sdílení se chová podobně jako [přiřazování rolí pro všechny zdroje na Azure](../active-directory/role-based-access-control-configure.md). Vlastník **šablony** nabízí oprávnění ostatním uživatelům, kteří můžete komunikovat šablonu zdroje. Osoba nebo skupina lidí, se kterými sdílíte **šablony** se bude moct najdete v článku správce prostředků šablony a vlastností galerie.

### <a name="access-control-for-the-microsoftgallery-resources"></a>Řízení přístupu k prostředkům Microsoft.Gallery

Role | Oprávnění
---|----
Vlastník | Umožňuje úplné řízení na šablonu zdroje, včetně sdílet
Čtečky | Umožňuje číst a Execute(Deploy) na šablonu zdroje
Skupiny přispěvatelů | Umožňuje upravit a odstranit oprávnění na šablonu zdroje. Uživatel nemůže šabloně sdílet s ostatními

Klepnutím pravým tlačítkem myši nebo na zásuvné zobrazení určité položky, vyberte položku Procházet **sdílet** . Spustí se setkat i v případě sdílet.

![Sdílet šablony](media/share-template-portal1a.png)  <br />

 Teď můžete roli a uživatele nebo skupiny zajistit přístup k určité **šablony**. Dostupné role se vlastníka, čtečka přispěvatele. Další informace v části [řízení přístupu](#access-control-for-a-tenant-resource-provider) .

![Sdílet šablony](media/share-template-portal2b.png)  <br />

![Sdílet šablony](media/share-template-portal3b.png)  <br />

Klikněte na **Vybrat** a **Ok**. Teď můžete vidět uživatele nebo skupiny, které jste přidali ke zdroji.

![Sdílet šablony](media/share-template-portal4b.png)  <br />

> [AZURE.NOTE] Šablony můžete sdílet jenom s uživatelé a skupiny ve stejném klientovi Azure Active Directory. Pokud sdílíte šablony s e-mailovou adresu, která není ve vašem klientovi, pozvánku odešle výzvou ke klientovi jako Host.

## <a name="next-steps"></a>Další kroky

- Další informace o vytváření správce prostředků šablon najdete v tématu [vytváření šablon](../resource-group-authoring-templates.md)
- Funkce, které můžete použít v šabloně aplikace Správce prostředků, najdete v tématu [funkce šablony](../resource-group-template-functions.md)
- Návrh šablony, najdete v článku [Doporučené postupy pro navrhování správce prostředků Azure šablony](../best-practices-resource-manager-design-templates.md)
