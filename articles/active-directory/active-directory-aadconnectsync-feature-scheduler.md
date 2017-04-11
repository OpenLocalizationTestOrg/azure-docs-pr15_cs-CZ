<properties
   pageTitle="Azure AD Connect synchronizace: Plánovač | Microsoft Azure"
   description="Toto téma popisuje funkce předdefinované Plánovač v Azure AD Connect synchronizovat."
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
   ms.date="08/04/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-scheduler"></a>Azure AD Connect synchronizace: Plánovač
Toto téma popisuje předdefinované Plánovač při synchronizaci Azure AD Connect (také synchronizace webu).

Tato funkce byla zavedená s sestavení 1.1.105.0 (vydána února 2016).

## <a name="overview"></a>Základní informace
Azure AD Connect synchronizace bude synchronizovat změny děje v místním adresáři pomocí plánovač. Existují dva Plánovač procesy, jedno pro synchronizaci hesel a další pro synchronizaci objektů a atributů a údržbu. Toto téma se zabývat těmito oblastmi ten.

V dřívějších verzích byl externí stroji synchronizace Plánovač pro objekty a atributy a Plánovač úloh systému Windows nebo samostatná služba Windows byl použit spustit synchronizaci. Plánovač je s předdefinované 1.1 vydáních k synchronizace modulu a umožňují některé vlastní nastavení. Nový výchozí četnost synchronizace je 30 minut.

Plánovač je zodpovědný za dva úkoly:

- **Synchronizace cyklické**. Proces importu, synchronizace a exportu změny.
- **Údržbu**. Obnovení klíče a certifikáty pro resetování hesla a zařízení registrace služby (DRS). Vymazání starých položek v protokolu operace.

Plánovač samotné vždy běží, ale lze spustit pouze jednu nebo žádný z těchto úkolů. Například pokud potřebujete mít vlastní synchronizaci obrázku, můžete zakázat tento úkol v Plánovač, ale pořád spuštění úlohy správy.

## <a name="scheduler-configuration"></a>Konfigurace Plánovač
Pokud chcete zobrazit aktuální nastavení, přejděte na prostředí PowerShell a spustit `Get-ADSyncScheduler`. Zobrazí se přibližně takto:

![GetSyncScheduler](./media/active-directory-aadconnectsync-feature-scheduler/getsynccyclesettings.png)

Pokud **není k dispozici příkaz Synchronizovat nebo rutiny** při spuštění tuto rutinu, modulu PowerShell nenačítá. Tato situace může nastat při spuštění Azure AD Connect u řadiče domény nebo na serveru s vyšší PowerShell omezení než výchozího nastavení. Pokud se zobrazí tato chyba, spusťte `Import-Module ADSync` zpřístupnit rutiny.

