<properties
    pageTitle="Azure AD Connect synchronizace: princip a vlastní nastavení synchronizace | Microsoft Azure"
    description="Vysvětluje, jak Azure AD Connect synchronizace funguje a přizpůsobení."
    services="active-directory"
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
    ms.date="08/29/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-understand-and-customize-synchronization"></a>Azure AD Connect synchronizace: princip a vlastní nastavení synchronizace
Synchronizace služby Azure Active Directory připojení (Azure AD Connect sync) je hlavní komponenty Azure AD Connect. Ho má na starosti všechny operace, které se vztahují k synchronizaci identity dat mezi místním prostředím a Azure AD. Azure AD Connect synchronizace je následník DirSync, Azure AD Sync a správce Forefront identit s Azure Active Directory spojnice nakonfigurované.

Toto téma je domov pro **synchronizaci Azure AD Connect** (nazývané také **sync engine**) a obsahuje odkazy na další témata s ním souvisejí. Odkazy na Azure AD Connect naleznete v tématu [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).

Synchronizace služby je tvořen ze dvou částí, komponentu **Azure AD Connect synchronizaci** místních a straně služby v Azure AD s názvem **synchronizace služby Azure AD Connect**. Služba je běžné DirSync, Azure AD Sync a Azure AD Connect.

## <a name="azure-ad-connect-sync-topics"></a>Témata synchronizace Azure AD Connect

Téma | Co zahrnuje a kdy ke čtení
----- | -----
**Základní informace o synchronizaci Azure AD Connect** |
[Principy architektura](active-directory-aadconnectsync-understanding-architecture.md) | Výsledky Komu začínáte s modul synchronizace a chcete se dozvědět o architektura a termíny používané.
[Technické koncepty](active-directory-aadconnectsync-technical-concepts.md) | Krátká verze témat architektury a stručně popisuje termíny používané.
[Topologie pro Azure AD připojení](active-directory-aadconnect-topologies.md) | Popisuje různé topologií a scénáře, které podporuje modul synchronizace.
**Vlastní konfigurace** |
[Opětovným spuštěním Průvodce instalací](active-directory-aadconnectsync-installation-wizard.md) | Tento článek vysvětluje možnosti máte k dispozici, když spustíte Průvodce instalací Azure AD Connect znovu.
[Principy deklarativní zřízení](active-directory-aadconnectsync-understanding-declarative-provisioning.md)| Popisuje model konfigurace s názvem deklarativní zřizování.
[Principy deklarativní zřizovací výrazů](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) | Popisuje syntaxe jazyka výraz použitý v deklarativní zřizování.
[Principy výchozí konfigurace](active-directory-aadconnectsync-understanding-default-configuration.md)| Popisuje pravidla mimo pole a výchozí konfigurace. Také popisuje, jak fungují pravidla společně mimo pole scénáře práce.
[Principy uživatelů a kontakty](active-directory-aadconnectsync-understanding-users-and-contacts.md) | Pokračuje na předchozí téma a popisuje, jak konfigurace pro uživatele a kontaktů bude vypadat, zejména v prostředí více doménové.
[Jak změnit výchozí konfigurace](active-directory-aadconnectsync-change-the-configuration.md) | Provede vás jak provádět běžné změníte atribut toků konfiguraci.
[Doporučené postupy pro změnu výchozí konfigurace](active-directory-aadconnectsync-best-practices-changing-default-configuration.md) | Podpora omezení a změn konfiguraci mimo pole.
[Konfigurace filtrování](active-directory-aadconnectsync-configure-filtering.md) | Popisuje různé možnosti pro omezení počtu objektů, které je synchronizované Azure AD a podrobný postup pro nastavení těchto možností.
**Funkce a scénáře** |
[Zabránit náhodné odstranění](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) | Popisuje funkci *Zabránění náhodné odstraníte* a jak ji nakonfigurovat.
[Plánovač](active-directory-aadconnectsync-feature-scheduler.md) | Popisuje předdefinované Plánovač, která je importu, synchronizace a export dat.
[Implementace synchronizace hesel](active-directory-aadconnectsync-implement-password-synchronization.md) | Popisuje, jak funguje synchronizace hesel, jak implementovat a způsob ovládání a odstraňování případných problémů.
[Zpětného zápisu zařízení](active-directory-aadconnect-feature-device-writeback.md) | Popisuje, jak funguje zpětného zápisu zařízení v Azure AD Connect.
[Rozšíření Directory](active-directory-aadconnectsync-feature-directory-extensions.md) | Popisuje, jak prodloužit schématu Azure AD pomocí vlastní atributy.
**Synchronizace služby** |
[Azure AD Connect synchronizační služba funkce.](active-directory-aadconnectsyncservice-features.md) | Popisuje straně synchronizace služby a jak lze změnit nastavení synchronizace v Azure AD.
[Duplikování odolnost proti chybám atribut](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) | Popisuje, jak povolit a používat odolnosti hodnoty duplicitní atribut **userPrincipalName** a **proxyAddresses** .
**Operace a uživatelské rozhraní** |
[Správce služby synchronizace](active-directory-aadconnectsync-service-manager-ui.md) | Popisuje uživatelské rozhraní Správce služby synchronizace, včetně [operace](active-directory-aadconnectsync-service-manager-ui-operations.md), [spojnic](active-directory-aadconnectsync-service-manager-ui-connectors.md), [Metaverse návrháře](active-directory-aadconnectsync-service-manager-ui-mvdesigner.md)a karty [Metaverse vyhledávání](active-directory-aadconnectsync-service-manager-ui-mvsearch.md) .
[Provozu a co byste měli zvážit](active-directory-aadconnectsync-operations.md) | Popisuje provozní pochybnosti, například havárie obnovení.
**Jak...** |
[Resetování účtu Azure AD](active-directory-aadconnectsync-howto-azureadaccount.md) | Jak obnovit přihlašovací údaje účtu služby používá pro připojení z Azure AD Connect synchronizace Azure AD.
**Další informace a odkazy** |
[Porty](active-directory-aadconnect-ports.md) | Seznam porty, které musíte otevřít mezi modul synchronizace a místního adresáře a Azure AD.
[Atributy synchronizovat s Azure Active Directory](active-directory-aadconnectsync-attributes-synchronized.md) | Obsahuje seznam všech atributů synchronizuje mezi místním AD a Azure AD.
[Přehled funkcí](active-directory-aadconnectsync-functions-reference.md) | Seznam všech funkcí dostupných v deklarativní zřizování.

## <a name="additional-resources"></a>Další zdroje informací

* [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md)
