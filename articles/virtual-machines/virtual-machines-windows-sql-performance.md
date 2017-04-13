<properties
    pageTitle="Výkon doporučené postupy pro systém SQL Server | Microsoft Azure"
    description="Poskytuje doporučené postupy pro optimalizaci výkonu v Microsoft Azure VMs SQL serveru."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/07/2016"
    ms.author="jroth" />

# <a name="performance-best-practices-for-sql-server-in-azure-virtual-machines"></a>Výkon doporučené postupy pro systém SQL Server ve virtuálních počítačích Azure

## <a name="overview"></a>Základní informace

Toto téma obsahuje doporučené postupy pro optimalizaci výkonu ve počítače virtuální Microsoft Azure SQL serveru. Při spuštění systému SQL Server ve virtuálních počítačích Azure, doporučujeme dál používat stejnou výkon databáze optimalizace možnosti, které se vztahují na serveru SQL Server v místním prostředí serveru. Výkon relační databáze ve veřejných cloudu ale závisí na spousta faktorů, jako je velikost virtuálního počítače a konfigurace disků data.

Při vytváření obrázky z SQL serveru, [zvažte zřizování VMs Azure portálu](virtual-machines-windows-portal-sql-server-provision.md). SQL Server VMs zřízení na portálu správce prostředků s provádět všechny tyto doporučené postupy, včetně konfigurace úložiště.

Tento článek se zaměřuje na získání *optimálního* výkonu pro systém SQL Server Azure VMs. Pokud váš pracovní zátěž méně náročné, nemusí nastavíte, že každé optimalizace vypsané dole. Zvažte výkon a pracovní zátěž vzorků při vyhodnocení těmito doporučeními.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="quick-check-list"></a>Stručný kontrolní seznam

Následující obrázek je stručný kontrolní seznam pro optimální výkon systému SQL Server na virtuálních počítačích Azure:

|Oblast|Optimalizace|
|---|---|
|[Velikost OM](#vm-size-guidance)|[DS3](virtual-machines-windows-sizes.md#standard-tier-ds-series) nebo vyšší pro SQL Enterprise edition.<br/><br/>[DS2](virtual-machines-windows-sizes.md#standard-tier-ds-series) nebo vyšší pro Web a SQL standardní vydání.|
|[Úložiště](#storage-guidance)|Použití [úložiště Premium](../storage/storage-premium-storage.md). Standardní úložiště doporučujeme použít pouze pro vývojáře nebo zkoušení.<br/><br/>Mějte na [úložiště klienta](../storage/storage-create-storage-account.md) a SQL Server OM stejné oblasti.<br/><br/>Zakázání Azure [geo nadbytečné úložiště](../storage/storage-redundancy.md) (geo replikace) na účtu úložiště.|
|[Disků](#disks-guidance)|Použijte aspoň 2 [P30 disků](../storage/storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage) (1 pro protokoly; 1 pro datové soubory a TempDB).<br/><br/>Nepoužívání operačního systému nebo dočasné disků úložiště databáze nebo protokolování.<br/><br/>Přečtěte si povolit ukládání do mezipaměti na discích hostingu datové soubory a TempDB.<br/><br/>Nedoporučujeme povolit ukládání do mezipaměti na discích hostingu soubor protokolu.<br/><br/>Důležité: Zastavení služby SQL Server při změně nastavení mezipaměti pro Azure OM disku.<br/><br/>Prokládat více disků Azure dat získat lepší výkon vstupu a výstupu.<br/><br/>Formát s popsána přidělení velikosti.|
|[VSTUPU A VÝSTUPU](#io-guidance)|Povolte kompresi stránky databáze.<br/><br/>Povolte rychlé soubor inicializace pro datové soubory.<br/><br/>Omezit nebo vypnout automatické zvětšování databázi.<br/><br/>Zakázání autoshrink databázi.<br/><br/>Přesuňte všechny databáze disků dat včetně databází systému.<br/><br/>Přesunutí SQL Server chyby protokolování a sledování adresářů souborů disků data.<br/><br/>Nastavení výchozí zálohování a databáze umístění souboru.<br/><br/>Povolte uzamčené stránek.<br/><br/>Použití opravy výkonu systému SQL Server.|
|[Specifické funkce](#feature-specific-guidance)|Obecnějším údajům přímo k úložišti objektů blob.|

Další informace o *tom, jak* a *Proč* provádět tyto optimalizace zkontrolujte podrobnosti a podle pokynů uvedených v následujících částech.

## <a name="vm-size-guidance"></a>Pokyny pro velikost OM

Výkonné citlivé aplikace se doporučuje používat tyto [formáty virtuálních počítačích](virtual-machines-windows-sizes.md):

- **SQL Server Enterprise Edition**: DS3 nebo vyšší

- **SQL Server Standard a edice Web**: DS2 nebo vyšší


## <a name="storage-guidance"></a>Pokyny pro ukládání

Pošta – řady (spolu s DSv2 řady a GS řad) VMs podpora [Premium úložiště](../storage/storage-premium-storage.md). Premium úložiště je vhodné pro všechny úlohami výroby.

> [AZURE.WARNING] Standardní úložiště má odlišnými čekacích dob a šířku pásma a doporučujeme použít pouze pro vývojáře nebo zkoušení úloh. Výrobní úloh používejte Premium úložiště.

Kromě toho doporučujeme, abyste vytvořit účet Azure úložiště v centru dat jako svého serveru SQL Server virtuálních počítačích zmenšit přenos zpoždění. Při vytváření účtu úložiště, zakážete geo replikace konzistentní zápisu různých disků zaručit. Zvažte místo toho konfigurace systému SQL Server havárie obnovení technologie mezi dvěma Azure datacentrech. Další informace najdete v tématu [dostupnost a obnovení pro systém SQL Server ve virtuálních počítačích Azure](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="disks-guidance"></a>Pokyny pro disků

Existují tři typy hlavní disk na Azure OM:

- **Operační systém disku**: při vytváření virtuálních počítačů Azure platformu bude připojovat alespoň jeden disk (označena jako jednotku **C** ) OM pro váš operační systém disk. Tento disk je virtuální pevný disk uložených jako objektů blob stránky v úložišti.
- **Dočasné disku**: Azure virtuálních počítačích obsahují jiný disk s názvem dočasné disku (textem je označená jako **D**: jednotky). Toto je disk na uzel, který se dá použít pro pracovní prostor.
- **Disků data**: můžete také připojíte další disk virtuálního počítače jako data a tyto budou uložené v úložišti jako objekty BLOB stránky.

Následující oddíly popisují doporučení pro používání různých discích.

### <a name="operating-system-disk"></a>Operační systém disku

Operační systém disk je virtuálního pevného disku, kterou můžete spustit připojit jako pracovního verze operačního systému a je označený jako jednotku **C:** .

Ukládání do mezipaměti zásad na disku operační systém výchozí hodnota je **Pro čtení i zápis**. Výkon citlivé aplikací doporučujeme použít disků dat místo disk operačního systému. Naleznete v části na dat discích dole.

### <a name="temporary-disk"></a>Dočasné disku

Dočasné úložiště disk označený jako **D**: řízení, není zachován k úložišti objektů blob Azure. Neukládejte uživatele databázové soubory nebo soubory protokolu transakcí uživatele na **D**: disk.

D řady Dv2 řady a VMs G řady dočasné disk na tyto VMs je založený na SSD. Pokud váš pracovní zátěž hodně využívá TempDB (například pro dočasné objekty nebo složité spojení), ukládání TempDB na disku **D** může vést k vyšší výkon TempDB a nižší latence TempDB.

Pro VMs, které podporují Premium úložiště (DS řady DSv2 řady a GS řad) doporučujeme uložit TempDB a/nebo rozšíření fondu vyrovnávací paměť na disku, který podporuje Premium úložiště s čtení povoleno ukládání do mezipaměti. Existuje jedinou výjimkou tohoto doporučení; Při použití TempDB náročné zapsat, můžete dosáhnout vyšší výkon ukládání TempDB na místní disk **D** , což je také SSD podle těchto velikostí počítače.

### <a name="data-disks"></a>Disků dat

- **Použití disků dat pro data a souborů protokolu**: minimálně pomocí 2 Premium úložiště [P30 disků](../storage/storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage) kde jeden disk obsahuje soubory protokolu a druhý datové soubory a TempDB.

- **Prokládání disku**: pro další výkon, můžete přidat další data disků a použít prokládání disku. Chcete-li zjistit počet disků dat, potřebujete analyzovat počet procesorů umožňující dat a protokolů discích. Tyto informace najdete v článku tabulek na procesorů za [OM velikost](virtual-machines-windows-sizes.md) a velikost disku v následujícím článku: [Použití Premium úložného prostoru pro discích](../storage/storage-premium-storage.md). Můžete použít následující těmito pokyny:

    - Windows 8 nebo Windows Server 2012 nebo novější použijte [Úložiště mezery](https://technet.microsoft.com/library/hh831739.aspx). Nastavte velikost prokládání a 64 KB OLTP úloh 256 KB skladovými pracovního vytížení dat chcete-li předejít vliv na výkon kvůli chybné oddíl zarovnání. Kromě toho, nastavte počet sloupců = počet discích. Konfigurace úložný prostor s více než 8 disků musíte pomocí prostředí PowerShell (ne uživatelské rozhraní správce serveru) explicitně nastavte počet sloupců tak, aby odpovídaly počet disků. Další informace o tom, jak nakonfigurovat [Úložiště mezery](https://technet.microsoft.com/library/hh831739.aspx), najdete v článku [Úložiště mezery rutin v prostředí Windows PowerShell](https://technet.microsoft.com/library/jj851254.aspx)

    - Windows 2008 R2 nebo starší můžete použít dynamické discích (s operačním systémem rozložené svazky) a velikost prokládání je vždy 64 KB. Všimněte si, že tato možnost se nedá použít k Windows 8 nebo Windows Server 2012. Informace najdete v tématu příkazu podpory na [Virtuální diskovou službu přechodem na rozhraní API správy úložiště systému Windows](https://msdn.microsoft.com/library/windows/desktop/hh848071.aspx).

    - Pokud váš pracovní zátěž protokolu náročné a nemusí vyhrazené procesorů, můžete nakonfigurovat jenom pro jednu fondu úložiště. V opačném vytvořte dvěma fondů úložiště jeden pro soubory protokolu a jiného fondu úložiště pro datové soubory a TempDB. Určete počet disků přidružené každý fondu úložiště podle očekávání načíst. Mějte na paměti, že OM různě povolit různý počet disků připojená data. Další informace najdete v tématu [velikosti virtuálních počítačích](virtual-machines-windows-sizes.md).

    - Pokud nepoužíváte Premium úložiště (odchylka nebo zkoušení scénáře), doporučujeme přidat maximální počet dat disků nepodporuje [OM velikost](virtual-machines-windows-sizes.md) a používat prokládání disku.

- **Ukládání do mezipaměti zásad**: disků dat pro úložiště Premium povolit čtení ukládání do mezipaměti na discích dat hostingu datové soubory a TempDB pouze. Pokud nepoužíváte Premium úložiště, neumožňují všechny ukládání do mezipaměti na všech dat discích. Pokyny týkající se konfigurace diskové mezipaměti, najdete v těchto tématech: [AzureOSDisk sady](https://msdn.microsoft.com/library/azure/jj152847) a [Sady AzureDataDisk](https://msdn.microsoft.com/library/azure/jj152851.aspx).

    >[AZURE.WARNING] Při změně nastavení mezipaměti Azure OM disků předešli omezí se možné poškození databáze zastavte službu SQL Server.

- **Velikost jednotky přidělení NTFS**: při formátování disku dat, se doporučuje používat velikost jednotky 64 KB rozdělení pro data a souborů protokolu taky TempDB.

- **Doporučené postupy správy disku**: Když odeberete disk dat nebo změnit jeho typ mezipaměti zastavit službu SQL Server během změny. Při ukládání nastavení se změní na disku OS, Azure zastaví OM, změní typ mezipaměti a restartování OM. Při změně nastavení mezipaměti dat disk, OM nezastavíte, ale odpojí od OM během změny a pak znovu připevněn datový disk.

    >[AZURE.WARNING] Zastavení služby SQL Server během tyto operace může dojít poškození databáze.

## <a name="io-guidance"></a>Pokyny pro vstupu a výstupu

- Nejlepších výsledků dosáhnete Premium úložiště jsou dosáhnout při paralelní aplikace a požadavky. Premium úložiště je určený pro scénáře hloubka fronty vstupu a výstupu větší než 1, takže uvidíte malý nebo žádný zlepšení výkonu jedním podprocesem sériové požadavků (i když jsou nároky úložiště). Například to můžou mít vliv na výsledky jedním podprocesem testů výkonu analytické nástroje, například SQLIO.

- Zvažte použití [Komprese stránky databáze](https://msdn.microsoft.com/library/cc280449.aspx) jako můžete zvýšit výkon náročné pracovního vytížení vstupu a výstupu. Komprese dat však může zvýšit spotřebu procesoru na databázovém serveru.

- Zvažte povolení rychlé soubor inicializace snížit čas, který je potřebný pro přidělení počáteční soubor. Umožní využít výhod rychlé soubor inicializace udělit účet služby SQL Server (MSSQLSERVER) s SE_MANAGE_VOLUME_NAME a přiřadit ji někomu zásady zabezpečení **Úkoly hlasitost údržbu** . Pokud používáte obrázek platformu systému SQL Server pro Azure, výchozího účtu služby (NT Service\MSSQLSERVER) není přidána do zásady zabezpečení **Úkoly hlasitost údržbu** . Jinými slovy není povolený inicializace rychlé soubor obrázku platformy SQL Server Azure. Po přidání účtu služby SQL Server do zásad zabezpečení **Provádět úlohy údržby svazku** , znovu spusťte službu SQL Server. Může to být otázky bezpečnosti pro použití této funkce. Další informace najdete v tématu [Inicializace souboru databáze](https://msdn.microsoft.com/library/ms175935.aspx).

- **Automatické zvětšování** se považuje za pouze řešení nepředvídaných událostí pro neočekávané růst. Správu dat a protokolu růst každodenní s automatické zvětšování. Při automatické zvětšování předem zvětšit přepínačem velikost souboru.

- Zkontrolujte, že **autoshrink** je vypnutá, chcete-li předejít zbytečnému přetížení, které mohou nepříznivě ovlivnit výkon.

- Přesuňte všechny databáze disků dat včetně databází systému. Další informace najdete v tématu [Přesunutí databáze systému](https://msdn.microsoft.com/library/ms345408.aspx).

- Přesunutí SQL Server chyby protokolování a sledování adresářů souborů disků data. Stačí ve Správci konfigurace serveru SQL tak, že kliknete pravým tlačítkem myši instance serveru SQL Server a vyberte možnost Vlastnosti. Na kartě **Parametry spouštění** můžete změnit nastavení chyby protokolování a sledování soubor. Na kartě **Upřesnit** zadán výpis adresář. Následující obrázek ukazuje, kde má hledat parametr spuštění protokolu chyb.

    ![Snímek obrazovky ErrorLog SQL](./media/virtual-machines-windows-sql-performance/sql_server_error_log_location.png)

- Nastavení výchozí zálohování a databáze umístění souboru. V tomto tématu použít doporučení a proveďte požadované změny v okně Vlastnosti serveru. Pokyny najdete v tématu [zobrazení nebo změna výchozí umístění pro Data a souborů protokolu (SQL Server Management Studio)](https://msdn.microsoft.com/library/dd206993.aspx). Následující obrázek ukazuje, kde se tyto změny.

    ![Soubory protokolu SQL dat a zálohování](./media/virtual-machines-windows-sql-performance/sql_server_default_data_log_backup_locations.png)

- Povolte uzamčené stránek zmenšit vstupu a výstupu a stránkování činnostem. Další informace najdete v tématu [Povolení Zamknout stránky v paměti možnost (Windows)](https://msdn.microsoft.com/library/ms190730.aspx).

- Pokud používáte systém SQL Server 2012, nainstalujte Service Pack 1 kumulativní aktualizace 10. Tato aktualizace obsahuje oprava snížení výkonu na vstupu a výstupu při spuštění vyberte Select dočasné tabulky v SQL Server 2012. Informace najdete v tomto [článku znalostní báze knowledge base](http://support.microsoft.com/kb/2958012).

- Zvažte kompresi soubory data při přenosu nebo rezervace z Azure.

## <a name="feature-specific-guidance"></a>Pokyny pro konkrétní funkce

U některých nasazení může dosáhnout další výkonu výhod použití více pokročilé techniky konfigurace. V následujícím seznamu jsou uvedeny některé funkce SQL serveru, které pomáhají dosáhnout lepší výkon:

- **Zálohování k základnímu úložišti Azure**: při provádění zálohování pro systém SQL Server spuštěné v Azure virtuálních počítačích, můžete použít [Zálohování serveru SQL na adresu URL](https://msdn.microsoft.com/library/dn435916.aspx). Tato funkce je k dispozici počínaje SQL Server 2012 SP1 CU2 a pro zálohování disků připojená data doporučené. Pokud jste zálohování a obnovení z Azure úložiště doporučeními v [SQL Server zálohování URL osvědčených postupů a řešení potíží a obnovení ze zálohy uložené v úložišti Azure](https://msdn.microsoft.com/library/jj919149.aspx). Můžete také automatizovat tyto zálohy pomocí [Automatického zálohování pro systém SQL Server ve virtuálních počítačích Azure](virtual-machines-windows-classic-sql-automated-backup.md).

    Před SQL Server 2012 můžete použít [Zálohování serveru SQL Azure nástroji](https://www.microsoft.com/download/details.aspx?id=40740). Tento nástroj pomůže zvýšit záložní výkon pomocí několika záložní pruh cílů.

- **SQL Server datových souborů v Azure**: Tato nová funkce [SQL Server datových souborů v Azure](https://msdn.microsoft.com/library/dn385720.aspx)je k dispozici počínaje SQL Server 2014. Systém SQL Server s datovými soubory v Azure ukazuje srovnatelná výkonu vlastnosti jako pomocí disků Azure data.

## <a name="next-steps"></a>Další kroky

Pokud vás zajímají průzkum SQL serveru a úložiště Premium další podrobně, najdete v článku [Použití Azure Premium úložiště se serverem SQL Server na virtuálních počítačích](virtual-machines-windows-classic-sql-server-premium-storage.md).

Doporučené postupy pro zabezpečení najdete v článku [Otázky bezpečnosti pro systém SQL Server ve virtuálních počítačích Azure](virtual-machines-windows-sql-security.md).

Naleznete v dalších tématech SQL Server virtuálního počítače na [Serveru SQL Server na virtuálních počítačích přehled Azure](virtual-machines-windows-sql-server-iaas-overview.md).