- **AllowedSyncCycleInterval**. Azure AD nejčastěji vám umožní synchronizace problémy. Nemůžete synchronizovat častěji a dál nebude podporovat.
- **CurrentlyEffectiveSyncCycleInterval**. Plán v současné době efekt. Bude mít stejnou hodnotu jako CustomizedSyncInterval (Pokud nastavit) Pokud ještě není častější než AllowedSyncInterval. Pokud změníte CustomizedSyncCycleInterval, to se projeví po další synchronizaci obrázku.
- **CustomizedSyncCycleInterval**. Pokud chcete plánovač spustit frekvencí jakékoli jiné než výchozí 30 minut, bude toto nastavení. Na obrázku výše Plánovač je nastaveno provádět každou hodinu. Pokud má argument to hodnotou nižší než AllowedSyncInterval, ten se použijí.
- **NextSyncCyclePolicyType**. Delta nebo iniciála. Určuje, zda dalším spuštění měli pouze proces delta změny, nebo pokud dalším spuštění má udělat úplné importovat a synchronizovat, které by také znovu zpracovat novými a upravenými pravidla.
- **NextSyncCycleStartTimeInUTC**. Při příštím Plánovač se spustí další synchronizaci obrázku.
- **PurgeRunHistoryInterval**. Čas by měly být neustále protokoly operací. Tyto si prohlédnout ve Správci služby synchronizace. Ve výchozím nastavení je zachovat tyto dobu 7 dní.
- **SyncCycleEnabled**. Označuje, jestli Plánovač je spuštěný Průvodce importem, synchronizace a procesy exportovat jako součást jeho operace.
- **MaintenanceEnabled**. Zobrazí, když je povolené procesu údržbu. Bude aktualizovat certifikáty/klíče a vymazat protokol operace.
- **IsStagingModeEnabled**. Zobrazí, když je povolené [přípravu režimu](active-directory-aadconnectsync-operations.md#staging-mode) .

Můžete změnit některé nastavení s `Set-ADSyncScheduler`. Můžete změnit následující parametry:

- CustomizedSyncCycleInterval
- NextSyncCyclePolicyType
- PurgeRunHistoryInterval
- SyncCycleEnabled
- MaintenanceEnabled

Konfigurace Plánovač je uložena v Azure AD. Pokud máte pracovní server, všechny změny na primárním serveru také efektem pracovní server (s výjimkou IsStagingModeEnabled).

### <a name="customizedsynccycleinterval"></a>CustomizedSyncCycleInterval
Syntaxe:`Set-ADSyncScheduler -CustomizedSyncCycleInterval d.HH:mm:ss`  
d - dny, [HH - hodin, mm – minut, ss – sekund

Příklad:`Set-ADSyncScheduler -CustomizedSyncCycleInterval 03:00:00`  
Dojde ke změně plánovač spustit každé 3 hodiny.

Příklad:`Set-ADSyncScheduler -CustomizedSyncCycleInterval 1.0:0:0`  
Dojde ke změně Plánovač se spouštějí denně.

## <a name="start-the-scheduler"></a>Spustit Plánovač
Plánovač ve výchozím nastavení spustí každých 30 minut. V některých případech můžete chtít spustit obrázku synchronizace mezi plánovaná cyklů nebo potřebujete k spuštění jiný typ.

**Delta synchronizace obrázku**  
Synchronizace obrázku delta obsahuje následující kroky:

- Delta importovat na všechny spojnice
- Delta synchronizovat přes všechny spojnice
- Export na všechny spojnice

Je možné, že máte naléhavé změnit, které je třeba synchronizovat okamžitě a proto je potřeba ručně spustit obrázku. Pokud je potřeba ručně spustit cyklu, potom z prostředí PowerShell spusťte `Start-ADSyncSyncCycle -PolicyType Delta`.

**Úplné synchronizaci obrázku**  
Pokud jste zadali jednu z následujících změny konfigurace, potřebujete k spuštění úplnou synchronizaci obrázku (také Počáteční):

- Přidání více objektů nebo atributy pro import ze zdrojového adresáře
- Změny provedené v pravidel synchronizace
- Změnit tak, aby rozdílný počet objektů by měl být součástí [filtrování](active-directory-aadconnectsync-configure-filtering.md)

Pokud jste zadali jednu z těchto změn, je potřeba spustit úplné synchronizaci obrázku tak, aby modul synchronizace má příležitost znovu sloučíte mezery spojnice. Úplné synchronizaci obrázku zahrnuje tyto kroky:

- Úplný Import na všechny spojnice
- Úplné synchronizovat přes všechny spojnice
- Export na všechny spojnice

Zahajte úplné synchronizaci obrázku, spusťte `Start-ADSyncSyncCycle -PolicyType Initial` z příkazovém řádku prostředí PowerShell. Tím se spustí úplné synchronizaci obrázku.

## <a name="stop-the-scheduler"></a>Ukončení Plánovač
Pokud plánovač právě probíhá cyklu synchronizace bude potřeba ukončit. Pokud například spusťte Průvodce instalací a zobrazí se tato chyba:

![SyncCycleRunningError](./media/active-directory-aadconnectsync-feature-scheduler/synccyclerunningerror.png)

Při spuštění synchronizace obrázku nelze měnit konfigurace. Může Počkejte, dokud Plánovač dokončení procesu, ale můžete taky ukončit ji tak, aby se okamžitě proveďte požadované změny. Zastavení aktuální obrázku není nebezpečnými a změny stále není zpracována budou zpracovány s dalším spuštění.

1. Začněte tím, že oznámením Plánovač zastavit své aktuální obrázku s rutiny prostředí PowerShell `Stop-ADSyncSyncCycle`.
2. Zastavení Plánovač neukončí konektoru aktuální z aktuálního úkolu. Vynutit spojnice tak, aby ukončit, provést tyto akce: ![StopAConnector](./media/active-directory-aadconnectsync-feature-scheduler/stopaconnector.png)
    - Spuštění **Synchronizace klienta služby** z nabídky start. Přejděte ke **spojnicím**, zvýraznění konektoru stavem **spuštěný** a vyberte akce **Zastavit** .

Plánovač je stále aktivní a bude na nejbližší příležitosti zase spustí.

## <a name="custom-scheduler"></a>Vlastní plánovač
Rutiny pro uvedených v této části jsou pouze k dispozici v sestavení [1.1.130.0](active-directory-aadconnect-version-history.md#111300) a vyšší.

Pokud předdefinované Plánovač nevyhovuje vašim požadavkům, můžete naplánovat spojnic pomocí Powershellu.

### <a name="invoke-adsyncrunprofile"></a>Vyvolání ADSyncRunProfile
Profil můžete začít spojnice tímto způsobem:

```
Invoke-ADSyncRunProfile -ConnectorName "name of connector" -RunProfileName "name of profile"
```

Názvy pro účely [profilu](active-directory-aadconnectsync-service-manager-ui-connectors.md#configure-run-profiles) [spojnice názvy](active-directory-aadconnectsync-service-manager-ui-connectors.md) a spustit najdete v [Synchronizaci služby UI](active-directory-aadconnectsync-service-manager-ui.md).

![Vyvolání spuštění profilu](./media/active-directory-aadconnectsync-feature-scheduler/invokerunprofile.png)  

`Invoke-ADSyncRunProfile` Rutina je synchronní, tedy nevrátí ovládacího prvku, dokud spojnice dokončení operace úspěšně nebo zobrazí se chybová zpráva.

Při plánování spojnice doporučujeme naplánovat v následujícím pořadí:

1. (Celé nebo Delta) Import z místního adresáře, například Active Directory
2. (Celé nebo Delta) Import z Azure AD
3. (Celé nebo Delta) Synchronizace z místního adresáře, například Active Directory
4. (Celé nebo Delta) Synchronizace z Azure AD
5. Export do Azure AD
6. Export do místního adresáře, například Active Directory

Pokud se podíváte na předdefinované Plánovač, je to pořadí, ve kterém se spustí spojnice.

### <a name="get-adsyncconnectorrunstatus"></a>Get-ADSyncConnectorRunStatus
Můžete také sledovat modul Synchronizace vidět – Pokud je zaneprázdněn nebo nečinná. Tuto rutinu vrátí prázdný výsledek, pokud modul Synchronizace nečinnosti a neběží spojnice. Pokud je spuštěná spojnici, vrátí název spojnice.

```
Get-ADSyncConnectorRunStatus
```

![Stav spuštění spojnice](./media/active-directory-aadconnectsync-feature-scheduler/getconnectorrunstatus.png)  
Na obrázku nahoře je první řádek ze státu, kde je nečinný modul synchronizace. Na druhém řádku ze kdy konektoru Azure AD běží.

## <a name="scheduler-and-installation-wizard"></a>Průvodce Plánovač a instalace
Pokud spustíte Průvodce instalací, pak Plánovač bude dočasně pozastaví. Důvodem je implicitně se dosadí provedete změny konfigurace a tyto nemohou být použity pokud modul Synchronizace aktivně systém. Z tohoto důvodu nenechávejte Průvodce instalací otevřít od přestane modul synchronizace provádět žádné akce synchronizace.

## <a name="next-steps"></a>Další kroky
Další informace o konfiguraci [Azure AD Connect synchronizovat](active-directory-aadconnectsync-whatis.md) .

Další informace o [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).
