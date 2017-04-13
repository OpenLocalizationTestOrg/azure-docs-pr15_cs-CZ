<properties
    pageTitle="Podporované platformy migrace IaaS zdrojů z klasického správci zdrojů Azure | Microsoft Azure"
    description="Tento článek provede podporované platformy migrace zdrojů z klasického správci zdrojů Azure"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="kasing"/>

# <a name="platform-supported-migration-of-iaas-resources-from-classic-to-azure-resource-manager"></a>Podporované platformy migrace IaaS zdrojů z klasického správci zdrojů Azure

V tomto článku jsme popisují, jak jsme jste povolení migrace infrastruktury jako zdroje služby (IaaS) od klasickou modelů nasazení Správce prostředků. Další informace o [Správce prostředků Azure funkce a výhody](../azure-resource-manager/resource-group-overview.md). Jsme podrobností způsobu připojení zdroje ze dvou nasazení modelů, které společně existovat v předplatném pomocí virtuální sítě – sítěmi brány. 

## <a name="goal-for-migration"></a>Cíl z hlediska migrace

Správce prostředků umožňuje nasazení složitých aplikací pomocí šablon, nakonfiguruje virtuálních počítačích pomocí OM rozšíření a zahrnuje správu přístupu a značky. Azure správce prostředků obsahuje scalable, paralelní nasazení virtuálních počítačích do sad dostupnosti. Nový model nasazení obsahuje také Správa životního cyklu výpočetním, sítě a úložiště nezávisle na sobě. Nakonec je fokus na ve výchozím nastavení se vynucení virtuálních počítačích v síti virtuální povolení zabezpečení.

Skoro všechny funkce verze z klasické nasazení modelu jsou podporovány u výpočetním, sítě a úložiště v části správce prostředků Azure. Využívat nové funkce v Azure správce, můžete migrovat stávající nasazení z klasické nasazení modelu.

## <a name="changes-to-your-automation-and-tooling-after-migration"></a>Změny automatizaci a nástrojů po migraci

V rámci migrace zdrojů z nasazení modelu Klasický nasazení modelu správce prostředků budete muset aktualizovat existující automatizaci nebo nástrojů zajistit, že ho bude dál fungovat i po migraci.

## <a name="meaning-of-migration-of-iaas-resources-from-classic-to-resource-manager"></a>Význam migrace IaaS zdrojů z klasického správci zdrojů

Před jsme přecházet na podrobnější podrobnosti, Podívejme se na rozdíl mezi rovině dat a správa rovině operace IaaS zdroje.

- *Správa rovině* popisuje hovory, které jsou do rovině správy nebo rozhraní API pro úpravu zdroje. Například operací, jako je vytvoření virtuálního počítače, restartování virtuálního počítače a aktualizace virtuální sítě s novou podsítí spravovat pracovního zdroje. Neovlivňují přímo připojení k instance.
- *Rovině dat* (aplikace) popisuje runtime samotnou aplikaci a zahrnuje interakce s instance, které nechcete absolvovat rozhraní API Azure. Přístup k webu nebo načítat data z instanci systému SQL Server nebo serveru MongoDB pokládán za dat interakce ploché nebo aplikací. Kopírování objektů blob z účtu úložiště a přístup k veřejnou IP adresu RDP nebo SSH do virtuálního počítače jsou také dat rovině. Tyto operace zachovat aplikace ve výpočetním, sítě a úložiště.

>[AZURE.NOTE] V některých případech migrace Azure platformu přestane zruší přidělení a restartování virtuálních počítačích. Tím se poněkud krátké prostoje datové rovině.

## <a name="supported-scopes-of-migration"></a>Podporované obory migrace

Existují tři obory migrace primárně zaměřených výpočetním, sítě a úložiště. 

### <a name="migration-of-virtual-machines-not-in-a-virtual-network"></a>Migrace virtuálních počítačích (ale ne v virtuální síť)

V části model nasazení Správce prostředků vynucování zabezpečení aplikace ve výchozím nastavení. Všechny VMs musí být v modelu správce prostředků virtuální síť. Restartování Azure platformu (`Stop`, `Deallocate`, a `Start`) VMs jako součást migrace. Máte dvě možnosti, virtuální sítě:

- Můžete požádat o platformu a vytvořte novou virtuální síť migrovat virtuálního počítače do nového virtuální sítě.
- Virtuální počítač můžete migrovat do existující virtuální síť ve Správci zdrojů.

>[AZURE.NOTE] V tomto oboru migrace operace správy rovině a operace datové rovině neumožňují dobu průběhu migrace.

