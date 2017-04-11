<properties
    pageTitle="Azure Active Directory hybridní identity navrhování - určit požadavky synchronizace adresářů | Microsoft Azure"
    description="Zjistěte, jaké požadavky jsou potřebné pro synchronizaci všech uživatelů mezi dne = místní organizaci a cloudu pro organizace."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="determine-directory-synchronization-requirements"></a>Určení požadavky synchronizace adresářů
Synchronizace je celé o tom uživatelům identitu v cloudu na základě jejich místní identity. Též synchronizované účtu použije pro ověření nebo federované, uživatelé budou pořád třeba identitu v cloudu.  Tato identita muset udržovat a pravidelně aktualizovat.  Aktualizace může trvat lišit od změny názvu pro změnu hesla.  

Začněte tím, že vyhodnocení organizace místních identit řešení a uživatel požadavky. Toto hodnocení je třeba definovat technické požadavky na tom, jak vytvořit a spravovat v cloudu identity uživatele.  Pro většinu organizací služba Active Directory je místní a budou místního adresáře, která budou uživatelé používat tak, že synchronizovat z, ale v některých případech to nebude v případě.  

Ujistěte se, odpovězte na následující otázky:


- Máte jednu doménové AD násobek nebo žádný?
 - Kolik adresářů Azure AD bude můžete být synchronizace?
 
    1. Používáte filtrování?
    2. Máte k dispozici více serverů Azure AD Connect plánované?
  
- Nyní máte synchronizace nástroje místní?
  - Pokud ano, pokud mají uživatelé virtuální/integrace služby directory identit znamená vaši uživatelé?
- Máte k dispozici žádné další adresář místně, které chcete synchronizovat (například adresář LDAP, HR databáze a další)?
  - Chystáte se by všechny GALSync?
  - Co je aktuální stav UPN ve vaší organizaci? 
  - Máte jinou adresář, který ověřování uživatelů?
  - Má ve vaší organizaci používat Microsoft Exchange?
    - Tyto plán s hybridního nasazení exchange?

Teď, když máte představu o požadavcích na synchronizaci, budete muset zjistit, jaký nástroj je ten správný splňovat tyto požadavky.  Microsoft poskytuje několik nástrojů provádět integrace adresářů a synchronizace.  Podívejte se na [hybridní Identity adresáře integrace nástroje porovnávací tabulku](active-directory-hybrid-identity-design-considerations-tools-comparison.md) Další informace. 
   
Teď, když máte vašim požadavkům pro synchronizaci a nástroj dosáhnete ve svojí společnosti, budete muset zjistit aplikací, které používají tyto adresářovými službami. Toto hodnocení je třeba definovat technické požadavky integrovat tyto aplikace do cloudu. Ujistěte se, odpovězte na následující otázky:

- Tyto aplikace přesune do cloudu a použití adresáře?
- Nejdou nějaké zvláštní atributy, které je potřeba synchronizované do cloudu, můžete tyto aplikace používají úspěšně?
- Tyto aplikace muset být znovu došlo k zápisu využívat cloud auth?
- Tyto aplikace, zůstanou live místní během uživatelů k nim prostřednictvím identity cloudu?

Potřebujete zjistit zabezpečení požadavky a omezení synchronizace adresářů. Toto hodnocení je důležité, abyste získali přehled o požadavky, které je potřeba k vytvoření a správa identity uživatele v cloudu. Ujistěte se, odpovězte na následující otázky:

- Kde bude server synchronizace umístěn?
- Bude domény připojen?
- Bude nacházet serveru v síti s omezeným přístupem za bránou firewall, například DMZ?
  - Se budou moct otevřít porty požadované brány firewall pro podporu synchronizace?
- Máte plán havárie obnovy pro server synchronizace?
- Máte účet s správná oprávnění pro všechny strukturami, že které chcete synchronizovat s?
 - Pokud vaše společnost poslal, neví odpověď pro tuto otázku, přečtěte si část "Oprávnění pro synchronizaci hesel" v článku [instalace služba Azure Active Directory Sync](https://msdn.microsoft.com/library/azure/dn757602.aspx#BKMK_CreateAnADAccountForTheSyncService) a zjistit, jestli už máte účet u těchto oprávnění nebo v případě potřeby ho vytvořit.
- Pokud máte vícenásobného doménové je synchronizace serveru synchronizace dostali na jednotlivé domény?
 
>[AZURE.NOTE]
Ujistěte se, pořizovat poznámky každá odpověď a interpretaci důvodech odpověď. [Určení incidentu požadavky](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) přejde myší dostupné možnosti. Tak, že máte zodpovězené otázek, kterou vybereme kterou nejlepší možnost se hodí vyhovovaly potřebuje.

## <a name="next-steps"></a>Další kroky
[Určit požadavky vícefaktorové ověřování](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="see-also"></a>Viz taky
[Přehled aspektech návrhu](active-directory-hybrid-identity-design-considerations-overview.md)
