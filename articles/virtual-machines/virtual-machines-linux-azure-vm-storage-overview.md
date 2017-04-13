<properties
  pageTitle="Azure a Linux OM úložiště | Microsoft Azure"
  description="Popisuje Azure standardní a Premium úložiště s Linux virtuálních počítačích."
  services="virtual-machines-linux"
  documentationCenter="virtual-machines-linux"
  authors="vlivech"
  manager="timlt"
  editor=""/>

<tags
  ms.service="virtual-machines-linux"
  ms.devlang="NA"
  ms.topic="article"
  ms.tgt_pltfrm="vm-linux"
  ms.workload="infrastructure"
  ms.date="10/04/2016"
  ms.author="v-livech"/>

# <a name="azure-and-linux-vm-storage"></a>Azure a Linux OM úložiště

Azure úložiště je řešení cloudové úložiště pro moderní aplikace, které jsou závislé na životnost, dostupnost a škálovatelnost podle potřeby svých zákazníků.  Kromě umožňuje vývojářům vytvářet rozsáhlé aplikace podporuje nové scénáře, úložišti Azure také poskytuje úložiště základ pro Azure virtuálních počítačích.

## <a name="azure-storage-standard-and-premium"></a>Azure úložiště: Standard a Premium

Azure OM můžete vytvořené po disků standardní úložiště nebo disků úložiště premium.  Při použití portálu zvolit vaší OM musí přepnout rozevíracího seznamu v dialogovém okně základní informace o zobrazení standardních a premium discích.  Následující snímek obrazovky se zvýrazní této nabídce přepínač.  Když přepnete na SSD premium úložiště, zobrazí povolené VMs všechny podporovaným SSD jednotky.  Když přepnete na pevný disk, standardní úložiště povolený VMs zálohované otáčející diskových jednotek se zobrazí, spolu s premium úložiště VMs podporovaným SSD.

  ![screen1](../virtual-machines/media/virtual-machines-linux-azure-vm-storage-overview/screen1.png)

Při vytváření OM z `azure-cli` si můžete vybrat mezi standard a premium při výběru velikosti OM prostřednictvím `-z` nebo `--vm-size` příznak rozhraní příkazového řádku.

### <a name="create-a-vm-with-standard-storage-vm-on-the-cli"></a>Vytvoření virtuálního počítače s standardní úložiště OM v rozhraní příkazového řádku

Příznak rozhraní příkazového řádku `-z` zvolí Standard_A1 s A1 vůbec standardní úložiště na základě OM Linux.

```bash
azure vm quick-create -g rbg \
exampleVMname \
-l westus \
-y Linux \
-Q Debian \
-u exampleAdminUser \
-M ~/.ssh/id_rsa.pub
-z Standard_A1
```

### <a name="create-a-vm-with-premium-storage-on-the-cli"></a>Vytvoření virtuálního počítače s úložištěm premium na rozhraní příkazového řádku

Příznak rozhraní příkazového řádku `-z` zvolí Standard_DS1 s DS1 právě premium úložiště na základě OM Linux.

```bash
azure vm quick-create -g rbg \
exampleVMname \
-l westus \
-y Linux \
-Q Debian \
-u exampleAdminUser \
-M ~/.ssh/id_rsa.pub
-z Standard_DS1
```

## <a name="standard-storage"></a>Standardní úložiště

Azure standardní úložiště je výchozí typ úložiště.  Standardní úložiště je nákladů efektivní při tom mít pořád performant.  

## <a name="premium-storage"></a>Úložiště Premium

Azure Premium úložiště poskytuje podpora výkonných maximum, minimum latence disku virtuálních počítačích spuštěný můžu/O-značnou úloh. Disků virtuálního počítače (OM), které používají Premium úložiště ukládání dat v plné stavu jednotky (disky SSD). Vaše aplikace OM disků můžete migrovat do úložiště Premium Azure využívat rychlost a výkonu discích.

Úložiště prémiových funkcí:

- Disků úložiště Premium: Azure Premium úložiště podporuje disků OM, které může být připojen Pošta, DSv2 nebo GS řad Azure VMs.

- Kulatý stránky Premium: Premium úložiště podporuje objektů BLOB stránky Azure, která se používají pro ukládání trvalý disků pro virtuálních počítačích Azure (VMs).

- Premium místně nadbytečné úložiště: Účet Premium úložiště pouze podporuje místně nadbytečné úložiště (LRS) jako možnost replikace a ponechá tři kopie data v jedné oblasti.

