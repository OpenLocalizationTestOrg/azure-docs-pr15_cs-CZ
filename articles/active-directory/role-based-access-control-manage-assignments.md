<properties
    pageTitle="Zobrazení přiřazení přístup Azure zdrojů | Microsoft Azure"
    description="Zobrazení a správa přiřazení na základě rolí řízení přístupu uživatele nebo skupinu v portálu Azure"
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="jeffsta"/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="view-access-assignments-for-users-and-groups-in-the-azure-portal---public-preview"></a>Zobrazení přiřazení přístup uživatelů a skupin v portálu Azure - Public preview

> [AZURE.SELECTOR]
- [Správa přístupu uživatele nebo skupiny](role-based-access-control-manage-assignments.md)
- [Správa přístupu podle zdroje](role-based-access-control-configure.md)

S na základě rolí přístup ovládacího prvku (RBAC) v náhledu Azure Active Directory, se dá řídit přístup k prostředkům Azure. [Novinky v náhledu](active-directory-preview-explainer.md)

Access přiřazený RBAC totiž jemně odstupňovaná je možné omezit oprávnění dvěma způsoby:

- **Obor:** Přiřazení rolí RBAC je omezené konkrétní předplatného, skupina zdroje nebo zdroje. Uživateli poskytnout přístup k jeden zdroj nemáte přístup ke dalších zdrojů v rámci stejného předplatného.
- **Role:** V rozsahu přiřazení přístup zúžit i další tak, že přiřazení role. Role může být nejvyšší úrovně, jako je vlastník nebo konkrétní jako virtuální počítač Readeru.

Role můžete přiřadit pouze z v rámci předplatného, skupina zdroje nebo zdroj, který je obor pro přiřazení. Ale zobrazení přiřazení přístup pro daného uživatele nebo skupiny na jednom místě.

Získáte další informace o tom, jak [používat přiřazování rolí Správa přístupu k prostředkům Azure předplatného](role-based-access-control-configure.md).

##  <a name="view-access-assignments"></a>Zobrazení aplikace access přiřazení

Chcete-li vyhledat přiřazeních aplikace access pro jednoho uživatele nebo skupinu, spusťte v Azure Active Directory [Azure portálu](http://portal.azure.com).

1. Vyberte **Azure Active Directory**. Pokud tato možnost není vidět ve vašem seznamu navigace, vyberte **Další služby** a posuňte se dolů k vyhledání **Azure Active Directory**.
2. Vyberte **Uživatelé a skupiny**a pak buď **všem uživatelům** nebo **všechny skupiny**. V tomto příkladu jsme zaměřit na jednotlivé uživatele.
    ![Správa uživatelů a skupin služby Azure Active Directory - snímek](./media/role-based-access-control-manage-assignments/rbac_users_groups.png)
3. Hledání podle jména nebo uživatelské jméno uživatele.
4. Vyberte **Azure zdroje** na zásuvné uživatele. Zobrazí všechny přiřazeních aplikace access pro daného uživatele.

### <a name="read-permissions-to-view-assignments"></a>Oprávnění k zobrazení přiřazení jen pro čtení

Tato stránka zobrazuje pouze přiřazení přístup, ke kterým máte oprávnění ke čtení. Například máte přístup pro čtení k předplatnému A a přejděte na zásuvné Azure materiály ke kontrole přiřazení uživatele. Můžete zobrazit své přiřazení přístup pro předplatné A, ale nezobrazí také, že kontakt je přístup přiřazení předplatného B.

## <a name="delete-access-assignments"></a>Odstranění přiřazeních aplikace access

Z tohoto zásuvné můžete odstranit přístup přiřazení, které byly přiřazeny přímo do uživatele nebo skupiny. Pokud přístup přiřazení byl dědí od nadřazené skupiny, musíte přejít ke zdroji nebo předplatného a spravovat daného přiřazení.

1. V seznamu přiřazení přístupu uživatele nebo skupiny vyberte tu, kterou chcete odstranit.
2. Vyberte **Odebrat** a potom **Ano** pro potvrzení.
    ![Odebrání přístup přiřazení - snímek](./media/role-based-access-control-manage-assignments/delete_assignment.png)

## <a name="related-topics"></a>Příbuzná témata

- Začínáme s na základě rolí řízení přístupu k [použití přiřazování rolí Správa přístupu k prostředkům Azure předplatného](role-based-access-control-configure.md)
- V tématu [RBAC předdefinované role](role-based-access-built-in-roles.md)