### <a name="migration-of-virtual-machines-in-a-virtual-network"></a>Migrace virtuálních počítačích (v síti virtuální)

U většiny konfigurací OM pouze metadata migrace mezi modely nasazení klasické a správce prostředků. Základní VMs běží na stejné hardwaru, ve stejné síti a s stejné úložiště. Operace řízení plochy nemusí povolené pro určitou dobu během migrace. Však rovině dat bude dál fungovat. To znamená aplikace spuštěné nad VMs (klasické) ušetří prostoje během migrace.

Následující konfigurace aktuálně nepodporuje. Pokud je přidána podpora v budoucnu některé VMs v této konfiguraci může vzniknou prostoje (přejděte pomocí tabulátoru, zrušit a restartujte OM operace).

-   Máte víc než jeden dostupnost nastavení v jeden cloudové službě.
-   Máte jednu nebo více dostupnost sady a VMs, které nejsou v nastavení v jeden Cloudová služba dostupné.

>[AZURE.NOTE] V tomto oboru migrace rovině správy neumožňují dobu průběhu migrace. U některých konfigurace jak bylo popsáno dříve, nastane prostoje datové rovině.

### <a name="storage-accounts-migration"></a>Migrace účty úložiště

Umožňuje bezproblémové migrace nástroje můžete nasazovat správce prostředků VMs účet klasické úložiště. Pro tuto funkci pro využití a síťové zdroje můžete a měli k poštovním nezávisle na úložiště účty. Po migraci na virtuálních počítačích a virtuální sítě budete muset migrovat přes účtů úložiště při dokončování procesu migrace. 

>[AZURE.NOTE] Správce prostředků nasazení modelu nemá koncepci klasické obrázky a discích. Pokud účet úložiště je migrované klasické obrázky a disků nejsou zobrazeny ve vrstvě správce prostředků však zálohování VHD zůstanou v účtu úložiště. 

## <a name="unsupported-features-and-configurations"></a>Nepodporované funkce a konfigurace

Nepodporujeme aktuálně některé funkce a konfigurace. Následující oddíly popisují Naše doporučení okolo nich.

### <a name="unsupported-features"></a>Nepodporované funkce

Tyto funkce nejsou podporované aktuálně. Můžete případné odebrání tato nastavení, migrace VMs a znovu povolit nastavení v modelu nasazení Správce prostředků.

Zprostředkovatele prostředků | Funkce
---------- | ------------
Výpočet | Disků nepřidružený virtuálního počítače.
Výpočet | Virtuální počítač obrázky.
Sítě | Koncový bod ACL.
Sítě | Virtuální sítě brány (sítěmi, Azure ExpressRoute aplikace brány, přejděte na web).
Sítě | Použití VNet prozkoumávání virtuální sítě. (Migrace VNet do ARM, a pak druhé strany) Další informace o [VNet prozkoumávání] (... /Virtual-Network/Virtual-Network-peering-Overview.MD).
Sítě | Profily přenosy správce.

### <a name="unsupported-configurations"></a>Nepodporované konfigurace

Následující konfigurace aktuálně nepodporuje.

