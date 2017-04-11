<properties
    pageTitle="Správa členů skupin v náhledu Azure Active Directory | Microsoft Azure"
    description="Jak uživatelům a zařízením, které jsou součástí skupiny v Azure Active Directory"
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


# <a name="manage-the-members-for-a-group-in-azure-active-directory-preview"></a>Správa členů skupin v náhledu Azure Active Directory

Tento článek vysvětluje, jak můžete spravovat členy skupiny v náhledu Azure Active Directory (Azure AD). [Novinky v náhledu](active-directory-preview-explainer.md)

## <a name="how-do-i-find-the-members-and-manage-them"></a>Jak najít členy a spravovat je?

1.  Přihlaste se k [Azure portál](https://portal.azure.com) pomocí účtu, který je jako globální správce pro adresář.

2.  Vyberte **Další služby**, zadejte do textového pole **Uživatelé a skupiny** a pak stiskněte **Enter**.

  ![Otevření Správa uživatelů](./media/active-directory-groups-members-azure-portal/search-user-management.png)

3.  Na zásuvné **Uživatelé a skupiny** vyberte **všechny skupiny**.

  ![Otevření zásuvné skupiny](./media/active-directory-groups-members-azure-portal/view-groups-blade.png)

4. Na zásuvné **Uživatelé a skupiny – všechny skupiny** vyberte skupinu.

5. Na *název *skupiny – *skupiny* ** zásuvné, vyberte **členy **.

  ![Otevření zásuvné členy](./media/active-directory-groups-members-azure-portal/view-group-members.png)

6. Přidání členů do skupiny na zásuvné **Group - členů** vyberte **Přidat členy**.

  ![Přidání příkazu členy](./media/active-directory-groups-members-azure-portal/add-group-members-command.png)

7. Na zásuvné **členů** vyberte jeden nebo více uživatelů nebo zařízení přidat do skupiny a potom vyberte tlačítko **Vyberte** v dolní části zásuvné si je přidat do skupiny. Pole pro **uživatelské** filtry zobrazení založené na odpovídající položku na libovolnou část jména uživatele nebo zařízení. Žádné zástupné znaky přijaté v tomto poli.

8. Odebrání členů ze skupiny na zásuvné **skupině: členové** vyberte členy.

9. Na zásuvné ***jméno*** vyberte příkaz **Odebrat** a potvrďte výběr příkazového řádku.

  ![odebrání příkazu členy](./media/active-directory-groups-members-azure-portal/remove-group-members-command.png)

9. Po dokončení změn členy skupiny, vyberte **Uložit**.


## <a name="additional-information"></a>Další informace

Tyto články poskytují další informace o Azure Active Directory.

* [Zobrazit existující skupiny](active-directory-groups-view-azure-portal.md)
* [Vytvoření nové skupiny a přidání členů](active-directory-groups-create-azure-portal.md)
* [Správa nastavení skupiny](active-directory-groups-settings-azure-portal.md)
* [Správa členství ve skupině](active-directory-groups-membership-azure-portal.md)
* [Správa dynamických pravidel pro uživatele ve skupině](active-directory-groups-dynamic-membership-azure-portal.md)
