<properties 
   pageTitle="Co je StorSimple? | Microsoft Azure" 
   description="Popisuje StorSimple vrstvení, zařízení, virtuální zařízení, služby a Správa úložiště a uvádí klíčové termíny používané při StorSimple." 
   services="storsimple" 
   documentationCenter="NA" 
   authors="SharS" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD" 
   ms.date="10/05/2016"
   ms.author="v-sharos@microsoft.com"/>

# <a name="storsimple-8000-series-a-hybrid-cloud-storage-solution"></a>Řady StorSimple 8000: řešení hybridní cloudové úložiště

## <a name="overview"></a>Základní informace

Vítá vás Microsoft Azure StorSimple integrované úložiště řešení, která spravuje úložiště úkolů mezi místním zařízení a Microsoft Azure cloudového úložiště. StorSimple je efektivně, efektivní a snadno ovladatelným úložiště oblasti sítě SAN řešení, které eliminuje v mnoha problémů a náklady spojené s enterprise úložiště a ochranu dat. Použití vlastní řady zařízení StorSimple 8000, lze integrovat s cloudovým službám a k dispozici sadu Nástroje pro správu pro bezproblémové zobrazení všech enterprise úložiště, včetně cloudového úložiště. (Informace o nasazení StorSimple publikované na webu Microsoft Azure platí pro StorSimple 8000 řady jen zařízení. Pokud používáte zařízení StorSimple 5000/7000 řady, přejděte k [Nápovědě StorSimple](http://onlinehelp.storsimple.com/).)

StorSimple používá ke správě uložená data médium různých [vrstvení úložiště](#automatic-storage-tiering) . Aktuální pracovní sada je uložená místně na plnou stavu jednotkách (disky SSD), data, která se používá méně často uložený na pevných discích (pevných disků) a archivace dat se posune do cloudu. Kromě toho StorSimple používá deduplication a komprese zmenšit velikost úložiště, který využívá data. Další informace najdete v tématu [Deduplication a komprese](#deduplication-and-compression). Definice jiných klíčové termíny a pojmy použité v dokumentaci StorSimple 8000 řady přejděte na [StorSimple terminologie](#storsimple-terminology) na konci tohoto článku.

S StorSimple aktualizace 2 poznáte odpovídající objemy jako *místně připnuté* ověřit, že primární dat zůstane místní zařízení a není úroveň do cloudu. Umožňuje provádět úloh, které jsou citlivé na cloud latence, například SQL a virtuálního počítače úloh, místně připnuté objemů nadále používat cloudu pro zálohování. Další informace o místně připnuté svazky najdete v tématu [použití službu StorSimple správce pro správu svazky](storsimple-manage-volumes-u2.md). 

Aktualizace 2 umožňuje vytvářet StorSimple virtuální zařízení, která využít zhoršeným čekacích dob a vysoký výkon poskytovanou Azure premium úložiště. Další informace o StorSimple premium virtuální zařízení najdete v tématu [nasazení a správu StorSimple virtuální zařízení v Azure](storsimple-virtual-device-u2.md). Další informace o Azure premium úložiště, přejděte na [Premium úložiště: výkonné úložiště pro Azure virtuálního počítače úloh](../storage/storage-premium-storage.md).

Kromě Správa úložiště StorSimple funkce pro ochranu dat můžete vytvořit na vyžádání naplánované zálohy a potom ukládány místně nebo v cloudu. Zálohování přesměrováni formou přírůstková snímky, což znamená, že můžete být vytvořené a rychle obnovit. Snímky cloudu může být zásadně důležité v havárie obnovení scénáře, protože nahradit systémy sekundárním úložiště (například páskou zálohování) a umožňují dat do vašeho datacentra nebo alternativní weby v případě potřeby obnovit.

![Ikona video](./media/storsimple-overview/video_icon.png) Podívejte se video a získejte rychlý úvod k Microsoft Azure StorSimple.

> [AZURE.VIDEO storsimple-hybrid-cloud-storage-solution]

## <a name="why-use-storsimple"></a>Proč používat StorSimple?

Následující tabulka popisuje některé klíčové výhody, které poskytuje Microsoft Azure StorSimple.

| Funkce | Výhoda |
|---------|---------|
|Průhledný integrace | Microsoft Azure StorSimple používá protokol iSCSI tedy bez zásahu uživatele propojení dat úložiště zařízení. Uložený v cloudu v datacentru, zajistíte nebo na serverech remote se zobrazí uložený na jednom místě.|
|Omezená úložiště nákladů|Microsoft Azure StorSimple přidělí dostatečné místního nebo cloudového úložiště aktuální požadavkům a rozšiřuje cloudového úložiště pouze v případě potřeby. Další snižuje požadavky na úložiště a výdajů odstraněním nadbytečné verzích stejná data (deduplication) a pomocí komprese.|
|Správa zjednodušené úložiště|Microsoft Azure StorSimple poskytuje systém nástroje pro správu, které můžete použít ke konfiguraci a správě dat uložených v místním prostředí, na vzdálený server a v cloudu. Kromě toho můžete spravovat zálohování a obnovení funkce z modulu snap-in Microsoft Management Console (MMC). StorSimple poskytuje samostatný volitelné rozhraní, které slouží k rozšíření StorSimple řízení a data péče k obsahu uloženému na serverech SharePoint. |
|Vylepšené havárie obnovení a dodržování předpisů|Microsoft Azure StorSimple nevyžaduje rozšířené obnovení čas. Místo toho obnoví data, jako je to potřeba. To znamená, že normálně pokračovat s minimálními přerušení. Kromě toho můžete nakonfigurovat zásady určete záložní plány a uchovávání informací.
|Nastavení mobilních zařízení dat|Nahrané ke cloudovým službám společnosti Microsoft Azure je přístupný z jiných webů pro účely obnovení a migrace. Kromě toho můžete StorSimple konfigurace StorSimple virtuální zařízení na virtuálních počítačích (VMs) spuštěné v Microsoft Azure. VMs můžete používat virtuální zařízení pro přístup k uložená data pro účely test nebo obnovení.|
|Podpora pro jiné poskytovatele služby cloudu |Řady StorSimple 8000 softwarem aktualizace 1 nebo novější podporují Amazon S3 se záznamy, HP a OpenStack cloudové služby, jakož i Microsoft Azure. (Budete pořád potřebovat účet Microsoft Azure úložiště pro účely správy zařízení). Další informace přejděte na [Co je nového v 1.2 aktualizace](storsimple-update1-release-notes.md#whats-new-in-update-12).|
|Nepřerušený provoz | Aktualizace 1 nebo novější poskytuje novou funkci migrace, která umožňuje StorSimple 5000 7000 řady jejich data migrovat do zařízení StorSimple 8000 řady.|
|Dostupnost na portálu Azure Government | Je k dispozici v portálu pro státní správu Azure StorSimple aktualizace 1 nebo novější. Další informace najdete v tématu [nasazení zařízení místní StorSimple na portálu pro státní správu](storsimple-deployment-walkthrough-gov.md).|
|Ochrana dat a dostupnosti | Řady StorSimple 8000 s aktualizace 1 nebo novější podporuje zóny nadbytečné úložiště (ZRS), kromě místně nadbytečné úložiště (LRS) a Geo nadbytečné úložiště (GRS). Podívejte se do [tohoto článku o možnostech redundance úložišti Azure](https://azure.microsoft.com/documentation/articles/storage-redundancy/) podrobnosti ZRS.|
|Podpora pro důležité aplikace | S StorSimple aktualizace 2 poznáte odpovídající objemy místně připnuté. Tato možnost zaručuje, že údaje vyžadované důležité aplikace není vrstveny do cloudu. Místně připnuté svazky nejsou vyměřené poplatky za jeho čekacích dob cloudu nebo problémy s připojením. Další informace o místně připnuté svazky najdete v tématu [použití službu StorSimple správce pro správu svazky](storsimple-manage-volumes-u2.md).|
|Nízké latence a vysoký výkon | Aktualizace 2 StorSimple umožňuje vytváření virtuálních zařízení využívajících vysoký výkon zhoršeným latence funkce Azure premium úložiště. Další informace o StorSimple premium virtuální zařízení najdete v tématu [nasazení a správu StorSimple virtuální zařízení v Azure](storsimple-virtual-device-u2.md).|

![Ikona video](./media/storsimple-overview/video_icon.png) podívejte se na [Toto video](https://www.youtube.com/watch?v=4MhJT5xrvQw&feature=youtu.be) a získejte přehled o StorSimple 8000 řady funkce a výhody.

## <a name="storsimple-components"></a>Součásti StorSimple

Řešení Microsoft Azure StorSimple zahrnuje tyto prvky:

- **Zařízení Microsoft Azure StorSimple** – místní hybridní diskové pole, které obsahuje disky SSD a pevných disků, spolu s nadbytečné řadiče a automatické převzetí. Řadiče spravovat úložiště vrstvení, umístěním aktuálně používaných (nebo žádanou) dat v místním úložišti (v zařízení nebo místních serverů), při přesunování méně častých dat do cloudu.
- **Virtuální zařízení StorSimple** – označovaná taky jako StorSimple virtuální spotřebiče, je to verzi softwaru StorSimple zařízení, které zreplikuje architektura a většina funkcí úložné zařízení fyzicky hybridní. Virtuální zařízení StorSimple běží načítání v Azure virtuálního počítače. Virtuální zařízení Premium, která využít Azure premium úložiště, jsou k dispozici v aktualizaci 2 a vyšší.
- **Služba StorSimple správce** – rozšíření portálu Azure klasické, který umožňuje řízení StorSimple zařízení nebo StorSimple virtuální z jedné webové rozhraní. Vytvoření a správa služeb, zobrazení a správa zařízení, zobrazit upozornění, spravovat svazky a zobrazovat a spravovat záložní zásady a katalogu záložní můžete službu StorSimple správce.
- **Prostředí Windows PowerShell pro StorSimple** – rozhraní příkazového řádku, který slouží ke správě StorSimple zařízení. Prostředí Windows PowerShell pro StorSimple obsahuje funkce, které umožňují zaregistrovat zařízení StorSimple rozhraní network na svém zařízení nastavit, nainstalujte určitých typů aktualizace, Poradce při potížích s zařízení s přístupem k relaci podpory a změnit stav zařízení. Dostanete se k prostředí Windows PowerShell pro StorSimple připojení konzoly sériové nebo pomocí vzdálené prostředí Windows PowerShell.
- **Rutiny prostředí PowerShell StorSimple azure** – kolekce rutin prostředí Windows PowerShell, která umožňují automatizaci úrovni služeb a migrace úkoly z příkazového řádku. Další informace o rutinách Powershellu Azure StorSimple najdete [reference pro rutiny](https://msdn.microsoft.com/library/dn920427.aspx).
- **Správce snímek StorSimple** – modulu snap-in konzoly MMC používající hlasitost skupiny a službě Windows hlasitost stín kopie ke generování konzistenci aplikací zálohy. Kromě toho můžete StorSimple snímek správce vytvořit záložní plány a klonovat nebo obnovit svazky. 
- **StorSimple adaptér pro službu SharePoint** – nástroj, který transparentně rozšiřuje Microsoft Azure StorSimple úložiště a ochranu dat na SharePoint serverové farmy při vytváření StorSimple úložiště může zobrazit a spravovat z portálu Centrální správa SharePoint.

Následující diagram nabízí nejvyšší úrovně zobrazení Microsoft Azure StorSimple architektura a složek.

![StorSimple architektura](./media/storsimple-overview/overview-big-picture.png)

V následujících částech jsou popsány jednotlivé tyto součásti podrobněji a popisují, jak řešení uspořádá dat, přidělí úložiště a usnadňuje Správa úložiště a ochrana dat. V části poslední poskytuje definice některé důležité termíny a pojmy týkající se StorSimple součásti a jejich správy.

## <a name="storsimple-device"></a>StorSimple zařízení

Zařízení Microsoft Azure StorSimple je místní hybridní úložiště pole, které umožňuje primární úložiště a přístup k iSCSI dat uložených v něm. Spravuje komunikace s cloudového úložiště a pomáhají zajistit bezpečnost a důvěrnost všechna data, která jsou uložená na Microsoft Azure StorSimple řešení.

Zařízení StorSimple obsahuje disky SSD a pevných disků jednotky pevného disku, jakož i podpora clusterů a automatické převzetí. Obsahuje procesor s sdílených sdílené úložiště a dva zrcadlený řadiče. Každý řadiče přináší tyto možnosti:

- Připojení k počítači Host (hostitel)
- Až šest porty sítě se připojit k místní síti (LAN)
- Sledování hardwaru
- Bez stálých paměť (NVRAM), který zachovává informace, i když se přeruší power
- Shluk podporující aktualizace ke správě aktualizace softwaru na serverech překlopení obrázku tak, aby aktualizace minimální nebo žádný vliv na dostupnost služby
- Clusteru služba, která funguje jako back-end clusteru, poskytnutí dostupnost a minimalizace nežádoucí efekty, které může dojít, pokud pevný disk SSD selhává, nebo je v režimu offline

Jedinou řadiče je aktivní na libovolném místě v čase. Pokud aktivní řadiče nepovede, druhého řadiče aktivuje automaticky. 

Další informace přejděte na [StorSimple hardwarové součásti a stav](storsimple-monitor-hardware-status.md).

## <a name="storsimple-virtual-device"></a>Virtuální zařízení StorSimple

StorSimple slouží k vytváření virtuálních zařízení, které zreplikuje architektura a možnosti úložné zařízení fyzicky hybridní. Virtuální zařízení StorSimple (označovaná taky jako StorSimple virtuální spotřebiče) je spuštěn načítání v Azure virtuálního počítače. (Virtuální zařízení můžete vytvořit pouze na Azure virtuálního počítače. Nemůžete vytvořit jednu StorSimple zařízení nebo místního serveru.) 

Virtuální zařízení má následující funkce:

- Se chová jako fyzické zařízení a můžou nabízet iSCSI rozhraní virtuálních počítačích v cloudu. 
- Můžete vytvořit neomezený počet virtuální zařízení v cloudu a změnit jejich zapnutí a vypnutí podle potřeby. 
- Můžou pomoct simulovat místním prostředím havárie obnovení, vývoj a scénáře testování a můžou pomoct s načítání úrovni položek ze zálohy. 

S aktualizace 2 nebo novější, virtuální zařízení StorSimple je k dispozici ve dvou modelů: 8010 zařízení (dříve označované jako modelu 1100) a 8020 zařízení. Zařízení 8010 kapacitou maximální 30 TB. 8020 zařízení, které využívá Azure premium úložiště, kapacitou maximální 64 TB. (V místní úrovně Azure premium jsou uloženy dat na disky SSD že standardní jsou uloženy dat na pevných disků.) Všimněte si, že musíte mít účet Azure premium úložiště používat premium úložiště. Další informace o premium úložiště, přejděte na [Premium úložiště: výkonné úložiště pro Azure virtuálního počítače úloh](../storage/storage-premium-storage.md).

Další informace o StorSimple virtuální zařízení najdete v tématu [nasazení a správu StorSimple virtuální zařízení v Azure](storsimple-virtual-device-u2.md).

## <a name="storsimple-manager-service"></a>Služba StorSimple správce

Microsoft Azure StorSimple poskytuje webové uživatelské rozhraní (službu StorSimple správce), který umožňuje centrální správa datacentra a cloudového úložiště. Služba StorSimple správce můžete dělat tyto věci:

- Konfigurace nastavení systému StorSimple zařízení.
- Konfigurovat a spravovat nastavení zabezpečení pro StorSimple zařízení.
- Konfigurace přihlašovacích údajů cloudu a vlastnosti.
- Konfigurovat a spravovat svazky na serveru.
- Konfigurace skupin hlasitost.
- Zálohování a obnovení data.
- Sledovat výkon.
- Zkontrolujte nastavení systému a určit možné problémy.

Služba StorSimple správce umožňuje provádět všechny úlohy správy kromě těch, které vyžadují systém dolů dobu, například počáteční instalaci a nastavení aktualizací.

Další informace přejděte na článek [použití službu StorSimple správce ke správě StorSimple zařízení](storsimple-manager-service-administration.md).

## <a name="windows-powershell-for-storsimple"></a>Prostředí Windows PowerShell pro StorSimple

Prostředí Windows PowerShell pro StorSimple poskytuje rozhraní příkazového řádku, které můžete vytvářet a spravovat službu Microsoft Azure StorSimple a nastavení a sledovat StorSimple zařízení. Jedná se o založené na prostředí Windows PowerShell příkazového řádku rozhraní obsahující vyhrazené rutiny pro správu zařízení StorSimple. Prostředí Windows PowerShell pro StorSimple obsahuje funkce, které umožňují:

- Zaregistrujte zařízení.
- Konfigurace síťové rozhraní na zařízení.
- Nainstalujte určitých typů aktualizace.
- Poradce při potížích zařízení s přístupem k relaci podpory.
- Změna stavu zařízení.

Dostanete prostředí Windows PowerShell pro StorSimple sériové konzole (na hostitelském počítači připojené přímo k zařízení) nebo vzdáleně pomocí vzdálené prostředí Windows PowerShell. Všimněte si, že některé prostředí Windows PowerShell pro StorSimple úkoly, například počáteční zařízení registrace, jenom se teď dá v konzole sériové. 

Další informace přejděte na [Pomocí Windows Powershellu pro StorSimple ke správě zařízení](storsimple-windows-powershell-administration.md).

## <a name="azure-powershell-storsimple-cmdlets"></a>Azure rutiny prostředí PowerShell StorSimple

Rutiny prostředí PowerShell StorSimple Azure jsou kolekce rutin prostředí Windows PowerShell, která umožňují automatizaci úrovni služeb a migrace úkoly z příkazového řádku. Další informace o rutinách Powershellu Azure StorSimple najdete [reference pro rutiny](https://msdn.microsoft.com/library/dn920427.aspx).

## <a name="storsimple-snapshot-manager"></a>Správce StorSimple snímku

Správce snímek StorSimple je snap-in konzoly MMC (Microsoft Management), který slouží k vytvoření konzistentního, v okamžiku záložní kopie místní a cloudu data. Modulu snap-in běží na hostiteli založené na systému Windows Server. Můžete použít Správce StorSimple snímku:

- Konfigurace, obecnějším údajům a svazky odstranit.
- Konfigurace skupin hlasitost zajistit zálohovat data se konzistenci aplikací.
- Správa zásad záložní tak, aby se zálohovala na předem naplánovat a ukládání dat v určené umístění (místně nebo v cloudu).
- Obnovení svazky a jednotlivé soubory.

Zálohování ukládány jako snímky, které zaznamenávat pouze změny, protože byla přijata poslední snímek a vyžadovat úplně méně místa než úplné zálohování. Můžete vytvořit záložní plány nebo v případě potřeby převzít okamžité zálohy. Kromě toho můžete StorSimple snímek správce stanovit zásady uchovávání informací které určují kolik snímky budou uloženy. Pokud potřebujete následně obnovit data ze záložní, umožňuje StorSimple snímek správce vyberete z katalogu místní nebo cloudu snímky. 

Pokud dojde k selhání nebo v případě potřeby obnovit data z jiného důvodu, StorSimple snímek Manager obnoví ho postupně jako není potřeba. Obnovení dat není nutné zadávat vypnout celý systém při obnovení souboru, nahraďte zařízení nebo přesunutí operací na jiný web.

Další informace najdete v tématu [Co je správce snímek StorSimple?](storsimple-what-is-snapshot-manager.md)

## <a name="storsimple-adapter-for-sharepoint"></a>StorSimple adaptér pro službu SharePoint

Microsoft Azure StorSimple obsahuje adaptér StorSimple pro SharePoint, volitelná součást, která transparentně rozšiřuje StorSimple úložiště a data funkce ochrany farmách serverů SharePoint serveru. Adaptér spolupracuje zprostředkovatele vzdálené úložiště objektů Blob (RBS) a funkci SQL Server RBS umožňuje přesunout objekty BLOB na server zálohovala systémem Microsoft Azure StorSimple. Microsoft Azure StorSimple uloží objektů BLOB data místně nebo v cloudu, založené na použití.

Adaptér StorSimple pro službu SharePoint je spravováno z v portálu Centrální správa SharePoint. Proto zůstane centrální správy SharePoint a všechny úložiště vypadá to ve farmě služby SharePoint.

Další informace přejděte na [StorSimple adaptér pro službu SharePoint](storsimple-adapter-for-sharepoint.md). 

## <a name="storage-management-technologies"></a>Technologie pro správu úložiště
 
Kromě vyhrazené zařízení StorSimple virtuální zařízení a jiných komponent Microsoft Azure StorSimple používá následující technologie software poskytuje rychlý přístup k datům a snížit spotřebu úložiště:

- [Automatické ukládání vrstvení](#automatic-storage-tiering) 
- [Tenké zřízení](#thin-provisioning) 
- [Deduplication a komprese](#deduplication-and-compression) 

### <a name="automatic-storage-tiering"></a>Automatické ukládání vrstvení

Microsoft Azure StorSimple automaticky uspořádá dat v logických úrovní na základě aktuální využití, stáří a relace s jinými daty. Data, která je většina aktivní uložena, zatímco automaticky migraci méně aktivní a neaktivní dat do cloudu. Následující obrázek znázorňuje tento přístup úložiště.
 
![StorSimple úložiště úrovní](./media/storsimple-overview/hcs-data-services-storsimple-components-tiers.png)

Chcete-li povolit Rychlý přístup, StorSimple ukládá velmi aktivní data (data, aktivní) na disky SSD StorSimple zařízení. Slouží k ukládání dat, která se používá v některých případech (teplé dat) na pevných disků v zařízení nebo na serverech datacentra. Přesune neaktivní data, zálohování dat, a data se zachovají pro archivaci nebo účely dodržování předpisů v cloudu. 

>[AZURE.NOTE] Aktualizace 2 nebo novější můžete zadat hlasitost místně připnuté, v takovém případě dat zůstává v místním zařízením který není vrstveny do cloudu. 

StorSimple přizpůsobí novým uspořádáním dat a přiřazení úložiště jako vzorce použití změnit. Například některé informace o může aktivuje méně určitou dobu. Jak je z něj stal postupně méně aktivní, se nemigruje z disky SSD pevných disků a potom do cloudu. Pokud tato data opětovné aktivaci migruje se zpátky na paměťové zařízení.

Proces tiering úložiště proběhne následujícím způsobem:

1. Účet Microsoft Azure cloudové úložiště nastavuje správce systému.
2. Používá správce sériové konzoly a služba StorSimple správce (službou Azure klasické portálu) konfigurace serveru zařízení a soubor vytváření zásady ochrany svazky a data. Místního počítače (například serverech) použijte Internet Small počítače systém rozhraní (iSCSI) získat přístup k StorSimple zařízení.
3. Nejprve StorSimple ukládá data u rychlé osy SSD zařízení.
4. Jakmile osy SSD přiblíží kapacitu, StorSimple deduplicates zkomprimuje nejstarší bloky dat a přesune ji do složky osy pevný disk.
5. Jako přístupy kapacita pevný disk osy StorSimple šifruje nejstarší bloky dat a odešle zabezpečené úložiště účet Microsoft Azure prostřednictvím HTTPS.
6. Microsoft Azure vytvoří více kopie data v jeho datacentra a vzdálené datacentra zajistit, že pokud dojde k selhání můžete obnovit data. 
7. Když souborový server požaduje data uložená v cloudu, StorSimple vrátí bezproblémové a ukládá kopie u SSD osy StorSimple zařízení.

### <a name="thin-provisioning"></a>Tenké zřízení

Tenké zřizování je upgradovaný, ve kterém se zobrazí volného překročení fyzické zdroje. Místo předem rezervace dostatečné úložiště StorSimple pomocí tenké zřizování přidělit jenom dostatek místa pro aktuální předpisů. Pružná povahy cloudového úložiště usnadňuje tento přístup, protože StorSimple můžete zvětšit nebo zmenšit cloudového úložiště změna požadavkům. 

>[AZURE.NOTE] Místně připnuté svazky nejsou k dispozici tence. Úložiště přidělit jen místní hlasitost máte k dispozici jako celek při vytvoření hlasitost.

### <a name="deduplication-and-compression"></a>Deduplication a komprese

Microsoft Azure StorSimple používá kompresi deduplication a dat a dál tak snížit požadavky na úložiště.

Deduplication snižuje celkové částky dat uložených odstraněním redundance uložené uvedenou množinu dat. Při změně informací StorSimple ignoruje beze změny dat a zaznamenává pouze změny. Kromě toho StorSimple zmenší uložená data a odstranění nepotřebných informací. 

>[AZURE.NOTE] Data místně připnuté objemů není deduplicated nebo komprimovány. Zálohování místně připnuté svazky se však deduplicated a komprimovány.

## <a name="storsimple-workload-summary"></a>Souhrnné pracovní zátěž StorSimple

Přehled podporované pracovního vytížení StorSimple je: tabulkový níže.

| Scénář                  | Pracovní zátěž                | Podporované |  Omezení                                  | Verze              |
|---------------------------|-------------------------|-----------|------------------------------------------------|----------------------|
| Spolupráce             | Sdílení souborů            | Ano       |                                                | Všechny verze         |
| Spolupráce             | Sdílení souborů| Ano       |                                                | Všechny verze         |
| Spolupráce             | Služby SharePoint              | Ano *      |Podporované jenom s místně připnuté svazky      | Aktualizace 2 a novější   |
| Archivace                  | Archivace Simple souborů   | Ano       |                                                | Všechny verze         |
| Virtualizace            | Virtuálních počítačích        | Ano *      |Podporované jenom s místně připnuté svazky      | Aktualizace 2 a novější   |
| Databáze                  | SQL                     | Ano *      |Podporované jenom s místně připnuté svazky      | Aktualizace 2 a novější   |
| Sledování videa        | Sledování videa      | Ano *       |Podporované, když StorSimple zařízení se snaží o pouze pro tento pracovní zátěž| Aktualizace 2 a novější   |
| Zálohování                    | Primární cílové zálohování | Ano *      |Podporované, když StorSimple zařízení se snaží o pouze pro tento pracovní zátěž| Aktualizace 3 nebo novější |
| Zálohování                    | Sekundární cílové zálohování | Ano *      |Podporované, když StorSimple zařízení se snaží o pouze pro tento pracovní zátěž| Aktualizace 3 nebo novější |

*Ano & #42; -Pokyny k používání řešení a omezení má použito.*

Následující pracovního vytížení nepodporuje StorSimple 8000 řady zařízení. Pokud nasazený na StorSimple pracovní těchto vytížení výsledkem bude Nepodporovaná konfigurace.

-  Zdravotnictví
-  Exchange
-  VDI
-  Oracle
-  SAP
-  Big dat
-  Rozdělení obsahu
-  Spuštění z SCSI

Toto je seznam součástí infrastruktury StorSimple podporované. 

| Scénář | Pracovní zátěž      | Podporované |  Omezení                                 | Verze      |
|----------|---------------|-----------|-----------------------------------------------|--------------|
| Obecné  | Expresní směrování | Ano       |                                               |Všechny verze |
| Obecné  | DataCore FC   | Ano *       |Podporované s DataCore SANsymphony            | Všechny verze |
| Obecné  | DFSR          | Ano *      |Podporované jenom s místně připnuté svazky     | Všechny verze |
| Obecné  | Indexování      | Ano *       |Vrstvené svazky indexování pouze metadat je podporované (žádná data).<br>Místně připnuté svazky dokončení indexování je podporované.| Všechny verze |
| Obecné  | Ochrana proti virům    | Ano *       |Pro vrstvené svazky je podporována pouze prohledávání při otevření a zavření.<br> Místně připnuté svazky úplnou kontrolu podporované.| Všechny verze |

*Ano & #42; -Pokyny k používání řešení a omezení má použito.*

## <a name="storsimple-terminology"></a>StorSimple terminologie 

Před nasazením řešení Microsoft Azure StorSimple, doporučujeme, abyste si prošli následující pojmy a definice.

### <a name="key-terms-and-definitions"></a>Klíčové pojmy a definice

| Období (zkratka nebo zkratku) | Popis |
| ------------------------------ | ---------------- |
| záznam o řízení přístupu (ACR)    | Záznam přidružený hlasitosti na Microsoft Azure StorSimple zařízení, které určují, kteří hostitelé k ní připojit. Určení je založeno na iSCSI kvalifikovaný název IQN () hostitelů (obsažené v ACR), kteří se připojují k StorSimple zařízení.|
| AES 256                        | Algoritmus šifrování AES (Advanced Standard) 256-bit pro šifrování dat tak, jak se přesune do a z cloudu. |
| pole přidělení velikost jednotky (Austrálie)     | Nejmenší množství místa na disku, kterou můžete přidělit pro uložení souboru ve vaší Windowsové systémy souborů. Nejsou-li velikost souboru násobkem velikost obrázku, mezery, musí se používat pro uložení souboru (nahoru Další násobek velikost obrázku) výsledkem ztraceny místa a rozdělení pevném disku. <br>Doporučené Austrálie Azure StorSimple svazky je 64 KB protože ty fungují dobře s algoritmy deduplication.|
| Automatické ukládání vrstvení      | Automatické přesunutí méně aktivní data z disky SSD pevných disků a klikněte na úroveň v cloudu a povolení správy všechny úložiště z centrálního uživatelského rozhraní.|
| zálohování katalogu | Sbírka zálohy, obvykle souvisí typ aplikace, která byla použita. Tato kolekce se zobrazí na stránce Katalog zálohování StorSimple Správce služby UI.|
| soubor záložní katalogu             | Soubor obsahující seznam dostupných snímky obsažené v záložní databáze z StorSimple snímek správce. |
| zásady zálohování                   | Výběr svazky, typ zálohování a časový plán, který umožňuje vytváření záloh na předdefinované plán.|
| binární velké objekty (objektů BLOB)    | Sbírka binární data uložená jako jedna entita v systému správy databáze. Objekty BLOB jsou obvykle obrázky, zvukové soubory nebo jiné multimediální objekty, i když se někdy binární spustitelný kód je uložená jako objekt BLOB.|
| Ověřovací kód programu Handshake ověřování (CHAP Protocol) | Protokol sloužící k ověření partnera připojením založeným na sdílení heslo nebo tajná mezi dvěma účastníky. CHAP může být jednosměrný nebo vzájemné. S jednosměrné protokol ověří cílem způsobit. Vzájemné CHAP vyžaduje cíl ověření vyzývající a že vyzývající ověří cíl. | 
| klonovat                          | Kopii svazku. |
|Shluk jako osy (CaaT)          | Úložiště integrované jako osy v rámci architektury úložiště tak, aby všechny úložiště se má být součástí jedné podnikovou síť úložiště v cloudu.|
| Shluk poskytovatele (CSP)   | Zprostředkovatel cloudu výpočetních služby.|
| snímek cloudu                 | V okamžiku kopii objemu dat, který je uložený v cloudu. Snímek cloudu odpovídá snímek replikovat na jiné, mimo úložiště systému. Snímky cloudu jsou užitečné zejména v případech obnovení havárie.|
| cloudové úložiště šifrovacího klíče   | Heslo nebo klávesa používané zařízení StorSimple k datům šifrované odeslaný zařízení do cloudu.|
| Shluk aktualizace         | Správa aktualizací na serverech překlopení obrázku tak, aby aktualizace minimální nebo žádný vliv na dostupnost služby.|
| DataPath                       | Sbírka funkční jednotky, které provádí operace propojených zpracování dat.|
| deaktivace                     | Trvalé, konce připojení mezi StorSimple zařízení a související cloudovou službu. Snímky cloudu zařízení zůstane po tomto procesu a lze klonovat nebo je použít k obnovení havárie.|
| odrážející strukturu disku                 | Replikace logické diskové svazky na samostatném pevné jednotky v reálném čase zajistit nepřetržitý dostupnosti.|
| dynamický odrážející strukturu         | Replikace logické diskové svazky na dynamických discích.|
| dynamické disků                  | Formát Hlasitost disku, který používá k ukládání a správa dat mezi více discích logické disku Manager (Správce logických disků). Dynamické disků můžete zvětšit poskytnout více volného místa.|
| Rozšířené přílohu skupina disků (EBOD) | Sekundární přílohu Microsoft Azure StorSimple zařízení, která obsahuje další pevném disku disků pro další úložiště.|
| tuku zřízení               | Normální úložiště zřizování v které úložiště je přiděleno místo podle očekávaných potřeb (a obvykle za aktuální nutnosti). Viz taky *tenké zřizování*.|
| jednotku pevného disku (pevný disk)          | Disk a poslat používá několik rotujících ploten k ukládání dat.|
| hybridní cloudového úložiště           | Architektura úložiště, která používá místní a mimo zdrojů, včetně cloudového úložiště.|
| Malé počítač systém (iSCSI Internet Interface) | Na základě protokolu IP úložiště sítě standard propojení dat úložiště zařízení nebo zařízení.|
| Autor iSCSI                 | Součástí software, která umožňuje hostitele počítači se systémem Windows pro připojení k síti externí úložiště na základě iSCSI.|
| iSCSI kvalifikovaný název IQN ()      | Jedinečný název, který identifikuje cíle iSCSI nebo Autor.|
| cíl iSCSI                    | Součást softwaru, který poskytuje centralizované iSCSI diskové podsystémy v SAN.|
| Live archivace                  | Úložiště přístup ve kterém archivace dat přístupný vždycky (není uloží mimo na páskou, například). Microsoft Azure StorSimple používá živou archivaci.|
|místně připnuté hlasitosti | objem, který je umístěn na zařízení a nikdy vrstveny do cloudu. |
| místní snímku                  | V okamžiku kopii objemu dat, který je uložený na Microsoft Azure StorSimple zařízení.|
| StorSimple Microsoft Azure      | Výkonné řešení obsahující zařízení úložiště datacentra a software, který umožňuje organizacím IT můžete využít cloudového úložiště, jako kdyby to byl úložiště datacentra. StorSimple zjednodušuje ochrana dat a správa dat při snížení nákladů. Řešení slučuje primární úložiště, archivace, zálohování a havárie obnovení (DR) až Bezproblémová integrace s cloudu. Zkombinováním SAN úložiště a cloudu správy dat na platformě podnikových StorSimple zařízení povolit rychlosti, zjednodušení a spolehlivost pro všech souvisejících s úložištěm potřeb.|
| Výkon a chlazení modul (PCM)  | Hardwarové součásti StorSimple zařízení obsahující napájecí zdroje a chlazení ventilace, tedy název Power a chlazení modul. Primární přílohu zařízení má dvě PCMs 764W zatímco přílohu EBOD má dvě PCMs 580W.|
| primární přílohu               | Hlavní přílohu StorSimple zařízení, která obsahuje řadiče platformy aplikace.|
| obnovení čas cíle (RTO)   | Maximální časový úsek, které by měl být použito před obchodních procesů nebo systému je plně obnovit po selhání.| 
|sériově připojené SCSI (přidružení zabezpečení)       | Typ jednotku pevného disku (pevného disku).|
| Služba dat šifrovacího klíče     | Klíč k dispozici pro všechny nové StorSimple zařízení, která registruje službu StorSimple správce. Konfigurace data přenášena mezi službu StorSimple správce a zařízení musí být zašifrovaný pomocí veřejným klíčem a lze dešifrovat jenom na zařízení pomocí privátním klíčem. Služba dat šifrovací klíč umožňuje služby a získat tento soukromý klíč pro dešifrování.|
| klíč registrace služby        | Klávesy, která vám pomůže zaregistrovat zařízení StorSimple ke službě StorSimple správce tak, aby se zobrazil Azure klasické portálu pro další činnost správy.|
| Malé počítače (SCSI System Interface) | Nastavení organizace pro standardy fyzicky připojení počítače a předávání dat mezi nimi.|
| plné stavu jednotka (SSD)         | Disk, který obsahuje žádné proměnné části; například, flash disk.|
| úložiště účtu                 | Sadu přihlašovacích údajů přístup spojeným s vaším účtem úložiště pro dané cloudu poskytovatele služeb.| 
| StorSimple adaptér pro službu SharePoint| Součást Microsoft Azure StorSimple transparentně rozšiřuje StorSimple úložiště a ochranu dat na SharePoint serverové farmy.|
| Služba StorSimple správce      | Rozšíření Azure klasické portál, který umožňuje spravovat Azure StorSimple místní a virtuální zařízení.|
| Správce StorSimple snímku     | Modul Microsoft Management Console (MMC) modulu snap-in pro správu zálohování a obnovení operace v Microsoft Azure StorSimple.|
| Prohlédněte zálohování                     | Funkce, která umožňuje uživateli trvat interaktivní zálohování svazku. Je alternativní způsob přijímání ruční zálohování objemu namísto provedením automatické zálohování prostřednictvím definované zásady.|
| tenké zřízení               | Metoda optimalizace efektivity, se kterým se používá volného místa v systémech úložiště. V tenké zřizování úložiště rozdělí více uživatelů podle pole minimální vyžadované každého uživatele v daném okamžiku. Viz taky *fat zřizování*.|
| Vrstvení | Uspořádání dat v logické seskupení podle aktuální využití, stáří a relace s jinými daty. StorSimple automaticky uspořádá dat v úrovní.   |
| objem                          | Logické úložištích prezentovány ve formě jednotky. StorSimple svazky odpovídají svazky připevněna hostiteli, včetně těch, které zjistil použitím iSCSI a StorSimple zařízení.|
 | Hlasitost kontejneru                | Seskupení svazky a nastavení, které se vztahují k nim. Všechny svazky v zařízení s StorSimple budou seskupené do hlasitost kontejnery. Kontejner hlasitost patří účty úložiště, nastavení šifrování pro data odeslaná do cloudu pomocí přidružené šifrovacího klíče a šířku pásma spotřebované množství pro operace týkající se v cloudu.|
| Hlasitost skupiny                    | Ve Správci snímek StorSimple skupinu hlasitost najdete objemu nakonfigurované tak, aby usnadnění záložní zpracování.|
| Služba kopie hlasitost stín (VSS)| Služba operačního systému Windows Server, která usnadňuje konzistenci aplikace tak, že komunikaci s VSS aplikací ke koordinaci vytváření přírůstková snímků. VSS zajišťuje aplikace dočasně neaktivní při snímky.|
| Prostředí Windows PowerShell pro StorSimple | Na základě prostředí Windows PowerShell rozhraní příkazového řádku slouží k ovládání a správě StorSimple zařízení. A zachovat část základních funkcí prostředí Windows PowerShell, má toto rozhraní další vyhrazené rutin, které jsou zaměřené Správa StorSimple zařízení.|


## <a name="next-steps"></a>Další kroky

Informace o [StorSimple zabezpečení](storsimple-security.md).




 

 