Služba | Konfigurace | Doporučení
---------- | ------------ | ------------
Správce prostředků | Role na základě přístup ovládacího prvku (RBAC) pro klasické zdroje | Protože URI zdroje se mění po migraci, je vhodné plán aktualizace RBAC zásad, které potřebujete k tomu může dojít po migraci.
Výpočet | Více podsítí přidružené virtuálního počítače | Aktualizujte konfigurace podsítě neodkazuje pouze podsítí.
Výpočet | Virtuálních počítačích, které patří virtuální síť, ale nemáte explicitní podsítě přiřazené | Volitelně můžete odstranit OM.
Výpočet | Virtuálních počítačích, které mají upozornění, zásady automatické měřítko | Migrace prochází a toto nastavení se nezobrazí. Důrazně doporučujeme vyhodnocení prostředí dříve, než se migrace. Můžete taky můžete změnit konfiguraci nastavení upozornění po dokončení migrace.
Výpočet | Rozšíření OM XML (BGInfo 1.* Visual Studio ladění, nasazení webu a vzdálené ladění) | Toto není podporovaná. Doporučujeme odebrat následujících přípon z počítače virtuální pokračujte migrace nebo jejich dojde ke ztrátě automaticky během procesu migrace.
Výpočet | Spuštění diagnostiky s úložištěm Premium | Zakázání funkce spouštěcí Diagnostika pro VMs před pokračováním migrace. Po dokončení migrace můžete znovu povolit spuštění Diagnostika v sadě Správce prostředků. Objekty BLOB použité pro snímek a sériové protokoly je třeba navíc zrušit tak, aby vám bude účtovaná už pro tyto objekty BLOB.
Výpočet | Cloudové služby, které obsahují role webu nebo pracovního | Toto není aktuálně podporován.
Sítě | Virtuální sítě, které obsahují virtuálních počítačích a webové či pracovníka role |  Toto není aktuálně podporován.
Azure aplikace služby | Virtuální sítě, které obsahují prostředí aplikace služby | Toto není aktuálně podporován.
Azure HDInsight | Virtuální sítě, které obsahují HDInsight služby | Toto není aktuálně podporován.
Služby Microsoft Dynamics životního cyklu | Virtuální sítě, které obsahují virtuálních počítačích, které jsou spravovány Dynamics životního cyklu služby | Toto není aktuálně podporován.
Výpočet | Centrum zabezpečení Azure rozšíření s VNET, která má brána VPN a ER bráně pro místní DNS server | Centrum zabezpečení Azure automaticky nainstaluje rozšíření virtuálních počítačích a sledovat jejich zabezpečení vysokoškolskou úroveň upozornění. Tato rozšíření obvykle nainstalovat automaticky Pokud Centrum zabezpečení Azure je povolena u předplatného. Migrace brány není aktuálně podporován a brány je potřeba odstranit před pokračováním potvrzování migrace, přístup k Internetu k účtu úložiště OM budou ztraceny při odstranění brány. Migrace nezačne důvody tohoto chování jako nelze vyplněné objektů blob hosta agent stavu. Doporučuje se zásada Centrum zabezpečení Azure předplatným 3 hodiny před pokračováním migrace.

## <a name="the-migration-experience"></a>Možnosti migrace

Než začnete prostředí migrace, je vhodné takto:

- Ujistěte se, že prostředky, které chcete migrovat nepoužívejte jakékoli konfigurace nebo nepodporované funkce. Obvykle platformu rozpozná tyto problémy a dojde k chybě.
- Pokud máte VMs, které nejsou v síti virtuální se zastaví a uvolnit jako součást operace připravit. Pokud nechcete ztratíte veřejnou IP adresu, podívejte se do rezervaci na IP adresu před aktivací operaci připravit. Ale pokud VMs jsou v síti virtuální, nejsou přerušili a odebrána.
- Plánování migrace není pracovní dobu tak, aby zahrnoval pro všechny neočekávané chyby, ke kterým může dojít během migrace.
- Stáhněte si aktuální konfiguraci vašeho VMs pomocí prostředí PowerShell rozhraní příkazového řádku (rozhraní příkazového řádku) příkazy a rozhraní REST API usnadňuje ověřovacích po dokončení kroku připravit.
- Aktualizujte skriptů automatizaci/operationalization zpracovávání nasazení modelu správce prostředků před zahájením migrace. Volitelně můžete udělat GET operace při zdroje jsou připravené stav.
- Vyhodnocení RBAC zásad, které jsou nakonfigurovaný na klasické IaaS zdroje a plán pro po dokončení migrace.

Proces migrace vypadá takto

![Snímek obrazovky ukazující migrace pracovního postupu](./media/virtual-machines-windows-migration-classic-resource-manager/migration-workflow.png)

>[AZURE.NOTE] Všechny operace popsaných v následujících částech jsou idempotent. Pokud máte problém než s nepodporovanou funkcí nebo Chyba konfigurace, je vhodné opakovat připravit, zrušit nebo potvrdit operace. Azure platformu se snaží akci.

### <a name="validate"></a>Ověření

Operace ověřit je cílem prvního kroku v procesu migrace. Cílem tento krok je analýza dat v pozadí u zdrojů v části migrace a vrátí úspěch/selhání, pokud zdroje jsou může migrace.

Vyberete virtuální sítě nebo hostovanou službu (Pokud není virtuální sítě), které chcete ověřit migraci.

* Pokud zdroje nedokáže migrace, Azure platformu uvádí všechny důvody proč není podporována pro migraci.

### <a name="prepare"></a>Příprava

Příprava operace je druhým krokem při procesu migrace. Cílem tento krok je simulovat transformace IaaS zdrojů z klasického pracovníkům správce prostředků a prezentovat vedle sebe u vizualizovat.

Vyberete virtuální sítě nebo hostovanou službu (Pokud není virtuální sítě), že chcete Příprava migraci.