- [Úložiště Premium](../storage/storage-premium-storage.md)

## <a name="premium-storage-supported-vms"></a>Úložiště Premium podporované VMs

Úložiště Premium podporuje DS-řady, DSv2 řady, GS řady a Fs řady Azure virtuálních počítačích (VMs). Můžete Standard a Premium disků úložiště s úložištěm Premium podporované VMs. Ale nemůžete používat disků Premium úložiště s OM řad, které nejsou kompatibilní Premium úložiště.

Tady jsou distribuce Linux jsme ověřit s úložištěm Premium.

| Rozdělení. | Verze                 | Podporované jádra    |
|--------------|-------------------------|---------------------|
| Se systémem Ubuntu       | 12.04                   | 3.2.0-75.110+       |
| Se systémem Ubuntu       | 14.04                   | 3.13.0-44.73+       |
| Debian       | 7.x, 8.x                | 3.16.7-ckt4-1+      |
| SLES         | SLES 12                 | 3.12.36-38.1+       |
| SLES         | SLES 11 SP4             | 3.0.101-0.63.1+     |
| Jádro operačního systému       | 584.0.0+                | 3.18.4+             |
| Centos       | 6.5 6.6, 6,7, 7.0, 7.1 | 3.10.0-229.1.2.el7+ |
| RHEL         | 6.8 +, 7.2 +              |                     |


## <a name="file-storage"></a>Ukládání souborů

Azure úložiště souborů nabízí sdílených souborů v cloudu pomocí standardní protokol SMB. Azure soubory můžete migrovat podnikových aplikací, které jsou závislé na serverech souboru a Azure. Aplikace spuštěné v Azure můžete snadno připojit sdílených souborů z Azure virtuálních počítačích s Linux. A nejnovější verze úložiště souborů můžete připojit také sdílené složky z místního aplikace, která podporuje SMB 3.0.  Vzhledem k tomu sdílené složky sdílené položky SMB, dostanete se k nim prostřednictvím standardní systém souborů rozhraní API.

Uložení souboru jsou založeny na stejné technologii jako úložiště objektů Blob, tabulky a fronty tak úložiště souborů nabízí dostupnost, životnost, škálovatelnost a geo redundance, která je integrovaná v platformu Azure úložiště. Podrobnosti o cílů výkonu úložiště souborů a omezení najdete v tématu Azure úložiště škálovatelnost a výkonu cílů.

- [Použití úložiště souborů Azure s Linux](../storage/storage-how-to-use-files-linux.md)

## <a name="hot-storage"></a>Aktivní úložiště

Vrstvy Azure žádanou úložiště je optimalizováno pro ukládání data, která se často pracuje.  Aktivní – úložiště je výchozí typ úložiště objektů blob ukládá.

## <a name="cool-storage"></a>Vytvoření skvělých úložiště

Vrstvy Azure zajímavých úložiště je optimalizováno pro ukládání data, která jsou k nim přistupovat a dlouhodobé zřídka. Příklad použití balení zajímavých úložiště zahrnout zálohy, mediální obsah, matematickém dat, dodržování předpisů a archivace data. Obecně všechna data, která je zřídka k nim získat přístup je ideální uchazeč o zajímavých úložiště.

|                             | Aktivní úložiště osy      | Vytvoření skvělých úložiště osy     |
|:----------------------------|:---------------------:|:---------------------:|
| Dostupnost                | 99,9 %                 | 99 %                   |
| Dostupnost (Vzdálená pomoc GRS čtení) | 99,99 %                | 99,9 %                 |
| Poplatky za použití               | Vyšší náklady úložiště  | Nižší náklady úložiště   |
|                             | Nižší přístup          | Vyšší přístup         |
|                             | a náklady transakce | a náklady transakce |


## <a name="redundancy"></a>Zálohování

Data ve vašem účtu Microsoft Azure úložiště je vždy replikovat ověřit životnosti a dostupnost schůzky SLA úložiště Azure i při selhání přechodná hardwaru.

Při vytváření účtu úložiště je třeba vybrat jednu z následujících možností replikace:

- Místně nadbytečné úložiště (LRS)
- Zóny nadbytečné úložiště (ZRS)
- GEO nadbytečné úložiště (GRS)
- Čtení geo nadbytečné úložiště (Vzdálená pomoc GRS)

