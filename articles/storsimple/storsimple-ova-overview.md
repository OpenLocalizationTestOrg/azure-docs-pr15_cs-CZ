<properties
   pageTitle="Přehled StorSimple virtuální pole | Microsoft Azure"
   description="Popisuje StorSimple virtuální pole, integrované úložiště řešení, která spravuje úložiště úkolů mezi místním virtuální zařízení a Microsoft Azure cloudového úložiště."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="10/06/2016"
   ms.author="alkohli" />

# <a name="introduction-to-the-storsimple-virtual-array"></a>Úvod do maticových virtuální StorSimple

## <a name="overview"></a>Základní informace

Vítá vás Microsoft Azure StorSimple virtuální matice, integrované úložiště řešení, která spravuje úložiště úkolů mezi virtuální zařízení s místním systémem hypervisoru a Microsoft Azure cloudového úložiště. Virtuální pole (označovaná taky jako StorSimple místní virtuální zařízení) je efektivně, efektivní a snadno ovladatelným souborový server nebo iSCSI serveru řešení, které eliminuje v mnoha problémů a náklady spojené s enterprise úložiště a ochranu dat. Virtuální pole je zejména vhodné pro vzdálené office/větev scénáře office (ROBO).

Přehled se zaměřuje na virtuální matice. 

- Přehled řady StorSimple 8000 naleznete v tématu [řady StorSimple 8000: hybridní cloudové řešení](storsimple-overview.md). 