* Pokud zdroje nedokáže migrace, Azure platformu zastaví procesu migrace a seznamy důvod, proč připravit se nezdařila.
* Pokud zdroj se může migrace, uzamčení prvního Azure platformu dolů operace rovině správy zdrojů v části migrace. Například nejste moct disk dat do OM v části migrace.

Azure platformu potom začne migrace metadat z klasického správci zdrojů migrace zdrojů.

Po dokončení operace připravit máte možnost vizualizace zdrojů v obou klasické a správce prostředků. Pro každou službu cloudu v modelu klasické nasazení Azure platformu vytvoří zdroje název skupiny, který má vzorek `cloud-service-name>-migrated`.

>[AZURE.NOTE] Virtuálních počítačích, které nejsou v síti klasické virtuální jsou ukončit zrušeny, takže v této fázi migrace.

### <a name="check-manual-or-scripted"></a>Kontrola (ručně nebo skriptů)

V kroku zaškrtnutí Volitelně můžete konfigurace, který jste stáhli dříve k ověření, že migrace správné. Můžete taky můžete přihlásit k portálu a kontrola vlastností a prostředky k ověření dobrá metadat migrace.

Při migraci virtuální sítě, není restartování většina konfigurace virtuálních počítačích. Pro aplikace v těchto VMs můžete ověřit, že aplikace je pořád začátcích.

Můžete otestovat sledování/automatizace a provozní skripty VMs pracují podle očekávání a aktualizované skripty fungovat správně. Pouze operace GET jsou podporované, když zdroje jsou připravené stav.

Neexistuje žádné nastavení časového intervalu před který budete muset potvrdit migrace. Může trvat tak dlouho chcete v tomto stavu. Však rovině správy Zamčený pro tyto materiály, dokud zrušit nebo potvrdit.

Pokud se zobrazí všech problémů, můžete vždy přerušit migrace a vraťte se do modelu klasické nasazení. Po vrátit zpátky, Azure platformu otevře se operace správy rovině o zdrojích, aby obnovit normálně na tyto VMs v modelu klasické nasazení.

### <a name="abort"></a>Přerušení

Přerušení krok je volitelný využívající vrátit změny modelu klasické nasazení a zastavení migrace.

>[AZURE.NOTE] Tuto operaci nelze provést po mít aktivaci operace potvrzení.  

### <a name="commit"></a>Potvrzení

Po dokončení ověřování, je potvrdit migrace. Zdroje se už nezobrazují v klasickém a jsou k dispozici pouze v modelu nasazení Správce prostředků. Dá se ovládat migrované prostředky pouze v novém portálu.

>[AZURE.NOTE] Toto je operaci idempotent. Pokud se nezdaří, je vhodné opakovat operaci. Pokud budou dál selhání, požadavek podpory můžete vytvořit nebo příspěvek ve fóru klíčovým slovem ClassicIaaSMigration na naše [OM fórum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WAVirtualMachinesforWindows).

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

**Tento plán migrace vliv některou existující služby nebo aplikace spuštěné v operačním systému Azure virtuálních počítačích?**

Ne. VMs (klasické) jsou plně podporované služby v všeobecně dostupná. Můžete dál používat tyto materiály rozbalte nároky na Microsoft Azure.

**Co se stane na můj VMs můžu nemáte v plánu přenést nevidět?**

Jsme nejsou platnosti existující klasické rozhraní API a model prostředků. Chcete upozornit migrace zvažovat pokročilé funkce, které jsou k dispozici v modelu nasazení Správce prostředků. Doporučujeme, abyste si prošli [některé zlepšení](virtual-machines-windows-compare-deployment-models.md) , které jsou součástí IaaS v části správce prostředků.

**Co znamená tento plán migrace pro svůj stávající nástrojů?**

Aktualizace vaší nástrojů Správce prostředků nasazení modelu je jedním ze všech nejdůležitější změny, které je potřeba počítat v plánech migrace.

**Jak dlouho bude výpadku správy rovině k?**

To záleží na počet zdrojů převáděných. Menší nasazení (několik desítky VMs) měli celé migrace trvat menší než za hodinu. Rozsáhlé nasazení (stovky VMs) může migrace trvat několik hodin.

**Můžete vrátit zpět po migraci zdrojů jsou potvrzené ve Správci zdrojů?**

Můžete ji zrušit migraci, dokud zdroje jsou připravené stav. Vrácení zpět není podporována po zdrojů prostřednictvím operace potvrzení úspěšně migraci.

**Můžu vrátit Moje migrace když operace potvrzení selže?**

