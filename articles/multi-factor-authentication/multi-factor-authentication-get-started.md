<properties
    pageTitle="Azure MFA cloudu a server | Microsoft Azure"
    description="Zvolte vícefaktorové ověřování čelení řešení prstem doprava zobrazte je pomocí s dotazem, co jsem pokusu o zabezpečení a kde jsou moje uživatelé.  Klikněte na cloudové MFA Server nebo službu AD FS."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>

#<a name="choose-the-azure-multi-factor-authentication-solution-for-you"></a>Zvolte řešení Azure Vícefaktorové ověřování pro vás

Existuje několik provedeních z Azure Multi-Factor Authentication (MFA), jsme musíte odpovědět na pár otázek k určení prodejce, které je verze správné používat.  Jsou tyto otázky:

-   [Co mám pokusu o zabezpečení](#what-am-i-trying-to-secure)
-   [Kde se nacházejí uživatelů](#where-are-the-users-located)
- [Jaké funkce je potřeba?](#what-featured-do-i-need)

V následujících částech obsahují pokyny zjištění, každý z těchto odpovědi.

## <a name="what-am-i-trying-to-secure"></a>Co mám pokusu o zabezpečení?

Chcete-li zjistit řešení správné dvoustupňového ověření, nejdřív jsme musí odpovědět otázku, co by se pokusu o zabezpečení u druhého metody ověřování.  To je aplikace, která je v Azure?  Vzdálený systém nebo?  Určením co jsme se snažíte zabezpečené jsme odpověď otázku, kde Vícefaktorové ověřování musí být povolený.  


Co chcete zabezpečené| Vícefaktorové ověřování v cloudu|Vícefaktorové ověřování serveru
------------- | :-------------: | :-------------: |
Aplikace Microsoft první strany|● |● |
Aplikace SaaS v galerii aplikace|● |● |
Publikování aplikace služby IIS proxy server Azure AD aplikace|● |● |
Aplikace služby IIS nepublikované proxy server Azure AD aplikace | |● |
Vzdálený přístup například VPN, RDG| |● |



## <a name="where-are-the-users-located"></a>Kde se nacházejí uživatelů

Pak prohlížíte kde jsou umístěny uživatelích vám pomůže určit správné řešení použít v cloudu a místní pomocí serveru MFA.



Umístění uživatele| Vícefaktorové ověřování v cloudu|Vícefaktorové ověřování serveru
------------- | :-------------: | :-------------: |
Azure Active Directory|● | |
Azure AD a využití místního AD pomocí federace AD FS|● |● |
Azure AD a využití místního AD pomocí nástroje DirSync, Azure AD Sync, Azure AD Connect - žádné synchronizace hesel|● |● |
Azure AD a využití místního AD pomocí nástroje DirSync, Azure AD Sync, Azure AD Connect - synchronizaci hesel|● | |
Místní služby Active Directory| |● |

## <a name="what-features-do-i-need"></a>Jaké funkce je potřeba?

V následující tabulce je porovnání funkcí, které jsou dostupné s Vícefaktorové ověřování v cloudu a multi-Factor ověřování serveru.

 | Vícefaktorové ověřování v cloudu | Vícefaktorové ověřování serveru
------------- | :-------------: | :-------------: |
Mobilní aplikace oznámení jako druhý faktor | ● | ● |
Mobilní aplikace ověřovací kód jako druhý faktorem | ● | ●
Zavolat jako druhý faktor | ● | ●
Jednosměrný SMS jako druhý faktor | ● | ●
Mezi dvěma stranami SMS jako druhý faktorem |  | ●
Hardwarové tokeny jako druhý faktorem |  | ●
Aplikace hesla pro klienty, které nepodporují MFA | ● |  
Správce publikum nemůže ovládat metody ověřování | ● | ●
Režim PIN kódu |  | ●
Podvod upozornění | ● | ●
Sestavy MFA | ● | ●
Jednorázové přemostění |  | ●
Vlastní pozdrav pro telefonní hovory | ● | ●
ID volajícího přizpůsobitelné pro telefonní hovory | ● | ●
Důvěryhodné IP adresy | ● | ●
Mějte na paměti MFA důvěryhodných zařízení  | ● |  
Podmíněný přístup | ● | ●
Mezipaměti |  | ●

Teď můžeme usoudili, jestli používat cloudové vícefaktorové ověřování nebo serveru MFA místní, abychom mohli rovnou začít nastavením a použitím Azure Vícefaktorové ověřování. **Vyberte ikonu, která představuje nefunguje.**

<center>




[![Cloud](./media/multi-factor-authentication-get-started/cloud2.png)](multi-factor-authentication-get-started-cloud.md)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[![Proofup](./media/multi-factor-authentication-get-started/server2.png)](multi-factor-authentication-get-started-server.md)  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</center>
