<properties
   pageTitle="Azure AD Connect: Upgradovat z DirSync | Microsoft Azure"
   description="Naučte se upgradovat z DirSync Azure AD Connect. Tento článek popisuje postup upgradu z DirSync na Azure AD Connect."
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
   ms.topic="get-started-article"
   ms.date="08/19/2016"
   ms.author="shoatman;billmath"/>

# <a name="azure-ad-connect-upgrade-from-dirsync"></a>Azure AD Connect: Upgrade z DirSync
Azure AD Connect je následník DirSync. Najděte způsobů, jak můžete upgradovat z DirSync v tomto tématu. Tyto kroky nefungují pro upgrade z jiné verzi Azure AD Connect nebo z Azure AD Sync.

Než začnete instalaci Azure AD Connect, Nejdřív musíte mít [Stáhnout Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771) a dokončete předpokladem kroků v [Azure AD Connect: Hardware a požadavcích](../active-directory-aadconnect-prerequisites.md). Zejména si chcete přečíst o následující, protože tyto oblasti se liší od DirSync:

- Požadovanou verzi .net a Powershellu. Novější verze musí být na serveru než co DirSync potřeby.
- Konfigurace proxy serveru. Pokud používáte proxy server přístup k Internetu, musí být toto nastavení nakonfigurované před upgradem. Nástroj DirSync vždy používá proxy server nakonfigurovaný pro uživatele instalaci, ale nastavení počítače Azure AD Connect používá místo.
- Adresy URL musí být otevřený na proxy serveru. Základní scénáře, můžou být také nepodporuje DirSync, jsou požadavky na stejné. Pokud chcete použít některý z nových funkcí s Azure AD Connect, musí být otevřena některé nové adresy URL.