Migrace nelze přerušit, pokud se nezdaří potvrzení. Jsou všechny operace migrace, včetně operace potvrzení idempotent. Proto doporučujeme opakujte akci za chvíli. Pokud pořád obličej chybu vytvořit požadavek podpory můžete nebo příspěvek ve fóru slovem ClassicIaaSMigration na naše [OM fórum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WAVirtualMachinesforWindows).

**Je potřeba koupit jiné okruh express směrování máte použít IaaS v části správce prostředků?**

Ne. Naposledy povolené [Přesunutí ExpressRoute obvody od klasického nasazení modelu správce prostředků](../expressroute/expressroute-move.md). Nemusíte koupit nový okruh ExpressRoute, pokud ještě nemáte.

**Co když mám měli nakonfigurované zásady řízení přístupu na základě rolí pro klasické zdroje IaaS?**

V průběhu migrace zdrojů transformace od klasického správci zdrojů. Proto doporučujeme, abyste plán aktualizace RBAC zásad, které potřebujete k tomu může dojít po migraci.

**Co když používám obnovení webu Azure nebo zálohování Azure dnes?**

Migrace virtuálního počítače, které jsou povolené pro zálohování, přečtěte si téma [můžu vytvoření zálohy Moje klasické VMs záložní trezoru. Teď chcete migrovat Moje VMs z klasický režim do režimu správce prostředků. Jak můžu zálohovat jejich obnovení služby trezoru?](../backup/backup-azure-backup-ibiza-faq.md#i-have-backed-up-my-classic-vms-in-backup-vault-now-i-want-to-migrate-my-vms-from-classic-mode-to-resource-manager-mode-how-can-i-backup-them-in-recovery-services-vault)

**Můžete ověřit Moje předplatné nebo zdroje, chcete-li zobrazit, pokud se vám může migrace?**

Ano. V možnosti podporované platformy migrace prvním krokem při přípravě migrace je k ověření, že zdroje jsou může migrace. V případě, že ověření se nezdaří, dostáváte zprávy důvodů všechny migrace nelze dokončit.

**Co se stane, když mám nastat chyby kvóty při přípravě zdroje IaaS migrace?**

Doporučujeme přerušit migrace a potom se znovu přihlaste žádost o podporu zvětšíte kvóty v oblasti, kde migrujete VMs. Po schválení žádosti o kvóty můžete začít znova provádění kroky migrace.

**Jak mohu ohlásit problém?**

Vystavte problémy a otázky o migraci na naše [Fórum komunity OM](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WAVirtualMachinesforWindows), s ClassicIaaSMigration klíčového slova. Doporučujeme publikovat všechny dotazy v této Fórum komunity. Pokud máte smlouvu o podpoře, jste Vítá vás přihlásit požadavek podpory můžete taky.

**Co když mám nelíbí názvů zdrojů, které platformu rozhodli, že v průběhu migrace?**

Během migrace se zachovají všechny zdroje, které zadáte explicitně názvy v modelu klasické nasazení. V některých případech se vytvářejí nové zdroje. Příklad: síťové rozhraní se vytvoří pro každé OM. Aktuálně nepodporujeme možnost na názvy těchto nové prostředků vytvořené v průběhu migrace ovládacích prvků. Přihlaste se hlasů pro tuto funkci na [Fórum komunity Azure svůj názor](http://feedback.azure.com).

* *Máte zprávu *"OM hlásí celkový stav agent jako není připraveno. Proto nelze migrovat OM. Ujistěte se, že Agent OM hlásí celkový stav agent připravenost"* nebo *"OM obsahuje příponu stavem není hlášené z OM. Proto tato OM nelze migrovat."***

Tato zpráva se stavu, při OM nemá odchozí připojení k Internetu. Agent OM pomocí odchozí připojení k dosažení účet Azure úložiště pro aktualizace stavu agent každých pět minut.


## <a name="next-steps"></a>Další kroky
Poté, co seznámili migrace klasické IaaS zdroje pro správce zdrojů můžete spustit migraci zdrojů.

- [Technická hloubkové postupy na podporované platformy migrace z klasického správci zdrojů Azure](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
- [Použití Powershellu ke migrovat zdroje IaaS z klasické správci zdrojů Azure](virtual-machines-windows-ps-migration-classic-resource-manager.md)
- [Použití rozhraní příkazového řádku k migraci zdrojů IaaS od klasického správci zdrojů Azure](virtual-machines-linux-cli-migration-classic-resource-manager.md)
- [Klonovat klasické virtuálního počítače správci zdrojů Azure pomocí skriptů Powershellu komunity](virtual-machines-windows-migration-scripts.md)
