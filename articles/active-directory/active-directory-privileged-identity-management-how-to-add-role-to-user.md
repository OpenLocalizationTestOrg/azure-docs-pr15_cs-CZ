<properties
   pageTitle="Jak přidat nebo odebrat role uživatele | Microsoft Azure"
   description="Naučte se přidávat role privilegovaných identit s Azure Active Directory privilegovaných Správa identit aplikací."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/24/2016"
   ms.author="kgremban"/>

# <a name="azure-ad-privileged-identity-management-how-to-add-or-remove-a-user-role"></a>Azure AD privilegovaných Správa identit: Jak pokud chcete přidat nebo odebrat role uživatele

S Azure Active Directory (AD), globální správce (nebo company Administrators) můžete aktualizovat určující, kteří uživatelé jsou **trvale** přiřazená k rolím v Azure AD. To lze provést pomocí rutin prostředí PowerShell jako `Add-MsolRoleMember` a `Remove-MsolRoleMember`. Nebo můžou použít portálu Azure klasické způsobem popsaným v článku [přiřazení rolí správce v Azure Active Directory](active-directory-assign-admin-roles.md).

Aplikace Azure AD privilegovaných správy identit umožňuje správcům privilegovaných role provést trvalé přiřazení rolí stejně. Kromě toho privilegovaných role Správci uživatelé **oprávněn** pro rolí správců. Možný správce můžete aktivovat roli, když potřebují, a potom jejich datum vypršení platnosti oprávnění až budete mít.

## <a name="manage-roles-with-pim-in-the-azure-portal"></a>Spravovat role s správce osobních informací v portálu Azure

Ve vaší organizaci můžete přiřadit uživatelům různých správní role v Azure AD, Office 365 a další služby a aplikace.  Další informace o dostupných rolí najdete na [role v Azure AD správce osobních informací](active-directory-privileged-identity-management-roles.md).

Přidání nebo odebrání uživatele v roli pomocí privilegovaných správy identit, otevřete okno řídicím panelu Správce osobních informací. Potom klikněte na tlačítko **uživatelů v roli správce** , nebo vyberte konkrétní rolí (například globální správce) z tabulky role.

> [AZURE.NOTE] Pokud jste nepovolili správce osobních informací v portálu Azure ještě, přejděte na [Začínáme s Azure AD osobních](active-directory-privileged-identity-management-getting-started.md) podrobnosti.

Pokud chcete dát jiný uživatel přístup k osobních samotné, role, které správce osobních informací vyžaduje uživatel měl jsou popsány dále v [tom, jak udělit přístup správce osobních informací](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="add-a-user-to-a-role"></a>Přidání uživatele do role

1. [Azure portál](https://portal.azure.com/)vyberte dlaždici **Azure AD privilegovaných Správa identit** na řídicím panelu.
2. Vyberte **Spravovat privilegovaných role**.
3. V tabulce **Role souhrnné** vyberte roli, kterou si chcete spravovat.
4. V roli zásuvné vyberte **Přidat**.
5. Klikněte na **Vybrat uživatele** a hledat uživatele na zásuvné **Vyberte uživatelé** .  
6. Vyberte uživatele ze seznamu výsledků hledání a klikněte na tlačítko **Hotovo**.
4. Kliknutím na **OK** výběr uložíte. Uživatele, kterého jste vybrali se zobrazí v seznamu jako nárok na roli.

> [AZURE.NOTE]
>Nových uživatelů v roli jsou jenom poskytnout role ve výchozím nastavení. Pokud budete chtít natrvalo roli, klikněte na jméno v seznamu. Informace o uživateli se zobrazí v nové zásuvné. Vyberte **Zkontrolujte oprávnění** v nabídce uživatelské informace.  
>Pokud uživatel nemůže registrovat pro Azure Multi-Factor Authentication (MFA) nebo se pomocí účtu Microsoft (obvykle @outlook.com), musíte provést trvalé jejich role. Možný správci se zobrazí dotaz, registrace pro MFA při aktivaci.

Teď, když uživatel je oprávněn pro roli, dejte mu to vědět, že budou můžete aktivovat podle pokynů v tématu [jak aktivovat nebo deaktivovat roli](active-directory-privileged-identity-management-how-to-activate-role.md).

## <a name="remove-a-user-from-a-role"></a>Odebrání uživatele z role

Odebrání uživatelů ze měl rolemi, ale zkontrolujte, jestli je vždy aspoň jeden uživatel trvalé globálního správce.

Tímto postupem odebrat určitého uživatele z role:

1. Přejděte k roli v seznamu role výběrem role v řídicím panelu Správce osobních informací Azure AD nebo kliknutím na tlačítko **uživatelů v rolí správců** .
2. Klikněte na uživatele v seznamu uživatelů.
3. Klikněte na tlačítko **Odebrat**. Zpráva vás vyzve k potvrzení.
4. Kliknutím na tlačítko **Ano** odebrat roli uživatele.

Pokud si nejste jistí, které uživatelé musí jejich přiřazování rolí, potom můžete [začít revize přístup pro roli](active-directory-privileged-identity-management-how-to-start-security-review.md).


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Další kroky
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