### <a name="locally-redundant-storage"></a>Místně nadbytečné úložiště

Místně nadbytečné úložiště (LRS) zreplikuje dat v oblasti, ve které jste vytvořili účtu úložiště. Maximalizovat životnosti, je každý žádost proti dat ve vašem účtu úložiště replikovat třikrát. Tyto tři každý replikami v samostatném poruch domén a upgrade domén.  Žádost o vrátí úspěšně pouze jednou byla vytvořena do tří ostatními.

### <a name="zone-redundant-storage"></a>Zóny nadbytečné úložiště

Zóny nadbytečné úložiště (ZRS) zreplikuje dat přes dvě nebo tři zařízení, která, buď v rámci jedné oblasti nebo dvou oblastí poskytují vyšší odolnost než LRS. Pokud váš účet úložiště má ZRS povoleno, je dat trvalé i v případě selhání najednou zařízení.

### <a name="geo-redundant-storage"></a>GEO nadbytečné úložiště

GEO nadbytečné úložiště (GRS) zreplikuje dat na vedlejší oblast, která je stovky mil od primární oblast. Pokud váš účet úložiště GRS povoleno, je vaše data trvalé i v případě výpadku dokončení místní nebo selhání, ve kterém není obnovitelné oblasti primární.

### <a name="read-access-geo-redundant-storage"></a>Geo nadbytečné úložiště přístup pro čtení

Čtení geo nadbytečné úložiště (Vzdálená pomoc GRS) maximalizuje dostupnost pro váš účet úložiště zadáním jen pro čtení přístup k datům v sekundární umístění kromě replikace přes dvou oblastí poskytovanou GRS. V případě, že data nebude k dispozici v oblasti primární, aplikace číst data z oblasti sekundární.

Pro hloubkové postupy do Azure úložiště redundance najdete v článku:

- [Azure replikace úložiště](../storage/storage-redundancy.md)

## <a name="scalability"></a>Škálovatelnost:

Azure úložiště je datových scalable mohou být uloženy a proces stovky TB dat pro podporu scénáře velký dat vyžadované matematickém, finanční analýzy a multimediálních aplikací. Nebo se můžou být uloženy krátkých data potřebná pro web small business. Ať spadají vašim potřebám, můžete zaplatit pouze data, která máte uložený. Azure úložiště aktuálně ukládá desítky trillions objektů jedinečné zákazníka a průměrně zpracovává miliony požadavků na sekundu.

U účtů standardní úložiště: účet standardní úložiště obsahuje maximální celkové žádost výnosnost 20 000 procesorů. Celkové procesorů ve všech disků virtuálního počítače v účtu standardní úložiště neměli překročit toto omezení.

U účtů úložiště premium: účtu úložiště premium obsahuje maximální celkový výkon výši 50 s technologií. Celkový výkon všech disků OM neměli překročit toto omezení.

## <a name="availability"></a>Dostupnost

Jsme zaručit, že aspoň 99,99 % (99,9 zajímavých osy přístup) času, jsme se úspěšně zpracovávat žádosti o načtení dat z účtů přístup pro čtení Geo nadbytečné úložiště (Vzdálená pomoc GRS) za předpokladu, že neúspěšné pokusy o načtení dat z oblasti primární jsou opakované na vedlejší oblast.

Nemůžeme zaručit, že aspoň 99,9 % (99 % zajímavých úroveň přístupu) času, jsme bude úspěšně zpracovávat žádosti o načtení dat z místně nadbytečné úložiště (LRS), zóny nadbytečné úložiště (ZRS) a účty Geo nadbytečné úložiště (GRS).

Nemůžeme zaručit, že aspoň 99,9 % (99 % zajímavých úroveň přístupu) času, jsme se úspěšně zpracovávat požadavky na zápis dat do místně nadbytečné úložiště (LRS), zóny nadbytečné úložiště (ZRS) a Geo nadbytečné úložiště (GRS) účty a účty adresářové přístup pro čtení Geo nadbytečné úložiště (Vzdálená pomoc GRS).

