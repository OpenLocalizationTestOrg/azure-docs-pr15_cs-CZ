<properties
   pageTitle="Jak udělit přístup k osobních | Microsoft Azure"
   description="Naučte se přidávat role pro uživatele s příponou Azure Active Directory privilegovaných Správa identit, aby mohl spravovat správce osobních informací."
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
   ms.date="09/22/2016"
   ms.author="kgremban"/>

# <a name="how-to-give-access-to-manage-azure-ad-privileged-identity-management"></a>Jak dát přístup ke správě Azure AD privilegovaných Správa identit

Globální správce, který umožňuje Azure AD privilegovaných Správa identit (osobních) pro organizaci automaticky získat přiřazování rolí a přístup k správce osobních informací. Nikdo jiný získá přístup pro zápis ve výchozím nastavení, včetně dalších globálních správců. Další globální správci, správci zabezpečení a zabezpečení čteček využívat jen pro čtení Azure AD správce osobních informací. Udělit přístup k správce osobních informací, první uživatel můžete ostatním uživatelům přiřadit roli **privilegovaných role správce** . Přiřazení musí provádět v osobních samotné a nelze změnit pomocí prostředí PowerShell nebo jiných portály.

> [AZURE.NOTE] Správa osobních Azure AD vyžaduje Azure MFA. Od účtů Microsoft nemůže registrovat pro Azure MFA, nemáte přístup ke uživatel přihlásí účtem Microsoft Azure AD správce osobních informací.

Ujistěte se, jsou vždy aspoň dva uživatelé v roli správce privilegovaných role v případě, že jeden uzamčen nebo odstraněný svůj účet.

## <a name="give-another-user-access-to-manage-pim"></a>Udělení přístupu jiného uživatele ke správě správce osobních informací

1. Přihlaste se k [portálu Azure](https://portal.azure.com/) a vyberte aplikaci **Azure AD privilegovaných Správa identit** na řídicím panelu.
2. Vyberte **Spravovat privilegovaných role** > **privilegovaných role správce** > **Přidat**.

    ![Přidání privilegovaných role správců - snímek][1]

4. Na zásuvné přidat spravovaných uživatelů už je dokončení kroku 1 Vyberte krok 2, **Vyberte uživatele** a vyhledejte uživatele, kterého chcete přidat.

    ![Vyberte uživatelé - snímek][2]

6. Ve výsledcích hledání vyberte uživatele a klikněte na tlačítko **Hotovo**.
7. Kliknutím na **OK** výběr uložíte. Uživatele, kterého jste vybrali se zobrazí v seznamu privilegovaných rolí správců.

    - Pokaždé, když novou roli přiřadit jinému, budou se automaticky nastaví poskytnout aktivovat roli. Pokud chcete převést na trvalé v roli, klikněte na jméno v seznamu. Vyberte **Zkontrolujte oprávnění** v nabídce uživatelské informace.

8. Odeslání odkazu uživatele na [Začínáme s Azure AD privilegovaných Správa identit](active-directory-privileged-identity-management-getting-started.md).


## <a name="remove-another-users-access-rights-for-managing-pim"></a>Odebrání jiný uživatel oprávnění pro správu Správce osobních informací

Před odebrání uživatele z role správce privilegovaných role vždy zkontrolujte, tam pořád bude dva uživatelé přiřazenou.

1. V řídicím panelu Správce osobních informací klikněte na roli **privilegovaných role správce**.  Zobrazí se seznam uživatelů v současné době určitou roli.
2. Klikněte na uživatele v seznamu uživatelů.
3. Klikněte na **Odebrat**.  Zobrazí se potvrzovací zpráva.
4. Kliknutím na tlačítko **Ano** odebrat uživatele z role.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Další kroky
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_add_PRA.png
[2]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_select_users.png
