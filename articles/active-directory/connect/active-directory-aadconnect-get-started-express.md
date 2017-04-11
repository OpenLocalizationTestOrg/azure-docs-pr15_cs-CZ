<properties
    pageTitle="Azure AD Connect: Začínáme používat Expresní nastavení | Microsoft Azure"
    description="Informace o stažení, instalace a spuštění Průvodce nastavením pro Azure AD Connect."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/13/2016"
    ms.author="billmath"/>

# <a name="getting-started-with-azure-ad-connect-using-express-settings"></a>Začínáme s použitím Expresní nastavení Azure AD Connect
Azure AD Connect **Expresní nastavení** se používá, když máte topologie jednoduchým struktury a [Synchronizace hesel](../active-directory-aadconnectsync-implement-password-synchronization.md) pro ověřování. **Expresní nastavení** je výchozí možnost a používá nejčastěji nasazeném scénáře. Měli byste být jenom pár krátké kliknutími pryč pro prodloužení svého místního adresáře do cloudu.

Než začnete instalaci Azure AD Connect, Nejdřív musíte mít [Stáhnout Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771) a dokončete předpokladem kroků v [Azure AD Connect: Hardware a předpokladech](../active-directory-aadconnect-prerequisites.md).

Pokud Expresní nastavení neodpovídá topologii, přečtěte si článek [související si přečtěte následující dokumentaci](#related-documentation) pro další scénáře.

## <a name="express-installation-of-azure-ad-connect"></a>Expresní instalace Azure AD Connect
Zobrazí se následující postup v akci v části [videa](#videos) .

1. Přihlaste se jako místní správce serveru, ke kterému se chcete nainstalovat Azure AD Connect. Je vhodné provést na serveru, který chcete mít serveru synchronizace.
2. Přejděte na a poklikejte na **AzureADConnect.msi**.
3. Na úvodní obrazovce zaškrtněte políčko s podmínkami této licenční smlouvy a klikněte na **pokračovat**.  
4. Na obrazovce nastavení Express klikněte na **použít Expresní nastavení**.  
![Vítá vás Azure AD připojení](./media/active-directory-aadconnect-get-started-express/express.png)
5. V části připojit služby Azure AD obrazovku zadejte uživatelské jméno a heslo globálního správce pro Azure AD. Klikněte na tlačítko **Další**.  
![Připojení k Azure AD](./media/active-directory-aadconnect-get-started-express/connectaad.png) Pokud dojít k chybě a máte problémy s připojením, pak v tématu [Poradce při potížích s připojením](../active-directory-aadconnect-troubleshoot-connectivity.md).
6. V části připojit služby AD DS obrazovku zadejte uživatelské jméno a heslo účtu správce organizace. Zadejte část doménu ve formátu NetBios nebo plně kvalifikovaný název domény, které je FABRIKAM\administrator nebo fabrikam.com\administrator. Klikněte na tlačítko **Další**.  
![Připojení k službě AD DS](./media/active-directory-aadconnect-get-started-express/connectad.png)
7. Stránka [**Azure AD přihlašovací konfigurace**](../active-directory-aadconnect-user-signin.md#azure-ad-sign-in-configuration) zobrazuje pouze pokud je nebylo dokončeno, [zjistit předpoklady pro](../active-directory-aadconnect-prerequisites.md) [ověření vaší domény](../active-directory-add-domain.md) .
![Neověřené domén](./media/active-directory-aadconnect-get-started-express/unverifieddomain.png)  
Pokud se zobrazí na této stránce, prostudujte každá doména označené **Nepřidali** a **Ne ověření**. Zkontrolujte, že tyto domény, který používáte ověřili v Azure AD. Kliknutím na aktualizovat po ověření svojí domény.
8. V dialogu jste připravení konfigurovat obrazovku klikněte na **instalovat**.
    - Pokud chcete v dialogu jste připravení konfigurace stránky, můžete můžete zrušit zaškrtnutí položky zaškrtávací políčko **Spustit synchronizaci hned po dokončení konfigurace** . Pokud chcete provést další konfiguraci, třeba [filtrování](../active-directory-aadconnectsync-configure-filtering.md)by měl zrušte zaškrtnutí políčka toto políčko. Pokud zrušíte výběr této možnosti, Průvodce nakonfiguruje synchronizace, ale necháte Plánovač zakázané. Nejde spustit, dokud je ji povolit ručně spuštěním [Průvodce instalací](../active-directory-aadconnectsync-installation-wizard.md).
    - Máte Exchange Active Directory vaší místní také máte možnost Povolit [**hybridní nasazení Exchange nasazení**](https://technet.microsoft.com/library/jj200581.aspx). Tuto možnost použijte, pokud chcete mít poštovní schránky serveru Exchange i v cloudu a místní ve stejnou dobu.
![Jste připravení konfigurace Azure AD Connect](./media/active-directory-aadconnect-get-started-express/readytoconfigure.png)
9. Po dokončení instalace klikněte na **Konec**.
10. Po dokončení instalace odhlášení a znovu se přihlaste před použitím Správce služby synchronizace nebo Editor pravidel synchronizace.

## <a name="videos"></a>Videa

Video o používání Expresní instalace najdete v tématu:

>[AZURE.VIDEO azure-active-directory-connect-express-settings]

## <a name="next-steps"></a>Další kroky
Teď, když máte nainstalovaný Azure AD Connect můžete [ověřit instalaci a přiřazovat licence](../active-directory-aadconnect-whats-next.md).

Další informace o těchto funkcích, které jsou povoleny k instalaci: [Automatické upgrade](../active-directory-aadconnect-feature-automatic-upgrade.md) [náhodné zabránit odstranění](../active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)a [Stavu připojení Azure AD](../active-directory-aadconnect-health-sync.md).

Další informace o tématech běžné: [Plánovač a jak spustit synchronizaci](../active-directory-aadconnectsync-feature-scheduler.md).

Další informace o [Integrace místních identit s Azure Active Directory](../active-directory-aadconnect.md).

## <a name="related-documentation"></a>Související si přečtěte následující dokumentaci

Téma |  
--------- | ---------
Přehled Azure AD Connect | [Integrace místních identit s Azure Active Directory](../active-directory-aadconnect.md)
Instalace používá vlastní nastavení | [Vlastní instalace Azure AD Connect](active-directory-aadconnect-get-started-custom.md)
Upgrade z DirSync | [Upgrade z synchronizační nástroj služby Azure AD (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md)
Účty, které používají pro instalaci | [Další informace o Azure AD Connect účtů a oprávnění](active-directory-aadconnect-accounts-permissions.md)
