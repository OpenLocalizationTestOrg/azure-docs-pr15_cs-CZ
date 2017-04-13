<properties
    pageTitle="Použití Azure úložiště pro SQL Server zálohování a obnovení | Microsoft Azure"
    description="Naučte se obecnějším údajům SQL serveru k základnímu úložišti Azure. Tento článek vysvětluje výhody zálohování databáze SQL Azure úložiště."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="MikeRayMSFT"
    manager="jhubbard"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="07/22/2016"
    ms.author="mikeray"/>

# <a name="use-azure-storage-for-sql-server-backup-and-restore"></a>Použití Azure úložiště pro SQL Server zálohování a obnovení

## <a name="overview"></a>Základní informace

Začnete s SQL Server 2012 SP1 CU2, můžete teď napsat zálohy systému SQL Server přímo do služba úložiště objektů Blob Azure. Pomocí této funkce můžete zálohování a obnovení objektů Blob Azure službě s místní databáze aplikace nebo databáze SQL serveru v Azure virtuálního počítače. Zálohování do cloudu nabízí výhody dostupnost neomezený replikovat geo mimo úložiště a snadnější Migrace údajů do a z cloudu. Zadejte zálohování a obnovení příkazy pomocí jazyce Transact-SQL nebo SMO.

SQL Server 2016 uvádí nové funkce; [záložní soubor snímek](http://msdn.microsoft.com/library/mt169363.aspx) slouží k provádění téměř okamžité zálohování a obnovení velmi snadné.

Toto téma vysvětluje, proč může rozhodnete sdělit nám záloh SQL Azure úložiště a pak popisuje součástí. Pro přístup k návody a dalších informací a začněte používat tuto službu s zálohování SQL serveru můžete použít zdroje uvedené na konci tohoto článku.

## <a name="benefits-of-using-the-azure-blob-service-for-sql-server-backups"></a>Výhody používání služby objektů Blob Azure záloh SQL serveru

Existuje několik problémy, které při zálohování SQL serveru. Tyto výzvy zahrnují Správa úložiště, rizika úložiště nepovede, přístup k mimo úložiště a konfigurace hardwaru. V mnoha tyto výzvy adresami pomocí služby úložiště objektů Blob Azure záloh SQL serveru. Zvažte následující výhody:

- **Usnadnění používání**: ukládání záloh v objektů BLOB Azure může být pohodlný flexibilní a snadný přístup k mimo možnost. Vytváření mimo úložiště záloh SQL serveru může být stejně snadná jako úprava vaší stávající skripty/úlohy pomocí následující syntaxe **Zálohování na adresu URL** . Mimo úložiště by měl být obvykle dostatečně úplně z umístění databáze výrobní nechcete, aby jeden havárie, které můžou ovlivnit mimo a výrobních umístění se databáze. Výběrem [objektů BLOB geo replikace vaší Azure](../storage/storage-redundancy.md), máte další úroveň ochrany v případě havárie, které by mohly ovlivnit celé oblasti.
- **Zálohování archiv**: služby úložišti objektů Blob Azure nabízí lepší alternativou často používaný páskou možnost Archivovat zálohy. Úložiště páskou může vyžadovat fyzické dopravy mimo zařízení a míry chránit médií. Ukládání záloh v úložišti objektů Blob Azure poskytuje rychlých, vysoce k dispozici, a spotřeby archivace možnost.
- **Hardwarová spravovaných**: neexistuje žádné režijních hardwaru Správa služby Azure. Azure služby Správa hardware a poskytovat geo replikace redundance a ochrana před selháním hardwaru.
- **Neomezený tarif úložiště**: povolením přímé zálohy objektů BLOB Azure, máte přístup k základnímu úložišti neomezený. Můžete taky zálohování disk Azure virtuální počítač má omezení na základě velikosti počítače. Existuje omezení počtu disků, které můžete připojit k Azure virtuálního počítače záloh. Toto omezení je 16 disků největší instance a méně pro menší instance.
- **Zálohování dostupnost**: záloh uložených v objektů BLOB Azure jsou k dispozici odkudkoli a kdykoli a můžete jednoduše k nim získat přístup k obnovení do místního SQL serveru nebo další SQL Server spuštěné v Azure virtuálního počítače, bez nutnosti databázi připojit nebo odpojit nebo stahování a připojování virtuální pevný disk.
- **Pole náklady**: platí pouze pro službu, který se používá. Může být efektivní jako jednu z možností mimo a zálohování archivu. V tématu [Azure ceny kalkulačky](http://go.microsoft.com/fwlink/?LinkId=277060 "Ceny kalkulačky")a [ceny Azure článek](http://go.microsoft.com/fwlink/?LinkId=277059 "ceny článku") Další informace.
- **Ukládání snímků**: když používáte SQL serveru 2016, databázové soubory uložené v objektů blob Azure [zálohy soubor snímku](http://msdn.microsoft.com/library/mt169363.aspx) můžete provádět téměř okamžité zálohování a obnovení velmi snadné.

Další informace najdete v tématu [SQL Server zálohování a obnovení se službou úložiště objektů Blob Azure](http://go.microsoft.com/fwlink/?LinkId=271617).

Následující dva oddíly Představte službu úložiště objektů Blob Azure, včetně požadované součásti SQL Server. Je důležité pochopit součásti a jejich interakce úspěšně pomocí zálohování a obnovení na služba úložiště objektů Blob Azure.

## <a name="azure-blob-storage-service-components"></a>Součásti služby úložiště objektů Blob Azure

Následující Azure součásti se používají při zálohování k úložišti objektů Blob Azure.

| Součásti               | Popis                          |
|---------------------|-------------------------------|
| **Úložiště účtu** | Účet úložiště je výchozí bod pro všechny úložiště služby. Pro přístup k úložišti objektů Blob Azure službě, nejprve vytvořte účet Azure úložiště. Další informace o službě úložiště objektů Blob Azure přečtěte si, [jak používat službu úložiště objektů Blob Azure](https://azure.microsoft.com/develop/net/how-to-guides/blob-storage/) |
| **Kontejner** | Kontejner poskytuje seskupení sadu objektů BLOB a mohou být uloženy neomezený počet objektů BLOB. Psát SQL Server záložní objektů Blob Azure služby, musíte mít alespoň Kořenový kontejner vytvořili. |
| **Objektů BLOB** | Soubor typu a velikosti. Objekty BLOB jsou s možností zadání pomocí následující formát adresy URL: **https://[storage account].blob.core.windows.net/[container]/[blob]**. Další informace o objektů BLOB stránky najdete v článku [Principy bloku a stránku objekty BLOB](http://msdn.microsoft.com/library/azure/ee691964.aspx) |

## <a name="sql-server-components"></a>Součásti SQL Server

Následující součásti SQL Server se používají při zálohování k úložišti objektů Blob Azure.

| Součásti               | Popis                          |
|---------------------|-------------------------------|
| **ADRESA URL** | Adresy URL určuje identifikátor URI (Uniform Resource) jedinečné záložního souboru. Adresa URL se používá k zadejte umístění a název záložního souboru SQL serveru. Adresa URL musí odkazovat skutečné blob, nikoli pouze kontejner. Pokud objektů blob neexistuje, je vytvořen. Pokud existující objektů blob není zadán, zálohování selže, pokud > není zadán možnost s formátu. Následuje příklad by zadaná v příkazu zálohování adresa URL: **http[s]://[storageaccount].blob.core.windows.net/[container]/[FILENAME.bak]**. Protokol HTTPS doporučujeme ale nejsou nutné. |
| **Přihlašovací údaje** | Informace, které je potřeba k připojení a ověřte ke službě úložiště objektů Blob Azure uloží jako pověření.  Aby SQL serveru k zápisu zálohy objektů Blob Azure nebo obnovit z něho je nutné vytvořit pověření systému SQL Server. Další informace najdete v tématu [SQL Server pověření](https://msdn.microsoft.com/library/ms189522.aspx). |

> [AZURE.NOTE] Pokud budete chtít zkopírovat a nahrajte záložního souboru ke službě úložiště objektů Blob Azure, musíte použít v typu objektů blob stránky jako možnost úložiště případě se chystáte používat tento soubor při obnově. OBNOVENÍ objektů blob typu bloku se nepovede zobrazí se chybová zpráva.

## <a name="next-steps"></a>Další kroky

1. Pokud už nemáte, vytvořte účet Azure. Pokud jsou vyhodnocení Azure, zvažte [bezplatnou zkušební verzi](https://azure.microsoft.com/free/).

1. Přejděte pomocí jedné z následujících kurzy, které vám pomohou při vytvoření účtu úložiště a provádění obnovení.

    - **SQL Server 2014**: [kurz: SQL Server 2014 zálohování a obnovení na Microsoft Azure služba úložiště objektů Blob](https://msdn.microsoft.com/library/jj720558\(v=sql.120\).aspx).
    - **SQL Server 2016**: [kurz: Služba úložiště objektů Blob Microsoft Azure pomocí databáze SQL serveru 2016](https://msdn.microsoft.com/library/dn466438.aspx)

1. Prohlédněte si přečtěte následující dokumentaci další počínaje [SQL Server zálohování a obnovení se službou úložiště objektů Blob Microsoft Azure](https://msdn.microsoft.com/library/jj919148.aspx).

Pokud máte nějaké problémy, prohlédněte si téma [Zálohování SQL serveru k URL osvědčených postupů a Poradce při potížích](https://msdn.microsoft.com/library/jj919149.aspx).

Další SQL Server zálohování a obnovení možnosti najdete v tématu [zálohování a obnovení pro systém SQL Server ve virtuálních počítačích Azure](../virtual-machines/virtual-machines-windows-sql-backup-recovery.md).
