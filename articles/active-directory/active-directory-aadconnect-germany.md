<properties
    pageTitle="Azure AD Connect v cloudu společnosti Microsoft Německo"
    description="Azure AD Connect integrace adresářů místní s Azure Active Directory. To umožňuje poskytovat běžné identity pro aplikace Office 365, Azure a SaaS integrovaný s Azure AD."
    keywords="Úvod k Azure AD Connect Azure AD Connect přehled, co je Azure AD Connect, nainstalujte službu active directory, Německo, černé struktury"
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/08/2016"
    ms.author="billmath"/>

#<a name="azure-ad-connect-in-microsoft-cloud-germany---public-preview"></a>Azure AD Connect v cloudu společnosti Microsoft Německo – Public Preview

## <a name="introduction"></a>Úvod
Azure AD Connect poskytuje synchronizace mezi místní služby Active Directory a Azure Active Directory.
V současné době v mnoha případech v [cloudu společnosti Microsoft Německo](https://www.microsoft.com/de-de/cloud/deutschland/default.aspx) musí udělat pomocí operátoru. Při použití Německo cloudu společnosti Microsoft, musí být vědom z následujících akcí:


- Následující adresy URL musí být otevřena na proxy serveru pro synchronizaci úspěšně:
    - *. microsoftonline.de
    - *. windows.net
    - + Seznamy odvolaných certifikátů

- Při přihlašování do adresáře Azure AD používají účet v doméně onmicrosoft.de.
- Tyto funkce nejsou k dispozici:
    - Azure AD Connect stavu
    - Automatické aktualizace
    - Heslo zpětného zápisu

## <a name="download"></a>Ke stažení
Azure AD Connect si můžete stáhnout z Azure AD Connect zásuvné v rámci portálu.  Vyhledání zásuvné Azure AD Connect pomocí níže uvedených pokynů.

### <a name="the-azure-ad-connect-blade"></a>Azure AD Connect zásuvné

Jakmile jste přihlášení k portálu Azure, postupujte takto:

1. Přejděte na Procházet
2.  Vyberte Azure Active Directory
3.  Vyberte Azure AD Connect

Měli byste vidět takto:

![Azure AD Connect zásuvné](media\active-directory-aadconnect-germany\germany1.png)

 
Následující tabulka popisuje funkce zobrazené v zásuvné.


Název|Popis|
----- | ----- |
STAV SYNCHRONIZACE|Pojďme víte, zda je povoleno synchronizace.|
POSLEDNÍ SYNCHRONIZACE|Posledního byla úspěšně dokončena úspěšné synchronizace.|
FEDEROVANÝCH DOMÉNÁCH|Zobrazí číslo federovaných doménách již nakonfigurovány.|


## <a name="installation"></a>Instalace
Pokud chcete nainstalovat Azure AD Connect, můžete použít v dokumentaci [tady](active-directory-aadconnect.md#install-azure-ad-connect).

## <a name="advanced-features-and-additional-information"></a>Pokročilé funkce a další informace
Další informace a pokyny ke vlastními nastaveními nebo rozšířené konfigurace začínají [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).  Tato stránka obsahuje informace a odkazy na další pokyny.
