<properties
   pageTitle="Azure AD Connect: Upgrade je | Microsoft Azure"
   description="Toto téma popisuje integrované funkci automatického upgradu v Azure AD Connect synchronizovat."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/24/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-automatic-upgrade"></a>Azure AD Connect: Upgrade je
Tato funkce byla zavedená s sestavení 1.1.105.0 (vydána únor 2016).

## <a name="overview"></a>Základní informace
Jak zajistit, že vaše instalace Azure AD Connect je vždy aktuální nebyl jednodušší funkci **Automatické upgrade** . Tato funkce je standardně pro expresní instalace a upgrade DirSync. Po uvolnění novou verzi instalace automaticky aktualizovány.

Automatické upgrade aktivované ve výchozím nastavení tyto věci:

- Expresní nastavení instalace a DirSync upgrady.
- Použití SQL Express LocalDB, tedy co Express nastavení pracujte. Nástroj DirSync s SQL Express také použít LocalDB.
- Účet AD je výchozí účet MSOL_ vytvořil DirSync a nastavení Express.
- Máte míň než 100 000 objektů v metaverse.

Aktuální stav upgrade je možné zobrazit pomocí rutiny prostředí PowerShell `Get-ADSyncAutoUpgrade`. Obsahuje následující režimy:

Stav | Komentář
---- | ----
Povoleno | Upgrade je zapnuta.
Pozastavené | Nastavte systém pouze. Systém je už přijímat automatické upgrady.
Vypnutí | Automatické upgrade zakázán.

Můžete změnit mezi **povoleno** a **Zakázáno** s `Set-ADSyncAutoUpgrade`. Pouze systém by měl nastavit stav **Suspended**.

Automatické upgrade používá stavu připojení Azure AD pro upgrade infrastrukturu. Pro automatické upgrade pracovat zkontrolujte, že jste otevřeli adresy URL v proxy serveru pro **Stav připojení Azure AD** jak je uvedeno v [Office 365 URL a rozsahy IP adres](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

Pokud **Správce synchronizace služby** UI běží na serveru, se pozastaví upgradu, dokud nezavřete uživatelského rozhraní.

## <a name="troubleshooting"></a>Řešení potíží
Pokud svoji instalaci připojit upgrade samotné podle očekávání, postupujte takto chcete zjistit, co může být špatně.

Nejdřív Neočekávejte automatické inovace pokoušet první den, který vydána nová verze. Před pokusem upgrade tak není sleduje výstražné zařízení, pokud není instalace hned upgradovat není záměrný náhodnosti.

Pokud se myslíte, že něco není v pořádku, nejdřív spusťte `Get-ADSyncAutoUpgrade` být povolena automatický upgrade.

Ujistěte se, že jste otevřeli požadované URL v proxy nebo brány firewall. Automatická aktualizace používá stavu připojení Azure AD podle popisu v části [Přehled](#overview). Pokud používáte proxy server, zkontrolujte, že stav nakonfigurovaný na použití [proxy serveru](active-directory-aadconnect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy). Vyzkoušejte taky [stavu připojení](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service) k Azure AD.

S připojením k Azure AD ověřené je čas podívat protokoly událostí. Spusťte Prohlížeč událostí a podívejte se v protokolu událostí **aplikace** . Přidání filtr událostí pro tento zdroj **Upgrade Azure AD připojení** a události id oblasti **300 399**.  
![Protokol událostí filtr pro automatické upgrade](./media/active-directory-aadconnect-feature-automatic-upgrade/eventlogfilter.png)  

Teď můžete vidět související se stavem automatické upgrade protokoly událostí.  
![Protokol událostí filtr pro automatické upgrade](./media/active-directory-aadconnect-feature-automatic-upgrade/eventlogresult.png)  

Kód výsledku má předponu u přehledu informací o stavu.

Výsledkem kódu předpony | Popis
--- | ---
Úspěch | Instalace byla úspěšně upgradovat.
UpgradeAborted | Dočasné podmínku zastavit upgradu. Bude znovu opakované a očekává se, že mu později.
UpgradeNotSupported | Systém má konfiguraci, která blokuje systému automaticky aktualizovány. Zopakuje zobrazíte-li změnit stav, ale očekává se, jestli chcete systém musíte upgradovat ručně.

Tady je seznam nejběžnějších zpráv, které můžete najít. Neobsahuje seznam všech, ale zprávy výsledek by měl být zrušte zaškrtnutí políčka u jaký problém je.

Výsledek zprávy | Popis
--- | ---
**UpgradeAborted** |
UpgradeAbortedCouldNotSetUpgradeMarker | Nelze zapisovat do registru.
UpgradeAbortedInsufficientDatabasePermissions | Předdefinované skupiny administrators nemá oprávnění k databázi. Ruční upgradujte na nejnovější verzi Azure AD Connect při řešení tohoto problému.
UpgradeAbortedInsufficientDiskSpace | Není dost místa na disku podpory upgradu.
UpgradeAbortedSecurityGroupsNotPresent | Nelze najít a vyřešit všechny skupiny zabezpečení používat modul synchronizace.
UpgradeAbortedServiceCanNotBeStarted | NT služby **Microsoft Azure AD Sync** se nepodařilo spustit.
UpgradeAbortedServiceCanNotBeStopped | Ukončení NT služby **Microsoft Azure AD Sync** se nepovedlo.
UpgradeAbortedServiceIsNotRunning | Není spuštěná služba NT **Microsoft Azure AD Sync** .
UpgradeAbortedSyncCycleDisabled | Možnost SyncCycle v [Plánovač](active-directory-aadconnectsync-feature-scheduler.md) byl zakázán.
UpgradeAbortedSyncExeInUse | [Synchronizace služby UI](active-directory-aadconnectsync-service-manager-ui.md) je otevřít na serveru.
UpgradeAbortedSyncOrConfigurationInProgress | Spuštění Průvodce instalací nebo k synchronizaci naplánovaný mimo plánovač.
**UpgradeNotSupported** |
UpgradeNotSupportedCustomizedSyncRules | Konfigurace přidáte vlastní pravidel.
UpgradeNotSupportedDeviceWritebackEnabled | Povolili jste funkci [zpětného zápisu zařízení](active-directory-aadconnect-feature-device-writeback.md) .
UpgradeNotSupportedGroupWritebackEnabled | Povolili jste funkci [zpětným skupiny](active-directory-aadconnect-feature-preview.md#group-writeback) .
UpgradeNotSupportedInvalidPersistedState | Instalace není nastavení Express nebo DirSync upgradovat.
UpgradeNotSupportedMetaverseSizeExceeeded | Máte víc než 100 000 objektů v metaverse.
UpgradeNotSupportedMultiForestSetup | Se připojujete k více doménové. Expresní nastavení pouze připojení k jedné doménové.
UpgradeNotSupportedNonLocalDbInstall | Nepoužíváte databázi SQL serveru Express LocalDB.
UpgradeNotSupportedNonMsolAccount | [Spojnice AD účtu](active-directory-aadconnect-accounts-permissions.md#active-directory-account) už není výchozí MSOL_ účet.
UpgradeNotSupportedStagingModeEnabled | Server je nastavená v [pracovní režimu](active-directory-aadconnectsync-operations.md#staging-mode).
UpgradeNotSupportedUserWritebackEnabled | Povolili jste funkci [zpětným uživatele](active-directory-aadconnect-feature-preview.md#user-writeback) .

## <a name="next-steps"></a>Další kroky
Další informace o [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).
