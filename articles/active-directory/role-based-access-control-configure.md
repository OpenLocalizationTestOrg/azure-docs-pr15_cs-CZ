<properties
    pageTitle="Pomocí řízení přístupu na základě rolí v portálu Azure | Microsoft Azure"
    description="Začněte v části Správa přístupu na základě rolí řízení přístupu na portálu Azure. Přiřazení rolí umožňuje oprávnění přiřadit zdroje."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="use-role-assignments-to-manage-access-to-your-azure-subscription-resources"></a>Správa přístupu k prostředkům Azure předplatného pomocí přiřazování rolí

> [AZURE.SELECTOR]
- [Správa přístupu uživatele nebo skupiny](role-based-access-control-manage-assignments.md)
- [Správa přístupu podle zdroje](role-based-access-control-configure.md)

Azure na základě rolí přístup ovládacího prvku (RBAC) umožňuje Správa jemně odstupňovaná přístupu pro Azure. Použití RBAC, můžete udělit jenom množství přístup, aby uživatelé provádět své práce. Tento článek pomáhá vstoupit do začátků s RBAC portálu Azure. Pokud budete potřebovat další informace o způsobu RBAC pomáhá spravovat přístup, přečtěte si téma [Co je řízení přístupu na základě rolí](role-based-access-control-what-is.md).

## <a name="view-access"></a>Přístup k zobrazení
Můžete zjistit, kdo má přístup ke zdroji, skupina zdroje nebo předplatného z hlavního zásuvné [Azure portálu](https://portal.azure.com). Chceme třeba zjistit, kdo má přístup k některému z našich skupiny zdrojů:

1. Na navigačním panelu na levé straně vyberte **skupiny zdrojů** .  
    ![Skupiny zdrojů – ikona](./media/role-based-access-control-configure/resourcegroups_icon.png)
2. Vyberte název skupiny zdrojů z zásuvné **skupiny zdrojů** .
3. V nabídce nalevo vyberte **řízení přístupu (IAM)** .  
4. Ovládací prvek zásuvné přístup seznam všech uživatelů, skupin a aplikace, které máte udělený přístup ke skupině zdroje.  

    ![Uživatelé zásuvné - zděděný a přiřazené snímek aplikace access](./media/role-based-access-control-configure/view-access.png)

Oznámení, že někteří uživatelé jsou **přiřazené** zatímco jiné **Inherited** k němu přístup. Access konkrétně přiřazená skupina zdroje nebo dědí od přiřazení nadřazeného předplatného.

> [AZURE.NOTE] Klasický předplatné správce a dalších správců jsou považovány za vlastníci předplatného v novém RBAC modelu.


## <a name="add-access"></a>Přidání aplikace Access
Udělit přístup z aplikace zdroje, skupina zdroje nebo předplatné, které je rozsah přiřazování rolí.

1. Klikněte na **Přidat** na zásuvné řízení přístupu.  
2. Vyberte roli, která chcete z zásuvné **Vyberte roli** přiřadit.
3. Vyberte uživatele, skupinu nebo aplikace v adresáři, který chcete udělit přístup k. Můžete hledat v adresáři se zobrazit jména, e-mailové adresy a identifikátory objektů.  

    ![Přidání uživatelů zásuvné – hledání snímek](./media/role-based-access-control-configure/grant-access2.png)

4. Kliknutím na **OK** vytvořte přiřazení. V rozevírací nabídce **Přidání uživatele** ke sledování průběhu.  
    ![Indikátor průběhu přidávání uživatelů – snímek](./media/role-based-access-control-configure/addinguser_popup.png)

Po úspěšném přidání přiřazování rolí, zobrazí se na zásuvné **uživatelů** .

## <a name="remove-access"></a>Odebrání aplikace Access

1. Vyberte přiřazování rolí na zásuvné řízení přístupu.
2. Vyberte **Odebrat** v zásuvné podrobnosti o přiřazení.  
3. Výběrem možnosti **Ano** potvrďte odebrání.  
    ![Uživatelé zásuvné - odebrat z role snímek](./media/role-based-access-control-configure/remove-access1.png)

Zděděný přiřazení nejde odebrat. Všimněte si na následujícím obrázku je šedá na tlačítko Odebrat. Místo toho podívejte se na **Přiřazené na** podrobnosti. Přejděte na zdroje tam uvedených odebrat přiřazování rolí.

![Uživatelé zásuvné – zakáže zděděných přístup odebrat tlačítko snímek](./media/role-based-access-control-configure/remove-access2.png)

## <a name="other-tools-to-manage-access"></a>Další nástroje pro správu aplikace access
Můžete přiřadit role a spravovat přístup s Azure RBAC příkazy v nástrojích než portálu Azure.  Použít odkazy a další informace o požadavky a začít pracovat s příkazy Azure RBAC.

- [Azure Powershellu](role-based-access-control-manage-access-powershell.md)
- [Azure rozhraní příkazového řádku](role-based-access-control-manage-access-azure-cli.md)
- [ROZHRANÍ REST API](role-based-access-control-manage-access-rest.md)

## <a name="next-steps"></a>Další kroky
- [Vytvoření sestavy historie změn aplikace access](role-based-access-control-access-change-history-report.md)
- V tématu [RBAC předdefinované role](role-based-access-built-in-roles.md)
- Definovat vlastní [vlastní role v Azure RBAC](role-based-access-control-custom-roles.md)
