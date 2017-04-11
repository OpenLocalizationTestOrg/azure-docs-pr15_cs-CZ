

<properties
    pageTitle="Připojit se ke osobním zařízením pro vaši organizaci | Microsoft Azure"
    description="Vysvětluje, jak uživatelé můžou zaregistrovat své osobní zařízeních s Windows 10 k podnikové síti a obsahuje kroky nasazení pro situace BYOD."
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
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="join-a-personal-device-to-your-organization"></a>Připojit se ke osobním zařízením pro vaši organizaci

## <a name="to-join-a-windows-10-device-to-your-organization"></a>Připojení zařízení s Windows 10 pro vaši organizaci

1.  V nabídce **Start** klikněte na **Nastavení**.
2.  Vyberte **účty**a potom klepněte na **svůj účet**.
3.  Klikněte na **Přidat pracovního nebo školního účtu**a zadejte do účtu vaší organizace.
4.  Na přihlašovací stránku pro vaši organizaci zadejte svoje uživatelské jméno a heslo a klikněte na tlačítko **OK**.
5.  Zobrazí se výzva pro výzvu vícefaktorové ověřování. (Tento ověřovací kód programu je, která dokáže nahradit správce IT.)
6.  Azure Active Directory (Azure AD) zkontroluje, jestli zařízení vyžaduje zápis správy mobilních zařízení.
7.  Systém Windows zaregistruje zařízení v adresáři organizace v Azure AD a zapsán v části Správa mobilních zařízení podle potřeby.
8.  Pokud se uživatel spravované Windows vás provede na plochu automatické přihlášení.
9.  Pokud jste Federovaný uživatel, přejdete do Windows přihlašovací obrazovka k zadání přihlašovacích údajů.

## <a name="additional-information"></a>Další informace
* [Windows 10 Enterprise: způsoby použití zařízení pro práci](active-directory-azureadjoin-windows10-devices-overview.md)
* [Rozšíření možností cloudu na zařízeních s Windows 10 až Azure Active Directory připojit se ke](active-directory-azureadjoin-user-upgrade.md)
* [Ověření identity bez hesla prostřednictvím Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Další informace o použití scénáře pro Azure AD připojení](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Připojte zařízení doméně Azure AD pro Windows 10 prostředí](active-directory-azureadjoin-devices-group-policy.md)
* [Nastavení připojení Azure AD](active-directory-azureadjoin-setup.md)