- Informace o zařízení StorSimple 5000/7000 řady přejděte na [StorSimple Online nápověda](http://onlinehelp.storsimple.com/).

Pole virtuální podporuje iSCSI nebo zprávu blok protokol Server (SMB). Běží na stávající infrastrukturu hypervisoru a poskytuje vrstvení cloudu, zálohování cloudu, rychlé obnovení, obnovení úrovni položek a funkce pro obnovení havárie.

Následující tabulka shrnuje důležité funkce virtuální matice.

| Funkce | Virtuální matice |
| ------- | ------------- |
|Požadavky na instalaci | Použití virtualizace infrastruktury (Hyper-V nebo VMware)|
| Dostupnost | Jeden uzel |
| Součet kapacita (včetně cloudu) |Až 64 TB použitelné kapacitu virtuální zařízení |
| Místní kapacity | 390 GB 6,4 TB použitelné kapacitu virtuální zařízení (třeba zřízení 500 GB 8 TB místa na disku)|
| Nativní protokoly | iSCSI nebo SMB |
| Obnovení čas cíle (RTO) | iSCSI: menší než 2 minuty bez ohledu na velikost |
| Obnovení čárky cíle (operace RPO) | Denní a zálohování na vyžádání |
| Vrstvení úložiště | Použití tepelná mapování a zjistit, jaká data má vrstveny či oddálení |
| Podpora | Infrastruktura virtualizace podporované dodavatelem |
| Výkon | Liší se podle podkladových infrastruktury |
| Nastavení mobilních zařízení dat | Obnovení na stejné zařízení nebo úrovni položek obnovení (souborovém serveru) |
| Úrovně úložiště | Místní hypervisoru úložiště a cloudu |
| Sdílení velikost |Vrstveny: až 20 TB; místně připnuté: až do velikosti 2 TB |
| Velikost svazku |Vrstveny: až 5 TB; místně připnuté: až 500 GB |
| Snímky | Konzistentní z hlediska chyb |
| Obnovení úrovni položek | Ano; uživatele můžete obnovit ze sdílené složky |

## <a name="why-use-storsimple"></a>Proč používat StorSimple?

StorSimple propojuje uživatelů a servery k základnímu úložišti Azure minut s žádné změny aplikace.

Následující tabulka popisuje některé klíčové výhody, které poskytuje řešení virtuální pole.

| Funkce | Výhoda |
|---------|---------|
| Průhledný integrace | Pole virtuální podporuje iSCSI nebo protokol SMB. Přesun dat mezi místním osy a osy cloudu je bezproblémové a průhledné uživateli.|
| Omezená úložiště nákladů | S StorSimple zřizujete dostatečné místní úložiště aktuální požadavkům pro nejpoužívanějších žádanou data. Při ukládání potřeby zvětšovat StorSimple úrovní studenou data do efektivní cloudového úložiště. Data jsou deduplicated a komprimovány před odesláním do cloudu a dál tak snížit náklady a požadavky na úložiště.|
| Správa zjednodušené úložiště | StorSimple poskytuje centralizované správy v cloudu pomocí StorSimple správce pro správu víc zařízení.| 
| Vylepšené havárie obnovení a dodržování předpisů | StorSimple usnadňuje rychlejší obnovení havárie obnovení metadata okamžitě a obnovení dat podle potřeby. To znamená, že normálně pokračovat s minimálními přerušení.|
| Nastavení mobilních zařízení dat | Data vrstveny do cloudu můžete k nim získat přístup z jiných webů pro účely obnovení a migrace. Poznámka: data můžete obnovit jenom do pole původní virtuální. Však použijete možnosti obnovení havárie obnovit celý virtuální pole do jiného virtuální pole.|

## <a name="storsimple-workload-summary"></a>Souhrnné pracovní zátěž StorSimple

Přehled podporované pracovního vytížení StorSimple je: tabulkový níže.

| Scénář                | Pracovní zátěž              | Podporované |  Omezení                                  | Verze              |
|-------------------------|-----------------------|-----------|------------------------------------------------|----------------------|
| ROBO spolupráce      | Sdílení souborů          | Ano       | V tématu [maximální limity pro souborovém serveru](storsimple-ova-limits.md). <br>V tématu [Systémové požadavky pro podporované SMB verze](storsimple-ova-system-requirements.md).   | Všechny verze      |


## <a name="workflows"></a>Pracovní postupy

Virtuální pole StorSimple je vhodné zejména pro následující postupy:

- [Správa cloudového úložiště](#cloud-based-storage-management)
- [Nezávislé na místa zálohu](#location-independent-backup)
- [Ochrana a havárie obnovení dat](#data-protection-and-disaster-recovery)

### <a name="cloud-based-storage-management"></a>Správa cloudového úložiště

Můžete StorSimple Správce služby Azure klasické portálu pro správu dat uložených na několika zařízeních a ve víc umístěních. Toto je užitečné v distribuované větev scénáře. Všimněte si, že musíte vytvořit samostatné instance službu StorSimple správce pro správu virtuální maticemi a fyzických StorSimple zařízení. 

![Správa cloudového úložiště](./media/storsimple-ova-overview/cloud-based-storage-management.png)

### <a name="location-independent-backup"></a>Nezávislé na místa zálohu

Pole virtuální cloudu snímky poskytnutí nezávislé na umístění, v okamžiku kopii hlasitost nebo sdílet. Snímky cloudu jsou standardně a se nedá zakázat. Svazky jsou všechna a jejich počet zálohovat ve stejnou dobu pomocí jednoho denní záložní zásady a může trvat další ad hoc zálohování kdykoli je to nutné.

### <a name="data-protection-and-disaster-recovery"></a>Obnovení protection a havárie dat

Virtuální pole podporuje dat protection a havárie obnovení následovně:

- **Obnovení hlasitost nebo sdílet** – použití obnovení jako nový pracovní postup k obnovení hlasitost nebo sdílet. Tuto možnost použijte proto pokud chcete obnovit celý hlasitost nebo sdílet.
- **Obnovení úrovni položek** – akcie povolení zjednodušené přístupu k poslední doby nějaké zálohy. Soubor můžete snadno obnovení ze složky zvláštní .backup k dispozici v cloudu. Tato možnost obnovit je řízené úsilím a požaduje zásah pro správu.
- **Obnovení havárie** – použití převzetí možnost obnovit všechny svazky nebo složky do nového virtuální pole. Vytvořte nový virtuální pole a zaregistrovat ke službě StorSimple správce a potom selhání původní virtuální matice. Nové pole virtuální pak převezme zřizování zdroje. 

## <a name="virtual-array-components"></a>Virtuální prvků matic.

Virtuální matice obsahuje tyto prvky:

- [Virtuální maticový](#virtual-array) – úložné zařízení cloudu hybridní podle virtuálního počítače zřízení ve virtualizovaném prostředí nebo hypervisoru.  
- [Služba StorSimple správce](#storsimple-manager-service) – rozšíření portálu Azure klasické, který vám umožní jeden nebo více StorSimple zařízení můžete spravovat z jedné webové rozhraní, včetně, které se můžete dostat z různých míst. Vytvoření a správa služeb, zobrazení a správa zařízení a upozornění a spravovat svazky, jejich počet a existující snímků můžete službu StorSimple správce.
- [Místní web uživatelské rozhraní](#local-web-user-interface) – uživatelské rozhraní založené na webu, který se používá ke konfiguraci zařízení, aby mohli připojit k místní síti a zaregistrovat zařízení se službou StorSimple správce. 
- [Rozhraní příkazového řádku](#command-line-interface) – rozhraní prostředí Windows PowerShell, které slouží k zahájení relace podpory pro virtuální matici.
V následujících částech jsou popsány jednotlivé tyto součásti podrobněji a popisují, jak řešení uspořádá dat, přidělí úložiště a usnadňuje Správa úložiště a ochrana dat.

### <a name="virtual-array"></a>Virtuální matice

Virtuální matice je jednoduchá uzel úložiště řešení, které poskytuje primární úložiště, spravuje komunikace s cloudového úložiště a pomáhají zajistit bezpečnost a důvěrnost všechna data, která jsou uložená na zařízení.

Virtuální matice je k dispozici v jednom modelu, který je k dispozici ke stažení. Pole úložiště kapacitou maximální 6,4 TB na zařízení (základní požadavek úložiště 8 TB) a včetně 64 TB cloudového úložiště. 

Virtuální matice obsahuje následující funkce:

- Je nákladů účinné. Je něco v ní pomocí stávající infrastrukturu virtualizace a můžou být nasazené na existující Hyper-V nebo VMware hypervisoru.
- Je umístěn v datacentra a lze nastavit jako iSCSI server nebo souborovém serveru. 
- Je integrována s cloudu.
- Zálohování jsou uložené v cloudu, které můžete usnadnění havárie využití a zjednodušení obnovení úrovni položek (ILR). 
- Můžete používat aktualizace virtuální matici stejně, jako by použijete je fyzické zařízení.

>[AZURE.NOTE] Virtuální pole nelze rozbalit. Proto je důležité zajištění odpovídající úložiště při vytváření virtuálních zařízení. 

### <a name="storsimple-manager-service"></a>Služba StorSimple správce

Microsoft Azure StorSimple poskytuje webové uživatelské rozhraní (službu StorSimple správce), který umožňuje centrální správa datacentra a cloudového úložiště. Služba StorSimple správce můžete dělat tyto věci:

- Správa více StorSimple virtuální pole z jedné služby. 
- Konfigurovat a spravovat nastavení zabezpečení pro StorSimple zařízení. (Šifrování v cloudu závisí na Microsoft Azure API.)
- Konfigurace přihlašovací údaje účtu úložiště a vlastnosti.
- Konfigurovat a spravovat svazky nebo jejich počet.
- Zálohování a obnovení dat na svazky nebo jejich počet.
- Sledovat výkon.
- Zkontrolujte nastavení systému a určit možné problémy.

Používáte službu StorSimple správce provádět každodenní správu virtuální pole.

Další informace přejděte na článek [použití službu StorSimple správce ke správě StorSimple zařízení](storsimple-manager-service-administration.md).

### <a name="local-web-user-interface"></a>Místní web uživatelské rozhraní

Virtuální matice obsahuje uživatelské rozhraní webu založené, který se používá pro jednorázové konfigurace a registrace zařízení se službou StorSimple správce. Můžete ho vypnout a restartujte virtuální matice, spuštění diagnostických testů, aktualizujte software, změnit heslo správce zařízení, zobrazit protokoly systému a kontaktovat Microsoft Support poslat žádost o službu. 

Informace o používání rozhraní založené na webu přejděte na článek [použití uživatelského rozhraní webových spravovat své StorSimple virtuální pole](storsimple-ova-web-ui-admin.md).

### <a name="command-line-interface"></a>Rozhraní příkazového řádku

Rozhraní však započítávány prostředí Windows PowerShell umožňují zahájit relaci podporu s Microsoft Support tak, aby mohli umožňují odstraňování a řešení problémů, které můžete narazit na zařízení s virtuální.

## <a name="storage-management-technologies"></a>Technologie pro správu úložiště

Kromě virtuální matice a jiných komponent řešení StorSimple používá následující software technologie umožňují rychlý přístup k důležitá data, snížit spotřebu úložiště a ochrana dat uložených na virtuální pole:

- [Automatické ukládání vrstvení](#automatic-storage-tiering) 
- [Místně připnuté sdílené položky a svazky](#locally-pinned-shares-and-volumes)
- [Deduplication komprese dat vrstveny a zálohovat do cloudu](#deduplication-and-compression-for-data-tiered/backed-up-to-the-cloud) 
- [Plánované a na vyžádání zálohování](#scheduled-and-on-demand-backups)

### <a name="automatic-storage-tiering"></a>Automatické ukládání vrstvení

Pole virtuální pracuje na nové tiering uložená data provádět správu přes virtuální matice a cloudu. Existuje pouze dvou úrovní: místní virtuální matice a Azure cloudového úložiště. Pole virtuální StorSimple automaticky jsou uspořádány dat úrovní podle tepelné mapy, která sleduje aktuální využití, stáří a relace s jinými daty. Data, která je většina aktivní (nejprodávanějších) je uložená místně, zatímco automaticky migraci méně aktivní a neaktivní dat do cloudu. (Všechny zálohy jsou uložené v cloudu). StorSimple přizpůsobí novým uspořádáním dat a přiřazení úložiště jako vzorce použití změnit. Například některé informace o může aktivuje méně určitou dobu. Jak je z něj stal postupně méně aktivní, je vrstveny se do cloudu. Pokud tato data opětovné aktivaci vrstveny se v matici úložiště.

Data pro konkrétní vrstvené sdílet nebo hlasitost je zaručena vlastním prostoru místní osy. (přibližně vypočítá 10 % celkové zřizování místo pro danou sdílet nebo hlasitost). Během sníží volného na virtuální zařízení pro danou sdílet nebo objem, zaručuje, že vrstvení pro jeden sdílet nebo objem se nevztahuje tiering potřeb jiné sdílené položky a svazky. Velmi zaneprázdněné pracovní zátěž na jedno sdílené složky nebo objem tedy nelze vynutit všechny úlohami do cloudu. 

![Automatické ukládání vrstvení](./media/storsimple-ova-overview/automatic-storage-tiering.png)

>[AZURE.NOTE] Můžete zadat hlasitost místně připnuté, v takovém případě dat zůstává v poli virtuální a nesmí vrstveny do cloudu. Další informace přejděte na [místně připnuté sdílené položky a svazky](#locally-pinned-shares-and-volumes).

### <a name="locally-pinned-shares-and-volumes"></a>Místně připnuté sdílené položky a svazky

Můžete vytvořit příslušné složky a svazky místně připnuté. Tato možnost zajišťuje, že údaje vyžadované důležitých aplikací zůstane v poli virtuální a nikdy vrstveny do cloudu. Místně připnuté sdílené položky a svazky mají tyto funkce: 

- Nejsou vyměřené poplatky za jeho čekacích dob cloudu nebo problémy s připojením.
- Budou využívat výhod StorSimple cloudu zálohování a havárie funkce pro obnovení.

Můžete obnovit místně připnuté sdílet nebo hlasitost podle vrstveny nebo vrstvené sdílet nebo připnuté místně hlasitost. 

Další informace o místně připnuté svazky přejděte na článek [použití službu StorSimple správce pro správu svazky](storsimple-manage-volumes-u2.md).

### <a name="deduplication-and-compression-for-data-tiered-or-backed-up-to-the-cloud"></a>Deduplication komprese dat vrstveny a zálohovat do cloudu

StorSimple používá kompresi deduplication a dat a dál tak snížit požadavky na úložiště v cloudu. Deduplication snižuje celkové částky dat uložených odstraněním redundance uložené uvedenou množinu dat. Při změně informací StorSimple ignoruje beze změny dat a zaznamenává pouze změny. Kromě toho StorSimple zmenší uložená data a odebrání duplicitních. 

>[AZURE.NOTE] Data uložená v poli virtuální není deduplicated nebo komprimovány. Všechny deduplication a komprese nastane jenom data se odesílá do cloudu.

### <a name="scheduled-and-on-demand-backups"></a>Plánované a na vyžádání zálohování

Funkce pro ochranu dat StorSimple umožňují vytvářet na vyžádání zálohy. Výchozí záložní plán navíc zajistíte dat je zálohování denně. Zálohování přesměrováni ve formě přírůstková snímky, které jsou uložené v cloudu. Snímky, které zaznamenávat pouze změny od posledního zálohování, můžete vytvořili a rychle obnovit. Tyto snímky může být v postupech obnovení havárie zásadně důležité, protože nahradit systémy sekundárním úložiště (například páskou zálohování) a umožňují dat do vašeho datacentra nebo alternativní weby v případě potřeby obnovit.

## <a name="next-steps"></a>Další kroky

Zjistěte, jak [připravit portálu virtuální pole](storsimple-ova-deploy1-portal-prep.md).


