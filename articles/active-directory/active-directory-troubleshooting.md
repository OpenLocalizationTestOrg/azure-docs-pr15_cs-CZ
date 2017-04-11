<properties
   pageTitle="Řešení potíží: "Active Directory" je položka chybějící nebo není k dispozici | Microsoft Azure "
   description="Co dělat, když se nezobrazuje na portálu Správa Azure položka nabídky služby Active Directory."
   services="active-directory"
   documentationCenter="na"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="troubleshooting-active-directory-item-is-missing-or-not-available"></a>Řešení potíží: "Active Directory" je položka chybějící nebo není k dispozici

V mnoha pokyny pro používání funkce Azure Active Directory a služby začínají na "Přejděte na portál správy Azure a klikněte na **služby Active Directory**." Ale co můžete dělat, pokud není zobrazena Active Directory linky nebo nabídky položky nebo pokud je označený **Není k dispozici**? Toto téma usnadňuje. Popisuje podmínky, za jakých **Služby Active Directory** nezobrazí nebo není k dispozici a vysvětluje, jak pokračovat.

## <a name="active-directory-is-missing"></a>Chybí služby Active Directory

Obvykle **Služby Active Directory** položky se zobrazí v levé navigační nabídky. Pokyny v Azure Active Directory postupy se předpokládá, že je tato položka v zobrazení.

![Snímek obrazovky: služby Active Directory v Azure](./media/active-directory-troubleshooting/typical-view.png)

Položky služby Active Directory se zobrazí v levé navigační nabídky, když platí některá z následujících podmínek. V opačném položku nezobrazí.

* Aktuálního uživatele přihlášení pod svým účtem Microsoft (dříve Windows Live ID).

    NEBO

* Azure klienta má adresář a aktuální účet správce adresář.

    NEBO

* Azure klienta má obor názvů alespoň jeden řízení přístupu Azure AD (ACS). Další informace najdete v tématu [Namespace řízení přístupu](https://msdn.microsoft.com/library/azure/gg185908.aspx).

    NEBO

* Azure klienta obsahuje alespoň jeden Azure Vícefaktorové ověřování poskytovatele. Další informace najdete v tématu [Správa zprostředkovatelů ověřování Multi-Factor Azure](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

Pokud chcete vytvořit Vícefaktorové ověřování poskytovatele nebo názvů řízení přístupu, klikněte na **+ Nový** > **Aplikace služby** > **Služby Active Directory**.

Získat oprávnění správce v adresáři, požádejte správce, přiřadit roli správce ke svému účtu. Podrobnosti najdete v tématu [přiřazení rolí správce](active-directory-assign-admin-roles.md).

## <a name="active-directory-is-not-available"></a>Služba Active Directory není k dispozici

Po klepnutí na tlačítko **+ Nový** > **Aplikace služby** **Active Directory** položka se zobrazí. Konkrétně položku služby Active Directory zobrazíte funkcí služby Active Directory, například adresáře, řízení přístupu nebo poskytovatele Auth Multi-Factor, budou k dispozici aktuálního uživatele.

Však při načítání stránky na položku není k dispozici a je označený **Není k dispozici**. Toto je stavu dočasný. Pokud budete chtít čekat celých několik sekund, než, bude k dispozici na položku. Pokud prodloužit dobu často aktualizaci na webovou stránku řeší potíže.

![Snímek obrazovky: Služba Active Directory není k dispozici](./media/active-directory-troubleshooting/not-available.png)
