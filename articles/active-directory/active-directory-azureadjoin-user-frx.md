<properties
    pageTitle="Nastavení nové zařízení s Azure AD během instalace | Microsoft Azure"
    description="Téma, které vysvětluje, jak můžou uživatelé nastavit připojení Azure AD během prvního spuštění možnosti uživatelů."
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

# <a name="set-up-a-new-device-with-azure-ad-during-setup"></a>Nastavení nové zařízení s Azure AD během instalace

Ve Windows 10 můžete připojit uživatelé jejich zařízení pro službu Azure Active Directory (Azure AD) v prvního spuštění (FRX). To umožňuje organizacím distribuce zatavené zařízení a jejich zaměstnanců a studenty, nebo automatické je zvolte svoje zařízení (CYOD).
Pokud edice Windows 10 Professional nebo Windows 10 Enterprise nainstalovaný na zařízení, na prostředí ve výchozím nastavení proces instalace pro vlastní společnosti zařízení.

## <a name="to-join-a-device-to-azure-ad"></a>Připojte zařízení k Azure AD


1. Při zapnutí na nové zařízení a zahájení procesu nastavení, byste měli vidět **Začíná připravení** zprávy. Postupujte podle pokynů k nastavení vašeho zařízení.
2. Začněte tím, že přizpůsobení oblast a jazyk. Potvrďte licenční podmínky pro Software společnosti Microsoft.
![Přizpůsobení pro vaši oblast](./media/active-directory-azureadjoin/active-directory-azureadjoin-customize-region.png)
3. Vyberte síť, kterou chcete použít pro připojení k Internetu.
4. Vyberte, jestli používáte osobní zařízení nebo vlastnictví společnosti. Je-li vlastnictví společnosti, klikněte na **Toto zařízení patří mé organizaci**. Spustí se prostředí Azure AD připojení. Následuje obrazovky, že se zobrazí, pokud používáte Windows 10 Professional.
<center>
![Kdo vlastní tento Vypnutou obrazovku počítače](./media/active-directory-azureadjoin/active-directory-azureadjoin-who-owns-pc.png)

5.  Zadejte přihlašovací údaje, které jsou pro vás poskytovanou vaší organizací.
<center>
![Přihlašovací obrazovka](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)
6.  Po zadání uživatelského jména do odpovídajícího klienta je umístěn v Azure AD. Pokud jste z federované domény, budete přesměrováni na místního serveru zabezpečené tokenu Service (Služba tokenů zabezpečení) – například Active Directory Federation Services (AD FS).
7. Pokud jste uživatel v nefederované doméně, zadejte svoje přihlašovací údaje přímo na stránce Azure AD hostované. Pokud společnost branding o konfiguraci, bude taky v tématu logem vaší organizace a podporují text.
8.  Zobrazí se výzva pro výzvu vícefaktorové ověřování. To je konfigurovat správce IT.
9.  Azure AD zkontroluje, zda tohoto uživatele a zařízení vyžaduje zápisu správy mobilních zařízení.
10. Systém Windows zaregistruje zařízení v adresáři organizace v Azure AD a zapsán v části Správa mobilních zařízení podle potřeby.
11. Pokud se uživatel spravované Windows přejdete na plochu procesem automatické přihlášení.
12. Pokud jste Federovaný uživatel, budete přesměrováni na přihlašovací obrazovce Windows zadejte svoje přihlašovací údaje.

> [AZURE.NOTE] Připojení místní služby Active Directory pro Windows Server domény ve verzi Windows mimo pole není podporovaná. Proto pokud nebudete chtít připojení počítače k doméně, by měl vyberete odkaz **Nastavení Windows pod svým účtem místní** místo. Jak už máte hotové před se můžete připojit potom domény z nastavení ve vašem počítači.

## <a name="additional-information"></a>Další informace
* [Windows 10 Enterprise: způsoby použití zařízení pro práci](active-directory-azureadjoin-windows10-devices-overview.md)
* [Rozšíření možností cloudu na zařízeních s Windows 10 až Azure Active Directory připojit se ke](active-directory-azureadjoin-user-upgrade.md)
* [Ověření identity bez hesla prostřednictvím Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Další informace o použití scénáře pro Azure AD připojení](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Připojte zařízení doméně Azure AD pro Windows 10 prostředí](active-directory-azureadjoin-devices-group-policy.md)
* [Nastavení připojení Azure AD](active-directory-azureadjoin-setup.md)