- [Azure SLA úložiště](https://azure.microsoft.com/support/legal/sla/storage/v1_1/)


## <a name="regions"></a>Oblasti

Azure neexistuje obvykle 30 oblastí celého světa a má se ohlásí, plány 4 Další oblastí. Zeměpisné rozšíření totiž prioritu Azure umožňuje zákazníci k dosažení vyšší výkon a podpora jejich požadavky a předvolby týkající se data umístění.  Azures nejnovější oblasti, kterou chcete spustit probíhá Německo.

Německo cloudu společnosti Microsoft poskytuje odlišné možnost ke službám cloudu společnosti Microsoft, už k dispozici v rámci Europe, vytváření lepší možnosti pro inovace a růstu pro vysoce regulovaném partnery a zákazníky v Německo, Evropské unie (EU) a Evropský volného obchodu přidružení (ESVO).

Data o zákaznících v těchto nové datacentrech Magdeburgu a Frankfurt, spravují pod kontrolou správce dat, mezinárodní systémy, T, nezávislé německé společnosti a pobočky německá Telekom. Komerční cloudovým službám společnosti Microsoft v těchto datacentrech dodržovat německé dat zpracování předpisy a dejte zákazníkům další možnosti jak a kam zpracování.


- [Azure oblastí mapy](https://azure.microsoft.com/regions/)

## <a name="security"></a>Zabezpečení

Azure úložiště umožňuje komplexní sady možnosti zabezpečení, které dohromady vývojářům umožňuje vytvářet zabezpečené aplikace. Přímo účet úložiště lze zabezpečit pomocí řízení přístupu na základě rolí a Azure Active Directory. Data lze zabezpečit mezi aplikací a Azure pomocí šifrování na straně klienta, HTTPS nebo SMB 3.0. Data můžete nastavit automaticky zašifrovat zápisu k základnímu úložišti Azure pomocí úložiště služby šifrování (SSE). Šifrování pomocí šifrování disku Azure můžete nastavit OS a dat disků používaný virtuálních počítačích. Delegovaná přístup k objektům dat v úložišti Azure můžete k ní mít udělený používání sdílené podpisů přístup.

### <a name="management-plane-security"></a>Správa rovině zabezpečení

Správa rovině se skládá ze zdroje sloužící ke správě účtu úložiště. V této části podíváme nasazení modelu správce prostředků Azure a jak pomocí řízení přístupu na základě rolí (RBAC) k řízení přístupu k účtům úložiště. Budeme se taky o správě kódu účtu Key úložiště a jak je obnovit.

### <a name="data-plane-security"></a>Zabezpečení rovině dat

V tomto oddílu se podíváme na povolení přístupu k objektům vlastních dat ve vašem účtu úložiště, jako je objektů BLOB, soubory, dotazů a tabulek, pomocí sdílené podpisy přístup a uložené zásady přístupu. Budeme se zabývat těmito oblastmi přidružení služby úroveň zabezpečení a přidružení účtu úroveň zabezpečení. Ukážeme si taky, jak omezit přístup k určité IP adresy (nebo rozsah IP adresy), jak omezit protokol sloužící k HTTPS a jak odvolat přístup podpis sdílené bez nutnosti čekání vypršení platnosti.

## <a name="encryption-in-transit"></a>Šifrování při přenosu šifrovaná

Tato část popisuje, jak zajistit data při přenosu nebo stmívání Azure úložiště. Podíváme doporučené použití HTTPS a šifrování používaný SMB 3.0 pro Azure sdílené složky. Jsme bude taky podívejte se na straně klienta šifrování, které umožňuje šifrování data před bude převedena do úložiště v klientské aplikaci a dešifrování data po přenosu z úložiště.

## <a name="encryption-at-rest"></a>Šifrování na ostatní

Bude budeme úložiště služby šifrování (SSE) a jak ji povolit účtu úložiště, výsledkem je vaše objektů BLOB blok objektů BLOB stránky a připojit automaticky šifrovány zápisu Azure úložiště objektů BLOB. Podíváme se také na tom, jak můžete použít šifrování disku Azure a prozkoumat základní rozdíly a případech šifrování disku a SSE versus šifrování na straně klienta. Podíváme bude stručně na FIPS dodržování předpisů pro vládní organizace počítače.

- [Azure Průvodce zabezpečením úložiště](../storage/storage-security-guide.md)

## <a name="cost-savings"></a>Úspora nákladů

- [Pole náklady úložiště](https://azure.microsoft.com/pricing/details/storage/)

- [Kalkulačka úložiště nákladů](https://azure.microsoft.com/pricing/calculator/?service=storage)

## <a name="storage-limits"></a>Omezení úložiště

- [Omezení úložiště služby](../azure-subscription-service-limits.md#storage-limits)
