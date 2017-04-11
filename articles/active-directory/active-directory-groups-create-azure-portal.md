<properties
    pageTitle="Vytvoření nové skupiny v náhledu Azure Active Directory | Microsoft Azure"
    description="Postup vytvoření skupiny v Azure Active Directory a přidání uživatelů (členové) do skupiny"
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="curtand"/>


# <a name="create-a-new-group-in-azure-active-directory-preview"></a>Vytvoření nové skupiny v náhledu Azure Active Directory

> [AZURE.SELECTOR]
- [Azure portálu](active-directory-groups-create-azure-portal.md)
- [Azure klasické portálu](active-directory-accessmanagement-manage-groups.md)
- [Prostředí PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)

Tento článek vysvětluje, jak vytvořit a naplňte hodnotami nové skupiny v náhledu Azure Active Directory (Azure AD). [Novinky v náhledu](active-directory-preview-explainer.md) Umožňuje provádět úlohy správy třeba přiřazování licencí nebo oprávnění pro určitý počet uživatelů nebo zařízení najednou skupiny.

## <a name="how-do-i-create-a-group"></a>Jak vytvořím skupinu?

1. Přihlaste se k [Azure portál](https://portal.azure.com) pomocí účtu, který je jako globální správce pro adresář.

2. Vyberte **Další služby**, zadejte **uživatele a skupiny,** do textového pole a potom stiskněte **Enter**.

  ![Otevření Správa uživatelů](./media/active-directory-groups-create-azure-portal/search-user-management.png)

3. Na zásuvné **Uživatelé a skupiny** vyberte **všechny skupiny**.

  ![Otevření zásuvné skupiny](./media/active-directory-groups-create-azure-portal/view-groups-blade.png)

4. Na zásuvné **Uživatelé a skupiny – všechny skupiny** vyberte příkaz **Přidat** .

  ![Výběr příkazu Přidat](./media/active-directory-groups-create-azure-portal/add-group-command.png)

5. Na zásuvné **Skupina** přidejte název a popis skupiny.

6. Chcete-li vybrat členové přidejte do skupiny, vyberte **přiřazené** v poli **typ členství** a vyberte **členy**. Další informace o tom, jak dynamicky Správa členství ve skupině najdete v článku [použití atributy vytvářet složitější pravidla pro členství ve skupině](active-directory-groups-dynamic-membership-azure-portal.md).

  ![Výběr členové přidejte](./media/active-directory-groups-create-azure-portal/select-members.png)

5. Na zásuvné **členů** vyberte jeden nebo více uživatelů nebo zařízení přidat do skupiny a potom vyberte tlačítko **Vyberte** v dolní části zásuvné si je přidat do skupiny. Pole pro **uživatelské** filtry zobrazení podle odpovídající položku na libovolnou část jména uživatele nebo zařízení. Žádné zástupné znaky přijaté v tomto poli.

6. Po dokončení přidávání členů do skupiny, vyberte možnost **vytvořit** na zásuvné **skupiny** .    

  ![Vytvoření skupiny potvrzení](./media/active-directory-groups-create-azure-portal/create-group-confirmation.png)




## <a name="additional-information"></a>Další informace

Tyto články poskytují další informace o Azure Active Directory.

* [Zobrazit existující skupiny](active-directory-groups-view-azure-portal.md)
* [Správa nastavení skupiny](active-directory-groups-settings-azure-portal.md)
* [Správa členů skupiny](active-directory-groups-members-azure-portal.md)
* [Správa členství ve skupině](active-directory-groups-membership-azure-portal.md)
* [Správa dynamických pravidel pro uživatele ve skupině](active-directory-groups-dynamic-membership-azure-portal.md)
