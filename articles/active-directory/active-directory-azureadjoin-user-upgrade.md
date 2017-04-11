<properties
    pageTitle="Nastavení zařízení s Windows 10 s Azure AD ze stránky nastavení | Microsoft Azure"
    description="Vysvětluje, jak uživatelé se můžou připojit k Azure AD pomocí nabídky nastavení."
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

# <a name="set-up-a-windows-10-device-with-azure-ad-from-settings"></a>Nastavení zařízení s Windows 10 s Azure AD ze stránky nastavení
Pokud už používáte Windows 7 nebo Windows 8 a počítače nebo zařízení dochází k upgradu na Windows 10, se můžete připojit ke službě Azure Active Directory (Azure AD) z nabídky nastavení.

## <a name="to-join-to-azure-ad-from-the-settings-menu"></a>Pokud chcete připojit ke Azure AD v nabídce nastavení


1. V nabídce **Start** klikněte na ovládací tlačítko **Nastavení** .
2. V **Nastavení**vyberte **systém**->**o**->**připojení Azure AD**.
<center>
![Připojit se ke Azure AD z nabídky nastavení](./media/active-directory-azureadjoin/active-directory-azureadjoin-settings.png)</center>

3. Kliknutím na **pokračovat** v okně Připojit se ke Azure AD zprávy.
<center>
![Okno zprávy spojení Azure AD](./media/active-directory-azureadjoin/active-directory-azureadjoin-message.png)</center>
4. Zadejte přihlašovací údaje. Tento přihlášení bude obsahovat všechny potřebné kroky, které jsou potřeba k dokončení ověřování. Pokud si nejste součástí federované klienta, správce vám poskytne prostředí federace, který je hostovaný ve vaší organizaci.
<center>
![Zadejte přihlašovací údaje](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)</center>
5. Pokud vaše organizace nakonfiguroval Azure Vícefaktorové ověřování pro připojení ke Azure AD, zadejte faktor druhé před pokračováním.
6. V dialogovém okně **Povolit zařízení spravovat** klikněte na **přijmout** .
7. Zobrazí se zpráva "zařízení teď připojen k vaší organizace v Azure AD".


## <a name="additional-information"></a>Další informace
* [Další informace o použití scénáře pro Azure AD připojení](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Připojte zařízení doméně Azure AD pro Windows 10 prostředí](active-directory-azureadjoin-devices-group-policy.md)
* [Nastavení připojení Azure AD](active-directory-azureadjoin-setup.md)
* [Ověření identity bez hesla prostřednictvím Microsoft Passport](active-directory-azureadjoin-passport.md)
