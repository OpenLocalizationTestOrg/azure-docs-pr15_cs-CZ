<properties
    pageTitle="Azure AD Connect synchronizace: opětovným spuštěním Průvodce instalací | Microsoft Azure"
    description="Vysvětluje, jak funguje Průvodce instalací druhém čas spuštění."
    keywords="Průvodce instalací Azure AD Connect je možné konfigurovat nastavení údržby druhého času spuštění"
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
    ms.date="08/31/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-running-the-installation-wizard-a-second-time"></a>Azure AD Connect synchronizace: opětovným spuštěním Průvodce instalací
Při prvním spuštění Průvodce instalací Azure AD Connect ho provede vás jak nakonfigurovat instalace. Pokud znova spusťte Průvodce instalací nabízí možnosti pro údržbu.

Průvodce instalací můžete najít v nabídce start s názvem **Azure AD Connect**.

![Nabídka Start](./media/active-directory-aadconnectsync-installation-wizard/startmenu.png)

Po spuštění Průvodce instalací se zobrazí stránka tyto možnosti:

![Stránka se zobrazí seznam dalších úkolů](./media/active-directory-aadconnectsync-installation-wizard/additionaltasks.png)

Pokud jste nainstalovali služby AD FS s Azure AD Connect, máte i další možnosti. Další možnosti, které používáte pro služby AD FS jsou popsány v [části Správa služby AD FS](active-directory-aadconnect-federation-management.md#ad-fs-management).

Vyberte jednu z možností a klikněte na **Další** a pokračujte.

> [AZURE.IMPORTANT] Během ještě otevřeného Průvodce instalací se pozastaví všechny operace v modulu synchronizace. Zkontrolujte, že zavřete průvodce instalací hned po dokončení změny konfigurace.

## <a name="view-current-configuration"></a>Konfigurace aktuální zobrazení
Tato možnost umožňuje rychlý přehled o konfigurovaných možností.

![Stránka s seznam možností a stavu](./media/active-directory-aadconnectsync-installation-wizard/viewconfig.png)

Klikněte na **předchozí** přejdete zpět. Pokud vyberete možnost **Ukončit**, zavřete průvodce instalací.

## <a name="customize-synchronization-options"></a>Přizpůsobení možností synchronizace
Tato možnost slouží k synchronizaci konfiguraci měnit. Zobrazí podmnožinu možnosti z instalační cesta vlastní konfigurace. I když jste původně použili Expresní instalace uvidíte tuto možnost.

- Můžete [Přidat další adresáře](active-directory-aadconnect-get-started-custom.md#connect-your-directories). Odebrání adresáře, najdete v tématu [Odstranění spojnice](active-directory-aadconnectsync-service-manager-ui-connectors.md#delete).
- [Změna domény a OU filtrování](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering).
- Filtrování odstranit skupinu.
- [Volitelné funkce změnit](active-directory-aadconnect-get-started-custom.md#optional-features).

Další možnosti z počáteční instalace nemohli měnit a nejsou k dispozici. Jsou tyto možnosti:

- Změňte atribut userPrincipalName a sourceAnchor.
- Změňte metodu spojování pro objekty z jiné struktuře.
- Povolte skupinových filtrování.

## <a name="refresh-directory-schema"></a>Aktualizace schématu adresáře
Tato možnost se používá při změně schématu jedním ze svého místního služby AD DS strukturami. Například může mít instalaci Exchange nebo upgradu na Windows Server 2012 schéma s objekty zařízení. V takovém případě budete muset pokyn Azure AD Connect schématu znovu ze služby AD DS a aktualizovat mezipaměti. Tato akce také obnoví pravidla synchronizace. Pokud chcete přidat schématu Exchange jako příklad, synchronizace pravidla pro Exchange se přidají do konfiguraci.

Pokud vyberete tuto možnost, jsou uvedené všechny adresáře v konfiguraci. Můžete ponechat ve výchozím nastavení a aktualizace všech strukturami nebo zrušení výběru některé z nich.

![Stránka se zobrazí seznam všech adresářů v prostředí](./media/active-directory-aadconnectsync-installation-wizard/refreshschema.png)

## <a name="configure-staging-mode"></a>Konfigurace pracovní režimu
Tato možnost umožňuje povolit nebo zakázat pracovní režimu na serveru. Další informace o přípravu režimu a jak se používá najdete v [operace](active-directory-aadconnectsync-operations.md#staging-mode).

Možnost zobrazí, pokud pracovní aktuálně povolit nebo zakázat:  
![Možnost také zobrazuje aktuální stav pracovní režimu](./media/active-directory-aadconnectsync-installation-wizard/stagingmodecurrentstate.png)

Pokud chcete změnit stav, vyberte tuto možnost a výběr nebo zrušení výběru zaškrtávacího políčka.  
![Možnost také zobrazuje aktuální stav pracovní režimu](./media/active-directory-aadconnectsync-installation-wizard/stagingmodeenable.png)

## <a name="change-user-sign-in"></a>Změna přihlašování uživatelů
Tato možnost umožňuje změnit z synchronizaci hesel federace nebo naopak. Nemůžete změnit na **Konfigurovat**.

Další informace o tuto možnost najdete v článku [přihlášení uživatele](active-directory-aadconnect-user-signin.md#changing-user-sign-in-method).

## <a name="next-steps"></a>Další kroky

- Další informace o konfiguraci modelu používaný synchronizace Azure AD Connect v [Principy deklarativní zřizování](active-directory-aadconnectsync-understanding-declarative-provisioning.md).

**Přehled témat**

- [Azure AD Connect synchronizace: princip a vlastní nastavení synchronizace](active-directory-aadconnectsync-whatis.md)
- [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md)