Pokud nejsou upgrade z DirSync, přečtěte si článek [související si přečtěte následující dokumentaci](#related-documentation) pro další scénáře.

## <a name="upgrade-from-dirsync"></a>Upgrade z DirSync
V závislosti na nasazením aktuální DirSync existují různé možnosti pro upgrade. Pokud předpokládanou dobu, po upgradu méně než tři hodiny, pak doporučujeme provést místní upgrade. Pokud předpokládanou dobu, po upgradu je delší než tři hodiny, doporučení je dělat paralelní nasazení na jiném serveru. Odhadne se, že pokud máte víc než 50 000 objekty přenese více než tři hodiny dělat upgradu.

Scénář |  
---- | ----
[Místní upgrade](#in-place-upgrade)  | Upřednostňované políčko v případě, že upgrade je očekáván na menší než 3 hodin.
[Paralelní nasazení](#parallel-deployment) | Upřednostňovaného možnost Pokud upgrade trvat víc než 3 hodin.

>[AZURE.NOTE] Při plánování upgradovat z DirSync Azure AD Connect odinstalování DirSync sami před upgradem. Azure AD Connect bude číst migrovat konfiguraci z DirSync a po podrobná serveru odinstalovat.

**Místní upgrade**  
Očekávaný čas na dokončení upgradu se zobrazí průvodce. Tento odhad vychází z předpokladu, že trvá tři hodiny k dokončení upgradu pro danou databázi s více než 50 000 objekty (uživatele, kontakty a skupiny). Pokud počet objektů v databázi je menší než 50 000, Azure AD Connect doporučuje upgrade na místě. Pokud se rozhodnete dál, aktuální nastavení automaticky, použijí se při upgradu a vašeho serveru automaticky obnoví synchronizace služby active.

Pokud chcete udělat konfigurace migraci a proveďte paralelní nasazení, můžete přepsat místní upgrade doporučení. Například může trvat možnost aktualizace hardware a operační systém. Další informace v části [paralelní nasazení](#parallel-deployment) .

**Paralelní nasazení**  
Pokud máte víc než 50 000 objektů se doporučuje paralelní nasazení. To je zabráněno provozní zpoždění zkušených uživatelů. Instalace Azure AD Connect pokusí k odhadu výpadek upgradu, ale po upgradu DirSync v minulosti vlastní prostředí bude pravděpodobně vhodné průvodce.

### <a name="supported-dirsync-configurations-to-be-upgraded"></a>Podporované konfigurace DirSync k upgradu
Následující konfigurace, které změny jsou podporovány u DirSync upgradu:

- Domény a OU filtrování
- Alternativní ID (UPN)
- Synchronizace hesel a hybridní nastavení serveru Exchange
- Doménové/domény a nastavení služby Azure AD
- Filtrování podle atributy uživatele

Následující změnu nejde upgradovat. Pokud máte tuto konfiguraci, upgradu je blokován:

- Nepodporované DirSync změny, například odebrány atributy a ve tvaru přípony vlastní DLL

![Upgrade blokované](./media/active-directory-aadconnect-dirsync-upgrade-get-started/analysisblocked.png)

V těchto případech doporučujeme nainstalovat nové server Azure AD Connect v [pracovní režimu](../active-directory-aadconnectsync-operations.md#staging-mode) a ověřte staré DirSync a nová Azure AD Connect konfigurace. Znovu použijte změny pomocí vlastní konfigurace popsané v [Azure AD Sync připojení vlastní konfigurace](../active-directory-aadconnectsync-whatis.md).

Hesly potřebnými pomocí DirSync účtů služeb nelze načíst a se nepřenášejí. Tato hesla se neobnoví během upgradu.

### <a name="high-level-steps-for-upgrading-from-dirsync-to-azure-ad-connect"></a>Podrobný postup pro upgrade z DirSync Azure AD Connect

1. Vítá vás Azure AD připojení
2. Analýza aktuální konfigurace DirSync
3. Shromáždit Azure AD globální správce hesel
4. Zadání přihlašovacích údajů pro účet správce organizace (jenom pro používaný během instalace Azure AD Connect)
5. Instalace Azure AD připojení
    * Odinstalace DirSync (nebo dočasném vypnutí)
    * Instalace Azure AD připojit
    * Pokud chcete začít synchronizace

Další kroky jsou potřeba při:

* Momentálně používáte úplné serveru SQL Server – místní nebo vzdálené
* Máte víc než 50 000 objekty v rozsahu synchronizace

## <a name="in-place-upgrade"></a>Místní upgrade

1. Spusťte instalační služby Azure AD Connect (MSI).
2. Zkontrolujte a souhlasíte s licenční podmínky a osobních údajů.
![Vítá vás Azure AD](./media/active-directory-aadconnect-dirsync-upgrade-get-started/Welcome.png)
3. Začněte analýza existující instalace DirSync, klikněte na další.
![Analýza existující instalace synchronizace adresáře](./media/active-directory-aadconnect-dirsync-upgrade-get-started/Analyze.png)
4. Po dokončení analýza zobrazí doporučení o tom, jak pokračovat.  
    - Pokud používáte SQL serveru Express a menší než 50 000 objekty, se zobrazují na následující obrazovce: ![Analýza dokončena připravení na upgrade z DirSync](./media/active-directory-aadconnect-dirsync-upgrade-get-started/AnalysisReady.png)
    - Pokud používáte úplné SQL serveru pro DirSync, můžete na této stránce najdete místo: ![Analýza dokončena připravení na upgrade z DirSync](./media/active-directory-aadconnect-dirsync-upgrade-get-started/AnalysisReadyFullSQL.png)  
Zobrazí se informace týkající se používá DirSync existující databázový server SQL Server. V případě potřeby proveďte odpovídající úpravy. Klikněte na **Další** a pokračujte instalace.
    - Pokud máte víc než 50 000 objektů, místo toho zobrazí tato obrazovka: ![Analýza dokončena připravení na upgrade z DirSync](./media/active-directory-aadconnect-dirsync-upgrade-get-started/AnalysisRecommendParallel.png)  
Pokračujte v místní upgrade, zaškrtněte políčko vedle tato zpráva: **pokračujte v upgradu DirSync na tomto počítači.**
Pokud chcete místo toho [paralelní nasazení](#parallel-deployment) , export nastavení konfigurace DirSync a přesunout konfiguraci nový server.
5. Zadejte heslo pro účet, který používáte pro připojení k Azure AD. Musí být účtu aktuálně používaná DirSync.
![Zadejte svoje přihlašovací údaje Azure AD](./media/active-directory-aadconnect-dirsync-upgrade-get-started/ConnectToAzureAD.png)  
Pokud dojít k chybě a máte problémy s připojením, přečtěte si článek [Poradce při potížích s připojením](../active-directory-aadconnect-troubleshoot-connectivity.md).
6. Zadejte účet správce organizace služby Active Directory.
![Zadejte svoje přihlašovací údaje přidá](./media/active-directory-aadconnect-dirsync-upgrade-get-started/ConnectToADDS.png)
7. Teď jste připraveni konfigurace. Když kliknete na **upgradovat**, odinstalaci DirSync a Azure AD Connect nakonfigurované a začne synchronizaci.
![Připraveno ke konfiguraci](./media/active-directory-aadconnect-dirsync-upgrade-get-started/ReadyToConfigure.png)
8. Po dokončení instalace odhlaste se a přihlaste se znova k Windows před použitím Správce služby synchronizace, Editor pravidel synchronizace, nebo se pokusíte dělat jakékoliv jiné změny konfigurace.

## <a name="parallel-deployment"></a>Paralelní nasazení

### <a name="export-the-dirsync-configuration"></a>Export konfigurace DirSync
**Paralelní nasazení s víc než 50 000 objektů**

Pokud máte víc než 50 000 objektů, instalace Azure AD Connect doporučuje paralelní nasazení.

Zobrazí se obrazovka podobná této:

![Analýza dokončena](./media/active-directory-aadconnect-dirsync-upgrade-get-started/AnalysisRecommendParallel.png)

Pokud chcete pokračovat paralelní nasazení, musíte provést následující postup:

- Klikněte na tlačítko **Exportovat nastavení** . Při instalaci Azure AD Connect na samostatném serveru toto nastavení se migrují od aktuální nástroj DirSync k instalaci nového Azure AD Connect.

Po úspěšném exportu nastavení můžete Azure AD Connect Průvodce ukončíte na DirSync server. Pokračujte dalším krokem a [nainstalujte na samostatný server Azure AD Connect](#installation-of-azure-ad-connect-on-separate-server)

**Paralelní nasazení, kde je menší než 50 000 objektů**

Pokud máte míň než 50 000 objektů, ale ještě měli chtít paralelní nasazení, pak udělejte toto:

1. Spusťte instalační program Azure AD Connect (MSI).
2. Pokud se zobrazí obrazovce **Vítá vás Azure AD Connect** , ukončete Průvodce instalací kliknutím na křížek "X" v pravém horním rohu okna.
3. Otevřete příkazový řádek.
4. Z umístění instalace Azure AD Connect (výchozí: C:\Program Files\Microsoft Azure Active Directory se připojit) zadejte následující příkaz:  `AzureADConnect.exe /ForceExport`.
5. Klikněte na tlačítko **Exportovat nastavení** .  Při instalaci Azure AD Connect na samostatném serveru toto nastavení se migrují od aktuální nástroj DirSync k instalaci nového Azure AD Connect.

![Analýza dokončena](./media/active-directory-aadconnect-dirsync-upgrade-get-started/forceexport.png)

Po úspěšném exportu nastavení můžete Azure AD Connect Průvodce ukončíte na DirSync server. Pokračujte dalším krokem a [nainstalujte na samostatný server Azure AD Connect](#installation-of-azure-ad-connect-on-separate-server)

### <a name="install-azure-ad-connect-on-separate-server"></a>Instalace na samostatný server Azure AD Connect

Při instalaci Azure AD Connect na novém serveru je za předpokladu, že chcete provést instalaci Azure AD Connect. Když budete chtít používat konfigurace DirSync, existují některé dodatečné kroky:

1. Spusťte instalační program Azure AD Connect (MSI).
2. Pokud se zobrazí obrazovce **Vítá vás Azure AD Connect** , ukončete Průvodce instalací kliknutím na křížek "X" v pravém horním rohu okna.
3. Otevřete příkazový řádek.
4. Z umístění instalace Azure AD Connect (výchozí: C:\Program Files\Microsoft Azure Active Directory se připojit) zadejte následující příkaz:  `AzureADConnect.exe /migrate`.
    Průvodce instalací Azure AD Connect spustí a zobrazí se na následující obrazovce: ![zadejte svoje přihlašovací údaje Azure AD](./media/active-directory-aadconnect-dirsync-upgrade-get-started/ImportSettings.png)
5. Vyberte nastavení soubor, který vyexportovaný z instalace DirSync.
6. Konfigurace všechny pokročilé možnosti včetně:
    - Vlastní instalace umístění Azure AD Connect.
    - Existující instanci systému SQL Server (výchozí: Azure AD Connect nainstaluje SQL Server 2012 Express). Nepoužívejte stejný instance databázového jako DirSync server.
    - Účet služby používá pro připojení k serveru SQL Server (Pokud databázi SQL serveru vzdálený a pak tento účet musí být účet služby domény).
Tyto možnosti se vejde na této obrazovce: ![zadejte svoje přihlašovací údaje Azure AD](./media/active-directory-aadconnect-dirsync-upgrade-get-started/advancedsettings.png)
7. Klikněte na tlačítko **Další**.
8. Na stránce **Připraveno ke konfiguraci** nechte **zahájení procesu synchronizace hned po dokončení konfigurace** zaškrtnuté. Server je teď v [přípravu režimu](../active-directory-aadconnectsync-operations.md#staging-mode) tak změny nejsou exportovány Azure AD.
9. Klikněte na **instalovat**.
10. Po dokončení instalace odhlaste se a přihlaste se znova k Windows před použitím Správce služby synchronizace, Editor pravidel synchronizace, nebo se pokusíte dělat jakékoliv jiné změny konfigurace.

>[AZURE.NOTE] Synchronizace služby Active Directory pro Windows Server a Azure Active Directory, nebude zahájen, ale žádné změny jsou exportovány Azure AD. Jedinou nástroj pro synchronizaci můžete aktivně exportovat změn najednou. Tento stav se nazývá [přípravu režimu](../active-directory-aadconnectsync-operations.md#staging-mode).

### <a name="verify-that-azure-ad-connect-is-ready-to-begin-synchronization"></a>Ověřte, zda je připraven zahájit synchronizace Azure AD Connect

Ověřte, jestli je Azure AD Connect chtít převzít řízení z DirSync, musíte otevřít **Správce služby synchronizace** ve skupině **Azure AD Connect** z nabídky start.

V aplikaci přejděte na kartu **operace** . Na této kartě potvrďte, že jste dokončili tyto operace:

- Import na spojnici AD
- Import na Azure AD spojnice
- Úplné synchronizovat přes AD spojnice
- Úplné synchronizovat přes Azure AD spojnice

![Import a synchronizace dokončena](./media/active-directory-aadconnect-dirsync-upgrade-get-started/importsynccompleted.png)

Zkontrolujte výsledky z těchto akcí a ujistěte se, že nejsou žádné chyby.

Pokud chcete, aby se zobrazily a kontrolovat změny, které jsou o k exportu do Azure AD pak si projděte témata ověření konfigurace ve skupinovém rámečku [pracovní režim](../active-directory-aadconnectsync-operations.md#staging-mode). Dokud se nezobrazí žádné neočekávané změňte požadovaná konfigurace.

Jste připraveni chcete přepnout snímání z DirSync Azure AD při po provedení těchto kroků a spokojení s výsledkem.

### <a name="uninstall-dirsync-old-server"></a>Odinstalace DirSync (starý server)

- **Programy** a funkce najdete **synchronizační nástroj služby Windows Azure Active Directory**
- Odinstalace **synchronizační nástroj služby Windows Azure Active Directory**
- Odinstalace může trvat až 15 minut.

Pokud chcete odinstalovat DirSync později, můžete taky dočasně vypnout serveru nebo vypnutí služby. Pokud se vyskytne problém, tato metoda umožňuje znovu povolit. Však není očekává, dalším krokem se nepovede, tak by neměly být potřeba.

Pomocí DirSync odinstalovat nebo zakázat active server není exportu do Azure AD. Dalším krokem povolit Azure AD Connect musí být dokončen před změny ve vaší místní Active Directory, zůstanou dál synchronizovat s Azure AD.

### <a name="enable-azure-ad-connect-new-server"></a>Povolení Azure AD Connect (nový server)
Po instalaci, znovu otevřete Azure AD připojení bude umožňuje provádět změny další konfiguraci. Spuštění **Azure AD Connect** z nabídky start nebo zástupce na ploše. Zkontrolujte, že není pokusíte znovu spusťte instalaci sady MSI.

Měli byste vidět takto:

![Další úkoly](./media/active-directory-aadconnect-dirsync-upgrade-get-started/AdditionalTasks.png)

- Vyberte **Konfigurovat přípravu režimu**.
- Vypnutí přípravu zrušením zaškrtnutí zaškrtávacího políčka **Povolit pracovní režimu** .

![Zadejte svoje přihlašovací údaje Azure AD](./media/active-directory-aadconnect-dirsync-upgrade-get-started/configurestaging.png)

- Kliknutím na tlačítko **Další**
- Na stránce potvrzení klikněte na tlačítko **nainstalovat** .

Azure AD Connect je nyní aktivní serveru.

## <a name="next-steps"></a>Další kroky
Teď, když máte nainstalovaný Azure AD Connect můžete [ověřit instalaci a přiřazovat licence](../active-directory-aadconnect-whats-next.md).

Další informace o těchto nových funkcích, které jsou povoleny k instalaci: [Automatické upgrade](../active-directory-aadconnect-feature-automatic-upgrade.md) [náhodné zabránit odstranění](../active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)a [Stavu připojení Azure AD](../active-directory-aadconnect-health-sync.md).

Další informace o tématech běžné: [Plánovač a jak spustit synchronizaci](../active-directory-aadconnectsync-feature-scheduler.md).

Další informace o [Integrace místních identit s Azure Active Directory](../active-directory-aadconnect.md).

## <a name="related-documentation"></a>Související si přečtěte následující dokumentaci

Téma |  
--------- | ---------
Přehled Azure AD Connect | [Integrace místních identit s Azure Active Directory](../active-directory-aadconnect.md)
Upgrade z předchozí verze připojení | [Upgrade z předchozí verze připojení](../active-directory-aadconnect-upgrade-previous-version.md)
Instalace pomocí nastavení Express | [Expresní instalace Azure AD Connect](active-directory-aadconnect-get-started-express.md)
Instalace používá vlastní nastavení | [Vlastní instalace Azure AD Connect](active-directory-aadconnect-get-started-custom.md)
Účty, které používají pro instalaci | [Další informace o Azure AD Connect účtů a oprávnění](active-directory-aadconnect-accounts-permissions.md)
