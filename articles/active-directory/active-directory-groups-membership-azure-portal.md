<properties
    pageTitle="Správa skupin skupiny patří do skupiny ve verzi preview Azure Active Directory | Microsoft Azure"
    description="Skupiny může obsahovat další skupiny služby Azure Active Directory. Tady je postup, jak spravovat tyto členství."
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
    ms.date="09/12/2016"
    ms.author="curtand"/>


# <a name="manage-the-groups-your-group-is-a-member-of-in-azure-active-directory-preview"></a>Správa skupin, do kterých patří do skupiny v náhledu Azure Active Directory vaší skupině

Skupiny můžou obsahovat jiné skupiny v náhledu Azure Active Directory. [Novinky v náhledu](active-directory-preview-explainer.md) Tady je postup, jak spravovat tyto členství.

## <a name="how-do-i-find-the-groups-my-group-is-a-member-of"></a>Jak najít skupin, do kterých patří do skupiny mé skupiny?

1.  Přihlaste se k [Azure portál](https://portal.azure.com) pomocí účtu, který je jako globální správce pro adresář.

2.  Vyberte **Další služby**, zadejte do textového pole **Uživatelé a skupiny** a pak stiskněte **Enter**.

  ![Otevření Správa uživatelů](./media/active-directory-groups-membership-azure-portal/search-user-management.png)

3.  Na zásuvné **Uživatelé a skupiny** vyberte **všechny skupiny**.

  ![Otevření zásuvné skupiny](./media/active-directory-groups-membership-azure-portal/view-groups-blade.png)

4. Na zásuvné **Uživatelé a skupiny – všechny skupiny** vyberte skupinu.

5. Na *název *skupiny – *skupiny* ** zásuvné, vyberte **skupinu členství **.

  ![Otevření zásuvné členství ve skupině](./media/active-directory-groups-membership-azure-portal/group-membership-blade.png)

6. Chcete-li přidat skupiny jako člen jiné skupiny na zásuvné **Group - členství ve skupinách** vyberte příkaz **Přidat** .

7. Vyberte ze zásuvné **Skupiny vyberte** skupinu a klikněte na tlačítko **Vybrat** v dolní části zásuvné. Skupiny můžete přidat pouze do jedné skupiny najednou. Pole pro **uživatelské** filtry zobrazení podle odpovídající položku na libovolnou část jména uživatele nebo zařízení. Žádné zástupné znaky přijaté v tomto poli.

  ![Přidání členství ve skupinách](./media/active-directory-groups-membership-azure-portal/add-group-membership.png)

8. Odebrání skupiny jako člen jiné skupiny na zásuvné **Group - členství ve skupinách** , vyberte požadovanou skupinu.

9. Na zásuvné ***název skupiny*** vyberte příkaz **Odebrat** a potvrďte svojí volbě příkazového řádku.

  ![odebrání příkazu členství](./media/active-directory-groups-membership-azure-portal/remove-group-membership.png)

9. Po dokončení změn členství ve skupině skupiny, vyberte **Uložit**.


## <a name="additional-information"></a>Další informace

Tyto články poskytují další informace o Azure Active Directory.

* [Zobrazit existující skupiny](active-directory-groups-view-azure-portal.md)
* [Vytvoření nové skupiny a přidání členů](active-directory-groups-create-azure-portal.md)
* [Správa nastavení skupiny](active-directory-groups-settings-azure-portal.md)
* [Správa členů skupiny](active-directory-groups-members-azure-portal.md)
* [Správa dynamických pravidel pro uživatele ve skupině](active-directory-groups-dynamic-membership-azure-portal.md)
