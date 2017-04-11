<properties
    pageTitle="Nastavení připojení Azure AD pro vaši uživatelé | Microsoft Azure"
    description="Vysvětluje, jak správci můžou nastavit Azure AD připojení místního adresáře a registrace zařízení."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="setting-up-azure-ad-join-in-your-organization"></a>Nastavení připojení Azure AD ve vaší organizaci

Než nastavíte Azure Active Directory připojení (připojit se ke Azure AD), budete muset synchronizovat přes místního adresáře uživatelů do cloudu nebo ručně vytvořit spravované účty v Azure AD.

Podrobné pokyny k synchronizaci místních uživatelům Azure AD jsou obsaženy v [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).


Ruční vytvoření a Správa uživatelů v Azure AD najdete v nápovědě k [Správa uživatelů v Azure AD](https://msdn.microsoft.com/library/azure/hh967609.aspx).

## <a name="set-up-device-registration"></a>Nastavení registrace zařízení
1. Přihlaste se k portálu Azure jako správce.
2. V levém podokně vyberte **Služby Active Directory**.
3. Na kartě **adresáře** vyberte adresář.
4. Vyberte kartu **Konfigurovat** .
5. Přejděte do části **zařízení** .
6. Na kartě **zařízení** nastavte takto:  
   * **Maximální počet ze zařízení uživatele**: Vyberte maximálního počtu zařízení, které uživatel může mít v Azure AD.  Pokud uživatel dosáhne této kvóty, nebudou moct přidat další zařízení, dokud jednu nebo víc z existujících zařízeních se odeberou.
   * **Vyžadovat AUTH Multi-FACTOR na zařízení spojení**: nastavte, zda uživatelé, kteří jsou musí poskytovat druhý faktor ověřování ke spojení svého zařízení a Azure AD. Další informace o Azure Vícefaktorové ověřování najdete v článku [Začínáme s Azure Vícefaktorové ověřování v cloudu](..\multi-factor-authentication\multi-factor-authentication-get-started-cloud.md).
   * **Uživatelé MŮŽOU AZURE AD spojení zařízení**: Vyberte uživatelé a skupiny, které jsou povoleny ke spojení zařízení a Azure AD.
   * **Další správci dál AZURE AD PŘIPOJEN zařízení**: Azure AD Premium nebo sady mobilita Enterprise (EMS), můžete zvolit určující, kteří uživatelé mít příslušná oprávnění místního správce zařízení. Globální správci a vlastníci zařízení mít příslušná oprávnění místního správce ve výchozím nastavení.

<center>![Nastavení zařízení regisration](./media/active-directory-azureadjoin/active-directory-aadjoin-configure-devices.png)</center>

Po nastavení připojení Azure AD pro uživatele, si můžou připojit k Azure AD pomocí své zařízení podnikové nebo pro jednotlivce.

Tady jsou tři scénáře, které můžete použít k povolení uživatelům nastavení připojení Azure AD:

- Uživatelé se vlastnictví společnosti zařízení připojit přímo do Azure AD.
- Uživatelé-připojení k doméně vlastnictví společnosti zařízení na adresářová služba Active Directory a potom rozšířit zařízení Azure AD.
- Uživatelé přidávat pracovní nebo školní účty systému Windows na osobním zařízením

## <a name="additional-information"></a>Další informace
* [Windows 10 Enterprise: způsoby použití zařízení pro práci](active-directory-azureadjoin-windows10-devices-overview.md)
* [Rozšíření možností cloudu na zařízeních s Windows 10 až Azure Active Directory připojit se ke](active-directory-azureadjoin-user-upgrade.md)
* [Další informace o použití scénáře pro Azure AD připojení](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Připojte zařízení doméně Azure AD pro Windows 10 prostředí](active-directory-azureadjoin-devices-group-policy.md)
* [Nastavení připojení Azure AD](active-directory-azureadjoin-setup.md)
