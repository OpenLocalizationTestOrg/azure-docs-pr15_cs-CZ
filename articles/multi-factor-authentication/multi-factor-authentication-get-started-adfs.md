<properties
    pageTitle="Azure MFA a AD FS | Microsoft Azure"
    description="Tohle je stránka Azure Multi-Factor ověřování, který popisuje, jak začít s Azure MFA a služby AD FS."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na" ms.topic="get-started-article"
    ms.date="10/17/2016"
    ms.author="kgremban"/>

# <a name="getting-started-with-azure-multi-factor-authentication-and-active-directory-federation-services"></a>Začínáme s Azure Vícefaktorové ověřování a Active Directory Federation Services



<center>![Cloud](./media/multi-factor-authentication-get-started-adfs/adfs.png)</center>

Pokud vaše organizace má federované vaší místní služby Active Directory s Azure Active Directory pomocí služby AD FS, existují dva způsoby použití Azure Vícefaktorové ověřování.

- Zabezpečení prostředků cloudu pomocí Azure Vícefaktorové ověřování nebo Active Directory Federation Services
- Zabezpečené cloudu a místní zdrojů pomocí serveru Multi-Factor Authentication Azure

Následující tabulka shrnuje možnosti ověření mezi zabezpečení zdroje s Azure Vícefaktorové ověřování a služby AD FS

|Ověření možnosti – Procházet aplikacemi|Ověření možnosti – bez webovými aplikacemi
:------------- | :------------- | :------------- |
Zabezpečení prostředků Azure AD pomocí Azure Vícefaktorové ověřování |<li>Cílem prvního kroku ověření provádí místních pomocí služby AD FS.</li> <li>Druhým krokem je metodu telefonních konstantě cloudu ověřování.</li>|Koncoví uživatelé můžete použít aplikaci hesla k přihlášení.
Zabezpečení prostředků Azure AD pomocí Active Directory Federation Services |<li>Cílem prvního kroku ověření provádí místních pomocí služby AD FS.</li><li>Druhým krokem je provedena místního ctít zásady deklaraci.</li>|Koncoví uživatelé můžete použít aplikaci hesla k přihlášení.

Upozornění s aplikací hesel pro federovaní uživatelé:

- Aplikace hesla ověření pomocí ověřování cloudu, aby nepoužili federace. Federace pouze aktivně používá při nastavování heslo k aplikaci.
- Místní nastavení řízení přístupu klienta nejsou dodržet tak, že aplikace hesla.
- Dojde ke ztrátě místní funkce protokolování ověřování aplikace hesel.
- Zakázání nebo odstranění účtu může trvat až tři dobu synchronizace adresáře, zpoždění zakázání nebo odstranění hesel aplikace v cloudu identity.

## <a name="next-steps"></a>Další kroky

Další informace o nastavení Azure Vícefaktorové ověřování nebo Server Azure Multi-Factor Authentication se službou AD FS naleznete v následujících článcích:

- [Zabezpečené cloudové zdrojů prostřednictvím Azure Vícefaktorové ověřování a služby AD FS](multi-factor-authentication-get-started-adfs-cloud.md)
- [Zabezpečené cloudové a místních zdrojů pomocí Azure Multi-Factor ověřování serveru v systému Windows Server 2012 R2 AD FS](multi-factor-authentication-get-started-adfs-w2k12.md)
- [Zabezpečené cloudové a místních zdrojů prostřednictvím Azure Multi-Factor ověřování serveru se službou AD FS 2.0](multi-factor-authentication-get-started-adfs-adfs2.md)
