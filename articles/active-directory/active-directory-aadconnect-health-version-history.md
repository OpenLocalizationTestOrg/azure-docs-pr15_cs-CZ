<properties
    pageTitle="Azure AD Connect historie verzí stavu"
    description="Tento dokument popisuje uvolnění Azure AD připojení zdraví a co je součástí těchto verzí."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="azure-ad-connect-health-version-release-history"></a>Azure AD Connect stavu: Historie verzí verzi

Týmu Azure Active Directory se aktualizuje pravidelně stavu připojení Azure AD s novými funkcemi a funkce. Tento článek uvádí verzí a funkce, které byl vydán.

## <a name="october-2016"></a>Říjen 2016
**Agent aktualizace:**
- Agent Azure AD připojení stavu služby AD FS \(verze 2.6.408.0\)
    1. Vylepšení ke zjišťování klienta IP adres v žádosti o ověřování
    2. Opravy chyb souvisejících s upozornění
- Agent Azure AD připojení stavu služby AD DS (verze 2.6.408.0)
    1. Opravy chyb souvisejících s upozornění.
- Azure agent stavu připojení AD pro synchronizaci (verze 2.6.353.0) obsažena v Azure AD Connect verze 1.1.281.0
    1. Zadání požadovaná data pro zpráv o chybách synchronizace
    2. Opravy chyb souvisejících s upozornění

**Nové funkce:**
- Připojení zpráv o chybách synchronizace pro Azure AD

**Nové funkce:**
- Azure AD připojení stavu služby AD FS - pole IP adresa je k dispozici ve zprávě o horní 50 uživatelů s špatné uživatelské jméno a heslo.

## <a name="july-2016"></a>Červenec 2016

**Nové funkce:**

- [Azure AD Connect stavu služby AD DS](active-directory-aadconnect-health-adds.md).


## <a name="january-2016"></a>Leden 2016


**Agent aktualizace:**

- Agent Azure AD připojení stavu služby AD FS (verze 2.6.91.1512)


**Nové funkce:**

- [Nástroj Test připojení pro Azure AD připojení agenti stavu](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service)


## <a name="november-2015"></a>Listopadu 2015


**Nové funkce:**

- Podpora pro [řízení přístupu na základě rolí](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control)


**Nové funkce:**

- [Azure AD připojení stav synchronizace](active-directory-aadconnect-health-sync.md).

**Pevné problémy:**

- Opravy chyb chyby během agent registrace.

## <a name="september-2015"></a>Září 2015

**Nové funkce:**

- Nesprávné uživatelské jméno heslo sestavy služby AD FS
- Podpora pro nastavení neověřených nastavit informace HTTP Proxy
- Podpora pro konfiguraci agent jádra serveru
- Vylepšení upozornění na AD FS
- Vylepšení v Azure AD připojení Agent stavu služby AD FS pro připojení a dat nahrát.


**Pevné problémy:**

- Opravy chyb v používání přehledy pro typy chyb AD FS.


## <a name="june-2015"></a>Června 2015

**Počáteční verzi Azure AD stavu připojení pro službu AD FS a AD FS Proxy.**

**Nové funkce:**

- Upozornění pro sledování servery AD FS a AD FS Proxy s e-mailová oznámení.
- Snadný přístup k topologie AD FS a vzorce v AD FS výkonnosti.
- Trend úspěšné tokenu požadavky na servery služby AD FS seskupené podle aplikace, metody ověřování atd požádat o síťové umístění.
- Trendy v žádosti o nezdařeném uložení na servery služby AD FS seskupené aplikacemi chyby typy atd.
- Jednodušší nasazení Agent pomocí přihlašovacích údajů Azure AD globálního správce.  




## <a name="next-steps"></a>Další kroky
Další informace o [sledování vaší místní identity infrastruktury a synchronizaci služby v cloudu](active-directory-aadconnect-health.md).
