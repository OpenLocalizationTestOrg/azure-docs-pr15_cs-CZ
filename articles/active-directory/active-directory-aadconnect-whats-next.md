<properties
    pageTitle="Azure AD Connect: Další kroky a jak mají ovládat Azure AD Connect | Microsoft Azure"
    description="Zjistěte, jak prodloužit výchozí konfigurace a provozu Azure AD Connect."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="next-steps-and-how-to-manage-azure-ad-connect"></a>Další kroky a jak mají ovládat Azure AD Connect
Následující jsou rozšířené provozní témata, která umožňuje přizpůsobit Azure Active Directory připojit podle potřeby a požadavky organizace.  

## <a name="add-additional-sync-administrators"></a>Přidání další synchronizaci správců
Ve výchozím nastavení pouze uživatele, kterému nebyla instalace a místních správců bude moct spravovat modul nainstalované synchronizace. Další uživatelé moct přejít a správu modul Synchronizace najděte požadovanou skupinu s názvem ADSyncAdmins místního serveru a přidat je do této skupiny.

## <a name="assigning-licenses-to-azure-ad-premium-and-enterprise-mobility-users"></a>Přiřazení licence uživatelům Azure AD Premium a Enterprise mobilita

Teď, když uživatelé byly synchronizované do cloudu, musíte přiřadit licenci, můžete Začínáme s aplikacemi jiných cloudu například Office 365.

### <a name="to-assign-an-azure-ad-premium-or-enterprise-mobility-suite-license"></a>Pokud chcete přiřadit Azure AD Premium nebo licenci Enterprise mobilita sady
--------------------------------------------------------------------------------
1. Přihlaste se k portálu Azure jako správce.
2. V levé části vyberte **Služby Active Directory**.
3. Na stránce služby Active Directory dvakrát klikněte na adresář, který má uživatelů, které chcete povolit.
4. V horní části stránky služby directory vyberte **licence**.
5. Na stránce licence vyberte Active Directory Premium nebo Enterprise mobilita sady a pak klikněte na **přiřadit**.
6. V dialogovém okně vyberte uživatele, kterému chcete přiřadit licence a potom klikněte na ikonu zaškrtnutí k uložení změn.


## <a name="verifying-the-scheduled-synchronization-task"></a>Ověření naplánované synchronizace úkolů
Pokud chcete zkontrolovat stav synchronizace provést kontrolou Azure portálu.

### <a name="to-verify-the-scheduled-synchronization-task"></a>Chcete-li ověřit naplánované synchronizace úkolů
--------------------------------------------------------------------------------
1. Přihlaste se k portálu Azure jako správce.
2. V levé části vyberte **Služby Active Directory**.
3. Na stránce služby Active Directory dvakrát klikněte na adresář, který má uživatelů, které chcete povolit.
4. V horní části stránky služby directory vyberte **Integrace adresářů**.
5. V části Integrace se službou místní služby active directory poznámku poslední synchronizace.

<center>![Cloud](./media/active-directory-aadconnect-whats-next/verify.png)</center>

## <a name="starting-a-scheduled-synchronization-task"></a>Spuštění úlohy naplánované synchronizace
Pokud potřebujete k spuštění synchronizace úkolů provést opětovným spuštěním Průvodce Azure AD Connect.  Je třeba zadat svoje přihlašovací údaje Azure AD.  V průvodci vyberte úkol **Přizpůsobit synchronizace možnosti** a klikněte na další procházet všemi kroky průvodce. Na konci Ujistěte se, že je zaškrtnuté políčko **Spustit synchronizaci hned po dokončení počáteční konfiguraci** .

<center>![Cloud](./media/active-directory-aadconnect-whats-next/startsynch.png)</center>

Další informace o synchronizaci Azure AD Connect: Plánovač, najdete v článku [Azure AD připojení Plánovač](active-directory-aadconnectsync-feature-scheduler.md)


## <a name="additional-tasks-available-in-azure-ad-connect"></a>Další úkoly k dispozici v Azure AD Connect
Po instalaci počáteční Azure AD Connect vždy spustíte průvodce znovu z místní Azure AD Connect úvodní stránku nebo plochy.  Zjistíte, že znovu přejít pomocí Průvodce obsahuje několik nových možností v podobě dalších úkolů.  

Následující tabulka obsahuje přehled těchto úkolů a stručný popis jednotlivých hodnot.

![Připojit se ke pravidla](./media/active-directory-aadconnect-whats-next/addtasks.png)


Další úkolu | Popis
------------- | ------------- |
Zobrazení vybraného scénáře  |Umožňuje zobrazit aktuální Azure AD Connect řešení.  Jedná se o obecné nastavení synchronizované adresáře, nastavení synchronizace, atd.
Přizpůsobení možností synchronizace | Umožňuje změnit aktuální konfigurace včetně přidání dalších doménových služby Active Directory konfiguraci nebo povolení synchronizace možnosti například uživatele, skupinu, zařízení nebo heslo zpětného zápisu.
Povolit režim pracovní |  Umožňuje fázi informace, které bude dál synchronizovat, ale nic budou exportovány Azure AD nebo Active Directory.  Umožňuje zobrazit náhled synchronizace před jejich vzniku.

## <a name="next-steps"></a>Další kroky
Další informace o [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).
