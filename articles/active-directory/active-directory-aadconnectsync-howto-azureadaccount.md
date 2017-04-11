<properties
    pageTitle="Azure AD Connect synchronizace: Správa účtu služby Azure AD | Microsoft Azure"
    description="Toto téma popisuje způsob obnovení účtu služby Azure AD."
    services="active-directory"
    keywords="AADSTS70002, AADSTS50054, jak resetovat heslo pro synchronizaci Azure AD Connect účet služby spojnice"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-sync-how-to-manage-the-azure-ad-service-account"></a>Azure AD Connect synchronizace: Správa účtu služby Azure AD
Účet služby používaný konektoru Azure AD by mělo být bezplatné služby. Pokud potřebujete resetovat své přihlašovací údaje, toto téma je za vás. Například pokud globální správce má k dispozici omylem resetování hesla na účet služby pomocí Powershellu.

## <a name="reset-the-credentials"></a>Obnovení pověření
Pokud účet služby definované na spojnici Azure AD nelze kontaktovat Azure AD kvůli problémům ověřování, můžete resetování hesel.

1. Přihlaste se k synchronizaci server Azure AD Connect a začněte Powershellu.
2. Spuštění `Add-ADSyncAADServiceAccount`.  
![Addadsyncaadserviceaccount rutiny prostředí PowerShell](./media/active-directory-aadconnectsync-howto-azureadaccount/addadsyncaadserviceaccount.png)
3. Zadejte přihlašovací údaje Azure AD globálního správce.

Tato rutina obnoví heslo k účtu služby a aktualizace v Azure AD i v modulu synchronizace.

## <a name="known-issues-these-steps-can-solve"></a>Známé problémy můžete vyřešit takto
Tento oddíl je seznam chyb zákazníci opravené pomocí přihlašovacích údajů obnovit na účtu služby Azure AD.

-----------
Událost 6900  
Server došlo k neočekávané chybě při zpracování upozornění na změnu hesla:  
AADSTS70002: Chyby ověření přihlašovací údaje. AADSTS50054: Staré heslo slouží k ověření.

----------
Událost 659  
Chyba při načítání konfigurace synchronizace zásad heslo. Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException:  
AADSTS70002: Chyby ověření přihlašovací údaje. AADSTS50054: Staré heslo slouží k ověření.

## <a name="next-steps"></a>Další kroky

**Přehled témat**

- [Azure AD Connect synchronizace: princip a vlastní nastavení synchronizace](active-directory-aadconnectsync-whatis.md)
- [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md)
