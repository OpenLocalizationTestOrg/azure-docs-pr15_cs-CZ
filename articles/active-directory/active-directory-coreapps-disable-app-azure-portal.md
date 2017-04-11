<properties
    pageTitle="Zakázání přihlášení uživatele podnikové aplikace ve verzi preview Azure Active Directory pro | Microsoft Azure"
    description="Jak zakázat podnikové aplikaci tak, že žádní uživatelé mohou Přihlaste se k němu v Azure Active Directory"
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


# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory-preview"></a>Zakázání přihlášení uživatele aplikace enterprise v náhledu Azure Active Directory

Není těžké si zakázat podnikové aplikaci tak, že žádní uživatelé mohou Přihlaste se k němu v náhledu Azure Active Directory (Azure AD). [Novinky v náhledu](active-directory-preview-explainer.md) Máte potřebná oprávnění ke správě aplikaci organizace. V aktuálním náhledu musí být globální správce pro adresář.

## <a name="how-do-i-disable-user-sign-ins"></a>Jak zakázat přihlášení uživatele?

1. Přihlaste se k [Azure portál](https://portal.azure.com) pomocí účtu, který je jako globální správce pro adresář.

2. Vyberte **Další služby**, zadejte **Azure Active Directory** do textového pole a potom stiskněte **Enter**.

3. Na * *Azure Active Directory - *directoryname* ** zásuvné (to znamená Azure AD zásuvné adresáře, které spravujete), vyberte **Enterprise aplikací **.

    ![Otevření podnikových aplikací](./media/active-directory-coreapps-disable-app-azure-portal/open-enterprise-apps.png)

4. Na zásuvné **podnikových aplikací** vyberte **všechny aplikace**. Zobrazí se seznam aplikace, které můžete spravovat.

5. Na zásuvné **podnikových aplikací – všechny aplikace** vyberte aplikace.

6. Na zásuvné ***název_aplikace*** (to znamená zásuvné s názvem aplikaci vybrané v názvu) vyberte **Vlastnosti**.

    ![Výběr příkazu všechny aplikace](./media/active-directory-coreapps-disable-app-azure-portal/select-app.png)

7. Na ***název_aplikace*** **– Vlastnosti** zásuvné, vyberte **Ne** pro **pro uživatele k přihlášení?**.

8. Výběr příkazu **Uložit** .

## <a name="next-steps"></a>Další kroky

- [Zobrazit všechny skupiny](active-directory-groups-view-azure-portal.md)
- [Přiřadit aplikace enterprise uživatele nebo skupiny](active-directory-coreapps-assign-user-azure-portal.md)
- [Odebrat uživatele nebo skupinu přiřazení z aplikace enterprise](active-directory-coreapps-remove-assignment-azure-portal.md)
- [Změna názvu a loga aplikace enterprise](active-directory-coreapps-change-app-logo-user-azure-portal.md)
